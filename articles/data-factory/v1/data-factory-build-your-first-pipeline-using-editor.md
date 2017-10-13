---
title: "Az első data factory létrehozása (Azure Portal) | Microsoft Docs"
description: "Az oktatóanyag során létre fog hozni egy minta Azure Data Factory-folyamatot az Azure Portal Data Factory Editor eszközével."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: d5b14e9e-e358-45be-943c-5297435d402d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
robots: noindex
ms.openlocfilehash: 19f9686de9face1e53fc84eac23381eadc9fb5cd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-portal"></a>Oktatóanyag: az első Azure data factory létrehozása az Azure Portal használatával
> [!div class="op_single_selector"]
> * [Áttekintés és előfeltételek](data-factory-build-your-first-pipeline.md)
> * [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Resource Manager-sablon](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)


Ez a cikk bemutatja, hogyan hozhatja létre az első Azure-os adat-előállítóját az [Azure Portal](https://portal.azure.com/) használatával. Ha ezt az oktatóanyagot más eszközök/SDK-k használatával szeretné elvégezni, válassza ki az egyik lehetőséget a legördülő listából. 

A jelen oktatóanyagban szereplő folyamat egyetlen tevékenységet tartalmaz: ez a **HDInsight Hive-tevékenység**. A tevékenység egy hive-szkriptet futtat egy Azure HDInsight fürtön, amely a bemeneti adatokat átalakítja a kimeneti adatok előállításához. A folyamat úgy van ütemezve, hogy havonta egyszer fusson a megadott kezdő és befejező időpontok közt. 

> [!NOTE]
> Az oktatóanyagban található adatfolyamat átalakítja a bemeneti adatokat, hogy ezzel kimeneti adatokat hozzon létre. Az adatok Azure Data Factory használatával történő másolásának útmutatásáért olvassa el [az adatok Blob Storage-ból SQL Database-be történő másolását ismertető oktatóanyagot](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> Egy folyamathoz több tevékenység is tartozhat. Ezenkívül össze is fűzhet két tevékenységet (egymás után futtathatja őket), ha az egyik tevékenység kimeneti adatkészletét a másik tevékenység bemeneti adatkészleteként állítja be. További tudnivalókért lásd: [Ütemezés és végrehajtás a Data Factoryban](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).

## <a name="prerequisites"></a>Előfeltételek
1. Olvassa el [Az oktatóanyag áttekintése](data-factory-build-your-first-pipeline.md) című cikket, és hajtsa végre az **előfeltételként** felsorolt lépéseket.
2. Ez a cikk nem nyújt fogalmi áttekintést az Azure Data Factory szolgáltatásról. Javasoljuk, hogy a szolgáltatás részletes áttekintéséhez olvassa el az [Introduction to Azure Data Factory](data-factory-introduction.md) (Az Azure Data Factory bemutatása) című cikket.  

## <a name="create-data-factory"></a>Data factory létrehozása
A data factory egy vagy több folyamattal rendelkezhet. A folyamaton belül egy vagy több tevékenység lehet. Például egy olyan másolási tevékenység, amely adatokat másol egy forrásadattárból egy céladattárba, és egy HDInsight Hive-tevékenység, amely egy Hive-szkript futtatásával alakítja át a bemeneti adatokat kimeneti adatokká. Ebben a lépésben létrehozzuk a data factoryt.

1. Jelentkezzen be az [Azure portálra](https://portal.azure.com/).
2. Kattintson a **NEW** (Új) lehetőségre a bal oldali menüben, kattintson az **Data + Analytics** (Adatok + analitika) pontra, majd a **Data Factory** elemre.

   ![A Create (Létrehozás) panel](./media/data-factory-build-your-first-pipeline-using-editor/create-blade.png)
3. A **New data factory** (Új data factory) panelen adja meg a **GetStartedDF** nevet.

   ![A New data factory (Új data factory) panel](./media/data-factory-build-your-first-pipeline-using-editor/new-data-factory-blade.png)

   > [!IMPORTANT]
   > Az Azure data factory nevének **globálisan egyedinek** kell lennie. Ha a **Data factory name “GetStartedDF” is not available** (A „GetStartedDF” data factory-név nem érhető el) hibaüzenetet kapja: Módosítsa a nevet (például sajátnévGetStartedDF-re) és próbálkozzon újra a létrehozással. A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.
   >
   > A data factory neve később **DNS**-névként regisztrálható, így nyilvánosan láthatóvá válhat.
   >
   >
4. Jelölje ki azt az **Azure-előfizetést**, ahol létre szeretné hozni a data factoryt.
5. Jelöljön ki egy meglévő **erőforráscsoportot**, vagy hozzon létre egyet. Az oktatóanyag elvégzéséhez hozzon létre egy erőforráscsoportot a következő névvel: **ADFGetStartedRG**.
6. Válassza ki a Data Factory **helyét**. A legördülő listában csak a Data Factory szolgáltatás által támogatott régiók jelennek meg.
7. Válassza a **Rögzítés az irányítópulton** lehetőséget. 
8. Kattintson a **Create** (Létrehozás) elemre a **New data factory** (Új data factory) panelen.

   > [!IMPORTANT]
   > Data Factory-példány létrehozásához a [Data Factory közreműködője](../../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) szerepkör tagjának kell lennie az előfizetés/erőforráscsoport szintjén.
   >
   >
7. Az irányítópulton megjelenő csempén a következő állapotleírás látható: Data Factory üzembe helyezése.    

   ![A data factory létrehozásának állapota](./media/data-factory-build-your-first-pipeline-using-editor/creating-data-factory-image.png)
8. Gratulálunk! Sikeresen létrehozta első data factoryjét. A data factory sikeres létrehozása után megjelenik a data factory oldal, amely megjeleníti a data factory tartalmát.     

    ![A Data Factory panel](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-blade.png)

A data factoryban a folyamat létrehozása előtt először létre kell hoznia néhány Data Factory-entitást. Először társított szolgáltatásokat kell létrehoznia, amelyek adattárakat/számítási szolgáltatásokat társítanak az adattárhoz, majd bemeneti és kimeneti adatkészleteket kell meghatároznia, amelyek a társított adattárakban lévő bemeneti/kimeneti adatokat képviselik, végül létrehozhatja a folyamatot egy olyan tevékenységgel, amely ezeket az adatkészleteket használja.

## <a name="create-linked-services"></a>Társított szolgáltatások létrehozása
Ebben a lépésben az Azure Storage-fiókját és egy igény szerinti Azure HDInsight-fürtöt társít az adat-előállítóhoz. Ebben a példában az Azure Storage-fiók a bemeneti és a kimeneti adatokat tárolja a folyamathoz. A HDInsight társított szolgáltatás a mintában szereplő folyamat tevékenységében meghatározott Hive-szkriptet futtatja. Határozza meg, hogy mely [adattárat](data-factory-data-movement-activities.md)/[számítási szolgáltatásokat](data-factory-compute-linked-services.md) használja a forgatókönyvben, és társítsa ezeket a szolgáltatások a data factoryhoz társított szolgáltatások létrehozásával.  

### <a name="create-azure-storage-linked-service"></a>Azure Storage társított szolgáltatás létrehozása
Ebben a lépésben társítja az Azure Storage-fiókot a data factoryjához. A jelen oktatóanyag esetében ugyanazt az Azure Storage-fiókot fogja használni a bemeneti/kimeneti adatok és a HQL-parancsfájl tárolásához.

1. A **GetStartedDF** **DATA FACTORY** panelén kattintson az **Author and deploy** (Fejlesztés és üzembe helyezés) elemre. Ekkor meg kell jelennie a Data Factory Editornak.

   ![Az Author and deploy (Fejlesztés és üzembe helyezés) csempe](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-author-deploy.png)
2. Kattintson a **New data store** (Új adattár) elemre, és válassza az **Azure Storage** elemet.

   ![Új adattár – Azure Storage – menü](./media/data-factory-build-your-first-pipeline-using-editor/new-data-store-azure-storage-menu.png)
3. A szerkesztőben megjelenik az Azure Storage társított szolgáltatás létrehozására szolgáló JSON-parancsfájl.

   ![Azure Storage társított szolgáltatás](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. Az **account name** kifejezést cserélje az Azure Storage-fiókja nevére, az **account key** kifejezést pedig az Azure Storage-fiók kulcsára. A tárelérési kulcs lekéréséről többet is megtudhat, ha elolvassa a tárelérési kulcsok megtekintésével, másolásával és újragenerálásával kapcsolatos információkat [A tárfiók kezelése](../../storage/common/storage-create-storage-account.md#manage-your-storage-account) című részben.
5. A társított szolgáltatás üzembe helyezéséhez kattintson a parancssáv **Deploy** (Üzembe helyezés) elemére.

    ![A Deploy (Üzembe helyezés) gomb](./media/data-factory-build-your-first-pipeline-using-editor/deploy-button.png)

   A társított szolgáltatás sikeres üzembe helyezését követően megjelenik a **Draft-1** (Vázlat-1) ablak, amelynek bal oldalán, fanézetben látható az **AzureStorageLinkedService** szolgáltatás.

    ![Storage társított szolgáltatás a menüben](./media/data-factory-build-your-first-pipeline-using-editor/StorageLinkedServiceInTree.png)    

### <a name="create-azure-hdinsight-linked-service"></a>Azure HDInsight társított szolgáltatás létrehozása
Ebben a lépésben egy igény szerinti HDInsight-fürtöt társít a data factoryhoz. A HDInsight-fürtöt a rendszer automatikusan létrehozza a futásidő során, majd törli a feldolgozás befejezését követően, miután egy adott ideig tétlen volt.

1. A **Data Factory Editorban** kattintson ide: **... More** (... További), kattintson a **New compute** (Új számítási példány) elemre, majd válassza az **On-demand HDInsight cluster** (Igény szerinti HDInsight-fürt) lehetőséget.

    ![A New compute (Új számítás) elem](./media/data-factory-build-your-first-pipeline-using-editor/new-compute-menu.png)
2. Másolja és illessze be a következő kódrészletet a **Draft-1** (Vázlat-1) ablakba. A JSON-kódrészlet megadja az igény szerinti HDInsight-fürt létrehozásához használt tulajdonságokat.

    ```JSON
    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.5",
                "clusterSize": 1,
                "timeToLive": "00:05:00",
                "osType": "Linux",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }
    ```

    Az alábbi táblázat ismerteti a kódrészletben használt JSON-tulajdonságokat:

   | Tulajdonság | Leírás |
   |:--- |:--- |
   | ClusterSize |Megadja a HDInsight-fürt méretét. |
   | TimeToLive | Megadja, hogy a HDInsight-fürt mennyi ideig lehet tétlen, mielőtt törölné a rendszer. |
   | linkedServiceName | Ez megadja a HDInsight által előállított naplók tárolására szolgáló tárfiókot. |

    Vegye figyelembe a következő szempontokat:

   * A Data Factory létrehoz egy **Linux-alapú** HDInsight-fürtöt a JSON-fájllal. További információkért lásd: [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).
   * Igény szerinti HDInsight-fürt helyett **saját HDInsight-fürtöt** is használhat. További információ: [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) (HDInsight társított szolgáltatás).
   * A HDInsight-fürt létrehoz egy **alapértelmezett tárolót** a JSON-fájlban megadott blob-tárolóban (**linkedServiceName**). A fürt törlésekor a HDInsight nem törli ezt a tárolót. Ez a működésmód szándékos. Igény szerinti HDInsight társított szolgáltatás esetén a rendszer a szeletek feldolgozásakor mindig létrehoz egy HDInsight-fürtöt, kivéve, ha van meglévő élő fürt (**timeToLive**). A fürt automatikusan törlődik a feldolgozás megtörténtekor.

       Ahogy egyre több szelet lesz feldolgozva, egyre több tároló jelenik meg az Azure Blob Storage-tárban. Ha nincs szüksége rájuk a feladatokkal kapcsolatos hibaelhárításhoz, törölheti őket a tárolási költségek csökkentése érdekében. A tárolók neve a következő mintát követi: „adf**yourdatafactoryname**-**linkedservicename**-datetimestamp”. Az Azure Blob Storage-tárból olyan eszközökkel törölheti a tárolókat, mint például a [Microsoft Storage Explorer](http://storageexplorer.com/).

     További információkért lásd: [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).
3. A társított szolgáltatás üzembe helyezéséhez kattintson a parancssáv **Deploy** (Üzembe helyezés) elemére.

    ![Igény szerinti HDInsight társított szolgáltatás üzembe helyezése](./media/data-factory-build-your-first-pipeline-using-editor/ondemand-hdinsight-deploy.png)
4. Győződjön meg arról, hogy a bal oldali fanézetben megjelenik az **AzureStorageLinkedService** és a **HDInsightOnDemandLinkedService**.

    ![Fanézet a társított szolgáltatásokkal](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-linked-services.png)

## <a name="create-datasets"></a>Adatkészletek létrehozása
Ebben a lépésben adatkészleteket hoz létre, amelyek a Hive-feldolgozás bemeneti és kimeneti adatait képviselik. Ezek az adatkészletek az oktatóanyag során korábban létrehozott **AzureStorageLinkedService** szolgáltatásra hivatkoznak. A társított szolgáltatás egy Azure Storage-fiókra mutat, az adatkészletek pedig meghatározzák a bemeneti és kimeneti adatokat tartalmazó tárban lévő tárolót, mappát és fájlnevet.   

### <a name="create-input-dataset"></a>Bemeneti adatkészlet létrehozása
1. A **Data Factory Editorban** kattintson ide: **... More** (... További) a parancssávon, kattintson a **New dataset** (Új adathalmaz) elemre, és válassza az **Azure Blob Storage** lehetőséget.

    ![New dataset (Új adatkészlet)](./media/data-factory-build-your-first-pipeline-using-editor/new-data-set.png)
2. Másolja és illessze be a következő kódrészletet a Draft-1 (Vázlat-1) ablakba. A JSON-kódrészletben hozza létre az **AzureBlobInput** nevű adatkészletet, amely a folyamat egyik tevékenységének bemeneti adatait képviseli. Emellett határozza meg azt is, hogy a bemeneti adatok az **adfgetstarted** nevű blob-tárolóban és az **inputdata** nevű mappában találhatók.

    ```JSON
    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "input.log",
                "folderPath": "adfgetstarted/inputdata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }
    ```
    Az alábbi táblázat ismerteti a kódrészletben használt JSON-tulajdonságokat:

   | Tulajdonság | Leírás |
   |:--- |:--- |
   | type |A tulajdonság beállításának értéke **AzureBlob**, mert az adatok egy Azure Blob Storage-tárban találhatók. |
   | linkedServiceName |A korábban létrehozott **AzureStorageLinkedService** szolgáltatásra vonatkozik. |
   | folderPath | A bemeneti blobokat tartalmazó **blobtárolót** és **mappát** adja meg. | 
   | fileName |Ez a tulajdonság nem kötelező. Ha kihagyja, az összes fájl ki lesz választva a folderPath útvonalról. Ebben az oktatóanyagban csak az **input.log** feldolgozása történik meg. |
   | type |Mivel a naplófájlok szövegformátumúak, a **TextFormat** típust kell beállítani. |
   | columnDelimiter |A naplófájlokban lévő oszlopok **vesszővel (`,`)** vannak elválasztva. |
   | frequency/interval |A frequency (gyakoriság) beállítás értéke **Month** (Hónap), az interval (időköz) beállítás értéke **1**, tehát a bemeneti szeletek havonta érhetők el. |
   | external | Ez a tulajdonság a **true** (igaz) értékre van állítva, ha a bemeneti adatokat nem ez a folyamat hozta létre. Mivel ebben az oktatóanyagban a bemeneti naplófájlt nem ez a folyamat hozta létre, a tulajdonság értéke „igaz”. |

    Ezekről a JSON-tulajdonságokról további tudnivalók az [Azure Blob-összekötőről](data-factory-azure-blob-connector.md#dataset-properties) szóló cikkben olvashatók.
3. Az újonnan létrehozott adatkészlet üzembe helyezéséhez kattintson a parancssáv **Deploy** (Üzembe helyezés) elemére. Az adatkészletnek meg kell jelennie a bal oldali fanézetben.

### <a name="create-output-dataset"></a>Kimeneti adatkészlet létrehozása
Most a kimeneti adatkészletet hozza létre, amely az Azure Blob Storage-tárban tárolt kimeneti adatokat jelöli.

1. A **Data Factory Editorban** kattintson ide: **... More** (... További) a parancssávon, kattintson a **New dataset** (Új adathalmaz) elemre, és válassza az **Azure Blob Storage** lehetőséget.  
2. Másolja és illessze be a következő kódrészletet a Draft-1 (Vázlat-1) ablakba. A JSON-kódrészletben hozza létre az **AzureBlobOutput** nevű adatkészletet, és határozza meg a Hive-parancsfájl által előállított adatok szerkezetét. Emellett határozza meg azt is, hogy az eredmények tárolása az **adfgetstarted** nevű blob-tárolóban és a **partitioneddata** nevű mappában történjen. Az **availability** (rendelkezésre állás) szakasz meghatározza, hogy a kimeneti adatkészlet előállítása havonta történik.

    ```JSON
    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
          "folderPath": "adfgetstarted/partitioneddata",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "availability": {
          "frequency": "Month",
          "interval": 1
        }
      }
    }
    ```
    A tulajdonságok leírását a **Bemeneti adatkészlet létrehozása** című szakaszban tekintheti meg. Külső adatkészlet esetén nem kell beállítani az external (külső) tulajdonságot, mert az adatkészletet a Data Factory szolgáltatás állítja elő.
3. Az újonnan létrehozott adatkészlet üzembe helyezéséhez kattintson a parancssáv **Deploy** (Üzembe helyezés) elemére.
4. Ellenőrizze az adatkészlet létrehozása sikeres volt-e.

    ![Fanézet a társított szolgáltatásokkal](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-data-set.png)

## <a name="create-pipeline"></a>Folyamat létrehozása
Ebben a lépésben létrehozza a **HDInsightHive** tevékenységgel rendelkező első adatcsatornát. A bemeneti szelet havonta érhető el (frequency: Month, interval: 1), a kimeneti szelet előállítása havonta történik, és a tevékenység scheduler (ütemező) tulajdonsága szintén a hónap értékre van állítva. A kimeneti adatkészlet és a tevékenységütemező beállításainak egyezniük kell. Jelenleg a kimeneti adatkészlet vezérli az ütemezést, ezért kimeneti adatkészletet akkor is létre kell hoznia, ha a tevékenység nem állít elő semmilyen kimenetet. Ha a tevékenység nem fogad semmilyen bemenetet, kihagyhatja a bemeneti adatkészlet létrehozását. Az alábbi JSON-fájlban használt tulajdonságok magyarázata a szakasz végén található.

1. A **Data Factory Editorban** kattintson a **további parancsokat jelölő három pontra (...)**, majd kattintson a **New pipeline** (Új folyamat) elemre.

    ![A New pipeline (Új folyamat) gomb](./media/data-factory-build-your-first-pipeline-using-editor/new-pipeline-button.png)
2. Másolja és illessze be a következő kódrészletet a Draft-1 (Vázlat-1) ablakba.

   > [!IMPORTANT]
   > A JSON-fájlban cserélje a **storageaccountname** kifejezést a tárfiókja nevére.
   >
   >

    ```JSON
    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "AzureStorageLinkedService",
                        "defines": {
                            "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                            "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "policy": {
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "name": "RunSampleHiveActivity",
                    "linkedServiceName": "HDInsightOnDemandLinkedService"
                }
            ],
            "start": "2017-07-01T00:00:00Z",
            "end": "2017-07-02T00:00:00Z",
            "isPaused": false
        }
    }
    ```

    A JSON-kódrészletben létrehoz egy folyamatot, amely egyetlen tevékenységből áll, és a tevékenység a Hive használatával dolgozza fel az adatokat egy HDInsight-fürtön.

    A **partitionweblogs.hql** Hive-parancsfájl tárolása az Azure Storage-fiókban (az **AzureStorageLinkedService** nevű scriptLinkedService szolgáltatás által megadva), és az **adfgetstarted** tároló **script** mappájában történik.

    A **defines** (meghatározza) szakasz meghatározza a futásidő beállításait, amelyek Hive konfigurációs értékekként (például ${hiveconf:inputtable}, ${hiveconf:partitionedtable}) lesznek átadva a Hive-parancsfájlnak.

    A folyamat **start** (kezdés) és **end** (befejezés) tulajdonságai a folyamat aktív időszakát határozzák meg.

    A tevékenység JSON-fájljában meg van határozva, hogy a Hive-parancsfájl a **linkedServiceName** – **HDInsightOnDemandLinkedService** által meghatározott számítási szolgáltatáson fut.

   > [!NOTE]
   > A példában használt JSON-tulajdonságokkal kapcsolatos részletekért lásd „A folyamat JSON-fájlja” szakaszt a [Folyamatok és tevékenységek az Azure Data Factoryban](data-factory-create-pipelines.md) című témakörben.
   >
   >
3. Ellenőrizze az alábbiakat:

   1. Az **input.log** fájl létezik az Azure blob-tároló **adfgetstarted** tárolójának **inputdata** mappájában.
   2. A **partitionweblogs.hql** fájl létezik az Azure blob-tároló **adfgetstarted** tárolójának **script** mappájában. Ha nem látja ezeket a fájlokat, hajtsa végre [Az oktatóanyag áttekintése](data-factory-build-your-first-pipeline.md) című cikkben előfeltételként felsorolt lépéseket.
   3. Ellenőrizze, hogy a folyamat JSON-fájljában lecserélte-e a **storageaccountname** kifejezést a tárfiókja nevére.
4. A folyamat üzembe helyezéséhez kattintson a parancssáv **Deploy** (Üzembe helyezés) elemére. Mivel a **start** (kezdés) és az **end** (befejezés) időpontok múltbeli értékekre vannak beállítva, és az **isPaused** tulajdonság értéke false (hamis), a folyamat (a folyamatban foglalt tevékenység) az üzembe helyezés után azonnal fut.
5. Győződjön meg arról, hogy a folyamat megjelenik a fanézetben.

    ![A fanézet a folyamattal](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-pipeline.png)
6. Gratulálunk, sikeresen létrehozta első folyamatát!

## <a name="monitor-pipeline"></a>Folyamat figyelése
### <a name="monitor-pipeline-using-diagram-view"></a>Folyamat figyelése diagramnézetben
1. A Data Factory Editor paneljeinek a bezárásához és a Data Factory panelre való visszatéréshez kattintson az **X**, majd a **Diagram** elemre.

    ![Diagram csempe](./media/data-factory-build-your-first-pipeline-using-editor/diagram-tile.png)
2. A diagramnézet áttekintést nyújt az oktatóanyagban használt folyamatokról és adatkészletekről.

    ![Diagramnézet](./media/data-factory-build-your-first-pipeline-using-editor/diagram-view-2.png)
3. A folyamat összes tevékenységének megtekintéséhez kattintson a jobb gombbal a folyamatra a diagramban, majd kattintson a Feldolgozási sor megnyitása elemre.

    ![Folyamat megnyitása menü](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-menu.png)
4. Győződjön meg arról, hogy a HDInsightHive tevékenység megjelenik a folyamatban.

    ![Folyamat megnyitása nézet](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-view.png)

    Az előző nézethez való visszatéréshez az oldal tetején lévő navigációs menüben kattintson a **Data factory** elemre.
5. A **diagramnézetben** kattintson duplán az **AzureBlobInput** adatkészletre. Győződjön meg arról, hogy a szelet **Ready** (Kész) állapotú. Eltarthat néhány percig, amíg a szelet Ready (Kész) állapotúként jelenik meg. Ha ez azután sem történik meg, hogy vár néhány percet, ellenőrizze, hogy a megfelelő tárolóba (adfgetstarted) és mappába (inputdata) helyezte-e a bemeneti fájlt (input.log).

   ![Kész állapotú bemeneti szelet](./media/data-factory-build-your-first-pipeline-using-editor/input-slice-ready.png)
6. Kattintson az **X** elemre az **AzureBlobInput** panel bezárásához.
7. A **diagramnézetben** kattintson duplán az **AzureBlobOutput** adatkészletre. Látni fogja, hogy a szelet feldolgozás alatt áll.

   ![Adatkészlet](./media/data-factory-build-your-first-pipeline-using-editor/dataset-blade.png)
8. A feldolgozás befejeztével a szelet **Ready** (Kész) állapotúra vált.

   ![Adatkészlet](./media/data-factory-build-your-first-pipeline-using-editor/dataset-slice-ready.png)  

   > [!IMPORTANT]
   > Az igény szerinti HDInsight-fürt létrehozása általában eltart egy ideig (körülbelül 20 percig). Ezért a folyamat várhatóan       **körülbelül 30 perc** alatt dolgozza fel a szeletet.
   >
   >

9. Ha a szelet **Ready** (Kész) állapotú, a kimeneti adatok **adfgetstarted** Blob Storage-tárolójában ellenőrizze a **partitioneddata** mappát.  

   ![kimeneti adatok](./media/data-factory-build-your-first-pipeline-using-editor/three-ouptut-files.png)
10. A szelet részleteinek az **Adatszelet** panelen való megtekintéséhez kattintson a szeletre.

   ![Adatszelet részletei](./media/data-factory-build-your-first-pipeline-using-editor/data-slice-details.png)  
11. Kattintson egy tevékenységfuttatásra az **Activity runs list** (Tevékenységfuttatások listája) területen a tevékenységfuttatás (ebben az esetben Hive-tevékenység) részleteinek az **Activity run details** (Tevékenységfuttatás részletei) ablakban való megjelenítéséhez.   

   ![Tevékenységfuttatás részletei](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-blade.png)    

   A naplófájlokban láthatja a végrehajtott Hive-lekérdezést és az állapotadatokat. A naplók hasznosak bármilyen hiba esetén a hibaelhárításban.
   További részletekért tekintse meg a [Monitor and manage pipelines using Azure portal blades](data-factory-monitor-manage-pipelines.md) (Folyamatok figyelése és felügyelete az Azure Portal paneleinek használatával) című cikket.

> [!IMPORTANT]
> A szelet sikeres feldolgozásakor a rendszer törli a bemeneti fájlt. Ezért ha újra le szeretné futtatni a szeletet, vagy újra el szeretné végezni az oktatóanyagban foglaltakat, töltse fel a bemeneti fájlt (input.log) az adfgetstarted tároló inputdata mappájába.
>
>

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Folyamat figyelése a Monitor & Manage alkalmazással
A folyamatok figyeléséhez a Monitor & Manage alkalmazást is használhatja. Az alkalmazás használatával kapcsolatos részletes információkért tekintse meg a [Monitor and manage Azure Data Factory pipelines using Monitoring and Management App](data-factory-monitor-manage-app.md) (Azure Data Factory-folyamatok figyelése és felügyelete a Monitoring and Management használatával) című cikket.

1. Kattintson a **Monitor & Manage** csempére a Data Factory kezdőlapján.

    ![Monitor & Manage csempe](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-tile.png)
2. Meg kell jelennie a **Monitor & Manage alkalmazásnak**. Módosítsa a **kezdési idő** és a **befejezési idő** értékét, hogy megfeleljen a folyamat kezdési és befejezési idejének, és kattintson az **Apply** (Alkalmaz) elemre.

    ![Monitor & Manage alkalmazás](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-app.png)
3. Válasszon egy tevékenységablakot az **Activity Windows** (Tevékenységablakok) listában a részleteinek a megtekintéséhez.

    ![Tevékenységablakok részletei](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-details.png)

## <a name="summary"></a>Összefoglalás
Az oktatóanyag során létrehozott egy Azure data factoryt, amely egy HDInsight Hadoop-fürtön futtatott Hive-parancsfájllal dolgozza fel az adatokat. Az Azure Portal Data Factory Editor eszközét használta a következő lépések végrehajtásához:  

1. Létrehozott egy Azure **data factoryt**.
2. Létrehozott két **társított szolgáltatást**:
   1. Az **Azure Storage** társított szolgáltatást, amely a bemeneti és kimeneti adatokat tároló Azure blob-tárolót társítja a data factoryval.
   2. Az **Azure HDInsight** igény szerinti társított szolgáltatást, amely egy igény szerinti HDInsight Hadoop-fürtöt társít a data factoryval. Az Azure Data Factory létrehoz egy HDInsight Hadoop-fürtöt, amely igény szerint dolgozza fel a bemeneti adatokat és állítja elő a kimeneti adatokat.
3. Létrehozott két **adatkészletet**, amelyek leírják a bemeneti és kimeneti adatokat az adatcsatorna HDInsight Hive-tevékenysége számára.
4. Létrehozott egy **folyamatot** egy **HDInsight Hive**-tevékenységgel.

## <a name="next-steps"></a>Következő lépések
Az oktatóanyag során létrehozott egy folyamatot egy adatátalakítási tevékenységgel (HDInsight-tevékenység), amely Hive-parancsfájlt futtat egy igény szerinti HDInsight-fürtön. Ha tudni szeretné, hogyan használhatja a Másolás tevékenységet az adatok Azure-blobból Azure SQL Database adatbázisba történő másolásához, tekintse meg a következő cikket: [Tutorial: Copy data from an Azure blob to Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) (Oktatóanyag: adatok másolása Azure-blobból Azure SQL Database adatbázisba).

## <a name="see-also"></a>Lásd még:
| Témakör | Leírás |
|:--- |:--- |
| [Folyamatok](data-factory-create-pipelines.md) |Ennek a cikknek a segítségével megismerheti a Azure Data Factory folyamatait és tevékenységeit, és megtudhatja, hogyan hozhat létre velük teljes körű, adatvezérelt munkafolyamatokat saját forgatókönyvéhez vagy vállalkozásához. |
| [Adatkészletek](data-factory-create-datasets.md) |Ennek a cikknek a segítségével megismerheti az adatkészleteket az Azure Data Factoryban. |
| [Ütemezés és végrehajtás](data-factory-scheduling-and-execution.md) |Ez a cikk ismerteti az Azure Data Factory-alkalmazásmodell ütemezési és végrehajtási aspektusait. |
| [Folyamatok figyelése és felügyelete a Monitoring App használatával](data-factory-monitor-manage-app.md) |Ez a cikk ismerteti, hogyan figyelheti és felügyelheti a folyamatokat, illetve hogyan kereshet bennük hibákat a Monitoring & Management App használatával. |
