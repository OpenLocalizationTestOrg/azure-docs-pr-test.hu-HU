---
title: "aaaManage az Azure recovery services-tárolók és a kiszolgálók |} Microsoft Docs"
description: "Az oktatóanyag toolearn hogyan toomanage az Azure recovery services-tárolók és a kiszolgálók használata."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: tysonn
ms.assetid: 4eea984b-7ed6-4600-ac60-99d2e9cb6d8a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: markgal
ms.openlocfilehash: b4c35c86faa0828b3c63a13b85c095c0cbaba50e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-azure-recovery-services-vaults-and-servers-for-windows-machines"></a><span data-ttu-id="0f25f-103">Windows-gépek Azure Recovery Services-tárolóinak és -kiszolgálóinak figyelése és kezelése</span><span class="sxs-lookup"><span data-stu-id="0f25f-103">Monitor and manage Azure recovery services vaults and servers for Windows machines</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0f25f-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="0f25f-104">Resource Manager</span></span>](backup-azure-manage-windows-server.md)
> * [<span data-ttu-id="0f25f-105">Klasszikus</span><span class="sxs-lookup"><span data-stu-id="0f25f-105">Classic</span></span>](backup-azure-manage-windows-server-classic.md)
>
>

<span data-ttu-id="0f25f-106">Ebben a cikkben találhat hello áttekintést hello Azure portál és hello Microsoft Azure biztonságimásolat-készítő ügynök keresztül elérhető biztonsági mentési figyelő és a felügyeleti feladatokat.</span><span class="sxs-lookup"><span data-stu-id="0f25f-106">In this article you'll find an overview of hello backup monitor and management tasks available through hello Azure portal and hello Microsoft Azure Backup agent.</span></span> <span data-ttu-id="0f25f-107">Ez a cikk azt feltételezi, hogy már rendelkezik Azure-előfizetéssel, és legalább egy Recovery Services-tároló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0f25f-107">This article assumes you already have an Azure subscription and have created at least one Recovery Services vault.</span></span>

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]


## <a name="open-a-recovery-services-vault"></a><span data-ttu-id="0f25f-108">Nyissa meg a Recovery Services-tároló</span><span class="sxs-lookup"><span data-stu-id="0f25f-108">Open a Recovery Services vault</span></span>

<span data-ttu-id="0f25f-109">hello Recovery Services-tároló irányítópult látható hello részleteit, illetve a Recovery Services-tároló attribútumait.</span><span class="sxs-lookup"><span data-stu-id="0f25f-109">hello Recovery Services vault dashboard shows you hello details or attributes of a Recovery Services vault.</span></span>

1. <span data-ttu-id="0f25f-110">Jelentkezzen be toohello [Azure Portal](https://portal.azure.com/) használata az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="0f25f-110">Sign in toohello [Azure Portal](https://portal.azure.com/) using your Azure subscription.</span></span>
2. <span data-ttu-id="0f25f-111">Hello központ menüben kattintson a **több szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="0f25f-111">On hello Hub menu, click **More Services**.</span></span>

    ![Recovery Services-tárolók 1.lépés listájának megnyitása](./media/backup-azure-manage-windows-server/open-rs-vault-list.png) <br/>

3. <span data-ttu-id="0f25f-113">Azt szeretné, hogy tooopen Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="0f25f-113">You want tooopen a Recovery Services vault.</span></span> <span data-ttu-id="0f25f-114">A hello párbeszédpanelen kezdje el begépelni **Recovery Services**.</span><span class="sxs-lookup"><span data-stu-id="0f25f-114">In hello dialog box, start typing **Recovery Services**.</span></span> <span data-ttu-id="0f25f-115">Írja be megkezdése előtt, hello listát szűrheti a megadott feltételeknek.</span><span class="sxs-lookup"><span data-stu-id="0f25f-115">As you begin typing, hello list will filter based on your input.</span></span> <span data-ttu-id="0f25f-116">Kattintson a **Recovery Services-tárolók** toodisplay hello listája Recovery Services-tárolók az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="0f25f-116">Click **Recovery Services vaults** toodisplay hello list of Recovery Services vaults in your subscription.</span></span>

    ![Recovery Services-tároló létrehozása – 1. lépés](./media/backup-azure-manage-windows-server/browse-to-rs-vaults-2.png) <br/>

    <span data-ttu-id="0f25f-118">Recovery Services-tárolók hello listáját nyitja meg.</span><span class="sxs-lookup"><span data-stu-id="0f25f-118">hello list of Recovery Services vaults opens.</span></span>

    ![Recovery Services-tároló létrehozása – 1. lépés](./media/backup-azure-manage-windows-server/list-of-rs-vaults.png) <br/>

4. <span data-ttu-id="0f25f-120">Tárolók hello listában jelölje ki azt szeretné, hogy tooopen Recovery Services-tároló hello hello neve.</span><span class="sxs-lookup"><span data-stu-id="0f25f-120">From hello list of vaults, select hello name of hello Recovery Services vault you want tooopen.</span></span> <span data-ttu-id="0f25f-121">hello Recovery Services-tároló irányítópult panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="0f25f-121">hello Recovery Services vault dashboard blade opens.</span></span>

    ![helyreállítási szolgáltatások tároló irányítópult](./media/backup-azure-manage-windows-server/rs-vault-blade.png) <br/>

    <span data-ttu-id="0f25f-123">Most, hogy a Recovery Services-tároló hello nyitotta meg, próbálkozzon a hello figyelés vagy a felügyeleti feladatokat.</span><span class="sxs-lookup"><span data-stu-id="0f25f-123">Now that you have opened hello Recovery Services vault, try any of hello monitoring or management tasks.</span></span>

## <a name="monitor-backup-jobs-and-alerts"></a><span data-ttu-id="0f25f-124">A figyelő biztonsági mentési feladatok és riasztások</span><span class="sxs-lookup"><span data-stu-id="0f25f-124">Monitor backup jobs and alerts</span></span>

<span data-ttu-id="0f25f-125">Feladatok figyelése és hello riasztásait Recovery Services tároló irányítópult, ahol láthatja:</span><span class="sxs-lookup"><span data-stu-id="0f25f-125">You monitor jobs and alerts from hello Recovery Services vault dashboard, where you see:</span></span>

* <span data-ttu-id="0f25f-126">Biztonsági mentési riasztás részletei</span><span class="sxs-lookup"><span data-stu-id="0f25f-126">Backup alerts details</span></span>
* <span data-ttu-id="0f25f-127">Fájlok és mappák, valamint a védett hello felhőben Azure virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="0f25f-127">Files and folders, as well as Azure virtual machines protected in hello cloud</span></span>
* <span data-ttu-id="0f25f-128">Teljes Azure-ban felhasznált tárterület</span><span class="sxs-lookup"><span data-stu-id="0f25f-128">Total storage consumed in Azure</span></span>
* <span data-ttu-id="0f25f-129">Biztonsági mentési feladat állapota</span><span class="sxs-lookup"><span data-stu-id="0f25f-129">Backup job status</span></span>

![Biztonsági mentési irányítópult feladatok](./media/backup-azure-manage-windows-server/dashboard-tiles.png)

<span data-ttu-id="0f25f-131">Hello információt az egyes ezen csempék kattintva megnyílik a hello társított panel, ahol a kapcsolódó feladatok kezelése.</span><span class="sxs-lookup"><span data-stu-id="0f25f-131">Clicking hello information in each of these tiles will open hello associated blade where you manage related tasks.</span></span>

<span data-ttu-id="0f25f-132">Az irányítópult hello hello elejéhez:</span><span class="sxs-lookup"><span data-stu-id="0f25f-132">From hello top of hello Dashboard:</span></span>

* <span data-ttu-id="0f25f-133">Beállítások hozzáférést biztosít az elérhető biztonsági mentési feladatokat.</span><span class="sxs-lookup"><span data-stu-id="0f25f-133">Settings provides access available backup tasks.</span></span>
* <span data-ttu-id="0f25f-134">Biztonsági mentés - segítségével biztonsági mentést új fájlok és mappák (vagy Azure virtuális gépeken) toohello Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="0f25f-134">Backup - helps you back up new files and folders (or Azure VMs) toohello Recovery Services vault.</span></span>
* <span data-ttu-id="0f25f-135">Törlés – ha egy helyreállítási szolgáltatások tároló már nem használja, törölheti azt toofree tárolási helyet.</span><span class="sxs-lookup"><span data-stu-id="0f25f-135">Delete - If a recovery services vault is no longer being used, you can delete it toofree up storage space.</span></span> <span data-ttu-id="0f25f-136">Törlése csak akkor engedélyezett, miután az összes védett kiszolgálók, hogy törölték a hello tárolójából.</span><span class="sxs-lookup"><span data-stu-id="0f25f-136">Delete is only enabled after all protected servers have been deleted from hello vault.</span></span>

![Biztonsági mentési irányítópult feladatok](./media/backup-azure-manage-windows-server/dashboard-tasks.png)

## <a name="alerts-for-backups-using-azure-backup-agent"></a><span data-ttu-id="0f25f-138">A biztonsági mentéseket az Azure Backup szolgáltatás ügynöke riasztások:</span><span class="sxs-lookup"><span data-stu-id="0f25f-138">Alerts for backups using Azure backup agent:</span></span>
| <span data-ttu-id="0f25f-139">Riasztási szint</span><span class="sxs-lookup"><span data-stu-id="0f25f-139">Alert Level</span></span> | <span data-ttu-id="0f25f-140">Értesítések küldése</span><span class="sxs-lookup"><span data-stu-id="0f25f-140">Alerts sent</span></span> |
| --- | --- |
| <span data-ttu-id="0f25f-141">Kritikus</span><span class="sxs-lookup"><span data-stu-id="0f25f-141">Critical</span></span> |<span data-ttu-id="0f25f-142">Sikertelen biztonsági mentéshez, helyreállítási hiba</span><span class="sxs-lookup"><span data-stu-id="0f25f-142">Backup failure, recovery failure</span></span> |
| <span data-ttu-id="0f25f-143">Figyelmeztetés</span><span class="sxs-lookup"><span data-stu-id="0f25f-143">Warning</span></span> |<span data-ttu-id="0f25f-144">A biztonsági mentés befejeződött, de figyelmeztetéseket generált (Ha kevesebb mint száz fájlok nem készül biztonsági mentés toocorruption problémák miatt, és több mint egymillió fájlok sikeresen biztonsági mentése)</span><span class="sxs-lookup"><span data-stu-id="0f25f-144">Backup completed with warnings (when fewer than one hundred files are not backed up due toocorruption issues, and more than one million files are successfully backed up)</span></span> |
| <span data-ttu-id="0f25f-145">Tájékoztató</span><span class="sxs-lookup"><span data-stu-id="0f25f-145">Informational</span></span> |<span data-ttu-id="0f25f-146">None</span><span class="sxs-lookup"><span data-stu-id="0f25f-146">None</span></span> |

## <a name="manage-backup-alerts"></a><span data-ttu-id="0f25f-147">Biztonsági riasztások kezelése</span><span class="sxs-lookup"><span data-stu-id="0f25f-147">Manage Backup alerts</span></span>
<span data-ttu-id="0f25f-148">Hello kattintson **biztonsági mentési riasztások** csempe tooopen hello **biztonsági mentési riasztások** panel és kezelheti a riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="0f25f-148">Click hello **Backup Alerts** tile tooopen hello **Backup Alerts** blade and manage alerts.</span></span>

![Biztonsági mentési riasztás](./media/backup-azure-manage-windows-server/manage-backup-alerts.png)

<span data-ttu-id="0f25f-150">hello biztonsági riasztások csempe megjeleníti hello száma:</span><span class="sxs-lookup"><span data-stu-id="0f25f-150">hello Backup Alerts tile shows you hello number of:</span></span>

* <span data-ttu-id="0f25f-151">kritikus riasztás feloldva a legutóbbi 24 órában</span><span class="sxs-lookup"><span data-stu-id="0f25f-151">critical alerts unresolved in last 24 hours</span></span>
* <span data-ttu-id="0f25f-152">az elmúlt 24 órában feloldatlan figyelmeztető riasztások</span><span class="sxs-lookup"><span data-stu-id="0f25f-152">warning alerts unresolved in last 24 hours</span></span>

<span data-ttu-id="0f25f-153">Kattintson a valamennyi kapcsolatot tart toohello **biztonsági mentési riasztások** szűrt láthassák ezeket a riasztásokat (kritikus vagy figyelmeztetési) panelen.</span><span class="sxs-lookup"><span data-stu-id="0f25f-153">Clicking on each of these links takes you toohello **Backup Alerts** blade with a filtered view of these alerts (critical or warning).</span></span>

<span data-ttu-id="0f25f-154">A hello biztonsági riasztások panel hogy:</span><span class="sxs-lookup"><span data-stu-id="0f25f-154">From hello Backup Alerts blade, you:</span></span>

* <span data-ttu-id="0f25f-155">Válassza ki a riasztások hello megfelelő adatokat tooinclude.</span><span class="sxs-lookup"><span data-stu-id="0f25f-155">Choose hello appropriate information tooinclude with your alerts.</span></span>

    ![Oszlopok kiválasztása](./media/backup-azure-manage-windows-server/choose-alerts-colunms.png)
* <span data-ttu-id="0f25f-157">Riasztások szűrése a súlyosság, állapota és a kezdő és záró időpont.</span><span class="sxs-lookup"><span data-stu-id="0f25f-157">Filter alerts for severity, status and start/end times.</span></span>

    ![Riasztások szűrése](./media/backup-azure-manage-windows-server/filter-alerts.png)
* <span data-ttu-id="0f25f-159">Súlyosság, a gyakoriság és a címzettek értesítések konfigurálása, valamint figyelmeztetések engedélyezése vagy letiltása.</span><span class="sxs-lookup"><span data-stu-id="0f25f-159">Configure notifications for severity, frequency and recipients, as well as turn alerts on or off.</span></span>

    ![Riasztások szűrése](./media/backup-azure-manage-windows-server/configure-notifications.png)

<span data-ttu-id="0f25f-161">Ha **/ riasztási** hello választotta **értesítendő** gyakoriság nincs csoportosítás vagy e-mailek csökkenése történik.</span><span class="sxs-lookup"><span data-stu-id="0f25f-161">If **Per Alert** is selected as hello **Notify** frequency no grouping or reduction in emails occurs.</span></span> <span data-ttu-id="0f25f-162">Minden egyes riasztás 1 értesítési eredményez.</span><span class="sxs-lookup"><span data-stu-id="0f25f-162">Every alert results in 1 notification.</span></span> <span data-ttu-id="0f25f-163">Ez az alapértelmezett beállítás hello és hello feloldási e-mailt is küld azonnal.</span><span class="sxs-lookup"><span data-stu-id="0f25f-163">This is hello default setting and hello resolution email is also sent out immediately.</span></span>

<span data-ttu-id="0f25f-164">Ha **óránkénti kivonatoló** hello választotta **értesítendő** egy e-mailt küld toohello felhasználói közölve, hogy van a gyakoriság feloldatlan új riasztások jönnek létre hello az elmúlt egy óra.</span><span class="sxs-lookup"><span data-stu-id="0f25f-164">If **Hourly Digest** is selected as hello **Notify** frequency one email is sent toohello user telling them that there are unresolved new alerts generated in hello last hour.</span></span> <span data-ttu-id="0f25f-165">A feloldási e-mail által kiküldött hello óra hello végén.</span><span class="sxs-lookup"><span data-stu-id="0f25f-165">A resolution email is sent out at hello end of hello hour.</span></span>

<span data-ttu-id="0f25f-166">Is elküldi a riasztásokat a következő súlyossági szintek hello:</span><span class="sxs-lookup"><span data-stu-id="0f25f-166">Alerts can be sent for hello following severity levels:</span></span>

* <span data-ttu-id="0f25f-167">Kritikus</span><span class="sxs-lookup"><span data-stu-id="0f25f-167">critical</span></span>
* <span data-ttu-id="0f25f-168">Figyelmeztetés</span><span class="sxs-lookup"><span data-stu-id="0f25f-168">warning</span></span>
* <span data-ttu-id="0f25f-169">Információ</span><span class="sxs-lookup"><span data-stu-id="0f25f-169">information</span></span>

<span data-ttu-id="0f25f-170">Hello hello riasztás inaktiválja **inaktiválása** hello feladat részletei panel gombjára.</span><span class="sxs-lookup"><span data-stu-id="0f25f-170">You inactivate hello alert with hello **inactivate** button in hello job details blade.</span></span> <span data-ttu-id="0f25f-171">Kattintva inaktiválása, megadhatja a feloldási megjegyzések.</span><span class="sxs-lookup"><span data-stu-id="0f25f-171">When you click inactivate, you can provide resolution notes.</span></span>

<span data-ttu-id="0f25f-172">Hello oszlopok kiválasztása tooappear kívánt hello hello riasztás részeként **oszlopok kiválasztása** gombra.</span><span class="sxs-lookup"><span data-stu-id="0f25f-172">You choose hello columns you want tooappear as part of hello alert with hello **Choose columns** button.</span></span>

> [!NOTE]
> <span data-ttu-id="0f25f-173">A hello **beállítások** panelen, kezelheti a biztonsági mentési riasztás kiválasztásával **figyelés és jelentéskészítés > riasztások és események > biztonsági mentésekkel kapcsolatos riasztások** , majd kattintson **szűrő** vagy  **Értesítések konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="0f25f-173">From hello **Settings** blade, you manage backup alerts by selecting **Monitoring and Reports > Alerts and Events > Backup Alerts** and then clicking **Filter** or **Configure Notifications**.</span></span>
>
>

## <a name="manage-backup-items"></a><span data-ttu-id="0f25f-174">Biztonsági mentés elemek kezelése</span><span class="sxs-lookup"><span data-stu-id="0f25f-174">Manage Backup items</span></span>
<span data-ttu-id="0f25f-175">A helyi biztonsági kezelése mostantól elérhető hello felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="0f25f-175">Managing on-premises backups is now available in hello management portal.</span></span> <span data-ttu-id="0f25f-176">Hello irányítópult hello biztonsági mentés területen hello **biztonsági másolati elemei** csempe megjeleníti hello biztonsági mentési elemszáma védett toohello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="0f25f-176">In hello Backup section of hello dashboard, hello **Backup Items** tile shows hello number of backup items protected toohello vault.</span></span>

<span data-ttu-id="0f25f-177">Kattintson a **-mappákban** a biztonsági mentés elemek csempe hello.</span><span class="sxs-lookup"><span data-stu-id="0f25f-177">Click **File-Folders** in hello Backup Items tile.</span></span>

![Biztonsági másolati elemei csempe](./media/backup-azure-manage-windows-server/backup-items-tile.png)

<span data-ttu-id="0f25f-179">hello biztonsági másolati elemei panel nyílik meg hello szűrése minden felsorolt elem biztonsági másolat megtapasztalhatja tooFile-mappa.</span><span class="sxs-lookup"><span data-stu-id="0f25f-179">hello Backup Items blade opens with hello filter set tooFile-Folder where you see each specific backup item listed.</span></span>

![Biztonsági másolati elemei](./media/backup-azure-manage-windows-server/backup-item-list.png)

<span data-ttu-id="0f25f-181">Ha egy biztonsági mentési cikket hello listából válassza ki, hello alapvető adatait, hogy az elem látható.</span><span class="sxs-lookup"><span data-stu-id="0f25f-181">If you select a specific backup item from hello list, you see hello essential details for that item.</span></span>

> [!NOTE]
> <span data-ttu-id="0f25f-182">A hello **beállítások** panelen, kezelheti a fájlok és mappák kiválasztásával **védett elemek > biztonsági mentés elemek** és jelölje be **-mappákban** hello a legördülő menü.</span><span class="sxs-lookup"><span data-stu-id="0f25f-182">From hello **Settings** blade, you manage files and folders by selecting **Protected Items > Backup Items** and then selecting **File-Folders** from hello drop down menu.</span></span>
>
>

![A beállítások biztonsági másolati elemei](./media/backup-azure-manage-windows-server/backup-files-and-folders.png)

## <a name="manage-backup-jobs"></a><span data-ttu-id="0f25f-184">Biztonsági mentési feladatok kezelése</span><span class="sxs-lookup"><span data-stu-id="0f25f-184">Manage Backup jobs</span></span>
<span data-ttu-id="0f25f-185">Mind a helyszíni (ha hello helyszíni kiszolgáló biztonsági másolatot készít a tooAzure), és az Azure biztonsági mentések tartozó biztonsági mentési feladatok hello irányítópult láthatók.</span><span class="sxs-lookup"><span data-stu-id="0f25f-185">Backup jobs for both on-premises (when hello on-premises server is backing up tooAzure) and Azure backups are visible in hello dashboard.</span></span>

<span data-ttu-id="0f25f-186">A biztonsági mentés szakasz hello irányítópult hello hello biztonsági mentési feladat csempe feladatok hello számát jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="0f25f-186">In hello Backup section of hello dashboard, hello Backup job tile shows hello number of jobs:</span></span>

* <span data-ttu-id="0f25f-187">folyamatban van</span><span class="sxs-lookup"><span data-stu-id="0f25f-187">in progress</span></span>
* <span data-ttu-id="0f25f-188">nem sikerült hello utolsó 24 órában.</span><span class="sxs-lookup"><span data-stu-id="0f25f-188">failed in hello last 24 hours.</span></span>

<span data-ttu-id="0f25f-189">toomanage a biztonsági mentési feladatok kattintson hello **biztonsági mentési feladatok** csempe, amely hello biztonsági mentési feladatok paneljének megnyitása.</span><span class="sxs-lookup"><span data-stu-id="0f25f-189">toomanage your backup jobs, click hello **Backup Jobs** tile, which opens hello Backup Jobs blade.</span></span>

![A beállítások biztonsági másolati elemei](./media/backup-azure-manage-windows-server/backup-jobs.png)

<span data-ttu-id="0f25f-191">Hello információk érhetők el a biztonsági mentési feladatok panelről hello hello módosítása **oszlopok kiválasztása** hello oldal hello tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="0f25f-191">You modify hello information available in hello Backup Jobs blade with hello **Choose columns** button at hello top of hello page.</span></span>

<span data-ttu-id="0f25f-192">Használjon hello **szűrő** gomb tooselect fájlok és mappák és az Azure virtuális gép biztonsági mentése között.</span><span class="sxs-lookup"><span data-stu-id="0f25f-192">Use hello **Filter** button tooselect between Files and folders and Azure virtual machine backup.</span></span>

<span data-ttu-id="0f25f-193">Ha nem látja a biztonsági mentés fájlokat és mappákat, kattintson a **szűrő** hello lap, és válassza ki a hello tetején gomb **fájlok és mappák** hello elemtípus menüből.</span><span class="sxs-lookup"><span data-stu-id="0f25f-193">If you don't see your backed up files and folders, click **Filter** button at hello top of hello page and select **Files and folders** from hello Item Type menu.</span></span>

> [!NOTE]
> <span data-ttu-id="0f25f-194">A hello **beállítások** panelen kiválasztásával kezelheti a biztonsági mentési feladatok **figyelés és jelentéskészítés > feladatok > biztonsági mentési feladatok** és jelölje be **-mappákban** hello legördülő menüből menüre.</span><span class="sxs-lookup"><span data-stu-id="0f25f-194">From hello **Settings** blade, you manage backup jobs by selecting **Monitoring and Reports > Jobs > Backup Jobs** and then selecting **File-Folders** from hello drop down menu.</span></span>
>
>

## <a name="monitor-backup-usage"></a><span data-ttu-id="0f25f-195">Biztonsági másolat használatának figyelése</span><span class="sxs-lookup"><span data-stu-id="0f25f-195">Monitor Backup usage</span></span>
<span data-ttu-id="0f25f-196">A biztonsági mentés szakasz hello irányítópult hello hello biztonsági mentés használata csempe az Azure-ban felhasznált hello tárhely jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="0f25f-196">In hello Backup section of hello dashboard, hello Backup Usage tile shows hello storage consumed in Azure.</span></span> <span data-ttu-id="0f25f-197">Tárhelyhasználatot biztosított:</span><span class="sxs-lookup"><span data-stu-id="0f25f-197">Storage usage is provided for:</span></span>

* <span data-ttu-id="0f25f-198">A felhőalapú LRS storage használata hello tárolóhoz társított</span><span class="sxs-lookup"><span data-stu-id="0f25f-198">Cloud LRS storage usage associated with hello vault</span></span>
* <span data-ttu-id="0f25f-199">A felhőalapú Georedundáns tárolás használata hello tárolóhoz társított</span><span class="sxs-lookup"><span data-stu-id="0f25f-199">Cloud GRS storage usage associated with hello vault</span></span>

## <a name="manage-your-production-servers"></a><span data-ttu-id="0f25f-200">Az üzemi kiszolgálók kezelése</span><span class="sxs-lookup"><span data-stu-id="0f25f-200">Manage your production servers</span></span>
<span data-ttu-id="0f25f-201">toomanage az üzemi kiszolgálók kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="0f25f-201">toomanage your production servers, click **Settings**.</span></span>

<span data-ttu-id="0f25f-202">Kattintson a kezelés **biztonsági infrastruktúra > az üzemi kiszolgálók**.</span><span class="sxs-lookup"><span data-stu-id="0f25f-202">Under Manage click **Backup infrastructure > Production Servers**.</span></span>

<span data-ttu-id="0f25f-203">hello az üzemi kiszolgálók panel listák az összes rendelkezésre álló üzemi kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="0f25f-203">hello Production Servers blade lists of all your available production servers.</span></span> <span data-ttu-id="0f25f-204">Kattintson az adott kiszolgálón az hello lista tooopen hello kiszolgáló adatait.</span><span class="sxs-lookup"><span data-stu-id="0f25f-204">Click on a server in hello list tooopen hello server details.</span></span>

![Védett elemek](./media/backup-azure-manage-windows-server/production-server-list.png)


## <a name="open-hello-azure-backup-agent"></a><span data-ttu-id="0f25f-206">Nyissa meg hello Azure Backup szolgáltatás ügynöke</span><span class="sxs-lookup"><span data-stu-id="0f25f-206">Open hello Azure Backup agent</span></span>
<span data-ttu-id="0f25f-207">Nyissa meg hello **Microsoft Azure Backup szolgáltatás ügynökének** (rákeresve a gépen található *a Microsoft Azure Backup szolgáltatás*).</span><span class="sxs-lookup"><span data-stu-id="0f25f-207">Open hello **Microsoft Azure Backup agent** (you find it by searching your machine for *Microsoft Azure Backup*).</span></span>

![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/snap-in-search.png)

<span data-ttu-id="0f25f-209">A hello **műveletek** hello sarkában hello biztonságimásolat-készítő ügynök konzolon elérhető hajt végre a következő felügyeleti feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="0f25f-209">From hello **Actions** available at hello right of hello backup agent console you perform hello following management tasks:</span></span>

* <span data-ttu-id="0f25f-210">Kiszolgáló regisztrálása</span><span class="sxs-lookup"><span data-stu-id="0f25f-210">Register Server</span></span>
* <span data-ttu-id="0f25f-211">Biztonsági mentés ütemezése</span><span class="sxs-lookup"><span data-stu-id="0f25f-211">Schedule Backup</span></span>
* <span data-ttu-id="0f25f-212">Biztonsági másolat készítése</span><span class="sxs-lookup"><span data-stu-id="0f25f-212">Back Up now</span></span>
* <span data-ttu-id="0f25f-213">Tulajdonságainak módosítása</span><span class="sxs-lookup"><span data-stu-id="0f25f-213">Change Properties</span></span>

![A Microsoft Azure Backup szolgáltatás ügynöke konzol műveletek](./media/backup-azure-manage-windows-server/console-actions.png)

> [!NOTE]
> <span data-ttu-id="0f25f-215">túl**adatok helyreállítása**, lásd: [fájlok tooa Windows server vagy Windows-ügyfélszámítógép visszaállítása](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="0f25f-215">too**Recover Data**, see [Restore files tooa Windows server or Windows client machine](backup-azure-restore-windows-server.md).</span></span>
>
>

## <a name="modify-hello-backup-schedule"></a><span data-ttu-id="0f25f-216">Hello biztonsági mentési ütemezés módosítása</span><span class="sxs-lookup"><span data-stu-id="0f25f-216">Modify hello backup schedule</span></span>
1. <span data-ttu-id="0f25f-217">A Microsoft Azure Backup szolgáltatás ügynökének hello kattintson **biztonsági mentés ütemezése**.</span><span class="sxs-lookup"><span data-stu-id="0f25f-217">In hello Microsoft Azure Backup agent click **Schedule Backup**.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/schedule-backup.png)
2. <span data-ttu-id="0f25f-219">A hello **ütemezett biztonsági mentés varázsló** hello hagyja **módosítása toobackup elemek vagy időpontok szerepelnek** jelölőnégyzetet és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="0f25f-219">In hello **Schedule Backup Wizard** leave hello **Make changes toobackup items or times** option selected and click **Next**.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/modify-or-stop-a-scheduled-backup.png)
3. <span data-ttu-id="0f25f-221">Ha szeretné, hogy tooadd, vagy módosítsa az elemeket, hello **elemek kijelölése tooBackup** képernyőn kattintson **elemek hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="0f25f-221">If you want tooadd or change items, on hello **Select Items tooBackup** screen click **Add Items**.</span></span>

    <span data-ttu-id="0f25f-222">Azt is beállíthatja **kizárások beállításai** hello varázslóban ezen a lapon.</span><span class="sxs-lookup"><span data-stu-id="0f25f-222">You can also set **Exclusion Settings** from this page in hello wizard.</span></span> <span data-ttu-id="0f25f-223">Ha azt szeretné, hogy tooexclude fájlok vagy fájltípusok olvasási hozzáadására szolgáló eljárást hello [kizárások beállításai](#manage-exclusion-settings).</span><span class="sxs-lookup"><span data-stu-id="0f25f-223">If you want tooexclude files or file types read hello procedure for adding [exclusion settings](#manage-exclusion-settings).</span></span>
4. <span data-ttu-id="0f25f-224">Válassza ki a hello fájlok és mappák tooback szeretné, hogy fel, és kattintson a **gépházban**.</span><span class="sxs-lookup"><span data-stu-id="0f25f-224">Select hello files and folders you want tooback up and click **Okay**.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/add-items-modify.png)
5. <span data-ttu-id="0f25f-226">Adja meg a hello **biztonsági mentés ütemezése** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="0f25f-226">Specify hello **backup schedule** and click **Next**.</span></span>

    <span data-ttu-id="0f25f-227">(A 3-szor naponkénti maximum) napi vagy heti biztonsági mentései is ütemezheti.</span><span class="sxs-lookup"><span data-stu-id="0f25f-227">You can schedule daily (at a maximum of 3 times per day) or weekly backups.</span></span>

    ![Windows Server biztonsági mentés elemei](./media/backup-azure-manage-windows-server/specify-backup-schedule-modify-close.png)

   > [!NOTE]
   > <span data-ttu-id="0f25f-229">Adja meg a hello biztonsági mentés ütemezése esetén, tekintse meg a részletes [cikk](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="0f25f-229">Specifying hello backup schedule is explained in detail in this [article](backup-azure-backup-cloud-as-tape.md).</span></span>
   >

6. <span data-ttu-id="0f25f-230">Jelölje be hello **adatmegőrzési** hello biztonsági másolatot, majd kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="0f25f-230">Select hello **Retention Policy** for hello backup copy and click **Next**.</span></span>

    ![Windows Server biztonsági mentés elemei](./media/backup-azure-manage-windows-server/select-retention-policy-modify.png)
7. <span data-ttu-id="0f25f-232">A hello **megerősítő** képernyőn hello információk áttekintése, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="0f25f-232">On hello **Confirmation** screen review hello information and click **Finish**.</span></span>
8. <span data-ttu-id="0f25f-233">Miután hello varázsló létrehozta a hello **biztonsági mentés ütemezése**, kattintson a **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="0f25f-233">Once hello wizard finishes creating hello **backup schedule**, click **Close**.</span></span>

    <span data-ttu-id="0f25f-234">Védelmi módosítása, után ellenőrizheti, hogy biztonsági mentések váltanak ki megfelelően fog toohello által **feladatok** lapra, és erősítse meg, hogy a módosítások megjelennek hello biztonsági mentési feladatok.</span><span class="sxs-lookup"><span data-stu-id="0f25f-234">After modifying protection, you can confirm that backups are triggering correctly by going toohello **Jobs** tab and confirming that changes are reflected in hello backup jobs.</span></span>

## <a name="enable-network-throttling"></a><span data-ttu-id="0f25f-235">Hálózati sávszélesség-szabályozás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="0f25f-235">Enable Network Throttling</span></span>

<span data-ttu-id="0f25f-236">hello Azure Backup szolgáltatás ügynökének a sávszélesség-szabályozási fülre, amely lehetővé teszi a hálózati sávszélesség használatának adatátvitel során toocontrol biztosít.</span><span class="sxs-lookup"><span data-stu-id="0f25f-236">hello Azure Backup agent provides a Throttling tab which allows you toocontrol how network bandwidth is used during data transfer.</span></span> <span data-ttu-id="0f25f-237">Ez a vezérlő akkor lehet hasznos, ha tooback adatokat kell munkaidőben, de nem hello biztonsági mentési folyamat toointerfere a más internetes forgalmat.</span><span class="sxs-lookup"><span data-stu-id="0f25f-237">This control can be helpful if you need tooback up data during work hours but do not want hello backup process toointerfere with other internet traffic.</span></span> <span data-ttu-id="0f25f-238">Sávszélesség-szabályozás adatok átvitel mentése tooback vonatkozik, és állítsa vissza a tevékenységek.</span><span class="sxs-lookup"><span data-stu-id="0f25f-238">Throttling of data transfer applies tooback up and restore activities.</span></span>  

<span data-ttu-id="0f25f-239">sávszélesség-szabályozás tooenable:</span><span class="sxs-lookup"><span data-stu-id="0f25f-239">tooenable throttling:</span></span>

1. <span data-ttu-id="0f25f-240">A hello **Backup szolgáltatás ügynökének**, kattintson a **tulajdonságainak módosítása**.</span><span class="sxs-lookup"><span data-stu-id="0f25f-240">In hello **Backup agent**, click **Change Properties**.</span></span>
2. <span data-ttu-id="0f25f-241">A hello ** szabályozás lapra, válassza ki **engedélyezi az internetes sávszélesség szabályozásának a biztonsági mentési műveleteknél**.</span><span class="sxs-lookup"><span data-stu-id="0f25f-241">On hello **Throttling tab, select **Enable internet bandwidth usage throttling for backup operations**.</span></span>

    ![Hálózati sávszélesség-szabályozás](./media/backup-azure-manage-windows-server/throttling-dialog.png)

    <span data-ttu-id="0f25f-243">Miután engedélyezte a sávszélesség-szabályozás, adja meg a biztonsági mentési adatátvitel során sávszélesség engedélyezett hello **időpontokat a munkaidőhöz** és **munkaidőn kívüli**.</span><span class="sxs-lookup"><span data-stu-id="0f25f-243">Once you have enabled throttling, specify hello allowed bandwidth for backup data transfer during **Work hours** and **Non-work hours**.</span></span>

    <span data-ttu-id="0f25f-244">hello sávszélesség értékek kezdődjenek 512 kilobájt / másodperc (Kbps), és lépjen be too1023 megabájt / másodperc (Mbps).</span><span class="sxs-lookup"><span data-stu-id="0f25f-244">hello bandwidth values begin at 512 kilobytes per second (Kbps) and can go up too1023 megabytes per second (Mbps).</span></span> <span data-ttu-id="0f25f-245">Is kijelölése hello kezdő és a Befejezés **időpontokat a munkaidőhöz**, és hello hét mely napján minősülnek munkahelyi nap.</span><span class="sxs-lookup"><span data-stu-id="0f25f-245">You can also designate hello start and finish for **Work hours**, and which days of hello week are considered Work days.</span></span> <span data-ttu-id="0f25f-246">hello kívül időpontokat a munkaidőhöz kijelölt hello ideje figyelembe vett toobe munkaidőn kívüli órákra.</span><span class="sxs-lookup"><span data-stu-id="0f25f-246">hello time outside of hello designated Work hours is considered toobe non-work hours.</span></span>
3. <span data-ttu-id="0f25f-247">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="0f25f-247">Click **OK**.</span></span>

## <a name="manage-exclusion-settings"></a><span data-ttu-id="0f25f-248">Kizárási beállításainak kezelése</span><span class="sxs-lookup"><span data-stu-id="0f25f-248">Manage exclusion settings</span></span>
1. <span data-ttu-id="0f25f-249">Nyissa meg hello **Microsoft Azure Backup szolgáltatás ügynökének** (a gép kereséssel megtalálja *a Microsoft Azure Backup szolgáltatás*).</span><span class="sxs-lookup"><span data-stu-id="0f25f-249">Open hello **Microsoft Azure Backup agent** (you can find it by searching your machine for *Microsoft Azure Backup*).</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/snap-in-search.png)
2. <span data-ttu-id="0f25f-251">A Microsoft Azure Backup szolgáltatás ügynökének hello kattintson **biztonsági mentés ütemezése**.</span><span class="sxs-lookup"><span data-stu-id="0f25f-251">In hello Microsoft Azure Backup agent click **Schedule Backup**.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/schedule-backup.png)
3. <span data-ttu-id="0f25f-253">Az ütemezett biztonsági mentés varázsló hello hagyja hello **módosítása toobackup elemek vagy időpontok szerepelnek** jelölőnégyzetet és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="0f25f-253">In hello Schedule Backup Wizard leave hello **Make changes toobackup items or times** option selected and click **Next**.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/modify-or-stop-a-scheduled-backup.png)
4. <span data-ttu-id="0f25f-255">Kattintson a **kizárások beállításai**.</span><span class="sxs-lookup"><span data-stu-id="0f25f-255">Click **Exclusions Settings**.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/exclusion-settings.png)
5. <span data-ttu-id="0f25f-257">Kattintson a **adja hozzá a kizárási**.</span><span class="sxs-lookup"><span data-stu-id="0f25f-257">Click **Add Exclusion**.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/add-exclusion.png)
6. <span data-ttu-id="0f25f-259">Adja meg hello helyét, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="0f25f-259">Select hello location and then, click **OK**.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/exclusion-location.png)
7. <span data-ttu-id="0f25f-261">Hello fájlkiterjesztés hozzáadása a hello **Fájltípus** mező.</span><span class="sxs-lookup"><span data-stu-id="0f25f-261">Add hello file extension in hello **File Type** field.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/exclude-file-type.png)

    <span data-ttu-id="0f25f-263">.Mp3 bővítmény felvétele</span><span class="sxs-lookup"><span data-stu-id="0f25f-263">Adding an .mp3 extension</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/exclude-mp3.png)

    <span data-ttu-id="0f25f-265">tooadd egy másik kiterjesztést, kattintson a **Kizárás hozzáadása** , és adja meg egy másik típus fájlnévkiterjesztés (.jpeg-bővítmény hozzáadása).</span><span class="sxs-lookup"><span data-stu-id="0f25f-265">tooadd another extension, click **Add Exclusion** and enter another file type extension (adding a .jpeg extension).</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/exclude-jpg.png)
8. <span data-ttu-id="0f25f-267">Ha az összes hello bővítmény felvett, kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="0f25f-267">When you've added all hello extensions, click **OK**.</span></span>
9. <span data-ttu-id="0f25f-268">Ütemezett biztonsági mentés varázsló hello keresztül kattintva továbbra is **következő** amíg hello **visszaigazolási lapja**, kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="0f25f-268">Continue through hello Schedule Backup Wizard by clicking **Next** until hello **Confirmation page**, then click **Finish**.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/finish-exclusions.png)

## <a name="frequently-asked-questions"></a><span data-ttu-id="0f25f-270">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="0f25f-270">Frequently asked questions</span></span>
<span data-ttu-id="0f25f-271">**1. KÉRDÉS. hello biztonsági mentési feladat állapotüzenet látható módon fejezte be hello Azure Backup szolgáltatás ügynöke, miért nem az beszerzése kerülnek azonnal portálon?**</span><span class="sxs-lookup"><span data-stu-id="0f25f-271">**Q1. hello backup job status shows as completed in hello Azure backup agent, why doesn't it get reflected immediately in portal?**</span></span>

<span data-ttu-id="0f25f-272">1. válasz</span><span class="sxs-lookup"><span data-stu-id="0f25f-272">A1.</span></span> <span data-ttu-id="0f25f-273">Nincs maximális késleltetés 15 perc közötti hello biztonsági mentési feladat állapotát a következő tükrözi hello Azure Backup szolgáltatás ügynöke és hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="0f25f-273">There is at maximum delay of 15 mins between hello backup job status reflected in hello Azure backup agent and hello Azure portal.</span></span>

<span data-ttu-id="0f25f-274">**Q.2, amikor egy biztonsági mentési feladat sikertelen lesz, hogy mennyi ideig tart tooraise riasztást?**</span><span class="sxs-lookup"><span data-stu-id="0f25f-274">**Q.2 When a backup job fails, how long does it take tooraise an alert?**</span></span>

<span data-ttu-id="0f25f-275">Riasztás A.2 hello Azure sikertelen biztonsági mentéshez, 20 perc belül következik be.</span><span class="sxs-lookup"><span data-stu-id="0f25f-275">A.2 An alert is raised within 20 mins of hello Azure backup failure.</span></span>

<span data-ttu-id="0f25f-276">**3. NEGYEDÉVÉBEN. Ha egy e-mailt nem küldi el, ha értesítések beállítása eset van?**</span><span class="sxs-lookup"><span data-stu-id="0f25f-276">**Q3. Is there a case where an email won’t be sent if notifications are configured?**</span></span>

<span data-ttu-id="0f25f-277">3. válasz</span><span class="sxs-lookup"><span data-stu-id="0f25f-277">A3.</span></span> <span data-ttu-id="0f25f-278">Amikor hello értesítés nem lesz elküldve a rendelés tooreduce hello riasztási zaj az alábbiakban hello esetekben:</span><span class="sxs-lookup"><span data-stu-id="0f25f-278">Below are hello cases when hello notification will not be sent in order tooreduce hello alert noise:</span></span>

* <span data-ttu-id="0f25f-279">Ha az értesítések beállítása óránkénti, és az következik be van-e és hello órán belül megoldott riasztás</span><span class="sxs-lookup"><span data-stu-id="0f25f-279">If notifications are configured hourly and an alert is raised and resolved within hello hour</span></span>
* <span data-ttu-id="0f25f-280">Feladat megszakadt.</span><span class="sxs-lookup"><span data-stu-id="0f25f-280">Job is canceled.</span></span>
* <span data-ttu-id="0f25f-281">Második biztonsági mentési feladat sikertelen volt, mert az eredeti biztonsági mentési feladat van folyamatban.</span><span class="sxs-lookup"><span data-stu-id="0f25f-281">Second backup job failed because original backup job is in progress.</span></span>

## <a name="troubleshooting-monitoring-issues"></a><span data-ttu-id="0f25f-282">Figyelési problémák elhárítása</span><span class="sxs-lookup"><span data-stu-id="0f25f-282">Troubleshooting Monitoring Issues</span></span>
<span data-ttu-id="0f25f-283">**Probléma:** feladatok és/vagy hello Azure Backup szolgáltatás ügynökének származó riasztások nem jelennek meg hello portálon.</span><span class="sxs-lookup"><span data-stu-id="0f25f-283">**Issue:** Jobs and/or alerts from hello Azure Backup agent do not appear in hello portal.</span></span>

<span data-ttu-id="0f25f-284">**Hibaelhárítási lépések:** folyamat hello ```OBRecoveryServicesManagementAgent```, küld hello feladat és riasztás adatok toohello Azure Backup szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="0f25f-284">**Troubleshooting steps:** hello process, ```OBRecoveryServicesManagementAgent```, sends hello job and alert data toohello Azure Backup service.</span></span> <span data-ttu-id="0f25f-285">Ez a folyamat állapottal alkalmanként vagy -leállítás.</span><span class="sxs-lookup"><span data-stu-id="0f25f-285">Occasionally this process can become stuck or shutdown.</span></span>

1. <span data-ttu-id="0f25f-286">tooverify hello folyamat nem fut, nyissa meg a **Feladatkezelő** , és ellenőrizze, hogy hello ```OBRecoveryServicesManagementAgent``` folyamat fut.</span><span class="sxs-lookup"><span data-stu-id="0f25f-286">tooverify hello process is not running, open **Task Manager** and check if hello ```OBRecoveryServicesManagementAgent``` process is running.</span></span>
2. <span data-ttu-id="0f25f-287">Feltételezve, hogy hello folyamat nem fut, nyissa meg a **Vezérlőpult** , és keresse meg a szolgáltatás hello listája.</span><span class="sxs-lookup"><span data-stu-id="0f25f-287">Assuming that hello process is not running, open **Control Panel** and browse hello list of services.</span></span> <span data-ttu-id="0f25f-288">Indítsa el, vagy indítsa újra a **Microsoft Azure Recovery Services Management Agent**.</span><span class="sxs-lookup"><span data-stu-id="0f25f-288">Start or restart **Microsoft Azure Recovery Services Management Agent**.</span></span>

    <span data-ttu-id="0f25f-289">További információért keresse meg a hello naplózásra kerül:</span><span class="sxs-lookup"><span data-stu-id="0f25f-289">For further information, browse hello logs at:</span></span><br/><span data-ttu-id="0f25f-290">
   `<AzureBackup_agent_install_folder>\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider*`Példa:</span><span class="sxs-lookup"><span data-stu-id="0f25f-290">
   `<AzureBackup_agent_install_folder>\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider*` For example:</span></span><br/>
   `C:\Program Files\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider0.errlog`

## <a name="next-steps"></a><span data-ttu-id="0f25f-291">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0f25f-291">Next steps</span></span>
* [<span data-ttu-id="0f25f-292">Windows Server vagy a Windows ügyfél visszaállítása az Azure-ból</span><span class="sxs-lookup"><span data-stu-id="0f25f-292">Restore Windows Server or Windows Client from Azure</span></span>](backup-azure-restore-windows-server.md)
* <span data-ttu-id="0f25f-293">További információ az Azure Backup szolgáltatásban toolearn lásd [Azure biztonsági mentés áttekintése](backup-introduction-to-azure-backup.md)</span><span class="sxs-lookup"><span data-stu-id="0f25f-293">toolearn more about Azure Backup, see [Azure Backup Overview](backup-introduction-to-azure-backup.md)</span></span>
* <span data-ttu-id="0f25f-294">A Microsoft hello [Azure biztonsági mentés fórum](http://go.microsoft.com/fwlink/p/?LinkId=290933)</span><span class="sxs-lookup"><span data-stu-id="0f25f-294">Visit hello [Azure Backup Forum](http://go.microsoft.com/fwlink/p/?LinkId=290933)</span></span>
