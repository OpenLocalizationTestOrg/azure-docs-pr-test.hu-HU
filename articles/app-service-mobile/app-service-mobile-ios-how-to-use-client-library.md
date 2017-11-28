---
title: "aaaHow tooUse iOS SDK Azure Mobile Apps-alkalmazáshoz"
description: "Hogyan tooUse iOS SDK Azure Mobile Apps-alkalmazáshoz"
services: app-service\mobile
documentationcenter: ios
author: ysxu
manager: yochayk
editor: 
ms.assetid: 4e8e45df-c36a-4a60-9ad4-393ec10b7eb9
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/01/2016
ms.author: yuaxu
ms.openlocfilehash: fa299ab3f152bad12d821832fa9fb5495d1fa296
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-ios-client-library-for-azure-mobile-apps"></a><span data-ttu-id="996e4-103">Hogyan tooUse iOS Azure Mobile Apps készült ügyféloldali kódtára</span><span class="sxs-lookup"><span data-stu-id="996e4-103">How tooUse iOS Client Library for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

<span data-ttu-id="996e4-104">Ez az útmutató útmutatást ad, hogy tooperform szolgáltatást használó általános forgatókönyvhöz hello legújabb [Azure Mobile Apps iOS SDK][1].</span><span class="sxs-lookup"><span data-stu-id="996e4-104">This guide teaches you tooperform common scenarios using hello latest [Azure Mobile Apps iOS SDK][1].</span></span> <span data-ttu-id="996e4-105">Ha új tooAzure Mobile Apps, először végezzen [Azure Mobile Apps gyors üzembe helyezés] toocreate egy háttér, hozzon létre egy táblát, és egy előre elkészített iOS Xcode-projekt letöltése.</span><span class="sxs-lookup"><span data-stu-id="996e4-105">If you are new tooAzure Mobile Apps, first complete [Azure Mobile Apps Quick Start] toocreate a backend, create a table, and download a pre-built iOS Xcode project.</span></span> <span data-ttu-id="996e4-106">Az útmutató azt összpontosítani hello ügyféloldali iOS SDK is.</span><span class="sxs-lookup"><span data-stu-id="996e4-106">In this guide, we focus on hello client-side iOS SDK.</span></span> <span data-ttu-id="996e4-107">toolearn arról kiszolgálóoldali SDK hello háttérkiszolgáló hello című hello Server SDK HOWTOs.</span><span class="sxs-lookup"><span data-stu-id="996e4-107">toolearn more about hello server-side SDK for hello backend, see hello Server SDK HOWTOs.</span></span>

## <a name="reference-documentation"></a><span data-ttu-id="996e4-108">Segédanyagok</span><span class="sxs-lookup"><span data-stu-id="996e4-108">Reference documentation</span></span>
<span data-ttu-id="996e4-109">hello hello iOS ügyfél SDK referenciadokumentációt tartalmaz a következő helyen található: [Azure Mobile Apps iOS ügyfél hivatkozási][2].</span><span class="sxs-lookup"><span data-stu-id="996e4-109">hello reference documentation for hello iOS client SDK is located here: [Azure Mobile Apps iOS Client Reference][2].</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="996e4-110">A támogatott platformok</span><span class="sxs-lookup"><span data-stu-id="996e4-110">Supported Platforms</span></span>
<span data-ttu-id="996e4-111">hello iOS SDK támogatja Objective-C projektek, a Swift 2.2 projektek és a Swift 2.3-projektek az iOS 8.0-s vagy újabb verziójú.</span><span class="sxs-lookup"><span data-stu-id="996e4-111">hello iOS SDK supports Objective-C projects, Swift 2.2 projects, and Swift 2.3 projects for iOS versions 8.0 or later.</span></span>

<span data-ttu-id="996e4-112">hello "server-folyamat" hitelesítési egy webes nézet jelenik meg a felhasználói felület hello használ.</span><span class="sxs-lookup"><span data-stu-id="996e4-112">hello "server-flow" authentication uses a WebView for hello presented UI.</span></span>  <span data-ttu-id="996e4-113">Hello eszköz nem tudja toopresent webes nézet felhasználói Felületet, majd egy másik hitelesítési módszer szükség, amely akkor hello termék külső hello hatókör.</span><span class="sxs-lookup"><span data-stu-id="996e4-113">If hello device is not able toopresent a WebView UI, then another method of authentication is required that is outside hello scope of hello product.</span></span>  
<span data-ttu-id="996e4-114">Ez az SDK így nem alkalmas figyelési típusú vagy hasonló módon korlátozott eszközök.</span><span class="sxs-lookup"><span data-stu-id="996e4-114">This SDK is thus not suitable for Watch-type or similarly restricted devices.</span></span>

## <span data-ttu-id="996e4-115"><a name="Setup"></a>A telepítő és Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="996e4-115"><a name="Setup"></a>Setup and Prerequisites</span></span>
<span data-ttu-id="996e4-116">Ez az útmutató feltételezi, hogy létrehozott egy táblát a háttérkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="996e4-116">This guide assumes that you have created a backend with a table.</span></span> <span data-ttu-id="996e4-117">Ez az útmutató feltételezi, hogy hello táblához hello táblák ugyanazon séma ezen oktatóprogram a.</span><span class="sxs-lookup"><span data-stu-id="996e4-117">This guide assumes that hello table has the same schema as hello tables in those tutorials.</span></span> <span data-ttu-id="996e4-118">Ez az útmutató feltételezi, hogy a kódban hivatkozik `MicrosoftAzureMobile.framework` , majd importálja `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.</span><span class="sxs-lookup"><span data-stu-id="996e4-118">This guide also assumes that in your code, you reference `MicrosoftAzureMobile.framework` and import `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.</span></span>

## <span data-ttu-id="996e4-119"><a name="create-client"></a>Hogyan: ügyfél létrehozása</span><span class="sxs-lookup"><span data-stu-id="996e4-119"><a name="create-client"></a>How to: Create Client</span></span>
<span data-ttu-id="996e4-120">a projekt egy Azure Mobile Apps-háttéralkalmazás tooaccess hozzon létre egy `MSClient`.</span><span class="sxs-lookup"><span data-stu-id="996e4-120">tooaccess an Azure Mobile Apps backend in your project, create an `MSClient`.</span></span> <span data-ttu-id="996e4-121">Cserélje le `AppUrl` hello alkalmazás URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="996e4-121">Replace `AppUrl` with hello app URL.</span></span> <span data-ttu-id="996e4-122">Hagyhatja `gatewayURLString` és `applicationKey` üres.</span><span class="sxs-lookup"><span data-stu-id="996e4-122">You may leave `gatewayURLString` and `applicationKey` empty.</span></span> <span data-ttu-id="996e4-123">Ha állít be egy átjárót a hitelesítéshez, feltöltése `gatewayURLString` hello átjáró URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="996e4-123">If you set up a gateway for authentication, populate `gatewayURLString` with hello gateway URL.</span></span>

<span data-ttu-id="996e4-124">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-124">**Objective-C**:</span></span>

```
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

<span data-ttu-id="996e4-125">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-125">**Swift**:</span></span>

```
let client = MSClient(applicationURLString: "AppUrl")
```


## <span data-ttu-id="996e4-126"><a name="table-reference"></a>Hogyan: táblahivatkozás létrehozása</span><span class="sxs-lookup"><span data-stu-id="996e4-126"><a name="table-reference"></a>How to: Create Table Reference</span></span>
<span data-ttu-id="996e4-127">tooaccess vagy frissítés adatokat, a hivatkozás toohello háttér tábla létrehozása.</span><span class="sxs-lookup"><span data-stu-id="996e4-127">tooaccess or update data, create a reference toohello backend table.</span></span> <span data-ttu-id="996e4-128">Cserélje le `TodoItem` hello nevű a táblázat</span><span class="sxs-lookup"><span data-stu-id="996e4-128">Replace `TodoItem` with hello name of your table</span></span>

<span data-ttu-id="996e4-129">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-129">**Objective-C**:</span></span>

```
MSTable *table = [client tableWithName:@"TodoItem"];
```

<span data-ttu-id="996e4-130">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-130">**Swift**:</span></span>

```
let table = client.tableWithName("TodoItem")
```


## <span data-ttu-id="996e4-131"><a name="querying"></a>Útmutató: adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="996e4-131"><a name="querying"></a>How to: Query Data</span></span>
<span data-ttu-id="996e4-132">toocreate egy adatbázis-lekérdezés lekérdezés hello `MSTable` objektum.</span><span class="sxs-lookup"><span data-stu-id="996e4-132">toocreate a database query, query hello `MSTable` object.</span></span> <span data-ttu-id="996e4-133">hello alábbi lekérdezés lekérdezi összes hello elemet `TodoItem` és a naplók hello minden elem szövegét.</span><span class="sxs-lookup"><span data-stu-id="996e4-133">hello following query gets all hello items in `TodoItem` and logs hello text of each item.</span></span>

<span data-ttu-id="996e4-134">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-134">**Objective-C**:</span></span>

```
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occured
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

<span data-ttu-id="996e4-135">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-135">**Swift**:</span></span>

```
table.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

## <span data-ttu-id="996e4-136"><a name="filtering"></a>Hogyan: szűrő adatokat adott vissza.</span><span class="sxs-lookup"><span data-stu-id="996e4-136"><a name="filtering"></a>How to: Filter Returned Data</span></span>
<span data-ttu-id="996e4-137">toofilter eredmény elérése érdekében számos lehetőség közül.</span><span class="sxs-lookup"><span data-stu-id="996e4-137">toofilter results, there are many available options.</span></span>

<span data-ttu-id="996e4-138">toofilter használatával predikátum, használjon egy `NSPredicate` és `readWithPredicate`.</span><span class="sxs-lookup"><span data-stu-id="996e4-138">toofilter using a predicate, use an `NSPredicate` and `readWithPredicate`.</span></span> <span data-ttu-id="996e4-139">hello következő szűrők visszaadott adatok toofind csak hiányos Todo elemeket.</span><span class="sxs-lookup"><span data-stu-id="996e4-139">hello following filters returned data toofind only incomplete Todo items.</span></span>

<span data-ttu-id="996e4-140">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-140">**Objective-C**:</span></span>

```
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query hello TodoItem table
[table readWithPredicate:predicate completion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

<span data-ttu-id="996e4-141">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-141">**Swift**:</span></span>

```
// Create a predicate that finds items where complete is false
let predicate =  NSPredicate(format: "complete == NO")
// Query hello TodoItem table
table.readWithPredicate(predicate) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

## <span data-ttu-id="996e4-142"><a name="query-object"></a>Hogyan: MSQuery használata</span><span class="sxs-lookup"><span data-stu-id="996e4-142"><a name="query-object"></a>How to: Use MSQuery</span></span>
<span data-ttu-id="996e4-143">tooperform egy összetett lekérdezés (beleértve a rendezés és lapozáshoz), hozzon létre egy `MSQuery` objektum, közvetlenül vagy predikátum használatával:</span><span class="sxs-lookup"><span data-stu-id="996e4-143">tooperform a complex query (including sorting and paging), create an `MSQuery` object, directly or by using a predicate:</span></span>

<span data-ttu-id="996e4-144">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-144">**Objective-C**:</span></span>

```
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

<span data-ttu-id="996e4-145">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-145">**Swift**:</span></span>

```
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

<span data-ttu-id="996e4-146">`MSQuery`lehetővé teszi, hogy több lekérdezés viselkedések szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="996e4-146">`MSQuery` lets you control several query behaviors.</span></span>

* <span data-ttu-id="996e4-147">Adja meg az eredmények sorrendje</span><span class="sxs-lookup"><span data-stu-id="996e4-147">Specify order of results</span></span>
* <span data-ttu-id="996e4-148">Mely mezők tooreturn korlátozása</span><span class="sxs-lookup"><span data-stu-id="996e4-148">Limit which fields tooreturn</span></span>
* <span data-ttu-id="996e4-149">Tooreturn rekordok számának korlátozása</span><span class="sxs-lookup"><span data-stu-id="996e4-149">Limit how many records tooreturn</span></span>
* <span data-ttu-id="996e4-150">Adja meg a számuk adott válasz</span><span class="sxs-lookup"><span data-stu-id="996e4-150">Specify total count in response</span></span>
* <span data-ttu-id="996e4-151">Adja meg az egyéni lekérdezési karakterlánc paramétert kérelem</span><span class="sxs-lookup"><span data-stu-id="996e4-151">Specify custom query string parameters in request</span></span>
* <span data-ttu-id="996e4-152">További funkciók alkalmazása</span><span class="sxs-lookup"><span data-stu-id="996e4-152">Apply additional functions</span></span>

<span data-ttu-id="996e4-153">Hajtsa végre egy `MSQuery` meghívásával lekérdezés `readWithCompletion` hello objektumon.</span><span class="sxs-lookup"><span data-stu-id="996e4-153">Execute an `MSQuery` query by calling `readWithCompletion` on hello object.</span></span>

## <span data-ttu-id="996e4-154"><a name="sorting"></a>Hogyan: MSQuery az adatok rendezése</span><span class="sxs-lookup"><span data-stu-id="996e4-154"><a name="sorting"></a>How to: Sort Data with MSQuery</span></span>
<span data-ttu-id="996e4-155">toosort eredményeket, például vizsgáljuk meg.</span><span class="sxs-lookup"><span data-stu-id="996e4-155">toosort results, let's look at an example.</span></span> <span data-ttu-id="996e4-156">mező "text" növekvő, azután a "teljes" csökkenő toosort meghívása `MSQuery` , például így:</span><span class="sxs-lookup"><span data-stu-id="996e4-156">toosort by field 'text' ascending, then by 'complete' descending, invoke `MSQuery` like so:</span></span>

<span data-ttu-id="996e4-157">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-157">**Objective-C**:</span></span>

```
[query orderByAscending:@"text"];
[query orderByDescending:@"complete"];
[query readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

<span data-ttu-id="996e4-158">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-158">**Swift**:</span></span>

```
query.orderByAscending("text")
query.orderByDescending("complete")
query.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```


## <span data-ttu-id="996e4-159"><a name="selecting"></a><a name="parameters"></a>Hogyan: mezők korlátjának növelését, és bontsa ki a lekérdezési karakterlánc paramétereket MSQuery</span><span class="sxs-lookup"><span data-stu-id="996e4-159"><a name="selecting"></a><a name="parameters"></a>How to: Limit Fields and Expand Query String Parameters with MSQuery</span></span>
<span data-ttu-id="996e4-160">a lekérdezés által visszaadott toolimit mezők toobe hello hello mezők nevét adja meg hello **selectFields** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="996e4-160">toolimit fields toobe returned in a query, specify hello names of hello fields in hello **selectFields** property.</span></span> <span data-ttu-id="996e4-161">Ebben a példában csak a hello szöveg és a befejezett mezők adja vissza:</span><span class="sxs-lookup"><span data-stu-id="996e4-161">This example returns only hello text and completed fields:</span></span>

<span data-ttu-id="996e4-162">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-162">**Objective-C**:</span></span>

```
query.selectFields = @[@"text", @"complete"];
```

<span data-ttu-id="996e4-163">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-163">**Swift**:</span></span>

```
query.selectFields = ["text", "complete"]
```

<span data-ttu-id="996e4-164">hello server tooinclude további lekérdezési karakterlánc-paraméterrel (például azért, mert egy egyéni kiszolgálóoldali parancsfájl használja őket) kérelmezéséhez feltöltése `query.parameters` , például így:</span><span class="sxs-lookup"><span data-stu-id="996e4-164">tooinclude additional query string parameters in hello server request (for example, because a custom server-side script uses them), populate `query.parameters` like so:</span></span>

<span data-ttu-id="996e4-165">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-165">**Objective-C**:</span></span>

```
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

<span data-ttu-id="996e4-166">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-166">**Swift**:</span></span>

```
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <span data-ttu-id="996e4-167"><a name="paging"></a>Hogyan: oldalméret konfigurálása</span><span class="sxs-lookup"><span data-stu-id="996e4-167"><a name="paging"></a>How to: Configure Page Size</span></span>
<span data-ttu-id="996e4-168">Az Azure Mobile Apps hello mérete Lapvezérlők hello hello háttér táblázatokból egyszerre vannak lekért rekordok száma.</span><span class="sxs-lookup"><span data-stu-id="996e4-168">With Azure Mobile Apps, hello page size controls hello number of records that are pulled at a time from hello backend tables.</span></span> <span data-ttu-id="996e4-169">A hívás túl`pull` adatokat szeretné majd a batch-adatokat, a lap mérete alapján, amíg nincs további rekordok toopull.</span><span class="sxs-lookup"><span data-stu-id="996e4-169">A call too`pull` data would then batch up data, based on this page size, until there are no more records toopull.</span></span>

<span data-ttu-id="996e4-170">A lap méret használatával lehetséges tooconfigure **MSPullSettings** alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="996e4-170">It's possible tooconfigure a page size using **MSPullSettings** as shown below.</span></span> <span data-ttu-id="996e4-171">hello alapértelmezett oldal mérete 50, és az alábbi példa hello too3 módosítja azt.</span><span class="sxs-lookup"><span data-stu-id="996e4-171">hello default page size is 50, and hello example below changes it too3.</span></span>

<span data-ttu-id="996e4-172">Konfigurálhatja a különböző méretet a jobb teljesítmény érdekében.</span><span class="sxs-lookup"><span data-stu-id="996e4-172">You could configure a different page size for performance reasons.</span></span> <span data-ttu-id="996e4-173">Ha kis rekordok nagy száma, a magas oldalméret csökkenti server üzenetváltások hello számát.</span><span class="sxs-lookup"><span data-stu-id="996e4-173">If you have a large number of small data records, a high page size reduces hello number of server round-trips.</span></span>

<span data-ttu-id="996e4-174">Ez a beállítás csak hello oldalméret hello ügyféloldalon szabályozza.</span><span class="sxs-lookup"><span data-stu-id="996e4-174">This setting controls only hello page size on hello client side.</span></span> <span data-ttu-id="996e4-175">Ha hello ügyfél hello Mobile Apps-háttéralkalmazás támogatja, mint nagyobb oldalméretet kér, hello oldalméret tárfiókonként maximum hello maximális hello háttér pedig konfigurált toosupport.</span><span class="sxs-lookup"><span data-stu-id="996e4-175">If hello client asks for a larger page size than hello Mobile Apps backend supports, hello page size is capped at hello maximum hello backend is configured toosupport.</span></span>

<span data-ttu-id="996e4-176">Ez a beállítás akkor is hello *szám* az rekordok, nem hello *bájtméretnek*.</span><span class="sxs-lookup"><span data-stu-id="996e4-176">This setting is also hello *number* of data records, not hello *byte size*.</span></span>

<span data-ttu-id="996e4-177">Ha növeli hello ügyfél oldalméret, is növelje hello oldalméret hello kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="996e4-177">If you increase hello client page size, you should also increase hello page size on hello server.</span></span> <span data-ttu-id="996e4-178">Lásd: ["hogyan: hello tábla a lapozófájl méretének módosítása"](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) a hello lépések toodo ez.</span><span class="sxs-lookup"><span data-stu-id="996e4-178">See ["How to: Adjust hello table paging size"](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) for hello steps toodo this.</span></span>

<span data-ttu-id="996e4-179">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-179">**Objective-C**:</span></span>

```
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                }
                           }];
```


<span data-ttu-id="996e4-180">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-180">**Swift**:</span></span>

```
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    }
}
```

## <span data-ttu-id="996e4-181"><a name="inserting"></a>Útmutató: adatok beszúrása</span><span class="sxs-lookup"><span data-stu-id="996e4-181"><a name="inserting"></a>How to: Insert Data</span></span>
<span data-ttu-id="996e4-182">tooinsert egy új sorának, hozzon létre egy `NSDictionary` és invoke `table insert`.</span><span class="sxs-lookup"><span data-stu-id="996e4-182">tooinsert a new table row, create a `NSDictionary` and invoke `table insert`.</span></span> <span data-ttu-id="996e4-183">Ha [dinamikus séma] van engedélyezve van, hello Azure App Service mobil-háttéralkalmazást automatikusan hoz létre új oszlopok hello alapján `NSDictionary`.</span><span class="sxs-lookup"><span data-stu-id="996e4-183">If [Dynamic Schema] is enabled, hello Azure App Service mobile backend automatically generates new columns based on hello `NSDictionary`.</span></span>

<span data-ttu-id="996e4-184">Ha `id` van a nem Microsofttól származó, hello háttér automatikusan hoz létre egy új egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="996e4-184">If `id` is not provided, hello backend automatically generates a new unique ID.</span></span> <span data-ttu-id="996e4-185">Adja meg a saját `id` toouse e-mail címeket, a felhasználónevek vagy a saját egyéni értékeket, azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="996e4-185">Provide your own `id` toouse email addresses, usernames, or your own custom values as ID.</span></span> <span data-ttu-id="996e4-186">Saját azonosító megadása is megkönnyítik illesztések és üzleti célú adatbázis programot.</span><span class="sxs-lookup"><span data-stu-id="996e4-186">Providing your own ID may ease joins and business-oriented database logic.</span></span>

<span data-ttu-id="996e4-187">Hello `result` beszúrt hello új cikk tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="996e4-187">hello `result` contains hello new item that was inserted.</span></span> <span data-ttu-id="996e4-188">Attól függően, hogy a kiszolgáló logika lehet további, vagy módosított adatok képest toowhat toohello server lett átadva.</span><span class="sxs-lookup"><span data-stu-id="996e4-188">Depending on your server logic, it may have additional or modified data compared toowhat was passed toohello server.</span></span>

<span data-ttu-id="996e4-189">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-189">**Objective-C**:</span></span>

```
NSDictionary *newItem = @{@"id": @"custom-id", @"text": @"my new item", @"complete" : @NO};
[table insert:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

<span data-ttu-id="996e4-190">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-190">**Swift**:</span></span>

```
let newItem = ["id": "custom-id", "text": "my new item", "complete": false]
table.insert(newItem) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

## <span data-ttu-id="996e4-191"><a name="modifying"></a>Útmutató: adatok módosítása</span><span class="sxs-lookup"><span data-stu-id="996e4-191"><a name="modifying"></a>How to: Modify Data</span></span>
<span data-ttu-id="996e4-192">egy meglévő sor tooupdate módosítani egy elemet, és a hívás `update`:</span><span class="sxs-lookup"><span data-stu-id="996e4-192">tooupdate an existing row, modify an item and call `update`:</span></span>

<span data-ttu-id="996e4-193">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-193">**Objective-C**:</span></span>

```
NSMutableDictionary *newItem = [oldItem mutableCopy]; // oldItem is NSDictionary
[newItem setValue:@"Updated text" forKey:@"text"];
[table update:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

<span data-ttu-id="996e4-194">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-194">**Swift**:</span></span>

```
if let newItem = oldItem.mutableCopy() as? NSMutableDictionary {
    newItem["text"] = "Updated text"
    table2.update(newItem as [NSObject: AnyObject], completion: { (result, error) -> Void in
        if let err = error {
            print("ERROR ", err)
        } else if let item = result {
            print("Todo Item: ", item["text"])
        }
    })
}
```

<span data-ttu-id="996e4-195">Másik lehetőségként adja meg a hello Sorazonosító és a frissített hello mező:</span><span class="sxs-lookup"><span data-stu-id="996e4-195">Alternatively, supply hello row ID and hello updated field:</span></span>

<span data-ttu-id="996e4-196">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-196">**Objective-C**:</span></span>

```
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

<span data-ttu-id="996e4-197">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-197">**Swift**:</span></span>

```
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

<span data-ttu-id="996e4-198">Legalább hello `id` attribútumot úgy kell beállítani, amikor frissíteni.</span><span class="sxs-lookup"><span data-stu-id="996e4-198">At minimum, hello `id` attribute must be set when making updates.</span></span>

## <span data-ttu-id="996e4-199"><a name="deleting"></a>Útmutató: adatok törlése</span><span class="sxs-lookup"><span data-stu-id="996e4-199"><a name="deleting"></a>How to: Delete Data</span></span>
<span data-ttu-id="996e4-200">egy elem, toodelete meghívása `delete` hello elemhez:</span><span class="sxs-lookup"><span data-stu-id="996e4-200">toodelete an item, invoke `delete` with hello item:</span></span>

<span data-ttu-id="996e4-201">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-201">**Objective-C**:</span></span>

```
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

<span data-ttu-id="996e4-202">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-202">**Swift**:</span></span>

```
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

<span data-ttu-id="996e4-203">Azt is megteheti törölje a Sorazonosító megadásával:</span><span class="sxs-lookup"><span data-stu-id="996e4-203">Alternatively, delete by providing a row ID:</span></span>

<span data-ttu-id="996e4-204">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-204">**Objective-C**:</span></span>

```
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

<span data-ttu-id="996e4-205">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-205">**Swift**:</span></span>

```
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

<span data-ttu-id="996e4-206">Legalább hello `id` attribútumot úgy kell beállítani, ha törli az elvégzése.</span><span class="sxs-lookup"><span data-stu-id="996e4-206">At minimum, hello `id` attribute must be set when making deletes.</span></span>

## <span data-ttu-id="996e4-207"><a name="customapi"></a>Útmutató: egyéni API hívása</span><span class="sxs-lookup"><span data-stu-id="996e4-207"><a name="customapi"></a>How to: Call Custom API</span></span>
<span data-ttu-id="996e4-208">Egy egyéni API-val olyan háttér funkciót is elérhetővé teheti.</span><span class="sxs-lookup"><span data-stu-id="996e4-208">With a custom API, you can expose any backend functionality.</span></span> <span data-ttu-id="996e4-209">Nincs beállítva a toomap tooa művelet.</span><span class="sxs-lookup"><span data-stu-id="996e4-209">It doesn't have toomap tooa table operation.</span></span> <span data-ttu-id="996e4-210">Nem csak akkor kapnak az üzenetkezelési teljesebb körű vezérlése, még akkor is is fejlécek olvasási vagy állították be, és módosítsa a hello válasz törzsében formátuma.</span><span class="sxs-lookup"><span data-stu-id="996e4-210">Not only do you gain more control over messaging, you can even read/set headers and change hello response body format.</span></span> <span data-ttu-id="996e4-211">Hogyan toocreate egyéni API hello háttérkiszolgálón, olvassa el toolearn [egyéni API-k](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)</span><span class="sxs-lookup"><span data-stu-id="996e4-211">toolearn how toocreate a custom API on hello backend, read [Custom APIs](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)</span></span>

<span data-ttu-id="996e4-212">toocall egy egyéni API-hívás `MSClient.invokeAPI`.</span><span class="sxs-lookup"><span data-stu-id="996e4-212">toocall a custom API, call `MSClient.invokeAPI`.</span></span> <span data-ttu-id="996e4-213">hello kérelem-válasz tartalom JSON tekintendők.</span><span class="sxs-lookup"><span data-stu-id="996e4-213">hello request and response content are treated as JSON.</span></span> <span data-ttu-id="996e4-214">toouse más adathordozók típusairól [használja más túlterhelése hello `invokeAPI` ] [ 5].</span><span class="sxs-lookup"><span data-stu-id="996e4-214">toouse other media types, [use hello other overload of `invokeAPI`][5].</span></span>  <span data-ttu-id="996e4-215">toomake egy `GET` ahelyett, hogy kérjen egy `POST` kérelmezéséhez set paraméter `HTTPMethod` túl`"GET"` és paraméter `body` túl`nil` (mivel a GET-kérésekhez nincs az üzenet törzse.) Ha az egyéni API támogatja az egyéb HTTP-műveletek, módosítsa `HTTPMethod` megfelelően.</span><span class="sxs-lookup"><span data-stu-id="996e4-215">toomake a `GET` request instead of a `POST` request, set parameter `HTTPMethod` too`"GET"` and parameter `body` too`nil` (since GET requests do not have message bodies.) If your custom API supports other HTTP verbs, change `HTTPMethod` appropriately.</span></span>

<span data-ttu-id="996e4-216">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-216">**Objective-C**:</span></span>

```
[self.client invokeAPI:@"sendEmail"
                  body:@{ @"contents": @"Hello world!" }
            HTTPMethod:@"POST"
            parameters:@{ @"to": @"bill@contoso.com", @"subject" : @"Hi!" }
               headers:nil
            completion: ^(NSData *result, NSHTTPURLResponse *response, NSError *error) {
                if(error) {
                    NSLog(@"ERROR %@", error);
                } else {
                    // Do something with result
                }
            }];
```

<span data-ttu-id="996e4-217">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-217">**Swift**:</span></span>

```
client.invokeAPI("sendEmail",
            body: [ "contents": "Hello World" ],
            HTTPMethod: "POST",
            parameters: [ "to": "bill@contoso.com", "subject" : "Hi!" ],
            headers: nil)
            {
                (result, response, error) -> Void in
                if let err = error {
                    print("ERROR ", err)
                } else if let res = result {
                          // Do something with result
                }
        }
```

## <span data-ttu-id="996e4-218"><a name="templates"></a>Útmutató: regisztráció leküldéses sablonok toosend platformfüggetlen értesítések</span><span class="sxs-lookup"><span data-stu-id="996e4-218"><a name="templates"></a>How to: Register push templates toosend cross-platform notifications</span></span>
<span data-ttu-id="996e4-219">tooregister sablonok adja át a sablon is van a **client.push registerDeviceToken** ügyfélalkalmazás metódust.</span><span class="sxs-lookup"><span data-stu-id="996e4-219">tooregister templates, pass templates with your **client.push registerDeviceToken** method in your client app.</span></span>

<span data-ttu-id="996e4-220">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-220">**Objective-C**:</span></span>

```
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

<span data-ttu-id="996e4-221">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-221">**Swift**:</span></span>

```
    client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
        if let err = error {
            print("ERROR ", err)
        }
    })
```

<span data-ttu-id="996e4-222">A sablonok NSDictionary típusú, és tartalmazhat több sablonok hello a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="996e4-222">Your templates are of type NSDictionary and can contain multiple templates in hello following format:</span></span>

<span data-ttu-id="996e4-223">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-223">**Objective-C**:</span></span>

```
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

<span data-ttu-id="996e4-224">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-224">**Swift**:</span></span>

```
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

<span data-ttu-id="996e4-225">Minden címkék biztonsági hello kérelem üres karaktert törölni.</span><span class="sxs-lookup"><span data-stu-id="996e4-225">All tags are stripped from hello request for security.</span></span>  <span data-ttu-id="996e4-226">tooadd címkéket tooinstallations vagy sablonok telepítések belül, lásd: [használható hello .NET háttérkiszolgáló SDK az Azure Mobile Apps a][4].</span><span class="sxs-lookup"><span data-stu-id="996e4-226">tooadd tags tooinstallations or templates within installations, see [Work with hello .NET backend server SDK for Azure Mobile Apps][4].</span></span>  <span data-ttu-id="996e4-227">regisztrált ezeket a sablonokat, toosend értesítéseket együttműködve [Notification Hubs API-k][3].</span><span class="sxs-lookup"><span data-stu-id="996e4-227">toosend notifications using these registered templates, work with [Notification Hubs APIs][3].</span></span>

## <span data-ttu-id="996e4-228"><a name="errors"></a>Hogyan: hibák kezelésének</span><span class="sxs-lookup"><span data-stu-id="996e4-228"><a name="errors"></a>How to: Handle Errors</span></span>
<span data-ttu-id="996e4-229">Az Azure App Service mobil-háttéralkalmazást hívásakor hello befejezési blokk tartalmaz egy `NSError` paraméter.</span><span class="sxs-lookup"><span data-stu-id="996e4-229">When you call an Azure App Service mobile backend, hello completion block contains an `NSError` parameter.</span></span> <span data-ttu-id="996e4-230">Ha hiba lép fel, a paraméter nem üres.</span><span class="sxs-lookup"><span data-stu-id="996e4-230">When an error occurs, this parameter is non-nil.</span></span> <span data-ttu-id="996e4-231">A kódban Ez a paraméter ellenőrizze és hello hiba szükséges, kezelését, ahogyan az kódrészleteket megelőző hello.</span><span class="sxs-lookup"><span data-stu-id="996e4-231">In your code, you should check this parameter and handle hello error as needed, as demonstrated in hello preceding code snippets.</span></span>

<span data-ttu-id="996e4-232">hello fájl [ `<WindowsAzureMobileServices/MSError.h>` ] [ 6] hello állandók meghatározása `MSErrorResponseKey`, `MSErrorRequestKey`, és `MSErrorServerItemKey`.</span><span class="sxs-lookup"><span data-stu-id="996e4-232">hello file [`<WindowsAzureMobileServices/MSError.h>`][6] defines hello constants `MSErrorResponseKey`, `MSErrorRequestKey`, and `MSErrorServerItemKey`.</span></span> <span data-ttu-id="996e4-233">tooget kapcsolódó további adatok toohello hiba:</span><span class="sxs-lookup"><span data-stu-id="996e4-233">tooget more data related toohello error:</span></span>

<span data-ttu-id="996e4-234">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-234">**Objective-C**:</span></span>

```
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

<span data-ttu-id="996e4-235">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-235">**Swift**:</span></span>

```
let serverItem = error.userInfo[MSErrorServerItemKey]
```

<span data-ttu-id="996e4-236">Ezenkívül hello fájl meghatározza, hogy minden hibakód állandókat:</span><span class="sxs-lookup"><span data-stu-id="996e4-236">In addition, hello file defines constants for each error code:</span></span>

<span data-ttu-id="996e4-237">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-237">**Objective-C**:</span></span>

```
if (error.code == MSErrorPreconditionFailed) {
```

<span data-ttu-id="996e4-238">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-238">**Swift**:</span></span>

```
if (error.code == MSErrorPreconditionFailed) {
```

## <span data-ttu-id="996e4-239"><a name="adal"></a>Hogyan: hitelesíti a felhasználókat az Active Directory Authentication Library hello</span><span class="sxs-lookup"><span data-stu-id="996e4-239"><a name="adal"></a>How to: Authenticate users with hello Active Directory Authentication Library</span></span>
<span data-ttu-id="996e4-240">Az alkalmazás Azure Active Directory használatával hello Active Directory Authentication Library (ADAL) toosign felhasználók is használhatja.</span><span class="sxs-lookup"><span data-stu-id="996e4-240">You can use hello Active Directory Authentication Library (ADAL) toosign users into your application using Azure Active Directory.</span></span> <span data-ttu-id="996e4-241">Ügyfél adatfolyam-hitelesítéshez az identitásszolgáltató SDK előnyösebb toousing hello `loginWithProvider:completion:` metódust.</span><span class="sxs-lookup"><span data-stu-id="996e4-241">Client flow authentication using an identity provider SDK is preferable toousing hello `loginWithProvider:completion:` method.</span></span>  <span data-ttu-id="996e4-242">Ügyfél folyamata hitelesítést biztosít több natív UX abba, és lehetővé teszi, hogy további testreszabási.</span><span class="sxs-lookup"><span data-stu-id="996e4-242">Client flow authentication provides a more native UX feel and allows for additional customization.</span></span>

1. <span data-ttu-id="996e4-243">A mobil-háttéralkalmazás számára az AAD-bejelentkezés konfigurálása következő hello [hogyan tooconfigure App Service az Active Directory bejelentkezési] [ 7] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="996e4-243">Configure your mobile app backend for AAD sign-in by following hello [How tooconfigure App Service for Active Directory login][7] tutorial.</span></span> <span data-ttu-id="996e4-244">Győződjön meg arról, hogy toocomplete hello opcionális lépés egy natív ügyfélalkalmazás regisztrációján.</span><span class="sxs-lookup"><span data-stu-id="996e4-244">Make sure toocomplete hello optional step of registering a native client application.</span></span> <span data-ttu-id="996e4-245">Az iOS, azt javasoljuk, hogy hello átirányítási URI megadása hello űrlap `<app-scheme>://<bundle-id>`.</span><span class="sxs-lookup"><span data-stu-id="996e4-245">For iOS, we recommend that hello redirect URI is of hello form `<app-scheme>://<bundle-id>`.</span></span> <span data-ttu-id="996e4-246">További információkért lásd: hello [ADAL iOS gyors üzembe helyezés][8].</span><span class="sxs-lookup"><span data-stu-id="996e4-246">For more information, see hello [ADAL iOS quickstart][8].</span></span>
2. <span data-ttu-id="996e4-247">Telepítse a Cocoapods segítségével adal-t.</span><span class="sxs-lookup"><span data-stu-id="996e4-247">Install ADAL using Cocoapods.</span></span> <span data-ttu-id="996e4-248">A következő definícióját, hogy Podfile tooinclude hello szerkesztése **YOUR-projekt** az Xcode-projektjéhez nevű hello:</span><span class="sxs-lookup"><span data-stu-id="996e4-248">Edit your Podfile tooinclude hello following definition, replacing **YOUR-PROJECT** with hello name of your Xcode project:</span></span>

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   <span data-ttu-id="996e4-249">és hello Pod:</span><span class="sxs-lookup"><span data-stu-id="996e4-249">and hello Pod:</span></span>

        pod 'ADALiOS'
3. <span data-ttu-id="996e4-250">Hello terminálban futtatása használatával `pod install` hello könyvtárból a projektet tartalmazó, majd nyissa meg a létrehozott hello Xcode-munkaterületet (hello projekthez nem).</span><span class="sxs-lookup"><span data-stu-id="996e4-250">Using hello Terminal, run `pod install` from hello directory containing your project, and then open hello generated Xcode workspace (not hello project).</span></span>
4. <span data-ttu-id="996e4-251">Adja hozzá a következő kód tooyour alkalmazás, használ toohello nyelv szerint hello.</span><span class="sxs-lookup"><span data-stu-id="996e4-251">Add hello following code tooyour application, according toohello language you are using.</span></span> <span data-ttu-id="996e4-252">Minden ellenőrizze a cserét:</span><span class="sxs-lookup"><span data-stu-id="996e4-252">In each, make these replacements:</span></span>

   * <span data-ttu-id="996e4-253">Cserélje le **INSERT-SZOLGÁLTATÓ-Itt** hello nevű hello bérlő, amelyben az alkalmazás kiépítve.</span><span class="sxs-lookup"><span data-stu-id="996e4-253">Replace **INSERT-AUTHORITY-HERE** with hello name of hello tenant in which you provisioned your application.</span></span> <span data-ttu-id="996e4-254">A következő formátumban kell megadni https://login.microsoftonline.com/contoso.onmicrosoft.com. Ez az érték hello [klasszikus Azure portálra] Azure Active Directory tartományi lapfülén hello másolhatók.</span><span class="sxs-lookup"><span data-stu-id="996e4-254">The format should be https://login.microsoftonline.com/contoso.onmicrosoft.com. This value can be copied from hello Domain tab in your Azure Active Directory in hello [Azure classic portal].</span></span>
   * <span data-ttu-id="996e4-255">Cserélje le **INSERT-erőforrás-azonosító-Itt** hello ügyfél-azonosítójú a mobil-háttéralkalmazást.</span><span class="sxs-lookup"><span data-stu-id="996e4-255">Replace **INSERT-RESOURCE-ID-HERE** with hello client ID for your mobile app backend.</span></span> <span data-ttu-id="996e4-256">Az ügyfél-azonosító szerezhet be a hello **speciális** lap **Azure Active Directory beállításai** hello portálon.</span><span class="sxs-lookup"><span data-stu-id="996e4-256">You can obtain the client ID from hello **Advanced** tab under **Azure Active Directory Settings** in hello portal.</span></span>
   * <span data-ttu-id="996e4-257">Cserélje le **INSERT-ügyfél-azonosító-Itt** hello ügyfél-azonosítójú hello natív ügyfélalkalmazás másolta.</span><span class="sxs-lookup"><span data-stu-id="996e4-257">Replace **INSERT-CLIENT-ID-HERE** with hello client ID you copied from hello native client application.</span></span>
   * <span data-ttu-id="996e4-258">Cserélje le **INSERT-REDIRECT-URI-Itt** a hellyel való */.auth/login/done* végpont hello HTTPS protokollt használ.</span><span class="sxs-lookup"><span data-stu-id="996e4-258">Replace **INSERT-REDIRECT-URI-HERE** with your site's */.auth/login/done* endpoint, using hello HTTPS scheme.</span></span> <span data-ttu-id="996e4-259">Ez az érték túl hasonló legyen*https://contoso.azurewebsites.net/.auth/login/done*.</span><span class="sxs-lookup"><span data-stu-id="996e4-259">This value should be similar too*https://contoso.azurewebsites.net/.auth/login/done*.</span></span>

<span data-ttu-id="996e4-260">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-260">**Objective-C**:</span></span>

    #import <ADALiOS/ADAuthenticationContext.h>
    #import <ADALiOS/ADAuthenticationSettings.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*))completionBlock;
    {
        NSString *authority = @"INSERT-AUTHORITY-HERE";
        NSString *resourceId = @"INSERT-RESOURCE-ID-HERE";
        NSString *clientId = @"INSERT-CLIENT-ID-HERE";
        NSURL *redirectUri = [[NSURL alloc]initWithString:@"INSERT-REDIRECT-URI-HERE"];
        ADAuthenticationError *error;
        ADAuthenticationContext *authContext = [ADAuthenticationContext authenticationContextWithAuthority:authority error:&error];
        authContext.parentController = parent;
        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:resourceId
                                     clientId:clientId
                                  redirectUri:redirectUri
                              completionBlock:^(ADAuthenticationResult *result) {
                                  if (result.status != AD_SUCCEEDED)
                                  {
                                      completionBlock(nil, result.error);;
                                  }
                                  else
                                  {
                                      NSDictionary *payload = @{
                                                                @"access_token" : result.tokenCacheStoreItem.accessToken
                                                                };
                                      [client loginWithProvider:@"aad" token:payload completion:completionBlock];
                                  }
                              }];
    }


<span data-ttu-id="996e4-261">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-261">**Swift**:</span></span>

    // add hello following imports tooyour bridging header:
    //        #import <ADALiOS/ADAuthenticationContext.h>
    //        #import <ADALiOS/ADAuthenticationSettings.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let authority = "INSERT-AUTHORITY-HERE"
        let resourceId = "INSERT-RESOURCE-ID-HERE"
        let clientId = "INSERT-CLIENT-ID-HERE"
        let redirectUri = NSURL(string: "INSERT-REDIRECT-URI-HERE")
        var error: AutoreleasingUnsafeMutablePointer<ADAuthenticationError?> = nil
        let authContext = ADAuthenticationContext(authority: authority, error: error)
        authContext.parentController = parent
        ADAuthenticationSettings.sharedInstance().enableFullScreen = true
        authContext.acquireTokenWithResource(resourceId, clientId: clientId, redirectUri: redirectUri) { (result) in
                if result.status != AD_SUCCEEDED {
                    completion(nil, result.error)
                }
                else {
                    let payload: [String: String] = ["access_token": result.tokenCacheStoreItem.accessToken]
                    client.loginWithProvider("aad", token: payload, completion: completion)
                }
            }
    }

## <span data-ttu-id="996e4-262"><a name="facebook-sdk"></a>Hogyan: hello Facebook SDK iOS-hez a felhasználók hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="996e4-262"><a name="facebook-sdk"></a>How to: Authenticate users with hello Facebook SDK for iOS</span></span>
<span data-ttu-id="996e4-263">Is használhatja hello Facebook SDK iOS-toosign felhasználóknak az alkalmazás Facebook használatával.</span><span class="sxs-lookup"><span data-stu-id="996e4-263">You can use hello Facebook SDK for iOS toosign users into your application using Facebook.</span></span>  <span data-ttu-id="996e4-264">Ügyfél-hitelesítési folyamat használata hatékonyabb toousing hello `loginWithProvider:completion:` metódust.</span><span class="sxs-lookup"><span data-stu-id="996e4-264">Using a client flow authentication is preferable toousing hello `loginWithProvider:completion:` method.</span></span>  <span data-ttu-id="996e4-265">hello ügyfélhitelesítés folyamat több natív UX abba biztosít, és lehetővé teszi, hogy további testreszabási.</span><span class="sxs-lookup"><span data-stu-id="996e4-265">hello client flow authentication provides a more native UX feel and allows for additional customization.</span></span>

1. <span data-ttu-id="996e4-266">A mobil-háttéralkalmazás a Facebook-bejelentkezés konfigurálása a következő a [hogyan tooconfigure App Service Facebook bejelentkezési azonosítóhoz] [ 9] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="996e4-266">Configure your mobile app backend for Facebook sign-in by following the [How tooconfigure App Service for Facebook login][9] tutorial.</span></span>
2. <span data-ttu-id="996e4-267">Hello Facebook SDK az iOS által következő hello telepítése [Facebook SDK iOS - első lépések] [ 10] dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="996e4-267">Install hello Facebook SDK for iOS by following hello [Facebook SDK for iOS - Getting Started][10] documentation.</span></span> <span data-ttu-id="996e4-268">Alkalmazás létrehozása, helyett hello iOS platform tooyour meglévő regisztrációs is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="996e4-268">Instead of creating an app, you can add hello iOS platform tooyour existing registration.</span></span>
3. <span data-ttu-id="996e4-269">Facebook tartozó dokumentáció néhány Objective-C kódjának hello App delegált tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="996e4-269">Facebook's documentation includes some Objective-C code in hello App Delegate.</span></span> <span data-ttu-id="996e4-270">Ha használ **Swift**, használhatja a következő AppDelegate.swift fordításainak hello:</span><span class="sxs-lookup"><span data-stu-id="996e4-270">If you are using **Swift**, you can use hello following translations for AppDelegate.swift:</span></span>

        // Add hello following import tooyour bridging header:
        //        #import <FBSDKCoreKit/FBSDKCoreKit.h>

        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            FBSDKApplicationDelegate.sharedInstance().application(application, didFinishLaunchingWithOptions: launchOptions)
            // Add any custom logic here.
            return true
        }

        func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject?) -> Bool {
            let handled = FBSDKApplicationDelegate.sharedInstance().application(application, openURL: url, sourceApplication: sourceApplication, annotation: annotation)
            // Add any custom logic here.
            return handled
        }
4. <span data-ttu-id="996e4-271">Továbbá tooadding a `FBSDKCoreKit.framework` tooyour projektre, túl is adjon hozzá egy hivatkozást`FBSDKLoginKit.framework` hello a megszokott módon.</span><span class="sxs-lookup"><span data-stu-id="996e4-271">In addition tooadding `FBSDKCoreKit.framework` tooyour project, also add a reference too`FBSDKLoginKit.framework` in hello same way.</span></span>
5. <span data-ttu-id="996e4-272">Adja hozzá a következő kód tooyour alkalmazás, használ toohello nyelv szerint hello.</span><span class="sxs-lookup"><span data-stu-id="996e4-272">Add hello following code tooyour application, according toohello language you are using.</span></span>

<span data-ttu-id="996e4-273">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-273">**Objective-C**:</span></span>

    #import <FBSDKLoginKit/FBSDKLoginKit.h>
    #import <FBSDKCoreKit/FBSDKAccessToken.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*)) completionBlock;
    {        
        FBSDKLoginManager *loginManager = [[FBSDKLoginManager alloc] init];
        [loginManager
         logInWithReadPermissions: @[@"public_profile"]
         fromViewController:parent
         handler:^(FBSDKLoginManagerLoginResult *result, NSError *error) {
             if (error) {
                 completionBlock(nil, error);
             } else if (result.isCancelled) {
                 completionBlock(nil, error);
             } else {
                 NSDictionary *payload = @{
                                           @"access_token":result.token.tokenString
                                           };
                 [client loginWithProvider:@"facebook" token:payload completion:completionBlock];
             }
         }];
    }

<span data-ttu-id="996e4-274">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-274">**Swift**:</span></span>

    // Add hello following imports tooyour bridging header:
    //        #import <FBSDKLoginKit/FBSDKLoginKit.h>
    //        #import <FBSDKCoreKit/FBSDKAccessToken.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let loginManager = FBSDKLoginManager()
        loginManager.logInWithReadPermissions(["public_profile"], fromViewController: parent) { (result, error) in
            if (error != nil) {
                completion(nil, error)
            }
            else if result.isCancelled {
                completion(nil, error)
            }
            else {
                let payload: [String: String] = ["access_token": result.token.tokenString]
                client.loginWithProvider("facebook", token: payload, completion: completion)
            }
        }
    }

## <span data-ttu-id="996e4-275"><a name="twitter-fabric"></a>Útmutató: az IOS-es Twitter-hálóval felhasználók hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="996e4-275"><a name="twitter-fabric"></a>How to: Authenticate users with Twitter Fabric for iOS</span></span>
<span data-ttu-id="996e4-276">Is használhatja háló iOS toosign felhasználóknak az alkalmazás Twitter használatával.</span><span class="sxs-lookup"><span data-stu-id="996e4-276">You can use Fabric for iOS toosign users into your application using Twitter.</span></span> <span data-ttu-id="996e4-277">Ügyfél folyamata a hitelesítés az előnyösebb toousing hello `loginWithProvider:completion:` metódus, mert több natív UX abba biztosít, és lehetővé teszi, hogy további testreszabási.</span><span class="sxs-lookup"><span data-stu-id="996e4-277">Client Flow authentication is preferable toousing hello `loginWithProvider:completion:` method, as it provides a more native UX feel and allows for additional customization.</span></span>

1. <span data-ttu-id="996e4-278">A mobil-háttéralkalmazás számára Twitter-bejelentkezés konfigurálása a következő hello [hogyan tooconfigure App Service Twitter bejelentkezési azonosítóhoz](app-service-mobile-how-to-configure-twitter-authentication.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="996e4-278">Configure your mobile app backend for Twitter sign-in by following hello [How tooconfigure App Service for Twitter login](app-service-mobile-how-to-configure-twitter-authentication.md) tutorial.</span></span>
2. <span data-ttu-id="996e4-279">Adja hozzá a háló tooyour projekt által következő hello [(iOS) – első lépések a háló] dokumentáció és TwitterKit beállítása.</span><span class="sxs-lookup"><span data-stu-id="996e4-279">Add Fabric tooyour project by following hello [Fabric for iOS - Getting Started] documentation and setting up TwitterKit.</span></span>

   > [!NOTE]
   > <span data-ttu-id="996e4-280">Alapértelmezés szerint háló létrehoz egy Twitter-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="996e4-280">By default, Fabric creates a Twitter application for you.</span></span> <span data-ttu-id="996e4-281">Az alkalmazás létrehozása hello kulcsa és fogyasztói titkos kulcsot, korábban létrehozott alábbi kódtöredékek hello regisztrálásával elkerülheti a.</span><span class="sxs-lookup"><span data-stu-id="996e4-281">You can avoid creating an application by registering hello Consumer Key and Consumer Secret you created earlier using hello following code snippets.</span></span>    <span data-ttu-id="996e4-282">Azt is megteheti, lecserélheti hello kulcsa és fogyasztói titkos kulcs értékeket, hogy megadja a hello szolgáltatás tooApp értékekkel című hello [háló irányítópult].</span><span class="sxs-lookup"><span data-stu-id="996e4-282">Alternatively, you can replace hello Consumer Key and Consumer Secret values that you provide tooApp Service with hello values you see in hello [Fabric Dashboard].</span></span> <span data-ttu-id="996e4-283">Válassza ezt a beállítást, ha lehet, hogy tooset hello visszahívási URL-cím tooa helyőrző értékű, például a `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.</span><span class="sxs-lookup"><span data-stu-id="996e4-283">If you choose this option, be sure tooset hello callback URL tooa placeholder value, such as `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.</span></span>
   >
   >

    <span data-ttu-id="996e4-284">Ha toouse korábban létrehozott hello titkok, adja hozzá a következő kód tooyour App delegált hello:</span><span class="sxs-lookup"><span data-stu-id="996e4-284">If you choose toouse hello secrets you created earlier, add hello following code tooyour App Delegate:</span></span>

    <span data-ttu-id="996e4-285">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-285">**Objective-C**:</span></span>

        #import <Fabric/Fabric.h>
        #import <TwitterKit/TwitterKit.h>
        // ...
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [[Twitter sharedInstance] startWithConsumerKey:@"your_key" consumerSecret:@"your_secret"];
            [Fabric with:@[[Twitter class]]];
            // Add any custom logic here.
            return YES;
        }

    <span data-ttu-id="996e4-286">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-286">**Swift**:</span></span>

        import Fabric
        import TwitterKit
        // ...
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
            Fabric.with([Twitter.self])
            // Add any custom logic here.
            return true
        }
3. <span data-ttu-id="996e4-287">Adja hozzá a következő kód tooyour alkalmazás, használ toohello nyelv szerint hello.</span><span class="sxs-lookup"><span data-stu-id="996e4-287">Add hello following code tooyour application, according toohello language you are using.</span></span>

<span data-ttu-id="996e4-288">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-288">**Objective-C**:</span></span>

    #import <TwitterKit/TwitterKit.h>
    // ...
    - (void)authenticate:(UIViewController*)parent completion:(void (^) (MSUser*, NSError*))completionBlock
    {
        [[Twitter sharedInstance] logInWithCompletion:^(TWTRSession *session, NSError *error) {
            if (session) {
                NSDictionary *payload = @{
                                            @"access_token":session.authToken,
                                            @"access_token_secret":session.authTokenSecret
                                        };
                [client loginWithProvider:@"twitter" token:payload completion:completionBlock];
            } else {
                completionBlock(nil, error);
            }
        }];
    }

<span data-ttu-id="996e4-289">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-289">**Swift**:</span></span>

    import TwitterKit
    // ...
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let client = self.table!.client
        Twitter.sharedInstance().logInWithCompletion { session, error in
            if (session != nil) {
                let payload: [String: String] = ["access_token": session!.authToken, "access_token_secret": session!.authTokenSecret]
                client.loginWithProvider("twitter", token: payload, completion: completion)
            } else {
                completion(nil, error)
            }
        }
    }

## <span data-ttu-id="996e4-290"><a name="google-sdk"></a>Hogyan: hello Google bejelentkezési SDK iOS-hez a felhasználók hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="996e4-290"><a name="google-sdk"></a>How to: Authenticate users with hello Google Sign-In SDK for iOS</span></span>
<span data-ttu-id="996e4-291">Is használhatja hello Google bejelentkezési SDK iOS-toosign felhasználóknak az alkalmazás a Google-fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="996e4-291">You can use hello Google Sign-In SDK for iOS toosign users into your application using a Google account.</span></span>  <span data-ttu-id="996e4-292">Google nemrég jelentette be módosítások tootheir OAuth biztonsági házirendeket.</span><span class="sxs-lookup"><span data-stu-id="996e4-292">Google recently announced changes tootheir OAuth security policies.</span></span>  <span data-ttu-id="996e4-293">A módosítások a házirend a Google SDK-t jövőbeli hello hello használata szükséges.</span><span class="sxs-lookup"><span data-stu-id="996e4-293">These policy changes will require hello use of the Google SDK in hello future.</span></span>

1. <span data-ttu-id="996e4-294">A mobil-háttéralkalmazás Google-bejelentkezés a következő hello konfigurálása [hogyan tooconfigure App Service Google bejelentkezési azonosítóhoz](app-service-mobile-how-to-configure-google-authentication.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="996e4-294">Configure your mobile app backend for Google sign-in by following hello [How tooconfigure App Service for Google login](app-service-mobile-how-to-configure-google-authentication.md) tutorial.</span></span>
2. <span data-ttu-id="996e4-295">Telepítse a Google SDK. hello következő hello által az iOS [Google bejelentkezhet az iOS - Start integrálása](https://developers.google.com/identity/sign-in/ios/start-integrating) dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="996e4-295">Install hello Google SDK for iOS by following hello [Google Sign-In for iOS - Start integrating](https://developers.google.com/identity/sign-in/ios/start-integrating) documentation.</span></span> <span data-ttu-id="996e4-296">Előfordulhat, hogy átugorja hello "Hitelesítéséhez és a háttérkiszolgáló" szakaszt.</span><span class="sxs-lookup"><span data-stu-id="996e4-296">You may skip hello "Authenticate with a Backend Server" section.</span></span>
3. <span data-ttu-id="996e4-297">Adja hozzá a következő tooyour delegált hello `signIn:didSignInForUser:withError:` módszer szerint toohello nyelvet használ.</span><span class="sxs-lookup"><span data-stu-id="996e4-297">Add hello following tooyour delegate's `signIn:didSignInForUser:withError:` method, according toohello language you are using.</span></span>

<span data-ttu-id="996e4-298">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-298">**Objective-C**:</span></span>

        NSDictionary *payload = @{
                                  @"id_token":user.authentication.idToken,
                                  @"authorization_code":user.serverAuthCode
                                  };

        [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
            // ...
        }];

<span data-ttu-id="996e4-299">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-299">**Swift**:</span></span>

        let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
        client.loginWithProvider("google", token: payload) { (user, error) in
            // ...
        }

1. <span data-ttu-id="996e4-300">Győződjön meg arról is hozzáadhat hello túl a következő`application:didFinishLaunchingWithOptions:` delegálása az alkalmazás, "SERVER_CLIENT_ID" lecserélését hello azonos azonosító, hogy Ön használt tooconfigure App Service az 1. lépésben.</span><span class="sxs-lookup"><span data-stu-id="996e4-300">Make sure you also add hello following too`application:didFinishLaunchingWithOptions:` in your app delegate, replacing "SERVER_CLIENT_ID" with hello same ID that you used tooconfigure App Service in step 1.</span></span>

<span data-ttu-id="996e4-301">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-301">**Objective-C**:</span></span>

         [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";

 <span data-ttu-id="996e4-302">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-302">**Swift**:</span></span>

        GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"


1. <span data-ttu-id="996e4-303">Adja hozzá a következő kód tooyour alkalmazást egy UIViewController hello megvalósító hello `GIDSignInUIDelegate` protokoll szerinti toohello nyelvet használ.</span><span class="sxs-lookup"><span data-stu-id="996e4-303">Add hello following code tooyour application in a UIViewController that implements hello `GIDSignInUIDelegate` protocol, according toohello language you are using.</span></span>  <span data-ttu-id="996e4-304">Be van jelentkezve mielőtt újra éppen bejelentkezett, és nincs szüksége tooenter a hitelesítő adatok újra, bár látja-e a hozzájárulási párbeszédablak.</span><span class="sxs-lookup"><span data-stu-id="996e4-304">You are signed out before being signed in again, and although you don't need tooenter your credentials again, you see a consent dialog.</span></span>  <span data-ttu-id="996e4-305">Ez a metódus csak hívható, ha hello munkameneti jogkivonat lejárt.</span><span class="sxs-lookup"><span data-stu-id="996e4-305">Only call this method when hello session token has expired.</span></span>

   <span data-ttu-id="996e4-306">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="996e4-306">**Objective-C**:</span></span>

       #import <Google/SignIn.h>
       // ...
       - (void)authenticate
       {
               [GIDSignIn sharedInstance].uiDelegate = self;
               [[GIDSignIn sharedInstance] signOut];
               [[GIDSignIn sharedInstance] signIn];
        }

   <span data-ttu-id="996e4-307">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="996e4-307">**Swift**:</span></span>

       // ...
       func authenticate() {
           GIDSignIn.sharedInstance().uiDelegate = self
           GIDSignIn.sharedInstance().signOut()
           GIDSignIn.sharedInstance().signIn()
       }

<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[Setup and Prerequisites]: #Setup
[How to: Create hello Mobile Services client]: #create-client
[How to: Create a table reference]: #table-reference
[How to: Query data from a mobile service]: #querying
[Filter returned data]: #filtering
[Sort returned data]: #sorting
[Return data in pages]: #paging
[Select specific columns]: #selecting
[How to: Bind data toohello user interface]: #binding
[How to: Insert data into a mobile service]: #inserting
[How to: Modify data in a mobile service]: #modifying
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching-tokens
[How to: Upload images and large files]: #blobs
[How to: Handle errors]: #errors
[How to: Design unit tests]: #unit-testing
[How to: Customize hello client]: #customizing
[Customize request headers]: #custom-headers
[Customize data type serialization]: #custom-serialization
[Next Steps]: #next-steps
[How to: Use MSQuery]: #query-object

<!-- Images. -->

<!-- URLs. -->
[Azure Mobile Apps gyors üzembe helyezés]: app-service-mobile-ios-get-started.md

[Add Mobile Services tooExisting App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode

[Handling Expired Tokens]: http://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: http://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: http://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts tooauthorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[dinamikus séma]: http://go.microsoft.com/fwlink/p/?LinkId=296271
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: http://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: http://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI toomanage Mobile Services tables]: /cli/azure/get-started-with-az-cli2
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[háló irányítópult]: https://www.fabric.io/home
[(iOS) – első lépések a háló]: https://docs.fabric.io/ios/fabric/getting-started.html
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: http://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: http://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: app-service-mobile-how-to-configure-active-directory-authentication.md
[8]: ../active-directory/active-directory-devquickstarts-ios.md
[9]: app-service-mobile-how-to-configure-facebook-authentication.md
[10]: https://developers.facebook.com/docs/ios/getting-started
