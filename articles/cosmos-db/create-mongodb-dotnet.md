---
title: "Azure Cosmos DB: A .NET webalkalmazás létrehozása és MongoDB API hello |} Microsoft Docs"
description: "Megadja a .NET kódminta, használhatja a tooconnect tooand lekérdezés hello Azure Cosmos DB MongoDB API"
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
ms.openlocfilehash: c85cc47f772a19aaa7181611b75a8acaedbc4c42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-mongodb-api-web-app-with-net-and-hello-azure-portal"></a><span data-ttu-id="92857-103">Azure Cosmos-adatbázis: A .NET MongoDB API webalkalmazás létrehozása és hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="92857-103">Azure Cosmos DB: Build a MongoDB API web app with .NET and hello Azure portal</span></span>

<span data-ttu-id="92857-104">Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="92857-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="92857-105">Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="92857-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="92857-106">A gyors üzembe helyezési bemutatja, hogyan toocreate Azure Cosmos DB fiókkal, a dokumentum-adatbázis és gyűjtemény használja hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="92857-106">This quick start demonstrates how toocreate an Azure Cosmos DB account, document database, and collection using hello Azure portal.</span></span> <span data-ttu-id="92857-107">Kell majd build és hello épülő feladatok lista webalkalmazás üzembe helyezése [MongoDB .NET illesztőprogram](https://docs.mongodb.com/ecosystem/drivers/csharp/).</span><span class="sxs-lookup"><span data-stu-id="92857-107">You'll then build and deploy a tasks list web app built on hello [MongoDB .NET driver](https://docs.mongodb.com/ecosystem/drivers/csharp/).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="92857-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="92857-108">Prerequisites</span></span>

<span data-ttu-id="92857-109">Ha még nincs telepítve a Visual Studio 2017, töltse le és használja a hello **szabad** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="92857-109">If you don’t already have Visual Studio 2017 installed, you can download and use hello **free** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="92857-110">Győződjön meg arról, hogy engedélyezze **Azure fejlesztési** hello Visual Studio telepítése során.</span><span class="sxs-lookup"><span data-stu-id="92857-110">Make sure that you enable **Azure development** during hello Visual Studio setup.</span></span>
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-account"></a>
## <a name="create-a-database-account"></a><span data-ttu-id="92857-111">Adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="92857-111">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="clone-hello-sample-application"></a><span data-ttu-id="92857-112">Klónozza a mintaalkalmazást hello</span><span class="sxs-lookup"><span data-stu-id="92857-112">Clone hello sample application</span></span>

<span data-ttu-id="92857-113">Most tegyük a githubból, a klón a MongoDB API app hello kapcsolati karakterlánc beállítása, és futtassa azt.</span><span class="sxs-lookup"><span data-stu-id="92857-113">Now let's clone a MongoDB API app from github, set hello connection string, and run it.</span></span> <span data-ttu-id="92857-114">Láthatja, milyen egyszerűen adatokkal toowork programozott módon.</span><span class="sxs-lookup"><span data-stu-id="92857-114">You'll see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="92857-115">Nyisson meg egy git terminálablakot, például a git bash eszközt, és `cd` tooa munkakönyvtárát.</span><span class="sxs-lookup"><span data-stu-id="92857-115">Open a git terminal window, such as git bash, and `cd` tooa working directory.</span></span>  

2. <span data-ttu-id="92857-116">Futtassa a következő parancs tooclone hello minta tárház hello.</span><span class="sxs-lookup"><span data-stu-id="92857-116">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-getting-started.git
    ```

3. <span data-ttu-id="92857-117">Ezután nyissa meg a hello megoldásfájlt a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="92857-117">Then open hello solution file in Visual Studio.</span></span> 

## <a name="review-hello-code"></a><span data-ttu-id="92857-118">Tekintse át a hello kódot</span><span class="sxs-lookup"><span data-stu-id="92857-118">Review hello code</span></span>

<span data-ttu-id="92857-119">Most Meggyőződünk arról, mi történik a hello app gyors áttekintése.</span><span class="sxs-lookup"><span data-stu-id="92857-119">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="92857-120">Nyissa meg hello **Dal.cs** hello fájlt **DAL** könyvtár, és látható, hogy ezek a sorok, a kód hello Azure Cosmos DB erőforrások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="92857-120">Open hello **Dal.cs** file under hello **DAL** directory and you'll find that these lines of code create hello Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="92857-121">Hello Mongo ügyfél inicializálása.</span><span class="sxs-lookup"><span data-stu-id="92857-121">Initialize hello Mongo Client.</span></span>

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

* <span data-ttu-id="92857-122">Hello adatbázis és gyűjtemény hello beolvasása.</span><span class="sxs-lookup"><span data-stu-id="92857-122">Retrieve hello database and hello collection.</span></span>

    ```cs
    private string dbName = "Tasks";
    private string collectionName = "TasksList";

    var database = client.GetDatabase(dbName);
    var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
    ```

* <span data-ttu-id="92857-123">Az összes dokumentum lekérése.</span><span class="sxs-lookup"><span data-stu-id="92857-123">Retrieve all documents.</span></span>

    ```cs
    collection.Find(new BsonDocument()).ToList();
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="92857-124">A kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="92857-124">Update your connection string</span></span>

<span data-ttu-id="92857-125">Most lépjen vissza az Azure portál tooget toohello kapcsolati karakterlánc adatainak és hello alkalmazásba másolja.</span><span class="sxs-lookup"><span data-stu-id="92857-125">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span>

1. <span data-ttu-id="92857-126">A hello [Azure-portálon](http://portal.azure.com/), az Azure Cosmos DB a fiókot, kattintson a bal oldali navigációs hello **kapcsolati karakterlánc**, és kattintson a **írható-olvasható kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="92857-126">In hello [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in hello left navigation click **Connection String**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="92857-127">Hello másolási gombok hello jobb oldalán hello képernyő toocopy hello felhasználónév, jelszó és a kiszolgáló hello Dal.cs fájlba hello következő lépésben fogja használni.</span><span class="sxs-lookup"><span data-stu-id="92857-127">You'll use hello copy buttons on hello right side of hello screen toocopy hello Username, Password, and Host into hello Dal.cs file in hello next step.</span></span>

2. <span data-ttu-id="92857-128">Nyissa meg hello **Dal.cs** hello fájlban **DAL** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="92857-128">Open hello **Dal.cs** file in hello **DAL** directory.</span></span> 

3. <span data-ttu-id="92857-129">Másolás a **felhasználónév** portálról hello (hello Másolás gombra) értékét, és könnyebben hello értékének hello **felhasználónév** a a **Dal.cs** fájlt.</span><span class="sxs-lookup"><span data-stu-id="92857-129">Copy your **username** value from hello portal (using hello copy button) and make it hello value of hello **username** in your **Dal.cs** file.</span></span> 

4. <span data-ttu-id="92857-130">Másolja a **állomás** hello portálról értékét, és könnyebben hello értékének hello **állomás** a a **Dal.cs** fájlt.</span><span class="sxs-lookup"><span data-stu-id="92857-130">Then copy your **host** value from hello portal and make it hello value of hello **host** in your **Dal.cs** file.</span></span> 

5. <span data-ttu-id="92857-131">Végül másolja a **jelszó** hello portálról értékét, és könnyebben hello értékének hello **jelszó** a a **Dal.cs** fájlt.</span><span class="sxs-lookup"><span data-stu-id="92857-131">Finally copy your **password** value from hello portal and make it hello value of hello **password** in your **Dal.cs** file.</span></span> 

<span data-ttu-id="92857-132">Most már frissítette az alkalmazást az Azure Cosmos DB toocommunicate szükséges összes hello információval.</span><span class="sxs-lookup"><span data-stu-id="92857-132">You've now updated your app with all hello info it needs toocommunicate with Azure Cosmos DB.</span></span> 
    
## <a name="run-hello-web-app"></a><span data-ttu-id="92857-133">Hello webes alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="92857-133">Run hello web app</span></span>

1. <span data-ttu-id="92857-134">A Visual Studióban, kattintson a jobb gombbal a hello projekt **Megoldáskezelőben** majd **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="92857-134">In Visual Studio, right-click on hello project in **Solution Explorer** and then click **Manage NuGet Packages**.</span></span> 

2. <span data-ttu-id="92857-135">A hello NuGet **Tallózás** mezőbe írja be *MongoDB.Driver*.</span><span class="sxs-lookup"><span data-stu-id="92857-135">In hello NuGet **Browse** box, type *MongoDB.Driver*.</span></span>

3. <span data-ttu-id="92857-136">Hello eredmények közül telepítse a hello **MongoDB.Driver** könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="92857-136">From hello results, install hello **MongoDB.Driver** library.</span></span> <span data-ttu-id="92857-137">Ez telepíti a hello MongoDB.Driver csomagot, valamint az összes függősége.</span><span class="sxs-lookup"><span data-stu-id="92857-137">This installs hello MongoDB.Driver package as well as all dependencies.</span></span>

4. <span data-ttu-id="92857-138">Kattintson a CTRL + F5 toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="92857-138">Click CTRL + F5 toorun hello application.</span></span> <span data-ttu-id="92857-139">Az alkalmazás megjelenik a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="92857-139">Your app displays in your browser.</span></span> 

5. <span data-ttu-id="92857-140">Kattintson a **létrehozása** a böngésző hello és néhány új feladatot létrehozni a feladat alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="92857-140">Click **Create** in hello browser and create a few new tasks in your task list app.</span></span>

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="92857-141">Tekintse át a szolgáltatásiszint-szerződések a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="92857-141">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="92857-142">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="92857-142">Clean up resources</span></span>

<span data-ttu-id="92857-143">Toocontinue toouse az alkalmazás nem fog, ha törli az összes erőforrást hozta létre a gyors üzembe helyezés hello az Azure-portálon az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="92857-143">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="92857-144">A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson a létrehozott hello erőforrás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="92857-144">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="92857-145">Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="92857-145">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="92857-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="92857-146">Next steps</span></span>

<span data-ttu-id="92857-147">A gyors üzembe helyezés mér megismerte, hogyan toocreate Azure Cosmos DB fiók és a web app használatával futtatja hello API mongodb-protokolltámogatással.</span><span class="sxs-lookup"><span data-stu-id="92857-147">In this quickstart, you've learned how toocreate an Azure Cosmos DB account and run a web app using hello API for MongoDB.</span></span> <span data-ttu-id="92857-148">További adatok tooyour Cosmos DB fiókot most importálhatja.</span><span class="sxs-lookup"><span data-stu-id="92857-148">You can now import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="92857-149">Adatok importálása az Azure Cosmos DB a hello MongoDB API</span><span class="sxs-lookup"><span data-stu-id="92857-149">Import data into Azure Cosmos DB for hello MongoDB API</span></span>](mongodb-migrate.md)

