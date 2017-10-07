---
title: "aaaHow tooData profil adatforrások"
description: "Hogyan-tooarticle hogyan profilok tooinclude tábla és oszlop szintű adatokat, amikor adatforrások regisztrálása az Azure Data Catalog, és hogyan toouse adatok profilok toounderstand adatforrások kiemelve."
services: data-catalog
documentationcenter: 
author: spelluru
manager: NA
editor: 
tags: 
ms.assetid: 94a8274b-5c9c-4962-a4b1-2fed38a3d919
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/03/2017
ms.author: spelluru
ms.openlocfilehash: 12c9f38501cdaee903d0dcbbdd0b82395f35a187
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="data-profile-data-sources"></a>Adatforrások adatprofiljának készítése
## <a name="introduction"></a>Bevezetés
**A Microsoft Azure Data Catalog** egy teljes körűen felügyelt felhőszolgáltatás, amely a regisztráció és a rendszer a vállalati adatforrások felderítési funkcionál. Más szóval **Azure Data Catalog** összes segítve a személyek ismerje meg, megértése és adatforrások használja, és segíti a szervezeteket tooget több értékét a meglévő adatok. Ha egy adatforrás regisztrálva van **Azure Data Catalog**, a metaadatai másolt és indexelik hello szolgáltatást, de hello szövegegység nincs nem végződhet.

Hello **adatok profilkészítési** szolgáltatása **Azure Data Catalog** megvizsgálja-e a támogatott adatforrások a katalógusban hello adatait és a statisztikák és adatok információt gyűjt. Könnyen tooinclude egy profil az adategységet is. Amikor regisztrál egy adategységet, válassza ki a **adatok profillal együtt** a hello adatforrás-regisztráló eszköz.

## <a name="what-is-data-profiling"></a>Mi az a profilkészítési adatokat
A profilkészítési adatokat regisztrált hello adatforrás hello adatokat vizsgálja, és statisztikák és adatok információt gyűjt. Adatforrás-felderítés, során a statisztikai információk segít meghatározni hello adatok toosolve hello megfelelőségét az üzleti probléma.

<!-- In [How toodiscover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How tooinclude a data profile when registering a data source](#howto). -->

hello következő adatforrásokat támogatja a profilkészítési adatokat:

* SQL Server (beleértve az Azure SQL Database és Azure SQL Data Warehouse) táblák és nézetek
* Oracle-táblák és nézetek
* Teradata-táblák és nézetek
* Hive táblák

Adategységek regisztrálása segít a felhasználóknak többek között a következőket adatokat-profilok az adatforrásokról, beleértve a kérdések megválaszolása:

* Azt is használt toosolve kell a saját üzleti probléma?
* Hello adatok tooparticular szabványok vagy minták megfelelnek?
* Melyek azok hello adatforrás hello rendellenességeket?
* Mik azok a lehetséges kihívásai ezeket az adatokat integrálása az alkalmazásban?

> [!NOTE]
> Azt is megteheti dokumentáció tooan eszköz toodescribe hogyan adatok sikerült integrált egy alkalmazásba. Lásd: [hogyan toodocument adatforrások](data-catalog-how-to-documentation.md).
>
>

<a name="howto"/>

## <a name="how-tooinclude-a-data-profile-when-registering-a-data-source"></a>Hogyan tooinclude egy adatok profiljával egy adatforrás regisztrálása
Egyszerű tooinclude adatforrás profil. Amikor regisztrál egy adatforrást a hello **regisztrált objektumok toobe** hello az adatforrás regisztrációja panelje eszköz, válassza a **adatok profillal együtt**.

![](media/data-catalog-data-profile/data-catalog-register-profile.png)

További részletek toolearn, hogyan tooregister adatforrások esetén: [hogyan tooregister adatforrások](data-catalog-how-to-register.md) és [Ismerkedés az Azure Data Catalog](data-catalog-get-started.md).

## <a name="filtering-on-data-assets-that-include-data-profiles"></a>Szűrést az adategységek, amely tartalmazza az adat-profilok
tartalmazhatnak, amely tartalmazza az adatok profil toodiscover adategységek, `has:tableDataProfiles` vagy `has:columnsDataProfiles` rendelkezésre álló a keresési feltételeket.

> [!NOTE]
> Kiválasztása **adatok profillal együtt** hello adatok adatforrás-regisztráló eszköz tartalmazza a tábla és a oszlopszintű profillal kapcsolatos információk. Azonban hello Data Catalog API lehetővé teszi, hogy csak egy készletét tartalmazza profiladatok adatok eszközök toobe regisztrálva.
>
>

## <a name="viewing-data-profile-information"></a>Adatok profil információk megtekintése
Miután megtalálta a megfelelő adatforrás profillal, hello az profil adatait tekintheti meg. tooview hello adatok profilt válassza ki egy adategységet, válassza a **adatok profil** hello Data Catalog-portál ablakban.

![](media/data-catalog-data-profile/data-catalog-view.png)

Az adatok profil **Azure Data Catalog** jeleníti meg a tábla és oszlop profil információkat, így:

### <a name="object-data-profile"></a>Objektum adatok profil
* A sorok számát
* A táblázat mérete
* Hello objektum utolsó frissítésekor

### <a name="column-data-profile"></a>Oszlop adatok profil
* Adattípus oszlop
* A distinct érték
* NULL értéket tartalmazó sorok száma
* Minimum, maximum, átlagos és -értékek szórásának

## <a name="summary"></a>Összefoglalás
Profilkészítési adatokat nyújt statisztikai adatokat, és információt regisztrált adatok eszközök toohelp meghatározhatja, hogy hello adatok toosolve üzleti problémák hello megfelelőségét. Ellátása megjegyzésekkel és adatforrások dokumentálása, valamint adatokat profilok adhat a felhasználóknak bemutatják, az adatokat.

## <a name="see-also"></a>Lásd még:
* [Hogyan tooregister adatforrások](data-catalog-how-to-register.md)
* [Ismerkedés az Azure Data Catalog szolgáltatással](data-catalog-get-started.md)
