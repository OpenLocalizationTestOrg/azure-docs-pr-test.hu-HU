---
title: "Az Eclipse - alkalmazások HDInsight Spark Scala létrehozása az Azure eszközkészlet |} Microsoft Docs"
description: "Azure eszköztára eclipse-ben a HDInsight Tools használatával scalában írt Spark-alkalmazások fejlesztéséhez és küldheti el ezeket a HDInsight Spark-fürt közvetlenül a számukra az Eclipse IDE."
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
ms.openlocfilehash: 4bcb1987a62c0b7f4965e6fd257315e820004238
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-toolkit-for-eclipse-to-create-spark-applications-for-an-hdinsight-cluster"></a>Azure eszköztára Eclipse használata Spark-alkalmazások a HDInsight-fürtök létrehozása

Az Eclipse eszköztára Azure HDInsight Tools használatával scalában írt Spark-alkalmazások fejlesztéséhez és küldheti el ezeket az Azure HDInsight Spark-fürt az Eclipse IDE-ről. A HDInsight Tools beépülő modul néhány különböző módokon használhatja:

* Fejlesztésére, és küldje el a Scala Spark-alkalmazás egy HDInsight Spark-fürt
* Az Azure HDInsight Spark-fürt erőforrások eléréséhez.
* Fejlesztésére és Scala Spark-alkalmazás helyileg történő futtatása

> [!IMPORTANT]
> Ez az eszköz létrehozásához és elküldéséhez az alkalmazások csak a HDInsight Spark-fürt Linux rendszeren használható.
> 
> 

## <a name="prerequisites"></a>Előfeltételek

* A HDInsight az Apache Spark-fürt. Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).
* Oracle Java fejlesztői készlet 8-as verzió, az Eclipse IDE futásidejű használt. Letöltheti azt a [Oracle webhely](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
* Eclipse IDE. Ebben a cikkben az Eclipse Neonfény. Telepítheti azt a [Eclipse webhely](https://www.eclipse.org/downloads/).   
* Spark SDK. Letöltheti a [GitHub](http://go.microsoft.com/fwlink/?LinkID=723585&clcid=0x409).


## <a name="install-hdinsight-tools-in-azure-toolkit-for-eclipse-and-scala-plugin"></a>A HDInsight Tools Azure eszközkészlet az eclipse-ben és a Scala beépülő modul telepítése
### <a name="install-hdinsight-tools"></a>HDInsight eszközök telepítése
A HDInsight Tools for eclipse-ben érhető el az Eclipse Azure eszköztára részeként. A telepítési utasításokért lásd: [Azure eszközkészlet telepítése az Eclipse](../azure-toolkit-for-eclipse-installation.md).
### <a name="install-scala-plugin"></a>Scala beépülő modul telepítése
Az Intellij megnyitásakor a HDInsight Tools automatikusan észleli, hogy Scala beépülő modul telepítve, vagy nem. Kattintson a **OK** folytatja, és kövesse az utasításokat az Eclipse piactér a telepítést.

 ![Automatikus telepítés Scala beépülő modul](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala.png)

## <a name="sign-in-to-your-azure-subscription"></a>Jelentkezzen be az Azure-előfizetésébe
1. Indítsa el az Eclipse IDE, és nyissa meg az Azure-kezelővel. Az a **ablak** menüben kattintson a **nézet megjelenítése**, és kattintson a **más**. A megnyíló párbeszédpanelen bontsa ki a **Azure**, kattintson a **Azure Explorer**, és kattintson a **OK**.

    ![Nézet párbeszédpanel megjelenítése](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-1.png)
2. Kattintson a jobb gombbal a **Azure** csomópontra, majd **bejelentkezés**.
3. Az a **Azure bejelentkezés** párbeszédpanelen, a hitelesítési módszer kiválasztása párbeszédpanelen kattintson **jelentkezzen be a**, Azure hitelesítő adataival.
   
    ![Az Azure bejelentkezési párbeszédpanel](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-2.png)
4. Miután bejelentkezett a, a **előfizetések kiválasztása** párbeszédpanel megjeleníti az összes Azure-előfizetések társított hitelesítő adatok. Kattintson a **válasszon** a párbeszédpanel bezárásához.

    ![Válassza ki az előfizetések párbeszédpanel](./media/hdinsight-apache-spark-eclipse-tool-plugin/Select-Subscriptions.png)
5. Az a **Azure Explorer** lapján bontsa ki a **HDInsight** a HDInsight Spark-fürtök az előfizetéshez tartozó megjelenítéséhez.
   
    ![HDInsight Spark-fürtjei az Azure-kezelővel](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-3.png)
6. Ennél jobban is kibonthatja az erőforrások (például storage-fiókok) a fürthöz tartozó megjelenítéséhez fürtcsomópont nevét.
   
    ![A fürt nevét cikkekben találhat kibontása](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-4.png)



## <a name="set-up-a-spark-scala-project-for-an-hdinsight-spark-cluster"></a>A Spark Scala-projektjét, amely egy HDInsight Spark-fürt beállítása

1. Az Eclipse IDE munkaterületen kattintson **fájl**, kattintson a **új**, és kattintson a **projekt**. 
2. Bontsa ki az új projekt varázsló **HDInsight**, jelölje be **a Spark on HDInsight (Scala)**, és kattintson a **következő**.

    ![A Spark on HDInsight (Scala) projekt kiválasztása](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-2.png)
3. A Scala projekt létrehozása varázsló automatikusan észleli, hogy Scala beépülő modul telepítve, vagy nem. Kattintson a **OK** folytatja, a Scala beépülő modul letöltése, és kövesse az utasításokat az Eclipse újraindítását.

    ![scala ellenőrzése](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-scala-2.png)
4. Az a **új HDInsight Scala projekt** párbeszédpanelen adja meg a következő értékeket, és kattintson **következő**:
   * Adja meg a projekt nevét.
   * A a **JRE** területen győződjön meg arról, hogy **használja a végrehajtási környezet JRE** értéke **JavaSE-1.7** vagy újabb.
   * Győződjön meg arról, hogy a helyet, ahol az SDK-val letöltött Spark SDK van beállítva. A letöltési hely hivatkozás jelenik meg a [Előfeltételek](#prerequisites) korábbi ebben a cikkben. Az SDK-t is letölthetők a hivatkozás szerepel a párbeszédpanelen.

    ![Új HDInsight Scala projekt párbeszédpanel](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-3.png)
5.  A következő párbeszédpanelen, kattintson a **szalagtárak** lapon, és hagyja az alapértelmezett beállításokat, majd kattintson **Befejezés**. 
   
    ![Szalagtárak lap](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-hdi-scala-app-4.png)
  
## <a name="create-a-scala-application-for-an-hdinsight-spark-cluster"></a>A HDInsight Spark-fürtök Scala-alkalmazás létrehozása

1. Az Eclipse IDE a csomag Explorer eszközből bontsa ki a korábban létrehozott projekt, kattintson a jobb gombbal **src**, mutasson a **új**, és kattintson a **más**.
2. Az a **válassza ki a varázsló** párbeszédpanelen bontsa ki **Scala varázslók**, kattintson a **Scala objektum**, és kattintson a **következő**.
   
    ![Válassza ki a varázsló párbeszédpanelje](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-1.png)
3. Az a **hozzon létre új fájl** párbeszédpanelen adja meg az objektum nevét, és kattintson **Befejezés**.
   
    ![Hozzon létre új fájl párbeszédpanel](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-2.png)
4. Illessze be a következő kódot a szövegszerkesztőben:
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        object MyClusterApp{
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find the rows that have only one digit in the seventh column in the CSV
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACOut")
          }        
        }
5. Futtassa az alkalmazást egy HDInsight Spark-fürt:
   
   1. A csomag Explorer, kattintson a jobb gombbal a projekt nevére, majd válassza ki **küldje el a Spark-alkalmazást, amely**.        
   2. Az a **Spark küldésének** párbeszédpanelen adja meg a következő értékeket, és kattintson **Submit**:
      
      * A **fürtnév**, válassza ki a HDInsight Spark-fürt, amelyen szeretné futtatni az alkalmazást.
      * Válasszon egy összetevő az Eclipse-projekt, vagy válasszon ki egy, a merevlemez-meghajtóról. Az alapértelmezett érték a cikk a jobb gombbal a csomag explorer függ.
      * Az a **fő osztálynév** dropdownlist, küldésének varázsló megjeleníti a kijelölt projektből az összes objektum nevét. Válassza ki vagy adjon meg egy, a futtatni kívánt. Összetevő merevlemezről válassza ki, ha saját maga által megadott kell fő osztály nevét. 
      * Mivel ebben a példában az alkalmazás kódja nem igényel parancssori argumentumokat vagy hivatkozás JAR-fájlok kivételével vagy fájlokat, üresen többi mezőbe.
        
       ![Spark küldésének párbeszédpanel](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-3.png)
   3. A **Spark küldésének** lapon kell kezdenie a folyamatban lévő megjelenítése. A piros gombra kattintva leállíthatja az alkalmazás a **Spark küldésének** ablak. A naplók az adott alkalmazáshoz, futtassa a földgömb ikon (az ábrán kék mező jelölik) kattintva is megtekintheti.
      
       ![Spark küldésének ablak](./media/hdinsight-apache-spark-eclipse-tool-plugin/create-scala-proj-4.png)

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-hdinsight-tools-in-azure-toolkit-for-eclipse"></a>Férhessen hozzá és felügyelhesse a HDInsight Spark-fürtjei a HDInsight Tools használatával Azure eszközkészlet az eclipse-ben
A feladat kimenetére történő hozzáféréshez, a HDInsight Tools használatával különféle műveleteket hajthat végre.

### <a name="access-the-job-view"></a>Hozzáférés a feladat megtekintése
1. Az Azure Explorerben bontsa ki a **HDInsight**, bontsa ki a Spark-fürt nevére, majd **feladatok**. 

    ![Feladatok nézet csomópont](./media/hdinsight-apache-spark-intellij-tool-plugin/job-view-node.png)

2. Kattintson a **feladatok** csomópont. A HDInsight Tools automatikus-észleli, hogy az E (fx) clipse beépülő modul telepítve, vagy nem. Kattintson a **OK** folytatja, és kövesse az utasításokat az Eclipse piactér telepítése, majd indítsa újra az eclipse-ben.

    ![E (fx) clipse telepítése](./media/hdinsight-apache-spark-eclipse-tool-plugin/auto-install-efxclipse.png)

3. Nyissa meg a feladat nézetet a **feladatok** csomópont. A jobb oldali ablaktáblában a **Spark feladat megtekintése** lap megjeleníti a fürtön futó összes alkalmazást. Kattintson a nevére, amelynek meg szeretné tekinteni a további részleteket az alkalmazás.

    ![Az alkalmazás részletei](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)
4. Ha a feladat grafikonon mutat, alapszintű futó feladat adatait jeleníti meg. A feladatgrafikon kattintva látható szakaszból graph és adatait, amely minden feladatot hoz létre.

    ![Feladat szakasz részletei](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

5. A gyakran használt naplókat, illesztőprogram Stderr, illesztőprogram Stdout és Directory adatai jelennek meg a **napló** fülre.

    ![Naplófájl részletei](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-log-info.png)
6. A Spark-előzmények felhasználói felület és a YARN felhasználói felületen (az alkalmazás szintjén) az ablak tetején a megfelelő hivatkozásra kattintva is megnyithatja.

### <a name="access-the-storage-container-for-the-cluster"></a>A tároló hozzáférés a fürthöz
1. Azure-kezelővel, bontsa ki a **HDInsight** gyökércsomópont elérhető HDInsight Spark-fürtjei listájának megjelenítéséhez.
2. Bontsa ki a fürt nevét, a tárfiók, és az alapértelmezett tároló, a fürt számára.
   
    ![Storage-fiókot és az alapértelmezett tároló](./media/hdinsight-apache-spark-eclipse-tool-plugin/view-explorer-5.png)
3. Kattintson a fürthöz rendelt tárolási tároló nevére. A jobb oldali ablaktáblában kattintson duplán a **HVACOut** mappa. Nyissa meg az egyik a **rész -** fájlok, hogy az alkalmazás kimenete.

### <a name="access-the-spark-history-server"></a>A Spark-előzmények kiszolgáló elérhető
1. Az Azure-kezelővel, kattintson a jobb gombbal a Spark-fürt nevét, majd válassza ki **nyissa meg a Spark feladatelőzmények felhasználói felület**. Amikor a rendszer kéri, adja meg a fürt rendszergazdai hitelesítő adatokat. Ezek a fürt kiépítése során van megadva.
2. A Spark előzmények server Irányítópult segítségével az alkalmazás neve keresse meg az alkalmazás csak futása befejeződött. Az előző kódban használatával beállíthatja az alkalmazás neve `val conf = new SparkConf().setAppName("MyClusterApp")`. Emiatt a Spark alkalmazásnév lett **MyClusterApp**.

### <a name="start-the-ambari-portal"></a>Indítsa el az Ambari portálon
1. Az Azure-kezelővel, kattintson a jobb gombbal a Spark-fürt nevét, majd válassza ki **nyitott fürt Management Portal (Ambari)**. 
2. Amikor a rendszer kéri, adja meg a fürt rendszergazdai hitelesítő adatokat. Ezek a fürt kiépítése során van megadva.

### <a name="manage-azure-subscriptions"></a>Az Azure-előfizetések kezelése
Alapértelmezés szerint a HDInsight Tools az Eclipse Azure eszköztára a Spark-fürtök az összes Azure-előfizetések sorolja fel. Ha szükséges, megadhatja az előfizetéseket, amelyekhez a fürt eléréséhez. 

1. Az Azure-kezelővel, kattintson a jobb gombbal a **Azure** gyökércsomópont, és kattintson a **előfizetések kezelése oldalt**. 
2. A párbeszédpanelen jelölőnégyzetek az előfizetés, amelyet szeretne elérni, és kattintson a **Bezárás**. Is **Kijelentkezés** Ha azt szeretné, hogy kijelentkezik, az Azure-előfizetéshez.

## <a name="run-a-spark-scala-application-locally"></a>A Spark Scala alkalmazás helyileg történő futtatása
Azure eszköztára eclipse-ben a HDInsight Tools használatával Spark Scala-alkalmazások helyi futtatása a munkaállomáson. Általában ezek az alkalmazások nem szükséges hozzáférési jogot a fürterőforrások például a tárolót, és futtatja, és helyben tesztelheti őket.

### <a name="prerequisite"></a>Előfeltétel
A helyi Spark Scala-alkalmazások Windows rendszerű számítógépeken futtatása, közben kivétel kaphat, ahogy [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356). Ez a kivétel akkor fordul elő, mert **WinUtils.exe** Windows hiányzik. 

Ez a hiba elhárításához kell [töltse le a végrehajtható fájl](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) egy olyan helyre, például a **C:\WinUtils\bin**. Majd adja hozzá a következő környezeti változó **HADOOP_HOME** és a változó értékét állíthatja be **C\WinUtils**.

### <a name="run-a-local-spark-scala-application"></a>A helyi Spark Scala-alkalmazások futtatása
1. Indítsa el az Eclipse, és hozzon létre egy projektet. Az a **új projekt** párbeszédpanelen hajtsa végre a következők közül választhat, és kattintson **következő**.
   
   * A bal oldali panelen válassza ki a **HDInsight**.
   * A jobb oldali ablaktáblában jelölje ki a **a Spark on HDInsight helyi futtatása minta (Scala)**.

    ![A New Project (Új projekt) párbeszédpanel](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run.png)
2. Arra, hogy a projekt részleteit, hajtsa végre a 3-6. lépéseket a korábbi szakaszában [állítson be egy Spark Scala-projektjét, amely egy HDInsight Spark-fürt](#set-up-a-spark-scala-project-for-an-hdinsight-spark cluster).
3. A sablon hozzáadása egy mintakód (**LogQuery**) alatt a **src** mappát, amelyet a számítógépen helyileg futtathat.
   
    ![LogQuery helye](./media/hdinsight-apache-spark-eclipse-tool-plugin/local-app.png)
4. Kattintson a jobb gombbal a **LogQuery** alkalmazás, pont **futtató**, és kattintson a **1 Scala alkalmazás**. Az ehhez a hasonló kimenetet fog látni a **konzol** fül:
   
   ![Spark alkalmazás helyi futtatás eredménye](./media/hdinsight-apache-spark-eclipse-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="faq"></a>GYIK
Azure Data Lake Store kérelmet elküldéséhez válassza **interaktív** mód az Azure bejelentkezési folyamat során. Ha **automatikus** mód, hibaüzenetet kaphat.

![interaktív bejelentkezés](./media/hdinsight-apache-spark-eclipse-tool-plugin/interactive-authentication.png)

Most azt feloldotta azt. Az Azure Data Lake fürt elküldeni az alkalmazás egyik bejelentkezési módszer kiválasztása

## <a name="feedback-and-known-issues"></a>Visszajelzések és ismert problémák
Spark kimenetek közvetlenül megtekintése jelenleg nem támogatott.

Ha javaslata vagy visszajelzést, vagy ha az eszköz használata során felmerülő problémákat, nyugodtan küldjön egy e-mailt hdivstool@microsoft.com.

## <a name="seealso"></a>Lásd még:
* [Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Forgatókönyvek
* [Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel](hdinsight-apache-spark-use-bi-tools.md)
* [Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark és Machine Learning: A Spark on HDInsight használata az élelmiszervizsgálati eredmények előrejelzésére](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására](hdinsight-apache-spark-eventhub-streaming.md)
* [A webhelynapló elemzése a Spark on HDInsight használatával](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Létrehozása és alkalmazások futtatása
* [Önálló alkalmazás létrehozása a Scala használatával](hdinsight-apache-spark-create-standalone-application.md)
* [Feladatok távoli futtatása Spark-fürtön a Livy használatával](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Eszközök és bővítmények
* [Az intellij-t Azure eszközkészlet segítségével hozza létre, és küldje el a Spark Scala-alkalmazások](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Az intellij-t Azure eszközkészlet segítségével VPN-en keresztül távolról Spark-alkalmazások](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Az intellij-t Azure eszközkészlet segítségével SSH keresztül távolról Spark-alkalmazások](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Használja a HDInsight Tools for IntelliJ a Hortonworks védőfal](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Zeppelin notebookok használata Spark-fürttel HDInsighton](hdinsight-apache-spark-zeppelin-notebook.md)
* [Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Külső csomagok használata Jupyter notebookokkal](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [A Jupyter telepítése a számítógépre, majd csatlakozás egy HDInsight Spark-fürthöz](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a>Erőforrások kezelése
* [Apache Spark-fürt erőforrásainak kezelése az Azure HDInsightban](hdinsight-apache-spark-resource-manager.md)
* [Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban](hdinsight-apache-spark-job-debugging.md)

