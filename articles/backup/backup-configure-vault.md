---
title: "Azure Backup-ügynököt fájlok és mappák biztonsági mentése |} Microsoft Docs"
description: "A Microsoft Azure Backup szolgáltatás ügynökének használatával Windows-fájlok és mappák biztonsági mentése az Azure-bA. Recovery Services-tároló létrehozása, a biztonsági mentési ügynök telepítése, a biztonsági mentési házirend meghatározása és a fájlok és mappák a kezdeti biztonsági mentés futtatására."
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
ms.openlocfilehash: b95dc0a83d8e5618effb573353f419e1837d30c5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="back-up-a-windows-server-or-client-to-azure-using-the-resource-manager-deployment-model"></a><span data-ttu-id="44fe6-105">Windows-kiszolgálóról vagy -ügyfél biztonsági mentése az Azure-ba a Resource Manager-alapú üzemi modell használatával</span><span class="sxs-lookup"><span data-stu-id="44fe6-105">Back up a Windows Server or client to Azure using the Resource Manager deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="44fe6-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="44fe6-106">Azure portal</span></span>](backup-configure-vault.md)
> * [<span data-ttu-id="44fe6-107">Klasszikus portál</span><span class="sxs-lookup"><span data-stu-id="44fe6-107">Classic portal</span></span>](backup-configure-vault-classic.md)
>
>

<span data-ttu-id="44fe6-108">Ez a cikk azt ismerteti, hogyan biztonsági másolatot készíthet a Windows Server (és a Windows-ügyfél) fájlok és mappák Azure Resource Manager telepítési modellel Azure Backup szolgáltatásnál.</span><span class="sxs-lookup"><span data-stu-id="44fe6-108">This article explains how to back up your Windows Server (or Windows client) files and folders to Azure with Azure Backup using the Resource Manager deployment model.</span></span>

[!INCLUDE [learn-about-deployment-models](../../includes/backup-deployment-models.md)]

![Biztonsági mentési folyamat lépései](./media/backup-configure-vault/initial-backup-process.png)

## <a name="before-you-start"></a><span data-ttu-id="44fe6-110">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="44fe6-110">Before you start</span></span>
<span data-ttu-id="44fe6-111">Biztonsági mentése a kiszolgáló vagy az ügyfél az Azure-ba, az Azure-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="44fe6-111">To back up a server or client to Azure, you need an Azure account.</span></span> <span data-ttu-id="44fe6-112">Ha még nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/) néhány percig.</span><span class="sxs-lookup"><span data-stu-id="44fe6-112">If you don't have one, you can create a [free account](https://azure.microsoft.com/free/) in just a couple of minutes.</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="44fe6-113">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="44fe6-113">Create a Recovery Services vault</span></span>
<span data-ttu-id="44fe6-114">Recovery Services-tároló olyan entitás, amely tárolja a biztonsági mentések és a helyreállítási pontokat hoz létre adott idő alatt.</span><span class="sxs-lookup"><span data-stu-id="44fe6-114">A Recovery Services vault is an entity that stores all the backups and recovery points you create over time.</span></span> <span data-ttu-id="44fe6-115">A Recovery Services-tároló alkalmazott a védett fájlok és mappák biztonsági mentési házirendet is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="44fe6-115">The Recovery Services vault also contains the backup policy applied to the protected files and folders.</span></span> <span data-ttu-id="44fe6-116">Recovery Services-tároló létrehozásakor is válassza ki a megfelelő tárolási redundancia lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="44fe6-116">When you create a Recovery Services vault, you should also select the appropriate storage redundancy option.</span></span>

### <a name="to-create-a-recovery-services-vault"></a><span data-ttu-id="44fe6-117">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="44fe6-117">To create a Recovery Services vault</span></span>
1. <span data-ttu-id="44fe6-118">Ha még nem tette meg, jelentkezzen be az [Azure Portalra](https://portal.azure.com/) az Azure-előfizetésével.</span><span class="sxs-lookup"><span data-stu-id="44fe6-118">If you haven't already done so, sign in to the [Azure Portal](https://portal.azure.com/) using your Azure subscription.</span></span>
2. <span data-ttu-id="44fe6-119">A központi menüben kattintson a **További szolgáltatások** elemre, majd az erőforrások listájában írja be a **Recovery Services** szöveget, és kattintson a **Recovery Services-tárolók** elemre.</span><span class="sxs-lookup"><span data-stu-id="44fe6-119">On the Hub menu, click **More services** and in the list of resources, type **Recovery Services** and click **Recovery Services vaults**.</span></span>

    ![Recovery Services-tároló létrehozása – 1. lépés](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    <span data-ttu-id="44fe6-121">Ha az előfizetés Recovery Services-tárolókat tartalmaz, a tárolók fel vannak sorolva.</span><span class="sxs-lookup"><span data-stu-id="44fe6-121">If there are recovery services vaults in the subscription, the vaults are listed.</span></span>

3. <span data-ttu-id="44fe6-122">A **Recovery Services-tárolók** menüben kattintson a **Hozzáadás** elemre.</span><span class="sxs-lookup"><span data-stu-id="44fe6-122">On the **Recovery Services vaults** menu, click **Add**.</span></span>

    ![Recovery Services-tároló létrehozása – 2. lépés](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    <span data-ttu-id="44fe6-124">Megnyílik a Recovery Services-tároló panelje, a rendszer pedig egy **Név**, **Előfizetés**, **Erőforráscsoport** és **Hely** megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="44fe6-124">The Recovery Services vault blade opens, prompting you to provide a **Name**, **Subscription**, **Resource group**, and **Location**.</span></span>

    ![Recovery Services-tároló létrehozása – 3. lépés](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. <span data-ttu-id="44fe6-126">A **Név** mezőben adjon meg egy egyszerű nevet a tároló azonosításához.</span><span class="sxs-lookup"><span data-stu-id="44fe6-126">For **Name**, enter a friendly name to identify the vault.</span></span> <span data-ttu-id="44fe6-127">A névnek egyedinek kell lennie az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="44fe6-127">The name needs to be unique for the Azure subscription.</span></span> <span data-ttu-id="44fe6-128">Írjon be egy 2–50 karakter hosszúságú nevet.</span><span class="sxs-lookup"><span data-stu-id="44fe6-128">Type a name that contains between 2 and 50 characters.</span></span> <span data-ttu-id="44fe6-129">Ennek egy betűvel kell kezdődnie, és csak betűket, számokat és kötőjeleket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="44fe6-129">It must start with a letter, and can contain only letters, numbers, and hyphens.</span></span>

5. <span data-ttu-id="44fe6-130">Az **Előfizetés** szakaszban, az Azure-előfizetés kiválasztásához használja a legördülő menüt.</span><span class="sxs-lookup"><span data-stu-id="44fe6-130">In the **Subscription** section, use the drop-down menu to choose the Azure subscription.</span></span> <span data-ttu-id="44fe6-131">Ha csak egy előfizetést használ, az az előfizetés jelenik meg, és továbbléphet a következő lépésre.</span><span class="sxs-lookup"><span data-stu-id="44fe6-131">If you use only one subscription, that subscription appears and you can skip to the next step.</span></span> <span data-ttu-id="44fe6-132">Ha nem biztos benne, hogy melyik előfizetést szeretné használni, használja az alapértelmezett (vagy javasolt) előfizetést.</span><span class="sxs-lookup"><span data-stu-id="44fe6-132">If you are not sure which subscription to use, use the default (or suggested) subscription.</span></span> <span data-ttu-id="44fe6-133">Csak akkor lesz több választási lehetőség, ha a szervezetéhez tartozó fiók több Azure-előfizetéssel van összekötve.</span><span class="sxs-lookup"><span data-stu-id="44fe6-133">There are multiple choices only if your organizational account is associated with multiple Azure subscriptions.</span></span>

6. <span data-ttu-id="44fe6-134">Az **Erőforráscsoport** szakaszban:</span><span class="sxs-lookup"><span data-stu-id="44fe6-134">In the **Resource group** section:</span></span>

    * <span data-ttu-id="44fe6-135">válassza az **Új létrehozása** lehetőséget, ha új erőforráscsoportot szeretne létrehozni.</span><span class="sxs-lookup"><span data-stu-id="44fe6-135">select **Create new** if you want to create a new Resource group.</span></span>
    <span data-ttu-id="44fe6-136">Vagy</span><span class="sxs-lookup"><span data-stu-id="44fe6-136">Or</span></span>
    * <span data-ttu-id="44fe6-137">válassza a **Meglévő használata** lehetőséget, és kattintson a legördülő menüben az elérhető erőforráscsoportok listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="44fe6-137">select **Use existing** and click the drop-down menu to see the available list of Resource groups.</span></span>

  <span data-ttu-id="44fe6-138">Átfogó információk az erőforráscsoportokkal kapcsolatban: [Az Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="44fe6-138">For complete information on Resource groups, see the [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

7. <span data-ttu-id="44fe6-139">Kattintson a **Hely** elemre a tárolóhoz tartozó földrajzi régió kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="44fe6-139">Click **Location** to select the geographic region for the vault.</span></span> <span data-ttu-id="44fe6-140">Ez a választás határozza meg a földrajzi régiót, ahová az adatok biztonsági másolata el lesz küldve.</span><span class="sxs-lookup"><span data-stu-id="44fe6-140">This choice determines the geographic region where your backup data is sent.</span></span>

8. <span data-ttu-id="44fe6-141">Kattintson a Recovery Services-tároló panel alján a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="44fe6-141">At the bottom of the Recovery Services vault blade, click **Create**.</span></span>

  <span data-ttu-id="44fe6-142">A Recovery Services-tároló létrehozása több percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="44fe6-142">It can take several minutes for the Recovery Services vault to be created.</span></span> <span data-ttu-id="44fe6-143">Figyelje az állapotértesítéseket a portál jobb felső területén.</span><span class="sxs-lookup"><span data-stu-id="44fe6-143">Monitor the status notifications in the upper right-hand area of the portal.</span></span> <span data-ttu-id="44fe6-144">Miután a tároló létrejött, megjelenik a Recovery Services-tárolók listájában.</span><span class="sxs-lookup"><span data-stu-id="44fe6-144">Once your vault is created, it appears in the list of Recovery Services vaults.</span></span> <span data-ttu-id="44fe6-145">Ha több perc után sem látja a tárolót, kattintson a **Frissítés** gombra.</span><span class="sxs-lookup"><span data-stu-id="44fe6-145">If after several minutes you don't see your vault, click **Refresh**.</span></span>

  ![Kattintson a Frissítés gombra](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

  <span data-ttu-id="44fe6-147">Ha látja a tárolót a Recovery Services-tárolók listájában, készen áll tárhely-redundancia beállítására.</span><span class="sxs-lookup"><span data-stu-id="44fe6-147">Once you see your vault in the list of Recovery Services vaults, you are ready to set the storage redundancy.</span></span>


### <a name="set-storage-redundancy"></a><span data-ttu-id="44fe6-148">Set adattároló redundanciája, amely</span><span class="sxs-lookup"><span data-stu-id="44fe6-148">Set storage redundancy</span></span>
<span data-ttu-id="44fe6-149">Amikor először hoz létre Recovery Services-tárolót, meghatározza a tároló replikálásának módját.</span><span class="sxs-lookup"><span data-stu-id="44fe6-149">When you first create a Recovery Services vault you determine how storage is replicated.</span></span>

1. <span data-ttu-id="44fe6-150">A **Recovery Services-tárolók** panelen kattintson az új tárolóra.</span><span class="sxs-lookup"><span data-stu-id="44fe6-150">From the **Recovery Services vaults** blade, click the new vault.</span></span>

    ![A Recovery Services-tárolók listájából válassza ki az új tárolót](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    <span data-ttu-id="44fe6-152">Ha kiválasztja a tárolót, a **Recovery Services-tároló** panel leszűkül, és a Beállítások panel (*amelynek tetején a tároló neve látható*), valamint a tároló részleteit tartalmazó panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="44fe6-152">When you select the vault, the **Recovery Services vault** blade narrows, and the Settings blade (*which has the name of the vault at the top*) and the vault details blade open.</span></span>

    ![Az új tároló tárolási konfigurációjának beállítása](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)

2. <span data-ttu-id="44fe6-154">Használja a függőleges csúszkát az új tároló Beállítások paneljén a legörgetéshez a Kezelés szakaszhoz, és kattintson a **Biztonsági mentési infrastruktúra** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="44fe6-154">In the new vault's Settings blade, use the vertical slide to scroll down to the Manage section, and click **Backup Infrastructure**.</span></span>

  <span data-ttu-id="44fe6-155">Megnyílik a Biztonsági mentési infrastruktúra panel.</span><span class="sxs-lookup"><span data-stu-id="44fe6-155">The Backup Infrastructure blade opens.</span></span>

3. <span data-ttu-id="44fe6-156">A Biztonsági mentési infrastruktúra panelen kattintson a **Biztonsági mentés konfigurációja** elemre a **Biztonsági mentés konfigurációja** panel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="44fe6-156">In the Backup Infrastructure blade, click **Backup Configuration** to open the **Backup Configuration** blade.</span></span>

  ![Az új tároló tárolási konfigurációjának beállítása](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)

4. <span data-ttu-id="44fe6-158">Válassza ki a megfelelő tárolóreplikációs beállítást a tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="44fe6-158">Choose the appropriate storage replication option for your vault.</span></span>

  ![a tároló konfigurálásának lehetőségei](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

  <span data-ttu-id="44fe6-160">Alapértelmezés szerint a tárolója georedundáns tárolással rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="44fe6-160">By default, your vault has geo-redundant storage.</span></span> <span data-ttu-id="44fe6-161">Ha az Azure-t használja az elsődleges biztonsági mentési tároló végpontjaként, folytassa a **georedundáns** beállítás használatát.</span><span class="sxs-lookup"><span data-stu-id="44fe6-161">If you use Azure as a primary backup storage endpoint, continue to use **Geo-redundant**.</span></span> <span data-ttu-id="44fe6-162">Ha nem az Azure-t használja az elsődleges biztonsági mentési tároló végpontjaként, válassza a **Helyileg redundáns** lehetőséget, amely csökkenti az Azure Storage költségeit.</span><span class="sxs-lookup"><span data-stu-id="44fe6-162">If you don't use Azure as a primary backup storage endpoint, then choose **Locally-redundant**, which reduces the Azure storage costs.</span></span> <span data-ttu-id="44fe6-163">A [georedundáns](../storage/common/storage-redundancy.md#geo-redundant-storage) és a [helyileg redundáns](../storage/common/storage-redundancy.md#locally-redundant-storage) tárolási lehetőségekről többet olvashat ebben a [Tárhely-redundancia áttekintésben](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="44fe6-163">Read more about [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) and [locally redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) storage options in this [Storage redundancy overview](../storage/common/storage-redundancy.md).</span></span>

<span data-ttu-id="44fe6-164">Most, hogy létrehozta a tárolót, készítse elő az infrastruktúrát, letöltése és telepítése a Microsoft Azure Recovery Services Agent ügynököt, töltse le a tároló és ezen hitelesítő adatok használatával regisztrálhatja az ügynököt a fájlok és mappák biztonsági a tároló.</span><span class="sxs-lookup"><span data-stu-id="44fe6-164">Now that you've created a vault, prepare your infrastructure to back up files and folders by downloading and installing the Microsoft Azure Recovery Services agent, downloading vault credentials, and then using those credentials to register the agent with the vault.</span></span>

## <a name="configure-the-vault"></a><span data-ttu-id="44fe6-165">A tároló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="44fe6-165">Configure the vault</span></span>

1. <span data-ttu-id="44fe6-166">A Recovery Services-tároló (az imént létrehozott tároló) paneljén a Bevezetés szakaszban kattintson a **Biztonsági mentés** elemre, majd a **Bevezetés a biztonsági mentés használatába** panelen válassza a **Biztonsági mentés célja** elemet.</span><span class="sxs-lookup"><span data-stu-id="44fe6-166">On the Recovery Services vault blade (for the vault you just created), in the Getting Started section, click **Backup**, then on the **Getting Started with Backup** blade, select **Backup goal**.</span></span>

  ![A biztonsági mentés célja panel megnyitása](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

  <span data-ttu-id="44fe6-168">Megnyílik a **Biztonsági mentés célja** panel.</span><span class="sxs-lookup"><span data-stu-id="44fe6-168">The **Backup Goal** blade opens.</span></span> <span data-ttu-id="44fe6-169">Ha a Recovery Services-tároló korábban lett konfigurálva, majd a **biztonsági mentési cél** paneleken kattintva megjelenik **biztonsági mentés** a Recovery Services tároló panel.</span><span class="sxs-lookup"><span data-stu-id="44fe6-169">If the Recovery Services vault has been previously configured, then the **Backup Goal** blades opens when you click **Backup** on the Recovery Services vault blade.</span></span>

  ![A biztonsági mentés célja panel megnyitása](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. <span data-ttu-id="44fe6-171">A **Hol futnak az alkalmazások és szolgáltatások?** legördülő menüből válassza a **Helyszíni** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="44fe6-171">From the **Where is your workload running?** drop-down menu, select **On-premises**.</span></span>

  <span data-ttu-id="44fe6-172">Azért kell a **Helyszíni** lehetőséget választania, mert a Windows Server- vagy Windows-számítógépe egy fizikai gép, amely nem az Azure része.</span><span class="sxs-lookup"><span data-stu-id="44fe6-172">You choose **On-premises** because your Windows Server or Windows computer is a physical machine that is not in Azure.</span></span>

3. <span data-ttu-id="44fe6-173">A **Miről szeretne biztonsági másolatot készíteni?** menüben válassza a **Fájlok és mappák** lehetőséget, és kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="44fe6-173">From the **What do you want to backup?** menu, select **Files and folders**, and click **OK**.</span></span>

  ![Fájlok és mappák konfigurálása](./media/backup-try-azure-backup-in-10-mins/set-file-folder.png)

  <span data-ttu-id="44fe6-175">Miután az OK gombra kattint, a **Biztonsági mentés célja** mellett megjelenik egy pipa, és megnyílik **Az infrastruktúra előkészítése** panel.</span><span class="sxs-lookup"><span data-stu-id="44fe6-175">After clicking OK, a checkmark appears next to **Backup goal**, and the **Prepare infrastructure** blade opens.</span></span>

  ![Ha konfigurálta a biztonsági mentés célját, a következő lépés az infrastruktúra előkészítése](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. <span data-ttu-id="44fe6-177">**Az infrastruktúra előkészítése** panelen kattintson **A Windows Server- vagy a Windows-ügyfél ügynökének letöltése** elemre.</span><span class="sxs-lookup"><span data-stu-id="44fe6-177">On the **Prepare infrastructure** blade, click **Download Agent for Windows Server or Windows Client**.</span></span>

  ![infrastruktúra előkészítése](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

  <span data-ttu-id="44fe6-179">Ha Windows Server Essential verziót használ, akkor a Windows Server Essential ügynökét töltse le.</span><span class="sxs-lookup"><span data-stu-id="44fe6-179">If you are using Windows Server Essential, then choose to download the agent for Windows Server Essential.</span></span> <span data-ttu-id="44fe6-180">Egy előugró menü rákérdez, hogy futtatni vagy menteni kívánja-e a MARSAgentInstaller.exe fájlt.</span><span class="sxs-lookup"><span data-stu-id="44fe6-180">A pop-up menu prompts you to run or save MARSAgentInstaller.exe.</span></span>

  ![MARSAgentInstaller párbeszédpanel](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. <span data-ttu-id="44fe6-182">A letöltési előugró menüben kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="44fe6-182">In the download pop-up menu, click **Save**.</span></span>

  <span data-ttu-id="44fe6-183">Alapértelmezés szerint az **MARSagentinstaller.exe** fájlt a rendszer a Downloads mappába menti.</span><span class="sxs-lookup"><span data-stu-id="44fe6-183">By default, the **MARSagentinstaller.exe** file is saved to your Downloads folder.</span></span> <span data-ttu-id="44fe6-184">Amikor a telepítő végzett, megjelenik egy előugró ablak, amely rákérdez, hogy szeretné-e futtatni a telepítőt, vagy megnyitni a mappát.</span><span class="sxs-lookup"><span data-stu-id="44fe6-184">When the installer completes, you will see a pop-up asking if you want to run the installer, or open the folder.</span></span>

  ![infrastruktúra előkészítése](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

  <span data-ttu-id="44fe6-186">Az ügynököt még nem kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="44fe6-186">You don't need to install the agent yet.</span></span> <span data-ttu-id="44fe6-187">A tároló hitelesítő adatainak letöltése után telepítheti az ügynököt.</span><span class="sxs-lookup"><span data-stu-id="44fe6-187">You can install the agent after you have downloaded the vault credentials.</span></span>

6. <span data-ttu-id="44fe6-188">**Az infrastruktúra előkészítése** panelen kattintson a **Letöltés** elemre.</span><span class="sxs-lookup"><span data-stu-id="44fe6-188">On the **Prepare infrastructure** blade, click **Download**.</span></span>

  ![a tároló hitelesítő adatainak letöltése](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

  <span data-ttu-id="44fe6-190">A tároló hitelesítő adatait a rendszer a Letöltések mappába menti.</span><span class="sxs-lookup"><span data-stu-id="44fe6-190">The vault credentials download to your Downloads folder.</span></span> <span data-ttu-id="44fe6-191">Miután a tároló hitelesítő adatainak letöltése befejeződött, megjelenik egy előugró ablak, amely rákérdez, hogy szeretné-e megnyitni vagy menteni a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="44fe6-191">After the vault credentials finish downloading, you see a pop-up asking if you want to open or save the credentials.</span></span> <span data-ttu-id="44fe6-192">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="44fe6-192">Click **Save**.</span></span> <span data-ttu-id="44fe6-193">Ha véletlenül a **Megnyitás** gombra kattint, hagyja, hogy sikertelen legyen a párbeszédpanel, amely megpróbálja megnyitni a tároló hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="44fe6-193">If you accidentally click **Open**, let the dialog that attempts to open the vault credentials, fail.</span></span> <span data-ttu-id="44fe6-194">A tároló hitelesítő adatai nem nyithatók meg.</span><span class="sxs-lookup"><span data-stu-id="44fe6-194">You cannot open the vault credentials.</span></span> <span data-ttu-id="44fe6-195">Folytassa a következő lépéssel.</span><span class="sxs-lookup"><span data-stu-id="44fe6-195">Proceed to the next step.</span></span> <span data-ttu-id="44fe6-196">A tároló hitelesítő adatai a Letöltések mappában találhatók.</span><span class="sxs-lookup"><span data-stu-id="44fe6-196">The vault credentials are in the Downloads folder.</span></span>   

  ![a tároló hitelesítő adatainak letöltése befejeződött](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)

## <a name="install-and-register-the-agent"></a><span data-ttu-id="44fe6-198">Az ügynök telepítése és regisztrálása</span><span class="sxs-lookup"><span data-stu-id="44fe6-198">Install and register the agent</span></span>

> [!NOTE]
> <span data-ttu-id="44fe6-199">A biztonsági mentés engedélyezése az Azure Portalon keresztül még nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="44fe6-199">Enabling backup through the Azure portal is not available, yet.</span></span> <span data-ttu-id="44fe6-200">Használja a Microsoft Azure Recovery Services-ügynököt biztonsági másolat készítésére a fájlokról és mappákról.</span><span class="sxs-lookup"><span data-stu-id="44fe6-200">Use the Microsoft Azure Recovery Services Agent to back up your files and folders.</span></span>
>

1. <span data-ttu-id="44fe6-201">Keresse meg és kattintson duplán az **MARSagentinstaller.exe** fájlra a Letöltések mappában (vagy más mentési helyen).</span><span class="sxs-lookup"><span data-stu-id="44fe6-201">Locate and double-click the **MARSagentinstaller.exe** from the Downloads folder (or other saved location).</span></span>

  <span data-ttu-id="44fe6-202">A telepítő egy sor üzenetet jelenít meg, miközben kibontja, telepíti és regisztrálja a Recovery Services-ügynököt.</span><span class="sxs-lookup"><span data-stu-id="44fe6-202">The installer provides a series of messages as it extracts, installs, and registers the Recovery Services agent.</span></span>

  ![a Recovery Services-ügynök telepítőjének futtatása, hitelesítő adatok](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. <span data-ttu-id="44fe6-204">Végezze el a Microsoft Azure Recovery Services ügynök telepítővarázslójának lépéseit.</span><span class="sxs-lookup"><span data-stu-id="44fe6-204">Complete the Microsoft Azure Recovery Services Agent Setup Wizard.</span></span> <span data-ttu-id="44fe6-205">A varázsló lépéseinek elvégzéséhez a következőket kell tennie:</span><span class="sxs-lookup"><span data-stu-id="44fe6-205">To complete the wizard, you need to:</span></span>

  * <span data-ttu-id="44fe6-206">Válasszon egy helyet a telepítés és a gyorsítótár mappája számára.</span><span class="sxs-lookup"><span data-stu-id="44fe6-206">Choose a location for the installation and cache folder.</span></span>
  * <span data-ttu-id="44fe6-207">Adja meg a proxykiszolgáló információit, ha proxykiszolgálóval csatlakozik az internethez.</span><span class="sxs-lookup"><span data-stu-id="44fe6-207">Provide your proxy server info if you use a proxy server to connect to the internet.</span></span>
  * <span data-ttu-id="44fe6-208">Adja meg a felhasználónevének és jelszavának részleteit, ha hitelesített proxyt használ.</span><span class="sxs-lookup"><span data-stu-id="44fe6-208">Provide your user name and password details if you use an authenticated proxy.</span></span>
  * <span data-ttu-id="44fe6-209">Adja meg a tároló letöltött hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="44fe6-209">Provide the downloaded vault credentials</span></span>
  * <span data-ttu-id="44fe6-210">Mentse a titkosítási jelszót egy biztonságos helyen.</span><span class="sxs-lookup"><span data-stu-id="44fe6-210">Save the encryption passphrase in a secure location.</span></span>

  > [!NOTE]
  > <span data-ttu-id="44fe6-211">Ha elveszíti vagy elfelejti a jelszót, a Microsoft nem tud segíteni az adatok biztonsági másolatának visszaállításában.</span><span class="sxs-lookup"><span data-stu-id="44fe6-211">If you lose or forget the passphrase, Microsoft cannot help recover the backup data.</span></span> <span data-ttu-id="44fe6-212">Mentse a fájlt egy biztonságos helyen.</span><span class="sxs-lookup"><span data-stu-id="44fe6-212">Save the file in a secure location.</span></span> <span data-ttu-id="44fe6-213">Erre szükség van a biztonsági másolat visszaállításához.</span><span class="sxs-lookup"><span data-stu-id="44fe6-213">It is required to restore a backup.</span></span>
  >
  >

<span data-ttu-id="44fe6-214">Az ügynök most telepítve van, és a gépe regisztrálva van a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="44fe6-214">The agent is now installed and your machine is registered to the vault.</span></span> <span data-ttu-id="44fe6-215">Készen áll a biztonsági mentés konfigurálására és ütemezésére.</span><span class="sxs-lookup"><span data-stu-id="44fe6-215">You're ready to configure and schedule your backup.</span></span>

## <a name="network-and-connectivity-requirements"></a><span data-ttu-id="44fe6-216">Hálózati és kapcsolati követelmények</span><span class="sxs-lookup"><span data-stu-id="44fe6-216">Network and Connectivity Requirements</span></span>

<span data-ttu-id="44fe6-217">A machine /-proxy korlátozott internet-hozzáférést, ha győződjön meg arról, hogy tűzfal beállításait a machine /-proxy a engedélyezése a következő URL-címekkel vannak konfigurálva:</span><span class="sxs-lookup"><span data-stu-id="44fe6-217">If your machine/proxy has limited internet access, ensure that firewall settings on the machine/proxy are configured to allow the following URLs:</span></span> <br>
    1. <span data-ttu-id="44fe6-218">www.msftncsi.com</span><span class="sxs-lookup"><span data-stu-id="44fe6-218">www.msftncsi.com</span></span>
    2. <span data-ttu-id="44fe6-219">*.Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="44fe6-219">*.Microsoft.com</span></span>
    3. <span data-ttu-id="44fe6-220">*.WindowsAzure.com</span><span class="sxs-lookup"><span data-stu-id="44fe6-220">*.WindowsAzure.com</span></span>
    4. <span data-ttu-id="44fe6-221">*.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="44fe6-221">*.microsoftonline.com</span></span>
    5. <span data-ttu-id="44fe6-222">*.windows.ne</span><span class="sxs-lookup"><span data-stu-id="44fe6-222">*.windows.ne</span></span>


## <a name="create-the-backup-policy"></a><span data-ttu-id="44fe6-223">A biztonsági mentési házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="44fe6-223">Create the backup policy</span></span>
<span data-ttu-id="44fe6-224">A biztonsági mentési házirend az ütemezés, ha a helyreállítási pontokat készít, és mennyi ideig megőrzi a helyreállítási pontok.</span><span class="sxs-lookup"><span data-stu-id="44fe6-224">The backup policy is the schedule when recovery points are taken, and the length of time the recovery points are retained.</span></span> <span data-ttu-id="44fe6-225">A fájlok és mappák biztonsági mentési házirend létrehozásához használja a Microsoft Azure Backup szolgáltatás ügynöke.</span><span class="sxs-lookup"><span data-stu-id="44fe6-225">Use the Microsoft Azure Backup agent to create the backup policy for files and folders.</span></span>

### <a name="to-create-a-backup-schedule"></a><span data-ttu-id="44fe6-226">Biztonsági mentési ütemezés létrehozása</span><span class="sxs-lookup"><span data-stu-id="44fe6-226">To create a backup schedule</span></span>
1. <span data-ttu-id="44fe6-227">Nyissa meg a Microsoft Azure Backup szolgáltatás ügynöke.</span><span class="sxs-lookup"><span data-stu-id="44fe6-227">Open the Microsoft Azure Backup agent.</span></span> <span data-ttu-id="44fe6-228">A megkereséséhez keressen rá a gépen a **Microsoft Azure Backup** kifejezésre.</span><span class="sxs-lookup"><span data-stu-id="44fe6-228">You can find it by searching your machine for **Microsoft Azure Backup**.</span></span>

    ![Indítsa el az Azure Backup szolgáltatás ügynöke](./media/backup-configure-vault/snap-in-search.png)
2. <span data-ttu-id="44fe6-230">A biztonsági mentési ügynök **műveletek** ablaktáblán kattintson a **biztonsági mentés ütemezése** biztonsági mentés ütemezése varázsló indításához.</span><span class="sxs-lookup"><span data-stu-id="44fe6-230">In the Backup agent's **Actions** pane, click **Schedule Backup** to launch the Schedule Backup Wizard.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-configure-vault/schedule-first-backup.png)

3. <span data-ttu-id="44fe6-232">A a **bevezetés** lap az ütemezett biztonsági mentés varázsló kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="44fe6-232">On the **Getting started** page of the Schedule Backup Wizard, click **Next**.</span></span>
4. <span data-ttu-id="44fe6-233">Az a **elemek kijelölése biztonsági mentéshez** kattintson **elemek hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="44fe6-233">On the **Select Items to Backup** page, click **Add Items**.</span></span>

  <span data-ttu-id="44fe6-234">Az elemek kijelölése párbeszédpanel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="44fe6-234">The Select Items dialog opens.</span></span>

5. <span data-ttu-id="44fe6-235">Válassza ki a fájlokat és mappákat védeni, és kattintson a kívánt **OK**.</span><span class="sxs-lookup"><span data-stu-id="44fe6-235">Select the files and folders that you want to protect, and then click **OK**.</span></span>
6. <span data-ttu-id="44fe6-236">Az a **elemek kijelölése biztonsági mentéshez** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="44fe6-236">In the **Select Items to Backup** page, click **Next**.</span></span>
7. <span data-ttu-id="44fe6-237">Az a **adja meg a biztonsági mentés ütemezése** lapon adja meg a biztonsági mentési ütemezést, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="44fe6-237">On the **Specify Backup Schedule** page, specify the backup schedule and click **Next**.</span></span>

    <span data-ttu-id="44fe6-238">Napi (legfeljebb napi háromszori) vagy heti biztonsági mentéseket ütemezhet.</span><span class="sxs-lookup"><span data-stu-id="44fe6-238">You can schedule daily (at a maximum rate of three times per day) or weekly backups.</span></span>

    ![Windows Server biztonsági mentés elemei](./media/backup-configure-vault/specify-backup-schedule-close.png)

   > [!NOTE]
   > <span data-ttu-id="44fe6-240">A biztonsági mentés ütemezésének meghatározásával kapcsolatos további információért tekintse meg a [Use Azure Backup to replace your tape infrastructure](backup-azure-backup-cloud-as-tape.md) (Az Azure Backup használata a szalagos infrastruktúra lecseréléséhez) című cikket.</span><span class="sxs-lookup"><span data-stu-id="44fe6-240">For more information about how to specify the backup schedule, see the article [Use Azure Backup to replace your tape infrastructure](backup-azure-backup-cloud-as-tape.md).</span></span>
   >
   >

8. <span data-ttu-id="44fe6-241">A a **válassza ki az adatmegőrzési** lapon, válassza ki az adott adatmegőrzési szabályokról a a biztonsági másolatot, majd kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="44fe6-241">On the **Select Retention Policy** page, choose the specific retention policies the for the backup copy and click **Next**.</span></span>

    <span data-ttu-id="44fe6-242">Az adatmegőrzési Megadja azt az időtartamot, amely a biztonsági másolatot.</span><span class="sxs-lookup"><span data-stu-id="44fe6-242">The retention policy specifies the duration which the backup is stored.</span></span> <span data-ttu-id="44fe6-243">Ahelyett, hogy mindegyik biztonsági mentési ponthoz „lapos házirendet” határozna meg, különböző megőrzési házirendeket határozhat meg a biztonsági másolat készítésének ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="44fe6-243">Rather than just specifying a “flat policy” for all backup points, you can specify different retention policies based on when the backup occurs.</span></span> <span data-ttu-id="44fe6-244">Igényei szerint módosíthatja a napi, heti, havi és évi megőrzési házirendeket.</span><span class="sxs-lookup"><span data-stu-id="44fe6-244">You can modify the daily, weekly, monthly, and yearly retention policies to meet your needs.</span></span>
9. <span data-ttu-id="44fe6-245">A Kezdeti biztonsági mentés típusának kiválasztása oldalon válassza ki a kezdeti biztonsági mentés típusát.</span><span class="sxs-lookup"><span data-stu-id="44fe6-245">On the Choose Initial Backup Type page, choose the initial backup type.</span></span> <span data-ttu-id="44fe6-246">Hagyja bejelölve az **Automatikusan a hálózaton keresztül** beállítást, majd kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="44fe6-246">Leave the option **Automatically over the network** selected, and then click **Next**.</span></span>

    <span data-ttu-id="44fe6-247">Automatikusan készíthet biztonsági másolatot a hálózaton keresztül, vagy offline készíthet biztonsági másolatot.</span><span class="sxs-lookup"><span data-stu-id="44fe6-247">You can back up automatically over the network, or you can back up offline.</span></span> <span data-ttu-id="44fe6-248">Ezen cikk többi része az automatikus biztonsági mentés folyamatát írja le.</span><span class="sxs-lookup"><span data-stu-id="44fe6-248">The remainder of this article describes the process for backing up automatically.</span></span> <span data-ttu-id="44fe6-249">Ha offline biztonsági mentést szeretne végezni, további információért tekintse meg az [Offline backup workflow in Azure Backup](backup-azure-backup-import-export.md) (Offline biztonsági mentési munkafolyamat az Azure Backupban) című cikket.</span><span class="sxs-lookup"><span data-stu-id="44fe6-249">If you prefer to do an offline backup, review the article [Offline backup workflow in Azure Backup](backup-azure-backup-import-export.md) for additional information.</span></span>
10. <span data-ttu-id="44fe6-250">A Jóváhagyás lapon ellenőrizze az információkat, majd kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="44fe6-250">On the Confirmation page, review the information, and then click **Finish**.</span></span>
11. <span data-ttu-id="44fe6-251">Miután a varázsló befejezte a biztonsági mentési ütemezés létrehozását, kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="44fe6-251">After the wizard finishes creating the backup schedule, click **Close**.</span></span>

### <a name="enable-network-throttling"></a><span data-ttu-id="44fe6-252">Hálózati sávszélesség-szabályozás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="44fe6-252">Enable network throttling</span></span>
<span data-ttu-id="44fe6-253">A Microsoft Azure Backup szolgáltatás ügynökének biztosít a hálózati sávszélesség-szabályozás.</span><span class="sxs-lookup"><span data-stu-id="44fe6-253">The Microsoft Azure Backup agent provides network throttling.</span></span> <span data-ttu-id="44fe6-254">Szabályozza, hogyan adatátvitel során használt hálózati sávszélesség-szabályozás.</span><span class="sxs-lookup"><span data-stu-id="44fe6-254">Throttling controls how network bandwidth is used during data transfer.</span></span> <span data-ttu-id="44fe6-255">Ez a vezérlő akkor lehet hasznos, ha biztonsági kell során az adatokat munkaidő, de nem szeretné a biztonsági mentési folyamat zavarja a más internetes forgalmat.</span><span class="sxs-lookup"><span data-stu-id="44fe6-255">This control can be helpful if you need to back up data during work hours but do not want the backup process to interfere with other Internet traffic.</span></span> <span data-ttu-id="44fe6-256">Sávszélesség-szabályozás biztonsági mentése és visszaállítása tevékenységek vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="44fe6-256">Throttling applies to back up and restore activities.</span></span>

> [!NOTE]
> <span data-ttu-id="44fe6-257">Hálózati sávszélesség-szabályozás nem érhető el a Windows Server 2008 R2 SP1, Windows Server 2008 SP2 vagy Windows 7 (szervizcsomagokkal).</span><span class="sxs-lookup"><span data-stu-id="44fe6-257">Network throttling is not available on Windows Server 2008 R2 SP1, Windows Server 2008 SP2, or Windows 7 (with service packs).</span></span> <span data-ttu-id="44fe6-258">Az Azure biztonsági mentési hálózati szabályozásával szolgáltatásminőség (QoS) a helyi operációs rendszer kapcsolatba lép.</span><span class="sxs-lookup"><span data-stu-id="44fe6-258">The Azure Backup network throttling feature engages Quality of Service (QoS) on the local operating system.</span></span> <span data-ttu-id="44fe6-259">Bár az Azure Backup védheti az ilyen operációs rendszerek, QoS érhető el, ezek a rendszerek verziójának Azure biztonsági mentési hálózati sávszélesség-szabályozás nem működik.</span><span class="sxs-lookup"><span data-stu-id="44fe6-259">Though Azure Backup can protect these operating systems, the version of QoS available on these platforms doesn't work with Azure Backup network throttling.</span></span> <span data-ttu-id="44fe6-260">Az összes egyéb hálózati sávszélesség-szabályozás használható [támogatott operációs rendszerek](backup-azure-backup-faq.md).</span><span class="sxs-lookup"><span data-stu-id="44fe6-260">Network throttling can be used on all other [supported operating systems](backup-azure-backup-faq.md).</span></span>
>
>

<span data-ttu-id="44fe6-261">**Hálózati sávszélesség-szabályozás engedélyezése**</span><span class="sxs-lookup"><span data-stu-id="44fe6-261">**To enable network throttling**</span></span>

1. <span data-ttu-id="44fe6-262">Kattintson a Microsoft Azure Backup szolgáltatás ügynökének **tulajdonságainak módosítása**.</span><span class="sxs-lookup"><span data-stu-id="44fe6-262">In the Microsoft Azure Backup agent, click **Change Properties**.</span></span>

    ![Tulajdonságainak módosítása](./media/backup-configure-vault/change-properties.png)
2. <span data-ttu-id="44fe6-264">Az a **sávszélesség-szabályozási** lapon jelölje be a **engedélyezi az internetes sávszélesség szabályozásának a biztonsági mentési műveleteknél** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="44fe6-264">On the **Throttling** tab, select the **Enable internet bandwidth usage throttling for backup operations** check box.</span></span>

    ![Hálózati sávszélesség-szabályozás](./media/backup-configure-vault/throttling-dialog.png)
3. <span data-ttu-id="44fe6-266">Miután engedélyezte a sávszélesség-szabályozás, adja meg az engedélyezett sávszélesség vonatkozó biztonsági mentési adatátvitel során **időpontokat a munkaidőhöz** és **munkaidőn kívüli**.</span><span class="sxs-lookup"><span data-stu-id="44fe6-266">After you have enabled throttling, specify the allowed bandwidth for backup data transfer during **Work hours** and **Non-work hours**.</span></span>

    <span data-ttu-id="44fe6-267">A sávszélesség értékek 512 kilobit / másodperc (Kbps) kezdődik, és folytathatja a legfeljebb 1,023 megabájt / másodperc (MBps).</span><span class="sxs-lookup"><span data-stu-id="44fe6-267">The bandwidth values begin at 512 kilobits per second (Kbps) and can go up to 1,023 megabytes per second (MBps).</span></span> <span data-ttu-id="44fe6-268">Is kijelölni a kezdő és a Befejezés **időpontokat a munkaidőhöz**, és a hét melyik napjain figyelembe vett munkanapok.</span><span class="sxs-lookup"><span data-stu-id="44fe6-268">You can also designate the start and finish for **Work hours**, and which days of the week are considered work days.</span></span> <span data-ttu-id="44fe6-269">Óra között útmutatóul szolgálnak a kijelölt munkahelyi kívül óra munkaórákon kívüli időre.</span><span class="sxs-lookup"><span data-stu-id="44fe6-269">Hours outside of designated work hours are considered non-work hours.</span></span>
4. <span data-ttu-id="44fe6-270">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="44fe6-270">Click **OK**.</span></span>

### <a name="to-back-up-files-and-folders-for-the-first-time"></a><span data-ttu-id="44fe6-271">A fájlok és mappák biztonsági mentése első alkalommal</span><span class="sxs-lookup"><span data-stu-id="44fe6-271">To back up files and folders for the first time</span></span>
1. <span data-ttu-id="44fe6-272">Kattintson a biztonságimásolat-készítő ügynök **biztonsági másolat készítése most** befejeződik, a kezdeti összehangolása a hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="44fe6-272">In the backup agent, click **Back Up Now** to complete the initial seeding over the network.</span></span>

    ![Windows Server biztonsági másolat készítése](./media/backup-configure-vault/backup-now.png)
2. <span data-ttu-id="44fe6-274">A Jóváhagyás lapon tekintse át azokat a beállításokat, amelyeket a Biztonsági másolat készítése varázsló a gép biztonsági mentéséhez fog használni.</span><span class="sxs-lookup"><span data-stu-id="44fe6-274">On the Confirmation page, review the settings that the Back Up Now Wizard will use to back up the machine.</span></span> <span data-ttu-id="44fe6-275">Ezután kattintson a **Biztonsági mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="44fe6-275">Then click **Back Up**.</span></span>
3. <span data-ttu-id="44fe6-276">A varázsló bezárásához kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="44fe6-276">Click **Close** to close the wizard.</span></span> <span data-ttu-id="44fe6-277">Ha ezt a biztonsági mentési folyamat befejezése előtt teszi, a varázsló továbbra is fut a háttérben.</span><span class="sxs-lookup"><span data-stu-id="44fe6-277">If you do this before the backup process finishes, the wizard continues to run in the background.</span></span>

<span data-ttu-id="44fe6-278">A kezdeti biztonsági mentés befejezése után a **Feladat befejezve** állapot jelenik meg a biztonsági mentési konzolon.</span><span class="sxs-lookup"><span data-stu-id="44fe6-278">After the initial backup is completed, the **Job completed** status appears in the Backup console.</span></span>

![IR befejezve](./media/backup-configure-vault/ircomplete.png)

## <a name="questions"></a><span data-ttu-id="44fe6-280">Kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="44fe6-280">Questions?</span></span>
<span data-ttu-id="44fe6-281">Ha kérdései vannak, vagy van olyan szolgáltatás, amelyről hallani szeretne, [küldjön visszajelzést](http://aka.ms/azurebackup_feedback).</span><span class="sxs-lookup"><span data-stu-id="44fe6-281">If you have questions, or if there is any feature that you would like to see included, [send us feedback](http://aka.ms/azurebackup_feedback).</span></span>

## <a name="next-steps"></a><span data-ttu-id="44fe6-282">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="44fe6-282">Next steps</span></span>
<span data-ttu-id="44fe6-283">Virtuális gépek vagy más munkaterhelések biztonsági mentésével kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="44fe6-283">For additional information about backing up VMs or other workloads, see:</span></span>

* <span data-ttu-id="44fe6-284">Most, hogy biztonsági másolatot készített a fájlokról és mappákról, [kezelheti a tárlókat és a kiszolgálókat](backup-azure-manage-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="44fe6-284">Now that you've backed up your files and folders, you can [manage your vaults and servers](backup-azure-manage-windows-server.md).</span></span>
* <span data-ttu-id="44fe6-285">Ha vissza kell állítania egy biztonsági másolatot, ezzel a cikkel [állíthat vissza fájlokat Windows rendszerű gépre](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="44fe6-285">If you need to restore a backup, use this article to [restore files to a Windows machine](backup-azure-restore-windows-server.md).</span></span>
