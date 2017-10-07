---
title: "a Windows virtuális gép olyan virtuális merevlemezről speciális az Azure-ban aaaCreate |} Microsoft Docs"
description: "Hozzon létre egy új Windows virtuális gép speciális felügyelt lemezes hello Resource Manager üzembe helyezési modellel használatával hello az operációs rendszer lemezeként való csatolásával."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3b7d3cd5-e3d7-4041-a2a7-0290447458ea
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: cynthn
ms.openlocfilehash: c72c6f4fb650e2664e87cf38ec9be62f0b2209fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-vm-from-a-specialized-disk"></a>A speciális lemezről Windows virtuális gép létrehozása

Új virtuális gép létrehozása a Powershell használatával hello az operációs rendszer lemezeként speciális felügyelt lemezes csatolásával. A speciális lemez egy példányát a virtuális merevlemez (VHD) a meglévő virtuális hello felhasználói fiókok, alkalmazások és más állapot adatait az eredeti virtuális gép által. 

Speciális VHD toocreate egy új virtuális gép használata esetén az új virtuális gép megőrzi hello számítógépneve hello hello eredeti virtuális gép. Olyan számítógép-specifikus információkat is megmarad, és néhány esetben az ismétlődő adatokat hibát okozhat. Vegye figyelembe, milyen típusú számítógép-specifikus adatok az alkalmazások támaszkodnak, ha egy virtuális Gépet.

Erre két lehetősége van:
* [VHD feltöltése](#option-1-upload-a-specialized-vhd)
* [Egy meglévő Azure virtuális gép másolása](#option-2-copy-an-existing-azure-vm)

Ez a témakör bemutatja, hogyan toouse által kezelt lemezeken. Ha egy örökölt üzembe helyezéssel rendelkezik, amelyhez, tárfiókot használ, lásd: [olyan virtuális merevlemezről speciális tárfiók a virtuális gép létrehozása](sa-create-vm-specialized.md)

## <a name="before-you-begin"></a>Előkészületek
Ha a PowerShell segítségével, győződjön meg arról, hogy rendelkezik-e hello hello AzureRM.Compute PowerShell modul legújabb verzióját. 

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
További információkért lásd: [Azure PowerShell Versioning](/powershell/azure/overview).


## <a name="option-1-upload-a-specialized-vhd"></a>1. lehetőség: A speciális virtuális merevlemez feltöltéséhez.

A speciális virtuális gép virtuális merevlemez eszközzel létrehozott egy helyszíni virtualizációs, például a Hyper-V vagy a virtuális gép egy másik felhőből exportált hello feltölthet.

### <a name="prepare-hello-vm"></a>Hello virtuális gép előkészítése
Ha azt tervezi, hogy toouse virtuális Merevlemezt hello-toocreate van egy új virtuális Gépet, győződjön meg arról, hello lépések elvégzése. 
  
  * [Készítse elő a Windows virtuális merevlemez tooupload tooAzure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). **Ne** generalize hello Sysprep használó virtuális gépek.
  * Távolítsa el a Vendég virtualizációs eszközök és a virtuális gép hello (például a VMware-eszközök) telepített ügynökök.
  * Ellenőrizze a virtuális gép hello konfigurált toopull van, az IP-cím és DNS-beállítások DHCP. Ez biztosítja, hogy hello kiszolgálón hello virtuális hálózaton belül egy IP-címet kap, indításkor. 


### <a name="get-hello-storage-account"></a>Hello storage-fiók beszerzése
Tárolási van szüksége az Azure toostore hello fiók feltöltött virtuális merevlemez. Meglévő tárfiók használata, vagy hozzon létre egy újat. 

tooshow hello rendelkezésre álló tárfiókok, írja be:

```powershell
Get-AzureRmStorageAccount
```

Ha azt szeretné, hogy a meglévő tárfiók toouse, lépjen a toohello [feltöltés hello VHD](#upload-the-vhd-to-your-storage-account) szakasz.

Ha a tárfiók toocreate van szüksége, kövesse az alábbi lépéseket:

1. Ahol hozható létre tárfiók hello hello erőforráscsoport nevét hello van szüksége. toofind ki minden hello erőforrás csoportokat, amelyek az előfizetéshez, típus:
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    Erőforráscsoport nevű toocreate *myResourceGroup* a hello *USA nyugati régiója* régió, írja be:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. Hozzon létre egy tárfiókot, nevű *mystorageaccount* Ez az erőforráscsoport hello segítségével a [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) parancsmagot:
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```

### <a name="upload-hello-vhd-tooyour-storage-account"></a>Hello VHD tooyour tárfiók feltöltése 
Használjon hello [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) parancsmag tooupload hello VHD-tooa tároló tárfiókba. Ez a példa feltöltések hello fájl *myVHD.vhd* a `"C:\Users\Public\Documents\Virtual hard disks\"` nevű tooa tárfiók *mystorageaccount* a hello *myResourceGroup* erőforráscsoport. hello fájl nevű hello tárolóban található *mycontainer* hello új fájlnevet, és *myUploadedVHD.vhd*.

```powershell
$resourceGroupName = "myResourceGroup"
$urlOfUploadedVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $resourceGroupName -Destination $urlOfUploadedVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Sikeres ellenőrzés esetén, a következőhöz hasonló toothis választ kap:

```powershell
MD5 hash is being calculated for hello file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for hello operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

A hálózati kapcsolat és hello méretét a VHD-fájl, a parancs eltarthat egy ideig toocomplete

### <a name="create-a-managed-disk-from-hello-vhd"></a>Felügyelt lemezes hello virtuális merevlemez létrehozása

Hozzon létre egy felügyelt lemezes hello a virtuális merevlemez kifejezetten a tárolási fiók használatával [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk). Ez a példa **myOSDisk1** hello lemez neve, a visszahelyezi a lemez hello *StandardLRS* tárolási és felhasználási *https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd* , hello hello forrás VHD URI.

Hozzon létre egy új erőforráscsoportot hello új virtuális Gépet.

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location -Name $destinationResourceGroup
```

Hozzon létre hello új operációsrendszer-lemez hello a feltöltött virtuális merevlemez. 

```powershell
$sourceUri = https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd)
$osDiskName = 'myOsDisk'
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig -AccountType StandardLRS  -Location $location -CreateOption Import `
    -SourceUri $sourceUri) `
    -ResourceGroupName $destinationResourceGroup
```

## <a name="option-2-copy-an-existing-azure-vm"></a>2. lehetőség: Egy meglévő Azure virtuális gép másolása

Létrehozhat egy virtuális gép által használt lemezek kezeli a virtuális gép hello pillanatképének készítése, akkor a pillanatkép toocreate új felügyelt lemezes és új virtuális gép egy másolatát.


### <a name="take-a-snapshot-of-hello-os-disk"></a>Pillanatkép készítése hello operációsrendszer-lemez

Választhat (beleértve az összes lemez) pillanatkép, és a teljes virtuális gép vagy egy egyetlen lemez. hello következő lépések bemutatják, hogyan tootake a virtuális gép használata csak az operációs rendszer hello lemeze pillanatkép hello [New-AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) parancsmag. 

Egyes paraméterek megadása 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$vmName = 'myVM'
$location = 'westus' 
$snapshotName = 'mySnapshot'  
```

Hello Virtuálisgép-objektum beolvasása.

```powershell
$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $resourceGroupName
```
Hello operációsrendszer-lemez nevének beolvasása.

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $vm.StorageProfile.OsDisk.Name
```

Hello pillanatkép-konfiguráció létrehozása. 

 ```powershell
$snapshotConfig =  New-AzureRmSnapshotConfig -SourceUri $disk.Id -OsType Windows -CreateOption Copy -Location $location 
```

Hello pillanatképet.

```powershell
$snapShot = New-AzureRmSnapshot -Snapshot $snapshotConfig -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName
```


Ha azt tervezi, toouse hello pillanatkép toocreate egy virtuális Gépet, amelyet a toobe magas hajt végre, használja a hello paramétert `-AccountType Premium_LRS` a New-AzureRmSnapshot hello paranccsal. hello paraméter hello pillanatképet hoz, hogy a prémium felügyelt lemezként tárolja. Prémium szintű kezelt lemezek drágább szabvány. Ezért mindenképpen valóban szükség van prémium hello paraméter használata előtt.

### <a name="create-a-new-disk-from-hello-snapshot"></a>Hozzon létre egy új lemezt hello pillanatképből

Hello pillanatkép-használatával hozhat létre egy felügyelt lemezt [New-AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk). Ez a példa *myOSDisk* hello lemez neve.

Hozzon létre egy új erőforráscsoportot hello új virtuális Gépet.

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location -Name $destinationResourceGroup
```

Állítsa be a hello az operációs rendszer lemezének neve. 

```powershell
$osDiskName = 'myOsDisk'
```

Hello kezelt lemez létrehozása.

```powershell
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig  -Location $location -CreateOption Copy `
    -SourceResourceId $snapshot.Id) `
    -ResourceGroupName $destinationResourceGroup
```


## <a name="create-hello-new-vm"></a>Hozzon létre új virtuális gép hello 

Hálózati és egyéb hello által használt Virtuálisgép-erőforrások toobe hozzon létre új virtuális Gépet.

### <a name="create-hello-subnet-and-vnet"></a>Hello alhálózat és virtuális hálózat létrehozása

Hozzon létre hello vNet és alhálózat a hello [virtuális hálózati](../../virtual-network/virtual-networks-overview.md).

Hozzon létre hello alhálózatot. Ez a példa-alhálózatot hoz létre nevű **mySubNet**, hello erőforráscsoportban **myDestinationResourceGroup**, és a készlet túl hello alhálózati cím előtag**10.0.0.0/24**.
   
```powershell
$subnetName = 'mySubNet'
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
```

Hello vNet létrehozása. Ebben a példában beállítása virtuális hálózati név toobe hello **myVnetName**, hely túl hello**USA nyugati régiója**, és a virtuális hálózat hello címelőtag túl hello**10.0.0.0/16**. 
   
```powershell
$vnetName = "myVnetName"
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $destinationResourceGroup -Location $location `
    -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
```    


### <a name="create-hello-network-security-group-and-an-rdp-rule"></a>Hello hálózati biztonsági csoport és egy RDP-szabály létrehozása
toobe képes toolog tooyour a virtuális gép RDP, kell toohave egy biztonsági szabályt, amely lehetővé teszi a hozzáférést RDP a 3389-es porton. Mivel a virtuális merevlemez hello hello új virtuális gép egy meglévő speciális virtuális gép alapján készült, az fiók hello forrás virtuális gép RDP használja.

Ez a példa beállítása hello NSG neve túl**myNsg** és hello RDP-szabály neve túl**myRdpRule**.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $destinationResourceGroup -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

Végpontok és NSG-szabályok kapcsolatos további információkért lásd: [portok tooa VM megnyitása az Azure-ban a PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="create-a-public-ip-address-and-nic"></a>Nyilvános IP-cím és a hálózati adapter létrehozása
hello virtuális gép virtuális hálózati hello kommunikációs tooenable, van szüksége egy [nyilvános IP-cím](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) és egy hálózati adapter.

Hozzon létre hello nyilvános IP-cím. Ebben a példában hello nyilvános IP-cím neve túl van beállítva**myIP**.
   
```powershell
$ipName = "myIP"
$pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $destinationResourceGroup -Location $location `
   -AllocationMethod Dynamic
```       

Hozzon létre hello hálózati adaptert. Ebben a példában hello hálózati adapter neve túl van beállítva**myNicName**.
   
```powershell
$nicName = "myNicName"
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $destinationResourceGroup `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```



### <a name="set-hello-vm-name-and-size"></a>Hello virtuális gép neve és mérete

Ez a példa beállítása virtuális gép neve túl hello*myVM* és hello virtuális gép mérete túl*Standard_A2*.

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-hello-nic"></a>A hálózati adapter hello hozzáadása
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    

### <a name="add-hello-os-disk"></a>Az operációs rendszer hello lemez hozzáadása 

Adja hozzá az operációs rendszer hello lemez toohello használt konfiguráció [Set-AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk). Ez a példa beállítja a hello hello lemez túl*128 GB-os* rendeli hello felügyelt lemezt, és egy *Windows* operációsrendszer-lemez.
 
```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -StorageAccountType StandardLRS `
    -DiskSizeInGB 128 -CreateOption Attach -Windows
```

### <a name="complete-hello-vm"></a>Végezze el a virtuális gép hello 

Létre hello VM [New-AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm)hello az imént létrehozott konfigurációkat.

```powershell
New-AzureRmVM -ResourceGroupName $destinationResourceGroup -Location $location -VM $vm
```

Ha ez a parancs sikeres, megjelenik a kimenet ehhez hasonló:

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-hello-vm-was-created"></a>Győződjön meg arról, hogy létrejött a virtuális gép hello
Megtekintheti az hello az újonnan létrehozott virtuális gép vagy hello [Azure-portálon](https://portal.azure.com)a **Tallózás** > **virtuális gépek**, vagy a következő PowerShell hello segítségével parancsok:

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $destinationResourceGroup
$vmList.Name
```

## <a name="next-steps"></a>Következő lépések
a tooyour új virtuális gép esetén böngészés toohello VM a hello toosign [portal](https://portal.azure.com), kattintson **Connect**, és a nyitott hello távoli asztal RDP-fájlt. Hello fiók hitelesítő adatait az eredeti virtuális gép toosign tooyour új virtuális gép használja. További információkért lásd: [hogyan tooconnect és a bejelentkezés tooan Azure virtuális gépen futó Windows](connect-logon.md).

