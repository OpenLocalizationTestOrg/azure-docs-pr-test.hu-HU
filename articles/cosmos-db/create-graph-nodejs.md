---
title: "egy Azure Cosmos DB Node.js-alkalmazás Graph API-jával aaaBuild |} Microsoft Docs"
description: "Megadja a Node.js kódminta tooconnect tooand használható Azure Cosmos DB lekérdezése"
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
ms.date: 07/14/2017
ms.author: denlee
ms.openlocfilehash: 1445755842bc4e4a84ca2b2f789aadde8467e190
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-nodejs-application-by-using-graph-api"></a><span data-ttu-id="44b0d-103">Azure Cosmos DB: Node.js-alkalmazás létrehozása a Graph API-val</span><span class="sxs-lookup"><span data-stu-id="44b0d-103">Azure Cosmos DB: Build a Node.js application by using Graph API</span></span>

<span data-ttu-id="44b0d-104">Azure Cosmos-adatbázis a Microsoft hello globálisan elosztott több modellre adatbázis-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="44b0d-104">Azure Cosmos DB is hello globally distributed multi-model database service from Microsoft.</span></span> <span data-ttu-id="44b0d-105">Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="44b0d-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="44b0d-106">Ez gyors üzembe helyezési a cikk bemutatja, hogyan toocreate egy Azure Cosmos DB fiókot a Graph API-val (előzetes verzió), az adatbázis és a graph hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="44b0d-106">This quick-start article demonstrates how toocreate an Azure Cosmos DB account for Graph API (preview), database, and graph by using hello Azure portal.</span></span> <span data-ttu-id="44b0d-107">Majd létrehozása és futtatása egy konzolalkalmazás hello nyílt forráskódú használatával [Gremlin Node.js](https://www.npmjs.com/package/gremlin-secure) illesztőprogram.</span><span class="sxs-lookup"><span data-stu-id="44b0d-107">You then build and run a console app by using hello open-source [Gremlin Node.js](https://www.npmjs.com/package/gremlin-secure) driver.</span></span>  

> [!NOTE]
> <span data-ttu-id="44b0d-108">hello npm modult `gremlin-secure` egy módosított verziója `gremlin` modul támogatja az SSL és SASL Azure Cosmos DB való csatlakozáshoz szükséges.</span><span class="sxs-lookup"><span data-stu-id="44b0d-108">hello npm module `gremlin-secure` is a modified version of `gremlin` module, with support for SSL and SASL required for connecting with Azure Cosmos DB.</span></span> <span data-ttu-id="44b0d-109">A forráskód elérhető a [GitHubon](https://github.com/CosmosDB/gremlin-javascript).</span><span class="sxs-lookup"><span data-stu-id="44b0d-109">Source code is available on [GitHub](https://github.com/CosmosDB/gremlin-javascript).</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="44b0d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="44b0d-110">Prerequisites</span></span>

<span data-ttu-id="44b0d-111">Ez a minta futtatásához, a következő előfeltételek hello kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="44b0d-111">Before you can run this sample, you must have hello following prerequisites:</span></span>
* <span data-ttu-id="44b0d-112">[Node.js](https://nodejs.org/en/)-verzió: 0.10.29-es vagy újabb</span><span class="sxs-lookup"><span data-stu-id="44b0d-112">[Node.js](https://nodejs.org/en/) version v0.10.29 or later</span></span>
* [<span data-ttu-id="44b0d-113">Git</span><span class="sxs-lookup"><span data-stu-id="44b0d-113">Git</span></span>](http://git-scm.com/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="44b0d-114">Adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="44b0d-114">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a><span data-ttu-id="44b0d-115">Gráf hozzáadása</span><span class="sxs-lookup"><span data-stu-id="44b0d-115">Add a graph</span></span>

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a name="clone-hello-sample-application"></a><span data-ttu-id="44b0d-116">Klónozza a mintaalkalmazást hello</span><span class="sxs-lookup"><span data-stu-id="44b0d-116">Clone hello sample application</span></span>

<span data-ttu-id="44b0d-117">Most tegyük a Githubból, a Klónozás egy grafikonon API app hello kapcsolati karakterlánc beállítása, és futtassa azt.</span><span class="sxs-lookup"><span data-stu-id="44b0d-117">Now let's clone a Graph API app from GitHub, set hello connection string, and run it.</span></span> <span data-ttu-id="44b0d-118">Láthatja, milyen egyszerűen adatokkal toowork programozott módon.</span><span class="sxs-lookup"><span data-stu-id="44b0d-118">You'll see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="44b0d-119">Nyissa meg a Git terminálablakot, például a Git bash eszközt, és módosítsa (keresztül `cd` parancs) tooa munkakönyvtárát.</span><span class="sxs-lookup"><span data-stu-id="44b0d-119">Open a Git terminal window, such as Git Bash, and change (via `cd` command) tooa working directory.</span></span>  

2. <span data-ttu-id="44b0d-120">Futtassa a következő parancs tooclone hello minta tárház hello.</span><span class="sxs-lookup"><span data-stu-id="44b0d-120">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-nodejs-getting-started.git
    ```

3. <span data-ttu-id="44b0d-121">Nyissa meg a hello megoldásfájlt a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="44b0d-121">Open hello solution file in Visual Studio.</span></span> 

## <a name="review-hello-code"></a><span data-ttu-id="44b0d-122">Tekintse át a hello kódot</span><span class="sxs-lookup"><span data-stu-id="44b0d-122">Review hello code</span></span>

<span data-ttu-id="44b0d-123">Most Meggyőződünk arról, mi történik a hello app gyors áttekintése.</span><span class="sxs-lookup"><span data-stu-id="44b0d-123">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="44b0d-124">Nyissa meg hello `app.js` fájlt, és a következő sornyi kód hello találhat.</span><span class="sxs-lookup"><span data-stu-id="44b0d-124">Open hello `app.js` file, and you'll find hello following lines of code.</span></span> 

* <span data-ttu-id="44b0d-125">hello Gremlin ügyfelet jön létre.</span><span class="sxs-lookup"><span data-stu-id="44b0d-125">hello Gremlin client is created.</span></span>

    ```nodejs
    const client = Gremlin.createClient(
        443, 
        config.endpoint, 
        { 
            "session": false, 
            "ssl": true, 
            "user": `/dbs/${config.database}/colls/${config.collection}`,
            "password": config.primaryKey
        });
    ```

  <span data-ttu-id="44b0d-126">hello konfigurációk tartoznak `config.js`, amely azt a szakasz a következő hello szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="44b0d-126">hello configurations are all in `config.js`, which we edit in hello following section.</span></span>

* <span data-ttu-id="44b0d-127">Gremlin lépések egy sorozatát hello hajtja végre `client.execute` metódust.</span><span class="sxs-lookup"><span data-stu-id="44b0d-127">A series of Gremlin steps are executed with hello `client.execute` method.</span></span>

    ```nodejs
    console.log('Running Count'); 
    client.execute("g.V().count()", { }, (err, results) => {
        if (err) return console.error(err);
        console.log(JSON.stringify(results));
        console.log();
    });
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="44b0d-128">A kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="44b0d-128">Update your connection string</span></span>

1. <span data-ttu-id="44b0d-129">Nyissa meg hello config.js fájl.</span><span class="sxs-lookup"><span data-stu-id="44b0d-129">Open hello config.js file.</span></span> 

2. <span data-ttu-id="44b0d-130">A config.js, töltse ki hello config.endpoint kulcsot hello **Gremlin URI** hello értéket **áttekintése** hello Azure-portálon oldalán.</span><span class="sxs-lookup"><span data-stu-id="44b0d-130">In config.js, fill in hello config.endpoint key with hello **Gremlin URI** value from hello **Overview** page of hello Azure portal.</span></span> 

    `config.endpoint = "GRAPHENDPOINT";`

    ![Megtekintése és másolása egy hozzáférési kulcsot a hello Azure-portálon, a kulcsok panelen](./media/create-graph-nodejs/gremlin-uri.png)

   <span data-ttu-id="44b0d-132">Ha hello **Gremlin URI** nincs megadva érték, hello érték generálása hello **kulcsok** hello portálon, hello lap **URI** érték, https:// eltávolítását és módosítását dokumentumok toographs.</span><span class="sxs-lookup"><span data-stu-id="44b0d-132">If hello **Gremlin URI** value is blank, you can generate hello value from hello **Keys** page in hello portal, using hello **URI** value, removing https://, and changing documents toographs.</span></span>

   <span data-ttu-id="44b0d-133">hello Gremlin végpont például kell nélkül hello protokoll/portszámát, amelyet csak hello állomásnév `mygraphdb.graphs.azure.com` (nem `https://mygraphdb.graphs.azure.com` vagy `mygraphdb.graphs.azure.com:433`).</span><span class="sxs-lookup"><span data-stu-id="44b0d-133">hello Gremlin endpoint must be only hello host name without hello protocol/port number, like `mygraphdb.graphs.azure.com` (not `https://mygraphdb.graphs.azure.com` or `mygraphdb.graphs.azure.com:433`).</span></span>

3. <span data-ttu-id="44b0d-134">A config.js, töltse ki kell jelentkezniük az hello hello config.primaryKey érték **elsődleges kulcs** hello értéket **kulcsok** hello Azure-portálon oldalán.</span><span class="sxs-lookup"><span data-stu-id="44b0d-134">In config.js, fill in hello config.primaryKey value in with hello **Primary Key** value from hello **Keys** page of hello Azure portal.</span></span> 

    `config.primaryKey = "PRIMARYKEY";`

   ![hello kulcsok Azure portál panel](./media/create-graph-nodejs/keys.png)

4. <span data-ttu-id="44b0d-136">Adja meg a hello adatbázis nevét, és hello érték config.database és config.collection graph (tároló) nevét.</span><span class="sxs-lookup"><span data-stu-id="44b0d-136">Enter hello database name, and graph (container) name for hello value of config.database and config.collection.</span></span> 

<span data-ttu-id="44b0d-137">Az elkészült config.js fájl olyan lesz, ahogy az alábbi példában látható:</span><span class="sxs-lookup"><span data-stu-id="44b0d-137">Here is an example of what your completed config.js file should look like:</span></span>

```nodejs
var config = {}

// Note that this must not have HTTPS or hello port number
config.endpoint = "testgraphacct.graphs.azure.com";
config.primaryKey = "Pams6e7LEUS7LJ2Qk0fjZf3eGo65JdMWHmyn65i52w8ozPX2oxY3iP0yu05t9v1WymAHNcMwPIqNAEv3XDFsEg==";
config.database = "graphdb"
config.collection = "Persons"

module.exports = config;
```

## <a name="run-hello-console-app"></a><span data-ttu-id="44b0d-138">Hello konzol alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="44b0d-138">Run hello console app</span></span>

1. <span data-ttu-id="44b0d-139">Nyisson meg egy terminálablakot, és módosítsa (keresztül `cd` parancs) toohello telepítési könyvtárában található hello projekt hello package.json fájl.</span><span class="sxs-lookup"><span data-stu-id="44b0d-139">Open a terminal window and change (via `cd` command) toohello installation directory for hello package.json file that's included in hello project.</span></span>  

2. <span data-ttu-id="44b0d-140">Futtatás `npm install` tooinstall hello npm modult, beleértve a szükséges `gremlin-secure`.</span><span class="sxs-lookup"><span data-stu-id="44b0d-140">Run `npm install` tooinstall hello required npm modules, including `gremlin-secure`.</span></span>

3. <span data-ttu-id="44b0d-141">Futtatás `node app.js` a Terminálszolgáltatások toostart a node.js-alkalmazásokban.</span><span class="sxs-lookup"><span data-stu-id="44b0d-141">Run `node app.js` in a terminal toostart your node application.</span></span>

## <a name="browse-with-data-explorer"></a><span data-ttu-id="44b0d-142">Tallózás az Adatkezelővel</span><span class="sxs-lookup"><span data-stu-id="44b0d-142">Browse with Data Explorer</span></span>

<span data-ttu-id="44b0d-143">Ezután lépjen vissza az Azure portál tooview hello Explorer tooData, lekérdezése, módosítása, és az új diagram adatokkal dolgozni.</span><span class="sxs-lookup"><span data-stu-id="44b0d-143">You can now go back tooData Explorer in hello Azure portal tooview, query, modify, and work with your new graph data.</span></span>

<span data-ttu-id="44b0d-144">Az adatok Explorer hello új adatbázis megjelenik hello **diagramjait** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="44b0d-144">In Data Explorer, hello new database appears in hello **Graphs** pane.</span></span> <span data-ttu-id="44b0d-145">Bontsa ki a hello adatbázis hello gyűjtemény követ, majd kattintson az **Graph**.</span><span class="sxs-lookup"><span data-stu-id="44b0d-145">Expand hello database, followed by hello collection, then click **Graph**.</span></span>

<span data-ttu-id="44b0d-146">hello minta alkalmazás által generált hello adatai megjelennének hello következő ablaktábla belül hello **Graph** fülre kattintva **szűrés**.</span><span class="sxs-lookup"><span data-stu-id="44b0d-146">hello data generated by hello sample app is displayed in hello next pane within hello **Graph** tab when you click **Apply Filter**.</span></span>

<span data-ttu-id="44b0d-147">Próbálja befejezése `g.V()` rendelkező `.has('firstName', 'Thomas')` tootest hello szűrő.</span><span class="sxs-lookup"><span data-stu-id="44b0d-147">Try completing `g.V()` with `.has('firstName', 'Thomas')` tootest hello filter.</span></span> <span data-ttu-id="44b0d-148">Vegye figyelembe, hogy hello érték kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="44b0d-148">Do note that hello value is case sensitive.</span></span>

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="44b0d-149">Tekintse át a szolgáltatásiszint-szerződések a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="44b0d-149">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-your-resources"></a><span data-ttu-id="44b0d-150">Az erőforrások törlése</span><span class="sxs-lookup"><span data-stu-id="44b0d-150">Clean up your resources</span></span>

<span data-ttu-id="44b0d-151">Ha nem tervezi meg az alkalmazás használatával toocontinue, törli az összes erőforrást, létrehozott ebben a cikkben hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="44b0d-151">If you do not plan toocontinue using this app, delete all resources that you created in this article by doing hello following:</span></span> 

1. <span data-ttu-id="44b0d-152">Hello Azure-portálon, hello bal oldali navigációs menüjében kattintson **erőforráscsoportok**, majd kattintson a létrehozott hello erőforrás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="44b0d-152">In hello Azure portal, on hello left navigation menu, click **Resource groups**, and then click hello name of hello resource that you created.</span></span> 
2. <span data-ttu-id="44b0d-153">Az erőforrás csoport lapján kattintson a **törlése**, írja be a törölt hello erőforrás toobe hello nevét, és kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="44b0d-153">On your resource group page, click **Delete**, type hello name of hello resource toobe deleted, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="44b0d-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="44b0d-154">Next steps</span></span>

<span data-ttu-id="44b0d-155">A cikkben hogy megismerte hogyan toocreate Azure Cosmos DB adatait, hozzon létre egy grafikonon adatkezelő használatával, és futtassa az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="44b0d-155">In this article, you've learned how toocreate an Azure Cosmos DB account, create a graph by using Data Explorer, and run an app.</span></span> <span data-ttu-id="44b0d-156">Mostantól a Gremlin használatával összetettebb lekérdezéseket is létrehozhat, és hatékony gráfbejárási logikákat implementálhat.</span><span class="sxs-lookup"><span data-stu-id="44b0d-156">You can now build more complex queries and implement powerful graph traversal logic by using Gremlin.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="44b0d-157">Lekérdezés a Gremlin használatával</span><span class="sxs-lookup"><span data-stu-id="44b0d-157">Query using Gremlin</span></span>](tutorial-query-graph.md)
