---
title: "a felügyelt Azure helyszíni általánosított virtuális Merevlemezt a virtuális gépek aaaCreate |} Microsoft Docs"
description: "Töltse fel egy általánosított virtuális Merevlemezt tooAzure és toocreate használhatja új virtuális gépek hello Resource Manager üzembe helyezési modellben."
services: virtual-machines-windows
documentationcenter: 
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
ms.date: 05/19/2017
ms.author: cynthn
ms.openlocfilehash: 2fd0c0eec922e6ca8af4e712c1bceb1f9466105c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-a-generalized-vhd-and-use-it-toocreate-new-vms-in-azure"></a>Egy általánosított virtuális merevlemez feltöltéséhez, illetve használják toocreate új virtuális gépek Azure-ban

Ez a témakör bemutatja, hogyan használja a PowerShell tooupload egy általánosított virtuális gép tooAzure virtuális Merevlemezét, kép hello VHD létrehozása, és hozzon létre egy új virtuális Gépet erről a képről. Egy helyszíni virtualizálási eszköz vagy egy másik felhőben az exportált virtuális merevlemez feltöltése Használatával [kezelt lemezek](managed-disks-overview.md) hello az új virtuális gép egyszerűbb hello VM kezelése és jobb rendelkezésre állást biztosít, ha hello virtuális gép rendelkezésre állási csoportba. 

Ha azt szeretné, hogy toouse egy parancsfájlt, lásd: [Sample script tooupload egy virtuális merevlemez tooAzure, és hozzon létre egy új virtuális Gépet](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md)

## <a name="before-you-begin"></a>Előkészületek

- Minden virtuális merevlemez tooAzure feltöltés, előtt kövesse [Windows VHD- vagy VHDX tooupload tooAzure előkészítése](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
- Felülvizsgálati [hello áttelepítés tervezése tooManaged lemezek](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) túl az áttelepítés megkezdése előtt[kezelt lemezek](managed-disks-overview.md).
- Győződjön meg arról, hogy rendelkezik-e hello hello AzureRM.Compute PowerShell modul legújabb verzióját. Futtatás hello parancs tooinstall követően.

    ```powershell
    Install-Module AzureRM.Compute -RequiredVersion 2.6.0
    ```
    További információkért lásd: [Azure PowerShell Versioning](/powershell/azure/overview).


## <a name="generalize-hello-windows-vm-using-sysprep"></a>Generalize hello Windows virtuális gépet a Sysprep

A Sysprep eltávolítja a személyes adatok, többek között, és előkészíti a hello gép toobe képként használni. A Sysprep kapcsolatos részletekért lásd: [hogyan tooUse Sysprep: Bevezetés](http://technet.microsoft.com/library/bb457073.aspx).

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
6. A Sysprep befejezését követően hello virtuális gép leáll. Ne indítsa újra a virtuális gép hello.



## <a name="log-in-tooazure"></a>Jelentkezzen be tooAzure
Ha még nem rendelkezik PowerShell 1.4-es verzió vagy újabb operációs rendszerre telepíteni, olvassa el [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

1. Nyissa meg az Azure PowerShell, és jelentkezzen be Azure-fiók tooyour. Egy előugró ablak nyílik meg tooenter az az Azure-fiók hitelesítő adatait.
   
    ```powershell
    Login-AzureRmAccount
    ```
2. Az elérhető előfizetések beolvasása hello előfizetési azonosítók.
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. Állítsa be a megfelelő előfizetés hello hello előfizetés-azonosítójával. Cserélje le  *<subscriptionID>*  hello azonosítójú hello, javítsa ki az előfizetést.
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="get-hello-storage-account"></a>Hello storage-fiók beszerzése
Az Azure toostore feltöltött hello Virtuálisgép-lemezkép tárolási fiók szükséges. Meglévő tárfiók használata, vagy hozzon létre egy újat. 

Ha használ hello VHD toocreate felügyelt lemezes virtuális gépeknél, hello tárfiókhely ahol létrehozni hello VM hello ugyanott kell lennie.

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

    Erőforráscsoport nevű toocreate **myResourceGroup** a hello **USA keleti régiója** régió, írja be:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "East US"
    ```

2. Hozzon létre egy tárfiókot, nevű **mystorageaccount** Ez az erőforráscsoport hello segítségével a [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) parancsmagot:
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "East US"`
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
   
    -SkuName érvényes értékei:
   
   * **Standard_LRS** -helyileg redundáns tárolás. 
   * **Standard_ZRS** -redundáns tárolási zóna.
   * **Standard_GRS** -földrajzi redundáns tárolás. 
   * **Standard_RAGRS** -írásvédett georedundáns redundáns tárolás. 
   * **Premium_LRS** -prémium helyileg redundáns tárolás. 

## <a name="upload-hello-vhd-tooyour-storage-account"></a>Hello VHD tooyour tárfiók feltöltése

Használjon hello [Add-AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx) parancsmag tooupload hello VHD-tooa tároló tárfiókba. Ez a példa feltöltések hello fájl *myVHD.vhd* a *"C:\Users\Public\Documents\Virtual merevlemezek\"*  nevű tooa tárfiók *mystorageaccount*a hello *myResourceGroup* erőforráscsoportot. hello fájl nevű hello tárolóba kerülnek *mycontainer* hello új fájlnevet, és *myUploadedVHD.vhd*.

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

A hálózati kapcsolat és hello méretét a VHD-fájl, a parancs eltarthat egy ideig toocomplete

Mentse a hello **cél URI** elérési toouse később, ha egy felügyelt lemezes toocreate fog vagy hello segítségével új virtuális gép virtuális merevlemez feltöltése.

### <a name="other-options-for-uploading-a-vhd"></a>Más beállításokat a virtuális merevlemez feltöltése
 
 
Hello következő használó virtuális merevlemez tooyour tárfiókot is feltöltheti:

- [AzCopy](http://aka.ms/downloadazcopy)
- [Az Azure Storage másolási Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Az Azure Storage Explorer feltöltése a BLOB](https://azurestorageexplorer.codeplex.com/)
- [Storage Import/Export szolgáltatás REST API-referencia](https://msdn.microsoft.com/library/dn529096.aspx)
-   Ajánlott Import/Export szolgáltatás használata, ha becsült a 7 napnál hosszabb idő feltöltése. Használhat [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) adatok méretét és átviteli egység tooestimate hello időpontját. 
    Importálási/exportálási lehet toocopy tooa standard szintű tárfiókot használja. Standard szintű tárolást toopremium tárkonfigurációt AzCopy hasonló eszköz használatával toocopy kell.


## <a name="create-a-managed-image-from-hello-uploaded-vhd"></a>Hozzon létre egy felügyelt hello lemezkép feltöltött virtuális merevlemez 

Hozzon létre egy felügyelt képre az operációs rendszer általánosított példányát VHD használatával. Hello értékek cserélje le a saját adatait.


1.  Első lépésként állítsa be az általános paraméterek hello:

    ```powershell
    $vmName = "myVM"
    $computerName = "myComputer"
    $vmSize = "Standard_DS1_v2"
    $location = "East US" 
    $imageName = "yourImageName"
    ```

4.  Az operációs rendszer általánosított példányát VHD hello lemezkép létrehozása.

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $urlOfUploadedImageVhd
    $image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```

## <a name="create-a-virtual-network"></a>Virtuális hálózat létrehozása
Hozzon létre hello vNet és alhálózat a hello [virtuális hálózati](../../virtual-network/virtual-networks-overview.md).

1. Hozzon létre hello alhálózatot. Ez a példa-alhálózatot hoz létre nevű *mySubnet* a címelőtagot hello *10.0.0.0/24*.  
   
    ```powershell
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Hello virtuális hálózat létrehozása. Ebben a példában egy virtuális hálózatot nevű hoz létre *myVnet* a címelőtagot hello *10.0.0.0/16*.  
   
    ```powershell
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-network-interface"></a>Hozzon létre egy nyilvános IP-cím és a hálózati kapcsolat

hello virtuális gép virtuális hálózati hello kommunikációs tooenable, van szüksége egy [nyilvános IP-cím](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) és egy hálózati adapter.

1. Hozzon létre egy nyilvános IP-címet. Ez a példa létrehoz egy nyilvános IP-cím nevű *myPip*. 
   
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

## <a name="create-hello-network-security-group-and-an-rdp-rule"></a>Hello hálózati biztonsági csoport és egy RDP-szabály létrehozása

toobe képes toolog tooyour a virtuális gép RDP, kell toohave hálózati biztonsági szabály (NSG), amely lehetővé teszi a hozzáférést RDP a 3389-es porton. 

Ez a példa létrehoz egy NSG nevű *myNsg* nevű szabályt tartalmaz, amelyek *myRdpRule* RDP-forgalmát, amely engedélyezi a 3389-es porton keresztül. Az NSG-k kapcsolatos további információkért lásd: [portok tooa VM megnyitása az Azure-ban a PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

```powershell
$nsgName = "myNsg"
$ruleName = "myRdpRule"
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-hello-virtual-network"></a>Hozzon létre egy változót hello virtuális hálózat

Hozzon létre egy változót, virtuális hálózat hello befejeződött. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

## <a name="get-hello-credentials-for-hello-vm"></a>A virtuális gép hello hello hitelesítő adatainak lekérése

hello következő parancsmag megnyílik egy ablak, ha beírja egy új felhasználói nevet és jelszót toouse hello helyi rendszergazda fiókot távolról a virtuális gép hello eléréséhez. 

```powershell
$cred = Get-Credential
```

## <a name="add-hello-vm-name-and-size-toohello-vm-configuration"></a>Adja hozzá a hello virtuális gép neve és mérete toohello Virtuálisgép-konfiguráció.

```powershell
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

## <a name="set-hello-vm-image-as-source-image-for-hello-new-vm"></a>Set hello Virtuálisgép-lemezkép forrás képként hello az új virtuális gép

Állítsa be a hello forráslemezkép hello felügyelt Virtuálisgép-lemezkép hello Azonosítóját használja.

```powershell
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## <a name="set-hello-os-configuration-and-add-hello-nic"></a>Állítsa be a hello operációs rendszer konfigurációja, és adja hozzá a hello hálózati adaptert.

Adja meg a hello tárolási típust (PremiumLRS vagy StandardLRS) és hello hello operációsrendszer-lemez méretét. Ez a példa beállítja hello fiók típusa túl*PremiumLRS*, lemez mérete túl hello*128 GB-os* és a lemez gyorsítótár túl*ReadWrite*.

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## <a name="create-hello-vm"></a>Hello virtuális gép létrehozása

Hello hello tárolt hello-konfigurációt használni, új virtuális gép létrehozása **$vm** változó.

```powershell
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

## <a name="verify-that-hello-vm-was-created"></a>Győződjön meg arról, hogy létrejött a virtuális gép hello
Amikor végzett, láthatja az újonnan létrehozott virtuális gép hello hello [Azure-portálon](https://portal.azure.com) alatt **Tallózás** > **virtuális gépek**, vagy a következő hello használatával PowerShell-parancsokkal:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Következő lépések

a tooyour új virtuális gép esetén böngészés toohello VM a hello toosign [portal](https://portal.azure.com), kattintson **Connect**, és a nyitott hello távoli asztal RDP-fájlt. Hello fiók hitelesítő adatait az eredeti virtuális gép toosign tooyour új virtuális gép használja. További információkért lásd: [hogyan tooconnect és a bejelentkezés tooan Azure virtuális gépen futó Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

