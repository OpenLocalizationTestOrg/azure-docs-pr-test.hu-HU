---
title: "Hozzon létre, és a biztonsági másolat visszaállításával a BizTalk szolgáltatások |} Microsoft Docs"
description: "BizTalk szolgáltatások részét képező, biztonsági mentését és helyreállítását. Megtudhatja, hogyan hozzon létre, és a biztonsági másolat visszaállításával, és határozza meg, mi lekérdezi a biztonsági mentés. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 59f91173-4683-48df-abd5-41262bfce6df
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: c55d1ab124441c42101b4ad60924a9ea28231408
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="biztalk-services-backup-and-restore"></a><span data-ttu-id="0b110-105">BizTalk Services: Biztonsági mentés és visszaállítás</span><span class="sxs-lookup"><span data-stu-id="0b110-105">BizTalk Services: Backup and Restore</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

<span data-ttu-id="0b110-106">Azure BizTalk-szolgáltatásokhoz kínálja biztonsági mentését és helyreállítását.</span><span class="sxs-lookup"><span data-stu-id="0b110-106">Azure BizTalk Services includes Backup and Restore capabilities.</span></span> <span data-ttu-id="0b110-107">Ez a témakör ismerteti, hogyan biztonsági mentése és visszaállítása a BizTalk-szolgáltatásokat a klasszikus Azure portál segítségével.</span><span class="sxs-lookup"><span data-stu-id="0b110-107">This topic describes how to backup and restore BizTalk Services using the Azure classic portal.</span></span>

<span data-ttu-id="0b110-108">BizTalk szolgáltatások használatával is készíthet a [BizTalk szolgáltatás REST API](http://go.microsoft.com/fwlink/p/?LinkID=325584).</span><span class="sxs-lookup"><span data-stu-id="0b110-108">You can also back up BizTalk Services using the [BizTalk Services REST API](http://go.microsoft.com/fwlink/p/?LinkID=325584).</span></span> 

> [!NOTE]
> <span data-ttu-id="0b110-109">Hibrid kapcsolatok nem készül biztonsági mentés, a kiadástól függetlenül.</span><span class="sxs-lookup"><span data-stu-id="0b110-109">Hybrid Connections are NOT backed up, regardless of the Edition.</span></span> <span data-ttu-id="0b110-110">A hibrid kapcsolatok újra létre kell hoznia.</span><span class="sxs-lookup"><span data-stu-id="0b110-110">You must recreate your hybrid connections.</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="0b110-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="0b110-111">Before you Begin</span></span>
* <span data-ttu-id="0b110-112">Biztonsági mentés és visszaállítás nem lehet elérhető összes kiadása esetén.</span><span class="sxs-lookup"><span data-stu-id="0b110-112">Backup and Restore may not be available for all editions.</span></span> <span data-ttu-id="0b110-113">Lásd: [BizTalk szolgáltatások: kiadások diagram](biztalk-editions-feature-chart.md).</span><span class="sxs-lookup"><span data-stu-id="0b110-113">See [BizTalk Services: Editions Chart](biztalk-editions-feature-chart.md).</span></span>
* <span data-ttu-id="0b110-114">A klasszikus Azure portált használja, hozzon létre egy igény szerinti biztonsági mentést, vagy hozzon létre ütemezett biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="0b110-114">Using the Azure classic portal, you can create an On Demand backup or create a scheduled backup.</span></span> 
* <span data-ttu-id="0b110-115">Biztonsági mentési tartalom ugyanaz a BizTalk szolgáltatás, vagy új BizTalk szolgáltatás állíthatók vissza.</span><span class="sxs-lookup"><span data-stu-id="0b110-115">Backup content can be restored to the same BizTalk Service or to a new BizTalk Service.</span></span> <span data-ttu-id="0b110-116">A BizTalk szolgáltatás az azonos név használata visszaállításához a meglévő BizTalk szolgáltatás törölni kell, és a név elérhetőnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="0b110-116">To restore the BizTalk Service using the same name, the existing BizTalk Service must be deleted and the name must be available.</span></span> <span data-ttu-id="0b110-117">Ha töröl egy BizTalk szolgáltatás, annak elérhetőnek lennie az olyan hosszabb időt is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="0b110-117">After you delete a BizTalk Service, it can take longer time than wanted for the same name to be available.</span></span> <span data-ttu-id="0b110-118">Ha nem várja meg a néven elérhető legyen, majd állítsa vissza egy új BizTalk szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="0b110-118">If you cannot wait for the same name to be available, then restore to a new BizTalk Service.</span></span>
* <span data-ttu-id="0b110-119">BizTalk szolgáltatások azonos kiadást vagy újabb kiadását vissza tudja állítani.</span><span class="sxs-lookup"><span data-stu-id="0b110-119">BizTalk Services can be restored to the same edition or a higher edition.</span></span> <span data-ttu-id="0b110-120">BizTalk szolgáltatások visszaállítása alacsonyabb verzióra, ha a biztonsági mentés készült, nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="0b110-120">Restoring BizTalk Services to a lower edition, from when the backup was taken, is not supported.</span></span>
  
    <span data-ttu-id="0b110-121">Például egy alapszintű kiadására biztonsági másolat visszaállítása végezhető el verziót prémium kiadásúra.</span><span class="sxs-lookup"><span data-stu-id="0b110-121">For example, a backup using the Basic Edition can be restored to the Premium Edition.</span></span> <span data-ttu-id="0b110-122">A prémium kiadással biztonsági a Standard Edition nem állítható vissza.</span><span class="sxs-lookup"><span data-stu-id="0b110-122">A backup using the Premium Edition cannot be restored to the Standard Edition.</span></span>
* <span data-ttu-id="0b110-123">EDI-ellenőrző számainak folytonossága vezérlő számok készül biztonsági másolat.</span><span class="sxs-lookup"><span data-stu-id="0b110-123">The EDI Control numbers are backed up to maintain continuity of the control numbers.</span></span> <span data-ttu-id="0b110-124">Üzenetek feldolgozása után utolsó biztonsági mentés, ha a biztonsági mentési tartalom visszaállítása ismétlődő vezérlő számok okozhat.</span><span class="sxs-lookup"><span data-stu-id="0b110-124">If messages are processed after the last backup, restoring this backup content can cause duplicate control numbers.</span></span>
* <span data-ttu-id="0b110-125">Ha egy kötegelt aktív üzenetek, dolgozza fel a kötegelt **előtt** biztonsági másolat készítése.</span><span class="sxs-lookup"><span data-stu-id="0b110-125">If a batch has active messages, process the batch **before** running a backup.</span></span> <span data-ttu-id="0b110-126">(A szükséges vagy ütemezett) a biztonsági másolat létrehozásakor a kötegek üzenetek soha nem kerülnek.</span><span class="sxs-lookup"><span data-stu-id="0b110-126">When creating a backup (as needed or scheduled), messages in batches are never stored.</span></span> 
  
    <span data-ttu-id="0b110-127">**Biztonsági másolatot készít aktív üzeneteket kötegekben, ha ezek az üzenetek nem készül biztonsági mentés, és ezért elvesznek.**</span><span class="sxs-lookup"><span data-stu-id="0b110-127">**If a backup is taken with active messages in a batch, these messages are not backed up and are therefore lost.**</span></span>
* <span data-ttu-id="0b110-128">Választható lehetőség: A BizTalk szolgáltatások portál leállítása felügyeleti műveleteket sem.</span><span class="sxs-lookup"><span data-stu-id="0b110-128">Optional: In the BizTalk Services Portal, stop any management operations.</span></span>

## <a name="create-a-backup"></a><span data-ttu-id="0b110-129">Biztonsági mentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="0b110-129">Create a backup</span></span>
<span data-ttu-id="0b110-130">A biztonsági mentés akkor van szükség, tetszőleges időpontban, és teljesen vezérli akkor.</span><span class="sxs-lookup"><span data-stu-id="0b110-130">A backup can be taken at any time and is completely controlled by you.</span></span> <span data-ttu-id="0b110-131">Ez a rész felsorolja a lépésekkel hozza létre a biztonsági mentéseket a klasszikus Azure portálra, beleértve:</span><span class="sxs-lookup"><span data-stu-id="0b110-131">This section lists the steps to create backups using the Azure classic portal, including:</span></span>

[<span data-ttu-id="0b110-132">Az igény szerinti biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="0b110-132">On Demand backup</span></span>](#backupnow)

[<span data-ttu-id="0b110-133">A biztonsági mentés ütemezése</span><span class="sxs-lookup"><span data-stu-id="0b110-133">Schedule a backup</span></span>](#backupschedule)

#### <span data-ttu-id="0b110-134"><a name="backupnow"></a>Az igény szerinti biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="0b110-134"><a name="backupnow"></a>On Demand backup</span></span>
1. <span data-ttu-id="0b110-135">A klasszikus Azure portálon, válassza ki a **BizTalk szolgáltatások**, majd válassza ki a kívánt BizTalk szolgáltatás biztonsági mentését.</span><span class="sxs-lookup"><span data-stu-id="0b110-135">In the Azure classic portal, select **BizTalk Services**, and then select the BizTalk Service you want to backup.</span></span>
2. <span data-ttu-id="0b110-136">Az a **irányítópult** lapon jelölje be **készítsen biztonsági másolatot** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="0b110-136">In the **Dashboard** tab, select **Back up** at the bottom of the page.</span></span>
3. <span data-ttu-id="0b110-137">Adja meg a biztonsági másolat neve.</span><span class="sxs-lookup"><span data-stu-id="0b110-137">Enter a backup name.</span></span> <span data-ttu-id="0b110-138">Adja meg például *myBizTalkService*BU*dátum*.</span><span class="sxs-lookup"><span data-stu-id="0b110-138">For example, enter *myBizTalkService*BU*Date*.</span></span>
4. <span data-ttu-id="0b110-139">Válassza ki a blob storage-fiók, és válassza ki a jelet a biztonsági másolat segédprogram elindításához.</span><span class="sxs-lookup"><span data-stu-id="0b110-139">Choose a blob storage account and select the checkmark to start the backup.</span></span>

<span data-ttu-id="0b110-140">Miután befejeződött a biztonsági másolat, akkor adja meg a biztonsági mentési nevű tárolója a tárfiók jön létre.</span><span class="sxs-lookup"><span data-stu-id="0b110-140">Once the backup completes, a container with the backup name you enter is created in the storage account.</span></span> <span data-ttu-id="0b110-141">Ebben a tárolóban a BizTalk szolgáltatás biztonsági mentési konfigurációját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0b110-141">This container contains your BizTalk Service backup configuration.</span></span>

#### <span data-ttu-id="0b110-142"><a name="backupschedule"></a>A biztonsági mentés ütemezése</span><span class="sxs-lookup"><span data-stu-id="0b110-142"><a name="backupschedule"></a>Schedule a backup</span></span>
1. <span data-ttu-id="0b110-143">A klasszikus Azure portálon, válassza ki a **BizTalk szolgáltatások**, válassza ki a BizTalk szolgáltatás nevét, a biztonsági mentés ütemezését, és válassza ki a **konfigurálása** fülre.</span><span class="sxs-lookup"><span data-stu-id="0b110-143">In the Azure classic portal, select **BizTalk Services**, select the BizTalk Service name you want to schedule the backup, and then select the **Configure** tab.</span></span>
2. <span data-ttu-id="0b110-144">Állítsa be a **biztonsági mentési állapot** való **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="0b110-144">Set the **Backup Status** to **Automatic**.</span></span> 
3. <span data-ttu-id="0b110-145">Válassza ki a **Tárfiók** a biztonsági másolatának tárolásához, adja meg a **gyakorisága** a biztonsági mentés létrehozásához és mennyi ideig tartsa meg a biztonsági mentések (**megőrzési nap**):</span><span class="sxs-lookup"><span data-stu-id="0b110-145">Select the **Storage Account** to store the backup, enter the **Frequency** to create the backups, and how long to keep the backups (**Retention Days**):</span></span>
   
    ![][AutomaticBU]
   
    <span data-ttu-id="0b110-146">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="0b110-146">**Notes**</span></span>     
   
   * <span data-ttu-id="0b110-147">A **megőrzési nap**, a megőrzési idő a biztonsági mentési gyakoriság nagyobbnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="0b110-147">In **Retention Days**, the retention period must be greater than the backup frequency.</span></span>
   * <span data-ttu-id="0b110-148">Válassza ki **mindig maradjon meg legalább egy biztonsági másolat**, akkor is, ha a megőrzési idő lejárt.</span><span class="sxs-lookup"><span data-stu-id="0b110-148">Select **Always keep at least one backup**, even if it is past the retention period.</span></span>
4. <span data-ttu-id="0b110-149">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="0b110-149">Select **Save**.</span></span>

<span data-ttu-id="0b110-150">Ha egy ütemezett biztonsági mentési feladat futhasson, hozna létre egy tárolót (a biztonsági mentési adatok tárolására) a megadott tárfiók.</span><span class="sxs-lookup"><span data-stu-id="0b110-150">When a scheduled backup job runs, it creates a container (to store backup data) in the storage account you entered.</span></span> <span data-ttu-id="0b110-151">A tároló neve *nevét – dátum és idő BizTalk szolgáltatás*.</span><span class="sxs-lookup"><span data-stu-id="0b110-151">The name of the container is named *BizTalk Service Name-date-time*.</span></span> 

<span data-ttu-id="0b110-152">Ha a BizTalk szolgáltatás irányítópultját mutatja egy **sikertelen** állapota:</span><span class="sxs-lookup"><span data-stu-id="0b110-152">If the BizTalk Service dashboard shows a **Failed** status:</span></span>

![Utolsó ütemezett biztonsági mentés állapota][BackupStatus] 

<span data-ttu-id="0b110-154">A hivatkozás megnyitja a felügyeleti szolgáltatások műveletnaplók hibaelhárítás elősegítése érdekében.</span><span class="sxs-lookup"><span data-stu-id="0b110-154">The link opens the Management Services Operation Logs to help troubleshoot.</span></span> <span data-ttu-id="0b110-155">Lásd: [BizTalk szolgáltatások: műveletnaplók használata – hibaelhárítás](http://go.microsoft.com/fwlink/p/?LinkId=391211).</span><span class="sxs-lookup"><span data-stu-id="0b110-155">See [BizTalk Services: Troubleshoot using operation logs](http://go.microsoft.com/fwlink/p/?LinkId=391211).</span></span>

## <a name="restore"></a><span data-ttu-id="0b110-156">Visszaállítás</span><span class="sxs-lookup"><span data-stu-id="0b110-156">Restore</span></span>
<span data-ttu-id="0b110-157">A klasszikus Azure portálon vagy a biztonsági mentések visszaállíthatja a [visszaállítása BizTalk szolgáltatás REST API](http://go.microsoft.com/fwlink/p/?LinkID=325582).</span><span class="sxs-lookup"><span data-stu-id="0b110-157">You can restore backups from the Azure classic portal or from the [Restore BizTalk Service REST API](http://go.microsoft.com/fwlink/p/?LinkID=325582).</span></span> <span data-ttu-id="0b110-158">Ez a rész felsorolja a lépéseket a klasszikus portál segítségével történő visszaállításhoz.</span><span class="sxs-lookup"><span data-stu-id="0b110-158">This section lists the steps to restore using the classic portal.</span></span>

#### <a name="before-restoring-a-backup"></a><span data-ttu-id="0b110-159">Biztonsági másolat visszaállítása előtt</span><span class="sxs-lookup"><span data-stu-id="0b110-159">Before restoring a backup</span></span>
* <span data-ttu-id="0b110-160">Új követési archiválás és tárolók figyelése BizTalk szolgáltatás visszaállítása közben lehet megadni.</span><span class="sxs-lookup"><span data-stu-id="0b110-160">New tracking, archiving, and monitoring stores can be entered while restoring a BizTalk Service.</span></span>
* <span data-ttu-id="0b110-161">Az azonos EDI futási adataihoz helyreáll.</span><span class="sxs-lookup"><span data-stu-id="0b110-161">The same EDI Runtime data is restored.</span></span> <span data-ttu-id="0b110-162">A EDI futásidejű biztonsági másolatot tárol a vezérlő.</span><span class="sxs-lookup"><span data-stu-id="0b110-162">The EDI Runtime backup stores the control numbers.</span></span> <span data-ttu-id="0b110-163">A visszaállított vezérlő számok sorozatát a biztonsági másolat készítésének idején szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="0b110-163">The restored control numbers are in sequence from the time of the backup.</span></span> <span data-ttu-id="0b110-164">Üzenetek feldolgozása után utolsó biztonsági mentés, ha a biztonsági mentési tartalom visszaállítása ismétlődő vezérlő számok okozhat.</span><span class="sxs-lookup"><span data-stu-id="0b110-164">If messages are processed after the last backup, restoring this backup content can cause duplicate control numbers.</span></span>

#### <a name="restore-a-backup"></a><span data-ttu-id="0b110-165">Visszaállítás biztonsági másolatból</span><span class="sxs-lookup"><span data-stu-id="0b110-165">Restore a backup</span></span>
1. <span data-ttu-id="0b110-166">A klasszikus Azure portálon, válassza ki a **új** > **alkalmazásszolgáltatások** > **BizTalk szolgáltatás** > **visszaállítása**:</span><span class="sxs-lookup"><span data-stu-id="0b110-166">In the Azure classic portal, select **New** > **App Services** > **BizTalk Service** > **Restore**:</span></span>
   
    ![Visszaállítás biztonsági másolatból][Restore]
2. <span data-ttu-id="0b110-168">A **biztonsági mentési URL-cím**, jelölje ki a mappára, majd bontsa ki az Azure storage-fiók, amely tárolja a BizTalk szolgáltatás konfigurációs biztonsági másolat.</span><span class="sxs-lookup"><span data-stu-id="0b110-168">In **Backup URL**, select the folder icon and expand the Azure storage account that stores the BizTalk Service configuration backup.</span></span> <span data-ttu-id="0b110-169">Bontsa ki a tárolót, és jobb oldali ablaktáblában jelölje ki a megfelelő biztonságimásolat-.txt fájl.</span><span class="sxs-lookup"><span data-stu-id="0b110-169">Expand the container and in the right pane, select the corresponding back up .txt file.</span></span> 
   <br/><br/>
   <span data-ttu-id="0b110-170">Válassza ki **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="0b110-170">Select **Open**.</span></span>
3. <span data-ttu-id="0b110-171">A a **visszaállítás BizTalk szolgáltatás** lapján adjon meg egy **BizTalk szolgáltatás neve** , és ellenőrizze a **tartomány URL-cím**, **Edition**, és **Régió** a visszaállított BizTalk szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="0b110-171">On the **Restore BizTalk Service** page, enter a **BizTalk Service Name** and verify the **Domain URL**, **Edition**, and **Region** for the restored BizTalk Service.</span></span> <span data-ttu-id="0b110-172">**Hozzon létre egy új SQL-adatbázispéldány** a nyomon követési adatbázishoz:</span><span class="sxs-lookup"><span data-stu-id="0b110-172">**Create a new SQL database instance** for the tracking database:</span></span>
   
    ![][RestoreBizTalkService]
   
    <span data-ttu-id="0b110-173">Válassza ki a következő mutató nyílra.</span><span class="sxs-lookup"><span data-stu-id="0b110-173">Select the next arrow.</span></span>
4. <span data-ttu-id="0b110-174">Ellenőrizze az SQL-adatbázis nevét, adja meg, hogy a kiszolgáló a fizikai kiszolgálón, ahol az SQL-adatbázis fog létrejönni, és a felhasználónév/jelszó.</span><span class="sxs-lookup"><span data-stu-id="0b110-174">Verify the name of the SQL database, enter the physical server where the SQL database will be created, and a username/password for that server.</span></span>

    <span data-ttu-id="0b110-175">Ha az SQL adatbázis-kiadás, méretének és egyéb tulajdonságok konfigurálása, jelölje be **speciális adatbázis-beállítások konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="0b110-175">If you want to configure the SQL database edition, size, and other properties, select  **Configure Advanced Database Settings**.</span></span> 

    <span data-ttu-id="0b110-176">Válassza ki a következő mutató nyílra.</span><span class="sxs-lookup"><span data-stu-id="0b110-176">Select the next arrow.</span></span>

1. <span data-ttu-id="0b110-177">Hozzon létre egy új tárfiókot, vagy adjon meg egy meglévő tárfiók a BizTalk szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="0b110-177">Create a new storage account or enter an existing storage account for the BizTalk Service.</span></span>
2. <span data-ttu-id="0b110-178">Válassza ki a helyreállítás megkezdéséhez a pipára.</span><span class="sxs-lookup"><span data-stu-id="0b110-178">Select the checkmark to start the restore.</span></span>

<span data-ttu-id="0b110-179">Miután a visszaállítás sikeresen befejeződött, egy új BizTalk szolgáltatás szerepel a klasszikus Azure portálon BizTalk szolgáltatások lapon felfüggesztett állapotban.</span><span class="sxs-lookup"><span data-stu-id="0b110-179">Once the restoration successfully completes, a new BizTalk Service is listed in a suspended state on the BizTalk Services page in the Azure classic portal.</span></span>

### <span data-ttu-id="0b110-180"><a name="postrestore"></a>Biztonsági másolat visszaállítása után</span><span class="sxs-lookup"><span data-stu-id="0b110-180"><a name="postrestore"></a>After restoring a backup</span></span>
<span data-ttu-id="0b110-181">A BizTalk szolgáltatás mindig visszaállítása egy **felfüggesztett** állapotát.</span><span class="sxs-lookup"><span data-stu-id="0b110-181">The BizTalk Service is always restored in a **Suspended** state.</span></span> <span data-ttu-id="0b110-182">Ebben az állapotban biztosíthatja végrehajtott bármilyen konfigurációs módosításokat, mielőtt az új környezet is használható, beleértve:</span><span class="sxs-lookup"><span data-stu-id="0b110-182">In this state, you can make any configuration changes before the new environment is functional, including:</span></span>

* <span data-ttu-id="0b110-183">Ha létrehozta a BizTalk szolgáltatás alkalmazások az Azure BizTalk Services SDK használatával, esetleg az Access Control (ACS) hitelesítő adatait a ezeknek az alkalmazásoknak a visszaállított környezet használata.</span><span class="sxs-lookup"><span data-stu-id="0b110-183">If you created BizTalk Service applications using the Azure BizTalk Services SDK, you may need to to update the Access Control (ACS) credentials in those applications to work with the restored environment.</span></span>
* <span data-ttu-id="0b110-184">A BizTalk szolgáltatás replikálni egy meglévő BizTalk Service-környezet visszaállítása</span><span class="sxs-lookup"><span data-stu-id="0b110-184">You restore a BizTalk Service to replicate an existing BizTalk Service environment.</span></span> <span data-ttu-id="0b110-185">Ebben a helyzetben FTP forrásmappa, használja az eredeti BizTalk szolgáltatások portálon konfigurált szerződések esetén szükség lehet frissíteni a megállapodások az újonnan visszaállított környezetben egy másik FTP forrásmappa használatára.</span><span class="sxs-lookup"><span data-stu-id="0b110-185">In this situation, if there are agreements configured in the original BizTalk Services portal that use a source FTP folder, you may need to update the agreements in the newly restored environment to use a different source FTP folder.</span></span> <span data-ttu-id="0b110-186">Ellenkező esetben lehet két különböző megállapodások ugyanaz az üzenet lekérésére tett kísérlet.</span><span class="sxs-lookup"><span data-stu-id="0b110-186">Otherwise, there may be two different agreements trying to pull the same message.</span></span>
* <span data-ttu-id="0b110-187">Ha szeretné, hogy több BizTalk szolgáltatás környezet visszaállítása, ellenőrizze, hogy célozhat meg a megfelelő környezet a Visual Studio alkalmazást, a PowerShell-parancsmagok, a REST API-k vagy a kereskedelmi Partner OM API-val.</span><span class="sxs-lookup"><span data-stu-id="0b110-187">If you restored to have multiple BizTalk Service environments, make sure you target the correct environment in the Visual Studio applications, PowerShell cmdlets, REST APIs, or Trading Partner Management OM APIs.</span></span>
* <span data-ttu-id="0b110-188">Ajánlott az automatikus biztonsági mentések konfigurálása az újonnan visszaállított BizTalk szolgáltatás környezetben.</span><span class="sxs-lookup"><span data-stu-id="0b110-188">It's a good practice to configure automated backups on the newly restored BizTalk Service environment.</span></span>

<span data-ttu-id="0b110-189">A BizTalk szolgáltatás elindításához a klasszikus Azure portálon, válassza ki a visszaállított BizTalk szolgáltatás, majd **folytatása** a tálcán.</span><span class="sxs-lookup"><span data-stu-id="0b110-189">To start the BizTalk Service in the Azure classic portal, select the restored BizTalk Service and select **Resume** in the task bar.</span></span> 

## <a name="what-gets-backed-up"></a><span data-ttu-id="0b110-190">Mi a biztonsági mentés beolvasása</span><span class="sxs-lookup"><span data-stu-id="0b110-190">What gets backed up</span></span>
<span data-ttu-id="0b110-191">Biztonsági másolat létrehozásakor a következő elemek biztonsági mentése:</span><span class="sxs-lookup"><span data-stu-id="0b110-191">When a backup is created, the following items are backed up:</span></span>

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH><span data-ttu-id="0b110-192">Biztonsági mentésben szereplő elemek</span><span class="sxs-lookup"><span data-stu-id="0b110-192">Items backed up</span></span></TH> 
</tr> 
<tr>
<td colspan="2"><span data-ttu-id="0b110-193">
 <strong>Az Azure BizTalk szolgáltatások portálja</strong></span><span class="sxs-lookup"><span data-stu-id="0b110-193">
 <strong>Azure BizTalk Services Portal</strong></span></span></td>
</tr> 
<tr>
<td><span data-ttu-id="0b110-194">Konfigurációs és futásidejű</span><span class="sxs-lookup"><span data-stu-id="0b110-194">Configuration and Runtime</span></span></td> 
<td>
<ul>
<li><span data-ttu-id="0b110-195">Partnerek és a profil részletei</span><span class="sxs-lookup"><span data-stu-id="0b110-195">Partner and profile details</span></span></li>
<li><span data-ttu-id="0b110-196">Egyezmények</span><span class="sxs-lookup"><span data-stu-id="0b110-196">Partner Agreements</span></span></li>
<li><span data-ttu-id="0b110-197">Egyéni szerelvényeket telepített</span><span class="sxs-lookup"><span data-stu-id="0b110-197">Custom assemblies deployed</span></span></li>
<li><span data-ttu-id="0b110-198">Telepített hidak</span><span class="sxs-lookup"><span data-stu-id="0b110-198">Bridges deployed</span></span></li>
<li><span data-ttu-id="0b110-199">Tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="0b110-199">Certificates</span></span></li>
<li><span data-ttu-id="0b110-200">Telepített átalakítások</span><span class="sxs-lookup"><span data-stu-id="0b110-200">Transforms deployed</span></span></li>
<li><span data-ttu-id="0b110-201">Folyamatok</span><span class="sxs-lookup"><span data-stu-id="0b110-201">Pipelines</span></span></li>
<li><span data-ttu-id="0b110-202">Sablonok létrehozása és mentése a BizTalk szolgáltatások portálon</span><span class="sxs-lookup"><span data-stu-id="0b110-202">Templates created and saved in the BizTalk Services Portal</span></span></li>
<li><span data-ttu-id="0b110-203">X12 ST01 és GS01</span><span class="sxs-lookup"><span data-stu-id="0b110-203">X12 ST01 and GS01 mappings</span></span></li>
<li><span data-ttu-id="0b110-204">Vezérlő számok (EDI)</span><span class="sxs-lookup"><span data-stu-id="0b110-204">Control numbers (EDI)</span></span></li>
<li><span data-ttu-id="0b110-205">AS2-üzenet MIC értékek</span><span class="sxs-lookup"><span data-stu-id="0b110-205">AS2 Message MIC values</span></span></li>
</ul>
</td>
</tr> 

<tr>
<td colspan="2"><span data-ttu-id="0b110-206">
 <strong>Azure BizTalk szolgáltatás</strong></span><span class="sxs-lookup"><span data-stu-id="0b110-206">
 <strong>Azure BizTalk Service</strong></span></span></td>
</tr> 
<tr>
<td><span data-ttu-id="0b110-207">SSL-tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="0b110-207">SSL Certificate</span></span></td> 
<td>
<ul>
<li><span data-ttu-id="0b110-208">SSL-Tanúsítványadatok</span><span class="sxs-lookup"><span data-stu-id="0b110-208">SSL Certificate Data</span></span></li>
<li><span data-ttu-id="0b110-209">SSL-tanúsítvány jelszava</span><span class="sxs-lookup"><span data-stu-id="0b110-209">SSL Certificate Password</span></span></li>
</ul>
</td>
</tr> 
<tr>
<td><span data-ttu-id="0b110-210">BizTalk szolgáltatás beállításai</span><span class="sxs-lookup"><span data-stu-id="0b110-210">BizTalk Service Settings</span></span></td> 
<td>
<ul>
<li><span data-ttu-id="0b110-211">Méretezési egységek száma</span><span class="sxs-lookup"><span data-stu-id="0b110-211">Scale unit count</span></span></li>
<li><span data-ttu-id="0b110-212">Kiadás</span><span class="sxs-lookup"><span data-stu-id="0b110-212">Edition</span></span></li>
<li><span data-ttu-id="0b110-213">Termékverzió</span><span class="sxs-lookup"><span data-stu-id="0b110-213">Product Version</span></span></li>
<li><span data-ttu-id="0b110-214">Régió/adatközpont</span><span class="sxs-lookup"><span data-stu-id="0b110-214">Region/Datacenter</span></span></li>
<li><span data-ttu-id="0b110-215">Access Control Service (ACS) névtér és a kulcs</span><span class="sxs-lookup"><span data-stu-id="0b110-215">Access Control Service (ACS) namespace and key</span></span></li>
<li><span data-ttu-id="0b110-216">Adatbázis-kapcsolati karakterlánc nyomon követése</span><span class="sxs-lookup"><span data-stu-id="0b110-216">Tracking database connection string</span></span></li>
<li><span data-ttu-id="0b110-217">Archiválási tárolási fiók kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="0b110-217">Archiving Storage account connection string</span></span></li>
<li><span data-ttu-id="0b110-218">Figyelési tárolási fiók kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="0b110-218">Monitoring storage account connection string</span></span></li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2"><span data-ttu-id="0b110-219">
 <strong>További elemek</strong></span><span class="sxs-lookup"><span data-stu-id="0b110-219">
 <strong>Additional Items</strong></span></span></td>
</tr> 
<tr>
<td><span data-ttu-id="0b110-220">Adatbázis nyomon követése</span><span class="sxs-lookup"><span data-stu-id="0b110-220">Tracking Database</span></span></td> 
<td><span data-ttu-id="0b110-221">A BizTalk szolgáltatás létrehozása, ha a nyomkövetési adatainak van-e megadva, beleértve az Azure SQL adatbázis-kiszolgáló és a nyomkövetési adatbázisnevet.</span><span class="sxs-lookup"><span data-stu-id="0b110-221">When the BizTalk Service is created, the Tracking Database details are entered, including the Azure SQL Database Server and the Tracking Database name.</span></span> <span data-ttu-id="0b110-222">A követés adatbázis nem automatikusan biztonsági másolat.</span><span class="sxs-lookup"><span data-stu-id="0b110-222">The Tracking Database is not automatically backed up.</span></span>
<br/><br/><span data-ttu-id="0b110-223">
<strong>Fontos</strong></span><span class="sxs-lookup"><span data-stu-id="0b110-223">
<strong>Important</strong></span></span><br/>
<span data-ttu-id="0b110-224">Ha a nyomkövetési adatbázisa törlődik, és az adatbázisban kell helyreállítani, a korábbi biztonsági léteznie kell.</span><span class="sxs-lookup"><span data-stu-id="0b110-224">If the Tracking Database is deleted and the database needs recovered, a previous backup must exist.</span></span> <span data-ttu-id="0b110-225">Ha a biztonsági mentés nem létezik, a követési adatbázis és az adatokat is nem helyreállítható.</span><span class="sxs-lookup"><span data-stu-id="0b110-225">If a backup does not exist, the Tracking Database and its data are not recoverable.</span></span> <span data-ttu-id="0b110-226">Ebben a helyzetben új követési adatbázis létrehozása a azonos nevű adatbázis.</span><span class="sxs-lookup"><span data-stu-id="0b110-226">In this situation, create a new Tracking Database with the same database name.</span></span> <span data-ttu-id="0b110-227">A Georeplikáció ajánlott.</span><span class="sxs-lookup"><span data-stu-id="0b110-227">Geo-Replication is recommended.</span></span></td>
</tr> 
</table>

## <a name="next"></a><span data-ttu-id="0b110-228">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="0b110-228">Next</span></span>
<span data-ttu-id="0b110-229">Azure BizTalk szolgáltatás létrehozása a klasszikus Azure portálon, keresse fel [BizTalk szolgáltatások: kiépítés használata Azure klasszikus portál](http://go.microsoft.com/fwlink/p/?LinkID=302280).</span><span class="sxs-lookup"><span data-stu-id="0b110-229">To create Azure BizTalk Services in the Azure classic portal, go to [BizTalk Services: Provisioning Using Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=302280).</span></span> <span data-ttu-id="0b110-230">Az alkalmazások létrehozásának megkezdéséhez ugorjon az [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197) című témakörre.</span><span class="sxs-lookup"><span data-stu-id="0b110-230">To start creating applications, go to [Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).</span></span>

## <a name="see-also"></a><span data-ttu-id="0b110-231">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="0b110-231">See Also</span></span>
* [<span data-ttu-id="0b110-232">Biztonsági mentési BizTalk szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="0b110-232">Backup BizTalk Service</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [<span data-ttu-id="0b110-233">Állítsa vissza biztonsági másolatból BizTalk szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="0b110-233">Restore BizTalk Service from Backup</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=325582)
* [<span data-ttu-id="0b110-234">BizTalk szolgáltatások: Fejlesztői, Basic, Standard és prémium kiadás diagram</span><span class="sxs-lookup"><span data-stu-id="0b110-234">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302279)
* [<span data-ttu-id="0b110-235">BizTalk szolgáltatások: Kiépítés használata Azure klasszikus portál</span><span class="sxs-lookup"><span data-stu-id="0b110-235">BizTalk Services: Provisioning Using Azure classic portal</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302280)
* [<span data-ttu-id="0b110-236">BizTalk Services: Kiépítési állapot diagramja</span><span class="sxs-lookup"><span data-stu-id="0b110-236">BizTalk Services: Provisioning Status Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329870)
* [<span data-ttu-id="0b110-237">BizTalk Services: Irányítópult, Figyelés és Méret lapok</span><span class="sxs-lookup"><span data-stu-id="0b110-237">BizTalk Services: Dashboard, Monitor and Scale tabs</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302281)
* [<span data-ttu-id="0b110-238">BizTalk Services: Szabályozás</span><span class="sxs-lookup"><span data-stu-id="0b110-238">BizTalk Services: Throttling</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302282)
* [<span data-ttu-id="0b110-239">BizTalk Services: Kiállító neve és kiállító kulcsa</span><span class="sxs-lookup"><span data-stu-id="0b110-239">BizTalk Services: Issuer Name and Issuer Key</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303941)
* [<span data-ttu-id="0b110-240">Hogyan kezdhetem el az Azure BizTalk Services SDK használatát</span><span class="sxs-lookup"><span data-stu-id="0b110-240">How do I Start Using the Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png

