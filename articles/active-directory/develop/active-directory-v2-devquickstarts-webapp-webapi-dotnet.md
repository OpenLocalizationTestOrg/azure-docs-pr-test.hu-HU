---
title: "aaaAzure AD v2.0 .NET web app hívó API bevezetés |} Microsoft Docs"
description: "Hogyan toobuild .NET MVC webes alkalmazás, amely behívja webszolgáltatások használatával személyes Microsoft fiókok és a munkahelyi vagy iskolai fiókot a következő bejelentkezés."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 56be906e-71de-469d-9a5c-9fc08aae4223
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 1a70791418bc2a7d1fdfbafb9b5126a033a32292
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="calling-a-web-api-from-a-net-web-app"></a><span data-ttu-id="a077e-103">A webes API hívása a .NET-webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="a077e-103">Calling a web API from a .NET web app</span></span>
<span data-ttu-id="a077e-104">Hello v2.0-végponttal gyorsan hozzáadhat hitelesítési tooyour webalkalmazások és webes API-khoz személyes Microsoft-fiókot is támogatása és a munkahelyi vagy iskolai fiókok.</span><span class="sxs-lookup"><span data-stu-id="a077e-104">With hello v2.0 endpoint, you can quickly add authentication tooyour web apps and web APIs with support for both personal Microsoft accounts and work or school accounts.</span></span>  <span data-ttu-id="a077e-105">Itt azt fogja létrehozni egy MVC webalkalmazás, mely aláírja a felhasználók az OpenID Connect, használja a Microsoft OWIN köztes segítséget.</span><span class="sxs-lookup"><span data-stu-id="a077e-105">Here, we'll build an MVC web app that signs users in using OpenID Connect, with some help from Microsoft's OWIN middleware.</span></span>  <span data-ttu-id="a077e-106">hello webes alkalmazás OAuth 2.0 hozzáférési jogkivonatok lekérése egy webes api lehetővé teszi, hogy OAuth 2.0-s által védett létrehozása, olvasása, és törli a egy adott felhasználó "Feladatlista".</span><span class="sxs-lookup"><span data-stu-id="a077e-106">hello web app will get OAuth 2.0 access tokens for a web api secured by OAuth 2.0 that allows create, read, and delete on a given user's "To-Do List".</span></span>

<span data-ttu-id="a077e-107">Ez az oktatóanyag fog elsősorban összpontosítanak MSAL tooacquire használatával, és a hozzáférési jogkivonatok használata egy webalkalmazás teljes ismertetett [Itt](active-directory-v2-flows.md#web-apps).</span><span class="sxs-lookup"><span data-stu-id="a077e-107">This tutorial will focus primarily on using MSAL tooacquire and use access tokens in a web app, described in full [here](active-directory-v2-flows.md#web-apps).</span></span>  <span data-ttu-id="a077e-108">Előfeltételként, érdemes lehet toofirst megtudhatja, hogyan túl[hozzáadása alapszintű bejelentkezési tooa webalkalmazás](active-directory-v2-devquickstarts-dotnet-web.md) vagy hogyan túl[megfelelően a webes API biztonságossá tétele](active-directory-v2-devquickstarts-dotnet-api.md).</span><span class="sxs-lookup"><span data-stu-id="a077e-108">As prerequisites, you may want toofirst learn how too[add basic sign-in tooa web app](active-directory-v2-devquickstarts-dotnet-web.md) or how too[properly secure a web API](active-directory-v2-devquickstarts-dotnet-api.md).</span></span>

> [!NOTE]
> <span data-ttu-id="a077e-109">Nem minden Azure Active Directory forgatókönyvek és funkciók támogatják hello v2.0-végponttól.</span><span class="sxs-lookup"><span data-stu-id="a077e-109">Not all Azure Active Directory scenarios & features are supported by hello v2.0 endpoint.</span></span>  <span data-ttu-id="a077e-110">toodetermine használatát hello v2.0-végpontra, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="a077e-110">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="download-sample-code"></a><span data-ttu-id="a077e-111">Mintakód letöltése</span><span class="sxs-lookup"><span data-stu-id="a077e-111">Download sample code</span></span>
<span data-ttu-id="a077e-112">az oktatóanyag kódjának hello kezelt [a Githubon](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet).</span><span class="sxs-lookup"><span data-stu-id="a077e-112">hello code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet).</span></span>  <span data-ttu-id="a077e-113">toofollow mellett, akkor [töltse le a .zip hello alkalmazás vázát](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) vagy a Klónozás hello vázat:</span><span class="sxs-lookup"><span data-stu-id="a077e-113">toofollow along, you can [download hello app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) or clone hello skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

<span data-ttu-id="a077e-114">Alternatív megoldásként, [letöltése befejeződött hello alkalmazást egy .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) vagy a Klónozás befejeződött hello alkalmazásba:</span><span class="sxs-lookup"><span data-stu-id="a077e-114">Alternatively, you can [download hello completed app as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) or clone hello completed app:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

## <a name="register-an-app"></a><span data-ttu-id="a077e-115">Alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="a077e-115">Register an app</span></span>
<span data-ttu-id="a077e-116">Hozzon létre egy új alkalmazást [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy kövesse az alábbi [részletes lépéseket](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="a077e-116">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="a077e-117">Győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="a077e-117">Make sure to:</span></span>

* <span data-ttu-id="a077e-118">Másolja le hello **alkalmazásazonosító** tooyour app hozzárendelve, szüksége lehet rájuk hamarosan.</span><span class="sxs-lookup"><span data-stu-id="a077e-118">Copy down hello **Application Id** assigned tooyour app, you'll need it soon.</span></span>
* <span data-ttu-id="a077e-119">Hozzon létre egy **alkalmazás titkos kulcs** a hello **jelszó** típusát, és másolja le az érték későbbi használatra</span><span class="sxs-lookup"><span data-stu-id="a077e-119">Create an **App Secret** of hello **Password** type, and copy down its value for later</span></span>
* <span data-ttu-id="a077e-120">Adja hozzá a hello **webes** platform az alkalmazásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="a077e-120">Add hello **Web** platform for your app.</span></span>
* <span data-ttu-id="a077e-121">Adja meg a megfelelő hello **átirányítási URI-**.</span><span class="sxs-lookup"><span data-stu-id="a077e-121">Enter hello correct **Redirect URI**.</span></span> <span data-ttu-id="a077e-122">hello átirányítási uri azt jelzi, ha hitelesítési válaszok legyenek irányítva – hello ebben az oktatóanyagban alapértelmezés szerint AD tooAzure `https://localhost:44326/`.</span><span class="sxs-lookup"><span data-stu-id="a077e-122">hello redirect uri indicates tooAzure AD where authentication responses should be directed - hello default for this tutorial is `https://localhost:44326/`.</span></span>

## <a name="install-owin"></a><span data-ttu-id="a077e-123">Az OWIN telepítése</span><span class="sxs-lookup"><span data-stu-id="a077e-123">Install OWIN</span></span>
<span data-ttu-id="a077e-124">Adja hozzá a hello OWIN köztes NuGet csomagjainak toohello `TodoList-WebApp` projektet a hello Csomagkezelő konzol.</span><span class="sxs-lookup"><span data-stu-id="a077e-124">Add hello OWIN middleware NuGet packages toohello `TodoList-WebApp` project using hello Package Manager Console.</span></span>  <span data-ttu-id="a077e-125">hello OWIN köztes kell használt tooissue be- és kijelentkezési kérések, hello felhasználói munkamenet kezeli, és többek között a hello felhasználó adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="a077e-125">hello OWIN middleware will be used tooissue sign-in and sign-out requests, manage hello user's session, and get information about hello user, amongst other things.</span></span>

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

## <a name="sign-hello-user-in"></a><span data-ttu-id="a077e-126">A bejelentkezési hello felhasználó</span><span class="sxs-lookup"><span data-stu-id="a077e-126">Sign hello user in</span></span>
<span data-ttu-id="a077e-127">Mostantól konfigurálhatja az hello OWIN köztes toouse hello [OpenID Connect hitelesítési protokoll](active-directory-v2-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="a077e-127">Now configure hello OWIN middleware toouse hello [OpenID Connect authentication protocol](active-directory-v2-protocols.md).</span></span>  

* <span data-ttu-id="a077e-128">Nyissa meg hello `web.config` hello hello legfelső-szintű fájl `TodoList-WebApp` projektre, és írja be az alkalmazás konfigurációs értékeit hello `<appSettings>` szakasz.</span><span class="sxs-lookup"><span data-stu-id="a077e-128">Open hello `web.config` file in hello root of hello `TodoList-WebApp` project, and enter your app's configuration values in hello `<appSettings>` section.</span></span>
  * <span data-ttu-id="a077e-129">Hello `ida:ClientId` hello van **alkalmazásazonosító** tooyour app hello regisztrációs portálon rendelt.</span><span class="sxs-lookup"><span data-stu-id="a077e-129">hello `ida:ClientId` is hello **Application Id** assigned tooyour app in hello registration portal.</span></span>
  * <span data-ttu-id="a077e-130">Hello `ida:ClientSecret` hello van **alkalmazás titkos kulcs** hello regisztrációs portálon létrehozott.</span><span class="sxs-lookup"><span data-stu-id="a077e-130">hello `ida:ClientSecret` is hello **App Secret** you created in hello registration portal.</span></span>
  * <span data-ttu-id="a077e-131">Hello `ida:RedirectUri` hello van **átirányítási Uri-** hello portálon megadott.</span><span class="sxs-lookup"><span data-stu-id="a077e-131">hello `ida:RedirectUri` is hello **Redirect Uri** you entered in hello portal.</span></span>
* <span data-ttu-id="a077e-132">Nyissa meg hello `web.config` hello hello legfelső-szintű fájl `TodoList-Service` projektre, és cserélje le a hello `ida:Audience` a hello azonos **alkalmazásazonosító** a fentiek szerint.</span><span class="sxs-lookup"><span data-stu-id="a077e-132">Open hello `web.config` file in hello root of hello `TodoList-Service` project, and replace hello `ida:Audience` with hello same **Application Id** as above.</span></span>
* <span data-ttu-id="a077e-133">Nyissa meg hello fájl `App_Start\Startup.Auth.cs` , és adja hozzá `using` a hello tárak a fenti utasítások.</span><span class="sxs-lookup"><span data-stu-id="a077e-133">Open hello file `App_Start\Startup.Auth.cs` and add `using` statements for hello libraries from above.</span></span>
* <span data-ttu-id="a077e-134">A hello ugyanazt a fájlt, hello megvalósítása `ConfigureAuth(...)` metódust.</span><span class="sxs-lookup"><span data-stu-id="a077e-134">In hello same file, implement hello `ConfigureAuth(...)` method.</span></span>  <span data-ttu-id="a077e-135">Megadja a paraméterek hello `OpenIDConnectAuthenticationOptions` erre a célra az Azure ad alkalmazás toocommunicate koordinátáit.</span><span class="sxs-lookup"><span data-stu-id="a077e-135">hello parameters you provide in `OpenIDConnectAuthenticationOptions` will serve as coordinates for your app toocommunicate with Azure AD.</span></span>

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
                    Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0 "),
                    Scope = "openid email profile offline_access",
                    RedirectUri = redirectUri,
                    PostLogoutRedirectUri = redirectUri,
                    TokenValidationParameters = new TokenValidationParameters
                    {
                        ValidateIssuer = false,
                    },

                    // hello `AuthorizationCodeReceived` notification is used toocapture and redeem hello authorization_code that hello v2.0 endpoint returns tooyour app.

                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed,
                        AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    }

        });
}
// ...
```

## <a name="use-msal-tooget-access-tokens"></a><span data-ttu-id="a077e-136">MSAL tooget hozzáférési jogkivonatok használata</span><span class="sxs-lookup"><span data-stu-id="a077e-136">Use MSAL tooget access tokens</span></span>
<span data-ttu-id="a077e-137">A hello `AuthorizationCodeReceived` értesítési, azt szeretnénk, ha toouse [OAuth 2.0 párhuzamosan az OpenID Connect](active-directory-v2-protocols.md) tooredeem hello authorization_code egy hozzáférési jogkivonat toohello tennivalók listája szolgáltatás számára.</span><span class="sxs-lookup"><span data-stu-id="a077e-137">In hello `AuthorizationCodeReceived` notification, we want toouse [OAuth 2.0 in tandem with OpenID Connect](active-directory-v2-protocols.md) tooredeem hello authorization_code for an access token toohello To-Do List Service.</span></span>  <span data-ttu-id="a077e-138">MSAL teheti a folyamat egyszerűen meg:</span><span class="sxs-lookup"><span data-stu-id="a077e-138">MSAL can make this process easy for you:</span></span>

* <span data-ttu-id="a077e-139">Először telepítse a MSAL hello előzetes verziója:</span><span class="sxs-lookup"><span data-stu-id="a077e-139">First, install hello preview version of MSAL:</span></span>

```PM> Install-Package Microsoft.Identity.Client -ProjectName TodoList-WebApp -IncludePrerelease```

* <span data-ttu-id="a077e-140">Adja hozzá egy másik `using` utasítás toohello `App_Start\Startup.Auth.cs` MSAL fájlt.</span><span class="sxs-lookup"><span data-stu-id="a077e-140">And add another `using` statement toohello `App_Start\Startup.Auth.cs` file for MSAL.</span></span>
* <span data-ttu-id="a077e-141">Ezután adja hozzá egy új módszer, hello `OnAuthorizationCodeReceived` eseménykezelő.</span><span class="sxs-lookup"><span data-stu-id="a077e-141">Now add a new method, hello `OnAuthorizationCodeReceived` event handler.</span></span>  <span data-ttu-id="a077e-142">A kezelő MSAL tooacquire egy hozzáférési jogkivonat toohello tennivalók listája API-t fogja használni, és hello jogkivonat-ban tárolja MSAL tartozó jogkivonat gyorsítótára későbbi használatra:</span><span class="sxs-lookup"><span data-stu-id="a077e-142">This handler will use MSAL tooacquire an access token toohello To-Do List API, and will store hello token in MSAL's token cache for later:</span></span>

```C#
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
        string userObjectId = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        string tenantID = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenantID, string.Empty);
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        // Here you ask for a token using hello web app's clientId as hello scope, since hello web app and service share hello same clientId.
        app = new ConfidentialClientApplication(Startup.clientId, redirectUri, cred, new NaiveSessionCache(userObjectId, notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase)) {};
        var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { clientId }, notification.Code);
}
// ...
```

* <span data-ttu-id="a077e-143">A webalkalmazásokban MSAL rendelkezik egy bővíthető jogkivonatok gyorsítótárát, amely használt toostore jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="a077e-143">In web apps, MSAL has an extensible token cache that can be used toostore tokens.</span></span>  <span data-ttu-id="a077e-144">Ezt a mintát megvalósító hello `NaiveSessionCache` http munkamenet tárolási toocache jogkivonatokat használ, amely.</span><span class="sxs-lookup"><span data-stu-id="a077e-144">This sample implements hello `NaiveSessionCache` which uses http session storage toocache tokens.</span></span>

<!-- TODO: Token Cache article -->


## <a name="call-hello-web-api"></a><span data-ttu-id="a077e-145">Hello Web API hívása</span><span class="sxs-lookup"><span data-stu-id="a077e-145">Call hello Web API</span></span>
<span data-ttu-id="a077e-146">Most már tooactually használja a 3. lépésben beszerzett hello access_token idő.</span><span class="sxs-lookup"><span data-stu-id="a077e-146">Now it's time tooactually use hello access_token you acquired in step 3.</span></span>  <span data-ttu-id="a077e-147">Nyissa meg hello webes alkalmazás `Controllers\TodoListController.cs` fájlt, ami lehetővé teszi, hogy minden hello CRUD-kérelmek toohello tennivalók listája API.</span><span class="sxs-lookup"><span data-stu-id="a077e-147">Open hello web app's `Controllers\TodoListController.cs` file, which makes all hello CRUD requests toohello To-Do List API.</span></span>

* <span data-ttu-id="a077e-148">Használhatja a MSAL újra ide toofetch access_tokens a hello MSAL gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="a077e-148">You can use MSAL again here toofetch access_tokens from hello MSAL cache.</span></span>  <span data-ttu-id="a077e-149">Először adja hozzá a `using` MSAL toothis fájl utasítást.</span><span class="sxs-lookup"><span data-stu-id="a077e-149">First, add a `using` statement for MSAL toothis file.</span></span>
  
    `using Microsoft.Identity.Client;`
* <span data-ttu-id="a077e-150">A hello `Index` művelet, használjon MSAL `AcquireTokenSilentAsync` metódus tooget egy access_token használható hello feladatlista szolgáltatáshoz használt tooread adatait:</span><span class="sxs-lookup"><span data-stu-id="a077e-150">In hello `Index` action, use MSAL's `AcquireTokenSilentAsync` method tooget an access_token that can be used tooread data from hello To-Do List service:</span></span>

```C#
// ...
string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
string tenantID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, tenantID, string.Empty);
ClientCredential credential = new ClientCredential(Startup.clientId, Startup.clientSecret);

// Here you ask for a token using hello web app's clientId as hello scope, since hello web app and service share hello same clientId.
app = new ConfidentialClientApplication(Startup.clientId, redirectUri, credential, new NaiveSessionCache(userObjectID, this.HttpContext)){};
result = await app.AcquireTokenSilentAsync(new string[] { Startup.clientId });
// ...
```

* <span data-ttu-id="a077e-151">hello minta majd felveszi hello eredményül kapott jogkivonat toohello HTTP GET kérést hello `Authorization` fejléc, mely hello feladatlista szolgáltatás használja tooauthenticate hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="a077e-151">hello sample then adds hello resulting token toohello HTTP GET request as hello `Authorization` header, which hello To-Do List service uses tooauthenticate hello request.</span></span>
* <span data-ttu-id="a077e-152">Ha hello feladatlista szolgáltatás adja vissza egy `401 Unauthorized` választ, a MSAL hello access_tokens váltak érvénytelen valamilyen okból.</span><span class="sxs-lookup"><span data-stu-id="a077e-152">If hello To-Do List service returns a `401 Unauthorized` response, hello access_tokens in MSAL have become invalid for some reason.</span></span>  <span data-ttu-id="a077e-153">Ebben az esetben engedje el bármilyen access_tokens a hello MSAL gyorsítótár és hello felhasználót, hogy a újra, amely újraindítja hello token beszerzési folyamatot toosign esetleg üzenet megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="a077e-153">In this case, you should drop any access_tokens from hello MSAL cache and show hello user a message that they may need toosign in again, which will restart hello token acquisition flow.</span></span>

```C#
// ...
// If hello call failed with access denied, then drop hello current access token from hello cache,
// and show hello user an error indicating they might need toosign-in again.
if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
        app.AppTokenCache.Clear(Startup.clientId);

        return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need toosign in again.");
}
// ...
```

* <span data-ttu-id="a077e-154">Hasonlóképpen ha MSAL nem tooreturn egy access_token bármilyen okból, meg kell kérni a hello felhasználói toosign a újra.</span><span class="sxs-lookup"><span data-stu-id="a077e-154">Similarly, if MSAL is unable tooreturn an access_token for any reason, you should instruct hello user toosign in again.</span></span>  <span data-ttu-id="a077e-155">Ez egyszerűen bármelyik elfogja az `MSALException`:</span><span class="sxs-lookup"><span data-stu-id="a077e-155">This is as simple as catching any `MSALException`:</span></span>

```C#
// ...
catch (MsalException ee)
{
        // If MSAL could not get a token silently, show hello user an error indicating they might need toosign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading tooDo List: " + ee.Message + " You might need toolog out and log back in.");
}
// ...
```

* <span data-ttu-id="a077e-156">hello pontos azonos `AcquireTokenSilentAsync` tekintendő, amely a hello implementd `Create` és `Delete` műveletek.</span><span class="sxs-lookup"><span data-stu-id="a077e-156">hello exact same `AcquireTokenSilentAsync` call is implementd in hello `Create` and `Delete` actions.</span></span>  <span data-ttu-id="a077e-157">Webalkalmazások a MSAL metódus tooget access_tokens is használhatja, amikor az alkalmazás kell.</span><span class="sxs-lookup"><span data-stu-id="a077e-157">In web apps, you can use this MSAL method tooget access_tokens whenever you need them in your app.</span></span>  <span data-ttu-id="a077e-158">MSAL beszerzése, gyorsítótárazás és jogkivonatok frissítése az Ön kezeli.</span><span class="sxs-lookup"><span data-stu-id="a077e-158">MSAL will take care of acquiring, caching, and refreshing tokens for you.</span></span>

<span data-ttu-id="a077e-159">Végül felépítéséhez és az alkalmazás futtatása!</span><span class="sxs-lookup"><span data-stu-id="a077e-159">Finally, build and run your app!</span></span>  <span data-ttu-id="a077e-160">Jelentkezzen be Microsoft-Account vagy egy Azure AD-fiókot, és figyelje meg, hogyan hello felhasználói identitás hello felső navigációs sáv megjelenik.</span><span class="sxs-lookup"><span data-stu-id="a077e-160">Sign in with either a Microsoft Account or an Azure AD Account, and notice how hello user's identity is reflected in hello top navigation bar.</span></span>  <span data-ttu-id="a077e-161">Adja hozzá, és töröljön néhány elemet hello felhasználói feladatlistát OAuth 2.0 API védett toosee hello meghívja a működés közben.</span><span class="sxs-lookup"><span data-stu-id="a077e-161">Add and delete some items from hello user's To-Do List toosee hello OAuth 2.0 secured API calls in action.</span></span>  <span data-ttu-id="a077e-162">Most már rendelkezik egy webalkalmazás & webes API egyaránt az iparági szabványos protokollok, amely képes hitelesíteni a személyes és munkahelyi vagy iskolai fiókkal rendelkező felhasználók használatával biztonságossá.</span><span class="sxs-lookup"><span data-stu-id="a077e-162">You now have a web app & web API, both secured using industry standard protocols, that can authenticate users with both their personal and work/school accounts.</span></span>

<span data-ttu-id="a077e-163">Referenciaként hello befejeződött (a konfigurációs értékek nélkül) minta [itt megadott](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="a077e-163">For reference, hello completed sample (without your configuration values) [is provided here](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).</span></span>  

## <a name="next-steps"></a><span data-ttu-id="a077e-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a077e-164">Next Steps</span></span>
<span data-ttu-id="a077e-165">További forrásokért tekintse meg:</span><span class="sxs-lookup"><span data-stu-id="a077e-165">For additional resources, check out:</span></span>

* [<span data-ttu-id="a077e-166">hello v2.0 – útmutató fejlesztőknek >></span><span class="sxs-lookup"><span data-stu-id="a077e-166">hello v2.0 developer guide >></span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="a077e-167">StackOverflow "azure-active-directory" címke >></span><span class="sxs-lookup"><span data-stu-id="a077e-167">StackOverflow "azure-active-directory" tag >></span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="a077e-168">Biztonsági frissítések termékeinkhez</span><span class="sxs-lookup"><span data-stu-id="a077e-168">Get security updates for our products</span></span>
<span data-ttu-id="a077e-169">Javasoljuk, tooget értesítést a bekövetkező biztonsági incidensekről ellátogatva [ezen a lapon](https://technet.microsoft.com/security/dd252948) és előfizetés tooSecurity tanácsadói riasztásokra.</span><span class="sxs-lookup"><span data-stu-id="a077e-169">We encourage you tooget notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing tooSecurity Advisory Alerts.</span></span>

