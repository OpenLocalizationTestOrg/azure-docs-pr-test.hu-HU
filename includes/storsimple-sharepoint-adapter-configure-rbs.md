<!--author=SharS last changed: 1/14/2016 -->

> [!NOTE]
> <span data-ttu-id="b0a11-101">A StorSimple-adaptert SharePoint RBS konfigurációs beállításainak módosításakor, meg kell bejelentkeznie, amely a Tartománygazdák csoporthoz tartozó fiókkal.</span><span class="sxs-lookup"><span data-stu-id="b0a11-101">When making changes to the StorSimple Adapter for SharePoint RBS configuration, you must be logged on with a user account that belongs to the Domain Admins group.</span></span> <span data-ttu-id="b0a11-102">A lap továbbá egy központi adminisztrációs ugyanazon a gazdagépen futó böngészőben kell elérnie.</span><span class="sxs-lookup"><span data-stu-id="b0a11-102">Additionally, you must access the configuration page from a browser running on the same host as Central Administration.</span></span>
> 
> 

#### <a name="to-configure-rbs"></a><span data-ttu-id="b0a11-103">RBS konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b0a11-103">To configure RBS</span></span>
1. <span data-ttu-id="b0a11-104">Nyissa meg a SharePoint központi felügyelet lapját, és keresse meg a **rendszerbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="b0a11-104">Open the SharePoint Central Administration page, and browse to **System Settings**.</span></span> 
2. <span data-ttu-id="b0a11-105">Az a **Azure StorSimple** kattintson **konfigurálása a StorSimple-Adapter**.</span><span class="sxs-lookup"><span data-stu-id="b0a11-105">In the **Azure StorSimple** section, click **Configure StorSimple Adapter**.</span></span>
   
    ![A StorSimple-Adapter konfigurálásához.](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS1-include.png) 
3. <span data-ttu-id="b0a11-107">Az a **konfigurálása a StorSimple-Adapter** lap:</span><span class="sxs-lookup"><span data-stu-id="b0a11-107">On the **Configure StorSimple Adapter** page:</span></span>
   
   1. <span data-ttu-id="b0a11-108">Győződjön meg arról, hogy a **szerkesztési útvonal engedélyezéséhez** jelölőnégyzet be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="b0a11-108">Make sure that the **Enable editing path** check box is selected.</span></span>
   2. <span data-ttu-id="b0a11-109">A szövegmezőbe írja be az univerzális elnevezési konvenció (UNC) szerinti elérési utat a BLOB-tároló.</span><span class="sxs-lookup"><span data-stu-id="b0a11-109">In the text box, type the Universal Naming Convention (UNC) path of the BLOB store.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="b0a11-110">A BLOB-tároló kötetet a StorSimple eszközön konfigurált iSCSI-köteten kell elhelyezni.</span><span class="sxs-lookup"><span data-stu-id="b0a11-110">The BLOB store volume must be hosted on an iSCSI volume configured on the StorSimple device.</span></span>

   3. <span data-ttu-id="b0a11-111">Kattintson a **engedélyezése** távtároló konfigurálni kívánt tartalom-adatbázisok mindegyike esetében gombra.</span><span class="sxs-lookup"><span data-stu-id="b0a11-111">Click the **Enable** button below each of the content databases that you want to configure for remote storage.</span></span>
      
      > [!NOTE]
      > <span data-ttu-id="b0a11-112">A BLOB-tárolónak kell lennie megosztott és elérhető-e az összes webes előtér-(WFE) kiszolgáló által, és a felhasználói fiók, amelyet a SharePoint-kiszolgálófarm beállított rendelkeznie kell a megosztás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="b0a11-112">The BLOB store must be shared and reachable by all web front-end (WFE) servers, and the user account that is configured for the SharePoint server farm must have access to the share.</span></span>
      
      ![A RBS szolgáltató engedélyezése](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS2-include.png)
      
      <span data-ttu-id="b0a11-114">Ha engedélyezi vagy letiltja a RBS, akkor is látható a következő üzenetet.</span><span class="sxs-lookup"><span data-stu-id="b0a11-114">When you enable or disable RBS, you will also see the following message.</span></span>
      
      ![Konfigurálja a StorSimple Adapter engedélyezése letiltása](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_ConfigureStorSimpleAdapterEnableDisableMessage-include.png)

   4. <span data-ttu-id="b0a11-116">Kattintson a **frissítés** a beállítások gombra.</span><span class="sxs-lookup"><span data-stu-id="b0a11-116">Click the **Update** button to apply the configuration.</span></span> <span data-ttu-id="b0a11-117">Amikor rákattint az **frissítés** gombra, a RBS konfiguráció állapota frissülni fog a ELŐTÉR-webkiszolgáló és a teljes farmban lesznek RBS engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="b0a11-117">When you click the **Update** button, the RBS configuration status will be updated on all WFE servers, and the entire farm will be RBS-enabled.</span></span> <span data-ttu-id="b0a11-118">A következő üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b0a11-118">The following message appears.</span></span>
      
      ![Csatoló konfigurációs üzenet](./media/storsimple-sharepoint-adapter-configure-rbs/HCS_SSASP_ConfigRBS3-include.png)
      
      > [!NOTE]
      > <span data-ttu-id="b0a11-120">RBS nagyon sok (több mint 200) adatbázisok SharePoint-farm beállításakor, a SharePoint központi felügyelet weblap előfordulhat, hogy túllépi az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="b0a11-120">If you are configuring RBS for a SharePoint farm with a very large number of databases (greater than 200), the SharePoint Central Administration web page might time out.</span></span> <span data-ttu-id="b0a11-121">Ha ez történik, frissítse az oldalt.</span><span class="sxs-lookup"><span data-stu-id="b0a11-121">If that occurs, refresh the page.</span></span> <span data-ttu-id="b0a11-122">Ez nem befolyásolja a konfigurációs folyamat.</span><span class="sxs-lookup"><span data-stu-id="b0a11-122">This does not affect the configuration process.</span></span>

4. <span data-ttu-id="b0a11-123">A konfiguráció ellenőrzése:</span><span class="sxs-lookup"><span data-stu-id="b0a11-123">Verify the configuration:</span></span>
   
   1. <span data-ttu-id="b0a11-124">Jelentkezzen be a SharePoint központi felügyeleti webhelyet, és keresse meg a **konfigurálása a StorSimple-Adapter** lap.</span><span class="sxs-lookup"><span data-stu-id="b0a11-124">Log on to the SharePoint Central Administration website, and browse to the **Configure StorSimple Adapter** page.</span></span>
   2. <span data-ttu-id="b0a11-125">Ellenőrizze a konfigurációs adatait, győződjön meg arról, hogy ezek megegyeznek-e a megadott beállításokat.</span><span class="sxs-lookup"><span data-stu-id="b0a11-125">Check the configuration details to make sure that they match the settings that you entered.</span></span> 
5. <span data-ttu-id="b0a11-126">Győződjön meg arról, hogy RBS megfelelően működik-e:</span><span class="sxs-lookup"><span data-stu-id="b0a11-126">Verify that RBS works correctly:</span></span>
   
   1. <span data-ttu-id="b0a11-127">SharePoint dokumentum feltöltése.</span><span class="sxs-lookup"><span data-stu-id="b0a11-127">Upload a document to SharePoint.</span></span> 
   2. <span data-ttu-id="b0a11-128">Tallózással keresse meg a beállított UNC elérési utat.</span><span class="sxs-lookup"><span data-stu-id="b0a11-128">Browse to the UNC path that you configured.</span></span> <span data-ttu-id="b0a11-129">Győződjön meg arról, hogy létrejött-e a RBS könyvtárszerkezetét és a feltöltött objektumot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b0a11-129">Make sure that the RBS directory structure was created and that it contains the uploaded object.</span></span>
6. <span data-ttu-id="b0a11-130">(Választható) Használhatja a Microsoft RBS `Migrate()` részét képező SharePoint meglévő blobtartalom át a StorSimple eszköz PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="b0a11-130">(Optional) You can use the Microsoft RBS `Migrate()` PowerShell cmdlet included with SharePoint to migrate existing BLOB content to the StorSimple device.</span></span> <span data-ttu-id="b0a11-131">További információkért lásd: [tartalmat át a virtuális gépbe vagy onnan a SharePoint 2013 RBS] [ 6] vagy [tartalmat át a virtuális gépbe vagy onnan RBS (SharePoint Foundation 2010)] [7].</span><span class="sxs-lookup"><span data-stu-id="b0a11-131">For more information, see [Migrate content into or out of RBS in SharePoint 2013][6] or [Migrate content into or out of RBS (SharePoint Foundation 2010)][7].</span></span>
7. <span data-ttu-id="b0a11-132">(Választható) A teszt telepítésekre ellenőrizheti, hogy a Blobok került kívül a tartalom-adatbázist az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="b0a11-132">(Optional) On test installations, you can verify that the BLOBs were moved out of the content database as follows:</span></span> 
   
   1. <span data-ttu-id="b0a11-133">Indítsa el az SQL Management Studiót.</span><span class="sxs-lookup"><span data-stu-id="b0a11-133">Start SQL Management Studio.</span></span>
   2. <span data-ttu-id="b0a11-134">A ListBlobsInDB_2010.sql vagy ListBlobsInDB_2013.sql lekérdezés futtatása, az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="b0a11-134">Run the ListBlobsInDB_2010.sql or ListBlobsInDB_2013.sql query, as follows.</span></span>
      
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
      
      <span data-ttu-id="b0a11-135">Ha RBS megfelelően lett konfigurálva, NULL értéket meg kell jelennie az összes objektum feltöltött, illetve a RBS sikeresen externalized SizeOfContentInDB oszlop.</span><span class="sxs-lookup"><span data-stu-id="b0a11-135">If RBS was configured correctly, a NULL value should appear in the SizeOfContentInDB column for any object that was uploaded and successfully externalized with RBS.</span></span>
8. <span data-ttu-id="b0a11-136">(Választható) Miután RBS konfigurálása, és helyezze át az összes BLOB tartalma a StorSimple eszköz, az eszköz áthelyezheti a tartalom-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="b0a11-136">(Optional) After you configure RBS and move all BLOB content to the StorSimple device, you can move the content database to the device.</span></span> <span data-ttu-id="b0a11-137">A tartalom-adatbázis áthelyezése választja, azt javasoljuk, hogy az eszköz elsődleges kötetként konfigurálása a tartalom-adatbázist tároló.</span><span class="sxs-lookup"><span data-stu-id="b0a11-137">If you choose to move the content database, we recommend that you configure the content database storage on the device as a primary volume.</span></span> <span data-ttu-id="b0a11-138">Használja majd, SQL Server ajánlott eljárások a tartalom-adatbázis áttelepítéséhez a StorSimple-eszköz létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b0a11-138">Then, use established SQL Server best practices to migrate the content database to the StorSimple device.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="b0a11-139">A tartalom-adatbázis áthelyezése az eszköz csak a StorSimple 8000 series (de még nem támogatott a 5000 vagy 7000-es sorozathoz) támogatott.</span><span class="sxs-lookup"><span data-stu-id="b0a11-139">Moving the content database to the device is only supported for the StorSimple 8000 series (it is not supported for the 5000 or 7000 series).</span></span>
   
   <span data-ttu-id="b0a11-140">Blobok és a tartalom-adatbázist a StorSimple eszköz külön köteteken tárolja, azt javasoljuk a kötet tárolóhoz konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="b0a11-140">If you store BLOBs and the content database in separate volumes on the StorSimple device, we recommend that you configure them in the same volume container.</span></span> <span data-ttu-id="b0a11-141">Ez biztosítja, hogy azok készül biztonsági másolat együtt.</span><span class="sxs-lookup"><span data-stu-id="b0a11-141">This ensures that they will be backed up together.</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="b0a11-142">RBS nincs engedélyezve, ha a tartalom-adatbázis áthelyezése a StorSimple-eszköz nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="b0a11-142">If you have not enabled RBS, we do not recommend moving the content database to the StorSimple device.</span></span> <span data-ttu-id="b0a11-143">Ez a nem tesztelt konfigurálása során.</span><span class="sxs-lookup"><span data-stu-id="b0a11-143">This is an untested configuration.</span></span>
   
9. <span data-ttu-id="b0a11-144">Nyissa meg a következő lépéssel: [szemétgyűjtés konfigurálása](#configure-garbage-collection).</span><span class="sxs-lookup"><span data-stu-id="b0a11-144">Go to the next step: [Configure garbage collection](#configure-garbage-collection).</span></span>

[6]: https://technet.microsoft.com/library/ff628254(v=office.15).aspx
[7]: https://technet.microsoft.com/library/ff628255(v=office.14).aspx
