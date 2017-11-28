---
title: "egy általánosított virtuális Gépet az Azure-ban nem felügyelt képe aaaCreate |} Microsoft Docs"
description: "Egy általános windowsos virtuális gép toouse toocreate unmanged képe több példányt hoz létre a virtuális gépek Azure-ban."
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
ms.openlocfilehash: b8a044d20313efa6dd9b9757e61154f922134476
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-an-unmanaged-vm-image-from-an-azure-vm"></a><span data-ttu-id="8dfbb-103">Hogyan toocreate egy nem felügyelt virtuális Gépre az Azure virtuális gép kép</span><span class="sxs-lookup"><span data-stu-id="8dfbb-103">How toocreate an unmanaged VM image from an Azure VM</span></span>

<span data-ttu-id="8dfbb-104">Ez a cikk ismerteti a storage-fiókok használatával.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-104">This article covers using storage accounts.</span></span> <span data-ttu-id="8dfbb-105">Javasoljuk, hogy használjon felügyelt lemezek és a felügyelt képek helyett egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-105">We recommend that you use managed disks and managed images instead of a storage account.</span></span> <span data-ttu-id="8dfbb-106">További információkért lásd: [egy általánosított virtuális Gépet az Azure-ban a felügyelt lemezképének](capture-image-resource.md).</span><span class="sxs-lookup"><span data-stu-id="8dfbb-106">For more information, see [Capture a managed image of a generalized VM in Azure](capture-image-resource.md).</span></span>

<span data-ttu-id="8dfbb-107">Ez a cikk bemutatja, hogyan toouse Azure PowerShell toocreate tárfiókot használ általánosított Azure virtuális gép lemezképét.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-107">This article shows you how toouse Azure PowerShell toocreate an image of a generalized Azure VM using a storage account.</span></span> <span data-ttu-id="8dfbb-108">Hello kép toocreate másik virtuális gép majd használni.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-108">You can then use hello image toocreate another VM.</span></span> <span data-ttu-id="8dfbb-109">hello kép hello operációsrendszer-lemez és, amelyek a virtuális géphez csatolt toohello hello adatlemezt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-109">hello image includes hello OS disk and hello data disks that are attached toohello virtual machine.</span></span> <span data-ttu-id="8dfbb-110">hello kép nem tartalmaz hello virtuális hálózati erőforrások, ezért meg kell tooset fel ezeket az erőforrásokat, amikor hoz létre új virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-110">hello image doesn't include hello virtual network resources, so you need tooset up those resources when you create hello new VM.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="8dfbb-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8dfbb-111">Prerequisites</span></span>
<span data-ttu-id="8dfbb-112">Azure PowerShell-verzió toohave kell 1.0.x vagy újabb verzióját telepítették.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-112">You need toohave Azure PowerShell version 1.0.x or newer installed.</span></span> <span data-ttu-id="8dfbb-113">Ha még nem telepítette a PowerShell, olvassa el [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) telepítési lépéseit.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-113">If you haven't already installed PowerShell, read [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for installation steps.</span></span>

## <a name="generalize-hello-vm"></a><span data-ttu-id="8dfbb-114">Virtuális gép hello generalize</span><span class="sxs-lookup"><span data-stu-id="8dfbb-114">Generalize hello VM</span></span> 
<span data-ttu-id="8dfbb-115">Ez a szakasz bemutatja, hogyan toogeneralize képként használható a Windows virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-115">This section shows you how toogeneralize your Windows virtual machine for use as an image.</span></span> <span data-ttu-id="8dfbb-116">A virtuális gépek normalizálása eltávolítja a személyes adatok, többek között, és előkészíti a hello gép toobe képként használni.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-116">Generalizing a VM removes all your personal account information, among other things, and prepares hello machine toobe used as an image.</span></span> <span data-ttu-id="8dfbb-117">A Sysprep kapcsolatos részletekért lásd: [hogyan tooUse Sysprep: Bevezetés](http://technet.microsoft.com/library/bb457073.aspx).</span><span class="sxs-lookup"><span data-stu-id="8dfbb-117">For details about Sysprep, see [How tooUse Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).</span></span>

<span data-ttu-id="8dfbb-118">Ellenőrizze, hogy fut a gépen hello hello kiszolgálói szerepkörök Sysprep által támogatott.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-118">Make sure hello server roles running on hello machine are supported by Sysprep.</span></span> <span data-ttu-id="8dfbb-119">További információkért lásd: [Sysprep támogatási kiszolgálói szerepköre tekintetében](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span><span class="sxs-lookup"><span data-stu-id="8dfbb-119">For more information, see [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8dfbb-120">Ha a virtuális merevlemez tooAzure hello az első alkalommal, győződjön meg arról, hogy [a virtuális gép előkészített](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a Sysprep futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-120">If you are uploading your VHD tooAzure for hello first time, make sure you have [prepared your VM](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before running Sysprep.</span></span> 
> 
> 

<span data-ttu-id="8dfbb-121">Linux virtuális gépet is általánosítása `sudo waagent -deprovision+user` , majd PowerShell toocapture hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-121">You can also generalize a Linux VM using `sudo waagent -deprovision+user` and then use PowerShell toocapture hello VM.</span></span> <span data-ttu-id="8dfbb-122">Hello CLI toocapture a virtuális gépek használatával kapcsolatos információkért lásd: [hogyan toogeneralize és rögzítése egy Linux virtuális gép használt hello Azure CLI ](../linux/capture-image.md).</span><span class="sxs-lookup"><span data-stu-id="8dfbb-122">For information about using hello CLI toocapture a VM, see [How toogeneralize and capture a Linux virtual machine using hello Azure CLI ](../linux/capture-image.md).</span></span>


1. <span data-ttu-id="8dfbb-123">Bejelentkezés toohello Windows rendszerű virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-123">Sign in toohello Windows virtual machine.</span></span>
2. <span data-ttu-id="8dfbb-124">Nyissa meg a hello parancssort rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-124">Open hello Command Prompt window as an administrator.</span></span> <span data-ttu-id="8dfbb-125">Hello könyvtárváltás túl**%windir%\system32\sysprep**, majd futtassa a `sysprep.exe`.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-125">Change hello directory too**%windir%\system32\sysprep**, and then run `sysprep.exe`.</span></span>
3. <span data-ttu-id="8dfbb-126">A hello **rendszer-előkészítő eszköz** párbeszédpanelen jelölje ki **adja meg a rendszer Out-of-Box élmény (OOBE)**, és győződjön meg arról, hogy hello **Generalize** jelölőnégyzet be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-126">In hello **System Preparation Tool** dialog box, select **Enter System Out-of-Box Experience (OOBE)**, and make sure that hello **Generalize** check box is selected.</span></span>
4. <span data-ttu-id="8dfbb-127">A **leállítási beállítások**, jelölje be **leállítási**.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-127">In **Shutdown Options**, select **Shutdown**.</span></span>
5. <span data-ttu-id="8dfbb-128">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-128">Click **OK**.</span></span>
   
    ![Indítsa el a Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. <span data-ttu-id="8dfbb-130">A Sysprep befejezését követően hello virtuális gép leáll.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-130">When Sysprep completes, it shuts down hello virtual machine.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="8dfbb-131">Újraindítás hello virtuális gép még nem kész feltöltése hello VHD tooAzure vagy kép hello VM történő létrehozását.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-131">Do not restart hello VM until you are done uploading hello VHD tooAzure or creating an image from hello VM.</span></span> <span data-ttu-id="8dfbb-132">Ha a virtuális gép hello véletlenül lekérdezi az újraindítás futtassa a Sysprep toogeneralize azt újra.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-132">If hello VM accidentally gets restarted, run Sysprep toogeneralize it again.</span></span>
> 
> 

## <a name="log-in-tooazure-powershell"></a><span data-ttu-id="8dfbb-133">Jelentkezzen be tooAzure PowerShell</span><span class="sxs-lookup"><span data-stu-id="8dfbb-133">Log in tooAzure PowerShell</span></span>
1. <span data-ttu-id="8dfbb-134">Nyissa meg az Azure PowerShell, és jelentkezzen be Azure-fiók tooyour.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-134">Open Azure PowerShell and sign in tooyour Azure account.</span></span>
   
    ```powershell
    Login-AzureRmAccount
    ```
   
    <span data-ttu-id="8dfbb-135">Egy előugró ablak nyílik meg tooenter az az Azure-fiók hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-135">A pop-up window opens for you tooenter your Azure account credentials.</span></span>
2. <span data-ttu-id="8dfbb-136">Az elérhető előfizetések beolvasása hello előfizetési azonosítók.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-136">Get hello subscription IDs for your available subscriptions.</span></span>
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. <span data-ttu-id="8dfbb-137">Állítsa be a megfelelő előfizetés hello hello előfizetés-azonosítójával.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-137">Set hello correct subscription using hello subscription ID.</span></span>
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="deallocate-hello-vm-and-set-hello-state-toogeneralized"></a><span data-ttu-id="8dfbb-138">Hello virtuális gép felszabadítása és hello állapot toogeneralized beállítása</span><span class="sxs-lookup"><span data-stu-id="8dfbb-138">Deallocate hello VM and set hello state toogeneralized</span></span>
1. <span data-ttu-id="8dfbb-139">Virtuálisgép-erőforrások hello felszabadítani.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-139">Deallocate hello VM resources.</span></span>
   
    ```powershell
    Stop-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```
   
    <span data-ttu-id="8dfbb-140">Hello *állapot* a virtuális gép hello a hello Azure portal-ről változik **leállítva** túl**leállítva (felszabadítva)**.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-140">hello *Status* for hello VM in hello Azure portal changes from **Stopped** too**Stopped (deallocated)**.</span></span>
2. <span data-ttu-id="8dfbb-141">Hello állapot hello virtuális gép beállítása túl**Generalized**.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-141">Set hello status of hello virtual machine too**Generalized**.</span></span> 
   
    ```powershell
    Set-AzureRmVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```
3. <span data-ttu-id="8dfbb-142">Virtuális gép hello hello állapotának ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-142">Check hello status of hello VM.</span></span> <span data-ttu-id="8dfbb-143">Hello **OSState/általánosítva** hello VM kell rendelkeznie a hello szakasz **DisplayStatus** túl beállítása**virtuális gép általánosítva**.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-143">hello **OSState/generalized** section for hello VM should have hello **DisplayStatus** set too**VM generalized**.</span></span>  
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## <a name="create-hello-image"></a><span data-ttu-id="8dfbb-144">Hello lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="8dfbb-144">Create hello image</span></span>

<span data-ttu-id="8dfbb-145">Hozzon létre egy nem felügyelt virtuálisgép-lemezkép hello cél tárolót használja a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-145">Create an unmanaged virtual machine image in hello destination storage container using this command.</span></span> <span data-ttu-id="8dfbb-146">hello lemezkép létrehozása hello azonos módon az eredeti virtuális gép hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-146">hello image is created in hello same storage account as hello original virtual machine.</span></span> <span data-ttu-id="8dfbb-147">Hello `-Path` paraméter hello forrás virtuális gép tooyour helyi számítógép hello JSON-sablon másolatának mentése.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-147">hello `-Path` parameter saves a copy of hello JSON template for hello source VM tooyour local computer.</span></span> <span data-ttu-id="8dfbb-148">Hello `-DestinationContainerName` paraméter hello tároló, amelyet toohold a képek hello neve.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-148">hello `-DestinationContainerName` parameter is hello name of hello container that you want toohold your images.</span></span> <span data-ttu-id="8dfbb-149">Ha hello tároló nem létezik, az Ön létrejön.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-149">If hello container doesn't exist, it is created for you.</span></span>
   
```powershell
Save-AzureRmVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
    -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
    -Path <C:\local\Filepath\Filename.json>
```
   
<span data-ttu-id="8dfbb-150">A kép URL-címe hello hello JSON-fájl sablon az kérheti le.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-150">You can get hello URL of your image from hello JSON file template.</span></span> <span data-ttu-id="8dfbb-151">Nyissa meg toohello **erőforrások** > **storageProfile** > **osDisk** > **kép**  >  **uri** a lemezkép szakasza hello teljes elérési útja.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-151">Go toohello **resources** > **storageProfile** > **osDisk** > **image** > **uri** section for hello complete path of your image.</span></span> <span data-ttu-id="8dfbb-152">hello hello kép URL-címe a következőhöz hasonló: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-152">hello URL of hello image looks like: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.</span></span>
   
<span data-ttu-id="8dfbb-153">Hello URI hello portálon is ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-153">You can also verify hello URI in hello portal.</span></span> <span data-ttu-id="8dfbb-154">hello kép egy nevű másolt tooa tároló **rendszer** tárfiókba.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-154">hello image is copied tooa container named **system** in your storage account.</span></span> 

## <a name="create-a-vm-from-hello-image"></a><span data-ttu-id="8dfbb-155">Hozzon létre egy virtuális Gépet hello lemezképből</span><span class="sxs-lookup"><span data-stu-id="8dfbb-155">Create a VM from hello image</span></span>

<span data-ttu-id="8dfbb-156">Egy vagy több virtuális gépek most hello nem felügyelt lemezképből hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-156">Now you can create one or more VMs from hello unmanaged image.</span></span>

### <a name="set-hello-uri-of-hello-vhd"></a><span data-ttu-id="8dfbb-157">Állítsa be a hello hello VHD URI-val</span><span class="sxs-lookup"><span data-stu-id="8dfbb-157">Set hello URI of hello VHD</span></span>

<span data-ttu-id="8dfbb-158">a virtuális merevlemez toouse hello URI hello hello formátumban kell megadni: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**.vhd.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-158">hello URI for hello VHD toouse is in hello format: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**.vhd.</span></span> <span data-ttu-id="8dfbb-159">Ebben a példában a virtuális merevlemez nevű hello **myVHD** hello tárfiók van **mystorageaccount** hello tárolóban **mycontainer**.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-159">In this example hello VHD named **myVHD** is in hello storage account **mystorageaccount** in hello container **mycontainer**.</span></span>

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


### <a name="create-a-virtual-network"></a><span data-ttu-id="8dfbb-160">Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="8dfbb-160">Create a virtual network</span></span>
<span data-ttu-id="8dfbb-161">Hozzon létre hello vNet és alhálózat a hello [virtuális hálózati](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8dfbb-161">Create hello vNet and subnet of hello [virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="8dfbb-162">Hozzon létre hello alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-162">Create hello subnet.</span></span> <span data-ttu-id="8dfbb-163">hello alábbi minta létrehoz egy nevű alhálózat **mySubnet** hello erőforráscsoportban **myResourceGroup** a címelőtagot hello **10.0.0.0/24**.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-163">hello following sample creates a subnet named **mySubnet** in hello resource group **myResourceGroup** with hello address prefix of **10.0.0.0/24**.</span></span>  
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. <span data-ttu-id="8dfbb-164">Hello virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-164">Create hello virtual network.</span></span> <span data-ttu-id="8dfbb-165">hello alábbi minta létrehoz egy virtuális hálózati nevű **myVnet** a hello **USA nyugati régiója** hellyel, amely címelőtagot hello **10.0.0.0/16**.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-165">hello following sample creates a virtual network named **myVnet** in hello **West US** location with hello address prefix of **10.0.0.0/16**.</span></span>  
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-network-interface"></a><span data-ttu-id="8dfbb-166">Hozzon létre egy nyilvános IP-cím és a hálózati kapcsolat</span><span class="sxs-lookup"><span data-stu-id="8dfbb-166">Create a public IP address and network interface</span></span>
<span data-ttu-id="8dfbb-167">hello virtuális gép virtuális hálózati hello kommunikációs tooenable, van szüksége egy [nyilvános IP-cím](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) és egy hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-167">tooenable communication with hello virtual machine in hello virtual network, you need a [public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) and a network interface.</span></span>

1. <span data-ttu-id="8dfbb-168">Hozzon létre egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-168">Create a public IP address.</span></span> <span data-ttu-id="8dfbb-169">Ez a példa létrehoz egy nyilvános IP-cím nevű **myPip**.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-169">This example creates a public IP address named **myPip**.</span></span> 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. <span data-ttu-id="8dfbb-170">Hozzon létre hello hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-170">Create hello NIC.</span></span> <span data-ttu-id="8dfbb-171">Ez a példa létrehoz egy hálózati adapter nevű **myNic**.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-171">This example creates a NIC named **myNic**.</span></span> 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-hello-network-security-group-and-an-rdp-rule"></a><span data-ttu-id="8dfbb-172">Hello hálózati biztonsági csoport és egy RDP-szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="8dfbb-172">Create hello network security group and an RDP rule</span></span>
<span data-ttu-id="8dfbb-173">toobe képes toolog tooyour a virtuális gép RDP, kell toohave egy biztonsági szabályt, amely lehetővé teszi a hozzáférést RDP a 3389-es porton.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-173">toobe able toolog in tooyour VM using RDP, you need toohave a security rule that allows RDP access on port 3389.</span></span> 

<span data-ttu-id="8dfbb-174">Ez a példa létrehoz egy NSG nevű **myNsg** nevű szabályt tartalmaz, amelyek **myRdpRule** RDP-forgalmát, amely engedélyezi a 3389-es porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-174">This example creates an NSG named **myNsg** that contains a rule called **myRdpRule** that allows RDP traffic over port 3389.</span></span> <span data-ttu-id="8dfbb-175">Az NSG-k kapcsolatos további információkért lásd: [portok tooa VM megnyitása az Azure-ban a PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8dfbb-175">For more information about NSGs, see [Opening ports tooa VM in Azure using PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


### <a name="create-a-variable-for-hello-virtual-network"></a><span data-ttu-id="8dfbb-176">Hozzon létre egy változót hello virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="8dfbb-176">Create a variable for hello virtual network</span></span>
<span data-ttu-id="8dfbb-177">Hozzon létre egy változót, virtuális hálózat hello befejeződött.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-177">Create a variable for hello completed virtual network.</span></span> 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

### <a name="create-hello-vm"></a><span data-ttu-id="8dfbb-178">Hello virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="8dfbb-178">Create hello VM</span></span>
<span data-ttu-id="8dfbb-179">hello következő PowerShell hello virtuálisgép-konfiguráció befejezése és a nem felügyelt lemezképet használja hello forrásként hello új telepítés.</span><span class="sxs-lookup"><span data-stu-id="8dfbb-179">hello following PowerShell completes hello virtual machine configurations and uses unmanaged image as hello source for hello new installation.</span></span>

</br>

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

### <a name="verify-that-hello-vm-was-created"></a><span data-ttu-id="8dfbb-180">Győződjön meg arról, hogy létrejött a virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="8dfbb-180">Verify that hello VM was created</span></span>
<span data-ttu-id="8dfbb-181">Amikor végzett, láthatja az újonnan létrehozott virtuális gép hello hello [Azure-portálon](https://portal.azure.com) alatt **Tallózás** > **virtuális gépek**, vagy a következő hello használatával PowerShell-parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="8dfbb-181">When complete, you should see hello newly created VM in hello [Azure portal](https://portal.azure.com) under **Browse** > **Virtual machines**, or by using hello following PowerShell commands:</span></span>

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a><span data-ttu-id="8dfbb-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8dfbb-182">Next steps</span></span>
<span data-ttu-id="8dfbb-183">toomanage az új virtuális gépet az Azure PowerShell, lásd: [kezelése az Azure Resource Manager és a PowerShell használatával virtuális gépek](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8dfbb-183">toomanage your new virtual machine with Azure PowerShell, see [Manage virtual machines using Azure Resource Manager and PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>


