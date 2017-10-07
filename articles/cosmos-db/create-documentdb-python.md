---
title: "Azure Cosmos DB: Hozza létre egy alkalmazást a Python, és a DocumentDB API hello |} Microsoft Docs"
description: "Megadja a Python kódminta, használhatja a tooconnect tooand lekérdezés hello Azure Cosmos DB DocumentDB API"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 51c11be2-af6d-425f-a86a-39cbfe61da29
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: hero-article
ms.date: 05/13/2017
ms.author: mimig
ms.openlocfilehash: e66965ab493c6ef693e88a3767a401d39e1bde2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-documentdb-api-app-with-python-and-hello-azure-portal"></a><span data-ttu-id="1af95-103">Azure Cosmos-adatbázis: A Python egy DocumentDB API-alkalmazást létrehozni, és hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1af95-103">Azure Cosmos DB: Build a DocumentDB API app with Python and hello Azure portal</span></span>

<span data-ttu-id="1af95-104">Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="1af95-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="1af95-105">Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="1af95-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="1af95-106">A gyors üzembe helyezési bemutatja, hogyan toocreate Azure Cosmos DB fiókkal, a dokumentum-adatbázis és gyűjtemény használja hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="1af95-106">This quick start demonstrates how toocreate an Azure Cosmos DB account, document database, and collection using hello Azure portal.</span></span> <span data-ttu-id="1af95-107">Majd létrehozása és futtatása egy konzolalkalmazás hello épülő [DocumentDB Python API](documentdb-sdk-python.md).</span><span class="sxs-lookup"><span data-stu-id="1af95-107">You then build and run a console app built on hello [DocumentDB Python API](documentdb-sdk-python.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1af95-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1af95-108">Prerequisites</span></span>

* <span data-ttu-id="1af95-109">Ez a minta futtatásához, a következő előfeltételek hello kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="1af95-109">Before you can run this sample, you must have hello following prerequisites:</span></span>
    * <span data-ttu-id="1af95-110">[Visual Studio 2015](http://www.visualstudio.com/) vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="1af95-110">[Visual Studio 2015](http://www.visualstudio.com/) or higher.</span></span>
    * <span data-ttu-id="1af95-111">Python Tools for Visual Studio, amely beszerezhető a [GitHubról](http://microsoft.github.io/PTVS/).</span><span class="sxs-lookup"><span data-stu-id="1af95-111">Python Tools for Visual Studio from [GitHub](http://microsoft.github.io/PTVS/).</span></span> <span data-ttu-id="1af95-112">Ez az oktatóanyag a Python Tools VS 2015-ös verziót használja.</span><span class="sxs-lookup"><span data-stu-id="1af95-112">This tutorial uses Python Tools for VS 2015.</span></span>
    * <span data-ttu-id="1af95-113">A [python.org](https://www.python.org/downloads/release/python-2712/) webhelyen elérhető Python 2.7-es verzió</span><span class="sxs-lookup"><span data-stu-id="1af95-113">Python 2.7 from [python.org](https://www.python.org/downloads/release/python-2712/)</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="1af95-114">Adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="1af95-114">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a><span data-ttu-id="1af95-115">Gyűjtemény hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1af95-115">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-hello-sample-application"></a><span data-ttu-id="1af95-116">Klónozza a mintaalkalmazást hello</span><span class="sxs-lookup"><span data-stu-id="1af95-116">Clone hello sample application</span></span>

<span data-ttu-id="1af95-117">Most tegyük a githubból, klónozás egy DocumentDB API app hello kapcsolati karakterlánc beállítása, és futtassa azt.</span><span class="sxs-lookup"><span data-stu-id="1af95-117">Now let's clone a DocumentDB API app from github, set hello connection string, and run it.</span></span> <span data-ttu-id="1af95-118">Ön meg, milyen egyszerűen adatokkal toowork programozott módon.</span><span class="sxs-lookup"><span data-stu-id="1af95-118">You see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="1af95-119">Nyisson meg egy git terminálablakot, például a git bash eszközt, és `cd` tooa munkakönyvtárát.</span><span class="sxs-lookup"><span data-stu-id="1af95-119">Open a git terminal window, such as git bash, and `cd` tooa working directory.</span></span>  

2. <span data-ttu-id="1af95-120">Futtassa a következő parancs tooclone hello minta tárház hello.</span><span class="sxs-lookup"><span data-stu-id="1af95-120">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-python-getting-started.git
    ```  
## <a name="review-hello-code"></a><span data-ttu-id="1af95-121">Tekintse át a hello kódot</span><span class="sxs-lookup"><span data-stu-id="1af95-121">Review hello code</span></span>

<span data-ttu-id="1af95-122">Most Meggyőződünk arról, mi történik a hello app gyors áttekintése.</span><span class="sxs-lookup"><span data-stu-id="1af95-122">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="1af95-123">Nyissa meg hello DocumentDBGetStarted.py fájlt, és, hogy ezek a sorok, a kód létrehozása hello Azure Cosmos DB erőforrások találhat.</span><span class="sxs-lookup"><span data-stu-id="1af95-123">Open hello DocumentDBGetStarted.py file and you'll find that these lines of code create hello Azure Cosmos DB resources.</span></span> 


* <span data-ttu-id="1af95-124">hello DocumentClient inicializálva van.</span><span class="sxs-lookup"><span data-stu-id="1af95-124">hello DocumentClient is initialized.</span></span>

    ```python
    # Initialize hello Python DocumentDB client
    client = document_client.DocumentClient(config['ENDPOINT'], {'masterKey': config['MASTERKEY']})
    ```

* <span data-ttu-id="1af95-125">A rendszer létrehozza az új adatbázist.</span><span class="sxs-lookup"><span data-stu-id="1af95-125">A new database is created.</span></span>

    ```python
    # Create a database
    db = client.CreateDatabase({ 'id': config['DOCUMENTDB_DATABASE'] })
    ```

* <span data-ttu-id="1af95-126">A rendszer létrehozza az új gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="1af95-126">A new collection is created.</span></span>

    ```python
    # Create collection options
    options = {
        'offerEnableRUPerMinuteThroughput': True,
        'offerVersion': "V2",
        'offerThroughput': 400
    }

    # Create a collection
    collection = client.CreateCollection(db['_self'], { 'id': config['DOCUMENTDB_COLLECTION'] }, options)
    ```

* <span data-ttu-id="1af95-127">A rendszer létrehoz néhány dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="1af95-127">Some documents are created.</span></span>

    ```python
    # Create some documents
    document1 = client.CreateDocument(collection['_self'],
        { 
            'id': 'server1',
            'Web Site': 0,
            'Cloud Service': 0,
            'Virtual Machine': 0,
            'name': 'some' 
        })
    ```

* <span data-ttu-id="1af95-128">A rendszer végrehajt egy lekérdezést az SQL használatával</span><span class="sxs-lookup"><span data-stu-id="1af95-128">A query is performed using SQL</span></span>

    ```python
    # Query them in SQL
    query = { 'query': 'SELECT * FROM server s' }    
            
    options = {} 
    options['enableCrossPartitionQuery'] = True
    options['maxItemCount'] = 2

    result_iterable = client.QueryDocuments(collection['_self'], query, options)
    results = list(result_iterable);

    print(results)
    ```

## <a name="update-your-connection-string"></a><span data-ttu-id="1af95-129">A kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="1af95-129">Update your connection string</span></span>

<span data-ttu-id="1af95-130">Most lépjen vissza az Azure portál tooget toohello kapcsolati karakterlánc adatainak és hello alkalmazásba másolja.</span><span class="sxs-lookup"><span data-stu-id="1af95-130">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span>

1. <span data-ttu-id="1af95-131">A hello [Azure-portálon](http://portal.azure.com/), az Azure Cosmos DB a fiókot, kattintson a bal oldali navigációs hello **kulcsok**, és kattintson a **írható-olvasható kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="1af95-131">In hello [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in hello left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="1af95-132">Fogjuk hello másolási gombok hello jobb oldalán hello képernyő toocopy hello URI és elsődleges kulcs a hello `DocumentDBGetStarted.py` fájl hello következő lépésben.</span><span class="sxs-lookup"><span data-stu-id="1af95-132">You'll use hello copy buttons on hello right side of hello screen toocopy hello URI and Primary Key into hello `DocumentDBGetStarted.py` file in hello next step.</span></span>

    ![Megtekintése és másolása egy hozzáférési kulcsot a hello Azure-portálon, a kulcsok panelen](./media/create-documentdb-dotnet/keys.png)

2. <span data-ttu-id="1af95-134">A nyitott hello `DocumentDBGetStarted.py` fájlt.</span><span class="sxs-lookup"><span data-stu-id="1af95-134">In Open hello `DocumentDBGetStarted.py` file.</span></span> 

3. <span data-ttu-id="1af95-135">Az URI értéket másol a portálról hello (hello Másolás gombra), és teszi hello végpont kulcsának hello `DocumentDBGetStarted.py`.</span><span class="sxs-lookup"><span data-stu-id="1af95-135">Copy your URI value from hello portal (using hello copy button) and make it hello value of hello endpoint key in `DocumentDBGetStarted.py`.</span></span> 

    `config.ENDPOINT : "https://FILLME.documents.azure.com"`

4. <span data-ttu-id="1af95-136">Ezután másolja az elsődleges kulcs-érték hello portálról, és teszi hello értékének hello `config.MASTERKEY` a `DocumentDBGetStarted.py`.</span><span class="sxs-lookup"><span data-stu-id="1af95-136">Then copy your PRIMARY KEY value from hello portal and make it hello value of hello `config.MASTERKEY` in `DocumentDBGetStarted.py`.</span></span> <span data-ttu-id="1af95-137">Most már frissítette az alkalmazást az Azure Cosmos DB toocommunicate szükséges összes hello információval.</span><span class="sxs-lookup"><span data-stu-id="1af95-137">You've now updated your app with all hello info it needs toocommunicate with Azure Cosmos DB.</span></span> 

    `config.MASTERKEY : "FILLME"`
    
## <a name="run-hello-app"></a><span data-ttu-id="1af95-138">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="1af95-138">Run hello app</span></span>
1. <span data-ttu-id="1af95-139">A Visual Studióban, kattintson a jobb gombbal a hello projekt **Megoldáskezelőben**, válassza ki a jelenlegi környezet hello a Python, majd kattintson a jobb gombbal.</span><span class="sxs-lookup"><span data-stu-id="1af95-139">In Visual Studio, right-click on hello project in **Solution Explorer**, select hello current Python environment, then right click.</span></span>

2. <span data-ttu-id="1af95-140">Válassza ki a Python-csomag telepítése lehetőséget, majd írja be a következőt: **pydocumentdb**</span><span class="sxs-lookup"><span data-stu-id="1af95-140">Select Install Python Package, then type in **pydocumentdb**</span></span>

3. <span data-ttu-id="1af95-141">F5 toorun hello alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="1af95-141">Run F5 toorun hello application.</span></span> <span data-ttu-id="1af95-142">Az alkalmazás megjelenik a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="1af95-142">Your app displays in your browser.</span></span> 

<span data-ttu-id="1af95-143">Mostantól tooData Explorer visszaléphet, és tekintse meg a lekérdezés, módosítása, és ezekkel az új adatokkal.</span><span class="sxs-lookup"><span data-stu-id="1af95-143">You can now go back tooData Explorer and see query, modify, and work with this new data.</span></span> 

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="1af95-144">Tekintse át a szolgáltatásiszint-szerződések a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1af95-144">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="1af95-145">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="1af95-145">Clean up resources</span></span>

<span data-ttu-id="1af95-146">Toocontinue toouse az alkalmazás nem fog, ha törli az összes erőforrást hozta létre a gyors üzembe helyezés hello az Azure-portálon az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="1af95-146">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="1af95-147">A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson a létrehozott hello erőforrás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="1af95-147">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="1af95-148">Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="1af95-148">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1af95-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1af95-149">Next steps</span></span>

<span data-ttu-id="1af95-150">A gyors üzembe helyezés hogy megtanulta, hogyan toocreate Azure Cosmos DB adatait, hozzon létre egy gyűjteményt hello adatkezelő használatával, és futtassa az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1af95-150">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, create a collection using hello Data Explorer, and run an app.</span></span> <span data-ttu-id="1af95-151">További adatok tooyour Cosmos DB fiókot most importálhatja.</span><span class="sxs-lookup"><span data-stu-id="1af95-151">You can now import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="1af95-152">A DocumentDB API hello Azure Cosmos DB adatok importálása</span><span class="sxs-lookup"><span data-stu-id="1af95-152">Import data into Azure Cosmos DB for hello DocumentDB API</span></span>](import-data.md)


