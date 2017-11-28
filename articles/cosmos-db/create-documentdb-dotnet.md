---
title: "Azure Cosmos DB: A .NET webalkalmazás létrehozása és a DocumentDB API hello |} Microsoft Docs"
description: "Megadja a .NET kódminta, használhatja a tooconnect tooand lekérdezés hello Azure Cosmos DB DocumentDB API"
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
ms.openlocfilehash: 35517e35d80c48662a51a99814652ffa1121fc5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-documentdb-api-web-app-with-net-and-hello-azure-portal"></a><span data-ttu-id="55ae2-103">Azure Cosmos DB: Hozza létre a DocumentDB API webes alkalmazás a .NET, és hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="55ae2-103">Azure Cosmos DB: Build a DocumentDB API web app with .NET and hello Azure portal</span></span>

<span data-ttu-id="55ae2-104">Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="55ae2-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="55ae2-105">Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="55ae2-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="55ae2-106">A gyors üzembe helyezési bemutatja, hogyan toocreate Azure Cosmos DB fiókkal, a dokumentum-adatbázis és gyűjtemény használja hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="55ae2-106">This quick start demonstrates how toocreate an Azure Cosmos DB account, document database, and collection using hello Azure portal.</span></span> <span data-ttu-id="55ae2-107">Kell majd létrehozása és központi telepítése egy hello épülő teendőlistát megvalósító webes alkalmazás [DocumentDB .NET API](documentdb-sdk-dotnet.md), ahogy az alábbi képernyőfelvétel a hello.</span><span class="sxs-lookup"><span data-stu-id="55ae2-107">You'll then build and deploy a todo list web app built on hello [DocumentDB .NET API](documentdb-sdk-dotnet.md), as shown in hello following screenshot.</span></span> 

![Teendőkezelő alkalmazás mintaadatokkal](./media/create-documentdb-dotnet/azure-comosdb-todo-app-list.png)

## <a name="prerequisites"></a><span data-ttu-id="55ae2-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="55ae2-109">Prerequisites</span></span>

<span data-ttu-id="55ae2-110">Ha még nincs telepítve a Visual Studio 2017, töltse le és használja a hello **szabad** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="55ae2-110">If you don’t already have Visual Studio 2017 installed, you can download and use hello **free** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="55ae2-111">Győződjön meg arról, hogy engedélyezze **Azure fejlesztési** hello Visual Studio telepítése során.</span><span class="sxs-lookup"><span data-stu-id="55ae2-111">Make sure that you enable **Azure development** during hello Visual Studio setup.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-account"></a>
## <a name="create-a-database-account"></a><span data-ttu-id="55ae2-112">Adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="55ae2-112">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<a id="create-collection"></a>
## <a name="add-a-collection"></a><span data-ttu-id="55ae2-113">Gyűjtemény hozzáadása</span><span class="sxs-lookup"><span data-stu-id="55ae2-113">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a><span data-ttu-id="55ae2-114">Mintaadatok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="55ae2-114">Add sample data</span></span>

<span data-ttu-id="55ae2-115">Most tooyour új adatgyűjtés adatkezelő használatával adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="55ae2-115">You can now add data tooyour new collection using Data Explorer.</span></span>

1. <span data-ttu-id="55ae2-116">Új adatbázis hello Data Explorer hello Gyűjtemények ablaktáblán jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="55ae2-116">In Data Explorer, hello new database appears in hello Collections pane.</span></span> <span data-ttu-id="55ae2-117">Bontsa ki a hello **feladatok** adatbázisra, és bontsa ki a hello **elemek** gyűjtemény, kattintson a **dokumentumok**, és kattintson a **új dokumentumok**.</span><span class="sxs-lookup"><span data-stu-id="55ae2-117">Expand hello **Tasks** database, expand hello **Items** collection, click **Documents**, and then click **New Documents**.</span></span> 

   ![Hozzon létre új dokumentumok az adatkezelő a hello Azure-portálon](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
2. <span data-ttu-id="55ae2-119">A struktúra a következő hello mostantól hozzáadhatja azok toohello dokumentumgyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="55ae2-119">Now add a document toohello collection with hello following structure.</span></span>

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

3. <span data-ttu-id="55ae2-120">Hello json toohello hozzáadása után **dokumentumok** lapra, majd **mentése**.</span><span class="sxs-lookup"><span data-stu-id="55ae2-120">Once you've added hello json toohello **Documents** tab, click **Save**.</span></span>

    ![Másolja át json-adatokat, és kattintson a Mentés gombra az adatok Explorer hello Azure-portálon](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-save-document.png)

4.  <span data-ttu-id="55ae2-122">Hozzon létre és mentsen egy egyedi értéket hello helyezze egy további dokumentum `id` tulajdonság, és a változás hello más tulajdonságok, ahogyan szeretné.</span><span class="sxs-lookup"><span data-stu-id="55ae2-122">Create and save one more document where you insert a unique value for hello `id` property, and change hello other properties as you see fit.</span></span> <span data-ttu-id="55ae2-123">Mivel az Azure Cosmos DB nem kötelezi egy adott adatséma használatára, új dokumentumaihoz bármilyen struktúrát választhat.</span><span class="sxs-lookup"><span data-stu-id="55ae2-123">Your new documents can have any structure you want as Azure Cosmos DB doesn't impose any schema on your data.</span></span>

     <span data-ttu-id="55ae2-124">Használhatja az adatkezelő tooretrieve lekérdezések adatait.</span><span class="sxs-lookup"><span data-stu-id="55ae2-124">You can now use queries in Data Explorer tooretrieve your data.</span></span> <span data-ttu-id="55ae2-125">Alapértelmezés szerint adatkezelő használja `SELECT * FROM c` összes hello gyűjtemény, de a dokumentumok tooretrieve módosíthatja, hogy különböző tooa [SQL-lekérdezés](documentdb-sql-query.md), például a `SELECT * FROM c ORDER BY c._ts DESC`, csökkenő sorrendben minden hello dokumentum alapján tooreturn az időbélyegző.</span><span class="sxs-lookup"><span data-stu-id="55ae2-125">By default, Data Explorer uses `SELECT * FROM c` tooretrieve all documents in hello collection, but you can change that tooa different [SQL query](documentdb-sql-query.md), such as `SELECT * FROM c ORDER BY c._ts DESC`, tooreturn all hello documents in descending order based on their timestamp.</span></span>
 
     <span data-ttu-id="55ae2-126">Is használhatja adatkezelő toocreate tárolt eljárások, felhasználó által megadott függvények és eseményindítók tooperform kiszolgálóoldali üzleti logikát is mint méretezési átviteli sebesség.</span><span class="sxs-lookup"><span data-stu-id="55ae2-126">You can also use Data Explorer toocreate stored procedures, UDFs, and triggers tooperform server-side business logic as well as scale throughput.</span></span> <span data-ttu-id="55ae2-127">Adatkezelő mutatja meg az összes hello beépített programozott adatelérési hello API-k érhető el, de hello Azure-portálon található egyszerű a hozzáférés tooyour adatokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="55ae2-127">Data Explorer exposes all of hello built-in programmatic data access available in hello APIs, but provides easy access tooyour data in hello Azure portal.</span></span>

## <a name="clone-hello-sample-application"></a><span data-ttu-id="55ae2-128">Klónozza a mintaalkalmazást hello</span><span class="sxs-lookup"><span data-stu-id="55ae2-128">Clone hello sample application</span></span>

<span data-ttu-id="55ae2-129">Most tegyük kapcsoló tooworking kóddal.</span><span class="sxs-lookup"><span data-stu-id="55ae2-129">Now let's switch tooworking with code.</span></span> <span data-ttu-id="55ae2-130">Most klónozza a Githubból, állítsa be a hello kapcsolati karakterláncot, és futtassa azt a DocumentDB API-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="55ae2-130">Let's clone a DocumentDB API app from GitHub, set hello connection string, and run it.</span></span> <span data-ttu-id="55ae2-131">Láthatja, milyen egyszerűen adatokkal toowork programozott módon.</span><span class="sxs-lookup"><span data-stu-id="55ae2-131">You'll see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="55ae2-132">Nyisson meg egy git terminálablakot, például a git bash eszközt, és `CD` tooa munkakönyvtárát.</span><span class="sxs-lookup"><span data-stu-id="55ae2-132">Open a git terminal window, such as git bash, and `CD` tooa working directory.</span></span>  

2. <span data-ttu-id="55ae2-133">Futtassa a következő parancs tooclone hello minta tárház hello.</span><span class="sxs-lookup"><span data-stu-id="55ae2-133">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/documentdb-dotnet-todo-app.git
    ```

3. <span data-ttu-id="55ae2-134">Ezután nyissa meg az hello todo megoldásfájlt a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="55ae2-134">Then open hello todo solution file in Visual Studio.</span></span> 

## <a name="review-hello-code"></a><span data-ttu-id="55ae2-135">Tekintse át a hello kódot</span><span class="sxs-lookup"><span data-stu-id="55ae2-135">Review hello code</span></span>

<span data-ttu-id="55ae2-136">Most Meggyőződünk arról, mi történik a hello app gyors áttekintése.</span><span class="sxs-lookup"><span data-stu-id="55ae2-136">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="55ae2-137">Nyissa meg hello DocumentDBRepository.cs fájlt, és, hogy ezek a sorok, a kód létrehozása hello Azure Cosmos DB erőforrások találhat.</span><span class="sxs-lookup"><span data-stu-id="55ae2-137">Open hello DocumentDBRepository.cs file and you'll find that these lines of code create hello Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="55ae2-138">hello DocumentClient sor 73 inicializálva van.</span><span class="sxs-lookup"><span data-stu-id="55ae2-138">hello DocumentClient is initialized on line 73.</span></span>

    ```csharp
    client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);`
    ```

* <span data-ttu-id="55ae2-139">A rendszer létrehozza az új adatbázist a 88. sorban.</span><span class="sxs-lookup"><span data-stu-id="55ae2-139">A new database is created on line 88.</span></span>

    ```csharp
    await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
    ```

* <span data-ttu-id="55ae2-140">A rendszer létrehozza az új gyűjteményt a 107. sorban.</span><span class="sxs-lookup"><span data-stu-id="55ae2-140">A new collection is created on line 107.</span></span>

    ```csharp
    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(DatabaseId),
        new DocumentCollection { Id = CollectionId },
        new RequestOptions { OfferThroughput = 1000 });
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="55ae2-141">A kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="55ae2-141">Update your connection string</span></span>

<span data-ttu-id="55ae2-142">Most lépjen vissza az Azure portál tooget toohello kapcsolati karakterlánc adatainak és hello alkalmazásba másolja.</span><span class="sxs-lookup"><span data-stu-id="55ae2-142">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span>

1. <span data-ttu-id="55ae2-143">A hello [Azure-portálon](http://portal.azure.com/), az Azure Cosmos DB a fiókot, kattintson a bal oldali navigációs hello **kulcsok**, és kattintson a **írható-olvasható kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="55ae2-143">In hello [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in hello left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="55ae2-144">Hello másolási gombok hello jobb oldalán hello képernyő toocopy hello URI és elsődleges kulcs hello web.config fájlba hello következő lépésben fogja használni.</span><span class="sxs-lookup"><span data-stu-id="55ae2-144">You'll use hello copy buttons on hello right side of hello screen toocopy hello URI and Primary Key into hello web.config file in hello next step.</span></span>

    ![Megtekintése és másolása egy hozzáférési kulcsot a hello Azure-portálon, a kulcsok panelen](./media/create-documentdb-dotnet/keys.png)

2. <span data-ttu-id="55ae2-146">Nyissa meg a Visual Studio 2017 hello web.config fájlt.</span><span class="sxs-lookup"><span data-stu-id="55ae2-146">In Visual Studio 2017, open hello web.config file.</span></span> 

3. <span data-ttu-id="55ae2-147">Az URI értéket másol a portálról hello (hello Másolás gombra), és teszi hello a Web.config fájlban hello végpont kulcs értékét.</span><span class="sxs-lookup"><span data-stu-id="55ae2-147">Copy your URI value from hello portal (using hello copy button) and make it hello value of hello endpoint key in web.config.</span></span> 

    `<add key="endpoint" value="FILLME" />`

4. <span data-ttu-id="55ae2-148">Ezután másolja az elsődleges kulcs-érték hello portálról, és teszi hello értékének hello authKey a Web.config fájlban. Most már frissítette az alkalmazást az Azure Cosmos DB toocommunicate szükséges összes hello információval.</span><span class="sxs-lookup"><span data-stu-id="55ae2-148">Then copy your PRIMARY KEY value from hello portal and make it hello value of hello authKey in web.config. You've now updated your app with all hello info it needs toocommunicate with Azure Cosmos DB.</span></span> 

    `<add key="authKey" value="FILLME" />`
    
## <a name="run-hello-web-app"></a><span data-ttu-id="55ae2-149">Hello webes alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="55ae2-149">Run hello web app</span></span>
1. <span data-ttu-id="55ae2-150">A Visual Studióban, kattintson a jobb gombbal a hello projekt **Megoldáskezelőben** majd **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="55ae2-150">In Visual Studio, right-click on hello project in **Solution Explorer** and then click **Manage NuGet Packages**.</span></span> 

2. <span data-ttu-id="55ae2-151">A hello NuGet **Tallózás** mezőbe írja be *DocumentDB*.</span><span class="sxs-lookup"><span data-stu-id="55ae2-151">In hello NuGet **Browse** box, type *DocumentDB*.</span></span>

3. <span data-ttu-id="55ae2-152">Hello eredmények közül telepítse a hello **Microsoft.Azure.DocumentDB** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="55ae2-152">From hello results, install hello **Microsoft.Azure.DocumentDB** library.</span></span> <span data-ttu-id="55ae2-153">Ez telepíti a hello Microsoft.Azure.DocumentDB csomagot, valamint az összes függősége.</span><span class="sxs-lookup"><span data-stu-id="55ae2-153">This installs hello Microsoft.Azure.DocumentDB package as well as all dependencies.</span></span>

4. <span data-ttu-id="55ae2-154">Kattintson a CTRL + F5 toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="55ae2-154">Click CTRL + F5 toorun hello application.</span></span> <span data-ttu-id="55ae2-155">Az alkalmazás megjelenik a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="55ae2-155">Your app displays in your browser.</span></span> 

5. <span data-ttu-id="55ae2-156">Kattintson a **hozzon létre új** a böngésző hello és néhány új feladatot létrehozni az tennivaló alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="55ae2-156">Click **Create New** in hello browser and create a few new tasks in your to-do app.</span></span>

   ![Teendőkezelő alkalmazás mintaadatokkal](./media/create-documentdb-dotnet/azure-comosdb-todo-app-list.png)

<span data-ttu-id="55ae2-158">Mostantól tooData Explorer visszaléphet, és tekintse meg a lekérdezés, módosítása, és ezekkel az új adatokkal.</span><span class="sxs-lookup"><span data-stu-id="55ae2-158">You can now go back tooData Explorer and see query, modify, and work with this new data.</span></span> 

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="55ae2-159">Tekintse át a szolgáltatásiszint-szerződések a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="55ae2-159">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="55ae2-160">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="55ae2-160">Clean up resources</span></span>

<span data-ttu-id="55ae2-161">Toocontinue toouse az alkalmazás nem fog, ha törli az összes erőforrást hozta létre a gyors üzembe helyezés hello az Azure-portálon az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="55ae2-161">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="55ae2-162">A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson a létrehozott hello erőforrás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="55ae2-162">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="55ae2-163">Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="55ae2-163">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="55ae2-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="55ae2-164">Next steps</span></span>

<span data-ttu-id="55ae2-165">A gyors üzembe helyezés hogy megtanulta, hogyan toocreate Azure Cosmos DB adatait, hozzon létre egy gyűjteményt hello adatkezelő használatával, és futtassa a webes alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="55ae2-165">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, create a collection using hello Data Explorer, and run a web app.</span></span> <span data-ttu-id="55ae2-166">További adatok tooyour Cosmos DB fiókot most importálhatja.</span><span class="sxs-lookup"><span data-stu-id="55ae2-166">You can now import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="55ae2-167">Adatok importálása az Azure Cosmos DB-be</span><span class="sxs-lookup"><span data-stu-id="55ae2-167">Import data into Azure Cosmos DB</span></span>](import-data.md)


