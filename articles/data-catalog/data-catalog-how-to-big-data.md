---
title: "\"a big data\" adatforrások aaaHow toowork |} Microsoft Docs"
description: "Hogyan-tooarticle kijelölő mintákat az Azure Data Catalog használatával \"big data\" adatforrások, beleértve az Azure Blob Storage tárolóban, az Azure Data Lake és a Hadoop HDFS."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 626d1568-0780-4726-bad1-9c5000c6b31a
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: e478f71f26744975a7d7e1784b74bf50b424cf65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toowork-with-big-data-sources-in-azure-data-catalog"></a>Hogyan toowork a big Data típusú adatok az Azure Data Catalog adatforrások
## <a name="introduction"></a>Bevezetés
**A Microsoft Azure Data Catalog** egy teljes körűen felügyelt felhőszolgáltatás, amely a regisztráció és a rendszer a vállalati adatforrások felderítési funkcionál. Az összes vonatkozó útmutatás nyújtása a felhasználók felderítése, megismeréséhez és használatához adatforrások, és több érték szervezetek tooget segíti a meglévő adatforrásokból, beleértve a big Data típusú adatok is.

**Az Azure Data Catalog** támogatja hello Azure Blog Storage-BLOB és a könyvtárak, valamint a Hadoop HDFS fájlok és könyvtárak regisztrációja. Ezeknek az adatforrásoknak félig strukturált jellege hello nagyfokú rugalmasságot biztosít. Azonban tooget hello regisztrálja őket a legtöbb érték **Azure Data Catalog**, felhasználók figyelembe kell venni, hogyan vannak rendszerezve hello adatforrások.

## <a name="directories-as-logical-data-sets"></a>Könyvtárak, logikai adatkészletek
Az általános mintázatát big Data típusú adatok forrásból szervezése tootreat könyvtárak, mint logikai adatkészletek. Legfelső szintű könyvtárak használt toodefine adatkészlet, addig, amíg a almappák definiáljon partíciókat, és a bennük hello fájlok tárolásához hello adatokat mozgatná.

Ez a minta egy példát a következő lehet:

    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...

Ebben a példában a vehicle_maintenance_events és location_tracking_events logikai adatkészletek jelölik. Ezek a mappák mindegyikének hónap és év szerint vannak csoportosítva almappák adatok fájlokat tartalmazza. Ezek a mappák mindegyikének tartalmazhat több száz vagy ezer.

Ebben a mintában az egyes fájlok regisztrálása **Azure Data Catalog** valószínűleg nem értelmezhető. Ehelyett regisztrálja, amelyek megfelelnek a jelentéssel bíró toohello felhasználók hello adatokkal végzett munka hello adatkészletek hello könyvtárak.

## <a name="reference-data-files"></a>Hivatkozás az adatfájlok
A kiegészítő minta toostore hivatkozási adatkészletek, mint a fájlokat. Ezek az adathalmazok is értelmezhető hello "kicsi" big Data típusú adatok oldalán, és gyakran az analitikus adatok modell hasonló toodimensions. Hivatkozás az adatfájlok hello tömeges belül máshol tárolódnak hello big Data típusú adatok tárolási hello adatfájlok környezetben használt tooprovide rekordokat tartalmaz.

Ez a minta egy példát a következő lehet:

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

Egy elemző vagy az adatok tudósok hello nagyobb könyvtárstruktúrák hello adatait az használatakor lehet-e az hello adatokat a referencia-fájlokban használt tooprovide részletes információkat, amelyek entitások említett tooonly név vagy azonosító alapján hello nagyobb adatokban Állítsa be.

Ebben a mintában az így tooregister hello egyedi hivatkozást tartalmazó adatfájlokat **Azure Data Catalog**. Minden fájl jelöl egy adatokat, és minden egyes feliratozva, és egyenként felderített.

## <a name="alternate-patterns"></a>Alternatív minták
hello hello előző szakaszban leírt minták a big Data típusú adatok tárolási rendezi lehetséges, hogy csak két lehetséges módjait, de minden megvalósítása nem egyezik. Függetlenül attól, hogy az adatforrások felépítése, amikor nagy adatforrások regisztrálása **Azure Data Catalog**, összpontosítani hello fájlok és könyvtárak, amelyek megfelelnek a hello adatkészleteket, amelyek érték tooothers belül regisztrálása a szervezet. A fájlok és könyvtárak regisztrálása is megzavarhatják hello katalógus, így nehezebb a felhasználók toofind mi van szükségük.

## <a name="summary"></a>Összefoglalás
Az adatforrások nyilvántartására **Azure Data Catalog** teszi ezeket könnyebb toodiscover és megérteni. Regisztráció és ellátása megjegyzésekkel hello big Data típusú adatok fájlok és könyvtárak, amelyek megfelelnek a logikai adatkészletek, segíthet felhasználók megkeresheti, hello nagy számukra szükséges adatforrásokat.
