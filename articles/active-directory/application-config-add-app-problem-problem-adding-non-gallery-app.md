---
title: "egy nem galéria alkalmazás hozzáadása aaaProblem |} Microsoft Docs"
description: "Hello gyakori problémák személyek arcfelismerési áttekinteni egyéni nem galéria-alkalmazások hozzáadása"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: cfe9b657ae18cbacaddbd85658471a2c57c9cf95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="problem-adding-a-non-gallery-application"></a>A probléma nem galéria-alkalmazás hozzáadása

Ez a cikk segítséget toounderstand hello gyakori problémák személyek arcfelismerési hozzáadásakor **egyéni nem galéria alkalmazások** és mit tehet tooresolve őket. 

## <a name="i-clicked-hello-add-button-and-my-application-took-a-long-time-tooappear"></a>"Hozzáadás" gombra, és az alkalmazás által a hosszú idő tooappear hello gombra kattintás után

Bizonyos körülmények percig is eltarthat, 1 – 2 (és egyes esetekben hosszabb) számára az alkalmazás tooappear tooyour directory adásakor. Ez nem hello normál várt teljesítményét, hello alkalmazás hozzáadása folyamatban van a hello kattintva megtekintheti **értesítések** hello jobb felső sarkában hello (hello harang) ikonra [Azure Portal](https://portal.azure.com/), és keres egy **folyamatban lévő** vagy **befejezve** feliratú értesítési **alkalmazás létrehozása**.

Ha az alkalmazás soha nem adtak hozzá vagy hibát észlel a gombra kattintva hello **hozzáadása** gombra kattint, megjelenik egy **értesítési** a egy **hiba** állapota. Ha azt szeretné, hogy további információkat a támogatási engingeer megosztása több tooor hello hiba toolearn, hello hibával kapcsolatos további tudnivalókért hello hello utasításait követve megtekintheti [hogyan toosee hello adatait a portál értesítései](#how-to-see-the-details-of-a-portal-notification) szakasz.

## <a name="i-clicked-hello-add-button-and-my-application-didnt-appear"></a>I hello "Hozzáadás" gombra kattintott, és az alkalmazás nem jelenik meg

Egyes esetekben tootransient problémák miatt hálózati problémák, vagy egy hiba hozzáadása egy kérelem sikertelen lesz. Beállíthatja, hogy ez akkor fordul elő, amikor hello kattint **értesítések** (hello harang) ikonra az hello jobb felső sarkában hello Azure portálon, és tekintse meg a piros (!) ikon következő tooyour **alkalmazás létrehozása** értesítés. Ez azt jelzi, hogy hibás hello alkalmazás létrehozásakor.

Ha hibát észlel a gombra kattintva hello **Hozzáadás** gombra kattint, megjelenik egy **értesítési** a egy **hiba** állapota. Ha azt szeretné, hogy további információkat a támogatási engingeer megosztása több tooor hello hiba toolearn, hello hibával kapcsolatos további tudnivalókért hello hello utasításait követve megtekintheti [hogyan toosee hello adatait a portál értesítései](#how-to-see-the-details-of-a-portal-notification) szakasz.

## <a name="i-dont-know-how-tooset-up-my-application-once-ive-added-it"></a>Nem tudom, hogy hogyan tooset be egyszer az alkalmazásban felvett azt

Ha egyéni alkalmazások megtanulni segítségre van szüksége, hello [az Azure AD-alkalmazások dokumentumtár](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) toolearn kapcsolatos további információkért az egyszeri bejelentkezés az Azure ad-vel és annak működéséről.

## <a name="how-toosee-hello-details-of-a-portal-notification"></a>Hogyan toosee hello adatait a portál értesítései

A portál értesítései hello részleteit láthatja hello alábbi lépéseket követve:

1.  Kattintson a hello **értesítések** hello hello Azure portál jobb felső sarkában (hello harang) ikonra

2.  Válassza ki az értesítésekhez egy **hiba** (piros (!) következő toothem rendelkezők) állapotát.

   >[!NOTE]
   >Nem kattint az értesítések egy **sikeres** vagy **folyamatban lévő** állapotát.
   >
   >

3.  A nyitott hello **értesítési részletek** panelen.

4.  Ezt az információt használja saját kezűleg toounderstand több hello probléma vonatkozó részletek.

5.  Ha további segítségre van, is megoszthatja ezeket az információkat a támogatási szakértőhöz vagy hello csoport tooget Terméksúgó a probléma megoldásában.

6.  Hello kattintson **másolás ikon** sarkában hello toohello **hiba másolása** szövegmező toocopy összes hello értesítési részletek tooshare és a támogatási szolgálathoz vagy a termék csoport mérnöke.

## <a name="how-tooget-help-by-sending-notification-details-tooa-support-engineer"></a>Hogyan segíthet az tooget értesítési részletek tooa támogatási szakértőhöz küldése

Nagyon fontos, hogy megosztott **összes alább felsorolt részletek hello** , ha segítségre van szüksége, így gyorsan segít a támogatási szakértőhöz. Ehhez egyszerűen az **elkészíti a képernyőfelvételt** vagy hello kattintva **másolási hibaikon**, hello toohello jobbra található **hiba másolása** szövegmező.

## <a name="notification-details-explained"></a>Értesítési részletek alapján

az alábbi hello több milyen egyes hello értesítési elemek jelent, és azok által biztosított példák ismerteti.

### <a name="essential-notification-items"></a>Alapvető értesítési elemek

-   **Cím** – hello informatív címet hello értesítés
   *  Példa – **Application proxy beállításai**

-   **Leírás** – hello Mi történt hello művelet leírása

   *  Példa – **megadott belső URL-cím már használatban van egy másik alkalmazás.**

-   **Értesítés azonosítója** – hello értesítési hello egyedi azonosítója

   *  Példa – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**

-   **Ügyfélkérelem-azonosító** – a böngésző által végrehajtott hello adott kérelem azonosítója

   *  Példa – **302fd775-3329-4670-a9f3-bea37004f0bc**

-   **Stamp UTC-idő** – hello időbélyeg időintervalluma hello értesítési történt UTC szerint

   *  Példa – **2017-03-23T19:50:43.7583681Z**

-   **Belső tranzakció azonosítója** – hello használhatjuk toolook hello hiba a rendszer belső azonosítója

   *  Példa – **71a2f329-ca29-402f-aa72-bc00a7aca603**

-   **Egyszerű felhasználónév** – hello műveletet végre hello felhasználó

   *  – Példa**tperkins@f128.info**

-   **A bérlői azonosító** – hello egyedi Azonosítóját hello olyan bérlő számára, felhasználói hello hello műveletet végrehajtó tagja volt.

   *  Példa – **7918d4b5-0442-4a97-be2d-36f9f9962ece**

-   **Felhasználói objektum azonosítója** – hello hello műveletet végre hello felhasználó egyedi azonosítója

 *  Példa – **17f84be4-51f8-483a-b533-383791227a99**

### <a name="detailed-notification-items"></a>Részletes értesítési elemek

-   **Megjelenítendő név** – **(üres is lehet)** hello hiba részletes megjelenítendő neve

  *  Példa – **Application proxy beállításai**

-   **Állapot** – hello hello értesítési adott állapota

   *  Példa – **nem sikerült**

-   **Objektumazonosító:** – **(üres is lehet)** hello Objektumazonosító alapján mely hello művelet végrehajtása

   *  Példa – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**

-   **Részletek** – hello részletes Mi történt hello művelet leírása

   *  Példa – **belső URL-cím "http://bing.com/" érvénytelen, mert már használatban van**

-   **Másolja át a hiba** – hello kattintson **másolás ikon** sarkában hello toohello **hiba másolása** szövegmező toocopy összes hello értesítési részletek tooshare a támogatási szolgálathoz vagy a termék csoport visszafejtés

   *  Példa```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'http://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'http://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

## <a name="next-steps"></a>Következő lépések
[Alkalmazások kezelése az Azure Active Directoryban](active-directory-enable-sso-scenario.md)



