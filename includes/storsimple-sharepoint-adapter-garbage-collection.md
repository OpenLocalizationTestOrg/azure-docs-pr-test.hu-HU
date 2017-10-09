<!--author=SharS last changed: 9/17/15-->

<span data-ttu-id="19063-101">Ezzel az eljárással tartalma:</span><span class="sxs-lookup"><span data-stu-id="19063-101">In this procedure, you will:</span></span>

1. <span data-ttu-id="19063-102">[Készítse elő a toorun hello karbantartó végrehajtható](#to-prepare-to-run-the-maintainer) .</span><span class="sxs-lookup"><span data-stu-id="19063-102">[Prepare toorun hello Maintainer executable](#to-prepare-to-run-the-maintainer) .</span></span>
2. <span data-ttu-id="19063-103">[Hello tartalom-adatbázist és a Lomtár előkészítése árva Blobok azonnali törlését](#to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs).</span><span class="sxs-lookup"><span data-stu-id="19063-103">[Prepare hello content database and Recycle Bin for immediate deletion of orphaned BLOBs](#to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs).</span></span>
3. <span data-ttu-id="19063-104">[Futtassa a Maintainer.exe](#to-run-the-maintainer).</span><span class="sxs-lookup"><span data-stu-id="19063-104">[Run Maintainer.exe](#to-run-the-maintainer).</span></span>
4. <span data-ttu-id="19063-105">[Hello tartalom-adatbázist és a beállítások visszaállításához](#to-revert-the-content-database-and-recycle-bin-settings).</span><span class="sxs-lookup"><span data-stu-id="19063-105">[Revert hello content database and Recycle Bin settings](#to-revert-the-content-database-and-recycle-bin-settings).</span></span>

#### <a name="tooprepare-toorun-hello-maintainer"></a><span data-ttu-id="19063-106">tooprepare toorun hello karbantartó</span><span class="sxs-lookup"><span data-stu-id="19063-106">tooprepare toorun hello Maintainer</span></span>
1. <span data-ttu-id="19063-107">Hello előtér-webkiszolgálón nyissa meg a SharePoint 2013 felügyeleti rendszerhéjat hello rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="19063-107">On hello Web front-end server, open hello SharePoint 2013 Management Shell as an administrator.</span></span>
2. <span data-ttu-id="19063-108">Keresse meg a toohello mappa *rendszerindítási meghajtó*: \Program Files\Microsoft SQL távoli Blob Storage 10.50\Maintainer\.</span><span class="sxs-lookup"><span data-stu-id="19063-108">Navigate toohello folder *boot drive*:\Program Files\Microsoft SQL Remote Blob Storage 10.50\Maintainer\.</span></span>
3. <span data-ttu-id="19063-109">Nevezze át **Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config** túl**web.config**.</span><span class="sxs-lookup"><span data-stu-id="19063-109">Rename **Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config** too**web.config**.</span></span>
4. <span data-ttu-id="19063-110">Használjon `aspnet_regiis -pdf connectionStrings` toodecrypt hello web.config fájlt.</span><span class="sxs-lookup"><span data-stu-id="19063-110">Use `aspnet_regiis -pdf connectionStrings` toodecrypt hello web.config file.</span></span>
5. <span data-ttu-id="19063-111">Hello visszafejtett web.config fájl hello `connectionStrings` csomópont, vegye fel a hello kapcsolati karakterláncot az SQL server-példányt, és hello tartalom-adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="19063-111">In hello decrypted web.config file, under hello `connectionStrings` node, add hello connection string for your SQL server instance and hello content database name.</span></span> <span data-ttu-id="19063-112">Tekintse meg a következő példa hello.</span><span class="sxs-lookup"><span data-stu-id="19063-112">See hello following example.</span></span>
   
    `<add name=”RBSMaintainerConnectionWSSContent” connectionString="Data Source=SHRPT13-SQL12\SHRPT13;Initial Catalog=WSS_Content;Integrated Security=True;Application Name=&quot;Remote Blob Storage Maintainer for WSS_Content&quot;" providerName="System.Data.SqlClient" />`
6. <span data-ttu-id="19063-113">Használjon `aspnet_regiis –pef connectionStrings` toore-hello web.config fájl titkosításához.</span><span class="sxs-lookup"><span data-stu-id="19063-113">Use `aspnet_regiis –pef connectionStrings` toore-encrypt hello web.config file.</span></span> 
7. <span data-ttu-id="19063-114">Nevezze át a web.config tooMicrosoft.Data.SqlRemoteBlobs.Maintainer.exe.config.</span><span class="sxs-lookup"><span data-stu-id="19063-114">Rename web.config tooMicrosoft.Data.SqlRemoteBlobs.Maintainer.exe.config.</span></span> 

#### <a name="tooprepare-hello-content-database-and-recycle-bin-tooimmediately-delete-orphaned-blobs"></a><span data-ttu-id="19063-115">tooprepare hello tartalom-adatbázist és a Lomtár tooimmediately árva Blobok törlése</span><span class="sxs-lookup"><span data-stu-id="19063-115">tooprepare hello content database and Recycle Bin tooimmediately delete orphaned BLOBs</span></span>
1. <span data-ttu-id="19063-116">Futtassa a következő frissítés lekérdezések hello tartalom céladatbázis hello hello SQL Server, az SQL Management Studio eszközben:</span><span class="sxs-lookup"><span data-stu-id="19063-116">On hello SQL Server, in SQL Management Studio, run hello following update queries for hello target content database:</span></span> 
   
       `use WSS_Content`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ’time 00:00:00’`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’time 00:00:00’`
2. <span data-ttu-id="19063-117">A hello webes előtér-kiszolgáló, a **központi adminisztrációs**, hello szerkesztése **webalkalmazás általános beállítások** a hello tartalom-adatbázist tootemporarily letiltása hello Lomtárban van szükség.</span><span class="sxs-lookup"><span data-stu-id="19063-117">On hello web front-end server, under **Central Administration**, edit hello **Web Application General Settings** for hello desired content database tootemporarily disable hello Recycle Bin.</span></span> <span data-ttu-id="19063-118">A művelet hatására is üres hello Lomtár bármely kapcsolódó helygyűjteményekhez.</span><span class="sxs-lookup"><span data-stu-id="19063-118">This action will also empty hello Recycle Bin for any related site collections.</span></span> <span data-ttu-id="19063-119">toodo, kattintson **központi adminisztrációs** -> **Alkalmazáskezelés** -> **webalkalmazások (kezelése webalkalmazások)**  ->  **SharePoint - 80** -> **általános Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="19063-119">toodo this, click **Central Administration** -> **Application Management** -> **Web Applications (Manage web applications)** -> **SharePoint - 80** -> **General Application Settings**.</span></span> <span data-ttu-id="19063-120">Set hello **Recycle Bin állapot** túl**OFF**.</span><span class="sxs-lookup"><span data-stu-id="19063-120">Set hello **Recycle Bin Status** too**OFF**.</span></span>
   
    ![Webalkalmazás általános beállításai](./media/storsimple-sharepoint-adapter-garbage-collection/HCS_WebApplicationGeneralSettings-include.png)

#### <a name="toorun-hello-maintainer"></a><span data-ttu-id="19063-122">toorun hello karbantartó</span><span class="sxs-lookup"><span data-stu-id="19063-122">toorun hello Maintainer</span></span>
* <span data-ttu-id="19063-123">Hello előtér-webkiszolgálón a SharePoint 2013 felügyeleti rendszerhéjat, hello futtassa hello karbantartó az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="19063-123">On hello web front-end server, in hello SharePoint 2013 Management Shell, run hello Maintainer as follows:</span></span>
  
      `Microsoft.Data.SqlRemoteBlobs.Maintainer.exe -ConnectionStringName RBSMaintainerConnectionWSSContent -Operation GarbageCollection -GarbageCollectionPhases rdo`
  
  > [!NOTE]
  > <span data-ttu-id="19063-124">Csak hello `GarbageCollection` művelet StorSimple jelenleg támogatott.</span><span class="sxs-lookup"><span data-stu-id="19063-124">Only hello `GarbageCollection` operation is supported for StorSimple at this time.</span></span> <span data-ttu-id="19063-125">Vegye figyelembe azt is, hogy hello Microsoft.Data.SqlRemoteBlobs.Maintainer.exe kiállított paraméterei és a nagybetűk között.</span><span class="sxs-lookup"><span data-stu-id="19063-125">Also note that hello parameters issued for Microsoft.Data.SqlRemoteBlobs.Maintainer.exe are case sensitive.</span></span> 
  > 
  > 

#### <a name="toorevert-hello-content-database-and-recycle-bin-settings"></a><span data-ttu-id="19063-126">toorevert hello tartalom-adatbázist és beállítások</span><span class="sxs-lookup"><span data-stu-id="19063-126">toorevert hello content database and Recycle Bin settings</span></span>
1. <span data-ttu-id="19063-127">Futtassa a következő frissítés lekérdezések hello tartalom céladatbázis hello hello SQL Server, az SQL Management Studio eszközben:</span><span class="sxs-lookup"><span data-stu-id="19063-127">On hello SQL Server, in SQL Management Studio, run hello following update queries for hello target content database:</span></span>
   
      `use WSS_Content`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ‘days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘orphan_scan_period’ , ’days 30’`
2. <span data-ttu-id="19063-128">Hello előtér-webkiszolgálón a **központi adminisztrációs**, hello szerkesztése **webalkalmazás általános beállítások** a hello tartalom-adatbázist toore-enable hello Lomtárban van szükség.</span><span class="sxs-lookup"><span data-stu-id="19063-128">On hello web front-end server, in **Central Administration**, edit hello **Web Application General Settings** for hello desired content database toore-enable hello Recycle Bin.</span></span> <span data-ttu-id="19063-129">toodo, kattintson **központi adminisztrációs** -> **Alkalmazáskezelés** -> **webalkalmazások (kezelése webalkalmazások)**  ->  **SharePoint - 80** -> **általános Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="19063-129">toodo this, click **Central Administration** -> **Application Management** -> **Web Applications (Manage web applications)** -> **SharePoint - 80** -> **General Application Settings**.</span></span> <span data-ttu-id="19063-130">Állítsa be a hello Recycle Bin állapot túl**ON**.</span><span class="sxs-lookup"><span data-stu-id="19063-130">Set hello Recycle Bin Status too**ON**.</span></span>

