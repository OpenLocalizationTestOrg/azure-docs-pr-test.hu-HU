---
title: "Az IntelliJ Azure eszköztára: Debug Spark alkalmazások távolról SSH |} Microsoft Docs"
description: "Részletes útmutatás hogyan toouse Azure eszközkészlet a HDInsight Tools IntelliJ toodebug alkalmazások távolról a HDInsight-fürtök SSH-n keresztül"
keywords: "Debug távolról intellij, távoli hibakeresés intellij, ssh, az intellij hdinsight, a debug intellij, hibakeresés"
services: hdinsight
documentationcenter: 
author: jejiang
manager: DJ
editor: Jenny Jiang
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 08/24/2017
ms.author: Jenny Jiang
ms.openlocfilehash: bf3ab9d04c2ff9fcb6bbbdeefb11f55a12fbd845
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debug-spark-applications-on-an-hdinsight-cluster-with-azure-toolkit-for-intellij-through-ssh"></a>Az SSH-n keresztül az IntelliJ HDInsight-fürtök az Azure-eszközkészlet a Spark-alkalmazások

Ez a cikk részletes útmutatást hogyan toouse Azure eszközkészlet a HDInsight Tools for IntelliJ toodebug applications távolról a HDInsight-fürtök. toodebug a projekt is megtekintheti a hello [az IntelliJ Debug HDInsight Spark Azure eszközkészlet alkalmazások](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ) videó.

**Előfeltételek**

* **A HDInsight Tools az intellij-t Azure eszköztára**. Ez az eszköz az intellij-t Azure Toolkit részét képezi. További információkért lásd: [Azure eszközkészlet telepítése az IntelliJ](https://docs.microsoft.com/en-us/azure/azure-toolkit-for-intellij-installation).
* **Az IntelliJ Azure eszköztára**. Az eszközkészlet toocreate Spark-alkalmazások használata a HDInsight-fürtök. További információkért kövesse a hello utasításait [IntelliJ toocreate Spark-alkalmazások HDInsight-fürtök használata Azure eszköztára](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-plugin).
* **HDInsight SSH szolgáltatást a felhasználónév és jelszó felügyeleti**. További információkért lásd: [tooHDInsight (Hadoop) csatlakozzon az ssh protokoll használatával](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) és [az SSH tunneling tooaccess Ambari webes felhasználói felület, JobHistory, NameNode, Oozie és egyéb web UI](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-linux-ambari-ssh-tunnel). 
 

## <a name="create-a-spark-scala-application-and-configure-it-for-remote-debugging"></a>Spark Scala-alkalmazás létrehozása, és konfigurálja a távoli hibakeresés

1. Indítsa el az IntelliJ IDEA, és ezután a projekt létrehozásához. A hello **új projekt** párbeszédpanel mezőbe hello a következő:

   a. Válassza ki **HDInsight**. 

   b. A Sablonválasztás Java vagy Scala a beállítások alapján. Hello alábbi beállítások közül választhat:

      - **A Spark on HDInsight (Scala)**

      - **A Spark on HDInsight (Java)**

      - **A Spark on HDInsight-fürt futtatási minta (Scala)**

      Ez a példa egy **a Spark on HDInsight fürt futtatása minta (Scala)** sablont.

   c. A hello **Build eszköz** listára, válassza ki a következő, tooyour szükség szerint hello valamelyikét:

      - **Maven**, Scala-projekt létrehozása varázsló támogatásához

      -  **SBT**, hello függőségek kezelése és hello Scala-projekt létrehozása 

      ![Hibakeresési-projekt létrehozása](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-create-projectfor-debug-remotely.png)

   d. Válassza ki **következő**.     
 
3. Hello a következő **új projekt** ablakban, a következő hello:

   ![Válassza ki a hello Spark SDK](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-new-project.png)

   a. Adja meg a projekt nevét és a projekt helyére.

   b. A hello **projekt SDK** legördülő listában válassza **Java 1.8** a **Spark 2.x** fürt, vagy válasszon **Java 1.7** a **Spark 1. x** fürt.

   c. A hello **Spark verzió** legördülő listából válassza ki, hello Scala projekt létrehozása varázsló Spark SDK és Scala SDK integrálja a hello megfelelő verzióját. Ha hello spark-fürt verziója korábbi, mint 2,0, válassza ki a **Spark 1.x**. Máskülönben válassza **Spark 2.x.** Ez a példa **Spark 2.0.2 (Scala 2.11.8)**.

   d. Válassza a **Finish** (Befejezés) elemet.

4. Válassza ki **src** > **fő** > **scala** tooopen hello projekt kódját. Ez a példa hello **SparkCore_wasbloTest** parancsfájl.

5. tooaccess hello **szerkesztése konfigurációk** menü, hello jobb felső sarokban válassza hello ikonra. Ebből a menüből is készít vagy szerkeszt hello konfigurációk távoli hibakereséshez.

   ![Konfigurációk szerkesztése](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-edit-configurations.png) 

6. A hello **Futtatás/Debug konfigurációk** párbeszédpanel megnyitásához, jelölje be hello plusz jel (**+**). Válassza ki hello **Spark feladat elküldése** lehetőséget.

   ![Adja hozzá az új konfiguráció](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-add-new-Configuration.png)
7. Adja meg a információkat **neve**, **Spark-fürt**, és **fő osztálynév**. Válassza ki **speciális konfigurációs**. 

   ![Futtassa a hibakeresési konfigurációk](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-run-debug-configurations.png)

8. A hello **Spark küldésének speciális konfiguráció** párbeszédpanelen jelölje ki **engedélyezése Spark távoli hibakeresési**. Adja meg az SSH-felhasználónév hello, majd adjon meg egy jelszót vagy titkos kulcsfájlt. toosave hello konfigurációs, jelölje be **OK**.

   ![Spark távoli hibakeresés engedélyezése](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-enable-spark-remote-debug.png)

9. hello konfigurációs megadott hello nevű mentése. tooview hello konfigurációs adatokat, válassza hello konfiguráció neve. toomake módosításokat, válassza ki **szerkesztése konfigurációk**. 

10. Hello konfigurációs beállítások elvégzése után hello projekt futtatásához hello távoli fürt, vagy hajtsa végre a távoli hibakeresés.

## <a name="learn-how-tooperform-remote-debugging"></a>Megtudhatja, hogyan tooperform távoli hibakeresés
### <a name="scenario-1-perform-remote-run"></a>1. forgatókönyv: Hajtsa végre a távoli Futtatás

Ez a szakasz azt mutatja be toodebug illesztőprogramok és végrehajtója.

    import org.apache.spark.{SparkConf, SparkContext}

    object LogQuery {
      val exampleApacheLogs = List(
        """10.10.10.10 - "FRED" [18/Jan/2013:17:56:07 +1100] "GET http://images.com/2013/Generic.jpg
          | HTTP/1.1" 304 315 "http://referall.com/" "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1;
          | GTB7.4; .NET CLR 2.0.50727; .NET CLR 3.0.04506.30; .NET CLR 3.0.04506.648; .NET CLR
          | 3.5.21022; .NET CLR 3.0.4506.2152; .NET CLR 1.0.3705; .NET CLR 1.1.4322; .NET CLR
          | 3.5.30729; Release=ARP)" "UD-1" - "image/jpeg" "whatever" 0.350 "-" - "" 265 923 934 ""
          | 62.24.11.25 images.com 1358492167 - Whatup""".stripMargin.lines.mkString,
        """10.10.10.10 - "FRED" [18/Jan/2013:18:02:37 +1100] "GET http://images.com/2013/Generic.jpg
          | HTTP/1.1" 304 306 "http:/referall.com" "Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1;
          | GTB7.4; .NET CLR 2.0.50727; .NET CLR 3.0.04506.30; .NET CLR 3.0.04506.648; .NET CLR
          | 3.5.21022; .NET CLR 3.0.4506.2152; .NET CLR 1.0.3705; .NET CLR 1.1.4322; .NET CLR
          | 3.5.30729; Release=ARP)" "UD-1" - "image/jpeg" "whatever" 0.352 "-" - "" 256 977 988 ""
          | 0 73.23.2.15 images.com 1358492557 - Whatup""".stripMargin.lines.mkString
      )
      def main(args: Array[String]) {
        val sparkconf = new SparkConf().setAppName("Log Query")
        val sc = new SparkContext(sparkconf)
        val dataSet = sc.parallelize(exampleApacheLogs)
        // scalastyle:off
        val apacheLogRegex =
          """^([\d.]+) (\S+) (\S+) \[([\w\d:/]+\s[+\-]\d{4})\] "(.+?)" (\d{3}) ([\d\-]+) "([^"]+)" "([^"]+)".*""".r
        // scalastyle:on
        /** Tracks hello total query count and number of aggregate bytes for a particular group. */
        class Stats(val count: Int, val numBytes: Int) extends Serializable {
          def merge(other: Stats): Stats = new Stats(count + other.count, numBytes + other.numBytes)
          override def toString: String = "bytes=%s\tn=%s".format(numBytes, count)
        }
        def extractKey(line: String): (String, String, String) = {
          apacheLogRegex.findFirstIn(line) match {
            case Some(apacheLogRegex(ip, _, user, dateTime, query, status, bytes, referer, ua)) =>
              if (user != "\"-\"") (ip, user, query)
              else (null, null, null)
            case _ => (null, null, null)
          }
        }
        def extractStats(line: String): Stats = {
          apacheLogRegex.findFirstIn(line) match {
            case Some(apacheLogRegex(ip, _, user, dateTime, query, status, bytes, referer, ua)) =>
              new Stats(1, bytes.toInt)
            case _ => new Stats(1, 0)
          }
        }
        
        dataSet.map(line => (extractKey(line), extractStats(line)))
          .reduceByKey((a, b) => a.merge(b))
          .collect().foreach{
          case (user, query) => println("%s\t%s".format(user, query))}

        sc.stop()
      }
    }


1. A legfrissebb pontok beállítása, majd válassza ki a hello **Debug** ikonra.

   ![Válassza ki a hello hibakeresési ikon](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debug-icon.png)

2. Hello program végrehajtását hello legfrissebb pontot ér el, ha megjelenik egy **illesztőprogram** lapra, és két **végrehajtó** hello lapfülek **hibakereső** ablaktáblán. Jelölje be hello **folytatása Program** ikon toocontinue hello kód, amely majd eléri a következő töréspont hello és hello megfelelő összpontosít futtató **végrehajtó** lapon. Áttekintheti a hello a végrehajtási naplók hello megfelelő **konzol** fülre.

   ![Hibakeresés lap](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debugger-tab.png)

### <a name="scenario-2-perform-remote-debugging-and-bug-fixing"></a>2. forgatókönyv: Hajtsa végre a távoli hibakereséssel és a hiba elhárítása
Ebben a szakaszban megmutatjuk, hogyan toodynamically frissítés hello változó értékét a hello IntelliJ hibakeresést egy egyszerű javítás funkció. Az alábbi kódpéldát hello a rendszer kivételt hoz létre, mert hello célfájl már létezik.
  
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext

        object SparkCore_WasbIOTest {
          def main(arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("SparkCore_WasbIOTest")
            val sc = new SparkContext(conf)
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

            // Find hello rows that have only one digit in hello sixth column.
            val rdd1 = rdd.filter(s => s.split(",")(6).length() == 1)

            try {
              var target = "wasb:///HVACout2_testdebug1";
              rdd1.saveAsTextFile(target);
            } catch {
              case ex: Exception => {
                throw ex;
              }
            }
          }
        }


#### <a name="tooperform-remote-debugging-and-bug-fixing"></a>tooperform távoli hibakereséssel és a hiba elhárítása
1. Két legfrissebb pontok beállítása, majd válassza ki a hello **Debug** ikon toostart hello távoli folyamat hibakeresése.

2. hello kód nem hello első legfrissebb ponton, és hello paraméter és a változó információk láthatók hello **változók** ablaktáblán. 

3. Jelölje be hello **folytatása Program** ikon toocontinue. hello kód megáll hello második pont. hello kivétel várt módon.

  ![A throw hiba](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-throw-error.png) 

4. Jelölje be hello **folytatása Program** újra ikonra. Hello **HDInsight Spark küldésének** ablak egy "feladat futtatása nem sikerült" hibaüzenet jelenik meg.

  ![Hibaüzenet küldése](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-error-submission.png) 

5. toodynamically frissítés hello változó értéke által hello IntelliJ hibakeresési funkcióval, válassza ki **Debug** újra. Hello **változók** ablaktáblán jelenik meg újra. 

6. Kattintson a jobb gombbal hello célja a hello **Debug** lapra, majd válassza ki **érték beállítása**. Ezután adja meg az új érték hello változó. Válassza ki **Enter** toosave hello érték. 

  ![Érték beállítása](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-set-value.png) 

7. Jelölje be hello **folytatása Program** ikon toocontinue toorun hello program. Nincs kivétel ebben az esetben. Láthatja, hogy hello projekt sikeresen kivételek nélkül futtatja.

  ![Kivétel nélkül hibakeresése](./media/hdinsight-apache-spark-intellij-tool-debug-remotely/hdinsight-debug-without-exception.png)

## <a name="seealso"></a>Következő lépések
* [Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)](hdinsight-apache-spark-overview.md)

### <a name="demo"></a>Bemutató
* Hozzon létre Scala project (videó): [Spark Scala-alkalmazások létrehozása](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Távoli hibakeresési (videó): [IntelliJ toodebug Spark-alkalmazások távolról a HDInsight-fürtök használata Azure eszköztára](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>Forgatókönyvek
* [Spark és BI: interaktív adatelemzés végrehajtása a Spark hdinsight BI-eszközökkel](hdinsight-apache-spark-use-bi-tools.md)
* [Spark és Machine Learning: használja a Spark on HDInsight tooanalyze épület-hőmérséklet HVAC-adatok](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: Használja a Spark on HDInsight toobuild valós idejű streamelési alkalmazások](hdinsight-apache-spark-eventhub-streaming.md)
* [A webhelynapló elemzése a Spark on HDInsight használatával](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Alkalmazások létrehozása és futtatása
* [Önálló alkalmazás létrehozása a Scala használatával](hdinsight-apache-spark-create-standalone-application.md)
* [Feladatok távoli futtatása Spark-fürtön a Livy használatával](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Eszközök és bővítmények
* [Az IntelliJ toocreate Spark-alkalmazások HDInsight-fürtök használata Azure eszköztára](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Az IntelliJ toodebug Spark-alkalmazások VPN-en keresztül távolról használható Azure eszköztára](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Használja a HDInsight Tools for IntelliJ a Hortonworks védőfal](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [HDInsight-eszközök használata az Eclipse toocreate Spark-alkalmazások Azure eszköztára](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Zeppelin notebookok használata Spark-fürttel HDInsighton](hdinsight-apache-spark-zeppelin-notebook.md)
* [A HDInsight Spark-fürt hello Jupyter notebookokhoz elérhető kernelek](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Külső csomagok használata Jupyter notebookokkal](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Erőforrások kezelése
* [Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése](hdinsight-apache-spark-resource-manager.md)
* [Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban](hdinsight-apache-spark-job-debugging.md)
