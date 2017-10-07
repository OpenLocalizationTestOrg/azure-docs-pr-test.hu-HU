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
# <a name="azure-cosmos-db-connect-tooa-mongodb-app-using-net"></a>Azure Cosmos DB: Csatlakozás tooa MongoDB alkalmazásokhoz .NET használatával

Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása. Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése. 

Ez az oktatóanyag bemutatja, hogyan egy Azure Cosmos DB fiók használatával toocreate hello Azure-portálon, és hogyan toocreate adatbázis és gyűjtemény toostore adatok hello [MongoDB API](mongodb-introduction.md). 

Ez az oktatóanyag ismerteti a következő feladatok hello:

> [!div class="checklist"]
> * Azure Cosmos DB-fiók létrehozása 
> * A kapcsolati karakterlánc frissítése
> * A virtuális gépen a MongoDB-alkalmazás létrehozása 


## <a name="create-a-database-account"></a>Adatbázisfiók létrehozása

Először hozzon létre egy Azure Cosmos DB fiókot a hello Azure-portálon.  

> [!TIP]
> * Már van Azure Cosmos DB fiókja? Ha igen, hagyja ki azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS)
> * Kellett az Azure DocumentDB-fiókot? Ha igen, a fiókját most már Azure Cosmos DB fiókkal, és kihagyhatja azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS).  
> * Ha hello Azure Cosmos DB Emulator használata esetén kövesse hello készítésével [Azure Cosmos DB emulátor](local-emulator.md) toosetup hello emulátor, és hagyja ki azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS). 
>
>

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="update-your-connection-string"></a>A kapcsolati karakterlánc frissítése

1. Az Azure portálon hello hello **Azure Cosmos DB** lapon, válassza ki a hello API-t a MongoDB-fiók. 
2. Hello bal oldali sávon hello fiók panelen kattintson **gyors üzembe helyezési**. 
3. Válassza ki a platformot (*.NET illesztőprogram*, *Node.js illesztőprogram*, *MongoDB rendszerhéj*, *Java illesztőprogram*, *Python illesztőprogram*). Ha nem látja az illesztőprogram vagy eszköz szerepel, ne aggódjon, azt folyamatosan dokumentum további kapcsolat-kódrészleteket. 
4. Másolja és illessze be a hello kódrészletet a MongoDB alkalmazásba, és készen áll a toogo.

## <a name="set-up-your-mongodb-app"></a>A MongoDB-alkalmazás beállítása

Hello használható [egy webalkalmazás létrehozása az Azure virtuális gépen futó tooMongoDB csatlakozó](../app-service-web/web-sites-dotnet-store-data-mongodb-vm.md) oktatóanyag, minimális módosítással, tooquickly beállítása a MongoDB-alkalmazás (vagy helyileg vagy közzétett tooan Azure web app), amely Csatlakozás tooan API-t a MongoDB-fiók.  

1. Az oktatóanyag hello, egy módosítással.  Hello Dal.cs kód cserélje le ezt:

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

2. Módosítsa a változók / fiókbeállításokban hello Dal.cs fájlban következő oldaláról hello kulcsok hello Azure-portálon hello:

    ```csharp   
    private string userName = "<your user name>";
    private string host = "<your host>";
    private string password = "<your password>";
    ```

3. Hello alkalmazás használatát!

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Toocontinue toouse az alkalmazás nem fog, ha használja a következő lépéseket toodelete hello Azure-portálon az oktatóprogram során létrehozott összes erőforrás hello. 

1. A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson a létrehozott hello erőforrás hello nevét. 
2. Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban hello következő régebben már kötöttek:

> [!div class="checklist"]
> * Azure Cosmos DB-fiók létrehozása 
> * A kapcsolati karakterlánc frissítése
> * A virtuális gépen a MongoDB-alkalmazás létrehozása

Folytassa a következő oktatóanyag toohello, és importálja a MongoDB-adatok tooAzure Cosmos DB.  

> [!div class="nextstepaction"]
> [MongoDB adatok importálása az Azure Cosmos DB-be](mongodb-migrate.md)

