---
title: "az Azure storage-fiókok, a tárolók és a VHD törlésekor aaaTroubleshoot hibák |} Microsoft Docs"
description: "Hibaelhárítás az Azure storage-fiókok, a tárolók és a VHD törlésekor"
services: storage
documentationcenter: 
author: genlin
manager: cshepard
editor: na
tags: storage
ms.assetid: 17403aa1-fe8d-45ec-bc33-2c0b61126286
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: genli
ms.openlocfilehash: 33261562a2dd2614b35bc1118924513f8c624d6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-errors-when-you-delete-azure-storage-accounts-containers-or-vhds"></a><span data-ttu-id="0ab04-103">Hibaelhárítás az Azure storage-fiókok, a tárolók és a VHD törlésekor</span><span class="sxs-lookup"><span data-stu-id="0ab04-103">Troubleshoot errors when you delete Azure storage accounts, containers, or VHDs</span></span>

<span data-ttu-id="0ab04-104">Előfordulhat, hogy hibák merülnek fel toodelete meg egy Azure storage-fiók, a tárolót, vagy a virtuális merevlemez (VHD) a hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0ab04-104">You might receive errors when you try toodelete an Azure storage account, container, or virtual hard disk (VHD) in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="0ab04-105">Ez a cikk nyújt hibaelhárítási útmutatót toohelp resolve hello probléma Azure Resource Manager-telepítés.</span><span class="sxs-lookup"><span data-stu-id="0ab04-105">This article provides troubleshooting guidance toohelp resolve hello problem in an Azure Resource Manager deployment.</span></span>

<span data-ttu-id="0ab04-106">Ha ez a cikk nem oldja meg az Azure problémát, látogasson el az Azure-fórumok hello [MSDN és a Stack Overflow](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="0ab04-106">If this article doesn't address your Azure problem, visit hello Azure forums on [MSDN and Stack Overflow](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="0ab04-107">A probléma a fórumokban beküldheti vagy too@AzureSupport a Twitteren.</span><span class="sxs-lookup"><span data-stu-id="0ab04-107">You can post your problem on these forums or too@AzureSupport on Twitter.</span></span> <span data-ttu-id="0ab04-108">Emellett fájlt a az Azure támogatási kérelmet kiválasztásával **segítségre van szüksége** a hello [az Azure támogatási](https://azure.microsoft.com/support/options/) hely.</span><span class="sxs-lookup"><span data-stu-id="0ab04-108">Also, you can file an Azure support request by selecting **Get support** on hello [Azure support](https://azure.microsoft.com/support/options/) site.</span></span>

## <a name="symptoms"></a><span data-ttu-id="0ab04-109">Probléma</span><span class="sxs-lookup"><span data-stu-id="0ab04-109">Symptoms</span></span>
### <a name="scenario-1"></a><span data-ttu-id="0ab04-110">1. forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="0ab04-110">Scenario 1</span></span>
<span data-ttu-id="0ab04-111">Amikor toodelete egy tárfiókot a Resource Manager üzembe a virtuális merevlemez, hello a következő hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="0ab04-111">When you try toodelete a VHD in a storage account in a Resource Manager deployment, you receive hello following error message:</span></span>

<span data-ttu-id="0ab04-112">**Nem sikerült toodelete blob "vhds/BlobName.vhd". Hiba: Nincs jelenleg a címbérlet hello blob, és nincs bérleti azonosító hello kérelemben lett megadva.**</span><span class="sxs-lookup"><span data-stu-id="0ab04-112">**Failed toodelete blob 'vhds/BlobName.vhd'. Error: There is currently a lease on hello blob and no lease ID was specified in hello request.**</span></span>

<span data-ttu-id="0ab04-113">Ez a probléma akkor fordulhat elő, mert a virtuális gép (VM) rendelkezik a címbérlet hello toodelete próbált VHD.</span><span class="sxs-lookup"><span data-stu-id="0ab04-113">This problem can occur because a virtual machine (VM) has a lease on hello VHD that you are trying toodelete.</span></span>

### <a name="scenario-2"></a><span data-ttu-id="0ab04-114">2. forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="0ab04-114">Scenario 2</span></span>
<span data-ttu-id="0ab04-115">Amikor toodelete egy tárfiókot a Resource Manager üzembe egy tárolóját, hello a következő hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="0ab04-115">When you try toodelete a container in a storage account in a Resource Manager deployment, you receive hello following error message:</span></span>

<span data-ttu-id="0ab04-116">**Nem sikerült toodelete tárolási tároló "VHD". Hiba: Jelenleg a címbérlet hello tárolóra, és nincs bérleti azonosító hello kérelemben lett megadva.**</span><span class="sxs-lookup"><span data-stu-id="0ab04-116">**Failed toodelete storage container 'vhds'. Error: There is currently a lease on hello container and no lease ID was specified in hello request.**</span></span>

<span data-ttu-id="0ab04-117">Ez a probléma akkor fordulhat elő, mert hello tároló hello bérleti állapotban zárolva van egy virtuális Merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="0ab04-117">This problem can occur because hello container has a VHD that is locked in hello lease state.</span></span>

### <a name="scenario-3"></a><span data-ttu-id="0ab04-118">3. forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="0ab04-118">Scenario 3</span></span>
<span data-ttu-id="0ab04-119">Amikor toodelete egy tárfiókot a Resource Manager üzembe, hello a következő hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="0ab04-119">When you try toodelete a storage account in a Resource Manager deployment, you receive hello following error message:</span></span>

<span data-ttu-id="0ab04-120">**Nem sikerült toodelete tárfiók "StorageAccountName". Hiba: hello storage-fiók nem törölhető a miatt tooits összetevői használatban vannak.**</span><span class="sxs-lookup"><span data-stu-id="0ab04-120">**Failed toodelete storage account 'StorageAccountName'. Error: hello storage account cannot be deleted due tooits artifacts being in use.**</span></span>

<span data-ttu-id="0ab04-121">Ez a probléma akkor fordulhat elő, mert hello tárfiók hello bérleti állapotú virtuális Merevlemezt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="0ab04-121">This problem can occur because hello storage account contains a VHD that is in hello lease state.</span></span>

## <a name="solution"></a><span data-ttu-id="0ab04-122">Megoldás</span><span class="sxs-lookup"><span data-stu-id="0ab04-122">Solution</span></span> 
<span data-ttu-id="0ab04-123">tooresolve ezek a problémák, meg kell adnia hello hello hibát okozó VHD és hello kapcsolódó virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="0ab04-123">tooresolve these problems, you must identify hello VHD that is causing hello error and hello associated VM.</span></span> <span data-ttu-id="0ab04-124">Ezután válassza le a virtuális gép hello (az adatlemezek) a virtuális merevlemez hello, vagy hello (az operációsrendszer-lemezek) hello VHD-t használó virtuális gép törlése.</span><span class="sxs-lookup"><span data-stu-id="0ab04-124">Then, detach hello VHD from hello VM (for data disks) or delete hello VM that is using hello VHD (for OS disks).</span></span> <span data-ttu-id="0ab04-125">Ez hello bérleti eltávolítja hello VHD-t, és lehetővé teszi, hogy törölte toobe.</span><span class="sxs-lookup"><span data-stu-id="0ab04-125">This removes hello lease from hello VHD and allows it toobe deleted.</span></span> 

<span data-ttu-id="0ab04-126">toodo a, használja a következő módszerek hello:</span><span class="sxs-lookup"><span data-stu-id="0ab04-126">toodo this, use one of hello following methods:</span></span>

### <a name="method-1---use-azure-storage-explorer"></a><span data-ttu-id="0ab04-127">1 - metódus használata Azure Tártallózó</span><span class="sxs-lookup"><span data-stu-id="0ab04-127">Method 1 - Use Azure storage explorer</span></span>

### <a name="step-1-identify-hello-vhd-that-prevent-deletion-of-hello-storage-account"></a><span data-ttu-id="0ab04-128">1. lépés azonosítása hello hello tárfiók törlésének megakadályozása VHD-t.</span><span class="sxs-lookup"><span data-stu-id="0ab04-128">Step 1 Identify hello VHD that prevent deletion of hello storage account</span></span>

1. <span data-ttu-id="0ab04-129">Hello tárfiók törlése, kap egy üzenet párbeszédpanelen például hello következő:</span><span class="sxs-lookup"><span data-stu-id="0ab04-129">When you delete hello storage account, you will receive a message dialog such as hello following:</span></span> 

    ![jelenik meg, amikor hello tárfiók törlése](././media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. <span data-ttu-id="0ab04-131">Ellenőrizze a hello **lemez URL-cím** tooidentify hello tárfiók és a virtuális Merevlemezt, amely megakadályozza a hello hello a tárfiók törlése.</span><span class="sxs-lookup"><span data-stu-id="0ab04-131">Check hello **Disk URL** tooidentify hello storage account and hello VHD that prevents you delete hello storage account.</span></span> <span data-ttu-id="0ab04-132">A következő példa hello, hello előtt karakterlánc ". blob.core.windows.net" hello tárfiók neve, és "SCCM2012-2015-08-28.vhd" hello virtuális merevlemez nevét.</span><span class="sxs-lookup"><span data-stu-id="0ab04-132">In hello following example, hello string before “.blob.core.windows.net “ is hello storage account name, and "SCCM2012-2015-08-28.vhd" is hello VHD name.</span></span>  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

### <a name="step-2-delete-hello-vhd-by-using-azure-storage-explorer"></a><span data-ttu-id="0ab04-133">2. lépés törlése hello VHD Azure Tártallózó használatával</span><span class="sxs-lookup"><span data-stu-id="0ab04-133">Step 2 Delete hello VHD by using Azure Storage Explorer</span></span>

1. <span data-ttu-id="0ab04-134">A letöltés és telepítés hello legújabb verziójának [Azure Tártallózó](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="0ab04-134">Download and Install hello latest version of [Azure Storage Explorer](http://storageexplorer.com/).</span></span> <span data-ttu-id="0ab04-135">Ez az eszköz egy különálló alkalmazás, amely lehetővé teszi tooeasily dolgozhat Azure Storage-adatokkal Windows, a macOS és a Linux Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0ab04-135">This tool is a standalone app from Microsoft that allows you tooeasily work with Azure Storage data on Windows, macOS and Linux.</span></span>
2. <span data-ttu-id="0ab04-136">Nyissa meg az Azure Tártallózó, kiválasztása</span><span class="sxs-lookup"><span data-stu-id="0ab04-136">Open Azure Storage Explorer, select</span></span> ![fiók ikon](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/account.png) <span data-ttu-id="0ab04-138">a bal oldali sávon hello válassza ki az Azure környezetben, és jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="0ab04-138">on hello left bar, select your Azure environment, and then sign in.</span></span>

3. <span data-ttu-id="0ab04-139">Válassza ki az összes előfizetések vagy hello előfizetés, amely tartalmazza azt szeretné, hogy toodelete hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="0ab04-139">Select all subscriptions or hello subscription that contains hello storage account you want toodelete.</span></span>

    ![Előfizetés hozzáadása](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/addsub.png)

4. <span data-ttu-id="0ab04-141">Nyissa meg a Microsoft hello lemez URL-cím korábbi, jelölje be hello lekért toohello tárfiók **Blobtárolók** > **VHD-k** , és keresse meg, amely megakadályozza a virtuális merevlemez hello hello tárfiók törlése.</span><span class="sxs-lookup"><span data-stu-id="0ab04-141">Go toohello storage account that we obtained from hello disk URL earlier, select hello **Blob Containers** > **vhds** and search for hello VHD that prevents you delete hello storage account.</span></span>
5. <span data-ttu-id="0ab04-142">Ha hello VHD található, ellenőrizze a hello **nevet a virtuális gép** oszlop toofind hello virtuális Gépet, amely a VHD-t használja.</span><span class="sxs-lookup"><span data-stu-id="0ab04-142">If hello VHD is found,  check hello **VM Name** column toofind hello VM that is using this VHD.</span></span>

    ![Ellenőrizze a virtuális gép](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/check-vm.png)

6. <span data-ttu-id="0ab04-144">Távolítsa el hello bérleti hello VHD az Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="0ab04-144">Remove hello lease from hello VHD by using Azure portal.</span></span> <span data-ttu-id="0ab04-145">További információkért lásd: [eltávolítása hello bérletet hello VHD](#remove-the-lease-from-the-vhd).</span><span class="sxs-lookup"><span data-stu-id="0ab04-145">For more information, see [Remove hello lease from hello VHD](#remove-the-lease-from-the-vhd).</span></span> 

7. <span data-ttu-id="0ab04-146">Nyissa meg toohello Azure Tártallózó, kattintson a jobb gombbal a virtuális merevlemez hello, és válassza a törlés.</span><span class="sxs-lookup"><span data-stu-id="0ab04-146">Go toohello Azure Storage Explorer, right-click hello VHD and then select delete.</span></span>

8. <span data-ttu-id="0ab04-147">Hello a tárfiók törlése.</span><span class="sxs-lookup"><span data-stu-id="0ab04-147">Delete hello storage account.</span></span>

### <a name="method-2---use-azure-portal"></a><span data-ttu-id="0ab04-148">2 - módszer használata Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="0ab04-148">Method 2 - Use Azure portal</span></span> 

#### <a name="step-1-identify-hello-vhd-that-prevent-deletion-of-hello-storage-account"></a><span data-ttu-id="0ab04-149">1. lépés: Azonosítása hello hello tárfiók törlésének megakadályozása VHD-t.</span><span class="sxs-lookup"><span data-stu-id="0ab04-149">Step 1: Identify hello VHD that prevent deletion of hello storage account</span></span>

1. <span data-ttu-id="0ab04-150">Hello tárfiók törlése, kap egy üzenet párbeszédpanelen például hello következő:</span><span class="sxs-lookup"><span data-stu-id="0ab04-150">When you delete hello storage account, you will receive a message dialog such as hello following:</span></span> 

    ![jelenik meg, amikor hello tárfiók törlése](././media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. <span data-ttu-id="0ab04-152">Ellenőrizze a hello **lemez URL-cím** tooidentify hello tárfiók és a virtuális Merevlemezt, amely megakadályozza a hello hello a tárfiók törlése.</span><span class="sxs-lookup"><span data-stu-id="0ab04-152">Check hello **Disk URL** tooidentify hello storage account and hello VHD that prevents you delete hello storage account.</span></span> <span data-ttu-id="0ab04-153">A következő példa hello, hello előtt karakterlánc ". blob.core.windows.net" hello tárfiók neve, és "SCCM2012-2015-08-28.vhd" hello virtuális merevlemez nevét.</span><span class="sxs-lookup"><span data-stu-id="0ab04-153">In hello following example, hello string before “.blob.core.windows.net “ is hello storage account name, and "SCCM2012-2015-08-28.vhd" is hello VHD name.</span></span>  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

2. <span data-ttu-id="0ab04-154">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0ab04-154">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
3. <span data-ttu-id="0ab04-155">Hello központ menüben válassza ki a **összes erőforrás**.</span><span class="sxs-lookup"><span data-stu-id="0ab04-155">On hello Hub menu, select **All resources**.</span></span> <span data-ttu-id="0ab04-156">Nyissa meg a tárfiók toohello, és válassza ki **Blobok** > **VHD-k**.</span><span class="sxs-lookup"><span data-stu-id="0ab04-156">Go toohello storage account, and then select **Blobs** > **vhds**.</span></span>

    ![Képernyőfelvétel a hello portálhoz, a hello tárfiók és hello "VHD" tároló kiemelve](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/opencontainer.png)

4. <span data-ttu-id="0ab04-158">Keresse meg a virtuális Merevlemezt, amely a Microsoft hello lemez URL-címről korábban beszerzett hello.</span><span class="sxs-lookup"><span data-stu-id="0ab04-158">Locate hello VHD that we obtained from hello disk URL earlier.</span></span> <span data-ttu-id="0ab04-159">Ezt követően, hogy melyik virtuális gép által használt virtuális merevlemez hello.</span><span class="sxs-lookup"><span data-stu-id="0ab04-159">Then, determine which VM is using hello VHD.</span></span> <span data-ttu-id="0ab04-160">Általában megadhatja, hogy melyik virtuális gép rendelkezik hello VHD hello virtuális merevlemez neve ellenőrzésével:</span><span class="sxs-lookup"><span data-stu-id="0ab04-160">Usually, you can determine which VM holds hello VHD by checking name of hello VHD:</span></span>

<span data-ttu-id="0ab04-161">Virtuális gép Resource Manager fejlesztési modellben</span><span class="sxs-lookup"><span data-stu-id="0ab04-161">VM in Resource Manager development  model</span></span>

   * <span data-ttu-id="0ab04-162">Az operációs rendszer lemezek általában kövesse ezt az elnevezési konvenciót: VMName-éééé-hh-nn-HHMMSS.vhd</span><span class="sxs-lookup"><span data-stu-id="0ab04-162">OS disks generally follow this naming convention: VMName-YYYY-MM-DD-HHMMSS.vhd</span></span>
   * <span data-ttu-id="0ab04-163">Az adatlemezek általában kövesse ezt az elnevezési konvenciót: VMName-éééé-hh-nn-HHMMSS.vhd</span><span class="sxs-lookup"><span data-stu-id="0ab04-163">Data disks generally follow this naming convention: VMName-YYYY-MM-DD-HHMMSS.vhd</span></span>

<span data-ttu-id="0ab04-164">A klasszikus fejlesztési modell VM</span><span class="sxs-lookup"><span data-stu-id="0ab04-164">VM in Classic development model</span></span>

   * <span data-ttu-id="0ab04-165">Az operációs rendszer lemezek általában kövesse ezt az elnevezési konvenciót: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd</span><span class="sxs-lookup"><span data-stu-id="0ab04-165">OS disks generally follow this naming convention: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd</span></span>
   * <span data-ttu-id="0ab04-166">Az adatlemezek általában kövesse ezt az elnevezési konvenciót: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd</span><span class="sxs-lookup"><span data-stu-id="0ab04-166">Data disks generally follow this naming convention: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd</span></span>

#### <a name="step-2-remove-hello-lease-from-hello-vhd"></a><span data-ttu-id="0ab04-167">2. lépés: Virtuális merevlemez hello hello bérleti eltávolítása</span><span class="sxs-lookup"><span data-stu-id="0ab04-167">Step 2: Remove hello lease from hello VHD</span></span>

<span data-ttu-id="0ab04-168">[Hello bérleti eltávolítása hello VHD](#remove-the-lease-from-the-vhd), és törölje a hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="0ab04-168">[Remove hello lease from hello VHD](#remove-the-lease-from-the-vhd), and then delete hello storage account.</span></span>

## <a name="what-is-a-lease"></a><span data-ttu-id="0ab04-169">Mi az a címbérlet?</span><span class="sxs-lookup"><span data-stu-id="0ab04-169">What is a lease?</span></span>
<span data-ttu-id="0ab04-170">A címbérlet egy zároláshoz használt toocontrol hozzáférés tooa blob (például egy VHD-k) lehet.</span><span class="sxs-lookup"><span data-stu-id="0ab04-170">A lease is a lock that can be used toocontrol access tooa blob (for example, a VHD).</span></span> <span data-ttu-id="0ab04-171">Ha egy blobot egy bérelt, hello bérleti csak hello tulajdonosainak hello blob férhet hozzá.</span><span class="sxs-lookup"><span data-stu-id="0ab04-171">When a blob is leased, only hello owners of hello lease can access hello blob.</span></span> <span data-ttu-id="0ab04-172">A címbérlet fontos hello a következő okok miatt:</span><span class="sxs-lookup"><span data-stu-id="0ab04-172">A lease is important for hello following reasons:</span></span>

* <span data-ttu-id="0ab04-173">Megakadályozza, hogy a program sérült adatokat, ha több tulajdonosok próbálja toowrite toohello hello blob: hello azonos részében ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="0ab04-173">It prevents data corruption if multiple owners try toowrite toohello same portion of hello blob at hello same time.</span></span>
* <span data-ttu-id="0ab04-174">Megakadályozza, hogy hello blob törlődnek, ha valami aktívan használja (például virtuális gép).</span><span class="sxs-lookup"><span data-stu-id="0ab04-174">It prevents hello blob from being deleted if something is actively using it (for example, a VM).</span></span>
* <span data-ttu-id="0ab04-175">Megakadályozza, hogy hello tárfiók törlődnek, ha valami aktívan használja (például virtuális gép).</span><span class="sxs-lookup"><span data-stu-id="0ab04-175">It prevents hello storage account from being deleted if something is actively using it (for example, a VM).</span></span>

### <a name="remove-hello-lease-from-hello-vhd"></a><span data-ttu-id="0ab04-176">Virtuális merevlemez hello hello bérleti eltávolítása</span><span class="sxs-lookup"><span data-stu-id="0ab04-176">Remove hello lease from hello VHD</span></span>
<span data-ttu-id="0ab04-177">Ha hello VHD egy operációsrendszer-lemez, törölnie kell az hello VM tooremove hello bérleti:</span><span class="sxs-lookup"><span data-stu-id="0ab04-177">If hello VHD is an OS disk, you must delete hello VM tooremove hello lease:</span></span>

1. <span data-ttu-id="0ab04-178">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0ab04-178">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="0ab04-179">A hello **Hub** menü **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="0ab04-179">On hello **Hub** menu, select **Virtual Machines**.</span></span>
3. <span data-ttu-id="0ab04-180">Válassza ki, amely tárolja a címbérlet hello virtuális Merevlemezt a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="0ab04-180">Select hello VM that holds a lease on hello VHD.</span></span>
4. <span data-ttu-id="0ab04-181">Győződjön meg arról, hogy semmi sem aktívan használ hello virtuális gépet, és, hogy Ön már nem kell hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="0ab04-181">Make sure that nothing is actively using hello virtual machine, and that you no longer need hello virtual machine.</span></span>
5. <span data-ttu-id="0ab04-182">Hello hello tetején **VM részletek** panelen válassza **törlése**, és kattintson a **Igen** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="0ab04-182">At hello top of hello **VM details** blade, select **Delete**, and then click **Yes** tooconfirm.</span></span>
6. <span data-ttu-id="0ab04-183">virtuális gép hello törölni kell, de hello VHD-t meg kell őrizni.</span><span class="sxs-lookup"><span data-stu-id="0ab04-183">hello VM should be deleted, but hello VHD can be retained.</span></span> <span data-ttu-id="0ab04-184">Azonban hello virtuális merevlemez már nem kell a címbérlet rajta.</span><span class="sxs-lookup"><span data-stu-id="0ab04-184">However, hello VHD should no longer have a lease on it.</span></span> <span data-ttu-id="0ab04-185">Hello bérleti toobe kiadott néhány percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="0ab04-185">It may take a few minutes for hello lease toobe released.</span></span> <span data-ttu-id="0ab04-186">amely bérleti hello tooverify felszabadul, nyissa meg túl**összes erőforrás** > **Tárfióknév** > **Blobok**  >  **VHD-k**.</span><span class="sxs-lookup"><span data-stu-id="0ab04-186">tooverify that hello lease is released, go too**All resources** > **Storage Account Name** > **Blobs** > **vhds**.</span></span> <span data-ttu-id="0ab04-187">A hello **Blob-tulajdonságok** ablaktáblában hello **bérleti állapot** értéket kell **feloldva**.</span><span class="sxs-lookup"><span data-stu-id="0ab04-187">In hello **Blob properties** pane, hello **Lease Status** value should be **Unlocked**.</span></span>

<span data-ttu-id="0ab04-188">Ha hello VHD adatlemezt, válassza le a hello VM tooremove hello bérleti a virtuális merevlemez hello:</span><span class="sxs-lookup"><span data-stu-id="0ab04-188">If hello VHD is a data disk, detach hello VHD from hello VM tooremove hello lease:</span></span>

1. <span data-ttu-id="0ab04-189">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0ab04-189">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="0ab04-190">A hello **Hub** menü **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="0ab04-190">On hello **Hub** menu, select **Virtual Machines**.</span></span>
3. <span data-ttu-id="0ab04-191">Válassza ki, amely tárolja a címbérlet hello virtuális Merevlemezt a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="0ab04-191">Select hello VM that holds a lease on hello VHD.</span></span>
4. <span data-ttu-id="0ab04-192">Válassza ki **lemezek** a hello **VM részletek** panelen.</span><span class="sxs-lookup"><span data-stu-id="0ab04-192">Select **Disks** on hello **VM details** blade.</span></span>
5. <span data-ttu-id="0ab04-193">Válassza ki, amely tárolja a címbérlet a hello virtuális merevlemez adatlemeze hello.</span><span class="sxs-lookup"><span data-stu-id="0ab04-193">Select hello data disk that holds a lease on hello VHD.</span></span> <span data-ttu-id="0ab04-194">Megadhatja, hogy melyik virtuális merevlemez a csatolt hello lemez hello VHD hello URL-címe ellenőrzésével.</span><span class="sxs-lookup"><span data-stu-id="0ab04-194">You can determine which VHD is attached in hello disk by checking hello URL of hello VHD.</span></span>
6. <span data-ttu-id="0ab04-195">Határozza meg, hogy semmi sem aktívan használ hello adatlemez biztosan.</span><span class="sxs-lookup"><span data-stu-id="0ab04-195">Determine with certainty that nothing is actively using hello data disk.</span></span>
7. <span data-ttu-id="0ab04-196">Kattintson a **leválasztási** a hello **részletek lemez** panelen.</span><span class="sxs-lookup"><span data-stu-id="0ab04-196">Click **Detach** on hello **Disk details** blade.</span></span>
8. <span data-ttu-id="0ab04-197">hello lemez most hello virtuális gép választható le, és hello virtuális merevlemez már nem kell a címbérlet rajta.</span><span class="sxs-lookup"><span data-stu-id="0ab04-197">hello disk should now be detached from hello VM, and hello VHD should no longer have a lease on it.</span></span> <span data-ttu-id="0ab04-198">Hello bérleti toobe kiadott néhány percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="0ab04-198">It may take a few minutes for hello lease toobe released.</span></span> <span data-ttu-id="0ab04-199">amely bérleti hello tooverify kiadása, nyissa meg túl**összes erőforrás** > **Tárfióknév** > **Blobok**  >  **VHD-k**.</span><span class="sxs-lookup"><span data-stu-id="0ab04-199">tooverify that hello lease has been released, go too**All resources** > **Storage Account Name** > **Blobs** > **vhds**.</span></span> <span data-ttu-id="0ab04-200">A hello **Blob-tulajdonságok** ablaktáblában hello **bérleti állapot** értéket kell **feloldva**.</span><span class="sxs-lookup"><span data-stu-id="0ab04-200">In hello **Blob properties** pane, hello **Lease Status** value should be **Unlocked**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0ab04-201">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0ab04-201">Next steps</span></span>
* [<span data-ttu-id="0ab04-202">Tárfiók törlése</span><span class="sxs-lookup"><span data-stu-id="0ab04-202">Delete a storage account</span></span>](storage-create-storage-account.md#delete-a-storage-account)
* [<span data-ttu-id="0ab04-203">Hogyan toobreak hello zárolva van címbérlete a blob storage (PowerShell) a Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="0ab04-203">How toobreak hello locked lease of blob storage in Microsoft Azure (PowerShell)</span></span>](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
