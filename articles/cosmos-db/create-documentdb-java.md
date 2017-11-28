---
title: "egy Azure Cosmos DB dokumentum adatbázist, Java aaaCreate |} Microsoft dokumentumok |} Microsoft Docs"
description: "Megadja a Java kódminta, használhatja a tooconnect tooand lekérdezés hello Azure Cosmos DB DocumentDB API"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 89ea62bb-c620-46d5-baa0-eefd9888557c
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 08/02/2017
ms.author: mimig
ms.openlocfilehash: 400c9e7780034d3e28d749e734786e950edad22f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-create-a-document-database-using-java-and-hello-azure-portal"></a><span data-ttu-id="b371e-103">Azure Cosmos DB: Hozzon létre egy dokumentum adatbázist, Java használatával, és hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="b371e-103">Azure Cosmos DB: Create a document database using Java and hello Azure portal</span></span>

<span data-ttu-id="b371e-104">Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="b371e-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="b371e-105">Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="b371e-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="b371e-106">A gyors üzembe helyezés dokumentum létrehozása az Azure portál eszközök adatbázis használatával hello Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="b371e-106">This quickstart creates a document database using hello Azure portal tools for Azure Cosmos DB.</span></span> <span data-ttu-id="b371e-107">A gyors üzembe helyezés azt is bemutatja, hogyan tooquickly hozzon létre egy Java-Konzolalkalmazás használatával hello [DocumentDB Java API](documentdb-sdk-java.md).</span><span class="sxs-lookup"><span data-stu-id="b371e-107">This quickstart also shows you how tooquickly create a Java console app using hello [DocumentDB Java API](documentdb-sdk-java.md).</span></span> <span data-ttu-id="b371e-108">hello utasításait a gyors üzembe helyezés követhetők bármely operációs rendszeren, amely alkalmas a Java futtatására.</span><span class="sxs-lookup"><span data-stu-id="b371e-108">hello instructions in this quickstart can be followed on any operating system that is capable of running Java.</span></span> <span data-ttu-id="b371e-109">A gyors üzembe helyezés befejezése lesz ismeri a létrehozása és módosítása a dokumentum adatbázis erőforrásainak vagy hello felhasználói felületén, vagy programozottan, amelyik igény szerint.</span><span class="sxs-lookup"><span data-stu-id="b371e-109">By completing this quickstart you'll be familiar with creating and modifying document database resources in either hello UI or programatically, whichever is your preference.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b371e-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b371e-110">Prerequisites</span></span>

* [<span data-ttu-id="b371e-111">Java fejlesztői készlet (JDK) 1.7+</span><span class="sxs-lookup"><span data-stu-id="b371e-111">Java Development Kit (JDK) 1.7+</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * <span data-ttu-id="b371e-112">Ubuntu, futtassa `apt-get install default-jdk` tooinstall hello JDK.</span><span class="sxs-lookup"><span data-stu-id="b371e-112">On Ubuntu, run `apt-get install default-jdk` tooinstall hello JDK.</span></span>
    * <span data-ttu-id="b371e-113">Lehet, hogy tooset hello JAVA_HOME környezeti változó toopoint toohello mappa hello JDK futtató.</span><span class="sxs-lookup"><span data-stu-id="b371e-113">Be sure tooset hello JAVA_HOME environment variable toopoint toohello folder where hello JDK is installed.</span></span>
* <span data-ttu-id="b371e-114">[Maven](http://maven.apache.org/download.cgi) bináris archívum [letöltése](http://maven.apache.org/install.html) és [telepítése](http://maven.apache.org/)</span><span class="sxs-lookup"><span data-stu-id="b371e-114">[Download](http://maven.apache.org/download.cgi) and [install](http://maven.apache.org/install.html) a [Maven](http://maven.apache.org/) binary archive</span></span>
    * <span data-ttu-id="b371e-115">Ubuntu, futtathatja `apt-get install maven` tooinstall Maven.</span><span class="sxs-lookup"><span data-stu-id="b371e-115">On Ubuntu, you can run `apt-get install maven` tooinstall Maven.</span></span>
* [<span data-ttu-id="b371e-116">Git</span><span class="sxs-lookup"><span data-stu-id="b371e-116">Git</span></span>](https://www.git-scm.com/)
    * <span data-ttu-id="b371e-117">Ubuntu, futtathatja `sudo apt-get install git` tooinstall Git.</span><span class="sxs-lookup"><span data-stu-id="b371e-117">On Ubuntu, you can run `sudo apt-get install git` tooinstall Git.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="b371e-118">Adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="b371e-118">Create a database account</span></span>

<span data-ttu-id="b371e-119">A dokumentum-adatbázis létrehozása előtt kell toocreate egy Cosmos-DB Azure SQL (DocumentDB) adatbázis fiókja.</span><span class="sxs-lookup"><span data-stu-id="b371e-119">Before you can create a document database, you need toocreate a SQL (DocumentDB) database account with Azure Cosmos DB.</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a><span data-ttu-id="b371e-120">Gyűjtemény hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b371e-120">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a><span data-ttu-id="b371e-121">Mintaadatok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b371e-121">Add sample data</span></span>

<span data-ttu-id="b371e-122">Most tooyour új adatgyűjtés adatkezelő használatával adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="b371e-122">You can now add data tooyour new collection using Data Explorer.</span></span>

1. <span data-ttu-id="b371e-123">Új adatbázis hello Data Explorer hello Gyűjtemények ablaktáblán jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b371e-123">In Data Explorer, hello new database appears in hello Collections pane.</span></span> <span data-ttu-id="b371e-124">Bontsa ki a hello **feladatok** adatbázisra, és bontsa ki a hello **elemek** gyűjtemény, kattintson a **dokumentumok**, és kattintson a **új dokumentumok**.</span><span class="sxs-lookup"><span data-stu-id="b371e-124">Expand hello **Tasks** database, expand hello **Items** collection, click **Documents**, and then click **New Documents**.</span></span> 

   ![Hozzon létre új dokumentumok az adatkezelő a hello Azure-portálon](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
2. <span data-ttu-id="b371e-126">A struktúra a következő hello mostantól hozzáadhatja azok toohello dokumentumgyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="b371e-126">Now add a document toohello collection with hello following structure.</span></span>

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

3. <span data-ttu-id="b371e-127">Hello json toohello hozzáadása után **dokumentumok** lapra, majd **mentése**.</span><span class="sxs-lookup"><span data-stu-id="b371e-127">Once you've added hello json toohello **Documents** tab, click **Save**.</span></span>

    ![Másolja át json-adatokat, és kattintson a Mentés gombra az adatok Explorer hello Azure-portálon](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-save-document.png)

4.  <span data-ttu-id="b371e-129">Hozzon létre és mentsen egy egyedi értéket hello helyezze egy további dokumentum `id` tulajdonság, és a változás hello más tulajdonságok, ahogyan szeretné.</span><span class="sxs-lookup"><span data-stu-id="b371e-129">Create and save one more document where you insert a unique value for hello `id` property, and change hello other properties as you see fit.</span></span> <span data-ttu-id="b371e-130">Mivel az Azure Cosmos DB nem kötelezi egy adott adatséma használatára, új dokumentumaihoz bármilyen struktúrát választhat.</span><span class="sxs-lookup"><span data-stu-id="b371e-130">Your new documents can have any structure you want as Azure Cosmos DB doesn't impose any schema on your data.</span></span>

     <span data-ttu-id="b371e-131">Most használja lekérdezi az adatkezelő tooretrieve az adatok hello kattintva is **szűrő szerkesztése** és **szűrés** gombokat.</span><span class="sxs-lookup"><span data-stu-id="b371e-131">You can now use queries in Data Explorer tooretrieve your data by clicking hello **Edit Filter** and **Apply Filter** buttons.</span></span> <span data-ttu-id="b371e-132">Alapértelmezés szerint adatkezelő használja `SELECT * FROM c` összes hello gyűjtemény, de a dokumentumok tooretrieve módosíthatja, hogy különböző tooa [SQL-lekérdezés](documentdb-sql-query.md), például a `SELECT * FROM c ORDER BY c._ts DESC`, csökkenő sorrendben minden hello dokumentum alapján tooreturn az időbélyegző.</span><span class="sxs-lookup"><span data-stu-id="b371e-132">By default, Data Explorer uses `SELECT * FROM c` tooretrieve all documents in hello collection, but you can change that tooa different [SQL query](documentdb-sql-query.md), such as `SELECT * FROM c ORDER BY c._ts DESC`, tooreturn all hello documents in descending order based on their timestamp.</span></span> 
 
     <span data-ttu-id="b371e-133">Is használhatja adatkezelő toocreate tárolt eljárások, felhasználó által megadott függvények és eseményindítók tooperform kiszolgálóoldali üzleti logikát is mint méretezési átviteli sebesség.</span><span class="sxs-lookup"><span data-stu-id="b371e-133">You can also use Data Explorer toocreate stored procedures, UDFs, and triggers tooperform server-side business logic as well as scale throughput.</span></span> <span data-ttu-id="b371e-134">Adatkezelő mutatja meg az összes hello beépített programozott adatelérési hello API-k érhető el, de hello Azure-portálon található egyszerű a hozzáférés tooyour adatokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="b371e-134">Data Explorer exposes all of hello built-in programmatic data access available in hello APIs, but provides easy access tooyour data in hello Azure portal.</span></span>

## <a name="clone-hello-sample-application"></a><span data-ttu-id="b371e-135">Klónozza a mintaalkalmazást hello</span><span class="sxs-lookup"><span data-stu-id="b371e-135">Clone hello sample application</span></span>

<span data-ttu-id="b371e-136">Most tegyük kapcsoló tooworking kóddal.</span><span class="sxs-lookup"><span data-stu-id="b371e-136">Now let's switch tooworking with code.</span></span> <span data-ttu-id="b371e-137">Most klónozza a Githubból, állítsa be a hello kapcsolati karakterláncot, és futtassa azt a DocumentDB API-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="b371e-137">Let's clone a DocumentDB API app from GitHub, set hello connection string, and run it.</span></span> <span data-ttu-id="b371e-138">Ön meg, milyen egyszerűen adatokkal toowork programozott módon.</span><span class="sxs-lookup"><span data-stu-id="b371e-138">You see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="b371e-139">Nyisson meg egy git terminálablakot, például a git bash eszközt, és `CD` tooa munkakönyvtárát.</span><span class="sxs-lookup"><span data-stu-id="b371e-139">Open a git terminal window, such as git bash, and `CD` tooa working directory.</span></span>  

2. <span data-ttu-id="b371e-140">Futtassa a következő parancs tooclone hello minta tárház hello.</span><span class="sxs-lookup"><span data-stu-id="b371e-140">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git
    ```

## <a name="review-hello-code"></a><span data-ttu-id="b371e-141">Tekintse át a hello kódot</span><span class="sxs-lookup"><span data-stu-id="b371e-141">Review hello code</span></span>

<span data-ttu-id="b371e-142">Most Meggyőződünk arról, mi történik a hello app gyors áttekintése.</span><span class="sxs-lookup"><span data-stu-id="b371e-142">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="b371e-143">Nyissa meg hello `Program.java` hello \src\GetStarted mappából fájlt, és ezek a sorok, a kód által létrehozott hello Azure Cosmos DB erőforrások található.</span><span class="sxs-lookup"><span data-stu-id="b371e-143">Open hello `Program.java` file from hello \src\GetStarted folder, and find these lines of code that create hello Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="b371e-144">Hello `DocumentClient` inicializálva van.</span><span class="sxs-lookup"><span data-stu-id="b371e-144">hello `DocumentClient` is initialized.</span></span>

    ```java
    this.client = new DocumentClient("https://FILLME.documents.azure.com",
            "FILLME", 
            new ConnectionPolicy(),
            ConsistencyLevel.Session);
    ```

* <span data-ttu-id="b371e-145">A rendszer létrehozza az új adatbázist.</span><span class="sxs-lookup"><span data-stu-id="b371e-145">A new database is created.</span></span>

    ```java
    Database database = new Database();
    database.setId(databaseName);
    
    this.client.createDatabase(database, null);
    ```

* <span data-ttu-id="b371e-146">A rendszer létrehozza az új gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="b371e-146">A new collection is created.</span></span>

    ```java
    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId(collectionName);

    ...

    this.client.createCollection(databaseLink, collectionInfo, requestOptions);
    ```

* <span data-ttu-id="b371e-147">A rendszer létrehoz néhány dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="b371e-147">Some documents are created.</span></span>

    ```java
    // Any Java object within your code can be serialized into JSON and written tooAzure Cosmos DB
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen");
    // More properties

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    this.client.createDocument(collectionLink, family, new RequestOptions(), true);
    ```

* <span data-ttu-id="b371e-148">A egy SQL-lekérdezést hajt végre a JSON-on.</span><span class="sxs-lookup"><span data-stu-id="b371e-148">A SQL query over JSON is performed.</span></span>

    ```java
    FeedOptions queryOptions = new FeedOptions();
    queryOptions.setPageSize(-1);
    queryOptions.setEnableCrossPartitionQuery(true);

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    FeedResponse<Document> queryResults = this.client.queryDocuments(
        collectionLink,
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", queryOptions);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }
    ```    

## <a name="update-your-connection-string"></a><span data-ttu-id="b371e-149">A kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="b371e-149">Update your connection string</span></span>

<span data-ttu-id="b371e-150">Most lépjen vissza az Azure portál tooget toohello kapcsolati karakterlánc adatainak és hello alkalmazásba másolja.</span><span class="sxs-lookup"><span data-stu-id="b371e-150">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span> <span data-ttu-id="b371e-151">Ez lehetővé teszi az alkalmazás toocommunicate az üzemeltetett adatbázissal.</span><span class="sxs-lookup"><span data-stu-id="b371e-151">This will enable your app toocommunicate with your hosted database.</span></span>

1. <span data-ttu-id="b371e-152">A hello [Azure-portálon](http://portal.azure.com/), az Azure Cosmos DB a fiókot, kattintson a bal oldali navigációs hello **kulcsok**, és kattintson a **írható-olvasható kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="b371e-152">In hello [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in hello left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="b371e-153">Fogjuk hello másolási gombok hello jobb oldalán hello képernyő toocopy hello URI és elsődleges kulcs a hello `Program.java` fájl hello következő lépésben.</span><span class="sxs-lookup"><span data-stu-id="b371e-153">You'll use hello copy buttons on hello right side of hello screen toocopy hello URI and PRIMARY KEY into hello `Program.java` file in hello next step.</span></span>

    ![Megtekintése és másolása egy hozzáférési kulcsot a hello Azure-portálon, a kulcsok panelen](./media/create-documentdb-dotnet/keys.png)

2. <span data-ttu-id="b371e-155">Hello nyissa meg a `Program.java` fájlt, másolja az URI érték portálról hello (hello Másolás gombra), és könnyebben hello végpont toohello DocumentClient konstruktor a értékének hello `Program.java`.</span><span class="sxs-lookup"><span data-stu-id="b371e-155">In hello open `Program.java` file, copy your URI value from hello portal (using hello copy button) and make it hello value of hello endpoint toohello DocumentClient constructor in `Program.java`.</span></span> 

    `"https://FILLME.documents.azure.com"`

4. <span data-ttu-id="b371e-156">Ezután másolja az elsődleges kulcs-érték hello portálról, és illessze be azt "FILLME", így keresztül hello hello DocumentClient konstruktor második paramétere.</span><span class="sxs-lookup"><span data-stu-id="b371e-156">Then copy your PRIMARY KEY value from hello portal and paste it over “FILLME”, making it hello second parameter in hello DocumentClient constructor.</span></span> <span data-ttu-id="b371e-157">Most már frissítette az alkalmazást az Azure Cosmos DB toocommunicate szükséges összes hello információval.</span><span class="sxs-lookup"><span data-stu-id="b371e-157">You've now updated your app with all hello info it needs toocommunicate with Azure Cosmos DB.</span></span> 
    
## <a name="run-hello-app"></a><span data-ttu-id="b371e-158">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="b371e-158">Run hello app</span></span>

1. <span data-ttu-id="b371e-159">A hello git terminálablakot `cd` toohello azure-cosmos-db-documentdb-java-getting-started mappa.</span><span class="sxs-lookup"><span data-stu-id="b371e-159">In hello git terminal window, `cd` toohello azure-cosmos-db-documentdb-java-getting-started folder.</span></span>

2. <span data-ttu-id="b371e-160">Írja be a hello git terminálablakot, `mvn package` tooinstall hello szükséges Java-csomagok.</span><span class="sxs-lookup"><span data-stu-id="b371e-160">In hello git terminal window, type `mvn package` tooinstall hello required Java packages.</span></span>

3. <span data-ttu-id="b371e-161">Hello git terminál-ablakban futtassa `mvn exec:java -D exec.mainClass=GetStarted.Program` a hello terminálablakot toostart a Java-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b371e-161">In hello git terminal window, run `mvn exec:java -D exec.mainClass=GetStarted.Program` in hello terminal window toostart your Java application.</span></span>

    <span data-ttu-id="b371e-162">A hello terminálablakot értesíti arról, hogy az adatbázis készült FamilyDB hello és egy kulcs toocontinue toopress kap.</span><span class="sxs-lookup"><span data-stu-id="b371e-162">In hello terminal window, you'll receive notification that hello FamilyDB database was created, and toopress a key toocontinue.</span></span> <span data-ttu-id="b371e-163">Nyomja le az ENTER kulcs toocreate hello adatbázis, majd váltson az adatkezelő toohello, és látni fogja, hogy most már tartalmaz egy FamilyDB adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b371e-163">Press a key toocreate hello database, then switch toohello Data Explorer and you'll see that it now contains a FamilyDB database.</span></span> <span data-ttu-id="b371e-164">Továbbra is toopress kulcsok toocreate hello adatgyűjtési és -hello dokumentumokat, és végezze el a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="b371e-164">Continue toopress keys toocreate hello collection and hello documents and then perform a query.</span></span> <span data-ttu-id="b371e-165">Hello projekt befejezését követően hello erőforrások törlése fiókjából.</span><span class="sxs-lookup"><span data-stu-id="b371e-165">When hello project completes, hello resources are deleted from your account.</span></span> 

    ![Megtekintése és másolása egy hozzáférési kulcsot a hello Azure-portálon, a kulcsok panelen](./media/create-documentdb-java/console-output.png)


## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="b371e-167">Tekintse át a szolgáltatásiszint-szerződések a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="b371e-167">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="b371e-168">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="b371e-168">Clean up resources</span></span>

<span data-ttu-id="b371e-169">Toocontinue toouse az alkalmazás nem fog, ha törli az összes erőforrást hozta létre a gyors üzembe helyezés hello az Azure-portálon az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b371e-169">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="b371e-170">A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson a létrehozott hello erőforrás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="b371e-170">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="b371e-171">Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="b371e-171">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b371e-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b371e-172">Next steps</span></span>

<span data-ttu-id="b371e-173">A gyors üzembe helyezés mér megismerte, hogyan toocreate Azure Cosmos DB fiókkal, a dokumentum-adatbázis és gyűjtemény hello adatkezelő használatával, és futtassa az alkalmazást toodo hello ugyanaz programozott módon.</span><span class="sxs-lookup"><span data-stu-id="b371e-173">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, document database, and collection using hello Data Explorer, and run an app toodo hello same thing programmatically.</span></span> <span data-ttu-id="b371e-174">További adatok tooyour Cosmos DB fiókot most importálhatja.</span><span class="sxs-lookup"><span data-stu-id="b371e-174">You can now import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="b371e-175">Adatok importálása az Azure Cosmos DB-be</span><span class="sxs-lookup"><span data-stu-id="b371e-175">Import data into Azure Cosmos DB</span></span>](import-data.md)


