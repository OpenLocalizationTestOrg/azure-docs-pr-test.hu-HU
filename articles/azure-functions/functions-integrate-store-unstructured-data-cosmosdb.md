---
title: "aaaStore Azure Functions és Cosmos DB strukturálatlan adatok"
description: "Strukturálatlan adatok tárolása az Azure Functions és a Cosmos DB használatával"
services: functions
documentationcenter: functions
author: rachelappel
manager: erikre
editor: 
tags: 
keywords: "azure functions, függvények, eseményfeldolgozás, Cosmos DB, dinamikus számítás, kiszolgáló nélküli architektúra"
ms.assetid: 
ms.service: functions
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/03/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 48d6899c20d3e6f6b062725fca329972ead3c696
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="store-unstructured-data-using-azure-functions-and-cosmos-db"></a>Strukturálatlan adatok tárolása az Azure Functions és a Cosmos DB használatával

[Az Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) egy strukturálatlan kiváló módja toostore és a JSON-adatokat. A Cosmos DB, az Azure Functions szolgáltatással kombinálva felgyorsítja és megkönnyíti az adattárolást, hiszen lényegesen kevesebb kódot igényel, mint amennyi egy relációs adatbázisban történő adattároláshoz szükséges.

Az Azure Functions bemeneti és kimeneti kötések adja meg a deklaratív módon tooconnect tooexternal szolgáltatás adatait a függvény. Ebben a témakörben megtudhatja, hogyan tooupdate meglévő C# működni tooadd egy strukturálatlan adatot tárol egy Cosmos DB dokumentum kimeneti kötése. 

![Cosmos DB](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-cosmosdb.png)

## <a name="prerequisites"></a>Előfeltételek

toocomplete Ez az oktatóanyag:

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

## <a name="add-an-output-binding"></a>Kimeneti kötés hozzáadása

1. Bontsa ki a függvényalkalmazást és a függvényt.

1. Válassza ki **integráció** és **+ új kimeneti**, amely jelenleg hello hello oldal jobb felső. Válassza az **Azure Cosmos DB** elemet, majd kattintson a **Kiválasztás** lehetőségre.

    ![Cosmos DB kimeneti kötésének hozzáadása](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-integrate-tab-add-new-output-binding.png)

3. Használjon hello **Azure Cosmos DB kimeneti** beállításokat hello táblázatban megadottak szerint: 

    ![A Cosmos DB kimeneti kötésének konfigurálása](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-integrate-tab-configure-cosmosdb-binding.png)

    | Beállítás      | Ajánlott érték  | Leírás                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | **Dokumentumparaméter neve** | taskDocument | A kódban toohello Cosmos DB objektum neve. |
    | **Adatbázis neve** | taskDatabase | Az adatbázis toosave dokumentumok neve. |
    | **Gyűjtemény neve** | TaskCollection | A Cosmos DB-adatbázisok gyűjteményének neve. |
    | **Igaz értéke esetén hello Cosmos DB adatbázist és a gyűjteményt hoz létre** | Bejelölve | hello gyűjtemény nem már létezik, ezért hozza létre. |

4. Válassza ki **új** következő toohello **Cosmos DB dokumentum kapcsolat** címkével, és válassza a **+ új létrehozása**. 

5. Használjon hello **új fiók** beállításokat hello táblázatban megadottak szerint: 

    ![A Cosmos DB-kapcsolat konfigurálása](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-create-CosmosDB.png)

    | Beállítás      | Ajánlott érték  | Leírás                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | **Azonosító** | Az adatbázis neve | Hello Cosmos DB adatbázis egyedi azonosítója  |
    | **API** | SQL (DocumentDB) | Hello dokumentum adatbázis az API lehetőséget választhatja.  |
    | **Előfizetés** | Azure-előfizetés | Azure-előfizetés  |
    | **Erőforráscsoport** | myResourceGroup |  Hello meglévő erőforráscsoportot, amely tartalmazza az függvény alkalmazás használja. |
    | **Hely**  | WestEurope | Jelöljön ki egy helyet tooeither közelében, a függvény alkalmazás vagy hello tárolt dokumentumok használó tooother alkalmazások.  |

6. Kattintson a **OK** toocreate hello adatbázis. Eltarthat néhány percig toocreate hello adatbázis. Hello adatbázis létrehozását követően hello adatbázis-kapcsolati karakterlánc egy függvény Alkalmazásbeállítás tárolja. az Alkalmazásbeállítás neve hello behelyezi **Cosmos DB fiók kapcsolat**. 
 
8. Hello kapcsolati karakterlánc beállítása után válassza ki a **mentése** toocreate hello kötés.

## <a name="update-hello-function-code"></a>Hello funkciókódot frissítése

Hello meglévő C# funkciókódot cserélje le a következő kód hello:

```csharp
using System.Net;

public static HttpResponseMessage Run(HttpRequestMessage req, out object taskDocument, TraceWriter log)
{
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    string task = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "task", true) == 0)
        .Value;

    string duedate = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "duedate", true) == 0)
        .Value;

    taskDocument = new {
        name = name,
        duedate = duedate.ToString(),
        task = task
    };

    if (name != "" && task != "") {
        return req.CreateResponse(HttpStatusCode.OK);
    }
    else {
        return req.CreateResponse(HttpStatusCode.BadRequest);
    }
}

```
A fenti beolvassa hello HTTP-kérelem lekérdezési karakterláncok, és hozzárendeli a hello toofields `taskDocument` objektum. Hello `taskDocument` kötés küldi hello objektumadatokat a kötési paraméter toobe hello kötött dokumentum-adatbázis tárolja. hello adatbázis hello hello függvény első futásakor létrejön.

## <a name="test-hello-function-and-database"></a>Teszt hello függvény és adatbázis

1. Bontsa ki a hello jobb oldali, és válassza ki **teszt**. A **lekérdezés**, kattintson a **+ Hozzáadás paraméter** , és adja hozzá a következő paraméterek toohello lekérdezési karakterlánc hello:

    + `name`
    + `task`
    + `duedate`

2. Kattintson a **Futtatás** parancsra, és ellenőrizze, hogy a rendszer 200-as állapotüzenetet ad-e vissza.

    ![A Cosmos DB kimeneti kötésének konfigurálása](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-test-function.png)

1. A bal oldalán található hello Azure-portálon hello, bontsa ki a hello ikon sávon típus `cosmos` hello a keresési mezőbe, majd válassza **Azure Cosmos DB**.

    ![Keresse meg a hello Cosmos DB szolgáltatás](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-search-cosmos-db.png)

2. Jelölje be hello adatbázis hozott létre, majd válassza ki **adatkezelő**. Bontsa ki a hello **gyűjtemények** csomópontot, válassza ki a hello új dokumentum, és győződjön meg arról, hogy hello dokumentum a lekérdezési karakterlánc-értékek, valamint néhány további metaadatokat tartalmaz. 

    ![A Cosmos DB-bejegyzés ellenőrzése](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-verify-cosmosdb-output.png)

Sikeresen hozzáadta a kötés tooyour HTTP eseményindító strukturálatlan adatokat tároló Cosmos DB adatbázisban.

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Következő lépések

[!INCLUDE [functions-quickstart-next-steps](../../includes/functions-quickstart-next-steps.md)]

További információ a kötés tooa Cosmos DB adatbázisban: [Azure Functions Cosmos DB kötések](functions-bindings-documentdb.md).
