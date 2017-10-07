---
title: "Azure Data Catalog aaaHow toodiscover adatforrások |} Microsoft Docs"
description: "Ez a cikk emel ki, hogyan toodiscover regisztrált adategységeket Azure Data Catalog, beleértve a Keresés és szűrés, majd hello segítségével nyomja le a hello Azure Data Catalog-portál kijelölő képességeit."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: f72ae3a3-6573-4710-89a7-f13555e1968c
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 624834b8895dd50c8931c9d3e6f8dc217927c617
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodiscover-data-sources-in-azure-data-catalog"></a>Hogyan toodiscover adatforrás az Azure Data Catalog
## <a name="introduction"></a>Bevezetés
Az Azure Data Catalog egy teljes körűen felügyelt felhőszolgáltatás, amely a regisztráció és a vállalati adatforrások felderítését a rendszer funkcionál. Más szóval a Data Catalog segít személyek felderítése, megismeréséhez és használatához adatforrások, és segít a szervezeteknek több érték lekérése a meglévő adatokat. A Data Catalog egy adatforrás regisztrálása után a metaadatait indexelik hello szolgáltatást, így könnyen kereshet toodiscover hello adatokat kell.

## <a name="searching-and-filtering"></a>A Keresés és szűrés
A Data Catalog felderítési két elsődleges mechanizmus használja: a Keresés és szűrés.

Tervezett toobe elsajátítható és hatékony keresést. Alapértelmezés szerint a keresési feltételek elleni hello katalógusban, beleértve a felhasználó által megadott jegyzetek egyik tulajdonságnak sem teljesül.

Szűrés úgy van kialakítva, toocomplement keresése. Kiválaszthatja, hogy adott jellemzőiket, például a szakértők, adatforrástípust, objektumtípus és címkék. Csak megfelelő adategységeket megtekintheti, és korlátozhatja a keresési eredmények toomatching eszközök.

A Keresés és szűrés együttes használatával a Data Catalog toodiscover hello adatforrások kell regisztrált adatforrások hello gyorsan lépjen.

## <a name="search-syntax"></a>Keresési szintaxis
Bár az hello alapértelmezett szabad szöveges keresés egyszerű és intuitív, is használható a Data Catalog keresési szintaxisának nagyobb mértékben vezérelheti a hello keresési eredmények között. Data Catalog keresési támogatja hello a következő módszerek:

| Módszer | Használat | Példa |
| --- | --- | --- |
| Az egyszerű keresés |Az egyszerű keresés által használt egy vagy több keresési feltételeket. A eredményei bármely eszközök, amelyek megfelelnek egy vagy több megadott hello feltételek egyik tulajdonságnak sem. |`sales data` |
| Tulajdonság keresése |Ha hello keresési kifejezés egyeztetve van-e hello visszatérési csak adatforrások megadva tulajdonság. |`name:finance` |
| Logikai operátorok |Bővíthetők, vagy a megadásával szűkíthető, a Keresés logikai műveletekkel. |`finance NOT corporate` |
| Csoportosítás zárójelekkel |Használjon kerek zárójeleket tartalmazhatnak toogroup részei hello lekérdezés tooachieve logikai elkülönítés érdekében, különösen a logikai operátorokkal együtt használva. |`name:finance AND (tags:Q1 OR tags:Q2)` |
| Összehasonlító operátorok |Nem egyenlő összehasonlítások is használni azokhoz a tulajdonságokhoz, szám és adat adattípusúak. |`modifiedTime > "11/05/2014"` |

A Data Catalog keresési kapcsolatos további információkért lásd: hello [Azure Data Catalog](https://msdn.microsoft.com/library/azure/mt267594.aspx) cikk.

## <a name="hit-highlighting"></a>Találatok kiemelése
Keresési eredmények között, bármely jelenik meg, amelyek megfelelnek a megadott hello tulajdonságok keresési feltételek (például hello adatok eszköz neve, leírása és címkék) kiemelve toomake, miért adta vissza egy adott adategységet egy adott keresési könnyebb tooidentify.

> [!NOTE]
> ki kiemelésének használata hello találat tooturn **kiemelése** hello Data Catalog-portál kapcsoló.
>
>

Amikor a keresési eredmények között, nem mindig lehet nyilvánvaló ezért egy adategységet megtalálható, még akkor is kiemeléssel találati engedélyezve van. Az összes tulajdonság figyelembe veszi a keresésnél alapértelmezés szerint, mert egy adategységet esetleg visszaadott egyezés miatt az oszlopszintű tulajdonság. És több felhasználó megjegyzéseket fűzhet regisztrált adategységeket saját címkéket és leírásokat, mert nem minden metaadat előfordulhat jelenik meg a keresési eredmények hello listája.

Hello alapértelmezett mozaik elrendezés nézetben minden egyes hello keresési eredmények megjelenített mozaik tartalmaz egy **nézet keresési kifejezés megegyezik** ikonra, így gyorsan megtekintheti hello egyező helyen, és toojump toothem Ha azt szeretné.

 ![Kattintson a kijelölés, és megkeresi a megadott hello Azure Data Catalog-portál](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a>Összefoglalás
Mert az adatforrás regisztrálása a Data Catalog másolatok szerkezeti és leíró metaadatok hello adatokból forrás toohello katalógusszolgáltatás, hello adatforrás toodiscover egyszerűbbé válik és ismertetése. Egy adatforrás regisztrálása után észleléséhez szűrésének használata, és a Data Catalog-portál hello keresni.

## <a name="next-steps"></a>Következő lépések
* Részletes részletei toodiscover adatforrások, lásd: [Ismerkedés az Azure Data Catalog](data-catalog-get-started.md).
