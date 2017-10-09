---
title: "aaaSpark BI adatok képi megjelenítés eszközökkel on Azure HDInsight |} Microsoft Docs"
description: "Adatok képi megjelenítés eszközök segítségével analytics Apache Spark BI használata a HDInsight-fürtökön"
keywords: "Apache spark üzleti intelligencia, spark bi, spark adatábrázolási, spark üzleti intelligencia"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1448b536-9bc8-46bc-bbc6-d7001623642a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: ba4bfff737ce80ffca5c24f1c2c82a1447f467fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="apache-spark-bi-using-data-visualization-tools-with-azure-hdinsight"></a>Apache Spark BI adatok képi megjelenítés eszközökkel Azure hdinsightban

Ismerje meg, hogyan toouse adatábrázolási eszközök, például a Power BI és a Tableau tooanalyze a nyers minta adatkészletet, Apache Spark BI használata a HDInsight-fürtökön.

> [!NOTE]
> Ebben a cikkben leírt Üzletiintelligencia-eszközök kapcsolattal nem támogatott Spark 2.1 a Azure HDInsight 3.6 Preview-ban. Csak a Spark verziók 1.6-os és 2.0 (HDInsight 3.4, 3.5 rendre) támogatottak.
>

Ez az oktatóanyag egy HDInsight Spark-fürt Jupyter notebook, is érhető el. hello notebook élmény lehetővé teszi a hello Python kódtöredékek futtassa hello notebook magát. tooperform hello oktatóprogram belül a notebook Spark-fürt létrehozása, indítsa el a Jupyter notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), és futtassa a hello notebook **használata BI eszközök az Apache Spark on HDInsight.ipynb** alatt hello **Python**  mappa.

## <a name="prerequisites"></a>Előfeltételek

* A HDInsight az Apache Spark-fürt. Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).


## <a name="hivetable"></a>Adatok előkészítése Spark adatábrázolási

Ebben a szakaszban hello használjuk [Jupyter](https://jupyter.org) notebook egy HDInsight Spark-fürt toorun feladatokból, amely a nyers mintaadatok feldolgozni, és mentse egy tábla. hello mintaadatokat egy CSV-fájlt (hvac.csv) elérhető alapértelmezés szerint minden fürtön. Miután az adatok mentése táblázatként hello a következő szakaszban azt BI eszközök tooconnect toohello tábla, és hajtsa végre az adatmegjelenítéseket.

> [!NOTE]
> Akkor, ha hello szükséges lépések Ez a cikk utasításait hello befejezése után [interaktív lekérdezések futtatására egy HDInsight Spark-fürt](hdinsight-apache-spark-load-data-run-query.md), az alábbi 8 tooStep kihagyhatja.
>

1. A hello [Azure-portálon](https://portal.azure.com/), hello kezdőpulton, kattintson a Spark-fürt hello csempére (ha toohello kezdőpulton rögzítette azt). Tooyour fürt alapján is megtalálhatja **összes tallózása** > **a HDInsight-fürtök**.   

2. Hello Spark-fürt panelén kattintson **fürt irányítópult**, és kattintson a **Jupyter Notebook**. Ha a rendszer kéri, adja meg hello fürt hello rendszergazdai hitelesítő adataival.

   > [!NOTE]
   > A fürt URL-címet a böngészőben a következő megnyitásakor hello által is hello Jupyter Notebook lehet elérni. Cserélje le **CLUSTERNAME** hello néven a fürt:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. Hozzon létre egy notebookot. Kattintson a **New** (Új), majd a **PySpark** elemre.

    ![Jupyter notebook létrehozása az Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/create-jupyter-notebook-for-spark-bi.png "Apache Spark bi Jupyter notebook létrehozása")

4. Új notebook létrejött, és Untitled.pynb hello nevű. Hello notebook neve hello tetején kattintson, és adjon meg egy rövid nevet.

    ![Nevezze el a hello notebook Apache Spark bi](./media/hdinsight-apache-spark-use-bi-tools/jupyter-notebook-name-for-spark-bi.png "nevezze el a hello notebook Apache Spark BI")

5. Mivel a notebook PySpark kernelt hello hozott létre, nem kell toocreate semmilyen tartalmat explicit módon. hello Spark és Hive-környezetek automatikusan létrejönnek hello első kódcella futtatásakor. Ehhez a forgatókönyvhöz szükséges hello típusok importálásával megkezdése. toodo tehát kurzorral hello hello cellába, majd nyomja le az **SHIFT + ENTER**.

        from pyspark.sql import *

6. Töltse be a mintaadatokat egy ideiglenes táblába. Spark-fürt hdinsightban történő hello mintaadatfájlokat, létrehozásakor **hvac.csv**, a másolt toohello kapcsolódó tárfiók **\HdiSamples\HdiSamples\SensorSampleData\hvac**.

    A cella üres, illessze be a hello alábbi kódrészletet, és nyomja le az **SHIFT + ENTER**. Ezt a kódrészletet a következő táblába regisztrálja a hello adatok **hvac**.

        # Create an RDD from sample data
        hvacText = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        # Create a schema for our data
        Entry = Row('Date', 'Time', 'TargetTemp', 'ActualTemp', 'BuildingID')

        # Parse hello data and create a schema
        hvacParts = hvacText.map(lambda s: s.split(',')).filter(lambda s: s[0] != 'Date')
        hvac = hvacParts.map(lambda p: Entry(str(p[0]), str(p[1]), int(p[2]), int(p[3]), int(p[6])))

        # Infer hello schema and create a table       
        hvacTable = sqlContext.createDataFrame(hvac)
        hvacTable.registerTempTable('hvactemptable')
        dfw = DataFrameWriter(hvacTable)
        dfw.saveAsTable('hvac')

7. Győződjön meg arról, hogy hello tábla létrehozása sikeresen megtörtént. Használhatja a hello `%%sql` magic toorun Hive-lekérdezéseket közvetlenül. Hello kapcsolatos további információk `%%sql` magic és hello PySpark kernellel elérhető egyéb magics [Spark HDInsight-fürtökkel használt Jupyter notebookokban elérhető kernelek](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

        %%sql
        SHOW TABLES

    Hasonló kimenetnek látja, lent látható módon:

        +---------------+-------------+
        |tableName      |isTemporary  |
        +---------------+-------------+
        |hvactemptable  |true        |
        |hivesampletable|false        |
        |hvac           |false        |
        +---------------+-------------+

    Csak a tábla, amelyen a hello hamis hello **isTemporary** oszlop hello metaadattárhoz tárolódnak, és elérhető az hello Üzletiintelligencia-eszközök hive táblákat. Ebben az oktatóanyagban a Microsoft connect toohello **hvac** létrehozott tábla.

8. Győződjön meg arról, hogy hello tábla szánt hello adatokat tartalmaz. Egy üres cellába hello notebook, másolja a hello alábbi kódrészletet, és nyomja le az **SHIFT + ENTER**.

        %%sql
        SELECT * FROM hvac LIMIT 10

9. Állítsa le a hello notebook toorelease hello erőforrásokat. toodo Igen, a hello **fájl** hello notebook menüjében kattintson **zárja be és Halt**.

## <a name="powerbi"></a>A Power BI használata Spark adatábrázolási

> [!NOTE]
> Ez a szakasz esetén csak a HDInsight 3.4 Spark 1.6-os és Spark 2.0 a HDInsight 3.5 alkalmazható.
>
>

Ha hello adatok táblázatként mentette, használja a Power BI tooconnect toohello adatokat, és megjelenítheti azt toocreate jelentések irányítópultok stb.

1. Győződjön meg arról, hogy hozzáférési tooPower BI. A Power bi-ban az ingyenes kiadásra előfizetői kaphat [http://www.powerbi.com/](http://www.powerbi.com/).

2. Jelentkezzen be a túl[Power BI](http://www.powerbi.com/).

3. Hello aljáról hello bal oldali panelen, kattintson a **adatok beolvasása**.

4. A hello **adatok beolvasása** lap **importálni, vagy csatlakoztassa a tooData**, a **adatbázisok**, kattintson a **beolvasása**.

    ![Adatok beolvasása a Power bi-bA az Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "adatok beszerzése Apache Spark BI Power BI-bA")

5. Hello következő képernyőn kattintson a **a Spark on Azure HDInsight** majd **Connect**. Amikor a rendszer kéri, adja meg a hello fürt URL-CÍMÉT (`mysparkcluster.azurehdinsight.net`) és hello hitelesítő adatok tooconnect toohello fürt.

    ![Csatlakozás tooApache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-apache-spark-bi.png "tooApache Spark BI csatlakozás")

    Hello kapcsolat létrejötte után a Power BI adatok importálását a HDInsight Spark-fürt hello kezdődik.

6. A Power BI hello adatok importálására, és hozzáadja a **Spark** adatkészlet alapján hello **adatkészletek** fejléc. Kattintson a hello adatkészlet tooopen egy új munkalapra toovisualize hello adatokat. Hello munkalap jelentést is mentheti. a munkalapra, hello toosave **fájl** menüben kattintson **mentése**.

    ![A Power BI irányítópult Apache Spark BI csempéjén](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-tile-dashboard.png "Apache Spark BI csempét a Power BI-irányítópulton")
7. Figyelje meg, hogy hello **mezők** hello lista meg hello jobb **hvac** korábban létrehozott tábla. Hello tábla toosee hello mezők hello tábla, bontsa ki a korábban meghatározott jegyzetfüzet.

      ![Apache Spark BI irányítópulton lévő táblák listázása](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-display-tables.png "Apache Spark BI irányítópulton lévő táblák listázása")

8. Build a képi megjelenítés tooshow hello eltérés cél hőmérséklet és az egyes épület tényleges hőmérséklet között. toovisualize regisztrációhoz adatokat, válassza ki **területdiagram** (vörös téglalappal látható). toodefine hello tengely fogd és vidd hello **BuildingID** eleménél **tengely**, és **ActualTemp**/**TargetTemp** a mezők **érték**.

    ![Hozzon létre a Spark Apache Spark BI használatával adatmegjelenítésekkel](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "Spark létrehozása az adatmegjelenítéseket Apache Spark BI használatával")

9. Alapértelmezés szerint hello képi megjelenítés mutatja hello összegük **ActualTemp** és **TargetTemp**. Mindkét hello mezőit hello legördülő menüben válasszon **átlagos** tooget tényleges átlagosan és a cél-hőmérsékletek mindkét épületek.

    ![Hozzon létre a Spark Apache Spark BI használatával adatmegjelenítésekkel](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "Spark létrehozása az adatmegjelenítéseket Apache Spark BI használatával")

10. Az adatok vizuális hasonló toohello egy hello a képernyőfelvételen látható kell lennie. Vigye a kurzort hello képi megjelenítés tooget eszköztipp vonatkozó adatokat tartalmazó.

    ![Hozzon létre a Spark Apache Spark BI használatával adatmegjelenítésekkel](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "Spark létrehozása az adatmegjelenítéseket Apache Spark BI használatával")

11. Kattintson a **mentése** hello a felső menüben, és adja meg a jelentés nevét. Hello visual is rögzítheti. Képi megjelenítés rögzíti, amikor tárolása az irányítópulton, hello legutolsó értékét egy pillanat alatt követheti nyomon.

   A kívánt annyi képi megjelenítések is hozzáadhat ugyanahhoz az adatkészlethez hello és az adatok pillanatképe toohello irányítópulthoz rögzítheti őket. A HDInsight Spark-fürtjei is BI közvetlen kapcsolódás csatlakoztatott tooPower. Ez biztosítja, hogy a Power BI mindig rendelkezik hello legtöbb naprakész adatokat a fürt így tooschedule frissítések hello az adatkészlethez nem szükséges.

## <a name="tableau"></a>A Spark adatábrázolási Tableau asztal használata

> [!NOTE]
> Ez a szakasz esetén csak az Azure HDInsight létrehozott Spark 1.5.2-fürtök alkalmazható.
>
>

1. Telepítés [Tableau asztali](http://www.tableau.com/products/desktop) hello az Apache Spark BI oktatóanyag futtató számítógépen.

2. Győződjön meg arról, hogy számítógépen is van telepítve a Microsoft Spark ODBC-illesztőprogram. A hello illesztőprogram telepítése [Itt](http://go.microsoft.com/fwlink/?LinkId=616229).

1. Tableau asztal indításakor. Hello bal oldali ablaktáblán, a kiszolgáló tooconnect hello listája kattintson **Spark SQL**. Ha hello bal oldali ablaktáblán alapértelmezés szerint nem szerepel a Spark SQL, megtalálhatja azt kattintson **több kiszolgálók**.
2. A hello Spark SQL kapcsolati párbeszédpanelen adja meg hello hello képernyőfelvételen látható módon, és kattintson a **OK**.

    ![Csatlakozás tooa fürt az Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-tableau-apache-spark-bi.png "Connect tooa fürt az Apache Spark BI")

    hitelesítési legördülő listákból hello **Microsoft Azure HDInsight szolgáltatás** lehetőség csak akkor, ha telepítette a hello [Microsoft Spark ODBC-illesztőprogram](http://go.microsoft.com/fwlink/?LinkId=616229) hello számítógépen.
3. Hello következő képernyőn, a hello **séma** legördülő menüben kattintson a hello **található** ikonra, végül **alapértelmezett**.

    ![Található séma a következő Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-schema-apache-spark-bi.png "Apache Spark BI keresés sémája")
4. A hello **tábla** , majd kattintson a hello **található** ikon újra összes hello toolist Hive táblák hello fürt érhető el. Megtekintheti az hello **hvac** , korábban létrehozott hello notebook tábla.

    ![Az Apache Spark BI tábla található](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-table-apache-spark-bi.png "Apache Spark bi keresési tábla")
5. Áthúzása hello tábla toohello felső mezőbe a megfelelő hello. Tableau hello adatok importálására és hello séma példának piros hello mező alapján jeleníti meg.

    ![Adja hozzá a táblák tooTableau Apache Spark bi](./media/hdinsight-apache-spark-use-bi-tools/tableau-add-table-apache-spark-bi.png "Apache Spark bi táblák tooTableau hozzáadása")
6. Kattintson a hello **Munka1** hello bal alsó lapot. Ellenőrizze a képi megjelenítés hello átlagos cél- és a tényleges összes épület-hőmérsékletek jelennek meg. minden egyes dátum. A csomóponthúzási **dátum** és **épület azonosító** túl**oszlopok** és **tényleges Temp**/**cél Temp**túl**sorok**. A **jelek**, jelölje be **terület** toouse Spark adatábrázolási terület interaktív.

     ![Adja hozzá a mezőket a Spark adatábrázolási](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-add-fields.png "Spark adatábrázolási mezők hozzáadása")
7. Alapértelmezés szerint megjelenítendő hello hőmérséklet mezők szerint összesítést. Ha inkább tooshow hello átlaghőmérséklet, akkor teheti hello legördülő, az alább látható módon.

    ![Átlagos Spark adatok vizuális megjelenítéshez tartozó hőmérséklet érvénybe](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-average-temperature.png "átlagos Spark adatok vizuális megjelenítéshez tartozó hőmérséklet igénybe")

8. Akkor is Szuper-adhat egy hőmérséklet-leképezést több mint hello más tooget jobb arculatának cél és a tényleges hőmérsékletek közötti különbség. Helyezze át a hello egér toohello sarkában hello alacsonyabb terület térkép keretein belül hello leíró alakzat kiemelt piros kör látható. Húzza hello térkép toohello más térkép hello top és a kiadási hello egér piros téglalap kiemelt hello alakzat megjelenésekor.

    ![A Spark adatábrázolási maps egyesítése](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-merge-maps.png "egyesítési leképezi a Spark adatábrázolási")

     Az adatok vizuális hello képernyőfelvételen látható módon kell módosítani:

    ![A Spark adatábrázolási tableau kimeneti](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-tableau-output.png "Spark adatábrázolási Tableau kimenete")
9. Kattintson a **mentése** toosave hello munkalapra. Irányítópultok létrehozása, és adja hozzá egy vagy több lapok tooit.

## <a name="next-steps"></a>Következő lépések

Amennyiben megtanulta, hogyan toocreate egy fürtbe, Spark adatok keretek tooquery adatok létrehozása, és majd érheti el az adatok Üzletiintelligencia-eszközök. Most már megtekintheti toomanage fürterőforrások hello és a HDInsight Spark-fürtben lévő éppen futó feladatok hibakeresési utasításokat.

* [Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése](hdinsight-apache-spark-resource-manager.md)
* [Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban](hdinsight-apache-spark-job-debugging.md)

