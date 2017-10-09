---
title: "a Spark on Azure HDInsight használatának Adattudomány aaaOverview |} Microsoft Docs"
description: "hello Spark MLlib eszközkészlet jelentős gépi tanulás modellezési képességekkel elosztott toohello HDInsight környezet elérhetővé teszi."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: a4e1de99-a554-4240-9647-2c6d669593c8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2017
ms.author: deguhath;bradsev;gokuma
ms.openlocfilehash: 515705684a46917c2741bf063d439b1cda016abb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-data-science-using-spark-on-azure-hdinsight"></a>Adattudomány Spark on Azure HDInsight használatának áttekintése
[!INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Ennek a programcsomagnak a témakörök bemutatja, hogyan toouse HDInsight Spark toocomplete közös adattudomány feladatokat, például az adatfeldolgozást, a szolgáltatás mérnöki csapathoz, a modellezési és a modell kiértékelése. hello használt adata hello 2013 NYC taxi út és a jegy ára dataset mintáját. beépített hello modellek logisztikai és lineáris regressziós, véletlenszerű erdők és átmenetes súlyozott fák tartalmazza. hello témakörökben is láthatja hogyan toostore ezek a modellek az Azure blob storage (WASB), és hogyan tooscore és értékelje a prediktív teljesítményét. Összetettebb témákra fedik le, hogyan lehet a modellek betanítása a kereszt-ellenőrzési és a hyper-paraméter abszolút használatával. Jelen összefoglaló téma is hivatkozik hello témakörök ismertetik, hogyan mentése tooset hello igénylő toocomplete hello lépéseit hello forgatókönyvek a megadott Spark-fürt. 

## <a name="spark-and-mllib"></a>Spark és MLlib
[Spark](http://spark.apache.org/) egy nyílt forráskódú párhuzamos feldolgozást végző keretrendszer, amely támogatja a memórián belüli feldolgozását végzi big data elemző alkalmazások tooboost hello teljesítményét. hello Spark program sebességét, a könnyű, valamint a kifinomult analytics lett tervezve. A Spark memóriában elosztott tárolt számítási képességei jól funkcionálnak a hello szerepel a machine learning és a graph számítások iteratív algoritmusaival. [MLlib](http://spark.apache.org/mllib/) van a Spark méretezhető machine learning könyvtárban, amely csökkenti a hello algoritmikus modellezési képességekkel toothis elosztott környezetekben. 

## <a name="hdinsight-spark"></a>A HDInsight Spark
[HDInsight Spark](../hdinsight/hdinsight-apache-spark-overview.md) hello Azure üzemelteti a nyílt forráskódú Spark kínál. Is támogatja a **Jupyter PySpark notebookok** a hello Spark-fürt futtatható Spark SQL interaktív lekérdezések átalakítása, szűrési és megjeleníteni az Azure BLOB (WASB) tárolt adatokat. PySpark hello Python API-t a Spark. hello Spark-fürtjei telepítve hello kódrészletek, amelyek hello megoldást nyújtanak, és megfelelő hello tevékenységtérkép itt futtatása a Jupyter notebookok toovisualize hello adatok megjelenítése. hello modellezési lépések a következő témakörökben talál, amely bemutatja, hogyan tootrain, értékelje ki, mentse, és felhasználhatják modell különböző típusú kódot tartalmaznak. 

## <a name="setup-spark-clusters-and-jupyter-notebooks"></a>A telepítő: A Spark-fürtök és a Jupyter notebookok
Beállítási lépéseket és kód okat ebben a forgatókönyvben egy HDInsight Spark 1.6 használatával. De Jupyter notebookok HDInsight Spark 1.6-os és a Spark 2.0 fürtök rendelkeznek. Hello jegyzetfüzet és -hivatkozások toothem leírása szerepelnek hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) az azokat tartalmazó hello GitHub-tárházban. Ezenkívül hello kód itt kapcsolódó hello jegyzetfüzetekben általános és a Spark-fürt működnek. Ha nem használja a HDInsight Spark, hello fürttelepítés, és lehet, hogy a felügyeleti lépések némileg eltér az itt látható. Kényelmi célokat szolgál az alábbiakban hello hivatkozások toohello Jupyter notebookok Spark 1.6 (toobe futtatása a Jupyter Notebook server hello hello pySpark kernel) és a Spark 2.0 (toobe hello pySpark3 kernel a Jupyter Notebook server hello futtatni):

### <a name="spark-16-notebooks"></a>Spark 1.6-os notebookok
Ezek notebookok futtatása a Jupyter notebook kiszolgáló hello pySpark kernel toobe.

- [pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb): bemutatja, hogy miként tooperform adatok feltárása, modellezéséhez, és számos különböző algoritmusok pontozási.
- [pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): #1 jegyzetfüzet és modell fejlesztési hyperparameter hangolása és kereszt-ellenőrzési témaköröket tartalmazza.
- [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb): bemutatja, hogyan toooperationalize egy mentett modell Python használata a HDInsight-fürtök.

### <a name="spark-20-notebooks"></a>Spark 2.0 notebookok
Ezek notebookok futtatása a Jupyter notebook kiszolgáló hello pySpark3 kernel toobe.

- [Spark2.0-pySpark3-Machine-Learning-Data-Science-Spark-Advanced-Data-exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Ez a fájl bemutatja, hogyan tooperform adatok feltárása, modellezés és Spark 2.0 pontozási fürtök hello NYC Taxi út használata és jegy ára-adatkészlet leírt [Itt](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data). A notebook gyorsan felfedezése Spark 2.0 adtunk hello kódot az jó kiindulási pont lehet. További részletes notebook hello NYC Taxi adatok elemzi, lásd: hello következő notebook ezen a listán. Tekintse meg a lista a következő hello megjegyzéseket, ezek notebookok összehasonlító. 
- [Spark2.0-pySpark3_NYC_Taxi_Tip_Regression.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_NYC_Taxi_Tip_Regression.ipynb): Ez a fájl bemutatja, hogyan tooperform adatok wrangling (Spark SQL és dataframe műveletek), feltárása, modellezéséhez és pontozási használatával hello NYC Taxi út és a jegy ára adatkészlet leírt [ Itt](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).
- [Spark2.0-pySpark3_Airline_Departure_Delay_Classification.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_Airline_Departure_Delay_Classification.ipynb): Ez a fájl bemutatja, hogyan tooperform adatok wrangling (Spark SQL és dataframe műveletek), feltárása, modellezéséhez és pontozási használatával hello jól ismert légitársaság időben indító a DataSet 2012 és a 2011. Azt integrálva hello légitársaság dataset hello repülőtéri időjárási adatokat (pl. szélsebesség, hőmérséklet, magasság stb.) a korábbi toomodeling, időjárási felhasználásokhoz hello modellt tartalmazhat.

<!-- -->

> [!NOTE]
> hello légitársaság dataset hozzá lett adva a Spark 2.0 toohello notebookok toobetter hello besorolás algoritmusok használatát mutatja be. Tekintse meg a következő hivatkozások légitársaság időben indító dataset és időjárási dataset információt hello:

>- Légitársaság időben indító adatok: [http://www.transtats.bts.gov/ONTIME/](http://www.transtats.bts.gov/ONTIME/)

>- Repülőtéri időjárási adatok: [https://www.ncdc.noaa.gov/](https://www.ncdc.noaa.gov/) 
> 
> 

<!-- -->

<!-- -->

> [!NOTE]
hello NYC taxi hello Spark 2.0 jegyzetfüzetek és légitársaság repülési késleltetés-adatkészleteket is igénybe vehet, 10 perc vagy több toorun (attól függően, hogy a HDI-fürtnek hello mérete). hello első jegyzetfüzetet hello lista fölött látható hello adatfeltárás sok aspektusait, a képi megjelenítés és ML modell, amely kevesebb idő toorun le mintát NYC adatkészlet, mely hello a taxi és jegy ára fájlok voltak előre illesztett jegyzetfüzet képzési: [ Spark2.0-pySpark3-Machine-Learning-Data-Science-Spark-Advanced-Data-exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb) jegyzetfüzet tart egy sokkal rövidebb idő toofinish (2-3 perc), és előfordulhat, hogy lehet Jó kiindulópont gyorsan felfedezése hello kódot kell a megadott Spark 2.0. 

<!-- -->

A Spark 2.0 modell és a model felhasználás pontozó hello operationalization útmutatóért lásd: hello [Spark 1.6-os dokumentum a felhasználás](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) egy példát, amely hello lépéseit tartalmazza. toouse hello Python kódját fájlt a Spark 2.0, cserélje le [ezt a fájlt](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py).

### <a name="prerequisites"></a>Előfeltételek
a következő eljárások hello kapcsolódó tooSpark 1.6-os. Hello Spark 2.0-s verziójához hello notebookok használata leírt, és a kapcsolódó toopreviously. 

1. rendelkeznie kell Azure-előfizetéssel. Ha még nem rendelkezik egy, lásd: [beolvasása az Azure ingyenes próbaverzió](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

2 van szüksége a Spark 1.6-os-fürt toocomplete Ez a forgatókönyv. toocreate, lásd: hello utasításokat [első lépések: Apache Spark on Azure HDInsight létrehozása](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). hello fürt típusa és verzió van megadva a hello **fürt típusának kiválasztása** menü. 

![Fürt konfigurálása](./media/machine-learning-data-science-spark-overview/spark-cluster-on-portal.png)

<!-- -->

> [!NOTE]
> Ez a témakör bemutatja, hogyan toouse Python helyett Scala toocomplete feladatokat az adatok végpontok közötti tudományos folyamatai, lásd: hello [Azure Spark Scala használatával Adattudomány](machine-learning-data-science-process-scala-walkthrough.md).
> 
> 

<!-- -->

> [!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]
> 
> 

## <a name="hello-nyc-2013-taxi-data"></a>hello NYC 2013 Taxi adatok
hello NYC Taxi út adatok körülbelül 20 GB tömörített vesszővel tagolt (CSV) fájl (tömörítetlen ~ 48 GB), amely több mint 173 millió egyedi utak és hello turistajegyek esetében minden út kifizette. Minden út rekord tartalmazza, hello felvételével kapcsolatban és Gyűjtőtár hely és idő, anonimizált rejthetők el (illesztőprogram) licenc száma, és medallion (taxi tartozó egyedi azonosító) számát. hello adatok hello évben 2013 hozzá van rendelve minden való adatváltások számát, és megtalálható a következő két adatkészletet havonta hello:

1. hello "trip_data" CSV-fájlok tartalmazza út, például az utasok száma, onnan folytathatja az adatgyűjtést, és dropoff mutat, út időtartamát és út hossza. Íme néhány példa rekordok:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. hello "trip_fare" CSV-fájlok hello jegy ára minden út, például a fizetési módot, jegy ára összeg, emelt díjas és, tippeket és autópályadíjakra, és kifizetett hello teljes összeg kifizette részleteit tartalmazza. Íme néhány példa rekordok:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Ezeket a fájlokat és illesztett hello út 0,1 % mintát átirányította\_adatok és út\_csak egyet fájlok díjszabás be egy egyetlen dataset toouse hello bemeneti adatkészletet a forgatókönyv szerint. hello egyedi kulcs toojoin út\_adatok és út\_jegy ára áll hello mezők: medallion, rejthetők el\_engedély és a felvételi\_datetime. Hello adatkészlet egyes rekord tartalmazza a következő attribútumok egy NYC Taxi út képviselő hello:

| Mező | Rövid leírás |
| --- | --- |
| medallion |Anonimizált taxi medallion (egyedi taxi azonosító) |
| hack_license |Anonimizált Hackney kocsivissza licenc száma |
| vendor_id |Taxi szállító azonosítója |
| rate_code |Jegy ára taxi gyakorisága a következőt: |
| store_and_fwd_flag |Tárolási és előre jelző |
| pickup_datetime |Dátum és idő átvételéhez |
| dropoff_datetime |Dropoff dátum és idő |
| pickup_hour |Óra átvételéhez |
| pickup_week |Hello év hete átvételéhez |
| milyen napra esik |Milyen napra esik (tartomány: 1-7) |
| passenger_count |Egy taxi út az utasok száma |
| trip_time_in_secs |Visszatérési ideje másodpercben |
| trip_distance |Miles út távolság |
| pickup_longitude |A földrajzi hosszúság értéke átvételéhez |
| pickup_latitude |A földrajzi hosszúság átvételéhez |
| dropoff_longitude |Dropoff hosszúság |
| dropoff_latitude |Dropoff szélesség |
| direct_distance |Közvetlen kivételezési közötti távolság fel és dropoff helye |
| payment_type |A fizetési mód (cas, hitelkártya stb.) |
| fare_amount |A jegy ára összeg |
| Emelt díjas |Emelt díjas |
| mta_tax |MTA adó |
| tip_amount |Tipp összeg |
| tolls_amount |Autópályadíjak összeg |
| total_amount |Teljes összeg |
| Formabontó |Formabontó (0 vagy 1. nem vagy Igen) |
| tip_class |Tipp osztály (0: 0, 1: $0-5, 2: $6 – 10., 3: $11-20, 4: > $20) |

## <a name="execute-code-from-a-jupyter-notebook-on-hello-spark-cluster"></a>A Spark-fürt hello Jupyter notebook hajt végre kódot
Az Azure-portálon hello Jupyter Notebook hello indíthatja el. Keresse meg a Spark-fürt az irányítópulton, és kattintson rá a tooenter felügyelet lapon a fürt számára. tooopen hello notebook társított hello Spark-fürt, kattintson a **fürt irányítópultok** -> **Jupyter Notebook** .

![Fürt irányítópultok](./media/machine-learning-data-science-spark-overview/spark-jupyter-on-portal.png)

Tallózással is kikeresheti is túl***https://CLUSTERNAME.azurehdinsight.net/jupyter*** tooaccess hello Jupyter notebookok. Az URL-cím része hello CLUSTERNAME cserélje le a saját fürt hello neve. A rendszergazdai fiók tooaccess hello notebookok hello jelszó szükséges.

![Keresse meg a Jupyter notebookok](./media/machine-learning-data-science-spark-overview/spark-jupyter-notebook.png)

Válassza ki a PySpark toosee néhány példa a PySpark API.hello notebookok hello mintakódok Ez a témakör a Spark Suite tartalmazó érhetők el hello használó előcsomagolt notebookok tartalmazó könyvtár [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark)

Feltöltheti közvetlenül a hello notebookok [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark) toohello Jupyter notebook kiszolgáló a Spark-fürtön. A Jupyter hello kezdőlapján, kattintson a hello **feltöltése** jobb része az üdvözlő képernyőt hello gombjára. A Fájlkezelőben nyílik meg. Itt hello jegyzetfüzet és kattintson a hello GitHub (nyers tartalom) URL-cím illeszthető **nyitott**. 

A Jupyter fájl nevére, lásd: hello fájlnév egy **feltöltése** újra gombra. Ide **feltöltése** gombra. Most már importált hello notebookot. Ismételje meg ezeket a lépéseket tooupload hello a Ez a forgatókönyv más notebookok.

> [!TIP]
> Kattintson a jobb egérgombbal hello hivatkozásokat a böngésző, és válassza ki a **hivatkozás másolása** tooget hello github nyers tartalom URL-CÍMÉT. Az URL-cím bemásolhatja hello Jupyter feltöltése explorer párbeszédpanel jelenik meg.
> 
> 

Most már a következőket teheti:

* Lásd: hello kód hello notebook kattintva.
* Minden cella végrehajtása billentyűkombináció lenyomásával **a SHIFT + ENTER**.
* Hello teljes jegyzetfüzet kattintva elindítja **cella** -> **futtatása**.
* A lekérdezések hello automatikus képi megjelenítés használata.

> [!TIP]
> hello PySpark kernel automatikusan visualizes hello (HiveQL) az SQL-lekérdezések eredményének. Hello beállítás tooselect között számos különböző típusú megjelenítések (táblázat, torta, vonal, terület vagy sáv) hello használatával lehetősége van **típus** menü gombok hello jegyzetfüzet:
> 
> 

![Logisztikai regresszió: ROC-görbe általános módszer](./media/machine-learning-data-science-spark-overview/pyspark-jupyter-autovisualization.png)

## <a name="whats-next"></a>A következő lépések
Most, hogy be vannak állítva a HDInsight Spark-fürt és a feltöltött hello Jupyter notebookok áll készen toowork hello témakörök, amelyek megfelelnek a toohello három PySpark notebookok keresztül. Azok hogyan tooexplore adatait és ezután hogyan toocreate és modellek felhasználását. az adatok feltárása, és hogyan modellezési notebook mutat be speciális hello tooinclude kereszt-ellenőrzési, a hyper-paraméter abszolút, és a modell kiértékelése. 

**Az adatok feltárása és Spark modellezés:** hello dataset felfedezése és létrehozása, pontozása és hello machine learning modellek kiértékelési feldolgozása révén hello [hello Spark adatok bináris besorolási és regressziós modell létrehozása MLlib eszközkészlet](machine-learning-data-science-spark-data-exploration-modeling.md) témakör.

**Modellhez tartozó felhasználás:** toolearn hogyan tooscore hello besorolási és regressziós modell létrehozása ebben a témakörben talál [pontszám és értékelje ki a Spark-beépített machine learning modellek](machine-learning-data-science-spark-model-consumption.md).

**Kereszt-ellenőrzési és hyperparameter abszolút**: lásd: [speciális adatok feltárása és Spark modellezés](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) meg, hogyan lehet a modellek betanítása a kereszt-ellenőrzési és a hyper-paraméter abszolút használatával

