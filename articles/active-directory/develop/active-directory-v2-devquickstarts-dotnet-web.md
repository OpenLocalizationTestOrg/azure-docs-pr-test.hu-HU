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
# <a name="add-sign-in-tooan-net-mvc-web-app"></a><span data-ttu-id="b1ec2-103">Bejelentkezési tooan .NET MVC webalkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b1ec2-103">Add sign-in tooan .NET MVC web app</span></span>
<span data-ttu-id="b1ec2-104">Hello v2.0-végponttal gyorsan hitelesítési tooyour webalkalmazások támogatja a személyes Microsoft-fiókot is, valamint a munkahelyi vagy iskolai fiókokat is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-104">With hello v2.0 endpoint, you can quickly add authentication tooyour web apps with support for both personal Microsoft accounts and work or school accounts.</span></span>  <span data-ttu-id="b1ec2-105">Az ASP.NET web apps Ez elvégezhető a .NET-keretrendszer 4.5 része a Microsoft OWIN köztes használatával.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-105">In ASP.NET web apps, you can accomplish this using Microsoft's OWIN middleware included in .NET Framework 4.5.</span></span>

> [!NOTE]
> <span data-ttu-id="b1ec2-106">Nem minden Azure Active Directory forgatókönyvek és funkciók támogatják hello v2.0-végponttól.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-106">Not all Azure Active Directory scenarios & features are supported by hello v2.0 endpoint.</span></span>  <span data-ttu-id="b1ec2-107">toodetermine használatát hello v2.0-végpontra, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="b1ec2-107">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
>
>

 <span data-ttu-id="b1ec2-108">Itt azt fogja hozza létre egy webes alkalmazás megjelenítési az OWIN toosign hello felhasználói használó hello felhasználói adatait, és bejelentkezési hello felhasználói hello alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-108">Here we'll build an web app that uses OWIN toosign hello user in, display some information about hello user, and sign hello user out of hello app.</span></span>

## <a name="download"></a><span data-ttu-id="b1ec2-109">Letöltés</span><span class="sxs-lookup"><span data-stu-id="b1ec2-109">Download</span></span>
<span data-ttu-id="b1ec2-110">az oktatóanyag kódjának hello kezelt [a Githubon](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet).</span><span class="sxs-lookup"><span data-stu-id="b1ec2-110">hello code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet).</span></span>  <span data-ttu-id="b1ec2-111">toofollow mellett, akkor [töltse le a .zip hello alkalmazás vázát](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) vagy a Klónozás hello vázat:</span><span class="sxs-lookup"><span data-stu-id="b1ec2-111">toofollow along, you can [download hello app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) or clone hello skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

<span data-ttu-id="b1ec2-112">hello végén, valamint az oktatóanyag befejezése hello app valósul meg.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-112">hello completed app is provided at hello end of this tutorial as well.</span></span>

## <a name="register-an-app"></a><span data-ttu-id="b1ec2-113">Alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="b1ec2-113">Register an app</span></span>
<span data-ttu-id="b1ec2-114">Hozzon létre egy új alkalmazást [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy kövesse az alábbi [részletes lépéseket](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="b1ec2-114">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="b1ec2-115">Győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="b1ec2-115">Make sure to:</span></span>

* <span data-ttu-id="b1ec2-116">Másolja le hello **alkalmazásazonosító** tooyour app hozzárendelve, szüksége lehet rájuk hamarosan.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-116">Copy down hello **Application Id** assigned tooyour app, you'll need it soon.</span></span>
* <span data-ttu-id="b1ec2-117">Adja hozzá a hello **webes** platform az alkalmazásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-117">Add hello **Web** platform for your app.</span></span>
* <span data-ttu-id="b1ec2-118">Adja meg a megfelelő hello **átirányítási URI-**.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-118">Enter hello correct **Redirect URI**.</span></span> <span data-ttu-id="b1ec2-119">hello átirányítási uri azt jelzi, ha hitelesítési válaszok legyenek irányítva – hello ebben az oktatóanyagban alapértelmezés szerint AD tooAzure `https://localhost:44326/`.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-119">hello redirect uri indicates tooAzure AD where authentication responses should be directed - hello default for this tutorial is `https://localhost:44326/`.</span></span>

## <a name="install--configure-owin-authentication"></a><span data-ttu-id="b1ec2-120">Telepítse és konfigurálja az OWIN hitelesítést</span><span class="sxs-lookup"><span data-stu-id="b1ec2-120">Install & configure OWIN authentication</span></span>
<span data-ttu-id="b1ec2-121">Itt konfigurálását végezzük el hello OWIN köztes toouse hello OpenID Connect hitelesítési protokoll.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-121">Here, we'll configure hello OWIN middleware toouse hello OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="b1ec2-122">OWIN kell használt tooissue be- és kijelentkezési kérések, hello felhasználói munkamenet kezeli, és többek között a hello felhasználó adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-122">OWIN will be used tooissue sign-in and sign-out requests, manage hello user's session, and get information about hello user, amongst other things.</span></span>

1. <span data-ttu-id="b1ec2-123">toobegin, nyissa meg hello `web.config` hello projekt gyökerében hello fájlt, és adja meg az alkalmazás konfigurációs értékeit hello `<appSettings>` szakasz.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-123">toobegin, open hello `web.config` file in hello root of hello project, and enter your app's configuration values in hello `<appSettings>` section.</span></span>

  * <span data-ttu-id="b1ec2-124">Hello `ida:ClientId` hello van **alkalmazásazonosító** tooyour app hello regisztrációs portálon rendelt.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-124">hello `ida:ClientId` is hello **Application Id** assigned tooyour app in hello registration portal.</span></span>
  * <span data-ttu-id="b1ec2-125">Hello `ida:RedirectUri` hello van **átirányítási Uri-** hello portálon megadott.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-125">hello `ida:RedirectUri` is hello **Redirect Uri** you entered in hello portal.</span></span>

2. <span data-ttu-id="b1ec2-126">Ezután adja hozzá a hello OWIN köztes NuGet csomagjainak toohello projekt hello Csomagkezelő konzol használatával.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-126">Next, add hello OWIN middleware NuGet packages toohello project using hello Package Manager Console.</span></span>

        ```
        PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
        PM> Install-Package Microsoft.Owin.Security.Cookies
        PM> Install-Package Microsoft.Owin.Host.SystemWeb
        ```  

3. <span data-ttu-id="b1ec2-127">Az "OWIN indítási osztály" toohello projektet nevezik Hozzáadás `Startup.cs` jobb hello projektben kattintson--> **Hozzáadás** --> **új elem** --> "OWIN" keresése.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-127">Add an "OWIN Startup Class" toohello project called `Startup.cs`  Right click on hello project --> **Add** --> **New Item** --> Search for "OWIN".</span></span>  <span data-ttu-id="b1ec2-128">hello OWIN köztes által aktivált hello `Configuration(...)` módszer az alkalmazás indításakor.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-128">hello OWIN middleware will invoke hello `Configuration(...)` method when your app starts.</span></span>
4. <span data-ttu-id="b1ec2-129">Hello osztálydeklaráció túl módosítása`public partial class Startup` -azt korábban már megvalósított Ez az osztály tartozik, egy másik fájlban.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-129">Change hello class declaration too`public partial class Startup` - we've already implemented part of this class for you in another file.</span></span>  <span data-ttu-id="b1ec2-130">A hello `Configuration(...)` módszert, győződjön meg a hívás tooConfigureAuth(...) tooset hitelesítés a webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="b1ec2-130">In hello `Configuration(...)` method, make a call tooConfigureAuth(...) tooset up authentication for your web app</span></span>  

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

5. <span data-ttu-id="b1ec2-131">Nyissa meg hello fájl `App_Start\Startup.Auth.cs` és valósíthatnak meg hello `ConfigureAuth(...)` metódust.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-131">Open hello file `App_Start\Startup.Auth.cs` and implement hello `ConfigureAuth(...)` method.</span></span>  <span data-ttu-id="b1ec2-132">Megadja a paraméterek hello `OpenIdConnectAuthenticationOptions` erre a célra az Azure ad alkalmazás toocommunicate koordinátáit.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-132">hello parameters you provide in `OpenIdConnectAuthenticationOptions` will serve as coordinates for your app toocommunicate with Azure AD.</span></span>  <span data-ttu-id="b1ec2-133">Azt is el kell készíteni, hitelesítési cookie-k tooset – hello OpenID Connect köztes hello magában foglalja az alatta lévő cookie-kat használ.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-133">You'll also need tooset up Cookie Authentication - hello OpenID Connect middleware uses cookies underneath hello covers.</span></span>

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

## <a name="send-authentication-requests"></a><span data-ttu-id="b1ec2-134">Hitelesítési kérések küldése</span><span class="sxs-lookup"><span data-stu-id="b1ec2-134">Send authentication requests</span></span>
<span data-ttu-id="b1ec2-135">Az alkalmazás már megfelelően konfigurált toocommunicate hello v2.0-végponttal hello OpenID Connect hitelesítési protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-135">Your app is now properly configured toocommunicate with hello v2.0 endpoint using hello OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="b1ec2-136">OWIN rendelkezik végrehajtott összes hitelesítési üzenetek létrehozásával, ellenőrzése az Azure ad-jogkivonatok és karbantartásáról a felhasználói munkamenet ugly részleteit hello megvagyunk.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-136">OWIN has taken care of all of hello ugly details of crafting authentication messages, validating tokens from Azure AD, and maintaining user session.</span></span>  <span data-ttu-id="b1ec2-137">Hogy továbbra is van toogive a felhasználók egy a módon toosign és kijelentkezett.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-137">All that remains is toogive your users a way toosign in and sign out.</span></span>

- <span data-ttu-id="b1ec2-138">Használható címkék engedélyezik a tartományvezérlők toorequire az, hogy a felhasználó jelentkezik be egy bizonyos lap elérése előtt.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-138">You can use authorize tags in your controllers toorequire that user signs in before accessing a certain page.</span></span>  <span data-ttu-id="b1ec2-139">Nyissa meg `Controllers\HomeController.cs`, és adja hozzá a hello `[Authorize]` toohello vezérlő vonatkozó címke.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-139">Open `Controllers\HomeController.cs`, and add hello `[Authorize]` tag toohello About controller.</span></span>
        
        ```C#
        [Authorize]
        public ActionResult About()
        {
          ...
        ```

- <span data-ttu-id="b1ec2-140">OWIN toodirectly probléma a érkező hitelesítési kéréseket a kód is használható.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-140">You can also use OWIN toodirectly issue authentication requests from within your code.</span></span>  <span data-ttu-id="b1ec2-141">Nyissa meg `Controllers\AccountController.cs`.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-141">Open `Controllers\AccountController.cs`.</span></span>  <span data-ttu-id="b1ec2-142">Hello SignIn() és SignOut() műveleteket adja a challenge OpenID Connect és kijelentkezési kérések ki.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-142">In hello SignIn() and SignOut() actions, issue OpenID Connect challenge and sign-out requests, respectively.</span></span>

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

- <span data-ttu-id="b1ec2-143">Most, nyissa meg a `Views\Shared\_LoginPartial.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-143">Now, open `Views\Shared\_LoginPartial.cshtml`.</span></span>  <span data-ttu-id="b1ec2-144">Ez az amelyen hello felhasználói megjelenítése az alkalmazáshoz be- és kijelentkezési hivatkozásokat, és nyomtassa ki hello felhasználónév nézetben.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-144">This is where you'll show hello user your app's sign-in and sign-out links, and print out hello user's name in a view.</span></span>

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

## <a name="display-user-information"></a><span data-ttu-id="b1ec2-145">Felhasználói adatok megjelenítése</span><span class="sxs-lookup"><span data-stu-id="b1ec2-145">Display user information</span></span>
<span data-ttu-id="b1ec2-146">Az OpenID Connect felhasználók hitelesítésekor hello v2.0-végponttal, amely tartalmazza a jogcímeket vagy helyességi feltételek hello felhasználóról id_token toohello alkalmazás adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-146">When authenticating users with OpenID Connect, hello v2.0 endpoint returns an id_token toohello app that contains claims, or assertions about hello user.</span></span>  <span data-ttu-id="b1ec2-147">A jogcímek toopersonalize az alkalmazás használhatja:</span><span class="sxs-lookup"><span data-stu-id="b1ec2-147">You can use these claims toopersonalize your app:</span></span>

- <span data-ttu-id="b1ec2-148">Nyissa meg hello `Controllers\HomeController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-148">Open hello `Controllers\HomeController.cs` file.</span></span>  <span data-ttu-id="b1ec2-149">A tartományvezérlők keresztül hello keresztül elérhető hello felhasználói jogcímek `ClaimsPrincipal.Current` rendszerbiztonsági objektumot.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-149">You can access hello user's claims in your controllers via hello `ClaimsPrincipal.Current` security principal object.</span></span>

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

## <a name="run"></a><span data-ttu-id="b1ec2-150">Futtassa a következőt:</span><span class="sxs-lookup"><span data-stu-id="b1ec2-150">Run</span></span>
<span data-ttu-id="b1ec2-151">Végül felépítéséhez és az alkalmazás futtatása!</span><span class="sxs-lookup"><span data-stu-id="b1ec2-151">Finally, build and run your app!</span></span>   <span data-ttu-id="b1ec2-152">Jelentkezzen be személyes Microsoft-Account vagy a munkahelyi vagy iskolai fiókkal, és figyelje meg, hogyan hello felhasználói identitás hello felső navigációs sáv megjelenik.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-152">Sign in with either a personal Microsoft Account or a work or school account, and notice how hello user's identity is reflected in hello top navigation bar.</span></span>  <span data-ttu-id="b1ec2-153">Most már rendelkezik egy webalkalmazást, amely képes hitelesíteni a személyes és munkahelyi vagy iskolai fiókkal rendelkező felhasználók iparági szabványos protokollok használatával biztonságossá.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-153">You now have a web app secured using industry standard protocols that can authenticate users with both their personal and work/school accounts.</span></span>

<span data-ttu-id="b1ec2-154">Referenciaként hello befejeződött (a konfigurációs értékek nélkül) minta [is letöltheti a .zip Itt](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip), vagy a Githubból is klónozhatja:</span><span class="sxs-lookup"><span data-stu-id="b1ec2-154">For reference, hello completed sample (without your configuration values) [is provided as a .zip here](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip), or you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

## <a name="next-steps"></a><span data-ttu-id="b1ec2-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b1ec2-155">Next steps</span></span>
<span data-ttu-id="b1ec2-156">Ön most már továbbléphet az összetettebb témákra.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-156">You can now move onto more advanced topics.</span></span>  <span data-ttu-id="b1ec2-157">Érdemes lehet tootry:</span><span class="sxs-lookup"><span data-stu-id="b1ec2-157">You may want tootry:</span></span>

[<span data-ttu-id="b1ec2-158">A Web API-t hello hello v2.0-végponttól biztonságos >></span><span class="sxs-lookup"><span data-stu-id="b1ec2-158">Secure a Web API with hello hello v2.0 endpoint >></span></span>](active-directory-devquickstarts-webapi-dotnet.md)

<span data-ttu-id="b1ec2-159">További forrásokért tekintse meg:</span><span class="sxs-lookup"><span data-stu-id="b1ec2-159">For additional resources, check out:</span></span>

* [<span data-ttu-id="b1ec2-160">hello v2.0 – útmutató fejlesztőknek >></span><span class="sxs-lookup"><span data-stu-id="b1ec2-160">hello v2.0 developer guide >></span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="b1ec2-161">StackOverflow "azure-active-directory" címke >></span><span class="sxs-lookup"><span data-stu-id="b1ec2-161">StackOverflow "azure-active-directory" tag >></span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="b1ec2-162">Biztonsági frissítések termékeinkhez</span><span class="sxs-lookup"><span data-stu-id="b1ec2-162">Get security updates for our products</span></span>
<span data-ttu-id="b1ec2-163">Javasoljuk, tooget értesítést a bekövetkező biztonsági incidensekről ellátogatva [ezen a lapon](https://technet.microsoft.com/security/dd252948) és előfizetés tooSecurity tanácsadói riasztásokra.</span><span class="sxs-lookup"><span data-stu-id="b1ec2-163">We encourage you tooget notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing tooSecurity Advisory Alerts.</span></span>
