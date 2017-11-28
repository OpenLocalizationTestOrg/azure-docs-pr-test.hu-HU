---
title: "Az Azure AD v2.0 .NET web app bejelentkezési első lépések |} Microsoft Docs"
description: "Hogyan hozhat létre egy .NET MVC webalkalmazás, mely aláírja a felhasználók be mindkét személyes Microsoft-Account és a munkahelyi vagy iskolai fiókját."
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
ms.openlocfilehash: ba5bdf7daba6086b70aec54ebe25d4445fa708c3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-sign-in-to-an-net-mvc-web-app"></a><span data-ttu-id="2edb8-103">Bejelentkezés hozzáadása egy .NET MVC webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="2edb8-103">Add sign-in to an .NET MVC web app</span></span>
<span data-ttu-id="2edb8-104">A v2.0-végponttal támogatja a személyes Microsoft-fiókot is a webes alkalmazásokhoz való hitelesítés és a munkahelyi vagy iskolai fiókok gyorsan is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="2edb8-104">With the v2.0 endpoint, you can quickly add authentication to your web apps with support for both personal Microsoft accounts and work or school accounts.</span></span>  <span data-ttu-id="2edb8-105">Az ASP.NET web apps Ez elvégezhető a .NET-keretrendszer 4.5 része a Microsoft OWIN köztes használatával.</span><span class="sxs-lookup"><span data-stu-id="2edb8-105">In ASP.NET web apps, you can accomplish this using Microsoft's OWIN middleware included in .NET Framework 4.5.</span></span>

> [!NOTE]
> <span data-ttu-id="2edb8-106">Nem minden Azure Active Directory forgatókönyvek és funkciók támogatják a v2.0-végponttól.</span><span class="sxs-lookup"><span data-stu-id="2edb8-106">Not all Azure Active Directory scenarios & features are supported by the v2.0 endpoint.</span></span>  <span data-ttu-id="2edb8-107">Annak meghatározásához, ha a v2.0-végponttal kell használnia, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="2edb8-107">To determine if you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
>
>

 <span data-ttu-id="2edb8-108">Itt azt fogja létrehozni egy webes alkalmazás OWIN beléptetni a felhasználót, a felhasználói információkat jelenítsen meg, és jelentkezzen ki az alkalmazásból a felhasználó által.</span><span class="sxs-lookup"><span data-stu-id="2edb8-108">Here we'll build an web app that uses OWIN to sign the user in, display some information about the user, and sign the user out of the app.</span></span>

## <a name="download"></a><span data-ttu-id="2edb8-109">Letöltés</span><span class="sxs-lookup"><span data-stu-id="2edb8-109">Download</span></span>
<span data-ttu-id="2edb8-110">Az oktatóanyag kódjának [karbantartása a GitHubon történik](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet).</span><span class="sxs-lookup"><span data-stu-id="2edb8-110">The code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet).</span></span>  <span data-ttu-id="2edb8-111">Követéséhez is [töltse le az alkalmazás vázát egy .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) vagy klónozza a vázat:</span><span class="sxs-lookup"><span data-stu-id="2edb8-111">To follow along, you can [download the app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) or clone the skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

<span data-ttu-id="2edb8-112">A kész alkalmazásról, valamint az oktatóanyag végén valósul meg.</span><span class="sxs-lookup"><span data-stu-id="2edb8-112">The completed app is provided at the end of this tutorial as well.</span></span>

## <a name="register-an-app"></a><span data-ttu-id="2edb8-113">Alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="2edb8-113">Register an app</span></span>
<span data-ttu-id="2edb8-114">Hozzon létre egy új alkalmazást [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy kövesse az alábbi [részletes lépéseket](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="2edb8-114">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="2edb8-115">Győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="2edb8-115">Make sure to:</span></span>

* <span data-ttu-id="2edb8-116">Másolja le a **alkalmazásazonosító** be az alkalmazáshoz hozzárendelt szüksége lehet rájuk hamarosan.</span><span class="sxs-lookup"><span data-stu-id="2edb8-116">Copy down the **Application Id** assigned to your app, you'll need it soon.</span></span>
* <span data-ttu-id="2edb8-117">Adja hozzá a **webes** platform az alkalmazásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="2edb8-117">Add the **Web** platform for your app.</span></span>
* <span data-ttu-id="2edb8-118">Adja meg a megfelelő **átirányítási URI-**.</span><span class="sxs-lookup"><span data-stu-id="2edb8-118">Enter the correct **Redirect URI**.</span></span> <span data-ttu-id="2edb8-119">Az Azure ad Szolgáltatásba, ahol hitelesítési válaszok legyenek irányítva – ehhez az oktatóanyaghoz az alapértelmezett érték jelzi az átirányítási uri `https://localhost:44326/`.</span><span class="sxs-lookup"><span data-stu-id="2edb8-119">The redirect uri indicates to Azure AD where authentication responses should be directed - the default for this tutorial is `https://localhost:44326/`.</span></span>

## <a name="install--configure-owin-authentication"></a><span data-ttu-id="2edb8-120">Telepítse és konfigurálja az OWIN hitelesítést</span><span class="sxs-lookup"><span data-stu-id="2edb8-120">Install & configure OWIN authentication</span></span>
<span data-ttu-id="2edb8-121">Itt konfigurálását végezzük el az OWIN közbenső szoftvert az OpenID Connect hitelesítési protokoll használatára.</span><span class="sxs-lookup"><span data-stu-id="2edb8-121">Here, we'll configure the OWIN middleware to use the OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="2edb8-122">OWIN be- és kijelentkezési kérések kiállítása, a felhasználói munkamenet kezelésére, és többek között a felhasználó adatai használható.</span><span class="sxs-lookup"><span data-stu-id="2edb8-122">OWIN will be used to issue sign-in and sign-out requests, manage the user's session, and get information about the user, amongst other things.</span></span>

1. <span data-ttu-id="2edb8-123">Első lépésként nyissa meg a `web.config` fájlt a projekt gyökérkönyvtárában, és adja meg az alkalmazás konfigurációs értékeit a `<appSettings>` szakasz.</span><span class="sxs-lookup"><span data-stu-id="2edb8-123">To begin, open the `web.config` file in the root of the project, and enter your app's configuration values in the `<appSettings>` section.</span></span>

  * <span data-ttu-id="2edb8-124">A `ida:ClientId` van a **alkalmazásazonosító** az alkalmazáshoz a regisztrációs portálon rendelt.</span><span class="sxs-lookup"><span data-stu-id="2edb8-124">The `ida:ClientId` is the **Application Id** assigned to your app in the registration portal.</span></span>
  * <span data-ttu-id="2edb8-125">A `ida:RedirectUri` van a **átirányítási Uri-** a portálon megadott.</span><span class="sxs-lookup"><span data-stu-id="2edb8-125">The `ida:RedirectUri` is the **Redirect Uri** you entered in the portal.</span></span>

2. <span data-ttu-id="2edb8-126">Ezt követően az OWIN köztes NuGet-csomagok hozzáadása a projekthez, a Csomagkezelő konzol használatával.</span><span class="sxs-lookup"><span data-stu-id="2edb8-126">Next, add the OWIN middleware NuGet packages to the project using the Package Manager Console.</span></span>

        ```
        PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
        PM> Install-Package Microsoft.Owin.Security.Cookies
        PM> Install-Package Microsoft.Owin.Host.SystemWeb
        ```  

3. <span data-ttu-id="2edb8-127">Adja hozzá a "OWIN indítási osztály" a projekt neve `Startup.cs` jobb kattintson a projektre--> **Hozzáadás** --> **új elem** --> "OWIN" keresése.</span><span class="sxs-lookup"><span data-stu-id="2edb8-127">Add an "OWIN Startup Class" to the project called `Startup.cs`  Right click on the project --> **Add** --> **New Item** --> Search for "OWIN".</span></span>  <span data-ttu-id="2edb8-128">Az OWIN közbenső szoftver meghívja a `Configuration(...)` metódust az alkalmazás indulásakor.</span><span class="sxs-lookup"><span data-stu-id="2edb8-128">The OWIN middleware will invoke the `Configuration(...)` method when your app starts.</span></span>
4. <span data-ttu-id="2edb8-129">Módosítsa az osztálydeklaráció való `public partial class Startup` -azt korábban már megvalósított Ez az osztály tartozik, egy másik fájlban.</span><span class="sxs-lookup"><span data-stu-id="2edb8-129">Change the class declaration to `public partial class Startup` - we've already implemented part of this class for you in another file.</span></span>  <span data-ttu-id="2edb8-130">Az a `Configuration(...)` metódus hívása ConfigureAuth(...) hitelesítés a webalkalmazás beállítása legyen</span><span class="sxs-lookup"><span data-stu-id="2edb8-130">In the `Configuration(...)` method, make a call to ConfigureAuth(...) to set up authentication for your web app</span></span>  

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

5. <span data-ttu-id="2edb8-131">Nyissa meg a fájlt `App_Start\Startup.Auth.cs` és megvalósítását a `ConfigureAuth(...)` metódust.</span><span class="sxs-lookup"><span data-stu-id="2edb8-131">Open the file `App_Start\Startup.Auth.cs` and implement the `ConfigureAuth(...)` method.</span></span>  <span data-ttu-id="2edb8-132">Megadja a paraméterek `OpenIdConnectAuthenticationOptions` koordináták az alkalmazás az Azure AD kommunikálni fog szolgálni.</span><span class="sxs-lookup"><span data-stu-id="2edb8-132">The parameters you provide in `OpenIdConnectAuthenticationOptions` will serve as coordinates for your app to communicate with Azure AD.</span></span>  <span data-ttu-id="2edb8-133">Biztosítani kell a Cookie-hitelesítés beállítása – az OpenID Connect köztes alatt a magában foglalja az cookie-kat használ.</span><span class="sxs-lookup"><span data-stu-id="2edb8-133">You'll also need to set up Cookie Authentication - the OpenID Connect middleware uses cookies underneath the covers.</span></span>

        ```C#
        public void ConfigureAuth(IAppBuilder app)
                     {
                             app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);
        
                             app.UseCookieAuthentication(new CookieAuthenticationOptions());
        
                             app.UseOpenIdConnectAuthentication(
                                     new OpenIdConnectAuthenticationOptions
                                     {
                                             // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0
                                             // The `Scope` describes the permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                                             // In a real application you could use issuer validation for additional checks, like making sure the user's organization has signed up for your app, for instance.
        
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

## <a name="send-authentication-requests"></a><span data-ttu-id="2edb8-134">Hitelesítési kérések küldése</span><span class="sxs-lookup"><span data-stu-id="2edb8-134">Send authentication requests</span></span>
<span data-ttu-id="2edb8-135">Az alkalmazás megfelelően van konfigurálva az OpenID Connect hitelesítési protokoll használatával a v2.0-végponttal való kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="2edb8-135">Your app is now properly configured to communicate with the v2.0 endpoint using the OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="2edb8-136">OWIN rendelkezik végrehajtott összes hitelesítési üzenetek létrehozásával, ellenőrzése az Azure ad-jogkivonatok és karbantartása a felhasználói munkamenet ugly részletes megvagyunk.</span><span class="sxs-lookup"><span data-stu-id="2edb8-136">OWIN has taken care of all of the ugly details of crafting authentication messages, validating tokens from Azure AD, and maintaining user session.</span></span>  <span data-ttu-id="2edb8-137">Most már kell biztosítani a felhasználóknak a be-és kijelentkezés úgy.</span><span class="sxs-lookup"><span data-stu-id="2edb8-137">All that remains is to give your users a way to sign in and sign out.</span></span>

- <span data-ttu-id="2edb8-138">Használhatja a tartományvezérlőket, hogy a felhasználó bejelentkezik egy bizonyos lap elérése előtt kötelező címkék engedélyezik.</span><span class="sxs-lookup"><span data-stu-id="2edb8-138">You can use authorize tags in your controllers to require that user signs in before accessing a certain page.</span></span>  <span data-ttu-id="2edb8-139">Nyissa meg `Controllers\HomeController.cs`, és adja hozzá a `[Authorize]` címke a jogi tudnivalók megjelenítése Névjegy vezérlőhöz.</span><span class="sxs-lookup"><span data-stu-id="2edb8-139">Open `Controllers\HomeController.cs`, and add the `[Authorize]` tag to the About controller.</span></span>
        
        ```C#
        [Authorize]
        public ActionResult About()
        {
          ...
        ```

- <span data-ttu-id="2edb8-140">OWIN segítségével közvetlenül kiadni a kód a érkező hitelesítési kéréseket.</span><span class="sxs-lookup"><span data-stu-id="2edb8-140">You can also use OWIN to directly issue authentication requests from within your code.</span></span>  <span data-ttu-id="2edb8-141">Nyissa meg `Controllers\AccountController.cs`.</span><span class="sxs-lookup"><span data-stu-id="2edb8-141">Open `Controllers\AccountController.cs`.</span></span>  <span data-ttu-id="2edb8-142">A SignIn() és SignOut() műveletek adja a challenge OpenID Connect és kijelentkezési kérések ki.</span><span class="sxs-lookup"><span data-stu-id="2edb8-142">In the SignIn() and SignOut() actions, issue OpenID Connect challenge and sign-out requests, respectively.</span></span>

        ```C#
        public void SignIn()
        {
            // Send an OpenID Connect sign-in request.
            if (!Request.IsAuthenticated)
            {
                HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
            }
        }
        
        // BUGBUG: Ending a session with the v2.0 endpoint is not yet supported.  Here, we just end the session with the web app.  
        public void SignOut()
        {
            // Send an OpenID Connect sign-out request.
            HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
            Response.Redirect("/");
        }
        ```

- <span data-ttu-id="2edb8-143">Most, nyissa meg a `Views\Shared\_LoginPartial.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="2edb8-143">Now, open `Views\Shared\_LoginPartial.cshtml`.</span></span>  <span data-ttu-id="2edb8-144">Ez az amelyen megjelenítése a felhasználónak az alkalmazás be- és kijelentkezési hivatkozások, és nyomtassa ki a nézetben a felhasználó nevét.</span><span class="sxs-lookup"><span data-stu-id="2edb8-144">This is where you'll show the user your app's sign-in and sign-out links, and print out the user's name in a view.</span></span>

        ```HTML
        @if (Request.IsAuthenticated)
        {
            <text>
                <ul class="nav navbar-nav navbar-right">
                    <li class="navbar-text">
        
                        @*The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves.*@
        
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

## <a name="display-user-information"></a><span data-ttu-id="2edb8-145">Felhasználói adatok megjelenítése</span><span class="sxs-lookup"><span data-stu-id="2edb8-145">Display user information</span></span>
<span data-ttu-id="2edb8-146">Az OpenID Connect felhasználók hitelesítésekor a v2.0-végpontra egy id_token az alkalmazást, a jogcímeket vagy helyességi feltételek a felhasználóról tartalmaz adja vissza.</span><span class="sxs-lookup"><span data-stu-id="2edb8-146">When authenticating users with OpenID Connect, the v2.0 endpoint returns an id_token to the app that contains claims, or assertions about the user.</span></span>  <span data-ttu-id="2edb8-147">Ezeket a jogcímeket segítségével szabja személyre az alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="2edb8-147">You can use these claims to personalize your app:</span></span>

- <span data-ttu-id="2edb8-148">Nyissa meg az `Controllers\HomeController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="2edb8-148">Open the `Controllers\HomeController.cs` file.</span></span>  <span data-ttu-id="2edb8-149">A felhasználói jogcímek, a vezérlők keresztül érheti el a `ClaimsPrincipal.Current` rendszerbiztonsági objektumot.</span><span class="sxs-lookup"><span data-stu-id="2edb8-149">You can access the user's claims in your controllers via the `ClaimsPrincipal.Current` security principal object.</span></span>

        ```C#
        [Authorize]
        public ActionResult About()
        {
            ViewBag.Name = ClaimsPrincipal.Current.FindFirst("name").Value;
        
            // The object ID claim will only be emitted for work or school accounts at this time.
            Claim oid = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier");
            ViewBag.ObjectId = oid == null ? string.Empty : oid.Value;
        
            // The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves
            ViewBag.Username = ClaimsPrincipal.Current.FindFirst("preferred_username").Value;
        
            // The subject or nameidentifier claim can be used to uniquely identify the user
            ViewBag.Subject = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        
            return View();
        }
        ```

## <a name="run"></a><span data-ttu-id="2edb8-150">Futtassa a következőt:</span><span class="sxs-lookup"><span data-stu-id="2edb8-150">Run</span></span>
<span data-ttu-id="2edb8-151">Végül felépítéséhez és az alkalmazás futtatása!</span><span class="sxs-lookup"><span data-stu-id="2edb8-151">Finally, build and run your app!</span></span>   <span data-ttu-id="2edb8-152">Jelentkezzen be személyes Microsoft-Account vagy a munkahelyi vagy iskolai fiókkal, és figyelje meg, hogyan jelenik meg a felső navigációs sáv a felhasználó identitását.</span><span class="sxs-lookup"><span data-stu-id="2edb8-152">Sign in with either a personal Microsoft Account or a work or school account, and notice how the user's identity is reflected in the top navigation bar.</span></span>  <span data-ttu-id="2edb8-153">Most már rendelkezik egy webalkalmazást, amely képes hitelesíteni a személyes és munkahelyi vagy iskolai fiókkal rendelkező felhasználók iparági szabványos protokollok használatával biztonságossá.</span><span class="sxs-lookup"><span data-stu-id="2edb8-153">You now have a web app secured using industry standard protocols that can authenticate users with both their personal and work/school accounts.</span></span>

<span data-ttu-id="2edb8-154">Az elkészült mintát (a konfigurációs értékek nélkül) referenciaként [is letöltheti a .zip Itt](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip), vagy a Githubból is klónozhatja:</span><span class="sxs-lookup"><span data-stu-id="2edb8-154">For reference, the completed sample (without your configuration values) [is provided as a .zip here](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip), or you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

## <a name="next-steps"></a><span data-ttu-id="2edb8-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2edb8-155">Next steps</span></span>
<span data-ttu-id="2edb8-156">Ön most már továbbléphet az összetettebb témákra.</span><span class="sxs-lookup"><span data-stu-id="2edb8-156">You can now move onto more advanced topics.</span></span>  <span data-ttu-id="2edb8-157">Előfordulhat, hogy ki szeretné próbálni:</span><span class="sxs-lookup"><span data-stu-id="2edb8-157">You may want to try:</span></span>

[<span data-ttu-id="2edb8-158">A Web API-t biztonságossá a a v2.0-végpontra >></span><span class="sxs-lookup"><span data-stu-id="2edb8-158">Secure a Web API with the the v2.0 endpoint >></span></span>](active-directory-devquickstarts-webapi-dotnet.md)

<span data-ttu-id="2edb8-159">További forrásokért tekintse meg:</span><span class="sxs-lookup"><span data-stu-id="2edb8-159">For additional resources, check out:</span></span>

* [<span data-ttu-id="2edb8-160">A v2.0 – útmutató fejlesztőknek >></span><span class="sxs-lookup"><span data-stu-id="2edb8-160">The v2.0 developer guide >></span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="2edb8-161">StackOverflow "azure-active-directory" címke >></span><span class="sxs-lookup"><span data-stu-id="2edb8-161">StackOverflow "azure-active-directory" tag >></span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="2edb8-162">Biztonsági frissítések termékeinkhez</span><span class="sxs-lookup"><span data-stu-id="2edb8-162">Get security updates for our products</span></span>
<span data-ttu-id="2edb8-163">Javasoljuk, hogy kérjen értesítést a bekövetkező biztonsági incidensekről. Látogasson el [erre a lapra](https://technet.microsoft.com/security/dd252948), és fizessen elő a biztonsági tanácsadói riasztásokra.</span><span class="sxs-lookup"><span data-stu-id="2edb8-163">We encourage you to get notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing to Security Advisory Alerts.</span></span>
