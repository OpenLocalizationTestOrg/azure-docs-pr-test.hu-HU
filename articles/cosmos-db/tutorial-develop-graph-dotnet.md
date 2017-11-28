---
title: "Az Azure Cosmos DB: A Graph API a .NET fejlesztés |} Microsoft Docs"
description: "Ismerje meg, hogyan fejleszthet Azure Cosmos DB DocumentDB API-t a .NET használatával"
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
ms.openlocfilehash: eeaa0c4f84a408815371742334d2ba7ce600b72f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-cosmos-db-develop-with-the-graph-api-in-net"></a><span data-ttu-id="8cf1d-103">Az Azure Cosmos DB: A Graph API a .NET fejlesztés</span><span class="sxs-lookup"><span data-stu-id="8cf1d-103">Azure Cosmos DB: Develop with the Graph API in .NET</span></span>
<span data-ttu-id="8cf1d-104">Azure Cosmos-adatbázis egy Microsoft globálisan elosztott több modellre adatbázis szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-104">Azure Cosmos DB is Microsoft's globally distributed multi-model database service.</span></span> <span data-ttu-id="8cf1d-105">Segítségével gyorsan létrehozhat és lekérdezhet dokumentum-, kulcs/érték és gráf típusú adatbázisokat, melyek mindegyike felhasználja az Azure Cosmos DB középpontjában álló globális elosztási és horizontális skálázhatósági képességeket.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="8cf1d-106">Ez az oktatóanyag azt mutatja be, az Azure portál használatával egy Cosmos-DB Azure-fiók létrehozása és egy grafikonon adatbázis és a tároló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-106">This tutorial demonstrates how to create an Azure Cosmos DB account using the Azure portal and how to create a graph database and container.</span></span> <span data-ttu-id="8cf1d-107">Az alkalmazás ezután létrehoz egy egyszerű közösségi hálózati használatával négy személyekkel a [Graph API](graph-sdk-dotnet.md) (előzetes verzió), akkor lép át, és lekérdezi a graph használatával Gremlin.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-107">The application then creates a simple social network with four people using the [Graph API](graph-sdk-dotnet.md) (preview), then traverses and queries the graph using Gremlin.</span></span>

<span data-ttu-id="8cf1d-108">Ez az oktatóanyag ismerteti a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="8cf1d-108">This tutorial covers the following tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8cf1d-109">Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="8cf1d-109">Create an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="8cf1d-110">Hozzon létre egy grafikonon adatbázis és a tároló</span><span class="sxs-lookup"><span data-stu-id="8cf1d-110">Create a graph database and container</span></span>
> * <span data-ttu-id="8cf1d-111">Szerializálható csúcsban és a .NET-objektumokhoz</span><span class="sxs-lookup"><span data-stu-id="8cf1d-111">Serialize vertices and edges to .NET objects</span></span>
> * <span data-ttu-id="8cf1d-112">Adja hozzá a csúcsban és élei számára</span><span class="sxs-lookup"><span data-stu-id="8cf1d-112">Add vertices and edges</span></span>
> * <span data-ttu-id="8cf1d-113">A graph használatával Gremlin lekérdezése</span><span class="sxs-lookup"><span data-stu-id="8cf1d-113">Query the graph using Gremlin</span></span>

## <a name="graphs-in-azure-cosmos-db"></a><span data-ttu-id="8cf1d-114">Az Azure Cosmos DB grafikonja</span><span class="sxs-lookup"><span data-stu-id="8cf1d-114">Graphs in Azure Cosmos DB</span></span>
<span data-ttu-id="8cf1d-115">Azure Cosmos DB használatával létrehozása, frissítése és diagramjait használatával lekérdezni a [Microsoft.Azure.Graphs](graph-sdk-dotnet.md) könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-115">You can use Azure Cosmos DB to create, update, and query graphs using the [Microsoft.Azure.Graphs](graph-sdk-dotnet.md) library.</span></span> <span data-ttu-id="8cf1d-116">Egy bővítmény módszert biztosít a Microsoft.Azure.Graph könyvtár `CreateGremlinQuery<T>` a a `DocumentClient` osztály Gremlin lekérdezések végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-116">The Microsoft.Azure.Graph library provides a single extension method `CreateGremlinQuery<T>` on top of the `DocumentClient` class to execute Gremlin queries.</span></span>

<span data-ttu-id="8cf1d-117">Gremlin egy funkcionális programozási nyelv, amely támogatja az írási műveletek (DML) és a lekérdezés- és átjárás műveletei.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-117">Gremlin is a functional programming language that supports write operations (DML) and query and traversal operations.</span></span> <span data-ttu-id="8cf1d-118">Ebben a cikkben a elindított Gremlin beolvasandó néhány példa azt foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-118">We cover a few examples in this article to get your started with Gremlin.</span></span> <span data-ttu-id="8cf1d-119">Lásd: [Gremlin lekérdezések](gremlin-support.md) Gremlin lehetőségeinek Azure Cosmos DB részletes útmutatást.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-119">See [Gremlin queries](gremlin-support.md) for a detailed walkthrough of Gremlin capabilities available in Azure Cosmos DB.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="8cf1d-120">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8cf1d-120">Prerequisites</span></span>
<span data-ttu-id="8cf1d-121">Győződjön meg róla, hogy rendelkezik az alábbiakkal:</span><span class="sxs-lookup"><span data-stu-id="8cf1d-121">Please make sure you have the following:</span></span>

* <span data-ttu-id="8cf1d-122">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-122">An active Azure account.</span></span> <span data-ttu-id="8cf1d-123">Ha még nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="8cf1d-123">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span> 
    * <span data-ttu-id="8cf1d-124">Vagy használhatja az [Azure DocumentDB Emulatort](local-emulator.md) ebben az oktatóanyagban.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-124">Alternatively, you can use the [Azure DocumentDB Emulator](local-emulator.md) for this tutorial.</span></span>
* <span data-ttu-id="8cf1d-125">[Visual Studio](http://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="8cf1d-125">[Visual Studio](http://www.visualstudio.com/).</span></span>

## <a name="create-database-account"></a><span data-ttu-id="8cf1d-126">Adatbázis-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="8cf1d-126">Create database account</span></span>

<span data-ttu-id="8cf1d-127">Először hozzon létre egy Azure Cosmos DB fiókot az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-127">Let's start by creating an Azure Cosmos DB account in the Azure portal.</span></span>  

> [!TIP]
> * <span data-ttu-id="8cf1d-128">Már van Azure Cosmos DB fiókja?</span><span class="sxs-lookup"><span data-stu-id="8cf1d-128">Already have an Azure Cosmos DB account?</span></span> <span data-ttu-id="8cf1d-129">Ha igen, ugorjon előre [a Visual Studio megoldás beállítása](#SetupVS)</span><span class="sxs-lookup"><span data-stu-id="8cf1d-129">If so, skip ahead to [Set up your Visual Studio solution](#SetupVS)</span></span>
> * <span data-ttu-id="8cf1d-130">Kellett az Azure DocumentDB-fiókot?</span><span class="sxs-lookup"><span data-stu-id="8cf1d-130">Did you have an Azure DocumentDB account?</span></span> <span data-ttu-id="8cf1d-131">Ha igen, a fiókját most már Azure Cosmos DB fiókkal, és ugorjon előre a [a Visual Studio megoldás beállítása](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="8cf1d-131">If so, your account is now an Azure Cosmos DB account and you can skip ahead to [Set up your Visual Studio solution](#SetupVS).</span></span>  
> * <span data-ttu-id="8cf1d-132">Ha az Azure Cosmos DB Emulator használ, adja kövesse a [Azure Cosmos DB emulátor](local-emulator.md) kell beállítania az emulátor, és ugorjon előre [a Visual Studio megoldás beállítása](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="8cf1d-132">If you are using the Azure Cosmos DB Emulator, please follow the steps at [Azure Cosmos DB Emulator](local-emulator.md) to setup the emulator and skip ahead to [Set up your Visual Studio Solution](#SetupVS).</span></span> 
>
> 

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <span data-ttu-id="8cf1d-133"><a id="SetupVS"></a>A Visual Studio-megoldás beállítása</span><span class="sxs-lookup"><span data-stu-id="8cf1d-133"><a id="SetupVS"></a>Set up your Visual Studio solution</span></span>
1. <span data-ttu-id="8cf1d-134">Nyissa meg a **Visual Studiót** a számítógépén.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-134">Open **Visual Studio** on your computer.</span></span>
2. <span data-ttu-id="8cf1d-135">A **Fájl** menüben válassza az **Új**, majd a **Projekt** elemet.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-135">On the **File** menu, select **New**, and then choose **Project**.</span></span>
3. <span data-ttu-id="8cf1d-136">Az a **új projekt** párbeszédablakban válassza **sablonok** / **Visual C#** / **Konzolalkalmazás (.NET-keretrendszer)**, nevezze el a projektet, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-136">In the **New Project** dialog, select **Templates** / **Visual C#** / **Console App (.NET Framework)**, name your project, and then click **OK**.</span></span>
4. <span data-ttu-id="8cf1d-137">A **Megoldáskezelőben** kattintson a jobb gombbal az új konzolalkalmazásra, amely a Visual Studio megoldás alatt található, majd kattintson a **NuGet-csomagok kezelése...** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-137">In the **Solution Explorer**, right click on your new console application, which is under your Visual Studio solution, and then click **Manage NuGet Packages...**</span></span>
5. <span data-ttu-id="8cf1d-138">Az a **NuGet** lapra, majd **Tallózás**, és írja be **Microsoft.Azure.Graphs** a keresőmezőbe, és ellenőrizze a **előzetes verzióját tartalmazzák**.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-138">In the **NuGet** tab, click **Browse**, and type **Microsoft.Azure.Graphs** in the search box, and check the **Include prerelease versions**.</span></span>
6. <span data-ttu-id="8cf1d-139">A találatok között található **Microsoft.Azure.Graphs** kattintson **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-139">Within the results, find **Microsoft.Azure.Graphs** and click **Install**.</span></span>
   
   <span data-ttu-id="8cf1d-140">Ha a megoldás módosításainak áttekintéséről szóló üzenetet kap, kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-140">If you get a message about reviewing changes to the solution, click **OK**.</span></span> <span data-ttu-id="8cf1d-141">Ha a licenc elfogadásáról szóló üzenetet kap, kattintson az **Elfogadom** gombra.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-141">If you get a message about license acceptance, click **I accept**.</span></span>
   
    <span data-ttu-id="8cf1d-142">A `Microsoft.Azure.Graphs` tár egyetlen bővítmény módszert biztosít a `CreateGremlinQuery<T>` Gremlin műveletek hajthatók végre.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-142">The `Microsoft.Azure.Graphs` library provides a single extension method `CreateGremlinQuery<T>` for executing Gremlin operations.</span></span> <span data-ttu-id="8cf1d-143">Gremlin egy funkcionális programozási nyelv, amely támogatja az írási műveletek (DML) és a lekérdezés- és átjárás műveletei.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-143">Gremlin is a functional programming language that supports write operations (DML) and query and traversal operations.</span></span> <span data-ttu-id="8cf1d-144">Ebben a cikkben a elindított Gremlin beolvasandó néhány példa azt foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-144">We cover a few examples in this article to get your started with Gremlin.</span></span> <span data-ttu-id="8cf1d-145">[Gremlin lekérdezések](gremlin-support.md) rendelkezik Gremlin képességek részletes útmutató az Azure Cosmos Adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-145">[Gremlin queries](gremlin-support.md) has a detailed walkthrough of Gremlin capabilities in Azure Cosmos DB.</span></span>

## <span data-ttu-id="8cf1d-146"><a id="add-references"></a>Az alkalmazás kapcsolódni</span><span class="sxs-lookup"><span data-stu-id="8cf1d-146"><a id="add-references"></a>Connect your app</span></span>

<span data-ttu-id="8cf1d-147">Adja hozzá ezt a két állandót és a *ügyfél* változó az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-147">Add these two constants and your *client* variable in your application.</span></span> 

```csharp
string endpoint = ConfigurationManager.AppSettings["Endpoint"]; 
string authKey = ConfigurationManager.AppSettings["AuthKey"]; 
``` 
<span data-ttu-id="8cf1d-148">A következő biztonsági head a [Azure-portálon](https://portal.azure.com) a végponti URL-cím és az elsődleges kulcs beolvasása.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-148">Next, head back to the [Azure portal](https://portal.azure.com) to retrieve your endpoint URL and primary key.</span></span> <span data-ttu-id="8cf1d-149">A végponti URL-cím és az elsődleges kulcs ahhoz szükséges, hogy az alkalmazás tudja, hova kell csatlakoznia, az Azure Cosmos DB pedig megbízzon az alkalmazás által létesített kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-149">The endpoint URL and primary key are necessary for your application to understand where to connect to, and for Azure Cosmos DB to trust your application's connection.</span></span> 

<span data-ttu-id="8cf1d-150">Az Azure portálon lépjen az Azure Cosmos DB fiókjába, kattintson **kulcsok**, és kattintson a **írható-olvasható kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-150">In the Azure portal, navigate to your Azure Cosmos DB account, click **Keys**, and then click **Read-write Keys**.</span></span> 

<span data-ttu-id="8cf1d-151">Az URI a portálról másolása és beillesztése azt `Endpoint` a fenti az endpoint tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-151">Copy the URI from the portal and paste it over `Endpoint` in the endpoint property above.</span></span> <span data-ttu-id="8cf1d-152">Ezután másolja az elsődleges kulcsot a portálról, és illessze be azt a `AuthKey` fenti tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-152">Then copy the PRIMARY KEY from the portal and paste it into the `AuthKey` property above.</span></span> 

<span data-ttu-id="8cf1d-153">! [Képernyőfelvétel a C#-alkalmazás létrehozása az oktatóanyagban használt Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-153">![Screen shot of the Azure portal used by the tutorial to create a C# application.</span></span> <span data-ttu-id="8cf1d-154">Egy Azure Cosmos DB fiók az Azure Cosmos DB navigációs KEYS gomb, és a kulcsok panelen lévő URI és PRIMARY KEY értékek megjelenítése] [kulcsok]</span><span class="sxs-lookup"><span data-stu-id="8cf1d-154">Shows an Azure Cosmos DB account the KEYS button highlighted on the Azure Cosmos DB navigation , and the URI and PRIMARY KEY values highlighted on the Keys blade][keys]</span></span> 
 
## <span data-ttu-id="8cf1d-155"><a id="instantiate"></a>A documentclient ügyfél segítségével hozható létre</span><span class="sxs-lookup"><span data-stu-id="8cf1d-155"><a id="instantiate"></a>Instantiate the DocumentClient</span></span> 
<span data-ttu-id="8cf1d-156">Ezután hozzon létre egy új példányt a **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-156">Next, create a new instance of the **DocumentClient**.</span></span>  

```csharp 
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey); 
``` 

## <span data-ttu-id="8cf1d-157"><a id="create-database"></a>Adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="8cf1d-157"><a id="create-database"></a>Create a database</span></span> 

<span data-ttu-id="8cf1d-158">Ezután hozzon létre egy Azure Cosmos DB [adatbázis](documentdb-resources.md#databases) használatával a [Documentclient](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) metódus vagy [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) metódusában a **DocumentClient** osztályával a [DocumentDB .NET SDK](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="8cf1d-158">Now, create an Azure Cosmos DB [database](documentdb-resources.md#databases) by using the [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) method or [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) method of the **DocumentClient** class from the [DocumentDB .NET SDK](documentdb-sdk-dotnet.md).</span></span>  

```csharp 
Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" }); 
``` 
 
## <a name="create-a-graph"></a><span data-ttu-id="8cf1d-159">Hozzon létre egy grafikonon</span><span class="sxs-lookup"><span data-stu-id="8cf1d-159">Create a graph</span></span> 

<span data-ttu-id="8cf1d-160">Ezután hozzon létre egy grafikonon tároló használatával, a használatával a [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) metódus vagy [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) metódusában a **DocumentClient** osztály.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-160">Next, create a graph container by using the using the [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) method or [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) method of the **DocumentClient** class.</span></span> <span data-ttu-id="8cf1d-161">A gyűjtemény egy grafikonon entitások tároló.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-161">A collection is a container of graph entities.</span></span> 

```csharp 
DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync( 
    UriFactory.CreateDatabaseUri("graphdb"), 
    new DocumentCollection { Id = "graphcollz" }, 
    new RequestOptions { OfferThroughput = 1000 }); 
``` 

## <span data-ttu-id="8cf1d-162"><a id="serializing"></a>Szerializálható csúcsban és a .NET-objektumokhoz</span><span class="sxs-lookup"><span data-stu-id="8cf1d-162"><a id="serializing"></a>Serialize vertices and edges to .NET objects</span></span>
<span data-ttu-id="8cf1d-163">Azure Cosmos-adatbázis használja a [GraphSON egybeírt](gremlin-support.md), amely megadja, hogy a csúcsban, szélek és tulajdonságait egy JSON-séma.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-163">Azure Cosmos DB uses the [GraphSON wire format](gremlin-support.md), which defines a JSON schema for vertices, edges, and properties.</span></span> <span data-ttu-id="8cf1d-164">Az Azure Cosmos DB .NET SDK magában foglalja a függőség beállításához JSON.NET, és ez lehetővé teszi, hogy a szerializálás/deszerializálás GraphSON .NET objektumot, amely azt használható a kódban.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-164">The Azure Cosmos DB .NET SDK includes JSON.NET as a dependency, and this allows us to serialize/deserialize GraphSON into .NET objects that we can work with in code.</span></span>

<span data-ttu-id="8cf1d-165">Tegyük fel most dolgozni egy egyszerű közösségi hálózati négy személyekkel.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-165">As an example, let's work with a simple social network with four people.</span></span> <span data-ttu-id="8cf1d-166">Úgy tekintünk létrehozása `Person` csúcsban, adja hozzá `Knows` köztük lévő viszonyt is, majd a lekérdezés és haladnak át a diagramot úgy, hogy keresse meg a "" Friend "friend" kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-166">We look at how to create `Person` vertices, add `Knows` relationships between them, then query and traverse the graph to find "friend of friend" relationships.</span></span> 

<span data-ttu-id="8cf1d-167">A `Microsoft.Azure.Graphs.Elements` névteret biztosít `Vertex`, `Edge`, `Property` és `VertexProperty` osztályok a .NET jól meghatározott objektumok GraphSON válaszokat deszerializálása során.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-167">The `Microsoft.Azure.Graphs.Elements` namespace provides `Vertex`, `Edge`, `Property` and `VertexProperty` classes for deserializing GraphSON responses to well-defined .NET objects.</span></span>

## <a name="run-gremlin-using-creategremlinquery"></a><span data-ttu-id="8cf1d-168">Gremlin CreateGremlinQuery használatával futtassa</span><span class="sxs-lookup"><span data-stu-id="8cf1d-168">Run Gremlin using CreateGremlinQuery</span></span>
<span data-ttu-id="8cf1d-169">Gremlin, például az SQL, olvasási, írási és lekérdezési műveletek támogatja.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-169">Gremlin, like SQL, supports read, write, and query operations.</span></span> <span data-ttu-id="8cf1d-170">Például az alábbi kódrészletben láthatja a csúcsban, szélek létrehozása, hajtsa végre az egyes mintalekérdezések hogyan `CreateGremlinQuery<T>`, és aszinkron módon iterációt ezekkel az eredményekkel használatával `ExecuteNextAsync` és "HasMoreResults.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-170">For example, the following snippet shows how to create vertices, edges, perform some sample queries using `CreateGremlinQuery<T>`, and asynchronously iterate through these results using `ExecuteNextAsync` and \`HasMoreResults.</span></span>

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

    // The CreateGremlinQuery method extensions allow you to execute Gremlin queries and iterate
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

## <a name="add-vertices-and-edges"></a><span data-ttu-id="8cf1d-171">Adja hozzá a csúcsban és élei számára</span><span class="sxs-lookup"><span data-stu-id="8cf1d-171">Add vertices and edges</span></span>

<span data-ttu-id="8cf1d-172">Vizsgáljuk meg a további részletek az előző szakaszban látható Gremlin utasításokat.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-172">Let's look at the Gremlin statements shown in the preceding section more detail.</span></span> <span data-ttu-id="8cf1d-173">Első azt néhány Gremlin tartozó használatával csúcsban `addV` metódust.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-173">First we some vertices using Gremlin's `addV` method.</span></span> <span data-ttu-id="8cf1d-174">Például az alábbi kódrészletben hoz létre a "Személy" típusú "Thomas Andersen" csúcspont utónevét, vezetéknevét és kora tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-174">For example, the following snippet creates a "Thomas Andersen" vertex of type "Person", with properties for first name, last name, and age.</span></span>

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

<span data-ttu-id="8cf1d-175">Ezután azt néhány szélek között ezek csúcsban Gremlin tartozó használatával hoz létre `addE` metódust.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-175">Then we create some edges between these vertices using Gremlin's `addE` method.</span></span> 

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

<span data-ttu-id="8cf1d-176">Egy meglévő csúcspont segítségével frissítheti azt `properties` Gremlin lépést.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-176">We can update an existing vertex by using `properties` step in Gremlin.</span></span> <span data-ttu-id="8cf1d-177">Azt a hívást végre a lekérdezés keresztül kihagyása `HasMoreResults` és `ExecuteNextAsync` a többi példák.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-177">We skip the call to execute the query via `HasMoreResults` and `ExecuteNextAsync` for the rest of the examples.</span></span>

```cs
// Update a vertex
client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.V('thomas').property('age', 45)");
```

<span data-ttu-id="8cf1d-178">Szegélyek és Gremlin tartozó használatával csúcsban elvetné `drop` lépés.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-178">You can drop edges and vertices using Gremlin's `drop` step.</span></span> <span data-ttu-id="8cf1d-179">Íme egy kódrészletet, amely bemutatja, hogyan törölhető egy csomópont és él.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-179">Here's a snippet that shows how to delete a vertex and an edge.</span></span> <span data-ttu-id="8cf1d-180">Vegye figyelembe, hogy társított széleinek kaszkádolt delete csúcspont eldobása végez.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-180">Note that dropping a vertex performs a cascading delete of the associated edges.</span></span>

```cs
// Drop an edge
client.CreateGremlinQuery(graphCollection, "g.E('thomasKnowsRobin').drop()");

// Drop a vertex
client.CreateGremlinQuery(graphCollection, "g.V('robin').drop()");
```

## <a name="query-the-graph"></a><span data-ttu-id="8cf1d-181">A lekérdezés a diagramhoz</span><span class="sxs-lookup"><span data-stu-id="8cf1d-181">Query the graph</span></span>

<span data-ttu-id="8cf1d-182">Lekérdezések és traversals Gremlin is használatával végezheti el.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-182">You can perform queries and traversals also using Gremlin.</span></span> <span data-ttu-id="8cf1d-183">Például az alábbi kódrészletben mutatja be a grafikonon csúcsban számát:</span><span class="sxs-lookup"><span data-stu-id="8cf1d-183">For example, the following snippet shows how to count the number of vertices in the graph:</span></span>

```cs
// Run a query to count vertices
IDocumentQuery<int> countQuery = client.CreateGremlinQuery<int>(graphCollection, "g.V().count()");
```
<span data-ttu-id="8cf1d-184">Szűrők Gremlin tartozó használatával végezheti el `has` és `hasLabel` lépéseit, és egyesíthet használatával `and`, `or`, és `not` összetettebb szűrők létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="8cf1d-184">You can perform filters using Gremlin's `has` and `hasLabel` steps, and combine them using `and`, `or`, and `not` to build more complex filters:</span></span>

```cs
// Run a query with filter
IDocumentQuery<Vertex> personsByAge = client.CreateGremlinQuery<Vertex>(
  graphCollection, 
  "g.V().hasLabel('person').has('age', gt(40))");
```

<span data-ttu-id="8cf1d-185">Kivetítheti az egyes tulajdonságok a lekérdezés eredményében használatával a `values` . lépés:</span><span class="sxs-lookup"><span data-stu-id="8cf1d-185">You can project certain properties in the query results using the `values` step:</span></span>

```cs
// Run a query with projection
IDocumentQuery<string> firstNames = client.CreateGremlinQuery<string>(
  graphCollection, 
  $"g.V().hasLabel('person').values('firstName')");
```

<span data-ttu-id="8cf1d-186">Eddig is csak láttuk lekérdezési operátorok, amelyek működnek a bármely adatbázis.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-186">So far, we've only seen query operators that work in any database.</span></span> <span data-ttu-id="8cf1d-187">Diagramok esetén gyors és hatékony átjárás műveleteihez kapcsolódó szélek és csúcsban keresse meg kell.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-187">Graphs are fast and efficient for traversal operations when you need to navigate to related edges and vertices.</span></span> <span data-ttu-id="8cf1d-188">Keressük Thomas összes barátok.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-188">Let's find all friends of Thomas.</span></span> <span data-ttu-id="8cf1d-189">Jelenleg ezt úgy teheti meg Gremlin `outE` keresheti meg az összes lépést a Thomas, majd a használatával Gremlin tartozó szegélyek a a-csúcsban bejárása a kimenő éleinek `inV` . lépés:</span><span class="sxs-lookup"><span data-stu-id="8cf1d-189">We do this by using Gremlin's `outE` step to find all the out-edges from Thomas, then traversing to the in-vertices from those edges using Gremlin's `inV` step:</span></span>

```cs
// Run a traversal (find friends of Thomas)
IDocumentQuery<Vertex> friendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person')");
```

<span data-ttu-id="8cf1d-190">A következő lekérdezés hajtja végre két ugrások található összes Thomas' "ismerősök olyan ismerőseinek", a függvény meghívásával `outE` és `inV` kétszer.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-190">The next query performs two hops to find all of Thomas' "friends of friends", by calling `outE` and `inV` two times.</span></span> 

```cs
// Run a traversal (find friends of friends of Thomas)
IDocumentQuery<Vertex> friendsOfFriendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')");
```

<span data-ttu-id="8cf1d-191">Összetettebb lekérdezések létrehozhatja és hatékony graph átjárás logika Gremlin, beleértve a keverése szűrőkifejezéseket, ismétlési használatával végez használatával valósítja meg a `loop` lépés, és végrehajtási feltételes navigációs használ a `choose` lépés.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-191">You can build more complex queries and implement powerful graph traversal logic using Gremlin, including mixing filter expressions, performing looping using the `loop` step, and implementing conditional navigation using the `choose` step.</span></span> <span data-ttu-id="8cf1d-192">További információ a teendők [Gremlin támogatási](gremlin-support.md)!</span><span class="sxs-lookup"><span data-stu-id="8cf1d-192">Learn more about what you can do with [Gremlin support](gremlin-support.md)!</span></span>

<span data-ttu-id="8cf1d-193">Ennyi az egész, az Azure Cosmos DB oktatóanyag befejeződött!</span><span class="sxs-lookup"><span data-stu-id="8cf1d-193">That's it, this Azure Cosmos DB tutorial is complete!</span></span> 

## <a name="clean-up-resources"></a><span data-ttu-id="8cf1d-194">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="8cf1d-194">Clean up resources</span></span>

<span data-ttu-id="8cf1d-195">Ha az alkalmazást már nem használja, a következő lépések használatával törölje az oktatóanyag során létrehozott összes erőforrást az Azure Portalon.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-195">If you're not going to continue to use this app, use the following steps to delete all resources created by this tutorial in the Azure portal.</span></span>  

1. <span data-ttu-id="8cf1d-196">Az Azure Portal bal oldali menüjében kattintson az **Erőforráscsoportok** lehetőségre, majd kattintson a létrehozott erőforrás nevére.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-196">From the left-hand menu in the Azure portal, click **Resource groups** and then click the name of the resource you created.</span></span> 
2. <span data-ttu-id="8cf1d-197">Az erőforráscsoport lapján kattintson a **Törlés** elemre, írja be a törölni kívánt erőforrás nevét a szövegmezőbe, majd kattintson a **Törlés** gombra.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-197">On your resource group page, click **Delete**, type the name of the resource to delete in the text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8cf1d-198">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8cf1d-198">Next Steps</span></span>

<span data-ttu-id="8cf1d-199">Ebben az oktatóanyagban ezt a következők:</span><span class="sxs-lookup"><span data-stu-id="8cf1d-199">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8cf1d-200">Egy Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="8cf1d-200">Created an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="8cf1d-201">Egy grafikonon adatbázis és a tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="8cf1d-201">Created a graph database and container</span></span>
> * <span data-ttu-id="8cf1d-202">A szerializált csúcsban és a .NET-objektumokhoz</span><span class="sxs-lookup"><span data-stu-id="8cf1d-202">Serialized vertices and edges to .NET objects</span></span>
> * <span data-ttu-id="8cf1d-203">A hozzáadott csúcsban és élei számára</span><span class="sxs-lookup"><span data-stu-id="8cf1d-203">Added vertices and edges</span></span>
> * <span data-ttu-id="8cf1d-204">A graph használatával Gremlin lekérdezése</span><span class="sxs-lookup"><span data-stu-id="8cf1d-204">Queried the graph using Gremlin</span></span>

<span data-ttu-id="8cf1d-205">Most már készen áll arra, hogy a Gremlin használatával összetettebb lekérdezéseket hozzon létre és hatékony gráfbejárási logikákat implementáljon.</span><span class="sxs-lookup"><span data-stu-id="8cf1d-205">You can now build more complex queries and implement powerful graph traversal logic using Gremlin.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="8cf1d-206">Lekérdezés a Gremlin használatával</span><span class="sxs-lookup"><span data-stu-id="8cf1d-206">Query using Gremlin</span></span>](tutorial-query-graph.md)
