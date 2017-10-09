---
title: "aaaAzure AD v2.0 .NET web app bejelentkezési első lépések |} Microsoft Docs"
description: "Hogyan toobuild .NET MVC webes alkalmazás, amely bejelentkezik felhasználók mindkét személyes Microsoft Account és munkahelyi vagy iskolai fiókját."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: c8b97ac6-0a06-4367-81b6-7d1d98152b14
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 241e9c90bd752fbecc3696ce4f1bed3f9772189d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooan-net-mvc-web-app"></a>Bejelentkezési tooan .NET MVC webalkalmazás hozzáadása
Hello v2.0-végponttal gyorsan hitelesítési tooyour webalkalmazások támogatja a személyes Microsoft-fiókot is, valamint a munkahelyi vagy iskolai fiókokat is hozzáadhat.  Az ASP.NET web apps Ez elvégezhető a .NET-keretrendszer 4.5 része a Microsoft OWIN köztes használatával.

> [!NOTE]
> Nem minden Azure Active Directory forgatókönyvek és funkciók támogatják hello v2.0-végponttól.  toodetermine használatát hello v2.0-végpontra, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).
>
>

 Itt azt fogja hozza létre egy webes alkalmazás megjelenítési az OWIN toosign hello felhasználói használó hello felhasználói adatait, és bejelentkezési hello felhasználói hello alkalmazásból.

## <a name="download"></a>Letöltés
az oktatóanyag kódjának hello kezelt [a Githubon](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet).  toofollow mellett, akkor [töltse le a .zip hello alkalmazás vázát](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) vagy a Klónozás hello vázat:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

hello végén, valamint az oktatóanyag befejezése hello app valósul meg.

## <a name="register-an-app"></a>Alkalmazás regisztrálása
Hozzon létre egy új alkalmazást [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy kövesse az alábbi [részletes lépéseket](active-directory-v2-app-registration.md).  Győződjön meg arról, hogy:

* Másolja le hello **alkalmazásazonosító** tooyour app hozzárendelve, szüksége lehet rájuk hamarosan.
* Adja hozzá a hello **webes** platform az alkalmazásra vonatkozóan.
* Adja meg a megfelelő hello **átirányítási URI-**. hello átirányítási uri azt jelzi, ha hitelesítési válaszok legyenek irányítva – hello ebben az oktatóanyagban alapértelmezés szerint AD tooAzure `https://localhost:44326/`.

## <a name="install--configure-owin-authentication"></a>Telepítse és konfigurálja az OWIN hitelesítést
Itt konfigurálását végezzük el hello OWIN köztes toouse hello OpenID Connect hitelesítési protokoll.  OWIN kell használt tooissue be- és kijelentkezési kérések, hello felhasználói munkamenet kezeli, és többek között a hello felhasználó adatainak beolvasása.

1. toobegin, nyissa meg hello `web.config` hello projekt gyökerében hello fájlt, és adja meg az alkalmazás konfigurációs értékeit hello `<appSettings>` szakasz.

  * Hello `ida:ClientId` hello van **alkalmazásazonosító** tooyour app hello regisztrációs portálon rendelt.
  * Hello `ida:RedirectUri` hello van **átirányítási Uri-** hello portálon megadott.

2. Ezután adja hozzá a hello OWIN köztes NuGet csomagjainak toohello projekt hello Csomagkezelő konzol használatával.

        ```
        PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
        PM> Install-Package Microsoft.Owin.Security.Cookies
        PM> Install-Package Microsoft.Owin.Host.SystemWeb
        ```  

3. Az "OWIN indítási osztály" toohello projektet nevezik Hozzáadás `Startup.cs` jobb hello projektben kattintson--> **Hozzáadás** --> **új elem** --> "OWIN" keresése.  hello OWIN köztes által aktivált hello `Configuration(...)` módszer az alkalmazás indításakor.
4. Hello osztálydeklaráció túl módosítása`public partial class Startup` -azt korábban már megvalósított Ez az osztály tartozik, egy másik fájlban.  A hello `Configuration(...)` módszert, győződjön meg a hívás tooConfigureAuth(...) tooset hitelesítés a webalkalmazás  

        ```C#
        [assembly: OwinStartup(typeof(Startup))]
        
        namespace TodoList_WebApp
        {
            public partial class Startup
            {
                public void Configuration(IAppBuilder app)
                {
                    ConfigureAuth(app);
                }
            }
        }
        ```

5. Nyissa meg hello fájl `App_Start\Startup.Auth.cs` és valósíthatnak meg hello `ConfigureAuth(...)` metódust.  Megadja a paraméterek hello `OpenIdConnectAuthenticationOptions` erre a célra az Azure ad alkalmazás toocommunicate koordinátáit.  Azt is el kell készíteni, hitelesítési cookie-k tooset – hello OpenID Connect köztes hello magában foglalja az alatta lévő cookie-kat használ.

        ```C#
        public void ConfigureAuth(IAppBuilder app)
                     {
                             app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);
        
                             app.UseCookieAuthentication(new CookieAuthenticationOptions());
        
                             app.UseOpenIdConnectAuthentication(
                                     new OpenIdConnectAuthenticationOptions
                                     {
                                             // hello `Authority` represents hello v2.0 endpoint - https://login.microsoftonline.com/common/v2.0
                                             // hello `Scope` describes hello permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                                             // In a real application you could use issuer validation for additional checks, like making sure hello user's organization has signed up for your app, for instance.
        
                                             ClientId = clientId,
                                             Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0"),
                                             RedirectUri = redirectUri,
                                             Scope = "openid email profile",
                                             ResponseType = "id_token",
                                             PostLogoutRedirectUri = redirectUri,
                                             TokenValidationParameters = new TokenValidationParameters
                                             {
                                                     ValidateIssuer = false,
                                             },
                                             Notifications = new OpenIdConnectAuthenticationNotifications
                                             {
                                                     AuthenticationFailed = OnAuthenticationFailed,
                                             }
                                     });
                     }
        ```

## <a name="send-authentication-requests"></a>Hitelesítési kérések küldése
Az alkalmazás már megfelelően konfigurált toocommunicate hello v2.0-végponttal hello OpenID Connect hitelesítési protokoll használatával.  OWIN rendelkezik végrehajtott összes hitelesítési üzenetek létrehozásával, ellenőrzése az Azure ad-jogkivonatok és karbantartásáról a felhasználói munkamenet ugly részleteit hello megvagyunk.  Hogy továbbra is van toogive a felhasználók egy a módon toosign és kijelentkezett.

- Használható címkék engedélyezik a tartományvezérlők toorequire az, hogy a felhasználó jelentkezik be egy bizonyos lap elérése előtt.  Nyissa meg `Controllers\HomeController.cs`, és adja hozzá a hello `[Authorize]` toohello vezérlő vonatkozó címke.
        
        ```C#
        [Authorize]
        public ActionResult About()
        {
          ...
        ```

- OWIN toodirectly probléma a érkező hitelesítési kéréseket a kód is használható.  Nyissa meg `Controllers\AccountController.cs`.  Hello SignIn() és SignOut() műveleteket adja a challenge OpenID Connect és kijelentkezési kérések ki.

        ```C#
        public void SignIn()
        {
            // Send an OpenID Connect sign-in request.
            if (!Request.IsAuthenticated)
            {
                HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
            }
        }
        
        // BUGBUG: Ending a session with hello v2.0 endpoint is not yet supported.  Here, we just end hello session with hello web app.  
        public void SignOut()
        {
            // Send an OpenID Connect sign-out request.
            HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
            Response.Redirect("/");
        }
        ```

- Most, nyissa meg a `Views\Shared\_LoginPartial.cshtml`.  Ez az amelyen hello felhasználói megjelenítése az alkalmazáshoz be- és kijelentkezési hivatkozásokat, és nyomtassa ki hello felhasználónév nézetben.

        ```HTML
        @if (Request.IsAuthenticated)
        {
            <text>
                <ul class="nav navbar-nav navbar-right">
                    <li class="navbar-text">
        
                        @*hello 'preferred_username' claim can be used for showing hello user's primary way of identifying themselves.*@
        
                        Hello, @(System.Security.Claims.ClaimsPrincipal.Current.FindFirst("preferred_username").Value)!
                    </li>
                    <li>
                        @Html.ActionLink("Sign out", "SignOut", "Account")
                    </li>
                </ul>
            </text>
        }
        else
        {
            <ul class="nav navbar-nav navbar-right">
                <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
            </ul>
        }
        ```

## <a name="display-user-information"></a>Felhasználói adatok megjelenítése
Az OpenID Connect felhasználók hitelesítésekor hello v2.0-végponttal, amely tartalmazza a jogcímeket vagy helyességi feltételek hello felhasználóról id_token toohello alkalmazás adja vissza.  A jogcímek toopersonalize az alkalmazás használhatja:

- Nyissa meg hello `Controllers\HomeController.cs` fájlt.  A tartományvezérlők keresztül hello keresztül elérhető hello felhasználói jogcímek `ClaimsPrincipal.Current` rendszerbiztonsági objektumot.

        ```C#
        [Authorize]
        public ActionResult About()
        {
            ViewBag.Name = ClaimsPrincipal.Current.FindFirst("name").Value;
        
            // hello object ID claim will only be emitted for work or school accounts at this time.
            Claim oid = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier");
            ViewBag.ObjectId = oid == null ? string.Empty : oid.Value;
        
            // hello 'preferred_username' claim can be used for showing hello user's primary way of identifying themselves
            ViewBag.Username = ClaimsPrincipal.Current.FindFirst("preferred_username").Value;
        
            // hello subject or nameidentifier claim can be used toouniquely identify hello user
            ViewBag.Subject = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        
            return View();
        }
        ```

## <a name="run"></a>Futtassa a következőt:
Végül felépítéséhez és az alkalmazás futtatása!   Jelentkezzen be személyes Microsoft-Account vagy a munkahelyi vagy iskolai fiókkal, és figyelje meg, hogyan hello felhasználói identitás hello felső navigációs sáv megjelenik.  Most már rendelkezik egy webalkalmazást, amely képes hitelesíteni a személyes és munkahelyi vagy iskolai fiókkal rendelkező felhasználók iparági szabványos protokollok használatával biztonságossá.

Referenciaként hello befejeződött (a konfigurációs értékek nélkül) minta [is letöltheti a .zip Itt](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip), vagy a Githubból is klónozhatja:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

## <a name="next-steps"></a>Következő lépések
Ön most már továbbléphet az összetettebb témákra.  Érdemes lehet tootry:

[A Web API-t hello hello v2.0-végponttól biztonságos >>](active-directory-devquickstarts-webapi-dotnet.md)

További forrásokért tekintse meg:

* [hello v2.0 – útmutató fejlesztőknek >>](active-directory-appmodel-v2-overview.md)
* [StackOverflow "azure-active-directory" címke >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Biztonsági frissítések termékeinkhez
Javasoljuk, tooget értesítést a bekövetkező biztonsági incidensekről ellátogatva [ezen a lapon](https://technet.microsoft.com/security/dd252948) és előfizetés tooSecurity tanácsadói riasztásokra.
