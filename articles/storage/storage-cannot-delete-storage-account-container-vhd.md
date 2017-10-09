---
title: "az Azure storage-fiókok, a tárolók és a VHD törlése a klasszikus telepítési aaaTroubleshoot |} Microsoft Docs"
description: "Az Azure storage-fiókok, a tárolók és a VHD törlése a klasszikus telepítési hibaelhárítása"
services: storage
documentationcenter: 
author: genlin
manager: felixwu
editor: tysonn
tags: storage
ms.assetid: 0f7a8243-d8dc-432a-9d37-1272a0cb3a5c
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: genli
ms.openlocfilehash: 6bbfa032e1968718c623227bb426d553e2951075
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deleting-azure-storage-accounts-containers-or-vhds-in-a-classic-deployment"></a><span data-ttu-id="d88aa-103">Az Azure storage-fiókok, a tárolók és a VHD törlése a klasszikus telepítési hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="d88aa-103">Troubleshoot deleting Azure storage accounts, containers, or VHDs in a classic deployment</span></span>
[!INCLUDE [storage-selector-cannot-delete-storage-account-container-vhd](../../includes/storage-selector-cannot-delete-storage-account-container-vhd.md)]

<span data-ttu-id="d88aa-104">Hibák akkor fordulhat elő, amikor toodelete hello Azure storage-fiók, a tároló vagy a virtuális Merevlemezt próbál hello [Azure-portálon](https://portal.azure.com/) vagy hello [a klasszikus Azure portálon](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="d88aa-104">You might receive errors when you try toodelete hello Azure storage account, container, or VHD in hello [Azure portal](https://portal.azure.com/) or hello [Azure classic portal](https://manage.windowsazure.com/).</span></span> <span data-ttu-id="d88aa-105">hello problémák hello a következő körülmények okozhatják:</span><span class="sxs-lookup"><span data-stu-id="d88aa-105">hello issues can be caused by hello following circumstances:</span></span>

* <span data-ttu-id="d88aa-106">Ha töröl egy virtuális Gépet, hello lemez és a virtuális merevlemez nem törlődnek automatikusan.</span><span class="sxs-lookup"><span data-stu-id="d88aa-106">When you delete a VM, hello disk and VHD are not automatically deleted.</span></span> <span data-ttu-id="d88aa-107">Lehet, hogy storage-fiók törlése a hiba okának hello.</span><span class="sxs-lookup"><span data-stu-id="d88aa-107">That might be hello reason for failure on storage account deletion.</span></span> <span data-ttu-id="d88aa-108">Hello lemez azt ne törölje, így hello lemez toomount másik virtuális gép is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d88aa-108">We don't delete hello disk so that you can use hello disk toomount another VM.</span></span>
* <span data-ttu-id="d88aa-109">Nincs még a címbérlet hello lemez tartozó lemez vagy hello blob.</span><span class="sxs-lookup"><span data-stu-id="d88aa-109">There is still a lease on a disk or hello blob that's associated with hello disk.</span></span>
* <span data-ttu-id="d88aa-110">Van még egy Virtuálisgép-lemezkép, amely egy blob, -tároló vagy tárolási fiókot használja.</span><span class="sxs-lookup"><span data-stu-id="d88aa-110">There is still a VM image that is using a blob, container, or storage account.</span></span>

<span data-ttu-id="d88aa-111">Ha az Azure nem problémával az ebben a cikkben, látogasson el az Azure-fórumok hello [MSDN és hello a Stack Overflow](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="d88aa-111">If your Azure issue is not addressed in this article, visit hello Azure forums on [MSDN and hello Stack Overflow](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="d88aa-112">A probléma a fórumokban beküldheti vagy too@AzureSupport a Twitteren.</span><span class="sxs-lookup"><span data-stu-id="d88aa-112">You can post your issue on these forums or too@AzureSupport on Twitter.</span></span> <span data-ttu-id="d88aa-113">Emellett fájlt a az Azure támogatási kérelmet kiválasztásával **segítségre van szüksége** a hello [az Azure támogatási](https://azure.microsoft.com/support/options/) hely.</span><span class="sxs-lookup"><span data-stu-id="d88aa-113">Also, you can file an Azure support request by selecting **Get support** on hello [Azure support](https://azure.microsoft.com/support/options/) site.</span></span>

## <a name="symptoms"></a><span data-ttu-id="d88aa-114">Probléma</span><span class="sxs-lookup"><span data-stu-id="d88aa-114">Symptoms</span></span>
<span data-ttu-id="d88aa-115">a következő szakasz hello közös azon hibákat tartalmazza, akkor fordulhat elő, amikor megpróbál toodelete hello Azure storage-fiókok, a tárolók és a VHD.</span><span class="sxs-lookup"><span data-stu-id="d88aa-115">hello following section lists common errors that you might receive when you try toodelete hello Azure storage accounts, containers, or VHDs.</span></span>

### <a name="scenario-1-unable-toodelete-a-storage-account"></a><span data-ttu-id="d88aa-116">1. eset: Nem toodelete a storage-fiók</span><span class="sxs-lookup"><span data-stu-id="d88aa-116">Scenario 1: Unable toodelete a storage account</span></span>
<span data-ttu-id="d88aa-117">Klasszikus tárfiókokat toohello hello kiválasztásakor [Azure-portálon](https://portal.azure.com/) válassza **törlése**, akadályozzák a hello tárfiók törlésének objektumok listáját típusától:</span><span class="sxs-lookup"><span data-stu-id="d88aa-117">When you navigate toohello classic storage account in hello [Azure portal](https://portal.azure.com/) and select **Delete**, you may be presented with a list of objects that are preventing deletion of hello storage account:</span></span>

  ![Hiba történt a kép Ha a hello Storage-fiók törlése](./media/storage-cannot-delete-storage-account-container-vhd/newerror.png)

<span data-ttu-id="d88aa-119">Toohello storage-fiókot hello kiválasztásakor [a klasszikus Azure portálon](https://manage.windowsazure.com/) válassza **törlése**, előfordulhat, hogy tekintse meg a következő hibák hello egyikét:</span><span class="sxs-lookup"><span data-stu-id="d88aa-119">When you navigate toohello storage account in hello [Azure classic portal](https://manage.windowsazure.com/) and select **Delete**, you might see one of hello following errors:</span></span>

- <span data-ttu-id="d88aa-120">*A tárfiók StorageAccountName VM lemezképet is tartalmaz. Győződjön meg arról, ez a tárfiók törlése előtt a Virtuálisgép-lemezképek eltávolításáról.*</span><span class="sxs-lookup"><span data-stu-id="d88aa-120">*Storage account StorageAccountName contains VM Images. Ensure these VM Images are removed before deleting this storage account.*</span></span>

- <span data-ttu-id="d88aa-121">*Nem sikerült toodelete tárfiók < vm-storage-fiók-neve >. Nem lehet toodelete tárfiók < vm-storage-fiók-neve >: "< virtuális gép-storage-fiók-neve > tárfiók rendelkezik néhány tárfiókban aktív lemezképek és/vagy lemezek. Győződjön meg arról, a lemezkép és/vagy lemezek vannak eltávolítására, mielőtt törölné a tárfiókot. ".*</span><span class="sxs-lookup"><span data-stu-id="d88aa-121">*Failed toodelete storage account <vm-storage-account-name>. Unable toodelete storage account <vm-storage-account-name>: 'Storage account <vm-storage-account-name> has some active image(s) and/or disk(s). Ensure these image(s) and/or disk(s) are removed before deleting this storage account.'.*</span></span>

- <span data-ttu-id="d88aa-122">*A tárfiók < vm-storage-fiók-neve > rendelkezik néhány tárfiókban aktív lemezképek és/vagy lemezek, pl. xxxxxxxxx-xxxxxxxxx-O-209490240936090599. Győződjön meg arról, ezek a lemezképek és/vagy lemez(ek) törlődnek, mielőtt törölné a tárfiókot.*</span><span class="sxs-lookup"><span data-stu-id="d88aa-122">*Storage account <vm-storage-account-name> has some active image(s) and/or disk(s), e.g. xxxxxxxxx- xxxxxxxxx-O-209490240936090599. Ensure these image(s) and/or disk(s) are removed before deleting this storage account.*</span></span>

- <span data-ttu-id="d88aa-123">*A tárfiók < vm-storage-fiók-neve > 1 tárolója, amely aktív lemezkép és/vagy a lemez összetevők rendelkezik. Győződjön meg arról, ez a tárfiók törlése előtt ezeket az összetevőket eltávolításuk hello lemezképtárból*.</span><span class="sxs-lookup"><span data-stu-id="d88aa-123">*Storage account <vm-storage-account-name> has 1 container(s) which have an active image and/or disk artifacts. Ensure those artifacts are removed from hello image repository before deleting this storage account*.</span></span>

- <span data-ttu-id="d88aa-124">*Küldje el a sikertelen tárfiók < vm-storage-fiók-neve > 1 tárolója, amely aktív lemezkép és/vagy a lemez összetevők rendelkezik. Gondoskodjon arról, hogy ezek az összetevők eltávolításuk hello lemezképtárból, ez a tárfiók törlése előtt. Toodelete tárfiók kísérli meg, és a vele társított még mindig aktív lemezek vannak, megjelenik egy üzenet jelenik meg törölt toobe igénylő aktív lemezek*.</span><span class="sxs-lookup"><span data-stu-id="d88aa-124">*Submit Failed Storage account <vm-storage-account-name> has 1 container(s) which have an active image and/or disk artifacts. Ensure those artifacts are removed from hello image repository before deleting this storage account. When you attempt toodelete a storage account and there are still active disks associated with it, you will see a message telling you there are active disks that need toobe deleted*.</span></span>

### <a name="scenario-2-unable-toodelete-a-container"></a><span data-ttu-id="d88aa-125">2. eset: Nem toodelete tárolója</span><span class="sxs-lookup"><span data-stu-id="d88aa-125">Scenario 2: Unable toodelete a container</span></span>
<span data-ttu-id="d88aa-126">Toodelete hello tároló meg, láthatja a következő hiba hello:</span><span class="sxs-lookup"><span data-stu-id="d88aa-126">When you try toodelete hello storage container, you might see hello following error:</span></span>

<span data-ttu-id="d88aa-127">*Nem sikerült toodelete tároló <container name>. Hiba: "jelenleg a címbérlet hello tárolóra, és nincs bérleti azonosító hello kérelemben megadott*.</span><span class="sxs-lookup"><span data-stu-id="d88aa-127">*Failed toodelete storage container <container name>. Error: 'There is currently a lease on hello container and no lease ID was specified in hello request*.</span></span>

<span data-ttu-id="d88aa-128">Vagy</span><span class="sxs-lookup"><span data-stu-id="d88aa-128">Or</span></span>

<span data-ttu-id="d88aa-129">*a következő virtuális gépek lemezeit hello ebben a tárolóban lévő blobok használ, így a hello tároló nem törölhető: VirtualMachineDiskName1, VirtualMachineDiskName2,...*</span><span class="sxs-lookup"><span data-stu-id="d88aa-129">*hello following virtual machine disks use blobs in this container, so hello container cannot be deleted: VirtualMachineDiskName1, VirtualMachineDiskName2, ...*</span></span>

### <a name="scenario-3-unable-toodelete-a-vhd"></a><span data-ttu-id="d88aa-130">3. eset: Nem toodelete virtuális merevlemez</span><span class="sxs-lookup"><span data-stu-id="d88aa-130">Scenario 3: Unable toodelete a VHD</span></span>
<span data-ttu-id="d88aa-131">Miután törli a virtuális gép, és ezután próbálja toodelete hello blobok hello tartozó virtuális merevlemezek, akkor fordulhat elő, a következő üzenet hello:</span><span class="sxs-lookup"><span data-stu-id="d88aa-131">After you delete a VM and then try toodelete hello blobs for hello associated VHDs, you might receive hello following message:</span></span>

<span data-ttu-id="d88aa-132">*Nem sikerült toodelete blob "elérési út/XXXXXX-XXXXXX-os-1447379084699.vhd'. Hiba: "jelenleg a címbérlet hello blob és hello kérelemben nincs bérleti azonosító lett megadva.*</span><span class="sxs-lookup"><span data-stu-id="d88aa-132">*Failed toodelete blob 'path/XXXXXX-XXXXXX-os-1447379084699.vhd'. Error: 'There is currently a lease on hello blob and no lease ID was specified in hello request.*</span></span>

<span data-ttu-id="d88aa-133">Vagy</span><span class="sxs-lookup"><span data-stu-id="d88aa-133">Or</span></span>

<span data-ttu-id="d88aa-134">*BLOB "BlobName.vhd" virtuálisgép-lemez "VirtualMachineDiskName" használatban van, ezért hello blob nem törölhető.*</span><span class="sxs-lookup"><span data-stu-id="d88aa-134">*Blob 'BlobName.vhd' is in use as virtual machine disk 'VirtualMachineDiskName', so hello blob cannot be deleted.*</span></span>

## <a name="solution"></a><span data-ttu-id="d88aa-135">Megoldás</span><span class="sxs-lookup"><span data-stu-id="d88aa-135">Solution</span></span>
<span data-ttu-id="d88aa-136">tooresolve hello leggyakoribb problémák, próbálja meg a következő metódus hello:</span><span class="sxs-lookup"><span data-stu-id="d88aa-136">tooresolve hello most common issues, try hello following method:</span></span>

### <a name="step-1-delete-any-disks-that-are-preventing-deletion-of-hello-storage-account-container-or-vhd"></a><span data-ttu-id="d88aa-137">1. lépés: Akadályozzák a törlés hello tárfiókot, a tároló vagy a VHD lemezek törlése</span><span class="sxs-lookup"><span data-stu-id="d88aa-137">Step 1: Delete any disks that are preventing deletion of hello storage account, container, or VHD</span></span>
1. <span data-ttu-id="d88aa-138">Váltás toohello [a klasszikus Azure portálon](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="d88aa-138">Switch toohello [Azure classic portal](https://manage.windowsazure.com/).</span></span>
2. <span data-ttu-id="d88aa-139">Válassza ki **virtuális gép** > **lemezek**.</span><span class="sxs-lookup"><span data-stu-id="d88aa-139">Select **VIRTUAL MACHINE** > **DISKS**.</span></span>

    ![A klasszikus Azure portálon található virtuális gépek lemezek képe.](./media/storage-cannot-delete-storage-account-container-vhd/VMUI.png)
3. <span data-ttu-id="d88aa-141">Keresse meg a társított hello tárfiókot, tároló vagy VHD-t, amelyet az toodelete hello lemezek.</span><span class="sxs-lookup"><span data-stu-id="d88aa-141">Locate hello disks that are associated with hello storage account, container, or VHD that you want toodelete.</span></span> <span data-ttu-id="d88aa-142">Ha engedélyezi az hello lemez hello helyét, található hello kapcsolódó tárfiók, a tároló vagy a virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="d88aa-142">When you check hello location of hello disk, you will find hello associated storage account, container, or VHD.</span></span>

    ![A klasszikus Azure portálon helyére vonatkozó információkat lemezek bemutató kép](./media/storage-cannot-delete-storage-account-container-vhd/DiskLocation.png)
4. <span data-ttu-id="d88aa-144">Hello lemezek törlése hello a következő módszerek egyikével:</span><span class="sxs-lookup"><span data-stu-id="d88aa-144">Delete hello disks by using one of hello following methods:</span></span>

  - <span data-ttu-id="d88aa-145">Ha nincs virtuális gép szerepel hello **csatlakoztatva** mező hello lemez, törölheti a hello lemez közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="d88aa-145">If  there is no VM listed on hello **Attached To** field of hello disk, you can delete hello disk directly.</span></span>

  - <span data-ttu-id="d88aa-146">Ha hello lemez adatlemez, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d88aa-146">If hello disk is a data disk, follow these steps:</span></span>

    1. <span data-ttu-id="d88aa-147">Ellenőrizze a virtuális Gépet, amely hello lemez csatolva van hello hello nevét.</span><span class="sxs-lookup"><span data-stu-id="d88aa-147">Check hello name of hello VM that hello disk is attached to.</span></span>
    2. <span data-ttu-id="d88aa-148">Nyissa meg túl**virtuális gépek** > **példányok**, majd keresse meg a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="d88aa-148">Go too**Virtual Machines** > **Instances**, and then locate hello VM.</span></span>
    3. <span data-ttu-id="d88aa-149">Győződjön meg arról, hogy semmi sem aktívan hello lemezt használ.</span><span class="sxs-lookup"><span data-stu-id="d88aa-149">Make sure that nothing is actively using hello disk.</span></span>
    4. <span data-ttu-id="d88aa-150">Válassza ki **leválasztani a lemezt** hello portál toodetach hello lemez hello alján.</span><span class="sxs-lookup"><span data-stu-id="d88aa-150">Select **Detach Disk** at hello bottom of hello portal toodetach hello disk.</span></span>
    5. <span data-ttu-id="d88aa-151">Nyissa meg túl**virtuális gépek** > **lemezek**, és várja meg, hello **csatlakoztatva** mező tooturn üres.</span><span class="sxs-lookup"><span data-stu-id="d88aa-151">Go too**Virtual Machines** > **Disks**, and wait for hello **Attached To** field tooturn blank.</span></span> <span data-ttu-id="d88aa-152">Ez azt jelzi, hogy hello lemez sikeresen le hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="d88aa-152">This indicates hello disk has successfully detached from hello VM.</span></span>
    6. <span data-ttu-id="d88aa-153">Válassza ki **törlése** hello aljához **virtuális gépek** > **lemezek** toodelete hello lemez.</span><span class="sxs-lookup"><span data-stu-id="d88aa-153">Select **Delete** at hello bottom of **Virtual Machines** > **Disks** toodelete hello disk.</span></span>

  - <span data-ttu-id="d88aa-154">Ha hello lemez operációsrendszer-lemez (hello **tartalmaz operációs rendszer** mező értéke Windows-) és a csatolt tooa VM, kövesse az alábbi lépéseket toodelete hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="d88aa-154">If hello disk is an OS disk (hello **Contains OS** field has a value like Windows) and attached tooa VM, follow these steps toodelete hello VM.</span></span> <span data-ttu-id="d88aa-155">hello operációsrendszer-lemez nem választható le, hogy toodelete hello VM toorelease hello bérleti.</span><span class="sxs-lookup"><span data-stu-id="d88aa-155">hello OS disk cannot be detached, so we have toodelete hello VM toorelease hello lease.</span></span>

    1. <span data-ttu-id="d88aa-156">Hello Névellenőrzés hello virtuális gép hello adatlemez van csatolva.</span><span class="sxs-lookup"><span data-stu-id="d88aa-156">Check hello name of hello Virtual Machine hello Data Disk is attached to.</span></span>  
    2. <span data-ttu-id="d88aa-157">Nyissa meg túl**virtuális gépek** > **példányok**, majd jelölje be hello virtuális Gépet, amely a lemez hello kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="d88aa-157">Go too**Virtual Machines** > **Instances**, and then select hello VM that hello disk is attached to.</span></span>
    3. <span data-ttu-id="d88aa-158">Győződjön meg arról, hogy semmi sem aktívan használ hello virtuális gépet, és, hogy Ön már nem kell hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="d88aa-158">Make sure that nothing is actively using hello virtual machine, and that you no longer need hello virtual machine.</span></span>
    4. <span data-ttu-id="d88aa-159">Jelölje be hello VM hello lemez csatolva van, majd válassza ki **törlése** > **Delete hello csatlakoztatott lemezekkel**.</span><span class="sxs-lookup"><span data-stu-id="d88aa-159">Select hello VM hello disk is attached to, then select **Delete** > **Delete hello attached disks**.</span></span>
    5. <span data-ttu-id="d88aa-160">Nyissa meg túl**virtuális gépek** > **lemezek**, és várja meg, hello lemez toodisappear.</span><span class="sxs-lookup"><span data-stu-id="d88aa-160">Go too**Virtual Machines** > **Disks**, and wait for hello disk toodisappear.</span></span>  <span data-ttu-id="d88aa-161">A toooccur néhány percig is eltarthat, és esetleg toorefresh hello lap.</span><span class="sxs-lookup"><span data-stu-id="d88aa-161">It may take a few minutes for this toooccur, and you may need toorefresh hello page.</span></span>
    6. <span data-ttu-id="d88aa-162">Ha hello lemez nem tűnik el, várjon, amíg hello **csatlakoztatva** mező tooturn üres.</span><span class="sxs-lookup"><span data-stu-id="d88aa-162">If hello disk does not disappear, wait for hello **Attached To** field tooturn blank.</span></span> <span data-ttu-id="d88aa-163">Ez azt jelzi, hogy hello lemez teljesen le hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="d88aa-163">This indicates hello disk has fully detached from hello VM.</span></span>  <span data-ttu-id="d88aa-164">Ezután válasszon hello lemezt, és válassza ki **törlése** hello lap toodelete hello lemez hello alján.</span><span class="sxs-lookup"><span data-stu-id="d88aa-164">Then, select hello disk, and select **Delete** at hello bottom of hello page toodelete hello disk.</span></span>


   > [!NOTE]
   > <span data-ttu-id="d88aa-165">Ha a lemez csatlakoztatott tooa VM, csak akkor tudja toodelete azt.</span><span class="sxs-lookup"><span data-stu-id="d88aa-165">If a disk is attached tooa VM, you will not be able toodelete it.</span></span> <span data-ttu-id="d88aa-166">Lemezek aszinkron módon leválasztott törölt virtuális gép alapján.</span><span class="sxs-lookup"><span data-stu-id="d88aa-166">Disks are detached from a deleted VM asynchronously.</span></span> <span data-ttu-id="d88aa-167">Ez a mező tooclear fel a hello virtuális gép törlése után néhány percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="d88aa-167">It might take a few minutes after hello VM is deleted for this field tooclear up.</span></span>
   >
   >


### <a name="step-2-delete-any-vm-images-that-are-preventing-deletion-of-hello-storage-account-or-container"></a><span data-ttu-id="d88aa-168">2. lépés: Törölje az összes virtuális gép lemezképet, amely akadályozzák a hello tárfiók és tároló törlése</span><span class="sxs-lookup"><span data-stu-id="d88aa-168">Step 2: Delete any VM Images that are preventing deletion of hello storage account or container</span></span>
1. <span data-ttu-id="d88aa-169">Váltás toohello [a klasszikus Azure portálon](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="d88aa-169">Switch toohello [Azure classic portal](https://manage.windowsazure.com/).</span></span>
2. <span data-ttu-id="d88aa-170">Válassza ki **virtuális gép** > **képek**, és törölje a társított hello tárfiókot, a tároló vagy a virtuális merevlemez képek hello.</span><span class="sxs-lookup"><span data-stu-id="d88aa-170">Select **VIRTUAL MACHINE** > **IMAGES**, and then delete hello images that are associated with hello storage account, container, or VHD.</span></span>

    <span data-ttu-id="d88aa-171">Ezt követően próbálja meg toodelete hello tárfiókot, a tároló vagy a virtuális merevlemez újra.</span><span class="sxs-lookup"><span data-stu-id="d88aa-171">After that, try toodelete hello storage account, container, or VHD again.</span></span>

> [!WARNING]
> <span data-ttu-id="d88aa-172">Bármi is meg arról, hogy tooback kívánt toosave hello fiók törlése előtt.</span><span class="sxs-lookup"><span data-stu-id="d88aa-172">Be sure tooback up anything you want toosave before you delete hello account.</span></span> <span data-ttu-id="d88aa-173">Miután törli a virtuális merevlemez, blob, tábla, várólista vagy fájlt, az véglegesen törölve lesz.</span><span class="sxs-lookup"><span data-stu-id="d88aa-173">Once you delete a VHD, blob, table, queue, or file, it is permanently deleted.</span></span> <span data-ttu-id="d88aa-174">Győződjön meg arról, hogy hello erőforrás nincs használatban.</span><span class="sxs-lookup"><span data-stu-id="d88aa-174">Ensure that hello resource is not in use.</span></span>
>
>

## <a name="about-hello-stopped-deallocated-status"></a><span data-ttu-id="d88aa-175">Hello állapot Leállítva (felszabadítva) kapcsolatban</span><span class="sxs-lookup"><span data-stu-id="d88aa-175">About hello Stopped (deallocated) status</span></span>
<span data-ttu-id="d88aa-176">Virtuális gépek hello klasszikus üzembe helyezési modellel létrehozott, és, amely megőrzi lesz hello **leállítva (felszabadítva)** vagy hello állapot [Azure-portálon](https://portal.azure.com/) vagy [a klasszikus Azure portálon ](https://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="d88aa-176">VMs that were created in hello classic deployment model and that have been retained will have hello **Stopped (deallocated)** status on either hello [Azure portal](https://portal.azure.com/) or [Azure classic portal](https://manage.windowsazure.com/).</span></span>

<span data-ttu-id="d88aa-177">**A klasszikus Azure portálon**:</span><span class="sxs-lookup"><span data-stu-id="d88aa-177">**Azure classic portal**:</span></span>

![Azure-portál virtuális gépek leállt (Deallocated) állapotát.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo2.png)

<span data-ttu-id="d88aa-179">**Azure-portálon**:</span><span class="sxs-lookup"><span data-stu-id="d88aa-179">**Azure portal**:</span></span>

![Leállított (felszabadított) állapota a virtuális gépek a klasszikus Azure portálon.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo1.png)

<span data-ttu-id="d88aa-181">Egy "Leállítva (felszabadítva)" állapot hello számítógépes erőforrások, például hello Processzor, memória és a hálózati kiadását.</span><span class="sxs-lookup"><span data-stu-id="d88aa-181">A "Stopped (deallocated)" status releases hello computer resources, such as hello CPU, memory, and network.</span></span> <span data-ttu-id="d88aa-182">hello lemezek, azonban továbbra is megmaradnak, hogy a gyors újbóli létrehozásához hello VM szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="d88aa-182">hello disks, however, are still retained so that you can quickly re-create hello VM if necessary.</span></span> <span data-ttu-id="d88aa-183">Ezek a lemezek VHD-k, az Azure storage által támogatott funkciók mellett kártevőészlelést jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="d88aa-183">These disks are created on top of VHDs, which are backed by Azure storage.</span></span> <span data-ttu-id="d88aa-184">hello tárfiók vannak a virtuális merevlemezek, és hello lemezek bérleteket kell azokat a virtuális merevlemezeken.</span><span class="sxs-lookup"><span data-stu-id="d88aa-184">hello storage account has these VHDs, and hello disks have leases on those VHDs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d88aa-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d88aa-185">Next steps</span></span>
* [<span data-ttu-id="d88aa-186">Tárfiók törlése</span><span class="sxs-lookup"><span data-stu-id="d88aa-186">Delete a storage account</span></span>](storage-create-storage-account.md#delete-a-storage-account)
