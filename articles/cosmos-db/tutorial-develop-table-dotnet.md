---
title: "Az Azure Cosmos DB: Hello tábla API a .NET fejlesztést |} Microsoft Docs"
description: "Megtudhatja, hogyan toodevelop Azure Cosmos DB tábla API-t a .NET használatával"
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
ms.openlocfilehash: 70c6985a1dffdbcdb07e377f8ad10355bb97712a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-develop-with-hello-table-api-in-net"></a><span data-ttu-id="6dce6-103">Azure Cosmos DB: Tábla API a .NET hello fejlesztést</span><span class="sxs-lookup"><span data-stu-id="6dce6-103">Azure Cosmos DB: Develop with hello Table API in .NET</span></span>

<span data-ttu-id="6dce6-104">Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="6dce6-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="6dce6-105">Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="6dce6-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span>

<span data-ttu-id="6dce6-106">Ez az oktatóanyag ismerteti a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="6dce6-106">This tutorial covers hello following tasks:</span></span> 

> [!div class="checklist"] 
> * <span data-ttu-id="6dce6-107">Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="6dce6-107">Create an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="6dce6-108">A funkciónak az üdvözlő app.config fájlban</span><span class="sxs-lookup"><span data-stu-id="6dce6-108">Enable functionality in hello app.config file</span></span> 
> * <span data-ttu-id="6dce6-109">Tábla létrehozása hello segítségével [tábla API](table-introduction.md) (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="6dce6-109">Create a table using hello [Table API](table-introduction.md) (preview)</span></span>
> * <span data-ttu-id="6dce6-110">Egy entitás tooa táblázat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6dce6-110">Add an entity tooa table</span></span> 
> * <span data-ttu-id="6dce6-111">Entitásköteg beszúrása</span><span class="sxs-lookup"><span data-stu-id="6dce6-111">Insert a batch of entities</span></span> 
> * <span data-ttu-id="6dce6-112">Egyetlen entitás lekérdezése</span><span class="sxs-lookup"><span data-stu-id="6dce6-112">Retrieve a single entity</span></span> 
> * <span data-ttu-id="6dce6-113">Lekérdezés entitások automatikus másodlagos indexek használata</span><span class="sxs-lookup"><span data-stu-id="6dce6-113">Query entities using automatic secondary indexes</span></span> 
> * <span data-ttu-id="6dce6-114">Entitás cseréje</span><span class="sxs-lookup"><span data-stu-id="6dce6-114">Replace an entity</span></span> 
> * <span data-ttu-id="6dce6-115">Entitás törlése</span><span class="sxs-lookup"><span data-stu-id="6dce6-115">Delete an entity</span></span> 
> * <span data-ttu-id="6dce6-116">Tábla törlése</span><span class="sxs-lookup"><span data-stu-id="6dce6-116">Delete a table</span></span>
 
## <a name="tables-in-azure-cosmos-db"></a><span data-ttu-id="6dce6-117">Az Azure Cosmos DB táblák</span><span class="sxs-lookup"><span data-stu-id="6dce6-117">Tables in Azure Cosmos DB</span></span> 

<span data-ttu-id="6dce6-118">Az Azure Cosmos DB biztosít hello [tábla API](table-introduction.md) (előzetes) séma nélküli kialakítás egy kulcs-érték tároló igénylő alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="6dce6-118">Azure Cosmos DB provides hello [Table API](table-introduction.md) (preview) for applications that need a key-value store with a schema-less design.</span></span> <span data-ttu-id="6dce6-119">[Az Azure Table storage](../storage/common/storage-introduction.md) SDK-k és a REST API-k lehet az Azure Cosmos DB használt toowork.</span><span class="sxs-lookup"><span data-stu-id="6dce6-119">[Azure Table storage](../storage/common/storage-introduction.md) SDKs and REST APIs can be used toowork with Azure Cosmos DB.</span></span> <span data-ttu-id="6dce6-120">Magas teljesítmény-követelményekkel rendelkező Azure Cosmos DB toocreate táblák is használhatja.</span><span class="sxs-lookup"><span data-stu-id="6dce6-120">You can use Azure Cosmos DB toocreate tables with high throughput requirements.</span></span> <span data-ttu-id="6dce6-121">Az Azure Cosmos DB jelenleg nyilvános előzetes verzióban támogatja az átviteli sebességre optimalizált táblákat (más néven „prémium szintű táblákat”).</span><span class="sxs-lookup"><span data-stu-id="6dce6-121">Azure Cosmos DB supports throughput-optimized tables (informally called "premium tables"), currently in public preview.</span></span> 

<span data-ttu-id="6dce6-122">Azure Table storage magas tárolási és alacsonyabb átviteli követelmények táblák toouse tovább.</span><span class="sxs-lookup"><span data-stu-id="6dce6-122">You can continue toouse Azure Table storage for tables with high storage and lower throughput requirements.</span></span> <span data-ttu-id="6dce6-123">Azure Cosmos DB bevezet egy jövőbeli frissítéssel tárolási optimalizált táblák támogatása, és a meglévő és új Azure Table storage-fiókok zökkenőmentesen lesz frissítve tooAzure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="6dce6-123">Azure Cosmos DB will introduce support for storage-optimized tables in a future update, and existing and new Azure Table storage accounts will be seamlessly upgraded tooAzure Cosmos DB.</span></span>

<span data-ttu-id="6dce6-124">Ha Azure Table storage jelenleg használ, a következő előnyöket hello "prémium table" előnézettel hello ettől kezdve:</span><span class="sxs-lookup"><span data-stu-id="6dce6-124">If you currently use Azure Table storage, you gain hello following benefits with hello "premium table" preview:</span></span>

- <span data-ttu-id="6dce6-125">Kulcsrakész [globális terjesztési](distribute-data-globally.md) a többszörös homing és [automatikus és manuális feladatátvétel](regional-failover.md)</span><span class="sxs-lookup"><span data-stu-id="6dce6-125">Turn-key [global distribution](distribute-data-globally.md) with multi-homing and [automatic and manual failovers](regional-failover.md)</span></span>
- <span data-ttu-id="6dce6-126">Automatikus séma-független elleni tulajdonságokat ("másodlagos indexek"), és gyors lekérdezéseket indexelő támogatása</span><span class="sxs-lookup"><span data-stu-id="6dce6-126">Support for automatic schema-agnostic indexing against all properties ("secondary indexes"), and fast queries</span></span> 
- <span data-ttu-id="6dce6-127">Támogatja az [független méretezése tárolási és átviteli](partition-data.md), tetszőleges számú régiók között</span><span class="sxs-lookup"><span data-stu-id="6dce6-127">Support for [independent scaling of storage and throughput](partition-data.md), across any number of regions</span></span>
- <span data-ttu-id="6dce6-128">Támogatja az [táblánként dedikált átviteli](request-units.md) , amely az akár több százszor is méretezhető toomillions kérelmek / másodperc</span><span class="sxs-lookup"><span data-stu-id="6dce6-128">Support for [dedicated throughput per table](request-units.md) that can be scaled from hundreds toomillions of requests per second</span></span>
- <span data-ttu-id="6dce6-129">Támogatja az [öt konzisztenciaszinteket](consistency-levels.md) tootrade rendelkezésre állás, a késés és a konzisztencia ki az alkalmazások igényeihez alapján</span><span class="sxs-lookup"><span data-stu-id="6dce6-129">Support for [five tunable consistency levels](consistency-levels.md) tootrade off availability, latency, and consistency based on your application needs</span></span>
- <span data-ttu-id="6dce6-130">rendelkezésre állás 99,99 % belül egy régiót, és képes tooadd további magas rendelkezésre állás érdekében, régiók és [iparágvezető átfogó SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/) az általánosan rendelkezésre álló</span><span class="sxs-lookup"><span data-stu-id="6dce6-130">99.99% availability within a single region, and ability tooadd more regions for higher availability, and [industry-leading comprehensive SLAs](https://azure.microsoft.com/support/legal/sla/cosmos-db/) on general availability</span></span>
- <span data-ttu-id="6dce6-131">Hello meglévő az Azure storage .NET SDK-val, és egyetlen kód módosítások tooyour alkalmazás</span><span class="sxs-lookup"><span data-stu-id="6dce6-131">Work with hello existing Azure storage .NET SDK, and no code changes tooyour application</span></span>

<span data-ttu-id="6dce6-132">Hello előzetes Azure Cosmos DB támogatja hello tábla API hello .NET SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="6dce6-132">During hello preview, Azure Cosmos DB supports hello Table API using hello .NET SDK.</span></span> <span data-ttu-id="6dce6-133">Letöltheti a hello [Azure Storage szolgáltatás előzetes SDK](https://aka.ms/premiumtablenuget) a Nugetből, amely rendelkezik hello azonos osztályok és metódus-aláírása hello, [Azure Storage szolgáltatás SDK](https://www.nuget.org/packages/WindowsAzure.Storage), de is kapcsolódhatnak tooAzure Cosmos DB fiókok hello használata Tábla API.</span><span class="sxs-lookup"><span data-stu-id="6dce6-133">You can download hello [Azure Storage Preview SDK](https://aka.ms/premiumtablenuget) from NuGet, that has hello same classes and method signatures as hello [Azure Storage SDK](https://www.nuget.org/packages/WindowsAzure.Storage), but also can connect tooAzure Cosmos DB accounts using hello Table API.</span></span>

<span data-ttu-id="6dce6-134">További információ az Azure Table storage összetett feladatok, toolearn lásd:</span><span class="sxs-lookup"><span data-stu-id="6dce6-134">toolearn more about complex Azure Table storage tasks, see:</span></span>

* [<span data-ttu-id="6dce6-135">Bevezetés tooAzure Cosmos DB: tábla API</span><span class="sxs-lookup"><span data-stu-id="6dce6-135">Introduction tooAzure Cosmos DB: Table API</span></span>](table-introduction.md)
* <span data-ttu-id="6dce6-136">Table szolgáltatás referenciadokumentációt elérhető API-kat vonatkozó hello [Storage ügyféloldali kódtára a .NET-referencia](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="6dce6-136">hello Table service reference documentation for complete details about available APIs [Storage Client Library for .NET reference](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)</span></span>

### <a name="about-this-tutorial"></a><span data-ttu-id="6dce6-137">Az oktatóanyag ismertetése</span><span class="sxs-lookup"><span data-stu-id="6dce6-137">About this tutorial</span></span>
<span data-ttu-id="6dce6-138">Ez az oktatóanyag a fejlesztők számára, aki ismeri a hello Azure Table storage SDK, és szeretné toouse hello premium szolgáltatásainak használata Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="6dce6-138">This tutorial is for developers who are familiar with hello Azure Table storage SDK, and would like toouse hello premium features available using Azure Cosmos DB.</span></span> <span data-ttu-id="6dce6-139">Alapul [Ismerkedés az Azure Table storage használatának .NET](table-storage-how-to-use-dotnet.md) és bemutatja, hogyan tootake előnye, hogy további funkciókat, például másodlagos indexek, a létesített átviteli sebesség és a többszörös homing.</span><span class="sxs-lookup"><span data-stu-id="6dce6-139">It is based on [Get Started with Azure Table storage using .NET](table-storage-how-to-use-dotnet.md) and shows how tootake advantage of additional capabilities like secondary indexes, provisioned throughput, and multi-homing.</span></span> <span data-ttu-id="6dce6-140">A Microsoft fedik le, hogyan toouse hello Azure portál toocreate Azure Cosmos DB adatait, majd létrehozása és egy tábla alkalmazás központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="6dce6-140">We cover how toouse hello Azure portal toocreate an Azure Cosmos DB account, and then build and deploy a Table application.</span></span> <span data-ttu-id="6dce6-141">A Microsoft .NET példákból létrehozása és egy tábla törlésével és beszúrni, frissítése, törlése és tábla adatok lekérdezése is ismerteti.</span><span class="sxs-lookup"><span data-stu-id="6dce6-141">We also walk through .NET examples for creating and deleting a table, and inserting, updating, deleting, and querying table data.</span></span> 

<span data-ttu-id="6dce6-142">Ha még nincs telepítve a Visual Studio 2017, töltse le és használja a hello **szabad** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="6dce6-142">If you don't already have Visual Studio 2017 installed, you can download and use hello **free** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="6dce6-143">Győződjön meg arról, hogy engedélyezze **Azure fejlesztési** hello Visual Studio telepítése során.</span><span class="sxs-lookup"><span data-stu-id="6dce6-143">Make sure that you enable **Azure development** during hello Visual Studio setup.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="6dce6-144">Adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="6dce6-144">Create a database account</span></span>

<span data-ttu-id="6dce6-145">Először hozzon létre egy Azure Cosmos DB fiókot a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="6dce6-145">Let's start by creating an Azure Cosmos DB account in hello Azure portal.</span></span>  

> [!TIP]
> * <span data-ttu-id="6dce6-146">Már van Azure Cosmos DB fiókja?</span><span class="sxs-lookup"><span data-stu-id="6dce6-146">Already have an Azure Cosmos DB account?</span></span> <span data-ttu-id="6dce6-147">Ha igen, hagyja ki azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="6dce6-147">If so, skip ahead too[Set up your Visual Studio solution](#SetupVS).</span></span>
> * <span data-ttu-id="6dce6-148">Kellett az Azure DocumentDB-fiókot?</span><span class="sxs-lookup"><span data-stu-id="6dce6-148">Did you have an Azure DocumentDB account?</span></span> <span data-ttu-id="6dce6-149">Ha igen, a fiókját most már Azure Cosmos DB fiókkal, és kihagyhatja azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="6dce6-149">If so, your account is now an Azure Cosmos DB account and you can skip ahead too[Set up your Visual Studio solution](#SetupVS).</span></span>  
> * <span data-ttu-id="6dce6-150">Ha hello Azure Cosmos DB Emulator használata esetén kövesse hello készítésével [Azure Cosmos DB emulátor](local-emulator.md) toosetup hello emulátor, és hagyja ki azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS).</span><span class="sxs-lookup"><span data-stu-id="6dce6-150">If you are using hello Azure Cosmos DB Emulator, please follow hello steps at [Azure Cosmos DB Emulator](local-emulator.md) toosetup hello emulator and skip ahead too[Set up your Visual Studio Solution](#SetupVS).</span></span>
<!---Loc Comment: Please, check link [Set up your Visual Studio solution] since it's not redirecting tooany location.---> 
>
>

[!INCLUDE [cosmosdb-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)] 

## <a name="clone-hello-sample-application"></a><span data-ttu-id="6dce6-151">Klónozza a mintaalkalmazást hello</span><span class="sxs-lookup"><span data-stu-id="6dce6-151">Clone hello sample application</span></span>

<span data-ttu-id="6dce6-152">Most tegyük klónozza a githubból, állítsa be a hello kapcsolati karakterláncot, és futtassa azt egy tábla alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6dce6-152">Now let's clone a Table app from github, set hello connection string, and run it.</span></span>

1. <span data-ttu-id="6dce6-153">Nyisson meg egy git terminálablakot, például a git bash eszközt, és `cd` tooa munkakönyvtárát.</span><span class="sxs-lookup"><span data-stu-id="6dce6-153">Open a git terminal window, such as git bash, and `cd` tooa working directory.</span></span>  

2. <span data-ttu-id="6dce6-154">Futtassa a következő parancs tooclone hello minta tárház hello.</span><span class="sxs-lookup"><span data-stu-id="6dce6-154">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-getting-started
    ```

3. <span data-ttu-id="6dce6-155">Ezután nyissa meg a hello megoldásfájlt a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6dce6-155">Then open hello solution file in Visual Studio.</span></span>

## <a name="update-your-connection-string"></a><span data-ttu-id="6dce6-156">A kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="6dce6-156">Update your connection string</span></span>

<span data-ttu-id="6dce6-157">Most lépjen vissza az Azure portál tooget toohello kapcsolati karakterlánc adatainak és hello alkalmazásba másolja.</span><span class="sxs-lookup"><span data-stu-id="6dce6-157">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span>

1. <span data-ttu-id="6dce6-158">A hello [Azure-portálon](http://portal.azure.com/), az Azure Cosmos DB a fiókot, kattintson a bal oldali navigációs hello **kulcsok**, és kattintson a **írható-olvasható kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="6dce6-158">In hello [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in hello left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="6dce6-159">Hello képernyő toocopy hello kapcsolati karakterlánc jobb oldalán hello hello másolási gombok hello app.config fájlba hello következő lépésben fogja használni.</span><span class="sxs-lookup"><span data-stu-id="6dce6-159">You'll use hello copy buttons on hello right side of hello screen toocopy hello connection string into hello app.config file in hello next step.</span></span>

2. <span data-ttu-id="6dce6-160">A Visual Studióban nyissa meg a hello app.config fájlt.</span><span class="sxs-lookup"><span data-stu-id="6dce6-160">In Visual Studio, open hello app.config file.</span></span> 

3. <span data-ttu-id="6dce6-161">Az URI értéket másol a portálról hello (hello Másolás gombra), és teszi hello hello fiók-kulcs az App.config fájlban értékét. Használja az App.config fájlban fióknevet a korábban létrehozott hello fióknevet.</span><span class="sxs-lookup"><span data-stu-id="6dce6-161">Copy your URI value from hello portal (using hello copy button) and make it hello value of hello account-key in app.config. Use hello account name created earlier for account-name in app.config.</span></span>
  
```
<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key;TableEndpoint=https://account-name.documents.azure.com" />
```

> [!NOTE]
> <span data-ttu-id="6dce6-162">toouse toochange hello kapcsolati karakterláncot a kell ezt az alkalmazást az szabványos Azure Table Storage, `app.config file`.</span><span class="sxs-lookup"><span data-stu-id="6dce6-162">toouse this app with standard Azure Table Storage, you need toochange hello connection string in `app.config file`.</span></span> <span data-ttu-id="6dce6-163">Azure Storage elsődleges kulcsként használja tábla fióknevet és kulcsot hello fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="6dce6-163">Use hello account name as Table-account name and key as Azure Storage Primary key.</span></span> <br>
>`<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key;EndpointSuffix=core.windows.net" />`
> 
>

## <a name="build-and-deploy-hello-app"></a><span data-ttu-id="6dce6-164">Hozza létre és hello alkalmazás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="6dce6-164">Build and deploy hello app</span></span>
1. <span data-ttu-id="6dce6-165">A Visual Studióban, kattintson a jobb gombbal a hello projekt **Megoldáskezelőben** majd **NuGet-csomagok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="6dce6-165">In Visual Studio, right-click on hello project in **Solution Explorer** and then click **Manage NuGet Packages**.</span></span> 

2. <span data-ttu-id="6dce6-166">A hello NuGet **Tallózás** mezőbe írja be ***windowsazure.Storage kifejezésre-PremiumTable***.</span><span class="sxs-lookup"><span data-stu-id="6dce6-166">In hello NuGet **Browse** box, type ***WindowsAzure.Storage-PremiumTable***.</span></span> <span data-ttu-id="6dce6-167">Ellenőrizze **előzetes verzióját tartalmazzák**.</span><span class="sxs-lookup"><span data-stu-id="6dce6-167">Check **Include prerelease versions**.</span></span>

3. <span data-ttu-id="6dce6-168">Hello eredmények közül telepítse a hello **windowsazure.Storage kifejezésre-PremiumTable** , és válassza a hello preview build `0.0.1-preview`.</span><span class="sxs-lookup"><span data-stu-id="6dce6-168">From hello results, install hello **WindowsAzure.Storage-PremiumTable** and choose hello preview build `0.0.1-preview`.</span></span> <span data-ttu-id="6dce6-169">Ez a művelet hello Azure Table storage csomag és az összes függőséget telepíti.</span><span class="sxs-lookup"><span data-stu-id="6dce6-169">This action installs hello Azure Table storage package and all dependencies.</span></span>

4. <span data-ttu-id="6dce6-170">Kattintson a CTRL + F5 toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6dce6-170">Click CTRL + F5 toorun hello application.</span></span> 

<span data-ttu-id="6dce6-171">Ezután lépjen vissza tooData Explorer és tekintse meg a lekérdezés, módosítása, és a tábla adatokkal dolgozni.</span><span class="sxs-lookup"><span data-stu-id="6dce6-171">You can now go back tooData Explorer and see query, modify, and work with this table data.</span></span> 

> [!NOTE]
> <span data-ttu-id="6dce6-172">toouse az alkalmazás emulátorral egy Azure Cosmos DB, csak akkor kell toochange hello kapcsolati karakterláncot a `app.config file`.</span><span class="sxs-lookup"><span data-stu-id="6dce6-172">toouse this app with an Azure Cosmos DB Emulator, you just need toochange hello connection string in `app.config file`.</span></span> <span data-ttu-id="6dce6-173">Használja a hello Emulator érték alá.</span><span class="sxs-lookup"><span data-stu-id="6dce6-173">Use hello below value for emulator.</span></span> <br>
>`<add key="StorageConnectionString" value=DefaultEndpointsProtocol=https;AccountName=localhost;AccountKey=<insertkey>==;TableEndpoint=https://localhost -->`
> 
>

## <a name="azure-cosmos-db-capabilities"></a><span data-ttu-id="6dce6-174">Azure Cosmos DB-képességek</span><span class="sxs-lookup"><span data-stu-id="6dce6-174">Azure Cosmos DB capabilities</span></span>
<span data-ttu-id="6dce6-175">Azure Cosmos DB képességeket kínál, amelyek nem érhetők el a hello Azure Table storage API számos támogat.</span><span class="sxs-lookup"><span data-stu-id="6dce6-175">Azure Cosmos DB supports a number of capabilities that are not available in hello Azure Table storage API.</span></span> <span data-ttu-id="6dce6-176">hello új funkciók engedélyezéséhez hello következő `appSettings` konfigurációs értékeket.</span><span class="sxs-lookup"><span data-stu-id="6dce6-176">hello new functionality can be enabled via hello following `appSettings` configuration values.</span></span> <span data-ttu-id="6dce6-177">Bármely új aláírások vagy túlterhelés toohello preview Azure Storage szolgáltatás SDK nem bemutatása volt után.</span><span class="sxs-lookup"><span data-stu-id="6dce6-177">We did not introduce any new signatures or overloads toohello preview Azure Storage SDK.</span></span> <span data-ttu-id="6dce6-178">Ez lehetővé teszi, hogy tooconnect tooboth standard és premium táblák és az egyéb Azure Storage-szolgáltatásokat, mint a blobokat és üzenetsorokat használata.</span><span class="sxs-lookup"><span data-stu-id="6dce6-178">This allows you tooconnect tooboth standard and premium tables, and work with other Azure Storage services like Blobs and Queues.</span></span> 


| <span data-ttu-id="6dce6-179">Kulcs</span><span class="sxs-lookup"><span data-stu-id="6dce6-179">Key</span></span> | <span data-ttu-id="6dce6-180">Leírás</span><span class="sxs-lookup"><span data-stu-id="6dce6-180">Description</span></span> |
| --- | --- |
| <span data-ttu-id="6dce6-181">TableConnectionMode</span><span class="sxs-lookup"><span data-stu-id="6dce6-181">TableConnectionMode</span></span>  | <span data-ttu-id="6dce6-182">Azure Cosmos-adatbázis két csatlakozási módot támogat.</span><span class="sxs-lookup"><span data-stu-id="6dce6-182">Azure Cosmos DB supports two connectivity modes.</span></span> <span data-ttu-id="6dce6-183">A `Gateway` mód, mindig kérések toohello Azure Cosmos DB átjárón, amely továbbítja azt toohello megfelelő adatok partíciókat.</span><span class="sxs-lookup"><span data-stu-id="6dce6-183">In `Gateway` mode, requests are always made toohello Azure Cosmos DB gateway, which forwards it toohello corresponding data partitions.</span></span> <span data-ttu-id="6dce6-184">A `Direct` csatlakozási mód hello ügyfél lekéri a táblák toopartitions hello leképezése, és a kérések közvetlenül az adatok partíciók szemben.</span><span class="sxs-lookup"><span data-stu-id="6dce6-184">In `Direct` connectivity mode, hello client fetches hello mapping of tables toopartitions, and requests are made directly against data partitions.</span></span> <span data-ttu-id="6dce6-185">Ajánlott `Direct`, alapértelmezett hello.</span><span class="sxs-lookup"><span data-stu-id="6dce6-185">We recommend `Direct`, hello default.</span></span>  |
| <span data-ttu-id="6dce6-186">TableConnectionProtocol</span><span class="sxs-lookup"><span data-stu-id="6dce6-186">TableConnectionProtocol</span></span> | <span data-ttu-id="6dce6-187">Az Azure Cosmos DB támogatja két kapcsolat protokoll - `Https` és `Tcp`.</span><span class="sxs-lookup"><span data-stu-id="6dce6-187">Azure Cosmos DB supports two connection protocols - `Https` and `Tcp`.</span></span> <span data-ttu-id="6dce6-188">`Tcp`hello alapértelmezett, és javasolt, mert több egyszerűsített.</span><span class="sxs-lookup"><span data-stu-id="6dce6-188">`Tcp` is hello default, and recommended because it is more lightweight.</span></span> |
| <span data-ttu-id="6dce6-189">TablePreferredLocations</span><span class="sxs-lookup"><span data-stu-id="6dce6-189">TablePreferredLocations</span></span> | <span data-ttu-id="6dce6-190">Előnyben részesített (többhelyű) helyek az olvasási műveletek vesszővel elválasztott listája.</span><span class="sxs-lookup"><span data-stu-id="6dce6-190">Comma-separated list of preferred (multi-homing) locations for reads.</span></span> <span data-ttu-id="6dce6-191">Minden Azure Cosmos DB fiókhoz társítható 1-30 + régiók.</span><span class="sxs-lookup"><span data-stu-id="6dce6-191">Each Azure Cosmos DB account can be associated with 1-30+ regions.</span></span> <span data-ttu-id="6dce6-192">Minden ügyfél példány előnyben részesített hello ahhoz, hogy kis késleltetésű olvasási adhatja meg e régiók egy részét.</span><span class="sxs-lookup"><span data-stu-id="6dce6-192">Each client instance can specify a subset of these regions in hello preferred order for low latency reads.</span></span> <span data-ttu-id="6dce6-193">hello régiók névvel kell ellátni használatával a [megjelenített neveket](https://msdn.microsoft.com/library/azure/gg441293.aspx), például `West US`.</span><span class="sxs-lookup"><span data-stu-id="6dce6-193">hello regions must be named using their [display names](https://msdn.microsoft.com/library/azure/gg441293.aspx), for example, `West US`.</span></span> <span data-ttu-id="6dce6-194">Lásd még: [több homing API-k](tutorial-global-distribution-table.md).</span><span class="sxs-lookup"><span data-stu-id="6dce6-194">Also see [Multi-homing APIs](tutorial-global-distribution-table.md).</span></span>
| <span data-ttu-id="6dce6-195">TableConsistencyLevel</span><span class="sxs-lookup"><span data-stu-id="6dce6-195">TableConsistencyLevel</span></span> | <span data-ttu-id="6dce6-196">Akkor is kompromisszumot közötti késleltetés, konzisztencia- és rendelkezésre állás öt jól meghatározott konzisztenciaszintek közötti választással: `Strong`, `Session`, `Bounded-Staleness`, `ConsistentPrefix`, és `Eventual`.</span><span class="sxs-lookup"><span data-stu-id="6dce6-196">You can trade off between latency, consistency, and availability by choosing between five well-defined consistency levels: `Strong`, `Session`, `Bounded-Staleness`, `ConsistentPrefix`, and `Eventual`.</span></span> <span data-ttu-id="6dce6-197">Alapértelmezett érték a `Session`.</span><span class="sxs-lookup"><span data-stu-id="6dce6-197">Default is `Session`.</span></span> <span data-ttu-id="6dce6-198">hello választott konzisztenciaszint lehetővé teszi egy jelentős teljesítménybeli különbség több területi beállításokat.</span><span class="sxs-lookup"><span data-stu-id="6dce6-198">hello choice of consistency level makes a significant performance difference in multi-region setups.</span></span> <span data-ttu-id="6dce6-199">Lásd: [konzisztenciaszintek](consistency-levels.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="6dce6-199">See [Consistency levels](consistency-levels.md) for details.</span></span> |
| <span data-ttu-id="6dce6-200">TableThroughput</span><span class="sxs-lookup"><span data-stu-id="6dce6-200">TableThroughput</span></span> | <span data-ttu-id="6dce6-201">Fenntartott átviteli sebességet a kérelemegység (RU) másodpercenként kifejezett hello tábla.</span><span class="sxs-lookup"><span data-stu-id="6dce6-201">Reserved throughput for hello table expressed in request units (RU) per second.</span></span> <span data-ttu-id="6dce6-202">Egyetlen tábla RU/mp 100-as egység millióit képes támogatni.</span><span class="sxs-lookup"><span data-stu-id="6dce6-202">Single tables can support 100s-millions of RU/s.</span></span> <span data-ttu-id="6dce6-203">Lásd: [egységek kérelem](request-units.md).</span><span class="sxs-lookup"><span data-stu-id="6dce6-203">See [Request units](request-units.md).</span></span> <span data-ttu-id="6dce6-204">Alapértelmezett érték`400`</span><span class="sxs-lookup"><span data-stu-id="6dce6-204">Default is `400`</span></span> |
| <span data-ttu-id="6dce6-205">TableIndexingPolicy</span><span class="sxs-lookup"><span data-stu-id="6dce6-205">TableIndexingPolicy</span></span> | <span data-ttu-id="6dce6-206">Egységes és automatikus másodlagos indexelése lévő táblák oszlopok</span><span class="sxs-lookup"><span data-stu-id="6dce6-206">Consistent and automatic secondary indexing of all columns within tables</span></span> | <span data-ttu-id="6dce6-207">JSON string indexelő házirend meghatározása megfelelő toohello.</span><span class="sxs-lookup"><span data-stu-id="6dce6-207">JSON string conforming toohello indexing policy specification.</span></span> <span data-ttu-id="6dce6-208">Lásd: [indexelő házirend](indexing-policies.md) toosee hogyan módosíthatja az egyes oszlopok indexelési házirend tooinclude/kizárási.</span><span class="sxs-lookup"><span data-stu-id="6dce6-208">See [Indexing Policy](indexing-policies.md) toosee how you can change indexing policy tooinclude/exclude specific columns.</span></span> | <span data-ttu-id="6dce6-209">Automatikus indexeléshez levő összes tulajdonság (kivonatoló karakterláncok), és a számok tartománya</span><span class="sxs-lookup"><span data-stu-id="6dce6-209">Automatic indexing of all properties (hash for strings, and range for numbers)</span></span> |
| <span data-ttu-id="6dce6-210">TableQueryMaxItemCount</span><span class="sxs-lookup"><span data-stu-id="6dce6-210">TableQueryMaxItemCount</span></span> | <span data-ttu-id="6dce6-211">Konfigurálja a hello tábla lekérdezésenként egyetlen oda-vissza a visszaadott elemek maximális számát.</span><span class="sxs-lookup"><span data-stu-id="6dce6-211">Configure hello maximum number of items returned per table query in a single round trip.</span></span> <span data-ttu-id="6dce6-212">Alapértelmezett érték a `-1`, ami lehetővé teszi, hogy Azure Cosmos DB hello értéket futásidőben dinamikus meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="6dce6-212">Default is `-1`, which lets Azure Cosmos DB dynamically determine hello value at runtime.</span></span> |
| <span data-ttu-id="6dce6-213">TableQueryEnableScan</span><span class="sxs-lookup"><span data-stu-id="6dce6-213">TableQueryEnableScan</span></span> | <span data-ttu-id="6dce6-214">Hello lekérdezés hello index bármely szűrő nem használható, ha majd, mégis futtatni a vizsgálat keresztül.</span><span class="sxs-lookup"><span data-stu-id="6dce6-214">If hello query cannot use hello index for any filter, then run it anyway via a scan.</span></span> <span data-ttu-id="6dce6-215">Alapértelmezett érték a `false`.</span><span class="sxs-lookup"><span data-stu-id="6dce6-215">Default is `false`.</span></span>|
| <span data-ttu-id="6dce6-216">TableQueryMaxDegreeOfParallelism</span><span class="sxs-lookup"><span data-stu-id="6dce6-216">TableQueryMaxDegreeOfParallelism</span></span> | <span data-ttu-id="6dce6-217">a kereszt-partíció lekérdezés végrehajtási párhuzamossági hello fok.</span><span class="sxs-lookup"><span data-stu-id="6dce6-217">hello degree of parallelism for execution of a cross-partition query.</span></span> <span data-ttu-id="6dce6-218">`0`a nem lehívását, soros `1` lehívását a soros, és nagyobb értékek hello növelési párhuzamossági.</span><span class="sxs-lookup"><span data-stu-id="6dce6-218">`0` is serial with no pre-fetching, `1` is serial with pre-fetching, and higher values increase hello rate of parallelism.</span></span> <span data-ttu-id="6dce6-219">Alapértelmezett érték a `-1`, ami lehetővé teszi, hogy Azure Cosmos DB hello értéket futásidőben dinamikus meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="6dce6-219">Default is `-1`, which lets Azure Cosmos DB dynamically determine hello value at runtime.</span></span> |

<span data-ttu-id="6dce6-220">toochange hello alapértelmezett érték, nyissa meg hello `app.config` fájlt a Visual Studio megoldáskezelőjében.</span><span class="sxs-lookup"><span data-stu-id="6dce6-220">toochange hello default value, open hello `app.config` file from Solution Explorer in Visual Studio.</span></span> <span data-ttu-id="6dce6-221">Adja hozzá a hello hello tartalmát `<appSettings>` elem alább látható.</span><span class="sxs-lookup"><span data-stu-id="6dce6-221">Add hello contents of hello `<appSettings>` element shown below.</span></span> <span data-ttu-id="6dce6-222">Cserélje le `account-name` hello nevet a tárfiók, és `account-key` a fiók hozzáférési kulccsal.</span><span class="sxs-lookup"><span data-stu-id="6dce6-222">Replace `account-name` with hello name of your storage account, and `account-key` with your account access key.</span></span> 

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

<span data-ttu-id="6dce6-223">Most Meggyőződünk arról, mi történik a hello app gyors áttekintése.</span><span class="sxs-lookup"><span data-stu-id="6dce6-223">Let's make a quick review of what's happening in hello app.</span></span> <span data-ttu-id="6dce6-224">Nyissa meg hello `Program.cs` fájlt, és találja, hogy ezek a sorok, a kód hello tábla erőforrások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6dce6-224">Open hello `Program.cs` file and you find that these lines of code create hello Table resources.</span></span> 

## <a name="create-hello-table-client"></a><span data-ttu-id="6dce6-225">Hello tábla ügyfél létrehozása</span><span class="sxs-lookup"><span data-stu-id="6dce6-225">Create hello table client</span></span>
<span data-ttu-id="6dce6-226">Ön inicializálni egy `CloudTableClient` tooconnect toohello tábla fiók.</span><span class="sxs-lookup"><span data-stu-id="6dce6-226">You initialize a `CloudTableClient` tooconnect toohello table account.</span></span>

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```
<span data-ttu-id="6dce6-227">Ez az ügyfél inicializálása hello segítségével `TableConnectionMode`, `TableConnectionProtocol`, `TableConsistencyLevel`, és `TablePreferredLocations` konfigurációs értékei, ha meg van adva a hello Alkalmazásbeállítások.</span><span class="sxs-lookup"><span data-stu-id="6dce6-227">This client is initialized using hello `TableConnectionMode`, `TableConnectionProtocol`, `TableConsistencyLevel`, and `TablePreferredLocations` configuration values if specified in hello app settings.</span></span>
    
## <a name="create-a-table"></a><span data-ttu-id="6dce6-228">Tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="6dce6-228">Create a table</span></span>
<span data-ttu-id="6dce6-229">Ezután létrehozhat egy tábla használatával `CloudTable`.</span><span class="sxs-lookup"><span data-stu-id="6dce6-229">Then, you create a table using `CloudTable`.</span></span> <span data-ttu-id="6dce6-230">Azure Cosmos DB táblák egymástól függetlenül is méretezhető tárolási és átviteli sebesség szempontjából, és particionálás kezeli automatikusan hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="6dce6-230">Tables in Azure Cosmos DB can scale independently in terms of storage and throughput, and partitioning is handled automatically by hello service.</span></span> <span data-ttu-id="6dce6-231">Azure Cosmos-adatbázis a rögzített méretű és korlátlan táblák is támogatja.</span><span class="sxs-lookup"><span data-stu-id="6dce6-231">Azure Cosmos DB supports both fixed size and unlimited tables.</span></span> <span data-ttu-id="6dce6-232">Lásd: [Azure Cosmos DB a particionálás](partition-data.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="6dce6-232">See [Partitioning in Azure Cosmos DB](partition-data.md) for details.</span></span> 

```csharp
CloudTable table = tableClient.GetTableReference("people");

table.CreateIfNotExists();
```

<span data-ttu-id="6dce6-233">Nincs a táblák létrehozását egy fontos különbséggel.</span><span class="sxs-lookup"><span data-stu-id="6dce6-233">There is an important difference in how tables are created.</span></span> <span data-ttu-id="6dce6-234">Cosmos. Azure-adatbázis fenntartja a teljesítményt, eltérően az Azure storage fogyasztás alapján modell az egyes tranzakciókra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="6dce6-234">Azure Cosmos DB reserves throughput, unlike Azure storage's consumption-based model for transactions.</span></span> <span data-ttu-id="6dce6-235">hello foglalás modellnek két nagy előnye van:</span><span class="sxs-lookup"><span data-stu-id="6dce6-235">hello reservation model has two key benefits:</span></span>

* <span data-ttu-id="6dce6-236">Az átviteli sebesség dedikált/foglalt, így Ön soha nem get szabályozva a kérelmek aránya vagy az alatt a létesített átviteli sebesség esetén</span><span class="sxs-lookup"><span data-stu-id="6dce6-236">Your throughput is dedicated/reserved, so you never get throttled if your request rate is at or below your provisioned throughput</span></span>
* <span data-ttu-id="6dce6-237">hello foglalás modell több [költséghatékony a teljesítmény-gyakori feladatok](key-value-store-cost.md)</span><span class="sxs-lookup"><span data-stu-id="6dce6-237">hello reservation model is more [cost effective for throughput-heavy workloads](key-value-store-cost.md)</span></span>

<span data-ttu-id="6dce6-238">Hello alapértelmezett átviteli konfigurálhatja a hello beállítás konfigurálásával `TableThroughput` RU (kérelemegység) másodpercenként tekintetében.</span><span class="sxs-lookup"><span data-stu-id="6dce6-238">You can configure hello default throughput by configuring hello setting for `TableThroughput` in terms of RU (request units) per second.</span></span> 

<span data-ttu-id="6dce6-239">Egy 1 KB-os entitás olvasási van normalizált 1 RU és egyéb műveletekre RU érték a Processzor, memória és IOPS fogyasztás alapján rögzített normalizált tooa.</span><span class="sxs-lookup"><span data-stu-id="6dce6-239">A read of a 1-KB entity is normalized as 1 RU, and other operations are normalized tooa fixed RU value based on their CPU, memory, and IOPS consumption.</span></span> <span data-ttu-id="6dce6-240">További információ [Azure Cosmos DB egység kérelem](request-units.md).</span><span class="sxs-lookup"><span data-stu-id="6dce6-240">Learn more about [Request units in Azure Cosmos DB](request-units.md).</span></span>

> [!NOTE]
> <span data-ttu-id="6dce6-241">A Table storage SDK jelenleg nem támogatja a módosítása átviteli, amíg hello átviteli azonnal bármikor hello Azure-portálon vagy az Azure parancssori felület használatával módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="6dce6-241">While Table storage SDK does not currently support modifying throughput, you can change hello throughput instantaneously at any time using hello Azure portal or Azure CLI.</span></span>

<span data-ttu-id="6dce6-242">Ezután azt hello egyszerű olvasható útmutatót, és írási (CRUD) műveletet hello Azure Table storage SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="6dce6-242">Next, we walk through hello simple read and write (CRUD) operations using hello Azure Table storage SDK.</span></span> <span data-ttu-id="6dce6-243">Ez az oktatóanyag azt mutatja be, előre jelezhető alacsony egyjegyű ezredmásodperces késések és Azure Cosmos DB által nyújtott gyors lekérdezéseket.</span><span class="sxs-lookup"><span data-stu-id="6dce6-243">This tutorial demonstrates predictable low single-digit millisecond latencies and fast queries provided by Azure Cosmos DB.</span></span>

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="6dce6-244">Egy entitás tooa táblázat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6dce6-244">Add an entity tooa table</span></span>
<span data-ttu-id="6dce6-245">Az Azure Table storage entitások terjessze ki a hello `TableEntity` osztályhoz, és rendelkeznie kell `PartitionKey` és `RowKey` tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="6dce6-245">Entities in Azure Table storage extend from hello `TableEntity` class and must have `PartitionKey` and `RowKey` properties.</span></span> <span data-ttu-id="6dce6-246">Íme egy minta definíciója egy ügyfélentitást.</span><span class="sxs-lookup"><span data-stu-id="6dce6-246">Here's a sample definition for a customer entity.</span></span>

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

<span data-ttu-id="6dce6-247">a következő kódrészletet hello bemutatja, hogyan tooinsert egy entitás a hello az Azure storage SDK.</span><span class="sxs-lookup"><span data-stu-id="6dce6-247">hello following snippet shows how tooinsert an entity with hello Azure storage SDK.</span></span> <span data-ttu-id="6dce6-248">Azure Cosmos DB végzi a kis késleltetésű bármilyen léptékben garantált hello world között.</span><span class="sxs-lookup"><span data-stu-id="6dce6-248">Azure Cosmos DB is designed for guaranteed low latency at any scale, across hello world.</span></span>

<span data-ttu-id="6dce6-249">Írási műveletek befejezése < 15 ms p99 és a futó alkalmazások p50 ~ 6 ms hello és hello Azure Cosmos DB fiók ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="6dce6-249">Writes complete <15 ms at p99 and ~6 ms at p50 for applications running in hello same region as hello Azure Cosmos DB account.</span></span> <span data-ttu-id="6dce6-250">És az ezen időtartam hello tényt, hogy beírja a fiókok ismernek hátsó toohello ügyfél csak azokat a rendszer szinkron módon replikálja, tartósan véglegesítve lett, és minden tartalom indexelt után.</span><span class="sxs-lookup"><span data-stu-id="6dce6-250">And this duration accounts for hello fact that writes are acknowledged back toohello client only after they are synchronously replicated, durably committed, and all content is indexed.</span></span>

<span data-ttu-id="6dce6-251">Tábla API for Azure Cosmos DB hello jelenleg előzetes verzióban érhető.</span><span class="sxs-lookup"><span data-stu-id="6dce6-251">hello Table API for Azure Cosmos DB is in preview.</span></span> <span data-ttu-id="6dce6-252">Általánosan rendelkezésre álló hello p99 késés garanciák SLA-k, például a más Azure Cosmos DB API-k által támogatott.</span><span class="sxs-lookup"><span data-stu-id="6dce6-252">At general availability, hello p99 latency guarantees are backed by SLAs like other Azure Cosmos DB APIs.</span></span> 

```csharp
// Create a new customer entity.
CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
customer1.Email = "Walter@contoso.com";
customer1.PhoneNumber = "425-555-0101";

// Create hello TableOperation object that inserts hello customer entity.
TableOperation insertOperation = TableOperation.Insert(customer1);

// Execute hello insert operation.
table.Execute(insertOperation);
```

## <a name="insert-a-batch-of-entities"></a><span data-ttu-id="6dce6-253">Entitásköteg beszúrása</span><span class="sxs-lookup"><span data-stu-id="6dce6-253">Insert a batch of entities</span></span>
<span data-ttu-id="6dce6-254">Az Azure Table storage támogatja egy kötegelt művelet API-t, amely lehetővé teszi frissítés, törlés, kombinálhatja, és a Beszúrás hello egyetlen kötegművelettel.</span><span class="sxs-lookup"><span data-stu-id="6dce6-254">Azure Table storage supports a batch operation API, that lets you combine updates, deletes, and inserts in hello same single batch operation.</span></span> <span data-ttu-id="6dce6-255">Azure Cosmos-adatbázis nem lehet hello korlátozások némelyike hello kötegelt API-t az Azure Table storage.</span><span class="sxs-lookup"><span data-stu-id="6dce6-255">Azure Cosmos DB does not have some of hello limitations on hello batch API as Azure Table storage.</span></span> <span data-ttu-id="6dce6-256">Például egy kötegelt belül többszöri beolvasás végezheti el, több írások toohello hajthat végre egy kötegelt belül ugyanaz az entitás és kötegenként 100 műveletek korlátozva van.</span><span class="sxs-lookup"><span data-stu-id="6dce6-256">For example, you can perform multiple reads within a batch, you can perform multiple writes toohello same entity within a batch, and there is no limit on 100 operations per batch.</span></span> 

```csharp
// Create hello batch operation.
TableBatchOperation batchOperation = new TableBatchOperation();

// Create a customer entity and add it toohello table.
CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
customer1.Email = "Jeff@contoso.com";
customer1.PhoneNumber = "425-555-0104";

// Create another customer entity and add it toohello table.
CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
customer2.Email = "Ben@contoso.com";
customer2.PhoneNumber = "425-555-0102";

// Add both customer entities toohello batch insert operation.
batchOperation.Insert(customer1);
batchOperation.Insert(customer2);

// Execute hello batch operation.
table.ExecuteBatch(batchOperation);
```
## <a name="retrieve-a-single-entity"></a><span data-ttu-id="6dce6-257">Egyetlen entitás lekérdezése</span><span class="sxs-lookup"><span data-stu-id="6dce6-257">Retrieve a single entity</span></span>
<span data-ttu-id="6dce6-258">(Lekérdezi) lekérdezi a teljes Azure Cosmos DB < 10 ms p99 és ~ 1, a p50 ms hello azonos Azure-régiót.</span><span class="sxs-lookup"><span data-stu-id="6dce6-258">Retrieves (GETs) in Azure Cosmos DB complete <10 ms at p99 and ~1 ms at p50 in hello same Azure region.</span></span> <span data-ttu-id="6dce6-259">Tetszőleges számú régiók tooyour fiók hozzáadása a kis késleltetésű olvasása, és központi telepítése a helyi régió ("többhelyű") az alkalmazások tooread beállításával `TablePreferredLocations`.</span><span class="sxs-lookup"><span data-stu-id="6dce6-259">You can add as many regions tooyour account for low latency reads, and deploy applications tooread from their local region ("multi-homed") by setting `TablePreferredLocations`.</span></span> 

<span data-ttu-id="6dce6-260">A következő kódrészletet hello segítségével egyetlen entitás le:</span><span class="sxs-lookup"><span data-stu-id="6dce6-260">You can retrieve a single entity using hello following snippet:</span></span>

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute hello retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
> [!TIP]
> <span data-ttu-id="6dce6-261">További tudnivalók a többhelyű API-k [fejlesztés több régióba az](tutorial-global-distribution-table.md)</span><span class="sxs-lookup"><span data-stu-id="6dce6-261">Learn about multi-homing APIs at [Developing with multiple regions](tutorial-global-distribution-table.md)</span></span>
>

## <a name="query-entities-using-automatic-secondary-indexes"></a><span data-ttu-id="6dce6-262">Lekérdezés entitások automatikus másodlagos indexek használata</span><span class="sxs-lookup"><span data-stu-id="6dce6-262">Query entities using automatic secondary indexes</span></span>
<span data-ttu-id="6dce6-263">Táblák hello lekérdezhetők `TableQuery` osztály.</span><span class="sxs-lookup"><span data-stu-id="6dce6-263">Tables can be queried using hello `TableQuery` class.</span></span> <span data-ttu-id="6dce6-264">Azure Cosmos-adatbázis egy adatbázis írási optimalizált motor, amely automatikusan elvégzi a belül a táblázat összes oszlopa van.</span><span class="sxs-lookup"><span data-stu-id="6dce6-264">Azure Cosmos DB has a write-optimized database engine that automatically indexes all columns within your table.</span></span> <span data-ttu-id="6dce6-265">Az Azure Cosmos Adatbázisba indexelő független tooschema.</span><span class="sxs-lookup"><span data-stu-id="6dce6-265">Indexing in Azure Cosmos DB is agnostic tooschema.</span></span> <span data-ttu-id="6dce6-266">Ezért még akkor is, ha a séma sorok különböznek, vagy hello séma fejlődésének adott idő alatt, akkor automatikusan indexelt.</span><span class="sxs-lookup"><span data-stu-id="6dce6-266">Therefore, even if your schema is different between rows, or if hello schema evolves over time, it is automatically indexed.</span></span> <span data-ttu-id="6dce6-267">Mivel Azure Cosmos DB támogatja az automatikus másodlagos indexek, a lekérdezések egyik tulajdonságnak sem használható hello index, és hatékonyan szolgáltatható.</span><span class="sxs-lookup"><span data-stu-id="6dce6-267">Since Azure Cosmos DB supports automatic secondary indexes, queries against any property can use hello index and be served efficiently.</span></span>

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

<span data-ttu-id="6dce6-268">A képen Azure Cosmos DB támogatja hello azonos Azure Table storage a tábla API hello funkciókat lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="6dce6-268">In preview, Azure Cosmos DB supports hello same query functionality as Azure Table storage for hello Table API.</span></span> <span data-ttu-id="6dce6-269">Azure Cosmos-adatbázis is támogatja a rendezést, összesítések, a földrajzi lekérdezést, a hierarchia és a számos különféle beépített funkciók.</span><span class="sxs-lookup"><span data-stu-id="6dce6-269">Azure Cosmos DB also supports sorting, aggregates, geospatial query, hierarchy, and a wide range of built-in functions.</span></span> <span data-ttu-id="6dce6-270">Tábla API hello jövőbeli szolgáltatásfrissítés hello további funkciók lesznek közzétéve.</span><span class="sxs-lookup"><span data-stu-id="6dce6-270">hello additional functionality will be provided in hello Table API in a future service update.</span></span> <span data-ttu-id="6dce6-271">Lásd: [Azure Cosmos adatbázis-lekérdezés](documentdb-sql-query.md) ezeket a képességeket áttekintését.</span><span class="sxs-lookup"><span data-stu-id="6dce6-271">See [Azure Cosmos DB query](documentdb-sql-query.md) for an overview of these capabilities.</span></span> 

## <a name="replace-an-entity"></a><span data-ttu-id="6dce6-272">Entitás cseréje</span><span class="sxs-lookup"><span data-stu-id="6dce6-272">Replace an entity</span></span>
<span data-ttu-id="6dce6-273">tooupdate egy entitás lekéréséhez hello Table szolgáltatásból, módosítsa a hello forrásentitás-objektum, és majd hello módosítások mentése toohello Table szolgáltatás biztonsági.</span><span class="sxs-lookup"><span data-stu-id="6dce6-273">tooupdate an entity, retrieve it from hello Table service, modify hello entity object, and then save hello changes back toohello Table service.</span></span> <span data-ttu-id="6dce6-274">hello alábbira vált egy meglévő ügyfél telefonszámát.</span><span class="sxs-lookup"><span data-stu-id="6dce6-274">hello following code changes an existing customer's phone number.</span></span> 

```csharp
TableOperation updateOperation = TableOperation.Replace(updateEntity);
table.Execute(updateOperation);
```
<span data-ttu-id="6dce6-275">Hasonló módon végezheti el `InsertOrMerge` vagy `Merge` műveletek.</span><span class="sxs-lookup"><span data-stu-id="6dce6-275">Similarly, you can perform `InsertOrMerge` or `Merge` operations.</span></span>  

## <a name="delete-an-entity"></a><span data-ttu-id="6dce6-276">Entitás törlése</span><span class="sxs-lookup"><span data-stu-id="6dce6-276">Delete an entity</span></span>
<span data-ttu-id="6dce6-277">Egyszerűen törölheti entitás hello segítségével beolvasása után egy entitás frissítésénél bemutatott minta.</span><span class="sxs-lookup"><span data-stu-id="6dce6-277">You can easily delete an entity after you have retrieved it by using hello same pattern shown for updating an entity.</span></span> <span data-ttu-id="6dce6-278">a következő kód hello kéri le, és töröl egy ügyfélentitást.</span><span class="sxs-lookup"><span data-stu-id="6dce6-278">hello following code retrieves and deletes a customer entity.</span></span>

```csharp
TableOperation deleteOperation = TableOperation.Delete(deleteEntity);
table.Execute(deleteOperation);
```

## <a name="delete-a-table"></a><span data-ttu-id="6dce6-279">Tábla törlése</span><span class="sxs-lookup"><span data-stu-id="6dce6-279">Delete a table</span></span>
<span data-ttu-id="6dce6-280">Végezetül alábbi kódpéldát hello törölhető egy tábla a tárfiókból.</span><span class="sxs-lookup"><span data-stu-id="6dce6-280">Finally, hello following code example deletes a table from a storage account.</span></span> <span data-ttu-id="6dce6-281">Töröl, és hozza létre újból az azonnali Azure Cosmos DB tartalmazó tábla.</span><span class="sxs-lookup"><span data-stu-id="6dce6-281">You can delete and recreate a table immediately with Azure Cosmos DB.</span></span>

```csharp
CloudTable table = tableClient.GetTableReference("people");
table.DeleteIfExists();
```

## <a name="clean-up-resources"></a><span data-ttu-id="6dce6-282">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="6dce6-282">Clean up resources</span></span> 

<span data-ttu-id="6dce6-283">Toocontinue toouse az alkalmazás nem fog, ha használja a következő lépéseket toodelete hello Azure-portálon az oktatóprogram során létrehozott összes erőforrás hello.</span><span class="sxs-lookup"><span data-stu-id="6dce6-283">If you're not going toocontinue toouse this app, use hello following steps toodelete all resources created by this tutorial in hello Azure portal.</span></span>   

1. <span data-ttu-id="6dce6-284">A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson a létrehozott hello erőforrás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="6dce6-284">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you created.</span></span>  
2. <span data-ttu-id="6dce6-285">Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="6dce6-285">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="6dce6-286">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6dce6-286">Next steps</span></span>

<span data-ttu-id="6dce6-287">Az oktatóanyag azt kezelt indításának a tábla API hello Azure Cosmos DB használatával tooget, és hello következő ezt:</span><span class="sxs-lookup"><span data-stu-id="6dce6-287">In this tutorial, we covered how tooget started using Azure Cosmos DB with hello Table API, and you've done hello following:</span></span> 

> [!div class="checklist"] 
> * <span data-ttu-id="6dce6-288">Egy Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="6dce6-288">Created an Azure Cosmos DB account</span></span> 
> * <span data-ttu-id="6dce6-289">Engedélyezett funkciókat hello app.config fájlban</span><span class="sxs-lookup"><span data-stu-id="6dce6-289">Enabled functionality in hello app.config file</span></span> 
> * <span data-ttu-id="6dce6-290">Táblázat létrehozása</span><span class="sxs-lookup"><span data-stu-id="6dce6-290">Created a table</span></span> 
> * <span data-ttu-id="6dce6-291">Egy entitás tooa tábla hozzáadva</span><span class="sxs-lookup"><span data-stu-id="6dce6-291">Added an entity tooa table</span></span> 
> * <span data-ttu-id="6dce6-292">Szúrja be egy teljes entitásköteget</span><span class="sxs-lookup"><span data-stu-id="6dce6-292">Inserted a batch of entities</span></span> 
> * <span data-ttu-id="6dce6-293">Egyetlen entitás lekérése</span><span class="sxs-lookup"><span data-stu-id="6dce6-293">Retrieved a single entity</span></span> 
> * <span data-ttu-id="6dce6-294">Automatikus másodlagos indexek használatával lekérdezett entitásokat</span><span class="sxs-lookup"><span data-stu-id="6dce6-294">Queried entities using automatic secondary indexes</span></span> 
> * <span data-ttu-id="6dce6-295">Entitás cseréje</span><span class="sxs-lookup"><span data-stu-id="6dce6-295">Replaced an entity</span></span> 
> * <span data-ttu-id="6dce6-296">Egy entitás törlése</span><span class="sxs-lookup"><span data-stu-id="6dce6-296">Deleted an entity</span></span> 
> * <span data-ttu-id="6dce6-297">Tábla törlése</span><span class="sxs-lookup"><span data-stu-id="6dce6-297">Deleted a table</span></span>  

<span data-ttu-id="6dce6-298">Ezután folytassa a következő oktatóanyag toohello és tudhat meg többet a tábla adatainak lekérdezésekor.</span><span class="sxs-lookup"><span data-stu-id="6dce6-298">You can now proceed toohello next tutorial and learn more about querying table data.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="6dce6-299">Tábla API hello lekérdezése</span><span class="sxs-lookup"><span data-stu-id="6dce6-299">Query with hello Table API</span></span>](tutorial-query-table.md)
