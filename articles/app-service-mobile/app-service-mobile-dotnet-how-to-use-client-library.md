---
title: "az App Service Mobile Apps hello aaaWorking ügyféloldali kódtár által felügyelt (Windows |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse egy .NET-ügyfél Azure App Service Mobile Apps Windows és a Xamarin-alkalmazásokat."
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 0280785c-e027-4e0d-aaf2-6f155e5a6197
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 01/04/2017
ms.author: glenga
ms.openlocfilehash: b056e606b19406398f5b6faabb0931ad651125e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-managed-client-for-azure-mobile-apps"></a><span data-ttu-id="cd2a3-103">Hogyan kezeli a toouse hello az Azure Mobile Apps-ügyfél</span><span class="sxs-lookup"><span data-stu-id="cd2a3-103">How toouse hello managed client for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

## <a name="overview"></a><span data-ttu-id="cd2a3-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="cd2a3-104">Overview</span></span>
<span data-ttu-id="cd2a3-105">Ez az útmutató bemutatja, hogyan kezeli a tooperform szolgáltatást használó általános forgatókönyvhöz hello az ügyféloldali kódtára a Azure App Service Mobile Apps for Windows és a Xamarin-alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-105">This guide shows you how tooperform common scenarios using hello managed client library for Azure App Service Mobile Apps for Windows and Xamarin apps.</span></span> <span data-ttu-id="cd2a3-106">Ha új tooMobile alkalmazások, érdemes lehet első épp hello [Azure Mobile Apps gyors üzembe helyezés] [ 1] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-106">If you are new tooMobile Apps, you should consider first completing hello [Azure Mobile Apps quickstart][1] tutorial.</span></span> <span data-ttu-id="cd2a3-107">Az útmutató azt összpontosítani hello ügyféloldali SDK kezeli.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-107">In this guide, we focus on hello client-side managed SDK.</span></span> <span data-ttu-id="cd2a3-108">További részletek hello kiszolgálóoldali SDK-k a Mobile Apps, hello hello dokumentációjában toolearn [.NET SDK-kiszolgáló] [ 2] vagy a [Node.js Server SDK] [ 3].</span><span class="sxs-lookup"><span data-stu-id="cd2a3-108">toolearn more about hello server-side SDKs for Mobile Apps, see hello documentation for hello [.NET Server SDK][2] or the [Node.js Server SDK][3].</span></span>

## <a name="reference-documentation"></a><span data-ttu-id="cd2a3-109">Segédanyagok</span><span class="sxs-lookup"><span data-stu-id="cd2a3-109">Reference documentation</span></span>
<span data-ttu-id="cd2a3-110">hello hello ügyfél SDK referenciadokumentációt tartalmaz a következő helyen található: [Azure Mobile Apps .NET ügyfél hivatkozási][4].</span><span class="sxs-lookup"><span data-stu-id="cd2a3-110">hello reference documentation for hello client SDK is located here: [Azure Mobile Apps .NET client reference][4].</span></span>
<span data-ttu-id="cd2a3-111">Hello is található több ügyfél mintát [Azure-minták GitHub-tárházban][5].</span><span class="sxs-lookup"><span data-stu-id="cd2a3-111">You can also find several client samples in hello [Azure-Samples GitHub repository][5].</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="cd2a3-112">A támogatott platformok</span><span class="sxs-lookup"><span data-stu-id="cd2a3-112">Supported Platforms</span></span>
<span data-ttu-id="cd2a3-113">hello .NET Platform hello a következő platformokat támogatja:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-113">hello .NET Platform supports hello following platforms:</span></span>

* <span data-ttu-id="cd2a3-114">Xamarin Android API 19 kiadását – 24 (KitKat nugát keresztül)</span><span class="sxs-lookup"><span data-stu-id="cd2a3-114">Xamarin Android releases for API 19 through 24 (KitKat through Nougat)</span></span>
* <span data-ttu-id="cd2a3-115">Xamarin iOS kiadását iOS 8.0-s és újabb verziók</span><span class="sxs-lookup"><span data-stu-id="cd2a3-115">Xamarin iOS releases for iOS versions 8.0 and later</span></span>
* <span data-ttu-id="cd2a3-116">Az univerzális Windows Platform</span><span class="sxs-lookup"><span data-stu-id="cd2a3-116">Universal Windows Platform</span></span>
* <span data-ttu-id="cd2a3-117">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="cd2a3-117">Windows Phone 8.1</span></span>
* <span data-ttu-id="cd2a3-118">Windows Phone 8.0 kivételével a Silverlight alkalmazások részére</span><span class="sxs-lookup"><span data-stu-id="cd2a3-118">Windows Phone 8.0 except for Silverlight applications</span></span>

<span data-ttu-id="cd2a3-119">hello "server-folyamat" hitelesítési egy webes nézet jelenik meg a felhasználói felület hello használ.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-119">hello "server-flow" authentication uses a WebView for hello presented UI.</span></span>  <span data-ttu-id="cd2a3-120">Ha hello eszköz nem tudja toopresent webes nézet felhasználói Felületet, más hitelesítési módszert van szükség.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-120">If hello device is not able toopresent a WebView UI, then other methods of authentication are needed.</span></span>  <span data-ttu-id="cd2a3-121">Ez az SDK így nem alkalmas figyelési típusú vagy hasonló módon korlátozott eszközök.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-121">This SDK is thus not suitable for Watch-type or similarly restricted devices.</span></span>

## <span data-ttu-id="cd2a3-122"><a name="setup"></a>A telepítő és Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cd2a3-122"><a name="setup"></a>Setup and Prerequisites</span></span>
<span data-ttu-id="cd2a3-123">Feltételezzük, hogy már létrehozott és közzétett mobilalkalmazás háttérprojekt, amely legalább egy olyan táblát tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-123">We assume that you have already created and published your Mobile App backend project, which includes at least one table.</span></span>  <span data-ttu-id="cd2a3-124">A jelen témakörben használt hello kódban hello tábla nevű `TodoItem` és van-e a következő oszlopok hello: `Id`, `Text`, és `Complete`.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-124">In hello code used in this topic, hello table is named `TodoItem` and it has hello following columns: `Id`, `Text`, and `Complete`.</span></span> <span data-ttu-id="cd2a3-125">Ez a táblázat hello ugyanaz a tábla létrehozása, amikor befejezte a [Azure Mobile Apps gyors üzembe helyezés][1].</span><span class="sxs-lookup"><span data-stu-id="cd2a3-125">This table is hello same table created when you complete the [Azure Mobile Apps quickstart][1].</span></span>

<span data-ttu-id="cd2a3-126">hello megfelelő típusos ügyféloldali C# típus a következő osztály hello:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-126">hello corresponding typed client-side type in C# is hello following class:</span></span>

```
public class TodoItem
{
    public string Id { get; set; }

    [JsonProperty(PropertyName = "text")]
    public string Text { get; set; }

    [JsonProperty(PropertyName = "complete")]
    public bool Complete { get; set; }
}
```

<span data-ttu-id="cd2a3-127">Hello [JsonPropertyAttribute] [ 6] használt toodefine hello van *PropertyName* hello ügyfél és a hello tábla mező közötti megfeleltetés.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-127">hello [JsonPropertyAttribute][6] is used toodefine hello *PropertyName* mapping between hello client field and hello table field.</span></span>

<span data-ttu-id="cd2a3-128">Hogyan toocreate táblák a Mobile Apps-háttéralkalmazásának toolearn lásd: hello [.NET Server SDK témakörben] [ 7] vagy hello [Node.js Server SDK témakörben] [ 8] .</span><span class="sxs-lookup"><span data-stu-id="cd2a3-128">toolearn how toocreate tables in your Mobile Apps backend, see hello [.NET Server SDK topic][7] or hello [Node.js Server SDK topic][8].</span></span> <span data-ttu-id="cd2a3-129">Ha a mobilalkalmazás háttérrendszerének hozott létre a hello Azure portál használatával hello gyors üzembe helyezés, használhatja hello **könnyen táblák** hello beállítása [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="cd2a3-129">If you created your Mobile App backend in hello Azure portal using hello QuickStart, you can also use hello **Easy tables** setting in hello [Azure portal].</span></span>

### <a name="how-to-install-hello-managed-client-sdk-package"></a><span data-ttu-id="cd2a3-130">Útmutató: telepítés hello felügyelt ügyfél SDK-csomagot</span><span class="sxs-lookup"><span data-stu-id="cd2a3-130">How to: Install hello managed client SDK package</span></span>
<span data-ttu-id="cd2a3-131">Használja a következő módszerek tooinstall hello hello felügyelt ügyfél SDK-csomagot a Mobile Apps [NuGet][9]:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-131">Use one of hello following methods tooinstall hello managed client SDK package for Mobile Apps from [NuGet][9]:</span></span>

* <span data-ttu-id="cd2a3-132">**A Visual Studio** kattintson jobb gombbal a projektre, kattintson a **NuGet-csomagok kezelése**, keressen a hello `Microsoft.Azure.Mobile.Client` csomagot, majd kattintson az **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-132">**Visual Studio** Right-click your project, click **Manage NuGet Packages**, search for hello `Microsoft.Azure.Mobile.Client` package, then click **Install**.</span></span>
* <span data-ttu-id="cd2a3-133">**Xamarin Studio** kattintson jobb gombbal a projektre, kattintson a **Hozzáadás** > **NuGet-csomagok hozzáadása**, keressen a hello `Microsoft.Azure.Mobile.Client `csomagot, majd kattintson a **hozzáadása Csomag**.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-133">**Xamarin Studio** Right-click your project, click **Add** > **Add NuGet Packages**, search for hello `Microsoft.Azure.Mobile.Client `package, and then click **Add Package**.</span></span>

<span data-ttu-id="cd2a3-134">A fő tevékenységnél fájlban, ne feledje tooadd hello következő **használatával** utasítást:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-134">In your main activity file, remember tooadd hello following **using** statement:</span></span>

```
using Microsoft.WindowsAzure.MobileServices;
```

### <span data-ttu-id="cd2a3-135"><a name="symbolsource"></a>Útmutató: a Visual Studio hibakeresési szimbólumok használata</span><span class="sxs-lookup"><span data-stu-id="cd2a3-135"><a name="symbolsource"></a>How to: Work with debug symbols in Visual Studio</span></span>
<span data-ttu-id="cd2a3-136">hello szimbólumok hello Microsoft.Azure.Mobile névtér érhetők el a [SymbolSource][10].</span><span class="sxs-lookup"><span data-stu-id="cd2a3-136">hello symbols for hello Microsoft.Azure.Mobile namespace are available on [SymbolSource][10].</span></span>  <span data-ttu-id="cd2a3-137">Tekintse meg a toothe [SymbolSource utasításokat] [ 11] toointegrate SymbolSource a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-137">Refer toothe [SymbolSource instructions][11] toointegrate SymbolSource with Visual Studio.</span></span>

## <span data-ttu-id="cd2a3-138"><a name="create-client"></a>Hello Mobile Apps-ügyfél létrehozása</span><span class="sxs-lookup"><span data-stu-id="cd2a3-138"><a name="create-client"></a>Create hello Mobile Apps client</span></span>
<span data-ttu-id="cd2a3-139">hello alábbi kód létrehoz hello [MobileServiceClient] [ 12] objektum, amely használt tooaccess a mobilalkalmazás háttérrendszerének.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-139">hello following code creates hello [MobileServiceClient][12] object that is used tooaccess your Mobile App backend.</span></span>

```
var client = new MobileServiceClient("MOBILE_APP_URL");
```

<span data-ttu-id="cd2a3-140">Cserélje le a hello megelőző kódot, `MOBILE_APP_URL` hello URL-címével hello Mobile Apps-háttéralkalmazás, amely megtalálható a Mobile Apps-háttéralkalmazás hello a panel [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="cd2a3-140">In hello preceding code, replace `MOBILE_APP_URL` with hello URL of hello Mobile App backend, which is found in the blade for your Mobile App backend in hello [Azure portal].</span></span> <span data-ttu-id="cd2a3-141">hello MobileServiceClient objektum egypéldányos kell lennie.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-141">hello MobileServiceClient object should be a singleton.</span></span>

## <a name="work-with-tables"></a><span data-ttu-id="cd2a3-142">A táblázatok használata</span><span class="sxs-lookup"><span data-stu-id="cd2a3-142">Work with Tables</span></span>
<span data-ttu-id="cd2a3-143">a következő szakasz hogyan hello toosearch lehívása és hello tábla hello adatainak módosítása.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-143">hello following section details how toosearch and retrieve records and modify hello data within hello table.</span></span>  <span data-ttu-id="cd2a3-144">hello a következő témakörök ismertetnek:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-144">hello following topics are covered:</span></span>

* [<span data-ttu-id="cd2a3-145">Egy tábla hivatkozás létrehozása</span><span class="sxs-lookup"><span data-stu-id="cd2a3-145">Create a table reference</span></span>](#instantiating)
* [<span data-ttu-id="cd2a3-146">Adatait kérdezi le.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-146">Query data</span></span>](#querying)
* [<span data-ttu-id="cd2a3-147">Visszaadott adatok szűrése</span><span class="sxs-lookup"><span data-stu-id="cd2a3-147">Filter returned data</span></span>](#filtering)
* [<span data-ttu-id="cd2a3-148">A rendezési adatokat adott vissza</span><span class="sxs-lookup"><span data-stu-id="cd2a3-148">Sort returned data</span></span>](#sorting)
* [<span data-ttu-id="cd2a3-149">A lapok visszatérési adatai</span><span class="sxs-lookup"><span data-stu-id="cd2a3-149">Return data in pages</span></span>](#paging)
* [<span data-ttu-id="cd2a3-150">Egyes oszlopok kiválasztásához</span><span class="sxs-lookup"><span data-stu-id="cd2a3-150">Select specific columns</span></span>](#selecting)
* [<span data-ttu-id="cd2a3-151">Kereshet meg egy olyan rekordot-azonosító szerint</span><span class="sxs-lookup"><span data-stu-id="cd2a3-151">Look up a record by Id</span></span>](#lookingup)
* [<span data-ttu-id="cd2a3-152">A típus nélküli lekérdezések kezelése</span><span class="sxs-lookup"><span data-stu-id="cd2a3-152">Dealing with untyped queries</span></span>](#untypedqueries)
* [<span data-ttu-id="cd2a3-153">Adatok beszúrása</span><span class="sxs-lookup"><span data-stu-id="cd2a3-153">Inserting data</span></span>](#inserting)
* [<span data-ttu-id="cd2a3-154">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="cd2a3-154">Updating data</span></span>](#updating)
* [<span data-ttu-id="cd2a3-155">Adatok törlése</span><span class="sxs-lookup"><span data-stu-id="cd2a3-155">Deleting data</span></span>](#deleting)
* [<span data-ttu-id="cd2a3-156">Ütközések feloldása, és az egyidejű hozzáférések optimista</span><span class="sxs-lookup"><span data-stu-id="cd2a3-156">Conflict Resolution and Optimistic Concurrency</span></span>](#optimisticconcurrency)
* [<span data-ttu-id="cd2a3-157">Kötési tooa Windows felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="cd2a3-157">Binding tooa Windows User Interface</span></span>](#binding)
* [<span data-ttu-id="cd2a3-158">Hello Oldalméret módosítása</span><span class="sxs-lookup"><span data-stu-id="cd2a3-158">Changing hello Page Size</span></span>](#pagesize)

### <span data-ttu-id="cd2a3-159"><a name="instantiating"></a>Hogyan: tábla hivatkozás létrehozása</span><span class="sxs-lookup"><span data-stu-id="cd2a3-159"><a name="instantiating"></a>How to: Create a table reference</span></span>
<span data-ttu-id="cd2a3-160">Az összes hello kód, amely fér hozzá vagy módosít egy háttér táblában funkciók meghívja a hello `MobileServiceTable` objektum.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-160">All hello code that accesses or modifies data in a backend table calls functions on hello `MobileServiceTable` object.</span></span> <span data-ttu-id="cd2a3-161">Szerezze be a referenciatábla toohello hívó hello [GetTable] módszert az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-161">Obtain a reference toohello table by calling hello [GetTable] method, as follows:</span></span>

```
IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
```

<span data-ttu-id="cd2a3-162">hello visszaadott objektum hello típusos szerializálási modellt használ.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-162">hello returned object uses hello typed serialization model.</span></span> <span data-ttu-id="cd2a3-163">Az a típus nélküli szerializálás modell is támogatott.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-163">An untyped serialization model is also supported.</span></span> <span data-ttu-id="cd2a3-164">Az alábbi példa [táblázatot hoz létre hivatkozást tooan típusos]:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-164">The following example [creates a reference tooan untyped table]:</span></span>

```
// Get an untyped table reference
IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");
```

<span data-ttu-id="cd2a3-165">A típus nélküli lekérdezések meg kell adnia az alapul szolgáló OData-lekérdezési karakterlánc hello.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-165">In untyped queries, you must specify hello underlying OData query string.</span></span>

### <span data-ttu-id="cd2a3-166"><a name="querying"></a>Útmutató: a mobilalkalmazás adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="cd2a3-166"><a name="querying"></a>How to: Query data from your Mobile App</span></span>
<span data-ttu-id="cd2a3-167">Ez a szakasz ismerteti, hogyan tooissue lekérdezi toohello Mobile Apps-háttéralkalmazás, amely tartalmazza a következő funkciók hello:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-167">This section describes how tooissue queries toohello Mobile App backend, which includes hello following functionality:</span></span>

* [<span data-ttu-id="cd2a3-168">Visszaadott adatok szűrése</span><span class="sxs-lookup"><span data-stu-id="cd2a3-168">Filter returned data</span></span>](#filtering)
* [<span data-ttu-id="cd2a3-169">A rendezési adatokat adott vissza</span><span class="sxs-lookup"><span data-stu-id="cd2a3-169">Sort returned data</span></span>](#sorting)
* [<span data-ttu-id="cd2a3-170">A lapok visszatérési adatai</span><span class="sxs-lookup"><span data-stu-id="cd2a3-170">Return data in pages</span></span>](#paging)
* [<span data-ttu-id="cd2a3-171">Egyes oszlopok kiválasztásához</span><span class="sxs-lookup"><span data-stu-id="cd2a3-171">Select specific columns</span></span>](#selecting)
* [<span data-ttu-id="cd2a3-172">Adatok-azonosító szerint</span><span class="sxs-lookup"><span data-stu-id="cd2a3-172">Look up data by ID</span></span>](#lookingup)

> [!NOTE]
> <span data-ttu-id="cd2a3-173">A kiszolgáló adatvezérelt lap mérete összes olyan sort ad vissza az érvényben lévő tooprevent.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-173">A server-driven page size is enforced tooprevent all rows from being returned.</span></span>  <span data-ttu-id="cd2a3-174">Lapozófájl alapértelmezett kérelmek nagy adatkészletek tarthatja negatív befolyásolása hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-174">Paging keeps default requests for large data sets from negatively impacting hello service.</span></span>  <span data-ttu-id="cd2a3-175">tooreturn több mint 50 sorok, használja a hello `Skip` és `Take` metódus, a [vissza adatokat a lapok](#paging).</span><span class="sxs-lookup"><span data-stu-id="cd2a3-175">tooreturn more than 50 rows, use hello `Skip` and `Take` method, as described in [Return data in pages](#paging).</span></span>

### <span data-ttu-id="cd2a3-176"><a name="filtering"></a>Hogyan: szűrő adatokat adott vissza.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-176"><a name="filtering"></a>How to: Filter returned data</span></span>
<span data-ttu-id="cd2a3-177">hello alábbi kód bemutatja, hogyan toofilter adatokat, beleértve a következőket a `Where` alzáradékában.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-177">hello following code illustrates how toofilter data by including a `Where` clause in a query.</span></span> <span data-ttu-id="cd2a3-178">Az összes elemet adja vissza `todoTable` amelynek `Complete` tulajdonság értéke túl`false`.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-178">It returns all items from `todoTable` whose `Complete` property is equal too`false`.</span></span> <span data-ttu-id="cd2a3-179">Hello [ahol] függvény hello lekérdezéshez hello táblázaton predikátum szűrés sor vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-179">hello [Where] function applies a row filtering predicate to hello query against hello table.</span></span>

```
// This query filters out completed TodoItems and items without a timestamp.
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .ToListAsync();
```

<span data-ttu-id="cd2a3-180">Üzenet hálózatfelügyeleti szoftverek, például a böngésző fejlesztői eszközök segítségével hello hello küldött kérelem toohello háttér URI Azonosítóját is megtekintheti vagy [Fiddler].</span><span class="sxs-lookup"><span data-stu-id="cd2a3-180">You can view hello URI of hello request sent toohello backend by using message inspection software, such as browser developer tools or [Fiddler].</span></span> <span data-ttu-id="cd2a3-181">Ha megnézzük hello kérelem URI-azonosítója, figyelje meg, hogy van-e módosítva hello lekérdezési karakterlánc:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-181">If you look at hello request URI, notice that hello query string is modified:</span></span>

```
GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1
```

<span data-ttu-id="cd2a3-182">Az OData-kérelem hello Server SDK által lefordítását egy SQL-lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-182">This OData request is translated into an SQL query by hello Server SDK:</span></span>

```
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
```

<span data-ttu-id="cd2a3-183">függvényen toohello hello `Where` metódus egy tetszőleges számú feltételek lehet.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-183">hello function that is passed toohello `Where` method can have an arbitrary number of conditions.</span></span>

```
// This query filters out completed TodoItems where Text isn't null
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false && todoItem.Text != null)
    .ToListAsync();
```

<span data-ttu-id="cd2a3-184">Ez a példa volna fordítható egy SQL-lekérdezést, hello Server SDK által:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-184">This example would be translated into an SQL query by hello Server SDK:</span></span>

```
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0
```

<span data-ttu-id="cd2a3-185">Ez a lekérdezés több záradékot is osztható:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-185">This query can also be split into multiple clauses:</span></span>

```
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .Where(todoItem => todoItem.Text != null)
    .ToListAsync();
```

<span data-ttu-id="cd2a3-186">hello két módszert, és szabadon felcserélhetők.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-186">hello two methods are equivalent and may be used interchangeably.</span></span>  <span data-ttu-id="cd2a3-187">korábbi beállítás hello&mdash;hozzáfűzésével több predikátumok egy lekérdezést a&mdash;tömörebb és ajánlott.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-187">hello former option&mdash;of concatenating multiple predicates in one query&mdash;is more compact and recommended.</span></span>

<span data-ttu-id="cd2a3-188">Hello `Where` záradék támogatja a műveleteket, lehet lefordítani a hello OData részhalmaza.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-188">hello `Where` clause supports operations that be translated into hello OData subset.</span></span> <span data-ttu-id="cd2a3-189">Műveletek a következők:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-189">Operations include:</span></span>

* <span data-ttu-id="cd2a3-190">Relációs operátorok (==,! =, <, < =, >, > =),</span><span class="sxs-lookup"><span data-stu-id="cd2a3-190">Relational operators (==, !=, <, <=, >, >=),</span></span>
* <span data-ttu-id="cd2a3-191">Aritmetikai operátor (+, -, /, *, %),</span><span class="sxs-lookup"><span data-stu-id="cd2a3-191">Arithmetic operators (+, -, /, *, %),</span></span>
* <span data-ttu-id="cd2a3-192">A pontosság (Math.Floor, Math.Ceiling), szám</span><span class="sxs-lookup"><span data-stu-id="cd2a3-192">Number precision (Math.Floor, Math.Ceiling),</span></span>
* <span data-ttu-id="cd2a3-193">Karakterlánc (hossza, Substring, Replace, IndexOf, megadott módon kezdődő, megadott módon végződő),</span><span class="sxs-lookup"><span data-stu-id="cd2a3-193">String functions (Length, Substring, Replace, IndexOf, StartsWith, EndsWith),</span></span>
* <span data-ttu-id="cd2a3-194">Tulajdonságok (év, hónap, nap, óra, perc másodperc), dátum</span><span class="sxs-lookup"><span data-stu-id="cd2a3-194">Date properties (Year, Month, Day, Hour, Minute, Second),</span></span>
* <span data-ttu-id="cd2a3-195">Az objektum tulajdonságainak hozzáférést és</span><span class="sxs-lookup"><span data-stu-id="cd2a3-195">Access properties of an object, and</span></span>
* <span data-ttu-id="cd2a3-196">A kifejezések kombinálásával egyik műveletet.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-196">Expressions combining any of these operations.</span></span>

<span data-ttu-id="cd2a3-197">Ha figyelembe véve, hogy mi hello Server SDK támogatja, érdemes lehet a hello [OData v3 dokumentáció].</span><span class="sxs-lookup"><span data-stu-id="cd2a3-197">When considering what hello Server SDK supports, you can consider hello [OData v3 Documentation].</span></span>

### <span data-ttu-id="cd2a3-198"><a name="sorting"></a>Hogyan: rendezési adatokat adott vissza.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-198"><a name="sorting"></a>How to: Sort returned data</span></span>
<span data-ttu-id="cd2a3-199">hello alábbi kód bemutatja, hogyan toosort adatokat, beleértve a következőket egy [OrderBy] vagy [OrderByDescending] hello lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-199">hello following code illustrates how toosort data by including an [OrderBy] or [OrderByDescending] function in hello query.</span></span> <span data-ttu-id="cd2a3-200">Adja vissza, a cikkek `todoTable` növekvő sorrend szerint hello `Text` mező.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-200">It returns items from `todoTable` sorted ascending by hello `Text` field.</span></span>

```
// Sort items in ascending order by Text field
MobileServiceTableQuery<TodoItem> query = todoTable
                .OrderBy(todoItem => todoItem.Text)
List<TodoItem> items = await query.ToListAsync();

// Sort items in descending order by Text field
MobileServiceTableQuery<TodoItem> query = todoTable
                .OrderByDescending(todoItem => todoItem.Text)
List<TodoItem> items = await query.ToListAsync();
```

### <span data-ttu-id="cd2a3-201"><a name="paging"></a>Hogyan: vissza adatokat lap</span><span class="sxs-lookup"><span data-stu-id="cd2a3-201"><a name="paging"></a>How to: Return data in pages</span></span>
<span data-ttu-id="cd2a3-202">Alapértelmezés szerint hello háttér sorát adja vissza csak hello első 50.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-202">By default, hello backend returns only hello first 50 rows.</span></span> <span data-ttu-id="cd2a3-203">Így javítható hívó hello hello visszaadott sorok száma [érvénybe] metódust.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-203">You can increase hello number of returned rows by calling hello [Take] method.</span></span> <span data-ttu-id="cd2a3-204">Használjon `Take` együtt hello [kihagyása] metódus toorequest egy adott "lap" hello teljes adatkészlet hello lekérdezés által visszaadott.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-204">Use `Take` along with hello [Skip] method toorequest a specific "page" of hello total dataset returned by hello query.</span></span> <span data-ttu-id="cd2a3-205">hello következő lekérdezés végrehajtásakor adja vissza hello a három legfontosabb elemek hello táblában.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-205">hello following query, when executed, returns hello top three items in hello table.</span></span>

```
// Define a filtered query that returns hello top 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Take(3);
List<TodoItem> items = await query.ToListAsync();
```

<span data-ttu-id="cd2a3-206">hello alábbi javított lekérdezés átugrása hello első három eredményeit, és értéket ad vissza a következő három eredmények hello.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-206">hello following revised query skips hello first three results and returns hello next three results.</span></span> <span data-ttu-id="cd2a3-207">Ez a lekérdezés hello második "lap" az adatok, ahol hello lap mérete három elemek eredményez.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-207">This query produces hello second "page" of data, where hello page size is three items.</span></span>

```
// Define a filtered query that skips hello top 3 items and returns hello next 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Skip(3).Take(3);
List<TodoItem> items = await query.ToListAsync();
```

<span data-ttu-id="cd2a3-208">Hello [IncludeTotalCount] metódus kérelmek teljes száma hello *összes* azt jelzi, hogy akkor adott vissza, figyelmen kívül hagyja a lapozófájl/vonatkozó záradékban megadott hello:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-208">hello [IncludeTotalCount] method requests hello total count for *all* hello records that would have been returned, ignoring any paging/limit clause specified:</span></span>

```
query = query.IncludeTotalCount();
```

<span data-ttu-id="cd2a3-209">Egy valós alkalmazás használhat lekérdezések hasonló toohello személyhívó vezérlő vagy hasonló felhasználói felület – példa megelőző oldalai között.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-209">In a real world app, you can use queries similar toohello preceding example with a pager control or comparable UI to navigate between pages.</span></span>

> [!NOTE]
> <span data-ttu-id="cd2a3-210">toooverride hello soron kívüli 50-korlát a mobil-háttéralkalmazást, telepítenie kell az is hello [EnableQueryAttribute] toohello nyilvános GET metódus, és adja meg a hello lapozás viselkedését.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-210">toooverride hello 50-row limit in a Mobile App backend, you must also apply hello [EnableQueryAttribute] toohello public GET method and specify hello paging behavior.</span></span> <span data-ttu-id="cd2a3-211">Ha az alkalmazott toohello metódus, a következő hello beállítja a visszaadott sorok maximális száma too1000:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-211">When applied toohello method, hello following sets the maximum returned rows too1000:</span></span>
>
> `[EnableQuery(MaxTop=1000)]`


### <span data-ttu-id="cd2a3-212"><a name="selecting"></a>Hogyan: egyes oszlopok kiválasztásához</span><span class="sxs-lookup"><span data-stu-id="cd2a3-212"><a name="selecting"></a>How to: Select specific columns</span></span>
<span data-ttu-id="cd2a3-213">Megadhatja a hello tulajdonságok tooinclude körét eredmények hozzáadásával egy [válasszon] záradék tooyour lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-213">You can specify which set of properties tooinclude in hello results by adding a [Select] clause tooyour query.</span></span> <span data-ttu-id="cd2a3-214">Például az a következő kód bemutatja hogyan hello tooselect csak egy mezőt, és hogyan is tooselect és több mező formázása:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-214">For example, hello following code shows how tooselect just one field and also how tooselect and format multiple fields:</span></span>

```
// Select one field -- just hello Text
MobileServiceTableQuery<TodoItem> query = todoTable
                .Select(todoItem => todoItem.Text);
List<string> items = await query.ToListAsync();

// Select multiple fields -- both Complete and Text info
MobileServiceTableQuery<TodoItem> query = todoTable
                .Select(todoItem => string.Format("{0} -- {1}",
                    todoItem.Text.PadRight(30), todoItem.Complete ?
                    "Now complete!" : "Incomplete!"));
List<string> items = await query.ToListAsync();
```

<span data-ttu-id="cd2a3-215">Az összes hello funkciók leírása, amennyiben azok additívak, így azt is tartsa láncolás őket.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-215">All hello functions described so far are additive, so we can keep chaining them.</span></span> <span data-ttu-id="cd2a3-216">Minden láncolt hívás több hello lekérdezés hatással van.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-216">Each chained call affects more of hello query.</span></span> <span data-ttu-id="cd2a3-217">Egy további példa:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-217">One more example:</span></span>

```
MobileServiceTableQuery<TodoItem> query = todoTable
                .Where(todoItem => todoItem.Complete == false)
                .Select(todoItem => todoItem.Text)
                .Skip(3).
                .Take(3);
List<string> items = await query.ToListAsync();
```

### <span data-ttu-id="cd2a3-218"><a name="lookingup"></a>Útmutató: adatok-azonosító szerint</span><span class="sxs-lookup"><span data-stu-id="cd2a3-218"><a name="lookingup"></a>How to: Look up data by ID</span></span>
<span data-ttu-id="cd2a3-219">Hello [LookupAsync] függvény lehet használt toolook objektumainak hello adatbázis egy adott azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-219">hello [LookupAsync] function can be used toolook up objects from hello database with a particular ID.</span></span>

```
// This query filters out hello item with hello ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");
```

### <span data-ttu-id="cd2a3-220"><a name="untypedqueries"></a>Útmutató: a típus nélküli lekérdezések végrehajtása</span><span class="sxs-lookup"><span data-stu-id="cd2a3-220"><a name="untypedqueries"></a>How to: Execute untyped queries</span></span>
<span data-ttu-id="cd2a3-221">A típus nélküli tábla objektum lekérdezést végrehajtásakor explicit módon meg kell hello OData-lekérdezési karakterlánc meghívásával [ReadAsync], a példában a következő hello:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-221">When executing a query using an untyped table object, you must explicitly specify hello OData query string by calling [ReadAsync], as in hello following example:</span></span>

```
// Lookup untyped data using OData
JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");
```

<span data-ttu-id="cd2a3-222">JSON értékekre, amelyek például a tulajdonságcsomag vissza.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-222">You get back JSON values that you can use like a property bag.</span></span> <span data-ttu-id="cd2a3-223">JToken és Newtonsoft Json.NET további információkért lásd: hello [Json.NET] hely.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-223">For more information on JToken and Newtonsoft Json.NET, see hello [Json.NET] site.</span></span>

### <span data-ttu-id="cd2a3-224"><a name="inserting"></a>Útmutató: a Mobile Apps-háttéralkalmazás adatok beszúrása</span><span class="sxs-lookup"><span data-stu-id="cd2a3-224"><a name="inserting"></a>How to: Insert data into a Mobile App backend</span></span>
<span data-ttu-id="cd2a3-225">Minden ügyfél esetében tartalmaznia kell egy nevű tag **azonosító**, amely alapértelmezés szerint ki egy karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-225">All client types must contain a member named **Id**, which is by default a string.</span></span> <span data-ttu-id="cd2a3-226">Ez **azonosító** CRUD műveletek elvégzéséhez szükséges, és a kapcsolat nélküli szinkronizálás. hello a következő kód bemutatja, hogyan toouse hello [InsertAsync] metódus tooinsert új sorok egy táblába.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-226">This **Id** is required to perform CRUD operations and for offline sync. hello following code illustrates how toouse hello [InsertAsync] method tooinsert new rows into a table.</span></span> <span data-ttu-id="cd2a3-227">hello paraméter hello adatok toobe .NET objektumként beszúrni tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-227">hello parameter contains hello data toobe inserted as a .NET object.</span></span>

```
await todoTable.InsertAsync(todoItem);
```

<span data-ttu-id="cd2a3-228">Ha az egyéni azonosító egyedi érték nem szerepel a hello `todoItem` során egy INSERT utasítás hello kiszolgáló által létrehozott GUID.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-228">If a unique custom ID value is not included in hello `todoItem` during an insert, a GUID is generated by hello server.</span></span>
<span data-ttu-id="cd2a3-229">Kérheti le hello létrehozott azonosító hello objektum vizsgálatával vissza hello hívása után.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-229">You can retrieve hello generated Id by inspecting hello object after hello call returns.</span></span>

<span data-ttu-id="cd2a3-230">tooinsert adatok típus nélküli, Json.NET előnyeit is:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-230">tooinsert untyped data, you may take advantage of Json.NET:</span></span>

```
JObject jo = new JObject();
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

<span data-ttu-id="cd2a3-231">Íme egy példa egy e-mail cím használata egy egyedi karakterlánc-azonosító:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-231">Here is an example using an email address as a unique string id:</span></span>

```
JObject jo = new JObject();
jo.Add("id", "myemail@emaildomain.com");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

### <a name="working-with-id-values"></a><span data-ttu-id="cd2a3-232">Azonosító értékek használata</span><span class="sxs-lookup"><span data-stu-id="cd2a3-232">Working with ID values</span></span>
<span data-ttu-id="cd2a3-233">Mobile Apps egyedi egyéni karakterlánc-értékek támogatja hello tábla **azonosító** oszlop.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-233">Mobile Apps supports unique custom string values for hello table's **id** column.</span></span> <span data-ttu-id="cd2a3-234">Egy karakterláncértéket lehetővé teszi alkalmazások toouse egyéni értékeket, például az e-mail címek vagy felhasználónevek hello azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-234">A string value allows applications toouse custom values such as email addresses or user names for hello ID.</span></span>  <span data-ttu-id="cd2a3-235">Karakterlánc-azonosítóknak hello a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-235">String IDs provide you with hello following benefits:</span></span>

* <span data-ttu-id="cd2a3-236">Azonosítók anélkül, hogy az oda-vissza toohello adatbázis jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-236">IDs are generated without making a round trip toohello database.</span></span>
* <span data-ttu-id="cd2a3-237">Rekordok könnyebb toomerge különböző táblákhoz vagy adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-237">Records are easier toomerge from different tables or databases.</span></span>
* <span data-ttu-id="cd2a3-238">Azonosítók értékek jobban integrálható egy alkalmazás logikáját.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-238">IDs values can integrate better with an application's logic.</span></span>

<span data-ttu-id="cd2a3-239">Egy karakterláncértéket azonosítója nincs beállítva egy beszúrt rekordot, hello Mobile Apps-háttéralkalmazás hoz létre egy egyedi értéket a azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-239">When a string ID value is not set on an inserted record, hello Mobile App backend generates a unique value for the ID.</span></span> <span data-ttu-id="cd2a3-240">Használhatja a hello [Guid.NewGuid] saját azonosító értéket, hello ügyfélen vagy a hello háttérbeli metódus toogenerate.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-240">You can use hello [Guid.NewGuid] method toogenerate your own ID values, either on hello client or in hello backend.</span></span>

```
JObject jo = new JObject();
jo.Add("id", Guid.NewGuid().ToString("N"));
```

### <span data-ttu-id="cd2a3-241"><a name="modifying"></a>Útmutató: a Mobile Apps-háttéralkalmazás adatainak módosítása</span><span class="sxs-lookup"><span data-stu-id="cd2a3-241"><a name="modifying"></a>How to: Modify data in a Mobile App backend</span></span>
<span data-ttu-id="cd2a3-242">hello alábbi kód bemutatja, hogyan toouse hello [UpdateAsync] metódus tooupdate hello egy meglévő rekord azonos azonosító új információkkal.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-242">hello following code illustrates how toouse hello [UpdateAsync] method tooupdate an existing record with hello same ID with new information.</span></span> <span data-ttu-id="cd2a3-243">hello paraméter hello adatok toobe frissíteni, mivel a .NET-objektum tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-243">hello parameter contains hello data toobe updated as a .NET object.</span></span>

```
await todoTable.UpdateAsync(todoItem);
```

<span data-ttu-id="cd2a3-244">tooupdate típus nélküli adatokat, akkor lehetséges, hogy előnyeit [Json.NET] az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-244">tooupdate untyped data, you may take advantage of [Json.NET] as follows:</span></span>

```
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.UpdateAsync(jo);
```

<span data-ttu-id="cd2a3-245">Egy `id` mezőt meg kell adni, ha egy frissítés.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-245">An `id` field must be specified when making an update.</span></span> <span data-ttu-id="cd2a3-246">hello háttér használ hello `id` mező tooidentify, amely tooupdate sor.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-246">hello backend uses hello `id` field tooidentify which row tooupdate.</span></span> <span data-ttu-id="cd2a3-247">Hello `id` mező hello hello eredményét szerezhető `InsertAsync` hívható meg.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-247">hello `id` field can be obtained from hello result of hello `InsertAsync` call.</span></span> <span data-ttu-id="cd2a3-248">Egy `ArgumentException` jelenik meg, ha egy elem tooupdate hello megadása nélkül próbálja `id` érték.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-248">An `ArgumentException` is raised if you try tooupdate an item without providing hello `id` value.</span></span>

### <span data-ttu-id="cd2a3-249"><a name="deleting"></a>Útmutató: a Mobile Apps-háttéralkalmazás adatok törlése</span><span class="sxs-lookup"><span data-stu-id="cd2a3-249"><a name="deleting"></a>How to: Delete data in a Mobile App backend</span></span>
<span data-ttu-id="cd2a3-250">hello alábbi kód bemutatja, hogyan toouse hello [DeleteAsync] metódus toodelete egy meglévő példányát.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-250">hello following code illustrates how toouse hello [DeleteAsync] method toodelete an existing instance.</span></span> <span data-ttu-id="cd2a3-251">hello példány hello azonosíthatók `id` hello be mezőben `todoItem`.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-251">hello instance is identified by hello `id` field set on hello `todoItem`.</span></span>

```
await todoTable.DeleteAsync(todoItem);
```

<span data-ttu-id="cd2a3-252">toodelete típus nélküli adatokat, akkor előfordulhat, hogy előnyeit Json.NET az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-252">toodelete untyped data, you may take advantage of Json.NET as follows:</span></span>

```
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
await table.DeleteAsync(jo);
```

<span data-ttu-id="cd2a3-253">Ha elvégezte a törlési kérelmet, meg kell adni egy Azonosítót.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-253">When you make a delete request, an ID must be specified.</span></span> <span data-ttu-id="cd2a3-254">Egyéb tulajdonságok nem átadott toohello szolgáltatás, vagy a hello szolgáltatás figyelmen kívül lesznek hagyva.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-254">Other properties are not passed toohello service or are ignored at hello service.</span></span> <span data-ttu-id="cd2a3-255">hello eredménye egy `DeleteAsync` tekintendő, amely általában `null`.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-255">hello result of a `DeleteAsync` call is usually `null`.</span></span> <span data-ttu-id="cd2a3-256">hello azonosító toopass a hello hello eredményét szerezhető `InsertAsync` hívható meg.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-256">hello ID toopass in can be obtained from hello result of hello `InsertAsync` call.</span></span> <span data-ttu-id="cd2a3-257">A `MobileServiceInvalidOperationException` toodelete elem meg hello megadása nélkül történt `id` mező.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-257">A `MobileServiceInvalidOperationException` is thrown when you try toodelete an item without specifying hello `id` field.</span></span>

### <span data-ttu-id="cd2a3-258"><a name="optimisticconcurrency"></a>Hogyan: használata egyidejű hozzáférések optimista az ütközések feloldása</span><span class="sxs-lookup"><span data-stu-id="cd2a3-258"><a name="optimisticconcurrency"></a>How to: Use Optimistic Concurrency for conflict resolution</span></span>
<span data-ttu-id="cd2a3-259">Két vagy több ügyfél írhat módosítások toohello azonos elem: hello azonos idő.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-259">Two or more clients may write changes toohello same item at hello same time.</span></span> <span data-ttu-id="cd2a3-260">Ütközésészlelés, nélkül hello utolsó írási felülírná korábbi frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-260">Without conflict detection, hello last write would overwrite any previous updates.</span></span> <span data-ttu-id="cd2a3-261">**Egyidejű hozzáférések optimista vezérlését** feltételezi, hogy az egyes tranzakciókra véglegesítheti, és ezért nem használhatja semmilyen erőforrás zárolását.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-261">**Optimistic concurrency control** assumes that each transaction can commit and therefore does not use any resource locking.</span></span>  <span data-ttu-id="cd2a3-262">Előtt véglegesítése tranzakció, egyidejű hozzáférések optimista vezérlését ellenőrzi, hogy nincs másik tranzakció módosította hello adatok.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-262">Before committing a transaction, optimistic concurrency control verifies that no other transaction has modified hello data.</span></span> <span data-ttu-id="cd2a3-263">Ha hello adatok módosítva lett, tranzakció véglegesítésekor hello vissza lesz állítva.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-263">If hello data has been modified, hello committing transaction is rolled back.</span></span>

<span data-ttu-id="cd2a3-264">Mobile Apps egyidejű hozzáférések optimista vezérlését támogatja, módosításokat tooeach elem hello segítségével nyomon követésével `version` rendszer tulajdonság oszlop, amely minden táblában a Mobile Apps-háttéralkalmazás van definiálva.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-264">Mobile Apps supports optimistic concurrency control by tracking changes tooeach item using hello `version` system property column that is defined for each table in your Mobile App backend.</span></span> <span data-ttu-id="cd2a3-265">Minden alkalommal, amikor frissíti, a Mobile Apps beállítja a hello `version` tulajdonság az adott jegyezze tooa új értéket.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-265">Each time a record is updated, Mobile Apps sets hello `version` property for that record tooa new value.</span></span> <span data-ttu-id="cd2a3-266">Minden egyes frissítés kérelem során hello `version` hello rekord hello kérés részét képező tulajdonság értéke összehasonlított toohello hello rekord hello kiszolgáló ugyanahhoz a tulajdonsághoz.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-266">During each update request, hello `version` property of hello record included with hello request is compared toohello same property for hello record on hello server.</span></span> <span data-ttu-id="cd2a3-267">Ha átadni a verzió hello kérelem nem egyezik meg a hello háttér, majd riasztást hello ügyféloldali kódtára a `MobileServicePreconditionFailedException<T>` kivétel.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-267">If the version passed with hello request does not match hello backend, then hello client library raises a `MobileServicePreconditionFailedException<T>` exception.</span></span> <span data-ttu-id="cd2a3-268">hello hello kivétel mellékelt típus hello háttérrendszer tartalmazó hello kiszolgálók verzió hello rekord hello-bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-268">hello type included with hello exception is hello record from hello backend containing hello servers version of hello record.</span></span> <span data-ttu-id="cd2a3-269">hello alkalmazás használhatja az információk toodecide e tooexecute hello frissítés kérést megfelelő hello `version` hello háttér toocommit módosítások közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-269">hello application can then use this information toodecide whether tooexecute hello update request again with hello correct `version` value from hello backend toocommit changes.</span></span>

<span data-ttu-id="cd2a3-270">Adja meg a hello tábla osztály hello az oszlop `version` rendszer tulajdonság tooenable optimista konkurencia.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-270">Define a column on hello table class for hello `version` system property tooenable optimistic concurrency.</span></span> <span data-ttu-id="cd2a3-271">Példa:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-271">For example:</span></span>

```
public class TodoItem
{
    public string Id { get; set; }

    [JsonProperty(PropertyName = "text")]
    public string Text { get; set; }

    [JsonProperty(PropertyName = "complete")]
    public bool Complete { get; set; }

    // *** Enable Optimistic Concurrency *** //
    [JsonProperty(PropertyName = "version")]
    public string Version { set; get; }
}
```

<span data-ttu-id="cd2a3-272">A típus nélküli táblák használata kiszolgálóalkalmazások lehetővé teszik az hello beállítása egyidejű hozzáférések optimista `Version` a jelzőt a `SystemProperties` hello a tábla az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-272">Applications using untyped tables enable optimistic concurrency by setting hello `Version` flag on the `SystemProperties` of hello table as follows.</span></span>

```
//Enable optimistic concurrency by retrieving version
todoTable.SystemProperties |= MobileServiceSystemProperties.Version;
```

<span data-ttu-id="cd2a3-273">A hozzáadása tooenabling egyidejű hozzáférések optimista, meg kell is catch hello `MobileServicePreconditionFailedException<T>` a kódban meghívásakor kivétel [UpdateAsync].</span><span class="sxs-lookup"><span data-stu-id="cd2a3-273">In addition tooenabling optimistic concurrency, you must also catch hello `MobileServicePreconditionFailedException<T>` exception in your code when calling [UpdateAsync].</span></span>  <span data-ttu-id="cd2a3-274">Oldja fel a hello ütközést úgy, hogy alkalmazza a megfelelő hello `version` toothe frissített rekord és a hívás [UpdateAsync] a hello feloldotta a rekordot.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-274">Resolve hello conflict by applying hello correct `version` toothe updated record and call [UpdateAsync] with hello resolved record.</span></span> <span data-ttu-id="cd2a3-275">a következő kód hello látható tooresolve egy írás ütközés egyszer észlelésének módját:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-275">hello following code shows how tooresolve a write conflict once detected:</span></span>

```
private async void UpdateToDoItem(TodoItem item)
{
    MobileServicePreconditionFailedException<TodoItem> exception = null;

    try
    {
        //update at hello remote table
        await todoTable.UpdateAsync(item);
    }
    catch (MobileServicePreconditionFailedException<TodoItem> writeException)
    {
        exception = writeException;
    }

    if (exception != null)
    {
        // Conflict detected, hello item has changed since hello last query
        // Resolve hello conflict between hello local and server item
        await ResolveConflict(item, exception.Item);
    }
}


private async Task ResolveConflict(TodoItem localItem, TodoItem serverItem)
{
    //Ask user toochoose hello resoltion between versions
    MessageDialog msgDialog = new MessageDialog(
        String.Format("Server Text: \"{0}\" \nLocal Text: \"{1}\"\n",
        serverItem.Text, localItem.Text),
        "CONFLICT DETECTED - Select a resolution:");

    UICommand localBtn = new UICommand("Commit Local Text");
    UICommand ServerBtn = new UICommand("Leave Server Text");
    msgDialog.Commands.Add(localBtn);
    msgDialog.Commands.Add(ServerBtn);

    localBtn.Invoked = async (IUICommand command) =>
    {
        // tooresolve hello conflict, update hello version of hello item being committed. Otherwise, you will keep
        // catching a MobileServicePreConditionFailedException.
        localItem.Version = serverItem.Version;

        // Updating recursively here just in case another change happened while hello user was making a decision
        UpdateToDoItem(localItem);
    };

    ServerBtn.Invoked = async (IUICommand command) =>
    {
        RefreshTodoItems();
    };

    await msgDialog.ShowAsync();
}
```

<span data-ttu-id="cd2a3-276">További információkért lásd: hello [az Azure Mobile Apps Offline adatszinkronizálás] témakör.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-276">For more information, see hello [Offline Data Sync in Azure Mobile Apps] topic.</span></span>

### <span data-ttu-id="cd2a3-277"><a name="binding"></a>Útmutató: a kötés Mobile Apps adatok tooa Windows felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="cd2a3-277"><a name="binding"></a>How to: Bind Mobile Apps data tooa Windows user interface</span></span>
<span data-ttu-id="cd2a3-278">Ez a szakasz bemutatja, hogyan toodisplay visszaadott adatok objektumok felhasználói felületi elemei használják a Windows-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-278">This section shows how toodisplay returned data objects using UI elements in a Windows app.</span></span>  <span data-ttu-id="cd2a3-279">Az alábbi példakód toohello lista forrása a hello hiányos elemek lekérdezéssel van kötve.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-279">The following example code binds toohello source of hello list with a query for incomplete items.</span></span> <span data-ttu-id="cd2a3-280">A [MobileServiceCollection] létrehoz egy Mobile Apps-kompatibilis kötés gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-280">The [MobileServiceCollection] creates a Mobile Apps-aware binding collection.</span></span>

```
// This query filters out completed TodoItems.
MobileServiceCollection<TodoItem, TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .ToCollectionAsync();

// itemsControl is an IEnumerable that could be bound tooa UI list control
IEnumerable itemsControl  = items;

// Bind this tooa ListBox
ListBox lb = new ListBox();
lb.ItemsSource = items;
```

<span data-ttu-id="cd2a3-281">Egyes vezérlők hello a felügyelt futásidejű támogatását illesztőfelület nevű [ISupportIncrementalLoading].</span><span class="sxs-lookup"><span data-stu-id="cd2a3-281">Some controls in hello managed runtime support an interface called [ISupportIncrementalLoading].</span></span> <span data-ttu-id="cd2a3-282">Ez az interfész lehetővé teszi, hogy a vezérlők toorequest hello felhasználói görgetésekor további adatokat.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-282">This interface allows controls toorequest extra data when hello user scrolls.</span></span> <span data-ttu-id="cd2a3-283">Ez az interfész keresztül univerzális Windows-alkalmazások beépített támogatása [MobileServiceIncrementalLoadingCollection], amely automatikusan kezeli a hívások a hello vezérlők.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-283">There is built-in support for this interface for universal Windows apps via [MobileServiceIncrementalLoadingCollection], which automatically handles the calls from hello controls.</span></span> <span data-ttu-id="cd2a3-284">Használjon `MobileServiceIncrementalLoadingCollection` a Windows-alkalmazások az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-284">Use `MobileServiceIncrementalLoadingCollection` in Windows apps as follows:</span></span>

```
MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
items = todoTable.Where(todoItem => todoItem.Complete == false).ToIncrementalLoadingCollection();

ListBox lb = new ListBox();
lb.ItemsSource = items;
```

<span data-ttu-id="cd2a3-285">toouse hello új gyűjtemény a Windows Phone 8 és a "Silverlight" alkalmazásokra, használjon hello `ToCollection` a kiterjesztésmetódusok `IMobileServiceTableQuery<T>` és `IMobileServiceTable<T>`.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-285">toouse hello new collection on Windows Phone 8 and "Silverlight" apps, use hello `ToCollection` extension methods on `IMobileServiceTableQuery<T>` and `IMobileServiceTable<T>`.</span></span> <span data-ttu-id="cd2a3-286">tooload adatok hívás `LoadMoreItemsAsync()`.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-286">tooload data, call `LoadMoreItemsAsync()`.</span></span>

```
MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
await items.LoadMoreItemsAsync();
```

<span data-ttu-id="cd2a3-287">Meghívásával létrehozott hello gyűjtemény használatakor `ToCollectionAsync` vagy `ToCollection`, kap egy gyűjteményt, amely lehet kötött tooUI szabályozza.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-287">When you use hello collection created by calling `ToCollectionAsync` or `ToCollection`, you get a collection that can be bound tooUI controls.</span></span>  <span data-ttu-id="cd2a3-288">Ez a gyűjtemény a lapozófájl-kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-288">This collection is paging-aware.</span></span>  <span data-ttu-id="cd2a3-289">Hello gyűjtemény adatokat tölt be a hálózaton, mivel néha betöltése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-289">Since hello collection is loading data from the network, loading sometimes fails.</span></span> <span data-ttu-id="cd2a3-290">toohandle ilyen hibák felülbírálása hello `OnException` metódusa `MobileServiceIncrementalLoadingCollection` hívások túl eredő toohandle kivételek`LoadMoreItemsAsync`.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-290">toohandle such failures, override hello `OnException` method on `MobileServiceIncrementalLoadingCollection` toohandle exceptions resulting from calls too`LoadMoreItemsAsync`.</span></span>

<span data-ttu-id="cd2a3-291">Vegye figyelembe, ha a tábla sok mezőt tartalmaz, de azt szeretné, toodisplay némelyikük a vezérlőben.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-291">Consider if your table has many fields but you only want toodisplay some of them in your control.</span></span> <span data-ttu-id="cd2a3-292">Hello útmutatást használhatja az előző szakaszban hello "[egyes oszlopok kiválasztásához](#selecting)" tooselect adott oszlopok toodisplay a felhasználói felület hello.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-292">You may use hello guidance in hello preceding section "[Select specific columns](#selecting)" tooselect specific columns toodisplay in hello UI.</span></span>

### <span data-ttu-id="cd2a3-293"><a name="pagesize"></a>Oldalméret módosítása hello</span><span class="sxs-lookup"><span data-stu-id="cd2a3-293"><a name="pagesize"></a>Change hello Page size</span></span>
<span data-ttu-id="cd2a3-294">Az Azure Mobile Apps egy legfeljebb 50 elemet kérelmenként alapértelmezés szerint adja vissza.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-294">Azure Mobile Apps returns a maximum of 50 items per request by default.</span></span>  <span data-ttu-id="cd2a3-295">Hello a lapozófájl méretének növelésével hello maximális lapméretét hello ügyfél és kiszolgáló egyaránt a módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-295">You can change hello paging size by increasing hello maximum page size on both hello client and server.</span></span>  <span data-ttu-id="cd2a3-296">tooincrease hello kért oldalméret, adja meg `PullOptions` használatakor `PullAsync()`:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-296">tooincrease hello requested page size, specify `PullOptions` when using `PullAsync()`:</span></span>

```
PullOptions pullOptions = new PullOptions
    {
        MaxPageSize = 100
    };
```

<span data-ttu-id="cd2a3-297">Feltéve, hogy hello végrehajtott `PageSize` egyenlő tooor hello kiszolgálón belül 100-nál nagyobb, a kérelem legfeljebb 100 elemet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-297">Assuming you have made hello `PageSize` equal tooor greater than 100 within hello server, a request returns up to 100 items.</span></span>

## <span data-ttu-id="cd2a3-298"><a name="#offlinesync"></a>A kapcsolat nélküli táblázatok használata</span><span class="sxs-lookup"><span data-stu-id="cd2a3-298"><a name="#offlinesync"></a>Work with Offline Tables</span></span>
<span data-ttu-id="cd2a3-299">Kapcsolat nélküli táblák egy helyi SQLite toostore adat tárolása használ, amikor a kapcsolat nélküli használata.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-299">Offline tables use a local SQLite store toostore data for use when offline.</span></span>  <span data-ttu-id="cd2a3-300">Minden tábla elleni hello végzett műveletek helyi SQLite helyett hello távoli kiszolgáló tárolójában tárolja.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-300">All table operations are done against hello local SQLite store instead of hello remote server store.</span></span>  <span data-ttu-id="cd2a3-301">egy kapcsolat nélküli tábla toocreate előkészítenie a projekthez:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-301">toocreate an offline table, first prepare your project:</span></span>

1. <span data-ttu-id="cd2a3-302">A Visual Studióban, kattintson a jobb gombbal hello megoldás > **NuGet-csomagok kezelése megoldáshoz...** , majd keresse meg, és telepítse a **Microsoft.Azure.Mobile.Client.SQLiteStore** projektek hello megoldás NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-302">In Visual Studio, right-click hello solution > **Manage NuGet Packages for Solution...**, then search for and install the **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet package for all projects in hello solution.</span></span>
2. <span data-ttu-id="cd2a3-303">(Választható) toosupport Windows-eszközök esetében egy SQLite futtatókörnyezetek következő hello telepíthető:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-303">(Optional) toosupport Windows devices, install one of hello following SQLite runtime packages:</span></span>

   * <span data-ttu-id="cd2a3-304">**Windows 8.1 futásidejű:** telepítése [a Windows 8.1 SQLite][3].</span><span class="sxs-lookup"><span data-stu-id="cd2a3-304">**Windows 8.1 Runtime:** Install [SQLite for Windows 8.1][3].</span></span>
   * <span data-ttu-id="cd2a3-305">**Windows Phone 8.1:** telepítése [a Windows Phone 8.1 SQLite][4].</span><span class="sxs-lookup"><span data-stu-id="cd2a3-305">**Windows Phone 8.1:** Install [SQLite for Windows Phone 8.1][4].</span></span>
   * <span data-ttu-id="cd2a3-306">**Univerzális Windows Platform** telepítése [az univerzális Windows hello SQLite][5].</span><span class="sxs-lookup"><span data-stu-id="cd2a3-306">**Universal Windows Platform** Install [SQLite for hello Universal Windows][5].</span></span>
3. <span data-ttu-id="cd2a3-307">(Nem kötelező).</span><span class="sxs-lookup"><span data-stu-id="cd2a3-307">(Optional).</span></span> <span data-ttu-id="cd2a3-308">Windows-eszközökhöz, kattintson a **hivatkozások** > **hivatkozás hozzáadása...** , bontsa ki a hello **Windows** mappa > **bővítmények**, engedélyeznie kell a megfelelő hello **SQLite for Windows** SDK hello együtt **Windows Visual C++ 2013 futásidejű** SDK.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-308">For Windows devices, click **References** > **Add Reference...**, expand hello **Windows** folder > **Extensions**, then enable hello appropriate **SQLite for Windows** SDK along with hello **Visual C++ 2013 Runtime for Windows** SDK.</span></span>
    <span data-ttu-id="cd2a3-309">hello SQLite SDK nevek függően eltérőek az egyes Windows-platformra.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-309">hello SQLite SDK names vary slightly with each Windows platform.</span></span>

<span data-ttu-id="cd2a3-310">Egy tábla hivatkozás létrehozása előtt hello helyi tárolóban kell készíteni:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-310">Before a table reference can be created, hello local store must be prepared:</span></span>

```
var store = new MobileServiceSQLiteStore(Constants.OfflineDbPath);
store.DefineTable<TodoItem>();

//Initializes hello SyncContext using hello default IMobileServiceSyncHandler.
await this.client.SyncContext.InitializeAsync(store);
```

<span data-ttu-id="cd2a3-311">Tanúsítványtár inicializálásához hello ügyfél létrehozása után azonnal normál módon történik.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-311">Store initialization is normally done immediately after hello client is created.</span></span>  <span data-ttu-id="cd2a3-312">Hello **OfflineDbPath** használja az Ön által támogatott összes platformon megfelelő legyen a fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-312">hello **OfflineDbPath** should be a filename suitable for use on all platforms that you support.</span></span>  <span data-ttu-id="cd2a3-313">Ha hello elérési út egy teljes mértékben minősített elérési út (Ez azt jelenti, hogy kezdődik perjellel), majd, hogy az elérési utat használja.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-313">If hello path is a fully qualified path (that is, it starts with a slash), then that path is used.</span></span>  <span data-ttu-id="cd2a3-314">Ha hello elérési út nem teljesen minősített, hello fájl egy platformspecifikus helyre kerül.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-314">If hello path is not fully qualified, hello file is placed in a platform-specific location.</span></span>

* <span data-ttu-id="cd2a3-315">Az iOS és Android-eszközök esetében a hello alapértelmezett elérési út hello "Személyes fájlok" mappa.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-315">For iOS and Android devices, hello default path is hello "Personal Files" folder.</span></span>
* <span data-ttu-id="cd2a3-316">Windows-eszközök esetén a hello alapértelmezett elérési út hello alkalmazásspecifikus "AppData" mappa.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-316">For Windows devices, hello default path is hello application-specific "AppData" folder.</span></span>

<span data-ttu-id="cd2a3-317">Egy táblahivatkozás hello szerezhetők `GetSyncTable<>` módszert:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-317">A table reference can be obtained using hello `GetSyncTable<>` method:</span></span>

```
var table = client.GetSyncTable<TodoItem>();
```

<span data-ttu-id="cd2a3-318">Nem kell tooauthenticate toouse egy offline tábla.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-318">You do not need tooauthenticate toouse an offline table.</span></span>  <span data-ttu-id="cd2a3-319">Csak akkor kell tooauthenticate amikor hello háttérszolgáltatást kommunikációra.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-319">You only need tooauthenticate when you are communicating with hello backend service.</span></span>

### <span data-ttu-id="cd2a3-320"><a name="syncoffline"></a>Egy kapcsolat nélküli tábla szinkronizálása</span><span class="sxs-lookup"><span data-stu-id="cd2a3-320"><a name="syncoffline"></a>Syncing an Offline Table</span></span>
<span data-ttu-id="cd2a3-321">Kapcsolat nélküli táblák nincsenek szinkronizálva a hello háttéralkalmazással alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-321">Offline tables are not synchronized with hello backend by default.</span></span>  <span data-ttu-id="cd2a3-322">Szinkronizálás két részre van osztva.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-322">Synchronization is split into two pieces.</span></span>  <span data-ttu-id="cd2a3-323">Töltsön le új elemek külön-külön tolható módosításokat.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-323">You can push changes separately from downloading new items.</span></span>  <span data-ttu-id="cd2a3-324">Íme egy jellegzetes szinkronizálási módszere:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-324">Here is a typical sync method:</span></span>

```
public async Task SyncAsync()
{
    ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

    try
    {
        await this.client.SyncContext.PushAsync();

        await this.todoTable.PullAsync(
            //hello first parameter is a query name that is used internally by hello client SDK tooimplement incremental sync.
            //Use a different query name for each unique query in your program
            "allTodoItems",
            this.todoTable.CreateQuery());
    }
    catch (MobileServicePushFailedException exc)
    {
        if (exc.PushResult != null)
        {
            syncErrors = exc.PushResult.Errors;
        }
    }

    // Simple error/conflict handling. A real application would handle hello various errors like network conditions,
    // server conflicts and others via hello IMobileServiceSyncHandler.
    if (syncErrors != null)
    {
        foreach (var error in syncErrors)
        {
            if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
            {
                //Update failed, reverting tooserver's copy.
                await error.CancelAndUpdateItemAsync(error.Result);
            }
            else
            {
                // Discard local change.
                await error.CancelAndDiscardItemAsync();
            }

            Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.", error.TableName, error.Item["id"]);
        }
    }
}
```

<span data-ttu-id="cd2a3-325">Ha első argumentum túl hello`PullAsync` null értékű, akkor nem használja a növekményes szinkronizálás.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-325">If hello first argument too`PullAsync` is null, then incremental sync is not used.</span></span>  <span data-ttu-id="cd2a3-326">Minden egyes szinkronizálási műveletet lekéri az összes rekordot.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-326">Each sync operation retrieves all records.</span></span>

<span data-ttu-id="cd2a3-327">hello SDK hajt végre egy implicit `PushAsync()` előtt az rekord.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-327">hello SDK performs an implicit `PushAsync()` before pulling records.</span></span>

<span data-ttu-id="cd2a3-328">Kezelési ütközésben történik, az egy `PullAsync()` metódust.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-328">Conflict handling happens on a `PullAsync()` method.</span></span>  <span data-ttu-id="cd2a3-329">Akkor is foglalkozik hello ütközéseket azonos módon online táblák.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-329">You can deal with conflicts in hello same way as online tables.</span></span>  <span data-ttu-id="cd2a3-330">hello ütközés hozzák amikor `PullAsync()` során hello insert, update vagy delete neve helyett.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-330">hello conflict is produced when `PullAsync()` is called instead of during hello insert, update, or delete.</span></span> <span data-ttu-id="cd2a3-331">Több ütközés fordulhat elő, ha azok egyetlen MobileServicePushFailedException be vannak becsomagolva.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-331">If multiple conflicts happen, they are bundled into a single MobileServicePushFailedException.</span></span>  <span data-ttu-id="cd2a3-332">Minden egyes hiba külön kezelni.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-332">Handle each failure separately.</span></span>

## <span data-ttu-id="cd2a3-333"><a name="#customapi"></a>Egy egyéni API használata</span><span class="sxs-lookup"><span data-stu-id="cd2a3-333"><a name="#customapi"></a>Work with a custom API</span></span>
<span data-ttu-id="cd2a3-334">Egy egyéni API lehetővé teszi a toodefine egyéni végpontokat visszaállítását kiszolgálói funkciók nem leképezése tooan insert, update, törlése vagy olvasási művelete.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-334">A custom API enables you toodefine custom endpoints that expose server functionality that does not map tooan insert, update, delete, or read operation.</span></span> <span data-ttu-id="cd2a3-335">Egy egyéni API használatával is befolyásolni további üzenetküldés, beleértve az olvasási és HTTP-üzenet fejlécek beállítása meghatározó JSON nem üzenet törzsének formátumban.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-335">By using a custom API, you can have more control over messaging, including reading and setting HTTP message headers and defining a message body format other than JSON.</span></span>

<span data-ttu-id="cd2a3-336">Egy egyéni API hívása hello egyik [InvokeApiAsync] módszerek hello ügyfélen.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-336">You call a custom API by calling one of hello [InvokeApiAsync] methods on hello client.</span></span> <span data-ttu-id="cd2a3-337">Például a következő kódsort hello küld egy POST kérést toohello **completeAll** hello háttérkiszolgálón API:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-337">For example, hello following line of code sends a POST request toohello **completeAll** API on hello backend:</span></span>

```
var result = await client.InvokeApiAsync<MarkAllResult>("completeAll", System.Net.Http.HttpMethod.Post, null);
```

<span data-ttu-id="cd2a3-338">Ezt az űrlapot típusos metódusát, és megköveteli, hogy hello **MarkAllResult** visszatérési, van definiálva típus.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-338">This form is a typed method call and requires that hello **MarkAllResult** return type is defined.</span></span> <span data-ttu-id="cd2a3-339">Típusos és a nem típusos metódusok támogatottak.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-339">Both typed and untyped methods are supported.</span></span>

<span data-ttu-id="cd2a3-340">hello InvokeApiAsync() metódus lefoglalja/api/toohello API toocall kívánja, kivéve, ha hello API kezdődik-e a "/".</span><span class="sxs-lookup"><span data-stu-id="cd2a3-340">hello InvokeApiAsync() method prepends '/api/' toohello API that you wish toocall unless hello API starts with a '/'.</span></span>
<span data-ttu-id="cd2a3-341">Példa:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-341">For example:</span></span>

* <span data-ttu-id="cd2a3-342">`InvokeApiAsync("completeAll",...)`meghívja a /api/completeAll hello háttér</span><span class="sxs-lookup"><span data-stu-id="cd2a3-342">`InvokeApiAsync("completeAll",...)` calls /api/completeAll on hello backend</span></span>
* <span data-ttu-id="cd2a3-343">`InvokeApiAsync("/.auth/me",...)`meghívja a /.auth/me hello háttér</span><span class="sxs-lookup"><span data-stu-id="cd2a3-343">`InvokeApiAsync("/.auth/me",...)` calls /.auth/me on hello backend</span></span>

<span data-ttu-id="cd2a3-344">InvokeApiAsync toocall bármely WebAPI, beleértve a nem meghatározott e WebAPIs az Azure Mobile Apps használhatja.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-344">You can use InvokeApiAsync toocall any WebAPI, including those WebAPIs that are not defined with Azure Mobile Apps.</span></span>  <span data-ttu-id="cd2a3-345">InvokeApiAsync() használatakor hello megfelelő fejlécek, beleértve a hitelesítési fejléceket, hello kérelem küldése.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-345">When you use InvokeApiAsync(), hello appropriate headers, including authentication headers, are sent with hello request.</span></span>

## <span data-ttu-id="cd2a3-346"><a name="authentication"></a>Felhasználók hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="cd2a3-346"><a name="authentication"></a>Authenticate users</span></span>
<span data-ttu-id="cd2a3-347">Mobile Apps hitelesítése és engedélyezése a felhasználók alkalmazás különböző külső Identitásszolgáltatók támogatja: Facebook, a Google, a Microsoft Account, a Twitter és az Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-347">Mobile Apps supports authenticating and authorizing app users using various external identity providers: Facebook, Google, Microsoft Account, Twitter, and Azure Active Directory.</span></span> <span data-ttu-id="cd2a3-348">Beállíthatja engedélyeit a táblák toorestrict hozzáférési megadott műveletek tooonly hitelesített felhasználók.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-348">You can set permissions on tables toorestrict access for specific operations tooonly authenticated users.</span></span> <span data-ttu-id="cd2a3-349">Használhatja a hitelesített felhasználók tooimplement az engedélyezési szabályok hello identitás server parancsfájlokban.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-349">You can also use hello identity of authenticated users tooimplement authorization rules in server scripts.</span></span> <span data-ttu-id="cd2a3-350">További információkért lásd: hello oktatóanyag [Add authentication tooyour alkalmazás].</span><span class="sxs-lookup"><span data-stu-id="cd2a3-350">For more information, see hello tutorial [Add authentication tooyour app].</span></span>

<span data-ttu-id="cd2a3-351">Két hitelesítési forgalom támogatottak: *ügyfél által felügyelt* és *kiszolgáló által kezelt* folyamata.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-351">Two authentication flows are supported: *client-managed* and *server-managed* flow.</span></span> <span data-ttu-id="cd2a3-352">hello kiszolgáló által felügyelt folyamat nyújt hello legegyszerűbb hitelesítési élmény hello szolgáltató webes hitelesítés felület támaszkodnak.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-352">hello server-managed flow provides hello simplest authentication experience, as it relies on hello provider's web authentication interface.</span></span> <span data-ttu-id="cd2a3-353">hello ügyfél által felügyelt folyamat lehetővé teszi a eszközspecifikus képességekkel szorosabb integrációt, a szolgáltatói eszközspecifikus SDK-k támaszkodnak.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-353">hello client-managed flow allows for deeper integration with device-specific capabilities as it relies on provider-specific device-specific SDKs.</span></span>

> [!NOTE]
> <span data-ttu-id="cd2a3-354">Az éles alkalmazásokban lévő ügyfél által felügyelt folyamat használatát javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-354">We recommend using a client-managed flow in your production apps.</span></span>

<span data-ttu-id="cd2a3-355">hitelesítés tooset, regisztrálnia kell az alkalmazás egy vagy több identitás-szolgáltatóktól.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-355">tooset up authentication, you must register your app with one or more identity providers.</span></span>  <span data-ttu-id="cd2a3-356">hello identitásszolgáltató egy ügyfél-Azonosítót és az alkalmazás egy ügyfélkulcsot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-356">hello identity provider generates a client ID and a client secret for your app.</span></span>  <span data-ttu-id="cd2a3-357">A háttérrendszer tooenable Azure App Service hitelesítés/engedélyezés ezután állítsa be a ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-357">These values are then set in your backend tooenable Azure App Service authentication/authorization.</span></span>  <span data-ttu-id="cd2a3-358">További információért hajtsa végre a hello részletes utasításokat az oktatóprogram [Add authentication tooyour alkalmazás].</span><span class="sxs-lookup"><span data-stu-id="cd2a3-358">For more information, follow hello detailed instructions in the tutorial [Add authentication tooyour app].</span></span>

<span data-ttu-id="cd2a3-359">Ez a szakasz hello a következő témakörök ismertetnek:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-359">hello following topics are covered in this section:</span></span>

* [<span data-ttu-id="cd2a3-360">Ügyfél által felügyelt hitelesítés</span><span class="sxs-lookup"><span data-stu-id="cd2a3-360">Client-managed authentication</span></span>](#clientflow)
* [<span data-ttu-id="cd2a3-361">A hitelesítési kiszolgáló által felügyelt</span><span class="sxs-lookup"><span data-stu-id="cd2a3-361">Server-managed authentication</span></span>](#serverflow)
* [<span data-ttu-id="cd2a3-362">Gyorsítótárazás hello hitelesítési jogkivonata</span><span class="sxs-lookup"><span data-stu-id="cd2a3-362">Caching hello authentication token</span></span>](#caching)

### <span data-ttu-id="cd2a3-363"><a name="clientflow"></a>Ügyfél által felügyelt hitelesítés</span><span class="sxs-lookup"><span data-stu-id="cd2a3-363"><a name="clientflow"></a>Client-managed authentication</span></span>
<span data-ttu-id="cd2a3-364">Az alkalmazás is egymástól függetlenül forduljon hello identitásszolgáltató és adja meg hello visszaküldött jogkivonat bejelentkezés során a fájlok.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-364">Your app can independently contact hello identity provider and then provide hello returned token during login with your backend.</span></span> <span data-ttu-id="cd2a3-365">Az ügyfél folyamata lehetővé teszi tooprovide egy egyszeri bejelentkezést a felhasználók vagy a hello identitásszolgáltató tooretrieve további felhasználó adatait.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-365">This client flow enables you tooprovide a single sign-on experience for users or tooretrieve additional user data from hello identity provider.</span></span> <span data-ttu-id="cd2a3-366">Ügyfél folyamat hitelesítés az előnyben részesített toousing a kiszolgáló folyamata, hello identitásszolgáltató SDK több natív UX abba biztosít, és lehetővé teszi, hogy további testreszabási.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-366">Client flow authentication is preferred toousing a server flow as hello identity provider SDK provides a more native UX feel and allows for additional customization.</span></span>

<span data-ttu-id="cd2a3-367">Példák a következő ügyfél-folyamat hitelesítési minták hello áll rendelkezésre:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-367">Examples are provided for hello following client-flow authentication patterns:</span></span>

* [<span data-ttu-id="cd2a3-368">Az Active Directory hitelesítési könyvtár</span><span class="sxs-lookup"><span data-stu-id="cd2a3-368">Active Directory Authentication Library</span></span>](#adal)
* [<span data-ttu-id="cd2a3-369">Facebook-on vagy a Google</span><span class="sxs-lookup"><span data-stu-id="cd2a3-369">Facebook or Google</span></span>](#client-facebook)
* [<span data-ttu-id="cd2a3-370">Élő SDK</span><span class="sxs-lookup"><span data-stu-id="cd2a3-370">Live SDK</span></span>](#client-livesdk)

#### <span data-ttu-id="cd2a3-371"><a name="adal"></a>Hitelesíti a felhasználókat az Active Directory Authentication Library hello</span><span class="sxs-lookup"><span data-stu-id="cd2a3-371"><a name="adal"></a>Authenticate users with hello Active Directory Authentication Library</span></span>
<span data-ttu-id="cd2a3-372">Hello Active Directory Authentication Library (ADAL) tooinitiate felhasználói hitelesítés az Azure Active Directory-hitelesítéssel hello ügyféltől is használhatja.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-372">You can use hello Active Directory Authentication Library (ADAL) tooinitiate user authentication from hello client using Azure Active Directory authentication.</span></span>

1. <span data-ttu-id="cd2a3-373">A mobil-háttéralkalmazás az aad-ben bejelentkezés konfigurálása következő hello [hogyan tooconfigure App Service az Active Directory bejelentkezési] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-373">Configure your mobile app backend for AAD sign-on by following hello [How tooconfigure App Service for Active Directory login] tutorial.</span></span> <span data-ttu-id="cd2a3-374">Győződjön meg arról, hogy toocomplete hello opcionális lépés egy natív ügyfélalkalmazás regisztrációján.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-374">Make sure toocomplete hello optional step of registering a native client application.</span></span>
2. <span data-ttu-id="cd2a3-375">A Visual Studio és Xamarin Studio, nyissa meg a projektet, és adja hozzá egy hivatkozást toothe `Microsoft.IdentityModel.CLients.ActiveDirectory` NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-375">In Visual Studio or Xamarin Studio, open your project and add a reference toothe `Microsoft.IdentityModel.CLients.ActiveDirectory` NuGet package.</span></span> <span data-ttu-id="cd2a3-376">Ha keres, vegye fel az előzetes verzióját.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-376">When searching, include pre-release versions.</span></span>
3. <span data-ttu-id="cd2a3-377">Adja hozzá a következő kód tooyour alkalmazás, toohello platformra szerint hello.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-377">Add hello following code tooyour application, according toohello platform you are using.</span></span> <span data-ttu-id="cd2a3-378">Minden győződjön meg a következő cserékhez hello:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-378">In each, make hello following replacements:</span></span>

   * <span data-ttu-id="cd2a3-379">Cserélje le **INSERT-SZOLGÁLTATÓ-Itt** hello nevű hello bérlő, amelyben az alkalmazás kiépítve.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-379">Replace **INSERT-AUTHORITY-HERE** with hello name of hello tenant in which you provisioned your application.</span></span> <span data-ttu-id="cd2a3-380">A következő formátumban kell megadni https://login.microsoftonline.com/contoso.onmicrosoft.com. Ez az érték lehet másolni az Azure Active Directory hello tartományi lapfülén hello [a klasszikus Azure portálon].</span><span class="sxs-lookup"><span data-stu-id="cd2a3-380">The format should be https://login.microsoftonline.com/contoso.onmicrosoft.com. This value can be copied from hello Domain tab in your Azure Active Directory in hello [Azure classic portal].</span></span>
   * <span data-ttu-id="cd2a3-381">Cserélje le **INSERT-erőforrás-azonosító-Itt** hello ügyfél-azonosítójú a mobil-háttéralkalmazást.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-381">Replace **INSERT-RESOURCE-ID-HERE** with hello client ID for your mobile app backend.</span></span> <span data-ttu-id="cd2a3-382">Ügyfél-azonosító hello szerezhet be a hello **speciális** lap **Azure Active Directory beállításai** hello portálon.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-382">You can obtain hello client ID from hello **Advanced** tab under **Azure Active Directory Settings** in hello portal.</span></span>
   * <span data-ttu-id="cd2a3-383">Cserélje le **INSERT-ügyfél-azonosító-Itt** hello ügyfél-azonosítójú hello natív ügyfélalkalmazás másolta.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-383">Replace **INSERT-CLIENT-ID-HERE** with hello client ID you copied from hello native client application.</span></span>
   * <span data-ttu-id="cd2a3-384">Cserélje le **INSERT-REDIRECT-URI-Itt** a hellyel való */.auth/login/done* végpont hello HTTPS protokollt használ.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-384">Replace **INSERT-REDIRECT-URI-HERE** with your site's */.auth/login/done* endpoint, using hello HTTPS scheme.</span></span> <span data-ttu-id="cd2a3-385">Ez az érték túl hasonló legyen*https://contoso.azurewebsites.net/.auth/login/done*.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-385">This value should be similar too*https://contoso.azurewebsites.net/.auth/login/done*.</span></span>

     <span data-ttu-id="cd2a3-386">az egyes platformokon szükséges hello kódot a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-386">hello code needed for each platform follows:</span></span>

     <span data-ttu-id="cd2a3-387">**Windows:**</span><span class="sxs-lookup"><span data-stu-id="cd2a3-387">**Windows:**</span></span>

    ```
    private MobileServiceUser user;
    private async Task AuthenticateAsync()
    {

        string authority = "INSERT-AUTHORITY-HERE";
        string resourceId = "INSERT-RESOURCE-ID-HERE";
        string clientId = "INSERT-CLIENT-ID-HERE";
        string redirectUri = "INSERT-REDIRECT-URI-HERE";
        while (user == null)
        {
            string message;
            try
            {
                AuthenticationContext ac = new AuthenticationContext(authority);
                AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                    new Uri(redirectUri), new PlatformParameters(PromptBehavior.Auto, false) );
                JObject payload = new JObject();
                payload["access_token"] = ar.AccessToken;
                user = await App.MobileService.LoginAsync(
                    MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
                message = string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }
    ```

     <span data-ttu-id="cd2a3-388">**Xamarin.iOS**</span><span class="sxs-lookup"><span data-stu-id="cd2a3-388">**Xamarin.iOS**</span></span>

    ```
    private MobileServiceUser user;
    private async Task AuthenticateAsync(UIViewController view)
    {

        string authority = "INSERT-AUTHORITY-HERE";
        string resourceId = "INSERT-RESOURCE-ID-HERE";
        string clientId = "INSERT-CLIENT-ID-HERE";
        string redirectUri = "INSERT-REDIRECT-URI-HERE";
        try
        {
            AuthenticationContext ac = new AuthenticationContext(authority);
            AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                new Uri(redirectUri), new PlatformParameters(view));
            JObject payload = new JObject();
            payload["access_token"] = ar.AccessToken;
            user = await client.LoginAsync(
                MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
        }
        catch (Exception ex)
        {
            Console.Error.WriteLine(@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
        }
    }
    ```

     <span data-ttu-id="cd2a3-389">**Xamarin.Android**</span><span class="sxs-lookup"><span data-stu-id="cd2a3-389">**Xamarin.Android**</span></span>

    ```
    private MobileServiceUser user;
    private async Task AuthenticateAsync()
    {

        string authority = "INSERT-AUTHORITY-HERE";
        string resourceId = "INSERT-RESOURCE-ID-HERE";
        string clientId = "INSERT-CLIENT-ID-HERE";
        string redirectUri = "INSERT-REDIRECT-URI-HERE";
        try
        {
            AuthenticationContext ac = new AuthenticationContext(authority);
            AuthenticationResult ar = await ac.AcquireTokenAsync(resourceId, clientId,
                new Uri(redirectUri), new PlatformParameters(this));
            JObject payload = new JObject();
            payload["access_token"] = ar.AccessToken;
            user = await client.LoginAsync(
                MobileServiceAuthenticationProvider.WindowsAzureActiveDirectory, payload);
        }
        catch (Exception ex)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(ex.Message);
            builder.SetTitle("You must log in. Login Required");
            builder.Create().Show();
        }
    }
    protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
    {

        base.OnActivityResult(requestCode, resultCode, data);
        AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
    }
    ```

#### <span data-ttu-id="cd2a3-390"><a name="client-facebook"></a>Egyszeri bejelentkezés Facebook-on vagy a Google származó jogkivonat használatával</span><span class="sxs-lookup"><span data-stu-id="cd2a3-390"><a name="client-facebook"></a>Single Sign-On using a token from Facebook or Google</span></span>
<span data-ttu-id="cd2a3-391">Ahogy az a kódrészletet a Facebook-on vagy a Google hello ügyfél folyamatában használhatja.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-391">You can use hello client flow as shown in this snippet for Facebook or Google.</span></span>

```
var token = new JObject();
// Replace access_token_value with actual value of your access token obtained
// using hello Facebook or Google SDK.
token.Add("access_token", "access_token_value");

private MobileServiceUser user;
private async Task AuthenticateAsync()
{
    while (user == null)
    {
        string message;
        try
        {
            // Change MobileServiceAuthenticationProvider.Facebook
            // tooMobileServiceAuthenticationProvider.Google if using Google auth.
            user = await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
            message = string.Format("You are now logged in - {0}", user.UserId);
        }
        catch (InvalidOperationException)
        {
            message = "You must log in. Login Required";
        }

        var dialog = new MessageDialog(message);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
}
```

#### <span data-ttu-id="cd2a3-392"><a name="client-livesdk"></a>Az egyszeri bejelentkezés a Microsoft-Account hello Live SDK</span><span class="sxs-lookup"><span data-stu-id="cd2a3-392"><a name="client-livesdk"></a>Single Sign On using Microsoft Account with hello Live SDK</span></span>
<span data-ttu-id="cd2a3-393">tooauthenticate felhasználók regisztrálnia kell a Microsoft-fiók fejlesztői központ hello az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-393">tooauthenticate users, you must register your app at hello Microsoft account Developer Center.</span></span> <span data-ttu-id="cd2a3-394">A regisztrációs adatokat konfigurálja a mobilalkalmazás háttérrendszerének.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-394">Configure the registration details on your Mobile App backend.</span></span> <span data-ttu-id="cd2a3-395">toocreate Microsoft fiók regisztrálása, és csatlakoztassa tooyour mobil-háttéralkalmazást, teljes hello szükséges lépések [regisztrálja az alkalmazást toouse egy Microsoft-fiók bejelentkezési].</span><span class="sxs-lookup"><span data-stu-id="cd2a3-395">toocreate a Microsoft account registration and connect it tooyour Mobile App backend, complete hello steps in [Register your app toouse a Microsoft account login].</span></span> <span data-ttu-id="cd2a3-396">Ha az alkalmazás Windows áruház és a Windows Phone 8/Silverlight verzióit, először regisztráljon hello Windows Áruházbeli verziójához.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-396">If you have both Windows Store and Windows Phone 8/Silverlight versions of your app, register hello Windows Store version first.</span></span>

<span data-ttu-id="cd2a3-397">hello alábbira Live SDK használatával és hello által visszaadott token toosign tooyour Mobile Apps-háttéralkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-397">hello following code authenticates using Live SDK and uses hello returned token toosign in tooyour Mobile App backend.</span></span>

```
private LiveConnectSession session;
    //private static string clientId = "<microsoft-account-client-id>";
private async System.Threading.Tasks.Task AuthenticateAsync()
{

    // Get hello URL hello Mobile App backend.
    var serviceUrl = App.MobileService.ApplicationUri.AbsoluteUri;

    // Create hello authentication client for Windows Store using hello service URL.
    LiveAuthClient liveIdClient = new LiveAuthClient(serviceUrl);
    //// Create hello authentication client for Windows Phone using hello client ID of hello registration.
    //LiveAuthClient liveIdClient = new LiveAuthClient(clientId);

    while (session == null)
    {
        // Request hello authentication token from hello Live authentication service.
        // hello wl.basic scope should always be requested.  Other scopes can be added
        LiveLoginResult result = await liveIdClient.LoginAsync(new string[] { "wl.basic" });
        if (result.Status == LiveConnectSessionStatus.Connected)
        {
            session = result.Session;

            // Get information about hello logged-in user.
            LiveConnectClient client = new LiveConnectClient(session);
            LiveOperationResult meResult = await client.GetAsync("me");

            // Use hello Microsoft account auth token toosign in tooApp Service.
            MobileServiceUser loginResult = await App.MobileService
                .LoginWithMicrosoftAccountAsync(result.Session.AuthenticationToken);

            // Display a personalized sign-in greeting.
            string title = string.Format("Welcome {0}!", meResult.Result["first_name"]);
            var message = string.Format("You are now logged in - {0}", loginResult.UserId);
            var dialog = new MessageDialog(message, title);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
        else
        {
            session = null;
            var dialog = new MessageDialog("You must log in.", "Login Required");
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }
}
```

<span data-ttu-id="cd2a3-398">További információkért lásd: hello [Windows Live SDK] dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-398">For more information, see hello [Windows Live SDK] documentation.</span></span>

### <span data-ttu-id="cd2a3-399"><a name="serverflow"></a>A hitelesítési kiszolgáló által felügyelt</span><span class="sxs-lookup"><span data-stu-id="cd2a3-399"><a name="serverflow"></a>Server-managed authentication</span></span>
<span data-ttu-id="cd2a3-400">Miután regisztrálta az identitásszolgáltató, hívja hello [LoginAsync] hello [MobileServiceClient] hello rendelkező metódust [MobileServiceAuthenticationProvider] a szolgáltató érték.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-400">Once you have registered your identity provider, call hello [LoginAsync] method on hello [MobileServiceClient] with hello [MobileServiceAuthenticationProvider] value of your provider.</span></span> <span data-ttu-id="cd2a3-401">Például hello alábbira indít el a kiszolgáló folyamata bejelentkezés Facebook-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-401">For example, hello following code initiates a server flow sign-in by using Facebook.</span></span>

```
private MobileServiceUser user;
private async System.Threading.Tasks.Task Authenticate()
{
    while (user == null)
    {
        string message;
        try
        {
            user = await client
                .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
            message =
                string.Format("You are now logged in - {0}", user.UserId);
        }
        catch (InvalidOperationException)
        {
            message = "You must log in. Login Required";
        }

        var dialog = new MessageDialog(message);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
}
```

<span data-ttu-id="cd2a3-402">Ha használja az identitásszolgáltató Facebook-on kívül, hello értékének módosítása [MobileServiceAuthenticationProvider] toohello értékét a szolgáltatónál.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-402">If you are using an identity provider other than Facebook, change hello value of [MobileServiceAuthenticationProvider] toohello value for your provider.</span></span>

<span data-ttu-id="cd2a3-403">Egy kiszolgáló folyamatában Azure App Service hello bejelentkezési oldal hello kijelölt szolgáltató megjelenítésével kezeli a hello OAuth hitelesítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-403">In a server flow, Azure App Service manages hello OAuth authentication flow by displaying hello sign-in page of hello selected provider.</span></span>  <span data-ttu-id="cd2a3-404">Egyszer hello identitás-szolgáltató értéket ad eredményül, Azure App Service egy App Service hitelesítési jogkivonatot állít elő.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-404">Once hello identity provider returns, Azure App Service generates an App Service authentication token.</span></span> <span data-ttu-id="cd2a3-405">Hello [LoginAsync] metódus értéket ad vissza egy [MobileServiceUser], amely biztosítja, hogy mindkét hello [UserId] hello a hitelesített felhasználó és hello [ MobileServiceAuthenticationToken], mint egy JSON webes jogkivonatot (JWT).</span><span class="sxs-lookup"><span data-stu-id="cd2a3-405">hello [LoginAsync] method returns a [MobileServiceUser], which provides both hello [UserId] of hello authenticated user and hello [MobileServiceAuthenticationToken], as a JSON web token (JWT).</span></span> <span data-ttu-id="cd2a3-406">Ez a token gyorsítótárazható, és újra felhasználható, amíg le nem jár.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-406">This token can be cached and reused until it expires.</span></span> <span data-ttu-id="cd2a3-407">További információkért lásd: [gyorsítótárazását hello hitelesítésére szolgáló jogkivonat](#caching).</span><span class="sxs-lookup"><span data-stu-id="cd2a3-407">For more information, see [Caching hello authentication token](#caching).</span></span>

### <span data-ttu-id="cd2a3-408"><a name="caching"></a>Gyorsítótárazás hello hitelesítési jogkivonata</span><span class="sxs-lookup"><span data-stu-id="cd2a3-408"><a name="caching"></a>Caching hello authentication token</span></span>
<span data-ttu-id="cd2a3-409">Bizonyos esetekben bejelentkezési toohello hello telefonhívás elkerülhető hello első sikeres hitelesítés után hello hitelesítésére szolgáló jogkivonat hello szolgáltató elhelyezésével.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-409">In some cases, hello call toohello login method can be avoided after hello first successful authentication by storing hello authentication token from hello provider.</span></span>  <span data-ttu-id="cd2a3-410">Windows áruház és az UWP-alkalmazások használhatják [PasswordVault] toocache a jelenlegi hitelesítési lexikális elem után egy sikeres bejelentkezés, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-410">Windows Store and UWP apps can use [PasswordVault] toocache the current authentication token after a successful sign-in, as follows:</span></span>

```
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook);

PasswordVault vault = new PasswordVault();
vault.Add(new PasswordCredential("Facebook", client.currentUser.UserId,
    client.currentUser.MobileServiceAuthenticationToken));
```

<span data-ttu-id="cd2a3-411">hello UserId érték hello felhasználónév a hello hitelesítő adatok tárolása és hello lexikális elem hello hello jelszó tárolja.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-411">hello UserId value is stored as hello UserName of hello credential and hello token is hello stored as hello Password.</span></span> <span data-ttu-id="cd2a3-412">A következő kezdő vállalkozások számára, ellenőrizheti a hello **PasswordVault** a gyorsítótárazott hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-412">On subsequent start-ups, you can check hello **PasswordVault** for cached credentials.</span></span> <span data-ttu-id="cd2a3-413">hello következő példában gyorsítótárazott hitelesítő adatokat, ha találhatók, valamint egyéb próbálja újra a hello háttéralkalmazással tooauthenticate:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-413">hello following example uses cached credentials when they are found, and otherwise attempts tooauthenticate again with hello backend:</span></span>

```
// Try tooretrieve stored credentials.
var creds = vault.FindAllByResource("Facebook").FirstOrDefault();
if (creds != null)
{
    // Create hello current user from hello stored credentials.
    client.currentUser = new MobileServiceUser(creds.UserName);
    client.currentUser.MobileServiceAuthenticationToken =
        vault.Retrieve("Facebook", creds.UserName).Password;
}
else
{
    // Regular login flow and cache hello token as shown above.
}
```

<span data-ttu-id="cd2a3-414">Amikor a felhasználó kijelentkezik, el kell távolítani a hello tárolt hitelesítő adatai, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-414">When you sign out a user, you must also remove hello stored credential, as follows:</span></span>

```
client.Logout();
vault.Remove(vault.Retrieve("Facebook", client.currentUser.UserId));
```

<span data-ttu-id="cd2a3-415">Xamarin-alkalmazásokat használni hello [Xamarin.Auth] API-k toosecurely adattárolóhoz használandó hitelesítő adatok a egy **fiók** objektum.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-415">Xamarin    apps use hello [Xamarin.Auth] APIs toosecurely store credentials in an **Account** object.</span></span> <span data-ttu-id="cd2a3-416">Ezen API-k használatának példája, lásd: hello [AuthStore.cs] hello a kódfájl [minta megosztása ContosoMoments fénykép](https://github.com/azure-appservice-samples/ContosoMoments).</span><span class="sxs-lookup"><span data-stu-id="cd2a3-416">For an example of using these APIs, see hello [AuthStore.cs] code file in hello [ContosoMoments photo sharing sample](https://github.com/azure-appservice-samples/ContosoMoments).</span></span>

<span data-ttu-id="cd2a3-417">Ügyfél által felügyelt hitelesítés használatakor például a Facebook- vagy Twitter szolgáltatótól kapott jogkivonat hello is képes gyorsítótárazni.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-417">When you use client-managed authentication, you can also cache hello access token obtained from your provider such as Facebook or Twitter.</span></span> <span data-ttu-id="cd2a3-418">Ez a token lehet megadott toorequest egy új hitelesítési jogkivonat hello háttérrendszerből, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-418">This token can be supplied toorequest a new authentication token from hello backend, as follows:</span></span>

```
var token = new JObject();
// Replace <your_access_token_value> with actual value of your access token
token.Add("access_token", "<your_access_token_value>");

// Authenticate using hello access token.
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
```

## <span data-ttu-id="cd2a3-419"><a name="pushnotifications"></a>Leküldéses értesítések</span><span class="sxs-lookup"><span data-stu-id="cd2a3-419"><a name="pushnotifications"></a>Push Notifications</span></span>
<span data-ttu-id="cd2a3-420">a következő témakörök borítóján leküldéses értesítések hello:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-420">hello following topics cover Push Notifications:</span></span>

* [<span data-ttu-id="cd2a3-421">A leküldéses értesítések regisztrálása</span><span class="sxs-lookup"><span data-stu-id="cd2a3-421">Register for Push Notifications</span></span>](#register-for-push)
* [<span data-ttu-id="cd2a3-422">Szerezzen be egy Windows Áruházbeli csomag biztonsági azonosítója</span><span class="sxs-lookup"><span data-stu-id="cd2a3-422">Obtain a Windows Store package SID</span></span>](#package-sid)
* [<span data-ttu-id="cd2a3-423">Platformok közötti sablonok regisztrálása</span><span class="sxs-lookup"><span data-stu-id="cd2a3-423">Register with Cross-platform templates</span></span>](#register-xplat)

### <span data-ttu-id="cd2a3-424"><a name="register-for-push"></a>Útmutató: a leküldéses értesítések regisztrálása</span><span class="sxs-lookup"><span data-stu-id="cd2a3-424"><a name="register-for-push"></a>How to: Register for Push Notifications</span></span>
<span data-ttu-id="cd2a3-425">hello Mobile Apps-ügyfél lehetővé teszi az Azure Notification Hubs leküldéses értesítések tooregister.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-425">hello Mobile Apps client enables you tooregister for push notifications with Azure Notification Hubs.</span></span> <span data-ttu-id="cd2a3-426">Amikor regisztrál, a hello platform-specifikus származó leírót igényelni Push Notification szolgáltatás (PNS).</span><span class="sxs-lookup"><span data-stu-id="cd2a3-426">When registering, you obtain a handle that you obtain from hello platform-specific Push Notification Service (PNS).</span></span> <span data-ttu-id="cd2a3-427">Azt adja meg ezt az értéket a címkékkel együtt hello regisztrációs létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-427">You then provide this value along with any tags when you create hello registration.</span></span> <span data-ttu-id="cd2a3-428">hello alábbira regisztrál a Windows-alkalmazást a leküldéses értesítések hello Windows értesítési szolgáltatása (WNS):</span><span class="sxs-lookup"><span data-stu-id="cd2a3-428">hello following code registers your Windows app for push notifications with hello Windows Notification Service (WNS):</span></span>

```
private async void InitNotificationsAsync()
{
    // Request a push notification channel.
    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // Register for notifications using hello new channel.
    await MobileService.GetPush().RegisterNativeAsync(channel.Uri, null);
}
```

<span data-ttu-id="cd2a3-429">Ha tooWNS leküldendő, akkor meg kell [Windows áruház csomagot SID](#package-sid).</span><span class="sxs-lookup"><span data-stu-id="cd2a3-429">If you are pushing tooWNS, then you MUST [obtain a Windows Store package SID](#package-sid).</span></span>  <span data-ttu-id="cd2a3-430">További információ a Windows-alkalmazásokra beleértve a hogyan sablon regisztrációkhoz tooregister: [Hozzáadás leküldéses értesítések tooyour app].</span><span class="sxs-lookup"><span data-stu-id="cd2a3-430">For more information on Windows apps, including how tooregister for template registrations, see [Add push notifications tooyour app].</span></span>

<span data-ttu-id="cd2a3-431">Címkék kér hello ügyfél nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-431">Requesting tags from hello client is not supported.</span></span>  <span data-ttu-id="cd2a3-432">A regisztrációs csendes eldobott címke kérelmek.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-432">Tag Requests are silently dropped from registration.</span></span>
<span data-ttu-id="cd2a3-433">Ha a tooregister a címkékkel rendelkező eszköz, hozzon létre egy egyéni API-t használó hello Notification Hubs API tooperform hello regisztrációs az Ön nevében.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-433">If you wish tooregister your device with tags, create a Custom API that uses hello Notification Hubs API tooperform hello registration on your behalf.</span></span>  <span data-ttu-id="cd2a3-434">[Hello egyéni API hívása](#customapi) helyett hello `RegisterNativeAsync()` metódust.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-434">[Call hello Custom API](#customapi) instead of hello `RegisterNativeAsync()` method.</span></span>

### <span data-ttu-id="cd2a3-435"><a name="package-sid"></a>Hogyan: szerezze be a Windows áruház csomag biztonsági azonosítója</span><span class="sxs-lookup"><span data-stu-id="cd2a3-435"><a name="package-sid"></a>How to: Obtain a Windows Store package SID</span></span>
<span data-ttu-id="cd2a3-436">A Windows Áruházbeli alkalmazások leküldéses értesítések engedélyezése a csomag biztonsági azonosítója szükséges.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-436">A package SID is needed for enabling push notifications in Windows Store apps.</span></span>  <span data-ttu-id="cd2a3-437">a csomag biztonsági azonosítója, tooreceive hello Windows áruház regisztrálhatja alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-437">tooreceive a package SID, register your application with hello Windows Store.</span></span>

<span data-ttu-id="cd2a3-438">tooobtain ezt az értéket:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-438">tooobtain this value:</span></span>

1. <span data-ttu-id="cd2a3-439">A Visual Studio Solution Explorerben, kattintson a jobb gombbal hello Windows Áruházbeli alkalmazás projektjére, kattintson a **tároló** > **hello Áruházbeli alkalmazás társítása...** .</span><span class="sxs-lookup"><span data-stu-id="cd2a3-439">In Visual Studio Solution Explorer, right-click hello Windows Store app project, click **Store** > **Associate App with hello Store...**.</span></span>
2. <span data-ttu-id="cd2a3-440">Hello varázslóban kattintson **következő**, jelentkezzen be Microsoft-fiókjával, adjon meg egy nevet az alkalmazáshoz a **lefoglalni egy új alkalmazás neve**, majd kattintson a **tartalék**.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-440">In hello wizard, click **Next**, sign in with your Microsoft account, type a name for your app in **Reserve a new app name**, then click **Reserve**.</span></span>
3. <span data-ttu-id="cd2a3-441">Sikeresen létrejött, jelölje be hello alkalmazásnév hello alkalmazás regisztrálása után kattintson **következő**, és kattintson a **társítása**.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-441">After hello app registration is successfully created, select hello app name, click **Next**, and then click **Associate**.</span></span>
4. <span data-ttu-id="cd2a3-442">Jelentkezzen be toohello [Windows fejlesztői központ] a Microsoft Account.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-442">Log in toohello [Windows Dev Center] using your Microsoft Account.</span></span> <span data-ttu-id="cd2a3-443">A **alkalmazásaimat**, kattintson a létrehozott hello alkalmazás regisztrációja.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-443">Under **My apps**, click hello app registration you created.</span></span>
5. <span data-ttu-id="cd2a3-444">Kattintson a **Alkalmazáskezelés** > **identitását**, majd görgessen lefelé toofind a **CSOMAGAZONOSÍTÓT**.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-444">Click **App management** > **App identity**, and then scroll down toofind your **Package SID**.</span></span>

<span data-ttu-id="cd2a3-445">Számos felhasználási hello csomag biztonsági azonosítója kezelni egy URI-azonosítóként, ebben az esetben szüksége toouse *ms-app: / /* hello sémát.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-445">Many uses of hello package SID treat it as a URI, in which case you need toouse *ms-app://* as hello scheme.</span></span> <span data-ttu-id="cd2a3-446">Jegyezze meg a csomag biztonsági azonosítója, ez az érték előtagjaként hozzáfűzésével megfelelő hello verzióját.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-446">Make note of hello version of your package SID formed by concatenating this value as a prefix.</span></span>

<span data-ttu-id="cd2a3-447">Xamarin-alkalmazásokba szükséges néhány további kódrészletet toobe képes tooregister hello iOS vagy Android rendszereken futó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-447">Xamarin apps require some additional code toobe able tooregister an app running on hello iOS or Android platforms.</span></span> <span data-ttu-id="cd2a3-448">További információkért lásd: a platformra hello témakör:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-448">For more information, see hello topic for your platform:</span></span>

* [<span data-ttu-id="cd2a3-449">Xamarin.Android</span><span class="sxs-lookup"><span data-stu-id="cd2a3-449">Xamarin.Android</span></span>](app-service-mobile-xamarin-android-get-started-push.md#add-push)
* [<span data-ttu-id="cd2a3-450">Xamarin.iOS</span><span class="sxs-lookup"><span data-stu-id="cd2a3-450">Xamarin.iOS</span></span>](app-service-mobile-xamarin-ios-get-started-push.md#add-push-notifications-to-your-app)

### <span data-ttu-id="cd2a3-451"><a name="register-xplat"></a>Útmutató: regisztráció leküldéses sablonok toosend platformfüggetlen értesítések</span><span class="sxs-lookup"><span data-stu-id="cd2a3-451"><a name="register-xplat"></a>How to: Register push templates toosend cross-platform notifications</span></span>
<span data-ttu-id="cd2a3-452">tooregister sablonok használata hello `RegisterAsync()` metódus hello sablonok, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-452">tooregister templates, use hello `RegisterAsync()` method with hello templates, as follows:</span></span>

```
JObject templates = myTemplates();
MobileService.GetPush().RegisterAsync(channel.Uri, templates);
```

<span data-ttu-id="cd2a3-453">A sablonok kell `JObject` meg kell adnia, és tartalmazhat több sablonok a következő JSON formátummal hello:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-453">Your templates should be `JObject` types and can contain multiple templates in hello following JSON format:</span></span>

```
public JObject myTemplates()
{
    // single template for Windows Notification Service toast
    var template = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(message)</text></binding></visual></toast>";

    var templates = new JObject
    {
        ["generic-message"] = new JObject
        {
            ["body"] = template,
            ["headers"] = new JObject
            {
                ["X-WNS-Type"] = "wns/toast"
            },
            ["tags"] = new JArray()
        },
        ["more-templates"] = new JObject {...}
    };
    return templates;
}
```

<span data-ttu-id="cd2a3-454">hello metódus **RegisterAsync()** másodlagos Csempéket is fogad:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-454">hello method **RegisterAsync()** also accepts Secondary Tiles:</span></span>

```
MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);
```

<span data-ttu-id="cd2a3-455">Található összes kódcímkének számítógépnél tisztító vannak a biztonsági regisztrálása során.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-455">All tags are stripped away during registration for security.</span></span> <span data-ttu-id="cd2a3-456">tooadd címkéket tooinstallations vagy sablonok telepítések belül, lásd: [együttműködnek a háttérkiszolgálón a hello .NET SDK az Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="cd2a3-456">tooadd tags tooinstallations or templates within installations, see [Work with hello .NET backend server SDK for Azure Mobile Apps].</span></span>

<span data-ttu-id="cd2a3-457">toosend értesítések ezen regisztrált sablonok használatával tekintse meg a toohello [Notification Hubs API-k].</span><span class="sxs-lookup"><span data-stu-id="cd2a3-457">toosend notifications utilizing these registered templates, refer toohello [Notification Hubs APIs].</span></span>

## <span data-ttu-id="cd2a3-458"><a name="misc"></a>Vegyes kapcsolatos témakörök</span><span class="sxs-lookup"><span data-stu-id="cd2a3-458"><a name="misc"></a>Miscellaneous Topics</span></span>
### <span data-ttu-id="cd2a3-459"><a name="errors"></a>Hogyan: hibák kezelésének</span><span class="sxs-lookup"><span data-stu-id="cd2a3-459"><a name="errors"></a>How to: Handle errors</span></span>
<span data-ttu-id="cd2a3-460">Ha hiba lép fel hello háttér, hello ügyfél SDK kivált egy `MobileServiceInvalidOperationException`.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-460">When an error occurs in hello backend, hello client SDK raises a `MobileServiceInvalidOperationException`.</span></span>  <span data-ttu-id="cd2a3-461">Az alábbi példában látható hogyan toohandle hello háttéralkalmazása által visszaadott kivétel:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-461">The following example shows how toohandle an exception that is returned by hello backend:</span></span>

```
private async void InsertTodoItem(TodoItem todoItem)
{
    // This code inserts a new TodoItem into hello database. When hello operation completes
    // and App Service has assigned an Id, hello item is added toohello CollectionView
    try
    {
        await todoTable.InsertAsync(todoItem);
        items.Add(todoItem);
    }
    catch (MobileServiceInvalidOperationException e)
    {
        // Handle error
    }
}
```

<span data-ttu-id="cd2a3-462">Egy másik példa való hibaállapotok hello található [Mobile Apps fájlok minta].</span><span class="sxs-lookup"><span data-stu-id="cd2a3-462">Another example of dealing with error conditions can be found in hello [Mobile Apps Files Sample].</span></span> <span data-ttu-id="cd2a3-463">A [LoggingHandler] példa biztosít egy naplózási delegált kezelő toolog hello kérelmek toohello háttér végeznek.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-463">The [LoggingHandler] example provides a logging delegate handler toolog hello requests being made toohello backend.</span></span>

### <span data-ttu-id="cd2a3-464"><a name="headers"></a>Hogyan: testreszabása kérelem fejlécei</span><span class="sxs-lookup"><span data-stu-id="cd2a3-464"><a name="headers"></a>How to: Customize request headers</span></span>
<span data-ttu-id="cd2a3-465">toosupport az adott alkalmazást esetben szüksége lehet a Mobile Apps-háttéralkalmazás hello toocustomize kommunikál.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-465">toosupport your specific app scenario, you might need toocustomize communication with hello Mobile App backend.</span></span> <span data-ttu-id="cd2a3-466">Például előfordulhat, hogy szeretné, hogy egy egyéni fejlécet tooevery kimenő kérelem tooadd, vagy még akkor is módosíthatja a válaszok állapotkódok.</span><span class="sxs-lookup"><span data-stu-id="cd2a3-466">For example, you may want tooadd a custom header tooevery outgoing request or even change responses status codes.</span></span> <span data-ttu-id="cd2a3-467">Használhat egyéni [DelegatingHandler], a példában a következő hello:</span><span class="sxs-lookup"><span data-stu-id="cd2a3-467">You can use a custom [DelegatingHandler], as in hello following example:</span></span>

```
public async Task CallClientWithHandler()
{
    MobileServiceClient client = new MobileServiceClient("AppUrl", new MyHandler());
    IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
    var newItem = new TodoItem { Text = "Hello world", Complete = false };
    await todoTable.InsertAsync(newItem);
}

public class MyHandler : DelegatingHandler
{
    protected override async Task<HttpResponseMessage>
        SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)
    {
        // Change hello request-side here based on hello HttpRequestMessage
        request.Headers.Add("x-my-header", "my value");

        // Do hello request
        var response = await base.SendAsync(request, cancellationToken);

        // Change hello response-side here based on hello HttpResponseMessage

        // Return hello modified response
        return response;
    }
}
```


<!-- Anchors. -->


<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-windows-store-dotnet-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[4]: https://msdn.microsoft.com/en-us/library/azure/mt419521(v=azure.10).aspx
[5]: https://github.com/Azure-Samples
[6]: http://www.newtonsoft.com/json/help/html/Properties_T_Newtonsoft_Json_JsonPropertyAttribute.htm
[7]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#define-table-controller
[8]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[9]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/
[10]: http://www.symbolsource.org/
[11]: http://www.symbolsource.org/Public/Wiki/Using
[12]: https://msdn.microsoft.com/en-us/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient(v=azure.10).aspx

[Add authentication tooyour alkalmazás]: app-service-mobile-windows-store-dotnet-get-started-users.md
[az Azure Mobile Apps Offline adatszinkronizálás]: app-service-mobile-offline-data-sync.md
[Hozzáadás leküldéses értesítések tooyour app]: app-service-mobile-windows-store-dotnet-get-started-push.md
[regisztrálja az alkalmazást toouse egy Microsoft-fiók bejelentkezési]: app-service-mobile-how-to-configure-microsoft-authentication.md
[hogyan tooconfigure App Service az Active Directory bejelentkezési]: app-service-mobile-how-to-configure-active-directory-authentication.md

<!-- Microsoft URLs. -->
[MobileServiceCollection]: https://msdn.microsoft.com/en-us/library/azure/dn250636(v=azure.10).aspx
[MobileServiceIncrementalLoadingCollection]: https://msdn.microsoft.com/en-us/library/azure/dn268408(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider(v=azure.10).aspx
[MobileServiceUser]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser(v=azure.10).aspx
[ MobileServiceAuthenticationToken]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken(v=azure.10).aspx
[GetTable]: https://msdn.microsoft.com/en-us/library/azure/jj554275(v=azure.10).aspx
[táblázatot hoz létre hivatkozást tooan típusos]: https://msdn.microsoft.com/en-us/library/azure/jj554278(v=azure.10).aspx
[DeleteAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296407(v=azure.10).aspx
[IncludeTotalCount]: https://msdn.microsoft.com/en-us/library/azure/dn250560(v=azure.10).aspx
[InsertAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296400(v=azure.10).aspx
[InvokeApiAsync]: https://msdn.microsoft.com/en-us/library/azure/dn268343(v=azure.10).aspx
[LoginAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296411(v=azure.10).aspx
[LookupAsync]: https://msdn.microsoft.com/en-us/library/azure/jj871654(v=azure.10).aspx
[OrderBy]: https://msdn.microsoft.com/en-us/library/azure/dn250572(v=azure.10).aspx
[OrderByDescending]: https://msdn.microsoft.com/en-us/library/azure/dn250568(v=azure.10).aspx
[ReadAsync]: https://msdn.microsoft.com/en-us/library/azure/mt691741(v=azure.10).aspx
[érvénybe]: https://msdn.microsoft.com/en-us/library/azure/dn250574(v=azure.10).aspx
[válasszon]: https://msdn.microsoft.com/en-us/library/azure/dn250569(v=azure.10).aspx
[kihagyása]: https://msdn.microsoft.com/en-us/library/azure/dn250573(v=azure.10).aspx
[UpdateAsync]: https://msdn.microsoft.com/en-us/library/azure/dn250536.(v=azure.10)aspx
[Felhasználói azonosítóját]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid(v=azure.10).aspx
[ahol]: https://msdn.microsoft.com/en-us/library/azure/dn250579(v=azure.10).aspx
[Azure-portálon]: https://portal.azure.com/
[a klasszikus Azure portálon]: https://manage.windowsazure.com/
[EnableQueryAttribute]: https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx
[Guid.NewGuid]: https://msdn.microsoft.com/en-us/library/system.guid.newguid(v=vs.110).aspx
[ISupportIncrementalLoading]: http://msdn.microsoft.com/library/windows/apps/Hh701916.aspx
[Windows fejlesztői központ]: https://dev.windows.com/en-us/overview
[DelegatingHandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx
[Windows Live SDK]: https://msdn.microsoft.com/en-us/library/bb404787.aspx
[PasswordVault]: http://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx
[ProtectedData]: http://msdn.microsoft.com/library/system.security.cryptography.protecteddata%28VS.95%29.aspx
[Notification Hubs API-k]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[Mobile Apps fájlok minta]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files
[LoggingHandler]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files/blob/master/src/client/MobileAppsFilesSample/Helpers/LoggingHandler.cs#L63

<!-- External URLs -->
[OData v3 dokumentáció]: http://www.odata.org/documentation/odata-version-3-0/
[Fiddler]: http://www.telerik.com/fiddler
[Json.NET]: http://www.newtonsoft.com/json
[Xamarin.Auth]: https://components.xamarin.com/view/xamarin.auth/
[AuthStore.cs]: https://github.com/azure-appservice-samples/ContosoMoments
[ContosoMoments photo sharing sample]: https://github.com/azure-appservice-samples/ContosoMoments
