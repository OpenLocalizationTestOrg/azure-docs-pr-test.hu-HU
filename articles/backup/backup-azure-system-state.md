---
title: "mentése a Windows rendszer állapota tooAzure aaaBack |} Microsoft Docs"
description: "Ismerje meg a Windows Server és/vagy a Windows-számítógépek tooAzure hello rendszerállapotot tooback."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: carmonm
editor: 
keywords: "Hogyan toobackup; Hogyan tooback; a biztonságimásolat-fájlok és mappák"
ms.assetid: 5b15ebf1-2214-4722-b937-96e2be8872bb
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: saurse;markgal
ms.openlocfilehash: be5d4be81af981c10de82add9fe962a730753cf5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-windows-system-state-in-resource-manager-deployment"></a><span data-ttu-id="2e590-104">A Resource Manager telepítés Windows rendszerállapot biztonsági mentését</span><span class="sxs-lookup"><span data-stu-id="2e590-104">Back up Windows system state in Resource Manager deployment</span></span>
<span data-ttu-id="2e590-105">Ez a cikk azt ismerteti, hogyan tooback el a Windows Server rendszert állapot tooAzure.</span><span class="sxs-lookup"><span data-stu-id="2e590-105">This article explains how tooback up your Windows Server system state tooAzure.</span></span> <span data-ttu-id="2e590-106">Egy oktatóanyag tervezett toowalk hello alapokat nyújt.</span><span class="sxs-lookup"><span data-stu-id="2e590-106">It's a tutorial intended toowalk you through hello basics.</span></span>

<span data-ttu-id="2e590-107">Ha azt szeretné, hogy további információk az Azure Backup tooknow, olvassa el ezt [áttekintése](backup-introduction-to-azure-backup.md).</span><span class="sxs-lookup"><span data-stu-id="2e590-107">If you want tooknow more about Azure Backup, read this [overview](backup-introduction-to-azure-backup.md).</span></span>

<span data-ttu-id="2e590-108">Ha még nincs Azure-előfizetése, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/), amellyel bármely Azure-szolgáltatást elérhet.</span><span class="sxs-lookup"><span data-stu-id="2e590-108">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/) that lets you access any Azure service.</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="2e590-109">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="2e590-109">Create a recovery services vault</span></span>
<span data-ttu-id="2e590-110">a fájlok és mappák tooback, meg kell toocreate hello régióban, ahová toostore hello adatokat Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="2e590-110">tooback up your files and folders, you need toocreate a Recovery Services vault in hello region where you want toostore hello data.</span></span> <span data-ttu-id="2e590-111">Meg kell toodetermine a replikált tároló módját is.</span><span class="sxs-lookup"><span data-stu-id="2e590-111">You also need toodetermine how you want your storage replicated.</span></span>

### <a name="toocreate-a-recovery-services-vault"></a><span data-ttu-id="2e590-112">Recovery Services-tároló toocreate</span><span class="sxs-lookup"><span data-stu-id="2e590-112">toocreate a Recovery Services vault</span></span>
1. <span data-ttu-id="2e590-113">Ha még nem tette meg, jelentkezzen be toohello [Azure Portal](https://portal.azure.com/) használata az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="2e590-113">If you haven't already done so, sign in toohello [Azure Portal](https://portal.azure.com/) using your Azure subscription.</span></span>
2. <span data-ttu-id="2e590-114">Hello központ menüben kattintson a **további szolgáltatások** hello az erőforrások listájához, írja be a **Recovery Services** kattintson **Recovery Services-tárolók**.</span><span class="sxs-lookup"><span data-stu-id="2e590-114">On hello Hub menu, click **More services** and in hello list of resources, type **Recovery Services** and click **Recovery Services vaults**.</span></span>

    ![Recovery Services-tároló létrehozása – 1. lépés](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    <span data-ttu-id="2e590-116">Ha nincsenek recovery services-tárolók hello az előfizetést, hello tárolók vannak felsorolva.</span><span class="sxs-lookup"><span data-stu-id="2e590-116">If there are recovery services vaults in hello subscription, hello vaults are listed.</span></span>
3. <span data-ttu-id="2e590-117">A hello **Recovery Services-tárolók** menüben kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="2e590-117">On hello **Recovery Services vaults** menu, click **Add**.</span></span>

    ![Recovery Services-tároló létrehozása – 2. lépés](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    <span data-ttu-id="2e590-119">hello Recovery Services tároló panel nyílik tooprovide kéri egy **neve**, **előfizetés**, **erőforráscsoport**, és **hely**.</span><span class="sxs-lookup"><span data-stu-id="2e590-119">hello Recovery Services vault blade opens, prompting you tooprovide a **Name**, **Subscription**, **Resource group**, and **Location**.</span></span>

    ![Recovery Services-tároló létrehozása – 3. lépés](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. <span data-ttu-id="2e590-121">A **neve**, adjon meg egy rövid nevet tooidentify hello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="2e590-121">For **Name**, enter a friendly name tooidentify hello vault.</span></span> <span data-ttu-id="2e590-122">hello nevének kell toobe egyedi hello Azure-előfizetés esetében.</span><span class="sxs-lookup"><span data-stu-id="2e590-122">hello name needs toobe unique for hello Azure subscription.</span></span> <span data-ttu-id="2e590-123">Írjon be egy 2–50 karakter hosszúságú nevet.</span><span class="sxs-lookup"><span data-stu-id="2e590-123">Type a name that contains between 2 and 50 characters.</span></span> <span data-ttu-id="2e590-124">Ennek egy betűvel kell kezdődnie, és csak betűket, számokat és kötőjeleket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="2e590-124">It must start with a letter, and can contain only letters, numbers, and hyphens.</span></span>

5. <span data-ttu-id="2e590-125">A hello **előfizetés** területen hello legördülő menü toochoose hello Azure-előfizetés használatára.</span><span class="sxs-lookup"><span data-stu-id="2e590-125">In hello **Subscription** section, use hello drop-down menu toochoose hello Azure subscription.</span></span> <span data-ttu-id="2e590-126">Ha csak egyetlen előfizetéssel, előfizetés megjelenő használja, és tovább toohello a lépést kihagyhatja.</span><span class="sxs-lookup"><span data-stu-id="2e590-126">If you use only one subscription, that subscription appears and you can skip toohello next step.</span></span> <span data-ttu-id="2e590-127">Ha nem biztos abban, hogy melyik előfizetéssel toouse, használhatja az alapértelmezettet hello (vagy javasolt) előfizetés.</span><span class="sxs-lookup"><span data-stu-id="2e590-127">If you are not sure which subscription toouse, use hello default (or suggested) subscription.</span></span> <span data-ttu-id="2e590-128">Csak akkor lesz több választási lehetőség, ha a szervezetéhez tartozó fiók több Azure-előfizetéssel van összekötve.</span><span class="sxs-lookup"><span data-stu-id="2e590-128">There are multiple choices only if your organizational account is associated with multiple Azure subscriptions.</span></span>

6. <span data-ttu-id="2e590-129">A hello **erőforráscsoport** szakasz:</span><span class="sxs-lookup"><span data-stu-id="2e590-129">In hello **Resource group** section:</span></span>

    * <span data-ttu-id="2e590-130">Válassza ki **hozzon létre új** Ha azt szeretné, hogy toocreate egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="2e590-130">select **Create new** if you want toocreate a Resource group.</span></span>
    <span data-ttu-id="2e590-131">Vagy</span><span class="sxs-lookup"><span data-stu-id="2e590-131">Or</span></span>
    * <span data-ttu-id="2e590-132">Válassza ki **meglévő** kattintson hello legördülő menü toosee hello elérhető erőforráscsoportok listáját.</span><span class="sxs-lookup"><span data-stu-id="2e590-132">select **Use existing** and click hello drop-down menu toosee hello available list of Resource groups.</span></span>

  <span data-ttu-id="2e590-133">Az erőforráscsoportok, tanulmányozza hello [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2e590-133">For complete information on Resource groups, see hello [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

7. <span data-ttu-id="2e590-134">Kattintson a **hely** tooselect hello földrajzi régióban hello tároló.</span><span class="sxs-lookup"><span data-stu-id="2e590-134">Click **Location** tooselect hello geographic region for hello vault.</span></span> <span data-ttu-id="2e590-135">Ez a beállítás meghatározza, hogy hello földrajzi régiót, ahol a biztonsági mentési adatokat küldi el.</span><span class="sxs-lookup"><span data-stu-id="2e590-135">This choice determines hello geographic region where your backup data is sent.</span></span>

8. <span data-ttu-id="2e590-136">A Recovery Services-tároló panel hello hello alján kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="2e590-136">At hello bottom of hello Recovery Services vault blade, click **Create**.</span></span>

    <span data-ttu-id="2e590-137">Recovery Services-tároló létrehozása toobe hello több percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="2e590-137">It can take several minutes for hello Recovery Services vault toobe created.</span></span> <span data-ttu-id="2e590-138">Hello állapot értesítések hello felső jobb területen hello portál figyelése.</span><span class="sxs-lookup"><span data-stu-id="2e590-138">Monitor hello status notifications in hello upper right-hand area of hello portal.</span></span> <span data-ttu-id="2e590-139">A tároló létrehozása után hello Recovery Services-tárolók listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2e590-139">Once your vault is created, it appears in hello list of Recovery Services vaults.</span></span> <span data-ttu-id="2e590-140">Ha több perc után sem látja a tárolót, kattintson a **Frissítés** gombra.</span><span class="sxs-lookup"><span data-stu-id="2e590-140">If after several minutes you don't see your vault, click **Refresh**.</span></span>

    ![Kattintson a Frissítés gombra](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    <span data-ttu-id="2e590-142">A tároló a Recovery Services-tárolók hello listája jelenik meg, ha készen áll a tooset hello adattároló redundanciája, amely áll.</span><span class="sxs-lookup"><span data-stu-id="2e590-142">Once you see your vault in hello list of Recovery Services vaults, you are ready tooset hello storage redundancy.</span></span>

### <a name="set-storage-redundancy-for-hello-vault"></a><span data-ttu-id="2e590-143">Állítsa be az adattároló redundanciája, amely hello tároló</span><span class="sxs-lookup"><span data-stu-id="2e590-143">Set storage redundancy for hello vault</span></span>
<span data-ttu-id="2e590-144">Recovery Services-tároló létrehozásakor ellenőrizze, hogy adattároló redundanciája, amely konfigurált hello igényeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="2e590-144">When you create a Recovery Services vault, make sure storage redundancy is configured hello way you want.</span></span>

1. <span data-ttu-id="2e590-145">A hello **Recovery Services-tárolók** paneljén kattintson hello új tárolóra.</span><span class="sxs-lookup"><span data-stu-id="2e590-145">From hello **Recovery Services vaults** blade, click hello new vault.</span></span>

    ![Válassza ki az új tároló hello a hello Recovery Services-tároló](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    <span data-ttu-id="2e590-147">Hello tároló kiválasztásakor hello **Recovery Services-tároló** panel narrows és hello-beállítások panel (*hello tároló nevére hello hello felső tartalmaz*) és hello tároló Részletek panel megnyitása.</span><span class="sxs-lookup"><span data-stu-id="2e590-147">When you select hello vault, hello **Recovery Services vault** blade narrows, and hello Settings blade (*which has hello name of hello vault at hello top*) and hello vault details blade open.</span></span>

    ![Hello új tároló tárolási konfiguráció megtekintése](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)
2. <span data-ttu-id="2e590-149">Hello új tároló-beállítások panelen hello függőleges diák tooscroll le toohello kezelése szakasz használja, és kattintson **biztonsági infrastruktúra**.</span><span class="sxs-lookup"><span data-stu-id="2e590-149">In hello new vault's Settings blade, use hello vertical slide tooscroll down toohello Manage section, and click **Backup Infrastructure**.</span></span>
    <span data-ttu-id="2e590-150">hello biztonsági infrastruktúra panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="2e590-150">hello Backup Infrastructure blade opens.</span></span>
3. <span data-ttu-id="2e590-151">Hello biztonsági infrastruktúra paneljén kattintson **biztonsági mentési konfigurációhoz** tooopen hello **biztonsági mentési konfigurációhoz** panelen.</span><span class="sxs-lookup"><span data-stu-id="2e590-151">In hello Backup Infrastructure blade, click **Backup Configuration** tooopen hello **Backup Configuration** blade.</span></span>

    ![Új tároló hello tárolási konfiguráció beállítása](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)
4. <span data-ttu-id="2e590-153">Válassza ki a hello megfelelő tárolási replikációs beállítás a tároló számára.</span><span class="sxs-lookup"><span data-stu-id="2e590-153">Choose hello appropriate storage replication option for your vault.</span></span>

    ![a tároló konfigurálásának lehetőségei](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    <span data-ttu-id="2e590-155">Alapértelmezés szerint a tárolója georedundáns tárolással rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2e590-155">By default, your vault has geo-redundant storage.</span></span> <span data-ttu-id="2e590-156">Azure biztonságimásolat-tároláshoz-végpontként használatakor, továbbra is toouse **georedundáns**.</span><span class="sxs-lookup"><span data-stu-id="2e590-156">If you use Azure as a primary backup storage endpoint, continue toouse **Geo-redundant**.</span></span> <span data-ttu-id="2e590-157">Ha nem adja meg Azure biztonságimásolat-tároláshoz-végpontként, majd válasszon **helyileg redundáns**, amely csökkenti a hello Azure storage költségei.</span><span class="sxs-lookup"><span data-stu-id="2e590-157">If you don't use Azure as a primary backup storage endpoint, then choose **Locally-redundant**, which reduces hello Azure storage costs.</span></span> <span data-ttu-id="2e590-158">A [georedundáns](../storage/common/storage-redundancy.md#geo-redundant-storage) és a [helyileg redundáns](../storage/common/storage-redundancy.md#locally-redundant-storage) tárolási lehetőségekről többet olvashat ebben a [Tárhely-redundancia áttekintésben](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="2e590-158">Read more about [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) and [locally redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) storage options in this [Storage redundancy overview](../storage/common/storage-redundancy.md).</span></span>

<span data-ttu-id="2e590-159">Most, hogy létrehozta a tárolót, konfigurálja a Windows rendszerállapotról készít biztonsági mentést.</span><span class="sxs-lookup"><span data-stu-id="2e590-159">Now that you've created a vault, configure it for backing up Windows System State.</span></span>

## <a name="configure-hello-vault"></a><span data-ttu-id="2e590-160">Hello tároló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2e590-160">Configure hello vault</span></span>
1. <span data-ttu-id="2e590-161">A Recovery Services tároló paneljén (hello tároló most létrehozott), a Bevezetés című szakaszt hello hello, kattintson a **biztonsági mentés**, végül a hello **Ismerkedés a biztonsági mentés** panelen válassza  **Biztonsági mentési cél**.</span><span class="sxs-lookup"><span data-stu-id="2e590-161">On hello Recovery Services vault blade (for hello vault you just created), in hello Getting Started section, click **Backup**, then on hello **Getting Started with Backup** blade, select **Backup goal**.</span></span>

    ![A biztonsági mentés célja panel megnyitása](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

    <span data-ttu-id="2e590-163">Hello **biztonsági mentési cél** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="2e590-163">hello **Backup Goal** blade opens.</span></span>

    ![A biztonsági mentés célja panel megnyitása](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. <span data-ttu-id="2e590-165">A hello **a számítási feladatok futtató?** legördülő menüben válassza **helyszíni**.</span><span class="sxs-lookup"><span data-stu-id="2e590-165">From hello **Where is your workload running?** drop-down menu, select **On-premises**.</span></span>

    <span data-ttu-id="2e590-166">Azért kell a **Helyszíni** lehetőséget választania, mert a Windows Server- vagy Windows-számítógépe egy fizikai gép, amely nem az Azure része.</span><span class="sxs-lookup"><span data-stu-id="2e590-166">You choose **On-premises** because your Windows Server or Windows computer is a physical machine that is not in Azure.</span></span>

3. <span data-ttu-id="2e590-167">A hello **miről szeretné, hogy toobackup?** menüjében válassza **rendszerállapot**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="2e590-167">From hello **What do you want toobackup?** menu, select **System State**, and click **OK**.</span></span>

    ![Fájlok és mappák konfigurálása](./media/backup-azure-system-state/backup-goal-system-state.png)

    <span data-ttu-id="2e590-169">Az OK gombra kattintva jelölje megjelenése után következő túl**biztonsági mentési cél**, és hello **infrastruktúra előkészítése** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="2e590-169">After clicking OK, a checkmark appears next too**Backup goal**, and hello **Prepare infrastructure** blade opens.</span></span>

    ![Ha konfigurálta a biztonsági mentés célját, a következő lépés az infrastruktúra előkészítése](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. <span data-ttu-id="2e590-171">A hello **infrastruktúra előkészítése** panelen kattintson a **letöltése ügynök Windows vagy a Windows ügyfél**.</span><span class="sxs-lookup"><span data-stu-id="2e590-171">On hello **Prepare infrastructure** blade, click **Download Agent for Windows Server or Windows Client**.</span></span>

    ![infrastruktúra előkészítése](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

    <span data-ttu-id="2e590-173">Ha a Windows Server alapvető használ, majd válassza a toodownload hello ügynök a Windows Server alapvető.</span><span class="sxs-lookup"><span data-stu-id="2e590-173">If you are using Windows Server Essential, then choose toodownload hello agent for Windows Server Essential.</span></span> <span data-ttu-id="2e590-174">Felbukkanó toorun kéri, vagy MARSAgentInstaller.exe menti.</span><span class="sxs-lookup"><span data-stu-id="2e590-174">A pop-up menu prompts you toorun or save MARSAgentInstaller.exe.</span></span>

    ![MARSAgentInstaller párbeszédpanel](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. <span data-ttu-id="2e590-176">Hello letöltési megjelenő menüben, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="2e590-176">In hello download pop-up menu, click **Save**.</span></span>

    <span data-ttu-id="2e590-177">Alapértelmezés szerint hello **MARSagentinstaller.exe** fájl tooyour Letöltések mappába kerül.</span><span class="sxs-lookup"><span data-stu-id="2e590-177">By default, hello **MARSagentinstaller.exe** file is saved tooyour Downloads folder.</span></span> <span data-ttu-id="2e590-178">Hello telepítő befejezése után megjelenik egy előugró ablak, amely rákérdez, ha szeretné, hogy toorun hello telepítő, vagy nyissa meg a hello mappa.</span><span class="sxs-lookup"><span data-stu-id="2e590-178">When hello installer completes, you will see a pop-up asking if you want toorun hello installer, or open hello folder.</span></span>

    ![infrastruktúra előkészítése](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

    <span data-ttu-id="2e590-180">Tooinstall hello ügynök még nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="2e590-180">You don't need tooinstall hello agent yet.</span></span> <span data-ttu-id="2e590-181">Hello ügynök is telepíthet, miután letöltötte a hello tárolói hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="2e590-181">You can install hello agent after you have downloaded hello vault credentials.</span></span>

6. <span data-ttu-id="2e590-182">A hello **infrastruktúra előkészítése** panelen kattintson a **letöltése**.</span><span class="sxs-lookup"><span data-stu-id="2e590-182">On hello **Prepare infrastructure** blade, click **Download**.</span></span>

    ![a tároló hitelesítő adatainak letöltése](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

    <span data-ttu-id="2e590-184">hello tárolói hitelesítő adatok letöltése tooyour Letöltések mappába.</span><span class="sxs-lookup"><span data-stu-id="2e590-184">hello vault credentials download tooyour Downloads folder.</span></span> <span data-ttu-id="2e590-185">Hello tárolói hitelesítő adatokat, akkor letöltés befejezése után megjelenik egy előugró ablak, amely rákérdez, ha azt szeretné tooopen, vagy hello hitelesítő adatok mentése.</span><span class="sxs-lookup"><span data-stu-id="2e590-185">After hello vault credentials finish downloading, you see a pop-up asking if you want tooopen or save hello credentials.</span></span> <span data-ttu-id="2e590-186">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="2e590-186">Click **Save**.</span></span> <span data-ttu-id="2e590-187">Ha véletlenül kattintson **nyitott**, lehetővé teszik a hello párbeszédpanel érintő tooopen hello tárolói hitelesítő adatokat, a művelet sikertelen.</span><span class="sxs-lookup"><span data-stu-id="2e590-187">If you accidentally click **Open**, let hello dialog that attempts tooopen hello vault credentials, fail.</span></span> <span data-ttu-id="2e590-188">Hello tárolói hitelesítő adatok nem nyitható meg.</span><span class="sxs-lookup"><span data-stu-id="2e590-188">You cannot open hello vault credentials.</span></span> <span data-ttu-id="2e590-189">A folytatáshoz toohello következő lépésre.</span><span class="sxs-lookup"><span data-stu-id="2e590-189">Proceed toohello next step.</span></span> <span data-ttu-id="2e590-190">hello tárolói hitelesítő adatok vannak hello Letöltések mappába.</span><span class="sxs-lookup"><span data-stu-id="2e590-190">hello vault credentials are in hello Downloads folder.</span></span>   

    ![a tároló hitelesítő adatainak letöltése befejeződött](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)

## <a name="install-and-register-hello-agent"></a><span data-ttu-id="2e590-192">Telepítse és regisztrálja a hello ügynök</span><span class="sxs-lookup"><span data-stu-id="2e590-192">Install and register hello agent</span></span>

> [!NOTE]
> <span data-ttu-id="2e590-193">Lehetővé teszi biztonsági mentés hello Azure-portálon keresztül még nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="2e590-193">Enabling backup through hello Azure portal is not available, yet.</span></span> <span data-ttu-id="2e590-194">Hello Microsoft Azure Recovery Services Agent tooback használja a Windows Server rendszer állapotát.</span><span class="sxs-lookup"><span data-stu-id="2e590-194">Use hello Microsoft Azure Recovery Services Agent tooback up Windows Server System State.</span></span>
>

1. <span data-ttu-id="2e590-195">Keresse meg és kattintson duplán a hello **MARSagentinstaller.exe** hello Letöltések mappába (vagy más mentett helyről).</span><span class="sxs-lookup"><span data-stu-id="2e590-195">Locate and double-click hello **MARSagentinstaller.exe** from hello Downloads folder (or other saved location).</span></span>

    <span data-ttu-id="2e590-196">hello telepítője biztosítja az üzenetből bontja ki, mert telepíti, és regisztrálja hello Recovery Services Agent ügynököt.</span><span class="sxs-lookup"><span data-stu-id="2e590-196">hello installer provides a series of messages as it extracts, installs, and registers hello Recovery Services agent.</span></span>

    ![a Recovery Services-ügynök telepítőjének futtatása, hitelesítő adatok](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. <span data-ttu-id="2e590-198">Hello Microsoft Azure Recovery Services ügynök telepítő varázsló befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="2e590-198">Complete hello Microsoft Azure Recovery Services Agent Setup Wizard.</span></span> <span data-ttu-id="2e590-199">toocomplete hello varázsló kell:</span><span class="sxs-lookup"><span data-stu-id="2e590-199">toocomplete hello wizard, you need to:</span></span>

   * <span data-ttu-id="2e590-200">Válassza ki a hello telepítési és a gyorsítótár mappa.</span><span class="sxs-lookup"><span data-stu-id="2e590-200">Choose a location for hello installation and cache folder.</span></span>
   * <span data-ttu-id="2e590-201">Adja meg a proxykiszolgáló kiszolgálóadatok használatakor a proxy server tooconnect toohello internet.</span><span class="sxs-lookup"><span data-stu-id="2e590-201">Provide your proxy server info if you use a proxy server tooconnect toohello internet.</span></span>
   * <span data-ttu-id="2e590-202">Adja meg a felhasználónevének és jelszavának részleteit, ha hitelesített proxyt használ.</span><span class="sxs-lookup"><span data-stu-id="2e590-202">Provide your user name and password details if you use an authenticated proxy.</span></span>
   * <span data-ttu-id="2e590-203">Adja meg a letöltött hello tárolói hitelesítő adatokat</span><span class="sxs-lookup"><span data-stu-id="2e590-203">Provide hello downloaded vault credentials</span></span>
   * <span data-ttu-id="2e590-204">Hello titkosítási jelszó mentse egy biztonságos helyre.</span><span class="sxs-lookup"><span data-stu-id="2e590-204">Save hello encryption passphrase in a secure location.</span></span>

     > [!NOTE]
     > <span data-ttu-id="2e590-205">Ha elveszíti vagy elfelejti hello jelszót, a Microsoft nem súgó hello biztonsági mentési adatok helyreállítását.</span><span class="sxs-lookup"><span data-stu-id="2e590-205">If you lose or forget hello passphrase, Microsoft cannot help recover hello backup data.</span></span> <span data-ttu-id="2e590-206">Mentse a hello fájlt biztonságos helyen.</span><span class="sxs-lookup"><span data-stu-id="2e590-206">Save hello file in a secure location.</span></span> <span data-ttu-id="2e590-207">A biztonsági mentés szükséges toorestore.</span><span class="sxs-lookup"><span data-stu-id="2e590-207">It is required toorestore a backup.</span></span>
     >
     >

<span data-ttu-id="2e590-208">hello ügynök telepítve van, és a számítógép regisztrált toohello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="2e590-208">hello agent is now installed and your machine is registered toohello vault.</span></span> <span data-ttu-id="2e590-209">Most készen áll a tooconfigure, és a biztonsági mentés ütemezése.</span><span class="sxs-lookup"><span data-stu-id="2e590-209">You're ready tooconfigure and schedule your backup.</span></span>

## <a name="back-up-windows-server-system-state-preview"></a><span data-ttu-id="2e590-210">Készítsen biztonsági másolatot a Windows Server rendszer állapota (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="2e590-210">Back up Windows Server System State (Preview)</span></span>
<span data-ttu-id="2e590-211">hello kezdeti biztonsági másolatot a három feladatokból áll:</span><span class="sxs-lookup"><span data-stu-id="2e590-211">hello initial backup includes three tasks:</span></span>

* <span data-ttu-id="2e590-212">Rendszerállapot biztonsági mentésének segítségével hello Azure Backup szolgáltatás ügynökének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="2e590-212">Enable System State Backup using hello Azure Backup agent</span></span>
* <span data-ttu-id="2e590-213">Hello biztonsági mentés ütemezése</span><span class="sxs-lookup"><span data-stu-id="2e590-213">Schedule hello backup</span></span>
* <span data-ttu-id="2e590-214">Fájlok és mappák biztonsági mentése a hello első alkalommal</span><span class="sxs-lookup"><span data-stu-id="2e590-214">Back up files and folders for hello first time</span></span>

<span data-ttu-id="2e590-215">toocomplete hello kezdeti biztonsági másolatot, használjon hello Microsoft Azure Recovery Services Agent ügynököt.</span><span class="sxs-lookup"><span data-stu-id="2e590-215">toocomplete hello initial backup, use hello Microsoft Azure Recovery Services agent.</span></span>

### <a name="tooenable-system-state-backup-using-hello-azure-backup-agent"></a><span data-ttu-id="2e590-216">tooenable rendszerállapot biztonsági mentésének segítségével hello Azure biztonságimásolat-készítő ügynökkel</span><span class="sxs-lookup"><span data-stu-id="2e590-216">tooenable System State backup using hello Azure Backup agent</span></span>

1. <span data-ttu-id="2e590-217">Egy PowerShell-munkamenetet futtassa a következő parancs toostop hello Azure biztonsági mentés motor hello.</span><span class="sxs-lookup"><span data-stu-id="2e590-217">In a PowerShell session, run hello following command toostop hello Azure Backup engine.</span></span>

  ```
  PS C:\> Net stop obengine
  ```

2. <span data-ttu-id="2e590-218">Nyissa meg a Windows beállításjegyzék hello.</span><span class="sxs-lookup"><span data-stu-id="2e590-218">Open hello Windows Registry.</span></span>

  ```
  PS C:\> regedit.exe
  ```

3. <span data-ttu-id="2e590-219">Adja hozzá a hello rendszerleíró kulcsot következő hello megadott DWord-értéket.</span><span class="sxs-lookup"><span data-stu-id="2e590-219">Add hello following registry key with hello specified DWord Value.</span></span>

  | <span data-ttu-id="2e590-220">Beállításjegyzékbeli elérési út</span><span class="sxs-lookup"><span data-stu-id="2e590-220">Registry path</span></span> | <span data-ttu-id="2e590-221">Beállításkulcs</span><span class="sxs-lookup"><span data-stu-id="2e590-221">Registry key</span></span> | <span data-ttu-id="2e590-222">Duplaszó</span><span class="sxs-lookup"><span data-stu-id="2e590-222">DWord value</span></span> |
  |---------------|--------------|-------------|
  | <span data-ttu-id="2e590-223">HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider</span><span class="sxs-lookup"><span data-stu-id="2e590-223">HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider</span></span> | <span data-ttu-id="2e590-224">TurnOffSSBFeature</span><span class="sxs-lookup"><span data-stu-id="2e590-224">TurnOffSSBFeature</span></span> | <span data-ttu-id="2e590-225">2</span><span class="sxs-lookup"><span data-stu-id="2e590-225">2</span></span> |

4. <span data-ttu-id="2e590-226">Indítsa újra a hello biztonsági mentés motor hajtja végre a következő parancsot egy rendszergazda jogú parancssort a hello.</span><span class="sxs-lookup"><span data-stu-id="2e590-226">Restart hello Backup engine by executing hello following command in an elevated command prompt.</span></span>

  ```
  PS C:\> Net start obengine
  ```

### <a name="tooschedule-hello-backup-job"></a><span data-ttu-id="2e590-227">tooschedule hello biztonsági mentési feladat</span><span class="sxs-lookup"><span data-stu-id="2e590-227">tooschedule hello backup job</span></span>

1. <span data-ttu-id="2e590-228">Nyissa meg a hello Microsoft Azure Recovery Services Agent ügynököt.</span><span class="sxs-lookup"><span data-stu-id="2e590-228">Open hello Microsoft Azure Recovery Services agent.</span></span> <span data-ttu-id="2e590-229">A megkereséséhez keressen rá a gépen a **Microsoft Azure Backup** kifejezésre.</span><span class="sxs-lookup"><span data-stu-id="2e590-229">You can find it by searching your machine for **Microsoft Azure Backup**.</span></span>

    ![Indítsa el a hello Azure Recovery Services Agent ügynök](./media/backup-try-azure-backup-in-10-mins/snap-in-search.png)

2. <span data-ttu-id="2e590-231">Hello Recovery Services Agent ügynököt, kattintson **biztonsági mentés ütemezése**.</span><span class="sxs-lookup"><span data-stu-id="2e590-231">In hello Recovery Services agent, click **Schedule Backup**.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)

3. <span data-ttu-id="2e590-233">A hello bevezetés hello ütemezett biztonsági mentés varázsló lapján, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="2e590-233">On hello Getting started page of hello Schedule Backup Wizard, click **Next**.</span></span>

4. <span data-ttu-id="2e590-234">Kattintson hello elemek kijelölése tooBackup lap **elemek hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="2e590-234">On hello Select Items tooBackup page, click **Add Items**.</span></span>

5. <span data-ttu-id="2e590-235">Válassza ki **rendszerállapot** majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="2e590-235">Select **System State** and then click **OK**.</span></span>

6. <span data-ttu-id="2e590-236">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="2e590-236">Click **Next**.</span></span>

7. <span data-ttu-id="2e590-237">hello rendszerállapot biztonsági mentése és a megőrzési ütemezés nem automatikusan állítják be minden vasárnap tooback 9:00 PM helyi idő és hello megőrzési idő van beállítva too60 nap.</span><span class="sxs-lookup"><span data-stu-id="2e590-237">hello System State Backup and Retention schedule is automatically set tooback up every Sunday at 9:00 PM local time, and hello retention period is set too60 days.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2e590-238">Rendszerállapot biztonsági mentési és adatmegőrzési házirend automatikusan konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="2e590-238">System State backup and retention policy is automatically configured.</span></span> <span data-ttu-id="2e590-239">Ha a biztonsági mentést fájlok és mappák továbbá toohello Windows kiszolgáló rendszerállapotának, csak hello biztonsági mentési és adatmegőrzési házirend megadása a fájl mentését hello varázsló.</span><span class="sxs-lookup"><span data-stu-id="2e590-239">If you back up Files and Folders in addition toohello Windows Server System State, specify only hello Backup and Retention policy for file backups from hello wizard.</span></span> 
   >

8. <span data-ttu-id="2e590-240">A hello megerősítése lapon tekintse át hello adatokat, és kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="2e590-240">On hello Confirmation page, review hello information, and then click **Finish**.</span></span>

9. <span data-ttu-id="2e590-241">Hello biztonsági mentési ütemezés létrehozása hello varázsló befejezése után kattintson **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="2e590-241">After hello wizard finishes creating hello backup schedule, click **Close**.</span></span>

### <a name="tooback-up-windows-server-system-state-for-hello-first-time"></a><span data-ttu-id="2e590-242">hello először a Windows Server rendszerállapotot tooback</span><span class="sxs-lookup"><span data-stu-id="2e590-242">tooback up Windows Server System State for hello first time</span></span>

1. <span data-ttu-id="2e590-243">Győződjön meg arról, hogy nincsenek függőben lévő frissítések a Windows Server, a számítógép újraindítása szükséges.</span><span class="sxs-lookup"><span data-stu-id="2e590-243">Make sure there are no pending updates for Windows Server that require a reboot.</span></span>

2. <span data-ttu-id="2e590-244">Hello Recovery Services Agent ügynököt, kattintson **biztonsági másolat készítése most** toocomplete hello kezdeti összehangolása hello hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="2e590-244">In hello Recovery Services agent, click **Back Up Now** toocomplete hello initial seeding over hello network.</span></span>

    ![Windows Server biztonsági másolat készítése](./media/backup-try-azure-backup-in-10-mins/backup-now.png)

3. <span data-ttu-id="2e590-246">A hello megerősítése lapon, amely a biztonsági mentést most varázsló hello hello beállítások áttekintése hello gép tooback fogja használni.</span><span class="sxs-lookup"><span data-stu-id="2e590-246">On hello Confirmation page, review hello settings that hello Back Up Now Wizard will use tooback up hello machine.</span></span> <span data-ttu-id="2e590-247">Ezután kattintson a **Biztonsági mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="2e590-247">Then click **Back Up**.</span></span>

4. <span data-ttu-id="2e590-248">Kattintson a **Bezárás** tooclose hello varázsló.</span><span class="sxs-lookup"><span data-stu-id="2e590-248">Click **Close** tooclose hello wizard.</span></span> <span data-ttu-id="2e590-249">Ha a biztonsági mentési folyamat hello befejeződése előtt bezárja hello varázslót, hello varázsló toorun hello háttérben folytatódik.</span><span class="sxs-lookup"><span data-stu-id="2e590-249">If you close hello wizard before hello backup process finishes, hello wizard continues toorun in hello background.</span></span>

5. <span data-ttu-id="2e590-250">Ha a biztonsági mentést fájlok és mappák a kiszolgálón, továbbá toohello Windows Server rendszer állapotát, a hello most biztonsági mentés varázsló csak készít biztonsági másolatot a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="2e590-250">If you back up Files and Folders on your server, in addition toohello Windows Server System State, hello Backup Now wizard will only back up files.</span></span> <span data-ttu-id="2e590-251">az ad hoc rendszerállapot biztonsági mentése, a következő PowerShell-parancs használata hello tooperform:</span><span class="sxs-lookup"><span data-stu-id="2e590-251">tooperform an ad hoc System State back up, use hello following PowerShell command:</span></span>

    ```
    PS C:\> Start-OBSystemStateBackup
    ```

  <span data-ttu-id="2e590-252">Hello kezdeti biztonsági mentés befejezése után hello **feladata befejezve** állapota megjelenik hello biztonsági mentés konzolban.</span><span class="sxs-lookup"><span data-stu-id="2e590-252">After hello initial backup is completed, hello **Job completed** status appears in hello Backup console.</span></span>

  ![IR befejezve](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## <a name="frequently-asked-questions"></a><span data-ttu-id="2e590-254">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="2e590-254">Frequently asked questions</span></span>

<span data-ttu-id="2e590-255">hello alábbi kérdések és válaszok nyújt kiegészítő információkat.</span><span class="sxs-lookup"><span data-stu-id="2e590-255">hello following questions and answers provide supplementary information.</span></span>

### <a name="what-is-hello-staging-volume"></a><span data-ttu-id="2e590-256">Mi az az átmeneti kötet hello?</span><span class="sxs-lookup"><span data-stu-id="2e590-256">What is hello Staging Volume?</span></span>

<span data-ttu-id="2e590-257">hello átmeneti kötet hello közbenső hely, ahol hello natív módon, a Windows Server biztonsági másolat készít elő hello rendszerállapot biztonsági mentését jelöli.</span><span class="sxs-lookup"><span data-stu-id="2e590-257">hello Staging Volume represents hello intermediate location where hello natively available, Windows Server Backup stages hello System State Backup.</span></span> <span data-ttu-id="2e590-258">Az Azure Backup szolgáltatás ügynökének akkor tömöríti és titkosítja a köztes biztonsági mentési és elküldi azt biztonságos HTTPS protokoll toohello konfigurált Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="2e590-258">Azure Backup agent then compresses and encrypts this intermediate backup and sends it via secure HTTPS Protocol toohello configured Recovery Services Vault.</span></span> <span data-ttu-id="2e590-259">**Erősen ajánlott a nem Windows-OS kötet hello átmeneti kötet létrehozása. Azt láthatja, hogy a rendszerállapot biztonsági mentéseit problémáit, a átmeneti kötet hello hely ellenőrzése akkor hello hibaelhárítás első lépése.**</span><span class="sxs-lookup"><span data-stu-id="2e590-259">**We strongly recommended you establish hello Staging Volume in a non-Windows-OS volume. If you observe problems with System State Backups, checking hello location of your Staging Volume is hello first troubleshooting step.**</span></span> 

### <a name="how-can-i-change-hello-staging-volume-path-specified-in-hello-azure-backup-agent"></a><span data-ttu-id="2e590-260">Hogyan módosítható hello átmeneti elérési útjával hello Azure Backup szolgáltatás ügynökének megadott?</span><span class="sxs-lookup"><span data-stu-id="2e590-260">How can I change hello Staging Volume Path specified in hello Azure Backup agent?</span></span>

<span data-ttu-id="2e590-261">hello átmeneti kötet alapértelmezés szerint hello gyorsítótár mappában található.</span><span class="sxs-lookup"><span data-stu-id="2e590-261">hello Staging Volume is located in hello cache folder by default.</span></span> 

1. <span data-ttu-id="2e590-262">toochange ezen a helyen, a következő parancsot (a rendszergazda jogú parancssorból) használata hello:</span><span class="sxs-lookup"><span data-stu-id="2e590-262">toochange this location, use hello following command (in an elevated command prompt):</span></span>
  ```
  PS C:\> Net stop obengine
  ```

2. <span data-ttu-id="2e590-263">A következő beállításjegyzék-bejegyzések hello elérési toohello új átmeneti kötet mappa hello frissíteni.</span><span class="sxs-lookup"><span data-stu-id="2e590-263">Then update hello following registry entries with hello path toohello new Staging Volume folder.</span></span>

  |<span data-ttu-id="2e590-264">Beállításjegyzékbeli elérési út</span><span class="sxs-lookup"><span data-stu-id="2e590-264">Registry path</span></span>|<span data-ttu-id="2e590-265">Beállításkulcs</span><span class="sxs-lookup"><span data-stu-id="2e590-265">Registry key</span></span>|<span data-ttu-id="2e590-266">Érték</span><span class="sxs-lookup"><span data-stu-id="2e590-266">Value</span></span>|
  |-------------|------------|-----|
  |<span data-ttu-id="2e590-267">HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider</span><span class="sxs-lookup"><span data-stu-id="2e590-267">HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Azure Backup\Config\CloudBackupProvider</span></span> | <span data-ttu-id="2e590-268">SSBStagingPath</span><span class="sxs-lookup"><span data-stu-id="2e590-268">SSBStagingPath</span></span> | <span data-ttu-id="2e590-269">Új átmeneti kötet hely</span><span class="sxs-lookup"><span data-stu-id="2e590-269">new staging volume location</span></span> |

<span data-ttu-id="2e590-270">hello átmeneti elérési út a kis-és nagybetűket, és hello pontos azonos kis-és nagybetűk, mi létezik a hello kiszolgálón kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2e590-270">hello Staging Path is case sensitive and must be hello exact same casing as what exists on hello server.</span></span> 

3. <span data-ttu-id="2e590-271">Hello átmeneti kötet elérési út módosítása után indítsa újra a hello biztonsági mentés motor:</span><span class="sxs-lookup"><span data-stu-id="2e590-271">Once you change hello Staging volume path, restart hello Backup engine:</span></span>
  ```
  PS C:\> Net start obengine
  ```
4. <span data-ttu-id="2e590-272">toopick megváltozott hello elérési útja, hello nyissa meg a Microsoft Azure Recovery Services Agent ügynököt és egy ad hoc biztonsági mentés rendszerállapot eseményindító.</span><span class="sxs-lookup"><span data-stu-id="2e590-272">toopick up hello changed path, open hello Microsoft Azure Recovery Services agent and trigger an ad hoc backup of System State.</span></span>

### <a name="why-is-hello-system-state-default-retention-set-too60-days"></a><span data-ttu-id="2e590-273">Miért érdemes az alapértelmezett megőrzési beállítása too60 nap rendszerállapot hello?</span><span class="sxs-lookup"><span data-stu-id="2e590-273">Why is hello System State default retention set too60 days?</span></span>

<span data-ttu-id="2e590-274">a rendszerállapot biztonsági mentését hello élettartama van hello ugyanaz, mint a hello Windows Server Active Directory szerepkör hello "szemétgyűjtési" beállítást.</span><span class="sxs-lookup"><span data-stu-id="2e590-274">hello useful life of a system state backup is hello same as hello "tombstone lifetime" setting for hello Windows Server Active Directory role.</span></span> <span data-ttu-id="2e590-275">hello alapértelmezett hello törlésre élettartama bejegyzés értéke 60 nap.</span><span class="sxs-lookup"><span data-stu-id="2e590-275">hello default value for hello tombstone lifetime entry is 60 days.</span></span> <span data-ttu-id="2e590-276">Ez az érték beállítható hello címtárszolgáltatás (NTDS) konfigurációs objektum.</span><span class="sxs-lookup"><span data-stu-id="2e590-276">This value can be set on hello Directory Service (NTDS) config object.</span></span>

### <a name="how-do-i-change-hello-default-backup-and-retention-policy-for-system-state"></a><span data-ttu-id="2e590-277">Hogyan változtathatom meg hello alapértelmezett biztonsági mentési és adatmegőrzési rendszerállapot?</span><span class="sxs-lookup"><span data-stu-id="2e590-277">How do I change hello default Backup and Retention Policy for System State?</span></span>

<span data-ttu-id="2e590-278">toochange hello alapértelmezett biztonsági mentési és adatmegőrzési rendszerállapot:</span><span class="sxs-lookup"><span data-stu-id="2e590-278">toochange hello default Backup and Retention Policy for System State:</span></span>
1. <span data-ttu-id="2e590-279">Állítsa le a hello biztonsági mentést vezérlő.</span><span class="sxs-lookup"><span data-stu-id="2e590-279">Stop hello Backup engine.</span></span> <span data-ttu-id="2e590-280">Futtassa a következő parancsot egy emelt szintű parancssorból hello.</span><span class="sxs-lookup"><span data-stu-id="2e590-280">Run hello following command from an elevated command prompt.</span></span>

  ```
  PS C:\> Net stop obengine
  ```

2. <span data-ttu-id="2e590-281">Adja hozzá, vagy frissítse a következő beállításkulcs-bejegyzésekről a HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider hello.</span><span class="sxs-lookup"><span data-stu-id="2e590-281">Add or update hello following registry key entries in HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider.</span></span>

  |<span data-ttu-id="2e590-282">Rendszerleíró adatbázis neve</span><span class="sxs-lookup"><span data-stu-id="2e590-282">Registry Name</span></span>|<span data-ttu-id="2e590-283">Leírás</span><span class="sxs-lookup"><span data-stu-id="2e590-283">Description</span></span>|<span data-ttu-id="2e590-284">Érték</span><span class="sxs-lookup"><span data-stu-id="2e590-284">Value</span></span>|
  |-------------|-----------|-----|
  |<span data-ttu-id="2e590-285">SSBScheduleTime</span><span class="sxs-lookup"><span data-stu-id="2e590-285">SSBScheduleTime</span></span>|<span data-ttu-id="2e590-286">Hello biztonsági másolat készítésének idején tooconfigure hello használt.</span><span class="sxs-lookup"><span data-stu-id="2e590-286">Used tooconfigure hello time of hello backup.</span></span> <span data-ttu-id="2e590-287">Alapértelmezett érték a helyi idő du. 9.</span><span class="sxs-lookup"><span data-stu-id="2e590-287">Default is 9PM local time.</span></span>|<span data-ttu-id="2e590-288">DWord: Formátum HHMM (decimális) például 2130 9:30 PM helyi ideje</span><span class="sxs-lookup"><span data-stu-id="2e590-288">DWord: Format HHMM (Decimal) for example 2130 for 9:30PM local time</span></span>|
  |<span data-ttu-id="2e590-289">SSBScheduleDays</span><span class="sxs-lookup"><span data-stu-id="2e590-289">SSBScheduleDays</span></span>|<span data-ttu-id="2e590-290">Ha rendszerállapot biztonsági mentését kell végrehajtani hello használt tooconfigure hello nap megadva.</span><span class="sxs-lookup"><span data-stu-id="2e590-290">Used tooconfigure hello days when System State Backup must be performed at hello specified time.</span></span> <span data-ttu-id="2e590-291">Egyes számjegyek hello napot adja meg.</span><span class="sxs-lookup"><span data-stu-id="2e590-291">Individual digits specify days of hello week.</span></span> <span data-ttu-id="2e590-292">0 a vasárnapot jelenti, 1 hétfő, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="2e590-292">0 represents Sunday, 1 is Monday, and so on.</span></span> <span data-ttu-id="2e590-293">Alapértelmezett napi biztonsági mentéshez a vasárnapot jelenti.</span><span class="sxs-lookup"><span data-stu-id="2e590-293">Default day for backup is Sunday.</span></span>|<span data-ttu-id="2e590-294">DWord: nap hello hét toorun mentési (decimális) például 1230 ütemez biztonsági mentések hétfő, kedd, szerda és vasárnap.</span><span class="sxs-lookup"><span data-stu-id="2e590-294">DWord: days of hello week toorun backup (decimal) for example 1230 schedules backups on Monday, Tuesday, Wednesday, and Sunday.</span></span>|
  |<span data-ttu-id="2e590-295">SSBRetentionDays</span><span class="sxs-lookup"><span data-stu-id="2e590-295">SSBRetentionDays</span></span>|<span data-ttu-id="2e590-296">Használt tooconfigure hello nap tooretain biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="2e590-296">Used tooconfigure hello days tooretain backup.</span></span> <span data-ttu-id="2e590-297">Alapértelmezett érték 60.</span><span class="sxs-lookup"><span data-stu-id="2e590-297">Default value is 60.</span></span> <span data-ttu-id="2e590-298">Megengedett maximális értéket 180.</span><span class="sxs-lookup"><span data-stu-id="2e590-298">Maximum allowed value is 180.</span></span>|<span data-ttu-id="2e590-299">DWord: Nap tooretain biztonsági mentés (decimális).</span><span class="sxs-lookup"><span data-stu-id="2e590-299">DWord: Days tooretain backup (decimal).</span></span>|

3. <span data-ttu-id="2e590-300">A következő parancs toorestart hello biztonságimásolat-készítő motor hello használata.</span><span class="sxs-lookup"><span data-stu-id="2e590-300">Use hello following command toorestart hello backup engine.</span></span>
    ```
    PS C:\> Net start obengine
    ```

4. <span data-ttu-id="2e590-301">Nyissa meg a hello Microsoft Recovery Services Agent ügynököt.</span><span class="sxs-lookup"><span data-stu-id="2e590-301">Open hello Microsoft Recovery Services agent.</span></span>

5. <span data-ttu-id="2e590-302">Kattintson a **biztonsági mentés ütemezése** majd **következő** amíg hello módosítások megjelenik.</span><span class="sxs-lookup"><span data-stu-id="2e590-302">Click **Schedule Backup** and then click **Next** until you see hello changes reflected.</span></span>

6. <span data-ttu-id="2e590-303">Kattintson a **Befejezés** tooapply hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="2e590-303">Click **Finish** tooapply hello changes.</span></span>


## <a name="questions"></a><span data-ttu-id="2e590-304">Kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="2e590-304">Questions?</span></span>
<span data-ttu-id="2e590-305">Ha kérdése van, vagy ha bármely új szolgáltatása része, toosee szeretné [visszajelzést küldhet](http://aka.ms/azurebackup_feedback).</span><span class="sxs-lookup"><span data-stu-id="2e590-305">If you have questions, or if there is any feature that you would like toosee included, [send us feedback](http://aka.ms/azurebackup_feedback).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2e590-306">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2e590-306">Next steps</span></span>
* <span data-ttu-id="2e590-307">További részletek a [Windows rendszerű gépek biztonsági mentéséről](backup-configure-vault.md).</span><span class="sxs-lookup"><span data-stu-id="2e590-307">Get more details about [backing up Windows machines](backup-configure-vault.md).</span></span>
* <span data-ttu-id="2e590-308">Most, hogy biztonsági másolatot készített a fájlokról és mappákról, [kezelheti a tárlókat és a kiszolgálókat](backup-azure-manage-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="2e590-308">Now that you've backed up your files and folders, you can [manage your vaults and servers](backup-azure-manage-windows-server.md).</span></span>
* <span data-ttu-id="2e590-309">Ha szükség toorestore biztonsági másolat, akkor ez a cikk túl[fájlok tooa Windows számítógép visszaállítása](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="2e590-309">If you need toorestore a backup, use this article too[restore files tooa Windows machine](backup-azure-restore-windows-server.md).</span></span>
