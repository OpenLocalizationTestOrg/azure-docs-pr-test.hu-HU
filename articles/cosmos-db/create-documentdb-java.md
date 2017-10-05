---
title: "Azure Cosmos DB dokumentum-adatbázis létrehozása Javával | Microsoft Docs | Microsoft Docs'"
description: "Egy Java-kódmintát mutat be, amellyel csatlakozhat az Azure Cosmos DB DocumentDB API-hoz és lekérdezheti azt"
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
ms.openlocfilehash: df1a25d703a7b8082bdabb4f7d593cb005d416fe
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cosmos-db-create-a-document-database-using-java-and-the-azure-portal"></a><span data-ttu-id="3c1c5-103">Azure Cosmos DB: Dokumentum-adatbázis létrehozása a Java és az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="3c1c5-103">Azure Cosmos DB: Create a document database using Java and the Azure portal</span></span>

<span data-ttu-id="3c1c5-104">Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="3c1c5-105">Segítségével gyorsan létrehozhat és lekérdezhet dokumentum-, kulcs/érték és gráf típusú adatbázisokat, melyek mindegyike felhasználja az Azure Cosmos DB középpontjában álló globális elosztási és horizontális skálázhatósági képességeket.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="3c1c5-106">Ez a rövid útmutató létrehoz egy dokumentum-adatbázist az Azure Cosmos DB-hez készült Azure Portal-eszközök használatával.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-106">This quickstart creates a document database using the Azure portal tools for Azure Cosmos DB.</span></span> <span data-ttu-id="3c1c5-107">A rövid útmutató emellett bemutatja, hogyan hozhat létre gyorsan egy Java-konzolalkalmazást a [DocumentDB Java API](documentdb-sdk-java.md) használatával.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-107">This quickstart also shows you how to quickly create a Java console app using the [DocumentDB Java API](documentdb-sdk-java.md).</span></span> <span data-ttu-id="3c1c5-108">A rövid útmutatóban lévő utasítások bármilyen, Java-programok futtatására alkalmas operációs rendszeren végrehajthatók.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-108">The instructions in this quickstart can be followed on any operating system that is capable of running Java.</span></span> <span data-ttu-id="3c1c5-109">A rövid útmutató követésével megismerheti a dokumentumadatbázis-erőforrások létrehozását és módosítását a felhasználói felületen vagy programozás útján.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-109">By completing this quickstart you'll be familiar with creating and modifying document database resources in either the UI or programatically, whichever is your preference.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c1c5-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3c1c5-110">Prerequisites</span></span>

* [<span data-ttu-id="3c1c5-111">Java fejlesztői készlet (JDK) 1.7+</span><span class="sxs-lookup"><span data-stu-id="3c1c5-111">Java Development Kit (JDK) 1.7+</span></span>](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * <span data-ttu-id="3c1c5-112">Ubuntu rendszeren futtassa az `apt-get install default-jdk` parancsot a JDK telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-112">On Ubuntu, run `apt-get install default-jdk` to install the JDK.</span></span>
    * <span data-ttu-id="3c1c5-113">Ügyeljen arra, hogy a JAVA_HOME környezeti változó arra a mappára mutasson, ahová a JDK telepítve lett.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-113">Be sure to set the JAVA_HOME environment variable to point to the folder where the JDK is installed.</span></span>
* <span data-ttu-id="3c1c5-114">[Maven](http://maven.apache.org/download.cgi) bináris archívum [letöltése](http://maven.apache.org/install.html) és [telepítése](http://maven.apache.org/)</span><span class="sxs-lookup"><span data-stu-id="3c1c5-114">[Download](http://maven.apache.org/download.cgi) and [install](http://maven.apache.org/install.html) a [Maven](http://maven.apache.org/) binary archive</span></span>
    * <span data-ttu-id="3c1c5-115">Ubuntu rendszeren futtathatja az `apt-get install maven` parancsot a Maven telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-115">On Ubuntu, you can run `apt-get install maven` to install Maven.</span></span>
* [<span data-ttu-id="3c1c5-116">Git</span><span class="sxs-lookup"><span data-stu-id="3c1c5-116">Git</span></span>](https://www.git-scm.com/)
    * <span data-ttu-id="3c1c5-117">Ubuntu rendszeren futtathatja a `sudo apt-get install git` parancsot a Git telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-117">On Ubuntu, you can run `sudo apt-get install git` to install Git.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="3c1c5-118">Adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="3c1c5-118">Create a database account</span></span>

<span data-ttu-id="3c1c5-119">A dokumentum-adatbázis létrehozásához először létre kell hoznia egy SQL- (DocumentDB-) adatbázisfiókot az Azure Cosmos DB segítségével.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-119">Before you can create a document database, you need to create a SQL (DocumentDB) database account with Azure Cosmos DB.</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a><span data-ttu-id="3c1c5-120">Gyűjtemény hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3c1c5-120">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a><span data-ttu-id="3c1c5-121">Mintaadatok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3c1c5-121">Add sample data</span></span>

<span data-ttu-id="3c1c5-122">Az Adatkezelő segítségével adatokat adhat hozzá az új gyűjteményhez.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-122">You can now add data to your new collection using Data Explorer.</span></span>

1. <span data-ttu-id="3c1c5-123">Az új adatbázis az Adatkezelőben a Gyűjtemények ablaktáblán jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-123">In Data Explorer, the new database appears in the Collections pane.</span></span> <span data-ttu-id="3c1c5-124">Bontsa ki a **Feladatok** adatbázist, majd az **Elemek** gyűjteményt, végül kattintson a **Dokumentumok**, majd pedig az **Új dokumentumok** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-124">Expand the **Tasks** database, expand the **Items** collection, click **Documents**, and then click **New Documents**.</span></span> 

   ![Új dokumentumok létrehozása az Azure Portal Adatkezelőjében](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
2. <span data-ttu-id="3c1c5-126">Adjon hozzá egy dokumentumot a gyűjteményhez az alábbi struktúrával.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-126">Now add a document to the collection with the following structure.</span></span>

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

3. <span data-ttu-id="3c1c5-127">A JSON hozzáadása után a **Dokumentumok** lapon kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-127">Once you've added the json to the **Documents** tab, click **Save**.</span></span>

    ![Másolja át a json-adatokat, és kattintson a Mentés gombra az Adatkezelőben az Azure-portálon](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-save-document.png)

4.  <span data-ttu-id="3c1c5-129">Hozzon létre és mentsen még egy dokumentumot, amelyben egyedi értéket szúr be az `id` tulajdonság számára, és tetszés szerint módosítja a többi tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-129">Create and save one more document where you insert a unique value for the `id` property, and change the other properties as you see fit.</span></span> <span data-ttu-id="3c1c5-130">Mivel az Azure Cosmos DB nem kötelezi egy adott adatséma használatára, új dokumentumaihoz bármilyen struktúrát választhat.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-130">Your new documents can have any structure you want as Azure Cosmos DB doesn't impose any schema on your data.</span></span>

     <span data-ttu-id="3c1c5-131">Az Adatkezelővel így már lekérdezések használatával lekérheti adatait. Ehhez kattintson a **Szűrő szerkesztése**, majd az **Alkalmaz** gombokra.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-131">You can now use queries in Data Explorer to retrieve your data by clicking the **Edit Filter** and **Apply Filter** buttons.</span></span> <span data-ttu-id="3c1c5-132">Az Adatkezelő alapértelmezés szerint a `SELECT * FROM c` lekérdezést használja a gyűjteményben lévő összes dokumentum lekéréséhez, de ezt más [SQL-lekérdezésre](documentdb-sql-query.md) is módosíthatja, például a `SELECT * FROM c ORDER BY c._ts DESC` lekérdezésre, ha azt szeretné, hogy a rendszer a dokumentumokat időbélyegzőik szerint csökkenő sorrendben adja vissza.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-132">By default, Data Explorer uses `SELECT * FROM c` to retrieve all documents in the collection, but you can change that to a different [SQL query](documentdb-sql-query.md), such as `SELECT * FROM c ORDER BY c._ts DESC`, to return all the documents in descending order based on their timestamp.</span></span> 
 
     <span data-ttu-id="3c1c5-133">Az Adatkezelővel létrehozhat tárolt eljárásokat is, felhasználói függvényeket és a kiszolgálóoldali üzleti logikákat végrehajtó eseményindítókat, valamint szabályozhatja az átviteli sebességet.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-133">You can also use Data Explorer to create stored procedures, UDFs, and triggers to perform server-side business logic as well as scale throughput.</span></span> <span data-ttu-id="3c1c5-134">Az Adatkezelő hozzáférhetővé teszi az API-k összes beépített, programozható adatelérési funkcióját, és az Azure Portalon tárolt adataihoz is egyszerű hozzáférést biztosít.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-134">Data Explorer exposes all of the built-in programmatic data access available in the APIs, but provides easy access to your data in the Azure portal.</span></span>

## <a name="clone-the-sample-application"></a><span data-ttu-id="3c1c5-135">A mintaalkalmazás klónozása</span><span class="sxs-lookup"><span data-stu-id="3c1c5-135">Clone the sample application</span></span>

<span data-ttu-id="3c1c5-136">Most pedig váltsunk át kódok használatára.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-136">Now let's switch to working with code.</span></span> <span data-ttu-id="3c1c5-137">Klónozunk egy DocumentDB API-alkalmazást a GitHubról, beállítjuk a kapcsolati karakterláncot, és futtatjuk az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-137">Let's clone a DocumentDB API app from GitHub, set the connection string, and run it.</span></span> <span data-ttu-id="3c1c5-138">Ilyen egyszerű az adatokkal programozott módon dolgozni.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-138">You see how easy it is to work with data programmatically.</span></span> 

1. <span data-ttu-id="3c1c5-139">Nyisson meg egy git terminálablakot, például a git bash eszközt, és a `CD` paranccsal lépjen egy munkakönyvtárba.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-139">Open a git terminal window, such as git bash, and `CD` to a working directory.</span></span>  

2. <span data-ttu-id="3c1c5-140">Futtassa a következő parancsot a minta tárház klónozásához.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-140">Run the following command to clone the sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git
    ```

## <a name="review-the-code"></a><span data-ttu-id="3c1c5-141">A kód áttekintése</span><span class="sxs-lookup"><span data-stu-id="3c1c5-141">Review the code</span></span>

<span data-ttu-id="3c1c5-142">Tekintsük át, hogy mi történik az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-142">Let's make a quick review of what's happening in the app.</span></span> <span data-ttu-id="3c1c5-143">Nyissa meg a \src\GetStarted mappában található `Program.java` fájlt, és keresse meg ezeket a kódsorokat, amelyek létrehozzák az Azure Cosmos DB-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-143">Open the `Program.java` file from the \src\GetStarted folder, and find these lines of code that create the Azure Cosmos DB resources.</span></span> 

* <span data-ttu-id="3c1c5-144">A `DocumentClient` inicializálva van.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-144">The `DocumentClient` is initialized.</span></span>

    ```java
    this.client = new DocumentClient("https://FILLME.documents.azure.com",
            "FILLME", 
            new ConnectionPolicy(),
            ConsistencyLevel.Session);
    ```

* <span data-ttu-id="3c1c5-145">A rendszer létrehozza az új adatbázist.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-145">A new database is created.</span></span>

    ```java
    Database database = new Database();
    database.setId(databaseName);
    
    this.client.createDatabase(database, null);
    ```

* <span data-ttu-id="3c1c5-146">A rendszer létrehozza az új gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-146">A new collection is created.</span></span>

    ```java
    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId(collectionName);

    ...

    this.client.createCollection(databaseLink, collectionInfo, requestOptions);
    ```

* <span data-ttu-id="3c1c5-147">A rendszer létrehoz néhány dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-147">Some documents are created.</span></span>

    ```java
    // Any Java object within your code can be serialized into JSON and written to Azure Cosmos DB
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen");
    // More properties

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    this.client.createDocument(collectionLink, family, new RequestOptions(), true);
    ```

* <span data-ttu-id="3c1c5-148">A egy SQL-lekérdezést hajt végre a JSON-on.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-148">A SQL query over JSON is performed.</span></span>

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

## <a name="update-your-connection-string"></a><span data-ttu-id="3c1c5-149">A kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="3c1c5-149">Update your connection string</span></span>

<span data-ttu-id="3c1c5-150">Lépjen vissza az Azure Portalra a kapcsolati karakterlánc adataiért, majd másolja be azokat az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-150">Now go back to the Azure portal to get your connection string information and copy it into the app.</span></span> <span data-ttu-id="3c1c5-151">Ez lehetővé teszi az alkalmazás számára, hogy kommunikáljon az üzemeltetett adatbázissal.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-151">This will enable your app to communicate with your hosted database.</span></span>

1. <span data-ttu-id="3c1c5-152">Az [Azure Portalon](http://portal.azure.com/) az Azure Cosmos DB-fiókban a bal oldalsávon kattintson a **kulcsok** elemre, majd kattintson az **írási/olvasási kulcsok** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-152">In the [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in the left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="3c1c5-153">A következő lépésben a képernyő jobb oldalán lévő másolási gombokkal másolhatja az URI és az ELSŐDLEGES KULCS értékét a `Program.java` fájlba.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-153">You'll use the copy buttons on the right side of the screen to copy the URI and PRIMARY KEY into the `Program.java` file in the next step.</span></span>

    ![Hozzáférési kulcs megtekintése és másolása az Azure Portal kulcsok paneljén](./media/create-documentdb-dotnet/keys.png)

2. <span data-ttu-id="3c1c5-155">A megnyitott `Program.java` fájlba (a másolás gomb használatával) másolja be az URI érteket a portálról, és azt adja meg a DocumentClient konstruktor végpontértékeként a `Program.java` fájlban.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-155">In the open `Program.java` file, copy your URI value from the portal (using the copy button) and make it the value of the endpoint to the DocumentClient constructor in `Program.java`.</span></span> 

    `"https://FILLME.documents.azure.com"`

4. <span data-ttu-id="3c1c5-156">Ezután másolja ki az ELSŐDLEGES KULCS értékét a portálról, és illessze be a „FILLME” helyőrző helyére, így az a második paraméter lesz a DocumentClient konstruktorában.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-156">Then copy your PRIMARY KEY value from the portal and paste it over “FILLME”, making it the second parameter in the DocumentClient constructor.</span></span> <span data-ttu-id="3c1c5-157">Az alkalmazás frissítve lett minden olyan információval, amely az Azure Cosmos DB-vel való kommunikációhoz szükséges.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-157">You've now updated your app with all the info it needs to communicate with Azure Cosmos DB.</span></span> 
    
## <a name="run-the-app"></a><span data-ttu-id="3c1c5-158">Az alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="3c1c5-158">Run the app</span></span>

1. <span data-ttu-id="3c1c5-159">A git terminálablakában a `cd` paranccsal lépjen az azure-cosmos-db-documentdb-java-getting-started mappába.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-159">In the git terminal window, `cd` to the azure-cosmos-db-documentdb-java-getting-started folder.</span></span>

2. <span data-ttu-id="3c1c5-160">A git terminálablakába írja be az `mvn package` parancsot a szükséges Java-csomagok telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-160">In the git terminal window, type `mvn package` to install the required Java packages.</span></span>

3. <span data-ttu-id="3c1c5-161">A git terminálablakában futtassa az `mvn exec:java -D exec.mainClass=GetStarted.Program` parancsot a Java-alkalmazás elindításához.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-161">In the git terminal window, run `mvn exec:java -D exec.mainClass=GetStarted.Program` in the terminal window to start your Java application.</span></span>

    <span data-ttu-id="3c1c5-162">Amikor a terminálablakban értesítést kap arról, hogy a FamilyDB adatbázis létrejött, nyomjon le egy billentyűt a folytatáshoz.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-162">In the terminal window, you'll receive notification that the FamilyDB database was created, and to press a key to continue.</span></span> <span data-ttu-id="3c1c5-163">Nyomjon le egy billentyűt az adatbázis létrehozásához, majd váltson át az Adatkezelőre, ahol láthatja, hogy az már tartalmazza a FamilyDB adatbázist.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-163">Press a key to create the database, then switch to the Data Explorer and you'll see that it now contains a FamilyDB database.</span></span> <span data-ttu-id="3c1c5-164">Nyomjon le további billentyűket a gyűjtemény és a dokumentumok létrehozásához, majd hajtson végre egy lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-164">Continue to press keys to create the collection and the documents and then perform a query.</span></span> <span data-ttu-id="3c1c5-165">Amikor a projekt befejeződik, a rendszer törli az erőforrásokat a fiókjából.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-165">When the project completes, the resources are deleted from your account.</span></span> 

    ![Hozzáférési kulcs megtekintése és másolása az Azure Portal kulcsok paneljén](./media/create-documentdb-java/console-output.png)


## <a name="review-slas-in-the-azure-portal"></a><span data-ttu-id="3c1c5-167">Az SLA-k áttekintése az Azure Portalon</span><span class="sxs-lookup"><span data-stu-id="3c1c5-167">Review SLAs in the Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="3c1c5-168">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="3c1c5-168">Clean up resources</span></span>

<span data-ttu-id="3c1c5-169">Ha az alkalmazást már nem használja, akkor a következő lépésekkel a mintaalkalmazás által létrehozott összes erőforrást törölheti az Azure Portalon:</span><span class="sxs-lookup"><span data-stu-id="3c1c5-169">If you're not going to continue to use this app, delete all resources created by this quickstart in the Azure portal with the following steps:</span></span>

1. <span data-ttu-id="3c1c5-170">Az Azure Portal bal oldali menüjében kattintson az **Erőforráscsoportok** lehetőségre, majd kattintson a létrehozott erőforrás nevére.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-170">From the left-hand menu in the Azure portal, click **Resource groups** and then click the name of the resource you created.</span></span> 
2. <span data-ttu-id="3c1c5-171">Az erőforráscsoport lapján kattintson a **Törlés** elemre, írja be a törölni kívánt erőforrás nevét a szövegmezőbe, majd kattintson a **Törlés** gombra.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-171">On your resource group page, click **Delete**, type the name of the resource to delete in the text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3c1c5-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3c1c5-172">Next steps</span></span>

<span data-ttu-id="3c1c5-173">Ebben a rövid útmutatóban bemutattuk, hogyan hozhat létre Azure Cosmos DB-fiókot, dokumentum-adatbázist és gyűjteményt az Adatkezelő segítségével, valamint hogyan futtathat egy alkalmazást, amely programozottan hajtja végre ugyanezt.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-173">In this quickstart, you've learned how to create an Azure Cosmos DB account, document database, and collection using the Data Explorer, and run an app to do the same thing programmatically.</span></span> <span data-ttu-id="3c1c5-174">Most további adatokat importálhat a Cosmos DB-fiókba.</span><span class="sxs-lookup"><span data-stu-id="3c1c5-174">You can now import additional data to your Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="3c1c5-175">Adatok importálása az Azure Cosmos DB-be</span><span class="sxs-lookup"><span data-stu-id="3c1c5-175">Import data into Azure Cosmos DB</span></span>](import-data.md)


