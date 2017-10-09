---
title: "Az Azure Active Directory B2C: A kért hozzáférési jogkivonatok |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan toosetup ügyfélalkalmazás és egy hozzáférési jogkivonat."
services: active-directory-b2c
documentationcenter: android
author: parakhj
manager: krassk
editor: 
ms.assetid: 1c75f17f-5ec5-493a-b906-f543b3b1ea66
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: parakhj
ms.openlocfilehash: 410639424fd4e5119a5d58751dfdb6e8957fd100
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-requesting-access-tokens"></a>Az Azure AD B2C: A kért hozzáférési jogkivonatok

Olyan hozzáférési jogkivonatot (jelölése **hozzáférés\_token** az Azure AD B2C hello válaszainak) a biztonsági jogkivonat tooaccess erőforrások az ügyfél használhatja által védett egy formája, amely egy [engedélyezési server](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-protocols#the-basics), például egy webes API-t. Hozzáférési jogkivonatok helyettesítik [JWTs](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-tokens#types-of-tokens) hello kívánt erőforrás-kiszolgáló és a hello engedélyeket toohello server vonatkozó adatokat tartalmaznak. Erőforrás-kiszolgáló hello meghívásakor hello hozzáférési jogkivonat szerepelnie hello HTTP-kérelem.

Ez a cikk ismerteti, hogyan rendelés az ügyfélalkalmazás tooconfigure és webes API-t a tooobtain egy **hozzáférés\_token**.

> [!NOTE]
> **Webes API-láncok (a-nevében-a) nem támogatja az Azure AD B2C.**
>
> Számos architektúra tartalmaz egy webes API-t, amelyet a toocall egy másik alsóbb rétegbeli webes API-t, mind az Azure AD B2C által védett. Ez a forgatókönyv esetében gyakori, a hátsó célból egy webes API-t, amely meghívja a egy Microsoft Online Services szolgáltatással, például az Azure AD Graph API hello rendelkező natív ügyfelek.
>
> Ez a láncolatba fűzött webes API-forgatókönyv használatával hello OAuth 2.0 JWT tulajdonosi hitelesítő adatok megadása, más néven hello a-meghatalmazásos folyamat támogatja. Azonban a-meghatalmazásos folyamat hello jelenleg nincs megvalósítva az Azure AD B2C.

## <a name="register-a-web-api-and-publish-permissions"></a>Webes API-k regisztrálja, és tegye közzé az engedélyek

Mielőtt kérelmezi a hozzáférési tokent, akkor először kell tooregister webes API-k és közzététele engedélyeket (hatókör), toohello ügyfélalkalmazás számára engedélyezhetők.

### <a name="register-a-web-api"></a>Webes API regisztrálása

1. A hello B2C funkciók panelére hello Azure-portálon, kattintson **alkalmazások**.
1. Kattintson a **+ Hozzáadás** hello panel hello tetején.
1. Adjon meg egy **neve** hello alkalmazáshoz, amely az alkalmazás tooconsumers ismerteti. Például adja meg "Contoso API".
1. Váltás hello **közé tartozik a web app / web API** túl kapcsoló**Igen**.
1. Adjon meg egy tetszőleges értéket hello **válasz URL-címek**. Adja meg például a következőt: `https://localhost:44316/`. hello érték nem számít, mivel az API-k kell nem kapnia hello jogkivonatot az Azure AD B2C-ről.
1. Adjon meg egy **Alkalmazásazonosító URI-t**. Ez a hello azonosítója a webes API-t használja. Hello mezőbe írja be például, "Megjegyzések". Hello **App ID URI** lesz `https://{tenantName}.onmicrosoft.com/notes`.
1. Kattintson a **létrehozása** tooregister az alkalmazást.
1. Kattintson az imént létrehozott hello alkalmazás, és másolja le a globálisan egyedi hello **alkalmazás ügyfél-azonosító** , amelyet később a kódjában fog használni.

### <a name="publishing-permissions"></a>Közzétételi engedélyek

Hatókörök, amelyek hasonló toopermissions, szükség, ha az alkalmazás hívja az API-t. Néhány példa a hatókörök "olvasása" vagy "write". Tegyük fel, hogy a webkiszolgáló vagy túl "olvasása" API a natív alkalmazással. Az alkalmazás az Azure AD B2C meghívta, és egy hozzáférési jogkivonatot, amely kérelem ad hozzáférési toohello hatókör "read". Ahhoz, hogy az Azure AD B2C tooemit olyan hozzáférési jogkivonatot hello alkalmazásnak kell toobe kap engedélyt túl "olvasása" hello adott API-n. toodo, először meg kell toopublish hello "olvasása" hatókör az API-t.

1. Az Azure AD B2C hello belül **alkalmazások** panelen megnyitott hello webes API-alkalmazás ("Contoso API").
1. Kattintson a **Published scopes** (Közzétett hatókörök) elemre. Ez az Itt adhatja meg hello engedélyeket (hatókör) tooother alkalmazások számára engedélyezhetők.
1. Adja hozzá **hatókörök értékeinek** szükség szerint "(például" olvasása). Alapértelmezés szerint hello "user_impersonation" hatókör határozza meg. Ha ez kihagyhatja. Írja be a leírást hello hatókör hello **hatókör neve** oszlop.
1. Kattintson a **Save** (Mentés) gombra.

> [!IMPORTANT]
> Hello **hatókör neve** hello hello leírása **hatókör értéke**. Hello hatókör használata esetén győződjön meg arról, hogy toouse hello **hatókör értéke**.

## <a name="grant-a-native-or-web-app-permissions-tooa-web-api"></a>Adja meg egy natív vagy webes alkalmazás engedélyek tooa webes API-t

Ha az API-k konfigurált toopublish hatókörök, hello ügyfél alkalmazást kell toobe kap e hatókörök hello Azure-portálon keresztül.

1. Keresse meg a toohello **alkalmazások** hello B2C funkciók panelje menüjében.
1. Egy ügyfélalkalmazás regisztrálni ([webalkalmazás](active-directory-b2c-app-registration.md#register-a-web-app) vagy [natív ügyfél](active-directory-b2c-app-registration.md#register-a-mobile-or-native-app)) Ha már nincs ilyen. Ha ez az útmutató a kiindulási pontként, az ügyfélalkalmazás tooregister lesz szüksége.
1. Kattintson a **API-hozzáférés**.
1. Kattintson a **Hozzáadás** gombra.
1. Válassza ki a webes API és hello hatókört szeretné toogrant (engedélyek).
1. Kattintson az **OK** gombra.

> [!NOTE]
> Az Azure AD B2C nem kéri az ügyfél az alkalmazás felhasználóinak a hozzájárulásukat. Üdvözöljük a rendszergazdákat, a fent leírt hello alkalmazások közötti konfigurált hello engedélyeinek ehelyett minden beleegyezést biztosítja. Ha az alkalmazás engedélyt támogatás visszavonja, minden olyan felhasználók, akik korábban már képes tooacquire az engedélyt nem fognak tudni toodo úgy kell.

## <a name="requesting-a-token"></a>Jogkivonat kérésével

Olyan hozzáférési jogkivonatot kérésekor hello ügyfélalkalmazás kell toospecify hello szükséges engedélyek a hello **hatókör** hello kérelem paraméter. Például toospecify hello **hatókör értéke** "olvasása" hello, amelynek hello API a **App ID URI** a `https://contoso.onmicrosoft.com/notes`, hello hatókör lenne `https://contoso.onmicrosoft.com/notes/read`. Az alábbiakban látható egy példa olyan engedélyezési kód kérelem toohello `/authorize` végpont.

> [!NOTE]
> Egyéni tartományok jelenleg nem támogatottak hozzáférési jogkivonatok együtt. Hello kérelem URL-CÍMÉT a tenantName.onmicrosoft.com tartományt kell használnia.

```
https://login.microsoftonline.com/<tenantName>.onmicrosoft.com/oauth2/v2.0/authorize?p=<yourPolicyId>&client_id=<appID_of_your_client_application>&nonce=anyRandomValue&redirect_uri=<redirect_uri_of_your_client_application>&scope=https%3A%2F%2Fcontoso.onmicrosoft.com%2Fnotes%2Fread&response_type=code 
```

tooacquire hello több engedélyek azonos kérelem, több bejegyzést adhat hozzá egyetlen hello **hatókör** paraméter választják el egymástól. Példa:

Dekódolt URL-címe:

```
scope=https://contoso.onmicrosoft.com/notes/read openid offline_access
```

Kódolt URL-címe:

```
scope=https%3A%2F%2Fcontoso.onmicrosoft.com%2Fnotes%2Fread%20openid%20offline_access
```

További hatókörök (engedélyek) mint mi az ügyfélalkalmazás számára adott erőforrás kérheti. Ha ez helyzet hello, hello hívás sikeres lesz, ha legalább egy engedélyt. hello eredő **hozzáférés\_token** lesz az "scp" igényét csak hello kiosztott engedélyeket sikeresen feltöltve.

> [!NOTE] 
> Nem támogatjuk kérelmező engedélyek két különböző webes erőforrásokon hello a kérésben. Az ilyen kérelem sikertelen lesz.

### <a name="special-cases"></a>Bizonyos esetekben

hello OpenID Connect szabványos több különleges "hatókör" értéket adja meg. hello túl a következő különleges hatókörök jelentik hello engedély "hello profil hozzáférési":

* **openid**: Ez az azonosító tokent kérelmek
* **kapcsolat nélküli\_hozzáférés**: ezt kéri, hogy a frissítési jogkivonat (használatával [viszonylatában Auth kód](active-directory-b2c-reference-oauth-code.md)).

Ha hello `response_type` paraméterének egy `/authorize` kérelemben `token`, hello `scope` paraméternek tartalmaznia kell legalább egy erőforrás hatókör (eltérő `openid` és `offline_access`) fog kapni. Ellenkező esetben hello `/authorize` kérelem hibával leáll.

## <a name="hello-returned-token"></a>hello adott vissza

A sikeresen minted **hozzáférés\_token** (vagy hello a `/authorize` vagy `/token` végpont), hello következő jogcímek kerül be:

| Név | Jogcím | Leírás |
| --- | --- | --- |
|Célközönség |`aud` |Hello **Alkalmazásazonosító** hello egyetlen erőforrás adott hello token hozzáférést biztosít. |
|Hatókör |`scp` |hello engedélyek toohello erőforrás. Több engedélyeket terület lesz elválasztva. |
|Jogosult felek |`azp` |Hello **Alkalmazásazonosító** hello kérelem kezdeményezett hello ügyfélalkalmazás. |

Ha az API-t kap hello **hozzáférés\_token**, azt kell [hello jogkivonat érvényesítése](active-directory-b2c-reference-tokens.md) , amely a hello token tooprove eredetiségét, valamint hello megfelelő jogcímeket.

Azt a rendszer mindig megnyitott toofeedback és javaslatok! Ha bármilyen nehézségbe ütközik az ebben a témakörben, tegye a Stack Overflow hello címke használata ["azure-ad-b2c"](https://stackoverflow.com/questions/tagged/azure-ad-b2c). A szolgáltatás kéréseket, vegye fel őket túl[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).

## <a name="next-steps"></a>Következő lépések

* Egy webes API használatával Build [.NET Core](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi)
* Egy webes API használatával Build [Node.JS](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi)
