---
title: "Készítsen biztonsági másolatot a Windows rendszer állapotát az Azure-bA |} Microsoft Docs"
description: "Ismerje meg, a rendszerállapot biztonsági mentését a Windows Server és/vagy a Windows számítógépek az Azure-bA."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: carmonm
editor: 
keywords: "biztonsági mentés menete; biztonsági mentési útmutató; fájlok és mappák biztonsági mentése"
ms.assetid: 5b15ebf1-2214-4722-b937-96e2be8872bb
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: saurse;markgal
ms.openlocfilehash: 6fbd96935f444d8b0c6d068ebd0d28e612f19816
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="back-up-windows-system-state-in-resource-manager-deployment"></a><span data-ttu-id="be604-104">A Resource Manager telepítés Windows rendszerállapot biztonsági mentését</span><span class="sxs-lookup"><span data-stu-id="be604-104">Back up Windows system state in Resource Manager deployment</span></span>
<span data-ttu-id="be604-105">Ez a cikk ismerteti a rendszerállapot biztonsági mentését a Windows Server az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="be604-105">This article explains how to back up your Windows Server system state to Azure.</span></span> <span data-ttu-id="be604-106">Ez az oktatóanyag végigvezeti az alapokon.</span><span class="sxs-lookup"><span data-stu-id="be604-106">It's a tutorial intended to walk you through the basics.</span></span>

<span data-ttu-id="be604-107">Ha többet szeretne megtudni az Azure Backupról, olvassa el ezt az [áttekintést](backup-introduction-to-azure-backup.md).</span><span class="sxs-lookup"><span data-stu-id="be604-107">If you want to know more about Azure Backup, read this [overview](backup-introduction-to-azure-backup.md).</span></span>

<span data-ttu-id="be604-108">Ha még nincs Azure-előfizetése, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/), amellyel bármely Azure-szolgáltatást elérhet.</span><span class="sxs-lookup"><span data-stu-id="be604-108">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/) that lets you access any Azure service.</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="be604-109">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="be604-109">Create a recovery services vault</span></span>
<span data-ttu-id="be604-110">A fájlok és mappák biztonsági mentéséhez létre kell hoznia egy Recovery Services-tárolót abban a régióban, ahol az adatokat tárolni szeretné.</span><span class="sxs-lookup"><span data-stu-id="be604-110">To back up your files and folders, you need to create a Recovery Services vault in the region where you want to store the data.</span></span> <span data-ttu-id="be604-111">Emellett a tároló replikálásának módját is meg kell határoznia.</span><span class="sxs-lookup"><span data-stu-id="be604-111">You also need to determine how you want your storage replicated.</span></span>

### <a name="to-create-a-recovery-services-vault"></a><span data-ttu-id="be604-112">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="be604-112">To create a Recovery Services vault</span></span>
1. <span data-ttu-id="be604-113">Ha még nem tette meg, jelentkezzen be az [Azure Portalra](https://portal.azure.com/) az Azure-előfizetésével.</span><span class="sxs-lookup"><span data-stu-id="be604-113">If you haven't already done so, sign in to the [Azure Portal](https://portal.azure.com/) using your Azure subscription.</span></span>
2. <span data-ttu-id="be604-114">A központi menüben kattintson a **További szolgáltatások** elemre, majd az erőforrások listájában írja be a **Recovery Services** szöveget, és kattintson a **Recovery Services-tárolók** elemre.</span><span class="sxs-lookup"><span data-stu-id="be604-114">On the Hub menu, click **More services** and in the list of resources, type **Recovery Services** and click **Recovery Services vaults**.</span></span>

    ![Recovery Services-tároló létrehozása – 1. lépés](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    <span data-ttu-id="be604-116">Ha az előfizetés Recovery Services-tárolókat tartalmaz, a tárolók fel vannak sorolva.</span><span class="sxs-lookup"><span data-stu-id="be604-116">If there are recovery services vaults in the subscription, the vaults are listed.</span></span>
3. <span data-ttu-id="be604-117">A **Recovery Services-tárolók** menüben kattintson a **Hozzáadás** elemre.</span><span class="sxs-lookup"><span data-stu-id="be604-117">On the **Recovery Services vaults** menu, click **Add**.</span></span>

    ![Recovery Services-tároló létrehozása – 2. lépés](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    <span data-ttu-id="be604-119">Megnyílik a Recovery Services-tároló panelje, a rendszer pedig egy **Név**, **Előfizetés**, **Erőforráscsoport** és **Hely** megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="be604-119">The Recovery Services vault blade opens, prompting you to provide a **Name**, **Subscription**, **Resource group**, and **Location**.</span></span>

    ![Recovery Services-tároló létrehozása – 3. lépés](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. <span data-ttu-id="be604-121">A **Név** mezőben adjon meg egy egyszerű nevet a tároló azonosításához.</span><span class="sxs-lookup"><span data-stu-id="be604-121">For **Name**, enter a friendly name to identify the vault.</span></span> <span data-ttu-id="be604-122">A névnek egyedinek kell lennie az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="be604-122">The name needs to be unique for the Azure subscription.</span></span> <span data-ttu-id="be604-123">Írjon be egy 2–50 karakter hosszúságú nevet.</span><span class="sxs-lookup"><span data-stu-id="be604-123">Type a name that contains between 2 and 50 characters.</span></span> <span data-ttu-id="be604-124">Ennek egy betűvel kell kezdődnie, és csak betűket, számokat és kötőjeleket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="be604-124">It must start with a letter, and can contain only letters, numbers, and hyphens.</span></span>

5. <span data-ttu-id="be604-125">Az **Előfizetés** szakaszban, az Azure-előfizetés kiválasztásához használja a legördülő menüt.</span><span class="sxs-lookup"><span data-stu-id="be604-125">In the **Subscription** section, use the drop-down menu to choose the Azure subscription.</span></span> <span data-ttu-id="be604-126">Ha csak egy előfizetést használ, az az előfizetés jelenik meg, és továbbléphet a következő lépésre.</span><span class="sxs-lookup"><span data-stu-id="be604-126">If you use only one subscription, that subscription appears and you can skip to the next step.</span></span> <span data-ttu-id="be604-127">Ha nem biztos benne, hogy melyik előfizetést szeretné használni, használja az alapértelmezett (vagy javasolt) előfizetést.</span><span class="sxs-lookup"><span data-stu-id="be604-127">If you are not sure which subscription to use, use the default (or suggested) subscription.</span></span> <span data-ttu-id="be604-128">Csak akkor lesz több választási lehetőség, ha a szervezetéhez tartozó fiók több Azure-előfizetéssel van összekötve.</span><span class="sxs-lookup"><span data-stu-id="be604-128">There are multiple choices only if your organizational account is associated with multiple Azure subscriptions.</span></span>

6. <span data-ttu-id="be604-129">Az **Erőforráscsoport** szakaszban:</span><span class="sxs-lookup"><span data-stu-id="be604-129">In the **Resource group** section:</span></span>

    * <span data-ttu-id="be604-130">válassza az **Új létrehozása** lehetőséget, ha erőforráscsoportot szeretne létrehozni.</span><span class="sxs-lookup"><span data-stu-id="be604-130">select **Create new** if you want to create a Resource group.</span></span>
    <span data-ttu-id="be604-131">Vagy</span><span class="sxs-lookup"><span data-stu-id="be604-131">Or</span></span>
    * <span data-ttu-id="be604-132">válassza a **Meglévő használata** lehetőséget, és kattintson a legördülő menüben az elérhető erőforráscsoportok listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="be604-132">select **Use existing** and click the drop-down menu to see the available list of Resource groups.</span></span>

  <span data-ttu-id="be604-133">Átfogó információk az erőforráscsoportokkal kapcsolatban: [Az Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="be604-133">For complete information on Resource groups, see the [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

7. <span data-ttu-id="be604-134">Kattintson a **Hely** elemre a tárolóhoz tartozó földrajzi régió kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="be604-134">Click **Location** to select the geographic region for the vault.</span></span> <span data-ttu-id="be604-135">Ez a választás határozza meg a földrajzi régiót, ahová az adatok biztonsági másolata el lesz küldve.</span><span class="sxs-lookup"><span data-stu-id="be604-135">This choice determines the geographic region where your backup data is sent.</span></span>

8. <span data-ttu-id="be604-136">Kattintson a Recovery Services-tároló panel alján a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="be604-136">At the bottom of the Recovery Services vault blade, click **Create**.</span></span>

    <span data-ttu-id="be604-137">A Recovery Services-tároló létrehozása több percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="be604-137">It can take several minutes for the Recovery Services vault to be created.</span></span> <span data-ttu-id="be604-138">Figyelje az állapotértesítéseket a portál jobb felső területén.</span><span class="sxs-lookup"><span data-stu-id="be604-138">Monitor the status notifications in the upper right-hand area of the portal.</span></span> <span data-ttu-id="be604-139">Miután a tároló létrejött, megjelenik a Recovery Services-tárolók listájában.</span><span class="sxs-lookup"><span data-stu-id="be604-139">Once your vault is created, it appears in the list of Recovery Services vaults.</span></span> <span data-ttu-id="be604-140">Ha több perc után sem látja a tárolót, kattintson a **Frissítés** gombra.</span><span class="sxs-lookup"><span data-stu-id="be604-140">If after several minutes you don't see your vault, click **Refresh**.</span></span>

    ![Kattintson a Frissítés gombra](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    <span data-ttu-id="be604-142">Ha látja a tárolót a Recovery Services-tárolók listájában, készen áll tárhely-redundancia beállítására.</span><span class="sxs-lookup"><span data-stu-id="be604-142">Once you see your vault in the list of Recovery Services vaults, you are ready to set the storage redundancy.</span></span>

### <a name="set-storage-redundancy-for-the-vault"></a><span data-ttu-id="be604-143">Tárhely-redundancia beállítása a tárolóhoz</span><span class="sxs-lookup"><span data-stu-id="be604-143">Set storage redundancy for the vault</span></span>
<span data-ttu-id="be604-144">A Recovery Services-tároló létrehozásakor győződjön meg róla, hogy a tárhely-redundancia a saját igényei szerint van beállítva.</span><span class="sxs-lookup"><span data-stu-id="be604-144">When you create a Recovery Services vault, make sure storage redundancy is configured the way you want.</span></span>

1. <span data-ttu-id="be604-145">A **Recovery Services-tárolók** panelen kattintson az új tárolóra.</span><span class="sxs-lookup"><span data-stu-id="be604-145">From the **Recovery Services vaults** blade, click the new vault.</span></span>

    ![A Recovery Services-tárolók listájából válassza ki az új tárolót](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    <span data-ttu-id="be604-147">Ha kiválasztja a tárolót, a **Recovery Services-tároló** panel leszűkül, és a Beállítások panel (*amelynek tetején a tároló neve látható*), valamint a tároló részleteit tartalmazó panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="be604-147">When you select the vault, the **Recovery Services vault** blade narrows, and the Settings blade (*which has the name of the vault at the top*) and the vault details blade open.</span></span>

    ![Az új tároló tárolási konfigurációjának beállítása](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)
2. <span data-ttu-id="be604-149">Használja a függőleges csúszkát az új tároló Beállítások paneljén a legörgetéshez a Kezelés szakaszhoz, és kattintson a **Biztonsági mentési infrastruktúra** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="be604-149">In the new vault's Settings blade, use the vertical slide to scroll down to the Manage section, and click **Backup Infrastructure**.</span></span>
    <span data-ttu-id="be604-150">Megnyílik a Biztonsági mentési infrastruktúra panel.</span><span class="sxs-lookup"><span data-stu-id="be604-150">The Backup Infrastructure blade opens.</span></span>
3. <span data-ttu-id="be604-151">A Biztonsági mentési infrastruktúra panelen kattintson a **Biztonsági mentés konfigurációja** elemre a **Biztonsági mentés konfigurációja** panel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="be604-151">In the Backup Infrastructure blade, click **Backup Configuration** to open the **Backup Configuration** blade.</span></span>

    ![Az új tároló tárolási konfigurációjának beállítása](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)
4. <span data-ttu-id="be604-153">Válassza ki a megfelelő tárolóreplikációs beállítást a tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="be604-153">Choose the appropriate storage replication option for your vault.</span></span>

    ![a tároló konfigurálásának lehetőségei](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    <span data-ttu-id="be604-155">Alapértelmezés szerint a tárolója georedundáns tárolással rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="be604-155">By default, your vault has geo-redundant storage.</span></span> <span data-ttu-id="be604-156">Ha az Azure-t használja az elsődleges biztonsági mentési tároló végpontjaként, folytassa a **georedundáns** beállítás használatát.</span><span class="sxs-lookup"><span data-stu-id="be604-156">If you use Azure as a primary backup storage endpoint, continue to use **Geo-redundant**.</span></span> <span data-ttu-id="be604-157">Ha nem az Azure-t használja az elsődleges biztonsági mentési tároló végpontjaként, válassza a **Helyileg redundáns** lehetőséget, amely csökkenti az Azure Storage költségeit.</span><span class="sxs-lookup"><span data-stu-id="be604-157">If you don't use Azure as a primary backup storage endpoint, then choose **Locally-redundant**, which reduces the Azure storage costs.</span></span> <span data-ttu-id="be604-158">A [georedundáns](../storage/common/storage-redundancy.md#geo-redundant-storage) és a [helyileg redundáns](../storage/common/storage-redundancy.md#locally-redundant-storage) tárolási lehetőségekről többet olvashat ebben a [Tárhely-redundancia áttekintésben](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="be604-158">Read more about [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) and [locally redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) storage options in this [Storage redundancy overview](../storage/common/storage-redundancy.md).</span></span>

<span data-ttu-id="be604-159">Most, hogy létrehozta a tárolót, konfigurálja a Windows rendszerállapotról készít biztonsági mentést.</span><span class="sxs-lookup"><span data-stu-id="be604-159">Now that you've created a vault, configure it for backing up Windows System State.</span></span>

## <a name="configure-the-vault"></a><span data-ttu-id="be604-160">A tároló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="be604-160">Configure the vault</span></span>
1. <span data-ttu-id="be604-161">A Recovery Services-tároló (az imént létrehozott tároló) paneljén a Bevezetés szakaszban kattintson a **Biztonsági mentés** elemre, majd a **Bevezetés a biztonsági mentés használatába** panelen válassza a **Biztonsági mentés célja** elemet.</span><span class="sxs-lookup"><span data-stu-id="be604-161">On the Recovery Services vault blade (for the vault you just created), in the Getting Started section, click **Backup**, then on the **Getting Started with Backup** blade, select **Backup goal**.</span></span>

    ![A biztonsági mentés célja panel megnyitása](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

    <span data-ttu-id="be604-163">Megnyílik a **Biztonsági mentés célja** panel.</span><span class="sxs-lookup"><span data-stu-id="be604-163">The **Backup Goal** blade opens.</span></span>

    ![A biztonsági mentés célja panel megnyitása](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. <span data-ttu-id="be604-165">A **Hol futnak az alkalmazások és szolgáltatások?** legördülő menüből válassza a **Helyszíni** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="be604-165">From the **Where is your workload running?** drop-down menu, select **On-premises**.</span></span>

    <span data-ttu-id="be604-166">Azért kell a **Helyszíni** lehetőséget választania, mert a Windows Server- vagy Windows-számítógépe egy fizikai gép, amely nem az Azure része.</span><span class="sxs-lookup"><span data-stu-id="be604-166">You choose **On-premises** because your Windows Server or Windows computer is a physical machine that is not in Azure.</span></span>

3. <span data-ttu-id="be604-167">Az a **miről szeretne biztonsági másolatot készíteni?** menüjében válassza **rendszerállapot**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="be604-167">From the **What do you want to backup?** menu, select **System State**, and click **OK**.</span></span>

    ![Fájlok és mappák konfigurálása](./media/backup-azure-system-state/backup-goal-system-state.png)

    <span data-ttu-id="be604-169">Miután az OK gombra kattint, a **Biztonsági mentés célja** mellett megjelenik egy pipa, és megnyílik **Az infrastruktúra előkészítése** panel.</span><span class="sxs-lookup"><span data-stu-id="be604-169">After clicking OK, a checkmark appears next to **Backup goal**, and the **Prepare infrastructure** blade opens.</span></span>

    ![Ha konfigurálta a biztonsági mentés célját, a következő lépés az infrastruktúra előkészítése](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. <span data-ttu-id="be604-171">**Az infrastruktúra előkészítése** panelen kattintson **A Windows Server- vagy a Windows-ügyfél ügynökének letöltése** elemre.</span><span class="sxs-lookup"><span data-stu-id="be604-171">On the **Prepare infrastructure** blade, click **Download Agent for Windows Server or Windows Client**.</span></span>

    ![infrastruktúra előkészítése](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

    <span data-ttu-id="be604-173">Ha Windows Server Essential verziót használ, akkor a Windows Server Essential ügynökét töltse le.</span><span class="sxs-lookup"><span data-stu-id="be604-173">If you are using Windows Server Essential, then choose to download the agent for Windows Server Essential.</span></span> <span data-ttu-id="be604-174">Egy előugró menü rákérdez, hogy futtatni vagy menteni kívánja-e a MARSAgentInstaller.exe fájlt.</span><span class="sxs-lookup"><span data-stu-id="be604-174">A pop-up menu prompts you to run or save MARSAgentInstaller.exe.</span></span>

    ![MARSAgentInstaller párbeszédpanel](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. <span data-ttu-id="be604-176">A letöltési előugró menüben kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="be604-176">In the download pop-up menu, click **Save**.</span></span>

    <span data-ttu-id="be604-177">Alapértelmezés szerint az **MARSagentinstaller.exe** fájlt a rendszer a Downloads mappába menti.</span><span class="sxs-lookup"><span data-stu-id="be604-177">By default, the **MARSagentinstaller.exe** file is saved to your Downloads folder.</span></span> <span data-ttu-id="be604-178">Amikor a telepítő végzett, megjelenik egy előugró ablak, amely rákérdez, hogy szeretné-e futtatni a telepítőt, vagy megnyitni a mappát.</span><span class="sxs-lookup"><span data-stu-id="be604-178">When the installer completes, you will see a pop-up asking if you want to run the installer, or open the folder.</span></span>

    ![infrastruktúra előkészítése](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

    <span data-ttu-id="be604-180">Az ügynököt még nem kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="be604-180">You don't need to install the agent yet.</span></span> <span data-ttu-id="be604-181">A tároló hitelesítő adatainak letöltése után telepítheti az ügynököt.</span><span class="sxs-lookup"><span data-stu-id="be604-181">You can install the agent after you have downloaded the vault credentials.</span></span>

6. <span data-ttu-id="be604-182">**Az infrastruktúra előkészítése** panelen kattintson a **Letöltés** elemre.</span><span class="sxs-lookup"><span data-stu-id="be604-182">On the **Prepare infrastructure** blade, click **Download**.</span></span>

    ![a tároló hitelesítő adatainak letöltése](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

    <span data-ttu-id="be604-184">A tároló hitelesítő adatait a rendszer a Letöltések mappába menti.</span><span class="sxs-lookup"><span data-stu-id="be604-184">The vault credentials download to your Downloads folder.</span></span> <span data-ttu-id="be604-185">Miután a tároló hitelesítő adatainak letöltése befejeződött, megjelenik egy előugró ablak, amely rákérdez, hogy szeretné-e megnyitni vagy menteni a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="be604-185">After the vault credentials finish downloading, you see a pop-up asking if you want to open or save the credentials.</span></span> <span data-ttu-id="be604-186">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="be604-186">Click **Save**.</span></span> <span data-ttu-id="be604-187">Ha véletlenül a **Megnyitás** gombra kattint, hagyja, hogy sikertelen legyen a párbeszédpanel, amely megpróbálja megnyitni a tároló hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="be604-187">If you accidentally click **Open**, let the dialog that attempts to open the vault credentials, fail.</span></span> <span data-ttu-id="be604-188">A tároló hitelesítő adatai nem nyithatók meg.</span><span class="sxs-lookup"><span data-stu-id="be604-188">You cannot open the vault credentials.</span></span> <span data-ttu-id="be604-189">Folytassa a következő lépéssel.</span><span class="sxs-lookup"><span data-stu-id="be604-189">Proceed to the next step.</span></span> <span data-ttu-id="be604-190">A tároló hitelesítő adatai a Letöltések mappában találhatók.</span><span class="sxs-lookup"><span data-stu-id="be604-190">The vault credentials are in the Downloads folder.</span></span>   

    ![a tároló hitelesítő adatainak letöltése befejeződött](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)

## <a name="install-and-register-the-agent"></a><span data-ttu-id="be604-192">Az ügynök telepítése és regisztrálása</span><span class="sxs-lookup"><span data-stu-id="be604-192">Install and register the agent</span></span>

> [!NOTE]
> <span data-ttu-id="be604-193">A biztonsági mentés engedélyezése az Azure Portalon keresztül még nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="be604-193">Enabling backup through the Azure portal is not available, yet.</span></span> <span data-ttu-id="be604-194">A Microsoft Azure Recovery Services Agent használatával Windows Server rendszerállapot biztonsági mentését.</span><span class="sxs-lookup"><span data-stu-id="be604-194">Use the Microsoft Azure Recovery Services Agent to back up Windows Server System State.</span></span>
>

1. <span data-ttu-id="be604-195">Keresse meg és kattintson duplán az **MARSagentinstaller.exe** fájlra a Letöltések mappában (vagy más mentési helyen).</span><span class="sxs-lookup"><span data-stu-id="be604-195">Locate and double-click the **MARSagentinstaller.exe** from the Downloads folder (or other saved location).</span></span>

    <span data-ttu-id="be604-196">A telepítő egy sor üzenetet jelenít meg, miközben kibontja, telepíti és regisztrálja a Recovery Services-ügynököt.</span><span class="sxs-lookup"><span data-stu-id="be604-196">The installer provides a series of messages as it extracts, installs, and registers the Recovery Services agent.</span></span>

    ![a Recovery Services-ügynök telepítőjének futtatása, hitelesítő adatok](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. <span data-ttu-id="be604-198">Végezze el a Microsoft Azure Recovery Services ügynök telepítővarázslójának lépéseit.</span><span class="sxs-lookup"><span data-stu-id="be604-198">Complete the Microsoft Azure Recovery Services Agent Setup Wizard.</span></span> <span data-ttu-id="be604-199">A varázsló lépéseinek elvégzéséhez a következőket kell tennie:</span><span class="sxs-lookup"><span data-stu-id="be604-199">To complete the wizard, you need to:</span></span>

   * <span data-ttu-id="be604-200">Válasszon egy helyet a telepítés és a gyorsítótár mappája számára.</span><span class="sxs-lookup"><span data-stu-id="be604-200">Choose a location for the installation and cache folder.</span></span>
   * <span data-ttu-id="be604-201">Adja meg a proxykiszolgáló információit, ha proxykiszolgálóval csatlakozik az internethez.</span><span class="sxs-lookup"><span data-stu-id="be604-201">Provide your proxy server info if you use a proxy server to connect to the internet.</span></span>
   * <span data-ttu-id="be604-202">Adja meg a felhasználónevének és jelszavának részleteit, ha hitelesített proxyt használ.</span><span class="sxs-lookup"><span data-stu-id="be604-202">Provide your user name and password details if you use an authenticated proxy.</span></span>
   * <span data-ttu-id="be604-203">Adja meg a tároló letöltött hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="be604-203">Provide the downloaded vault credentials</span></span>
   * <span data-ttu-id="be604-204">Mentse a titkosítási jelszót egy biztonságos helyen.</span><span class="sxs-lookup"><span data-stu-id="be604-204">Save the encryption passphrase in a secure location.</span></span>

     > [!NOTE]
     > <span data-ttu-id="be604-205">Ha elveszíti vagy elfelejti a jelszót, a Microsoft nem tud segíteni az adatok biztonsági másolatának visszaállításában.</span><span class="sxs-lookup"><span data-stu-id="be604-205">If you lose or forget the passphrase, Microsoft cannot help recover the backup data.</span></span> <span data-ttu-id="be604-206">Mentse a fájlt egy biztonságos helyen.</span><span class="sxs-lookup"><span data-stu-id="be604-206">Save the file in a secure location.</span></span> <span data-ttu-id="be604-207">Erre szükség van a biztonsági másolat visszaállításához.</span><span class="sxs-lookup"><span data-stu-id="be604-207">It is required to restore a backup.</span></span>
     >
     >

<span data-ttu-id="be604-208">Az ügynök most telepítve van, és a gépe regisztrálva van a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="be604-208">The agent is now installed and your machine is registered to the vault.</span></span> <span data-ttu-id="be604-209">Készen áll a biztonsági mentés konfigurálására és ütemezésére.</span><span class="sxs-lookup"><span data-stu-id="be604-209">You're ready to configure and schedule your backup.</span></span>

## <a name="back-up-windows-server-system-state-preview"></a><span data-ttu-id="be604-210">Készítsen biztonsági másolatot a Windows Server rendszer állapota (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="be604-210">Back up Windows Server System State (Preview)</span></span>
<span data-ttu-id="be604-211">A kezdeti biztonsági másolatot a három feladatokból áll:</span><span class="sxs-lookup"><span data-stu-id="be604-211">The initial backup includes three tasks:</span></span>

* <span data-ttu-id="be604-212">Rendszerállapot biztonsági mentésének segítségével az Azure Backup szolgáltatás ügynökének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="be604-212">Enable System State Backup using the Azure Backup agent</span></span>
* <span data-ttu-id="be604-213">A biztonsági mentés ütemezése</span><span class="sxs-lookup"><span data-stu-id="be604-213">Schedule the backup</span></span>
* <span data-ttu-id="be604-214">A fájlok és mappák biztonsági mentése első alkalommal</span><span class="sxs-lookup"><span data-stu-id="be604-214">Back up files and folders for the first time</span></span>

<span data-ttu-id="be604-215">A kezdeti biztonsági mentés végrehajtásához használja a Microsoft Azure Recovery Services-ügynököt.</span><span class="sxs-lookup"><span data-stu-id="be604-215">To complete the initial backup, use the Microsoft Azure Recovery Services agent.</span></span>

### <a name="to-enable-system-state-backup-using-the-azure-backup-agent"></a><span data-ttu-id="be604-216">Rendszerállapot biztonsági mentésének segítségével az Azure Backup szolgáltatás ügynöke engedélyezése</span><span class="sxs-lookup"><span data-stu-id="be604-216">To enable System State backup using the Azure Backup agent</span></span>

1. <span data-ttu-id="be604-217">Egy PowerShell-munkamenetben futtassa a következő parancs futtatásával állítsa le az Azure Backup motor.</span><span class="sxs-lookup"><span data-stu-id="be604-217">In a PowerShell session, run the following command to stop the Azure Backup engine.</span></span>

  ```
  PS C:\> Net stop obengine
  ```

2. <span data-ttu-id="be604-218">Nyissa meg a Windows beállításjegyzéket.</span><span class="sxs-lookup"><span data-stu-id="be604-218">Open the Windows Registry.</span></span>

  ```
  PS C:\> regedit.exe
  ```

3. <span data-ttu-id="be604-219">Adja hozzá a következő beállításkulcs DWord értéket.</span><span class="sxs-lookup"><span data-stu-id="be604-219">Add the following registry key with the specified DWord Value.</span></span>

  | <span data-ttu-id="be604-220">Beállításjegyzékbeli elérési út</span><span class="sxs-lookup"><span data-stu-id="be604-220">Registry path</span></span> | <span data-ttu-id="be604-221">Beállításkulcs</span><span class="sxs-lookup"><span data-stu-id="be604-221">Registry key</span></span> | <span data-ttu-id="be604-222">Duplaszó</span><span class="sxs-lookup"><span data-stu-id="be604-222">DWord value</span></span> |
  |---------------|--------------|-------------|
  | <span data-ttu-id="be604-223">HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider</span><span class="sxs-lookup"><span data-stu-id="be604-223">HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider</span></span> | <span data-ttu-id="be604-224">TurnOffSSBFeature</span><span class="sxs-lookup"><span data-stu-id="be604-224">TurnOffSSBFeature</span></span> | <span data-ttu-id="be604-225">2</span><span class="sxs-lookup"><span data-stu-id="be604-225">2</span></span> |

4. <span data-ttu-id="be604-226">Indítsa újra a biztonsági mentés motor hajtja végre a következő parancsot egy rendszergazda jogú parancssort a.</span><span class="sxs-lookup"><span data-stu-id="be604-226">Restart the Backup engine by executing the following command in an elevated command prompt.</span></span>

  ```
  PS C:\> Net start obengine
  ```

### <a name="to-schedule-the-backup-job"></a><span data-ttu-id="be604-227">A biztonsági mentési feladat ütemezése</span><span class="sxs-lookup"><span data-stu-id="be604-227">To schedule the backup job</span></span>

1. <span data-ttu-id="be604-228">Nyissa meg a Microsoft Azure Recovery Services-ügynököt.</span><span class="sxs-lookup"><span data-stu-id="be604-228">Open the Microsoft Azure Recovery Services agent.</span></span> <span data-ttu-id="be604-229">A megkereséséhez keressen rá a gépen a **Microsoft Azure Backup** kifejezésre.</span><span class="sxs-lookup"><span data-stu-id="be604-229">You can find it by searching your machine for **Microsoft Azure Backup**.</span></span>

    ![Az Azure Recovery Services-ügynök indítása](./media/backup-try-azure-backup-in-10-mins/snap-in-search.png)

2. <span data-ttu-id="be604-231">A Recovery Services-ügynökben kattintson a **Biztonsági mentés ütemezése** gombra.</span><span class="sxs-lookup"><span data-stu-id="be604-231">In the Recovery Services agent, click **Schedule Backup**.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)

3. <span data-ttu-id="be604-233">A Biztonsági mentés ütemezése varázsló Első lépések oldalán kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="be604-233">On the Getting started page of the Schedule Backup Wizard, click **Next**.</span></span>

4. <span data-ttu-id="be604-234">Az Elemek kijelölése biztonsági mentéshez oldalon kattintson az **Elemek hozzáadása** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="be604-234">On the Select Items to Backup page, click **Add Items**.</span></span>

5. <span data-ttu-id="be604-235">Válassza ki **rendszerállapot** majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="be604-235">Select **System State** and then click **OK**.</span></span>

6. <span data-ttu-id="be604-236">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="be604-236">Click **Next**.</span></span>

7. <span data-ttu-id="be604-237">A rendszerállapot biztonsági mentése és a megőrzési ütemezés automatikusan értéke készítsen biztonsági másolatot minden vasárnap 9:00 PM helyi időben, és a megőrzési idő 60 napra van állítva.</span><span class="sxs-lookup"><span data-stu-id="be604-237">The System State Backup and Retention schedule is automatically set to back up every Sunday at 9:00 PM local time, and the retention period is set to 60 days.</span></span>

   > [!NOTE]
   > <span data-ttu-id="be604-238">Rendszerállapot biztonsági mentési és adatmegőrzési házirend automatikusan konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="be604-238">System State backup and retention policy is automatically configured.</span></span> <span data-ttu-id="be604-239">Ha a biztonsági mentést a fájlok és mappák mellett a Windows Server rendszer állapotáról, adja meg, csak a biztonsági mentési és adatmegőrzési házirend a fájl biztonsági mentésekhez, a varázslóból.</span><span class="sxs-lookup"><span data-stu-id="be604-239">If you back up Files and Folders in addition to the Windows Server System State, specify only the Backup and Retention policy for file backups from the wizard.</span></span> 
   >

8. <span data-ttu-id="be604-240">A Jóváhagyás lapon ellenőrizze az információkat, majd kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="be604-240">On the Confirmation page, review the information, and then click **Finish**.</span></span>

9. <span data-ttu-id="be604-241">Miután a varázsló befejezte a biztonsági mentési ütemezés létrehozását, kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="be604-241">After the wizard finishes creating the backup schedule, click **Close**.</span></span>

### <a name="to-back-up-windows-server-system-state-for-the-first-time"></a><span data-ttu-id="be604-242">Biztonsági mentése a Windows Server rendszer állapota első alkalommal</span><span class="sxs-lookup"><span data-stu-id="be604-242">To back up Windows Server System State for the first time</span></span>

1. <span data-ttu-id="be604-243">Győződjön meg arról, hogy nincsenek függőben lévő frissítések a Windows Server, a számítógép újraindítása szükséges.</span><span class="sxs-lookup"><span data-stu-id="be604-243">Make sure there are no pending updates for Windows Server that require a reboot.</span></span>

2. <span data-ttu-id="be604-244">A Recovery Services-ügynökben kattintson a **Biztonsági mentés** gombra a hálózaton keresztüli kezdeti összehangolás befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="be604-244">In the Recovery Services agent, click **Back Up Now** to complete the initial seeding over the network.</span></span>

    ![Windows Server biztonsági másolat készítése](./media/backup-try-azure-backup-in-10-mins/backup-now.png)

3. <span data-ttu-id="be604-246">A Jóváhagyás lapon tekintse át azokat a beállításokat, amelyeket a Biztonsági másolat készítése varázsló a gép biztonsági mentéséhez fog használni.</span><span class="sxs-lookup"><span data-stu-id="be604-246">On the Confirmation page, review the settings that the Back Up Now Wizard will use to back up the machine.</span></span> <span data-ttu-id="be604-247">Ezután kattintson a **Biztonsági mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="be604-247">Then click **Back Up**.</span></span>

4. <span data-ttu-id="be604-248">A varázsló bezárásához kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="be604-248">Click **Close** to close the wizard.</span></span> <span data-ttu-id="be604-249">Ha bezárja a varázslót a biztonsági mentési folyamat befejezése előtt, a varázsló továbbra is fut a háttérben.</span><span class="sxs-lookup"><span data-stu-id="be604-249">If you close the wizard before the backup process finishes, the wizard continues to run in the background.</span></span>

5. <span data-ttu-id="be604-250">Ha a kiszolgálón kívül a Windows Server rendszerállapot biztonsági mentése a fájlokat és mappákat a biztonsági mentés most varázsló csak mentésére fájlokat.</span><span class="sxs-lookup"><span data-stu-id="be604-250">If you back up Files and Folders on your server, in addition to the Windows Server System State, the Backup Now wizard will only back up files.</span></span> <span data-ttu-id="be604-251">Hajtsa végre az alkalmi rendszerállapot biztonsági mentése, használja a következő PowerShell-parancsot:</span><span class="sxs-lookup"><span data-stu-id="be604-251">To perform an ad hoc System State back up, use the following PowerShell command:</span></span>

    ```
    PS C:\> Start-OBSystemStateBackup
    ```

  <span data-ttu-id="be604-252">A kezdeti biztonsági mentés befejezése után a **Feladat befejezve** állapot jelenik meg a biztonsági mentési konzolon.</span><span class="sxs-lookup"><span data-stu-id="be604-252">After the initial backup is completed, the **Job completed** status appears in the Backup console.</span></span>

  ![IR befejezve](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## <a name="frequently-asked-questions"></a><span data-ttu-id="be604-254">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="be604-254">Frequently asked questions</span></span>

<span data-ttu-id="be604-255">Az alábbi kérdések és válaszok kiegészítő információkat.</span><span class="sxs-lookup"><span data-stu-id="be604-255">The following questions and answers provide supplementary information.</span></span>

### <a name="what-is-the-staging-volume"></a><span data-ttu-id="be604-256">Mi az az átmeneti kötet?</span><span class="sxs-lookup"><span data-stu-id="be604-256">What is the Staging Volume?</span></span>

<span data-ttu-id="be604-257">Az átmeneti kötet a köztes helyre, ahol a natív módon, a Windows Server biztonsági másolat készít elő a rendszerállapot biztonsági mentését jelöli.</span><span class="sxs-lookup"><span data-stu-id="be604-257">The Staging Volume represents the intermediate location where the natively available, Windows Server Backup stages the System State Backup.</span></span> <span data-ttu-id="be604-258">Az Azure Backup szolgáltatás ügynökének majd tömöríti és titkosítja a köztes biztonsági mentési és biztonságos HTTPS protokollon keresztül elküldi a konfigurált Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="be604-258">Azure Backup agent then compresses and encrypts this intermediate backup and sends it via secure HTTPS Protocol to the configured Recovery Services Vault.</span></span> <span data-ttu-id="be604-259">**Erősen ajánlott kapcsolatot hoz létre az átmeneti köteten nem-Windows-OS köteten. Azt láthatja, hogy a rendszerállapot biztonsági mentéseit problémákat, ha a hibaelhárítás első lépése a átmeneti kötet hely annak ellenőrzése, hogy áll.**</span><span class="sxs-lookup"><span data-stu-id="be604-259">**We strongly recommended you establish the Staging Volume in a non-Windows-OS volume. If you observe problems with System State Backups, checking the location of your Staging Volume is the first troubleshooting step.**</span></span> 

### <a name="how-can-i-change-the-staging-volume-path-specified-in-the-azure-backup-agent"></a><span data-ttu-id="be604-260">Hogyan lehet módosítani a átmeneti kötet megadott elérési út az Azure Backup szolgáltatás ügynökének?</span><span class="sxs-lookup"><span data-stu-id="be604-260">How can I change the Staging Volume Path specified in the Azure Backup agent?</span></span>

<span data-ttu-id="be604-261">Az átmeneti kötet alapértelmezés szerint a gyorsítótár mappában található.</span><span class="sxs-lookup"><span data-stu-id="be604-261">The Staging Volume is located in the cache folder by default.</span></span> 

1. <span data-ttu-id="be604-262">Ez a hely módosításához használja a következő parancsot (a rendszergazda jogú parancssorból):</span><span class="sxs-lookup"><span data-stu-id="be604-262">To change this location, use the following command (in an elevated command prompt):</span></span>
  ```
  PS C:\> Net stop obengine
  ```

2. <span data-ttu-id="be604-263">Ezután frissítse a következő beállításjegyzék-bejegyzések az új kötet átmeneti mappa elérési útját.</span><span class="sxs-lookup"><span data-stu-id="be604-263">Then update the following registry entries with the path to the new Staging Volume folder.</span></span>

  |<span data-ttu-id="be604-264">Beállításjegyzékbeli elérési út</span><span class="sxs-lookup"><span data-stu-id="be604-264">Registry path</span></span>|<span data-ttu-id="be604-265">Beállításkulcs</span><span class="sxs-lookup"><span data-stu-id="be604-265">Registry key</span></span>|<span data-ttu-id="be604-266">Érték</span><span class="sxs-lookup"><span data-stu-id="be604-266">Value</span></span>|
  |-------------|------------|-----|
  |<span data-ttu-id="be604-267">HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider</span><span class="sxs-lookup"><span data-stu-id="be604-267">HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Azure Backup\Config\CloudBackupProvider</span></span> | <span data-ttu-id="be604-268">SSBStagingPath</span><span class="sxs-lookup"><span data-stu-id="be604-268">SSBStagingPath</span></span> | <span data-ttu-id="be604-269">Új átmeneti kötet hely</span><span class="sxs-lookup"><span data-stu-id="be604-269">new staging volume location</span></span> |

<span data-ttu-id="be604-270">Az átmeneti elérési út kis-és nagybetűket, és a pontos azonos kis-és nagybetűk, mi létezik a kiszolgálón kell lennie.</span><span class="sxs-lookup"><span data-stu-id="be604-270">The Staging Path is case sensitive and must be the exact same casing as what exists on the server.</span></span> 

3. <span data-ttu-id="be604-271">Az átmeneti kötet elérési út módosítása után indítsa újra a biztonsági mentés motor:</span><span class="sxs-lookup"><span data-stu-id="be604-271">Once you change the Staging volume path, restart the Backup engine:</span></span>
  ```
  PS C:\> Net start obengine
  ```
4. <span data-ttu-id="be604-272">A módosított elérési utat átvételéhez, nyissa meg a Microsoft Azure Recovery Services Agent ügynököt, és rendszerállapot ad hoc biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="be604-272">To pick up the changed path, open the Microsoft Azure Recovery Services agent and trigger an ad hoc backup of System State.</span></span>

### <a name="why-is-the-system-state-default-retention-set-to-60-days"></a><span data-ttu-id="be604-273">A rendszerállapot alapértelmezett megőrzési miért van beállítva 60 napra?</span><span class="sxs-lookup"><span data-stu-id="be604-273">Why is the System State default retention set to 60 days?</span></span>

<span data-ttu-id="be604-274">A rendszerállapot biztonsági mentését élettartama megegyezik a Windows Server Active Directory szerepkör "szemétgyűjtési" beállítást.</span><span class="sxs-lookup"><span data-stu-id="be604-274">The useful life of a system state backup is the same as the "tombstone lifetime" setting for the Windows Server Active Directory role.</span></span> <span data-ttu-id="be604-275">A törlésre kijelölt élettartama bejegyzés alapértelmezett értéke 60 nap.</span><span class="sxs-lookup"><span data-stu-id="be604-275">The default value for the tombstone lifetime entry is 60 days.</span></span> <span data-ttu-id="be604-276">Ez az érték akkor állíthatók be a címtár szolgáltatást (NTDS) konfigurációs objektum.</span><span class="sxs-lookup"><span data-stu-id="be604-276">This value can be set on the Directory Service (NTDS) config object.</span></span>

### <a name="how-do-i-change-the-default-backup-and-retention-policy-for-system-state"></a><span data-ttu-id="be604-277">Hogyan módosítható az alapértelmezett biztonsági mentési és adatmegőrzési rendszerállapot?</span><span class="sxs-lookup"><span data-stu-id="be604-277">How do I change the default Backup and Retention Policy for System State?</span></span>

<span data-ttu-id="be604-278">Az alapértelmezett biztonsági mentési és adatmegőrzési rendszerállapot módosíthatja:</span><span class="sxs-lookup"><span data-stu-id="be604-278">To change the default Backup and Retention Policy for System State:</span></span>
1. <span data-ttu-id="be604-279">Állítsa le a biztonsági mentés motor.</span><span class="sxs-lookup"><span data-stu-id="be604-279">Stop the Backup engine.</span></span> <span data-ttu-id="be604-280">A következő parancsot egy emelt szintű parancssorból.</span><span class="sxs-lookup"><span data-stu-id="be604-280">Run the following command from an elevated command prompt.</span></span>

  ```
  PS C:\> Net stop obengine
  ```

2. <span data-ttu-id="be604-281">Adja hozzá, vagy frissítse a HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider következő beállításjegyzékbeli kulcs bejegyzéseit.</span><span class="sxs-lookup"><span data-stu-id="be604-281">Add or update the following registry key entries in HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider.</span></span>

  |<span data-ttu-id="be604-282">Rendszerleíró adatbázis neve</span><span class="sxs-lookup"><span data-stu-id="be604-282">Registry Name</span></span>|<span data-ttu-id="be604-283">Leírás</span><span class="sxs-lookup"><span data-stu-id="be604-283">Description</span></span>|<span data-ttu-id="be604-284">Érték</span><span class="sxs-lookup"><span data-stu-id="be604-284">Value</span></span>|
  |-------------|-----------|-----|
  |<span data-ttu-id="be604-285">SSBScheduleTime</span><span class="sxs-lookup"><span data-stu-id="be604-285">SSBScheduleTime</span></span>|<span data-ttu-id="be604-286">A biztonsági másolat készítésének idején konfigurálásához használt.</span><span class="sxs-lookup"><span data-stu-id="be604-286">Used to configure the time of the backup.</span></span> <span data-ttu-id="be604-287">Alapértelmezett érték a helyi idő du. 9.</span><span class="sxs-lookup"><span data-stu-id="be604-287">Default is 9PM local time.</span></span>|<span data-ttu-id="be604-288">DWord: Formátum HHMM (decimális) például 2130 9:30 PM helyi ideje</span><span class="sxs-lookup"><span data-stu-id="be604-288">DWord: Format HHMM (Decimal) for example 2130 for 9:30PM local time</span></span>|
  |<span data-ttu-id="be604-289">SSBScheduleDays</span><span class="sxs-lookup"><span data-stu-id="be604-289">SSBScheduleDays</span></span>|<span data-ttu-id="be604-290">Az nap, amikor a rendszerállapot biztonsági mentését kell végrehajtani, a megadott időpontban konfigurálásához használt.</span><span class="sxs-lookup"><span data-stu-id="be604-290">Used to configure the days when System State Backup must be performed at the specified time.</span></span> <span data-ttu-id="be604-291">Egyes számjegyeket adja meg a napokat a hét.</span><span class="sxs-lookup"><span data-stu-id="be604-291">Individual digits specify days of the week.</span></span> <span data-ttu-id="be604-292">0 a vasárnapot jelenti, 1 hétfő, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="be604-292">0 represents Sunday, 1 is Monday, and so on.</span></span> <span data-ttu-id="be604-293">Alapértelmezett napi biztonsági mentéshez a vasárnapot jelenti.</span><span class="sxs-lookup"><span data-stu-id="be604-293">Default day for backup is Sunday.</span></span>|<span data-ttu-id="be604-294">DWord: napokat a hét például 1230 ütemez biztonsági mentések hétfő, kedd, szerda és vasárnap (decimális) biztonsági mentés futtatására.</span><span class="sxs-lookup"><span data-stu-id="be604-294">DWord: days of the week to run backup (decimal) for example 1230 schedules backups on Monday, Tuesday, Wednesday, and Sunday.</span></span>|
  |<span data-ttu-id="be604-295">SSBRetentionDays</span><span class="sxs-lookup"><span data-stu-id="be604-295">SSBRetentionDays</span></span>|<span data-ttu-id="be604-296">A biztonsági mentés napokban konfigurálásához használt.</span><span class="sxs-lookup"><span data-stu-id="be604-296">Used to configure the days to retain backup.</span></span> <span data-ttu-id="be604-297">Alapértelmezett érték 60.</span><span class="sxs-lookup"><span data-stu-id="be604-297">Default value is 60.</span></span> <span data-ttu-id="be604-298">Megengedett maximális értéket 180.</span><span class="sxs-lookup"><span data-stu-id="be604-298">Maximum allowed value is 180.</span></span>|<span data-ttu-id="be604-299">DWord: Napig szeretné megőrizni a biztonsági mentés (decimális).</span><span class="sxs-lookup"><span data-stu-id="be604-299">DWord: Days to retain backup (decimal).</span></span>|

3. <span data-ttu-id="be604-300">A következő paranccsal indítsa újra a biztonsági mentési motor.</span><span class="sxs-lookup"><span data-stu-id="be604-300">Use the following command to restart the backup engine.</span></span>
    ```
    PS C:\> Net start obengine
    ```

4. <span data-ttu-id="be604-301">Nyissa meg a Microsoft Recovery Services Agent ügynököt.</span><span class="sxs-lookup"><span data-stu-id="be604-301">Open the Microsoft Recovery Services agent.</span></span>

5. <span data-ttu-id="be604-302">Kattintson a **biztonsági mentés ütemezése** majd **következő** mindaddig, amíg a módosítások megjelenése.</span><span class="sxs-lookup"><span data-stu-id="be604-302">Click **Schedule Backup** and then click **Next** until you see the changes reflected.</span></span>

6. <span data-ttu-id="be604-303">Kattintson a **Befejezés** a módosítások életbe léptetéséhez.</span><span class="sxs-lookup"><span data-stu-id="be604-303">Click **Finish** to apply the changes.</span></span>


## <a name="questions"></a><span data-ttu-id="be604-304">Kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="be604-304">Questions?</span></span>
<span data-ttu-id="be604-305">Ha kérdései vannak, vagy van olyan szolgáltatás, amelyről hallani szeretne, [küldjön visszajelzést](http://aka.ms/azurebackup_feedback).</span><span class="sxs-lookup"><span data-stu-id="be604-305">If you have questions, or if there is any feature that you would like to see included, [send us feedback](http://aka.ms/azurebackup_feedback).</span></span>

## <a name="next-steps"></a><span data-ttu-id="be604-306">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="be604-306">Next steps</span></span>
* <span data-ttu-id="be604-307">További részletek a [Windows rendszerű gépek biztonsági mentéséről](backup-configure-vault.md).</span><span class="sxs-lookup"><span data-stu-id="be604-307">Get more details about [backing up Windows machines](backup-configure-vault.md).</span></span>
* <span data-ttu-id="be604-308">Most, hogy biztonsági másolatot készített a fájlokról és mappákról, [kezelheti a tárlókat és a kiszolgálókat](backup-azure-manage-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="be604-308">Now that you've backed up your files and folders, you can [manage your vaults and servers](backup-azure-manage-windows-server.md).</span></span>
* <span data-ttu-id="be604-309">Ha vissza kell állítania egy biztonsági másolatot, ezzel a cikkel [állíthat vissza fájlokat Windows rendszerű gépre](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="be604-309">If you need to restore a backup, use this article to [restore files to a Windows machine](backup-azure-restore-windows-server.md).</span></span>
