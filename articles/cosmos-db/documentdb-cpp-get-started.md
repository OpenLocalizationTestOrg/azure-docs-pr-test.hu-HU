---
title: "C++ oktatóanyag az Azure Cosmos DB-hez | Microsoft Docs"
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
ms.openlocfilehash: 7d8de973765830ccd7983182bc1bb19b1e01e505
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-c-console-application-tutorial-for-the-documentdb-api"></a><span data-ttu-id="848a7-104">Azure Cosmos DB: C++ konzolalkalmazás oktatóanyaga DocumentDB API-hoz</span><span class="sxs-lookup"><span data-stu-id="848a7-104">Azure Cosmos DB: C++ console application tutorial for the DocumentDB API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="848a7-105">.NET</span><span class="sxs-lookup"><span data-stu-id="848a7-105">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="848a7-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="848a7-106">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="848a7-107">Node.js MongoDB-hez</span><span class="sxs-lookup"><span data-stu-id="848a7-107">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="848a7-108">Node.js</span><span class="sxs-lookup"><span data-stu-id="848a7-108">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="848a7-109">Java</span><span class="sxs-lookup"><span data-stu-id="848a7-109">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="848a7-110">C++</span><span class="sxs-lookup"><span data-stu-id="848a7-110">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 
 

<span data-ttu-id="848a7-111">Üdvözöljük az Azure Cosmos DB DocumentDB API által támogatott C++ SDK-hoz készült C++ oktatóanyagban!</span><span class="sxs-lookup"><span data-stu-id="848a7-111">Welcome to the C++ tutorial for the Azure Cosmos DB DocumentDB API endorsed SDK for C++!</span></span> <span data-ttu-id="848a7-112">Az oktatóanyag lépéseinek követésével egy olyan konzolalkalmazást hozhat létre, amely Azure Cosmos DB-erőforrásokat (például C++ adatbázisokat) hoz létre és kérdez le.</span><span class="sxs-lookup"><span data-stu-id="848a7-112">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources, including a C++ database.</span></span>

<span data-ttu-id="848a7-113">Az oktatóanyag a következőket ismerteti:</span><span class="sxs-lookup"><span data-stu-id="848a7-113">We'll cover:</span></span>

* <span data-ttu-id="848a7-114">Azure Cosmos DB-fiók létrehozása és csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="848a7-114">Creating and connecting to an Azure Cosmos DB account</span></span>
* <span data-ttu-id="848a7-115">Az alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="848a7-115">Setting up your application</span></span>
* <span data-ttu-id="848a7-116">C++ Azure Cosmos DB-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="848a7-116">Creating a C++ Azure Cosmos DB database</span></span>
* <span data-ttu-id="848a7-117">Gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="848a7-117">Creating a collection</span></span>
* <span data-ttu-id="848a7-118">JSON-dokumentumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="848a7-118">Creating JSON documents</span></span>
* <span data-ttu-id="848a7-119">A gyűjtemény lekérdezése</span><span class="sxs-lookup"><span data-stu-id="848a7-119">Querying the collection</span></span>
* <span data-ttu-id="848a7-120">Dokumentum cseréje</span><span class="sxs-lookup"><span data-stu-id="848a7-120">Replacing a document</span></span>
* <span data-ttu-id="848a7-121">Dokumentum törlése</span><span class="sxs-lookup"><span data-stu-id="848a7-121">Deleting a document</span></span>
* <span data-ttu-id="848a7-122">C++ Azure Cosmos DB-adatbázis törlése</span><span class="sxs-lookup"><span data-stu-id="848a7-122">Deleting the C++ Azure Cosmos DB database</span></span>

<span data-ttu-id="848a7-123">Nincs elég ideje?</span><span class="sxs-lookup"><span data-stu-id="848a7-123">Don't have time?</span></span> <span data-ttu-id="848a7-124">Ne aggódjon!</span><span class="sxs-lookup"><span data-stu-id="848a7-124">Don't worry!</span></span> <span data-ttu-id="848a7-125">A teljes megoldás elérhető a [GitHubon](https://github.com/stalker314314/DocumentDBCpp).</span><span class="sxs-lookup"><span data-stu-id="848a7-125">The complete solution is available on [GitHub](https://github.com/stalker314314/DocumentDBCpp).</span></span> <span data-ttu-id="848a7-126">Gyors útmutatásért tekintse meg [A teljes megoldás beszerzése](#GetSolution) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="848a7-126">See [Get the complete solution](#GetSolution) for quick instructions.</span></span>

<span data-ttu-id="848a7-127">A C++ oktatóanyag befejezése után a lap alján található szavazógombok használatával küldhet visszajelzést.</span><span class="sxs-lookup"><span data-stu-id="848a7-127">After you've completed the C++ tutorial, please use the voting buttons at the bottom of this page to give us feedback.</span></span> 

<span data-ttu-id="848a7-128">Ha szeretne közvetlenül kapcsolatba lépni velünk, a hozzászólásaiban tüntesse fel az e-mail-címét, vagy [lépjen kapcsolatba velünk itt](https://www.research.net/r/8BKRJ3Z).</span><span class="sxs-lookup"><span data-stu-id="848a7-128">If you'd like us to contact you directly, feel free to include your email address in your comments or [reach out to us here](https://www.research.net/r/8BKRJ3Z).</span></span> 

<span data-ttu-id="848a7-129">Most pedig lássunk neki!</span><span class="sxs-lookup"><span data-stu-id="848a7-129">Now let's get started!</span></span>

## <a name="prerequisites-for-the-c-tutorial"></a><span data-ttu-id="848a7-130">A C++ oktatóanyag előfeltételei</span><span class="sxs-lookup"><span data-stu-id="848a7-130">Prerequisites for the C++ tutorial</span></span>
<span data-ttu-id="848a7-131">Győződjön meg róla, hogy rendelkezik az alábbiakkal:</span><span class="sxs-lookup"><span data-stu-id="848a7-131">Please make sure you have the following:</span></span>

* <span data-ttu-id="848a7-132">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="848a7-132">An active Azure account.</span></span> <span data-ttu-id="848a7-133">Ha még nincs fiókja, regisztrálhat az [Azure ingyenes próbaverziójára](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="848a7-133">If you don't have one, you can sign up for a [Free Azure Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="848a7-134">[Visual Studio](https://www.visualstudio.com/downloads/), telepített C++ nyelvi összetevőkkel.</span><span class="sxs-lookup"><span data-stu-id="848a7-134">[Visual Studio](https://www.visualstudio.com/downloads/), with the C++ language components installed.</span></span>

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="848a7-135">1. lépés: Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="848a7-135">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="848a7-136">Hozzunk létre egy Azure Cosmos DB-fiókot.</span><span class="sxs-lookup"><span data-stu-id="848a7-136">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="848a7-137">Ha már rendelkezik egy használni kívánt fiókkal, folytassa [A C++ alkalmazás beállítása](#SetupNode) című lépéssel.</span><span class="sxs-lookup"><span data-stu-id="848a7-137">If you already have an account you want to use, you can skip ahead to [Setup your C++ application](#SetupNode).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="848a7-138"><a id="SetupC++"></a>2. lépés: A C++ alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="848a7-138"><a id="SetupC++"></a>Step 2: Set up your C++ application</span></span>
1. <span data-ttu-id="848a7-139">Nyissa meg a Visual Studiót, majd a **File** (Fájl) menüben kattintson a **New** (Új) elemre, majd kattintson a **Project** (Projekt) elemre.</span><span class="sxs-lookup"><span data-stu-id="848a7-139">Open Visual Studio, and then on the **File** menu, click **New**, and then click **Project**.</span></span> 
2. <span data-ttu-id="848a7-140">A **New Project** (Új projekt) ablakban, az **Installed** (Telepítve) panelen bontsa ki a **Visual C++** elemet, kattintson a **Win32** elemre, majd kattintson a **Win32 Console Application** (Win32 konzolalkalmazás) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="848a7-140">In the **New Project** window, in the **Installed** pane, expand **Visual C++**, click **Win32**, and then click **Win32 Console Application**.</span></span> <span data-ttu-id="848a7-141">Adja a hellodocumentdb nevet a projektnek, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="848a7-141">Name the project hellodocumentdb and then click **OK**.</span></span> 
   
    ![A (New project) (Új projekt) varázsló képernyőképe](media/documentdb-cpp-get-started/hello.png)
3. <span data-ttu-id="848a7-143">A Win32 alkalmazás varázslójának elindulásakor kattintson a **Finish** (Befejezés) gombra.</span><span class="sxs-lookup"><span data-stu-id="848a7-143">When the Win32 Application Wizard starts, click **Finish**.</span></span>
4. <span data-ttu-id="848a7-144">Ha létrejött a projekt, nyissa meg a NuGet-csomagkezelőt. Ehhez kattintson a jobb gombbal a **hellodocumentdb** projektre a **Solution Explorer** (Megoldáskezelő) felületén, és kattintson a **Manage NuGet Packages** (NuGet-csomagok kezelése) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="848a7-144">Once the project has been created, open the NuGet package manager by right-clicking the **hellodocumentdb** project in **Solution Explorer** and clicking **Manage NuGet Packages**.</span></span> 
   
    ![A projektmenüben a Manage NuGet Package (NuGet-csomagok kezelése) parancsot bemutató képernyőkép](media/documentdb-cpp-get-started/nuget.png)
5. <span data-ttu-id="848a7-146">A **NuGet: hellodocumentdb** lapon kattintson a **Browse** (Tallózás) gombra, majd keressen a *documentdbcpp* kifejezésre.</span><span class="sxs-lookup"><span data-stu-id="848a7-146">In the **NuGet: hellodocumentdb** tab, click **Browse**, and then search for *documentdbcpp*.</span></span> <span data-ttu-id="848a7-147">Az eredményekben válassza a DocumentDbCPP elemet az alábbi képernyőképen látható módon.</span><span class="sxs-lookup"><span data-stu-id="848a7-147">In the results, select DocumentDbCPP, as shown in the following screenshot.</span></span> <span data-ttu-id="848a7-148">Ez a csomag a C++ REST SDK hivatkozásait telepíti, amely a DocumentDbCPP függősége.</span><span class="sxs-lookup"><span data-stu-id="848a7-148">This package installs references to C++ REST SDK, which is a dependency for the DocumentDbCPP.</span></span>  
   
    ![A kiemelt DocumentDbCpp csomagot bemutató képernyőkép](media/documentdb-cpp-get-started/cpp.png)
   
    <span data-ttu-id="848a7-150">Miután a csomagokat a projekthez adta, készen állunk a kódírásra.</span><span class="sxs-lookup"><span data-stu-id="848a7-150">Once the packages have been added to your project, we are all set to start writing some code.</span></span>   

## <span data-ttu-id="848a7-151"><a id="Config"></a>3. lépés: Kapcsolat részleteinek másolása az Azure Portalról az Azure Cosmos DB-adatbázisba</span><span class="sxs-lookup"><span data-stu-id="848a7-151"><a id="Config"></a>Step 3: Copy connection details from Azure portal for your Azure Cosmos DB database</span></span>
<span data-ttu-id="848a7-152">Nyissa meg az [Azure Portalt](https://portal.azure.com), és lépjen a létrehozott Azure Cosmos DB-adatbázisfiókra.</span><span class="sxs-lookup"><span data-stu-id="848a7-152">Bring up [Azure portal](https://portal.azure.com) and traverse to the Azure Cosmos DB database account you created.</span></span> <span data-ttu-id="848a7-153">A következő lépésben szükségünk lesz az URI-re és az elsődleges kulcsra az Azure Portalról, hogy kapcsolatot hozzunk létre a C++ kódrészletből.</span><span class="sxs-lookup"><span data-stu-id="848a7-153">We will need the URI and the primary key from Azure portal in the next step to establish a connection from our C++ code snippet.</span></span> 

![Azure Cosmos DB URI és kulcsok az Azure Portalon](media/documentdb-cpp-get-started/nosql-tutorial-keys.png)

## <span data-ttu-id="848a7-155"><a id="Connect"></a>4. lépés: Csatlakozás egy Azure Cosmos DB-fiókhoz</span><span class="sxs-lookup"><span data-stu-id="848a7-155"><a id="Connect"></a>Step 4: Connect to an Azure Cosmos DB account</span></span>
1. <span data-ttu-id="848a7-156">Adja a következő fejléceket és névtereket a forráskódhoz, az `#include "stdafx.h"` elem után.</span><span class="sxs-lookup"><span data-stu-id="848a7-156">Add the following headers and namespaces to your source code, after `#include "stdafx.h"`.</span></span>
   
        #include <cpprest/json.h>
        #include <documentdbcpp\DocumentClient.h>
        #include <documentdbcpp\exceptions.h>
        #include <documentdbcpp\TriggerOperation.h>
        #include <documentdbcpp\TriggerType.h>
        using namespace documentdb;
        using namespace std;
        using namespace web::json;
2. <span data-ttu-id="848a7-157">Ezután adja a következő kódot a fő függvényhez, és cserélje le a fiók konfigurációját és az elsődleges kulcsot, hogy egyezzenek a 3. lépés Azure Cosmos DB beállításaival.</span><span class="sxs-lookup"><span data-stu-id="848a7-157">Next add the following code to your main function and replace the account configuration and primary key to match your Azure Cosmos DB settings from step 3.</span></span> 
   
        DocumentDBConfiguration conf (L"<account_configuration_uri>", L"<primary_key>");
        DocumentClient client (conf);
   
    <span data-ttu-id="848a7-158">Most, hogy rendelkezik a documentdb-ügyfél elindításához szükséges kóddal, vessünk egy pillantást az Azure Cosmos DB-erőforrások használatára.</span><span class="sxs-lookup"><span data-stu-id="848a7-158">Now that you have the code to initialize the documentdb client, let's take a look at working with Azure Cosmos DB resources.</span></span>

## <span data-ttu-id="848a7-159"><a id="CreateDBColl"></a>5. lépés: C++ adatbázis és gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="848a7-159"><a id="CreateDBColl"></a>Step 5: Create a C++ database and collection</span></span>
<span data-ttu-id="848a7-160">A lépés elvégzése előtt az Azure Cosmos DB-t nem ismerő felhasználók érdekében vegyük át az adatbázis, a gyűjtemény és a dokumentumok kapcsolatát.</span><span class="sxs-lookup"><span data-stu-id="848a7-160">Before we perform this step, let's go over how a database, collection and documents interact for those of you who are new to Azure Cosmos DB.</span></span> <span data-ttu-id="848a7-161">Az [adatbázisok](documentdb-resources.md#databases) a dokumentumtároló gyűjtemények között particionált logikai tárolói.</span><span class="sxs-lookup"><span data-stu-id="848a7-161">A [database](documentdb-resources.md#databases) is a logical container of document storage portioned across collections.</span></span> <span data-ttu-id="848a7-162">A [gyűjtemények](documentdb-resources.md#collections) JSON-dokumentumokat és a kapcsolódó JavaScript alkalmazáslogikát tartalmazó tárolók.</span><span class="sxs-lookup"><span data-stu-id="848a7-162">A [collection](documentdb-resources.md#collections) is a container of JSON documents and the associated JavaScript application logic.</span></span> <span data-ttu-id="848a7-163">Az Azure Cosmos DB hierarchikus erőforrásmodellről és fogalmakról további információt az [Azure Cosmos DB hierarchikus erőforrásmodell és fogalmak](documentdb-resources.md) című cikkben talál.</span><span class="sxs-lookup"><span data-stu-id="848a7-163">You can learn more about the Azure Cosmos DB hierarchical resource model and concepts in [Azure Cosmos DB hierarchical resource model and concepts](documentdb-resources.md).</span></span>

<span data-ttu-id="848a7-164">Egy adatbázis és egy megfelelő gyűjtemény létrehozása érdekében adja a következő kódot a fő függvény végére.</span><span class="sxs-lookup"><span data-stu-id="848a7-164">To create a database and a corresponding collection add the following code to the end of your main function.</span></span> <span data-ttu-id="848a7-165">Ez létrehozza a „FamilyRegistry” nevű adatbázist és a „FamilyCollection” nevű gyűjteményt az előző lépésben megadott ügyfél-konfigurációval.</span><span class="sxs-lookup"><span data-stu-id="848a7-165">This creates a database called 'FamilyRegistry’ and a collection called ‘FamilyCollection’ using the client configuration you declared in the previous step.</span></span>

    try {
      shared_ptr<Database> db = client.CreateDatabase(L"FamilyRegistry");
      shared_ptr<Collection> coll = db->CreateCollection(L"FamilyCollection");
    } catch (DocumentDBRuntimeException ex) {
      wcout << ex.message();
    }


## <span data-ttu-id="848a7-166"><a id="CreateDoc"></a>6. lépés: Dokumentum létrehozása</span><span class="sxs-lookup"><span data-stu-id="848a7-166"><a id="CreateDoc"></a>Step 6: Create a document</span></span>
<span data-ttu-id="848a7-167">A [dokumentumok](documentdb-resources.md#documents) a felhasználó által megadott (tetszőleges) JSON-tartalmak.</span><span class="sxs-lookup"><span data-stu-id="848a7-167">[Documents](documentdb-resources.md#documents) are user-defined (arbitrary) JSON content.</span></span> <span data-ttu-id="848a7-168">Most már beszúrhat egy dokumentumot az Azure Cosmos DB-be.</span><span class="sxs-lookup"><span data-stu-id="848a7-168">You can now insert a document into Azure Cosmos DB.</span></span> <span data-ttu-id="848a7-169">Egy dokumentum létrehozásához másolja a következő kódot a fő függvény végére.</span><span class="sxs-lookup"><span data-stu-id="848a7-169">You can create a document by copying the following code into the end of the main function.</span></span> 

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

<span data-ttu-id="848a7-170">Összefoglalva, ez a kód Azure Cosmos DB-adatbázist, -gyűjteményt és -dokumentumokat hoz létre, amelyeket a Dokumentumkezelőben kérhet le az Azure Portalon.</span><span class="sxs-lookup"><span data-stu-id="848a7-170">To summarize, this code creates an Azure Cosmos DB database, collection, and documents, which you can query in Document Explorer in Azure portal.</span></span> 

![C++ oktatóanyag – A fiók, az adatbázis, a gyűjtemény és a dokumentumok hierarchikus kapcsolatát ábrázoló diagram](media/documentdb-cpp-get-started/docs.png)

## <span data-ttu-id="848a7-172"><a id="QueryDB"></a>7. lépés: Az Azure Cosmos DB-erőforrások lekérdezése</span><span class="sxs-lookup"><span data-stu-id="848a7-172"><a id="QueryDB"></a>Step 7: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="848a7-173">Az Azure Cosmos DB támogatja az egyes gyűjteményekben tárolt JSON-dokumentumokon végzett [részletes lekérdezéseket](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="848a7-173">Azure Cosmos DB supports [rich queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span> <span data-ttu-id="848a7-174">Az alábbi mintakód egy SQL szintaxissal készített lekérdezést mutat be, amelyet az előző lépésben létrehozott dokumentumokra vonatkozóan futtathat le.</span><span class="sxs-lookup"><span data-stu-id="848a7-174">The following sample code shows a query made using SQL syntax that you can run against the documents we created in the previous step.</span></span>

<span data-ttu-id="848a7-175">A függvény az adatbázis és a gyűjtemény egyedi azonosítóját vagy erőforrás-azonosítóját veszi fel argumentumokként a dokumentum ügyfelével együtt.</span><span class="sxs-lookup"><span data-stu-id="848a7-175">The function takes in as arguments the unique identifier or resource id for the database and the collection along with the document client.</span></span> <span data-ttu-id="848a7-176">Adja ezt a kódot a fő függvény elé.</span><span class="sxs-lookup"><span data-stu-id="848a7-176">Add this code before main function.</span></span>

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

## <span data-ttu-id="848a7-177"><a id="Replace"></a>8. lépés: Dokumentum cseréje</span><span class="sxs-lookup"><span data-stu-id="848a7-177"><a id="Replace"></a>Step 8: Replace a document</span></span>
<span data-ttu-id="848a7-178">Az Azure Cosmos DB támogatja a JSON-dokumentumok cseréjét, ahogyan az a következő kódban is látható.</span><span class="sxs-lookup"><span data-stu-id="848a7-178">Azure Cosmos DB supports replacing JSON documents, as demonstrated in the following code.</span></span> <span data-ttu-id="848a7-179">Adja ezt a kódot az executesimplequery függvény után.</span><span class="sxs-lookup"><span data-stu-id="848a7-179">Add this code after the executesimplequery function.</span></span>

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

## <span data-ttu-id="848a7-180"><a id="Delete"></a>9. lépés: Dokumentum törlése</span><span class="sxs-lookup"><span data-stu-id="848a7-180"><a id="Delete"></a>Step 9: Delete a document</span></span>
<span data-ttu-id="848a7-181">Az Azure Cosmos DB támogatja a JSON-dokumentumok törlését. Ehhez másolja és illessze be a következő kódot a replacedocument függvény után.</span><span class="sxs-lookup"><span data-stu-id="848a7-181">Azure Cosmos DB supports deleting JSON documents, you can do so by copy and pasting the following code after the replacedocument function.</span></span> 

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

## <span data-ttu-id="848a7-182"><a id="DeleteDB"></a>10. lépés: Adatbázis törlése</span><span class="sxs-lookup"><span data-stu-id="848a7-182"><a id="DeleteDB"></a>Step 10: Delete a database</span></span>
<span data-ttu-id="848a7-183">A létrehozott adatbázis törlésével az adatbázis és az összes gyermekerőforrás (gyűjtemények, dokumentumok stb.) is törlődik.</span><span class="sxs-lookup"><span data-stu-id="848a7-183">Deleting the created database removes the database and all child resources (collections, documents, etc.).</span></span>

<span data-ttu-id="848a7-184">Másolja és illessze be a következő kódrészletet (cleanup (tisztítás) függvény) a deletedocument függvény után az adatbázis, valamint minden gyermekerőforrásának törléséhez.</span><span class="sxs-lookup"><span data-stu-id="848a7-184">Copy and paste the following code snippet (function cleanup) after the deletedocument function to remove the database and all the child resources.</span></span>

    void deletedb(const DocumentClient &client, const wstring dbresourceid) {
      try {
        client.DeleteDatabase(dbresourceid);
      } catch (DocumentDBRuntimeException ex) {
        wcout << ex.message();
      }
    }

## <span data-ttu-id="848a7-185"><a id="Run"></a>11. lépés: A teljes C++ alkalmazás futtatása!</span><span class="sxs-lookup"><span data-stu-id="848a7-185"><a id="Run"></a>Step 11: Run your C++ application all together!</span></span>
<span data-ttu-id="848a7-186">Ezzel hozzáadtuk a különböző Azure Cosmos DB-erőforrások létrehozására, lekérdezésére, módosítására és törlésére szolgáló kódot.</span><span class="sxs-lookup"><span data-stu-id="848a7-186">We have now added code to create, query, modify, and delete different Azure Cosmos DB resources.</span></span>  <span data-ttu-id="848a7-187">Mindezek összegzéséhez adjunk hívásokat a különböző függvényekhez a hellodocumentdb.cpp fájlban lévő fő függvényből néhány diagnosztikai üzenettel együtt.</span><span class="sxs-lookup"><span data-stu-id="848a7-187">Let us now wire this up by adding calls to these different functions from our main function in hellodocumentdb.cpp along with some diagnostic messages.</span></span>

<span data-ttu-id="848a7-188">Ehhez cserélje le az alkalmazás fő függvényét a következő kóddal.</span><span class="sxs-lookup"><span data-stu-id="848a7-188">You can do so by replacing the main function of your application with the following code.</span></span> <span data-ttu-id="848a7-189">Ez felülírja a 3. lépésben a kódba másolt account_configuration_uri és primary_key elemet, ezért mentse a sort, vagy másolja le az értékeket ismét a portálból.</span><span class="sxs-lookup"><span data-stu-id="848a7-189">This writes over the account_configuration_uri and primary_key you copied into the code in Step 3, so save that line or copy the values in again from the portal.</span></span> 

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

<span data-ttu-id="848a7-190">Most az F5 billentyűt lenyomva vagy a terminálablakban az alkalmazást megkeresve és a végrehajtható fájlt futtatva felépítheti és futtathatja a kódot a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="848a7-190">You should now be able to build and run your code in Visual Studio by pressing F5 or alternatively in the terminal window by locating the application and running the executable.</span></span> 

<span data-ttu-id="848a7-191">Meg kell jelennie az első lépések alkalmazás kimenetének.</span><span class="sxs-lookup"><span data-stu-id="848a7-191">You should see the output of your get started app.</span></span> <span data-ttu-id="848a7-192">A kimenetnek meg kell egyeznie az alábbi képernyőképpel.</span><span class="sxs-lookup"><span data-stu-id="848a7-192">The output should match the following screenshot.</span></span>

![Azure Cosmos DB C++ alkalmazás kimenete](media/documentdb-cpp-get-started/console.png)

<span data-ttu-id="848a7-194">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="848a7-194">Congratulations!</span></span> <span data-ttu-id="848a7-195">Ezennel befejezte a C++-oktatóanyagot, és létrehozta első saját Azure Cosmos DB-konzolalkalmazását!</span><span class="sxs-lookup"><span data-stu-id="848a7-195">You've completed the C++ tutorial and have your first Azure Cosmos DB console application!</span></span>

## <span data-ttu-id="848a7-196"><a id="GetSolution"></a>A C++ oktatóanyagban szereplő teljes megoldás beszerzése</span><span class="sxs-lookup"><span data-stu-id="848a7-196"><a id="GetSolution"></a>Get the complete C++ tutorial solution</span></span>
<span data-ttu-id="848a7-197">A cikkben szereplő összes példát tartalmazó GetStarted-megoldás lefordításához az alábbiakra lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="848a7-197">To build the GetStarted solution that contains all the samples in this article, you need the following:</span></span>

* <span data-ttu-id="848a7-198">[Azure Cosmos DB-fiók][create-account].</span><span class="sxs-lookup"><span data-stu-id="848a7-198">[Azure Cosmos DB account][create-account].</span></span>
* <span data-ttu-id="848a7-199">A GitHubon elérhető [GetStarted](https://github.com/stalker314314/DocumentDBCpp) megoldás.</span><span class="sxs-lookup"><span data-stu-id="848a7-199">The [GetStarted](https://github.com/stalker314314/DocumentDBCpp) solution available on GitHub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="848a7-200">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="848a7-200">Next steps</span></span>
* <span data-ttu-id="848a7-201">Ismerje meg, hogyan [figyelhet egy Azure Cosmos DB-fiókot](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="848a7-201">Learn how to [monitor an Azure Cosmos DB account](monitor-accounts.md).</span></span>
* <span data-ttu-id="848a7-202">Futtasson lekérdezéseket a minta-adatkészleteken a [Query Playground](https://www.documentdb.com/sql/demo) (Tesztlekérdezések) használatával.</span><span class="sxs-lookup"><span data-stu-id="848a7-202">Run queries against our sample dataset in the [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="848a7-203">A programozási modellel kapcsolatos további tudnivalókat az [Azure Cosmos DB-dokumentációs oldalának](https://azure.microsoft.com/documentation/services/documentdb/) Develop (Fejlesztés) szakaszában találja.</span><span class="sxs-lookup"><span data-stu-id="848a7-203">Learn more about the programming model in the Develop section of the [Azure Cosmos DB documentation page](https://azure.microsoft.com/documentation/services/documentdb/).</span></span>

[create-account]: create-documentdb-dotnet.md#create-account


