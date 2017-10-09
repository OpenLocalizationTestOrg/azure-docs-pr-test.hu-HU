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
## <a name="set-up-your-project"></a><span data-ttu-id="773ec-103">A projekt beállítása</span><span class="sxs-lookup"><span data-stu-id="773ec-103">Set up your project</span></span>

<span data-ttu-id="773ec-104">Ez a szakasz hello lépéseket tooinstall jeleníti meg, és állítsa be hello hitelesítési folyamatot OWIN köztes keresztül egy ASP.NET-projekt OpenID Connect használatával.</span><span class="sxs-lookup"><span data-stu-id="773ec-104">This section shows hello steps tooinstall and configure hello authentication pipeline via OWIN middleware on an ASP.NET project using OpenID Connect.</span></span> 

> <span data-ttu-id="773ec-105">Inkább toodownload Ez a minta Visual Studio-projekt helyett?</span><span class="sxs-lookup"><span data-stu-id="773ec-105">Prefer toodownload this sample's Visual Studio project instead?</span></span> <span data-ttu-id="773ec-106">[Töltse le a projekt](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip) toohello kihagyása [konfigurációs lépés](#create-an-application-express) tooconfigure hello kódminta végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="773ec-106">[Download a project](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip) and skip toohello [Configuration step](#create-an-application-express) tooconfigure hello code sample before executing.</span></span>

<!--start-collapse-->
> ### <a name="create-your-aspnet-project"></a><span data-ttu-id="773ec-107">Az ASP.NET-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="773ec-107">Create your ASP.NET project</span></span>

> 1. <span data-ttu-id="773ec-108">A Visual Studio:`File` > `New` > `Project`</span><span class="sxs-lookup"><span data-stu-id="773ec-108">In Visual Studio: `File` > `New` > `Project`</span></span><br/>
> 2. <span data-ttu-id="773ec-109">A *Visual C# \Web*, jelölje be `ASP.NET Web Application (.NET Framework)`.</span><span class="sxs-lookup"><span data-stu-id="773ec-109">Under *Visual C#\Web*, select `ASP.NET Web Application (.NET Framework)`.</span></span>
> 3. <span data-ttu-id="773ec-110">Az alkalmazás neve, és kattintson a *OK*</span><span class="sxs-lookup"><span data-stu-id="773ec-110">Name your application and click *OK*</span></span>
> 4. <span data-ttu-id="773ec-111">Válassza ki `Empty` és select hello jelölőnégyzet tooadd `MVC` hivatkozások</span><span class="sxs-lookup"><span data-stu-id="773ec-111">Select `Empty` and select hello checkbox tooadd `MVC` references</span></span>
<!--end-collapse-->

## <a name="add-authentication-components"></a><span data-ttu-id="773ec-112">Hitelesítés-összetevők hozzáadása</span><span class="sxs-lookup"><span data-stu-id="773ec-112">Add authentication components</span></span>

1. <span data-ttu-id="773ec-113">A Visual Studio:`Tools` > `Nuget Package Manager` > `Package Manager Console`</span><span class="sxs-lookup"><span data-stu-id="773ec-113">In Visual Studio: `Tools` > `Nuget Package Manager` > `Package Manager Console`</span></span>
2. <span data-ttu-id="773ec-114">Adja hozzá *OWIN köztes NuGet-csomagok* hello Package Manager Console ablakban írja be a következő hello:</span><span class="sxs-lookup"><span data-stu-id="773ec-114">Add *OWIN middleware NuGet packages* by typing hello following in hello Package Manager Console window:</span></span>

```powershell
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb
```

<!--start-collapse-->
> ### <a name="about-these-libraries"></a><span data-ttu-id="773ec-115">Ezek a kódtárak kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="773ec-115">About these libraries</span></span>

><span data-ttu-id="773ec-116">a fenti hello szalagtárak engedélyezése az egyszeri bejelentkezés (SSO) használata az OpenID Connect cookie-alapú hitelesítéssel.</span><span class="sxs-lookup"><span data-stu-id="773ec-116">hello libraries above enable single sign-on (SSO) using OpenID Connect via cookie-based authentication.</span></span> <span data-ttu-id="773ec-117">Után a hitelesítés végbemegy, és hello felhasználói képviselő hello token tooyour alkalmazás küldött, az OWIN köztes egy munkamenetcookie-t hoz létre.</span><span class="sxs-lookup"><span data-stu-id="773ec-117">After authentication is completed and hello token representing hello user is sent tooyour application, OWIN middleware creates a session cookie.</span></span> <span data-ttu-id="773ec-118">hello böngésző majd használ a cookie-k további kérések így hello felhasználónak nem kell tooretype a jelszavát, és nincs további ellenőrzés van szükség.</span><span class="sxs-lookup"><span data-stu-id="773ec-118">hello browser then uses this cookie on subsequent requests so hello user doesn't need tooretype their password, and no additional verification is needed.</span></span>
<!--end-collapse-->

## <a name="configure-hello-authentication-pipeline"></a><span data-ttu-id="773ec-119">Hello hitelesítési folyamatot konfigurálása</span><span class="sxs-lookup"><span data-stu-id="773ec-119">Configure hello authentication pipeline</span></span>
<span data-ttu-id="773ec-120">hello az alábbi lépésekre használt toocreate egy OWIN köztes indítási osztály tooconfigure OpenID Connect hitelesítési.</span><span class="sxs-lookup"><span data-stu-id="773ec-120">hello steps below are used toocreate an OWIN middleware Startup Class tooconfigure OpenID Connect authentication.</span></span> <span data-ttu-id="773ec-121">Ez az osztály a lesz automatikusan végre, amikor az IIS-folyamat elindul.</span><span class="sxs-lookup"><span data-stu-id="773ec-121">This class will be executed automatically when your IIS process starts.</span></span>

> <span data-ttu-id="773ec-122">Ha a projekt nem rendelkezik egy `Startup.cs` fájl a gyökérmappában hello:</span><span class="sxs-lookup"><span data-stu-id="773ec-122">If your project doesn't have a `Startup.cs` file in hello root folder:</span></span><br/>
> 1. <span data-ttu-id="773ec-123">Kattintson jobb gombbal a projekt hello gyökérmappa: >`Add` > `New Item...` > `OWIN Startup class`</span><span class="sxs-lookup"><span data-stu-id="773ec-123">Right click on hello project's root folder: >  `Add` > `New Item...` > `OWIN Startup class`</span></span><br/>
> 2. <span data-ttu-id="773ec-124">Nevezze el`Startup.cs`</span><span class="sxs-lookup"><span data-stu-id="773ec-124">Name it `Startup.cs`</span></span>

> <span data-ttu-id="773ec-125">Ellenőrizze, hogy kiválasztott hello osztály OWIN indítási osztály és a nem szabványos C#-osztály.</span><span class="sxs-lookup"><span data-stu-id="773ec-125">Make sure hello class selected is an OWIN Startup Class and not a standard C# class.</span></span> <span data-ttu-id="773ec-126">Ellenőrizheti, ha ellenőrzésével `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` hello névtér fent.</span><span class="sxs-lookup"><span data-stu-id="773ec-126">Confirm this by checking if you see `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` above hello namespace.</span></span>


1. <span data-ttu-id="773ec-127">Adja hozzá *OWIN* és *Microsoft.IdentityModel* túl hivatkozik`Startup.cs`:</span><span class="sxs-lookup"><span data-stu-id="773ec-127">Add *OWIN* and *Microsoft.IdentityModel* references too`Startup.cs`:</span></span>

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
<span data-ttu-id="773ec-128">Cserélje le indítási osztály hello kódot:</span><span class="sxs-lookup"><span data-stu-id="773ec-128">Replace Startup class with hello code below:</span></span>
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
> ### <a name="more-information"></a><span data-ttu-id="773ec-129">További információ</span><span class="sxs-lookup"><span data-stu-id="773ec-129">More Information</span></span>

> <span data-ttu-id="773ec-130">Megadja a paraméterek hello *OpenIDConnectAuthenticationOptions* hello alkalmazás toocommunicate az Azure AD-koordináták szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="773ec-130">hello parameters you provide in *OpenIDConnectAuthenticationOptions* serve as coordinates for hello application toocommunicate with Azure AD.</span></span> <span data-ttu-id="773ec-131">Hello OpenID Connect köztes hello háttérben cookie-kat használ, mert szükség cookie-k hitelesítés tooset hello kódú fent látható.</span><span class="sxs-lookup"><span data-stu-id="773ec-131">Because hello OpenID Connect middleware uses cookies in hello background, you also need tooset up cookie authentication as hello code above shows.</span></span> <span data-ttu-id="773ec-132">Hello *ValidateIssuer* értéke közli OpenIdConnect toonot tooone adott szervezeti hozzáférés korlátozása.</span><span class="sxs-lookup"><span data-stu-id="773ec-132">hello *ValidateIssuer* value tells OpenIdConnect toonot restrict access tooone specific organization.</span></span>
<!--end-collapse-->

