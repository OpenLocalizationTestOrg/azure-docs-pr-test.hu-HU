---
title: "Azure Cosmos DB .NET-alkalmazás létrehozása a Graph API-val | Microsoft Docs"
description: "Egy .NET-kódmintát mutat be, amellyel csatlakozhat egy Cosmos DB-adatbázishoz, és lekérdezéseket hajthat végre."
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/28/2017
ms.author: denlee
ms.openlocfilehash: a973b81ea5b06c5826cc31c399aae9dec43f5b72
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-cosmos-db-build-a-net-application-using-the-graph-api"></a><span data-ttu-id="9ee71-103">Azure Cosmos DB: .NET-alkalmazás létrehozása a Graph API-val</span><span class="sxs-lookup"><span data-stu-id="9ee71-103">Azure Cosmos DB: Build a .NET application using the Graph API</span></span>

<span data-ttu-id="9ee71-104">Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="9ee71-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="9ee71-105">Segítségével gyorsan létrehozhat és lekérdezhet dokumentum-, kulcs/érték és gráf típusú adatbázisokat, melyek mindegyike felhasználja az Azure Cosmos DB középpontjában álló globális elosztási és horizontális skálázhatósági képességeket.</span><span class="sxs-lookup"><span data-stu-id="9ee71-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="9ee71-106">A bevezető bemutatja, hogyan hozhat létre az Azure Portal segítségével Azure Cosmos DB-fiókot, adatbázist és gráfot (tárolót).</span><span class="sxs-lookup"><span data-stu-id="9ee71-106">This quick start demonstrates how to create an Azure Cosmos DB account, database, and graph (container) using the Azure portal.</span></span> <span data-ttu-id="9ee71-107">Ezután megtudhatja hogyan hozhat létre és futtathat egy a [Graph API](graph-sdk-dotnet.md) előzetes verziójával létrehozott konzolalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9ee71-107">You then build and run a console app built on the [Graph API](graph-sdk-dotnet.md) (preview).</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="9ee71-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9ee71-108">Prerequisites</span></span>

<span data-ttu-id="9ee71-109">Ha nincs telepítve a Visual Studio 2017, letöltheti és használhatja az **ingyenes** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)t.</span><span class="sxs-lookup"><span data-stu-id="9ee71-109">If you don’t already have Visual Studio 2017 installed, you can download and use the **free** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="9ee71-110">Ügyeljen arra, hogy engedélyezze az **Azure Development** használatát a Visual Studio telepítése során.</span><span class="sxs-lookup"><span data-stu-id="9ee71-110">Make sure that you enable **Azure development** during the Visual Studio setup.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="9ee71-111">Adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ee71-111">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a><span data-ttu-id="9ee71-112">Gráf hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9ee71-112">Add a graph</span></span>

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a name="clone-the-sample-application"></a><span data-ttu-id="9ee71-113">A mintaalkalmazás klónozása</span><span class="sxs-lookup"><span data-stu-id="9ee71-113">Clone the sample application</span></span>

<span data-ttu-id="9ee71-114">Most pedig klónozunk egy Graph API-alkalmazást a GitHubról, beállítjuk a kapcsolati karakterláncot, majd futtatni fogjuk az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9ee71-114">Now let's clone a Graph API app from github, set the connection string, and run it.</span></span> <span data-ttu-id="9ee71-115">Látni fogja, milyen egyszerű az adatokkal programozott módon dolgozni.</span><span class="sxs-lookup"><span data-stu-id="9ee71-115">You'll see how easy it is to work with data programmatically.</span></span> 

1. <span data-ttu-id="9ee71-116">Nyisson meg egy git terminálablakot, például a git bash eszközt, és a `cd` paranccsal lépjen egy munkakönyvtárba.</span><span class="sxs-lookup"><span data-stu-id="9ee71-116">Open a git terminal window, such as git bash, and `cd` to a working directory.</span></span>  

2. <span data-ttu-id="9ee71-117">Futtassa a következő parancsot a mintatárház klónozásához.</span><span class="sxs-lookup"><span data-stu-id="9ee71-117">Run the following command to clone the sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-dotnet-getting-started.git
    ```

3. <span data-ttu-id="9ee71-118">Ezután nyissa meg a Visual Studiót, majd a megoldásfájlt.</span><span class="sxs-lookup"><span data-stu-id="9ee71-118">Then open Visual Studio and open the solution file.</span></span> 

## <a name="review-the-code"></a><span data-ttu-id="9ee71-119">A kód áttekintése</span><span class="sxs-lookup"><span data-stu-id="9ee71-119">Review the code</span></span>

<span data-ttu-id="9ee71-120">Tekintsük át, hogy mi történik az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="9ee71-120">Let's make a quick review of what's happening in the app.</span></span> <span data-ttu-id="9ee71-121">Nyissa meg a Program.cs fájlt: az itt található kódsorok hozzák létre az Azure Cosmos DB erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="9ee71-121">Open the Program.cs file and you'll find that these lines of code create the Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="9ee71-122">A DocumentClient inicializálva van.</span><span class="sxs-lookup"><span data-stu-id="9ee71-122">The DocumentClient is initialized.</span></span> <span data-ttu-id="9ee71-123">Az előzetes verzióban hozzáadtunk egy gráfbővítmény API-t az Azure Cosmos DB-ügyfélhez.</span><span class="sxs-lookup"><span data-stu-id="9ee71-123">In the preview, we added a graph extension API on the Azure Cosmos DB client.</span></span> <span data-ttu-id="9ee71-124">Jelenleg egy különálló gráfügyfélen dolgozunk, amely az Azure Cosmos DB-ügyféltől és annak erőforrásaitól függetlenül működik.</span><span class="sxs-lookup"><span data-stu-id="9ee71-124">We are working on a standalone graph client decoupled from the Azure Cosmos DB client and resources.</span></span>

    ```csharp
    using (DocumentClient client = new DocumentClient(
        new Uri(endpoint),
        authKey,
        new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp }))
    ```

* <span data-ttu-id="9ee71-125">A rendszer létrehozza az új adatbázist.</span><span class="sxs-lookup"><span data-stu-id="9ee71-125">A new database is created.</span></span>

    ```csharp
    Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" });
    ```

* <span data-ttu-id="9ee71-126">A rendszer létrehozza az új gráfot.</span><span class="sxs-lookup"><span data-stu-id="9ee71-126">A new graph is created.</span></span>

    ```csharp
    DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync(
        UriFactory.CreateDatabaseUri("graphdb"),
        new DocumentCollection { Id = "graph" },
        new RequestOptions { OfferThroughput = 1000 });
    ```
* <span data-ttu-id="9ee71-127">A `CreateGremlinQuery` metódus használatával a program végrehajtja a Gremlin lépéssorozatot.</span><span class="sxs-lookup"><span data-stu-id="9ee71-127">A series of Gremlin steps are executed using the `CreateGremlinQuery` method.</span></span>

    ```csharp
    // The CreateGremlinQuery method extensions allow you to execute Gremlin queries and iterate
    // results asychronously
    IDocumentQuery<dynamic> query = client.CreateGremlinQuery<dynamic>(graph, "g.V().count()");
    while (query.HasMoreResults)
    {
        foreach (dynamic result in await query.ExecuteNextAsync())
        {
            Console.WriteLine($"\t {JsonConvert.SerializeObject(result)}");
        }
    }

    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="9ee71-128">A kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="9ee71-128">Update your connection string</span></span>

<span data-ttu-id="9ee71-129">Lépjen vissza az Azure Portalra a kapcsolati karakterlánc adataiért, majd másolja be azokat az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="9ee71-129">Now go back to the Azure portal to get your connection string information and copy it into the app.</span></span>

1. <span data-ttu-id="9ee71-130">Nyissa meg az App.config fájlt a Visual Studio 2017-ben.</span><span class="sxs-lookup"><span data-stu-id="9ee71-130">In Visual Studio 2017, open the App.config file.</span></span> 

2. <span data-ttu-id="9ee71-131">Az Azure Portalon az Azure Cosmos DB-fiók bal oldali navigációs sávján kattintson a **Kulcsok** elemre.</span><span class="sxs-lookup"><span data-stu-id="9ee71-131">In the Azure portal, in your Azure Cosmos DB account, click **Keys** in the left navigation.</span></span> 

    ![Elsődleges kulcs megtekintése és másolása az Azure Portal Kulcsok oldalán](./media/create-graph-dotnet/keys.png)

3. <span data-ttu-id="9ee71-133">Másolja az **URI** értéket a portálról, és adja meg az App.config fájl végpontkulcsának értékeként.</span><span class="sxs-lookup"><span data-stu-id="9ee71-133">Copy your **URI** value from the portal and make it the value of the Endpoint key in App.config.</span></span> <span data-ttu-id="9ee71-134">Az értéket az előző képernyőképen látható Másolás gombbal másolhatja.</span><span class="sxs-lookup"><span data-stu-id="9ee71-134">You can use the copy button as shown in the preceding screenshot to copy the value.</span></span>

    `<add key="Endpoint" value="https://FILLME.documents.azure.com:443" />`

4. <span data-ttu-id="9ee71-135">Másolja az **ELSŐDLEGES KULCS** értékét a portálról, és adja meg az App.config fájl AuthKey kulcsaként, majd mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="9ee71-135">Copy your **PRIMARY KEY** value from the portal, and make it the value of the AuthKey key in App.config, then save your changes.</span></span> 

    `<add key="AuthKey" value="FILLME" />`

<span data-ttu-id="9ee71-136">Az alkalmazás frissítve lett minden olyan információval, amely az Azure Cosmos DB-vel való kommunikációhoz szükséges.</span><span class="sxs-lookup"><span data-stu-id="9ee71-136">You've now updated your app with all the info it needs to communicate with Azure Cosmos DB.</span></span> 

## <a name="run-the-console-app"></a><span data-ttu-id="9ee71-137">A konzolalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="9ee71-137">Run the console app</span></span>

1. <span data-ttu-id="9ee71-138">A Visual Studióban kattintson a jobb gombbal a **GraphGetStarted** projektre a **Megoldáskezelőben**, majd kattintson a **NuGet-csomagok kezelése** elemre.</span><span class="sxs-lookup"><span data-stu-id="9ee71-138">In Visual Studio, right-click on the **GraphGetStarted** project in **Solution Explorer** and then click **Manage NuGet Packages**.</span></span> 

2. <span data-ttu-id="9ee71-139">A NuGet **Browse** (Tallózás) mezőjébe írja be *Microsoft.Azure.Graphs* kifejezést, és jelölje be az **Includes prerelease** (Előzetes verzió is) jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="9ee71-139">In the NuGet **Browse** box, type *Microsoft.Azure.Graphs* and check the **Includes prerelease** box.</span></span> 

3. <span data-ttu-id="9ee71-140">Az eredmények közül telepítse a **Microsoft.Azure.DocumentDB** kódtárat.</span><span class="sxs-lookup"><span data-stu-id="9ee71-140">From the results, install the **Microsoft.Azure.Graphs** library.</span></span> <span data-ttu-id="9ee71-141">Ezzel telepíti az Azure Cosmos DB gráfbővítmény kódtárcsomagja és annak összes függőségét.</span><span class="sxs-lookup"><span data-stu-id="9ee71-141">This installs the Azure Cosmos DB graph extension library package and all dependencies.</span></span>

    <span data-ttu-id="9ee71-142">Ha a megoldás módosításainak áttekintéséről szóló üzenetet kap, kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="9ee71-142">If you get a message about reviewing changes to the solution, click **OK**.</span></span> <span data-ttu-id="9ee71-143">Ha a licenc elfogadásáról szóló üzenetet kap, kattintson az **Elfogadom** gombra.</span><span class="sxs-lookup"><span data-stu-id="9ee71-143">If you get a message about license acceptance, click **I accept**.</span></span>

4. <span data-ttu-id="9ee71-144">Az alkalmazás futtatásához nyomja le a CTRL + F5 billentyűkombinációt.</span><span class="sxs-lookup"><span data-stu-id="9ee71-144">Click CTRL + F5 to run the application.</span></span>

   <span data-ttu-id="9ee71-145">A konzolablakban megjelennek a gráfhoz hozzáadandó csúcspontok és élek.</span><span class="sxs-lookup"><span data-stu-id="9ee71-145">The console window displays the vertexes and edges being added to the graph.</span></span> <span data-ttu-id="9ee71-146">Miután a parancsfájl futása befejeződött, nyomja meg kétszer az ENTER billentyűt a konzolablak bezárásához.</span><span class="sxs-lookup"><span data-stu-id="9ee71-146">When the script completes, press ENTER twice to close the console window.</span></span> 

## <a name="browse-using-the-data-explorer"></a><span data-ttu-id="9ee71-147">Tallózás az Adatkezelővel</span><span class="sxs-lookup"><span data-stu-id="9ee71-147">Browse using the Data Explorer</span></span>

<span data-ttu-id="9ee71-148">Ezután visszaléphet az Adatkezelőbe az Azure Portalon, ahol tallózhatja és lekérdezheti az új gráfadatokat.</span><span class="sxs-lookup"><span data-stu-id="9ee71-148">You can now go back to Data Explorer in the Azure portal and browse and query your new graph data.</span></span>

1. <span data-ttu-id="9ee71-149">Az Adatkezelőben az új adatbázis a Gráfok ablaktáblán jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="9ee71-149">In Data Explorer, the new database appears in the Graphs pane.</span></span> <span data-ttu-id="9ee71-150">Bontsa ki a **graphdb**, **graphcollz** pontokat, és kattintson a **Gráf** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="9ee71-150">Expand **graphdb**, **graphcollz**, and then click **Graph**.</span></span>

2. <span data-ttu-id="9ee71-151">Kattintson a **Szűrő alkalmazása** gombra a gráf összes csúcspontjának megtekintéséhez az alapértelmezett lekérdezéssel.</span><span class="sxs-lookup"><span data-stu-id="9ee71-151">Click the **Apply Filter** button to use the default query to view all the verticies in the graph.</span></span> <span data-ttu-id="9ee71-152">A mintaalkalmazás által létrehozott adatokat a Gráfok ablaktáblán találja.</span><span class="sxs-lookup"><span data-stu-id="9ee71-152">The data generated by the sample app is displayed in the Graphs pane.</span></span>

    <span data-ttu-id="9ee71-153">Szabadon nagyíthatja és kicsinyítheti a gráfot, kibonthatja a gráf megjelenítési területét, további csúcspontokat vehet fel, illetve áthelyezheti a csúcspontokat a megjelenítési felületen.</span><span class="sxs-lookup"><span data-stu-id="9ee71-153">You can zoom in and out of the graph, you can expand the graph display space, add additional verticies, and move verticies on the display surface.</span></span>

    ![A gráf megtekintése az Azure Portal Adatkezelőjében](./media/create-graph-dotnet/graph-explorer.png)

## <a name="review-slas-in-the-azure-portal"></a><span data-ttu-id="9ee71-155">Az SLA-k áttekintése az Azure Portalon</span><span class="sxs-lookup"><span data-stu-id="9ee71-155">Review SLAs in the Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="9ee71-156">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="9ee71-156">Clean up resources</span></span>

<span data-ttu-id="9ee71-157">Ha az alkalmazást már nem használja, akkor a következő lépésekkel a mintaalkalmazás által létrehozott összes erőforrást törölheti az Azure Portalon:</span><span class="sxs-lookup"><span data-stu-id="9ee71-157">If you're not going to continue to use this app, delete all resources created by this quickstart in the Azure portal with the following steps:</span></span> 

1. <span data-ttu-id="9ee71-158">Az Azure Portal bal oldali menüjében kattintson az **Erőforráscsoportok** lehetőségre, majd kattintson a létrehozott erőforrás nevére.</span><span class="sxs-lookup"><span data-stu-id="9ee71-158">From the left-hand menu in the Azure portal, click **Resource groups** and then click the name of the resource you created.</span></span> 
2. <span data-ttu-id="9ee71-159">Az erőforráscsoport lapján kattintson a **Törlés** elemre, írja be a törölni kívánt erőforrás nevét a szövegmezőbe, majd kattintson a **Törlés** gombra.</span><span class="sxs-lookup"><span data-stu-id="9ee71-159">On your resource group page, click **Delete**, type the name of the resource to delete in the text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9ee71-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9ee71-160">Next steps</span></span>

<span data-ttu-id="9ee71-161">Ebben a rövid útmutatóban bemutattuk, hogyan lehet Azure Cosmos DB-fiókot létrehozni, hogyan lehet az Adatkezelő segítségével gráfot készíteni, és hogyan lehet futtatni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9ee71-161">In this quickstart, you've learned how to create an Azure Cosmos DB account, create a graph using the Data Explorer, and run an app.</span></span> <span data-ttu-id="9ee71-162">Most már készen áll arra, hogy a Gremlin használatával összetettebb lekérdezéseket hozzon létre és hatékony gráfbejárási logikákat implementáljon.</span><span class="sxs-lookup"><span data-stu-id="9ee71-162">You can now build more complex queries and implement powerful graph traversal logic using Gremlin.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="9ee71-163">Lekérdezés a Gremlin használatával</span><span class="sxs-lookup"><span data-stu-id="9ee71-163">Query using Gremlin</span></span>](tutorial-query-graph.md)

