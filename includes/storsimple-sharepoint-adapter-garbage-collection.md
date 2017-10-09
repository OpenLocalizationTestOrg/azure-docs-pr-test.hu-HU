<!--author=SharS last changed: 9/17/15-->

Ezzel az eljárással tartalma:

1. [Készítse elő a toorun hello karbantartó végrehajtható](#to-prepare-to-run-the-maintainer) .
2. [Hello tartalom-adatbázist és a Lomtár előkészítése árva Blobok azonnali törlését](#to-prepare-the-content-database-and-recycle-bin-to-immediately-delete-orphaned-blobs).
3. [Futtassa a Maintainer.exe](#to-run-the-maintainer).
4. [Hello tartalom-adatbázist és a beállítások visszaállításához](#to-revert-the-content-database-and-recycle-bin-settings).

#### <a name="tooprepare-toorun-hello-maintainer"></a>tooprepare toorun hello karbantartó
1. Hello előtér-webkiszolgálón nyissa meg a SharePoint 2013 felügyeleti rendszerhéjat hello rendszergazdaként.
2. Keresse meg a toohello mappa *rendszerindítási meghajtó*: \Program Files\Microsoft SQL távoli Blob Storage 10.50\Maintainer\.
3. Nevezze át **Microsoft.Data.SqlRemoteBlobs.Maintainer.exe.config** túl**web.config**.
4. Használjon `aspnet_regiis -pdf connectionStrings` toodecrypt hello web.config fájlt.
5. Hello visszafejtett web.config fájl hello `connectionStrings` csomópont, vegye fel a hello kapcsolati karakterláncot az SQL server-példányt, és hello tartalom-adatbázis neve. Tekintse meg a következő példa hello.
   
    `<add name=”RBSMaintainerConnectionWSSContent” connectionString="Data Source=SHRPT13-SQL12\SHRPT13;Initial Catalog=WSS_Content;Integrated Security=True;Application Name=&quot;Remote Blob Storage Maintainer for WSS_Content&quot;" providerName="System.Data.SqlClient" />`
6. Használjon `aspnet_regiis –pef connectionStrings` toore-hello web.config fájl titkosításához. 
7. Nevezze át a web.config tooMicrosoft.Data.SqlRemoteBlobs.Maintainer.exe.config. 

#### <a name="tooprepare-hello-content-database-and-recycle-bin-tooimmediately-delete-orphaned-blobs"></a>tooprepare hello tartalom-adatbázist és a Lomtár tooimmediately árva Blobok törlése
1. Futtassa a következő frissítés lekérdezések hello tartalom céladatbázis hello hello SQL Server, az SQL Management Studio eszközben: 
   
       `use WSS_Content`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ’time 00:00:00’`
   
       `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’time 00:00:00’`
2. A hello webes előtér-kiszolgáló, a **központi adminisztrációs**, hello szerkesztése **webalkalmazás általános beállítások** a hello tartalom-adatbázist tootemporarily letiltása hello Lomtárban van szükség. A művelet hatására is üres hello Lomtár bármely kapcsolódó helygyűjteményekhez. toodo, kattintson **központi adminisztrációs** -> **Alkalmazáskezelés** -> **webalkalmazások (kezelése webalkalmazások)**  ->  **SharePoint - 80** -> **általános Alkalmazásbeállítások**. Set hello **Recycle Bin állapot** túl**OFF**.
   
    ![Webalkalmazás általános beállításai](./media/storsimple-sharepoint-adapter-garbage-collection/HCS_WebApplicationGeneralSettings-include.png)

#### <a name="toorun-hello-maintainer"></a>toorun hello karbantartó
* Hello előtér-webkiszolgálón a SharePoint 2013 felügyeleti rendszerhéjat, hello futtassa hello karbantartó az alábbiak szerint:
  
      `Microsoft.Data.SqlRemoteBlobs.Maintainer.exe -ConnectionStringName RBSMaintainerConnectionWSSContent -Operation GarbageCollection -GarbageCollectionPhases rdo`
  
  > [!NOTE]
  > Csak hello `GarbageCollection` művelet StorSimple jelenleg támogatott. Vegye figyelembe azt is, hogy hello Microsoft.Data.SqlRemoteBlobs.Maintainer.exe kiállított paraméterei és a nagybetűk között. 
  > 
  > 

#### <a name="toorevert-hello-content-database-and-recycle-bin-settings"></a>toorevert hello tartalom-adatbázist és beállítások
1. Futtassa a következő frissítés lekérdezések hello tartalom céladatbázis hello hello SQL Server, az SQL Management Studio eszközben:
   
      `use WSS_Content`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘garbage_collection_time_window’ , ‘days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘delete_scan_period’ , ’days 30’`
   
      `exec mssqlrbs.rbs_sp_set_config_value ‘orphan_scan_period’ , ’days 30’`
2. Hello előtér-webkiszolgálón a **központi adminisztrációs**, hello szerkesztése **webalkalmazás általános beállítások** a hello tartalom-adatbázist toore-enable hello Lomtárban van szükség. toodo, kattintson **központi adminisztrációs** -> **Alkalmazáskezelés** -> **webalkalmazások (kezelése webalkalmazások)**  ->  **SharePoint - 80** -> **általános Alkalmazásbeállítások**. Állítsa be a hello Recycle Bin állapot túl**ON**.

