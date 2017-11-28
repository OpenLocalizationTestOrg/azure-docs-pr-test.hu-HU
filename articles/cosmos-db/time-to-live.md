---
title: "Élettartam Azure Cosmos DB az adatok lejárnak |} Microsoft Docs"
description: "A TTL-t a Microsoft Azure Cosmos DB teszi lehetővé a dokumentumokat egy bizonyos idő eltelte után a rendszer automatikusan törlődnek."
services: cosmos-db
documentationcenter: 
keywords: "élettartam"
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
ms.openlocfilehash: 6f1c43ca0113dc7579b0fc3743d3314c16ce78a4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="expire-data-in-azure-cosmos-db-collections-automatically-with-time-to-live"></a><span data-ttu-id="25550-104">Azure Cosmos DB gyűjtemények automatikusan élettartama adatok lejár</span><span class="sxs-lookup"><span data-stu-id="25550-104">Expire data in Azure Cosmos DB collections automatically with time to live</span></span>
<span data-ttu-id="25550-105">Alkalmazások létrehozására és hatalmas mennyiségű adat tárolására is használható.</span><span class="sxs-lookup"><span data-stu-id="25550-105">Applications can produce and store vast amounts of data.</span></span> <span data-ttu-id="25550-106">Az adatok egy részét, például generált gép esemény adatokat, a naplókat, és a felhasználói munkamenet az információ csak véges időn.</span><span class="sxs-lookup"><span data-stu-id="25550-106">Some of this data, like machine generated event data, logs, and user session information is only useful for a finite period of time.</span></span> <span data-ttu-id="25550-107">Miután az adatok válnak az alkalmazás igényeinek felesleges ezek az adatok kiürítése és a tárolási igényeinek, az alkalmazások biztonságos.</span><span class="sxs-lookup"><span data-stu-id="25550-107">Once the data becomes surplus to the needs of the application it is safe to purge this data and reduce the storage needs of an application.</span></span>

<span data-ttu-id="25550-108">"Élő idő" vagy a TTL-t a Microsoft Azure Cosmos DB nyújt a lehetővé teszi, hogy a dokumentumok automatikusan törlődnek az adatbázis egy bizonyos idő eltelte után.</span><span class="sxs-lookup"><span data-stu-id="25550-108">With "time to live" or TTL, Microsoft Azure Cosmos DB provides the ability to have documents automatically purged from the database after a period of time.</span></span> <span data-ttu-id="25550-109">Az alapértelmezett élettartama a gyűjtemény szintjén állítsa be, és dokumentumra alapon felülbírálható.</span><span class="sxs-lookup"><span data-stu-id="25550-109">The default time to live can be set at the collection level, and overridden on a per-document basis.</span></span> <span data-ttu-id="25550-110">Élettartam beállítása, a gyűjtemény alapértelmezett vagy a dokumentum szinten után Cosmos DB automatikusan eltávolítja a dokumentumok létező adott időn belül, (másodpercben), mert utolsó módosítás.</span><span class="sxs-lookup"><span data-stu-id="25550-110">Once TTL is set, either as a collection default or at a document level, Cosmos DB will automatically remove documents that exist after that period of time, in seconds, since they were last modified.</span></span>

<span data-ttu-id="25550-111">A Cosmos DB élettartama a dokumentum utolsó módosításának elleni eltolás használja.</span><span class="sxs-lookup"><span data-stu-id="25550-111">Time to live in Cosmos DB uses an offset against when the document was last modified.</span></span> <span data-ttu-id="25550-112">Ehhez használja a `_ts` mező, amelyben megtalálható minden a dokumentumban.</span><span class="sxs-lookup"><span data-stu-id="25550-112">To do this it uses the `_ts` field which exists on every document.</span></span> <span data-ttu-id="25550-113">A _ts mezője dátumát és idejét jelző időbélyeg unix típusú epoch.</span><span class="sxs-lookup"><span data-stu-id="25550-113">The _ts field is a unix-style epoch timestamp representing the date and time.</span></span> <span data-ttu-id="25550-114">A `_ts` mező frissül minden alkalommal, amikor a dokumentum módosításakor.</span><span class="sxs-lookup"><span data-stu-id="25550-114">The `_ts` field is updated every time a document is modified.</span></span> 

## <a name="ttl-behavior"></a><span data-ttu-id="25550-115">Élettartam viselkedése</span><span class="sxs-lookup"><span data-stu-id="25550-115">TTL behavior</span></span>
<span data-ttu-id="25550-116">A TTL-szolgáltatás TTL tulajdonságok két szinten – a gyűjtemény és a dokumentum vezérli.</span><span class="sxs-lookup"><span data-stu-id="25550-116">The TTL feature is controlled by TTL properties at two levels - the collection level and the document level.</span></span> <span data-ttu-id="25550-117">Az értékek másodpercen belül vannak beállítva, és a különbözeti számít a `_ts` azon, hogy a dokumentum utolsó volt módosítani.</span><span class="sxs-lookup"><span data-stu-id="25550-117">The values are set in seconds and are treated as a delta from the `_ts` that the document was last modified at.</span></span>

1. <span data-ttu-id="25550-118">A gyűjtemény DefaultTTL</span><span class="sxs-lookup"><span data-stu-id="25550-118">DefaultTTL for the collection</span></span>
   
   * <span data-ttu-id="25550-119">Ha hiányoznak (vagy null értékű), dokumentumok nem törlődnek automatikusan.</span><span class="sxs-lookup"><span data-stu-id="25550-119">If missing (or set to null), documents are not deleted automatically.</span></span>
   * <span data-ttu-id="25550-120">Ha jelen van, és az érték "-1" = végtelen – dokumentumok alapértelmezés szerint nem jár le</span><span class="sxs-lookup"><span data-stu-id="25550-120">If present and the value is "-1" = infinite – documents don’t expire by default</span></span>
   * <span data-ttu-id="25550-121">Ha jelen van, és az érték az egyes szám ("n") – a dokumentumok lejárnak "n" másodperc után utolsó módosítás</span><span class="sxs-lookup"><span data-stu-id="25550-121">If present and the value is some number ("n") – documents expire "n” seconds after last modification</span></span>
2. <span data-ttu-id="25550-122">A dokumentumok élettartam:</span><span class="sxs-lookup"><span data-stu-id="25550-122">TTL for the documents:</span></span> 
   
   * <span data-ttu-id="25550-123">A tulajdonság csak akkor, ha a fölérendelt gyűjtemény DefaultTTL nincs alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="25550-123">Property is applicable only if DefaultTTL is present for the parent collection.</span></span>
   * <span data-ttu-id="25550-124">Felülbírálja a fölérendelt gyűjtemény DefaultTTL értékét.</span><span class="sxs-lookup"><span data-stu-id="25550-124">Overrides the DefaultTTL value for the parent collection.</span></span>

<span data-ttu-id="25550-125">Amint a dokumentum lejárt (`ttl`  +  `_ts` > = aktuális server idő), a dokumentum jelölést "lejárt".</span><span class="sxs-lookup"><span data-stu-id="25550-125">As soon as the document has expired (`ttl` + `_ts` >= current server time), the document is marked as "expired”.</span></span> <span data-ttu-id="25550-126">Nincs művelet után most kapnak a dokumentumokon, és azok nem kerülnek bele az összes végrehajtott lekérdezések eredményeit.</span><span class="sxs-lookup"><span data-stu-id="25550-126">No operation will be allowed on these documents after this time and they will be excluded from the results of any queries performed.</span></span> <span data-ttu-id="25550-127">A dokumentumok fizikailag törlődnek a rendszerben, és törli a háttérben szerint egy későbbi időpontban.</span><span class="sxs-lookup"><span data-stu-id="25550-127">The documents are physically deleted in the system, and are deleted in the background opportunistically at a later time.</span></span> <span data-ttu-id="25550-128">Ez nem foglal le a [kérelem egységek (RUs)](request-units.md) gyűjtemény költségvetés.</span><span class="sxs-lookup"><span data-stu-id="25550-128">This does not consume any [Request Units (RUs)](request-units.md) from the collection budget.</span></span>

<span data-ttu-id="25550-129">A fenti logika mutatja be a következő mátrix:</span><span class="sxs-lookup"><span data-stu-id="25550-129">The above logic can be shown in the following matrix:</span></span>

|  | <span data-ttu-id="25550-130">DefaultTTL hiányzik vagy nem a gyűjtemény beállítása</span><span class="sxs-lookup"><span data-stu-id="25550-130">DefaultTTL missing/not set on the collection</span></span> | <span data-ttu-id="25550-131">DefaultTTL = -1-gyűjteménytől</span><span class="sxs-lookup"><span data-stu-id="25550-131">DefaultTTL = -1 on collection</span></span> | <span data-ttu-id="25550-132">DefaultTTL = "n" gyűjteménytől</span><span class="sxs-lookup"><span data-stu-id="25550-132">DefaultTTL = "n" on collection</span></span> |
| --- |:--- |:--- |:--- |
| <span data-ttu-id="25550-133">A dokumentum hiányzó élettartam</span><span class="sxs-lookup"><span data-stu-id="25550-133">TTL Missing on document</span></span> |<span data-ttu-id="25550-134">Nem végezhető el a dokumentum szinten bírálja felül, mivel a dokumentum és a gyűjtemény nem ismerik a TTL-t.</span><span class="sxs-lookup"><span data-stu-id="25550-134">Nothing to override at document level since both the document and collection have no concept of TTL.</span></span> |<span data-ttu-id="25550-135">Ebben a gyűjteményben nem dokumentumok lejárnak.</span><span class="sxs-lookup"><span data-stu-id="25550-135">No documents in this collection will expire.</span></span> |<span data-ttu-id="25550-136">A gyűjtemény dokumentumok amikor időköz n lejárta lejár.</span><span class="sxs-lookup"><span data-stu-id="25550-136">The documents in this collection will expire when interval n elapses.</span></span> |
| <span data-ttu-id="25550-137">Élettartam dokumentum -1 =</span><span class="sxs-lookup"><span data-stu-id="25550-137">TTL = -1 on document</span></span> |<span data-ttu-id="25550-138">Nincs a szintjén bírálja felül, mert a gyűjtemény nem adja meg a dokumentum felülíró DefaultTTL tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="25550-138">Nothing to override at the document level since the collection doesn’t define the DefaultTTL property that a document can override.</span></span> <span data-ttu-id="25550-139">Egy dokumentumot a TTL-t a rendszer nem értelmezett.</span><span class="sxs-lookup"><span data-stu-id="25550-139">TTL on a document is un-interpreted by the system.</span></span> |<span data-ttu-id="25550-140">Ebben a gyűjteményben nem dokumentumok lejárnak.</span><span class="sxs-lookup"><span data-stu-id="25550-140">No documents in this collection will expire.</span></span> |<span data-ttu-id="25550-141">A dokumentum az élettartam =-1. a gyűjtemény nem jár.</span><span class="sxs-lookup"><span data-stu-id="25550-141">The document with TTL=-1 in this collection will never expire.</span></span> <span data-ttu-id="25550-142">Minden más dokumentum "n" időszak után lejár.</span><span class="sxs-lookup"><span data-stu-id="25550-142">All other documents will expire after "n" interval.</span></span> |
| <span data-ttu-id="25550-143">Élettartam dokumentumon n =</span><span class="sxs-lookup"><span data-stu-id="25550-143">TTL = n on document</span></span> |<span data-ttu-id="25550-144">Nem végezhető el a felülírja a dokumentum szinten.</span><span class="sxs-lookup"><span data-stu-id="25550-144">Nothing to override at the document level.</span></span> <span data-ttu-id="25550-145">A dokumentumban TTL nem értelmezett, a rendszer.</span><span class="sxs-lookup"><span data-stu-id="25550-145">TTL on a document in un-interpreted by the system.</span></span> |<span data-ttu-id="25550-146">A dokumentum TTL = n időköz n másodperc után lejár.</span><span class="sxs-lookup"><span data-stu-id="25550-146">The document with TTL = n will expire after interval n, in seconds.</span></span> <span data-ttu-id="25550-147">Egyéb dokumentumokat fog örökli időintervalluma -1, és nem jár le.</span><span class="sxs-lookup"><span data-stu-id="25550-147">Other documents will inherit interval of -1 and never expire.</span></span> |<span data-ttu-id="25550-148">A dokumentum TTL = n időköz n másodperc után lejár.</span><span class="sxs-lookup"><span data-stu-id="25550-148">The document with TTL = n will expire after interval n, in seconds.</span></span> <span data-ttu-id="25550-149">Egyéb dokumentumokat "n" időköz örökli a gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="25550-149">Other documents will inherit "n" interval from the collection.</span></span> |

## <a name="configuring-ttl"></a><span data-ttu-id="25550-150">Élettartam konfigurálása</span><span class="sxs-lookup"><span data-stu-id="25550-150">Configuring TTL</span></span>
<span data-ttu-id="25550-151">Alapértelmezés szerint élettartama összes Cosmos DB gyűjtemények, valamint minden dokumentumok alapértelmezés szerint le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="25550-151">By default, time to live is disabled by default in all Cosmos DB collections and on all documents.</span></span>

## <a name="enabling-ttl"></a><span data-ttu-id="25550-152">Élettartam engedélyezése</span><span class="sxs-lookup"><span data-stu-id="25550-152">Enabling TTL</span></span>
<span data-ttu-id="25550-153">Engedélyezze a TTL-t a gyűjteményt, vagy a dokumentumokat a gyűjteményen belül, a tulajdonsága DefaultTTL gyűjtemény vagy a -1, vagy a nullától eltérő pozitív számnak kell.</span><span class="sxs-lookup"><span data-stu-id="25550-153">To enable TTL on a collection, or the documents within a collection, you need to set the DefaultTTL property of a collection to either -1 or a non-zero positive number.</span></span> <span data-ttu-id="25550-154">Beállítása a DefaultTTL-1 érték azt jelenti, hogy a gyűjteményben lévő összes dokumentumot végtelen lesz élő alapértelmezett, de a Cosmos DB szolgáltatás által a gyűjteményhez, akik bírálták felül az alapértelmezett dokumentumok célszerű figyelemmel kísérni.</span><span class="sxs-lookup"><span data-stu-id="25550-154">Setting the DefaultTTL to -1 means that by default all documents in the collection will live forever but the Cosmos DB service should monitor this collection for documents that have overridden this default.</span></span>

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive =-1; //never expire by default

    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(databaseName),
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });

## <a name="configuring-default-ttl-on-a-collection"></a><span data-ttu-id="25550-155">A gyűjtemény alapértelmezett élettartam konfigurálása</span><span class="sxs-lookup"><span data-stu-id="25550-155">Configuring default TTL on a collection</span></span>
<span data-ttu-id="25550-156">Biztosan egy alapértelmezett élettartama a gyűjtemény szintjén konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="25550-156">You are able to configure a default time to live at a collection level.</span></span> <span data-ttu-id="25550-157">Egy gyűjtemény az élettartam beállításához meg kell adnia egy nullától eltérő pozitív szám, amely jelzi az időtartam (másodpercben), a gyűjteményben található összes dokumentum lejárati után utolsó módosításának időbélyeg a dokumentum (`_ts`).</span><span class="sxs-lookup"><span data-stu-id="25550-157">To set the TTL on a collection, you need to provide a non-zero positive number that indicates the period, in seconds, to expire all documents in the collection after the last modified timestamp of the document (`_ts`).</span></span> <span data-ttu-id="25550-158">Vagy az alapértelmezett -1, ami azt jelenti, hogy beszúrni a gyűjtemény összes dokumentumot határozatlan ideig fog élő alapértelmezés szerint állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="25550-158">Or, you can set the default to -1, which implies that all documents inserted in to the collection will live indefinitely by default.</span></span>

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive = 90 * 60 * 60 * 24; // expire all documents after 90 days
    
    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        "/dbs/salesdb",
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });


## <a name="setting-ttl-on-a-document"></a><span data-ttu-id="25550-159">A dokumentum TTL beállítása</span><span class="sxs-lookup"><span data-stu-id="25550-159">Setting TTL on a document</span></span>
<span data-ttu-id="25550-160">Egy gyűjtemény egy alapértelmezett élettartam beállítása mellett adott TTL dokumentum szinten állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="25550-160">In addition to setting a default TTL on a collection you can set specific TTL at a document level.</span></span> <span data-ttu-id="25550-161">Ez a művelet felülírja a gyűjtemény az alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="25550-161">Doing this will override the default of the collection.</span></span>

* <span data-ttu-id="25550-162">A dokumentum az élettartam beállításához meg kell adnia egy nullától eltérő pozitív szám, amely megadja, hogy az az időtartam (másodpercben), a dokumentum lejárati után utolsó módosításának időbélyeg a dokumentum (`_ts`).</span><span class="sxs-lookup"><span data-stu-id="25550-162">To set the TTL on a document, you need to provide a non-zero positive number which indicates the period, in seconds, to expire the document after the last modified timestamp of the document (`_ts`).</span></span>
* <span data-ttu-id="25550-163">Ha egy dokumentum TTL mező nem rendelkezik, a gyűjtemény az alapértelmezett fog vonatkozni.</span><span class="sxs-lookup"><span data-stu-id="25550-163">If a document has no TTL field, then the default of the collection will apply.</span></span>
* <span data-ttu-id="25550-164">Ha TTL le van tiltva, a gyűjtemény szintjén, a dokumentum TTL mezőjét figyelmen kívül TTL ismételt engedélyezéséig a gyűjteményen.</span><span class="sxs-lookup"><span data-stu-id="25550-164">If TTL is disabled at the collection level, the TTL field on the document will be ignored until TTL is enabled again on the collection.</span></span>

<span data-ttu-id="25550-165">Íme egy részlet bemutatja, hogyan állítsa be a TTL lejárata egy dokumentumon:</span><span class="sxs-lookup"><span data-stu-id="25550-165">Here's a snippet showing how to set the TTL expiration on a document:</span></span>

    // Include a property that serializes to "ttl" in JSON
    public class SalesOrder
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        
        [JsonProperty(PropertyName="cid")]
        public string CustomerId { get; set; }
        
        // used to set expiration policy
        [JsonProperty(PropertyName = "ttl", NullValueHandling = NullValueHandling.Ignore)]
        public int? TimeToLive { get; set; }
        
        //...
    }
    
    // Set the value to the expiration in seconds
    SalesOrder salesOrder = new SalesOrder
    {
        Id = "SO05",
        CustomerId = "CO18009186470",
        TimeToLive = 60 * 60 * 24 * 30;  // Expire sales orders in 30 days 
    };


## <a name="extending-ttl-on-an-existing-document"></a><span data-ttu-id="25550-166">A létező dokumentum TTL kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="25550-166">Extending TTL on an existing document</span></span>
<span data-ttu-id="25550-167">A dokumentum bármely írási művelet végrehajtásával alaphelyzetbe állíthatja a TTL-t, a dokumentumon.</span><span class="sxs-lookup"><span data-stu-id="25550-167">You can reset the TTL on a document by doing any write operation on the document.</span></span> <span data-ttu-id="25550-168">Ezzel állítja a `_ts` a jelenlegi időpontnál, és a dokumentum lejárati ideje beállított visszaszámlálás a `ttl`, megkezdődik a újra.</span><span class="sxs-lookup"><span data-stu-id="25550-168">Doing this will set the `_ts` to the current time, and the countdown to the document expiry, as set by the `ttl`, will begin again.</span></span> <span data-ttu-id="25550-169">Ha úgy szeretné módosítani a `ttl` egy dokumentum is frissítheti a mező, mint bármely más beállítható mező teheti.</span><span class="sxs-lookup"><span data-stu-id="25550-169">If you wish to change the `ttl` of a document, you can update the field as you can do with any other settable field.</span></span>

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = 60 * 30 * 30; // update time to live
    
    response = await client.ReplaceDocumentAsync(salesOrder);

## <a name="removing-ttl-from-a-document"></a><span data-ttu-id="25550-170">Egy dokumentum eltávolítása a TTL-t</span><span class="sxs-lookup"><span data-stu-id="25550-170">Removing TTL from a document</span></span>
<span data-ttu-id="25550-171">Ha már nem szeretné, hogy a dokumentum lejárati TTL be van állítva egy dokumentumot, majd a dokumentum beolvasása, távolítsa el a TTL mező és cserélje le a dokumentumot a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="25550-171">If a TTL has been set on a document and you no longer want that document to expire, then you can retrieve the document, remove the TTL field and replace the document on the server.</span></span> <span data-ttu-id="25550-172">Az élettartam mező törlődik a dokumentumot, a gyűjtemény az alapértelmezett lépnek érvénybe.</span><span class="sxs-lookup"><span data-stu-id="25550-172">When the TTL field is removed from the document, the default of the collection will be applied.</span></span> <span data-ttu-id="25550-173">Egy dokumentum lejár, és nem örököl a gyűjtemény majd be kell az élettartam értéke -1 értékre.</span><span class="sxs-lookup"><span data-stu-id="25550-173">To stop a document from expiring and not inherit from the collection then you need to set the TTL value to -1.</span></span>

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = null; // inherit the default TTL of the collection
    
    response = await client.ReplaceDocumentAsync(salesOrder);

## <a name="disabling-ttl"></a><span data-ttu-id="25550-174">Élettartam letiltása</span><span class="sxs-lookup"><span data-stu-id="25550-174">Disabling TTL</span></span>
<span data-ttu-id="25550-175">A TTL letiltása teljes egészében a gyűjtemény, majd állítsa le a háttérben folyamatot törölni kell a gyűjtemény DefaultTTL tulajdonsága lejárt dokumentumot keres.</span><span class="sxs-lookup"><span data-stu-id="25550-175">To disable TTL entirely on a collection and stop the background process from looking for expired documents the DefaultTTL property on the collection should be deleted.</span></span> <span data-ttu-id="25550-176">Ez a tulajdonság törlése eltér a -1 értékre állítaná.</span><span class="sxs-lookup"><span data-stu-id="25550-176">Deleting this property is different from setting it to -1.</span></span> <span data-ttu-id="25550-177">A beállítást, ha a-1 érték azt jelenti, hogy a gyűjteménybe felvett új dokumentumok végtelen lesz élő, de ez felülírható a gyűjtemény adott dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="25550-177">Setting to -1 means new documents added to the collection will live forever but you can override this on specific documents in the collection.</span></span> <span data-ttu-id="25550-178">Ez a tulajdonság teljesen eltávolítása a gyűjtemény azt jelenti, hogy a dokumentumok lejár, akkor is, ha vannak, akik explicit módon bírálták felül az előző alapértelmezett dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="25550-178">Removing this property entirely from the collection means that no documents will expire, even if there are documents that have explicitly overridden a previous default.</span></span>

    DocumentCollection collection = await client.ReadDocumentCollectionAsync("/dbs/salesdb/colls/orders");
    
    // Disable TTL
    collection.DefaultTimeToLive = null;
    
    await client.ReplaceDocumentCollectionAsync(collection);


## <a name="faq"></a><span data-ttu-id="25550-179">GYIK</span><span class="sxs-lookup"><span data-stu-id="25550-179">FAQ</span></span>
<span data-ttu-id="25550-180">**Mit fog költség TTL-t?**</span><span class="sxs-lookup"><span data-stu-id="25550-180">**What will TTL cost me?**</span></span>

<span data-ttu-id="25550-181">Nincs a TTL beállítást a dokumentum további költség nélkül.</span><span class="sxs-lookup"><span data-stu-id="25550-181">There is no additional cost to setting a TTL on a document.</span></span>

<span data-ttu-id="25550-182">**Mennyi ideig tart a dokumentum törlése után az élettartam működik-e?**</span><span class="sxs-lookup"><span data-stu-id="25550-182">**How long will it take to delete my document once the TTL is up?**</span></span>

<span data-ttu-id="25550-183">A dokumentumok lejárt azonnal, amennyiben a TTL-t, működik-e, és nem CRUD vagy a lekérdezés API-k elérhetők lesznek.</span><span class="sxs-lookup"><span data-stu-id="25550-183">The documents are expired immediately once the TTL is up, and will not be accessible via CRUD or query APIs.</span></span> 

<span data-ttu-id="25550-184">**Egy dokumentumon TTL semmilyen hatással lesz a RU díjak?**</span><span class="sxs-lookup"><span data-stu-id="25550-184">**Will TTL on a document have any impact on RU charges?**</span></span>

<span data-ttu-id="25550-185">Nem, nem lesznek nincs hatással az keresztül (élettartam) – Cosmos DB lejárt dokumentumok törlési RU költségek.</span><span class="sxs-lookup"><span data-stu-id="25550-185">No, there will be no impact on RU charges for deletions of expired documents via TTL in Cosmos DB.</span></span>

<span data-ttu-id="25550-186">**A TTL-szolgáltatás csak vonatkozik a teljes dokumentumot, vagy is szeretnék egyes dokumentum tulajdonságértékek lejárnak?**</span><span class="sxs-lookup"><span data-stu-id="25550-186">**Does the TTL feature only apply to entire documents, or can I expire individual document property values?**</span></span>

<span data-ttu-id="25550-187">TTL-t a teljes dokumentum vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="25550-187">TTL applies to the entire document.</span></span> <span data-ttu-id="25550-188">Ha azt szeretné, csak egy dokumentumot egy részének lejár, majd javasoljuk, hogy az a része kinyerése a fő dokumentum külön "kapcsolódó" dokumentumhoz, és majd használni a TTL-t a kibontott dokumentumon.</span><span class="sxs-lookup"><span data-stu-id="25550-188">If you would like to expire just a portion of a document, then it is recommended that you extract the portion from the main document in to a separate "linked” document and then use TTL on that extracted document.</span></span>

<span data-ttu-id="25550-189">**Rendelkezik a TTL-t a szolgáltatás indexelési különleges követelményeket?**</span><span class="sxs-lookup"><span data-stu-id="25550-189">**Does the TTL feature have any specific indexing requirements?**</span></span>

<span data-ttu-id="25550-190">Igen.</span><span class="sxs-lookup"><span data-stu-id="25550-190">Yes.</span></span> <span data-ttu-id="25550-191">Rendelkeznie kell a gyűjtemény [szabályzatokkal indexelő](indexing-policies.md) Consistent vagy Lazy.</span><span class="sxs-lookup"><span data-stu-id="25550-191">The collection must have [indexing policy set](indexing-policies.md) to either Consistent or Lazy.</span></span> <span data-ttu-id="25550-192">Az indexelő beállítása None szervezendő DefaultTTL beállítása közben hiba, mint fognak, kapcsolja ki a gyűjteményt, amely már be van állítva egy DefaultTTL az indexelő próbál eredményez.</span><span class="sxs-lookup"><span data-stu-id="25550-192">Trying to set DefaultTTL on a collection with indexing set to None will result in an error, as will trying to turn off indexing on a collection that has a DefaultTTL already set.</span></span>

## <a name="next-steps"></a><span data-ttu-id="25550-193">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="25550-193">Next steps</span></span>
<span data-ttu-id="25550-194">Azure Cosmos DB kapcsolatos további tudnivalókért tekintse meg a szolgáltatás [ *dokumentáció* ](https://azure.microsoft.com/documentation/services/cosmos-db/) lap.</span><span class="sxs-lookup"><span data-stu-id="25550-194">To learn more about Azure Cosmos DB, refer to the service [*documentation*](https://azure.microsoft.com/documentation/services/cosmos-db/) page.</span></span>

