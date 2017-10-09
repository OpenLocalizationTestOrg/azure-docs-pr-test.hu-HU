---
title: "mentése az Azure Data Catalog szabályoz címkézésre hello üzleti szószedet aaaSet |} Microsoft Docs"
description: "Hogyan-tooarticle kijelölő hello üzleti szószedet az Azure Data Catalog meghatározása és a segítségével egy közös üzleti szószedet tootag regisztrált adategységeket."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: b3d63dbe-1ae7-499f-bc46-42124e950cd6
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: c9adf663bd08ac3c0c7b5d3551e6af409fe69ebc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-hello-business-glossary-for-governed-tagging"></a>A címkézés szabályozott hello üzleti szószedet beállítása
## <a name="introduction"></a>Bevezetés
Az Azure Data Catalog adatforrás felderítés lehetővé teszi, így akkor is könnyen megtalálhatóvá és értelmezhetővé hello adatforrások tooperform elemzés kell, és döntéseket. Ezeket a képességeket hello legnagyobb hatást ellenőrizze, ha található, és elérhető adatforrások legszélesebb skáláját hello ismertetése.

Egy Data Catalog szolgáltatás, amely elősegíti a eszközök adatok megértése címkézés van. Címkézés használatával is társíthat kulcsszavak egy eszköz vagy egy oszlop, így pedig könnyebb toodiscover hello eszköz keresést, vagy keresse meg. Címkézés is segítségével megismerheti, további könnyen hello környezet és a leképezés hello eszköz.

Azonban címkézés néha problémákat okozhat saját. Néhány példa a problémák, amelyek címkézése a következők:

* hello használja az egyes eszközök rövidítések és mások bővített szöveg. Ez inkonzisztenciát hátráltatja hello felderítési eszközök, annak ellenére, hogy hello leképezés lett tootag hello eszközök hello azonos címkével.
* Ez azt jelenti, attól függően, hogy a környezetben esetleges változásait. Például a címke neve *bevétel* az ügyfél az adatkészlet jelentheti ügyfél bevétel, de hello a negyedéves értékesítési dataset azonos címkével jelentheti negyedéves bevétel hello vállalat számára.  

toohelp cím ezeket és más hasonló kihívás, a Data Catalog egy üzleti szószedetet tartalmaz.

Hello Data Catalog üzleti szószedet segítségével egy szervezet üzleti feltételek és a definíciók toocreate egy közös üzleti szószedet is dokumentum. A cégirányítási hello szervezet lehetővé teszi a használati adatok konzisztenciáját. Miután egy kifejezés hello üzleti szószedet van definiálva, rendelhető hozzá tooa adategységet hello katalógusban. Ez a megközelítés *szabályozott címkézés*, van ugyanezt a megközelítést, a címkézés hello.

## <a name="glossary-availability-and-privileges"></a>Szószedet rendelkezésre állás és a jogosultságokat
hello üzleti szószedet csak a Standard Edition Azure Data Catalog hello érhető el. hello a Data Catalog szabad Edition nem tartalmazza a termékben, és nem biztosít képességek az irányadó címkézésre.

Hello üzleti szószedet hello keresztül érheti el **szószedet** hello Data Catalog portal navigációs menü lehetőséget.  

![Hello üzleti szószedet elérése](./media/data-catalog-how-to-business-glossary/01-portal-menu.png)

Katalógus-rendszergazdák és a Rendszergazdák szerepkör hozhat létre, hello szószedet szerkesztése és törlése hello üzleti szószedet szószedet feltételeit. A Data Catalog senki hello kifejezés definíciókat és címke eszközök szószedet kifejezéseivel jelölhetik tekintheti meg.

![Egy új szószedetben hozzáadása](./media/data-catalog-how-to-business-glossary/02-new-term.png)

## <a name="creating-glossary-terms"></a>SZÓJEGYZÉK létrehozása
Katalógus-rendszergazdák és rendszergazdák szószedet kattintva hozhat létre szószedetben hello **új kifejezés** gombra. Minden egyes szószedetben hello a következő mezőket tartalmazza:

* Hello kifejezés egy üzleti definíciója.
* Egy leírást, amely rögzíti a hello szánt hello eszköz vagy az oszlop használatát vagy az üzleti szabályok
* A legtöbb kapcsolatos hello kifejezés hello ismerő résztvevők listáját
* hello szülő kifejezés, amely meghatározza, melyik hello a kifejezés szerveződik hello hierarchia

## <a name="glossary-term-hierarchies"></a>Szószedet kifejezés hierarchiák
Hello Data Catalog üzleti szószedet segítségével egy szervezet az üzleti szószedet leírhatja kifejezések hierarchikus, és létrehozhat egy besorolási feltételek az üzleti elnevezési rendszert jobban jelölő.

Egy kifejezés egyedinek kell lennie a hierarchiában megadott mértékét. Duplikált nevek használata nem engedélyezett. A hierarchia szintjének Nincs korlát toohello száma, de a hierarchiában van gyakran több könnyen érthető amikor három szintje vagy kevesebb.

hello üzleti szószedet hierarchiái hello használata nem kötelező. Hangpostaüzenetek hello szülő kifejezés mező üresen szószedetben feltételek simán (nem hierarchikus) listájának a hello szószedet hoz létre.  

## <a name="tagging-assets-with-glossary-terms"></a>Szószedet kifejezéseivel jelölhetik eszközök címkézése
Után szószedetben hello katalógus belül, az eszközök címkézése hello élménye nem optimalizált toosearch hello szószedet, mert az a felhasználó beír egy címkét. hello Data Catalog-portált a megfelelő szószedet feltételek toochoose listáját jeleníti meg. Ha hello felhasználó kijelöli a a szószedetben hello listából, hello kifejezés toohello eszköz van adva egy címkét (más néven a szószedet címkével). hello felhasználó is választhat toocreate új címke írja be a keresőkifejezést, amely nincs hello szószedet (más néven a felhasználói kód).

![Egy felhasználó címkével és két szószedet címkék címkézett adategységet](./media/data-catalog-how-to-business-glossary/03-tagged-asset.png)

> [!NOTE]
> Felhasználói címkék hello, csak a támogatott címke típusú hello a Data Catalog ingyenes kiadásra.
>
>

### <a name="hover-behavior-on-tags"></a>Vigye a címkék viselkedése
A Data Catalog-portál hello hello címkék kétféle vizuálisan önálló, és jelen különböző rámutatáskor viselkedések. Ha egy felhasználó címke mutat, láthatja a hello címke szövegét és hello felhasználó vagy felhasználók, akik hello címke hozzáadása. Ha egy szószedet címke mutat, is látni hello definíciójának hello szószedet és a hivatkozás tooopen hello üzleti szószedet tooview hello teljes definíciójának hello.

### <a name="search-filters-for-tags"></a>A címkék keresési szűrők
Szószedet címkék és a felhasználói címkék nem kereshető is, és alkalmazhatja azokat a keresési szűrőként.

## <a name="summary"></a>Összefoglalás
Hello üzleti szószedet Azure Data Catalog, és a címkézést, lehetővé teszi, hogy szabályozott hello segítségével azonosíthatja, kezelése és egységes módon adategységeket. hello üzleti szószedet előléptetheti hello üzleti szószedet szervezet tagjai tudomást. hello szószedet is támogatja a rögzítés jelentéssel bíró metaadatok, amely egyszerűbbé teszi az eszköz felderítése és értelmezését.

## <a name="next-steps"></a>Következő lépések
* [Üzleti szószedet műveletek REST API dokumentációja](https://msdn.microsoft.com/library/mt708855.aspx)
