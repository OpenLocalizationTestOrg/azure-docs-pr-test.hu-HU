---
title: "aaaHow tooannotate adatforrások |} Microsoft Docs"
description: "Hogyan-tooarticle kiemelés hogyan az Azure Data Catalog, beleértve a rövid nevek, címkéket, leírásokat és szakértők tooannotate adategységeket."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 5a7e6bb2-863c-4eca-b614-1c814920d9ed
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 1d1ef34e3f1ef73cdc65129209d938abe1e36c01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooannotate-data-sources"></a>Hogyan tooannotate adatforrások
## <a name="introduction"></a>Bevezetés
**A Microsoft Azure Data Catalog** egy teljes körűen felügyelt felhőszolgáltatás, amely a regisztráció és a rendszer a vállalati adatforrások felderítési funkcionál. Ez azt jelenti a Data Catalog legyen, és segíti a felderítése, megismeréséhez és használatához adatforrások személyek, a meglévő adatok több értéket szervezetek tooget segíti. Ha egy adatforrás regisztrálva lett a Data Catalog, a metaadatok másolt és indexelik hello szolgáltatást, de hello szövegegység nincs nem végződhet. A Data Catalog lehetővé teszi, hogy felhasználók tooprovide saját – például a leírásokat és címkék – leíró metaadatok toosupplement hello metaadatokat hello adatforrásból, és toomake hello adatforrás többen érthető toomore.

## <a name="annotation-and-crowdsourcing"></a>A jegyzet és közösségi hozzáadása
Mindenki rendelkezik véleményt. És Ez azért hasznos.
A Data Catalog felismeri, hogy különböző felhasználók rendelkeznek-e a vállalati adatforrások különböző szempontok szerint, és, hogy a szempontok mindegyikének igen hasznos lehet. Vegye figyelembe a következő forgatókönyv hello:

* hello rendszergazda tudja hello szolgáltatásiszint-szerződés hello kiszolgálók vagy a szolgáltatások állomás hello adatforráshoz.
* adatbázis-rendszergazda hello hello biztonsági mentés ütemezése az egyes adatbázisok, és a megengedett ETL feldolgozási windows hello tudja.
* hello rendszertulajdonos tudja hello folyamat felhasználók toorequest hozzáférés toohello az adatforrásra vonatkozóan.
* hello adatok steward tudja, hogyan hello eszközök és hello adatforrás attribútumok hozzárendelését toohello vállalati adatokat az adatmodellbe.
* hello elemző tudja hello adatok felhasznált hello környezetében hello üzleti folyamatok támogatja azt.

A szempontok egy értékes, és a Data Catalog egy közösségi megközelítés toometadata, amely lehetővé teszi, hogy minden egyes egy toobe rögzített, és a használt tooprovide regisztrált adatforrások átfogó képet használja. Hello Data Catalog-portált használja, minden felhasználóhoz adhat hozzá és szerkeszthet saját megjegyzéseit, ugyanakkor nem képes tooview jegyzetek más felhasználók által biztosított.

## <a name="different-types-of-annotations"></a>A jegyzetek különböző típusaihoz
Adatok katalógus által támogatott hello jegyzetek típusú a következő:

| Megjegyzés | Megjegyzések |
| --- | --- |
| Rövid név |Rövid nevét is meg kell adnia szinten hello adatok eszköz toomake hello adategységeket több könnyen érthető. Rövid nevek lesz a leghasznosabb, ha hello alapul szolgáló objektum neve mégis fontos kontextusinformációkat, rövidített vagy más módon nem értelmezhető toousers. |
| Leírás |Leírások szolgáltatható hello adategységet és attribútum / oszlop szintek. Leírások hello felhasználója szempontjából a hello adategységet, vagy a használatával leíró szabad formátumú rövid szöveges jegyzetek. |
| Címkék (felhasználói címkék) |Címkék szolgáltatható hello adategységet és attribútum / oszlop szintek. Felhasználói címke található a felhasználói címkék használt toocategorize adategységeket vagy attribútumok. |
| Címkék (szószedet címkék) |Címkék szolgáltatható hello adategységet és attribútum / oszlop szintek. Szószedet címke található a központilag meghatározott Szójegyzék használt toocategorize adategységeket vagy attribútumok üzleti taxonómia használatával. További információ: [hogyan mentése tooset hello szabályozott címkézésre üzleti szószedet](data-catalog-how-to-business-glossary.md) |
| Szakértői |Szakértői hello adatok eszköz szinten is meg kell adnia. Szakértői szakértői meglátásokkal rendelkező hello adatok felhasználók vagy csoportok határozza meg és is szolgál, felhasználók, akik hello felderítése kapcsolódási pontok regisztrált adatforrások és kérdései hello meglévő megjegyzéseknek nem válaszol. |
| Hozzáférés kérése |Kérelem adatok eléréséhez meg kell adnia hello adatok eszköz szinten. Ez az információ van a felhasználók számára, hogy még nincs engedélyek tooaccess adatforrás felderítéséhez. Felhasználók hello felhasználót vagy csoportot, akik engedélyezi a hozzáférést, hello hello folyamat URL-CÍMÉT vagy, hogy a felhasználók szükség toogain hozzáférést eszközt, vagy maga hello folyamat szöveget adja meg e-mail címét hello adhat meg. |
| Dokumentáció |Dokumentáció hello adatok eszköz szinten is meg kell adnia. Eszköz dokumentációja rich text formátumú adatokat tartalmazó hivatkozásokat és a képeket, és amely tud biztosítani a továbbított adatokat nem leírásokat és címkék keresztül is. |

## <a name="annotating-multiple-assets"></a>Több eszköz ellátása megjegyzésekkel
A Data Catalog-portál hello lehetőséget választva egyszerre több adategységet, felhasználók fűzhet egyetlen művelettel az összes kijelölt eszközökhöz. Jegyzetek fog kijelölt tooall eszközöket, így könnyen tooselect alkalmazni, és adjon meg konzisztens leírását és címkék és szakértők kapcsolódó adategységeket.

> [!NOTE]
> Címkék és szakértők is megadható amikor hello Data Catalog adatokkal regisztrálásakor adategységeket forrás-regisztráló eszközzel.
>
>

Ha több táblák és nézetek, csak az oszlopok, hogy az összes kijelölt adatok eszközök rendelkezik közös kiválasztásával megjelenő hello Data Catalog-portált. Ez lehetővé teszi a felhasználók tooprovide címkéket és az összes oszlophoz hello ugyanaz a neve, leírása az összes kijelölt eszközök.

## <a name="annotations-and-discovery"></a>Jegyzetek és felderítés
Ahogy hello regisztrálás során hello adatforrás kinyert metaadatokat toohello Data Catalog search-index hozzáadott, felhasználó által megadott metaadatok. Ez azt jelenti, hogy nem csak tegye jegyzetek megkönnyítik a felhasználók toounderstand hello adatok azokat felderítésére, jegyzeteket is megkönnyítik a felhasználók toodiscover feliratozva hello adategységeket történő kereséssel hello feltételeket, amelyek logika toothem.

## <a name="summary"></a>Összefoglalás
A Data Catalog egy adatforrás regisztrálása teszi adatok felderíthető szerkezeti és leíró metaadatok hello adatforrásból hello katalógusszolgáltatás másolja. Miután egy adatforrás regisztrálva van, a felhasználók adja meg a jegyzetek toomake könnyebb toodiscover és megérteni a belül hello Data Catalog-portált.

## <a name="see-also"></a>Lásd még:
* [Ismerkedés az Azure Data Catalog](data-catalog-get-started.md) oktatóprogram lépésről lépésre részletei tooannotate adatforrások.
