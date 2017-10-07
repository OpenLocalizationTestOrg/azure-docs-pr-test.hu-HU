---
title: "aaaAzure Eclipse - alkalmazások HDInsight Spark Scala létrehozása eszköztára |} Microsoft Docs"
description: "A HDInsight Tools használni az Azure-eszközkészlet Eclipse toodevelop Spark-alkalmazások scalában írt és küldheti el ezeket a HDInsight Spark-fürt tooan, azokat közvetlenül a hello Eclipse IDE."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: f6c79550-5803-4e13-b541-e86c4abb420b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: nitinme
ms.openlocfilehash: 3ab70857c1e81f591a1c7e29bc1706ec4899ff58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-toolkit-for-eclipse-toocreate-spark-applications-for-an-hdinsight-cluster"></a>Azure eszközkészlet használata az Eclipse toocreate Spark-alkalmazások HDInsight-fürtök

A HDInsight Tools használni az Azure-eszközkészlet Eclipse toodevelop Spark-alkalmazások scalában írt, és küldje el azokat tooan Azure HDInsight Spark-fürt közvetlenül a hello Eclipse IDE. Hello HDInsight eszközök beépülő modul néhány különböző módokon használhatja:

* toodevelop, és küldje el a Scala Spark-alkalmazás egy HDInsight Spark-fürt
* tooaccess az Azure HDInsight Spark-fürt erőforrások
* toodevelop, és futtassa helyileg a Scala Spark-alkalmazások

> [!IMPORTANT]
> Ez az eszköz használt toocreate kell, és küldje el az alkalmazásokat csak egy HDInsight Spark-fürt Linux rendszeren.
> 
> 

## <a name="prerequisites"></a>Előfeltételek

* A HDInsight az Apache Spark-fürt. Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).
* Oracle Java fejlesztői készlet 8-as verzió, amely hello Eclipse IDE futásidejű használható. Letölthető hello [Oracle webhely](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* Eclipse IDE. Ebben a cikkben az Eclipse Neonfény. A későbbiekben telepítheti az hello [Eclipse webhely](https://www.eclipse.org/downloads/).   
* Spark SDK. Letöltheti a [GitHub](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409).


## <a name="install-hdinsight-tools-in-azure-toolkit-for-eclipse-and-scala-plugin"></a>A HDInsight Tools Azure eszközkészlet az eclipse-ben és a Scala beépülő modul telepítése
### <a name="install-hdinsight-tools"></a>HDInsight eszközök telepítése
A HDInsight Tools for eclipse-ben érhető el az Eclipse Azure eszköztára részeként. A telepítési utasításokért lásd: [Azure eszközkészlet telepítése az Eclipse](../azure-toolkit-for-eclipse-installation.md).
### <a name="install-scala-plugin"></a>Scala beépülő modul telepítése
Az Intellij hello megnyitásakor hello HDInsight eszközök automatikus észleli, hogy Scala beépülő modul telepítve, vagy nem. Kattintson a **OK** toocontinue és követi hello utasításokat tooinstall hello Eclipse piactér által.

 ![Automatikus telepítés Scala beépülő modul](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala.png)

## <a name="sign-in-tooyour-azure-subscription"></a>Jelentkezzen be tooyour Azure-előfizetés
1. Indítsa el a hello Eclipse IDE, és nyissa meg az Azure-kezelővel. A hello **ablak** menüben kattintson a **nézet megjelenítése**, és kattintson a **más**. A hello párbeszédpanel, bontsa ki a **Azure**, kattintson a **Azure Explorer**, és kattintson a **OK**.

    ![Nézet párbeszédpanel megjelenítése](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-1.png)
2. Kattintson a jobb gombbal hello **Azure** csomópontra, majd **bejelentkezés**.
3. A hello **Azure bejelentkezés** párbeszédpanel, hello hitelesítési módszer kiválasztása párbeszédpanelen kattintson **jelentkezzen be a**, és az Azure hitelesítő adatait.
   
    ![Az Azure bejelentkezési párbeszédpanel](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-2.png)
4. Be van jelentkezve, miután hello **előfizetések kiválasztása** párbeszédpanel bezárásához listák összes hello Azure-előfizetéssel társított hello hitelesítő adatokat. Kattintson a **válasszon** tooclose hello párbeszédpanel megnyitásához.

    ![Válassza ki az előfizetések párbeszédpanel](./media/hdinsight-apache-spark-eclipse-tool-plugin/Select-Subscriptions.png)
5. A hello **Azure Explorer** lapján bontsa ki a **HDInsight** HDInsight Spark-fürtök az előfizetéshez tartozó toosee hello.
   
    ![HDInsight Spark-fürtjei az Azure-kezelővel](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-3.png)
6. Ennél jobban is kibonthatja a fürt neve csomópont toosee hello erőforrások (például storage-fiókok) hello-fürthöz tartozó.
   
    ![A fürterőforrások toosee kibontása](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-4.png)



## <a name="set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster"></a>A Spark Scala-projektjét, amely egy HDInsight Spark-fürt beállítása

1. Hello Eclipse IDE munkaterületen kattintson **fájl**, kattintson a **új**, és kattintson a **projekt**. 
2. A hello új projekt varázsló, bontsa ki a **HDInsight**, jelölje be **a Spark on HDInsight (Scala)**, és kattintson a **következő**.

    ![Hello Spark on HDInsight (Scala) projekt kiválasztása](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-2.png)
3. hello Scala projekt létrehozása varázsló automatikusan észleli, hogy Scala beépülő modul telepítve, vagy nem. Kattintson a **OK** toocontinue hello Scala beépülő modult, majd hajtsa végre hello utasításokat toorestart Eclipse letöltése.

    ![scala ellenőrzése](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala-2.png)
4. A hello **új HDInsight Scala projekt** párbeszédpanelen adja meg a következő értékek hello, és kattintson **következő**:
   * Adja meg a hello projekt nevét.
   * A hello **JRE** területen győződjön meg arról, hogy **használja a végrehajtási környezet JRE** értéke túl**JavaSE-1.7** vagy újabb.
   * Győződjön meg arról, hogy a Spark SDK hello SDK kezelőportálon toohello hely beállítása. hello hivatkozás toohello letöltési hely megtalálható hello [Előfeltételek](#prerequisites) korábbi ebben a cikkben. Is letölthetők hello SDK hello hello párbeszédpanel szereplő hivatkozásra.

    ![Új HDInsight Scala projekt párbeszédpanel](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-3.png)
5.  A következő párbeszédpanelen hello, kattintson a hello **szalagtárak** lapon, és tartsa hello alapértelmezett beállításokat, majd kattintson **Befejezés**. 
   
    ![Szalagtárak lap](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-4.png)
  
## <a name="create-a-scala-application-for-an-hdinsight-spark-cluster"></a>A HDInsight Spark-fürtök Scala-alkalmazás létrehozása

1. Kattintson a jobb gombbal a hello Eclipse IDE, a csomag Explorer eszközből bontsa ki a korábban létrehozott hello projekt **src**, pont túl**új**, és kattintson a **más**.
2. A hello **válassza ki a varázsló** párbeszédpanelen bontsa ki **Scala varázslók**, kattintson a **Scala objektum**, és kattintson a **következő**.
   
    ![Válassza ki a varázsló párbeszédpanelje](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-1.png)
3. A hello **hozzon létre új fájl** párbeszédpanelen írja be a hello objektum nevét, és kattintson **Befejezés**.
   
    ![Hozzon létre új fájl párbeszédpanel](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-2.png)
4. Illessze be a következő kód hello szövegszerkesztőben hello:
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        object MyClusterApp{
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find hello rows that have only one digit in hello seventh column in hello CSV
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACOut")
          }        
        }
5. Hello az alkalmazás futtatása egy HDInsight Spark-fürt:
   
   1. A csomag Explorer, kattintson a jobb gombbal a hello projekt nevét, majd válassza ki **küldje el a külső alkalmazás tooHDInsight**.        
   2. A hello **Spark küldésének** párbeszédpanelen adja meg a következő értékek hello, és kattintson **Submit**:
      
      * A **fürtnév**, válassza ki hello HDInsight Spark-fürt toorun kívánja az alkalmazást.
      * Válasszon egy közbülső hello Eclipse-projekt, vagy válasszon ki egy, a merevlemez-meghajtóról. hello alapértelmezett érték a jobb gombbal a csomag explorer hello elem függ.
      * A hello **fő osztálynév** dropdownlist, küldésének varázsló megjeleníti a kijelölt projektből az összes objektum nevét. Válassza ki vagy adjon meg egy megjeleníteni kívánt toorun. Összetevő merevlemezről válassza ki, ha saját maga által megadott kell fő osztály nevét. 
      * Nem igényel parancssori argumentumokat a hello alkalmazáskód ebben a példában, vagy hivatkozzon a JAR-fájlok kivételével vagy fájlokat, mert üres szövegmezőkben fennmaradó hello hagyhatja.
        
       ![Spark küldésének párbeszédpanel](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-3.png)
   3. Hello **Spark küldésének** lapon kell kezdenie megjelenítése hello folyamatban van. Hello alkalmazás hello piros hello gombra kattintva leállíthatja **Spark küldésének** ablak. Hello naplók az adott alkalmazás futtassa hello földgömb ikon (hello kék mező hello kép jelölik) kattintva is megtekintheti.
      
       ![Spark küldésének ablak](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-4.png)

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-hdinsight-tools-in-azure-toolkit-for-eclipse"></a>Férhessen hozzá és felügyelhesse a HDInsight Spark-fürtjei a HDInsight Tools használatával Azure eszközkészlet az eclipse-ben
A HDInsight Tools történő hozzáféréshez hello feladatkiemenetét használatával különféle műveleteket hajthat végre.

### <a name="access-hello-job-view"></a>Hozzáférés hello feladat megtekintése
1. Az Azure Explorerben bontsa ki a **HDInsight**, bontsa ki hello Spark-fürt nevét, és kattintson a **feladatok**. 

    ![Feladatok nézet csomópont](./media/hdinsight-apache-spark-intellij-tool-plugin/job-view-node.png)

2. Kattintson a hello **feladatok** csomópont. hello HDInsight eszközök automatikus-észleli, hogy hello E (fx) clipse beépülő modul telepítve, vagy nem. Kattintson a **OK** tooinstall hello Eclipse piactéren, és indítsa újra az Eclipse toocontinue és követi hello utasításokat.

    ![E (fx) clipse telepítése](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-efxclipse.png)

3. Nyissa meg a feladat megtekintése a hello hello **feladatok** csomópont. A jobb oldali hello hello **Spark feladat megtekintése** lap hello fürtön futó összes hello-alkalmazást jeleníti meg. Kattintson a kívánt toosee további részleteket hello alkalmazás hello neve.

    ![Az alkalmazás részletei](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)
4. Ha a feladat ábra hello mutat, alapszintű futó feladat adatait jeleníti meg. Kattintson a hello feladatgrafikon hello szakaszból graph és adatait, amely minden feladatot hoz létre tartalmazza.

    ![Feladat szakasz részletei](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

5. Gyakran használt naplókat, például illesztőprogram Stderr, illesztőprogram Stdout és Directory adatai láthatók hello **napló** fülre.

    ![Naplófájl részletei](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-log-info.png)
6. Hello Spark előzmények felhasználói felület és a YARN felhasználói felületen (hello alkalmazási szinten) hello hello megfelelő hivatkozás hello ablak hello tetején kattintva is megnyithatja.

### <a name="access-hello-storage-container-for-hello-cluster"></a>Hozzáférés a hello tároló hello fürt
1. Az Azure Explorerben bontsa ki a hello **HDInsight** legfelső szintű csomópont toosee elérhető HDInsight Spark-fürtök listáját.
2. Bontsa ki a hello fürt neve toosee hello tárfiók és hello alapértelmezett tároló hello fürthöz.
   
    ![Storage-fiókot és az alapértelmezett tároló](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-5.png)
3. Kattintson a hello hello fürthöz rendelt tárolási Tárolónév. Hello jobb oldali ablaktáblában kattintson duplán a hello **HVACOut** mappa. Nyissa meg az egyik hello **rész -** fájlok hello alkalmazás toosee hello kimenete.

### <a name="access-hello-spark-history-server"></a>Hello Spark előzmények kiszolgáló
1. Az Azure-kezelővel, kattintson a jobb gombbal a Spark-fürt nevét, majd válassza ki **nyissa meg a Spark feladatelőzmények felhasználói felület**. Amikor a rendszer kéri, adja meg hello fürt hello rendszergazdai hitelesítő adataival. Ezek hello fürt kiépítése során van megadva.
2. Hello Spark előzmények kiszolgáló irányítópultjának használ hello alkalmazás neve toolook hello alkalmazás imént futása befejeződött. Hello megelőző kódot, akkor be hello alkalmazásnév `val conf = new SparkConf().setAppName("MyClusterApp")`. Emiatt a Spark alkalmazásnév lett **MyClusterApp**.

### <a name="start-hello-ambari-portal"></a>Indítsa el a hello Ambari portál
1. Az Azure-kezelővel, kattintson a jobb gombbal a Spark-fürt nevét, majd válassza ki **nyitott fürt Management Portal (Ambari)**. 
2. Amikor a rendszer kéri, adja meg hello fürt hello rendszergazdai hitelesítő adataival. Ezek hello fürt kiépítése során van megadva.

### <a name="manage-azure-subscriptions"></a>Az Azure-előfizetések kezelése
Alapértelmezés szerint a HDInsight Tools az Eclipse Azure eszköztára hello Spark-fürtök az összes Azure-előfizetések sorolja fel. Ha szükséges, megadhatja a kívánt tooaccess hello fürt hello előfizetések. 

1. Az Azure-kezelővel, kattintson a jobb gombbal hello **Azure** gyökércsomópont, és kattintson a **előfizetések kezelése oldalt**. 
2. A hello párbeszédpanelen törölje a hello előfizetés, hogy nem szeretné, hogy tooaccess, és kattintson a hello jelölőnégyzeteit **Bezárás**. Is **Kijelentkezés** toosign kívül az Azure-előfizetés tetszés.

## <a name="run-a-spark-scala-application-locally"></a>A Spark Scala alkalmazás helyileg történő futtatása
Használhatja a HDInsight Tools Azure eszközkészlet Eclipse toorun Spark Scala-alkalmazások helyileg a munkaállomáson. Általában ezek az alkalmazások hozzáférési toocluster erőforrások, például a tároló nem szükséges, és futtatja, és helyben tesztelheti őket.

### <a name="prerequisite"></a>Előfeltétel
Hello helyi Spark Scala-alkalmazások Windows rendszerű számítógépeken futtatása, közben kivétel kaphat, ahogy [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356). Ez a kivétel akkor fordul elő, mert **WinUtils.exe** Windows hiányzik. 

tooresolve Ez a hiba kell [végrehajtható hello letöltése](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) tooa helyre, például **C:\WinUtils\bin**. Hozzá kell majd adnia a hello környezeti változó **HADOOP_HOME** és hello hello változó értékének túl**C\WinUtils**.

### <a name="run-a-local-spark-scala-application"></a>A helyi Spark Scala-alkalmazások futtatása
1. Indítsa el az Eclipse, és hozzon létre egy projektet. A hello **új projekt** párbeszédpanelen hajtsa végre a következő lehetőségek hello, és kattintson **következő**.
   
   * Hello bal oldali ablaktáblában jelöljön ki **HDInsight**.
   * Hello jobb oldali ablaktáblában jelöljön ki **a Spark on HDInsight helyi futtatása minta (Scala)**.

    ![A New Project (Új projekt) párbeszédpanel](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run.png)
2. tooprovide hello projekt részleteit, hajtsa végre lépéseket 3-6 a korábbi szakaszban hello [állítson be egy Spark Scala-projektjét, amely egy HDInsight Spark-fürt](#set-up-a-spark-scala-project-for-an-hdinsight-spark cluster).
3. hello sablon hozzáadása egy mintakód (**LogQuery**) alatt hello **src** mappát, amelyet a számítógépen helyileg futtathat.
   
    ![LogQuery helye](./media/hdinsight-apache-spark-eclipse-tool-plugin/local-app.png)
4. Kattintson a jobb gombbal hello **LogQuery** alkalmazás, pont túl**futtató**, és kattintson a **1 Scala alkalmazás**. Egy hello az ehhez hasonló kimenetet fog látni **konzol** hello alsó lapon:
   
   ![Spark alkalmazás helyi futtatás eredménye](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="faq"></a>GYIK
Válasszon egy alkalmazás tooAzure Data Lake Store toosubmit **interaktív** mód hello Azure bejelentkezés során. Ha **automatikus** mód, hibaüzenetet kaphat.

![interaktív bejelentkezés](./media/hdinsight-apache-spark-eclipse-tool-plugin/interactive-authentication.png)

Most azt feloldotta azt. Választhat egy Azure Data Lake fürt toosubmit az alkalmazás egyik bejelentkezési módszer.

## <a name="feedback-and-known-issues"></a>Visszajelzések és ismert problémák
Spark kimenetek közvetlenül megtekintése jelenleg nem támogatott.

Ha javaslata vagy visszajelzést, vagy ha az eszköz használata során felmerülő problémákat, érzi, hogy szabad toosend, egy e-mailt hdivstool@microsoft.com.

## <a name="seealso"></a>Lásd még:
* [Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Forgatókönyvek
* [Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel](hdinsight-apache-spark-use-bi-tools.md)
* [Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására](hdinsight-apache-spark-eventhub-streaming.md)
* [A webhelynapló elemzése a Spark on HDInsight használatával](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Létrehozása és alkalmazások futtatása
* [Önálló alkalmazás létrehozása a Scala használatával](hdinsight-apache-spark-create-standalone-application.md)
* [Feladatok távoli futtatása Spark-fürtön a Livy használatával](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Eszközök és bővítmények
* [Az IntelliJ toocreate Azure eszközkészlet használja, és küldje el a Spark Scala-alkalmazások](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Az IntelliJ toodebug Spark-alkalmazások VPN-en keresztül távolról használható Azure eszköztára](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Az IntelliJ toodebug Spark-alkalmazások SSH keresztül távolról használható Azure eszköztára](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Használja a HDInsight Tools for IntelliJ a Hortonworks védőfal](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Zeppelin notebookok használata Spark-fürttel HDInsighton](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Külső csomagok használata Jupyter notebookokkal](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Erőforrások kezelése
* [Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése](hdinsight-apache-spark-resource-manager.md)
* [Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban](hdinsight-apache-spark-job-debugging.md)

