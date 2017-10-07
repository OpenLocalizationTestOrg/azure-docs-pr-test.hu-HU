---
title: "Azure Cosmos DB: Fejlesztése a DocumentDB API .NET hello |} Microsoft Docs"
description: "Megtudhatja, hogyan toodevelop Azure Cosmos DB DocumentDB API-t a .NET használatával"
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
ms.openlocfilehash: 0d3d17afa782054c8fdf3cbac421e5a5d0a6800c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmosdb-develop-with-hello-documentdb-api-in-net"></a><span data-ttu-id="9b73d-103">Azure CosmosDB: A DocumentDB API .NET hello fejleszthet</span><span class="sxs-lookup"><span data-stu-id="9b73d-103">Azure CosmosDB: Develop with hello DocumentDB API in .NET</span></span>

<span data-ttu-id="9b73d-104">Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="9b73d-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="9b73d-105">Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="9b73d-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="9b73d-106">Ez az oktatóanyag bemutatja, hogyan egy Azure Cosmos DB fiók használatával toocreate hello Azure-portálon, és majd a dokumentum-adatbázis és gyűjtemény létrehozása egy [partíciókulcs](documentdb-partition-data.md#partition-keys) hello segítségével [DocumentDB .NET API](documentdb-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9b73d-106">This tutorial demonstrates how toocreate an Azure Cosmos DB account using hello Azure portal, and then create a document database and collection with a [partition key](documentdb-partition-data.md#partition-keys) using hello [DocumentDB .NET API](documentdb-introduction.md).</span></span> <span data-ttu-id="9b73d-107">Hozzon létre egy partíciókulcsot, ha létrehoz egy gyűjteményt, amelyet az alkalmazás készen áll a testreszabásra tooscale egyszerűen lehet az adatok növekedésével.</span><span class="sxs-lookup"><span data-stu-id="9b73d-107">By defining a partition key when you create a collection, your application is prepared tooscale effortlessly as your data grows.</span></span> 

<span data-ttu-id="9b73d-108">Az útmutató tartalma hello alábbi feladatokat hello segítségével [DocumentDB .NET API](documentdb-sdk-dotnet.md):</span><span class="sxs-lookup"><span data-stu-id="9b73d-108">This tutorial covers hello following tasks by using hello [DocumentDB .NET API](documentdb-sdk-dotnet.md):</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9b73d-109">Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="9b73d-109">Create an Azure Cosmos DB account</span></span>
> * <span data-ttu-id="9b73d-110">Egy adatbázis és gyűjtemény létrehozása egy partíciós kulccsal</span><span class="sxs-lookup"><span data-stu-id="9b73d-110">Create a database and collection with a partition key</span></span>
> * <span data-ttu-id="9b73d-111">JSON-dokumentumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="9b73d-111">Create JSON documents</span></span>
> * <span data-ttu-id="9b73d-112">Dokumentum frissítése</span><span class="sxs-lookup"><span data-stu-id="9b73d-112">Update a document</span></span>
> * <span data-ttu-id="9b73d-113">A particionált gyűjtemények lekérdezése</span><span class="sxs-lookup"><span data-stu-id="9b73d-113">Query partitioned collections</span></span>
> * <span data-ttu-id="9b73d-114">A tárolt eljárások futtatása</span><span class="sxs-lookup"><span data-stu-id="9b73d-114">Run stored procedures</span></span>
> * <span data-ttu-id="9b73d-115">Dokumentum törlése</span><span class="sxs-lookup"><span data-stu-id="9b73d-115">Delete a document</span></span>
> * <span data-ttu-id="9b73d-116">Adatbázis törlése</span><span class="sxs-lookup"><span data-stu-id="9b73d-116">Delete a database</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9b73d-117">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9b73d-117">Prerequisites</span></span>
<span data-ttu-id="9b73d-118">Győződjön meg arról, hogy a következő hello:</span><span class="sxs-lookup"><span data-stu-id="9b73d-118">Please make sure you have hello following:</span></span>

* <span data-ttu-id="9b73d-119">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="9b73d-119">An active Azure account.</span></span> <span data-ttu-id="9b73d-120">Ha még nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="9b73d-120">If you don't have one, you can sign up for a [free account](https://azure.microsoft.com/free/).</span></span> 
    * <span data-ttu-id="9b73d-121">Másik lehetőségként használhatja a hello [Azure Cosmos DB emulátor](local-emulator.md) ehhez az oktatóanyaghoz, ha azt szeretné, hogy toouse emulálja a fejlesztéshez hello Azure DocumentDB szolgáltatás egy helyi környezet.</span><span class="sxs-lookup"><span data-stu-id="9b73d-121">Alternatively, you can use hello [Azure Cosmos DB Emulator](local-emulator.md) for this tutorial if you'd like toouse a local environment that emulates hello Azure DocumentDB service for development purposes.</span></span>
* <span data-ttu-id="9b73d-122">[Visual Studio](http://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="9b73d-122">[Visual Studio](http://www.visualstudio.com/).</span></span>

## <a name="create-an-azure-cosmos-db-account"></a><span data-ttu-id="9b73d-123">Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="9b73d-123">Create an Azure Cosmos DB account</span></span>

<span data-ttu-id="9b73d-124">Először hozzon létre egy Azure Cosmos DB fiókot a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="9b73d-124">Let's start by creating an Azure Cosmos DB account in hello Azure portal.</span></span>

> [!TIP]
> * <span data-ttu-id="9b73d-125">Már van Azure Cosmos DB fiókja?</span><span class="sxs-lookup"><span data-stu-id="9b73d-125">Already have an Azure Cosmos DB account?</span></span> <span data-ttu-id="9b73d-126">Ha igen, hagyja ki azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS)</span><span class="sxs-lookup"><span data-stu-id="9b73d-126">If so, skip ahead too[Set up your Visual Studio solution](#SetupVS)</span></span>
> * <span data-ttu-id="9b73d-127">Kellett az Azure DocumentDB-fiókot?</span><span class="sxs-lookup"><span data-stu-id="9b73d-127">Did you have an Azure DocumentDB account?</span></span> <span data-ttu-id="9b73d-128">Ha igen, a fiókját most már Azure Cosmos DB fiókkal, és kihagyhatja azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="9b73d-128">If so, your account is now an Azure Cosmos DB account and you can skip ahead too[Set up your Visual Studio solution](#SetupVS).</span></span>  
> * <span data-ttu-id="9b73d-129">Ha hello Azure Cosmos DB Emulator használata esetén kövesse hello készítésével [Azure Cosmos DB emulátor](local-emulator.md) toosetup hello emulátor, és hagyja ki azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="9b73d-129">If you are using hello Azure Cosmos DB Emulator, please follow hello steps at [Azure Cosmos DB Emulator](local-emulator.md) toosetup hello emulator and skip ahead too[Set up your Visual Studio Solution](#SetupVS).</span></span> 
>
>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="9b73d-130"><a id="SetupVS"></a>A Visual Studio-megoldás beállítása</span><span class="sxs-lookup"><span data-stu-id="9b73d-130"><a id="SetupVS"></a>Set up your Visual Studio solution</span></span>
1. <span data-ttu-id="9b73d-131">Nyissa meg a **Visual Studiót** a számítógépén.</span><span class="sxs-lookup"><span data-stu-id="9b73d-131">Open **Visual Studio** on your computer.</span></span>
2. <span data-ttu-id="9b73d-132">A hello **fájl** menüjében válassza **új**, és válassza a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="9b73d-132">On hello **File** menu, select **New**, and then choose **Project**.</span></span>
3. <span data-ttu-id="9b73d-133">A hello **új projekt** párbeszédablakban válassza **sablonok** / **Visual C#** / **Konzolalkalmazás (.NET-keretrendszer)**, nevezze el a projektet, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="9b73d-133">In hello **New Project** dialog, select **Templates** / **Visual C#** / **Console App (.NET Framework)**, name your project, and then click **OK**.</span></span>
   <span data-ttu-id="9b73d-134">![Képernyőfelvétel a hello új projekt ablakról](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-new-project-2.png)</span><span class="sxs-lookup"><span data-stu-id="9b73d-134">![Screen shot of hello New Project window](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-new-project-2.png)</span></span>

4. <span data-ttu-id="9b73d-135">A hello **Megoldáskezelőben**, kattintson az új konzolalkalmazásra, amely a Visual Studio megoldás alatt, a jobb gombbal, és kattintson a **NuGet-csomagok kezelése...**</span><span class="sxs-lookup"><span data-stu-id="9b73d-135">In hello **Solution Explorer**, right click on your new console application, which is under your Visual Studio solution, and then click **Manage NuGet Packages...**</span></span>
    
    ![Képernyőfelvétel a hello jobbra a hello projekt helyi](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-manage-nuget-pacakges.png)
5. <span data-ttu-id="9b73d-137">A hello **NuGet** lapra, majd **Tallózás**, és írja be **documentdb** hello Keresés mezőbe.</span><span class="sxs-lookup"><span data-stu-id="9b73d-137">In hello **NuGet** tab, click **Browse**, and type **documentdb** in hello search box.</span></span>
<!---stopped here--->
6. <span data-ttu-id="9b73d-138">Hello eredmények belül található **Microsoft.Azure.DocumentDB** kattintson **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="9b73d-138">Within hello results, find **Microsoft.Azure.DocumentDB** and click **Install**.</span></span>
   <span data-ttu-id="9b73d-139">hello csomag azonosítója hello Azure Cosmos DB ügyféloldali kódtár [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB).</span><span class="sxs-lookup"><span data-stu-id="9b73d-139">hello package ID for hello Azure Cosmos DB Client Library is [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB).</span></span>
   <span data-ttu-id="9b73d-140">![Képernyőfelvétel a hello NuGet menüről a Azure Cosmos DB ügyfél SDK-kereséshez](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-manage-nuget-pacakges-2.png)</span><span class="sxs-lookup"><span data-stu-id="9b73d-140">![Screen shot of hello NuGet Menu for finding Azure Cosmos DB Client SDK](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-manage-nuget-pacakges-2.png)</span></span>

    <span data-ttu-id="9b73d-141">Ha kapcsolatos módosításokat toohello megoldás áttekintése figyelmeztető hibaüzenet jelenik meg, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="9b73d-141">If you get a message about reviewing changes toohello solution, click **OK**.</span></span> <span data-ttu-id="9b73d-142">Ha a licenc elfogadásáról szóló üzenetet kap, kattintson az **Elfogadom** gombra.</span><span class="sxs-lookup"><span data-stu-id="9b73d-142">If you get a message about license acceptance, click **I accept**.</span></span>

## <span data-ttu-id="9b73d-143"><a id="Connect"></a>Hivatkozások tooyour projekt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9b73d-143"><a id="Connect"></a>Add references tooyour project</span></span>
<span data-ttu-id="9b73d-144">az oktatóanyag adjon meg hello DocumentDB API kód kódtöredékek szükséges toocreate és a frissítés Azure Cosmos DB erőforrásokat a projekt fennmaradó hello szükséges lépések.</span><span class="sxs-lookup"><span data-stu-id="9b73d-144">hello remaining steps in this tutorial provide hello DocumentDB API code snippets required toocreate and update Azure Cosmos DB resources in your project.</span></span>

<span data-ttu-id="9b73d-145">Először adja hozzá ezeket a hivatkozásokat tooyour alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9b73d-145">First, add these references tooyour application.</span></span>
<!---These aren't added by default when you install hello pkg?--->

```csharp
using System.Net;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
```

## <span data-ttu-id="9b73d-146"><a id="add-references"></a>Az alkalmazás kapcsolódni</span><span class="sxs-lookup"><span data-stu-id="9b73d-146"><a id="add-references"></a>Connect your app</span></span>

<span data-ttu-id="9b73d-147">Ezután adja hozzá ezt a két állandót és a *ügyfél* változó az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9b73d-147">Next, add these two constants and your *client* variable in your application.</span></span>

```csharp
private const string EndpointUrl = "<your endpoint URL>";
private const string PrimaryKey = "<your primary key>";
private DocumentClient client;
```

<span data-ttu-id="9b73d-148">Ezt követően a head biztonsági toohello [Azure-portálon](https://portal.azure.com) tooretrieve a végponti URL-cím és az elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="9b73d-148">Then, head back toohello [Azure portal](https://portal.azure.com) tooretrieve your endpoint URL and primary key.</span></span> <span data-ttu-id="9b73d-149">hello végponti URL-cím és elsődleges kulcs szükség az alkalmazás toounderstand ahol tooconnect esetén és Azure Cosmos DB tootrust az alkalmazás által létesített kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="9b73d-149">hello endpoint URL and primary key are necessary for your application toounderstand where tooconnect to, and for Azure Cosmos DB tootrust your application's connection.</span></span>

<span data-ttu-id="9b73d-150">Az Azure-portálon hello lépjen tooyour Azure Cosmos DB fiókot, kattintson **kulcsok**, és kattintson a **írható-olvasható kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="9b73d-150">In hello Azure portal, navigate tooyour Azure Cosmos DB account, click **Keys**, and then click **Read-write Keys**.</span></span>

<span data-ttu-id="9b73d-151">Hello portálról hello URI másolása és beillesztése azt `<your endpoint URL>` hello program.cs fájlban.</span><span class="sxs-lookup"><span data-stu-id="9b73d-151">Copy hello URI from hello portal and paste it over `<your endpoint URL>` in hello program.cs file.</span></span> <span data-ttu-id="9b73d-152">Akkor másolási elsődleges kulcs hello hello portálról, és illessze be keresztül `<your primary key>`.</span><span class="sxs-lookup"><span data-stu-id="9b73d-152">Then copy hello PRIMARY KEY from hello portal and paste it over `<your primary key>`.</span></span> <span data-ttu-id="9b73d-153">Lehet, hogy tooremove hello `<` és `>` a értékeiből.</span><span class="sxs-lookup"><span data-stu-id="9b73d-153">Be sure tooremove hello `<` and `>` from your values.</span></span>

![Képernyőfelvétel a hello Azure-portálon hello NoSQL-oktatóanyag toocreate által használt egy C# konzolalkalmazást.](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-keys.png)

## <span data-ttu-id="9b73d-156"><a id="instantiate"></a>Hello DocumentClient példányosítható</span><span class="sxs-lookup"><span data-stu-id="9b73d-156"><a id="instantiate"></a>Instantiate hello DocumentClient</span></span>

<span data-ttu-id="9b73d-157">Most, hozzon létre egy új példányát hello **DocumentClient**.</span><span class="sxs-lookup"><span data-stu-id="9b73d-157">Now, create a new instance of hello **DocumentClient**.</span></span>

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
```

## <span data-ttu-id="9b73d-158"><a id="create-database"></a>Adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="9b73d-158"><a id="create-database"></a>Create a database</span></span>

<span data-ttu-id="9b73d-159">Ezután hozzon létre egy Azure Cosmos DB [adatbázis](documentdb-resources.md#databases) hello segítségével [Documentclient](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) metódus vagy [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) hello metódusában ** DocumentClient** hello osztály [DocumentDB .NET SDK](documentdb-sdk-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="9b73d-159">Next, create an Azure Cosmos DB [database](documentdb-resources.md#databases) by using hello [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) method or [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) method of hello **DocumentClient** class from hello [DocumentDB .NET SDK](documentdb-sdk-dotnet.md).</span></span> <span data-ttu-id="9b73d-160">Egy adatbázis a JSON-dokumentumtároló gyűjtemények között particionált logikai tárolója hello.</span><span class="sxs-lookup"><span data-stu-id="9b73d-160">A database is hello logical container of JSON document storage partitioned across collections.</span></span>

```csharp
await client.CreateDatabaseAsync(new Database { Id = "db" });
```
## <a name="decide-on-a-partition-key"></a><span data-ttu-id="9b73d-161">Adja meg a partíciós kulcs</span><span class="sxs-lookup"><span data-stu-id="9b73d-161">Decide on a partition key</span></span> 

<span data-ttu-id="9b73d-162">Gyűjtemények tárolói dokumentumok tárolására.</span><span class="sxs-lookup"><span data-stu-id="9b73d-162">Collections are containers for storing documents.</span></span> <span data-ttu-id="9b73d-163">Ezek a logikai erőforrások és is [egy vagy több fizikai partíció span](partition-data.md).</span><span class="sxs-lookup"><span data-stu-id="9b73d-163">They are logical resources and can [span one or more physical partitions](partition-data.md).</span></span> <span data-ttu-id="9b73d-164">A [partíciókulcs](documentdb-partition-data.md) a tulajdonság (vagy elérési út) belül van, amely használt toodistribute dokumentumok hello kiszolgálók között vagy partíció adatait.</span><span class="sxs-lookup"><span data-stu-id="9b73d-164">A [partition key](documentdb-partition-data.md) is a property (or path) within your documents that is used toodistribute your data among hello servers or partitions.</span></span> <span data-ttu-id="9b73d-165">Hello ugyanazzal a partíciókulccsal tárolja az összes dokumentum hello egyazon partícióra kerüljenek.</span><span class="sxs-lookup"><span data-stu-id="9b73d-165">All documents with hello same partition key are stored in hello same partition.</span></span> 

<span data-ttu-id="9b73d-166">A partíciós kulcs egy fontos döntés toomake, egy gyűjtemény létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="9b73d-166">Determining a partition key is an important decision toomake before you create a collection.</span></span> <span data-ttu-id="9b73d-167">Partíciókulcsok a tulajdonság (vagy elérési út) tartoznak, amelyek lehetnek dokumentumok Azure Cosmos DB toodistribute használja az adatok több kiszolgálók között vagy partíció.</span><span class="sxs-lookup"><span data-stu-id="9b73d-167">Partition keys are a property (or path) within your documents that can be used by Azure Cosmos DB toodistribute your data among multiple servers or partitions.</span></span> <span data-ttu-id="9b73d-168">Cosmos DB hello a partíciós kulcs értéke csak, és használja a kivonatolt hello eredmény toodetermine hello partíció mely toostore hello dokumentumban.</span><span class="sxs-lookup"><span data-stu-id="9b73d-168">Cosmos DB hashes hello partition key value and uses hello hashed result toodetermine hello partition in which toostore hello document.</span></span> <span data-ttu-id="9b73d-169">Hello ugyanazzal a partíciókulccsal tárolja az összes dokumentum hello egyazon partícióra kerüljenek, és partíciókulcsok gyűjtemény létrehozása után nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="9b73d-169">All documents with hello same partition key are stored in hello same partition, and partition keys cannot be changed once a collection is created.</span></span> 

<span data-ttu-id="9b73d-170">Ebben az oktatóanyagban az oktatóanyagban módosítjuk tooset hello partíciós kulcs túl`/deviceId` úgy, amely hello hello minden adatot az egyetlen eszközt egyetlen partícióra van tárolva.</span><span class="sxs-lookup"><span data-stu-id="9b73d-170">For this tutorial, we're going tooset hello partition key too`/deviceId` so that hello all hello data for a single device is stored in a single partition.</span></span> <span data-ttu-id="9b73d-171">Azt szeretné, hogy toochoose, amely rendelkezik, amelyek mindegyike használt értékek nagy számú partíciós kulcsa: hello kapcsolatos azonos gyakoriság tooensure Cosmos DB is a terhelést, ha az adatok növekszik, és hello teljes átviteli képessége – hello gyűjtemény elérése.</span><span class="sxs-lookup"><span data-stu-id="9b73d-171">You want toochoose a partition key that has a large number of values, each of which are used at about hello same frequency tooensure Cosmos DB can load balance as your data grows and achieve hello full throughput of hello collection.</span></span> 

<span data-ttu-id="9b73d-172">Partícionálásra vonatkozó további információkért lásd: [hogyan toopartition és a skála Azure Cosmos DB?](partition-data.md)</span><span class="sxs-lookup"><span data-stu-id="9b73d-172">For more information about partitioning, see [How toopartition and scale in Azure Cosmos DB?](partition-data.md)</span></span> 

## <span data-ttu-id="9b73d-173"><a id="CreateColl"></a>Gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="9b73d-173"><a id="CreateColl"></a>Create a collection</span></span> 

<span data-ttu-id="9b73d-174">Most, hogy tudjuk, hogy a partíciós kulcs `/deviceId`, lehetővé teszi, hogy hozzon létre egy [gyűjtemény](documentdb-resources.md#collections) hello segítségével [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) metódus vagy [ CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) hello metódusában **DocumentClient** osztály.</span><span class="sxs-lookup"><span data-stu-id="9b73d-174">Now that we know our partition key, `/deviceId`, lets create a [collection](documentdb-resources.md#collections) by using hello [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) method or [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="9b73d-175">A gyűjtemény egy JSON-dokumentumokat és a kapcsolódó JavaScript-alkalmazáslogikát tartalmazó tároló.</span><span class="sxs-lookup"><span data-stu-id="9b73d-175">A collection is a container of JSON documents and any associated JavaScript application logic.</span></span> 

> [!WARNING]
> <span data-ttu-id="9b73d-176">Gyűjtemény létrehozása megegyezik megvalósítását, árképzési lefoglalja hello alkalmazás toocommunicate rendelkező Azure Cosmos DB adatátviteli sebességét.</span><span class="sxs-lookup"><span data-stu-id="9b73d-176">Creating a collection has pricing implications, as you are reserving throughput for hello application toocommunicate with Azure Cosmos DB.</span></span> <span data-ttu-id="9b73d-177">További részletekért tekintse meg a [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/cosmos-db/)</span><span class="sxs-lookup"><span data-stu-id="9b73d-177">For more details, please visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/)</span></span>
> 
> 

```csharp
// Collection for device telemetry. Here hello JSON property deviceId is used  
// as hello partition key toospread across partitions. Configured for 2500 RU/s  
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

<span data-ttu-id="9b73d-178">A metódus elérhetővé válnak a REST API-t hívja fel a tooAzure Cosmos DB, és hello szolgáltatás rendelkezések hello kért átviteli sebesség alapján létrehozott partícióknak számos.</span><span class="sxs-lookup"><span data-stu-id="9b73d-178">This method makes a REST API call tooAzure Cosmos DB, and hello service provisions a number of partitions based on hello requested throughput.</span></span> <span data-ttu-id="9b73d-179">A teljesítmény igények fejlődnek hello SDK vagy hello segítségével módosíthatja hello átviteli gyűjtemény [Azure-portálon](set-throughput.md).</span><span class="sxs-lookup"><span data-stu-id="9b73d-179">You can change hello throughput of a collection as your performance needs evolve using hello SDK or hello [Azure portal](set-throughput.md).</span></span>

## <span data-ttu-id="9b73d-180"><a id="CreateDoc"></a>JSON-dokumentumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="9b73d-180"><a id="CreateDoc"></a>Create JSON documents</span></span>
<span data-ttu-id="9b73d-181">Az Azure Cosmos DB szúrjon be néhány JSON-dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="9b73d-181">Let's insert some JSON documents into Azure Cosmos DB.</span></span> <span data-ttu-id="9b73d-182">A [dokumentum](documentdb-resources.md#documents) hello segítségével hozhatók létre [Documentclient](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) hello metódusában **DocumentClient** osztály.</span><span class="sxs-lookup"><span data-stu-id="9b73d-182">A [document](documentdb-resources.md#documents) can be created by using hello [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) method of hello **DocumentClient** class.</span></span> <span data-ttu-id="9b73d-183">A dokumentumok a felhasználó által megadott (tetszőleges) JSON-tartalmak.</span><span class="sxs-lookup"><span data-stu-id="9b73d-183">Documents are user-defined (arbitrary) JSON content.</span></span> <span data-ttu-id="9b73d-184">Ez a minta az osztály tartalmaz egy eszköz olvasásra, és egy hívás tooCreateDocumentAsync tooinsert egy új eszközt egy gyűjteményhez olvasásakor.</span><span class="sxs-lookup"><span data-stu-id="9b73d-184">This sample class contains a device reading, and a call tooCreateDocumentAsync tooinsert a new device reading into a collection.</span></span>

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

// Create a document. Here hello partition key is extracted 
// as "XMS-0001" based on hello collection definition
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
## <a name="read-data"></a><span data-ttu-id="9b73d-185">Adatok olvasása</span><span class="sxs-lookup"><span data-stu-id="9b73d-185">Read data</span></span>

<span data-ttu-id="9b73d-186">Most elolvasni hello dokumentumot a partíciókulcs és hello ReadDocumentAsync metódussal azonosítója.</span><span class="sxs-lookup"><span data-stu-id="9b73d-186">Let's read hello document by its partition key and Id using hello ReadDocumentAsync method.</span></span> <span data-ttu-id="9b73d-187">Vegye figyelembe, hogy a hello olvasások PartitionKey értéket tartalmazza (megfelelő toohello `x-ms-documentdb-partitionkey` hello REST API-t a kérelem fejlécében).</span><span class="sxs-lookup"><span data-stu-id="9b73d-187">Note that hello reads include a PartitionKey value (corresponding toohello `x-ms-documentdb-partitionkey` request header in hello REST API).</span></span>

```csharp
// Read document. Needs hello partition key and hello Id toobe specified
Document result = await client.ReadDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

DeviceReading reading = (DeviceReading)(dynamic)result;
```

## <a name="update-data"></a><span data-ttu-id="9b73d-188">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="9b73d-188">Update data</span></span>

<span data-ttu-id="9b73d-189">Most tegyük a néhány hello ReplaceDocumentAsync metódussal adatainak frissítése.</span><span class="sxs-lookup"><span data-stu-id="9b73d-189">Now let's update some data using hello ReplaceDocumentAsync method.</span></span>

```csharp
// Update hello document. Partition key is not required, again extracted from hello document
reading.MetricValue = 104;
reading.ReadingTime = DateTime.UtcNow;

await client.ReplaceDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  reading);
```

## <a name="delete-data"></a><span data-ttu-id="9b73d-190">Adat törlése</span><span class="sxs-lookup"><span data-stu-id="9b73d-190">Delete data</span></span>

<span data-ttu-id="9b73d-191">Most már lehetővé teszi, hogy a dokumentum által partíciókulcs és azonosító törlése hello DeleteDocumentAsync módszer használatával.</span><span class="sxs-lookup"><span data-stu-id="9b73d-191">Now lets delete a document by partition key and id by using hello DeleteDocumentAsync method.</span></span>

```csharp
// Delete a document. hello partition key is required.
await client.DeleteDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```
## <a name="query-partitioned-collections"></a><span data-ttu-id="9b73d-192">A particionált gyűjtemények lekérdezése</span><span class="sxs-lookup"><span data-stu-id="9b73d-192">Query partitioned collections</span></span>

<span data-ttu-id="9b73d-193">Amikor a adatait a particionált gyűjtemények, Azure Cosmos DB automatikusan útvonalak hello lekérdezési toohello partíciók megfelelő toohello partíció megadott kulcsértékek hello szűrő (ha vannak ilyenek).</span><span class="sxs-lookup"><span data-stu-id="9b73d-193">When you query data in partitioned collections, Azure Cosmos DB automatically routes hello query toohello partitions corresponding toohello partition key values specified in hello filter (if there are any).</span></span> <span data-ttu-id="9b73d-194">Például egy, a lekérdezés nem útválasztásos toojust hello partíció tartalmazó hello partíciós kulcs "XMS-0001".</span><span class="sxs-lookup"><span data-stu-id="9b73d-194">For example, this query is routed toojust hello partition containing hello partition key "XMS-0001".</span></span>

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```
    
<span data-ttu-id="9b73d-195">hello következő lekérdezés nincs szűrő hello partíciókulcs (DeviceId) és tooall partíciók hol végre hello partíció index alapján van rendezve.</span><span class="sxs-lookup"><span data-stu-id="9b73d-195">hello following query does not have a filter on hello partition key (DeviceId) and is fanned out tooall partitions where it is executed against hello partition's index.</span></span> <span data-ttu-id="9b73d-196">Vegye figyelembe, hogy rendelkezik-e toospecify hello EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` a hello REST API-t) toohave hello SDK tooexecute közötti partíciók lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="9b73d-196">Note that you have toospecify hello EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` in hello REST API) toohave hello SDK tooexecute a query across partitions.</span></span>

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

## <a name="parallel-query-execution"></a><span data-ttu-id="9b73d-197">Párhuzamos lekérdezés-végrehajtás</span><span class="sxs-lookup"><span data-stu-id="9b73d-197">Parallel query execution</span></span>
<span data-ttu-id="9b73d-198">hello Azure Cosmos DB DocumentDB SDK 1.9.0 és támogatási fent párhuzamos lekérdezés végrehajtási beállítások, melyek lehetővé teszik tooperform kis késleltetésű lekérdezi a particionált gyűjtemények szemben még akkor is, ha szükségük van tootouch partíciók nagy számú.</span><span class="sxs-lookup"><span data-stu-id="9b73d-198">hello Azure Cosmos DB DocumentDB SDKs 1.9.0 and above support parallel query execution options, which allow you tooperform low latency queries against partitioned collections, even when they need tootouch a large number of partitions.</span></span> <span data-ttu-id="9b73d-199">Például a következő lekérdezés hello partíciók nem konfigurált toorun párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="9b73d-199">For example, hello following query is configured toorun in parallel across partitions.</span></span>

```csharp
// Cross-partition Order By queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```
    
<span data-ttu-id="9b73d-200">A következő paraméterek hello hangolása kezelheti párhuzamos lekérdezés-végrehajtás:</span><span class="sxs-lookup"><span data-stu-id="9b73d-200">You can manage parallel query execution by tuning hello following parameters:</span></span>

* <span data-ttu-id="9b73d-201">Úgy, hogy `MaxDegreeOfParallelism`, azaz hello egyidejű hálózati kapcsolatok toohello gyűjtemény a partíciók száma párhuzamos hello fokú szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="9b73d-201">By setting `MaxDegreeOfParallelism`, you can control hello degree of parallelism i.e., hello maximum number of simultaneous network connections toohello collection's partitions.</span></span> <span data-ttu-id="9b73d-202">Ha ez túl-1, párhuzamossági hello fokát hello SDK kezeli.</span><span class="sxs-lookup"><span data-stu-id="9b73d-202">If you set this too-1, hello degree of parallelism is managed by hello SDK.</span></span> <span data-ttu-id="9b73d-203">Ha hello `MaxDegreeOfParallelism` értéke nincs megadva, illetve too0, amely hello alapértelmezett érték, egy egyetlen hálózati kapcsolat toohello gyűjtemény partíciók lesz.</span><span class="sxs-lookup"><span data-stu-id="9b73d-203">If hello `MaxDegreeOfParallelism` is not specified or set too0, which is hello default value, there will be a single network connection toohello collection's partitions.</span></span>
* <span data-ttu-id="9b73d-204">Úgy, hogy `MaxBufferedItemCount`, akkor is kompromisszumot lekérdezés-késleltetés és ügyféloldali memóriahasználata.</span><span class="sxs-lookup"><span data-stu-id="9b73d-204">By setting `MaxBufferedItemCount`, you can trade off query latency and client-side memory utilization.</span></span> <span data-ttu-id="9b73d-205">Ha kihagyja ezt a paramétert, vagy állítsa 1 túl, hello száma párhuzamos lekérdezés-végrehajtás során pufferelt elemek hello SDK kezeli.</span><span class="sxs-lookup"><span data-stu-id="9b73d-205">If you omit this parameter or set this too-1, hello number of items buffered during parallel query execution is managed by hello SDK.</span></span>

<span data-ttu-id="9b73d-206">Hello megadott hello gyűjtemény olyan állapotban, a párhuzamos lekérdezések lesz az eredményeket hello ugyanaz, mint soros végrehajtási sorrendben.</span><span class="sxs-lookup"><span data-stu-id="9b73d-206">Given hello same state of hello collection, a parallel query will return results in hello same order as in serial execution.</span></span> <span data-ttu-id="9b73d-207">Rendezés (ORDER BY és/vagy felső) tartalmazó kereszt-partíció lekérdezés végrehajtásakor a hello DocumentDB SDK-t állít ki hello lekérdezés párhuzamosan partíciók között, és egyesíti hello ügyfél oldalán tooproduce globálisan rendezett eredmények részben rendezett eredményez.</span><span class="sxs-lookup"><span data-stu-id="9b73d-207">When performing a cross-partition query that includes sorting (ORDER BY and/or TOP), hello DocumentDB SDK issues hello query in parallel across partitions and merges partially sorted results in hello client side tooproduce globally ordered results.</span></span>

## <a name="execute-stored-procedures"></a><span data-ttu-id="9b73d-208">Tárolt eljárások végrehajtása</span><span class="sxs-lookup"><span data-stu-id="9b73d-208">Execute stored procedures</span></span>
<span data-ttu-id="9b73d-209">Végül, Ön is végrehajthatja a dokumentumokon végzett atomi tranzakciók ugyanazon Eszközazonosítót, pl. hello Ha éppen karbantartása összesítések, vagy hello egyetlen dokumentum eszköz aktuális állapotát a következő kód tooyour projekt hello hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="9b73d-209">Lastly, you can execute atomic transactions against documents with hello same device ID, e.g. if you're maintaining aggregates or hello latest state of a device in a single document by adding hello following code tooyour project.</span></span>

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
    "XMS-001-FE24C");
```

<span data-ttu-id="9b73d-210">És ennyi az egész!</span><span class="sxs-lookup"><span data-stu-id="9b73d-210">And that's it!</span></span> <span data-ttu-id="9b73d-211">most már az hello fő összetevői közötti partíciók partíciós kulcs tooefficiently méretezési adatok terjesztési használó Azure Cosmos DB alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9b73d-211">those are hello main components of an Azure Cosmos DB application that uses a partition key tooefficiently scale data distribution across partitions.</span></span>  

## <a name="clean-up-resources"></a><span data-ttu-id="9b73d-212">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="9b73d-212">Clean up resources</span></span>

<span data-ttu-id="9b73d-213">Toocontinue toouse az alkalmazás nem fog, ha törli az összes erőforrást oktatóprogram során létrehozott hello az Azure-portálon az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="9b73d-213">If you're not going toocontinue toouse this app, delete all resources created by this tutorial in hello Azure portal with hello following steps:</span></span>

1. <span data-ttu-id="9b73d-214">A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson a létrehozott hello erőforrás hello egyedi nevét.</span><span class="sxs-lookup"><span data-stu-id="9b73d-214">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello unique name of hello resource you created.</span></span> 
2. <span data-ttu-id="9b73d-215">Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="9b73d-215">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9b73d-216">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9b73d-216">Next steps</span></span>

<span data-ttu-id="9b73d-217">Ebben az oktatóanyagban hello következő régebben már kötöttek:</span><span class="sxs-lookup"><span data-stu-id="9b73d-217">In this tutorial, you've done hello following:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="9b73d-218">Egy Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="9b73d-218">Created an Azure Cosmos DB account</span></span>
> * <span data-ttu-id="9b73d-219">Egy adatbázis és gyűjtemény létrehozása egy partíciós kulccsal</span><span class="sxs-lookup"><span data-stu-id="9b73d-219">Created a database and collection with a partition key</span></span>
> * <span data-ttu-id="9b73d-220">Létrehozott JSON-dokumentumok</span><span class="sxs-lookup"><span data-stu-id="9b73d-220">Created JSON documents</span></span>
> * <span data-ttu-id="9b73d-221">A dokumentumok frissítése</span><span class="sxs-lookup"><span data-stu-id="9b73d-221">Updated a document</span></span>
> * <span data-ttu-id="9b73d-222">A particionált gyűjtemények lekérdezése</span><span class="sxs-lookup"><span data-stu-id="9b73d-222">Queried partitioned collections</span></span>
> * <span data-ttu-id="9b73d-223">A tárolt eljárás futott</span><span class="sxs-lookup"><span data-stu-id="9b73d-223">Ran a stored procedure</span></span>
> * <span data-ttu-id="9b73d-224">A dokumentum törlése</span><span class="sxs-lookup"><span data-stu-id="9b73d-224">Deleted a document</span></span>
> * <span data-ttu-id="9b73d-225">Adatbázis törlése</span><span class="sxs-lookup"><span data-stu-id="9b73d-225">Deleted a database</span></span>

<span data-ttu-id="9b73d-226">Ezután folytassa a következő oktatóanyag toohello és további adatok tooyour Cosmos DB fiók importálása.</span><span class="sxs-lookup"><span data-stu-id="9b73d-226">You can now proceed toohello next tutorial and import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="9b73d-227">Adatok importálása az Azure Cosmos DB-be</span><span class="sxs-lookup"><span data-stu-id="9b73d-227">Import data into Azure Cosmos DB</span></span>](import-data.md)
