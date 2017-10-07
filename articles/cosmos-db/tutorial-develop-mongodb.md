---
title: "aaaUse Azure Cosmos DB API-t a MongoDB toobuild webalkalmazás |} Microsoft Docs"
description: "Egy Azure Cosmos adatbázis-oktatóanyag, amely egy online adatbázis webalkalmazás mongodb hello API használatával."
keywords: "mongodb-példák"
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 61a2ab3a-2fc3-4d49-a263-ed87c66628f6
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: anhoh
ms.custom: mvc
ms.openlocfilehash: 6dfa7fef49fc53ea2fcfcfbad3b3fcf97ac18e94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-connect-tooa-mongodb-app-using-net"></a><span data-ttu-id="50dea-104">Azure Cosmos DB: Csatlakozás tooa MongoDB alkalmazásokhoz .NET használatával</span><span class="sxs-lookup"><span data-stu-id="50dea-104">Azure Cosmos DB: Connect tooa MongoDB app using .NET</span></span>

<span data-ttu-id="50dea-105">Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="50dea-105">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="50dea-106">Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="50dea-106">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="50dea-107">Ez az oktatóanyag bemutatja, hogyan egy Azure Cosmos DB fiók használatával toocreate hello Azure-portálon, és hogyan toocreate adatbázis és gyűjtemény toostore adatok hello [MongoDB API](mongodb-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="50dea-107">This tutorial demonstrates how toocreate an Azure Cosmos DB account using hello Azure portal, and how toocreate a database and collection toostore data using hello [MongoDB API](mongodb-introduction.md).</span></span> 

<span data-ttu-id="50dea-108">Ez az oktatóanyag ismerteti a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="50dea-108">This tutorial covers hello following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="50dea-109">Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="50dea-109">Create an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="50dea-110">A kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="50dea-110">Update your connection string</span></span>
> * <span data-ttu-id="50dea-111">A virtuális gépen a MongoDB-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="50dea-111">Create a MongoDB app on a virtual machine</span></span> 


## <a name="create-a-database-account"></a><span data-ttu-id="50dea-112">Adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="50dea-112">Create a database account</span></span>

<span data-ttu-id="50dea-113">Először hozzon létre egy Azure Cosmos DB fiókot a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="50dea-113">Let's start by creating an Azure Cosmos DB account in hello Azure portal.</span></span>  

> [!TIP]
> * <span data-ttu-id="50dea-114">Már van Azure Cosmos DB fiókja?</span><span class="sxs-lookup"><span data-stu-id="50dea-114">Already have an Azure Cosmos DB account?</span></span> <span data-ttu-id="50dea-115">Ha igen, hagyja ki azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS)</span><span class="sxs-lookup"><span data-stu-id="50dea-115">If so, skip ahead too[Set up your Visual Studio solution](#SetupVS)</span></span>
> * <span data-ttu-id="50dea-116">Kellett az Azure DocumentDB-fiókot?</span><span class="sxs-lookup"><span data-stu-id="50dea-116">Did you have an Azure DocumentDB account?</span></span> <span data-ttu-id="50dea-117">Ha igen, a fiókját most már Azure Cosmos DB fiókkal, és kihagyhatja azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="50dea-117">If so, your account is now an Azure Cosmos DB account and you can skip ahead too[Set up your Visual Studio solution](#SetupVS).</span></span>  
> * <span data-ttu-id="50dea-118">Ha hello Azure Cosmos DB Emulator használata esetén kövesse hello készítésével [Azure Cosmos DB emulátor](local-emulator.md) toosetup hello emulátor, és hagyja ki azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="50dea-118">If you are using hello Azure Cosmos DB Emulator, please follow hello steps at [Azure Cosmos DB Emulator](local-emulator.md) toosetup hello emulator and skip ahead too[Set up your Visual Studio Solution](#SetupVS).</span></span> 
>
>

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="update-your-connection-string"></a><span data-ttu-id="50dea-119">A kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="50dea-119">Update your connection string</span></span>

1. <span data-ttu-id="50dea-120">Az Azure portálon hello hello **Azure Cosmos DB** lapon, válassza ki a hello API-t a MongoDB-fiók.</span><span class="sxs-lookup"><span data-stu-id="50dea-120">In hello Azure portal, in hello **Azure Cosmos DB** page, select hello API for MongoDB account.</span></span> 
2. <span data-ttu-id="50dea-121">Hello bal oldali sávon hello fiók panelen kattintson **gyors üzembe helyezési**.</span><span class="sxs-lookup"><span data-stu-id="50dea-121">In hello left bar of hello account blade, click **Quick start**.</span></span> 
3. <span data-ttu-id="50dea-122">Válassza ki a platformot (*.NET illesztőprogram*, *Node.js illesztőprogram*, *MongoDB rendszerhéj*, *Java illesztőprogram*, *Python illesztőprogram*).</span><span class="sxs-lookup"><span data-stu-id="50dea-122">Choose your platform (*.NET driver*, *Node.js driver*, *MongoDB Shell*, *Java driver*, *Python driver*).</span></span> <span data-ttu-id="50dea-123">Ha nem látja az illesztőprogram vagy eszköz szerepel, ne aggódjon, azt folyamatosan dokumentum további kapcsolat-kódrészleteket.</span><span class="sxs-lookup"><span data-stu-id="50dea-123">If you don't see your driver or tool listed, don't worry, we continuously document more connection code snippets.</span></span> 
4. <span data-ttu-id="50dea-124">Másolja és illessze be a hello kódrészletet a MongoDB alkalmazásba, és készen áll a toogo.</span><span class="sxs-lookup"><span data-stu-id="50dea-124">Copy and paste hello code snippet into your MongoDB app, and you are ready toogo.</span></span>

## <a name="set-up-your-mongodb-app"></a><span data-ttu-id="50dea-125">A MongoDB-alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="50dea-125">Set up your MongoDB app</span></span>

<span data-ttu-id="50dea-126">Hello használható [egy webalkalmazás létrehozása az Azure virtuális gépen futó tooMongoDB csatlakozó](../app-service-web/web-sites-dotnet-store-data-mongodb-vm.md) oktatóanyag, minimális módosítással, tooquickly beállítása a MongoDB-alkalmazás (vagy helyileg vagy közzétett tooan Azure web app), amely Csatlakozás tooan API-t a MongoDB-fiók.</span><span class="sxs-lookup"><span data-stu-id="50dea-126">You can use hello [Create a web app in Azure that connects tooMongoDB running on a virtual machine](../app-service-web/web-sites-dotnet-store-data-mongodb-vm.md) tutorial, with minimal modification, tooquickly setup a MongoDB application (either locally or published tooan Azure web app) that connects tooan API for MongoDB account.</span></span>  

1. <span data-ttu-id="50dea-127">Az oktatóanyag hello, egy módosítással.</span><span class="sxs-lookup"><span data-stu-id="50dea-127">Follow hello tutorial, with one modification.</span></span>  <span data-ttu-id="50dea-128">Hello Dal.cs kód cserélje le ezt:</span><span class="sxs-lookup"><span data-stu-id="50dea-128">Replace hello Dal.cs code with this:</span></span>

    ```csharp   
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using MyTaskListApp.Models;
    using MongoDB.Driver;
    using MongoDB.Bson;
    using System.Configuration;
    using System.Security.Authentication;
   
    namespace MyTaskListApp
    {
        public class Dal : IDisposable
        {
            //private MongoServer mongoServer = null;
            private bool disposed = false;
   
            // toodo: update hello connection string with hello DNS name
            // or IP address of your server. 
            //For example, "mongodb://testlinux.cloudapp.net
            private string connectionString = "mongodb://localhost:27017";
            private string userName = "<your user name>";
            private string host = "<your host>";
            private string password = "<your password>";
   
            // This sample uses a database named "Tasks" and a 
            //collection named "TasksList".  hello database and collection 
            //will be automatically created if they don't already exist.
            private string dbName = "Tasks";
            private string collectionName = "TasksList";
   
            // Default constructor.        
            public Dal()
            {
            }
   
            // Gets all Task items from hello MongoDB server.        
            public List<MyTask> GetAllTasks()
            {
                try
                {
                    var collection = GetTasksCollection();
                    return collection.Find(new BsonDocument()).ToList();
                }
                catch (MongoConnectionException)
                {
                    return new List<MyTask>();
                }
            }
   
            // Creates a Task and inserts it into hello collection in MongoDB.
            public void CreateTask(MyTask task)
            {
                var collection = GetTasksCollectionForEdit();
                try
                {
                    collection.InsertOne(task);
                }
                catch (MongoCommandException ex)
                {
                    string msg = ex.Message;
                }
            }
   
            private IMongoCollection<MyTask> GetTasksCollection()
            {
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
                var database = client.GetDatabase(dbName);
                var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                return todoTaskCollection;
            }
   
            private IMongoCollection<MyTask> GetTasksCollectionForEdit()
            {
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
                var database = client.GetDatabase(dbName);
                var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                return todoTaskCollection;
            }
   
            # region IDisposable
   
            public void Dispose()
            {
                this.Dispose(true);
                GC.SuppressFinalize(this);
            }
   
            protected virtual void Dispose(bool disposing)
            {
                if (!this.disposed)
                {
                    if (disposing)
                    {
                    }
                }
   
                this.disposed = true;
            }
   
            # endregion
        }
    }
    ```

2. <span data-ttu-id="50dea-129">Módosítsa a változók / fiókbeállításokban hello Dal.cs fájlban következő oldaláról hello kulcsok hello Azure-portálon hello:</span><span class="sxs-lookup"><span data-stu-id="50dea-129">Modify hello following variables in hello Dal.cs file per your account settings from hello Keys page in hello Azure portal:</span></span>

    ```csharp   
    private string userName = "<your user name>";
    private string host = "<your host>";
    private string password = "<your password>";
    ```

3. <span data-ttu-id="50dea-130">Hello alkalmazás használatát!</span><span class="sxs-lookup"><span data-stu-id="50dea-130">Use hello app!</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="50dea-131">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="50dea-131">Clean up resources</span></span>

<span data-ttu-id="50dea-132">Toocontinue toouse az alkalmazás nem fog, ha használja a következő lépéseket toodelete hello Azure-portálon az oktatóprogram során létrehozott összes erőforrás hello.</span><span class="sxs-lookup"><span data-stu-id="50dea-132">If you're not going toocontinue toouse this app, use hello following steps toodelete all resources created by this tutorial in hello Azure portal.</span></span> 

1. <span data-ttu-id="50dea-133">A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson a létrehozott hello erőforrás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="50dea-133">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="50dea-134">Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="50dea-134">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="50dea-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="50dea-135">Next steps</span></span>

<span data-ttu-id="50dea-136">Ebben az oktatóanyagban hello következő régebben már kötöttek:</span><span class="sxs-lookup"><span data-stu-id="50dea-136">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="50dea-137">Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="50dea-137">Create an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="50dea-138">A kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="50dea-138">Update your connection string</span></span>
> * <span data-ttu-id="50dea-139">A virtuális gépen a MongoDB-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="50dea-139">Create a MongoDB app on a virtual machine</span></span>

<span data-ttu-id="50dea-140">Folytassa a következő oktatóanyag toohello, és importálja a MongoDB-adatok tooAzure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="50dea-140">You can proceed toohello next tutorial and import your MongoDB data tooAzure Cosmos DB.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="50dea-141">MongoDB adatok importálása az Azure Cosmos DB-be</span><span class="sxs-lookup"><span data-stu-id="50dea-141">Import MongoDB data into Azure Cosmos DB</span></span>](mongodb-migrate.md)

