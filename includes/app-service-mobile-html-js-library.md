## <span data-ttu-id="64d2f-101"><a name="create-client"></a>Ügyfélkapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="64d2f-101"><a name="create-client"></a>Create a client connection</span></span>
<span data-ttu-id="64d2f-102">Hozzon létre egy ügyfélkapcsolatot egy `WindowsAzure.MobileServiceClient` objektum létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="64d2f-102">Create a client connection by creating a `WindowsAzure.MobileServiceClient` object.</span></span>  <span data-ttu-id="64d2f-103">Cserélje le `appUrl` az az URL-cím tooyour mobilalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="64d2f-103">Replace `appUrl` with the URL tooyour Mobile App.</span></span>

```
var client = WindowsAzure.MobileServiceClient(appUrl);
```

## <span data-ttu-id="64d2f-104"><a name="table-reference"></a>Táblázatok használata</span><span class="sxs-lookup"><span data-stu-id="64d2f-104"><a name="table-reference"></a>Work with tables</span></span>
<span data-ttu-id="64d2f-105">tooaccess vagy frissítés adatokat, a hivatkozás toohello háttér tábla létrehozása.</span><span class="sxs-lookup"><span data-stu-id="64d2f-105">tooaccess or update data, create a reference toohello backend table.</span></span> <span data-ttu-id="64d2f-106">Cserélje le `tableName` hello nevű a táblázat</span><span class="sxs-lookup"><span data-stu-id="64d2f-106">Replace `tableName` with hello name of your table</span></span>

```
var table = client.getTable(tableName);
```

<span data-ttu-id="64d2f-107">Ha létrehozta a táblahivatkozást, további műveleteket végezhet a táblával:</span><span class="sxs-lookup"><span data-stu-id="64d2f-107">Once you have a table reference, you can work further with your table:</span></span>

* [<span data-ttu-id="64d2f-108">Tábla lekérdezése</span><span class="sxs-lookup"><span data-stu-id="64d2f-108">Query a Table</span></span>](#querying)
  * [<span data-ttu-id="64d2f-109">Adatok szűrése</span><span class="sxs-lookup"><span data-stu-id="64d2f-109">Filtering Data</span></span>](#table-filter)
  * [<span data-ttu-id="64d2f-110">Adatok lapozása</span><span class="sxs-lookup"><span data-stu-id="64d2f-110">Paging through Data</span></span>](#table-paging)
  * [<span data-ttu-id="64d2f-111">Adatok rendezése</span><span class="sxs-lookup"><span data-stu-id="64d2f-111">Sorting Data</span></span>](#sorting-data)
* [<span data-ttu-id="64d2f-112">Adatok beszúrása</span><span class="sxs-lookup"><span data-stu-id="64d2f-112">Inserting Data</span></span>](#inserting)
* [<span data-ttu-id="64d2f-113">Adatok módosítása</span><span class="sxs-lookup"><span data-stu-id="64d2f-113">Modifying Data</span></span>](#modifying)
* [<span data-ttu-id="64d2f-114">Adatok törlése</span><span class="sxs-lookup"><span data-stu-id="64d2f-114">Deleting Data</span></span>](#deleting)

### <span data-ttu-id="64d2f-115"><a name="querying"></a>Útmutató: Táblahivatkozás lekérdezése</span><span class="sxs-lookup"><span data-stu-id="64d2f-115"><a name="querying"></a>How to: Query a table reference</span></span>
<span data-ttu-id="64d2f-116">Miután egy táblahivatkozás, használhatja azt tooquery hello kiszolgálón tárolt adatok.</span><span class="sxs-lookup"><span data-stu-id="64d2f-116">Once you have a table reference, you can use it tooquery for data on hello server.</span></span>  <span data-ttu-id="64d2f-117">A lekérdezések egy, a LINQ-hez hasonló nyelv használatával végezhetőek el.</span><span class="sxs-lookup"><span data-stu-id="64d2f-117">Queries are made in a "LINQ-like" language.</span></span>
<span data-ttu-id="64d2f-118">tooreturn hello táblából, a következő használatát hello minden adat kód:</span><span class="sxs-lookup"><span data-stu-id="64d2f-118">tooreturn all data from hello table, use hello following code:</span></span>

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

<span data-ttu-id="64d2f-119">hello sikeres függvény hívása esetén hello eredményekkel.</span><span class="sxs-lookup"><span data-stu-id="64d2f-119">hello success function is called with hello results.</span></span>  <span data-ttu-id="64d2f-120">Ne használjon `for (var i in results)` hello sikeres a kívánt módon, hogy fog ismétlés hello eredmények szereplő információk amikor más lekérdezési funkciók (például `.includeTotalCount()`) használt.</span><span class="sxs-lookup"><span data-stu-id="64d2f-120">Do not use `for (var i in results)` in hello success function as that will iterate over information that is included in hello results when other query functions (such as `.includeTotalCount()`) are used.</span></span>

<span data-ttu-id="64d2f-121">A lekérdezés szintaxisa hello további információkért lásd: hello [lekérdezésekre vonatkozó dokumentációt objektum].</span><span class="sxs-lookup"><span data-stu-id="64d2f-121">For more information on hello Query syntax, see hello [Query object documentation].</span></span>

#### <span data-ttu-id="64d2f-122"><a name="table-filter"></a>Hello kiszolgálón lévő adatok szűrése</span><span class="sxs-lookup"><span data-stu-id="64d2f-122"><a name="table-filter"></a>Filtering data on hello server</span></span>
<span data-ttu-id="64d2f-123">Használhatja a `where` záradék található hello táblahivatkozás:</span><span class="sxs-lookup"><span data-stu-id="64d2f-123">You can use a `where` clause on hello table reference:</span></span>

```
table
    .where({ userId: user.userId, complete: false })
    .read()
    .then(success, failure);
```

<span data-ttu-id="64d2f-124">Szűrő hello objektum függvény is használható.</span><span class="sxs-lookup"><span data-stu-id="64d2f-124">You can also use a function that filters hello object.</span></span>  <span data-ttu-id="64d2f-125">Ebben az esetben hello `this` változó hozzá van rendelve a szűrni kívánt toothe aktuális objektum.</span><span class="sxs-lookup"><span data-stu-id="64d2f-125">In this case, hello `this` variable is assigned toothe current object being filtered.</span></span>  <span data-ttu-id="64d2f-126">a következő kód hello funkcionális szempontból egyenértékű toohello a korábbi példában látható:</span><span class="sxs-lookup"><span data-stu-id="64d2f-126">hello following code is functionally equivalent toohello prior example:</span></span>

```
function filterByUserId(currentUserId) {
    return this.userId === currentUserId && this.complete === false;
}

table
    .where(filterByUserId, user.userId)
    .read()
    .then(success, failure);
```

#### <span data-ttu-id="64d2f-127"><a name="table-paging"></a>Adatok lapozása</span><span class="sxs-lookup"><span data-stu-id="64d2f-127"><a name="table-paging"></a>Paging through data</span></span>
<span data-ttu-id="64d2f-128">Hello használata `take()` és `skip()` módszerek.</span><span class="sxs-lookup"><span data-stu-id="64d2f-128">Utilize hello `take()` and `skip()` methods.</span></span>  <span data-ttu-id="64d2f-129">Ha például 100 soron kívüli bejegyzésekbe toosplit hello tábla kívánja:</span><span class="sxs-lookup"><span data-stu-id="64d2f-129">For example, if you wish toosplit hello table into 100-row records:</span></span>

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

<span data-ttu-id="64d2f-130">Hello `.includeTotalCount()` metódus használt tooadd egy totalCount mező toohello eredmények objektum.</span><span class="sxs-lookup"><span data-stu-id="64d2f-130">hello `.includeTotalCount()` method is used tooadd a totalCount field toohello results object.</span></span>  <span data-ttu-id="64d2f-131">A totalCount mezőben azt jelzi, hogy nincs lapozás használata vissza hello száma megjelenik.</span><span class="sxs-lookup"><span data-stu-id="64d2f-131">The totalCount field is filled with hello total number of records that would be returned if no paging is used.</span></span>

<span data-ttu-id="64d2f-132">Ezután használhatja hello lapok változó és néhány UI gombokat tooprovide egy lista; használjon `loadPage()` hello új rögzíti az összes lapon betöltése.</span><span class="sxs-lookup"><span data-stu-id="64d2f-132">You can then use hello pages variable and some UI buttons tooprovide a page list; use `loadPage()` to load hello new records for each page.</span></span>  <span data-ttu-id="64d2f-133">Már be van töltve gyorsítótárazási toospeed hozzáférés toorecords megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="64d2f-133">Implement caching toospeed access toorecords that have already been loaded.</span></span>

#### <span data-ttu-id="64d2f-134"><a name="sorting-data"></a>Útmutató: Rendezett adatok visszaadása</span><span class="sxs-lookup"><span data-stu-id="64d2f-134"><a name="sorting-data"></a>How to: Return sorted data</span></span>
<span data-ttu-id="64d2f-135">Használjon hello `.orderBy()` vagy `.orderByDescending()` lekérdezési módszerek:</span><span class="sxs-lookup"><span data-stu-id="64d2f-135">Use hello `.orderBy()` or `.orderByDescending()` query methods:</span></span>

```
table
    .orderBy('name')
    .read()
    .then(success, failure);
```

<span data-ttu-id="64d2f-136">Hello lekérdezés objektumon további információkért lásd: hello [lekérdezésekre vonatkozó dokumentációt objektum].</span><span class="sxs-lookup"><span data-stu-id="64d2f-136">For more information on hello Query object, see hello [Query object documentation].</span></span>

### <span data-ttu-id="64d2f-137"><a name="inserting"></a>Útmutató: Adatok beszúrása</span><span class="sxs-lookup"><span data-stu-id="64d2f-137"><a name="inserting"></a>How to: Insert data</span></span>
<span data-ttu-id="64d2f-138">Hozzon létre egy JavaScript objektum hello megfelelő dátumot és hívás `table.insert()` aszinkron módon:</span><span class="sxs-lookup"><span data-stu-id="64d2f-138">Create a JavaScript object with hello appropriate date and call `table.insert()` asynchronously:</span></span>

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

<span data-ttu-id="64d2f-139">A sikeres beszúrási hello beszúrt elem mezőkkel hello további szinkronizálási műveletek szükséges adja vissza.</span><span class="sxs-lookup"><span data-stu-id="64d2f-139">On successful insertion, hello inserted item is returned with hello additional fields that are required for sync operations.</span></span>  <span data-ttu-id="64d2f-140">Frissítse ezekkel az információkkal a gyorsítótárat a későbbi frissítésekhez.</span><span class="sxs-lookup"><span data-stu-id="64d2f-140">Update your own cache with this information for later updates.</span></span>

<span data-ttu-id="64d2f-141">hello Azure Mobile Apps Node.js Server SDK támogatja a dinamikus séma fejlesztési célokra szolgál.</span><span class="sxs-lookup"><span data-stu-id="64d2f-141">hello Azure Mobile Apps Node.js Server SDK supports dynamic schema for development purposes.</span></span>  <span data-ttu-id="64d2f-142">Dinamikus séma lehetővé teszi tooadd oszlopok toohello tábla insert vagy az update művelet megadásával.</span><span class="sxs-lookup"><span data-stu-id="64d2f-142">Dynamic Schema allows you tooadd columns toohello table by specifying them in an insert or update operation.</span></span>  <span data-ttu-id="64d2f-143">Azt javasoljuk, hogy kikapcsolja dinamikus séma az alkalmazás tooproduction áthelyezése előtt.</span><span class="sxs-lookup"><span data-stu-id="64d2f-143">We recommend that you turn off dynamic schema before moving your application tooproduction.</span></span>

### <span data-ttu-id="64d2f-144"><a name="modifying"></a>Útmutató: Adatok módosítása</span><span class="sxs-lookup"><span data-stu-id="64d2f-144"><a name="modifying"></a>How to: Modify data</span></span>
<span data-ttu-id="64d2f-145">Hasonló toohello `.insert()` módszer, kell létrehozni egy frissítés objektumot és hívhatja `.update()`.</span><span class="sxs-lookup"><span data-stu-id="64d2f-145">Similar toohello `.insert()` method, you should create an Update object and then call `.update()`.</span></span>  <span data-ttu-id="64d2f-146">hello frissítés objektumnak tartalmaznia kell hello azonosítója hello rekord toobe frissítése – hello azonosító egységekre hello rekord olvasásakor, vagy meghívásakor `.insert()`.</span><span class="sxs-lookup"><span data-stu-id="64d2f-146">hello update object must contain hello ID of hello record toobe updated - hello ID is obtained when reading hello record or when calling `.insert()`.</span></span>

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

### <span data-ttu-id="64d2f-147"><a name="deleting"></a>Útmutató: Adatok törlése</span><span class="sxs-lookup"><span data-stu-id="64d2f-147"><a name="deleting"></a>How to: Delete data</span></span>
<span data-ttu-id="64d2f-148">toodelete rekord, hívás hello `.del()` metódust.</span><span class="sxs-lookup"><span data-stu-id="64d2f-148">toodelete a record, call hello `.del()` method.</span></span>  <span data-ttu-id="64d2f-149">Adjon át hello azonosító objektumhivatkozás:</span><span class="sxs-lookup"><span data-stu-id="64d2f-149">Pass hello ID in an object reference:</span></span>

```
table
    .del({ id: '7163bc7a-70b2-4dde-98e9-8818969611bd' })
    .done(function () {
        // Record is now deleted - update your cache
    }, failure);
```
