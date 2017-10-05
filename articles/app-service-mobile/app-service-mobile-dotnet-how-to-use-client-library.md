---
title: "Az App Service Mobile Apps felügyelt ügyféloldali kódtár használata (Windows |} Microsoft Docs"
description: "Útmutató a .NET-ügyfél használata az Azure App Service Mobile Apps Windows és a Xamarin-alkalmazásokat."
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
ms.openlocfilehash: 5f4cc3e97ba7adde2aaac471951a3130d79910f6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-use-the-managed-client-for-azure-mobile-apps"></a><span data-ttu-id="05915-103">A felügyelt ügyfelek használata az Azure Mobile Apps-alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="05915-103">How to use the managed client for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

## <a name="overview"></a><span data-ttu-id="05915-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="05915-104">Overview</span></span>
<span data-ttu-id="05915-105">Ez az útmutató bemutatja, hogyan hajthat végre a felügyelt ügyféloldali kódtár használata Azure App Service Mobile Apps for Windows és a Xamarin-alkalmazásokba gyakori forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="05915-105">This guide shows you how to perform common scenarios using the managed client library for Azure App Service Mobile Apps for Windows and Xamarin apps.</span></span> <span data-ttu-id="05915-106">Ha most ismerkedik a Mobile Apps, fontolja meg először befejezése a [Azure Mobile Apps gyors üzembe helyezés] [ 1] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="05915-106">If you are new to Mobile Apps, you should consider first completing the [Azure Mobile Apps quickstart][1] tutorial.</span></span> <span data-ttu-id="05915-107">Ez az útmutató azt figyelmet az ügyféloldali felügyelt SDK-val.</span><span class="sxs-lookup"><span data-stu-id="05915-107">In this guide, we focus on the client-side managed SDK.</span></span> <span data-ttu-id="05915-108">További információt a kiszolgálóoldali SDK-k a Mobile Apps, dokumentációjában a [.NET SDK-kiszolgáló] [ 2] vagy a [Node.js Server SDK][3].</span><span class="sxs-lookup"><span data-stu-id="05915-108">To learn more about the server-side SDKs for Mobile Apps, see the documentation for the [.NET Server SDK][2] or the [Node.js Server SDK][3].</span></span>

## <a name="reference-documentation"></a><span data-ttu-id="05915-109">Segédanyagok</span><span class="sxs-lookup"><span data-stu-id="05915-109">Reference documentation</span></span>
<span data-ttu-id="05915-110">Az ügyfél SDK dokumentációját a következő helyen található: [Azure Mobile Apps .NET ügyfél hivatkozási][4].</span><span class="sxs-lookup"><span data-stu-id="05915-110">The reference documentation for the client SDK is located here: [Azure Mobile Apps .NET client reference][4].</span></span>
<span data-ttu-id="05915-111">Több ügyfél minták is megkeresheti a [Azure-minták GitHub-tárházban][5].</span><span class="sxs-lookup"><span data-stu-id="05915-111">You can also find several client samples in the [Azure-Samples GitHub repository][5].</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="05915-112">A támogatott platformok</span><span class="sxs-lookup"><span data-stu-id="05915-112">Supported Platforms</span></span>
<span data-ttu-id="05915-113">A .NET platformon a következő platformokat támogatja:</span><span class="sxs-lookup"><span data-stu-id="05915-113">The .NET Platform supports the following platforms:</span></span>

* <span data-ttu-id="05915-114">Xamarin Android API 19 kiadását – 24 (KitKat nugát keresztül)</span><span class="sxs-lookup"><span data-stu-id="05915-114">Xamarin Android releases for API 19 through 24 (KitKat through Nougat)</span></span>
* <span data-ttu-id="05915-115">Xamarin iOS kiadását iOS 8.0-s és újabb verziók</span><span class="sxs-lookup"><span data-stu-id="05915-115">Xamarin iOS releases for iOS versions 8.0 and later</span></span>
* <span data-ttu-id="05915-116">Az univerzális Windows Platform</span><span class="sxs-lookup"><span data-stu-id="05915-116">Universal Windows Platform</span></span>
* <span data-ttu-id="05915-117">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="05915-117">Windows Phone 8.1</span></span>
* <span data-ttu-id="05915-118">Windows Phone 8.0 kivételével a Silverlight alkalmazások részére</span><span class="sxs-lookup"><span data-stu-id="05915-118">Windows Phone 8.0 except for Silverlight applications</span></span>

<span data-ttu-id="05915-119">A "server-folyamat" hitelesítési bemutatott felhasználói felülete a webes nézet használja.</span><span class="sxs-lookup"><span data-stu-id="05915-119">The "server-flow" authentication uses a WebView for the presented UI.</span></span>  <span data-ttu-id="05915-120">Az eszköz nincs jelen webes nézet felhasználói Felületet, ha más hitelesítési módszert van szükség.</span><span class="sxs-lookup"><span data-stu-id="05915-120">If the device is not able to present a WebView UI, then other methods of authentication are needed.</span></span>  <span data-ttu-id="05915-121">Ez az SDK így nem alkalmas figyelési típusú vagy hasonló módon korlátozott eszközök.</span><span class="sxs-lookup"><span data-stu-id="05915-121">This SDK is thus not suitable for Watch-type or similarly restricted devices.</span></span>

## <span data-ttu-id="05915-122"><a name="setup"></a>A telepítő és Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="05915-122"><a name="setup"></a>Setup and Prerequisites</span></span>
<span data-ttu-id="05915-123">Feltételezzük, hogy már létrehozott és közzétett mobilalkalmazás háttérprojekt, amely legalább egy olyan táblát tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="05915-123">We assume that you have already created and published your Mobile App backend project, which includes at least one table.</span></span>  <span data-ttu-id="05915-124">A kód a jelen témakörben használt, a táblázat neve `TodoItem` és van-e a következő oszlopokat: `Id`, `Text`, és `Complete`.</span><span class="sxs-lookup"><span data-stu-id="05915-124">In the code used in this topic, the table is named `TodoItem` and it has the following columns: `Id`, `Text`, and `Complete`.</span></span> <span data-ttu-id="05915-125">Ez a táblázat ugyanahhoz a táblához jön létre, amikor befejezte a [Azure Mobile Apps gyors üzembe helyezés][1].</span><span class="sxs-lookup"><span data-stu-id="05915-125">This table is the same table created when you complete the [Azure Mobile Apps quickstart][1].</span></span>

<span data-ttu-id="05915-126">A megfelelő típusos ügyféloldali C# típus a következő osztály:</span><span class="sxs-lookup"><span data-stu-id="05915-126">The corresponding typed client-side type in C# is the following class:</span></span>

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

<span data-ttu-id="05915-127">A [JsonPropertyAttribute] [ 6] azonosítására szolgál a *PropertyName* az ügyfél és a tábla mező közötti megfeleltetés.</span><span class="sxs-lookup"><span data-stu-id="05915-127">The [JsonPropertyAttribute][6] is used to define the *PropertyName* mapping between the client field and the table field.</span></span>

<span data-ttu-id="05915-128">Táblázatok létrehozása a Mobile Apps-háttéralkalmazásának további tudnivalókért lásd: a [.NET Server SDK témakörben] [ 7] vagy a [Node.js Server SDK témakörben][8].</span><span class="sxs-lookup"><span data-stu-id="05915-128">To learn how to create tables in your Mobile Apps backend, see the [.NET Server SDK topic][7] or the [Node.js Server SDK topic][8].</span></span> <span data-ttu-id="05915-129">Ha a gyors üzembe helyezés az Azure portálon létrehozott Mobile Apps-háttéralkalmazását, használhatja a **könnyen táblák** beállítást azokban a [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="05915-129">If you created your Mobile App backend in the Azure portal using the QuickStart, you can also use the **Easy tables** setting in the [Azure portal].</span></span>

### <a name="how-to-install-the-managed-client-sdk-package"></a><span data-ttu-id="05915-130">Útmutató: a kezelt ügyfél SDK telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="05915-130">How to: Install the managed client SDK package</span></span>
<span data-ttu-id="05915-131">A felügyelt ügyfél SDK csomag telepítéséhez a Mobile Apps-alkalmazáshoz az alábbi módszerek valamelyikével [NuGet][9]:</span><span class="sxs-lookup"><span data-stu-id="05915-131">Use one of the following methods to install the managed client SDK package for Mobile Apps from [NuGet][9]:</span></span>

* <span data-ttu-id="05915-132">**A Visual Studio** kattintson jobb gombbal a projektre, kattintson a **NuGet-csomagok kezelése**, keresse meg a `Microsoft.Azure.Mobile.Client` csomagot, majd kattintson az **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="05915-132">**Visual Studio** Right-click your project, click **Manage NuGet Packages**, search for the `Microsoft.Azure.Mobile.Client` package, then click **Install**.</span></span>
* <span data-ttu-id="05915-133">**Xamarin Studio** kattintson jobb gombbal a projektre, kattintson a **Hozzáadás** > **NuGet-csomagok hozzáadása**, keresse meg a `Microsoft.Azure.Mobile.Client `csomagot, majd kattintson a **csomag hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="05915-133">**Xamarin Studio** Right-click your project, click **Add** > **Add NuGet Packages**, search for the `Microsoft.Azure.Mobile.Client `package, and then click **Add Package**.</span></span>

<span data-ttu-id="05915-134">A fő tevékenységnél fájlban, ne felejtse el hozzáadni a következő **használatával** utasítást:</span><span class="sxs-lookup"><span data-stu-id="05915-134">In your main activity file, remember to add the following **using** statement:</span></span>

```
using Microsoft.WindowsAzure.MobileServices;
```

### <span data-ttu-id="05915-135"><a name="symbolsource"></a>Útmutató: a Visual Studio hibakeresési szimbólumok használata</span><span class="sxs-lookup"><span data-stu-id="05915-135"><a name="symbolsource"></a>How to: Work with debug symbols in Visual Studio</span></span>
<span data-ttu-id="05915-136">A szimbólumokat a Microsoft.Azure.Mobile névtér érhetők el a [SymbolSource][10].</span><span class="sxs-lookup"><span data-stu-id="05915-136">The symbols for the Microsoft.Azure.Mobile namespace are available on [SymbolSource][10].</span></span>  <span data-ttu-id="05915-137">Tekintse meg a [SymbolSource utasításokat] [ 11] SymbolSource a Visual Studio integrálását.</span><span class="sxs-lookup"><span data-stu-id="05915-137">Refer to the [SymbolSource instructions][11] to integrate SymbolSource with Visual Studio.</span></span>

## <span data-ttu-id="05915-138"><a name="create-client"></a>A Mobile Apps-ügyfél létrehozása</span><span class="sxs-lookup"><span data-stu-id="05915-138"><a name="create-client"></a>Create the Mobile Apps client</span></span>
<span data-ttu-id="05915-139">A következő kódot hoz létre a [MobileServiceClient] [ 12] objektum, amely használható a Mobile Apps-háttéralkalmazás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="05915-139">The following code creates the [MobileServiceClient][12] object that is used to access your Mobile App backend.</span></span>

```
var client = new MobileServiceClient("MOBILE_APP_URL");
```

<span data-ttu-id="05915-140">Cserélje le az előzőekben látható kód `MOBILE_APP_URL` a Mobile Apps-háttéralkalmazás URL-címét, amely megtalálható a Mobile Apps-háttéralkalmazását panel a [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="05915-140">In the preceding code, replace `MOBILE_APP_URL` with the URL of the Mobile App backend, which is found in the blade for your Mobile App backend in the [Azure portal].</span></span> <span data-ttu-id="05915-141">A MobileServiceClient objektum egypéldányos kell lennie.</span><span class="sxs-lookup"><span data-stu-id="05915-141">The MobileServiceClient object should be a singleton.</span></span>

## <a name="work-with-tables"></a><span data-ttu-id="05915-142">A táblázatok használata</span><span class="sxs-lookup"><span data-stu-id="05915-142">Work with Tables</span></span>
<span data-ttu-id="05915-143">Az alábbi szakasz részletesen keresése és lehívása és módosíthatja az adatokat a táblán belül.</span><span class="sxs-lookup"><span data-stu-id="05915-143">The following section details how to search and retrieve records and modify the data within the table.</span></span>  <span data-ttu-id="05915-144">A következő témaköröket:</span><span class="sxs-lookup"><span data-stu-id="05915-144">The following topics are covered:</span></span>

* [<span data-ttu-id="05915-145">Egy tábla hivatkozás létrehozása</span><span class="sxs-lookup"><span data-stu-id="05915-145">Create a table reference</span></span>](#instantiating)
* [<span data-ttu-id="05915-146">Adatait kérdezi le.</span><span class="sxs-lookup"><span data-stu-id="05915-146">Query data</span></span>](#querying)
* [<span data-ttu-id="05915-147">Visszaadott adatok szűrése</span><span class="sxs-lookup"><span data-stu-id="05915-147">Filter returned data</span></span>](#filtering)
* [<span data-ttu-id="05915-148">A rendezési adatokat adott vissza</span><span class="sxs-lookup"><span data-stu-id="05915-148">Sort returned data</span></span>](#sorting)
* [<span data-ttu-id="05915-149">A lapok visszatérési adatai</span><span class="sxs-lookup"><span data-stu-id="05915-149">Return data in pages</span></span>](#paging)
* [<span data-ttu-id="05915-150">Egyes oszlopok kiválasztásához</span><span class="sxs-lookup"><span data-stu-id="05915-150">Select specific columns</span></span>](#selecting)
* [<span data-ttu-id="05915-151">Kereshet meg egy olyan rekordot-azonosító szerint</span><span class="sxs-lookup"><span data-stu-id="05915-151">Look up a record by Id</span></span>](#lookingup)
* [<span data-ttu-id="05915-152">A típus nélküli lekérdezések kezelése</span><span class="sxs-lookup"><span data-stu-id="05915-152">Dealing with untyped queries</span></span>](#untypedqueries)
* [<span data-ttu-id="05915-153">Adatok beszúrása</span><span class="sxs-lookup"><span data-stu-id="05915-153">Inserting data</span></span>](#inserting)
* [<span data-ttu-id="05915-154">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="05915-154">Updating data</span></span>](#updating)
* [<span data-ttu-id="05915-155">Adatok törlése</span><span class="sxs-lookup"><span data-stu-id="05915-155">Deleting data</span></span>](#deleting)
* [<span data-ttu-id="05915-156">Ütközések feloldása, és az egyidejű hozzáférések optimista</span><span class="sxs-lookup"><span data-stu-id="05915-156">Conflict Resolution and Optimistic Concurrency</span></span>](#optimisticconcurrency)
* [<span data-ttu-id="05915-157">Egy Windows felhasználói felület kötése</span><span class="sxs-lookup"><span data-stu-id="05915-157">Binding to a Windows User Interface</span></span>](#binding)
* [<span data-ttu-id="05915-158">Az oldalméret módosítása</span><span class="sxs-lookup"><span data-stu-id="05915-158">Changing the Page Size</span></span>](#pagesize)

### <span data-ttu-id="05915-159"><a name="instantiating"></a>Hogyan: tábla hivatkozás létrehozása</span><span class="sxs-lookup"><span data-stu-id="05915-159"><a name="instantiating"></a>How to: Create a table reference</span></span>
<span data-ttu-id="05915-160">A kódot, amely hozzáfér, vagy módosítja a háttérrendszer táblákban tárolt adatokat meghívja a funkciók a `MobileServiceTable` objektum.</span><span class="sxs-lookup"><span data-stu-id="05915-160">All the code that accesses or modifies data in a backend table calls functions on the `MobileServiceTable` object.</span></span> <span data-ttu-id="05915-161">A tábla mutató hivatkozás beszerzése meghívásával a [GetTable] módszert az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="05915-161">Obtain a reference to the table by calling the [GetTable] method, as follows:</span></span>

```
IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
```

<span data-ttu-id="05915-162">A visszaadott objektumot a típusos szerializálási modellt használ.</span><span class="sxs-lookup"><span data-stu-id="05915-162">The returned object uses the typed serialization model.</span></span> <span data-ttu-id="05915-163">Az a típus nélküli szerializálás modell is támogatott.</span><span class="sxs-lookup"><span data-stu-id="05915-163">An untyped serialization model is also supported.</span></span> <span data-ttu-id="05915-164">Az alábbi példa [hoz létre egy típusos táblára mutató hivatkozás]:</span><span class="sxs-lookup"><span data-stu-id="05915-164">The following example [creates a reference to an untyped table]:</span></span>

```
// Get an untyped table reference
IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");
```

<span data-ttu-id="05915-165">A típus nélküli lekérdezések meg kell adnia az alapul szolgáló OData-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="05915-165">In untyped queries, you must specify the underlying OData query string.</span></span>

### <span data-ttu-id="05915-166"><a name="querying"></a>Útmutató: a mobilalkalmazás adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="05915-166"><a name="querying"></a>How to: Query data from your Mobile App</span></span>
<span data-ttu-id="05915-167">Ez a szakasz ismerteti, hogyan lekérdezések kiadni a Mobile Apps-háttéralkalmazás, amely a következő funkciókat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="05915-167">This section describes how to issue queries to the Mobile App backend, which includes the following functionality:</span></span>

* [<span data-ttu-id="05915-168">Visszaadott adatok szűrése</span><span class="sxs-lookup"><span data-stu-id="05915-168">Filter returned data</span></span>](#filtering)
* [<span data-ttu-id="05915-169">A rendezési adatokat adott vissza</span><span class="sxs-lookup"><span data-stu-id="05915-169">Sort returned data</span></span>](#sorting)
* [<span data-ttu-id="05915-170">A lapok visszatérési adatai</span><span class="sxs-lookup"><span data-stu-id="05915-170">Return data in pages</span></span>](#paging)
* [<span data-ttu-id="05915-171">Egyes oszlopok kiválasztásához</span><span class="sxs-lookup"><span data-stu-id="05915-171">Select specific columns</span></span>](#selecting)
* [<span data-ttu-id="05915-172">Adatok-azonosító szerint</span><span class="sxs-lookup"><span data-stu-id="05915-172">Look up data by ID</span></span>](#lookingup)

> [!NOTE]
> <span data-ttu-id="05915-173">Egy kiszolgáló adatvezérelt oldalméret ad vissza az összes sort megakadályozása van kényszerítve.</span><span class="sxs-lookup"><span data-stu-id="05915-173">A server-driven page size is enforced to prevent all rows from being returned.</span></span>  <span data-ttu-id="05915-174">Lapozófájl alapértelmezett kérelmek nagy adatkészletek tarthatja negatív hatással a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="05915-174">Paging keeps default requests for large data sets from negatively impacting the service.</span></span>  <span data-ttu-id="05915-175">Több mint 50 sorok visszaállításához használja a `Skip` és `Take` metódus, a [vissza adatokat a lapok](#paging).</span><span class="sxs-lookup"><span data-stu-id="05915-175">To return more than 50 rows, use the `Skip` and `Take` method, as described in [Return data in pages](#paging).</span></span>

### <span data-ttu-id="05915-176"><a name="filtering"></a>Hogyan: szűrő adatokat adott vissza.</span><span class="sxs-lookup"><span data-stu-id="05915-176"><a name="filtering"></a>How to: Filter returned data</span></span>
<span data-ttu-id="05915-177">Az alábbi kód bemutatja az adatok szűrése, beleértve a következőket a `Where` alzáradékában.</span><span class="sxs-lookup"><span data-stu-id="05915-177">The following code illustrates how to filter data by including a `Where` clause in a query.</span></span> <span data-ttu-id="05915-178">Az összes elemet adja vissza `todoTable` amelynek `Complete` tulajdonság értéke `false`.</span><span class="sxs-lookup"><span data-stu-id="05915-178">It returns all items from `todoTable` whose `Complete` property is equal to `false`.</span></span> <span data-ttu-id="05915-179">A [ahol] függvény egy sort a lekérdezéshez a táblázaton predikátum szűrés vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="05915-179">The [Where] function applies a row filtering predicate to the query against the table.</span></span>

```
// This query filters out completed TodoItems and items without a timestamp.
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .ToListAsync();
```

<span data-ttu-id="05915-180">Megtekintheti a küldött üzenet hálózatfelügyeleti szoftverek, például a böngésző fejlesztői eszközök segítségével a háttér-kérelem URI vagy [Fiddler].</span><span class="sxs-lookup"><span data-stu-id="05915-180">You can view the URI of the request sent to the backend by using message inspection software, such as browser developer tools or [Fiddler].</span></span> <span data-ttu-id="05915-181">Ha a kérelem URI-azonosítója tekinti meg, figyelje meg, hogy van-e módosítva a lekérdezési karakterlánc:</span><span class="sxs-lookup"><span data-stu-id="05915-181">If you look at the request URI, notice that the query string is modified:</span></span>

```
GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1
```

<span data-ttu-id="05915-182">Az OData-kérelem a kiszolgáló SDK lefordítását egy SQL-lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="05915-182">This OData request is translated into an SQL query by the Server SDK:</span></span>

```
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
```

<span data-ttu-id="05915-183">A függvénynek átadott a `Where` metódus egy tetszőleges számú feltételek lehet.</span><span class="sxs-lookup"><span data-stu-id="05915-183">The function that is passed to the `Where` method can have an arbitrary number of conditions.</span></span>

```
// This query filters out completed TodoItems where Text isn't null
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false && todoItem.Text != null)
    .ToListAsync();
```

<span data-ttu-id="05915-184">Ebben a példában volna fordítható egy SQL-lekérdezést, a kiszolgáló SDK-ban:</span><span class="sxs-lookup"><span data-stu-id="05915-184">This example would be translated into an SQL query by the Server SDK:</span></span>

```
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0
```

<span data-ttu-id="05915-185">Ez a lekérdezés több záradékot is osztható:</span><span class="sxs-lookup"><span data-stu-id="05915-185">This query can also be split into multiple clauses:</span></span>

```
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .Where(todoItem => todoItem.Text != null)
    .ToListAsync();
```

<span data-ttu-id="05915-186">A két módszer egyenértékű, és szabadon felcserélhetők.</span><span class="sxs-lookup"><span data-stu-id="05915-186">The two methods are equivalent and may be used interchangeably.</span></span>  <span data-ttu-id="05915-187">A korábbi beállítás&mdash;hozzáfűzésével több predikátumok egy lekérdezést a&mdash;tömörebb és ajánlott.</span><span class="sxs-lookup"><span data-stu-id="05915-187">The former option&mdash;of concatenating multiple predicates in one query&mdash;is more compact and recommended.</span></span>

<span data-ttu-id="05915-188">A `Where` záradék támogatja a műveleteket, lehet, az OData-részhalmazt lefordítva.</span><span class="sxs-lookup"><span data-stu-id="05915-188">The `Where` clause supports operations that be translated into the OData subset.</span></span> <span data-ttu-id="05915-189">Műveletek a következők:</span><span class="sxs-lookup"><span data-stu-id="05915-189">Operations include:</span></span>

* <span data-ttu-id="05915-190">Relációs operátorok (==,! =, <, < =, >, > =),</span><span class="sxs-lookup"><span data-stu-id="05915-190">Relational operators (==, !=, <, <=, >, >=),</span></span>
* <span data-ttu-id="05915-191">Aritmetikai operátor (+, -, /, *, %),</span><span class="sxs-lookup"><span data-stu-id="05915-191">Arithmetic operators (+, -, /, *, %),</span></span>
* <span data-ttu-id="05915-192">A pontosság (Math.Floor, Math.Ceiling), szám</span><span class="sxs-lookup"><span data-stu-id="05915-192">Number precision (Math.Floor, Math.Ceiling),</span></span>
* <span data-ttu-id="05915-193">Karakterlánc (hossza, Substring, Replace, IndexOf, megadott módon kezdődő, megadott módon végződő),</span><span class="sxs-lookup"><span data-stu-id="05915-193">String functions (Length, Substring, Replace, IndexOf, StartsWith, EndsWith),</span></span>
* <span data-ttu-id="05915-194">Tulajdonságok (év, hónap, nap, óra, perc másodperc), dátum</span><span class="sxs-lookup"><span data-stu-id="05915-194">Date properties (Year, Month, Day, Hour, Minute, Second),</span></span>
* <span data-ttu-id="05915-195">Az objektum tulajdonságainak hozzáférést és</span><span class="sxs-lookup"><span data-stu-id="05915-195">Access properties of an object, and</span></span>
* <span data-ttu-id="05915-196">A kifejezések kombinálásával egyik műveletet.</span><span class="sxs-lookup"><span data-stu-id="05915-196">Expressions combining any of these operations.</span></span>

<span data-ttu-id="05915-197">Annak eldöntéséhez, hogy a kiszolgáló SDK támogatja, akkor is fontolóra veheti a [OData v3 dokumentáció].</span><span class="sxs-lookup"><span data-stu-id="05915-197">When considering what the Server SDK supports, you can consider the [OData v3 Documentation].</span></span>

### <span data-ttu-id="05915-198"><a name="sorting"></a>Hogyan: rendezési adatokat adott vissza.</span><span class="sxs-lookup"><span data-stu-id="05915-198"><a name="sorting"></a>How to: Sort returned data</span></span>
<span data-ttu-id="05915-199">Az alábbi kód bemutatja, hogyan lehet rendezni az adatok-ot egy [OrderBy] vagy [OrderByDescending] a lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="05915-199">The following code illustrates how to sort data by including an [OrderBy] or [OrderByDescending] function in the query.</span></span> <span data-ttu-id="05915-200">Adja vissza, a cikkek `todoTable` növekvő sorrend által a `Text` mező.</span><span class="sxs-lookup"><span data-stu-id="05915-200">It returns items from `todoTable` sorted ascending by the `Text` field.</span></span>

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

### <span data-ttu-id="05915-201"><a name="paging"></a>Hogyan: vissza adatokat lap</span><span class="sxs-lookup"><span data-stu-id="05915-201"><a name="paging"></a>How to: Return data in pages</span></span>
<span data-ttu-id="05915-202">Alapértelmezés szerint a háttér csak az első 50 sort adja vissza.</span><span class="sxs-lookup"><span data-stu-id="05915-202">By default, the backend returns only the first 50 rows.</span></span> <span data-ttu-id="05915-203">Visszaadott sorok száma meghívásával növelheti a [érvénybe] metódust.</span><span class="sxs-lookup"><span data-stu-id="05915-203">You can increase the number of returned rows by calling the [Take] method.</span></span> <span data-ttu-id="05915-204">Használjon `Take` együtt a [kihagyása] módszer egy adott "lap" a teljes adatkészlet a lekérdezés által visszaadott kéréséhez.</span><span class="sxs-lookup"><span data-stu-id="05915-204">Use `Take` along with the [Skip] method to request a specific "page" of the total dataset returned by the query.</span></span> <span data-ttu-id="05915-205">A következő lekérdezés végrehajtásakor, a három legfontosabb cikkeket a táblát adja vissza.</span><span class="sxs-lookup"><span data-stu-id="05915-205">The following query, when executed, returns the top three items in the table.</span></span>

```
// Define a filtered query that returns the top 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Take(3);
List<TodoItem> items = await query.ToListAsync();
```

<span data-ttu-id="05915-206">Az alábbi javított lekérdezés kihagyja az első három eredményeket, és a következő három eredményeket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="05915-206">The following revised query skips the first three results and returns the next three results.</span></span> <span data-ttu-id="05915-207">Ez a lekérdezés második "lapján" adatok, ahol az oldalméret három elemet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="05915-207">This query produces the second "page" of data, where the page size is three items.</span></span>

```
// Define a filtered query that skips the top 3 items and returns the next 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Skip(3).Take(3);
List<TodoItem> items = await query.ToListAsync();
```

<span data-ttu-id="05915-208">A [IncludeTotalCount] metódus kérelmek teljes számát *összes* az azt jelzi, hogy akkor adott vissza, figyelmen kívül hagyja a lapozófájl/vonatkozó záradékban megadott:</span><span class="sxs-lookup"><span data-stu-id="05915-208">The [IncludeTotalCount] method requests the total count for *all* the records that would have been returned, ignoring any paging/limit clause specified:</span></span>

```
query = query.IncludeTotalCount();
```

<span data-ttu-id="05915-209">Egy valós alkalmazás használatával az előző példához hasonló lekérdezéseket a személyhívó vezérlőre vagy hasonló felhasználói felület oldalai között.</span><span class="sxs-lookup"><span data-stu-id="05915-209">In a real world app, you can use queries similar to the preceding example with a pager control or comparable UI to navigate between pages.</span></span>

> [!NOTE]
> <span data-ttu-id="05915-210">A Mobile Apps-háttéralkalmazás-50-sor korlát felülbírálásához is telepítenie kell a [EnableQueryAttribute] nyilvános GET metódus, és adja meg a lapozófájl viselkedését.</span><span class="sxs-lookup"><span data-stu-id="05915-210">To override the 50-row limit in a Mobile App backend, you must also apply the [EnableQueryAttribute] to the public GET method and specify the paging behavior.</span></span> <span data-ttu-id="05915-211">A metódus alkalmazásakor a következő állítja a maximálisan engedélyezett 1000 sort adott vissza:</span><span class="sxs-lookup"><span data-stu-id="05915-211">When applied to the method, the following sets the maximum returned rows to 1000:</span></span>
>
> `[EnableQuery(MaxTop=1000)]`


### <span data-ttu-id="05915-212"><a name="selecting"></a>Hogyan: egyes oszlopok kiválasztásához</span><span class="sxs-lookup"><span data-stu-id="05915-212"><a name="selecting"></a>How to: Select specific columns</span></span>
<span data-ttu-id="05915-213">Megadhatja, amely tulajdonságok beállítása hozzáadásával az eredmények felvenni egy [kiválasztása] záradékot a lekérdezéshez.</span><span class="sxs-lookup"><span data-stu-id="05915-213">You can specify which set of properties to include in the results by adding a [Select] clause to your query.</span></span> <span data-ttu-id="05915-214">Például a következő kód bemutatja, csak egy mező kiválasztása, és válassza ki, és több mező formázása:</span><span class="sxs-lookup"><span data-stu-id="05915-214">For example, the following code shows how to select just one field and also how to select and format multiple fields:</span></span>

```
// Select one field -- just the Text
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

<span data-ttu-id="05915-215">Amennyiben leírt minden funkció additívak, így azt is tartsa láncolás őket.</span><span class="sxs-lookup"><span data-stu-id="05915-215">All the functions described so far are additive, so we can keep chaining them.</span></span> <span data-ttu-id="05915-216">Minden láncolt hívás több lekérdezés van hatással.</span><span class="sxs-lookup"><span data-stu-id="05915-216">Each chained call affects more of the query.</span></span> <span data-ttu-id="05915-217">Egy további példa:</span><span class="sxs-lookup"><span data-stu-id="05915-217">One more example:</span></span>

```
MobileServiceTableQuery<TodoItem> query = todoTable
                .Where(todoItem => todoItem.Complete == false)
                .Select(todoItem => todoItem.Text)
                .Skip(3).
                .Take(3);
List<string> items = await query.ToListAsync();
```

### <span data-ttu-id="05915-218"><a name="lookingup"></a>Útmutató: adatok-azonosító szerint</span><span class="sxs-lookup"><span data-stu-id="05915-218"><a name="lookingup"></a>How to: Look up data by ID</span></span>
<span data-ttu-id="05915-219">A [LookupAsync] függvény használható objektumokat kereshet meg egy adott. az adatbázisból</span><span class="sxs-lookup"><span data-stu-id="05915-219">The [LookupAsync] function can be used to look up objects from the database with a particular ID.</span></span>

```
// This query filters out the item with the ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");
```

### <span data-ttu-id="05915-220"><a name="untypedqueries"></a>Útmutató: a típus nélküli lekérdezések végrehajtása</span><span class="sxs-lookup"><span data-stu-id="05915-220"><a name="untypedqueries"></a>How to: Execute untyped queries</span></span>
<span data-ttu-id="05915-221">A típus nélküli tábla objektum lekérdezést végrehajtásakor pontosan meg kell adni az OData-lekérdezési karakterlánc meghívásával [ReadAsync], az alábbi példa:</span><span class="sxs-lookup"><span data-stu-id="05915-221">When executing a query using an untyped table object, you must explicitly specify the OData query string by calling [ReadAsync], as in the following example:</span></span>

```
// Lookup untyped data using OData
JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");
```

<span data-ttu-id="05915-222">JSON értékekre, amelyek például a tulajdonságcsomag vissza.</span><span class="sxs-lookup"><span data-stu-id="05915-222">You get back JSON values that you can use like a property bag.</span></span> <span data-ttu-id="05915-223">JToken és Newtonsoft Json.NET további információkért lásd: a [Json.NET] hely.</span><span class="sxs-lookup"><span data-stu-id="05915-223">For more information on JToken and Newtonsoft Json.NET, see the [Json.NET] site.</span></span>

### <span data-ttu-id="05915-224"><a name="inserting"></a>Útmutató: a Mobile Apps-háttéralkalmazás adatok beszúrása</span><span class="sxs-lookup"><span data-stu-id="05915-224"><a name="inserting"></a>How to: Insert data into a Mobile App backend</span></span>
<span data-ttu-id="05915-225">Minden ügyfél esetében tartalmaznia kell egy nevű tag **azonosító**, amely alapértelmezés szerint ki egy karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="05915-225">All client types must contain a member named **Id**, which is by default a string.</span></span> <span data-ttu-id="05915-226">Ez **azonosító** CRUD műveletek elvégzéséhez szükséges és a kapcsolat nélküli szinkronizálás.</span><span class="sxs-lookup"><span data-stu-id="05915-226">This **Id** is required to perform CRUD operations and for offline sync.</span></span> <span data-ttu-id="05915-227">Az alábbi kód bemutatja, hogyan használható a [InsertAsync] módszer új sorok beillesztéséhez egy tábla.</span><span class="sxs-lookup"><span data-stu-id="05915-227">The following code illustrates how to use the [InsertAsync] method to insert new rows into a table.</span></span> <span data-ttu-id="05915-228">A paraméter tartalmazza az adatokat beszúrni .NET objektumként.</span><span class="sxs-lookup"><span data-stu-id="05915-228">The parameter contains the data to be inserted as a .NET object.</span></span>

```
await todoTable.InsertAsync(todoItem);
```

<span data-ttu-id="05915-229">Ha egyéni azonosító egyedi érték nem szerepel a `todoItem` során egy INSERT utasítás, a kiszolgáló által létrehozott GUID.</span><span class="sxs-lookup"><span data-stu-id="05915-229">If a unique custom ID value is not included in the `todoItem` during an insert, a GUID is generated by the server.</span></span>
<span data-ttu-id="05915-230">A generált kódot adott vissza a hívása után az objektum vizsgálatával kérheti le.</span><span class="sxs-lookup"><span data-stu-id="05915-230">You can retrieve the generated Id by inspecting the object after the call returns.</span></span>

<span data-ttu-id="05915-231">Típusos adatok beszúrásához eltarthat Json.NET előnye:</span><span class="sxs-lookup"><span data-stu-id="05915-231">To insert untyped data, you may take advantage of Json.NET:</span></span>

```
JObject jo = new JObject();
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

<span data-ttu-id="05915-232">Íme egy példa egy e-mail cím használata egy egyedi karakterlánc-azonosító:</span><span class="sxs-lookup"><span data-stu-id="05915-232">Here is an example using an email address as a unique string id:</span></span>

```
JObject jo = new JObject();
jo.Add("id", "myemail@emaildomain.com");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

### <a name="working-with-id-values"></a><span data-ttu-id="05915-233">Azonosító értékek használata</span><span class="sxs-lookup"><span data-stu-id="05915-233">Working with ID values</span></span>
<span data-ttu-id="05915-234">Mobile Apps támogatja egyedi egyéni karakterlánc-értékek a táblázat **azonosító** oszlop.</span><span class="sxs-lookup"><span data-stu-id="05915-234">Mobile Apps supports unique custom string values for the table's **id** column.</span></span> <span data-ttu-id="05915-235">Egy karakterláncértéket lehetővé teszi, hogy az alkalmazások az egyéni értékek, például az e-mail címek vagy felhasználónevek a azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="05915-235">A string value allows applications to use custom values such as email addresses or user names for the ID.</span></span>  <span data-ttu-id="05915-236">Karakterlánc-azonosítóknak a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="05915-236">String IDs provide you with the following benefits:</span></span>

* <span data-ttu-id="05915-237">Azonosítók akkor jönnek létre, de oda-vissza az adatbázist.</span><span class="sxs-lookup"><span data-stu-id="05915-237">IDs are generated without making a round trip to the database.</span></span>
* <span data-ttu-id="05915-238">Rekordok olyan könnyebben különböző táblákhoz vagy adatbázisok egyesíteni.</span><span class="sxs-lookup"><span data-stu-id="05915-238">Records are easier to merge from different tables or databases.</span></span>
* <span data-ttu-id="05915-239">Azonosítók értékek jobban integrálható egy alkalmazás logikáját.</span><span class="sxs-lookup"><span data-stu-id="05915-239">IDs values can integrate better with an application's logic.</span></span>

<span data-ttu-id="05915-240">Ha a karakterlánc-azonosító értéke nincs beállítva egy beszúrt rekordot, a Mobile Apps-háttéralkalmazás állít elő, egyedi értéket a azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="05915-240">When a string ID value is not set on an inserted record, the Mobile App backend generates a unique value for the ID.</span></span> <span data-ttu-id="05915-241">Használhatja a [Guid.NewGuid] metódus saját azonosító értékeket, az ügyfélen vagy a háttérben létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="05915-241">You can use the [Guid.NewGuid] method to generate your own ID values, either on the client or in the backend.</span></span>

```
JObject jo = new JObject();
jo.Add("id", Guid.NewGuid().ToString("N"));
```

### <span data-ttu-id="05915-242"><a name="modifying"></a>Útmutató: a Mobile Apps-háttéralkalmazás adatainak módosítása</span><span class="sxs-lookup"><span data-stu-id="05915-242"><a name="modifying"></a>How to: Modify data in a Mobile App backend</span></span>
<span data-ttu-id="05915-243">Az alábbi kód bemutatja, hogyan használható a [UpdateAsync] metódust ugyanazzal az azonosítóval rendelkező új információ egy meglévő rekordjának frissítése érdekében.</span><span class="sxs-lookup"><span data-stu-id="05915-243">The following code illustrates how to use the [UpdateAsync] method to update an existing record with the same ID with new information.</span></span> <span data-ttu-id="05915-244">A paraméter tartalmazza az adatokat, frissítenie kell a .NET objektumként.</span><span class="sxs-lookup"><span data-stu-id="05915-244">The parameter contains the data to be updated as a .NET object.</span></span>

```
await todoTable.UpdateAsync(todoItem);
```

<span data-ttu-id="05915-245">Típusos adatok frissítéséhez is igénybe vehet előnyeit [Json.NET] az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="05915-245">To update untyped data, you may take advantage of [Json.NET] as follows:</span></span>

```
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.UpdateAsync(jo);
```

<span data-ttu-id="05915-246">Egy `id` mezőt meg kell adni, ha egy frissítés.</span><span class="sxs-lookup"><span data-stu-id="05915-246">An `id` field must be specified when making an update.</span></span> <span data-ttu-id="05915-247">A háttéralkalmazás használja a `id` mező frissítése sor azonosítására.</span><span class="sxs-lookup"><span data-stu-id="05915-247">The backend uses the `id` field to identify which row to update.</span></span> <span data-ttu-id="05915-248">A `id` mező eredménye lehet lekérni a `InsertAsync` hívható meg.</span><span class="sxs-lookup"><span data-stu-id="05915-248">The `id` field can be obtained from the result of the `InsertAsync` call.</span></span> <span data-ttu-id="05915-249">Egy `ArgumentException` jelenik meg, ha egy elem frissítése megadása nélkül próbál a `id` érték.</span><span class="sxs-lookup"><span data-stu-id="05915-249">An `ArgumentException` is raised if you try to update an item without providing the `id` value.</span></span>

### <span data-ttu-id="05915-250"><a name="deleting"></a>Útmutató: a Mobile Apps-háttéralkalmazás adatok törlése</span><span class="sxs-lookup"><span data-stu-id="05915-250"><a name="deleting"></a>How to: Delete data in a Mobile App backend</span></span>
<span data-ttu-id="05915-251">Az alábbi kód bemutatja, hogyan használható a [DeleteAsync] metódus törlése egy meglévő példányát.</span><span class="sxs-lookup"><span data-stu-id="05915-251">The following code illustrates how to use the [DeleteAsync] method to delete an existing instance.</span></span> <span data-ttu-id="05915-252">A példány azonosíthatók a `id` set mezője az `todoItem`.</span><span class="sxs-lookup"><span data-stu-id="05915-252">The instance is identified by the `id` field set on the `todoItem`.</span></span>

```
await todoTable.DeleteAsync(todoItem);
```

<span data-ttu-id="05915-253">Típusos adatok törlése, akkor előfordulhat, hogy előnyeit Json.NET az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="05915-253">To delete untyped data, you may take advantage of Json.NET as follows:</span></span>

```
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
await table.DeleteAsync(jo);
```

<span data-ttu-id="05915-254">Ha elvégezte a törlési kérelmet, meg kell adni egy Azonosítót.</span><span class="sxs-lookup"><span data-stu-id="05915-254">When you make a delete request, an ID must be specified.</span></span> <span data-ttu-id="05915-255">Egyéb tulajdonságok nem továbbítódnak a szolgáltatás, vagy figyelmen kívül lesznek hagyva, a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="05915-255">Other properties are not passed to the service or are ignored at the service.</span></span> <span data-ttu-id="05915-256">Eredménye egy `DeleteAsync` tekintendő, amely általában `null`.</span><span class="sxs-lookup"><span data-stu-id="05915-256">The result of a `DeleteAsync` call is usually `null`.</span></span> <span data-ttu-id="05915-257">Adjon át az Azonosítót az eredménye lehet lekérni a `InsertAsync` hívható meg.</span><span class="sxs-lookup"><span data-stu-id="05915-257">The ID to pass in can be obtained from the result of the `InsertAsync` call.</span></span> <span data-ttu-id="05915-258">A `MobileServiceInvalidOperationException` fordul elő, amikor egy elem törlése megadása nélkül próbál a `id` mező.</span><span class="sxs-lookup"><span data-stu-id="05915-258">A `MobileServiceInvalidOperationException` is thrown when you try to delete an item without specifying the `id` field.</span></span>

### <span data-ttu-id="05915-259"><a name="optimisticconcurrency"></a>Hogyan: használata egyidejű hozzáférések optimista az ütközések feloldása</span><span class="sxs-lookup"><span data-stu-id="05915-259"><a name="optimisticconcurrency"></a>How to: Use Optimistic Concurrency for conflict resolution</span></span>
<span data-ttu-id="05915-260">Két vagy több ügyfelet az egy időben ugyanazt a cikket is írni módosítások.</span><span class="sxs-lookup"><span data-stu-id="05915-260">Two or more clients may write changes to the same item at the same time.</span></span> <span data-ttu-id="05915-261">Nélkül ütközésészlelés az utolsó írás felülírná korábbi frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="05915-261">Without conflict detection, the last write would overwrite any previous updates.</span></span> <span data-ttu-id="05915-262">**Egyidejű hozzáférések optimista vezérlését** feltételezi, hogy az egyes tranzakciókra véglegesítheti, és ezért nem használhatja semmilyen erőforrás zárolását.</span><span class="sxs-lookup"><span data-stu-id="05915-262">**Optimistic concurrency control** assumes that each transaction can commit and therefore does not use any resource locking.</span></span>  <span data-ttu-id="05915-263">Előtt véglegesítése tranzakció, egyidejű hozzáférések optimista vezérlését ellenőrzi, hogy nincs másik tranzakció módosította-e az adatokat.</span><span class="sxs-lookup"><span data-stu-id="05915-263">Before committing a transaction, optimistic concurrency control verifies that no other transaction has modified the data.</span></span> <span data-ttu-id="05915-264">Ha az adatok módosítva lett, a végrehajtása tranzakció vissza lesz állítva.</span><span class="sxs-lookup"><span data-stu-id="05915-264">If the data has been modified, the committing transaction is rolled back.</span></span>

<span data-ttu-id="05915-265">Mobile Apps egyidejű hozzáférések optimista vezérlését támogatja a változások követése minden elem használatát a `version` rendszer tulajdonság oszlop, amely minden táblában a Mobile Apps-háttéralkalmazás van definiálva.</span><span class="sxs-lookup"><span data-stu-id="05915-265">Mobile Apps supports optimistic concurrency control by tracking changes to each item using the `version` system property column that is defined for each table in your Mobile App backend.</span></span> <span data-ttu-id="05915-266">Minden alkalommal, amikor frissíti, a Mobile Apps beállítja a `version` tulajdonság értéke rekord.</span><span class="sxs-lookup"><span data-stu-id="05915-266">Each time a record is updated, Mobile Apps sets the `version` property for that record to a new value.</span></span> <span data-ttu-id="05915-267">Minden egyes frissítés kérelem során a `version` tulajdonság a rekord tartalmazza a kéréshez a rendszer összehasonlítja a rekord a kiszolgáló ugyanahhoz a tulajdonsághoz.</span><span class="sxs-lookup"><span data-stu-id="05915-267">During each update request, the `version` property of the record included with the request is compared to the same property for the record on the server.</span></span> <span data-ttu-id="05915-268">Ha átadni a verziót a kérelem nem egyezik meg a háttérrendszer, majd az ügyféloldali kódtár kivált egy `MobileServicePreconditionFailedException<T>` kivétel.</span><span class="sxs-lookup"><span data-stu-id="05915-268">If the version passed with the request does not match the backend, then the client library raises a `MobileServicePreconditionFailedException<T>` exception.</span></span> <span data-ttu-id="05915-269">A kivétel mellékelt típus a rekord a rekord kiszolgálók verzióját tartalmazó háttérrendszerből.</span><span class="sxs-lookup"><span data-stu-id="05915-269">The type included with the exception is the record from the backend containing the servers version of the record.</span></span> <span data-ttu-id="05915-270">Az alkalmazás ezután használhatja ezt az információt határozza meg, hogy a frissítési kérelem újra a megfelelő `version` a háttérkiszolgálón a változtatások véglegesítése a határidő közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="05915-270">The application can then use this information to decide whether to execute the update request again with the correct `version` value from the backend to commit changes.</span></span>

<span data-ttu-id="05915-271">Adja meg a tábla osztály az oszlopot a `version` rendszer tulajdonság egyidejű hozzáférések optimista engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="05915-271">Define a column on the table class for the `version` system property to enable optimistic concurrency.</span></span> <span data-ttu-id="05915-272">Példa:</span><span class="sxs-lookup"><span data-stu-id="05915-272">For example:</span></span>

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

<span data-ttu-id="05915-273">A típus nélküli táblák használata kiszolgálóalkalmazások lehetővé teszik az egyidejű hozzáférések optimista úgy, hogy a `Version` a jelzőt a `SystemProperties` a tábla az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="05915-273">Applications using untyped tables enable optimistic concurrency by setting the `Version` flag on the `SystemProperties` of the table as follows.</span></span>

```
//Enable optimistic concurrency by retrieving version
todoTable.SystemProperties |= MobileServiceSystemProperties.Version;
```

<span data-ttu-id="05915-274">Egyidejű hozzáférések optimista engedélyezniük, meg kell is dolgozza fel a `MobileServicePreconditionFailedException<T>` a kódban meghívásakor kivétel [UpdateAsync].</span><span class="sxs-lookup"><span data-stu-id="05915-274">In addition to enabling optimistic concurrency, you must also catch the `MobileServicePreconditionFailedException<T>` exception in your code when calling [UpdateAsync].</span></span>  <span data-ttu-id="05915-275">Oldja fel az ütközést úgy, hogy alkalmazza a megfelelő `version` a frissített rekord és a hívás [UpdateAsync] a feloldott bejegyzéshez.</span><span class="sxs-lookup"><span data-stu-id="05915-275">Resolve the conflict by applying the correct `version` to the updated record and call [UpdateAsync] with the resolved record.</span></span> <span data-ttu-id="05915-276">A következő kód bemutatja, hogyan feloldani egy írás ütközés többször észlelte:</span><span class="sxs-lookup"><span data-stu-id="05915-276">The following code shows how to resolve a write conflict once detected:</span></span>

```
private async void UpdateToDoItem(TodoItem item)
{
    MobileServicePreconditionFailedException<TodoItem> exception = null;

    try
    {
        //update at the remote table
        await todoTable.UpdateAsync(item);
    }
    catch (MobileServicePreconditionFailedException<TodoItem> writeException)
    {
        exception = writeException;
    }

    if (exception != null)
    {
        // Conflict detected, the item has changed since the last query
        // Resolve the conflict between the local and server item
        await ResolveConflict(item, exception.Item);
    }
}


private async Task ResolveConflict(TodoItem localItem, TodoItem serverItem)
{
    //Ask user to choose the resoltion between versions
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
        // To resolve the conflict, update the version of the item being committed. Otherwise, you will keep
        // catching a MobileServicePreConditionFailedException.
        localItem.Version = serverItem.Version;

        // Updating recursively here just in case another change happened while the user was making a decision
        UpdateToDoItem(localItem);
    };

    ServerBtn.Invoked = async (IUICommand command) =>
    {
        RefreshTodoItems();
    };

    await msgDialog.ShowAsync();
}
```

<span data-ttu-id="05915-277">További információkért lásd: a [az Azure Mobile Apps Offline adatszinkronizálás] témakör.</span><span class="sxs-lookup"><span data-stu-id="05915-277">For more information, see the [Offline Data Sync in Azure Mobile Apps] topic.</span></span>

### <span data-ttu-id="05915-278"><a name="binding"></a>Útmutató: egy Windows felhasználói felületét Bind Mobile Apps-adatok</span><span class="sxs-lookup"><span data-stu-id="05915-278"><a name="binding"></a>How to: Bind Mobile Apps data to a Windows user interface</span></span>
<span data-ttu-id="05915-279">Ez a szakasz bemutatja, hogyan visszaadott adatok objektumok felhasználói felületi elemei használják a Windows-alkalmazások megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="05915-279">This section shows how to display returned data objects using UI elements in a Windows app.</span></span>  <span data-ttu-id="05915-280">Az alábbi példakód az a lista forrása a hiányos elemeket a lekérdezéssel van kötve.</span><span class="sxs-lookup"><span data-stu-id="05915-280">The following example code binds to the source of the list with a query for incomplete items.</span></span> <span data-ttu-id="05915-281">A [MobileServiceCollection] létrehoz egy Mobile Apps-kompatibilis kötés gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="05915-281">The [MobileServiceCollection] creates a Mobile Apps-aware binding collection.</span></span>

```
// This query filters out completed TodoItems.
MobileServiceCollection<TodoItem, TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .ToCollectionAsync();

// itemsControl is an IEnumerable that could be bound to a UI list control
IEnumerable itemsControl  = items;

// Bind this to a ListBox
ListBox lb = new ListBox();
lb.ItemsSource = items;
```

<span data-ttu-id="05915-282">A felügyelt futásidejű vezérlőelemeket támogat egy felületet nevű [ISupportIncrementalLoading].</span><span class="sxs-lookup"><span data-stu-id="05915-282">Some controls in the managed runtime support an interface called [ISupportIncrementalLoading].</span></span> <span data-ttu-id="05915-283">Ez az interfész lehetővé teszi, hogy a vezérlők további adatokat kér, amikor a felhasználó görget.</span><span class="sxs-lookup"><span data-stu-id="05915-283">This interface allows controls to request extra data when the user scrolls.</span></span> <span data-ttu-id="05915-284">Ez az interfész keresztül univerzális Windows-alkalmazások beépített támogatása [MobileServiceIncrementalLoadingCollection], amely automatikusan kezeli a vezérlők hívást.</span><span class="sxs-lookup"><span data-stu-id="05915-284">There is built-in support for this interface for universal Windows apps via [MobileServiceIncrementalLoadingCollection], which automatically handles the calls from the controls.</span></span> <span data-ttu-id="05915-285">Használjon `MobileServiceIncrementalLoadingCollection` a Windows-alkalmazások az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="05915-285">Use `MobileServiceIncrementalLoadingCollection` in Windows apps as follows:</span></span>

```
MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
items = todoTable.Where(todoItem => todoItem.Complete == false).ToIncrementalLoadingCollection();

ListBox lb = new ListBox();
lb.ItemsSource = items;
```

<span data-ttu-id="05915-286">Az új gyűjtemény használatához a Windows Phone 8 és a "Silverlight" alkalmazásokban a `ToCollection` a kiterjesztésmetódusok `IMobileServiceTableQuery<T>` és `IMobileServiceTable<T>`.</span><span class="sxs-lookup"><span data-stu-id="05915-286">To use the new collection on Windows Phone 8 and "Silverlight" apps, use the `ToCollection` extension methods on `IMobileServiceTableQuery<T>` and `IMobileServiceTable<T>`.</span></span> <span data-ttu-id="05915-287">Adatok betöltése, hívja meg a `LoadMoreItemsAsync()`.</span><span class="sxs-lookup"><span data-stu-id="05915-287">To load data, call `LoadMoreItemsAsync()`.</span></span>

```
MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
await items.LoadMoreItemsAsync();
```

<span data-ttu-id="05915-288">A hívó által létrehozott gyűjtemény használatakor `ToCollectionAsync` vagy `ToCollection`, kap egy gyűjteményt, amely a felhasználói felületi vezérlők köthető.</span><span class="sxs-lookup"><span data-stu-id="05915-288">When you use the collection created by calling `ToCollectionAsync` or `ToCollection`, you get a collection that can be bound to UI controls.</span></span>  <span data-ttu-id="05915-289">Ez a gyűjtemény a lapozófájl-kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="05915-289">This collection is paging-aware.</span></span>  <span data-ttu-id="05915-290">A gyűjtemény adatokat tölt be a hálózaton, mivel néha betöltése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="05915-290">Since the collection is loading data from the network, loading sometimes fails.</span></span> <span data-ttu-id="05915-291">Ilyen hibák kezeléséhez, bírálja felül a `OnException` metódusa `MobileServiceIncrementalLoadingCollection` hívások eredő kivételek kezelésének `LoadMoreItemsAsync`.</span><span class="sxs-lookup"><span data-stu-id="05915-291">To handle such failures, override the `OnException` method on `MobileServiceIncrementalLoadingCollection` to handle exceptions resulting from calls to `LoadMoreItemsAsync`.</span></span>

<span data-ttu-id="05915-292">Vegye figyelembe, ha a tábla sok mezőt tartalmaz, de a vezérlőben megjelenítendő némelyike csak szeretne.</span><span class="sxs-lookup"><span data-stu-id="05915-292">Consider if your table has many fields but you only want to display some of them in your control.</span></span> <span data-ttu-id="05915-293">Segítségével az útmutatót az előző szakaszban leírt "[egyes oszlopok kiválasztásához](#selecting)" a felhasználói felületen megjelenő egyes oszlopok kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="05915-293">You may use the guidance in the preceding section "[Select specific columns](#selecting)" to select specific columns to display in the UI.</span></span>

### <span data-ttu-id="05915-294"><a name="pagesize"></a>Az oldalméret módosítása</span><span class="sxs-lookup"><span data-stu-id="05915-294"><a name="pagesize"></a>Change the Page size</span></span>
<span data-ttu-id="05915-295">Az Azure Mobile Apps egy legfeljebb 50 elemet kérelmenként alapértelmezés szerint adja vissza.</span><span class="sxs-lookup"><span data-stu-id="05915-295">Azure Mobile Apps returns a maximum of 50 items per request by default.</span></span>  <span data-ttu-id="05915-296">Az ügyfél és a kiszolgáló a maximális méretének növelésével módosíthatja a lapozófájl mérete.</span><span class="sxs-lookup"><span data-stu-id="05915-296">You can change the paging size by increasing the maximum page size on both the client and server.</span></span>  <span data-ttu-id="05915-297">A kért méretének növelése érdekében adja meg `PullOptions` használatakor `PullAsync()`:</span><span class="sxs-lookup"><span data-stu-id="05915-297">To increase the requested page size, specify `PullOptions` when using `PullAsync()`:</span></span>

```
PullOptions pullOptions = new PullOptions
    {
        MaxPageSize = 100
    };
```

<span data-ttu-id="05915-298">Feltéve, hogy végzett a `PageSize` egyenlő vagy nagyobb, mint 100 belül a kiszolgáló, a kérelem legfeljebb 100 elemek adja vissza.</span><span class="sxs-lookup"><span data-stu-id="05915-298">Assuming you have made the `PageSize` equal to or greater than 100 within the server, a request returns up to 100 items.</span></span>

## <span data-ttu-id="05915-299"><a name="#offlinesync"></a>A kapcsolat nélküli táblázatok használata</span><span class="sxs-lookup"><span data-stu-id="05915-299"><a name="#offlinesync"></a>Work with Offline Tables</span></span>
<span data-ttu-id="05915-300">Kapcsolat nélküli táblák egy helyi SQLite tároló a tároló adatokat használ, amikor a kapcsolat nélküli használja.</span><span class="sxs-lookup"><span data-stu-id="05915-300">Offline tables use a local SQLite store to store data for use when offline.</span></span>  <span data-ttu-id="05915-301">A helyi elleni végzett műveletek minden tábla SQLite helyett a távoli kiszolgáló tárolójában tárolja.</span><span class="sxs-lookup"><span data-stu-id="05915-301">All table operations are done against the local SQLite store instead of the remote server store.</span></span>  <span data-ttu-id="05915-302">Egy kapcsolat nélküli tábla létrehozásához először készítse elő a projekthez:</span><span class="sxs-lookup"><span data-stu-id="05915-302">To create an offline table, first prepare your project:</span></span>

1. <span data-ttu-id="05915-303">A Visual Studióban, kattintson a jobb gombbal a megoldás > **NuGet-csomagok kezelése megoldáshoz...** , majd keresse meg, és telepítse a **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-csomagot az összes projektet a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="05915-303">In Visual Studio, right-click the solution > **Manage NuGet Packages for Solution...**, then search for and install the **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet package for all projects in the solution.</span></span>
2. <span data-ttu-id="05915-304">(Választható) Windows-eszközök támogatása érdekében telepítse a következő SQLite futtatókörnyezetek egyike:</span><span class="sxs-lookup"><span data-stu-id="05915-304">(Optional) To support Windows devices, install one of the following SQLite runtime packages:</span></span>

   * <span data-ttu-id="05915-305">**Windows 8.1 futásidejű:** telepítése [a Windows 8.1 SQLite][3].</span><span class="sxs-lookup"><span data-stu-id="05915-305">**Windows 8.1 Runtime:** Install [SQLite for Windows 8.1][3].</span></span>
   * <span data-ttu-id="05915-306">**Windows Phone 8.1:** telepítése [a Windows Phone 8.1 SQLite][4].</span><span class="sxs-lookup"><span data-stu-id="05915-306">**Windows Phone 8.1:** Install [SQLite for Windows Phone 8.1][4].</span></span>
   * <span data-ttu-id="05915-307">**Az univerzális Windows Platform** telepítése [az univerzális Windows SQLite][5].</span><span class="sxs-lookup"><span data-stu-id="05915-307">**Universal Windows Platform** Install [SQLite for the Universal Windows][5].</span></span>
3. <span data-ttu-id="05915-308">(Nem kötelező).</span><span class="sxs-lookup"><span data-stu-id="05915-308">(Optional).</span></span> <span data-ttu-id="05915-309">Windows-eszközökhöz, kattintson a **hivatkozások** > **hivatkozás hozzáadása...** , bontsa ki a **Windows** mappa > **bővítmények**, engedélyeznie kell a megfelelő **SQLite for Windows** SDK, valamint a **Visual C++ 2013 futásidejű Windows** SDK.</span><span class="sxs-lookup"><span data-stu-id="05915-309">For Windows devices, click **References** > **Add Reference...**, expand the **Windows** folder > **Extensions**, then enable the appropriate **SQLite for Windows** SDK along with the **Visual C++ 2013 Runtime for Windows** SDK.</span></span>
    <span data-ttu-id="05915-310">Az SQLite SDK nevek függően eltérőek az egyes Windows-platformra.</span><span class="sxs-lookup"><span data-stu-id="05915-310">The SQLite SDK names vary slightly with each Windows platform.</span></span>

<span data-ttu-id="05915-311">Egy tábla hivatkozás létrehozása előtt elő kell készíteni a helyi tárolójába:</span><span class="sxs-lookup"><span data-stu-id="05915-311">Before a table reference can be created, the local store must be prepared:</span></span>

```
var store = new MobileServiceSQLiteStore(Constants.OfflineDbPath);
store.DefineTable<TodoItem>();

//Initializes the SyncContext using the default IMobileServiceSyncHandler.
await this.client.SyncContext.InitializeAsync(store);
```

<span data-ttu-id="05915-312">Tanúsítványtár inicializálásához általában akkor történik, az ügyfél létrehozása után azonnal.</span><span class="sxs-lookup"><span data-stu-id="05915-312">Store initialization is normally done immediately after the client is created.</span></span>  <span data-ttu-id="05915-313">A **OfflineDbPath** használja az Ön által támogatott összes platformon megfelelő legyen a fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="05915-313">The **OfflineDbPath** should be a filename suitable for use on all platforms that you support.</span></span>  <span data-ttu-id="05915-314">Ha az elérési út egy teljes mértékben minősített elérési út (Ez azt jelenti, hogy kezdődik perjellel), majd, hogy az elérési utat használja.</span><span class="sxs-lookup"><span data-stu-id="05915-314">If the path is a fully qualified path (that is, it starts with a slash), then that path is used.</span></span>  <span data-ttu-id="05915-315">Ha az elérési út nem teljesen minősített, a fájl egy platformspecifikus helyre kerül.</span><span class="sxs-lookup"><span data-stu-id="05915-315">If the path is not fully qualified, the file is placed in a platform-specific location.</span></span>

* <span data-ttu-id="05915-316">Az iOS és Android-eszközök esetében az alapértelmezett elérési út a "Személyes fájlok" mappa.</span><span class="sxs-lookup"><span data-stu-id="05915-316">For iOS and Android devices, the default path is the "Personal Files" folder.</span></span>
* <span data-ttu-id="05915-317">Windows-eszközök esetén az alapértelmezett útvonal: az alkalmazás-specifikus "AppData" mappa.</span><span class="sxs-lookup"><span data-stu-id="05915-317">For Windows devices, the default path is the application-specific "AppData" folder.</span></span>

<span data-ttu-id="05915-318">Egy táblahivatkozás használatával lehet megszerezni a `GetSyncTable<>` módszert:</span><span class="sxs-lookup"><span data-stu-id="05915-318">A table reference can be obtained using the `GetSyncTable<>` method:</span></span>

```
var table = client.GetSyncTable<TodoItem>();
```

<span data-ttu-id="05915-319">Nem kell, hogy egy kapcsolat nélküli táblázat hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="05915-319">You do not need to authenticate to use an offline table.</span></span>  <span data-ttu-id="05915-320">Csak kell hitelesíteni, ha a háttérszolgáltatáshoz kommunikációra.</span><span class="sxs-lookup"><span data-stu-id="05915-320">You only need to authenticate when you are communicating with the backend service.</span></span>

### <span data-ttu-id="05915-321"><a name="syncoffline"></a>Egy kapcsolat nélküli tábla szinkronizálása</span><span class="sxs-lookup"><span data-stu-id="05915-321"><a name="syncoffline"></a>Syncing an Offline Table</span></span>
<span data-ttu-id="05915-322">Kapcsolat nélküli táblák nincsenek szinkronizálva a háttéralkalmazással való alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="05915-322">Offline tables are not synchronized with the backend by default.</span></span>  <span data-ttu-id="05915-323">Szinkronizálás két részre van osztva.</span><span class="sxs-lookup"><span data-stu-id="05915-323">Synchronization is split into two pieces.</span></span>  <span data-ttu-id="05915-324">Töltsön le új elemek külön-külön tolható módosításokat.</span><span class="sxs-lookup"><span data-stu-id="05915-324">You can push changes separately from downloading new items.</span></span>  <span data-ttu-id="05915-325">Íme egy jellegzetes szinkronizálási módszere:</span><span class="sxs-lookup"><span data-stu-id="05915-325">Here is a typical sync method:</span></span>

```
public async Task SyncAsync()
{
    ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

    try
    {
        await this.client.SyncContext.PushAsync();

        await this.todoTable.PullAsync(
            //The first parameter is a query name that is used internally by the client SDK to implement incremental sync.
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

    // Simple error/conflict handling. A real application would handle the various errors like network conditions,
    // server conflicts and others via the IMobileServiceSyncHandler.
    if (syncErrors != null)
    {
        foreach (var error in syncErrors)
        {
            if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
            {
                //Update failed, reverting to server's copy.
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

<span data-ttu-id="05915-326">Ha az első argumentumának `PullAsync` null értékű, akkor nem használja a növekményes szinkronizálás.</span><span class="sxs-lookup"><span data-stu-id="05915-326">If the first argument to `PullAsync` is null, then incremental sync is not used.</span></span>  <span data-ttu-id="05915-327">Minden egyes szinkronizálási műveletet lekéri az összes rekordot.</span><span class="sxs-lookup"><span data-stu-id="05915-327">Each sync operation retrieves all records.</span></span>

<span data-ttu-id="05915-328">Az SDK-t hajt végre egy implicit `PushAsync()` előtt az rekord.</span><span class="sxs-lookup"><span data-stu-id="05915-328">The SDK performs an implicit `PushAsync()` before pulling records.</span></span>

<span data-ttu-id="05915-329">Kezelési ütközésben történik, az egy `PullAsync()` metódust.</span><span class="sxs-lookup"><span data-stu-id="05915-329">Conflict handling happens on a `PullAsync()` method.</span></span>  <span data-ttu-id="05915-330">Ugyanúgy online táblák ütközés is foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="05915-330">You can deal with conflicts in the same way as online tables.</span></span>  <span data-ttu-id="05915-331">Az ütközés hozzák amikor `PullAsync()` ahelyett, hogy közben az insert, update vagy delete nevezik.</span><span class="sxs-lookup"><span data-stu-id="05915-331">The conflict is produced when `PullAsync()` is called instead of during the insert, update, or delete.</span></span> <span data-ttu-id="05915-332">Több ütközés fordulhat elő, ha azok egyetlen MobileServicePushFailedException be vannak becsomagolva.</span><span class="sxs-lookup"><span data-stu-id="05915-332">If multiple conflicts happen, they are bundled into a single MobileServicePushFailedException.</span></span>  <span data-ttu-id="05915-333">Minden egyes hiba külön kezelni.</span><span class="sxs-lookup"><span data-stu-id="05915-333">Handle each failure separately.</span></span>

## <span data-ttu-id="05915-334"><a name="#customapi"></a>Egy egyéni API használata</span><span class="sxs-lookup"><span data-stu-id="05915-334"><a name="#customapi"></a>Work with a custom API</span></span>
<span data-ttu-id="05915-335">Egy egyéni API lehetővé teszi, hogy adhatók meg egyéni végpontokat teszi közzé a kiszolgálói funkciót, amely nem egy INSERT utasítás van leképezve, frissítése, törlése, vagy olvasási művelete.</span><span class="sxs-lookup"><span data-stu-id="05915-335">A custom API enables you to define custom endpoints that expose server functionality that does not map to an insert, update, delete, or read operation.</span></span> <span data-ttu-id="05915-336">Egy egyéni API használatával is befolyásolni további üzenetküldés, beleértve az olvasási és HTTP-üzenet fejlécek beállítása meghatározó JSON nem üzenet törzsének formátumban.</span><span class="sxs-lookup"><span data-stu-id="05915-336">By using a custom API, you can have more control over messaging, including reading and setting HTTP message headers and defining a message body format other than JSON.</span></span>

<span data-ttu-id="05915-337">Egy egyéni API hívása egyik a [InvokeApiAsync] módszerek az ügyfélen.</span><span class="sxs-lookup"><span data-stu-id="05915-337">You call a custom API by calling one of the [InvokeApiAsync] methods on the client.</span></span> <span data-ttu-id="05915-338">Például a következő kódsort egy POST kérést küld a **completeAll** a háttérkiszolgálón API:</span><span class="sxs-lookup"><span data-stu-id="05915-338">For example, the following line of code sends a POST request to the **completeAll** API on the backend:</span></span>

```
var result = await client.InvokeApiAsync<MarkAllResult>("completeAll", System.Net.Http.HttpMethod.Post, null);
```

<span data-ttu-id="05915-339">Az űrlap típusos metódusát, és megköveteli, hogy a **MarkAllResult** visszatérési, van definiálva típus.</span><span class="sxs-lookup"><span data-stu-id="05915-339">This form is a typed method call and requires that the **MarkAllResult** return type is defined.</span></span> <span data-ttu-id="05915-340">Típusos és a nem típusos metódusok támogatottak.</span><span class="sxs-lookup"><span data-stu-id="05915-340">Both typed and untyped methods are supported.</span></span>

<span data-ttu-id="05915-341">A InvokeApiAsync() metódus lefoglalja a API hívása, kivéve, ha a API-val kezdődik-e használni kívánt "/ api /" a "/".</span><span class="sxs-lookup"><span data-stu-id="05915-341">The InvokeApiAsync() method prepends '/api/' to the API that you wish to call unless the API starts with a '/'.</span></span>
<span data-ttu-id="05915-342">Példa:</span><span class="sxs-lookup"><span data-stu-id="05915-342">For example:</span></span>

* <span data-ttu-id="05915-343">`InvokeApiAsync("completeAll",...)`meghívja a /api/completeAll a háttér</span><span class="sxs-lookup"><span data-stu-id="05915-343">`InvokeApiAsync("completeAll",...)` calls /api/completeAll on the backend</span></span>
* <span data-ttu-id="05915-344">`InvokeApiAsync("/.auth/me",...)`a háttérkiszolgálón hívások /.auth/me</span><span class="sxs-lookup"><span data-stu-id="05915-344">`InvokeApiAsync("/.auth/me",...)` calls /.auth/me on the backend</span></span>

<span data-ttu-id="05915-345">InvokeApiAsync segítségével bármely WebAPI, beleértve a nem meghatározott e WebAPIs az Azure Mobile Apps hívja.</span><span class="sxs-lookup"><span data-stu-id="05915-345">You can use InvokeApiAsync to call any WebAPI, including those WebAPIs that are not defined with Azure Mobile Apps.</span></span>  <span data-ttu-id="05915-346">InvokeApiAsync() használata esetén a megfelelő fejlécek, beleértve a hitelesítési fejléceket, a kérelem küldése.</span><span class="sxs-lookup"><span data-stu-id="05915-346">When you use InvokeApiAsync(), the appropriate headers, including authentication headers, are sent with the request.</span></span>

## <span data-ttu-id="05915-347"><a name="authentication"></a>Felhasználók hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="05915-347"><a name="authentication"></a>Authenticate users</span></span>
<span data-ttu-id="05915-348">Mobile Apps hitelesítése és engedélyezése a felhasználók alkalmazás különböző külső Identitásszolgáltatók támogatja: Facebook, a Google, a Microsoft Account, a Twitter és az Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="05915-348">Mobile Apps supports authenticating and authorizing app users using various external identity providers: Facebook, Google, Microsoft Account, Twitter, and Azure Active Directory.</span></span> <span data-ttu-id="05915-349">A engedélyeket korlátozhatja a hozzáférést a megadott művelet csak a hitelesített felhasználók táblákon.</span><span class="sxs-lookup"><span data-stu-id="05915-349">You can set permissions on tables to restrict access for specific operations to only authenticated users.</span></span> <span data-ttu-id="05915-350">Hitelesített felhasználók identitásának használhatja, ha az engedélyezési szabályok megvalósítását a kiszolgáló parancsfájlokat.</span><span class="sxs-lookup"><span data-stu-id="05915-350">You can also use the identity of authenticated users to implement authorization rules in server scripts.</span></span> <span data-ttu-id="05915-351">További információt a [hitelesítés alkalmazásokhoz történő hozzáadását] ismertető oktatóanyagban találhat.</span><span class="sxs-lookup"><span data-stu-id="05915-351">For more information, see the tutorial [Add authentication to your app].</span></span>

<span data-ttu-id="05915-352">Két hitelesítési forgalom támogatottak: *ügyfél által felügyelt* és *kiszolgáló által kezelt* folyamata.</span><span class="sxs-lookup"><span data-stu-id="05915-352">Two authentication flows are supported: *client-managed* and *server-managed* flow.</span></span> <span data-ttu-id="05915-353">A kiszolgáló által felügyelt folyamat nyújt a legegyszerűbb felhasználói hitelesítés, a szolgáltató webes hitelesítés felület támaszkodnak.</span><span class="sxs-lookup"><span data-stu-id="05915-353">The server-managed flow provides the simplest authentication experience, as it relies on the provider's web authentication interface.</span></span> <span data-ttu-id="05915-354">Az ügyfél által felügyelt folyamat lehetővé teszi a eszközspecifikus képességekkel szorosabb integrációt, a szolgáltatói eszközspecifikus SDK-k támaszkodnak.</span><span class="sxs-lookup"><span data-stu-id="05915-354">The client-managed flow allows for deeper integration with device-specific capabilities as it relies on provider-specific device-specific SDKs.</span></span>

> [!NOTE]
> <span data-ttu-id="05915-355">Az éles alkalmazásokban lévő ügyfél által felügyelt folyamat használatát javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="05915-355">We recommend using a client-managed flow in your production apps.</span></span>

<span data-ttu-id="05915-356">Hitelesítés beállítása az alkalmazás regisztrálnia kell egy vagy több identitás-szolgáltatóktól.</span><span class="sxs-lookup"><span data-stu-id="05915-356">To set up authentication, you must register your app with one or more identity providers.</span></span>  <span data-ttu-id="05915-357">Az identitásszolgáltató egy ügyfél-Azonosítót és az alkalmazás egy ügyfélkulcsot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="05915-357">The identity provider generates a client ID and a client secret for your app.</span></span>  <span data-ttu-id="05915-358">Ezeket az értékeket az a kiszolgáló akkor értéke a engedélyezése az Azure App Service hitelesítés/engedélyezés.</span><span class="sxs-lookup"><span data-stu-id="05915-358">These values are then set in your backend to enable Azure App Service authentication/authorization.</span></span>  <span data-ttu-id="05915-359">További információkért kövesse a részletes utasításokat az oktatóanyag [hitelesítés alkalmazásokhoz történő hozzáadását].</span><span class="sxs-lookup"><span data-stu-id="05915-359">For more information, follow the detailed instructions in the tutorial [Add authentication to your app].</span></span>

<span data-ttu-id="05915-360">Ez a szakasz a következő témákat tárgyalja:</span><span class="sxs-lookup"><span data-stu-id="05915-360">The following topics are covered in this section:</span></span>

* [<span data-ttu-id="05915-361">Ügyfél által felügyelt hitelesítés</span><span class="sxs-lookup"><span data-stu-id="05915-361">Client-managed authentication</span></span>](#clientflow)
* [<span data-ttu-id="05915-362">A hitelesítési kiszolgáló által felügyelt</span><span class="sxs-lookup"><span data-stu-id="05915-362">Server-managed authentication</span></span>](#serverflow)
* [<span data-ttu-id="05915-363">A hitelesítési jogkivonat gyorsítótárazása</span><span class="sxs-lookup"><span data-stu-id="05915-363">Caching the authentication token</span></span>](#caching)

### <span data-ttu-id="05915-364"><a name="clientflow"></a>Ügyfél által felügyelt hitelesítés</span><span class="sxs-lookup"><span data-stu-id="05915-364"><a name="clientflow"></a>Client-managed authentication</span></span>
<span data-ttu-id="05915-365">Az alkalmazás egymástól függetlenül lépjen kapcsolatba az identitásszolgáltató, és adja meg a visszaküldött jogkivonat bejelentkezés során a fájlok.</span><span class="sxs-lookup"><span data-stu-id="05915-365">Your app can independently contact the identity provider and then provide the returned token during login with your backend.</span></span> <span data-ttu-id="05915-366">Az ügyféltanúsítvány-folyamat lehetővé teszi, hogy egy egyszeri bejelentkezéses élményt nyújtanak a felhasználóknak, vagy további felhasználói adatok beolvasása az identitásszolgáltató által.</span><span class="sxs-lookup"><span data-stu-id="05915-366">This client flow enables you to provide a single sign-on experience for users or to retrieve additional user data from the identity provider.</span></span> <span data-ttu-id="05915-367">Folyamat ügyfélhitelesítés használata ajánlott a használatával egy kiszolgáló folyamata, mint az identitásszolgáltató SDK több natív UX abba biztosít, és lehetővé teszi, hogy további testreszabási.</span><span class="sxs-lookup"><span data-stu-id="05915-367">Client flow authentication is preferred to using a server flow as the identity provider SDK provides a more native UX feel and allows for additional customization.</span></span>

<span data-ttu-id="05915-368">Példák a következő ügyfél-folyamat hitelesítési minták áll rendelkezésre:</span><span class="sxs-lookup"><span data-stu-id="05915-368">Examples are provided for the following client-flow authentication patterns:</span></span>

* [<span data-ttu-id="05915-369">Az Active Directory hitelesítési könyvtár</span><span class="sxs-lookup"><span data-stu-id="05915-369">Active Directory Authentication Library</span></span>](#adal)
* [<span data-ttu-id="05915-370">Facebook-on vagy a Google</span><span class="sxs-lookup"><span data-stu-id="05915-370">Facebook or Google</span></span>](#client-facebook)
* [<span data-ttu-id="05915-371">Élő SDK</span><span class="sxs-lookup"><span data-stu-id="05915-371">Live SDK</span></span>](#client-livesdk)

#### <span data-ttu-id="05915-372"><a name="adal"></a>Az Active Directory Authentication Library a felhasználók hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="05915-372"><a name="adal"></a>Authenticate users with the Active Directory Authentication Library</span></span>
<span data-ttu-id="05915-373">Az Active Directory Authentication Library (ADAL) segítségével kezdeményezhet felhasználói hitelesítést, az Azure Active Directory-hitelesítéssel ügyfél.</span><span class="sxs-lookup"><span data-stu-id="05915-373">You can use the Active Directory Authentication Library (ADAL) to initiate user authentication from the client using Azure Active Directory authentication.</span></span>

1. <span data-ttu-id="05915-374">A mobil-háttéralkalmazás az aad-ben bejelentkezés konfigurálása a következő a [App Service konfigurálása az Active Directory bejelentkezési] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="05915-374">Configure your mobile app backend for AAD sign-on by following the [How to configure App Service for Active Directory login] tutorial.</span></span> <span data-ttu-id="05915-375">Ügyeljen arra, hogy a natív ügyfélalkalmazás regisztrációján választható lépés elvégzése után.</span><span class="sxs-lookup"><span data-stu-id="05915-375">Make sure to complete the optional step of registering a native client application.</span></span>
2. <span data-ttu-id="05915-376">A Visual Studio és Xamarin Studio, nyissa meg a projektet, és adjon hozzá egy hivatkozást, a `Microsoft.IdentityModel.CLients.ActiveDirectory` NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="05915-376">In Visual Studio or Xamarin Studio, open your project and add a reference to the `Microsoft.IdentityModel.CLients.ActiveDirectory` NuGet package.</span></span> <span data-ttu-id="05915-377">Ha keres, vegye fel az előzetes verzióját.</span><span class="sxs-lookup"><span data-stu-id="05915-377">When searching, include pre-release versions.</span></span>
3. <span data-ttu-id="05915-378">Adja hozzá a következő kódot az alkalmazásba, használja a platformtól függően.</span><span class="sxs-lookup"><span data-stu-id="05915-378">Add the following code to your application, according to the platform you are using.</span></span> <span data-ttu-id="05915-379">Az egyes ellenőrizze az alábbi új:</span><span class="sxs-lookup"><span data-stu-id="05915-379">In each, make the following replacements:</span></span>

   * <span data-ttu-id="05915-380">Cserélje le **INSERT-SZOLGÁLTATÓ-Itt** nevű, a bérlő, amelyben az alkalmazás üzembe.</span><span class="sxs-lookup"><span data-stu-id="05915-380">Replace **INSERT-AUTHORITY-HERE** with the name of the tenant in which you provisioned your application.</span></span> <span data-ttu-id="05915-381">A következő formátumban kell megadni https://login.microsoftonline.com/contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="05915-381">The format should be https://login.microsoftonline.com/contoso.onmicrosoft.com.</span></span> <span data-ttu-id="05915-382">Ez az érték lehet másolni az Azure Active Directory tartományi lapján a [a klasszikus Azure portálon].</span><span class="sxs-lookup"><span data-stu-id="05915-382">This value can be copied from the Domain tab in your Azure Active Directory in the [Azure classic portal].</span></span>
   * <span data-ttu-id="05915-383">Cserélje le **INSERT-erőforrás-azonosító-Itt** az ügyfél-azonosító a mobil-háttéralkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="05915-383">Replace **INSERT-RESOURCE-ID-HERE** with the client ID for your mobile app backend.</span></span> <span data-ttu-id="05915-384">Ezt úgy szerezheti be az ügyfél-azonosító a **speciális** lap **Azure Active Directory beállításai** a portálon.</span><span class="sxs-lookup"><span data-stu-id="05915-384">You can obtain the client ID from the **Advanced** tab under **Azure Active Directory Settings** in the portal.</span></span>
   * <span data-ttu-id="05915-385">Cserélje le **INSERT-ügyfél-azonosító-Itt** , az ügyfél-Azonosítót a natív ügyfélalkalmazás másolta.</span><span class="sxs-lookup"><span data-stu-id="05915-385">Replace **INSERT-CLIENT-ID-HERE** with the client ID you copied from the native client application.</span></span>
   * <span data-ttu-id="05915-386">Cserélje le **INSERT-REDIRECT-URI-Itt** a hellyel való */.auth/login/done* végpont, a HTTPS protokollt használ.</span><span class="sxs-lookup"><span data-stu-id="05915-386">Replace **INSERT-REDIRECT-URI-HERE** with your site's */.auth/login/done* endpoint, using the HTTPS scheme.</span></span> <span data-ttu-id="05915-387">Ez az érték a következőképpen kell kinéznie *https://contoso.azurewebsites.net/.auth/login/done*.</span><span class="sxs-lookup"><span data-stu-id="05915-387">This value should be similar to *https://contoso.azurewebsites.net/.auth/login/done*.</span></span>

     <span data-ttu-id="05915-388">Az egyes platformokon szükséges kódot a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="05915-388">The code needed for each platform follows:</span></span>

     <span data-ttu-id="05915-389">**Windows:**</span><span class="sxs-lookup"><span data-stu-id="05915-389">**Windows:**</span></span>

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

     <span data-ttu-id="05915-390">**Xamarin.iOS**</span><span class="sxs-lookup"><span data-stu-id="05915-390">**Xamarin.iOS**</span></span>

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

     <span data-ttu-id="05915-391">**Xamarin.Android**</span><span class="sxs-lookup"><span data-stu-id="05915-391">**Xamarin.Android**</span></span>

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

#### <span data-ttu-id="05915-392"><a name="client-facebook"></a>Egyszeri bejelentkezés Facebook-on vagy a Google származó jogkivonat használatával</span><span class="sxs-lookup"><span data-stu-id="05915-392"><a name="client-facebook"></a>Single Sign-On using a token from Facebook or Google</span></span>
<span data-ttu-id="05915-393">Az ügyféltanúsítvány-folyamat is használhat, ahogy az a kódrészletet a Facebook-on vagy a Google.</span><span class="sxs-lookup"><span data-stu-id="05915-393">You can use the client flow as shown in this snippet for Facebook or Google.</span></span>

```
var token = new JObject();
// Replace access_token_value with actual value of your access token obtained
// using the Facebook or Google SDK.
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
            // to MobileServiceAuthenticationProvider.Google if using Google auth.
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

#### <span data-ttu-id="05915-394"><a name="client-livesdk"></a>Egyszeri bejelentkezés Microsoft Account használatával az élő SDK-val</span><span class="sxs-lookup"><span data-stu-id="05915-394"><a name="client-livesdk"></a>Single Sign On using Microsoft Account with the Live SDK</span></span>
<span data-ttu-id="05915-395">Hitelesíti a felhasználókat, regisztrálnia kell az alkalmazást a Microsoft-fiók fejlesztői központban.</span><span class="sxs-lookup"><span data-stu-id="05915-395">To authenticate users, you must register your app at the Microsoft account Developer Center.</span></span> <span data-ttu-id="05915-396">A regisztrációs adatokat konfigurálja a mobilalkalmazás háttérrendszerének.</span><span class="sxs-lookup"><span data-stu-id="05915-396">Configure the registration details on your Mobile App backend.</span></span> <span data-ttu-id="05915-397">Hozzon létre egy Microsoft-fiók regisztrálása, és csatlakoztassa a mobil-háttéralkalmazást, hajtsa végre a lépéseket a [regisztrálja az alkalmazást egy Microsoft-fiók bejelentkezési használandó].</span><span class="sxs-lookup"><span data-stu-id="05915-397">To create a Microsoft account registration and connect it to your Mobile App backend, complete the steps in [Register your app to use a Microsoft account login].</span></span> <span data-ttu-id="05915-398">Ha az alkalmazás Windows áruház és a Windows Phone 8/Silverlight verzióit, először Regisztráljon a Windows Áruházbeli verziójához.</span><span class="sxs-lookup"><span data-stu-id="05915-398">If you have both Windows Store and Windows Phone 8/Silverlight versions of your app, register the Windows Store version first.</span></span>

<span data-ttu-id="05915-399">Az alábbi kód Live SDK használatával, és jelentkezzen be a mobilalkalmazás háttérrendszerének a visszaküldött jogkivonat alapján.</span><span class="sxs-lookup"><span data-stu-id="05915-399">The following code authenticates using Live SDK and uses the returned token to sign in to your Mobile App backend.</span></span>

```
private LiveConnectSession session;
    //private static string clientId = "<microsoft-account-client-id>";
private async System.Threading.Tasks.Task AuthenticateAsync()
{

    // Get the URL the Mobile App backend.
    var serviceUrl = App.MobileService.ApplicationUri.AbsoluteUri;

    // Create the authentication client for Windows Store using the service URL.
    LiveAuthClient liveIdClient = new LiveAuthClient(serviceUrl);
    //// Create the authentication client for Windows Phone using the client ID of the registration.
    //LiveAuthClient liveIdClient = new LiveAuthClient(clientId);

    while (session == null)
    {
        // Request the authentication token from the Live authentication service.
        // The wl.basic scope should always be requested.  Other scopes can be added
        LiveLoginResult result = await liveIdClient.LoginAsync(new string[] { "wl.basic" });
        if (result.Status == LiveConnectSessionStatus.Connected)
        {
            session = result.Session;

            // Get information about the logged-in user.
            LiveConnectClient client = new LiveConnectClient(session);
            LiveOperationResult meResult = await client.GetAsync("me");

            // Use the Microsoft account auth token to sign in to App Service.
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

<span data-ttu-id="05915-400">További információkért lásd: a [Windows Live SDK] dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="05915-400">For more information, see the [Windows Live SDK] documentation.</span></span>

### <span data-ttu-id="05915-401"><a name="serverflow"></a>A hitelesítési kiszolgáló által felügyelt</span><span class="sxs-lookup"><span data-stu-id="05915-401"><a name="serverflow"></a>Server-managed authentication</span></span>
<span data-ttu-id="05915-402">Miután regisztrálta az identitásszolgáltató, hívja az [LoginAsync] [MobileServiceClient] rendelkező metódust a [MobileServiceAuthenticationProvider] érték a szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="05915-402">Once you have registered your identity provider, call the [LoginAsync] method on the [MobileServiceClient] with the [MobileServiceAuthenticationProvider] value of your provider.</span></span> <span data-ttu-id="05915-403">Az alábbi kód például a kiszolgáló folyamata bejelentkezés kezdeményezi, Facebook-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="05915-403">For example, the following code initiates a server flow sign-in by using Facebook.</span></span>

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

<span data-ttu-id="05915-404">Ha eltérő Facebook identitásszolgáltató használ, módosítsa [MobileServiceAuthenticationProvider] a szolgáltató értékre.</span><span class="sxs-lookup"><span data-stu-id="05915-404">If you are using an identity provider other than Facebook, change the value of [MobileServiceAuthenticationProvider] to the value for your provider.</span></span>

<span data-ttu-id="05915-405">Egy kiszolgáló folyamatában Azure App Service szolgáltatás az OAuth hitelesítési folyamatot megjelenítése a bejelentkezési lapon a kiválasztott szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="05915-405">In a server flow, Azure App Service manages the OAuth authentication flow by displaying the sign-in page of the selected provider.</span></span>  <span data-ttu-id="05915-406">Miután az identity provider értéket ad eredményül, Azure App Service létrehoz egy App Service hitelesítés jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="05915-406">Once the identity provider returns, Azure App Service generates an App Service authentication token.</span></span> <span data-ttu-id="05915-407">A [LoginAsync] metódus értéket ad vissza egy [MobileServiceUser], amely biztosítja, hogy mind a [UserId] a hitelesített felhasználó és a [MobileServiceAuthenticationToken], egy JSON webes jogkivonatot (JWT).</span><span class="sxs-lookup"><span data-stu-id="05915-407">The [LoginAsync] method returns a [MobileServiceUser], which provides both the [UserId] of the authenticated user and the [MobileServiceAuthenticationToken], as a JSON web token (JWT).</span></span> <span data-ttu-id="05915-408">Ez a token gyorsítótárazható, és újra felhasználható, amíg le nem jár.</span><span class="sxs-lookup"><span data-stu-id="05915-408">This token can be cached and reused until it expires.</span></span> <span data-ttu-id="05915-409">További információkért lásd: [a hitelesítési jogkivonat gyorsítótárazás](#caching).</span><span class="sxs-lookup"><span data-stu-id="05915-409">For more information, see [Caching the authentication token](#caching).</span></span>

### <span data-ttu-id="05915-410"><a name="caching"></a>A hitelesítési jogkivonat gyorsítótárazása</span><span class="sxs-lookup"><span data-stu-id="05915-410"><a name="caching"></a>Caching the authentication token</span></span>
<span data-ttu-id="05915-411">Néhány esetben a bejelentkezési metódus hívása elkerülhető az első sikeres hitelesítést követően a hitelesítési jogkivonat-szolgáltatójáról való elhelyezésével.</span><span class="sxs-lookup"><span data-stu-id="05915-411">In some cases, the call to the login method can be avoided after the first successful authentication by storing the authentication token from the provider.</span></span>  <span data-ttu-id="05915-412">Windows áruház és az UWP-alkalmazások használhatják [PasswordVault] a következőképpen gyorsítótárában, miután egy sikeres bejelentkezés, a jelenlegi hitelesítési jogkivonat:</span><span class="sxs-lookup"><span data-stu-id="05915-412">Windows Store and UWP apps can use [PasswordVault] to cache the current authentication token after a successful sign-in, as follows:</span></span>

```
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook);

PasswordVault vault = new PasswordVault();
vault.Add(new PasswordCredential("Facebook", client.currentUser.UserId,
    client.currentUser.MobileServiceAuthenticationToken));
```

<span data-ttu-id="05915-413">A UserId értéket a UserName hitelesítő adat tárolja, és a lexikális elem a tárolt jelszóként.</span><span class="sxs-lookup"><span data-stu-id="05915-413">The UserId value is stored as the UserName of the credential and the token is the stored as the Password.</span></span> <span data-ttu-id="05915-414">A következő kezdő vállalkozások számára, ellenőrizheti a **PasswordVault** a gyorsítótárazott hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="05915-414">On subsequent start-ups, you can check the **PasswordVault** for cached credentials.</span></span> <span data-ttu-id="05915-415">Az alábbi példában gyorsítótárazott hitelesítő adatokat, ha találhatók, valamint egyéb próbál meg újra a háttéralkalmazással való:</span><span class="sxs-lookup"><span data-stu-id="05915-415">The following example uses cached credentials when they are found, and otherwise attempts to authenticate again with the backend:</span></span>

```
// Try to retrieve stored credentials.
var creds = vault.FindAllByResource("Facebook").FirstOrDefault();
if (creds != null)
{
    // Create the current user from the stored credentials.
    client.currentUser = new MobileServiceUser(creds.UserName);
    client.currentUser.MobileServiceAuthenticationToken =
        vault.Retrieve("Facebook", creds.UserName).Password;
}
else
{
    // Regular login flow and cache the token as shown above.
}
```

<span data-ttu-id="05915-416">Amikor a felhasználó kijelentkezik, el kell távolítani a tárolt hitelesítő adatok, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="05915-416">When you sign out a user, you must also remove the stored credential, as follows:</span></span>

```
client.Logout();
vault.Remove(vault.Retrieve("Facebook", client.currentUser.UserId));
```

<span data-ttu-id="05915-417">Xamarin alkalmazások használatát a [Xamarin.Auth] API-k biztonságosan tárolni a felhasználó hitelesítő adatait egy **fiók** objektum.</span><span class="sxs-lookup"><span data-stu-id="05915-417">Xamarin    apps use the [Xamarin.Auth] APIs to securely store credentials in an **Account** object.</span></span> <span data-ttu-id="05915-418">Ezen API-k használatának példája, tekintse meg a [AuthStore.cs] a forráskód fájlja a [minta megosztása ContosoMoments fénykép](https://github.com/azure-appservice-samples/ContosoMoments).</span><span class="sxs-lookup"><span data-stu-id="05915-418">For an example of using these APIs, see the [AuthStore.cs] code file in the [ContosoMoments photo sharing sample](https://github.com/azure-appservice-samples/ContosoMoments).</span></span>

<span data-ttu-id="05915-419">Ügyfél által felügyelt hitelesítés használatakor a Facebook- vagy Twitter például szolgáltatótól kapott hozzáférési jogkivonat is képes gyorsítótárazni.</span><span class="sxs-lookup"><span data-stu-id="05915-419">When you use client-managed authentication, you can also cache the access token obtained from your provider such as Facebook or Twitter.</span></span> <span data-ttu-id="05915-420">Ez a token is kell adni egy új hitelesítési jogkivonatot kérhet a háttér, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="05915-420">This token can be supplied to request a new authentication token from the backend, as follows:</span></span>

```
var token = new JObject();
// Replace <your_access_token_value> with actual value of your access token
token.Add("access_token", "<your_access_token_value>");

// Authenticate using the access token.
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
```

## <span data-ttu-id="05915-421"><a name="pushnotifications"></a>Leküldéses értesítések</span><span class="sxs-lookup"><span data-stu-id="05915-421"><a name="pushnotifications"></a>Push Notifications</span></span>
<span data-ttu-id="05915-422">A következő témakörök a leküldéses értesítések terjed ki:</span><span class="sxs-lookup"><span data-stu-id="05915-422">The following topics cover Push Notifications:</span></span>

* [<span data-ttu-id="05915-423">A leküldéses értesítések regisztrálása</span><span class="sxs-lookup"><span data-stu-id="05915-423">Register for Push Notifications</span></span>](#register-for-push)
* [<span data-ttu-id="05915-424">Szerezzen be egy Windows Áruházbeli csomag biztonsági azonosítója</span><span class="sxs-lookup"><span data-stu-id="05915-424">Obtain a Windows Store package SID</span></span>](#package-sid)
* [<span data-ttu-id="05915-425">Platformok közötti sablonok regisztrálása</span><span class="sxs-lookup"><span data-stu-id="05915-425">Register with Cross-platform templates</span></span>](#register-xplat)

### <span data-ttu-id="05915-426"><a name="register-for-push"></a>Útmutató: a leküldéses értesítések regisztrálása</span><span class="sxs-lookup"><span data-stu-id="05915-426"><a name="register-for-push"></a>How to: Register for Push Notifications</span></span>
<span data-ttu-id="05915-427">A Mobile Apps-ügyfél lehetővé teszi az Azure Notification Hubs leküldéses értesítések regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="05915-427">The Mobile Apps client enables you to register for push notifications with Azure Notification Hubs.</span></span> <span data-ttu-id="05915-428">Amikor regisztrál, be kell szereznie egy leíró beszerezni az a platform-specifikus leküldéses értesítési szolgáltatás (PNS).</span><span class="sxs-lookup"><span data-stu-id="05915-428">When registering, you obtain a handle that you obtain from the platform-specific Push Notification Service (PNS).</span></span> <span data-ttu-id="05915-429">Ezt az értéket a címkékkel együtt, majd adja meg, a regisztráció létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="05915-429">You then provide this value along with any tags when you create the registration.</span></span> <span data-ttu-id="05915-430">Az alábbi kód regisztrálja a Windows-alkalmazást a leküldéses értesítések és a Windows értesítési szolgáltatásának (WNS):</span><span class="sxs-lookup"><span data-stu-id="05915-430">The following code registers your Windows app for push notifications with the Windows Notification Service (WNS):</span></span>

```
private async void InitNotificationsAsync()
{
    // Request a push notification channel.
    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // Register for notifications using the new channel.
    await MobileService.GetPush().RegisterNativeAsync(channel.Uri, null);
}
```

<span data-ttu-id="05915-431">Ha a WNS leküldendő, akkor meg kell [Windows áruház csomagot SID](#package-sid).</span><span class="sxs-lookup"><span data-stu-id="05915-431">If you are pushing to WNS, then you MUST [obtain a Windows Store package SID](#package-sid).</span></span>  <span data-ttu-id="05915-432">További információ a Windows-alkalmazásokra, hogyan kell regisztrálni a sablon regisztrációhoz, beleértve: [leküldéses értesítések hozzáadása az alkalmazáshoz].</span><span class="sxs-lookup"><span data-stu-id="05915-432">For more information on Windows apps, including how to register for template registrations, see [Add push notifications to your app].</span></span>

<span data-ttu-id="05915-433">Címkék kér az ügyfél nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="05915-433">Requesting tags from the client is not supported.</span></span>  <span data-ttu-id="05915-434">A regisztrációs csendes eldobott címke kérelmek.</span><span class="sxs-lookup"><span data-stu-id="05915-434">Tag Requests are silently dropped from registration.</span></span>
<span data-ttu-id="05915-435">Ha az eszköz regisztrálása címkék, hozzon létre egy egyéni API-t, amely a regisztráció elvégzéséhez a nevünkben a Notification Hubs API segítségével.</span><span class="sxs-lookup"><span data-stu-id="05915-435">If you wish to register your device with tags, create a Custom API that uses the Notification Hubs API to perform the registration on your behalf.</span></span>  <span data-ttu-id="05915-436">[Az egyéni API](#customapi) ahelyett, hogy a `RegisterNativeAsync()` metódust.</span><span class="sxs-lookup"><span data-stu-id="05915-436">[Call the Custom API](#customapi) instead of the `RegisterNativeAsync()` method.</span></span>

### <span data-ttu-id="05915-437"><a name="package-sid"></a>Hogyan: szerezze be a Windows áruház csomag biztonsági azonosítója</span><span class="sxs-lookup"><span data-stu-id="05915-437"><a name="package-sid"></a>How to: Obtain a Windows Store package SID</span></span>
<span data-ttu-id="05915-438">A Windows Áruházbeli alkalmazások leküldéses értesítések engedélyezése a csomag biztonsági azonosítója szükséges.</span><span class="sxs-lookup"><span data-stu-id="05915-438">A package SID is needed for enabling push notifications in Windows Store apps.</span></span>  <span data-ttu-id="05915-439">Ha szeretne kapni a csomag biztonsági azonosítója, regisztrálhatja alkalmazását a Windows áruházban.</span><span class="sxs-lookup"><span data-stu-id="05915-439">To receive a package SID, register your application with the Windows Store.</span></span>

<span data-ttu-id="05915-440">Ez az érték beszerzése:</span><span class="sxs-lookup"><span data-stu-id="05915-440">To obtain this value:</span></span>

1. <span data-ttu-id="05915-441">A Visual Studio Solution Explorerben kattintson a jobb gombbal a Windows Áruházbeli alkalmazás projektjére, kattintson a **tároló** > **az áruház alkalmazás társítása...** .</span><span class="sxs-lookup"><span data-stu-id="05915-441">In Visual Studio Solution Explorer, right-click the Windows Store app project, click **Store** > **Associate App with the Store...**.</span></span>
2. <span data-ttu-id="05915-442">A varázslóban kattintson **következő**, jelentkezzen be Microsoft-fiókjával, adjon meg egy nevet az alkalmazáshoz a **lefoglalni egy új alkalmazás neve**, majd kattintson a **tartalék**.</span><span class="sxs-lookup"><span data-stu-id="05915-442">In the wizard, click **Next**, sign in with your Microsoft account, type a name for your app in **Reserve a new app name**, then click **Reserve**.</span></span>
3. <span data-ttu-id="05915-443">Az alkalmazás-regisztráció sikeres létrehozása után válassza ki az alkalmazás nevére, kattintson a **következő**, és kattintson a **társítása**.</span><span class="sxs-lookup"><span data-stu-id="05915-443">After the app registration is successfully created, select the app name, click **Next**, and then click **Associate**.</span></span>
4. <span data-ttu-id="05915-444">Jelentkezzen be a [Windows fejlesztői központ] a Microsoft Account.</span><span class="sxs-lookup"><span data-stu-id="05915-444">Log in to the [Windows Dev Center] using your Microsoft Account.</span></span> <span data-ttu-id="05915-445">A **alkalmazásaimat**, kattintson a létrehozott alkalmazás regisztrációját.</span><span class="sxs-lookup"><span data-stu-id="05915-445">Under **My apps**, click the app registration you created.</span></span>
5. <span data-ttu-id="05915-446">Kattintson a **Alkalmazáskezelés** > **identitását**, majd görgessen le a Keresés és a **CSOMAGAZONOSÍTÓT**.</span><span class="sxs-lookup"><span data-stu-id="05915-446">Click **App management** > **App identity**, and then scroll down to find your **Package SID**.</span></span>

<span data-ttu-id="05915-447">A csomag biztonsági AZONOSÍTÓJÁT számos felhasználási kezelni egy URI-azonosítóként, ebben az esetben kell használnia *ms-app: / /* sémát.</span><span class="sxs-lookup"><span data-stu-id="05915-447">Many uses of the package SID treat it as a URI, in which case you need to use *ms-app://* as the scheme.</span></span> <span data-ttu-id="05915-448">Jegyezze meg a csomag biztonsági azonosítója, ez az érték előtagjaként hozzáfűzésével megfelelő verzióját.</span><span class="sxs-lookup"><span data-stu-id="05915-448">Make note of the version of your package SID formed by concatenating this value as a prefix.</span></span>

<span data-ttu-id="05915-449">Xamarin-alkalmazásokat kell regisztrálnia az alkalmazást az iOS vagy Android rendszereken futó további kódot igényel.</span><span class="sxs-lookup"><span data-stu-id="05915-449">Xamarin apps require some additional code to be able to register an app running on the iOS or Android platforms.</span></span> <span data-ttu-id="05915-450">További információkért lásd: a témakör a platformra:</span><span class="sxs-lookup"><span data-stu-id="05915-450">For more information, see the topic for your platform:</span></span>

* [<span data-ttu-id="05915-451">Xamarin.Android</span><span class="sxs-lookup"><span data-stu-id="05915-451">Xamarin.Android</span></span>](app-service-mobile-xamarin-android-get-started-push.md#add-push)
* [<span data-ttu-id="05915-452">Xamarin.iOS</span><span class="sxs-lookup"><span data-stu-id="05915-452">Xamarin.iOS</span></span>](app-service-mobile-xamarin-ios-get-started-push.md#add-push-notifications-to-your-app)

### <span data-ttu-id="05915-453"><a name="register-xplat"></a>Útmutató: regisztráció leküldéses sablonok platformfüggetlen értesítések küldéséhez</span><span class="sxs-lookup"><span data-stu-id="05915-453"><a name="register-xplat"></a>How to: Register push templates to send cross-platform notifications</span></span>
<span data-ttu-id="05915-454">Sablonok regisztrálásához használja a `RegisterAsync()` metódust használ a sablonokat, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="05915-454">To register templates, use the `RegisterAsync()` method with the templates, as follows:</span></span>

```
JObject templates = myTemplates();
MobileService.GetPush().RegisterAsync(channel.Uri, templates);
```

<span data-ttu-id="05915-455">A sablonok kell `JObject` meg kell adnia, és tartalmazhat több sablonok a következő JSON formátummal:</span><span class="sxs-lookup"><span data-stu-id="05915-455">Your templates should be `JObject` types and can contain multiple templates in the following JSON format:</span></span>

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

<span data-ttu-id="05915-456">A metódus **RegisterAsync()** másodlagos Csempéket is fogad:</span><span class="sxs-lookup"><span data-stu-id="05915-456">The method **RegisterAsync()** also accepts Secondary Tiles:</span></span>

```
MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);
```

<span data-ttu-id="05915-457">Található összes kódcímkének számítógépnél tisztító vannak a biztonsági regisztrálása során.</span><span class="sxs-lookup"><span data-stu-id="05915-457">All tags are stripped away during registration for security.</span></span> <span data-ttu-id="05915-458">Címkék hozzáadása a telepítésekkel és sablonok telepítések belül, lásd: [használata a .NET-háttérrendszer server SDK az Azure Mobile Apps].</span><span class="sxs-lookup"><span data-stu-id="05915-458">To add tags to installations or templates within installations, see [Work with the .NET backend server SDK for Azure Mobile Apps].</span></span>

<span data-ttu-id="05915-459">A regisztrált sablonok használatával értesítést küldeni, tekintse meg a [Notification Hubs API-k].</span><span class="sxs-lookup"><span data-stu-id="05915-459">To send notifications utilizing these registered templates, refer to the [Notification Hubs APIs].</span></span>

## <span data-ttu-id="05915-460"><a name="misc"></a>Vegyes kapcsolatos témakörök</span><span class="sxs-lookup"><span data-stu-id="05915-460"><a name="misc"></a>Miscellaneous Topics</span></span>
### <span data-ttu-id="05915-461"><a name="errors"></a>Hogyan: hibák kezelésének</span><span class="sxs-lookup"><span data-stu-id="05915-461"><a name="errors"></a>How to: Handle errors</span></span>
<span data-ttu-id="05915-462">Ha hiba lép fel a háttér, az ügyfél SDK kivált egy `MobileServiceInvalidOperationException`.</span><span class="sxs-lookup"><span data-stu-id="05915-462">When an error occurs in the backend, the client SDK raises a `MobileServiceInvalidOperationException`.</span></span>  <span data-ttu-id="05915-463">A következő példa bemutatja, hogyan legyen kezelve az háttéralkalmazása által visszaadott kivétel:</span><span class="sxs-lookup"><span data-stu-id="05915-463">The following example shows how to handle an exception that is returned by the backend:</span></span>

```
private async void InsertTodoItem(TodoItem todoItem)
{
    // This code inserts a new TodoItem into the database. When the operation completes
    // and App Service has assigned an Id, the item is added to the CollectionView
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

<span data-ttu-id="05915-464">Egy másik példa való hibaállapotok itt található: a [Mobile Apps fájlok minta].</span><span class="sxs-lookup"><span data-stu-id="05915-464">Another example of dealing with error conditions can be found in the [Mobile Apps Files Sample].</span></span> <span data-ttu-id="05915-465">A [LoggingHandler] példa biztosít a naplózás delegált kezelő naplózni a háttérrendszer történik.</span><span class="sxs-lookup"><span data-stu-id="05915-465">The [LoggingHandler] example provides a logging delegate handler to log the requests being made to the backend.</span></span>

### <span data-ttu-id="05915-466"><a name="headers"></a>Hogyan: testreszabása kérelem fejlécei</span><span class="sxs-lookup"><span data-stu-id="05915-466"><a name="headers"></a>How to: Customize request headers</span></span>
<span data-ttu-id="05915-467">Az adott alkalmazást forgatókönyv támogatása érdekében szükség lehet a Mobile Apps-háttéralkalmazás kommunikáció testreszabása.</span><span class="sxs-lookup"><span data-stu-id="05915-467">To support your specific app scenario, you might need to customize communication with the Mobile App backend.</span></span> <span data-ttu-id="05915-468">Érdemes lehet például egy egyéni fejléc hozzáadását a kimenő kérelmek vagy is módosíthatja a válaszok állapotkódok.</span><span class="sxs-lookup"><span data-stu-id="05915-468">For example, you may want to add a custom header to every outgoing request or even change responses status codes.</span></span> <span data-ttu-id="05915-469">Használhat egyéni [DelegatingHandler], az alábbi példa szerint:</span><span class="sxs-lookup"><span data-stu-id="05915-469">You can use a custom [DelegatingHandler], as in the following example:</span></span>

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
        // Change the request-side here based on the HttpRequestMessage
        request.Headers.Add("x-my-header", "my value");

        // Do the request
        var response = await base.SendAsync(request, cancellationToken);

        // Change the response-side here based on the HttpResponseMessage

        // Return the modified response
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

<span data-ttu-id="05915-470">[hitelesítés alkalmazásokhoz történő hozzáadását]: app-service-mobile-windows-store-dotnet-get-started-users.md</span><span class="sxs-lookup"><span data-stu-id="05915-470">[Add authentication to your app]: app-service-mobile-windows-store-dotnet-get-started-users.md</span></span>
<span data-ttu-id="05915-471">[az Azure Mobile Apps Offline adatszinkronizálás]: app-service-mobile-offline-data-sync.md</span><span class="sxs-lookup"><span data-stu-id="05915-471">[Offline Data Sync in Azure Mobile Apps]: app-service-mobile-offline-data-sync.md</span></span>
<span data-ttu-id="05915-472">[leküldéses értesítések hozzáadása az alkalmazáshoz]: app-service-mobile-windows-store-dotnet-get-started-push.md</span><span class="sxs-lookup"><span data-stu-id="05915-472">[Add push notifications to your app]: app-service-mobile-windows-store-dotnet-get-started-push.md</span></span>
<span data-ttu-id="05915-473">[regisztrálja az alkalmazást egy Microsoft-fiók bejelentkezési használandó]: app-service-mobile-how-to-configure-microsoft-authentication.md</span><span class="sxs-lookup"><span data-stu-id="05915-473">[Register your app to use a Microsoft account login]: app-service-mobile-how-to-configure-microsoft-authentication.md</span></span>
<span data-ttu-id="05915-474">[App Service konfigurálása az Active Directory bejelentkezési]: app-service-mobile-how-to-configure-active-directory-authentication.md</span><span class="sxs-lookup"><span data-stu-id="05915-474">[How to configure App Service for Active Directory login]: app-service-mobile-how-to-configure-active-directory-authentication.md</span></span>

<!-- Microsoft URLs. -->
<span data-ttu-id="05915-475">[MobileServiceCollection]: https://msdn.microsoft.com/en-us/library/azure/dn250636(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="05915-475">[MobileServiceCollection]: https://msdn.microsoft.com/en-us/library/azure/dn250636(v=azure.10).aspx</span></span>
<span data-ttu-id="05915-476">[MobileServiceIncrementalLoadingCollection]: https://msdn.microsoft.com/en-us/library/azure/dn268408(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="05915-476">[MobileServiceIncrementalLoadingCollection]: https://msdn.microsoft.com/en-us/library/azure/dn268408(v=azure.10).aspx</span></span>
<span data-ttu-id="05915-477">[MobileServiceAuthenticationProvider]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="05915-477">[MobileServiceAuthenticationProvider]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider(v=azure.10).aspx</span></span>
<span data-ttu-id="05915-478">[MobileServiceUser]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="05915-478">[MobileServiceUser]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser(v=azure.10).aspx</span></span>
<span data-ttu-id="05915-479">[MobileServiceAuthenticationToken]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="05915-479">[MobileServiceAuthenticationToken]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken(v=azure.10).aspx</span></span>
<span data-ttu-id="05915-480">[GetTable]: https://msdn.microsoft.com/en-us/library/azure/jj554275(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="05915-480">[GetTable]: https://msdn.microsoft.com/en-us/library/azure/jj554275(v=azure.10).aspx</span></span>
<span data-ttu-id="05915-481">[hoz létre egy típusos táblára mutató hivatkozás]: https://msdn.microsoft.com/en-us/library/azure/jj554278(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="05915-481">[creates a reference to an untyped table]: https://msdn.microsoft.com/en-us/library/azure/jj554278(v=azure.10).aspx</span></span>
<span data-ttu-id="05915-482">[DeleteAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296407(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="05915-482">[DeleteAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296407(v=azure.10).aspx</span></span>
<span data-ttu-id="05915-483">[IncludeTotalCount]: https://msdn.microsoft.com/en-us/library/azure/dn250560(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="05915-483">[IncludeTotalCount]: https://msdn.microsoft.com/en-us/library/azure/dn250560(v=azure.10).aspx</span></span>
<span data-ttu-id="05915-484">[InsertAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296400(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="05915-484">[InsertAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296400(v=azure.10).aspx</span></span>
<span data-ttu-id="05915-485">[InvokeApiAsync]: https://msdn.microsoft.com/en-us/library/azure/dn268343(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="05915-485">[InvokeApiAsync]: https://msdn.microsoft.com/en-us/library/azure/dn268343(v=azure.10).aspx</span></span>
<span data-ttu-id="05915-486">[LoginAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296411(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="05915-486">[LoginAsync]: https://msdn.microsoft.com/en-us/library/azure/dn296411(v=azure.10).aspx</span></span>
<span data-ttu-id="05915-487">[LookupAsync]: https://msdn.microsoft.com/en-us/library/azure/jj871654(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="05915-487">[LookupAsync]: https://msdn.microsoft.com/en-us/library/azure/jj871654(v=azure.10).aspx</span></span>
<span data-ttu-id="05915-488">[OrderBy]: https://msdn.microsoft.com/en-us/library/azure/dn250572(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="05915-488">[OrderBy]: https://msdn.microsoft.com/en-us/library/azure/dn250572(v=azure.10).aspx</span></span>
<span data-ttu-id="05915-489">[OrderByDescending]: https://msdn.microsoft.com/en-us/library/azure/dn250568(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="05915-489">[OrderByDescending]: https://msdn.microsoft.com/en-us/library/azure/dn250568(v=azure.10).aspx</span></span>
<span data-ttu-id="05915-490">[ReadAsync]: https://msdn.microsoft.com/en-us/library/azure/mt691741(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="05915-490">[ReadAsync]: https://msdn.microsoft.com/en-us/library/azure/mt691741(v=azure.10).aspx</span></span>
<span data-ttu-id="05915-491">[érvénybe]: https://msdn.microsoft.com/en-us/library/azure/dn250574(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="05915-491">[Take]: https://msdn.microsoft.com/en-us/library/azure/dn250574(v=azure.10).aspx</span></span>
<span data-ttu-id="05915-492">[kiválasztása]: https://msdn.microsoft.com/en-us/library/azure/dn250569(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="05915-492">[Select]: https://msdn.microsoft.com/en-us/library/azure/dn250569(v=azure.10).aspx</span></span>
<span data-ttu-id="05915-493">[kihagyása]: https://msdn.microsoft.com/en-us/library/azure/dn250573(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="05915-493">[Skip]: https://msdn.microsoft.com/en-us/library/azure/dn250573(v=azure.10).aspx</span></span>
<span data-ttu-id="05915-494">[UpdateAsync]: https://msdn.microsoft.com/en-us/library/azure/dn250536.(v=azure.10)aspx</span><span class="sxs-lookup"><span data-stu-id="05915-494">[UpdateAsync]: https://msdn.microsoft.com/en-us/library/azure/dn250536.(v=azure.10)aspx</span></span>
<span data-ttu-id="05915-495">[Felhasználói azonosítóját]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="05915-495">[UserID]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid(v=azure.10).aspx</span></span>
<span data-ttu-id="05915-496">[ahol]: https://msdn.microsoft.com/en-us/library/azure/dn250579(v=azure.10).aspx</span><span class="sxs-lookup"><span data-stu-id="05915-496">[Where]: https://msdn.microsoft.com/en-us/library/azure/dn250579(v=azure.10).aspx</span></span>
<span data-ttu-id="05915-497">[Azure-portálon]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="05915-497">[Azure portal]: https://portal.azure.com/</span></span>
<span data-ttu-id="05915-498">[a klasszikus Azure portálon]: https://manage.windowsazure.com/</span><span class="sxs-lookup"><span data-stu-id="05915-498">[Azure classic portal]: https://manage.windowsazure.com/</span></span>
<span data-ttu-id="05915-499">[EnableQueryAttribute]: https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx</span><span class="sxs-lookup"><span data-stu-id="05915-499">[EnableQueryAttribute]: https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx</span></span>
<span data-ttu-id="05915-500">[Guid.NewGuid]: https://msdn.microsoft.com/en-us/library/system.guid.newguid(v=vs.110).aspx</span><span class="sxs-lookup"><span data-stu-id="05915-500">[Guid.NewGuid]: https://msdn.microsoft.com/en-us/library/system.guid.newguid(v=vs.110).aspx</span></span>
<span data-ttu-id="05915-501">[ISupportIncrementalLoading]: http://msdn.microsoft.com/library/windows/apps/Hh701916.aspx</span><span class="sxs-lookup"><span data-stu-id="05915-501">[ISupportIncrementalLoading]: http://msdn.microsoft.com/library/windows/apps/Hh701916.aspx</span></span>
<span data-ttu-id="05915-502">[Windows fejlesztői központ]: https://dev.windows.com/en-us/overview</span><span class="sxs-lookup"><span data-stu-id="05915-502">[Windows Dev Center]: https://dev.windows.com/en-us/overview</span></span>
<span data-ttu-id="05915-503">[DelegatingHandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx</span><span class="sxs-lookup"><span data-stu-id="05915-503">[DelegatingHandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx</span></span>
<span data-ttu-id="05915-504">[Windows Live SDK]: https://msdn.microsoft.com/en-us/library/bb404787.aspx</span><span class="sxs-lookup"><span data-stu-id="05915-504">[Windows Live SDK]: https://msdn.microsoft.com/en-us/library/bb404787.aspx</span></span>
<span data-ttu-id="05915-505">[PasswordVault]: http://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx</span><span class="sxs-lookup"><span data-stu-id="05915-505">[PasswordVault]: http://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx</span></span>
[ProtectedData]: http://msdn.microsoft.com/library/system.security.cryptography.protecteddata%28VS.95%29.aspx
<span data-ttu-id="05915-506">[Notification Hubs API-k]: https://msdn.microsoft.com/library/azure/dn495101.aspx</span><span class="sxs-lookup"><span data-stu-id="05915-506">[Notification Hubs APIs]: https://msdn.microsoft.com/library/azure/dn495101.aspx</span></span>
<span data-ttu-id="05915-507">[Mobile Apps fájlok minta]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files</span><span class="sxs-lookup"><span data-stu-id="05915-507">[Mobile Apps Files Sample]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files</span></span>
<span data-ttu-id="05915-508">[LoggingHandler]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files/blob/master/src/client/MobileAppsFilesSample/Helpers/LoggingHandler.cs#L63</span><span class="sxs-lookup"><span data-stu-id="05915-508">[LoggingHandler]: https://github.com/Azure-Samples/app-service-mobile-dotnet-todo-list-files/blob/master/src/client/MobileAppsFilesSample/Helpers/LoggingHandler.cs#L63</span></span>

<!-- External URLs -->
<span data-ttu-id="05915-509">[OData v3 dokumentáció]: http://www.odata.org/documentation/odata-version-3-0/</span><span class="sxs-lookup"><span data-stu-id="05915-509">[OData v3 Documentation]: http://www.odata.org/documentation/odata-version-3-0/</span></span>
<span data-ttu-id="05915-510">[Fiddler]: http://www.telerik.com/fiddler</span><span class="sxs-lookup"><span data-stu-id="05915-510">[Fiddler]: http://www.telerik.com/fiddler</span></span>
<span data-ttu-id="05915-511">[Json.NET]: http://www.newtonsoft.com/json</span><span class="sxs-lookup"><span data-stu-id="05915-511">[Json.NET]: http://www.newtonsoft.com/json</span></span>
<span data-ttu-id="05915-512">[Xamarin.Auth]: https://components.xamarin.com/view/xamarin.auth/</span><span class="sxs-lookup"><span data-stu-id="05915-512">[Xamarin.Auth]: https://components.xamarin.com/view/xamarin.auth/</span></span>
<span data-ttu-id="05915-513">[AuthStore.cs]: https://github.com/azure-appservice-samples/ContosoMoments</span><span class="sxs-lookup"><span data-stu-id="05915-513">[AuthStore.cs]: https://github.com/azure-appservice-samples/ContosoMoments</span></span>
[ContosoMoments photo sharing sample]: https://github.com/azure-appservice-samples/ContosoMoments
