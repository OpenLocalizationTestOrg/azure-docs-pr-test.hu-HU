---
title: "aaaManage erőforrásokat az Apache Spark on Azure HDInsight fürt |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse a jobb teljesítmény érdekében az Azure HDInsight Spark-fürtjei erőforrásainak kezelése."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9da7d4e3-458e-4296-a628-77b14643f7e4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: e18682a24f77494db884105f9db03c0a350ddad6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-resources-for-apache-spark-cluster-on-azure-hdinsight"></a>Az Azure HDInsight az Apache Spark-fürt erőforrásainak kezelése 

Ebben a cikkben megtudhatja, hogyan tooaccess hello felületek, például a Ambari felhasználói felület, a YARN felhasználói felületen és a Spark előzmények Server hello a Spark-fürthöz kapcsolódó. Azt is megtudhatja, hogyan tootune hello fürtkonfiguráció az optimális teljesítmény kapcsolatos.

**Előfeltételek:**

Hello következő kell rendelkeznie:

* Azure-előfizetés. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* A HDInsight az Apache Spark-fürt. Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="how-do-i-launch-hello-ambari-web-ui"></a>Hogyan indítsa el az Ambari webes felhasználói felületén hello?
1. A hello [Azure Portal](https://portal.azure.com/), hello kezdőpulton, kattintson a Spark-fürt hello csempére (ha toohello kezdőpulton rögzítette azt). Tooyour fürt alapján is megtalálhatja **összes tallózása** > **a HDInsight-fürtök**.
2. Hello Spark-fürt panelén kattintson **irányítópult**. Amikor a rendszer kéri, adja meg hello Spark-fürt hello rendszergazdai hitelesítő adataival.

    ![Indítsa el az Ambari](./media/hdinsight-apache-spark-resource-manager/hdinsight-launch-cluster-dashboard.png "erőforrás-kezelő indítása")
3. Ez kell hello Ambari webes felhasználói felületén, nyissa meg alább látható módon.

    ![Ambari webes felhasználói felület](./media/hdinsight-apache-spark-resource-manager/ambari-web-ui.png "Ambari webes felhasználói felület")   

## <a name="how-do-i-launch-hello-spark-history-server"></a>Hogyan indítsa el a Spark előzmények Server hello?
1. A hello [Azure Portal](https://portal.azure.com/), hello kezdőpulton, kattintson a Spark-fürt hello csempére (ha toohello kezdőpulton rögzítette azt).
2. Hello a fürt paneljén a **Gyorshivatkozások**, kattintson a **fürt irányítópult**. A hello **fürt irányítópult** panelen kattintson a **Spark előzmények Server**.

    ![Spark előzmények Server](./media/hdinsight-apache-spark-resource-manager/launch-history-server.png "Spark előzmények kiszolgáló")

    Amikor a rendszer kéri, adja meg hello Spark-fürt hello rendszergazdai hitelesítő adataival.

## <a name="how-do-i-launch-hello-yarn-ui"></a>Hogyan indítsa el a Yarn felhasználói felületen hello?
Hello YARN felhasználói felületen toomonitor alkalmazások hello Spark-fürt a jelenleg futó is használhatja.

1. Hello-fürt panelén kattintson **fürt irányítópult**, és kattintson a **YARN**.

    ![Indítsa el a YARN felhasználói felületen](./media/hdinsight-apache-spark-resource-manager/launch-yarn-ui.png)

   > [!TIP]
   > Másik lehetőségként is elindíthatja a hello YARN felhasználói felületen a hello Ambari felhasználói felület. toolaunch hello Ambari felhasználói felületén, a hello-fürt panelén kattintson **fürt irányítópult**, és kattintson a **HDInsight fürt irányítópult**. A hello Ambari felhasználói felület, kattintson az **YARN**, kattintson a **Gyorshivatkozások**, kattintson hello aktív erőforrás-kezelő, majd **erőforrás-kezelő felhasználói felületén**.
   >
   >

## <a name="what-is-hello-optimum-cluster-configuration-toorun-spark-applications"></a>Mi az a hello optimális fürt konfigurációs toorun Spark-alkalmazások?
hello három fő, amely nem használható alkalmazás követelményeitől függően Spark konfigurációs paraméterei `spark.executor.instances`, `spark.executor.cores`, és `spark.executor.memory`. Egy művelettípus végrehajtója az a folyamat egy Spark-alkalmazáshoz elindítva. Hello munkavégző csomóponton fut, és hello feladatokat hello alkalmazás felelős toocarry. hello alapértelmezett száma végrehajtója és az egyes fürtökön hello végrehajtó mérete alapján van kiszámítva munkavégző csomópontokhoz és hello munkavégző csomópont méretének hello száma. Ezek tárolják `spark-defaults.conf` hello központi fürtcsomóponton.

hello három konfigurációs paraméterek konfigurálható szintjén hello fürt (hello fürtön futó összes alkalmazást), vagy minden egyes alkalmazáshoz adható meg.

### <a name="change-hello-parameters-using-ambari-ui"></a>Ambari felületen hello paraméterek módosítása
1. Hello Ambari felhasználói felületén kattintson **Spark**, kattintson a **Configs**, majd bontsa ki a **egyéni spark-alapértelmezett**.

    ![A megadott paraméterek Ambari használatával](./media/hdinsight-apache-spark-resource-manager/set-parameters-using-ambari.png)
2. hello alapértelmezett értékei jó toohave 4 Spark-alkalmazások hello fürt egyidejű futtatását. Is módosítások ezeket az értékeket hello felhasználói felület, a lent látható módon.

    ![A megadott paraméterek Ambari használatával](./media/hdinsight-apache-spark-resource-manager/set-executor-parameters.png)
3. Kattintson a **mentése** toosave hello konfigurációs módosításokat. Hello hello oldal tetején, kérni fogja az összes hello toorestart érintett szolgáltatások. Kattintson a **indítsa újra a**.

    ![Szolgáltatások újraindítása](./media/hdinsight-apache-spark-resource-manager/restart-services.png)

### <a name="change-hello-parameters-for-an-application-running-in-jupyter-notebook"></a>Jupyter notebook alkalmazás hello paramétereinek módosítása
Az alkalmazások hello Jupyter notebook, hello használhatja `%%configure` magic toomake hello konfigurációs módosításokat. Ideális esetben meg kell nyitnia a változások hello alkalmazást, mielőtt újra lefuttatja az első kódcella hello elején. Ez biztosítja, hogy a hello konfigurálása alkalmazott toohello Livy munkamenet, amikor lekérdezi a létrehozott. Ha azt szeretné, hogy toochange hello konfigurációs hello alkalmazásban egy későbbi időpontban, használnia kell a hello `-f` paraméter. Azonban így minden hello az előrehaladás végrehajtásával alkalmazás elvesznek.

az alábbi hello kódrészletben láthatja, hogyan toochange hello Jupyter egy alkalmazáskészlet konfigurációját.

    %%configure
    {"executorMemory": "3072M", "executorCores": 4, "numExecutors":10}

Konfigurációs paraméterek a JSON karakterláncként kell átadnia, és későbbinek kell lennie hello következő sorban hello magic hello példa oszlopban látható.

### <a name="change-hello-parameters-for-an-application-submitted-using-spark-submit"></a>Változás hello paramétereinek használatával kérelem spark-elküldése
A következő parancs példája hogyan toochange hello konfigurációs paraméterek használatával küldött kötegelt alkalmazások `spark-submit`.

    spark-submit --class <hello application class tooexecute> --executor-memory 3072M --executor-cores 4 –-num-executors 10 <location of application jar file> <application parameters>

### <a name="change-hello-parameters-for-an-application-submitted-using-curl"></a>Hello használata cURL használatával kérelem paramétereinek módosítása
A következő parancs példája hogyan toochange hello konfigurációs paraméterek használata cURL használatával küldött kötegelt alkalmazáshoz.

    curl -k -v -H 'Content-Type: application/json' -X POST -d '{"file":"<location of application jar file>", "className":"<hello application class tooexecute>", "args":[<application parameters>], "numExecutors":10, "executorMemory":"2G", "executorCores":5' localhost:8998/batches

### <a name="how-do-i-change-these-parameters-on-a-spark-thrift-server"></a>Hogyan változtathatom meg ezeket a paramétereket a Spark Thrift-kiszolgáló?
A Spark Thrift-kiszolgáló JDBC-/ ODBC hozzáférés tooa Spark-fürt és használt tooservice Spark SQL-lekérdezések. Eszközök, például a Power BI-ban Tableau stb. ODBC protokoll toocommunicate használata Spark Thrift-kiszolgáló tooexecute Spark SQL-lekérdezések a Spark-alkalmazásként. Spark-fürt létrehozásakor hello Spark Thrift-kiszolgáló indulnak el, egy minden átjárócsomópont két példánya. Minden egyes Spark Thrift-kiszolgáló látható hello YARN felhasználói felületen a Spark-alkalmazásként.

A Spark Thrift-kiszolgáló által használt dinamikus végrehajtó foglalási Spark, és ezért hello `spark.executor.instances` nem használatos. Ehelyett használja a Spark Thrift-kiszolgáló `spark.dynamicAllocation.minExecutors` és `spark.dynamicAllocation.maxExecutors` toospecify hello végrehajtó száma. konfigurációs paraméterek hello `spark.executor.cores` és `spark.executor.memory` van használt toomodify hello végrehajtó méretét. Ezek a paraméterek módosíthatja a lent látható módon.

* Bontsa ki a hello **spark-thrift-sparkconf speciális** kategória tooupdate hello paraméterek `spark.dynamicAllocation.minExecutors`, `spark.dynamicAllocation.maxExecutors`, és `spark.executor.memory`.

    ![A Spark thrift-kiszolgáló konfigurálása](./media/hdinsight-apache-spark-resource-manager/spark-thrift-server-1.png)    
* Bontsa ki a hello **spark-thrift-sparkconf egyéni** kategória tooupdate hello paraméter `spark.executor.cores`.

    ![A Spark thrift-kiszolgáló konfigurálása](./media/hdinsight-apache-spark-resource-manager/spark-thrift-server-2.png)

### <a name="how-do-i-change-hello-driver-memory-of-hello-spark-thrift-server"></a>Hogyan változtathatom meg a Spark Thrift-kiszolgáló hello hello illesztőprogram memória?
A Spark Thrift-kiszolgáló illesztőprogram memóriát hello teljes RAM hello átjárócsomópont mérete 14GB-nál nagyobb hello átjárócsomópont RAM mérete, a konfigurált too25 % kerül. Hello Ambari felhasználói felület toochange hello illesztőprogram memóriakövetelménye, használhatja a lent látható módon.

* Hello Ambari felhasználói felületén kattintson **Spark**, kattintson **Configs**, bontsa ki **spark-env speciális**, és adja meg a hello érték **spark_thrift_cmd_opts**.

    ![A Spark thrift-kiszolgáló RAM konfigurálása](./media/hdinsight-apache-spark-resource-manager/spark-thrift-server-ram.png)

## <a name="i-do-not-use-bi-with-spark-cluster-how-do-i-take-hello-resources-back"></a>BI nem Spark-fürt használata. Hogyan tudom vissza igénybe hello erőforrásokat?
Spark dinamikus foglalási használjuk, mivel hello csak thrift-kiszolgáló által felhasznált erőforrások hello két alkalmazás főkiszolgálók hello erőforrásait. Ezeket az erőforrásokat le kell állítania hello hello fürt Thrift-kiszolgáló szolgáltatás tooreclaim.

1. Hello Ambari UI hello bal oldali ablaktáblában kattintson **Spark**.
2. A következő lapon hello kattintson **Spark Thrift kiszolgálók**.

    ![Indítsa újra a thrift-kiszolgáló](./media/hdinsight-apache-spark-resource-manager/restart-thrift-server-1.png)
3. Hello két headnodes mely hello Spark Thrift-kiszolgáló fut. kell megjelennie. Kattintson az egyik hello headnodes.

    ![Indítsa újra a thrift-kiszolgáló](./media/hdinsight-apache-spark-resource-manager/restart-thrift-server-2.png)
4. hello következő lap felsorolja az adott headnode futó összes hello szolgáltatást. Hello listából kattintson hello legördülő gomb következő tooSpark Thrift-kiszolgáló, majd **leállítása**.

    ![Indítsa újra a thrift-kiszolgáló](./media/hdinsight-apache-spark-resource-manager/restart-thrift-server-3.png)
5. Ismételje meg ezeket a lépéseket a hello más headnode is.

## <a name="my-jupyter-notebooks-are-not-running-as-expected-how-can-i-restart-hello-service"></a>A Jupyter notebookok nem elvárt módon futnak. Hogyan újraindíthatja hello szolgáltatást?
Indítsa el a hello Ambari webes felhasználói felületén, ahogy fent látható. Hello bal oldali navigációs ablaktáblán kattintson **Jupyter**, kattintson a **szolgáltatás műveletek**, és kattintson a **indítsa újra az összes**. Hello Jupyter szolgáltatás elindítja az összes hello headnodes.

    ![Restart Jupyter](./media/hdinsight-apache-spark-resource-manager/restart-jupyter.png "Restart Jupyter")

## <a name="how-do-i-know-if-i-am-running-out-of-resources"></a>Hogyan állapítható meg, hogy ha erőforrások fut-e?
Indítsa el a hello Yarn felhasználói felületen, a fentiek szerint. Fürt metrikáinak tábla fölött üdvözlő képernyőt, ellenőrizze az értékeket **használt memória** és **memória teljes** oszlopok. Hello 2 értékei rendkívül szoros, ha a nem feltétlenül elegendő erőforrást toostart hello tovább alkalmazás. hello Ugyanez vonatkozik toohello **VCores használt** és **VCores összesen** oszlopok. Is, hello fő nézetben, ha egy alkalmazás tartózkodott a **elfogadott** állapotát, és nem változik a **futtató** sem **sikertelen** állapotba kerül, ez arra utal, hogy is lehet hogy nem elég erőforrások toostart kap.

    ![Resource Limit](./media/hdinsight-apache-spark-resource-manager/resource-limit.png "Resource Limit")

## <a name="how-do-i-kill-a-running-application-toofree-up-resource"></a>Hogyan kill futó alkalmazás toofree erőforrást?
1. A Yarn felhasználói felületen, hello hello bal oldali panelen, kattintson **futtató**. A futó alkalmazások hello listában határozza meg a hello alkalmazás toobe leállítása, majd kattintson a hello **azonosító**.

    ![Az App1 Kill](./media/hdinsight-apache-spark-resource-manager/kill-app1.png "App1 leállítása")

2. Kattintson a **Kill alkalmazás** hello jobb felső sarokban, majd kattintson **OK**.

    ![Kill App2](./media/hdinsight-apache-spark-resource-manager/kill-app2.png "App2 leállítása")

## <a name="see-also"></a>Lásd még:
* [Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban](hdinsight-apache-spark-job-debugging.md)

### <a name="for-data-analysts"></a>Az adatok elemző

* [Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [A webhelynapló elemzése a Spark on HDInsight használatával](hdinsight-apache-spark-custom-library-website-log-analysis.md)
* [Az Application Insights telemetriai adatainak elemzése a Spark on HDInsight használatával](hdinsight-spark-analyze-application-insight-logs.md)
* [Az Azure HDInsight Spark Caffe elosztott mély tanulási használata](hdinsight-deep-learning-caffe-spark.md)

### <a name="for-spark-developers"></a>A Spark-fejlesztőknek

* [Önálló alkalmazás létrehozása a Scala használatával](hdinsight-apache-spark-create-standalone-application.md)
* [Feladatok távoli futtatása Spark-fürtön a Livy használatával](hdinsight-apache-spark-livy-rest-interface.md)
* [Toocreate IntelliJ IDEA HDInsight-eszközei beépülő használja, és küldje el a Spark Scala-alkalmazások](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására](hdinsight-apache-spark-eventhub-streaming.md)
* [IntelliJ IDEA toodebug Spark-alkalmazások HDInsight-eszközei beépülő távolról használni](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Zeppelin notebookok használata Spark-fürttel HDInsighton](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Külső csomagok használata Jupyter notebookokkal](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan](hdinsight-apache-spark-jupyter-notebook-install-locally.md)
