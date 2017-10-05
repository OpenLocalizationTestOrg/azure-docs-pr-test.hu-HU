---
title: "Rendelkezésre állási készletek oktatóanyag Windows virtuális gépek Azure-ban |} Microsoft Docs"
description: "További tudnivalók a rendelkezésre állási csoportok Windows virtuális gépek Azure-ban."
documentationcenter: 
services: virtual-machines-windows
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: d918362106ef93cf47620e0018d363cd510884b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-availability-sets"></a>Rendelkezésre állási készletek használata

Ebben az oktatóanyagban, megtudhatja, hogyan a rendelkezésre állása és megbízhatósága szempontjából a virtuális gép megoldások Azure-ban egy rendelkezésre állási készletek nevű képesség növelésére. Rendelkezésre állási készletek győződjön meg arról, hogy a telepített Azure virtuális gépek több elkülönített hardver fürt különböző pontjain. Ez biztosítja, hogy ha az Azure hardveres vagy szoftveres hiba akkor fordul elő, csak a virtuális gépek alárendelt meghatározott csökkenhet, és, amely a teljes megoldás elérhető és az azt használó ügyfelek szemszögéből működési marad. 

Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * Rendelkezésre állási csoport létrehozása
> * Hozzon létre egy virtuális gép rendelkezésre állási csoportba
> * Ellenőrizze a rendelkezésre álló Virtuálisgép-méretek

Az oktatóanyaghoz az Azure PowerShell-modul 3.6-os vagy újabb verziójára lesz szükség. A verzió azonosításához futtassa a következőt: ` Get-Module -ListAvailable AzureRM`. Ha frissítenie kell, lásd: [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).

## <a name="availability-set-overview"></a>Rendelkezésre állási csoport – áttekintés

Rendelkezésre állási csoport egy olyan logikai jellegű csoportosítását képességgel, győződjön meg arról, hogy a Virtuálisgép-erőforrások helyezi-e benne el különítve egymástól belül egy Azure-adatközpontban telepítésekor használhatja az Azure-ban. Azure biztosítja, hogy a virtuális gépek elhelyezésekor belül egy rendelkezésre állási készlet több fizikai kiszolgálón futtassa, például rackszekrények, a tárolási egység és a hálózati kapcsolók számítási. Ez biztosítja, hogy hardver vagy Azure szoftverhiba lép fel, csökkenhet a virtuális gépek csak egy részét, és az alkalmazás általános mentése marad, és továbbra is elérhetők az ügyfeleknek. Ha létrehozhatnak megbízható felhőalapú megoldásokat szeretne használni az alapvető magasabb rendelkezésre állási csoportokkal.

Mérlegeljük, ahol például 4 előtér-webkiszolgáló, és 2 háttér-virtuális gép egy adatbázist az tipikus Virtuálisgép-alapú megoldás. Az Azure-szeretné a virtuális gépek telepítése előtt határozza meg a két rendelkezésre állási csoportok: egy rendelkezésre állási készletét, a "web" réteg és a rendelkezésre állási csoporthoz az "adatbázis" szinthez. Amikor létrehoz egy új virtuális Gépet, majd megadhatja a rendelkezésre állási csoport egy paramétert az virtuális gép létrehozása parancs, és az Azure automatikusan biztosítja, hogy a virtuális gépeket hoz létre a rendelkezésre álló halmazában munkakönyvtárral több fizikai hardver-erőforrások között. Ez azt jelenti, hogy a fizikai hardverrel, az egyik webkiszolgálón vagy adatbázis-kiszolgáló virtuális gépeken futó probléma van, ha tudja, hogy a webkiszolgáló és az adatbázis virtuális gépek más példányát fogja futhat részletes mert másik hardveren.

Rendelkezésre állási készletek mindig használjon, ha megbízható Virtuálisgép-alapú megoldások Azure-ban telepíteni szeretné.

## <a name="create-an-availability-set"></a>Rendelkezésre állási csoport létrehozása

Rendelkezésre állási készlet használatával hozhat létre [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset). Ebben a példában mindkét, frissítés és a tartalék tartományok számának hivatott *2* esetében a rendelkezésre állási csoportot elnevezett *myAvailabilitySet* a a *myResourceGroupAvailability* erőforráscsoportot.

Hozzon létre egy erőforráscsoportot.

```powershell
New-AzureRmResourceGroup -Name myResourceGroupAvailability -Location EastUS
```


```powershell
New-AzureRmAvailabilitySet `
   -Location EastUS `
   -Name myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability `
   -Managed `
   -PlatformFaultDomainCount 2 `
   -PlatformUpdateDomainCount 2
```

## <a name="create-vms-inside-an-availability-set"></a>Hozzon létre virtuális gépek rendelkezésre állási csoportok belül

Virtuális gépek létre kívánja hozni a rendelkezésre állási készletét, győződjön meg arról, hogy a hardver megfelelően vannak elosztva a belül. Rendelkezésre állási készlet létrehozása után nem lehet hozzáadni egy meglévő virtuális Gépre. 

A hardver egy helyen több frissítési tartományt és a tartalék tartományok tagolódik. Egy **frissítési tartomány** egy csoport a virtuális gépek és a mögöttes fizikai hardver, amely egy időben újra kell indítani. Virtuális gépek ugyanazon **tartalék tartomány** közös tárolási, valamint egy közös forrás- és hálózati kikapcsolás megosztani. 

Amikor létrehoz egy virtuális gép konfigurációs használatával [New-AzureRMVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) a rendelkezésre állási csoportot a használatával megadhatja a `-AvailabilitySetId` paraméterrel adhatja meg a rendelkezésre állási csoport azonosítója.

A 2 virtuális gép létrehozása [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) a rendelkezésre állási csoport.

```powershell
$availabilitySet = Get-AzureRmAvailabilitySet `
    -ResourceGroupName myResourceGroupAvailability `
    -Name myAvailabilitySet

$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupAvailability `
    -Location EastUS `
    -Name MYvNET `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

for ($i=1; $i -le 2; $i++)
{
   $pip = New-AzureRmPublicIpAddress `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -Name "mypublicdns$(Get-Random)" `
        -AllocationMethod Static `
        -IdleTimeoutInMinutes 4

   $nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
        -Name myNetworkSecurityGroupRuleRDP$i `
        -Protocol Tcp `
        -Direction Inbound `
        -Priority 1000 `
        -SourceAddressPrefix * `
        -SourcePortRange * `
        -DestinationAddressPrefix * `
        -DestinationPortRange 3389 `
        -Access Allow

   $nsg = New-AzureRmNetworkSecurityGroup `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -Name myNetworkSecurityGroup$i `
        -SecurityRules $nsgRuleRDP

   $nic = New-AzureRmNetworkInterface `
        -Name myNic$i `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -SubnetId $vnet.Subnets[0].Id `
        -PublicIpAddressId $pip.Id `
        -NetworkSecurityGroupId $nsg.Id

   # Here is where we specify the availability set
   $vm = New-AzureRmVMConfig `
        -VMName myVM$i `
        -VMSize Standard_D1 `
        -AvailabilitySetId $availabilitySet.Id

   $vm = Set-AzureRmVMOperatingSystem `
        -VM $vm `
        -Windows -ComputerName myVM$i `   
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
   $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
   New-AzureRmVM `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -VM $vm
}

```

Hozzon létre, és mindkét virtuális gépek néhány percet vesz igénybe. Ha elkészült, 2 virtuális gép a mögöttes hardver pontjain fog rendelkezni. 

## <a name="check-for-available-vm-sizes"></a>Ellenőrizze az elérhető Virtuálisgép-méretek 

A rendelkezésre állási csoportot később adhat hozzá további virtuális gépek, de milyen Virtuálisgép-méretek állnak rendelkezésre a hardver tudnia kell. Használjon [Get-AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) listázza az összes hardveren elérhető méretek fürtön a rendelkezésre állási csoport számára.

```powershell
Get-AzureRmVMSize `
   -AvailabilitySetName myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability  
```

## <a name="next-steps"></a>Következő lépések

Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Rendelkezésre állási csoport létrehozása
> * Hozzon létre egy virtuális gép rendelkezésre állási csoportba
> * Ellenőrizze a rendelkezésre álló Virtuálisgép-méretek

Virtuálisgép-méretezési csoportok olvashat a következő oktatóanyag továbblépés.

> [!div class="nextstepaction"]
> [Virtuálisgép-méretezési csoport létrehozása](tutorial-create-vmss.md)


