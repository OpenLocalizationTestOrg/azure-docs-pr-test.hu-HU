---
title: "aaaBack fel Windows fájlok és mappák tooAzure (Resource Manager) |} Microsoft Docs"
description: "Ismerje meg, hogy fel egy erőforrás-kezelő központi telepítés Windows-fájlok és mappák tooAzure tooback."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "Hogyan toobackup; Hogyan tooback; a biztonságimásolat-fájlok és mappák"
ms.assetid: 5b15ebf1-2214-4722-b937-96e2be8872bb
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 8/15/2017
ms.author: markgal;
ms.openlocfilehash: 07d6580a84d5092ed2c61bf86ff5fcb148423ef2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="first-look-back-up-files-and-folders-in-resource-manager-deployment"></a><span data-ttu-id="e7e8b-104">Áttekintés: Fájlok és mappák biztonsági mentése a Resource Manager-alapú üzemelő példányban</span><span class="sxs-lookup"><span data-stu-id="e7e8b-104">First look: back up files and folders in Resource Manager deployment</span></span>
<span data-ttu-id="e7e8b-105">Ez a cikk azt ismerteti, hogyan készíthet a Windows Server (és a Windows-számítógép) tooback fájlok és mappák tooAzure erőforrás-kezelő központi telepítéssel.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-105">This article explains how tooback up your Windows Server (or Windows computer) files and folders tooAzure using a Resource Manager deployment.</span></span> <span data-ttu-id="e7e8b-106">Egy oktatóanyag tervezett toowalk hello alapokat nyújt.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-106">It's a tutorial intended toowalk you through hello basics.</span></span> <span data-ttu-id="e7e8b-107">Ha azt szeretné, hogy tooget lépések az Azure Backup használatával, éppen hello megfelelő helyen.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-107">If you want tooget started using Azure Backup, you're in hello right place.</span></span>

<span data-ttu-id="e7e8b-108">Ha azt szeretné, hogy további információk az Azure Backup tooknow, olvassa el ezt [áttekintése](backup-introduction-to-azure-backup.md).</span><span class="sxs-lookup"><span data-stu-id="e7e8b-108">If you want tooknow more about Azure Backup, read this [overview](backup-introduction-to-azure-backup.md).</span></span>

<span data-ttu-id="e7e8b-109">Ha még nincs Azure-előfizetése, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/), amellyel bármely Azure-szolgáltatást elérhet.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-109">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/) that lets you access any Azure service.</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="e7e8b-110">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="e7e8b-110">Create a recovery services vault</span></span>
<span data-ttu-id="e7e8b-111">a fájlok és mappák tooback, meg kell toocreate hello régióban, ahová toostore hello adatokat Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-111">tooback up your files and folders, you need toocreate a Recovery Services vault in hello region where you want toostore hello data.</span></span> <span data-ttu-id="e7e8b-112">Meg kell toodetermine a replikált tároló módját is.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-112">You also need toodetermine how you want your storage replicated.</span></span>

### <a name="toocreate-a-recovery-services-vault"></a><span data-ttu-id="e7e8b-113">Recovery Services-tároló toocreate</span><span class="sxs-lookup"><span data-stu-id="e7e8b-113">toocreate a Recovery Services vault</span></span>
1. <span data-ttu-id="e7e8b-114">Ha még nem tette meg, jelentkezzen be toohello [Azure Portal](https://portal.azure.com/) használata az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-114">If you haven't already done so, sign in toohello [Azure Portal](https://portal.azure.com/) using your Azure subscription.</span></span>
2. <span data-ttu-id="e7e8b-115">Hello központ menüben kattintson a **további szolgáltatások** hello az erőforrások listájához, írja be a **Recovery Services** kattintson **Recovery Services-tárolók**.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-115">On hello Hub menu, click **More services** and in hello list of resources, type **Recovery Services** and click **Recovery Services vaults**.</span></span>

    ![Recovery Services-tároló létrehozása – 1. lépés](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    <span data-ttu-id="e7e8b-117">Ha nincsenek recovery services-tárolók hello az előfizetést, hello tárolók vannak felsorolva.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-117">If there are recovery services vaults in hello subscription, hello vaults are listed.</span></span>
3. <span data-ttu-id="e7e8b-118">A hello **Recovery Services-tárolók** menüben kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-118">On hello **Recovery Services vaults** menu, click **Add**.</span></span>

    ![Recovery Services-tároló létrehozása – 2. lépés](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    <span data-ttu-id="e7e8b-120">hello Recovery Services tároló panel nyílik tooprovide kéri egy **neve**, **előfizetés**, **erőforráscsoport**, és **hely**.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-120">hello Recovery Services vault blade opens, prompting you tooprovide a **Name**, **Subscription**, **Resource group**, and **Location**.</span></span>

    ![Recovery Services-tároló létrehozása – 3. lépés](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. <span data-ttu-id="e7e8b-122">A **neve**, adjon meg egy rövid nevet tooidentify hello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-122">For **Name**, enter a friendly name tooidentify hello vault.</span></span> <span data-ttu-id="e7e8b-123">hello nevének kell toobe egyedi hello Azure-előfizetés esetében.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-123">hello name needs toobe unique for hello Azure subscription.</span></span> <span data-ttu-id="e7e8b-124">Írjon be egy 2–50 karakter hosszúságú nevet.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-124">Type a name that contains between 2 and 50 characters.</span></span> <span data-ttu-id="e7e8b-125">Ennek egy betűvel kell kezdődnie, és csak betűket, számokat és kötőjeleket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-125">It must start with a letter, and can contain only letters, numbers, and hyphens.</span></span>

5. <span data-ttu-id="e7e8b-126">A hello **előfizetés** területen hello legördülő menü toochoose hello Azure-előfizetés használatára.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-126">In hello **Subscription** section, use hello drop-down menu toochoose hello Azure subscription.</span></span> <span data-ttu-id="e7e8b-127">Ha csak egyetlen előfizetéssel, előfizetés megjelenő használja, és tovább toohello a lépést kihagyhatja.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-127">If you use only one subscription, that subscription appears and you can skip toohello next step.</span></span> <span data-ttu-id="e7e8b-128">Ha nem biztos abban, hogy melyik előfizetéssel toouse, használhatja az alapértelmezettet hello (vagy javasolt) előfizetés.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-128">If you are not sure which subscription toouse, use hello default (or suggested) subscription.</span></span> <span data-ttu-id="e7e8b-129">Csak akkor lesz több választási lehetőség, ha a szervezetéhez tartozó fiók több Azure-előfizetéssel van összekötve.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-129">There are multiple choices only if your organizational account is associated with multiple Azure subscriptions.</span></span>

6. <span data-ttu-id="e7e8b-130">A hello **erőforráscsoport** szakasz:</span><span class="sxs-lookup"><span data-stu-id="e7e8b-130">In hello **Resource group** section:</span></span>

    * <span data-ttu-id="e7e8b-131">Válassza ki **hozzon létre új** Ha azt szeretné, hogy toocreate egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-131">select **Create new** if you want toocreate a new Resource group.</span></span>
    <span data-ttu-id="e7e8b-132">Vagy</span><span class="sxs-lookup"><span data-stu-id="e7e8b-132">Or</span></span>
    * <span data-ttu-id="e7e8b-133">Válassza ki **meglévő** kattintson hello legördülő menü toosee hello elérhető erőforráscsoportok listáját.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-133">select **Use existing** and click hello drop-down menu toosee hello available list of Resource groups.</span></span>

  <span data-ttu-id="e7e8b-134">Az erőforráscsoportok, tanulmányozza hello [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e7e8b-134">For complete information on Resource groups, see hello [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

7. <span data-ttu-id="e7e8b-135">Kattintson a **hely** tooselect hello földrajzi régióban hello tároló.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-135">Click **Location** tooselect hello geographic region for hello vault.</span></span> <span data-ttu-id="e7e8b-136">Ez a beállítás meghatározza, hogy hello földrajzi régiót, ahol a biztonsági mentési adatokat küldi el.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-136">This choice determines hello geographic region where your backup data is sent.</span></span>

8. <span data-ttu-id="e7e8b-137">A Recovery Services-tároló panel hello hello alján kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-137">At hello bottom of hello Recovery Services vault blade, click **Create**.</span></span>

    <span data-ttu-id="e7e8b-138">Recovery Services-tároló létrehozása toobe hello több percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-138">It can take several minutes for hello Recovery Services vault toobe created.</span></span> <span data-ttu-id="e7e8b-139">Hello állapot értesítések hello felső jobb területen hello portál figyelése.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-139">Monitor hello status notifications in hello upper right-hand area of hello portal.</span></span> <span data-ttu-id="e7e8b-140">A tároló létrehozása után hello Recovery Services-tárolók listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-140">Once your vault is created, it appears in hello list of Recovery Services vaults.</span></span> <span data-ttu-id="e7e8b-141">Ha több perc után sem látja a tárolót, kattintson a **Frissítés** gombra.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-141">If after several minutes you don't see your vault, click **Refresh**.</span></span>

    ![Kattintson a Frissítés gombra](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    <span data-ttu-id="e7e8b-143">A tároló a Recovery Services-tárolók hello listája jelenik meg, ha készen áll a tooset hello adattároló redundanciája, amely áll.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-143">Once you see your vault in hello list of Recovery Services vaults, you are ready tooset hello storage redundancy.</span></span>

### <a name="set-storage-redundancy-for-hello-vault"></a><span data-ttu-id="e7e8b-144">Állítsa be az adattároló redundanciája, amely hello tároló</span><span class="sxs-lookup"><span data-stu-id="e7e8b-144">Set storage redundancy for hello vault</span></span>
<span data-ttu-id="e7e8b-145">Recovery Services-tároló létrehozásakor ellenőrizze, hogy adattároló redundanciája, amely konfigurált hello igényeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-145">When you create a Recovery Services vault, make sure storage redundancy is configured hello way you want.</span></span>

1. <span data-ttu-id="e7e8b-146">A hello **Recovery Services-tárolók** paneljén kattintson hello új tárolóra.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-146">From hello **Recovery Services vaults** blade, click hello new vault.</span></span>

    ![Válassza ki az új tároló hello a hello Recovery Services-tároló](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    <span data-ttu-id="e7e8b-148">Hello tároló kiválasztásakor hello **Recovery Services-tároló** panel narrows és hello-beállítások panel (*hello tároló nevére hello hello felső tartalmaz*) és hello tároló Részletek panel megnyitása.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-148">When you select hello vault, hello **Recovery Services vault** blade narrows, and hello Settings blade (*which has hello name of hello vault at hello top*) and hello vault details blade open.</span></span>

    ![Hello új tároló tárolási konfiguráció megtekintése](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)
2. <span data-ttu-id="e7e8b-150">Hello új tároló-beállítások panelen hello függőleges diák tooscroll le toohello kezelése szakasz használja, és kattintson **biztonsági infrastruktúra**.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-150">In hello new vault's Settings blade, use hello vertical slide tooscroll down toohello Manage section, and click **Backup Infrastructure**.</span></span>
    <span data-ttu-id="e7e8b-151">hello biztonsági infrastruktúra panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-151">hello Backup Infrastructure blade opens.</span></span>
3. <span data-ttu-id="e7e8b-152">Hello biztonsági infrastruktúra paneljén kattintson **biztonsági mentési konfigurációhoz** tooopen hello **biztonsági mentési konfigurációhoz** panelen.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-152">In hello Backup Infrastructure blade, click **Backup Configuration** tooopen hello **Backup Configuration** blade.</span></span>

    ![Új tároló hello tárolási konfiguráció beállítása](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)
4. <span data-ttu-id="e7e8b-154">Válassza ki a hello megfelelő tárolási replikációs beállítás a tároló számára.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-154">Choose hello appropriate storage replication option for your vault.</span></span>

    ![a tároló konfigurálásának lehetőségei](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    <span data-ttu-id="e7e8b-156">Alapértelmezés szerint a tárolója georedundáns tárolással rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-156">By default, your vault has geo-redundant storage.</span></span> <span data-ttu-id="e7e8b-157">Azure biztonságimásolat-tároláshoz-végpontként használatakor, továbbra is toouse **georedundáns**.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-157">If you use Azure as a primary backup storage endpoint, continue toouse **Geo-redundant**.</span></span> <span data-ttu-id="e7e8b-158">Ha nem adja meg Azure biztonságimásolat-tároláshoz-végpontként, majd válasszon **helyileg redundáns**, amely csökkenti a hello Azure storage költségei.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-158">If you don't use Azure as a primary backup storage endpoint, then choose **Locally-redundant**, which reduces hello Azure storage costs.</span></span> <span data-ttu-id="e7e8b-159">A [georedundáns](../storage/common/storage-redundancy.md#geo-redundant-storage) és a [helyileg redundáns](../storage/common/storage-redundancy.md#locally-redundant-storage) tárolási lehetőségekről többet olvashat ebben a [Tárhely-redundancia áttekintésben](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="e7e8b-159">Read more about [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) and [locally redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) storage options in this [Storage redundancy overview](../storage/common/storage-redundancy.md).</span></span>

<span data-ttu-id="e7e8b-160">A tároló létrehozása után konfigurálja azt a fájlok és mappák biztonsági mentésére.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-160">Now that you've created a vault, configure it for backing up files and folders.</span></span>

## <a name="configure-hello-vault"></a><span data-ttu-id="e7e8b-161">Hello tároló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e7e8b-161">Configure hello vault</span></span>
1. <span data-ttu-id="e7e8b-162">A Recovery Services tároló paneljén (hello tároló most létrehozott), a Bevezetés című szakaszt hello hello, kattintson a **biztonsági mentés**, végül a hello **Ismerkedés a biztonsági mentés** panelen válassza  **Biztonsági mentési cél**.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-162">On hello Recovery Services vault blade (for hello vault you just created), in hello Getting Started section, click **Backup**, then on hello **Getting Started with Backup** blade, select **Backup goal**.</span></span>

    ![A biztonsági mentés célja panel megnyitása](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

    <span data-ttu-id="e7e8b-164">Hello **biztonsági mentési cél** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-164">hello **Backup Goal** blade opens.</span></span>

    ![A biztonsági mentés célja panel megnyitása](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. <span data-ttu-id="e7e8b-166">A hello **a számítási feladatok futtató?** legördülő menüben válassza **helyszíni**.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-166">From hello **Where is your workload running?** drop-down menu, select **On-premises**.</span></span>

    <span data-ttu-id="e7e8b-167">Azért kell a **Helyszíni** lehetőséget választania, mert a Windows Server- vagy Windows-számítógépe egy fizikai gép, amely nem az Azure része.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-167">You choose **On-premises** because your Windows Server or Windows computer is a physical machine that is not in Azure.</span></span>

3. <span data-ttu-id="e7e8b-168">A hello **miről szeretné, hogy toobackup?** menüjében válassza **fájlok és mappák**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-168">From hello **What do you want toobackup?** menu, select **Files and folders**, and click **OK**.</span></span>

    ![Fájlok és mappák konfigurálása](./media/backup-try-azure-backup-in-10-mins/set-file-folder.png)

    <span data-ttu-id="e7e8b-170">Az OK gombra kattintva jelölje megjelenése után következő túl**biztonsági mentési cél**, és hello **infrastruktúra előkészítése** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-170">After clicking OK, a checkmark appears next too**Backup goal**, and hello **Prepare infrastructure** blade opens.</span></span>

    ![Ha konfigurálta a biztonsági mentés célját, a következő lépés az infrastruktúra előkészítése](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. <span data-ttu-id="e7e8b-172">A hello **infrastruktúra előkészítése** panelen kattintson a **letöltése ügynök Windows vagy a Windows ügyfél**.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-172">On hello **Prepare infrastructure** blade, click **Download Agent for Windows Server or Windows Client**.</span></span>

    ![infrastruktúra előkészítése](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

    <span data-ttu-id="e7e8b-174">Ha a Windows Server alapvető használ, majd válassza a toodownload hello ügynök a Windows Server alapvető.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-174">If you are using Windows Server Essential, then choose toodownload hello agent for Windows Server Essential.</span></span> <span data-ttu-id="e7e8b-175">Felbukkanó toorun kéri, vagy MARSAgentInstaller.exe menti.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-175">A pop-up menu prompts you toorun or save MARSAgentInstaller.exe.</span></span>

    ![MARSAgentInstaller párbeszédpanel](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. <span data-ttu-id="e7e8b-177">Hello letöltési megjelenő menüben, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-177">In hello download pop-up menu, click **Save**.</span></span>

    <span data-ttu-id="e7e8b-178">Alapértelmezés szerint hello **MARSagentinstaller.exe** fájl tooyour Letöltések mappába kerül.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-178">By default, hello **MARSagentinstaller.exe** file is saved tooyour Downloads folder.</span></span> <span data-ttu-id="e7e8b-179">Hello telepítő befejezése után megjelenik egy előugró ablak, amely rákérdez, ha szeretné, hogy toorun hello telepítő, vagy nyissa meg a hello mappa.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-179">When hello installer completes, you will see a pop-up asking if you want toorun hello installer, or open hello folder.</span></span>

    ![infrastruktúra előkészítése](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

    <span data-ttu-id="e7e8b-181">Tooinstall hello ügynök még nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-181">You don't need tooinstall hello agent yet.</span></span> <span data-ttu-id="e7e8b-182">Hello ügynök is telepíthet, miután letöltötte a hello tárolói hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-182">You can install hello agent after you have downloaded hello vault credentials.</span></span>

6. <span data-ttu-id="e7e8b-183">A hello **infrastruktúra előkészítése** panelen kattintson a **letöltése**.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-183">On hello **Prepare infrastructure** blade, click **Download**.</span></span>

    ![a tároló hitelesítő adatainak letöltése](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

    <span data-ttu-id="e7e8b-185">hello tárolói hitelesítő adatok letöltése tooyour Letöltések mappába.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-185">hello vault credentials download tooyour Downloads folder.</span></span> <span data-ttu-id="e7e8b-186">Hello tárolói hitelesítő adatokat, akkor letöltés befejezése után megjelenik egy előugró ablak, amely rákérdez, ha azt szeretné tooopen, vagy hello hitelesítő adatok mentése.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-186">After hello vault credentials finish downloading, you see a pop-up asking if you want tooopen or save hello credentials.</span></span> <span data-ttu-id="e7e8b-187">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-187">Click **Save**.</span></span> <span data-ttu-id="e7e8b-188">Ha véletlenül kattintson **nyitott**, lehetővé teszik a hello párbeszédpanel érintő tooopen hello tárolói hitelesítő adatokat, a művelet sikertelen.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-188">If you accidentally click **Open**, let hello dialog that attempts tooopen hello vault credentials, fail.</span></span> <span data-ttu-id="e7e8b-189">Hello tárolói hitelesítő adatok nem nyitható meg.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-189">You cannot open hello vault credentials.</span></span> <span data-ttu-id="e7e8b-190">A folytatáshoz toohello következő lépésre.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-190">Proceed toohello next step.</span></span> <span data-ttu-id="e7e8b-191">hello tárolói hitelesítő adatok vannak hello Letöltések mappába.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-191">hello vault credentials are in hello Downloads folder.</span></span>   

    ![a tároló hitelesítő adatainak letöltése befejeződött](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)

## <a name="install-and-register-hello-agent"></a><span data-ttu-id="e7e8b-193">Telepítse és regisztrálja a hello ügynök</span><span class="sxs-lookup"><span data-stu-id="e7e8b-193">Install and register hello agent</span></span>

> [!NOTE]
> <span data-ttu-id="e7e8b-194">Lehetővé teszi biztonsági mentés hello Azure-portálon keresztül még nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-194">Enabling backup through hello Azure portal is not available, yet.</span></span> <span data-ttu-id="e7e8b-195">A fájlok és mappák hello Microsoft Azure Recovery Services Agent tooback használja.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-195">Use hello Microsoft Azure Recovery Services Agent tooback up your files and folders.</span></span>
>

1. <span data-ttu-id="e7e8b-196">Keresse meg és kattintson duplán a hello **MARSagentinstaller.exe** hello Letöltések mappába (vagy más mentett helyről).</span><span class="sxs-lookup"><span data-stu-id="e7e8b-196">Locate and double-click hello **MARSagentinstaller.exe** from hello Downloads folder (or other saved location).</span></span>

    <span data-ttu-id="e7e8b-197">hello telepítője biztosítja az üzenetből bontja ki, mert telepíti, és regisztrálja hello Recovery Services Agent ügynököt.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-197">hello installer provides a series of messages as it extracts, installs, and registers hello Recovery Services agent.</span></span>

    ![a Recovery Services-ügynök telepítőjének futtatása, hitelesítő adatok](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. <span data-ttu-id="e7e8b-199">Hello Microsoft Azure Recovery Services ügynök telepítő varázsló befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-199">Complete hello Microsoft Azure Recovery Services Agent Setup Wizard.</span></span> <span data-ttu-id="e7e8b-200">toocomplete hello varázsló kell:</span><span class="sxs-lookup"><span data-stu-id="e7e8b-200">toocomplete hello wizard, you need to:</span></span>

   * <span data-ttu-id="e7e8b-201">Válassza ki a hello telepítési és a gyorsítótár mappa.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-201">Choose a location for hello installation and cache folder.</span></span>
   * <span data-ttu-id="e7e8b-202">Adja meg a proxykiszolgáló kiszolgálóadatok használatakor a proxy server tooconnect toohello internet.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-202">Provide your proxy server info if you use a proxy server tooconnect toohello internet.</span></span>
   * <span data-ttu-id="e7e8b-203">Adja meg a felhasználónevének és jelszavának részleteit, ha hitelesített proxyt használ.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-203">Provide your user name and password details if you use an authenticated proxy.</span></span>
   * <span data-ttu-id="e7e8b-204">Adja meg a letöltött hello tárolói hitelesítő adatokat</span><span class="sxs-lookup"><span data-stu-id="e7e8b-204">Provide hello downloaded vault credentials</span></span>
   * <span data-ttu-id="e7e8b-205">Hello titkosítási jelszó mentse egy biztonságos helyre.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-205">Save hello encryption passphrase in a secure location.</span></span>

     > [!NOTE]
     > <span data-ttu-id="e7e8b-206">Ha elveszíti vagy elfelejti hello jelszót, a Microsoft nem súgó hello biztonsági mentési adatok helyreállítását.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-206">If you lose or forget hello passphrase, Microsoft cannot help recover hello backup data.</span></span> <span data-ttu-id="e7e8b-207">Mentse a hello fájlt biztonságos helyen.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-207">Save hello file in a secure location.</span></span> <span data-ttu-id="e7e8b-208">A biztonsági mentés szükséges toorestore.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-208">It is required toorestore a backup.</span></span>
     >
     >

<span data-ttu-id="e7e8b-209">hello ügynök telepítve van, és a számítógép regisztrált toohello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-209">hello agent is now installed and your machine is registered toohello vault.</span></span> <span data-ttu-id="e7e8b-210">Most készen áll a tooconfigure, és a biztonsági mentés ütemezése.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-210">You're ready tooconfigure and schedule your backup.</span></span>

## <a name="network-and-connectivity-requirements"></a><span data-ttu-id="e7e8b-211">Hálózati és kapcsolati követelmények</span><span class="sxs-lookup"><span data-stu-id="e7e8b-211">Network and Connectivity Requirements</span></span>

<span data-ttu-id="e7e8b-212">Ha a machine /-proxy korlátozott internet-hozzáféréssel, győződjön meg arról, hogy tűzfal beállításait a hello mcahine /-proxy konfigurált tooallow hello következő URL-címek:</span><span class="sxs-lookup"><span data-stu-id="e7e8b-212">If your machine/proxy has limited internet access, ensure that firewall settings on hello mcahine/proxy are configured tooallow hello following URLs:</span></span> <br>
    1. <span data-ttu-id="e7e8b-213">www.msftncsi.com</span><span class="sxs-lookup"><span data-stu-id="e7e8b-213">www.msftncsi.com</span></span>
    2. <span data-ttu-id="e7e8b-214">*.Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="e7e8b-214">*.Microsoft.com</span></span>
    3. <span data-ttu-id="e7e8b-215">*.WindowsAzure.com</span><span class="sxs-lookup"><span data-stu-id="e7e8b-215">*.WindowsAzure.com</span></span>
    4. <span data-ttu-id="e7e8b-216">*.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="e7e8b-216">*.microsoftonline.com</span></span>
    5. <span data-ttu-id="e7e8b-217">*.windows.ne</span><span class="sxs-lookup"><span data-stu-id="e7e8b-217">*.windows.ne</span></span>

## <a name="back-up-your-files-and-folders"></a><span data-ttu-id="e7e8b-218">Biztonsági másolat készítése a fájlokról és mappákról</span><span class="sxs-lookup"><span data-stu-id="e7e8b-218">Back up your files and folders</span></span>
<span data-ttu-id="e7e8b-219">hello kezdeti biztonsági másolatot a két fő feladatokból áll:</span><span class="sxs-lookup"><span data-stu-id="e7e8b-219">hello initial backup includes two key tasks:</span></span>

* <span data-ttu-id="e7e8b-220">Hello biztonsági mentés ütemezése</span><span class="sxs-lookup"><span data-stu-id="e7e8b-220">Schedule hello backup</span></span>
* <span data-ttu-id="e7e8b-221">Fájlok és mappák biztonsági mentése a hello első alkalommal</span><span class="sxs-lookup"><span data-stu-id="e7e8b-221">Back up files and folders for hello first time</span></span>

<span data-ttu-id="e7e8b-222">toocomplete hello kezdeti biztonsági másolatot, használjon hello Microsoft Azure Recovery Services Agent ügynököt.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-222">toocomplete hello initial backup, use hello Microsoft Azure Recovery Services agent.</span></span>

### <a name="tooschedule-hello-backup-job"></a><span data-ttu-id="e7e8b-223">tooschedule hello biztonsági mentési feladat</span><span class="sxs-lookup"><span data-stu-id="e7e8b-223">tooschedule hello backup job</span></span>
1. <span data-ttu-id="e7e8b-224">Nyissa meg a hello Microsoft Azure Recovery Services Agent ügynököt.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-224">Open hello Microsoft Azure Recovery Services agent.</span></span> <span data-ttu-id="e7e8b-225">A megkereséséhez keressen rá a gépen a **Microsoft Azure Backup** kifejezésre.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-225">You can find it by searching your machine for **Microsoft Azure Backup**.</span></span>

    ![Indítsa el a hello Azure Recovery Services Agent ügynök](./media/backup-try-azure-backup-in-10-mins/snap-in-search.png)
2. <span data-ttu-id="e7e8b-227">Hello Recovery Services Agent ügynököt, kattintson **biztonsági mentés ütemezése**.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-227">In hello Recovery Services agent, click **Schedule Backup**.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)
3. <span data-ttu-id="e7e8b-229">A hello bevezetés hello ütemezett biztonsági mentés varázsló lapján, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-229">On hello Getting started page of hello Schedule Backup Wizard, click **Next**.</span></span>
4. <span data-ttu-id="e7e8b-230">Kattintson hello elemek kijelölése tooBackup lap **elemek hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-230">On hello Select Items tooBackup page, click **Add Items**.</span></span>
5. <span data-ttu-id="e7e8b-231">Válassza ki a hello fájlok és mappák tooback szeretné, hogy fel, és kattintson a **gépházban**.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-231">Select hello files and folders that you want tooback up, and then click **Okay**.</span></span>
6. <span data-ttu-id="e7e8b-232">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-232">Click **Next**.</span></span>
7. <span data-ttu-id="e7e8b-233">A hello **adja meg a biztonsági mentés ütemezése** adja meg azokat a hello **biztonsági mentés ütemezése** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-233">On hello **Specify Backup Schedule** page, specify hello **backup schedule** and click **Next**.</span></span>

    <span data-ttu-id="e7e8b-234">Napi (legfeljebb napi háromszori) vagy heti biztonsági mentéseket ütemezhet.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-234">You can schedule daily (at a maximum rate of three times per day) or weekly backups.</span></span>

    ![Windows Server biztonsági mentés elemei](./media/backup-try-azure-backup-in-10-mins/specify-backup-schedule-close.png)

   > [!NOTE]
   > <span data-ttu-id="e7e8b-236">Hogyan toospecify hello biztonsági mentés ütemezése kapcsolatos további információkért lásd: hello cikk [használata Azure biztonsági mentési tooreplace a szalag infrastruktúra](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="e7e8b-236">For more information about how toospecify hello backup schedule, see hello article [Use Azure Backup tooreplace your tape infrastructure](backup-azure-backup-cloud-as-tape.md).</span></span>
   >

8. <span data-ttu-id="e7e8b-237">A hello **válassza ki az adatmegőrzési** lapra, jelölje be hello **adatmegőrzési** a hello biztonsági másolatot.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-237">On hello **Select Retention Policy** page, select hello **Retention Policy** for hello backup copy.</span></span>

    <span data-ttu-id="e7e8b-238">hello adatmegőrzési határozza meg, mennyi ideig hello biztonsági mentési adatokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-238">hello retention policy specifies how long hello backup data is stored.</span></span> <span data-ttu-id="e7e8b-239">Ahelyett, hogy adja meg az összes biztonsági mentési pont "egyszerű policy", a másik adatmegőrzési hello biztonsági mentés esetén alapján is megadhat.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-239">Rather than specifying a “flat policy” for all backup points, you can specify different retention policies based on when hello backup occurs.</span></span> <span data-ttu-id="e7e8b-240">Hello napi, heti, havi és éves megőrzési házirendek toomeet módosíthatja az igényeinek.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-240">You can modify hello daily, weekly, monthly, and yearly retention policies toomeet your needs.</span></span>
9. <span data-ttu-id="e7e8b-241">Hello kezdeti biztonsági mentési típusának kiválasztása lapon válassza a hello kezdeti biztonsági mentés típusát.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-241">On hello Choose Initial Backup Type page, choose hello initial backup type.</span></span> <span data-ttu-id="e7e8b-242">Hagyja hello beállítást **automatikusan hello hálózaton keresztül** kiválasztva, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-242">Leave hello option **Automatically over hello network** selected, and then click **Next**.</span></span>

    <span data-ttu-id="e7e8b-243">Biztonsági másolatot készíthet automatikusan hello hálózaton keresztül, vagy a biztonsági mentést készíthet offline állapotba.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-243">You can back up automatically over hello network, or you can back up offline.</span></span> <span data-ttu-id="e7e8b-244">hello a cikk hátralévő része a biztonsági mentési automatikusan hello folyamatot írja le.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-244">hello remainder of this article describes hello process for backing up automatically.</span></span> <span data-ttu-id="e7e8b-245">Ha jobban szeret toodo offline biztonsági másolat, tekintse át a hello cikk [az Azure Backup Offline biztonsági másolat munkafolyamat](backup-azure-backup-import-export.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-245">If you prefer toodo an offline backup, review hello article [Offline backup workflow in Azure Backup](backup-azure-backup-import-export.md) for additional information.</span></span>
10. <span data-ttu-id="e7e8b-246">A hello megerősítése lapon tekintse át hello adatokat, és kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-246">On hello Confirmation page, review hello information, and then click **Finish**.</span></span>
11. <span data-ttu-id="e7e8b-247">Hello biztonsági mentési ütemezés létrehozása hello varázsló befejezése után kattintson **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-247">After hello wizard finishes creating hello backup schedule, click **Close**.</span></span>

### <a name="tooback-up-files-and-folders-for-hello-first-time"></a><span data-ttu-id="e7e8b-248">az első alkalommal hello mappák és fájlok tooback</span><span class="sxs-lookup"><span data-stu-id="e7e8b-248">tooback up files and folders for hello first time</span></span>
1. <span data-ttu-id="e7e8b-249">Hello Recovery Services Agent ügynököt, kattintson **biztonsági másolat készítése most** toocomplete hello kezdeti összehangolása hello hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-249">In hello Recovery Services agent, click **Back Up Now** toocomplete hello initial seeding over hello network.</span></span>

    ![Windows Server biztonsági másolat készítése](./media/backup-try-azure-backup-in-10-mins/backup-now.png)
2. <span data-ttu-id="e7e8b-251">A hello megerősítése lapon, amely a biztonsági mentést most varázsló hello hello beállítások áttekintése hello gép tooback fogja használni.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-251">On hello Confirmation page, review hello settings that hello Back Up Now Wizard will use tooback up hello machine.</span></span> <span data-ttu-id="e7e8b-252">Ezután kattintson a **Biztonsági mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-252">Then click **Back Up**.</span></span>
3. <span data-ttu-id="e7e8b-253">Kattintson a **Bezárás** tooclose hello varázsló.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-253">Click **Close** tooclose hello wizard.</span></span> <span data-ttu-id="e7e8b-254">Ha a biztonsági mentési folyamat hello befejeződése előtt bezárja hello varázslót, hello varázsló toorun hello háttérben folytatódik.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-254">If you close hello wizard before hello backup process finishes, hello wizard continues toorun in hello background.</span></span>

<span data-ttu-id="e7e8b-255">Hello kezdeti biztonsági mentés befejezése után hello **feladata befejezve** állapota megjelenik hello biztonsági mentés konzolban.</span><span class="sxs-lookup"><span data-stu-id="e7e8b-255">After hello initial backup is completed, hello **Job completed** status appears in hello Backup console.</span></span>

![IR befejezve](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## <a name="questions"></a><span data-ttu-id="e7e8b-257">Kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="e7e8b-257">Questions?</span></span>
<span data-ttu-id="e7e8b-258">Ha kérdése van, vagy ha bármely új szolgáltatása része, toosee szeretné [visszajelzést küldhet](http://aka.ms/azurebackup_feedback).</span><span class="sxs-lookup"><span data-stu-id="e7e8b-258">If you have questions, or if there is any feature that you would like toosee included, [send us feedback](http://aka.ms/azurebackup_feedback).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e7e8b-259">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e7e8b-259">Next steps</span></span>
* <span data-ttu-id="e7e8b-260">További részletek a [Windows rendszerű gépek biztonsági mentéséről](backup-configure-vault.md).</span><span class="sxs-lookup"><span data-stu-id="e7e8b-260">Get more details about [backing up Windows machines](backup-configure-vault.md).</span></span>
* <span data-ttu-id="e7e8b-261">Most, hogy biztonsági másolatot készített a fájlokról és mappákról, [kezelheti a tárlókat és a kiszolgálókat](backup-azure-manage-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="e7e8b-261">Now that you've backed up your files and folders, you can [manage your vaults and servers](backup-azure-manage-windows-server.md).</span></span>
* <span data-ttu-id="e7e8b-262">Ha szükség toorestore biztonsági másolat, akkor ez a cikk túl[fájlok tooa Windows számítógép visszaállítása](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="e7e8b-262">If you need toorestore a backup, use this article too[restore files tooa Windows machine](backup-azure-restore-windows-server.md).</span></span>
