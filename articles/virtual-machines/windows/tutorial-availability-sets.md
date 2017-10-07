---
title: "aaaAvailability beállítja az oktatóanyag a Windows-alapú virtuális gépek Azure-ban |} Microsoft Docs"
description: "További információk a hello rendelkezésre állási készletek a Windows virtuális gépek Azure-ban."
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
ms.openlocfilehash: 853775c5f126dd815c1933f9d71d2274a75ea661
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-availability-sets"></a>Hogyan toouse rendelkezésre állási beállítása

Ebből az oktatóanyagból megtudhatja, hogyan tooincrease hello rendelkezésre állása és megbízhatósága szempontjából a virtuális gép megoldások Azure-ban egy képesség nevű rendelkezésre állási készletek. Rendelkezésre állási készletek győződjön meg arról, hogy több elkülönített hardver fürt központi telepítése az Azure virtuális gépek elosztott hello. Ez biztosítja, hogy ha az Azure hardveres vagy szoftveres hiba akkor fordul elő, csak a virtuális gépek alárendelt meghatározott csökkenhet, és, amely a teljes megoldás elérhető és az azt használó ügyfelek hello szempontjából működési marad. 

Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * Rendelkezésre állási csoport létrehozása
> * Hozzon létre egy virtuális gép rendelkezésre állási csoportba
> * Ellenőrizze a rendelkezésre álló Virtuálisgép-méretek

Ez az oktatóanyag hello Azure PowerShell 3,6 vagy újabb verziója szükséges. Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója. Ha tooupgrade van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).

## <a name="availability-set-overview"></a>Rendelkezésre állási csoport – áttekintés

A logikai csoportosításhoz képessége, amelyik használható rendelkezésre állási csoport megtalálható, hogy hello Virtuálisgép-erőforrások helyezi-e benne el különítve egymástól belül egy Azure-adatközpontban telepítésekor Azure tooensure. Azure biztosítja, hogy hello helyezi-e belül egy rendelkezésre állási készlet több fizikai kiszolgálókon futó virtuális gépek, a számítási, például rackszekrények, a tárolási egység és a hálózati kapcsolók. Ez biztosítja, hogy hello eseményben hardver vagy Azure szoftverhiba lép fel, csökkenhet a virtuális gépek csak egy részét, és az alkalmazás általános mentése marad, és továbbra is toobe elérhető tooyour ügyfelek. Az alapvető funkció tooleverage rendelkezésre állási csoportokkal akkor, ha azt szeretné, hogy megbízható toobuild megoldások.

Mérlegeljük, ahol például 4 előtér-webkiszolgáló, és 2 háttér-virtuális gép egy adatbázist az tipikus Virtuálisgép-alapú megoldás. Az Azure-érdemes toodefine két rendelkezésre állási csoportok csak a virtuális gépekre telepítheti: egy rendelkezésre állási készlet hello "web" réteg és a rendelkezésre állási csoporthoz hello "adatbázis" szinthez. Egy új virtuális Gépet, majd megadhatja a hello a rendelkezésre állási csoportban paraméter toohello az virtuális gép létrehozása parancs, és az Azure automatikusan biztosítja, hogy hello belül elérhető hello hoz létre virtuális gépek létrehozásakor a készlet több fizikai hardver-erőforrások között munkakönyvtárral. Ez azt jelenti, hogy hello fizikai hardver, az egyik webkiszolgálón vagy adatbázis-kiszolgáló virtuális gépeken futó hibásan működik, ha tudja, hogy hello a webkiszolgáló és az adatbázis virtuális gépek más példányai marad futó részletes mert másik hardveren.

Rendelkezésre állási készletek mindig használjon, ha azt szeretné, hogy toodeploy megbízható Virtuálisgép-alapú megoldások Azure-ban.

## <a name="create-an-availability-set"></a>Rendelkezésre állási csoport létrehozása

Rendelkezésre állási készlet használatával hozhat létre [New-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset). Ebben a példában a frissítés és a tartalék tartományok mindkét hello száma hivatott *2* a hello rendelkezésre állási csoport elnevezett *myAvailabilitySet* a hello *myResourceGroupAvailability*erőforráscsoportot.

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

Virtuális gépek kell toobe hello rendelkezésre állási készlet toomake meg arról, hogy helyesen hello hardver vannak elosztva jött létre. Meglévő virtuális gép tooan rendelkezésre állási készlet létrehozása után nem vehető fel. 

egy helyen hello hardver toomultiple frissítési tartományok és a tartalék tartományok oszlik meg. Egy **frissítési tartomány** virtuális gépek és a mögöttes fizikai hardver, hogy újra kell indítani a hello csoportja ugyanannyi időt vesz igénybe. A virtuális gépek azonos hello **tartalék tartomány** közös tárolási, valamint egy közös forrás- és hálózati kikapcsolás megosztani. 

Amikor létrehoz egy virtuális gép konfigurációs használatával [New-AzureRMVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) hello a rendelkezésre állási csoportban hello segítségével megadhat `-AvailabilitySetId` paraméter toospecify hello azonosító hello rendelkezésre állási csoport.

A 2 virtuális gép létrehozása [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) hello rendelkezésre állási csoport.

```powershell
$availabilitySet = Get-AzureRmAvailabilitySet `
    -ResourceGroupName myResourceGroupAvailability `
    -Name myAvailabilitySet

$cred = Get-Credential -Message "Enter a username and password for hello virtual machine."

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

   # Here is where we specify hello availability set
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

Néhány perc toocreate vesz igénybe, és mindkét virtuális gépek. Ha elkészült, 2 virtuális gépek az alapul szolgáló hardver hello pontjain fog rendelkezni. 

## <a name="check-for-available-vm-sizes"></a>Ellenőrizze az elérhető Virtuálisgép-méretek 

További virtuális gépek toohello rendelkezésre állási csoportban később adhat hozzá, de kell tooknow milyen Virtuálisgép-méretek hello hardveren érhetők el. Használjon [Get-AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) toolist hello rendelkezésre állási csoport fürt összes hello elérhető méretek hello hardveren.

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

Előzetes toohello oktatóanyag következő toolearn kapcsolatos virtuálisgép-méretezési készlet.

> [!div class="nextstepaction"]
> [Virtuálisgép-méretezési csoport létrehozása](tutorial-create-vmss.md)


