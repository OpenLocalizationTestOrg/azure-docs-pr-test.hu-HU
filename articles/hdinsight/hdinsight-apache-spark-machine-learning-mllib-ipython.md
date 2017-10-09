---
title: "Példa a HDInsight - Azure Spark MLlib tartalmazó tanulási aaaMachine |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Spark MLlib toocreate egy machine learning-alkalmazást, amely elemzi a DataSet adatkészlet besorolásával keresztül logisztikai regresszió."
keywords: "Spark gépi tanulási, spark machine learning – példa"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c0fd4baa-946d-4e03-ad2c-a03491bd90c8
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 5c3b83482de5d8fba224398aaafe07fa67ec1fb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-spark-mllib-toobuild-a-machine-learning-application-and-analyze-a-dataset"></a>Spark MLlib toobuild használata a machine learning-alkalmazás, és elemezze a DataSet adatkészlet

Megtudhatja, hogyan toouse Spark **MLlib** toocreate a gépi tanulási alkalmazás toodo egyszerű prediktív elemzési egy megnyitott adatkészlethez. A Spark beépített gépi tanulási szalagtárak, a példában az *besorolás* logisztikai regresszió keresztül. 

> [!TIP]
> Ebben a példában, Ön által létrehozott hdinsight (Linux) Spark-fürtön Jupyter notebook is érhető el. hello notebook élmény lehetővé teszi a hello Python kódtöredékek futtassa hello notebook magát. toofollow hello oktatóprogram belül a notebook létrehozása egy Spark fürt és a Jupyter notebook indítási (`https://CLUSTERNAME.azurehdinsight.net/jupyter`). Futtassa a hello notebook **Spark Machine Learning - étele ellenőrző adatok MLlib.ipynb a prediktív elemzési** alatt hello **Python** mappa.
>
>

MLlib egy Spark Alapkönyvtár, amely számos hasznos segédprogramokat biztosít gépi tanulási feladatok, beleértve a segédprogramok:

* Besorolás
* Regressziós
* Fürtszolgáltatás
* A témakör modellezési
* Szinguláris érték felbontás ellen (SVD) és egyszerű összetevő elemzés (PEM)
* Alapul, tesztelése és minta számításához

## <a name="what-are-classification-and-logistic-regression"></a>Mik azok a besorolás és logisztikai regresszió?
*Besorolási*népszerű Machine learning-feladat, hello folyamat a bemeneti adatok rendezése kategóriákba. A besorolási algoritmus toofigure tudhat meg a tooassign "címkék" tooinput adatok megadandó hello feladat. Például egy gépi tanulási algoritmus, amely a bemeneti készlet adatokat fogadja a sikerült véleménye és felosztása hello készlet két kategóriába sorolhatók: kell értékesítő készletek és a készletek, amelyek kell tartania.

Logisztikai regresszió hello algoritmus besorolási használó. A Spark logisztikai regresszió API akkor hasznos, ha *bináris osztályozási*, vagy egy két bemeneti adatok besorolása. További információ a logisztikai regresszió: [Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression).

Az összegzés hello logisztikai regresszió illesztése egy *logisztikai függvény* , amely lehet, hogy egy bemeneti vektort tartozik egy csoport vagy más hello használt toopredict hello valószínűség.  

## <a name="predictive-analysis-example-on-food-inspection-data"></a>A prediktív elemzés példa étele ellenőrző adatok
Ebben a példában használja Spark tooperform néhány prediktív elemzési étele ellenőrző adatokat (**Food_Inspections1.csv**), amely lett beszerzett hello [város a Chicagói adat portált](https://data.cityofchicago.org/). Ez az adatkészlet chicagóban, beleértve az egyes kialakítása végezték étele kialakítása ellenőrzésekre kapcsolatos információt tartalmazza, hello szabálytalanság található (ha van ilyen), és hello hello vizsgálat eredményeit. hello fürthöz társított hello tárfiókban már hello CSV adatfájl **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**.

Hello az alábbi lépések fejlesztése a modell toosee mi toopass tart, vagy a étele ellenőrzése sikertelen.

## <a name="start-building-a-spark-mmlib-machine-learning-app"></a>Indítsa el a Spark MMLib machine learning-alkalmazás létrehozása
1. A hello [Azure-portálon](https://portal.azure.com/), hello kezdőpulton, kattintson a Spark-fürt hello csempére (ha toohello kezdőpulton rögzítette azt). Tooyour fürt alapján is megtalálhatja **összes tallózása** > **a HDInsight-fürtök**.   
1. Hello Spark-fürt panelén kattintson **fürt irányítópult**, és kattintson a **Jupyter Notebook**. Ha a rendszer kéri, adja meg hello fürt hello rendszergazdai hitelesítő adataival.

   > [!NOTE]
   > A fürt URL-címet a böngészőben a következő megnyitásakor hello által is hello Jupyter Notebook lehet elérni. Cserélje le **CLUSTERNAME** hello néven a fürt:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
1. Hozzon létre egy notebookot. Kattintson a **New** (Új), majd a **PySpark** elemre.

    ![A Jupyter notebook létrehozása](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-create-jupyter.png "egy új Jupyter notebook létrehozása")
1. Új notebook létrejött, és Untitled.pynb hello nevű. Hello notebook neve hello tetején kattintson, és adjon meg egy rövid nevet.

    ![Adjon meg egy nevet hello notebook](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-name-jupyter.png "adjon meg egy nevet hello notebook")
1. Mivel a notebook PySpark kernelt hello hozott létre, nem kell toocreate semmilyen tartalmat explicit módon. hello Spark és Hive-környezetek automatikusan létrejönnek hello első kódcella futtatásakor. A machine learning alkalmazás ehhez a forgatókönyvhöz szükséges hello típusok importálásával felépítése elindíthatja. toodo tehát kurzorral hello hello cellába, majd nyomja le az **SHIFT + ENTER**.

        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        from pyspark.sql.functions import UserDefinedFunction
        from pyspark.sql.types import *

## <a name="construct-an-input-dataframe"></a>Egy bemeneti dataframe szerkezet
Használhatunk `sqlContext` tooperform átalakítások, a strukturált adatok. első feladata tooload hello mintaadatok hello ((**Food_Inspections1.csv**)) egy Spark SQL *dataframe*.

1. Mivel hello nyers adatok CSV formátumban, igazolnia kell toouse hello Spark környezetben toopull hello fájl minden sora a memóriába strukturálatlan szövegként; Ezután használhatja Python a fürt megosztott kötetei szolgáltatás könyvtár tooparse soronként külön-külön.

        def csvParse(s):
            import csv
            from StringIO import StringIO
            sio = StringIO(s)
            value = csv.reader(sio).next()
            sio.close()
            return value

        inspections = sc.textFile('wasb:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv')\
                        .map(csvParse)
1. A Microsoft most egy RDD hello CSV-fájl lehet.  toounderstand hello séma hello adatok, nem beolvasni egy sor hello RDD.

        inspections.take(1)

    Hello hasonló kimenetnek kell megjelennie:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [['413707',
          'LUNA PARK INC',
          'LUNA PARK  DAY CARE',
          '2049789',
          "Children's Services Facility",
          'Risk 1 (High)',
          '3250 W FOSTER AVE ',
          'CHICAGO',
          'IL',
          '60625',
          '09/21/2010',
          'License-Task Force',
          'Fail',
          '24. DISH WASHING FACILITIES: PROPERLY DESIGNED, CONSTRUCTED, MAINTAINED, INSTALLED, LOCATED AND OPERATED - Comments: All dishwashing machines must be of a type that complies with all requirements of hello plumbing section of hello Municipal Code of Chicago and Rules and Regulation of hello Board of Health. OBSEVERD hello 3 COMPARTMENT SINK BACKING UP INTO hello 1ST AND 2ND COMPARTMENT WITH CLEAR WATER AND SLOWLY DRAINING OUT. INST NEED HAVE IT REPAIR. CITATION ISSUED, SERIOUS VIOLATION 7-38-030 H000062369-10 COURT DATE 10-28-10 TIME 1 P.M. ROOM 107 400 W. SURPERIOR. | 36. LIGHTING: REQUIRED MINIMUM FOOT-CANDLES OF LIGHT PROVIDED, FIXTURES SHIELDED - Comments: Shielding tooprotect against broken glass falling into food shall be provided for all artificial lighting sources in preparation, service, and display facilities. LIGHT SHIELD ARE MISSING UNDER HOOD OF  COOKING EQUIPMENT AND NEED tooREPLACE LIGHT UNDER UNIT. 4 LIGHTS ARE OUT IN hello REAR CHILDREN AREA,IN hello KINDERGARDEN CLASS ROOM. 2 LIGHT ARE OUT EAST REAR, LIGHT FRONT WEST ROOM. NEED tooREPLACE ALL LIGHT THAT ARE NOT WORKING. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: hello walls and ceilings shall be in good repair and easily cleaned. MISSING CEILING TILES WITH STAINS IN WEST,EAST, IN FRONT AREA WEST, AND BY hello 15MOS AREA. NEED tooBE REPLACED. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair. SPLASH GUARDED ARE NEEDED BY hello EXPOSED HAND SINK IN hello KITCHEN AREA | 34. FLOORS: CONSTRUCTED PER CODE, CLEANED, GOOD REPAIR, COVING INSTALLED, DUST-LESS CLEANING METHODS USED - Comments: hello floors shall be constructed per code, be smooth and easily cleaned, and be kept clean and in good repair. INST NEED tooELEVATE ALL FOOD ITEMS 6INCH OFF hello FLOOR 6 INCH AWAY FORM WALL.  ',
          '41.97583445690982',
          '-87.7107455232781',
          '(41.97583445690982, -87.7107455232781)']]
1. hello előző kimeneti biztosítanak számunkra hello séma hello bemeneti fájl képet. Ez magában foglalja a minden kialakítása, kialakítása, hello címét, és hello ellenőrzések hello helyét, többek között a hello adatok hello típusú hello nevét. Most válassza ki, amelyek hasznosak a prediktív elemzési néhány oszlopot, és csoport hello eredménye egy dataframe, mely azt, majd használja toocreate ideiglenes táblából.

        schema = StructType([
        StructField("id", IntegerType(), False),
        StructField("name", StringType(), False),
        StructField("results", StringType(), False),
        StructField("violations", StringType(), True)])

        df = sqlContext.createDataFrame(inspections.map(lambda l: (int(l[0]), l[1], l[12], l[13])) , schema)
        df.registerTempTable('CountResults')
1. Most már rendelkezik egy *dataframe*, `df` amelyen most a elemzés elvégezni. Egy ideiglenes tábla hívás is tudunk **CountResults**. A Microsoft hello dataframe jelentőséggel négy oszlop szerepel: **azonosító**, **neve**, **eredmények**, és **megsértésének**.

    Folytassuk hello adatok mintát:

        df.show(5)

    Hello hasonló kimenetnek kell megjelennie:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +------+--------------------+-------+--------------------+
        |    id|                name|results|          violations|
        +------+--------------------+-------+--------------------+
        |413707|       LUNA PARK INC|   Fail|24. DISH WASHING ...|
        |391234|       CAFE SELMARIE|   Fail|2. FACILITIES too...|
        |413751|          MANCHU WOK|   Pass|33. FOOD AND NON-...|
        |413708|BENCHMARK HOSPITA...|   Pass|                    |
        |413722|           JJ BURGER|   Pass|                    |
        +------+--------------------+-------+--------------------+

## <a name="understand-hello-data"></a>Hello adatok ismertetése
1. Kezdjük tooget egyfajta az adatkészletet tartalmaz. Például, melyek hello eltérő értéket hello **eredmények** oszlop?

        df.select('results').distinct().show()

    Hello hasonló kimenetnek kell megjelennie:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +--------------------+
        |             results|
        +--------------------+
        |                Fail|
        |Business Not Located|
        |                Pass|
        |  Pass w/ Conditions|
        |     Out of Business|
        +--------------------+
1. Gyors képi megjelenítés segíthet OK kapcsolatos ezek eredményekkel hello terjesztése. Egy ideiglenes tábla hello adatok már tudunk **CountResults**. A következő SQL-lekérdezést hello tábla tooget hello szeretné jobban megismerni hello eredmények elosztása hogyan futtathat.

        %%sql -o countResultsdf
        SELECT results, COUNT(results) AS cnt FROM CountResults GROUP BY results

    Hello `%%sql` magic követ `-o countResultsdf` biztosítja, hogy hello lekérdezés kimenetét hello hello (általában hello headnode hello fürt) Jupyter kiszolgálón helyileg megőrződjenek. hello kimeneti tárolva a [Pandas](http://pandas.pydata.org/) dataframe hello a megadott név **countResultsdf**.

    Hello hasonló kimenetnek kell megjelennie:

    ![SQL-lekérdezés kimeneti](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-query-output.png "SQL-lekérdezés kimenete")

    Hello kapcsolatos további információk `%%sql` magic és hello PySpark kernellel elérhető egyéb magics [Spark HDInsight-fürtökkel használt Jupyter notebookokban elérhető kernelek](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).
1. Matplotlib is használható marad, a szalagtár tooconstruct képi megjelenítés adatainak, toocreate rajzot használja. Mert a rajzolási hello hello helyileg kell létrehozni a megőrzött **countResultsdf** dataframe, hello kódrészletet a következővel kell kezdődnie hello `%%local` magic. Ez biztosítja, hogy hello kód hello Jupyter kiszolgálón helyileg futnak.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt

        labels = countResultsdf['results']
        sizes = countResultsdf['cnt']
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    Hello hasonló kimenetnek kell megjelennie:

    ![Spark gépi tanulási a alkalmazás kimeneti - öt különböző vizsgálat eredményekkel kördiagram](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-1.png "Spark gépi tanulási a eredmény kimenete")
1. Láthatja, hogy vannak-e 5 különböző eredményeket, amelyeken az ellenőrzés:

   * Nem található üzleti
   * Sikertelen
   * Fázis
   * Feltételek keresztüli rendelkező PSS
   * Üzleti kívül

     Ossza meg velünk fejlesztése modell, amely kitalálni a hello étele hálózatfelügyeleti, az adott hello megsértésének eredménye. Mivel logisztikai regresszió egy bináris osztályozási módszer, így logika toogroup adataink két kategóriába sorolhatók: **sikertelen** és **átadni**. Egy "átadni keresztüli rendelkező feltételek" még nem állt le a hozzáférési, így amikor hello modell betanításához azt adatraktárakban hello két eredmények egyenértékű. Adatok a hello más eredmények ("Üzleti nem található" vagy "Out üzleti") esetén nem lehet hasznos, hogy távolítsa el azokat a gyakorlókészlethez. Ennek ellenére kell, óta ezek két kategóriába mégis alkotó hello eredmények nagyon kis százalékát.
1. Ossza meg velünk lépjen tovább, és alakítsa át a meglévő dataframe (`df`) egy új dataframe, ahol minden egyes ellenőrzés címke-megsértésének párban szerepel-e be. Ebben az esetben a címke `0.0` jelenti. a hiba, a címkék `1.0` sikeres, és a címke jelöli `-1.0` egyes eredményeket e két mellett jelöli. A Microsoft kiszűrhetők az egyéb eredmények hello új adatok keret számításakor.

        def labelForResults(s):
            if s == 'Fail':
                return 0.0
            elif s == 'Pass w/ Conditions' or s == 'Pass':
                return 1.0
            else:
                return -1.0
        label = UserDefinedFunction(labelForResults, DoubleType())
        labeledData = df.select(label(df.results).alias('label'), df.violations).where('label >= 0')

    toosee milyen hello feliratú adatok tűnik, most beolvasni egy sort.

        labeledData.take(1)

    Hello hasonló kimenetnek kell megjelennie:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [Row(label=0.0, violations=u"41. PREMISES MAINTAINED FREE OF LITTER, UNNECESSARY ARTICLES, CLEANING  EQUIPMENT PROPERLY STORED - Comments: All parts of hello food establishment and all parts of hello property used in connection with hello operation of hello establishment shall be kept neat and clean and should not produce any offensive odors.  REMOVE MATTRESS FROM SMALL DUMPSTER. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: hello walls and ceilings shall be in good repair and easily cleaned.  REPAIR MISALIGNED DOORS AND DOOR NEAR ELEVATOR.  DETAIL CLEAN BLACK MOLD LIKE SUBSTANCE FROM WALLS BY BOTH DISH MACHINES.  REPAIR OR REMOVE BASEBOARD UNDER DISH MACHINE (LEFT REAR KITCHEN). SEAL ALL GAPS.  REPLACE MILK CRATES USED IN WALK IN COOLERS AND STORAGE AREAS WITH PROPER SHELVING AT LEAST 6' OFF hello FLOOR.  | 38. VENTILATION: ROOMS AND EQUIPMENT VENTED AS REQUIRED: PLUMBING: INSTALLED AND MAINTAINED - Comments: hello flow of air discharged from kitchen fans shall always be through a duct tooa point above hello roofline.  REPAIR BROKEN VENTILATION IN MEN'S AND WOMEN'S WASHROOMS NEXT tooDINING AREA. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair.  REPAIR DAMAGED PLUG ON LEFT SIDE OF 2 COMPARTMENT SINK.  REPAIR SELF CLOSER ON BOTTOM LEFT DOOR OF 4 DOOR PREP UNIT NEXT tooOFFICE.")]

## <a name="create-a-logistic-regression-model-from-hello-input-dataframe"></a>A bemeneti dataframe hello logisztikai regresszió modell létrehozása
A végső feladat adatok címkével olyan formátumra, amely logisztikai regresszió által elemezhető tooconvert hello. hello bemeneti tooa logisztikai regresszió algoritmus kell lenniük, amelyek *címke-funkció vektoros párok*, ahol a "szolgáltatás vektoros" hello hello bemeneti pontot jelölő számok vektor. Igen szükséges tooconvert hello "megsértésének" oszlop, amely félig strukturált és szabad szöveges, tooan tömb valós szám, amely a gép könnyen értelmezhető formára sok megjegyzéseket tartalmaz.

Feldolgozási természetes nyelvű egy szabványos machine learning megközelítés tooassign minden különálló egy "index" szó, és akkor továbbítja a vektoros toohello a gépi tanulási algoritmus, úgy, hogy minden egyes indexértéket tartalmaz hello relatív gyakoriságot hello szöveges szó karakterlánc.

MLlib biztosít egy egyszerűen tooperform ezt a műveletet. Első lépésként "tokenize" minden megsértésének karakterlánc tooget hello a szöveg minden karakterlánc. Ezt követően egy `HashingTF` be a szolgáltatás vektor, amely majd átadott toohello logisztikai regresszió algoritmus tooconstruct jogkivonatok egyes készlete tooconvert egy modell. Azt végezze el az összes lépés sorrendben használja a "futószalag".

    tokenizer = Tokenizer(inputCol="violations", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

    model = pipeline.fit(labeledData)

## <a name="evaluate-hello-model-on-a-separate-test-dataset"></a>Egy külön vizsgálat adatkészlethez hello modell kiértékelése
Korábban létrehozott hello modell használhatjuk túl*előrejelzése* milyen hello új ellenőrzések eredményeinek lesz, a megfigyelt hello szabálysértések alapján. Jelenleg ez a modell hello adatkészlethez betanítása **Food_Inspections1.csv**. Ossza meg velünk használjon egy második dataset **Food_Inspections2.csv**, túl*kiértékelése* ebben a modellben az új adatok erősségével hello. A második készlet (**Food_Inspections2.csv**) kell lennie a hello hello-fürthöz tartozó alapértelmezett tárolókat.

1. hello következő kódrészlettel létrehoz egy új dataframe **predictionsDf** tartalmazó hello előrejelzés hello modell által generált. hello részlet is létrehoz egy ideiglenes táblát **előrejelzéseket** hello dataframe alapján.

        testData = sc.textFile('wasb:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections2.csv')\
                 .map(csvParse) \
                 .map(lambda l: (int(l[0]), l[1], l[12], l[13]))
        testDf = sqlContext.createDataFrame(testData, schema).where("results = 'Fail' OR results = 'Pass' OR results = 'Pass w/ Conditions'")
        predictionsDf = model.transform(testDf)
        predictionsDf.registerTempTable('Predictions')
        predictionsDf.columns

    Hello hasonló kimenetnek kell megjelennie:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        ['id',
         'name',
         'results',
         'violations',
         'words',
         'features',
         'rawPrediction',
         'probability',
         'prediction']
1. Tekintse meg hello előrejelzéseket egyikét. Futtassa ezt a kódrészletet:

        predictionsDf.take(1)

   Első bejegyzés hello hello teszt adatkészlet előrejelzéshez van.
1. Hello `model.transform()` módhoz hello azonos átalakítása tooany új adatok hello ugyanazon séma, és hogyan tooclassify hello adatok előrejelzése érkeznie. Tehetünk ennek néhány egyszerű statisztika tooget egyfajta hogyan pontos az előrejelzés volt:

        numSuccesses = predictionsDf.where("""(prediction = 0 AND results = 'Fail') OR
                                              (prediction = 1 AND (results = 'Pass' OR
                                                                   results = 'Pass w/ Conditions'))""").count()
        numInspections = predictionsDf.count()

        print "There were", numInspections, "inspections and there were", numSuccesses, "successful predictions"
        print "This is a", str((float(numSuccesses) / float(numInspections)) * 100) + "%", "success rate"

    hello kimeneti következőhöz hasonló hello:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        There were 9315 inspections and there were 8087 successful predictions
        This is a 86.8169618894% success rate

    Logisztikai regresszió használata Spark lehetőséget ad olyan pontos modell hello kapcsolat megsértésének leírását angol nyelven, és hogy egy adott üzleti volna továbbítja, vagy a étele ellenőrzése sikertelen.

## <a name="create-a-visual-representation-of-hello-prediction"></a>Hello előrejelzés vizuális ábrázolását létrehozása
Most azt állíthat össze egy, az ebben a tesztben hello eredmény okból végső képi megjelenítés toohelp.

1. Először hello különböző előrejelzéseket beolvasásával és eredményeinek hello **előrejelzéseket** korábban létrehozott ideiglenes tábla. hello következő lekérdezések külön hello kimeneti adatok *true_positive*, *false_positive*, *true_negative*, és *false_negative*. Az alábbi hello lekérdezésekben, azt kapcsolja ki a képi megjelenítés használatával `-q` hello kimeneti is mentheti, és (használatával `-o`), majd használható a hello dataframes `%%local` magic.

        %%sql -q -o true_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND results = 'Fail'

        %%sql -q -o false_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND (results = 'Pass' OR results = 'Pass w/ Conditions')

        %%sql -q -o true_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND results = 'Fail'

        %%sql -q -o false_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND (results = 'Pass' OR results = 'Pass w/ Conditions')
1. Végül, használja a következő kódrészletet toogenerate hello rajzot használatával hello **Matplotlib**.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt

        labels = ['True positive', 'False positive', 'True negative', 'False negative']
        sizes = [true_positive['cnt'], false_positive['cnt'], false_negative['cnt'], true_negative['cnt']]
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    A következő kimeneti hello kell megjelennie:

    ![Spark gépi tanulási alkalmazás kimeneti - Kördiagram százalékos sikertelen étele ellenőrzések. ] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-2.png "Spark gépi tanulási a eredmény kimenete")

    Ezen a diagramon "pozitív" nem sikerült toohello étele hálózatfelügyeleti, hivatkozik, amíg negatív eredmény hálózatfelügyeleti átadott tooa hivatkozik.

## <a name="shut-down-hello-notebook"></a>Hello notebook leállítása
Miután befejezte a hello alkalmazást futtat, állítsa le a hello notebook toorelease hello erőforrásokat. toodo Igen, a hello **fájl** hello notebook menüjében kattintson **zárja be és Halt**. Ezzel leállítja és bezárja a notebookot hello.

## <a name="seealso"></a>Lásd még:
* [Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Forgatókönyvek
* [Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel](hdinsight-apache-spark-use-bi-tools.md)
* [Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására](hdinsight-apache-spark-eventhub-streaming.md)
* [A webhelynapló elemzése a Spark on HDInsight használatával](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Alkalmazások létrehozása és futtatása
* [Önálló alkalmazás létrehozása a Scala használatával](hdinsight-apache-spark-create-standalone-application.md)
* [Feladatok távoli futtatása Spark-fürtön a Livy használatával](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Eszközök és bővítmények
* [Toocreate IntelliJ IDEA HDInsight-eszközei beépülő használja, és küldje el a Spark Scala-alkalmazások](hdinsight-apache-spark-intellij-tool-plugin.md)
* [IntelliJ IDEA toodebug Spark-alkalmazások HDInsight-eszközei beépülő távolról használni](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Zeppelin notebookok használata Spark-fürttel HDInsighton](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Külső csomagok használata Jupyter notebookokkal](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Erőforrások kezelése
* [Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése](hdinsight-apache-spark-resource-manager.md)
* [Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban](hdinsight-apache-spark-job-debugging.md)
