---
title: "Azure Cosmos DB: Webalkalmazás létrehozása .NET-tel és MongoDB API-val | Microsoft Docs"
description: "Egy .NET-es kódmintát mutat be, amellyel csatlakozni lehet az Azure Cosmos DB MongoDB API-hoz, és lekérdezést lehet végezni vele"
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
ms.openlocfilehash: 2d30bec75d701b1fd55355d1e139350b6d828c9a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-build-a-mongodb-api-web-app-with-net-and-the-azure-portal"></a><span data-ttu-id="35482-103">Azure Cosmos DB: MongoDB API Webalkalmazás létrehozása .NET-tel és az Azure Portallal</span><span class="sxs-lookup"><span data-stu-id="35482-103">Azure Cosmos DB: Build a MongoDB API web app with .NET and the Azure portal</span></span>

<span data-ttu-id="35482-104">Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="35482-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="35482-105">Segítségével gyorsan létrehozhat és lekérdezhet dokumentum-, kulcs/érték és gráf típusú adatbázisokat, melyek mindegyike felhasználja az Azure Cosmos DB középpontjában álló globális elosztási és horizontális skálázhatósági képességeket.</span><span class="sxs-lookup"><span data-stu-id="35482-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="35482-106">A rövid útmutató bemutatja, hogyan hozhat létre az Azure Portal segítségével Azure Cosmos DB-fiókot, dokumentum-adatbázist, és gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="35482-106">This quick start demonstrates how to create an Azure Cosmos DB account, document database, and collection using the Azure portal.</span></span> <span data-ttu-id="35482-107">Ezután létrehozhatja és üzembe helyezheti a [MongoDB .NET driver-re](https://docs.mongodb.com/ecosystem/drivers/csharp/) épülő feladatlista webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="35482-107">You'll then build and deploy a tasks list web app built on the [MongoDB .NET driver](https://docs.mongodb.com/ecosystem/drivers/csharp/).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="35482-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="35482-108">Prerequisites</span></span>

<span data-ttu-id="35482-109">Ha nincs telepítve a Visual Studio 2017, letöltheti és használhatja az **ingyenes** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)t.</span><span class="sxs-lookup"><span data-stu-id="35482-109">If you don’t already have Visual Studio 2017 installed, you can download and use the **free** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="35482-110">Ügyeljen arra, hogy engedélyezze az **Azure Development** használatát a Visual Studio telepítése során.</span><span class="sxs-lookup"><span data-stu-id="35482-110">Make sure that you enable **Azure development** during the Visual Studio setup.</span></span>
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-account"></a>
## <a name="create-a-database-account"></a><span data-ttu-id="35482-111">Adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="35482-111">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="clone-the-sample-application"></a><span data-ttu-id="35482-112">A mintaalkalmazás klónozása</span><span class="sxs-lookup"><span data-stu-id="35482-112">Clone the sample application</span></span>

<span data-ttu-id="35482-113">Klónozzunk egy MongoDB API-alkalmazást a GitHubról, állítsuk be a kapcsolati karakterláncot, és futtassuk.</span><span class="sxs-lookup"><span data-stu-id="35482-113">Now let's clone a MongoDB API app from github, set the connection string, and run it.</span></span> <span data-ttu-id="35482-114">Látni fogja, milyen egyszerű az adatokkal programozott módon dolgozni.</span><span class="sxs-lookup"><span data-stu-id="35482-114">You'll see how easy it is to work with data programmatically.</span></span> 

1. <span data-ttu-id="35482-115">Nyisson meg egy git terminálablakot, például a git bash eszközt, és a `cd` paranccsal lépjen egy munkakönyvtárba.</span><span class="sxs-lookup"><span data-stu-id="35482-115">Open a git terminal window, such as git bash, and `cd` to a working directory.</span></span>  

2. <span data-ttu-id="35482-116">Futtassa a következő parancsot a mintatárház klónozásához.</span><span class="sxs-lookup"><span data-stu-id="35482-116">Run the following command to clone the sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-getting-started.git
    ```

3. <span data-ttu-id="35482-117">Ezután nyissa meg a megoldásfájlt a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="35482-117">Then open the solution file in Visual Studio.</span></span> 

## <a name="review-the-code"></a><span data-ttu-id="35482-118">A kód áttekintése</span><span class="sxs-lookup"><span data-stu-id="35482-118">Review the code</span></span>

<span data-ttu-id="35482-119">Tekintsük át, hogy mi történik az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="35482-119">Let's make a quick review of what's happening in the app.</span></span> <span data-ttu-id="35482-120">Nyissa meg a **Dal.cs** fájlt a **DAL** könyvtárból: az itt található kódsorok hozzák létre az Azure Cosmos DB erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="35482-120">Open the **Dal.cs** file under the **DAL** directory and you'll find that these lines of code create the Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="35482-121">A Mongo Client inicializálása.</span><span class="sxs-lookup"><span data-stu-id="35482-121">Initialize the Mongo Client.</span></span>

    ```cs
        MongoClientSettings settings = new MongoClientSettings();
        settings.Server = new MongoServerAddress(host, 10255);
        settings.UseSsl = true;
        settings.SslSettings = new SslSettings();
        settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;

        MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
        MongoIdentityEvidence evidence = new PasswordEvidence(password);

        settings.Credentials = new List<MongoCredential>()
        {
            new MongoCredential("SCRAM-SHA-1", identity, evidence)
        };

        MongoClient client = new MongoClient(settings);
    ```

* <span data-ttu-id="35482-122">Az adatbázis és a gyűjtemény lekérése.</span><span class="sxs-lookup"><span data-stu-id="35482-122">Retrieve the database and the collection.</span></span>

    ```cs
    private string dbName = "Tasks";
    private string collectionName = "TasksList";

    var database = client.GetDatabase(dbName);
    var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
    ```

* <span data-ttu-id="35482-123">Az összes dokumentum lekérése.</span><span class="sxs-lookup"><span data-stu-id="35482-123">Retrieve all documents.</span></span>

    ```cs
    collection.Find(new BsonDocument()).ToList();
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="35482-124">A kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="35482-124">Update your connection string</span></span>

<span data-ttu-id="35482-125">Lépjen vissza az Azure Portalra a kapcsolati karakterlánc adataiért, majd másolja be azokat az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="35482-125">Now go back to the Azure portal to get your connection string information and copy it into the app.</span></span>

1. <span data-ttu-id="35482-126">Az [Azure Portalon](http://portal.azure.com/) az Azure Cosmos DB-fiókban a bal oldalsávon kattintson a **Kapcsolati karakterlánc** elemre, majd kattintson az **írási/olvasási kulcsok** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="35482-126">In the [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in the left navigation click **Connection String**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="35482-127">A következő lépésben használja a képernyő jobb oldalán lévő másolási gombokat a felhasználónév, a jelszó és a gazdagép másolásához a Dal.cs fájlba.</span><span class="sxs-lookup"><span data-stu-id="35482-127">You'll use the copy buttons on the right side of the screen to copy the Username, Password, and Host into the Dal.cs file in the next step.</span></span>

2. <span data-ttu-id="35482-128">Nyissa meg a **DAL** könyvtárban található **Dal.cs** fájlt.</span><span class="sxs-lookup"><span data-stu-id="35482-128">Open the **Dal.cs** file in the **DAL** directory.</span></span> 

3. <span data-ttu-id="35482-129">Másolja ki a **felhasználónév** érteket a Portalról (a másolási gomb használatával), és ezt adja meg a **felhasználónév** értékeként a **Dal.cs** fájlban.</span><span class="sxs-lookup"><span data-stu-id="35482-129">Copy your **username** value from the portal (using the copy button) and make it the value of the **username** in your **Dal.cs** file.</span></span> 

4. <span data-ttu-id="35482-130">Ezután másolja ki a **gazdagép** értékét a Portalról, és adja meg a **gazdagép** értékeként a **Dal.cs** fájlban.</span><span class="sxs-lookup"><span data-stu-id="35482-130">Then copy your **host** value from the portal and make it the value of the **host** in your **Dal.cs** file.</span></span> 

5. <span data-ttu-id="35482-131">Végezetül másolja ki a **jelszó** értékét a Portalról, és azt adja meg a **jelszó** értékeként a **Dal.cs** fájlban.</span><span class="sxs-lookup"><span data-stu-id="35482-131">Finally copy your **password** value from the portal and make it the value of the **password** in your **Dal.cs** file.</span></span> 

<span data-ttu-id="35482-132">Az alkalmazás frissítve lett minden olyan információval, amely az Azure Cosmos DB-vel való kommunikációhoz szükséges.</span><span class="sxs-lookup"><span data-stu-id="35482-132">You've now updated your app with all the info it needs to communicate with Azure Cosmos DB.</span></span> 
    
## <a name="run-the-web-app"></a><span data-ttu-id="35482-133">A webalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="35482-133">Run the web app</span></span>

1. <span data-ttu-id="35482-134">A Visual Studióban kattintson a jobb gombbal a **Megoldáskezelő** projektre, majd kattintson a **NuGet-csomagok kezelése** parancsra.</span><span class="sxs-lookup"><span data-stu-id="35482-134">In Visual Studio, right-click on the project in **Solution Explorer** and then click **Manage NuGet Packages**.</span></span> 

2. <span data-ttu-id="35482-135">A NuGet **Tallózás** mezőjébe írja be a *MongoDB.Driver* szöveget.</span><span class="sxs-lookup"><span data-stu-id="35482-135">In the NuGet **Browse** box, type *MongoDB.Driver*.</span></span>

3. <span data-ttu-id="35482-136">Az eredmények közül telepítse a **MongoDB.Driver** könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="35482-136">From the results, install the **MongoDB.Driver** library.</span></span> <span data-ttu-id="35482-137">Ez telepíti a MongoDB.Driver-csomagot és az összes függőségeit.</span><span class="sxs-lookup"><span data-stu-id="35482-137">This installs the MongoDB.Driver package as well as all dependencies.</span></span>

4. <span data-ttu-id="35482-138">Az alkalmazás futtatásához nyomja le a CTRL + F5 billentyűkombinációt.</span><span class="sxs-lookup"><span data-stu-id="35482-138">Click CTRL + F5 to run the application.</span></span> <span data-ttu-id="35482-139">Az alkalmazás megjelenik a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="35482-139">Your app displays in your browser.</span></span> 

5. <span data-ttu-id="35482-140">Kattintson a **Létrehozás** lehetőségre a böngészőben, és hozzon létre néhány új tevékenységet a tevékenységek lista alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="35482-140">Click **Create** in the browser and create a few new tasks in your task list app.</span></span>

## <a name="review-slas-in-the-azure-portal"></a><span data-ttu-id="35482-141">Tekintse át az SLA-kat az Azure Portalon</span><span class="sxs-lookup"><span data-stu-id="35482-141">Review SLAs in the Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="35482-142">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="35482-142">Clean up resources</span></span>

<span data-ttu-id="35482-143">Ha az alkalmazást már nem használja, akkor a következő lépésekkel a mintaalkalmazás által létrehozott összes erőforrást törölheti az Azure Portalon:</span><span class="sxs-lookup"><span data-stu-id="35482-143">If you're not going to continue to use this app, delete all resources created by this quickstart in the Azure portal with the following steps:</span></span>

1. <span data-ttu-id="35482-144">Az Azure Portal bal oldali menüjében kattintson az **Erőforráscsoportok** lehetőségre, majd kattintson a létrehozott erőforrás nevére.</span><span class="sxs-lookup"><span data-stu-id="35482-144">From the left-hand menu in the Azure portal, click **Resource groups** and then click the name of the resource you created.</span></span> 
2. <span data-ttu-id="35482-145">Az erőforráscsoport lapján kattintson a **Törlés** elemre, írja be a törölni kívánt erőforrás nevét a szövegmezőbe, majd kattintson a **Törlés** gombra.</span><span class="sxs-lookup"><span data-stu-id="35482-145">On your resource group page, click **Delete**, type the name of the resource to delete in the text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="35482-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="35482-146">Next steps</span></span>

<span data-ttu-id="35482-147">Ebben a rövid útmutatóban bemutattuk, hogyan lehet Azure Cosmos DB-fiókot létrehozni, és a MongoDB-hez tartozó API használatával webalkalmazást futtatni.</span><span class="sxs-lookup"><span data-stu-id="35482-147">In this quickstart, you've learned how to create an Azure Cosmos DB account and run a web app using the API for MongoDB.</span></span> <span data-ttu-id="35482-148">Most további adatokat importálhat a Cosmos DB-fiókba.</span><span class="sxs-lookup"><span data-stu-id="35482-148">You can now import additional data to your Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="35482-149">Adatok importálása az Azure Cosmos DB-be a MongoDB API-hoz</span><span class="sxs-lookup"><span data-stu-id="35482-149">Import data into Azure Cosmos DB for the MongoDB API</span></span>](mongodb-migrate.md)

