---
title: a Data Catalog aaaIntroduction tooAzure |} Microsoft Docs
description: "Ez a cikk áttekintése a Microsoft Azure Data Catalog, beleértve annak szolgáltatásait és hello problémákat tárgyalja. A Data Catalog lehetővé teszi, hogy bármely felhasználó tooregister, felderítését, értelmezését és felhasználását adatforrások."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: cc733907-17ec-4153-9f0c-5b3754b2db19
ms.service: data-catalog
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 82144c440b5692d3608af08208f36ee8e6dfdc93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-data-catalog"></a>Mi az az Azure Data Catalog?
Az Azure Data Catalog egy teljes körűen felügyelt felhőszolgáltatás, amelynek fedezhetnek hello adatforrások kell, és hello talált adatforrásokat értelmezzék tisztában. Hello, azonos időben, a Data Catalog segítségével a szervezetek get több érték, a meglévő befektetéseikből. 

A Data Catalog segítségével bármely felhasználó (elemző, adattudós, vagy fejlesztő) felfedezhet, értelmezhet és felhasználhat adatforrásokat. A Data Catalog tartalmaz egy közösségi modellt is a metaadatok és megjegyzések kiszervezéséhez. A minden szervezet felhasználók toocontribute egyetlen, a központi hely a Tudásbázis, és létre egy közösségi és az adatok kulturális környezet.

## <a name="discovery-challenges-for-data-consumers"></a>Az adatok felfedezésének kihívásai az adatfelhasználók számára
A vállalati adatforrások felfedezése hosszú ideje egy organikus, kollektív tudáson alapú folyamat. A vállalatok számára, így tooget hello információs legtöbb érték ez a megközelítés számos kihívást:

* A felhasználók lehet, hogy csak akkor szereznek tudomást egy adatforrás létezéséről, amikor egy másik munkafolyamat során kapcsolatba kerülnek vele. Nincs egyetlen központi hely az adatforrások nyilvántartására.
* Kivéve, ha a felhasználók ismeri egy adatforrás hello helyét, toohello adatok nem kapcsolódnak egy ügyfél alkalmazás használatával. Adatok-felhasználásának követelménye igényelnek a felhasználók tooknow hello kapcsolati karakterláncot vagy az elérési út.
* Kivéve, ha a felhasználók tudják, hello helyét egy adatforrás dokumentációjának, ismerniük nem készült hello hello adatok használja. Lehet, hogy az adatforrások és dokumentáció csak különböző helyeken és különböző módokon érhetőek el.
* Ha a felhasználók egy kérdése van, kell hello szakértővel vagy csapattal, hello adatok felelős keresse meg és offline megszólítása őket. Nincs explicit kapcsolat az adatok és az azok felhasználására szakértői módon rálátó emberek között.
* Kivéve, ha a felhasználók a hozzáférés toohello adatforrás kérelmezési hello folyamatának megismerése, hello adatforrás és a hozzá tartozó dokumentáció felderítésére még nem segítséget nyújthatnak a támogatási hello adatok eléréséhez.

## <a name="discovery-challenges-for-data-producers"></a>Az adatok felfedezésének kihívásai az adatalkotók számára
Bár az adatok fogyasztók arcfelismerési hello kihívást azt már korábban említettük, a felhasználókat, akik előállító és információs eszközeinek karbantartásáért felelős a saját kihívásaik szembesülhetnek:

* Az adatforrások felcímkézése tájékoztató jellegű metaadatokkal gyakran hiábavalónak bizonyul. Az ügyfélalkalmazások általában figyelmen kívül hagyása hello adatforrás tárolt leírásokat.
* Adatforrásokhoz dokumentációt készíteni gyakran hiábavalónak bizonyul. Szinkronban tartani az adatforrást és annak dokumentációját folyamatos felelősséget jelent, és a felhasználók nem is nagyon bíznak egy elavultnak tűnő dokumentációban.
* Az adatforrásokhoz tartozó dokumentációk létrehozása és fenntartása összetett és időigényes feladat. Adott dokumentáció azonnal elérhetők legyenek tooeveryone hello adatforrást használ, akik így még inkább lehet.
* Hozzáférés toodata források korlátozása, és győződjön meg arról, hogy az adatfelhasználók ismerjék a toorequest hozzáférés Mitől egy állandó kihívás.

Az ilyen kihívások kombinálva jelentős korlátokat jelentenek a vállalatok számára, aki szeretné, hogy tooencourage, és léptesse elő a hello felhasználását és értelmezését a vállalati adatok.

## <a name="azure-data-catalog-can-help"></a>Az Azure Data Catalog segíthet
A Data Catalog tervezett tooaddress ezeket a problémákat és toohelp vállalatok get hello legtöbb értékét a meglévő információs eszközeiket. A Data Catalog lehetővé teszi az adatforrások könnyebben feltárhatóvá és értelmezhetővé hello felhasználók hello adatok kezelésére.

A Data Catalog egy felhőalapú szolgáltatást biztosít, amelybe az adatforrásokat regisztrálni lehet. hello adatok továbbra is a helyükön, de a metaadatok másolatát kerül tooData katalógus, valamint egy hivatkozást toohello adatforrás helyre. metaadatok hello könnyen felfedezhetők keresési és értelmezhetővé toohello felhasználók, akik tudja azt deríteni minden adatforrás indexelt toomake is van.

Miután egy adatforrás regisztrálva van, a metaadatai bővíthetők, hello regisztrált felhasználó folyamatos, vagy más felhasználók hello vállalaton belül. Bármely felhasználó megjegyzésekkel láthatja el az adatforrásokat, amelyekben leírásokat, címkéket és egyéb metaadatokat, például dokumentációkat és hozzáférés-kérelmezési eljárásokat adhat meg. Ez a leíró metaadatok kiegészítik hello szerkezeti metaadatokat (például oszlop nevét és adattípusok) hello adatforrásból regisztrált.

Felderítésének, illetve az adatforrások és a használatukat ismertetése célja hello elsődleges hello adatforrások regisztrálása. Vállalati felhasználók adatok módosítania kell az üzleti intelligencia, alkalmazásfejlesztés, adattudomány vagy bármilyen egyéb jellegű feladat ahol hello megfelelő adatokra szükség. Használhatnak hello Data Catalog felderítési élmény tooquickly található adatok igényeiknek, értelmezését hello adatok tooevaluate hello célra való alkalmasságra és hello adatok felhasználását hello adatforrás tetszőleges eszköz általi megnyitásával. 

At hello azonos idő, felhasználók által címkézés dokumentálása, és már regisztrált adatforrások ellátása megjegyzésekkel járulhat toohello katalógus. Akkor is regisztrálhatja az új adatforrások, amely majd felderíthető, megértettem, és a katalógus felhasználóinak Közössége hello által felhasznált.

![A Data Catalog képességei](./media/data-catalog-what-is-data-catalog/data-catalog-capabilities.png)

## <a name="learn-more-about-data-catalog"></a>További információ a Data Catalog szolgáltatásról
További információk a Data Catalog képességeinek hello toolearn lásd:

* [Hogyan tooregister adatforrások](data-catalog-how-to-register.md)
* [Hogyan toodiscover adatforrások](data-catalog-how-to-discover.md)
* [Hogyan tooannotate adatforrások](data-catalog-how-to-annotate.md)
* [Hogyan toodocument adatforrások](data-catalog-how-to-documentation.md)
* [Hogyan tooconnect toodata források](data-catalog-how-to-connect.md)
* [Hogyan toowork a big Data típusú adatok](data-catalog-how-to-big-data.md)
* [Hogyan toomanage adategységeket](data-catalog-how-to-manage.md)
* [Hogyan tooset be hello üzleti szószedet](data-catalog-how-to-business-glossary.md)
* [Gyakori kérdések](data-catalog-frequently-asked-questions.md)

## <a name="next-steps"></a>Következő lépések
a Data Catalog használatába tooget Ugrás:
* [Microsoft Azure Data Catalog](https://www.azuredatacatalog.com)
* [Ismerkedés az Azure Data Catalog szolgáltatással](data-catalog-get-started.md)
