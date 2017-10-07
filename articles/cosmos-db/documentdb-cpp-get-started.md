---
title: "Azure Cosmos DB aaaC ++ oktatóanyaga |} Microsoft Docs"
description: "Ez egy C++ oktatóanyag, amely egy C++ adatbázis és egy konzolalkalmazás létrehozását ismerteti az Azure Cosmos DB által támogatott C++ SDK használatával. Az Azure Cosmos DB egy világméretű adatbázis-szolgáltatás."
services: cosmos-db
documentationcenter: cpp
author: asthana86
manager: jhubbard
editor: 
ms.assetid: b8756b60-8d41-4231-ba4f-6cfcfe3b4bab
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: cpp
ms.topic: article
ms.date: 12/25/2016
ms.author: aasthan
ms.openlocfilehash: 2d5eeff349b7753e39936b7eb77557ad30c5830a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-c-console-application-tutorial-for-hello-documentdb-api"></a><span data-ttu-id="6bdf2-104">Az Azure Cosmos DB: C++ konzol oktatóanyag a DocumentDB API hello</span><span class="sxs-lookup"><span data-stu-id="6bdf2-104">Azure Cosmos DB: C++ console application tutorial for hello DocumentDB API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6bdf2-105">.NET</span><span class="sxs-lookup"><span data-stu-id="6bdf2-105">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="6bdf2-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="6bdf2-106">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="6bdf2-107">Node.js MongoDB-hez</span><span class="sxs-lookup"><span data-stu-id="6bdf2-107">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="6bdf2-108">Node.js</span><span class="sxs-lookup"><span data-stu-id="6bdf2-108">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="6bdf2-109">Java</span><span class="sxs-lookup"><span data-stu-id="6bdf2-109">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="6bdf2-110">C++</span><span class="sxs-lookup"><span data-stu-id="6bdf2-110">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 
 

<span data-ttu-id="6bdf2-111">Üdvözli a toohello C++ útmutató hello Azure Cosmos DB DocumentDB API SDK for C++ által támogatott!</span><span class="sxs-lookup"><span data-stu-id="6bdf2-111">Welcome toohello C++ tutorial for hello Azure Cosmos DB DocumentDB API endorsed SDK for C++!</span></span> <span data-ttu-id="6bdf2-112">Az oktatóanyag lépéseinek követésével egy olyan konzolalkalmazást hozhat létre, amely Azure Cosmos DB-erőforrásokat (például C++ adatbázisokat) hoz létre és kérdez le.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-112">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources, including a C++ database.</span></span>

<span data-ttu-id="6bdf2-113">Az oktatóanyag a következőket ismerteti:</span><span class="sxs-lookup"><span data-stu-id="6bdf2-113">We'll cover:</span></span>

* <span data-ttu-id="6bdf2-114">Hoz létre és csatlakoztatja tooan Azure Cosmos DB fiók</span><span class="sxs-lookup"><span data-stu-id="6bdf2-114">Creating and connecting tooan Azure Cosmos DB account</span></span>
* <span data-ttu-id="6bdf2-115">Az alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="6bdf2-115">Setting up your application</span></span>
* <span data-ttu-id="6bdf2-116">C++ Azure Cosmos DB-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="6bdf2-116">Creating a C++ Azure Cosmos DB database</span></span>
* <span data-ttu-id="6bdf2-117">Gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="6bdf2-117">Creating a collection</span></span>
* <span data-ttu-id="6bdf2-118">JSON-dokumentumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="6bdf2-118">Creating JSON documents</span></span>
* <span data-ttu-id="6bdf2-119">Hello gyűjtemény lekérdezése</span><span class="sxs-lookup"><span data-stu-id="6bdf2-119">Querying hello collection</span></span>
* <span data-ttu-id="6bdf2-120">Dokumentum cseréje</span><span class="sxs-lookup"><span data-stu-id="6bdf2-120">Replacing a document</span></span>
* <span data-ttu-id="6bdf2-121">Dokumentum törlése</span><span class="sxs-lookup"><span data-stu-id="6bdf2-121">Deleting a document</span></span>
* <span data-ttu-id="6bdf2-122">Hello C++ Azure Cosmos DB adatbázis törlése</span><span class="sxs-lookup"><span data-stu-id="6bdf2-122">Deleting hello C++ Azure Cosmos DB database</span></span>

<span data-ttu-id="6bdf2-123">Nincs elég ideje?</span><span class="sxs-lookup"><span data-stu-id="6bdf2-123">Don't have time?</span></span> <span data-ttu-id="6bdf2-124">Ne aggódjon!</span><span class="sxs-lookup"><span data-stu-id="6bdf2-124">Don't worry!</span></span> <span data-ttu-id="6bdf2-125">a teljes megoldás hello érhető [GitHub](https://github.com/stalker314314/DocumentDBCpp).</span><span class="sxs-lookup"><span data-stu-id="6bdf2-125">hello complete solution is available on [GitHub](https://github.com/stalker314314/DocumentDBCpp).</span></span> <span data-ttu-id="6bdf2-126">Lásd: [hello teljes megoldás beszerzése](#GetSolution) gyors utasításokért.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-126">See [Get hello complete solution](#GetSolution) for quick instructions.</span></span>

<span data-ttu-id="6bdf2-127">Hello C++ az oktatóanyag befejezése után adjon használata hello szavazás gombok hello alján a lap toogive nekünk visszajelzést.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-127">After you've completed hello C++ tutorial, please use hello voting buttons at hello bottom of this page toogive us feedback.</span></span> 

<span data-ttu-id="6bdf2-128">Ha szeretné toocontact úgy közvetlenül, érzi, hogy az e-mail cím szabad tooinclude megjegyzéseit a vagy [elérhetők a toous Itt](https://www.research.net/r/8BKRJ3Z).</span><span class="sxs-lookup"><span data-stu-id="6bdf2-128">If you'd like us toocontact you directly, feel free tooinclude your email address in your comments or [reach out toous here](https://www.research.net/r/8BKRJ3Z).</span></span> 

<span data-ttu-id="6bdf2-129">Most pedig lássunk neki!</span><span class="sxs-lookup"><span data-stu-id="6bdf2-129">Now let's get started!</span></span>

## <a name="prerequisites-for-hello-c-tutorial"></a><span data-ttu-id="6bdf2-130">Hello C++ oktatóanyag előfeltételei</span><span class="sxs-lookup"><span data-stu-id="6bdf2-130">Prerequisites for hello C++ tutorial</span></span>
<span data-ttu-id="6bdf2-131">Győződjön meg arról, hogy a következő hello:</span><span class="sxs-lookup"><span data-stu-id="6bdf2-131">Please make sure you have hello following:</span></span>

* <span data-ttu-id="6bdf2-132">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-132">An active Azure account.</span></span> <span data-ttu-id="6bdf2-133">Ha még nincs fiókja, regisztrálhat az [Azure ingyenes próbaverziójára](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6bdf2-133">If you don't have one, you can sign up for a [Free Azure Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="6bdf2-134">[A Visual Studio](https://www.visualstudio.com/downloads/), amelyben hello C++ nyelvi összetevő telepítve van.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-134">[Visual Studio](https://www.visualstudio.com/downloads/), with hello C++ language components installed.</span></span>

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="6bdf2-135">1. lépés: Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="6bdf2-135">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="6bdf2-136">Hozzunk létre egy Azure Cosmos DB-fiókot.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-136">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="6bdf2-137">Ha már rendelkezik toouse kívánt fiókkal, akkor kihagyhatja azokat, amelyek túl[a C++-alkalmazás beállítása](#SetupNode).</span><span class="sxs-lookup"><span data-stu-id="6bdf2-137">If you already have an account you want toouse, you can skip ahead too[Setup your C++ application](#SetupNode).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="6bdf2-138"><a id="SetupC++"></a>2. lépés: A C++ alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="6bdf2-138"><a id="SetupC++"></a>Step 2: Set up your C++ application</span></span>
1. <span data-ttu-id="6bdf2-139">Nyissa meg a Visual Studio, majd a hello **fájl** menüben kattintson a **új**, és kattintson a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-139">Open Visual Studio, and then on hello **File** menu, click **New**, and then click **Project**.</span></span> 
2. <span data-ttu-id="6bdf2-140">A hello **új projekt** ablak hello **telepített** ablaktáblában bontsa ki a **Visual C++**, kattintson a **Win32**, és kattintson a  **A Win32-Konzolalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-140">In hello **New Project** window, in hello **Installed** pane, expand **Visual C++**, click **Win32**, and then click **Win32 Console Application**.</span></span> <span data-ttu-id="6bdf2-141">Hello projekt hellodocumentdb nevet és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-141">Name hello project hellodocumentdb and then click **OK**.</span></span> 
   
    ![Képernyőfelvétel a hello új projekt varázsló](media/documentdb-cpp-get-started/hello.png)
3. <span data-ttu-id="6bdf2-143">Hello Win32 alkalmazás varázsló elindításakor kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-143">When hello Win32 Application Wizard starts, click **Finish**.</span></span>
4. <span data-ttu-id="6bdf2-144">Hello projekt létrehozása után, nyissa meg a NuGet-Csomagkezelő hello kattintson a jobb gombbal a hello **hellodocumentdb** a projekt **Megoldáskezelőben** elemre kattintva **NuGet-csomagokkezelése**.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-144">Once hello project has been created, open hello NuGet package manager by right-clicking hello **hellodocumentdb** project in **Solution Explorer** and clicking **Manage NuGet Packages**.</span></span> 
   
    ![Hello Projekt menü NuGet-csomag kezelni ábrázoló képernyőfelvétel](media/documentdb-cpp-get-started/nuget.png)
5. <span data-ttu-id="6bdf2-146">A hello **NuGet: hellodocumentdb** lapra, majd **Tallózás**, majd keresse meg a *documentdbcpp*.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-146">In hello **NuGet: hellodocumentdb** tab, click **Browse**, and then search for *documentdbcpp*.</span></span> <span data-ttu-id="6bdf2-147">Hello eredményeit válassza ki DocumentDbCPP, ahogy az alábbi képernyőfelvétel a hello.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-147">In hello results, select DocumentDbCPP, as shown in hello following screenshot.</span></span> <span data-ttu-id="6bdf2-148">Ez a csomag telepíti hivatkozások tooC ++ REST SDK, vagyis hello DocumentDbCPP függőségei.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-148">This package installs references tooC++ REST SDK, which is a dependency for hello DocumentDbCPP.</span></span>  
   
    ![Képernyőfelvétel: hello DocumentDbCpp csomag kiemelve](media/documentdb-cpp-get-started/cpp.png)
   
    <span data-ttu-id="6bdf2-150">Hello csomagok tooyour projekt lettek hozzáadva, ha folyamatban, minden set toostart néhány kódot ír.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-150">Once hello packages have been added tooyour project, we are all set toostart writing some code.</span></span>   

## <span data-ttu-id="6bdf2-151"><a id="Config"></a>3. lépés: Kapcsolat részleteinek másolása az Azure Portalról az Azure Cosmos DB-adatbázisba</span><span class="sxs-lookup"><span data-stu-id="6bdf2-151"><a id="Config"></a>Step 3: Copy connection details from Azure portal for your Azure Cosmos DB database</span></span>
<span data-ttu-id="6bdf2-152">Nyithat [Azure-portálon](https://portal.azure.com) és bejárhat toohello létrehozott Azure Cosmos DB adatbázis fiókot.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-152">Bring up [Azure portal](https://portal.azure.com) and traverse toohello Azure Cosmos DB database account you created.</span></span> <span data-ttu-id="6bdf2-153">Fel kell hello URI és elsődleges kulcs hello hello tovább lépés tooestablish kapcsolatot az Azure portálról származó a C++ kódrészletet.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-153">We will need hello URI and hello primary key from Azure portal in hello next step tooestablish a connection from our C++ code snippet.</span></span> 

![Az Azure Cosmos DB URI és a kulcsot hello Azure-portálon](media/documentdb-cpp-get-started/nosql-tutorial-keys.png)

## <span data-ttu-id="6bdf2-155"><a id="Connect"></a>4. lépés: Csatlakozás tooan Azure Cosmos DB fiók</span><span class="sxs-lookup"><span data-stu-id="6bdf2-155"><a id="Connect"></a>Step 4: Connect tooan Azure Cosmos DB account</span></span>
1. <span data-ttu-id="6bdf2-156">Adja hozzá a következő fejlécek és névterek tooyour forráskódját, után hello `#include "stdafx.h"`.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-156">Add hello following headers and namespaces tooyour source code, after `#include "stdafx.h"`.</span></span>
   
        #include <cpprest/json.h>
        #include <documentdbcpp\DocumentClient.h>
        #include <documentdbcpp\exceptions.h>
        #include <documentdbcpp\TriggerOperation.h>
        #include <documentdbcpp\TriggerType.h>
        using namespace documentdb;
        using namespace std;
        using namespace web::json;
2. <span data-ttu-id="6bdf2-157">Ezután adja hozzá a hello alábbi code tooyour fő funkciója, és cserélje hello fiók konfigurálásával és az elsődleges kulcs toomatch a 3. lépés Azure Cosmos DB beállításait.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-157">Next add hello following code tooyour main function and replace hello account configuration and primary key toomatch your Azure Cosmos DB settings from step 3.</span></span> 
   
        DocumentDBConfiguration conf (L"<account_configuration_uri>", L"<primary_key>");
        DocumentClient client (conf);
   
    <span data-ttu-id="6bdf2-158">Most, hogy hello kód tooinitialize hello documentdb-ügyfél, vessen egy pillantást a Azure Cosmos DB erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-158">Now that you have hello code tooinitialize hello documentdb client, let's take a look at working with Azure Cosmos DB resources.</span></span>

## <span data-ttu-id="6bdf2-159"><a id="CreateDBColl"></a>5. lépés: C++ adatbázis és gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="6bdf2-159"><a id="CreateDBColl"></a>Step 5: Create a C++ database and collection</span></span>
<span data-ttu-id="6bdf2-160">Most elvégezni ezt a lépést, mielőtt ugorjunk hogyan működnek együtt a adatbázis, gyűjtemény és dokumentumok azok is, akik új tooAzure Cosmos DB keresztül.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-160">Before we perform this step, let's go over how a database, collection and documents interact for those of you who are new tooAzure Cosmos DB.</span></span> <span data-ttu-id="6bdf2-161">Az [adatbázisok](documentdb-resources.md#databases) a dokumentumtároló gyűjtemények között particionált logikai tárolói.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-161">A [database](documentdb-resources.md#databases) is a logical container of document storage portioned across collections.</span></span> <span data-ttu-id="6bdf2-162">A [gyűjtemény](documentdb-resources.md#collections) JSON-dokumentumok és hello kapcsolódó JavaScript-alkalmazáslogika.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-162">A [collection](documentdb-resources.md#collections) is a container of JSON documents and hello associated JavaScript application logic.</span></span> <span data-ttu-id="6bdf2-163">További információ a hello Azure Cosmos DB hierarchikus erőforrás-modellje és fogalmai terhelésekről [Azure Cosmos DB hierarchikus erőforrás-modellje és fogalmai](documentdb-resources.md).</span><span class="sxs-lookup"><span data-stu-id="6bdf2-163">You can learn more about hello Azure Cosmos DB hierarchical resource model and concepts in [Azure Cosmos DB hierarchical resource model and concepts](documentdb-resources.md).</span></span>

<span data-ttu-id="6bdf2-164">toocreate adatbázis és a megfelelő gyűjtemény hozzáadása a következő kód toohello vége a fő funkciója hello.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-164">toocreate a database and a corresponding collection add hello following code toohello end of your main function.</span></span> <span data-ttu-id="6bdf2-165">Ez egy "FamilyRegistry" és a hello ügyfél konfigurációs meg az előző lépésben hello deklarált FamilyCollection nevű gyűjtemény nevű adatbázist hoz létre.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-165">This creates a database called 'FamilyRegistry’ and a collection called ‘FamilyCollection’ using hello client configuration you declared in hello previous step.</span></span>

    try {
      shared_ptr<Database> db = client.CreateDatabase(L"FamilyRegistry");
      shared_ptr<Collection> coll = db->CreateCollection(L"FamilyCollection");
    } catch (DocumentDBRuntimeException ex) {
      wcout << ex.message();
    }


## <span data-ttu-id="6bdf2-166"><a id="CreateDoc"></a>6. lépés: Dokumentum létrehozása</span><span class="sxs-lookup"><span data-stu-id="6bdf2-166"><a id="CreateDoc"></a>Step 6: Create a document</span></span>
<span data-ttu-id="6bdf2-167">A [dokumentumok](documentdb-resources.md#documents) a felhasználó által megadott (tetszőleges) JSON-tartalmak.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-167">[Documents](documentdb-resources.md#documents) are user-defined (arbitrary) JSON content.</span></span> <span data-ttu-id="6bdf2-168">Most már beszúrhat egy dokumentumot az Azure Cosmos DB-be.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-168">You can now insert a document into Azure Cosmos DB.</span></span> <span data-ttu-id="6bdf2-169">Létrehozhat egy dokumentum kód hello fő funkciója hello végét a következő hello másolásával.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-169">You can create a document by copying hello following code into hello end of hello main function.</span></span> 

    try {
      value document_family;
      document_family[L"id"] = value::string(L"AndersenFamily");
      document_family[L"FirstName"] = value::string(L"Thomas");
      document_family[L"LastName"] = value::string(L"Andersen");
      shared_ptr<Document> doc = coll->CreateDocumentAsync(document_family).get();

      document_family[L"id"] = value::string(L"WakefieldFamily");
      document_family[L"FirstName"] = value::string(L"Lucy");
      document_family[L"LastName"] = value::string(L"Wakefield");
      doc = coll->CreateDocumentAsync(document_family).get();
    } catch (ResourceAlreadyExistsException ex) {
      wcout << ex.message();
    }

<span data-ttu-id="6bdf2-170">toosummarize, ez a kód létrehoz egy Azure Cosmos DB adatbázis, gyűjtemény és dokumentumok, amely Azure-portálon található Dokumentumtallózó lekérdezheti.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-170">toosummarize, this code creates an Azure Cosmos DB database, collection, and documents, which you can query in Document Explorer in Azure portal.</span></span> 

![C++ útmutató – hello fiók, hello adatbázis, gyűjtemény hello és hello dokumentumok hierarchikus kapcsolatát hello ábrázoló Diagram](media/documentdb-cpp-get-started/docs.png)

## <span data-ttu-id="6bdf2-172"><a id="QueryDB"></a>7. lépés: Az Azure Cosmos DB-erőforrások lekérdezése</span><span class="sxs-lookup"><span data-stu-id="6bdf2-172"><a id="QueryDB"></a>Step 7: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="6bdf2-173">Az Azure Cosmos DB támogatja az egyes gyűjteményekben tárolt JSON-dokumentumokon végzett [részletes lekérdezéseket](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="6bdf2-173">Azure Cosmos DB supports [rich queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span> <span data-ttu-id="6bdf2-174">hello alábbi mintakód bemutatja hello előző lépésben létrehozott is futtathatók hello dokumentumok SQL-szintaxis használatával létrehozott lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-174">hello following sample code shows a query made using SQL syntax that you can run against hello documents we created in hello previous step.</span></span>

<span data-ttu-id="6bdf2-175">hello függvény akkor argumentumok hello egyedi azonosítója vagy erőforrás-azonosító hello adatbázis és a hello gyűjtemény hello dokumentum ügyfél együtt.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-175">hello function takes in as arguments hello unique identifier or resource id for hello database and hello collection along with hello document client.</span></span> <span data-ttu-id="6bdf2-176">Adja ezt a kódot a fő függvény elé.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-176">Add this code before main function.</span></span>

    void executesimplequery(const DocumentClient &client,
                            const wstring dbresourceid,
                            const wstring collresourceid) {
      try {
        client.GetDatabase(dbresourceid).get();
        shared_ptr<Database> db = client.GetDatabase(dbresourceid);
        shared_ptr<Collection> coll = db->GetCollection(collresourceid);
        wstring coll_name = coll->id();
        shared_ptr<DocumentIterator> iter =
            coll->QueryDocumentsAsync(wstring(L"SELECT * FROM " + coll_name)).get();
        wcout << "\n\nQuerying collection:";
        while (iter->HasMore()) {
          shared_ptr<Document> doc = iter->Next();
          wstring doc_name = doc->id();
          wcout << "\n\t" << doc_name << "\n";
          wcout << "\t"
                << "[{\"FirstName\":"
                << doc->payload().at(U("FirstName")).as_string()
                << ",\"LastName\":" << doc->payload().at(U("LastName")).as_string()
                << "}]";
        }
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <span data-ttu-id="6bdf2-177"><a id="Replace"></a>8. lépés: Dokumentum cseréje</span><span class="sxs-lookup"><span data-stu-id="6bdf2-177"><a id="Replace"></a>Step 8: Replace a document</span></span>
<span data-ttu-id="6bdf2-178">Azure Cosmos-adatbázis mutatja be a kódját a következő hello támogatja a JSON-dokumentumok cseréjét.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-178">Azure Cosmos DB supports replacing JSON documents, as demonstrated in hello following code.</span></span> <span data-ttu-id="6bdf2-179">Hello executesimplequery függvény után adja hozzá ezt a kódot.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-179">Add this code after hello executesimplequery function.</span></span>

    void replacedocument(const DocumentClient &client, const wstring dbresourceid,
                         const wstring collresourceid,
                         const wstring docresourceid) {
      try {
        client.GetDatabase(dbresourceid).get();
        shared_ptr<Database> db = client.GetDatabase(dbresourceid);
        shared_ptr<Collection> coll = db->GetCollection(collresourceid);
        value newdoc;
        newdoc[L"id"] = value::string(L"WakefieldFamily");
        newdoc[L"FirstName"] = value::string(L"Lucy");
        newdoc[L"LastName"] = value::string(L"Smith Wakefield");
        coll->ReplaceDocument(docresourceid, newdoc);
      } catch (DocumentDBRuntimeException ex) {
        throw;
      }
    }

## <span data-ttu-id="6bdf2-180"><a id="Delete"></a>9. lépés: Dokumentum törlése</span><span class="sxs-lookup"><span data-stu-id="6bdf2-180"><a id="Delete"></a>Step 9: Delete a document</span></span>
<span data-ttu-id="6bdf2-181">Az Azure Cosmos DB támogatja a JSON-dokumentumok törlése, akkor ehhez kód hello replacedocument függvény után a következő, a másolás és beillesztés hello.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-181">Azure Cosmos DB supports deleting JSON documents, you can do so by copy and pasting hello following code after hello replacedocument function.</span></span> 

    void deletedocument(const DocumentClient &client, const wstring dbresourceid,
                        const wstring collresourceid, const wstring docresourceid) {
      try {
        client.GetDatabase(dbresourceid).get();
        shared_ptr<Database> db = client.GetDatabase(dbresourceid);
        shared_ptr<Collection> coll = db->GetCollection(collresourceid);
        coll->DeleteDocumentAsync(docresourceid).get();
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <span data-ttu-id="6bdf2-182"><a id="DeleteDB"></a>10. lépés: Adatbázis törlése</span><span class="sxs-lookup"><span data-stu-id="6bdf2-182"><a id="DeleteDB"></a>Step 10: Delete a database</span></span>
<span data-ttu-id="6bdf2-183">Törlése hello létrehozott adatbázis eltávolítja hello adatbázis és az összes gyermek-erőforrás (gyűjtemények, dokumentumok stb.).</span><span class="sxs-lookup"><span data-stu-id="6bdf2-183">Deleting hello created database removes hello database and all child resources (collections, documents, etc.).</span></span>

<span data-ttu-id="6bdf2-184">Másolja és illessze be a következő kódrészletet (függvény tisztítás) után hello deletedocument függvény tooremove hello adatbázis és az összes hello gyermekerőforrásait hello.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-184">Copy and paste hello following code snippet (function cleanup) after hello deletedocument function tooremove hello database and all hello child resources.</span></span>

    void deletedb(const DocumentClient &client, const wstring dbresourceid) {
      try {
        client.DeleteDatabase(dbresourceid);
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <span data-ttu-id="6bdf2-185"><a id="Run"></a>11. lépés: A teljes C++ alkalmazás futtatása!</span><span class="sxs-lookup"><span data-stu-id="6bdf2-185"><a id="Run"></a>Step 11: Run your C++ application all together!</span></span>
<span data-ttu-id="6bdf2-186">Azt most hozzáadott kódot toocreate, lekérdezése, módosítása és törlése különböző Azure Cosmos DB erőforrások.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-186">We have now added code toocreate, query, modify, and delete different Azure Cosmos DB resources.</span></span>  <span data-ttu-id="6bdf2-187">Ossza meg velünk most hozzá kell fűznie ez fel a fő funkciója az egyes diagnosztikai üzenetek együtt hellodocumentdb.cpp hívások toothese a különböző funkciók hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-187">Let us now wire this up by adding calls toothese different functions from our main function in hellodocumentdb.cpp along with some diagnostic messages.</span></span>

<span data-ttu-id="6bdf2-188">Hello fő funkciója az alkalmazás helyett a kód a következő hello azt is megteheti.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-188">You can do so by replacing hello main function of your application with hello following code.</span></span> <span data-ttu-id="6bdf2-189">Az írási műveletek hello account_configuration_uri és a hello kódra a 3. lépésben másolt primary_key úgy, hogy sor másolási hello értékei vagy újból menteni hello portálról.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-189">This writes over hello account_configuration_uri and primary_key you copied into hello code in Step 3, so save that line or copy hello values in again from hello portal.</span></span> 

    int main() {
        try {
            // Start by defining your account's configuration
            DocumentDBConfiguration conf (L"<account_configuration_uri>", L"<primary_key>");
            // Create your client
            DocumentClient client(conf);
            // Create a new database
            try {
                shared_ptr<Database> db = client.CreateDatabase(L"FamilyDB");
                wcout << "\nCreating database:\n" << db->id();
                // Create a collection inside database
                shared_ptr<Collection> coll = db->CreateCollection(L"FamilyColl");
                wcout << "\n\nCreating collection:\n" << coll->id();
                value document_family;
                document_family[L"id"] = value::string(L"AndersenFamily");
                document_family[L"FirstName"] = value::string(L"Thomas");
                document_family[L"LastName"] = value::string(L"Andersen");
                shared_ptr<Document> doc =
                    coll->CreateDocumentAsync(document_family).get();
                wcout << "\n\nCreating document:\n" << doc->id();
                document_family[L"id"] = value::string(L"WakefieldFamily");
                document_family[L"FirstName"] = value::string(L"Lucy");
                document_family[L"LastName"] = value::string(L"Wakefield");
                doc = coll->CreateDocumentAsync(document_family).get();
                wcout << "\n\nCreating document:\n" << doc->id();
                executesimplequery(client, db->resource_id(), coll->resource_id());
                replacedocument(client, db->resource_id(), coll->resource_id(),
                    doc->resource_id());
                wcout << "\n\nReplaced document:\n" << doc->id();
                executesimplequery(client, db->resource_id(), coll->resource_id());
                deletedocument(client, db->resource_id(), coll->resource_id(),
                    doc->resource_id());
                wcout << "\n\nDeleted document:\n" << doc->id();
                deletedb(client, db->resource_id());
                wcout << "\n\nDeleted db:\n" << db->id();
                cin.get();
            }
            catch (ResourceAlreadyExistsException ex) {
                wcout << ex.message();
            }
        }
        catch (DocumentDBRuntimeException ex) {
            wcout << ex.message();
        }
        cin.get();
    }

<span data-ttu-id="6bdf2-190">Meg kell most képes toobuild és futtathatók a kódot a Visual Studio F5 billentyű lenyomásával vagy másik lehetőségként hello terminálablakot hello alkalmazás keresése és futtatása hello a végrehajtható.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-190">You should now be able toobuild and run your code in Visual Studio by pressing F5 or alternatively in hello terminal window by locating hello application and running hello executable.</span></span> 

<span data-ttu-id="6bdf2-191">Az első lépések alkalmazás kimenetének hello kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-191">You should see hello output of your get started app.</span></span> <span data-ttu-id="6bdf2-192">hello kimeneti meg kell felelnie a következő képernyőkép hello.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-192">hello output should match hello following screenshot.</span></span>

![Azure Cosmos DB C++ alkalmazás kimenete](media/documentdb-cpp-get-started/console.png)

<span data-ttu-id="6bdf2-194">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="6bdf2-194">Congratulations!</span></span> <span data-ttu-id="6bdf2-195">Hello C++ oktatóprogram elvégzése után, és rendelkezik az első Azure Cosmos DB konzolalkalmazást!</span><span class="sxs-lookup"><span data-stu-id="6bdf2-195">You've completed hello C++ tutorial and have your first Azure Cosmos DB console application!</span></span>

## <span data-ttu-id="6bdf2-196"><a id="GetSolution"></a>Hello teljes C++ oktatóanyag megoldás beszerzése</span><span class="sxs-lookup"><span data-stu-id="6bdf2-196"><a id="GetSolution"></a>Get hello complete C++ tutorial solution</span></span>
<span data-ttu-id="6bdf2-197">toobuild hello GetStarted-megoldás, amely tartalmazza az összes hello minta ebben a cikkben, következőkre lesz szüksége hello:</span><span class="sxs-lookup"><span data-stu-id="6bdf2-197">toobuild hello GetStarted solution that contains all hello samples in this article, you need hello following:</span></span>

* <span data-ttu-id="6bdf2-198">[Azure Cosmos DB-fiók][create-account].</span><span class="sxs-lookup"><span data-stu-id="6bdf2-198">[Azure Cosmos DB account][create-account].</span></span>
* <span data-ttu-id="6bdf2-199">Hello [GetStarted](https://github.com/stalker314314/DocumentDBCpp) megoldás elérhető a Githubon.</span><span class="sxs-lookup"><span data-stu-id="6bdf2-199">hello [GetStarted](https://github.com/stalker314314/DocumentDBCpp) solution available on GitHub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6bdf2-200">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6bdf2-200">Next steps</span></span>
* <span data-ttu-id="6bdf2-201">Ismerje meg, hogyan túl[figyelése Azure Cosmos DB fiók](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="6bdf2-201">Learn how too[monitor an Azure Cosmos DB account](monitor-accounts.md).</span></span>
* <span data-ttu-id="6bdf2-202">A minta-adatkészleteken hello lekérdezéseinek futtatásához [Tesztlekérdezéseket](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="6bdf2-202">Run queries against our sample dataset in hello [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="6bdf2-203">További tudnivalók a programozási modellt hello hello hello Develop szakasza [Azure Cosmos DB dokumentációs oldal](https://azure.microsoft.com/documentation/services/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="6bdf2-203">Learn more about hello programming model in hello Develop section of hello [Azure Cosmos DB documentation page](https://azure.microsoft.com/documentation/services/documentdb/).</span></span>

[create-account]: create-documentdb-dotnet.md#create-account


