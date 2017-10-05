---
title: "Hogyan használata IOS-hez készült Azure Mobile Apps SDK"
description: "Hogyan használata IOS-hez készült Azure Mobile Apps SDK"
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
ms.openlocfilehash: 65817208e1b26fb5f9eb56d164f48b44d57dce56
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-use-ios-client-library-for-azure-mobile-apps"></a><span data-ttu-id="351e9-103">Hogyan használja iOS Azure Mobile Apps készült ügyféloldali kódtára</span><span class="sxs-lookup"><span data-stu-id="351e9-103">How to Use iOS Client Library for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

<span data-ttu-id="351e9-104">Ez az útmutató útmutatást ad teszi a végrehajtását szolgáltatást a legújabb használó általános forgatókönyvhöz [Azure Mobile Apps iOS SDK][1].</span><span class="sxs-lookup"><span data-stu-id="351e9-104">This guide teaches you to perform common scenarios using the latest [Azure Mobile Apps iOS SDK][1].</span></span> <span data-ttu-id="351e9-105">Ha most ismerkedik az Azure Mobile Apps, először végezzen [Azure Mobile Apps gyors üzembe helyezés] -háttéralkalmazás létrehozása, hozzon létre egy táblát, és töltse le egy előre elkészített iOS Xcode projekt.</span><span class="sxs-lookup"><span data-stu-id="351e9-105">If you are new to Azure Mobile Apps, first complete [Azure Mobile Apps Quick Start] to create a backend, create a table, and download a pre-built iOS Xcode project.</span></span> <span data-ttu-id="351e9-106">Az útmutató azt összpontosítanak az ügyféloldali iOS SDK is.</span><span class="sxs-lookup"><span data-stu-id="351e9-106">In this guide, we focus on the client-side iOS SDK.</span></span> <span data-ttu-id="351e9-107">A kiszolgálóoldali SDK a háttérkiszolgáló kapcsolatos további tudnivalókért tekintse meg a kiszolgáló SDK HOWTOs.</span><span class="sxs-lookup"><span data-stu-id="351e9-107">To learn more about the server-side SDK for the backend, see the Server SDK HOWTOs.</span></span>

## <a name="reference-documentation"></a><span data-ttu-id="351e9-108">Segédanyagok</span><span class="sxs-lookup"><span data-stu-id="351e9-108">Reference documentation</span></span>
<span data-ttu-id="351e9-109">Az iOS-ügyfél SDK dokumentációját a következő helyen található: [Azure Mobile Apps iOS ügyfél hivatkozási][2].</span><span class="sxs-lookup"><span data-stu-id="351e9-109">The reference documentation for the iOS client SDK is located here: [Azure Mobile Apps iOS Client Reference][2].</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="351e9-110">A támogatott platformok</span><span class="sxs-lookup"><span data-stu-id="351e9-110">Supported Platforms</span></span>
<span data-ttu-id="351e9-111">Az iOS SDK támogatja Objective-C projektek, a Swift 2.2 projektek és a Swift 2.3-projektek az iOS 8.0-s vagy újabb verziójú.</span><span class="sxs-lookup"><span data-stu-id="351e9-111">The iOS SDK supports Objective-C projects, Swift 2.2 projects, and Swift 2.3 projects for iOS versions 8.0 or later.</span></span>

<span data-ttu-id="351e9-112">A "server-folyamat" hitelesítési bemutatott felhasználói felülete a webes nézet használja.</span><span class="sxs-lookup"><span data-stu-id="351e9-112">The "server-flow" authentication uses a WebView for the presented UI.</span></span>  <span data-ttu-id="351e9-113">Ha az eszköz nem képesek egy webes nézet felhasználói felület, akkor egy másik hitelesítési módszer szükséges, amely a termék hatókörén kívül esik.</span><span class="sxs-lookup"><span data-stu-id="351e9-113">If the device is not able to present a WebView UI, then another method of authentication is required that is outside the scope of the product.</span></span>  
<span data-ttu-id="351e9-114">Ez az SDK így nem alkalmas figyelési típusú vagy hasonló módon korlátozott eszközök.</span><span class="sxs-lookup"><span data-stu-id="351e9-114">This SDK is thus not suitable for Watch-type or similarly restricted devices.</span></span>

## <span data-ttu-id="351e9-115"><a name="Setup"></a>A telepítő és Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="351e9-115"><a name="Setup"></a>Setup and Prerequisites</span></span>
<span data-ttu-id="351e9-116">Ez az útmutató feltételezi, hogy létrehozott egy táblát a háttérkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="351e9-116">This guide assumes that you have created a backend with a table.</span></span> <span data-ttu-id="351e9-117">Ez az útmutató feltételezi, hogy rendelkezik-e a tábla a táblák ugyanazon séma ezen oktatóprogram a.</span><span class="sxs-lookup"><span data-stu-id="351e9-117">This guide assumes that the table has the same schema as the tables in those tutorials.</span></span> <span data-ttu-id="351e9-118">Ez az útmutató feltételezi, hogy a kódban hivatkozik `MicrosoftAzureMobile.framework` , majd importálja `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.</span><span class="sxs-lookup"><span data-stu-id="351e9-118">This guide also assumes that in your code, you reference `MicrosoftAzureMobile.framework` and import `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.</span></span>

## <span data-ttu-id="351e9-119"><a name="create-client"></a>Hogyan: ügyfél létrehozása</span><span class="sxs-lookup"><span data-stu-id="351e9-119"><a name="create-client"></a>How to: Create Client</span></span>
<span data-ttu-id="351e9-120">Az Azure Mobile Apps-háttéralkalmazás a projekt eléréséhez hozzon létre egy `MSClient`.</span><span class="sxs-lookup"><span data-stu-id="351e9-120">To access an Azure Mobile Apps backend in your project, create an `MSClient`.</span></span> <span data-ttu-id="351e9-121">Cserélje le `AppUrl` az alkalmazás URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="351e9-121">Replace `AppUrl` with the app URL.</span></span> <span data-ttu-id="351e9-122">Hagyhatja `gatewayURLString` és `applicationKey` üres.</span><span class="sxs-lookup"><span data-stu-id="351e9-122">You may leave `gatewayURLString` and `applicationKey` empty.</span></span> <span data-ttu-id="351e9-123">Ha állít be egy átjárót a hitelesítéshez, feltöltése `gatewayURLString` a átjáró URL-címet.</span><span class="sxs-lookup"><span data-stu-id="351e9-123">If you set up a gateway for authentication, populate `gatewayURLString` with the gateway URL.</span></span>

<span data-ttu-id="351e9-124">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-124">**Objective-C**:</span></span>

```
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

<span data-ttu-id="351e9-125">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-125">**Swift**:</span></span>

```
let client = MSClient(applicationURLString: "AppUrl")
```


## <span data-ttu-id="351e9-126"><a name="table-reference"></a>Hogyan: táblahivatkozás létrehozása</span><span class="sxs-lookup"><span data-stu-id="351e9-126"><a name="table-reference"></a>How to: Create Table Reference</span></span>
<span data-ttu-id="351e9-127">Az adatok elérése vagy frissítése érdekében hozzon létre a háttértáblára mutató hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="351e9-127">To access or update data, create a reference to the backend table.</span></span> <span data-ttu-id="351e9-128">A `TodoItem` helyére írja be a tábla nevét.</span><span class="sxs-lookup"><span data-stu-id="351e9-128">Replace `TodoItem` with the name of your table</span></span>

<span data-ttu-id="351e9-129">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-129">**Objective-C**:</span></span>

```
MSTable *table = [client tableWithName:@"TodoItem"];
```

<span data-ttu-id="351e9-130">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-130">**Swift**:</span></span>

```
let table = client.tableWithName("TodoItem")
```


## <span data-ttu-id="351e9-131"><a name="querying"></a>Útmutató: adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="351e9-131"><a name="querying"></a>How to: Query Data</span></span>
<span data-ttu-id="351e9-132">Adatbázis-lekérdezés létrehozásához lekérdezni a `MSTable` objektum.</span><span class="sxs-lookup"><span data-stu-id="351e9-132">To create a database query, query the `MSTable` object.</span></span> <span data-ttu-id="351e9-133">Az alábbi lekérdezés lekérdezi a elemek `TodoItem` és a szöveg, az egyes elemek naplózza.</span><span class="sxs-lookup"><span data-stu-id="351e9-133">The following query gets all the items in `TodoItem` and logs the text of each item.</span></span>

<span data-ttu-id="351e9-134">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-134">**Objective-C**:</span></span>

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

<span data-ttu-id="351e9-135">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-135">**Swift**:</span></span>

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

## <span data-ttu-id="351e9-136"><a name="filtering"></a>Hogyan: szűrő adatokat adott vissza.</span><span class="sxs-lookup"><span data-stu-id="351e9-136"><a name="filtering"></a>How to: Filter Returned Data</span></span>
<span data-ttu-id="351e9-137">Az eredmények szűréséhez többféleképpen érhető el.</span><span class="sxs-lookup"><span data-stu-id="351e9-137">To filter results, there are many available options.</span></span>

<span data-ttu-id="351e9-138">Szűrés predikátum, használja az `NSPredicate` és `readWithPredicate`.</span><span class="sxs-lookup"><span data-stu-id="351e9-138">To filter using a predicate, use an `NSPredicate` and `readWithPredicate`.</span></span> <span data-ttu-id="351e9-139">A szűrők csak hiányos Todo elemeket található adatokat adott vissza.</span><span class="sxs-lookup"><span data-stu-id="351e9-139">The following filters returned data to find only incomplete Todo items.</span></span>

<span data-ttu-id="351e9-140">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-140">**Objective-C**:</span></span>

```
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query the TodoItem table
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

<span data-ttu-id="351e9-141">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-141">**Swift**:</span></span>

```
// Create a predicate that finds items where complete is false
let predicate =  NSPredicate(format: "complete == NO")
// Query the TodoItem table
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

## <span data-ttu-id="351e9-142"><a name="query-object"></a>Hogyan: MSQuery használata</span><span class="sxs-lookup"><span data-stu-id="351e9-142"><a name="query-object"></a>How to: Use MSQuery</span></span>
<span data-ttu-id="351e9-143">Egy összetett lekérdezés (beleértve a rendezés és lapozáshoz), hozzon létre egy `MSQuery` objektum, közvetlenül vagy predikátum használatával:</span><span class="sxs-lookup"><span data-stu-id="351e9-143">To perform a complex query (including sorting and paging), create an `MSQuery` object, directly or by using a predicate:</span></span>

<span data-ttu-id="351e9-144">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-144">**Objective-C**:</span></span>

```
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

<span data-ttu-id="351e9-145">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-145">**Swift**:</span></span>

```
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

<span data-ttu-id="351e9-146">`MSQuery`lehetővé teszi, hogy több lekérdezés viselkedések szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="351e9-146">`MSQuery` lets you control several query behaviors.</span></span>

* <span data-ttu-id="351e9-147">Adja meg az eredmények sorrendje</span><span class="sxs-lookup"><span data-stu-id="351e9-147">Specify order of results</span></span>
* <span data-ttu-id="351e9-148">Térjen vissza mezőket korlátozása</span><span class="sxs-lookup"><span data-stu-id="351e9-148">Limit which fields to return</span></span>
* <span data-ttu-id="351e9-149">Vissza rekordok számának korlátozása</span><span class="sxs-lookup"><span data-stu-id="351e9-149">Limit how many records to return</span></span>
* <span data-ttu-id="351e9-150">Adja meg a számuk adott válasz</span><span class="sxs-lookup"><span data-stu-id="351e9-150">Specify total count in response</span></span>
* <span data-ttu-id="351e9-151">Adja meg az egyéni lekérdezési karakterlánc paramétert kérelem</span><span class="sxs-lookup"><span data-stu-id="351e9-151">Specify custom query string parameters in request</span></span>
* <span data-ttu-id="351e9-152">További funkciók alkalmazása</span><span class="sxs-lookup"><span data-stu-id="351e9-152">Apply additional functions</span></span>

<span data-ttu-id="351e9-153">Hajtsa végre egy `MSQuery` meghívásával lekérdezés `readWithCompletion` az objektumon.</span><span class="sxs-lookup"><span data-stu-id="351e9-153">Execute an `MSQuery` query by calling `readWithCompletion` on the object.</span></span>

## <span data-ttu-id="351e9-154"><a name="sorting"></a>Hogyan: MSQuery az adatok rendezése</span><span class="sxs-lookup"><span data-stu-id="351e9-154"><a name="sorting"></a>How to: Sort Data with MSQuery</span></span>
<span data-ttu-id="351e9-155">Az eredmények rendezéséhez nézzük például.</span><span class="sxs-lookup"><span data-stu-id="351e9-155">To sort results, let's look at an example.</span></span> <span data-ttu-id="351e9-156">Mező "text" növekvő, azután a "teljes" csökkenő rendezéséhez meghívása `MSQuery` , például így:</span><span class="sxs-lookup"><span data-stu-id="351e9-156">To sort by field 'text' ascending, then by 'complete' descending, invoke `MSQuery` like so:</span></span>

<span data-ttu-id="351e9-157">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-157">**Objective-C**:</span></span>

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

<span data-ttu-id="351e9-158">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-158">**Swift**:</span></span>

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


## <span data-ttu-id="351e9-159"><a name="selecting"></a><a name="parameters"></a>Hogyan: mezők korlátjának növelését, és bontsa ki a lekérdezési karakterlánc paramétereket MSQuery</span><span class="sxs-lookup"><span data-stu-id="351e9-159"><a name="selecting"></a><a name="parameters"></a>How to: Limit Fields and Expand Query String Parameters with MSQuery</span></span>
<span data-ttu-id="351e9-160">A lekérdezés által visszaadott mezők korlátozásához adja meg a mezők nevét a **selectFields** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="351e9-160">To limit fields to be returned in a query, specify the names of the fields in the **selectFields** property.</span></span> <span data-ttu-id="351e9-161">Ebben a példában csak a szöveg és a befejezett mezők adja vissza:</span><span class="sxs-lookup"><span data-stu-id="351e9-161">This example returns only the text and completed fields:</span></span>

<span data-ttu-id="351e9-162">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-162">**Objective-C**:</span></span>

```
query.selectFields = @[@"text", @"complete"];
```

<span data-ttu-id="351e9-163">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-163">**Swift**:</span></span>

```
query.selectFields = ["text", "complete"]
```

<span data-ttu-id="351e9-164">A kiszolgálói kérelem közé tartoznak további lekérdezési karakterlánc paraméter (például azért, mert egy egyéni kiszolgálóoldali parancsfájl használja őket), feltöltéséhez `query.parameters` , például így:</span><span class="sxs-lookup"><span data-stu-id="351e9-164">To include additional query string parameters in the server request (for example, because a custom server-side script uses them), populate `query.parameters` like so:</span></span>

<span data-ttu-id="351e9-165">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-165">**Objective-C**:</span></span>

```
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

<span data-ttu-id="351e9-166">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-166">**Swift**:</span></span>

```
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <span data-ttu-id="351e9-167"><a name="paging"></a>Hogyan: oldalméret konfigurálása</span><span class="sxs-lookup"><span data-stu-id="351e9-167"><a name="paging"></a>How to: Configure Page Size</span></span>
<span data-ttu-id="351e9-168">Az Azure Mobile Apps a lap mérete határozza meg, hogy a háttér tábla egyszerre vannak lekért rekordok száma.</span><span class="sxs-lookup"><span data-stu-id="351e9-168">With Azure Mobile Apps, the page size controls the number of records that are pulled at a time from the backend tables.</span></span> <span data-ttu-id="351e9-169">Hívása `pull` adatokat szeretné majd a batch-adatokat, a lap mérete alapján, amíg nincsenek további rekordok való lekérésére.</span><span class="sxs-lookup"><span data-stu-id="351e9-169">A call to `pull` data would then batch up data, based on this page size, until there are no more records to pull.</span></span>

<span data-ttu-id="351e9-170">Konfigurálhatja a lap mérete segítségével lehet **MSPullSettings** alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="351e9-170">It's possible to configure a page size using **MSPullSettings** as shown below.</span></span> <span data-ttu-id="351e9-171">Az alapértelmezett oldal mérete 50, és az alábbi példa 3 módosítja azt.</span><span class="sxs-lookup"><span data-stu-id="351e9-171">The default page size is 50, and the example below changes it to 3.</span></span>

<span data-ttu-id="351e9-172">Konfigurálhatja a különböző méretet a jobb teljesítmény érdekében.</span><span class="sxs-lookup"><span data-stu-id="351e9-172">You could configure a different page size for performance reasons.</span></span> <span data-ttu-id="351e9-173">Ha kis rekordok nagy száma, a magas oldalméret csökkenti a kiszolgáló üzenetváltások számát.</span><span class="sxs-lookup"><span data-stu-id="351e9-173">If you have a large number of small data records, a high page size reduces the number of server round-trips.</span></span>

<span data-ttu-id="351e9-174">Ez a beállítás csak az ügyféloldali oldalméret szabályozza.</span><span class="sxs-lookup"><span data-stu-id="351e9-174">This setting controls only the page size on the client side.</span></span> <span data-ttu-id="351e9-175">Ha az ügyfél egy nagyobb oldalméret, mint a Mobile Apps-háttéralkalmazás támogatja, a lapméretnél tárfiókonként a maximumot, a háttér támogatására van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="351e9-175">If the client asks for a larger page size than the Mobile Apps backend supports, the page size is capped at the maximum the backend is configured to support.</span></span>

<span data-ttu-id="351e9-176">Ez a beállítás akkor is a *szám* az rekordok, nem a *bájtméretnek*.</span><span class="sxs-lookup"><span data-stu-id="351e9-176">This setting is also the *number* of data records, not the *byte size*.</span></span>

<span data-ttu-id="351e9-177">Ha a ügyfél méretének növeléséhez is növelje az ajánlott méretet a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="351e9-177">If you increase the client page size, you should also increase the page size on the server.</span></span> <span data-ttu-id="351e9-178">Lásd: ["hogyan: a tábla a lapozófájl méretének módosítása"](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) ennek lépéseit.</span><span class="sxs-lookup"><span data-stu-id="351e9-178">See ["How to: Adjust the table paging size"](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) for the steps to do this.</span></span>

<span data-ttu-id="351e9-179">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-179">**Objective-C**:</span></span>

```
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                }
                           }];
```


<span data-ttu-id="351e9-180">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-180">**Swift**:</span></span>

```
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    }
}
```

## <span data-ttu-id="351e9-181"><a name="inserting"></a>Útmutató: adatok beszúrása</span><span class="sxs-lookup"><span data-stu-id="351e9-181"><a name="inserting"></a>How to: Insert Data</span></span>
<span data-ttu-id="351e9-182">Új tábla sor beszúrására, hozzon létre egy `NSDictionary` és invoke `table insert`.</span><span class="sxs-lookup"><span data-stu-id="351e9-182">To insert a new table row, create a `NSDictionary` and invoke `table insert`.</span></span> <span data-ttu-id="351e9-183">Ha [dinamikus séma] van engedélyezve, az Azure App Service mobil-háttéralkalmazást automatikusan hoz létre új oszlopok alapján a `NSDictionary`.</span><span class="sxs-lookup"><span data-stu-id="351e9-183">If [Dynamic Schema] is enabled, the Azure App Service mobile backend automatically generates new columns based on the `NSDictionary`.</span></span>

<span data-ttu-id="351e9-184">Ha `id` van nincs megadva, a háttér automatikusan hoz létre egy új egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="351e9-184">If `id` is not provided, the backend automatically generates a new unique ID.</span></span> <span data-ttu-id="351e9-185">Adja meg a saját `id` használni e-mail címek, felhasználónevek, vagy a saját egyéni értékeinek azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="351e9-185">Provide your own `id` to use email addresses, usernames, or your own custom values as ID.</span></span> <span data-ttu-id="351e9-186">Saját azonosító megadása is megkönnyítik illesztések és üzleti célú adatbázis programot.</span><span class="sxs-lookup"><span data-stu-id="351e9-186">Providing your own ID may ease joins and business-oriented database logic.</span></span>

<span data-ttu-id="351e9-187">A `result` tartalmazza az új elemet beszúrt.</span><span class="sxs-lookup"><span data-stu-id="351e9-187">The `result` contains the new item that was inserted.</span></span> <span data-ttu-id="351e9-188">Attól függően, hogy a kiszolgáló logika mellette további vagy módosított adatok milyen lett átadva a kiszolgáló képest.</span><span class="sxs-lookup"><span data-stu-id="351e9-188">Depending on your server logic, it may have additional or modified data compared to what was passed to the server.</span></span>

<span data-ttu-id="351e9-189">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-189">**Objective-C**:</span></span>

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

<span data-ttu-id="351e9-190">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-190">**Swift**:</span></span>

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

## <span data-ttu-id="351e9-191"><a name="modifying"></a>Útmutató: adatok módosítása</span><span class="sxs-lookup"><span data-stu-id="351e9-191"><a name="modifying"></a>How to: Modify Data</span></span>
<span data-ttu-id="351e9-192">Egy meglévő sor frissítése, módosítsa egy elemet, és a hívás `update`:</span><span class="sxs-lookup"><span data-stu-id="351e9-192">To update an existing row, modify an item and call `update`:</span></span>

<span data-ttu-id="351e9-193">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-193">**Objective-C**:</span></span>

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

<span data-ttu-id="351e9-194">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-194">**Swift**:</span></span>

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

<span data-ttu-id="351e9-195">Másik lehetőségként adja meg a Sorazonosító, és a frissített mező:</span><span class="sxs-lookup"><span data-stu-id="351e9-195">Alternatively, supply the row ID and the updated field:</span></span>

<span data-ttu-id="351e9-196">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-196">**Objective-C**:</span></span>

```
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

<span data-ttu-id="351e9-197">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-197">**Swift**:</span></span>

```
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

<span data-ttu-id="351e9-198">Legalább a `id` attribútumot úgy kell beállítani, amikor frissíteni.</span><span class="sxs-lookup"><span data-stu-id="351e9-198">At minimum, the `id` attribute must be set when making updates.</span></span>

## <span data-ttu-id="351e9-199"><a name="deleting"></a>Útmutató: adatok törlése</span><span class="sxs-lookup"><span data-stu-id="351e9-199"><a name="deleting"></a>How to: Delete Data</span></span>
<span data-ttu-id="351e9-200">Egy elem törléséhez meghívása `delete` a cikket:</span><span class="sxs-lookup"><span data-stu-id="351e9-200">To delete an item, invoke `delete` with the item:</span></span>

<span data-ttu-id="351e9-201">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-201">**Objective-C**:</span></span>

```
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

<span data-ttu-id="351e9-202">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-202">**Swift**:</span></span>

```
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

<span data-ttu-id="351e9-203">Azt is megteheti törölje a Sorazonosító megadásával:</span><span class="sxs-lookup"><span data-stu-id="351e9-203">Alternatively, delete by providing a row ID:</span></span>

<span data-ttu-id="351e9-204">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-204">**Objective-C**:</span></span>

```
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

<span data-ttu-id="351e9-205">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-205">**Swift**:</span></span>

```
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

<span data-ttu-id="351e9-206">Legalább a `id` attribútumot úgy kell beállítani, ha törli az elvégzése.</span><span class="sxs-lookup"><span data-stu-id="351e9-206">At minimum, the `id` attribute must be set when making deletes.</span></span>

## <span data-ttu-id="351e9-207"><a name="customapi"></a>Útmutató: egyéni API hívása</span><span class="sxs-lookup"><span data-stu-id="351e9-207"><a name="customapi"></a>How to: Call Custom API</span></span>
<span data-ttu-id="351e9-208">Egy egyéni API-val olyan háttér funkciót is elérhetővé teheti.</span><span class="sxs-lookup"><span data-stu-id="351e9-208">With a custom API, you can expose any backend functionality.</span></span> <span data-ttu-id="351e9-209">Egy tábla művelet hozzárendelése nem tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="351e9-209">It doesn't have to map to a table operation.</span></span> <span data-ttu-id="351e9-210">Nem csak akkor kapnak az üzenetkezelési teljesebb körű vezérlése, még akkor is is fejlécek olvasási vagy állították be, és módosítsa a válasz törzsében formátuma.</span><span class="sxs-lookup"><span data-stu-id="351e9-210">Not only do you gain more control over messaging, you can even read/set headers and change the response body format.</span></span> <span data-ttu-id="351e9-211">Egy egyéni API létrehozása a háttérkiszolgálón, olvassa el [egyéni API-k](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)</span><span class="sxs-lookup"><span data-stu-id="351e9-211">To learn how to create a custom API on the backend, read [Custom APIs](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)</span></span>

<span data-ttu-id="351e9-212">Egy egyéni API hívása, hívja meg a `MSClient.invokeAPI`.</span><span class="sxs-lookup"><span data-stu-id="351e9-212">To call a custom API, call `MSClient.invokeAPI`.</span></span> <span data-ttu-id="351e9-213">A kérelem és válasz tartalom JSON tekintendők.</span><span class="sxs-lookup"><span data-stu-id="351e9-213">The request and response content are treated as JSON.</span></span> <span data-ttu-id="351e9-214">Más media tárolótípus [használja más `invokeAPI` ] [ 5].</span><span class="sxs-lookup"><span data-stu-id="351e9-214">To use other media types, [use the other overload of `invokeAPI`][5].</span></span>  <span data-ttu-id="351e9-215">Annak a `GET` ahelyett, hogy kérjen egy `POST` kérelmezéséhez set paraméter `HTTPMethod` való `"GET"` és paraméter `body` való `nil` (mivel a GET-kérésekhez nincs az üzenet törzse.) Ha az egyéni API támogatja az egyéb HTTP-műveletek, módosítsa `HTTPMethod` megfelelően.</span><span class="sxs-lookup"><span data-stu-id="351e9-215">To make a `GET` request instead of a `POST` request, set parameter `HTTPMethod` to `"GET"` and parameter `body` to `nil` (since GET requests do not have message bodies.) If your custom API supports other HTTP verbs, change `HTTPMethod` appropriately.</span></span>

<span data-ttu-id="351e9-216">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-216">**Objective-C**:</span></span>

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

<span data-ttu-id="351e9-217">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-217">**Swift**:</span></span>

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

## <span data-ttu-id="351e9-218"><a name="templates"></a>Útmutató: regisztráció leküldéses sablonok platformfüggetlen értesítések küldéséhez</span><span class="sxs-lookup"><span data-stu-id="351e9-218"><a name="templates"></a>How to: Register push templates to send cross-platform notifications</span></span>
<span data-ttu-id="351e9-219">Sablonok regisztrálásához adja át a sablon is van a **client.push registerDeviceToken** ügyfélalkalmazás metódust.</span><span class="sxs-lookup"><span data-stu-id="351e9-219">To register templates, pass templates with your **client.push registerDeviceToken** method in your client app.</span></span>

<span data-ttu-id="351e9-220">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-220">**Objective-C**:</span></span>

```
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

<span data-ttu-id="351e9-221">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-221">**Swift**:</span></span>

```
    client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
        if let err = error {
            print("ERROR ", err)
        }
    })
```

<span data-ttu-id="351e9-222">A sablonok NSDictionary típusú, és tartalmazhat több sablon a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="351e9-222">Your templates are of type NSDictionary and can contain multiple templates in the following format:</span></span>

<span data-ttu-id="351e9-223">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-223">**Objective-C**:</span></span>

```
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

<span data-ttu-id="351e9-224">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-224">**Swift**:</span></span>

```
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

<span data-ttu-id="351e9-225">Az összes címke a kérelemből a biztonság üres karaktert törölni.</span><span class="sxs-lookup"><span data-stu-id="351e9-225">All tags are stripped from the request for security.</span></span>  <span data-ttu-id="351e9-226">Címkék hozzáadása a telepítésekkel és sablonok telepítések belül, lásd: [használható a .NET-háttérrendszer server SDK az Azure Mobile Apps a][4].</span><span class="sxs-lookup"><span data-stu-id="351e9-226">To add tags to installations or templates within installations, see [Work with the .NET backend server SDK for Azure Mobile Apps][4].</span></span>  <span data-ttu-id="351e9-227">A regisztrált sablonokkal értesítések küldéséhez, együttműködve [Notification Hubs API-k][3].</span><span class="sxs-lookup"><span data-stu-id="351e9-227">To send notifications using these registered templates, work with [Notification Hubs APIs][3].</span></span>

## <span data-ttu-id="351e9-228"><a name="errors"></a>Hogyan: hibák kezelésének</span><span class="sxs-lookup"><span data-stu-id="351e9-228"><a name="errors"></a>How to: Handle Errors</span></span>
<span data-ttu-id="351e9-229">Az Azure App Service mobil-háttéralkalmazást hívásakor a befejezési blokk tartalmaz egy `NSError` paraméter.</span><span class="sxs-lookup"><span data-stu-id="351e9-229">When you call an Azure App Service mobile backend, the completion block contains an `NSError` parameter.</span></span> <span data-ttu-id="351e9-230">Ha hiba lép fel, a paraméter nem üres.</span><span class="sxs-lookup"><span data-stu-id="351e9-230">When an error occurs, this parameter is non-nil.</span></span> <span data-ttu-id="351e9-231">A kódban ellenőrizze a paraméter és a hiba, ha szükséges, kezelni, ahogyan az a megelőző kódrészletek.</span><span class="sxs-lookup"><span data-stu-id="351e9-231">In your code, you should check this parameter and handle the error as needed, as demonstrated in the preceding code snippets.</span></span>

<span data-ttu-id="351e9-232">A fájl [ `<WindowsAzureMobileServices/MSError.h>` ] [ 6] határozza meg a állandók `MSErrorResponseKey`, `MSErrorRequestKey`, és `MSErrorServerItemKey`.</span><span class="sxs-lookup"><span data-stu-id="351e9-232">The file [`<WindowsAzureMobileServices/MSError.h>`][6] defines the constants `MSErrorResponseKey`, `MSErrorRequestKey`, and `MSErrorServerItemKey`.</span></span> <span data-ttu-id="351e9-233">A hibával kapcsolatos további adatok eléréséhez:</span><span class="sxs-lookup"><span data-stu-id="351e9-233">To get more data related to the error:</span></span>

<span data-ttu-id="351e9-234">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-234">**Objective-C**:</span></span>

```
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

<span data-ttu-id="351e9-235">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-235">**Swift**:</span></span>

```
let serverItem = error.userInfo[MSErrorServerItemKey]
```

<span data-ttu-id="351e9-236">Emellett a fájl meghatározza, hogy minden hibakód állandókat:</span><span class="sxs-lookup"><span data-stu-id="351e9-236">In addition, the file defines constants for each error code:</span></span>

<span data-ttu-id="351e9-237">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-237">**Objective-C**:</span></span>

```
if (error.code == MSErrorPreconditionFailed) {
```

<span data-ttu-id="351e9-238">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-238">**Swift**:</span></span>

```
if (error.code == MSErrorPreconditionFailed) {
```

## <span data-ttu-id="351e9-239"><a name="adal"></a>Útmutató: az Active Directory Authentication Library a felhasználók hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="351e9-239"><a name="adal"></a>How to: Authenticate users with the Active Directory Authentication Library</span></span>
<span data-ttu-id="351e9-240">Az Active Directory Authentication Library (ADAL) segítségével a felhasználók jelentkezzen be az alkalmazás Azure Active Directory használatával.</span><span class="sxs-lookup"><span data-stu-id="351e9-240">You can use the Active Directory Authentication Library (ADAL) to sign users into your application using Azure Active Directory.</span></span> <span data-ttu-id="351e9-241">Ügyfél adatfolyam-hitelesítéshez az identitásszolgáltató SDK használata helyett a `loginWithProvider:completion:` metódust.</span><span class="sxs-lookup"><span data-stu-id="351e9-241">Client flow authentication using an identity provider SDK is preferable to using the `loginWithProvider:completion:` method.</span></span>  <span data-ttu-id="351e9-242">Ügyfél folyamata hitelesítést biztosít több natív UX abba, és lehetővé teszi, hogy további testreszabási.</span><span class="sxs-lookup"><span data-stu-id="351e9-242">Client flow authentication provides a more native UX feel and allows for additional customization.</span></span>

1. <span data-ttu-id="351e9-243">A mobil-háttéralkalmazás számára az AAD-bejelentkezés konfigurálása a következő a [App Service konfigurálása az Active Directory bejelentkezési] [ 7] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="351e9-243">Configure your mobile app backend for AAD sign-in by following the [How to configure App Service for Active Directory login][7] tutorial.</span></span> <span data-ttu-id="351e9-244">Ügyeljen arra, hogy a natív ügyfélalkalmazás regisztrációján választható lépés elvégzése után.</span><span class="sxs-lookup"><span data-stu-id="351e9-244">Make sure to complete the optional step of registering a native client application.</span></span> <span data-ttu-id="351e9-245">Az iOS, azt javasoljuk, hogy az átirányítási URI megadása az űrlap `<app-scheme>://<bundle-id>`.</span><span class="sxs-lookup"><span data-stu-id="351e9-245">For iOS, we recommend that the redirect URI is of the form `<app-scheme>://<bundle-id>`.</span></span> <span data-ttu-id="351e9-246">További információkért lásd: a [ADAL iOS gyors üzembe helyezés][8].</span><span class="sxs-lookup"><span data-stu-id="351e9-246">For more information, see the [ADAL iOS quickstart][8].</span></span>
2. <span data-ttu-id="351e9-247">Telepítse a Cocoapods segítségével adal-t.</span><span class="sxs-lookup"><span data-stu-id="351e9-247">Install ADAL using Cocoapods.</span></span> <span data-ttu-id="351e9-248">A következő definícióját tartalmazza a Podfile szerkesztése cseréje **YOUR-projekt** az Xcode-projektjéhez nevét:</span><span class="sxs-lookup"><span data-stu-id="351e9-248">Edit your Podfile to include the following definition, replacing **YOUR-PROJECT** with the name of your Xcode project:</span></span>

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   <span data-ttu-id="351e9-249">és fogyasztanak:</span><span class="sxs-lookup"><span data-stu-id="351e9-249">and the Pod:</span></span>

        pod 'ADALiOS'
3. <span data-ttu-id="351e9-250">Futtassa a terminál használatával `pod install` a könyvtárból, amely tartalmazza a projekthez, és nyissa meg a létrehozott Xcode-munkaterületet (ne a projektre).</span><span class="sxs-lookup"><span data-stu-id="351e9-250">Using the Terminal, run `pod install` from the directory containing your project, and then open the generated Xcode workspace (not the project).</span></span>
4. <span data-ttu-id="351e9-251">Adja hozzá a következő kódot az alkalmazás használ nyelvének megfelelően.</span><span class="sxs-lookup"><span data-stu-id="351e9-251">Add the following code to your application, according to the language you are using.</span></span> <span data-ttu-id="351e9-252">Minden ellenőrizze a cserét:</span><span class="sxs-lookup"><span data-stu-id="351e9-252">In each, make these replacements:</span></span>

   * <span data-ttu-id="351e9-253">Cserélje le **INSERT-SZOLGÁLTATÓ-Itt** nevű, a bérlő, amelyben az alkalmazás üzembe.</span><span class="sxs-lookup"><span data-stu-id="351e9-253">Replace **INSERT-AUTHORITY-HERE** with the name of the tenant in which you provisioned your application.</span></span> <span data-ttu-id="351e9-254">A következő formátumban kell megadni https://login.microsoftonline.com/contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="351e9-254">The format should be https://login.microsoftonline.com/contoso.onmicrosoft.com.</span></span> <span data-ttu-id="351e9-255">Ez az érték a [klasszikus Azure portálon] az Azure Active Directory tartományi lapján másolhatók.</span><span class="sxs-lookup"><span data-stu-id="351e9-255">This value can be copied from the Domain tab in your Azure Active Directory in the [Azure classic portal].</span></span>
   * <span data-ttu-id="351e9-256">Cserélje le **INSERT-erőforrás-azonosító-Itt** az ügyfél-azonosító a mobil-háttéralkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="351e9-256">Replace **INSERT-RESOURCE-ID-HERE** with the client ID for your mobile app backend.</span></span> <span data-ttu-id="351e9-257">Ezt úgy szerezheti be az ügyfél-azonosító a **speciális** lap **Azure Active Directory beállításai** a portálon.</span><span class="sxs-lookup"><span data-stu-id="351e9-257">You can obtain the client ID from the **Advanced** tab under **Azure Active Directory Settings** in the portal.</span></span>
   * <span data-ttu-id="351e9-258">Cserélje le **INSERT-ügyfél-azonosító-Itt** , az ügyfél-Azonosítót a natív ügyfélalkalmazás másolta.</span><span class="sxs-lookup"><span data-stu-id="351e9-258">Replace **INSERT-CLIENT-ID-HERE** with the client ID you copied from the native client application.</span></span>
   * <span data-ttu-id="351e9-259">Cserélje le **INSERT-REDIRECT-URI-Itt** a hellyel való */.auth/login/done* végpont, a HTTPS protokollt használ.</span><span class="sxs-lookup"><span data-stu-id="351e9-259">Replace **INSERT-REDIRECT-URI-HERE** with your site's */.auth/login/done* endpoint, using the HTTPS scheme.</span></span> <span data-ttu-id="351e9-260">Ez az érték a következőképpen kell kinéznie *https://contoso.azurewebsites.net/.auth/login/done*.</span><span class="sxs-lookup"><span data-stu-id="351e9-260">This value should be similar to *https://contoso.azurewebsites.net/.auth/login/done*.</span></span>

<span data-ttu-id="351e9-261">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-261">**Objective-C**:</span></span>

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


<span data-ttu-id="351e9-262">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-262">**Swift**:</span></span>

    // add the following imports to your bridging header:
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

## <span data-ttu-id="351e9-263"><a name="facebook-sdk"></a>Útmutató: a Facebook SDK iOS-hez a felhasználók hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="351e9-263"><a name="facebook-sdk"></a>How to: Authenticate users with the Facebook SDK for iOS</span></span>
<span data-ttu-id="351e9-264">A Facebook SDK iOS segítségével a felhasználók jelentkezzen be az alkalmazás Facebook használatával.</span><span class="sxs-lookup"><span data-stu-id="351e9-264">You can use the Facebook SDK for iOS to sign users into your application using Facebook.</span></span>  <span data-ttu-id="351e9-265">A folyamat ügyfél-hitelesítés használata használata helyett a `loginWithProvider:completion:` metódust.</span><span class="sxs-lookup"><span data-stu-id="351e9-265">Using a client flow authentication is preferable to using the `loginWithProvider:completion:` method.</span></span>  <span data-ttu-id="351e9-266">Az ügyfél-hitelesítési folyamat több natív UX abba biztosít, és lehetővé teszi, hogy további testreszabási.</span><span class="sxs-lookup"><span data-stu-id="351e9-266">The client flow authentication provides a more native UX feel and allows for additional customization.</span></span>

1. <span data-ttu-id="351e9-267">A mobil-háttéralkalmazás a Facebook-bejelentkezés konfigurálása a következő a [App Service konfigurálása Facebook bejelentkezési] [ 9] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="351e9-267">Configure your mobile app backend for Facebook sign-in by following the [How to configure App Service for Facebook login][9] tutorial.</span></span>
2. <span data-ttu-id="351e9-268">A Facebook SDK iOS-követve telepítse a [Facebook SDK iOS - első lépések] [ 10] dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="351e9-268">Install the Facebook SDK for iOS by following the [Facebook SDK for iOS - Getting Started][10] documentation.</span></span> <span data-ttu-id="351e9-269">Alkalmazás létrehozása, helyett a meglévő regisztrációs az iOS platform adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="351e9-269">Instead of creating an app, you can add the iOS platform to your existing registration.</span></span>
3. <span data-ttu-id="351e9-270">Az alkalmazás delegált néhány Objective-C kódjának Facebook tartozó dokumentáció tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="351e9-270">Facebook's documentation includes some Objective-C code in the App Delegate.</span></span> <span data-ttu-id="351e9-271">Ha használ **Swift**, a következő fordítások AppDelegate.swift használható:</span><span class="sxs-lookup"><span data-stu-id="351e9-271">If you are using **Swift**, you can use the following translations for AppDelegate.swift:</span></span>

        // Add the following import to your bridging header:
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
4. <span data-ttu-id="351e9-272">Hozzáadásán `FBSDKCoreKit.framework` a projektbe egy hivatkozást is meg kell adni `FBSDKLoginKit.framework` azonos módon.</span><span class="sxs-lookup"><span data-stu-id="351e9-272">In addition to adding `FBSDKCoreKit.framework` to your project, also add a reference to `FBSDKLoginKit.framework` in the same way.</span></span>
5. <span data-ttu-id="351e9-273">Adja hozzá a következő kódot az alkalmazás használ nyelvének megfelelően.</span><span class="sxs-lookup"><span data-stu-id="351e9-273">Add the following code to your application, according to the language you are using.</span></span>

<span data-ttu-id="351e9-274">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-274">**Objective-C**:</span></span>

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

<span data-ttu-id="351e9-275">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-275">**Swift**:</span></span>

    // Add the following imports to your bridging header:
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

## <span data-ttu-id="351e9-276"><a name="twitter-fabric"></a>Útmutató: az IOS-es Twitter-hálóval felhasználók hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="351e9-276"><a name="twitter-fabric"></a>How to: Authenticate users with Twitter Fabric for iOS</span></span>
<span data-ttu-id="351e9-277">IOS-háló segítségével felhasználók jelentkezzen be az alkalmazás Twitter használatával.</span><span class="sxs-lookup"><span data-stu-id="351e9-277">You can use Fabric for iOS to sign users into your application using Twitter.</span></span> <span data-ttu-id="351e9-278">Ügyfél folyamata a hitelesítés az használata helyett a `loginWithProvider:completion:` metódus, mert több natív UX abba biztosít, és lehetővé teszi, hogy további testreszabási.</span><span class="sxs-lookup"><span data-stu-id="351e9-278">Client Flow authentication is preferable to using the `loginWithProvider:completion:` method, as it provides a more native UX feel and allows for additional customization.</span></span>

1. <span data-ttu-id="351e9-279">A mobil-háttéralkalmazás számára Twitter-bejelentkezés konfigurálása a következő a [App Service konfigurálása Twitter bejelentkezési](app-service-mobile-how-to-configure-twitter-authentication.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="351e9-279">Configure your mobile app backend for Twitter sign-in by following the [How to configure App Service for Twitter login](app-service-mobile-how-to-configure-twitter-authentication.md) tutorial.</span></span>
2. <span data-ttu-id="351e9-280">Háló hozzáadása a projekthez követve a [(iOS) – első lépések a háló] dokumentáció és TwitterKit beállítása.</span><span class="sxs-lookup"><span data-stu-id="351e9-280">Add Fabric to your project by following the [Fabric for iOS - Getting Started] documentation and setting up TwitterKit.</span></span>

   > [!NOTE]
   > <span data-ttu-id="351e9-281">Alapértelmezés szerint háló létrehoz egy Twitter-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="351e9-281">By default, Fabric creates a Twitter application for you.</span></span> <span data-ttu-id="351e9-282">Elkerülheti, ha regisztrálja a fogyasztói kulcs és a fogyasztói titkos kulcsot, korábban létrehozott az alábbi kódrészleteket alkalmazások létrehozására.</span><span class="sxs-lookup"><span data-stu-id="351e9-282">You can avoid creating an application by registering the Consumer Key and Consumer Secret you created earlier using the following code snippets.</span></span>    <span data-ttu-id="351e9-283">Másik lehetőségként a fogyasztói kulcs és a fogyasztói titkos értékek, amelyek látható értékekkel biztosítanak az App Service lecserélheti a [háló irányítópult].</span><span class="sxs-lookup"><span data-stu-id="351e9-283">Alternatively, you can replace the Consumer Key and Consumer Secret values that you provide to App Service with the values you see in the [Fabric Dashboard].</span></span> <span data-ttu-id="351e9-284">Ha ezt a lehetőséget választja, ügyeljen arra, hogy a visszahívási URL-Címének beállítása a helyőrző értékre, mint például `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.</span><span class="sxs-lookup"><span data-stu-id="351e9-284">If you choose this option, be sure to set the callback URL to a placeholder value, such as `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.</span></span>
   >
   >

    <span data-ttu-id="351e9-285">Ha a korábban létrehozott titkos kulcsok használatát választja, adja hozzá a következő kódot az alkalmazás delegált:</span><span class="sxs-lookup"><span data-stu-id="351e9-285">If you choose to use the secrets you created earlier, add the following code to your App Delegate:</span></span>

    <span data-ttu-id="351e9-286">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-286">**Objective-C**:</span></span>

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

    <span data-ttu-id="351e9-287">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-287">**Swift**:</span></span>

        import Fabric
        import TwitterKit
        // ...
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
            Fabric.with([Twitter.self])
            // Add any custom logic here.
            return true
        }
3. <span data-ttu-id="351e9-288">Adja hozzá a következő kódot az alkalmazás használ nyelvének megfelelően.</span><span class="sxs-lookup"><span data-stu-id="351e9-288">Add the following code to your application, according to the language you are using.</span></span>

<span data-ttu-id="351e9-289">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-289">**Objective-C**:</span></span>

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

<span data-ttu-id="351e9-290">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-290">**Swift**:</span></span>

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

## <span data-ttu-id="351e9-291"><a name="google-sdk"></a>Útmutató: a Google-bejelentkezés SDK iOS-felhasználók hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="351e9-291"><a name="google-sdk"></a>How to: Authenticate users with the Google Sign-In SDK for iOS</span></span>
<span data-ttu-id="351e9-292">A Google-bejelentkezés SDK iOS segítségével a felhasználók jelentkezzen be az alkalmazás a Google-fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="351e9-292">You can use the Google Sign-In SDK for iOS to sign users into your application using a Google account.</span></span>  <span data-ttu-id="351e9-293">Google nemrég jelentette be az OAuth-biztonsági házirendek módosításai.</span><span class="sxs-lookup"><span data-stu-id="351e9-293">Google recently announced changes to their OAuth security policies.</span></span>  <span data-ttu-id="351e9-294">A módosítások a házirend megköveteli a Google SDK a továbbiakban.</span><span class="sxs-lookup"><span data-stu-id="351e9-294">These policy changes will require the use of the Google SDK in the future.</span></span>

1. <span data-ttu-id="351e9-295">A mobil-háttéralkalmazás a Google-bejelentkezés konfigurálása a következő a [App Service konfigurálása Google bejelentkezési](app-service-mobile-how-to-configure-google-authentication.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="351e9-295">Configure your mobile app backend for Google sign-in by following the [How to configure App Service for Google login](app-service-mobile-how-to-configure-google-authentication.md) tutorial.</span></span>
2. <span data-ttu-id="351e9-296">Követve telepítse a Google SDK iOS-hez a [Google bejelentkezhet az iOS - Start integrálása](https://developers.google.com/identity/sign-in/ios/start-integrating) dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="351e9-296">Install the Google SDK for iOS by following the [Google Sign-In for iOS - Start integrating](https://developers.google.com/identity/sign-in/ios/start-integrating) documentation.</span></span> <span data-ttu-id="351e9-297">A "Hitelesítéséhez és a háttérkiszolgáló" szakaszt kihagyhatja.</span><span class="sxs-lookup"><span data-stu-id="351e9-297">You may skip the "Authenticate with a Backend Server" section.</span></span>
3. <span data-ttu-id="351e9-298">Adja hozzá a következőket a delegált `signIn:didSignInForUser:withError:` módszert használ nyelvének megfelelően.</span><span class="sxs-lookup"><span data-stu-id="351e9-298">Add the following to your delegate's `signIn:didSignInForUser:withError:` method, according to the language you are using.</span></span>

<span data-ttu-id="351e9-299">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-299">**Objective-C**:</span></span>

        NSDictionary *payload = @{
                                  @"id_token":user.authentication.idToken,
                                  @"authorization_code":user.serverAuthCode
                                  };

        [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
            // ...
        }];

<span data-ttu-id="351e9-300">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-300">**Swift**:</span></span>

        let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
        client.loginWithProvider("google", token: payload) { (user, error) in
            // ...
        }

1. <span data-ttu-id="351e9-301">Győződjön meg arról is adja hozzá a következőt `application:didFinishLaunchingWithOptions:` a az alkalmazás delegált cseréje a "SERVER_CLIENT_ID" App Service konfigurálása az 1. lépésben használt ugyanazzal az azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="351e9-301">Make sure you also add the following to `application:didFinishLaunchingWithOptions:` in your app delegate, replacing "SERVER_CLIENT_ID" with the same ID that you used to configure App Service in step 1.</span></span>

<span data-ttu-id="351e9-302">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-302">**Objective-C**:</span></span>

         [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";

 <span data-ttu-id="351e9-303">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-303">**Swift**:</span></span>

        GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"


1. <span data-ttu-id="351e9-304">Adja hozzá a következő kódot az alkalmazást egy UIViewController, amely megvalósítja a `GIDSignInUIDelegate` protokollt használ nyelvének megfelelően.</span><span class="sxs-lookup"><span data-stu-id="351e9-304">Add the following code to your application in a UIViewController that implements the `GIDSignInUIDelegate` protocol, according to the language you are using.</span></span>  <span data-ttu-id="351e9-305">Be van jelentkezve mielőtt újra éppen bejelentkezett, és nem kell újra a hitelesítő adatainak megadását, bár látja-e a hozzájárulási párbeszédablak.</span><span class="sxs-lookup"><span data-stu-id="351e9-305">You are signed out before being signed in again, and although you don't need to enter your credentials again, you see a consent dialog.</span></span>  <span data-ttu-id="351e9-306">Ez a metódus csak hívható, amikor a munkamenet-token érvényessége lejárt.</span><span class="sxs-lookup"><span data-stu-id="351e9-306">Only call this method when the session token has expired.</span></span>

   <span data-ttu-id="351e9-307">**Objective-C**:</span><span class="sxs-lookup"><span data-stu-id="351e9-307">**Objective-C**:</span></span>

       #import <Google/SignIn.h>
       // ...
       - (void)authenticate
       {
               [GIDSignIn sharedInstance].uiDelegate = self;
               [[GIDSignIn sharedInstance] signOut];
               [[GIDSignIn sharedInstance] signIn];
        }

   <span data-ttu-id="351e9-308">**SWIFT**:</span><span class="sxs-lookup"><span data-stu-id="351e9-308">**Swift**:</span></span>

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
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #table-reference
[How to: Query data from a mobile service]: #querying
[Filter returned data]: #filtering
[Sort returned data]: #sorting
[Return data in pages]: #paging
[Select specific columns]: #selecting
[How to: Bind data to the user interface]: #binding
[How to: Insert data into a mobile service]: #inserting
[How to: Modify data in a mobile service]: #modifying
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching-tokens
[How to: Upload images and large files]: #blobs
[How to: Handle errors]: #errors
[How to: Design unit tests]: #unit-testing
[How to: Customize the client]: #customizing
[Customize request headers]: #custom-headers
[Customize data type serialization]: #custom-serialization
[Next Steps]: #next-steps
[How to: Use MSQuery]: #query-object

<!-- Images. -->

<!-- URLs. -->
<span data-ttu-id="351e9-309">[Azure Mobile Apps gyors üzembe helyezés]: app-service-mobile-ios-get-started.md</span><span class="sxs-lookup"><span data-stu-id="351e9-309">[Azure Mobile Apps Quick Start]: app-service-mobile-ios-get-started.md</span></span>

[Add Mobile Services to Existing App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode

[Handling Expired Tokens]: http://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: http://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: http://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts to authorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
<span data-ttu-id="351e9-310">[dinamikus séma]: http://go.microsoft.com/fwlink/p/?LinkId=296271</span><span class="sxs-lookup"><span data-stu-id="351e9-310">[Dynamic Schema]: http://go.microsoft.com/fwlink/p/?LinkId=296271</span></span>
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: http://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: http://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI to manage Mobile Services tables]: /cli/azure/get-started-with-az-cli2
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

<span data-ttu-id="351e9-311">[háló irányítópult]: https://www.fabric.io/home</span><span class="sxs-lookup"><span data-stu-id="351e9-311">[Fabric Dashboard]: https://www.fabric.io/home</span></span>
<span data-ttu-id="351e9-312">[(iOS) – első lépések a háló]: https://docs.fabric.io/ios/fabric/getting-started.html</span><span class="sxs-lookup"><span data-stu-id="351e9-312">[Fabric for iOS - Getting Started]: https://docs.fabric.io/ios/fabric/getting-started.html</span></span>
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
