---
title: "aaaMobile Engagement – fogalmak |} Microsoft Docs"
description: "Azure Mobile Engagement – fogalmak"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 8d19abd1-0a6c-4772-9fa5-5e99980ac5da
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 5aa7f28c00cd641a36a6e040c6b13d802ea6ae41
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement-concepts"></a>Azure Mobile Engagement – fogalmak
A Mobile engagement néhány fogalmak közös tooall támogatott platformokat. Ez a cikk röviden ismerteti ezen fogalmakat.

Ez a cikk remek kezdőpont, ha új tooMobile Engagement. Emellett győződjön meg arról, hogy tooread hello dokumentáció adott toohello platformot használ, mert az pontosítja további részletek és példák, valamint lehetséges korlátozásokkal ebben a cikkben ismertetett hello fogalmakat.

## <a name="devices-and-users"></a>Eszközök és felhasználók
A Mobile Engagement az egyes eszközök számára létrehozott egyedi azonosítóval azonosítja a felhasználókat. Ezen azonosító neve eszközazonosító hello (vagy `deviceid`). Úgy, hogy a futó összes alkalmazás hello azonos állítja elő eszköz megosztás hello ugyanazon eszközazonosítót használja.

Ez lényegében azt jelenti, hogy a Mobile Engagement úgy tekinti, egy eszköz toobelong tooexactly egy felhasználót, és így, felhasználók és eszközök is egyenértékű fogalmak.

## <a name="sessions-and-activities"></a>Munkamenetek és tevékenységek
A munkamenet, a hello idő hello felhasználó, felhasználó által végrehajtott hello alkalmazás használatát elindítja a befejezéséig tart hello felhasználói leáll.

Egy tevékenység készen egy része hello alkalmazás alrészének egy felhasználó általi egyszeri használata (általában egy képernyő, de semmi megfelelő toohello alkalmazás lehet).

Egy felhasználó egyszerre csak egy tevékenységet végezhet.

Egy tevékenység nevét (korlátozott too64 karakter) által azonosított, és tartalmazhatnak néhány további adatokat (a hello a legfeljebb 1024 bájt mennyiségben).

A felhasználó által végrehajtott műveletek sorozata hello a munkamenetek számítása automatikusan történik. A munkamenet akkor kezdődik, amikor hello felhasználó elindítja az első tevékenységet, és befejezésével ér véget, hogy az utolsó tevékenység. Ez azt jelenti, hogy a munkamenetet nem kell külön elindítani vagy leállítani toobe. Helyette a tevékenységeket kell külön elindítani és leállítani. Ha nincs jelentett tevékenység, jelentett munkamenet sincs.

## <a name="events"></a>Események
Eseményeket használt tooreport azonnali műveletek (például milyen gombokra vagy mely cikkeket olvasta el felhasználók) is.

Egy esemény lehet kapcsolódó toohello aktuális munkamenet tooa feladat fut, vagy lehet különálló esemény.

Az esemény nevét (korlátozott too64 karakter) által azonosított, és tartalmazhatnak néhány további adatokat (a hello a legfeljebb 1024 bájt mennyiségben).

## <a name="error"></a>Hiba
Hibák (például hibás felhasználói műveletek vagy sikertelen API-hívások) hello alkalmazás által helyesen észlelt használt tooreport problémák.

Hiba lehet kapcsolódó toohello aktuális munkamenet tooa feladat fut, vagy lehet különálló hiba.

Hiba történt a nevét (korlátozott too64 karakter) által azonosított, és tartalmazhatnak néhány további adatokat (a hello a legfeljebb 1024 bájt mennyiségben).

## <a name="job"></a>Feladat
Feladatok olyan időtartammal használt tooreport műveletek (például API-hívások időtartama jelenik meg a hirdetések, időtartama, háttérfeladatok vagy felhasználói műveletek időtartama).

Egy feladat nincs kapcsolódó tooa munkamenet, mert egy feladat elvégezhető hello háttérben, felhasználói beavatkozás nélkül.

A feladat nevét (korlátozott too64 karakter) által azonosított, és tartalmazhatnak néhány további adatokat (a hello a legfeljebb 1024 bájt mennyiségben).

## <a name="crash"></a>Összeomlás
Összeomlásokat automatikusan bocsátja hello Mobile Engagement SDK tooreport alkalmazás összeomlás-hibák, ahol hello alkalmazás nem észlelt problémák teszik.

## <a name="application-information"></a>Alkalmazásadatok
Alkalmazással kapcsolatos adatok (vagy alkalmazásadatok) van használt tootag felhasználók, ez azt jelenti, hogy tooassociate egyes adatok toohello felhasználók egy alkalmazás (Ez a hasonló tooweb cookie-kat, azzal a különbséggel, hogy az alkalmazásadatok tárolása azonban hello kiszolgáló oldalán a hello Azure Mobile Engagement platformján).

Alkalmazásadatok regisztrálható hello Mobile Engagement SDK API használatával, vagy hello a Mobile Engagement platform eszköz API használatával.

Alkalmazásadatok a kulcs/érték pár társított tooa eszközről szó. hello kulcsa hello neve hello alkalmazásadat (korlátozott too64 ASCII-betűből [a-zA-Z], [0-9] számok és aláhúzásjelből (_)). hello érték (korlátozott too1024 karakter) bármilyen karakterlánc, egész szám, dátum (éééé-hh-nn) vagy logikai (IGAZ vagy hamis) lehet.

Bármennyi alkalmazásadat a kapcsolódó tooa eszköz, hello Mobile Engagement díjszabási feltételei által meghatározott hello határokon belül lehet. Egy adott kulcsra vonatkozóan a Mobile Engagement csak nyomon követi az hello legutóbbi értékét követi (az előzményeket nem). Beállítása vagy módosítása egy alkalmazásadat hello érték kényszeríti a Mobile Engagement toore-célközönségre vonatkozó feltételeket az alkalmazás beállítása kiértékelése információ (ha van ilyen), tehát az alkalmazásadatok használt tootrigger valós idejű leküldések lehet.

## <a name="extra-data"></a>További adatok
További adatokat (vagy kiegészítő funkciók) olyan tetszőleges adatok, amelyek csatlakoztatott tooevents, hibákat, tevékenységeket és feladatok lehetnek.

Kiegészítő funkciók felépítése tooJSON objektumok hasonlóképpen: a kulcs/érték párok fa épülnek. A kulcsokban korlátozott too64 ASCII-betűből [a-zA-Z], [0-9] számok és aláhúzásjelből (_)), és kiegészítő funkciók hello teljes mérete (egyszer kódolású JSON hello Mobile Engagement SDK által) korlátozott too1024 karaktereket.

hello kulcs/érték párok teljes fájának tárolása JSON-objektum. Mindazonáltal csak hello első kulcs/érték szintje lebontott toobe közvetlenül elérhető toosome speciális funkciók szegmensekhez (például egyszerűen definiálhat egy szegmenset "Sci-fi-rajongók", amely az összes felhasználó legalább 10 alkalommal hello esemény elküldését nevű "tartalmazza, akik" további kulccsal hello "content_type" a készlet toohello érték "scifi" hello előző hónap). Ezért ajánlott toosend csak kiegészítő funkciók olyan egyszerű listákból a kulcs/érték párok skaláris értékek (például karakterláncok, dátumok, egész szám vagy logikai).

## <a name="next-steps"></a>Következő lépések
* [Windows Universal SDK és Azure Mobile Engagement – áttekintés](mobile-engagement-windows-store-sdk-overview.md)
* [Windows Phone Silverlight SDK és Azure Mobile Engagement – áttekintés](mobile-engagement-windows-phone-sdk-overview.md)
* [iOS SDK és Azure Mobile Engagement](mobile-engagement-ios-sdk-overview.md)
* [Android SDK és Azure Mobile Engagement](mobile-engagement-android-sdk-overview.md)

