---
title: "aaaAzure a Mobile Engagement iOS SDK kibocsátási megjegyzései |} Microsoft Docs"
description: "Legújabb frissítések és az Azure Mobile Engagement SDK iOS eljárásai"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a43ff0f6-90d5-4b3c-8d7a-a1db21bc776b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: ae29d200ebb1784357b29edbd1f66b71df0778cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement-ios-sdk-release-notes"></a>Az Azure Mobile Engagement iOS SDK kibocsátási megjegyzései

## <a name="410-07172017"></a>4.1.0 (07/17/2017)
* Rögzített jelvények háttérben törölve lett.
* Rögzített figyelmeztetések a XCode 9 kapcsolatos API-k nem hívja a fő várakozási sorba.
* Rögzített memóriavesztés Reach lekérdezéseinél.
* Támogatás az iOS eldobott 6.X. A verzió hello központi telepítés célja az alkalmazás-től kezdődő kell lennie legalább iOS 7-es.

## <a name="401-12132016"></a>4.0.1 (12/13/2016)
* Továbbfejlesztett naplózási kézbesítési háttérben.

## <a name="400-09122016"></a>4.0.0 (09/12/2016)
* Rögzített értesítési nem műveletet kiváltó iOS 10-eszközökön.
* XCode 7 érvényteleníthető.

## <a name="324-06302016"></a>3.2.4 (06/30/2016)
* Rögzített összesítési műszaki naplókat, valamint további naplófájlokat között.

## <a name="323-06072016"></a>3.2.3 (06/07/2016)
* Rögzített hello hiba, ahol kézbesítési visszajelzés jelentése nem hello háttérben alkalmazás esetén.
* Optimalizált hello műszaki naplók küldése.

## <a name="322-04072016"></a>3.2.2 (04/07/2016)
* A HTTP kérelem törlését, ami időnként toocrash rögzített hiba.

## <a name="321-12112015"></a>3.2.1 (12/11/2015)
* Ha egy új app-példány váltja ki, egy értesítés a mélyhivatkozással rögzített hello késleltetés

## <a name="320-10082015"></a>3.2.0 (10/08/2015)
* Hello SDK toomake azt együttműködve Bitcode engedélyezve **Xcode 7**.
* Javított hibákról kapcsolódó tooin alkalmazáson belüli értesítések.
* Alacsony töltöttségű telepre vonatkozó és egyéb ilyen forgatókönyvek esetén további megbízható végrehajtott hello az alkalmazásbeli értesítésekben.
* 3. fél könyvtár által létrehozott további konzolnaplófájlokban eltávolítva.

## <a name="310-08262015"></a>3.1.0 (08/26/2015)
* Javítsa ki az iOS 9-es kompatibilitási hiba egy harmadik féltől származó könyvtárral. Összeomlások okozott amíg küldése kérdezze le az eredményeket, az alkalmazással kapcsolatos adatok vagy a további adatokat.

## <a name="300-06192015"></a>3.0.0 (06/19/2015)
* A Mobile Engagement csendes leküldéses értesítések használja.
* Támogatás az iOS eldobott 4.X. A verzió hello központi telepítés célja az alkalmazás-től kezdődő kell lennie legalább iOS 6.

## <a name="220-05212015"></a>2.2.0 (05/21/2015)
* hello Mobile Engagement-eszközazonosítót az eszközök < iOS 6 alapján a telepítéskor létrehozott GUID.

## <a name="210-04242015"></a>2.1.0 (04/24/2015)
* A hozzáadott Swift kompatibilitási.
* Kattintva egy értesítést, ha az URL-cím már hello művelet végre jobb, hello alkalmazás megnyitása után.
* A hozzáadott hiányzó fejlécfájlt az SDK-csomagot.
* Megtörtént egy probléma javítása amikor hello a Mobile Engagement összeomlási jelentéskészítői le lett tiltva.

## <a name="200-02172015"></a>2.0.0 (02/17/2015)
* Az Azure Mobile Engagement eredeti kiadás
* a kapcsolódási karakterlánc konfigurációs appId/sdkKey konfigurációs cseréli le.
* API toosend és a tetszőleges XMPP-üzeneteket fogadjon tetszőleges XMPP-entitások.
* API toosend eltávolítva és eszközök közötti üzeneteket fogadni.
* Biztonsági fejlesztések.
* SmartAd követési eltávolítva.
