---
title: "aaaUse Azure Backup agent tooback fájlok és mappák |} Microsoft Docs"
description: "Hello Microsoft Azure Backup agent tooback Windows fájlok és mappák tooAzure másolatot használja. Recovery Services-tároló létrehozása, hello Backup szolgáltatás ügynökének telepítése, hello biztonsági mentési házirend meghatározása és hello kezdeti biztonsági mentés futtatására hello fájlok és mappák."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "mentési tároló; Készítsen biztonsági másolatot a Windows server; biztonsági mentési Időablakok;"
ms.assetid: 7f5b1943-b3c1-4ddb-8fb7-3560533c68d5
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/15/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: be203c24841971872b5c6e7cf260a2fa5c58ac47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-a-windows-server-or-client-tooazure-using-hello-resource-manager-deployment-model"></a><span data-ttu-id="5dd65-105">Készítsen biztonsági másolatot a Windows Server vagy az ügyfél tooAzure hello Resource Manager telepítési modell segítségével</span><span class="sxs-lookup"><span data-stu-id="5dd65-105">Back up a Windows Server or client tooAzure using hello Resource Manager deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5dd65-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5dd65-106">Azure portal</span></span>](backup-configure-vault.md)
> * [<span data-ttu-id="5dd65-107">Klasszikus portál</span><span class="sxs-lookup"><span data-stu-id="5dd65-107">Classic portal</span></span>](backup-configure-vault-classic.md)
>
>

<span data-ttu-id="5dd65-108">Ez a cikk azt ismerteti, hogyan tooback készítése a Windows Server (vagy a Windows-ügyfél) fájlok és mappák tooAzure az Azure Backup használatával hello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="5dd65-108">This article explains how tooback up your Windows Server (or Windows client) files and folders tooAzure with Azure Backup using hello Resource Manager deployment model.</span></span>

[!INCLUDE [learn-about-deployment-models](../../includes/backup-deployment-models.md)]

![Biztonsági mentési folyamat lépései](./media/backup-configure-vault/initial-backup-process.png)

## <a name="before-you-start"></a><span data-ttu-id="5dd65-110">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="5dd65-110">Before you start</span></span>
<span data-ttu-id="5dd65-111">egy kiszolgáló vagy az ügyfél tooAzure tooback, kell az Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="5dd65-111">tooback up a server or client tooAzure, you need an Azure account.</span></span> <span data-ttu-id="5dd65-112">Ha még nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/) néhány percig.</span><span class="sxs-lookup"><span data-stu-id="5dd65-112">If you don't have one, you can create a [free account](https://azure.microsoft.com/free/) in just a couple of minutes.</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="5dd65-113">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="5dd65-113">Create a Recovery Services vault</span></span>
<span data-ttu-id="5dd65-114">Recovery Services-tároló olyan entitás, amely minden hello biztonsági mentések és helyreállítási pontokat hoz létre adott idő alatt tárolja.</span><span class="sxs-lookup"><span data-stu-id="5dd65-114">A Recovery Services vault is an entity that stores all hello backups and recovery points you create over time.</span></span> <span data-ttu-id="5dd65-115">hello Recovery Services-tárolónak a hello alkalmazza a biztonsági mentési házirend toohello védett fájlok és mappák is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="5dd65-115">hello Recovery Services vault also contains hello backup policy applied toohello protected files and folders.</span></span> <span data-ttu-id="5dd65-116">Recovery Services-tároló létrehozásakor kell is lehetőséggel hello megfelelő tárolási redundanciát.</span><span class="sxs-lookup"><span data-stu-id="5dd65-116">When you create a Recovery Services vault, you should also select hello appropriate storage redundancy option.</span></span>

### <a name="toocreate-a-recovery-services-vault"></a><span data-ttu-id="5dd65-117">Recovery Services-tároló toocreate</span><span class="sxs-lookup"><span data-stu-id="5dd65-117">toocreate a Recovery Services vault</span></span>
1. <span data-ttu-id="5dd65-118">Ha még nem tette meg, jelentkezzen be toohello [Azure Portal](https://portal.azure.com/) használata az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="5dd65-118">If you haven't already done so, sign in toohello [Azure Portal](https://portal.azure.com/) using your Azure subscription.</span></span>
2. <span data-ttu-id="5dd65-119">Hello központ menüben kattintson a **további szolgáltatások** hello az erőforrások listájához, írja be a **Recovery Services** kattintson **Recovery Services-tárolók**.</span><span class="sxs-lookup"><span data-stu-id="5dd65-119">On hello Hub menu, click **More services** and in hello list of resources, type **Recovery Services** and click **Recovery Services vaults**.</span></span>

    ![Recovery Services-tároló létrehozása – 1. lépés](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    <span data-ttu-id="5dd65-121">Ha nincsenek recovery services-tárolók hello az előfizetést, hello tárolók vannak felsorolva.</span><span class="sxs-lookup"><span data-stu-id="5dd65-121">If there are recovery services vaults in hello subscription, hello vaults are listed.</span></span>

3. <span data-ttu-id="5dd65-122">A hello **Recovery Services-tárolók** menüben kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="5dd65-122">On hello **Recovery Services vaults** menu, click **Add**.</span></span>

    ![Recovery Services-tároló létrehozása – 2. lépés](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    <span data-ttu-id="5dd65-124">hello Recovery Services tároló panel nyílik tooprovide kéri egy **neve**, **előfizetés**, **erőforráscsoport**, és **hely**.</span><span class="sxs-lookup"><span data-stu-id="5dd65-124">hello Recovery Services vault blade opens, prompting you tooprovide a **Name**, **Subscription**, **Resource group**, and **Location**.</span></span>

    ![Recovery Services-tároló létrehozása – 3. lépés](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. <span data-ttu-id="5dd65-126">A **neve**, adjon meg egy rövid nevet tooidentify hello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="5dd65-126">For **Name**, enter a friendly name tooidentify hello vault.</span></span> <span data-ttu-id="5dd65-127">hello nevének kell toobe egyedi hello Azure-előfizetés esetében.</span><span class="sxs-lookup"><span data-stu-id="5dd65-127">hello name needs toobe unique for hello Azure subscription.</span></span> <span data-ttu-id="5dd65-128">Írjon be egy 2–50 karakter hosszúságú nevet.</span><span class="sxs-lookup"><span data-stu-id="5dd65-128">Type a name that contains between 2 and 50 characters.</span></span> <span data-ttu-id="5dd65-129">Ennek egy betűvel kell kezdődnie, és csak betűket, számokat és kötőjeleket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="5dd65-129">It must start with a letter, and can contain only letters, numbers, and hyphens.</span></span>

5. <span data-ttu-id="5dd65-130">A hello **előfizetés** területen hello legördülő menü toochoose hello Azure-előfizetés használatára.</span><span class="sxs-lookup"><span data-stu-id="5dd65-130">In hello **Subscription** section, use hello drop-down menu toochoose hello Azure subscription.</span></span> <span data-ttu-id="5dd65-131">Ha csak egyetlen előfizetéssel, előfizetés megjelenő használja, és tovább toohello a lépést kihagyhatja.</span><span class="sxs-lookup"><span data-stu-id="5dd65-131">If you use only one subscription, that subscription appears and you can skip toohello next step.</span></span> <span data-ttu-id="5dd65-132">Ha nem biztos abban, hogy melyik előfizetéssel toouse, használhatja az alapértelmezettet hello (vagy javasolt) előfizetés.</span><span class="sxs-lookup"><span data-stu-id="5dd65-132">If you are not sure which subscription toouse, use hello default (or suggested) subscription.</span></span> <span data-ttu-id="5dd65-133">Csak akkor lesz több választási lehetőség, ha a szervezetéhez tartozó fiók több Azure-előfizetéssel van összekötve.</span><span class="sxs-lookup"><span data-stu-id="5dd65-133">There are multiple choices only if your organizational account is associated with multiple Azure subscriptions.</span></span>

6. <span data-ttu-id="5dd65-134">A hello **erőforráscsoport** szakasz:</span><span class="sxs-lookup"><span data-stu-id="5dd65-134">In hello **Resource group** section:</span></span>

    * <span data-ttu-id="5dd65-135">Válassza ki **hozzon létre új** Ha azt szeretné, hogy toocreate egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="5dd65-135">select **Create new** if you want toocreate a new Resource group.</span></span>
    <span data-ttu-id="5dd65-136">Vagy</span><span class="sxs-lookup"><span data-stu-id="5dd65-136">Or</span></span>
    * <span data-ttu-id="5dd65-137">Válassza ki **meglévő** kattintson hello legördülő menü toosee hello elérhető erőforráscsoportok listáját.</span><span class="sxs-lookup"><span data-stu-id="5dd65-137">select **Use existing** and click hello drop-down menu toosee hello available list of Resource groups.</span></span>

  <span data-ttu-id="5dd65-138">Az erőforráscsoportok, tanulmányozza hello [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5dd65-138">For complete information on Resource groups, see hello [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

7. <span data-ttu-id="5dd65-139">Kattintson a **hely** tooselect hello földrajzi régióban hello tároló.</span><span class="sxs-lookup"><span data-stu-id="5dd65-139">Click **Location** tooselect hello geographic region for hello vault.</span></span> <span data-ttu-id="5dd65-140">Ez a beállítás meghatározza, hogy hello földrajzi régiót, ahol a biztonsági mentési adatokat küldi el.</span><span class="sxs-lookup"><span data-stu-id="5dd65-140">This choice determines hello geographic region where your backup data is sent.</span></span>

8. <span data-ttu-id="5dd65-141">A Recovery Services-tároló panel hello hello alján kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="5dd65-141">At hello bottom of hello Recovery Services vault blade, click **Create**.</span></span>

  <span data-ttu-id="5dd65-142">Recovery Services-tároló létrehozása toobe hello több percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="5dd65-142">It can take several minutes for hello Recovery Services vault toobe created.</span></span> <span data-ttu-id="5dd65-143">Hello állapot értesítések hello felső jobb területen hello portál figyelése.</span><span class="sxs-lookup"><span data-stu-id="5dd65-143">Monitor hello status notifications in hello upper right-hand area of hello portal.</span></span> <span data-ttu-id="5dd65-144">A tároló létrehozása után hello Recovery Services-tárolók listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="5dd65-144">Once your vault is created, it appears in hello list of Recovery Services vaults.</span></span> <span data-ttu-id="5dd65-145">Ha több perc után sem látja a tárolót, kattintson a **Frissítés** gombra.</span><span class="sxs-lookup"><span data-stu-id="5dd65-145">If after several minutes you don't see your vault, click **Refresh**.</span></span>

  ![Kattintson a Frissítés gombra](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

  <span data-ttu-id="5dd65-147">A tároló a Recovery Services-tárolók hello listája jelenik meg, ha készen áll a tooset hello adattároló redundanciája, amely áll.</span><span class="sxs-lookup"><span data-stu-id="5dd65-147">Once you see your vault in hello list of Recovery Services vaults, you are ready tooset hello storage redundancy.</span></span>


### <a name="set-storage-redundancy"></a><span data-ttu-id="5dd65-148">Set adattároló redundanciája, amely</span><span class="sxs-lookup"><span data-stu-id="5dd65-148">Set storage redundancy</span></span>
<span data-ttu-id="5dd65-149">Amikor először hoz létre Recovery Services-tárolót, meghatározza a tároló replikálásának módját.</span><span class="sxs-lookup"><span data-stu-id="5dd65-149">When you first create a Recovery Services vault you determine how storage is replicated.</span></span>

1. <span data-ttu-id="5dd65-150">A hello **Recovery Services-tárolók** paneljén kattintson hello új tárolóra.</span><span class="sxs-lookup"><span data-stu-id="5dd65-150">From hello **Recovery Services vaults** blade, click hello new vault.</span></span>

    ![Válassza ki az új tároló hello a hello Recovery Services-tároló](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    <span data-ttu-id="5dd65-152">Hello tároló kiválasztásakor hello **Recovery Services-tároló** panel narrows és hello-beállítások panel (*hello tároló nevére hello hello felső tartalmaz*) és hello tároló Részletek panel megnyitása.</span><span class="sxs-lookup"><span data-stu-id="5dd65-152">When you select hello vault, hello **Recovery Services vault** blade narrows, and hello Settings blade (*which has hello name of hello vault at hello top*) and hello vault details blade open.</span></span>

    ![Hello új tároló tárolási konfiguráció megtekintése](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)

2. <span data-ttu-id="5dd65-154">Hello új tároló-beállítások panelen hello függőleges diák tooscroll le toohello kezelése szakasz használja, és kattintson **biztonsági infrastruktúra**.</span><span class="sxs-lookup"><span data-stu-id="5dd65-154">In hello new vault's Settings blade, use hello vertical slide tooscroll down toohello Manage section, and click **Backup Infrastructure**.</span></span>

  <span data-ttu-id="5dd65-155">hello biztonsági infrastruktúra panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="5dd65-155">hello Backup Infrastructure blade opens.</span></span>

3. <span data-ttu-id="5dd65-156">Hello biztonsági infrastruktúra paneljén kattintson **biztonsági mentési konfigurációhoz** tooopen hello **biztonsági mentési konfigurációhoz** panelen.</span><span class="sxs-lookup"><span data-stu-id="5dd65-156">In hello Backup Infrastructure blade, click **Backup Configuration** tooopen hello **Backup Configuration** blade.</span></span>

  ![Új tároló hello tárolási konfiguráció beállítása](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)

4. <span data-ttu-id="5dd65-158">Válassza ki a hello megfelelő tárolási replikációs beállítás a tároló számára.</span><span class="sxs-lookup"><span data-stu-id="5dd65-158">Choose hello appropriate storage replication option for your vault.</span></span>

  ![a tároló konfigurálásának lehetőségei](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

  <span data-ttu-id="5dd65-160">Alapértelmezés szerint a tárolója georedundáns tárolással rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="5dd65-160">By default, your vault has geo-redundant storage.</span></span> <span data-ttu-id="5dd65-161">Azure biztonságimásolat-tároláshoz-végpontként használatakor, továbbra is toouse **georedundáns**.</span><span class="sxs-lookup"><span data-stu-id="5dd65-161">If you use Azure as a primary backup storage endpoint, continue toouse **Geo-redundant**.</span></span> <span data-ttu-id="5dd65-162">Ha nem adja meg Azure biztonságimásolat-tároláshoz-végpontként, majd válasszon **helyileg redundáns**, amely csökkenti a hello Azure storage költségei.</span><span class="sxs-lookup"><span data-stu-id="5dd65-162">If you don't use Azure as a primary backup storage endpoint, then choose **Locally-redundant**, which reduces hello Azure storage costs.</span></span> <span data-ttu-id="5dd65-163">A [georedundáns](../storage/common/storage-redundancy.md#geo-redundant-storage) és a [helyileg redundáns](../storage/common/storage-redundancy.md#locally-redundant-storage) tárolási lehetőségekről többet olvashat ebben a [Tárhely-redundancia áttekintésben](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="5dd65-163">Read more about [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) and [locally redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) storage options in this [Storage redundancy overview](../storage/common/storage-redundancy.md).</span></span>

<span data-ttu-id="5dd65-164">Most, hogy létrehozta a tárolót, készítse elő az infrastruktúra tooback mappák és fájlok letöltése és hello Microsoft Azure Recovery Services Agent ügynök telepítése, töltse le a tároló, és ezen hitelesítő adatok tooregister hello ügynök használata hello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="5dd65-164">Now that you've created a vault, prepare your infrastructure tooback up files and folders by downloading and installing hello Microsoft Azure Recovery Services agent, downloading vault credentials, and then using those credentials tooregister hello agent with hello vault.</span></span>

## <a name="configure-hello-vault"></a><span data-ttu-id="5dd65-165">Hello tároló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5dd65-165">Configure hello vault</span></span>

1. <span data-ttu-id="5dd65-166">A Recovery Services tároló paneljén (hello tároló most létrehozott), a Bevezetés című szakaszt hello hello, kattintson a **biztonsági mentés**, végül a hello **Ismerkedés a biztonsági mentés** panelen válassza  **Biztonsági mentési cél**.</span><span class="sxs-lookup"><span data-stu-id="5dd65-166">On hello Recovery Services vault blade (for hello vault you just created), in hello Getting Started section, click **Backup**, then on hello **Getting Started with Backup** blade, select **Backup goal**.</span></span>

  ![A biztonsági mentés célja panel megnyitása](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

  <span data-ttu-id="5dd65-168">Hello **biztonsági mentési cél** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="5dd65-168">hello **Backup Goal** blade opens.</span></span> <span data-ttu-id="5dd65-169">Ha a Recovery Services-tároló hello korábban volt konfigurálva, majd hello **biztonsági mentési cél** paneleken kattintva megjelenik **biztonsági mentés** hello Recovery Services tároló panel.</span><span class="sxs-lookup"><span data-stu-id="5dd65-169">If hello Recovery Services vault has been previously configured, then hello **Backup Goal** blades opens when you click **Backup** on hello Recovery Services vault blade.</span></span>

  ![A biztonsági mentés célja panel megnyitása](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. <span data-ttu-id="5dd65-171">A hello **a számítási feladatok futtató?** legördülő menüben válassza **helyszíni**.</span><span class="sxs-lookup"><span data-stu-id="5dd65-171">From hello **Where is your workload running?** drop-down menu, select **On-premises**.</span></span>

  <span data-ttu-id="5dd65-172">Azért kell a **Helyszíni** lehetőséget választania, mert a Windows Server- vagy Windows-számítógépe egy fizikai gép, amely nem az Azure része.</span><span class="sxs-lookup"><span data-stu-id="5dd65-172">You choose **On-premises** because your Windows Server or Windows computer is a physical machine that is not in Azure.</span></span>

3. <span data-ttu-id="5dd65-173">A hello **miről szeretné, hogy toobackup?** menüjében válassza **fájlok és mappák**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="5dd65-173">From hello **What do you want toobackup?** menu, select **Files and folders**, and click **OK**.</span></span>

  ![Fájlok és mappák konfigurálása](./media/backup-try-azure-backup-in-10-mins/set-file-folder.png)

  <span data-ttu-id="5dd65-175">Az OK gombra kattintva jelölje megjelenése után következő túl**biztonsági mentési cél**, és hello **infrastruktúra előkészítése** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="5dd65-175">After clicking OK, a checkmark appears next too**Backup goal**, and hello **Prepare infrastructure** blade opens.</span></span>

  ![Ha konfigurálta a biztonsági mentés célját, a következő lépés az infrastruktúra előkészítése](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. <span data-ttu-id="5dd65-177">A hello **infrastruktúra előkészítése** panelen kattintson a **letöltése ügynök Windows vagy a Windows ügyfél**.</span><span class="sxs-lookup"><span data-stu-id="5dd65-177">On hello **Prepare infrastructure** blade, click **Download Agent for Windows Server or Windows Client**.</span></span>

  ![infrastruktúra előkészítése](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

  <span data-ttu-id="5dd65-179">Ha a Windows Server alapvető használ, majd válassza a toodownload hello ügynök a Windows Server alapvető.</span><span class="sxs-lookup"><span data-stu-id="5dd65-179">If you are using Windows Server Essential, then choose toodownload hello agent for Windows Server Essential.</span></span> <span data-ttu-id="5dd65-180">Felbukkanó toorun kéri, vagy MARSAgentInstaller.exe menti.</span><span class="sxs-lookup"><span data-stu-id="5dd65-180">A pop-up menu prompts you toorun or save MARSAgentInstaller.exe.</span></span>

  ![MARSAgentInstaller párbeszédpanel](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. <span data-ttu-id="5dd65-182">Hello letöltési megjelenő menüben, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="5dd65-182">In hello download pop-up menu, click **Save**.</span></span>

  <span data-ttu-id="5dd65-183">Alapértelmezés szerint hello **MARSagentinstaller.exe** fájl tooyour Letöltések mappába kerül.</span><span class="sxs-lookup"><span data-stu-id="5dd65-183">By default, hello **MARSagentinstaller.exe** file is saved tooyour Downloads folder.</span></span> <span data-ttu-id="5dd65-184">Hello telepítő befejezése után megjelenik egy előugró ablak, amely rákérdez, ha szeretné, hogy toorun hello telepítő, vagy nyissa meg a hello mappa.</span><span class="sxs-lookup"><span data-stu-id="5dd65-184">When hello installer completes, you will see a pop-up asking if you want toorun hello installer, or open hello folder.</span></span>

  ![infrastruktúra előkészítése](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

  <span data-ttu-id="5dd65-186">Tooinstall hello ügynök még nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="5dd65-186">You don't need tooinstall hello agent yet.</span></span> <span data-ttu-id="5dd65-187">Hello ügynök is telepíthet, miután letöltötte a hello tárolói hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="5dd65-187">You can install hello agent after you have downloaded hello vault credentials.</span></span>

6. <span data-ttu-id="5dd65-188">A hello **infrastruktúra előkészítése** panelen kattintson a **letöltése**.</span><span class="sxs-lookup"><span data-stu-id="5dd65-188">On hello **Prepare infrastructure** blade, click **Download**.</span></span>

  ![a tároló hitelesítő adatainak letöltése](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

  <span data-ttu-id="5dd65-190">hello tárolói hitelesítő adatok letöltése tooyour Letöltések mappába.</span><span class="sxs-lookup"><span data-stu-id="5dd65-190">hello vault credentials download tooyour Downloads folder.</span></span> <span data-ttu-id="5dd65-191">Hello tárolói hitelesítő adatokat, akkor letöltés befejezése után megjelenik egy előugró ablak, amely rákérdez, ha azt szeretné tooopen, vagy hello hitelesítő adatok mentése.</span><span class="sxs-lookup"><span data-stu-id="5dd65-191">After hello vault credentials finish downloading, you see a pop-up asking if you want tooopen or save hello credentials.</span></span> <span data-ttu-id="5dd65-192">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="5dd65-192">Click **Save**.</span></span> <span data-ttu-id="5dd65-193">Ha véletlenül kattintson **nyitott**, lehetővé teszik a hello párbeszédpanel érintő tooopen hello tárolói hitelesítő adatokat, a művelet sikertelen.</span><span class="sxs-lookup"><span data-stu-id="5dd65-193">If you accidentally click **Open**, let hello dialog that attempts tooopen hello vault credentials, fail.</span></span> <span data-ttu-id="5dd65-194">Hello tárolói hitelesítő adatok nem nyitható meg.</span><span class="sxs-lookup"><span data-stu-id="5dd65-194">You cannot open hello vault credentials.</span></span> <span data-ttu-id="5dd65-195">A folytatáshoz toohello következő lépésre.</span><span class="sxs-lookup"><span data-stu-id="5dd65-195">Proceed toohello next step.</span></span> <span data-ttu-id="5dd65-196">hello tárolói hitelesítő adatok vannak hello Letöltések mappába.</span><span class="sxs-lookup"><span data-stu-id="5dd65-196">hello vault credentials are in hello Downloads folder.</span></span>   

  ![a tároló hitelesítő adatainak letöltése befejeződött](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)

## <a name="install-and-register-hello-agent"></a><span data-ttu-id="5dd65-198">Telepítse és regisztrálja a hello ügynök</span><span class="sxs-lookup"><span data-stu-id="5dd65-198">Install and register hello agent</span></span>

> [!NOTE]
> <span data-ttu-id="5dd65-199">Lehetővé teszi biztonsági mentés hello Azure-portálon keresztül még nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="5dd65-199">Enabling backup through hello Azure portal is not available, yet.</span></span> <span data-ttu-id="5dd65-200">A fájlok és mappák hello Microsoft Azure Recovery Services Agent tooback használja.</span><span class="sxs-lookup"><span data-stu-id="5dd65-200">Use hello Microsoft Azure Recovery Services Agent tooback up your files and folders.</span></span>
>

1. <span data-ttu-id="5dd65-201">Keresse meg és kattintson duplán a hello **MARSagentinstaller.exe** hello Letöltések mappába (vagy más mentett helyről).</span><span class="sxs-lookup"><span data-stu-id="5dd65-201">Locate and double-click hello **MARSagentinstaller.exe** from hello Downloads folder (or other saved location).</span></span>

  <span data-ttu-id="5dd65-202">hello telepítője biztosítja az üzenetből bontja ki, mert telepíti, és regisztrálja hello Recovery Services Agent ügynököt.</span><span class="sxs-lookup"><span data-stu-id="5dd65-202">hello installer provides a series of messages as it extracts, installs, and registers hello Recovery Services agent.</span></span>

  ![a Recovery Services-ügynök telepítőjének futtatása, hitelesítő adatok](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. <span data-ttu-id="5dd65-204">Hello Microsoft Azure Recovery Services ügynök telepítő varázsló befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="5dd65-204">Complete hello Microsoft Azure Recovery Services Agent Setup Wizard.</span></span> <span data-ttu-id="5dd65-205">toocomplete hello varázsló kell:</span><span class="sxs-lookup"><span data-stu-id="5dd65-205">toocomplete hello wizard, you need to:</span></span>

  * <span data-ttu-id="5dd65-206">Válassza ki a hello telepítési és a gyorsítótár mappa.</span><span class="sxs-lookup"><span data-stu-id="5dd65-206">Choose a location for hello installation and cache folder.</span></span>
  * <span data-ttu-id="5dd65-207">Adja meg a proxykiszolgáló kiszolgálóadatok használatakor a proxy server tooconnect toohello internet.</span><span class="sxs-lookup"><span data-stu-id="5dd65-207">Provide your proxy server info if you use a proxy server tooconnect toohello internet.</span></span>
  * <span data-ttu-id="5dd65-208">Adja meg a felhasználónevének és jelszavának részleteit, ha hitelesített proxyt használ.</span><span class="sxs-lookup"><span data-stu-id="5dd65-208">Provide your user name and password details if you use an authenticated proxy.</span></span>
  * <span data-ttu-id="5dd65-209">Adja meg a letöltött hello tárolói hitelesítő adatokat</span><span class="sxs-lookup"><span data-stu-id="5dd65-209">Provide hello downloaded vault credentials</span></span>
  * <span data-ttu-id="5dd65-210">Hello titkosítási jelszó mentse egy biztonságos helyre.</span><span class="sxs-lookup"><span data-stu-id="5dd65-210">Save hello encryption passphrase in a secure location.</span></span>

  > [!NOTE]
  > <span data-ttu-id="5dd65-211">Ha elveszíti vagy elfelejti hello jelszót, a Microsoft nem súgó hello biztonsági mentési adatok helyreállítását.</span><span class="sxs-lookup"><span data-stu-id="5dd65-211">If you lose or forget hello passphrase, Microsoft cannot help recover hello backup data.</span></span> <span data-ttu-id="5dd65-212">Mentse a hello fájlt biztonságos helyen.</span><span class="sxs-lookup"><span data-stu-id="5dd65-212">Save hello file in a secure location.</span></span> <span data-ttu-id="5dd65-213">A biztonsági mentés szükséges toorestore.</span><span class="sxs-lookup"><span data-stu-id="5dd65-213">It is required toorestore a backup.</span></span>
  >
  >

<span data-ttu-id="5dd65-214">hello ügynök telepítve van, és a számítógép regisztrált toohello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="5dd65-214">hello agent is now installed and your machine is registered toohello vault.</span></span> <span data-ttu-id="5dd65-215">Most készen áll a tooconfigure, és a biztonsági mentés ütemezése.</span><span class="sxs-lookup"><span data-stu-id="5dd65-215">You're ready tooconfigure and schedule your backup.</span></span>

## <a name="network-and-connectivity-requirements"></a><span data-ttu-id="5dd65-216">Hálózati és kapcsolati követelmények</span><span class="sxs-lookup"><span data-stu-id="5dd65-216">Network and Connectivity Requirements</span></span>

<span data-ttu-id="5dd65-217">Ha a machine /-proxy korlátozott internet-hozzáféréssel, győződjön meg arról, hogy tűzfal beállításait a hello machine /-proxy konfigurált tooallow hello következő URL-címek:</span><span class="sxs-lookup"><span data-stu-id="5dd65-217">If your machine/proxy has limited internet access, ensure that firewall settings on hello machine/proxy are configured tooallow hello following URLs:</span></span> <br>
    1. <span data-ttu-id="5dd65-218">www.msftncsi.com</span><span class="sxs-lookup"><span data-stu-id="5dd65-218">www.msftncsi.com</span></span>
    2. <span data-ttu-id="5dd65-219">*.Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="5dd65-219">*.Microsoft.com</span></span>
    3. <span data-ttu-id="5dd65-220">*.WindowsAzure.com</span><span class="sxs-lookup"><span data-stu-id="5dd65-220">*.WindowsAzure.com</span></span>
    4. <span data-ttu-id="5dd65-221">*.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="5dd65-221">*.microsoftonline.com</span></span>
    5. <span data-ttu-id="5dd65-222">*.windows.ne</span><span class="sxs-lookup"><span data-stu-id="5dd65-222">*.windows.ne</span></span>


## <a name="create-hello-backup-policy"></a><span data-ttu-id="5dd65-223">Hello biztonsági mentési házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="5dd65-223">Create hello backup policy</span></span>
<span data-ttu-id="5dd65-224">a biztonsági mentési házirend hello hello ütemezés akkor, ha a helyreállítási pontokat készít, és mennyi ideig hello helyreállítási pontok megmaradnak hello.</span><span class="sxs-lookup"><span data-stu-id="5dd65-224">hello backup policy is hello schedule when recovery points are taken, and hello length of time hello recovery points are retained.</span></span> <span data-ttu-id="5dd65-225">Hello Microsoft Azure Backup agent toocreate hello biztonsági mentési házirendet használja a fájlok és mappák.</span><span class="sxs-lookup"><span data-stu-id="5dd65-225">Use hello Microsoft Azure Backup agent toocreate hello backup policy for files and folders.</span></span>

### <a name="toocreate-a-backup-schedule"></a><span data-ttu-id="5dd65-226">a biztonsági mentés ütemezését toocreate</span><span class="sxs-lookup"><span data-stu-id="5dd65-226">toocreate a backup schedule</span></span>
1. <span data-ttu-id="5dd65-227">Nyissa meg a Microsoft Azure Backup szolgáltatás ügynökének hello.</span><span class="sxs-lookup"><span data-stu-id="5dd65-227">Open hello Microsoft Azure Backup agent.</span></span> <span data-ttu-id="5dd65-228">A megkereséséhez keressen rá a gépen a **Microsoft Azure Backup** kifejezésre.</span><span class="sxs-lookup"><span data-stu-id="5dd65-228">You can find it by searching your machine for **Microsoft Azure Backup**.</span></span>

    ![Indítsa el a hello Azure Backup szolgáltatás ügynöke](./media/backup-configure-vault/snap-in-search.png)
2. <span data-ttu-id="5dd65-230">A hello Backup szolgáltatás ügynökének **műveletek** ablaktáblán kattintson a **biztonsági mentés ütemezése** toolaunch hello ütemezett biztonsági mentés varázsló.</span><span class="sxs-lookup"><span data-stu-id="5dd65-230">In hello Backup agent's **Actions** pane, click **Schedule Backup** toolaunch hello Schedule Backup Wizard.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-configure-vault/schedule-first-backup.png)

3. <span data-ttu-id="5dd65-232">A hello **bevezetés** hello ütemezett biztonsági mentés varázsló oldalán kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="5dd65-232">On hello **Getting started** page of hello Schedule Backup Wizard, click **Next**.</span></span>
4. <span data-ttu-id="5dd65-233">A hello **elemek kijelölése tooBackup** kattintson **elemek hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="5dd65-233">On hello **Select Items tooBackup** page, click **Add Items**.</span></span>

  <span data-ttu-id="5dd65-234">hello elemek kijelölése párbeszédpanel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="5dd65-234">hello Select Items dialog opens.</span></span>

5. <span data-ttu-id="5dd65-235">Válassza ki a hello fájlokat és mappákat, hogy szeretné, hogy tooprotect, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="5dd65-235">Select hello files and folders that you want tooprotect, and then click **OK**.</span></span>
6. <span data-ttu-id="5dd65-236">A hello **elemek kijelölése tooBackup** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="5dd65-236">In hello **Select Items tooBackup** page, click **Next**.</span></span>
7. <span data-ttu-id="5dd65-237">A hello **adja meg a biztonsági mentés ütemezése** adja meg azokat a hello biztonsági mentési ütemezést, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="5dd65-237">On hello **Specify Backup Schedule** page, specify hello backup schedule and click **Next**.</span></span>

    <span data-ttu-id="5dd65-238">Napi (legfeljebb napi háromszori) vagy heti biztonsági mentéseket ütemezhet.</span><span class="sxs-lookup"><span data-stu-id="5dd65-238">You can schedule daily (at a maximum rate of three times per day) or weekly backups.</span></span>

    ![Windows Server biztonsági mentés elemei](./media/backup-configure-vault/specify-backup-schedule-close.png)

   > [!NOTE]
   > <span data-ttu-id="5dd65-240">Hogyan toospecify hello biztonsági mentés ütemezése kapcsolatos további információkért lásd: hello cikk [használata Azure biztonsági mentési tooreplace a szalag infrastruktúra](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="5dd65-240">For more information about how toospecify hello backup schedule, see hello article [Use Azure Backup tooreplace your tape infrastructure](backup-azure-backup-cloud-as-tape.md).</span></span>
   >
   >

8. <span data-ttu-id="5dd65-241">A hello **válassza ki az adatmegőrzési** lapon válassza ki a hello vélt megőrzési házirendek hello hello biztonsági másolatot, majd kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="5dd65-241">On hello **Select Retention Policy** page, choose hello specific retention policies hello for hello backup copy and click **Next**.</span></span>

    <span data-ttu-id="5dd65-242">hello adatmegőrzési mely hello biztonsági másolatot hello időtartamát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="5dd65-242">hello retention policy specifies hello duration which hello backup is stored.</span></span> <span data-ttu-id="5dd65-243">Ahelyett, hogy csak adja meg az összes biztonsági mentési pont "egyszerű policy", a másik adatmegőrzési hello biztonsági mentés esetén alapján is megadhat.</span><span class="sxs-lookup"><span data-stu-id="5dd65-243">Rather than just specifying a “flat policy” for all backup points, you can specify different retention policies based on when hello backup occurs.</span></span> <span data-ttu-id="5dd65-244">Hello napi, heti, havi és éves megőrzési házirendek toomeet módosíthatja az igényeinek.</span><span class="sxs-lookup"><span data-stu-id="5dd65-244">You can modify hello daily, weekly, monthly, and yearly retention policies toomeet your needs.</span></span>
9. <span data-ttu-id="5dd65-245">Hello kezdeti biztonsági mentési típusának kiválasztása lapon válassza a hello kezdeti biztonsági mentés típusát.</span><span class="sxs-lookup"><span data-stu-id="5dd65-245">On hello Choose Initial Backup Type page, choose hello initial backup type.</span></span> <span data-ttu-id="5dd65-246">Hagyja hello beállítást **automatikusan hello hálózaton keresztül** kiválasztva, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="5dd65-246">Leave hello option **Automatically over hello network** selected, and then click **Next**.</span></span>

    <span data-ttu-id="5dd65-247">Biztonsági másolatot készíthet automatikusan hello hálózaton keresztül, vagy a biztonsági mentést készíthet offline állapotba.</span><span class="sxs-lookup"><span data-stu-id="5dd65-247">You can back up automatically over hello network, or you can back up offline.</span></span> <span data-ttu-id="5dd65-248">hello a cikk hátralévő része a biztonsági mentési automatikusan hello folyamatot írja le.</span><span class="sxs-lookup"><span data-stu-id="5dd65-248">hello remainder of this article describes hello process for backing up automatically.</span></span> <span data-ttu-id="5dd65-249">Ha jobban szeret toodo offline biztonsági másolat, tekintse át a hello cikk [az Azure Backup Offline biztonsági másolat munkafolyamat](backup-azure-backup-import-export.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="5dd65-249">If you prefer toodo an offline backup, review hello article [Offline backup workflow in Azure Backup](backup-azure-backup-import-export.md) for additional information.</span></span>
10. <span data-ttu-id="5dd65-250">A hello megerősítése lapon tekintse át hello adatokat, és kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="5dd65-250">On hello Confirmation page, review hello information, and then click **Finish**.</span></span>
11. <span data-ttu-id="5dd65-251">Hello biztonsági mentési ütemezés létrehozása hello varázsló befejezése után kattintson **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="5dd65-251">After hello wizard finishes creating hello backup schedule, click **Close**.</span></span>

### <a name="enable-network-throttling"></a><span data-ttu-id="5dd65-252">Hálózati sávszélesség-szabályozás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="5dd65-252">Enable network throttling</span></span>
<span data-ttu-id="5dd65-253">hello Microsoft Azure Backup szolgáltatás ügynökének biztosít a hálózati sávszélesség-szabályozás.</span><span class="sxs-lookup"><span data-stu-id="5dd65-253">hello Microsoft Azure Backup agent provides network throttling.</span></span> <span data-ttu-id="5dd65-254">Szabályozza, hogyan adatátvitel során használt hálózati sávszélesség-szabályozás.</span><span class="sxs-lookup"><span data-stu-id="5dd65-254">Throttling controls how network bandwidth is used during data transfer.</span></span> <span data-ttu-id="5dd65-255">Ez a vezérlő akkor lehet hasznos, ha tooback adatokat kell munkaidőben, de nem hello biztonsági mentési folyamat toointerfere a más internetes forgalmat.</span><span class="sxs-lookup"><span data-stu-id="5dd65-255">This control can be helpful if you need tooback up data during work hours but do not want hello backup process toointerfere with other Internet traffic.</span></span> <span data-ttu-id="5dd65-256">Sávszélesség-szabályozás tooback vonatkozik fel, és állítsa vissza a tevékenységeket.</span><span class="sxs-lookup"><span data-stu-id="5dd65-256">Throttling applies tooback up and restore activities.</span></span>

> [!NOTE]
> <span data-ttu-id="5dd65-257">Hálózati sávszélesség-szabályozás nem érhető el a Windows Server 2008 R2 SP1, Windows Server 2008 SP2 vagy Windows 7 (szervizcsomagokkal).</span><span class="sxs-lookup"><span data-stu-id="5dd65-257">Network throttling is not available on Windows Server 2008 R2 SP1, Windows Server 2008 SP2, or Windows 7 (with service packs).</span></span> <span data-ttu-id="5dd65-258">hello Azure biztonsági mentési hálózati sávszélesség-szabályozás szolgáltatás szolgáltatásminőség (QoS) hello helyi operációs rendszer kapcsolatba lép.</span><span class="sxs-lookup"><span data-stu-id="5dd65-258">hello Azure Backup network throttling feature engages Quality of Service (QoS) on hello local operating system.</span></span> <span data-ttu-id="5dd65-259">Bár az Azure biztonsági mentés az említett operációs rendszerektől védelemmel való ellátása hello verziója érhető el, ezek a rendszerek QoS Azure biztonsági mentési hálózati sávszélesség-szabályozás nem működik.</span><span class="sxs-lookup"><span data-stu-id="5dd65-259">Though Azure Backup can protect these operating systems, hello version of QoS available on these platforms doesn't work with Azure Backup network throttling.</span></span> <span data-ttu-id="5dd65-260">Az összes egyéb hálózati sávszélesség-szabályozás használható [támogatott operációs rendszerek](backup-azure-backup-faq.md).</span><span class="sxs-lookup"><span data-stu-id="5dd65-260">Network throttling can be used on all other [supported operating systems](backup-azure-backup-faq.md).</span></span>
>
>

<span data-ttu-id="5dd65-261">**tooenable hálózati sávszélesség-szabályozás**</span><span class="sxs-lookup"><span data-stu-id="5dd65-261">**tooenable network throttling**</span></span>

1. <span data-ttu-id="5dd65-262">Hello Microsoft Azure Backup szolgáltatás ügynökének, kattintson **tulajdonságainak módosítása**.</span><span class="sxs-lookup"><span data-stu-id="5dd65-262">In hello Microsoft Azure Backup agent, click **Change Properties**.</span></span>

    ![Tulajdonságainak módosítása](./media/backup-configure-vault/change-properties.png)
2. <span data-ttu-id="5dd65-264">A hello **sávszélesség-szabályozási** lapra, jelölje be hello **engedélyezi az internetes sávszélesség szabályozásának a biztonsági mentési műveleteknél** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="5dd65-264">On hello **Throttling** tab, select hello **Enable internet bandwidth usage throttling for backup operations** check box.</span></span>

    ![Hálózati sávszélesség-szabályozás](./media/backup-configure-vault/throttling-dialog.png)
3. <span data-ttu-id="5dd65-266">Miután engedélyezte a sávszélesség-szabályozás, adja meg a biztonsági mentési adatátvitel során sávszélesség engedélyezett hello **időpontokat a munkaidőhöz** és **munkaidőn kívüli**.</span><span class="sxs-lookup"><span data-stu-id="5dd65-266">After you have enabled throttling, specify hello allowed bandwidth for backup data transfer during **Work hours** and **Non-work hours**.</span></span>

    <span data-ttu-id="5dd65-267">hello sávszélesség értékek 512 kilobit / másodperc (Kbps) kezdődik, és lépjen be too1, 023 megabájt / másodperc (MBps).</span><span class="sxs-lookup"><span data-stu-id="5dd65-267">hello bandwidth values begin at 512 kilobits per second (Kbps) and can go up too1,023 megabytes per second (MBps).</span></span> <span data-ttu-id="5dd65-268">Is kijelölése hello kezdő és a Befejezés **időpontokat a munkaidőhöz**, és a hello hét mely napján figyelembe vett munkanapok.</span><span class="sxs-lookup"><span data-stu-id="5dd65-268">You can also designate hello start and finish for **Work hours**, and which days of hello week are considered work days.</span></span> <span data-ttu-id="5dd65-269">Óra között útmutatóul szolgálnak a kijelölt munkahelyi kívül óra munkaórákon kívüli időre.</span><span class="sxs-lookup"><span data-stu-id="5dd65-269">Hours outside of designated work hours are considered non-work hours.</span></span>
4. <span data-ttu-id="5dd65-270">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="5dd65-270">Click **OK**.</span></span>

### <a name="tooback-up-files-and-folders-for-hello-first-time"></a><span data-ttu-id="5dd65-271">az első alkalommal hello mappák és fájlok tooback</span><span class="sxs-lookup"><span data-stu-id="5dd65-271">tooback up files and folders for hello first time</span></span>
1. <span data-ttu-id="5dd65-272">Hello biztonságimásolat-készítő ügynök, kattintson **biztonsági másolat készítése most** toocomplete hello kezdeti összehangolása hello hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="5dd65-272">In hello backup agent, click **Back Up Now** toocomplete hello initial seeding over hello network.</span></span>

    ![Windows Server biztonsági másolat készítése](./media/backup-configure-vault/backup-now.png)
2. <span data-ttu-id="5dd65-274">A hello megerősítése lapon, amely a biztonsági mentést most varázsló hello hello beállítások áttekintése hello gép tooback fogja használni.</span><span class="sxs-lookup"><span data-stu-id="5dd65-274">On hello Confirmation page, review hello settings that hello Back Up Now Wizard will use tooback up hello machine.</span></span> <span data-ttu-id="5dd65-275">Ezután kattintson a **Biztonsági mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="5dd65-275">Then click **Back Up**.</span></span>
3. <span data-ttu-id="5dd65-276">Kattintson a **Bezárás** tooclose hello varázsló.</span><span class="sxs-lookup"><span data-stu-id="5dd65-276">Click **Close** tooclose hello wizard.</span></span> <span data-ttu-id="5dd65-277">Ha ezt teszi hello biztonsági mentési folyamat befejezése előtt, hello varázsló toorun hello háttérben folytatódik.</span><span class="sxs-lookup"><span data-stu-id="5dd65-277">If you do this before hello backup process finishes, hello wizard continues toorun in hello background.</span></span>

<span data-ttu-id="5dd65-278">Hello kezdeti biztonsági mentés befejezése után hello **feladata befejezve** állapota megjelenik hello biztonsági mentés konzolban.</span><span class="sxs-lookup"><span data-stu-id="5dd65-278">After hello initial backup is completed, hello **Job completed** status appears in hello Backup console.</span></span>

![IR befejezve](./media/backup-configure-vault/ircomplete.png)

## <a name="questions"></a><span data-ttu-id="5dd65-280">Kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="5dd65-280">Questions?</span></span>
<span data-ttu-id="5dd65-281">Ha kérdése van, vagy ha bármely új szolgáltatása része, toosee szeretné [visszajelzést küldhet](http://aka.ms/azurebackup_feedback).</span><span class="sxs-lookup"><span data-stu-id="5dd65-281">If you have questions, or if there is any feature that you would like toosee included, [send us feedback](http://aka.ms/azurebackup_feedback).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5dd65-282">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5dd65-282">Next steps</span></span>
<span data-ttu-id="5dd65-283">Virtuális gépek vagy más munkaterhelések biztonsági mentésével kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="5dd65-283">For additional information about backing up VMs or other workloads, see:</span></span>

* <span data-ttu-id="5dd65-284">Most, hogy biztonsági másolatot készített a fájlokról és mappákról, [kezelheti a tárlókat és a kiszolgálókat](backup-azure-manage-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="5dd65-284">Now that you've backed up your files and folders, you can [manage your vaults and servers](backup-azure-manage-windows-server.md).</span></span>
* <span data-ttu-id="5dd65-285">Ha szükség toorestore biztonsági másolat, akkor ez a cikk túl[fájlok tooa Windows számítógép visszaállítása](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="5dd65-285">If you need toorestore a backup, use this article too[restore files tooa Windows machine](backup-azure-restore-windows-server.md).</span></span>
