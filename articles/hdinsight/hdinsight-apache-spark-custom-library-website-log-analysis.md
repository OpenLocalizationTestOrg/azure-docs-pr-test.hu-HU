---
title: "a Spark - Azure Python könyvtárak aaaAnalyze webhelyek naplóinak |} Microsoft Docs"
description: "A notebook bemutatja, hogyan tooanalyze naplózni a Spark on Azure Hdinsighttal egyéni szalagtárat használ."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 8c61c70f-fe7f-4f0f-a4ab-0cccee5668c9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 29e4308b2a359aee6d69494a98307d4da07f7909
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-website-logs-using-a-custom-python-library-with-spark-cluster-on-hdinsight"></a>Egy egyéni Python kódtár használata a HDInsight Spark-fürt webhelyek naplóinak elemzése

A notebook bemutatja, hogyan tooanalyze naplózni a Spark on HDInsight az egyéni szalagtárat használ. hello egyéni tárát használjuk egy Python kódtár nevű **iislogparser.py**.

> [!TIP]
> Ez az oktatóanyag, Ön által létrehozott hdinsight (Linux) Spark-fürtön Jupyter notebook is érhető el. hello notebook élmény lehetővé teszi a hello Python kódtöredékek futtassa hello notebook magát. tooperform hello oktatóprogram belül a notebook Spark-fürt létrehozása, indítsa el a Jupyter notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), majd futtassa a hello notebook **naplóinak elemzése a Spark egy egyéni library.ipynb használatával** hello alatt  **PySpark** mappa.
>
>

**Előfeltételek:**

Hello következő kell rendelkeznie:

* Azure-előfizetés. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* A HDInsight az Apache Spark-fürt. Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="save-raw-data-as-an-rdd"></a>Nyers adatok elmentse egy RDD
Ebben a szakaszban hello használjuk [Jupyter](https://jupyter.org) Apache Spark-fürt a HDInsight toorun feladatok, amely a nyers mintaadatok feldolgozni, és mentse egy Hive tábla társított notebook. hello mintaadatokat egy CSV-fájlt (hvac.csv) elérhető alapértelmezés szerint minden fürtön.

Miután az adatok mentése Hive tábla hello a következő szakaszban azt csatlakoztassa toohello Hive tábla a Power BI és a Tableau Üzletiintelligencia-eszközök segítségével.

1. A hello [Azure-portálon](https://portal.azure.com/), hello kezdőpulton, kattintson a Spark-fürt hello csempére (ha toohello kezdőpulton rögzítette azt). Tooyour fürt alapján is megtalálhatja **összes tallózása** > **a HDInsight-fürtök**.   
2. Hello Spark-fürt panelén kattintson **fürt irányítópult**, és kattintson a **Jupyter Notebook**. Ha a rendszer kéri, adja meg hello fürt hello rendszergazdai hitelesítő adataival.

   > [!NOTE]
   > A fürt URL-címet a böngészőben a következő megnyitásakor hello által is hello Jupyter Notebook lehet elérni. Cserélje le **CLUSTERNAME** hello néven a fürt:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Hozzon létre új notebookot. Kattintson a **New** (Új), majd a **PySpark** elemre.

    ![Új Jupyter notebook létrehozása](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-create-jupyter-notebook.png "Új Jupyter notebook létrehozása")
4. Új notebook létrejött, és Untitled.pynb hello nevű. Hello notebook neve hello tetején kattintson, és adjon meg egy rövid nevet.

    ![Adjon meg egy nevet hello notebook](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-name-jupyter-notebook.png "adjon meg egy nevet hello notebook")
5. Mivel a notebook PySpark kernelt hello hozott létre, nem kell toocreate semmilyen tartalmat explicit módon. hello Spark és Hive-környezetek automatikusan létrehozza az Ön hello első kódcella futtatásakor. Ehhez a forgatókönyvhöz szükséges típusok hello importálásával megkezdése. Illessze be az alábbi részlet egy üres cellába hello, és nyomja le az **SHIFT + ENTER**.

        from pyspark.sql import Row
        from pyspark.sql.types import *


1. Hozzon létre egy minta hello napló már rendelkezésre álló adatok használatával hello fürtön RDD. Hello hello fürthöz tartozó alapértelmezett tárfiók hello az adatok eléréséhez **\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log**.

        logs = sc.textFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log')


1. Kérje le egy mintanaplót beállítása tooverify hello a korábbi lépés sikeresen befejeződött.

        logs.take(5)

    Egy kimeneti hasonló toohello következő kell megjelennie:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [u'#Software: Microsoft Internet Information Services 8.0',
         u'#Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47']

## <a name="analyze-log-data-using-a-custom-python-library"></a>Egy egyéni Python kódtár használatával napló adatok elemzése
1. A fenti hello kimenetében hello első néhány sor hello fejléc-információ tartalmazza, és minden fennmaradó sor megfelel, hogy a fejléc ismertetett hello séma. Ilyen naplók elemzése bonyolult lehet. Igen, egy egyéni Python kódtár használjuk (**iislogparser.py**), amely lehetővé teszi az ilyen sokkal könnyebben naplók elemzése. Alapértelmezés szerint ezt a szalagtárat megtalálható a következő hdinsight Spark-fürt **/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py**.

    Azonban ez a tár nincs hello `PYTHONPATH` , nem tudjuk használni, például import utasítás segítségével `import iislogparser`. toouse ezt a tárat, azt el kell osztania az tooall hello munkavégző csomópontokhoz. Futtassa a következő kódrészletet hello.

        sc.addPyFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py')


1. `iislogparser`funkció `parse_log_line` , amely visszaadja a `None` Ha egy napló sor a fejlécsor lesz, és hello példányának `LogLine` osztály, ha egy napló sor ütközik. Használjon hello `LogLine` osztály tooextract csak hello hello RDD napló sorok:

        def parse_line(l):
            import iislogparser
            return iislogparser.parse_log_line(l)
        logLines = logs.map(parse_line).filter(lambda p: p is not None).cache()
2. Kibontott napló sorok tooverify, amely hello sikeresen befejeződött. lépés néhány beolvasása.

       logLines.take(2)

   hello kimeneti hasonló toohello következő legyen:

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       [2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46,
        2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32]
3. Hello `LogLine` osztály, viszont az például néhány hasznos metódusokkal rendelkezik `is_error()`, amely adja vissza, hogy rendelkezik-e a naplóbejegyzés hibakódot. Hibák száma a toocompute hello kibontott hello napló sorok használja, és ezután minden hello hibák tooa másik fájlt.

       errors = logLines.filter(lambda p: p.is_error())
       numLines = logLines.count()
       numErrors = errors.count()
       print 'There are', numErrors, 'errors and', numLines, 'log entries'
       errors.map(lambda p: str(p)).saveAsTextFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b-2.log')

   Hello hasonló kimenetnek kell megjelennie:

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       There are 30 errors and 646 log entries
4. Is **Matplotlib** tooconstruct hello adatok képi megjelenítés. Például ha azt szeretné, hogy hosszú ideig futó kérelmek tooisolate hello okait, érdemes toofind hello fájlok, amelyek hello legtöbb idő tooserve átlagosan.
   az alábbi hello részlet hello felső 25 erőforrást, amelyre szükség volt a legtöbb idő tooserve kérelmet kéri le.

       def avgTimeTakenByKey(rdd):
           return rdd.combineByKey(lambda line: (line.time_taken, 1),
                                   lambda x, line: (x[0] + line.time_taken, x[1] + 1),
                                   lambda x, y: (x[0] + y[0], x[1] + y[1]))\
                     .map(lambda x: (x[0], float(x[1][0]) / float(x[1][1])))

       avgTimeTakenByKey(logLines.map(lambda p: (p.cs_uri_stem, p))).top(25, lambda x: x[1])

   Hello hasonló kimenetnek kell megjelennie:

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       [(u'/blogposts/mvc4/step13.png', 197.5),
        (u'/blogposts/mvc2/step10.jpg', 179.5),
        (u'/blogposts/extractusercontrol/step5.png', 170.0),
        (u'/blogposts/mvc4/step8.png', 159.0),
        (u'/blogposts/mvcrouting/step22.jpg', 155.0),
        (u'/blogposts/mvcrouting/step3.jpg', 152.0),
        (u'/blogposts/linqsproc1/step16.jpg', 138.75),
        (u'/blogposts/linqsproc1/step26.jpg', 137.33333333333334),
        (u'/blogposts/vs2008javascript/step10.jpg', 127.0),
        (u'/blogposts/nested/step2.jpg', 126.0),
        (u'/blogposts/adminpack/step1.png', 124.0),
        (u'/BlogPosts/datalistpaging/step2.png', 118.0),
        (u'/blogposts/mvc4/step35.png', 117.0),
        (u'/blogposts/mvcrouting/step2.jpg', 116.5),
        (u'/blogposts/aboutme/basketball.jpg', 109.0),
        (u'/blogposts/anonymoustypes/step11.jpg', 109.0),
        (u'/blogposts/mvc4/step12.png', 106.0),
        (u'/blogposts/linq8/step0.jpg', 105.5),
        (u'/blogposts/mvc2/step18.jpg', 104.0),
        (u'/blogposts/mvc2/step11.jpg', 104.0),
        (u'/blogposts/mvcrouting/step1.jpg', 104.0),
        (u'/blogposts/extractusercontrol/step1.png', 103.0),
        (u'/blogposts/sqlvideos/sqlvideos.jpg', 102.0),
        (u'/blogposts/mvcrouting/step21.jpg', 101.0),
        (u'/blogposts/mvc4/step1.png', 98.0)]
5. Ez az információ használatát a tartománynevekben rajzolási hello is jelenthet. Egy első lépés toocreate rajzot, ossza meg velünk először hozzon létre egy ideiglenes tábla **AverageTime**. Ha bármely szokatlan késés igényeiben jelentkező történt egy adott időpontban hello tábla csoportok hello által idő toosee naplózza.

       avgTimeTakenByMinute = avgTimeTakenByKey(logLines.map(lambda p: (p.datetime.minute, p))).sortByKey()
       schema = StructType([StructField('Minutes', IntegerType(), True),
                            StructField('Time', FloatType(), True)])

       avgTimeTakenByMinuteDF = sqlContext.createDataFrame(avgTimeTakenByMinute, schema)
       avgTimeTakenByMinuteDF.registerTempTable('AverageTime')
6. Ezután futtassa a következő SQL-lekérdezés tooget hello bejegyzésekkel hello a hello **AverageTime** tábla.

       %%sql -o averagetime
       SELECT * FROM AverageTime

   Hello `%%sql` magic követ `-o averagetime` biztosítja, hogy hello lekérdezés kimenetét hello hello (általában hello headnode hello fürt) Jupyter kiszolgálón helyileg megőrződjenek. hello kimeneti tárolva a [Pandas](http://pandas.pydata.org/) dataframe hello a megadott név **averagetime**.

   Hello hasonló kimenetnek kell megjelennie:

   ![SQL-lekérdezés kimeneti](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-jupyter-sql-qyery-output.png "SQL-lekérdezés kimenete")

   További információ a hello `%%sql` magic, lásd: [hello támogatott paraméterek %% sql magic](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).
7. Ezután már használhatja a Matplotlib marad, a szalagtár tooconstruct képi megjelenítés adatainak, toocreate rajzot használja. Mert a rajzolási hello hello helyileg kell létrehozni a megőrzött **averagetime** dataframe, hello kódrészletet a következővel kell kezdődnie hello `%%local` magic. Ez biztosítja, hogy hello kód hello Jupyter kiszolgálón helyileg futnak.

       %%local
       %matplotlib inline
       import matplotlib.pyplot as plt

       plt.plot(averagetime['Minutes'], averagetime['Time'], marker='o', linestyle='--')
       plt.xlabel('Time (min)')
       plt.ylabel('Average time taken for request (ms)')

   Hello hasonló kimenetnek kell megjelennie:

   ![Matplotlib kimeneti](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-apache-spark-web-log-analysis-plot.png "Matplotlib kimeneti")
8. Miután befejezte a hello alkalmazást futtat, akkor leállítási hello notebook toorelease hello erőforrásokat. toodo Igen, a hello **fájl** hello notebook menüjében kattintson **zárja be és Halt**. Ezzel leállítja és Bezárás hello notebookot.

## <a name="seealso"></a>Lásd még:
* [Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Forgatókönyvek
* [Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel](hdinsight-apache-spark-use-bi-tools.md)
* [Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására](hdinsight-apache-spark-eventhub-streaming.md)

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
