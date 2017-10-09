---
title: "Az IntelliJ Azure eszköztára: Spark-alkalmazások a HDInsight-fürtök létrehozása |} Microsoft Docs"
description: "IntelliJ toodevelop Spark-alkalmazások scalában írt hello Azure eszközkészlet használni, és küldje el azokat a HDInsight Spark-fürt tooan."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 73304272-6c8b-482e-af7c-cd25d95dab4d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: nitinme
ms.openlocfilehash: 22cce014bb848a54e198e77a50bf13448012310e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-toolkit-for-intellij-toocreate-spark-applications-for-an-hdinsight-cluster"></a>Az IntelliJ toocreate Spark-alkalmazások HDInsight-fürtök használata Azure eszköztára

Az IntelliJ beépülő modul toodevelop Spark-alkalmazások scalában írt hello Azure eszközkészlet használni, és ezután küldenie kell őket tooan hello IntelliJ integrált fejlesztési környezeti (IDE) közvetlenül a HDInsight Spark-fürt. Néhány beépülő modulja hello használhatja:

* Fejleszthet, és küldje el a Scala Spark alkalmazás egy HDInsight Spark-fürtön.
* Az Azure HDInsight Spark-fürt erőforrások eléréséhez.
* Fejleszthet, és egy Scala Spark alkalmazás helyileg történő futtatása.

toocreate a projekt, a nézet hello [Spark-alkalmazások létrehozása az intellij-t Azure eszköztára hello](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) videó.

> [!IMPORTANT]
> A beépülő modul toocreate használja, és küldje el az alkalmazásokat csak egy HDInsight Spark-fürt Linux rendszeren.
> 

## <a name="prerequisites"></a>Előfeltételek

- HDInsight Linux rendszeren Apache Spark-fürt. Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).
- Oracle Java fejlesztői készlet. A későbbiekben telepítheti az hello [Oracle webhely](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
- IntelliJ IDEA. Ez a cikk 2017.1 verzióját használja. A későbbiekben telepítheti az hello [JetBrains webhely](https://www.jetbrains.com/idea/download/).

## <a name="install-azure-toolkit-for-intellij"></a>Az intellij-t Azure eszközkészlet telepítése
A telepítési utasításokért lásd: [Azure eszközkészlet telepítése az IntelliJ](../azure-toolkit-for-intellij-installation.md).

## <a name="sign-in-tooyour-azure-subscription"></a>Jelentkezzen be tooyour Azure-előfizetés

1. Indítsa el a hello IntelliJ IDE, és nyissa meg az Azure-kezelővel. A hello **nézet** menüjében válassza **eszköz Windows**, majd válassza ki **Azure Explorer**.
       
   ![hello Azure Explorer hivatkozás](./media/hdinsight-apache-spark-intellij-tool-plugin/show-azure-explorer.png)

2. Kattintson a jobb gombbal hello **Azure** csomópont, és válassza **bejelentkezés**.

3. A hello **Azure bejelentkezés** párbeszédpanelen jelölje ki **bejelentkezés**, és írja be Azure hitelesítő adatait.

    ![hello Azure bejelentkezés párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-2.png)

4. Be van jelentkezve, miután hello **előfizetések kiválasztása** párbeszédpanel bezárásához listák összes hello Azure-előfizetések társított hello hitelesítő adatokat. Jelölje be hello **válasszon** gombra.

    ![hello előfizetések kiválasztása párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/Select-Subscriptions.png)

5. A hello **Azure Explorer** lapján bontsa ki a **HDInsight** tooview hello HDInsight Spark-fürtök, amelyek az előfizetésben.
   
    ![HDInsight Spark-fürtjei az Azure-kezelővel](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-3.png)

6. tooview hello erőforrások (például storage-fiókok) hello fürt társított ennél jobban is kibonthatja a fürtnév csomópont.
   
    ![Egy bővített fürtnév csomópont](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-4.png)

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>Futtassa a Spark Scala-alkalmazások HDInsight Spark-fürt

1. Indítsa el az IntelliJ IDEA, és ezután a projekt létrehozásához. A hello **új projekt** párbeszédpanel mezőbe hello a következő: 

   a. Válassza ki **HDInsight** > **a Spark on HDInsight (Scala)**.

   b. A hello **Build eszköz** listára, válassza ki a következő, tooyour szükség szerint hello valamelyikét:

      * **Maven**, Scala-projekt létrehozása varázsló támogatásához
      * **SBT**, hello függőségek kezelése és hello Scala-projekt létrehozása

    ![hello új projekt párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)

2. Válassza ki **következő**.

3. hello Scala projekt-létrehozási varázsló automatikusan észleli, hogy telepítette-e hello Scala beépülő modult. Válassza ki **telepítése**.

   ![Scala beépülő modul ellenőrzése](./media/hdinsight-apache-spark-intellij-tool-plugin/Scala-Plugin-check-Reminder.PNG) 

4. toodownload hello beépülő modult, jelölje be a Scala **OK**. Hajtsa végre a hello utasításokat toorestart intellij-t. 

   ![hello Scala beépülő modul telepítése párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/Choose-Scala-Plugin.PNG)

5. A hello **új projekt** ablakban, a következő hello:  

    ![Hello Spark SDK kiválasztása](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-new-project.png)

   a. Adja meg a projekt nevét és helyét.

   b. A hello **projekt SDK** legördülő listában válassza **Java 1.8** hello Spark 2.x fürt, vagy válassza ki a **Java 1.7** hello Spark 1.x fürthöz.

   c. A hello **Spark verzió** legördülő listából válassza ki, Scala-projekt létrehozása varázsló hello a verzió megfelelőségének integrálja a Spark SDK és a Scala SDK. Ha hello Spark-fürt verziója korábbi, mint 2,0, válassza ki a **Spark 1.x**. Máskülönben válassza **Spark2.x**. Ez a példa **Spark 2.0.2 (Scala 2.11.8)**.

6. Válassza a **Finish** (Befejezés) elemet.

7. hello Spark projekt automatikusan létrehoz egy összetevő. tooview hello összetevő, a következő hello:

   a. A hello **fájl** menü **szerkezetének**.

   b. A hello **szerkezetének** párbeszédpanelen jelölje ki **összetevők** tooview hello alapértelmezett összetevő, amely jön létre. A saját összetevő hello plusz jel kiválasztásával is létrehozhat (**+**).

      ![Összetevő info hello párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/default-artifact.png)
      
8. Adja hozzá az alkalmazás forráskódjához hello következő tevékenységek végrehajtásával:

   a. A Project Explorer, kattintson a jobb gombbal **src**, pont túl**új**, majd válassza ki **Scala osztály**.
      
      ![Parancsok a Project Explorer Scala osztály létrehozása](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)

   b. A hello **hozzon létre új Scala osztály** párbeszédpanel mezőben adjon meg egy nevet, válasszon **objektum** a hello **jellegű** mezőbe, majd válassza ki **OK**.
      
      ![Hozzon létre új Scala osztály párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)

   c. A hello **MyClusterApp.scala** fájlt, illessze be a kódját a következő hello. hello kód hello adatokat olvas HVAC.csv (az összes HDInsight Spark-fürtjei elérhető), lekéri a hello a sorokat hello hetedik oszlopban hello CSV-fájlban csak egy számot, és kiírja hello kimeneti túl**/HVACOut** hello alapértelmezés szerint a tároló hello fürthöz.

        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
    
        object MyClusterApp{
            def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
    
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    
            //find hello rows that have only one digit in hello seventh column in hello CSV file
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
    
            rdd1.saveAsTextFile("wasb:///HVACOut")
            }
    
        }

9. Futtassa a hello alkalmazást a HDInsight Spark-fürt hello következő tevékenységek végrehajtásával:

   a. A Project Explorer, kattintson a jobb gombbal a hello projekt nevét, majd válassza ki **küldje el a külső alkalmazás tooHDInsight**.
      
      ![hello küldje el a külső alkalmazás tooHDInsight parancs](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)

   b. Meg vannak felszólító tooenter Azure-előfizetés hitelesítő adatait. A hello **Spark küldésének** párbeszédpanelen adja meg a következő értékek hello, és válassza **Submit**.
      
      * A **Spark-fürtök (csak Linux)**, válassza ki hello HDInsight Spark-fürt toorun kívánja az alkalmazást.

      * Összetevő hello IntelliJ projektben, vagy válasszon egy hello merevlemez-meghajtóról.

      * A hello **fő osztálynév** mezőben, válassza hello három pont (**...** ), hello fő osztályban található az alkalmazás forráskódjához, majd válassza ki és **OK**.

        ![hello fő osztály kiválasztása párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-3.png)

      * Nem igényel parancssori argumentumot a hello alkalmazáskód ebben a példában, vagy hivatkozzon a JAR-fájlok kivételével vagy fájlokat, mert üres mezőkbe fennmaradó hello hagyhatja. Miután megadta a hello kapcsolatos összes információ, hello párbeszédpanel kép a következő hello kell hasonlítania.
        
        ![hello Spark küldésének párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-2.png)

   c. Hello **Spark küldésének** hello ablak hello alján lapon kell kezdenie megjelenítése hello folyamatban van. Hello alkalmazás állítsa le a hello piros hello gombra kattintva **Spark küldésének** ablak.
      
      ![hello Spark küldésének ablak](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)
      
      toolearn hogyan tooaccess hello feladatkiemenetét, tekintse meg a hello "hozzáférés és a HDInsight Spark-fürtök kezelése az intellij-t Azure eszközkészlet használatával" című szakaszban ebben a cikkben található.

## <a name="run-or-debug-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a>Futtassa, vagy egy HDInsight Spark-fürt Spark Scala alkalmazások hibakeresése
Javasoljuk továbbá egy másik módszer a hello Spark alkalmazás toohello fürt elküldése. Ehhez hello hello paraméterek beállításával **Futtatás/Debug konfigurációk** IDE. További információkért lásd: [távolról az IntelliJ SSH-n keresztül a Azure eszközkészlet a HDInsight-fürtök a Spark-alkalmazások hibakeresését](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-azure-toolkit-for-intellij"></a>Férhessen hozzá és felügyelhesse a HDInsight Spark-fürtjei IntelliJ Azure eszközkészlet használatával
Az intellij-t Azure eszközkészlet használatával különféle műveleteket hajthat végre.

### <a name="access-hello-job-view"></a>Hozzáférés hello feladat megtekintése
1. Az Azure Explorerben bontsa ki a **HDInsight**, bontsa ki hello Spark-fürt nevét, majd válassza ki **feladatok**.  

    ![Feladatok nézet csomópont](./media/hdinsight-apache-spark-intellij-tool-plugin/job-view-node.png)

2. A jobb oldali hello hello **Spark feladat megtekintése** lap hello fürtön futó összes hello-alkalmazást jeleníti meg. Válassza ki, amelynek toosee további részleteket szeretne hello alkalmazás hello nevét.

    ![Az alkalmazás részletei](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)

3. toodisplay futó feladat alapinformációk, hello feladatgrafikon az egérmutatót. tooview hello szakaszból grafikon és információt, amely minden feladatot hoz létre, válasszon ki egy csomópontot, a hello feladatgrafikon.

    ![Feladat szakasz részletei](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

4. például csak a gyakran használt naplókat, tooview *illesztőprogram Stderr*, *illesztőprogram Stdout*, és *Directory Info*, jelölje be hello **napló** fülre.

    ![Naplófájl részletei](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-log-info.png)

5. Hello Spark előzmények felhasználói felület és a YARN felhasználói felületen (hello alkalmazási szinten) hello hello ablak hello tetején hivatkozás kiválasztásával is megtekintheti.

### <a name="access-hello-spark-history-server"></a>Hello Spark előzmények kiszolgáló
1. Az Azure Explorerben bontsa ki a **HDInsight**, kattintson a jobb gombbal a Spark-fürt nevét, majd válassza ki **nyissa meg a Spark feladatelőzmények felhasználói felület**. 

2. Amikor a rendszer kéri, adja meg a hello fürt rendszergazdai hitelesítő adataival, a megadott hello fürt beállításakor.

3. Hello Spark előzmények server irányítópult használható hello alkalmazás neve toolook hello alkalmazás imént futása befejeződött. Hello megelőző kódot, akkor be hello alkalmazásnév `val conf = new SparkConf().setAppName("MyClusterApp")`. A Spark alkalmazásnév ezért **MyClusterApp**.

### <a name="start-hello-ambari-portal"></a>Indítsa el a hello Ambari portál
1. Az Azure Explorerben bontsa ki a **HDInsight**, kattintson a jobb gombbal a Spark-fürt nevét, majd válassza ki **nyitott fürt Management Portal (Ambari)**. 

2. Amikor a rendszer kéri, adja meg hello fürt hello rendszergazdai hitelesítő adataival. Ezeket a hitelesítő adatokat adott meg hello fürt beállítási folyamata során.

### <a name="manage-azure-subscriptions"></a>Az Azure-előfizetések kezelése
Alapértelmezés szerint az intellij-t Azure eszköztára hello Spark-fürtök az összes Azure-előfizetések sorolja fel. Ha szükséges, megadhatja, hogy szeretné-e tooaccess hello előfizetések. 

1. Az Azure-kezelővel, kattintson a jobb gombbal hello **Azure** gyökércsomópont, és válassza ki **előfizetések kezelése oldalt**. 

2. A hello párbeszédpanelen törölje a hello jelölőnégyzetek következő toohello előfizetések, hogy nem szeretné, hogy tooaccess, és válassza ki **Bezárás**. Igény szerint kiválaszthatja **Kijelentkezés** toosign kívül az Azure-előfizetés tetszés.

## <a name="run-a-spark-scala-application-locally"></a>A Spark Scala alkalmazás helyileg történő futtatása
Használhat Azure eszközkészlet IntelliJ toorun Spark Scala-alkalmazások helyileg a munkaállomáson. hello alkalmazások általában nem kell erőforrásokhoz férnek hozzá toocluster, például a tárolókban, és futtatja, és helyben tesztelheti őket.

### <a name="prerequisite"></a>Előfeltétel
Hello helyi Spark Scala-alkalmazások Windows rendszerű számítógépeken futtatása, közben kivétel, kaphat, ahogy [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356). hello kivétel következik be, mert WinUtils.exe hiányzik a Windows. 

tooresolve Ez a hiba [végrehajtható hello letöltése](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa helyen **C:\WinUtils\bin**. Adja hozzá a hello környezeti változó **HADOOP_HOME**, és állítsa be a hello hello változó értékének túl**C\WinUtils**.

### <a name="run-a-local-spark-scala-application"></a>A helyi Spark Scala-alkalmazások futtatása
1. Indítsa el az IntelliJ IDEA, és hozzon létre egy projektet. 

2. A hello **új projekt** párbeszédpanel mezőbe hello a következő:
   
    a. Válassza ki **HDInsight** > **a Spark on HDInsight helyi futtatási minta (Scala)**.

    b. A hello **Build eszköz** listára, válassza ki a következő, tooyour szükség szerint hello valamelyikét:

      * **Maven**, Scala-projekt létrehozása varázsló támogatásához
      * **SBT**, hello függőségek kezelése és hello Scala-projekt létrehozása

    ![hello új projekt párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run.png)

3. Válassza ki **következő**.
 
4. Az hello hello következő ablakban, a következő:
   
    a. Adja meg a projekt nevét és helyét.

    b. A hello **projekt SDK** legördülő listára, válassza ki a 1.7 verziónál újabb Java-verziót.

    c. A hello **Spark verzió** legördülő listában, jelölje be hello verzióját Scala, amelyet az toouse: Scala Spark 2.0-s vagy Scala 2.11.x 2.10.x a Spark 1.6-os.

    ![hello új projekt párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/Create-local-project.PNG)

5. Válassza a **Finish** (Befejezés) elemet.

6. hello sablon hozzáadása egy mintakód (**LogQuery**) alatt hello **src** mappát, amelyet a számítógépen helyileg futtathat.
   
    ![LogQuery helye](./media/hdinsight-apache-spark-intellij-tool-plugin/local-app.png)

7. Kattintson a jobb gombbal hello **LogQuery** alkalmazás, és adja **Futtatás "LogQuery"**. A hello **futtatása** lapon hello alján hello hasonló kimenetnek lásd:
   
   ![Spark alkalmazás helyi futtatás eredménye](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="convert-existing-intellij-idea-applications-toouse-azure-toolkit-for-intellij"></a>Az IntelliJ alakítsa át a meglévő IntelliJ IDEA alkalmazások toouse Azure eszközkészlet
Hello meglévő Spark Scala alkalmazások meg az IntelliJ IDEA toobe kompatibilis Azure eszközkészlet az IntelliJ válthat. Ezután használhatja a hello beépülő modul toosubmit hello alkalmazások tooan HDInsight Spark-fürt.

1. Egy meglévő Spark Scala-alkalmazás, amely az IntelliJ IDEA használatával hozták létre nyissa meg a hello társított .iml fájlt.

2. Hello gyökerében szintje a **modul** hello következő elem:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">

   Hello elem tooadd szerkesztése `UniqueKey="HDInsightTool"` úgy, hogy hello **modul** elem következőhöz hasonló hello:
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">

3. Hello módosítások mentéséhez. Az alkalmazás most már Azure eszköztára IntelliJ kompatibilisnek kell lennie. Kattintson a jobb gombbal a Project Explorer hello projektnevet tesztelheti. hello előugró menüjét most már hello beállítás **küldje el a külső alkalmazás tooHDInsight**.

## <a name="troubleshooting"></a>Hibaelhárítás

### <a name="error-in-local-run-please-use-a-larger-heap-size"></a>Hiba történt a helyi futtatáskor: *használjon egy nagyobb halommemória mérete*
A Spark 1.6 használata egy 32 bites Java SDK helyi futtatás során léphetnek fel a következő hibák hello:

    Exception in thread "main" java.lang.IllegalArgumentException: System memory 259522560 must be at least 4.718592E8. Please use a larger heap size.
        at org.apache.spark.memory.UnifiedMemoryManager$.getMaxMemory(UnifiedMemoryManager.scala:193)
        at org.apache.spark.memory.UnifiedMemoryManager$.apply(UnifiedMemoryManager.scala:175)
        at org.apache.spark.SparkEnv$.create(SparkEnv.scala:354)
        at org.apache.spark.SparkEnv$.createDriverEnv(SparkEnv.scala:193)
        at org.apache.spark.SparkContext.createSparkEnv(SparkContext.scala:288)
        at org.apache.spark.SparkContext.<init>(SparkContext.scala:457)
        at LogQuery$.main(LogQuery.scala:53)
        at LogQuery.main(LogQuery.scala)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:606)
        at com.intellij.rt.execution.application.AppMain.main(AppMain.java:144)

Ezek a hibák akkor fordulhat elő, mert hello halommemória mérete nem elég nagy a Spark toorun. Spark legalább 471 MB terület szükséges. (További információkért lásd: [SPARK-12081](https://issues.apache.org/jira/browse/SPARK-12081).) Egy egyszerű megoldást egy 64 bites Java SDK toouse. Adja hozzá az alábbi beállítások hello IntelliJ hello JVM beállításait is módosíthatja:

    -Xms128m -Xmx512m -XX:MaxPermSize=300m -ea

!["Virtuális gép beállításai" mezőben beállítások toohello hozzáadása az IntelliJ](./media/hdinsight-apache-spark-intellij-tool-plugin/change-heap-size.png)

## <a name="faq"></a>GYIK
Válasszon egy alkalmazás tooAzure Data Lake Store toosubmit **interaktív** mód hello Azure bejelentkezés során. Ha **automatikus** mód, hibaüzenetet kaphat.

![interaktív bejelentkezés](./media/hdinsight-apache-spark-intellij-tool-plugin/interative-signin.png)

Most azt feloldotta azt. Választhat egy Azure Data Lake fürt toosubmit az alkalmazás egyik bejelentkezési módszer.

## <a name="feedback-and-known-issues"></a>Visszajelzések és ismert problémák
Spark kimenetek közvetlenül megtekintése jelenleg nem támogatott.

Ha javaslata vagy visszajelzést, vagy ha ez a beépülő modul használatakor bármely problémákat tapasztal, e-mail nekünk az hdivstool@microsoft.com.

## <a name="seealso"></a>Következő lépések
* [Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)](hdinsight-apache-spark-overview.md)

### <a name="demo"></a>Bemutató
* Hozzon létre Scala project (videó): [Spark Scala-alkalmazások létrehozása](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)
* Távoli hibakeresési (videó): [IntelliJ toodebug Spark-alkalmazások távolról a HDInsight-fürt használata Azure eszköztára](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)

### <a name="scenarios"></a>Forgatókönyvek
* [Spark és BI: interaktív adatelemzés végrehajtása a Spark hdinsight BI-eszközökkel](hdinsight-apache-spark-use-bi-tools.md)
* [Spark és Machine Learning: használja a Spark on HDInsight tooanalyze épület-hőmérséklet HVAC-adatok](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: Használja a Spark on HDInsight toobuild valós idejű streamelési alkalmazások](hdinsight-apache-spark-eventhub-streaming.md)
* [A webhelynapló elemzése a Spark on HDInsight használatával](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Létrehozása és alkalmazások futtatása
* [Önálló alkalmazás létrehozása a Scala használatával](hdinsight-apache-spark-create-standalone-application.md)
* [Feladatok távoli futtatása Spark-fürtön a Livy használatával](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Eszközök és bővítmények
* [Az IntelliJ toodebug Spark-alkalmazások VPN-en keresztül távolról használható Azure eszköztára](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Az IntelliJ toodebug Spark-alkalmazások SSH keresztül távolról használható Azure eszköztára](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Használja a HDInsight Tools for IntelliJ a Hortonworks védőfal](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [HDInsight-eszközök használata az Eclipse toocreate Spark-alkalmazások Azure eszköztára](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [Zeppelin notebookok használata Spark-fürttel HDInsighton](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Külső csomagok használata Jupyter notebookokkal](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Erőforrások kezelése
* [Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése](hdinsight-apache-spark-resource-manager.md)
* [Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban](hdinsight-apache-spark-job-debugging.md)

