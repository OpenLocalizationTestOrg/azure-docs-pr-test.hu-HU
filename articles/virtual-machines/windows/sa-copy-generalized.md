---
title: "Nem felügyelt lemezkép egy általánosított virtuális gép létrehozása az Azure-ban |} Microsoft Docs"
description: "Hozzon létre egy általános windowsos virtuális gép több másolatot a virtuális gépek létrehozása az Azure-ban unmanged képe."
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
ms.date: 05/23/2017
ms.author: cynthn
ms.openlocfilehash: d7f4a9558175835eba9096e6845726f21c7459d3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-create-an-unmanaged-vm-image-from-an-azure-vm"></a><span data-ttu-id="beda8-103">Egy nem felügyelt Virtuálisgép-lemezkép az Azure virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="beda8-103">How to create an unmanaged VM image from an Azure VM</span></span>

<span data-ttu-id="beda8-104">Ez a cikk ismerteti a storage-fiókok használatával.</span><span class="sxs-lookup"><span data-stu-id="beda8-104">This article covers using storage accounts.</span></span> <span data-ttu-id="beda8-105">Javasoljuk, hogy használjon felügyelt lemezek és a felügyelt képek helyett egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="beda8-105">We recommend that you use managed disks and managed images instead of a storage account.</span></span> <span data-ttu-id="beda8-106">További információkért lásd: [egy általánosított virtuális Gépet az Azure-ban a felügyelt lemezképének](capture-image-resource.md).</span><span class="sxs-lookup"><span data-stu-id="beda8-106">For more information, see [Capture a managed image of a generalized VM in Azure](capture-image-resource.md).</span></span>

<span data-ttu-id="beda8-107">Ez a cikk bemutatja, hogyan egy általánosított Azure VM-lemezkép létrehozása az Azure PowerShell használatával történő tárfiókot használ.</span><span class="sxs-lookup"><span data-stu-id="beda8-107">This article shows you how to use Azure PowerShell to create an image of a generalized Azure VM using a storage account.</span></span> <span data-ttu-id="beda8-108">A lemezkép segítségével majd hozzon létre egy másik virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="beda8-108">You can then use the image to create another VM.</span></span> <span data-ttu-id="beda8-109">A lemezkép az operációs rendszer lemezének és az adatlemezek a virtuális géphez csatolt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="beda8-109">The image includes the OS disk and the data disks that are attached to the virtual machine.</span></span> <span data-ttu-id="beda8-110">A kép nem tartalmaz virtuális hálózati erőforrások, úgy kell beállítani ezeket az erőforrásokat, ha az új virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="beda8-110">The image doesn't include the virtual network resources, so you need to set up those resources when you create the new VM.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="beda8-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="beda8-111">Prerequisites</span></span>
<span data-ttu-id="beda8-112">Szüksége van az Azure PowerShell-verzió 1.0.x vagy újabb verzióját telepítették.</span><span class="sxs-lookup"><span data-stu-id="beda8-112">You need to have Azure PowerShell version 1.0.x or newer installed.</span></span> <span data-ttu-id="beda8-113">Ha még nem telepítette a PowerShell, olvassa el [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview) telepítési lépéseit.</span><span class="sxs-lookup"><span data-stu-id="beda8-113">If you haven't already installed PowerShell, read [How to install and configure Azure PowerShell](/powershell/azure/overview) for installation steps.</span></span>

## <a name="generalize-the-vm"></a><span data-ttu-id="beda8-114">A virtuális gép generalize</span><span class="sxs-lookup"><span data-stu-id="beda8-114">Generalize the VM</span></span> 
<span data-ttu-id="beda8-115">Ez a szakasz bemutatja, hogyan általánosítja a Windows rendszerű virtuális gép képként használatra.</span><span class="sxs-lookup"><span data-stu-id="beda8-115">This section shows you how to generalize your Windows virtual machine for use as an image.</span></span> <span data-ttu-id="beda8-116">A virtuális gépek normalizálása eltávolítja a személyes adatok, többek között, és előkészíti a számítógépet, hogy képként használni.</span><span class="sxs-lookup"><span data-stu-id="beda8-116">Generalizing a VM removes all your personal account information, among other things, and prepares the machine to be used as an image.</span></span> <span data-ttu-id="beda8-117">A Sysprep kapcsolatos részletekért lásd: [hogyan használja a Sysprep: Bevezetés](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="beda8-117">For details about Sysprep, see [How to Use Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>

<span data-ttu-id="beda8-118">Győződjön meg arról, hogy a Sysprep által a gépen futó kiszolgálói szerepkörök támogatottak.</span><span class="sxs-lookup"><span data-stu-id="beda8-118">Make sure the server roles running on the machine are supported by Sysprep.</span></span> <span data-ttu-id="beda8-119">További információkért lásd: [Sysprep támogatási kiszolgálói szerepköre tekintetében](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span><span class="sxs-lookup"><span data-stu-id="beda8-119">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="beda8-120">Ha a VHD-t az Azure-bA először, győződjön meg arról, hogy [a virtuális gép előkészített](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a Sysprep futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="beda8-120">If you are uploading your VHD to Azure for the first time, make sure you have [prepared your VM](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before running Sysprep.</span></span> 
> 
> 

<span data-ttu-id="beda8-121">Linux virtuális gépet is általánosítása `sudo waagent -deprovision+user` és a PowerShell segítségével a virtuális gép rögzítése.</span><span class="sxs-lookup"><span data-stu-id="beda8-121">You can also generalize a Linux VM using `sudo waagent -deprovision+user` and then use PowerShell to capture the VM.</span></span> <span data-ttu-id="beda8-122">A virtuális gép rögzítése a parancssori felület használatával kapcsolatos információkért lásd: [generalize és az Azure parancssori felület használatával Linux virtuális gép rögzítése ](../linux/capture-image.md).</span><span class="sxs-lookup"><span data-stu-id="beda8-122">For information about using the CLI to capture a VM, see [How to generalize and capture a Linux virtual machine using the Azure CLI ](../linux/capture-image.md).</span></span>


1. <span data-ttu-id="beda8-123">Jelentkezzen be a Windows rendszerű virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="beda8-123">Sign in to the Windows virtual machine.</span></span>
2. <span data-ttu-id="beda8-124">Nyissa meg a parancssort rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="beda8-124">Open the Command Prompt window as an administrator.</span></span> <span data-ttu-id="beda8-125">Lépjen be **%windir%\system32\sysprep**, majd futtassa a `sysprep.exe`.</span><span class="sxs-lookup"><span data-stu-id="beda8-125">Change the directory to **%windir%\system32\sysprep**, and then run `sysprep.exe`.</span></span>
3. <span data-ttu-id="beda8-126">Az a **rendszer-előkészítő eszköz** párbeszédpanelen jelölje ki **adja meg a rendszer Out-of-Box élmény (OOBE)**, és győződjön meg arról, hogy a **Generalize** jelölőnégyzet be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="beda8-126">In the **System Preparation Tool** dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that the **Generalize** check box is selected.</span></span>
4. <span data-ttu-id="beda8-127">A **leállítási beállítások**, jelölje be **leállítási**.</span><span class="sxs-lookup"><span data-stu-id="beda8-127">In **Shutdown Options**, select **Shutdown**.</span></span>
5. <span data-ttu-id="beda8-128">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="beda8-128">Click **OK**.</span></span>
   
    ![Indítsa el a Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. <span data-ttu-id="beda8-130">A Sysprep befejezését követően a virtuális gép leáll.</span><span class="sxs-lookup"><span data-stu-id="beda8-130">When Sysprep completes, it shuts down the virtual machine.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="beda8-131">Ne indítsa újra a virtuális gép, amíg elkészült, a virtuális merevlemez feltöltése az Azure-bA vagy lemezkép létrehozása a virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="beda8-131">Do not restart the VM until you are done uploading the VHD to Azure or creating an image from the VM.</span></span> <span data-ttu-id="beda8-132">Ha a virtuális gép véletlenül lekérdezi újra, futtassa a Sysprep általánosítja azt újra.</span><span class="sxs-lookup"><span data-stu-id="beda8-132">If the VM accidentally gets restarted, run Sysprep to generalize it again.</span></span>
> 
> 

## <a name="log-in-to-azure-powershell"></a><span data-ttu-id="beda8-133">Jelentkezzen be az Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="beda8-133">Log in to Azure PowerShell</span></span>
1. <span data-ttu-id="beda8-134">Nyissa meg az Azure PowerShell, és jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="beda8-134">Open Azure PowerShell and sign in to your Azure account.</span></span>
   
    ```powershell
    Login-AzureRmAccount
    ```
   
    <span data-ttu-id="beda8-135">Egy előugró ablak nyílik meg Azure-fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="beda8-135">A pop-up window opens for you to enter your Azure account credentials.</span></span>
2. <span data-ttu-id="beda8-136">Az előfizetési azonosítók az elérhető előfizetések beolvasása.</span><span class="sxs-lookup"><span data-stu-id="beda8-136">Get the subscription IDs for your available subscriptions.</span></span>
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. <span data-ttu-id="beda8-137">Állítsa be a megfelelő előfizetés az előfizetés-azonosítójával.</span><span class="sxs-lookup"><span data-stu-id="beda8-137">Set the correct subscription using the subscription ID.</span></span>
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="deallocate-the-vm-and-set-the-state-to-generalized"></a><span data-ttu-id="beda8-138">A virtuális gép felszabadítása és általános állapotának beállítása</span><span class="sxs-lookup"><span data-stu-id="beda8-138">Deallocate the VM and set the state to generalized</span></span>
1. <span data-ttu-id="beda8-139">A Virtuálisgép-erőforrások felszabadítani.</span><span class="sxs-lookup"><span data-stu-id="beda8-139">Deallocate the VM resources.</span></span>
   
    ```powershell
    Stop-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```
   
    <span data-ttu-id="beda8-140">A *állapot* a virtuális gép az Azure portálon módosítja **leállítva** való **leállítva (felszabadítva)**.</span><span class="sxs-lookup"><span data-stu-id="beda8-140">The *Status* for the VM in the Azure portal changes from **Stopped** to **Stopped (deallocated)**.</span></span>
2. <span data-ttu-id="beda8-141">A virtuális gép állapotának beállítása **Generalized**.</span><span class="sxs-lookup"><span data-stu-id="beda8-141">Set the status of the virtual machine to **Generalized**.</span></span> 
   
    ```powershell
    Set-AzureRmVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```
3. <span data-ttu-id="beda8-142">A virtuális gép állapotának ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="beda8-142">Check the status of the VM.</span></span> <span data-ttu-id="beda8-143">A **OSState/általánosítva** szakaszban, a virtuális gép kell rendelkeznie a **DisplayStatus** beállítása **virtuális gép általánosítva**.</span><span class="sxs-lookup"><span data-stu-id="beda8-143">The **OSState/generalized** section for the VM should have the **DisplayStatus** set to **VM generalized**.</span></span>  
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## <a name="create-the-image"></a><span data-ttu-id="beda8-144">A lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="beda8-144">Create the image</span></span>

<span data-ttu-id="beda8-145">Hozzon létre egy nem felügyelt virtuálisgép-lemezkép a cél tárolót használja a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="beda8-145">Create an unmanaged virtual machine image in the destination storage container using this command.</span></span> <span data-ttu-id="beda8-146">A kép ugyanazt a tárfiókot az eredeti virtuális gépként jön létre.</span><span class="sxs-lookup"><span data-stu-id="beda8-146">The image is created in the same storage account as the original virtual machine.</span></span> <span data-ttu-id="beda8-147">A `-Path` paraméter a forrás virtuális gép a JSON-sablon másolatának mentése a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="beda8-147">The `-Path` parameter saves a copy of the JSON template for the source VM to your local computer.</span></span> <span data-ttu-id="beda8-148">A `-DestinationContainerName` paraméter megadása a tároló, a lemezképek tárolásához használni kívánt nevét.</span><span class="sxs-lookup"><span data-stu-id="beda8-148">The `-DestinationContainerName` parameter is the name of the container that you want to hold your images.</span></span> <span data-ttu-id="beda8-149">Ha a tároló nem létezik, az Ön létrejön.</span><span class="sxs-lookup"><span data-stu-id="beda8-149">If the container doesn't exist, it is created for you.</span></span>
   
```powershell
Save-AzureRmVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
    -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
    -Path <C:\local\Filepath\Filename.json>
```
   
<span data-ttu-id="beda8-150">A JSON-fájl sablon lekérheti a kép URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="beda8-150">You can get the URL of your image from the JSON file template.</span></span> <span data-ttu-id="beda8-151">Lépjen a **erőforrások** > **storageProfile** > **osDisk** > **kép** > **uri** a lemezkép szakasza a teljes elérési útja.</span><span class="sxs-lookup"><span data-stu-id="beda8-151">Go to the **resources** > **storageProfile** > **osDisk** > **image** > **uri** section for the complete path of your image.</span></span> <span data-ttu-id="beda8-152">A kép URL-címe a következőhöz hasonló: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.</span><span class="sxs-lookup"><span data-stu-id="beda8-152">The URL of the image looks like: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.</span></span>
   
<span data-ttu-id="beda8-153">Azt is ellenőrizheti, hogy az URI a portálon.</span><span class="sxs-lookup"><span data-stu-id="beda8-153">You can also verify the URI in the portal.</span></span> <span data-ttu-id="beda8-154">A kép másolódik nevű tárolót **rendszer** tárfiókba.</span><span class="sxs-lookup"><span data-stu-id="beda8-154">The image is copied to a container named **system** in your storage account.</span></span> 

## <a name="create-a-vm-from-the-image"></a><span data-ttu-id="beda8-155">Hozzon létre egy virtuális Gépet a lemezképből</span><span class="sxs-lookup"><span data-stu-id="beda8-155">Create a VM from the image</span></span>

<span data-ttu-id="beda8-156">Most egy vagy több virtuális gépek is létrehozhat a nem felügyelt lemezképből.</span><span class="sxs-lookup"><span data-stu-id="beda8-156">Now you can create one or more VMs from the unmanaged image.</span></span>

### <a name="set-the-uri-of-the-vhd"></a><span data-ttu-id="beda8-157">A virtuális merevlemez URI beállítása</span><span class="sxs-lookup"><span data-stu-id="beda8-157">Set the URI of the VHD</span></span>

<span data-ttu-id="beda8-158">URI-JÁNAK a VHD-fájlt használjon a formátumban kell megadni: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**.vhd.</span><span class="sxs-lookup"><span data-stu-id="beda8-158">The URI for the VHD to use is in the format: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**.vhd.</span></span> <span data-ttu-id="beda8-159">Ebben a példában a virtuális merevlemez nevű **myVHD** szerepel a tárfiók **mystorageaccount** tárolóban **mycontainer**.</span><span class="sxs-lookup"><span data-stu-id="beda8-159">In this example the VHD named **myVHD** is in the storage account **mystorageaccount** in the container **mycontainer**.</span></span>

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


### <a name="create-a-virtual-network"></a><span data-ttu-id="beda8-160">Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="beda8-160">Create a virtual network</span></span>
<span data-ttu-id="beda8-161">Hozza létre a virtuális hálózat és az alhálózati a [virtuális hálózati](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="beda8-161">Create the vNet and subnet of the [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="beda8-162">Hozza létre az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="beda8-162">Create the subnet.</span></span> <span data-ttu-id="beda8-163">Az alábbi minta létrehoz egy nevű alhálózat **mySubnet** erőforráscsoportban **myResourceGroup** a címelőtagot rendelkező **10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="beda8-163">The following sample creates a subnet named **mySubnet** in the resource group **myResourceGroup** with the address prefix of **10.0.0.0/24**.</span></span>  
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="beda8-164">Hozza létre a virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="beda8-164">Create the virtual network.</span></span> <span data-ttu-id="beda8-165">Az alábbi minta létrehoz egy virtuális hálózati nevű **myVnet** a a **USA nyugati régiója** hellyel, amely a címelőtagot **10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="beda8-165">The following sample creates a virtual network named **myVnet** in the **West US** location with the address prefix of **10.0.0.0/16**.</span></span>  
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-network-interface"></a><span data-ttu-id="beda8-166">Hozzon létre egy nyilvános IP-cím és a hálózati kapcsolat</span><span class="sxs-lookup"><span data-stu-id="beda8-166">Create a public IP address and network interface</span></span>
<span data-ttu-id="beda8-167">A virtuális hálózaton a virtuális géppel való kommunikáció biztosításához egy [nyilvános IP-cím](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) és egy hálózati adapter szükséges.</span><span class="sxs-lookup"><span data-stu-id="beda8-167">To enable communication with the virtual machine in the virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="beda8-168">Hozzon létre egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="beda8-168">Create a public IP address.</span></span> <span data-ttu-id="beda8-169">Ez a példa létrehoz egy nyilvános IP-cím nevű **myPip**.</span><span class="sxs-lookup"><span data-stu-id="beda8-169">This example creates a public IP address named **myPip**.</span></span> 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="beda8-170">Hozzon létre a hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="beda8-170">Create the NIC.</span></span> <span data-ttu-id="beda8-171">Ez a példa létrehoz egy hálózati adapter nevű **myNic**.</span><span class="sxs-lookup"><span data-stu-id="beda8-171">This example creates a NIC named **myNic**.</span></span> 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-the-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="beda8-172">A hálózati biztonsági csoport és egy RDP-szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="beda8-172">Create the network security group and an RDP rule</span></span>
<span data-ttu-id="beda8-173">Nem fogja tudni jelentkezzen be a virtuális gép RDP Funkciót használnak, olyan biztonsági szabályt, amely lehetővé teszi a hozzáférést RDP a 3389-es porton kell.</span><span class="sxs-lookup"><span data-stu-id="beda8-173">To be able to log in to your VM using RDP, you need to have a security rule that allows RDP access on port 3389.</span></span> 

<span data-ttu-id="beda8-174">Ez a példa létrehoz egy NSG nevű **myNsg** nevű szabályt tartalmaz, amelyek **myRdpRule** RDP-forgalmát, amely engedélyezi a 3389-es porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="beda8-174">This example creates an NSG named **myNsg** that contains a rule called **myRdpRule** that allows RDP traffic over port 3389.</span></span> <span data-ttu-id="beda8-175">Az NSG-k kapcsolatos további információkért lásd: [az Azure PowerShell használatával egy virtuális gép portok megnyitása](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="beda8-175">For more information about NSGs, see [Opening ports to a VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


### <a name="create-a-variable-for-the-virtual-network"></a><span data-ttu-id="beda8-176">A virtuális hálózat változó létrehozása</span><span class="sxs-lookup"><span data-stu-id="beda8-176">Create a variable for the virtual network</span></span>
<span data-ttu-id="beda8-177">Hozzon létre egy változót, a befejezett virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="beda8-177">Create a variable for the completed virtual network.</span></span> 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

### <a name="create-the-vm"></a><span data-ttu-id="beda8-178">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="beda8-178">Create the VM</span></span>
<span data-ttu-id="beda8-179">A következő PowerShell befejeződött a virtuális gépek konfigurációit, és nem felügyelt lemezképet használja forrásként az új telepítés.</span><span class="sxs-lookup"><span data-stu-id="beda8-179">The following PowerShell completes the virtual machine configurations and uses unmanaged image as the source for the new installation.</span></span>

</br>

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

### <a name="verify-that-the-vm-was-created"></a><span data-ttu-id="beda8-180">Győződjön meg arról, hogy létrejött-e a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="beda8-180">Verify that the VM was created</span></span>
<span data-ttu-id="beda8-181">Amikor végzett, megjelenik az újonnan létrehozott virtuális gép a [Azure-portálon](https://portal.azure.com) alatt **Tallózás** > **virtuális gépek**, vagy a következő PowerShell-parancsok használatával:</span><span class="sxs-lookup"><span data-stu-id="beda8-181">When complete, you should see the newly created VM in the [Azure portal](https://portal.azure.com) under **Browse** > **Virtual machines**, or by using the following PowerShell commands:</span></span>

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="beda8-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="beda8-182">Next steps</span></span>
<span data-ttu-id="beda8-183">Az új virtuális gépet az Azure PowerShell kezeléséhez, tekintse meg a [kezelése az Azure Resource Manager és a PowerShell használatával virtuális gépek](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="beda8-183">To manage your new virtual machine with Azure PowerShell, see [Manage virtual machines using Azure Resource Manager and PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


