---
title: "Strukturálatlan adatok tárolása az Azure Functions és a Cosmos DB használatával"
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
ms.openlocfilehash: 7c18676ff94ec7da17094abc5f33fb3c6a79895f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="store-unstructured-data-using-azure-functions-and-cosmos-db"></a><span data-ttu-id="99496-104">Strukturálatlan adatok tárolása az Azure Functions és a Cosmos DB használatával</span><span class="sxs-lookup"><span data-stu-id="99496-104">Store unstructured data using Azure Functions and Cosmos DB</span></span>

<span data-ttu-id="99496-105">Az [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) kiválóan alkalmas strukturálatlan és JSON-adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="99496-105">[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) is a great way to store unstructured and JSON data.</span></span> <span data-ttu-id="99496-106">A Cosmos DB, az Azure Functions szolgáltatással kombinálva felgyorsítja és megkönnyíti az adattárolást, hiszen lényegesen kevesebb kódot igényel, mint amennyi egy relációs adatbázisban történő adattároláshoz szükséges.</span><span class="sxs-lookup"><span data-stu-id="99496-106">Combined with Azure Functions, Cosmos DB makes storing data quick and easy with much less code than required for storing data in a relational database.</span></span>

<span data-ttu-id="99496-107">Az Azure Functions bemeneti és kimeneti kötései deklaratív módszert biztosítanak a külső szolgáltatások adataihoz a függvényből történő csatlakozásra.</span><span class="sxs-lookup"><span data-stu-id="99496-107">In Azure Functions, input and output bindings provide a declarative way to connect to external service data from your function.</span></span> <span data-ttu-id="99496-108">Ebben a témakörben megtudhatja, hogyan módosíthat egy meglévő C#-függvényt egy kimeneti kötés hozzáadásához, amely strukturálatlan adatokat tárol egy Cosmos DB-dokumentumban.</span><span class="sxs-lookup"><span data-stu-id="99496-108">In this topic, learn how to update an existing C# function to add an output binding that stores unstructured data in a Cosmos DB document.</span></span> 

![Cosmos DB](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-cosmosdb.png)

## <a name="prerequisites"></a><span data-ttu-id="99496-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="99496-110">Prerequisites</span></span>

<span data-ttu-id="99496-111">Az oktatóanyag elvégzéséhez:</span><span class="sxs-lookup"><span data-stu-id="99496-111">To complete this tutorial:</span></span>

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

## <a name="add-an-output-binding"></a><span data-ttu-id="99496-112">Kimeneti kötés hozzáadása</span><span class="sxs-lookup"><span data-stu-id="99496-112">Add an output binding</span></span>

1. <span data-ttu-id="99496-113">Bontsa ki a függvényalkalmazást és a függvényt.</span><span class="sxs-lookup"><span data-stu-id="99496-113">Expand both your function app and your function.</span></span>

1. <span data-ttu-id="99496-114">Válassza az oldal jobb felső sarkában található **Integráció** és **+ Új kimenet** elemeket.</span><span class="sxs-lookup"><span data-stu-id="99496-114">Select **Integrate** and **+ New Output**, which is at the top right of the page.</span></span> <span data-ttu-id="99496-115">Válassza az **Azure Cosmos DB** elemet, majd kattintson a **Kiválasztás** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="99496-115">Choose **Azure Cosmos DB**, and click **Select**.</span></span>

    ![Cosmos DB kimeneti kötésének hozzáadása](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-integrate-tab-add-new-output-binding.png)

3. <span data-ttu-id="99496-117">Használja a táblázatban megadott **Azure Cosmos DB kimeneti** beállításokat:</span><span class="sxs-lookup"><span data-stu-id="99496-117">Use the **Azure Cosmos DB output** settings as specified in the table:</span></span> 

    ![A Cosmos DB kimeneti kötésének konfigurálása](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-integrate-tab-configure-cosmosdb-binding.png)

    | <span data-ttu-id="99496-119">Beállítás</span><span class="sxs-lookup"><span data-stu-id="99496-119">Setting</span></span>      | <span data-ttu-id="99496-120">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="99496-120">Suggested value</span></span>  | <span data-ttu-id="99496-121">Leírás</span><span class="sxs-lookup"><span data-stu-id="99496-121">Description</span></span>                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | <span data-ttu-id="99496-122">**Dokumentumparaméter neve**</span><span class="sxs-lookup"><span data-stu-id="99496-122">**Document parameter name**</span></span> | <span data-ttu-id="99496-123">taskDocument</span><span class="sxs-lookup"><span data-stu-id="99496-123">taskDocument</span></span> | <span data-ttu-id="99496-124">A Cosmos DB-objektumra utaló név a kódban.</span><span class="sxs-lookup"><span data-stu-id="99496-124">Name that refers to the Cosmos DB object in code.</span></span> |
    | <span data-ttu-id="99496-125">**Adatbázis neve**</span><span class="sxs-lookup"><span data-stu-id="99496-125">**Database name**</span></span> | <span data-ttu-id="99496-126">taskDatabase</span><span class="sxs-lookup"><span data-stu-id="99496-126">taskDatabase</span></span> | <span data-ttu-id="99496-127">Adatbázis neve a dokumentumok mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="99496-127">Name of database to save documents.</span></span> |
    | <span data-ttu-id="99496-128">**Gyűjtemény neve**</span><span class="sxs-lookup"><span data-stu-id="99496-128">**Collection name**</span></span> | <span data-ttu-id="99496-129">TaskCollection</span><span class="sxs-lookup"><span data-stu-id="99496-129">TaskCollection</span></span> | <span data-ttu-id="99496-130">A Cosmos DB-adatbázisok gyűjteményének neve.</span><span class="sxs-lookup"><span data-stu-id="99496-130">Name of collection of Cosmos DB databases.</span></span> |
    | <span data-ttu-id="99496-131">**Ha az értéke true, létrehozza a Cosmos DB-adatbázist és -gyűjteményt**</span><span class="sxs-lookup"><span data-stu-id="99496-131">**If true, creates the Cosmos DB database and collection**</span></span> | <span data-ttu-id="99496-132">Bejelölve</span><span class="sxs-lookup"><span data-stu-id="99496-132">Checked</span></span> | <span data-ttu-id="99496-133">A gyűjtemény még nem létezik, hozza létre.</span><span class="sxs-lookup"><span data-stu-id="99496-133">The collection doesn't already exist, so create it.</span></span> |

4. <span data-ttu-id="99496-134">Kattintson a **Cosmos DB-dokumentumkapcsolat** címke melletti **Új** gombra, és válassza az **+ Új létrehozása** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="99496-134">Select **New** next to the **Cosmos DB document connection** label, and select **+ Create new**.</span></span> 

5. <span data-ttu-id="99496-135">Használja a táblázatban megadott, **új fiókra** vonatkozó beállításokat:</span><span class="sxs-lookup"><span data-stu-id="99496-135">Use the **New account** settings as specified in the table:</span></span> 

    ![A Cosmos DB-kapcsolat konfigurálása](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-create-CosmosDB.png)

    | <span data-ttu-id="99496-137">Beállítás</span><span class="sxs-lookup"><span data-stu-id="99496-137">Setting</span></span>      | <span data-ttu-id="99496-138">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="99496-138">Suggested value</span></span>  | <span data-ttu-id="99496-139">Leírás</span><span class="sxs-lookup"><span data-stu-id="99496-139">Description</span></span>                                |
    | ------------ | ---------------- | ------------------------------------------ |
    | <span data-ttu-id="99496-140">**Azonosító**</span><span class="sxs-lookup"><span data-stu-id="99496-140">**ID**</span></span> | <span data-ttu-id="99496-141">Az adatbázis neve</span><span class="sxs-lookup"><span data-stu-id="99496-141">Name of database</span></span> | <span data-ttu-id="99496-142">A Cosmos DB adatbázis egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="99496-142">Unique ID for the Cosmos DB database</span></span>  |
    | <span data-ttu-id="99496-143">**API**</span><span class="sxs-lookup"><span data-stu-id="99496-143">**API**</span></span> | <span data-ttu-id="99496-144">SQL (DocumentDB)</span><span class="sxs-lookup"><span data-stu-id="99496-144">SQL (DocumentDB)</span></span> | <span data-ttu-id="99496-145">Válassza ki a dokumentum-adatbázis API-ját.</span><span class="sxs-lookup"><span data-stu-id="99496-145">Select the document database API.</span></span>  |
    | <span data-ttu-id="99496-146">**Előfizetés**</span><span class="sxs-lookup"><span data-stu-id="99496-146">**Subscription**</span></span> | <span data-ttu-id="99496-147">Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="99496-147">Azure Subscription</span></span> | <span data-ttu-id="99496-148">Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="99496-148">Azure Subscription</span></span>  |
    | <span data-ttu-id="99496-149">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="99496-149">**Resource Group**</span></span> | <span data-ttu-id="99496-150">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="99496-150">myResourceGroup</span></span> |  <span data-ttu-id="99496-151">Használja a függvényalkalmazást tartalmazó meglévő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="99496-151">Use the existing resource group that contains your function app.</span></span> |
    | <span data-ttu-id="99496-152">**Hely**</span><span class="sxs-lookup"><span data-stu-id="99496-152">**Location**</span></span>  | <span data-ttu-id="99496-153">WestEurope</span><span class="sxs-lookup"><span data-stu-id="99496-153">WestEurope</span></span> | <span data-ttu-id="99496-154">Olyan helyet válasszon, amely közel van a függvényalkalmazáshoz vagy a tárolt dokumentumokat használó egyéb alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="99496-154">Select a location near to either your function app or to other apps that use the stored documents.</span></span>  |

6. <span data-ttu-id="99496-155">Az adatbázis létrehozásához kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="99496-155">Click **OK** to create the database.</span></span> <span data-ttu-id="99496-156">Az adatbázis létrehozása eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="99496-156">It may take a few minutes to create the database.</span></span> <span data-ttu-id="99496-157">Az adatbázis létrehozása után a rendszer az adatbázis kapcsolati karakterláncát függvényalkalmazás-beállításként tárolja.</span><span class="sxs-lookup"><span data-stu-id="99496-157">After the database is created, the database connection string is stored as a function app setting.</span></span> <span data-ttu-id="99496-158">Ennek az alkalmazásbeállításnak a neve van beszúrva a **Cosmos DB-fiókkapcsolatba**.</span><span class="sxs-lookup"><span data-stu-id="99496-158">The name of this app setting is inserted in **Cosmos DB account connection**.</span></span> 
 
8. <span data-ttu-id="99496-159">A kapcsolati karakterlánc beállítása után a kötés létrehozásához válassza a **Mentés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="99496-159">After the connection string is set, select **Save** to create the binding.</span></span>

## <a name="update-the-function-code"></a><span data-ttu-id="99496-160">A függvénykód módosítása</span><span class="sxs-lookup"><span data-stu-id="99496-160">Update the function code</span></span>

<span data-ttu-id="99496-161">Cserélje le a meglévő C#-függvénykódot a következő kódra:</span><span class="sxs-lookup"><span data-stu-id="99496-161">Replace the existing C# function code with the following code:</span></span>

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
<span data-ttu-id="99496-162">A mintakód beolvassa a HTTP-kérelem karakterláncait, és egy `taskDocument` objektum mezőihez rendeli azokat.</span><span class="sxs-lookup"><span data-stu-id="99496-162">This code sample reads the HTTP Request query strings and assigns them to fields in the `taskDocument` object.</span></span> <span data-ttu-id="99496-163">A `taskDocument` kötés elküldi az ezen kötési paramétertől származó objektumadatokat a kötött dokumentum-adatbázisba tárolásra.</span><span class="sxs-lookup"><span data-stu-id="99496-163">The `taskDocument` binding sends the object data from this binding parameter to be stored in the bound document database.</span></span> <span data-ttu-id="99496-164">Az adatbázis a függvény első futtatásakor jön létre.</span><span class="sxs-lookup"><span data-stu-id="99496-164">The database is created the first time the function runs.</span></span>

## <a name="test-the-function-and-database"></a><span data-ttu-id="99496-165">A függvény és az adatbázis tesztelése</span><span class="sxs-lookup"><span data-stu-id="99496-165">Test the function and database</span></span>

1. <span data-ttu-id="99496-166">Bontsa ki a jobb oldali ablakot, és válassza a **Teszt** elemet.</span><span class="sxs-lookup"><span data-stu-id="99496-166">Expand the right window and select **Test**.</span></span> <span data-ttu-id="99496-167">A **Lekérdezés** területen kattintson a **+ Paraméter hozzáadása** elemre, és adja hozzá a következő paramétereket a lekérdezési karakterlánchoz:</span><span class="sxs-lookup"><span data-stu-id="99496-167">Under **Query**, click **+ Add parameter** and add the following parameters to the query string:</span></span>

    + `name`
    + `task`
    + `duedate`

2. <span data-ttu-id="99496-168">Kattintson a **Futtatás** parancsra, és ellenőrizze, hogy a rendszer 200-as állapotüzenetet ad-e vissza.</span><span class="sxs-lookup"><span data-stu-id="99496-168">Click **Run** and verify that a 200 status is returned.</span></span>

    ![A Cosmos DB kimeneti kötésének konfigurálása](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-test-function.png)

1. <span data-ttu-id="99496-170">Az Azure Portal bal oldali menüjében bontsa ki az ikonsort, gépelje be a `cosmos` szöveget a keresőmezőbe, és válassza az **Azure Cosmos DB** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="99496-170">On the left side of the Azure portal, expand the icon bar, type `cosmos` in the search field, and select **Azure Cosmos DB**.</span></span>

    ![A Cosmos DB szolgáltatás megkeresése](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-search-cosmos-db.png)

2. <span data-ttu-id="99496-172">Válassza ki a létrehozott adatbázist, majd válassza az **Adatkezelő** elemet.</span><span class="sxs-lookup"><span data-stu-id="99496-172">Select the database you created, then select **Data Explorer**.</span></span> <span data-ttu-id="99496-173">Bontsa ki a **Gyűjtemények** csomópontokat, válassza ki az új dokumentumot, és győződjön meg arról, hogy a dokumentum tartalmazza a lekérdezési karakterlánc értékeit, valamint további metaadatokat.</span><span class="sxs-lookup"><span data-stu-id="99496-173">Expand the **Collections** nodes, select the new document, and confirm that the document contains your query string values, along with some additional metadata.</span></span> 

    ![A Cosmos DB-bejegyzés ellenőrzése](./media/functions-integrate-store-unstructured-data-cosmosdb/functions-verify-cosmosdb-output.png)

<span data-ttu-id="99496-175">Sikeresen hozzáadott egy kötést a Cosmos DB-adatbázisban strukturálatlan adatokat tároló HTTP-eseményindítóhoz.</span><span class="sxs-lookup"><span data-stu-id="99496-175">You have successfully added a binding to your HTTP trigger that stores unstructured data in a Cosmos DB database.</span></span>

[!INCLUDE [Clean-up section](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="99496-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="99496-176">Next steps</span></span>

[!INCLUDE [functions-quickstart-next-steps](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="99496-177">A Cosmos DB-adatbázisokhoz végzett kötésről további információt az [Azure Functions Cosmos DB-kötéseket](functions-bindings-documentdb.md) ismertető cikk tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="99496-177">For more information about binding to a Cosmos DB database, see [Azure Functions Cosmos DB bindings](functions-bindings-documentdb.md).</span></span>
