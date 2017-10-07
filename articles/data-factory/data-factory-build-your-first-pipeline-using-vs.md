---
title: "aaaBuild az első adat-előállítóban (Visual Studio) |} Microsoft Docs"
description: "Az oktatóanyag során létrehoz egy mintául szolgáló Azure Data Factory-folyamatot a Visual Studio használatával."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 7398c0c9-7a03-4628-94b3-f2aaef4a72c5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 0c5eb01b685d978d80916da0293cc2d3701b2d57
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-data-factory-by-using-visual-studio"></a>Oktatóanyag: adat-előállító létrehozása a Visual Studióval
> [!div class="op_single_selector" title="Tools/SDKs"]
> * [Áttekintés és előfeltételek](data-factory-build-your-first-pipeline.md)
> * [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Resource Manager-sablon](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)

Az oktatóanyag bemutatja, hogyan toocreate egy az Azure data factory Visual Studio használatával. Hello adat-előállító projektsablon használatával Visual Studio-projekt létrehozása, JSON formátumban adja meg a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és pipeline) és majd közzététele/telepítheti ezeket entitások toohello felhő. 

Ebben az oktatóanyagban hello folyamat rendelkezik egy tevékenység: **HDInsight Hive tevékenység**. Ez a tevékenység hive parancsfájlok futtatására szolgál, hogy átalakítások bemeneti adatok tooproduce kimeneti adatok Azure HDInsight-fürtöt. hello csővezeték ütemezett toorun, a hónap közötti hello kezdő és záró időpontjának megadása után. 

> [!NOTE]
> Ez az oktatóanyag nem tartalmazza az adatok Azure Data Factory használatával történő másolásának leírását. Hogyan oktatóanyagért toocopy adatok Azure Data Factory használatával, lásd: [oktatóanyag: adatok másolása a Blob Storage tooSQL adatbázis](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> Egy folyamathoz több tevékenység is tartozhat. És láncolt telepítését (egymás után futtatni egy tevékenységet) két tevékenység által hello bemeneti hello az adatkészlet többi tevékenység hello kimeneti adatkészlet egy tevékenység beállítását. További tudnivalókért lásd: [Ütemezés és végrehajtás a Data Factoryban](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).


## <a name="walkthrough-create-and-publish-data-factory-entities"></a>Útmutató: Data Factory-entitások létrehozása és közzététele
Ez a forgatókönyv során végrehajtandó hello lépések a következők:

1. Létrehozza a következő két társított szolgáltatást: **AzureStorageLinkedService1** és **HDInsightOnDemandLinkedService1**. 
   
    Ebben az oktatóanyagban a bemeneti és kimeneti adatok a hello hive tevékenység a hello azonos Azure Blob Storage. Egy igény szerinti HDInsight fürt tooprocess meglévő bemeneti adatok tooproduce kimeneti adatokat használ. hello igény szerinti HDInsight-fürt automatikusan létrejön az Ön által az Azure Data Factory futási időben feldolgozott készen toobe hello bemeneti adatok esetén. Az adatokat tárolja, vagy tooyour adat-előállító kiszámítja, hogy hello Data Factory szolgáltatásnak csatlakozhassanak a futási időben toothem toolink van szüksége. Ezért akkor az Azure Storage-fiók toohello adat-előállító hello AzureStorageLinkedService1 használatával hivatkozásra, és hello HDInsightOnDemandLinkedService1 segítségével igény szerinti HDInsight-fürtök hivatkozásra. Közzétételekor, hello data factory toobe létrehozott hello nevét vagy valamelyik adat-előállítót adja meg.  
2. Hozzon létre két adatkészletet: **InputDataset** és **OutputDataset**, amely képviseli hello Azure blob Storage tárolóban tárolt hello bemeneti/kimeneti adatokat. 
   
    A dataset definíciók tekintse meg a hello előző lépésben létrehozott toohello Azure tárolás társított szolgáltatása. A hello InputDataset hello blob-tároló (adfgetstarted) megadása, és egy blobot a hello bemeneti adatokat tartalmazó mappát (inptutdata) hello. A hello OutputDataset hello blob tároló (adfgetstarted) ad meg, és hello mappa (partitioneddata), amely tartalmazza a hello kimeneti adatokat. Egyéb tulajdonságokat is megad, például a szerkezetet, rendelkezésre állást és a szabályzatot.
3. Hozzon létre egy folyamatot **MyFirstPipeline** néven. 
  
    Ebben a bemutatóban hello folyamat csak egy tevékenység rendelkezik: **HDInsight Hive tevékenység**. A tevékenység transzformáció adatok tooproduce kimeneti adatokat adjon meg egy igény szerinti HDInsight-fürt hive-parancsfájl futtatásával. További információ a hive tevékenység toolearn lásd: [Hive tevékenység](data-factory-hive-activity.md) 
4. Létrehoz egy **DataFactoryUsingVS** nevű data factoryt. Telepítse a hello data factory és az összes adat-előállító entitások (a társított szolgáltatások, a táblák és a hello folyamat).
5. Miután közzéteszi, az Azure portál paneleken és figyelés & a felügyeleti alkalmazás toomonitor hello folyamat. 
  
### <a name="prerequisites"></a>Előfeltételek
1. Olvassa végig [oktatóanyag – áttekintés](data-factory-build-your-first-pipeline.md) cikkben és a teljes hello **előfeltétel** lépéseket. Igény szerint kiválaszthatja hello **áttekintése és előfeltételek** beállítás hello felső tooswitch toohello cikk hello legördülő listáról. Hello Előfeltételek befejezése után váltson vissza toothis cikk kiválasztásával **Visual Studio** hello legördülő lista beállítását.
2. toocreate adat-előállító példányok hello tagjának kell lennie [Data Factory közreműködői](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) szerepkör hello előfizetés-erőforráscsoport szintjén.  
3. A számítógépen telepített hello következő kell rendelkeznie:
   * Visual Studio 2013 vagy Visual Studio 2015
   * Töltse le az Azure SDK-t a Visual Studio 2013-hoz vagy a Visual Studio 2015-höz. Keresse meg a túl[Azure letöltési oldalát](https://azure.microsoft.com/downloads/) kattintson **VS 2013** vagy **VS 2015** a hello **.NET** szakasz.
   * Töltse le a legfrissebb hello Azure Data Factory beépülő modul a Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) vagy [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Hello beépülő modult is frissítheti hello lépések végrehajtásával: hello menüjének **eszközök** -> **bővítmények és frissítések** -> **Online**  ->  **Visual Studio galériában** -> **Microsoft Azure Data Factory Tools for Visual Studio** -> **frissítés**.

Most tegyük a Visual Studio toocreate egy az Azure data factory használja.

### <a name="create-visual-studio-project"></a>Visual Studio-projekt létrehozása
1. Indítsa el a **Visual Studio 2013-at** vagy a **Visual Studio 2015-öt**. Kattintson a **fájl**, pont túl**új**, és kattintson a **projekt**. Megtekintheti az hello **új projekt** párbeszédpanel megnyitásához.  
2. A hello **új projekt** párbeszédpanelen jelölje be hello **DataFactory** sablont, majd kattintson **üres Data Factory-projektekhez**.   

    ![A New project (Új projekt) párbeszédpanel](./media/data-factory-build-your-first-pipeline-using-vs/new-project-dialog.png)
3. Adja meg egy **neve** hello projekt **hely**, és hello nevét **megoldás**, és kattintson a **OK**.

    ![Megoldáskezelő](./media/data-factory-build-your-first-pipeline-using-vs/solution-explorer.png)

### <a name="create-linked-services"></a>Társított szolgáltatások létrehozása
Ebben a lépésben létrehozza a következő két társított szolgáltatást: **Azure Storage** és **igény szerinti HDInsight**. 

hello Azure Storage az Azure Storage-fiók toohello adat-előállító kapcsolódó szolgáltatás hivatkozásokat, adja meg a hello kapcsolódási információt. Data Factory szolgáltatásnak hello kapcsolati karakterláncot, kapcsolódó hello szolgáltatásbeállítás tooconnect toohello az Azure storage futtatás közben használ. Ez a tároló tárolja a bemeneti és kimeneti adatok hello pipeline és hello hive parancsfájl hello hive tevékenység által használt. 

Igény szerinti HDInsight kapcsolódó szolgáltatás, a HDInsight-fürt hello automatikusan megtörténik futásidőben hello bemeneti adata készen áll a tooprocessed. hello fürtök törlése után feldolgozásra és üresjárati hello megadott időtartamig. 

> [!NOTE]
> Létrehozhat egy adat-előállító hello helyreállításkor közzététele a Data Factory megoldás nevét és beállítások megadásával.

#### <a name="create-azure-storage-linked-service"></a>Azure Storage társított szolgáltatás létrehozása
1. Kattintson a jobb gombbal **összekapcsolt szolgáltatások** hello megoldáskezelőben pontot túl**Hozzáadás**, és kattintson a **új elem**.      
2. A hello **új elem hozzáadása** párbeszédpanelen jelölje ki **Azure Storage társított szolgáltatás** hello listából, és kattintson a **Hozzáadás**.
    ![Azure Storage társított szolgáltatás](./media/data-factory-build-your-first-pipeline-using-vs/new-azure-storage-linked-service.png)
3. Cserélje le `<accountname>` és `<accountkey>` hello nevű Azure-tárfiókot és a kulcsok. toolearn hogyan tooget tárhelyét a hozzáférési kulcs, hogyan tooview, másolása és újragenerálása tárolási hívóbetűk a hello adatainak megjelenítéséhez [a tárfiók kezelése](../storage/common/storage-create-storage-account.md#manage-your-storage-account).
    ![Azure Storage társított szolgáltatás](./media/data-factory-build-your-first-pipeline-using-vs/azure-storage-linked-service.png)
4. Mentse a hello **AzureStorageLinkedService1.json** fájlt.

#### <a name="create-azure-hdinsight-linked-service"></a>Azure HDInsight társított szolgáltatás létrehozása
1. A hello **Solution Explorer**, kattintson a jobb gombbal **összekapcsolt szolgáltatások**, pont túl**Hozzáadás**, és kattintson a **új elem**.
2. Válassza a **HDInsight On Demand Linked Service** (HDInsight igény szerinti társított szolgáltatás) lehetőséget, és kattintson az **Add** (Hozzáadás) parancsra.
3. Cserélje le a hello **JSON** a következő JSON hello:

     ```json
    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
        "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.5",
                "clusterSize": 1,
                "timeToLive": "00:05:00",
                "osType": "Linux",
                "linkedServiceName": "AzureStorageLinkedService1"
            }
        }
    }
    ```

    hello következő táblázat ismerteti hello részlet használt hello JSON-tulajdonságok:

    Tulajdonság | Leírás
    -------- | ----------- 
    ClusterSize | HDInsight Hadoop-fürt hello hello méretét határozza meg.
    TimeToLive | Adott hello hello HDInsight-fürtjéhez, üresjárati idejét határozza meg, törlés előtt.
    linkedServiceName | Megadja a hello tárfiókja, amely a HDInsight Hadoop-fürt által létrehozott használt toostore hello naplókat. 

    > [!IMPORTANT]
    > hello HDInsight-fürtöt hoz létre egy **alapértelmezett tároló** hello hello JSON (linkedServiceName) a megadott blob Storage tárolóban. HDInsight nem törli a tárolót hello fürt törlésekor. Ez a működésmód szándékos. Igény szerinti HDInsight társított szolgáltatás esetén a rendszer a szeletek feldolgozásakor mindig létrehoz egy HDInsight-fürtöt, kivéve, ha van meglévő élő fürt (timeToLive). hello feldolgozása hello fürt automatikusan törlődnek.
    > 
    > Ahogy egyre több szelet lesz feldolgozva, egyre több tároló jelenik meg az Azure Blob Storage-tárban. Ha nem kell őket hello feladatok hibaelhárítási, érdemes lehet a toodelete őket tooreduce hello tárolási költségeket. ezekhez a tárolókhoz hello nevei, hajtsa végre a minta: `adf<yourdatafactoryname>-<linkedservicename>-datetimestamp`. Használjon például az eszközök [Microsoft Tártallózó](http://storageexplorer.com/) toodelete tárolókat az az Azure blob-tároló.

    A JSON-tulajdonságokkal kapcsolatos további információkért lásd a [Társított szolgáltatások számítása](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) című cikket. 
4. Mentse a hello **HDInsightOnDemandLinkedService1.json** fájlt.

### <a name="create-datasets"></a>Adatkészletek létrehozása
Ebben a lépésben létrehozni adatkészletek toorepresent hello bemeneti és kimeneti adatai Hive feldolgozásra. Ezek az adatkészletek tekintse meg a toohello **AzureStorageLinkedService1** Ez az oktatóanyag korábbi részében létrehozott. hello társított szolgáltatás pontok tooan Azure Storage-fiókot, és adja meg tároló, mappa, fájl neve adatkészletek hello tárolóban, amely tárolja a bemeneti és kimeneti adatokat.   

#### <a name="create-input-dataset"></a>Bemeneti adatkészlet létrehozása
1. A hello **megoldáskezelő**, kattintson a jobb gombbal **táblák**, pont túl**hozzáadása**, és kattintson a **új elem**.
2. Válassza ki **Azure Blob** hello listából hello nevének módosítása hello fájl túl**InputDataSet.json**, és kattintson a **Hozzáadás**.
3. Cserélje le a hello **JSON** hello szerkesztőben a következő JSON részlet hello:

    ```json
    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
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
    A JSON-részlet nevű adatkészlet meghatározása **AzureBlobInput** képviselő hello hive tevékenység hello folyamat a bemeneti adatok. Megadja, hogy hello bemeneti adatok nevű hello blob tárolóban található `adfgetstarted` és nevű hello mappát `inputdata`.

    hello következő táblázat ismerteti hello részlet használt hello JSON-tulajdonságok:

    Tulajdonság | Leírás |
    -------- | ----------- |
    type |hello type tulajdonság beállítása túl**AzureBlob** adatokat az Azure Blob Storage helyezkedik el.
    linkedServiceName | A korábban létrehozott AzureStorageLinkedService1 toohello hivatkozik.
    fileName |Ez a tulajdonság nem kötelező. Ha ez a tulajdonság nincs megadva, az összes hello fájlok hello folderPath leltárhoz. Ebben az esetben csak hello input.log dolgoz fel.
    type | hello naplófájlok szöveges formátumú, így szöveges használjuk. |
    columnDelimiter | oszlopok hello naplófájlokban határolja hello vesszővel karakter (`,`)
    frequency/interval | gyakoriságának beállítása tooMonth és időköz 1, ami azt jelenti, hogy hello bemeneti szeletek elérhető havi.
    external | Ez a tulajdonság beállítása tootrue hello bemeneti adatok hello tevékenység hello az adatcsatorna nem jön létre. Ez a tulajdonság csak a bemeneti adatkészleteken van meghatározva. Hello bemeneti adatkészlet hello első tevékenység mindig állítani tootrue.
4. Mentse a hello **InputDataset.json** fájlt.

#### <a name="create-output-dataset"></a>Kimeneti adatkészlet létrehozása
Hello kimeneti adatkészlet toorepresent kimeneti tárolt adatok hello Azure Blob-tároló létrehozása

1. A hello **megoldáskezelő**, kattintson a jobb gombbal **táblák**, pont túl**hozzáadása**, és kattintson a **új elem**.
2. Válassza ki **Azure Blob** hello listából hello nevének módosítása hello fájl túl**OutputDataset.json**, és kattintson a **Hozzáadás**.
3. Cserélje le a hello **JSON** hello szerkesztőben a következő JSON hello:
    
    ```json
    {
        "name": "AzureBlobOutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
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
    hello JSON részlet nevű adatkészlet meghatározása **AzureBlobOutput** , hogy jelöli kimeneti hello folyamat hello hive tevékenység által létrehozott adatokat. Megadja, hogy hello kimeneti adatok hozzák hello hive tevékenység hello blob tároló nevű kerül `adfgetstarted` és nevű hello mappát `partitioneddata`. 
    
    Hello **rendelkezésre állási** szakaszban határozza meg, hogy hello kimeneti adatkészlet havonta jön létre. hello kimeneti adatkészlet meghajtók hello ütemezés hello folyamatának. hello folyamat fut havi a kezdő és záró időpont között. 

    Lásd: **létrehozni hello bemeneti adatkészletet** szakasz ezeket a tulajdonságokat leírását. Be kell állítani nem hello külső tulajdonság egy kimeneti adatkészletet, hello dataset hozzák hello folyamat.
4. Mentse a hello **OutputDataset.json** fájlt.

### <a name="create-pipeline"></a>Folyamat létrehozása
Hello Azure tárolás társított szolgáltatása, és a bemeneti és kimeneti adatkészletek eddig hozott létre. Most létrehozhat egy folyamatot egy **HDInsightHive**-tevékenységgel. Hello **bemeneti** a hello hive tevékenység túl van beállítva**AzureBlobInput** és **kimeneti** értéke túl**AzureBlobOutput**. A szelet egy bemeneti adatkészlet érhető el havonta (gyakoriság: hónap, időköz: 1), és hello kimeneti szelet jön létre havonta túl. 

1. A hello **Solution Explorer**, kattintson a jobb gombbal **folyamatok**, pont túl**hozzáadása**, és kattintson a **új cikk.**
2. Válassza ki **Hive átalakítási folyamat** hello listából, és kattintson a **Hozzáadás**.
3. Cserélje le a hello **JSON** a hello a következő kódrészletet:

    > [!IMPORTANT]
    > Cserélje le `<storageaccountname>` hello a tárfiók nevére.

    ```json
    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "AzureStorageLinkedService1",
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
            "start": "2016-04-01T00:00:00Z",
            "end": "2016-04-02T00:00:00Z",
            "isPaused": false
        }
    }
    ```

    > [!IMPORTANT]
    > Cserélje le `<storageaccountname>` hello a tárfiók nevére.

    hello JSON részlet definiál egy folyamatot, amely egy adott tevékenység (Hive tevékenység) áll. Ez a tevékenység a Hive parancsfájl tooprocess bemeneti adatok az igény szerinti HDInsight fürt tooproduce kimeneti adatok futtatja. Hello adatcsatorna JSON hello tevékenységek területen látható hello tömb csak egy tevékenység típusa túl a**HDInsightHive**. 

    A hello típusú tulajdonságokhoz adott tooHDInsight Hive tevékenység megadhatja, milyen Azure tárolás társított szolgáltatásának hello hive parancsfájl, a hello elérési toohello parancsfájlt és a paraméterek toohello parancsfájl rendelkezik. 

    hello Hive parancsfájl, **partitionweblogs.hql**, hello Azure storage-fiók (hello scriptLinkedService által megadott), és a hello tárolt `script` hello tároló mappa `adfgetstarted`.

    Hello `defines` szakaszban használt toospecify hello futtatási beállítások Hive értékként toohello hive parancsfájl átadott (például `${hiveconf:inputtable}`, `${hiveconf:partitionedtable})`.

    Hello **start** és **end** hello folyamatának tulajdonságok hello hello adatcsatorna aktív időszakát határozza meg. Konfigurált hello dataset toobe előállított havonta, ezért csak akkor, ha a szelet hozzák hello csővezeték (mivel ugyanazt a kezdő és befejező dátumok hello hónap).

    Hello tevékenység JSON-NÁ, meghatározza, hogy hello Hive parancsfájl futó hello által megadott hello számítási **linkedServiceName** – **HDInsightOnDemandLinkedService**.
4. Mentse a hello **HiveActivity1.json** fájlt.

### <a name="add-partitionweblogshql-and-inputlog-as-a-dependency"></a>A partitionweblogs.hql és az input.log fájl hozzáadása függőségként
1. Kattintson a jobb gombbal **függőségek** hello a **Solution Explorer** ablakban pont túl**Hozzáadás**, és kattintson a **a létező elemet**.  
2. Keresse meg a toohello **C:\ADFGettingStarted** válassza **partitionweblogs.hql**, **input.log** fájlokat, majd kattintson **Hozzáadás**. Ezeket a fájlokat két hello Előfeltételek részeként létrehozott [oktatóanyag – áttekintés](data-factory-build-your-first-pipeline.md).

Hello megoldás hello következő lépésben közzétételekor hello **partitionweblogs.hql** fájl feltöltött toohello **parancsfájl** hello mappájában `adfgetstarted` blob tároló.   

### <a name="publishdeploy-data-factory-entities"></a>Data Factory-entitások közzététele/üzembe helyezése
Ebben a lépésben közzététele hello adat-előállító entitások (összekapcsolt szolgáltatások adatkészletek és pipeline) a projekt toohello Azure Data Factory szolgáltatásnak. A közzététel folyamatban hello adja meg a data factory hello nevét. 

1. Kattintson a jobb gombbal a projektre a Solution Explorer hello, és kattintson a **közzététel**.
2. Ha látja **jelentkezzen be Microsoft-fiók tooyour** párbeszédpanelen adja meg Azure-előfizetéssel rendelkező hello fiók hitelesítő adatait, és kattintson a **bejelentkezés**.
3. A következő párbeszédpanel hello kell megjelennie:

   ![Publish (Közzététel) párbeszédpanel](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)
4. A hello **konfigurálása adat-előállító** lapján hello a következő lépéseket:

    ![Közzététel – Új data factory beállításai](media/data-factory-build-your-first-pipeline-using-vs/publish-new-data-factory.png)

   1. Válassza a **Create New Data Factory** (Új data factory létrehozása) lehetőséget.
   2. Adjon meg egy egyedi **neve** hello adat-előállító esetében. Például: **DataFactoryUsingVS09152016**. hello nevének globálisan egyedinek kell lennie.
   3. Válassza ki a megfelelő előfizetés hello hello **előfizetés** mező. 
        > [!IMPORTANT]
        > Ha nem látja bármely előfizetés, győződjön meg arról, hogy olyan fiókkal, amely egy rendszergazdai vagy társadminisztrátori hello előfizetés naplóba.
   4. Jelölje be hello **erőforráscsoport** a hello data factory toobe létrehozott.
   5. Jelölje be hello **régió** hello adat-előállító esetében.
   6. Kattintson a **következő** tooswitch toohello **elemek közzététele** lap. (Nyomja meg az **lapon** kívül hello neve mező tooif hello toomove **következő** gomb le van tiltva.)

    > [!IMPORTANT]
    > Ha hello hibaüzenet **nem érhető el adat-előállító "DataFactoryUsingVS"** közzétételekor, módosítsa a hello nevét (például yournameDataFactoryUsingVS). A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.   
1. A hello **elemek közzététele** lapon, győződjön meg arról, hogy az összes hello adat-előállítók entitások van kiválasztva, és kattintson a **következő** tooswitch toohello **összegzés** lap.

    ![Publish items (Elemek közzététele) oldal](media/data-factory-build-your-first-pipeline-using-vs/publish-items-page.png)     
2. Olvassa el az összesítő hello és a **következő** toostart hello központi telepítési folyamat és a nézet hello **központi telepítési állapot**.

    ![Összefoglaló lap](media/data-factory-build-your-first-pipeline-using-vs/summary-page.png)
3. A hello **központi telepítési állapot** lapon hello telepítési folyamatának állapotát hello kell megjelennie. Hello a telepítés befejezése után kattintson a Befejezés gombra.

Fontos pontok toonote:

- Ha hello hibaüzenetet kapja: **ehhez az előfizetéshez nincs regisztrált toouse névtér Microsoft.DataFactory**, hello alábbi, és próbálja meg újra közzétenni:
    - Azure PowerShell futtassa a következő parancs tooregister hello adat-előállító szolgáltató hello.
        ```PowerShell   
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
        ```
        A következő parancs tooconfirm hello futtathatja, hogy hello Data Factory szolgáltató regisztrálva van.

        ```PowerShell
        Get-AzureRmResourceProvider
        ```
    - Bejelentkezés használatával hello Azure-előfizetés toohello [Azure-portálon](https://portal.azure.com) , és keresse meg a tooa adat-előállító panel (vagy) hozzon létre egy adat-előállító hello Azure-portálon. Ez a művelet automatikusan regisztrálja az Ön hello szolgáltató.
- hello adat-előállító nevét hello előfordulhat, hogy jövőbeli hello a DNS-névként regisztrált, és ezért a nyilvánosan láthatóvá válnak.
- toocreate adat-előállító példányok, toobe rendszergazdája vagy szükséges hello Azure-előfizetés társadminisztrátornak

### <a name="monitor-pipeline"></a>Folyamat figyelése
Ebben a lépésben hello csővezeték hello adat-előállítót a Diagram nézet használata a figyelheti meg. 

#### <a name="monitor-pipeline-using-diagram-view"></a>Folyamat figyelése diagramnézetben
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/), hello a következő lépéseket:
   1. Kattintson a **További szolgáltatások**, majd az **Adat-előállítók** elemre.
       
        ![Data factoryk tallózása](./media/data-factory-build-your-first-pipeline-using-vs/browse-datafactories.png)
   2. A data factory válassza hello nevét (például: **DataFactoryUsingVS09152016**) adat-előállítók hello listája.
   
       ![A data factory kiválasztása](./media/data-factory-build-your-first-pipeline-using-vs/select-first-data-factory.png)
2. A data factory hello kezdőlapján kattintson **Diagram**.

    ![Diagram csempe](./media/data-factory-build-your-first-pipeline-using-vs/diagram-tile.png)
3. Hello Diagram nézet hello folyamatok, és ebben az oktatóanyagban használt adatkészletek áttekintése látható.

    ![Diagramnézet](./media/data-factory-build-your-first-pipeline-using-vs/diagram-view-2.png)
4. az összes tevékenység hello sorban, kattintson a jobb gombbal kimenetátirányítási mechanizmusával hello ábra, majd kattintson a nyitott folyamat tooview.

    ![Folyamat megnyitása menü](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-menu.png)
5. Győződjön meg arról, hogy megjelenik-e hello adatcsatorna hello HDInsightHive tevékenysége.

    ![Folyamat megnyitása nézet](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-view.png)

    toonavigate biztonsági toohello előző nézetével, kattintson a **adat-előállító** hello navigációs menüjében hello tetején.
6. A hello **diagramnézet**, kattintson duplán a hello dataset **AzureBlobInput**. Győződjön meg arról, hogy hello szelet a **készen** állapotát. Azt is tarthat néhány percet a hello szelet tooshow üzemkész állapotban. Ha nem így lenne után egy kis ideig, lásd: Ha hello bemeneti fájl (input.log) hello megfelelő tárolóban van (`adfgetstarted`) és a mappa (`inputdata`). És győződjön meg arról, hogy hello **külső** hello bemeneti adatkészlethez tulajdonsága túl**igaz**. 

   ![Kész állapotú bemeneti szelet](./media/data-factory-build-your-first-pipeline-using-vs/input-slice-ready.png)
7. Kattintson a **X** tooclose **AzureBlobInput** panelen.
8. A hello **diagramnézet**, kattintson duplán a hello dataset **AzureBlobOutput**. Láthatja, hogy hello szelet, amely folyamatban van.

   ![Adatkészlet](./media/data-factory-build-your-first-pipeline-using-vs/dataset-blade.png)
9. Ha nem hajtja végre, megjelenik az hello szeletre **készen** állapotát.

   > [!IMPORTANT]
   > Az igény szerinti HDInsight-fürt létrehozása általában eltart egy ideig (körülbelül 20 percig). Ezért várt hello csővezeték tootake **körülbelül 30 percet** tooprocess hello szelet.  
   
    ![Adatkészlet](./media/data-factory-build-your-first-pipeline-using-vs/dataset-slice-ready.png)    
10. Ha hello szelet belül van **készen áll a** állapot, ellenőrizze a hello `partitioneddata` hello mappájában `adfgetstarted` a blob-tároló hello tárolóhoz kimeneti adatokat.  

    ![kimeneti adatok](./media/data-factory-build-your-first-pipeline-using-vs/three-ouptut-files.png)
11. Hello szelet toosee részleteit a kattintson egy **adatszelet** panelen.

    ![Adatszelet részletei](./media/data-factory-build-your-first-pipeline-using-vs/data-slice-details.png)  
12. Hello futtatása tevékenység kattintson **tevékenység fut lista** toosee adatait egy tevékenység futtatását (esetünkben Hive tevékenység) egy **tevékenység fut részletek** ablak. 
  
    ![Tevékenységfuttatás részletei](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-blade.png)    

    Hello naplófájlokból láthatja, hogy végre lett hajtva hello Hive-lekérdezések és állapotára vonatkozó információkat. A naplók hasznosak bármilyen hiba esetén a hibaelhárításban.  

Lásd: [adatkészletek és a folyamat figyelése](data-factory-monitor-manage-pipelines.md) hogyan toouse hello Azure portál toomonitor hello feldolgozási sorban lévő és adatkészletek létrehozott ebben az oktatóanyagban kapcsolatos utasításokat.

#### <a name="monitor-pipeline-using-monitor--manage-app"></a>Folyamat figyelése a Monitor & Manage alkalmazással
Is figyelheti, & alkalmazás toomonitor kezelése a folyamatok. Az alkalmazás használatával kapcsolatos részletes információkért olvassa el a [Monitor and manage Azure Data Factory pipelines using Monitoring and Management App](data-factory-monitor-manage-app.md) (Azure Data Factory-folyamatok figyelése és felügyelete a Monitoring and Management használatával) című cikket.

1. Kattintson a Monitor & Manage csempére.

    ![Monitor & Manage csempe](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-tile.png)
2. Meg kell jelennie a Monitor & Manage alkalmazásnak. Változás hello **kezdési időpont** és **befejező időpontja** toomatch start (04-01-2016 12:00-kor) és befejezési időpontja (04-02-2016 12:00-kor) a feldolgozási sor, majd kattintson **alkalmaz**.

    ![Monitor & Manage alkalmazás](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-app.png)
3. toosee adatait egy tevékenység ablakban jelölje ki a hello **tevékenység Windows lista** toosee részleteit.
    ![Tevékenységablakok részletei](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-details.png)

> [!IMPORTANT]
> hello bemeneti fájl törlése hello szelet feldolgozása sikeresen megtörtént. Ezért, ha szeretné, hogy toorerun hello szelet, vagy újra hello oktatóanyag, feltöltése hello bemeneti fájl (input.log) toohello `inputdata` mappában található hello `adfgetstarted` tároló.

### <a name="additional-notes"></a>További megjegyzések
- A data factory egy vagy több folyamattal rendelkezhet. A folyamaton belül egy vagy több tevékenység lehet. Adja meg például a másolási tevékenység toocopy forrás tooa cél adattárat és adatait egy HDInsight Hive tevékenység toorun a Hive parancsfájl tootransform adatokat. Lásd: [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) az összes hello források és mosdók hello másolási tevékenység által támogatott. Lásd: [összekapcsolt szolgáltatások számítási](data-factory-compute-linked-services.md) a Data Factory által támogatott számítási szolgáltatás hello listája.
- Összekapcsolt szolgáltatások adattárolókhoz hivatkozásra, vagy a szolgáltatások tooan az Azure data factory számítási. Lásd: [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) az összes hello források és mosdók hello másolási tevékenység által támogatott. Lásd: [összekapcsolt szolgáltatások számítási](data-factory-compute-linked-services.md) a Data Factory által támogatott számítási szolgáltatás hello listája és [átalakítása tevékenységek](data-factory-data-transformation-activities.md) , futtathatja őket.
- Lásd: [helyezi át az adatokat a Blob-tooAzure /](data-factory-azure-blob-connector.md#azure-storage-linked-service) hello használt JSON-tulajdonságok vonatkozó további információért Azure Storage társított szolgáltatás definíciójának.
- Igény szerinti HDInsight-fürt helyett saját HDInsight-fürtöt is használhat. További információ: [Compute Linked Services](data-factory-compute-linked-services.md) (Számítási társított szolgáltatás).
-  hello adat-előállító létrehoz egy **Linux-alapú** rendelkező JSON megelőző hello meg HDInsight-fürthöz. További információkért lásd: [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).
- hello HDInsight-fürtöt hoz létre egy **alapértelmezett tároló** hello hello JSON (linkedServiceName) a megadott blob Storage tárolóban. HDInsight nem törli a tárolót hello fürt törlésekor. Ez a működésmód szándékos. Igény szerinti HDInsight társított szolgáltatás esetén a rendszer a szeletek feldolgozásakor mindig létrehoz egy HDInsight-fürtöt, kivéve, ha van meglévő élő fürt (timeToLive). hello feldolgozása hello fürt automatikusan törlődnek.
    
    Ahogy egyre több szelet lesz feldolgozva, egyre több tároló jelenik meg az Azure Blob Storage-tárban. Ha nem kell őket hello feladatok hibaelhárítási, érdemes lehet a toodelete őket tooreduce hello tárolási költségeket. ezekhez a tárolókhoz hello nevei, hajtsa végre a minta: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`. Használjon például az eszközök [Microsoft Tártallózó](http://storageexplorer.com/) toodelete tárolókat az az Azure blob-tároló.
- Kimeneti adatkészlet jelenleg milyen meghajtók hello ütemezését, így még akkor is, ha hello tevékenység nem ad kimenetet kell létrehoznia egy kimeneti adatkészletet. Ha hello tevékenység egyetlen bemeneti nem veszi, kihagyhatja létrehozása hello bemeneti adatkészletet. 
- Ez az oktatóanyag nem tartalmazza az adatok Azure Data Factory használatával történő másolásának leírását. Hogyan oktatóanyagért toocopy adatok Azure Data Factory használatával, lásd: [oktatóanyag: adatok másolása a Blob Storage tooSQL adatbázis](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


## <a name="use-server-explorer-tooview-data-factories"></a>Használja a Server Explorer tooview adat-előállítók
1. A **Visual Studio**, kattintson a **nézet** hello menü, és kattintson a **Server Explorer**.
2. Hello Server Explorer-ablakban bontsa ki a **Azure** csomópontot **adat-előállító**. Ha látja **tooVisual Studio bejelentkezés**, adja meg a hello **fiók** társított a Azure-előfizetéssel, és kattintson **Folytatás**. Adja meg a **jelszót**, és kattintson a **Sign in** (Bejelentkezés) elemre. A Visual Studio megpróbál az előfizetés az összes Azure adat-előállítók tooget információt. Ez a művelet hello hello állapotát látja **Data Factory feladatlista** ablak.

    ![Server Explorer (Kiszolgálókezelő)](./media/data-factory-build-your-first-pipeline-using-vs/server-explorer.png)
3. Kattintson a jobb gombbal egy adat-előállítót, és válassza ki **exportálása adat-előállító tooNew projekt** toocreate Visual Studio-projekt valamelyik adat-előállítót alapján.

    ![Export data factory (Data factory exportálása) menüelem](./media/data-factory-build-your-first-pipeline-using-vs/export-data-factory-menu.png)

## <a name="update-data-factory-tools-for-visual-studio"></a>Visual Studióhoz készült Data Factory-eszközök frissítése
tooupdate Azure Data Factory tools for Visual Studio hello a következő lépéseket:

1. Kattintson a **eszközök** hello menüre, majd válassza a **bővítmények és frissítések**.
2. Válassza ki **frissítések** a hello bal oldali ablaktáblán, és válassza ki **Visual Studio galériában**.
3. Válassza az **Azure Data Factory tools for Visual Studio** lehetőséget, és kattintson az **Update** (Frissítés) elemre. Ez a bejegyzés nem látható, ha már rendelkezik hello hello eszközök legújabb verzióját.

## <a name="use-configuration-files"></a>Konfigurációs fájlok használata
Minden környezet eltérően a szolgáltatások/táblák/folyamatok csatolt konfigurációs fájlokat a Visual Studio tooconfigure tulajdonságai használható.

Vegye figyelembe a következő az Azure tárolás társított szolgáltatásának JSON-definícióból hello. toospecify **connectionString** accountname és az accountkey elemeket hello (fejlesztési/tesztelési/éles) környezetben toowhich alapján különböző értékekkel a Data Factory entitások telepít. Az ilyen működést úgy érheti el, hogy minden környezethez külön konfigurációs fájlt használ.

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "description": "",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

### <a name="add-a-configuration-file"></a>Konfigurációs fájl hozzáadása
Minden környezet konfigurációs fájl felvétele hello lépések végrehajtásával:   

1. Hello adat-előállító projektre a Visual Studio megoldásnak a jobb gombbal túl**Hozzáadás**, és kattintson a **új elem**.
2. Válassza ki **Config** hello bal oldali telepített sablonok hello listában jelölje ki **konfigurációs fájl**, adjon meg egy **neve** hello konfiguráció fájlt, és kattintson a **Hozzáadása**.

    ![Konfigurációs fájl hozzáadása](./media/data-factory-build-your-first-pipeline-using-vs/add-config-file.png)
3. Adja hozzá a következő formátumban hello konfigurációs paramétereit és azok értékét:

    ```json
    {
        "$schema": "http://datafactories.schema.management.azure.com/vsschemas/V1/Microsoft.DataFactory.Config.json",
        "AzureStorageLinkedService1": [
            {
                "name": "$.properties.typeProperties.connectionString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        ],
        "AzureSqlLinkedService1": [
            {
                "name": "$.properties.typeProperties.connectionString",
                "value":  "Server=tcp:spsqlserver.database.windows.net,1433;Database=spsqldb;User ID=spelluru;Password=Sowmya123;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        ]
    }
    ```

    Ez a példa konfigurálja egy Azure Storage társított szolgáltatás és egy Azure SQL társított szolgáltatás connectionString tulajdonságát. Figyelje meg, hogy hello nevét megadó szintaxisa a következő [JsonPath](http://goessner.net/articles/JsonPath/).   

    Ha JSON, amely rendelkezik egy tömböt, ahogy az a következő kód hello tulajdonsággal rendelkezik:  

    ```json
    "structure": [
          {
              "name": "FirstName",
            "type": "String"
          },
          {
            "name": "LastName",
            "type": "String"
        }
    ],
    ```

    Ahogy az következő konfigurációs fájl (használata nulla alapú indexelést) hello tulajdonságainak konfigurálása:

    ```json
    {
        "name": "$.properties.structure[0].name",
        "value": "FirstName"
    }
    {
        "name": "$.properties.structure[0].type",
        "value": "String"
    }
    {
        "name": "$.properties.structure[1].name",
        "value": "LastName"
    }
    {
        "name": "$.properties.structure[1].type",
        "value": "String"
    }
    ```

### <a name="property-names-with-spaces"></a>Tulajdonságnevek szóközökkel
Ha egy tulajdonság neve szóközöket tartalmaz, használja a kapcsos zárójeleket látható módon a következő példa (adatbázis-kiszolgáló neve) hello:

```json
 {
     "name": "$.properties.activities[1].typeProperties.webServiceParameters.['Database server name']",
     "value": "MyAsqlServer.database.windows.net"
 }
```

### <a name="deploy-solution-using-a-configuration"></a>Megoldás üzembe helyezése konfiguráció használatával
Ha az Azure Data Factory entitások tesznek közzé a Visual STUDIO, hello konfigurációs toouse használandó közzétételi művelet is megadhat.

a konfigurációs fájl használata az Azure Data Factory-projektek entitások toopublish:   

1. Kattintson a jobb gombbal a Data Factory-projektet, és kattintson a **közzététel** toosee hello **elemek közzététele** párbeszédpanel megnyitásához.
2. Válassza ki valamelyik adat-előállítót, vagy adjon meg egy adat-előállító létrehozása a hello értékeinek **konfigurálása adat-előállító** lapon, majd kattintson **következő**.   
3. A hello **elemek közzététele** lap: megjelenik egy legördülő listából válassza ki az elérhető konfigurációk hello **a telepítési konfiguráció kiválasztása** mező.

    ![Konfigurációs fájl kiválasztása](./media/data-factory-build-your-first-pipeline-using-vs/select-config-file.png)
4. Jelölje be hello **konfigurációs fájl** , hogy kívánja toouse, majd kattintson a **következő**.
5. Ellenőrizze, hogy látható-e a hello JSON-fájl neve hello **összegzés** lapot, és kattintson **következő**.
6. Kattintson a **Befejezés** hello központi telepítési művelet befejezése után.

Üzembe helyezésekor hello hello konfigurációs fájlból származó értékek használt tooset értékek hello JSON-fájlok tulajdonságok előtt hello entitások telepített tooAzure Data Factory szolgáltatásnak.   

## <a name="use-azure-key-vault"></a>Az Azure Key Vault használata
Már nem ajánlott, és gyakran biztonsági házirend toocommit érzékeny adatok, például kapcsolati karakterláncok toohello kód tárház ellen. Lásd: [ADF biztonságos közzétételéhez](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFSecurePublish) mintát a Githubon toolearn bizalmas adatok tárolása az Azure Key Vault és használhassa a Data Factory entitások közzététele közben. hello bővítmény biztonságos közzététele a Visual Studio lehetővé teszi, hogy a hello titkok toobe Key Vault tárolja, és csak a hivatkozások toothem meg van határozva a társított szolgáltatások / szolgáltatástelepítési konfigurációk. Ha közzéteszi a Data Factory entitások tooAzure feloldása az ezeket a hivatkozásokat. Ezek a fájlok majd lehet véglegesíteni toosource tárház anélkül, hogy a titkos kulcsok.

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

Hello kimeneti adatkészlet egy tevékenység beállítását hello bemeneti hello az adatkészlet többi tevékenység által láncolt lehessen két tevékenység (egy tevékenység egymás után). Lásd [a Data Factorybeli ütemezést és végrehajtást](data-factory-scheduling-and-execution.md) ismertető cikket. 


## <a name="see-also"></a>Lásd még:
| Témakör | Leírás |
|:--- |:--- |
| [Folyamatok](data-factory-create-pipelines.md) |Ez a cikk segít megérteni a folyamatok és az Azure Data Factory tevékenységeket, és hogyan toouse tooconstruct adatvezérelt munkafolyamatok a forgatókönyv vagy üzleti őket. |
| [Adatkészletek](data-factory-create-datasets.md) |Ennek a cikknek a segítségével megismerheti az adatkészleteket az Azure Data Factoryban. |
| [Adatátalakítási tevékenységek](data-factory-data-transformation-activities.md) |Ez a cikk felsorolja az Azure Data Factory által támogatott adatátalakítási tevékenységeket (mint például a jelen oktatóanyagban használt HDInsight Hive-átalakítás). |
| [Ütemezés és végrehajtás](data-factory-scheduling-and-execution.md) |Ez a cikk ismerteti az Azure Data Factory alkalmazásmodell hello ütemezés és a végrehajtási szempontjait. |
| [Folyamatok figyelése és felügyelete a Monitoring App használatával](data-factory-monitor-manage-app.md) |Ez a cikk ismerteti, hogyan toomonitor, kezelése és hibakeresése folyamatok használatával hello figyelés & a felügyeleti alkalmazás. |
