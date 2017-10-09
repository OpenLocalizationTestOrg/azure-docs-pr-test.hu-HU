---
title: "virtuális gép aaaCreate speciális lemezről az Azure-ban |} Microsoft Docs"
description: "Hozzon létre egy új virtuális gép lemezcsatlakoztatás egy speciális nem felügyelt, hello Resource Manager üzembe helyezési modellben."
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
ms.date: 05/23/2017
ms.author: cynthn
ms.openlocfilehash: c88f213b6629a6c1d6ff5845e76c2f7719672714
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-from-a-specialized-vhd-in-a-storage-account"></a>Olyan virtuális merevlemezről speciális tárfiók a virtuális gép létrehozása

Hozzon létre egy új virtuális Gépet egy speciális nem kezelt lemez csatolása a Powershell használatával hello az operációs rendszer lemezeként. A speciális lemez egy példányát egy meglévő virtuális Gépet, amely megőrzi a hello felhasználói fiókok, alkalmazások és más állapot adatait az eredeti virtuális gép virtuális merevlemez. 

Erre két lehetősége van:
* [VHD feltöltése](create-vm-specialized.md#option-1-upload-a-specialized-vhd)
* [Hello egy meglévő Azure virtuális gép virtuális Merevlemezének másolni](create-vm-specialized.md#option-2-copy-an-existing-azure-vm)

## <a name="before-you-begin"></a>Előkészületek
Ha a PowerShell segítségével, győződjön meg arról, hogy rendelkezik-e hello hello AzureRM.Compute PowerShell modul legújabb verzióját. Futtatás hello parancs tooinstall követően.

```powershell
Install-Module AzureRM.Compute 
```
További információkért lásd: [Azure PowerShell Versioning](/powershell/azure/overview).


## <a name="option-1-upload-a-specialized-vhd"></a>1. lehetőség: A speciális virtuális merevlemez feltöltéséhez.

A speciális virtuális gép virtuális merevlemez eszközzel létrehozott egy helyszíni virtualizációs, például a Hyper-V vagy a virtuális gép egy másik felhőből exportált hello feltölthet.

### <a name="prepare-hello-vm"></a>Hello virtuális gép előkészítése
Egy helyszíni virtuális gép vagy egy másik felhőből exportált virtuális merevlemez használatával létrehozott speciális virtuális merevlemez feltöltése Speciális virtuális merevlemez hello felhasználói fiókok, alkalmazások és más állapot adatait az eredeti virtuális gép kezeli. Ha azt tervezi, hogy toouse virtuális Merevlemezt hello-toocreate van egy új virtuális Gépet, győződjön meg arról, hello lépések elvégzése. 
  
  * [Készítse elő a Windows virtuális merevlemez tooupload tooAzure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). **Ne** generalize hello Sysprep használó virtuális gépek.
  * Távolítsa el a Vendég virtualizációs eszközök és a virtuális gép (azaz a VMware-eszközök) hello telepített ügynökök.
  * Ellenőrizze a virtuális gép hello konfigurált toopull van, az IP-cím és DNS-beállítások DHCP. Ez biztosítja, hogy hello kiszolgálón hello virtuális hálózaton belül egy IP-címet kap, indításkor. 


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
   
### <a name="upload-hello-vhd-tooyour-storage-account"></a>Hello VHD tooyour tárfiók feltöltése
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


## <a name="option-2-copy-hello-vhd-from-an-existing-azure-vm"></a>2. lehetőség: Hello VHD másolhat egy meglévő Azure virtuális Gépen

Új, ismétlődő virtuális gép létrehozásakor a virtuális merevlemez tooanother tárolási fiók toouse másolhatja.

### <a name="before-you-begin"></a>Előkészületek
Győződjön meg arról, hogy Ön:

* Hello információ rendelkezik **forrás és cél tárfiókok**. Hello virtuális gép – forrásként, a kell toohave hello tárolási fiók és a tároló nevét. Általában hello tároló neve lesz **VHD-k**. A cél tárfiókkal toohave is kell. Ha még nem rendelkezik egy, elkészítheti vagy hello portál használatával (**több szolgáltatások** > tárfiókok > hozzáadása) vagy hello segítségével [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) parancsmag. 
* Letöltött és telepített hello [AzCopy eszköz](../../storage/common/storage-use-azcopy.md). 

### <a name="deallocate-hello-vm"></a>Hello virtuális gép felszabadítása
Hello területet szabadít fel hello VHD toobe másolt virtuális gépek felszabadítani. 

* **Portál**: kattintson a **virtuális gépek** > **myVM** > leállítása
* **PowerShell**: használata [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) toostop (felszabadítása) nevű virtuális gép hello **myVM** erőforráscsoportban **myResourceGroup**.

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
```

Hello **állapot** a virtuális gép hello a hello Azure portal-ről változik **leállítva** túl**leállítva (felszabadítva)**.

### <a name="get-hello-storage-account-urls"></a>Hello tárolási fiók URL-címek lekérése
Hello URL-címek hello a forrás és cél tárfiókok van szüksége. hello URL-címei a következőképpen néznek: `https://<storageaccount>.blob.core.windows.net/<containerName>/`. Ha már ismeri hello tárolási fiók és a tároló neve, csak lecserélheti hello adatainak közötti hello zárójeleket toocreate az URL-cím. 

Hello Azure-portálon vagy az Azure Powershell tooget hello URL-cím használható:

* **Portál**: hello kattintson  **>**  a **további szolgáltatások** > **tárfiókok**  >   *a tárfiók* > **Blobok** , és a forrás VHD-fájl valószínűleg hello **VHD-k** tároló. Kattintson a **tulajdonságok** hello tárolót, és címkével hello szöveg másolása **URL-cím**. Mindkét hello forrás és cél tárolók hello URL-címei lesz szüksége. 
* **PowerShell**: használata [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) tooget hello adatai nevű virtuális gép **myVM** hello erőforráscsoportban **myResourceGroup**. Hello eredmények hely hello **tárolási profilban** hello szakasz **virtuális merevlemez Uri**. hello első hello Uri része hello URL-cím toohello tárolót, és hello utolsó része hello hello virtuális gép operációs rendszer virtuális merevlemez nevét.

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
``` 

## <a name="get-hello-storage-access-keys"></a>Hello tárelérési kulcsok beszerzése
Hello elérési kulcsainak hello forrás és cél tárfiókok keresése. Tárelérési kulcsok kapcsolatos további információkért lásd: [tudnivalók az Azure storage-fiókok](../../storage/common/storage-create-storage-account.md).

* **Portál**: kattintson a **további szolgáltatások** > **tárfiókok** > *tárfiók*  >  **Hívóbetűk**. Másolás hello kulcs címkézve **key1**.
* **PowerShell**: használata [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) tooget hello kulcs hello tárfiók **mystorageaccount** hello erőforráscsoportban  **myResourceGroup**. Másolás hello kulcs feliratú **key1**.

```powershell
Get-AzureRmStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup
```

### <a name="copy-hello-vhd"></a>Hello virtuális merevlemez másolása
AzCopy alkalmazó tárfiókok között másolhatja. Hello cél tároló Ha hello megadott tároló nem létezik, akkor létrehozza meg. 

toouse AzCopy, nyisson meg egy parancssort, a helyi számítógépen, és keresse meg a toohello mappa, ahol telepítve van-e az AzCopy. Ez hasonló lesz túl*C:\Program Files (x86) \Microsoft SDKs\Azure\AzCopy*. 

a tárolóban lévő összes hello fájlok toocopy, használhat hello **/S** váltani. Ez lehet az operációs rendszer virtuális merevlemez használt toocopy hello és hello adatok lemezeket lévő hello ugyanabban a tárolóban. A példa bemutatja, hogyan hello tároló összes hello adatbázisfájlok toocopy **mysourcecontainer** tárfiókban **mysourcestorageaccount** toohello tároló **mydestinationcontainer**  a hello **mydestinationstorageaccount** tárfiók. A saját cserélje le a hello storage-fiókok és a tárolók hello nevét. Cserélje le `<sourceStorageAccountKey1>` és `<destinationStorageAccountKey1>` saját kulcsokkal.

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
    /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
    /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

Ha csak szeretné egy adott VHD tároló toocopy több fájl, hello fájlnév hello /Pattern kapcsoló használatával is megadhatja. Ebben a példában csak hello nevű **myFileName.vhd** kerülnek.

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
  /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
  /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
  /Pattern:myFileName.vhd
```


Amikor elkészült, alábbihoz hasonló üzenet jelenik meg:

```
Finished 2 of total 2 file(s).
[2016/10/07 17:37:41] Transfer summary:
-----------------
Total files transferred: 2
Transfer successfully:   2
Transfer skipped:        0
Transfer failed:         0
Elapsed time:            00.00:13:07
```

### <a name="troubleshooting"></a>Hibaelhárítás
* Akkor használja az AZCopy, ha hello hibát látja "Kiszolgáló nem tudta tooauthenticate hello kérelem", ellenőrizze, hello hello engedélyezési fejléc értékének formátuma helyes beleértve hello aláírás. Ha a kulcs 2 vagy a másodlagos hello kulcsot használ, próbáljon hello 1. vagy elsődleges kulcs.

## <a name="create-hello-new-vm"></a>Hozzon létre új virtuális gép hello 

Szüksége toocreate hálózat és a többi virtuális gép erőforrások toobe hello által használt új virtuális Gépet.

### <a name="create-hello-subnet-and-vnet"></a>Hello alhálózat és virtuális hálózat létrehozása

Hozzon létre hello vNet és alhálózat a hello [virtuális hálózati](../../virtual-network/virtual-networks-overview.md).

1. Hozzon létre hello alhálózatot. Ez a példa-alhálózatot hoz létre nevű **mySubNet**, hello erőforráscsoportban **myResourceGroup**, és a készletek hello alhálózati cím előtag túl**10.0.0.0/24**.
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Hello vNet létrehozása. Ebben a példában beállítása virtuális hálózati név toobe hello **myVnetName**, hely túl hello**USA nyugati régiója**, és a virtuális hálózat hello címelőtag túl hello**10.0.0.0/16**. 
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-nic"></a>Nyilvános IP-cím és a hálózati adapter létrehozása
hello virtuális gép virtuális hálózati hello kommunikációs tooenable, van szüksége egy [nyilvános IP-cím](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) és egy hálózati adapter.

1. Hozzon létre hello nyilvános IP-cím. Ebben a példában hello nyilvános IP-cím neve túl van beállítva**myIP**.
   
    ```powershell
    $ipName = "myIP"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. Hozzon létre hello hálózati adaptert. Ebben a példában hello hálózati adapter neve túl van beállítva**myNicName**.
   
    ```powershell
    $nicName = "myNicName"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-hello-network-security-group-and-an-rdp-rule"></a>Hello hálózati biztonsági csoport és egy RDP-szabály létrehozása
toobe képes toolog tooyour a virtuális gép RDP, kell toohave egy biztonsági szabály, amely lehetővé teszi a hozzáférést RDP a 3389-es porton. Mivel hello VHD-t az új virtuális Gépet létrehozták, egy meglévő hello kifejezetten a virtuális gép, miután hello virtuális gép létrehozása után segítségével hello forrás virtuális gépről, akinek engedélye toolog RDP Funkciót használnak a meglévő fiók.
Ez a példa beállítása hello NSG neve túl**myNsg** és hello RDP-szabály neve túl**myRdpRule**.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

Végpontok és NSG-szabályok kapcsolatos további információkért lásd: [portok tooa VM megnyitása az Azure-ban a PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="set-hello-vm-name-and-size"></a>Hello virtuális gép neve és mérete

Ez a példa beállítása hello virtuális gép neve túl "myVM" és a virtuális gép hello méret túl "Standard_A2".
```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-hello-nic"></a>A hálózati adapter hello hozzáadása
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    
    
### <a name="configure-hello-os-disk"></a>Az operációs rendszer hello lemez konfigurálása

1. Set hello URI hello feltöltött, illetve másolt virtuális Merevlemezt. Ebben a példában a VHD-fájl nevű hello **myOsDisk.vhd** nevű tárfiók maradjanak **myStorageAccount** nevű tároló **myContainer**.

    ```powershell
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    ```
2. Adja hozzá az operációs rendszer hello lemezt. Ebben a példában a hello operációsrendszer-lemez létrehozásakor hello kifejezés "osDisk" appened toohello virtuális gép neve toocreate hello operációsrendszer-lemez neve. Ebben a példában is megadhatja, hogy a Windows-alapú virtuális merevlemez csatolt toohello VM hello az operációs rendszer lemezeként.
    
    ```powershell
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
    ```

Nem kötelező: Ha adatlemezt, hogy szükség toobe csatolt toohello VM hello URL-címek adatainak VHD-k segítségével adja hozzá a hello adatlemezek és hello megfelelő logikai egységen (Lun).

```powershell
$dataDiskName = $vmName + "dataDisk"
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 1 -CreateOption attach
```

A storage-fiókok használatakor hello adatokat és operációsrendszer-lemez URL-címek kinéznie: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`. Található ez hello portal böngészés toohello cél tároló, kattintva hello operációs rendszer vagy az adatok másolta, virtuális Merevlemezt, és majd a hello URL-cím hello tartalom másolása.


### <a name="complete-hello-vm"></a>Végezze el a virtuális gép hello 

Hozzon létre virtuális gépet, amely az imént létrehozott hello konfigurációk hello.

```powershell
#Create hello new VM
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
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
$vmList = Get-AzureRmVM -ResourceGroupName $rgName
$vmList.Name
```

## <a name="next-steps"></a>Következő lépések
a tooyour új virtuális gép esetén böngészés toohello VM a hello toosign [portal](https://portal.azure.com), kattintson **Connect**, és a nyitott hello távoli asztal RDP-fájlt. Hello fiók hitelesítő adatait az eredeti virtuális gép toosign tooyour új virtuális gép használja. További információkért lásd: [hogyan tooconnect és a bejelentkezés tooan Azure virtuális gépen futó Windows](connect-logon.md).

