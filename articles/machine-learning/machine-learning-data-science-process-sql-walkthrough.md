---
title: "aaaBuild és a gépi tanulási modellek használata az SQL Server egy Azure virtuális gépen telepítése |} Microsoft Docs"
description: "Bővített Analitikát folyamat és a technológia, működés közben"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 6066b083-262c-4453-a712-a5c05acc3df8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: fashah;bradsev
ms.openlocfilehash: 30ba9a9e3cf65f75015e13f9c7876dcbccc5bc47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="hello-team-data-science-process-in-action-using-sql-server"></a>hello Team adatok tudományos folyamat működés közben: SQL Server használata
Az oktatóanyag ismerteti hello folyamatot, amely létrehozása és telepítése a gépi tanulási modellek használata az SQL Server és a nyilvánosan elérhető dataset – hello [NYC Taxi Utazgatással](http://www.andresmh.com/nyctaxitrips/) adatkészlet. hello eljárást követi a szabványos adatelemezési munkafolyamatot: betöltési és hello adatokba, tervezését szolgáltatások toofacilitate tanulási, majd build és a modell rendszerbe állítása.

## <a name="dataset"></a>NYC Taxi utazás közben adatkészlet leírása
hello NYC Taxi út adatok körülbelül 20GB tömörített CSV-fájlok (tömörítetlen ~ 48GB), amely több mint 173 millió egyedi utak és hello turistajegyek esetében minden út kifizette. Minden út rekord tartalmazza hello felvétel és Gyűjtőtár hely és idő, anonimizált rejthetők el (illesztőprogram) engedély száma és medallion (taxi tartozó egyedi azonosító) számát. hello adatok hello évben 2013 hozzá van rendelve minden való adatváltások számát, és megtalálható a következő két adatkészletet havonta hello:

1. hello "trip_data" CSV út részleteit, például a utasok, a felvételi és dropoff pontok, út időtartama és út hossza tartalmazza. Íme néhány példa rekordok:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. hello "trip_fare" CSV hello jegy ára minden út, például a fizetési módot, jegy ára összeg, emelt díjas és, tippeket és autópályadíjakra, és kifizetett hello teljes összeg kifizette részleteit tartalmazza. Íme néhány példa rekordok:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

hello egyedi kulcs toojoin út\_adatok és út\_jegy ára áll hello mezők: medallion, rejthetők el\_engedély és a felvételi\_datetime.

## <a name="mltasks"></a>Előrejelzés feladatok példák
Azt a három előrejelzés problémák hello alapján fogalmaz *tipp\_összeg*, nevezetesen:

1. Bináris osztályozás: előre jelezni, függetlenül attól, tipp kifizetett utazás, azaz egy *tipp\_összeg* nagyobb, mint 0 egy pozitív példában látható, miközben egy *tipp\_összeg* $ 0 van egy negatív példa.
2. Multiclass besorolási: hello út fizetett tipp toopredict hello tartományán. A Microsoft hello osztani *tipp\_összeg* öt bins vagy osztályok:
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. Regressziós feladat: toopredict hello tipp fizetni útnak.  

## <a name="setup"></a>A beállítás mentése hello az Azure data tudományos környezet speciális elemzésekre
A hello látható [a környezet megtervezése](machine-learning-data-science-plan-your-environment.md) az útmutatóban több beállítások toowork hello NYC Taxi való adatváltások számát az adatkészlethez az Azure-ban van:

* Hello adatokat az Azure-blobokat, majd a modellt az Azure Machine Learning használata
* Hello adatok betöltése az SQL Server-adatbázist, majd a modellt az Azure Machine Learning

Ebben az oktatóanyagban a párhuzamos tömeges importálással hello adatok tooa SQL Server, az adatok feltárása, a szolgáltatás a bemutatjuk mérnöki mintavételi le SQL Server Management Studio használatával, valamint IPython Notebook használatával. [Minta parancsfájlok](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) és [IPython notebookok](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) megosztott a Githubon. Egy minta IPython notebook toowork hello adatokkal az Azure-blobokat is rendelkezésre áll, a hello ugyanazon a helyen.

az Azure Adattudomány környezet tooset:

1. [Tárfiók létrehozása](../storage/common/storage-create-storage-account.md)
2. [Az Azure Machine Learning-munkaterület létrehozása](machine-learning-create-workspace.md)
3. [Rendszerű virtuális gép adatok tudományos](machine-learning-data-science-setup-sql-server-virtual-machine.md), amely biztosítja, hogy egy SQL Server és IPython Notebook kiszolgáló.
   
   > [!NOTE]
   > hello mintaparancsfájlok és IPython notebookok letöltött tooyour Adattudomány virtuális gép lesz hello beállítási folyamata során. Hello VM telepítés utáni parancsfájl befejezése után a virtuális gép dokumentumtár lehet hello minták:  
   > 
   > * Minta parancsfájlok:`C:\Users\<user_name>\Documents\Data Science Scripts`  
   > * A minta IPython notebookok:`C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`  
   >   Ha `<user_name>` a virtuális gép Windows bejelentkezési név. Minta mappák toohello hivatkozik **mintaparancsfájlok** és **minta IPython notebookok**.
   > 
   > 

Hello adatkészlet méretének, adatforrásról és hello Azure célként kiválasztott környezet alapján, ebben a forgatókönyvben hasonlít túl[forgatókönyv \#5: nagy adatkészlet egy helyi fájlok, SQL Server Azure virtuális gép cél](machine-learning-data-science-plan-sample-scenarios.md#largelocaltodb).

## <a name="getdata"></a>Nyilvános forráskódú hello adatok lekérése
tooget hello [NYC Taxi Utazgatással](http://www.andresmh.com/nyctaxitrips/) dataset nyilvános helyéről, segítségével ismertetett hello-metódusok bármelyike [adatok áthelyezése tooand az Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) toocopy hello adatok tooyour új virtuális gép.

toocopy hello adatok AzCopy használatával:

1. Jelentkezzen be tooyour virtuális gép (VM)
2. Hozzon létre egy új könyvtárat a hello VM adatlemez (Megjegyzés: ne használjon hello ideiglenes lemez, amely virtuális Gépet állítsanak adatlemezt hello).
3. Egy parancssori ablakot futtassa a következő Azcopy parancssori, < path_to_data_folder > cseréje a data mappán (2) létrehozott hello:
   
        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S
   
    Hello AzCopy befejezését követően a rendszer összesen 24 zip CSV-fájlok (12-út\_adatok és a 12-út\_jegy ára) hello adatok mappában kell lennie.
4. Bontsa ki a letöltött hello fájlokat. Vegye figyelembe a tömörítetlen hello fájlokat tároló hello mappát. Ez a mappa lesz hivatkozott tooas hello < elérési út\_való\_adatok\_fájlok\>.

## <a name="dbload"></a>A tömeges adatok importálása az SQL Server-adatbázisba
hello betöltés/átvitele a nagy mennyiségű adatok tooan SQL-adatbázis és a lekérdezések teljesítményét növelhető a *particionált táblák és nézetek*. Ebben a szakaszban ismertetett hello utasításokat követi azt [párhuzamos tömeges adatok importálása használatával SQL partíciós táblákba](machine-learning-data-science-parallel-load-sql-partitioned-tables.md) toocreate egy új adatbázist és hello az adatok betöltése párhuzamosan particionált táblába.

1. A virtuális gép tooyour bejelentkezve indítsa el a **SQL Server Management Studio**.
2. Csatlakozás a Windows-hitelesítés használatával.
   
    ![SSMS csatlakozás][12]
3. Ha még nem módosított hello SQL Server hitelesítési mód, és létrehozott egy új SQL-bejelentkezési felhasználójának, nyissa meg a nevű hello parancsfájl **módosítása\_auth.sql** a hello **mintaparancsfájlok** mappát. Hello alapértelmezett felhasználónév és jelszó módosítása Kattintson a **! Végrehajtás** hello eszköztár toorun hello parancsfájlban.
   
    ![Parancsfájlok végrehajtását][13]
4. Győződjön meg arról, és/vagy módosításához hello SQL Server alapértelmezett adatbázishoz és naplófájlokhoz mappák tooensure adatbázisok újonnan létrehozott adatlemezt fogja tárolni. hello SQL Server Virtuálisgép-lemezkép datawarehousing terhelések optimalizált előre konfigurált adatainak és naplókönyvtárainak lemezzel. Ha a virtuális gép nem tartalmazza az adatlemezt, és a hozzáadott új virtuális merevlemezek hello Virtuálisgép-telepítési folyamat során, módosítsa az alábbiak szerint hello alapértelmezett mappák:
   
   * Kattintson a jobb gombbal hello SQL-kiszolgáló neve a bal oldali panelen, és kattintson hello **tulajdonságok**.
     
       ![SQL-kiszolgáló tulajdonságai][14]
   * Válassza ki **adatbázis beállításainak** a hello **oldal kijelölése** toohello bal oldali listában.
   * Ellenőrizze és/vagy módosításához a hello **adatbázis alapértelmezett helyek** toohello **adatlemez** az Ön által választott helyen. Ez az új adatbázisokat tároló Ha hello alapértelmezett helye beállításokkal hozza létre.
     
       ![SQL-adatbázis alapértelmezett értéke][15]  
5. toocreate egy új adatbázist és a fájlcsoport toohold hello particionált táblák, nyissa meg a hello mintaparancsfájl **létrehozása\_db\_default.sql**. hello parancsfájl nevű új adatbázist fog létrehozni **TaxiNYC** és 12 fájlcsoportok hello alapértelmezett adatok helyen. Minden fájlcsoport tárolására egy hónap út\_adatok és út\_díjszabás adatokat. Ha szükséges, módosítsa a hello adatbázis neve. Kattintson a **! Végrehajtás** toorun hello parancsfájl.
6. Ezután hozzon létre két partíció tábla, egy a hello út\_adatokat, majd egy másikat a hello út\_jegy ára. Nyissa meg a hello mintaparancsfájl **létrehozása\_particionált\_table.sql**, amelynél:
   
   * Hozzon létre egy partíciós függvény toosplit hello adatok hónap szerint.
   * Hozzon létre egy partíciós séma toomap minden hónap tooa különböző adatfájlcsoport.
   * Hozzon létre két particionált táblák csatlakoztatott toohello partícióséma: **nyctaxi\_út** hello út tárolására\_adatok és **nyctaxi\_jegy ára** hello út tárolására \_díjszabás adatokat.
     
     Kattintson a **! Végrehajtás** toorun parancsfájl hello és hello particionált táblák létrehozása.
7. A hello **mintaparancsfájlok** mappa, két minta PowerShell-parancsfájlok megadott toodemonstrate párhuzamos tömeges importálások tooSQL Server adattáblák van.
   
   * **BCP\_párhuzamos\_generic.ps1** van egy általános parancsfájl tooparallel tömeges adatok importálása egy táblába. A parancsfájl tooset hello bemeneti és a cél változók módosítása a hello Megjegyzés sorok hello parancsfájlban.
   * **BCP\_párhuzamos\_nyctaxi.ps1** előre konfigurált változata hello általános parancsfájlt, és használt tootooload hello NYC Taxi Utazgatással adatok mindkét táblát.  
8. Kattintson a jobb gombbal hello **bcp\_párhuzamos\_nyctaxi.ps1** parancsfájl nevét, és kattintson **szerkesztése** tooopen azt a PowerShellben. Felülvizsgálati hello előre változóival, és módosítsa a függően tooyour kijelölt adatbázis neve, a bemeneti adatok mappa, a célmappa napló és a elérési utak toohello minta formátumú fájlok **nyctaxi_trip.xml** és **nyctaxi\_ fare.xml** (hello megadott **mintaparancsfájlok** mappa).
   
    ![A tömeges adatok importálása][16]
   
    Hello hitelesítési módot is kiválaszthat, alapértelmezett Windows-hitelesítést. Kattintson a hello eszköztár toorun hello zöld nyílra. hello parancsfájl 24 tömeges importálási műveletek párhuzamos, 12 particionált táblák elindul. Előfordulhat, hogy figyelheti hello adatok importálása folyamatban fenti beállított hello SQL Server alapértelmezett Adatmappa megnyitásával.
9. hello PowerShell parancsfájl jelentések hello kezdési és befejezési időpontja. Az összes tömeges teljes importálja, befejezési időpont hello jelenteni. Hello cél napló mappa tooverify hello tömeges importálása sikerült-e, azaz, hogy ellenőrizze, nincs hiba hello napló célmappában.
10. Az adatbázis készen áll a feltárása, a szolgáltatás tervezés és egyéb műveletek. Mivel hello táblák particionáltak toohello szerint **a felvételi\_dátum és idő** mezőben, a lekérdezések, többek között **a felvételi\_datetime** hello feltételek  **Ha** záradékot használják ki a hello partícióséma előnyeit.
11. A **SQL Server Management Studio**, megismerkedhet a megadott hello mintaparancsfájl **minta\_queries.sql**. hello mintalekérdezések, kiemelési hello bármelyikét sorok lekérdezéséhez, majd kattintson az toorun **! Végrehajtás** hello eszköztáron.
12. hello NYC Taxi Utazgatással adatok két külön táblázatban be van töltve. tooimprove összekapcsolási műveletek, lehetőleg tooindex hello táblákat. mintaparancsfájl hello **létrehozása\_particionált\_index.sql** particionált indexek létrehozása a hello összetett illesztési kulcs **medallion, rejthetők el\_licenc, és a felvételi\_datetime**.

## <a name="dbexplore"></a>Az adatok feltárása és az SQL Server szolgáltatás manipuláció
Ebben a szakaszban végezzük el adatok feltárása és a szolgáltatás generálása hello közvetlenül az SQL-lekérdezések futtatásával **SQL Server Management Studio** korábban létrehozott hello SQL Server-adatbázis használata. Egy minta parancsfájlt nevű **minta\_queries.sql** hello megadott **mintaparancsfájlok** mappát. Hello parancsfájl toochange hello adatbázis neve, módosítása, ha hello alapértelmezettől eltérő: **TaxiNYC**.

Ebben a gyakorlatban a következő történik:

* Csatlakozás túl**SQL Server Management Studio** vagy Windows-hitelesítés használatával, vagy használja az SQL-hitelesítést és hello SQL-bejelentkezési nevet és jelszót.
* Megismerkedhet a különböző idő windows néhány mezőinek disztribúciók adatok.
* Vizsgálja meg az adatok minőségének hello szélességi és hosszúsági mezőket.
* Hello alapján multiclass és bináris besorolási címkék létrehozása **tipp\_összeg**.
* Szolgáltatások létrehozása és számítási/összehasonlítása út távolság.
* Csatlakozás hello két táblák, és bontsa ki a használt toobuild modellek leendő véletlenszerű minta.

Amikor készen áll a tooproceed tooAzure gépi tanulás, akkor előfordulhat, hogy vagy:  

1. Hello végső SQL lekérdezés tooextract és minta hello adatok és a másolás-beillesztés hello lekérdezés mentése közvetlenül egy [és adatokat importálhat] [ import-data] modul az Azure Machine Learning, vagy
2. Hello mintát megmaradnak, és azt tervezi, hogy az adatbázis felépítése modell toouse visszafejtett adatok tábla, és a hello új tábla hello [és adatokat importálhat] [ import-data] az Azure Machine Learning modulban.

Ebben a szakaszban hello utolsó lekérdezési tooextract és minta hello adatok menti azt. második módszer hello mutatják be hello [adatok feltárása és IPython jegyzetfüzet mérnöki szolgáltatás](#ipnb) szakasz.

Sorok és oszlopok a hello hello számának gyors ellenőrzés táblák fel korábban a párhuzamos tömeges importálással segítségével

    -- Report number of rows in table nyctaxi_trip without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('nyctaxi_trip')

    -- Report number of columns in table nyctaxi_trip
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = 'nyctaxi_trip'

#### <a name="exploration-trip-distribution-by-medallion"></a>Feltárása: Út elosztása medallion szerint
Ebben a példában hello medallion (taxi számok) azonosítja a 100-nál több való adatváltások számát egy adott időtartamon belül. hello lekérdezés előnyös hello particionált tábla hozzáférés óta, akkor annak hello partíciós sémája által **a felvételi\_datetime**. Hello teljes adatkészlet lekérdezése is teszi particionált tábla hello használata és/vagy vizsgálat index.

    SELECT medallion, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

#### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Feltárása: Út terjesztési medallion és hack_license
    SELECT medallion, hack_license, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

#### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Adatok vizsgálatának: Helytelen hosszúsági és/vagy szélességi rekordjainak ellenőrzése
Ez a példa pedig megvizsgálja, ha bármelyik hello hosszúsági és/vagy szélességi mezők vagy érvénytelen értéket tartalmaz (radián fok -90 és 90 között kell lennie), vagy (0, 0) koordinátarendszerében.

    SELECT COUNT(*) FROM nyctaxi_trip
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

#### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Feltárása: A vs Formabontó. Nem Formabontó Utazgatással terjesztési
Ez a példa hello száma, amelyek volt Formabontó és időszak (vagy a teljes adatkészletet hello Ha hello teljes évre vonatkozó) egy adott idő alatt nem Formabontó utak megkeresése. Ehhez a terjesztéshez hello bináris címke terjesztési toobe később használatos bináris osztályozási modellezési tükrözi.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM nyctaxi_fare
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

#### <a name="exploration-tip-classrange-distribution"></a>Feltárása: Az osztály vagy tartomány terjesztési tipp
Ebben a példában egy adott idő alatt hello tipp tartományok terjesztése kiszámítja az időszak (vagy a teljes adatkészletet hello Ha hello teljes évre vonatkozó). Ez a hello címke osztályokkal rendelkezik, amelyek később használandó multiclass besorolás modellezési hello terjesztése.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

#### <a name="exploration-compute-and-compare-trip-distance"></a>Feltárása: A számítási, és hasonlítsa össze a út távolság
Ebben a példában a felvételi és Gyűjtőtár hosszúság hello alakítja át, és tooSQL a földrajzi szélesség mutat, hello út távolság SQL geográfiai pontok különbség használatával kiszámítja és visszaadja az összehasonlításhoz hello eredmények véletlenszerű minta. hello példa eredményekre hello toovalid koordinálja, csak a hello minőségi assessment adatlekérdezés kezelt korábban.

    SELECT
    pickup_location=geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326)
    ,dropoff_location=geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326)
    ,trip_distance
    ,computedist=round(geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326).STDistance(geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326))/1000, 2)
    FROM nyctaxi_trip
    tablesample(0.01 percent)
    WHERE CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND   CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

#### <a name="feature-engineering-in-sql-queries"></a>Az SQL-lekérdezések mérnöki szolgáltatás
hello címke generációs és földrajzi átalakítás feltárása lekérdezéseket is használt toogenerate címkék/szolgáltatások része számbavételi hello eltávolításával. A szolgáltatás további mérnöki SQL példák hello szerepelnek [adatok feltárása és IPython jegyzetfüzet mérnöki szolgáltatás](#ipnb) szakasz. Hatékonyabb toorun hello szolgáltatás generációs lekérdezések teljes adatkészlet hello vagy egy nagy részét, közvetlenül a hello SQL Server adatbázis-példány az SQL-lekérdezések használatával is. hello lekérdezéseket is végrehajtható a **SQL Server Management Studio**, IPython Notebook vagy semmilyen eszköz/fejlesztőkörnyezet amely elérheti hello adatbázis helyileg vagy távolról.

#### <a name="preparing-data-for-model-building"></a>Adatok előkészítése az modell létrehozásának
hello következő lekérdezés illesztések hello **nyctaxi\_út** és **nyctaxi\_jegy ára** táblák, állít elő, a bináris besorolási címkék **Formabontó**, egy több osztály besorolási címke **tipp\_osztály**, és kinyeri 1 % véletlenszerűen hello teljes illesztett adatkészletből. Ez a lekérdezés másolja, majd közvetlenül az hello beillesztett [Azure Machine Learning Studio](https://studio.azureml.net) [és adatokat importálhat] [ import-data] közvetlen adatfeldolgozást hello SQL Server adatbázis-modul az Azure-példányt. hello lekérdezés nem tartalmazza a rekordok helytelen (0, 0) koordinátarendszerében.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,     f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_trip t, nyctaxi_fare f
    TABLESAMPLE (1 percent)
    WHERE t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'


## <a name="ipnb"></a>Az adatok feltárása, a szolgáltatás fejlesztés IPython jegyzetfüzet
Ebben a szakaszban végezzük el a Python és az SQL lekérdezések írásában, korábban létrehozott hello SQL Server-adatbázis használata a szolgáltatás generálása és az adatok feltárása. Egy minta IPython notebook nevű **machine-Learning-data-science-process-sql-story.ipynb** hello megadott **minta IPython notebookok** mappa. A notebook is rendelkezésre áll, a [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks).

big Data típusú adatok használata ajánlott az feladatütemezési hello hello következő:

* Olvassa el egy a memóriában levő keretbe hello adatok mintát.
* Hajtsa végre az egyes képi megjelenítéseket, és segítségével explorations hello mintaadatokat.
* Összehasonlítást az adatminta szolgáltatás mérnöki hello segítségével a kísérletet.
* A nagyobb az adatok feltárása adatkezelési és a szolgáltatás mérnöki csapathoz, használjon Python tooissue SQL lekérdezések közvetlenül hello SQL Server-adatbázis hello Azure virtuális Gépen.
* Döntse el, az Azure Machine Learning modell létrehozásának hello minta mérete toouse.

Ha készen áll a Machine Learning tooproceed tooAzure, vagy lehet:  

1. Hello végső SQL lekérdezés tooextract és minta hello adatok és a másolás-beillesztés hello lekérdezés mentése közvetlenül egy [és adatokat importálhat] [ import-data] az Azure Machine Learning modulban. Ez a módszer mutatják be hello [épület modellek az Azure Machine Learning](#mlmodel) szakasz.    
2. Hello mintát és azt tervezi, hogy az új adatbázistábla létrehozása modell toouse visszafejtett adatokat, majd használja hello új tábla hello [és adatokat importálhat] [ import-data] modul.

hello az alábbiakban néhány adatok feltárása, adatábrázolási és példák mérnöki szolgáltatás. További példákért lásd hello minta SQL IPython jegyzetfüzetet hello **minta IPython notebookok** mappa.

#### <a name="initialize-database-credentials"></a>Adatbázis-hitelesítő adatok inicializálása
Az adatbázis-kapcsolati beállításokat a következő változók hello inicializálása:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database server>

#### <a name="create-database-connection"></a>Adatbázis-kapcsolat létrehozása
    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

#### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Sorok és oszlopok a tábla nyctaxi_trip jelentés száma
    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('nyctaxi_trip')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('nyctaxi_trip')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

* A sorok teljes számát = 173179759  
* Az oszlopok teljes száma = 14

#### <a name="read-in-a-small-data-sample-from-hello-sql-server-database"></a>Olvassa el a hello SQL Server-adatbázis egy kisméretű mintát
    t0 = time.time()

    query = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (0.05 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time tooread hello sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Idő tooread hello mintatáblázat: 6.492000 másodperc  
A beolvasott sorok és oszlopok száma = (84952, 21)

#### <a name="descriptive-statistics"></a>Leíró statisztika
Most állnak készen tooexplore mintát hello adatok. Először a hello leíró statisztika megnézi **út\_távolság** (vagy bármely más) mezőből:

    df1['trip_distance'].describe()

#### <a name="visualization-box-plot-example"></a>A képi megjelenítés: Rajzot – példa
Úgy tekintünk hello Dobozdiagram hello út távolság toovisualize hello quantiles a Tovább gombra

    df1.boxplot(column='trip_distance',return_type='dict')

![Tőzsdei #1][1]

#### <a name="visualization-distribution-plot-example"></a>A képi megjelenítés: Terjesztési rajzot – példa
    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Tőzsdei #2][2]

#### <a name="visualization-bar-and-line-plots"></a>A képi megjelenítés: Sáv és sor előkészítésére
Ebben a példában a Microsoft hello út távolság az öt bins bin és hello dobozolás eredmények megjelenítése.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

A Microsoft hello fent egy sávon bin terjesztési tőzsdei vagy sor a következő ábra

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![#3 ábrázolása][3]

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![#4 ábrázolása][4]

#### <a name="visualization-scatterplot-example"></a>A képi megjelenítés: Scatterplot – példa
Pontdiagram rajzot közötti megmutatjuk **út\_idő\_a\_másodperc** és **út\_távolság** toosee bármely korrelációs esetén

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![#6 ábrázolása][6]

Hasonlóképpen azt hello közötti kapcsolat ellenőrzése **arány\_kód** és **út\_távolság**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![#8 ábrázolása][8]

### <a name="sub-sampling-hello-data-in-sql"></a>Alárendelt mintavételi hello SQL-adatok
Adatok létrehozása modell előkészítésekor [Azure Machine Learning Studio](https://studio.azureml.net), vagy dönthet a hello **SQL lekérdezés toouse közvetlenül az adatok importálása modul hello** vagy hello fejthetők vissza és mintát megőrzése új táblát, amely hello használhat adatok [és adatokat importálhat] [ import-data] modul egy egyszerű **kiválasztása * FROM < a\_új\_tábla\_neve >**.

Ebben a szakaszban létrehozunk egy új tábla toohold hello mintát, és fejthetők vissza az adatokat. Például egy közvetlen SQL-lekérdezés a modell létrehozásának megadott hello [adatok feltárása és az SQL Server mérnöki szolgáltatás](#dbexplore) szakasz.

#### <a name="create-a-sample-table-and-populate-with-1-of-hello-joined-tables-drop-table-first-if-it-exists"></a>Hozzon létre egy minta-tábla és a feltöltési hello csatlakoztatott táblák 1 %. Eldobni a tábla első, ha van ilyen.
Ez a szakasz azt csatlakozás hello táblák **nyctaxi\_út** és **nyctaxi\_jegy ára**, bontsa ki a 1 % véletlenszerűen és egy új tábla nevében mintát hello adatok megőrzése  **nyctaxi\_egy\_százalék**:

    cursor = conn.cursor()

    drop_table_if_exists = '''
        IF OBJECT_ID('nyctaxi_one_percent', 'U') IS NOT NULL DROP TABLE nyctaxi_one_percent
    '''

    nyctaxi_one_percent_insert = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount
        INTO nyctaxi_one_percent
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (1 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
        AND   pickup_longitude <> '0' AND dropoff_longitude <> '0'
    '''

    cursor.execute(drop_table_if_exists)
    cursor.execute(nyctaxi_one_percent_insert)
    cursor.commit()

### <a name="data-exploration-using-sql-queries-in-ipython-notebook"></a>Az SQL-lekérdezések IPython Notebook használatával az adatok feltárása
Ebben a részben azt megismerkedhet adatok azokat a terjesztéseket használatával hello mintát 1 % adat, amely a fenti létrehozott hello új tábla megőrződjenek. Vegye figyelembe, hogy hasonló explorations használatával végezheti el hello eredeti táblák, külön kérésre mintázatot használ **TABLESAMPLE** toolimit hello feltárása minta vagy hello korlátozásával eredmények hello időszakának megadott tooa **felvétel \_datetime** particionálja a hello [adatok feltárása és az SQL Server mérnöki szolgáltatás](#dbexplore) szakasz.

#### <a name="exploration-daily-distribution-of-trips"></a>Feltárása: Való adatváltások számát napi eloszlásáról
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Feltárása: Utazás per medallion terjesztési
    query = '''
        SELECT medallion,count(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

### <a name="feature-generation-using-sql-queries-in-ipython-notebook"></a>Az SQL-lekérdezések használatával IPython jegyzetfüzet szolgáltatás létrehozása
Ez a szakasz az új címkék fog létrehozni, és közvetlenül az SQL-lekérdezések használatával funkciókat, hello 1 % mintatáblázat működő létrehozott hello előző szakaszban.

#### <a name="label-generation-generate-class-labels"></a>Címke generációs: Osztály címkék létrehozása
A következő példa hello létre azt a modellezési címkék toouse két csoportja:

1. Bináris osztály címkék **Formabontó** (ha tipp kapnak becslése)
2. Multiclass címkék **tipp\_osztály** (előrejelzésére hello tipp bin vagy tartomány)
   
        nyctaxi_one_percent_add_col = '''
            ALTER TABLE nyctaxi_one_percent ADD tipped bit, tip_class int
        '''
   
        cursor.execute(nyctaxi_one_percent_add_col)
        cursor.commit()
   
        nyctaxi_one_percent_update_col = '''
            UPDATE nyctaxi_one_percent
            SET
               tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
               tip_class = CASE WHEN (tip_amount = 0) THEN 0
                                WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                                WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                                WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                                ELSE 4
                            END
        '''
   
        cursor.execute(nyctaxi_one_percent_update_col)
        cursor.commit()

#### <a name="feature-engineering-count-features-for-categorical-columns"></a>A szolgáltatás műszaki osztály: Száma szolgáltatások Kategorikus oszlopokhoz
Ebben a példában egy numerikus mezőbe kategorikus mező átalakítja azáltal, hogy minden kategória hello hello adatok az előfordulások száma.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD cmt_count int, vts_count int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B AS
        (
            SELECT medallion, hack_license,
                SUM(CASE WHEN vendor_id = 'cmt' THEN 1 ELSE 0 END) AS cmt_count,
                SUM(CASE WHEN vendor_id = 'vts' THEN 1 ELSE 0 END) AS vts_count
            FROM nyctaxi_one_percent
            GROUP BY medallion, hack_license
        )

        UPDATE nyctaxi_one_percent
        SET nyctaxi_one_percent.cmt_count = B.cmt_count,
            nyctaxi_one_percent.vts_count = B.vts_count
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion AND A.hack_license = B.hack_license
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-bin-features-for-numerical-columns"></a>A szolgáltatás műszaki osztály: Bin szolgáltatások numerikus oszlopokhoz
Ebben a példában egy folyamatos számmező átalakítja az előre definiált kategória tartományokat, azaz átalakító számmező kategorikus mező be azokat.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD trip_time_bin int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B(medallion,hack_license,pickup_datetime,trip_time_in_secs, BinNumber ) AS
        (
            SELECT medallion,hack_license,pickup_datetime,trip_time_in_secs,
            NTILE(5) OVER (ORDER BY trip_time_in_secs) AS BinNumber from nyctaxi_one_percent
        )

        UPDATE nyctaxi_one_percent
        SET trip_time_bin = B.BinNumber
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion
        AND A.hack_license = B.hack_license
        AND A.pickup_datetime = B.pickup_datetime
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-extract-location-features-from-decimal-latitudelongitude"></a>A szolgáltatás műszaki osztály: Hely szolgáltatások kinyerése decimális szélesség/hosszúság
Ebben a példában megszakad a szélességi és/vagy hosszúsági mező decimális ábrázolása hello több régióban mezőibe eltérő a granularitási, többek között ország város, város, letiltása, stb. Vegye figyelembe, hogy hello új földrajzi-mezők nem leképezett tooactual helyét. Leképezési geocode helyeken további információkért lásd: [Bing Maps REST szolgáltatások](https://msdn.microsoft.com/library/ff701710.aspx).

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent
        ADD l1 varchar(6), l2 varchar(3), l3 varchar(3), l4 varchar(3),
            l5 varchar(3), l6 varchar(3), l7 varchar(3)
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        UPDATE nyctaxi_one_percent
        SET l1=round(pickup_longitude,0)
            , l2 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 1 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),1,1) ELSE '0' END     
            , l3 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 2 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),2,1) ELSE '0' END     
            , l4 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 3 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),3,1) ELSE '0' END     
            , l5 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 4 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),4,1) ELSE '0' END     
            , l6 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 5 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),5,1) ELSE '0' END     
            , l7 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 6 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),6,1) ELSE '0' END
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="verify-hello-final-form-of-hello-featurized-table"></a>Hello végső űrlap hello featurized tábla ellenőrzése
    query = '''SELECT TOP 100 * FROM nyctaxi_one_percent'''
    pd.read_sql(query,conn)

Dolgozunk most már készen áll a tooproceed toomodel épület és a modell telepítési [Azure Machine Learning](https://studio.azureml.net). hello adatok készen áll a hello előrejelzés probléma azonosított korábbi, nevezetesen:

1. Bináris osztályozás: toopredict tipp egy út kifizetett-e.
2. Multiclass besorolási: toopredict hello tartomány tipp fizetős, toohello korábban definiált osztályok szerint.
3. Regressziós feladat: toopredict hello tipp fizetni útnak.  

## <a name="mlmodel"></a>Az Azure Machine Learning modellek létrehozása
toobegin hello modellezési a gyakorlatban tooyour Azure Machine Learning-munkaterület bejelentkezés. Ha még nem hozott létre machine learning-munkaterület, lásd: [hozzon létre egy Azure Machine Learning munkaterülettel](machine-learning-create-workspace.md).

1. az Azure Machine Learning lépései tooget lásd: [Mi az Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)
2. Jelentkezzen be túl[Azure Machine Learning Studio](https://studio.azureml.net).
3. hello Studio kezdőlap számos olyan információt, videók, oktatóanyagok, hivatkozások toohello modulok hivatkozás és egyéb erőforrásokat biztosít. Fore Azure Machine Learning kapcsolatos további információkért tekintse meg a hello [Azure Machine Learning dokumentációs központban](https://azure.microsoft.com/documentation/services/machine-learning/).

Egy tipikus tanítási kísérletet hello következő tevődik össze:

1. Hozzon létre egy **+ új** kipróbálásához.
2. Hello adatok tooAzure Machine Learning beolvasása.
3. Előre feldolgozzák, átalakítás és kezelhetők hello adatok, igény szerint.
4. Szolgáltatások létrehozása, igény szerint.
5. Hello adatok felosztása adatkészletek képzési/érvényesítési/tesztelése (külön adatkészleteket, vagy az egyes).
6. Válasszon egy vagy több számítógépet tanulási algoritmus attól függően, hogy a probléma toosolve tanulási hello. Például bináris osztályozási multiclass besorolás, regressziós.
7. Hello képzési adatkészlet egy vagy több modell betanításához.
8. Pontszám hello érvényesítési dataset hello betanított modellek segítségével.
9. Hello modellek toocompute hello vonatkozó metrikáinak probléma tanulási hello kiértékeléséhez.
10. Konfigurálva finomhangolhatják a hello modellek és select hello legjobb modell toodeploy.

Ebben a gyakorlatban azt már megismerkedett és fejthetők vissza az SQL Server hello adatokat, és úgy döntött, a hello minta mérete tooingest az Azure Machine Learning. egy vagy több hello előrejelzési modellek toobuild döntöttünk:

1. Hello adatok tooAzure Machine Learning beolvasása hello segítségével [és adatokat importálhat] [ import-data] modul hello elérhető **bemeneti és kimeneti** szakasz. További információkért lásd: hello [és adatokat importálhat] [ import-data] modul referencialapja.
   
    ![Az Azure Machine Learning-adatok beolvasása][17]
2. Válassza ki **Azure SQL Database** , hello **adatforrás** a hello **tulajdonságok** panel.
3. Adja meg a hello adatbázis DNS-név hello **adatbázis-kiszolgáló neve** mező. Formátum:`tcp:<your_virtual_machine_DNS_name>,1433`
4. Adja meg a hello **adatbázisnév** hello megfelelő mezőben.
5. Adja meg a hello **SQL felhasználónév** hello a ** felhasználói aqccount kiszolgálónév és a jelszó hello hello **kiszolgáló felhasználói fiók jelszavát**.
6. Ellenőrizze **fogadja el a kiszolgálói tanúsítvány** lehetőséget.
7. A hello **adatbázis-lekérdezés** szerkesztése területre, és beillesztheti hello szükséges adatbázis-(beleértve a számított mezőket például hello címkék) mezőt, és le kivonatok minták hello adatok szükséges toohello mintaméret hello lekérdezés.

Példa egy bináris osztályozási kísérletet, közvetlenül a hello SQL Server-adatbázis adatainak olvasása: hello az alábbi ábra a. Hasonló kísérletek multiclass besorolás és a visszavonási lehet létrehozni.

![Az Azure Machine Learning vonat][10]

> [!IMPORTANT]
> Az adatok kinyerése modellezési, és a korábbi szakaszokban bemutatott lekérdezés példák mintavételi hello **hello három modellezési gyakorlatok az összes felirathoz szerepelnek hello lekérdezés**. Az egyes gyakorlatok modellezési hello (kötelező) fontos lépés túl van**kizárása** hello szükségtelen címkéit hello más két problémákat, és minden egyéb **kiszivárgásának cél**. A például bináris osztályozási használjon hello címke **Formabontó** , és amelyeket hello mezők **tipp\_osztály**, **tipp\_összeg**, és **teljes\_összeg**. Ez utóbbi hello cél kiszivárgásának, mivel azok utalnak hello tipp fizetett.
> 
> felesleges oszlopok tooexclude és/vagy a cél kiszivárgásának, használhatja a hello [Select Columns in Dataset] [ select-columns] modul vagy hello [szerkesztése metaadatok] [ edit-metadata]. További információkért lásd: [Select Columns in Dataset] [ select-columns] és [szerkesztése metaadatok] [ edit-metadata] lapok hivatkoznak.
> 
> 

## <a name="mldeploy"></a>Az Azure Machine Learning modellek telepítéséről
Amikor készen áll a modell, könnyedén telepítheti azt közvetlenül a hello kísérlet webszolgáltatásként. További információ az Azure Machine Learning webszolgáltatások üzembe helyezése: [központi telepítése az Azure Machine Learning webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md).

egy új webszolgáltatás-bővítmény toodeploy, kell:

1. A pontozási kísérlet létrehozása.
2. Hello webes szolgáltatás üzembe helyezése.

a pontozási kísérletezhet a toocreate egy **befejezett** betanítása kísérletet, kattintson **létrehozása pontozási KÍSÉRLETEZHET** hello alacsonyabb művelet sávon.

![Pontozás az Azure][18]

Az Azure Machine Learning toocreate egy pontozási kísérletet hello tanítási kísérletet hello összetevői alapján kísérli meg. Különösen a következőket hajtja végre:

1. Hello betanított modell mentéséhez, és távolítsa el a hello modell képzési modulok.
2. Azonosítsa a logikai **bemeneti port** toorepresent hello várt bemeneti adatok séma.
3. Azonosítsa a logikai **kimeneti port** toorepresent hello várt webes szolgáltatás kimeneti sémával.

Amikor hello pontozási kísérlet jön létre, ellenőrzésre, majd szükség szerint állítsa be. Egy tipikus módosítás tooreplace hello bemeneti adatkészlet és/vagy entitáselem, amely kizárja a címke mezők esetén, mert ezek nem lesz elérhető hello szolgáltatás meghívásakor. Akkor is hello célszerű tooreduce hello méretet adjon meg adatkészlet és/vagy lekérdezési tooa néhány rekordjának, éppen elegendő tooindicate hello bemeneti sémát. A hello kimeneti portra, azt közös tooexclude összes beviteli mezőt, és csak terjed hello **pontozott címkék** és **program pontozza a mennyiségeket valószínűség** hello hello használja a [Select Columns in Dataset ] [ select-columns] modul.

Kísérlet pontozási minta hello az alábbi ábra van. Ha készen áll a toodeploy, kattintson a hello **WEBSZOLGÁLTATÁS közzététele** hello alacsonyabb művelet található gombra.

![Az Azure gépi tanulás közzététele][11]

toorecap, ez a forgatókönyv az oktatóanyagban hozott létre az Azure data tudományos környezethez, nagy nyilvános adatbázisból származó adatok megszerzése toomodel képzési és az Azure Machine Learning webszolgáltatás üzembe helyezése összes hello módon együttműködik.

### <a name="license-information"></a>Licencinformációk
Ez a minta forgatókönyv és a mellékelt parancsfájlok és IPython notebook(s) által megosztott Microsoft hello MIT licence. Ellenőrizze hello LICENSE.txt fájl hello Directory hello mintakód a Githubon további részleteket.

### <a name="references"></a>Referencia
• [Andrés Monroy NYC Taxi utak letöltési oldala](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing NYC taxiköltség út adatok Chris Whong által](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC Taxi és Limousine Bizottság kutatási és statisztika](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)

[1]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sql-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sql-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sql-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sql-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sql-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sql-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sql-walkthrough/amlscoring.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
