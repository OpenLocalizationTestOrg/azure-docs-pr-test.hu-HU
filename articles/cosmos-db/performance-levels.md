---
title: "A DocumentDB API teljesítményszintet |} Microsoft Docs"
description: "További információk a hogyan DocumentDB API teljesítményszintek lehetővé teszik a tároló / alapon átviteli lefoglalni."
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
ms.openlocfilehash: c8d4733e57eb760dbb8e8ca96f6ba55671d1742f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="retiring-the-s1-s2-and-s3-performance-levels"></a>A S1, S2 és S3 teljesítményszintet kivonása

> [!IMPORTANT] 
> A cikkben szereplő S1, S2 és S3 teljesítményszintet használatból van, és már nem érhetők el az új DocumentDB API-fiókokat.
>

Ez a cikk áttekintést S1, S2 és S3 teljesítményszintet, és ismerteti, hogyan a gyűjteményeket, a teljesítmény szinteket használó telepíti át az egypartíciós gyűjtemények 2017 augusztus 1. a. A cikk elolvasása után képes lesz a következő kérdések megválaszolásához:

- [Miért van a S1, S2 és S3 teljesítmény szintek hatókörről?](#why-retired)
- [Hogyan hajtsa végre az egypartíciós gyűjtemények és a particionált gyűjtemények hasonlítsa össze a S1, S2, S3 teljesítményszintet?](#compare)
- [Mit kell tennie, az adatok folyamatos hozzáférés érdekében?](#uninterrupted-access)
- [Hogyan változik a saját gyűjteményembe az áttelepítést követően?](#collection-change)
- [Hogyan fogja módosítani a saját számlázási I vagyok történő áttelepítése után az egypartíciós gyűjtemények?](#billing-change)
- [Mi történik, ha több mint 10 GB tárhelyet kell?](#more-storage-needed)
- [Módosítható a S1, S2 és S3 között teljesítményszintet 2017. augusztus 1. előtt?](#change-before)
- [Milyen operációs rendszer, hogy ha a gyűjtemény át lett telepítve?](#when-migrated)
- [Hogyan tudom át az S1, S2, S3 teljesítményszintek az egypartíciós gyűjtemények önállóan?](#migrate-diy)
- [Hogyan vagyok feladatátvétele a Ha az ügyfél egy EA vagyok?](#ea-customer)

<a name="why-retired"></a>

## <a name="why-are-the-s1-s2-and-s3-performance-levels-being-retired"></a>Miért van a S1, S2 és S3 teljesítmény szintek hatókörről?

A S1, S2 és S3 teljesítményszintet biztosít, amely DocumentDB API gyűjtemények nyújt rugalmasságot. S1, S2, S3 teljesítményszintet, az átviteli sebesség és a tárolási kapacitás előre beállított és nem ajánlja fel a rugalmasság. Azure Cosmos DB kínál testreszabása az átviteli sebesség és tárterület, felkínálva sokkal nagyobb rugalmasságot biztosít arra, hogy a méretezés pedig az igényeinek módosítása.

<a name="compare"></a>

## <a name="how-do-single-partition-collections-and-partitioned-collections-compare-to-the-s1-s2-s3-performance-levels"></a>Hogyan hajtsa végre az egypartíciós gyűjtemények és a particionált gyűjtemények hasonlítsa össze a S1, S2, S3 teljesítményszintet?

Az alábbi táblázat összehasonlítja az átviteli sebesség és tárterület beállításait az egypartíciós gyűjtemények, a particionált gyűjtemények és S1, S2, S3 teljesítményszintet. Íme egy példa a amerikai keleti régiója 2 régió:

|   |Particionált gyűjtemény|Az egypartíciós gyűjtemény|S1|S2|S3|
|---|---|---|---|---|---|
|Maximális átviteli sebesség|Korlátlan|10 KB-os RU/mp|250 RU/mp|1 K RU/mp|2.5-K RU/mp|
|Minimális átviteli sebesség|2.5-K RU/mp|400 RU/mp|250 RU/mp|1 K RU/mp|2.5-K RU/mp|
|Maximális tárolóméret|Korlátlan|10 GB|10 GB|10 GB|10 GB|
|Árlista (havonta)|Átviteli sebesség: $6 / 100 RU/mp<br><br>Tárolás: $ 0,25/GB|Átviteli sebesség: $6 / 100 RU/mp<br><br>Tárolás: $ 0,25/GB|$25 USD|$50 USD|$100 USD|

Az ügyfél egy EA folytatja? Ha igen, tekintse meg a [hogyan vagyok I csökkentheti az EA felhasználóinál vagyok?](#ea-customer)

<a name="uninterrupted-access"></a>

## <a name="what-do-i-need-to-do-to-ensure-uninterrupted-access-to-my-data"></a>Mit kell tennie, az adatok folyamatos hozzáférés érdekében?

Semmi, Cosmos DB kezeli, az áttelepítés. Ha egy S1, S2 vagy S3 gyűjteményt, a jelenlegi gyűjtemény telepíti át a 2017. július 31 egypartíciós gyűjtemény. 

<a name="collection-change"></a>

## <a name="how-will-my-collection-change-after-the-migration"></a>Hogyan változik a saját gyűjteményembe az áttelepítést követően?

Ha egy S1 gyűjteményt, akkor telepíti át egy egypartíciós gyűjtemény átviteli sebességgel 400 RU/mp. 400 RU/mp a legalacsonyabb átviteli sebesség érhető el az egypartíciós gyűjtemények. Azonban a 400 RU/mp a költsége az egypartíciós gyűjtemény célja megközelítőleg azonos, amelyben meg volt fizető S1 gyűjteményének és 250 RU/mp –, így nem kell fizet az extra 150 RU/mp elérhető.

Ha egy S2 gyűjteményt, akkor telepíti át egy 1 KB-os RU/mp egypartíciós gyűjtemény. Nincs változás az átviteli szinten jelenik meg.

Ha egy S3 gyűjteményt, akkor telepíti át egy egypartíciós gyűjtemény 2,5 K RU/mp. Nincs változás az átviteli szinten jelenik meg.

Minden ezekben az esetekben miután áttelepítette a gyűjteményt, meg fog tudni az átviteli szintű testreszabása, vagy skálázhatja azt felfelé és lefelé alacsony késésű hozzáférést biztosítani a felhasználók igény szerint. Átviteli szintjének módosítása után a gyűjtemény át lett telepítve, egyszerűen nyissa meg a Cosmos DB fiók az Azure portálon, kattintson a Scale, válassza ki a gyűjteményt, és az alábbi képernyőfelvételen látható módon adja meg az átviteli szintű:

![Az Azure portálon átviteli méretezése](./media/performance-levels/portal-scale-throughput.png)

<a name="billing-change"></a>

## <a name="how-will-my-billing-change-after-im-migrated-to-the-single-partition-collections"></a>Hogyan fogja módosítani a saját számlázási I vagyok történő áttelepítése után az egypartíciós gyűjtemények?

Feltéve, hogy 10 S1 gyűjtemények, 1 GB tárhelyet minden, a US keleti terület rendelkezik, és ezek 10 S1 gyűjteményt telepít át, a 10 az egypartíciós gyűjtemények: 400 RU/mp (minimális). A számlázási következőképpen fog kinézni, ha a 10 az egypartíciós gyűjtemények esetében a teljes hónap:

![Hogyan S1 tarifacsomag 10 gyűjtemények összehasonlítja 10 gyűjtemények használatával az egypartíciós gyűjtemény díjszabása](./media/performance-levels/s1-vs-standard-pricing.png)

<a name="more-storage-needed"></a>

## <a name="what-if-i-need-more-than-10-gb-of-storage"></a>Mi történik, ha több mint 10 GB tárhelyet kell?

E rendelkezik egy gyűjtemény egy S1, S2 vagy S3 teljesítményszint szükséges, vagy ezek mindegyike rendelkezik, 10 GB-os kapacitású, a Cosmos DB adatáttelepítés eszközzel az adatok áttelepítéséhez egy particionált gyűjtemény gyakorlatilag az egypartíciós gyűjtemény korlátlan tárterület. A particionált gyűjtemény előnyeivel kapcsolatos információk: [particionálás és az Azure Cosmos Adatbázisba skálázás](documentdb-partition-data.md). A S1, S2, S3 vagy az egypartíciós gyűjtemény egy particionált gyűjtemény áttelepítésével kapcsolatos információkért lásd: [egypartíciós telepít át a particionált gyűjtemények](documentdb-partition-data.md#migrating-from-single-partition). 

<a name="change-before"></a>

## <a name="can-i-change-between-the-s1-s2-and-s3-performance-levels-before-august-1-2017"></a>Módosítható a S1, S2 és S3 között teljesítményszintet 2017. augusztus 1. előtt?

S1, S2 és S3 teljesítménnyel csak meglévő fiókok tudják módosítani, és módosítsa a teljesítmény szintű rétegek a portálon vagy programozottan. 2017. augusztus 1., amelyet a S1, S2 és S3 teljesítményszintet már nem lesz elérhető. Ha egy egypartíciós gyűjtemény S1, S3 vagy S3 módosítja, a S1, S2 vagy S3 teljesítményszintet nem lehet visszatérni.

<a name="when-migrated"></a>

## <a name="how-will-i-know-when-my-collection-has-migrated"></a>Milyen operációs rendszer, hogy ha a gyűjtemény át lett telepítve?

Az áttelepítés 2017. július 31 fordulhat elő. Ha a S1 alkalmazó gyűjteményt, S2 vagy S3 teljesítményszintet, a Cosmos DB csapata kapcsolatba lép Önnel e-mailben történik az áttelepítés előtt. Az áttelepítés befejezése után, a 2017. augusztus 1., az Azure-portálon jelenik meg, hogy a gyűjtemény használja, a Standard díjszabás.

![Bemutatja, hogyan ellenőrizheti a gyűjtemény át lett telepítve, a standard tarifacsomag](./media/performance-levels/portal-standard-pricing-applied.png)

<a name="migrate-diy"></a>

## <a name="how-do-i-migrate-from-the-s1-s2-s3-performance-levels-to-single-partition-collections-on-my-own"></a>Hogyan tudom át az S1, S2, S3 teljesítményszintek az egypartíciós gyűjtemények önállóan?

A S1, S2 és S3 teljesítményszintek az Azure portál használatával az egypartíciós gyűjtemények áttelepíthetők vagy programon keresztül. Ehhez a saját augusztus 1 rugalmas átviteli lehetőségekről az egypartíciós gyűjtemények kihasználják előtt, vagy áthelyezni, a gyűjtemények a 2017. július 31.

**Az Azure portál használata az egypartíciós gyűjtemények áttelepítése**

1. A a [ **Azure-portálon**](https://portal.azure.com), kattintson a **Azure Cosmos DB**, majd válassza ki a Cosmos DB fiók módosítása. 
 
    Ha **Azure Cosmos DB** értéke nem az Ugrósávon kattintson >, görgessen **adatbázisok**, jelölje be **Azure Cosmos DB**, majd válassza ki a DocumentDB-fiók.  

2. Erőforrás menüjében a **tárolók**, kattintson a **méretezési**, válassza ki a gyűjteményt, és kattintson a legördülő listából módosításához **árképzési szintjében**. Előre definiált átviteli használatával fiókok jogosultak az S1, S2 vagy S3 tarifacsomagot.  A a **válasszon tarifacsomagot** panelen kattintson **szabványos** felhasználói átviteli módosítsa, majd **válasszon** menteni a módosítást.

    ![A beállítások panelről, hol változtatható meg az átviteli sebesség értéket ábrázoló képernyőfelvétel](./media/performance-levels/change-performance-set-thoughput.png)

3. Vissza a **méretezési** panelen a **Tarifacsomagot** változott **szabványos** és a **átviteli sebesség (RU/mp)** mezőben jelenik meg az alapértelmezett érték 400 értéke. Állítsa be az átviteli sebesség 400 és 10 000 között [egységek kérelem](request-units.md)/second (RU/mp). A **havi számla becsült** a lap frissítések, automatikusan a havi költségeket becsült alján. 

    >[!IMPORTANT] 
    > Miután menti a módosításokat, és helyezze át a Standard tarifacsomag, nem állítható vissza a S1, S2 vagy S3 teljesítmény szintre.

4. Kattintson a **mentése** menti a módosításokat.

    Ha azt állapítja meg, hogy van szüksége további átviteli sebesség (nagyobb, mint 10000 RU/mp) vagy további tárhelyet (10GB-nál nagyobb) particionált gyűjtemény hozható létre. Az egypartíciós gyűjtemény egy particionált gyűjtemény áttelepítéséhez lásd: [egypartíciós telepít át a particionált gyűjtemények](documentdb-partition-data.md#migrating-from-single-partition).

    > [!NOTE]
    > Standard S1, S2 vagy S3 módosítása 2 percet is igénybe vehet.
    > 
    > 

**A .NET SDK használatával az egypartíciós gyűjtemények áttelepítése**

A gyűjtemények teljesítményszintet módosítására vonatkozóan egy másik lehetőség az SDK-k keresztül történik. Ez a szakasz csak hozzá van rendelve egy gyűjtési teljesítmény módosítása szinten használatával a [DocumentDB .NET API](documentdb-sdk-dotnet.md), de a folyamat hasonló, ha az egyéb SDK-k.

Íme egy kódrészletet a a gyűjtemény átviteli sebességének módosítása a 5 000 kérelemegység / másodperc:
    
```C#
    //Fetch the resource to be updated
    Offer offer = client.CreateOfferQuery()
                      .Where(r => r.ResourceLink == collection.SelfLink)    
                      .AsEnumerable()
                      .SingleOrDefault();

    // Set the throughput to 5000 request units per second
    offer = new OfferV2(offer, 5000);

    //Now persist these changes to the database by replacing the original resource
    await client.ReplaceOfferAsync(offer);
```

Látogasson el [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.aspx) további példákat és a további tudnivalók az ajánlat módszerek:

* [**ReadOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readofferasync.aspx)
* [**ReadOffersFeedAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.readoffersfeedasync.aspx)
* [**ReplaceOfferAsync**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.replaceofferasync.aspx)
* [**CreateOfferQuery**](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.documentqueryable.createofferquery.aspx)

<a name="ea-customer"></a>

## <a name="how-am-i-impacted-if-im-an-ea-customer"></a>Hogyan vagyok feladatátvétele a Ha az ügyfél egy EA vagyok?

Nagyvállalati ügyfelek az aktuális szerződés végéig védett ár is.

## <a name="next-steps"></a>Következő lépések
Tarifa- és Azure Cosmos DB adatok kezelésével kapcsolatos további tudnivalókért ismerheti meg ezeket az erőforrásokat:

1.  [Particionálás adatokat az Adatbázisba az Cosmos](documentdb-partition-data.md). Az egypartíciós tároló particionált tárolók, valamint tippek az zökkenőmentesen méretezési jó particionálási stratégia megvalósítása közötti különbségek megértése.
2.  [Cosmos DB árképzési](https://azure.microsoft.com/pricing/details/cosmos-db/). További információk a telepítés átviteli sebesség és a tároló felhasználása költsége.
3.  [Egységek kérelem](request-units.md). Ismerje meg, a felhasználás átviteli különböző művelet-típusok, például a rekordhoz olvasási, írási, lekérdezés.
