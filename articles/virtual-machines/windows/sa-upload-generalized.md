---
title: "Töltse fel a VHD-fájlt több virtuális gép létrehozása az Azure-ban generalize |} Microsoft Docs"
description: "Töltse fel a Resource Manager üzembe helyezési modellben használandó Windows virtuális gép létrehozása Azure storage-fiók egy általánosított virtuális Merevlemezt."
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
ms.openlocfilehash: e6fc49855b449a7723a7f8a0c1c41516b3a44ee5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="upload-a-generalized-vhd-to-azure-to-create-a-new-vm"></a><span data-ttu-id="84a3a-103">Egy általánosított virtuális Merevlemezt feltöltése az Azure-bA egy új virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="84a3a-103">Upload a generalized VHD to Azure to create a new VM</span></span>

<span data-ttu-id="84a3a-104">Ez a témakör egy általánosított nem felügyelt lemezt feltöltése a tárfiókba, és majd létrehoz egy új virtuális Gépet, a feltöltött lemez használatával.</span><span class="sxs-lookup"><span data-stu-id="84a3a-104">This topic covers uploading a generalized unmanaged disk to a storage account and then creating a new VM using the uploaded disk.</span></span> <span data-ttu-id="84a3a-105">Egy általánosított virtuális Merevlemezt lemezkép volt-e az összes személyes fiókadatait a Sysprep segítségével távolítja el.</span><span class="sxs-lookup"><span data-stu-id="84a3a-105">A generalized VHD image has had all of your personal account information removed using Sysprep.</span></span> 

<span data-ttu-id="84a3a-106">Ha szeretne létrehozni egy virtuális gép olyan virtuális merevlemezről speciális tárfiókokban, [hozzon létre egy virtuális Gépet egy speciális virtuális merevlemezről](sa-create-vm-specialized.md).</span><span class="sxs-lookup"><span data-stu-id="84a3a-106">If you want to create a VM from a specialized VHD in a storage account, see [Create a VM from a specialized VHD](sa-create-vm-specialized.md).</span></span>

<span data-ttu-id="84a3a-107">Ez a témakör ismerteti a storage-fiókok használatával, de ügyfelek áthelyezés felügyelt lemezek használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="84a3a-107">This topic covers using storage accounts, but we recommend customers move to using Managed Disks instead.</span></span> <span data-ttu-id="84a3a-108">Teljes segédlet előkészítése, töltse fel, és hozzon létre egy új virtuális Gépet felügyelt lemezt használ, tekintse meg [hozzon létre egy új virtuális Gépet egy általánosított virtuális Merevlemezt a feltöltött Azure-ban kezelt lemezek](upload-generalized-managed.md).</span><span class="sxs-lookup"><span data-stu-id="84a3a-108">For a complete walk-through of how to prepare, upload and create a new VM using managed disks, see [Create a new VM from a generalized VHD uploaded to Azure using Managed Disks](upload-generalized-managed.md).</span></span>



## <a name="prepare-the-vm"></a><span data-ttu-id="84a3a-109">A virtuális gép előkészítése</span><span class="sxs-lookup"><span data-stu-id="84a3a-109">Prepare the VM</span></span>

<span data-ttu-id="84a3a-110">Egy általánosított virtuális Merevlemezt volt-e az összes személyes fiókadatait a Sysprep segítségével távolítja el.</span><span class="sxs-lookup"><span data-stu-id="84a3a-110">A generalized VHD has had all of your personal account information removed using Sysprep.</span></span> <span data-ttu-id="84a3a-111">Ha azt tervezi, a virtuális merevlemez használata képként az új virtuális gépek létrehozásához, a következőket:</span><span class="sxs-lookup"><span data-stu-id="84a3a-111">If you intend to use the VHD as an image to create new VMs from, you should:</span></span>
  
  * <span data-ttu-id="84a3a-112">[Az Azure-bA feltölteni egy Windows virtuális merevlemez előkészítése](prepare-for-upload-vhd-image.md).</span><span class="sxs-lookup"><span data-stu-id="84a3a-112">[Prepare a Windows VHD to upload to Azure](prepare-for-upload-vhd-image.md).</span></span> 
  * <span data-ttu-id="84a3a-113">A virtuális gépet a Sysprep használatával generalize</span><span class="sxs-lookup"><span data-stu-id="84a3a-113">Generalize the virtual machine using Sysprep</span></span>

### <a name="generalize-a-windows-virtual-machine-using-sysprep"></a><span data-ttu-id="84a3a-114">A Windows virtuális gépek Sysprep generalize</span><span class="sxs-lookup"><span data-stu-id="84a3a-114">Generalize a Windows virtual machine using Sysprep</span></span>
<span data-ttu-id="84a3a-115">Ez a szakasz bemutatja, hogyan általánosítja a Windows rendszerű virtuális gép képként használatra.</span><span class="sxs-lookup"><span data-stu-id="84a3a-115">This section shows you how to generalize your Windows virtual machine for use as an image.</span></span> <span data-ttu-id="84a3a-116">A Sysprep eltávolítja a személyes adatok, többek között, és előkészíti a számítógépet, hogy képként használni.</span><span class="sxs-lookup"><span data-stu-id="84a3a-116">Sysprep removes all your personal account information, among other things, and prepares the machine to be used as an image.</span></span> <span data-ttu-id="84a3a-117">A Sysprep kapcsolatos részletekért lásd: [hogyan használja a Sysprep: Bevezetés](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="84a3a-117">For details about Sysprep, see [How to Use Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>

<span data-ttu-id="84a3a-118">Győződjön meg arról, hogy a Sysprep által a gépen futó kiszolgálói szerepkörök támogatottak.</span><span class="sxs-lookup"><span data-stu-id="84a3a-118">Make sure the server roles running on the machine are supported by Sysprep.</span></span> <span data-ttu-id="84a3a-119">További információkért lásd: [Sysprep támogatási kiszolgálói szerepköre tekintetében](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span><span class="sxs-lookup"><span data-stu-id="84a3a-119">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="84a3a-120">Ha előtt a virtuális merevlemez feltöltése az Azure-bA az első alkalommal futtatja a Sysprep, győződjön meg arról, hogy [a virtuális gép előkészített](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a Sysprep futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="84a3a-120">If you are running Sysprep before uploading your VHD to Azure for the first time, make sure you have [prepared your VM](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before running Sysprep.</span></span> 
> 
> 

1. <span data-ttu-id="84a3a-121">Jelentkezzen be a Windows rendszerű virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="84a3a-121">Sign in to the Windows virtual machine.</span></span>
2. <span data-ttu-id="84a3a-122">Nyissa meg a parancssort rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="84a3a-122">Open the Command Prompt window as an administrator.</span></span> <span data-ttu-id="84a3a-123">Lépjen be **%windir%\system32\sysprep**, majd futtassa a `sysprep.exe`.</span><span class="sxs-lookup"><span data-stu-id="84a3a-123">Change the directory to **%windir%\system32\sysprep**, and then run `sysprep.exe`.</span></span>
3. <span data-ttu-id="84a3a-124">Az a **rendszer-előkészítő eszköz** párbeszédpanelen jelölje ki **adja meg a rendszer Out-of-Box élmény (OOBE)**, és győződjön meg arról, hogy a **Generalize** jelölőnégyzet be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="84a3a-124">In the **System Preparation Tool** dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that the **Generalize** check box is selected.</span></span>
4. <span data-ttu-id="84a3a-125">A **leállítási beállítások**, jelölje be **leállítási**.</span><span class="sxs-lookup"><span data-stu-id="84a3a-125">In **Shutdown Options**, select **Shutdown**.</span></span>
5. <span data-ttu-id="84a3a-126">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="84a3a-126">Click **OK**.</span></span>
   
    ![Indítsa el a Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. <span data-ttu-id="84a3a-128">A Sysprep befejezését követően a virtuális gép leáll.</span><span class="sxs-lookup"><span data-stu-id="84a3a-128">When Sysprep completes, it shuts down the virtual machine.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="84a3a-129">Ne indítsa újra a virtuális gép, amíg elkészült, a virtuális merevlemez feltöltése az Azure-bA vagy lemezkép létrehozása a virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="84a3a-129">Do not restart the VM until you are done uploading the VHD to Azure or creating an image from the VM.</span></span> <span data-ttu-id="84a3a-130">Ha a virtuális gép véletlenül lekérdezi újra, futtassa a Sysprep általánosítja azt újra.</span><span class="sxs-lookup"><span data-stu-id="84a3a-130">If the VM accidentally gets restarted, run Sysprep to generalize it again.</span></span>
> 
> 


## <a name="upload-the-vhd"></a><span data-ttu-id="84a3a-131">A virtuális merevlemez feltöltéséhez</span><span class="sxs-lookup"><span data-stu-id="84a3a-131">Upload the VHD</span></span>

<span data-ttu-id="84a3a-132">A VHD-fájlt feltölti egy Azure storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="84a3a-132">Upload the VHD to an Azure storage account.</span></span>

### <a name="log-in-to-azure"></a><span data-ttu-id="84a3a-133">Jelentkezzen be az Azure-ba</span><span class="sxs-lookup"><span data-stu-id="84a3a-133">Log in to Azure</span></span>
<span data-ttu-id="84a3a-134">Ha még nem rendelkezik PowerShell 1.4-es verzió vagy újabb operációs rendszerre telepíteni, olvassa el [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="84a3a-134">If you don't already have PowerShell version 1.4 or above installed, read [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

1. <span data-ttu-id="84a3a-135">Nyissa meg az Azure PowerShell, és jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="84a3a-135">Open Azure PowerShell and sign in to your Azure account.</span></span> <span data-ttu-id="84a3a-136">Egy előugró ablak nyílik meg Azure-fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="84a3a-136">A pop-up window opens for you to enter your Azure account credentials.</span></span>
   
    ```powershell
    Login-AzureRmAccount
    ```
2. <span data-ttu-id="84a3a-137">Az előfizetési azonosítók az elérhető előfizetések beolvasása.</span><span class="sxs-lookup"><span data-stu-id="84a3a-137">Get the subscription IDs for your available subscriptions.</span></span>
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. <span data-ttu-id="84a3a-138">Állítsa be a megfelelő előfizetés az előfizetés-azonosítójával.</span><span class="sxs-lookup"><span data-stu-id="84a3a-138">Set the correct subscription using the subscription ID.</span></span> <span data-ttu-id="84a3a-139">Cserélje le `<subscriptionID>` , azonosító: a megfelelő előfizetés.</span><span class="sxs-lookup"><span data-stu-id="84a3a-139">Replace `<subscriptionID>` with the ID of the correct subscription.</span></span>
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

### <a name="get-the-storage-account"></a><span data-ttu-id="84a3a-140">A storage-fiók beszerzése</span><span class="sxs-lookup"><span data-stu-id="84a3a-140">Get the storage account</span></span>
<span data-ttu-id="84a3a-141">Kell egy a feltöltött Virtuálisgép-lemezkép tárolásához Azure storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="84a3a-141">You need a storage account in Azure to store the uploaded VM image.</span></span> <span data-ttu-id="84a3a-142">Meglévő tárfiók használata, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="84a3a-142">You can either use an existing storage account or create a new one.</span></span> 

<span data-ttu-id="84a3a-143">A rendelkezésre álló tárfiókok megjelenítéséhez írja be:</span><span class="sxs-lookup"><span data-stu-id="84a3a-143">To show the available storage accounts, type:</span></span>

```powershell
Get-AzureRmStorageAccount
```

<span data-ttu-id="84a3a-144">Ha egy meglévő tárfiókot használni szeretne, ugorjon a [a Virtuálisgép-lemezkép feltöltése](#upload-the-vm-vhd-to-your-storage-account) szakasz.</span><span class="sxs-lookup"><span data-stu-id="84a3a-144">If you want to use an existing storage account, proceed to the [Upload the VM image](#upload-the-vm-vhd-to-your-storage-account) section.</span></span>

<span data-ttu-id="84a3a-145">Ha létrehoz egy tárfiókot van szüksége, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="84a3a-145">If you need to create a storage account, follow these steps:</span></span>

1. <span data-ttu-id="84a3a-146">Az erőforráscsoport, ahol kell létrehozni a tárfiók neve van szüksége.</span><span class="sxs-lookup"><span data-stu-id="84a3a-146">You need the name of the resource group where the storage account should be created.</span></span> <span data-ttu-id="84a3a-147">Tudja meg, amelyek az előfizetésében szereplő összes erőforráscsoport, írja be:</span><span class="sxs-lookup"><span data-stu-id="84a3a-147">To find out all the resource groups that are in your subscription, type:</span></span>
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    <span data-ttu-id="84a3a-148">Nevű erőforráscsoport létrehozásához **myResourceGroup** a a **USA nyugati régiója** régió, írja be:</span><span class="sxs-lookup"><span data-stu-id="84a3a-148">To create a resource group named **myResourceGroup** in the **West US** region, type:</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. <span data-ttu-id="84a3a-149">Hozzon létre egy tárfiókot, nevű **mystorageaccount** Ez az erőforráscsoport segítségével a a [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="84a3a-149">Create a storage account named **mystorageaccount** in this resource group by using the [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:</span></span>
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
 
### <a name="start-the-upload"></a><span data-ttu-id="84a3a-150">Feltöltés indításához</span><span class="sxs-lookup"><span data-stu-id="84a3a-150">Start the upload</span></span> 

<span data-ttu-id="84a3a-151">Használja a [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) parancsmag segítségével feltölti a lemezképet a tárfiókban lévő tárolót.</span><span class="sxs-lookup"><span data-stu-id="84a3a-151">Use the [Add-AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet to upload the image to a container in your storage account.</span></span> <span data-ttu-id="84a3a-152">Ez a példa feltölti **myVHD.vhd** a `"C:\Users\Public\Documents\Virtual hard disks\"` tárfiókba nevű **mystorageaccount** a a **myResourceGroup** erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="84a3a-152">This example uploads the file **myVHD.vhd** from `"C:\Users\Public\Documents\Virtual hard disks\"` to a storage account named **mystorageaccount** in the **myResourceGroup** resource group.</span></span> <span data-ttu-id="84a3a-153">A fájl bekerülnek a nevű tárolót **mycontainer** és az új fájlnevet lesz **myUploadedVHD.vhd**.</span><span class="sxs-lookup"><span data-stu-id="84a3a-153">The file will be placed into the container named **mycontainer** and the new file name will be **myUploadedVHD.vhd**.</span></span>

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


<span data-ttu-id="84a3a-154">Sikeres ellenőrzés esetén, a következőhöz hasonló választ kap:</span><span class="sxs-lookup"><span data-stu-id="84a3a-154">If successful, you get a response that looks similar to this:</span></span>

```powershell
MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for the operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

<span data-ttu-id="84a3a-155">Attól függően, hogy a hálózati kapcsolat és a VHD-fájl mérete a parancs is igénybe vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="84a3a-155">Depending on your network connection and the size of your VHD file, this command may take a while to complete.</span></span>


## <a name="create-a-new-vm"></a><span data-ttu-id="84a3a-156">Új virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="84a3a-156">Create a new VM</span></span> 

<span data-ttu-id="84a3a-157">A feltöltött virtuális merevlemez segítségével most hozzon létre egy új virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="84a3a-157">You can now use the uploaded VHD to create a new VM.</span></span> 

### <a name="set-the-uri-of-the-vhd"></a><span data-ttu-id="84a3a-158">A virtuális merevlemez URI beállítása</span><span class="sxs-lookup"><span data-stu-id="84a3a-158">Set the URI of the VHD</span></span>

<span data-ttu-id="84a3a-159">URI-JÁNAK a VHD-fájlt használjon a formátumban kell megadni: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**.vhd.</span><span class="sxs-lookup"><span data-stu-id="84a3a-159">The URI for the VHD to use is in the format: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**.vhd.</span></span> <span data-ttu-id="84a3a-160">Ebben a példában a virtuális merevlemez nevű **myVHD** szerepel a tárfiók **mystorageaccount** tárolóban **mycontainer**.</span><span class="sxs-lookup"><span data-stu-id="84a3a-160">In this example the VHD named **myVHD** is in the storage account **mystorageaccount** in the container **mycontainer**.</span></span>

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


### <a name="create-a-virtual-network"></a><span data-ttu-id="84a3a-161">Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="84a3a-161">Create a virtual network</span></span>
<span data-ttu-id="84a3a-162">Hozza létre a virtuális hálózat és az alhálózati a [virtuális hálózati](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="84a3a-162">Create the vNet and subnet of the [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="84a3a-163">Hozza létre az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="84a3a-163">Create the subnet.</span></span> <span data-ttu-id="84a3a-164">Az alábbi minta létrehoz egy nevű alhálózat **mySubnet** erőforráscsoportban **myResourceGroup** a címelőtagot rendelkező **10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="84a3a-164">The following sample creates a subnet named **mySubnet** in the resource group **myResourceGroup** with the address prefix of **10.0.0.0/24**.</span></span>  
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="84a3a-165">Hozza létre a virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="84a3a-165">Create the virtual network.</span></span> <span data-ttu-id="84a3a-166">Az alábbi minta létrehoz egy virtuális hálózati nevű **myVnet** a a **USA nyugati régiója** hellyel, amely a címelőtagot **10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="84a3a-166">The following sample creates a virtual network named **myVnet** in the **West US** location with the address prefix of **10.0.0.0/16**.</span></span>  
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-network-interface"></a><span data-ttu-id="84a3a-167">Hozzon létre egy nyilvános IP-cím és a hálózati kapcsolat</span><span class="sxs-lookup"><span data-stu-id="84a3a-167">Create a public IP address and network interface</span></span>
<span data-ttu-id="84a3a-168">A virtuális hálózaton a virtuális géppel való kommunikáció biztosításához egy [nyilvános IP-cím](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) és egy hálózati adapter szükséges.</span><span class="sxs-lookup"><span data-stu-id="84a3a-168">To enable communication with the virtual machine in the virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="84a3a-169">Hozzon létre egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="84a3a-169">Create a public IP address.</span></span> <span data-ttu-id="84a3a-170">Ez a példa létrehoz egy nyilvános IP-cím nevű **myPip**.</span><span class="sxs-lookup"><span data-stu-id="84a3a-170">This example creates a public IP address named **myPip**.</span></span> 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="84a3a-171">Hozzon létre a hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="84a3a-171">Create the NIC.</span></span> <span data-ttu-id="84a3a-172">Ez a példa létrehoz egy hálózati adapter nevű **myNic**.</span><span class="sxs-lookup"><span data-stu-id="84a3a-172">This example creates a NIC named **myNic**.</span></span> 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-the-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="84a3a-173">A hálózati biztonsági csoport és egy RDP-szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="84a3a-173">Create the network security group and an RDP rule</span></span>
<span data-ttu-id="84a3a-174">Nem fogja tudni jelentkezzen be a virtuális gép RDP Funkciót használnak, olyan biztonsági szabályt, amely lehetővé teszi a hozzáférést RDP a 3389-es porton kell.</span><span class="sxs-lookup"><span data-stu-id="84a3a-174">To be able to log in to your VM using RDP, you need to have a security rule that allows RDP access on port 3389.</span></span> 

<span data-ttu-id="84a3a-175">Ez a példa létrehoz egy NSG nevű **myNsg** nevű szabályt tartalmaz, amelyek **myRdpRule** RDP-forgalmát, amely engedélyezi a 3389-es porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="84a3a-175">This example creates an NSG named **myNsg** that contains a rule called **myRdpRule** that allows RDP traffic over port 3389.</span></span> <span data-ttu-id="84a3a-176">Az NSG-k kapcsolatos további információkért lásd: [az Azure PowerShell használatával egy virtuális gép portok megnyitása](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="84a3a-176">For more information about NSGs, see [Opening ports to a VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


### <a name="create-a-variable-for-the-virtual-network"></a><span data-ttu-id="84a3a-177">A virtuális hálózat változó létrehozása</span><span class="sxs-lookup"><span data-stu-id="84a3a-177">Create a variable for the virtual network</span></span>
<span data-ttu-id="84a3a-178">Hozzon létre egy változót, a befejezett virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="84a3a-178">Create a variable for the completed virtual network.</span></span> 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

### <a name="create-the-vm"></a><span data-ttu-id="84a3a-179">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="84a3a-179">Create the VM</span></span>
<span data-ttu-id="84a3a-180">A következő PowerShell-parancsfájl bemutatja, hogyan állítsa be a virtuális gépek konfigurációit, és a feltöltött Virtuálisgép-lemezkép az új telepítés számára a forrásként használt.</span><span class="sxs-lookup"><span data-stu-id="84a3a-180">The following PowerShell script shows how to set up the virtual machine configurations and use the uploaded VM image as the source for the new installation.</span></span>



```powershell
# Enter a new user name and password to use as the local administrator account 
    # for remotely accessing the VM.
    $cred = Get-Credential

    # Name of the storage account where the VHD is located. This example sets the 
    # storage account name as "myStorageAccount"
    $storageAccName = "myStorageAccount"

    # Name of the virtual machine. This example sets the VM name as "myVM".
    $vmName = "myVM"

    # Size of the virtual machine. This example creates "Standard_D2_v2" sized VM. 
    # See the VM sizes documentation for more information: 
    # https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "Standard_D2_v2"

    # Computer name for the VM. This examples sets the computer name as "myComputer".
    $computerName = "myComputer"

    # Name of the disk that holds the OS. This example sets the 
    # OS disk name as "myOsDisk"
    $osDiskName = "myOsDisk"

    # Assign a SKU name. This example sets the SKU name as "Standard_LRS"
    # Valid values for -SkuName are: Standard_LRS - locally redundant storage, Standard_ZRS - zone redundant
    # storage, Standard_GRS - geo redundant storage, Standard_RAGRS - read access geo redundant storage,
    # Premium_LRS - premium locally redundant storage. 
    $skuName = "Standard_LRS"

    # Get the storage account where the uploaded image is stored
    $storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName

    # Set the VM name and size
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize

    #Set the Windows operating system configuration and add the NIC
    $vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id

    # Create the OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName

    # Configure the OS disk to be created from the existing VHD image (-CreateOption fromImage).
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows

    # Create the new VM
    New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

## <a name="verify-that-the-vm-was-created"></a><span data-ttu-id="84a3a-181">Győződjön meg arról, hogy létrejött-e a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="84a3a-181">Verify that the VM was created</span></span>
<span data-ttu-id="84a3a-182">Amikor végzett, megjelenik az újonnan létrehozott virtuális gép a [Azure-portálon](https://portal.azure.com) alatt **Tallózás** > **virtuális gépek**, vagy a következő PowerShell-parancsok használatával:</span><span class="sxs-lookup"><span data-stu-id="84a3a-182">When complete, you should see the newly created VM in the [Azure portal](https://portal.azure.com) under **Browse** > **Virtual machines**, or by using the following PowerShell commands:</span></span>

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="84a3a-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="84a3a-183">Next steps</span></span>
<span data-ttu-id="84a3a-184">Az új virtuális gépet az Azure PowerShell kezeléséhez, tekintse meg a [kezelése az Azure Resource Manager és a PowerShell használatával virtuális gépek](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="84a3a-184">To manage your new virtual machine with Azure PowerShell, see [Manage virtual machines using Azure Resource Manager and PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


