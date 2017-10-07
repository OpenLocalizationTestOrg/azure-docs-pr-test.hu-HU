---
title: "NoSQL-oktatóanyag: DocumentDB API Azure Cosmos DB Java SDK |} Microsoft Docs"
description: "NoSQL-oktatóanyag, amely létrehoz egy online adatbázist és a DocumentDB API hello használata Azure Cosmos DB Java Konzolalkalmazás. Az Azure DocumentDB egy NoSQL-alapú adatbázis a JSON formátumhoz."
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
ms.openlocfilehash: 1a298a15ab911d140b9df30ad52cfe0fa07c55b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="nosql-tutorial-build-a-documentdb-api-java-console-application"></a><span data-ttu-id="ec0d3-105">NoSQL-oktatóanyag: a DocumentDB API Java Konzolalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ec0d3-105">NoSQL tutorial: Build a DocumentDB API Java console application</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ec0d3-106">.NET</span><span class="sxs-lookup"><span data-stu-id="ec0d3-106">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="ec0d3-107">.NET Core</span><span class="sxs-lookup"><span data-stu-id="ec0d3-107">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="ec0d3-108">Node.js MongoDB-hez</span><span class="sxs-lookup"><span data-stu-id="ec0d3-108">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="ec0d3-109">Node.js</span><span class="sxs-lookup"><span data-stu-id="ec0d3-109">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="ec0d3-110">Java</span><span class="sxs-lookup"><span data-stu-id="ec0d3-110">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="ec0d3-111">C++</span><span class="sxs-lookup"><span data-stu-id="ec0d3-111">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 

<span data-ttu-id="ec0d3-112">Toohello NoSQL-oktatóanyag a DocumentDB API hello Azure Cosmos DB Java SDK – Üdvözöljük!</span><span class="sxs-lookup"><span data-stu-id="ec0d3-112">Welcome toohello NoSQL tutorial for hello DocumentDB API for Azure Cosmos DB Java SDK!</span></span> <span data-ttu-id="ec0d3-113">Az oktatóanyag lépéseinek követésével egy olyan konzolalkalmazást készít, amely Azure Cosmos DB-erőforrásokat hoz létre és kérdez le.</span><span class="sxs-lookup"><span data-stu-id="ec0d3-113">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources.</span></span>

<span data-ttu-id="ec0d3-114">Az oktatóanyag a következőket ismerteti:</span><span class="sxs-lookup"><span data-stu-id="ec0d3-114">We cover:</span></span>

* <span data-ttu-id="ec0d3-115">Hoz létre és csatlakoztatja tooan Azure Cosmos DB fiók</span><span class="sxs-lookup"><span data-stu-id="ec0d3-115">Creating and connecting tooan Azure Cosmos DB account</span></span>
* <span data-ttu-id="ec0d3-116">A Visual Studio megoldás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ec0d3-116">Configuring your Visual Studio Solution</span></span>
* <span data-ttu-id="ec0d3-117">Online adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="ec0d3-117">Creating an online database</span></span>
* <span data-ttu-id="ec0d3-118">Gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="ec0d3-118">Creating a collection</span></span>
* <span data-ttu-id="ec0d3-119">JSON-dokumentumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="ec0d3-119">Creating JSON documents</span></span>
* <span data-ttu-id="ec0d3-120">Hello gyűjtemény lekérdezése</span><span class="sxs-lookup"><span data-stu-id="ec0d3-120">Querying hello collection</span></span>
* <span data-ttu-id="ec0d3-121">JSON-dokumentumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="ec0d3-121">Creating JSON documents</span></span>
* <span data-ttu-id="ec0d3-122">Hello gyűjtemény lekérdezése</span><span class="sxs-lookup"><span data-stu-id="ec0d3-122">Querying hello collection</span></span>
* <span data-ttu-id="ec0d3-123">Dokumentum cseréje</span><span class="sxs-lookup"><span data-stu-id="ec0d3-123">Replacing a document</span></span>
* <span data-ttu-id="ec0d3-124">Dokumentum törlése</span><span class="sxs-lookup"><span data-stu-id="ec0d3-124">Deleting a document</span></span>
* <span data-ttu-id="ec0d3-125">Hello adatbázis törlése</span><span class="sxs-lookup"><span data-stu-id="ec0d3-125">Deleting hello database</span></span>

<span data-ttu-id="ec0d3-126">Most pedig lássunk neki!</span><span class="sxs-lookup"><span data-stu-id="ec0d3-126">Now let's get started!</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ec0d3-127">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ec0d3-127">Prerequisites</span></span>
<span data-ttu-id="ec0d3-128">Győződjön meg arról, hogy a következő hello:</span><span class="sxs-lookup"><span data-stu-id="ec0d3-128">Make sure you have hello following:</span></span>

* <span data-ttu-id="ec0d3-129">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="ec0d3-129">An active Azure account.</span></span> <span data-ttu-id="ec0d3-130">Ha még nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="ec0d3-130">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span> <span data-ttu-id="ec0d3-131">Másik lehetőségként használhatja a hello [Azure Cosmos DB emulátor](local-emulator.md) ehhez az oktatóanyaghoz.</span><span class="sxs-lookup"><span data-stu-id="ec0d3-131">Alternatively, you can use hello [Azure Cosmos DB Emulator](local-emulator.md) for this tutorial.</span></span>
* [<span data-ttu-id="ec0d3-132">Git</span><span class="sxs-lookup"><span data-stu-id="ec0d3-132">Git</span></span>](https://git-scm.com/downloads)
* <span data-ttu-id="ec0d3-133">[Java fejlesztői készlet (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="ec0d3-133">[Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span>
* <span data-ttu-id="ec0d3-134">[Maven](http://maven.apache.org/download.cgi).</span><span class="sxs-lookup"><span data-stu-id="ec0d3-134">[Maven](http://maven.apache.org/download.cgi).</span></span>

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="ec0d3-135">1. lépés: Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="ec0d3-135">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="ec0d3-136">Hozzunk létre egy Azure Cosmos DB-fiókot.</span><span class="sxs-lookup"><span data-stu-id="ec0d3-136">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="ec0d3-137">Ha már rendelkezik toouse kívánt fiókkal, akkor kihagyhatja azokat, amelyek túl[Klónozás hello GitHub projekt](#GitClone).</span><span class="sxs-lookup"><span data-stu-id="ec0d3-137">If you already have an account you want toouse, you can skip ahead too[Clone hello GitHub project](#GitClone).</span></span> <span data-ttu-id="ec0d3-138">Ha hello Azure Cosmos DB Emulator használata esetén kövesse a hello készítésével [Azure Cosmos DB emulátor](local-emulator.md) tooset hello emulátor kialakításához, és hagyja ki azokat, amelyek túl[Klónozás hello GitHub projekt](#GitClone).</span><span class="sxs-lookup"><span data-stu-id="ec0d3-138">If you are using hello Azure Cosmos DB Emulator, follow hello steps at [Azure Cosmos DB Emulator](local-emulator.md) tooset up hello emulator and skip ahead too[Clone hello GitHub project](#GitClone).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="ec0d3-139"><a id="GitClone"></a>2. lépés: Másolat hello GitHub-projekt</span><span class="sxs-lookup"><span data-stu-id="ec0d3-139"><a id="GitClone"></a>Step 2: Clone hello GitHub project</span></span>
<span data-ttu-id="ec0d3-140">Ismerkedés a GitHub-tárházban hello klónozásával [Ismerkedés az Azure Cosmos DB és Java](https://github.com/Azure-Samples/documentdb-java-getting-started).</span><span class="sxs-lookup"><span data-stu-id="ec0d3-140">You can get started by cloning hello GitHub repository for [Get Started with Azure Cosmos DB and Java](https://github.com/Azure-Samples/documentdb-java-getting-started).</span></span> <span data-ttu-id="ec0d3-141">Például egy helyi könyvtárból futtassa a következő tooretrieve hello minta-projekt helyi hello.</span><span class="sxs-lookup"><span data-stu-id="ec0d3-141">For example, from a local directory run hello following tooretrieve hello sample project locally.</span></span>

    git clone git@github.com:Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git

    cd azure-cosmos-db-documentdb-java-getting-started

<span data-ttu-id="ec0d3-142">hello könyvtár neve tartalmazza a `pom.xml` hello projekt és egy `src` Java forrás kód beleértve tartalmazó mappa `Program.java` mely bemutatja hogyan rendelkező Azure Cosmos DB például a dokumentumok létrehozása és adatainak lekérdezése egyszerű műveleteket hajtanak végre egy gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="ec0d3-142">hello directory contains a `pom.xml` for hello project and a `src` folder containing Java source code including `Program.java` which shows how perform simple operations with Azure Cosmos DB like creating documents and querying data within a collection.</span></span> <span data-ttu-id="ec0d3-143">Hello `pom.xml` hello függőség tartalmaz [DocumentDB Java SDK a Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb).</span><span class="sxs-lookup"><span data-stu-id="ec0d3-143">hello `pom.xml` includes a dependency on hello [DocumentDB Java SDK on Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb).</span></span>

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-documentdb</artifactId>
        <version>LATEST</version>
    </dependency>

## <span data-ttu-id="ec0d3-144"><a id="Connect"></a>3. lépés: Csatlakozás tooan Azure Cosmos DB fiók</span><span class="sxs-lookup"><span data-stu-id="ec0d3-144"><a id="Connect"></a>Step 3: Connect tooan Azure Cosmos DB account</span></span>
<span data-ttu-id="ec0d3-145">A következő head biztonsági toohello [Azure Portal](https://portal.azure.com) tooretrieve a végpont és az elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="ec0d3-145">Next, head back toohello [Azure Portal](https://portal.azure.com) tooretrieve your endpoint and primary master key.</span></span> <span data-ttu-id="ec0d3-146">hello Azure Cosmos DB végpont és elsődleges kulcs szükség az alkalmazás toounderstand ahol tooconnect esetén és Azure Cosmos DB tootrust az alkalmazás által létesített kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="ec0d3-146">hello Azure Cosmos DB endpoint and primary key are necessary for your application toounderstand where tooconnect to, and for Azure Cosmos DB tootrust your application's connection.</span></span>

<span data-ttu-id="ec0d3-147">A hello Azure portál, keresse meg a tooyour Azure Cosmos DB fiókot, és kattintson **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="ec0d3-147">In hello Azure Portal, navigate tooyour Azure Cosmos DB account, and then click **Keys**.</span></span> <span data-ttu-id="ec0d3-148">Hello portálról hello URI másolja és illessze be azt `https://FILLME.documents.azure.com` hello Program.java fájlban.</span><span class="sxs-lookup"><span data-stu-id="ec0d3-148">Copy hello URI from hello portal and paste it into `https://FILLME.documents.azure.com` in hello Program.java file.</span></span> <span data-ttu-id="ec0d3-149">Ezután másolási elsődleges kulcs hello hello portálról, és illessze be azt `FILLME`.</span><span class="sxs-lookup"><span data-stu-id="ec0d3-149">Then copy hello PRIMARY KEY from hello portal and paste it into `FILLME`.</span></span>

    this.client = new DocumentClient(
        "https://FILLME.documents.azure.com",
        "FILLME"
        , new ConnectionPolicy(),
        ConsistencyLevel.Session);

![Képernyőfelvétel a hello hello NoSQL-oktatóanyag toocreate konzol Java-alkalmazások által használt Azure-portálról.][keys]

## <a name="step-4-create-a-database"></a><span data-ttu-id="ec0d3-152">4. lépés: Adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="ec0d3-152">Step 4: Create a database</span></span>
<span data-ttu-id="ec0d3-153">Az Azure Cosmos DB [adatbázis](documentdb-resources.md#databases) hello segítségével hozhatók létre [createDatabase](/java/api/com.microsoft.azure.documentdb._document_client.createdatabase) hello metódusában **DocumentClient** osztály.</span><span class="sxs-lookup"><span data-stu-id="ec0d3-153">Your Azure Cosmos DB [database](documentdb-resources.md#databases) can be created by using hello [createDatabase](/java/api/com.microsoft.azure.documentdb._document_client.createdatabase) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="ec0d3-154">Egy adatbázis a JSON-dokumentumtároló gyűjtemények között particionált logikai tárolója hello.</span><span class="sxs-lookup"><span data-stu-id="ec0d3-154">A database is hello logical container of JSON document storage partitioned across collections.</span></span>

    Database database = new Database();
    database.setId("familydb");
    this.client.createDatabase(database, null);

## <span data-ttu-id="ec0d3-155"><a id="CreateColl"></a>5. lépés: Gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="ec0d3-155"><a id="CreateColl"></a>Step 5: Create a collection</span></span>
> [!WARNING]
> <span data-ttu-id="ec0d3-156">A **createCollection** létrehoz egy fenntartott adattovábbítási kapacitással rendelkező új gyűjteményt, amely költségeket von maga után.</span><span class="sxs-lookup"><span data-stu-id="ec0d3-156">**createCollection** creates a new collection with reserved throughput, which has pricing implications.</span></span> <span data-ttu-id="ec0d3-157">További részletekért látogasson el az [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="ec0d3-157">For more details, visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span>
> 
> 

<span data-ttu-id="ec0d3-158">A [gyűjtemény](documentdb-resources.md#collections) hello segítségével hozhatók létre [createCollection](/java/api/com.microsoft.azure.documentdb._document_client.createcollection) hello metódusában **DocumentClient** osztály.</span><span class="sxs-lookup"><span data-stu-id="ec0d3-158">A [collection](documentdb-resources.md#collections) can be created by using hello [createCollection](/java/api/com.microsoft.azure.documentdb._document_client.createcollection) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="ec0d3-159">A gyűjtemény egy JSON-dokumentumokat és a kapcsolódó JavaScript-alkalmazáslogikát tartalmazó tároló.</span><span class="sxs-lookup"><span data-stu-id="ec0d3-159">A collection is a container of JSON documents and associated JavaScript application logic.</span></span>


    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId("familycoll");

    // Azure Cosmos DB collections can be reserved with throughput specified in request units/second. 
    // Here we create a collection with 400 RU/s.
    RequestOptions requestOptions = new RequestOptions();
    requestOptions.setOfferThroughput(400);

    this.client.createCollection("/dbs/familydb", collectionInfo, requestOptions);

## <span data-ttu-id="ec0d3-160"><a id="CreateDoc"></a>6. lépés: JSON-dokumentumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="ec0d3-160"><a id="CreateDoc"></a>Step 6: Create JSON documents</span></span>
<span data-ttu-id="ec0d3-161">A [dokumentum](documentdb-resources.md#documents) hello segítségével hozhatók létre [Documentclient](/java/api/com.microsoft.azure.documentdb._document_client.createdocument) hello metódusában **DocumentClient** osztály.</span><span class="sxs-lookup"><span data-stu-id="ec0d3-161">A [document](documentdb-resources.md#documents) can be created by using hello [createDocument](/java/api/com.microsoft.azure.documentdb._document_client.createdocument) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="ec0d3-162">A dokumentumok a felhasználó által megadott (tetszőleges) JSON-tartalmak.</span><span class="sxs-lookup"><span data-stu-id="ec0d3-162">Documents are user-defined (arbitrary) JSON content.</span></span> <span data-ttu-id="ec0d3-163">Most már beilleszthetünk egy vagy több dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="ec0d3-163">We can now insert one or more documents.</span></span> <span data-ttu-id="ec0d3-164">Ha van olyan adat, milyen toostore az adatbázisban, használhatja az Azure Cosmos DB [adatáttelepítési eszközét](import-data.md) tooimport hello adatokat az adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="ec0d3-164">If you already have data you'd like toostore in your database, you can use Azure Cosmos DB's [Data Migration tool](import-data.md) tooimport hello data into a database.</span></span>

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

![Diagram szemléltető hello hierarchikus kapcsolatát hello fiók, hello online adatbázis, gyűjtemény hello és hello NoSQL-oktatóanyag toocreate konzol Java-alkalmazások által használt hello dokumentumok](./media/documentdb-get-started/nosql-tutorial-account-database.png)

## <span data-ttu-id="ec0d3-166"><a id="Query"></a>7. lépés: Az Azure Cosmos DB-erőforrások lekérdezése</span><span class="sxs-lookup"><span data-stu-id="ec0d3-166"><a id="Query"></a>Step 7: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="ec0d3-167">Az Azure Cosmos DB támogatja az egyes gyűjteményekben tárolt JSON-dokumentumokon végzett [részletes lekérdezéseket](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="ec0d3-167">Azure Cosmos DB supports rich [queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span>  <span data-ttu-id="ec0d3-168">hello a következő mintakód bemutatja, hogyan tooquery dokumentumokat az Azure Cosmos Adatbázisba SQL-szintaxis használata hello [queryDocuments](/java/api/com.microsoft.azure.documentdb._document_client.querydocuments) metódust.</span><span class="sxs-lookup"><span data-stu-id="ec0d3-168">hello following sample code shows how tooquery documents in Azure Cosmos DB using SQL syntax with hello [queryDocuments](/java/api/com.microsoft.azure.documentdb._document_client.querydocuments) method.</span></span>

    FeedResponse<Document> queryResults = this.client.queryDocuments(
        "/dbs/familydb/colls/familycoll",
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", 
        null);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }

## <span data-ttu-id="ec0d3-169"><a id="ReplaceDocument"></a>8. lépés: JSON-dokumentumok cseréje</span><span class="sxs-lookup"><span data-stu-id="ec0d3-169"><a id="ReplaceDocument"></a>Step 8: Replace JSON document</span></span>
<span data-ttu-id="ec0d3-170">Azure Cosmos-adatbázis frissítése JSON-dokumentumok hello segítségével támogatja [replaceDocument](/java/api/com.microsoft.azure.documentdb._document_client.replacedocument) metódust.</span><span class="sxs-lookup"><span data-stu-id="ec0d3-170">Azure Cosmos DB supports updating JSON documents using hello [replaceDocument](/java/api/com.microsoft.azure.documentdb._document_client.replacedocument) method.</span></span>

    // Update a property
    andersenFamily.Children[0].Grade = 6;

    this.client.replaceDocument(
        "/dbs/familydb/colls/familycoll/docs/Andersen.1", 
        andersenFamily,
        null);

## <span data-ttu-id="ec0d3-171"><a id="DeleteDocument"></a>9. lépés: JSON-dokumentumok törlése</span><span class="sxs-lookup"><span data-stu-id="ec0d3-171"><a id="DeleteDocument"></a>Step 9: Delete JSON document</span></span>
<span data-ttu-id="ec0d3-172">Hasonlóképpen, Azure Cosmos DB támogatja a JSON-dokumentumok törlését hello segítségével [deleteDocument](/java/api/com.microsoft.azure.documentdb._document_client.deletedocument) metódust.</span><span class="sxs-lookup"><span data-stu-id="ec0d3-172">Similarly, Azure Cosmos DB supports deleting JSON documents using hello [deleteDocument](/java/api/com.microsoft.azure.documentdb._document_client.deletedocument) method.</span></span>  

    this.client.delete("/dbs/familydb/colls/familycoll/docs/Andersen.1", null);

## <span data-ttu-id="ec0d3-173"><a id="DeleteDatabase"></a>10. lépés: Hello adatbázis törlése</span><span class="sxs-lookup"><span data-stu-id="ec0d3-173"><a id="DeleteDatabase"></a>Step 10: Delete hello database</span></span>
<span data-ttu-id="ec0d3-174">Törlése hello létrehozott adatbázis eltávolítja hello adatbázis és az összes gyermekerőforrás (gyűjtemények, dokumentumok stb.).</span><span class="sxs-lookup"><span data-stu-id="ec0d3-174">Deleting hello created database removes hello database and all children resources (collections, documents, etc.).</span></span>

    this.client.deleteDatabase("/dbs/familydb", null);

## <span data-ttu-id="ec0d3-175"><a id="Run"></a>11. lépés: Futtassa a teljes Java konzolalkalmazást!</span><span class="sxs-lookup"><span data-stu-id="ec0d3-175"><a id="Run"></a>Step 11: Run your Java console application all together!</span></span>
<span data-ttu-id="ec0d3-176">toorun hello alkalmazás hello-konzolon keresse meg a toohello projektmappa és fordítási Maven használatával:</span><span class="sxs-lookup"><span data-stu-id="ec0d3-176">toorun hello application from hello console, navigate toohello project folder and compile using Maven:</span></span>
    
    mvn package

<span data-ttu-id="ec0d3-177">Futó `mvn package` hello legújabb Azure Cosmos DB könyvtár Maven tölt le, és hozza létre `GetStarted-0.0.1-SNAPSHOT.jar`.</span><span class="sxs-lookup"><span data-stu-id="ec0d3-177">Running `mvn package` downloads hello latest Azure Cosmos DB library from Maven and produces `GetStarted-0.0.1-SNAPSHOT.jar`.</span></span> <span data-ttu-id="ec0d3-178">Ezután futtassa a hello alkalmazás futtatásával:</span><span class="sxs-lookup"><span data-stu-id="ec0d3-178">Then run hello app by running:</span></span>

    mvn exec:java -D exec.mainClass=GetStarted.Program

<span data-ttu-id="ec0d3-179">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="ec0d3-179">Congratulations!</span></span> <span data-ttu-id="ec0d3-180">Elvégezte a NoSQL-oktatóanyagot, és egy működőképes Java konzolalkalmazással rendelkezik!</span><span class="sxs-lookup"><span data-stu-id="ec0d3-180">You've completed this NoSQL tutorial and have a working Java console application!</span></span>

## <a name="next-steps"></a><span data-ttu-id="ec0d3-181">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ec0d3-181">Next steps</span></span>
* <span data-ttu-id="ec0d3-182">Szüksége van egy Java-webalkalmazás létrehozására vonatkozó oktatóanyagra?</span><span class="sxs-lookup"><span data-stu-id="ec0d3-182">Want a Java web app tutorial?</span></span> <span data-ttu-id="ec0d3-183">Tekintse meg a [Java-webalkalmazás létrehozása az Azure Cosmos DB használatával](documentdb-java-application.md) című cikket.</span><span class="sxs-lookup"><span data-stu-id="ec0d3-183">See [Build a web application with Java using Azure Cosmos DB](documentdb-java-application.md).</span></span>
* <span data-ttu-id="ec0d3-184">Ismerje meg, hogyan túl[figyelése Azure Cosmos DB fiók](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="ec0d3-184">Learn how too[monitor an Azure Cosmos DB account](monitor-accounts.md).</span></span>
* <span data-ttu-id="ec0d3-185">A minta-adatkészleteken hello lekérdezéseinek futtatásához [Tesztlekérdezéseket](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="ec0d3-185">Run queries against our sample dataset in hello [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="ec0d3-186">További tudnivalók a programozási modellt hello hello hello Develop szakasza [Azure Cosmos DB dokumentációs oldal](https://azure.microsoft.com/documentation/services/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="ec0d3-186">Learn more about hello programming model in hello Develop section of hello [Azure Cosmos DB documentation page](https://azure.microsoft.com/documentation/services/documentdb/).</span></span>

[keys]: media/documentdb-get-started/nosql-tutorial-keys.png
