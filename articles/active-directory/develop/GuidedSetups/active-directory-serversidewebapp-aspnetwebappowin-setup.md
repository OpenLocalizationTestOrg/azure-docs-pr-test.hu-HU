---
title: "Az Azure AD v2 ASP.NET Web Server első lépések - telepítő |} Microsoft Docs"
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
ms.openlocfilehash: ebf54f5a203adb7f0e5b0c47dcc07595e269e218
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
## <a name="set-up-your-project"></a><span data-ttu-id="f1b25-103">A projekt beállítása</span><span class="sxs-lookup"><span data-stu-id="f1b25-103">Set up your project</span></span>

<span data-ttu-id="f1b25-104">Ez a szakasz ismerteti a lépéseket, telepítését és konfigurálását a hitelesítési folyamatot OWIN köztes keresztül egy ASP.NET-projekt OpenID Connect használatával.</span><span class="sxs-lookup"><span data-stu-id="f1b25-104">This section shows the steps to install and configure the authentication pipeline via OWIN middleware on an ASP.NET project using OpenID Connect.</span></span> 

> <span data-ttu-id="f1b25-105">Ez a minta Visual Studio-projekt letöltése helyette inkább?</span><span class="sxs-lookup"><span data-stu-id="f1b25-105">Prefer to download this sample's Visual Studio project instead?</span></span> <span data-ttu-id="f1b25-106">[Töltse le a projekt](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip) és ugorjon a [konfigurációs lépés](#create-an-application-express) konfigurálása a kódminta végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="f1b25-106">[Download a project](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip) and skip to the [Configuration step](#create-an-application-express) to configure the code sample before executing.</span></span>

<!--start-collapse-->
> ### <a name="create-your-aspnet-project"></a><span data-ttu-id="f1b25-107">Az ASP.NET-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="f1b25-107">Create your ASP.NET project</span></span>

> 1. <span data-ttu-id="f1b25-108">A Visual Studio:`File` > `New` > `Project`</span><span class="sxs-lookup"><span data-stu-id="f1b25-108">In Visual Studio: `File` > `New` > `Project`</span></span><br/>
> 2. <span data-ttu-id="f1b25-109">A *Visual C# \Web*, jelölje be `ASP.NET Web Application (.NET Framework)`.</span><span class="sxs-lookup"><span data-stu-id="f1b25-109">Under *Visual C#\Web*, select `ASP.NET Web Application (.NET Framework)`.</span></span>
> 3. <span data-ttu-id="f1b25-110">Az alkalmazás neve, és kattintson a *OK*</span><span class="sxs-lookup"><span data-stu-id="f1b25-110">Name your application and click *OK*</span></span>
> 4. <span data-ttu-id="f1b25-111">Válassza ki `Empty` hozzáadása jelölőnégyzet bejelölésével és `MVC` hivatkozások</span><span class="sxs-lookup"><span data-stu-id="f1b25-111">Select `Empty` and select the checkbox to add `MVC` references</span></span>
<!--end-collapse-->

## <a name="add-authentication-components"></a><span data-ttu-id="f1b25-112">Hitelesítés-összetevők hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f1b25-112">Add authentication components</span></span>

1. <span data-ttu-id="f1b25-113">A Visual Studio:`Tools` > `Nuget Package Manager` > `Package Manager Console`</span><span class="sxs-lookup"><span data-stu-id="f1b25-113">In Visual Studio: `Tools` > `Nuget Package Manager` > `Package Manager Console`</span></span>
2. <span data-ttu-id="f1b25-114">Adja hozzá *OWIN köztes NuGet-csomagok* , a Package Manager Console ablakban írja be a következő:</span><span class="sxs-lookup"><span data-stu-id="f1b25-114">Add *OWIN middleware NuGet packages* by typing the following in the Package Manager Console window:</span></span>

```powershell
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb
```

<!--start-collapse-->
> ### <a name="about-these-libraries"></a><span data-ttu-id="f1b25-115">Ezek a kódtárak kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="f1b25-115">About these libraries</span></span>

><span data-ttu-id="f1b25-116">A fenti könyvtárak engedélyezése az egyszeri bejelentkezés (SSO) használata az OpenID Connect cookie-alapú hitelesítéssel.</span><span class="sxs-lookup"><span data-stu-id="f1b25-116">The libraries above enable single sign-on (SSO) using OpenID Connect via cookie-based authentication.</span></span> <span data-ttu-id="f1b25-117">Miután a hitelesítés végbemegy, és a felhasználó jelképező jogkivonatot kap az alkalmazáshoz, OWIN köztes egy munkamenetcookie-t hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f1b25-117">After authentication is completed and the token representing the user is sent to your application, OWIN middleware creates a session cookie.</span></span> <span data-ttu-id="f1b25-118">A böngésző a felhasználónak nem kell írnia a jelszavát, és nincs további ellenőrzés van szükség, majd használja a cookie későbbi kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="f1b25-118">The browser then uses this cookie on subsequent requests so the user doesn't need to retype their password, and no additional verification is needed.</span></span>
<!--end-collapse-->

## <a name="configure-the-authentication-pipeline"></a><span data-ttu-id="f1b25-119">A hitelesítési folyamatot konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f1b25-119">Configure the authentication pipeline</span></span>
<span data-ttu-id="f1b25-120">Az alábbi lépéseket egy OWIN indítási osztály konfigurálása az OpenID Connect hitelesítési köztes létrehozásához használt.</span><span class="sxs-lookup"><span data-stu-id="f1b25-120">The steps below are used to create an OWIN middleware Startup Class to configure OpenID Connect authentication.</span></span> <span data-ttu-id="f1b25-121">Ez az osztály a lesz automatikusan végre, amikor az IIS-folyamat elindul.</span><span class="sxs-lookup"><span data-stu-id="f1b25-121">This class will be executed automatically when your IIS process starts.</span></span>

> <span data-ttu-id="f1b25-122">Ha a projekt nem rendelkezik egy `Startup.cs` fájl a gyökérmappában található:</span><span class="sxs-lookup"><span data-stu-id="f1b25-122">If your project doesn't have a `Startup.cs` file in the root folder:</span></span><br/>
> 1. <span data-ttu-id="f1b25-123">Kattintson jobb gombbal a projekt gyökérmappához: >`Add` > `New Item...` > `OWIN Startup class`</span><span class="sxs-lookup"><span data-stu-id="f1b25-123">Right click on the project's root folder: >    `Add` > `New Item...` > `OWIN Startup class`</span></span><br/>
> 2. <span data-ttu-id="f1b25-124">Nevezze el`Startup.cs`</span><span class="sxs-lookup"><span data-stu-id="f1b25-124">Name it `Startup.cs`</span></span>

> <span data-ttu-id="f1b25-125">Győződjön meg arról, hogy a kiválasztott osztály egy OWIN indítási osztályt és a nem szabványos C#-osztály.</span><span class="sxs-lookup"><span data-stu-id="f1b25-125">Make sure the class selected is an OWIN Startup Class and not a standard C# class.</span></span> <span data-ttu-id="f1b25-126">Ellenőrizheti, ha ellenőrzésével `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` fent a névteret.</span><span class="sxs-lookup"><span data-stu-id="f1b25-126">Confirm this by checking if you see `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` above the namespace.</span></span>


1. <span data-ttu-id="f1b25-127">Adja hozzá *OWIN* és *Microsoft.IdentityModel* hivatkozása `Startup.cs`:</span><span class="sxs-lookup"><span data-stu-id="f1b25-127">Add *OWIN* and *Microsoft.IdentityModel* references to `Startup.cs`:</span></span>

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
<span data-ttu-id="f1b25-128">Indítási osztály cserélje le az alábbi kódot:</span><span class="sxs-lookup"><span data-stu-id="f1b25-128">Replace Startup class with the code below:</span></span>
</li>
</ol>

```csharp
public class Startup
{        
    // The Client ID is used by the application to uniquely identify itself to Azure AD.
    string clientId = System.Configuration.ConfigurationManager.AppSettings["ClientId"];

    // RedirectUri is the URL where the user will be redirected to after they sign in.
    string redirectUri = System.Configuration.ConfigurationManager.AppSettings["RedirectUri"];

    // Tenant is the tenant ID (e.g. contoso.onmicrosoft.com, or 'common' for multi-tenant)
    static string tenant = System.Configuration.ConfigurationManager.AppSettings["Tenant"];

    // Authority is the URL for authority, composed by Azure Active Directory v2 endpoint and the tenant name (e.g. https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0)
    string authority = String.Format(System.Globalization.CultureInfo.InvariantCulture, System.Configuration.ConfigurationManager.AppSettings["Authority"], tenant);

    /// <summary>
    /// Configure OWIN to use OpenIdConnect 
    /// </summary>
    /// <param name="app"></param>
    public void Configuration(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseCookieAuthentication(new CookieAuthenticationOptions());
            app.UseOpenIdConnectAuthentication(
            new OpenIdConnectAuthenticationOptions
            {
                // Sets the ClientId, authority, RedirectUri as obtained from web.config
                ClientId = clientId,
                Authority = authority,
                RedirectUri = redirectUri,
                // PostLogoutRedirectUri is the page that users will be redirected to after sign-out. In this case, it is using the home page
                PostLogoutRedirectUri = redirectUri,
                Scope = OpenIdConnectScopes.OpenIdProfile,
                // ResponseType is set to request the id_token - which contains basic information about the signed-in user
                ResponseType = OpenIdConnectResponseTypes.IdToken,
                // ValidateIssuer set to false to allow personal and work accounts from any organization to sign in to your application
                // To only allow users from a single organizations, set ValidateIssuer to true and 'tenant' setting in web.config to the tenant name
                // To allow users from only a list of specific organizations, set ValidateIssuer to true and use ValidIssuers parameter 
                TokenValidationParameters = new System.IdentityModel.Tokens.TokenValidationParameters() { ValidateIssuer = false },
                // OpenIdConnectAuthenticationNotifications configures OWIN to send notification of failed authentications to OnAuthenticationFailed method
                Notifications = new OpenIdConnectAuthenticationNotifications
                {
                    AuthenticationFailed = OnAuthenticationFailed
                }
            }
        );
    }

    /// <summary>
    /// Handle failed authentication requests by redirecting the user to the home page with an error in the query string
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
> ### <a name="more-information"></a><span data-ttu-id="f1b25-129">További információ</span><span class="sxs-lookup"><span data-stu-id="f1b25-129">More Information</span></span>

> <span data-ttu-id="f1b25-130">Megadja a paraméterek *OpenIDConnectAuthenticationOptions* kommunikálni az Azure AD alkalmazás-koordináták szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="f1b25-130">The parameters you provide in *OpenIDConnectAuthenticationOptions* serve as coordinates for the application to communicate with Azure AD.</span></span> <span data-ttu-id="f1b25-131">Az OpenID Connect köztes cookie-kat használ, a háttérben, mert akkor is be kell állítania cookie-hitelesítés, a fent látható kódot.</span><span class="sxs-lookup"><span data-stu-id="f1b25-131">Because the OpenID Connect middleware uses cookies in the background, you also need to set up cookie authentication as the code above shows.</span></span> <span data-ttu-id="f1b25-132">A *ValidateIssuer* értéke közli OpenIdConnect úgy nem korlátozza a hozzáférést egy adott szervezet számára.</span><span class="sxs-lookup"><span data-stu-id="f1b25-132">The *ValidateIssuer* value tells OpenIdConnect to not restrict access to one specific organization.</span></span>
<!--end-collapse-->

