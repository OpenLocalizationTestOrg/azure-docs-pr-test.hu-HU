<!--author=SharS last changed: 1/14/2016 -->

> [!NOTE]
> <span data-ttu-id="7bbb6-101">Módosítások toohello StorSimple Adapter SharePoint RBS konfiguráció meghozásakor akkor kell bejelentkeznie toohello tartományi rendszergazdák csoportba tartozó felhasználói fiókkal.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-101">When making changes toohello StorSimple Adapter for SharePoint RBS configuration, you must be logged on with a user account that belongs toohello Domain Admins group.</span></span> <span data-ttu-id="7bbb6-102">Emellett ugyanaz, mint a központi adminisztrációs gazdagép hello futó böngészővel hello konfigurálása lapon kell elérnie.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-102">Additionally, you must access hello configuration page from a browser running on hello same host as Central Administration.</span></span>
> 
> 

#### <a name="tooconfigure-rbs"></a><span data-ttu-id="7bbb6-103">tooconfigure RBS</span><span class="sxs-lookup"><span data-stu-id="7bbb6-103">tooconfigure RBS</span></span>
1. <span data-ttu-id="7bbb6-104">Nyissa meg hello SharePoint központi felügyelet lapját, és keresse meg a túl**rendszerbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-104">Open hello SharePoint Central Administration page, and browse too**System Settings**.</span></span> 
2. <span data-ttu-id="7bbb6-105">A hello **Azure StorSimple** kattintson **konfigurálása a StorSimple-Adapter**.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-105">In hello **Azure StorSimple** section, click **Configure StorSimple Adapter**.</span></span>
   
    ![StorSimple Adapter hello konfigurálása](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS1-include.png) 
3. <span data-ttu-id="7bbb6-107">A hello **konfigurálása a StorSimple-Adapter** lap:</span><span class="sxs-lookup"><span data-stu-id="7bbb6-107">On hello **Configure StorSimple Adapter** page:</span></span>
   
   1. <span data-ttu-id="7bbb6-108">Győződjön meg arról, hogy hello **szerkesztési útvonal engedélyezéséhez** jelölőnégyzet be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-108">Make sure that hello **Enable editing path** check box is selected.</span></span>
   2. <span data-ttu-id="7bbb6-109">Hello mezőbe írja be a hello univerzális elnevezési konvenció (UNC) szerinti elérési útját hello BLOB-tároló.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-109">In hello text box, type hello Universal Naming Convention (UNC) path of hello BLOB store.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="7bbb6-110">hello BLOB tároló kötet hello StorSimple eszközön konfigurált iSCSI-köteten kell elhelyezni.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-110">hello BLOB store volume must be hosted on an iSCSI volume configured on hello StorSimple device.</span></span>

   3. <span data-ttu-id="7bbb6-111">Kattintson a hello **engedélyezése** egyes hello tartalom-adatbázisokra, amelyet az tooconfigure távoli tárolás gombra.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-111">Click hello **Enable** button below each of hello content databases that you want tooconfigure for remote storage.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="7bbb6-112">hello BLOB-tárolónak kell lennie megosztott és elérhető-e az összes webes előtér-(WFE) kiszolgáló által, és hello felhasználói fiókot, amelyet a SharePoint-kiszolgálófarm hello beállított hozzáférési toohello megosztást kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-112">hello BLOB store must be shared and reachable by all web front-end (WFE) servers, and hello user account that is configured for hello SharePoint server farm must have access toohello share.</span></span>
      
      ![Hello RBS szolgáltató engedélyezése](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS2-include.png)
      
      <span data-ttu-id="7bbb6-114">Engedélyezése vagy letiltása RBS, akkor a következő üzenet hello is látható.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-114">When you enable or disable RBS, you will also see hello following message.</span></span>
      
      ![Konfigurálja a StorSimple Adapter engedélyezése letiltása](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_ConfigureStorSimpleAdapterEnableDisableMessage-include.png)

   4. <span data-ttu-id="7bbb6-116">Kattintson a hello **frissítés** gombok tooapply hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-116">Click hello **Update** button tooapply hello configuration.</span></span> <span data-ttu-id="7bbb6-117">Amikor rákattint hello **frissítés** gomb, hello RBS konfiguráció állapota frissülni fog minden WFE-kiszolgálón, és a farm egésze hello lesz, RBS engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-117">When you click hello **Update** button, hello RBS configuration status will be updated on all WFE servers, and hello entire farm will be RBS-enabled.</span></span> <span data-ttu-id="7bbb6-118">hello a következő üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-118">hello following message appears.</span></span>
      
      ![Csatoló konfigurációs üzenet](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS3-include.png)
      
      > [!NOTE]
      > <span data-ttu-id="7bbb6-120">RBS nagyon sok (több mint 200) adatbázisok SharePoint-farm beállításakor, hello SharePoint központi felügyelet weblap előfordulhat, hogy túllépi az időkorlátot. Ha ez történik, frissítse a hello lapot.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-120">If you are configuring RBS for a SharePoint farm with a very large number of databases (greater than 200), hello SharePoint Central Administration web page might time out. If that occurs, refresh hello page.</span></span> <span data-ttu-id="7bbb6-121">Ez nem befolyásolja a hello konfigurációs folyamatát.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-121">This does not affect hello configuration process.</span></span>

4. <span data-ttu-id="7bbb6-122">Hello konfigurációjának ellenőrzése:</span><span class="sxs-lookup"><span data-stu-id="7bbb6-122">Verify hello configuration:</span></span>
   
   1. <span data-ttu-id="7bbb6-123">Jelentkezzen be a SharePoint központi felügyeleti webhely toohello, és keresse meg a toohello **konfigurálása a StorSimple-Adapter** lap.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-123">Log on toohello SharePoint Central Administration website, and browse toohello **Configure StorSimple Adapter** page.</span></span>
   2. <span data-ttu-id="7bbb6-124">Ellenőrizze a hello konfigurációs részletek toomake meg arról, hogy ezek megegyeznek-e a megadott hello-beállítások.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-124">Check hello configuration details toomake sure that they match hello settings that you entered.</span></span> 
5. <span data-ttu-id="7bbb6-125">Győződjön meg arról, hogy RBS megfelelően működik-e:</span><span class="sxs-lookup"><span data-stu-id="7bbb6-125">Verify that RBS works correctly:</span></span>
   
   1. <span data-ttu-id="7bbb6-126">Töltse fel a dokumentum tooSharePoint.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-126">Upload a document tooSharePoint.</span></span> 
   2. <span data-ttu-id="7bbb6-127">Keresse meg a konfigurált toohello UNC elérési utat.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-127">Browse toohello UNC path that you configured.</span></span> <span data-ttu-id="7bbb6-128">Győződjön meg arról, hogy létrejött-e hello RBS könyvtárszerkezetét és feltöltött hello objektumot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-128">Make sure that hello RBS directory structure was created and that it contains hello uploaded object.</span></span>
6. <span data-ttu-id="7bbb6-129">(Választható) Használhatja a Microsoft RBS hello `Migrate()` részét képező SharePoint toomigrate meglévő BLOB tartalom toohello StorSimple eszköz PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-129">(Optional) You can use hello Microsoft RBS `Migrate()` PowerShell cmdlet included with SharePoint toomigrate existing BLOB content toohello StorSimple device.</span></span> <span data-ttu-id="7bbb6-130">További információkért lásd: [tartalmat át a virtuális gépbe vagy onnan a SharePoint 2013 RBS] [ 6] vagy [tartalmat át a virtuális gépbe vagy onnan RBS (SharePoint Foundation 2010)] [7].</span><span class="sxs-lookup"><span data-stu-id="7bbb6-130">For more information, see [Migrate content into or out of RBS in SharePoint 2013][6] or [Migrate content into or out of RBS (SharePoint Foundation 2010)][7].</span></span>
7. <span data-ttu-id="7bbb6-131">(Választható) A teszt telepítésekre ellenőrizheti, hogy hello Blobok került kívül hello tartalom-adatbázist az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="7bbb6-131">(Optional) On test installations, you can verify that hello BLOBs were moved out of hello content database as follows:</span></span> 
   
   1. <span data-ttu-id="7bbb6-132">Indítsa el az SQL Management Studiót.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-132">Start SQL Management Studio.</span></span>
   2. <span data-ttu-id="7bbb6-133">Hello ListBlobsInDB_2010.sql vagy ListBlobsInDB_2013.sql lekérdezés, az alábbiak szerint futtatni.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-133">Run hello ListBlobsInDB_2010.sql or ListBlobsInDB_2013.sql query, as follows.</span></span>
      
      ```
      **ListBlobsInDB_2013.sql**
      
        USE WSS_Content
        GO
      
        SELECT DocStreams.DocId,
      
               LeafName AS Name,
               Content,
               AllDocs.Size AS OrigSizeOfContent,
               LEN(CAST(Content AS VARBINARY(MAX))) AS SizeOfContentInDB,
               DocStreams.RbsId,
               TimeLastModified
      
        FROM DocStreams
      
             INNER JOIN AllDocs ON DocStreams.DocId = AllDocs.Id
        ORDER BY TimeLastModified DESC
        GO
      
      **ListBlobsInDB_2010.sql**
      
        USE WSS_Content
        GO
      
        SELECT AllDocStreams.Id,
      
               LeafName AS Name,
               Content,
               AllDocs.Size AS OrigSizeOfContent,
               LEN(CAST(Content AS VARBINARY(MAX))) AS SizeOfContentInDB,
               RbsId,
               TimeLastModified
        FROM AllDocStreams
      
             INNER JOIN AllDocs ON AllDocStreams.Id = AllDocs.Id
        ORDER BY TimeLastModified DESC
        GO
      ```
      
      <span data-ttu-id="7bbb6-134">Ha RBS megfelelően lett konfigurálva, NULL értéket meg kell jelennie hello SizeOfContentInDB oszlop lett feltöltve, és sikeresen externalized RBS az objektumhoz.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-134">If RBS was configured correctly, a NULL value should appear in hello SizeOfContentInDB column for any object that was uploaded and successfully externalized with RBS.</span></span>
8. <span data-ttu-id="7bbb6-135">(Választható) Miután RBS konfigurál, és helyezze át az összes BLOB tartalom toohello StorSimple eszköz áthelyezheti hello tartalom-adatbázist toohello eszköz.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-135">(Optional) After you configure RBS and move all BLOB content toohello StorSimple device, you can move hello content database toohello device.</span></span> <span data-ttu-id="7bbb6-136">Ha úgy dönt, hogy toomove hello tartalom-adatbázist, ajánlott konfigurált hello tartalom-adatbázist tároló hello eszköz elsődleges kötetként.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-136">If you choose toomove hello content database, we recommend that you configure hello content database storage on hello device as a primary volume.</span></span> <span data-ttu-id="7bbb6-137">Használja majd, létrehozni, SQL Server ajánlott eljárásokat toomigrate hello tartalom-adatbázist toohello StorSimple eszközt.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-137">Then, use established SQL Server best practices toomigrate hello content database toohello StorSimple device.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="7bbb6-138">Toohello eszköz áthelyezése hello tartalom-adatbázist csak támogatott hello StorSimple 8000 series (ez nem támogatott hello 5000 vagy a 7000-es adatsorozathoz).</span><span class="sxs-lookup"><span data-stu-id="7bbb6-138">Moving hello content database toohello device is only supported for hello StorSimple 8000 series (it is not supported for hello 5000 or 7000 series).</span></span>
   
   <span data-ttu-id="7bbb6-139">Ha hello tartalom-adatbázis és a Blobok hello StorSimple eszközön külön köteteken tárolja, azt javasoljuk a hello konfigurálása ugyanazon kötettároló.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-139">If you store BLOBs and hello content database in separate volumes on hello StorSimple device, we recommend that you configure them in hello same volume container.</span></span> <span data-ttu-id="7bbb6-140">Ez biztosítja, hogy azok készül biztonsági másolat együtt.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-140">This ensures that they will be backed up together.</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="7bbb6-141">Ha RBS nincs engedélyezve, hello tartalom-adatbázist toohello StorSimple eszköz áthelyezése nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-141">If you have not enabled RBS, we do not recommend moving hello content database toohello StorSimple device.</span></span> <span data-ttu-id="7bbb6-142">Ez a nem tesztelt konfigurálása során.</span><span class="sxs-lookup"><span data-stu-id="7bbb6-142">This is an untested configuration.</span></span>
   
9. <span data-ttu-id="7bbb6-143">Nyissa meg toohello következő lépés: [szemétgyűjtés konfigurálása](#configure-garbage-collection).</span><span class="sxs-lookup"><span data-stu-id="7bbb6-143">Go toohello next step: [Configure garbage collection](#configure-garbage-collection).</span></span>

[6]: https://technet.microsoft.com/library/ff628254(v=office.15).aspx
[7]: https://technet.microsoft.com/library/ff628255(v=office.14).aspx
