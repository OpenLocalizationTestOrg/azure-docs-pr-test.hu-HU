---
title: "Azure Cosmos DB: Webalkalmazás létrehozása .NET-tel és DocumentDB API-val | Microsoft Docs"
description: "Egy .NET-es kódmintát mutat be, amellyel csatlakozni lehet az Azure Cosmos DB DocumentDB API-hoz és lekérdezést lehet végezni vele"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 9bb863261da64c97f99757d4a0cb3474a7755591
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cosmos-db-build-a-documentdb-api-web-app-with-net-and-the-azure-portal"></a><span data-ttu-id="ef2bb-103">Azure Cosmos DB: DocumentDB API-webalkalmazás létrehozása .NET-tel és az Azure Portallal</span><span class="sxs-lookup"><span data-stu-id="ef2bb-103">Azure Cosmos DB: Build a DocumentDB API web app with .NET and the Azure portal</span></span>

<span data-ttu-id="ef2bb-104">Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="ef2bb-105">Segítségével gyorsan létrehozhat és lekérdezhet dokumentum-, kulcs/érték és gráf típusú adatbázisokat, melyek mindegyike felhasználja az Azure Cosmos DB középpontjában álló globális elosztási és horizontális skálázhatósági képességeket.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="ef2bb-106">Ez a rövid útmutató bemutatja, hogyan hozhat létre az Azure Portal segítségével Azure Cosmos DB-fiókot, dokumentum-adatbázist és gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-106">This quick start demonstrates how to create an Azure Cosmos DB account, document database, and collection using the Azure portal.</span></span> <span data-ttu-id="ef2bb-107">Ezután megtudhatja, hogyan hozhat létre és hogyan helyezhet üzembe egy a [DocumentDB .NET API-n](documentdb-sdk-dotnet.md) alapuló teendőlista-kezelő webalkalmazást. (Lásd az alábbi képernyőfelvételen.)</span><span class="sxs-lookup"><span data-stu-id="ef2bb-107">You'll then build and deploy a todo list web app built on the [DocumentDB .NET API](documentdb-sdk-dotnet.md), as shown in the following screenshot.</span></span> 

![Teendőkezelő alkalmazás mintaadatokkal](./media/create-documentdb-dotnet/azure-comosdb-todo-app-list.png)

## <a name="prerequisites"></a><span data-ttu-id="ef2bb-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ef2bb-109">Prerequisites</span></span>

<span data-ttu-id="ef2bb-110">Ha nincs telepítve a Visual Studio 2017, letöltheti és használhatja az **ingyenes** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)t.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-110">If you don’t already have Visual Studio 2017 installed, you can download and use the **free** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="ef2bb-111">Ügyeljen arra, hogy engedélyezze az **Azure Development** használatát a Visual Studio telepítése során.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-111">Make sure that you enable **Azure development** during the Visual Studio setup.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-account"></a>
## <a name="create-a-database-account"></a><span data-ttu-id="ef2bb-112">Adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="ef2bb-112">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<a id="create-collection"></a>
## <a name="add-a-collection"></a><span data-ttu-id="ef2bb-113">Gyűjtemény hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ef2bb-113">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a><span data-ttu-id="ef2bb-114">Mintaadatok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ef2bb-114">Add sample data</span></span>

<span data-ttu-id="ef2bb-115">Az Adatkezelő segítségével adatokat adhat hozzá az új gyűjteményhez.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-115">You can now add data to your new collection using Data Explorer.</span></span>

1. <span data-ttu-id="ef2bb-116">Az új adatbázis az Adatkezelőben a Gyűjtemények ablaktáblán jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-116">In Data Explorer, the new database appears in the Collections pane.</span></span> <span data-ttu-id="ef2bb-117">Bontsa ki a **Feladatok** adatbázist, majd az **Elemek** gyűjteményt, végül kattintson a **Dokumentumok**, majd pedig az **Új dokumentumok** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-117">Expand the **Tasks** database, expand the **Items** collection, click **Documents**, and then click **New Documents**.</span></span> 

   ![Új dokumentumok létrehozása az Azure Portal Adatkezelőjében](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
2. <span data-ttu-id="ef2bb-119">Adjon hozzá egy dokumentumot a gyűjteményhez az alábbi struktúrával.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-119">Now add a document to the collection with the following structure.</span></span>

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

3. <span data-ttu-id="ef2bb-120">A JSON hozzáadása után a **Dokumentumok** lapon kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-120">Once you've added the json to the **Documents** tab, click **Save**.</span></span>

    ![Másolja át a json-adatokat, és kattintson a Mentés gombra az Adatkezelőben az Azure-portálon](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-save-document.png)

4.  <span data-ttu-id="ef2bb-122">Hozzon létre és mentsen még egy dokumentumot, amelyben egyedi értéket szúr be az `id` tulajdonság számára, és tetszés szerint módosítja a többi tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-122">Create and save one more document where you insert a unique value for the `id` property, and change the other properties as you see fit.</span></span> <span data-ttu-id="ef2bb-123">Mivel az Azure Cosmos DB nem kötelezi egy adott adatséma használatára, új dokumentumaihoz bármilyen struktúrát választhat.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-123">Your new documents can have any structure you want as Azure Cosmos DB doesn't impose any schema on your data.</span></span>

     <span data-ttu-id="ef2bb-124">Az Adatkezelővel így már lekérdezések használatával lekérheti adatait.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-124">You can now use queries in Data Explorer to retrieve your data.</span></span> <span data-ttu-id="ef2bb-125">Az Adatkezelő alapértelmezés szerint a `SELECT * FROM c` lekérdezést használja a gyűjteményben lévő összes dokumentum lekéréséhez, de ezt más [SQL-lekérdezésre](documentdb-sql-query.md) is módosíthatja, például a `SELECT * FROM c ORDER BY c._ts DESC` lekérdezésre, ha azt szeretné, hogy a rendszer a dokumentumokat időbélyegzőik szerint csökkenő sorrendben adja vissza.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-125">By default, Data Explorer uses `SELECT * FROM c` to retrieve all documents in the collection, but you can change that to a different [SQL query](documentdb-sql-query.md), such as `SELECT * FROM c ORDER BY c._ts DESC`, to return all the documents in descending order based on their timestamp.</span></span>
 
     <span data-ttu-id="ef2bb-126">Az Adatkezelővel létrehozhat tárolt eljárásokat is, felhasználói függvényeket és a kiszolgálóoldali üzleti logikákat végrehajtó eseményindítókat, valamint szabályozhatja az átviteli sebességet.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-126">You can also use Data Explorer to create stored procedures, UDFs, and triggers to perform server-side business logic as well as scale throughput.</span></span> <span data-ttu-id="ef2bb-127">Az Adatkezelő hozzáférhetővé teszi az API-k összes beépített, programozható adatelérési funkcióját, és az Azure Portalon tárolt adataihoz is egyszerű hozzáférést biztosít.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-127">Data Explorer exposes all of the built-in programmatic data access available in the APIs, but provides easy access to your data in the Azure portal.</span></span>

## <a name="clone-the-sample-application"></a><span data-ttu-id="ef2bb-128">A mintaalkalmazás klónozása</span><span class="sxs-lookup"><span data-stu-id="ef2bb-128">Clone the sample application</span></span>

<span data-ttu-id="ef2bb-129">Most pedig váltsunk át kódok használatára.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-129">Now let's switch to working with code.</span></span> <span data-ttu-id="ef2bb-130">Klónozunk egy DocumentDB API-alkalmazást a GitHubról, beállítjuk a kapcsolati karakterláncot, és futtatjuk az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-130">Let's clone a DocumentDB API app from GitHub, set the connection string, and run it.</span></span> <span data-ttu-id="ef2bb-131">Látni fogja, milyen egyszerű az adatokkal programozott módon dolgozni.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-131">You'll see how easy it is to work with data programmatically.</span></span> 

1. <span data-ttu-id="ef2bb-132">Nyisson meg egy git terminálablakot, például a git bash eszközt, és a `CD` paranccsal lépjen egy munkakönyvtárba.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-132">Open a git terminal window, such as git bash, and `CD` to a working directory.</span></span>  

2. <span data-ttu-id="ef2bb-133">Futtassa a következő parancsot a mintatárház klónozásához.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-133">Run the following command to clone the sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/documentdb-dotnet-todo-app.git
    ```

3. <span data-ttu-id="ef2bb-134">Ezután nyissa meg a teendő megoldásfájlját a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-134">Then open the todo solution file in Visual Studio.</span></span> 

## <a name="review-the-code"></a><span data-ttu-id="ef2bb-135">A kód áttekintése</span><span class="sxs-lookup"><span data-stu-id="ef2bb-135">Review the code</span></span>

<span data-ttu-id="ef2bb-136">Tekintsük át, hogy mi is történik az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-136">Let's make a quick review of what's happening in the app.</span></span> <span data-ttu-id="ef2bb-137">Nyissa meg a DocumentDBRepository.cs fájlt: az itt található kódsorok hozzák létre az Azure Cosmos DB-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-137">Open the DocumentDBRepository.cs file and you'll find that these lines of code create the Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="ef2bb-138">A DocumentClient inicializálva van a 73. sorban.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-138">The DocumentClient is initialized on line 73.</span></span>

    ```csharp
    client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);`
    ```

* <span data-ttu-id="ef2bb-139">A rendszer létrehozza az új adatbázist a 88. sorban.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-139">A new database is created on line 88.</span></span>

    ```csharp
    await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
    ```

* <span data-ttu-id="ef2bb-140">A rendszer létrehozza az új gyűjteményt a 107. sorban.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-140">A new collection is created on line 107.</span></span>

    ```csharp
    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(DatabaseId),
        new DocumentCollection { Id = CollectionId },
        new RequestOptions { OfferThroughput = 1000 });
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="ef2bb-141">A kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="ef2bb-141">Update your connection string</span></span>

<span data-ttu-id="ef2bb-142">Lépjen vissza az Azure Portalra a kapcsolati karakterlánc adataiért, majd másolja be azokat az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-142">Now go back to the Azure portal to get your connection string information and copy it into the app.</span></span>

1. <span data-ttu-id="ef2bb-143">Az [Azure Portalon](http://portal.azure.com/) az Azure Cosmos DB-fiókban a bal oldalsávon kattintson a **Kulcsok** elemre, majd kattintson az **Írási/olvasási kulcsok** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-143">In the [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in the left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="ef2bb-144">A következő lépésben a képernyő jobb oldalán lévő másolási gombokkal másolhatja az URI-t és az elsődleges kulcsot a web.config fájlba.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-144">You'll use the copy buttons on the right side of the screen to copy the URI and Primary Key into the web.config file in the next step.</span></span>

    ![Hozzáférési kulcs megtekintése és másolása az Azure Portal Kulcsok panelén](./media/create-documentdb-dotnet/keys.png)

2. <span data-ttu-id="ef2bb-146">Nyissa meg a web.config fájlt a Visual Studio 2017 alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-146">In Visual Studio 2017, open the web.config file.</span></span> 

3. <span data-ttu-id="ef2bb-147">A másolási gomb használatával másolja ki az URI érteket a Portalról, és azt adja meg a végpont kulcs értékeként a web.config fájlban.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-147">Copy your URI value from the portal (using the copy button) and make it the value of the endpoint key in web.config.</span></span> 

    `<add key="endpoint" value="FILLME" />`

4. <span data-ttu-id="ef2bb-148">Ezután másolja ki az ELSŐDLEGES KULCS értékét a Portalról, és azt adja meg az authKey értékeként a web.config fájlban. Az alkalmazás frissítve lett minden olyan információval, amely az Azure Cosmos DB-vel való kommunikációhoz szükséges.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-148">Then copy your PRIMARY KEY value from the portal and make it the value of the authKey in web.config. You've now updated your app with all the info it needs to communicate with Azure Cosmos DB.</span></span> 

    `<add key="authKey" value="FILLME" />`
    
## <a name="run-the-web-app"></a><span data-ttu-id="ef2bb-149">A webalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="ef2bb-149">Run the web app</span></span>
1. <span data-ttu-id="ef2bb-150">A Visual Studióban kattintson a jobb gombbal a projektre a **Megoldáskezelőben**, majd kattintson a **NuGet-csomagok kezelése** elemre.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-150">In Visual Studio, right-click on the project in **Solution Explorer** and then click **Manage NuGet Packages**.</span></span> 

2. <span data-ttu-id="ef2bb-151">A NuGet **Tallózás** mezőjébe írja be a *DocumentDB* szöveget.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-151">In the NuGet **Browse** box, type *DocumentDB*.</span></span>

3. <span data-ttu-id="ef2bb-152">Az eredmények közül telepítse a **Microsoft.Azure.DocumentDB** kódtárat.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-152">From the results, install the **Microsoft.Azure.DocumentDB** library.</span></span> <span data-ttu-id="ef2bb-153">Ez telepíti a Microsoft.Azure.DocumentDB csomagot, valamint annak összes függőségét.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-153">This installs the Microsoft.Azure.DocumentDB package as well as all dependencies.</span></span>

4. <span data-ttu-id="ef2bb-154">Az alkalmazás futtatásához nyomja le a CTRL + F5 billentyűkombinációt.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-154">Click CTRL + F5 to run the application.</span></span> <span data-ttu-id="ef2bb-155">Az alkalmazás megjelenik a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-155">Your app displays in your browser.</span></span> 

5. <span data-ttu-id="ef2bb-156">Kattintson a **Új létrehozása** lehetőségre a böngészőben, és hozzon létre néhány új feladatot a teendőkezelő alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-156">Click **Create New** in the browser and create a few new tasks in your to-do app.</span></span>

   ![Teendőkezelő alkalmazás mintaadatokkal](./media/create-documentdb-dotnet/azure-comosdb-todo-app-list.png)

<span data-ttu-id="ef2bb-158">Lépjen vissza az Adatkezelőbe, ahol lekérdezheti és módosíthatja az új adatokat, valamint dolgozhat azokkal.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-158">You can now go back to Data Explorer and see query, modify, and work with this new data.</span></span> 

## <a name="review-slas-in-the-azure-portal"></a><span data-ttu-id="ef2bb-159">Az SLA-k áttekintése az Azure Portalon</span><span class="sxs-lookup"><span data-stu-id="ef2bb-159">Review SLAs in the Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="ef2bb-160">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="ef2bb-160">Clean up resources</span></span>

<span data-ttu-id="ef2bb-161">Ha az alkalmazást már nem használja, akkor a következő lépésekkel a mintaalkalmazás által létrehozott összes erőforrást törölheti az Azure Portalon:</span><span class="sxs-lookup"><span data-stu-id="ef2bb-161">If you're not going to continue to use this app, delete all resources created by this quickstart in the Azure portal with the following steps:</span></span>

1. <span data-ttu-id="ef2bb-162">Az Azure Portal bal oldali menüjében kattintson az **Erőforráscsoportok** lehetőségre, majd kattintson a létrehozott erőforrás nevére.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-162">From the left-hand menu in the Azure portal, click **Resource groups** and then click the name of the resource you created.</span></span> 
2. <span data-ttu-id="ef2bb-163">Az erőforráscsoport lapján kattintson a **Törlés** elemre, írja be a törölni kívánt erőforrás nevét a szövegmezőbe, majd kattintson a **Törlés** gombra.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-163">On your resource group page, click **Delete**, type the name of the resource to delete in the text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ef2bb-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ef2bb-164">Next steps</span></span>

<span data-ttu-id="ef2bb-165">Ebben a rövid útmutatóban megtudtuk, hogyan lehet Azure Cosmos DB-fiókot létrehozni, hogyan lehet az Adatkezelő segítségével gyűjteményt készíteni, és hogyan lehet futtatni a webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-165">In this quickstart, you've learned how to create an Azure Cosmos DB account, create a collection using the Data Explorer, and run a web app.</span></span> <span data-ttu-id="ef2bb-166">Így már további adatokat importálhat a Cosmos DB-fiókba.</span><span class="sxs-lookup"><span data-stu-id="ef2bb-166">You can now import additional data to your Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="ef2bb-167">Adatok importálása az Azure Cosmos DB-be</span><span class="sxs-lookup"><span data-stu-id="ef2bb-167">Import data into Azure Cosmos DB</span></span>](import-data.md)


