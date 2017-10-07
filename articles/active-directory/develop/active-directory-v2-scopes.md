---
title: "az Active Directory aaaAzure 2.0 hatókörök, engedélyek és hozzájárulási |} Microsoft Docs"
description: "Engedélyezés hello Azure AD v2.0-végpontra, beleértve a hatókörök, engedélyek és beleegyezése leírását."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 8f98cbf0-a71d-4e34-babf-e644ad9ff423
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 5721d368c435868bfb4ae91cff7fbb9bc4a79b66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scopes-permissions-and-consent-in-hello-azure-active-directory-v20-endpoint"></a>Hatókörök, engedélyek és hello Azure Active Directory v2.0-végponttól jóváhagyása
Alkalmazásokat, amelyekbe beépül az Azure Active Directory (Azure AD) hajtsa végre az olyan engedélyezési modell, amely lehetőséget ad a felhasználók szabályozhatják, hogyan az alkalmazások is hozzáférjenek az adataikhoz. hello v2.0 végrehajtásának hello engedélyezési modellt már frissítve van, és hogyan kapcsolatba kell lépnie egy alkalmazást az Azure ad-val változik. Ez a cikk ismerteti az engedélyezési modellt, beleértve a hatókörök, engedélyek és beleegyezése hello alapvető fogalmait.

> [!NOTE]
> hello v2.0-végpontra nem támogatja az összes Azure Active Directory forgatókönyvek és funkciók. toodetermine kell használnia a hello v2.0-végpontra, hogy olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).
>
>

## <a name="scopes-and-permissions"></a>Hatókörök és engedélyek
Az Azure AD megvalósítja hello [OAuth 2.0](active-directory-v2-protocols.md) hitelesítési protokoll. OAuth 2.0 egy metódust, amelyen keresztül a külső alkalmazások hozzáférhetnek a webkiszolgáló által szolgáltatott erőforrásokhoz egy felhasználó nevében. A webkiszolgáló által szolgáltatott erőforrás, amely az Azure AD tartalmaz egy erőforrás-azonosítót, vagy *Application ID URI*. Például a Microsoft web által szolgáltatott erőforrások közé:

* Office 365 egyesített Mail API hello:`https://outlook.office.com`
* hello Azure AD Graph API:`https://graph.windows.net`
* A Microsoft Graph:`https://graph.microsoft.com`

hello esetében, amely integrálva van az Azure AD külső erőforrásokat is igaz. Ezeket az erőforrásokat is definiálhat az engedélyeket, lehet használt toodivide hello funkciókat erőforrás kisebb csoportjai. Tegyük fel [Microsoft Graph](https://graph.microsoft.io) feladatokat, többek között a következő engedélyek toodo hello definiálva van:

* A felhasználó naptár olvasása
* Írási tooa felhasználói naptár
* E-mailek küldése más felhasználó nevében

Az ilyen típusú engedélyek megadásával hello erőforrás rendelkezik részletes szabályozhatják, az adatokat, és hogyan hello adatok ki van téve. A külső alkalmazások is azokat az engedélyeket kérhet egy alkalmazás felhasználói. hello app felhasználó működhet-e hello app hello felhasználó nevében előtt jóvá kell hagynia hello engedélyek. Kisebb engedélycsoportok hello erőforrás funkciókat adattömbösítő, harmadik felek alkalmazásaihoz lehetnek beépített toorequest csak hello konkrét engedélyeket, hogy be kell tooperform működésére. Alkalmazás felhasználói tudhatja, pontosan egy alkalmazás általi felhasználásának módja az adatokat, és azok lehet több abban, hogy hello app rosszindulatú viselkedik.

Az Azure AD és az OAuth, ezek az engedélyek nevezik *hatókörök*. Néha is hivatkozott tooas *oAuth2Permissions*. A hatókör karakterlánc-érték jelzi az Azure ad-ben. Hello Microsoft Graph példát folytatva, hello hatókör minden engedély értéke:

* Olvassa el a felhasználó naptár használatával`Calendar.Read`
* Írási tooa felhasználói naptár használatával`Mail.ReadWrite`
* Egy felhasználó használt által e-maileket küldjön`Mail.Send`

Egy alkalmazás ezeket az engedélyeket kérhet kérelmek toohello v2.0-végponttól hello hatókörök megadásával.

## <a name="openid-connect-scopes"></a>Hatókörök OpenID Connect
az OpenID Connect hello v2.0 végrehajtása rendelkezik néhány jól meghatározott hatókörök tooa megadott erőforrás nem alkalmazható: `openid`, `email`, `profile`, és `offline_access`.

### <a name="openid"></a>openid
Ha egy alkalmazás segítségével hajtja végre a bejelentkezési [OpenID Connect](active-directory-v2-protocols.md), azt kell igényelnie hello `openid` hatókör. Hello `openid` hatókörét jelzi a hello munkahelyi fiókot lap hozzájárulás engedély "Bejelentkezés" hello, és hello személyes Microsoft-fiókjában hozzájárulás lap, hello "Profilját és tooapps és a Microsoft-fiókjával szolgáltatások" engedéllyel. Ezzel az engedéllyel, egy alkalmazás hello felhasználó egyedi azonosítóját fogadhat hello hello formájában `sub` jogcímek. Emellett a hello app hozzáférés toohello UserInfo végpont segítségével. Hello `openid` hatókör hello v2.0 token-végpont tooacquire azonosító-jogkivonatokat, amelyek az alkalmazások összetevői között használt toosecure HTTP-hívások is lehet használni.

### <a name="email"></a>E-mailek
Hello `email` hatókör használható hello `openid` hatókörben és bármely más. Hello app hozzáférés toohello a felhasználó elsődleges e-mail címét hello hello formájában biztosít `email` jogcímek. Hello `email` jogcím szerepel egy token csak akkor, ha az e-mail címet helyzet nem mindig hello hello felhasználói fiókhoz társítva. Ha hello használ `email` hatókör, az alkalmazás kell toohandle eset melyik hello az előkészített `email` jogcím hello jogkivonat nem létezik.

### <a name="profile"></a>Profil
Hello `profile` hatókör használható hello `openid` hatókörben és bármely más. Hello app hozzáférés tooa jelentős mennyiségű hello felhasználói kapcsolatos információkat biztosít. hozzáférhet hello információkat tartalmaz, de nem korlátozódik, felhasználó által megadott névvel, a Vezetéknév, az elsődleges felhasználónév hello, és objektum-azonosítót. Hello profil jogcímek hello id_tokens paraméter elérhető egy adott felhasználó teljes listáját lásd: hello [v2.0 jogkivonatok hivatkozás](active-directory-v2-tokens.md).

### <a name="offlineaccess"></a>offline_access
Hello [ `offline_access` hatókör](http://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) hosszú ideig tartó ad az alkalmazás hozzáférési tooresources hello felhasználó nevében. Hello munkahelyi fiók hozzájárulási lapon a hatókör jelenik meg, hello "Az adatok elérése bármikor" engedéllyel. Hello személyes Microsoft fiók hozzájárulási lapon akkor jelenik meg, hello hozzáférés-adataihoz bármikor"engedéllyel. Amikor egy felhasználó jóvá hello `offline_access` hatókör, az alkalmazás fogadhat frissítési jogkivonatokat hello v2.0 jogkivonat végpontjához. Frissítési jogkivonatok hosszú élettartamú. Az alkalmazás kérheti le új jogkivonatot, ahogyan a régieket érvényessége lejár.

Ha az alkalmazás nem kér hello `offline_access` hatókör, hogy nem kapja a frissítési jogkivonatokat. Ez azt jelenti, hogy ha egy engedélyezési kódot hello beváltja [OAuth 2.0 hitelesítésikód-folyamata](active-directory-v2-protocols.md), csak olyan hozzáférési jogkivonatot kap a hello `/token` végpont. hello hozzáférési jogkivonat érvénytelen rövid időre. hello hozzáférési jogkivonat általában egy óra múlva lejár. Ezen a ponton a kell tooredirect hello felhasználói hátsó toohello `/authorize` végpont tooget egy új hitelesítési kódot. Az átirányítás, attól függően, hogy hello típusú alkalmazás során hello felhasználói előfordulhat, hogy kell tooenter hitelesítő adataikkal újra vagy hozzájárulás újra toopermissions.

Hogyan tooget és -felhasználási frissítési jogkivonatok kapcsolatos további információkért lásd: hello [v2.0 protokoll referenciái](active-directory-v2-protocols.md).

## <a name="requesting-individual-user-consent"></a>Egyéni felhasználói hozzájárulás kérése
Az egy [OpenID Connect vagy OAuth 2.0](active-directory-v2-protocols.md) engedélyt kér, egy alkalmazás kérheti hello segítségével kell hello engedélyek `scope` lekérdezési paraméter. Például egy felhasználó bejelentkezik tooan app, hello app kérést küld például hello példa (az olvashatóságot hozzá sortörést) a következő:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=
https%3A%2F%2Fgraph.microsoft.com%2Fcalendar.read%20
https%3A%2F%2Fgraph.microsoft.com%2Fmail.send
&state=12345
```

Hello `scope` paraméter kér az alkalmazás hello hatókörök szóközökkel elválasztott listáját. Minden hatókör hello hatókör érték toohello erőforrás-azonosító (hello Application ID URI) hozzáfűzésével jelzi. Hello kérelem példában hello kell alkalmazást engedély tooread hello felhasználói naptár és; e-mail hello felhasználóként.

Hello felhasználó megadja a hitelesítő adatokat, miután hello v2.0-végponttól ellenőrzi egy egyező rekordot *felhasználói hozzájárulás*. Ha hello felhasználó nem hozzájárult tooany hello a kért engedélyek hello korábbi, a v2.0-végponttól hello kéri hello felhasználói toogrant hello kért engedélyeket.

![Munkahelyi fiók hozzájárulás](../../media/active-directory-v2-flows/work_account_consent.png)

Hello felhasználói jóvá hello engedéllyel, amikor hello hozzájárulási, hogy hello a felhasználó nem rendelkezik tooconsent újra a későbbi bejelentkezések rögzíti.

## <a name="requesting-consent-for-an-entire-tenant"></a>Egy teljes bérlő hozzájárulás kérése
Gyakran Ha egy szervezet vásárol egy licenc- vagy egy alkalmazás-előfizetés, a hello szervezet által toofully rendelkezés hello alkalmazás az alkalmazottait. Ez a folyamat részeként a rendszergazda biztosíthat bármely alkalmazott nevében hello alkalmazás tooact kapcsolatos jóváhagyásról. Üdvözöljük a rendszergazdákat a hozzájárulási hello teljes bérlő számára biztosít, ha az hello szervezet alkalmazottai hello alkalmazás hozzájárulási lap nem jelenik meg.

a bérlőt, az alkalmazás összes felhasználója számára toorequest hozzájárulási hello rendszergazda jóváhagyását végpont használhatja.

## <a name="admin-restricted-scopes"></a>Rendszergazda által korlátozott hatókör
Néhány magas szintű jogosultságokkal a hello Microsoft ökoszisztéma túl beállítható*admin korlátozott*. Az ilyen típusú hatókörök például hello alábbi engedélyek használata:

* Segítségével a szervezetek címtáradatok olvasása`Directory.Read`
* Írási adatok tooan szervezet címtárához használatával`Directory.ReadWrite`
* Olvassa el a munkahely biztonsági csoportok segítségével`Groups.Read.All`

Bár a fogyasztói felhasználó milyen adatokat adhat egy alkalmazás-hozzáférés toothis, szervezeti felhasználók nem állhatnak ugyanaz a bizalmas vállalati adatok készletét hozzáférés toohello megadása. Ha az alkalmazás kér egy szervezeti felhasználói hozzáférés tooone ezek az engedélyek, hello üzenet egy hiba, amely szerint a nem hitelesített tooconsent tooyour app-engedélyek.

Ha az alkalmazás hozzáférési tooadmin korlátozott hatókörök a szervezet számára szükséges, igényeljen őket a vállalati rendszergazda közvetlenül a is hello rendszergazda jóváhagyását végpont, leírtak alapján használatával.

Ha a rendszergazda engedélyezi ezeket az engedélyeket keresztül Üdvözöljük a rendszergazdákat hozzájárulás végpont, hozzájárulási kapnak a hello bérlő összes felhasználója számára.

## <a name="using-hello-admin-consent-endpoint"></a>Hello rendszergazda jóváhagyását végpont használatával
Kövesse az alábbi lépéseket, ha az alkalmazás gyűjthet bérlőjében, beleértve a rendszergazda által korlátozott hatókörök minden felhasználó engedélyeit. toosee egy kódminta, amely megvalósítja az hello lépéseiért lásd: hello [admin korlátozott hatókörök minta](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2).

### <a name="request-hello-permissions-in-hello-app-registration-portal"></a>Hello app regisztrációs portálon hello engedélyek kéréséhez
1. Nyissa meg a hello tooyour alkalmazás [alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy [hozzon létre egy alkalmazást](active-directory-v2-app-registration.md) Ha még nem tette meg.
2. Keresse meg a hello **Microsoft Graph engedélyek** szakaszt, és adja hozzá a hello az engedélyeket, az alkalmazás használatához.
3. Győződjön meg arról, hogy **mentése** hello alkalmazás regisztrációja.

### <a name="recommended-sign-hello-user-in-tooyour-app"></a>Ajánlott: Bejelentkezési hello felhasználói tooyour alkalmazásban
Általában az alkalmazás építésekor hello rendszergazda jóváhagyását végpont használó, hello alkalmazásnak kell lapon vagy a mely hello admin jóváhagyhatja hello app engedélyek nézetben. Ezen a lapon lehet hello alkalmazás-előfizetési folyamat része, hello alkalmazás beállításai, vagy egy dedikált "Csatlakozás" folyamat lehet. Sok esetben érdemes a hello app tooshow a "Csatlakozás" nézetben, csak azt követően a felhasználó a munkahelyi vagy iskolai Microsoft-fiókkal van bejelentkezve.

Tooyour alkalmazásban hello felhasználói bejelentkezéskor hello szervezet kiválaszthat toowhich Üdvözöljük a rendszergazdákat tartozik előtt tooapprove hello szükséges engedélyek kéri a felhasználót. Bár nem feltétlenül szükséges, ez segíthet a szervezeti felhasználók intuitívabb környezetet. toosign hello felhasználó, hajtsa végre a [v2.0 protokoll oktatóanyagok](active-directory-v2-protocols.md).

### <a name="request-hello-permissions-from-a-directory-admin"></a>Hello engedélyeket kérhet a directory-rendszergazda
Amikor készen áll a toorequest engedélyek az Ön szervezetének rendszergazdájával, átirányítja a hello felhasználói toohello v2.0 *rendszergazda jóváhagyását végpont*.

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro tip: Try pasting hello below request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Paraméter | Az állapot | Leírás |
| --- | --- | --- |
| Bérlői |Szükséges |hello directory-bérlőt, amelyet toorequest engedélyt. Megadható a GUID vagy rövid név formátumban. |
| client_id |Szükséges |hello alkalmazás azonosító adott hello [alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) tooyour app rendelve. |
| redirect_uri |Szükséges |hello átirányítási URI, ahol azt szeretné, hogy az alkalmazás toohandle küldött hello válasz toobe. Ez pontosan egyeznie kell hello átirányítási URI-azonosítók hello app regisztrációs portálon regisztrált. |
| state |Ajánlott |Hello token válaszul is visszaadott hello kérelemben szereplő érték. Bármilyen tartalmat karakterlánc lehet. Hello hitelesítési kérelem történt, például hello lap vagy nézet, amilyenek korábban voltak a hello alkalmazásban hello felhasználói állapotával kapcsolatos információkat használatára hello tooencode állapotadatait. |

Ezen a ponton az Azure AD egy Bérlői rendszergazda toosign toocomplete hello kérés van szükség. hello rendszergazdai engedélyek, az alkalmazás hello app regisztrációs portálon kért hello összes tooapprove kéri.

#### <a name="successful-response"></a>A sikeres válasz
Üdvözöljük a rendszergazdákat jóvá hello engedélyek beállítása az alkalmazáshoz, ha a sikeres válasz hello néz ki:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Paraméter | Leírás |
| --- | --- | --- |
| Bérlői |hello directory-bérlő tartozik, amely az alkalmazás hello engedélyeket azt kérte, GUID formátumban. |
| state |Hello kérelem is visszaadott hello biztonságijogkivonat-válaszban szereplő érték. Bármilyen tartalmat karakterlánc lehet. hello állapota hello felhasználói állapot hello alkalmazásban használt tooencode információ hello hitelesítési kérelem történt, például hello lap vagy amilyenek korábban voltak a megtekintése előtt. |
| admin_consent |Lesz beállítva, túl**igaz**. |

#### <a name="error-response"></a>Hibaválaszba
Üdvözöljük a rendszergazdákat nem hagyja jóvá az alkalmazás hello engedélyeit, ha a hello válasz néz sikertelen volt:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Paraméter | Leírás |
| --- | --- | --- |
| error |Egy hiba kód karakterlánc, amely lehet használt tooclassify típusú előforduló hibákat, és a használt tooreact tooerrors lehet. |
| error_description |Egy adott hibaüzenet, amelyek segítségével a fejlesztők hello hiba okának azonosításához. |

Miután hello rendszergazda jóváhagyását végpont már a sikeres válasz érkezett, az alkalmazás köszönhetően a kért hello engedélyeket. A következő kívánt hello erőforrás jogkivonatot kérhet.

## <a name="using-permissions"></a>Engedélyek használata
Miután hello felhasználó hozzájárul az alkalmazás toopermissions, az alkalmazás szerezhetnek be a hozzáférési jogkivonatok, amelyek megfelelnek az alkalmazás engedélyt tooaccess néhány kapacitás az erőforráshoz. Olyan hozzáférési jogkivonatot csak egyetlen használható, de kódolású belül hello hozzáférési jogkivonatot minden engedélyt, hogy az alkalmazás rendelkezik az adott erőforráshoz. tooacquire hozzáférési tokent, az alkalmazás a kérelem toohello v2.0 jogkivonat végpontjához ilyen létesíthetik:

```
POST common/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/json

{
    "grant_type": "authorization_code",
    "client_id": "6731de76-14a6-49ae-97bc-6eba6914391e",
    "scope": "https://outlook.office.com/mail.read https://outlook.office.com/mail.send",
    "code": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq..."
    "redirect_uri": "https://localhost/myapp",
    "client_secret": "zc53fwe80980293klaj9823"  // NOTE: Only required for web apps
}
```

A HTTP-kérelmek toohello erőforrás hello eredményül kapott hozzáférési jogkivonat is használhatja. Azt jelzi, hogy az alkalmazás rendelkezik megfelelő engedéllyel tooperform hello egy adott feladat toohello erőforrás megbízhatóan.  

További információ a hello OAuth 2.0 protokoll, és hogyan tooget hozzáférési jogkivonatok, lásd: hello [v2.0 protokoll végponthivatkozás](active-directory-v2-protocols.md).
