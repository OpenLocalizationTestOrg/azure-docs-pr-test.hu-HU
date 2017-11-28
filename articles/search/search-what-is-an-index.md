---
title: "az Azure Search-index aaaCreate |} A Microsoft Azure |} Üzemeltetett felhőalapú keresőszolgáltatás"
description: "Mi az Azure Search-index, és hogyan használható?"
services: search
documentationcenter: 
author: ashmaka
ms.assetid: a395e166-bf2e-4fca-8bfc-116a46c5f7b1
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 12/08/2016
ms.author: ashmaka
ms.openlocfilehash: c01cc654ff91427c8f1569b2f5b060a0a0f044c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-search-index"></a>Azure Search-index létrehozása
> [!div class="op_single_selector"]
> * [Áttekintés](search-what-is-an-index.md)
> * [Portál](search-create-index-portal.md)
> * [.NET](search-create-index-dotnet.md)
> * [REST](search-create-index-rest-api.md)
> 
> 

## <a name="what-is-an-index"></a>Mit jelent az index?
Az *index* az Azure Search szolgáltatás által használt *dokumentumok* és egyéb szerkezetek állandó tárolója. A dokumentum az indexben lévő kereshető adatok egyedi egysége. Az elektronikus kereskedelemmel foglalkozó ügyfelek például minden egyes értékesített áru, egy hírközlő szervezet pedig minden egyes cikk esetében rendelkezhet dokumentumokkal. Ezen fogalmakat toomore ismerős adatbázis alakokat leképezési: egy *index* fogalmilag hasonló tooa van *tábla*, és *dokumentumok* nagyjából megfelel túl*sorok* egy táblázatban.

Ha Ön hozzáadása/feltöltése dokumentumok és küldje el a keresési lekérdezések tooAzure keresési, elküldését a kérelmek tooa egyedi indexet a keresési szolgáltatás.

## <a name="field-types-and-attributes-in-an-azure-search-index"></a>Mezőtípusok és -attribútumok egy Azure Search-indexben
Mivel a séma határozza meg, meg kell adnia hello neve, típusa és attribútumok minden mező az indexben. hello mező típusa, a mező tárolt adatok hello osztályozza. Attribútumok állítottak be egyéni mezők toospecify hello mező használatáról. hello táblázatokban hello típusok és számbavétele attribútumokat adhat meg.

### <a name="field-types"></a>Mezőtípusok
| Típus | Leírás |
| --- | --- |
| *Edm.String* |A teljes szöveges keresés (például szóhatároló, származtató) érdekében lehetőség van a szöveg tokenekre bontására. |
| *Collection(Edm.String)* |A teljes szöveges keresés érdekében lehetőség van a karakterlánclista tokenekre bontására. Nincs a gyűjtemény elemeinek száma hello elméleti felső korlát, de hello 16 MB felső korlát a terhelés méretének toocollections vonatkozik. |
| *Edm.Boolean* |Igaz/hamis értékeket tartalmaz. |
| *Edm.Int32* |32 bites egész számok. |
| *Edm.Int64* |64 bites egész számok. |
| *Edm.Double* |Kétszeres pontosságú numerikus adatok. |
| *Edm.DateTimeOffset* |Hello OData V4 formátumban idő értékek dátum (pl. `yyyy-MM-ddTHH:mm:ss.fffZ` vagy `yyyy-MM-ddTHH:mm:ss.fff[+/-]HH:mm`). |
| *Edm.GeographyPoint* |A pont képviselő hello földgömb a földrajzi helyet. |

Részletesebb információkat az Azure Search által [támogatott adattípusokról itt](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) talál.

### <a name="field-attributes"></a>Mezőattribútumok
| Attribútum | Leírás |
| --- | --- |
| *Kulcs* |Hello dokumentum keresse meg a használt egyes dokumentumok egyedi Azonosítóját megadó karakterlánc. Minden egyes indexnek egy kulccsal kell rendelkeznie. Csak egy mező lehet hello kulcs, és a típusa tooEdm.String kell állítani. |
| *Lekérhető* |Megadja, hogy az adott mező visszaadható-e egy keresési eredményben. |
| *Szűrhető* |Lehetővé teszi, hogy a szűrő lekérdezésekben használt hello mező toobe. |
| *Rendezhető* |Lehetővé teszi, hogy a lekérdezés toosort eredményt, ezt a mezőt. |
| *Értékkorlátozó* |Lehetővé teszi, hogy egy mező toobe szerepel egy [jellemzőalapú navigációs](search-faceted-navigation.md) struktúra irányuló szűréséhez. Általában használható toogroup ismétlődő értékeket tartalmazó mezők több dokumentumok együtt (például több dokumentumot, amely egyetlen márka vagy szolgáltatás kategóriája) a legmegfelelőbb, értékkorlátozás. |
| *Kereshető* |Jelek hello mező, mint a teljes szöveges keresés. |

Részletesebb információkat az Azure Search [indexattribútumairól itt](https://docs.microsoft.com/rest/api/searchservice/Create-Index) talál.

## <a name="guidance-for-defining-an-index-schema"></a>Útmutatás az indexsémák meghatározásához
Az index tervezésekor, a tervezési fázis toothink keresztül minden döntési hello a a időt vehet igénybe. Fontos, hogy a keresés felhasználói élményt és az üzleti igényeket figyelembe venni az index tervezésekor, mivel minden egyes mezőt hozzá kell rendelni hello [megfelelő attribútumokhoz](https://docs.microsoft.com/rest/api/searchservice/Create-Index). Az index módosítása a telepítés után magában foglalja a újraépítése és hello adatok újbóli betöltése.

Amennyiben az adattárolási követelmények idővel változnak, a partíciók hozzáadásával vagy eltávolításával növelheti vagy csökkentheti a rendszer kapacitását. Részletes információkat [A Search szolgáltatás kezelése az Azure-ban](search-manage.md) vagy a [Szolgáltatásokra vonatkozó korlátozások](search-limits-quotas-capacity.md) című cikkekben talál.

