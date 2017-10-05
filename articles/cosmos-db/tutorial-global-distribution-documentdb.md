---
title: "A DocumentDB API Azure Cosmos DB globális telepítési útmutató |} Microsoft Docs"
description: "Megtudhatja, hogyan beállítani az Azure Cosmos DB globális terjesztési a DocumentDB API használatával."
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
ms.openlocfilehash: f4d8efe9814bd28bb902567a23b541bc9b5414a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-documentdb-api"></a><span data-ttu-id="7c75b-104">How Azure Cosmos DB globális terjesztési a DocumentDB API-jával beállítása</span><span class="sxs-lookup"><span data-stu-id="7c75b-104">How to setup Azure Cosmos DB global distribution using the DocumentDB API</span></span>

<span data-ttu-id="7c75b-105">Ebben a cikkben megmutatjuk, hogyan használható az Azure-portálon Azure Cosmos DB globális terjesztési beállításához, és csatlakoztassa a DocumentDB API használatával.</span><span class="sxs-lookup"><span data-stu-id="7c75b-105">In this article, we show how to use the Azure portal to setup Azure Cosmos DB global distribution and then connect using the DocumentDB API.</span></span>

<span data-ttu-id="7c75b-106">Ez a cikk ismerteti a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="7c75b-106">This article covers the following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="7c75b-107">Az Azure portál használatával globális terjesztési konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7c75b-107">Configure global distribution using the Azure portal</span></span>
> * <span data-ttu-id="7c75b-108">Globális terjesztési használatával konfigurálja a [DocumentDB API-k](documentdb-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="7c75b-108">Configure global distribution using the [DocumentDB APIs](documentdb-introduction.md)</span></span>

<a id="portal"></a>
[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-documentdb-api"></a><span data-ttu-id="7c75b-109">A preferált régió a DocumentDB API-jával való kapcsolódás</span><span class="sxs-lookup"><span data-stu-id="7c75b-109">Connecting to a preferred region using the DocumentDB API</span></span>

<span data-ttu-id="7c75b-110">Kihasználása érdekében [globális terjesztési](distribute-data-globally.md), ügyfélalkalmazások is adja meg a dokumentum műveletek végrehajtásához használandó régiók rendezett beállítások listáját.</span><span class="sxs-lookup"><span data-stu-id="7c75b-110">In order to take advantage of [global distribution](distribute-data-globally.md), client applications can specify the ordered preference list of regions to be used to perform document operations.</span></span> <span data-ttu-id="7c75b-111">Ezt megteheti a kapcsolat házirend beállításával.</span><span class="sxs-lookup"><span data-stu-id="7c75b-111">This can be done by setting the connection policy.</span></span> <span data-ttu-id="7c75b-112">Az Azure Cosmos DB-fiók konfigurációja, az aktuális területi rendelkezésre állás és a megadott beállításokat szabályozó lista alapján, a legoptimálisabb végpont választja ki a DocumentDB SDK írási és olvasási műveletek.</span><span class="sxs-lookup"><span data-stu-id="7c75b-112">Based on the Azure Cosmos DB account configuration, current regional availability and the preference list specified, the most optimal endpoint will be chosen by the DocumentDB SDK to perform write and read operations.</span></span>

<span data-ttu-id="7c75b-113">Ez a beállítás lista van megadva, a DocumentDB SDK-k használata a kapcsolat inicializálása közben.</span><span class="sxs-lookup"><span data-stu-id="7c75b-113">This preference list is specified when initializing a connection using the DocumentDB SDKs.</span></span> <span data-ttu-id="7c75b-114">Az SDK-k elfogadása "PreferredLocations" nem kötelező paraméter, amely egy Azure-régiók rendezett listáját.</span><span class="sxs-lookup"><span data-stu-id="7c75b-114">The SDKs accept an optional parameter "PreferredLocations" that is an ordered list of Azure regions.</span></span>

<span data-ttu-id="7c75b-115">Az SDK automatikusan elküld minden írási műveleteket ad ki az aktuális írási régió.</span><span class="sxs-lookup"><span data-stu-id="7c75b-115">The SDK will automatically send all writes to the current write region.</span></span>

<span data-ttu-id="7c75b-116">Az összes olvasási kapnak a PreferredLocations lista első rendelkezésre álló terület.</span><span class="sxs-lookup"><span data-stu-id="7c75b-116">All reads will be sent to the first available region in the PreferredLocations list.</span></span> <span data-ttu-id="7c75b-117">A kérés nem teljesíthető, ha az ügyfél lefelé a listában, a következő régióban sikertelen, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="7c75b-117">If the request fails, the client will fail down the list to the next region, and so on.</span></span>

<span data-ttu-id="7c75b-118">Az SDK-k csak megpróbálja beolvasni a régió van megadva a PreferredLocations.</span><span class="sxs-lookup"><span data-stu-id="7c75b-118">The SDKs will only attempt to read from the regions specified in PreferredLocations.</span></span> <span data-ttu-id="7c75b-119">Igen például ha az adatbázis-fiókot a három régióban, de az ügyfél csak meghatározza a nem írási régiók két PreferredLocations, majd nincs olvasási szolgáltató kívül az írási régió, feladatátvétel esetén is.</span><span class="sxs-lookup"><span data-stu-id="7c75b-119">So, for example, if the Database Account is available in three regions, but the client only specifies two of the non-write regions for PreferredLocations, then no reads will be served out of the write region, even in the case of failover.</span></span>

<span data-ttu-id="7c75b-120">Az alkalmazás a jelenlegi írási végpont ellenőrizheti és olvassa el a két tulajdonság, WriteEndpoint és ReadEndpoint, elérhető, a SDK 1.8-as verzióját és az újabb ellenőrzésével az SDK által választott végpont.</span><span class="sxs-lookup"><span data-stu-id="7c75b-120">The application can verify the current write endpoint and read endpoint chosen by the SDK by checking two properties, WriteEndpoint and ReadEndpoint, available in SDK version 1.8 and above.</span></span>

<span data-ttu-id="7c75b-121">A PreferredLocations tulajdonsága nincs beállítva, ha minden kérésnél szolgáltató aktuális írási régióban.</span><span class="sxs-lookup"><span data-stu-id="7c75b-121">If the PreferredLocations property is not set, all requests will be served from the current write region.</span></span>

## <a name="net-sdk"></a><span data-ttu-id="7c75b-122">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="7c75b-122">.NET SDK</span></span>
<span data-ttu-id="7c75b-123">Az SDK kód módosítások nélkül használható.</span><span class="sxs-lookup"><span data-stu-id="7c75b-123">The SDK can be used without any code changes.</span></span> <span data-ttu-id="7c75b-124">Az SDK ebben az esetben automatikusan arra utasítja, mind az Olvasás, és az aktuális írási területen írja.</span><span class="sxs-lookup"><span data-stu-id="7c75b-124">In this case, the SDK automatically directs both reads and writes to the current write region.</span></span>

<span data-ttu-id="7c75b-125">A .NET SDK 1,8 és újabb verziójában a ConnectionPolicy paramétert a DocumentClient konstruktor Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations nevű tulajdonsággal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="7c75b-125">In version 1.8 and later of the .NET SDK, the ConnectionPolicy parameter for the DocumentClient constructor has a property called Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations.</span></span> <span data-ttu-id="7c75b-126">Ez a tulajdonság nem gyűjtemény típusú `<string>` és tartalmaznia kell a régió neveinek listáját.</span><span class="sxs-lookup"><span data-stu-id="7c75b-126">This property is of type Collection `<string>` and should contain a list of region names.</span></span> <span data-ttu-id="7c75b-127">A karakterlánc-értékek az a régió nevét oszloponként formázott a [Azure-régiókat] [ regions] , szóközök nélkül előtt vagy után az első lap, és az utolsó karakter kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="7c75b-127">The string values are formatted per the Region Name column on the [Azure Regions][regions] page, with no spaces before or after the first and last character respectively.</span></span>

<span data-ttu-id="7c75b-128">Az aktuális írási és olvasási végpontok találhatók DocumentClient.WriteEndpoint és DocumentClient.ReadEndpoint kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="7c75b-128">The current write and read endpoints are available in DocumentClient.WriteEndpoint and DocumentClient.ReadEndpoint respectively.</span></span>

> [!NOTE]
> <span data-ttu-id="7c75b-129">Az URL-címeket a végpontok nem hosszú élettartamú állandók kell tekinteni.</span><span class="sxs-lookup"><span data-stu-id="7c75b-129">The URLs for the endpoints should not be considered as long-lived constants.</span></span> <span data-ttu-id="7c75b-130">A szolgáltatás ezek bármikor előfordulhat, hogy frissíteni.</span><span class="sxs-lookup"><span data-stu-id="7c75b-130">The service may update these at any point.</span></span> <span data-ttu-id="7c75b-131">Az SDK kezeli automatikusan ezt a módosítást.</span><span class="sxs-lookup"><span data-stu-id="7c75b-131">The SDK handles this change automatically.</span></span>
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

// connect to DocDB
await docClient.OpenAsync().ConfigureAwait(false);
```

## <a name="nodejs-javascript-and-python-sdks"></a><span data-ttu-id="7c75b-132">NodeJS és a Python SDK-k</span><span class="sxs-lookup"><span data-stu-id="7c75b-132">NodeJS, JavaScript, and Python SDKs</span></span>
<span data-ttu-id="7c75b-133">Az SDK kód módosítások nélkül használható.</span><span class="sxs-lookup"><span data-stu-id="7c75b-133">The SDK can be used without any code changes.</span></span> <span data-ttu-id="7c75b-134">Ebben az esetben az SDK automatikusan átirányítja a mind az Olvasás, mind az írás az aktuális írási régióban.</span><span class="sxs-lookup"><span data-stu-id="7c75b-134">In this case, the SDK will automatically direct both reads and writes to the current write region.</span></span>

<span data-ttu-id="7c75b-135">A 1,8 és minden SDK újabb verziója a ConnectionPolicy paramétert a DocumentClient konstruktor új tulajdonság neve DocumentClient.ConnectionPolicy.PreferredLocations.</span><span class="sxs-lookup"><span data-stu-id="7c75b-135">In version 1.8 and later of each SDK, the ConnectionPolicy parameter for the DocumentClient constructor a new property called DocumentClient.ConnectionPolicy.PreferredLocations.</span></span> <span data-ttu-id="7c75b-136">A paraméter karakterláncok tömbje, amely régió neveinek listáját.</span><span class="sxs-lookup"><span data-stu-id="7c75b-136">This is parameter is an array of strings that takes a list of region names.</span></span> <span data-ttu-id="7c75b-137">A nevek a régió nevét oszloponként vannak formázva a [Azure-régiókat] [ regions] lap.</span><span class="sxs-lookup"><span data-stu-id="7c75b-137">The names are formatted per the Region Name column in the [Azure Regions][regions] page.</span></span> <span data-ttu-id="7c75b-138">A kényelem objektumban AzureDocuments.Regions is használhatja az előre definiált állandók</span><span class="sxs-lookup"><span data-stu-id="7c75b-138">You can also use the predefined constants in the convenience object AzureDocuments.Regions</span></span>

<span data-ttu-id="7c75b-139">Az aktuális írási és olvasási végpontok találhatók DocumentClient.getWriteEndpoint és DocumentClient.getReadEndpoint kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="7c75b-139">The current write and read endpoints are available in DocumentClient.getWriteEndpoint and DocumentClient.getReadEndpoint respectively.</span></span>

> [!NOTE]
> <span data-ttu-id="7c75b-140">Az URL-címeket a végpontok nem hosszú élettartamú állandók kell tekinteni.</span><span class="sxs-lookup"><span data-stu-id="7c75b-140">The URLs for the endpoints should not be considered as long-lived constants.</span></span> <span data-ttu-id="7c75b-141">A szolgáltatás ezek bármikor előfordulhat, hogy frissíteni.</span><span class="sxs-lookup"><span data-stu-id="7c75b-141">The service may update these at any point.</span></span> <span data-ttu-id="7c75b-142">Az SDK automatikusan kezeli ezt a módosítást.</span><span class="sxs-lookup"><span data-stu-id="7c75b-142">The SDK will handle this change automatically.</span></span>
>
>

<span data-ttu-id="7c75b-143">Az alábbiakban egy példa van NodeJS/Javascript.</span><span class="sxs-lookup"><span data-stu-id="7c75b-143">Below is a code example for NodeJS/Javascript.</span></span> <span data-ttu-id="7c75b-144">Python és a Java a rendszer ugyanazt a sablont követi.</span><span class="sxs-lookup"><span data-stu-id="7c75b-144">Python and Java will follow the same pattern.</span></span>

```java
// Creating a ConnectionPolicy object
var connectionPolicy = new DocumentBase.ConnectionPolicy();

// Setting read region selection preference, in the following order -
// 1 - West US
// 2 - East US
// 3 - North Europe
connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];

// initialize the connection
var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);
```

## <a name="rest"></a><span data-ttu-id="7c75b-145">REST</span><span class="sxs-lookup"><span data-stu-id="7c75b-145">REST</span></span>
<span data-ttu-id="7c75b-146">Az adatbázisfiók elérhetővé tett több régióba, ha az ügyfelek lekérhetnek által egy GET kérelem végrehajtása a következő URI-CÍMÉN elérhetőségét.</span><span class="sxs-lookup"><span data-stu-id="7c75b-146">Once a database account has been made available in multiple regions, clients can query its availability by performing a GET request on the following URI.</span></span>

    https://{databaseaccount}.documents.azure.com/

<span data-ttu-id="7c75b-147">A szolgáltatás régiók és a replikák számára a megfelelő Azure Cosmos DB végpont URI-azonosítók listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="7c75b-147">The service will return a list of regions and their corresponding Azure Cosmos DB endpoint URIs for the replicas.</span></span> <span data-ttu-id="7c75b-148">Az aktuális írási terület jelzik a válaszban.</span><span class="sxs-lookup"><span data-stu-id="7c75b-148">The current write region will be indicated in the response.</span></span> <span data-ttu-id="7c75b-149">Az ügyfél kiválaszthatja a megfelelő végpont minden további REST API-kérelmek az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="7c75b-149">The client can then select the appropriate endpoint for all further REST API requests as follows.</span></span>

<span data-ttu-id="7c75b-150">Példa egy válasz</span><span class="sxs-lookup"><span data-stu-id="7c75b-150">Example response</span></span>

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


* <span data-ttu-id="7c75b-151">Minden PUT, POST és DELETE kérelmek be kell lépnie a jelzett írási URI</span><span class="sxs-lookup"><span data-stu-id="7c75b-151">All PUT, POST and DELETE requests must go to the indicated write URI</span></span>
* <span data-ttu-id="7c75b-152">Minden lekérdezi és egyéb olvasási kérések (például a lekérdezések) lehet, hogy nyissa meg az az ügyfél által választott bármely végponthoz</span><span class="sxs-lookup"><span data-stu-id="7c75b-152">All GETs and other read-only requests (for example queries) may go to any endpoint of the client’s choice</span></span>

<span data-ttu-id="7c75b-153">Az írási kérelmek írásvédett régiók sikertelen lesz, és HTTP-hibakódot 403 ("tiltott").</span><span class="sxs-lookup"><span data-stu-id="7c75b-153">Write requests to read-only regions will fail with HTTP error code 403 (“Forbidden”).</span></span>

<span data-ttu-id="7c75b-154">Ha az írási régió változik, az ügyfél kezdeti felderítés fázis után, a későbbi írási műveleteket ad ki az előző írási régió sikertelen lesz, és HTTP-hibakódot 403 ("tiltott").</span><span class="sxs-lookup"><span data-stu-id="7c75b-154">If the write region changes after the client’s initial discovery phase, subsequent writes to the previous write region will fail with HTTP error code 403 (“Forbidden”).</span></span> <span data-ttu-id="7c75b-155">Az ügyfél ezután SZEREZHETI be újra a frissített írási régió beolvasandó régiók listáját.</span><span class="sxs-lookup"><span data-stu-id="7c75b-155">The client should then GET the list of regions again to get the updated write region.</span></span>

<span data-ttu-id="7c75b-156">Ez azt, hogy ez az oktatóanyag befejezése.</span><span class="sxs-lookup"><span data-stu-id="7c75b-156">That's it, that completes this tutorial.</span></span> <span data-ttu-id="7c75b-157">Megismerheti a globális replikált fiókja konzisztencia kezeléséhez olvasásával [Azure Cosmos DB-ben konzisztenciaszintek](consistency-levels.md).</span><span class="sxs-lookup"><span data-stu-id="7c75b-157">You can learn how to manage the consistency of your globally replicated account by reading [Consistency levels in Azure Cosmos DB](consistency-levels.md).</span></span> <span data-ttu-id="7c75b-158">És hogyan globális adatbázis-replikációval kapcsolatos további információk az Azure Cosmos Adatbázisba működik, a következő témakörben: [adatok globálisan Azure Cosmos DB terjesztése](distribute-data-globally.md).</span><span class="sxs-lookup"><span data-stu-id="7c75b-158">And for more information about how global database replication works in Azure Cosmos DB, see [Distribute data globally with Azure Cosmos DB](distribute-data-globally.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7c75b-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7c75b-159">Next steps</span></span>

<span data-ttu-id="7c75b-160">Ebben az oktatóanyagban ezt a következők:</span><span class="sxs-lookup"><span data-stu-id="7c75b-160">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="7c75b-161">Az Azure portál használatával globális terjesztési konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7c75b-161">Configure global distribution using the Azure portal</span></span>
> * <span data-ttu-id="7c75b-162">A DocumentDB API-k használatával globális terjesztési konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7c75b-162">Configure global distribution using the DocumentDB APIs</span></span>

<span data-ttu-id="7c75b-163">Most már folytathatja a következő oktatóanyag megtudhatja, hogyan fejleszthet, helyileg emulátorral Azure Cosmos DB helyi.</span><span class="sxs-lookup"><span data-stu-id="7c75b-163">You can now proceed to the next tutorial to learn how to develop locally using the Azure Cosmos DB local emulator.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="7c75b-164">Helyileg emulátorral fejlesztése</span><span class="sxs-lookup"><span data-stu-id="7c75b-164">Develop locally with the emulator</span></span>](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

