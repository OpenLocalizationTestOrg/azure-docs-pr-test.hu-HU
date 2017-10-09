---
title: "aaaAzure AD v2 ASP.NET Web Server bevezetés - teszt |} Microsoft Docs"
description: "A Microsoft bejelentkezés végrehajtási egy ASP.NET-megoldás a hagyományos böngészőalapú webalkalmazás a szabványos OpenID Connect"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 99c7525b9146605142180962fc2a61b3c953c064
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="test-your-code"></a>Tesztelheti a kódját

Nyomja le az `F5` toorun a projektre a Visual Studióban. hello böngésző megnyitja és közvetlen, túl*http://localhost: {port}* ahol láthatja hello *jelentkezzen be Microsoft* gombra. Lépjen tovább, és kattintson rá a toosign.

Ha most készen áll a tootest, munkahelyi vagy iskolai (az Azure Active Directory), vagy egy személyes (live.com, outlook.com) toosign a fiókot használja. 

![Jelentkezzen be Microsoft-böngészőablakot](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin.png)

![Jelentkezzen be Microsoft-böngészőablakot](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin2.png)

#### <a name="expected-results"></a>Kívánt eredmény elérése érdekében
Bejelentkezéskor a hello felhasználói párt átirányított toohello kezdőlap a webhely, amely hello hello Microsoft alkalmazásregisztrációs portálra az alkalmazás regisztrációs adatait a megadott HTTPS URL-címet. Ezen a lapon látható most *Hello {felhasználó}* és egy hivatkozás toosign kibővített, és a hivatkozás toosee hello felhasználói jogcímek – amely egy hivatkozás toohello engedélyezés vezérlő korábban létrehozott-e.

### <a name="see-users-claims"></a>Tekintse meg a felhasználói jogcímek
Válassza ki a hello hivatkozás toosee hello felhasználói jogcímeket. A részletes útmutatást toohello tartományvezérlő, és tekintse meg, hogy csak hitelesített elérhető toousers.

#### <a name="expected-results"></a>Kívánt eredmény elérése érdekében
 Hello alaptulajdonságait hello bejelentkezett felhasználó tartalmazó táblát kell megjelennie:

| Tulajdonság | Érték | Leírás|
|---|---|---|
| Név | {Felhasználó teljes neve} | hello felhasználói vezeték- és keresztneve
|Felhasználónév | <span>user@domain.com</span>| hello felhasználónév használt tooidentify hello bejelentkezett felhasználó
| Tárgy| {Tulajdonos}|Egy karakterlánc toouniquely hello felhasználói bejelentkezési hello weben azonosításához.|
| Bérlőazonosító| {Guid}| A *guid* toouniquely hello felhasználó Azure Active Directory szervezetet képviseljék.|

Ezenkívül megjelenik egy táblázatot, beleértve az összes felhasználó szerepel a hitelesítési kérelmet. Egy azonosító Token és magyarázat minden jogcím listáját lásd: a [cikk](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "lista a jogcímek a lexikális elem azonosítója").


### <a name="test-accessing-a-method-that-has-an-authorize-attribute-optional"></a>Egy metódust, amelynek elérésekor teszt egy *[engedélyezés]* attribútum (nem kötelező)
Ebben a lépésben tesztelni elérése során hello hitelesített vezérlő névtelen felhasználóként:<br/>
Válassza ki a hello hivatkozás toosign kibővített hello felhasználó- és kijelentkezési folyamat teljes hello.<br/>
A böngészőben, a parancssorba írja be a http://localhost: {port} / hitelesített tooaccess a tartományvezérlő által védett hello `[Authorize]` attribútum

#### <a name="expected-results"></a>Kívánt eredmény elérése érdekében
Nincs szükség tooauthenticate toosee hello nézet hello kérdés kell kapnia.

## <a name="additional-information"></a>További információ

<!--start-collapse-->
### <a name="protect-your-entire-web-site"></a>A teljes webhelyet védelme
tooprotect a teljes webhelyet hello hozzáadása `AuthorizeAttribute` túl`GlobalFilters` a `Global.asax` `Application_Start` módszert:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

<div></div>
<br/>

> [!NOTE]
> **Hogyan tooyour alkalmazásban csak egy szervezet toosign toorestrict felhasználók**

> Alapértelmezés szerint személyes fiókok (például outlook.com, live.com, és egyéb), valamint a vállalat vagy szervezet, amely az Azure Active Directoryval integrált munkahelyi és iskolai fiókok is bejelentkezhet tooyour alkalmazás. 

> Ha azt szeretné, hogy az alkalmazás tooaccept bejelentkezések csak egy Azure Active Directory szervezeti, cserélje le a hello `Tenant` paramétere *web.config* a `Common` toohello bérlő neve hello szervezet – példa *contoso.onmicrosoft.com*. Ezt követően módosíthatja a hello `ValidateIssuer` argumentumának a *OWIN indítási osztály* túl`true`.

> tooallow felhasználók közül csak bizonyos szervezetekkel meg `ValidateIssuer` tootrue és -felhasználási hello `ValidIssuers` paraméter toospecify szervezetek listáját.

> Egy másik lehetőség van egyéni módszer tooimplement toovalidate hello kiállítók IssuerValidator paraméter használatával. További információ `TokenValidationParameters`, lásd: [ez](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "TokenValidationParameters MSDN-cikk") MSDN-cikk tárgyalja.

