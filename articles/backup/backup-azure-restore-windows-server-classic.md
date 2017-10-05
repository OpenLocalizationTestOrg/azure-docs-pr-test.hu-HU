---
title: "A visszaállítás egy Windows Server vagy a Windows-ügyfél a klasszikus telepítési modell segítségével az Azure-ból |} Microsoft Docs"
description: "Megtudhatja, hogyan visszaállítása a Windows Server vagy a Windows-ügyfél."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 85585dfc-c764-4e8c-8f0e-40b969640ac2
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: saurse;trinadhk;markgal;
ms.openlocfilehash: 300b2b17b44e21ed446fd63d572a2461e2fc1343
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="restore-files-to-a-windows-server-or-windows-client-machine-using-the-classic-deployment-model"></a><span data-ttu-id="e4485-103">Fájlok visszaállítása a Windows-kiszolgálóra vagy -ügyfélre a klasszikus üzemi modell használatával</span><span class="sxs-lookup"><span data-stu-id="e4485-103">Restore files to a Windows server or Windows client machine using the classic deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e4485-104">Klasszikus portál</span><span class="sxs-lookup"><span data-stu-id="e4485-104">Classic portal</span></span>](backup-azure-restore-windows-server-classic.md)
> * [<span data-ttu-id="e4485-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e4485-105">Azure portal</span></span>](backup-azure-restore-windows-server.md)
>
>

<span data-ttu-id="e4485-106">Ez a cikk azt ismerteti, hogyan adatokat helyreállítani egy biztonsági mentési tárolóból, és annak visszaállítására egy kiszolgáló vagy a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e4485-106">This article explains how to recover data from a backup vault and restore it to a server or computer.</span></span> <span data-ttu-id="e4485-107">-Től kezdődően. március 2017, már nem készíthetők mentési tárolók a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="e4485-107">Starting in March 2017, you can no longer create backup vaults in the classic portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e4485-108">A biztonsági mentési tárolókról mostantól lehetőség van Recovery Services-tárolókra váltani.</span><span class="sxs-lookup"><span data-stu-id="e4485-108">You can now upgrade your Backup vaults to Recovery Services vaults.</span></span> <span data-ttu-id="e4485-109">A részletekről bővebben az [Váltás biztonsági mentési tárolóról Recovery Services-tárolóra](backup-azure-upgrade-backup-to-recovery-services.md) című cikkben olvashat.</span><span class="sxs-lookup"><span data-stu-id="e4485-109">For details, see the article [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="e4485-110">A Microsoft azt javasolja, hogy a biztonsági mentési tárolóról váltson Recovery Services-tárolóra.</span><span class="sxs-lookup"><span data-stu-id="e4485-110">Microsoft encourages you to upgrade your Backup vaults to Recovery Services vaults.</span></span><br/> <span data-ttu-id="e4485-111">**2017. október 15-től** a PowerShell a továbbiakban nem használható Backup-tárolók létrehozására.</span><span class="sxs-lookup"><span data-stu-id="e4485-111">**October 15, 2017**, you will no longer be able to use PowerShell to create Backup vaults.</span></span> <br/> <span data-ttu-id="e4485-112">**2017. november 1-től kezdődően**:</span><span class="sxs-lookup"><span data-stu-id="e4485-112">**Starting November 1, 2017**:</span></span>
>- <span data-ttu-id="e4485-113">A rendszer automatikusan elvégzi valamennyi megmaradó biztonsági mentési tároló átváltását Recovery Services-tárolókra.</span><span class="sxs-lookup"><span data-stu-id="e4485-113">Any remaining Backup vaults will be automatically upgraded to Recovery Services vaults.</span></span>
>- <span data-ttu-id="e4485-114">A klasszikus portálon nem lehet majd hozzáférni a biztonsági másolati adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="e4485-114">You won't be able to access your backup data in the classic portal.</span></span> <span data-ttu-id="e4485-115">Helyette az Azure Portal segítségével férhet hozzá a Recovery Services-tárolókban található biztonsági mentési adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="e4485-115">Instead, use the Azure portal to access your backup data in Recovery Services vaults.</span></span>
>

<span data-ttu-id="e4485-116">Adatok visszaállítása, használja az adatok helyreállítása varázsló a Microsoft Azure Recovery Services (MARS) ügynök.</span><span class="sxs-lookup"><span data-stu-id="e4485-116">To restore data, you use the Recover Data wizard in the Microsoft Azure Recovery Services (MARS) agent.</span></span> <span data-ttu-id="e4485-117">Adatok helyreállításakor lehetősége:</span><span class="sxs-lookup"><span data-stu-id="e4485-117">When you restore data, it is possible to:</span></span>

* <span data-ttu-id="e4485-118">Állítsa vissza az adatokat ugyanarra a számítógépre, amelyről a biztonsági mentések vették.</span><span class="sxs-lookup"><span data-stu-id="e4485-118">Restore data to the same machine from which the backups were taken.</span></span>
* <span data-ttu-id="e4485-119">A visszaállítás egy másik gépen.</span><span class="sxs-lookup"><span data-stu-id="e4485-119">Restore data to an alternate machine.</span></span>

<span data-ttu-id="e4485-120">2017. január, a Microsoft kiadott a MARS Agent előzetes frissítés.</span><span class="sxs-lookup"><span data-stu-id="e4485-120">In January 2017, Microsoft released a Preview update to the MARS agent.</span></span> <span data-ttu-id="e4485-121">Hibajavításokat tartalmaz, valamint a frissítés lehetővé teszi, hogy azonnali visszaállítása, amely lehetővé teszi egy helyreállítási kötet írható helyreállítási pont pillanatkép csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="e4485-121">Along with bug fixes, this update enables Instant Restore, which allows you to mount a writeable recovery point snapshot as a recovery volume.</span></span> <span data-ttu-id="e4485-122">A helyi számítógépre, ezáltal visszaállítása fájlok majd ismerje meg a helyreállítási kötet, és másolja a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="e4485-122">You can then explore the recovery volume and copy files to a local computer thereby selectively restoring files.</span></span>

> [!NOTE]
> <span data-ttu-id="e4485-123">A [2017. január Azure Backup frissítését](https://support.microsoft.com/en-us/help/3216528?preview) szükség, ha az adatok helyreállítását a azonnali visszaállításához használni kívánt.</span><span class="sxs-lookup"><span data-stu-id="e4485-123">The [January 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) is required if you want to use Instant Restore to restore data.</span></span> <span data-ttu-id="e4485-124">A biztonsági mentési adatok is a tárolók az területi beállításokat, a támogatási cikkben szereplő kell védeni.</span><span class="sxs-lookup"><span data-stu-id="e4485-124">Also the backup data must be protected in vaults in locales listed in the support article.</span></span> <span data-ttu-id="e4485-125">Tekintse át a [2017. január Azure Backup frissítését](https://support.microsoft.com/en-us/help/3216528?preview) területi beállításokat, amelyek támogatják a azonnali állítsa vissza a legfrissebb listáját.</span><span class="sxs-lookup"><span data-stu-id="e4485-125">Consult the [January 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) for the latest list of locales that support Instant Restore.</span></span> <span data-ttu-id="e4485-126">Azonnali visszaállítás **nem** elérhetők az összes területi beállításokat.</span><span class="sxs-lookup"><span data-stu-id="e4485-126">Instant Restore is **not** currently available in all locales.</span></span>
>

<span data-ttu-id="e4485-127">Azonnali visszaállítási nem áll rendelkezésre a Recovery Services-tárolók használható az Azure portál és a biztonsági mentési tárolók a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="e4485-127">Instant Restore is available for use in Recovery Services vaults in the Azure portal and Backup vaults in the classic portal.</span></span> <span data-ttu-id="e4485-128">Ha szeretné használni a azonnali visszaállítása, töltse le a MARS frissítést, és hajtsa végre, amelyek említik azonnali visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="e4485-128">If you want to use Instant Restore, download the MARS update, and follow the procedures that mention Instant Restore.</span></span>


## <a name="use-instant-restore-to-recover-data-to-the-same-machine"></a><span data-ttu-id="e4485-129">Ugyanaz a gép adatok helyreállíthatók a azonnali visszaállítás segítségével</span><span class="sxs-lookup"><span data-stu-id="e4485-129">Use Instant Restore to recover data to the same machine</span></span>

<span data-ttu-id="e4485-130">Ha véletlenül törölte a fájlt, és ugyanarra a számítógépre (ahol a biztonsági mentés lesz végrehajtva) visszaállítására szeretnének, a következő lépésekkel állítsa helyre az adatokat.</span><span class="sxs-lookup"><span data-stu-id="e4485-130">If you accidentally deleted a file and wish to restore it to the same machine (from which the backup is taken), the following steps will help you recover the data.</span></span>

1. <span data-ttu-id="e4485-131">Nyissa meg a **a Microsoft Azure Backup szolgáltatás** az illesztési.</span><span class="sxs-lookup"><span data-stu-id="e4485-131">Open the **Microsoft Azure Backup** snap in.</span></span> <span data-ttu-id="e4485-132">Ha nem tudja, ahová a beépülő modul telepítve van, a számítógép vagy a kiszolgáló keresése **a Microsoft Azure Backup szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="e4485-132">If you don't know where the snap in was installed, search the computer or server for **Microsoft Azure Backup**.</span></span>

    <span data-ttu-id="e4485-133">Egy asztali alkalmazás meg kell jelennie a keresési eredmények között.</span><span class="sxs-lookup"><span data-stu-id="e4485-133">The desktop app should appear in the search results.</span></span>

2. <span data-ttu-id="e4485-134">Kattintson a **adatok helyreállítása** varázsló elindításához.</span><span class="sxs-lookup"><span data-stu-id="e4485-134">Click **Recover Data** to start the wizard.</span></span>

    ![Adatok helyreállítása](./media/backup-azure-restore-windows-server/recover.png)

3. <span data-ttu-id="e4485-136">Az a **bevezetés** ablaktáblán, az adatok helyreállítását a kiszolgálóhoz vagy a számítógépen, válassza ki **ehhez a kiszolgálóhoz (`<server name>`)** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="e4485-136">On the **Getting Started** pane, to restore the data to the same server or computer, select **This server (`<server name>`)** and click **Next**.</span></span>

    ![Állítsa vissza az adatokat ugyanarra a számítógépre a kiszolgáló lehetőség kiválasztása](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. <span data-ttu-id="e4485-138">A a **válassza ki a helyreállítási mód** ablaktáblában válassza **egyes fájlok és mappák** , majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="e4485-138">On the **Select Recovery Mode** pane, choose **Individual files and folders** and then click **Next**.</span></span>

    ![Fájlok tallózása](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)

5. <span data-ttu-id="e4485-140">A a **mennyiség kiválasztása és a dátum** ablaktáblán válassza ki a kötetet, amely tartalmazza a fájlok és/vagy a visszaállítani kívánt mappákat.</span><span class="sxs-lookup"><span data-stu-id="e4485-140">On the **Select Volume and Date** pane, select the volume that contains the files and/or folders you want to restore.</span></span>

    <span data-ttu-id="e4485-141">A naptárban válasszon ki egy helyreállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="e4485-141">On the calendar, select a recovery point.</span></span> <span data-ttu-id="e4485-142">Bármelyik helyreállítási pontra állíthatja vissza időben.</span><span class="sxs-lookup"><span data-stu-id="e4485-142">You can restore from any recovery point in time.</span></span> <span data-ttu-id="e4485-143">A dátumok **félkövér** jelző legalább egy helyreállítási pont rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="e4485-143">Dates in **bold** indicate the availability of at least one recovery point.</span></span> <span data-ttu-id="e4485-144">Miután egy dátumot, ha több helyreállítási pont nem érhető el, válassza ki az adott helyreállítási pontot a **idő** legördülő menü.</span><span class="sxs-lookup"><span data-stu-id="e4485-144">Once you select a date, if multiple recovery points are available, choose the specific recovery point from the **Time** drop-down menu.</span></span>

    ![Kötet és a dátum](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. <span data-ttu-id="e4485-146">Miután kiválasztotta a visszaállítani kívánt helyreállítási pontot, kattintson a **csatlakoztatási**.</span><span class="sxs-lookup"><span data-stu-id="e4485-146">Once you have chosen the recovery point to restore, click **Mount**.</span></span>

    <span data-ttu-id="e4485-147">Azure biztonsági mentés, a helyi helyreállítási pontot csatlakoztatja, és egy helyreállítási kötet használja.</span><span class="sxs-lookup"><span data-stu-id="e4485-147">Azure Backup mounts the local recovery point, and uses it as a recovery volume.</span></span>

7. <span data-ttu-id="e4485-148">Az a **tallózással keresse meg és a fájlok helyreállítása** ablaktáblán kattintson a **Tallózás** nyissa meg a Windows Intézőt, és keresse meg a fájlokat és mappákat, amelyekről.</span><span class="sxs-lookup"><span data-stu-id="e4485-148">On the **Browse and Recover Files** pane, click **Browse** to open Windows Explorer and find the files and folders you want.</span></span>

    ![Helyreállítási beállítások](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)


8. <span data-ttu-id="e4485-150">A Windows Intézőben másolja a fájlokat és/vagy mappákat szeretné visszaállítani, majd illessze be a helyi a kiszolgálóhoz vagy a számítógép bármely helyére.</span><span class="sxs-lookup"><span data-stu-id="e4485-150">In Windows Explorer, copy the files and/or folders you want to restore and paste them to any location local to the server or computer.</span></span> <span data-ttu-id="e4485-151">Nyissa meg vagy adatfolyamként küldje el a fájlok közvetlenül a helyreállítási kötet, és ellenőrizze, hogy a megfelelő verzióra módon legyenek helyreállíthatók.</span><span class="sxs-lookup"><span data-stu-id="e4485-151">You can open or stream the files directly from the recovery volume and verify the correct versions are recovered.</span></span>

    ![Másolja és illessze be a fájlok és mappák csatlakoztatott kötet helyi helyre](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)

9. <span data-ttu-id="e4485-153">Ha elkészült a fájlok és/vagy mappák visszaállítása a a **tallózással keresse meg és a helyreállítási fájlokat** ablaktáblán kattintson a **leválasztási**.</span><span class="sxs-lookup"><span data-stu-id="e4485-153">When you are finished restoring the files and/or folders, on the **Browse and Recovery Files** pane, click **Unmount**.</span></span> <span data-ttu-id="e4485-154">Kattintson a **Igen** annak ellenőrzéséhez, hogy szeretné-e a kötet leválasztása.</span><span class="sxs-lookup"><span data-stu-id="e4485-154">Then click **Yes** to confirm that you want to unmount the volume.</span></span>

    ![A kötet leválasztása és megerősítése](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > <span data-ttu-id="e4485-156">Ha nem kattint az leválasztási, a helyreállítási kötet marad csatlakoztatott hat órán át a óta, amikor csatlakoztatva lett.</span><span class="sxs-lookup"><span data-stu-id="e4485-156">If you do not click Unmount, the Recovery Volume will remain mounted for six hours from the time when it was mounted.</span></span> <span data-ttu-id="e4485-157">Nincs biztonsági mentési műveletek fog futni, amíg a kötet csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="e4485-157">No backup operations will run while the volume is mounted.</span></span> <span data-ttu-id="e4485-158">Bármely az időszakban, ha a kötet csatlakoztatva, ütemezett biztonsági mentési művelet után a helyreállítási kötet nem fog futni.</span><span class="sxs-lookup"><span data-stu-id="e4485-158">Any backup operation scheduled to run during the time when the volume is mounted, will run after the recovery volume is unmounted.</span></span>
    >


## <a name="recover-data-to-the-same-machine"></a><span data-ttu-id="e4485-159">Ugyanarra a számítógépre adatok helyreállítása</span><span class="sxs-lookup"><span data-stu-id="e4485-159">Recover data to the same machine</span></span>
<span data-ttu-id="e4485-160">Ha véletlenül törölte a fájlt, és ugyanarra a számítógépre (ahol a biztonsági mentés lesz végrehajtva) visszaállítására szeretnének, a következő lépésekkel állítsa helyre az adatokat.</span><span class="sxs-lookup"><span data-stu-id="e4485-160">If you accidentally deleted a file and wish to restore it to the same machine (from which the backup is taken), the following steps will help you recover the data.</span></span>

1. <span data-ttu-id="e4485-161">Nyissa meg a **a Microsoft Azure Backup szolgáltatás** az illesztési.</span><span class="sxs-lookup"><span data-stu-id="e4485-161">Open the **Microsoft Azure Backup** snap in.</span></span>
2. <span data-ttu-id="e4485-162">Kattintson a **adatok helyreállítása** a munkafolyamat elindítására.</span><span class="sxs-lookup"><span data-stu-id="e4485-162">Click **Recover Data** to initiate the workflow.</span></span>

    ![Adatok helyreállítása](./media/backup-azure-restore-windows-server-classic/recover.png)
3. <span data-ttu-id="e4485-164">Válassza ki a  **ehhez a kiszolgálóhoz (*yourmachinename*) ** fájl ugyanazon a számítógépen a biztonsági másolat visszaállítása a beállítást.</span><span class="sxs-lookup"><span data-stu-id="e4485-164">Select the **This server (*yourmachinename*)** option to restore the backed up file on the same machine.</span></span>

    ![Ugyanaz a gép](./media/backup-azure-restore-windows-server-classic/samemachine.png)
4. <span data-ttu-id="e4485-166">Válassza ki a **fájlok tallózása** vagy **fájlok keresése**.</span><span class="sxs-lookup"><span data-stu-id="e4485-166">Choose to **Browse for files** or **Search for files**.</span></span>

    <span data-ttu-id="e4485-167">Hagyja bejelölve az alapértelmezett beállítás, ha azt tervezi, hogy az elérési úton található egy vagy több fájlok helyreállítása.</span><span class="sxs-lookup"><span data-stu-id="e4485-167">Leave the default option if you plan to restore one or more files whose path is known.</span></span> <span data-ttu-id="e4485-168">Ha nem tudja biztosan, hogy a mappaszerkezetet, de szeretné megkeresni a fájlt, válassza ki a **fájlok keresése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="e4485-168">If you are not sure about the folder structure but would like to search for a file, pick the **Search for files** option.</span></span> <span data-ttu-id="e4485-169">Ez a szakasz céljából azt folytatja az alapértelmezett beállítás.</span><span class="sxs-lookup"><span data-stu-id="e4485-169">For the purpose of this section, we will proceed with the default option.</span></span>

    ![Fájlok tallózása](./media/backup-azure-restore-windows-server-classic/browseandsearch.png)
5. <span data-ttu-id="e4485-171">Válassza ki a kötetet, amelyen állítsa vissza a fájlt kívánja.</span><span class="sxs-lookup"><span data-stu-id="e4485-171">Select the volume from which you wish to restore the file.</span></span>

    <span data-ttu-id="e4485-172">Minden olyan pont állíthatja vissza időben.</span><span class="sxs-lookup"><span data-stu-id="e4485-172">You can restore from any point in time.</span></span> <span data-ttu-id="e4485-173">Megjelenő dátumok **félkövér** a havinaptár-vezérlőben meg, a helyreállítási pont rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="e4485-173">Dates which appear in **bold** in the calendar control indicate the availability of a restore point.</span></span> <span data-ttu-id="e4485-174">A kijelölt dátum a biztonsági mentési ütemezést (és a biztonsági mentés sikeres) alapján választhat egy pont időpontját a **idő** legördülő listán.</span><span class="sxs-lookup"><span data-stu-id="e4485-174">Once a date is selected, based on your backup schedule (and the success of a backup operation), you can select a point in time from the **Time** drop down.</span></span>

    ![Kötet és a dátum](./media/backup-azure-restore-windows-server-classic/volanddate.png)
6. <span data-ttu-id="e4485-176">Válassza ki a helyreállítandó elemek.</span><span class="sxs-lookup"><span data-stu-id="e4485-176">Select the items to recover.</span></span> <span data-ttu-id="e4485-177">Beállíthatja a többszörös kiválasztási mappák és fájlok kívánja visszaállítani.</span><span class="sxs-lookup"><span data-stu-id="e4485-177">You can multi-select folders/files you wish to restore.</span></span>

    ![Fájlok kiválasztása](./media/backup-azure-restore-windows-server-classic/selectfiles.png)
7. <span data-ttu-id="e4485-179">Adja meg a helyreállítási paramétereket.</span><span class="sxs-lookup"><span data-stu-id="e4485-179">Specify the recovery parameters.</span></span>

    ![Helyreállítási beállítások](./media/backup-azure-restore-windows-server-classic/recoveroptions.png)

   * <span data-ttu-id="e4485-181">Lehetősége van az eredeti helyre (amelyben a fájl vagy mappa volna felülírja), vagy egyazon számítógépen egy másik helyre állítja vissza.</span><span class="sxs-lookup"><span data-stu-id="e4485-181">You have an option of restoring to the original location (in which the file/folder would be overwritten) or to another location in the same machine.</span></span>
   * <span data-ttu-id="e4485-182">A visszaállítani kívánt fájl vagy mappa a célhelyen létezésének (ugyanazon fájl két verziója)-másolatok létrehozása, felülírja a fájlokat a célhelyen, vagy a fájlok a cél létező helyreállításának kihagyása.</span><span class="sxs-lookup"><span data-stu-id="e4485-182">If the file/folder you wish to restore exists in the target location, you can create copies (two versions of the same file), overwrite the files in the target location, or skip the recovery of the files which exist in the target.</span></span>
   * <span data-ttu-id="e4485-183">Erősen ajánlott, hogy hagyja bejelölve az alapértelmezett beállítás állítja vissza az ACL-ek a helyreállítás alatt álló fájlokat.</span><span class="sxs-lookup"><span data-stu-id="e4485-183">It is highly recommended that you leave the default option of restoring the ACLs on the files which are being recovered.</span></span>
8. <span data-ttu-id="e4485-184">Amikor a bemeneti adatok érhetők el, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="e4485-184">Once these inputs are provided, click **Next**.</span></span> <span data-ttu-id="e4485-185">A helyreállítási munkafolyamat, amely a fájlok ezen a számítógépen, megkezdődik.</span><span class="sxs-lookup"><span data-stu-id="e4485-185">The recovery workflow, which restores the files to this machine, will begin.</span></span>

## <a name="recover-to-an-alternate-machine"></a><span data-ttu-id="e4485-186">Helyreállítás egy másik gépen</span><span class="sxs-lookup"><span data-stu-id="e4485-186">Recover to an alternate machine</span></span>
<span data-ttu-id="e4485-187">A teljes kiszolgáló nem vesztek el, ha akkor is továbbra is állíthat helyre adatokat az Azure Backup másik gépre.</span><span class="sxs-lookup"><span data-stu-id="e4485-187">If your entire server is lost, you can still recover data from Azure Backup to a different machine.</span></span> <span data-ttu-id="e4485-188">A következő lépések bemutatják a munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="e4485-188">The following steps illustrate the workflow.</span></span>  

<span data-ttu-id="e4485-189">Ezeket a lépéseket a használt terminológiával alábbiakat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="e4485-189">The terminology used in these steps includes:</span></span>

* <span data-ttu-id="e4485-190">*Forrásgép* – a biztonsági mentés készült, és ez jelenleg nem érhető el az eredeti gép.</span><span class="sxs-lookup"><span data-stu-id="e4485-190">*Source machine* – The original machine from which the backup was taken and which is currently unavailable.</span></span>
* <span data-ttu-id="e4485-191">*Célszámítógép* – a gép, amelyre az adatok helyreállítása.</span><span class="sxs-lookup"><span data-stu-id="e4485-191">*Target machine* – The machine to which the data is being recovered.</span></span>
* <span data-ttu-id="e4485-192">*Minta tároló* – a mentési tárolóba, amelyhez a *forrásgép* és *célgépen* van regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="e4485-192">*Sample vault* – The Backup vault to which the *Source machine* and *Target machine* are registered.</span></span> <br/>

> [!NOTE]
> <span data-ttu-id="e4485-193">A gépről készített biztonsági másolatok nem állítható vissza egy számítógépen, amelyen az operációs rendszer korábbi verzióját.</span><span class="sxs-lookup"><span data-stu-id="e4485-193">Backups taken from a machine cannot be restored on a machine which is running an earlier version of the operating system.</span></span> <span data-ttu-id="e4485-194">Például ha biztonsági mentést készít a Windows 7 gépről, akkor visszaállítása végezhető el a Windows 8 vagy újabb gép.</span><span class="sxs-lookup"><span data-stu-id="e4485-194">For example, if backups are taken from a Windows 7 machine, it can be restored on a Windows 8 or above machine.</span></span> <span data-ttu-id="e4485-195">Azonban a viszont nem rendelkezik igaz.</span><span class="sxs-lookup"><span data-stu-id="e4485-195">However, the vice-versa does not hold true.</span></span>
>
>

1. <span data-ttu-id="e4485-196">Nyissa meg a **a Microsoft Azure Backup szolgáltatás** a az illesztési a *célgépen*.</span><span class="sxs-lookup"><span data-stu-id="e4485-196">Open the **Microsoft Azure Backup** snap in on the *Target machine*.</span></span>
2. <span data-ttu-id="e4485-197">Győződjön meg arról, hogy a *célgépen* és a *forrásgép* ugyanazt a biztonsági mentési tárolóhoz regisztrált.</span><span class="sxs-lookup"><span data-stu-id="e4485-197">Ensure that the *Target machine* and the *Source machine* are registered to the same backup vault.</span></span>
3. <span data-ttu-id="e4485-198">Kattintson a **adatok helyreállítása** a munkafolyamat elindítására.</span><span class="sxs-lookup"><span data-stu-id="e4485-198">Click **Recover Data** to initiate the workflow.</span></span>

    ![Adatok helyreállítása](./media/backup-azure-restore-windows-server-classic/recover.png)
4. <span data-ttu-id="e4485-200">Válassza ki **egy másik kiszolgáló**</span><span class="sxs-lookup"><span data-stu-id="e4485-200">Select **Another server**</span></span>

    ![Egy másik kiszolgáló](./media/backup-azure-restore-windows-server-classic/anotherserver.png)
5. <span data-ttu-id="e4485-202">Adja meg a tároló hitelesítő adatait tartalmazó fájlt, amely megfelel a *minta tároló*.</span><span class="sxs-lookup"><span data-stu-id="e4485-202">Provide the vault credential file that corresponds to the *Sample vault*.</span></span> <span data-ttu-id="e4485-203">Ha a tárolói hitelesítő adatok fájlját érvénytelen (vagy lejárt) töltse le az új tárolói hitelesítő adatok fájlját az *minta tároló* a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="e4485-203">If the vault credential file is invalid (or expired) download a new vault credential file from the *Sample vault* in the Azure classic portal.</span></span> <span data-ttu-id="e4485-204">Miután a tárolói hitelesítő adatok fájlját valósul meg, szemben a tároló hitelesítő adatait tartalmazó fájlt a mentési tároló jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="e4485-204">Once the vault credential file is provided, the backup vault against the vault credential file is displayed.</span></span>
6. <span data-ttu-id="e4485-205">Válassza ki a *forrásgép* megjelenített gépet a listából.</span><span class="sxs-lookup"><span data-stu-id="e4485-205">Select the *Source machine* from the list of displayed machines.</span></span>

    ![Számítógépek listája](./media/backup-azure-restore-windows-server-classic/machinelist.png)
7. <span data-ttu-id="e4485-207">Válassza a **fájlok keresése** vagy **fájlok tallózása** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="e4485-207">Select either the **Search for files** or **Browse for files** option.</span></span> <span data-ttu-id="e4485-208">Ez a szakasz céljából használjuk a **fájlok keresése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="e4485-208">For the purpose of this section, we will use the **Search for files** option.</span></span>

    ![Keresés](./media/backup-azure-restore-windows-server-classic/search.png)
8. <span data-ttu-id="e4485-210">Válassza ki a kötet és a dátum a következő képernyőn.</span><span class="sxs-lookup"><span data-stu-id="e4485-210">Select the volume and date in the next screen.</span></span> <span data-ttu-id="e4485-211">Keresse meg a visszaállítani kívánt fájl/mappa nevét.</span><span class="sxs-lookup"><span data-stu-id="e4485-211">Search for the folder/file name you want to restore.</span></span>

    ![Keresési elemek](./media/backup-azure-restore-windows-server-classic/searchitems.png)
9. <span data-ttu-id="e4485-213">Válassza ki a helyet, ahol a fájlokat kell állítani.</span><span class="sxs-lookup"><span data-stu-id="e4485-213">Select the location where the files need to be restored.</span></span>

    ![Hely visszaállítása](./media/backup-azure-restore-windows-server-classic/restorelocation.png)
10. <span data-ttu-id="e4485-215">Adja meg a titkosítási jelszót, amely során *forrás gép* regisztrálási *minta tároló*.</span><span class="sxs-lookup"><span data-stu-id="e4485-215">Provide the encryption passphrase that was provided during *Source machine’s* registration to *Sample vault*.</span></span>

    ![Titkosítás](./media/backup-azure-restore-windows-server-classic/encryption.png)
11. <span data-ttu-id="e4485-217">Amikor a bemeneti valósul meg, kattintson a **helyreállítása**, amely elindítja a megadott célt a biztonsági mentésben szereplő fájlok helyreállítását.</span><span class="sxs-lookup"><span data-stu-id="e4485-217">Once the input is provided, click **Recover**, which triggers the restore of the backed up files to the destination provided.</span></span>

## <a name="use-instant-restore-to-restore-data-to-an-alternate-machine"></a><span data-ttu-id="e4485-218">Állítsa vissza az adatokat egy másik számítógépre a azonnali visszaállítás segítségével</span><span class="sxs-lookup"><span data-stu-id="e4485-218">Use Instant Restore to restore data to an alternate machine</span></span>
<span data-ttu-id="e4485-219">A teljes kiszolgáló nem vesztek el, ha akkor is továbbra is állíthat helyre adatokat az Azure Backup másik gépre.</span><span class="sxs-lookup"><span data-stu-id="e4485-219">If your entire server is lost, you can still recover data from Azure Backup to a different machine.</span></span> <span data-ttu-id="e4485-220">A következő lépések bemutatják a munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="e4485-220">The following steps illustrate the workflow.</span></span>

<span data-ttu-id="e4485-221">Ezeket a lépéseket a használt terminológiával alábbiakat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="e4485-221">The terminology used in these steps includes:</span></span>

* <span data-ttu-id="e4485-222">*Forrásgép* – a biztonsági mentés készült, és ez jelenleg nem érhető el az eredeti gép.</span><span class="sxs-lookup"><span data-stu-id="e4485-222">*Source machine* – The original machine from which the backup was taken and which is currently unavailable.</span></span>
* <span data-ttu-id="e4485-223">*Célszámítógép* – a gép, amelyre az adatok helyreállítása.</span><span class="sxs-lookup"><span data-stu-id="e4485-223">*Target machine* – The machine to which the data is being recovered.</span></span>
* <span data-ttu-id="e4485-224">*Minta tároló* – a Recovery Services-tároló, amelyhez a *forrásgép* és *célgépen* van regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="e4485-224">*Sample vault* – The Recovery Services vault to which the *Source machine* and *Target machine* are registered.</span></span> <br/>

> [!NOTE]
> <span data-ttu-id="e4485-225">Biztonsági mentés nem állítható vissza egy célszámítógépen az operációs rendszer korábbi verzióját futtató.</span><span class="sxs-lookup"><span data-stu-id="e4485-225">Backups can't be restored to a target machine running an earlier version of the operating system.</span></span> <span data-ttu-id="e4485-226">Például egy készült biztonsági másolatok a Windows 7 számítógép lehet vissza a Windows 8-as vagy újabb verzióját, számítógép.</span><span class="sxs-lookup"><span data-stu-id="e4485-226">For example, a backup taken from a Windows 7 computer can be restored on a Windows 8, or later, computer.</span></span> <span data-ttu-id="e4485-227">Nem állítható vissza egy készült biztonsági másolatok a Windows 8 számítógépről a Windows 7 rendszerű számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e4485-227">A backup taken from a Windows 8 computer cannot be restored to a Windows 7 computer.</span></span>
>
>

1. <span data-ttu-id="e4485-228">Nyissa meg a **a Microsoft Azure Backup szolgáltatás** a az illesztési a *célgépen*.</span><span class="sxs-lookup"><span data-stu-id="e4485-228">Open the **Microsoft Azure Backup** snap in on the *Target machine*.</span></span>

2. <span data-ttu-id="e4485-229">Győződjön meg arról a *célgépen* és a *forrásgép* ugyanazt a Recovery Services-tároló van regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="e4485-229">Ensure the *Target machine* and the *Source machine* are registered to the same Recovery Services vault.</span></span>

3. <span data-ttu-id="e4485-230">Kattintson a **adatok helyreállítása** megnyitásához a **adatok helyreállítása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="e4485-230">Click **Recover Data** to open the **Recover Data wizard**.</span></span>

    ![Adatok helyreállítása](./media/backup-azure-restore-windows-server/recover.png)

4. <span data-ttu-id="e4485-232">Az a **bevezetés** ablaktáblán válassza előbb **egy másik kiszolgáló**</span><span class="sxs-lookup"><span data-stu-id="e4485-232">On the **Getting Started** pane, select **Another server**</span></span>

    ![Egy másik kiszolgáló](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. <span data-ttu-id="e4485-234">Adja meg a tároló hitelesítő adatait tartalmazó fájlt, amely megfelel a *minta tároló*, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="e4485-234">Provide the vault credential file that corresponds to the *Sample vault*, and click **Next**.</span></span>

    <span data-ttu-id="e4485-235">Ha a tárolói hitelesítő adatok fájlját érvénytelen (vagy lejárt), töltse le az új tárolói hitelesítő adatok fájlját az *minta tároló* az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="e4485-235">If the vault credential file is invalid (or expired), download a new vault credential file from the *Sample vault* in the Azure portal.</span></span> <span data-ttu-id="e4485-236">Miután megadta a egy érvényes tároló hitelesítő adatait, a megfelelő mentési tároló neve jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="e4485-236">Once you provide a valid vault credential, the name of the corresponding Backup Vault appears.</span></span>

6. <span data-ttu-id="e4485-237">Az a **biztonsági mentés kiszolgáló kiválasztása** ablaktáblán válassza ki a *forrásgép* megjelenített gépet a listából, és adja meg a jelszót.</span><span class="sxs-lookup"><span data-stu-id="e4485-237">On the **Select Backup Server** pane, select the *Source machine* from the list of displayed machines and provide the passphrase.</span></span> <span data-ttu-id="e4485-238">Ezután kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="e4485-238">Then click **Next**.</span></span>

    ![Számítógépek listája](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. <span data-ttu-id="e4485-240">Az a **válassza ki a helyreállítási mód** ablaktáblán válassza ki az **egyes fájlok és mappák** kattintson **tovább**.</span><span class="sxs-lookup"><span data-stu-id="e4485-240">On the **Select Recovery Mode** pane, select **Individual files and folders** and click **Next**.</span></span>

    ![Keresés](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. <span data-ttu-id="e4485-242">A a **mennyiség kiválasztása és a dátum** ablaktáblán válassza ki a kötetet, amely tartalmazza a fájlok és/vagy a visszaállítani kívánt mappákat.</span><span class="sxs-lookup"><span data-stu-id="e4485-242">On the **Select Volume and Date** pane, select the volume that contains the files and/or folders you want to restore.</span></span>

    <span data-ttu-id="e4485-243">A naptárban válasszon ki egy helyreállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="e4485-243">On the calendar, select a recovery point.</span></span> <span data-ttu-id="e4485-244">Bármelyik helyreállítási pontra állíthatja vissza időben.</span><span class="sxs-lookup"><span data-stu-id="e4485-244">You can restore from any recovery point in time.</span></span> <span data-ttu-id="e4485-245">A dátumok **félkövér** jelző legalább egy helyreállítási pont rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="e4485-245">Dates in **bold** indicate the availability of at least one recovery point.</span></span> <span data-ttu-id="e4485-246">Miután egy dátumot, ha több helyreállítási pont nem érhető el, válassza ki az adott helyreállítási pontot a **idő** legördülő menü.</span><span class="sxs-lookup"><span data-stu-id="e4485-246">Once you select a date, if multiple recovery points are available, choose the specific recovery point from the **Time** drop-down menu.</span></span>

    ![Keresési elemek](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. <span data-ttu-id="e4485-248">Kattintson a **csatlakoztatási** helyileg csatlakoztatása a helyreállítás kötetként a helyreállítási pont a *célgépen*.</span><span class="sxs-lookup"><span data-stu-id="e4485-248">Click **Mount** to locally mount the recovery point as a recovery volume on your *Target machine*.</span></span>

10. <span data-ttu-id="e4485-249">Az a **tallózással keresse meg és a fájlok helyreállítása** ablaktáblán kattintson a **Tallózás** nyissa meg a Windows Intézőt, és keresse meg a fájlokat és mappákat, amelyekről.</span><span class="sxs-lookup"><span data-stu-id="e4485-249">On the **Browse and Recover Files** pane, click **Browse** to open Windows Explorer and find the files and folders you want.</span></span>

    ![Titkosítás](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. <span data-ttu-id="e4485-251">A Windows Intézőben, másolja a fájlokat és/vagy mappák a helyreállítási kötet be őket a *célgépen* helyét.</span><span class="sxs-lookup"><span data-stu-id="e4485-251">In Windows Explorer, copy the files and/or folders from the recovery volume and paste them to your *Target machine* location.</span></span> <span data-ttu-id="e4485-252">Nyissa meg vagy adatfolyamként küldje el a fájlok közvetlenül a helyreállítási kötet, és ellenőrizze, hogy a megfelelő verzióra módon legyenek helyreállíthatók.</span><span class="sxs-lookup"><span data-stu-id="e4485-252">You can open or stream the files directly from the recovery volume and verify the correct versions are recovered.</span></span>

    ![Titkosítás](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. <span data-ttu-id="e4485-254">Ha elkészült a fájlok és/vagy mappák visszaállítása a a **tallózással keresse meg és a helyreállítási fájlokat** ablaktáblán kattintson a **leválasztási**.</span><span class="sxs-lookup"><span data-stu-id="e4485-254">When you are finished restoring the files and/or folders, on the **Browse and Recovery Files** pane, click **Unmount**.</span></span> <span data-ttu-id="e4485-255">Kattintson a **Igen** annak ellenőrzéséhez, hogy szeretné-e a kötet leválasztása.</span><span class="sxs-lookup"><span data-stu-id="e4485-255">Then click **Yes** to confirm that you want to unmount the volume.</span></span>

    ![Titkosítás](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > <span data-ttu-id="e4485-257">Ha nem kattint az leválasztási, a helyreállítási kötet marad csatlakoztatott hat órán át a óta, amikor csatlakoztatva lett.</span><span class="sxs-lookup"><span data-stu-id="e4485-257">If you do not click Unmount, the Recovery Volume will remain mounted for six hours from the time when it was mounted.</span></span> <span data-ttu-id="e4485-258">Nincs biztonsági mentési műveletek fog futni, amíg a kötet csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="e4485-258">No backup operations will run while the volume is mounted.</span></span> <span data-ttu-id="e4485-259">Bármely az időszakban, ha a kötet csatlakoztatva, ütemezett biztonsági mentési művelet után a helyreállítási kötet nem fog futni.</span><span class="sxs-lookup"><span data-stu-id="e4485-259">Any backup operation scheduled to run during the time when the volume is mounted, will run after the recovery volume is unmounted.</span></span>
    >


## <a name="next-steps"></a><span data-ttu-id="e4485-260">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e4485-260">Next steps</span></span>
* [<span data-ttu-id="e4485-261">Az Azure biztonsági mentési – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="e4485-261">Azure Backup FAQ</span></span>](backup-azure-backup-faq.md)
* <span data-ttu-id="e4485-262">Látogasson el a [Azure biztonsági mentési fórum](http://go.microsoft.com/fwlink/p/?LinkId=290933).</span><span class="sxs-lookup"><span data-stu-id="e4485-262">Visit the [Azure Backup Forum](http://go.microsoft.com/fwlink/p/?LinkId=290933).</span></span>

## <a name="learn-more"></a><span data-ttu-id="e4485-263">Részletek</span><span class="sxs-lookup"><span data-stu-id="e4485-263">Learn more</span></span>
* [<span data-ttu-id="e4485-264">Az Azure biztonsági mentés áttekintése</span><span class="sxs-lookup"><span data-stu-id="e4485-264">Azure Backup Overview</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=222425)
* [<span data-ttu-id="e4485-265">Az Azure virtuális gépek biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="e4485-265">Backup Azure virtual machines</span></span>](backup-azure-vms-introduction.md)
* [<span data-ttu-id="e4485-266">Fel Microsoft-munkaterhelések biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="e4485-266">Backup up Microsoft workloads</span></span>](backup-azure-dpm-introduction.md)
