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
# <a name="how-toouse-hello-managed-client-for-azure-mobile-apps"></a>Hogyan kezeli a toouse hello az Azure Mobile Apps-ügyfél
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

## <a name="overview"></a>Áttekintés
Ez az útmutató bemutatja, hogyan kezeli a tooperform szolgáltatást használó általános forgatókönyvhöz hello az ügyféloldali kódtára a Azure App Service Mobile Apps for Windows és a Xamarin-alkalmazásokat. Ha új tooMobile alkalmazások, érdemes lehet első épp hello [Azure Mobile Apps gyors üzembe helyezés] [ 1] oktatóanyag. Az útmutató azt összpontosítani hello ügyféloldali SDK kezeli. További részletek hello kiszolgálóoldali SDK-k a Mobile Apps, hello hello dokumentációjában toolearn [.NET SDK-kiszolgáló] [ 2] vagy a [Node.js Server SDK] [ 3].

## <a name="reference-documentation"></a>Segédanyagok
hello hello ügyfél SDK referenciadokumentációt tartalmaz a következő helyen található: [Azure Mobile Apps .NET ügyfél hivatkozási][4].
Hello is található több ügyfél mintát [Azure-minták GitHub-tárházban][5].

## <a name="supported-platforms"></a>A támogatott platformok
hello .NET Platform hello a következő platformokat támogatja:

* Xamarin Android API 19 kiadását – 24 (KitKat nugát keresztül)
* Xamarin iOS kiadását iOS 8.0-s és újabb verziók
* Az univerzális Windows Platform
* Windows Phone 8.1
* Windows Phone 8.0 kivételével a Silverlight alkalmazások részére

hello "server-folyamat" hitelesítési egy webes nézet jelenik meg a felhasználói felület hello használ.  Ha hello eszköz nem tudja toopresent webes nézet felhasználói Felületet, más hitelesítési módszert van szükség.  Ez az SDK így nem alkalmas figyelési típusú vagy hasonló módon korlátozott eszközök.

## <a name="setup"></a>A telepítő és Előfeltételek
Feltételezzük, hogy már létrehozott és közzétett mobilalkalmazás háttérprojekt, amely legalább egy olyan táblát tartalmaz.  A jelen témakörben használt hello kódban hello tábla nevű `TodoItem` és van-e a következő oszlopok hello: `Id`, `Text`, és `Complete`. Ez a táblázat hello ugyanaz a tábla létrehozása, amikor befejezte a [Azure Mobile Apps gyors üzembe helyezés][1].

hello megfelelő típusos ügyféloldali C# típus a következő osztály hello:

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

Hello [JsonPropertyAttribute] [ 6] használt toodefine hello van *PropertyName* hello ügyfél és a hello tábla mező közötti megfeleltetés.

Hogyan toocreate táblák a Mobile Apps-háttéralkalmazásának toolearn lásd: hello [.NET Server SDK témakörben] [ 7] vagy hello [Node.js Server SDK témakörben] [ 8] . Ha a mobilalkalmazás háttérrendszerének hozott létre a hello Azure portál használatával hello gyors üzembe helyezés, használhatja hello **könnyen táblák** hello beállítása [Azure-portálon].

### <a name="how-to-install-hello-managed-client-sdk-package"></a>Útmutató: telepítés hello felügyelt ügyfél SDK-csomagot
Használja a következő módszerek tooinstall hello hello felügyelt ügyfél SDK-csomagot a Mobile Apps [NuGet][9]:

* **A Visual Studio** kattintson jobb gombbal a projektre, kattintson a **NuGet-csomagok kezelése**, keressen a hello `Microsoft.Azure.Mobile.Client` csomagot, majd kattintson az **telepítése**.
* **Xamarin Studio** kattintson jobb gombbal a projektre, kattintson a **Hozzáadás** > **NuGet-csomagok hozzáadása**, keressen a hello `Microsoft.Azure.Mobile.Client `csomagot, majd kattintson a **hozzáadása Csomag**.

A fő tevékenységnél fájlban, ne feledje tooadd hello következő **használatával** utasítást:

```
using Microsoft.WindowsAzure.MobileServices;
```

### <a name="symbolsource"></a>Útmutató: a Visual Studio hibakeresési szimbólumok használata
hello szimbólumok hello Microsoft.Azure.Mobile névtér érhetők el a [SymbolSource][10].  Tekintse meg a toothe [SymbolSource utasításokat] [ 11] toointegrate SymbolSource a Visual Studio.

## <a name="create-client"></a>Hello Mobile Apps-ügyfél létrehozása
hello alábbi kód létrehoz hello [MobileServiceClient] [ 12] objektum, amely használt tooaccess a mobilalkalmazás háttérrendszerének.

```
var client = new MobileServiceClient("MOBILE_APP_URL");
```

Cserélje le a hello megelőző kódot, `MOBILE_APP_URL` hello URL-címével hello Mobile Apps-háttéralkalmazás, amely megtalálható a Mobile Apps-háttéralkalmazás hello a panel [Azure-portálon]. hello MobileServiceClient objektum egypéldányos kell lennie.

## <a name="work-with-tables"></a>A táblázatok használata
a következő szakasz hogyan hello toosearch lehívása és hello tábla hello adatainak módosítása.  hello a következő témakörök ismertetnek:

* [Egy tábla hivatkozás létrehozása](#instantiating)
* [Adatait kérdezi le.](#querying)
* [Visszaadott adatok szűrése](#filtering)
* [A rendezési adatokat adott vissza](#sorting)
* [A lapok visszatérési adatai](#paging)
* [Egyes oszlopok kiválasztásához](#selecting)
* [Kereshet meg egy olyan rekordot-azonosító szerint](#lookingup)
* [A típus nélküli lekérdezések kezelése](#untypedqueries)
* [Adatok beszúrása](#inserting)
* [Adatok frissítése](#updating)
* [Adatok törlése](#deleting)
* [Ütközések feloldása, és az egyidejű hozzáférések optimista](#optimisticconcurrency)
* [Kötési tooa Windows felhasználói felület](#binding)
* [Hello Oldalméret módosítása](#pagesize)

### <a name="instantiating"></a>Hogyan: tábla hivatkozás létrehozása
Az összes hello kód, amely fér hozzá vagy módosít egy háttér táblában funkciók meghívja a hello `MobileServiceTable` objektum. Szerezze be a referenciatábla toohello hívó hello [GetTable] módszert az alábbiak szerint:

```
IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
```

hello visszaadott objektum hello típusos szerializálási modellt használ. Az a típus nélküli szerializálás modell is támogatott. Az alábbi példa [táblázatot hoz létre hivatkozást tooan típusos]:

```
// Get an untyped table reference
IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");
```

A típus nélküli lekérdezések meg kell adnia az alapul szolgáló OData-lekérdezési karakterlánc hello.

### <a name="querying"></a>Útmutató: a mobilalkalmazás adatok lekérdezése
Ez a szakasz ismerteti, hogyan tooissue lekérdezi toohello Mobile Apps-háttéralkalmazás, amely tartalmazza a következő funkciók hello:

* [Visszaadott adatok szűrése](#filtering)
* [A rendezési adatokat adott vissza](#sorting)
* [A lapok visszatérési adatai](#paging)
* [Egyes oszlopok kiválasztásához](#selecting)
* [Adatok-azonosító szerint](#lookingup)

> [!NOTE]
> A kiszolgáló adatvezérelt lap mérete összes olyan sort ad vissza az érvényben lévő tooprevent.  Lapozófájl alapértelmezett kérelmek nagy adatkészletek tarthatja negatív befolyásolása hello szolgáltatást.  tooreturn több mint 50 sorok, használja a hello `Skip` és `Take` metódus, a [vissza adatokat a lapok](#paging).

### <a name="filtering"></a>Hogyan: szűrő adatokat adott vissza.
hello alábbi kód bemutatja, hogyan toofilter adatokat, beleértve a következőket a `Where` alzáradékában. Az összes elemet adja vissza `todoTable` amelynek `Complete` tulajdonság értéke túl`false`. Hello [ahol] függvény hello lekérdezéshez hello táblázaton predikátum szűrés sor vonatkozik.

```
// This query filters out completed TodoItems and items without a timestamp.
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .ToListAsync();
```

Üzenet hálózatfelügyeleti szoftverek, például a böngésző fejlesztői eszközök segítségével hello hello küldött kérelem toohello háttér URI Azonosítóját is megtekintheti vagy [Fiddler]. Ha megnézzük hello kérelem URI-azonosítója, figyelje meg, hogy van-e módosítva hello lekérdezési karakterlánc:

```
GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1
```

Az OData-kérelem hello Server SDK által lefordítását egy SQL-lekérdezést:

```
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
```

függvényen toohello hello `Where` metódus egy tetszőleges számú feltételek lehet.

```
// This query filters out completed TodoItems where Text isn't null
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false && todoItem.Text != null)
    .ToListAsync();
```

Ez a példa volna fordítható egy SQL-lekérdezést, hello Server SDK által:

```
SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0
```

Ez a lekérdezés több záradékot is osztható:

```
List<TodoItem> items = await todoTable
    .Where(todoItem => todoItem.Complete == false)
    .Where(todoItem => todoItem.Text != null)
    .ToListAsync();
```

hello két módszert, és szabadon felcserélhetők.  korábbi beállítás hello&mdash;hozzáfűzésével több predikátumok egy lekérdezést a&mdash;tömörebb és ajánlott.

Hello `Where` záradék támogatja a műveleteket, lehet lefordítani a hello OData részhalmaza. Műveletek a következők:

* Relációs operátorok (==,! =, <, < =, >, > =),
* Aritmetikai operátor (+, -, /, *, %),
* A pontosság (Math.Floor, Math.Ceiling), szám
* Karakterlánc (hossza, Substring, Replace, IndexOf, megadott módon kezdődő, megadott módon végződő),
* Tulajdonságok (év, hónap, nap, óra, perc másodperc), dátum
* Az objektum tulajdonságainak hozzáférést és
* A kifejezések kombinálásával egyik műveletet.

Ha figyelembe véve, hogy mi hello Server SDK támogatja, érdemes lehet a hello [OData v3 dokumentáció].

### <a name="sorting"></a>Hogyan: rendezési adatokat adott vissza.
hello alábbi kód bemutatja, hogyan toosort adatokat, beleértve a következőket egy [OrderBy] vagy [OrderByDescending] hello lekérdezésben. Adja vissza, a cikkek `todoTable` növekvő sorrend szerint hello `Text` mező.

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

### <a name="paging"></a>Hogyan: vissza adatokat lap
Alapértelmezés szerint hello háttér sorát adja vissza csak hello első 50. Így javítható hívó hello hello visszaadott sorok száma [érvénybe] metódust. Használjon `Take` együtt hello [kihagyása] metódus toorequest egy adott "lap" hello teljes adatkészlet hello lekérdezés által visszaadott. hello következő lekérdezés végrehajtásakor adja vissza hello a három legfontosabb elemek hello táblában.

```
// Define a filtered query that returns hello top 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Take(3);
List<TodoItem> items = await query.ToListAsync();
```

hello alábbi javított lekérdezés átugrása hello első három eredményeit, és értéket ad vissza a következő három eredmények hello. Ez a lekérdezés hello második "lap" az adatok, ahol hello lap mérete három elemek eredményez.

```
// Define a filtered query that skips hello top 3 items and returns hello next 3 items.
MobileServiceTableQuery<TodoItem> query = todoTable.Skip(3).Take(3);
List<TodoItem> items = await query.ToListAsync();
```

Hello [IncludeTotalCount] metódus kérelmek teljes száma hello *összes* azt jelzi, hogy akkor adott vissza, figyelmen kívül hagyja a lapozófájl/vonatkozó záradékban megadott hello:

```
query = query.IncludeTotalCount();
```

Egy valós alkalmazás használhat lekérdezések hasonló toohello személyhívó vezérlő vagy hasonló felhasználói felület – példa megelőző oldalai között.

> [!NOTE]
> toooverride hello soron kívüli 50-korlát a mobil-háttéralkalmazást, telepítenie kell az is hello [EnableQueryAttribute] toohello nyilvános GET metódus, és adja meg a hello lapozás viselkedését. Ha az alkalmazott toohello metódus, a következő hello beállítja a visszaadott sorok maximális száma too1000:
>
> `[EnableQuery(MaxTop=1000)]`


### <a name="selecting"></a>Hogyan: egyes oszlopok kiválasztásához
Megadhatja a hello tulajdonságok tooinclude körét eredmények hozzáadásával egy [válasszon] záradék tooyour lekérdezés. Például az a következő kód bemutatja hogyan hello tooselect csak egy mezőt, és hogyan is tooselect és több mező formázása:

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

Az összes hello funkciók leírása, amennyiben azok additívak, így azt is tartsa láncolás őket. Minden láncolt hívás több hello lekérdezés hatással van. Egy további példa:

```
MobileServiceTableQuery<TodoItem> query = todoTable
                .Where(todoItem => todoItem.Complete == false)
                .Select(todoItem => todoItem.Text)
                .Skip(3).
                .Take(3);
List<string> items = await query.ToListAsync();
```

### <a name="lookingup"></a>Útmutató: adatok-azonosító szerint
Hello [LookupAsync] függvény lehet használt toolook objektumainak hello adatbázis egy adott azonosítóval.

```
// This query filters out hello item with hello ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");
```

### <a name="untypedqueries"></a>Útmutató: a típus nélküli lekérdezések végrehajtása
A típus nélküli tábla objektum lekérdezést végrehajtásakor explicit módon meg kell hello OData-lekérdezési karakterlánc meghívásával [ReadAsync], a példában a következő hello:

```
// Lookup untyped data using OData
JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");
```

JSON értékekre, amelyek például a tulajdonságcsomag vissza. JToken és Newtonsoft Json.NET további információkért lásd: hello [Json.NET] hely.

### <a name="inserting"></a>Útmutató: a Mobile Apps-háttéralkalmazás adatok beszúrása
Minden ügyfél esetében tartalmaznia kell egy nevű tag **azonosító**, amely alapértelmezés szerint ki egy karakterláncot. Ez **azonosító** CRUD műveletek elvégzéséhez szükséges, és a kapcsolat nélküli szinkronizálás. hello a következő kód bemutatja, hogyan toouse hello [InsertAsync] metódus tooinsert új sorok egy táblába. hello paraméter hello adatok toobe .NET objektumként beszúrni tartalmazza.

```
await todoTable.InsertAsync(todoItem);
```

Ha az egyéni azonosító egyedi érték nem szerepel a hello `todoItem` során egy INSERT utasítás hello kiszolgáló által létrehozott GUID.
Kérheti le hello létrehozott azonosító hello objektum vizsgálatával vissza hello hívása után.

tooinsert adatok típus nélküli, Json.NET előnyeit is:

```
JObject jo = new JObject();
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

Íme egy példa egy e-mail cím használata egy egyedi karakterlánc-azonosító:

```
JObject jo = new JObject();
jo.Add("id", "myemail@emaildomain.com");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.InsertAsync(jo);
```

### <a name="working-with-id-values"></a>Azonosító értékek használata
Mobile Apps egyedi egyéni karakterlánc-értékek támogatja hello tábla **azonosító** oszlop. Egy karakterláncértéket lehetővé teszi alkalmazások toouse egyéni értékeket, például az e-mail címek vagy felhasználónevek hello azonosítóját.  Karakterlánc-azonosítóknak hello a következő előnyöket biztosítja:

* Azonosítók anélkül, hogy az oda-vissza toohello adatbázis jönnek létre.
* Rekordok könnyebb toomerge különböző táblákhoz vagy adatbázisok.
* Azonosítók értékek jobban integrálható egy alkalmazás logikáját.

Egy karakterláncértéket azonosítója nincs beállítva egy beszúrt rekordot, hello Mobile Apps-háttéralkalmazás hoz létre egy egyedi értéket a azonosítóját. Használhatja a hello [Guid.NewGuid] saját azonosító értéket, hello ügyfélen vagy a hello háttérbeli metódus toogenerate.

```
JObject jo = new JObject();
jo.Add("id", Guid.NewGuid().ToString("N"));
```

### <a name="modifying"></a>Útmutató: a Mobile Apps-háttéralkalmazás adatainak módosítása
hello alábbi kód bemutatja, hogyan toouse hello [UpdateAsync] metódus tooupdate hello egy meglévő rekord azonos azonosító új információkkal. hello paraméter hello adatok toobe frissíteni, mivel a .NET-objektum tartalmazza.

```
await todoTable.UpdateAsync(todoItem);
```

tooupdate típus nélküli adatokat, akkor lehetséges, hogy előnyeit [Json.NET] az alábbiak szerint:

```
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
jo.Add("Text", "Hello World");
jo.Add("Complete", false);
var inserted = await table.UpdateAsync(jo);
```

Egy `id` mezőt meg kell adni, ha egy frissítés. hello háttér használ hello `id` mező tooidentify, amely tooupdate sor. Hello `id` mező hello hello eredményét szerezhető `InsertAsync` hívható meg. Egy `ArgumentException` jelenik meg, ha egy elem tooupdate hello megadása nélkül próbálja `id` érték.

### <a name="deleting"></a>Útmutató: a Mobile Apps-háttéralkalmazás adatok törlése
hello alábbi kód bemutatja, hogyan toouse hello [DeleteAsync] metódus toodelete egy meglévő példányát. hello példány hello azonosíthatók `id` hello be mezőben `todoItem`.

```
await todoTable.DeleteAsync(todoItem);
```

toodelete típus nélküli adatokat, akkor előfordulhat, hogy előnyeit Json.NET az alábbiak szerint:

```
JObject jo = new JObject();
jo.Add("id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
await table.DeleteAsync(jo);
```

Ha elvégezte a törlési kérelmet, meg kell adni egy Azonosítót. Egyéb tulajdonságok nem átadott toohello szolgáltatás, vagy a hello szolgáltatás figyelmen kívül lesznek hagyva. hello eredménye egy `DeleteAsync` tekintendő, amely általában `null`. hello azonosító toopass a hello hello eredményét szerezhető `InsertAsync` hívható meg. A `MobileServiceInvalidOperationException` toodelete elem meg hello megadása nélkül történt `id` mező.

### <a name="optimisticconcurrency"></a>Hogyan: használata egyidejű hozzáférések optimista az ütközések feloldása
Két vagy több ügyfél írhat módosítások toohello azonos elem: hello azonos idő. Ütközésészlelés, nélkül hello utolsó írási felülírná korábbi frissítéseket. **Egyidejű hozzáférések optimista vezérlését** feltételezi, hogy az egyes tranzakciókra véglegesítheti, és ezért nem használhatja semmilyen erőforrás zárolását.  Előtt véglegesítése tranzakció, egyidejű hozzáférések optimista vezérlését ellenőrzi, hogy nincs másik tranzakció módosította hello adatok. Ha hello adatok módosítva lett, tranzakció véglegesítésekor hello vissza lesz állítva.

Mobile Apps egyidejű hozzáférések optimista vezérlését támogatja, módosításokat tooeach elem hello segítségével nyomon követésével `version` rendszer tulajdonság oszlop, amely minden táblában a Mobile Apps-háttéralkalmazás van definiálva. Minden alkalommal, amikor frissíti, a Mobile Apps beállítja a hello `version` tulajdonság az adott jegyezze tooa új értéket. Minden egyes frissítés kérelem során hello `version` hello rekord hello kérés részét képező tulajdonság értéke összehasonlított toohello hello rekord hello kiszolgáló ugyanahhoz a tulajdonsághoz. Ha átadni a verzió hello kérelem nem egyezik meg a hello háttér, majd riasztást hello ügyféloldali kódtára a `MobileServicePreconditionFailedException<T>` kivétel. hello hello kivétel mellékelt típus hello háttérrendszer tartalmazó hello kiszolgálók verzió hello rekord hello-bejegyzést. hello alkalmazás használhatja az információk toodecide e tooexecute hello frissítés kérést megfelelő hello `version` hello háttér toocommit módosítások közötti értéket.

Adja meg a hello tábla osztály hello az oszlop `version` rendszer tulajdonság tooenable optimista konkurencia. Példa:

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

A típus nélküli táblák használata kiszolgálóalkalmazások lehetővé teszik az hello beállítása egyidejű hozzáférések optimista `Version` a jelzőt a `SystemProperties` hello a tábla az alábbiak szerint.

```
//Enable optimistic concurrency by retrieving version
todoTable.SystemProperties |= MobileServiceSystemProperties.Version;
```

A hozzáadása tooenabling egyidejű hozzáférések optimista, meg kell is catch hello `MobileServicePreconditionFailedException<T>` a kódban meghívásakor kivétel [UpdateAsync].  Oldja fel a hello ütközést úgy, hogy alkalmazza a megfelelő hello `version` toothe frissített rekord és a hívás [UpdateAsync] a hello feloldotta a rekordot. a következő kód hello látható tooresolve egy írás ütközés egyszer észlelésének módját:

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

További információkért lásd: hello [az Azure Mobile Apps Offline adatszinkronizálás] témakör.

### <a name="binding"></a>Útmutató: a kötés Mobile Apps adatok tooa Windows felhasználói felület
Ez a szakasz bemutatja, hogyan toodisplay visszaadott adatok objektumok felhasználói felületi elemei használják a Windows-alkalmazás.  Az alábbi példakód toohello lista forrása a hello hiányos elemek lekérdezéssel van kötve. A [MobileServiceCollection] létrehoz egy Mobile Apps-kompatibilis kötés gyűjteményt.

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

Egyes vezérlők hello a felügyelt futásidejű támogatását illesztőfelület nevű [ISupportIncrementalLoading]. Ez az interfész lehetővé teszi, hogy a vezérlők toorequest hello felhasználói görgetésekor további adatokat. Ez az interfész keresztül univerzális Windows-alkalmazások beépített támogatása [MobileServiceIncrementalLoadingCollection], amely automatikusan kezeli a hívások a hello vezérlők. Használjon `MobileServiceIncrementalLoadingCollection` a Windows-alkalmazások az alábbiak szerint:

```
MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
items = todoTable.Where(todoItem => todoItem.Complete == false).ToIncrementalLoadingCollection();

ListBox lb = new ListBox();
lb.ItemsSource = items;
```

toouse hello új gyűjtemény a Windows Phone 8 és a "Silverlight" alkalmazásokra, használjon hello `ToCollection` a kiterjesztésmetódusok `IMobileServiceTableQuery<T>` és `IMobileServiceTable<T>`. tooload adatok hívás `LoadMoreItemsAsync()`.

```
MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
await items.LoadMoreItemsAsync();
```

Meghívásával létrehozott hello gyűjtemény használatakor `ToCollectionAsync` vagy `ToCollection`, kap egy gyűjteményt, amely lehet kötött tooUI szabályozza.  Ez a gyűjtemény a lapozófájl-kompatibilis.  Hello gyűjtemény adatokat tölt be a hálózaton, mivel néha betöltése sikertelen. toohandle ilyen hibák felülbírálása hello `OnException` metódusa `MobileServiceIncrementalLoadingCollection` hívások túl eredő toohandle kivételek`LoadMoreItemsAsync`.

Vegye figyelembe, ha a tábla sok mezőt tartalmaz, de azt szeretné, toodisplay némelyikük a vezérlőben. Hello útmutatást használhatja az előző szakaszban hello "[egyes oszlopok kiválasztásához](#selecting)" tooselect adott oszlopok toodisplay a felhasználói felület hello.

### <a name="pagesize"></a>Oldalméret módosítása hello
Az Azure Mobile Apps egy legfeljebb 50 elemet kérelmenként alapértelmezés szerint adja vissza.  Hello a lapozófájl méretének növelésével hello maximális lapméretét hello ügyfél és kiszolgáló egyaránt a módosíthatja.  tooincrease hello kért oldalméret, adja meg `PullOptions` használatakor `PullAsync()`:

```
PullOptions pullOptions = new PullOptions
    {
        MaxPageSize = 100
    };
```

Feltéve, hogy hello végrehajtott `PageSize` egyenlő tooor hello kiszolgálón belül 100-nál nagyobb, a kérelem legfeljebb 100 elemet adja vissza.

## <a name="#offlinesync"></a>A kapcsolat nélküli táblázatok használata
Kapcsolat nélküli táblák egy helyi SQLite toostore adat tárolása használ, amikor a kapcsolat nélküli használata.  Minden tábla elleni hello végzett műveletek helyi SQLite helyett hello távoli kiszolgáló tárolójában tárolja.  egy kapcsolat nélküli tábla toocreate előkészítenie a projekthez:

1. A Visual Studióban, kattintson a jobb gombbal hello megoldás > **NuGet-csomagok kezelése megoldáshoz...** , majd keresse meg, és telepítse a **Microsoft.Azure.Mobile.Client.SQLiteStore** projektek hello megoldás NuGet-csomagot.
2. (Választható) toosupport Windows-eszközök esetében egy SQLite futtatókörnyezetek következő hello telepíthető:

   * **Windows 8.1 futásidejű:** telepítése [a Windows 8.1 SQLite][3].
   * **Windows Phone 8.1:** telepítése [a Windows Phone 8.1 SQLite][4].
   * **Univerzális Windows Platform** telepítése [az univerzális Windows hello SQLite][5].
3. (Nem kötelező). Windows-eszközökhöz, kattintson a **hivatkozások** > **hivatkozás hozzáadása...** , bontsa ki a hello **Windows** mappa > **bővítmények**, engedélyeznie kell a megfelelő hello **SQLite for Windows** SDK hello együtt **Windows Visual C++ 2013 futásidejű** SDK.
    hello SQLite SDK nevek függően eltérőek az egyes Windows-platformra.

Egy tábla hivatkozás létrehozása előtt hello helyi tárolóban kell készíteni:

```
var store = new MobileServiceSQLiteStore(Constants.OfflineDbPath);
store.DefineTable<TodoItem>();

//Initializes hello SyncContext using hello default IMobileServiceSyncHandler.
await this.client.SyncContext.InitializeAsync(store);
```

Tanúsítványtár inicializálásához hello ügyfél létrehozása után azonnal normál módon történik.  Hello **OfflineDbPath** használja az Ön által támogatott összes platformon megfelelő legyen a fájl nevét.  Ha hello elérési út egy teljes mértékben minősített elérési út (Ez azt jelenti, hogy kezdődik perjellel), majd, hogy az elérési utat használja.  Ha hello elérési út nem teljesen minősített, hello fájl egy platformspecifikus helyre kerül.

* Az iOS és Android-eszközök esetében a hello alapértelmezett elérési út hello "Személyes fájlok" mappa.
* Windows-eszközök esetén a hello alapértelmezett elérési út hello alkalmazásspecifikus "AppData" mappa.

Egy táblahivatkozás hello szerezhetők `GetSyncTable<>` módszert:

```
var table = client.GetSyncTable<TodoItem>();
```

Nem kell tooauthenticate toouse egy offline tábla.  Csak akkor kell tooauthenticate amikor hello háttérszolgáltatást kommunikációra.

### <a name="syncoffline"></a>Egy kapcsolat nélküli tábla szinkronizálása
Kapcsolat nélküli táblák nincsenek szinkronizálva a hello háttéralkalmazással alapértelmezés szerint.  Szinkronizálás két részre van osztva.  Töltsön le új elemek külön-külön tolható módosításokat.  Íme egy jellegzetes szinkronizálási módszere:

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

Ha első argumentum túl hello`PullAsync` null értékű, akkor nem használja a növekményes szinkronizálás.  Minden egyes szinkronizálási műveletet lekéri az összes rekordot.

hello SDK hajt végre egy implicit `PushAsync()` előtt az rekord.

Kezelési ütközésben történik, az egy `PullAsync()` metódust.  Akkor is foglalkozik hello ütközéseket azonos módon online táblák.  hello ütközés hozzák amikor `PullAsync()` során hello insert, update vagy delete neve helyett. Több ütközés fordulhat elő, ha azok egyetlen MobileServicePushFailedException be vannak becsomagolva.  Minden egyes hiba külön kezelni.

## <a name="#customapi"></a>Egy egyéni API használata
Egy egyéni API lehetővé teszi a toodefine egyéni végpontokat visszaállítását kiszolgálói funkciók nem leképezése tooan insert, update, törlése vagy olvasási művelete. Egy egyéni API használatával is befolyásolni további üzenetküldés, beleértve az olvasási és HTTP-üzenet fejlécek beállítása meghatározó JSON nem üzenet törzsének formátumban.

Egy egyéni API hívása hello egyik [InvokeApiAsync] módszerek hello ügyfélen. Például a következő kódsort hello küld egy POST kérést toohello **completeAll** hello háttérkiszolgálón API:

```
var result = await client.InvokeApiAsync<MarkAllResult>("completeAll", System.Net.Http.HttpMethod.Post, null);
```

Ezt az űrlapot típusos metódusát, és megköveteli, hogy hello **MarkAllResult** visszatérési, van definiálva típus. Típusos és a nem típusos metódusok támogatottak.

hello InvokeApiAsync() metódus lefoglalja/api/toohello API toocall kívánja, kivéve, ha hello API kezdődik-e a "/".
Példa:

* `InvokeApiAsync("completeAll",...)`meghívja a /api/completeAll hello háttér
* `InvokeApiAsync("/.auth/me",...)`meghívja a /.auth/me hello háttér

InvokeApiAsync toocall bármely WebAPI, beleértve a nem meghatározott e WebAPIs az Azure Mobile Apps használhatja.  InvokeApiAsync() használatakor hello megfelelő fejlécek, beleértve a hitelesítési fejléceket, hello kérelem küldése.

## <a name="authentication"></a>Felhasználók hitelesítéséhez
Mobile Apps hitelesítése és engedélyezése a felhasználók alkalmazás különböző külső Identitásszolgáltatók támogatja: Facebook, a Google, a Microsoft Account, a Twitter és az Azure Active Directory. Beállíthatja engedélyeit a táblák toorestrict hozzáférési megadott műveletek tooonly hitelesített felhasználók. Használhatja a hitelesített felhasználók tooimplement az engedélyezési szabályok hello identitás server parancsfájlokban. További információkért lásd: hello oktatóanyag [Add authentication tooyour alkalmazás].

Két hitelesítési forgalom támogatottak: *ügyfél által felügyelt* és *kiszolgáló által kezelt* folyamata. hello kiszolgáló által felügyelt folyamat nyújt hello legegyszerűbb hitelesítési élmény hello szolgáltató webes hitelesítés felület támaszkodnak. hello ügyfél által felügyelt folyamat lehetővé teszi a eszközspecifikus képességekkel szorosabb integrációt, a szolgáltatói eszközspecifikus SDK-k támaszkodnak.

> [!NOTE]
> Az éles alkalmazásokban lévő ügyfél által felügyelt folyamat használatát javasoljuk.

hitelesítés tooset, regisztrálnia kell az alkalmazás egy vagy több identitás-szolgáltatóktól.  hello identitásszolgáltató egy ügyfél-Azonosítót és az alkalmazás egy ügyfélkulcsot hoz létre.  A háttérrendszer tooenable Azure App Service hitelesítés/engedélyezés ezután állítsa be a ezeket az értékeket.  További információért hajtsa végre a hello részletes utasításokat az oktatóprogram [Add authentication tooyour alkalmazás].

Ez a szakasz hello a következő témakörök ismertetnek:

* [Ügyfél által felügyelt hitelesítés](#clientflow)
* [A hitelesítési kiszolgáló által felügyelt](#serverflow)
* [Gyorsítótárazás hello hitelesítési jogkivonata](#caching)

### <a name="clientflow"></a>Ügyfél által felügyelt hitelesítés
Az alkalmazás is egymástól függetlenül forduljon hello identitásszolgáltató és adja meg hello visszaküldött jogkivonat bejelentkezés során a fájlok. Az ügyfél folyamata lehetővé teszi tooprovide egy egyszeri bejelentkezést a felhasználók vagy a hello identitásszolgáltató tooretrieve további felhasználó adatait. Ügyfél folyamat hitelesítés az előnyben részesített toousing a kiszolgáló folyamata, hello identitásszolgáltató SDK több natív UX abba biztosít, és lehetővé teszi, hogy további testreszabási.

Példák a következő ügyfél-folyamat hitelesítési minták hello áll rendelkezésre:

* [Az Active Directory hitelesítési könyvtár](#adal)
* [Facebook-on vagy a Google](#client-facebook)
* [Élő SDK](#client-livesdk)

#### <a name="adal"></a>Hitelesíti a felhasználókat az Active Directory Authentication Library hello
Hello Active Directory Authentication Library (ADAL) tooinitiate felhasználói hitelesítés az Azure Active Directory-hitelesítéssel hello ügyféltől is használhatja.

1. A mobil-háttéralkalmazás az aad-ben bejelentkezés konfigurálása következő hello [hogyan tooconfigure App Service az Active Directory bejelentkezési] oktatóanyag. Győződjön meg arról, hogy toocomplete hello opcionális lépés egy natív ügyfélalkalmazás regisztrációján.
2. A Visual Studio és Xamarin Studio, nyissa meg a projektet, és adja hozzá egy hivatkozást toothe `Microsoft.IdentityModel.CLients.ActiveDirectory` NuGet-csomagot. Ha keres, vegye fel az előzetes verzióját.
3. Adja hozzá a következő kód tooyour alkalmazás, toohello platformra szerint hello. Minden győződjön meg a következő cserékhez hello:

   * Cserélje le **INSERT-SZOLGÁLTATÓ-Itt** hello nevű hello bérlő, amelyben az alkalmazás kiépítve. A következő formátumban kell megadni https://login.microsoftonline.com/contoso.onmicrosoft.com. Ez az érték lehet másolni az Azure Active Directory hello tartományi lapfülén hello [a klasszikus Azure portálon].
   * Cserélje le **INSERT-erőforrás-azonosító-Itt** hello ügyfél-azonosítójú a mobil-háttéralkalmazást. Ügyfél-azonosító hello szerezhet be a hello **speciális** lap **Azure Active Directory beállításai** hello portálon.
   * Cserélje le **INSERT-ügyfél-azonosító-Itt** hello ügyfél-azonosítójú hello natív ügyfélalkalmazás másolta.
   * Cserélje le **INSERT-REDIRECT-URI-Itt** a hellyel való */.auth/login/done* végpont hello HTTPS protokollt használ. Ez az érték túl hasonló legyen*https://contoso.azurewebsites.net/.auth/login/done*.

     az egyes platformokon szükséges hello kódot a következőképpen:

     **Windows:**

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

     **Xamarin.iOS**

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

     **Xamarin.Android**

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

#### <a name="client-facebook"></a>Egyszeri bejelentkezés Facebook-on vagy a Google származó jogkivonat használatával
Ahogy az a kódrészletet a Facebook-on vagy a Google hello ügyfél folyamatában használhatja.

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

#### <a name="client-livesdk"></a>Az egyszeri bejelentkezés a Microsoft-Account hello Live SDK
tooauthenticate felhasználók regisztrálnia kell a Microsoft-fiók fejlesztői központ hello az alkalmazást. A regisztrációs adatokat konfigurálja a mobilalkalmazás háttérrendszerének. toocreate Microsoft fiók regisztrálása, és csatlakoztassa tooyour mobil-háttéralkalmazást, teljes hello szükséges lépések [regisztrálja az alkalmazást toouse egy Microsoft-fiók bejelentkezési]. Ha az alkalmazás Windows áruház és a Windows Phone 8/Silverlight verzióit, először regisztráljon hello Windows Áruházbeli verziójához.

hello alábbira Live SDK használatával és hello által visszaadott token toosign tooyour Mobile Apps-háttéralkalmazás használja.

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

További információkért lásd: hello [Windows Live SDK] dokumentációját.

### <a name="serverflow"></a>A hitelesítési kiszolgáló által felügyelt
Miután regisztrálta az identitásszolgáltató, hívja hello [LoginAsync] hello [MobileServiceClient] hello rendelkező metódust [MobileServiceAuthenticationProvider] a szolgáltató érték. Például hello alábbira indít el a kiszolgáló folyamata bejelentkezés Facebook-fiókkal.

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

Ha használja az identitásszolgáltató Facebook-on kívül, hello értékének módosítása [MobileServiceAuthenticationProvider] toohello értékét a szolgáltatónál.

Egy kiszolgáló folyamatában Azure App Service hello bejelentkezési oldal hello kijelölt szolgáltató megjelenítésével kezeli a hello OAuth hitelesítési folyamat.  Egyszer hello identitás-szolgáltató értéket ad eredményül, Azure App Service egy App Service hitelesítési jogkivonatot állít elő. Hello [LoginAsync] metódus értéket ad vissza egy [MobileServiceUser], amely biztosítja, hogy mindkét hello [UserId] hello a hitelesített felhasználó és hello [ MobileServiceAuthenticationToken], mint egy JSON webes jogkivonatot (JWT). Ez a token gyorsítótárazható, és újra felhasználható, amíg le nem jár. További információkért lásd: [gyorsítótárazását hello hitelesítésére szolgáló jogkivonat](#caching).

### <a name="caching"></a>Gyorsítótárazás hello hitelesítési jogkivonata
Bizonyos esetekben bejelentkezési toohello hello telefonhívás elkerülhető hello első sikeres hitelesítés után hello hitelesítésére szolgáló jogkivonat hello szolgáltató elhelyezésével.  Windows áruház és az UWP-alkalmazások használhatják [PasswordVault] toocache a jelenlegi hitelesítési lexikális elem után egy sikeres bejelentkezés, az alábbiak szerint:

```
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook);

PasswordVault vault = new PasswordVault();
vault.Add(new PasswordCredential("Facebook", client.currentUser.UserId,
    client.currentUser.MobileServiceAuthenticationToken));
```

hello UserId érték hello felhasználónév a hello hitelesítő adatok tárolása és hello lexikális elem hello hello jelszó tárolja. A következő kezdő vállalkozások számára, ellenőrizheti a hello **PasswordVault** a gyorsítótárazott hitelesítő adatokat. hello következő példában gyorsítótárazott hitelesítő adatokat, ha találhatók, valamint egyéb próbálja újra a hello háttéralkalmazással tooauthenticate:

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

Amikor a felhasználó kijelentkezik, el kell távolítani a hello tárolt hitelesítő adatai, az alábbiak szerint:

```
client.Logout();
vault.Remove(vault.Retrieve("Facebook", client.currentUser.UserId));
```

Xamarin-alkalmazásokat használni hello [Xamarin.Auth] API-k toosecurely adattárolóhoz használandó hitelesítő adatok a egy **fiók** objektum. Ezen API-k használatának példája, lásd: hello [AuthStore.cs] hello a kódfájl [minta megosztása ContosoMoments fénykép](https://github.com/azure-appservice-samples/ContosoMoments).

Ügyfél által felügyelt hitelesítés használatakor például a Facebook- vagy Twitter szolgáltatótól kapott jogkivonat hello is képes gyorsítótárazni. Ez a token lehet megadott toorequest egy új hitelesítési jogkivonat hello háttérrendszerből, az alábbiak szerint:

```
var token = new JObject();
// Replace <your_access_token_value> with actual value of your access token
token.Add("access_token", "<your_access_token_value>");

// Authenticate using hello access token.
await client.LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
```

## <a name="pushnotifications"></a>Leküldéses értesítések
a következő témakörök borítóján leküldéses értesítések hello:

* [A leküldéses értesítések regisztrálása](#register-for-push)
* [Szerezzen be egy Windows Áruházbeli csomag biztonsági azonosítója](#package-sid)
* [Platformok közötti sablonok regisztrálása](#register-xplat)

### <a name="register-for-push"></a>Útmutató: a leküldéses értesítések regisztrálása
hello Mobile Apps-ügyfél lehetővé teszi az Azure Notification Hubs leküldéses értesítések tooregister. Amikor regisztrál, a hello platform-specifikus származó leírót igényelni Push Notification szolgáltatás (PNS). Azt adja meg ezt az értéket a címkékkel együtt hello regisztrációs létrehozásakor. hello alábbira regisztrál a Windows-alkalmazást a leküldéses értesítések hello Windows értesítési szolgáltatása (WNS):

```
private async void InitNotificationsAsync()
{
    // Request a push notification channel.
    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // Register for notifications using hello new channel.
    await MobileService.GetPush().RegisterNativeAsync(channel.Uri, null);
}
```

Ha tooWNS leküldendő, akkor meg kell [Windows áruház csomagot SID](#package-sid).  További információ a Windows-alkalmazásokra beleértve a hogyan sablon regisztrációkhoz tooregister: [Hozzáadás leküldéses értesítések tooyour app].

Címkék kér hello ügyfél nem támogatott.  A regisztrációs csendes eldobott címke kérelmek.
Ha a tooregister a címkékkel rendelkező eszköz, hozzon létre egy egyéni API-t használó hello Notification Hubs API tooperform hello regisztrációs az Ön nevében.  [Hello egyéni API hívása](#customapi) helyett hello `RegisterNativeAsync()` metódust.

### <a name="package-sid"></a>Hogyan: szerezze be a Windows áruház csomag biztonsági azonosítója
A Windows Áruházbeli alkalmazások leküldéses értesítések engedélyezése a csomag biztonsági azonosítója szükséges.  a csomag biztonsági azonosítója, tooreceive hello Windows áruház regisztrálhatja alkalmazását.

tooobtain ezt az értéket:

1. A Visual Studio Solution Explorerben, kattintson a jobb gombbal hello Windows Áruházbeli alkalmazás projektjére, kattintson a **tároló** > **hello Áruházbeli alkalmazás társítása...** .
2. Hello varázslóban kattintson **következő**, jelentkezzen be Microsoft-fiókjával, adjon meg egy nevet az alkalmazáshoz a **lefoglalni egy új alkalmazás neve**, majd kattintson a **tartalék**.
3. Sikeresen létrejött, jelölje be hello alkalmazásnév hello alkalmazás regisztrálása után kattintson **következő**, és kattintson a **társítása**.
4. Jelentkezzen be toohello [Windows fejlesztői központ] a Microsoft Account. A **alkalmazásaimat**, kattintson a létrehozott hello alkalmazás regisztrációja.
5. Kattintson a **Alkalmazáskezelés** > **identitását**, majd görgessen lefelé toofind a **CSOMAGAZONOSÍTÓT**.

Számos felhasználási hello csomag biztonsági azonosítója kezelni egy URI-azonosítóként, ebben az esetben szüksége toouse *ms-app: / /* hello sémát. Jegyezze meg a csomag biztonsági azonosítója, ez az érték előtagjaként hozzáfűzésével megfelelő hello verzióját.

Xamarin-alkalmazásokba szükséges néhány további kódrészletet toobe képes tooregister hello iOS vagy Android rendszereken futó alkalmazások. További információkért lásd: a platformra hello témakör:

* [Xamarin.Android](app-service-mobile-xamarin-android-get-started-push.md#add-push)
* [Xamarin.iOS](app-service-mobile-xamarin-ios-get-started-push.md#add-push-notifications-to-your-app)

### <a name="register-xplat"></a>Útmutató: regisztráció leküldéses sablonok toosend platformfüggetlen értesítések
tooregister sablonok használata hello `RegisterAsync()` metódus hello sablonok, az alábbiak szerint:

```
JObject templates = myTemplates();
MobileService.GetPush().RegisterAsync(channel.Uri, templates);
```

A sablonok kell `JObject` meg kell adnia, és tartalmazhat több sablonok a következő JSON formátummal hello:

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

hello metódus **RegisterAsync()** másodlagos Csempéket is fogad:

```
MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);
```

Található összes kódcímkének számítógépnél tisztító vannak a biztonsági regisztrálása során. tooadd címkéket tooinstallations vagy sablonok telepítések belül, lásd: [együttműködnek a háttérkiszolgálón a hello .NET SDK az Azure Mobile Apps].

toosend értesítések ezen regisztrált sablonok használatával tekintse meg a toohello [Notification Hubs API-k].

## <a name="misc"></a>Vegyes kapcsolatos témakörök
### <a name="errors"></a>Hogyan: hibák kezelésének
Ha hiba lép fel hello háttér, hello ügyfél SDK kivált egy `MobileServiceInvalidOperationException`.  Az alábbi példában látható hogyan toohandle hello háttéralkalmazása által visszaadott kivétel:

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

Egy másik példa való hibaállapotok hello található [Mobile Apps fájlok minta]. A [LoggingHandler] példa biztosít egy naplózási delegált kezelő toolog hello kérelmek toohello háttér végeznek.

### <a name="headers"></a>Hogyan: testreszabása kérelem fejlécei
toosupport az adott alkalmazást esetben szüksége lehet a Mobile Apps-háttéralkalmazás hello toocustomize kommunikál. Például előfordulhat, hogy szeretné, hogy egy egyéni fejlécet tooevery kimenő kérelem tooadd, vagy még akkor is módosíthatja a válaszok állapotkódok. Használhat egyéni [DelegatingHandler], a példában a következő hello:

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
