---
title: "Oktatóanyag: Hozzon létre egy Azure Data Factory adatcsatorna toocopy adatok (Azure-portál) |} Microsoft Docs"
description: "Ebben az oktatóanyagban az Azure portál toocreate egy Azure Data Factory-folyamat a másolási tevékenység toocopy adatait az Azure blob storage tooan Azure SQL-adatbázis használata."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: d9317652-0170-4fd3-b9b2-37711272162b
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: fadd840fe9a15cd8831cdb25dccbd10ac42fa161
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-use-azure-portal-toocreate-a-data-factory-pipeline-toocopy-data"></a>Oktatóanyag: Használja az Azure portál toocreate egy adat-előállító adatcsatorna toocopy adatok 
> [!div class="op_single_selector"]
> * [Áttekintés és előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Másolás varázsló](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Azure Resource Manager-sablon](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

Ebből a cikkből megismerheti, hogyan toouse [Azure-portálon](https://portal.azure.com) toocreate egy adat-előállítót, és egy folyamatot, amely másol adatokat az Azure blob storage tooan Azure SQL-adatbázis. Ha új tooAzure adat-előállítót, olvassa végig hello [Data Factory bemutatása tooAzure](data-factory-introduction.md) cikk Ez az oktatóanyag elvégzése előtt.   

Az oktatóanyag segítségével egyetlen tevékenységgel (másolási tevékenységgel) rendelkező folyamatot hozhat létre. hello másolási tevékenység során a támogatott adatokat tároló tooa támogatott fogadó adatokat tároló másol adatokat. A forrásként és fogadóként támogatott adattárak listájáért lásd: [támogatott adattárak](data-factory-data-movement-activities.md#supported-data-stores-and-formats). hello tevékenység egy globálisan elérhető szolgáltatás közötti biztonságos, megbízható és skálázható módon különböző adattárolókhoz másolhatja van-e kapcsolva. További információ a másolási tevékenység hello: [adatok mozgása tevékenységek](data-factory-data-movement-activities.md).

Egy folyamathoz több tevékenység is tartozhat. És láncolt telepítését (egymás után futtatni egy tevékenységet) két tevékenység által hello bemeneti hello az adatkészlet többi tevékenység hello kimeneti adatkészlet egy tevékenység beállítását. További információért lásd: [egy folyamaton belüli több tevékenység](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline). 

> [!NOTE] 
> hello adatok feldolgozási sor az oktatóanyag a forrás adatokat tároló tooa cél adatokat tároló másol adatokat. Hogyan oktatóanyagért tootransform adatok Azure Data Factory használatával, lásd: [oktatóanyag: a folyamat tootransform adatokat Hadoop-fürt létrehozása](data-factory-build-your-first-pipeline.md).

## <a name="prerequisites"></a>Előfeltételek
Végezze el a témakörben ismertetett előfeltételeknek: hello [oktatóanyag előfeltételei](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) cikk Ez az oktatóanyag végrehajtása előtt.

## <a name="steps"></a>Lépések
Ez az oktatóanyag részeként végrehajtandó hello lépések a következők:

1. Hozzon létre egy folyamatot az **adat-előállítóban**. Ebben a lépésben egy adat-előállítót hoz létre ADFTutorialDataFactory néven. 
2. Hozzon létre **összekapcsolt szolgáltatások** hello adat-előállítóban. Ebben a lépésben a következő két típusú társított szolgáltatást hozza létre: Azure Storage és Azure SQL Database. 
    
    hello AzureStorageLinkedService művelet ugyan összeköti a az Azure storage-fiók toohello adat-előállítóban. Létrejött a tároló és részeként feltöltött adatok toothis tárfiók [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   

    AzureSqlLinkedService az Azure SQL adatbázis toohello adat-előállító hivatkozásokat tartalmaz. hello blobtárolóból másolt hello adatok az adatbázis tárolja. Az [előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részeként létrehozta az SQL-táblát az adatbázisban.   
3. Hozzon létre a bemeneti és kimeneti **adatkészletek** hello adat-előállítóban.  
    
    hello Azure tárolás társított szolgáltatásának határozza meg a Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure storage-fiók használó hello kapcsolati karakterláncot. És hello bemeneti blob-adathalmazra hello tároló és hello bemeneti adatokat tartalmazó hello mappát adja meg.  

    Hello csatolt Azure SQL Database szolgáltatáshoz hasonlóan hello kapcsolati karakterláncot használó Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure SQL adatbázis határozza meg. És hello kimeneti SQL táblázat dataset megadja hello adatbázis toowhich hello adatokat a hello blob storage hello táblájában másolódik.
4. Hozzon létre egy **csővezeték** hello adat-előállítóban. Ebben a lépésben létre fog hozni egy másolási tevékenységgel rendelkező folyamatot.   
    
    hello másolási tevékenység hello Azure blob storage tooa tábla hello Azure SQL-adatbázis egy blobot másol adatokat. A másolási tevékenység használható egy folyamat toocopy adatokat bármely támogatott forrás támogatott tooany cél. A támogatott adattárak listájáért lásd: [adatáthelyezési tevékenységek](data-factory-data-movement-activities.md#supported-data-stores-and-formats). 
5. A figyelő hello folyamat. Ebben a lépésben meg **figyelő** hello szeletek bemeneti és kimeneti adathalmazokat az Azure portál használatával. 

## <a name="create-data-factory"></a>Data factory létrehozása
> [!IMPORTANT]
> Teljes [hello oktatóanyag előfeltételei](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) Ha még nem tette meg.   

A data factory egy vagy több folyamattal rendelkezhet. A folyamaton belül egy vagy több tevékenység lehet. Adja meg például a másolási tevékenység toocopy forrás tooa cél adattárat és adatait egy HDInsight Hive tevékenység toorun a Hive parancsfájl tootransform adatok tooproduct kimeneti adatokat. Kezdjük, az ebben a lépésben hello adat-előállító létrehozása.

1. Toohello a bejelentkezés után [Azure-portálon](https://portal.azure.com/), kattintson a **új** hello bal oldali menüben kattintson **adatok + analitika**, és kattintson a **adat-előállító**. 
   
   ![New (Új)->DataFactory](./media/data-factory-copy-activity-tutorial-using-azure-portal/NewDataFactoryMenu.png)    
2. A hello **új adat-előállító** panel:
   
   1. Adja meg **ADFTutorialDataFactory** a hello **neve**. 
      
         ![A New data factory (Új data factory) panel](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-new-data-factory.png)
      
       az Azure data factory hello hello nevének kell lennie **globálisan egyedi**. Hiba a következő hello megjelenésekor hello data factory (például yournameADFTutorialDataFactory), és próbálja meg újra létrehozni hello nevének módosítása. A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.
      
           Data factory name “ADFTutorialDataFactory” is not available  
      
       ![A Data Factory name not available (A data factory neve nem érhető el) üzenet](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-not-available.png)
   2. Válassza ki az Azure **előfizetés** kívánt toocreate hello adat-előállítóban. 
   3. A hello **erőforráscsoport**, hajtsa végre az alábbi lépésekkel hello:
      
      - Válassza ki **meglévő**, és válasszon ki egy meglévő erőforráscsoportot hello legördülő listából. 
      - Válassza ki **hozzon létre új**, és adja meg egy erőforráscsoportot hello nevét.   
         
          Egyes ebben az oktatóanyagban hello lépések azt feltételezik, hogy, hogy hello nevét használja: **ADFTutorialResourceGroup** hello erőforráscsoport. tudnivalók az erőforráscsoportokról, toolearn lásd [erőforrás segítségével csoportosítja toomanage az Azure-erőforrások](../azure-resource-manager/resource-group-overview.md).  
   4. Jelölje be hello **hely** hello adat-előállító esetében. Csak a Data Factory szolgáltatásnak hello által támogatott régiók hello legördülő listában jelennek meg.
   5. Válassza ki **PIN-kód toodashboard**.     
   6. Kattintson a **Create** (Létrehozás) gombra.
      
      > [!IMPORTANT]
      > toocreate adat-előállító példányok hello tagjának kell lennie [Data Factory közreműködői](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) szerepkör hello előfizetés-erőforráscsoport szintjén.
      > 
      > hello adat-előállító nevét hello előfordulhat, hogy jövőbeli hello a DNS-névként regisztrált, és ezért a nyilvánosan láthatóvá válnak.                
      > 
      > 
3. Hello irányítópult állapotú csempe hello következő látja: **Deploying adat-előállító**. 

    ![adat-előállító üzembe helyezése csempe](media/data-factory-copy-activity-tutorial-using-azure-portal/deploying-data-factory.png)
4. Hello létrehozásának befejezése után megjelenik a hello **adat-előállító** panel hello ábrán látható módon.
   
   ![Data factory kezdőlap](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-home-page.png)

## <a name="create-linked-services"></a>Társított szolgáltatások létrehozása
A data factory toolink adatait tárolja, és számítási szolgáltatások toohello adat-előállító létrehozása társított szolgáltatások. Ebben az oktatóanyagban nem használunk számítási szolgáltatásokat (például Azure HDInsight vagy Azure Data Lake Analytics). Csak kétféle típusú adattárat használunk: Azure Storage (forrás) és Azure SQL Database (cél). 

Ezért két társított szolgáltatást fog létrehozni AzureStorageLinkedService és AzureSqlLinkedService néven (típus: AzureStorage és AzureSqlDatabase).  

hello AzureStorageLinkedService művelet ugyan összeköti a az Azure storage-fiók toohello adat-előállítóban. Ez a tárfiók egy hello akkor létrejött a tároló és hello adatok részeként feltöltött [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   

AzureSqlLinkedService az Azure SQL adatbázis toohello adat-előállító hivatkozásokat tartalmaz. hello blobtárolóból másolt hello adatok az adatbázis tárolja. Hello üres tábla az adatbázisban részeként létrehozott [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).  

### <a name="create-azure-storage-linked-service"></a>Azure Storage társított szolgáltatás létrehozása
Ebben a lépésben a az Azure storage-fiók tooyour adat-előállító hivatkozásra. Ebben a szakaszban megadhatja hello nevét és az Azure storage-fiók kulcsát.  

1. A hello **adat-előállító** panelen kattintson a **Szerző és központi telepítése** csempére.
   
   ![Az Author and deploy (Fejlesztés és üzembe helyezés) csempe](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-author-deploy-tile.png) 
2. Megjelenik a hello **Data Factory Editor** látható hello kép a következő módon: 

    ![A Data Factory szerkesztője](./media/data-factory-copy-activity-tutorial-using-azure-portal/data-factory-editor.png)
3. Hello-szerkesztőben kattintson **az új adattároló** hello eszköztár, és válassza a gomb **az Azure storage** hello legördülő menüből. Meg kell jelennie hello JSON-sablon létrehozásához az Azure tárolás társított szolgáltatásának hello jobb oldali ablaktáblán. 
   
    ![A szerkesztő New data store (Új adattár) gombja](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-newdatastore-button.png)    
3. Cserélje le `<accountname>` és `<accountkey>` kulcsértékekkel hello fiók nevét és a fiók az Azure-tárfiókot. 
   
    ![Szerkesztő – A blobtároló JSON-fájlja](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-json.png)    
4. Kattintson a **telepítés** hello eszköztáron. Megtekintheti az telepített hello **AzureStorageLinkedService** hello fa megtekintése most. 
   
    ![Szerkesztő – A blobtároló üzembe helyezése](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-deploy.png)

    JSON-tulajdonságokat hello társított szolgáltatás definíciójának kapcsolatos további információkért lásd: [Azure Blob Storage összekötő](data-factory-azure-blob-connector.md#linked-service-properties) cikk.

### <a name="create-a-linked-service-for-hello-azure-sql-database"></a>Az Azure SQL Database hello társított szolgáltatás létrehozása
Ebben a lépésben az Azure SQL adatbázis tooyour adat-előállító hivatkozásra. Ebben a szakaszban megadhatja hello Azure SQL-kiszolgáló neve, az adatbázis nevét, a felhasználónév és a felhasználó jelszavát. 

1. A hello **Data Factory Editor**, kattintson a **az új adattároló** gombra hello eszköztár, és válassza a **Azure SQL Database** hello legördülő menüből. Meg kell jelennie a JSON-hello sablonját hello Azure SQL társított szolgáltatás létrehozása hello jobb oldali ablaktáblán.
2. Cserélje le a `<servername>`, `<databasename>`, `<username>@<servername>` és `<password>` paraméter értékét az Azure SQL-kiszolgáló, az adatbázis és a felhasználói fiók nevére, valamint a felhasználói fiók jelszavára. 
3. Kattintson a **telepítés** eszköztár toocreate hello és központi telepítése hello **AzureSqlLinkedService**.
4. Ellenőrizze, hogy látható **AzureSqlLinkedService** hello faszerkezetes nézetben a **összekapcsolt szolgáltatások**.  

    További információ a JSON-tulajdonságokról: [Azure SQL Database-összekötő](data-factory-azure-sql-connector.md#linked-service-properties).

## <a name="create-datasets"></a>Adatkészletek létrehozása
Hello előző lépésben létrehozott összekapcsolt szolgáltatások toolink az Azure Storage-fiók és az Azure SQL adatbázis tooyour adat-előállítóban. Ebben a lépésben InputDataset és OutputDataset, amelyek megfelelnek a bemeneti és kimeneti adatok AzureStorageLinkedService és AzureSqlLinkedService által hivatkozott hello adattárolókhoz tárolt nevű két adatkészletet határozza meg.

hello Azure tárolás társított szolgáltatásának határozza meg a Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure storage-fiók használó hello kapcsolati karakterláncot. És hello bemeneti blob-adathalmazra (InputDataset) hello tároló és hello bemeneti adatokat tartalmazó hello mappát adja meg.  

Hello csatolt Azure SQL Database szolgáltatáshoz hasonlóan hello kapcsolati karakterláncot használó Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure SQL adatbázis határozza meg. És hello kimeneti SQL táblázat dataset (OututDataset) hello adatbázis toowhich hello hello blobtárolóból az adatok másolásakor hello tábla határozza meg. 

### <a name="create-input-dataset"></a>Bemeneti adatkészlet létrehozása
Ebben a lépésben hoz létre, mely tooa fájlját (emp.txt) InputDataset nevű adatkészlete hello gyökérmappájában lévő mappának a blob-tároló (adftutorial) az Azure Storage hello AzureStorageLinkedService kapcsolódó szolgáltatás által képviselt hello. Ha nem hello fájlnév értéket (vagy hagyja ki), a hello bemeneti mappában található összes BLOB adatait is másolt toohello cél. Ebben az oktatóanyagban hello fájlnév értéket kell megadni. 

1. A hello **szerkesztő** hello adat-előállítót, kattintson a **... További**, kattintson a **új adatkészlet**, és kattintson a **Azure Blob Storage tárolóban** hello legördülő menüből. 
   
    ![Új adatkészlet menü](./media/data-factory-copy-activity-tutorial-using-azure-portal/new-dataset-menu.png)
2. Cserélje le a következő JSON részlet hello hello jobb oldali ablaktáblában JSON: 
   
    ```json
    {
      "name": "InputDataset",
      "properties": {
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
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
          "folderPath": "adftutorial/",
          "fileName": "emp.txt",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }
    ```   

    hello következő táblázat ismerteti hello részlet használt hello JSON-tulajdonságok:

    | Tulajdonság | Leírás |
    |:--- |:--- |
    | type | hello type tulajdonság beállítása túl**AzureBlob** , mert az adatok találhatók az Azure blob Storage tárolóban. |
    | linkedServiceName | Toohello hivatkozik **AzureStorageLinkedService** korábban létrehozott. |
    | folderPath | Adja meg a hello blob **tároló** és hello **mappa** , amely tartalmazza a bemeneti BLOB. Ebben az oktatóanyagban adftutorial hello blobtárolót és hello legfelső szintű mappa. | 
    | fileName | Ez a tulajdonság nem kötelező. Ha kihagyja ezt a tulajdonságot, leltárhoz hello folderPath lévő összes fájlt. Ebben az oktatóanyagban **emp.txt** hello fájlnév, hogy csak adott fájl van felvételre feldolgozásra számára megadott. |
    | formátum -> típus |hello bemeneti fájl hello szöveg formátumban van, így használjuk **szöveges**. |
    | columnDelimiter | hello bemeneti fájl hello oszlopai határolja **vesszővel karakter (`,`)**. |
    | frequency/interval | hello gyakoriságának beállítása túl**óra** és időköz értéke túl**1**, ami azt jelenti, hogy hello bemeneti szeletek érhetők el **óránkénti**. Más szóval hello Data Factory szolgáltatásnak megkeresi a bemeneti adatok óránként blob tároló hello gyökérmappájában (**adftutorial**) megadott. Hello adatainak hello folyamat kezdési és befejezési időpontokat, nem előtt vagy után ezekben az időszakokban keresi.  |
    | external | Ez a tulajdonság értéke túl**igaz** hello adatok nem jön létre, ez az adatcsatorna. Ebben az oktatóanyagban hello a bemeneti adatok nem jön létre a folyamat, így ez a tulajdonság tootrue hivatott hello emp.txt fájl van. |

    Ezekről a JSON-tulajdonságokról további tudnivalók az [Azure Blob-összekötőről](data-factory-azure-blob-connector.md#dataset-properties) szóló cikkben olvashatók.      
3. Kattintson a **telepítés** eszköztár toocreate hello és központi telepítése hello **InputDataset** adatkészlet. Ellenőrizze, hogy látható-e hello **InputDataset** hello faszerkezetes nézetben.

### <a name="create-output-dataset"></a>Kimeneti adatkészlet létrehozása
hello csatolt Azure SQL Database szolgáltatás hello kapcsolati karakterláncot használó Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure SQL adatbázis határozza meg. hello kimeneti SQL táblázat dataset (OututDataset) hoz létre ebben a lépésben meghatározza hello adatbázis toowhich hello adatokat a hello blob storage hello táblájában másolódik.

1. A hello **szerkesztő** hello adat-előállítót, kattintson a **... További**, kattintson a **új adatkészlet**, és kattintson a **Azure SQL** hello legördülő menüből. 
2. Cserélje le a következő JSON részlet hello hello jobb oldali ablaktáblában JSON:

    ```json   
    {
      "name": "OutputDataset",
      "properties": {
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
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "emp"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }
    ```     

    hello következő táblázat ismerteti hello részlet használt hello JSON-tulajdonságok:

    | Tulajdonság | Leírás |
    |:--- |:--- |
    | type | hello type tulajdonság beállítása túl**AzureSqlTable** mert adatok másolt tooa tábla Azure SQL-adatbázisban. |
    | linkedServiceName | Toohello hivatkozik **AzureSqlLinkedService** korábban létrehozott. |
    | tableName | Megadott hello **tábla** toowhich hello adatok másolását. | 
    | frequency/interval | hello gyakoriságának beállítása túl**óra** és időköz **1**, ami azt jelenti, hogy hello kimeneti szeletek előállítása **óránkénti** közötti hello folyamat kezdési és befejezési időpontokat, előtte vagy Miután ezekben az időszakokban.  |

    Három oszlop – **azonosító**, **Keresztnév**, és **Vezetéknév** – hello üres tábla hello adatbázisban. Azonosító: azonosító oszlopot, ezért meg kell, hogy csak toospecify **Keresztnév** és **Vezetéknév** itt.

    További információ ezekről a JSON-tulajdonságokról: [Azure SQL-összekötő](data-factory-azure-sql-connector.md#dataset-properties).
3. Kattintson a **telepítés** eszköztár toocreate hello és központi telepítése hello **OutputDataset** adatkészlet. Ellenőrizze, hogy látható-e hello **OutputDataset** hello faszerkezetes nézetben a **adatkészletek**. 

## <a name="create-pipeline"></a>Folyamat létrehozása
Ebben a lépésben létrehoz egy **másolási tevékenységgel** rendelkező folyamatot, amely bemenetként az **InputDataset**, kimenetként pedig az **OutputDataset** adatkészletet használja.

Kimeneti adatkészlet jelenleg milyen meghajtók hello ütemezést. Ebben az oktatóanyagban a kimeneti adatkészlet óránként konfigurált tooproduce szelet. hello folyamat rendelkezik egy kezdési és befejezési időpontja, amelyek egy nap telhet el, amely 24 óra. Ezért a kimeneti adatkészlet 24 szeletek előállított hello folyamat. 

1. A hello **szerkesztő** hello adat-előállítót, kattintson a **... Továbbiak**, majd az **Új adatcsatorna** elemre. Másik lehetőségként kattintson a jobb egérgombbal **folyamatok** hello fanézetben, és kattintson a **új adatcsatorna**.
2. Cserélje le a következő JSON részlet hello hello jobb oldali ablaktáblában JSON: 

    ```json   
    {
      "name": "ADFTutorialPipeline",
      "properties": {
        "description": "Copy data from a blob tooAzure SQL table",
        "activities": [
          {
            "name": "CopyFromBlobToSQL",
            "type": "Copy",
            "inputs": [
              {
                "name": "InputDataset"
              }
            ],
            "outputs": [
              {
                "name": "OutputDataset"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "SqlSink",
                "writeBatchSize": 10000,
                "writeBatchTimeout": "60:00:00"
              }
            },
            "Policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
        ],
        "start": "2017-05-11T00:00:00Z",
        "end": "2017-05-12T00:00:00Z"
      }
    } 
    ```   
    
    Vegye figyelembe a következő pontok hello:
   
    - Hello tevékenységek szakaszban csak egy tevékenység nincs amelynek **típus** értéke túl**másolási**. Hello másolási tevékenység kapcsolatos további információkért lásd: [adatok mozgása tevékenységek](data-factory-data-movement-activities.md). A Data Factory megoldásaiban használhatja az [adatátalakítási tevékenységeket](data-factory-data-transformation-activities.md) is.
    - Adjon meg a hello tevékenység értéke túl**InputDataset** és hello tevékenység túl van-e állítva a kimeneti**OutputDataset**. 
    - A hello **typeProperties** szakaszban **BlobSource** hello forrás típusaként van megadva, és **SqlSink** hello a fogadó típusa van megadva. Források és mosdók hello másolási tevékenység által támogatott adattárolókhoz teljes listáját lásd: [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Hogyan toouse a támogatott adatokat tárolót, mint a forrás/fogadó toolearn hivatkozásra hello hello táblában.
    - Mind a kezdő, mind a befejező dátum-időpont értéket [ISO formátumban](http://en.wikipedia.org/wiki/ISO_8601) kell megadni. Például: 2016-10-14T16:32:41Z. Hello **end** idő megadása nem kötelező, de ebben az oktatóanyagban használjuk. Ha nem ad meg értéket a hello **end** tulajdonságot, akkor a program "**kezdés + 48 óra**". toorun hello folyamat határozatlan ideig, adja meg **9999-09-09** hello hello értékként **end** tulajdonság.
     
    A fenti példa hello amelyeket 24 adatszeletek minden adatszelet rendszer óránként készít adatszeletet.

    A folyamathoz tartozó definíció JSON-tulajdonságainak leírásáért lásd: [folyamatok létrehozása](data-factory-create-pipelines.md). A másolási tevékenységhez tartozó definíció JSON-tulajdonságainak leírásáért lásd: [adatáthelyezési tevékenységek](data-factory-data-movement-activities.md). A BlobSource által támogatott JSON-tulajdonságok leírásáért lásd: [Azure Blob-összekötő](data-factory-azure-blob-connector.md). Az SqlSink által támogatott JSON-tulajdonságok leírásáért lásd: [Azure SQL Database-összekötő](data-factory-azure-sql-connector.md).
3. Kattintson a **telepítés** eszköztár toocreate hello és központi telepítése hello **ADFTutorialPipeline**. Ellenőrizze, hogy látható-e hello fanézetben hello csővezeték-e. 
4. Most, zárja be a hello **szerkesztő** panelre. Ehhez kattintson **X**. Kattintson a **X** újra toosee hello **adat-előállító** hello kezdőlapját **ADFTutorialDataFactory**.

**Gratulálunk!** Sikeresen létrehozott egy az Azure data factory egy folyamat toocopy Azure blob storage tooan Azure SQL-adatbázis adatait. 


## <a name="monitor-pipeline"></a>Folyamat figyelése
Ebben a lépésben az Azure data factory lesz az Azure portál toomonitor hello használja.    

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Folyamat figyelése a Monitor & Manage alkalmazással
hello következő lépések bemutatják, hogyan toomonitor folyamatok az adat-előállítóban hello figyelő & kezelése alkalmazással: 

1. Kattintson a **figyelő & kezelése** hello kezdőlap a data factory csempére.
   
    ![Monitor & Manage csempe](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-manage-tile.png) 
2. A **Monitor & Manage alkalmazásnak** egy külön lapon kell megjelennie. 

    > [!NOTE]
    > Ha adott hello webböngésző akadt-e a "Engedélyező..." című hello következő módokon: törölje a jelet hello **külső cookie-k blokkolását, és a helyadatok** jelölőnégyzetet (vagy) hozzon létre egy kivételt **login.microsoftonline.com**, és próbálkozzon újra a tooopen hello alkalmazást.

    ![Monitor & Manage alkalmazás](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-and-manage-app.png)
3. Változás hello **kezdési időpont** és **befejező időpontja** tooinclude indítsa el a (2017-05-11) és befejezési időpontja (2017-05-12) a feldolgozási folyamat, és kattintson a **alkalmaz**.     
3. Megjelenik a hello **tevékenység windows** közötti adatcsatorna kezdő és záró óránként társított hello középső ablaktábla listájában hello alkalommal. 
4. egy tevékenység ablakban válassza ki toosee adatait hello hello tevékenység ablakában **tevékenység Windows** listája. 
    ![Tevékenységablakok részletei](./media/data-factory-copy-activity-tutorial-using-azure-portal/activity-window-details.png)

    Tevékenység jobb hello Explorer ablak, látja, hogy hello szeletek toohello jelenlegi UTC idő (8:12 óra) összes feldolgozása (zöld színnel). hello 8 - PM, 9-10 PM, 10 – 11 PM, 23 óra - 12 Reggel 9 szeletek még nincsenek feldolgozva.

    Hello **kísérletek** hello jobb oldali ablaktáblán tevékenységről szolgáltat információkat hello hello adatszelet Futtatás szakasz. Hiba történt, mégis hello hiba részleteit. Például ha hello bemeneti mappa vagy a tároló nem létezik, hello szelet feldolgozása sikertelen, egy tároló hello feltüntetve hibaüzenet jelenik meg, vagy mappa nem létezik.

    ![Tevékenységfuttatási kísérletek](./media/data-factory-copy-activity-tutorial-using-azure-portal/activity-run-attempts.png) 
4. Indítsa el **SQL Server Management Studio**, csatlakozzon az Azure SQL Database toohello, és győződjön meg arról, hogy hello sorok egészül ki toohello **üres** hello adatbázis táblájában.
    
    ![SQL-lekérdezés eredményei](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-sql-query-results.png)

Az alkalmazás használatával kapcsolatos részletes információkért olvassa el a [Monitor and manage Azure Data Factory pipelines using Monitoring and Management App](data-factory-monitor-manage-app.md) (Azure Data Factory-folyamatok figyelése és felügyelete a Monitoring and Management használatával) című cikket.

### <a name="monitor-pipeline-using-diagram-view"></a>Folyamat figyelése diagramnézetben
Is figyelő adatok folyamatok hello diagram nézet használatával.  

1. A hello **adat-előállító** panelen kattintson a **Diagram**.
   
    ![Data Factory panel – Diagram csempe](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactoryblade-diagramtile.png)
2. Hello diagram hasonló toohello kép a következő kell megjelennie: 
   
    ![Diagramnézet](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-diagram-blade.png)  
5. Hello diagram nézetben kattintson duplán **InputDataset** toosee szeletek hello az adatkészlethez.  
   
    ![Adatkészletek kiválasztott InputDataset elemmel](./media/data-factory-copy-activity-tutorial-using-azure-portal/DataSetsWithInputDatasetFromBlobSelected.png)   
5. Kattintson a **további** hivatkozás toosee összes hello adatszeletek. Megjelennek a 24 órához tartozó szeletek a folyamat kezdési és befejezési időpontja között. 
   
    ![Összes bemeneti adatszelet](./media/data-factory-copy-activity-tutorial-using-azure-portal/all-input-slices.png)  
   
    Megjegyzendő, hogy az összes hello adatszeletek toohello jelenlegi UTC idő **készen** mert hello **emp.txt** fájl hello blob a tárolóban van mindig hello: **adftutorial\input**. a jövőbeli alkalommal hello hello szeletek még nem kész állapotú. Győződjön meg arról, hogy nincs szeletek megjelenni hello **nemrég volt hibás szeletet tartalmazó** szakasz hello lap alján.
6. Bezárás hello paneleken, amíg ki nem látható hello diagram nézet (vagy) görgetési bal oldali toosee hello diagram megtekintése. Ezután kattintson duplán az **OutputDataset** elemre. 
8. Kattintson a **további** hello hivatkozásra kattintva **tábla** paneljén **OutputDataset** toosee összes hello szeletek.

    ![Data slices (Adatszeletek) panel](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslices-blade.png) 
9. Figyelje meg, hogy az összes hello toohello jelenlegi UTC idő szeletek áthelyezése **végrehajtási függőben lévő** állapot = > **folyamatban** ==> **készen** állapotát. az elmúlt hello szeletek hello (előtt aktuális idő) dolgoznak fel a legújabb toooldest alapértelmezés szerint. Például ha hello aktuális időpont 8:12 PM UTC, a 7 - ESTE 8 óra hello szelet feldolgozása hello 18: 00 - 7 PM szelet előre. hello du. 8 – 9 PM szelet alapértelmezés szerint utáni 9 PM feldolgozása hello végén lévő hello alatt az időtartam alatt.  
10. Bármely adatszelet hello listából kattintson, és megtekintheti az hello **adatszelet** panelen. A tevékenységablakokhoz tartozó adatszeleteket nevezzük szeletnek. Egy szelet lehet egy vagy több fájl.  
    
     ![Data slice (Adatszelet) panel](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslice-blade.png)
    
     Ha hello szelet nincs hello **készen** állapot, hello felfelé irányuló szeletek nem készen áll, és blokkol hello aktuális szelet hello végrehajtás alatt látható **felfelé irányuló szeletek nem áll készen** listája.
11. A hello **ADATSZELET** panelen megtekintheti az összes tevékenység fut hello lista hello lap alján. Kattintson egy **tevékenységfuttatási** toosee hello **tevékenység fut részletek** panelen. 
    
    ![Az Activity Run Details (Tevékenységfuttatás részletei) panel](./media/data-factory-copy-activity-tutorial-using-azure-portal/ActivityRunDetails.png)

    Ezen a panelen, lásd: hogyan hosszú hello másolási művelet alatt zajlott le, milyen átviteli hány bájtnyi adat volt olvasási és írásbeli, futtatási kezdési ideje, Futtatás befejezési idő stb.  
12. Kattintson a **X** tooclose egyik hello panelen, amíg ki nem szükséges segítségnyújtáshoz toohello otthoni paneljén hello **ADFTutorialDataFactory**.
13. (választható) kattintson hello **adatkészletek** csempére vagy **folyamatok** csempe tooget hello paneleken láthatta hello fent leírt lépésekkel. 
14. Indítsa el **SQL Server Management Studio**, csatlakozzon az Azure SQL Database toohello, és győződjön meg arról, hogy hello sorok egészül ki toohello **üres** hello adatbázis táblájában.
    
    ![SQL-lekérdezés eredményei](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-sql-query-results.png)


## <a name="summary"></a>Összefoglalás
Ebben az oktatóanyagban létrehozott egy Azure data factory toocopy adatok Azure blob tooan Azure SQL-adatbázis. Hello Azure portál toocreate hello adat-előállító, a társított szolgáltatások, a adatkészletek és a folyamat használta. Az alábbiakban a gépen hajtotta végre ebben az oktatóanyagban hello magas szintű lépéseket:  

1. Létrehozott egy Azure **data factoryt**.
2. **Társított szolgáltatásokat** hozott létre:
   1. Egy **Azure Storage** kapcsolódó szolgáltatás toolink az Azure Storage-fiók, amely a bemeneti adatokat tárolja.     
   2. Egy **Azure SQL** kapcsolódó szolgáltatás toolink az Azure SQL-adatbázis, amely tárolja a hello kimeneti adatokat. 
3. **Adatkészleteket** hozott létre, amelyek a folyamat bemeneti és kimeneti adatait írják le.
4. Létrehozott egy **folyamatot** egy **Másolási tevékenységgel**, ahol a **BlobSource** a forrás, az **SqlSink** pedig a fogadó.  

## <a name="next-steps"></a>Következő lépések
Ez az oktatóanyag egy olyan másolási műveletet mutatott be, amelynek a forrásadattára egy Azure Blob Storage-tár, a céladattára pedig egy Azure SQL-adatbázis volt. hello következő táblázat felsorolja az adatforrások és a célhelyek hello másolási tevékenység által támogatott adattárolókhoz: 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Hogyan toocopy adatok belőle egy adatok tárolására, kapcsolatos toolearn hivatkozásra hello hello adattároló hello tábla.
