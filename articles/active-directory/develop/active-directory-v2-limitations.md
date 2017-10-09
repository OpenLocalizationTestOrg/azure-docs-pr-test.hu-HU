---
title: "az Active Directory aaaAzure v2.0 végpont korlátai és korlátozásai |} Microsoft Docs"
description: "Korlátozások és hello Azure AD v2.0-végpontra vonatkozó korlátozások listáját."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: a99289c0-e6ce-410c-94f6-c279387b4f66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: bcbb7239f1d117002d16ac23dca8f1feb13a9161
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="should-i-use-hello-v20-endpoint"></a>Hello v2.0-végponttól érdemes használni?
Alkalmazások, amelyekbe beépül az Azure Active Directory összeállításakor toodecide szüksége e hello v2.0 végpont és a hitelesítési protokoll az igényeinek. Az Azure Active Directory eredeti végpont továbbra is teljes mértékben támogatja, és néhány tekintetben a v2.0-nál több szolgáltatás gazdag. Azonban a v2.0-végponttól hello [vezet be, jelentős előnyt](active-directory-v2-compare.md) a fejlesztők számára.

Az egyszerűsített ajánlott megoldás a fejlesztők számára ezen a ponton a időben történő itt található:

* Ha személyes Microsoft-fiókok támogatnia kell az alkalmazásban, használja a hello v2.0-végponttól. De előtt, ne feledje, hogy tudomásul veszi, amely arról lesz szó ebben a cikkben hello korlátozások.
* Ha az alkalmazásnak csak toosupport Microsoft munkahelyi és iskolai fiókok, ne használjon hello v2.0-végponttól. Ehelyett tekintse meg a tooour [az Azure Active Directory fejlesztői útmutatója](active-directory-developers-guide.md).

Az idő múlásával hello v2.0-végponttól nő tooeliminate hello korlátozások itt látható, hogy csak akkor toouse hello v2.0-végponttól. Hello addig is, ez a cikk tervezett toohelp határozhatja meg, hogy-e az Ön számára legmegfelelőbb hello v2.0-végponttól. Folytatjuk tooupdate Ez a cikk tooreflect hello aktuális állapotának hello v2.0-végponttól. Ellenőrizze a v2.0-képességek vonatkozó követelmények tooreevaluate biztonsági másolatot.

Ha egy meglévő Azure AD-alkalmazás hello v2.0-végpontra nem használó, nincs teljesen új nincs szükség toostart. A jövőbeli hello lesz elérhető úgy az Ön toouse a meglévő Azure AD-alkalmazások hello v2.0-végponttal.

## <a name="restrictions-on-app-types"></a>Alkalmazástípusok korlátozásai
Jelenleg hello következő típusú alkalmazásokat nem támogatottak az hello v2.0-végponttól. Támogatott alkalmazástípusok ismertetését lásd: [alkalmazástípusok hello Azure Active Directory v2.0-végpontra vonatkozó](active-directory-v2-flows.md).

### <a name="standalone-web-apis"></a>Önálló webes API-k
Hello v2.0-végponttól használhatja túl[létrehozása egy webes API védett OAuth 2.0](active-directory-v2-flows.md#web-apis). Azonban, hogy a webes API-k képes jogkivonatokat fogadni csak olyan alkalmazás, amely rendelkezik hello ugyanazon alkalmazás azonosítóját. Nem fér hozzá egy webes API-t egy ügyfél, amely rendelkezik egy másik alkalmazás azonosítóját. hello ügyfél nem fog tudni toorequest vagy engedélyek tooyour Web API beszerzése.

Hogyan toobuild egy webes API, amely fogad, amely rendelkezik ügyféltől érkező jogkivonatok hello azonos Alkalmazásazonosítót: toosee hello v2.0 végpont Web API minta a [bevezetés](active-directory-appmodel-v2-overview.md#getting-started) szakasz.

## <a name="restrictions-on-app-registrations"></a>Alkalmazás regisztrációk korlátozásai
Jelenleg minden alkalmazás, amelyet az toointegrate hello v2.0-végponttal, létre kell hozni egy alkalmazást regisztrációs hello új [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). Meglévő Azure AD vagy a Microsoft-fiók alkalmazások nem kompatibilisek-e hello v2.0-végponttól. A portálon kívül hello alkalmazásregisztrációs portálra regisztrált alkalmazások nem kompatibilisek-e hello v2.0-végponttól. A jövőbeli hello hogy terv tooprovide egy módon toouse v2.0 alkalmazásként egy meglévő alkalmazást. Jelenleg azonban nincs áttelepítési útvonal az egy meglévő app toowork hello v2.0-végponttal.

Emellett a hello létrehozott alkalmazás regisztrációk [alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) hello a következő korlátozásokkal rendelkezik:

* Csak kettő használható azonosítót.
* Egy alkalmazás regisztrációs személyes Microsoft-fiókkal rendelkező felhasználó által regisztrált tekinthetők meg és felügyeli csak egyetlen fejlesztői fiók létrehozása. Több fejlesztők között nem lehet megosztani.  Ha azt szeretné, hogy tooshare az alkalmazás regisztrációját között több fejlesztők számára, létrehozhat hello alkalmazás hello regisztrációs portálon az Azure AD-fiókkal bejelentkezni.
* Nincsenek hello formátumának hello átirányítási URI-t, hogy több korlátozásait. Átirányítási URI-kkal kapcsolatos további információkért lásd a hello következő szakaszt.

## <a name="restrictions-on-redirect-uris"></a>Átirányítási URI-k korlátozásai
Az alkalmazásregisztrációs portálra hello regisztrált alkalmazások jelenleg korlátozott tooa korlátozott készletét átirányítási URI-értékek. az átirányítási URI-JÁNAK webalkalmazások és szolgáltatások hello sémát kell kezdődnie hello `https`, és az összes átirányítási URI-értékek közös egyetlen DNS-tartományt. Például egy webalkalmazást, hogy rendelkezik-e ezeket az átirányítási URI-azonosítók nem regisztrálható:

`https://login-east.contoso.com`  
`https://login-west.contoso.com`

hello regisztrációs rendszer összehasonlítja a hello meglévő átirányítási URI toohello DNS-neve hello átirányítási URI-t, hogy a hozzáadni kívánt hello teljes DNS-nevét. hello kérelem tooadd hello DNS-név sikertelen lesz, ha hello a következő feltételek valamelyike teljesül:  

* hello új átirányítási URI-t hello teljes DNS-neve nem egyezik meg a DNS-neve hello hello meglévő átirányítási URI-t.
* teljes DNS-neve hello hello új átirányítási URI-t altartománya nem hello meglévő átirányítási URI-t.

Ha például hello az alkalmazás rendelkezik az átirányítási URI-ja:

`https://login.contoso.com`

Hozzáadhat tooit ehhez hasonló:

`https://login.contoso.com/new`

Ebben az esetben hello DNS-név pontosan megegyezik. Esetleg a következőt teheti meg:

`https://new.login.contoso.com`

Ebben az esetben hivatkozott login.contoso.com a tooa DNS altartományában. Ha azt szeretné, hogy egy alkalmazás, amely rendelkezik bejelentkezési-east.contoso.com és a bejelentkezési-west.contoso.com átirányítási URI-ként toohave, hozzá kell adnia azokat átirányítási URI-azonosítók, az itt megadott sorrendben:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

Ez utóbbi két mert hello az altartományok először átirányítási URI-címe, hello is hozzáadhat contoso.com. Ez a korlátozás egy jövőbeli verzióban törlődni fog.

Hogyan hello alkalmazásregisztrációs portálra, az alkalmazás egy tooregister: toolearn [hogyan tooregister egy alkalmazást a v2.0-végponttól hello](active-directory-v2-app-registration.md).

## <a name="restrictions-on-services-and-apis"></a>Szolgáltatások és API-k korlátozásai
Jelenleg hello v2.0-végponttól támogatja bejelentkezési bármely alkalmazás, amely regisztrálva van a hello alkalmazásregisztrációs portálra, és amely hello listája esik [támogatott hitelesítési forgalom](active-directory-v2-flows.md). Ezek az alkalmazások azonban OAuth 2.0 hozzáférési jogkivonatok erőforrások nagyon korlátozott számú szerezheti be. hello v2.0 végpont problémák hozzáférési jogkivonatainak csak:

* hello alkalmazás által kért hello jogkivonat. Egy alkalmazás szerezhetnek be a hozzáférési tokent magának, ha hello logikai alkalmazás számos különböző összetevői vagy rétegek áll. toosee ebben a forgatókönyvben a művelet, tekintse meg a [bevezetés](active-directory-appmodel-v2-overview.md#getting-started) oktatóanyagok.
* hello Outlook posta, naptár és Contacts REST API-k, amelyek https://outlook.office.com találhatók. toolearn hogyan toowrite fér hozzá ezen API-k, alkalmazás: hello [Office bevezetés](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) oktatóanyagok.
* A Microsoft Graph API-k. További tudnivalók [Microsoft Graph](https://graph.microsoft.io) és a rendelkezésre álló tooyou hello adatokról.

Nincsenek más szolgáltatások jelenleg támogatottak. További Microsoft Online Services megjelenik hello jövőbeli, továbbá toosupport saját egyedi webes API-k és a szolgáltatások.

## <a name="restrictions-on-libraries-and-sdks"></a>Szalagtárak és SDK-k korlátozásai
Szalagtár támogatása hello v2.0-végponttól jelenleg korlátozott. Ha azt szeretné, hogy toouse hello v2.0-végponttól egy éles alkalmazásban, ezen lehetőség közül választhat:

* Ha egy webalkalmazást, Microsoft általánosan elérhető kiszolgálóoldali köztes tooperform bejelentkezési és a token érvényességi biztonságosan használhatja. Ezek közé tartozik hello OWIN Open ID Connect köztes az ASP.NET és a hello Node.js Passport beépülő modult. Használja a Microsoft köztes mintakódok, tekintse meg a [bevezetés](active-directory-appmodel-v2-overview.md#getting-started) szakasz.
* Ha egy asztali vagy hordozható alkalmazást fejleszt, Microsoft hitelesítési könyvtárak (MSAL) szereplő előzetes verzióban egyikét használhatja.  Ezeket a kódtárakat egy éles által támogatott előzetes vannak, ezért biztonságos toouse őket éles alkalmazásokban. Bővebben a használati hello hello előzetes megtekintéséhez és az elérhető szalagtárak hello elolvashatja a [hitelesítési könyvtárak hivatkozás](active-directory-v2-libraries.md).
* Nem fedi le Microsoft szalagtárak platformokhoz így integrálhatja hello v2.0-végponttal közvetlenül üzenetek küldése és fogadása protokoll az alkalmazás kódjában. hello v2.0 OpenID Connectet és az OAuth protokollok [explicit módon szerepelnek](active-directory-v2-protocols.md) toohelp ilyen integrációs elvégezhető.
* Végezetül használható nyílt forráskódú megnyitása ID Connect és OAuth szalagtárak toointegrate hello v2.0-végponttól. hello v2.0 protokoll számos nyílt forráskódú protokoll szalagtárak anélkül, hogy lényegesen módosul kompatibilisnek kell lennie. ilyen típusú szalagtárak hello elérhetősége függ a nyelvet és a platform. Hello [Open ID Connect](http://openid.net/connect/) és [OAuth 2.0](http://oauth.net/2/) webhelyek népszerű megvalósítások listának a karbantartására. További információkért lásd: [Azure Active Directory v2.0 és hitelesítési kódtárai](active-directory-v2-libraries.md), és nyílt forráskódú klienskódtárak és tesztelt hello v2.0-végponttal minták hello listája.

## <a name="restrictions-on-protocols"></a>Protokollok korlátozásai
hello v2.0-végpontra nem támogatja az SAML-alapú és a WS-Federation; csak a támogatott Open ID Connect és az OAuth 2.0-s.  Nem minden funkciók és képességek OAuth protokollok bekerültek hello v2.0-végponttól. Ezen protokoll funkciók és képességek jelenleg *nem érhető el* a v2.0-végponttól hello:

* Hello v2.0-végponttól által kiállított azonosító-jogkivonatokat nem tartalmaznak egy `email` jogcím hello felhasználó, akkor is, ha szerez be hello felhasználói tooview engedélyt az e-maileket.
* hello OpenID Connect UserInfo végpont nincs megvalósítva a hello v2.0-végponttól. Azonban a felhasználói profil összes adata, amely potenciálisan kapna a végponti érhető el a Microsoft Graph hello `/me` végpont.
* hello v2.0-végponttól kibocsátó szerepkör vagy csoport jogcímek azonosító-jogkivonatokat támogatja.
* Hello [OAuth 2.0 erőforrás tulajdonosa jelszó hitelesítő adatok megadása](https://tools.ietf.org/html/rfc6749#section-4.3) hello v2.0-végponttól által nem támogatott.

A addtion hello v2.0-végpontra nem támogatja bármely formájára vonatkozó hello SAML vagy a WS-Federation protokollok.

toobetter megértéséhez hello hatókör protokoll funkció támogatja a hello v2.0-végpontra, olvassa végig a [OpenID Connectet és az OAuth 2.0 protokoll referenciája](active-directory-v2-protocols.md).

## <a name="restrictions-for-work-and-school-accounts"></a>A munkahelyi és iskolai fiókok korlátozások
Ha Windows-alkalmazások már használta az Active Directory Authentication Library (ADAL), előfordulhat, hogy elvégezte előnyeit, integrált Windows-hitelesítést, amely hello Security Assertion Markup Language (SAML) helyességi feltétel grant használja. Ez a támogatás a felhasználók összevont Azure AD bérlők csendes hitelesítheti a helyszíni Active Directory példánnyal hitelesítő adatok megadása nélkül. Hello SAML-alapú assertion grant hello v2.0 végponton nem támogatott.
