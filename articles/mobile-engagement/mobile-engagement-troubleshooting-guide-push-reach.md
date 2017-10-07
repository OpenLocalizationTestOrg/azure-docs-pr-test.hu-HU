---
title: "Mobile Engagement hibaelhárítási útmutatója - aaaAzure leküldéses/Reach"
description: "Azure Mobile Engagement felhasználói beavatkozás és értesítési problémák elhárítása"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 3f1886b7-1fdd-47f4-b6b0-d79f158d5ef3
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 4ee0b34b9b753a98ccf24863acb28a5dc76bfb95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-push-and-reach-issues"></a>Hibaelhárítási útmutató a leküldési és elérési hibákhoz
Az alábbiakban hello lehetséges problémák merülhetnek fel a hogyan Azure Mobile Engagement küld információt tooyour felhasználók.

## <a name="push-failures"></a>Leküldéses hibák
### <a name="issue"></a>Probléma
* Leküldéses értesítések nem használhatók (az alkalmazás, alkalmazás, vagy mindkettő).

### <a name="causes"></a>Okok
* Sokszor leküldéses hiba arra utal, hogy Azure Mobile Engagement, Reach vagy az Azure Mobile Engagement egy másik speciális szolgáltatás nincs megfelelően integrálva, és hogy van-e frissítés szükséges hello SDK toofix egy ismert probléma az új operációs rendszer vagy az eszköz platformja.
* Ha az alkalmazás vagy kívüli alkalmazás probléma valami, tesztelje a csak egy alkalmazást a leküldéses és csak egy kívüli alkalmazás leküldéses toodetermine.
* Tesztelje a hello felhasználói felület és a hello API, egy hibaelhárítási lépés toosee milyen további információkat mindkét helyen.
* Alkalmazásból leküldéses értesítések nem fog működni, kivéve, ha mind az Azure Mobile Engagement, és a Reach hello SDK van integrálva.
* Leküldéses értesítések nem fog működni, ha tanúsítványokat érvénytelenek, vagy termék vs használ. Fejlesztői megfelelően (csak iOS esetén). (**Megjegyzés:** "kívüli alkalmazás" leküldéses értesítések nem terjeszthetők tooiOS, ha mindkét hello fejlesztési (fejlesztés) és az alkalmazás üzemi (termék) verzió telepítve hello azonos óta hello biztonsági jogkivonat társított eszközök a tanúsítványhoz esetleg érvényteleníti Apple. tooresolve probléma hello fejlesztői és az alkalmazás termék verzióit is el, majd telepítse újra a csak hello egy verziót az eszközön.)
* Alkalmazásból leküldéses számok kezeli eltérően (iOS információval is kevesebb mint Android Ha a natív leküldéses értesítések le vannak tiltva az eszközön, API hello felhasználói felület több információt biztosíthat a leküldéses statisztikák hello) különböző platformokon.
* Alkalmazásból leküldéses értesítések blokkolhatja az ügyfelek operációs rendszer szintjén (iOS és Android).
* Alkalmazásból leküldéses értesítések jelenik meg az Azure Mobile Engagement felhasználói felületén hello le van tiltva, ha nem megfelelően integrált, de a hello API csendes meghiúsulhat.
* Alkalmazás leküldéses értesítések nem fog működni, kivéve, ha mind az Azure Mobile Engagement, és a Reach hello SDK van integrálva.
* GCM és ADM leküldéses értesítések nem fog működni, kivéve, ha hello (csak Android) SDK integrált Azure Mobile Engagement és hello adott kiszolgálóra.
* Az alkalmazás és leküldéses értesítések kell App kívüli tesztelt külön-külön toodetermine Ha leküldéses vagy Reach hiba történt.
* Az alkalmazás leküldéses értesítések követelmény hello alkalmazást nyitott toobe kapott.
* Az alkalmazás-leküldéses értesítések gyakran telepítő toobe egy alkalmazás szolgáltatás aktiválása vagy a letiltási info címke szerint szűrve.
* Ha egy egyéni kategória használhatja a Reach toodisplay az alkalmazásbeli értesítésekben, toofollow hello megfelelő életciklusát hello értesítési van szüksége, ellenkező esetben hello értesítési törlése nem lehetséges, hogy mikor hello felhasználói üzenet bezárásához.
* Ha nincs befejezési dátum kampány kezdődik, és egy eszköz hello fogadása alkalmazásban megjelenő értesítésre, de nem jelenik meg, még, hello felhasználó továbbra is kap hello értesítési hello legközelebb bejelentkezik hello alkalmazást, akkor is, ha manuálisan befejezi a hello kampány.
* Hello leküldéses API problémáinak győződjön meg arról, hogy valóban szeretné, hogy toouse hello leküldéses API helyett hello Reach API (mivel hello Reach API több gyakran használt), és hogy nem egyértelmű hello "tartalom" és "bejelentő" paramétert.
* Tesztelje a leküldéses kampány eszközzel mindkét keresztül Wi-Fi és a 3G tooeliminate hello hálózati kapcsolati problémák lehetséges forrásaként.

## <a name="push-testing"></a>Leküldéses tesztelése
### <a name="issue"></a>Probléma
* Leküldéses értesítések küldhetők tooa eszközre alapján egy eszköz azonosítót.

### <a name="causes"></a>Okok
* Teszteszközök telepítő eltérően az egyes platformokhoz készült, de egy eseményt, amely egy vizsgálati eszköz az alkalmazásban, és keres az Eszközazonosítót hello portálon kell működnie toofind az Eszközazonosítót az összes platformra.
* Teszteszközök működnek másképp a vs idfa-JÁT. Idfv fogja elvégezni (csak iOS esetén). lehetővé.

## <a name="push-customization"></a>Leküldéses testreszabása
### <a name="issue"></a>Probléma
* Speciális elem nem fog működni a tartalom leküldéses (jelvények, gyűrű, alaplemezzel, kép, stb.).
* Leküldéses értesítések hivatkozások nem működnek (kívül, belül app, tooa webhely, alkalmazás tooa helye).
* Leküldéses statisztika megjelenítése, amely egy leküldéses nem küldött tooas az elvárásoknak megfelelően sokan (túl sok vagy nem elegendő).
* Leküldéses ismétlődik, és kétszer kapott.
* Nem lehet regisztrálni vizsgálati eszköz az Azure Mobile Engagement leküldéses értesítések (a saját termék vagy fejlesztői alkalmazással).

### <a name="causes"></a>Okok
* toolink tooa adott helyen alkalmazásban "kategóriák" (csak Android esetén) van szükség.
* Részletes sémák linking tooredirect felhasználók tooan másik helyre leküldéses értesítés kattintás után kell toobe létrehozott és az alkalmazás- és hello eszköz operációs rendszere nem a Mobile Engagement közvetlenül kezeli. (**Megjegyzés:** alkalmazásból értesítések nem hivatkozhat közvetlenül iOS app helyekre tooin csak az Android tudja.)
* Külső kép kiszolgálóknak kell toobe képes toouse HTTP: "GET" és "HEAD" a nagy vonalakban tekinti leküldéses értesítések toowork (csak Android esetén).
* A kódban, tiltsa le a hello Azure Mobile Engagement ügynök hello billentyűzet megnyitásakor, és újra kell aktiválniuk hello Azure Mobile Engagement ügynök hello billentyűzet le van zárva, így hello billentyűzet nincsenek hatással a hello megjelenése után a kód a értesítés (csak iOS esetén).
* Bizonyos elemek nem működik a teszt szimulációja, de csak valós kampányok (jelvények, gyűrű, alaplemezzel, kép, stb.).
* Nem a rendszer adatokat naplózza hello gomb használatakor kiszolgálóoldali túl "teszt" leküldéses értesítések. Adatok csak a rendszer naplózza a valódi leküldéses kampányokra.
* toohelp a problémát, elhárítása: tesztelje, szimulálása, és egy valódi kampányt, mivel ezek mindegyike kissé eltérően működik.
* hello mennyi ideig a "alkalmazás" és "minden alkalommal" kampányok ütemezett toorun is érvényesíti kézbesítési számok óta kampány csak akkor kézbesíti toousers, akik "alkalmazás" hello kampány futtatása közben (és az Eszközbeállítások rendelkező felhasználók tooreceive beállítása Értesítések küldése "kívüli alkalmazás").
* Hogyan Android és iOS leíró alkalmazásbeli értesítésekben kívül teszi, hogy nehéz toodirectly hello különbségei az alkalmazás hello Android és iOS verziója leküldéses statisztikáinak összehasonlítására. Android további információk az operációs rendszer szintű értesítést, mint iOS. Android jelentések natív értesítést kapott, kattintott, vagy hello értesítési központ törölt, de iOS nem készít jelentést ezt az információt, kivéve, ha hello értesítési kattint. 
* hello legfőbb oka, hogy "megnyomott" számok nem ugyanaz, mint más, mint az "i" számok kampányok elérni, hogy "az alkalmazás" és "kívül" értesítések számlálási másképp. "Az alkalmazás" értesítések a Mobile Engagement kezeli, de "alkalmazás" verzióról értesítéseket hello értesítési központ hello az operációs rendszer az eszköz a kezel.

## <a name="push-targeting"></a>Célcsoport-kezelési leküldéses
### <a name="issue"></a>Probléma
* A beépített célcsoport-kezelési nem a várt módon működik.
* Alkalmazásadatok címke célcsoport-kezelési nem működik megfelelően.
* Földrajzihely-célzó nem működik megfelelően.
* Nyelvi beállítások nem a várt módon működnek

### <a name="causes"></a>Okok
* Győződjön meg arról, hogy már feltöltött alkalmazás info címkék hello Azure Mobile Engagement felhasználói felületén vagy a API használatával.
* Sávszélesség-szabályozási hello leküldéses sebessége vagy a leküldéses kvóta hello alkalmazás szinten, vagy korlátozó hello célközönség hello kampány szinten előfordulhat, hogy egy személy egy adott leküldéses fogadjon, még akkor is, ha megfelelnek-e a többi célcsoport-kezelési feltételeknek. 
* A "nyelv" beállítás különbözik attól a célcsoport-kezelési ország alapján vagy területi beállítás, amely egyben célcsoport-kezelési eltérő földrajzi helyhez alapján phone helyre vagy GPS helye alapján.
* a "nyelv alapértelmezett" hello hello üzenetet küld tooany ügyfél, aki nem rendelkezik az eszköz tooone hello megadhat alternatív nyelv beállítása.

## <a name="push-scheduling"></a>Leküldéses ütemezése
### <a name="issue"></a>Probléma
* Leküldéses ütemezés nem működik az elvárásoknak megfelelően (küldött túl korai vagy késleltetett).

### <a name="causes"></a>Okok
* Időzóna az ütemezés, problémák is, különösen akkor, ha hello végfelhasználók számára időzónájának használatával.
* Speciális leküldéses szolgáltatások késleltetheti-e leküldéses értesítések.
* Leküldéses értesítések célzó phone késleltetheti-beállításai (helyett App-Info címkék) alapján, mivel Azure Mobile Engagement leküldéses elküldése előtt esetleg hello phone valós idejű toorequest adatait.
* A záró dátum nélküli létrehozott kampányok hello leküldéses hello eszköz helyileg tárolja, és szerepel, hogy hello következő megnyitásakor hello alkalmazás akkor is, ha hello kampány manuálisan véget ér.
* Egynél több kampány indítása hello egyszerre is készítsen egy hosszabb idő tooscan a felhasználói bázis (maximum négy egyszerre tooonly start egy kampány próbálja, célzott tooyour aktív felhasználókra, hogy a régi felhasználóknak nem kell beolvasott toobe).
* Hello "Ignore célközönség leküldéssel fogják megkapni toousers keresztül hello API" beállítás használatakor a Reach-kampány hello "Kampány" szakaszban nem automatikusan küldi hello kampány, szüksége lesz a toosend azt manuálisan hello Reach API.
* Ha egy egyéni kategória használhatja a Reach toodisplay alkalmazáson belüli értesítések, toofollow hello megfelelő életciklus-értesítést van szüksége, ellenkező esetben hello értesítési nem lehet kiüríteni amikor hello felhasználói üzenet bezárásához.

