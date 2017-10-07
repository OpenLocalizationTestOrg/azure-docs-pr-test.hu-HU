---
title: "aaaHow tooload egyenleg Windows virtuális gépek Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure betölteni a terheléselosztó toocreate egy magas rendelkezésre állású és biztonságos alkalmazás három Windows virtuális gépek között"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: bfbd6a24e90a33e60d7d4f7b0c471af5d4e71f45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooload-balance-windows-virtual-machines-in-azure-toocreate-a-highly-available-application"></a>Hogyan tooload egyenleg Windows virtuális gépek Azure toocreate a magas rendelkezésre állású alkalmazások
Terheléselosztás biztosít a rendelkezésre állási magasabb szintű bejövő kérelmek elosztásával el több virtuális gépre. Ebben az oktatóanyagban elsajátíthatja hello különböző összetevőkről hello Azure terheléselosztó ossza el a forgalmat, és magas rendelkezésre állás biztosításához. Az alábbiak végrehajtásának módját ismerheti meg:

> [!div class="checklist"]
> * Hozzon létre egy Azure terheléselosztó
> * A health terheléselosztói mintavétel létrehozása
> * Terheléselosztó forgalomra vonatkozó szabályok létrehozása
> * Hello egyéni parancsprogramok futtatására szolgáló bővítmény toocreate egy egyszerű IIS webhelyet használja
> * Virtuális gépek létrehozása és csatolása tooa terheléselosztó
> * A művelet egy terhelés-kiegyenlítő megtekintése
> * Adja hozzá, és távolítsa el a virtuális gépek a terheléselosztó

Ez az oktatóanyag hello Azure PowerShell 3,6 vagy újabb verziója szükséges. Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója. Ha tooupgrade van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).


## <a name="azure-load-balancer-overview"></a>Az Azure load balancer áttekintése
Egy Azure terheléselosztó a réteg-4 (TCP, UDP) terheléselosztóhoz, amely a magas rendelkezésre állást biztosít azáltal, hogy a bejövő forgalom kifogástalan állapotú virtuális gépek között. Terheléselosztói állapotfigyelő mintavétel az egyes virtuális gépek egy adott portot figyeli, és csak osztja el a forgalmat tooan működési virtuális gép.

Egy előtér-IP-konfigurációja, amely tartalmaz egy vagy több nyilvános IP-címeket adhat meg. Az előtér-IP-konfiguráció lehetővé teszi, hogy a load balancer és az alkalmazások toobe elérhető hello interneten keresztül. 

Virtuális gépek csatlakozni a virtuális hálózati kártya (NIC) használatával tooa terheléselosztóhoz. toodistribute forgalom toohello virtuális gépeket, egy háttér címkészletet hello IP-címek hello virtuális (NIC) csatlakoztatott toohello terheléselosztó tartalmazza.

forgalom toocontrol hello áramló, megadhatja a terheléselosztási szabály az adott portok és protokollok, amelyek kapcsolódnak tooyour virtuális gépek.


## <a name="create-azure-load-balancer"></a>Az Azure terheléselosztó létrehozása
Ez a szakasz részletesen, hogyan hozhat létre, és minden egyes összetevő hello terheléselosztó konfigurálása. A terheléselosztó létrehozása előtt hozzon létre egy erőforráscsoportot, a [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupLoadBalancer* a hello *EastUS* helye:

```powershell
New-AzureRmResourceGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS
```

### <a name="create-a-public-ip-address"></a>Hozzon létre egy nyilvános IP-címet
tooaccess rendszeren hello Internet, egy nyilvános IP-címet kell hello terheléselosztóhoz. Hozzon létre egy nyilvános IP-cím [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress). hello alábbi példa létrehoz egy nyilvános IP-cím nevű *myPublicIP* a hello *myResourceGroupLoadBalancer* erőforráscsoport:

```powershell
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIP
```

### <a name="create-a-load-balancer"></a>Terheléselosztó létrehozása
Hozzon létre egy előtérbeli IP-cím [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig). hello alábbi példa létrehoz egy előtérbeli IP-cím nevű *myFrontEndPool*: 

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name myFrontEndPool `
  -PublicIpAddress $publicIP
```

Hozzon létre egy háttér-címkészlet, amely [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig). hello alábbi példa létrehoz egy háttér címkészletet nevű *myBackEndPool*:

```powershell
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool
```

Ezután hozzon létre hello rendelkező terheléselosztó [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer). hello alábbi példa létrehoz egy terhelés-kiegyenlítő nevű *myLoadBalancer* hello segítségével *myPublicIP* cím:

```powershell
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myLoadBalancer `
  -Location EastUS `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool
```

### <a name="create-a-health-probe"></a>Hozzon létre egy állapotmintáihoz
tooallow hello terheléselosztó toomonitor hello állapotához az alkalmazás, használhat egy állapotmintáihoz. hello állapotmintáihoz dinamikusan eltávolítása vagy virtuális gépek hello terhelés terheléselosztó Elforgatás a válasz toohealth ellenőrzések alapján. Alapértelmezés szerint a virtuális gépek terheléselosztó terheléselosztási hello 15 másodperces időközönként két egymást követő hibák után törlődik. Létrehozhat egy állapotmintáihoz protokoll vagy egy meghatározott állapottal ellenőrzése lapon, az alkalmazás alapján. 

a következő példa hello egy TCP-Hálózatfigyelővel hoz létre. Egyéni HTTP mintavételt további részletes állapotának ellenőrzésére is létrehozhat. Ha egy egyéni HTTP-vizsgálatot, létre kell hoznia hello állapotának ellenőrzése lapon, például a *healthcheck.aspx*. hello mintavételi kell visszaadnia egy **HTTP 200 OK** hello terhelés terheléselosztó tookeep hello állomás Elforgatás válasz.

a TCP állapotmintáihoz toocreate, használhat [Add-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig). hello alábbi példa létrehoz egy nevű állapotmintáihoz *myHealthProbe* , amely figyeli az egyes virtuális gépek:

```powershell
Add-AzureRmLoadBalancerProbeConfig `
  -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2
```

Hello rendelkező terheléselosztó frissítése [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

### <a name="create-a-load-balancer-rule"></a>Hozzon létre olyan terheléselosztó szabályhoz
Olyan terheléselosztó szabályhoz használt toodefine forgalom Mitől elosztott toohello virtuális gépeket. Hello előtér-IP-konfiguráció hello bejövő forgalom és hello háttér-IP-készlet tooreceive hello forgalom, valamint hello szükséges forrás- és a célport adhat meg. a toomake meg arról, hogy csak a megfelelő virtuális gépek forgalom fogadására, is definiálhat hello állapotfigyelő mintavételi toouse.

Hozzon létre olyan terheléselosztó szabályhoz rendelkező [Add-AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig). hello alábbi példa létrehoz egy terheléselosztási szabály nevű *myLoadBalancerRule* és porton forgalom egyenlege *80*:

```powershell
$probe = Get-AzureRmLoadBalancerProbeConfig -LoadBalancer $lb -Name myHealthProbe

Add-AzureRmLoadBalancerRuleConfig `
  -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80 `
  -Probe $probe
```

Hello rendelkező terheléselosztó frissítése [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer):

```powershell
Set-AzureRmLoadBalancer -LoadBalancer $lb
```


## <a name="configure-virtual-network"></a>Virtuális hálózat konfigurálása
Mielőtt központilag az egyes virtuális gépek, és tesztelheti a terheléselosztó, hozzon létre virtuális hálózati erőforrások támogató hello. Virtuális hálózatok kapcsolatos további információkért lásd: hello [Azure virtuális hálózatok kezelése](tutorial-virtual-network.md) oktatóanyag.

### <a name="create-network-resources"></a>Hálózati erőforrások létrehozása
A virtuális hálózat létrehozása [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). hello alábbi példa létrehoz egy virtuális hálózatot nevű *myVnet* rendelkező *mySubnet*:

```powershell
# Create subnet config
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -AddressPrefix 192.168.1.0/24

# Create hello virtual network
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -Name myVnet `
  -AddressPrefix 192.168.0.0/16 `
  -Subnet $subnetConfig
```

A hálózati biztonsági csoport szabály létrehozása [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig), majd hozzon létre egy hálózati biztonsági csoport [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup). Hello hálózati biztonsági csoport toohello alhálózat hozzáadása [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) és frissítse a virtuális hálózat hello [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork). 

hello alábbi példa létrehoz egy hálózati biztonsági szabály nevű *myNetworkSecurityGroup* és alkalmazza azt túl*mySubnet*:

```powershell
# Create security rule config
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

# Create hello network security group
$nsg = New-AzureRmNetworkSecurityGroup `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Location EastUS `
  -Name myNetworkSecurityGroup `
  -SecurityRules $nsgRule

# Apply hello network security group tooa subnet
Set-AzureRmVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name mySubnet `
  -NetworkSecurityGroup $nsg `
  -AddressPrefix 192.168.1.0/24

# Update hello virtual network
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

Virtuális hálózati adapter jönnek létre [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface). hello alábbi példakód létrehozza három virtuális hálózati adapter. (Az egyes virtuális gépek virtuális hálózati adapter egy hoz létre a az alkalmazást a következő hello lépések). További virtuális hálózati adapterek és virtuális gépek létrehozása tetszőleges időpontban, és azok hozzáadása toohello terheléselosztó:

```powershell
for ($i=1; $i -le 3; $i++)
{
   New-AzureRmNetworkInterface `
     -ResourceGroupName myResourceGroupLoadBalancer `
     -Name myNic$i `
     -Location EastUS `
     -Subnet $vnet.Subnets[0] `
     -LoadBalancerBackendAddressPool $lb.BackendAddressPools[0]
}
```

## <a name="create-virtual-machines"></a>Virtuális gépek létrehozása
tooimprove hello magas rendelkezésre állás az alkalmazás a virtuális gépeket helyez egy rendelkezésre állási csoportot.

Állítsa be a rendelkezésre állási létrehozása [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset). hello alábbi példakód létrehozza rendelkezésre állási készlet elnevezett *myAvailabilitySet*:

```powershell
$availabilitySet = New-AzureRmAvailabilitySet `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myAvailabilitySet `
  -Location EastUS `
  -Managed `
  -PlatformFaultDomainCount 3 `
  -PlatformUpdateDomainCount 2
```

Állítsa a rendszergazda felhasználónevét és jelszavát hello rendelkező virtuális gépek [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Most a virtuális gépek hello hozhat létre [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). a következő példa hello három virtuális gépeket hoz létre:

```powershell
for ($i=1; $i -le 3; $i++)
{
  $vm = New-AzureRmVMConfig `
    -VMName myVM$i `
    -VMSize Standard_D1 `
    -AvailabilitySetId $availabilitySet.Id
  $vm = Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM$i `
    -Credential $cred `
    -ProvisionVMAgent `
    -EnableAutoUpdate
  $vm = Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
  $vm = Set-AzureRmVMOSDisk `
    -VM $vm `
    -Name myOsDisk$i `
    -DiskSizeInGB 128 `
    -CreateOption FromImage `
    -Caching ReadWrite
  $nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myNic$i
  $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
  New-AzureRmVM `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Location EastUS `
    -VM $vm
}
```

Néhány perc toocreate vesz igénybe, és minden három virtuális gép konfigurálása.

### <a name="install-iis-with-custom-script-extension"></a>Az egyéni parancsprogramok futtatására szolgáló bővítmény IIS telepítése
Az oktatóanyag előző [hogyan toocustomize egy Windows rendszerű virtuális gép](tutorial-automate-vm-deployment.md), megtudta, hogyan a tooautomate virtuális gép testreszabása az egyéni parancsfájl kiterjesztése a Windows hello. Használhatja a hello azonos tooinstall közelítse meg, és konfigurálja az IIS szolgáltatást a virtuális gépek.

Használjon [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooinstall hello egyéni parancsprogramok futtatására szolgáló bővítmény. bővítmény futtatása hello `powershell Add-WindowsFeature Web-Server` tooinstall hello IIS webkiszolgálót, majd a frissítések hello *Default.htm* lap tooshow hello hello virtuális gép állomásnevét:

```powershell
for ($i=1; $i -le 3; $i++)
{
   Set-AzureRmVMExtension `
     -ResourceGroupName myResourceGroupLoadBalancer `
     -ExtensionName IIS `
     -VMName myVM$i `
     -Publisher Microsoft.Compute `
     -ExtensionType CustomScriptExtension `
     -TypeHandlerVersion 1.4 `
     -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
     -Location EastUS
}
```

## <a name="test-load-balancer"></a>Teszt terheléselosztó
Hello nyilvános IP-cím a terheléselosztó az beszerzése [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). hello alábbi példa beszerzi hello IP-címet *myPublicIP* korábban létrehozott:

```powershell
Get-AzureRmPublicIPAddress `
  -ResourceGroupName myResourceGroupLoadBalancer `
  -Name myPublicIP | select IpAddress
```

Majd tooa webböngészőben hello nyilvános IP-címet adhat meg. hello webhely jelenik meg, beleértve a virtuális gép hello hello állomásnevét adott hello terheléselosztó forgalom tooas hello a következő példa az elosztott:

![Futó IIS-webhely](./media/tutorial-load-balancer/running-iis-website.png)

toosee hello terheléselosztó forgalom szét az alkalmazást futtató összes három virtuális gépet, a kényszerített-frissítési a webböngésző is.


## <a name="add-and-remove-vms"></a>Hozzá és távolíthat el a virtuális gépek
Szükség lehet az alkalmazás, például az operációs rendszer frissítéseinek telepítése futó virtuális gépeket hello tooperform karbantartása. toodeal megnövekedett forgalom tooyour alkalmazással, szükség lehet tooadd további virtuális gépeket. Ez a szakasz bemutatja, hogyan tooremove, vagy adja hozzá a virtuális gépek hello terheléselosztóról.

### <a name="remove-a-vm-from-hello-load-balancer"></a>Távolítsa el a virtuális gépek hello terheléselosztó
Hálózati kártya hello az beszerzése [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface), majd a készlet hello *konfigurációja terheléselosztói Háttércímkészletet* tulajdonsága túl hello a virtuális hálózati adapter*$null*. Végül frissítse a virtuális hálózati hello:

```powershell
$nic = Get-AzureRmNetworkInterface `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myNic2
$nic.Ipconfigurations[0].LoadBalancerBackendAddressPools=$null
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

toosee hello terheléselosztó forgalom szét a másik két hello az alkalmazást futtató virtuális gépek akkor is kényszerített frissítési a webböngésző. Karbantartási is képes lemezvizsgálatok elvégzésére, hogy a virtuális gép, például az operációs rendszer frissítéseinek telepítése vagy a virtuális gép újraindítása végrehajtása hello.

### <a name="add-a-vm-toohello-load-balancer"></a>Virtuális gép toohello terheléselosztó felvétele
Után VM karbantartásának végrehajtása, vagy ha tooexpand kapacitás van szüksége, állítsa be a hello *konfigurációja terheléselosztói Háttércímkészletet* hello virtuális hálózati adapter toohello tulajdonságának *BackendAddressPool* a [ Get-AzureRMLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer):

Hello terheléselosztó beolvasása:

```powershell
$lb = Get-AzureRMLoadBalancer `
    -ResourceGroupName myResourceGroupLoadBalancer `
    -Name myLoadBalancer 
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$lb.BackendAddressPools[0]
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban egy terhelés-kiegyenlítő létrehozott, és csatolt virtuális gépek tooit. Megismerte, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Hozzon létre egy Azure terheléselosztó
> * A health terheléselosztói mintavétel létrehozása
> * Terheléselosztó forgalomra vonatkozó szabályok létrehozása
> * Hello egyéni parancsprogramok futtatására szolgáló bővítmény toocreate egy egyszerű IIS webhelyet használja
> * Virtuális gépek létrehozása és csatolása tooa terheléselosztó
> * A művelet egy terhelés-kiegyenlítő megtekintése
> * Adja hozzá, és távolítsa el a virtuális gépek a terheléselosztó

Hogyan előzetes toohello következő útmutató toolearn toomanage VM-hálózat.

> [!div class="nextstepaction"]
> [Virtuális gépek és virtuális hálózatok kezelése](./tutorial-virtual-network.md)
