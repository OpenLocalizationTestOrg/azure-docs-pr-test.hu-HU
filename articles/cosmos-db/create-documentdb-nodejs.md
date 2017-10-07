---
title: "Azure Cosmos DB: Egy Node.js-alkalmazás létrehozása és a DocumentDB API hello |} Microsoft Docs"
description: "Megadja a Node.js kódminta, használhatja a tooconnect tooand lekérdezés hello Azure Cosmos DB DocumentDB API"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 9c0f033c-240e-4fee-8421-08907231087f
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 287d860c7d6f788f05a397b238ef0f841c3c30ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-documentdb-api-app-with-nodejs-and-hello-azure-portal"></a><span data-ttu-id="4b857-103">Azure Cosmos DB: Összeállítása a DocumentDB API-alkalmazást, a Node.js és hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="4b857-103">Azure Cosmos DB: Build a DocumentDB API app with Node.js and hello Azure portal</span></span>

<span data-ttu-id="4b857-104">Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="4b857-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="4b857-105">Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="4b857-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="4b857-106">A gyors üzembe helyezési bemutatja, hogyan toocreate Azure Cosmos DB fiókkal, a dokumentum-adatbázis és gyűjtemény használja hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="4b857-106">This quick start demonstrates how toocreate an Azure Cosmos DB account, document database, and collection using hello Azure portal.</span></span> <span data-ttu-id="4b857-107">Majd létrehozása és futtatása egy konzolalkalmazás hello épülő [DocumentDB Node.js API](documentdb-sdk-node.md).</span><span class="sxs-lookup"><span data-stu-id="4b857-107">You then build and run a console app built on hello [DocumentDB Node.js API](documentdb-sdk-node.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4b857-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4b857-108">Prerequisites</span></span>

* <span data-ttu-id="4b857-109">Ez a minta futtatásához, a következő előfeltételek hello kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="4b857-109">Before you can run this sample, you must have hello following prerequisites:</span></span>
    * <span data-ttu-id="4b857-110">[Node.js](https://nodejs.org/en/)-verzió: 0.10.29-es vagy újabb</span><span class="sxs-lookup"><span data-stu-id="4b857-110">[Node.js](https://nodejs.org/en/) version v0.10.29 or higher</span></span>
    * [<span data-ttu-id="4b857-111">Git</span><span class="sxs-lookup"><span data-stu-id="4b857-111">Git</span></span>](http://git-scm.com/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="4b857-112">Adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="4b857-112">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a><span data-ttu-id="4b857-113">Gyűjtemény hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4b857-113">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-hello-sample-application"></a><span data-ttu-id="4b857-114">Klónozza a mintaalkalmazást hello</span><span class="sxs-lookup"><span data-stu-id="4b857-114">Clone hello sample application</span></span>

<span data-ttu-id="4b857-115">Most tegyük a githubból, klónozás egy DocumentDB API app hello kapcsolati karakterlánc beállítása, és futtassa azt.</span><span class="sxs-lookup"><span data-stu-id="4b857-115">Now let's clone a DocumentDB API app from github, set hello connection string, and run it.</span></span> <span data-ttu-id="4b857-116">Ön meg, milyen egyszerűen adatokkal toowork programozott módon.</span><span class="sxs-lookup"><span data-stu-id="4b857-116">You see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="4b857-117">Nyisson meg egy git terminálablakot, például a git bash eszközt, és `CD` tooa munkakönyvtárát.</span><span class="sxs-lookup"><span data-stu-id="4b857-117">Open a git terminal window, such as git bash, and `CD` tooa working directory.</span></span>  

2. <span data-ttu-id="4b857-118">Futtassa a következő parancs tooclone hello minta tárház hello.</span><span class="sxs-lookup"><span data-stu-id="4b857-118">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-nodejs-getting-started.git
    ```

## <a name="review-hello-code"></a><span data-ttu-id="4b857-119">Tekintse át a hello kódot</span><span class="sxs-lookup"><span data-stu-id="4b857-119">Review hello code</span></span>

<span data-ttu-id="4b857-120">Most Meggyőződünk arról, mi történik a hello app gyors áttekintése.</span><span class="sxs-lookup"><span data-stu-id="4b857-120">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="4b857-121">Nyissa meg hello `app.js` fájlt, és találja, hogy ezek a sorok, a kód létrehozni hello Azure Cosmos DB-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="4b857-121">Open hello `app.js` file and you find that these lines of code create hello Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="4b857-122">Hello `documentClient` inicializálva van.</span><span class="sxs-lookup"><span data-stu-id="4b857-122">hello `documentClient` is initialized.</span></span>

    ```nodejs
    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });
    ```

* <span data-ttu-id="4b857-123">A rendszer létrehozza az új adatbázist.</span><span class="sxs-lookup"><span data-stu-id="4b857-123">A new database is created.</span></span>

    ```nodejs
    client.createDatabase(config.database, (err, created) => {
        if (err) reject(err)
        else resolve(created);
    });
    ```

* <span data-ttu-id="4b857-124">A rendszer létrehozza az új gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="4b857-124">A new collection is created.</span></span>

    ```nodejs
    client.createCollection(databaseUrl, config.collection, { offerThroughput: 400 }, (err, created) => {
        if (err) reject(err)
        else resolve(created);
    });
    ```

* <span data-ttu-id="4b857-125">A rendszer létrehoz néhány dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="4b857-125">Some documents are created.</span></span>

    ```nodejs
    client.createDocument(collectionUrl, document, (err, created) => {
        if (err) reject(err)
        else resolve(created);
    });
    ```

* <span data-ttu-id="4b857-126">A egy SQL-lekérdezést hajt végre a JSON-on.</span><span class="sxs-lookup"><span data-stu-id="4b857-126">A SQL query over JSON is performed.</span></span>

    ```nodejs
    client.queryDocuments(
        collectionUrl,
        'SELECT VALUE r.children FROM root r WHERE r.lastName = "Andersen"'
    ).toArray((err, results) => {
        if (err) reject(err)
        else {
            for (var queryResult of results) {
                let resultString = JSON.stringify(queryResult);
                console.log(`\tQuery returned ${resultString}`);
            }
            console.log();
            resolve(results);
        }
    });
    ```    

## <a name="update-your-connection-string"></a><span data-ttu-id="4b857-127">A kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="4b857-127">Update your connection string</span></span>

<span data-ttu-id="4b857-128">Most lépjen vissza az Azure portál tooget toohello kapcsolati karakterlánc adatainak és hello alkalmazásba másolja.</span><span class="sxs-lookup"><span data-stu-id="4b857-128">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span>

1. <span data-ttu-id="4b857-129">A hello [Azure-portálon](http://portal.azure.com/), az Azure Cosmos DB a fiókot, kattintson a bal oldali navigációs hello **kulcsok**, és kattintson a **írható-olvasható kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="4b857-129">In hello [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in hello left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="4b857-130">Fogjuk hello másolási gombok hello jobb oldalán hello képernyő toocopy hello URI és elsődleges kulcs a hello `config.js` fájl hello következő lépésben.</span><span class="sxs-lookup"><span data-stu-id="4b857-130">You'll use hello copy buttons on hello right side of hello screen toocopy hello URI and Primary Key into hello `config.js` file in hello next step.</span></span>

    ![Megtekintése és másolása egy hozzáférési kulcsot a hello Azure-portálon, a kulcsok panelen](./media/create-documentdb-dotnet/keys.png)

2. <span data-ttu-id="4b857-132">A nyitott hello `config.js` fájlt.</span><span class="sxs-lookup"><span data-stu-id="4b857-132">In Open hello `config.js` file.</span></span> 

3. <span data-ttu-id="4b857-133">Az URI értéket másol a portálról hello (hello Másolás gombra), és teszi hello végpont kulcsának hello `config.js`.</span><span class="sxs-lookup"><span data-stu-id="4b857-133">Copy your URI value from hello portal (using hello copy button) and make it hello value of hello endpoint key in `config.js`.</span></span> 

    `config.endpoint = "https://FILLME.documents.azure.com"`

4. <span data-ttu-id="4b857-134">Ezután másolja az elsődleges kulcs-érték hello portálról, és teszi hello értékének hello `config.primaryKey` a `config.js`.</span><span class="sxs-lookup"><span data-stu-id="4b857-134">Then copy your PRIMARY KEY value from hello portal and make it hello value of hello `config.primaryKey` in `config.js`.</span></span> <span data-ttu-id="4b857-135">Most már frissítette az alkalmazást az Azure Cosmos DB toocommunicate szükséges összes hello információval.</span><span class="sxs-lookup"><span data-stu-id="4b857-135">You've now updated your app with all hello info it needs toocommunicate with Azure Cosmos DB.</span></span> 

    `config.primaryKey "FILLME"`
    
## <a name="run-hello-app"></a><span data-ttu-id="4b857-136">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="4b857-136">Run hello app</span></span>
1. <span data-ttu-id="4b857-137">Futtatás `npm install` terminál tooinstall igényelt az npm modult</span><span class="sxs-lookup"><span data-stu-id="4b857-137">Run `npm install` in a terminal tooinstall required npm modules</span></span>

2. <span data-ttu-id="4b857-138">Futtatás `node app.js` a Terminálszolgáltatások toostart a node.js-alkalmazásokban.</span><span class="sxs-lookup"><span data-stu-id="4b857-138">Run `node app.js` in a terminal toostart your node application.</span></span>

<span data-ttu-id="4b857-139">Mostantól tooData Explorer visszaléphet, és tekintse meg a lekérdezés, módosítása, és ezekkel az új adatokkal.</span><span class="sxs-lookup"><span data-stu-id="4b857-139">You can now go back tooData Explorer and see query, modify, and work with this new data.</span></span> 

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="4b857-140">Tekintse át a szolgáltatásiszint-szerződések a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="4b857-140">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="4b857-141">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="4b857-141">Clean up resources</span></span>

<span data-ttu-id="4b857-142">Toocontinue toouse az alkalmazás nem fog, ha törli az összes erőforrást hozta létre a gyors üzembe helyezés hello az Azure-portálon az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4b857-142">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="4b857-143">A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson a létrehozott hello erőforrás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="4b857-143">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="4b857-144">Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="4b857-144">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4b857-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4b857-145">Next steps</span></span>

<span data-ttu-id="4b857-146">A gyors üzembe helyezés hogy megtanulta, hogyan toocreate Azure Cosmos DB adatait, hozzon létre egy gyűjteményt hello adatkezelő használatával, és futtassa az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4b857-146">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, create a collection using hello Data Explorer, and run an app.</span></span> <span data-ttu-id="4b857-147">További adatok tooyour Cosmos DB fiókot most importálhatja.</span><span class="sxs-lookup"><span data-stu-id="4b857-147">You can now import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="4b857-148">Adatok importálása az Azure Cosmos DB-be</span><span class="sxs-lookup"><span data-stu-id="4b857-148">Import data into Azure Cosmos DB</span></span>](import-data.md)


