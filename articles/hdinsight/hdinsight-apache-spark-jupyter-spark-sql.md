---
title: "aaaCreate az Apache Spark on Azure hdinsight fürt |} Microsoft Docs"
description: "Hogyan toocreate az Apache Spark on hdinsight fürt a HDInsight Spark gyorsindítási."
keywords: "spark gyorsútmutató,interaktív spark,interaktív lekérdezés,hdinsight spark,azure spark"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 91f41e6a-d463-4eb4-83ef-7bbb1f4556cc
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: 002f71b3cd4fb315d4a556cebc9263026515ec4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-apache-spark-cluster-in-azure-hdinsight"></a>Apache Spark-fürt létrehozása az Azure HDInsightban

Ebből a cikkből megismerheti, hogyan toocreate az Apache Spark on Azure hdinsight fürt. A Spark on HDInsight további információiért lásd: [Áttekintés: Apache Spark on Azure HDInsight](hdinsight-apache-spark-overview.md).

   ![Gyors üzembe helyezés diagram lépéseket toocreate Apache Spark-fürt leíró on Azure HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-quickstart-interactive-spark-query-flow.png "Spark gyors üzembe helyezés, Apache Spark on HDInsight használatával. Szemléltetett lépések: fürt létrehozása; interaktív Spark-lekérdezés futtatása")

## <a name="prerequisites"></a>Előfeltételek

* **Azure-előfizetés**. Az oktatóanyag elindításához Azure-előfizetéssel kell rendelkeznie. Lásd: [Ingyenes Azure-fiók létrehozása még ma](https://azure.microsoft.com/free).

## <a name="create-hdinsight-spark-cluster"></a>HDInsight Spark-fürt létrehozása

Ebben a szakaszban egy HDInsight Spark-fürtöt hozhat létre egy [Azure Resource Manager-sablonnal](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/). Egyéb fürtlétrehozási módszerek: [HDInsight-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md).

1. Kattintson a következő kép tooopen hello sablon hello Azure-portálon hello.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-jupyter-spark-sql/deploy-to-azure.png" alt="Deploy tooAzure"></a>

2. Adja meg a következő értékek hello:

    ![HDInsight Spark-fürt létrehozása egy Azure Resource Manager-sablonnal](./media/hdinsight-apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Spark-fürt létrehozása a HDInsightban egy Azure Resource Manager-sablonnal")

    * **Előfizetés**: Válassza ki a fürthöz használni kívánt Azure-előfizetést.
    * **Erőforráscsoport**: Hozzon létre egy erőforráscsoportot, vagy válasszon ki egy meglévőt. Erőforráscsoport van használt toomanage a projektek Azure-erőforrások.
    * **Hely**: hello erőforráscsoport helyének kiválasztására. hello sablon hello fürt létrehozásának, valamint mint hello alapértelmezett fürttároló ezen a helyen használja.
    * **ClusterName**: Adjon meg egy nevet, amelyet az toocreate hello HDInsight-fürthöz.
    * **Spark-verziójú**: válasszon **2.0** , amelyet az hello fürtön tooinstall hello verzióként.
    * **A fürt bejelentkezési nevet és jelszót**: hello alapértelmezett bejelentkezési név az admin.
    * **SSH-felhasználónév és -jelszó**.

   Jegyezze fel ezeket az értékeket.  Már szükség hello oktatóanyag későbbi részében.

3. Válassza ki **toohello feltételek és kikötések fenti elfogadom**, jelölje be **PIN-kód toodashboard**, és kattintson a **beszerzési**. Ekkor egy új csempe jelenik meg Submitting deployment for Template deployment (Üzemelő példány elküldése sablon üzemelő példányhoz) címmel. Körülbelül 20 percet toocreate hello fürt vesz igénybe.

Ha a HDInsight-fürtök létrehozása problémát tapasztal, lehet, hogy nem rendelkezik megfelelő engedélyekkel toodo hello így. További információért tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).

> [!NOTE]
> Ebben a cikkben létrehoz egy Spark-fürt által használt [Azure Storage Blobs, hello tárolási fürt](hdinsight-hadoop-use-blob-storage.md). Létrehozhat egy Spark-fürt által használt [Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md) hello alapértelmezett tárolóként. Útmutatás: [Create an HDInsight cluster with Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md) (HDInsight-fürt létrehozása a Data Lake Store-ral).
>
>

## <a name="run-a-hive-query-using-spark-sql"></a>Hive-lekérdezés futtatása a Spark SQL-lel

A HDInsight Spark-fürt konfigurált Jupyter notebook használatakor egy előre definiált kap `sqlContext` használható toorun Hive-lekérdezések Spark SQL használatával. Ebben a szakaszban megismerheti, hogyan toostart Jupyter notebook, majd futtassa az alapszintű Hive-lekérdezések.

1. Nyissa meg hello [Azure-portálon](https://portal.azure.com/).

2. Ha toopin hello fürt toohello irányítópult választotta, kattintson a hello fürt csempe hello irányítópult toolaunch hello fürt paneljén.

    Ha nem volt rögzíti hello fürt toohello irányítópult, hello bal oldali ablaktáblában kattintson **a HDInsight-fürtök**, majd kattintson a létrehozott hello fürt.

3. A **Gyorshivatkozások** menüben kattintson a **Fürt irányítópultjai** lehetőségre, majd a **Jupyter Notebook** elemre. Ha a rendszer kéri, adja meg hello fürt hello rendszergazdai hitelesítő adataival.

   ![Nyissa meg Jupyter notebook toorun interaktív Spark SQL-lekérdezés](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "nyitott Jupyter notebook toorun interaktív Spark SQL-lekérdezés")

   > [!NOTE]
   > A fürt URL-címet a böngészőben a következő megnyitásakor hello által hello Jupyter notebook is elérhetők. Cserélje le **CLUSTERNAME** hello néven a fürt:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Hozzon létre egy notebookot. Kattintson a **New** (Új), majd a **PySpark** elemre.

   ![A Jupyter notebook toorun interaktív Spark SQL-lekérdezés létrehozása](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "a Jupyter notebook toorun interaktív Spark SQL-lekérdezés létrehozása")

   Új notebook létrejött, és hello nevű Untitled(Untitled.pynb).

4. Hello notebook neve hello tetején kattintson, és adja meg egy rövid nevet, ha szeretné.

    ![Adjon meg egy nevet hello Jupter notebook toorun interaktív Spark-lekérdezést](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "adjon meg egy nevet hello Jupter notebook toorun interaktív Spark lekérdezése")

5.  Beillesztés hello következő kód egy üres cellába, és nyomja le az **SHIFT + ENTER** toorun hello kódot. Az alábbi kód hello `%%sql` (hívott hello sql magic) közli a Jupyter notebook toouse hello beállított `sqlContext` toorun hello Hive-lekérdezést. hello lekérdezés hello első 10 sorok egy táblából struktúra (**hivesampletable**), amely alapértelmezés szerint mindig elérhető az összes HDInsight-fürtök.

        %%sql
        SELECT * FROM hivesampletable LIMIT 10

    ![Hive-lekérdezés a HDInsight Sparkban](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "Hive-lekérdezés a HDInsight Sparkban")

    További információ a hello `%%sql` magic és hello beállított környezeteket, lásd: [Jupyter kernelek a HDInsight-fürtök rendelkezésre](hdinsight-apache-spark-jupyter-notebook-kernels.md).

    > [!NOTE]
    > Minden alkalommal, amikor lekérdezést futtat a Jupyter, a webböngésző ablakának címsorában látható egy **(foglalt)** állapot hello notebook neve mellett. Egy teli kör következő toohello is látni **PySpark** hello jobb felső sarokban lévő szöveg. Hello feladat befejezése után tooa jel üres körre változik.
    >
    >
    
6. üdvözlő képernyőt tooshow hello lekérdezés kimeneti kell frissíteni.

    ![Hive-lekérdezés kimenete a HDInsight Sparkban](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "Hive-lekérdezés kimenete a HDInsight Sparkban")

7. Hello notebook toorelease hello fürterőforrások leállítása hello alkalmazást futtató befejezése után. toodo Igen, a hello **fájl** hello notebook menüjében kattintson **zárja be és Halt**.

8. Ha toocomplete hello lépések egy későbbi időpontban, ellenőrizze, hogy ez a cikk létrehozott hello HDInsight-fürt törlése. 

    [!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-step"></a>Következő lépés 

Ebben a cikkben megtanulta, hogyan toocreate egy HDInsight Spark fürt és a Futtatás alapvető Spark SQL-lekérdezésben. Előzetes toohello hogyan toouse egy HDInsight Spark fürt tovább cikk toolearn toorun interaktív lekérdezések a mintaadatokat.

> [!div class="nextstepaction"]
>[Interaktív lekérdezések futtatása HDInsight Spark-fürtön](hdinsight-apache-spark-load-data-run-query.md)



