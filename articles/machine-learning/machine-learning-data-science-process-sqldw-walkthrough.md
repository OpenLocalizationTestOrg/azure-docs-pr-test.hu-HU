---
title: "hello Team adatok tudományos folyamat működés közben: az SQL Data Warehouse |} Microsoft Docs"
description: "Bővített Analitikát folyamat és a technológia, működés közben"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 88ba8e28-0bd7-49fe-8320-5dfa83b65724
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;hangzh;weig
ms.openlocfilehash: b1b6371583a023d32e33db59464cafd8c3b767d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="hello-team-data-science-process-in-action-using-sql-data-warehouse"></a>hello Team adatok tudományos folyamat működés közben: az SQL Data Warehouse
Az oktatóanyag azt ismerteti létrehozása és telepítése a gépi tanulási modellek az SQL Data Warehouse (SQL DW) keresztül egy nyilvánosan elérhető adatkészlet – hello [NYC Taxi Utazgatással](http://www.andresmh.com/nyctaxitrips/) adatkészlet. hello bináris osztályozási modell összeállított képes-e tipp fizetnek útnak, és multiclass besorolás és regressziós modell, amely hello terjesztési megjósolható a fizetős hello tipp adatmennyiség is ismertetése.

hello eljárást követi hello [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) munkafolyamat. Megmutatjuk, hogyan toosetup egy tudományos környezetben, hogyan tooload adatok hello SQL DW-be, és hogyan használja az SQL DW vagy egy IPython Notebook tooexplore hello adatok és a visszafejtés szolgáltatások toomodel. Ezután megmutatjuk, hogyan toobuild és a modell Azure Machine Learning segítségével telepíthet.

## <a name="dataset"></a>hello NYC Taxi Utazgatással adatkészlet
hello NYC Taxi út adatok körülbelül 20GB tömörített CSV-fájlok (~ 48GB tömörítetlen) áll, több mint 173 millió rögzítése egyéni utak és hello turistajegyek esetében minden út kifizette. Minden út rekord hello felvétel és Gyűjtőtár helyeket tartalmaz, és időpontokat, anonimizált adatokon alapul (illesztőprogram) licencszám ellophatja, és hello medallion (taxi tartozó egyedi azonosító) száma. hello adatok hello évben 2013 hozzá van rendelve minden való adatváltások számát, és megtalálható a következő két adatkészletet havonta hello:

1. Hello **trip_data.csv** fájl tartalmazza út részleteit, például az utasok, a felvételi és dropoff pontok, út időtartama és út hossza száma. Íme néhány példa rekordok:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. Hello **trip_fare.csv** fájl hello jegy ára minden út, például a fizetési módot, jegy ára összeg, emelt díjas és, tippeket és autópályadíjakra, és kifizetett hello teljes összeg kifizette részleteit tartalmazza. Íme néhány példa rekordok:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Hello **egyedi kulcs** toojoin út használt\_adatok és út\_jegy ára hello a következő három mezőből áll:

* medallion,
* ellophatja\_licenc és
* a felvételi\_datetime.

## <a name="mltasks"></a>Három típusú előrejelzés feladatok cím
A Microsoft hello alapján három előrejelzés problémák állítson össze *tipp\_összeg* tooillustrate három típusú feladatok modellezési:

1. **Bináris osztályozási**: toopredict-e tipp kifizetett utazás, azaz egy *tipp\_összeg* nagyobb, mint 0 egy pozitív példában látható, miközben egy *tipp\_Összeg* $ 0 egy negatív példában látható.
2. **Multiclass besorolási**: hello út fizetett tipp toopredict hello tartományán. A Microsoft hello osztani *tipp\_összeg* öt bins vagy osztályok:
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. **Regressziós feladat**: toopredict hello tipp fizetni útnak.  

## <a name="setup"></a>Hello az Azure data tudományos környezet speciális elemzésekre beállítása
az Azure Adattudomány környezet tooset kövesse az alábbi lépéseket.

**A saját Azure blob storage-fiók létrehozása**

* Ha a saját Azure blob-tároló, válassza az Azure blob storage a vagy a lehető legközelebb a földrajzi helymeghatározás túl**déli középső Régiójában**, vagyis hello NYC Taxi adatok tárolására. hello adatok hello nyilvános blob storage tárolóban tooa tárolót az AzCopy használatával saját tárfiókba kerülnek. hello közelebb az Azure blob storage tooSouth USA középső RÉGIÓJA, hello gyorsabb (4. lépés) Ez a feladat befejeződik.
* toocreate az Azure-tárolóban fiókot, a leírt lépéseket hajtsa végre hello [tudnivalók az Azure storage-fiókok](../storage/common/storage-create-storage-account.md). Lehet, hogy toomake tudnivalók a következő tárfiók hitelesítő adatainak hello értékeit, akkor ez az útmutató későbbi részében szükség lesz.
  
  * **A tárfiók neve**
  * **Tárfiók kulcsa**
  * **Tároló neve** (amelybe hello adatok toobe hello Azure blob Storage tárolóban tárolt)

**Az Azure SQL DW-példányt kiépítéséhez.**
Hajtsa végre a hello dokumentációját a [SQL Data Warehouse létrehozása](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) tooprovision egy SQL Data Warehouse-példányhoz. Győződjön meg arról, hogy, hogy az SQL Data Warehouse hitelesítő adataikat, amelyek a későbbi lépésekben használható a következő hello jelölések.

* **Kiszolgálónév**: <server Name>. database.windows.net
* **SQLDW (adatbázis) neve**
* **Felhasználónév**
* **Jelszó**

**Telepítse a Visual Studio és az SQL Server Data Tools összetevővel.** Útmutatásért lásd: [Visual Studio 2015 telepítése és/vagy az SSDT (SQL Server Data Tools) az SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).

**Kapcsolódás Azure SQL DW tooyour a Visual Studio.** Útmutatásért lásd: 1 és 2 lépéseket [csatlakozzon az SQL Data Warehouse tooAzure Visual Studio](../sql-data-warehouse/sql-data-warehouse-connect-overview.md).

> [!NOTE]
> Futtatási hello következő SQL-lekérdezést a hello adatbázis az SQL Data Warehouse létrehozott (hello lekérdezés hello 3. lépésében megadott helyett csatlakozzon a témakörben) túl**hozzon létre egy főkulcsot**.
> 
> 

    BEGIN TRY
           --Try toocreate hello master key
        CREATE MASTER KEY
    END TRY
    BEGIN CATCH
           --If hello master key exists, do nothing
    END CATCH;

**Hozzon létre egy Azure Machine Learning munkaterülettel alatt az Azure-előfizetéshez.** Útmutatásért lásd: [hozzon létre egy Azure Machine Learning munkaterülettel](machine-learning-create-workspace.md).

## <a name="getdata"></a>Hello adatok betöltése az SQL Data Warehouse
Nyisson meg egy Windows PowerShell-parancs konzolt. Futtassa a következő hello PowerShell parancsokkal toodownload hello példa SQL parancsfájlok, amelyek osztjuk meg Önnel GitHub tooa helyi könyvtárban hello paraméterrel megadott *- DestDir*. Módosíthatja a paraméter értékének hello *- DestDir* tooany helyi könyvtárba. Ha *- DestDir* nem létezik, a rendszer automatikusan létrehozza hello PowerShell-parancsfájl által.

> [!NOTE]
> Szükség lehet túl**Futtatás rendszergazdaként** hello, ha a következő PowerShell-parancsfájl végrehajtása közben a *DestDir* directory rendszergazdai jogosultság toocreate vagy toowrite tooit kell.
> 
> 

    $source = "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/Download_Scripts_SQLDW_Walkthrough.ps1"
    $ps1_dest = "$pwd\Download_Scripts_SQLDW_Walkthrough.ps1"
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_SQLDW_Walkthrough.ps1 –DestDir 'C:\tempSQLDW'

Sikeres végrehajtása után az aktuális munkakönyvtárban módosítja közé tartoznak túl*- DestDir*. Hozzá kell férnie toosee például az alábbi képernyőképen:

![][19]

Az a *- DestDir*, hajtsa végre a következő rendszergazdai jogú PowerShell-parancsfájl hello:

    ./SQLDW_Data_Import.ps1

Ha hello PowerShell-parancsfájl futtatása hello először, kérni fogja tooinput hello információk az Azure SQL DW és az Azure blob storage-fiók. A PowerShell parancsfájl befejeződésekor hello hello hitelesítő adatok első alkalommal fut, bemeneti fog készült tooa konfigurációs fájl SQLDW.conf hello jelen munkakönyvtárba. hello jövőbeli Futtatás a PowerShell parancsfájl hello beállítás tooread valamennyi szükséges paraméterekkel rendelkezik a konfigurációs fájlból. Ha egyes paramétereket kell toochange, dönthet úgy tooinput üdvözlő képernyőt törölni a konfigurációs fájlt, és a bevitel hello paraméterek értékét, kért kérdés vagy toochange hello paraméterértékek hello paraméterek hello SQLDW.conf fájl szerkesztésével az a *- DestDir* könyvtár.

> [!NOTE]
> A sorrend tooavoid séma neve ütközik a már meglévő az Azure SQL DW paraméterek közvetlenül a hello SQLDW.conf fájl olvasásakor 3 számjegyű véletlenszerűen meg van adva toohello sémanév hello SQLDW.conf fájlból hello alapértelmezett séma neve minden egyes futtatásához. PowerShell parancsfájl hello kérheti a séma nevének: felhasználói belátása hello neve adható meg.
> 
> 

Ez **PowerShell-parancsfájl** fájl befejeződött a következő feladatok hello:

* **Letölti és telepíti az AzCopy**, ha az AzCopy nincs telepítve
  
        $AzCopy_path = SearchAzCopy
        if ($AzCopy_path -eq $null){
               Write-Host "AzCopy.exe is not found in C:\Program Files*. Now, start installing AzCopy..." -ForegroundColor "Yellow"
            InstallAzCopy
            $AzCopy_path = SearchAzCopy
        }
            $env_path = $env:Path
            for ($i=0; $i -lt $AzCopy_path.count; $i++){
                if ($AzCopy_path.count -eq 1){
                    $AzCopy_path_i = $AzCopy_path
                } else {
                    $AzCopy_path_i = $AzCopy_path[$i]
                }
                if ($env_path -notlike '*' +$AzCopy_path_i+'*'){
                    Write-Host $AzCopy_path_i 'not in system path, add it...'
                    [Environment]::SetEnvironmentVariable("Path", "$AzCopy_path_i;$env_path", "Machine")
                    $env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine")
                    $env_path = $env:Path
                }
* **Másolja át az adatokat tooyour titkos blob storage-fiók** az AzCopy hello nyilvános blobból
  
        Write-Host "AzCopy is copying data from public blob tooyo storage account. It may take a while..." -ForegroundColor "Yellow"
        $start_time = Get-Date
        AzCopy.exe /Source:$Source /Dest:$DestURL /DestKey:$StorageAccountKey /S
        $end_time = Get-Date
        $time_span = $end_time - $start_time
        $total_seconds = [math]::Round($time_span.TotalSeconds,2)
        Write-Host "AzCopy finished copying data. Please check your storage account tooverify." -ForegroundColor "Yellow"
        Write-Host "This step (copying data from public blob tooyour storage account) takes $total_seconds seconds." -ForegroundColor "Green"
* **Polybase (LoadDataToSQLDW.sql végrehajtásával) tooyour Azure SQL DW használatával adatokat tölt** a következő parancsok hello a titkos blob tároló fiókhoz.
  
  * Hozzon létre egy séma
    
          EXEC (''CREATE SCHEMA {schemaname};'');
  * Egy adatbázishoz kötődő hitelesítő adatok létrehozása
    
          CREATE DATABASE SCOPED CREDENTIAL {KeyAlias}
          WITH IDENTITY = ''asbkey'' ,
          Secret = ''{StorageAccountKey}''
  * Az Azure storage-blob egy külső adatforrás létrehozása
    
          CREATE EXTERNAL DATA SOURCE {nyctaxi_trip_storage}
          WITH
          (
              TYPE = HADOOP,
              LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
              CREDENTIAL = {KeyAlias}
          )
          ;
    
          CREATE EXTERNAL DATA SOURCE {nyctaxi_fare_storage}
          WITH
          (
              TYPE = HADOOP,
              LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
              CREDENTIAL = {KeyAlias}
          )
          ;
  * Hozzon létre egy csv-fájl egy külső fájlformátum. Adatok tömörítetlen és mezők hello függőleges vonal választják el egymástól.
    
          CREATE EXTERNAL FILE FORMAT {csv_file_format}
          WITH
          (   
              FORMAT_TYPE = DELIMITEDTEXT,
              FORMAT_OPTIONS  
              (
                  FIELD_TERMINATOR ='','',
                  USE_TYPE_DEFAULT = TRUE
              )
          )
          ;
  * Külső jegy ára és NYC taxi adatkészlet út táblák létrehozása az Azure blob Storage tárolóban.
    
          CREATE EXTERNAL TABLE {external_nyctaxi_fare}
          (
              medallion varchar(50) not null,
              hack_license varchar(50) not null,
              vendor_id char(3),
              pickup_datetime datetime not null,
              payment_type char(3),
              fare_amount float,
              surcharge float,
              mta_tax float,
              tip_amount float,
              tolls_amount float,
              total_amount float
          )
          with (
              LOCATION    = ''/nyctaxifare/'',
              DATA_SOURCE = {nyctaxi_fare_storage},
              FILE_FORMAT = {csv_file_format},
              REJECT_TYPE = VALUE,
              REJECT_VALUE = 12     
          )  

            CREATE EXTERNAL TABLE {external_nyctaxi_trip}
            (
                   medallion varchar(50) not null,
                   hack_license varchar(50)  not null,
                   vendor_id char(3),
                   rate_code char(3),
                   store_and_fwd_flag char(3),
                   pickup_datetime datetime  not null,
                   dropoff_datetime datetime,
                   passenger_count int,
                   trip_time_in_secs bigint,
                   trip_distance float,
                   pickup_longitude varchar(30),
                   pickup_latitude varchar(30),
                   dropoff_longitude varchar(30),
                   dropoff_latitude varchar(30)
            )
            with (
                LOCATION    = ''/nyctaxitrip/'',
                DATA_SOURCE = {nyctaxi_trip_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12         
            )

    - Adatok betöltése az Azure blob storage tooSQL adatraktár külső táblák

            CREATE TABLE {schemaname}.{nyctaxi_fare}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_fare}
            ;

            CREATE TABLE {schemaname}.{nyctaxi_trip}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_trip}
            ;

    - Hozzon létre egy minta-adattábla (NYCTaxi_Sample), és adatok tooit beszúrása kiválasztása az SQL-lekérdezések hello út és a jegy ára táblákon. (Ez az útmutató néhány lépése kell toouse meg ez a minta a táblázat.)

            CREATE TABLE {schemaname}.{nyctaxi_sample}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            (
                SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount,
                tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
                tip_class = CASE
                        WHEN (tip_amount = 0) THEN 0
                        WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                        WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                        WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                        ELSE 4
                    END
                FROM {schemaname}.{nyctaxi_trip} t, {schemaname}.{nyctaxi_fare} f
                WHERE datepart("mi",t.pickup_datetime) = 1
                AND t.medallion = f.medallion
                AND   t.hack_license = f.hack_license
                AND   t.pickup_datetime = f.pickup_datetime
                AND   pickup_longitude <> ''0''
                AND   dropoff_longitude <> ''0''
            )
            ;

a storage-fiókok hello földrajzi helye a betöltési idők hatással van.

> [!NOTE]
> Attól függően, hogy a titkos blob storage-fiók földrajzi elhelyezkedése hello, hello folyamat adatok másolása egy nyilvános blob tooyour titkos tárfiókot is körülbelül 15 percet vesz igénybe, vagy akár már és hello feldolgozni az adatokat tölt be a tárfiók Azure SQL DW tooyour 20 percet vehet igénybe, vagy hosszabb.  
> 
> 

Hogy toodecide milyen műveleteket, ha ismétlődő a forrás és cél fájlok.

> [!NOTE]
> Ha hello .csv fájlok toobe átmásolva hello nyilvános blob storage tooyour titkos blob storage-fiók már létezik a személyes blob storage-fiók, AzCopy rákérdez, hogy szeretné-e toooverwrite őket. Ha nem szeretné, hogy toooverwrite őket, bemeneti  **n**  megjelenésekor. Ha azt szeretné, hogy toooverwrite **összes** , bemeneti **egy** megjelenésekor. Is szövegszerkesztőben **y** toooverwrite .csv fájlok külön-külön.
> 
> 

![#21. megrajzolásához][21]

Használhatja a saját adatait. Ha az adatok a valós életben alkalmazásban a helyszíni gépen, AzCopy tooupload a helyszíni adatok tooyour titkos Azure blob storage továbbra is használhatja. Csak akkor kell toochange hello **forrás** helyét, `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, az AzCopy parancs hello PowerShell parancsfájl fájl toohello helyi könyvtárat, az adatokat tartalmazó hello.

> [!TIP]
> Ha az adatok már a saját Azure blob Storage a valós életben alkalmazásban, kihagyhatja hello AzCopy hello PowerShell-parancsfájl lépést, és közvetlenül a hello adatok tooAzure SQL DW feltöltése. Ehhez szükséges további szerkeszti a hello parancsfájl tootailor azt toohello adatformátum.
> 
> 

A Powershell-parancsfájlt is csatlakoztatja az hello adatfájlokba hello feltárása példa SQLDW_Explorations.sql, SQLDW_Explorations.ipynb és SQLDW_Explorations_Scripts.py Azure SQL DW-információkat, hogy ezek a fájlok próbált azonnal készen toobe PowerShell parancsfájl hello után.

Egy sikeres végrehajtása után képernyőn megjelenik alá, például:

![][20]

## <a name="dbexplore"></a>Az adatok feltárása, a szolgáltatás fejlesztés az Azure SQL Data Warehouse
Ebben a szakaszban szemben Azure SQL DW közvetlenül az SQL-lekérdezések futtatásával végezzük adatok feltárása és a szolgáltatás generálása **Visual Studio Data Tools**. Ebben a szakaszban használt összes SQL-lekérdezések nevű hello mintaparancsfájl található *SQLDW_Explorations.sql*. Ez a fájl már letöltött tooyour helyi könyvtár hello PowerShell-parancsprogram. Azt is megtalálja a [GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql). De hello fájl a Githubban nincs csatlakoztatva hello Azure SQL DW-információ.

Csatlakozás tooyour Azure SQL DW hello SQL DW-bejelentkezési nevet és jelszót a Visual Studio használatával, és nyissa meg a hello **SQL Object Explorer** tooconfirm hello adatbázis és tábla nincs importálva. Hello beolvasása *SQLDW_Explorations.sql* fájlt.

> [!NOTE]
> a Parallel Data Warehouse (PDW) Lekérdezésszerkesztő tooopen hello használata **új lekérdezés** parancsot a PDW kiválasztása a hello **SQL Object Explorer**. szabványos SQL Lekérdezésszerkesztő hello PDW által nem támogatott.
> 
> 

Az alábbiakban hello típusú adatok feltárása és a szolgáltatás generálása feladatok végre ebben a szakaszban:

* Megismerkedhet a különböző idő windows néhány mezőinek disztribúciók adatok.
* Vizsgálja meg az adatok minőségének hello szélességi és hosszúsági mezőket.
* Hello alapján multiclass és bináris besorolási címkék létrehozása **tipp\_összeg**.
* Szolgáltatások létrehozása és számítási/összehasonlítása út távolság.
* Csatlakozás hello két táblák, és bontsa ki a használt toobuild modellek leendő véletlenszerű minta.

### <a name="data-import-verification"></a>Az importálás ellenőrzése
Ezeket a lekérdezéseket, adja meg a sorok és oszlopok a táblák fel korábban segítségével a Polybase által párhuzamos tömeges importálásához hello hello száma gyors ellenőrzése

    -- Report number of rows in table <nyctaxi_trip> without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')

    -- Report number of columns in table <nyctaxi_trip>
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = '<nyctaxi_trip>' AND table_schema = '<schemaname>'

**Kimenet:** 173,179,759 sorok és oszlopok 14 kapja meg.

### <a name="exploration-trip-distribution-by-medallion"></a>Feltárása: Út elosztása medallion szerint
Ez a példa lekérdezés hello medallions (taxi számok), 100-nál több való adatváltások számát egy adott időszakon belül elvégzett azonosítja. hello lekérdezés előnyös hello particionált tábla hozzáférés óta, akkor annak hello partíciós sémája által **a felvételi\_datetime**. Hello teljes adatkészlet lekérdezése is teszi particionált tábla hello használata és/vagy vizsgálat index.

    SELECT medallion, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

**Kimenet:** hello lekérdezés kell egy olyan tábla visszaadása hello 13,369 medallions (taxikra) megadása sorokból és hello 2013 befejezte út száma. hello utolsó oszlop hello számát hello befejeződött való adatváltások számát tartalmazza.

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Feltárása: Út terjesztési medallion és hack_license
Ez a példa hello medallions (taxi számok) azonosítja, és hack_license számok (-illesztőprogramok) az elvégzett több mint 100 való adatváltások számát egy adott időszakon belül.

    SELECT medallion, hack_license, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

**Kimenet:** hello lekérdezés visszaadja-e 13,369 sorait hello 13,369 autó/illesztőprogram elvégző több adott 100 utazgatással 2013 azonosítók megadása egy tábla. hello utolsó oszlop hello számát hello befejeződött való adatváltások számát tartalmazza.

### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Adatok vizsgálatának: helytelen hosszúsági és/vagy szélességi rekordjainak ellenőrzése
Ez a példa pedig megvizsgálja, ha bármelyik hello hosszúsági és/vagy szélességi mezők vagy érvénytelen értéket tartalmaz (radián fok -90 és 90 között kell lennie), vagy (0, 0) koordinátarendszerében.

    SELECT COUNT(*) FROM <schemaname>.<nyctaxi_trip>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

**Kimenet:** hello lekérdezés érvénytelen hosszúsági és/vagy szélességi mezői lehetnek 837,467 való adatváltások számát adja vissza.

### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Feltárása: Nem Formabontó utazgatással terjesztési és Formabontó
Ebben a példában hello száma való adatváltások számát, amely volt Formabontó és hello számát, amely nem volt Formabontó, egy adott időszakra vonatkozó (vagy a teljes adatkészlet hello Ha kiterjedő hello teljes év van beállítva itt) talál. Ehhez a terjesztéshez hello bináris címke terjesztési toobe később használatos bináris osztályozási modellezési tükrözi.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM <schemaname>.<nyctaxi_fare>
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

**Kimenet:** hello lekérdezés kell visszatérési hello következő tipp gyakoriságot hello év 2013: 90,447,622 Formabontó és nem Formabontó 82,264,709.

### <a name="exploration-tip-classrange-distribution"></a>Feltárása: Tipp osztály/tartomány terjesztési
Ebben a példában egy adott idő alatt hello tipp tartományok terjesztése kiszámítja az időszak (vagy a teljes adatkészletet hello Ha hello teljes évre vonatkozó). Ez a hello címke osztályokkal rendelkezik, amelyek később használandó multiclass besorolás modellezési hello terjesztése.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

**A kimenetre:**

| tip_class | tip_freq |
| --- | --- |
| 1 |82230915 |
| 2 |6198803 |
| 3 |1932223 |
| 0 |82264625 |
| 4 |85765 |

### <a name="exploration-compute-and-compare-trip-distance"></a>Feltárása: Számítási, és hasonlítsa össze a út távolság
Ebben a példában a felvételi és Gyűjtőtár hosszúság hello alakítja át, és tooSQL a földrajzi szélesség mutat, hello út távolság SQL geográfiai pontok különbség használatával kiszámítja és visszaadja az összehasonlításhoz hello eredmények véletlenszerű minta. hello példa eredményekre hello toovalid koordinálja, csak a hello minőségi assessment adatlekérdezés kezelt korábban.

    /****** Object:  UserDefinedFunction [dbo].[fnCalculateDistance] ******/
    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function toocalculate hello direct distance  in mile between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
          DECLARE @distance decimal(28, 10)
          -- Convert tooradians
          SET @Lat1 = @Lat1 / 57.2958
          SET @Long1 = @Long1 / 57.2958
          SET @Lat2 = @Lat2 / 57.2958
          SET @Long2 = @Long2 / 57.2958
          -- Calculate distance
          SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
          --Convert toomiles
          IF @distance <> 0
          BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
          END
          RETURN @distance
    END
    GO

    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

### <a name="feature-engineering-using-sql-functions"></a>A szolgáltatás műszaki osztály SQL függvények használata
SQL-függvény néha lehet a hatékony beállítás a szolgáltatás mérnöki csapathoz. Ebben a bemutatóban meghatározott SQL függvény toocalculate hello közvetlen távolság hello felvétel és a dropoff között. Futtathatja a következő SQL-parancsfájlok a hello **Visual Studio Data Tools**.

Itt található, amely meghatározza a hello távolság függvény hello SQL-parancsfájlt.

    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function calculate hello direct distance between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
          DECLARE @distance decimal(28, 10)
          -- Convert tooradians
          SET @Lat1 = @Lat1 / 57.2958
          SET @Long1 = @Long1 / 57.2958
          SET @Lat2 = @Lat2 / 57.2958
          SET @Long2 = @Long2 / 57.2958
          -- Calculate distance
          SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
          --Convert toomiles
          IF @distance <> 0
          BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
          END
          RETURN @distance
    END
    GO

Íme egy példa toocall a függvény toogenerate szolgáltatás az SQL-lekérdezésben:

    -- Sample query toocall hello function toocreate features
    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

**Kimenet:** Ez a lekérdezés az (2,803,538 sorok) tábla a felvétel és dropoff földrajzi szélesség hoz létre, és a hosszúságot, a megfelelő hello közvetlen miles megadott. Az alábbiakban az első 3 sort az hello eredménye:

|  | pickup_latitude | pickup_longitude | dropoff_latitude | dropoff_longitude | DirectDistance |
| --- | --- | --- | --- | --- | --- |
| 1 |40.731804 |-74.001083 |40.736622 |-73.988953 |.7169601222 |
| 2 |40.715794 |-74,010635 |40.725338 |-74.00399 |.7448343721 |
| 3 |40.761456 |-73.999886 |40.766544 |-73.988228 |0.7037227967 |

### <a name="prepare-data-for-model-building"></a>Adatok előkészítése a modell létrehozásának
hello következő lekérdezés illesztések hello **nyctaxi\_út** és **nyctaxi\_jegy ára** táblák, állít elő, a bináris besorolási címkék **Formabontó**, egy több osztály besorolási címke **tipp\_osztály**, és egy minta kiolvassa a hello teljes illesztett adatkészlethez. hello mintavétel felvételi ideje alapján hello utazgatással részhalmazának lekérésével.  Ez a lekérdezés másolja, majd közvetlenül az hello beillesztett [Azure Machine Learning Studio](https://studio.azureml.net) [és adatokat importálhat] [ import-data] hello SQL database-példányt a közvetlen adatfeldolgozást-modul az Azure. hello lekérdezés nem tartalmazza a rekordok helytelen (0, 0) koordinátarendszerében.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,     f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
    WHERE datepart("mi",t.pickup_datetime) = 1
    AND   t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

Amikor készen áll a tooproceed tooAzure gépi tanulás, akkor előfordulhat, hogy vagy:  

1. Hello végső SQL lekérdezés tooextract és minta hello adatok és a másolás-beillesztés hello lekérdezés mentése közvetlenül egy [és adatokat importálhat] [ import-data] modul az Azure Machine Learning, vagy
2. Hello mintát megmaradnak, és azt tervezi, hogy az új SQL-Adatraktár létrehozása modell toouse visszafejtett adatok tábla, és a hello új tábla hello [és adatokat importálhat] [ import-data] az Azure Machine Learning modulban. PowerShell-parancsfájl a korábbi lépésben hello végzett ezt meg. Ez a táblázat hello adatokat importálhat modul közvetlenül a olvashatja.

## <a name="ipnb"></a>Az adatok feltárása, a szolgáltatás fejlesztés IPython jegyzetfüzet
Ebben a szakaszban végezzük el az adatok feltárása és a szolgáltatás generálása mindkét pythonos környezetekben, és a korábban létrehozott SQL DW hello SQL-lekérdezéseket. Egy minta IPython notebook nevű **SQLDW_Explorations.ipynb** és a Python-parancsfájl **SQLDW_Explorations_Scripts.py** letöltött tooyour helyi könyvtárba lett volna. Akkor is elérhetők a [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW). Ezeket a fájlokat két Python parancsfájlok azonosak. hello Python-parancsfájl tooyou valósul meg, abban az esetben, ha nem rendelkezik egy IPython Notebook kiszolgálót. Ez a két minta úgy vannak beállítva, a Python **Python 2.7**.

hello hello mintában IPython Notebook szükséges Azure SQL DW-adatokat, és a Python-parancsfájl fájl letöltött tooyour helyi számítógép rendelkezik lett csatlakoztatva hello PowerShell-parancsfájl által korábban hello. Azok a módosítás nélkül végrehajtható.

Ha már az AzureML-munkaterülethez, közvetlenül hello minta IPython Notebook toohello AzureML IPython Notebook szolgáltatás feltöltése és elindultak, azt. Az alábbiakban hello lépéseket tooupload tooAzureML IPython Notebook szolgáltatás:

1. Jelentkezzen be tooyour AzureML munkaterület, hello tetején kattintson az "Studio" és "NOTEBOOKOK" hello hello weblap bal oldalán kattintson.
   
    ![#22 ábrázolása][22]
2. Kattintson az "Új" hello weblap hello bal alsó sarkában, és jelölje ki "Python 2". Ezt követően adjon meg egy nevet toohello notebook, és hello pipa toocreate hello új üres IPython Notebook kattintson.
   
    ![#23 ábrázolása][23]
3. Kattintson a bal felső sarkának hello hello hello "Jupyter" szimbólum IPython új Notebookot.
   
    ![#24 ábrázolása][24]
4. Hello minta IPython Notebook toohello áthúzása **fa** az AzureML IPython Notebook szolgáltatást, majd kattintson a lap **feltöltése**. Ezt követően a hello minta IPython Notebook feltöltött toohello AzureML IPython Notebook szolgáltatás lesz.
   
    ![#25 ábrázolása][25]

A sorrend toorun hello minta IPython Notebook vagy hello Python-parancsfájl, hello következő Python csomagok szükségesek. Ha hello AzureML IPython Notebook szolgáltatást használja, ezeket a csomagokat manuálisan telepítette.

    - pandas
    - numpy
    - matplotlib
    - pyodbc
    - PyTables

ajánlott az sorozat építve speciális elemzési megoldásokat AzureML nagy adatokkal hello hello következő:

* Olvassa el egy a memóriában levő keretbe hello adatok mintát.
* Hajtsa végre az egyes képi megjelenítéseket, és segítségével explorations hello mintaadatokat.
* Összehasonlítást az adatminta szolgáltatás mérnöki hello segítségével a kísérletet.
* A nagyobb az adatok feltárása a adatkezelési és a szolgáltatás mérnöki csapathoz, Python tooissue SQL lekérdezések közvetlenül hello SQL DW használjon.
* Döntse el, hello minta mérete toobe Azure Machine Learning modell létrehozásának alkalmas.

hello következőket a következők: néhány adatok feltárása, adatábrázolási és a szolgáltatás mérnöki példák. További adatok explorations hello minta IPython jegyzetfüzet és hello minta Python-parancsfájl találhatók.

### <a name="initialize-database-credentials"></a>Adatbázis-hitelesítő adatok inicializálása
Az adatbázis-kapcsolati beállításokat a következő változók hello inicializálása:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database driver>

### <a name="create-database-connection"></a>Adatbázis-kapcsolat létrehozása
Itt található hello kapcsolati karakterlánc, amely hello kapcsolat toohello adatbázist hoz létre.

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Sorok és oszlopok a tábla < nyctaxi_trip > jelentés száma
    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_trip>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

* A sorok teljes számát = 173179759  
* Az oszlopok teljes száma = 14

### <a name="report-number-of-rows-and-columns-in-table-nyctaxifare"></a>Sorok és oszlopok a tábla < nyctaxi_fare > jelentés száma
    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_fare>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_fare>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

* A sorok teljes számát = 173179759  
* Az oszlopok teljes száma = 11

### <a name="read-in-a-small-data-sample-from-hello-sql-data-warehouse-database"></a>Olvassa el a hello SQL-adatraktár-adatbázis egy kisméretű mintát
    t0 = time.time()

    query = '''
        SELECT TOP 10000 t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
        WHERE datepart("mi",t.pickup_datetime) = 1
        AND   t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time tooread hello sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Idő tooread hello mintatáblázat: 14.096495 másodperc.  
A beolvasott sorok és oszlopok száma = (1000, 21).

### <a name="descriptive-statistics"></a>Leíró statisztika
Most már áll készen tooexplore hello mintavételezett adatokat. Először néhány leíró statisztikája hello megnézi **út\_távolság** (vagy bármely más mezők kiválasztása toospecify).

    df1['trip_distance'].describe()

### <a name="visualization-box-plot-example"></a>A képi megjelenítés: Rajzot – példa
Ezután úgy tekintünk hello Dobozdiagram hello út távolság toovisualize hello quantiles számára.

    df1.boxplot(column='trip_distance',return_type='dict')

![Tőzsdei #1][1]

### <a name="visualization-distribution-plot-example"></a>A képi megjelenítés: Terjesztési rajzot – példa
Felvétel, megjelenítheti a hello terjesztési és hello a hisztogram út távolság mintát.

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Tőzsdei #2][2]

### <a name="visualization-bar-and-line-plots"></a>A képi megjelenítés: Sáv megnyitásához, majd sor ábra
Ebben a példában a Microsoft hello út távolság az öt bins bin és hello dobozolás eredmények megjelenítése.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

A Microsoft hello fent egy sávon bin terjesztési megrajzolásához, vagy a rajzolási. sor:

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![#3 ábrázolása][3]

és

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![#4 ábrázolása][4]

### <a name="visualization-scatterplot-examples"></a>A képi megjelenítés: Scatterplot példák
Pontdiagram rajzot közötti megmutatjuk **út\_idő\_a\_másodperc** és **út\_távolság** toosee bármely korrelációs esetén

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![#6 ábrázolása][6]

Hasonlóképpen azt hello közötti kapcsolat ellenőrzése **arány\_kód** és **út\_távolság**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![#8 ábrázolása][8]

### <a name="data-exploration-on-sampled-data-using-sql-queries-in-ipython-notebook"></a>Az adatok feltárása a mintaadatokat az SQL-lekérdezések IPython notebook használatával
Ebben a részben azt megismerkedhet adatok azokat a terjesztéseket használatával mintát hello adat, amely a fenti létrehozott hello új tábla megőrződjenek. Vegye figyelembe, hogy hasonló explorations végrehajtható hello eredeti táblák használata.

#### <a name="exploration-report-number-of-rows-and-columns-in-hello-sampled-table"></a>Feltárása: Sorok és oszlopok hello a jelentés száma mintát venni tábla
    nrows = pd.read_sql('''SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_sample>')''', conn)
    print 'Number of rows in sample = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''SELECT count(*) FROM information_schema.columns WHERE table_name = ('<nyctaxi_sample>') AND table_schema = '<schemaname>'''', conn)
    print 'Number of columns in sample = %d' % ncols.iloc[0,0]

#### <a name="exploration-tippednot-tripped-distribution"></a>Feltárása: A Formabontó/nem terjesztési azokat kioldják
    query = '''
        SELECT tipped, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tipped
        '''

    pd.read_sql(query, conn)

#### <a name="exploration-tip-class-distribution"></a>Feltárása: Tipp osztály terjesztési
    query = '''
        SELECT tip_class, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tip_class
    '''

    tip_class_dist = pd.read_sql(query, conn)

#### <a name="exploration-plot-hello-tip-distribution-by-class"></a>Feltárása: Hello tipp terjesztési megrajzolásához osztály
    tip_class_dist['tip_freq'].plot(kind='bar')

![#26 ábrázolása][26]

#### <a name="exploration-daily-distribution-of-trips"></a>Feltárása: Való adatváltások számát napi eloszlásáról
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Feltárása: Utazás per medallion terjesztési
    query = '''
        SELECT medallion,count(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>Feltárása: Út terjesztési medallion és rejthetők el licenc által
    query = '''select medallion, hack_license,count(*) from <schemaname>.<nyctaxi_sample> group by medallion, hack_license'''
    pd.read_sql(query,conn)


#### <a name="exploration-trip-time-distribution"></a>Feltárása: Út idő terjesztése
    query = '''select trip_time_in_secs, count(*) from <schemaname>.<nyctaxi_sample> group by trip_time_in_secs order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-trip-distance-distribution"></a>Feltárása: Út távolság terjesztési
    query = '''select floor(trip_distance/5)*5 as tripbin, count(*) from <schemaname>.<nyctaxi_sample> group by floor(trip_distance/5)*5 order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-payment-type-distribution"></a>Feltárása: Fizetési típus terjesztési
    query = '''select payment_type,count(*) from <schemaname>.<nyctaxi_sample> group by payment_type'''
    pd.read_sql(query,conn)

#### <a name="verify-hello-final-form-of-hello-featurized-table"></a>Hello végső űrlap hello featurized tábla ellenőrzése
    query = '''SELECT TOP 100 * FROM <schemaname>.<nyctaxi_sample>'''
    pd.read_sql(query,conn)

## <a name="mlmodel"></a>Az Azure Machine Learning modellek létrehozása
Dolgozunk most már készen áll a tooproceed toomodel épület és a modell telepítési [Azure Machine Learning](https://studio.azureml.net). hello adata készen áll, nevezetesen korábban azonosított hello előrejelzés problémák valamelyikében használt toobe:

1. **Bináris osztályozási**: toopredict tipp egy út kifizetett-e.
2. **Multiclass besorolási**: toopredict hello tartomány tipp fizetős, toohello korábban definiált osztályok szerint.
3. **Regressziós feladat**: toopredict hello tipp fizetni útnak.  

toobegin hello modellezési gyakorlat, jelentkezzen be tooyour **Azure Machine Learning** munkaterületen. Ha még nem hozott létre machine learning-munkaterület, lásd: [hozzon létre egy Azure ML munkaterületet](machine-learning-create-workspace.md).

1. az Azure Machine Learning lépései tooget lásd: [Mi az Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)
2. Jelentkezzen be túl[Azure Machine Learning Studio](https://studio.azureml.net).
3. hello Studio kezdőlap számos olyan információt, videók, oktatóanyagok, hivatkozások toohello modulok hivatkozás és egyéb erőforrásokat biztosít. Azure Machine Learning kapcsolatos további információkért tekintse meg a hello [Azure Machine Learning dokumentációs központban](https://azure.microsoft.com/documentation/services/machine-learning/).

Egy tipikus tanítási kísérletet hello lépéseket foglalja magában:

1. Hozzon létre egy **+ új** kipróbálásához.
2. Azure ml hello adatok beolvasása.
3. Előre feldolgozzák, átalakítás és kezelhetők hello adatok, igény szerint.
4. Szolgáltatások létrehozása, igény szerint.
5. Hello adatok felosztása adatkészletek képzési/érvényesítési/tesztelése (külön adatkészleteket, vagy az egyes).
6. Válasszon egy vagy több számítógépet tanulási algoritmus attól függően, hogy a probléma toosolve tanulási hello. Például bináris osztályozási multiclass besorolás, regressziós.
7. Hello képzési adatkészlet egy vagy több modell betanításához.
8. Pontszám hello érvényesítési dataset hello betanított modellek segítségével.
9. Hello modellek toocompute hello vonatkozó metrikáinak probléma tanulási hello kiértékeléséhez.
10. Konfigurálva finomhangolhatják a hello modellek és select hello legjobb modell toodeploy.

Ebben a gyakorlatban azt már megismerkedett és fejthetők vissza az SQL Data Warehouse hello adatokat, és úgy döntött, a hello minta mérete tooingest Azure ml. Hello eljárás toobuild Ez egy vagy több hello előrejelzési modellek:

1. Hello adatok beolvasása az Azure ml hello segítségével [és adatokat importálhat] [ import-data] modul hello elérhető **bemeneti és kimeneti** szakasz. További információkért lásd: hello [és adatokat importálhat] [ import-data] modul referencialapja.
   
    ![Azure ML importálási adatok][17]
2. Válassza ki **Azure SQL Database** , hello **adatforrás** a hello **tulajdonságok** panel.
3. Adja meg a hello adatbázis DNS-név hello **adatbázis-kiszolgáló neve** mező. Formátum:`tcp:<your_virtual_machine_DNS_name>,1433`
4. Adja meg a hello **adatbázisnév** hello megfelelő mezőben.
5. Adja meg a hello *SQL felhasználónév* a hello **kiszolgáló felhasználói fiók nevét**, és hello *jelszó* hello a **kiszolgáló felhasználói fiók jelszavát**.
6. Ellenőrizze a hello **fogadja el a kiszolgálói tanúsítvány** lehetőséget.
7. A hello **adatbázis-lekérdezés** szerkesztése területre, és beillesztheti hello szükséges adatbázis-(beleértve a számított mezőket például hello címkék) mezőt, és le kivonatok minták hello adatok szükséges toohello mintaméret hello lekérdezés.

Példa egy bináris osztályozási kísérletet, közvetlenül a hello SQL Data Warehouse-adatbázis adatainak olvasása: hello az alábbi ábra a (ne feledje tooreplace hello táblázatok neve nyctaxi_trip és nyctaxi_fare hello séma által használt nevét és hello táblanevek a a forgatókönyv). Hasonló kísérletek multiclass besorolás és a visszavonási lehet létrehozni.

![Az Azure ML vonat][10]

> [!IMPORTANT]
> Az adatok kinyerése modellezési, és a korábbi szakaszokban bemutatott lekérdezés példák mintavételi hello **hello három modellezési gyakorlatok az összes felirathoz szerepelnek hello lekérdezés**. Az egyes gyakorlatok modellezési hello (kötelező) fontos lépés túl van**kizárása** hello szükségtelen címkéit hello más két problémákat, és minden egyéb **kiszivárgásának cél**. Például bináris osztályozási használatakor használjon hello címke **Formabontó** , és amelyeket hello mezők **tipp\_osztály**, **tipp\_összeg**, és **teljes\_összeg**. Ez utóbbi hello cél kiszivárgásának, mivel azok utalnak hello tipp fizetett.
> 
> tooexclude bármely felesleges oszlopok vagy a cél kiszivárgásának hello segítségével [Select Columns in Dataset] [ select-columns] modul vagy hello [szerkesztése metaadatok] [ edit-metadata]. További információkért lásd: [Select Columns in Dataset] [ select-columns] és [szerkesztése metaadatok] [ edit-metadata] lapok hivatkoznak.
> 
> 

## <a name="mldeploy"></a>Az Azure Machine Learning modellek telepítése
Amikor készen áll a modell, könnyedén telepítheti azt közvetlenül a hello kísérlet webszolgáltatásként. Azure ML web services telepítésével kapcsolatos további információkért lásd: [központi telepítése az Azure Machine Learning webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md).

egy új webszolgáltatás-bővítmény toodeploy, kell:

1. A pontozási kísérlet létrehozása.
2. Hello webes szolgáltatás üzembe helyezése.

a pontozási kísérletezhet a toocreate egy **befejezett** betanítása kísérletet, kattintson **létrehozása pontozási KÍSÉRLETEZHET** hello alacsonyabb művelet sávon.

![Pontozás az Azure][18]

Az Azure Machine Learning toocreate egy pontozási kísérletet hello tanítási kísérletet hello összetevői alapján kísérli meg. Különösen a következőket hajtja végre:

1. Hello betanított modell mentéséhez, és távolítsa el a hello modell képzési modulok.
2. Azonosítsa a logikai **bemeneti port** toorepresent hello várt bemeneti adatok séma.
3. Azonosítsa a logikai **kimeneti port** toorepresent hello várt webes szolgáltatás kimeneti sémával.

Kísérlet pontozási hello létrehozásakor, tekintse át, és ellenőrizze, hogy szükség szerint állítsa be. Egy tipikus módosítás tooreplace hello bemeneti adatkészlet és/vagy entitáselem, amely kizárja a címke mezők esetén, mert ezek nem lesz elérhető hello szolgáltatás meghívásakor. Akkor is hello célszerű tooreduce hello méretet adjon meg adatkészlet és/vagy lekérdezési tooa néhány rekordjának, éppen elegendő tooindicate hello bemeneti sémát. A hello kimeneti portra, azt közös tooexclude összes beviteli mezőt, és csak terjed hello **pontozott címkék** és **program pontozza a mennyiségeket valószínűség** hello hello használja a [Select Columns in Dataset ] [ select-columns] modul.

Kísérlet pontozási minta hello az alábbi ábra valósul meg. Ha készen áll a toodeploy, kattintson a hello **WEBSZOLGÁLTATÁS közzététele** hello alacsonyabb művelet található gombra.

![Az Azure ML közzététele][11]

## <a name="summary"></a>Összefoglalás
toorecap mi azt megtette az bemutató oktatóanyag, hozott létre az Azure data tudományos környezethez, a nagy nyilvános dataset dolgozott véve a csapat az tudományos folyamata, beszerzési toomodel betanítási adatok, az összes hello módon hello keresztül, majd az Azure Machine Learning webszolgáltatás üzembe helyezése toohello.

### <a name="license-information"></a>Licencinformációk
Ez a minta forgatókönyv és a mellékelt parancsfájlok és IPython notebook(s) által megosztott Microsoft hello MIT licence. Ellenőrizze hello LICENSE.txt fájl hello Directory hello mintakód a Githubon további részleteket.

## <a name="references"></a>Referencia
• [Andrés Monroy NYC Taxi utak letöltési oldala](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing NYC taxiköltség út adatok Chris Whong által](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC Taxi és Limousine Bizottság kutatási és statisztika](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)

[1]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sqldw-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sqldw-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlscoring.png
[19]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_download_scripts.png
[20]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_load_data.png
[21]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azcopy-overwrite.png
[22]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-1.png
[23]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-2.png
[24]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-3.png
[25]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-4.png
[26]: ./media/machine-learning-data-science-process-sqldw-walkthrough/tip_class_hist_1.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
