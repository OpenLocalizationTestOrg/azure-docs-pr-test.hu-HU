---
title: "aaaDocumentDB API teljesítményszintet |} Microsoft Docs"
description: "További információk a hogyan DocumentDB API teljesítményszintek lehetővé teszik a tooreserve átviteli / tároló alapon."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 7dc21c71-47e2-4e06-aa21-e84af52866f4
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: mimig
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 716bc11ae238dbb0feebf004ed8d5f8a7515ec6f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="retiring-hello-s1-s2-and-s3-performance-levels"></a>Hello S1, S2 és S3 teljesítményszintet kivonása

> [!IMPORTANT] 
> hello S1, S2 és S3 teljesítményszintet cikkben említett használatból van, és már nem érhetők el az új DocumentDB API-fiókokat.
>

Ez a cikk áttekintést S1, S2 és S3 teljesítményszintet, és ismerteti, hogyan hello gyűjteményeket, a teljesítmény szinteket használó lesz az áttelepített toosingle partíció gyűjtemények 2017 augusztus 1.. A cikk elolvasása után be fog tudni tooanswer hello a következő kérdéseket:

- [Miért van hello S1, S2 és S3 teljesítmény szintek hatókörről?](#why-retired)
- [Hogyan hajtsa végre az egypartíciós gyűjtemények és a particionált gyűjtemények összehasonlítása toohello S1, S2, S3 teljesítményszintet?](#compare)
- [Mi készíthetek kell toodo tooensure folyamatos toomy adatok eléréséhez?](#uninterrupted-access)
- [Hogyan változik a saját gyűjteményembe hello áttelepítése után?](#collection-change)
- [Hogyan fogja módosítani a a számlázási után áttelepített toosingle partíció gyűjtemények vagyok?](#billing-change)
- [Mi történik, ha több mint 10 GB tárhelyet kell?](#more-storage-needed)
- [Módosítható hello S1, S2 és S3 teljesítményszintet 2017. augusztus 1. előtt között?](#change-before)
- [Milyen operációs rendszer, hogy ha a gyűjtemény át lett telepítve?](#when-migrated)
- [Hogyan hello S1, S2, S3 teljesítmény szintek toosingle partíció gyűjtemények önállóan át?](#migrate-diy)
- [Hogyan vagyok feladatátvétele a Ha az ügyfél egy EA vagyok?](#ea-customer)

<a name="why-retired"></a>

## <a name="why-are-hello-s1-s2-and-s3-performance-levels-being-retired"></a>Miért van hello S1, S2 és S3 teljesítmény szintek hatókörről?

hello S1, S2 és S3 teljesítményszintet nem ajánlat hello rugalmasságot biztosít, amely DocumentDB API gyűjtemények nyújt, hajtsa végre. Hello S1, S2, S3 teljesítményszintet, mindkét hello átviteli sebesség és a tárolási kapacitás előre beállított és nem ajánlja fel a rugalmasság. Azure Cosmos DB kínál: hello képességét toocustomize az átviteli sebesség és tárterület, felkínálva a lehetőséget tooscale sokkal nagyobb rugalmasságot igényeinek módosítása.

<a name="compare"></a>

## <a name="how-do-single-partition-collections-and-partitioned-collections-compare-toohello-s1-s2-s3-performance-levels"></a>Hogyan hajtsa végre az egypartíciós gyűjtemények és a particionált gyűjtemények összehasonlítása toohello S1, S2, S3 teljesítményszintet?

a következő táblázat hello hello átviteli sebesség és tárterület rendelkezésre álló lehetőségek az egypartíciós gyűjtemények, a particionált gyűjtemények és S1, S2, S3 teljesítményszintet hasonlítja össze. Íme egy példa a amerikai keleti régiója 2 régió:

|   |Particionált gyűjtemény|Az egypartíciós gyűjtemény|S1|S2|S3|
|---|---|---|---|---|---|
|Maximális átviteli sebesség|Korlátlan|10 KB-os RU/mp|250 RU/mp|1 K RU/mp|2.5-K RU/mp|
|Minimális átviteli sebesség|2.5-K RU/mp|400 RU/mp|250 RU/mp|1 K RU/mp|2.5-K RU/mp|
|Maximális tárolóméret|Korlátlan|10 GB|10 GB|10 GB|10 GB|
|Árlista (havonta)|Átviteli sebesség: $6 / 100 RU/mp<br><br>Tárolás: $ 0,25/GB|Átviteli sebesség: $6 / 100 RU/mp<br><br>Tárolás: $ 0,25/GB|$25 USD|$50 USD|$100 USD|

Az ügyfél egy EA folytatja? Ha igen, tekintse meg a [hogyan vagyok I csökkentheti az EA felhasználóinál vagyok?](#ea-customer)

<a name="uninterrupted-access"></a>

## <a name="what-do-i-need-toodo-tooensure-uninterrupted-access-toomy-data"></a>Mi készíthetek kell toodo tooensure folyamatos toomy adatok eléréséhez?

Semmi, Cosmos DB hello áttelepítési az Ön kezeli. Ha egy S1, S2 vagy S3 gyűjteményt, a jelenlegi gyűjtemény lesz áttelepített tooa egypartíciós gyűjtemény 2017. július 31. 

<a name="collection-change"></a>

## <a name="how-will-my-collection-change-after-hello-migration"></a>Hogyan változik a saját gyűjteményembe hello áttelepítése után?

Ha egy S1 gyűjteményt, fogja áttelepített tooa egypartíciós gyűjtemény 400 RU/s átviteli sebességgel. 400 RU/mp hello legalacsonyabb átviteli sebesség érhető el az egypartíciós gyűjtemények. Az egypartíciós gyűjtemény körülbelül megegyezik az Ön volt az S1 gyűjteménnyel fizető hello hello 400 RU/mp és 250 RU/mp – így a nem fizet hello költsége azonban nagyon 150 RU/mp elérhető tooyou hello.

Ha egy S2 gyűjteményt, fogja áttelepített tooa egypartíciós gyűjtemény 1 K RU/mp. Nincs változás tooyour átviteli szint jelenik meg.

Ha egy S3 gyűjteményt, fogja áttelepített tooa egypartíciós gyűjtemény 2,5 K RU/mp. Nincs változás tooyour átviteli szint jelenik meg.

Az egyes ezekben az esetekben miután áttelepítette a gyűjteményt, hoz kell tudni toocustomize az átviteli szintű, vagy szükséges tooprovide alacsony késésű hozzáférést tooyour felhasználóként vertikális fel-le azt. toochange hello átviteli szintű után a gyűjtemény át lett telepítve, egyszerűen nyissa meg a Cosmos DB fiók hello Azure-portálon, kattintson a Scale, válassza ki a gyűjtemény és hello átviteli szint, majd beállításához, ahogy az alábbi képernyőfelvétel a hello:

![Hogyan tooscale átviteli sebesség mértéke a hello Azure-portálon](./media/performance-levels/portal-scale-throughput.png)

<a name="billing-change"></a>

## <a name="how-will-my-billing-change-after-im-migrated-toohello-single-partition-collections"></a>Hogyan fogja módosítani a a számlázási után áttelepített toohello az egypartíciós gyűjtemények vagyok?

Feltéve, hogy 10 S1 gyűjtemények, 1 GB tárhelyet minden hello amerikai keleti terület, és a gyűjteményt telepít át, a 10 S1 gyűjtemények too10 egypartíciós: 400 RU/mp (hello minimális szintet). A számlázási a Ha a hello 10 az egypartíciós gyűjtemények esetében a teljes hónap következőképpen fog kinézni:

![Hogyan S1 tarifacsomag 10 gyűjtemények hasonlítja össze a too10 gyűjtemények használatával az egypartíciós gyűjtemény díjszabása](./media/performance-levels/s1-vs-standard-pricing.png)

<a name="more-storage-needed"></a>

## <a name="what-if-i-need-more-than-10-gb-of-storage"></a>Mi történik, ha több mint 10 GB tárhelyet kell?

Rendelkezik egy gyűjtemény egy S1, S2 vagy S3 teljesítményszint szükséges, vagy az egypartíciós gyűjtemény, ezek mindegyike rendelkezik, 10 GB tárterület érhető el, hogy az adatok tooa particionált gyűjtemény gyakorlatilag hello Cosmos DB adatáttelepítési eszköz toomigrate használhatja korlátlan tárterület. Egy particionált gyűjtemény hello előnyöket kapcsolatos információk: [particionálás és az Azure Cosmos Adatbázisba skálázás](documentdb-partition-data.md). További információ toomigrate a S1, S2, S3 vagy az egypartíciós gyűjtemény tooa particionált gyűjtemény, lásd: [toopartitioned egypartíciós gyűjtemények áttelepítése](documentdb-partition-data.md#migrating-from-single-partition). 

<a name="change-before"></a>

## <a name="can-i-change-between-hello-s1-s2-and-s3-performance-levels-before-august-1-2017"></a>Módosítható hello S1, S2 és S3 teljesítményszintet 2017. augusztus 1. előtt között?

S1, S2 és S3 teljesítménnyel csak meglévő fiókok képes toochange kell, és a teljesítmény szintű rétegek hello portálon vagy programozottan az alter. 2017. augusztus 1. hello S1, S2, és S3 teljesítményszintet többé nem érhető el. S1, S3 vagy S3 tooa egypartíciós gyűjtemény módosításakor S1, S2 vagy S3 teljesítményszintet toohello nem lehet visszatérni.

<a name="when-migrated"></a>

## <a name="how-will-i-know-when-my-collection-has-migrated"></a>Milyen operációs rendszer, hogy ha a gyűjtemény át lett telepítve?

hello áttelepítési 2017. július 31 fordulhat elő. Ha a gyűjtemény hello S1, S2 használó, vagy S3 teljesítményszintet, hello Cosmos DB team felveszi Önnel a e-mailben történik hello áttelepítés előtt. Hello áttelepítés akkor fejeződött be, a 2017. augusztus 1., hello Azure-portálon fognak megjelenni, hogy a gyűjtemény használja, a Standard díjszabás.

![Hogyan tooconfirm a gyűjtemény már áttelepített toohello a Standard tarifacsomag](./media/performance-levels/portal-standard-pricing-applied.png)

<a name="migrate-diy"></a>

## <a name="how-do-i-migrate-from-hello-s1-s2-s3-performance-levels-toosingle-partition-collections-on-my-own"></a>Hogyan hello S1, S2, S3 teljesítmény szintek toosingle partíció gyűjtemények önállóan át?

Hello S1, S2, telepíthet át, és S3 teljesítmény szinteken toosingle partíció gyűjtemények használatával hello Azure-portálon vagy programozottan. Ehhez a saját előtt augusztus 1 toobenefit hello rugalmas átviteli közül az egypartíciós gyűjtemények érhető el, vagy a gyűjteményeket, a 2017. július 31 áthelyezni.

**toomigrate toosingle partíció gyűjtemények hello Azure-portál használatával**

1. A hello [ **Azure-portálon**](https://portal.azure.com), kattintson a **Azure Cosmos DB**, majd válassza ki a hello Cosmos DB fiók toomodify. 
 
    Ha **Azure Cosmos DB** még nincs a hello Ugrósávon, kattintson a >, görgessen túl**adatbázisok**, jelölje be **Azure Cosmos DB**, majd válassza ki a hello DocumentDB-fiók.  

2. Hello erőforrás menü alatti **tárolók**, kattintson **méretezési**, válassza ki a hello gyűjtemény toomodify hello legördülő listából, és kattintson **Tarifacsomagot**. Előre definiált átviteli használatával fiókok jogosultak az S1, S2 vagy S3 tarifacsomagot.  A hello **válasszon tarifacsomagot** panelen kattintson a **szabványos** toochange toouser által megadott átviteli sebességet, és kattintson a **válassza ki** toosave a módosítás.

    ![Képernyőfelvétel a hello panelen – képernyőfelvétel, amelyen toochange hello átviteli érték](./media/performance-levels/change-performance-set-thoughput.png)

3. Vissza a hello **méretezési** panelen, hello **Tarifacsomagot** túl megváltozott**szabványos** és hello **átviteli sebesség (RU/mp)** mezőben jelenik meg, amely egy 400 alapértelmezett értéke. Set hello átviteli sebesség 400 és 10 000 között [egységek kérelem](request-units.md)/second (RU/mp). Hello **havi számla becsült** hello hello alján lap automatikusan frissül, tooprovide havi költségeket hello becslése. 

    >[!IMPORTANT] 
    > Miután menti a módosításokat, és helyezze át a Standard tarifacsomag toohello, nem állítható vissza toohello S1, S2 vagy S3 teljesítményszintet.

4. Kattintson a **mentése** toosave a módosításokat.

    Ha azt állapítja meg, hogy van szüksége további átviteli sebesség (nagyobb, mint 10000 RU/mp) vagy további tárhelyet (10GB-nál nagyobb) particionált gyűjtemény hozható létre. az egypartíciós gyűjtemény tooa particionált gyűjtemény toomigrate lásd: [toopartitioned egypartíciós gyűjtemények áttelepítése](documentdb-partition-data.md#migrating-from-single-partition).

    > [!NOTE]
    > S1, S2 vagy S3 tooStandard korlátlanról too2 percet igénybe vehet.
    > 
    > 

**toomigrate toosingle partíció gyűjtemények hello .NET SDK használatával**

A gyűjtemények teljesítményszintet módosítására vonatkozóan egy másik lehetőség az SDK-k keresztül történik. Ez a szakasz csak hozzá van rendelve egy gyűjtési teljesítmény módosítása szinten használatával a [DocumentDB .NET API](documentdb-sdk-dotnet.md), hello folyamata hasonló, ha a Csomagjától, de.

Íme egy kódrészletet módosítására hello gyűjtemény átviteli too5 000 kérelemegység / másodperc:
    
```C#
    //Fetch hello resource toobe updated
    Offer offer = client.CreateOfferQuery()
                      .Where(r => r.ResourceLink == collection.SelfLink)    
                      .AsEnumerable()
                      .SingleOrDefault();

    // Set hello throughput too5000 request units per second
    offer = new OfferV2(offer, 5000);

    //Now persist these changes toohello database by replacing hello original resource
    await client.ReplaceOfferAsync(offer);
```

Látogasson el [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) tooview további példákat és a további tudnivalók az ajánlat módszerek:

* [**ReadOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readofferasync.aspx)
* [**ReadOffersFeedAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readoffersfeedasync.aspx)
* [**ReplaceOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replaceofferasync.aspx)
* [**CreateOfferQuery**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createofferquery.aspx)

<a name="ea-customer"></a>

## <a name="how-am-i-impacted-if-im-an-ea-customer"></a>Hogyan vagyok feladatátvétele a Ha az ügyfél egy EA vagyok?

Nagyvállalati ügyfelek az aktuális szerződés hello végéig védett ár is.

## <a name="next-steps"></a>Következő lépések
További információ az árak és Azure Cosmos DB, az adatok kezelése toolearn ismerheti meg ezeket az erőforrásokat:

1.  [Particionálás adatokat az Adatbázisba az Cosmos](documentdb-partition-data.md). Ismerje meg az egypartíciós tároló particionált tárolók, valamint a particionálási stratégia tooscale zökkenőmentesen végrehajtási tippek hello különbsége.
2.  [Cosmos DB árképzési](https://azure.microsoft.com/pricing/details/cosmos-db/). További információk a telepítés átviteli sebesség és a tároló felhasználása hello költség.
3.  [Egységek kérelem](request-units.md). Ismerje meg különböző művelet-típusok, például a rekordhoz olvasási, írási, lekérdezés átviteli hello fogyasztás.
