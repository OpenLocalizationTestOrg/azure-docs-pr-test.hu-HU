---
title: "egyéni VM képeket aaaCreate hello Azure PowerShell |} Microsoft Docs"
description: "Útmutató – hozzon létre egy egyéni Virtuálisgép-lemezkép hello Azure PowerShell használatával."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 3a759fe1b7e7b72f531399b0f4a99e341713c6a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-image-of-an-azure-vm-using-powershell"></a>Egy Azure-VM PowerShell-lel egyéni lemezkép létrehozása

Egyéni lemezképek piactéren elérhető rendszerkép hasonló, de Ön hozza létre őket. Egyéni lemezképek lehet például az alkalmazások, alkalmazás, és más operációs rendszer konfigurációjában kerüli használt toobootstrap konfigurációkat. Ebben az oktatóanyagban létrehoz egy Azure virtuális gép saját egyéni rendszerképét. Az alábbiak végrehajtásának módját ismerheti meg:

> [!div class="checklist"]
> * A Sysprep és a virtuális gépek generalize
> * Egyéni lemezkép létrehozása
> * Virtuális gép létrehozása egy egyéni lemezképből
> * Az előfizetésében szereplő összes hello lemezképek felsorolása
> * Lemezkép törlése

Ez az oktatóanyag hello Azure PowerShell 3,6 vagy újabb verziója szükséges. Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója. Ha tooupgrade van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).

## <a name="before-you-begin"></a>Előkészületek

hello lépéseket részletesen tootake egy meglévő virtuális Gépre, és azt újra felhasználható egyéni lemezképet, hogy kapcsolja használatát toocreate új Virtuálisgép-példányok.

Ebben az oktatóanyagban toocomplete hello példában rendelkeznie kell egy meglévő virtuális gépet. Ha szükséges, ez [parancsfájl minta](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) hozhat létre egyet. Ha feldolgozása révén hello oktatóanyag, a csere hello erőforráscsoport és a virtuális gép nevét, ha szükséges.

## <a name="prepare-vm"></a>Virtuális gép előkészítése

toocreate egy virtuális gép lemezképét, tooprepare hello virtuális gép által a virtuális gép felszabadítása és hello forrás virtuális gép általánosítva van az Azure-ban, majd jelölés hello normalizálása kell.

### <a name="generalize-hello-windows-vm-using-sysprep"></a>Generalize hello Windows virtuális gépet a Sysprep

A Sysprep eltávolítja a személyes adatok, többek között, és előkészíti a hello gép toobe képként használni. A Sysprep kapcsolatos részletekért lásd: [hogyan tooUse Sysprep: Bevezetés](http://technet.microsoft.com/library/bb457073.aspx).


1. Csatlakoztassa a toohello virtuális gépet.
2. Nyissa meg a hello parancssort rendszergazdaként. Hello könyvtárváltás túl*%windir%\system32\sysprep*, majd futtassa a *sysprep.exe*.
3. A hello **rendszer-előkészítő eszköz** párbeszédpanelen jelölje ki *adja meg a rendszer Out-of-Box élmény (OOBE)*, és győződjön meg arról, hogy hello *Generalize* jelölőnégyzet be van jelölve.
4. A **leállítási beállítások**, jelölje be *leállítási* majd **OK**.
5. A Sysprep befejezését követően hello virtuális gép leáll. **Ne indítsa újra a virtuális gép hello**.

### <a name="deallocate-and-mark-hello-vm-as-generalized"></a>Felszabadítani, és jelölje be a virtuális gép általánosítva, hello

kép toocreate, hello VM kell toobe felszabadítása. lehetséges, és az Azure-ban általánosítva van megjelölve.

Hello felszabadított virtuális gép használatával [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupImages -Name myVM -Force
```

Hello állapot hello virtuális gép beállítása túl`-Generalized` használatával [Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm). 
   
```powershell
Set-AzureRmVM -ResourceGroupName myResourceGroupImages -Name myVM -Generalized
```


## <a name="create-hello-image"></a>Hello lemezkép létrehozása

Most használatával hozhat létre virtuális gép hello képe [New-AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) és [New-AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage). hello alábbi példakód létrehozza nevű kép *myImage* nevű VM *myVM*.

Hello virtuális gép beolvasása. 

```powershell
$vm = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroupImages
```

Hello lemezkép-konfiguráció létrehozása.

```powershell
$image = New-AzureRmImageConfig -Location EastUS -SourceVirtualMachineId $vm.ID 
```

Hello lemezkép létrehozása.

```powershell
New-AzureRmImage -Image $image -ImageName myImage -ResourceGroupName myResourceGroupImages
``` 

 
## <a name="create-vms-from-hello-image"></a>Hozzon létre a virtuális gépek hello lemezképből

Most, hogy egy lemezképet, létrehozhat egy vagy több új virtuális gépek hello lemezképéről. Virtuális gép létrehozása egy egyéni lemezképből nagyon hasonló toocreating egy virtuális gép egy Piactéri lemezképhez. A Piactéri lemezkép használatakor tooinformation hello kép, kép szolgáltató, ajánlat, SKU és verzió van. Az egyéni lemezképet egyszerűen hello egyéni lemezkép erőforrás tooprovide hello azonosítója. 

A következő parancsfájl hello, azt változó létrehozása *$image* toostore használatáról hello egyéni lemezképet [Get-AzureRmImage](/powershell/module/azurerm.compute/get-azurermimage) és használjuk majd [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage), és adja meg a hello azonosító használatával hello *$image* változó imént létrehozott. 

hello parancsfájlt hoz létre egy elnevezett VM *myVMfromImage* egy új erőforráscsoportot az egyéni lemezkép alapján nevű *myResourceGroupFromImage* a hello *USA nyugati régiója* helyét.


```powershell
$cred = Get-Credential -Message "Enter a username and password for hello virtual machine."

New-AzureRmResourceGroup -Name myResourceGroupFromImage -Location EastUS

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name MYvNET `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

$pip = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name "mypublicdns$(Get-Random)" `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4

  $nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

  $nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP

$nic = New-AzureRmNetworkInterface `
    -Name myNic `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $pip.Id `
    -NetworkSecurityGroupId $nsg.Id

$vmConfig = New-AzureRmVMConfig `
    -VMName myVMfromImage `
    -VMSize Standard_D1 | Set-AzureRmVMOperatingSystem -Windows `
        -ComputerName myComputer `
        -Credential $cred 

# Here is where we create a variable toostore information about hello image 
$image = Get-AzureRmImage `
    -ImageName myImage `
    -ResourceGroupName myResourceGroupImages

# Here is where we specify that we want toocreate hello VM from and image and provide hello image ID
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -Id $image.Id

$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

New-AzureRmVM `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -VM $vmConfig
```

## <a name="image-management"></a>Lemezkép-kezelési 

Az alábbiakban néhány olyan gyakori felügyeleti kép feladatokat, és hogyan toocomplete őket a PowerShell használatával.

Listázza az összes lemezkép neve.

```powershell
$images = Find-AzureRMResource -ResourceType Microsoft.Compute/images 
$images.name
```

Lemezkép törlése. Ez a példa törlések hello nevű kép *myOldImage* a hello *myResourceGroup*.

```powershell
Remove-AzureRmImage `
    -ImageName myOldImage `
    -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban létre egyéni Virtuálisgép-lemezképet. Megismerte, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * A Sysprep és a virtuális gépek generalize
> * Egyéni lemezkép létrehozása
> * Virtuális gép létrehozása egy egyéni lemezképből
> * Az előfizetésében szereplő összes hello lemezképek felsorolása
> * Lemezkép törlése

Előzetes toohello oktatóanyag következő toolearn kapcsolatos hogyan magas rendelkezésre állású virtuális gépek.

> [!div class="nextstepaction"]
> [Magas rendelkezésre állású virtuális gépek létrehozása](tutorial-availability-sets.md)



