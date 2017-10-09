## <a name="create-client"></a>Ügyfélkapcsolat létrehozása
Hozzon létre egy ügyfélkapcsolatot egy `WindowsAzure.MobileServiceClient` objektum létrehozásával.  Cserélje le `appUrl` az az URL-cím tooyour mobilalkalmazás.

```
var client = WindowsAzure.MobileServiceClient(appUrl);
```

## <a name="table-reference"></a>Táblázatok használata
tooaccess vagy frissítés adatokat, a hivatkozás toohello háttér tábla létrehozása. Cserélje le `tableName` hello nevű a táblázat

```
var table = client.getTable(tableName);
```

Ha létrehozta a táblahivatkozást, további műveleteket végezhet a táblával:

* [Tábla lekérdezése](#querying)
  * [Adatok szűrése](#table-filter)
  * [Adatok lapozása](#table-paging)
  * [Adatok rendezése](#sorting-data)
* [Adatok beszúrása](#inserting)
* [Adatok módosítása](#modifying)
* [Adatok törlése](#deleting)

### <a name="querying"></a>Útmutató: Táblahivatkozás lekérdezése
Miután egy táblahivatkozás, használhatja azt tooquery hello kiszolgálón tárolt adatok.  A lekérdezések egy, a LINQ-hez hasonló nyelv használatával végezhetőek el.
tooreturn hello táblából, a következő használatát hello minden adat kód:

```
/**
 * Process hello results that are received by a call tootable.read()
 *
 * @param {Object} results hello results as a pseudo-array
 * @param {int} results.length hello length of hello results array
 * @param {Object} results[] hello individual results
 */
function success(results) {
   var numItemsRead = results.length;

   for (var i = 0 ; i < results.length ; i++) {
       var row = results[i];
       // Each row is an object - hello properties are hello columns
   }
}

function failure(error) {
    throw new Error('Error loading data: ', error);
}

table
    .read()
    .then(success, failure);
```

hello sikeres függvény hívása esetén hello eredményekkel.  Ne használjon `for (var i in results)` hello sikeres a kívánt módon, hogy fog ismétlés hello eredmények szereplő információk amikor más lekérdezési funkciók (például `.includeTotalCount()`) használt.

A lekérdezés szintaxisa hello további információkért lásd: hello [lekérdezésekre vonatkozó dokumentációt objektum].

#### <a name="table-filter"></a>Hello kiszolgálón lévő adatok szűrése
Használhatja a `where` záradék található hello táblahivatkozás:

```
table
    .where({ userId: user.userId, complete: false })
    .read()
    .then(success, failure);
```

Szűrő hello objektum függvény is használható.  Ebben az esetben hello `this` változó hozzá van rendelve a szűrni kívánt toothe aktuális objektum.  a következő kód hello funkcionális szempontból egyenértékű toohello a korábbi példában látható:

```
function filterByUserId(currentUserId) {
    return this.userId === currentUserId && this.complete === false;
}

table
    .where(filterByUserId, user.userId)
    .read()
    .then(success, failure);
```

#### <a name="table-paging"></a>Adatok lapozása
Hello használata `take()` és `skip()` módszerek.  Ha például 100 soron kívüli bejegyzésekbe toosplit hello tábla kívánja:

```
var totalCount = 0, pages = 0;

// Step 1 - get hello total number of records
table.includeTotalCount().take(0).read(function (results) {
    totalCount = results.totalCount;
    pages = Math.floor(totalCount/100) + 1;
    loadPage(0);
}, failure);

function loadPage(pageNum) {
    let skip = pageNum * 100;
    table.skip(skip).take(100).read(function (results) {
        for (var i = 0 ; i < results.length ; i++) {
            var row = results[i];
            // Process each row
        }
    }
}
```

Hello `.includeTotalCount()` metódus használt tooadd egy totalCount mező toohello eredmények objektum.  A totalCount mezőben azt jelzi, hogy nincs lapozás használata vissza hello száma megjelenik.

Ezután használhatja hello lapok változó és néhány UI gombokat tooprovide egy lista; használjon `loadPage()` hello új rögzíti az összes lapon betöltése.  Már be van töltve gyorsítótárazási toospeed hozzáférés toorecords megvalósításához.

#### <a name="sorting-data"></a>Útmutató: Rendezett adatok visszaadása
Használjon hello `.orderBy()` vagy `.orderByDescending()` lekérdezési módszerek:

```
table
    .orderBy('name')
    .read()
    .then(success, failure);
```

Hello lekérdezés objektumon további információkért lásd: hello [lekérdezésekre vonatkozó dokumentációt objektum].

### <a name="inserting"></a>Útmutató: Adatok beszúrása
Hozzon létre egy JavaScript objektum hello megfelelő dátumot és hívás `table.insert()` aszinkron módon:

```javascript
var newItem = {
    name: 'My Name',
    signupDate: new Date()
};

table
    .insert(newItem)
    .done(function (insertedItem) {
        var id = insertedItem.id;
    }, failure);
```

A sikeres beszúrási hello beszúrt elem mezőkkel hello további szinkronizálási műveletek szükséges adja vissza.  Frissítse ezekkel az információkkal a gyorsítótárat a későbbi frissítésekhez.

hello Azure Mobile Apps Node.js Server SDK támogatja a dinamikus séma fejlesztési célokra szolgál.  Dinamikus séma lehetővé teszi tooadd oszlopok toohello tábla insert vagy az update művelet megadásával.  Azt javasoljuk, hogy kikapcsolja dinamikus séma az alkalmazás tooproduction áthelyezése előtt.

### <a name="modifying"></a>Útmutató: Adatok módosítása
Hasonló toohello `.insert()` módszer, kell létrehozni egy frissítés objektumot és hívhatja `.update()`.  hello frissítés objektumnak tartalmaznia kell hello azonosítója hello rekord toobe frissítése – hello azonosító egységekre hello rekord olvasásakor, vagy meghívásakor `.insert()`.

```javascript
var updateItem = {
    id: '7163bc7a-70b2-4dde-98e9-8818969611bd',
    name: 'My New Name'
};

table
    .update(updateItem)
    .done(function (updatedItem) {
        // You can now update your cached copy
    }, failure);
```

### <a name="deleting"></a>Útmutató: Adatok törlése
toodelete rekord, hívás hello `.del()` metódust.  Adjon át hello azonosító objektumhivatkozás:

```
table
    .del({ id: '7163bc7a-70b2-4dde-98e9-8818969611bd' })
    .done(function () {
        // Record is now deleted - update your cache
    }, failure);
```
