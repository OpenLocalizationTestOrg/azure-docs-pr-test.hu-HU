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
# <a name="expire-data-in-azure-cosmos-db-collections-automatically-with-time-toolive"></a>Automatikusan toolive idővel rendelkező Azure Cosmos DB gyűjteményekben lévő adatok lejárnak
Alkalmazások létrehozására és hatalmas mennyiségű adat tárolására is használható. Az adatok egy részét, például generált gép esemény adatokat, a naplókat, és a felhasználói munkamenet az információ csak véges időn. Után hello adatok válnak hello alkalmazás igényeinek felesleges toohello biztonságos toopurge ezeket az adatokat, és csökkentse egy alkalmazás hello tárolási igényeit.

"Toolive idő" vagy a TTL-t a Microsoft Azure Cosmos DB nyújt hello képességét toohave dokumentumok automatikusan törlődnek hello adatbázis egy bizonyos idő eltelte után. hello alapértelmezett idő toolive hello gyűjtemény szinten adja meg, és dokumentumra alapon felülbírálható. Élettartam beállítása, a gyűjtemény alapértelmezett vagy a dokumentum szinten után Cosmos DB automatikusan eltávolítja a dokumentumok létező adott időn belül, (másodpercben), mert utolsó módosítás.

A Cosmos DB toolive idő hello dokumentum utolsó módosításának elleni eltolás használja. toodo ezt használja hello `_ts` mező, amelyben megtalálható minden a dokumentumban. hello _ts mezője unix típusú epoch időbélyeg képviselő hello dátumot és időpontot. Hello `_ts` mező frissül minden alkalommal, amikor a dokumentum módosításakor. 

## <a name="ttl-behavior"></a>Élettartam viselkedése
hello TTL szolgáltatás TTL tulajdonságok két szintű - hello gyűjtemény és hello dokumentum vezérli. hello értékeket másodpercben van beállítva, és a hello különbözeti tekintendők `_ts` hello dokumentum utolsó módosításának következő.

1. Hello gyűjtemény DefaultTTL
   
   * Ha a hiányzó (vagy set toonull), dokumentumok nem törlődnek automatikusan.
   * Ha van ilyen hello értéke pedig "-1" = végtelen – dokumentumok alapértelmezés szerint nem jár le
   * Ha van ilyen és hello érték egy szám ("n") – a dokumentumok lejárnak "n" másodperc után utolsó módosítás
2. Hello dokumentumok élettartam: 
   
   * A tulajdonság csak DefaultTTL jelen a hello szülőgyűjtemény vonatkozik.
   * Felülbírálja a hello szülőgyűjtemény hello DefaultTTL értékét.

Amint hello dokumentum lejárt (`ttl`  +  `_ts` > = aktuális server idő), "lejárt" hello dokumentum van jelölve. Nincs művelet után most kapnak a dokumentumokon, és azok nem kerülnek bele az hello végrehajtott lekérdezések eredményeit. hello dokumentumok hello rendszer fizikailag el kell hagyni, és törli hello háttérben szerint egy későbbi időpontban. Ez nem foglal le a [kérelem egységek (RUs)](request-units.md) hello gyűjtemény költségvetés.

hello fent logika mutatja be a következő mátrix hello:

|  | DefaultTTL hiányzik vagy nem hello gyűjtemény beállítása | DefaultTTL = -1-gyűjteménytől | DefaultTTL = "n" gyűjteménytől |
| --- |:--- |:--- |:--- |
| A dokumentum hiányzó élettartam |Semmi toooverride hello dokumentum és a gyűjtemény tartozik TTL fogalma dokumentum szinten. |Ebben a gyűjteményben nem dokumentumok lejárnak. |a gyűjtemény hello dokumentumok amikor időköz n lejárta lejár. |
| Élettartam dokumentum -1 = |Semmi hello dokumentum szinten óta hello gyűjtemény nem ad meg toooverride hello DefaultTTL tulajdonság, amely egy dokumentum lehet felülbírálni. A dokumentum a TTL nem értelmezett hello rendszer. |Ebben a gyűjteményben nem dokumentumok lejárnak. |az élettartam =-1. a gyűjtemény hello dokumentum soha nem jár le. Minden más dokumentum "n" időszak után lejár. |
| Élettartam dokumentumon n = |Semmi toooverride hello dokumentum szinten. A dokumentumban TTL nem értelmezett hello rendszer. |Élettartam hello dokumentum = n időköz n másodperc után lejár. Egyéb dokumentumokat fog örökli időintervalluma -1, és nem jár le. |Élettartam hello dokumentum = n időköz n másodperc után lejár. Egyéb dokumentumokat öröklik a "n" időköz hello gyűjteményből. |

## <a name="configuring-ttl"></a>Élettartam konfigurálása
Alapértelmezés szerint toolive idő összes Cosmos DB gyűjtemények, valamint minden dokumentumok alapértelmezés szerint le van tiltva.

## <a name="enabling-ttl"></a>Élettartam engedélyezése
tooenable TTL-t a gyűjteményt, vagy egy gyűjteményen belül hello dokumentumok, tooset hello DefaultTTL tulajdonság egy gyűjtemény tooeither -1 vagy nullától eltérő pozitív számnak kell. Hello DefaultTTL túl-1 értékre, hogy alapértelmezés szerint hello gyűjteményben lévő összes dokumentumot végtelen lesz élő, de hello Cosmos DB szolgáltatás célszerű figyelemmel kísérni a gyűjteményhez, akik bírálták felül az alapértelmezett dokumentumok beállítása.

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive =-1; //never expire by default

    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(databaseName),
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });

## <a name="configuring-default-ttl-on-a-collection"></a>A gyűjtemény alapértelmezett élettartam konfigurálása
Biztosan tudja tooconfigure toolive alapértelmezett időpontot, a gyűjtemény szintjén. tooset hello TTL-t egy gyűjteményen kell tooprovide nullától eltérő pozitív szám, amely jelzi a hello időtartam (másodpercben), tooexpire hello gyűjteményben lévő összes dokumentumot után hello utolsó módosítás hello dokumentum időbélyegzője (`_ts`). Másik lehetőségként hello alapértelmezett túl-1, ami azt jelenti, hogy toohello gyűjteményben lévő összes dokumentumok határozatlan ideig fog élő alapértelmezés szerint állíthatja be.

    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.Id = "orders";
    collectionDefinition.PartitionKey.Paths.Add("/customerId");
    collectionDefinition.DefaultTimeToLive = 90 * 60 * 60 * 24; // expire all documents after 90 days
    
    DocumentCollection ttlEnabledCollection = await client.CreateDocumentCollectionAsync(
        "/dbs/salesdb",
        collectionDefinition,
        new RequestOptions { OfferThroughput = 20000 });


## <a name="setting-ttl-on-a-document"></a>A dokumentum TTL beállítása
Továbbá a gyűjtemény alapértelmezett élettartam toosetting állíthatja be adott TTL dokumentum szinten. Ez a művelet felülírja hello alapértelmezett hello gyűjtemény.

* tooset hello TTL-t egy dokumentum, egy nem nulla pozitív szám, amely megadja, hogy az az időszak (másodpercben), tooexpire hello dokumentum hello utolsó hello dokumentum időbélyeg módosítása után hello tooprovide szüksége (`_ts`).
* Ha egy dokumentum nem TTL mezőben, majd az alapértelmezett hello gyűjtemény hello érvényesek.
* Élettartam hello adatgyűjtési szint le van tiltva, ha hello TTL mező hello dokumentum figyelmen kívül TTL ismételt engedélyezéséig hello gyűjteményen.

Íme egy részlet jelenít meg, hogyan tooset hello TTL lejárata egy dokumentumon:

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


## <a name="extending-ttl-on-an-existing-document"></a>A létező dokumentum TTL kiterjesztése
Bármely dokumentum hello írási művelet végrehajtásával alaphelyzetbe állíthatja a hello TTL-t egy dokumentumon. Ezzel fogja beállítani hello `_ts` toohello aktuális idő és hello visszaszámlálási toohello dokumentum lejárati, hello beállított `ttl`, megkezdődik a újra. Ha toochange hello `ttl` egy dokumentum is frissítheti hello mező, mint bármely más beállítható mező teheti.

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = 60 * 30 * 30; // update time toolive
    
    response = await client.ReplaceDocumentAsync(salesOrder);

## <a name="removing-ttl-from-a-document"></a>Egy dokumentum eltávolítása a TTL-t
Ha már nem szeretné, hogy a dokumentum tooexpire TTL be van állítva egy dokumentumot, majd hello dokumentum beolvasása, távolítsa el hello TTL mező és hello kiszolgálón hello dokumentum cseréje. Hello TTL mező hello dokumentum törlődik, hello alapértelmezett hello gyűjtemény lépnek érvénybe. toostop lejárjanak dokumentum és nem örököl a hello gyűjtemény tooset hello TTL érték túl-1 kell használnia.

    response = await client.ReadDocumentAsync(
        "/dbs/salesdb/colls/orders/docs/SO05"), 
        new RequestOptions { PartitionKey = new PartitionKey("CO18009186470") });
    
    Document readDocument = response.Resource;
    readDocument.TimeToLive = null; // inherit hello default TTL of hello collection
    
    response = await client.ReplaceDocumentAsync(salesOrder);

## <a name="disabling-ttl"></a>Élettartam letiltása
toodisable teljesen a egy adatgyűjtési és -leállítási hello háttér TTL törölni kell a hello DefaultTTL tulajdonság hello gyűjteményen lejárt dokumentumot keres a feldolgozni. Ez a tulajdonság törlése eltér túl 1 – állítaná. Túl-1 értékre beállítása esetén az új dokumentumok hozzáadott toohello gyűjtemény lesz élő végtelen, de ez felülírható hello gyűjtemény adott dokumentumokat. Ez a tulajdonság teljesen hello gyűjteményből eltávolítása azt jelenti, hogy a dokumentumok lejár, akkor is, ha vannak, akik explicit módon bírálták felül az előző alapértelmezett dokumentumok.

    DocumentCollection collection = await client.ReadDocumentCollectionAsync("/dbs/salesdb/colls/orders");
    
    // Disable TTL
    collection.DefaultTimeToLive = null;
    
    await client.ReplaceDocumentCollectionAsync(collection);


## <a name="faq"></a>GYIK
**Mit fog költség TTL-t?**

Nem jelent többletköltséget toosetting egy dokumentumon TTL van.

**Mennyi ideig tart toodelete a dokumentum után hello TTL működik-e?**

hello dokumentumok lejárt után azonnal hello TTL működik-e, és nem CRUD vagy a lekérdezés API-k elérhetők lesznek. 

**Egy dokumentumon TTL semmilyen hatással lesz a RU díjak?**

Nem, nem lesznek nincs hatással az keresztül (élettartam) – Cosmos DB lejárt dokumentumok törlési RU költségek.

**Hello TTL-szolgáltatás csak vonatkozik tooentire dokumentumok, vagy lejárhat I egyes dokumentum tulajdonságértékek?**

Élettartam toohello a teljes dokumentum vonatkozik. Ha azt szeretné, hogy tooexpire egy részét egy dokumentumot, akkor az ajánlott hello részét kinyerése hello fő dokumentum tooa külön "kapcsolódó" dokumentum, és kövesse a kibontott dokumentumon TTL-t.

**Rendelkezik a hello TTL-t a szolgáltatás indexelési különleges követelményeket?**

Igen. rendelkeznie kell hello gyűjtemény [szabályzatokkal indexelő](indexing-policies.md) tooeither Consistent vagy Lazy. Az indexelő set tooNone szervezendő DefaultTTL tooset próbált azt eredményezi, hogy hiba, mint ki egy gyűjteményt, amely már be van állítva egy DefaultTTL az indexelés közben tooturn fognak.

## <a name="next-steps"></a>Következő lépések
toolearn Azure Cosmos DB, kapcsolatos további információkért tekintse meg a toohello szolgáltatás [ *dokumentáció* ](https://azure.microsoft.com/documentation/services/cosmos-db/) lap.

