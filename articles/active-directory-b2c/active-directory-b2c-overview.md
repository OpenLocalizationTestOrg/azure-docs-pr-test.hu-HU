---
title: "Áttekintés – Azure AD B2C | Microsoft Docs"
description: "A felhasználók felé néző alkalmazások fejlesztése az Azure Active Directory B2C-vel"
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: c465dbde-f800-4f2e-8814-0ff5f5dae610
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/06/2016
ms.author: saeedakhter-msft
ms.openlocfilehash: abfd742e710458de3193dc5051de7818a112376c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-focus-on-your-app-let-us-worry-about-sign-up-and-sign-in"></a>Azure AD B2C: Összpontosítson az alkalmazására, a regisztráció és a bejelentkezés a mi feladatunk

Az Azure AD B2C egy felhőbeli identitáskezelő megoldás a webes és mobilalkalmazásokhoz. Egy magas rendelkezésre állású globális szolgáltatás, amely az identitások millióinak toohundreds méretezi. A vállalati szintű biztonsági platformra épülő Azure AD B2C védelmet nyújt alkalmazásainak, vállalatának és ügyfeleinek.

Minimális konfiguráció esetén az Azure AD B2C lehetővé teszi, hogy az alkalmazás tooauthenticate:

* **Közösségi fiókok** (például Facebook, Google, LinkedIn stb.)
* **Vállalati fiókok** (nyílt szabványos protokollok, OpenID Connect vagy SAML használata esetén)
* **Helyi fiókok** (e-mail-cím és jelszó vagy felhasználónév és jelszó)

## <a name="get-started"></a>Bevezetés

Először leírt lépéseket hello segítségével a saját-bérlő beszerzése [az Azure AD B2C bérlő létrehozása](active-directory-b2c-get-started.md).

Ezután válassza ki az alkalmazásfejlesztési forgatókönyvet:

|  |  |  |  |
| --- | --- | --- | --- |
| <center>![Mobil- és asztali appok](../active-directory/develop/media/active-directory-developers-guide/NativeApp_Icon.png)<br />Mobil- és asztali appok</center> | [Áttekintés](active-directory-b2c-reference-oauth-code.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br /><br />[iOS](https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal)<br /><br />[Android](https://github.com/Azure-Samples/active-directory-b2c-android-native-msal) | [.NET](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop)<br /><br />[Xamarin](https://github.com/Azure-Samples/active-directory-b2c-xamarin-native) |  |
| <center>![Web Apps](../active-directory/develop/media/active-directory-developers-guide/Web_app.png)<br />Web Apps</center> | [Áttekintés](active-directory-b2c-reference-oidc.md)<br /><br />[ASP.NET](active-directory-b2c-devquickstarts-web-dotnet-susi.md)<br /><br />[ASP.NET Core](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapp) | [Node.js](active-directory-b2c-devquickstarts-web-node.md) |  |
| <center>![Egylapos appok](../active-directory/develop/media/active-directory-developers-guide/SPA.png)<br />Egylapos appok</center> | [Áttekintés](active-directory-b2c-reference-spa.md)<br /><br />[JavaScript](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp)<br /><br /> |  |  |
| <center>![Webes API-k](../active-directory/develop/media/active-directory-developers-guide/Web_API.png)<br />Webes API-k</center> | [ASP.NET](active-directory-b2c-devquickstarts-api-dotnet.md)<br /><br /> [ASP.NET Core](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi)<br /><br /> [Node.js](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi) | [.NET-alapú webes API hívása](active-directory-b2c-devquickstarts-web-api-dotnet.md) |

## <a name="whats-new"></a>Újdonságok

Térjen vissza ide gyakran kapcsolatos változásokról toohello Azure Active Directory B2C toolearn. A frissítésekről @AzureAD néven Twitter-üzeneteket is küldünk.

* Ezenkívül túl "Beépített házirendek" (általánosan rendelkezésre álló), hello ["Egyéni házirendek"](active-directory-b2c-overview-custom.md) funkció mostantól nyilvános előzetes verziójában.  Egyéni házirendek olyan identitás tapasztalataikat hello összetételének szabályozhatják igénylő identitás szakemberek számára.
* Hello [Access Token](https://azure.microsoft.com/en-us/blog/azure-ad-b2c-access-tokens-now-in-public-preview) funkció mostantól nyilvános előzetes verziójában.
* Bejelentettük az [európai Azure AD B2C-könyvtárak](https://azure.microsoft.com/en-us/blog/azuread-b2c-ga-eu/) általános elérhetőségét.
* Tekintse meg a [kódpéldák folyamatosan bővülő könyvtárát a Githubon](https://github.com/Azure-Samples?q=b2c)!

## <a name="how-tooarticles"></a>Hogyan-tooarticles

Megtudhatja, hogyan toouse Azure Active Directory B2C funkciók:

* [Facebook-](active-directory-b2c-setup-fb-app.md), [Google+-](active-directory-b2c-setup-goog-app.md), [Microsoft-](active-directory-b2c-setup-msa-app.md), [Amazon-](active-directory-b2c-setup-amzn-app.md) és [LinkedIn-](active-directory-b2c-setup-li-app.md)fiókok beállítása a felhasználók felé néző alkalmazásokban való használatra
* [Használja a toocollect egyéni attribútumok a felhasználókról](active-directory-b2c-reference-custom-attr.md).
* [Az Azure Multi-Factor Authentication alkalmazása a felhasználók felé néző alkalmazásokban](active-directory-b2c-reference-mfa.md)
* [Önkiszolgáló jelszóátállítás a felhasználók számára](active-directory-b2c-reference-sspr.md)
* [Hello megjelenésének és arculatának előfizetési testreszabása, jelentkezzen az és más felhasználók felé néző lapok](active-directory-b2c-reference-ui-customization.md) Azure Active Directory B2C által üzemeltetett.
* [Használjon hello Azure Active Directory Graph API tooprogrammatically létrehozása, olvasása, frissítése és törlése fogyasztók](active-directory-b2c-devquickstarts-graph-dotnet.md) az Azure Active Directory B2C-bérlőben.

## <a name="next-steps"></a>Következő lépések

Ezek a hivatkozások hasznosak felfedezése hello szolgáltatás részletesen:

* Lásd: hello [Azure Active Directory B2C díjszabási információit](https://azure.microsoft.com/pricing/details/active-directory-b2c/).
* Tekintse át az Azure Active Directory B2C-vel kapcsolatos [kódmintákat](https://azure.microsoft.com/en-us/resources/samples/?service=active-directory&term=b2c). 
* Segítség a Stack Overflow hello segítségével [azure-Active-b2c](http://stackoverflow.com/questions/tagged/azure-ad-b2c) címke.
* A gondolatait biztosítják [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c), azt szeretnénk, ha toohear őket!
* Felülvizsgálati hello [az Azure AD B2C protokoll referenciája](active-directory-b2c-reference-protocols.md).
* Felülvizsgálati hello [az Azure AD B2C-jogkivonatok Referenciájából](active-directory-b2c-reference-tokens.md).
* Olvasási hello [Azure Active Directory B2C – gyakori kérdések](active-directory-b2c-faqs.md).
* [Az Azure Active Directory B2C-vel kapcsolatos fájltámogatási kérések](active-directory-b2c-support.md).

## <a name="get-security-updates-for-our-products"></a>Biztonsági frissítések termékeinkhez

Javasoljuk, tooget értesítést a bekövetkező biztonsági incidensekről ellátogatva [ezen a lapon](https://technet.microsoft.com/security/dd252948) és előfizetés tooSecurity tanácsadói riasztásokra.

