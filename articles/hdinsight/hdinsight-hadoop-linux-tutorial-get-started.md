---
title: "aaaGet Hadoop és az Azure HDInsight Hive használatába |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate HDInsight-fürtök és a Hive lekérdezés adatok."
keywords: "hadoop első lépések, hadoop linux, hadoop rövid útmutató, hive első lépések, hive rövid útmutató"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6a12ed4c-9d49-4990-abf5-0a79fdfca459
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/23/2017
ms.author: jgao
ms.openlocfilehash: 3d96d78121200ebda3626dd2c3885e3ddacd546d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="hadoop-tutorial-get-started-using-hadoop-in-hdinsight"></a>Hadoop oktatóanyag: A Hadoop első lépései a HDInsightban

Megtudhatja, hogyan toocreate [Hadoop](http://hadoop.apache.org/) , és hogyan toorun Hive feladat HDInsight fürtöket. [Apache Hive](https://hive.apache.org/) hello a hello Hadoop ökoszisztémájának legnépszerűbb összetevője. A HDInsight jelenleg [7 különböző fürttípussal érhető el](hdinsight-hadoop-introduction.md#overview). Minden egyes fürttípus más és más összetevőket támogat. A Hive-ot minden fürttípus támogatja. A Hdinsightban támogatott összetevők listáját lásd: [What's new in HDInsight által biztosított hello Hadoop-fürt verziók?](hdinsight-component-versioning.md)  

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Előfeltételek
Az oktatóanyag elindításának feltétele:

* **Azure-előfizetés**: egy ingyenes egy hónapos próbafiók, toocreate Tallózás túl[azure.microsoft.com/free](https://azure.microsoft.com/free).

## <a name="create-cluster"></a>Fürt létrehozása

A legtöbb Hadoop-feladat kötegelt feladat. Hozzon létre egy fürtöt, futtat néhány feladatot, és törölje a hello fürt. Ebben a szakaszban egy Hadoop-fürtöt hozhat létre a HDInsightban egy [Azure Resource Manager-sablonnal](../azure-resource-manager/resource-group-template-deploy.md). Nem kell a Resource Manager-sablonok használatára vonatkozó tapasztalattal rendelkeznie az oktatóanyag követéséhez. Egyéb Fürtlétrehozási módszerekhez és ebben az oktatóanyagban használt hello tulajdonságok ismertetése, tanulmányozza a [HDInsight-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md). Hello választó hello felül a hello lap toochoose a fürt létrehozása beállításokat használják.

Ebben az oktatóanyagban használt hello Resource Manager sablon található [GitHub](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/). 

1. Kattintson a következő kép toosign tooAzure a és a nyitott hello Resource Manager sablon hello Azure-portálon hello. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-ssh-password%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hadoop-linux-tutorial-get-started/deploy-to-azure.png" alt="Deploy tooAzure"></a>
2. Adja meg, vagy válassza ki a következő értékek hello:
   
    ![HDInsight Linux első lépései a Resource Manager-sablon a portálon](./media/hdinsight-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-arm-template-on-portal.png "telepítése Hadoop-fürt HDInsigut használatával hello Azure-portál és a resource manager-sablon").
   
    * **Előfizetés**: Válassza ki az Azure-előfizetést.
    * **Erőforráscsoport**: Hozzon létre egy erőforráscsoportot, vagy válasszon ki egy már meglévőt.  Az erőforráscsoport az Azure összetevőit tartalmazó tároló.  Ebben az esetben hello erőforráscsoport hello HDInsight-fürt és a függő hello Azure Storage-fiók. 
    * **Hely**: Válasszon egy Azure-beli hely, ahová toocreate a fürt.  Válassza ki a hely szorosabb tooyou jobb teljesítmény érdekében. 
    * **Fürt típusa**: Válassza a **hadoop** lehetőséget ehhez az oktatóanyaghoz.
    * **A fürt neve**: hello Hadoop-fürt nevét adja meg.
    * **A fürt bejelentkezési nevet és jelszót**: hello alapértelmezett bejelentkezési név az **admin**.
    * **SSH-felhasználónév és jelszó**: hello alapértelmezett felhasználónév az **sshuser**.  Ezt át lehet nevezni. 
     
    Néhány tulajdonság hello sablonban szoftveresen kötött volt.  Ezek az értékek hello sablon alapján konfigurálhatja.

    * **Hely**: hello hello fürt helyét és hello függő storage-fiók megosztás hello hello erőforráscsoport és ugyanazon a helyen.
    * **Fürt verziója**: 3.5
    * **Operációs rendszer típusa**: Linux
    * **Munkavégző csomópontok száma**: 2

     Minden egyes fürt egy [Azure Storage-fióktól](hdinsight-hadoop-use-blob-storage.md) vagy egy [Azure Data Lake-fióktól](hdinsight-hadoop-use-data-lake-store.md) függ. Hello alapértelmezett tárfiók kezeli. HDInsight-fürt és az alapértelmezett tárfióknak közös elhelyezése szükséges a hello azonos Azure-régiót. Fürtök törlése nem érinti hello tárfiók. 
     
     További magyarázat ezekről a tulajdonságokról: [Hadoop-fürtök létrehozása a HDInsightban](hdinsight-hadoop-provision-linux-clusters.md).

3. Válassza ki **toohello feltételek és kikötések fenti elfogadom** és **PIN-kód toodashboard**, és kattintson a **beszerzési**. Ekkor megjelenik egy új csempe jelenik **telepítése sablon-üzembehelyezés** hello portál irányítópultján. Vesz igénybe körülbelül 20 percet toocreate egy fürt. Hello fürt létrehozása után hello csempe felirata hello az erőforráscsoport neve módosult toohello megadott. És hello portal hello erőforráscsoport automatikusan megnyílik egy új panelen. Hello fürt és a felsorolt hello alapértelmezett tárolási tekintheti meg.
   
    ![A HDInsight (Linux) használatának első lépései – erőforráscsoport](./media/hdinsight-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-resource-group.png "Azure HDInsight-alapú fürt erőforráscsoportja").

4. Kattintson a hello fürt neve tooopen hello fürt egy új panelen.

   ![fürtbeállítások](./media/hdinsight-hadoop-linux-tutorial-get-started/hdinsight-linux-get-started-cluster-settings.png "HDInsight-fürttulajdonságok")


## <a name="run-hive-queries"></a>Hive-lekérdezések futtatása
[Apache Hive](hdinsight-use-hive.md) hello használt hdinsight legnépszerűbb összetevője. Számos módon toorun Hive-feladatokat a Hdinsightban. Ebben az oktatóanyagban hello hello portál Ambari Hive nézete használja. A Hive-feladatok egyéb küldési módjaiért lásd: [Use Hive in HDInsight](hdinsight-use-hive.md) (A Hive használata a HDInsightban).

1. Kattintson az előző képernyőképet hello, **fürt irányítópult**, és kattintson a **HDInsight fürt irányítópult**.  Tallózással is kikeresheti is túl **https://&lt;ClusterName >. azurehdinsight.net**, ahol &lt;ClusterName > hello előző szakasz tooopen Ambari létrehozott hello fürt.
2. Adja meg a hello Hadoop-felhasználónevet és jelszót, amely hello előző szakaszban. hello alapértelmezett felhasználónév az **admin**.
3. Nyissa meg **Hive View** látható módon a következő képernyőkép hello:
   
    ![Az Ambari-nézetek kiválasztása](./media/hdinsight-hadoop-linux-tutorial-get-started/selecthiveview.png "HDInsight Hive Viewer menü").
4. A hello **Lekérdezésszerkesztő** hello lap, Beillesztés hello HiveQL utasítások hello munkalapra a következő szakaszban:
   
        SHOW TABLES;
   
   > [!NOTE]
   > A pontosvessző megadása a Hive-nál kötelező.       
   > 
   > 
5. Kattintson az **Execute** (Végrehajtás) parancsra. A **lekérdezési folyamat eredményei** szakasz hello Lekérdezésszerkesztő alatt jelennek meg, és meg kell hello feladat információinak megjelenítéséhez. 
   
    Miután hello lekérdezés befejeződött, hello **lekérdezési folyamat eredményei** szakasz hello művelet hello eredményeit jeleníti meg. Látni fog egy **hivesampletable** nevű táblát. A Hive mintatáblával az összes hello HDInsight-fürtökkel.
   
    ![HDInsight Hive-nézetek](./media/hdinsight-hadoop-linux-tutorial-get-started/hiveview.png "HDInsight Hive View lekérdezésszerkesztő").
6. Ismételje meg a 4. lépés és 5. lépés toorun hello a következő lekérdezést:
   
        SELECT * FROM hivesampletable;
   
   > [!TIP]
   > Megjegyzés: hello **-eredményeket menteni** hello a bal felső hello legördülő **lekérdezési folyamat eredményei** szakaszban, használhatja a tooeither letöltés hello eredményeit, vagy menti tooHDInsight tároló CSV-fájlként.
   > 
   > 
7. Kattintson a **előzmények** tooget hello feladatokra.

Egy Hive-feladat befejezését követően is [hello eredmények tooAzure SQL database vagy az SQL Server-adatbázis exportálása](hdinsight-use-sqoop-mac-linux.md), is [Excel használatával hello eredményeinek képi megjelenítése](hdinsight-connect-excel-power-query.md). Hdinsight Hive használatával kapcsolatos további információkért lásd: [használata Hive és a HiveQL HDInsight tooanalyze Apache log4j mintafájl a hadooppal](hdinsight-use-hive.md).

## <a name="clean-up-hello-tutorial"></a>Hello oktatóanyag tisztítása
Hello az oktatóanyag befejezése után érdemes lehet toodelete hello fürt. A HDInsight az Azure Storage szolgáltatásban tárolja az adatokat, így biztonságosan törölhet olyan fürtöket, amelyek nincsenek használatban. Ráadásul a HDInsight-fürtök akkor is díjkötelesek, amikor éppen nincsenek használatban. Mivel hello díjak hello fürt sokszor több mint hello a tárolási, érdemes gazdasági toodelete fürtök amikor nincsenek használatban. 

> [!NOTE]
> Használatával [Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md), akkor az igény szerinti HDInsight-fürtök létrehozásához, és beállíthatja egy TimeToLive beállítás túl az automatikusan hello fürtök törlése. 
> 
> 

**toodelete hello fürt és/vagy hello alapértelmezett tárfiók**

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello portál irányítópultján kattintson hello csempe, amely az erőforráscsoport neve hello hello fürt létrehozásakor használt.
3. Kattintson a **törlése** az erőforrás panel toodelete hello, tartalmazó erőforráscsoport hello fürt és az alapértelmezett tárfiók hello; hello, vagy kattintson a hello fürt neve a hello **erőforrások** csempén, majd **Törlése** hello fürt paneljén. Megjegyzés: hello erőforráscsoport törlése hello tárfiók törlése. Ha azt szeretné, hogy tookeep hello tárfiókot, válassza ki a toodelete hello fürt.

## <a name="troubleshoot"></a>Hibaelhárítás

Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóprogramban megtanulhatta, hogyan toocreate egy Linux-alapú HDInsight-fürt Resource Manager-sablon használatával, és hogyan tooperform alapszintű Hive-lekérdezéseket.

További információk a HDInsight az adatok elemzése toolearn lásd: a következő cikkek hello:

* További információk a hdinsight eszközzel, hogyan tooperform Hive lekérdezi a Visual Studio eszközből, beleértve a Hive eszközzel toolearn lásd [használata a HDInsight Hive][hdinsight-use-hive].
* Pig kapcsolatos toolearn, nyelv használt tootransform adatokat, lásd: [a Pig használata a hdinsight eszközzel][hdinsight-use-pig].
* toolearn MapReduce, egy módszer toowrite, Hadoopon adatokat feldolgozó programok kapcsolatban lásd: [és a HDInsight együttes használata MapReduce][hdinsight-use-mapreduce].
* toolearn hello HDInsight Tools for Visual Studio tooanalyze adatok, használatával kapcsolatban lásd: [első lépések a Visual Studio Hadoop tools for HDInsight használatával](hdinsight-hadoop-visual-studio-tools-get-started.md).

Ha készen áll a saját adatok használata toostart és kell tooknow arról, hogyan HDInsight adattárolási módszereiről, illetve hogyan tooget adatok HDInsight, tekintse meg a hello következőt:

* További információt az Azure Storage HDInsight általi használatáról [az Azure Storage és a HDInsight együttes használatát](hdinsight-hadoop-use-blob-storage.md) ismertető cikkben talál.
* Információ tooupload adatok tooHDInsight, lásd: [töltse fel az adatok tooHDInsight][hdinsight-upload-data].

Ha azt szeretné, vagy HDInsight-fürtök kezelésével kapcsolatos további toolearn, tekintse meg a hello következőt:

* a Linux-alapú HDInsight-fürt kezeléséhez toolearn lásd: [kezelése HDInsight-fürtök Ambari használatával](hdinsight-hadoop-manage-ambari.md).
* toolearn kiválasztható egy HDInsight-fürt létrehozásakor hello beállításokról bővebben lásd: [létrehozása HDInsight Linux egyéni beállításokkal](hdinsight-hadoop-provision-linux-clusters.md).
* Ha ismeri a Linux és a Hadoop, de szeretné, hogy a hello HDInsight Hadoop kapcsolatos tooknow rögzítésen, lásd: [a HDInsight használata Linux](hdinsight-hadoop-linux-information.md). Ez a cikk többek között az alábbi információkat tartalmazza:
  
  * Például az Ambari és a WebHCat hello fürtön tárolt szolgáltatások URL-címek
  * a Hadoop-fájlok és példák a helyi fájlrendszerben hello hello helye
  * hello alapértelmezett adattár hello használata az Azure Storage (WASB) HDFS helyett.

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md


