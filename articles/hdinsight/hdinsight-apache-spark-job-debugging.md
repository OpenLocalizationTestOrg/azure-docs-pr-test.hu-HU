---
title: "aaaDebug Apache Spark on Azure HDInsight futó feladatok |} Microsoft Docs"
description: "YARN felhasználói felületen, a Spark felhasználói felület és a Spark-előzmények server tootrack és hibakeresési futó feladatok az Azure HDInsight Spark-fürt használatára"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 59af05a7-2bd9-44b0-b55f-2438d294198b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: 33d352a5773920735aa4e5e8532b78122f381377
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debug-apache-spark-jobs-running-on-azure-hdinsight"></a>Azure hdinsighton futó Apache Spark feladatok hibakeresése

Ebben a cikkben megtudhatja, hogyan tootrack és hibakeresési hello YARN felhasználói felületen, Spark felhasználói felületén, HDInsight-fürtök futó feladatok Spark és Spark előzmények Server hello. Ebben a cikkben azt feladatot indít el a Spark elérhető Jegyzetfüzet használata Spark-fürt hello **gépi tanulás: prediktív elemzési étele ellenőrző adatokat, és MLLib a**. Hello lépések végrehajtásával tootrack beküldött bármely más megközelítéssel is, például egy alkalmazás alábbi **spark-nyújt**.

## <a name="prerequisites"></a>Előfeltételek
Hello következő kell rendelkeznie:

* Azure-előfizetés. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* A HDInsight az Apache Spark-fürt. Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).
* Meg kell kezdeni hello notebook futtató  **[gépi tanulás: prediktív elemzési a étele ellenőrző adatokat, és MLLib](hdinsight-apache-spark-machine-learning-mllib-ipython.md)**. Útmutatást toorun jegyzetfüzet, hajtsa végre hello hivatkozásra.  

## <a name="track-an-application-in-hello-yarn-ui"></a>Egy alkalmazás a YARN felhasználói felületen hello nyomon követése
1. Indítsa el a hello YARN felhasználói felületen. Hello-fürt panelén kattintson **fürt irányítópult**, és kattintson a **YARN**.
   
    ![Indítsa el a YARN felhasználói felületen](./media/hdinsight-apache-spark-job-debugging/launch-yarn-ui.png)
   
   > [!TIP]
   > Másik lehetőségként is elindíthatja a hello YARN felhasználói felületen a hello Ambari felhasználói felület. toolaunch hello Ambari felhasználói felületén, a hello-fürt panelén kattintson **fürt irányítópult**, és kattintson a **HDInsight fürt irányítópult**. A hello Ambari felhasználói felület, kattintson az **YARN**, kattintson a **Gyorshivatkozások**, kattintson hello aktív erőforrás-kezelő, majd **erőforrás-kezelő felhasználói felületén**.    
   > 
   > 
2. Hello Spark feladat Jupyter notebookok használatával indította, mert hello alkalmazásnak hello neve **remotesparkmagics** (Ez az minden olyan alkalmazásnál, amely hello notebookok való indítása hello név). Kattintson a hello Alkalmazásazonosító elleni hello alkalmazás neve tooget hello feladat további információt. Ekkor elindul a hello alkalmazás megtekintése.
   
    ![Keresés a külső alkalmazás azonosítója](./media/hdinsight-apache-spark-job-debugging/find-application-id.png)
   
    Az ilyen alkalmazások esetében a hello Jupyter notebookokból származó működését, hello állapota mindig **futtató** amíg hello notebook kilép.
3. Hello alkalmazás nézetből részletezve további toofind hello alkalmazás és hello naplók (stdout/stderr) társított hello tárolók ki. Külső felhasználói felület hello indítsa el megfelelő toohello linking hello kattintva **követési URL-cím**lent látható módon. 
   
    ![Tároló naplók letöltése](./media/hdinsight-apache-spark-job-debugging/download-container-logs.png)

## <a name="track-an-application-in-hello-spark-ui"></a>Nyomon követheti az alkalmazás hello Spark felhasználói felület
Hello Spark felhasználói felület, a részletezhető le, hogy a korábbi lépések hello alkalmazás által indított vannak hello Spark feladatok.

1. toolaunch hello Spark UI hello alkalmazás nézetből elleni hello hello hivatkozásra **követési URL-cím**, a fenti hello képernyőfelvételen látható módon. Az összes futó hello Jupyter notebook hello alkalmazás működését hello Spark feladatok tekintheti meg.
   
    ![A Spark-feladatok megtekintése](./media/hdinsight-apache-spark-job-debugging/view-spark-jobs.png)
2. Kattintson a hello **végrehajtója** lapon minden egyes végrehajtó toosee adatfeldolgozás és -tárolás adatait. Hello hívási verem hello kattintva is lekérhet **szál Dump** hivatkozásra.
   
    ![Spark végrehajtója megtekintése](./media/hdinsight-apache-spark-job-debugging/view-spark-executors.png)
3. Kattintson a hello **szakaszból** toosee hello szakaszból hello alkalmazáshoz kapcsolódó lapon.
   
    ![A Spark-állapotok megjelenítése](./media/hdinsight-apache-spark-job-debugging/view-spark-stages.png)
   
    Minden szakaszhoz rendelkezhet több feladatot, amely találhatja meg a végrehajtási statisztika, például alább látható módon.
   
    ![A Spark-állapotok megjelenítése](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-details.png) 
4. Hello szakasz részleteit megjelenítő oldalon DAG képi megjelenítés is elindíthatja. Bontsa ki a hello **DAG képi megjelenítés** hivatkozás hello lapon hello tetején alább látható módon.
   
    ![Spark szakaszból DAG képi megjelenítés megtekintése](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-dag-visualization.png)
   
    DAG vagy közvetlen Aclyic Graph jelöli hello különböző szakaszaiban hello alkalmazásban. Minden egyes kék mező hello grafikonon egy Spark művelet meghívása hello alkalmazásból jelöli.
5. Hello szakasz részleteit megjelenítő oldalon is elindíthatja a hello alkalmazás idősor nézetének megnyitására. Bontsa ki a hello **esemény ütemterv** hivatkozás hello lapon hello tetején alább látható módon.
   
    ![Spark szakaszból esemény ütemterv megtekintése](./media/hdinsight-apache-spark-job-debugging/view-spark-stages-event-timeline.png)
   
    Ez hello Spark eseményeket ütemterv hello formában jeleníti meg. hello Ütemterv nézet áll rendelkezésre három szinten különböző feladatokat, a feladatok és egy szakaszon belül. a fenti kép hello hello idősor nézetének megnyitására adott szakaszában rögzíti.
   
   > [!TIP]
   > Ha hello **engedélyezése a Nagyítás** jelölőnégyzetet, görgetve bal és jobb között hello idősor nézetének megnyitására.
   > 
   > 
6. A hello Spark felhasználói felületének más lapjaira hello Spark-példányban is hasznos információt nyújtanak.
   
   * Tárolási lap – Ha az alkalmazás létrehoz egy RDDs, akkor további információt talál hello tárolás fülön lévőket.
   * Környezet lap – ezen a lapon számos hasznos információt a Spark-példányban, például a hello 
     * Scala verzió
     * Hello fürt társított Eseménynapló könyvtár
     * Hello alkalmazás végrehajtó magok száma
     * Stb.

## <a name="find-information-about-completed-jobs-using-hello-spark-history-server"></a>Befejezett feladatokhoz hello Spark előzmények Server használatával kapcsolatos információk megkereséséhez
Ha egy feladat befejeződött, hello feladat hello információ hello Spark előzmények Server őrzi meg van.

1. toolaunch hello Spark előzmények Server, a hello-fürt panelén kattintson **fürt irányítópult**, és kattintson a **Spark előzmények Server**.
   
    ![Indítsa el a Spark előzmények kiszolgáló](./media/hdinsight-apache-spark-job-debugging/launch-spark-history-server.png)
   
   > [!TIP]
   > Másik lehetőségként is elindíthatja a hello Spark előzmények kiszolgáló felhasználói Felületét a hello Ambari felhasználói felület. toolaunch hello Ambari felhasználói felületén, a hello-fürt panelén kattintson **fürt irányítópult**, és kattintson a **HDInsight fürt irányítópult**. A hello Ambari felhasználói felület, kattintson az **Spark**, kattintson a **Gyorshivatkozások**, és kattintson a **Spark előzmények kiszolgáló felhasználói felülete**.
   > 
   > 
2. Minden felsorolt hello befejeződött alkalmazásokkal jelenik meg. Kattintson az alkalmazás azonosítója toodrill le egy alkalmazásba, további információért.
   
    ![Indítsa el a Spark előzmények kiszolgáló](./media/hdinsight-apache-spark-job-debugging/view-completed-applications.png)

## <a name="see-also"></a>Lásd még:
*  [Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése](hdinsight-apache-spark-resource-manager.md)

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


