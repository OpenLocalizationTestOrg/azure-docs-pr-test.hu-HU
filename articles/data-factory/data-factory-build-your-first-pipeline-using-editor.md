---
title: "aaaBuild az első adat-előállítóban (Azure-portál) |} Microsoft Docs"
description: "Ebben az oktatóanyagban létrehoz egy minta Azure Data Factory-folyamathoz Data Factory Editor hello Azure-portál használatával."
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
ms.openlocfilehash: fc80776001b181a59c04d80d2e05c20b107a63f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-portal"></a>Oktatóanyag: az első Azure data factory létrehozása az Azure Portal használatával
> [!div class="op_single_selector"]
> * [Áttekintés és előfeltételek](data-factory-build-your-first-pipeline.md)
> * [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Resource Manager-sablon](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)


Ebből a cikkből megismerheti, hogyan toouse [Azure-portálon](https://portal.azure.com/) toocreate az első az Azure data factory. toodo hello az oktatóanyagot más eszközök/SDK használatával hello legördülő listából válasszon hello lehetőségek közül. 

Ebben az oktatóanyagban hello folyamat rendelkezik egy tevékenység: **HDInsight Hive tevékenység**. Ez a tevékenység hive parancsfájlok futtatására szolgál, hogy átalakítások bemeneti adatok tooproduce kimeneti adatok Azure HDInsight-fürtöt. hello csővezeték ütemezett toorun, a hónap közötti hello kezdő és záró időpontjának megadása után. 

> [!NOTE]
> hello adatok csővezeték ebben az oktatóanyagban alakítja át a bemeneti adatok tooproduce kimeneti adatokat. Hogyan oktatóanyagért toocopy adatok Azure Data Factory használatával, lásd: [oktatóanyag: adatok másolása a Blob Storage tooSQL adatbázis](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> Egy folyamathoz több tevékenység is tartozhat. És láncolt telepítését (egymás után futtatni egy tevékenységet) két tevékenység által hello bemeneti hello az adatkészlet többi tevékenység hello kimeneti adatkészlet egy tevékenység beállítását. További tudnivalókért lásd: [Ütemezés és végrehajtás a Data Factoryban](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).

## <a name="prerequisites"></a>Előfeltételek
1. Olvassa végig [oktatóanyag – áttekintés](data-factory-build-your-first-pipeline.md) cikkben és a teljes hello **előfeltétel** lépéseket.
2. Ez a cikk nem nyújt áttekintést hello Azure Data Factory szolgáltatásnak. Javasoljuk, hogy olvassa végig [Data Factory bemutatása tooAzure](data-factory-introduction.md) cikk részletes áttekintés hello szolgáltatást.  

## <a name="create-data-factory"></a>Data factory létrehozása
A data factory egy vagy több folyamattal rendelkezhet. A folyamaton belül egy vagy több tevékenység lehet. Adja meg például a másolási tevékenység toocopy forrás tooa cél adattárat és adatait egy HDInsight Hive tevékenység toorun a Hive parancsfájl tootransform adatok tooproduct kimeneti adatokat. Kezdjük, az ebben a lépésben hello adat-előállító létrehozása.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Kattintson a **új** hello bal oldali menüben kattintson **adatok + analitika**, és kattintson a **adat-előállító**.

   ![A Create (Létrehozás) panel](./media/data-factory-build-your-first-pipeline-using-editor/create-blade.png)
3. A hello **új adat-előállító** panelen adja meg **GetStartedDF** a hello nevét.

   ![A New data factory (Új data factory) panel](./media/data-factory-build-your-first-pipeline-using-editor/new-data-factory-blade.png)

   > [!IMPORTANT]
   > az Azure data factory hello hello nevének kell lennie **globálisan egyedi**. Ha hello hibaüzenetet kapja: **nem érhető el adat-előállító "GetStartedDF"**. Módosítsa a hello adat-előállítóban (például yournameGetStartedDF) hello nevét, majd próbálja meg újra létrehozni. A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.
   >
   > hello adat-előállító nevét hello regisztrálva előfordulhat, hogy legyen egy **DNS** hello jövőben nevet, és ezért a nyilvánosan láthatóvá válnak.
   >
   >
4. Jelölje be hello **Azure-előfizetés** hello data factory toobe létrehozni kívánt.
5. Jelöljön ki egy meglévő **erőforráscsoportot**, vagy hozzon létre egyet. Az oktatóanyagban hello nevű erőforráscsoport létrehozása: **ADFGetStartedRG**.
6. Jelölje be hello **hely** hello adat-előállító esetében. Csak a Data Factory szolgáltatásnak hello által támogatott régiók hello legördülő listában jelennek meg.
7. Válassza ki **PIN-kód toodashboard**. 
8. Kattintson a **létrehozása** a hello **új adat-előállító** panelen.

   > [!IMPORTANT]
   > toocreate adat-előállító példányok hello tagjának kell lennie [Data Factory közreműködői](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) szerepkör hello előfizetés-erőforráscsoport szintjén.
   >
   >
7. Hello irányítópult állapotú csempe hello következő látja: telepítését adat-előállítóban.    

   ![A data factory létrehozásának állapota](./media/data-factory-build-your-first-pipeline-using-editor/creating-data-factory-image.png)
8. Gratulálunk! Sikeresen létrehozta első data factoryjét. Miután hello adat-előállító létrehozása sikerült, oldal akkor jelenik meg hello adatok gyári, amely jelzi, hogy hello hello adat-előállító tartalmát.     

    ![A Data Factory panel](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-blade.png)

Mielőtt létrehozna egy folyamat hello adat-előállítóban, kell toocreate néhány adat-előállító entitások először. Először létre kell hoznia összekapcsolt szolgáltatások toolink adatok tárolók/kiszámítja tooyour adatok tárolására, adja meg a bemeneti és kimeneti adatkészletek toorepresent bemeneti/kimeneti adatai csatolt adatok áruházakból és majd hozzon létre egy tevékenység által használt ezek az adatkészletek hello folyamat.

## <a name="create-linked-services"></a>Társított szolgáltatások létrehozása
Ebben a lépésben csatolja a az Azure Storage-fiók és az igény szerinti Azure HDInsight fürt tooyour adat-előállítóban. hello tartás hello hello adatcsatorna Ez a példa a bemeneti és kimeneti adatok Azure Storage-fiók. HDInsight kapcsolódó szolgáltatás hello használt toorun hello folyamatának Ez a példa hello tevékenységben megadott Hive parancsfájl. Mi azonosítása [adattár](data-factory-data-movement-activities.md)/[szolgáltatások számítási](data-factory-compute-linked-services.md) a forgatókönyvben használt, és ezen szolgáltatások toohello adat-előállító hivatkozás összekapcsolt szolgáltatások létrehozásával.  

### <a name="create-azure-storage-linked-service"></a>Azure Storage társított szolgáltatás létrehozása
Ebben a lépésben az Azure Storage-fiók tooyour adat-előállító hivatkozásra. Ebben az oktatóanyagban hello használata azonos Azure Storage-fiók toostore bemeneti/kimeneti adatok és hello HQL parancsfájlt.

1. Kattintson a **Szerző és központi telepítése** a hello **adat-előállító** paneljén **GetStartedDF**. Hello Data Factory Editor kell megjelennie.

   ![Az Author and deploy (Fejlesztés és üzembe helyezés) csempe](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-author-deploy.png)
2. Kattintson a **New data store** (Új adattár) elemre, és válassza az **Azure Storage** elemet.

   ![Új adattár – Azure Storage – menü](./media/data-factory-build-your-first-pipeline-using-editor/new-data-store-azure-storage-menu.png)
3. Megtekintheti az hello JSON-parancsfájl létrehozásához egy Azure Storage társított szolgáltatásnak hello-szerkesztőben.

   ![Azure Storage társított szolgáltatás](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. Cserélje le **fióknév** hello nevű Azure-tárfiókot és **fiókkulcs** kulcsával hello hozzáférés hello Azure storage-fiók. toolearn hogyan tooget tárhelyét a hozzáférési kulcs, hogyan tooview, másolása és újragenerálása tárolási hívóbetűk a hello adatainak megjelenítéséhez [a tárfiók kezelése](../storage/common/storage-create-storage-account.md#manage-your-storage-account).
5. Kattintson a **telepítés** a hello parancssávon toodeploy kapcsolódó hello szolgáltatást.

    ![A Deploy (Üzembe helyezés) gomb](./media/data-factory-build-your-first-pipeline-using-editor/deploy-button.png)

   Hello társított szolgáltatás telepítése után sikeresen hello **vázlat-1** ablak kell eltűnnek, és látni **AzureStorageLinkedService** a hello bal oldali fanézetben hello.

    ![Storage társított szolgáltatás a menüben](./media/data-factory-build-your-first-pipeline-using-editor/StorageLinkedServiceInTree.png)    

### <a name="create-azure-hdinsight-linked-service"></a>Azure HDInsight társított szolgáltatás létrehozása
Ebben a lépésben egy igény szerinti HDInsight fürt tooyour adat-előállító hivatkozásra. az automatikusan létrehozott futásidőben és törlése után feldolgozásra és üresjárati hello megadott időtartamig hello HDInsight-fürthöz.

1. A hello **Data Factory Editor**, kattintson a **... More** (... További), kattintson a **New compute** (Új számítási példány) elemre, majd válassza az **On-demand HDInsight cluster** (Igény szerinti HDInsight-fürt) lehetőséget.

    ![A New compute (Új számítás) elem](./media/data-factory-build-your-first-pipeline-using-editor/new-compute-menu.png)
2. Másolja és illessze be a következő kódrészletet toohello hello **vázlat-1** ablak. hello JSON részlet használt toocreate hello HDInsight fürt igény hello tulajdonságokat ismerteti.

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

    hello következő táblázat ismerteti hello részlet használt hello JSON-tulajdonságok:

   | Tulajdonság | Leírás |
   |:--- |:--- |
   | ClusterSize |HDInsight-fürt hello hello méretét adja meg. |
   | TimeToLive | Adott hello hello HDInsight-fürtjéhez, üresjárati idejét határozza meg, törlés előtt. |
   | linkedServiceName | Megadja a hello tárfiókja, amely a HDInsight által létrehozott használt toostore hello naplókat. |

    Vegye figyelembe a következő pontok hello:

   * hello adat-előállító létrehoz egy **Linux-alapú** a hello JSON meg HDInsight-fürthöz. További információkért lásd: [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).
   * Igény szerinti HDInsight-fürt helyett **saját HDInsight-fürtöt** is használhat. További információ: [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) (HDInsight társított szolgáltatás).
   * hello HDInsight-fürtöt hoz létre egy **alapértelmezett tároló** az Ön által megadott hello JSON hello blob storage (**linkedServiceName**). HDInsight nem törli a tárolót hello fürt törlésekor. Ez a működésmód szándékos. Igény szerinti HDInsight társított szolgáltatás esetén a rendszer a szeletek feldolgozásakor mindig létrehoz egy HDInsight-fürtöt, kivéve, ha van meglévő élő fürt (**timeToLive**). hello feldolgozása hello fürt automatikusan törlődnek.

       Ahogy egyre több szelet lesz feldolgozva, egyre több tároló jelenik meg az Azure Blob Storage-tárban. Ha nem kell őket hello feladatok hibaelhárítási, érdemes lehet a toodelete őket tooreduce hello tárolási költségeket. ezekhez a tárolókhoz hello nevei, hajtsa végre a minta: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Használjon például az eszközök [Microsoft Tártallózó](http://storageexplorer.com/) toodelete tárolókat az az Azure blob-tároló.

     További információkért lásd: [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).
3. Kattintson a **telepítés** a hello parancssávon toodeploy kapcsolódó hello szolgáltatást.

    ![Igény szerinti HDInsight társított szolgáltatás üzembe helyezése](./media/data-factory-build-your-first-pipeline-using-editor/ondemand-hdinsight-deploy.png)
4. Ellenőrizze, hogy látható mindkét **AzureStorageLinkedService** és **HDInsightOnDemandLinkedService** a hello bal oldali fanézetben hello.

    ![Fanézet a társított szolgáltatásokkal](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-linked-services.png)

## <a name="create-datasets"></a>Adatkészletek létrehozása
Ebben a lépésben létrehozni adatkészletek toorepresent hello bemeneti és kimeneti adatai Hive feldolgozásra. Ezek az adatkészletek tekintse meg a toohello **AzureStorageLinkedService** Ez az oktatóanyag korábbi részében létrehozott. hello társított szolgáltatás pontok tooan Azure Storage-fiókot, és adja meg tároló, mappa, fájl neve adatkészletek hello tárolóban, amely tárolja a bemeneti és kimeneti adatokat.   

### <a name="create-input-dataset"></a>Bemeneti adatkészlet létrehozása
1. A hello **Data Factory Editor**, kattintson a **... További** hello parancssávon kattintson **új adatkészlet**, és válassza ki **Azure Blob Storage tárolóban**.

    ![New dataset (Új adatkészlet)](./media/data-factory-build-your-first-pipeline-using-editor/new-data-set.png)
2. Másolja és illessze be a következő kódrészletet toohello vázlat-1 ablak hello. Hello JSON részlet, nevű adatkészlet létrehozásához **AzureBlobInput** hello feldolgozási soros tevékenység bemeneti adatokat képvisel, amelyek. Emellett megadhatja, hogy hello bemeneti adatok nevű hello blob tárolóban található **adfgetstarted** és nevű hello mappát **inputdata**.

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
    hello következő táblázat ismerteti hello részlet használt hello JSON-tulajdonságok:

   | Tulajdonság | Leírás |
   |:--- |:--- |
   | type |hello type tulajdonság beállítása túl**AzureBlob** , mert az adatok találhatók az Azure blob Storage tárolóban. |
   | linkedServiceName |Toohello hivatkozik **AzureStorageLinkedService** korábban létrehozott. |
   | folderPath | Adja meg a hello blob **tároló** és hello **mappa** , amely tartalmazza a bemeneti BLOB. | 
   | fileName |Ez a tulajdonság nem kötelező. Ha ez a tulajdonság nincs megadva, az összes hello fájlok hello folderPath leltárhoz. Ebben az oktatóanyagban csak hello **input.log** dolgoz fel. |
   | type |hello naplófájlok szöveges formátumú, így használjuk **szöveges**. |
   | columnDelimiter |oszlopok hello naplófájlokban határolja **vesszővel karakter (`,`)** |
   | frequency/interval |gyakoriságának beállítása túl**hónap** és időköz **1**, ami azt jelenti, hogy hello bemeneti szeletek havi érhetők el. |
   | external | Ez a tulajdonság értéke túl**igaz** hello bemeneti adatok nem jön létre, ez az adatcsatorna. Ebben az oktatóanyagban hello input.log fájl nem jön létre ez az adatcsatorna, így hello tulajdonság tootrue hivatott. |

    Ezekről a JSON-tulajdonságokról további tudnivalók az [Azure Blob-összekötőről](data-factory-azure-blob-connector.md#dataset-properties) szóló cikkben olvashatók.
3. Kattintson a **telepítés** a toodeploy hello az újonnan létrehozott adatkészlet hello parancssávon. Meg kell jelennie a hello bal oldali fanézetben hello hello adatkészlet.

### <a name="create-output-dataset"></a>Kimeneti adatkészlet létrehozása
Hello kimeneti adatkészlet toorepresent hello kimeneti tárolt adatok hello Azure Blob-tároló létrehozása

1. A hello **Data Factory Editor**, kattintson a **... További** hello parancssávon kattintson **új adatkészlet**, és válassza ki **Azure Blob Storage tárolóban**.  
2. Másolja és illessze be a következő kódrészletet toohello vázlat-1 ablak hello. Hello JSON részlet, nevű adatkészlet létrehozásához **AzureBlobOutput**, és hello struktúra hello Hive parancsfájl által létrehozott hello adatok megadásával. Emellett megadhatja, hogy hello eredmények nevű hello blob tárolóban kell tárolni **adfgetstarted** és nevű hello mappát **partitioneddata**. Hello **rendelkezésre állási** szakaszban határozza meg, hogy hello kimeneti adatkészlet havonta jön létre.

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
    Lásd: **létrehozni hello bemeneti adatkészletet** szakasz ezeket a tulajdonságokat leírását. Be kell állítani nem hello külső tulajdonság egy kimeneti adatkészletet, hello dataset hozzák hello Data Factory szolgáltatásnak.
3. Kattintson a **telepítés** a toodeploy hello az újonnan létrehozott adatkészlet hello parancssávon.
4. Győződjön meg arról, hogy hello adatkészlet sikeresen létrejött-e.

    ![Fanézet a társított szolgáltatásokkal](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-data-set.png)

## <a name="create-pipeline"></a>Folyamat létrehozása
Ebben a lépésben létrehozza a **HDInsightHive** tevékenységgel rendelkező első adatcsatornát. Bemeneti szelet érhető havonta (gyakoriság: hónap, időköz: 1), a kimeneti szelet havonta jön létre, és hello Feladatütemező hello tevékenység is tulajdonsága toomonthly. hello kimeneti adatkészlet és hello tevékenység Feladatütemező hello beállításait meg kell egyeznie. Kimeneti adatkészlet jelenleg milyen meghajtók hello ütemezését, így még akkor is, ha hello tevékenység nem ad kimenetet kell létrehoznia egy kimeneti adatkészletet. Ha hello tevékenység egyetlen bemeneti nem veszi, kihagyhatja létrehozása hello bemeneti adatkészletet. a következő JSON hello használt hello tulajdonságok hello végén ebben a szakaszban lévő magyarázatát olvashatja.

1. A hello **Data Factory Editor**, kattintson a **három ponttal (…) További parancsok** majd **új adatcsatorna**.

    ![A New pipeline (Új folyamat) gomb](./media/data-factory-build-your-first-pipeline-using-editor/new-pipeline-button.png)
2. Másolja és illessze be a következő kódrészletet toohello vázlat-1 ablak hello.

   > [!IMPORTANT]
   > Cserélje le **storageaccountname** hello nevű hello JSON a storage-fiókot.
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

    Hello JSON részlet egy folyamatot, amely tartalmaz egy adott tevékenység által használt Hive tooprocess adatokat a HDInsight-fürtök létrehozásához.

    hello Hive parancsfájl, **partitionweblogs.hql**, hello Azure storage-fiók tárolva van (hello scriptLinkedService nevű által megadott **AzureStorageLinkedService**), majd a  **parancsfájl** hello tároló mappa **adfgetstarted**.

    Hello **meghatározása** szakaszban használt toospecify hello futtatási beállítások Hive értékként toohello hive parancsfájl átadott (például ${hiveconf: inputtable}, {hiveconf:partitionedtable} $).

    Hello **start** és **end** hello folyamatának tulajdonságok hello hello adatcsatorna aktív időszakát határozza meg.

    Hello tevékenység JSON-NÁ, meghatározza, hogy hello Hive parancsfájl futó hello által megadott hello számítási **linkedServiceName** – **HDInsightOnDemandLinkedService**.

   > [!NOTE]
   > "Adatcsatorna JSON" című [folyamatok és az Azure Data Factory tevékenységek](data-factory-create-pipelines.md) hello példában használt JSON-tulajdonságok vonatkozó további információért.
   >
   >
3. Erősítse meg a következő hello:

   1. **input.log** fájl megtalálható-e hello **inputdata** mappában található hello **adfgetstarted** hello Azure blob storage tárolója
   2. **partitionweblogs.hql** fájl megtalálható-e hello **parancsfájl** mappában található hello **adfgetstarted** hello Azure blob storage tárolója. Teljes hello előfeltételként szükséges lépések hello [oktatóanyag – áttekintés](data-factory-build-your-first-pipeline.md) Ha nem látja ezeket a fájlokat.
   3. Győződjön meg arról, hogy lecseréli **storageaccountname** hello nevet, a tárfiók hello a csővezeték-JSON.
4. Kattintson a **telepítés** a toodeploy hello csővezeték hello parancssávon. Hello óta **start** és **end** az elmúlt hello beállítása és **isPaused** van a (hello feldolgozási soros tevékenységek) set toofalse, hello csővezeték azonnal telepítése után.
5. Ellenőrizze, hogy látható-e hello fanézetben hello csővezeték-e.

    ![A fanézet a folyamattal](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-pipeline.png)
6. Gratulálunk, sikeresen létrehozta első folyamatát!

## <a name="monitor-pipeline"></a>Folyamat figyelése
### <a name="monitor-pipeline-using-diagram-view"></a>Folyamat figyelése diagramnézetben
1. Kattintson a **X** tooclose Data Factory Editor paneleken toonavigate biztonsági toohello adat-előállító panelt, és kattintson **Diagram**.

    ![Diagram csempe](./media/data-factory-build-your-first-pipeline-using-editor/diagram-tile.png)
2. Hello Diagram nézet hello folyamatok, és ebben az oktatóanyagban használt adatkészletek áttekintése látható.

    ![Diagramnézet](./media/data-factory-build-your-first-pipeline-using-editor/diagram-view-2.png)
3. az összes tevékenység hello sorban, kattintson a jobb gombbal kimenetátirányítási mechanizmusával hello ábra, majd kattintson a nyitott folyamat tooview.

    ![Folyamat megnyitása menü](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-menu.png)
4. Győződjön meg arról, hogy megjelenik-e hello adatcsatorna hello HDInsightHive tevékenysége.

    ![Folyamat megnyitása nézet](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-view.png)

    toonavigate biztonsági toohello előző nézetével, kattintson a **adat-előállító** hello navigációs menüjében hello tetején.
5. A hello **diagramnézet**, kattintson duplán a hello dataset **AzureBlobInput**. Győződjön meg arról, hogy hello szelet a **készen** állapotát. Azt is tarthat néhány percet a hello szelet tooshow üzemkész állapotban. Várja meg a némi várakozás után történik, ha megjelenítéséhez hello bemeneti fájl (input.log) hello jobb tároló (adfgetstarted) és a mappa (inputdata).

   ![Kész állapotú bemeneti szelet](./media/data-factory-build-your-first-pipeline-using-editor/input-slice-ready.png)
6. Kattintson a **X** tooclose **AzureBlobInput** panelen.
7. A hello **diagramnézet**, kattintson duplán a hello dataset **AzureBlobOutput**. Láthatja, hogy hello szelet, amely folyamatban van.

   ![Adatkészlet](./media/data-factory-build-your-first-pipeline-using-editor/dataset-blade.png)
8. Ha nem hajtja végre, megjelenik az hello szeletre **készen** állapotát.

   ![Adatkészlet](./media/data-factory-build-your-first-pipeline-using-editor/dataset-slice-ready.png)  

   > [!IMPORTANT]
   > Az igény szerinti HDInsight-fürt létrehozása általában eltart egy ideig (körülbelül 20 percig). Ezért várt hello folyamat túl érvénybe **körülbelül 30 percet** tooprocess hello szelet.
   >
   >

9. Ha hello szelet belül van **készen áll a** állapot, ellenőrizze a hello **partitioneddata** hello mappájában **adfgetstarted** a blob-tároló hello tárolóhoz kimeneti adatokat.  

   ![kimeneti adatok](./media/data-factory-build-your-first-pipeline-using-editor/three-ouptut-files.png)
10. Hello szelet toosee részleteit a kattintson egy **adatszelet** panelen.

   ![Adatszelet részletei](./media/data-factory-build-your-first-pipeline-using-editor/data-slice-details.png)  
11. Hello futtatása tevékenység kattintson **tevékenység fut lista** toosee adatait egy tevékenység futtatását (esetünkben Hive tevékenység) egy **tevékenység fut részletek** ablak.   

   ![Tevékenységfuttatás részletei](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-blade.png)    

   Hello naplófájlokból láthatja, hogy végre lett hajtva hello Hive-lekérdezések és állapotára vonatkozó információkat. A naplók hasznosak bármilyen hiba esetén a hibaelhárításban.
   További részletekért tekintse meg a [Monitor and manage pipelines using Azure portal blades](data-factory-monitor-manage-pipelines.md) (Folyamatok figyelése és felügyelete az Azure Portal paneleinek használatával) című cikket.

> [!IMPORTANT]
> hello bemeneti fájl törlése hello szelet feldolgozása sikeresen megtörtént. Ezért ha szeretné, hogy toorerun hello szelet, vagy újra hello oktatóanyag, feltöltési hello bemeneti fájl (input.log) toohello inputdata mappa hello adfgetstarted tároló.
>
>

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Folyamat figyelése a Monitor & Manage alkalmazással
Is figyelheti, & alkalmazás toomonitor kezelése a folyamatok. Az alkalmazás használatával kapcsolatos részletes információkért olvassa el a [Monitor and manage Azure Data Factory pipelines using Monitoring and Management App](data-factory-monitor-manage-app.md) (Azure Data Factory-folyamatok figyelése és felügyelete a Monitoring and Management használatával) című cikket.

1. Kattintson a **figyelő & kezelése** hello kezdőlap a data factory csempére.

    ![Monitor & Manage csempe](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-tile.png)
2. Meg kell jelennie a **Monitor & Manage alkalmazásnak**. Változás hello **kezdési időpont** és **befejező időpontja** toomatch indítsa el és befejezési időpontja, a folyamat, és kattintson a **alkalmaz**.

    ![Monitor & Manage alkalmazás](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-app.png)
3. Egy tevékenység ablakban válassza a hello **tevékenység Windows** listában toosee részleteit.

    ![Tevékenységablakok részletei](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-details.png)

## <a name="summary"></a>Összefoglalás
Ebben az oktatóanyagban az Azure data factory tooprocess adatok Hive parancsfájl futtatásával a HDInsight hadoop-fürthöz létrehozott. A lépéseket követve hello Azure portál toodo hello a Data Factory Editor hello használt:  

1. Létrehozott egy Azure **data factoryt**.
2. Létrehozott két **társított szolgáltatást**:
   1. **Az Azure Storage** kapcsolódó szolgáltatás toolink az Azure blob storage, amely a bemeneti/kimeneti fájlok toohello adat-előállító tárolja.
   2. **Az Azure HDInsight** igény kapcsolódó szolgáltatás toolink egy igény szerinti HDInsight Hadoop fürthöz toohello adat-előállítóban. Az Azure Data Factory egy HDInsight Hadoop fürthöz just-in-time tooprocess bemeneti adatok és a által előállított kimeneti adatokat hoz létre.
3. Két létrehozott **adatkészletek**, leírják hello folyamat HDInsight Hive tevékenység bemeneti és kimeneti adatokat.
4. Létrehozott egy **folyamatot** egy **HDInsight Hive**-tevékenységgel.

## <a name="next-steps"></a>Következő lépések
Az oktatóanyag során létrehozott egy folyamatot egy adatátalakítási tevékenységgel (HDInsight-tevékenység), amely Hive-parancsfájlt futtat egy igény szerinti HDInsight-fürtön. Hogyan toouse egy Azure Blob tooAzure SQL, a másolási tevékenység toocopy adatait: toosee [oktatóanyag: adatok másolása az Azure blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="see-also"></a>Lásd még:
| Témakör | Leírás |
|:--- |:--- |
| [Folyamatok](data-factory-create-pipelines.md) |Ez a cikk segít megérteni a folyamatok és az Azure Data Factory tevékenységeket, és hogyan toouse tooconstruct végpont adatvezérelt munkafolyamatok a forgatókönyv vagy üzleti őket. |
| [Adatkészletek](data-factory-create-datasets.md) |Ennek a cikknek a segítségével megismerheti az adatkészleteket az Azure Data Factoryban. |
| [Ütemezés és végrehajtás](data-factory-scheduling-and-execution.md) |Ez a cikk ismerteti az Azure Data Factory alkalmazásmodell hello ütemezés és a végrehajtási szempontjait. |
| [Folyamatok figyelése és felügyelete a Monitoring App használatával](data-factory-monitor-manage-app.md) |Ez a cikk ismerteti, hogyan toomonitor, kezelése és hibakeresése folyamatok használatával hello figyelés & a felügyeleti alkalmazás. |
