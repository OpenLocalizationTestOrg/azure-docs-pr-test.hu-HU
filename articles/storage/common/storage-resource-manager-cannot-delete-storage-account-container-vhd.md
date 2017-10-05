---
title: "Az Azure storage-fiókok, a tárolók és a VHD törlésekor hibák elhárítása |} Microsoft Docs"
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
ms.openlocfilehash: 11944dd38b1cc30106c0b76a108480c018ca39d4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshoot-errors-when-you-delete-azure-storage-accounts-containers-or-vhds"></a><span data-ttu-id="6de1a-103">Hibaelhárítás az Azure storage-fiókok, a tárolók és a VHD törlésekor</span><span class="sxs-lookup"><span data-stu-id="6de1a-103">Troubleshoot errors when you delete Azure storage accounts, containers, or VHDs</span></span>

<span data-ttu-id="6de1a-104">Előfordulhat, hogy hibák merülnek fel egy Azure storage-fiók, a tárolót, vagy a virtuális merevlemez (VHD) törlése közben a a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6de1a-104">You might receive errors when you try to delete an Azure storage account, container, or virtual hard disk (VHD) in the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="6de1a-105">Ez a cikk nyújt hibaelhárítási útmutatót Azure Resource Manager-telepítés a probléma megoldása érdekében.</span><span class="sxs-lookup"><span data-stu-id="6de1a-105">This article provides troubleshooting guidance to help resolve the problem in an Azure Resource Manager deployment.</span></span>

<span data-ttu-id="6de1a-106">Ha ez a cikk nem oldja meg az Azure problémát, látogasson el az Azure-fórumok a [MSDN és a Stack Overflow](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="6de1a-106">If this article doesn't address your Azure problem, visit the Azure forums on [MSDN and Stack Overflow](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="6de1a-107">A probléma, beküldheti, ezek fórumokban, vagy @AzureSupport a Twitteren.</span><span class="sxs-lookup"><span data-stu-id="6de1a-107">You can post your problem on these forums or to @AzureSupport on Twitter.</span></span> <span data-ttu-id="6de1a-108">Emellett fájlt a az Azure támogatási kérelmet kiválasztásával **segítségre van szüksége** a a [az Azure támogatási](https://azure.microsoft.com/support/options/) hely.</span><span class="sxs-lookup"><span data-stu-id="6de1a-108">Also, you can file an Azure support request by selecting **Get support** on the [Azure support](https://azure.microsoft.com/support/options/) site.</span></span>

## <a name="symptoms"></a><span data-ttu-id="6de1a-109">Probléma</span><span class="sxs-lookup"><span data-stu-id="6de1a-109">Symptoms</span></span>
### <a name="scenario-1"></a><span data-ttu-id="6de1a-110">1. forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="6de1a-110">Scenario 1</span></span>
<span data-ttu-id="6de1a-111">Egy tárfiókot a Resource Manager üzembe a virtuális merevlemez törlését kísérli meg, amikor a következő hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="6de1a-111">When you try to delete a VHD in a storage account in a Resource Manager deployment, you receive the following error message:</span></span>

<span data-ttu-id="6de1a-112">**Nem sikerült törölni a blobot "vhds/BlobName.vhd". Hiba: Jelenleg a címbérlet blobot, és a kérelemben nincs bérleti azonosító lett megadva.**</span><span class="sxs-lookup"><span data-stu-id="6de1a-112">**Failed to delete blob 'vhds/BlobName.vhd'. Error: There is currently a lease on the blob and no lease ID was specified in the request.**</span></span>

<span data-ttu-id="6de1a-113">Ez a probléma akkor fordulhat elő, mert a virtuális gép (VM) a címbérlet a törölni kívánt virtuális.</span><span class="sxs-lookup"><span data-stu-id="6de1a-113">This problem can occur because a virtual machine (VM) has a lease on the VHD that you are trying to delete.</span></span>

### <a name="scenario-2"></a><span data-ttu-id="6de1a-114">2. forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="6de1a-114">Scenario 2</span></span>
<span data-ttu-id="6de1a-115">Amikor megpróbálja törölni egy tárfiókot a Resource Manager üzembe egy tárolóját, a következő hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="6de1a-115">When you try to delete a container in a storage account in a Resource Manager deployment, you receive the following error message:</span></span>

<span data-ttu-id="6de1a-116">**Nem sikerült törölni a tárolási tároló "VHD". Hiba: Jelenleg a címbérlet a tárolóra, és a kérelemben nincs bérleti azonosító lett megadva.**</span><span class="sxs-lookup"><span data-stu-id="6de1a-116">**Failed to delete storage container 'vhds'. Error: There is currently a lease on the container and no lease ID was specified in the request.**</span></span>

<span data-ttu-id="6de1a-117">Ez a probléma akkor fordulhat elő, mert a tároló virtuális Merevlemezt, amely a címbérlet állapotban zárolva van.</span><span class="sxs-lookup"><span data-stu-id="6de1a-117">This problem can occur because the container has a VHD that is locked in the lease state.</span></span>

### <a name="scenario-3"></a><span data-ttu-id="6de1a-118">3. forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="6de1a-118">Scenario 3</span></span>
<span data-ttu-id="6de1a-119">A Resource Manager üzembe tárfiók törlése megkísérlésekor a következő hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="6de1a-119">When you try to delete a storage account in a Resource Manager deployment, you receive the following error message:</span></span>

<span data-ttu-id="6de1a-120">**Nem sikerült törölni a tárfiókot "StorageAccountName". Hiba: A tárfiók nem törölhető a mert az összetevői használatban vannak.**</span><span class="sxs-lookup"><span data-stu-id="6de1a-120">**Failed to delete storage account 'StorageAccountName'. Error: The storage account cannot be deleted due to its artifacts being in use.**</span></span>

<span data-ttu-id="6de1a-121">Ez a probléma akkor fordulhat elő, mert a tárfiók tartalmaz virtuális Merevlemezt, amely a címbérlet állapotban van.</span><span class="sxs-lookup"><span data-stu-id="6de1a-121">This problem can occur because the storage account contains a VHD that is in the lease state.</span></span>

## <a name="solution"></a><span data-ttu-id="6de1a-122">Megoldás</span><span class="sxs-lookup"><span data-stu-id="6de1a-122">Solution</span></span> 
<span data-ttu-id="6de1a-123">Ezek a problémák megoldása érdekében meg kell adnia a VHD-t, a hibát okozó és a társított virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="6de1a-123">To resolve these problems, you must identify the VHD that is causing the error and the associated VM.</span></span> <span data-ttu-id="6de1a-124">Ezután válassza le a virtuális Merevlemezt a virtuális gép (az adatlemezek), vagy törölje a virtuális Gépet, amely a VHD-t használja (az operációsrendszer-lemezek).</span><span class="sxs-lookup"><span data-stu-id="6de1a-124">Then, detach the VHD from the VM (for data disks) or delete the VM that is using the VHD (for OS disks).</span></span> <span data-ttu-id="6de1a-125">Ez a címbérlet eltávolítja a VHD-t, és lehetővé teszi, hogy törölhető.</span><span class="sxs-lookup"><span data-stu-id="6de1a-125">This removes the lease from the VHD and allows it to be deleted.</span></span> 

<span data-ttu-id="6de1a-126">Ehhez használja a következő módszerek egyikét:</span><span class="sxs-lookup"><span data-stu-id="6de1a-126">To do this, use one of the following methods:</span></span>

### <a name="method-1---use-azure-storage-explorer"></a><span data-ttu-id="6de1a-127">1 - metódus használata Azure Tártallózó</span><span class="sxs-lookup"><span data-stu-id="6de1a-127">Method 1 - Use Azure storage explorer</span></span>

### <a name="step-1-identify-the-vhd-that-prevent-deletion-of-the-storage-account"></a><span data-ttu-id="6de1a-128">1. lépés azonosítása a virtuális Merevlemezt, amely a tárfiók törlésének megakadályozása</span><span class="sxs-lookup"><span data-stu-id="6de1a-128">Step 1 Identify the VHD that prevent deletion of the storage account</span></span>

1. <span data-ttu-id="6de1a-129">Ha törli a tárfiókot, kapni fog egy üzenet párbeszédpanelen a következő:</span><span class="sxs-lookup"><span data-stu-id="6de1a-129">When you delete the storage account, you will receive a message dialog such as the following:</span></span> 

    ![jelenik meg, amikor a tárfiók törlése](././media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. <span data-ttu-id="6de1a-131">Ellenőrizze a **lemez URL-cím** azonosítására a tárolási fiók és a virtuális Merevlemezt, amely megakadályozza, hogy a tárfiók törlése.</span><span class="sxs-lookup"><span data-stu-id="6de1a-131">Check the **Disk URL** to identify the storage account and the VHD that prevents you delete the storage account.</span></span> <span data-ttu-id="6de1a-132">A következő példában a karakterlánc előtt ". blob.core.windows.net" a tárfiók nevét, és "SCCM2012-2015-08-28.vhd" a virtuális merevlemez nevét.</span><span class="sxs-lookup"><span data-stu-id="6de1a-132">In the following example, the string before “.blob.core.windows.net “ is the storage account name, and "SCCM2012-2015-08-28.vhd" is the VHD name.</span></span>  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

### <a name="step-2-delete-the-vhd-by-using-azure-storage-explorer"></a><span data-ttu-id="6de1a-133">2. lépés törlése a virtuális merevlemez Azure Tártallózó használatával</span><span class="sxs-lookup"><span data-stu-id="6de1a-133">Step 2 Delete the VHD by using Azure Storage Explorer</span></span>

1. <span data-ttu-id="6de1a-134">Töltse le és telepítse a legújabb [Azure Tártallózó](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="6de1a-134">Download and Install the latest version of [Azure Storage Explorer](http://storageexplorer.com/).</span></span> <span data-ttu-id="6de1a-135">Ez az eszköz egy különálló alkalmazás, amely lehetővé teszi, hogy egyszerűen dolgozhat Azure Storage-adatokkal Windows, a macOS és a Linux Microsoft.</span><span class="sxs-lookup"><span data-stu-id="6de1a-135">This tool is a standalone app from Microsoft that allows you to easily work with Azure Storage data on Windows, macOS and Linux.</span></span>
2. <span data-ttu-id="6de1a-136">Nyissa meg az Azure Tártallózó, kiválasztása</span><span class="sxs-lookup"><span data-stu-id="6de1a-136">Open Azure Storage Explorer, select</span></span> ![fiók ikon](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/account.png) <span data-ttu-id="6de1a-138">a bal oldali sávon válassza ki az Azure környezetben, és jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="6de1a-138">on the left bar, select your Azure environment, and then sign in.</span></span>

3. <span data-ttu-id="6de1a-139">Válassza ki az összes előfizetést, vagy törölni szeretné az előfizetést, amely tartalmazza a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="6de1a-139">Select all subscriptions or the subscription that contains the storage account you want to delete.</span></span>

    ![Előfizetés hozzáadása](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/addsub.png)

4. <span data-ttu-id="6de1a-141">Lépjen a tárfiókhoz, hogy azt a lemez URL-cím, korábbi, jelölje be a **Blobtárolók** > **VHD-k** , és keresse meg a virtuális Merevlemezt, amely megakadályozza, hogy a tárfiók törlése.</span><span class="sxs-lookup"><span data-stu-id="6de1a-141">Go to the storage account that we obtained from the disk URL earlier, select the **Blob Containers** > **vhds** and search for the VHD that prevents you delete the storage account.</span></span>
5. <span data-ttu-id="6de1a-142">Ha a virtuális Merevlemezen található, ellenőrizze a **nevet a virtuális gép** oszlopban keresse meg a virtuális Gépet, amely a VHD-t használja.</span><span class="sxs-lookup"><span data-stu-id="6de1a-142">If the VHD is found,  check the **VM Name** column to find the VM that is using this VHD.</span></span>

    ![Ellenőrizze a virtuális gép](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/check-vm.png)

6. <span data-ttu-id="6de1a-144">Távolítsa el a címbérlet a VHD-t az Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="6de1a-144">Remove the lease from the VHD by using Azure portal.</span></span> <span data-ttu-id="6de1a-145">További információkért lásd: [távolítsa el a virtuális Merevlemezt a címbérlet](#remove-the-lease-from-the-vhd).</span><span class="sxs-lookup"><span data-stu-id="6de1a-145">For more information, see [Remove the lease from the VHD](#remove-the-lease-from-the-vhd).</span></span> 

7. <span data-ttu-id="6de1a-146">Ugrás az Azure Tártallózó, kattintson a jobb gombbal a virtuális Merevlemezt, és válassza a törlés.</span><span class="sxs-lookup"><span data-stu-id="6de1a-146">Go to the Azure Storage Explorer, right-click the VHD and then select delete.</span></span>

8. <span data-ttu-id="6de1a-147">Tárfiók törlése.</span><span class="sxs-lookup"><span data-stu-id="6de1a-147">Delete the storage account.</span></span>

### <a name="method-2---use-azure-portal"></a><span data-ttu-id="6de1a-148">2 - módszer használata Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="6de1a-148">Method 2 - Use Azure portal</span></span> 

#### <a name="step-1-identify-the-vhd-that-prevent-deletion-of-the-storage-account"></a><span data-ttu-id="6de1a-149">1. lépés: A virtuális Merevlemezt, amely a tárfiók törlésének megakadályozása azonosítása</span><span class="sxs-lookup"><span data-stu-id="6de1a-149">Step 1: Identify the VHD that prevent deletion of the storage account</span></span>

1. <span data-ttu-id="6de1a-150">Ha törli a tárfiókot, kapni fog egy üzenet párbeszédpanelen a következő:</span><span class="sxs-lookup"><span data-stu-id="6de1a-150">When you delete the storage account, you will receive a message dialog such as the following:</span></span> 

    ![jelenik meg, amikor a tárfiók törlése](././media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. <span data-ttu-id="6de1a-152">Ellenőrizze a **lemez URL-cím** azonosítására a tárolási fiók és a virtuális Merevlemezt, amely megakadályozza, hogy a tárfiók törlése.</span><span class="sxs-lookup"><span data-stu-id="6de1a-152">Check the **Disk URL** to identify the storage account and the VHD that prevents you delete the storage account.</span></span> <span data-ttu-id="6de1a-153">A következő példában a karakterlánc előtt ". blob.core.windows.net" a tárfiók nevét, és "SCCM2012-2015-08-28.vhd" a virtuális merevlemez nevét.</span><span class="sxs-lookup"><span data-stu-id="6de1a-153">In the following example, the string before “.blob.core.windows.net “ is the storage account name, and "SCCM2012-2015-08-28.vhd" is the VHD name.</span></span>  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

2. <span data-ttu-id="6de1a-154">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6de1a-154">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
3. <span data-ttu-id="6de1a-155">A központ menüben válassza ki a **összes erőforrás**.</span><span class="sxs-lookup"><span data-stu-id="6de1a-155">On the Hub menu, select **All resources**.</span></span> <span data-ttu-id="6de1a-156">Nyissa meg a tárfiók, és válassza ki **Blobok** > **VHD-k**.</span><span class="sxs-lookup"><span data-stu-id="6de1a-156">Go to the storage account, and then select **Blobs** > **vhds**.</span></span>

    ![Képernyőkép a tárfiók és a kiemelt "VHD" tárolóban, a portál](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/opencontainer.png)

4. <span data-ttu-id="6de1a-158">Keresse meg a virtuális Merevlemezt, amely azt a lemez URL-címről korábban beszerzett.</span><span class="sxs-lookup"><span data-stu-id="6de1a-158">Locate the VHD that we obtained from the disk URL earlier.</span></span> <span data-ttu-id="6de1a-159">Ezt követően, hogy melyik virtuális gép használ a VHD-t.</span><span class="sxs-lookup"><span data-stu-id="6de1a-159">Then, determine which VM is using the VHD.</span></span> <span data-ttu-id="6de1a-160">Általában megadhatja, hogy melyik virtuális gép, amely tárolja a virtuális merevlemez a VHD-fájl nevének ellenőrzésével:</span><span class="sxs-lookup"><span data-stu-id="6de1a-160">Usually, you can determine which VM holds the VHD by checking name of the VHD:</span></span>

<span data-ttu-id="6de1a-161">Virtuális gép Resource Manager fejlesztési modellben</span><span class="sxs-lookup"><span data-stu-id="6de1a-161">VM in Resource Manager development  model</span></span>

   * <span data-ttu-id="6de1a-162">Az operációs rendszer lemezek általában kövesse ezt az elnevezési konvenciót: VMName-éééé-hh-nn-HHMMSS.vhd</span><span class="sxs-lookup"><span data-stu-id="6de1a-162">OS disks generally follow this naming convention: VMName-YYYY-MM-DD-HHMMSS.vhd</span></span>
   * <span data-ttu-id="6de1a-163">Az adatlemezek általában kövesse ezt az elnevezési konvenciót: VMName-éééé-hh-nn-HHMMSS.vhd</span><span class="sxs-lookup"><span data-stu-id="6de1a-163">Data disks generally follow this naming convention: VMName-YYYY-MM-DD-HHMMSS.vhd</span></span>

<span data-ttu-id="6de1a-164">A klasszikus fejlesztési modell VM</span><span class="sxs-lookup"><span data-stu-id="6de1a-164">VM in Classic development model</span></span>

   * <span data-ttu-id="6de1a-165">Az operációs rendszer lemezek általában kövesse ezt az elnevezési konvenciót: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd</span><span class="sxs-lookup"><span data-stu-id="6de1a-165">OS disks generally follow this naming convention: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd</span></span>
   * <span data-ttu-id="6de1a-166">Az adatlemezek általában kövesse ezt az elnevezési konvenciót: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd</span><span class="sxs-lookup"><span data-stu-id="6de1a-166">Data disks generally follow this naming convention: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd</span></span>

#### <a name="step-2-remove-the-lease-from-the-vhd"></a><span data-ttu-id="6de1a-167">2. lépés: A címbérlet távolítsa el a virtuális merevlemez</span><span class="sxs-lookup"><span data-stu-id="6de1a-167">Step 2: Remove the lease from the VHD</span></span>

<span data-ttu-id="6de1a-168">[Távolítsa el a virtuális Merevlemezt a címbérlet](#remove-the-lease-from-the-vhd), majd törli a tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="6de1a-168">[Remove the lease from the VHD](#remove-the-lease-from-the-vhd), and then delete the storage account.</span></span>

## <a name="what-is-a-lease"></a><span data-ttu-id="6de1a-169">Mi az a címbérlet?</span><span class="sxs-lookup"><span data-stu-id="6de1a-169">What is a lease?</span></span>
<span data-ttu-id="6de1a-170">A címbérlet egy zároláshoz használható (például egy VHD-k) blob való hozzáférés vezérlése érdekében.</span><span class="sxs-lookup"><span data-stu-id="6de1a-170">A lease is a lock that can be used to control access to a blob (for example, a VHD).</span></span> <span data-ttu-id="6de1a-171">Ha egy blobot egy bérelt, csak a bérlet tulajdonosai férhet hozzá a blob.</span><span class="sxs-lookup"><span data-stu-id="6de1a-171">When a blob is leased, only the owners of the lease can access the blob.</span></span> <span data-ttu-id="6de1a-172">A címbérlet fontos a következők lehetnek az okai:</span><span class="sxs-lookup"><span data-stu-id="6de1a-172">A lease is important for the following reasons:</span></span>

* <span data-ttu-id="6de1a-173">Ha több tulajdonosok próbálja írni egy időben, a blob azonos részéhez megakadályozza, hogy sérült adatokat.</span><span class="sxs-lookup"><span data-stu-id="6de1a-173">It prevents data corruption if multiple owners try to write to the same portion of the blob at the same time.</span></span>
* <span data-ttu-id="6de1a-174">Megakadályozza, hogy a blob törlődnek, ha valami aktívan használja (például virtuális gép).</span><span class="sxs-lookup"><span data-stu-id="6de1a-174">It prevents the blob from being deleted if something is actively using it (for example, a VM).</span></span>
* <span data-ttu-id="6de1a-175">Megakadályozza, hogy a tárfiók törlődnek, ha valami aktívan használja (például virtuális gép).</span><span class="sxs-lookup"><span data-stu-id="6de1a-175">It prevents the storage account from being deleted if something is actively using it (for example, a VM).</span></span>

### <a name="remove-the-lease-from-the-vhd"></a><span data-ttu-id="6de1a-176">A bérlet eltávolítása a virtuális merevlemez</span><span class="sxs-lookup"><span data-stu-id="6de1a-176">Remove the lease from the VHD</span></span>
<span data-ttu-id="6de1a-177">Ha a virtuális Merevlemezt egy operációsrendszer-lemez, törölnie kell a virtuális gép eltávolítása a címbérlet:</span><span class="sxs-lookup"><span data-stu-id="6de1a-177">If the VHD is an OS disk, you must delete the VM to remove the lease:</span></span>

1. <span data-ttu-id="6de1a-178">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6de1a-178">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="6de1a-179">Az a **Hub** menü **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="6de1a-179">On the **Hub** menu, select **Virtual Machines**.</span></span>
3. <span data-ttu-id="6de1a-180">Válassza ki a virtuális Gépet, amely tárolja a címbérlet a virtuális merevlemezen.</span><span class="sxs-lookup"><span data-stu-id="6de1a-180">Select the VM that holds a lease on the VHD.</span></span>
4. <span data-ttu-id="6de1a-181">Győződjön meg arról, hogy semmi sem aktívan használ a virtuális gép és a virtuális gép már nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="6de1a-181">Make sure that nothing is actively using the virtual machine, and that you no longer need the virtual machine.</span></span>
5. <span data-ttu-id="6de1a-182">Felső részén a **VM részletek** panelen válassza **törlése**, és kattintson a **Igen** megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="6de1a-182">At the top of the **VM details** blade, select **Delete**, and then click **Yes** to confirm.</span></span>
6. <span data-ttu-id="6de1a-183">A virtuális Gépet törölni kell, de a VHD-t meg kell őrizni.</span><span class="sxs-lookup"><span data-stu-id="6de1a-183">The VM should be deleted, but the VHD can be retained.</span></span> <span data-ttu-id="6de1a-184">Azonban a virtuális merevlemez már nem kell a címbérlet rajta.</span><span class="sxs-lookup"><span data-stu-id="6de1a-184">However, the VHD should no longer have a lease on it.</span></span> <span data-ttu-id="6de1a-185">A címbérlet mikorra várható néhány percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="6de1a-185">It may take a few minutes for the lease to be released.</span></span> <span data-ttu-id="6de1a-186">Győződjön meg arról, hogy megjelent-e a címbérlet, keresse fel **összes erőforrás** > **Tárfióknév** > **Blobok**  >   **virtuális merevlemezek**.</span><span class="sxs-lookup"><span data-stu-id="6de1a-186">To verify that the lease is released, go to **All resources** > **Storage Account Name** > **Blobs** > **vhds**.</span></span> <span data-ttu-id="6de1a-187">Az a **Blob-tulajdonságok** ablaktáblán, a **bérleti állapot** értéket kell **feloldva**.</span><span class="sxs-lookup"><span data-stu-id="6de1a-187">In the **Blob properties** pane, the **Lease Status** value should be **Unlocked**.</span></span>

<span data-ttu-id="6de1a-188">Ha a virtuális merevlemez adatlemezt, válassza le a virtuális Merevlemezt a virtuális gép eltávolítása a címbérlet:</span><span class="sxs-lookup"><span data-stu-id="6de1a-188">If the VHD is a data disk, detach the VHD from the VM to remove the lease:</span></span>

1. <span data-ttu-id="6de1a-189">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6de1a-189">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="6de1a-190">Az a **Hub** menü **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="6de1a-190">On the **Hub** menu, select **Virtual Machines**.</span></span>
3. <span data-ttu-id="6de1a-191">Válassza ki a virtuális Gépet, amely tárolja a címbérlet a virtuális merevlemezen.</span><span class="sxs-lookup"><span data-stu-id="6de1a-191">Select the VM that holds a lease on the VHD.</span></span>
4. <span data-ttu-id="6de1a-192">Válassza ki **lemezek** a a **VM részletek** panelen.</span><span class="sxs-lookup"><span data-stu-id="6de1a-192">Select **Disks** on the **VM details** blade.</span></span>
5. <span data-ttu-id="6de1a-193">Válassza ki az adatlemezt, amely tárolja a címbérlet a virtuális merevlemezen.</span><span class="sxs-lookup"><span data-stu-id="6de1a-193">Select the data disk that holds a lease on the VHD.</span></span> <span data-ttu-id="6de1a-194">Segítségével meghatározhatja a virtuális merevlemez csatolva a lemezt a virtuális merevlemez URL-CÍMÉT ellenőrzésével.</span><span class="sxs-lookup"><span data-stu-id="6de1a-194">You can determine which VHD is attached in the disk by checking the URL of the VHD.</span></span>
6. <span data-ttu-id="6de1a-195">Határozza meg, hogy semmi sem aktívan használja az adatok biztosan.</span><span class="sxs-lookup"><span data-stu-id="6de1a-195">Determine with certainty that nothing is actively using the data disk.</span></span>
7. <span data-ttu-id="6de1a-196">Kattintson a **leválasztási** a a **részletek lemez** panelen.</span><span class="sxs-lookup"><span data-stu-id="6de1a-196">Click **Detach** on the **Disk details** blade.</span></span>
8. <span data-ttu-id="6de1a-197">A lemez most a virtuális gép választható le, és a virtuális merevlemez már nem kell rendelkeznie a címbérlet rajta.</span><span class="sxs-lookup"><span data-stu-id="6de1a-197">The disk should now be detached from the VM, and the VHD should no longer have a lease on it.</span></span> <span data-ttu-id="6de1a-198">A címbérlet mikorra várható néhány percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="6de1a-198">It may take a few minutes for the lease to be released.</span></span> <span data-ttu-id="6de1a-199">Győződjön meg arról, hogy a címbérlet kiadása, keresse fel **összes erőforrás** > **Tárfióknév** > **Blobok**  >  **VHD-k**.</span><span class="sxs-lookup"><span data-stu-id="6de1a-199">To verify that the lease has been released, go to **All resources** > **Storage Account Name** > **Blobs** > **vhds**.</span></span> <span data-ttu-id="6de1a-200">Az a **Blob-tulajdonságok** ablaktáblán, a **bérleti állapot** értéket kell **feloldva**.</span><span class="sxs-lookup"><span data-stu-id="6de1a-200">In the **Blob properties** pane, the **Lease Status** value should be **Unlocked**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6de1a-201">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6de1a-201">Next steps</span></span>
* [<span data-ttu-id="6de1a-202">Tárfiók törlése</span><span class="sxs-lookup"><span data-stu-id="6de1a-202">Delete a storage account</span></span>](storage-create-storage-account.md#delete-a-storage-account)
* [<span data-ttu-id="6de1a-203">Hogyan bontsa a zárolt címbérlete a blob storage (PowerShell) a Microsoft Azure-ban</span><span class="sxs-lookup"><span data-stu-id="6de1a-203">How to break the locked lease of blob storage in Microsoft Azure (PowerShell)</span></span>](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
