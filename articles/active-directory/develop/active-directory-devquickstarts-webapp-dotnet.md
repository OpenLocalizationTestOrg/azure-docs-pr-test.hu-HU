---
title: "aaaAzure AD .NET web app bevezetés |} Microsoft Docs"
description: "A .NET MVC webes alkalmazás, amely az Azure AD-bejelentkezés létrehozása."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: e15a41a4-dc5d-4c90-b3fe-5dc33b9a1e96
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 6d3098c9e3d7e1916ccb110c703f501ae52e788f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="aspnet-web-app-sign-in-and-sign-out-with-azure-ad"></a><span data-ttu-id="ac885-103">ASP.NET webalkalmazás be- és kijelentkezési az Azure ad szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="ac885-103">ASP.NET web app sign-in and sign-out with Azure AD</span></span>
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="ac885-104">Be- és kijelentkezési egyetlen azáltal csak néhány sornyi kódot, Azure Active Directory (Azure AD) egyszerűen meg toooutsource webalkalmazás identitáskezeléshez.</span><span class="sxs-lookup"><span data-stu-id="ac885-104">By providing a single sign-in and sign-out with only a few lines of code, Azure Active Directory (Azure AD) makes it simple for you toooutsource web-app identity management.</span></span> <span data-ttu-id="ac885-105">Regisztrálhat a felhasználók ASP.NET webes alkalmazások mindkét hello Microsoft általi implementációja Open Web Interface .NET (OWIN) köztes használatával.</span><span class="sxs-lookup"><span data-stu-id="ac885-105">You can sign users in and out of ASP.NET web apps by using hello Microsoft implementation of Open Web Interface for .NET (OWIN) middleware.</span></span> <span data-ttu-id="ac885-106">Közösségi szerkesztésű OWIN köztes a .NET-keretrendszer 4.5 része.</span><span class="sxs-lookup"><span data-stu-id="ac885-106">Community-driven OWIN middleware is included in .NET Framework 4.5.</span></span> <span data-ttu-id="ac885-107">Ez a cikk bemutatja, hogyan toouse OWIN számára:</span><span class="sxs-lookup"><span data-stu-id="ac885-107">This article shows how toouse OWIN to:</span></span>

* <span data-ttu-id="ac885-108">A felhasználók beléptetése tooweb alkalmazásokban az Azure AD hello identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="ac885-108">Sign users in tooweb apps by using Azure AD as hello identity provider.</span></span>
* <span data-ttu-id="ac885-109">Bizonyos felhasználói információk jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="ac885-109">Display some user information.</span></span>
* <span data-ttu-id="ac885-110">A felhasználók hello alkalmazások kívül.</span><span class="sxs-lookup"><span data-stu-id="ac885-110">Sign users out of hello apps.</span></span>

## <a name="before-you-get-started"></a><span data-ttu-id="ac885-111">A kezdés előtt</span><span class="sxs-lookup"><span data-stu-id="ac885-111">Before you get started</span></span>
* <span data-ttu-id="ac885-112">Töltse le a hello [app vázat](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) vagy töltse le a hello [elkészült mintát](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="ac885-112">Download hello [app skeleton](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) or download hello [completed sample](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).</span></span>
* <span data-ttu-id="ac885-113">Meg kell az Azure AD-bérlő mely tooregister hello alkalmazásban is.</span><span class="sxs-lookup"><span data-stu-id="ac885-113">You also need an Azure AD tenant in which tooregister hello app.</span></span> <span data-ttu-id="ac885-114">Ha még nem telepítette az Azure AD-bérlő [megtudhatja, hogyan egy tooget](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="ac885-114">If you don't already have an Azure AD tenant, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

<span data-ttu-id="ac885-115">Ha készen áll, hello eljárásokat hajtsa végre a következő négy részből áll hello.</span><span class="sxs-lookup"><span data-stu-id="ac885-115">When you are ready, follow hello procedures in hello next four sections.</span></span>

## <a name="step-1-register-hello-new-app-with-azure-ad"></a><span data-ttu-id="ac885-116">1. lépés: Hello új alkalmazás regisztrálása az Azure ad szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="ac885-116">Step 1: Register hello new app with Azure AD</span></span>
<span data-ttu-id="ac885-117">tooset hello app tooauthenticate felhasználókat, először regisztrálja az Ön bérelt szolgáltatásának hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="ac885-117">tooset up hello app tooauthenticate users, first register it in your tenant by doing hello following:</span></span>

1. <span data-ttu-id="ac885-118">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ac885-118">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ac885-119">Hello felső sávon kattintson a fiók nevére.</span><span class="sxs-lookup"><span data-stu-id="ac885-119">On hello top bar, click your account name.</span></span> <span data-ttu-id="ac885-120">A hello **Directory** listán, válassza hello tooregister hello alkalmazás kívánt Active Directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="ac885-120">Under hello **Directory** list, select hello Active Directory tenant where you want tooregister hello app.</span></span>
3. <span data-ttu-id="ac885-121">Kattintson a **több szolgáltatások** hello bal oldali ablaktáblán, és válassza ki **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ac885-121">Click **More Services** in hello left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="ac885-122">Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="ac885-122">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="ac885-123">Hajtsa végre a hello kérni fogja az új toocreate **webalkalmazás és/vagy WebAPI**.</span><span class="sxs-lookup"><span data-stu-id="ac885-123">Follow hello prompts toocreate a new **Web Application and/or WebAPI**.</span></span>
  * <span data-ttu-id="ac885-124">**Név** hello app toousers ismerteti.</span><span class="sxs-lookup"><span data-stu-id="ac885-124">**Name** describes hello app toousers.</span></span>
  * <span data-ttu-id="ac885-125">**Bejelentkezési URL-cím** hello alap URL-cím hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ac885-125">**Sign-On URL** is hello base URL of hello app.</span></span> <span data-ttu-id="ac885-126">hello vázat alapértelmezett URL-címe: https://localhost:44320 /.</span><span class="sxs-lookup"><span data-stu-id="ac885-126">hello skeleton's default URL is https://localhost:44320/.</span></span>
6. <span data-ttu-id="ac885-127">Hello regisztráció befejezését követően az Azure AD hozzárendel hello app egy egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="ac885-127">After you've completed hello registration, Azure AD assigns hello app a unique application ID.</span></span> <span data-ttu-id="ac885-128">Másolja ki hello app lap toouse hello következő szakaszokban lévő hello érték.</span><span class="sxs-lookup"><span data-stu-id="ac885-128">Copy hello value from hello app page toouse in hello next sections.</span></span>
7. <span data-ttu-id="ac885-129">A hello **beállítások** -> **tulajdonságok** az alkalmazás lapján hello App ID URI frissítése.</span><span class="sxs-lookup"><span data-stu-id="ac885-129">From hello **Settings** -> **Properties** page for your application, update hello App ID URI.</span></span> <span data-ttu-id="ac885-130">Hello **App ID URI** hello alkalmazás egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="ac885-130">hello **App ID URI** is a unique identifier for hello app.</span></span> <span data-ttu-id="ac885-131">hello elnevezési konvenció: `https://<tenant-domain>/<app-name>` (például `https://contoso.onmicrosoft.com/my-first-aad-app`).</span><span class="sxs-lookup"><span data-stu-id="ac885-131">hello naming convention is `https://<tenant-domain>/<app-name>` (for example, `https://contoso.onmicrosoft.com/my-first-aad-app`).</span></span>

## <a name="step-2-set-up-hello-app-toouse-hello-owin-authentication-pipeline"></a><span data-ttu-id="ac885-132">2. lépés: Hello app toouse hello OWIN hitelesítési folyamatot beállítása</span><span class="sxs-lookup"><span data-stu-id="ac885-132">Step 2: Set up hello app toouse hello OWIN authentication pipeline</span></span>
<span data-ttu-id="ac885-133">Ez a lépés konfigurál hello OWIN köztes toouse hello OpenID Connect hitelesítési protokoll.</span><span class="sxs-lookup"><span data-stu-id="ac885-133">In this step, you configure hello OWIN middleware toouse hello OpenID Connect authentication protocol.</span></span> <span data-ttu-id="ac885-134">Használjon OWIN tooissue be- és kijelentkezési kérések, kezelni a felhasználói munkameneteket, felhasználói adatok beolvasása, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="ac885-134">You use OWIN tooissue sign-in and sign-out requests, manage user sessions, get user information, and so forth.</span></span>

1. <span data-ttu-id="ac885-135">toobegin, hello OWIN köztes NuGet csomagjainak toohello projekt hozzáadása hello Csomagkezelő konzol használatával.</span><span class="sxs-lookup"><span data-stu-id="ac885-135">toobegin, add hello OWIN middleware NuGet packages toohello project by using hello Package Manager Console.</span></span>

     ```
     PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
     PM> Install-Package Microsoft.Owin.Security.Cookies
     PM> Install-Package Microsoft.Owin.Host.SystemWeb
     ```

2. <span data-ttu-id="ac885-136">az OWIN indítási osztály toohello projektet nevezik tooadd `Startup.cs`, kattintson a jobb gombbal a projekt hello, válassza ki **Hozzáadás**, jelölje be **új elem**, majd keresse meg a **OWIN**.</span><span class="sxs-lookup"><span data-stu-id="ac885-136">tooadd an OWIN Startup class toohello project called `Startup.cs`, right-click hello project, select **Add**, select **New Item**, and then search for **OWIN**.</span></span> <span data-ttu-id="ac885-137">hello OWIN köztes hív meg hello **Configuration(...)**  metódus hello alkalmazás indításakor.</span><span class="sxs-lookup"><span data-stu-id="ac885-137">hello OWIN middleware invokes hello **Configuration(...)** method when hello app starts.</span></span>
3. <span data-ttu-id="ac885-138">Hello osztálydeklaráció túl módosítása`public partial class Startup`.</span><span class="sxs-lookup"><span data-stu-id="ac885-138">Change hello class declaration too`public partial class Startup`.</span></span> <span data-ttu-id="ac885-139">Azt korábban már megvalósított Ez az osztály tartozik, egy másik fájlban.</span><span class="sxs-lookup"><span data-stu-id="ac885-139">We've already implemented part of this class for you in another file.</span></span> <span data-ttu-id="ac885-140">A hello **Configuration(...)**  módszer, túl hívás**ConfgureAuth(...)**  tooset hello alkalmazás hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="ac885-140">In hello **Configuration(...)** method, make a call too**ConfgureAuth(...)** tooset up authentication for hello app.</span></span>  

     ```C#
     public partial class Startup
     {
         public void Configuration(IAppBuilder app)
         {
             ConfigureAuth(app);
         }
     }
     ```

4. <span data-ttu-id="ac885-141">Nyissa meg a hello App_Start\Startup.Auth.cs fájlt, és megvalósításához hello **ConfigureAuth(...)**  metódust.</span><span class="sxs-lookup"><span data-stu-id="ac885-141">Open hello App_Start\Startup.Auth.cs file, and then implement hello **ConfigureAuth(...)** method.</span></span> <span data-ttu-id="ac885-142">Megadja a paraméterek hello *OpenIDConnectAuthenticationOptions* hello app toocommunicate az Azure AD-koordináták szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="ac885-142">hello parameters you provide in *OpenIDConnectAuthenticationOptions* serve as coordinates for hello app toocommunicate with Azure AD.</span></span> <span data-ttu-id="ac885-143">Szükség tooset cookie-k hitelesítés, mert hello OpenID Connect köztes hello háttérben cookie-kat használ.</span><span class="sxs-lookup"><span data-stu-id="ac885-143">You also need tooset up cookie authentication, because hello OpenID Connect middleware uses cookies in hello background.</span></span>

     ```C#
     public void ConfigureAuth(IAppBuilder app)
     {
         app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

         app.UseCookieAuthentication(new CookieAuthenticationOptions());

         app.UseOpenIdConnectAuthentication(
             new OpenIdConnectAuthenticationOptions
             {
                 ClientId = clientId,
                 Authority = authority,
                 PostLogoutRedirectUri = postLogoutRedirectUri,
                 Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = context =>
                        {
                            context.HandleResponse();
                            context.Response.Redirect("/Error?message=" + context.Exception.Message);
                            return Task.FromResult(0);
                        }
                    }
             });
     }
     ```

5. <span data-ttu-id="ac885-144">Nyissa meg hello web.config fájl hello hello projekt gyökérkönyvtárában, és adja meg az hello konfigurációs értékeket hello `<appSettings>` szakasz.</span><span class="sxs-lookup"><span data-stu-id="ac885-144">Open hello web.config file in hello root of hello project, and then enter hello configuration values in hello `<appSettings>` section.</span></span>
  * <span data-ttu-id="ac885-145">`ida:ClientId`: hello GUID, átmásolva hello Azure portálra az "1. lépés: Register hello új alkalmazást az Azure ad szolgáltatással."</span><span class="sxs-lookup"><span data-stu-id="ac885-145">`ida:ClientId`: hello GUID you copied from hello Azure portal in "Step 1: Register hello new app with Azure AD."</span></span>
  * <span data-ttu-id="ac885-146">`ida:Tenant`: az Azure AD-bérlő (például contoso.onmicrosoft.com) hello nevét.</span><span class="sxs-lookup"><span data-stu-id="ac885-146">`ida:Tenant`: hello name of your Azure AD tenant (for example, contoso.onmicrosoft.com).</span></span>
  * <span data-ttu-id="ac885-147">`ida:PostLogoutRedirectUri`: hello kijelző, amely közli az Azure AD, ha a rendszer átirányítja az a felhasználó a kijelentkezési kérés sikeres végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="ac885-147">`ida:PostLogoutRedirectUri`: hello indicator that tells Azure AD where a user should be redirected after a sign-out request is successfully completed.</span></span>

## <a name="step-3-use-owin-tooissue-sign-in-and-sign-out-requests-tooazure-ad"></a><span data-ttu-id="ac885-148">: 3. lépés OWIN tooissue be- és kijelentkezési kérések tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="ac885-148">Step 3: Use OWIN tooissue sign-in and sign-out requests tooAzure AD</span></span>
<span data-ttu-id="ac885-149">hello alkalmazás mostantól az Azure ad-val megfelelően konfigurált toocommunicate hello OpenID Connect hitelesítési protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="ac885-149">hello app is now properly configured toocommunicate with Azure AD by using hello OpenID Connect authentication protocol.</span></span> <span data-ttu-id="ac885-150">OWIN kezelt összes hitelesítési üzenetek létrehozásával, ellenőrzése az Azure ad-jogkivonatok és karbantartásáról a felhasználói munkamenetek hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="ac885-150">OWIN has handled all of hello details of crafting authentication messages, validating tokens from Azure AD, and maintaining user sessions.</span></span> <span data-ttu-id="ac885-151">Hogy továbbra is van toogive a felhasználók egy a módon toosign és kijelentkezett.</span><span class="sxs-lookup"><span data-stu-id="ac885-151">All that remains is toogive your users a way toosign in and sign out.</span></span>

1. <span data-ttu-id="ac885-152">Használhatja a tartományvezérlők toorequire felhasználók toosign a címkék engedélyezi bizonyos lapok való hozzáférés előtt.</span><span class="sxs-lookup"><span data-stu-id="ac885-152">You can use authorize tags in your controllers toorequire users toosign in before they access certain pages.</span></span> <span data-ttu-id="ac885-153">toodo Igen, nyissa meg a Controllers\HomeController.cs, és adja hozzá a hello `[Authorize]` toohello vezérlő vonatkozó címke.</span><span class="sxs-lookup"><span data-stu-id="ac885-153">toodo so, open Controllers\HomeController.cs, and then add hello `[Authorize]` tag toohello About controller.</span></span>

     ```C#
     [Authorize]
     public ActionResult About()
     {
       ...
     ```

2. <span data-ttu-id="ac885-154">OWIN toodirectly probléma a érkező hitelesítési kéréseket a kód is használható.</span><span class="sxs-lookup"><span data-stu-id="ac885-154">You can also use OWIN toodirectly issue authentication requests from within your code.</span></span> <span data-ttu-id="ac885-155">toodo nyisson meg Controllers\AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="ac885-155">toodo so, open Controllers\AccountController.cs.</span></span> <span data-ttu-id="ac885-156">Ezt követően a hello SignIn() és SignOut() műveletek adja ki az OpenID Connect kérdés és kijelentkezési kérések.</span><span class="sxs-lookup"><span data-stu-id="ac885-156">Then, in hello SignIn() and SignOut() actions, issue OpenID Connect challenge and sign-out requests.</span></span>

     ```C#
     public void SignIn()
     {
         // Send an OpenID Connect sign-in request.
         if (!Request.IsAuthenticated)
         {
             HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
         }
     }
     public void SignOut()
     {
         // Send an OpenID Connect sign-out request.
         HttpContext.GetOwinContext().Authentication.SignOut(
              OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
     }
     ```

3. <span data-ttu-id="ac885-157">Nyissa meg a Views\Shared\_LoginPartial.cshtml tooshow hello felhasználói hello app be- és kijelentkezési hivatkozásokat, és tooprint kimenő hello felhasználónév nézetben.</span><span class="sxs-lookup"><span data-stu-id="ac885-157">Open Views\Shared\_LoginPartial.cshtml tooshow hello user hello app sign-in and sign-out links, and tooprint out hello user's name in a view.</span></span>

    ```HTML
    @if (Request.IsAuthenticated)
    {
     <text>
         <ul class="nav navbar-nav navbar-right">
             <li class="navbar-text">
                 Hello, @User.Identity.Name!
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

## <a name="step-4-display-user-information"></a><span data-ttu-id="ac885-158">4. lépés: Felhasználói információk megjelenítése</span><span class="sxs-lookup"><span data-stu-id="ac885-158">Step 4: Display user information</span></span>
<span data-ttu-id="ac885-159">Amikor a felhasználók az OpenID Connect, az Azure AD, hogy a "jogcímek" vagy a helyességi feltételek hello felhasználóról tartalmaz id_token toohello alkalmazások adja vissza.</span><span class="sxs-lookup"><span data-stu-id="ac885-159">When it authenticates users with OpenID Connect, Azure AD returns an id_token toohello app that contains "claims," or assertions about hello user.</span></span> <span data-ttu-id="ac885-160">Használhatja a jogcímalapú toopersonalize hello alkalmazás hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="ac885-160">You can use these claims toopersonalize hello app by doing hello following:</span></span>

1. <span data-ttu-id="ac885-161">Nyissa meg a hello Controllers\HomeController.cs fájlt.</span><span class="sxs-lookup"><span data-stu-id="ac885-161">Open hello Controllers\HomeController.cs file.</span></span> <span data-ttu-id="ac885-162">A tartományvezérlők keresztül hello keresztül elérhető hello felhasználói jogcímek `ClaimsPrincipal.Current` rendszerbiztonsági objektumot.</span><span class="sxs-lookup"><span data-stu-id="ac885-162">You can access hello user's claims in your controllers via hello `ClaimsPrincipal.Current` security principal object.</span></span>

 ```C#
 public ActionResult About()
 {
     ViewBag.Name = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
     ViewBag.ObjectId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
     ViewBag.GivenName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.GivenName).Value;
     ViewBag.Surname = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Surname).Value;
     ViewBag.UPN = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value;

     return View();
 }
 ```

2. <span data-ttu-id="ac885-163">Hozza létre, és futtassa hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ac885-163">Build and run hello app.</span></span> <span data-ttu-id="ac885-164">Ha egy új felhasználó még nem hozott létre, hogy az Ön bérlőjében egy onmicrosoft.com tartománnyal, most már áll hello idő toodo stb.</span><span class="sxs-lookup"><span data-stu-id="ac885-164">If you haven't already created a new user in your tenant with an onmicrosoft.com domain, now is hello time toodo so.</span></span> <span data-ttu-id="ac885-165">Ezt a következőképpen teheti meg:</span><span class="sxs-lookup"><span data-stu-id="ac885-165">Here's how:</span></span>

  <span data-ttu-id="ac885-166">a.</span><span class="sxs-lookup"><span data-stu-id="ac885-166">a.</span></span> <span data-ttu-id="ac885-167">Jelentkezzen be, hogy a felhasználó, és vegye figyelembe, hogyan jelennek meg hello felső sávon hello felhasználói azonosító.</span><span class="sxs-lookup"><span data-stu-id="ac885-167">Sign in with that user, and note how hello user's identity is reflected on hello top bar.</span></span>

  <span data-ttu-id="ac885-168">b.</span><span class="sxs-lookup"><span data-stu-id="ac885-168">b.</span></span> <span data-ttu-id="ac885-169">Jelentkezzen ki, majd jelentkezzen be egy másik felhasználó az Ön bérelt szolgáltatásának.</span><span class="sxs-lookup"><span data-stu-id="ac885-169">Sign out, and then sign back in as another user in your tenant.</span></span>

  <span data-ttu-id="ac885-170">c.</span><span class="sxs-lookup"><span data-stu-id="ac885-170">c.</span></span> <span data-ttu-id="ac885-171">Különösen gondolkodhat már, ha regisztrálja, és futtassa az alkalmazást (a saját clientId) egy másik példánya, és egyszeri bejelentkezés működés közben bemutató.</span><span class="sxs-lookup"><span data-stu-id="ac885-171">If you're feeling particularly ambitious, register and run another instance of this app (with its own clientId), and watch single sign-in in action.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ac885-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ac885-172">Next steps</span></span>
<span data-ttu-id="ac885-173">Referenciáért lásd: [befejeződött hello minta](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip) (a konfigurációs értékek nélkül).</span><span class="sxs-lookup"><span data-stu-id="ac885-173">For reference, see [hello completed sample](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip) (without your configuration values).</span></span>

<span data-ttu-id="ac885-174">Most már továbbléphet a témakörök speciális toomore.</span><span class="sxs-lookup"><span data-stu-id="ac885-174">You can now move on toomore advanced topics.</span></span> <span data-ttu-id="ac885-175">Például [egy webes API-t az Azure ad-vel biztonságos](active-directory-devquickstarts-webapi-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="ac885-175">For example, try [Secure a Web API with Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
