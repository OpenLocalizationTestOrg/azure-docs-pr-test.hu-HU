---
title: "az Azure Cosmos DB aaaServer ügyféloldali JavaScript programozási |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure Cosmos DB toowrite tárolt eljárások, adatbázis-eseményindítók és felhasználó által megadott funkciókat (UDF) a JavaScript. Adatbázis programing tippeket és több kapják meg."
keywords: "Adatbázis-eseményindítók, tárolt eljárás, tárolt eljárás, adatbázis program, sproc, a documentdb, azure, a Microsoft azure"
services: cosmos-db
documentationcenter: 
author: aliuy
manager: jhubbard
editor: mimig
ms.assetid: 0fba7ebd-a4fc-4253-a786-97f1354fbf17
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: andrl
ms.openlocfilehash: 5a011d1c4b0b5908d5de73607a1bc328ed1711d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-server-side-programming-stored-procedures-database-triggers-and-udfs"></a><span data-ttu-id="a13f4-105">Az Azure Cosmos DB kiszolgálóoldali programozása: tárolt eljárások, eseményindítók adatbázis és a felhasználó által megadott függvények</span><span class="sxs-lookup"><span data-stu-id="a13f4-105">Azure Cosmos DB server-side programming: Stored procedures, database triggers, and UDFs</span></span>
<span data-ttu-id="a13f4-106">Ismerje meg, hogy Azure Cosmos DB nyelvi integrálva, a tranzakciós végrehajtását a JavaScript lehetővé teszi, hogy a fejlesztők írási **tárolt eljárások**, **eseményindítók** és **felhasználó által definiált funkciókat (UDF)** a natív módon egy [ECMAScript 2015](http://www.ecma-international.org/ecma-262/6.0/) JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a13f4-106">Learn how Azure Cosmos DB’s language integrated, transactional execution of JavaScript lets developers write **stored procedures**, **triggers** and **user defined functions (UDFs)** natively in an [ECMAScript 2015](http://www.ecma-international.org/ecma-262/6.0/) JavaScript.</span></span> <span data-ttu-id="a13f4-107">Ez lehetővé teszi toowrite adatbázis program alkalmazáslogikát szállított és végre közvetlenül a hello adatbázis adattárolási partíciókat.</span><span class="sxs-lookup"><span data-stu-id="a13f4-107">This allows you toowrite database program application logic that can be shipped and executed directly on hello database storage partitions.</span></span> 

<span data-ttu-id="a13f4-108">Ajánlott indította a következő néznek hello videó, ahol Andrew Liu egy rövid bevezető tooCosmos DB tartozó kiszolgálóoldali adatbázis programozási modellt biztosít.</span><span class="sxs-lookup"><span data-stu-id="a13f4-108">We recommend getting started by watching hello following video, where Andrew Liu provides a brief introduction tooCosmos DB's server-side database programming model.</span></span> 

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Demo-A-Quick-Intro-to-Azure-DocumentDBs-Server-Side-Javascript/player]
> 
> 

<span data-ttu-id="a13f4-109">Ezt követően térjen vissza toothis cikk, ahol megtudhatja, hello válaszok toohello a következő kérdéseket:</span><span class="sxs-lookup"><span data-stu-id="a13f4-109">Then, return toothis article, where you'll learn hello answers toohello following questions:</span></span>  

* <span data-ttu-id="a13f4-110">Hogyan írni egy tárolt eljárás, eseményindító vagy használatával JavaScript UDF?</span><span class="sxs-lookup"><span data-stu-id="a13f4-110">How do I write a a stored procedure, trigger, or UDF using JavaScript?</span></span>
* <span data-ttu-id="a13f4-111">Hogyan Cosmos DB sav garantálni?</span><span class="sxs-lookup"><span data-stu-id="a13f4-111">How does Cosmos DB guarantee ACID?</span></span>
* <span data-ttu-id="a13f4-112">Hogyan működik a Cosmos DB tranzakciók?</span><span class="sxs-lookup"><span data-stu-id="a13f4-112">How do transactions work in Cosmos DB?</span></span>
* <span data-ttu-id="a13f4-113">Mik azok a előre elindítja és utáni indítása, és hogyan hajtsa végre írási egy?</span><span class="sxs-lookup"><span data-stu-id="a13f4-113">What are pre-triggers and post-triggers and how do I write one?</span></span>
* <span data-ttu-id="a13f4-114">Hogyan regisztrálja és futtatni egy tárolt eljárás, eseményindító vagy UDF RESTful módon HTTP-n keresztül?</span><span class="sxs-lookup"><span data-stu-id="a13f4-114">How do I register and execute a stored procedure, trigger, or UDF in a RESTful manner by using HTTP?</span></span>
* <span data-ttu-id="a13f4-115">Mi Cosmos DB SDK elérhető toocreate és végrehajtani tárolt eljárások, eseményindítók és felhasználó által megadott függvények?</span><span class="sxs-lookup"><span data-stu-id="a13f4-115">What Cosmos DB SDKs are available toocreate and execute stored procedures, triggers, and UDFs?</span></span>

## <a name="introduction-toostored-procedure-and-udf-programming"></a><span data-ttu-id="a13f4-116">Bevezetés tooStored eljárás és UDF programozási</span><span class="sxs-lookup"><span data-stu-id="a13f4-116">Introduction tooStored Procedure and UDF Programming</span></span>
<span data-ttu-id="a13f4-117">Ez a megközelítés a *"JavaScript egy T-SQL modern napot"* felszabadítja a rendszer típuseltérést és objektum-relációs leképezés technológiák hello bonyolultságára alkalmazásfejlesztők.</span><span class="sxs-lookup"><span data-stu-id="a13f4-117">This approach of *“JavaScript as a modern day T-SQL”* frees application developers from hello complexities of type system mismatches and object-relational mapping technologies.</span></span> <span data-ttu-id="a13f4-118">Azt is, amelyek magas kihasználtsággal rendelkező toobuild gazdag alkalmazások lehetnek belső előnyeit számos:</span><span class="sxs-lookup"><span data-stu-id="a13f4-118">It also has a number of intrinsic advantages that can be utilized toobuild rich applications:</span></span>  

* <span data-ttu-id="a13f4-119">**Eljárási logika:** egy magas szintű programozási nyelv, JavaScript biztosít a felület gazdag és ismerős tooexpress üzleti logikát.</span><span class="sxs-lookup"><span data-stu-id="a13f4-119">**Procedural Logic:** JavaScript as a high level programming language, provides a rich and familiar interface tooexpress business logic.</span></span> <span data-ttu-id="a13f4-120">Műveletek szorosabb toohello adatok összetett sorozatát végezheti el.</span><span class="sxs-lookup"><span data-stu-id="a13f4-120">You can perform complex sequences of operations closer toohello data.</span></span>
* <span data-ttu-id="a13f4-121">**Az atomi tranzakciók:** Cosmos DB biztosítja, hogy az adatbázis-műveletet hajtott végre egy tárolt eljárás vagy eseményindító belül atomi.</span><span class="sxs-lookup"><span data-stu-id="a13f4-121">**Atomic Transactions:** Cosmos DB guarantees that database operations performed inside a single stored procedure or trigger are atomic.</span></span> <span data-ttu-id="a13f4-122">Ez lehetővé teszi, hogy egy alkalmazás egyesítése egyetlen kötegben kapcsolódó műveleteket, hogy az összes sikeres legyen, vagy egyiket sem sikerült.</span><span class="sxs-lookup"><span data-stu-id="a13f4-122">This lets an application combine related operations in a single batch so that either all of them succeed or none of them succeed.</span></span> 
* <span data-ttu-id="a13f4-123">**Teljesítmény:** hello tényt, hogy JSON belsőleg csatlakoztatott toohello Javascript nyelv típusrendszernek, is hello alapvető egysége a Cosmos DB tárolási lehetővé teszi, hogy például a JSON a Lusta materialization optimalizálásokat számos hello pufferben lévő dokumentumok készlet és minősítené elérhető igény szerinti toohello kód végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="a13f4-123">**Performance:** hello fact that JSON is intrinsically mapped toohello Javascript language type system and is also hello basic unit of storage in Cosmos DB allows for a number of optimizations like lazy materialization of JSON documents in hello buffer pool and making them available on-demand toohello executing code.</span></span> <span data-ttu-id="a13f4-124">Nincsenek további teljesítménybeli előnyökben szállítási üzleti logika toohello adatbázishoz tartozó:</span><span class="sxs-lookup"><span data-stu-id="a13f4-124">There are more performance benefits associated with shipping business logic toohello database:</span></span>
  
  * <span data-ttu-id="a13f4-125">Kötegelés – fejlesztők csoport műveletek tartoznak, mint a beszúrások, és a tömeges küldheti el ezeket.</span><span class="sxs-lookup"><span data-stu-id="a13f4-125">Batching – Developers can group operations like inserts and submit them in bulk.</span></span> <span data-ttu-id="a13f4-126">hello hálózati forgalom késés költségeket, és hello áruház általános toocreate külön tranzakciók jelentős mértékben csökken.</span><span class="sxs-lookup"><span data-stu-id="a13f4-126">hello network traffic latency cost and hello store overhead toocreate separate transactions are reduced significantly.</span></span> 
  * <span data-ttu-id="a13f4-127">A fordítás előtti – Cosmos DB precompiles tárolt eljárások, eseményindítók és felhasználó által megadott funkciókat (UDF) tooavoid egyes elindításaihoz JavaScript-fordítási költség.</span><span class="sxs-lookup"><span data-stu-id="a13f4-127">Pre-compilation – Cosmos DB precompiles stored procedures, triggers and user defined functions (UDFs) tooavoid JavaScript compilation cost for each invocation.</span></span> <span data-ttu-id="a13f4-128">hello hello bájt kód kialakításának hello eljárási logikai érték terhelés amortized tooa minimális érték.</span><span class="sxs-lookup"><span data-stu-id="a13f4-128">hello overhead of building hello byte code for hello procedural logic is amortized tooa minimal value.</span></span>
  * <span data-ttu-id="a13f4-129">Alkalmazás-előkészítés – sok műveleteket kell egy mellékhatása ("eseményindító"), amely potenciálisan magában foglalja a egy vagy több másodlagos tároló műveleteket.</span><span class="sxs-lookup"><span data-stu-id="a13f4-129">Sequencing – Many operations need a side-effect (“trigger”) that potentially involves doing one or many secondary store operations.</span></span> <span data-ttu-id="a13f4-130">Vezérelt atomicity, ez a további performant áthelyezésekor toohello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="a13f4-130">Aside from atomicity, this is more performant when moved toohello server.</span></span> 
* <span data-ttu-id="a13f4-131">**Beágyazás:** a tárolt eljárások használt toogroup üzleti logika egy helyen lehet.</span><span class="sxs-lookup"><span data-stu-id="a13f4-131">**Encapsulation:** Stored procedures can be used toogroup business logic in one place.</span></span> <span data-ttu-id="a13f4-132">Ez a két előnnyel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="a13f4-132">This has two advantages:</span></span>
  * <span data-ttu-id="a13f4-133">Hozzáadja egy hardverabsztrakciós réteg hello nyers adatait, amely lehetővé teszi, hogy a fejlesztők tooevolve adatok az alkalmazások egymástól függetlenül hello adatokból felett.</span><span class="sxs-lookup"><span data-stu-id="a13f4-133">It adds an abstraction layer on top of hello raw data, which enables data architects tooevolve their applications independently from hello data.</span></span> <span data-ttu-id="a13f4-134">Ez akkor különösen hasznos, ha hello adatok séma nélküli, toohello rideg feltételek, amelyek esetleg bővíthetőség hello alkalmazásba, ha toodeal adatokkal rendelkeznek közvetlenül toobe miatt.</span><span class="sxs-lookup"><span data-stu-id="a13f4-134">This is particularly advantageous when hello data is schema-less, due toohello brittle assumptions that may need toobe baked into hello application if they have toodeal with data directly.</span></span>  
  * <span data-ttu-id="a13f4-135">Ez az absztrakció lehetővé teszi, hogy a vállalatok számára az adatok biztonsága egyszerűsítése hello a hozzáférést a hello parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="a13f4-135">This abstraction lets enterprises keep their data secure by streamlining hello access from hello scripts.</span></span>  

<span data-ttu-id="a13f4-136">hello létrehozását és adatbázis-eseményindítók, tárolt eljárás és egyéni lekérdezési operátorok végrehajtásának támogatott keresztül hello [REST API](/rest/api/documentdb/), [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases), és [ügyfél SDK-k](documentdb-sdk-dotnet.md) például .NET, Node.js és JavaScript számos platformon.</span><span class="sxs-lookup"><span data-stu-id="a13f4-136">hello creation and execution of database triggers, stored procedure and custom query operators is supported through hello [REST API](/rest/api/documentdb/), [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases), and [client SDKs](documentdb-sdk-dotnet.md) in many platforms including .NET, Node.js and JavaScript.</span></span>

<span data-ttu-id="a13f4-137">Ez az oktatóanyag használja hello [Node.js SDK-val Q tett](http://azure.github.io/azure-documentdb-node-q/) tooillustrate szintaxis és a tárolt eljárások, eseményindítók és felhasználó által megadott függvények használatát.</span><span class="sxs-lookup"><span data-stu-id="a13f4-137">This tutorial uses hello [Node.js SDK with Q Promises](http://azure.github.io/azure-documentdb-node-q/) tooillustrate syntax and usage of stored procedures, triggers, and UDFs.</span></span>   

## <a name="stored-procedures"></a><span data-ttu-id="a13f4-138">Tárolt eljárások</span><span class="sxs-lookup"><span data-stu-id="a13f4-138">Stored procedures</span></span>
### <a name="example-write-a-simple-stored-procedure"></a><span data-ttu-id="a13f4-139">Példa: Egy egyszerű tárolt eljárás írása</span><span class="sxs-lookup"><span data-stu-id="a13f4-139">Example: Write a simple stored procedure</span></span>
<span data-ttu-id="a13f4-140">Kezdjük egy egyszerű tárolt eljárás, amely a "Hello, World" választ ad vissza.</span><span class="sxs-lookup"><span data-stu-id="a13f4-140">Let’s start with a simple stored procedure that returns a “Hello World” response.</span></span>

    var helloWorldStoredProc = {
        id: "helloWorld",
        serverScript: function () {
            var context = getContext();
            var response = context.getResponse();

            response.setBody("Hello, World");
        }
    }


<span data-ttu-id="a13f4-141">Tárolt eljárások gyűjteményenként regisztrálva van, és a dokumentum és a mellékletek megtalálható a gyűjteményben is működik.</span><span class="sxs-lookup"><span data-stu-id="a13f4-141">Stored procedures are registered per collection, and can operate on any document and attachment present in that collection.</span></span> <span data-ttu-id="a13f4-142">hello alábbi kódrészletben láthatja, hogyan tooregister hello helloWorld tárolt eljárás gyűjtemény tartozik.</span><span class="sxs-lookup"><span data-stu-id="a13f4-142">hello following snippet shows how tooregister hello helloWorld stored procedure with a collection.</span></span> 

    // register hello stored procedure
    var createdStoredProcedure;
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', helloWorldStoredProc)
        .then(function (response) {
            createdStoredProcedure = response.resource;
            console.log("Successfully created stored procedure");
        }, function (error) {
            console.log("Error", error);
        });


<span data-ttu-id="a13f4-143">Miután hello tárolt eljárás regisztrálva van, azt is végre hello gyűjtemény ellen, tartózkodik, olvassa el hello vissza hello ügyfélen.</span><span class="sxs-lookup"><span data-stu-id="a13f4-143">Once hello stored procedure is registered, we can execute it against hello collection, and read hello results back at hello client.</span></span> 

    // execute hello stored procedure
    client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/helloWorld')
        .then(function (response) {
            console.log(response.result); // "Hello, World"
        }, function (err) {
            console.log("Error", error);
        });


<span data-ttu-id="a13f4-144">hello környezeti objektumot, amely a Cosmos adatbázis-terület végrehajtható, valamint a toohello kérelem-válasz objektumokhoz tooall műveletek hozzáférést biztosít.</span><span class="sxs-lookup"><span data-stu-id="a13f4-144">hello context object provides access tooall operations that can be performed on Cosmos DB storage, as well as access toohello request and response objects.</span></span> <span data-ttu-id="a13f4-145">Ebben az esetben hello objektum tooset hello választörzs a hello választ küldött vissza toohello ügyfél használtuk.</span><span class="sxs-lookup"><span data-stu-id="a13f4-145">In this case, we used hello response object tooset hello body of hello response that was sent back toohello client.</span></span> <span data-ttu-id="a13f4-146">További részletekért tekintse meg a toohello [Azure Cosmos DB JavaScript server SDK-dokumentáció](http://azure.github.io/azure-documentdb-js-server/).</span><span class="sxs-lookup"><span data-stu-id="a13f4-146">For more details, refer toohello [Azure Cosmos DB JavaScript server SDK documentation](http://azure.github.io/azure-documentdb-js-server/).</span></span>  

<span data-ttu-id="a13f4-147">Ossza meg velünk bontsa ki az ebben a példában, és több adatbázis-funkciók hozzáadása toohello tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="a13f4-147">Let us expand on this example and add more database related functionality toohello stored procedure.</span></span> <span data-ttu-id="a13f4-148">Tárolt eljárások létrehozása, frissítése, olvassa el, lekérdezheti és dokumentumok és mellékletek belül hello gyűjtemény törlése.</span><span class="sxs-lookup"><span data-stu-id="a13f4-148">Stored procedures can create, update, read, query and delete documents and attachments inside hello collection.</span></span>    

### <a name="example-write-a-stored-procedure-toocreate-a-document"></a><span data-ttu-id="a13f4-149">Példa: Írni egy tárolt eljárás toocreate dokumentum</span><span class="sxs-lookup"><span data-stu-id="a13f4-149">Example: Write a stored procedure toocreate a document</span></span>
<span data-ttu-id="a13f4-150">hello következő kódrészletben láthatja, hogyan toouse hello környezeti objektum toointeract Cosmos DB erőforrásokkal.</span><span class="sxs-lookup"><span data-stu-id="a13f4-150">hello next snippet shows how toouse hello context object toointeract with Cosmos DB resources.</span></span>

    var createDocumentStoredProc = {
        id: "createMyDocument",
        serverScript: function createMyDocument(documentToCreate) {
            var context = getContext();
            var collection = context.getCollection();

            var accepted = collection.createDocument(collection.getSelfLink(),
                  documentToCreate,
                  function (err, documentCreated) {
                      if (err) throw new Error('Error' + err.message);
                      context.getResponse().setBody(documentCreated.id)
                  });
            if (!accepted) return;
        }
    }


<span data-ttu-id="a13f4-151">A tárolt eljárás időt vesz igénybe, mint bemeneti documentToCreate, a dokumentum toobe hello aktuális gyűjtemény létrehozott hello törzsét.</span><span class="sxs-lookup"><span data-stu-id="a13f4-151">This stored procedure takes as input documentToCreate, hello body of a document toobe created in hello current collection.</span></span> <span data-ttu-id="a13f4-152">Ilyen műveletek aszinkron jellegűek, és a JavaScript függvény visszahívások függenek.</span><span class="sxs-lookup"><span data-stu-id="a13f4-152">All such operations are asynchronous and depend on JavaScript function callbacks.</span></span> <span data-ttu-id="a13f4-153">hello visszahívási függvény két paraméterrel, egy a hello hiba objektum rendelkezik, abban az esetben hello művelet sikertelen lesz, és egy hello a létrehozott objektum.</span><span class="sxs-lookup"><span data-stu-id="a13f4-153">hello callback function has two parameters, one for hello error object in case hello operation fails, and one for hello created object.</span></span> <span data-ttu-id="a13f4-154">Hello visszahívási belüli felhasználók hello kivétel kezeléséhez, vagy hibaüzenetet küldjön.</span><span class="sxs-lookup"><span data-stu-id="a13f4-154">Inside hello callback, users can either handle hello exception or throw an error.</span></span> <span data-ttu-id="a13f4-155">Egy visszahívás nem áll rendelkezésre, és nem sikerül, akkor hello Azure Cosmos DB futásidejű hibát jelez.</span><span class="sxs-lookup"><span data-stu-id="a13f4-155">In case a callback is not provided and there is an error, hello Azure Cosmos DB runtime throws an error.</span></span>   

<span data-ttu-id="a13f4-156">Hello a fenti példában a hello visszahívása hibát jelez, ha hello művelet sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="a13f4-156">In hello example above, hello callback throws an error if hello operation failed.</span></span> <span data-ttu-id="a13f4-157">Ellenkező esetben állítja a dokumentum létrehozott hello válasz toohello ügyfél hello törzsként hello hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="a13f4-157">Otherwise, it sets hello id of hello created document as hello body of hello response toohello client.</span></span> <span data-ttu-id="a13f4-158">Ez a tárolt eljárás bemeneti paraméterekkel végrehajtásának módját.</span><span class="sxs-lookup"><span data-stu-id="a13f4-158">Here is how this stored procedure is executed with input parameters.</span></span>

    // register hello stored procedure
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', createDocumentStoredProc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;

            // run stored procedure toocreate a document
            var docToCreate = {
                id: "DocFromSproc",
                book: "hello Hitchhiker’s Guide toohello Galaxy",
                author: "Douglas Adams"
            };

            return client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/createMyDocument',
                  docToCreate);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response); // "DocFromSproc"
    }, function (error) {
        console.log("Error", error);
    });


<span data-ttu-id="a13f4-159">Vegye figyelembe, hogy ez a tárolt eljárás is lehet módosított tootake dokumentum szervezetek bemeneti tömb, és hozzon létre azokat az összes hello ugyanaz a tárolt eljárás végrehajtása több hálózati helyett kérelmek toocreate minden ezek külön-külön.</span><span class="sxs-lookup"><span data-stu-id="a13f4-159">Note that this stored procedure can be modified tootake an array of document bodies as input and create them all in hello same stored procedure execution instead of multiple network requests toocreate each of them individually.</span></span> <span data-ttu-id="a13f4-160">Ez lehet használt tooimplement egy hatékony tömeges importáló a Cosmos DB (Ez az oktatóanyag későbbi részében bemutatása).</span><span class="sxs-lookup"><span data-stu-id="a13f4-160">This can be used tooimplement an efficient bulk importer for Cosmos DB (discussed later in this tutorial).</span></span>   

<span data-ttu-id="a13f4-161">leírt hello példa azt mutatja, hogyan toouse tárolt eljárások.</span><span class="sxs-lookup"><span data-stu-id="a13f4-161">hello example described demonstrated how toouse stored procedures.</span></span> <span data-ttu-id="a13f4-162">Bemutatjuk, eseményindítók és felhasználó által megadott funkciókat (UDF) hello oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="a13f4-162">We will cover triggers and user defined functions (UDFs) later in hello tutorial.</span></span>

## <a name="database-program-transactions"></a><span data-ttu-id="a13f4-163">Adatbázis-program tranzakciók</span><span class="sxs-lookup"><span data-stu-id="a13f4-163">Database program transactions</span></span>
<span data-ttu-id="a13f4-164">Egy tipikus adatbázisban tranzakció munka egyetlen logikai egységként végrehajtott műveletek sorozata adható meg.</span><span class="sxs-lookup"><span data-stu-id="a13f4-164">Transaction in a typical database can be defined as a sequence of operations performed as a single logical unit of work.</span></span> <span data-ttu-id="a13f4-165">Minden tranzakció biztosít **ACID garanciák**.</span><span class="sxs-lookup"><span data-stu-id="a13f4-165">Each transaction provides **ACID guarantees**.</span></span> <span data-ttu-id="a13f4-166">SAV egy jól ismert mozaikszó négy tulajdonságai – Atomicity, konzisztencia, elkülönítési és tartósságot jelző.</span><span class="sxs-lookup"><span data-stu-id="a13f4-166">ACID is a well-known acronym that stands for four properties -  Atomicity, Consistency, Isolation and Durability.</span></span>  

<span data-ttu-id="a13f4-167">Röviden atomicity garantálja, hogy egyetlen egységként kezelt-e a minden hello dolgozott tranzakción belül hol vagy az összes véglegesítve vagy "none".</span><span class="sxs-lookup"><span data-stu-id="a13f4-167">Briefly, atomicity guarantees that all hello work done inside a transaction is treated as a single unit where either all of it is committed or none.</span></span> <span data-ttu-id="a13f4-168">Konzisztencia lehetővé teszi, hogy hello adatok mindig a megfelelő belső állapotban tranzakciók között.</span><span class="sxs-lookup"><span data-stu-id="a13f4-168">Consistency makes sure that hello data is always in a good internal state across transactions.</span></span> <span data-ttu-id="a13f4-169">Elkülönítési garantálja, hogy két tranzakciók zavarják – általában, a legtöbb kereskedelmi rendszerek adjon meg több elkülönítési szinten használható hello alkalmazás igények alapján.</span><span class="sxs-lookup"><span data-stu-id="a13f4-169">Isolation guarantees that no two transactions interfere with each other – generally, most commercial systems provide multiple isolation levels that can be used based on hello application needs.</span></span> <span data-ttu-id="a13f4-170">Tartósságot biztosítja, hogy mindig lesz jelen hello adatbázis előjegyzett változásait.</span><span class="sxs-lookup"><span data-stu-id="a13f4-170">Durability ensures that any change that’s committed in hello database will always be present.</span></span>   

<span data-ttu-id="a13f4-171">A Cosmos DB JavaScript üzemeltetett hello memóriaterületén hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="a13f4-171">In Cosmos DB, JavaScript is hosted in hello same memory space as hello database.</span></span> <span data-ttu-id="a13f4-172">Ezért, tárolt eljárások és eseményindítók kérelmek hajtható végre hello azonos hatókör egy adatbázis-munkamenet.</span><span class="sxs-lookup"><span data-stu-id="a13f4-172">Hence, requests made within stored procedures and triggers execute in hello same scope of a database session.</span></span> <span data-ttu-id="a13f4-173">Ez lehetővé teszi a Cosmos DB tooguarantee sav egyetlen tárolt eljárás vagy eseményindító részét képező minden műveletnél.</span><span class="sxs-lookup"><span data-stu-id="a13f4-173">This enables Cosmos DB tooguarantee ACID for all operations that are part of a single stored procedure/trigger.</span></span> <span data-ttu-id="a13f4-174">Vegye figyelembe a következőket hello tárolt eljárás definíciója:</span><span class="sxs-lookup"><span data-stu-id="a13f4-174">Consider hello following stored procedure definition:</span></span>

    // JavaScript source code
    var exchangeItemsSproc = {
        id: "exchangeItems",
        serverScript: function (playerId1, playerId2) {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();

            var player1Document, player2Document;

            // query for players
            var filterQuery = 'SELECT * FROM Players p where p.id  = "' + playerId1 + '"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery, {},
                function (err, documents, responseOptions) {
                    if (err) throw new Error("Error" + err.message);

                    if (documents.length != 1) throw "Unable toofind both names";
                    player1Document = documents[0];

                    var filterQuery2 = 'SELECT * FROM Players p where p.id = "' + playerId2 + '"';
                    var accept2 = collection.queryDocuments(collection.getSelfLink(), filterQuery2, {},
                        function (err2, documents2, responseOptions2) {
                            if (err2) throw new Error("Error" + err2.message);
                            if (documents2.length != 1) throw "Unable toofind both names";
                            player2Document = documents2[0];
                            swapItems(player1Document, player2Document);
                            return;
                        });
                    if (!accept2) throw "Unable tooread player details, abort ";
                });

            if (!accept) throw "Unable tooread player details, abort ";

            // swap hello two players’ items
            function swapItems(player1, player2) {
                var player1ItemSave = player1.item;
                player1.item = player2.item;
                player2.item = player1ItemSave;

                var accept = collection.replaceDocument(player1._self, player1,
                    function (err, docReplaced) {
                        if (err) throw "Unable tooupdate player 1, abort ";

                        var accept2 = collection.replaceDocument(player2._self, player2,
                            function (err2, docReplaced2) {
                                if (err) throw "Unable tooupdate player 2, abort"
                            });

                        if (!accept2) throw "Unable tooupdate player 2, abort";
                    });

                if (!accept) throw "Unable tooupdate player 1, abort";
            }
        }
    }

    // register hello stored procedure in Node.js client
    client.createStoredProcedureAsync(collection._self, exchangeItemsSproc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
        }
    );

<span data-ttu-id="a13f4-175">Ez a tárolt eljárás tranzakciók egy játék app tootrade elemek között két játékosok egyetlen művelettel belül használja.</span><span class="sxs-lookup"><span data-stu-id="a13f4-175">This stored procedure uses transactions within a gaming app tootrade items between two players in a single operation.</span></span> <span data-ttu-id="a13f4-176">hello tárolt eljárás kísérletek tooread két dokumentumok argumentumként átadott minden megfelelő toohello player azonosítók.</span><span class="sxs-lookup"><span data-stu-id="a13f4-176">hello stored procedure attempts tooread two documents each corresponding toohello player IDs passed in as an argument.</span></span> <span data-ttu-id="a13f4-177">Ha mindkét player dokumentum található, majd hello tárolt eljárás frissítéséhez hello dokumentumok csere az elemeket.</span><span class="sxs-lookup"><span data-stu-id="a13f4-177">If both player documents are found, then hello stored procedure updates hello documents by swapping their items.</span></span> <span data-ttu-id="a13f4-178">Ha bármilyen hiba történik mentén hello módja, azt, amely implicit módon a hello tranzakció megszakítása JavaScript kivételt okoz.</span><span class="sxs-lookup"><span data-stu-id="a13f4-178">If any errors are encountered along hello way, it throws a JavaScript exception that implicitly aborts hello transaction.</span></span>

<span data-ttu-id="a13f4-179">Ha hello gyűjtemény hello tárolt eljárás regisztrálva van-e elleni egypartíciós gyűjtemény, akkor hello tranzakció hatókörön belüli tooall hello dokumentumok hello gyűjteményen belül.</span><span class="sxs-lookup"><span data-stu-id="a13f4-179">If hello collection hello stored procedure is registered against is a single-partition collection, then hello transaction is scoped tooall hello documents within hello collection.</span></span> <span data-ttu-id="a13f4-180">Ha hello gyűjtemény particionálva van, majd a tárolt eljárások hello tranzakció hatókörében egypartíciós kulcs lesznek végrehajtva.</span><span class="sxs-lookup"><span data-stu-id="a13f4-180">If hello collection is partitioned, then stored procedures are executed in hello transaction scope of a single partition key.</span></span> <span data-ttu-id="a13f4-181">Minden egyes tárolt eljárás végrehajtása majd tartalmaznia kell egy megfelelő toohello hatókör hello transaction kell futnia a partíciós kulcs értéke.</span><span class="sxs-lookup"><span data-stu-id="a13f4-181">Each stored procedure execution must then include a partition key value corresponding toohello scope hello transaction must run under.</span></span> <span data-ttu-id="a13f4-182">További részletekért lásd: [Azure Cosmos DB particionálás](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="a13f4-182">For more details, see [Azure Cosmos DB Partitioning](partition-data.md).</span></span>

### <a name="commit-and-rollback"></a><span data-ttu-id="a13f4-183">Érvényesítés és visszaállítás</span><span class="sxs-lookup"><span data-stu-id="a13f4-183">Commit and rollback</span></span>
<span data-ttu-id="a13f4-184">Tranzakciók mélyen és natív módon integrálva vannak Cosmos DB JavaScript programozási modellt.</span><span class="sxs-lookup"><span data-stu-id="a13f4-184">Transactions are deeply and natively integrated into Cosmos DB’s JavaScript programming model.</span></span> <span data-ttu-id="a13f4-185">A JavaScript függvényen belül minden műveletet vannak automatikusan végett az egyetlen tranzakció alatt.</span><span class="sxs-lookup"><span data-stu-id="a13f4-185">Inside a JavaScript function, all operations are automatically wrapped under a single transaction.</span></span> <span data-ttu-id="a13f4-186">Ha hello JavaScript befejezése nélkül kivételen hello műveletek toohello adatbázis lépnek.</span><span class="sxs-lookup"><span data-stu-id="a13f4-186">If hello JavaScript completes without any exception, hello operations toohello database are committed.</span></span> <span data-ttu-id="a13f4-187">Gyakorlatilag hello "BEGIN TRANSACTION" és "COMMIT TRANSACTION" utasítás a relációs adatbázisok implicit Cosmos DB-ben.</span><span class="sxs-lookup"><span data-stu-id="a13f4-187">In effect, hello “BEGIN TRANSACTION” and “COMMIT TRANSACTION” statements in relational databases are implicit in Cosmos DB.</span></span>  

<span data-ttu-id="a13f4-188">Ha bármely hello parancsfájlból propagálja kivételek, Cosmos DB a JavaScript futásidejű visszaállítja hello teljes tranzakciót.</span><span class="sxs-lookup"><span data-stu-id="a13f4-188">If there is any exception that’s propagated from hello script, Cosmos DB’s JavaScript runtime will roll back hello whole transaction.</span></span> <span data-ttu-id="a13f4-189">Hello korábban ismertetett módon, egy kivétel kiváltása példája hatékonyan egyenértékű tooa Cosmos DB "ROLLBACK TRANSACTION".</span><span class="sxs-lookup"><span data-stu-id="a13f4-189">As shown in hello earlier example, throwing an exception is effectively equivalent tooa “ROLLBACK TRANSACTION” in Cosmos DB.</span></span>

### <a name="data-consistency"></a><span data-ttu-id="a13f4-190">Adatkonzisztencia</span><span class="sxs-lookup"><span data-stu-id="a13f4-190">Data consistency</span></span>
<span data-ttu-id="a13f4-191">Tárolt eljárások és eseményindítók mindig végre hello Azure Cosmos DB tároló hello elsődleges replikán.</span><span class="sxs-lookup"><span data-stu-id="a13f4-191">Stored procedures and triggers are always executed on hello primary replica of hello Azure Cosmos DB container.</span></span> <span data-ttu-id="a13f4-192">Ez biztosítja, hogy az olvasások belül tárolt eljárások ajánlat az erős konzisztencia.</span><span class="sxs-lookup"><span data-stu-id="a13f4-192">This ensures that reads from inside stored procedures offer strong consistency.</span></span> <span data-ttu-id="a13f4-193">Elsődleges hello vagy bármely másodlagos replikája lekérdezések felhasználó által definiált függvények használatával hajtható végre, de toomeet biztosítható hello konzisztenciaszint kért hello megfelelő replika kiválasztásával.</span><span class="sxs-lookup"><span data-stu-id="a13f4-193">Queries using user defined functions can be executed on hello primary or any secondary replica, but we ensure toomeet hello requested consistency level by choosing hello appropriate replica.</span></span>

## <a name="bounded-execution"></a><span data-ttu-id="a13f4-194">A kötött végrehajtása</span><span class="sxs-lookup"><span data-stu-id="a13f4-194">Bounded execution</span></span>
<span data-ttu-id="a13f4-195">Minden Cosmos DB műveletet kell végeznie a megadott hello kiszolgálón belüli kérelmek időtúllépési időtartama.</span><span class="sxs-lookup"><span data-stu-id="a13f4-195">All Cosmos DB operations must complete within hello server specified request timeout duration.</span></span> <span data-ttu-id="a13f4-196">Ennél a határértéknél tooJavaScript funkciók (tárolt eljárások, eseményindítók és felhasználó által definiált függvények) is vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="a13f4-196">This constraint also applies tooJavaScript functions (stored procedures, triggers and user-defined functions).</span></span> <span data-ttu-id="a13f4-197">Ha egy művelet nem fejeződött be az ezt az időkorlátot, hello tranzakció vissza lesz állítva.</span><span class="sxs-lookup"><span data-stu-id="a13f4-197">If an operation does not complete with that time limit, hello transaction is rolled back.</span></span> <span data-ttu-id="a13f4-198">JavaScript-funkcióként kell hello időn belül befejezni vagy egy alapú folytatási modell toobatch/Folytatás végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="a13f4-198">JavaScript functions must finish within hello time limit or implement a continuation based model toobatch/resume execution.</span></span>  

<span data-ttu-id="a13f4-199">Rendelés toosimplify fejlesztése tárolt eljárások és eseményindítók toohandle sokáig tartott, minden funkciók alatt hello gyűjtési objektumot (létrehozása, olvasása, cserélje le és törölje a dokumentumok és mellékletek) jelző logikai érték adja vissza. e, amely fogja végrehajtani a műveletet.</span><span class="sxs-lookup"><span data-stu-id="a13f4-199">In order toosimplify development of stored procedures and triggers toohandle time limits, all functions under hello collection object (for create, read, replace, and delete of documents and attachments) return a Boolean value that represents whether that operation will complete.</span></span> <span data-ttu-id="a13f4-200">Ha ez az érték hamis, akkor arra utal, hogy hello határidőn tooexpire kapcsolatos és hello eljárás mentése végrehajtási burkolnia kell.</span><span class="sxs-lookup"><span data-stu-id="a13f4-200">If this value is false, it is an indication that hello time limit is about tooexpire and that hello procedure must wrap up execution.</span></span>  <span data-ttu-id="a13f4-201">Műveletek aszinkron előzetes toohello első elfogadhatatlan adattárolási művelet garantáltan toocomplete, ha hello tárolt eljárás idő alatt fejeződik be, és nem várólistára további kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="a13f4-201">Operations queued prior toohello first unaccepted store operation are guaranteed toocomplete if hello stored procedure completes in time and does not queue any more requests.</span></span>  

<span data-ttu-id="a13f4-202">JavaScript-funkcióként is kötve van a hálózatierőforrás-fogyasztás.</span><span class="sxs-lookup"><span data-stu-id="a13f4-202">JavaScript functions are also bounded on resource consumption.</span></span> <span data-ttu-id="a13f4-203">Cosmos DB fenntart egy adatbázis-fiók üzembe hello méretének gyűjteményenként átviteli sebesség.</span><span class="sxs-lookup"><span data-stu-id="a13f4-203">Cosmos DB reserves throughput per collection based on hello provisioned size of a database account.</span></span> <span data-ttu-id="a13f4-204">Átviteli sebesség a CPU, a memória és a kérelemegység vagy RUs IO fogyasztás normalizált egységben van kifejezve.</span><span class="sxs-lookup"><span data-stu-id="a13f4-204">Throughput is expressed in terms of a normalized unit of CPU, memory and IO consumption called request units or RUs.</span></span> <span data-ttu-id="a13f4-205">JavaScript-funkcióként is használhat egy nagy számú RUs rövid időn belül, és előfordulhat, hogy olvasson sebessége korlátozott hello gyűjtemény korlát elérésekor.</span><span class="sxs-lookup"><span data-stu-id="a13f4-205">JavaScript functions can potentially use up a large number of RUs within a short time, and might get rate-limited if hello collection’s limit is reached.</span></span> <span data-ttu-id="a13f4-206">Erőforrás intenzív tárolt eljárások is egyszerű adatbázis-műveletek karanténba zárt tooensure rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="a13f4-206">Resource intensive stored procedures might also be quarantined tooensure availability of primitive database operations.</span></span>  

### <a name="example-bulk-importing-data-into-a-database-program"></a><span data-ttu-id="a13f4-207">Példa: Tömeges adatok importálása egy adatbázis-program</span><span class="sxs-lookup"><span data-stu-id="a13f4-207">Example: Bulk importing data into a database program</span></span>
<span data-ttu-id="a13f4-208">Alább példája toobulk-importálási dokumentumok gyűjteménybe írt tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="a13f4-208">Below is an example of a stored procedure that is written toobulk-import documents into a collection.</span></span> <span data-ttu-id="a13f4-209">Megjegyzés: hogyan hello tárolt eljárás kezeli, amelyet végrehajtása hello logikai ellenőrzésével visszatérési érték a Documentclient, és majd használ hello között kötegek egyes elindításaihoz hello tárolt eljárás tootrack és folytatása folyamatban van a dokumentumok száma.</span><span class="sxs-lookup"><span data-stu-id="a13f4-209">Note how hello stored procedure handles bounded execution by checking hello Boolean return value from createDocument, and then uses hello count of documents inserted in each invocation of hello stored procedure tootrack and resume progress across batches.</span></span>

    function bulkImport(docs) {
        var collection = getContext().getCollection();
        var collectionLink = collection.getSelfLink();

        // hello count of imported docs, also used as current doc index.
        var count = 0;

        // Validate input.
        if (!docs) throw new Error("hello array is undefined or null.");

        var docsLength = docs.length;
        if (docsLength == 0) {
            getContext().getResponse().setBody(0);
        }

        // Call hello create API toocreate a document.
        tryCreate(docs[count], callback);

        // Note that there are 2 exit conditions:
        // 1) hello createDocument request was not accepted. 
        //    In this case hello callback will not be called, we just call setBody and we are done.
        // 2) hello callback was called docs.length times.
        //    In this case all documents were created and we don’t need toocall tryCreate anymore. Just call setBody and we are done.
        function tryCreate(doc, callback) {
            var isAccepted = collection.createDocument(collectionLink, doc, callback);

            // If hello request was accepted, callback will be called.
            // Otherwise report current count back toohello client, 
            // which will call hello script again with remaining set of docs.
            if (!isAccepted) getContext().getResponse().setBody(count);
        }

        // This is called when collection.createDocument is done in order tooprocess hello result.
        function callback(err, doc, options) {
            if (err) throw err;

            // One more document has been inserted, increment hello count.
            count++;

            if (count >= docsLength) {
                // If we created all documents, we are done. Just set hello response.
                getContext().getResponse().setBody(count);
            } else {
                // Create next document.
                tryCreate(docs[count], callback);
            }
        }
    }

## <span data-ttu-id="a13f4-210"><a id="trigger"></a>Adatbázis-eseményindítók</span><span class="sxs-lookup"><span data-stu-id="a13f4-210"><a id="trigger"></a> Database triggers</span></span>
### <a name="database-pre-triggers"></a><span data-ttu-id="a13f4-211">Adatbázis előtti eseményindítók</span><span class="sxs-lookup"><span data-stu-id="a13f4-211">Database pre-triggers</span></span>
<span data-ttu-id="a13f4-212">Cosmos DB biztosít, amelyek végrehajtása, vagy egy műveletet a dokumentum által indított eseményindítók.</span><span class="sxs-lookup"><span data-stu-id="a13f4-212">Cosmos DB provides triggers that are executed or triggered by an operation on a document.</span></span> <span data-ttu-id="a13f4-213">Például lehetőségeiről előtti eseményindítót hoz létre egy dokumentumot – az előzetes eseményindító fog futni, hello dokumentum létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="a13f4-213">For example, you can specify a pre-trigger when you are creating a document – this pre-trigger will run before hello document is created.</span></span> <span data-ttu-id="a13f4-214">hello az alábbiakban látható egy példa előtti eseményindítók hogyan lehet a létrehozandó dokumentum használt toovalidate hello tulajdonságainak:</span><span class="sxs-lookup"><span data-stu-id="a13f4-214">hello following is an example of how pre-triggers can be used toovalidate hello properties of a document that is being created:</span></span>

    var validateDocumentContentsTrigger = {
        id: "validateDocumentContents",
        serverScript: function validate() {
            var context = getContext();
            var request = context.getRequest();

            // document toobe created in hello current operation
            var documentToCreate = request.getBody();

            // validate properties
            if (!("timestamp" in documentToCreate)) {
                var ts = new Date();
                documentToCreate["my timestamp"] = ts.getTime();
            }

            // update hello document that will be created
            request.setBody(documentToCreate);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.Create
    }


<span data-ttu-id="a13f4-215">És Node.js ügyféloldali regisztrációja kód hello eseményindító megfelelő hello:</span><span class="sxs-lookup"><span data-stu-id="a13f4-215">And hello corresponding Node.js client-side registration code for hello trigger:</span></span>

    // register pre-trigger
    client.createTriggerAsync(collection.self, validateDocumentContentsTrigger)
        .then(function (response) {
            console.log("Created", response.resource);
            var docToCreate = {
                id: "DocWithTrigger",
                event: "Error",
                source: "Network outage"
            };

            // run trigger while creating above document 
            var options = { preTriggerInclude: "validateDocumentContents" };

            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response.resource); // document with timestamp property added
    }, function (error) {
        console.log("Error", error);
    });


<span data-ttu-id="a13f4-216">Előtti eseményindítók nem tartozhat bemeneti paraméter.</span><span class="sxs-lookup"><span data-stu-id="a13f4-216">Pre-triggers cannot have any input parameters.</span></span> <span data-ttu-id="a13f4-217">hello kérelem objektum használt toomanipulate hello társított kérelemüzenethez tartozó hello művelet lehet.</span><span class="sxs-lookup"><span data-stu-id="a13f4-217">hello request object can be used toomanipulate hello request message associated with hello operation.</span></span> <span data-ttu-id="a13f4-218">Itt hello előtti eseményindító hello létrehozása egy dokumentum futtatják, és hello kérelem üzenettörzs tartalmazza hello dokumentum toobe JSON formátumú.</span><span class="sxs-lookup"><span data-stu-id="a13f4-218">Here, hello pre-trigger is being run with hello creation of a document, and hello request message body contains hello document toobe created in JSON format.</span></span>   

<span data-ttu-id="a13f4-219">Eseményindítók regisztrált, amikor a felhasználók megadhatják való futtatás hello műveletek.</span><span class="sxs-lookup"><span data-stu-id="a13f4-219">When triggers are registered, users can specify hello operations that it can run with.</span></span> <span data-ttu-id="a13f4-220">Ehhez az eseményindítóhoz TriggerOperation.Create, ami azt jelenti, nem engedélyezett a következő hello hozták létre.</span><span class="sxs-lookup"><span data-stu-id="a13f4-220">This trigger was created with TriggerOperation.Create, which means hello following is not permitted.</span></span>

    var options = { preTriggerInclude: "validateDocumentContents" };

    client.replaceDocumentAsync(docToReplace.self,
                  newDocBody, options)
    .then(function (response) {
        console.log(response.resource);
    }, function (error) {
        console.log("Error", error);
    });

    // Fails, can’t use a create trigger in a replace operation

### <a name="database-post-triggers"></a><span data-ttu-id="a13f4-221">Adatbázis utáni eseményindítók</span><span class="sxs-lookup"><span data-stu-id="a13f4-221">Database post-triggers</span></span>
<span data-ttu-id="a13f4-222">Utáni eseményindítók előtti eseményindítók, például egy műveletet a dokumentum tartoznak, és nem helyez el a bemeneti paramétereket.</span><span class="sxs-lookup"><span data-stu-id="a13f4-222">Post-triggers, like pre-triggers, are associated with an operation on a document and don’t take any input parameters.</span></span> <span data-ttu-id="a13f4-223">Futnak **után** hello művelet befejeződött, és hozzáférést toohello válaszüzenetet küldött toohello ügyfél rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="a13f4-223">They run **after** hello operation has completed, and have access toohello response message that is sent toohello client.</span></span>   

<span data-ttu-id="a13f4-224">a következő példa hello utáni eseményindítók működés közben látható:</span><span class="sxs-lookup"><span data-stu-id="a13f4-224">hello following example shows post-triggers in action:</span></span>

    var updateMetadataTrigger = {
        id: "updateMetadata",
        serverScript: function updateMetadata() {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();

            // document that was created
            var createdDocument = response.getBody();

            // query for metadata document
            var filterQuery = 'SELECT * FROM root r WHERE r.id = "_metadata"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery,
                updateMetadataCallback);
            if(!accept) throw "Unable tooupdate metadata, abort";

            function updateMetadataCallback(err, documents, responseOptions) {
                if(err) throw new Error("Error" + err.message);
                         if(documents.length != 1) throw 'Unable toofind metadata document';

                         var metadataDocument = documents[0];

                         // update metadata
                         metadataDocument.createdDocuments += 1;
                         metadataDocument.createdNames += " " + createdDocument.id;
                         var accept = collection.replaceDocument(metadataDocument._self,
                               metadataDocument, function(err, docReplaced) {
                                      if(err) throw "Unable tooupdate metadata, abort";
                               });
                         if(!accept) throw "Unable tooupdate metadata, abort";
                         return;                    
            }                                                                                            
        },
        triggerType: TriggerType.Post,
        triggerOperation: TriggerOperation.All
    }


<span data-ttu-id="a13f4-225">hello eseményindító regisztrálható, ahogy az a következő minta hello.</span><span class="sxs-lookup"><span data-stu-id="a13f4-225">hello trigger can be registered as shown in hello following sample.</span></span>

    // register post-trigger
    client.createTriggerAsync('dbs/testdb/colls/testColl', updateMetadataTrigger)
        .then(function(createdTrigger) { 
            var docToCreate = { 
                name: "artist_profile_1023",
                artist: "hello Band",
                albums: ["Hellujah", "Rotators", "Spinning Top"]
            };

            // run trigger while creating above document 
            var options = { postTriggerInclude: "updateMetadata" };

            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });


<span data-ttu-id="a13f4-226">Ehhez az eseményindítóhoz lekérdezi a hello metaadat-dokumentum, és frissíti azt az újonnan létrehozott hello dokumentum adatait.</span><span class="sxs-lookup"><span data-stu-id="a13f4-226">This trigger queries for hello metadata document and updates it with details about hello newly created document.</span></span>  

<span data-ttu-id="a13f4-227">Egyik dolog, ami fontos toonote hello **tranzakciós** Cosmos DB eseményindítók végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="a13f4-227">One thing that is important toonote is hello **transactional** execution of triggers in Cosmos DB.</span></span> <span data-ttu-id="a13f4-228">Ez utáni eseményindító hello részeként fut ugyanabban a tranzakcióban, hello hello eredeti dokumentumhoz létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a13f4-228">This post-trigger runs as part of hello same transaction as hello creation of hello original document.</span></span> <span data-ttu-id="a13f4-229">Ezért hello utáni eseményindítóval (például ha folyamatban, nem tooupdate hello metaadat-dokumentum) azt kivételt jelez, ha hello teljes tranzakció sikertelen lesz, és vissza lesz vonva.</span><span class="sxs-lookup"><span data-stu-id="a13f4-229">Therefore, if we throw an exception from hello post-trigger (say if we are unable tooupdate hello metadata document), hello whole transaction will fail and be rolled back.</span></span> <span data-ttu-id="a13f4-230">Nincs dokumentum jön létre, és kivételt adja vissza.</span><span class="sxs-lookup"><span data-stu-id="a13f4-230">No document will be created, and an exception will be returned.</span></span>  

## <span data-ttu-id="a13f4-231"><a id="udf"></a>Felhasználó által definiált függvények</span><span class="sxs-lookup"><span data-stu-id="a13f4-231"><a id="udf"></a>User-defined functions</span></span>
<span data-ttu-id="a13f4-232">Felhasználói függvény (UDF) használt tooextend hello DocumentDB API SQL-lekérdezési nyelv szintaxis és üzleti logika megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="a13f4-232">User-defined functions (UDFs) are used tooextend hello DocumentDB API SQL query language grammar and implement custom business logic.</span></span> <span data-ttu-id="a13f4-233">Ezek csak a hívható lekérdezéseken belül.</span><span class="sxs-lookup"><span data-stu-id="a13f4-233">They can only be called from inside queries.</span></span> <span data-ttu-id="a13f4-234">Ugyanakkor nem rendelkeznek hozzáféréssel toohello környezeti objektumot, és csak számítási JavaScript használt toobe úgy van kialakítva.</span><span class="sxs-lookup"><span data-stu-id="a13f4-234">They do not have access toohello context object and are meant toobe used as compute-only JavaScript.</span></span> <span data-ttu-id="a13f4-235">Ezért a felhasználó által megadott függvények másodlagos replikáin hello Cosmos DB szolgáltatás futtatható.</span><span class="sxs-lookup"><span data-stu-id="a13f4-235">Therefore, UDFs can be run on secondary replicas of hello Cosmos DB service.</span></span>  

<span data-ttu-id="a13f4-236">hello alábbi minta létrehoz egy UDF toocalculate adó különböző bevétel zárójeleket sebességet alapján, és azt használja a lekérdezés toofind belül minden olyan személyek, akik több mint 20 000 $ fizetett adók.</span><span class="sxs-lookup"><span data-stu-id="a13f4-236">hello following sample creates a UDF toocalculate income tax based on rates for various income brackets, and then uses it inside a query toofind all people who paid more than $20,000 in taxes.</span></span>

    var taxUdf = {
        id: "tax",
        serverScript: function tax(income) {

            if(income == undefined) 
                throw 'no input';

            if (income < 1000) 
                return income * 0.1;
            else if (income < 10000) 
                return income * 0.2;
            else
                return income * 0.4;
        }
    }


<span data-ttu-id="a13f4-237">hello UDF ezt követően használható a következő minta hello például a lekérdezések:</span><span class="sxs-lookup"><span data-stu-id="a13f4-237">hello UDF can subsequently be used in queries like in hello following sample:</span></span>

    // register UDF
    client.createUserDefinedFunctionAsync('dbs/testdb/colls/testColl', taxUdf)
        .then(function(response) { 
            console.log("Created", response.resource);

            var query = 'SELECT * FROM TaxPayers t WHERE udf.tax(t.income) > 20000'; 
            return client.queryDocuments('dbs/testdb/colls/testColl',
                   query).toArrayAsync();
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        var documents = response.feed;
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });

## <a name="javascript-language-integrated-query-api"></a><span data-ttu-id="a13f4-238">A JavaScript nyelv integrált lekérdezés API</span><span class="sxs-lookup"><span data-stu-id="a13f4-238">JavaScript language-integrated query API</span></span>
<span data-ttu-id="a13f4-239">Ezenkívül tooissuing lekérdezések DocumentDB SQL-szintaxis használatával, hello kiszolgálóoldali SDK lehetővé teszi, az SQL ismeretek nélkül Folyékonyan beszél JavaScript-illesztő segítségével tooperform optimalizált lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="a13f4-239">In addition tooissuing queries using DocumentDB’s SQL grammar, hello server-side SDK allows you tooperform optimized queries using a fluent JavaScript interface without any knowledge of SQL.</span></span> <span data-ttu-id="a13f4-240">API-jával tooprogrammatically build lekérdezések úgy, hogy a predikátum függvény chainable függvénynek hello JavaScript lekérdezés hívja, egy szintaxisa tisztában tooECMAScript5 tömb built-ins és lodash például népszerű JavaScript szalagtárak.</span><span class="sxs-lookup"><span data-stu-id="a13f4-240">hello JavaScript query API allows you tooprogrammatically build queries by passing predicate functions into chainable function calls, with a syntax familiar tooECMAScript5's Array built-ins and popular JavaScript libraries like lodash.</span></span> <span data-ttu-id="a13f4-241">Lekérdezések által hello JavaScript futásidejű toobe végre hatékonyan Azure Cosmos DB indexek elemzésének.</span><span class="sxs-lookup"><span data-stu-id="a13f4-241">Queries are parsed by hello JavaScript runtime toobe executed efficiently using Azure Cosmos DB’s indices.</span></span>

> [!NOTE]
> <span data-ttu-id="a13f4-242">`__`(kettős-aláhúzásjel) túl alias`getContext().getCollection()`.</span><span class="sxs-lookup"><span data-stu-id="a13f4-242">`__` (double-underscore) is an alias too`getContext().getCollection()`.</span></span>
> <br/>
> <span data-ttu-id="a13f4-243">Más szóval használhatja `__` vagy `getContext().getCollection()` tooaccess hello JavaScript lekérdezés API.</span><span class="sxs-lookup"><span data-stu-id="a13f4-243">In other words, you can use `__` or `getContext().getCollection()` tooaccess hello JavaScript query API.</span></span>
> 
> 

<span data-ttu-id="a13f4-244">Támogatott funkciók a következők:</span><span class="sxs-lookup"><span data-stu-id="a13f4-244">Supported functions include:</span></span>

<ul>
<li><span data-ttu-id="a13f4-245">
<b>CHAIN().... érték ([visszahívási] [, beállítások])</b>
</span><span class="sxs-lookup"><span data-stu-id="a13f4-245">
<b>chain() ... .value([callback] [, options])</b>
</span></span><ul>
<li>
<span data-ttu-id="a13f4-246">Value() – amely kell lezárni láncolt hívás kezdődik.</span><span class="sxs-lookup"><span data-stu-id="a13f4-246">Starts a chained call which must be terminated with value().</span></span>
</li>
</ul>
</li>
<li><span data-ttu-id="a13f4-247">
<b>szűrő (predicateFunction [, beállítások] [, visszahívási])</b>
</span><span class="sxs-lookup"><span data-stu-id="a13f4-247">
<b>filter(predicateFunction [, options] [, callback])</b>
</span></span><ul>
<li>
<span data-ttu-id="a13f4-248">Adjon meg egy ad vissza igaz/hamis értékű, a rendelés toofilter kimeneti/bemeneti dokumentumok hello eredő a predikátum függvény használatával hello szűrők.</span><span class="sxs-lookup"><span data-stu-id="a13f4-248">Filters hello input using a predicate function which returns true/false in order toofilter in/out input documents into hello resulting set.</span></span> <span data-ttu-id="a13f4-249">Ez úgy viselkedik, hasonló tooa SQL WHERE záradékban.</span><span class="sxs-lookup"><span data-stu-id="a13f4-249">This behaves similar tooa WHERE clause in SQL.</span></span>
</li>
</ul>
</li>
<li><span data-ttu-id="a13f4-250">
<b>térkép (transformationFunction [, beállítások] [, visszahívási])</b>
</span><span class="sxs-lookup"><span data-stu-id="a13f4-250">
<b>map(transformationFunction [, options] [, callback])</b>
</span></span><ul>
<li>
<span data-ttu-id="a13f4-251">Egy transzformációs függvény, amely minden egyes bemeneti elem tooa JavaScript-objektum vagy megadott leképezés vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="a13f4-251">Applies a projection given a transformation function which maps each input item tooa JavaScript object or value.</span></span> <span data-ttu-id="a13f4-252">Ez úgy viselkedik, hasonló tooa SELECT záradékban az SQL.</span><span class="sxs-lookup"><span data-stu-id="a13f4-252">This behaves similar tooa SELECT clause in SQL.</span></span>
</li>
</ul>
</li>
<li><span data-ttu-id="a13f4-253">
<b>pluck ([propertyName] [, beállítások] [, visszahívási])</b>
</span><span class="sxs-lookup"><span data-stu-id="a13f4-253">
<b>pluck([propertyName] [, options] [, callback])</b>
</span></span><ul>
<li>
<span data-ttu-id="a13f4-254">Ez egy olyan térképet, amely egyetlen tulajdonság értékének hello összes beviteli elemet a parancsikont.</span><span class="sxs-lookup"><span data-stu-id="a13f4-254">This is a shortcut for a map which extracts hello value of a single property from each input item.</span></span>
</li>
</ul>
</li>
<li><span data-ttu-id="a13f4-255">
<b>egybesimítására ([isShallow] [, beállítások] [, visszahívási])</b>
</span><span class="sxs-lookup"><span data-stu-id="a13f4-255">
<b>flatten([isShallow] [, options] [, callback])</b>
</span></span><ul>
<li>
<span data-ttu-id="a13f4-256">Egyesíti, és minden tooa egyetlen tömb bemeneti eleménél tömbök simítja.</span><span class="sxs-lookup"><span data-stu-id="a13f4-256">Combines and flattens arrays from each input item in tooa single array.</span></span> <span data-ttu-id="a13f4-257">A LINQ hasonló tooSelectMany ez viselkedik.</span><span class="sxs-lookup"><span data-stu-id="a13f4-257">This behaves similar tooSelectMany in LINQ.</span></span>
</li>
</ul>
</li>
<li><span data-ttu-id="a13f4-258">
<b>a sortBy ([predicate] [, beállítások] [, visszahívási])</b>
</span><span class="sxs-lookup"><span data-stu-id="a13f4-258">
<b>sortBy([predicate] [, options] [, callback])</b>
</span></span><ul>
<li>
<span data-ttu-id="a13f4-259">Létrehoznak egy új dokumentumot hello dokumentumok, a bemeneti dokumentum-adatfolyamra hello növekvő sorrendben predikátumban megadott hello segítségével rendezésével.</span><span class="sxs-lookup"><span data-stu-id="a13f4-259">Produce a new set of documents by sorting hello documents in hello input document stream in ascending order using hello given predicate.</span></span> <span data-ttu-id="a13f4-260">Ez úgy viselkedik, hasonló tooa ORDER BY záradékban az SQL.</span><span class="sxs-lookup"><span data-stu-id="a13f4-260">This behaves similar tooa ORDER BY clause in SQL.</span></span>
</li>
</ul>
</li>
<li><span data-ttu-id="a13f4-261">
<b>sortByDescending ([predicate] [, beállítások] [, visszahívási])</b>
</span><span class="sxs-lookup"><span data-stu-id="a13f4-261">
<b>sortByDescending([predicate] [, options] [, callback])</b>
</span></span><ul>
<li>
<span data-ttu-id="a13f4-262">Létrehoznak egy új dokumentumot hello dokumentumok, a bemeneti dokumentum-adatfolyamra hello csökkenő sorrendben predikátumban megadott hello segítségével rendezésével.</span><span class="sxs-lookup"><span data-stu-id="a13f4-262">Produce a new set of documents by sorting hello documents in hello input document stream in descending order using hello given predicate.</span></span> <span data-ttu-id="a13f4-263">Ez úgy viselkedik, hasonló tooa x DESC ORDER BY záradékban az SQL.</span><span class="sxs-lookup"><span data-stu-id="a13f4-263">This behaves similar tooa ORDER BY x DESC clause in SQL.</span></span>
</li>
</ul>
</li>
</ul>


<span data-ttu-id="a13f4-264">Amikor bekerüljön predikátum és/vagy választó funkciók, hello következő JavaScript-szerkezetek kérjen automatikusan optimalizált toorun közvetlenül a Azure Cosmos DB indexek:</span><span class="sxs-lookup"><span data-stu-id="a13f4-264">When included inside predicate and/or selector functions, hello following JavaScript constructs get automatically optimized toorun directly on Azure Cosmos DB indices:</span></span>

* <span data-ttu-id="a13f4-265">Egyszerű operátorok: = + - * / % |} ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;!</span><span class="sxs-lookup"><span data-stu-id="a13f4-265">Simple operators: = + - * / % | ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;!</span></span> ~
* <span data-ttu-id="a13f4-266">Szövegkonstans, többek között a hello objektum szövegkonstans: {}</span><span class="sxs-lookup"><span data-stu-id="a13f4-266">Literals, including hello object literal: {}</span></span>
* <span data-ttu-id="a13f4-267">var, visszatérési</span><span class="sxs-lookup"><span data-stu-id="a13f4-267">var, return</span></span>

<span data-ttu-id="a13f4-268">hello hoz létre a következő JavaScript nem Azure Cosmos DB indexek az beszerzése optimalizált:</span><span class="sxs-lookup"><span data-stu-id="a13f4-268">hello following JavaScript constructs do not get optimized for Azure Cosmos DB indices:</span></span>

* <span data-ttu-id="a13f4-269">Szabályozhatja a folyamat (pl. Ha, közben)</span><span class="sxs-lookup"><span data-stu-id="a13f4-269">Control flow (e.g. if, for, while)</span></span>
* <span data-ttu-id="a13f4-270">Függvényhívások</span><span class="sxs-lookup"><span data-stu-id="a13f4-270">Function calls</span></span>

<span data-ttu-id="a13f4-271">További információkért lásd: a [kiszolgálóoldali JSDocs](http://azure.github.io/azure-documentdb-js-server/).</span><span class="sxs-lookup"><span data-stu-id="a13f4-271">For more information, please see our [Server-Side JSDocs](http://azure.github.io/azure-documentdb-js-server/).</span></span>

### <a name="example-write-a-stored-procedure-using-hello-javascript-query-api"></a><span data-ttu-id="a13f4-272">Példa: Írási hello JavaScript API-jával lekérdezés tárolt eljárás</span><span class="sxs-lookup"><span data-stu-id="a13f4-272">Example: Write a stored procedure using hello JavaScript query API</span></span>
<span data-ttu-id="a13f4-273">a következő példakód hello példája hogyan hello JavaScript lekérdezés API hello környezetben tárolt eljárás használható.</span><span class="sxs-lookup"><span data-stu-id="a13f4-273">hello following code sample is an example of how hello JavaScript Query API can be used in hello context of a stored procedure.</span></span> <span data-ttu-id="a13f4-274">hello tárolt eljárás szúr be egy dokumentum, egy bemeneti paraméter által megadott, és frissíti a metaadat-dokumentum, hello segítségével `__.filter()` metódus minSize, a maxSize és a totalSize hello bemeneti dokumentum size tulajdonság alapján.</span><span class="sxs-lookup"><span data-stu-id="a13f4-274">hello stored procedure inserts a document, given by an input parameter, and updates a metadata document, using hello `__.filter()` method, with minSize, maxSize, and totalSize based upon hello input document's size property.</span></span>

    /**
     * Insert actual doc and update metadata doc: minSize, maxSize, totalSize based on doc.size.
     */
    function insertDocumentAndUpdateMetadata(doc) {
      // HTTP error codes sent tooour callback funciton by DocDB server.
      var ErrorCode = {
        RETRY_WITH: 449,
      }

      var isAccepted = __.createDocument(__.getSelfLink(), doc, {}, function(err, doc, options) {
        if (err) throw err;

        // Check hello doc (ignore docs with invalid/zero size and metaDoc itself) and call updateMetadata.
        if (!doc.isMetadata && doc.size > 0) {
          // Get hello meta document. We keep it in hello same collection. it's hello only doc that has .isMetadata = true.
          var result = __.filter(function(x) {
            return x.isMetadata === true
          }, function(err, feed, options) {
            if (err) throw err;

            // We assume that metadata doc was pre-created and must exist when this script is called.
            if (!feed || !feed.length) throw new Error("Failed toofind hello metadata document.");

            // hello metadata document.
            var metaDoc = feed[0];

            // Update metaDoc.minSize:
            // for 1st document use doc.Size, for all hello rest see if it's less than last min.
            if (metaDoc.minSize == 0) metaDoc.minSize = doc.size;
            else metaDoc.minSize = Math.min(metaDoc.minSize, doc.size);

            // Update metaDoc.maxSize.
            metaDoc.maxSize = Math.max(metaDoc.maxSize, doc.size);

            // Update metaDoc.totalSize.
            metaDoc.totalSize += doc.size;

            // Update/replace hello metadata document in hello store.
            var isAccepted = __.replaceDocument(metaDoc._self, metaDoc, function(err) {
              if (err) throw err;
              // Note: in case concurrent updates causes conflict with ErrorCode.RETRY_WITH, we can't read hello meta again 
              //       and update again because due tooSnapshot isolation we will read same exact version (we are in same transaction).
              //       We have tootake care of that on hello client side.
            });
            if (!isAccepted) throw new Error("replaceDocument(metaDoc) returned false.");
          });
          if (!result.isAccepted) throw new Error("filter for metaDoc returned false.");
        }
      });
      if (!isAccepted) throw new Error("createDocument(actual doc) returned false.");
    }

## <a name="sql-toojavascript-cheat-sheet"></a><span data-ttu-id="a13f4-275">SQL tooJavascript Adatlap</span><span class="sxs-lookup"><span data-stu-id="a13f4-275">SQL tooJavascript cheat sheet</span></span>
<span data-ttu-id="a13f4-276">hello alábbi táblázat mutatja be különböző SQL-lekérdezések és hello megfelelő JavaScript lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="a13f4-276">hello following table presents various SQL queries and hello corresponding JavaScript queries.</span></span>

<span data-ttu-id="a13f4-277">Az SQL-lekérdezések, a dokumentum tulajdonság kulcsok, (pl. `doc.id`)-és nagybetűk.</span><span class="sxs-lookup"><span data-stu-id="a13f4-277">As with SQL queries, document property keys (e.g. `doc.id`) are case-sensitive.</span></span>

|<span data-ttu-id="a13f4-278">SQL</span><span class="sxs-lookup"><span data-stu-id="a13f4-278">SQL</span></span>| <span data-ttu-id="a13f4-279">JavaScript lekérdezés API</span><span class="sxs-lookup"><span data-stu-id="a13f4-279">JavaScript Query API</span></span>|<span data-ttu-id="a13f4-280">Az alábbi leírása</span><span class="sxs-lookup"><span data-stu-id="a13f4-280">Description below</span></span>|
|---|---|---|
|<span data-ttu-id="a13f4-281">VÁLASSZA KI *</span><span class="sxs-lookup"><span data-stu-id="a13f4-281">SELECT *</span></span><br><span data-ttu-id="a13f4-282">A dokumentumok</span><span class="sxs-lookup"><span data-stu-id="a13f4-282">FROM docs</span></span>| <span data-ttu-id="a13f4-283">__.Map(Function(doc) {</span><span class="sxs-lookup"><span data-stu-id="a13f4-283">__.map(function(doc) {</span></span> <br><span data-ttu-id="a13f4-284">&nbsp;&nbsp;&nbsp;&nbsp;térjen vissza a doc;</span><span class="sxs-lookup"><span data-stu-id="a13f4-284">&nbsp;&nbsp;&nbsp;&nbsp;return doc;</span></span><br><span data-ttu-id="a13f4-285">});</span><span class="sxs-lookup"><span data-stu-id="a13f4-285">});</span></span>|<span data-ttu-id="a13f4-286">1</span><span class="sxs-lookup"><span data-stu-id="a13f4-286">1</span></span>|
|<span data-ttu-id="a13f4-287">Jelölje be docs.id, mint docs.message adatköltségek, docs.actions</span><span class="sxs-lookup"><span data-stu-id="a13f4-287">SELECT docs.id, docs.message AS msg, docs.actions</span></span> <br><span data-ttu-id="a13f4-288">A dokumentumok</span><span class="sxs-lookup"><span data-stu-id="a13f4-288">FROM docs</span></span>|<span data-ttu-id="a13f4-289">__.Map(Function(doc) {</span><span class="sxs-lookup"><span data-stu-id="a13f4-289">__.map(function(doc) {</span></span><br><span data-ttu-id="a13f4-290">&nbsp;&nbsp;&nbsp;&nbsp;{visszaadása</span><span class="sxs-lookup"><span data-stu-id="a13f4-290">&nbsp;&nbsp;&nbsp;&nbsp;return {</span></span><br><span data-ttu-id="a13f4-291">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;azonosító: doc.id,</span><span class="sxs-lookup"><span data-stu-id="a13f4-291">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;id: doc.id,</span></span><br><span data-ttu-id="a13f4-292">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;üzenet: doc.message,</span><span class="sxs-lookup"><span data-stu-id="a13f4-292">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;msg: doc.message,</span></span><br><span data-ttu-id="a13f4-293">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;actions:doc.Actions</span><span class="sxs-lookup"><span data-stu-id="a13f4-293">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;actions:doc.actions</span></span><br><span data-ttu-id="a13f4-294">&nbsp;&nbsp;&nbsp;&nbsp;};</span><span class="sxs-lookup"><span data-stu-id="a13f4-294">&nbsp;&nbsp;&nbsp;&nbsp;};</span></span><br><span data-ttu-id="a13f4-295">});</span><span class="sxs-lookup"><span data-stu-id="a13f4-295">});</span></span>|<span data-ttu-id="a13f4-296">2</span><span class="sxs-lookup"><span data-stu-id="a13f4-296">2</span></span>|
|<span data-ttu-id="a13f4-297">VÁLASSZA KI *</span><span class="sxs-lookup"><span data-stu-id="a13f4-297">SELECT *</span></span><br><span data-ttu-id="a13f4-298">A dokumentumok</span><span class="sxs-lookup"><span data-stu-id="a13f4-298">FROM docs</span></span><br><span data-ttu-id="a13f4-299">HOL docs.id="X998_Y998"</span><span class="sxs-lookup"><span data-stu-id="a13f4-299">WHERE docs.id="X998_Y998"</span></span>|<span data-ttu-id="a13f4-300">__.Filter(Function(doc) {</span><span class="sxs-lookup"><span data-stu-id="a13f4-300">__.filter(function(doc) {</span></span><br><span data-ttu-id="a13f4-301">&nbsp;&nbsp;&nbsp;&nbsp;térjen vissza a doc.id === "X998_Y998";</span><span class="sxs-lookup"><span data-stu-id="a13f4-301">&nbsp;&nbsp;&nbsp;&nbsp;return doc.id ==="X998_Y998";</span></span><br><span data-ttu-id="a13f4-302">});</span><span class="sxs-lookup"><span data-stu-id="a13f4-302">});</span></span>|<span data-ttu-id="a13f4-303">3</span><span class="sxs-lookup"><span data-stu-id="a13f4-303">3</span></span>|
|<span data-ttu-id="a13f4-304">VÁLASSZA KI *</span><span class="sxs-lookup"><span data-stu-id="a13f4-304">SELECT *</span></span><br><span data-ttu-id="a13f4-305">A dokumentumok</span><span class="sxs-lookup"><span data-stu-id="a13f4-305">FROM docs</span></span><br><span data-ttu-id="a13f4-306">HOL ARRAY_CONTAINS (dokumentumok. Címkék, 123)</span><span class="sxs-lookup"><span data-stu-id="a13f4-306">WHERE ARRAY_CONTAINS(docs.Tags, 123)</span></span>|<span data-ttu-id="a13f4-307">__.Filter(Function(x) {</span><span class="sxs-lookup"><span data-stu-id="a13f4-307">__.filter(function(x) {</span></span><br><span data-ttu-id="a13f4-308">&nbsp;&nbsp;&nbsp;&nbsp;térjen vissza a x.Tags & & x.Tags.indexOf(123) > -1;</span><span class="sxs-lookup"><span data-stu-id="a13f4-308">&nbsp;&nbsp;&nbsp;&nbsp;return x.Tags && x.Tags.indexOf(123) > -1;</span></span><br><span data-ttu-id="a13f4-309">});</span><span class="sxs-lookup"><span data-stu-id="a13f4-309">});</span></span>|<span data-ttu-id="a13f4-310">4</span><span class="sxs-lookup"><span data-stu-id="a13f4-310">4</span></span>|
|<span data-ttu-id="a13f4-311">Jelölje be docs.id, docs.message adatköltségek,</span><span class="sxs-lookup"><span data-stu-id="a13f4-311">SELECT docs.id, docs.message AS msg</span></span><br><span data-ttu-id="a13f4-312">A dokumentumok</span><span class="sxs-lookup"><span data-stu-id="a13f4-312">FROM docs</span></span><br><span data-ttu-id="a13f4-313">HOL docs.id="X998_Y998"</span><span class="sxs-lookup"><span data-stu-id="a13f4-313">WHERE docs.id="X998_Y998"</span></span>|<span data-ttu-id="a13f4-314">__.chain()</span><span class="sxs-lookup"><span data-stu-id="a13f4-314">__.chain()</span></span><br><span data-ttu-id="a13f4-315">&nbsp;&nbsp;&nbsp;&nbsp;.Filter(Function(doc) {</span><span class="sxs-lookup"><span data-stu-id="a13f4-315">&nbsp;&nbsp;&nbsp;&nbsp;.filter(function(doc) {</span></span><br><span data-ttu-id="a13f4-316">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;térjen vissza a doc.id === "X998_Y998";</span><span class="sxs-lookup"><span data-stu-id="a13f4-316">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return doc.id ==="X998_Y998";</span></span><br><span data-ttu-id="a13f4-317">&nbsp;&nbsp;&nbsp;&nbsp;})</span><span class="sxs-lookup"><span data-stu-id="a13f4-317">&nbsp;&nbsp;&nbsp;&nbsp;})</span></span><br><span data-ttu-id="a13f4-318">&nbsp;&nbsp;&nbsp;&nbsp;.Map(Function(doc) {</span><span class="sxs-lookup"><span data-stu-id="a13f4-318">&nbsp;&nbsp;&nbsp;&nbsp;.map(function(doc) {</span></span><br><span data-ttu-id="a13f4-319">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{visszaadása</span><span class="sxs-lookup"><span data-stu-id="a13f4-319">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return {</span></span><br><span data-ttu-id="a13f4-320">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;azonosító: doc.id,</span><span class="sxs-lookup"><span data-stu-id="a13f4-320">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;id: doc.id,</span></span><br><span data-ttu-id="a13f4-321">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;üzenet: doc.message</span><span class="sxs-lookup"><span data-stu-id="a13f4-321">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;msg: doc.message</span></span><br><span data-ttu-id="a13f4-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};</span><span class="sxs-lookup"><span data-stu-id="a13f4-322">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};</span></span><br><span data-ttu-id="a13f4-323">&nbsp;&nbsp;&nbsp;&nbsp;})</span><span class="sxs-lookup"><span data-stu-id="a13f4-323">&nbsp;&nbsp;&nbsp;&nbsp;})</span></span><br><span data-ttu-id="a13f4-324">.Value();</span><span class="sxs-lookup"><span data-stu-id="a13f4-324">.value();</span></span>|<span data-ttu-id="a13f4-325">5</span><span class="sxs-lookup"><span data-stu-id="a13f4-325">5</span></span>|
|<span data-ttu-id="a13f4-326">A SELECT VALUE címke</span><span class="sxs-lookup"><span data-stu-id="a13f4-326">SELECT VALUE tag</span></span><br><span data-ttu-id="a13f4-327">A dokumentumok</span><span class="sxs-lookup"><span data-stu-id="a13f4-327">FROM docs</span></span><br><span data-ttu-id="a13f4-328">CSATLAKOZTASSA a docs címke. Címkék</span><span class="sxs-lookup"><span data-stu-id="a13f4-328">JOIN tag IN docs.Tags</span></span><br><span data-ttu-id="a13f4-329">ORDER BY docs._ts</span><span class="sxs-lookup"><span data-stu-id="a13f4-329">ORDER BY docs._ts</span></span>|<span data-ttu-id="a13f4-330">__.chain()</span><span class="sxs-lookup"><span data-stu-id="a13f4-330">__.chain()</span></span><br><span data-ttu-id="a13f4-331">&nbsp;&nbsp;&nbsp;&nbsp;.Filter(Function(doc) {</span><span class="sxs-lookup"><span data-stu-id="a13f4-331">&nbsp;&nbsp;&nbsp;&nbsp;.filter(function(doc) {</span></span><br><span data-ttu-id="a13f4-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;térjen vissza a dokumentumot. Címkék & & Array.isArray (doc. Címkék);</span><span class="sxs-lookup"><span data-stu-id="a13f4-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return doc.Tags && Array.isArray(doc.Tags);</span></span><br><span data-ttu-id="a13f4-333">&nbsp;&nbsp;&nbsp;&nbsp;})</span><span class="sxs-lookup"><span data-stu-id="a13f4-333">&nbsp;&nbsp;&nbsp;&nbsp;})</span></span><br><span data-ttu-id="a13f4-334">&nbsp;&nbsp;&nbsp;&nbsp;.sortBy(function(doc) {</span><span class="sxs-lookup"><span data-stu-id="a13f4-334">&nbsp;&nbsp;&nbsp;&nbsp;.sortBy(function(doc) {</span></span><br><span data-ttu-id="a13f4-335">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;térjen vissza a doc._ts;</span><span class="sxs-lookup"><span data-stu-id="a13f4-335">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return doc._ts;</span></span><br><span data-ttu-id="a13f4-336">&nbsp;&nbsp;&nbsp;&nbsp;})</span><span class="sxs-lookup"><span data-stu-id="a13f4-336">&nbsp;&nbsp;&nbsp;&nbsp;})</span></span><br><span data-ttu-id="a13f4-337">&nbsp;&nbsp;&nbsp;&nbsp;.pluck("tags")</span><span class="sxs-lookup"><span data-stu-id="a13f4-337">&nbsp;&nbsp;&nbsp;&nbsp;.pluck("Tags")</span></span><br><span data-ttu-id="a13f4-338">&nbsp;&nbsp;&nbsp;&nbsp;.flatten()</span><span class="sxs-lookup"><span data-stu-id="a13f4-338">&nbsp;&nbsp;&nbsp;&nbsp;.flatten()</span></span><br><span data-ttu-id="a13f4-339">&nbsp;&nbsp;&nbsp;&nbsp;.Value()</span><span class="sxs-lookup"><span data-stu-id="a13f4-339">&nbsp;&nbsp;&nbsp;&nbsp;.value()</span></span>|<span data-ttu-id="a13f4-340">6</span><span class="sxs-lookup"><span data-stu-id="a13f4-340">6</span></span>|

<span data-ttu-id="a13f4-341">hello következő leírások ismertetik a fenti hello tábla minden egyes lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="a13f4-341">hello following descriptions explain each query in hello table above.</span></span>
1. <span data-ttu-id="a13f4-342">Az összes dokumentumok (a folytatási kód paginated) eredményez.</span><span class="sxs-lookup"><span data-stu-id="a13f4-342">Results in all documents (paginated with continuation token) as is.</span></span>
2. <span data-ttu-id="a13f4-343">Projektek hello azonosítója, az üzenet (aliasnevet toomsg) és a művelet az összes dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="a13f4-343">Projects hello id, message (aliased toomsg), and action from all documents.</span></span>
3. <span data-ttu-id="a13f4-344">A dokumentumok hello predikátum lekérdezések: azonosító = "X998_Y998".</span><span class="sxs-lookup"><span data-stu-id="a13f4-344">Queries for documents with hello predicate: id = "X998_Y998".</span></span>
4. <span data-ttu-id="a13f4-345">A lekérdezések, amelyek a Tags tulajdonság és címkék dokumentumok 123 hello értéket tartalmazó tömb.</span><span class="sxs-lookup"><span data-stu-id="a13f4-345">Queries for documents that have a Tags property and Tags is an array containing hello value 123.</span></span>
5. <span data-ttu-id="a13f4-346">Lekérdezések dokumentumok predikátumával, azonosító = "X998_Y998", és ezután projektek hello azonosítója és (aliasnevet toomsg) üzenet.</span><span class="sxs-lookup"><span data-stu-id="a13f4-346">Queries for documents with a predicate, id = "X998_Y998", and then projects hello id and message (aliased toomsg).</span></span>
6. <span data-ttu-id="a13f4-347">Szűrők tömbtulajdonság, címkék, amelyeknek dokumentumok és hello eredményül kapott dokumentumok rendezése hello _ts időbélyeg-rendszer tulajdonság, majd projektek + simítja hello címkék tömb.</span><span class="sxs-lookup"><span data-stu-id="a13f4-347">Filters for documents which have an array property, Tags, and sorts hello resulting documents by hello _ts timestamp system property, and then projects + flattens hello Tags array.</span></span>


## <a name="runtime-support"></a><span data-ttu-id="a13f4-348">Futásidejű támogatása</span><span class="sxs-lookup"><span data-stu-id="a13f4-348">Runtime support</span></span>
<span data-ttu-id="a13f4-349">[A DocumentDB a JavaScript kiszolgáló oldalán API](http://azure.github.io/azure-documentdb-js-server/) támogatást nyújt az hello hello többsége alapvető technikai JavaScript nyelv szolgáltatások által szabványosított [ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm).</span><span class="sxs-lookup"><span data-stu-id="a13f4-349">[DocumentDB JavaScript server side API](http://azure.github.io/azure-documentdb-js-server/) provides support for hello most of hello mainstream JavaScript language features as standardized by [ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm).</span></span>

### <a name="security"></a><span data-ttu-id="a13f4-350">Biztonság</span><span class="sxs-lookup"><span data-stu-id="a13f4-350">Security</span></span>
<span data-ttu-id="a13f4-351">JavaScript tárolt eljárások és eseményindítók elkülönített, hogy egy parancsfájl hello hatásait nem szivárognak más toohello hello pillanatkép-tranzakció elkülönítés hello adatbázis szintjén áthaladás nélkül.</span><span class="sxs-lookup"><span data-stu-id="a13f4-351">JavaScript stored procedures and triggers are sandboxed so that hello effects of one script do not leak toohello other without going through hello snapshot transaction isolation at hello database level.</span></span> <span data-ttu-id="a13f4-352">hello futtatási környezetekben a készletbe vonást, de tisztítani hello környezet után minden egyes futtatásához.</span><span class="sxs-lookup"><span data-stu-id="a13f4-352">hello runtime environments are pooled but cleaned of hello context after each run.</span></span> <span data-ttu-id="a13f4-353">Ezért ezek garantáltan toobe biztonságos az oldal nem várt hatások egymástól.</span><span class="sxs-lookup"><span data-stu-id="a13f4-353">Hence they are guaranteed toobe safe of any unintended side effects from each other.</span></span>

### <a name="pre-compilation"></a><span data-ttu-id="a13f4-354">A fordítás előtti</span><span class="sxs-lookup"><span data-stu-id="a13f4-354">Pre-compilation</span></span>
<span data-ttu-id="a13f4-355">Tárolt eljárások, eseményindítók és felhasználó által megadott függvények, hello minden parancsfájl foglalja találhatók az implicit módon lefordított toohello bájt kód formátum rendelés tooavoid fordítási költséggel jár.</span><span class="sxs-lookup"><span data-stu-id="a13f4-355">Stored procedures, triggers and UDFs are implicitly precompiled toohello byte code format in order tooavoid compilation cost at hello time of each script invocation.</span></span> <span data-ttu-id="a13f4-356">Ez biztosítja a tárolt eljárások indítások gyors, és egy kis erőforrásigényét.</span><span class="sxs-lookup"><span data-stu-id="a13f4-356">This ensures invocations of stored procedures are fast and have a low footprint.</span></span>

## <a name="client-sdk-support"></a><span data-ttu-id="a13f4-357">Ügyfél SDK-támogatás</span><span class="sxs-lookup"><span data-stu-id="a13f4-357">Client SDK support</span></span>
<span data-ttu-id="a13f4-358">A hozzáadása toohello DocumentDB API a [Node.js](documentdb-sdk-node.md) ügyfél, Azure Cosmos DB rendelkezik [.NET](documentdb-sdk-dotnet.md), [.NET Core](documentdb-sdk-dotnet-core.md), [Java](documentdb-sdk-java.md), [ JavaScript](http://azure.github.io/azure-documentdb-js/), és [Python SDK-k](documentdb-sdk-python.md) hello DocumentDB API számára.</span><span class="sxs-lookup"><span data-stu-id="a13f4-358">In addition toohello DocumentDB API for [Node.js](documentdb-sdk-node.md) client, Azure Cosmos DB has [.NET](documentdb-sdk-dotnet.md), [.NET Core](documentdb-sdk-dotnet-core.md), [Java](documentdb-sdk-java.md), [JavaScript](http://azure.github.io/azure-documentdb-js/), and [Python SDKs](documentdb-sdk-python.md) for hello DocumentDB API.</span></span> <span data-ttu-id="a13f4-359">Tárolt eljárások, eseményindítók és felhasználó által megadott függvények hozhatók létre, és végre bármely, valamint a SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="a13f4-359">Stored procedures, triggers and UDFs can be created and executed using any of these SDKs as well.</span></span> <span data-ttu-id="a13f4-360">a következő példa azt mutatja meg hogyan hello toocreate, majd hajtsa végre a tárolt eljárás hello .NET ügyfél használatával.</span><span class="sxs-lookup"><span data-stu-id="a13f4-360">hello following example shows how toocreate and execute a stored procedure using hello .NET client.</span></span> <span data-ttu-id="a13f4-361">Vegye figyelembe, hogyan hello .NET típusok átadott hello JSON-ként tárolt eljárást, és olvasási vissza.</span><span class="sxs-lookup"><span data-stu-id="a13f4-361">Note how hello .NET types are passed into hello stored procedure as JSON and read back.</span></span>

    var markAntiquesSproc = new StoredProcedure
    {
        Id = "ValidateDocumentAge",
        Body = @"
                function(docToCreate, antiqueYear) {
                    var collection = getContext().getCollection();    
                    var response = getContext().getResponse();    

                    if(docToCreate.Year != undefined && docToCreate.Year < antiqueYear){
                        docToCreate.antique = true;
                    }

                    collection.createDocument(collection.getSelfLink(), docToCreate, {}, 
                        function(err, docCreated, options) { 
                            if(err) throw new Error('Error while creating document: ' + err.message);                              
                            if(options.maxCollectionSizeInMb == 0) throw 'max collection size not found'; 
                            response.setBody(docCreated);
                    });
             }"
    };

    // register stored procedure
    StoredProcedure createdStoredProcedure = await client.CreateStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), markAntiquesSproc);
    dynamic document = new Document() { Id = "Borges_112" };
    document.Title = "Aleph";
    document.Year = 1949;

    // execute stored procedure
    Document createdDocument = await client.ExecuteStoredProcedureAsync<Document>(UriFactory.CreateStoredProcedureUri("db", "coll", "sproc"), document, 1920);


<span data-ttu-id="a13f4-362">Ez a példa bemutatja, hogyan toouse hello [DocumentDB .NET API](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) toocreate előtti eseményindítót, és hozzon létre egy dokumentum hello eseményindító engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="a13f4-362">This sample shows how toouse hello [DocumentDB .NET API](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) toocreate a pre-trigger and create a document with hello trigger enabled.</span></span> 

    Trigger preTrigger = new Trigger()
    {
        Id = "CapitalizeName",
        Body = @"function() {
            var item = getContext().getRequest().getBody();
            item.id = item.id.toUpperCase();
            getContext().getRequest().setBody(item);
        }",
        TriggerOperation = TriggerOperation.Create,
        TriggerType = TriggerType.Pre
    };

    Document createdItem = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), new Document { Id = "documentdb" },
        new RequestOptions
        {
            PreTriggerInclude = new List<string> { "CapitalizeName" },
        });


<span data-ttu-id="a13f4-363">És hello a következő példa bemutatja, hogyan toocreate egy felhasználói függvény (UDF), és ezért a [DocumentDB API SQL-lekérdezés](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="a13f4-363">And hello following example shows how toocreate a user defined function (UDF) and use it in a [DocumentDB API SQL query](documentdb-sql-query.md).</span></span>

    UserDefinedFunction function = new UserDefinedFunction()
    {
        Id = "LOWER",
        Body = @"function(input) 
        {
            return input.toLowerCase();
        }"
    };

    foreach (Book book in client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        "SELECT * FROM Books b WHERE udf.LOWER(b.Title) = 'war and peace'"))
    {
        Console.WriteLine("Read {0} from query", book);
    }

## <a name="rest-api"></a><span data-ttu-id="a13f4-364">REST API</span><span class="sxs-lookup"><span data-stu-id="a13f4-364">REST API</span></span>
<span data-ttu-id="a13f4-365">Minden Azure Cosmos DB műveletet RESTful módon végezheti el.</span><span class="sxs-lookup"><span data-stu-id="a13f4-365">All Azure Cosmos DB operations can be performed in a RESTful manner.</span></span> <span data-ttu-id="a13f4-366">Tárolt eljárások, eseményindítók és felhasználó által definiált függvények regisztrálhatók egy gyűjteményt a HTTP POST használatával.</span><span class="sxs-lookup"><span data-stu-id="a13f4-366">Stored procedures, triggers and user-defined functions can be registered under a collection by using HTTP POST.</span></span> <span data-ttu-id="a13f4-367">hello az alábbiakban látható egy példa bemutatja, hogyan tooregister tárolt eljárást:</span><span class="sxs-lookup"><span data-stu-id="a13f4-367">hello following is an example of how tooregister a stored procedure:</span></span>

    POST https://<url>/sprocs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT


    var x = {
      "name": "createAndAddProperty",
      "body": function (docToCreate, addedPropertyName, addedPropertyValue) {
                var collectionManager = getContext().getCollection();
                collectionManager.createDocument(
                    collectionManager.getSelfLink(),
                    docToCreate,
                    function(err, docCreated) {
                      if(err) throw new Error('Error:  ' + err.message);
                      docCreated[addedPropertyName] = addedPropertyValue;
                      getContext().getResponse().setBody(docCreated);
                    });
            }
    }


<span data-ttu-id="a13f4-368">hello tárolt eljárás regisztrálva van-e a POST kérelmet hello URI végrehajtásával adatbázisok/testdb/colls/testColl/sprocs hello törzs tartalmazó a tárolt eljárás toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="a13f4-368">hello stored procedure is registered by executing a POST request against hello URI dbs/testdb/colls/testColl/sprocs with hello body containing hello stored procedure toocreate.</span></span> <span data-ttu-id="a13f4-369">Eseményindítók és felhasználó által megadott függvények hasonlóan a FELADÁS egy vagy több/eseményindítók és /udfs rendre kiállításával regisztrálható.</span><span class="sxs-lookup"><span data-stu-id="a13f4-369">Triggers and UDFs can be registered similarly by issuing a POST against /triggers and /udfs respectively.</span></span>
<span data-ttu-id="a13f4-370">Ez tárolt eljárást is, majd a POST kérelmet az erőforrás-hivatkozás kiállításával hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="a13f4-370">This stored procedure can then be executed by issuing a POST request against its resource link:</span></span>

    POST https://<url>/sprocs/<sproc> HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:20 GMT


    [ { "name": "TestDocument", "book": "Autumn of hello Patriarch"}, "Price", 200 ]


<span data-ttu-id="a13f4-371">Itt hello bemeneti toohello tárolt eljárás lett átadva hello kérés törzsében.</span><span class="sxs-lookup"><span data-stu-id="a13f4-371">Here, hello input toohello stored procedure is passed in hello request body.</span></span> <span data-ttu-id="a13f4-372">Vegye figyelembe, hogy hello bemeneti, egy JSON-tömb, a bemeneti paraméterek átadása.</span><span class="sxs-lookup"><span data-stu-id="a13f4-372">Note that hello input is passed as a JSON array of input parameters.</span></span> <span data-ttu-id="a13f4-373">hello tárolt eljárás vesz hello első bemenet, amely egy adott válasz törzsének-dokumentumként.</span><span class="sxs-lookup"><span data-stu-id="a13f4-373">hello stored procedure takes hello first input as a document that is a response body.</span></span> <span data-ttu-id="a13f4-374">hello válasz érkező a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="a13f4-374">hello response we receive is as follows:</span></span>

    HTTP/1.1 200 OK

    { 
      name: 'TestDocument',
      book: ‘Autumn of hello Patriarch’,
      id: ‘V7tQANV3rAkDAAAAAAAAAA==‘,
      ts: 1407830727,
      self: ‘dbs/V7tQAA==/colls/V7tQANV3rAk=/docs/V7tQANV3rAkDAAAAAAAAAA==/’,
      etag: ‘6c006596-0000-0000-0000-53e9cac70000’,
      attachments: ‘attachments/’,
      Price: 200
    }


<span data-ttu-id="a13f4-375">Ellentétben a tárolt eljárások, eseményindítók közvetlenül nem hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="a13f4-375">Triggers, unlike stored procedures, cannot be executed directly.</span></span> <span data-ttu-id="a13f4-376">Ehelyett végrehajtás egy dokumentumot egy művelet részeként.</span><span class="sxs-lookup"><span data-stu-id="a13f4-376">Instead they are executed as part of an operation on a document.</span></span> <span data-ttu-id="a13f4-377">A kérelem HTTP-fejlécek használatával hello eseményindítók toorun azt adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="a13f4-377">We can specify hello triggers toorun with a request using HTTP headers.</span></span> <span data-ttu-id="a13f4-378">hello kérelem toocreate dokumentum látható.</span><span class="sxs-lookup"><span data-stu-id="a13f4-378">hello following is request toocreate a document.</span></span>

    POST https://<url>/docs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    x-ms-documentdb-pre-trigger-include: validateDocumentContents 
    x-ms-documentdb-post-trigger-include: bookCreationPostTrigger


    {
       "name": "newDocument",
       “title”: “hello Wizard of Oz”,
       “author”: “Frank Baum”,
       “pages”: 92
    }


<span data-ttu-id="a13f4-379">Itt hello előtti eseményindító toobe hello kérelem hibaüzenettel hello x-ms-documentdb-pre-trigger-include fejlécben megadott.</span><span class="sxs-lookup"><span data-stu-id="a13f4-379">Here hello pre-trigger toobe run with hello request is specified in hello x-ms-documentdb-pre-trigger-include header.</span></span> <span data-ttu-id="a13f4-380">Ennek megfelelően a utáni eseményindítókat hello x-ms-documentdb-post-trigger-include fejléc szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="a13f4-380">Correspondingly, any post-triggers are given in hello x-ms-documentdb-post-trigger-include header.</span></span> <span data-ttu-id="a13f4-381">Vegye figyelembe, hogy mindkét előtti és utáni eseményindítók adható meg egy adott kérés esetében.</span><span class="sxs-lookup"><span data-stu-id="a13f4-381">Note that both pre- and post-triggers can be specified for a given request.</span></span>

## <a name="sample-code"></a><span data-ttu-id="a13f4-382">Mintakód</span><span class="sxs-lookup"><span data-stu-id="a13f4-382">Sample code</span></span>
<span data-ttu-id="a13f4-383">További példákat kiszolgálóoldali található (beleértve a [tömeges törlési](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js), és [frissítése](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)) a a [GitHub-tárházban](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples).</span><span class="sxs-lookup"><span data-stu-id="a13f4-383">You can find more server-side code examples (including [bulk-delete](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js), and [update](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)) on our [GitHub repository](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples).</span></span>

<span data-ttu-id="a13f4-384">Tooshare szeretné, hogy a Soft tárolt eljárás?</span><span class="sxs-lookup"><span data-stu-id="a13f4-384">Want tooshare your awesome stored procedure?</span></span> <span data-ttu-id="a13f4-385">Kérjük küldjön egy lekérést!</span><span class="sxs-lookup"><span data-stu-id="a13f4-385">Please, send us a pull-request!</span></span> 

## <a name="next-steps"></a><span data-ttu-id="a13f4-386">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a13f4-386">Next steps</span></span>
<span data-ttu-id="a13f4-387">Miután egy vagy több tárolt eljárások, eseményindítók és felhasználó által definiált függvények létrehozott, betöltve helyezheti, és megtekintheti hello adatkezelő Azure portálra.</span><span class="sxs-lookup"><span data-stu-id="a13f4-387">Once you have one or more stored procedures, triggers, and user-defined functions created, you can load them and view them in hello Azure portal using Data Explorer.</span></span>

<span data-ttu-id="a13f4-388">Azt is tapasztalhatja hello következő hivatkozások és erőforrások az az elérési út toolearn Azure Cosmos dB kiszolgálóoldali programozása kapcsolatos további hasznos információkat:</span><span class="sxs-lookup"><span data-stu-id="a13f4-388">You may also find hello following references and resources useful in your path toolearn more about Azure Cosmos dB server-side programming:</span></span>

* [<span data-ttu-id="a13f4-389">Az Azure Cosmos DB SDK-k</span><span class="sxs-lookup"><span data-stu-id="a13f4-389">Azure Cosmos DB SDKs</span></span>](documentdb-sdk-dotnet.md)
* [<span data-ttu-id="a13f4-390">A DocumentDB Studio</span><span class="sxs-lookup"><span data-stu-id="a13f4-390">DocumentDB Studio</span></span>](https://github.com/mingaliu/DocumentDBStudio/releases)
* [<span data-ttu-id="a13f4-391">JSON</span><span class="sxs-lookup"><span data-stu-id="a13f4-391">JSON</span></span>](http://www.json.org/) 
* [<span data-ttu-id="a13f4-392">JavaScript ECMA-262</span><span class="sxs-lookup"><span data-stu-id="a13f4-392">JavaScript ECMA-262</span></span>](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
* [<span data-ttu-id="a13f4-393">Biztonságos és hordozható adatbázis bővíthetőség</span><span class="sxs-lookup"><span data-stu-id="a13f4-393">Secure and Portable Database Extensibility</span></span>](http://dl.acm.org/citation.cfm?id=276339) 
* [<span data-ttu-id="a13f4-394">Szolgáltatás készült adatbázis-architektúra</span><span class="sxs-lookup"><span data-stu-id="a13f4-394">Service Oriented Database Architecture</span></span>](http://dl.acm.org/citation.cfm?id=1066267&coll=Portal&dl=GUIDE) 
* [<span data-ttu-id="a13f4-395">A Microsoft SQL server üzemeltetési hello .NET-futtatókörnyezet</span><span class="sxs-lookup"><span data-stu-id="a13f4-395">Hosting hello .NET Runtime in Microsoft SQL server</span></span>](http://dl.acm.org/citation.cfm?id=1007669)

