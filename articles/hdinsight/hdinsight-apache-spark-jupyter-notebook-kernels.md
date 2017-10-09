---
title: aaaKernels Jupyter notebookokhoz a Spark on Azure hdinsight clusters |} Microsoft Docs
description: "További információk a Jupyter notebookokhoz elérhető Azure hdinsight Spark-fürtjei PySpark PySpark3 és Spark mag hello."
keywords: a spark, jupyter spark jupyter notebook
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 0719e503-ee6d-41ac-b37e-3d77db8b121b
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: nitinme
ms.openlocfilehash: 560c944fe850c5753ac9fa90550b804f0c47d14c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="kernels-for-jupyter-notebook-on-spark-clusters-in-azure-hdinsight"></a>Az Azure hdinsight Spark-fürtjei Jupyter notebookokhoz kernelek 

HDInsight Spark-fürtjei használata hello Jupyter notebook a Spark on az alkalmazások teszteléséhez kernelt biztosítanak. A rendszermag egy olyan program, fut, és a kód értelmezi. hello három kernelek a következők:

- **PySpark** – a Python2 írt alkalmazások esetén
- **PySpark3** – a Python3 írt alkalmazások esetén
- **Spark** - scalában írt alkalmazások esetén

Ebből a cikkből megismerheti, hogyan toouse ezek kernelek és a használatuk hello előnyeit.

## <a name="prerequisites"></a>Előfeltételek

* Apache Spark-fürt hdinsightban. Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="create-a-jupyter-notebook-on-spark-hdinsight"></a>A Spark on HDInsight Jupyter notebook létrehozása

1. A hello [Azure-portálon](https://portal.azure.com/), nyissa meg a fürt.  Lásd: [listája és megjelenítése fürtök](hdinsight-administer-use-portal-linux.md#list-and-show-clusters) hello utasításokat. egy új portálpanelen hello fürt nyílik meg.

2. A hello **Gyorshivatkozások** területén kattintson **irányítópultok fürt** tooopen hello **irányítópultok fürt** panelen.  Ha nem lát **Gyorshivatkozások**, kattintson a **áttekintése** hello panelen hello bal oldali menüből.

    ![A Spark Jupyter notebook](./media/hdinsight-apache-spark-jupyter-notebook-kernels/hdinsight-jupyter-notebook-on-spark.png "Spark a Jupyter notebook") 

3. Kattintson a **Jupyter Notebook**. Ha a rendszer kéri, adja meg hello fürt hello rendszergazdai hitelesítő adataival.
   
   > [!NOTE]
   > Előfordulhat, hogy is elérni a Spark-fürt URL-címet a böngészőben a következő megnyitásakor hello hello Jupyter notebook. Cserélje le **CLUSTERNAME** hello néven a fürt:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   > 
   > 

3. Kattintson a **új**, majd a **Pyspark**, **PySpark3**, vagy **Spark** toocreate jegyzetfüzet. Használja a hello Spark kernel Scala-alkalmazások, a PySpark kernel Python2 alkalmazások és a PySpark3 kernel Python3 alkalmazásokhoz.
   
    ![Jupyter notebookokhoz a Spark mag](./media/hdinsight-apache-spark-jupyter-notebook-kernels/kernel-jupyter-notebook-on-spark.png "Jupyter notebookokhoz a Spark mag") 

4. A notebook kiválasztott hello kernel nyílik meg.

## <a name="benefits-of-using-hello-kernels"></a>Hello kernelek használatának előnyei

Az alábbiakban néhány használatának előnyei egy hello új kernelek a Jupyter notebook a Spark HDInsight-fürtökön.

- **Az adott néven beállítás környezetek**. A **PySpark**, **PySpark3**, vagy hello **Spark** mag, nem kell tooset hello Spark- vagy Hive-környezeteket explicit módon az alkalmazások használatának megkezdése előtt. Ezek a alapértelmezés szerint elérhető. Ezek a környezetek a következők:
   
   * **sc** - Spark-környezet
   * **az sqlContext** - struktúra környezet

    Igen nincs toorun utasítások, mint például a következő tooset hello környezetek hello:

        sc SparkContext('yarn-client') az sqlContext = = HiveContext(sc)

    Ehelyett közvetlenül használható hello beállított környezetek az alkalmazásban.

- **A cella magics**. hello PySpark kernel tartalmaz néhány előre definiált "magics", amelyeket speciális meghívhatja a parancsok `%%` (például `%%MAGIC` <args>). hello magic parancs hello első szótól kód cella legyen, és lehetővé teszik a tartalom több sornyi. hello magic word hello első szótól hello cellában kell lennie. Hozzáadás semmit hello magic, még akkor is, a megjegyzéseket, mielőtt hibát okoz.     A magics további információkért lásd: [Itt](http://ipython.readthedocs.org/en/stable/interactive/magics.html).
   
    hello következő táblázat különböző magics hello hello kernelek keresztül érhető el.

   | Varázsszám | Példa | Leírás |
   | --- | --- | --- |
   | segítség |`%%help` |Létrehoz egy táblát az összes hello elérhető magics példa és leírása |
   | információ |`%%info` |Munkamenet-információk kimenetek hello aktuális Livy végpont |
   | Konfigurálása |`%%configure -f`<br>`{"executorMemory": "1000M"`,<br>`"executorCores": 4`} |Konfigurálja a hello paramétereit a munkamenet létrehozásához. hello force jelzést (-f) megadása kötelező, ha már létrehozott egy munkamenetet, amely biztosítja, hogy hello munkamenet eldobott és ismételt létrehozása megtörtént. Nézze meg [Livy a FELADÁS egy vagy több /sessions kérelem törzse](https://github.com/cloudera/livy#request-body) érvényes paraméterek listáját. Paraméterek a JSON karakterláncként kell átadnia, és későbbinek kell lennie hello következő sorban hello magic hello példa oszlopban látható. |
   | SQL |`%%sql -o <variable name>`<br> `SHOW TABLES` |Végrehajtja a Hive-lekérdezések hello az sqlContext ellen. Ha hello `-o` paramétert, a hello megőrződjenek hello hello lekérdezés eredménye %% helyi Python-környezetben, egy [Pandas](http://pandas.pydata.org/) dataframe. |
   | helyi |`%%local`<br>`a=1` |Az egymás utáni sorok összes hello kód végrehajtása helyileg. Kód hello kernel módjától függetlenül is érvényes Python2 kódot kell lennie. Igen, akkor is, ha a kiválasztott **PySpark3** vagy **Spark** kernelek hello notebook létrehozásakor, ha hello `%%local` magic cellába, ezt a cellát csak rendelkeznie kell érvényes Python2 kódot... |
   | naplók |`%%logs` |Kimenetek hello aktuális Livy munkamenet hello naplókat. |
   | törlése |`%%delete -f -s <session number>` |Egy adott munkamenet hello aktuális Livy végpont törlése. Vegye figyelembe, hogy hello munkamenet indításának hello kernel maga nem törölhető. |
   | Tisztítás |`%%cleanup -f` |Törli az összes hello munkamenet hello aktuális Livy végpont, beleértve a jegyzetfüzet munkamenet. hello kényszerített jelző -f megadása kötelező. |

   > [!NOTE]
   > Ezenkívül toohello magics által hozzáadott hello PySpark kernel, használhatja a hello [beépített IPython magics](https://ipython.org/ipython-doc/3/interactive/magics.html#cell-magics), többek között a következőket `%%sh`. Használhatja a hello `%%sh` magic toorun parancsfájlok és a fürt headnode hello kódblokkot.
   >
   >
2. **A képi megjelenítés automatikus**. Hello **Pyspark** kernel automatikusan visualizes hello kimeneti struktúra és az SQL-lekérdezéseket. Számos különféle típusú képi megjelenítések, beleértve a tábla, torta, vonal, terület, sáv választhat.

## <a name="parameters-supported-with-hello-sql-magic"></a>Hello támogatott paraméterek %% sql varázsszám
Hello `%%sql` magic támogatja a különböző paraméterek közül választhat, hogy lekérdezések futtatásakor kapó kimeneti toocontrol hello típusú. a következő táblázat hello hello kimeneti sorolja fel.

| Paraméter | Példa | Leírás |
| --- | --- | --- |
| -o |`-o <VARIABLE NAME>` |Használja a hello lekérdezés paraméter toopersist hello eredménye hello %% helyi Python-környezetben, mint egy [Pandas](http://pandas.pydata.org/) dataframe. hello hello dataframe változó értéke hello változó nevét, adja meg. |
| -k |`-q` |A képi megjelenítések ki tooturn hello cella használja. Ha nem szeretné, hogy tooauto-megjelenítheti hello egy cella tartalmát, és csak szeretné, hogy toocapture azt egy dataframe, majd használja `-q -o <VARIABLE>`. Ha azt szeretné, ki képi megjelenítések tooturn hello eredmények rögzítése nélkül (, például egy SQL-lekérdezést, például fut egy `CREATE TABLE` utasítás), használjon `-q` megadása nélkül egy `-o` argumentum. |
| -m |`-m <METHOD>` |Ha **METÓDUS** vagy **érvénybe** vagy **minta** (alapértelmezett érték a **érvénybe**). Ha hello módszer **igénybe**, hello kernel szerzi a hello felső hello adatok eredményhalmaz MAXROWS (később a táblázatban ismertetett) által meghatározott elemek. Ha hello módszer **minta**, hello kernel véletlenszerűen minták szerint túl hello adatkészlet elemeinek`-r` paramétert, a következő táblázatban leírt. |
| -r |`-r <FRACTION>` |Itt **mért** 0,0 és 1,0 közötti lebegőpontos szám. Ha hello minta hello SQL-lekérdezés módja `sample`, majd hello kernel véletlenszerűen minták hello hello elemeinek hello eredménykészlet, hogy a megadott mekkora részét. Például, ha futtatja az SQL-lekérdezést hello argumentumokkal `-m sample -r 0.01`, majd véletlenszerűen lekérdező hello sorok 1 %-át. |
| -n |`-n <MAXROWS>` |**MAXROWS** egész érték. hello kernel korlátozza hello kimeneti sorok száma túl**MAXROWS**. Ha **MAXROWS** például van egy negatív szám **-1**, majd hello hello eredménykészlet sorainak száma nincs korlátozva. |

**Példa**

    %%sql -q -m sample -r 0.1 -n 500 -o query2
    SELECT * FROM hivesampletable

a fenti hello utasítás hello a következő:

* Kiválasztja az összes rekordot **hivesampletable**.
* Használjuk a - q, mert automatikus-képi megjelenítés kikapcsolása.
* Mivel használjuk `-m sample -r 0.1 -n 500` véletlenszerűen – minták 10 % hello hivesampletable hello sorok és korlátai hello hello beállítása too500 sorok méretét.
* Végül mert használtuk `-o query2` is hello kimeneti menti azokat a dataframe nevű **lekérdezés2**.

## <a name="considerations-while-using-hello-new-kernels"></a>Új kernelek hello használata során kapcsolatos szempontok

Használja, amelyik kernel hello fürterőforrások futtató hello notebookok elhagyása igényel.  Ezek kernelek hello környezetek előre van állítva, mert egyszerűen Kilépés hello notebookok nem kill hello környezetben, és ezért hello fürterőforrások továbbra is toobe használja. Bevált gyakorlat az toouse hello **zárja be és Halt** hello notebook kapcsolót **fájl** használhatatlanná teszi hello környezetben hello notebook használatának befejezése után, és majd kilépés hello notebook menü.     

## <a name="show-me-some-examples"></a>Néhány példa megjelenítése

Jupyter notebook megnyitásakor látni hello gyökérszinten elérhető két mappát.

* Hello **PySpark** mappa rendelkezik minta notebookok adott használata hello új **Python** kernel.
* Hello **Scala** mappa rendelkezik minta notebookok adott használata hello új **Spark** kernel.

Megnyithatja a hello **Spark Magic 00 - [OLVASHATÓ első] Kernel szolgáltatások** hello a notebook **PySpark** vagy **Spark** mappa toolearn kapcsolatos hello különböző magics érhető el. Is használhatja más minta notebookok hello két mappák toolearn alapján hogyan hello tooachieve Jupyter notebookok használata a HDInsight Spark-fürtjei különböző helyzetekben.

## <a name="where-are-hello-notebooks-stored"></a>Hello notebookok tároló?

Jupyter notebookok menti a hello hello-fürthöz tartozó toohello tárfiók **/HdiNotebooks** mappa.  Notebookok, szöveges fájlt és mappát hoz létre a Jupyter belül hello tárfiókból érhetők el.  Például, ha a Jupyter toocreate mappa használata **SajátMappa** egy hordozható **myfolder/mynotebook.ipynb**, érheti el, hogy a notebook `/HdiNotebooks/myfolder/mynotebook.ipynb` hello tárfiókon belül.  fordított hello akkor is igaz értéke esetén ez azt jelenti, ha közvetlen tooyour tárolási fiókot a töltse fel a notebook `/HdiNotebooks/mynotebook1.ipynb`, valamint Jupyterről származó látható hello notebook.  Notebookok maradni hello tárfiók hello fürtök törlése után is.

hello notebookok mentett toohello tárfiók módja kompatibilis a HDFS. Így, ha az SSH-ból is használhat hello fürt fájl parancsok ahogy az alábbi részlet hello:

    hdfs dfs -ls /HdiNotebooks                               # List everything at hello root directory – everything in this directory is visible tooJupyter from hello home page
    hdfs dfs –copyToLocal /HdiNotebooks                    # Download hello contents of hello HdiNotebooks folder
    hdfs dfs –copyFromLocal example.ipynb /HdiNotebooks   # Upload a notebook example.ipynb toohello root folder so it’s visible from Jupyter


Abban az esetben hello fürt hello tárfiók elérése problémák vannak, hello notebookok is tárolja hello headnode `/var/lib/jupyter`.

## <a name="supported-browser"></a>Támogatott böngésző

A Spark HDInsight-fürtökön Jupyter notebookok csak Google Chrome támogatottak.

## <a name="feedback"></a>Visszajelzés
hello új kernelek szakasz fejlődnek, és adott idő alatt számos lesz. Ez is jelentheti, hogy API-k módosulhatnak, mivel ezek kernelek számos. Köszönjük volna olyan visszajelzést, hogy rendelkezik, ezek új kernelek használata során. Ez akkor hasznos, hello kiadásban ezek kernelek kialakításában. A megjegyzések/visszajelzések alapján hello hagyhatja **megjegyzések** szakasz ebben a cikkben hello alján.

## <a name="seealso"></a>Lásd még:
* [Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Forgatókönyvek
* [Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel](hdinsight-apache-spark-use-bi-tools.md)
* [Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására](hdinsight-apache-spark-eventhub-streaming.md)
* [A webhelynapló elemzése a Spark on HDInsight használatával](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Alkalmazások létrehozása és futtatása
* [Önálló alkalmazás létrehozása a Scala használatával](hdinsight-apache-spark-create-standalone-application.md)
* [Feladatok távoli futtatása Spark-fürtön a Livy használatával](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Eszközök és bővítmények
* [Toocreate IntelliJ IDEA HDInsight-eszközei beépülő használja, és küldje el a Spark Scala-alkalmazások](hdinsight-apache-spark-intellij-tool-plugin.md)
* [IntelliJ IDEA toodebug Spark-alkalmazások HDInsight-eszközei beépülő távolról használni](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Zeppelin notebookok használata Spark-fürttel HDInsighton](hdinsight-apache-spark-zeppelin-notebook.md)
* [Külső csomagok használata Jupyter notebookokkal](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Erőforrások kezelése
* [Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése](hdinsight-apache-spark-resource-manager.md)
* [Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban](hdinsight-apache-spark-job-debugging.md)
