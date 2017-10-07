---
title: "Oktatóanyag: Másolási tevékenységgel rendelkező folyamat létrehozása a Visual Studio használatával | Microsoft Docs"
description: "Ez az oktatóanyag bemutatja, hogyan hozhat létre másolási tevékenységgel rendelkező Azure Data Factory-folyamatot a Visual Studio használatával."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1751185b-ce0a-4ab2-a9c3-e37b4d149ca3
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: d99d8875807bab41f5122ab95a09f83f82923529
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-visual-studio"></a>Oktatóanyag: Másolási tevékenységgel rendelkező folyamat létrehozása a Visual Studio használatával
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

Ebből a cikkből megismerheti, hogyan toouse hello Microsoft Visual Studio toocreate egy adat-előállítót, és egy folyamatot, amely másol adatokat az Azure blob storage tooan Azure SQL-adatbázis. Ha új tooAzure adat-előállítót, olvassa végig hello [Data Factory bemutatása tooAzure](data-factory-introduction.md) cikk Ez az oktatóanyag elvégzése előtt.   

Az oktatóanyag segítségével egyetlen tevékenységgel (másolási tevékenységgel) rendelkező folyamatot hozhat létre. hello másolási tevékenység során a támogatott adatokat tároló tooa támogatott fogadó adatokat tároló másol adatokat. A forrásként és fogadóként támogatott adattárak listájáért lásd: [támogatott adattárak](data-factory-data-movement-activities.md#supported-data-stores-and-formats). hello tevékenység egy globálisan elérhető szolgáltatás közötti biztonságos, megbízható és skálázható módon különböző adattárolókhoz másolhatja van-e kapcsolva. További információ a másolási tevékenység hello: [adatok mozgása tevékenységek](data-factory-data-movement-activities.md).

Egy folyamathoz több tevékenység is tartozhat. És láncolt telepítését (egymás után futtatni egy tevékenységet) két tevékenység által hello bemeneti hello az adatkészlet többi tevékenység hello kimeneti adatkészlet egy tevékenység beállítását. További információért lásd: [egy folyamaton belüli több tevékenység](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).

> [!NOTE] 
> hello adatok feldolgozási sor az oktatóanyag a forrás adatokat tároló tooa cél adatokat tároló másol adatokat. Hogyan oktatóanyagért tootransform adatok Azure Data Factory használatával, lásd: [oktatóanyag: a folyamat tootransform adatokat Hadoop-fürt létrehozása](data-factory-build-your-first-pipeline.md).

## <a name="prerequisites"></a>Előfeltételek
1. Olvassa végig [oktatóanyag – áttekintés](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) cikkben és a teljes hello **előfeltétel** lépéseket.       
2. toocreate adat-előállító példányok hello tagjának kell lennie [Data Factory közreműködői](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) szerepkör hello előfizetés-erőforráscsoport szintjén.
3. A számítógépen telepített hello következő kell rendelkeznie: 
   * Visual Studio 2013 vagy Visual Studio 2015
   * Töltse le az Azure SDK-t a Visual Studio 2013-hoz vagy a Visual Studio 2015-höz. Keresse meg a túl[Azure letöltési oldalát](https://azure.microsoft.com/downloads/) kattintson **VS 2013** vagy **VS 2015** a hello **.NET** szakasz.
   * Töltse le a legfrissebb hello Azure Data Factory beépülő modul a Visual Studio: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) vagy [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Hello beépülő modult is frissítheti hello lépések végrehajtásával: hello menüjének **eszközök** -> **bővítmények és frissítések** -> **Online**  ->  **Visual Studio galériában** -> **Microsoft Azure Data Factory Tools for Visual Studio** -> **frissítés**.

## <a name="steps"></a>Lépések
Ez az oktatóanyag részeként végrehajtandó hello lépések a következők:

1. Hozzon létre **összekapcsolt szolgáltatások** hello adat-előállítóban. Ebben a lépésben a következő két típusú társított szolgáltatást hozza létre: Azure Storage és Azure SQL Database. 
    
    hello AzureStorageLinkedService művelet ugyan összeköti a az Azure storage-fiók toohello adat-előállítóban. Létrejött a tároló és részeként feltöltött adatok toothis tárfiók [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   

    AzureSqlLinkedService az Azure SQL adatbázis toohello adat-előállító hivatkozásokat tartalmaz. hello blobtárolóból másolt hello adatok az adatbázis tárolja. Az [előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részeként létrehozta az SQL-táblát az adatbázisban.     
2. Hozzon létre a bemeneti és kimeneti **adatkészletek** hello adat-előállítóban.  
    
    hello Azure tárolás társított szolgáltatásának határozza meg a Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure storage-fiók használó hello kapcsolati karakterláncot. És hello bemeneti blob-adathalmazra hello tároló és hello bemeneti adatokat tartalmazó hello mappát adja meg.  

    Hello csatolt Azure SQL Database szolgáltatáshoz hasonlóan hello kapcsolati karakterláncot használó Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure SQL adatbázis határozza meg. És hello kimeneti SQL táblázat dataset megadja hello adatbázis toowhich hello adatokat a hello blob storage hello táblájában másolódik.
3. Hozzon létre egy **csővezeték** hello adat-előállítóban. Ebben a lépésben létre fog hozni egy másolási tevékenységgel rendelkező folyamatot.   
    
    hello másolási tevékenység hello Azure blob storage tooa tábla hello Azure SQL-adatbázis egy blobot másol adatokat. A másolási tevékenység használható egy folyamat toocopy adatokat bármely támogatott forrás támogatott tooany cél. A támogatott adattárak listájáért lásd: [adatáthelyezési tevékenységek](data-factory-data-movement-activities.md#supported-data-stores-and-formats). 
4. Hozzon létre egy Azure **adat-előállítót** Data Factory-entitások (összekapcsolt szolgáltatások adatkészletek/táblák és folyamatok) telepítésekor. 

## <a name="create-visual-studio-project"></a>Visual Studio-projekt létrehozása
1. Indítsa el a **Visual Studio 2015-öt**. Kattintson a **fájl**, pont túl**új**, és kattintson a **projekt**. Megtekintheti az hello **új projekt** párbeszédpanel megnyitásához.  
2. A hello **új projekt** párbeszédpanelen jelölje be hello **DataFactory** sablont, majd kattintson **üres Data Factory-projektekhez**.  
   
    ![A New project (Új projekt) párbeszédpanel](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-project-dialog.png)
3. Adja meg a hello projekt, a hely hello megoldás hello nevét, és hello megoldás nevét, és kattintson **OK**.
   
    ![Megoldáskezelő](./media/data-factory-copy-activity-tutorial-using-visual-studio/solution-explorer.png)    

## <a name="create-linked-services"></a>Társított szolgáltatások létrehozása
A data factory toolink adatait tárolja, és számítási szolgáltatások toohello adat-előállító létrehozása társított szolgáltatások. Ebben az oktatóanyagban nem használunk számítási szolgáltatásokat (például Azure HDInsight vagy Azure Data Lake Analytics). Csak kétféle típusú adattárat használunk: Azure Storage (forrás) és Azure SQL Database (cél). 

Ezért a következő két típusú társított szolgáltatást hozza létre: AzureStorage és AzureSQLDatabase.  

hello Azure Storage társított szolgáltatás hivatkozások a az Azure storage-fiók toohello adat-előállítóban. Ez a tárfiók egy hello akkor létrejött a tároló és hello adatok részeként feltöltött [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).   

Az Azure SQL társított szolgáltatás hivatkozások az Azure SQL adatbázis toohello adat-előállítóban. hello blobtárolóból másolt hello adatok az adatbázis tárolja. Hello üres tábla az adatbázisban részeként létrehozott [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Összekapcsolt szolgáltatások adattárolókhoz hivatkozásra, vagy a szolgáltatások tooan az Azure data factory számítási. Lásd: [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) az összes hello források és mosdók hello másolási tevékenység által támogatott. Lásd: [összekapcsolt szolgáltatások számítási](data-factory-compute-linked-services.md) a Data Factory által támogatott számítási szolgáltatás hello listája. Az oktatóanyag során ne használjon számítási szolgáltatásokat. 

### <a name="create-hello-azure-storage-linked-service"></a>Hello Azure Storage társított szolgáltatás létrehozása
1. A **Megoldáskezelőben**, kattintson a jobb gombbal **összekapcsolt szolgáltatások**, pont túl**Hozzáadás**, és kattintson a **új elem**.      
2. A hello **új elem hozzáadása** párbeszédpanelen jelölje ki **Azure Storage társított szolgáltatás** hello listából, és kattintson a **Hozzáadás**. 
   
    ![Új társított szolgáltatás](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-linked-service-dialog.png)
3. Cserélje le `<accountname>` és `<accountkey>`* hello nevű Azure-tárfiókot és a kulcsok. 
   
    ![Azure Storage társított szolgáltatás](./media/data-factory-copy-activity-tutorial-using-visual-studio/azure-storage-linked-service.png)
4. Mentse a hello **AzureStorageLinkedService1.json** fájlt.

    JSON-tulajdonságokat hello társított szolgáltatás definíciójának kapcsolatos további információkért lásd: [Azure Blob Storage összekötő](data-factory-azure-blob-connector.md#linked-service-properties) cikk.

### <a name="create-hello-azure-sql-linked-service"></a>Hello Azure SQL társított szolgáltatás létrehozása
1. Kattintson a jobb gombbal a **összekapcsolt szolgáltatások** hello csomópontja **Megoldáskezelőben** újra, mutasson a túl**hozzáadása**, és kattintson a **új elem**. 
2. Ezúttal válassza a **Azure SQL Linked Service** (Azure SQL társított szolgáltatás) lehetőséget, és kattintson az **Add** (Hozzáadás) elemre. 
3. A hello **AzureSqlLinkedService1.json fájl**, cserélje le `<servername>`, `<databasename>`, `<username@servername>`, és `<password>` nevekkel az Azure SQL server, az adatbázis, a felhasználói fiókot, és a jelszót.    
4. Mentse a hello **AzureSqlLinkedService1.json** fájlt. 
    
    További információ a JSON-tulajdonságokról: [Azure SQL Database-összekötő](data-factory-azure-sql-connector.md#linked-service-properties).


## <a name="create-datasets"></a>Adatkészletek létrehozása
Hello előző lépésben létrehozott összekapcsolt szolgáltatások toolink az Azure Storage-fiók és az Azure SQL adatbázis tooyour adat-előállítóban. Ebben a lépésben InputDataset és OutputDataset, amelyek megfelelnek a bemeneti és kimeneti adatok AzureStorageLinkedService1 és AzureSqlLinkedService1 által hivatkozott hello adattárolókhoz tárolt nevű két adatkészletet határozza meg.

hello Azure tárolás társított szolgáltatásának határozza meg a Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure storage-fiók használó hello kapcsolati karakterláncot. És hello bemeneti blob-adathalmazra (InputDataset) hello tároló és hello bemeneti adatokat tartalmazó hello mappát adja meg.  

Hello csatolt Azure SQL Database szolgáltatáshoz hasonlóan hello kapcsolati karakterláncot használó Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure SQL adatbázis határozza meg. És hello kimeneti SQL táblázat dataset (OututDataset) hello adatbázis toowhich hello hello blobtárolóból az adatok másolásakor hello tábla határozza meg. 

### <a name="create-input-dataset"></a>Bemeneti adatkészlet létrehozása
Ebben a lépésben hoz létre, mely tooa fájlját (emp.txt) InputDataset nevű adatkészlete hello gyökérmappájában lévő mappának a blob-tároló (adftutorial) az Azure Storage hello AzureStorageLinkedService1 kapcsolódó szolgáltatás által képviselt hello. Ha nem hello fájlnév értéket (vagy hagyja ki), a hello bemeneti mappában található összes BLOB adatait is másolt toohello cél. Ebben az oktatóanyagban hello fájlnév értéket kell megadni. 

Itt használja a hello kifejezés "tábla" helyett "adatkészletek". Egy tábla egy négyszögletes dataset és hello csak ilyen típusú adatkészlet most támogatott. 

1. Kattintson a jobb gombbal **táblák** a hello **megoldáskezelő**, pont túl**hozzáadása**, és kattintson a **új elem**.
2. A hello **új elem hozzáadása** párbeszédpanelen jelölje ki **Azure Blob**, és kattintson a **Hozzáadás**.   
3. Cserélje le a következő szöveg hello hello JSON-szöveg, és mentse a hello **AzureBlobLocation1.json** fájlt. 

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
      "linkedServiceName": "AzureStorageLinkedService1",
      "typeProperties": {
        "folderPath": "adftutorial/",
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

### <a name="create-output-dataset"></a>Kimeneti adatkészlet létrehozása
Ebben a lépésben egy kimeneti adatkészletet hoz létre **OutputDataset** néven. Ez az adatkészlet mutat tooa SQL táblázat hello Azure SQL Database által képviselt **AzureSqlLinkedService1**. 

1. Kattintson a jobb gombbal **táblák** hello a **Megoldáskezelőben** újra, mutasson a túl**hozzáadása**, és kattintson a **új elem**.
2. A hello **új elem hozzáadása** párbeszédpanelen jelölje ki **Azure SQL**, és kattintson a **Hozzáadás**. 
3. Cserélje le a következő JSON hello hello JSON-szöveg, és mentse a hello **AzureSqlTableLocation1.json** fájlt.

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
       "linkedServiceName": "AzureSqlLinkedService1",
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

## <a name="create-pipeline"></a>Folyamat létrehozása
Ebben a lépésben létrehoz egy **másolási tevékenységgel** rendelkező folyamatot, amely bemenetként az **InputDataset**, kimenetként pedig az **OutputDataset** adatkészletet használja.

Kimeneti adatkészlet jelenleg milyen meghajtók hello ütemezést. Ebben az oktatóanyagban a kimeneti adatkészlet óránként konfigurált tooproduce szelet. hello folyamat rendelkezik egy kezdési és befejezési időpontja, amelyek egy nap telhet el, amely 24 óra. Ezért a kimeneti adatkészlet 24 szeletek előállított hello folyamat. 

1. Kattintson a jobb gombbal **folyamatok** hello a **Megoldáskezelőben**, pont túl**hozzáadása**, és kattintson a **új elem**.  
2. Válassza ki **másolási adatok csővezeték** a hello **új elem hozzáadása** párbeszédpanel megnyitásához, és kattintson **Hozzáadás**. 
3. Hello JSON cserélje le a következő JSON hello, és mentse a hello **CopyActivity1.json** fájlt.

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
             "style": "StartOfInterval",
             "retry": 0,
             "timeout": "01:00:00"
           }
         }
       ],
       "start": "2017-05-11T00:00:00Z",
       "end": "2017-05-12T00:00:00Z",
       "isPaused": false
     }
    }
    ```   
    - Hello tevékenységek szakaszban csak egy tevékenység nincs amelynek **típus** értéke túl**másolási**. Hello másolási tevékenység kapcsolatos további információkért lásd: [adatok mozgása tevékenységek](data-factory-data-movement-activities.md). A Data Factory megoldásaiban használhatja az [adatátalakítási tevékenységeket](data-factory-data-transformation-activities.md) is.
    - Adjon meg a hello tevékenység értéke túl**InputDataset** és hello tevékenység túl van-e állítva a kimeneti**OutputDataset**. 
    - A hello **typeProperties** szakaszban **BlobSource** hello forrás típusaként van megadva, és **SqlSink** hello a fogadó típusa van megadva. Források és mosdók hello másolási tevékenység által támogatott adattárolókhoz teljes listáját lásd: [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Hogyan toouse a támogatott adatokat tárolót, mint a forrás/fogadó toolearn hivatkozásra hello hello táblában.  
     
    Cserélje le a hello hello értékének **start** tulajdonságát az aktuális nap hello és **end** hello másnap értéket. Adja meg, csak a hello dátumrész, és hagyja ki a hello idő összetevőjét hello időpontja. Például "2016-02-03", amely túl egyenértékű "2016-02-03T00:00:00Z"
     
    Mind a kezdő, mind a befejező dátum-időpont értéket [ISO formátumban](http://en.wikipedia.org/wiki/ISO_8601) kell megadni. Például: 2016-10-14T16:32:41Z. Hello **end** idő megadása nem kötelező, de ebben az oktatóanyagban használjuk. 
     
    Ha nem ad meg értéket a hello **end** tulajdonságot, akkor a program "**kezdés + 48 óra**". toorun hello folyamat határozatlan ideig, adja meg **9999-09-09** hello hello értékként **end** tulajdonság.
     
    A fenti példa hello amelyeket 24 adatszeletek minden adatszelet rendszer óránként készít adatszeletet.

    A folyamathoz tartozó definíció JSON-tulajdonságainak leírásáért lásd: [folyamatok létrehozása](data-factory-create-pipelines.md). A másolási tevékenységhez tartozó definíció JSON-tulajdonságainak leírásáért lásd: [adatáthelyezési tevékenységek](data-factory-data-movement-activities.md). A BlobSource által támogatott JSON-tulajdonságok leírásáért lásd: [Azure Blob-összekötő](data-factory-azure-blob-connector.md). Az SqlSink által támogatott JSON-tulajdonságok leírása az [Azure SQL Database-összekötő](data-factory-azure-sql-connector.md) című cikkben található.

## <a name="publishdeploy-data-factory-entities"></a>Data Factory-entitások közzététele/üzembe helyezése
Ebben a lépésben a korábban létrehozott Data Factory-entitásokat (társított szolgáltatások, adatkészletek és folyamat) teszi közzé. Is meg hello új data factory toobe hello neve létrehozott toohold ezeket az entitásokat.  

1. Kattintson a jobb gombbal a projektre a Solution Explorer hello, és kattintson a **közzététel**. 
2. Ha látja **jelentkezzen be Microsoft-fiók tooyour** párbeszédpanelen adja meg Azure-előfizetéssel rendelkező hello fiók hitelesítő adatait, és kattintson a **bejelentkezés**.
3. A következő párbeszédpanel hello kell megjelennie:
   
   ![Publish (Közzététel) párbeszédpanel](./media/data-factory-copy-activity-tutorial-using-visual-studio/publish.png)
4. Hello konfigurálása data factory lapon hello lépéseket követve: 
   
   1. Válassza a **Create New Data Factory** (Új data factory létrehozása) lehetőséget.
   2. A **Name** (Név) mezőbe írja be a következőt: **VSTutorialFactory**.  
      
      > [!IMPORTANT]
      > az Azure data factory hello hello nevének globálisan egyedi kell lennie. Ha közzétételekor adat-előállító nevét hello kapcsolatos hibaüzenetet kap, hello data factory (például yournameVSTutorialFactory), és próbálja meg újra közzétenni hello nevének módosítása. A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.        
      > 
      > 
   3. Válassza ki az Azure-előfizetéshez tartozó hello **előfizetés** mező.
      
      > [!IMPORTANT]
      > Ha nem látja bármely előfizetés, győződjön meg arról, hogy olyan fiókkal, amely egy rendszergazdai vagy társadminisztrátori hello előfizetés naplóba.  
      > 
      > 
   4. Jelölje be hello **erőforráscsoport** a hello data factory toobe létrehozott. 
   5. Jelölje be hello **régió** hello adat-előállító esetében. Csak a Data Factory szolgáltatásnak hello által támogatott régiók hello legördülő listában jelennek meg.
   6. Kattintson a **következő** tooswitch toohello **elemek közzététele** lap.
      
       ![Configure data factory (Data factory konfigurálása) oldal](media/data-factory-copy-activity-tutorial-using-visual-studio/configure-data-factory-page.png)   
5. A hello **elemek közzététele** lapon, győződjön meg arról, hogy az összes hello adat-előállítók entitások van kiválasztva, és kattintson a **következő** tooswitch toohello **összegzés** lap.
   
   ![Publish items (Elemek közzététele) oldal](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-items-page.png)     
6. Olvassa el az összesítő hello és a **következő** toostart hello központi telepítési folyamat és a nézet hello **központi telepítési állapot**.
   
   ![Publish summary (Összefoglaló közzététele) oldal](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-summary-page.png)
7. A hello **központi telepítési állapot** lapon hello telepítési folyamatának állapotát hello kell megjelennie. Hello a telepítés befejezése után kattintson a Befejezés gombra.
 
   ![Üzembe helyezés állapota lap](media/data-factory-copy-activity-tutorial-using-visual-studio/deployment-status.png)

Vegye figyelembe a következő pontok hello: 

* Ha hello hibaüzenet: "Ez az előfizetés nem regisztrált toouse névtér Microsoft.DataFactory", hello alábbi, és próbálja meg újra közzétenni: 
  
  * Azure PowerShell futtassa a következő parancs tooregister hello adat-előállító szolgáltató hello. 

    ```PowerShell    
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
    A következő parancs tooconfirm hello futtathatja, hogy hello Data Factory szolgáltató regisztrálva van. 
    
    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * Bejelentkezés használatával hello Azure-előfizetéssel való hello [Azure-portálon](https://portal.azure.com) , és keresse meg a tooa adat-előállító panel (vagy) hozzon létre egy adat-előállító hello Azure-portálon. Ez a művelet automatikusan regisztrálja az Ön hello szolgáltató.
* hello adat-előállító nevét hello előfordulhat, hogy jövőbeli hello a DNS-névként regisztrált, és ezért a nyilvánosan láthatóvá válnak.

> [!IMPORTANT]
> toocreate adat-előállító esetekben kell toobe egy rendszergazdai vagy társadminisztrátori hello Azure-előfizetés az

## <a name="monitor-pipeline"></a>Folyamat figyelése
Keresse meg a data factory toohello kezdőlapját:

1. Jelentkezzen be túl[Azure-portálon](https://portal.azure.com).
2. Kattintson a **további szolgáltatások** hello bal oldali menüben, és kattintson a **adat-előállítók**.

    ![Data factoryk tallózása](media/data-factory-copy-activity-tutorial-using-visual-studio/browse-data-factories.png)
3. Kezdje el beírni a data factory hello nevét.

    ![Adat-előállító neve](media/data-factory-copy-activity-tutorial-using-visual-studio/enter-data-factory-name.png) 
4. Kattintson a data factory a hello eredmények lista toosee hello kezdőlapját a data factory.

    ![Data factory kezdőlap](media/data-factory-copy-activity-tutorial-using-visual-studio/data-factory-home-page.png)
5. Kövesse az utasításokat [adatkészletek és a folyamat figyelése](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) toomonitor hello feldolgozási sorban lévő és adatkészletek ebben az oktatóanyagban létrehozott. A Visual Studio jelenleg nem támogatja a Data Factory-folyamatok monitorozását. 

## <a name="summary"></a>Összefoglalás
Ebben az oktatóanyagban létrehozott egy Azure data factory toocopy adatok Azure blob tooan Azure SQL-adatbázis. Visual Studio toocreate hello adat-előállító, a társított szolgáltatások, a adatkészletek és a folyamat használta. Az alábbiakban a gépen hajtotta végre ebben az oktatóanyagban hello magas szintű lépéseket:  

1. Létrehozott egy Azure **data factoryt**.
2. **Társított szolgáltatásokat** hozott létre:
   1. Egy **Azure Storage** kapcsolódó szolgáltatás toolink az Azure Storage-fiók, amely a bemeneti adatokat tárolja.     
   2. Egy **Azure SQL** kapcsolódó szolgáltatás toolink az Azure SQL-adatbázis, amely tárolja a hello kimeneti adatokat. 
3. **Adatkészleteket** hozott létre, amelyek az adatcsatorna bemeneti és kimeneti adatait írják le.
4. Létrehozott egy **folyamatot** egy **Másolási tevékenységgel**, ahol a **BlobSource** a forrás, az **SqlSink** pedig a fogadó. 

Hogyan toouse egy HDInsight Hive tevékenység tootransform adatok Azure HDInsight-fürt használatával: toosee [ oktatóanyag: az első adatcsatorna tootransform adatok Hadoop-fürt létrehozása](data-factory-build-your-first-pipeline.md).

Hello kimeneti adatkészlet egy tevékenység beállítását hello bemeneti hello az adatkészlet többi tevékenység által láncolt lehessen két tevékenység (egy tevékenység egymás után). Lásd [a Data Factorybeli ütemezést és végrehajtást](data-factory-scheduling-and-execution.md) ismertető cikket. 

## <a name="view-all-data-factories-in-server-explorer"></a>Az összes adat-előállító megjelenítése a Kiszolgálókezelőben (Server Explorer)
Ez a szakasz ismerteti, hogyan toouse tooview Visual Studio Server Explorer hello összes hello adat-előállítók az Azure-előfizetése, és valamelyik adat-előállítót alapján Visual Studio-projekt létrehozása. 

1. A **Visual Studio**, kattintson a **nézet** hello menü, és kattintson a **Server Explorer**.
2. Hello Server Explorer-ablakban bontsa ki a **Azure** csomópontot **adat-előállító**. Ha látja **tooVisual Studio bejelentkezés**, adja meg a hello **fiók** társított a Azure-előfizetéssel, és kattintson **Folytatás**. Adja meg a **jelszót**, és kattintson a **Sign in** (Bejelentkezés) elemre. A Visual Studio megpróbál az előfizetés az összes Azure adat-előállítók tooget információt. Ez a művelet hello hello állapotát látja **Data Factory feladatlista** ablak.

    ![Server Explorer (Kiszolgálókezelő)](./media/data-factory-copy-activity-tutorial-using-visual-studio/server-explorer.png)

## <a name="create-a-visual-studio-project-for-an-existing-data-factory"></a>Visual Studio-projekt létrehozása egy meglévő adat-előállító alapján

- Kattintson a jobb gombbal a Server Explorer egy adat-előállítót, és válassza ki **exportálása adat-előállító tooNew projekt** toocreate Visual Studio-projekt valamelyik adat-előállítót alapján.

    ![Exportálja a data factory tooa VS projektet](./media/data-factory-copy-activity-tutorial-using-visual-studio/export-data-factory-menu.png)  

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


## <a name="next-steps"></a>Következő lépések
Ez az oktatóanyag egy olyan másolási műveletet mutatott be, amelynek a forrásadattára egy Azure Blob Storage-tár, a céladattára pedig egy Azure SQL-adatbázis volt. hello következő táblázat felsorolja az adatforrások és a célhelyek hello másolási tevékenység által támogatott adattárolókhoz: 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Hogyan toocopy adatok belőle egy adatok tárolására, kapcsolatos toolearn hivatkozásra hello hello adattároló hello tábla.
