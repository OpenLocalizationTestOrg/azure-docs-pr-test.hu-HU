---
title: "aaaAzure Active Directory-Kódminták |} Microsoft Docs"
description: "Azure Active Directory mintakódok forgatókönyv szerint vannak rendszerezve index."
services: active-directory
documentationcenter: dev-center-name
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: mbaldwin
ms.custom: aaddev
ms.openlocfilehash: 921ce08766cc6e29a6db2c4f38d216e6de11730f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-code-samples"></a>Az Azure Active Directory-Kódminták
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Használja a Microsoft Azure Active Directory (Azure AD) tooadd hitelesítési és engedélyezési tooyour webes alkalmazásokat, és a webes API-khoz. Ez a szakasz hivatkozásokat tartalmaz, akkor toosamples, amely bemutatja, hogyan történik és kódrészletek, amelyek az alkalmazások is használhatja. Hello minta kódlap találhat részletes olvasás-me témakörök, amelyek a követelmények, a telepítés és a telepítés számára. Hello kód megjegyzésként toohelp hello kritikus szakaszok ismernie.

toounderstand hello alapvető forgatókönyv az egyes minta, tekintse meg a hitelesítési forgatókönyvek az Azure AD.

Közreműködési lehetőségek tooour minták a Githubon: [Microsoft Azure Active Directory-példák és dokumentáció](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="web-browser-tooweb-application"></a>Webes böngésző tooWeb alkalmazás
Ezeket a mintákat megjelenítése hogyan toowrite egy webes alkalmazás, amely arra utasítja a hello felhasználó böngésző toosign tooAzure AD a őket.

| Nyelvi/Platform | Minta | Leírás |
| --- | --- | --- |
| C# / .NET |[Webalkalmazás-OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) |Az Azure AD-bérlő tooauthenticate felhasználók OpenID Connect (ASP.Net OpenID Connect OWIN köztes) használja. |
| C# / .NET |[Webalkalmazás-több-Bérlős-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) |Egy több-bérlős .NET MVC használó webalkalmazások OpenID Connect (ASP.Net OpenID Connect OWIN köztes) tooauthenticate felhasználók több Azure AD-bérlő. |
| C# / .NET |[Webalkalmazás-WSFederation-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) |Az Azure AD-bérlő felhasználóit WS-Federation (ASP.Net WS-Federation OWIN köztes) tooauthenticate használja. |

## <a name="single-page-application-spa"></a>Egylapos alkalmazások (SPA)
Ez a példa bemutatja, hogyan toowrite a egylapos alkalmazások biztosított az Azure ad-val.  

| Nyelvi/Platform | Minta | Leírás |
| --- | --- | --- |
| JavaScript, C# / .NET |[SinglePageApp-DotNet](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) |Adal-t használó JavaScript és az Azure AD egy AngularJS alapú toosecure egylapos alkalmazások egy ASP.NET web API háttér megvalósítva. |

## <a name="native-application-tooweb-api"></a>Natív alkalmazás tooWeb API
Ezek Kódminták bemutatják, hogyan toobuild natív ügyfél alkalmazások webes API-k, amelyek az Azure AD által biztosított. Használata [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) és [OAuth 2.0, az Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Nyelvi/Platform | Minta | Leírás |
| --- | --- | --- |
| Javascript |[NativeClient-MultiTarget-Cordova](https://github.com/Azure-Samples/active-directory-cordova-multitarget) |Hello ADAL beépülő modul használata Apache Cordova toobuild egy Apache Cordova-alkalmazást, amely meghívja a webes API-k és az Azure AD használ a hitelesítéshez. |
| C# / .NET |[NativeClient-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) |Egy .NET WPF-alkalmazás, amely meghívja a webes API-k, amelyek az Azure AD használatával lett biztonságossá. |
| C# / .NET |[NativeClient-WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) |Egy Windows Áruházbeli alkalmazás, amely meghívja a webes API-k, amelyek az Azure ad-vel védett. |
| C# / .NET |[NativeClient-WebAPI – több-Bérlős-WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) |A Windows Áruházbeli alkalmazással egy több-bérlős webes az Azure ad-vel védett API hívása. |
| C# / .NET |[WebAPI-OnBehalfOf-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof) |Egy natív ügyfélalkalmazást, amely behívja a webes API-k, amely lekérdezi egy token tooact hello eredeti felhasználó nevében, és ezután hello token toocall egy másik webes API-t. |
| C# / .NET |[NativeClient-WindowsPhone8.1](https://github.com/Azure-Samples/active-directory-dotnet-windowsphone-8.1) |A Windows Áruházbeli alkalmazás Windows Phone 8.1, amely behívja webes API-k, amelyek az Azure AD által védett. |
| ObjC |[NativeClient-iOS](https://github.com/Azure-Samples/active-directory-ios) |IOS-alkalmazás, amely behívja webes API-k, amelyek az Azure AD hitelesítési igényel. |
| C# / .NET |[WebAPI-ManuallyValidateJwt-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation) |Egy natív ügyfél tartalmazó alkalmazás logika tooprocess a JWT jogkivonat webes API-k, az OWIN köztes használata helyett. |
| C# / Xamarin |[NativeClient Xamarin-Android](https://github.com/Azure-Samples/active-directory-xamarin-android) |A Xamarin kötés toohello natív Azure AD Authentication Library (ADAL) az Androidos függvénytár hello. |
| C# / Xamarin |[NativeClient-Xamarin-iOS](https://github.com/Azure-Samples/active-directory-xamarin-ios) |A Xamarin kötés toohello natív Azure AD Authentication Library (ADAL) iOS-hez. |
| C# / Xamarin |[NativeClient-MultiTarget-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-multitarget) |A Xamarin-projekthez, amely öt platformok célozza, és meghívja a webes API-k, amelyek az Azure AD által védett. |
| C# / .NET |[NativeClient távfelügyeleti DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-headless) |Egy natív alkalmazás, amely nem interaktív hitelesítést hajt végre, és meghívja a webes API-k, amelyek az Azure AD által védett. |

## <a name="web-application-tooweb-api"></a>Webes alkalmazás tooWeb API
Megjelenítése hogyan használja a következő kód mintákat [OAuth 2.0, az Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) toobuild webes alkalmazásokhoz, amelyek a webes API-k hívásban, amely az Azure AD által biztosított.

| Nyelvi/Platform | Minta | Leírás |
| --- | --- | --- |
| C# / .NET |[Webalkalmazás-WebAPI-OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect) |Hello bejelentkezett felhasználó engedélyeivel webes API-hívás. |
| C# / .NET |[Webalkalmazás-WebAPI-OAuth2-AppIdentity-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity) |Hello alkalmazás engedélyekkel webes API-hívás. |
| C# / .NET |[Webalkalmazás-WebAPI-OAuth2-UserIdentity-Dotnet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) |Adja hozzá az engedélyezési [OAuth 2.0, az Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx) tooan meglévő webes alkalmazást, akkor meghívhatja a webes API-k. |
| JavaScript |[WebAPI-Nodejs](https://github.com/Azure-Samples/active-directory-node-webapi) |Állítsa be az Azure ad-val API védelmi integrált REST API-szolgáltatás. A Node.js-kiszolgáló egy webes API-t tartalmaz. |
| C# / .NET |[Webalkalmazás-WebAPI – több-Bérlős-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-multitenant-openidconnect) |Egy több-bérlős MVC webalkalmazás az Azure AD-bérlő tooauthenticate felhasználók OpenID Connect (ASP.Net OpenID Connect OWIN köztes) használja. Az engedélyezési kód tooinvoke hello Graph API-t használ. |

## <a name="server-or-daemon-application-tooweb-api"></a>Kiszolgáló vagy démon alkalmazás tooWeb API
A Kódminták megjelenítése hogyan toobuild démon vagy kiszolgálói alkalmazás, amely lekérdezi erőforrások a webes API-k használatával [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) és [OAuth 2.0, az Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).

| Nyelvi/Platform | Minta | Leírás |
| --- | --- | --- |
| C# / .NET |[Démon-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon) |A Konzolalkalmazás meghívja a webes API-k. meg kell jelszót hello ügyfél hitelesítő adatait. |
| C# / .NET |[Démon-CertificateCredential-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential) |Egy konzolalkalmazást, amely meghívja a webes API-k. meg kell egy tanúsítványt hello ügyfél hitelesítő adatait. |

## <a name="calling-azure-ad-graph-api"></a>Az Azure AD Graph API felület meghívásakor
Ezek kódminta bemutatják, hogyan toobuild alkalmazások hello Azure AD Graph API tooread, és írja a címtáradatok.

| Nyelvi/Platform | Minta | Leírás |
| --- | --- | --- |
| Java |[Webalkalmazás-GraphAPI-Java](https://github.com/Azure-Samples/active-directory-java-graphapi-web) |Egy webes alkalmazás hello Graph API tooaccess az Azure Active directory-adatokat. |
| PHP |[A PHP GraphAPI webalkalmazás](https://github.com/Azure-Samples/active-directory-php-graphapi-web) |Egy webes alkalmazás hello Graph API tooaccess az Azure Active directory-adatokat. |
| C# / .NET |[Webalkalmazás-GraphAPI-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) |Egy webes alkalmazás hello Graph API tooaccess az Azure Active directory-adatokat. |
| C# / .NET |[ConsoleApp-GraphAPI-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-console) |A Konzolalkalmazás közös olvasási és írási hívások toohello Graph API mutatja be, és bemutatja, hogyan tooexecute felhasználói licenc hozzárendelése, és frissítse egy felhasználó miniatűr fényképre és hivatkozásokat. |
| C# / .NET |[ConsoleApp-GraphAPI-DiffQuery-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-diffquery) |Egy konzolalkalmazást, amely a hello Graph API tooget rendszeres hello különbözeti lekérdezést használja az Azure AD-bérlő toouser objektumokat módosítja. |
| C# / .NET |[Webalkalmazás-GraphAPI-DirectoryExtensions-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-directoryextensions-web) |Egy MVC alkalmazás Graph API lekérdezések toogenerate egyszerű vállalati szervezeti diagram használja. |
| PHP |[Webalkalmazás-GraphAPI-DirectoryExtensions-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-directoryextensions-web) |A PHP-alkalmazás, amely meghívja hello Graph API tooregister bővítmény és olvassa el, frissítése és törlése hello mellék attribútum értékeket. |

## <a name="authorization"></a>Engedélyezés
Ezek az minták megjelenítése hogyan code toouse engedélyezési Azure ad-val.

| Nyelvi/Platform | Minta | Leírás |
| --- | --- | --- |
| C# / .NET |[Webalkalmazás-GroupClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) |Hajtsa végre a szerepköralapú hozzáférés-vezérlés (RBAC) Azure Active Directory csoport jogcímek használata az Azure ad-vel integrált alkalmazásban. |
| C# / .NET |[Webalkalmazás-RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) |Hajtsa végre a szerepkör alapú hozzáférés-vezérlést (RBAC) integrálva van az Azure AD alkalmazás Azure Active Directory-alkalmazás szerepköröket használ. |

## <a name="legacy-walkthroughs"></a>A hagyományos forgatókönyvek
Ezek a forgatókönyvek némileg régebbi technológiát használ, de továbbra is érdemes lehet.

| Nyelvi/Platform | Minta | Leírás |
| --- | --- | --- |
| C# / .NET |[A Microsoft Azure AD-alkalmazást a szerepkör- és hozzáférés-vezérlési lista-alapú engedélyezési](http://go.microsoft.com/fwlink/?LinkId=331694) |Hajtsa végre az Azure ad-vel integrált alkalmazás szerepköralapú hitelesítés (RBAC) és az ACL-alapú engedélyezési. |
| C# / .NET |[AAL - szolgáltatás a Windows Áruházbeli alkalmazás tooREST - hitelesítés](http://go.microsoft.com/fwlink/?LinkId=330605) |Használjon [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) (korábbi nevén AAL) a Windows áruház Beta tooadd felhasználói hitelesítési képességek tooa Windows áruház egy alkalmazásához. |
| C# / .NET |[A tallózási párbeszédpanelhez keresztül az AAD-ben ADAL - natív tooREST alkalmazásszolgáltatási - hitelesítés](http://go.microsoft.com/fwlink/?LinkId=259814) |Használjon [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) tooadd felhasználói hitelesítési képességek tooa WPF ügyfél. |
| C# / .NET |[A tallózási párbeszédpanelhez keresztül ACS ADAL - natív tooREST alkalmazásszolgáltatási - hitelesítés](http://code.msdn.microsoft.com/AAL-Native-App-to-REST-de57f2cc) |Használjon [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) és [Access Control Service 2.0 (ACS)](http://msdn.microsoft.com/library/azure/hh147631.aspx) tooadd felhasználói hitelesítési képességek tooa WPF ügyfél. |
| C# / .NET |[ADAL - hitelesítés Server tooServer](http://go.microsoft.com/fwlink/?LinkId=259816) |Használjon [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md) toosecure szolgáltatás hívást a kiszolgáló oldalán folyamat tooan MVC4 webes API REST-szolgáltatást. |
| C# / .NET |[Bejelentkezés tooYour webes alkalmazás használatával Azure felvétele AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) |Konfigurálja a .NET alkalmazás tooperform webes egyszeri bejelentkezés az Azure AD-vállalati címtár ellen. |
| C# / .NET |[Több-Bérlős webalkalmazások Azure AD-val létrehozása](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) |Használja az Azure AD tooadd toohello egyszeri bejelentkezést és a könyvtár hozzáférési képességeit egy .NET-alkalmazás toowork több szervezet. |
| JAVA |[Az Azure AD Graph API Java mintaalkalmazás](http://go.microsoft.com/fwlink/?LinkId=263969) |Hello Graph API tooaccess címtáradatok az Azure AD használatára. |
| PHP |[Az Azure AD Graph API PHP mintaalkalmazás](http://code.msdn.microsoft.com/PHP-Sample-App-For-Windows-228c6ddb) |Hello Graph API tooaccess címtáradatok az Azure AD használatára. |
| C# / .NET |[Az Azure AD Graph API mintaalkalmazás](http://go.microsoft.com/fwlink/?LinkID=262648) |Hello Graph API tooaccess címtáradatok az Azure AD használatára. |
| C# / .NET |[Az Azure AD Graph különbözeti lekérdezés mintaalkalmazás](http://go.microsoft.com/fwlink/?LinkId=275398) |Lekérdezéssel hello különbözeti hello Graph API tooget rendszeres módosítások toouser objektumok az Azure AD-bérlő. |
| C# / .NET |[Több-Bérlős felhőalapú alkalmazások az Azure Active Directory integrálásának mintaalkalmazás](http://go.microsoft.com/fwlink/?LinkId=275397) |A több-bérlős alkalmazások integrálása az Azure AD-be. |
| C# / .NET |[A Windows áruház-alkalmazás és az Azure AD REST webszolgáltatás biztonságossá tétele](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) |Hozzon létre egy egyszerű webes API-erőforrás és a Windows áruház ügyfélalkalmazás az Azure AD és hello [Azure AD Authentication Library (ADAL)](active-directory-authentication-libraries.md). |
| C# / .NET |[Hello Graph API tooQuery az Azure AD segítségével](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) |Konfigurálja a Microsoft .NET alkalmazás toouse hello Azure AD-bérlő címtár Azure AD Graph API tooaccess adatait. |

## <a name="see-also"></a>Lásd még:
##### <a name="other-resources"></a>Egyéb források
[Az Azure Active Directory fejlesztői útmutatója](active-directory-developers-guide.md)

[Az Azure AD Graph API fogalmi és referencia](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Az Azure AD Graph API Segédkódtárba helyezni](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
