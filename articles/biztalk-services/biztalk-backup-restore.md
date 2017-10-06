---
title: "aaaCreate és a biztonsági másolat visszaállításával a BizTalk szolgáltatások |} Microsoft Docs"
description: "BizTalk szolgáltatások részét képező, biztonsági mentését és helyreállítását. Megtudhatja, hogyan toocreate és a biztonsági másolat visszaállításával és határozza meg, mi lekérdezi a biztonsági mentés. MABS, WABS"
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
ms.openlocfilehash: 32356ad870678fa5fd5bbbbf13d9377188f770a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="biztalk-services-backup-and-restore"></a><span data-ttu-id="14162-105">BizTalk Services: Biztonsági mentés és visszaállítás</span><span class="sxs-lookup"><span data-stu-id="14162-105">BizTalk Services: Backup and Restore</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

<span data-ttu-id="14162-106">Azure BizTalk-szolgáltatásokhoz kínálja biztonsági mentését és helyreállítását.</span><span class="sxs-lookup"><span data-stu-id="14162-106">Azure BizTalk Services includes Backup and Restore capabilities.</span></span> <span data-ttu-id="14162-107">Ez a témakör ismerteti, hogyan toobackup és visszaállítási BizTalk szolgáltatások használatával hello a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="14162-107">This topic describes how toobackup and restore BizTalk Services using hello Azure classic portal.</span></span>

<span data-ttu-id="14162-108">Is biztonsági másolatot készíthet BizTalk szolgáltatásokat hello segítségével [BizTalk szolgáltatás REST API](http://go.microsoft.com/fwlink/p/?LinkID=325584).</span><span class="sxs-lookup"><span data-stu-id="14162-108">You can also back up BizTalk Services using hello [BizTalk Services REST API](http://go.microsoft.com/fwlink/p/?LinkID=325584).</span></span> 

> [!NOTE]
> <span data-ttu-id="14162-109">Hibrid kapcsolatok nem készül biztonsági mentés, hello Edition függetlenül.</span><span class="sxs-lookup"><span data-stu-id="14162-109">Hybrid Connections are NOT backed up, regardless of hello Edition.</span></span> <span data-ttu-id="14162-110">A hibrid kapcsolatok újra létre kell hoznia.</span><span class="sxs-lookup"><span data-stu-id="14162-110">You must recreate your hybrid connections.</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="14162-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="14162-111">Before you Begin</span></span>
* <span data-ttu-id="14162-112">Biztonsági mentés és visszaállítás nem lehet elérhető összes kiadása esetén.</span><span class="sxs-lookup"><span data-stu-id="14162-112">Backup and Restore may not be available for all editions.</span></span> <span data-ttu-id="14162-113">Lásd: [BizTalk szolgáltatások: kiadások diagram](biztalk-editions-feature-chart.md).</span><span class="sxs-lookup"><span data-stu-id="14162-113">See [BizTalk Services: Editions Chart](biztalk-editions-feature-chart.md).</span></span>
* <span data-ttu-id="14162-114">Hello klasszikus Azure-portált használja, hozzon létre egy igény szerinti biztonsági mentést, vagy hozzon létre ütemezett biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="14162-114">Using hello Azure classic portal, you can create an On Demand backup or create a scheduled backup.</span></span> 
* <span data-ttu-id="14162-115">Biztonsági mentés a tartalom lehet a visszaállított toohello BizTalk szolgáltatás azonos vagy tooa új BizTalk szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="14162-115">Backup content can be restored toohello same BizTalk Service or tooa new BizTalk Service.</span></span> <span data-ttu-id="14162-116">toorestore hello BizTalk szolgáltatás használata hello azonos nevű, meglévő BizTalk szolgáltatás hello törölni kell, és hello neve elérhetőnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="14162-116">toorestore hello BizTalk Service using hello same name, hello existing BizTalk Service must be deleted and hello name must be available.</span></span> <span data-ttu-id="14162-117">Ha töröl egy BizTalk szolgáltatás, a hello szeretett volna azonos hosszabb időt is igénybe vehet neve toobe érhető el.</span><span class="sxs-lookup"><span data-stu-id="14162-117">After you delete a BizTalk Service, it can take longer time than wanted for hello same name toobe available.</span></span> <span data-ttu-id="14162-118">Ha nem várja meg a hello azonos toobe rendelkezésre álló nevet, majd tooa visszaállítása új BizTalk szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="14162-118">If you cannot wait for hello same name toobe available, then restore tooa new BizTalk Service.</span></span>
* <span data-ttu-id="14162-119">BizTalk szolgáltatások lehet visszaállított toohello azonos kiadást vagy újabb kiadását.</span><span class="sxs-lookup"><span data-stu-id="14162-119">BizTalk Services can be restored toohello same edition or a higher edition.</span></span> <span data-ttu-id="14162-120">BizTalk szolgáltatások tooa visszaállítása korábbi kiadását, amikor hello a biztonsági mentés készült, nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="14162-120">Restoring BizTalk Services tooa lower edition, from when hello backup was taken, is not supported.</span></span>
  
    <span data-ttu-id="14162-121">Például egy biztonsági alapszintű kiadására lehet hello segítségével vissza toohello verziót prémium kiadásúra.</span><span class="sxs-lookup"><span data-stu-id="14162-121">For example, a backup using hello Basic Edition can be restored toohello Premium Edition.</span></span> <span data-ttu-id="14162-122">Prémium szintű kiadása nem lehet hello segítségével biztonsági toohello Standard Edition visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="14162-122">A backup using hello Premium Edition cannot be restored toohello Standard Edition.</span></span>
* <span data-ttu-id="14162-123">hello EDI-vezérlő számok készül biztonsági másolat hello vezérlő számok toomaintain folyamatosságát.</span><span class="sxs-lookup"><span data-stu-id="14162-123">hello EDI Control numbers are backed up toomaintain continuity of hello control numbers.</span></span> <span data-ttu-id="14162-124">Ha üzenetek feldolgozása hello utolsó biztonsági mentés óta, a biztonsági mentési tartalom visszaállítása okozhat ismétlődő vezérlő számokat.</span><span class="sxs-lookup"><span data-stu-id="14162-124">If messages are processed after hello last backup, restoring this backup content can cause duplicate control numbers.</span></span>
* <span data-ttu-id="14162-125">Ha egy kötegelt aktív üzenetek, dolgozza fel hello kötegelt **előtt** biztonsági másolat készítése.</span><span class="sxs-lookup"><span data-stu-id="14162-125">If a batch has active messages, process hello batch **before** running a backup.</span></span> <span data-ttu-id="14162-126">(A szükséges vagy ütemezett) a biztonsági másolat létrehozásakor a kötegek üzenetek soha nem kerülnek.</span><span class="sxs-lookup"><span data-stu-id="14162-126">When creating a backup (as needed or scheduled), messages in batches are never stored.</span></span> 
  
    <span data-ttu-id="14162-127">**Biztonsági másolatot készít aktív üzeneteket kötegekben, ha ezek az üzenetek nem készül biztonsági mentés, és ezért elvesznek.**</span><span class="sxs-lookup"><span data-stu-id="14162-127">**If a backup is taken with active messages in a batch, these messages are not backed up and are therefore lost.**</span></span>
* <span data-ttu-id="14162-128">Választható lehetőség: A hello BizTalk szolgáltatások portálja, állítsa le felügyeleti műveleteket sem.</span><span class="sxs-lookup"><span data-stu-id="14162-128">Optional: In hello BizTalk Services Portal, stop any management operations.</span></span>

## <a name="create-a-backup"></a><span data-ttu-id="14162-129">Biztonsági mentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="14162-129">Create a backup</span></span>
<span data-ttu-id="14162-130">A biztonsági mentés akkor van szükség, tetszőleges időpontban, és teljesen vezérli akkor.</span><span class="sxs-lookup"><span data-stu-id="14162-130">A backup can be taken at any time and is completely controlled by you.</span></span> <span data-ttu-id="14162-131">Ez a rész felsorolja hello lépéseket toocreate a biztonsági mentéseket hello klasszikus Azure portál, beleértve:</span><span class="sxs-lookup"><span data-stu-id="14162-131">This section lists hello steps toocreate backups using hello Azure classic portal, including:</span></span>

[<span data-ttu-id="14162-132">Az igény szerinti biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="14162-132">On Demand backup</span></span>](#backupnow)

[<span data-ttu-id="14162-133">A biztonsági mentés ütemezése</span><span class="sxs-lookup"><span data-stu-id="14162-133">Schedule a backup</span></span>](#backupschedule)

#### <span data-ttu-id="14162-134"><a name="backupnow"></a>Az igény szerinti biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="14162-134"><a name="backupnow"></a>On Demand backup</span></span>
1. <span data-ttu-id="14162-135">Hello a klasszikus Azure portálon, válassza ki **BizTalk szolgáltatások**, majd válassza ki hello BizTalk szolgáltatás kívánt toobackup és.</span><span class="sxs-lookup"><span data-stu-id="14162-135">In hello Azure classic portal, select **BizTalk Services**, and then select hello BizTalk Service you want toobackup.</span></span>
2. <span data-ttu-id="14162-136">A hello **irányítópult** lapon jelölje be **készítsen biztonsági másolatot** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="14162-136">In hello **Dashboard** tab, select **Back up** at hello bottom of hello page.</span></span>
3. <span data-ttu-id="14162-137">Adja meg a biztonsági másolat neve.</span><span class="sxs-lookup"><span data-stu-id="14162-137">Enter a backup name.</span></span> <span data-ttu-id="14162-138">Adja meg például *myBizTalkService*BU*dátum*.</span><span class="sxs-lookup"><span data-stu-id="14162-138">For example, enter *myBizTalkService*BU*Date*.</span></span>
4. <span data-ttu-id="14162-139">Válasszon ki egy blob storage-fiók és a select hello pipa toostart hello biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="14162-139">Choose a blob storage account and select hello checkmark toostart hello backup.</span></span>

<span data-ttu-id="14162-140">Hello biztonsági mentés befejeztével, miután egy tároló meg hello biztonsági másolat nevű hello storage-fiók jön létre.</span><span class="sxs-lookup"><span data-stu-id="14162-140">Once hello backup completes, a container with hello backup name you enter is created in hello storage account.</span></span> <span data-ttu-id="14162-141">Ebben a tárolóban a BizTalk szolgáltatás biztonsági mentési konfigurációját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="14162-141">This container contains your BizTalk Service backup configuration.</span></span>

#### <span data-ttu-id="14162-142"><a name="backupschedule"></a>A biztonsági mentés ütemezése</span><span class="sxs-lookup"><span data-stu-id="14162-142"><a name="backupschedule"></a>Schedule a backup</span></span>
1. <span data-ttu-id="14162-143">Hello a klasszikus Azure portálon, válassza ki **BizTalk szolgáltatások**, válassza ki a BizTalk szolgáltatás kívánt nevet tooschedule hello biztonsági mentést, majd válassza ki a hello hello **konfigurálása** fülre.</span><span class="sxs-lookup"><span data-stu-id="14162-143">In hello Azure classic portal, select **BizTalk Services**, select hello BizTalk Service name you want tooschedule hello backup, and then select hello **Configure** tab.</span></span>
2. <span data-ttu-id="14162-144">Set hello **biztonsági mentés állapotának** túl**automatikus**.</span><span class="sxs-lookup"><span data-stu-id="14162-144">Set hello **Backup Status** too**Automatic**.</span></span> 
3. <span data-ttu-id="14162-145">Jelölje be hello **Tárfiók** toostore hello biztonsági mentési, adja meg a hello **gyakorisága** toocreate hello, és mennyi ideig tookeep hello biztonsági mentés (**megőrzési nap**):</span><span class="sxs-lookup"><span data-stu-id="14162-145">Select hello **Storage Account** toostore hello backup, enter hello **Frequency** toocreate hello backups, and how long tookeep hello backups (**Retention Days**):</span></span>
   
    ![][AutomaticBU]
   
    <span data-ttu-id="14162-146">**Megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="14162-146">**Notes**</span></span>     
   
   * <span data-ttu-id="14162-147">A **megőrzési nap**, hello megőrzési időszak hello biztonsági mentési gyakoriság nagyobbnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="14162-147">In **Retention Days**, hello retention period must be greater than hello backup frequency.</span></span>
   * <span data-ttu-id="14162-148">Válassza ki **mindig maradjon meg legalább egy biztonsági másolat**, akkor is, ha hello megőrzési időszak lejárt.</span><span class="sxs-lookup"><span data-stu-id="14162-148">Select **Always keep at least one backup**, even if it is past hello retention period.</span></span>
4. <span data-ttu-id="14162-149">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="14162-149">Select **Save**.</span></span>

<span data-ttu-id="14162-150">Egy ütemezett biztonsági mentési feladat futtatása megadott hello tárfiókban lévő létrehoz egy tároló (toostore biztonsági mentési adatok).</span><span class="sxs-lookup"><span data-stu-id="14162-150">When a scheduled backup job runs, it creates a container (toostore backup data) in hello storage account you entered.</span></span> <span data-ttu-id="14162-151">hello hello tároló neve *nevét – dátum és idő BizTalk szolgáltatás*.</span><span class="sxs-lookup"><span data-stu-id="14162-151">hello name of hello container is named *BizTalk Service Name-date-time*.</span></span> 

<span data-ttu-id="14162-152">Ha hello BizTalk szolgáltatás irányítópultját mutatja egy **sikertelen** állapota:</span><span class="sxs-lookup"><span data-stu-id="14162-152">If hello BizTalk Service dashboard shows a **Failed** status:</span></span>

![Utolsó ütemezett biztonsági mentés állapota][BackupStatus] 

<span data-ttu-id="14162-154">hello a hivatkozás megnyitja hello felügyeleti szolgáltatások műveletnaplók toohelp hibaelhárítása.</span><span class="sxs-lookup"><span data-stu-id="14162-154">hello link opens hello Management Services Operation Logs toohelp troubleshoot.</span></span> <span data-ttu-id="14162-155">Lásd: [BizTalk szolgáltatások: műveletnaplók használata – hibaelhárítás](http://go.microsoft.com/fwlink/p/?LinkId=391211).</span><span class="sxs-lookup"><span data-stu-id="14162-155">See [BizTalk Services: Troubleshoot using operation logs](http://go.microsoft.com/fwlink/p/?LinkId=391211).</span></span>

## <a name="restore"></a><span data-ttu-id="14162-156">Visszaállítás</span><span class="sxs-lookup"><span data-stu-id="14162-156">Restore</span></span>
<span data-ttu-id="14162-157">Visszaállíthatja a biztonsági mentések hello a klasszikus Azure portálon, illetve hello [visszaállítása BizTalk szolgáltatás REST API](http://go.microsoft.com/fwlink/p/?LinkID=325582).</span><span class="sxs-lookup"><span data-stu-id="14162-157">You can restore backups from hello Azure classic portal or from hello [Restore BizTalk Service REST API](http://go.microsoft.com/fwlink/p/?LinkID=325582).</span></span> <span data-ttu-id="14162-158">Ez a szakasz hello lépéseket toorestore hello klasszikus portál használatával.</span><span class="sxs-lookup"><span data-stu-id="14162-158">This section lists hello steps toorestore using hello classic portal.</span></span>

#### <a name="before-restoring-a-backup"></a><span data-ttu-id="14162-159">Biztonsági másolat visszaállítása előtt</span><span class="sxs-lookup"><span data-stu-id="14162-159">Before restoring a backup</span></span>
* <span data-ttu-id="14162-160">Új követési archiválás és tárolók figyelése BizTalk szolgáltatás visszaállítása közben lehet megadni.</span><span class="sxs-lookup"><span data-stu-id="14162-160">New tracking, archiving, and monitoring stores can be entered while restoring a BizTalk Service.</span></span>
* <span data-ttu-id="14162-161">hello azonos EDI futási adataihoz helyreáll.</span><span class="sxs-lookup"><span data-stu-id="14162-161">hello same EDI Runtime data is restored.</span></span> <span data-ttu-id="14162-162">hello EDI futásidejű biztonsági mentés hello vezérlő számokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="14162-162">hello EDI Runtime backup stores hello control numbers.</span></span> <span data-ttu-id="14162-163">vissza hello vezérlő számok sorozatát hello biztonsági másolat készítésének idején hello szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="14162-163">hello restored control numbers are in sequence from hello time of hello backup.</span></span> <span data-ttu-id="14162-164">Ha üzenetek feldolgozása hello utolsó biztonsági mentés óta, a biztonsági mentési tartalom visszaállítása okozhat ismétlődő vezérlő számokat.</span><span class="sxs-lookup"><span data-stu-id="14162-164">If messages are processed after hello last backup, restoring this backup content can cause duplicate control numbers.</span></span>

#### <a name="restore-a-backup"></a><span data-ttu-id="14162-165">Visszaállítás biztonsági másolatból</span><span class="sxs-lookup"><span data-stu-id="14162-165">Restore a backup</span></span>
1. <span data-ttu-id="14162-166">Hello a klasszikus Azure portálon, válassza ki **új** > **alkalmazásszolgáltatások** > **BizTalk szolgáltatás** > **visszaállítása **:</span><span class="sxs-lookup"><span data-stu-id="14162-166">In hello Azure classic portal, select **New** > **App Services** > **BizTalk Service** > **Restore**:</span></span>
   
    ![Visszaállítás biztonsági másolatból][Restore]
2. <span data-ttu-id="14162-168">A **biztonsági mentési URL-cím**, válassza ki a hello ikonja, és bontsa ki, hogy a tároló BizTalk szolgáltatás konfigurációs biztonsági másolat hello hello Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="14162-168">In **Backup URL**, select hello folder icon and expand hello Azure storage account that stores hello BizTalk Service configuration backup.</span></span> <span data-ttu-id="14162-169">Hello tároló és hello jobb oldali ablaktáblában jelölje ki a megfelelő biztonsági mentése .txt fájl hello.</span><span class="sxs-lookup"><span data-stu-id="14162-169">Expand hello container and in hello right pane, select hello corresponding back up .txt file.</span></span> 
   <br/><br/>
   <span data-ttu-id="14162-170">Válassza ki **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="14162-170">Select **Open**.</span></span>
3. <span data-ttu-id="14162-171">A hello **visszaállítás BizTalk szolgáltatás** lapján adjon meg egy **BizTalk szolgáltatás neve** , és ellenőrizze a hello **tartomány URL-cím**, **Edition**, és **Régió** hello vissza BizTalk szolgáltatás számára.</span><span class="sxs-lookup"><span data-stu-id="14162-171">On hello **Restore BizTalk Service** page, enter a **BizTalk Service Name** and verify hello **Domain URL**, **Edition**, and **Region** for hello restored BizTalk Service.</span></span> <span data-ttu-id="14162-172">**Hozzon létre egy új SQL-adatbázispéldány** az adatbázis követési hello:</span><span class="sxs-lookup"><span data-stu-id="14162-172">**Create a new SQL database instance** for hello tracking database:</span></span>
   
    ![][RestoreBizTalkService]
   
    <span data-ttu-id="14162-173">Válassza ki a hello tovább nyílra.</span><span class="sxs-lookup"><span data-stu-id="14162-173">Select hello next arrow.</span></span>
4. <span data-ttu-id="14162-174">Ellenőrizze az SQL-adatbázis hello hello nevét, adja meg, hogy a kiszolgáló hello fizikai kiszolgálón, ahol hello SQL-adatbázis fog létrejönni, és a felhasználónév/jelszó.</span><span class="sxs-lookup"><span data-stu-id="14162-174">Verify hello name of hello SQL database, enter hello physical server where hello SQL database will be created, and a username/password for that server.</span></span>

    <span data-ttu-id="14162-175">Ha tooconfigure hello SQL adatbázis-kiadás, méretének és egyéb tulajdonságai, jelölje be **speciális adatbázis-beállítások konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="14162-175">If you want tooconfigure hello SQL database edition, size, and other properties, select  **Configure Advanced Database Settings**.</span></span> 

    <span data-ttu-id="14162-176">Válassza ki a hello tovább nyílra.</span><span class="sxs-lookup"><span data-stu-id="14162-176">Select hello next arrow.</span></span>

1. <span data-ttu-id="14162-177">Hozzon létre egy új tárfiókot, vagy adja meg egy meglévő tárfiók hello BizTalk szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="14162-177">Create a new storage account or enter an existing storage account for hello BizTalk Service.</span></span>
2. <span data-ttu-id="14162-178">Válassza ki a hello pipa toostart hello visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="14162-178">Select hello checkmark toostart hello restore.</span></span>

<span data-ttu-id="14162-179">Miután hello visszaállítása sikeresen befejeződött, egy új BizTalk szolgáltatás szerepel a hello BizTalk szolgáltatások lapon a klasszikus Azure portálon hello felfüggesztett állapotban.</span><span class="sxs-lookup"><span data-stu-id="14162-179">Once hello restoration successfully completes, a new BizTalk Service is listed in a suspended state on hello BizTalk Services page in hello Azure classic portal.</span></span>

### <span data-ttu-id="14162-180"><a name="postrestore"></a>Biztonsági másolat visszaállítása után</span><span class="sxs-lookup"><span data-stu-id="14162-180"><a name="postrestore"></a>After restoring a backup</span></span>
<span data-ttu-id="14162-181">hello BizTalk szolgáltatás mindig visszaállítása van egy **felfüggesztett** állapotát.</span><span class="sxs-lookup"><span data-stu-id="14162-181">hello BizTalk Service is always restored in a **Suspended** state.</span></span> <span data-ttu-id="14162-182">Ebben az állapotban végrehajtott bármilyen konfigurációs módosításokat előtt hello új környezetben is használható, beleértve teheti meg:</span><span class="sxs-lookup"><span data-stu-id="14162-182">In this state, you can make any configuration changes before hello new environment is functional, including:</span></span>

* <span data-ttu-id="14162-183">Ha BizTalk szolgáltatás alkalmazások hello Azure BizTalk Services SDK használatával hozott létre, szükség lehet a tootooupdate hello Access Control (ACS) hitelesítő adatokat ezen alkalmazások toowork vissza hello környezettel.</span><span class="sxs-lookup"><span data-stu-id="14162-183">If you created BizTalk Service applications using hello Azure BizTalk Services SDK, you may need tootooupdate hello Access Control (ACS) credentials in those applications toowork with hello restored environment.</span></span>
* <span data-ttu-id="14162-184">A BizTalk szolgáltatás tooreplicate egy meglévő BizTalk Service-környezet visszaállítása</span><span class="sxs-lookup"><span data-stu-id="14162-184">You restore a BizTalk Service tooreplicate an existing BizTalk Service environment.</span></span> <span data-ttu-id="14162-185">Ebben a helyzetben megállapodások hello eredeti BizTalk szolgáltatások portálon konfigurált FTP forrásmappa, használó esetén szükség lehet az újonnan visszaállított hello környezet toouse egy másik FTP forrásmappa tooupdate hello megállapodásokat.</span><span class="sxs-lookup"><span data-stu-id="14162-185">In this situation, if there are agreements configured in hello original BizTalk Services portal that use a source FTP folder, you may need tooupdate hello agreements in hello newly restored environment toouse a different source FTP folder.</span></span> <span data-ttu-id="14162-186">Ellenkező esetben előfordulhat toopull próbált két különböző megállapodások hello ugyanazt az üzenetet.</span><span class="sxs-lookup"><span data-stu-id="14162-186">Otherwise, there may be two different agreements trying toopull hello same message.</span></span>
* <span data-ttu-id="14162-187">Toohave több BizTalk szolgáltatás környezet visszaállítás után ellenőrizze, hogy hello megfelelő környezet hello Visual Studio alkalmazást, a PowerShell-parancsmagok, a REST API-k vagy a kereskedelmi Partner Management OM API-k célozhat meg.</span><span class="sxs-lookup"><span data-stu-id="14162-187">If you restored toohave multiple BizTalk Service environments, make sure you target hello correct environment in hello Visual Studio applications, PowerShell cmdlets, REST APIs, or Trading Partner Management OM APIs.</span></span>
* <span data-ttu-id="14162-188">Egy jó gyakorlat tooconfigure automatikus biztonsági mentés az újonnan visszaállított hello BizTalk szolgáltatás környezet is.</span><span class="sxs-lookup"><span data-stu-id="14162-188">It's a good practice tooconfigure automated backups on hello newly restored BizTalk Service environment.</span></span>

<span data-ttu-id="14162-189">toostart hello BizTalk szolgáltatás hello klasszikus Azure portálon válassza hello a visszaállított BizTalk szolgáltatás, és válassza ki **folytatása** hello tálcán.</span><span class="sxs-lookup"><span data-stu-id="14162-189">toostart hello BizTalk Service in hello Azure classic portal, select hello restored BizTalk Service and select **Resume** in hello task bar.</span></span> 

## <a name="what-gets-backed-up"></a><span data-ttu-id="14162-190">Mi a biztonsági mentés beolvasása</span><span class="sxs-lookup"><span data-stu-id="14162-190">What gets backed up</span></span>
<span data-ttu-id="14162-191">Biztonsági másolat létrehozásakor hello következő elemek készül biztonsági másolat:</span><span class="sxs-lookup"><span data-stu-id="14162-191">When a backup is created, hello following items are backed up:</span></span>

<table border="1"> 
<tr bgcolor="FAF9F9">
<th> </th>
<TH><span data-ttu-id="14162-192">Biztonsági mentésben szereplő elemek</span><span class="sxs-lookup"><span data-stu-id="14162-192">Items backed up</span></span></TH> 
</tr> 
<tr>
<td colspan="2"><span data-ttu-id="14162-193">
 <strong>Az Azure BizTalk szolgáltatások portálja</strong></span><span class="sxs-lookup"><span data-stu-id="14162-193">
 <strong>Azure BizTalk Services Portal</strong></span></span></td>
</tr> 
<tr>
<td><span data-ttu-id="14162-194">Konfigurációs és futásidejű</span><span class="sxs-lookup"><span data-stu-id="14162-194">Configuration and Runtime</span></span></td> 
<td>
<ul>
<li><span data-ttu-id="14162-195">Partnerek és a profil részletei</span><span class="sxs-lookup"><span data-stu-id="14162-195">Partner and profile details</span></span></li>
<li><span data-ttu-id="14162-196">Egyezmények</span><span class="sxs-lookup"><span data-stu-id="14162-196">Partner Agreements</span></span></li>
<li><span data-ttu-id="14162-197">Egyéni szerelvényeket telepített</span><span class="sxs-lookup"><span data-stu-id="14162-197">Custom assemblies deployed</span></span></li>
<li><span data-ttu-id="14162-198">Telepített hidak</span><span class="sxs-lookup"><span data-stu-id="14162-198">Bridges deployed</span></span></li>
<li><span data-ttu-id="14162-199">Tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="14162-199">Certificates</span></span></li>
<li><span data-ttu-id="14162-200">Telepített átalakítások</span><span class="sxs-lookup"><span data-stu-id="14162-200">Transforms deployed</span></span></li>
<li><span data-ttu-id="14162-201">Folyamatok</span><span class="sxs-lookup"><span data-stu-id="14162-201">Pipelines</span></span></li>
<li><span data-ttu-id="14162-202">BizTalk szolgáltatások portálja hello az sablonok</span><span class="sxs-lookup"><span data-stu-id="14162-202">Templates created and saved in hello BizTalk Services Portal</span></span></li>
<li><span data-ttu-id="14162-203">X12 ST01 és GS01</span><span class="sxs-lookup"><span data-stu-id="14162-203">X12 ST01 and GS01 mappings</span></span></li>
<li><span data-ttu-id="14162-204">Vezérlő számok (EDI)</span><span class="sxs-lookup"><span data-stu-id="14162-204">Control numbers (EDI)</span></span></li>
<li><span data-ttu-id="14162-205">AS2-üzenet MIC értékek</span><span class="sxs-lookup"><span data-stu-id="14162-205">AS2 Message MIC values</span></span></li>
</ul>
</td>
</tr> 

<tr>
<td colspan="2"><span data-ttu-id="14162-206">
 <strong>Azure BizTalk szolgáltatás</strong></span><span class="sxs-lookup"><span data-stu-id="14162-206">
 <strong>Azure BizTalk Service</strong></span></span></td>
</tr> 
<tr>
<td><span data-ttu-id="14162-207">SSL-tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="14162-207">SSL Certificate</span></span></td> 
<td>
<ul>
<li><span data-ttu-id="14162-208">SSL-Tanúsítványadatok</span><span class="sxs-lookup"><span data-stu-id="14162-208">SSL Certificate Data</span></span></li>
<li><span data-ttu-id="14162-209">SSL-tanúsítvány jelszava</span><span class="sxs-lookup"><span data-stu-id="14162-209">SSL Certificate Password</span></span></li>
</ul>
</td>
</tr> 
<tr>
<td><span data-ttu-id="14162-210">BizTalk szolgáltatás beállításai</span><span class="sxs-lookup"><span data-stu-id="14162-210">BizTalk Service Settings</span></span></td> 
<td>
<ul>
<li><span data-ttu-id="14162-211">Méretezési egységek száma</span><span class="sxs-lookup"><span data-stu-id="14162-211">Scale unit count</span></span></li>
<li><span data-ttu-id="14162-212">Kiadás</span><span class="sxs-lookup"><span data-stu-id="14162-212">Edition</span></span></li>
<li><span data-ttu-id="14162-213">Termékverzió</span><span class="sxs-lookup"><span data-stu-id="14162-213">Product Version</span></span></li>
<li><span data-ttu-id="14162-214">Régió/adatközpont</span><span class="sxs-lookup"><span data-stu-id="14162-214">Region/Datacenter</span></span></li>
<li><span data-ttu-id="14162-215">Access Control Service (ACS) névtér és a kulcs</span><span class="sxs-lookup"><span data-stu-id="14162-215">Access Control Service (ACS) namespace and key</span></span></li>
<li><span data-ttu-id="14162-216">Adatbázis-kapcsolati karakterlánc nyomon követése</span><span class="sxs-lookup"><span data-stu-id="14162-216">Tracking database connection string</span></span></li>
<li><span data-ttu-id="14162-217">Archiválási tárolási fiók kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="14162-217">Archiving Storage account connection string</span></span></li>
<li><span data-ttu-id="14162-218">Figyelési tárolási fiók kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="14162-218">Monitoring storage account connection string</span></span></li>
</ul>
</td>
</tr> 
<tr>
<td colspan="2"><span data-ttu-id="14162-219">
 <strong>További elemek</strong></span><span class="sxs-lookup"><span data-stu-id="14162-219">
 <strong>Additional Items</strong></span></span></td>
</tr> 
<tr>
<td><span data-ttu-id="14162-220">Adatbázis nyomon követése</span><span class="sxs-lookup"><span data-stu-id="14162-220">Tracking Database</span></span></td> 
<td><span data-ttu-id="14162-221">Hello BizTalk szolgáltatás létrehozásakor hello követési adatainak van-e megadva, beleértve a hello Azure SQL adatbázis-kiszolgáló és a hello követési nevét.</span><span class="sxs-lookup"><span data-stu-id="14162-221">When hello BizTalk Service is created, hello Tracking Database details are entered, including hello Azure SQL Database Server and hello Tracking Database name.</span></span> <span data-ttu-id="14162-222">hello adatbázis nyomon követése nem automatikusan biztonsági másolat.</span><span class="sxs-lookup"><span data-stu-id="14162-222">hello Tracking Database is not automatically backed up.</span></span>
<br/><br/><span data-ttu-id="14162-223">
<strong>Fontos</strong></span><span class="sxs-lookup"><span data-stu-id="14162-223">
<strong>Important</strong></span></span><br/>
<span data-ttu-id="14162-224">Ha hello követési adatbázis törlődik, és hello adatbázisban kell helyreállítani, a korábbi biztonsági léteznie kell.</span><span class="sxs-lookup"><span data-stu-id="14162-224">If hello Tracking Database is deleted and hello database needs recovered, a previous backup must exist.</span></span> <span data-ttu-id="14162-225">Ha a biztonsági mentés nem létezik, hello adatbázis nyomon követése és annak adatai nincsenek helyreállítható.</span><span class="sxs-lookup"><span data-stu-id="14162-225">If a backup does not exist, hello Tracking Database and its data are not recoverable.</span></span> <span data-ttu-id="14162-226">Ebben a helyzetben hozzon létre új követési adatbázis hello azonos adatbázisnevet.</span><span class="sxs-lookup"><span data-stu-id="14162-226">In this situation, create a new Tracking Database with hello same database name.</span></span> <span data-ttu-id="14162-227">A Georeplikáció ajánlott.</span><span class="sxs-lookup"><span data-stu-id="14162-227">Geo-Replication is recommended.</span></span></td>
</tr> 
</table>

## <a name="next"></a><span data-ttu-id="14162-228">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="14162-228">Next</span></span>
<span data-ttu-id="14162-229">toocreate Azure BizTalk szolgáltatások a klasszikus Azure portál, nyissa meg túl hello[BizTalk szolgáltatások: kiépítés használata Azure klasszikus portál](http://go.microsoft.com/fwlink/p/?LinkID=302280).</span><span class="sxs-lookup"><span data-stu-id="14162-229">toocreate Azure BizTalk Services in hello Azure classic portal, go too[BizTalk Services: Provisioning Using Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=302280).</span></span> <span data-ttu-id="14162-230">alkalmazások, lépjen túl létrehozásának toostart[Azure BizTalk szolgáltatások](http://go.microsoft.com/fwlink/p/?LinkID=235197).</span><span class="sxs-lookup"><span data-stu-id="14162-230">toostart creating applications, go too[Azure BizTalk Services](http://go.microsoft.com/fwlink/p/?LinkID=235197).</span></span>

## <a name="see-also"></a><span data-ttu-id="14162-231">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="14162-231">See Also</span></span>
* [<span data-ttu-id="14162-232">Biztonsági mentési BizTalk szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="14162-232">Backup BizTalk Service</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=325584)
* [<span data-ttu-id="14162-233">Állítsa vissza biztonsági másolatból BizTalk szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="14162-233">Restore BizTalk Service from Backup</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=325582)
* [<span data-ttu-id="14162-234">BizTalk szolgáltatások: Fejlesztői, Basic, Standard és prémium kiadás diagram</span><span class="sxs-lookup"><span data-stu-id="14162-234">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302279)
* [<span data-ttu-id="14162-235">BizTalk szolgáltatások: Kiépítés használata Azure klasszikus portál</span><span class="sxs-lookup"><span data-stu-id="14162-235">BizTalk Services: Provisioning Using Azure classic portal</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302280)
* [<span data-ttu-id="14162-236">BizTalk Services: Kiépítési állapot diagramja</span><span class="sxs-lookup"><span data-stu-id="14162-236">BizTalk Services: Provisioning Status Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329870)
* [<span data-ttu-id="14162-237">BizTalk Services: Irányítópult, Figyelés és Méret lapok</span><span class="sxs-lookup"><span data-stu-id="14162-237">BizTalk Services: Dashboard, Monitor and Scale tabs</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302281)
* [<span data-ttu-id="14162-238">BizTalk Services: Szabályozás</span><span class="sxs-lookup"><span data-stu-id="14162-238">BizTalk Services: Throttling</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302282)
* [<span data-ttu-id="14162-239">BizTalk Services: Kiállító neve és kiállító kulcsa</span><span class="sxs-lookup"><span data-stu-id="14162-239">BizTalk Services: Issuer Name and Issuer Key</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303941)
* [<span data-ttu-id="14162-240">Hogyan tudom használata hello Azure BizTalk szolgáltatások SDK-t</span><span class="sxs-lookup"><span data-stu-id="14162-240">How do I Start Using hello Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[BackupStatus]: ./media/biztalk-backup-restore/status-last-backup.png
[Restore]: ./media/biztalk-backup-restore/restore-ui.png
[AutomaticBU]: ./media/biztalk-backup-restore/AutomaticBU.png
[RestoreBizTalkService]: ./media/biztalk-backup-restore/RestoreBizTalkServiceWindow.png

