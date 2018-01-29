---
title: "Elágaztatás Azure Data Factory-folyamatokban | Microsoft Docs"
description: "Megismerheti, hogyan vezérelheti az adatok folyamát az Azure Data Factoryben elágaztatási és láncolási tevékenységekkel."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/11/2018
ms.author: shlo
ms.openlocfilehash: de48d61af0e8056a749715343ef821cfc35cb93d
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/19/2018
---
# <a name="branching-and-chaining-activities-in-a-data-factory-pipeline"></a>Elágaztatási és láncolási tevékenységek a Data Factory-folyamatokban
Ebben az oktatóanyagban egy olyan adat-előállító folyamatot hoz létre, amely bemutat néhány folyamvezérlési funkciót. A folyamat egy egyszerű másolást hajt végre egy Azure Blob Storage-beli tárolóból egy másik tárolóba, amely ugyanazon tárfiókban található. Ha a másolási tevékenység sikeres, a folyamat egy e-mailt küld a sikeres műveletről, amelyben szerepelnek a sikeres másolási művelet részletei (például az írt adatok mennyisége). Ha a másolási tevékenység sikertelen, a folyamat egy e-mailt küld a sikertelen műveletről, amelyben szerepelnek a sikertelen másolás részletei (például a hibaüzenet). Az oktatóanyag során megismerheti, hogyan adhatók át a paraméterek.

> [!NOTE]
> Ez a cikk a Data Factory 2. verziójára vonatkozik, amely jelenleg előzetes verzióban érhető el. Ha a Data Factory szolgáltatás általánosan elérhető 1. verzióját használja, lásd [a Data Factory 1. verziójának dokumentációját](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

A forgatókönyv általános áttekintése: ![Áttekintés](media/tutorial-control-flow-portal/overview.png)

Az oktatóanyagban az alábbi lépéseket fogja végrehajtani:

> [!div class="checklist"]
> * Adat-előállító létrehozása
> * Azure Storage-beli társított szolgáltatás létrehozása
> * Azure Blob-adatkészlet létrehozása
> * Másolási tevékenységet és webes tevékenységet tartalmazó folyamat létrehozása
> * Tevékenységek kimeneteinek elküldése a soron következő tevékenységek számára
> * Paraméterátadás és rendszerváltozók használata
> * Folyamat futásának indítása
> * A folyamat és a tevékenységek futásának monitorozása

Ez az oktatóanyag az Azure Portalt használja. Az Azure Data Factoryvel való interakció más módszerekkel is lehetséges. Ezekkel kapcsolatban a tartalomjegyzék „Gyors útmutatók” részében találhat információkat.

## <a name="prerequisites"></a>Előfeltételek

* **Azure-előfizetés**. Ha nem rendelkezik Azure-előfizetéssel, első lépésként mindössze néhány perc alatt létrehozhat egy [ingyenes](https://azure.microsoft.com/free/) fiókot.
* **Azure Storage-fiók** A blobtárolót használjuk **forrás** adattárként. Ha még nem rendelkezik Azure Storage-fiókkal, a létrehozás folyamatáért lásd a [tárfiók létrehozását](../storage/common/storage-create-storage-account.md#create-a-storage-account) ismertető cikket.
* **Azure SQL Database** Ezt az adatbázist használjuk **fogadó** adattárként. Ha még nem rendelkezik Azure SQL Database-adatbázissal, a létrehozás folyamatáért lásd az [Azure SQL-adatbázis létrehozását](../sql-database/sql-database-get-started-portal.md) ismertető cikket.

### <a name="create-blob-table"></a>Blobtábla létrehozása

1. Indítsa el a Jegyzettömböt. Másolja be az alábbi szöveget, és mentse egy **input.txt** nevű fájlként a lemezen.

    ```
    John,Doe
    Jane,Doe
    ```
2. Az [Azure Storage Explorerrel](http://storageexplorer.com/) vagy egy hasonló eszközzel hajtsa végre a következő lépéseket: 
    1. Hozza létre az **adfv2branch** tárolót.
    2. Hozza létre az **input** mappát az **adfv2branch** tárolóban.
    3. Töltse fel az **input.txt** fájlt a tárolóba.

## <a name="create-email-workflow-endpoints"></a>E-mail munkafolyamat végpontjainak létrehozása
Ahhoz, hogy aktiválja az e-mailek folyamatból történő küldését, a [Logic Apps](../logic-apps/logic-apps-overview.md) használatával határozhat meg munkafolyamatot. A Logic App-munkafolyamatok létrehozásának részleteit a [logikai alkalmazások létrehozását ismertető](../logic-apps/quickstart-create-first-logic-app-workflow.md) cikkben tekintheti meg. 

### <a name="success-email-workflow"></a>Sikeres műveletről tájékoztató e-mail munkafolyamata 
Hozzon létre egy `CopySuccessEmail` nevű Logic App-munkafolyamatot. A munkafolyamat eseményindítója legyen `When an HTTP request is received`, és adjon hozzá egy `Office 365 Outlook – Send an email` műveletet.

![Sikeres műveletről tájékoztató e-mail munkafolyamata](media/tutorial-control-flow-portal/success-email-workflow.png)

A kérelem eseményindítójához a `Request Body JSON Schema` esetében adja meg a következő JSON-t:

```json
{
    "properties": {
        "dataFactoryName": {
            "type": "string"
        },
        "message": {
            "type": "string"
        },
        "pipelineName": {
            "type": "string"
        },
        "receiver": {
            "type": "string"
        }
    },
    "type": "object"
}
```

A kérelem a következő képen láthatóhoz hasonlóan jelenik meg a Logikaialkalmazás-tervezőben: 

![Logikaialkalmazás-tervező – kérelem](media/tutorial-control-flow-portal/logic-app-designer-request.png)

Az **E-mail küldése** művelethez szabja testre az e-mail formátumát a kérelemtörzs JSON-sémájában átadott tulajdonságokkal. Például:

![Logikaialkalmazás-tervező – e-mail küldése művelet](media/tutorial-control-flow-portal/send-email-action-2.png)

Mentse a munkafolyamatot. Jegyezze fel a sikeres műveletről tájékoztató e-mail munkafolyamatához tartozó HTTP POST-kérelem URL-címét:

```
//Success Request Url
https://prodxxx.eastus.logic.azure.com:443/workflows/000000/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=000000
```

### <a name="fail-email-workflow"></a>Sikertelen műveletről tájékoztató e-mail munkafolyamata 
Ugyanezen lépésekkel hozzon létre egy másik Logic Apps-munkafolyamatot **CopyFailEmail** néven. A kérelem eseményindítójában a `Request Body JSON schema` ugyanaz. Módosítsa az e-mail formátumát (például a `Subject` értékét) a sikertelen műveletről tájékoztató e-mailnek megfelelően. Például:

![Logikaialkalmazás-tervező – Sikertelen műveletről tájékoztató e-mail munkafolyamata](media/tutorial-control-flow-portal/fail-email-workflow-2.png)

Mentse a munkafolyamatot. Jegyezze fel a sikertelen műveletről tájékoztató e-mail munkafolyamatához tartozó HTTP POST-kérelem URL-címét:

```
//Fail Request Url
https://prodxxx.eastus.logic.azure.com:443/workflows/000000/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=000000
```

Ekkor két munkafolyamathoz tartozó URL-címmel rendelkezik:

```
//Success Request Url
https://prodxxx.eastus.logic.azure.com:443/workflows/000000/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=000000

//Fail Request Url
https://prodxxx.eastus.logic.azure.com:443/workflows/000000/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=000000
```

## <a name="create-a-data-factory"></a>Data factory létrehozása

1. Kattintson az **Új** elemre, majd az **Adatok + analitika**, végül a **Data Factory** elemre. 
   
   ![New (Új)->DataFactory](./media/tutorial-control-flow-portal/new-azure-data-factory-menu.png)
2. Az **Új adat-előállító** lapon, a **Név** mezőben adja meg a következőt: **ADFTutorialDataFactory**. 
      
     ![Új adat-előállító lap](./media/tutorial-control-flow-portal/new-azure-data-factory.png)
 
   Az Azure data factory nevének **globálisan egyedinek** kell lennie. Ha a következő hibaüzenetet kapja, módosítsa a data factory nevét (például sajátnévADFTutorialDataFactory-ra), majd próbálkozzon újra a létrehozással. A Data Factory-összetevők részleteit a [Data Factory elnevezési szabályait](naming-rules.md) ismertető cikkben találja.
  
       `Data factory name “ADFTutorialDataFactory” is not available`
3. Válassza ki azt az **Azure-előfizetést**, amelyben az adat-előállítót létre szeretné hozni. 
4. Az **erőforráscsoportban** hajtsa végre a következő lépések egyikét:
     
      - Kattintson a **Meglévő használata** elemre, majd a legördülő listából válasszon egy meglévő erőforráscsoportot. 
      - Kattintson az **Új létrehozása** elemre, és adja meg az erőforráscsoport nevét.   
         
        Az erőforráscsoportokkal kapcsolatos információkért tekintse meg a [Using resource groups to manage your Azure resources](../azure-resource-manager/resource-group-overview.md) (Erőforráscsoportok használata az Azure-erőforrások kezeléséhez) című cikket.  
4. Válassza a **V2 (előzetes verzió)** értéket a **verzió** esetén.
5. Válassza ki a Data Factory **helyét**. A legördülő listán csak a támogatott helyek jelennek meg. Az adat-előállítók által használt adattárak (Azure Storage, Azure SQL Database stb.) és számítási erőforrások (HDInsight stb.) más régiókban is lehetnek.
6. Válassza a **Rögzítés az irányítópulton** lehetőséget.     
7. Kattintson a **Create** (Létrehozás) gombra.      
8. Az irányítópulton megjelenő csempén a következő állapotleírás látható: **Adat-előállító üzembe helyezése**. 

    ![adat-előállító üzembe helyezése csempe](media/tutorial-control-flow-portal/deploying-data-factory.png)
9. A létrehozás befejezése után a **Data Factory** lap a képen látható módon jelenik meg.
   
   ![Data factory kezdőlap](./media/tutorial-control-flow-portal/data-factory-home-page.png)
10. Az Azure Data Factory felhasználói felületének (UI) külön lapon történő elindításához kattintson a **Létrehozás és monitorozás** csempére.


## <a name="create-a-pipeline"></a>Folyamat létrehozása
Ebben a lépésben egy másolási tevékenységgel és két webes tevékenységgel rendelkező folyamatot fog létrehozni. A folyamat létrehozásához a következő funkciókat használja:

- Az adatkészletek által elért folyamat paraméterei. 
- Webes tevékenység a sikeres vagy sikertelen műveletről tájékoztató e-mailek elküldésére szolgáló Logic Apps-munkafolyamatok meghívásához. 
- Tevékenységek összekapcsolása (siker és meghiúsulás esetén)
- Egy tevékenység kimenetének használata egy következő tevékenység bemeneteként

1. A Data Factory felhasználói felületének **első lépéseket ismertető** oldalán kattintson a **Folyamat létrehozása** csempére.  

   ![Első lépések lap](./media/tutorial-control-flow-portal/get-started-page.png) 
3. A folyamat tulajdonságok ablakában váltson a **Paraméterek** lapra, és a **Új** gombbal adja hozzá a következő három karakterlánc típusú paramétert: sourceBlobContainer, sinkBlobContainer és receiver. 

    - **sourceBlobContainer** – a folyamat ezen paraméterét a forrás blob-adatkészlet használja fel.
    - **sinkBlobContainer** – a folyamat ezen paraméterét a fogadó blob-adatkészlet használja fel
    - **receiver** – ezt a paramétert a folyamatban lévő két webes tevékenység használja, amelyek sikerrel vagy hibával kapcsolatos e-maileket küldenek a paraméter által meghatározott e-mail-címmel rendelkező címzettnek.

   ![Új folyamat menü](./media/tutorial-control-flow-portal/pipeline-parameters.png)
4. A **Tevékenységek** eszközkészletben bontsa ki az **Adatfolyam** elemet, és húzza át a **Másolás** tevékenységet a folyamat tervezőfelületére. 

   ![Másolási tevékenység húzása](./media/tutorial-control-flow-portal/drag-drop-copy-activity.png)
5. A **Másolás** tevékenység **Tulajdonságok** ablakában (a képernyő alján) váltson a **Forrás** lapra, majd kattintson az **+ Új** elemre. Ebben a lépésben létrehoz egy forrásadatkészletet a másolási tevékenységhez. 

   ![Forrásadatkészlet](./media/tutorial-control-flow-portal/new-source-dataset-button.png)
6. Az **Új adatkészlet** ablakban válassza az **Azure Blob Storage** lehetőséget, majd kattintson a **Befejezés** gombra. 

   ![Select Azure Blob Storage](./media/tutorial-control-flow-portal/select-azure-blob-storage.png)
7. Ekkor megjelenik egy **AzureBlob1** nevű új **lap**. Módosítsa az adatkészlet nevét a következőre: **SourceBlobDataset**.

   ![Adatkészlet általános beállításai](./media/tutorial-control-flow-portal/dataset-general-page.png)
8. Váltson a **Kapcsolat** lapra a **Tulajdonságok** ablakban, majd kattintson a **Társított szolgáltatás** elemhez tartozó Új elemre. Ebben a lépésben létrehoz egy társított szolgáltatást, hogy az adat-előállítóhoz kapcsolja az Azure Storage-fiókot. 
    
   ![Adatkészlet kapcsolata – új társított szolgáltatás](./media/tutorial-control-flow-portal/dataset-connection-new-button.png)
9. Az **Új társított szolgáltatás** ablakban végezze el az alábbi lépéseket: 

    1. A **Név** mezőbe írja az **AzureStorageLinkedService** nevet.
    2. A **Storage-fiók neve** elemnél válassza ki saját Azure Storage-fiókját.
    3. Kattintson a **Save** (Mentés) gombra.

   ![Új Azure Storage-beli társított szolgáltatás](./media/tutorial-control-flow-portal/new-azure-storage-linked-service.png)
12. Adja meg a `@pipeline().parameters.sourceBlobContainer` mappanevet és az `emp.txt` fájlnevet. A sourceBlobContainer folyamatparaméterrel adhatja meg az adatkészlet mappájának elérési útját. 

    ![Forrásadatkészlet beállításai](./media/tutorial-control-flow-portal/source-dataset-settings.png)
13. Váltson a **folyamatot** tartalmazó lapra, (vagy) kattintson a folyamatra a fanézetben. Győződjön meg arról, hogy a **Forrásadatkészlet** mezőben a **SourceBlobDataset** adatkészlet van kiválasztva. 

   ![Forrásadatkészlet](./media/tutorial-control-flow-portal/pipeline-source-dataset-selected.png)
13. A tulajdonságok ablakában váltson a **Fogadó** lapra, és kattintson az **+ Új** elemre a **Fogadó adatkészlet** mezőben. Ebben a lépésben a forrásadatkészlet létrehozásához hasonlóan létrehoz egy fogadó adatkészletet a másolási tevékenységhez. 

    ![Új fogadó adatkészlet gomb](./media/tutorial-control-flow-portal/new-sink-dataset-button.png)
14. Az **Új adatkészlet** ablakban válassza az **Azure Blob Storage** lehetőséget, majd kattintson a **Befejezés** gombra. 
15. Az adatkészlet **Általános** beállításainak oldalán adja meg a **SinkBlobDataset** értéket a **Név** mezőben.
16. Váltson a **Kapcsolat** lapra, és végezze el az alábbi lépéseket: 

    1. A **LinkedService** mezőben válassza az **AzureStorageLinkedService** elemet.
    2. Adja meg a következő mappanevet: `@pipeline().parameters.sinkBlobContainer`.
    1. Adja meg a következő fájlnevet: `@CONCAT(pipeline().RunId, '.txt')`. A kifejezés az aktuális folyamatfuttatás azonosítóját használja fájlnévként. A támogatott rendszerváltozók és kifejezések listáját lásd a [rendszerváltozókat](control-flow-system-variables.md) és a [kifejezésnyelveket](control-flow-expression-language-functions.md) ismertető cikkekben.

        ![Fogadó adatkészlet beállításai](./media/tutorial-control-flow-portal/sink-dataset-settings.png)
17. Váltson a felül látható **Folyamat** lapra. A **Tevékenységek** eszközkészletben bontsa ki az **Általános** elemet, és húzzon egy **Webes** tevékenységet a folyamat tervezőfelületére. Állítsa a tevékenység nevét a következőre: **SendSuccessEmailActivity**. A webes tevékenység bármely REST-végpont meghívását lehetővé teszi. Erről a tevékenységről további információt a [webes tevékenységgel](control-flow-web-activity.md) kapcsolatos témakörben talál. Ez a folyamat egy webes tevékenységgel hívja meg a Logic Apps e-mail-munkafolyamatát. 

   ![Az első webes tevékenység áthúzása](./media/tutorial-control-flow-portal/success-web-activity-general.png)
18. Váltson az **Általános** lapról a **Beállítások** lapra, és végezze el az alábbi lépéseket: 
    1. Az **URL** mezőben adja meg a sikeres műveletet jelző e-mailt elküldő Logic Apps-munkafolyamat URL-címét.  
    2. A **Metódus** mezőben válassza a **POST** lehetőséget. 
    3. A **Fejlécek** szakaszban kattintson a **+ Fejléc hozzáadása** hivatkozásra. 
    4. Adjon hozzá egy **Content-Type** típusú fejlécet, amelynek beállítása **application/json**. 
    5. A **Törzs** értékeként adja meg a következő JSON-t. 

        ```json
        {
            "message": "@{activity('Copy1').output.dataWritten}",
            "dataFactoryName": "@{pipeline().DataFactory}",
            "pipelineName": "@{pipeline().Pipeline}",
            "receiver": "@pipeline().parameters.receiver"
        }
        ```
        Az üzenettörzs a következő tulajdonságokat tartalmazza:

        - Message – Az átadott érték: `@{activity('Copy1').output.dataWritten`. Hozzáfér az előző másolási tevékenység egy tulajdonságához, és átadja a dataWritten értéket. Sikertelen művelet esetén az átadott érték a `@{activity('CopyBlobtoBlob').error.message` helyett a hibakimenet.
        - Data Factory Name – Az átadott érték: `@{pipeline().DataFactory}`. Ez egy rendszerváltozó, amely lehetővé teszi a megfelelő adat-előállító nevének elérését. A rendszerváltozók listáját lásd a [rendszerváltozókkal](control-flow-system-variables.md) kapcsolatos cikkben.
        - Pipeline Name – Az átadott érték: `@{pipeline().Pipeline}`. Ez szintén egy rendszerváltozó, amely lehetővé teszi a megfelelő folyamat nevének elérését. 
        - Receiver – Az átadott érték: "@pipeline().parameters.receiver"). Hozzáfér a folyamat paramétereihez.
    6. A **Beállítások** képernyő a következő képhez hasonlóan néz ki: 

        ![Az első webes tevékenység beállításai](./media/tutorial-control-flow-portal/web-activity1-settings.png)         
19. Kapcsolja össze a **Másolás** tevékenységet a **Webes** tevékenységgel úgy, hogy a másolási tevékenység melletti zöld gombot a webes tevékenységre húzza. 

    ![A másolási tevékenység összekapcsolása az első webes tevékenységgel](./media/tutorial-control-flow-portal/connect-copy-web-activity1.png)
20. Húzzon át egy másik **webes** tevékenységet a Tevékenységek eszközkészletből a folyamat tervezőfelületére, és adja meg a **SendFailureEmailActivity** **nevet**.

    ![A második webes tevékenység neve](./media/tutorial-control-flow-portal/web-activity2-name.png)
21. Váltson a **Beállítások** lapra, és végezze el az alábbi lépéseket:

    1. Az **URL** mezőben adja meg a sikertelen műveletet jelző e-mailt elküldő Logic Apps-munkafolyamat URL-címét.  
    2. A **Metódus** mezőben válassza a **POST** lehetőséget. 
    3. A **Fejlécek** szakaszban kattintson a **+ Fejléc hozzáadása** hivatkozásra. 
    4. Adjon hozzá egy **Content-Type** típusú fejlécet, amelynek beállítása **application/json**. 
    5. A **Törzs** értékeként adja meg a következő JSON-t. 

        ```json
        {
            "message": "@{activity('Copy1').error.message}",
            "dataFactoryName": "@{pipeline().DataFactory}",
            "pipelineName": "@{pipeline().Pipeline}",
            "receiver": "@pipeline().parameters.receiver"
        }
        ```
    6. A **Beállítások** képernyő a következő képhez hasonlóan néz ki: 
    
        ![A második webes tevékenység beállításai](./media/tutorial-control-flow-portal/web-activity2-settings.png)         
22. Válassza ki a **Másolás** tevékenységet a folyamattervezőben, kattintson a **+->** gombra, majd válassza a **Hiba** lehetőséget.  

    ![A második webes tevékenység beállításai](./media/tutorial-control-flow-portal/select-copy-failure-link.png)
23. Húzza a másolási tevékenység melletti **piros** gombot a **SendFailureEmailActivity** nevű második webes tevékenységre. Szabadon áthelyezheti a tevékenységeket, így a folyamat a következő képhez hasonlóan fog kinézni: 

    ![A teljes folyamat az összes tevékenységgel](./media/tutorial-control-flow-portal/full-pipeline.png)
24. A folyamat érvényesítéséhez kattintson a **Érvényesítés** gombra az eszköztáron. Zárja be a **Folyamatérvényesítés kimenete** ablakot a **>>** gombra kattintva.

    ![Folyamat érvényesítése](./media/tutorial-control-flow-portal/validate-pipeline.png)
24. Ha közzé szeretné tenni az entitásokat (adatkészleteket, folyamatokat stb.) a Data Factory szolgáltatásban, kattintson a **Közzététel** elemre. Várjon, amíg megjelenik a **Sikeres közzététel** üzenet.

    ![Közzététel](./media/tutorial-control-flow-portal/publish-button.png)
 
## <a name="trigger-a-pipeline-run-that-succeeds"></a>Sikeres folyamatfuttatás aktiválása
1. A folyamat futásának **aktiválásához** kattintson az **Aktiválás** gombra az eszköztáron, majd az **Aktiválás most** elemre. 

    ![Folyamat futtatásának aktiválása](./media/tutorial-control-flow-portal/trigger-now-menu.png)
2. A **Folyamatfuttatás** ablakban végezze el az alábbi lépéseket: 

    1. A **sourceBlobContainer** paraméterhez adja meg az **adftutorial/adfv2branch/input** értéket. 
    2. A **sinkBlobContainer** paraméterhez adja meg az **adftutorial/adfv2branch/output** értéket. 
    3. Adja meg a **címzett** **e-mail-címét**. 
    4. Kattintson a **Befejezés** gombra.

        ![Folyamatfuttatás paraméterei](./media/tutorial-control-flow-portal/pipeline-run-parameters.png)

## <a name="monitor-the-successful-pipeline-run"></a>A sikeres folyamatfuttatás monitorozása

1. A folyamatfuttatás monitorozásához váltson a bal oldali **Monitorozás** lapra. Itt az Ön által manuálisan aktivált folyamatfuttatást láthatja. A lista frissítéséhez kattintson a **Frissítés** elemre. 
    
    ![Sikeres folyamatfuttatás](./media/tutorial-control-flow-portal/monitor-success-pipeline-run.png)
2. A folyamatfuttatáshoz társított **tevékenységfuttatások megtekintéséhez** kattintson az első hivatkozásra a **Műveletek** oszlopban. A fenti **Folyamatok** elemre kattintva visszaválthat az előző nézetre. A lista frissítéséhez kattintson a **Frissítés** elemre. 

    ![Tevékenységfuttatások](./media/tutorial-control-flow-portal/activity-runs-success.png)

## <a name="trigger-a-pipeline-run-that-fails"></a>Sikertelen folyamatfuttatás aktiválása
1. Váltson a bal oldalon található **Szerkesztés** lapra. 
2. A folyamat futásának **aktiválásához** kattintson az **Aktiválás** gombra az eszköztáron, majd az **Aktiválás most** elemre. 
3. A **Folyamatfuttatás** ablakban végezze el az alábbi lépéseket: 

    1. A **sourceBlobContainer** paraméterhez adja meg az **adftutorial/dummy/input** értéket. Győződjön meg arról, hogy a helyőrző mappa nem létezik az adftutorial tárolóban. 
    2. A **sinkBlobContainer** paraméterhez adja meg az **adftutorial/dummy/inputt** értéket. 
    3. Adja meg a **címzett** **e-mail-címét**. 
    4. Kattintson a **Befejezés** gombra.

## <a name="monitor-the-failed-pipeline-run"></a>A sikertelen folyamatfuttatás monitorozása

1. A folyamatfuttatás monitorozásához váltson a bal oldali **Monitorozás** lapra. Itt az Ön által manuálisan aktivált folyamatfuttatást láthatja. A lista frissítéséhez kattintson a **Frissítés** elemre. 
    
    ![Sikertelen folyamatfuttatás](./media/tutorial-control-flow-portal/monitor-failure-pipeline-run.png)
2. A hiba részleteinek megtekintéséhez kattintson a folyamatfuttatás **Hiba** hivatkozására. 

    ![Folyamathiba](./media/tutorial-control-flow-portal/pipeline-error-message.png)
2. A folyamatfuttatáshoz társított **tevékenységfuttatások megtekintéséhez** kattintson az első hivatkozásra a **Műveletek** oszlopban. A lista frissítéséhez kattintson a **Frissítés** elemre. Látható, hogy a folyamat másolási tevékenysége meghiúsult. A webes tevékenység sikeresen elküldte a sikertelen műveletről tájékoztató e-mailt a megadott címzettnek. 

    ![Tevékenységfuttatások](./media/tutorial-control-flow-portal/activity-runs-failure.png)
4. A hiba részleteinek megtekintéséhez kattintson a **Hiba** hivatkozásra a **Műveletek** oszlopban. 

    ![Tevékenységfuttatási hiba](./media/tutorial-control-flow-portal/activity-run-error.png)

## <a name="next-steps"></a>További lépések
Az oktatóanyagban az alábbi lépéseket hajtotta végre: 

> [!div class="checklist"]
> * Adat-előállító létrehozása
> * Azure Storage-beli társított szolgáltatás létrehozása
> * Azure Blob-adatkészlet létrehozása
> * Másolási tevékenységet és webes tevékenységet tartalmazó folyamat létrehozása
> * Tevékenységek kimeneteinek elküldése a soron következő tevékenységek számára
> * Paraméterátadás és rendszerváltozók használata
> * Folyamat futásának indítása
> * A folyamat és a tevékenységek futásának monitorozása

Az Azure Data Factoryvel kapcsolatos további információkért folytassa az Alapelvek szakasz áttekintésével.
> [!div class="nextstepaction"]
>[Folyamatok és tevékenységek](concepts-pipelines-activities.md)
