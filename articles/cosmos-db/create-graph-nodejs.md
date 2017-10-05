---
title: "Azure Cosmos DB-alapú Node.js-alkalmazás létrehozása a Graph API-val | Microsoft Docs"
description: "A cikk egy Node.js-kódmintát mutat be, amellyel csatlakozhat egy Azure Cosmos DB-adatbázishoz, és lekérdezéseket hajthat végre"
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
ms.openlocfilehash: 6d14719938af0ce825955389824441e111024869
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-cosmos-db-build-a-nodejs-application-by-using-graph-api"></a><span data-ttu-id="cd581-103">Azure Cosmos DB: Node.js-alkalmazás létrehozása a Graph API-val</span><span class="sxs-lookup"><span data-stu-id="cd581-103">Azure Cosmos DB: Build a Node.js application by using Graph API</span></span>

<span data-ttu-id="cd581-104">Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="cd581-104">Azure Cosmos DB is the globally distributed multi-model database service from Microsoft.</span></span> <span data-ttu-id="cd581-105">Segítségével gyorsan létrehozhat és lekérdezhet dokumentum-, kulcs/érték és gráf típusú adatbázisokat, melyek mindegyike felhasználja az Azure Cosmos DB középpontjában álló globális elosztási és horizontális skálázhatósági képességeket.</span><span class="sxs-lookup"><span data-stu-id="cd581-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="cd581-106">Ez a rövid útmutató bemutatja, hogy az Azure Portal segítségével hogyan hozhat létre Azure Cosmos DB-fiókot az előzetes verzióként elérhető Graph API-hoz, valamint adatbázist és gráfot.</span><span class="sxs-lookup"><span data-stu-id="cd581-106">This quick-start article demonstrates how to create an Azure Cosmos DB account for Graph API (preview), database, and graph by using the Azure portal.</span></span> <span data-ttu-id="cd581-107">Ezután a nyílt forráskódú [Gremlin Node.js](https://www.npmjs.com/package/gremlin-secure)-illesztőprogram segítségével létrehozhat és futtathat egy konzolalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="cd581-107">You then build and run a console app by using the open-source [Gremlin Node.js](https://www.npmjs.com/package/gremlin-secure) driver.</span></span>  

> [!NOTE]
> <span data-ttu-id="cd581-108">A `gremlin-secure` npm-modul a `gremlin` modul módosított verziója, amely támogatja az Azure Cosmos DB-hez történő csatlakozáshoz szükséges SSL és SASL protokollt.</span><span class="sxs-lookup"><span data-stu-id="cd581-108">The npm module `gremlin-secure` is a modified version of `gremlin` module, with support for SSL and SASL required for connecting with Azure Cosmos DB.</span></span> <span data-ttu-id="cd581-109">A forráskód elérhető a [GitHubon](https://github.com/CosmosDB/gremlin-javascript).</span><span class="sxs-lookup"><span data-stu-id="cd581-109">Source code is available on [GitHub](https://github.com/CosmosDB/gremlin-javascript).</span></span>
>

## <a name="prerequisites"></a><span data-ttu-id="cd581-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cd581-110">Prerequisites</span></span>

<span data-ttu-id="cd581-111">Mielőtt futtathatná ezt a mintát, rendelkeznie kell a következő előfeltételekkel:</span><span class="sxs-lookup"><span data-stu-id="cd581-111">Before you can run this sample, you must have the following prerequisites:</span></span>
* <span data-ttu-id="cd581-112">[Node.js](https://nodejs.org/en/)-verzió: 0.10.29-es vagy újabb</span><span class="sxs-lookup"><span data-stu-id="cd581-112">[Node.js](https://nodejs.org/en/) version v0.10.29 or later</span></span>
* [<span data-ttu-id="cd581-113">Git</span><span class="sxs-lookup"><span data-stu-id="cd581-113">Git</span></span>](http://git-scm.com/)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="cd581-114">Adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="cd581-114">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a><span data-ttu-id="cd581-115">Gráf hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cd581-115">Add a graph</span></span>

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a name="clone-the-sample-application"></a><span data-ttu-id="cd581-116">A mintaalkalmazás klónozása</span><span class="sxs-lookup"><span data-stu-id="cd581-116">Clone the sample application</span></span>

<span data-ttu-id="cd581-117">A következő lépésekben elvégezheti a Graph API-alkalmazás klónozását a GitHubról, beállíthatja a kapcsolati sztringet, és futtathatja az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="cd581-117">Now let's clone a Graph API app from GitHub, set the connection string, and run it.</span></span> <span data-ttu-id="cd581-118">Látni fogja, milyen egyszerű az adatokkal programozott módon dolgozni.</span><span class="sxs-lookup"><span data-stu-id="cd581-118">You'll see how easy it is to work with data programmatically.</span></span> 

1. <span data-ttu-id="cd581-119">Nyisson meg egy Git-terminálablakot, például a Git Bash eszközt, és a `cd` paranccsal váltson egy munkakönyvtárra.</span><span class="sxs-lookup"><span data-stu-id="cd581-119">Open a Git terminal window, such as Git Bash, and change (via `cd` command) to a working directory.</span></span>  

2. <span data-ttu-id="cd581-120">Futtassa a következő parancsot a mintatárház klónozásához.</span><span class="sxs-lookup"><span data-stu-id="cd581-120">Run the following command to clone the sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-nodejs-getting-started.git
    ```

3. <span data-ttu-id="cd581-121">Nyissa meg a megoldásfájlt a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="cd581-121">Open the solution file in Visual Studio.</span></span> 

## <a name="review-the-code"></a><span data-ttu-id="cd581-122">A kód áttekintése</span><span class="sxs-lookup"><span data-stu-id="cd581-122">Review the code</span></span>

<span data-ttu-id="cd581-123">Tekintsük át, hogy mi történik az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="cd581-123">Let's make a quick review of what's happening in the app.</span></span> <span data-ttu-id="cd581-124">Nyissa meg az `app.js` fájlt, amelyben a következő kódsorokat találja.</span><span class="sxs-lookup"><span data-stu-id="cd581-124">Open the `app.js` file, and you'll find the following lines of code.</span></span> 

* <span data-ttu-id="cd581-125">Létrejön a Gremlin-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="cd581-125">The Gremlin client is created.</span></span>

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

  <span data-ttu-id="cd581-126">Minden konfiguráció a `config.js` fájlban van, amelynek a szerkesztését a következő rész írja le.</span><span class="sxs-lookup"><span data-stu-id="cd581-126">The configurations are all in `config.js`, which we edit in the following section.</span></span>

* <span data-ttu-id="cd581-127">A `client.execute` metódussal a program Gremlin-lépések sorozatát hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="cd581-127">A series of Gremlin steps are executed with the `client.execute` method.</span></span>

    ```nodejs
    console.log('Running Count'); 
    client.execute("g.V().count()", { }, (err, results) => {
        if (err) return console.error(err);
        console.log(JSON.stringify(results));
        console.log();
    });
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="cd581-128">A kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="cd581-128">Update your connection string</span></span>

1. <span data-ttu-id="cd581-129">Nyissa meg a config.js fájlt.</span><span class="sxs-lookup"><span data-stu-id="cd581-129">Open the config.js file.</span></span> 

2. <span data-ttu-id="cd581-130">A config.js fájlban adja meg a config.endpoint kulcsot az Azure Portal **Áttekintés** lapjáról származó **Gremlin URI** értékkel.</span><span class="sxs-lookup"><span data-stu-id="cd581-130">In config.js, fill in the config.endpoint key with the **Gremlin URI** value from the **Overview** page of the Azure portal.</span></span> 

    `config.endpoint = "GRAPHENDPOINT";`

    ![Hozzáférési kulcs megtekintése és másolása az Azure Portal kulcsok paneljén](./media/create-graph-nodejs/gremlin-uri.png)

   <span data-ttu-id="cd581-132">Ha a **Gremlin URI** érték üres, létrehozhatja az értéket a portál **Kulcsok** oldalán az **URI** értékkel a https:// előtag eltávolításával és a dokumentumok gráfokká módosításával.</span><span class="sxs-lookup"><span data-stu-id="cd581-132">If the **Gremlin URI** value is blank, you can generate the value from the **Keys** page in the portal, using the **URI** value, removing https://, and changing documents to graphs.</span></span>

   <span data-ttu-id="cd581-133">A Gremlin-végpont csak protokoll/portszám nélküli gazdagépnév lehet, például `mygraphdb.graphs.azure.com` (és nem `https://mygraphdb.graphs.azure.com` vagy `mygraphdb.graphs.azure.com:433`).</span><span class="sxs-lookup"><span data-stu-id="cd581-133">The Gremlin endpoint must be only the host name without the protocol/port number, like `mygraphdb.graphs.azure.com` (not `https://mygraphdb.graphs.azure.com` or `mygraphdb.graphs.azure.com:433`).</span></span>

3. <span data-ttu-id="cd581-134">A config.js fájlban adja meg a config.primaryKey értéket az Azure Portal **Kulcsok** oldaláról származó **Elsődleges kulcs** értékkel.</span><span class="sxs-lookup"><span data-stu-id="cd581-134">In config.js, fill in the config.primaryKey value in with the **Primary Key** value from the **Keys** page of the Azure portal.</span></span> 

    `config.primaryKey = "PRIMARYKEY";`

   ![Kulcsok panel az Azure Portalon](./media/create-graph-nodejs/keys.png)

4. <span data-ttu-id="cd581-136">A config.database és a config.collection értékéhez adja meg az adatbázis és a gráf (tároló) nevét.</span><span class="sxs-lookup"><span data-stu-id="cd581-136">Enter the database name, and graph (container) name for the value of config.database and config.collection.</span></span> 

<span data-ttu-id="cd581-137">Az elkészült config.js fájl olyan lesz, ahogy az alábbi példában látható:</span><span class="sxs-lookup"><span data-stu-id="cd581-137">Here is an example of what your completed config.js file should look like:</span></span>

```nodejs
var config = {}

// Note that this must not have HTTPS or the port number
config.endpoint = "testgraphacct.graphs.azure.com";
config.primaryKey = "Pams6e7LEUS7LJ2Qk0fjZf3eGo65JdMWHmyn65i52w8ozPX2oxY3iP0yu05t9v1WymAHNcMwPIqNAEv3XDFsEg==";
config.database = "graphdb"
config.collection = "Persons"

module.exports = config;
```

## <a name="run-the-console-app"></a><span data-ttu-id="cd581-138">A konzolalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="cd581-138">Run the console app</span></span>

1. <span data-ttu-id="cd581-139">Nyisson meg egy terminálablakot, és a `cd` parancs használatával váltson a projektben lévő package.json fájl telepítési könyvtárára.</span><span class="sxs-lookup"><span data-stu-id="cd581-139">Open a terminal window and change (via `cd` command) to the installation directory for the package.json file that's included in the project.</span></span>  

2. <span data-ttu-id="cd581-140">Futtassa az `npm install` parancsot a szükséges npm-modulok (köztük a `gremlin-secure`) telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="cd581-140">Run `npm install` to install the required npm modules, including `gremlin-secure`.</span></span>

3. <span data-ttu-id="cd581-141">Futtassa a `node app.js` parancsot egy terminálban a node-alkalmazás elindításához.</span><span class="sxs-lookup"><span data-stu-id="cd581-141">Run `node app.js` in a terminal to start your node application.</span></span>

## <a name="browse-with-data-explorer"></a><span data-ttu-id="cd581-142">Tallózás az Adatkezelővel</span><span class="sxs-lookup"><span data-stu-id="cd581-142">Browse with Data Explorer</span></span>

<span data-ttu-id="cd581-143">Most visszaléphet az Adatkezelőbe az Azure Portalon, és megtekintheti, lekérdezheti, módosíthatja, és használatba veheti az új gráfadatokat.</span><span class="sxs-lookup"><span data-stu-id="cd581-143">You can now go back to Data Explorer in the Azure portal to view, query, modify, and work with your new graph data.</span></span>

<span data-ttu-id="cd581-144">Az Adatkezelőben az új adatbázis a **Gráfok** ablaktáblán jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="cd581-144">In Data Explorer, the new database appears in the **Graphs** pane.</span></span> <span data-ttu-id="cd581-145">Bontsa ki az adatbázist, majd a gyűjteményt, és kattintson a **Gráf** elemre.</span><span class="sxs-lookup"><span data-stu-id="cd581-145">Expand the database, followed by the collection, then click **Graph**.</span></span>

<span data-ttu-id="cd581-146">A mintaalkalmazás által létrehozott adatok a **Gráf** lap következő panelén jelennek meg, amikor a **Szűrő alkalmazása** gombra kattint.</span><span class="sxs-lookup"><span data-stu-id="cd581-146">The data generated by the sample app is displayed in the next pane within the **Graph** tab when you click **Apply Filter**.</span></span>

<span data-ttu-id="cd581-147">A szűrő teszteléséhez hajtsa végre a `g.V()` függvényt a következővel: `.has('firstName', 'Thomas')`.</span><span class="sxs-lookup"><span data-stu-id="cd581-147">Try completing `g.V()` with `.has('firstName', 'Thomas')` to test the filter.</span></span> <span data-ttu-id="cd581-148">Vegye figyelembe, hogy ez az érték megkülönbözteti a kis- és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="cd581-148">Do note that the value is case sensitive.</span></span>

## <a name="review-slas-in-the-azure-portal"></a><span data-ttu-id="cd581-149">Az SLA-k áttekintése az Azure Portalon</span><span class="sxs-lookup"><span data-stu-id="cd581-149">Review SLAs in the Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-your-resources"></a><span data-ttu-id="cd581-150">Az erőforrások törlése</span><span class="sxs-lookup"><span data-stu-id="cd581-150">Clean up your resources</span></span>

<span data-ttu-id="cd581-151">Ha nem tervezi az alkalmazás további használatát, törölje a cikkben létrehozott összes erőforrást a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="cd581-151">If you do not plan to continue using this app, delete all resources that you created in this article by doing the following:</span></span> 

1. <span data-ttu-id="cd581-152">Az Azure Portal bal oldali navigációs menüjében kattintson az **Erőforráscsoportok** lehetőségre, majd a létrehozott erőforrás nevére.</span><span class="sxs-lookup"><span data-stu-id="cd581-152">In the Azure portal, on the left navigation menu, click **Resource groups**, and then click the name of the resource that you created.</span></span> 
2. <span data-ttu-id="cd581-153">A saját erőforráscsoport oldalán kattintson a **Törlés** elemre, írja be a törölni kívánt erőforrás nevét, majd kattintson a **Törlés** elemre.</span><span class="sxs-lookup"><span data-stu-id="cd581-153">On your resource group page, click **Delete**, type the name of the resource to be deleted, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cd581-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cd581-154">Next steps</span></span>

<span data-ttu-id="cd581-155">Ez a cikk bemutatta, hogyan lehet létrehozni egy Azure Cosmos DB-fiókot, létrehozni egy gráfot az Adatkezelő használatával, és futtatni egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="cd581-155">In this article, you've learned how to create an Azure Cosmos DB account, create a graph by using Data Explorer, and run an app.</span></span> <span data-ttu-id="cd581-156">Mostantól a Gremlin használatával összetettebb lekérdezéseket is létrehozhat, és hatékony gráfbejárási logikákat implementálhat.</span><span class="sxs-lookup"><span data-stu-id="cd581-156">You can now build more complex queries and implement powerful graph traversal logic by using Gremlin.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="cd581-157">Lekérdezés a Gremlin használatával</span><span class="sxs-lookup"><span data-stu-id="cd581-157">Query using Gremlin</span></span>](tutorial-query-graph.md)
