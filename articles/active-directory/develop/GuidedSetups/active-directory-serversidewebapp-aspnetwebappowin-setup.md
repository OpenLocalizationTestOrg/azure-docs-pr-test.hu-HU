---
title: "aaaAzure AD v2 ASP.NET Web Server bevezetés - telepítő |} Microsoft Docs"
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
ms.openlocfilehash: eadc59666557e9cd294e6e99391001120579144c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="set-up-your-project"></a>A projekt beállítása

Ez a szakasz hello lépéseket tooinstall jeleníti meg, és állítsa be hello hitelesítési folyamatot OWIN köztes keresztül egy ASP.NET-projekt OpenID Connect használatával. 

> Inkább toodownload Ez a minta Visual Studio-projekt helyett? [Töltse le a projekt](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip) toohello kihagyása [konfigurációs lépés](#create-an-application-express) tooconfigure hello kódminta végrehajtása előtt.

<!--start-collapse-->
> ### <a name="create-your-aspnet-project"></a>Az ASP.NET-projekt létrehozása

> 1. A Visual Studio:`File` > `New` > `Project`<br/>
> 2. A *Visual C# \Web*, jelölje be `ASP.NET Web Application (.NET Framework)`.
> 3. Az alkalmazás neve, és kattintson a *OK*
> 4. Válassza ki `Empty` és select hello jelölőnégyzet tooadd `MVC` hivatkozások
<!--end-collapse-->

## <a name="add-authentication-components"></a>Hitelesítés-összetevők hozzáadása

1. A Visual Studio:`Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Adja hozzá *OWIN köztes NuGet-csomagok* hello Package Manager Console ablakban írja be a következő hello:

```powershell
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb
```

<!--start-collapse-->
> ### <a name="about-these-libraries"></a>Ezek a kódtárak kapcsolatos

>a fenti hello szalagtárak engedélyezése az egyszeri bejelentkezés (SSO) használata az OpenID Connect cookie-alapú hitelesítéssel. Után a hitelesítés végbemegy, és hello felhasználói képviselő hello token tooyour alkalmazás küldött, az OWIN köztes egy munkamenetcookie-t hoz létre. hello böngésző majd használ a cookie-k további kérések így hello felhasználónak nem kell tooretype a jelszavát, és nincs további ellenőrzés van szükség.
<!--end-collapse-->

## <a name="configure-hello-authentication-pipeline"></a>Hello hitelesítési folyamatot konfigurálása
hello az alábbi lépésekre használt toocreate egy OWIN köztes indítási osztály tooconfigure OpenID Connect hitelesítési. Ez az osztály a lesz automatikusan végre, amikor az IIS-folyamat elindul.

> Ha a projekt nem rendelkezik egy `Startup.cs` fájl a gyökérmappában hello:<br/>
> 1. Kattintson jobb gombbal a projekt hello gyökérmappa: >`Add` > `New Item...` > `OWIN Startup class`<br/>
> 2. Nevezze el`Startup.cs`

> Ellenőrizze, hogy kiválasztott hello osztály OWIN indítási osztály és a nem szabványos C#-osztály. Ellenőrizheti, ha ellenőrzésével `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` hello névtér fent.


1. Adja hozzá *OWIN* és *Microsoft.IdentityModel* túl hivatkozik`Startup.cs`:

```csharp
using Microsoft.Owin;
using Owin;
using Microsoft.IdentityModel.Protocols;
using Microsoft.Owin.Security;
using Microsoft.Owin.Security.Cookies;
using Microsoft.Owin.Security.OpenIdConnect;
using Microsoft.Owin.Security.Notifications;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Cserélje le indítási osztály hello kódot:
</li>
</ol>

```csharp
public class Startup
{        
    // hello Client ID is used by hello application toouniquely identify itself tooAzure AD.
    string clientId = System.Configuration.ConfigurationManager.AppSettings["ClientId"];

    // RedirectUri is hello URL where hello user will be redirected tooafter they sign in.
    string redirectUri = System.Configuration.ConfigurationManager.AppSettings["RedirectUri"];

    // Tenant is hello tenant ID (e.g. contoso.onmicrosoft.com, or 'common' for multi-tenant)
    static string tenant = System.Configuration.ConfigurationManager.AppSettings["Tenant"];

    // Authority is hello URL for authority, composed by Azure Active Directory v2 endpoint and hello tenant name (e.g. https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0)
    string authority = String.Format(System.Globalization.CultureInfo.InvariantCulture, System.Configuration.ConfigurationManager.AppSettings["Authority"], tenant);

    /// <summary>
    /// Configure OWIN toouse OpenIdConnect 
    /// </summary>
    /// <param name="app"></param>
    public void Configuration(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseCookieAuthentication(new CookieAuthenticationOptions());
            app.UseOpenIdConnectAuthentication(
            new OpenIdConnectAuthenticationOptions
            {
                // Sets hello ClientId, authority, RedirectUri as obtained from web.config
                ClientId = clientId,
                Authority = authority,
                RedirectUri = redirectUri,
                // PostLogoutRedirectUri is hello page that users will be redirected tooafter sign-out. In this case, it is using hello home page
                PostLogoutRedirectUri = redirectUri,
                Scope = OpenIdConnectScopes.OpenIdProfile,
                // ResponseType is set toorequest hello id_token - which contains basic information about hello signed-in user
                ResponseType = OpenIdConnectResponseTypes.IdToken,
                // ValidateIssuer set toofalse tooallow personal and work accounts from any organization toosign in tooyour application
                // tooonly allow users from a single organizations, set ValidateIssuer tootrue and 'tenant' setting in web.config toohello tenant name
                // tooallow users from only a list of specific organizations, set ValidateIssuer tootrue and use ValidIssuers parameter 
                TokenValidationParameters = new System.IdentityModel.Tokens.TokenValidationParameters() { ValidateIssuer = false },
                // OpenIdConnectAuthenticationNotifications configures OWIN toosend notification of failed authentications tooOnAuthenticationFailed method
                Notifications = new OpenIdConnectAuthenticationNotifications
                {
                    AuthenticationFailed = OnAuthenticationFailed
                }
            }
        );
    }

    /// <summary>
    /// Handle failed authentication requests by redirecting hello user toohello home page with an error in hello query string
    /// </summary>
    /// <param name="context"></param>
    /// <returns></returns>
    private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> context)
    {
        context.HandleResponse();
        context.Response.Redirect("/?errormessage=" + context.Exception.Message);
        return Task.FromResult(0);
    }
}

```
<!--start-collapse-->
> ### <a name="more-information"></a>További információ

> Megadja a paraméterek hello *OpenIDConnectAuthenticationOptions* hello alkalmazás toocommunicate az Azure AD-koordináták szolgálnak. Hello OpenID Connect köztes hello háttérben cookie-kat használ, mert szükség cookie-k hitelesítés tooset hello kódú fent látható. Hello *ValidateIssuer* értéke közli OpenIdConnect toonot tooone adott szervezeti hozzáférés korlátozása.
<!--end-collapse-->

