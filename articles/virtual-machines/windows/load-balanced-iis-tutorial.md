---
title: "Az oktatóanyag - Build egy magas rendelkezésre állású alkalmazások az Azure virtuális gépeken futó |} Microsoft Docs"
description: "Útmutató a magas rendelkezésre állású és biztonságos-alkalmazás létrehozásának három Windows virtuális gépek, az Azure-ban terheléselosztót között"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/30/2017
ms.author: davidmu
ms.openlocfilehash: 4b8690a11ec0e711782a112622e1193c24292289
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="build-a-load-balanced-highly-available-application-on-windows-virtual-machines-in-azure"></a>Egy elosztott terhelésű, a Windows Azure virtuális gépek magas rendelkezésre állású alkalmazás létrehozása

Ebben az oktatóanyagban létrehozhat egy magas rendelkezésre állású alkalmazás, amely a is lehetséges legyen karbantartási események. Az alkalmazás olyan terheléselosztóhoz, egy rendelkezésre állási csoportot és három Windows virtuális gépek (VM) használ. Ez az oktatóanyag telepíti az IIS szolgáltatást, abban az esetben, ha ez az oktatóanyag segítségével telepítheti egy másik alkalmazás-keretrendszer használatával a magas rendelkezésre állást azonos összetevők és irányelveket. 

## <a name="step-1---azure-prerequisites"></a>1. lépés – Azure-Előfeltételek

Az oktatóanyag elvégzéséhez, győződjön meg arról, hogy telepítette-e a legújabb [Azure PowerShell](/powershell/azure/overview) modul.

Első lépésként jelentkezzen be az Azure-előfizetéshez Login-AzureRmAccount paranccsal, és kövesse a képernyőn megjelenő utasításokat.

```powershell
Login-AzureRmAccount
```

Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat. Bármely más Azure-erőforrások létrehozása előtt hozzon létre egy erőforráscsoportot a kell [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Az alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a a `westeurope` régió: 

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location westeurope
```

## <a name="step-2---create-availability-set"></a>2. lépés - a rendelkezésre állási csoport létrehozása

Virtuális gépek közötti logikai tartalék hozhatók létre, és a tartományok frissítése. Minden logikai tartomány jelöli az alapul szolgáló Azure adatközpontjában hardver egy része. Két vagy több virtuális gép létrehozásakor a számítási és tárolási erőforrásokat ezekből a tartományokból különböző pontjain. Ehhez a terjesztéshez fenntartja az alkalmazás rendelkezésre állásának, ezért ha egy hardverösszetevő karbantartási. Rendelkezésre állási készletek lehetővé teszik, hogy a logikai hiba és a frissítési tartományok definiálása.

Állítsa be a rendelkezésre állási létrehozása [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset). Az alábbi példakód létrehozza a rendelkezésre állási készlet elnevezett `myAvailabilitySet`:

```powershell
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName myResourceGroup `
  -Name myAvailabilitySet `
  -Location westeurope `
  -Managed `
  -PlatformFaultDomainCount 3 `
  -PlatformUpdateDomainCount 2
```

## <a name="step-3---create-load-balancer"></a>3. lépés - betöltési létrehozása terheléselosztó

Egy Azure terheléselosztó a megadott virtuális gépeket használ a load balancer szabályok készletét forgalom elosztása. Egy állapotmintáihoz az egyes virtuális gépek egy adott portot figyeli, és csak osztja el a forgalmat egy operatív virtuális gépre.

### <a name="create-public-ip-address"></a>Nyilvános IP-cím létrehozása

Szeretne használni az alkalmazást az interneten, a terheléselosztó a nyilvános IP-címet rendel. Hozzon létre egy nyilvános IP-cím [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress). Az alábbi példa létrehoz egy nyilvános IP-cím nevű `myPublicIP`:

```powershell
$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-load-balancer"></a>Terheléselosztó létrehozása

Hozzon létre egy előtérbeli IP-cím [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig). Az alábbi példa létrehoz egy előtérbeli IP-cím nevű `myFrontEndPool`: 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name myFrontEndPool -PublicIpAddress $pip
```

Hozzon létre egy háttér-címkészlet, amely [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig). Az alábbi példa létrehoz egy háttér címkészletet nevű `myBackEndPool`:

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

Hozza létre a terheléselosztót, [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer). Az alábbi példa létrehoz egy terhelés-kiegyenlítő nevű `myLoadBalancer` használatával a `myPublicIP` cím:

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroup `
  -Name myLoadBalancer `
  -Location westeurope `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-health-probe"></a>Állapotfigyelő mintavétel létrehozása

Ahhoz, hogy a terheléselosztó a figyelheti az alkalmazás állapotát, használja a állapotmintáihoz. A állapotmintáihoz dinamikusan eltávolítása vagy virtuális gépek állapotát ellenőrzi a válasz alapján terhelés terheléselosztó elforgatási. Alapértelmezés szerint a virtuális gép törlődik a terheléselosztó terheléselosztási 15 másodperces időközönként két egymást követő hibák után.

Hozzon létre egy állapotmintáihoz rendelkező [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig). Az alábbi példa létrehoz egy állapotmintáihoz nevű `myHealthProbe` , amely figyeli az egyes virtuális gépek:

```powershell
Add-AzureRmLoadBalancerProbeConfig -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

### <a name="create-load-balancer-rule"></a>Terheléselosztási szabály létrehozása

Olyan terheléselosztó szabályhoz hogyan adatforgalom elosztása a virtuális gépek azonosítására szolgál.

Hozzon létre olyan terheléselosztó szabályhoz rendelkező [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig). Az alábbi példa létrehoz egy terheléselosztási szabály nevű `myLoadBalancerRule` és porton forgalom egyenlege `80`:

```powershell
Add-AzureRmLoadBalancerRuleConfig -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80
```

Frissítse a terheléselosztót, [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="step-4---configure-networking"></a>4. lépés - a hálózatkezelés konfigurálását

Minden virtuális gép rendelkezik legalább egy virtuális hálózati adapterek (NIC) egy virtuális hálózathoz csatlakozó. Ez a virtuális hálózat védett meghatározott hozzáférési szabályok alapján forgalom szűrésére.

### <a name="create-virtual-network"></a>Virtuális hálózat létrehozása

Először is konfiguráljon egy alhálózat [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Az alábbi példakód létrehozza nevű alhálózat `mySubnet`:

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24
```

Adja meg a hálózati kapcsolat a virtuális gépekhez, hozzon létre egy virtuális hálózat [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). Az alábbi példa létrehoz egy virtuális hálózatot nevű `myVnet` rendelkező `mySubnet`:

```powershell
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

### <a name="configure-network-security"></a>Konfigurálja a hálózati biztonság

Egy Azure [hálózati biztonsági csoport](../../virtual-network/virtual-networks-nsg.md) (NSG) szabályozza a bejövő és kimenő forgalmat egy vagy több virtuális géphez. Hálózati biztonsági csoportszabályok engedélyezheti vagy letilthatja a hálózati forgalmat egy adott portot vagy porttartományt. Ezek a szabályok is hozzáadhat egy forráscímelőtag, hogy csak egy előre meghatározott forrás származó forgalmat a virtuális gépek kommunikálhatnak.

Ahhoz, hogy webes forgalomban, az alkalmazás eléréséhez, hozzon létre egy hálózati biztonsági csoport szabály [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig). Az alábbi példa létrehoz egy hálózati biztonsági szabály nevű `myNetworkSecurityGroupRule`:

```powershell
$nsgRule = New-AzureRmNetworkSecurityRuleConfig `
  -Name myNetworkSecurityGroupRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 1001 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access Allow
```

Hozzon létre egy hálózati biztonsági csoport [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup). Az alábbi példa létrehoz egy NSG nevű `myNetworkSecurityGroup`:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location westeurope `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule
```

A hálózati biztonsági csoport hozzáadása a alhálózat [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig):

```powershell
Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24
```

A virtuális hálózat frissítése [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork):

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

### <a name="create-virtual-network-interface-cards"></a>Virtuális hálózati adapterek létrehozása

A virtuális hálózati adapter erőforrás helyett a tényleges virtuális gép egy terheléselosztó függvény betölteni. A virtuális hálózati adapter csatlakozik a terheléselosztóhoz, és ezután csatolni egy virtuális Gépet.

Hozzon létre egy virtuális hálózati adapter a [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface). Az alábbi példakód létrehozza a három virtuális hálózati adapter. (Az egyes virtuális gépek virtuális hálózati adapter egy hoz létre az alkalmazáshoz az alábbi lépéseket a):


```powershell
for ($i=1; $i -le 3; $i++)
{
   New-AzureRmNetworkInterface -ResourceGroupName myResourceGroup `
     -Name myNic$i `
     -Location westeurope `
     -Subnet $vnet.Subnets[0] `
     -LoadBalancerBackendAddressPool $lb.BackendAddressPools[0]
}

```

## <a name="step-5---create-virtual-machines"></a>5. lépés - a virtuális gépek létrehozása

Az alapul szolgáló összetevőit helyen most létrehozhat magas rendelkezésre állású virtuális gépek, az alkalmazás futtatásához. 

A felhasználónév és jelszó szükséges a virtuális gépen a rendszergazdai fiók beszerzése [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

A virtuális gépek létrehozása [új AzureRmVMConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/new-azurermvmconfig), [Set-AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmoperatingsystem), [Set-AzureRmVMSourceImage](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmsourceimage), [Set-AzureRmVMOSDisk](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/set-azurermvmosdisk), [hozzáadása AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.8.0/add-azurermvmnetworkinterface), és [új AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Az alábbi példakód létrehozza a három virtuális gépek:

```powershell
for ($i=1; $i -le 3; $i++)
{
   $vm = New-AzureRmVMConfig -VMName myVM$i -VMSize Standard_D1 -AvailabilitySetId $availabilitySet.Id
   $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName myVM$i -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
   $vm = Set-AzureRmVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2016-Datacenter -Version latest
   $vm = Set-AzureRmVMOSDisk -VM $vm -Name myOsDisk$i -StorageAccountType StandardLRS -DiskSizeInGB 128 -CreateOption FromImage -Caching ReadWrite
   $nic = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroup -Name myNic$i
   $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
   New-AzureRmVM -ResourceGroupName myResourceGroup -Location westeurope -VM $vm
}

```

Hozza létre és konfigurálja az összes három virtuális gép több percet vesz igénybe. A load balancer állapotmintáihoz automatikusan észleli, ha az alkalmazás futtatása az egyes virtuális gépek. A webalkalmazás működik, ha a terheléselosztó szabályhoz forgalom terjeszteni elindul.

### <a name="install-the-app"></a>Az alkalmazás telepítéséhez 

Az Azure virtuálisgép-bővítmények segítségével automatizálhatja a virtuális gép konfigurációs feladatokhoz, mint az alkalmazások telepítése és konfigurálása az operációs rendszer. A [egyéni parancsprogramok futtatására szolgáló bővítmény Windows](./../virtual-machines-windows-extensions-customscript.md) a virtuális gépen a PowerShell parancsfájl futtatásához használt. A parancsfájl az Azure storage, bármely elérhető HTTP-végpont tárolja, vagy egyéni parancsfájl bővítménykonfiguráció ágyazva. Ha az egyéni parancsprogramok futtatására szolgáló bővítmény, az Azure Virtuálisgép-ügynök kezeli a parancsfájl végrehajtása.

Használjon [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) az egyéni parancsprogramok futtatására szolgáló bővítmény telepítése. A bővítmény fut `powershell Add-WindowsFeature Web-Server` az IIS-webkiszolgáló telepítéséhez:

```powershell
for ($i=1; $i -le 3; $i++)
{
   Set-AzureRmVMExtension -ResourceGroupName myResourceGroup `
     -ExtensionName IIS `
     -VMName myVM$i `
     -Publisher Microsoft.Compute `
     -ExtensionType CustomScriptExtension `
     -TypeHandlerVersion 1.4 `
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server"}' `
     -Location westeurope
}
```

### <a name="test-your-app"></a>Az alkalmazás tesztelése

Szerezze be a terheléselosztó a nyilvános IP-címe [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Az alábbi példa beolvassa az IP-címek `myPublicIP` korábban létrehozott:

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroup -Name myPublicIP | select IpAddress
```

Adja meg a nyilvános IP-címet egy webböngészőben. Az NSG szabályhoz helyen az alapértelmezett IIS-webhely jelenik meg. 

![Alapértelmezett IIS-webhely](media/load-balanced-iis-tutorial/iis.png)

## <a name="step-6--management-tasks"></a>6. lépés – felügyeleti feladatok

Szükség lehet a az alkalmazás, például az operációs rendszer frissítéseinek telepítése futó virtuális gépeket karbantartásához. Az alkalmazás megnövekedett forgalom kezelésére, szükség lehet további virtuális gépek hozzáadása. Ez a szakasz bemutatja, hogyan távolítsa el, vagy adja hozzá a virtuális gépek a terheléselosztóról. 

### <a name="remove-a-vm-from-the-load-balancer"></a>A virtuális gép eltávolítása a terheléselosztó

A virtuális gép eltávolítása a háttér címkészletet alaphelyzetbe állítása a hálózati kártya konfigurációja terheléselosztói Háttércímkészletet tulajdonságát.

A hálózati kártya első [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface):

```powershell
$nic = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroup -Name myNic2
``` 

A hálózati kártya konfigurációja terheléselosztói Háttércímkészletet tulajdonságának beállítása a $null:

```powershell
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
```

A hálózati kártyát. frissítés:

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

### <a name="add-a-vm-to-the-load-balancer"></a>A virtuális gépek hozzáadása a terheléselosztó

Után VM karbantartásának végrehajtása, vagy bontsa ki a kapacitást kell, ha a hálózati Adaptert a virtuális gépek felvétele a háttér címkészletet, a terheléselosztó.

A load balancer beolvasása:

```powershell
$lb = Get-AzureRMLoadBalancer -ResourceGroupName myResourceGroup -Name myLoadBalancer 
```

A hálózati kártyát a háttér címkészletet, a terheléselosztó hozzáadása:

```powershell
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
```

A hálózati kártyát. frissítés:

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a>Következő lépések

Minták – [Azure virtuális gép PowerShell-mintaparancsfájlok](./../virtual-machines-windows-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
