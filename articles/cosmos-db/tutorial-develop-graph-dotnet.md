---
title: "Az Azure Cosmos DB: Hello Graph API a .NET fejlesztést |} Microsoft Docs"
description: "Megtudhatja, hogyan toodevelop Azure Cosmos DB DocumentDB API-t a .NET használatával"
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: cc8df0be-672b-493e-95a4-26dd52632261
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/10/2017
ms.author: denlee
ms.custom: mvc
ms.openlocfilehash: 12e435d8169aeee6e818dac4a3b66c7a0ec5f2d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-develop-with-hello-graph-api-in-net"></a><span data-ttu-id="b6776-103">Azure Cosmos DB: A Graph API a .NET hello fejlesztést</span><span class="sxs-lookup"><span data-stu-id="b6776-103">Azure Cosmos DB: Develop with hello Graph API in .NET</span></span>
<span data-ttu-id="b6776-104">Azure Cosmos-adatbázis egy Microsoft globálisan elosztott több modellre adatbázis szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b6776-104">Azure Cosmos DB is Microsoft's globally distributed multi-model database service.</span></span> <span data-ttu-id="b6776-105">Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="b6776-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="b6776-106">Ez az oktatóanyag bemutatja, hogyan egy Azure Cosmos DB fiók használatával toocreate hello Azure-portálon, és hogyan toocreate egy grafikonon adatbázis és a tároló.</span><span class="sxs-lookup"><span data-stu-id="b6776-106">This tutorial demonstrates how toocreate an Azure Cosmos DB account using hello Azure portal and how toocreate a graph database and container.</span></span> <span data-ttu-id="b6776-107">hello alkalmazás ezután létrehoz egy egyszerű közösségi hálózati hello segítségével négy személyekkel [Graph API](graph-sdk-dotnet.md) (előzetes verzió), majd halad át, és lekérdezi a hello graph Gremlin használatával.</span><span class="sxs-lookup"><span data-stu-id="b6776-107">hello application then creates a simple social network with four people using hello [Graph API](graph-sdk-dotnet.md) (preview), then traverses and queries hello graph using Gremlin.</span></span>

<span data-ttu-id="b6776-108">Ez az oktatóanyag ismerteti a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="b6776-108">This tutorial covers hello following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b6776-109">Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6776-109">Create an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="b6776-110">Hozzon létre egy grafikonon adatbázis és a tároló</span><span class="sxs-lookup"><span data-stu-id="b6776-110">Create a graph database and container</span></span>
> * <span data-ttu-id="b6776-111">És a csúcsban too.NET objektumokat szerializálni</span><span class="sxs-lookup"><span data-stu-id="b6776-111">Serialize vertices and edges too.NET objects</span></span>
> * <span data-ttu-id="b6776-112">Adja hozzá a csúcsban és élei számára</span><span class="sxs-lookup"><span data-stu-id="b6776-112">Add vertices and edges</span></span>
> * <span data-ttu-id="b6776-113">Lekérdezés hello graph Gremlin használatával</span><span class="sxs-lookup"><span data-stu-id="b6776-113">Query hello graph using Gremlin</span></span>

## <a name="graphs-in-azure-cosmos-db"></a><span data-ttu-id="b6776-114">Az Azure Cosmos DB grafikonja</span><span class="sxs-lookup"><span data-stu-id="b6776-114">Graphs in Azure Cosmos DB</span></span>
<span data-ttu-id="b6776-115">Használhat Azure Cosmos DB toocreate, a frissítés és a lekérdezés diagramokat hello segítségével [Microsoft.Azure.Graphs](graph-sdk-dotnet.md) könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="b6776-115">You can use Azure Cosmos DB toocreate, update, and query graphs using hello [Microsoft.Azure.Graphs](graph-sdk-dotnet.md) library.</span></span> <span data-ttu-id="b6776-116">hello Microsoft.Azure.Graph tár egyetlen bővítmény módszert biztosít a `CreateGremlinQuery<T>` fölött hello `DocumentClient` tooexecute Gremlin lekérdezések osztályban.</span><span class="sxs-lookup"><span data-stu-id="b6776-116">hello Microsoft.Azure.Graph library provides a single extension method `CreateGremlinQuery<T>` on top of hello `DocumentClient` class tooexecute Gremlin queries.</span></span>

<span data-ttu-id="b6776-117">Gremlin egy funkcionális programozási nyelv, amely támogatja az írási műveletek (DML) és a lekérdezés- és átjárás műveletei.</span><span class="sxs-lookup"><span data-stu-id="b6776-117">Gremlin is a functional programming language that supports write operations (DML) and query and traversal operations.</span></span> <span data-ttu-id="b6776-118">A lépések Gremlin Ez a cikk tooget található néhány példa azt foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="b6776-118">We cover a few examples in this article tooget your started with Gremlin.</span></span> <span data-ttu-id="b6776-119">Lásd: [Gremlin lekérdezések](gremlin-support.md) Gremlin lehetőségeinek Azure Cosmos DB részletes útmutatást.</span><span class="sxs-lookup"><span data-stu-id="b6776-119">See [Gremlin queries](gremlin-support.md) for a detailed walkthrough of Gremlin capabilities available in Azure Cosmos DB.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="b6776-120">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b6776-120">Prerequisites</span></span>
<span data-ttu-id="b6776-121">Győződjön meg arról, hogy a következő hello:</span><span class="sxs-lookup"><span data-stu-id="b6776-121">Please make sure you have hello following:</span></span>

* <span data-ttu-id="b6776-122">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="b6776-122">An active Azure account.</span></span> <span data-ttu-id="b6776-123">Ha még nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="b6776-123">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span> 
    * <span data-ttu-id="b6776-124">Másik lehetőségként használhatja a hello [Azure DocumentDB emulátor](local-emulator.md) ehhez az oktatóanyaghoz.</span><span class="sxs-lookup"><span data-stu-id="b6776-124">Alternatively, you can use hello [Azure DocumentDB Emulator](local-emulator.md) for this tutorial.</span></span>
* <span data-ttu-id="b6776-125">[Visual Studio](http://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="b6776-125">[Visual Studio](http://www.visualstudio.com/).</span></span>

## <a name="create-database-account"></a><span data-ttu-id="b6776-126">Adatbázis-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6776-126">Create database account</span></span>

<span data-ttu-id="b6776-127">Először hozzon létre egy Azure Cosmos DB fiókot a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="b6776-127">Let's start by creating an Azure Cosmos DB account in hello Azure portal.</span></span>  

> [!TIP]
> * <span data-ttu-id="b6776-128">Már van Azure Cosmos DB fiókja?</span><span class="sxs-lookup"><span data-stu-id="b6776-128">Already have an Azure Cosmos DB account?</span></span> <span data-ttu-id="b6776-129">Ha igen, hagyja ki azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS)</span><span class="sxs-lookup"><span data-stu-id="b6776-129">If so, skip ahead too[Set up your Visual Studio solution](#SetupVS)</span></span>
> * <span data-ttu-id="b6776-130">Kellett az Azure DocumentDB-fiókot?</span><span class="sxs-lookup"><span data-stu-id="b6776-130">Did you have an Azure DocumentDB account?</span></span> <span data-ttu-id="b6776-131">Ha igen, a fiókját most már Azure Cosmos DB fiókkal, és kihagyhatja azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="b6776-131">If so, your account is now an Azure Cosmos DB account and you can skip ahead too[Set up your Visual Studio solution](#SetupVS).</span></span>  
> * <span data-ttu-id="b6776-132">Ha hello Azure Cosmos DB Emulator használata esetén kövesse hello készítésével [Azure Cosmos DB emulátor](local-emulator.md) toosetup hello emulátor, és hagyja ki azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="b6776-132">If you are using hello Azure Cosmos DB Emulator, please follow hello steps at [Azure Cosmos DB Emulator](local-emulator.md) toosetup hello emulator and skip ahead too[Set up your Visual Studio Solution](#SetupVS).</span></span> 
>
> 

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <span data-ttu-id="b6776-133"><a id="SetupVS"></a>A Visual Studio-megoldás beállítása</span><span class="sxs-lookup"><span data-stu-id="b6776-133"><a id="SetupVS"></a>Set up your Visual Studio solution</span></span>
1. <span data-ttu-id="b6776-134">Nyissa meg a **Visual Studiót** a számítógépén.</span><span class="sxs-lookup"><span data-stu-id="b6776-134">Open **Visual Studio** on your computer.</span></span>
2. <span data-ttu-id="b6776-135">A hello **fájl** menüjében válassza **új**, és válassza a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="b6776-135">On hello **File** menu, select **New**, and then choose **Project**.</span></span>
3. <span data-ttu-id="b6776-136">A hello **új projekt** párbeszédablakban válassza **sablonok** / **Visual C#** / **Konzolalkalmazás (.NET-keretrendszer)**, nevezze el a projektet, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="b6776-136">In hello **New Project** dialog, select **Templates** / **Visual C#** / **Console App (.NET Framework)**, name your project, and then click **OK**.</span></span>
4. <span data-ttu-id="b6776-137">A hello **Megoldáskezelőben**, kattintson az új konzolalkalmazásra, amely a Visual Studio megoldás alatt, a jobb gombbal, és kattintson a **NuGet-csomagok kezelése...**</span><span class="sxs-lookup"><span data-stu-id="b6776-137">In hello **Solution Explorer**, right click on your new console application, which is under your Visual Studio solution, and then click **Manage NuGet Packages...**</span></span>
5. <span data-ttu-id="b6776-138">A hello **NuGet** lapra, majd **Tallózás**, és írja be **Microsoft.Azure.Graphs** hello keresési mezőbe, és ellenőrzés hello **előzetes verzióitartalmazza**.</span><span class="sxs-lookup"><span data-stu-id="b6776-138">In hello **NuGet** tab, click **Browse**, and type **Microsoft.Azure.Graphs** in hello search box, and check hello **Include prerelease versions**.</span></span>
6. <span data-ttu-id="b6776-139">Hello eredmények belül található **Microsoft.Azure.Graphs** kattintson **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="b6776-139">Within hello results, find **Microsoft.Azure.Graphs** and click **Install**.</span></span>
   
   <span data-ttu-id="b6776-140">Ha kapcsolatos módosításokat toohello megoldás áttekintése figyelmeztető hibaüzenet jelenik meg, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="b6776-140">If you get a message about reviewing changes toohello solution, click **OK**.</span></span> <span data-ttu-id="b6776-141">Ha a licenc elfogadásáról szóló üzenetet kap, kattintson az **Elfogadom** gombra.</span><span class="sxs-lookup"><span data-stu-id="b6776-141">If you get a message about license acceptance, click **I accept**.</span></span>
   
    <span data-ttu-id="b6776-142">Hello `Microsoft.Azure.Graphs` tár egyetlen bővítmény módszert biztosít a `CreateGremlinQuery<T>` Gremlin műveletek hajthatók végre.</span><span class="sxs-lookup"><span data-stu-id="b6776-142">hello `Microsoft.Azure.Graphs` library provides a single extension method `CreateGremlinQuery<T>` for executing Gremlin operations.</span></span> <span data-ttu-id="b6776-143">Gremlin egy funkcionális programozási nyelv, amely támogatja az írási műveletek (DML) és a lekérdezés- és átjárás műveletei.</span><span class="sxs-lookup"><span data-stu-id="b6776-143">Gremlin is a functional programming language that supports write operations (DML) and query and traversal operations.</span></span> <span data-ttu-id="b6776-144">A lépések Gremlin Ez a cikk tooget található néhány példa azt foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="b6776-144">We cover a few examples in this article tooget your started with Gremlin.</span></span> <span data-ttu-id="b6776-145">[Gremlin lekérdezések](gremlin-support.md) rendelkezik Gremlin képességek részletes útmutató az Azure Cosmos Adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="b6776-145">[Gremlin queries](gremlin-support.md) has a detailed walkthrough of Gremlin capabilities in Azure Cosmos DB.</span></span>

## <span data-ttu-id="b6776-146"><a id="add-references"></a>Az alkalmazás kapcsolódni</span><span class="sxs-lookup"><span data-stu-id="b6776-146"><a id="add-references"></a>Connect your app</span></span>

<span data-ttu-id="b6776-147">Adja hozzá ezt a két állandót és a *ügyfél* változó az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b6776-147">Add these two constants and your *client* variable in your application.</span></span> 

```csharp
string endpoint = ConfigurationManager.AppSettings["Endpoint"]; 
string authKey = ConfigurationManager.AppSettings["AuthKey"]; 
``` 
<span data-ttu-id="b6776-148">A következő head biztonsági toohello [Azure-portálon](https://portal.azure.com) tooretrieve a végponti URL-cím és az elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="b6776-148">Next, head back toohello [Azure portal](https://portal.azure.com) tooretrieve your endpoint URL and primary key.</span></span> <span data-ttu-id="b6776-149">hello végponti URL-cím és elsődleges kulcs szükség az alkalmazás toounderstand ahol tooconnect esetén és Azure Cosmos DB tootrust az alkalmazás által létesített kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="b6776-149">hello endpoint URL and primary key are necessary for your application toounderstand where tooconnect to, and for Azure Cosmos DB tootrust your application's connection.</span></span> 

<span data-ttu-id="b6776-150">Az Azure-portálon hello lépjen tooyour Azure Cosmos DB fiókot, kattintson **kulcsok**, és kattintson a **írható-olvasható kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="b6776-150">In hello Azure portal, navigate tooyour Azure Cosmos DB account, click **Keys**, and then click **Read-write Keys**.</span></span> 

<span data-ttu-id="b6776-151">Hello portálról hello URI másolása és beillesztése azt `Endpoint` a fenti hello endpoint tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b6776-151">Copy hello URI from hello portal and paste it over `Endpoint` in hello endpoint property above.</span></span> <span data-ttu-id="b6776-152">Ezután másolása az elsődleges kulcs hello hello portálról, és illessze be hello `AuthKey` fenti tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b6776-152">Then copy hello PRIMARY KEY from hello portal and paste it into hello `AuthKey` property above.</span></span> 

<span data-ttu-id="b6776-153">! [Képernyőfelvétel a hello Azure-portál által használt hello oktatóanyag toocreate C#-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b6776-153">![Screen shot of hello Azure portal used by hello tutorial toocreate a C# application.</span></span> <span data-ttu-id="b6776-154">Megjeleníti egy Azure Cosmos DB fiók hello a KEYS gomb kiemelve meg hello Azure Cosmos DB navigációs és hello URI és PRIMARY KEY értékek kiemelésével hello a kulcsok panelen] [kulcsok]</span><span class="sxs-lookup"><span data-stu-id="b6776-154">Shows an Azure Cosmos DB account hello KEYS button highlighted on hello Azure Cosmos DB navigation , and hello URI and PRIMARY KEY values highlighted on hello Keys blade][keys]</span></span> 
 
## <span data-ttu-id="b6776-155"><a id="instantiate"></a>Hello DocumentClient példányosítható</span><span class="sxs-lookup"><span data-stu-id="b6776-155"><a id="instantiate"></a>Instantiate hello DocumentClient</span></span> 
<span data-ttu-id="b6776-156">Ezután hozzon létre egy új példányát hello **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="b6776-156">Next, create a new instance of hello **DocumentClient**.</span></span>  

```csharp 
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey); 
``` 

## <span data-ttu-id="b6776-157"><a id="create-database"></a>Adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6776-157"><a id="create-database"></a>Create a database</span></span> 

<span data-ttu-id="b6776-158">Ezután hozzon létre egy Azure Cosmos DB [adatbázis](documentdb-resources.md#databases) hello segítségével [Documentclient](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) metódus vagy [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) hello metódusában  **DocumentClient** hello osztály [DocumentDB .NET SDK](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="b6776-158">Now, create an Azure Cosmos DB [database](documentdb-resources.md#databases) by using hello [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) method or [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) method of hello **DocumentClient** class from hello [DocumentDB .NET SDK](documentdb-sdk-dotnet.md).</span></span>  

```csharp 
Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" }); 
``` 
 
## <a name="create-a-graph"></a><span data-ttu-id="b6776-159">Hozzon létre egy grafikonon</span><span class="sxs-lookup"><span data-stu-id="b6776-159">Create a graph</span></span> 

<span data-ttu-id="b6776-160">Ezután hozzon létre egy grafikonon tároló hello segítségével hello segítségével [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) metódus vagy [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) hello metódusában **DocumentClient**  osztály.</span><span class="sxs-lookup"><span data-stu-id="b6776-160">Next, create a graph container by using hello using hello [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) method or [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="b6776-161">A gyűjtemény egy grafikonon entitások tároló.</span><span class="sxs-lookup"><span data-stu-id="b6776-161">A collection is a container of graph entities.</span></span> 

```csharp 
DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync( 
    UriFactory.CreateDatabaseUri("graphdb"), 
    new DocumentCollection { Id = "graphcollz" }, 
    new RequestOptions { OfferThroughput = 1000 }); 
``` 

## <span data-ttu-id="b6776-162"><a id="serializing"></a>És a csúcsban too.NET objektumokat szerializálni</span><span class="sxs-lookup"><span data-stu-id="b6776-162"><a id="serializing"></a>Serialize vertices and edges too.NET objects</span></span>
<span data-ttu-id="b6776-163">Azure Cosmos DB használ hello [GraphSON egybeírt](gremlin-support.md), amely megadja, hogy a csúcsban, szélek és tulajdonságait egy JSON-séma.</span><span class="sxs-lookup"><span data-stu-id="b6776-163">Azure Cosmos DB uses hello [GraphSON wire format](gremlin-support.md), which defines a JSON schema for vertices, edges, and properties.</span></span> <span data-ttu-id="b6776-164">hello Azure Cosmos DB .NET SDK magában foglalja a függőség beállításához JSON.NET, és ez lehetővé teszi tooserialize/deszerializálás GraphSON .NET objektumokba azt kód együtt is használható.</span><span class="sxs-lookup"><span data-stu-id="b6776-164">hello Azure Cosmos DB .NET SDK includes JSON.NET as a dependency, and this allows us tooserialize/deserialize GraphSON into .NET objects that we can work with in code.</span></span>

<span data-ttu-id="b6776-165">Tegyük fel most dolgozni egy egyszerű közösségi hálózati négy személyekkel.</span><span class="sxs-lookup"><span data-stu-id="b6776-165">As an example, let's work with a simple social network with four people.</span></span> <span data-ttu-id="b6776-166">Úgy tekintünk, hogyan toocreate `Person` csúcsban, adja hozzá `Knows` köztük lévő viszonyt is, majd lekérdezési és hello graph toofind "" Friend "friend" kapcsolatokat haladnak át.</span><span class="sxs-lookup"><span data-stu-id="b6776-166">We look at how toocreate `Person` vertices, add `Knows` relationships between them, then query and traverse hello graph toofind "friend of friend" relationships.</span></span> 

<span data-ttu-id="b6776-167">Hello `Microsoft.Azure.Graphs.Elements` névteret biztosít `Vertex`, `Edge`, `Property` és `VertexProperty` osztályok GraphSON válaszok toowell által definiált .NET objektum deszerializálása során.</span><span class="sxs-lookup"><span data-stu-id="b6776-167">hello `Microsoft.Azure.Graphs.Elements` namespace provides `Vertex`, `Edge`, `Property` and `VertexProperty` classes for deserializing GraphSON responses toowell-defined .NET objects.</span></span>

## <a name="run-gremlin-using-creategremlinquery"></a><span data-ttu-id="b6776-168">Gremlin CreateGremlinQuery használatával futtassa</span><span class="sxs-lookup"><span data-stu-id="b6776-168">Run Gremlin using CreateGremlinQuery</span></span>
<span data-ttu-id="b6776-169">Gremlin, például az SQL, olvasási, írási és lekérdezési műveletek támogatja.</span><span class="sxs-lookup"><span data-stu-id="b6776-169">Gremlin, like SQL, supports read, write, and query operations.</span></span> <span data-ttu-id="b6776-170">Például hello alábbi kódrészletben láthatja, hogyan hajtsa végre a toocreate csúcsban, szélén, az egyes mintalekérdezések `CreateGremlinQuery<T>`, és aszinkron módon iterációt ezekkel az eredményekkel használatával `ExecuteNextAsync` és "HasMoreResults.</span><span class="sxs-lookup"><span data-stu-id="b6776-170">For example, hello following snippet shows how toocreate vertices, edges, perform some sample queries using `CreateGremlinQuery<T>`, and asynchronously iterate through these results using `ExecuteNextAsync` and \`HasMoreResults.</span></span>

```cs
Dictionary<string, string> gremlinQueries = new Dictionary<string, string>
{
    { "Cleanup",        "g.V().drop()" },
    { "AddVertex 1",    "g.addV('person').property('id', 'thomas').property('firstName', 'Thomas').property('age', 44)" },
    { "AddVertex 2",    "g.addV('person').property('id', 'mary').property('firstName', 'Mary').property('lastName', 'Andersen').property('age', 39)" },
    { "AddVertex 3",    "g.addV('person').property('id', 'ben').property('firstName', 'Ben').property('lastName', 'Miller')" },
    { "AddVertex 4",    "g.addV('person').property('id', 'robin').property('firstName', 'Robin').property('lastName', 'Wakefield')" },
    { "AddEdge 1",      "g.V('thomas').addE('knows').to(g.V('mary'))" },
    { "AddEdge 2",      "g.V('thomas').addE('knows').to(g.V('ben'))" },
    { "AddEdge 3",      "g.V('ben').addE('knows').to(g.V('robin'))" },
    { "UpdateVertex",   "g.V('thomas').property('age', 44)" },
    { "CountVertices",  "g.V().count()" },
    { "Filter Range",   "g.V().hasLabel('person').has('age', gt(40))" },
    { "Project",        "g.V().hasLabel('person').values('firstName')" },
    { "Sort",           "g.V().hasLabel('person').order().by('firstName', decr)" },
    { "Traverse",       "g.V('thomas').outE('knows').inV().hasLabel('person')" },
    { "Traverse 2x",    "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')" },
    { "Loop",           "g.V('thomas').repeat(out()).until(has('id', 'robin')).path()" },
    { "DropEdge",       "g.V('thomas').outE('knows').where(inV().has('id', 'mary')).drop()" },
    { "CountEdges",     "g.E().count()" },
    { "DropVertex",     "g.V('thomas').drop()" },
};

foreach (KeyValuePair<string, string> gremlinQuery in gremlinQueries)
{
    Console.WriteLine($"Running {gremlinQuery.Key}: {gremlinQuery.Value}");

    // hello CreateGremlinQuery method extensions allow you tooexecute Gremlin queries and iterate
    // results asychronously
    IDocumentQuery<dynamic> query = client.CreateGremlinQuery<dynamic>(graph, gremlinQuery.Value);
    while (query.HasMoreResults)
    {
        foreach (dynamic result in await query.ExecuteNextAsync())
        {
            Console.WriteLine($"\t {JsonConvert.SerializeObject(result)}");
        }
    }

    Console.WriteLine();
}
```

## <a name="add-vertices-and-edges"></a><span data-ttu-id="b6776-171">Adja hozzá a csúcsban és élei számára</span><span class="sxs-lookup"><span data-stu-id="b6776-171">Add vertices and edges</span></span>

<span data-ttu-id="b6776-172">Nézzük hello Gremlin utasítások megjelenő szakasz megelőző hello további információkhoz juthat.</span><span class="sxs-lookup"><span data-stu-id="b6776-172">Let's look at hello Gremlin statements shown in hello preceding section more detail.</span></span> <span data-ttu-id="b6776-173">Első azt néhány Gremlin tartozó használatával csúcsban `addV` metódust.</span><span class="sxs-lookup"><span data-stu-id="b6776-173">First we some vertices using Gremlin's `addV` method.</span></span> <span data-ttu-id="b6776-174">Például hello következő kódrészlettel hoz létre a "Személy" típusú "Thomas Andersen" csúcspont utónevét, vezetéknevét és kora tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="b6776-174">For example, hello following snippet creates a "Thomas Andersen" vertex of type "Person", with properties for first name, last name, and age.</span></span>

```cs
// Create a vertex
IDocumentQuery<Vertex> createVertexQuery = client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.addV('person').property('firstName', 'Thomas')");

while (createVertexQuery.HasMoreResults)
{
    Vertex thomas = (await create.ExecuteNextAsync<Vertex>()).First();
}
```

<span data-ttu-id="b6776-175">Ezután azt néhány szélek között ezek csúcsban Gremlin tartozó használatával hoz létre `addE` metódust.</span><span class="sxs-lookup"><span data-stu-id="b6776-175">Then we create some edges between these vertices using Gremlin's `addE` method.</span></span> 

```cs
// Add a "knows" edge
IDocumentQuery<Edge> createEdgeQuery = client.CreateGremlinQuery<Edge>(
    graphCollection, 
    "g.V('thomas').addE('knows').to(g.V('mary'))");

while (create.HasMoreResults)
{
    Edge thomasKnowsMaryEdge = (await create.ExecuteNextAsync<Edge>()).First();
}
```

<span data-ttu-id="b6776-176">Egy meglévő csúcspont segítségével frissítheti azt `properties` Gremlin lépést.</span><span class="sxs-lookup"><span data-stu-id="b6776-176">We can update an existing vertex by using `properties` step in Gremlin.</span></span> <span data-ttu-id="b6776-177">A Microsoft hello hívás tooexecute hello lekérdezés keresztül kihagyása `HasMoreResults` és `ExecuteNextAsync` hello többi hello példák.</span><span class="sxs-lookup"><span data-stu-id="b6776-177">We skip hello call tooexecute hello query via `HasMoreResults` and `ExecuteNextAsync` for hello rest of hello examples.</span></span>

```cs
// Update a vertex
client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.V('thomas').property('age', 45)");
```

<span data-ttu-id="b6776-178">Szegélyek és Gremlin tartozó használatával csúcsban elvetné `drop` lépés.</span><span class="sxs-lookup"><span data-stu-id="b6776-178">You can drop edges and vertices using Gremlin's `drop` step.</span></span> <span data-ttu-id="b6776-179">Íme egy kódrészletet, amely bemutatja, hogyan toodelete a csomópontra, és az edge.</span><span class="sxs-lookup"><span data-stu-id="b6776-179">Here's a snippet that shows how toodelete a vertex and an edge.</span></span> <span data-ttu-id="b6776-180">Vegye figyelembe, hogy az eldobása csúcspont végez a hello kaszkádolt delete társított szélén.</span><span class="sxs-lookup"><span data-stu-id="b6776-180">Note that dropping a vertex performs a cascading delete of hello associated edges.</span></span>

```cs
// Drop an edge
client.CreateGremlinQuery(graphCollection, "g.E('thomasKnowsRobin').drop()");

// Drop a vertex
client.CreateGremlinQuery(graphCollection, "g.V('robin').drop()");
```

## <a name="query-hello-graph"></a><span data-ttu-id="b6776-181">Lekérdezés hello diagramhoz</span><span class="sxs-lookup"><span data-stu-id="b6776-181">Query hello graph</span></span>

<span data-ttu-id="b6776-182">Lekérdezések és traversals Gremlin is használatával végezheti el.</span><span class="sxs-lookup"><span data-stu-id="b6776-182">You can perform queries and traversals also using Gremlin.</span></span> <span data-ttu-id="b6776-183">Például a következő kódrészletet hello bemutatja, hogyan toocount hello hello Graph csúcsban száma:</span><span class="sxs-lookup"><span data-stu-id="b6776-183">For example, hello following snippet shows how toocount hello number of vertices in hello graph:</span></span>

```cs
// Run a query toocount vertices
IDocumentQuery<int> countQuery = client.CreateGremlinQuery<int>(graphCollection, "g.V().count()");
```
<span data-ttu-id="b6776-184">Szűrők Gremlin tartozó használatával végezheti el `has` és `hasLabel` lépéseit, és egyesíthet használatával `and`, `or`, és `not` toobuild összetettebb szűrők:</span><span class="sxs-lookup"><span data-stu-id="b6776-184">You can perform filters using Gremlin's `has` and `hasLabel` steps, and combine them using `and`, `or`, and `not` toobuild more complex filters:</span></span>

```cs
// Run a query with filter
IDocumentQuery<Vertex> personsByAge = client.CreateGremlinQuery<Vertex>(
  graphCollection, 
  "g.V().hasLabel('person').has('age', gt(40))");
```

<span data-ttu-id="b6776-185">Kivetítheti az egyes tulajdonságok hello lekérdezés eredményében hello segítségével `values` . lépés:</span><span class="sxs-lookup"><span data-stu-id="b6776-185">You can project certain properties in hello query results using hello `values` step:</span></span>

```cs
// Run a query with projection
IDocumentQuery<string> firstNames = client.CreateGremlinQuery<string>(
  graphCollection, 
  $"g.V().hasLabel('person').values('firstName')");
```

<span data-ttu-id="b6776-186">Eddig is csak láttuk lekérdezési operátorok, amelyek működnek a bármely adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b6776-186">So far, we've only seen query operators that work in any database.</span></span> <span data-ttu-id="b6776-187">Diagramok esetén gyors és hatékony átjárás műveletekhez toonavigate toorelated szélek és csúcsban van szükség.</span><span class="sxs-lookup"><span data-stu-id="b6776-187">Graphs are fast and efficient for traversal operations when you need toonavigate toorelated edges and vertices.</span></span> <span data-ttu-id="b6776-188">Keressük Thomas összes barátok.</span><span class="sxs-lookup"><span data-stu-id="b6776-188">Let's find all friends of Thomas.</span></span> <span data-ttu-id="b6776-189">Jelenleg ezt úgy teheti meg Gremlin `outE` toofind összes hello kimenő éleinek Thomas a lépést, majd toohello a-csúcsban áthaladó Gremlin tartozó használatával szegélyek a `inV` . lépés:</span><span class="sxs-lookup"><span data-stu-id="b6776-189">We do this by using Gremlin's `outE` step toofind all hello out-edges from Thomas, then traversing toohello in-vertices from those edges using Gremlin's `inV` step:</span></span>

```cs
// Run a traversal (find friends of Thomas)
IDocumentQuery<Vertex> friendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person')");
```

<span data-ttu-id="b6776-190">hello a következő lekérdezés hajt végre két ugrások toofind összes Thomas' "ismerősök olyan ismerőseinek", meghívásával `outE` és `inV` kétszer.</span><span class="sxs-lookup"><span data-stu-id="b6776-190">hello next query performs two hops toofind all of Thomas' "friends of friends", by calling `outE` and `inV` two times.</span></span> 

```cs
// Run a traversal (find friends of friends of Thomas)
IDocumentQuery<Vertex> friendsOfFriendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')");
```

<span data-ttu-id="b6776-191">Összetettebb lekérdezések létrehozhatja és hatékony graph átjárás logika használatával Gremlin, többek között a következőket keverési szűrő kifejezések használatával ismétlési végrehajtása hello megvalósítása `loop` lépést, és végrehajtási feltételes navigációs hello segítségével `choose` lépés.</span><span class="sxs-lookup"><span data-stu-id="b6776-191">You can build more complex queries and implement powerful graph traversal logic using Gremlin, including mixing filter expressions, performing looping using hello `loop` step, and implementing conditional navigation using hello `choose` step.</span></span> <span data-ttu-id="b6776-192">További információ a teendők [Gremlin támogatási](gremlin-support.md)!</span><span class="sxs-lookup"><span data-stu-id="b6776-192">Learn more about what you can do with [Gremlin support](gremlin-support.md)!</span></span>

<span data-ttu-id="b6776-193">Ennyi az egész, az Azure Cosmos DB oktatóanyag befejeződött!</span><span class="sxs-lookup"><span data-stu-id="b6776-193">That's it, this Azure Cosmos DB tutorial is complete!</span></span> 

## <a name="clean-up-resources"></a><span data-ttu-id="b6776-194">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="b6776-194">Clean up resources</span></span>

<span data-ttu-id="b6776-195">Toocontinue toouse az alkalmazás nem fog, ha használja a következő lépéseket toodelete hello Azure-portálon az oktatóprogram során létrehozott összes erőforrás hello.</span><span class="sxs-lookup"><span data-stu-id="b6776-195">If you're not going toocontinue toouse this app, use hello following steps toodelete all resources created by this tutorial in hello Azure portal.</span></span>  

1. <span data-ttu-id="b6776-196">A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson a létrehozott hello erőforrás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="b6776-196">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span> 
2. <span data-ttu-id="b6776-197">Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="b6776-197">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b6776-198">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b6776-198">Next Steps</span></span>

<span data-ttu-id="b6776-199">Ebben az oktatóanyagban hello következő régebben már kötöttek:</span><span class="sxs-lookup"><span data-stu-id="b6776-199">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b6776-200">Egy Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6776-200">Created an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="b6776-201">Egy grafikonon adatbázis és a tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6776-201">Created a graph database and container</span></span>
> * <span data-ttu-id="b6776-202">A szerializált és a csúcsban too.NET objektumok</span><span class="sxs-lookup"><span data-stu-id="b6776-202">Serialized vertices and edges too.NET objects</span></span>
> * <span data-ttu-id="b6776-203">A hozzáadott csúcsban és élei számára</span><span class="sxs-lookup"><span data-stu-id="b6776-203">Added vertices and edges</span></span>
> * <span data-ttu-id="b6776-204">Lekérdezett hello graph Gremlin használatával</span><span class="sxs-lookup"><span data-stu-id="b6776-204">Queried hello graph using Gremlin</span></span>

<span data-ttu-id="b6776-205">Most már készen áll arra, hogy a Gremlin használatával összetettebb lekérdezéseket hozzon létre és hatékony gráfbejárási logikákat implementáljon.</span><span class="sxs-lookup"><span data-stu-id="b6776-205">You can now build more complex queries and implement powerful graph traversal logic using Gremlin.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="b6776-206">Lekérdezés a Gremlin használatával</span><span class="sxs-lookup"><span data-stu-id="b6776-206">Query using Gremlin</span></span>](tutorial-query-graph.md)
