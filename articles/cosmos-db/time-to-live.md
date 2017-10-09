---
title: "aaaExpire adatokat az Adatbázisba az Azure Cosmos toolive idővel |} Microsoft Docs"
description: "A TTL-t a Microsoft Azure Cosmos DB biztosít a hello képességét toohave dokumentumok üríti ki automatikusan hello rendszerről egy bizonyos idő eltelte után."
services: cosmos-db
documentationcenter: 
keywords: "toolive idő"
author: arramac
manager: jhubbard
editor: 
ms.assetid: 25fcbbda-71f7-414a-bf57-d8671358ca3f
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: arramac
ms.openlocfilehash: 51d8ec46add72c9624457316a4ccd1e23fb83ad0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="expire-data-in-azure-cosmos-db-collections-automatically-with-time-toolive"></a><span data-ttu-id="c99fb-104">Automatikusan toolive idővel rendelkező Azure Cosmos DB gyűjteményekben lévő adatok lejárnak</span><span class="sxs-lookup"><span data-stu-id="c99fb-104">Expire data in Azure Cosmos DB collections automatically with time toolive</span></span>
<span data-ttu-id="c99fb-105">Alkalmazások létrehozására és hatalmas mennyiségű adat tárolására is használható.</span><span class="sxs-lookup"><span data-stu-id="c99fb-105">Applications can produce and store vast amounts of data.</span></span> <span data-ttu-id="c99fb-106">Az adatok egy részét, például generált gép esemény adatokat, a naplókat, és a felhasználói munkamenet az információ csak véges időn.</span><span class="sxs-lookup"><span data-stu-id="c99fb-106">Some of this data, like machine generated event data, logs, and user session information is only useful for a finite period of time.</span></span> <span data-ttu-id="c99fb-107">Után hello adatok válnak hello alkalmazás igényeinek felesleges toohello biztonságos toopurge ezeket az adatokat, és csökkentse egy alkalmazás hello tárolási igényeit.</span><span class="sxs-lookup"><span data-stu-id="c99fb-107">Once hello data becomes surplus toohello needs of hello application it is safe toopurge this data and reduce hello storage needs of an application.</span></span>

<span data-ttu-id="c99fb-108">"Toolive idő" vagy a TTL-t a Microsoft Azure Cosmos DB nyújt hello képességét toohave dokumentumok automatikusan törlődnek hello adatbázis egy bizonyos idő eltelte után.</span><span class="sxs-lookup"><span data-stu-id="c99fb-108">With "time toolive" or TTL, Microsoft Azure Cosmos DB provides hello ability toohave documents automatically purged from hello database after a period of time.</span></span> <span data-ttu-id="c99fb-109">hello alapértelmezett idő toolive hello gyűjtemény szinten adja meg, és dokumentumra alapon felülbírálható.</span><span class="sxs-lookup"><span data-stu-id="c99fb-109">hello default time toolive can be set at hello collection level, and overridden on a per-document basis.</span></span> <span data-ttu-id="c99fb-110">Élettartam beállítása, a gyűjtemény alapértelmezett vagy a dokumentum szinten után Cosmos DB automatikusan eltávolítja a dokumentumok létező adott időn belül, (másodpercben), mert utolsó módosítás.</span><span class="sxs-lookup"><span data-stu-id="c99fb-110">Once TTL is set, either as a collection default or at a document level, Cosmos DB will automatically remove documents that exist after that period of time, in seconds, since they were last modified.</span></span>

<span data-ttu-id="c99fb-111">A Cosmos DB toolive idő hello dokumentum utolsó módosításának elleni eltolás használja.</span><span class="sxs-lookup"><span data-stu-id="c99fb-111">Time toolive in Cosmos DB uses an offset against when hello document was last modified.</span></span> <span data-ttu-id="c99fb-112">toodo ezt használja hello `_ts` mező, amelyben megtalálható minden a dokumentumban.</span><span class="sxs-lookup"><span data-stu-id="c99fb-112">toodo this it uses hello `_ts` field which exists on every document.</span></span> <span data-ttu-id="c99fb-113">hello _ts mezője unix típusú epoch időbélyeg képviselő hello dátumot és időpontot.</span><span class="sxs-lookup"><span data-stu-id="c99fb-113">hello _ts field is a unix-style epoch timestamp representing hello date and time.</span></span> <span data-ttu-id="c99fb-114">Hello `_ts` mező frissül minden alkalommal, amikor a dokumentum módosításakor.</span><span class="sxs-lookup"><span data-stu-id="c99fb-114">hello `_ts` field is updated every time a document is modified.</span></span> 

## <a name="ttl-behavior"></a><span data-ttu-id="c99fb-115">Élettartam viselkedése</span><span class="sxs-lookup"><span data-stu-id="c99fb-115">TTL behavior</span></span>
<span data-ttu-id="c99fb-116">hello TTL szolgáltatás TTL tulajdonságok két szintű - hello gyűjtemény és hello dokumentum vezérli.</span><span class="sxs-lookup"><span data-stu-id="c99fb-116">hello TTL feature is controlled by TTL properties at two levels - hello collection level and hello document level.</span></span> <span data-ttu-id="c99fb-117">hello értékeket másodpercben van beállítva, és a hello különbözeti tekintendők `_ts` hello dokumentum utolsó módosításának következő.</span><span class="sxs-lookup"><span data-stu-id="c99fb-117">hello values are set in seconds and are treated as a delta from hello `_ts` that hello document was last modified at.</span></span>

1. <span data-ttu-id="c99fb-118">Hello gyűjtemény DefaultTTL</span><span class="sxs-lookup"><span data-stu-id="c99fb-118">DefaultTTL for hello collection</span></span>
   
   * <span data-ttu-id="c99fb-119">Ha a hiányzó (vagy set toonull), dokumentumok nem törlődnek automatikusan.</span><span class="sxs-lookup"><span data-stu-id="c99fb-119">If missing (or set toonull), documents are not deleted automatically.</span></span>
   * <span data-ttu-id="c99fb-120">Ha van ilyen hello értéke pedig "-1" = végtelen – dokumentumok alapértelmezés szerint nem jár le</span><span class="sxs-lookup"><span data-stu-id="c99fb-120">If present and hello value is "-1" = infinite – documents don’t expire by default</span></span>
   * <span data-ttu-id="c99fb-121">Ha van ilyen és hello érték egy szám ("n") – a dokumentumok lejárnak "n" másodperc után utolsó módosítás</span><span class="sxs-lookup"><span data-stu-id="c99fb-121">If present and hello value is some number ("n") – documents expire "n” seconds after last modification</span></span>
2. <span data-ttu-id="c99fb-122">Hello dokumentumok élettartam:</span><span class="sxs-lookup"><span data-stu-id="c99fb-122">TTL for hello documents:</span></span> 
   
   * <span data-ttu-id="c99fb-123">A tulajdonság csak DefaultTTL jelen a hello szülőgyűjtemény vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="c99fb-123">Property is applicable only if DefaultTTL is present for hello parent collection.</span></span>
   * <span data-ttu-id="c99fb-124">Felülbírálja a hello szülőgyűjtemény hello DefaultTTL értékét.</span><span class="sxs-lookup"><span data-stu-id="c99fb-124">Overrides hello DefaultTTL value for hello parent collection.</span></span>

<span data-ttu-id="c99fb-125">Amint hello dokumentum lejárt (`ttl`  +  `_ts` > = aktuális server idő), "lejárt" hello dokumentum van jelölve.</span><span class="sxs-lookup"><span data-stu-id="c99fb-125">As soon as hello document has expired (`ttl` + `_ts` >= current server time), hello document is marked as "expired”.</span></span> <span data-ttu-id="c99fb-126">Nincs művelet után most kapnak a dokumentumokon, és azok nem kerülnek bele az hello végrehajtott lekérdezések eredményeit.</span><span class="sxs-lookup"><span data-stu-id="c99fb-126">No operation will be allowed on these documents after this time and they will be excluded from hello results of any queries performed.</span></span> <span data-ttu-id="c99fb-127">hello dokumentumok hello rendszer fizikailag el kell hagyni, és törli hello háttérben szerint egy későbbi időpontban.</span><span class="sxs-lookup"><span data-stu-id="c99fb-127">hello documents are physically deleted in hello system, and are deleted in hello background opportunistically at a later time.</span></span> <span data-ttu-id="c99fb-128">Ez nem foglal le a [kérelem egységek (RUs)](request-units.md) hello gyűjtemény költségvetés.</span><span class="sxs-lookup"><span data-stu-id="c99fb-128">This does not consume any [Request Units (RUs)](request-units.md) from hello collection budget.</span></span>

<span data-ttu-id="c99fb-129">hello fent logika mutatja be a következő mátrix hello:</span><span class="sxs-lookup"><span data-stu-id="c99fb-129">hello above logic can be shown in hello following matrix:</span></span>

|  | <span data-ttu-id="c99fb-130">DefaultTTL hiányzik vagy nem hello gyűjtemény beállítása</span><span class="sxs-lookup"><span data-stu-id="c99fb-130">DefaultTTL missing/not set on hello collection</span></span> | <span data-ttu-id="c99fb-131">DefaultTTL = -1-gyűjteménytől</span><span class="sxs-lookup"><span data-stu-id="c99fb-131">DefaultTTL = -1 on collection</span></span> | <span data-ttu-id="c99fb-132">DefaultTTL = "n" gyűjteménytől</span><span class="sxs-lookup"><span data-stu-id="c99fb-132">DefaultTTL = "n" on collection</span></span> |
| --- |:--- |:--- |:--- |
| <span data-ttu-id="c99fb-133">A dokumentum hiányzó élettartam</span><span class="sxs-lookup"><span data-stu-id="c99fb-133">TTL Missing on document</span></span> |<span data-ttu-id="c99fb-134">Semmi toooverride hello dokumentum és a gyűjtemény tartozik TTL fogalma dokumentum szinten.</span><span class="sxs-lookup"><span data-stu-id="c99fb-134">Nothing toooverride at document level since both hello document and collection have no concept of TTL.</span></span> |<span data-ttu-id="c99fb-135">Ebben a gyűjteményben nem dokumentumok lejárnak.</span><span class="sxs-lookup"><span data-stu-id="c99fb-135">No documents in this collection will expire.</span></span> |<span data-ttu-id="c99fb-136">a gyűjtemény hello dokumentumok amikor időköz n lejárta lejár.</span><span class="sxs-lookup"><span data-stu-id="c99fb-136">hello documents in this collection will expire when interval n elapses.</span></span> |
| <span data-ttu-id="c99fb-137">Élettartam dokumentum -1 =</span><span class="sxs-lookup"><span data-stu-id="c99fb-137">TTL = -1 on document</span></span> |<span data-ttu-id="c99fb-138">Semmi hello dokumentum szinten óta hello gyűjtemény nem ad meg toooverride hello DefaultTTL tulajdonság, amely egy dokumentum lehet felülbírálni.</span><span class="sxs-lookup"><span data-stu-id="c99fb-138">Nothing toooverride at hello document level since hello collection doesn’t define hello DefaultTTL property that a document can override.</span></span> <span data-ttu-id="c99fb-139">A dokumentum a TTL nem értelmezett hello rendszer.</span><span class="sxs-lookup"><span data-stu-id="c99fb-139">TTL on a document is un-interpreted by hello system.</span></span> |<span data-ttu-id="c99fb-140">Ebben a gyűjteményben nem dokumentumok lejárnak.</span><span class="sxs-lookup"><span data-stu-id="c99fb-140">No documents in this collection will expire.</span></span> |<span data-ttu-id="c99fb-141">az élettartam =-1. a gyűjtemény hello dokumentum soha nem jár le.</span><span class="sxs-lookup"><span data-stu-id="c99fb-141">hello document with TTL=-1 in this collection will never expire.</span></span> <span data-ttu-id="c99fb-142">Minden más dokumentum "n" időszak után lejár.</span><span class="sxs-lookup"><span data-stu-id="c99fb-142">All other documents will expire after "n" interval.</span></span> |
| <span data-ttu-id="c99fb-143">Élettartam dokumentumon n =</span><span class="sxs-lookup"><span data-stu-id="c99fb-143">TTL = n on document</span></span> |<span data-ttu-id="c99fb-144">Semmi toooverride hello dokumentum szinten.</span><span class="sxs-lookup"><span data-stu-id="c99fb-144">Nothing toooverride at hello document level.</span></span> <span data-ttu-id="c99fb-145">A dokumentumban TTL nem értelmezett hello rendszer.</span><span class="sxs-lookup"><span data-stu-id="c99fb-145">TTL on a document in un-interpreted by hello system.</span></span> |<span data-ttu-id="c99fb-146">Élettartam hello dokumentum = n időköz n másodperc után lejár.</span><span class="sxs-lookup"><span data-stu-id="c99fb-146">hello document with TTL = n will expire after interval n, in seconds.</span></span> <span data-ttu-id="c99fb-147">Egyéb dokumentumokat fog örökli időintervalluma -1, és nem jár le.</span><span class="sxs-lookup"><span data-stu-id="c99fb-147">Other documents will inherit interval of -1 and never expire.</span></span> |<span data-ttu-id="c99fb-148">Élettartam hello dokumentum = n időköz n másodperc után lejár.</span><span class="sxs-lookup"><span data-stu-id="c99fb-148">hello document with TTL = n will expire after interval n, in seconds.</span></span> <span data-ttu-id="c99fb-149">Egyéb dokumentumokat öröklik a "n" időköz hello gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="c99fb-149">Other documents will inherit "n" interval from hello collection.</span></span> |

## <a name="configuring-ttl"></a><span data-ttu-id="c99fb-150">Élettartam konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c99fb-150">Configuring TTL</span></span>
<span data-ttu-id="c99fb-151">Alapértelmezés szerint toolive idő összes Cosmos DB gyűjtemények, valamint minden dokumentumok alapértelmezés szerint le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="c99fb-151">By default, time toolive is disabled by default in all Cosmos DB collections and on all documents.</span></span>

## <a name="enabling-ttl"></a><span data-ttu-id="c99fb-152">Élettartam engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c99fb-152">Enabling TTL</span></span>
<span data-ttu-id="c99fb-153">tooenable TTL-t a gyűjteményt, vagy egy gyűjteményen belül hello dokumentumok, tooset hello DefaultTTL tulajdonság egy gyűjtemény tooeither -1 vagy nullától eltérő pozitív számnak kell.</span><span class="sxs-lookup"><span data-stu-id="c99fb-153">tooenable TTL on a collection, or hello documents within a collection, you need tooset hello DefaultTTL property of a collection tooeither -1 or a non-zero positive number.</span></span> <span data-ttu-id="c99fb-154">Hello DefaultTTL túl-1 értékre, hogy alapértelmezés szerint hello gyűjteményben lévő összes dokumentumot végtelen lesz élő, de hello Cosmos DB szolgáltatás célszerű figyelemmel kísérni a gyűjteményhez, akik bírálták felül az alapértelmezett dokumentumok beállítása.</span><span class="sxs-lookup"><span data-stu-id="c99fb-154">Setting hello DefaultTTL too-1 means that by default all documents in hello collection will live forever but hello Cosmos DB service should monitor this collection for documents that have overridden this default.</span></span>

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive =-1; //never expire by default

    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(databaseName),
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });

## <a name="configuring-default-ttl-on-a-collection"></a><span data-ttu-id="c99fb-155">A gyűjtemény alapértelmezett élettartam konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c99fb-155">Configuring default TTL on a collection</span></span>
<span data-ttu-id="c99fb-156">Biztosan tudja tooconfigure toolive alapértelmezett időpontot, a gyűjtemény szintjén.</span><span class="sxs-lookup"><span data-stu-id="c99fb-156">You are able tooconfigure a default time toolive at a collection level.</span></span> <span data-ttu-id="c99fb-157">tooset hello TTL-t egy gyűjteményen kell tooprovide nullától eltérő pozitív szám, amely jelzi a hello időtartam (másodpercben), tooexpire hello gyűjteményben lévő összes dokumentumot után hello utolsó módosítás hello dokumentum időbélyegzője (`_ts`).</span><span class="sxs-lookup"><span data-stu-id="c99fb-157">tooset hello TTL on a collection, you need tooprovide a non-zero positive number that indicates hello period, in seconds, tooexpire all documents in hello collection after hello last modified timestamp of hello document (`_ts`).</span></span> <span data-ttu-id="c99fb-158">Másik lehetőségként hello alapértelmezett túl-1, ami azt jelenti, hogy toohello gyűjteményben lévő összes dokumentumok határozatlan ideig fog élő alapértelmezés szerint állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="c99fb-158">Or, you can set hello default too-1, which implies that all documents inserted in toohello collection will live indefinitely by default.</span></span>

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive = 90 * 60 * 60 * 24; // expire all documents after 90 days
    
    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        "/dbs/salesdb",
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });


## <a name="setting-ttl-on-a-document"></a><span data-ttu-id="c99fb-159">A dokumentum TTL beállítása</span><span class="sxs-lookup"><span data-stu-id="c99fb-159">Setting TTL on a document</span></span>
<span data-ttu-id="c99fb-160">Továbbá a gyűjtemény alapértelmezett élettartam toosetting állíthatja be adott TTL dokumentum szinten.</span><span class="sxs-lookup"><span data-stu-id="c99fb-160">In addition toosetting a default TTL on a collection you can set specific TTL at a document level.</span></span> <span data-ttu-id="c99fb-161">Ez a művelet felülírja hello alapértelmezett hello gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="c99fb-161">Doing this will override hello default of hello collection.</span></span>

* <span data-ttu-id="c99fb-162">tooset hello TTL-t egy dokumentum, egy nem nulla pozitív szám, amely megadja, hogy az az időszak (másodpercben), tooexpire hello dokumentum hello utolsó hello dokumentum időbélyeg módosítása után hello tooprovide szüksége (`_ts`).</span><span class="sxs-lookup"><span data-stu-id="c99fb-162">tooset hello TTL on a document, you need tooprovide a non-zero positive number which indicates hello period, in seconds, tooexpire hello document after hello last modified timestamp of hello document (`_ts`).</span></span>
* <span data-ttu-id="c99fb-163">Ha egy dokumentum nem TTL mezőben, majd az alapértelmezett hello gyűjtemény hello érvényesek.</span><span class="sxs-lookup"><span data-stu-id="c99fb-163">If a document has no TTL field, then hello default of hello collection will apply.</span></span>
* <span data-ttu-id="c99fb-164">Élettartam hello adatgyűjtési szint le van tiltva, ha hello TTL mező hello dokumentum figyelmen kívül TTL ismételt engedélyezéséig hello gyűjteményen.</span><span class="sxs-lookup"><span data-stu-id="c99fb-164">If TTL is disabled at hello collection level, hello TTL field on hello document will be ignored until TTL is enabled again on hello collection.</span></span>

<span data-ttu-id="c99fb-165">Íme egy részlet jelenít meg, hogyan tooset hello TTL lejárata egy dokumentumon:</span><span class="sxs-lookup"><span data-stu-id="c99fb-165">Here's a snippet showing how tooset hello TTL expiration on a document:</span></span>

    // Include a property that serializes too"ttl" in JSON
    public class SalesOrder
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        
        [JsonProperty(PropertyName="cid")]
        public string CustomerId { get; set; }
        
        // used tooset expiration policy
        [JsonProperty(PropertyName = "ttl", NullValueHandling = NullValueHandling.Ignore)]
        public int? TimeToLive { get; set; }
        
        //...
    }
    
    // Set hello value toohello expiration in seconds
    SalesOrder salesOrder = new SalesOrder
    {
        Id = "SO05",
        CustomerId = "CO18009186470",
        TimeToLive = 60 * 60 * 24 * 30;  // Expire sales orders in 30 days 
    };


## <a name="extending-ttl-on-an-existing-document"></a><span data-ttu-id="c99fb-166">A létező dokumentum TTL kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="c99fb-166">Extending TTL on an existing document</span></span>
<span data-ttu-id="c99fb-167">Bármely dokumentum hello írási művelet végrehajtásával alaphelyzetbe állíthatja a hello TTL-t egy dokumentumon.</span><span class="sxs-lookup"><span data-stu-id="c99fb-167">You can reset hello TTL on a document by doing any write operation on hello document.</span></span> <span data-ttu-id="c99fb-168">Ezzel fogja beállítani hello `_ts` toohello aktuális idő és hello visszaszámlálási toohello dokumentum lejárati, hello beállított `ttl`, megkezdődik a újra.</span><span class="sxs-lookup"><span data-stu-id="c99fb-168">Doing this will set hello `_ts` toohello current time, and hello countdown toohello document expiry, as set by hello `ttl`, will begin again.</span></span> <span data-ttu-id="c99fb-169">Ha toochange hello `ttl` egy dokumentum is frissítheti hello mező, mint bármely más beállítható mező teheti.</span><span class="sxs-lookup"><span data-stu-id="c99fb-169">If you wish toochange hello `ttl` of a document, you can update hello field as you can do with any other settable field.</span></span>

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = 60 * 30 * 30; // update time toolive
    
    response = await client.ReplaceDocumentAsync(salesOrder);

## <a name="removing-ttl-from-a-document"></a><span data-ttu-id="c99fb-170">Egy dokumentum eltávolítása a TTL-t</span><span class="sxs-lookup"><span data-stu-id="c99fb-170">Removing TTL from a document</span></span>
<span data-ttu-id="c99fb-171">Ha már nem szeretné, hogy a dokumentum tooexpire TTL be van állítva egy dokumentumot, majd hello dokumentum beolvasása, távolítsa el hello TTL mező és hello kiszolgálón hello dokumentum cseréje.</span><span class="sxs-lookup"><span data-stu-id="c99fb-171">If a TTL has been set on a document and you no longer want that document tooexpire, then you can retrieve hello document, remove hello TTL field and replace hello document on hello server.</span></span> <span data-ttu-id="c99fb-172">Hello TTL mező hello dokumentum törlődik, hello alapértelmezett hello gyűjtemény lépnek érvénybe.</span><span class="sxs-lookup"><span data-stu-id="c99fb-172">When hello TTL field is removed from hello document, hello default of hello collection will be applied.</span></span> <span data-ttu-id="c99fb-173">toostop lejárjanak dokumentum és nem örököl a hello gyűjtemény tooset hello TTL érték túl-1 kell használnia.</span><span class="sxs-lookup"><span data-stu-id="c99fb-173">toostop a document from expiring and not inherit from hello collection then you need tooset hello TTL value too-1.</span></span>

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = null; // inherit hello default TTL of hello collection
    
    response = await client.ReplaceDocumentAsync(salesOrder);

## <a name="disabling-ttl"></a><span data-ttu-id="c99fb-174">Élettartam letiltása</span><span class="sxs-lookup"><span data-stu-id="c99fb-174">Disabling TTL</span></span>
<span data-ttu-id="c99fb-175">toodisable teljesen a egy adatgyűjtési és -leállítási hello háttér TTL törölni kell a hello DefaultTTL tulajdonság hello gyűjteményen lejárt dokumentumot keres a feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="c99fb-175">toodisable TTL entirely on a collection and stop hello background process from looking for expired documents hello DefaultTTL property on hello collection should be deleted.</span></span> <span data-ttu-id="c99fb-176">Ez a tulajdonság törlése eltér túl 1 – állítaná.</span><span class="sxs-lookup"><span data-stu-id="c99fb-176">Deleting this property is different from setting it too-1.</span></span> <span data-ttu-id="c99fb-177">Túl-1 értékre beállítása esetén az új dokumentumok hozzáadott toohello gyűjtemény lesz élő végtelen, de ez felülírható hello gyűjtemény adott dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="c99fb-177">Setting too-1 means new documents added toohello collection will live forever but you can override this on specific documents in hello collection.</span></span> <span data-ttu-id="c99fb-178">Ez a tulajdonság teljesen hello gyűjteményből eltávolítása azt jelenti, hogy a dokumentumok lejár, akkor is, ha vannak, akik explicit módon bírálták felül az előző alapértelmezett dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="c99fb-178">Removing this property entirely from hello collection means that no documents will expire, even if there are documents that have explicitly overridden a previous default.</span></span>

    DocumentCollection collection = await client.ReadDocumentCollectionAsync("/dbs/salesdb/colls/orders");
    
    // Disable TTL
    collection.DefaultTimeToLive = null;
    
    await client.ReplaceDocumentCollectionAsync(collection);


## <a name="faq"></a><span data-ttu-id="c99fb-179">GYIK</span><span class="sxs-lookup"><span data-stu-id="c99fb-179">FAQ</span></span>
<span data-ttu-id="c99fb-180">**Mit fog költség TTL-t?**</span><span class="sxs-lookup"><span data-stu-id="c99fb-180">**What will TTL cost me?**</span></span>

<span data-ttu-id="c99fb-181">Nem jelent többletköltséget toosetting egy dokumentumon TTL van.</span><span class="sxs-lookup"><span data-stu-id="c99fb-181">There is no additional cost toosetting a TTL on a document.</span></span>

<span data-ttu-id="c99fb-182">**Mennyi ideig tart toodelete a dokumentum után hello TTL működik-e?**</span><span class="sxs-lookup"><span data-stu-id="c99fb-182">**How long will it take toodelete my document once hello TTL is up?**</span></span>

<span data-ttu-id="c99fb-183">hello dokumentumok lejárt után azonnal hello TTL működik-e, és nem CRUD vagy a lekérdezés API-k elérhetők lesznek.</span><span class="sxs-lookup"><span data-stu-id="c99fb-183">hello documents are expired immediately once hello TTL is up, and will not be accessible via CRUD or query APIs.</span></span> 

<span data-ttu-id="c99fb-184">**Egy dokumentumon TTL semmilyen hatással lesz a RU díjak?**</span><span class="sxs-lookup"><span data-stu-id="c99fb-184">**Will TTL on a document have any impact on RU charges?**</span></span>

<span data-ttu-id="c99fb-185">Nem, nem lesznek nincs hatással az keresztül (élettartam) – Cosmos DB lejárt dokumentumok törlési RU költségek.</span><span class="sxs-lookup"><span data-stu-id="c99fb-185">No, there will be no impact on RU charges for deletions of expired documents via TTL in Cosmos DB.</span></span>

<span data-ttu-id="c99fb-186">**Hello TTL-szolgáltatás csak vonatkozik tooentire dokumentumok, vagy lejárhat I egyes dokumentum tulajdonságértékek?**</span><span class="sxs-lookup"><span data-stu-id="c99fb-186">**Does hello TTL feature only apply tooentire documents, or can I expire individual document property values?**</span></span>

<span data-ttu-id="c99fb-187">Élettartam toohello a teljes dokumentum vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="c99fb-187">TTL applies toohello entire document.</span></span> <span data-ttu-id="c99fb-188">Ha azt szeretné, hogy tooexpire egy részét egy dokumentumot, akkor az ajánlott hello részét kinyerése hello fő dokumentum tooa külön "kapcsolódó" dokumentum, és kövesse a kibontott dokumentumon TTL-t.</span><span class="sxs-lookup"><span data-stu-id="c99fb-188">If you would like tooexpire just a portion of a document, then it is recommended that you extract hello portion from hello main document in tooa separate "linked” document and then use TTL on that extracted document.</span></span>

<span data-ttu-id="c99fb-189">**Rendelkezik a hello TTL-t a szolgáltatás indexelési különleges követelményeket?**</span><span class="sxs-lookup"><span data-stu-id="c99fb-189">**Does hello TTL feature have any specific indexing requirements?**</span></span>

<span data-ttu-id="c99fb-190">Igen.</span><span class="sxs-lookup"><span data-stu-id="c99fb-190">Yes.</span></span> <span data-ttu-id="c99fb-191">rendelkeznie kell hello gyűjtemény [szabályzatokkal indexelő](indexing-policies.md) tooeither Consistent vagy Lazy.</span><span class="sxs-lookup"><span data-stu-id="c99fb-191">hello collection must have [indexing policy set](indexing-policies.md) tooeither Consistent or Lazy.</span></span> <span data-ttu-id="c99fb-192">Az indexelő set tooNone szervezendő DefaultTTL tooset próbált azt eredményezi, hogy hiba, mint ki egy gyűjteményt, amely már be van állítva egy DefaultTTL az indexelés közben tooturn fognak.</span><span class="sxs-lookup"><span data-stu-id="c99fb-192">Trying tooset DefaultTTL on a collection with indexing set tooNone will result in an error, as will trying tooturn off indexing on a collection that has a DefaultTTL already set.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c99fb-193">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c99fb-193">Next steps</span></span>
<span data-ttu-id="c99fb-194">toolearn Azure Cosmos DB, kapcsolatos további információkért tekintse meg a toohello szolgáltatás [ *dokumentáció* ](https://azure.microsoft.com/documentation/services/cosmos-db/) lap.</span><span class="sxs-lookup"><span data-stu-id="c99fb-194">toolearn more about Azure Cosmos DB, refer toohello service [*documentation*](https://azure.microsoft.com/documentation/services/cosmos-db/) page.</span></span>

