---
title: "Az Azure recovery services-tárolók és a kiszolgálók felügyeletére |} Microsoft Docs"
description: "Ez az oktatóanyag segítségével megtudhatja, hogyan az Azure recovery services-tárolók és a kiszolgálók kezelésére."
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
ms.openlocfilehash: 5922e308f5c205a07bd329c28322ae82cea0e1fa
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-and-manage-azure-recovery-services-vaults-and-servers-for-windows-machines"></a><span data-ttu-id="e154e-103">Windows-gépek Azure Recovery Services-tárolóinak és -kiszolgálóinak figyelése és kezelése</span><span class="sxs-lookup"><span data-stu-id="e154e-103">Monitor and manage Azure recovery services vaults and servers for Windows machines</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e154e-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="e154e-104">Resource Manager</span></span>](backup-azure-manage-windows-server.md)
> * [<span data-ttu-id="e154e-105">Klasszikus</span><span class="sxs-lookup"><span data-stu-id="e154e-105">Classic</span></span>](backup-azure-manage-windows-server-classic.md)
>
>

<span data-ttu-id="e154e-106">Ebben a cikkben az Azure-portál és a Microsoft Azure Backup szolgáltatás ügynöke elérhető biztonsági mentési figyelő és a felügyeleti feladatok áttekintést találhat.</span><span class="sxs-lookup"><span data-stu-id="e154e-106">In this article you'll find an overview of the backup monitor and management tasks available through the Azure portal and the Microsoft Azure Backup agent.</span></span> <span data-ttu-id="e154e-107">Ez a cikk azt feltételezi, hogy már rendelkezik Azure-előfizetéssel, és legalább egy Recovery Services-tároló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e154e-107">This article assumes you already have an Azure subscription and have created at least one Recovery Services vault.</span></span>

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]


## <a name="open-a-recovery-services-vault"></a><span data-ttu-id="e154e-108">Nyissa meg a Recovery Services-tároló</span><span class="sxs-lookup"><span data-stu-id="e154e-108">Open a Recovery Services vault</span></span>

<span data-ttu-id="e154e-109">A Recovery Services-tároló irányítópult látható a részleteit, illetve a Recovery Services-tároló attribútumait.</span><span class="sxs-lookup"><span data-stu-id="e154e-109">The Recovery Services vault dashboard shows you the details or attributes of a Recovery Services vault.</span></span>

1. <span data-ttu-id="e154e-110">Jelentkezzen be a [Azure Portal](https://portal.azure.com/) használata az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="e154e-110">Sign in to the [Azure Portal](https://portal.azure.com/) using your Azure subscription.</span></span>
2. <span data-ttu-id="e154e-111">A központ menüben kattintson a **több szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="e154e-111">On the Hub menu, click **More Services**.</span></span>

    ![Recovery Services-tárolók 1.lépés listájának megnyitása](./media/backup-azure-manage-windows-server/open-rs-vault-list.png) <br/>

3. <span data-ttu-id="e154e-113">Nyissa meg a Recovery Services-tároló szeretné.</span><span class="sxs-lookup"><span data-stu-id="e154e-113">You want to open a Recovery Services vault.</span></span> <span data-ttu-id="e154e-114">A párbeszédpanelen, kezdje el begépelni **Recovery Services**.</span><span class="sxs-lookup"><span data-stu-id="e154e-114">In the dialog box, start typing **Recovery Services**.</span></span> <span data-ttu-id="e154e-115">Ahogy elkezd gépelni, a lista a beírtak alapján szűri a lehetőségeket.</span><span class="sxs-lookup"><span data-stu-id="e154e-115">As you begin typing, the list will filter based on your input.</span></span> <span data-ttu-id="e154e-116">Kattintson a **Recovery Services-tárolók** megjelenítse a Recovery Services-tárolók az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="e154e-116">Click **Recovery Services vaults** to display the list of Recovery Services vaults in your subscription.</span></span>

    ![Recovery Services-tároló létrehozása – 1. lépés](./media/backup-azure-manage-windows-server/browse-to-rs-vaults-2.png) <br/>

    <span data-ttu-id="e154e-118">A Recovery Services-tárolók listáját nyitja meg.</span><span class="sxs-lookup"><span data-stu-id="e154e-118">The list of Recovery Services vaults opens.</span></span>

    ![Recovery Services-tároló létrehozása – 1. lépés](./media/backup-azure-manage-windows-server/list-of-rs-vaults.png) <br/>

4. <span data-ttu-id="e154e-120">Tárolók a listából válassza ki a megnyitni kívánt Recovery Services-tároló nevét.</span><span class="sxs-lookup"><span data-stu-id="e154e-120">From the list of vaults, select the name of the Recovery Services vault you want to open.</span></span> <span data-ttu-id="e154e-121">Ekkor megnyílik a Recovery Services-tároló irányítópult panel.</span><span class="sxs-lookup"><span data-stu-id="e154e-121">The Recovery Services vault dashboard blade opens.</span></span>

    ![helyreállítási szolgáltatások tároló irányítópult](./media/backup-azure-manage-windows-server/rs-vault-blade.png) <br/>

    <span data-ttu-id="e154e-123">Most, hogy a Recovery Services-tároló nyitotta meg, próbálkozzon a figyelést, vagy a felügyeleti feladatokat a.</span><span class="sxs-lookup"><span data-stu-id="e154e-123">Now that you have opened the Recovery Services vault, try any of the monitoring or management tasks.</span></span>

## <a name="monitor-backup-jobs-and-alerts"></a><span data-ttu-id="e154e-124">A figyelő biztonsági mentési feladatok és riasztások</span><span class="sxs-lookup"><span data-stu-id="e154e-124">Monitor backup jobs and alerts</span></span>

<span data-ttu-id="e154e-125">Figyelheti a feladatok és riasztások az irányítópultról Recovery Services tárolóban, ahol láthatja:</span><span class="sxs-lookup"><span data-stu-id="e154e-125">You monitor jobs and alerts from the Recovery Services vault dashboard, where you see:</span></span>

* <span data-ttu-id="e154e-126">Biztonsági mentési riasztás részletei</span><span class="sxs-lookup"><span data-stu-id="e154e-126">Backup alerts details</span></span>
* <span data-ttu-id="e154e-127">Fájlok és mappák, valamint az Azure virtuális gép a felhőben védelme</span><span class="sxs-lookup"><span data-stu-id="e154e-127">Files and folders, as well as Azure virtual machines protected in the cloud</span></span>
* <span data-ttu-id="e154e-128">Teljes Azure-ban felhasznált tárterület</span><span class="sxs-lookup"><span data-stu-id="e154e-128">Total storage consumed in Azure</span></span>
* <span data-ttu-id="e154e-129">Biztonsági mentési feladat állapota</span><span class="sxs-lookup"><span data-stu-id="e154e-129">Backup job status</span></span>

![Biztonsági mentési irányítópult feladatok](./media/backup-azure-manage-windows-server/dashboard-tiles.png)

<span data-ttu-id="e154e-131">Minden egyes ezen csempék az információk kattintva megnyílik a társított panel, ahol a kapcsolódó feladatok kezelése.</span><span class="sxs-lookup"><span data-stu-id="e154e-131">Clicking the information in each of these tiles will open the associated blade where you manage related tasks.</span></span>

<span data-ttu-id="e154e-132">Az irányítópult tetején:</span><span class="sxs-lookup"><span data-stu-id="e154e-132">From the top of the Dashboard:</span></span>

* <span data-ttu-id="e154e-133">Beállítások hozzáférést biztosít az elérhető biztonsági mentési feladatokat.</span><span class="sxs-lookup"><span data-stu-id="e154e-133">Settings provides access available backup tasks.</span></span>
* <span data-ttu-id="e154e-134">A biztonsági mentés - biztonsági másolatot készíteni az új fájlok és mappák (vagy Azure virtuális gépek) számára a Recovery Services-tároló segítségével.</span><span class="sxs-lookup"><span data-stu-id="e154e-134">Backup - helps you back up new files and folders (or Azure VMs) to the Recovery Services vault.</span></span>
* <span data-ttu-id="e154e-135">Törlés – a recovery services-tároló már nem használatos, ha törlése tárhely felszabadítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="e154e-135">Delete - If a recovery services vault is no longer being used, you can delete it to free up storage space.</span></span> <span data-ttu-id="e154e-136">Törlés csak akkor engedélyezett, miután az összes védett kiszolgálón, hogy törölték a tárolóból.</span><span class="sxs-lookup"><span data-stu-id="e154e-136">Delete is only enabled after all protected servers have been deleted from the vault.</span></span>

![Biztonsági mentési irányítópult feladatok](./media/backup-azure-manage-windows-server/dashboard-tasks.png)

## <a name="alerts-for-backups-using-azure-backup-agent"></a><span data-ttu-id="e154e-138">A biztonsági mentéseket az Azure Backup szolgáltatás ügynöke riasztások:</span><span class="sxs-lookup"><span data-stu-id="e154e-138">Alerts for backups using Azure backup agent:</span></span>
| <span data-ttu-id="e154e-139">Riasztási szint</span><span class="sxs-lookup"><span data-stu-id="e154e-139">Alert Level</span></span> | <span data-ttu-id="e154e-140">Értesítések küldése</span><span class="sxs-lookup"><span data-stu-id="e154e-140">Alerts sent</span></span> |
| --- | --- |
| <span data-ttu-id="e154e-141">Kritikus</span><span class="sxs-lookup"><span data-stu-id="e154e-141">Critical</span></span> |<span data-ttu-id="e154e-142">Sikertelen biztonsági mentéshez, helyreállítási hiba</span><span class="sxs-lookup"><span data-stu-id="e154e-142">Backup failure, recovery failure</span></span> |
| <span data-ttu-id="e154e-143">Figyelmeztetés</span><span class="sxs-lookup"><span data-stu-id="e154e-143">Warning</span></span> |<span data-ttu-id="e154e-144">A biztonsági mentés befejeződött, de figyelmeztetéseket generált (Ha kevesebb mint száz fájlok nem készül biztonsági mentés okozta problémák miatt, és több mint egymillió fájlok sikeresen biztonsági mentése)</span><span class="sxs-lookup"><span data-stu-id="e154e-144">Backup completed with warnings (when fewer than one hundred files are not backed up due to corruption issues, and more than one million files are successfully backed up)</span></span> |
| <span data-ttu-id="e154e-145">Tájékoztató</span><span class="sxs-lookup"><span data-stu-id="e154e-145">Informational</span></span> |<span data-ttu-id="e154e-146">None</span><span class="sxs-lookup"><span data-stu-id="e154e-146">None</span></span> |

## <a name="manage-backup-alerts"></a><span data-ttu-id="e154e-147">Biztonsági riasztások kezelése</span><span class="sxs-lookup"><span data-stu-id="e154e-147">Manage Backup alerts</span></span>
<span data-ttu-id="e154e-148">Kattintson a **biztonsági mentési riasztások** csempére kattintva nyissa meg a **biztonsági mentési riasztások** panel és kezelheti a riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="e154e-148">Click the **Backup Alerts** tile to open the **Backup Alerts** blade and manage alerts.</span></span>

![Biztonsági mentési riasztás](./media/backup-azure-manage-windows-server/manage-backup-alerts.png)

<span data-ttu-id="e154e-150">A biztonsági riasztások csempén a számát mutatja:</span><span class="sxs-lookup"><span data-stu-id="e154e-150">The Backup Alerts tile shows you the number of:</span></span>

* <span data-ttu-id="e154e-151">kritikus riasztás feloldva a legutóbbi 24 órában</span><span class="sxs-lookup"><span data-stu-id="e154e-151">critical alerts unresolved in last 24 hours</span></span>
* <span data-ttu-id="e154e-152">az elmúlt 24 órában feloldatlan figyelmeztető riasztások</span><span class="sxs-lookup"><span data-stu-id="e154e-152">warning alerts unresolved in last 24 hours</span></span>

<span data-ttu-id="e154e-153">Kattintson a valamennyi kapcsolatot tart, hogy a **biztonsági mentési riasztások** szűrt láthassák ezeket a riasztásokat (kritikus vagy figyelmeztetési) panelen.</span><span class="sxs-lookup"><span data-stu-id="e154e-153">Clicking on each of these links takes you to the **Backup Alerts** blade with a filtered view of these alerts (critical or warning).</span></span>

<span data-ttu-id="e154e-154">A biztonsági mentési riasztások paneljéről meg:</span><span class="sxs-lookup"><span data-stu-id="e154e-154">From the Backup Alerts blade, you:</span></span>

* <span data-ttu-id="e154e-155">Válassza ki a megfelelő információkkal együtt a riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="e154e-155">Choose the appropriate information to include with your alerts.</span></span>

    ![Oszlopok kiválasztása](./media/backup-azure-manage-windows-server/choose-alerts-colunms.png)
* <span data-ttu-id="e154e-157">Riasztások szűrése a súlyosság, állapota és a kezdő és záró időpont.</span><span class="sxs-lookup"><span data-stu-id="e154e-157">Filter alerts for severity, status and start/end times.</span></span>

    ![Riasztások szűrése](./media/backup-azure-manage-windows-server/filter-alerts.png)
* <span data-ttu-id="e154e-159">Súlyosság, a gyakoriság és a címzettek értesítések konfigurálása, valamint figyelmeztetések engedélyezése vagy letiltása.</span><span class="sxs-lookup"><span data-stu-id="e154e-159">Configure notifications for severity, frequency and recipients, as well as turn alerts on or off.</span></span>

    ![Riasztások szűrése](./media/backup-azure-manage-windows-server/configure-notifications.png)

<span data-ttu-id="e154e-161">Ha **/ riasztási** választotta a **értesítendő** gyakoriság nincs csoportosítás vagy e-mailek csökkenése történik.</span><span class="sxs-lookup"><span data-stu-id="e154e-161">If **Per Alert** is selected as the **Notify** frequency no grouping or reduction in emails occurs.</span></span> <span data-ttu-id="e154e-162">Minden egyes riasztás 1 értesítési eredményez.</span><span class="sxs-lookup"><span data-stu-id="e154e-162">Every alert results in 1 notification.</span></span> <span data-ttu-id="e154e-163">Ez az alapértelmezett beállítás, és a feloldási e-mailt is küld azonnal.</span><span class="sxs-lookup"><span data-stu-id="e154e-163">This is the default setting and the resolution email is also sent out immediately.</span></span>

<span data-ttu-id="e154e-164">Ha **óránkénti kivonatoló** választotta a **értesítendő** egy e-mailt küld a felhasználó közölve, hogy van a gyakoriság feloldatlan új riasztások jönnek létre az elmúlt órában.</span><span class="sxs-lookup"><span data-stu-id="e154e-164">If **Hourly Digest** is selected as the **Notify** frequency one email is sent to the user telling them that there are unresolved new alerts generated in the last hour.</span></span> <span data-ttu-id="e154e-165">A feloldási e-mailt küld az órát végén.</span><span class="sxs-lookup"><span data-stu-id="e154e-165">A resolution email is sent out at the end of the hour.</span></span>

<span data-ttu-id="e154e-166">Is elküldi a riasztásokat a következő súlyossági szintek:</span><span class="sxs-lookup"><span data-stu-id="e154e-166">Alerts can be sent for the following severity levels:</span></span>

* <span data-ttu-id="e154e-167">Kritikus</span><span class="sxs-lookup"><span data-stu-id="e154e-167">critical</span></span>
* <span data-ttu-id="e154e-168">Figyelmeztetés</span><span class="sxs-lookup"><span data-stu-id="e154e-168">warning</span></span>
* <span data-ttu-id="e154e-169">Információ</span><span class="sxs-lookup"><span data-stu-id="e154e-169">information</span></span>

<span data-ttu-id="e154e-170">A riasztás inaktiválja a **inaktiválása** gombot a projekt részleteit megjelenítő panelen.</span><span class="sxs-lookup"><span data-stu-id="e154e-170">You inactivate the alert with the **inactivate** button in the job details blade.</span></span> <span data-ttu-id="e154e-171">Kattintva inaktiválása, megadhatja a feloldási megjegyzések.</span><span class="sxs-lookup"><span data-stu-id="e154e-171">When you click inactivate, you can provide resolution notes.</span></span>

<span data-ttu-id="e154e-172">Úgy dönt, hogy az oszlopok, akkor jelenik meg a riasztás részeként a **oszlopok kiválasztása** gombra.</span><span class="sxs-lookup"><span data-stu-id="e154e-172">You choose the columns you want to appear as part of the alert with the **Choose columns** button.</span></span>

> [!NOTE]
> <span data-ttu-id="e154e-173">Az a **beállítások** panelen kezelheti a biztonsági mentési riasztás kiválasztásával **figyelés és jelentéskészítés > riasztások és események > biztonsági mentésekkel kapcsolatos riasztások** , majd kattintson **szűrő** vagy  **Értesítések konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="e154e-173">From the **Settings** blade, you manage backup alerts by selecting **Monitoring and Reports > Alerts and Events > Backup Alerts** and then clicking **Filter** or **Configure Notifications**.</span></span>
>
>

## <a name="manage-backup-items"></a><span data-ttu-id="e154e-174">Biztonsági mentés elemek kezelése</span><span class="sxs-lookup"><span data-stu-id="e154e-174">Manage Backup items</span></span>
<span data-ttu-id="e154e-175">A helyi biztonsági kezelése már elérhető a felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="e154e-175">Managing on-premises backups is now available in the management portal.</span></span> <span data-ttu-id="e154e-176">A biztonsági mentés szakaszában az irányítópulton a **biztonsági másolati elemei** csempe a tárolóba védett biztonsági másolati elemei számát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="e154e-176">In the Backup section of the dashboard, the **Backup Items** tile shows the number of backup items protected to the vault.</span></span>

<span data-ttu-id="e154e-177">Kattintson a **-mappákban** a biztonsági mentés elemeknél csempére.</span><span class="sxs-lookup"><span data-stu-id="e154e-177">Click **File-Folders** in the Backup Items tile.</span></span>

![Biztonsági másolati elemei csempe](./media/backup-azure-manage-windows-server/backup-items-tile.png)

<span data-ttu-id="e154e-179">A biztonsági másolati elemei panel nyílik meg a szűrő beállítása fájlok és mappák, ahol minden egyes konkrét biztonsági másolat felsorolt elem látható.</span><span class="sxs-lookup"><span data-stu-id="e154e-179">The Backup Items blade opens with the filter set to File-Folder where you see each specific backup item listed.</span></span>

![Biztonsági másolati elemei](./media/backup-azure-manage-windows-server/backup-item-list.png)

<span data-ttu-id="e154e-181">Ha egy adott biztonsági mentési elemet a listáról válassza ki, alapvető részletes adatainak megadása, hogy az elem látható.</span><span class="sxs-lookup"><span data-stu-id="e154e-181">If you select a specific backup item from the list, you see the essential details for that item.</span></span>

> [!NOTE]
> <span data-ttu-id="e154e-182">Az a **beállítások** panelen, kezelheti a fájlok és mappák kiválasztásával **védett elemek > biztonsági mentés elemek** és jelölje be **-mappákban** a legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="e154e-182">From the **Settings** blade, you manage files and folders by selecting **Protected Items > Backup Items** and then selecting **File-Folders** from the drop down menu.</span></span>
>
>

![A beállítások biztonsági másolati elemei](./media/backup-azure-manage-windows-server/backup-files-and-folders.png)

## <a name="manage-backup-jobs"></a><span data-ttu-id="e154e-184">Biztonsági mentési feladatok kezelése</span><span class="sxs-lookup"><span data-stu-id="e154e-184">Manage Backup jobs</span></span>
<span data-ttu-id="e154e-185">Biztonsági mentés mind a helyszíni feladatokat, (Ha a helyszíni kiszolgáló biztonsági mentése az Azure-bA), és az Azure biztonsági mentések láthatók az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="e154e-185">Backup jobs for both on-premises (when the on-premises server is backing up to Azure) and Azure backups are visible in the dashboard.</span></span>

<span data-ttu-id="e154e-186">Az irányítópult a biztonsági mentés területen a biztonsági mentési feladat csempe feladatok számát jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="e154e-186">In the Backup section of the dashboard, the Backup job tile shows the number of jobs:</span></span>

* <span data-ttu-id="e154e-187">folyamatban van</span><span class="sxs-lookup"><span data-stu-id="e154e-187">in progress</span></span>
* <span data-ttu-id="e154e-188">nem sikerült az elmúlt 24 órában.</span><span class="sxs-lookup"><span data-stu-id="e154e-188">failed in the last 24 hours.</span></span>

<span data-ttu-id="e154e-189">A biztonsági mentési feladatok kezeléséhez kattintson a **biztonsági mentési feladatok** csempe, amely a biztonsági mentési feladatok paneljének megnyitása.</span><span class="sxs-lookup"><span data-stu-id="e154e-189">To manage your backup jobs, click the **Backup Jobs** tile, which opens the Backup Jobs blade.</span></span>

![A beállítások biztonsági másolati elemei](./media/backup-azure-manage-windows-server/backup-jobs.png)

<span data-ttu-id="e154e-191">A biztonsági mentési feladatok panelről a rendelkezésre álló információk módosítása a **oszlopok kiválasztása** gombra az oldal tetején.</span><span class="sxs-lookup"><span data-stu-id="e154e-191">You modify the information available in the Backup Jobs blade with the **Choose columns** button at the top of the page.</span></span>

<span data-ttu-id="e154e-192">Használja a **szűrő** gombra, majd a fájlok és mappák és az Azure virtuális gép biztonsági mentése között.</span><span class="sxs-lookup"><span data-stu-id="e154e-192">Use the **Filter** button to select between Files and folders and Azure virtual machine backup.</span></span>

<span data-ttu-id="e154e-193">Ha nem látja a biztonsági mentés fájlokat és mappákat, kattintson a **szűrő** gombbal és válassza ki **fájlok és mappák** a elemtípus menüből.</span><span class="sxs-lookup"><span data-stu-id="e154e-193">If you don't see your backed up files and folders, click **Filter** button at the top of the page and select **Files and folders** from the Item Type menu.</span></span>

> [!NOTE]
> <span data-ttu-id="e154e-194">Az a **beállítások** panelen kiválasztásával kezelheti a biztonsági mentési feladatok **figyelés és jelentéskészítés > feladatok > biztonsági mentési feladatok** és jelölje be **-mappákban** a legördülő menüben menü.</span><span class="sxs-lookup"><span data-stu-id="e154e-194">From the **Settings** blade, you manage backup jobs by selecting **Monitoring and Reports > Jobs > Backup Jobs** and then selecting **File-Folders** from the drop down menu.</span></span>
>
>

## <a name="monitor-backup-usage"></a><span data-ttu-id="e154e-195">Biztonsági másolat használatának figyelése</span><span class="sxs-lookup"><span data-stu-id="e154e-195">Monitor Backup usage</span></span>
<span data-ttu-id="e154e-196">Az irányítópult a biztonsági mentés területen a biztonsági mentés használata csempe az Azure-ban felhasznált tárhely jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="e154e-196">In the Backup section of the dashboard, the Backup Usage tile shows the storage consumed in Azure.</span></span> <span data-ttu-id="e154e-197">Tárhelyhasználatot biztosított:</span><span class="sxs-lookup"><span data-stu-id="e154e-197">Storage usage is provided for:</span></span>

* <span data-ttu-id="e154e-198">A tárolóhoz rendelt felhőalapú LRS-tároló használata</span><span class="sxs-lookup"><span data-stu-id="e154e-198">Cloud LRS storage usage associated with the vault</span></span>
* <span data-ttu-id="e154e-199">A felhőalapú Georedundáns tárolás használata a tárolóhoz rendelt</span><span class="sxs-lookup"><span data-stu-id="e154e-199">Cloud GRS storage usage associated with the vault</span></span>

## <a name="manage-your-production-servers"></a><span data-ttu-id="e154e-200">Az üzemi kiszolgálók kezelése</span><span class="sxs-lookup"><span data-stu-id="e154e-200">Manage your production servers</span></span>
<span data-ttu-id="e154e-201">Az üzemi kiszolgálók kezeléséhez kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="e154e-201">To manage your production servers, click **Settings**.</span></span>

<span data-ttu-id="e154e-202">Kattintson a kezelés **biztonsági infrastruktúra > az üzemi kiszolgálók**.</span><span class="sxs-lookup"><span data-stu-id="e154e-202">Under Manage click **Backup infrastructure > Production Servers**.</span></span>

<span data-ttu-id="e154e-203">A rendelkezésre álló üzemi kiszolgálók az üzemi kiszolgálók panel listáját.</span><span class="sxs-lookup"><span data-stu-id="e154e-203">The Production Servers blade lists of all your available production servers.</span></span> <span data-ttu-id="e154e-204">Kattintson egy kiszolgálón nyissa meg a részletek a listában.</span><span class="sxs-lookup"><span data-stu-id="e154e-204">Click on a server in the list to open the server details.</span></span>

![Védett elemek](./media/backup-azure-manage-windows-server/production-server-list.png)


## <a name="open-the-azure-backup-agent"></a><span data-ttu-id="e154e-206">Nyissa meg az Azure Backup szolgáltatás ügynöke</span><span class="sxs-lookup"><span data-stu-id="e154e-206">Open the Azure Backup agent</span></span>
<span data-ttu-id="e154e-207">Nyissa meg a **Microsoft Azure Backup szolgáltatás ügynökének** (rákeresve a gépen található *a Microsoft Azure Backup szolgáltatás*).</span><span class="sxs-lookup"><span data-stu-id="e154e-207">Open the **Microsoft Azure Backup agent** (you find it by searching your machine for *Microsoft Azure Backup*).</span></span>

![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/snap-in-search.png)

<span data-ttu-id="e154e-209">Az a **műveletek** érhető el a biztonságimásolat-készítő ügynök konzol jobb akkor hajtsa végre az alábbi felügyeleti feladatokat:</span><span class="sxs-lookup"><span data-stu-id="e154e-209">From the **Actions** available at the right of the backup agent console you perform the following management tasks:</span></span>

* <span data-ttu-id="e154e-210">Kiszolgáló regisztrálása</span><span class="sxs-lookup"><span data-stu-id="e154e-210">Register Server</span></span>
* <span data-ttu-id="e154e-211">Biztonsági mentés ütemezése</span><span class="sxs-lookup"><span data-stu-id="e154e-211">Schedule Backup</span></span>
* <span data-ttu-id="e154e-212">Biztonsági másolat készítése</span><span class="sxs-lookup"><span data-stu-id="e154e-212">Back Up now</span></span>
* <span data-ttu-id="e154e-213">Tulajdonságainak módosítása</span><span class="sxs-lookup"><span data-stu-id="e154e-213">Change Properties</span></span>

![A Microsoft Azure Backup szolgáltatás ügynöke konzol műveletek](./media/backup-azure-manage-windows-server/console-actions.png)

> [!NOTE]
> <span data-ttu-id="e154e-215">A **adatok helyreállítása**, lásd: [fájlokat állíthatja vissza a Windows server vagy a Windows-ügyfélszámítógép](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="e154e-215">To **Recover Data**, see [Restore files to a Windows server or Windows client machine](backup-azure-restore-windows-server.md).</span></span>
>
>

## <a name="modify-the-backup-schedule"></a><span data-ttu-id="e154e-216">A biztonsági mentési ütemezés módosítása</span><span class="sxs-lookup"><span data-stu-id="e154e-216">Modify the backup schedule</span></span>
1. <span data-ttu-id="e154e-217">Kattintson a Microsoft Azure Backup szolgáltatás ügynökének **biztonsági mentés ütemezése**.</span><span class="sxs-lookup"><span data-stu-id="e154e-217">In the Microsoft Azure Backup agent click **Schedule Backup**.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/schedule-backup.png)
2. <span data-ttu-id="e154e-219">Az a **ütemezett biztonsági mentés varázsló** hagyja a **biztonsági másolati elemei vagy időpontok szerepelnek módosítása** jelölőnégyzetet és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="e154e-219">In the **Schedule Backup Wizard** leave the **Make changes to backup items or times** option selected and click **Next**.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/modify-or-stop-a-scheduled-backup.png)
3. <span data-ttu-id="e154e-221">Ha szeretné-e hozzáadása vagy módosítása elemek, a a **elemek kijelölése biztonsági mentéshez** képernyőn kattintson **elemek hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="e154e-221">If you want to add or change items, on the **Select Items to Backup** screen click **Add Items**.</span></span>

    <span data-ttu-id="e154e-222">Azt is beállíthatja **kizárások beállításai** ezen a lapon, a varázslóban.</span><span class="sxs-lookup"><span data-stu-id="e154e-222">You can also set **Exclusion Settings** from this page in the wizard.</span></span> <span data-ttu-id="e154e-223">Ha ki szeretné zárni a fájlok vagy fájltípusok olvassa el, hogyan adhat hozzá [kizárások beállításai](#manage-exclusion-settings).</span><span class="sxs-lookup"><span data-stu-id="e154e-223">If you want to exclude files or file types read the procedure for adding [exclusion settings](#manage-exclusion-settings).</span></span>
4. <span data-ttu-id="e154e-224">Válassza ki a fájlok és mappák biztonsági mentése, és kattintson a kívánt **gépházban**.</span><span class="sxs-lookup"><span data-stu-id="e154e-224">Select the files and folders you want to back up and click **Okay**.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/add-items-modify.png)
5. <span data-ttu-id="e154e-226">Adja meg a **biztonsági mentés ütemezése** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="e154e-226">Specify the **backup schedule** and click **Next**.</span></span>

    <span data-ttu-id="e154e-227">(A 3-szor naponkénti maximum) napi vagy heti biztonsági mentései is ütemezheti.</span><span class="sxs-lookup"><span data-stu-id="e154e-227">You can schedule daily (at a maximum of 3 times per day) or weekly backups.</span></span>

    ![Windows Server biztonsági mentés elemei](./media/backup-azure-manage-windows-server/specify-backup-schedule-modify-close.png)

   > [!NOTE]
   > <span data-ttu-id="e154e-229">Adja meg a biztonsági mentés ütemezése esetén, tekintse meg a részletes [cikk](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="e154e-229">Specifying the backup schedule is explained in detail in this [article](backup-azure-backup-cloud-as-tape.md).</span></span>
   >

6. <span data-ttu-id="e154e-230">Válassza ki a **adatmegőrzési** a biztonsági másolatot, majd kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="e154e-230">Select the **Retention Policy** for the backup copy and click **Next**.</span></span>

    ![Windows Server biztonsági mentés elemei](./media/backup-azure-manage-windows-server/select-retention-policy-modify.png)
7. <span data-ttu-id="e154e-232">Az a **megerősítő** képernyőn tekintse át az adatokat, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="e154e-232">On the **Confirmation** screen review the information and click **Finish**.</span></span>
8. <span data-ttu-id="e154e-233">Miután a varázsló befejezi a **biztonsági mentés ütemezése**, kattintson a **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="e154e-233">Once the wizard finishes creating the **backup schedule**, click **Close**.</span></span>

    <span data-ttu-id="e154e-234">Védelmi módosítása, után ellenőrizheti, hogy biztonsági mentést a megfelelő váltanak a **feladatok** lapra, és erősítse meg, hogy a módosítások megjelennek a biztonsági mentési feladatok.</span><span class="sxs-lookup"><span data-stu-id="e154e-234">After modifying protection, you can confirm that backups are triggering correctly by going to the **Jobs** tab and confirming that changes are reflected in the backup jobs.</span></span>

## <a name="enable-network-throttling"></a><span data-ttu-id="e154e-235">Hálózati sávszélesség-szabályozás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="e154e-235">Enable Network Throttling</span></span>

<span data-ttu-id="e154e-236">Az Azure Backup szolgáltatás ügynökének biztosítja a sávszélesség-szabályozási fülre, amely lehetővé teszi, hogy vezérlését a hálózati sávszélesség használatának adatátvitel során.</span><span class="sxs-lookup"><span data-stu-id="e154e-236">The Azure Backup agent provides a Throttling tab which allows you to control how network bandwidth is used during data transfer.</span></span> <span data-ttu-id="e154e-237">Ez a vezérlő akkor lehet hasznos, ha biztonsági kell során az adatokat munkaidő, de nem szeretné a biztonsági mentési folyamat zavarja a más internetes forgalmat.</span><span class="sxs-lookup"><span data-stu-id="e154e-237">This control can be helpful if you need to back up data during work hours but do not want the backup process to interfere with other internet traffic.</span></span> <span data-ttu-id="e154e-238">Átviteli biztonsági mentése és visszaállítása a tevékenységek adatok szabályozás vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="e154e-238">Throttling of data transfer applies to back up and restore activities.</span></span>  

<span data-ttu-id="e154e-239">Elemre a szabályozás engedélyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="e154e-239">To enable throttling:</span></span>

1. <span data-ttu-id="e154e-240">Az a **Backup szolgáltatás ügynökének**, kattintson a **tulajdonságainak módosítása**.</span><span class="sxs-lookup"><span data-stu-id="e154e-240">In the **Backup agent**, click **Change Properties**.</span></span>
2. <span data-ttu-id="e154e-241">Az a ** szabályozás lapra, válassza ki **engedélyezi az internetes sávszélesség szabályozásának a biztonsági mentési műveleteknél**.</span><span class="sxs-lookup"><span data-stu-id="e154e-241">On the **Throttling tab, select **Enable internet bandwidth usage throttling for backup operations**.</span></span>

    ![Hálózati sávszélesség-szabályozás](./media/backup-azure-manage-windows-server/throttling-dialog.png)

    <span data-ttu-id="e154e-243">Miután engedélyezte a sávszélesség-szabályozás, adja meg a megengedett sávszélesség vonatkozó biztonsági mentési adatátvitel során **időpontokat a munkaidőhöz** és **munkaidőn kívüli**.</span><span class="sxs-lookup"><span data-stu-id="e154e-243">Once you have enabled throttling, specify the allowed bandwidth for backup data transfer during **Work hours** and **Non-work hours**.</span></span>

    <span data-ttu-id="e154e-244">A sávszélesség értékek kezdődjenek 512 kilobájt / másodperc (Kbps), és folytathatja a legfeljebb 1023 megabájt / másodperc (Mbps).</span><span class="sxs-lookup"><span data-stu-id="e154e-244">The bandwidth values begin at 512 kilobytes per second (Kbps) and can go up to 1023 megabytes per second (Mbps).</span></span> <span data-ttu-id="e154e-245">Is kijelölni a kezdő és a Befejezés **időpontokat a munkaidőhöz**, és a hét melyik napjain minősülnek munkahelyi nap.</span><span class="sxs-lookup"><span data-stu-id="e154e-245">You can also designate the start and finish for **Work hours**, and which days of the week are considered Work days.</span></span> <span data-ttu-id="e154e-246">A megadott munkaidőn kívül idő tekinthető munkaidőn kívüli időszakra.</span><span class="sxs-lookup"><span data-stu-id="e154e-246">The time outside of the designated Work hours is considered to be non-work hours.</span></span>
3. <span data-ttu-id="e154e-247">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="e154e-247">Click **OK**.</span></span>

## <a name="manage-exclusion-settings"></a><span data-ttu-id="e154e-248">Kizárási beállításainak kezelése</span><span class="sxs-lookup"><span data-stu-id="e154e-248">Manage exclusion settings</span></span>
1. <span data-ttu-id="e154e-249">Nyissa meg a **Microsoft Azure Backup szolgáltatás ügynökének** (a gép kereséssel megtalálja *a Microsoft Azure Backup szolgáltatás*).</span><span class="sxs-lookup"><span data-stu-id="e154e-249">Open the **Microsoft Azure Backup agent** (you can find it by searching your machine for *Microsoft Azure Backup*).</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/snap-in-search.png)
2. <span data-ttu-id="e154e-251">Kattintson a Microsoft Azure Backup szolgáltatás ügynökének **biztonsági mentés ütemezése**.</span><span class="sxs-lookup"><span data-stu-id="e154e-251">In the Microsoft Azure Backup agent click **Schedule Backup**.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/schedule-backup.png)
3. <span data-ttu-id="e154e-253">A biztonsági mentés ütemezése varázsló hagyja a a **biztonsági másolati elemei vagy időpontok szerepelnek módosítása** jelölőnégyzetet és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="e154e-253">In the Schedule Backup Wizard leave the **Make changes to backup items or times** option selected and click **Next**.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/modify-or-stop-a-scheduled-backup.png)
4. <span data-ttu-id="e154e-255">Kattintson a **kizárások beállításai**.</span><span class="sxs-lookup"><span data-stu-id="e154e-255">Click **Exclusions Settings**.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/exclusion-settings.png)
5. <span data-ttu-id="e154e-257">Kattintson a **adja hozzá a kizárási**.</span><span class="sxs-lookup"><span data-stu-id="e154e-257">Click **Add Exclusion**.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/add-exclusion.png)
6. <span data-ttu-id="e154e-259">Válassza ki azt a helyet, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="e154e-259">Select the location and then, click **OK**.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/exclusion-location.png)
7. <span data-ttu-id="e154e-261">Adja hozzá a fájl kiterjesztése a **Fájltípus** mező.</span><span class="sxs-lookup"><span data-stu-id="e154e-261">Add the file extension in the **File Type** field.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/exclude-file-type.png)

    <span data-ttu-id="e154e-263">.Mp3 bővítmény felvétele</span><span class="sxs-lookup"><span data-stu-id="e154e-263">Adding an .mp3 extension</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/exclude-mp3.png)

    <span data-ttu-id="e154e-265">Egy másik bővítmény hozzáadásához kattintson **Kizárás hozzáadása** , és adja meg egy másik típus fájlnévkiterjesztés (.jpeg-bővítmény hozzáadása).</span><span class="sxs-lookup"><span data-stu-id="e154e-265">To add another extension, click **Add Exclusion** and enter another file type extension (adding a .jpeg extension).</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/exclude-jpg.png)
8. <span data-ttu-id="e154e-267">Ha a bővítményeket adott hozzá, kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="e154e-267">When you've added all the extensions, click **OK**.</span></span>
9. <span data-ttu-id="e154e-268">A biztonsági mentés ütemezése varázsló segítségével kattintva folytatni **következő** mindaddig, amíg a **visszaigazolási lapja**, kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="e154e-268">Continue through the Schedule Backup Wizard by clicking **Next** until the **Confirmation page**, then click **Finish**.</span></span>

    ![Windows Server biztonsági mentés ütemezése](./media/backup-azure-manage-windows-server/finish-exclusions.png)

## <a name="frequently-asked-questions"></a><span data-ttu-id="e154e-270">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="e154e-270">Frequently asked questions</span></span>
<span data-ttu-id="e154e-271">**1. KÉRDÉS. A biztonsági mentési feladat állapota: módon fejezte be az Azure Backup szolgáltatás ügynöke, miért nem az beszerzése kerülnek azonnal portálon?**</span><span class="sxs-lookup"><span data-stu-id="e154e-271">**Q1. The backup job status shows as completed in the Azure backup agent, why doesn't it get reflected immediately in portal?**</span></span>

<span data-ttu-id="e154e-272">1. válasz</span><span class="sxs-lookup"><span data-stu-id="e154e-272">A1.</span></span> <span data-ttu-id="e154e-273">Nincs maximális késleltetés 15 perc között a biztonsági mentési feladat állapotát a következő megjelenik az Azure Backup szolgáltatás ügynöke és az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="e154e-273">There is at maximum delay of 15 mins between the backup job status reflected in the Azure backup agent and the Azure portal.</span></span>

<span data-ttu-id="e154e-274">**Q.2, amikor egy biztonsági mentési feladat sikertelen lesz, mennyi ideig tart a riasztást?**</span><span class="sxs-lookup"><span data-stu-id="e154e-274">**Q.2 When a backup job fails, how long does it take to raise an alert?**</span></span>

<span data-ttu-id="e154e-275">A.2 riasztás jelenik meg, az Azure biztonsági mentési hiba 20 perc belül.</span><span class="sxs-lookup"><span data-stu-id="e154e-275">A.2 An alert is raised within 20 mins of the Azure backup failure.</span></span>

<span data-ttu-id="e154e-276">**3. NEGYEDÉVÉBEN. Ha egy e-mailt nem küldi el, ha értesítések beállítása eset van?**</span><span class="sxs-lookup"><span data-stu-id="e154e-276">**Q3. Is there a case where an email won’t be sent if notifications are configured?**</span></span>

<span data-ttu-id="e154e-277">3. válasz</span><span class="sxs-lookup"><span data-stu-id="e154e-277">A3.</span></span> <span data-ttu-id="e154e-278">Az alábbiakban az esetekben, amikor az értesítés nem lesz elküldve riasztás zaj csökkentése érdekében:</span><span class="sxs-lookup"><span data-stu-id="e154e-278">Below are the cases when the notification will not be sent in order to reduce the alert noise:</span></span>

* <span data-ttu-id="e154e-279">Ha az értesítések beállítása óránkénti, és az következik be van-e és órán belül megoldott riasztás</span><span class="sxs-lookup"><span data-stu-id="e154e-279">If notifications are configured hourly and an alert is raised and resolved within the hour</span></span>
* <span data-ttu-id="e154e-280">Feladat megszakadt.</span><span class="sxs-lookup"><span data-stu-id="e154e-280">Job is canceled.</span></span>
* <span data-ttu-id="e154e-281">Második biztonsági mentési feladat sikertelen volt, mert az eredeti biztonsági mentési feladat van folyamatban.</span><span class="sxs-lookup"><span data-stu-id="e154e-281">Second backup job failed because original backup job is in progress.</span></span>

## <a name="troubleshooting-monitoring-issues"></a><span data-ttu-id="e154e-282">Figyelési problémák elhárítása</span><span class="sxs-lookup"><span data-stu-id="e154e-282">Troubleshooting Monitoring Issues</span></span>
<span data-ttu-id="e154e-283">**Probléma:** feladatok és/vagy az Azure Backup szolgáltatás ügynökének származó riasztások nem jelennek meg a portálon.</span><span class="sxs-lookup"><span data-stu-id="e154e-283">**Issue:** Jobs and/or alerts from the Azure Backup agent do not appear in the portal.</span></span>

<span data-ttu-id="e154e-284">**Hibaelhárítási lépések:** a folyamat ```OBRecoveryServicesManagementAgent```, a feladat és riasztás adatokat küld az Azure Backup szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="e154e-284">**Troubleshooting steps:** The process, ```OBRecoveryServicesManagementAgent```, sends the job and alert data to the Azure Backup service.</span></span> <span data-ttu-id="e154e-285">Ez a folyamat állapottal alkalmanként vagy -leállítás.</span><span class="sxs-lookup"><span data-stu-id="e154e-285">Occasionally this process can become stuck or shutdown.</span></span>

1. <span data-ttu-id="e154e-286">Ellenőrizze, hogy a folyamat nem fut, nyissa meg a **Feladatkezelő** és ellenőrizze, hogy a ```OBRecoveryServicesManagementAgent``` folyamat fut.</span><span class="sxs-lookup"><span data-stu-id="e154e-286">To verify the process is not running, open **Task Manager** and check if the ```OBRecoveryServicesManagementAgent``` process is running.</span></span>
2. <span data-ttu-id="e154e-287">Feltételezve, hogy a folyamat nem fut, nyissa meg a **Vezérlőpult** , és keresse meg a szolgáltatások listájában.</span><span class="sxs-lookup"><span data-stu-id="e154e-287">Assuming that the process is not running, open **Control Panel** and browse the list of services.</span></span> <span data-ttu-id="e154e-288">Indítsa el, vagy indítsa újra a **Microsoft Azure Recovery Services Management Agent**.</span><span class="sxs-lookup"><span data-stu-id="e154e-288">Start or restart **Microsoft Azure Recovery Services Management Agent**.</span></span>

    <span data-ttu-id="e154e-289">További információért keresse meg a naplózásra kerül:</span><span class="sxs-lookup"><span data-stu-id="e154e-289">For further information, browse the logs at:</span></span><br/><span data-ttu-id="e154e-290">
   `<AzureBackup_agent_install_folder>\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider*`Példa:</span><span class="sxs-lookup"><span data-stu-id="e154e-290">
   `<AzureBackup_agent_install_folder>\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider*` For example:</span></span><br/>
   `C:\Program Files\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider0.errlog`

## <a name="next-steps"></a><span data-ttu-id="e154e-291">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e154e-291">Next steps</span></span>
* [<span data-ttu-id="e154e-292">Windows Server vagy a Windows ügyfél visszaállítása az Azure-ból</span><span class="sxs-lookup"><span data-stu-id="e154e-292">Restore Windows Server or Windows Client from Azure</span></span>](backup-azure-restore-windows-server.md)
* <span data-ttu-id="e154e-293">Azure biztonsági mentés kapcsolatos további információkért lásd: [Azure biztonsági mentés áttekintése](backup-introduction-to-azure-backup.md)</span><span class="sxs-lookup"><span data-stu-id="e154e-293">To learn more about Azure Backup, see [Azure Backup Overview](backup-introduction-to-azure-backup.md)</span></span>
* <span data-ttu-id="e154e-294">Látogasson el a [Azure biztonsági mentési fórum](http://go.microsoft.com/fwlink/p/?LinkId=290933)</span><span class="sxs-lookup"><span data-stu-id="e154e-294">Visit the [Azure Backup Forum](http://go.microsoft.com/fwlink/p/?LinkId=290933)</span></span>
