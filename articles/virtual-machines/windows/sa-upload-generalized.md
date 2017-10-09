---
title: "egy generalize VHD toocreate aaaUpload több virtuális gép az Azure-ban |} Microsoft Docs"
description: "Töltsön fel egy általánosított virtuális Merevlemezt tooan az Azure storage fiók toocreate egy Windows virtuális gép toouse a hello Resource Manager üzembe helyezési modellben."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: cynthn
ms.openlocfilehash: aa1af2a0acf81685e62853de71afa51e819cb696
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-a-generalized-vhd-tooazure-toocreate-a-new-vm"></a>Egy általánosított virtuális Merevlemezt tooAzure toocreate egy új virtuális gép feltöltése

Ez a témakör ismerteti a nem felügyelt általánosított lemez tooa tárfiók feltöltését, és majd a feltöltött hello lemez egy új virtuális gép létrehozása. Egy általánosított virtuális Merevlemezt lemezkép volt-e az összes személyes fiókadatait a Sysprep segítségével távolítja el. 

Ha azt szeretné, hogy a virtuális gép olyan virtuális merevlemezről speciális tárfiókokban toocreate, [hozzon létre egy virtuális Gépet egy speciális virtuális merevlemezről](sa-create-vm-specialized.md).

Ez a témakör ismerteti a storage-fiókok használatával, de javasoljuk, hogy az ügyfelek áthelyezését toousing felügyelt helyette. Hogyan tooprepare, töltse fel, és új virtuális gép létrehozása a teljes útmutató által kezelt lemezeken, lásd: [hozzon létre egy új virtuális gép egy általánosított virtuális merevlemez feltöltése tooAzure felügyelt lemezekkel](upload-generalized-managed.md).



## <a name="prepare-hello-vm"></a>Hello virtuális gép előkészítése

Egy általánosított virtuális Merevlemezt volt-e az összes személyes fiókadatait a Sysprep segítségével távolítja el. Ha azt tervezi, hogy a virtuális Merevlemezt egy kép toocreate toouse hello kell az új virtuális gépeket:
  
  * [Készítse elő a Windows virtuális merevlemez tooupload tooAzure](prepare-for-upload-vhd-image.md). 
  * Generalize hello virtuális gépet a Sysprep használatával

### <a name="generalize-a-windows-virtual-machine-using-sysprep"></a>A Windows virtuális gépek Sysprep generalize
Ez a szakasz bemutatja, hogyan toogeneralize képként használható a Windows virtuális géphez. A Sysprep eltávolítja a személyes adatok, többek között, és előkészíti a hello gép toobe képként használni. A Sysprep kapcsolatos részletekért lásd: [hogyan tooUse Sysprep: Bevezetés](http://technet.microsoft.com/library/bb457073.aspx).

Ellenőrizze, hogy fut a gépen hello hello kiszolgálói szerepkörök Sysprep által támogatott. További információkért lásd: [Sysprep támogatási kiszolgálói szerepköre tekintetében](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> Ha futtatja a Sysprep eszközt a virtuális merevlemez tooAzure hello az első alkalommal feltöltés előtt, győződjön meg arról, hogy [a virtuális gép előkészített](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a Sysprep futtatása előtt. 
> 
> 

1. Bejelentkezés toohello Windows rendszerű virtuális gép.
2. Nyissa meg a hello parancssort rendszergazdaként. Hello könyvtárváltás túl**%windir%\system32\sysprep**, majd futtassa a `sysprep.exe`.
3. A hello **rendszer-előkészítő eszköz** párbeszédpanelen jelölje ki **adja meg a rendszer Out-of-Box élmény (OOBE)**, és győződjön meg arról, hogy hello **Generalize** jelölőnégyzet be van jelölve.
4. A **leállítási beállítások**, jelölje be **leállítási**.
5. Kattintson az **OK** gombra.
   
    ![Indítsa el a Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. A Sysprep befejezését követően hello virtuális gép leáll. 

> [!IMPORTANT]
> Újraindítás hello virtuális gép még nem kész feltöltése hello VHD tooAzure vagy kép hello VM történő létrehozását. Ha a virtuális gép hello véletlenül lekérdezi az újraindítás futtassa a Sysprep toogeneralize azt újra.
> 
> 


## <a name="upload-hello-vhd"></a>Hello VHD feltöltése

Töltse fel a hello VHD tooan Azure storage-fiók.

### <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure
Ha még nem rendelkezik PowerShell 1.4-es verzió vagy újabb operációs rendszerre telepíteni, olvassa el [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

1. Nyissa meg az Azure PowerShell, és jelentkezzen be Azure-fiók tooyour. Egy előugró ablak nyílik meg tooenter az az Azure-fiók hitelesítő adatait.
   
    ```powershell
    Login-AzureRmAccount
    ```
2. Az elérhető előfizetések beolvasása hello előfizetési azonosítók.
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. Állítsa be a megfelelő előfizetés hello hello előfizetés-azonosítójával. Cserélje le `<subscriptionID>` hello azonosítójú hello, javítsa ki az előfizetést.
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

### <a name="get-hello-storage-account"></a>Hello storage-fiók beszerzése
Az Azure toostore feltöltött hello Virtuálisgép-lemezkép tárolási fiók szükséges. Meglévő tárfiók használata, vagy hozzon létre egy újat. 

tooshow hello rendelkezésre álló tárfiókok, írja be:

```powershell
Get-AzureRmStorageAccount
```

Ha azt szeretné, hogy a meglévő tárfiók toouse, lépjen a toohello [feltöltés hello Virtuálisgép-lemezkép](#upload-the-vm-vhd-to-your-storage-account) szakasz.

Ha a tárfiók toocreate van szüksége, kövesse az alábbi lépéseket:

1. Ahol hozható létre tárfiók hello hello erőforráscsoport nevét hello van szüksége. toofind ki minden hello erőforrás csoportokat, amelyek az előfizetéshez, típus:
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    Erőforráscsoport nevű toocreate **myResourceGroup** a hello **USA nyugati régiója** régió, írja be:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. Hozzon létre egy tárfiókot, nevű **mystorageaccount** Ez az erőforráscsoport hello segítségével a [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) parancsmagot:
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
 
### <a name="start-hello-upload"></a>Indítsa el a hello feltöltése 

Használjon hello [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) parancsmag tooupload hello kép tooa tároló tárfiókba. Ez a példa feltöltések hello fájl **myVHD.vhd** a `"C:\Users\Public\Documents\Virtual hard disks\"` nevű tooa tárfiók **mystorageaccount** a hello **myResourceGroup** erőforráscsoport. hello fájl nevű hello tárolóba kerülnek **mycontainer** hello új fájlnevet, és **myUploadedVHD.vhd**.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
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

A hálózati kapcsolat és hello méretét a VHD-fájl, a parancs eltarthat egy ideig toocomplete.


## <a name="create-a-new-vm"></a>Új virtuális gép létrehozása 

Most használja hello feltöltött virtuális merevlemez toocreate egy új virtuális Gépet is. 

### <a name="set-hello-uri-of-hello-vhd"></a>Állítsa be a hello hello VHD URI-val

a virtuális merevlemez toouse hello URI hello hello formátumban kell megadni: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**.vhd. Ebben a példában a virtuális merevlemez nevű hello **myVHD** hello tárfiók van **mystorageaccount** hello tárolóban **mycontainer**.

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


### <a name="create-a-virtual-network"></a>Virtuális hálózat létrehozása
Hozzon létre hello vNet és alhálózat a hello [virtuális hálózati](../../virtual-network/virtual-networks-overview.md).

1. Hozzon létre hello alhálózatot. hello alábbi minta létrehoz egy nevű alhálózat **mySubnet** hello erőforráscsoportban **myResourceGroup** a címelőtagot hello **10.0.0.0/24**.  
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Hello virtuális hálózat létrehozása. hello alábbi minta létrehoz egy virtuális hálózati nevű **myVnet** a hello **USA nyugati régiója** hellyel, amely címelőtagot hello **10.0.0.0/16**.  
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-network-interface"></a>Hozzon létre egy nyilvános IP-cím és a hálózati kapcsolat
hello virtuális gép virtuális hálózati hello kommunikációs tooenable, van szüksége egy [nyilvános IP-cím](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) és egy hálózati adapter.

1. Hozzon létre egy nyilvános IP-címet. Ez a példa létrehoz egy nyilvános IP-cím nevű **myPip**. 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. Hozzon létre hello hálózati adaptert. Ez a példa létrehoz egy hálózati adapter nevű **myNic**. 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-hello-network-security-group-and-an-rdp-rule"></a>Hello hálózati biztonsági csoport és egy RDP-szabály létrehozása
toobe képes toolog tooyour a virtuális gép RDP, kell toohave egy biztonsági szabályt, amely lehetővé teszi a hozzáférést RDP a 3389-es porton. 

Ez a példa létrehoz egy NSG nevű **myNsg** nevű szabályt tartalmaz, amelyek **myRdpRule** RDP-forgalmát, amely engedélyezi a 3389-es porton keresztül. Az NSG-k kapcsolatos további információkért lásd: [portok tooa VM megnyitása az Azure-ban a PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


### <a name="create-a-variable-for-hello-virtual-network"></a>Hozzon létre egy változót hello virtuális hálózat
Hozzon létre egy változót, virtuális hálózat hello befejeződött. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

### <a name="create-hello-vm"></a>Hello virtuális gép létrehozása
hello következő PowerShell-parancsfájl bemutatja, hogyan hello virtuálisgép-konfigurációk és használata hello tooset feltöltött hello új telepítés hello forrásaként a Virtuálisgép-lemezkép.



```powershell
# Enter a new user name and password toouse as hello local administrator account 
    # for remotely accessing hello VM.
    $cred = Get-Credential

    # Name of hello storage account where hello VHD is located. This example sets hello 
    # storage account name as "myStorageAccount"
    $storageAccName = "myStorageAccount"

    # Name of hello virtual machine. This example sets hello VM name as "myVM".
    $vmName = "myVM"

    # Size of hello virtual machine. This example creates "Standard_D2_v2" sized VM. 
    # See hello VM sizes documentation for more information: 
    # https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "Standard_D2_v2"

    # Computer name for hello VM. This examples sets hello computer name as "myComputer".
    $computerName = "myComputer"

    # Name of hello disk that holds hello OS. This example sets hello 
    # OS disk name as "myOsDisk"
    $osDiskName = "myOsDisk"

    # Assign a SKU name. This example sets hello SKU name as "Standard_LRS"
    # Valid values for -SkuName are: Standard_LRS - locally redundant storage, Standard_ZRS - zone redundant
    # storage, Standard_GRS - geo redundant storage, Standard_RAGRS - read access geo redundant storage,
    # Premium_LRS - premium locally redundant storage. 
    $skuName = "Standard_LRS"

    # Get hello storage account where hello uploaded image is stored
    $storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName

    # Set hello VM name and size
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize

    #Set hello Windows operating system configuration and add hello NIC
    $vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id

    # Create hello OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName

    # Configure hello OS disk toobe created from hello existing VHD image (-CreateOption fromImage).
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows

    # Create hello new VM
    New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

## <a name="verify-that-hello-vm-was-created"></a>Győződjön meg arról, hogy létrejött a virtuális gép hello
Amikor végzett, láthatja az újonnan létrehozott virtuális gép hello hello [Azure-portálon](https://portal.azure.com) alatt **Tallózás** > **virtuális gépek**, vagy a következő hello használatával PowerShell-parancsokkal:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Következő lépések
toomanage az új virtuális gépet az Azure PowerShell, lásd: [kezelése az Azure Resource Manager és a PowerShell használatával virtuális gépek](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


