---
title: "Azure Cosmos DB: A tábla API a .NET fejlesztést |} Microsoft Docs"
description: "Ismerje meg, hogyan fejleszthet Azure Cosmos DB tábla API-t a .NET használatával"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 4b22cb49-8ea2-483d-bc95-1172cd009498
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/10/2017
ms.author: arramac
ms.custom: mvc
ms.openlocfilehash: 52cb5f2569b6c3a5301752b1e8bfb6cea13ff7f6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-cosmos-db-develop-with-the-table-api-in-net"></a><span data-ttu-id="8117d-103">Azure Cosmos DB: A tábla API a .NET fejlesztést</span><span class="sxs-lookup"><span data-stu-id="8117d-103">Azure Cosmos DB: Develop with the Table API in .NET</span></span>

<span data-ttu-id="8117d-104">Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="8117d-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="8117d-105">Segítségével gyorsan létrehozhat és lekérdezhet dokumentum-, kulcs/érték és gráf típusú adatbázisokat, melyek mindegyike felhasználja az Azure Cosmos DB középpontjában álló globális elosztási és horizontális skálázhatósági képességeket.</span><span class="sxs-lookup"><span data-stu-id="8117d-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from the global distribution and horizontal scale capabilities at the core of Azure Cosmos DB.</span></span>

<span data-ttu-id="8117d-106">Ez az oktatóanyag ismerteti a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="8117d-106">This tutorial covers the following tasks:</span></span> 

> [!div class="checklist"] 
> * <span data-ttu-id="8117d-107">Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="8117d-107">Create an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="8117d-108">A funkciónak az app.config fájlban</span><span class="sxs-lookup"><span data-stu-id="8117d-108">Enable functionality in the app.config file</span></span> 
> * <span data-ttu-id="8117d-109">Hozzon létre egy táblát az a [tábla API](table-introduction.md) (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="8117d-109">Create a table using the [Table API](table-introduction.md) (preview)</span></span>
> * <span data-ttu-id="8117d-110">Entitás hozzáadása a táblához</span><span class="sxs-lookup"><span data-stu-id="8117d-110">Add an entity to a table</span></span> 
> * <span data-ttu-id="8117d-111">Entitásköteg beszúrása</span><span class="sxs-lookup"><span data-stu-id="8117d-111">Insert a batch of entities</span></span> 
> * <span data-ttu-id="8117d-112">Egyetlen entitás lekérdezése</span><span class="sxs-lookup"><span data-stu-id="8117d-112">Retrieve a single entity</span></span> 
> * <span data-ttu-id="8117d-113">Lekérdezés entitások automatikus másodlagos indexek használata</span><span class="sxs-lookup"><span data-stu-id="8117d-113">Query entities using automatic secondary indexes</span></span> 
> * <span data-ttu-id="8117d-114">Entitás cseréje</span><span class="sxs-lookup"><span data-stu-id="8117d-114">Replace an entity</span></span> 
> * <span data-ttu-id="8117d-115">Entitás törlése</span><span class="sxs-lookup"><span data-stu-id="8117d-115">Delete an entity</span></span> 
> * <span data-ttu-id="8117d-116">Tábla törlése</span><span class="sxs-lookup"><span data-stu-id="8117d-116">Delete a table</span></span>
 
## <a name="tables-in-azure-cosmos-db"></a><span data-ttu-id="8117d-117">Az Azure Cosmos DB táblák</span><span class="sxs-lookup"><span data-stu-id="8117d-117">Tables in Azure Cosmos DB</span></span> 

<span data-ttu-id="8117d-118">Az Azure Cosmos DB biztosít a [tábla API](table-introduction.md) (előzetes) séma nélküli kialakítás egy kulcs-érték tároló igénylő alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="8117d-118">Azure Cosmos DB provides the [Table API](table-introduction.md) (preview) for applications that need a key-value store with a schema-less design.</span></span> <span data-ttu-id="8117d-119">Az [Azure Table Storage](../storage/common/storage-introduction.md) szolgáltatáshoz tartozó SDK-k és REST API-k képesek együttműködni az Azure Cosmos DB szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="8117d-119">[Azure Table storage](../storage/common/storage-introduction.md) SDKs and REST APIs can be used to work with Azure Cosmos DB.</span></span> <span data-ttu-id="8117d-120">Az Azure Cosmos DB használatával nagy átviteli sebességet megkövetelő táblákat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="8117d-120">You can use Azure Cosmos DB to create tables with high throughput requirements.</span></span> <span data-ttu-id="8117d-121">Az Azure Cosmos DB jelenleg nyilvános előzetes verzióban támogatja az átviteli sebességre optimalizált táblákat (más néven „prémium szintű táblákat”).</span><span class="sxs-lookup"><span data-stu-id="8117d-121">Azure Cosmos DB supports throughput-optimized tables (informally called "premium tables"), currently in public preview.</span></span> 

<span data-ttu-id="8117d-122">Az Azure Table Storage továbbra is használható nagy tárigényű és alacsonyabb átviteli sebességet megkövetelő táblákkal.</span><span class="sxs-lookup"><span data-stu-id="8117d-122">You can continue to use Azure Table storage for tables with high storage and lower throughput requirements.</span></span> <span data-ttu-id="8117d-123">Azure Cosmos DB bevezet egy jövőbeli frissítéssel tárolási optimalizált táblák támogatása, és a meglévő és új Azure Table storage-fiókok zökkenőmentesen frissíti az Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="8117d-123">Azure Cosmos DB will introduce support for storage-optimized tables in a future update, and existing and new Azure Table storage accounts will be seamlessly upgraded to Azure Cosmos DB.</span></span>

<span data-ttu-id="8117d-124">Ha Azure Table storage jelenleg használ, a következő előnyöket a "prémium table" előnézettel:</span><span class="sxs-lookup"><span data-stu-id="8117d-124">If you currently use Azure Table storage, you gain the following benefits with the "premium table" preview:</span></span>

- <span data-ttu-id="8117d-125">Kulcsrakész [globális terjesztési](distribute-data-globally.md) a többszörös homing és [automatikus és manuális feladatátvétel](regional-failover.md)</span><span class="sxs-lookup"><span data-stu-id="8117d-125">Turn-key [global distribution](distribute-data-globally.md) with multi-homing and [automatic and manual failovers](regional-failover.md)</span></span>
- <span data-ttu-id="8117d-126">Automatikus séma-független elleni tulajdonságokat ("másodlagos indexek"), és gyors lekérdezéseket indexelő támogatása</span><span class="sxs-lookup"><span data-stu-id="8117d-126">Support for automatic schema-agnostic indexing against all properties ("secondary indexes"), and fast queries</span></span> 
- <span data-ttu-id="8117d-127">Támogatja az [független méretezése tárolási és átviteli](partition-data.md), tetszőleges számú régiók között</span><span class="sxs-lookup"><span data-stu-id="8117d-127">Support for [independent scaling of storage and throughput](partition-data.md), across any number of regions</span></span>
- <span data-ttu-id="8117d-128">Támogatja az [táblánként dedikált átviteli](request-units.md) , amely a kérések száma másodpercenként több millió akár több százszor is méretezhető</span><span class="sxs-lookup"><span data-stu-id="8117d-128">Support for [dedicated throughput per table](request-units.md) that can be scaled from hundreds to millions of requests per second</span></span>
- <span data-ttu-id="8117d-129">Támogatja az [öt konzisztenciaszinteket](consistency-levels.md) rendelkezésre állás, a késés és az adott alkalmazástól függően konzisztencia kompromisszumot kell</span><span class="sxs-lookup"><span data-stu-id="8117d-129">Support for [five tunable consistency levels](consistency-levels.md) to trade off availability, latency, and consistency based on your application needs</span></span>
- <span data-ttu-id="8117d-130">rendelkezésre állás 99,99 % belül egy régiót, és a magas rendelkezésre állás érdekében további régiókban hozzáadásának lehetősége és [iparágvezető átfogó SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/) az általánosan rendelkezésre álló</span><span class="sxs-lookup"><span data-stu-id="8117d-130">99.99% availability within a single region, and ability to add more regions for higher availability, and [industry-leading comprehensive SLAs](https://azure.microsoft.com/support/legal/sla/cosmos-db/) on general availability</span></span>
- <span data-ttu-id="8117d-131">A meglévő Azure storage .NET SDK és az alkalmazáshoz nincs kódmódosításokat használata</span><span class="sxs-lookup"><span data-stu-id="8117d-131">Work with the existing Azure storage .NET SDK, and no code changes to your application</span></span>

<span data-ttu-id="8117d-132">Az előzetes Azure Cosmos DB támogatja a tábla API a .NET SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="8117d-132">During the preview, Azure Cosmos DB supports the Table API using the .NET SDK.</span></span> <span data-ttu-id="8117d-133">Letöltheti a [Azure Storage szolgáltatás előzetes SDK](https://aka.ms/premiumtablenuget) a Nugetből, amely rendelkezik a azonos osztályok és metódus-aláírása, mint a [Azure Storage szolgáltatás SDK](https://www.nuget.org/packages/WindowsAzure.Storage), de Azure Cosmos DB fiókok tábla API használatával is kapcsolódhatnak.</span><span class="sxs-lookup"><span data-stu-id="8117d-133">You can download the [Azure Storage Preview SDK](https://aka.ms/premiumtablenuget) from NuGet, that has the same classes and method signatures as the [Azure Storage SDK](https://www.nuget.org/packages/WindowsAzure.Storage), but also can connect to Azure Cosmos DB accounts using the Table API.</span></span>

<span data-ttu-id="8117d-134">Összetett Azure Table storage feladatokkal kapcsolatos további tudnivalókért lásd:</span><span class="sxs-lookup"><span data-stu-id="8117d-134">To learn more about complex Azure Table storage tasks, see:</span></span>

* [<span data-ttu-id="8117d-135">Bevezetés az Azure Cosmos DB: tábla API</span><span class="sxs-lookup"><span data-stu-id="8117d-135">Introduction to Azure Cosmos DB: Table API</span></span>](table-introduction.md)
* <span data-ttu-id="8117d-136">A tábla szolgáltatás elérhető API-kat vonatkozó referenciadokumentációt [Storage ügyféloldali kódtára a .NET-referencia](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="8117d-136">The Table service reference documentation for complete details about available APIs [Storage Client Library for .NET reference](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)</span></span>

### <a name="about-this-tutorial"></a><span data-ttu-id="8117d-137">Az oktatóanyag ismertetése</span><span class="sxs-lookup"><span data-stu-id="8117d-137">About this tutorial</span></span>
<span data-ttu-id="8117d-138">Ez az oktatóanyag a fejlesztők számára, aki ismeri az Azure Table storage SDK, és a premium szolgáltatásainak használatához használja Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="8117d-138">This tutorial is for developers who are familiar with the Azure Table storage SDK, and would like to use the premium features available using Azure Cosmos DB.</span></span> <span data-ttu-id="8117d-139">Alapul [Ismerkedés az Azure Table storage használatának .NET](table-storage-how-to-use-dotnet.md) és bemutatja, hogyan további szolgáltatásokat, például a másodlagos indexek, a létesített átviteli sebesség és a többszörös homing előnyeit.</span><span class="sxs-lookup"><span data-stu-id="8117d-139">It is based on [Get Started with Azure Table storage using .NET](table-storage-how-to-use-dotnet.md) and shows how to take advantage of additional capabilities like secondary indexes, provisioned throughput, and multi-homing.</span></span> <span data-ttu-id="8117d-140">A Microsoft foglalkozik az Azure-portál használatával hozzon létre egy Azure Cosmos DB fiókot és build és egy tábla alkalmazás központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="8117d-140">We cover how to use the Azure portal to create an Azure Cosmos DB account, and then build and deploy a Table application.</span></span> <span data-ttu-id="8117d-141">A Microsoft .NET példákból létrehozása és egy tábla törlésével és beszúrni, frissítése, törlése és tábla adatok lekérdezése is ismerteti.</span><span class="sxs-lookup"><span data-stu-id="8117d-141">We also walk through .NET examples for creating and deleting a table, and inserting, updating, deleting, and querying table data.</span></span> 

<span data-ttu-id="8117d-142">Ha még nincs telepítve a Visual Studio 2017, töltse le és használja a **szabad** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="8117d-142">If you don't already have Visual Studio 2017 installed, you can download and use the **free** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="8117d-143">Ügyeljen arra, hogy engedélyezze az **Azure Development** használatát a Visual Studio telepítése során.</span><span class="sxs-lookup"><span data-stu-id="8117d-143">Make sure that you enable **Azure development** during the Visual Studio setup.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="8117d-144">Adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="8117d-144">Create a database account</span></span>

<span data-ttu-id="8117d-145">Először hozzon létre egy Azure Cosmos DB fiókot az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="8117d-145">Let's start by creating an Azure Cosmos DB account in the Azure portal.</span></span>  

> [!TIP]
> * <span data-ttu-id="8117d-146">Már van Azure Cosmos DB fiókja?</span><span class="sxs-lookup"><span data-stu-id="8117d-146">Already have an Azure Cosmos DB account?</span></span> <span data-ttu-id="8117d-147">Ha igen, ugorjon előre [a Visual Studio megoldás beállítása](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="8117d-147">If so, skip ahead to [Set up your Visual Studio solution](#SetupVS).</span></span>
> * <span data-ttu-id="8117d-148">Kellett az Azure DocumentDB-fiókot?</span><span class="sxs-lookup"><span data-stu-id="8117d-148">Did you have an Azure DocumentDB account?</span></span> <span data-ttu-id="8117d-149">Ha igen, a fiókját most már Azure Cosmos DB fiókkal, és ugorjon előre a [a Visual Studio megoldás beállítása](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="8117d-149">If so, your account is now an Azure Cosmos DB account and you can skip ahead to [Set up your Visual Studio solution](#SetupVS).</span></span>  
> * <span data-ttu-id="8117d-150">Ha az Azure Cosmos DB Emulator használ, adja kövesse a [Azure Cosmos DB emulátor](local-emulator.md) kell beállítania az emulátor, és ugorjon előre [a Visual Studio megoldás beállítása](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="8117d-150">If you are using the Azure Cosmos DB Emulator, please follow the steps at [Azure Cosmos DB Emulator](local-emulator.md) to setup the emulator and skip ahead to [Set up your Visual Studio Solution](#SetupVS).</span></span>
<!---Loc Comment: Please, check link [Set up your Visual Studio solution] since it's not redirecting to any location.---> 
>
>

[!INCLUDE [cosmosdb-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)] 

## <a name="clone-the-sample-application"></a><span data-ttu-id="8117d-151">A mintaalkalmazás klónozása</span><span class="sxs-lookup"><span data-stu-id="8117d-151">Clone the sample application</span></span>

<span data-ttu-id="8117d-152">Most pedig klónozunk egy Table-alkalmazást a GitHubról, beállítjuk a kapcsolati karakterláncot, majd futtatni fogjuk az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8117d-152">Now let's clone a Table app from github, set the connection string, and run it.</span></span>

1. <span data-ttu-id="8117d-153">Nyisson meg egy git terminálablakot, például a git bash eszközt, és a `cd` paranccsal lépjen egy munkakönyvtárba.</span><span class="sxs-lookup"><span data-stu-id="8117d-153">Open a git terminal window, such as git bash, and `cd` to a working directory.</span></span>  

2. <span data-ttu-id="8117d-154">Futtassa a következő parancsot a mintatárház klónozásához.</span><span class="sxs-lookup"><span data-stu-id="8117d-154">Run the following command to clone the sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-getting-started
    ```

3. <span data-ttu-id="8117d-155">Ezután nyissa meg a megoldásfájlt a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="8117d-155">Then open the solution file in Visual Studio.</span></span>

## <a name="update-your-connection-string"></a><span data-ttu-id="8117d-156">A kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="8117d-156">Update your connection string</span></span>

<span data-ttu-id="8117d-157">Lépjen vissza az Azure Portalra a kapcsolati karakterlánc adataiért, majd másolja be azokat az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="8117d-157">Now go back to the Azure portal to get your connection string information and copy it into the app.</span></span>

1. <span data-ttu-id="8117d-158">Az [Azure Portalon](http://portal.azure.com/) az Azure Cosmos DB-fiókban a bal oldalsávon kattintson a **kulcsok** elemre, majd kattintson az **írási/olvasási kulcsok** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="8117d-158">In the [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in the left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="8117d-159">A másolási gombok fogjuk a képernyő jobb oldalán másolja a kapcsolati karakterláncot a következő lépésben az app.config fájlba kerülnek.</span><span class="sxs-lookup"><span data-stu-id="8117d-159">You'll use the copy buttons on the right side of the screen to copy the connection string into the app.config file in the next step.</span></span>

2. <span data-ttu-id="8117d-160">Nyissa meg az app.config fájlt a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="8117d-160">In Visual Studio, open the app.config file.</span></span> 

3. <span data-ttu-id="8117d-161">Az URI értéket másol a portálról (a Másolás gombra kattintva), és teszi a fiók-kulcsnak az értéke az App.config fájlban. Használja a korábban létrehozott fióknevet az App.config fájlban a fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="8117d-161">Copy your URI value from the portal (using the copy button) and make it the value of the account-key in app.config. Use the account name created earlier for account-name in app.config.</span></span>
  
```
<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key;TableEndpoint=https://account-name.documents.azure.com" />
```

> [!NOTE]
> <span data-ttu-id="8117d-162">Az alkalmazás használatához az szabványos Azure Table Storage, a kapcsolati karakterlánc módosítani szeretné `app.config file`.</span><span class="sxs-lookup"><span data-stu-id="8117d-162">To use this app with standard Azure Table Storage, you need to change the connection string in `app.config file`.</span></span> <span data-ttu-id="8117d-163">Azure Storage elsődleges kulcsként használja tábla fióknevet és kulcsot a fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="8117d-163">Use the account name as Table-account name and key as Azure Storage Primary key.</span></span> <br>
>`<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key;EndpointSuffix=core.windows.net" />`
> 
>

## <a name="build-and-deploy-the-app"></a><span data-ttu-id="8117d-164">Hozza létre, és az alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="8117d-164">Build and deploy the app</span></span>
1. <span data-ttu-id="8117d-165">A Visual Studióban kattintson a jobb gombbal a projektre a **Megoldáskezelőben**, majd kattintson a **NuGet-csomagok kezelése** elemre.</span><span class="sxs-lookup"><span data-stu-id="8117d-165">In Visual Studio, right-click on the project in **Solution Explorer** and then click **Manage NuGet Packages**.</span></span> 

2. <span data-ttu-id="8117d-166">A NuGet **Tallózás** mezőjébe írja be a ***WindowsAzure.Storage-PremiumTable*** kifejezést.</span><span class="sxs-lookup"><span data-stu-id="8117d-166">In the NuGet **Browse** box, type ***WindowsAzure.Storage-PremiumTable***.</span></span> <span data-ttu-id="8117d-167">Ellenőrizze **előzetes verzióját tartalmazzák**.</span><span class="sxs-lookup"><span data-stu-id="8117d-167">Check **Include prerelease versions**.</span></span>

3. <span data-ttu-id="8117d-168">Az eredmények közül telepítse a **windowsazure.Storage kifejezésre-PremiumTable** , és válassza a preview build `0.0.1-preview`.</span><span class="sxs-lookup"><span data-stu-id="8117d-168">From the results, install the **WindowsAzure.Storage-PremiumTable** and choose the preview build `0.0.1-preview`.</span></span> <span data-ttu-id="8117d-169">Ez a művelet telepíti az Azure Table storage csomag és az összes függősége.</span><span class="sxs-lookup"><span data-stu-id="8117d-169">This action installs the Azure Table storage package and all dependencies.</span></span>

4. <span data-ttu-id="8117d-170">Az alkalmazás futtatásához nyomja le a CTRL + F5 billentyűkombinációt.</span><span class="sxs-lookup"><span data-stu-id="8117d-170">Click CTRL + F5 to run the application.</span></span> 

<span data-ttu-id="8117d-171">Mostantól vissza a adatkezelő és tekintse meg a lekérdezés, módosíthatja, és a tábla adatokkal dolgozni.</span><span class="sxs-lookup"><span data-stu-id="8117d-171">You can now go back to Data Explorer and see query, modify, and work with this table data.</span></span> 

> [!NOTE]
> <span data-ttu-id="8117d-172">Az alkalmazás használatához az Azure Cosmos DB emulátorral, egyszerűen módosítani szeretné a kapcsolati karakterlánc `app.config file`.</span><span class="sxs-lookup"><span data-stu-id="8117d-172">To use this app with an Azure Cosmos DB Emulator, you just need to change the connection string in `app.config file`.</span></span> <span data-ttu-id="8117d-173">Használja a Emulator érték alá.</span><span class="sxs-lookup"><span data-stu-id="8117d-173">Use the below value for emulator.</span></span> <br>
>`<add key="StorageConnectionString" value=DefaultEndpointsProtocol=https;AccountName=localhost;AccountKey=<insertkey>==;TableEndpoint=https://localhost -->`
> 
>

## <a name="azure-cosmos-db-capabilities"></a><span data-ttu-id="8117d-174">Azure Cosmos DB-képességek</span><span class="sxs-lookup"><span data-stu-id="8117d-174">Azure Cosmos DB capabilities</span></span>
<span data-ttu-id="8117d-175">Az Azure Cosmos DB képességeket kínál, amelyek nem érhetők el az Azure Table storage API-ban számos támogat.</span><span class="sxs-lookup"><span data-stu-id="8117d-175">Azure Cosmos DB supports a number of capabilities that are not available in the Azure Table storage API.</span></span> <span data-ttu-id="8117d-176">Az új funkciók engedélyezéséhez a következő `appSettings` konfigurációs értékeket.</span><span class="sxs-lookup"><span data-stu-id="8117d-176">The new functionality can be enabled via the following `appSettings` configuration values.</span></span> <span data-ttu-id="8117d-177">Azt adta vezet be, minden új aláírások vagy az Azure Storage szolgáltatás SDK előzetes túlterheléssel.</span><span class="sxs-lookup"><span data-stu-id="8117d-177">We did not introduce any new signatures or overloads to the preview Azure Storage SDK.</span></span> <span data-ttu-id="8117d-178">Ez lehetővé teszi, hogy a standard és a prémium szintű csatlakozhat, és az egyéb Azure Storage services hasonlóan működik.</span><span class="sxs-lookup"><span data-stu-id="8117d-178">This allows you to connect to both standard and premium tables, and work with other Azure Storage services like Blobs and Queues.</span></span> 


| <span data-ttu-id="8117d-179">Kulcs</span><span class="sxs-lookup"><span data-stu-id="8117d-179">Key</span></span> | <span data-ttu-id="8117d-180">Leírás</span><span class="sxs-lookup"><span data-stu-id="8117d-180">Description</span></span> |
| --- | --- |
| <span data-ttu-id="8117d-181">TableConnectionMode</span><span class="sxs-lookup"><span data-stu-id="8117d-181">TableConnectionMode</span></span>  | <span data-ttu-id="8117d-182">Azure Cosmos-adatbázis két csatlakozási módot támogat.</span><span class="sxs-lookup"><span data-stu-id="8117d-182">Azure Cosmos DB supports two connectivity modes.</span></span> <span data-ttu-id="8117d-183">A `Gateway` mód, mindig kérések az Azure Cosmos DB átjárón, amely továbbítja a megfelelő adatok partíciókat.</span><span class="sxs-lookup"><span data-stu-id="8117d-183">In `Gateway` mode, requests are always made to the Azure Cosmos DB gateway, which forwards it to the corresponding data partitions.</span></span> <span data-ttu-id="8117d-184">A `Direct` csatlakozási mód, az ügyfél lekéri a táblák leképezése partíciókra, és a kérések közvetlenül az adatok partíciók szemben.</span><span class="sxs-lookup"><span data-stu-id="8117d-184">In `Direct` connectivity mode, the client fetches the mapping of tables to partitions, and requests are made directly against data partitions.</span></span> <span data-ttu-id="8117d-185">Ajánlott `Direct`, az alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="8117d-185">We recommend `Direct`, the default.</span></span>  |
| <span data-ttu-id="8117d-186">TableConnectionProtocol</span><span class="sxs-lookup"><span data-stu-id="8117d-186">TableConnectionProtocol</span></span> | <span data-ttu-id="8117d-187">Az Azure Cosmos DB támogatja két kapcsolat protokoll - `Https` és `Tcp`.</span><span class="sxs-lookup"><span data-stu-id="8117d-187">Azure Cosmos DB supports two connection protocols - `Https` and `Tcp`.</span></span> <span data-ttu-id="8117d-188">`Tcp`az alapértelmezett és ajánlott, mivel az több egyszerűsített.</span><span class="sxs-lookup"><span data-stu-id="8117d-188">`Tcp` is the default, and recommended because it is more lightweight.</span></span> |
| <span data-ttu-id="8117d-189">TablePreferredLocations</span><span class="sxs-lookup"><span data-stu-id="8117d-189">TablePreferredLocations</span></span> | <span data-ttu-id="8117d-190">Előnyben részesített (többhelyű) helyek az olvasási műveletek vesszővel elválasztott listája.</span><span class="sxs-lookup"><span data-stu-id="8117d-190">Comma-separated list of preferred (multi-homing) locations for reads.</span></span> <span data-ttu-id="8117d-191">Minden Azure Cosmos DB fiókhoz társítható 1-30 + régiók.</span><span class="sxs-lookup"><span data-stu-id="8117d-191">Each Azure Cosmos DB account can be associated with 1-30+ regions.</span></span> <span data-ttu-id="8117d-192">Minden ügyfél példány e régiók részhalmazát megadhat kis késleltetésű olvasása csatlakozási kísérleteinek kívánt sorrendjét.</span><span class="sxs-lookup"><span data-stu-id="8117d-192">Each client instance can specify a subset of these regions in the preferred order for low latency reads.</span></span> <span data-ttu-id="8117d-193">A régiók névvel kell ellátni használatával a [megjelenített neveket](https://msdn.microsoft.com/library/azure/gg441293.aspx), például `West US`.</span><span class="sxs-lookup"><span data-stu-id="8117d-193">The regions must be named using their [display names](https://msdn.microsoft.com/library/azure/gg441293.aspx), for example, `West US`.</span></span> <span data-ttu-id="8117d-194">Lásd még: [több homing API-k](tutorial-global-distribution-table.md).</span><span class="sxs-lookup"><span data-stu-id="8117d-194">Also see [Multi-homing APIs](tutorial-global-distribution-table.md).</span></span>
| <span data-ttu-id="8117d-195">TableConsistencyLevel</span><span class="sxs-lookup"><span data-stu-id="8117d-195">TableConsistencyLevel</span></span> | <span data-ttu-id="8117d-196">Akkor is kompromisszumot közötti késleltetés, konzisztencia- és rendelkezésre állás öt jól meghatározott konzisztenciaszintek közötti választással: `Strong`, `Session`, `Bounded-Staleness`, `ConsistentPrefix`, és `Eventual`.</span><span class="sxs-lookup"><span data-stu-id="8117d-196">You can trade off between latency, consistency, and availability by choosing between five well-defined consistency levels: `Strong`, `Session`, `Bounded-Staleness`, `ConsistentPrefix`, and `Eventual`.</span></span> <span data-ttu-id="8117d-197">Alapértelmezett érték a `Session`.</span><span class="sxs-lookup"><span data-stu-id="8117d-197">Default is `Session`.</span></span> <span data-ttu-id="8117d-198">A választott konzisztenciaszint lehetővé teszi több területi beállítások jelentős teljesítménybeli különbség.</span><span class="sxs-lookup"><span data-stu-id="8117d-198">The choice of consistency level makes a significant performance difference in multi-region setups.</span></span> <span data-ttu-id="8117d-199">Lásd: [konzisztenciaszintek](consistency-levels.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="8117d-199">See [Consistency levels](consistency-levels.md) for details.</span></span> |
| <span data-ttu-id="8117d-200">TableThroughput</span><span class="sxs-lookup"><span data-stu-id="8117d-200">TableThroughput</span></span> | <span data-ttu-id="8117d-201">A következő táblázatban a kérelemegység (RU) másodpercenként kifejezett fenntartott átviteli sebességet.</span><span class="sxs-lookup"><span data-stu-id="8117d-201">Reserved throughput for the table expressed in request units (RU) per second.</span></span> <span data-ttu-id="8117d-202">Egyetlen tábla RU/mp 100-as egység millióit képes támogatni.</span><span class="sxs-lookup"><span data-stu-id="8117d-202">Single tables can support 100s-millions of RU/s.</span></span> <span data-ttu-id="8117d-203">Lásd: [egységek kérelem](request-units.md).</span><span class="sxs-lookup"><span data-stu-id="8117d-203">See [Request units](request-units.md).</span></span> <span data-ttu-id="8117d-204">Alapértelmezett érték`400`</span><span class="sxs-lookup"><span data-stu-id="8117d-204">Default is `400`</span></span> |
| <span data-ttu-id="8117d-205">TableIndexingPolicy</span><span class="sxs-lookup"><span data-stu-id="8117d-205">TableIndexingPolicy</span></span> | <span data-ttu-id="8117d-206">Egységes és automatikus másodlagos indexelése lévő táblák oszlopok</span><span class="sxs-lookup"><span data-stu-id="8117d-206">Consistent and automatic secondary indexing of all columns within tables</span></span> | <span data-ttu-id="8117d-207">Az indexelési házirendet specifikációjának megfelelő JSON-karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="8117d-207">JSON string conforming to the indexing policy specification.</span></span> <span data-ttu-id="8117d-208">Lásd: [indexelő házirend](indexing-policies.md) tekintheti meg, hogyan módosíthatja az egyes oszlopok belefoglalási/kizárási indexelési házirendet.</span><span class="sxs-lookup"><span data-stu-id="8117d-208">See [Indexing Policy](indexing-policies.md) to see how you can change indexing policy to include/exclude specific columns.</span></span> | <span data-ttu-id="8117d-209">Automatikus indexeléshez levő összes tulajdonság (kivonatoló karakterláncok), és a számok tartománya</span><span class="sxs-lookup"><span data-stu-id="8117d-209">Automatic indexing of all properties (hash for strings, and range for numbers)</span></span> |
| <span data-ttu-id="8117d-210">TableQueryMaxItemCount</span><span class="sxs-lookup"><span data-stu-id="8117d-210">TableQueryMaxItemCount</span></span> | <span data-ttu-id="8117d-211">Konfigurálja az egyetlen oda-vissza tábla lekérdezésenként visszaküldött elemek maximális számát.</span><span class="sxs-lookup"><span data-stu-id="8117d-211">Configure the maximum number of items returned per table query in a single round trip.</span></span> <span data-ttu-id="8117d-212">Alapértelmezett érték a `-1`, ami lehetővé teszi, hogy Azure Cosmos DB dinamikusan alapján határozhatja meg a futási időben.</span><span class="sxs-lookup"><span data-stu-id="8117d-212">Default is `-1`, which lets Azure Cosmos DB dynamically determine the value at runtime.</span></span> |
| <span data-ttu-id="8117d-213">TableQueryEnableScan</span><span class="sxs-lookup"><span data-stu-id="8117d-213">TableQueryEnableScan</span></span> | <span data-ttu-id="8117d-214">Ha a lekérdezés nem használja az index bármely szűrőt, majd futtassa ennek ellenére a vizsgálat keresztül.</span><span class="sxs-lookup"><span data-stu-id="8117d-214">If the query cannot use the index for any filter, then run it anyway via a scan.</span></span> <span data-ttu-id="8117d-215">Alapértelmezett érték a `false`.</span><span class="sxs-lookup"><span data-stu-id="8117d-215">Default is `false`.</span></span>|
| <span data-ttu-id="8117d-216">TableQueryMaxDegreeOfParallelism</span><span class="sxs-lookup"><span data-stu-id="8117d-216">TableQueryMaxDegreeOfParallelism</span></span> | <span data-ttu-id="8117d-217">A kereszt-partíció lekérdezés végrehajtási párhuzamossági fokát.</span><span class="sxs-lookup"><span data-stu-id="8117d-217">The degree of parallelism for execution of a cross-partition query.</span></span> <span data-ttu-id="8117d-218">`0`a nem lehívását, soros `1` van előre terjesztésekor, és a magasabb értékkel soros növekedjen párhuzamossági.</span><span class="sxs-lookup"><span data-stu-id="8117d-218">`0` is serial with no pre-fetching, `1` is serial with pre-fetching, and higher values increase the rate of parallelism.</span></span> <span data-ttu-id="8117d-219">Alapértelmezett érték a `-1`, ami lehetővé teszi, hogy Azure Cosmos DB dinamikusan alapján határozhatja meg a futási időben.</span><span class="sxs-lookup"><span data-stu-id="8117d-219">Default is `-1`, which lets Azure Cosmos DB dynamically determine the value at runtime.</span></span> |

<span data-ttu-id="8117d-220">Az alapértelmezett érték módosításához nyissa meg a `app.config` fájlt a Visual Studio megoldáskezelőjében.</span><span class="sxs-lookup"><span data-stu-id="8117d-220">To change the default value, open the `app.config` file from Solution Explorer in Visual Studio.</span></span> <span data-ttu-id="8117d-221">Adja hozzá az alábbi `<appSettings>` elem tartalmát.</span><span class="sxs-lookup"><span data-stu-id="8117d-221">Add the contents of the `<appSettings>` element shown below.</span></span> <span data-ttu-id="8117d-222">Cserélje le `account-name` a tárfiók nevével és `account-key` a fiók hozzáférési kulccsal.</span><span class="sxs-lookup"><span data-stu-id="8117d-222">Replace `account-name` with the name of your storage account, and `account-key` with your account access key.</span></span> 

```xml
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
    </startup>
    <appSettings>
      <!-- Client options -->
      <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key; TableEndpoint=https://account-name.documents.azure.com" />
      <add key="TableConnectionMode" value="Direct"/>
      <add key="TableConnectionProtocol" value="Tcp"/>
      <add key="TablePreferredLocations" value="East US, West US, North Europe"/>
      <add key="TableConsistencyLevel" value="Eventual"/>

      <!--Table creation options -->
      <add key="TableThroughput" value="700"/>
      <add key="TableIndexingPolicy" value="{""indexingMode"": ""Consistent""}"/>

      <!-- Table query options -->
      <add key="TableQueryMaxItemCount" value="-1"/>
      <add key="TableQueryEnableScan" value="false"/>
      <add key="TableQueryMaxDegreeOfParallelism" value="-1"/>
      <add key="TableQueryContinuationTokenLimitInKb" value="16"/>
            
    </appSettings>
</configuration>
```

<span data-ttu-id="8117d-223">Tekintsük át, hogy mi történik az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="8117d-223">Let's make a quick review of what's happening in the app.</span></span> <span data-ttu-id="8117d-224">Nyissa meg a `Program.cs` fájlt, és található, az alábbi kódsorokat hozzon létre a tábla erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="8117d-224">Open the `Program.cs` file and you find that these lines of code create the Table resources.</span></span> 

## <a name="create-the-table-client"></a><span data-ttu-id="8117d-225">A tábla ügyfél létrehozása</span><span class="sxs-lookup"><span data-stu-id="8117d-225">Create the table client</span></span>
<span data-ttu-id="8117d-226">Ön inicializálni egy `CloudTableClient` kapcsolódni a tábla fiókjához.</span><span class="sxs-lookup"><span data-stu-id="8117d-226">You initialize a `CloudTableClient` to connect to the table account.</span></span>

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```
<span data-ttu-id="8117d-227">Ez az ügyfél inicializálása használatával a `TableConnectionMode`, `TableConnectionProtocol`, `TableConsistencyLevel`, és `TablePreferredLocations` konfigurációs értékei, ha az alkalmazás beállításaiban megadott.</span><span class="sxs-lookup"><span data-stu-id="8117d-227">This client is initialized using the `TableConnectionMode`, `TableConnectionProtocol`, `TableConsistencyLevel`, and `TablePreferredLocations` configuration values if specified in the app settings.</span></span>
    
## <a name="create-a-table"></a><span data-ttu-id="8117d-228">Tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="8117d-228">Create a table</span></span>
<span data-ttu-id="8117d-229">Ezután létrehozhat egy tábla használatával `CloudTable`.</span><span class="sxs-lookup"><span data-stu-id="8117d-229">Then, you create a table using `CloudTable`.</span></span> <span data-ttu-id="8117d-230">Azure Cosmos DB táblák egymástól függetlenül is méretezhető tárolási és átviteli sebesség szempontjából, és particionálás kezeli automatikusan a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="8117d-230">Tables in Azure Cosmos DB can scale independently in terms of storage and throughput, and partitioning is handled automatically by the service.</span></span> <span data-ttu-id="8117d-231">Azure Cosmos-adatbázis a rögzített méretű és korlátlan táblák is támogatja.</span><span class="sxs-lookup"><span data-stu-id="8117d-231">Azure Cosmos DB supports both fixed size and unlimited tables.</span></span> <span data-ttu-id="8117d-232">Lásd: [Azure Cosmos DB a particionálás](partition-data.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="8117d-232">See [Partitioning in Azure Cosmos DB](partition-data.md) for details.</span></span> 

```csharp
CloudTable table = tableClient.GetTableReference("people");

table.CreateIfNotExists();
```

<span data-ttu-id="8117d-233">Nincs a táblák létrehozását egy fontos különbséggel.</span><span class="sxs-lookup"><span data-stu-id="8117d-233">There is an important difference in how tables are created.</span></span> <span data-ttu-id="8117d-234">Cosmos. Azure-adatbázis fenntartja a teljesítményt, eltérően az Azure storage fogyasztás alapján modell az egyes tranzakciókra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="8117d-234">Azure Cosmos DB reserves throughput, unlike Azure storage's consumption-based model for transactions.</span></span> <span data-ttu-id="8117d-235">A foglalási modellnek két nagy előnye van:</span><span class="sxs-lookup"><span data-stu-id="8117d-235">The reservation model has two key benefits:</span></span>

* <span data-ttu-id="8117d-236">Az átviteli sebesség dedikált/foglalt, így Ön soha nem get szabályozva a kérelmek aránya vagy az alatt a létesített átviteli sebesség esetén</span><span class="sxs-lookup"><span data-stu-id="8117d-236">Your throughput is dedicated/reserved, so you never get throttled if your request rate is at or below your provisioned throughput</span></span>
* <span data-ttu-id="8117d-237">A foglalási modell több [költséghatékony a teljesítmény-gyakori feladatok](key-value-store-cost.md)</span><span class="sxs-lookup"><span data-stu-id="8117d-237">The reservation model is more [cost effective for throughput-heavy workloads](key-value-store-cost.md)</span></span>

<span data-ttu-id="8117d-238">Konfigurálja a beállítást úgy állíthatja be, az alapértelmezett átviteli `TableThroughput` RU (kérelemegység) másodpercenként tekintetében.</span><span class="sxs-lookup"><span data-stu-id="8117d-238">You can configure the default throughput by configuring the setting for `TableThroughput` in terms of RU (request units) per second.</span></span> 

<span data-ttu-id="8117d-239">Egy 1 KB-os entitás olvasási van normalizált 1 RU rögzített érték a Processzor, memória és IOPS fogyasztás alapján normalizálják RU és egyéb műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="8117d-239">A read of a 1-KB entity is normalized as 1 RU, and other operations are normalized to a fixed RU value based on their CPU, memory, and IOPS consumption.</span></span> <span data-ttu-id="8117d-240">További információ [Azure Cosmos DB egység kérelem](request-units.md).</span><span class="sxs-lookup"><span data-stu-id="8117d-240">Learn more about [Request units in Azure Cosmos DB](request-units.md).</span></span>

> [!NOTE]
> <span data-ttu-id="8117d-241">A Table storage SDK jelenleg nem támogatja a módosítása átviteli, amíg az átviteli sebesség azonnal bármikor az Azure portálon vagy az Azure parancssori felület használatával módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="8117d-241">While Table storage SDK does not currently support modifying throughput, you can change the throughput instantaneously at any time using the Azure portal or Azure CLI.</span></span>

<span data-ttu-id="8117d-242">A következő azt végezze el az egyszerű olvasható, és írási (CRUD) típusú műveletek az Azure Table storage SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="8117d-242">Next, we walk through the simple read and write (CRUD) operations using the Azure Table storage SDK.</span></span> <span data-ttu-id="8117d-243">Ez az oktatóanyag azt mutatja be, előre jelezhető alacsony egyjegyű ezredmásodperces késések és Azure Cosmos DB által nyújtott gyors lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="8117d-243">This tutorial demonstrates predictable low single-digit millisecond latencies and fast queries provided by Azure Cosmos DB.</span></span>

## <a name="add-an-entity-to-a-table"></a><span data-ttu-id="8117d-244">Entitás hozzáadása a táblához</span><span class="sxs-lookup"><span data-stu-id="8117d-244">Add an entity to a table</span></span>
<span data-ttu-id="8117d-245">Az Azure Table storage entitások terjessze ki a a `TableEntity` osztályhoz, és rendelkeznie kell `PartitionKey` és `RowKey` tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="8117d-245">Entities in Azure Table storage extend from the `TableEntity` class and must have `PartitionKey` and `RowKey` properties.</span></span> <span data-ttu-id="8117d-246">Íme egy minta definíciója egy ügyfélentitást.</span><span class="sxs-lookup"><span data-stu-id="8117d-246">Here's a sample definition for a customer entity.</span></span>

```csharp
public class CustomerEntity : TableEntity
{
    public CustomerEntity(string lastName, string firstName)
    {
        this.PartitionKey = lastName;
        this.RowKey = firstName;
    }

    public CustomerEntity() { }

    public string Email { get; set; }

    public string PhoneNumber { get; set; }
}
```

<span data-ttu-id="8117d-247">Az alábbi kódrészletben láthatja az Azure storage SDK rendelkező entitás beszúrása.</span><span class="sxs-lookup"><span data-stu-id="8117d-247">The following snippet shows how to insert an entity with the Azure storage SDK.</span></span> <span data-ttu-id="8117d-248">Azure Cosmos DB végzi a kis késleltetésű bármilyen léptékben garantált keresztül történik.</span><span class="sxs-lookup"><span data-stu-id="8117d-248">Azure Cosmos DB is designed for guaranteed low latency at any scale, across the world.</span></span>

<span data-ttu-id="8117d-249">Írási műveletek befejezése < 15 ms p99 és ~ 6 ms: p50 ugyanabban a régióban, az Azure Cosmos DB fiókként futó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="8117d-249">Writes complete <15 ms at p99 and ~6 ms at p50 for applications running in the same region as the Azure Cosmos DB account.</span></span> <span data-ttu-id="8117d-250">És ennek az időtartamnak a tényt, hogy írási műveletek is vonatkozik, az ügyfél csak azokat a rendszer szinkron módon replikálja, tartósan véglegesítve lett, és minden tartalom indexelt után tartozó fiókok.</span><span class="sxs-lookup"><span data-stu-id="8117d-250">And this duration accounts for the fact that writes are acknowledged back to the client only after they are synchronously replicated, durably committed, and all content is indexed.</span></span>

<span data-ttu-id="8117d-251">A tábla API-t a Azure Cosmos DB jelenleg előzetes verzióban érhető.</span><span class="sxs-lookup"><span data-stu-id="8117d-251">The Table API for Azure Cosmos DB is in preview.</span></span> <span data-ttu-id="8117d-252">Általánosan rendelkezésre álló p99 késés garanciák SLA-k, például a más Azure Cosmos DB API-k által támogatott.</span><span class="sxs-lookup"><span data-stu-id="8117d-252">At general availability, the p99 latency guarantees are backed by SLAs like other Azure Cosmos DB APIs.</span></span> 

```csharp
// Create a new customer entity.
CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
customer1.Email = "Walter@contoso.com";
customer1.PhoneNumber = "425-555-0101";

// Create the TableOperation object that inserts the customer entity.
TableOperation insertOperation = TableOperation.Insert(customer1);

// Execute the insert operation.
table.Execute(insertOperation);
```

## <a name="insert-a-batch-of-entities"></a><span data-ttu-id="8117d-253">Entitásköteg beszúrása</span><span class="sxs-lookup"><span data-stu-id="8117d-253">Insert a batch of entities</span></span>
<span data-ttu-id="8117d-254">Az Azure Table storage támogatja a kötegelt művelet API-t, amely lehetővé teszi, hogy kombinálhatja a frissítéseket, törléseket és beszúrásokat az egyetlen kötegművelettel.</span><span class="sxs-lookup"><span data-stu-id="8117d-254">Azure Table storage supports a batch operation API, that lets you combine updates, deletes, and inserts in the same single batch operation.</span></span> <span data-ttu-id="8117d-255">Azure Cosmos-adatbázis nem lehet a korlátozások némelyike a kötegelt API-t az Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="8117d-255">Azure Cosmos DB does not have some of the limitations on the batch API as Azure Table storage.</span></span> <span data-ttu-id="8117d-256">Például egy kötegelt belül többszöri beolvasás végezheti el, több írási műveleteket ad ki ugyanaz az entitás egy kötegelt belül végezheti el, és kötegenként 100 műveletek korlátozva van.</span><span class="sxs-lookup"><span data-stu-id="8117d-256">For example, you can perform multiple reads within a batch, you can perform multiple writes to the same entity within a batch, and there is no limit on 100 operations per batch.</span></span> 

```csharp
// Create the batch operation.
TableBatchOperation batchOperation = new TableBatchOperation();

// Create a customer entity and add it to the table.
CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
customer1.Email = "Jeff@contoso.com";
customer1.PhoneNumber = "425-555-0104";

// Create another customer entity and add it to the table.
CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
customer2.Email = "Ben@contoso.com";
customer2.PhoneNumber = "425-555-0102";

// Add both customer entities to the batch insert operation.
batchOperation.Insert(customer1);
batchOperation.Insert(customer2);

// Execute the batch operation.
table.ExecuteBatch(batchOperation);
```
## <a name="retrieve-a-single-entity"></a><span data-ttu-id="8117d-257">Egyetlen entitás lekérdezése</span><span class="sxs-lookup"><span data-stu-id="8117d-257">Retrieve a single entity</span></span>
<span data-ttu-id="8117d-258">(Lekérdezi) lekérdezi a teljes Azure Cosmos DB < 10 ms p99 és ~ 1 p50 azonos Azure-régióban, ms.</span><span class="sxs-lookup"><span data-stu-id="8117d-258">Retrieves (GETs) in Azure Cosmos DB complete <10 ms at p99 and ~1 ms at p50 in the same Azure region.</span></span> <span data-ttu-id="8117d-259">Annyi régiók hozzáadnia a fiókhoz a kis késleltetésű olvasása, és telepítsen alkalmazásokat úgy, hogy a helyi régió ("többhelyű") olvasni `TablePreferredLocations`.</span><span class="sxs-lookup"><span data-stu-id="8117d-259">You can add as many regions to your account for low latency reads, and deploy applications to read from their local region ("multi-homed") by setting `TablePreferredLocations`.</span></span> 

<span data-ttu-id="8117d-260">Az alábbi kódrészletben használatával egyetlen entitás le:</span><span class="sxs-lookup"><span data-stu-id="8117d-260">You can retrieve a single entity using the following snippet:</span></span>

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
> [!TIP]
> <span data-ttu-id="8117d-261">További tudnivalók a többhelyű API-k [fejlesztés több régióba az](tutorial-global-distribution-table.md)</span><span class="sxs-lookup"><span data-stu-id="8117d-261">Learn about multi-homing APIs at [Developing with multiple regions](tutorial-global-distribution-table.md)</span></span>
>

## <a name="query-entities-using-automatic-secondary-indexes"></a><span data-ttu-id="8117d-262">Lekérdezés entitások automatikus másodlagos indexek használata</span><span class="sxs-lookup"><span data-stu-id="8117d-262">Query entities using automatic secondary indexes</span></span>
<span data-ttu-id="8117d-263">Táblázatok használatával lehet lekérdezni a `TableQuery` osztály.</span><span class="sxs-lookup"><span data-stu-id="8117d-263">Tables can be queried using the `TableQuery` class.</span></span> <span data-ttu-id="8117d-264">Azure Cosmos-adatbázis egy adatbázis írási optimalizált motor, amely automatikusan elvégzi a belül a táblázat összes oszlopa van.</span><span class="sxs-lookup"><span data-stu-id="8117d-264">Azure Cosmos DB has a write-optimized database engine that automatically indexes all columns within your table.</span></span> <span data-ttu-id="8117d-265">Az Azure Cosmos Adatbázisba indexelő független sémát.</span><span class="sxs-lookup"><span data-stu-id="8117d-265">Indexing in Azure Cosmos DB is agnostic to schema.</span></span> <span data-ttu-id="8117d-266">Ezért még akkor is, ha a séma sorok különböznek, vagy a séma fejlődésének adott idő alatt, akkor automatikusan indexelt.</span><span class="sxs-lookup"><span data-stu-id="8117d-266">Therefore, even if your schema is different between rows, or if the schema evolves over time, it is automatically indexed.</span></span> <span data-ttu-id="8117d-267">Mivel Azure Cosmos DB támogatja az automatikus másodlagos indexek, lekérdezések egyik tulajdonságnak sem használhatja az index, és hatékonyan kell kézbesíteni.</span><span class="sxs-lookup"><span data-stu-id="8117d-267">Since Azure Cosmos DB supports automatic secondary indexes, queries against any property can use the index and be served efficiently.</span></span>

```csharp
CloudTable table = tableClient.GetTableReference("people");

// Filter against a property that's not partition key or row key
TableQuery<CustomerEntity> emailQuery = new TableQuery<CustomerEntity>().Where(
    TableQuery.GenerateFilterCondition("Email", QueryComparisons.Equal, "Ben@contoso.com"));

foreach (CustomerEntity entity in table.ExecuteQuery(emailQuery))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

<span data-ttu-id="8117d-268">A képen Azure Cosmos DB Azure Table storage azonos lekérdezés funkciókat támogatja a tábla API-hoz.</span><span class="sxs-lookup"><span data-stu-id="8117d-268">In preview, Azure Cosmos DB supports the same query functionality as Azure Table storage for the Table API.</span></span> <span data-ttu-id="8117d-269">Azure Cosmos-adatbázis is támogatja a rendezést, összesítések, a földrajzi lekérdezést, a hierarchia és a számos különféle beépített funkciók.</span><span class="sxs-lookup"><span data-stu-id="8117d-269">Azure Cosmos DB also supports sorting, aggregates, geospatial query, hierarchy, and a wide range of built-in functions.</span></span> <span data-ttu-id="8117d-270">A további funkciókat nyújtanak a jövőbeli szolgáltatásfrissítés tábla API.</span><span class="sxs-lookup"><span data-stu-id="8117d-270">The additional functionality will be provided in the Table API in a future service update.</span></span> <span data-ttu-id="8117d-271">Lásd: [Azure Cosmos adatbázis-lekérdezés](documentdb-sql-query.md) ezeket a képességeket áttekintését.</span><span class="sxs-lookup"><span data-stu-id="8117d-271">See [Azure Cosmos DB query](documentdb-sql-query.md) for an overview of these capabilities.</span></span> 

## <a name="replace-an-entity"></a><span data-ttu-id="8117d-272">Entitás cseréje</span><span class="sxs-lookup"><span data-stu-id="8117d-272">Replace an entity</span></span>
<span data-ttu-id="8117d-273">Ha frissíteni kíván egy entitást, kérje le a Table szolgáltatásból, módosítsa az entitásobjektumot, majd mentse a módosításokat a Table szolgáltatásba.</span><span class="sxs-lookup"><span data-stu-id="8117d-273">To update an entity, retrieve it from the Table service, modify the entity object, and then save the changes back to the Table service.</span></span> <span data-ttu-id="8117d-274">A következő kód egy meglévő ügyfél telefonszámát módosítja.</span><span class="sxs-lookup"><span data-stu-id="8117d-274">The following code changes an existing customer's phone number.</span></span> 

```csharp
TableOperation updateOperation = TableOperation.Replace(updateEntity);
table.Execute(updateOperation);
```
<span data-ttu-id="8117d-275">Hasonló módon végezheti el `InsertOrMerge` vagy `Merge` műveletek.</span><span class="sxs-lookup"><span data-stu-id="8117d-275">Similarly, you can perform `InsertOrMerge` or `Merge` operations.</span></span>  

## <a name="delete-an-entity"></a><span data-ttu-id="8117d-276">Entitás törlése</span><span class="sxs-lookup"><span data-stu-id="8117d-276">Delete an entity</span></span>
<span data-ttu-id="8117d-277">A lekérdezés után egyszerűen törölheti az entitásokat az entitások frissítésénél bemutatott minta alapján.</span><span class="sxs-lookup"><span data-stu-id="8117d-277">You can easily delete an entity after you have retrieved it by using the same pattern shown for updating an entity.</span></span> <span data-ttu-id="8117d-278">Az alábbi kód lekérdez, majd töröl egy ügyfélentitást.</span><span class="sxs-lookup"><span data-stu-id="8117d-278">The following code retrieves and deletes a customer entity.</span></span>

```csharp
TableOperation deleteOperation = TableOperation.Delete(deleteEntity);
table.Execute(deleteOperation);
```

## <a name="delete-a-table"></a><span data-ttu-id="8117d-279">Tábla törlése</span><span class="sxs-lookup"><span data-stu-id="8117d-279">Delete a table</span></span>
<span data-ttu-id="8117d-280">Végezetül pedig az alábbi példakóddal törölhető egy tábla a tárfiókból.</span><span class="sxs-lookup"><span data-stu-id="8117d-280">Finally, the following code example deletes a table from a storage account.</span></span> <span data-ttu-id="8117d-281">Töröl, és hozza létre újból az azonnali Azure Cosmos DB tartalmazó tábla.</span><span class="sxs-lookup"><span data-stu-id="8117d-281">You can delete and recreate a table immediately with Azure Cosmos DB.</span></span>

```csharp
CloudTable table = tableClient.GetTableReference("people");
table.DeleteIfExists();
```

## <a name="clean-up-resources"></a><span data-ttu-id="8117d-282">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="8117d-282">Clean up resources</span></span> 

<span data-ttu-id="8117d-283">Ha az alkalmazást már nem használja, a következő lépések használatával törölje az oktatóanyag során létrehozott összes erőforrást az Azure Portalon.</span><span class="sxs-lookup"><span data-stu-id="8117d-283">If you're not going to continue to use this app, use the following steps to delete all resources created by this tutorial in the Azure portal.</span></span>   

1. <span data-ttu-id="8117d-284">Az Azure Portal bal oldali menüjében kattintson az **Erőforráscsoportok** lehetőségre, majd kattintson a létrehozott erőforrás nevére.</span><span class="sxs-lookup"><span data-stu-id="8117d-284">From the left-hand menu in the Azure portal, click **Resource groups** and then click the name of the resource you created.</span></span>  
2. <span data-ttu-id="8117d-285">Az erőforráscsoport lapján kattintson a **Törlés** elemre, írja be a törölni kívánt erőforrás nevét a szövegmezőbe, majd kattintson a **Törlés** gombra.</span><span class="sxs-lookup"><span data-stu-id="8117d-285">On your resource group page, click **Delete**, type the name of the resource to delete in the text box, and then click **Delete**.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="8117d-286">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8117d-286">Next steps</span></span>

<span data-ttu-id="8117d-287">Az oktatóanyag azt a kezelt Ismerkedés az Azure Cosmos DB használatával tábla API-val, és ezt a következő:</span><span class="sxs-lookup"><span data-stu-id="8117d-287">In this tutorial, we covered how to get started using Azure Cosmos DB with the Table API, and you've done the following:</span></span> 

> [!div class="checklist"] 
> * <span data-ttu-id="8117d-288">Egy Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="8117d-288">Created an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="8117d-289">Engedélyezett funkciókat az app.config fájlban</span><span class="sxs-lookup"><span data-stu-id="8117d-289">Enabled functionality in the app.config file</span></span> 
> * <span data-ttu-id="8117d-290">Táblázat létrehozása</span><span class="sxs-lookup"><span data-stu-id="8117d-290">Created a table</span></span> 
> * <span data-ttu-id="8117d-291">Egy entitás hozzá</span><span class="sxs-lookup"><span data-stu-id="8117d-291">Added an entity to a table</span></span> 
> * <span data-ttu-id="8117d-292">Szúrja be egy teljes entitásköteget</span><span class="sxs-lookup"><span data-stu-id="8117d-292">Inserted a batch of entities</span></span> 
> * <span data-ttu-id="8117d-293">Egyetlen entitás lekérése</span><span class="sxs-lookup"><span data-stu-id="8117d-293">Retrieved a single entity</span></span> 
> * <span data-ttu-id="8117d-294">Automatikus másodlagos indexek használatával lekérdezett entitásokat</span><span class="sxs-lookup"><span data-stu-id="8117d-294">Queried entities using automatic secondary indexes</span></span> 
> * <span data-ttu-id="8117d-295">Entitás cseréje</span><span class="sxs-lookup"><span data-stu-id="8117d-295">Replaced an entity</span></span> 
> * <span data-ttu-id="8117d-296">Egy entitás törlése</span><span class="sxs-lookup"><span data-stu-id="8117d-296">Deleted an entity</span></span> 
> * <span data-ttu-id="8117d-297">Tábla törlése</span><span class="sxs-lookup"><span data-stu-id="8117d-297">Deleted a table</span></span>  

<span data-ttu-id="8117d-298">Ezután folytassa a következő oktatóanyag és tudhat meg többet a tábla adatainak lekérdezésekor.</span><span class="sxs-lookup"><span data-stu-id="8117d-298">You can now proceed to the next tutorial and learn more about querying table data.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="8117d-299">A tábla API lekérdezés</span><span class="sxs-lookup"><span data-stu-id="8117d-299">Query with the Table API</span></span>](tutorial-query-table.md)
