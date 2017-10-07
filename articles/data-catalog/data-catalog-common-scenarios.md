---
title: "aaaAzure Data Catalog gyakori forgatókönyvei |} Microsoft Docs"
description: "Az Azure Data Catalog, beleértve a hello regisztrációs és nagy értékű adatforrások felderítése, önkiszolgáló üzleti intelligencia engedélyezése és adatforrások és a folyamatok meglévő ismerete rögzítése gyakori forgatókönyvek áttekintése."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 60930d78-d2d4-4d5d-9651-bdda50b0da0e
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: a9bd222bcf85abc31621ce7c09264a399fbb7a4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-catalog-common-scenarios"></a>Az Azure Data Catalog gyakori forgatókönyvei
Ez a cikk bemutatja a gyakori forgatókönyvek, ahol a Azure Data Catalog segíthet a szervezet több érték lekérése a meglévő adatforrás.

## <a name="scenario-1-registration-of-central-data-sources"></a>1. forgatókönyv: Központi adatforrások regisztrálása
A szervezetek gyakran sok nagy értékű adatforrásokkal rendelkeznek. Ezeknek az adatforrásoknak üzleti, az online tranzakció-feldolgozási (OLTP) rendszerek, az adatraktárak és üzleti intelligencia/analytics adatbázisokat tartalmazza. hello rendszerek, és azokat közötti átfedés hello, általában növekszik adott idő alatt üzleti igények fejlődnek, és maga hello üzleti fejlődésének keresztül, például a fúziók és felvásárlások.

Nehézkes lehet a szervezet tagok tooknow ahol toolocate hello adatainak ezeket az adatforrásokat. A következők túl gyakori hello következő kérdésekre:

* Hello három HR rendszerek használjuk hello vállalaton belül, amelyek meg szeretnék használata toocreate jelentés az ilyen típusú?
* Hol kell haladhatok tovább tooget hello értékesítési számok hitelesített éppen véget ért hello évhez?
* Ki kell kéri, vagy mi hello folyamat tooget hozzáférés toohello adatraktár érdemes használni?
* Nem tudom, ha ezeket a számokat megfelelőek. Ki is kéri betekintés a a ezeket az adatokat hogyan kellene előtt az irányítópult megosztása a csapattal toobe?

toothese és további kérdésekre, az Azure Data Catalog biztosíthatnak válaszokat. hello központi, nagy értékű, informatikai RÉSZLEG által felügyelt adatforrások is használ, a szervezetek olyan gyakran hello logikai kiindulási pont a hello katalógus feltöltéséhez. Bár minden olyan felhasználó lehet adatforrást regisztrálni, valószínűleg tooprovide érték toohello legnagyobb számú felhasználók hello adatforrásokkal rendelkező kick-started hello katalógus rendelkező segít elfogadtatásához és hello rendszer használatát. 

Ha elkezdi az Azure Data Catalog, azonosítsa és regisztrálja az adatfelhasználók a különböző csapatok által használt fő adatforrások lehet az első lépés toosuccess.

Ebben a forgatókönyvben is megjelenít egy lehetőség tooannotate hello értékes adatok források toomake azokat könnyebben toounderstand és a hozzáférés. Egyik legfőbb szempontja, hogy ebből a törekvésből tooinclude információt hogyan használói kérhetnek hozzáférés toohello adatforrás. Az Azure Data Catalog hello felhasználói vagy ellenőrző adatforrás hozzáférés, a hivatkozások tooexisting eszközök vagy a dokumentációjával felelős csapat vagy szabad hello access-lekérő folyamat leíró szöveg hello e-mail címét adja meg. Ezt az információt segít tagok, aki regisztrált adatforrások felderítésére, de még nem rendelkeznek engedéllyel tooaccess tooeasily kérelem adatelérési hello hello folyamatok, amelyek a definiált és hello adatforrás tulajdonosok által vezérelt használatával.

## <a name="scenario-2-self-service-business-intelligence"></a>2. forgatókönyv: Az önkiszolgáló üzleti intelligencia
Bár a hagyományos vállalati üzletiintelligencia megoldások toobe továbbra is számos szervezet adatok hasznos információt részét képezik tájak, üzleti lépést módosítása hello tett önkiszolgáló üzleti Intelligencia, több fontos. Az önkiszolgáló üzleti Intelligencia használatával információval dolgozó munkatársak és elemzők hozhat létre a saját jelentéseket, munkafüzeteket és irányítópultok anélkül, hogy az egy központi IT-csoport, vagy éppen maximális száma korlátozza, hogy informatikai csapat ütemezés és a rendelkezésre állási.

Önkiszolgáló üzleti Intelligencia forgatókönyvekben a felhasználók általában sok előfordulhat, hogy nem rendelkezik korábban használt BI és elemzési több forrásból származó adatok össze. Az adatforrások egy részénél már ismertek, bár ez kihívást toodiscover milyen toodo toolocate kell, és lehetséges adatforrások egy adott tevékenység kiértékelése.

Hagyományosan, ez a felderítési folyamat, adott kézi: elemzők használja a társ hálózati kapcsolatok tooidentify kívüli személyek keresett hello adatokkal dolgozni. Miután egy adatforrás található, és a használt, hello folyamat megismétlődik újra minden ezt követő önkiszolgáló BI annak érdekében, a felderítés redundáns kézi folyamat végrehajtása több felhasználóval rendelkező.

A szervezet az Azure Data Catalog, ez a ciklus erőfeszítéssel érvénytelenné. Miután értesült a hagyományos módon adatforrás, valamelyik elemzőnek regisztrálhatja azt toomake azt könnyebben felderíthetők jövőbeli hello belüli más felhasználók által. Bár hello elemző ellátása megjegyzésekkel hello regisztrált adategységeket további értéket adhat, a jegyzet nem kell tootake helye: hello azonos időben eszközregisztrációs szerint. Felhasználók fokozatosan hozzáadása érték toohello hello katalógusban regisztrált adatforrások adott idő alatt, az ütemezések engedély járulhat.

A szerves növekedési hello katalógus tartalmának a természetes komplemens számnak toohello kezdeti központi adatforrások regisztrálása. Előre feltöltése hello katalógus adatokkal, amelyeket számos felhasználó kell egy kezdeti és felderítési motivator lehet. Felhasználók tooregister engedélyezése és ellátás jegyzettel további források lehet egy módon tookeep őket, és más szervezet tagjaival.

Érdemes megjegyezni, hogy bár ez a forgatókönyv kifejezetten önkiszolgáló üzleti Intelligencia összpontosít, hello azonos mintákat és kihívást alkalmazni toolarge méretű vállalati Üzletiintelligencia-projektek is. A Data Catalog használatával a szervezet javíthatja bármely erőfeszítés, amely magában foglalja az adatforrás felderítés manuális folyamat.

## <a name="scenario-3-capturing-tribal-knowledge"></a>3. forgatókönyv: Kollektív ismereteit rögzítése
Hogyan tudja, milyen adatokat kell toodo a feladatot, és ahol toofind adatokat?

Ha egy ideje már a feladat, akkor valószínűleg csak tudja. De folyamat tanulási fokozatos keresztül, és idővel tudásra hello adatforrások, amelyek kulcs tooyour napi munka.

Ha egy új alkalmazott csatlakozik a csapat, hogyan működik az adott személy tudja, hogy milyen adatokat szükséges hello feladat, és ahol toofind azt?

Valószínűleg, hello új személy ezeket a kérdéseket a tooyou származik.

A folyamatban lévő átruházása kollektív ismereteit hello adatforrás felderítési folyamat a szervezet kis és nagy részét képezi. Több vezető és tapasztalt csoport tagjai Tudásbázis hello évben létrehozott, és újabb csapattagok tudásra tooask Ha kérdése van, akkor azokat. hello gyakran a legtöbb fontos adatokat csak néhány kulcsfontosságú személy hello vezetői létezik, és ezen személyek szabadságon van, vagy hagyja hello team, romlik a hello szervezet.

Adatok szakértők szokásos ellenőrizze egy elérhető toodocument a Tudásbázis megosztja e-mailben, vagy a Word-dokumentumok csapat SharePoint-webhelyen. Bár lehet, hogy ez a megközelítés értékes, egy új felderítési problémát okozna: hogyan személyek tudja, milyen dokumentáció létezik, és ahol toofind azt?

Az Azure Data Catalog a szervezet rendelkezik egy egyetlen központi helyen tárolja, és a kollektív ismereteit megoszthatók, és hogy azok könnyen felfedezhetővé. A Data Catalog az adatok szakértők adhat megjegyzéseket az adategységekhez közvetlenül, és tartalmaznak egy tooexisting dokumentációjában. Hello katalógus toodiscover adatforrás használatakor a szervezet tagjai azok találhat nem csak hello forrás magát, de emellett a korábban már létező hello Tudásbázis csak a hello minds a szervezet szakértői.
