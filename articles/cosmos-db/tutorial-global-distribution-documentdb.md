---
title: "aaaAzure Cosmos DB globális terjesztési oktatóprogram a DocumentDB az API-hoz |} Microsoft Docs"
description: "Ismerje meg, hogyan hello DocumentDB API toosetup Azure Cosmos DB globális terjesztési használatával."
services: cosmos-db
keywords: "globális terjesztési, a documentdb"
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: a1d5f01faa62407fbbc9c078ef4a9589a1a29219
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosetup-azure-cosmos-db-global-distribution-using-hello-documentdb-api"></a><span data-ttu-id="2ac2a-104">Hogyan toosetup Azure Cosmos DB globális terjesztési használatával hello DocumentDB API</span><span class="sxs-lookup"><span data-stu-id="2ac2a-104">How toosetup Azure Cosmos DB global distribution using hello DocumentDB API</span></span>

<span data-ttu-id="2ac2a-105">Ez a cikk megmutatjuk, hogyan toouse hello Azure portál toosetup Azure Cosmos DB globális terjesztési, és csatlakoztassa a hello DocumentDB API használatával.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-105">In this article, we show how toouse hello Azure portal toosetup Azure Cosmos DB global distribution and then connect using hello DocumentDB API.</span></span>

<span data-ttu-id="2ac2a-106">Ez a cikk ismerteti a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="2ac2a-106">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="2ac2a-107">Konfigurálja a globális terjesztési hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="2ac2a-107">Configure global distribution using hello Azure portal</span></span>
> * <span data-ttu-id="2ac2a-108">Globális terjesztési használatával hello konfigurálása [DocumentDB API-k](documentdb-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="2ac2a-108">Configure global distribution using hello [DocumentDB APIs](documentdb-introduction.md)</span></span>

<a id="portal"></a>
[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-tooa-preferred-region-using-hello-documentdb-api"></a><span data-ttu-id="2ac2a-109">Csatlakozás előnyben részesített régióba tooa hello DocumentDB API</span><span class="sxs-lookup"><span data-stu-id="2ac2a-109">Connecting tooa preferred region using hello DocumentDB API</span></span>

<span data-ttu-id="2ac2a-110">A rendelés tootake előnyeit [globális terjesztési](distribute-data-globally.md), ügyfélalkalmazások hello rendezett használt tooperform dokumentum műveletek régiók toobe preferencia listája adhat meg.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-110">In order tootake advantage of [global distribution](distribute-data-globally.md), client applications can specify hello ordered preference list of regions toobe used tooperform document operations.</span></span> <span data-ttu-id="2ac2a-111">Ezt megteheti hello kapcsolat házirend beállításával.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-111">This can be done by setting hello connection policy.</span></span> <span data-ttu-id="2ac2a-112">Hello Azure Cosmos DB fiók konfigurációja alapján, aktuális területi rendelkezésre állási és hello preferencia lista megadva, a legtöbb optimális végpont választja ki a DocumentDB SDK tooperform írási és olvasási műveletek hello hello.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-112">Based on hello Azure Cosmos DB account configuration, current regional availability and hello preference list specified, hello most optimal endpoint will be chosen by hello DocumentDB SDK tooperform write and read operations.</span></span>

<span data-ttu-id="2ac2a-113">Ez a beállítás lista inicializálása közben hello DocumentDB SDK-k használatával van megadva.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-113">This preference list is specified when initializing a connection using hello DocumentDB SDKs.</span></span> <span data-ttu-id="2ac2a-114">hello SDK-k elfogadása "PreferredLocations" nem kötelező paraméter, amely egy Azure-régiók rendezett listáját.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-114">hello SDKs accept an optional parameter "PreferredLocations" that is an ordered list of Azure regions.</span></span>

<span data-ttu-id="2ac2a-115">hello SDK automatikusan elküld minden írások toohello aktuális írási terület.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-115">hello SDK will automatically send all writes toohello current write region.</span></span>

<span data-ttu-id="2ac2a-116">Az összes olvasási küld toohello első rendelkezésre álló terület hello PreferredLocations listában.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-116">All reads will be sent toohello first available region in hello PreferredLocations list.</span></span> <span data-ttu-id="2ac2a-117">Hello kérelem sikertelen lesz, ha hello ügyfél le hello lista toohello következő terület sikertelen, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-117">If hello request fails, hello client will fail down hello list toohello next region, and so on.</span></span>

<span data-ttu-id="2ac2a-118">hello SDK-k csak kísérli meg a hello régió van megadva a PreferredLocations tooread.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-118">hello SDKs will only attempt tooread from hello regions specified in PreferredLocations.</span></span> <span data-ttu-id="2ac2a-119">Így például ha hello adatbázisfiók három régiókban, de hello ügyfél csak határozza meg két hello nem írási régiók PreferredLocations, majd nincs olvasási szolgáltató hello írási régió, még akkor is, a feladatátvétel hello eset kívül.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-119">So, for example, if hello Database Account is available in three regions, but hello client only specifies two of hello non-write regions for PreferredLocations, then no reads will be served out of hello write region, even in hello case of failover.</span></span>

<span data-ttu-id="2ac2a-120">hello alkalmazás hello aktuális írási végpont ellenőrizheti és olvassa el a végpont által hello SDK WriteEndpoint és ReadEndpoint, elérhető, a SDK 1.8-as verzióját és az újabb ellenőrzési két tulajdonság által választott.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-120">hello application can verify hello current write endpoint and read endpoint chosen by hello SDK by checking two properties, WriteEndpoint and ReadEndpoint, available in SDK version 1.8 and above.</span></span>

<span data-ttu-id="2ac2a-121">Hello PreferredLocations tulajdonság nincs beállítva, ha minden kérésnél hello aktuális írási régióban kell kézbesíteni.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-121">If hello PreferredLocations property is not set, all requests will be served from hello current write region.</span></span>

## <a name="net-sdk"></a><span data-ttu-id="2ac2a-122">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="2ac2a-122">.NET SDK</span></span>
<span data-ttu-id="2ac2a-123">hello SDK kód módosítások nélkül használható.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-123">hello SDK can be used without any code changes.</span></span> <span data-ttu-id="2ac2a-124">Ebben az esetben hello SDK automatikusan számáról mindkét olvasási és az toohello aktuális írási területen írja.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-124">In this case, hello SDK automatically directs both reads and writes toohello current write region.</span></span>

<span data-ttu-id="2ac2a-125">1,8 és hello .NET SDK újabb verziójában hello ConnectionPolicy paramétere hello DocumentClient konstruktor hívása Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations tulajdonsággal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-125">In version 1.8 and later of hello .NET SDK, hello ConnectionPolicy parameter for hello DocumentClient constructor has a property called Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations.</span></span> <span data-ttu-id="2ac2a-126">Ez a tulajdonság nem gyűjtemény típusú `<string>` és tartalmaznia kell a régió neveinek listáját.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-126">This property is of type Collection `<string>` and should contain a list of region names.</span></span> <span data-ttu-id="2ac2a-127">hello karakterlánc-értékek vannak formázva a hello hello régió neve oszloponként [Azure-régiókat] [ regions] lap, szóközök nélkül előtt vagy után hello először, és az utolsó karakter rendre.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-127">hello string values are formatted per hello Region Name column on hello [Azure Regions][regions] page, with no spaces before or after hello first and last character respectively.</span></span>

<span data-ttu-id="2ac2a-128">hello aktuális írási és olvasási végpontok találhatók DocumentClient.WriteEndpoint és DocumentClient.ReadEndpoint kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-128">hello current write and read endpoints are available in DocumentClient.WriteEndpoint and DocumentClient.ReadEndpoint respectively.</span></span>

> [!NOTE]
> <span data-ttu-id="2ac2a-129">hello URL-címek hello végpontok hosszú élettartamú állandóként nem tekinthető.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-129">hello URLs for hello endpoints should not be considered as long-lived constants.</span></span> <span data-ttu-id="2ac2a-130">hello szolgáltatás ezek bármikor előfordulhat, hogy frissíteni.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-130">hello service may update these at any point.</span></span> <span data-ttu-id="2ac2a-131">hello SDK automatikusan kezeli ezt a módosítást.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-131">hello SDK handles this change automatically.</span></span>
>
>

```csharp
// Getting endpoints from application settings or other configuration location
Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
string accountKey = Properties.Settings.Default.GlobalDatabaseKey;
  
ConnectionPolicy connectionPolicy = new ConnectionPolicy();

//Setting read region selection preference
connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

// initialize connection
DocumentClient docClient = new DocumentClient(
    accountEndPoint,
    accountKey,
    connectionPolicy);

// connect tooDocDB
await docClient.OpenAsync().ConfigureAwait(false);
```

## <a name="nodejs-javascript-and-python-sdks"></a><span data-ttu-id="2ac2a-132">NodeJS és a Python SDK-k</span><span class="sxs-lookup"><span data-stu-id="2ac2a-132">NodeJS, JavaScript, and Python SDKs</span></span>
<span data-ttu-id="2ac2a-133">hello SDK kód módosítások nélkül használható.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-133">hello SDK can be used without any code changes.</span></span> <span data-ttu-id="2ac2a-134">Ebben az esetben SDK automatikusan átirányítja a hello is beolvassa és toohello aktuális írási terület.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-134">In this case, hello SDK will automatically direct both reads and writes toohello current write region.</span></span>

<span data-ttu-id="2ac2a-135">Verzió 1,8, és később a minden SDK hello ConnectionPolicy paraméterében hello DocumentClient konstruktor DocumentClient.ConnectionPolicy.PreferredLocations nevű új tulajdonsággal.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-135">In version 1.8 and later of each SDK, hello ConnectionPolicy parameter for hello DocumentClient constructor a new property called DocumentClient.ConnectionPolicy.PreferredLocations.</span></span> <span data-ttu-id="2ac2a-136">A paraméter karakterláncok tömbje, amely régió neveinek listáját.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-136">This is parameter is an array of strings that takes a list of region names.</span></span> <span data-ttu-id="2ac2a-137">a hello hello régió neve oszloponként formázott hello nevek [Azure-régiókat] [ regions] lap.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-137">hello names are formatted per hello Region Name column in hello [Azure Regions][regions] page.</span></span> <span data-ttu-id="2ac2a-138">Használhatja az előre megadott hello állandók hello AzureDocuments.Regions kényelmi objektumban</span><span class="sxs-lookup"><span data-stu-id="2ac2a-138">You can also use hello predefined constants in hello convenience object AzureDocuments.Regions</span></span>

<span data-ttu-id="2ac2a-139">hello aktuális írási és olvasási végpontok találhatók DocumentClient.getWriteEndpoint és DocumentClient.getReadEndpoint kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-139">hello current write and read endpoints are available in DocumentClient.getWriteEndpoint and DocumentClient.getReadEndpoint respectively.</span></span>

> [!NOTE]
> <span data-ttu-id="2ac2a-140">hello URL-címek hello végpontok hosszú élettartamú állandóként nem tekinthető.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-140">hello URLs for hello endpoints should not be considered as long-lived constants.</span></span> <span data-ttu-id="2ac2a-141">hello szolgáltatás ezek bármikor előfordulhat, hogy frissíteni.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-141">hello service may update these at any point.</span></span> <span data-ttu-id="2ac2a-142">hello SDK automatikusan kezeli ezt a módosítást.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-142">hello SDK will handle this change automatically.</span></span>
>
>

<span data-ttu-id="2ac2a-143">Az alábbiakban egy példa van NodeJS/Javascript.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-143">Below is a code example for NodeJS/Javascript.</span></span> <span data-ttu-id="2ac2a-144">Python és a Java követi hello ugyanilyen mintájú.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-144">Python and Java will follow hello same pattern.</span></span>

```java
// Creating a ConnectionPolicy object
var connectionPolicy = new DocumentBase.ConnectionPolicy();

// Setting read region selection preference, in hello following order -
// 1 - West US
// 2 - East US
// 3 - North Europe
connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];

// initialize hello connection
var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);
```

## <a name="rest"></a><span data-ttu-id="2ac2a-145">REST</span><span class="sxs-lookup"><span data-stu-id="2ac2a-145">REST</span></span>
<span data-ttu-id="2ac2a-146">Az adatbázisfiók elérhetővé tett több régióba, ha az ügyfelek lekérhetnek által egy GET kérelem végrehajtása a következő URI hello elérhetőségét.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-146">Once a database account has been made available in multiple regions, clients can query its availability by performing a GET request on hello following URI.</span></span>

    https://{databaseaccount}.documents.azure.com/

<span data-ttu-id="2ac2a-147">hello szolgáltatás régiók és a megfelelő Azure Cosmos DB végpont URI-azonosítók hello replikáinak listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-147">hello service will return a list of regions and their corresponding Azure Cosmos DB endpoint URIs for hello replicas.</span></span> <span data-ttu-id="2ac2a-148">hello aktuális írási terület hello válasz jelzik.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-148">hello current write region will be indicated in hello response.</span></span> <span data-ttu-id="2ac2a-149">hello ügyfél ezután kijelölhet hello megfelelő végpont a minden további REST API-kérelmek az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-149">hello client can then select hello appropriate endpoint for all further REST API requests as follows.</span></span>

<span data-ttu-id="2ac2a-150">Példa egy válasz</span><span class="sxs-lookup"><span data-stu-id="2ac2a-150">Example response</span></span>

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "West US",
                "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "East US",
                "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.com",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }


* <span data-ttu-id="2ac2a-151">Minden PUT, POST és DELETE kérelmek be kell lépnie jelzett toohello írási URI</span><span class="sxs-lookup"><span data-stu-id="2ac2a-151">All PUT, POST and DELETE requests must go toohello indicated write URI</span></span>
* <span data-ttu-id="2ac2a-152">Minden lekérdezi és egyéb olvasási kérések (például a lekérdezések) előfordulhat, hogy nyissa meg az hello ügyfél választott tooany végpont</span><span class="sxs-lookup"><span data-stu-id="2ac2a-152">All GETs and other read-only requests (for example queries) may go tooany endpoint of hello client’s choice</span></span>

<span data-ttu-id="2ac2a-153">Az írási kérések csak tooread régiók sikertelen lesz, és HTTP-hibakódot 403 ("tiltott").</span><span class="sxs-lookup"><span data-stu-id="2ac2a-153">Write requests tooread-only regions will fail with HTTP error code 403 (“Forbidden”).</span></span>

<span data-ttu-id="2ac2a-154">Ha hello írási régió után módosítja a hello ügyfél kezdeti felderítés fázisban későbbi ír előző toohello írási régió sikertelen lesz, és HTTP-hibakódot ("tiltott") 403.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-154">If hello write region changes after hello client’s initial discovery phase, subsequent writes toohello previous write region will fail with HTTP error code 403 (“Forbidden”).</span></span> <span data-ttu-id="2ac2a-155">hello ügyfél kell majd le régiók listáját hello újra tooget hello frissített írási régióban.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-155">hello client should then GET hello list of regions again tooget hello updated write region.</span></span>

<span data-ttu-id="2ac2a-156">Ez azt, hogy ez az oktatóanyag befejezése.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-156">That's it, that completes this tutorial.</span></span> <span data-ttu-id="2ac2a-157">Megismerheti, hogyan toomanage hello olvasásával globálisan replikált fiókja konzisztencia [Azure Cosmos DB-ben konzisztenciaszintek](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="2ac2a-157">You can learn how toomanage hello consistency of your globally replicated account by reading [Consistency levels in Azure Cosmos DB](consistency-levels.md).</span></span> <span data-ttu-id="2ac2a-158">És hogyan globális adatbázis-replikációval kapcsolatos további információk az Azure Cosmos Adatbázisba működik, a következő témakörben: [adatok globálisan Azure Cosmos DB terjesztése](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="2ac2a-158">And for more information about how global database replication works in Azure Cosmos DB, see [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2ac2a-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2ac2a-159">Next steps</span></span>

<span data-ttu-id="2ac2a-160">Ebben az oktatóanyagban hello következő régebben már kötöttek:</span><span class="sxs-lookup"><span data-stu-id="2ac2a-160">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2ac2a-161">Konfigurálja a globális terjesztési hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="2ac2a-161">Configure global distribution using hello Azure portal</span></span>
> * <span data-ttu-id="2ac2a-162">Globális terjesztési hello DocumentDB API-k használatával konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2ac2a-162">Configure global distribution using hello DocumentDB APIs</span></span>

<span data-ttu-id="2ac2a-163">Most már folytathatja a következő útmutató toolearn toohello hogyan helyileg toodevelop hello Azure Cosmos DB helyi emulátor.</span><span class="sxs-lookup"><span data-stu-id="2ac2a-163">You can now proceed toohello next tutorial toolearn how toodevelop locally using hello Azure Cosmos DB local emulator.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="2ac2a-164">Hello emulátorral helyileg fejlesztése</span><span class="sxs-lookup"><span data-stu-id="2ac2a-164">Develop locally with hello emulator</span></span>](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

