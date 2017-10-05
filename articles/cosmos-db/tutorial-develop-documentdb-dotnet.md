---
title: "Azure Cosmos DB: A DocumentDB API a .NET fejlesztést |} Microsoft Docs"
description: "Ismerje meg, hogyan fejleszthet Azure Cosmos DB DocumentDB API-t a .NET használatával"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: 2eed74ae9bd173b0944ec190dfe5d9a4bdc54c37
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-cosmosdb-develop-with-the-documentdb-api-in-net"></a><span data-ttu-id="34fe1-103">Azure CosmosDB: A DocumentDB API a .NET fejlesztést</span><span class="sxs-lookup"><span data-stu-id="34fe1-103">Azure CosmosDB: Develop with the DocumentDB API in .NET</span></span>

<span data-ttu-id="34fe1-104">Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="34fe1-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="34fe1-105">Segítségével gyorsan létrehozhat és lekérdezhet dokumentum-, kulcs/érték és gráf típusú adatbázisokat, melyek mindegyike felhasználja az Azure Cosmos DB középpontjában álló globális elosztási és horizontális skálázhatósági képességeket.</span><span class="sxs-lookup"><span data-stu-id="34fe1-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="34fe1-106">Ez az oktatóanyag bemutatja, hogyan hozzon létre egy Azure Cosmos DB fiókot az Azure portál használatával, és majd a dokumentum-adatbázis és gyűjtemény létrehozása egy [partíciókulcs](documentdb-partition-data.md#partition-keys) használatával a [DocumentDB .NET API](documentdb-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="34fe1-106">This tutorial demonstrates how to create an Azure Cosmos DB account using the Azure portal, and then create a document database and collection with a [partition key](documentdb-partition-data.md#partition-keys) using the [DocumentDB .NET API](documentdb-introduction.md).</span></span> <span data-ttu-id="34fe1-107">Hozzon létre egy partíciókulcsot, ha létrehoz egy gyűjteményt, amelyet az alkalmazás kész az adatok növekedésével egyszerűen lehet méretezni.</span><span class="sxs-lookup"><span data-stu-id="34fe1-107">By defining a partition key when you create a collection, your application is prepared to scale effortlessly as your data grows.</span></span> 

<span data-ttu-id="34fe1-108">Ez az oktatóanyag ismerteti a következő feladatok segítségével a [DocumentDB .NET API](documentdb-sdk-dotnet.md):</span><span class="sxs-lookup"><span data-stu-id="34fe1-108">This tutorial covers the following tasks by using the [DocumentDB .NET API](documentdb-sdk-dotnet.md):</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="34fe1-109">Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="34fe1-109">Create an Azure Cosmos DB account</span></span>
> * <span data-ttu-id="34fe1-110">Egy adatbázis és gyűjtemény létrehozása egy partíciós kulccsal</span><span class="sxs-lookup"><span data-stu-id="34fe1-110">Create a database and collection with a partition key</span></span>
> * <span data-ttu-id="34fe1-111">JSON-dokumentumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="34fe1-111">Create JSON documents</span></span>
> * <span data-ttu-id="34fe1-112">Dokumentum frissítése</span><span class="sxs-lookup"><span data-stu-id="34fe1-112">Update a document</span></span>
> * <span data-ttu-id="34fe1-113">A particionált gyűjtemények lekérdezése</span><span class="sxs-lookup"><span data-stu-id="34fe1-113">Query partitioned collections</span></span>
> * <span data-ttu-id="34fe1-114">A tárolt eljárások futtatása</span><span class="sxs-lookup"><span data-stu-id="34fe1-114">Run stored procedures</span></span>
> * <span data-ttu-id="34fe1-115">Dokumentum törlése</span><span class="sxs-lookup"><span data-stu-id="34fe1-115">Delete a document</span></span>
> * <span data-ttu-id="34fe1-116">Adatbázis törlése</span><span class="sxs-lookup"><span data-stu-id="34fe1-116">Delete a database</span></span>

## <a name="prerequisites"></a><span data-ttu-id="34fe1-117">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="34fe1-117">Prerequisites</span></span>
<span data-ttu-id="34fe1-118">Győződjön meg róla, hogy rendelkezik az alábbiakkal:</span><span class="sxs-lookup"><span data-stu-id="34fe1-118">Please make sure you have the following:</span></span>

* <span data-ttu-id="34fe1-119">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="34fe1-119">An active Azure account.</span></span> <span data-ttu-id="34fe1-120">Ha még nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="34fe1-120">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span> 
    * <span data-ttu-id="34fe1-121">Másik lehetőségként használhatja a [Azure Cosmos DB emulátor](local-emulator.md) ehhez az oktatóanyaghoz, ha azt szeretné, egy helyi környezet, amely emulálja a fejlesztéshez az Azure DocumentDB szolgáltatás használata.</span><span class="sxs-lookup"><span data-stu-id="34fe1-121">Alternatively, you can use the [Azure Cosmos DB Emulator](local-emulator.md) for this tutorial if you'd like to use a local environment that emulates the Azure DocumentDB service for development purposes.</span></span>
* <span data-ttu-id="34fe1-122">[Visual Studio](http://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="34fe1-122">[Visual Studio](http://www.visualstudio.com/).</span></span>

## <a name="create-an-azure-cosmos-db-account"></a><span data-ttu-id="34fe1-123">Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="34fe1-123">Create an Azure Cosmos DB account</span></span>

<span data-ttu-id="34fe1-124">Először hozzon létre egy Azure Cosmos DB fiókot az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="34fe1-124">Let's start by creating an Azure Cosmos DB account in the Azure portal.</span></span>

> [!TIP]
> * <span data-ttu-id="34fe1-125">Már van Azure Cosmos DB fiókja?</span><span class="sxs-lookup"><span data-stu-id="34fe1-125">Already have an Azure Cosmos DB account?</span></span> <span data-ttu-id="34fe1-126">Ha igen, ugorjon előre [a Visual Studio megoldás beállítása](#SetupVS)</span><span class="sxs-lookup"><span data-stu-id="34fe1-126">If so, skip ahead to [Set up your Visual Studio solution](#SetupVS)</span></span>
> * <span data-ttu-id="34fe1-127">Kellett az Azure DocumentDB-fiókot?</span><span class="sxs-lookup"><span data-stu-id="34fe1-127">Did you have an Azure DocumentDB account?</span></span> <span data-ttu-id="34fe1-128">Ha igen, a fiókját most már Azure Cosmos DB fiókkal, és ugorjon előre a [a Visual Studio megoldás beállítása](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="34fe1-128">If so, your account is now an Azure Cosmos DB account and you can skip ahead to [Set up your Visual Studio solution](#SetupVS).</span></span>  
> * <span data-ttu-id="34fe1-129">Ha az Azure Cosmos DB Emulator használ, adja kövesse a [Azure Cosmos DB emulátor](local-emulator.md) kell beállítania az emulátor, és ugorjon előre [a Visual Studio megoldás beállítása](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="34fe1-129">If you are using the Azure Cosmos DB Emulator, please follow the steps at [Azure Cosmos DB Emulator](local-emulator.md) to setup the emulator and skip ahead to [Set up your Visual Studio Solution](#SetupVS).</span></span> 
>
>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="34fe1-130"><a id="SetupVS"></a>A Visual Studio-megoldás beállítása</span><span class="sxs-lookup"><span data-stu-id="34fe1-130"><a id="SetupVS"></a>Set up your Visual Studio solution</span></span>
1. <span data-ttu-id="34fe1-131">Nyissa meg a **Visual Studiót** a számítógépén.</span><span class="sxs-lookup"><span data-stu-id="34fe1-131">Open **Visual Studio** on your computer.</span></span>
2. <span data-ttu-id="34fe1-132">A **Fájl** menüben válassza az **Új**, majd a **Projekt** elemet.</span><span class="sxs-lookup"><span data-stu-id="34fe1-132">On the **File** menu, select **New**, and then choose **Project**.</span></span>
3. <span data-ttu-id="34fe1-133">Az a **új projekt** párbeszédablakban válassza **sablonok** / **Visual C#** / **Konzolalkalmazás (.NET-keretrendszer)**, nevezze el a projektet, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="34fe1-133">In the **New Project** dialog, select **Templates** / **Visual C#** / **Console App (.NET Framework)**, name your project, and then click **OK**.</span></span>
   <span data-ttu-id="34fe1-134">![Képernyőfelvétel az Új projekt ablakról](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-new-project-2.png)</span><span class="sxs-lookup"><span data-stu-id="34fe1-134">![Screen shot of the New Project window](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-new-project-2.png)</span></span>

4. <span data-ttu-id="34fe1-135">A **Megoldáskezelőben** kattintson a jobb gombbal az új konzolalkalmazásra, amely a Visual Studio megoldás alatt található, majd kattintson a **NuGet-csomagok kezelése...** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="34fe1-135">In the **Solution Explorer**, right click on your new console application, which is under your Visual Studio solution, and then click **Manage NuGet Packages...**</span></span>
    
    ![A Projekt jobb gombos kattintással elérhető menüjének képernyőfelvétele](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-manage-nuget-pacakges.png)
5. <span data-ttu-id="34fe1-137">Az a **NuGet** lapra, majd **Tallózás**, és írja be **documentdb** be a keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="34fe1-137">In the **NuGet** tab, click **Browse**, and type **documentdb** in the search box.</span></span>
<!---stopped here--->
6. <span data-ttu-id="34fe1-138">A találatok között keresse meg a **Microsoft.Azure.DocumentDB** elemet, majd kattintson a **Telepítés** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="34fe1-138">Within the results, find **Microsoft.Azure.DocumentDB** and click **Install**.</span></span>
   <span data-ttu-id="34fe1-139">A csomag-azonosító az Azure Cosmos DB ügyféloldali kódtár [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB).</span><span class="sxs-lookup"><span data-stu-id="34fe1-139">The package ID for the Azure Cosmos DB Client Library is [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB).</span></span>
   <span data-ttu-id="34fe1-140">![Képernyőfelvétel a NuGet menüről a Azure Cosmos DB ügyfél SDK-kereséshez](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-manage-nuget-pacakges-2.png)</span><span class="sxs-lookup"><span data-stu-id="34fe1-140">![Screen shot of the NuGet Menu for finding Azure Cosmos DB Client SDK](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-manage-nuget-pacakges-2.png)</span></span>

    <span data-ttu-id="34fe1-141">Ha a megoldás módosításainak áttekintéséről szóló üzenetet kap, kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="34fe1-141">If you get a message about reviewing changes to the solution, click **OK**.</span></span> <span data-ttu-id="34fe1-142">Ha a licenc elfogadásáról szóló üzenetet kap, kattintson az **Elfogadom** gombra.</span><span class="sxs-lookup"><span data-stu-id="34fe1-142">If you get a message about license acceptance, click **I accept**.</span></span>

## <span data-ttu-id="34fe1-143"><a id="Connect"></a>Vegye fel a hivatkozásokat a projekthez</span><span class="sxs-lookup"><span data-stu-id="34fe1-143"><a id="Connect"></a>Add references to your project</span></span>
<span data-ttu-id="34fe1-144">A fennmaradó lépéseit az oktatóanyag a DocumentDB API kódrészletek, létrehozása és frissítése a projekt Azure Cosmos DB erőforrásainak szükséges adja meg.</span><span class="sxs-lookup"><span data-stu-id="34fe1-144">The remaining steps in this tutorial provide the DocumentDB API code snippets required to create and update Azure Cosmos DB resources in your project.</span></span>

<span data-ttu-id="34fe1-145">Először adja hozzá ezeket a hivatkozásokat, hogy az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="34fe1-145">First, add these references to your application.</span></span>
<!---These aren't added by default when you install the pkg?--->

```csharp
using System.Net;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
```

## <span data-ttu-id="34fe1-146"><a id="add-references"></a>Az alkalmazás kapcsolódni</span><span class="sxs-lookup"><span data-stu-id="34fe1-146"><a id="add-references"></a>Connect your app</span></span>

<span data-ttu-id="34fe1-147">Ezután adja hozzá ezt a két állandót és a *ügyfél* változó az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="34fe1-147">Next, add these two constants and your *client* variable in your application.</span></span>

```csharp
private const string EndpointUrl = "<your endpoint URL>";
private const string PrimaryKey = "<your primary key>";
private DocumentClient client;
```

<span data-ttu-id="34fe1-148">Majd, head vissza a [Azure-portálon](https://portal.azure.com) a végponti URL-cím és az elsődleges kulcs beolvasása.</span><span class="sxs-lookup"><span data-stu-id="34fe1-148">Then, head back to the [Azure portal](https://portal.azure.com) to retrieve your endpoint URL and primary key.</span></span> <span data-ttu-id="34fe1-149">A végponti URL-cím és az elsődleges kulcs ahhoz szükséges, hogy az alkalmazás tudja, hova kell csatlakoznia, az Azure Cosmos DB pedig megbízzon az alkalmazás által létesített kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="34fe1-149">The endpoint URL and primary key are necessary for your application to understand where to connect to, and for Azure Cosmos DB to trust your application's connection.</span></span>

<span data-ttu-id="34fe1-150">Az Azure portálon lépjen az Azure Cosmos DB fiókjába, kattintson **kulcsok**, és kattintson a **írható-olvasható kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="34fe1-150">In the Azure portal, navigate to your Azure Cosmos DB account, click **Keys**, and then click **Read-write Keys**.</span></span>

<span data-ttu-id="34fe1-151">Az URI a portálról másolása és beillesztése azt `<your endpoint URL>` a program.cs fájlban található.</span><span class="sxs-lookup"><span data-stu-id="34fe1-151">Copy the URI from the portal and paste it over `<your endpoint URL>` in the program.cs file.</span></span> <span data-ttu-id="34fe1-152">Az elsődleges kulcs másolása a portálról, majd illessze be keresztül `<your primary key>`.</span><span class="sxs-lookup"><span data-stu-id="34fe1-152">Then copy the PRIMARY KEY from the portal and paste it over `<your primary key>`.</span></span> <span data-ttu-id="34fe1-153">Ügyeljen arra, hogy távolítsa el a `<` és `>` a értékeiből.</span><span class="sxs-lookup"><span data-stu-id="34fe1-153">Be sure to remove the `<` and `>` from your values.</span></span>

![Képernyőfelvétel a NoSQL-oktatóanyagban a C# Konzolalkalmazás létrehozásához használt Azure-portálon.](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-keys.png)

## <span data-ttu-id="34fe1-156"><a id="instantiate"></a>A documentclient ügyfél segítségével hozható létre</span><span class="sxs-lookup"><span data-stu-id="34fe1-156"><a id="instantiate"></a>Instantiate the DocumentClient</span></span>

<span data-ttu-id="34fe1-157">Most, hozzon létre egy új példányt a **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="34fe1-157">Now, create a new instance of the **DocumentClient**.</span></span>

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
```

## <span data-ttu-id="34fe1-158"><a id="create-database"></a>Adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="34fe1-158"><a id="create-database"></a>Create a database</span></span>

<span data-ttu-id="34fe1-159">Ezután hozzon létre egy Azure Cosmos DB [adatbázis](documentdb-resources.md#databases) használatával a [Documentclient](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) metódus vagy [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) metódusában a  **DocumentClient** osztályával a [DocumentDB .NET SDK](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="34fe1-159">Next, create an Azure Cosmos DB [database](documentdb-resources.md#databases) by using the [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) method or [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) method of the **DocumentClient** class from the [DocumentDB .NET SDK](documentdb-sdk-dotnet.md).</span></span> <span data-ttu-id="34fe1-160">Az adatbázis a JSON-dokumentumtároló gyűjtemények között particionált logikai tárolója.</span><span class="sxs-lookup"><span data-stu-id="34fe1-160">A database is the logical container of JSON document storage partitioned across collections.</span></span>

```csharp
await client.CreateDatabaseAsync(new Database { Id = "db" });
```
## <a name="decide-on-a-partition-key"></a><span data-ttu-id="34fe1-161">Adja meg a partíciós kulcs</span><span class="sxs-lookup"><span data-stu-id="34fe1-161">Decide on a partition key</span></span> 

<span data-ttu-id="34fe1-162">Gyűjtemények tárolói dokumentumok tárolására.</span><span class="sxs-lookup"><span data-stu-id="34fe1-162">Collections are containers for storing documents.</span></span> <span data-ttu-id="34fe1-163">Ezek a logikai erőforrások és is [egy vagy több fizikai partíció span](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="34fe1-163">They are logical resources and can [span one or more physical partitions](partition-data.md).</span></span> <span data-ttu-id="34fe1-164">A [partíciókulcs](documentdb-partition-data.md) a tulajdonság (vagy elérési út) az adatok között a kiszolgálók vagy a partíciók terjesztéséhez használt dokumentumok belül van.</span><span class="sxs-lookup"><span data-stu-id="34fe1-164">A [partition key](documentdb-partition-data.md) is a property (or path) within your documents that is used to distribute your data among the servers or partitions.</span></span> <span data-ttu-id="34fe1-165">Az azonos partíciókulcsú összes dokumentumot partícióra vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="34fe1-165">All documents with the same partition key are stored in the same partition.</span></span> 

<span data-ttu-id="34fe1-166">A partíciós kulcs egy fontos döntés előtt hozzon létre egy gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="34fe1-166">Determining a partition key is an important decision to make before you create a collection.</span></span> <span data-ttu-id="34fe1-167">Partíció kulcsai a tulajdonság (vagy elérési út) az adatok több kiszolgálók között vagy partíciók terjeszteni használható Azure Cosmos DB dokumentumok belül.</span><span class="sxs-lookup"><span data-stu-id="34fe1-167">Partition keys are a property (or path) within your documents that can be used by Azure Cosmos DB to distribute your data among multiple servers or partitions.</span></span> <span data-ttu-id="34fe1-168">Cosmos DB csak a partíciós kulcs értékét és a kivonatolt eredménye alapján határozza meg, amely tárolja a dokumentum a partíció.</span><span class="sxs-lookup"><span data-stu-id="34fe1-168">Cosmos DB hashes the partition key value and uses the hashed result to determine the partition in which to store the document.</span></span> <span data-ttu-id="34fe1-169">Az azonos partíciókulcsú összes dokumentumot partícióra vannak tárolva, és partíciókulcsok gyűjtemény létrehozása után nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="34fe1-169">All documents with the same partition key are stored in the same partition, and partition keys cannot be changed once a collection is created.</span></span> 

<span data-ttu-id="34fe1-170">Ebben az oktatóanyagban a partíciós kulcs beállítva fogjuk `/deviceId` , hogy az összes adat egyetlen eszközhöz egyetlen partícióra van tárolva.</span><span class="sxs-lookup"><span data-stu-id="34fe1-170">For this tutorial, we're going to set the partition key to `/deviceId` so that the all the data for a single device is stored in a single partition.</span></span> <span data-ttu-id="34fe1-171">Válassza ki, hogy az értékek, amelyek segítségével a azonos gyakoriságát Cosmos DB tudja a terhelést, ha az adatok növekszik, és az adatgyűjtés a teljes teljesítmény elérése érdekében nagy számú partíciós kulcs szeretné.</span><span class="sxs-lookup"><span data-stu-id="34fe1-171">You want to choose a partition key that has a large number of values, each of which are used at about the same frequency to ensure Cosmos DB can load balance as your data grows and achieve the full throughput of the collection.</span></span> 

<span data-ttu-id="34fe1-172">Partícionálásra vonatkozó további információkért lásd: [partíció és a skála Azure Cosmos DB?](partition-data.md)</span><span class="sxs-lookup"><span data-stu-id="34fe1-172">For more information about partitioning, see [How to partition and scale in Azure Cosmos DB?](partition-data.md)</span></span> 

## <span data-ttu-id="34fe1-173"><a id="CreateColl"></a>Gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="34fe1-173"><a id="CreateColl"></a>Create a collection</span></span> 

<span data-ttu-id="34fe1-174">Most, hogy tudjuk, hogy a partíciós kulcs `/deviceId`, lehetővé teszi, hogy hozzon létre egy [gyűjtemény](documentdb-resources.md#collections) használatával a [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) metódus vagy [CreateDocumentCollectionIfNotExistsAsync ](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) metódusában a **DocumentClient** osztály.</span><span class="sxs-lookup"><span data-stu-id="34fe1-174">Now that we know our partition key, `/deviceId`, lets create a [collection](documentdb-resources.md#collections) by using the [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) method or [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) method of the **DocumentClient** class.</span></span> <span data-ttu-id="34fe1-175">A gyűjtemény egy JSON-dokumentumokat és a kapcsolódó JavaScript-alkalmazáslogikát tartalmazó tároló.</span><span class="sxs-lookup"><span data-stu-id="34fe1-175">A collection is a container of JSON documents and any associated JavaScript application logic.</span></span> 

> [!WARNING]
> <span data-ttu-id="34fe1-176">Gyűjtemény létrehozása megegyezik árképzési hatással vannak, az alkalmazás Azure Cosmos DB kommunikálni átviteli lefoglalja.</span><span class="sxs-lookup"><span data-stu-id="34fe1-176">Creating a collection has pricing implications, as you are reserving throughput for the application to communicate with Azure Cosmos DB.</span></span> <span data-ttu-id="34fe1-177">További részletekért tekintse meg a [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/cosmos-db/)</span><span class="sxs-lookup"><span data-stu-id="34fe1-177">For more details, please visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/)</span></span>
> 
> 

```csharp
// Collection for device telemetry. Here the JSON property deviceId is used  
// as the partition key to spread across partitions. Configured for 2500 RU/s  
// throughput and an indexing policy that supports sorting against any  
// number or string property. .
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 2500 });
```

<span data-ttu-id="34fe1-178">Ez a módszer egy REST API hívása Azure Cosmos DB, és a szolgáltatás a kért átviteli sebesség alapján létrehozott partícióknak számos teszi.</span><span class="sxs-lookup"><span data-stu-id="34fe1-178">This method makes a REST API call to Azure Cosmos DB, and the service provisions a number of partitions based on the requested throughput.</span></span> <span data-ttu-id="34fe1-179">Módosíthatja a gyűjtemény átviteli azt a teljesítményt kell fejleszteni a SDK használatával vagy a [Azure-portálon](set-throughput.md).</span><span class="sxs-lookup"><span data-stu-id="34fe1-179">You can change the throughput of a collection as your performance needs evolve using the SDK or the [Azure portal](set-throughput.md).</span></span>

## <span data-ttu-id="34fe1-180"><a id="CreateDoc"></a>JSON-dokumentumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="34fe1-180"><a id="CreateDoc"></a>Create JSON documents</span></span>
<span data-ttu-id="34fe1-181">Az Azure Cosmos DB szúrjon be néhány JSON-dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="34fe1-181">Let's insert some JSON documents into Azure Cosmos DB.</span></span> <span data-ttu-id="34fe1-182">A [dokumentumok](documentdb-resources.md#documents) a **DocumentClient** osztály [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) metódusának használatával hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="34fe1-182">A [document](documentdb-resources.md#documents) can be created by using the [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) method of the **DocumentClient** class.</span></span> <span data-ttu-id="34fe1-183">A dokumentumok a felhasználó által megadott (tetszőleges) JSON-tartalmak.</span><span class="sxs-lookup"><span data-stu-id="34fe1-183">Documents are user-defined (arbitrary) JSON content.</span></span> <span data-ttu-id="34fe1-184">Ez a minta az osztály egy eszköz olvasása és szúrható be egy új eszközt egy gyűjteményhez olvasása Documentclient hívásakor tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="34fe1-184">This sample class contains a device reading, and a call to CreateDocumentAsync to insert a new device reading into a collection.</span></span>

```csharp
public class DeviceReading
{
    [JsonProperty("id")]
    public string Id;

    [JsonProperty("deviceId")]
    public string DeviceId;

    [JsonConverter(typeof(IsoDateTimeConverter))]
    [JsonProperty("readingTime")]
    public DateTime ReadingTime;

    [JsonProperty("metricType")]
    public string MetricType;

    [JsonProperty("unit")]
    public string Unit;

    [JsonProperty("metricValue")]
    public double MetricValue;
  }

// Create a document. Here the partition key is extracted 
// as "XMS-0001" based on the collection definition
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "coll"),
    new DeviceReading
    {
        Id = "XMS-001-FE24C",
        DeviceId = "XMS-0001",
        MetricType = "Temperature",
        MetricValue = 105.00,
        Unit = "Fahrenheit",
        ReadingTime = DateTime.UtcNow
    });
```
## <a name="read-data"></a><span data-ttu-id="34fe1-185">Adatok olvasása</span><span class="sxs-lookup"><span data-stu-id="34fe1-185">Read data</span></span>

<span data-ttu-id="34fe1-186">Most, olvassa el a dokumentum a partíciós kulcs és a ReadDocumentAsync metódussal azonosítója.</span><span class="sxs-lookup"><span data-stu-id="34fe1-186">Let's read the document by its partition key and Id using the ReadDocumentAsync method.</span></span> <span data-ttu-id="34fe1-187">Vegye figyelembe, hogy az olvasások adni egy PartitionKey (a megfelelő a `x-ms-documentdb-partitionkey` kérelem fejléce a REST API-ban).</span><span class="sxs-lookup"><span data-stu-id="34fe1-187">Note that the reads include a PartitionKey value (corresponding to the `x-ms-documentdb-partitionkey` request header in the REST API).</span></span>

```csharp
// Read document. Needs the partition key and the Id to be specified
Document result = await client.ReadDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

DeviceReading reading = (DeviceReading)(dynamic)result;
```

## <a name="update-data"></a><span data-ttu-id="34fe1-188">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="34fe1-188">Update data</span></span>

<span data-ttu-id="34fe1-189">Most tegyük frissítése néhány adat, a ReplaceDocumentAsync metódussal.</span><span class="sxs-lookup"><span data-stu-id="34fe1-189">Now let's update some data using the ReplaceDocumentAsync method.</span></span>

```csharp
// Update the document. Partition key is not required, again extracted from the document
reading.MetricValue = 104;
reading.ReadingTime = DateTime.UtcNow;

await client.ReplaceDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  reading);
```

## <a name="delete-data"></a><span data-ttu-id="34fe1-190">Adat törlése</span><span class="sxs-lookup"><span data-stu-id="34fe1-190">Delete data</span></span>

<span data-ttu-id="34fe1-191">Most már lehetővé teszi, hogy a dokumentum által partíciókulcs és azonosító törlése DeleteDocumentAsync módszer használatával.</span><span class="sxs-lookup"><span data-stu-id="34fe1-191">Now lets delete a document by partition key and id by using the DeleteDocumentAsync method.</span></span>

```csharp
// Delete a document. The partition key is required.
await client.DeleteDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```
## <a name="query-partitioned-collections"></a><span data-ttu-id="34fe1-192">A particionált gyűjtemények lekérdezése</span><span class="sxs-lookup"><span data-stu-id="34fe1-192">Query partitioned collections</span></span>

<span data-ttu-id="34fe1-193">Amikor adatait a particionált gyűjtemények, Azure Cosmos DB automatikusan továbbítja a lekérdezés a partíciókat a partíciókulcs-értékek a szűrőben megadott (ha vannak ilyenek) megfelelő.</span><span class="sxs-lookup"><span data-stu-id="34fe1-193">When you query data in partitioned collections, Azure Cosmos DB automatically routes the query to the partitions corresponding to the partition key values specified in the filter (if there are any).</span></span> <span data-ttu-id="34fe1-194">Ez a lekérdezés például csak a partíciós kulcs "XMS-0001" tartalmazó partíció van átirányítva.</span><span class="sxs-lookup"><span data-stu-id="34fe1-194">For example, this query is routed to just the partition containing the partition key "XMS-0001".</span></span>

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```
    
<span data-ttu-id="34fe1-195">A következő lekérdezés nem rendelkezik egy szűrőt a partíciós kulcs (DeviceId), és minden olyan partíciónak, ahol hajtotta végre a partíció index alapján történő van rendezve.</span><span class="sxs-lookup"><span data-stu-id="34fe1-195">The following query does not have a filter on the partition key (DeviceId) and is fanned out to all partitions where it is executed against the partition's index.</span></span> <span data-ttu-id="34fe1-196">Vegye figyelembe, hogy meg kell adnia a EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` REST API-ja) kell rendelkeznie az SDK partíciók között a lekérdezés végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="34fe1-196">Note that you have to specify the EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` in the REST API) to have the SDK to execute a query across partitions.</span></span>

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

## <a name="parallel-query-execution"></a><span data-ttu-id="34fe1-197">Párhuzamos lekérdezés-végrehajtás</span><span class="sxs-lookup"><span data-stu-id="34fe1-197">Parallel query execution</span></span>
<span data-ttu-id="34fe1-198">Az Azure Cosmos DB DocumentDB SDK-k 1.9.0 és hajthat végre a particionált gyűjtemények, lekérdezések kis késés, még akkor is, amikor sok partíciók touch kell támogatási párhuzamos lekérdezés végrehajtási beállítások fent.</span><span class="sxs-lookup"><span data-stu-id="34fe1-198">The Azure Cosmos DB DocumentDB SDKs 1.9.0 and above support parallel query execution options, which allow you to perform low latency queries against partitioned collections, even when they need to touch a large number of partitions.</span></span> <span data-ttu-id="34fe1-199">A következő lekérdezés például több partíció párhuzamosan futó van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="34fe1-199">For example, the following query is configured to run in parallel across partitions.</span></span>

```csharp
// Cross-partition Order By queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```
    
<span data-ttu-id="34fe1-200">A következő paraméterek hangolása kezelheti párhuzamos lekérdezés-végrehajtás:</span><span class="sxs-lookup"><span data-stu-id="34fe1-200">You can manage parallel query execution by tuning the following parameters:</span></span>

* <span data-ttu-id="34fe1-201">Úgy, hogy `MaxDegreeOfParallelism`, szabályozhatja, azaz a gyűjtemény partíciókra egyidejű hálózati kapcsolatok maximális száma párhuzamos mértékét.</span><span class="sxs-lookup"><span data-stu-id="34fe1-201">By setting `MaxDegreeOfParallelism`, you can control the degree of parallelism i.e., the maximum number of simultaneous network connections to the collection's partitions.</span></span> <span data-ttu-id="34fe1-202">Ha a-1, milyen párhuzamossági az SDK kezeli.</span><span class="sxs-lookup"><span data-stu-id="34fe1-202">If you set this to -1, the degree of parallelism is managed by the SDK.</span></span> <span data-ttu-id="34fe1-203">Ha a `MaxDegreeOfParallelism` nem megadott vagy kell állítani, 0, amely az alapértelmezett érték, a gyűjtemény partíciók egyetlen hálózati kapcsolattal lesz.</span><span class="sxs-lookup"><span data-stu-id="34fe1-203">If the `MaxDegreeOfParallelism` is not specified or set to 0, which is the default value, there will be a single network connection to the collection's partitions.</span></span>
* <span data-ttu-id="34fe1-204">Úgy, hogy `MaxBufferedItemCount`, akkor is kompromisszumot lekérdezés-késleltetés és ügyféloldali memóriahasználata.</span><span class="sxs-lookup"><span data-stu-id="34fe1-204">By setting `MaxBufferedItemCount`, you can trade off query latency and client-side memory utilization.</span></span> <span data-ttu-id="34fe1-205">Ha kihagyja ezt a paramétert, vagy állítsa-1, párhuzamos lekérdezés-végrehajtás során pufferelt elemek száma. az SDK kezeli.</span><span class="sxs-lookup"><span data-stu-id="34fe1-205">If you omit this parameter or set this to -1, the number of items buffered during parallel query execution is managed by the SDK.</span></span>

<span data-ttu-id="34fe1-206">Megadott gyűjtemény olyan állapotban, a párhuzamos lekérdezések lesz az eredményeket ugyanabban a sorrendben, ahogy soros végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="34fe1-206">Given the same state of the collection, a parallel query will return results in the same order as in serial execution.</span></span> <span data-ttu-id="34fe1-207">Rendezés (ORDER BY és/vagy felső) tartalmazó kereszt-partíció lekérdezés végrehajtásakor a a DocumentDB SDK-t állít ki párhuzamos lekérdezés partíciók között, és egyesíti az ügyféloldalon globálisan rendezett eredmények eredményezett részben rendezett eredményez.</span><span class="sxs-lookup"><span data-stu-id="34fe1-207">When performing a cross-partition query that includes sorting (ORDER BY and/or TOP), the DocumentDB SDK issues the query in parallel across partitions and merges partially sorted results in the client side to produce globally ordered results.</span></span>

## <a name="execute-stored-procedures"></a><span data-ttu-id="34fe1-208">Tárolt eljárások végrehajtása</span><span class="sxs-lookup"><span data-stu-id="34fe1-208">Execute stored procedures</span></span>
<span data-ttu-id="34fe1-209">Végül végrehajthat atomi tranzakciókról dokumentumok ilyen azonosítójú eszköz, például ha a következő kódot ad hozzá a projekthez még karbantartása összesítések vagy egy eszközt az egyetlen dokumentum aktuális állapotát.</span><span class="sxs-lookup"><span data-stu-id="34fe1-209">Lastly, you can execute atomic transactions against documents with the same device ID, e.g. if you're maintaining aggregates or the latest state of a device in a single document by adding the following code to your project.</span></span>

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
    "XMS-001-FE24C");
```

<span data-ttu-id="34fe1-210">És ennyi az egész!</span><span class="sxs-lookup"><span data-stu-id="34fe1-210">And that's it!</span></span> <span data-ttu-id="34fe1-211">most már a hatékony méretezést adatok terjesztési közötti partíciók partíciókulcsot használó Azure Cosmos DB alkalmazás fő összetevőit.</span><span class="sxs-lookup"><span data-stu-id="34fe1-211">those are the main components of an Azure Cosmos DB application that uses a partition key to efficiently scale data distribution across partitions.</span></span>  

## <a name="clean-up-resources"></a><span data-ttu-id="34fe1-212">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="34fe1-212">Clean up resources</span></span>

<span data-ttu-id="34fe1-213">Hogy továbbra is használhatja az alkalmazás nem lesz, ha törli a következő lépések az Azure portálon oktatóprogram során létrehozott összes erőforrás:</span><span class="sxs-lookup"><span data-stu-id="34fe1-213">If you're not going to continue to use this app, delete all resources created by this tutorial in the Azure portal with the following steps:</span></span>

1. <span data-ttu-id="34fe1-214">Az Azure-portálon a bal oldali menüben kattintson a **erőforráscsoportok** és kattintson a létrehozott erőforrás egyedi nevét.</span><span class="sxs-lookup"><span data-stu-id="34fe1-214">From the left-hand menu in the Azure portal, click **Resource groups** and then click the unique name of the resource you created.</span></span> 
2. <span data-ttu-id="34fe1-215">Az erőforráscsoport lapján kattintson a **Törlés** elemre, írja be a törölni kívánt erőforrás nevét a szövegmezőbe, majd kattintson a **Törlés** gombra.</span><span class="sxs-lookup"><span data-stu-id="34fe1-215">On your resource group page, click **Delete**, type the name of the resource to delete in the text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="34fe1-216">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="34fe1-216">Next steps</span></span>

<span data-ttu-id="34fe1-217">Ebben az oktatóanyagban ezt a következők:</span><span class="sxs-lookup"><span data-stu-id="34fe1-217">In this tutorial, you've done the following:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="34fe1-218">Egy Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="34fe1-218">Created an Azure Cosmos DB account</span></span>
> * <span data-ttu-id="34fe1-219">Egy adatbázis és gyűjtemény létrehozása egy partíciós kulccsal</span><span class="sxs-lookup"><span data-stu-id="34fe1-219">Created a database and collection with a partition key</span></span>
> * <span data-ttu-id="34fe1-220">Létrehozott JSON-dokumentumok</span><span class="sxs-lookup"><span data-stu-id="34fe1-220">Created JSON documents</span></span>
> * <span data-ttu-id="34fe1-221">A dokumentumok frissítése</span><span class="sxs-lookup"><span data-stu-id="34fe1-221">Updated a document</span></span>
> * <span data-ttu-id="34fe1-222">A particionált gyűjtemények lekérdezése</span><span class="sxs-lookup"><span data-stu-id="34fe1-222">Queried partitioned collections</span></span>
> * <span data-ttu-id="34fe1-223">A tárolt eljárás futott</span><span class="sxs-lookup"><span data-stu-id="34fe1-223">Ran a stored procedure</span></span>
> * <span data-ttu-id="34fe1-224">A dokumentum törlése</span><span class="sxs-lookup"><span data-stu-id="34fe1-224">Deleted a document</span></span>
> * <span data-ttu-id="34fe1-225">Adatbázis törlése</span><span class="sxs-lookup"><span data-stu-id="34fe1-225">Deleted a database</span></span>

<span data-ttu-id="34fe1-226">Ezután folytassa a következő oktatóanyag és további adatok importálása a Cosmos DB fiókját.</span><span class="sxs-lookup"><span data-stu-id="34fe1-226">You can now proceed to the next tutorial and import additional data to your Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="34fe1-227">Adatok importálása az Azure Cosmos DB-be</span><span class="sxs-lookup"><span data-stu-id="34fe1-227">Import data into Azure Cosmos DB</span></span>](import-data.md)
