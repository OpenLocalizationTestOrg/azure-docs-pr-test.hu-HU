---
title: "aaaRestore adatok tooa Windows Server vagy a Windows ügyfél Azure használja a klasszikus üzembe helyezési modellel hello |} Microsoft Docs"
description: "Megtudhatja, hogyan toorestore egy Windows Server vagy a Windows-ügyfél."
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
ms.openlocfilehash: 4d1458d5233c4f55004ecfa95cbf7b3b18a03dde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restore-files-tooa-windows-server-or-windows-client-machine-using-hello-classic-deployment-model"></a><span data-ttu-id="5c7b6-103">Fájlok tooa Windows server vagy a Windows hello klasszikus üzembe helyezési modellt használó ügyfélszámítógép visszaállítása</span><span class="sxs-lookup"><span data-stu-id="5c7b6-103">Restore files tooa Windows server or Windows client machine using hello classic deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5c7b6-104">Klasszikus portál</span><span class="sxs-lookup"><span data-stu-id="5c7b6-104">Classic portal</span></span>](backup-azure-restore-windows-server-classic.md)
> * [<span data-ttu-id="5c7b6-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5c7b6-105">Azure portal</span></span>](backup-azure-restore-windows-server.md)
>
>

<span data-ttu-id="5c7b6-106">Ez a cikk azt ismerteti, hogyan toorecover adatokat egy biztonsági másolatból tároló, és visszaállíthatja a tooa kiszolgálón vagy számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-106">This article explains how toorecover data from a backup vault and restore it tooa server or computer.</span></span> <span data-ttu-id="5c7b6-107">-Től kezdődően. március 2017, már nem készíthetők mentési tárolók hello a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-107">Starting in March 2017, you can no longer create backup vaults in hello classic portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5c7b6-108">Most már frissítheti a mentési tárolók tooRecovery szolgáltatások tárolókban.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-108">You can now upgrade your Backup vaults tooRecovery Services vaults.</span></span> <span data-ttu-id="5c7b6-109">További információkért lásd: hello cikk [frissíteni a biztonsági mentési tároló tooa Recovery Services-tároló](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="5c7b6-109">For details, see hello article [Upgrade a Backup vault tooa Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="5c7b6-110">A Microsoft tooupgrade támogatja a mentési tárolókat tooRecovery szolgáltatások tárolók.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-110">Microsoft encourages you tooupgrade your Backup vaults tooRecovery Services vaults.</span></span><br/> <span data-ttu-id="5c7b6-111">**2017. október 15**, akkor nem fog tudni toouse PowerShell toocreate mentési tárolókban.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-111">**October 15, 2017**, you will no longer be able toouse PowerShell toocreate Backup vaults.</span></span> <br/> <span data-ttu-id="5c7b6-112">**2017. november 1-től kezdődően**:</span><span class="sxs-lookup"><span data-stu-id="5c7b6-112">**Starting November 1, 2017**:</span></span>
>- <span data-ttu-id="5c7b6-113">A többi biztonsági mentési tárolók lesz automatikusan frissített tooRecovery szolgáltatások tárolók.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-113">Any remaining Backup vaults will be automatically upgraded tooRecovery Services vaults.</span></span>
>- <span data-ttu-id="5c7b6-114">Ön nem fogja tudni tooaccess a biztonsági mentési adatok hello a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-114">You won't be able tooaccess your backup data in hello classic portal.</span></span> <span data-ttu-id="5c7b6-115">Ehelyett használjon hello Azure portál tooaccess a biztonsági mentési adatok a Recovery Services-tárolók.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-115">Instead, use hello Azure portal tooaccess your backup data in Recovery Services vaults.</span></span>
>

<span data-ttu-id="5c7b6-116">toorestore adatok hello adatok helyreállítása varázsló használható a Microsoft Azure Recovery Services (MARS) ügynök hello.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-116">toorestore data, you use hello Recover Data wizard in hello Microsoft Azure Recovery Services (MARS) agent.</span></span> <span data-ttu-id="5c7b6-117">Adatok helyreállításakor lehetősége:</span><span class="sxs-lookup"><span data-stu-id="5c7b6-117">When you restore data, it is possible to:</span></span>

* <span data-ttu-id="5c7b6-118">Ugyanaz a számítógép melyik hello biztonsági másolatból visszaállítási adatok toohello vették.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-118">Restore data toohello same machine from which hello backups were taken.</span></span>
* <span data-ttu-id="5c7b6-119">Állítsa vissza az adatok tooan másik gépen.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-119">Restore data tooan alternate machine.</span></span>

<span data-ttu-id="5c7b6-120">2017. január, a Microsoft, amely egy előzetes frissítés toohello MARS agent.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-120">In January 2017, Microsoft released a Preview update toohello MARS agent.</span></span> <span data-ttu-id="5c7b6-121">Hibajavításokat tartalmaz, valamint a frissítés lehetővé teszi, hogy azonnali visszaállítása, amely lehetővé teszi toomount írható helyreállítási pont pillanatkép helyreállítási kötetként.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-121">Along with bug fixes, this update enables Instant Restore, which allows you toomount a writeable recovery point snapshot as a recovery volume.</span></span> <span data-ttu-id="5c7b6-122">Megismerheti a hello helyreállítási kötet, és másolja fájlok tooa helyi számítógép ezáltal visszaállítása fájlok majd.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-122">You can then explore hello recovery volume and copy files tooa local computer thereby selectively restoring files.</span></span>

> [!NOTE]
> <span data-ttu-id="5c7b6-123">Hello [2017. január Azure Backup frissítését](https://support.microsoft.com/en-us/help/3216528?preview) szükség, ha azt szeretné, hogy toouse azonnali visszaállítása toorestore adatokat.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-123">hello [January 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) is required if you want toouse Instant Restore toorestore data.</span></span> <span data-ttu-id="5c7b6-124">Hello biztonsági mentési adatok is a tárolók az hello támogatási cikkben szereplő területi beállításokhoz kell védeni.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-124">Also hello backup data must be protected in vaults in locales listed in hello support article.</span></span> <span data-ttu-id="5c7b6-125">Tekintse át a hello [2017. január Azure Backup frissítését](https://support.microsoft.com/en-us/help/3216528?preview) hello területi beállításokat, amelyek támogatják a azonnali visszaállítása a legfrissebb listáját.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-125">Consult hello [January 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) for hello latest list of locales that support Instant Restore.</span></span> <span data-ttu-id="5c7b6-126">Azonnali visszaállítás **nem** elérhetők az összes területi beállításokat.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-126">Instant Restore is **not** currently available in all locales.</span></span>
>

<span data-ttu-id="5c7b6-127">Azonnali visszaállítási érhető el a Recovery Services-tárolók az Azure-portálon hello használata és a biztonsági mentési tárolók hello a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-127">Instant Restore is available for use in Recovery Services vaults in hello Azure portal and Backup vaults in hello classic portal.</span></span> <span data-ttu-id="5c7b6-128">Ha azt szeretné, hogy azonnali visszaállítása toouse, hello MARS frissítés letöltése, és kövesse a hello eljárások, amelyek említik azonnali visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-128">If you want toouse Instant Restore, download hello MARS update, and follow hello procedures that mention Instant Restore.</span></span>


## <a name="use-instant-restore-toorecover-data-toohello-same-machine"></a><span data-ttu-id="5c7b6-129">Azonnali visszaállítása toorecover adatok toohello használja egyazon számítógépen</span><span class="sxs-lookup"><span data-stu-id="5c7b6-129">Use Instant Restore toorecover data toohello same machine</span></span>

<span data-ttu-id="5c7b6-130">Ha véletlenül törli a fájlt, és szeretné toorestore az egyazon számítógépen (mely hello a biztonsági mentés használatban van), a következő hello lépések toohello segít hello adatok helyreállítását.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-130">If you accidentally deleted a file and wish toorestore it toohello same machine (from which hello backup is taken), hello following steps will help you recover hello data.</span></span>

1. <span data-ttu-id="5c7b6-131">Nyissa meg hello **a Microsoft Azure Backup szolgáltatás** az illesztési.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-131">Open hello **Microsoft Azure Backup** snap in.</span></span> <span data-ttu-id="5c7b6-132">Ha nem tudja, ahová a hello beépülő modul telepítve van, a keresés hello számítógép vagy kiszolgáló **a Microsoft Azure Backup szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-132">If you don't know where hello snap in was installed, search hello computer or server for **Microsoft Azure Backup**.</span></span>

    <span data-ttu-id="5c7b6-133">egy asztali alkalmazás hello hello keresési eredmények jelenjenek meg.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-133">hello desktop app should appear in hello search results.</span></span>

2. <span data-ttu-id="5c7b6-134">Kattintson a **adatok helyreállítása** toostart hello varázsló.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-134">Click **Recover Data** toostart hello wizard.</span></span>

    ![Adatok helyreállítása](./media/backup-azure-restore-windows-server/recover.png)

3. <span data-ttu-id="5c7b6-136">A hello **bevezetés** ablaktáblában toorestore hello adatok toohello ugyanazon a kiszolgálón vagy a számítógépen, válassza ki a **ehhez a kiszolgálóhoz (`<server name>`)** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-136">On hello **Getting Started** pane, toorestore hello data toohello same server or computer, select **This server (`<server name>`)** and click **Next**.</span></span>

    ![Válassza ki a kiszolgáló beállítás toorestore hello adatok toohello egyazon számítógépen](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. <span data-ttu-id="5c7b6-138">A hello **válassza ki a helyreállítási mód** ablaktáblán válassza **egyes fájlok és mappák** , majd **tovább**.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-138">On hello **Select Recovery Mode** pane, choose **Individual files and folders** and then click **Next**.</span></span>

    ![Fájlok tallózása](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)

5. <span data-ttu-id="5c7b6-140">A hello **mennyiség kiválasztása és a dátum** ablaktáblában hello fájlok és/vagy mappákról, amelyeket toorestore tartalmazó select hello kötetet.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-140">On hello **Select Volume and Date** pane, select hello volume that contains hello files and/or folders you want toorestore.</span></span>

    <span data-ttu-id="5c7b6-141">Hello naptárban válasszon ki egy helyreállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-141">On hello calendar, select a recovery point.</span></span> <span data-ttu-id="5c7b6-142">Bármelyik helyreállítási pontra állíthatja vissza időben.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-142">You can restore from any recovery point in time.</span></span> <span data-ttu-id="5c7b6-143">A dátumok **félkövér** jelző hello legalább egy helyreállítási pont rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-143">Dates in **bold** indicate hello availability of at least one recovery point.</span></span> <span data-ttu-id="5c7b6-144">Miután egy dátumot, ha több helyreállítási pont nem érhető el, hello adott helyreállítási pontot választhat hello **idő** legördülő menü.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-144">Once you select a date, if multiple recovery points are available, choose hello specific recovery point from hello **Time** drop-down menu.</span></span>

    ![Kötet és a dátum](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. <span data-ttu-id="5c7b6-146">Miután kiválasztotta a hello helyreállítási pont toorestore, kattintson a **csatlakoztatási**.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-146">Once you have chosen hello recovery point toorestore, click **Mount**.</span></span>

    <span data-ttu-id="5c7b6-147">Azure biztonsági mentés hello helyi helyreállítási pontot csatlakoztatja, és egy helyreállítási kötet használja.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-147">Azure Backup mounts hello local recovery point, and uses it as a recovery volume.</span></span>

7. <span data-ttu-id="5c7b6-148">A hello **tallózással keresse meg és a fájlok helyreállítása** ablaktáblában kattintson **Tallózás** tooopen Windows Explorer és hello fájlok és mappák.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-148">On hello **Browse and Recover Files** pane, click **Browse** tooopen Windows Explorer and find hello files and folders you want.</span></span>

    ![Helyreállítási beállítások](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)


8. <span data-ttu-id="5c7b6-150">A Windows Intézőben, hello fájlok másolása és/vagy mappák toorestore szeretné, és illessze be a tooany hely helyi toohello kiszolgálón vagy számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-150">In Windows Explorer, copy hello files and/or folders you want toorestore and paste them tooany location local toohello server or computer.</span></span> <span data-ttu-id="5c7b6-151">Nyissa meg vagy adatfolyam-fájlok hello közvetlenül hello helyreállítási kötet, és ellenőrizze a megfelelő verzió helyreállítás hello.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-151">You can open or stream hello files directly from hello recovery volume and verify hello correct versions are recovered.</span></span>

    ![Másolja és illessze be a fájlok és mappák csatlakoztatott kötet toolocal helyről](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)

9. <span data-ttu-id="5c7b6-153">Ha elkészült a visszaállítási hello fájlok és/vagy mappák hello **tallózással keresse meg és a helyreállítási fájlokat** ablaktáblán kattintson a **leválasztási**.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-153">When you are finished restoring hello files and/or folders, on hello **Browse and Recovery Files** pane, click **Unmount**.</span></span> <span data-ttu-id="5c7b6-154">Kattintson a **Igen** tooconfirm, amelyet az toounmount hello kötet.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-154">Then click **Yes** tooconfirm that you want toounmount hello volume.</span></span>

    ![Válassza le a hello kötet, és erősítse meg](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > <span data-ttu-id="5c7b6-156">Ha nem kattint az leválasztási, hello helyreállítási kötet marad csatlakoztatott hello idő, amikor csatlakoztatva lett a hat órán át.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-156">If you do not click Unmount, hello Recovery Volume will remain mounted for six hours from hello time when it was mounted.</span></span> <span data-ttu-id="5c7b6-157">Nincs biztonsági mentési műveletek fog futni, amíg hello kötet csatlakoztatva van.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-157">No backup operations will run while hello volume is mounted.</span></span> <span data-ttu-id="5c7b6-158">Minden ütemezett biztonsági mentési művelet toorun hello időszakban, amikor hello kötet csatlakoztatva van, miután hello helyreállítási kötet nem fog futni.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-158">Any backup operation scheduled toorun during hello time when hello volume is mounted, will run after hello recovery volume is unmounted.</span></span>
    >


## <a name="recover-data-toohello-same-machine"></a><span data-ttu-id="5c7b6-159">Adatok toohello helyreállítása egyazon számítógépen</span><span class="sxs-lookup"><span data-stu-id="5c7b6-159">Recover data toohello same machine</span></span>
<span data-ttu-id="5c7b6-160">Ha véletlenül törli a fájlt, és szeretné toorestore az egyazon számítógépen (mely hello a biztonsági mentés használatban van), a következő hello lépések toohello segít hello adatok helyreállítását.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-160">If you accidentally deleted a file and wish toorestore it toohello same machine (from which hello backup is taken), hello following steps will help you recover hello data.</span></span>

1. <span data-ttu-id="5c7b6-161">Nyissa meg hello **a Microsoft Azure Backup szolgáltatás** az illesztési.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-161">Open hello **Microsoft Azure Backup** snap in.</span></span>
2. <span data-ttu-id="5c7b6-162">Kattintson a **adatok helyreállítása** tooinitiate hello munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-162">Click **Recover Data** tooinitiate hello workflow.</span></span>

    ![Adatok helyreállítása](./media/backup-azure-restore-windows-server-classic/recover.png)
3. <span data-ttu-id="5c7b6-164">Jelölje be hello  **ehhez a kiszolgálóhoz (*yourmachinename*) ** beállítás toorestore hello fájl biztonsági másolatát a hello azonos gép.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-164">Select hello **This server (*yourmachinename*)** option toorestore hello backed up file on hello same machine.</span></span>

    ![Ugyanaz a gép](./media/backup-azure-restore-windows-server-classic/samemachine.png)
4. <span data-ttu-id="5c7b6-166">Válassza ki a túl**fájlok tallózása** vagy **fájlok keresése**.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-166">Choose too**Browse for files** or **Search for files**.</span></span>

    <span data-ttu-id="5c7b6-167">Hagyja hello alapértelmezett beállítást, ha azt tervezi, hogy toorestore elérési úton található egy vagy több fájlt.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-167">Leave hello default option if you plan toorestore one or more files whose path is known.</span></span> <span data-ttu-id="5c7b6-168">Ha hello mappaszerkezet kapcsolatban kérdése, de szeretné tenni, toosearch egy fájlhoz, válassza ki a hello **fájlok keresése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-168">If you are not sure about hello folder structure but would like toosearch for a file, pick hello **Search for files** option.</span></span> <span data-ttu-id="5c7b6-169">A jelen szakasz hello célra azt folytatja hello alapértelmezett beállítás.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-169">For hello purpose of this section, we will proceed with hello default option.</span></span>

    ![Fájlok tallózása](./media/backup-azure-restore-windows-server-classic/browseandsearch.png)
5. <span data-ttu-id="5c7b6-171">Válassza ki, amelyből toorestore hello fájlt kívánja hello kötetet.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-171">Select hello volume from which you wish toorestore hello file.</span></span>

    <span data-ttu-id="5c7b6-172">Minden olyan pont állíthatja vissza időben.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-172">You can restore from any point in time.</span></span> <span data-ttu-id="5c7b6-173">Megjelenő dátumok **félkövér** hello havinaptár-vezérlőben meg hello visszaállítási pont rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-173">Dates which appear in **bold** in hello calendar control indicate hello availability of a restore point.</span></span> <span data-ttu-id="5c7b6-174">A kijelölt dátum alapján a biztonsági mentés ütemezése (és a biztonsági mentés sikeres hello), kiválaszthat egy pontot időben a hello **idő** legördülő listán.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-174">Once a date is selected, based on your backup schedule (and hello success of a backup operation), you can select a point in time from hello **Time** drop down.</span></span>

    ![Kötet és a dátum](./media/backup-azure-restore-windows-server-classic/volanddate.png)
6. <span data-ttu-id="5c7b6-176">Válassza ki a hello elemek toorecover.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-176">Select hello items toorecover.</span></span> <span data-ttu-id="5c7b6-177">Többszörös kiválasztási mappák és fájlok toorestore kívánja is.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-177">You can multi-select folders/files you wish toorestore.</span></span>

    ![Fájlok kiválasztása](./media/backup-azure-restore-windows-server-classic/selectfiles.png)
7. <span data-ttu-id="5c7b6-179">Adjon meg hello helyreállítási paramétereket.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-179">Specify hello recovery parameters.</span></span>

    ![Helyreállítási beállítások](./media/backup-azure-restore-windows-server-classic/recoveroptions.png)

   * <span data-ttu-id="5c7b6-181">Lehetősége van toohello eredeti helyükre (mely hello a fájl vagy mappa volna felülírja) vagy hello tooanother helyére állítja vissza egyazon számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-181">You have an option of restoring toohello original location (in which hello file/folder would be overwritten) or tooanother location in hello same machine.</span></span>
   * <span data-ttu-id="5c7b6-182">Ha hello fájl vagy mappa toorestore szerepel hello célhelyet kívánja, létrehozhat másolatok (hello két verziója ugyanazt a fájlt), írja felül a hello célhelyet hello fájlokat vagy hello fájlok hello cél létező hello helyreállításának kihagyása.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-182">If hello file/folder you wish toorestore exists in hello target location, you can create copies (two versions of hello same file), overwrite hello files in hello target location, or skip hello recovery of hello files which exist in hello target.</span></span>
   * <span data-ttu-id="5c7b6-183">Erősen ajánlott, hogy hagyja bejelölve a helyreállítás alatt álló hello fájlokat hello ACL-ek visszaállításának hello alapértelmezett beállítás.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-183">It is highly recommended that you leave hello default option of restoring hello ACLs on hello files which are being recovered.</span></span>
8. <span data-ttu-id="5c7b6-184">Amikor a bemeneti adatok érhetők el, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-184">Once these inputs are provided, click **Next**.</span></span> <span data-ttu-id="5c7b6-185">hello helyreállítási munkafolyamat, amely visszaállítja hello fájlok toothis gépet, megkezdődik.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-185">hello recovery workflow, which restores hello files toothis machine, will begin.</span></span>

## <a name="recover-tooan-alternate-machine"></a><span data-ttu-id="5c7b6-186">Tooan alternatív gép helyreállítása</span><span class="sxs-lookup"><span data-stu-id="5c7b6-186">Recover tooan alternate machine</span></span>
<span data-ttu-id="5c7b6-187">A teljes kiszolgáló nem vesztek el, ha továbbra is helyreállítható adatok Azure biztonsági mentés tooa másik gépre.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-187">If your entire server is lost, you can still recover data from Azure Backup tooa different machine.</span></span> <span data-ttu-id="5c7b6-188">a lépéseket követve hello hello munkafolyamat mutatják be.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-188">hello following steps illustrate hello workflow.</span></span>  

<span data-ttu-id="5c7b6-189">hello terminológia a következő lépéseket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="5c7b6-189">hello terminology used in these steps includes:</span></span>

* <span data-ttu-id="5c7b6-190">*Forrásgép* – hello eredeti számítógép melyik hello biztonsági mentésből készült, és amelyen jelenleg nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-190">*Source machine* – hello original machine from which hello backup was taken and which is currently unavailable.</span></span>
* <span data-ttu-id="5c7b6-191">*Célszámítógép* – hello gép toowhich hello adatok helyreállítása folyamatban.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-191">*Target machine* – hello machine toowhich hello data is being recovered.</span></span>
* <span data-ttu-id="5c7b6-192">*Minta tároló* – hello biztonsági mentési tároló toowhich hello *forrásgép* és *célgépen* van regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-192">*Sample vault* – hello Backup vault toowhich hello *Source machine* and *Target machine* are registered.</span></span> <br/>

> [!NOTE]
> <span data-ttu-id="5c7b6-193">Gépről készített biztonsági másolatok nem állítható vissza egy korábbi operációs rendszer hello szolgáltatást futtató gépen.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-193">Backups taken from a machine cannot be restored on a machine which is running an earlier version of hello operating system.</span></span> <span data-ttu-id="5c7b6-194">Például ha biztonsági mentést készít a Windows 7 gépről, akkor visszaállítása végezhető el a Windows 8 vagy újabb gép.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-194">For example, if backups are taken from a Windows 7 machine, it can be restored on a Windows 8 or above machine.</span></span> <span data-ttu-id="5c7b6-195">Azonban hello viszont nem rendelkezik igaz.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-195">However, hello vice-versa does not hold true.</span></span>
>
>

1. <span data-ttu-id="5c7b6-196">Nyissa meg hello **a Microsoft Azure Backup szolgáltatás** az illesztési hello *célgépen*.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-196">Open hello **Microsoft Azure Backup** snap in on hello *Target machine*.</span></span>
2. <span data-ttu-id="5c7b6-197">Győződjön meg arról, hogy hello *célgépen* és hello *forrásgép* regisztrált toohello vannak azonos mentési tároló.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-197">Ensure that hello *Target machine* and hello *Source machine* are registered toohello same backup vault.</span></span>
3. <span data-ttu-id="5c7b6-198">Kattintson a **adatok helyreállítása** tooinitiate hello munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-198">Click **Recover Data** tooinitiate hello workflow.</span></span>

    ![Adatok helyreállítása](./media/backup-azure-restore-windows-server-classic/recover.png)
4. <span data-ttu-id="5c7b6-200">Válassza ki **egy másik kiszolgáló**</span><span class="sxs-lookup"><span data-stu-id="5c7b6-200">Select **Another server**</span></span>

    ![Egy másik kiszolgáló](./media/backup-azure-restore-windows-server-classic/anotherserver.png)
5. <span data-ttu-id="5c7b6-202">Adja meg a hello tárolói hitelesítő adatok fájlját, amely megfelel a toohello *minta tároló*.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-202">Provide hello vault credential file that corresponds toohello *Sample vault*.</span></span> <span data-ttu-id="5c7b6-203">Ha a hello tárolói hitelesítő adatok fájlját az érvénytelen (vagy lejárt) le új tárolói hitelesítő adatok fájlt hello *minta tároló* a klasszikus Azure portálon hello.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-203">If hello vault credential file is invalid (or expired) download a new vault credential file from hello *Sample vault* in hello Azure classic portal.</span></span> <span data-ttu-id="5c7b6-204">Hello tárolói hitelesítő adatok fájlját valósul meg, miután hello mentési tároló elleni hello tárolói hitelesítő adatok fájlját jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-204">Once hello vault credential file is provided, hello backup vault against hello vault credential file is displayed.</span></span>
6. <span data-ttu-id="5c7b6-205">Jelölje be hello *forrásgép* megjelenített gépek hello listája.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-205">Select hello *Source machine* from hello list of displayed machines.</span></span>

    ![Számítógépek listája](./media/backup-azure-restore-windows-server-classic/machinelist.png)
7. <span data-ttu-id="5c7b6-207">Válassza ki bármelyik hello **fájlok keresése** vagy **fájlok tallózása** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-207">Select either hello **Search for files** or **Browse for files** option.</span></span> <span data-ttu-id="5c7b6-208">A jelen szakasz hello célra használjuk hello **fájlok keresése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-208">For hello purpose of this section, we will use hello **Search for files** option.</span></span>

    ![Keresés](./media/backup-azure-restore-windows-server-classic/search.png)
8. <span data-ttu-id="5c7b6-210">Válassza ki a hello mennyiség és a dátum a következő képernyőn hello.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-210">Select hello volume and date in hello next screen.</span></span> <span data-ttu-id="5c7b6-211">Keresési hello fájl/mappa neve toorestore keresi.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-211">Search for hello folder/file name you want toorestore.</span></span>

    ![Keresési elemek](./media/backup-azure-restore-windows-server-classic/searchitems.png)
9. <span data-ttu-id="5c7b6-213">Ha a hello fájlok toobe vissza kell hello hely kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-213">Select hello location where hello files need toobe restored.</span></span>

    ![Hely visszaállítása](./media/backup-azure-restore-windows-server-classic/restorelocation.png)
10. <span data-ttu-id="5c7b6-215">Adja meg, amely során hello titkosítási jelszó *forrás gép* regisztrációs túl*minta tároló*.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-215">Provide hello encryption passphrase that was provided during *Source machine’s* registration too*Sample vault*.</span></span>

    ![Titkosítás](./media/backup-azure-restore-windows-server-classic/encryption.png)
11. <span data-ttu-id="5c7b6-217">Amikor hello bemeneti valósul meg, kattintson a **helyreállítása**, mely eseményindítók hello helyreállítására irányuló biztonsági mentés megadott fájlok toohello célt hello.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-217">Once hello input is provided, click **Recover**, which triggers hello restore of hello backed up files toohello destination provided.</span></span>

## <a name="use-instant-restore-toorestore-data-tooan-alternate-machine"></a><span data-ttu-id="5c7b6-218">Azonnali visszaállítása toorestore adatok tooan másik számítógépen</span><span class="sxs-lookup"><span data-stu-id="5c7b6-218">Use Instant Restore toorestore data tooan alternate machine</span></span>
<span data-ttu-id="5c7b6-219">A teljes kiszolgáló nem vesztek el, ha továbbra is helyreállítható adatok Azure biztonsági mentés tooa másik gépre.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-219">If your entire server is lost, you can still recover data from Azure Backup tooa different machine.</span></span> <span data-ttu-id="5c7b6-220">a lépéseket követve hello hello munkafolyamat mutatják be.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-220">hello following steps illustrate hello workflow.</span></span>

<span data-ttu-id="5c7b6-221">hello terminológia a következő lépéseket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="5c7b6-221">hello terminology used in these steps includes:</span></span>

* <span data-ttu-id="5c7b6-222">*Forrásgép* – hello eredeti számítógép melyik hello biztonsági mentésből készült, és amelyen jelenleg nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-222">*Source machine* – hello original machine from which hello backup was taken and which is currently unavailable.</span></span>
* <span data-ttu-id="5c7b6-223">*Célszámítógép* – hello gép toowhich hello adatok helyreállítása folyamatban.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-223">*Target machine* – hello machine toowhich hello data is being recovered.</span></span>
* <span data-ttu-id="5c7b6-224">*Minta tároló* – hello Recovery Services-tároló toowhich hello *forrásgép* és *célgépen* van regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-224">*Sample vault* – hello Recovery Services vault toowhich hello *Source machine* and *Target machine* are registered.</span></span> <br/>

> [!NOTE]
> <span data-ttu-id="5c7b6-225">Biztonsági mentések nem lehet visszaállított tooa hello operációs rendszer korábbi verzióját futtató célszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-225">Backups can't be restored tooa target machine running an earlier version of hello operating system.</span></span> <span data-ttu-id="5c7b6-226">Például egy készült biztonsági másolatok a Windows 7 számítógép lehet vissza a Windows 8-as vagy újabb verzióját, számítógép.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-226">For example, a backup taken from a Windows 7 computer can be restored on a Windows 8, or later, computer.</span></span> <span data-ttu-id="5c7b6-227">Egy készült biztonsági másolatok a Windows 8 számítógépről nem lehet visszaállított tooa Windows 7 számítógép.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-227">A backup taken from a Windows 8 computer cannot be restored tooa Windows 7 computer.</span></span>
>
>

1. <span data-ttu-id="5c7b6-228">Nyissa meg hello **a Microsoft Azure Backup szolgáltatás** az illesztési hello *célgépen*.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-228">Open hello **Microsoft Azure Backup** snap in on hello *Target machine*.</span></span>

2. <span data-ttu-id="5c7b6-229">Győződjön meg arról hello *célgépen* és hello *forrásgép* azonos Recovery Services-tároló regisztrált toohello vannak.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-229">Ensure hello *Target machine* and hello *Source machine* are registered toohello same Recovery Services vault.</span></span>

3. <span data-ttu-id="5c7b6-230">Kattintson a **adatok helyreállítása** tooopen hello **adatok helyreállítása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-230">Click **Recover Data** tooopen hello **Recover Data wizard**.</span></span>

    ![Adatok helyreállítása](./media/backup-azure-restore-windows-server/recover.png)

4. <span data-ttu-id="5c7b6-232">A hello **bevezetés** ablaktáblán válassza előbb **egy másik kiszolgáló**</span><span class="sxs-lookup"><span data-stu-id="5c7b6-232">On hello **Getting Started** pane, select **Another server**</span></span>

    ![Egy másik kiszolgáló](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. <span data-ttu-id="5c7b6-234">Adja meg a hello tárolói hitelesítő adatok fájlját, amely megfelel a toohello *minta tároló*, és kattintson a **tovább**.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-234">Provide hello vault credential file that corresponds toohello *Sample vault*, and click **Next**.</span></span>

    <span data-ttu-id="5c7b6-235">Ha hello tárolói hitelesítő adatok fájlját érvénytelen (vagy lejárt), egy új tárolói hitelesítő adatok fájlját letöltését hello *minta tároló* a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-235">If hello vault credential file is invalid (or expired), download a new vault credential file from hello *Sample vault* in hello Azure portal.</span></span> <span data-ttu-id="5c7b6-236">Miután megadta a egy érvényes tároló hitelesítő adatait, megfelelő mentési tároló hello hello neve jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-236">Once you provide a valid vault credential, hello name of hello corresponding Backup Vault appears.</span></span>

6. <span data-ttu-id="5c7b6-237">A hello **biztonsági mentés kiszolgáló kiválasztása** ablaktáblában válassza hello *forrásgép* megjelenített gépek hello listából, és adja meg a hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-237">On hello **Select Backup Server** pane, select hello *Source machine* from hello list of displayed machines and provide hello passphrase.</span></span> <span data-ttu-id="5c7b6-238">Ezután kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-238">Then click **Next**.</span></span>

    ![Számítógépek listája](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. <span data-ttu-id="5c7b6-240">A hello **válassza ki a helyreállítási mód** ablaktáblán válassza ki az **egyes fájlok és mappák** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-240">On hello **Select Recovery Mode** pane, select **Individual files and folders** and click **Next**.</span></span>

    ![Keresés](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. <span data-ttu-id="5c7b6-242">A hello **mennyiség kiválasztása és a dátum** ablaktáblában hello fájlok és/vagy mappákról, amelyeket toorestore tartalmazó select hello kötetet.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-242">On hello **Select Volume and Date** pane, select hello volume that contains hello files and/or folders you want toorestore.</span></span>

    <span data-ttu-id="5c7b6-243">Hello naptárban válasszon ki egy helyreállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-243">On hello calendar, select a recovery point.</span></span> <span data-ttu-id="5c7b6-244">Bármelyik helyreállítási pontra állíthatja vissza időben.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-244">You can restore from any recovery point in time.</span></span> <span data-ttu-id="5c7b6-245">A dátumok **félkövér** jelző hello legalább egy helyreállítási pont rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-245">Dates in **bold** indicate hello availability of at least one recovery point.</span></span> <span data-ttu-id="5c7b6-246">Miután egy dátumot, ha több helyreállítási pont nem érhető el, hello adott helyreállítási pontot választhat hello **idő** legördülő menü.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-246">Once you select a date, if multiple recovery points are available, choose hello specific recovery point from hello **Time** drop-down menu.</span></span>

    ![Keresési elemek](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. <span data-ttu-id="5c7b6-248">Kattintson a **csatlakoztatási** toolocally csatlakoztatási hello helyreállítási pont a helyreállítási kötetként a *célgépen*.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-248">Click **Mount** toolocally mount hello recovery point as a recovery volume on your *Target machine*.</span></span>

10. <span data-ttu-id="5c7b6-249">A hello **tallózással keresse meg és a fájlok helyreállítása** ablaktáblában kattintson **Tallózás** tooopen Windows Explorer és hello fájlok és mappák.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-249">On hello **Browse and Recover Files** pane, click **Browse** tooopen Windows Explorer and find hello files and folders you want.</span></span>

    ![Titkosítás](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. <span data-ttu-id="5c7b6-251">A Windows Intézőben hello fájlok és/vagy mappák másolása hello helyreállítási kötet, és illessze be a tooyour *célgépen* helyét.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-251">In Windows Explorer, copy hello files and/or folders from hello recovery volume and paste them tooyour *Target machine* location.</span></span> <span data-ttu-id="5c7b6-252">Nyissa meg vagy adatfolyam-fájlok hello közvetlenül hello helyreállítási kötet, és ellenőrizze a megfelelő verzió helyreállítás hello.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-252">You can open or stream hello files directly from hello recovery volume and verify hello correct versions are recovered.</span></span>

    ![Titkosítás](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. <span data-ttu-id="5c7b6-254">Ha elkészült a visszaállítási hello fájlok és/vagy mappák hello **tallózással keresse meg és a helyreállítási fájlokat** ablaktáblán kattintson a **leválasztási**.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-254">When you are finished restoring hello files and/or folders, on hello **Browse and Recovery Files** pane, click **Unmount**.</span></span> <span data-ttu-id="5c7b6-255">Kattintson a **Igen** tooconfirm, amelyet az toounmount hello kötet.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-255">Then click **Yes** tooconfirm that you want toounmount hello volume.</span></span>

    ![Titkosítás](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > <span data-ttu-id="5c7b6-257">Ha nem kattint az leválasztási, hello helyreállítási kötet marad csatlakoztatott hello idő, amikor csatlakoztatva lett a hat órán át.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-257">If you do not click Unmount, hello Recovery Volume will remain mounted for six hours from hello time when it was mounted.</span></span> <span data-ttu-id="5c7b6-258">Nincs biztonsági mentési műveletek fog futni, amíg hello kötet csatlakoztatva van.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-258">No backup operations will run while hello volume is mounted.</span></span> <span data-ttu-id="5c7b6-259">Minden ütemezett biztonsági mentési művelet toorun hello időszakban, amikor hello kötet csatlakoztatva van, miután hello helyreállítási kötet nem fog futni.</span><span class="sxs-lookup"><span data-stu-id="5c7b6-259">Any backup operation scheduled toorun during hello time when hello volume is mounted, will run after hello recovery volume is unmounted.</span></span>
    >


## <a name="next-steps"></a><span data-ttu-id="5c7b6-260">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5c7b6-260">Next steps</span></span>
* [<span data-ttu-id="5c7b6-261">Az Azure biztonsági mentési – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="5c7b6-261">Azure Backup FAQ</span></span>](backup-azure-backup-faq.md)
* <span data-ttu-id="5c7b6-262">A Microsoft hello [Azure biztonsági mentés fórum](http://go.microsoft.com/fwlink/p/?LinkId=290933).</span><span class="sxs-lookup"><span data-stu-id="5c7b6-262">Visit hello [Azure Backup Forum](http://go.microsoft.com/fwlink/p/?LinkId=290933).</span></span>

## <a name="learn-more"></a><span data-ttu-id="5c7b6-263">Részletek</span><span class="sxs-lookup"><span data-stu-id="5c7b6-263">Learn more</span></span>
* [<span data-ttu-id="5c7b6-264">Az Azure biztonsági mentés áttekintése</span><span class="sxs-lookup"><span data-stu-id="5c7b6-264">Azure Backup Overview</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=222425)
* [<span data-ttu-id="5c7b6-265">Az Azure virtuális gépek biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="5c7b6-265">Backup Azure virtual machines</span></span>](backup-azure-vms-introduction.md)
* [<span data-ttu-id="5c7b6-266">Fel Microsoft-munkaterhelések biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="5c7b6-266">Backup up Microsoft workloads</span></span>](backup-azure-dpm-introduction.md)
