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
# <a name="upload-a-generalized-vhd-tooazure-toocreate-a-new-vm"></a><span data-ttu-id="dcf03-103">Egy általánosított virtuális Merevlemezt tooAzure toocreate egy új virtuális gép feltöltése</span><span class="sxs-lookup"><span data-stu-id="dcf03-103">Upload a generalized VHD tooAzure toocreate a new VM</span></span>

<span data-ttu-id="dcf03-104">Ez a témakör ismerteti a nem felügyelt általánosított lemez tooa tárfiók feltöltését, és majd a feltöltött hello lemez egy új virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="dcf03-104">This topic covers uploading a generalized unmanaged disk tooa storage account and then creating a new VM using hello uploaded disk.</span></span> <span data-ttu-id="dcf03-105">Egy általánosított virtuális Merevlemezt lemezkép volt-e az összes személyes fiókadatait a Sysprep segítségével távolítja el.</span><span class="sxs-lookup"><span data-stu-id="dcf03-105">A generalized VHD image has had all of your personal account information removed using Sysprep.</span></span> 

<span data-ttu-id="dcf03-106">Ha azt szeretné, hogy a virtuális gép olyan virtuális merevlemezről speciális tárfiókokban toocreate, [hozzon létre egy virtuális Gépet egy speciális virtuális merevlemezről](sa-create-vm-specialized.md).</span><span class="sxs-lookup"><span data-stu-id="dcf03-106">If you want toocreate a VM from a specialized VHD in a storage account, see [Create a VM from a specialized VHD](sa-create-vm-specialized.md).</span></span>

<span data-ttu-id="dcf03-107">Ez a témakör ismerteti a storage-fiókok használatával, de javasoljuk, hogy az ügyfelek áthelyezését toousing felügyelt helyette.</span><span class="sxs-lookup"><span data-stu-id="dcf03-107">This topic covers using storage accounts, but we recommend customers move toousing Managed Disks instead.</span></span> <span data-ttu-id="dcf03-108">Hogyan tooprepare, töltse fel, és új virtuális gép létrehozása a teljes útmutató által kezelt lemezeken, lásd: [hozzon létre egy új virtuális gép egy általánosított virtuális merevlemez feltöltése tooAzure felügyelt lemezekkel](upload-generalized-managed.md).</span><span class="sxs-lookup"><span data-stu-id="dcf03-108">For a complete walk-through of how tooprepare, upload and create a new VM using managed disks, see [Create a new VM from a generalized VHD uploaded tooAzure using Managed Disks](upload-generalized-managed.md).</span></span>



## <a name="prepare-hello-vm"></a><span data-ttu-id="dcf03-109">Hello virtuális gép előkészítése</span><span class="sxs-lookup"><span data-stu-id="dcf03-109">Prepare hello VM</span></span>

<span data-ttu-id="dcf03-110">Egy általánosított virtuális Merevlemezt volt-e az összes személyes fiókadatait a Sysprep segítségével távolítja el.</span><span class="sxs-lookup"><span data-stu-id="dcf03-110">A generalized VHD has had all of your personal account information removed using Sysprep.</span></span> <span data-ttu-id="dcf03-111">Ha azt tervezi, hogy a virtuális Merevlemezt egy kép toocreate toouse hello kell az új virtuális gépeket:</span><span class="sxs-lookup"><span data-stu-id="dcf03-111">If you intend toouse hello VHD as an image toocreate new VMs from, you should:</span></span>
  
  * <span data-ttu-id="dcf03-112">[Készítse elő a Windows virtuális merevlemez tooupload tooAzure](prepare-for-upload-vhd-image.md).</span><span class="sxs-lookup"><span data-stu-id="dcf03-112">[Prepare a Windows VHD tooupload tooAzure](prepare-for-upload-vhd-image.md).</span></span> 
  * <span data-ttu-id="dcf03-113">Generalize hello virtuális gépet a Sysprep használatával</span><span class="sxs-lookup"><span data-stu-id="dcf03-113">Generalize hello virtual machine using Sysprep</span></span>

### <a name="generalize-a-windows-virtual-machine-using-sysprep"></a><span data-ttu-id="dcf03-114">A Windows virtuális gépek Sysprep generalize</span><span class="sxs-lookup"><span data-stu-id="dcf03-114">Generalize a Windows virtual machine using Sysprep</span></span>
<span data-ttu-id="dcf03-115">Ez a szakasz bemutatja, hogyan toogeneralize képként használható a Windows virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="dcf03-115">This section shows you how toogeneralize your Windows virtual machine for use as an image.</span></span> <span data-ttu-id="dcf03-116">A Sysprep eltávolítja a személyes adatok, többek között, és előkészíti a hello gép toobe képként használni.</span><span class="sxs-lookup"><span data-stu-id="dcf03-116">Sysprep removes all your personal account information, among other things, and prepares hello machine toobe used as an image.</span></span> <span data-ttu-id="dcf03-117">A Sysprep kapcsolatos részletekért lásd: [hogyan tooUse Sysprep: Bevezetés](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="dcf03-117">For details about Sysprep, see [How tooUse Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>

<span data-ttu-id="dcf03-118">Ellenőrizze, hogy fut a gépen hello hello kiszolgálói szerepkörök Sysprep által támogatott.</span><span class="sxs-lookup"><span data-stu-id="dcf03-118">Make sure hello server roles running on hello machine are supported by Sysprep.</span></span> <span data-ttu-id="dcf03-119">További információkért lásd: [Sysprep támogatási kiszolgálói szerepköre tekintetében](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span><span class="sxs-lookup"><span data-stu-id="dcf03-119">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dcf03-120">Ha futtatja a Sysprep eszközt a virtuális merevlemez tooAzure hello az első alkalommal feltöltés előtt, győződjön meg arról, hogy [a virtuális gép előkészített](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a Sysprep futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="dcf03-120">If you are running Sysprep before uploading your VHD tooAzure for hello first time, make sure you have [prepared your VM](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before running Sysprep.</span></span> 
> 
> 

1. <span data-ttu-id="dcf03-121">Bejelentkezés toohello Windows rendszerű virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="dcf03-121">Sign in toohello Windows virtual machine.</span></span>
2. <span data-ttu-id="dcf03-122">Nyissa meg a hello parancssort rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="dcf03-122">Open hello Command Prompt window as an administrator.</span></span> <span data-ttu-id="dcf03-123">Hello könyvtárváltás túl**%windir%\system32\sysprep**, majd futtassa a `sysprep.exe`.</span><span class="sxs-lookup"><span data-stu-id="dcf03-123">Change hello directory too**%windir%\system32\sysprep**, and then run `sysprep.exe`.</span></span>
3. <span data-ttu-id="dcf03-124">A hello **rendszer-előkészítő eszköz** párbeszédpanelen jelölje ki **adja meg a rendszer Out-of-Box élmény (OOBE)**, és győződjön meg arról, hogy hello **Generalize** jelölőnégyzet be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="dcf03-124">In hello **System Preparation Tool** dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that hello **Generalize** check box is selected.</span></span>
4. <span data-ttu-id="dcf03-125">A **leállítási beállítások**, jelölje be **leállítási**.</span><span class="sxs-lookup"><span data-stu-id="dcf03-125">In **Shutdown Options**, select **Shutdown**.</span></span>
5. <span data-ttu-id="dcf03-126">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="dcf03-126">Click **OK**.</span></span>
   
    ![Indítsa el a Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. <span data-ttu-id="dcf03-128">A Sysprep befejezését követően hello virtuális gép leáll.</span><span class="sxs-lookup"><span data-stu-id="dcf03-128">When Sysprep completes, it shuts down hello virtual machine.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="dcf03-129">Újraindítás hello virtuális gép még nem kész feltöltése hello VHD tooAzure vagy kép hello VM történő létrehozását.</span><span class="sxs-lookup"><span data-stu-id="dcf03-129">Do not restart hello VM until you are done uploading hello VHD tooAzure or creating an image from hello VM.</span></span> <span data-ttu-id="dcf03-130">Ha a virtuális gép hello véletlenül lekérdezi az újraindítás futtassa a Sysprep toogeneralize azt újra.</span><span class="sxs-lookup"><span data-stu-id="dcf03-130">If hello VM accidentally gets restarted, run Sysprep toogeneralize it again.</span></span>
> 
> 


## <a name="upload-hello-vhd"></a><span data-ttu-id="dcf03-131">Hello VHD feltöltése</span><span class="sxs-lookup"><span data-stu-id="dcf03-131">Upload hello VHD</span></span>

<span data-ttu-id="dcf03-132">Töltse fel a hello VHD tooan Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="dcf03-132">Upload hello VHD tooan Azure storage account.</span></span>

### <a name="log-in-tooazure"></a><span data-ttu-id="dcf03-133">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="dcf03-133">Log in tooAzure</span></span>
<span data-ttu-id="dcf03-134">Ha még nem rendelkezik PowerShell 1.4-es verzió vagy újabb operációs rendszerre telepíteni, olvassa el [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="dcf03-134">If you don't already have PowerShell version 1.4 or above installed, read [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

1. <span data-ttu-id="dcf03-135">Nyissa meg az Azure PowerShell, és jelentkezzen be Azure-fiók tooyour.</span><span class="sxs-lookup"><span data-stu-id="dcf03-135">Open Azure PowerShell and sign in tooyour Azure account.</span></span> <span data-ttu-id="dcf03-136">Egy előugró ablak nyílik meg tooenter az az Azure-fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="dcf03-136">A pop-up window opens for you tooenter your Azure account credentials.</span></span>
   
    ```powershell
    Login-AzureRmAccount
    ```
2. <span data-ttu-id="dcf03-137">Az elérhető előfizetések beolvasása hello előfizetési azonosítók.</span><span class="sxs-lookup"><span data-stu-id="dcf03-137">Get hello subscription IDs for your available subscriptions.</span></span>
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. <span data-ttu-id="dcf03-138">Állítsa be a megfelelő előfizetés hello hello előfizetés-azonosítójával.</span><span class="sxs-lookup"><span data-stu-id="dcf03-138">Set hello correct subscription using hello subscription ID.</span></span> <span data-ttu-id="dcf03-139">Cserélje le `<subscriptionID>` hello azonosítójú hello, javítsa ki az előfizetést.</span><span class="sxs-lookup"><span data-stu-id="dcf03-139">Replace `<subscriptionID>` with hello ID of hello correct subscription.</span></span>
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

### <a name="get-hello-storage-account"></a><span data-ttu-id="dcf03-140">Hello storage-fiók beszerzése</span><span class="sxs-lookup"><span data-stu-id="dcf03-140">Get hello storage account</span></span>
<span data-ttu-id="dcf03-141">Az Azure toostore feltöltött hello Virtuálisgép-lemezkép tárolási fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="dcf03-141">You need a storage account in Azure toostore hello uploaded VM image.</span></span> <span data-ttu-id="dcf03-142">Meglévő tárfiók használata, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="dcf03-142">You can either use an existing storage account or create a new one.</span></span> 

<span data-ttu-id="dcf03-143">tooshow hello rendelkezésre álló tárfiókok, írja be:</span><span class="sxs-lookup"><span data-stu-id="dcf03-143">tooshow hello available storage accounts, type:</span></span>

```powershell
Get-AzureRmStorageAccount
```

<span data-ttu-id="dcf03-144">Ha azt szeretné, hogy a meglévő tárfiók toouse, lépjen a toohello [feltöltés hello Virtuálisgép-lemezkép](#upload-the-vm-vhd-to-your-storage-account) szakasz.</span><span class="sxs-lookup"><span data-stu-id="dcf03-144">If you want toouse an existing storage account, proceed toohello [Upload hello VM image](#upload-the-vm-vhd-to-your-storage-account) section.</span></span>

<span data-ttu-id="dcf03-145">Ha a tárfiók toocreate van szüksége, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="dcf03-145">If you need toocreate a storage account, follow these steps:</span></span>

1. <span data-ttu-id="dcf03-146">Ahol hozható létre tárfiók hello hello erőforráscsoport nevét hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="dcf03-146">You need hello name of hello resource group where hello storage account should be created.</span></span> <span data-ttu-id="dcf03-147">toofind ki minden hello erőforrás csoportokat, amelyek az előfizetéshez, típus:</span><span class="sxs-lookup"><span data-stu-id="dcf03-147">toofind out all hello resource groups that are in your subscription, type:</span></span>
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    <span data-ttu-id="dcf03-148">Erőforráscsoport nevű toocreate **myResourceGroup** a hello **USA nyugati régiója** régió, írja be:</span><span class="sxs-lookup"><span data-stu-id="dcf03-148">toocreate a resource group named **myResourceGroup** in hello **West US** region, type:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. <span data-ttu-id="dcf03-149">Hozzon létre egy tárfiókot, nevű **mystorageaccount** Ez az erőforráscsoport hello segítségével a [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="dcf03-149">Create a storage account named **mystorageaccount** in this resource group by using hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span></span>
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
 
### <a name="start-hello-upload"></a><span data-ttu-id="dcf03-150">Indítsa el a hello feltöltése</span><span class="sxs-lookup"><span data-stu-id="dcf03-150">Start hello upload</span></span> 

<span data-ttu-id="dcf03-151">Használjon hello [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) parancsmag tooupload hello kép tooa tároló tárfiókba.</span><span class="sxs-lookup"><span data-stu-id="dcf03-151">Use hello [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet tooupload hello image tooa container in your storage account.</span></span> <span data-ttu-id="dcf03-152">Ez a példa feltöltések hello fájl **myVHD.vhd** a `"C:\Users\Public\Documents\Virtual hard disks\"` nevű tooa tárfiók **mystorageaccount** a hello **myResourceGroup** erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="dcf03-152">This example uploads hello file **myVHD.vhd** from `"C:\Users\Public\Documents\Virtual hard disks\"` tooa storage account named **mystorageaccount** in hello **myResourceGroup** resource group.</span></span> <span data-ttu-id="dcf03-153">hello fájl nevű hello tárolóba kerülnek **mycontainer** hello új fájlnevet, és **myUploadedVHD.vhd**.</span><span class="sxs-lookup"><span data-stu-id="dcf03-153">hello file will be placed into hello container named **mycontainer** and hello new file name will be **myUploadedVHD.vhd**.</span></span>

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


<span data-ttu-id="dcf03-154">Sikeres ellenőrzés esetén, a következőhöz hasonló toothis választ kap:</span><span class="sxs-lookup"><span data-stu-id="dcf03-154">If successful, you get a response that looks similar toothis:</span></span>

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

<span data-ttu-id="dcf03-155">A hálózati kapcsolat és hello méretét a VHD-fájl, a parancs eltarthat egy ideig toocomplete.</span><span class="sxs-lookup"><span data-stu-id="dcf03-155">Depending on your network connection and hello size of your VHD file, this command may take a while toocomplete.</span></span>


## <a name="create-a-new-vm"></a><span data-ttu-id="dcf03-156">Új virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="dcf03-156">Create a new VM</span></span> 

<span data-ttu-id="dcf03-157">Most használja hello feltöltött virtuális merevlemez toocreate egy új virtuális Gépet is.</span><span class="sxs-lookup"><span data-stu-id="dcf03-157">You can now use hello uploaded VHD toocreate a new VM.</span></span> 

### <a name="set-hello-uri-of-hello-vhd"></a><span data-ttu-id="dcf03-158">Állítsa be a hello hello VHD URI-val</span><span class="sxs-lookup"><span data-stu-id="dcf03-158">Set hello URI of hello VHD</span></span>

<span data-ttu-id="dcf03-159">a virtuális merevlemez toouse hello URI hello hello formátumban kell megadni: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**.vhd.</span><span class="sxs-lookup"><span data-stu-id="dcf03-159">hello URI for hello VHD toouse is in hello format: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**.vhd.</span></span> <span data-ttu-id="dcf03-160">Ebben a példában a virtuális merevlemez nevű hello **myVHD** hello tárfiók van **mystorageaccount** hello tárolóban **mycontainer**.</span><span class="sxs-lookup"><span data-stu-id="dcf03-160">In this example hello VHD named **myVHD** is in hello storage account **mystorageaccount** in hello container **mycontainer**.</span></span>

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


### <a name="create-a-virtual-network"></a><span data-ttu-id="dcf03-161">Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="dcf03-161">Create a virtual network</span></span>
<span data-ttu-id="dcf03-162">Hozzon létre hello vNet és alhálózat a hello [virtuális hálózati](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="dcf03-162">Create hello vNet and subnet of hello [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="dcf03-163">Hozzon létre hello alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="dcf03-163">Create hello subnet.</span></span> <span data-ttu-id="dcf03-164">hello alábbi minta létrehoz egy nevű alhálózat **mySubnet** hello erőforráscsoportban **myResourceGroup** a címelőtagot hello **10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="dcf03-164">hello following sample creates a subnet named **mySubnet** in hello resource group **myResourceGroup** with hello address prefix of **10.0.0.0/24**.</span></span>  
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="dcf03-165">Hello virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="dcf03-165">Create hello virtual network.</span></span> <span data-ttu-id="dcf03-166">hello alábbi minta létrehoz egy virtuális hálózati nevű **myVnet** a hello **USA nyugati régiója** hellyel, amely címelőtagot hello **10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="dcf03-166">hello following sample creates a virtual network named **myVnet** in hello **West US** location with hello address prefix of **10.0.0.0/16**.</span></span>  
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-network-interface"></a><span data-ttu-id="dcf03-167">Hozzon létre egy nyilvános IP-cím és a hálózati kapcsolat</span><span class="sxs-lookup"><span data-stu-id="dcf03-167">Create a public IP address and network interface</span></span>
<span data-ttu-id="dcf03-168">hello virtuális gép virtuális hálózati hello kommunikációs tooenable, van szüksége egy [nyilvános IP-cím](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) és egy hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="dcf03-168">tooenable communication with hello virtual machine in hello virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="dcf03-169">Hozzon létre egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="dcf03-169">Create a public IP address.</span></span> <span data-ttu-id="dcf03-170">Ez a példa létrehoz egy nyilvános IP-cím nevű **myPip**.</span><span class="sxs-lookup"><span data-stu-id="dcf03-170">This example creates a public IP address named **myPip**.</span></span> 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="dcf03-171">Hozzon létre hello hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="dcf03-171">Create hello NIC.</span></span> <span data-ttu-id="dcf03-172">Ez a példa létrehoz egy hálózati adapter nevű **myNic**.</span><span class="sxs-lookup"><span data-stu-id="dcf03-172">This example creates a NIC named **myNic**.</span></span> 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-hello-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="dcf03-173">Hello hálózati biztonsági csoport és egy RDP-szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="dcf03-173">Create hello network security group and an RDP rule</span></span>
<span data-ttu-id="dcf03-174">toobe képes toolog tooyour a virtuális gép RDP, kell toohave egy biztonsági szabályt, amely lehetővé teszi a hozzáférést RDP a 3389-es porton.</span><span class="sxs-lookup"><span data-stu-id="dcf03-174">toobe able toolog in tooyour VM using RDP, you need toohave a security rule that allows RDP access on port 3389.</span></span> 

<span data-ttu-id="dcf03-175">Ez a példa létrehoz egy NSG nevű **myNsg** nevű szabályt tartalmaz, amelyek **myRdpRule** RDP-forgalmát, amely engedélyezi a 3389-es porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="dcf03-175">This example creates an NSG named **myNsg** that contains a rule called **myRdpRule** that allows RDP traffic over port 3389.</span></span> <span data-ttu-id="dcf03-176">Az NSG-k kapcsolatos további információkért lásd: [portok tooa VM megnyitása az Azure-ban a PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="dcf03-176">For more information about NSGs, see [Opening ports tooa VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


### <a name="create-a-variable-for-hello-virtual-network"></a><span data-ttu-id="dcf03-177">Hozzon létre egy változót hello virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="dcf03-177">Create a variable for hello virtual network</span></span>
<span data-ttu-id="dcf03-178">Hozzon létre egy változót, virtuális hálózat hello befejeződött.</span><span class="sxs-lookup"><span data-stu-id="dcf03-178">Create a variable for hello completed virtual network.</span></span> 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

### <a name="create-hello-vm"></a><span data-ttu-id="dcf03-179">Hello virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="dcf03-179">Create hello VM</span></span>
<span data-ttu-id="dcf03-180">hello következő PowerShell-parancsfájl bemutatja, hogyan hello virtuálisgép-konfigurációk és használata hello tooset feltöltött hello új telepítés hello forrásaként a Virtuálisgép-lemezkép.</span><span class="sxs-lookup"><span data-stu-id="dcf03-180">hello following PowerShell script shows how tooset up hello virtual machine configurations and use hello uploaded VM image as hello source for hello new installation.</span></span>



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

## <a name="verify-that-hello-vm-was-created"></a><span data-ttu-id="dcf03-181">Győződjön meg arról, hogy létrejött a virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="dcf03-181">Verify that hello VM was created</span></span>
<span data-ttu-id="dcf03-182">Amikor végzett, láthatja az újonnan létrehozott virtuális gép hello hello [Azure-portálon](https://portal.azure.com) alatt **Tallózás** > **virtuális gépek**, vagy a következő hello használatával PowerShell-parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="dcf03-182">When complete, you should see hello newly created VM in hello [Azure portal](https://portal.azure.com) under **Browse** > **Virtual machines**, or by using hello following PowerShell commands:</span></span>

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="dcf03-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dcf03-183">Next steps</span></span>
<span data-ttu-id="dcf03-184">toomanage az új virtuális gépet az Azure PowerShell, lásd: [kezelése az Azure Resource Manager és a PowerShell használatával virtuális gépek](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="dcf03-184">toomanage your new virtual machine with Azure PowerShell, see [Manage virtual machines using Azure Resource Manager and PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


