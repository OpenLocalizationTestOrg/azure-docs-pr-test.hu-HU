---
title: "Ismerkedés az Azure AD .NET webalkalmazás |} Microsoft Docs"
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
ms.openlocfilehash: 7ac5d3e5cc28ead993e159d003244e6451acb0cc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="aspnet-web-app-sign-in-and-sign-out-with-azure-ad"></a><span data-ttu-id="b2171-103">ASP.NET webalkalmazás be- és kijelentkezési az Azure ad szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="b2171-103">ASP.NET web app sign-in and sign-out with Azure AD</span></span>
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="b2171-104">Be- és kijelentkezési egyetlen azáltal csak néhány sornyi kódot, Azure Active Directory (Azure AD) egyszerűen meg a webalkalmazás Identitáskezelés kihelyező.</span><span class="sxs-lookup"><span data-stu-id="b2171-104">By providing a single sign-in and sign-out with only a few lines of code, Azure Active Directory (Azure AD) makes it simple for you to outsource web-app identity management.</span></span> <span data-ttu-id="b2171-105">ASP.NET webes alkalmazások mindkét felhasználók regisztrálhat a Microsoft általi implementációja Open Web Interface .NET (OWIN) köztes használatával.</span><span class="sxs-lookup"><span data-stu-id="b2171-105">You can sign users in and out of ASP.NET web apps by using the Microsoft implementation of Open Web Interface for .NET (OWIN) middleware.</span></span> <span data-ttu-id="b2171-106">Közösségi szerkesztésű OWIN köztes a .NET-keretrendszer 4.5 része.</span><span class="sxs-lookup"><span data-stu-id="b2171-106">Community-driven OWIN middleware is included in .NET Framework 4.5.</span></span> <span data-ttu-id="b2171-107">Ez a cikk bemutatja, hogyan használható az OWIN:</span><span class="sxs-lookup"><span data-stu-id="b2171-107">This article shows how to use OWIN to:</span></span>

* <span data-ttu-id="b2171-108">A felhasználók beléptetése a webalkalmazások Azure AD használatával az identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="b2171-108">Sign users in to web apps by using Azure AD as the identity provider.</span></span>
* <span data-ttu-id="b2171-109">Bizonyos felhasználói információk jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="b2171-109">Display some user information.</span></span>
* <span data-ttu-id="b2171-110">A felhasználók az alkalmazások kívül.</span><span class="sxs-lookup"><span data-stu-id="b2171-110">Sign users out of the apps.</span></span>

## <a name="before-you-get-started"></a><span data-ttu-id="b2171-111">A kezdés előtt</span><span class="sxs-lookup"><span data-stu-id="b2171-111">Before you get started</span></span>
* <span data-ttu-id="b2171-112">Töltse le a [app vázat](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) vagy töltse le a [elkészült mintát](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="b2171-112">Download the [app skeleton](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) or download the [completed sample](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).</span></span>
* <span data-ttu-id="b2171-113">Szükség az Azure AD-bérlő, amelyben az alkalmazás regisztrálásához.</span><span class="sxs-lookup"><span data-stu-id="b2171-113">You also need an Azure AD tenant in which to register the app.</span></span> <span data-ttu-id="b2171-114">Ha még nem telepítette az Azure AD-bérlő [beszerzéséről egy](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="b2171-114">If you don't already have an Azure AD tenant, [learn how to get one](active-directory-howto-tenant.md).</span></span>

<span data-ttu-id="b2171-115">Ha készen áll, hajtsa végre a következő négy részből áll.</span><span class="sxs-lookup"><span data-stu-id="b2171-115">When you are ready, follow the procedures in the next four sections.</span></span>

## <a name="step-1-register-the-new-app-with-azure-ad"></a><span data-ttu-id="b2171-116">1. lépés: Az új alkalmazás regisztrálása az Azure ad szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="b2171-116">Step 1: Register the new app with Azure AD</span></span>
<span data-ttu-id="b2171-117">Az alkalmazás beállítása a felhasználók hitelesítésére, először regisztrálja az Ön bérelt szolgáltatásának a következő módon:</span><span class="sxs-lookup"><span data-stu-id="b2171-117">To set up the app to authenticate users, first register it in your tenant by doing the following:</span></span>

1. <span data-ttu-id="b2171-118">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b2171-118">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="b2171-119">A felső eszköztáron kattintson a fiók nevére.</span><span class="sxs-lookup"><span data-stu-id="b2171-119">On the top bar, click your account name.</span></span> <span data-ttu-id="b2171-120">Az a **Directory** listára, válassza ki az Active Directory-bérlőt, ahol az alkalmazást regisztrálni kívánt.</span><span class="sxs-lookup"><span data-stu-id="b2171-120">Under the **Directory** list, select the Active Directory tenant where you want to register the app.</span></span>
3. <span data-ttu-id="b2171-121">Kattintson a **több szolgáltatások** a bal oldali ablaktáblán, és válassza a **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="b2171-121">Click **More Services** in the left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="b2171-122">Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="b2171-122">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="b2171-123">Kövesse az utasításokat, hozzon létre egy új **webalkalmazás és/vagy WebAPI**.</span><span class="sxs-lookup"><span data-stu-id="b2171-123">Follow the prompts to create a new **Web Application and/or WebAPI**.</span></span>
  * <span data-ttu-id="b2171-124">**Név** az alkalmazásnak, hogy a felhasználók ismerteti.</span><span class="sxs-lookup"><span data-stu-id="b2171-124">**Name** describes the app to users.</span></span>
  * <span data-ttu-id="b2171-125">**Bejelentkezési URL-cím** az alkalmazás alap URL-címe.</span><span class="sxs-lookup"><span data-stu-id="b2171-125">**Sign-On URL** is the base URL of the app.</span></span> <span data-ttu-id="b2171-126">A vázat alapértelmezett URL-címe: https://localhost:44320 /.</span><span class="sxs-lookup"><span data-stu-id="b2171-126">The skeleton's default URL is https://localhost:44320/.</span></span>
6. <span data-ttu-id="b2171-127">A regisztráció befejezését követően az Azure AD rendeli hozzá az alkalmazás egy egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="b2171-127">After you've completed the registration, Azure AD assigns the app a unique application ID.</span></span> <span data-ttu-id="b2171-128">Kimásolhatja az értéket az alkalmazás oldalon a következő szakaszokban lévő használatára.</span><span class="sxs-lookup"><span data-stu-id="b2171-128">Copy the value from the app page to use in the next sections.</span></span>
7. <span data-ttu-id="b2171-129">Az a **beállítások** -> **tulajdonságok** az alkalmazás lapján frissítse a App ID URI.</span><span class="sxs-lookup"><span data-stu-id="b2171-129">From the **Settings** -> **Properties** page for your application, update the App ID URI.</span></span> <span data-ttu-id="b2171-130">A **App ID URI** az alkalmazás egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="b2171-130">The **App ID URI** is a unique identifier for the app.</span></span> <span data-ttu-id="b2171-131">Az elnevezési konvenció: `https://<tenant-domain>/<app-name>` (például `https://contoso.onmicrosoft.com/my-first-aad-app`).</span><span class="sxs-lookup"><span data-stu-id="b2171-131">The naming convention is `https://<tenant-domain>/<app-name>` (for example, `https://contoso.onmicrosoft.com/my-first-aad-app`).</span></span>

## <a name="step-2-set-up-the-app-to-use-the-owin-authentication-pipeline"></a><span data-ttu-id="b2171-132">2. lépés: Az alkalmazás használatának beállítása a OWIN hitelesítési folyamatot</span><span class="sxs-lookup"><span data-stu-id="b2171-132">Step 2: Set up the app to use the OWIN authentication pipeline</span></span>
<span data-ttu-id="b2171-133">Ebben a lépésben az OWIN közbenső szoftvert az OpenID Connect hitelesítési protokoll használatára konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="b2171-133">In this step, you configure the OWIN middleware to use the OpenID Connect authentication protocol.</span></span> <span data-ttu-id="b2171-134">OWIN segítségével be- és kijelentkezési kérések kiállítása, kezelni a felhasználói munkameneteket, felhasználói adatok beolvasása és így tovább.</span><span class="sxs-lookup"><span data-stu-id="b2171-134">You use OWIN to issue sign-in and sign-out requests, manage user sessions, get user information, and so forth.</span></span>

1. <span data-ttu-id="b2171-135">Első lépésként hozzá az OWIN köztes NuGet-csomagok a projekthez a Csomagkezelő konzol használatával.</span><span class="sxs-lookup"><span data-stu-id="b2171-135">To begin, add the OWIN middleware NuGet packages to the project by using the Package Manager Console.</span></span>

     ```
     PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
     PM> Install-Package Microsoft.Owin.Security.Cookies
     PM> Install-Package Microsoft.Owin.Host.SystemWeb
     ```

2. <span data-ttu-id="b2171-136">Egy OWIN indítási osztály hozzáadása a projekthez nevű `Startup.cs`, kattintson jobb gombbal a projektre, válassza ki **Hozzáadás**, jelölje be **új elem**, majd keresse meg a **OWIN**.</span><span class="sxs-lookup"><span data-stu-id="b2171-136">To add an OWIN Startup class to the project called `Startup.cs`, right-click the project, select **Add**, select **New Item**, and then search for **OWIN**.</span></span> <span data-ttu-id="b2171-137">Az OWIN köztes meghívja a **Configuration(...)**  módszer az alkalmazás indításakor.</span><span class="sxs-lookup"><span data-stu-id="b2171-137">The OWIN middleware invokes the **Configuration(...)** method when the app starts.</span></span>
3. <span data-ttu-id="b2171-138">Módosítsa az osztálydeklaráció való `public partial class Startup`.</span><span class="sxs-lookup"><span data-stu-id="b2171-138">Change the class declaration to `public partial class Startup`.</span></span> <span data-ttu-id="b2171-139">Azt korábban már megvalósított Ez az osztály tartozik, egy másik fájlban.</span><span class="sxs-lookup"><span data-stu-id="b2171-139">We've already implemented part of this class for you in another file.</span></span> <span data-ttu-id="b2171-140">Az a **Configuration(...)**  metódus hívása legyen **ConfgureAuth(...)**  az alkalmazás hitelesítés beállítása.</span><span class="sxs-lookup"><span data-stu-id="b2171-140">In the **Configuration(...)** method, make a call to **ConfgureAuth(...)** to set up authentication for the app.</span></span>  

     ```C#
     public partial class Startup
     {
         public void Configuration(IAppBuilder app)
         {
             ConfigureAuth(app);
         }
     }
     ```

4. <span data-ttu-id="b2171-141">Nyissa meg a App_Start\Startup.Auth.cs fájlt, és megvalósításához a **ConfigureAuth(...)**  metódust.</span><span class="sxs-lookup"><span data-stu-id="b2171-141">Open the App_Start\Startup.Auth.cs file, and then implement the **ConfigureAuth(...)** method.</span></span> <span data-ttu-id="b2171-142">Megadja a paraméterek *OpenIDConnectAuthenticationOptions* kommunikálni az Azure AD alkalmazás-koordináták szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="b2171-142">The parameters you provide in *OpenIDConnectAuthenticationOptions* serve as coordinates for the app to communicate with Azure AD.</span></span> <span data-ttu-id="b2171-143">Azt is be kell állítania cookie-hitelesítés, mert az OpenID Connect köztes cookie-kat használ a háttérben.</span><span class="sxs-lookup"><span data-stu-id="b2171-143">You also need to set up cookie authentication, because the OpenID Connect middleware uses cookies in the background.</span></span>

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

5. <span data-ttu-id="b2171-144">Nyissa meg a web.config fájlt a projekt gyökérkönyvtárában, és írja be a konfigurációs értékeket az a `<appSettings>` szakasz.</span><span class="sxs-lookup"><span data-stu-id="b2171-144">Open the web.config file in the root of the project, and then enter the configuration values in the `<appSettings>` section.</span></span>
  * <span data-ttu-id="b2171-145">`ida:ClientId`: A GUID-azonosító az Azure-portálról másolt "1. lépés: az új alkalmazás regisztrálása az Azure ad szolgáltatással."</span><span class="sxs-lookup"><span data-stu-id="b2171-145">`ida:ClientId`: The GUID you copied from the Azure portal in "Step 1: Register the new app with Azure AD."</span></span>
  * <span data-ttu-id="b2171-146">`ida:Tenant`: Az Azure AD-bérlő (például contoso.onmicrosoft.com) a neve.</span><span class="sxs-lookup"><span data-stu-id="b2171-146">`ida:Tenant`: The name of your Azure AD tenant (for example, contoso.onmicrosoft.com).</span></span>
  * <span data-ttu-id="b2171-147">`ida:PostLogoutRedirectUri`: A kijelző, amely közli az Azure AD, ha a rendszer átirányítja az a felhasználó a kijelentkezési kérés sikeres végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="b2171-147">`ida:PostLogoutRedirectUri`: The indicator that tells Azure AD where a user should be redirected after a sign-out request is successfully completed.</span></span>

## <a name="step-3-use-owin-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a><span data-ttu-id="b2171-148">3. lépés: Használja az Azure AD-be- és kijelentkezési kérések kiállítása az OWIN</span><span class="sxs-lookup"><span data-stu-id="b2171-148">Step 3: Use OWIN to issue sign-in and sign-out requests to Azure AD</span></span>
<span data-ttu-id="b2171-149">Az alkalmazás megfelelően konfigurálva van az Azure AD az OpenID Connect hitelesítési protokoll használatával kommunikálnak.</span><span class="sxs-lookup"><span data-stu-id="b2171-149">The app is now properly configured to communicate with Azure AD by using the OpenID Connect authentication protocol.</span></span> <span data-ttu-id="b2171-150">OWIN kezelt összes hitelesítési üzenetek létrehozásával, ellenőrzése az Azure ad-jogkivonatok és karbantartása a felhasználói munkamenetek részleteit.</span><span class="sxs-lookup"><span data-stu-id="b2171-150">OWIN has handled all of the details of crafting authentication messages, validating tokens from Azure AD, and maintaining user sessions.</span></span> <span data-ttu-id="b2171-151">Most már kell biztosítani a felhasználóknak a be-és kijelentkezés úgy.</span><span class="sxs-lookup"><span data-stu-id="b2171-151">All that remains is to give your users a way to sign in and sign out.</span></span>

1. <span data-ttu-id="b2171-152">Használhatja a tartományvezérlőket, a felhasználóknak a bejelentkezéshez, mielőtt hozzáférnek az egyes lapok címkék engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="b2171-152">You can use authorize tags in your controllers to require users to sign in before they access certain pages.</span></span> <span data-ttu-id="b2171-153">Ehhez nyissa meg a Controllers\HomeController.cs, és adja hozzá a `[Authorize]` címke a jogi tudnivalók megjelenítése Névjegy vezérlőhöz.</span><span class="sxs-lookup"><span data-stu-id="b2171-153">To do so, open Controllers\HomeController.cs, and then add the `[Authorize]` tag to the About controller.</span></span>

     ```C#
     [Authorize]
     public ActionResult About()
     {
       ...
     ```

2. <span data-ttu-id="b2171-154">OWIN segítségével közvetlenül kiadni a kód a érkező hitelesítési kéréseket.</span><span class="sxs-lookup"><span data-stu-id="b2171-154">You can also use OWIN to directly issue authentication requests from within your code.</span></span> <span data-ttu-id="b2171-155">Ehhez nyissa meg a Controllers\AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="b2171-155">To do so, open Controllers\AccountController.cs.</span></span> <span data-ttu-id="b2171-156">Ezt követően a SignIn() és SignOut() műveletek adja ki az OpenID Connect kérdés és kijelentkezési kérések.</span><span class="sxs-lookup"><span data-stu-id="b2171-156">Then, in the SignIn() and SignOut() actions, issue OpenID Connect challenge and sign-out requests.</span></span>

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

3. <span data-ttu-id="b2171-157">Nyissa meg a Views\Shared\_LoginPartial.cshtml megjelenítése a felhasználónak az alkalmazás be- és kijelentkezési hivatkozások, és nyomtassa ki a nézetben a felhasználó nevét.</span><span class="sxs-lookup"><span data-stu-id="b2171-157">Open Views\Shared\_LoginPartial.cshtml to show the user the app sign-in and sign-out links, and to print out the user's name in a view.</span></span>

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

## <a name="step-4-display-user-information"></a><span data-ttu-id="b2171-158">4. lépés: Felhasználói információk megjelenítése</span><span class="sxs-lookup"><span data-stu-id="b2171-158">Step 4: Display user information</span></span>
<span data-ttu-id="b2171-159">Amikor a felhasználók az OpenID Connect, az Azure AD egy id_token az alkalmazásra, amely tartalmazza a "jogcímek", vagy a felhasználóval kapcsolatos előfeltételek adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b2171-159">When it authenticates users with OpenID Connect, Azure AD returns an id_token to the app that contains "claims," or assertions about the user.</span></span> <span data-ttu-id="b2171-160">Ezeket a jogcímeket segítségével szabja személyre az alkalmazás a következő módon:</span><span class="sxs-lookup"><span data-stu-id="b2171-160">You can use these claims to personalize the app by doing the following:</span></span>

1. <span data-ttu-id="b2171-161">Nyissa meg a Controllers\HomeController.cs fájlt.</span><span class="sxs-lookup"><span data-stu-id="b2171-161">Open the Controllers\HomeController.cs file.</span></span> <span data-ttu-id="b2171-162">A felhasználói jogcímek, a vezérlők keresztül érheti el a `ClaimsPrincipal.Current` rendszerbiztonsági objektumot.</span><span class="sxs-lookup"><span data-stu-id="b2171-162">You can access the user's claims in your controllers via the `ClaimsPrincipal.Current` security principal object.</span></span>

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

2. <span data-ttu-id="b2171-163">Hozza létre, és futtassa az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b2171-163">Build and run the app.</span></span> <span data-ttu-id="b2171-164">Ha egy új felhasználó még nem hozott létre, hogy az Ön bérlőjében egy onmicrosoft.com tartománnyal, most az ehhez idő.</span><span class="sxs-lookup"><span data-stu-id="b2171-164">If you haven't already created a new user in your tenant with an onmicrosoft.com domain, now is the time to do so.</span></span> <span data-ttu-id="b2171-165">Ezt a következőképpen teheti meg:</span><span class="sxs-lookup"><span data-stu-id="b2171-165">Here's how:</span></span>

  <span data-ttu-id="b2171-166">a.</span><span class="sxs-lookup"><span data-stu-id="b2171-166">a.</span></span> <span data-ttu-id="b2171-167">Jelentkezzen be, hogy a felhasználó, és vegye figyelembe, hogyan jelennek meg a felső sávon a felhasználó identitását.</span><span class="sxs-lookup"><span data-stu-id="b2171-167">Sign in with that user, and note how the user's identity is reflected on the top bar.</span></span>

  <span data-ttu-id="b2171-168">b.</span><span class="sxs-lookup"><span data-stu-id="b2171-168">b.</span></span> <span data-ttu-id="b2171-169">Jelentkezzen ki, majd jelentkezzen be egy másik felhasználó az Ön bérelt szolgáltatásának.</span><span class="sxs-lookup"><span data-stu-id="b2171-169">Sign out, and then sign back in as another user in your tenant.</span></span>

  <span data-ttu-id="b2171-170">c.</span><span class="sxs-lookup"><span data-stu-id="b2171-170">c.</span></span> <span data-ttu-id="b2171-171">Különösen gondolkodhat már, ha regisztrálja, és futtassa az alkalmazást (a saját clientId) egy másik példánya, és egyszeri bejelentkezés működés közben bemutató.</span><span class="sxs-lookup"><span data-stu-id="b2171-171">If you're feeling particularly ambitious, register and run another instance of this app (with its own clientId), and watch single sign-in in action.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b2171-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b2171-172">Next steps</span></span>
<span data-ttu-id="b2171-173">Referenciáért lásd: [az elkészült mintát](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip) (a konfigurációs értékek nélkül).</span><span class="sxs-lookup"><span data-stu-id="b2171-173">For reference, see [the completed sample](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip) (without your configuration values).</span></span>

<span data-ttu-id="b2171-174">Most már továbbléphet az összetettebb témákra.</span><span class="sxs-lookup"><span data-stu-id="b2171-174">You can now move on to more advanced topics.</span></span> <span data-ttu-id="b2171-175">Például [egy webes API-t az Azure ad-vel biztonságos](active-directory-devquickstarts-webapi-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="b2171-175">For example, try [Secure a Web API with Azure AD](active-directory-devquickstarts-webapi-dotnet.md).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
