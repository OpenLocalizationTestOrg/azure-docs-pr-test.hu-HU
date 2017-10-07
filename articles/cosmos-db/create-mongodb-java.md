---
title: "Azure Cosmos DB: Egy Java-Konzolalkalmazás létrehozása és a MongoDB API hello |} Microsoft Docs"
description: "Megadja a Java kódminta, használhatja a tooconnect tooand lekérdezés hello Azure Cosmos DB MongoDB API"
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
ms.devlang: java
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: fbe416f6b20ed2bb83a1d41eb70ffc6e3cee2b61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-mongodb-api-console-app-with-java-and-hello-azure-portal"></a><span data-ttu-id="e3d98-103">Azure Cosmos DB: Hozza létre egy Java MongoDB API-Konzolalkalmazás, és hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e3d98-103">Azure Cosmos DB: Build a MongoDB API console app with Java and hello Azure portal</span></span>

<span data-ttu-id="e3d98-104">Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="e3d98-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="e3d98-105">Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="e3d98-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="e3d98-106">A gyors üzembe helyezési bemutatja, hogyan toocreate Azure Cosmos DB fiókkal, a dokumentum-adatbázis és gyűjtemény használja hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="e3d98-106">This quick start demonstrates how toocreate an Azure Cosmos DB account, document database, and collection using hello Azure portal.</span></span> <span data-ttu-id="e3d98-107">Kell majd létrehozása és központi telepítése egy konzolalkalmazás hello épülő [MongoDB Java illesztőprogram](https://docs.mongodb.com/ecosystem/drivers/java/).</span><span class="sxs-lookup"><span data-stu-id="e3d98-107">You'll then build and deploy a console app built on hello [MongoDB Java driver](https://docs.mongodb.com/ecosystem/drivers/java/).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="e3d98-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e3d98-108">Prerequisites</span></span>

* <span data-ttu-id="e3d98-109">Ez a minta futtatásához, a következő előfeltételek hello kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="e3d98-109">Before you can run this sample, you must have hello following prerequisites:</span></span>
   * <span data-ttu-id="e3d98-110">JDK 1.7+ (ha még nem rendelkezik a JDK-val, futtassa az `apt-get install default-jdk` parancsot)</span><span class="sxs-lookup"><span data-stu-id="e3d98-110">JDK 1.7+ (Run `apt-get install default-jdk` if you don't have JDK)</span></span>
   * <span data-ttu-id="e3d98-111">Maven (ha nem rendelkezik Maven-nel, futtassa az `apt-get install maven` parancsot)</span><span class="sxs-lookup"><span data-stu-id="e3d98-111">Maven (Run `apt-get install maven` if you don't have Maven)</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="e3d98-112">Adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="e3d98-112">Create a database account</span></span>

[!INCLUDE [mongodb-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="add-a-collection"></a><span data-ttu-id="e3d98-113">Gyűjtemény hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e3d98-113">Add a collection</span></span>

<span data-ttu-id="e3d98-114">Az új adatbázis neve legyen **db**, az új gyűjteményé pedig **coll**.</span><span class="sxs-lookup"><span data-stu-id="e3d98-114">Name your new database, **db**, and your new collection, **coll**.</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-hello-sample-application"></a><span data-ttu-id="e3d98-115">Klónozza a mintaalkalmazást hello</span><span class="sxs-lookup"><span data-stu-id="e3d98-115">Clone hello sample application</span></span>

<span data-ttu-id="e3d98-116">Most tegyük a githubból, a klón a MongoDB API app hello kapcsolati karakterlánc beállítása, és futtassa azt.</span><span class="sxs-lookup"><span data-stu-id="e3d98-116">Now let's clone a MongoDB API app from github, set hello connection string, and run it.</span></span> <span data-ttu-id="e3d98-117">Láthatja, milyen egyszerűen adatokkal toowork programozott módon.</span><span class="sxs-lookup"><span data-stu-id="e3d98-117">You'll see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="e3d98-118">Nyisson meg egy git terminálablakot, például a git bash eszközt, és `cd` tooa munkakönyvtárát.</span><span class="sxs-lookup"><span data-stu-id="e3d98-118">Open a git terminal window, such as git bash, and `cd` tooa working directory.</span></span>  

2. <span data-ttu-id="e3d98-119">Futtassa a következő parancs tooclone hello minta tárház hello.</span><span class="sxs-lookup"><span data-stu-id="e3d98-119">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-getting-started.git
    ```

3. <span data-ttu-id="e3d98-120">Ezután nyissa meg a hello megoldásfájlt a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e3d98-120">Then open hello solution file in Visual Studio.</span></span> 

## <a name="review-hello-code"></a><span data-ttu-id="e3d98-121">Tekintse át a hello kódot</span><span class="sxs-lookup"><span data-stu-id="e3d98-121">Review hello code</span></span>

<span data-ttu-id="e3d98-122">Most Meggyőződünk arról, mi történik a hello app gyors áttekintése.</span><span class="sxs-lookup"><span data-stu-id="e3d98-122">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="e3d98-123">Nyissa meg hello `Program.cs` fájlt, és látható, hogy ezek a sorok, a kód létrehozni hello Azure Cosmos DB-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="e3d98-123">Open hello `Program.cs` file and you'll find that these lines of code create hello Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="e3d98-124">hello DocumentClient inicializálva van.</span><span class="sxs-lookup"><span data-stu-id="e3d98-124">hello DocumentClient is initialized.</span></span>

    ```java
    MongoClientURI uri = new MongoClientURI("FILLME");`

    MongoClient mongoClient = new MongoClient(uri);            
    ```

* <span data-ttu-id="e3d98-125">A rendszer létrehozza az új adatbázist és a gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="e3d98-125">A new database and collection are created.</span></span>

    ```java
    MongoDatabase database = mongoClient.getDatabase("db");

    MongoCollection<Document> collection = database.getCollection("coll");
    ```

* <span data-ttu-id="e3d98-126">A rendszer beilleszt néhány dokumentumot a `MongoCollection.insertOne` használatával</span><span class="sxs-lookup"><span data-stu-id="e3d98-126">Some documents are inserted using `MongoCollection.insertOne`</span></span>

    ```java
    Document document = new Document("fruit", "apple")
    collection.insertOne(document);
    ```

* <span data-ttu-id="e3d98-127">A rendszer végrehajt néhány lekérdezést a `MongoCollection.find` használatával</span><span class="sxs-lookup"><span data-stu-id="e3d98-127">Some queries are performed using `MongoCollection.find`</span></span>

    ```java
    Document queryResult = collection.find(Filters.eq("fruit", "apple")).first();
    System.out.println(queryResult.toJson());       
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="e3d98-128">A kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="e3d98-128">Update your connection string</span></span>

<span data-ttu-id="e3d98-129">Most lépjen vissza az Azure portál tooget toohello kapcsolati karakterlánc adatainak és hello alkalmazásba másolja.</span><span class="sxs-lookup"><span data-stu-id="e3d98-129">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span>

1. <span data-ttu-id="e3d98-130">Hello fiók, jelölje ki **gyors üzembe helyezés**, válassza ki a Java, majd hello kapcsolati karakterlánc tooyour vágólapra másolása</span><span class="sxs-lookup"><span data-stu-id="e3d98-130">From hello Account, select **Quick Start**, select Java, then copy hello connection string tooyour clipboard</span></span>

2. <span data-ttu-id="e3d98-131">Nyissa meg hello `Program.java` fájlt, cserélje le a hello argumentum toohello MongoClientURI konstruktor hello kapcsolati karakterlánccal.</span><span class="sxs-lookup"><span data-stu-id="e3d98-131">Open hello `Program.java` file, replace hello argument toohello MongoClientURI constructor with hello connection string.</span></span> <span data-ttu-id="e3d98-132">Most már frissítette az alkalmazást az Azure Cosmos DB toocommunicate szükséges összes hello információval.</span><span class="sxs-lookup"><span data-stu-id="e3d98-132">You've now updated your app with all hello info it needs toocommunicate with Azure Cosmos DB.</span></span> 
    
## <a name="run-hello-console-app"></a><span data-ttu-id="e3d98-133">Hello konzol alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="e3d98-133">Run hello console app</span></span>

1. <span data-ttu-id="e3d98-134">Futtatás `mvn package` terminál tooinstall igényelt az npm modult</span><span class="sxs-lookup"><span data-stu-id="e3d98-134">Run `mvn package` in a terminal tooinstall required npm modules</span></span>

2. <span data-ttu-id="e3d98-135">Futtatás `mvn exec:java -D exec.mainClass=GetStarted.Program` a Terminálszolgáltatások toostart a Java-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e3d98-135">Run `mvn exec:java -D exec.mainClass=GetStarted.Program` in a terminal toostart your Java application.</span></span>

<span data-ttu-id="e3d98-136">Ezután már használhatja [Robomongo](mongodb-robomongo.md) / [Studio 3T](mongodb-mongochef.md) tooquery, módosítására és az új adatokkal dolgozni.</span><span class="sxs-lookup"><span data-stu-id="e3d98-136">You can now use [Robomongo](mongodb-robomongo.md) / [Studio 3T](mongodb-mongochef.md) tooquery, modify, and work with this new data.</span></span>

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="e3d98-137">Tekintse át a szolgáltatásiszint-szerződések a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e3d98-137">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="e3d98-138">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="e3d98-138">Clean up resources</span></span>

<span data-ttu-id="e3d98-139">Toocontinue toouse az alkalmazás nem fog, ha törli az összes erőforrást hozta létre a gyors üzembe helyezés hello az Azure-portálon az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e3d98-139">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="e3d98-140">A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson a létrehozott hello erőforrás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="e3d98-140">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="e3d98-141">Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="e3d98-141">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e3d98-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e3d98-142">Next steps</span></span>

<span data-ttu-id="e3d98-143">A gyors üzembe helyezés hogy megtanulta, hogyan toocreate Azure Cosmos DB adatait, hozzon létre egy gyűjteményt hello adatkezelő használatával, és futtassa egy konzolalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e3d98-143">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, create a collection using hello Data Explorer, and run a console app.</span></span> <span data-ttu-id="e3d98-144">További adatok tooyour Cosmos DB fiókot most importálhatja.</span><span class="sxs-lookup"><span data-stu-id="e3d98-144">You can now import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="e3d98-145">MongoDB adatok importálása az Azure Cosmos DB-be</span><span class="sxs-lookup"><span data-stu-id="e3d98-145">Import MongoDB data into Azure Cosmos DB</span></span>](mongodb-migrate.md)


