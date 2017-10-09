---
title: "aaaWhat nem azonos a hello Azure AD v2.0-végponttól? | Microsoft Docs"
description: "Hello eredeti az Azure AD és a v2.0-végpontok hello egy összehasonlítására."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 5060da46-b091-4e25-9fa8-af4ae4359b6c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: e7ed196f9053fc21db799cd6bc513ba5c2b92885
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="whats-different-about-hello-v20-endpoint"></a>Mi az a különböző hello v2.0-végponttal kapcsolatban?
Ha ismeri az Azure Active Directoryban, vagy integrálva van alkalmazások az Azure AD-t hello múltbeli, valószínűleg eltérések hello v2.0-végpontra nem teheti meg.  Ez a dokumentum ezen eltérésekhez tartozó megértését hív meg.

> [!NOTE]
> Nem minden Azure Active Directory forgatókönyvek és funkciók támogatják hello v2.0-végponttól.  toodetermine használatát hello v2.0-végpontra, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).
>

## <a name="microsoft-accounts-and-azure-ad-accounts"></a>Microsoft-fiókok és az Azure AD-fiókok
hello v2.0-végpontra a fejlesztők toowrite alkalmazásokat, amelyek fogadják-bejelentkezés a Microsoft Accounts és az Azure AD fiókokhoz, egy egyetlen hitelesítési végpont használatával.  Ez biztosítja az hello képességét toowrite az alkalmazás teljesen fiók-független; vizsgálatot fiókkal, amely a felhasználó jelentkezik be az hello hello típusú lehet.  Természetesen az, hogy *is* , hogy az alkalmazás használatát egy adott munkamenet használt fiók hello típusú, azonban nincs.

Például ha az alkalmazás hello [Microsoft Graph](https://graph.microsoft.io), néhány további funkciók és az adatok nem elérhető tooenterprise felhasználók, például a SharePoint-webhelyek vagy a címtáradatok.  Azonban több művelethez például [a felhasználó e-mail olvasása](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message), pontosan hello kód csak írható hello azonos Microsoft Accounts és az Azure AD-fiókok.  

Az alkalmazás integrálása Microsoft Accounts és az Azure AD-fiókok mostantól egy egyszerű folyamat.  Végpontok, egyetlen könyvtár és egy egyetlen meghatározott alkalmazás regisztrációs toogain hozzáférés tooboth hello fogyasztói, valamint vállalati világot egyetlen halmazába is használhatja.  További részletek hello v2.0-végpontra, tekintse meg toolearn [áttekintése hello](active-directory-appmodel-v2-overview.md).

## <a name="new-app-registration-portal"></a>Új alkalmazás-regisztrációs portál
egy alkalmazás, amely kompatibilis a v2.0-végponttól hello tooregister, kell használnia egy új alkalmazás-regisztrálási portál: [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Ez a hello portal szerezhet az Alkalmazásazonosító, az alkalmazás bejelentkezési oldalak megjelenésének hello testreszabása.  Tooaccess hello portál szüksége az alkalmazás bekapcsolja Microsoft-fiók - személyes vagy a munkahelyi vagy iskolai fiókjával.

## <a name="one-app-id-for-all-platforms"></a>Az összes platformra egy alkalmazás azonosítója
Ha már használta az Azure Active Directoryban, valószínűleg már regisztrálta egy projekthez több különböző alkalmazást.  Például, ha a webhely és a iOS-alkalmazást, kellett tooregister őket külön-külön, a két különböző alkalmazás-azonosítók használata. hello Azure AD alkalmazás-regisztrálási portál ezt a különbséget kényszerített meg toomake regisztrálás során:

![Régi regisztrációja felhasználói felület](../../media/active-directory-v2-flows/old_app_registration.PNG)

Hasonlóképpen ha egy webhely és a háttérkiszolgáló webes API-t, előfordulhat, hogy regisztrálta az egyes külön alkalmazásként az Azure AD.  Vagy ha egy iOS-alkalmazás- és Android-alkalmazás, is előfordulhat, hogy regisztrálta két különböző alkalmazások.  Az alkalmazás minden összetevő regisztrálása vezetett toosome fejlesztők és az ügyfelek nem várt működése:

* Minden egyes összetevő hello Azure Active Directory-bérlő minden ügyfél az külön alkalmazásként jelent meg.
* A bérlői rendszergazda megkísérelt tooapply házirend való hozzáférés kezelése, vagy törölje az alkalmazást, toodo kellene az egyes összetevők hello alkalmazás igen.
* Amikor az ügyfelek átadni kívánt hozzájárult, e tooan alkalmazás, minden egyes összetevő jelent hello hozzájárulási képernyőn egy különálló alkalmazás.

Hello v2.0-végponttal mostantól egy egyetlen meghatározott alkalmazás regisztrálása a projekt összetevők regisztrálásához, és a teljes projektet egy egyetlen alkalmazás-azonosító használata.  Adja hozzá a több "platformok" tooa minden olyan projekthez, és adja meg a megfelelő adatok hello ad hozzá minden egyes platformhoz.  Természetesen a követelményeitől függően, de az esetek többségében hello csak egy alkalmazásazonosítót kell szükséges annyi alkalmazásokat is létrehozhat.

Az célja, hogy ezzel tooa további egyszerűsített felügyeleti és fejlesztői környezetet vezethet, és hozzon létre egy projekt, amely akkor működik a több egyesített nézetét.

## <a name="scopes-not-resources"></a>Hatókörök, nem erőforrások
Az Azure Active Directoryban, az alkalmazások viselkedhet, mint egy **erőforrás**, vagy a jogkivonatok címzett.  Egy erőforrás definiálhat számos **hatókörök** vagy **oAuth2Permissions** , hogy megértette, így ügyfélalkalmazások toorequest jogkivonatok toothat erőforrás hatókörök bizonyos számú.  Vegye figyelembe a hello Azure AD Graph API példa bemutatja, erőforrás:

* Erőforrás-azonosítót, vagy `AppID URI`:`https://graph.windows.net/`
* Hatókörök, vagy `OAuth2Permissions`: `Directory.Read`, `Directory.Write`stb.  

Mindez igaz hello hello v2.0-végponttól.  Az alkalmazások továbbra is viselkedhet erőforrásként, adja meg a hatókörök és azonosítható, ha egy URI-t.  Ügyfél alkalmazások továbbra is kérhet hozzáférés toothose hatókörök.  Azonban, amelyben egy ügyfél lekérdezi ezeket az engedélyeket hello módszer megváltozott.  Az elmúlt hello az OAuth 2.0 engedélyezik a kérelem tooAzure AD előfordulhat, hogy rendelkezik keresést végrehajtani, például:

```
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https%3A%2F%2Fgraph.windows.net%2F
...
```

Ha hello **erőforrás** paraméter jelzi, hogy melyik erőforrás hello ügyfélalkalmazás engedélyt kér.  Az Azure AD számított hello alkalmazásához hello Azure-portál a statikus konfigurációján alapul, és ennek megfelelően kiadott jogkivonatokat szükséges hello engedélyek.  Most hello azonos OAuth 2.0 engedélyezik a kérelem néz ki:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

Ha hello **hatókör** paraméter azt jelzi, hogy melyik erőforrás és engedélyek hello alkalmazás által kért engedélyezését. a keresett erőforrás megtalálható továbbra is nagyon hello kérelem – egyszerűen lefedett minden hello hatókör-paraméter értékének hello hello.  Az ilyen módon hello hatókör-paraméter használatával lehetővé teszi több specifikációnak hello OAuth 2.0 hello v2.0 végpont toobe, és igazodik jobban közös iparági eljárásokat.  Emellett lehetővé teszi az alkalmazások tooperform [növekményes hozzájárulási](#incremental-and-dynamic-consent), amely hello a következő szakaszban ismertetett.

## <a name="incremental-and-dynamic-consent"></a>Növekményes és dinamikus hozzájárulás
Alkalmazások regisztrálni a korábban szükséges az Azure AD toospecify szükséges OAuth 2.0 engedélyeik hello Azure portál, az alkalmazás létrehozáskor:

![Engedélyek regisztrációs felhasználói felület](../../media/active-directory-v2-flows/app_reg_permissions.PNG)

szükséges alkalmazás be lett állítva hello engedélyek **statikusan**.  Ez hello app tooexist konfigurációja engedélyezett hello Azure portálon, és töltött és egyszerű hello kód tartani, azt is problémás lehet néhány fejlesztőknek:

* Egy alkalmazás tooknow kellett az összes alkalmazás létrehozáskor átállítására lenne kellene hello engedélyeket.  Engedélyek hozzáadása adott idő alatt volt nehéz folyamat.
* Egy alkalmazás kellett tooknow azt minden eddiginél elérésére időben hello erőforrásokat.  Nehéz toocreate alkalmazások, amely hozzáférhet erőforrások tetszőleges számú volt.
* Egy alkalmazás kellett toorequest legalább egyszer kellene hello felhasználói első bejelentkezés után minden hello engedélyt.  Egyes esetekben ez vezetett tooa hello alkalmazás hozzáférésének kezdeti bejelentkezés jóváhagyása a végfelhasználók számára javasolt engedélyek nagyon hosszú listáját.

Hello v2.0-végponttal, hello engedélyeket adhat meg az alkalmazás igényeinek **dinamikusan**, futásidőben, az alkalmazás rendszeres használat során.  toodo, így megadhatja, hello idő álljon az alkalmazás igényeinek hatóköröket belefoglalja ezeket hello `scope` engedélyezési kérések paraméter:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

a fenti hello hello app tooread a engedélyt kér, az Azure AD-felhasználó címtáradatok, valamint írási tootheir adatkönyvtárában.  Ha hello felhasználó hozzájárult múltbeli hello toothose engedélyeket az adott alkalmazás módba lép, egyszerűen csak a hitelesítő adataikat, és hello alkalmazást be kell jelentkeznie.  Hello felhasználó nem hozzájárult tooany ezek az engedélyek, ha hello v2.0-végponttól hello felhasználói hozzájárulás toothose engedélyek kéri.  További toolearn olvashatók a [engedélyek, beleegyezése és hatókörök](active-directory-v2-scopes.md).

Így az alkalmazások toorequest engedélyeket dinamikusan keresztül hello `scope` paraméter teljes ellenőrzést a felhasználói élmény ad meg.  Ha kívánja, dönthet úgy, Ön hozzájárul észlelnek, és egy kezdeti hitelesítési kérelem minden engedélyt kér toofrontload.  Vagy ha az alkalmazás számos engedélyeket igényel, dönthet úgy toogather ezeket az engedélyeket a hello felhasználó Növekményesen, megpróbálják toouse bizonyos funkciók, az alkalmazás adott idő alatt.

## <a name="well-known-scopes"></a>Az ismert hatókörök
#### <a name="offline-access"></a>Kapcsolat nélküli hozzáférés
Hello v2.0-végponttól használó alkalmazásokat lehet szükség hello egy új jól ismert engedély - alkalmazások hello `offline_access` hatókör.  Minden alkalmazás kell toorequest ezt az engedélyt, ha van szükségük a felhasználó nevében hello tooaccess erőforrások hosszabb ideig még akkor is, hello felhasználói előfordulhat, hogy nem kell aktívan hello alkalmazás használatakor.  Hello `offline_access` hatókör megjelenik-e a "Fér hozzá az adatokhoz offline", mint toohello felhasználói hozzájárulás a párbeszédpanelek hello felhasználói kell fogadnia.  Kérelmező hello `offline_access` engedély lehetővé teszi a webes alkalmazás tooreceive OAuth 2.0 refresh_tokens a hello v2.0-végponttól.  Refresh_tokens hosszú élettartamú, és új OAuth 2.0 access_tokens access hosszú időn keresztül kicserélhetők.  

Ha az alkalmazás nem kér hello `offline_access` hatókör, nem kapják meg refresh_tokens.  Ez azt jelenti, hogy amikor egy authorization_code hello OAuth 2.0 hitelesítésikód-folyamata a beváltásához ezután csak érkezik vissza egy access_token hello `/token` végpont.  Adott access_token rövid időn (általában egy órán keresztül) érvényes marad, de végül lejár.  Abban az esetben az alkalmazás tooredirect hello felhasználói hátsó toohello kell `/authorize` végpont tooretrieve egy új authorization_code.  Az átirányítás során hello felhasználói vagy előfordulhat, hogy nem kell tooenter hitelesítő adataikkal újra és újra hozzájárulás toopermissions, attól függően, hogy hello hello típusú alkalmazást.

További információk az OAuth 2.0-s, refresh_tokens és access_tokens, kivételének hello toolearn [v2.0 protokoll referenciái](active-directory-v2-protocols.md).

#### <a name="openid-profile-and-email"></a>OpenID, és az e-mail profil
Hagyományosan hello legalapvetőbb OpenID Connect bejelentkezési folyamata az Azure Active Directoryval volna adja meg az eredményül kapott id_token hello hello felhasználó adatai rengeteg hasznos.  hello jogcím szerepel egy id_token hello felhasználó nevét, elsődleges felhasználónév, e-mail címét, objektumazonosító: és több tartalmazhatnak.

Folyamatban van, most hello információk korlátozása, hogy hello `openid` hatókör számára biztosítja az alkalmazás eléréséhez.  hello "openid" hatókör csak felhasználónak lehetővé teszi az alkalmazás toosign hello, és fogadni hello felhasználó alkalmazás-specifikus azonosítója.  Tooobtain személyes azonosításra alkalmas adatokat (PII) hello felhasználóról az alkalmazásban, az alkalmazás fogja kell hello felhasználói toorequest további engedélyek.  Vezetjük be két új hatókört – hello `email` és `profile` hatókörök – amelyek toodo így.

Hello `email` hatóköre nagyon egyszerű – lehetővé teszi az alkalmazás hozzáférési toohello felhasználó elsődleges e-mail címét keresztül hello `email` hello id_token a jogcímek.  Hello `profile` objektumazonosító hatókör számára biztosítja az alkalmazás hozzáférési tooall más hello felhasználó – a neve, az elsődleges felhasználónév alapvető információkat, és így tovább.

Ez lehetővé teszi, hogy toocode minimális nyilvánosságra módon – az alkalmazás csak megkérheti hello felhasználói hello csoportját, hogy az alkalmazás által kért információ toodo feladata elvégzéséhez.  Ezek a hatókörök további információkért tekintse meg túl[v2.0 hatókör-hivatkozást hello](active-directory-v2-scopes.md).

## <a name="token-claims"></a>A jogcímek jogkivonat
hello jogcímeket a v2.0-végponttól hello által kiállított jogkivonatokat nem lesz azonos tootokens hello általánosan elérhető az Azure AD-végpontok által kibocsátott – alkalmazások toohello új szolgáltatás áttelepítése nem feltételezi egy adott jogcím van jelen id_tokens vagy access_tokens. v2.0 jogkivonatokba kibocsátott hello adott jogcímekről toolearn lásd: hello [v2.0 jogkivonat referenciái](active-directory-v2-tokens.md).

## <a name="limitations"></a>Korlátozások
Nincsenek ügyelnie, ha a hello v2.0 pont néhány korlátozások toobe.  Tekintse meg a toohello [v2.0 korlátozások doc](active-directory-v2-limitations.md) toosee, ha ezek a korlátozások bármelyike tooyour adott forgatókönyv.
