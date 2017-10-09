---
title: aaaUnderstanding hello OAuth2 implicit folyamata adja meg az Azure AD |} Microsoft Docs
description: "További tudnivalók az Azure Active Directory végrehajtásának hello OAuth2 implicit grant flow, és hogy-e megfelelő az alkalmazás."
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 90e42ff9-43b0-4b4f-a222-51df847b2a8d
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/15/2016
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 3402e6619709e1a5b1e790ffd79dc62139552d9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-hello-oauth2-implicit-grant-flow-in-azure-active-directory-ad"></a>Understanding hello OAuth2 implicit adja meg az Azure Active Directory (AD) folyamata
az OAuth2 hello implicit grant notorious ahhoz, hogy meg hello grant hello leghosszabb hello OAuth2 specifikációjában biztonsági szempontok listáját. És még, amely ADAL JS és egy ajánlott SPA alkalmazások írásához hello által megvalósított hello megközelítést. Mi ad? Mellékhatásokkal összes kérdése:, és akkor kapcsolja, mert hello implicit grant hello legjobb módszer az alkalmazások programozása egy webes API JavaScripttel böngészőből is végezze el.

## <a name="what-is-hello-oauth2-implicit-grant"></a>Mi az az hello OAuth2 implicit támogatás?
hello quintessential [OAuth2 engedélyezési code grant](https://tools.ietf.org/html/rfc6749#section-1.3.1) hello hitelesítésengedélyezési két külön végpontok használó van. hello engedélyezési végpont egy engedélyezési kódot hello felhasználói beavatkozás fázis használható. hello token-végpont majd használják hello ügyfél olyan hozzáférési jogkivonatot, és gyakran egy frissítési is hello kód cseréjét. Webes alkalmazások vannak a saját alkalmazás hitelesítő adatok toohello jogkivonat végpontjához szükséges toopresent, hogy hello engedélyezési kiszolgáló hitelesíthető hello ügyfél.

Hello [OAuth2 implicit grant](https://tools.ietf.org/html/rfc6749#section-1.3.2) egy változata, más engedélyt biztosít. Ez lehetővé teszi, hogy egy ügyfél tooobtain olyan hozzáférési jogkivonatot (és id_token, használatakor [OpenId Connect](http://openid.net/specs/openid-connect-core-1_0.html)) közvetlenül a hello engedélyezési végpont, lépjen kapcsolatba a jogkivonat végpontjához hello sem hello ügyfél hitelesítése nélkül. A variant JavaScript-alapú böngészőben futtatott alkalmazások lett kifejezetten: hello eredeti OAuth2 specifikációjában tokenek vissza az URI-töredéket. Így hello token bits elérhető toohello JavaScript-kód hello ügyfél, de azt biztosítja, hogy nem fog szerepelni a átirányítások hello kiszolgáló felé. Közvetlenül a hello engedélyezési végpont jogkivonatok visszaadó böngésző keresztül irányítja át. Azt is hello előnyeit, így kiküszöböli eredetű hívások indítását, szükséges, ha a JavaScript-alkalmazását hello szükséges toocontact hello token-végpont közötti vonatkozó követelmények.

Az hello OAuth2 implicit grant fontos jellemzője hello tényt, hogy ilyen adatfolyamok soha nem adja vissza a frissítési jogkivonatokat toohello ügyfél. Hello a következő szakasz azt látja, mint, nincs szükség, és ténylegesen lenne a biztonsági problémákból.

## <a name="suitable-scenarios-for-hello-oauth2-implicit-grant"></a>Adja meg hello OAuth2 implicit megfelelő forgatókönyvei
Hello OAuth2 specification maga deklarál, mivel hello implicit grant lett kidolgozott tooenable felhasználói ügynök alkalmazások – toosay által végrehajtott egy böngésző JavaScript-alkalmazások. az ilyen kérelmek jellemző meghatározása hello, hogy (általában egy webes API) kiszolgáló-erőforrások eléréséhez, és ennek megfelelően frissítési hello alkalmazás UX használja-e a JavaScript-kódot. Az alkalmazások, például a Gmailes vagy Outlook Web Access gondoljon: a beérkezett üzenet kiválasztásakor képi megjelenítés panel változik toodisplay hello új kijelölés, közben hello rest hello lap csak üdvözlőüzenetére változatlan marad. Ez a ellentétben hagyományos átirányításon alapuló webalkalmazások, ahol a teljes lap visszaküldés és a teljes lap megjelenítésének hello új kiszolgálóválasz eredményez-e a minden felhasználói beavatkozást.

Alkalmazásokat, amelyek hello JavaScript-alapú módszer tooits szélsőséges egyetlen oldal alkalmazások vagy gyógyfürdők nevezzük: hello lényege, hogy ezeket az alkalmazásokat csak osztja ki az egy kezdeti HTML-weblap és kapcsolódó JavaScript-az összes későbbi kapcsolati kritikus előfeltételei: Webes API-hívások JavaScripttel végre. Azonban hibrid megoldások, ahol hello alkalmazás többnyire visszaküldés adatvezérelt de alkalmi JS-hívást hajt végre, amelyek nem ritka, – hello vitafórum implicit engedélyezési folyamat használatával kapcsolatos fontos azok is.

Átirányításon alapuló alkalmazások általában biztonságos megközelítés JavaScript-alkalmazások esetében nem működik, valamint a kérelmek cookie-kat, azonban keresztül. Cookie-kat csak azok hozták létre, amíg JavaScript-hívásokat előfordulhat, hogy rendszer felé más tartományok hello tartomány alapján működik. Valójában gyakran fogja tárolni hello eset: gondoljon meghívása Microsoft Graph API-t Office API-t Azure API – amennyiben van lehetséges a kiszolgálása hello alkalmazás hello tartományon kívüli összes található alkalmazások. JavaScript-alkalmazásokhoz az egyre növekvő tendenciát egyáltalán toohave nincs háttér, függő 100 %-a 3. fél webes API-k tooimplement üzleti működésére.

Hello preferált hívások tooa Web API védelme jelenleg toouse hello OAuth2 tulajdonosi jogkivonat módszert használja, ahol minden hívás együtt egy OAuth2-jogkivonatot. Webes API hello megvizsgálja hello bejövő jogkivonatot, és ha talál azt hello szükséges hatókörök, akkor engedélyezi a hozzáférést toohello a kért műveletet. hello implicit engedélyezési folyamat lehetővé teszi a kényelmes JavaScript alkalmazások tooobtain hozzáférési jogkivonatok egy webes API számos előnye, figyelembe vegyék toocookies kínál:

* Jogkivonatok nélkül eredetű hívások indítását közötti megbízható szerezhetők – hello átirányítási URI toowhich jogkivonatok vannak vissza kötelező regisztrációja garantálja, hogy a rendszer nem kiszorított jogkivonatok
* JavaScript alkalmazások szerezhet be van szükségük, a lehető legtöbb webes API-k célként – tartományok korlátozás nélkül annyi hozzáférési jogkivonatok
* HTML5-szolgáltatások, például munkamenet, vagy a helyi tároló teljes hozzáférés token-gyorsítótárazási és életciklusának kezelését, mivel a cookie-k kezelése nem átlátszó toohello alkalmazás
* Hozzáférési jogkivonatok nem ki vannak téve tooCross-hely kérelem (CSRF) hamisítási támadások

hello implicit grant flow frissítési jogkivonatokat, főleg biztonsági okokból nem ad ki. Egy frissítési jogkivonat szűken, mint a hozzáférési jogkivonatok, ezért anyagi sokkal több kárt, abban az esetben, ha kiszivárgott sokkal nagyobb teljesítmény biztosítása nem hatókörét. Hello implicit engedélyezési folyamat, a jogkivonatok érkeznek hello URL-cím, ezért az hozzáférés hello veszélye magasabb, mint a hello hitelesítésengedélyezési kódot.

Vegye figyelembe azonban, hogy rendelkezik-e a JavaScript-alkalmazását egy másik mechanizmus a rendelkezésére a hozzáférési jogkivonatok megújítása ismételten a hitelesítő adatok hello felhasználó értesítése nélkül. hello alkalmazás használhat egy rejtett iframe tooperform új jogkivonat-kérelmeket elleni hello engedélyezési végpont az Azure AD: mindaddig, amíg hello böngésző még aktív munkamenet (olvassa el: egy munkamenetcookie tartozik) hello hello Azure AD-tartomány, a hitelesítési kérelem sikeresen fordulhat elő, felhasználói beavatkozás nélkül.

Ez a modell szerepkörök hello JavaScript alkalmazás hello tooindependently újítsa meg a jogkivonatot, és még szerezzen be egy új API-újakat (feltéve, hogy hello felhasználó korábban átadni kívánt hozzájárult e számukra. Ezzel elkerülhető az beszerzése, karbantartása és védelme a nagy értékű összetevő, például egy frissítési jogkivonat hozzáadott terhétől hello. hello összetevő hello csendes megújítási lehetséges, így hello Azure AD munkamenet cookie-k kezelése hello alkalmazáson kívülre. Ezt a módszert használja egy másik előnye, egy felhasználó bejelentkezhessen az Azure AD használatával hello alkalmazások az Azure AD, minden böngészőlapokon hello fut be van jelentkezve. Az eredmény hello törlésének hello Azure AD munkamenetcookie-t, és hello JavaScript-alkalmazás automatikusan elveszítik hello képességét, toorenew jogkivonatokat hello kijelentkezteti a felhasználót a.

## <a name="is-hello-implicit-grant-suitable-for-my-app"></a>Alkalmas hello implicit grant alkalmazásom?
hello implicit grant mutatja be, mint más biztosít további kockázatok, és megfelelően dokumentálva toopay figyelmet tooare hello területek kell. Például [visszaélés a hozzáférési jogkivonat tooImpersonate erőforrás tulajdonosa az Implicit Flow] [ OAuth2-Spec-Implicit-Misuse] és [OAuth 2.0 fenyegetések modellezése és biztonsági megfontolások] [ OAuth2-Threat-Model-And-Security-Implications]). Hello magasabb kockázatú profil azonban nagy mértékben miatt toohello tényt, hogy azt jelenti, tooenable alkalmazások, amelyek aktív kódot, a távoli erőforrás tooa webböngésző szolgálja ki. Ha azt tervezi, olyan SPA architektúrát nem háttér-összetevők vagy a tooinvoke egy webes API JavaScripttel, jogkivonat beszerzése az implicit engedélyezési folyamat hello használata ajánlott.

Ha az alkalmazás egy natív ügyfél, hello implicit engedélyezési folyamat nem kiváló méretezése. hello Azure AD munkamenetcookie-t natív ügyfelek hello környezetében hello hiányában megfosztja hello azt jelenti, hogy a hosszú élettartamú munkamenet fenntartásának az alkalmazás. Ami azt jelenti, hogy az alkalmazás ismételten figyelmeztetik hello hozzáférési jogkivonatok az új erőforrások beszerzésekor.

Ha egy webalkalmazást, beleértve a háttérkiszolgálón és felhasználása az API-k, a háttér kódból fejleszt, hello implicit engedélyezési folyamat is nem remekül beválik. Más biztosít Önnek sokkal nagyobb teljesítmény. Például hello OAuth2 ügyfél hitelesítő adatok megadása nyújt hello képességét tooobtain jogkivonatok hello engedélyek tükröző toohello alkalmazás magát, mint megakadályozását toouser delegálásokat hozzárendelése. Ez azt jelenti, hogy hello ügyfél hello képességét toomaintain programozott hozzáférés tooresources rendelkezik, akkor is, ha a felhasználó nem aktívan folytat egy munkamenetet, és így tovább. Nem csak az, hogy, de az ilyen adjon magasabb biztonsági garanciákat. Például hozzáférési jogkivonatok soha nem tranzit keresztül hello felhasználó böngészőben, nem kockáztatja a hello böngészési előzmények mentése zajlik, és így tovább. hello ügyfélalkalmazás is elvégezheti erős hitelesítés, amikor a jogkivonat kérésével.

## <a name="next-steps"></a>Következő lépések
* Fejlesztői erőforrások teljes listáját, beleértve referencia jellegű információt hello protokollok és OAuth2 engedélyezési adatfolyamok támogatás az Azure ad nyújtására tekintse meg a toohello [Azure AD fejlesztői útmutató][AAD-Developers-Guide]
* Lásd: [hogyan toointegrate az Azure AD alkalmazás] [ ACOM-How-To-Integrate] további mélység hello alkalmazás integrációs folyamatban.

<!--Image references-->

<!--Reference style links in use-->
[AAD-Developers-Guide]: active-directory-developers-guide.md
[ACOM-How-And-Why-Apps-Added-To-AAD]: active-directory-how-applications-are-added.md
[ACOM-How-To-Integrate]: active-directory-how-to-integrate.md
[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819
