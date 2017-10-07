---
title: "aaaUse Zeppelin notebookok az Apache Spark on Azure HDInsight fürt |} Microsoft Docs"
description: "Hogyan toouse Zeppelin notebookok az Apache Spark on Azure HDInsight clusters részletes ismertetése."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: df489d70-7788-4efa-a089-e5e5006421e2
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 3ab479cfccc7fd38a9bf6a9fb4f5928beec8ff7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-zeppelin-notebooks-with-apache-spark-cluster-on-azure-hdinsight"></a>Zeppelin notebookok használata Azure HDInsight az Apache Spark-fürt

HDInsight Spark-fürtjei Zeppelin notebookok használható toorun Spark feladatok közé tartozik. Ebből a cikkből megismerheti, hogyan toouse hello Zeppelin jegyzetfüzet a HDInsight-fürtöt.

> [!NOTE]
> Zeppelin notebookok csak 1.6.3 Spark on HDInsight 3.5-ös és 2.1.0 Spark on HDInsight 3.6 érhetők el.
>

**Előfeltételek:**

* Azure-előfizetés. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* A HDInsight az Apache Spark-fürt. Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="launch-a-zeppelin-notebook"></a>Indítsa el a Zeppelin notebook
1. Hello Spark-fürt panelén kattintson **fürt irányítópult**, és kattintson a **Zeppelin Notebook**. Ha a rendszer kéri, adja meg hello fürt hello rendszergazdai hitelesítő adataival.
   
   > [!NOTE]
   > A fürt URL-címet a böngészőben a következő megnyitásakor hello által is hello Zeppelin Notebook lehet elérni. Cserélje le **CLUSTERNAME** hello néven a fürt:
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`
   > 
   > 
2. Hozzon létre új notebookot. Hello fejléc ablaktáblában kattintson **Notebook**, és kattintson a **hozzon létre új megjegyzés**.
   
    ![Létrehozhat egy újat Zeppelin](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-create-zeppelin-notebook.png "egy új Zeppelin notebook létrehozása")
   
    Adjon meg egy nevet hello notebook, és kattintson **létrehozása Megjegyzés**.
3. Ellenőrizze azt is, hello notebook fejléc egy csatlakoztatott állapotát jeleníti meg. Hello jobb felső sarokban, zöld pontot helyén.
   
    ![Zeppelin notebook állapot](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-connected.png "Zeppelin notebook állapota")
4. Töltse be a mintaadatokat egy ideiglenes táblába. Spark-fürt hdinsightban történő hello mintaadatfájlokat, létrehozásakor **hvac.csv**, a másolt toohello kapcsolódó tárfiók **\HdiSamples\SensorSampleData\hvac**.
   
    Hello üres bekezdés, amely alapértelmezés szerint az új notebook hello jön létre illessze be a következő kódrészletet hello.
   
        %livy.spark
        //hello above magic instructs Zeppelin toouse hello Livy Scala interpreter
   
        // Create an RDD using hello default Spark context, sc
        val hvacText = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)
   
        // Map hello values in hello .csv file toohello schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0), 
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()
   
        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")
   
    Nyomja le az **SHIFT + ENTER** , vagy kattintson a hello **lejátszása** hello bekezdés toorun hello részlet gombra. hello hello bekezdés jobb-sarkában hello állapotának a kész, függőben lévő, FUTÓ tooFINISHED kell előrehaladás. hello hello alján megjelenik hello kimeneti egyazon paragraph. képernyőfelvétel a hello következőhöz hasonló hello:
   
    ![Hozzon létre egy ideiglenes táblából a nyers adatoktól](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-load-data.png "a nyers adatoktól ideiglenes tábla létrehozása")
   
    A cím tooeach bekezdés is megadhatja. A hello jobb sarkában, kattintson a hello **beállítások** ikonra, végül **cím megjelenítése a**.
5. Most futtathatja a Spark SQL-utasítások hello **hvac** tábla. Illessze be a következő lekérdezés egy új bekezdés hello. hello lekérdezés lekéri a hello épület azonosítója és hello különbség hello cél és a tényleges hőmérsékletek minden készítéséhez egy adott időpontban. Nyomja le az **SHIFT + ENTER**.
   
        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 
   
    Hello **% sql** hello elején utasítás hello notebook toouse hello Livy Scala parancsértelmező jelzi.
   
    hello alábbi képernyőfelvételen látható hello kimeneti.
   
    ![Hello notebook használata Spark SQL-utasítás futtatása](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-1.png "hello notebook használata Spark SQL-utasítás futtatása")
   
     Kattintson a hello megjelenítési beállítások (a kijelölt téglalap) tooswitch között különböző felelősséget a hello azonos kimenethez. Kattintson a **beállítások** toochoose milyen consitutes hello kulcs és hello kimeneti értékeit. képernyőfelvétel-készítés fent használ hello **buildingID** hello kulcs és hello átlaga **temp_diff** hello értékként.
6. Változók használata hello lekérdezés Spark SQL-utasítások is futtathatja. hello a következő kódrészletben látható szövegrészt hogyan toodefine egy változó **Temp**, hello lekérdezés hello a lehetséges értékek a tooquery kívánja. Hello lekérdezés első futtatásakor egy legördülő lista automatikusan töltődik hello változóhoz megadott hello értékek.
   
        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}" 
   
    Ezt a kódrészletet illessze be egy új bekezdés, és nyomja le az **SHIFT + ENTER**. hello alábbi képernyőfelvételen látható hello kimeneti.
   
    ![Hello notebook használata Spark SQL-utasítás futtatása](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-2.png "hello notebook használata Spark SQL-utasítás futtatása")
   
    A lekérdezések kiválaszthat egy új értéket a hello legördülő, és futtassa újra a hello lekérdezés. Kattintson a **beállítások** toochoose milyen consitutes hello kulcs és hello kimeneti értékeit. képernyőfelvétel-készítés fent használ hello **buildingID** hello kulcsként hello átlagos **temp_diff** hello értékként és **targettemp** hello csoportként.
7. Indítsa újra a hello Livy parancsértelmező tooexit hello alkalmazás. toodo tehát parancsértelmező beállításai megnyitásához hello bejelentkezett felhasználó nevét a hello jobb felső sarokban, és kattintson **parancsértelmező**.
   
    ![Indítsa el a parancsértelmező](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "kimeneti struktúra")
8. Görgessen tooLivy parancsértelmező beállításait, és kattintson a **indítsa újra a**.
   
    ![Indítsa újra a hello Livy intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "hello Zeppelin intepreter újraindítása")

## <a name="how-do-i-use-external-packages-with-hello-notebook"></a>Hogyan külső csomagok használata hello notebook?
Az Apache Spark-fürt a HDInsight (Linux) toouse külső, közösségi hozzájárult csomagok, amelyek nem szerepel az a-kész hello fürt hello Zeppelin notebook konfigurálhatja. Hello kereshet [Maven-tárház](http://search.maven.org/) hello csomagok rendelkezésre álló teljes listáját. Más forrásból is megkapható elérhető csomagok listáját. Például közösségi hozzájárult csomagok teljes listája megtalálható [Spark csomagok](http://spark-packages.org/).

Ebből a cikkből látni fogja hogyan toouse hello [spark-fürt megosztott kötetei szolgáltatás](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) hello Jupyter notebook számára.

1. Nyissa meg a parancsértelmező beállításait. Hello jobb felső sarokban, kattintson a hello jelentkezni a felhasználónevet, és kattintson a **parancsértelmező**.
   
    ![Indítsa el a parancsértelmező](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "kimeneti struktúra")
2. Görgessen tooLivy parancsértelmező beállításait, és kattintson a **szerkesztése**.
   
    ![Parancsértelmező beállításainak módosítása](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "parancsértelmező beállításainak módosítása")
3. Adja hozzá egy új kulcsot, úgynevezett **livy.spark.jars.packages** és az értékét állítsa hello formátumban `group:id:version`. Ha azt szeretné, hogy toouse hello [spark-fürt megosztott kötetei szolgáltatás](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) csomag, akkor értékre kell állítani hello hello kulcs túl`com.databricks:spark-csv_2.10:1.4.0`.
   
    ![Parancsértelmező beállításainak módosítása](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "parancsértelmező beállításainak módosítása")
   
    Kattintson a **mentése** , majd indítsa újra a hello Livy parancsértelmező.
4. **Tipp**: Ha azt szeretné, hogyan hello kulcs hello értékénél tooarrive fent, az alábbiakban megadott toounderstand módját.
   
    a. Keresse meg hello csomag hello Maven-tárházat. Ebben az oktatóanyagban használtuk [spark-fürt megosztott kötetei szolgáltatás](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
   
    b. Adattárból hello gyűjtse össze a hello értékeinek **GroupId**, **artifactid szakaszát**, és **verzió**.
   
    ![Külső csomagok használata Jupyter notebook](./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "külső csomagok használata Jupyter notebook")
   
    c. Összefűzésére hello három értékek, egymástól kettősponttal (**:**).
   
        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-hello-zeppelin-notebooks-saved"></a>Zeppelin notebookok mentett hol vannak a hello?
hello Zeppelin notebookok toohello fürt headnodes kerülnek mentésre. Így hello fürt törlésekor, hello notebookok is törlődik. Ha azt szeretné, toopreserve jegyzetfüzetek más fürtökön későbbi használatra, exportálnia kell őket futó hello feladatok befejezése után. a notebook tooexport kattintson hello **exportálása** ikon az alábbi hello ábrán látható módon.

![Töltse le a notebook](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "letöltési hello notebook")

Ez hello notebook egy JSON-fájl a letöltési helyre menti.

## <a name="livy-session-management"></a>Livy munkamenet-kezelés
A Zeppelin jegyzetfüzet hello első kód bekezdés futtatásakor a új Livy munkamenet létrehozása a HDInsight Spark-fürtön. A munkamenet által megosztott minden Zeppelin notebookok ezt követően hozzon létre. Ha valamilyen okból hello a Livy munkamenet leállítása (fürt újraindítás, stb.), nem fogja tudni toorun feladatokat azok hello Zeppelin notebook.

Ebben az esetben hello Zeppelin jegyzetfüzet a futó feladatok megkezdése előtt a következő lépéseket kell elvégeznie. 

1. Indítsa újra a hello Zeppelin notebook Livy parancsértelmező hello. toodo tehát parancsértelmező beállításai megnyitásához hello bejelentkezett felhasználó nevét a hello jobb felső sarokban, és kattintson **parancsértelmező**.
   
    ![Indítsa el a parancsértelmező](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "kimeneti struktúra")
2. Görgessen tooLivy parancsértelmező beállításait, és kattintson a **indítsa újra a**.
   
    ![Indítsa újra a hello Livy intepreter](./media/hdinsight-apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "hello Zeppelin intepreter újraindítása")
3. Futtassa a kódcella meglévő Zeppelin jegyzetfüzet. Ez létrehoz egy új Livy munkamenetet hello HDInsight-fürt.

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
* [Toocreate IntelliJ IDEA HDInsight-eszközei beépülő használja, és küldje el a Spark Scala applicatons](hdinsight-apache-spark-intellij-tool-plugin.md)
* [IntelliJ IDEA toodebug Spark-alkalmazások HDInsight-eszközei beépülő távolról használni](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Külső csomagok használata Jupyter notebookokkal](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Erőforrások kezelése
* [Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése](hdinsight-apache-spark-resource-manager.md)
* [Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md 







