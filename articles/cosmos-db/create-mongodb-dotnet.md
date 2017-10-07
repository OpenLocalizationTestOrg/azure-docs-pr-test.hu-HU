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
# <a name="azure-cosmos-db-build-a-mongodb-api-web-app-with-net-and-hello-azure-portal"></a>Azure Cosmos-adatbázis: A .NET MongoDB API webalkalmazás létrehozása és hello Azure-portálon

Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása. Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése. 

A gyors üzembe helyezési bemutatja, hogyan toocreate Azure Cosmos DB fiókkal, a dokumentum-adatbázis és gyűjtemény használja hello Azure-portálon. Kell majd build és hello épülő feladatok lista webalkalmazás üzembe helyezése [MongoDB .NET illesztőprogram](https://docs.mongodb.com/ecosystem/drivers/csharp/). 

## <a name="prerequisites"></a>Előfeltételek

Ha még nincs telepítve a Visual Studio 2017, töltse le és használja a hello **szabad** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Győződjön meg arról, hogy engedélyezze **Azure fejlesztési** hello Visual Studio telepítése során.
[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-account"></a>
## <a name="create-a-database-account"></a>Adatbázisfiók létrehozása

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="clone-hello-sample-application"></a>Klónozza a mintaalkalmazást hello

Most tegyük a githubból, a klón a MongoDB API app hello kapcsolati karakterlánc beállítása, és futtassa azt. Láthatja, milyen egyszerűen adatokkal toowork programozott módon. 

1. Nyisson meg egy git terminálablakot, például a git bash eszközt, és `cd` tooa munkakönyvtárát.  

2. Futtassa a következő parancs tooclone hello minta tárház hello. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-getting-started.git
    ```

3. Ezután nyissa meg a hello megoldásfájlt a Visual Studio. 

## <a name="review-hello-code"></a>Tekintse át a hello kódot

Most Meggyőződünk arról, mi történik a hello app gyors áttekintése. Nyissa meg hello **Dal.cs** hello fájlt **DAL** könyvtár, és látható, hogy ezek a sorok, a kód hello Azure Cosmos DB erőforrások létrehozása. 

* Hello Mongo ügyfél inicializálása.

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

* Hello adatbázis és gyűjtemény hello beolvasása.

    ```cs
    private string dbName = "Tasks";
    private string collectionName = "TasksList";

    var database = client.GetDatabase(dbName);
    var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
    ```

* Az összes dokumentum lekérése.

    ```cs
    collection.Find(new BsonDocument()).ToList();
    ```

## <a name="update-your-connection-string"></a>A kapcsolati karakterlánc frissítése

Most lépjen vissza az Azure portál tooget toohello kapcsolati karakterlánc adatainak és hello alkalmazásba másolja.

1. A hello [Azure-portálon](http://portal.azure.com/), az Azure Cosmos DB a fiókot, kattintson a bal oldali navigációs hello **kapcsolati karakterlánc**, és kattintson a **írható-olvasható kulcsok**. Hello másolási gombok hello jobb oldalán hello képernyő toocopy hello felhasználónév, jelszó és a kiszolgáló hello Dal.cs fájlba hello következő lépésben fogja használni.

2. Nyissa meg hello **Dal.cs** hello fájlban **DAL** könyvtár. 

3. Másolás a **felhasználónév** portálról hello (hello Másolás gombra) értékét, és könnyebben hello értékének hello **felhasználónév** a a **Dal.cs** fájlt. 

4. Másolja a **állomás** hello portálról értékét, és könnyebben hello értékének hello **állomás** a a **Dal.cs** fájlt. 

5. Végül másolja a **jelszó** hello portálról értékét, és könnyebben hello értékének hello **jelszó** a a **Dal.cs** fájlt. 

Most már frissítette az alkalmazást az Azure Cosmos DB toocommunicate szükséges összes hello információval. 
    
## <a name="run-hello-web-app"></a>Hello webes alkalmazás futtatása

1. A Visual Studióban, kattintson a jobb gombbal a hello projekt **Megoldáskezelőben** majd **NuGet-csomagok kezelése**. 

2. A hello NuGet **Tallózás** mezőbe írja be *MongoDB.Driver*.

3. Hello eredmények közül telepítse a hello **MongoDB.Driver** könyvtárban. Ez telepíti a hello MongoDB.Driver csomagot, valamint az összes függősége.

4. Kattintson a CTRL + F5 toorun hello alkalmazás. Az alkalmazás megjelenik a böngészőben. 

5. Kattintson a **létrehozása** a böngésző hello és néhány új feladatot létrehozni a feladat alkalmazás.

## <a name="review-slas-in-hello-azure-portal"></a>Tekintse át a szolgáltatásiszint-szerződések a hello Azure-portálon

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Toocontinue toouse az alkalmazás nem fog, ha törli az összes erőforrást hozta létre a gyors üzembe helyezés hello az Azure-portálon az alábbi lépésekkel hello:

1. A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson a létrehozott hello erőforrás hello nevét. 
2. Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.

## <a name="next-steps"></a>Következő lépések

A gyors üzembe helyezés mér megismerte, hogyan toocreate Azure Cosmos DB fiók és a web app használatával futtatja hello API mongodb-protokolltámogatással. További adatok tooyour Cosmos DB fiókot most importálhatja. 

> [!div class="nextstepaction"]
> [Adatok importálása az Azure Cosmos DB a hello MongoDB API](mongodb-migrate.md)

