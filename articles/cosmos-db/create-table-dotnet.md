---
title: "egy Azure Cosmos DB .NET alkalmazást, amely aaaBuild hello tábla API |} Microsoft Docs"
description: "Bevezetés az Azure Cosmos DB Table API-jának .NET-alapú használatába"
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 66327041-4d5e-4ce6-a394-fee107c18e59
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/22/2017
ms.author: arramac
ms.openlocfilehash: bdd4f8ec45407962b3d2cb26aa814a20cfc62173
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-net-application-using-hello-table-api"></a><span data-ttu-id="e5723-103">Az Azure Cosmos DB: Hello tábla API használatával .NET-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e5723-103">Azure Cosmos DB: Build a .NET application using hello Table API</span></span>

<span data-ttu-id="e5723-104">Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="e5723-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="e5723-105">Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="e5723-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="e5723-106">A gyors üzembe helyezési bemutatja, hogyan toocreate egy Azure Cosmos DB fiókot, és hozzon létre egy táblát a fiókban hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="e5723-106">This quick start demonstrates how toocreate an Azure Cosmos DB account, and create a table within that account using hello Azure portal.</span></span> <span data-ttu-id="e5723-107">Akkor lesz majd írási kód tooinsert, update és delete entitásokat és egyes lekérdezések futtatása használatával hello új [Windows Azure Storage prémium tábla](https://aka.ms/premiumtablenuget) NuGet-csomagot (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="e5723-107">You'll then write code tooinsert, update, and delete entities, and run some queries using hello new [Windows Azure Storage Premium Table](https://aka.ms/premiumtablenuget) (preview) package from NuGet.</span></span> <span data-ttu-id="e5723-108">Ez a függvénytár hello azonos osztályok és hello nyilvános metódus-aláírása [Windows Azure Storage szolgáltatás SDK](https://www.nuget.org/packages/WindowsAzure.Storage), de hello képességét tooconnect tooAzure Cosmos DB fiókok használatával hello [tábla API](table-introduction.md) (előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="e5723-108">This library has hello same classes and method signatures as hello public [Windows Azure Storage SDK](https://www.nuget.org/packages/WindowsAzure.Storage), but also has hello ability tooconnect tooAzure Cosmos DB accounts using hello [Table API](table-introduction.md) (preview).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="e5723-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e5723-109">Prerequisites</span></span>

<span data-ttu-id="e5723-110">Ha még nincs telepítve a Visual Studio 2017, töltse le és használja a hello **szabad** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="e5723-110">If you don’t already have Visual Studio 2017 installed, you can download and use hello **free** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="e5723-111">Győződjön meg arról, hogy engedélyezze **Azure fejlesztési** hello Visual Studio telepítése során.</span><span class="sxs-lookup"><span data-stu-id="e5723-111">Make sure that you enable **Azure development** during hello Visual Studio setup.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="e5723-112">Adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="e5723-112">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="add-a-table"></a><span data-ttu-id="e5723-113">Tábla hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e5723-113">Add a table</span></span>

[!INCLUDE [cosmos-db-create-table](../../includes/cosmos-db-create-table.md)]

## <a name="add-sample-data"></a><span data-ttu-id="e5723-114">Mintaadatok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e5723-114">Add sample data</span></span>

<span data-ttu-id="e5723-115">Most tooyour új adattábla adatok kezelővel (előzetes verzió) használatával adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="e5723-115">You can now add data tooyour new table using Data Explorer (Preview).</span></span>

1. <span data-ttu-id="e5723-116">Az Adatkezelőben bontsa ki a **minta tábla** pontot, és kattintson az **Entitások**, ezután pedig az **Entitás hozzáadása** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="e5723-116">In Data Explorer, expand **sample-table**, click **Entities**, and then click **Add Entity**.</span></span>

   ![Hozzon létre új entitások az adatkezelő a hello Azure-portálon](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-document.png)
2. <span data-ttu-id="e5723-118">Most adatok toohello PartitionKey érték és RowKey érték jelölését, és kattintson a **entitás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="e5723-118">Now add data toohello PartitionKey value box and RowKey value box, and click **Add Entity**.</span></span>

   ![Állítsa be partíciós kulcs és egy új entitás Sorkulcsa hello](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-entity.png)
  
    <span data-ttu-id="e5723-120">Most több entitások tooyour tábla hozzáadásához, szerkesztéséhez az entitások vagy adatkezelő az adatok lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="e5723-120">You can now add more entities tooyour table, edit your entities, or query your data in Data Explorer.</span></span> <span data-ttu-id="e5723-121">Adatkezelő akkor is, ahol az átviteli sebesség méretezhető és tárolt eljárások, felhasználó által megadott függvények és eseményindítók tooyour tábla.</span><span class="sxs-lookup"><span data-stu-id="e5723-121">Data Explorer is also where you can scale your throughput and add stored procedures, user defined functions, and triggers tooyour table.</span></span>

## <a name="clone-hello-sample-application"></a><span data-ttu-id="e5723-122">Klónozza a mintaalkalmazást hello</span><span class="sxs-lookup"><span data-stu-id="e5723-122">Clone hello sample application</span></span>

<span data-ttu-id="e5723-123">Most tegyük klónozza a githubból, állítsa be a hello kapcsolati karakterláncot, és futtassa azt egy tábla alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e5723-123">Now let's clone a Table app from github, set hello connection string, and run it.</span></span> <span data-ttu-id="e5723-124">Láthatja, milyen egyszerűen adatokkal toowork programozott módon.</span><span class="sxs-lookup"><span data-stu-id="e5723-124">You'll see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="e5723-125">Nyisson meg egy git terminálablakot, például a git bash eszközt, és `cd` tooa munkakönyvtárát.</span><span class="sxs-lookup"><span data-stu-id="e5723-125">Open a git terminal window, such as git bash, and `cd` tooa working directory.</span></span>  

2. <span data-ttu-id="e5723-126">Futtassa a következő parancs tooclone hello minta tárház hello.</span><span class="sxs-lookup"><span data-stu-id="e5723-126">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-getting-started.git
    ```

3. <span data-ttu-id="e5723-127">Ezután nyissa meg a hello megoldásfájlt a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e5723-127">Then open hello solution file in Visual Studio.</span></span> 

## <a name="review-hello-code"></a><span data-ttu-id="e5723-128">Tekintse át a hello kódot</span><span class="sxs-lookup"><span data-stu-id="e5723-128">Review hello code</span></span>

<span data-ttu-id="e5723-129">Most Meggyőződünk arról, mi történik a hello app gyors áttekintése.</span><span class="sxs-lookup"><span data-stu-id="e5723-129">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="e5723-130">Nyissa meg hello Program.cs fájlt, és, hogy ezek a sorok, a kód létrehozása hello Azure Cosmos DB erőforrások találhat.</span><span class="sxs-lookup"><span data-stu-id="e5723-130">Open hello Program.cs file and you'll find that these lines of code create hello Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="e5723-131">hello CloudTableClient inicializálva van.</span><span class="sxs-lookup"><span data-stu-id="e5723-131">hello CloudTableClient is initialized.</span></span>

    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); 
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

* <span data-ttu-id="e5723-132">Új táblázat jön létre, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="e5723-132">A new table is created if it does not exist.</span></span>

    ```csharp
    CloudTable table = tableClient.GetTableReference("people");
    table.CreateIfNotExists();
    ```

* <span data-ttu-id="e5723-133">Létrejön egy új táblázattároló.</span><span class="sxs-lookup"><span data-stu-id="e5723-133">A new Table container is created.</span></span> <span data-ttu-id="e5723-134">Megfigyelheti, hogy a kód nagyon hasonló tooregular Azure Table storage SDK.</span><span class="sxs-lookup"><span data-stu-id="e5723-134">You will notice this code very similar tooregular Azure Table storage SDK.</span></span> 

    ```csharp
    CustomerEntity item = new CustomerEntity()
                {
                    PartitionKey = Guid.NewGuid().ToString(),
                    RowKey = Guid.NewGuid().ToString(),
                    Email = $"{GetRandomString(6)}@contoso.com",
                    PhoneNumber = "425-555-0102",
                    Bio = GetRandomString(1000)
                };
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="e5723-135">A kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="e5723-135">Update your connection string</span></span>

<span data-ttu-id="e5723-136">Most azt frissítése hello kapcsolati karakterlánc adatokat, az alkalmazás tooAzure Cosmos DB működik.</span><span class="sxs-lookup"><span data-stu-id="e5723-136">Now we'll update hello connection string information so your app can talk tooAzure Cosmos DB.</span></span> 

1. <span data-ttu-id="e5723-137">A Visual Studióban nyissa meg a hello app.config fájlt.</span><span class="sxs-lookup"><span data-stu-id="e5723-137">In Visual Studio, open hello app.config file.</span></span> 

2. <span data-ttu-id="e5723-138">A hello [Azure-portálon](http://portal.azure.com/), hello Azure Cosmos DB bal oldali navigációs menü, kattintson a **kapcsolati karakterlánc**.</span><span class="sxs-lookup"><span data-stu-id="e5723-138">In hello [Azure portal](http://portal.azure.com/), in hello Azure Cosmos DB left navigation menu, click **Connection String**.</span></span> <span data-ttu-id="e5723-139">Hello új panelen kattintson a Másolás gombra hello hello kapcsolati karakterláncot kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="e5723-139">Then in hello new pane click hello copy button for hello connection string.</span></span> 

    ![Megtekintése és másolása hello végpont és a Fiókkulcsot hello kapcsolati karakterlánc panelen](./media/create-table-dotnet/keys.png)

3. <span data-ttu-id="e5723-141">Hello érték beillesztése hello app.config fájl hello PremiumStorageConnectionString hello értékeként.</span><span class="sxs-lookup"><span data-stu-id="e5723-141">Paste hello value into hello app.config file as hello value of hello PremiumStorageConnectionString.</span></span> 

    `<add key="PremiumStorageConnectionString" 
        value="DefaultEndpointsProtocol=https;AccountName=MYSTORAGEACCOUNT;AccountKey=AUTHKEY;TableEndpoint=https://COSMOSDB.documents.azure.com" />`    

    <span data-ttu-id="e5723-142">Hello StandardStorageConnectionString, mert a hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="e5723-142">You can leave hello StandardStorageConnectionString as is.</span></span>

<span data-ttu-id="e5723-143">Most már frissítette az alkalmazást az Azure Cosmos DB toocommunicate szükséges összes hello információval.</span><span class="sxs-lookup"><span data-stu-id="e5723-143">You've now updated your app with all hello info it needs toocommunicate with Azure Cosmos DB.</span></span> 

## <a name="run-hello-web-app"></a><span data-ttu-id="e5723-144">Hello webes alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="e5723-144">Run hello web app</span></span>

1. <span data-ttu-id="e5723-145">A Visual Studióban, kattintson a jobb gombbal a hello **PremiumTableGetStarted** a projekt **Megoldáskezelőben** majd **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="e5723-145">In Visual Studio, right-click on hello **PremiumTableGetStarted** project in **Solution Explorer** and then click **Manage NuGet Packages**.</span></span> 

2. <span data-ttu-id="e5723-146">A hello NuGet **Tallózás** mezőbe írja be *windowsazure.Storage kifejezésre-PremiumTable*.</span><span class="sxs-lookup"><span data-stu-id="e5723-146">In hello NuGet **Browse** box, type *WindowsAzure.Storage-PremiumTable*.</span></span>

3. <span data-ttu-id="e5723-147">Ellenőrizze a hello **közé tartoznak az előzetes** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="e5723-147">Check hello **Include prerelease** box.</span></span> 

4. <span data-ttu-id="e5723-148">Hello eredmények közül telepítse a hello **windowsazure.Storage kifejezésre-PremiumTable** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="e5723-148">From hello results, install hello **WindowsAzure.Storage-PremiumTable** library.</span></span> <span data-ttu-id="e5723-149">Ez telepíti a hello preview Azure Cosmos DB tábla API csomag, valamint az összes függősége.</span><span class="sxs-lookup"><span data-stu-id="e5723-149">This installs hello preview Azure Cosmos DB Table API package as well as all dependencies.</span></span> <span data-ttu-id="e5723-150">Vegye figyelembe, hogy a csomag egy másik NuGet hello Windows Azure Storage csomag használják az Azure Table storage-nál.</span><span class="sxs-lookup"><span data-stu-id="e5723-150">Note that this is a different NuGet package than hello Windows Azure Storage package used by Azure Table storage.</span></span> 

5. <span data-ttu-id="e5723-151">Kattintson a CTRL + F5 toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e5723-151">Click CTRL + F5 toorun hello application.</span></span>

    <span data-ttu-id="e5723-152">hello konzolablak hozzáadott, lekérését, lekérdezése, cseréje és hello táblából törölt hello adatokat jeleníthet meg.</span><span class="sxs-lookup"><span data-stu-id="e5723-152">hello console window displays hello data being added, retrieved, queried, replaced and deleted from hello table.</span></span> <span data-ttu-id="e5723-153">Hello parancsfájl befejezése után nyomja le az bármely főbb tooclose hello console ablakban.</span><span class="sxs-lookup"><span data-stu-id="e5723-153">When hello script completes, press any key tooclose hello console window.</span></span> 
    
    ![Hello gyors üzembe helyezés a konzol kimeneti](./media/create-table-dotnet/azure-cosmosdb-table-quickstart-console-output.png)

6. <span data-ttu-id="e5723-155">Ha azt szeretné, hogy toosee hello új entitások Data Explorer, a program.cs fájlban, ezért azok nem törlődnek, 188-208 sorok csak megjegyzésbe futtassa újból hello minta.</span><span class="sxs-lookup"><span data-stu-id="e5723-155">If you want toosee hello new entities in Data Explorer, just comment out lines 188-208 in program.cs so they aren't deleted, then run hello sample again.</span></span> 

    <span data-ttu-id="e5723-156">Most lépjen vissza tooData Explorerben (Megoldáskezelőben) kattintson **frissítése**, bontsa ki a hello **személyek** táblázatban, majd kattintson **entitások**, és majd ezekkel az új adatokkal.</span><span class="sxs-lookup"><span data-stu-id="e5723-156">You can now go back tooData Explorer, click **Refresh**, expand hello **people** table and click **Entities**, and then work with this new data.</span></span> 

    ![Új entitások az Adatkezelőben](./media/create-table-dotnet/azure-cosmosdb-table-quickstart-data-explorer.png)

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="e5723-158">Tekintse át a szolgáltatásiszint-szerződések a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e5723-158">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="e5723-159">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="e5723-159">Clean up resources</span></span>

<span data-ttu-id="e5723-160">Toocontinue toouse az alkalmazás nem fog, ha törli az összes erőforrást hozta létre a gyors üzembe helyezés hello az Azure-portálon az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e5723-160">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span> 

1. <span data-ttu-id="e5723-161">A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson a létrehozott hello erőforrás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="e5723-161">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="e5723-162">Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="e5723-162">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e5723-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e5723-163">Next steps</span></span>

<span data-ttu-id="e5723-164">A gyors üzembe helyezés hogy megtanulta, hogyan toocreate Azure Cosmos DB adatait, hozzon létre egy táblát hello adatkezelő használatával, és futtassa az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e5723-164">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, create a table using hello Data Explorer, and run an app.</span></span>  <span data-ttu-id="e5723-165">Most már tudja kérdezni hello tábla API használata esetén az adatok.</span><span class="sxs-lookup"><span data-stu-id="e5723-165">Now you can query your data using hello Table API.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="e5723-166">A lekérdezés hello tábla API használatával</span><span class="sxs-lookup"><span data-stu-id="e5723-166">Query using hello Table API</span></span>](tutorial-query-table.md)

