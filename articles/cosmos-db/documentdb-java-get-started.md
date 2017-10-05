---
title: "NoSQL-oktatóanyag: DocumentDB API Azure Cosmos DB Java SDK |} Microsoft Docs"
description: "NoSQL-oktatóanyag, amely létrehoz egy online adatbázist és a Java-konzolalkalmazást a DocumentDB API használatával az Azure Cosmos DB rendszerhez. Az Azure DocumentDB egy NoSQL-alapú adatbázis a JSON formátumhoz."
keywords: "nosql-oktatóanyag, online adatbázis, java konzolalkalmazás"
services: cosmos-db
documentationcenter: Java
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: 75a9efa1-7edd-4fed-9882-c0177274cbb2
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 05/22/2017
ms.author: arramac
ms.openlocfilehash: 5c4bcda308f001572e1c34e991616fc209250a02
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="nosql-tutorial-build-a-documentdb-api-java-console-application"></a><span data-ttu-id="b5553-105">NoSQL-oktatóanyag: a DocumentDB API Java Konzolalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="b5553-105">NoSQL tutorial: Build a DocumentDB API Java console application</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b5553-106">.NET</span><span class="sxs-lookup"><span data-stu-id="b5553-106">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="b5553-107">.NET Core</span><span class="sxs-lookup"><span data-stu-id="b5553-107">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="b5553-108">Node.js MongoDB-hez</span><span class="sxs-lookup"><span data-stu-id="b5553-108">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="b5553-109">Node.js</span><span class="sxs-lookup"><span data-stu-id="b5553-109">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="b5553-110">Java</span><span class="sxs-lookup"><span data-stu-id="b5553-110">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="b5553-111">C++</span><span class="sxs-lookup"><span data-stu-id="b5553-111">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 

<span data-ttu-id="b5553-112">Üdvözöljük a NoSQL-oktatóanyagban a DocumentDB API Azure Cosmos DB Java SDK-t!</span><span class="sxs-lookup"><span data-stu-id="b5553-112">Welcome to the NoSQL tutorial for the DocumentDB API for Azure Cosmos DB Java SDK!</span></span> <span data-ttu-id="b5553-113">Az oktatóanyag lépéseinek követésével egy olyan konzolalkalmazást készít, amely Azure Cosmos DB-erőforrásokat hoz létre és kérdez le.</span><span class="sxs-lookup"><span data-stu-id="b5553-113">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources.</span></span>

<span data-ttu-id="b5553-114">Az oktatóanyag a következőket ismerteti:</span><span class="sxs-lookup"><span data-stu-id="b5553-114">We cover:</span></span>

* <span data-ttu-id="b5553-115">Azure Cosmos DB-fiók létrehozása és csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="b5553-115">Creating and connecting to an Azure Cosmos DB account</span></span>
* <span data-ttu-id="b5553-116">A Visual Studio megoldás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b5553-116">Configuring your Visual Studio Solution</span></span>
* <span data-ttu-id="b5553-117">Online adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="b5553-117">Creating an online database</span></span>
* <span data-ttu-id="b5553-118">Gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="b5553-118">Creating a collection</span></span>
* <span data-ttu-id="b5553-119">JSON-dokumentumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="b5553-119">Creating JSON documents</span></span>
* <span data-ttu-id="b5553-120">A gyűjtemény lekérdezése</span><span class="sxs-lookup"><span data-stu-id="b5553-120">Querying the collection</span></span>
* <span data-ttu-id="b5553-121">JSON-dokumentumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="b5553-121">Creating JSON documents</span></span>
* <span data-ttu-id="b5553-122">A gyűjtemény lekérdezése</span><span class="sxs-lookup"><span data-stu-id="b5553-122">Querying the collection</span></span>
* <span data-ttu-id="b5553-123">Dokumentum cseréje</span><span class="sxs-lookup"><span data-stu-id="b5553-123">Replacing a document</span></span>
* <span data-ttu-id="b5553-124">Dokumentum törlése</span><span class="sxs-lookup"><span data-stu-id="b5553-124">Deleting a document</span></span>
* <span data-ttu-id="b5553-125">Adatbázis törlése</span><span class="sxs-lookup"><span data-stu-id="b5553-125">Deleting the database</span></span>

<span data-ttu-id="b5553-126">Most pedig lássunk neki!</span><span class="sxs-lookup"><span data-stu-id="b5553-126">Now let's get started!</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b5553-127">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b5553-127">Prerequisites</span></span>
<span data-ttu-id="b5553-128">Győződjön meg róla, hogy rendelkezik az alábbiakkal:</span><span class="sxs-lookup"><span data-stu-id="b5553-128">Make sure you have the following:</span></span>

* <span data-ttu-id="b5553-129">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="b5553-129">An active Azure account.</span></span> <span data-ttu-id="b5553-130">Ha még nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="b5553-130">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span> <span data-ttu-id="b5553-131">Másik lehetőségként használhatja az [Azure Cosmos DB Emulatort](local-emulator.md) az oktatóanyaghoz.</span><span class="sxs-lookup"><span data-stu-id="b5553-131">Alternatively, you can use the [Azure Cosmos DB Emulator](local-emulator.md) for this tutorial.</span></span>
* [<span data-ttu-id="b5553-132">Git</span><span class="sxs-lookup"><span data-stu-id="b5553-132">Git</span></span>](https://git-scm.com/downloads)
* <span data-ttu-id="b5553-133">[Java fejlesztői készlet (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="b5553-133">[Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span>
* <span data-ttu-id="b5553-134">[Maven](http://maven.apache.org/download.cgi).</span><span class="sxs-lookup"><span data-stu-id="b5553-134">[Maven](http://maven.apache.org/download.cgi).</span></span>

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="b5553-135">1. lépés: Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="b5553-135">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="b5553-136">Hozzunk létre egy Azure Cosmos DB-fiókot.</span><span class="sxs-lookup"><span data-stu-id="b5553-136">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="b5553-137">Ha már rendelkezik egy használni kívánt fiókkal, folytassa [A GitHub-projekt klónozása](#GitClone) című lépéssel.</span><span class="sxs-lookup"><span data-stu-id="b5553-137">If you already have an account you want to use, you can skip ahead to [Clone the GitHub project](#GitClone).</span></span> <span data-ttu-id="b5553-138">Ha az Azure Cosmos DB Emulatort használja, kövesse az [Azure Cosmos DB Emulatornál](local-emulator.md) leírt lépéseket az emulátor beállításához, majd ugorjon [A GitHub-projekt klónozása](#GitClone) című lépésre.</span><span class="sxs-lookup"><span data-stu-id="b5553-138">If you are using the Azure Cosmos DB Emulator, follow the steps at [Azure Cosmos DB Emulator](local-emulator.md) to set up the emulator and skip ahead to [Clone the GitHub project](#GitClone).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="b5553-139"><a id="GitClone"></a>2. lépés: A GitHub-projekt klónozása</span><span class="sxs-lookup"><span data-stu-id="b5553-139"><a id="GitClone"></a>Step 2: Clone the GitHub project</span></span>
<span data-ttu-id="b5553-140">A GitHub-adattár klónozásával kezdheti meg [az Azure Cosmos DB és a Java használatának első lépéseit](https://github.com/Azure-Samples/documentdb-java-getting-started).</span><span class="sxs-lookup"><span data-stu-id="b5553-140">You can get started by cloning the GitHub repository for [Get Started with Azure Cosmos DB and Java](https://github.com/Azure-Samples/documentdb-java-getting-started).</span></span> <span data-ttu-id="b5553-141">Futtassa például egy helyi könyvtárból az alábbi parancsot a mintaprojekt helyi lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="b5553-141">For example, from a local directory run the following to retrieve the sample project locally.</span></span>

    git clone git@github.com:Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git

    cd azure-cosmos-db-documentdb-java-getting-started

<span data-ttu-id="b5553-142">A könyvtár neve tartalmazza a `pom.xml` a projekt és egy `src` Java forrás kód beleértve tartalmazó mappa `Program.java` mely bemutatja hogyan Azure Cosmos DB például a dokumentumok létrehozása és egy gyűjteményben lévő adatok lekérdezése az egyszerű műveleteket hajtanak végre.</span><span class="sxs-lookup"><span data-stu-id="b5553-142">The directory contains a `pom.xml` for the project and a `src` folder containing Java source code including `Program.java` which shows how perform simple operations with Azure Cosmos DB like creating documents and querying data within a collection.</span></span> <span data-ttu-id="b5553-143">A `pom.xml` fájl tartalmaz egy [Maven DocumentDB Java SDK](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb)-függőséget.</span><span class="sxs-lookup"><span data-stu-id="b5553-143">The `pom.xml` includes a dependency on the [DocumentDB Java SDK on Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb).</span></span>

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-documentdb</artifactId>
        <version>LATEST</version>
    </dependency>

## <span data-ttu-id="b5553-144"><a id="Connect"></a>3. lépés: Csatlakozás egy Azure Cosmos DB-fiókhoz</span><span class="sxs-lookup"><span data-stu-id="b5553-144"><a id="Connect"></a>Step 3: Connect to an Azure Cosmos DB account</span></span>
<span data-ttu-id="b5553-145">Térjen vissza az [Azure Portalra](https://portal.azure.com) a végpont és az elsődleges főkulcs beszerzéséért.</span><span class="sxs-lookup"><span data-stu-id="b5553-145">Next, head back to the [Azure Portal](https://portal.azure.com) to retrieve your endpoint and primary master key.</span></span> <span data-ttu-id="b5553-146">Az Azure Cosmos DB végpont és az elsődleges kulcs ahhoz szükséges, hogy az alkalmazás tudja, hova kell csatlakoznia, az Azure Cosmos DB pedig megbízzon az alkalmazás által létesített kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="b5553-146">The Azure Cosmos DB endpoint and primary key are necessary for your application to understand where to connect to, and for Azure Cosmos DB to trust your application's connection.</span></span>

<span data-ttu-id="b5553-147">Az Azure Portalon lépjen a Azure Cosmos DB-fiókra, majd kattintson a **Kulcsok** elemre.</span><span class="sxs-lookup"><span data-stu-id="b5553-147">In the Azure Portal, navigate to your Azure Cosmos DB account, and then click **Keys**.</span></span> <span data-ttu-id="b5553-148">Másolja ki az URI-t a portálról, és illessze be a Program.java fájl `https://FILLME.documents.azure.com` elemébe.</span><span class="sxs-lookup"><span data-stu-id="b5553-148">Copy the URI from the portal and paste it into `https://FILLME.documents.azure.com` in the Program.java file.</span></span> <span data-ttu-id="b5553-149">Ezután másolja ki a PRIMARY KEY kulcsot a portálról, és illessze be a `FILLME` elembe.</span><span class="sxs-lookup"><span data-stu-id="b5553-149">Then copy the PRIMARY KEY from the portal and paste it into `FILLME`.</span></span>

    this.client = new DocumentClient(
        "https://FILLME.documents.azure.com",
        "FILLME"
        , new ConnectionPolicy(),
        ConsistencyLevel.Session);

![Képernyőfelvétel a NoSQL-oktatóanyagban a Java konzolalkalmazás létrehozásához használt Azure Portalról.][keys]

## <a name="step-4-create-a-database"></a><span data-ttu-id="b5553-152">4. lépés: Adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="b5553-152">Step 4: Create a database</span></span>
<span data-ttu-id="b5553-153">Az Azure Cosmos [DB-adatbázis](documentdb-resources.md#databases) a **DocumentClient** osztály [createDatabase](/java/api/com.microsoft.azure.documentdb._document_client.createdatabase) metódusának használatával hozható létre.</span><span class="sxs-lookup"><span data-stu-id="b5553-153">Your Azure Cosmos DB [database](documentdb-resources.md#databases) can be created by using the [createDatabase](/java/api/com.microsoft.azure.documentdb._document_client.createdatabase) method of the **DocumentClient** class.</span></span> <span data-ttu-id="b5553-154">Az adatbázis a JSON-dokumentumtároló gyűjtemények között particionált logikai tárolója.</span><span class="sxs-lookup"><span data-stu-id="b5553-154">A database is the logical container of JSON document storage partitioned across collections.</span></span>

    Database database = new Database();
    database.setId("familydb");
    this.client.createDatabase(database, null);

## <span data-ttu-id="b5553-155"><a id="CreateColl"></a>5. lépés: Gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="b5553-155"><a id="CreateColl"></a>Step 5: Create a collection</span></span>
> [!WARNING]
> <span data-ttu-id="b5553-156">A **createCollection** létrehoz egy fenntartott adattovábbítási kapacitással rendelkező új gyűjteményt, amely költségeket von maga után.</span><span class="sxs-lookup"><span data-stu-id="b5553-156">**createCollection** creates a new collection with reserved throughput, which has pricing implications.</span></span> <span data-ttu-id="b5553-157">További részletekért látogasson el az [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="b5553-157">For more details, visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span>
> 
> 

<span data-ttu-id="b5553-158">Egy [gyűjtemény](documentdb-resources.md#collections) a **DocumentClient** osztály [createCollection](/java/api/com.microsoft.azure.documentdb._document_client.createcollection) metódusával hozható létre.</span><span class="sxs-lookup"><span data-stu-id="b5553-158">A [collection](documentdb-resources.md#collections) can be created by using the [createCollection](/java/api/com.microsoft.azure.documentdb._document_client.createcollection) method of the **DocumentClient** class.</span></span> <span data-ttu-id="b5553-159">A gyűjtemény egy JSON-dokumentumokat és a kapcsolódó JavaScript-alkalmazáslogikát tartalmazó tároló.</span><span class="sxs-lookup"><span data-stu-id="b5553-159">A collection is a container of JSON documents and associated JavaScript application logic.</span></span>


    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId("familycoll");

    // Azure Cosmos DB collections can be reserved with throughput specified in request units/second. 
    // Here we create a collection with 400 RU/s.
    RequestOptions requestOptions = new RequestOptions();
    requestOptions.setOfferThroughput(400);

    this.client.createCollection("/dbs/familydb", collectionInfo, requestOptions);

## <span data-ttu-id="b5553-160"><a id="CreateDoc"></a>6. lépés: JSON-dokumentumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="b5553-160"><a id="CreateDoc"></a>Step 6: Create JSON documents</span></span>
<span data-ttu-id="b5553-161">A [dokumentumok](documentdb-resources.md#documents) a **DocumentClient** osztály [createDocument](/java/api/com.microsoft.azure.documentdb._document_client.createdocument) metódusával hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="b5553-161">A [document](documentdb-resources.md#documents) can be created by using the [createDocument](/java/api/com.microsoft.azure.documentdb._document_client.createdocument) method of the **DocumentClient** class.</span></span> <span data-ttu-id="b5553-162">A dokumentumok a felhasználó által megadott (tetszőleges) JSON-tartalmak.</span><span class="sxs-lookup"><span data-stu-id="b5553-162">Documents are user-defined (arbitrary) JSON content.</span></span> <span data-ttu-id="b5553-163">Most már beilleszthetünk egy vagy több dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="b5553-163">We can now insert one or more documents.</span></span> <span data-ttu-id="b5553-164">Ha már rendelkezik az adatbázisban tárolni kívánt adatokat, használhatja az Azure Cosmos DB [adatáttelepítési eszközét](import-data.md) az adatok importálása az adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="b5553-164">If you already have data you'd like to store in your database, you can use Azure Cosmos DB's [Data Migration tool](import-data.md) to import the data into a database.</span></span>

    // Insert your Java objects as documents 
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen")

    // More initialization skipped for brevity. You can have nested references
    andersenFamily.setParents(new Parent[] { parent1, parent2 });
    andersenFamily.setDistrict("WA5");
    Address address = new Address();
    address.setCity("Seattle");
    address.setCounty("King");
    address.setState("WA");

    andersenFamily.setAddress(address);
    andersenFamily.setRegistered(true);

    this.client.createDocument("/dbs/familydb/colls/familycoll", family, new RequestOptions(), true);

![A diagram a NoSQL-oktatóanyagban a Java konzolalkalmazás létrehozásához használt fiók, online adatbázis, gyűjtemény és dokumentumok hierarchikus kapcsolatát ábrázolja.](./media/documentdb-get-started/nosql-tutorial-account-database.png)

## <span data-ttu-id="b5553-166"><a id="Query"></a>7. lépés: Az Azure Cosmos DB-erőforrások lekérdezése</span><span class="sxs-lookup"><span data-stu-id="b5553-166"><a id="Query"></a>Step 7: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="b5553-167">Az Azure Cosmos DB támogatja az egyes gyűjteményekben tárolt JSON-dokumentumokon végzett [részletes lekérdezéseket](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="b5553-167">Azure Cosmos DB supports rich [queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span>  <span data-ttu-id="b5553-168">Az alábbi mintakód bemutatja, hogyan kérdezheti le a dokumentumokat az Azure Cosmos DB-ben az SQL-szintaxis és a [queryDocuments](/java/api/com.microsoft.azure.documentdb._document_client.querydocuments) metódus együttes használatával.</span><span class="sxs-lookup"><span data-stu-id="b5553-168">The following sample code shows how to query documents in Azure Cosmos DB using SQL syntax with the [queryDocuments](/java/api/com.microsoft.azure.documentdb._document_client.querydocuments) method.</span></span>

    FeedResponse<Document> queryResults = this.client.queryDocuments(
        "/dbs/familydb/colls/familycoll",
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", 
        null);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }

## <span data-ttu-id="b5553-169"><a id="ReplaceDocument"></a>8. lépés: JSON-dokumentumok cseréje</span><span class="sxs-lookup"><span data-stu-id="b5553-169"><a id="ReplaceDocument"></a>Step 8: Replace JSON document</span></span>
<span data-ttu-id="b5553-170">Az Azure Cosmos DB támogatja a JSON-dokumentumok [replaceDocument](/java/api/com.microsoft.azure.documentdb._document_client.replacedocument) metódussal végrehajtott frissítését.</span><span class="sxs-lookup"><span data-stu-id="b5553-170">Azure Cosmos DB supports updating JSON documents using the [replaceDocument](/java/api/com.microsoft.azure.documentdb._document_client.replacedocument) method.</span></span>

    // Update a property
    andersenFamily.Children[0].Grade = 6;

    this.client.replaceDocument(
        "/dbs/familydb/colls/familycoll/docs/Andersen.1", 
        andersenFamily,
        null);

## <span data-ttu-id="b5553-171"><a id="DeleteDocument"></a>9. lépés: JSON-dokumentumok törlése</span><span class="sxs-lookup"><span data-stu-id="b5553-171"><a id="DeleteDocument"></a>Step 9: Delete JSON document</span></span>
<span data-ttu-id="b5553-172">Hasonlóképpen az Azure Cosmos DB támogatja a JSON-dokumentumok [deleteDocument](/java/api/com.microsoft.azure.documentdb._document_client.deletedocument) metódussal végrehajtott törlését.</span><span class="sxs-lookup"><span data-stu-id="b5553-172">Similarly, Azure Cosmos DB supports deleting JSON documents using the [deleteDocument](/java/api/com.microsoft.azure.documentdb._document_client.deletedocument) method.</span></span>  

    this.client.delete("/dbs/familydb/colls/familycoll/docs/Andersen.1", null);

## <span data-ttu-id="b5553-173"><a id="DeleteDatabase"></a>10. lépés: Az adatbázis törlése</span><span class="sxs-lookup"><span data-stu-id="b5553-173"><a id="DeleteDatabase"></a>Step 10: Delete the database</span></span>
<span data-ttu-id="b5553-174">A létrehozott adatbázis törlésével az adatbázis és az összes gyermekerőforrás (gyűjtemények, dokumentumok stb.) is törlődik.</span><span class="sxs-lookup"><span data-stu-id="b5553-174">Deleting the created database removes the database and all children resources (collections, documents, etc.).</span></span>

    this.client.deleteDatabase("/dbs/familydb", null);

## <span data-ttu-id="b5553-175"><a id="Run"></a>11. lépés: Futtassa a teljes Java konzolalkalmazást!</span><span class="sxs-lookup"><span data-stu-id="b5553-175"><a id="Run"></a>Step 11: Run your Java console application all together!</span></span>
<span data-ttu-id="b5553-176">Az alkalmazás futtatásához a konzolról, keresse meg a projekt mappát, és fordítási Maven használatával:</span><span class="sxs-lookup"><span data-stu-id="b5553-176">To run the application from the console, navigate to the project folder and compile using Maven:</span></span>
    
    mvn package

<span data-ttu-id="b5553-177">A `mvn package` futtatása letölti a legújabb Azure Cosmos DB-erőforrástárat a Mavenről, és létrehozza a `GetStarted-0.0.1-SNAPSHOT.jar` fájlt.</span><span class="sxs-lookup"><span data-stu-id="b5553-177">Running `mvn package` downloads the latest Azure Cosmos DB library from Maven and produces `GetStarted-0.0.1-SNAPSHOT.jar`.</span></span> <span data-ttu-id="b5553-178">Ezután futtassa az alkalmazást az alábbi paranccsal:</span><span class="sxs-lookup"><span data-stu-id="b5553-178">Then run the app by running:</span></span>

    mvn exec:java -D exec.mainClass=GetStarted.Program

<span data-ttu-id="b5553-179">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="b5553-179">Congratulations!</span></span> <span data-ttu-id="b5553-180">Elvégezte a NoSQL-oktatóanyagot, és egy működőképes Java konzolalkalmazással rendelkezik!</span><span class="sxs-lookup"><span data-stu-id="b5553-180">You've completed this NoSQL tutorial and have a working Java console application!</span></span>

## <a name="next-steps"></a><span data-ttu-id="b5553-181">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b5553-181">Next steps</span></span>
* <span data-ttu-id="b5553-182">Szüksége van egy Java-webalkalmazás létrehozására vonatkozó oktatóanyagra?</span><span class="sxs-lookup"><span data-stu-id="b5553-182">Want a Java web app tutorial?</span></span> <span data-ttu-id="b5553-183">Tekintse meg a [Java-webalkalmazás létrehozása az Azure Cosmos DB használatával](documentdb-java-application.md) című cikket.</span><span class="sxs-lookup"><span data-stu-id="b5553-183">See [Build a web application with Java using Azure Cosmos DB](documentdb-java-application.md).</span></span>
* <span data-ttu-id="b5553-184">Ismerje meg, hogyan [figyelhet egy Azure Cosmos DB-fiókot](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="b5553-184">Learn how to [monitor an Azure Cosmos DB account](monitor-accounts.md).</span></span>
* <span data-ttu-id="b5553-185">Futtasson lekérdezéseket a minta-adatkészleteken a [Query Playground](https://www.documentdb.com/sql/demo) (Tesztlekérdezések) használatával.</span><span class="sxs-lookup"><span data-stu-id="b5553-185">Run queries against our sample dataset in the [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="b5553-186">A programozási modellel kapcsolatos további tudnivalókat az [Azure Cosmos DB-dokumentációs oldalának](https://azure.microsoft.com/documentation/services/documentdb/) Develop (Fejlesztés) szakaszában találja.</span><span class="sxs-lookup"><span data-stu-id="b5553-186">Learn more about the programming model in the Develop section of the [Azure Cosmos DB documentation page](https://azure.microsoft.com/documentation/services/documentdb/).</span></span>

[keys]: media/documentdb-get-started/nosql-tutorial-keys.png
