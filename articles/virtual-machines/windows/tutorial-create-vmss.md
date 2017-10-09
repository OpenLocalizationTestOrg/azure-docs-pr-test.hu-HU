---
title: "a virtuális gép méretezési beállítása a Windows Azure-ban aaaCreate |} Microsoft Docs"
description: "A Windows-alapú virtuális gépek használata a virtuálisgép-méretezési csoport egy magas rendelkezésre állású alkalmazás létrehozását és telepítését"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: 
ms.topic: article
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: a3914571005c28a492c069d880992630851afae7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-windows"></a>Hozzon létre egy virtuálisgép-méretezési csoportban, és a magas rendelkezésre állású alkalmazás a Windows központi telepítése
A virtuálisgép-méretezési csoport toodeploy lehetővé teszi, és az azonos, az automatikus skálázást virtuális gépek kezelésére. Hello hello méretezési csoportban lévő virtuális gépek száma manuálisan méretezhető, vagy a szabályok tooautoscale CPU kihasználtsága, a memória igény szerint vagy a hálózati forgalom alapján határozza meg. Ebben az oktatóanyagban telepít egy virtuálisgép-méretezési beállítása az Azure-ban. Az alábbiak végrehajtásának módját ismerheti meg:

> [!div class="checklist"]
> * Hello egyéni parancsprogramok futtatására szolgáló bővítmény toodefine egy IIS-webhely tooscale használata
> * Hozzon létre egy terheléselosztót a méretezési csoport
> * Hozzon létre egy virtuálisgép-méretezési csoport
> * Növeli vagy csökkenti a méretezési csoportban lévő példányok hello száma
> * Automatikus skálázási szabályok létrehozása

Ez az oktatóanyag hello Azure PowerShell 3,6 vagy újabb verziója szükséges. Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója. Ha tooupgrade van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).


## <a name="scale-set-overview"></a>Méretezési készlet – áttekintés
Méretezési készlet hasonló fogalmak használhatja, mint megismerte hello előző oktatóprogram túl[hozzon létre magas rendelkezésre állású virtuális gépek](tutorial-availability-sets.md). Méretezési csoportban lévő virtuális gépek különböző pontjain hiba és a frissítési tartományok hasonlóan a virtuális gépek rendelkezésre állási csoportba.

Virtuális gépek méretezési csoportban lévő igény szerint jönnek létre. Megadhatja az automatikus skálázási szabályok toocontrol hogyan és mikor virtuális gépek hozzáadásakor vagy eltávolításakor hello méretezési készlet. Ezek a szabályok alapján metrikák például CPU-terhelést, a memória használata vagy a hálózati forgalmat is elindíthatja.

Skálázási készletekben támogatási too1, 000 virtuális gépeken, ha az Azure platformon lemezképet használ fel. A munkaterhelések jelentős telepítés vagy a virtuális gép testreszabása követelmények, Kezdésként túl[hozzon létre egy egyéni Virtuálisgép-lemezkép](tutorial-custom-images.md). Too100 virtuális gépeinek egy méretezési állítható be, ha egyéni lemezkép használatával hozhat létre.


## <a name="create-an-app-tooscale"></a>Hozzon létre egy alkalmazást tooscale
A méretezési csoport létrehozása előtt hozzon létre egy erőforráscsoportot, a [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupAutomate* a hello *EastUS* helye:

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupScaleSet -Location EastUS
```

Egy korábbi oktatóanyagban megtanulta, hogyan túl[automatizálásához Virtuálisgép-konfiguráció](tutorial-automate-vm-deployment.md) egyéni parancsprogramok futtatására szolgáló bővítmény használatával hello. Hozzon létre egy méretezési készlet konfigurációja, majd egy egyéni parancsprogramok futtatására szolgáló bővítmény tooinstall vonatkoznak, és konfigurálja az IIS:

```powershell
# Create a config object
$vmssConfig = New-AzureRmVmssConfig `
    -Location EastUS `
    -SkuCapacity 2 `
    -SkuName Standard_DS2 `
    -UpgradePolicyMode Automatic

# Define hello script for your Custom Script Extension toorun
$publicSettings = @{
    "fileUris" = (,"https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate-iis.ps1");
    "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}

# Use Custom Script Extension tooinstall IIS and configure basic website
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $publicSettings
```

## <a name="create-scale-load-balancer"></a>Skála terheléselosztó létrehozása
Egy Azure terheléselosztó a réteg-4 (TCP, UDP) terheléselosztóhoz, amely a magas rendelkezésre állást biztosít azáltal, hogy a bejövő forgalom kifogástalan állapotú virtuális gépek között. Terheléselosztói állapotfigyelő mintavétel az egyes virtuális gépek egy adott portot figyeli, és csak osztja el a forgalmat tooan működési virtuális gép. További információkért lásd: hello következő oktatóanyaga a [hogyan tooload egyenleg Windows-alapú virtuális gépek](tutorial-load-balancer.md).

Hozzon létre olyan terheléselosztóhoz, amely a nyilvános IP-címmel rendelkezik, majd továbbítja a webes forgalom 80-as porton:

```powershell
# Create a public IP address
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupScaleSet `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIP

# Create a frontend and backend IP pool
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name myFrontEndPool `
  -PublicIpAddress $publicIP
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool

# Create hello load balancer
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroupScaleSet `
  -Name myLoadBalancer `
  -Location EastUS `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool

# Create a load balancer health probe on port 80
Add-AzureRmLoadBalancerProbeConfig -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2

# Create a load balancer rule toodistribute traffic on port 80
Add-AzureRmLoadBalancerRuleConfig `
  -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80

# Update hello load balancer configuration
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="create-a-scale-set"></a>Méretezési készlet létrehozása
Most hozzon létre egy virtuálisgép-méretezési állítható be [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvm). hello alábbi példa létrehoz egy méretezési készletben elnevezett *myScaleSet*:

```powershell
# Reference a virtual machine image from hello gallery
Set-AzureRmVmssStorageProfile $vmssConfig `
  -ImageReferencePublisher MicrosoftWindowsServer `
  -ImageReferenceOffer WindowsServer `
  -ImageReferenceSku 2016-Datacenter `
  -ImageReferenceVersion latest

# Set up information for authenticating with hello virtual machine
Set-AzureRmVmssOsProfile $vmssConfig `
  -AdminUsername azureuser `
  -AdminPassword P@ssword! `
  -ComputerNamePrefix myVM

# Create hello virtual network resources
$subnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -Name "myVnet" `
  -Location "EastUS" `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $subnet
$ipConfig = New-AzureRmVmssIpConfig `
  -Name "myIPConfig" `
  -LoadBalancerBackendAddressPoolsId $lb.BackendAddressPools[0].Id `
  -SubnetId $vnet.Subnets[0].Id

# Attach hello virtual network toohello config object
Add-AzureRmVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name "network-config" `
  -Primary $true `
  -IPConfiguration $ipConfig

# Create hello scale set with hello config object (this step might take a few minutes)
New-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -Name myScaleSet `
  -VirtualMachineScaleSet $vmssConfig
```

Néhány perc toocreate vesz igénybe, és hello méretezési készlet erőforrások és a virtuális gépek konfigurálása.


## <a name="test-your-app"></a>Az alkalmazás tesztelése
toosee az beszerzése hello nyilvános IP-címét a terheléselosztót, a művelet az IIS-webhely [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). hello alábbi példa beszerzi hello IP-címet *myPublicIP* hello méretezési részeként létrehozott:

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupScaleSet -Name myPublicIP | select IpAddress
```

Adja meg a nyilvános IP-cím hello tooa webböngészőben. hello alkalmazásokról, beleértve az adott hello VM betöltése elosztott terheléselosztó felé irányuló forgalom hello hello állomásnevét:

![Futó IIS-webhely](./media/tutorial-create-vmss/running-iis-site.png)

toosee hello méretezési készletben működés közben, akkor is kényszerített frissítési a webes böngésző toosee hello terhelését terheléselosztó forgalom szét az alkalmazást futtató összes hello virtuális gépet.


## <a name="management-tasks"></a>Felügyeleti feladatok
Hello méretezési hello életciklusa során szükség lehet a toorun egy vagy több felügyeleti feladatokat. Emellett érdemes lehet toocreate olyan parancsfájlok, amelyek különböző életciklus-feladatok automatizálásához. Az Azure PowerShell biztosít egy gyorsan toodo ezeket a feladatokat. Az alábbiakban néhány gyakori feladatot.

### <a name="view-vms-in-a-scale-set"></a>Nézet virtuális gépek méretezési csoportban lévő
tooview a skála futó virtuális gépek listájának megadásához használja [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) az alábbiak szerint:

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Loop through hello instanaces in your scale set
for ($i=1; $i -le ($scaleset.Sku.Capacity - 1); $i++) {
    Get-AzureRmVmssVM -ResourceGroupName myResourceGroupScaleSet `
      -VMScaleSetName myScaleSet `
      -InstanceId $i
}
```


### <a name="increase-or-decrease-vm-instances"></a>Növeli vagy csökkenti a Virtuálisgép-példányok
példányok száma toosee hello jelenleg egy méretezési állította, használjon [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) és a lekérdezés *sku.capacity*:

```powershell
Get-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -VMScaleSetName myScaleSet | `
    Select -ExpandProperty Sku
```

Majd manuálisan növelhető és csökkenthető hello méretezési a készletben lévő virtuális gépek száma hello [frissítés-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss). hello alábbi mintakód hello virtuális gépek száma a méretezési készletben túl a*5*:

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Set and update hello capacity of your scale set
$scaleset.sku.capacity = 5
Update-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -Name myScaleSet `
    -VirtualMachineScaleSet $scaleset
```

Ha vesz néhány percet tooupdate hello megadott található példányok száma a méretezési készlet.


### <a name="configure-autoscale-rules"></a>Automatikus skálázási szabályok konfigurálása
Ahelyett, állítsa be manuálisan a skálázási skálázás hello példányok száma, automatikus skálázási szabályok határozza meg. Ezek a szabályok figyelése hello a skála-példány beállítása és válaszol, ennek megfelelően metrikák és adhat meg küszöbértékek alapján. a következő példa hello méretezi-példányok száma hello ki egy esetén hello átlagos CPU-terhelés nagyobb, mint 60 % 5 perc alatt. Ha hello átlagos CPU-terhelés majd alá süllyed 30 % 5 perc alatt, hello példányok méretezése a egy példánya:

```powershell
# Define your scale set information
$mySubscriptionId = (Get-AzureRmSubscription).Id
$myResourceGroup = "myResourceGroupScaleSet"
$myScaleSet = "myScaleSet"
$myLocation = "East US"

# Create a scale up rule tooincrease hello number instances after 60% average CPU usage exceeded for a 5 minute period
$myRuleScaleUp = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -Operator GreaterThan `
  -MetricStatistic Average `
  -Threshold 60 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:05:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Increase `
  -ScaleActionValue 1

# Create a scale down rule toodecrease hello number of instances after 30% average CPU usage over a 5 minute period
$myRuleScaleDown = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -Operator LessThan `
  -MetricStatistic Average `
  -Threshold 30 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:05:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Decrease `
  -ScaleActionValue 1

# Create a scale profile with your scale up and scale down rules
$myScaleProfile = New-AzureRmAutoscaleProfile `
  -DefaultCapacity 2  `
  -MaximumCapacity 10 `
  -MinimumCapacity 2 `
  -Rules $myRuleScaleUp,$myRuleScaleDown `
  -Name "autoprofile"

# Apply hello autoscale rules
Add-AzureRmAutoscaleSetting `
  -Location $myLocation `
  -Name "autosetting" `
  -ResourceGroup $myResourceGroup `
  -TargetResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -AutoscaleProfiles $myScaleProfile
```


## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban létre egy virtuálisgép-méretezési készlet. Megismerte, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Hello egyéni parancsprogramok futtatására szolgáló bővítmény toodefine egy IIS-webhely tooscale használata
> * Hozzon létre egy terheléselosztót a méretezési csoport
> * Hozzon létre egy virtuálisgép-méretezési csoport
> * Növeli vagy csökkenti a méretezési csoportban lévő példányok hello száma
> * Automatikus skálázási szabályok létrehozása

Előzetes toohello következő útmutató toolearn bővebben a hálózati terheléselosztást a virtuális gépek fogalmakat.

> [!div class="nextstepaction"]
> [Virtuális gépek terhelést elosztani](tutorial-load-balancer.md)
