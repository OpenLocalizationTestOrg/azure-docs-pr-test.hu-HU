---
title: Active Directory B2C aaaAzure |} Microsoft Docs
description: "Hogyan toobuild .NET webalkalmazás és hívja meg a webes api Azure Active Directory B2C és az OAuth 2.0 hozzáférési jogkivonatok használatával."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: krassk
editor: 
ms.assetid: d3888556-2647-4a42-b068-027f9374aa61
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/17/2017
ms.author: parakhj
ms.openlocfilehash: 9b248e3bf18968e12aae73c07083fa8278befb3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-call-a-net-web-api-from-a-net-web-app"></a><span data-ttu-id="16e0e-103">Az Azure AD B2C: .NET webes API-hívás .NET-webalkalmazásból</span><span class="sxs-lookup"><span data-stu-id="16e0e-103">Azure AD B2C: Call a .NET web API from a .NET web app</span></span>

<span data-ttu-id="16e0e-104">Az Azure AD B2C segítségével hatékony identity management szolgáltatások tooyour webalkalmazások és webes API-kat is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="16e0e-104">By using Azure AD B2C, you can add powerful identity management features tooyour web apps and web APIs.</span></span> <span data-ttu-id="16e0e-105">Ez a cikk ismerteti, hogyan toorequest hozzáférési jogkivonatok, valamint a hívásokat a .NET-"Feladatlista" webes alkalmazás tooa .NET webes API-t.</span><span class="sxs-lookup"><span data-stu-id="16e0e-105">This article discusses how toorequest access tokens and make calls from a .NET "to-do list" web app tooa .NET web api.</span></span>

<span data-ttu-id="16e0e-106">Ez a cikk nem foglalkozik hogyan tooimplement bejelentkezés, regisztrációs profil-kezelés az Azure AD B2C és.</span><span class="sxs-lookup"><span data-stu-id="16e0e-106">This article does not cover how tooimplement sign-in, sign-up and profile management with Azure AD B2C.</span></span> <span data-ttu-id="16e0e-107">A cikk foglalkozik a hívó webes API-k hello felhasználó már hitelesítése után.</span><span class="sxs-lookup"><span data-stu-id="16e0e-107">It focuses on calling web APIs after hello user is already authenticated.</span></span> <span data-ttu-id="16e0e-108">Ha még nem tette meg, a következőket:</span><span class="sxs-lookup"><span data-stu-id="16e0e-108">If you haven't already, you should:</span></span>

* <span data-ttu-id="16e0e-109">Első lépések egy [.NET-webalkalmazás](active-directory-b2c-devquickstarts-web-dotnet-susi.md)</span><span class="sxs-lookup"><span data-stu-id="16e0e-109">Get started with a [.NET web app](active-directory-b2c-devquickstarts-web-dotnet-susi.md)</span></span>
* <span data-ttu-id="16e0e-110">Első lépések egy [.NET webes API-t](active-directory-b2c-devquickstarts-api-dotnet.md)</span><span class="sxs-lookup"><span data-stu-id="16e0e-110">Get started with a [.NET web api](active-directory-b2c-devquickstarts-api-dotnet.md)</span></span>

## <a name="prerequisite"></a><span data-ttu-id="16e0e-111">Előfeltétel</span><span class="sxs-lookup"><span data-stu-id="16e0e-111">Prerequisite</span></span>

<span data-ttu-id="16e0e-112">toobuild egy webes alkalmazás, amely meghívja a webes API-t kell:</span><span class="sxs-lookup"><span data-stu-id="16e0e-112">toobuild a web application that calls a web api, you need to:</span></span>

1. <span data-ttu-id="16e0e-113">[Az Azure AD B2C bérlő létrehozása](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="16e0e-113">[Create an Azure AD B2C tenant](active-directory-b2c-get-started.md).</span></span>
2. <span data-ttu-id="16e0e-114">[Regisztrálja a webes api](active-directory-b2c-app-registration.md#register-a-web-api).</span><span class="sxs-lookup"><span data-stu-id="16e0e-114">[Register a web api](active-directory-b2c-app-registration.md#register-a-web-api).</span></span>
3. <span data-ttu-id="16e0e-115">[A webes alkalmazás regisztrálása](active-directory-b2c-app-registration.md#register-a-web-app).</span><span class="sxs-lookup"><span data-stu-id="16e0e-115">[Register a web app](active-directory-b2c-app-registration.md#register-a-web-app).</span></span>
4. <span data-ttu-id="16e0e-116">[Házirendek beállítása](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="16e0e-116">[Set up policies](active-directory-b2c-reference-policies.md).</span></span>
5. <span data-ttu-id="16e0e-117">[Támogatás hello web app engedélyek toouse hello webes API-t](active-directory-b2c-access-tokens.md#publishing-permissions).</span><span class="sxs-lookup"><span data-stu-id="16e0e-117">[Grant hello web app permissions toouse hello web api](active-directory-b2c-access-tokens.md#publishing-permissions).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="16e0e-118">hello ügyfélalkalmazást és a webes API hello ugyanazt az Azure AD B2C-címtárban kell használnia.</span><span class="sxs-lookup"><span data-stu-id="16e0e-118">hello client application and web API must use hello same Azure AD B2C directory.</span></span>
>

## <a name="download-hello-code"></a><span data-ttu-id="16e0e-119">Hello kód letöltése</span><span class="sxs-lookup"><span data-stu-id="16e0e-119">Download hello code</span></span>

<span data-ttu-id="16e0e-120">az oktatóanyag kódjának hello fenntartott [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span><span class="sxs-lookup"><span data-stu-id="16e0e-120">hello code for this tutorial is maintained on [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span></span> <span data-ttu-id="16e0e-121">Hello minta klónozhat futtatásával:</span><span class="sxs-lookup"><span data-stu-id="16e0e-121">You can clone hello sample by running:</span></span>

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

<span data-ttu-id="16e0e-122">Hello mintakód letöltése után nyissa meg hello Visual Studio .sln fájlt tooget elindult.</span><span class="sxs-lookup"><span data-stu-id="16e0e-122">After you download hello sample code, open hello Visual Studio .sln file tooget started.</span></span> <span data-ttu-id="16e0e-123">hello megoldásfájl két projektet tartalmaz: `TaskWebApp` és `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="16e0e-123">hello solution file contains two projects: `TaskWebApp` and `TaskService`.</span></span> <span data-ttu-id="16e0e-124">`TaskWebApp`az MVC webalkalmazás hello felhasználó kommunikál.</span><span class="sxs-lookup"><span data-stu-id="16e0e-124">`TaskWebApp` is a MVC web application that hello user interacts with.</span></span> <span data-ttu-id="16e0e-125">`TaskService`minden felhasználói feladatlistát tároló hello app webes API van.</span><span class="sxs-lookup"><span data-stu-id="16e0e-125">`TaskService` is hello app's back-end web API that stores each user's to-do list.</span></span> <span data-ttu-id="16e0e-126">Ez a cikk nem tér ki az épület hello `TaskWebApp` webalkalmazás vagy hello `TaskService` webes API-t.</span><span class="sxs-lookup"><span data-stu-id="16e0e-126">This article does not cover building hello `TaskWebApp` web app or hello `TaskService` web api.</span></span> <span data-ttu-id="16e0e-127">toolearn hogyan toobuild hello .NET webalkalmazás-alkalmazásokhoz az Azure AD B2C használatával tekintse meg a [.NET web app oktatóanyag](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span><span class="sxs-lookup"><span data-stu-id="16e0e-127">toolearn how toobuild hello .NET web app using Azure AD B2C, see our [.NET web app tutorial](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span></span> <span data-ttu-id="16e0e-128">Hogyan toobuild hello .NET webes API-t az Azure AD B2C használatával biztonságossá toolearn tekintse meg a [.NET webes API-k oktatóanyag](active-directory-b2c-devquickstarts-api-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="16e0e-128">toolearn how toobuild hello .NET web API secured using Azure AD B2C, see our [.NET web API tutorial](active-directory-b2c-devquickstarts-api-dotnet.md).</span></span>

### <a name="update-hello-azure-ad-b2c-configuration"></a><span data-ttu-id="16e0e-129">Az Azure AD B2C hello konfiguráció frissítése</span><span class="sxs-lookup"><span data-stu-id="16e0e-129">Update hello Azure AD B2C configuration</span></span>

<span data-ttu-id="16e0e-130">A minta rendszer konfigurált toouse hello házirendek és az ügyfél-Azonosítóját a bemutató bérlő.</span><span class="sxs-lookup"><span data-stu-id="16e0e-130">Our sample is configured toouse hello policies and client ID of our demo tenant.</span></span> <span data-ttu-id="16e0e-131">Ha azt szeretné, hogy toouse saját bérlőt:</span><span class="sxs-lookup"><span data-stu-id="16e0e-131">If you would like toouse your own tenant:</span></span>

1. <span data-ttu-id="16e0e-132">Nyissa meg `web.config` a hello `TaskService` projektre, és cserélje le a hello értékei</span><span class="sxs-lookup"><span data-stu-id="16e0e-132">Open `web.config` in hello `TaskService` project and replace hello values for</span></span>

    * <span data-ttu-id="16e0e-133">Az `ida:Tenant` helyett szerepeljen a bérlő neve</span><span class="sxs-lookup"><span data-stu-id="16e0e-133">`ida:Tenant` with your tenant name</span></span>
    * <span data-ttu-id="16e0e-134">`ida:ClientId`a webes api-alkalmazás azonosítójú</span><span class="sxs-lookup"><span data-stu-id="16e0e-134">`ida:ClientId` with your web api application ID</span></span>
    * <span data-ttu-id="16e0e-135">Az `ida:SignUpSignInPolicyId` helyett szerepeljen a „regisztrálási vagy bejelentkezési” házirend neve</span><span class="sxs-lookup"><span data-stu-id="16e0e-135">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>

2. <span data-ttu-id="16e0e-136">Nyissa meg `web.config` a hello `TaskWebApp` projektre, és cserélje le a hello értékei</span><span class="sxs-lookup"><span data-stu-id="16e0e-136">Open `web.config` in hello `TaskWebApp` project and replace hello values for</span></span>

    * <span data-ttu-id="16e0e-137">Az `ida:Tenant` helyett szerepeljen a bérlő neve</span><span class="sxs-lookup"><span data-stu-id="16e0e-137">`ida:Tenant` with your tenant name</span></span>
    * <span data-ttu-id="16e0e-138">Az `ida:ClientId` helyett szerepeljen a webapp alkalmazásazonosítója</span><span class="sxs-lookup"><span data-stu-id="16e0e-138">`ida:ClientId` with your web app application ID</span></span>
    * <span data-ttu-id="16e0e-139">Az `ida:ClientSecret` helyett szerepeljen a webapp titkos kulcsa</span><span class="sxs-lookup"><span data-stu-id="16e0e-139">`ida:ClientSecret` with your web app secret key</span></span>
    * <span data-ttu-id="16e0e-140">Az `ida:SignUpSignInPolicyId` helyett szerepeljen a „regisztrálási vagy bejelentkezési” házirend neve</span><span class="sxs-lookup"><span data-stu-id="16e0e-140">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>
    * <span data-ttu-id="16e0e-141">Az `ida:EditProfilePolicyId` helyett szerepeljen a „profil szerkesztése” házirend neve</span><span class="sxs-lookup"><span data-stu-id="16e0e-141">`ida:EditProfilePolicyId` with your "Edit Profile" policy name</span></span>
    * <span data-ttu-id="16e0e-142">Az `ida:ResetPasswordPolicyId` helyett szerepeljen a „Jelszó alaphelyzetbe állítása” házirend neve</span><span class="sxs-lookup"><span data-stu-id="16e0e-142">`ida:ResetPasswordPolicyId` with your "Reset Password" policy name</span></span>



## <a name="requesting-and-saving-an-access-token"></a><span data-ttu-id="16e0e-143">A kért és mentése egy hozzáférési jogkivonatot:</span><span class="sxs-lookup"><span data-stu-id="16e0e-143">Requesting and saving an access token</span></span>

### <a name="specify-hello-permissions"></a><span data-ttu-id="16e0e-144">Hello engedélyek megadásához</span><span class="sxs-lookup"><span data-stu-id="16e0e-144">Specify hello permissions</span></span>

<span data-ttu-id="16e0e-145">Rendelés toomake hello hívás toohello webes API-t kell tooauthenticate hello felhasználói (a sign-Close-Up/bejelentkezési házirend használatával) és [olyan hozzáférési jogkivonatot kap](active-directory-b2c-access-tokens.md) az Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="16e0e-145">In order toomake hello call toohello web API, you need tooauthenticate hello user (using your sign-up/sign-in policy) and [receive an access token](active-directory-b2c-access-tokens.md) from Azure AD B2C.</span></span> <span data-ttu-id="16e0e-146">A sorrend tooreceive olyan hozzáférési jogkivonatot először meg kell hello access token toogrant milyen hello engedélyek.</span><span class="sxs-lookup"><span data-stu-id="16e0e-146">In order tooreceive an access token, you first must specify hello permissions you would like hello access token toogrant.</span></span> <span data-ttu-id="16e0e-147">hello engedélyek meg van határozva a hello `scope` paraméter, amikor hello kérelem toohello `/authorize` végpont.</span><span class="sxs-lookup"><span data-stu-id="16e0e-147">hello permissions are specified in hello `scope` parameter when you make hello request toohello `/authorize` endpoint.</span></span> <span data-ttu-id="16e0e-148">Például egy hozzáférési jogkivonat hello "olvasási" engedély toohello erőforrás alkalmazás tooacquire hello App ID URI-azonosítója `https://contoso.onmicrosoft.com/tasks`, hello hatókör lenne `https://contoso.onmicrosoft.com/tasks/read`.</span><span class="sxs-lookup"><span data-stu-id="16e0e-148">For example, tooacquire an access token with hello “read” permission toohello resource application that has hello App ID URI of `https://contoso.onmicrosoft.com/tasks`, hello scope would be `https://contoso.onmicrosoft.com/tasks/read`.</span></span>

<span data-ttu-id="16e0e-149">a minta, nyissa meg hello fájlban toospecify hello hatókör `App_Start\Startup.Auth.cs` , és adja meg a hello `Scope` OpenIdConnectAuthenticationOptions változóját.</span><span class="sxs-lookup"><span data-stu-id="16e0e-149">toospecify hello scope in our sample, open hello file `App_Start\Startup.Auth.cs` and define hello `Scope` variable in OpenIdConnectAuthenticationOptions.</span></span>

```CSharp
// App_Start\Startup.Auth.cs

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            ...

            // Specify hello scope by appending all of hello scopes requested into one string (seperated by a blank space)
            Scope = $"{OpenIdConnectScopes.OpenId} {ReadTasksScope} {WriteTasksScope}"
        }
    );
}
```

### <a name="exchange-hello-authorization-code-for-an-access-token"></a><span data-ttu-id="16e0e-150">Exchange hello engedélyezési kód egy hozzáférési jogkivonatot:</span><span class="sxs-lookup"><span data-stu-id="16e0e-150">Exchange hello authorization code for an access token</span></span>

<span data-ttu-id="16e0e-151">Egy felhasználó hello-előfizetés és a bejelentkezési élmény befejezése után az alkalmazás kap egy engedélyezési kód az Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="16e0e-151">After an user completes hello sign-up or sign-in experience, your app will receive an authorization code from Azure AD B2C.</span></span> <span data-ttu-id="16e0e-152">hello OWIN OpenID Connect köztes hello kódot fogja tárolni, de nem cserélnek azt a hozzáférési tokent.</span><span class="sxs-lookup"><span data-stu-id="16e0e-152">hello OWIN OpenID Connect middleware will store hello code, but will not exchange it for an access token.</span></span> <span data-ttu-id="16e0e-153">Használhatja a hello [MSAL könyvtár](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) toomake hello exchange.</span><span class="sxs-lookup"><span data-stu-id="16e0e-153">You can use hello [MSAL library](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) toomake hello exchange.</span></span> <span data-ttu-id="16e0e-154">A mintában szereplő konfigurált a lekérdezésértesítési visszahívást hello OpenID Connect köztes be, amikor egy engedélyezési kód érkezik.</span><span class="sxs-lookup"><span data-stu-id="16e0e-154">In our sample, we configured a notification callback into hello OpenID Connect middleware whenever an authorization code is received.</span></span> <span data-ttu-id="16e0e-155">A hello visszahívási azt MSAL tooexchange hello kód használata a jogkivonat és mentse hello token hello gyorsítótárába.</span><span class="sxs-lookup"><span data-stu-id="16e0e-155">In hello callback, we use MSAL tooexchange hello code for a token and save hello token into hello cache.</span></span>

```CSharp
/*
* Callback function when an authorization code is received
*/
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
    // Extract hello code from hello response notification
    var code = notification.Code;

    var userObjectId = notification.AuthenticationTicket.Identity.FindFirst(ObjectIdElement).Value;
    var authority = String.Format(AadInstance, Tenant, DefaultPolicy);
    var httpContext = notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase;

    // Exchange hello code for a token. Make sure toospecify hello necessary scopes
    ClientCredential cred = new ClientCredential(ClientSecret);
    ConfidentialClientApplication app = new ConfidentialClientApplication(authority, Startup.ClientId,
                                            RedirectUri, cred, new NaiveSessionCache(userObjectId, httpContext));
    var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { ReadTasksScope, WriteTasksScope }, code, DefaultPolicy);
}
```

## <a name="calling-hello-web-api"></a><span data-ttu-id="16e0e-156">Hello webes API meghívása</span><span class="sxs-lookup"><span data-stu-id="16e0e-156">Calling hello web API</span></span>

<span data-ttu-id="16e0e-157">Ez a szakasz ismerteti, hogyan toouse hello jogkivonatot kapott sign-Close-Up/bejelentkezés az Azure AD B2C sorrendben tooaccess hello webes API-t.</span><span class="sxs-lookup"><span data-stu-id="16e0e-157">This section discusses how toouse hello token received during sign-up/sign-in with Azure AD B2C in order tooaccess hello web API.</span></span>

### <a name="retrieve-hello-saved-token-in-hello-controllers"></a><span data-ttu-id="16e0e-158">A hello, tartományvezérlői mentett hello-jogkivonatot lekérdezni</span><span class="sxs-lookup"><span data-stu-id="16e0e-158">Retrieve hello saved token in hello controllers</span></span>

<span data-ttu-id="16e0e-159">Hello `TasksController` felelős hello webes API-k kommunikál a HTTP-kérelmek toohello API tooread küld, hozzon létre és törölheti a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="16e0e-159">hello `TasksController` is responsible for communicating with hello web API and for sending HTTP requests toohello API tooread, create, and delete tasks.</span></span> <span data-ttu-id="16e0e-160">Mivel hello API védi-e az Azure AD B2C, meg kell toofirst hello/token lekérése a fenti lépés hello mentette.</span><span class="sxs-lookup"><span data-stu-id="16e0e-160">Because hello API is secured by Azure AD B2C, you need toofirst retrieve hello token you saved in hello above step.</span></span>

```CSharp
// Controllers\TasksController.cs

/*
* Uses MSAL tooretrieve hello token from hello cache
*/
private async void acquireToken(String[] scope)
{
    string userObjectID = ClaimsPrincipal.Current.FindFirst(Startup.ObjectIdElement).Value;
    string authority = String.Format(Startup.AadInstance, Startup.Tenant, Startup.DefaultPolicy);

    ClientCredential credential = new ClientCredential(Startup.ClientSecret);

    // Retrieve hello token using hello provided scopes
    ConfidentialClientApplication app = new ConfidentialClientApplication(authority, Startup.ClientId,
                                        Startup.RedirectUri, credential,
                                        new NaiveSessionCache(userObjectID, this.HttpContext));
    AuthenticationResult result = await app.AcquireTokenSilentAsync(scope);

    accessToken = result.Token;
}
```

### <a name="read-tasks-from-hello-web-api"></a><span data-ttu-id="16e0e-161">Feladatok olvasni hello webes API-k</span><span class="sxs-lookup"><span data-stu-id="16e0e-161">Read tasks from hello web API</span></span>

<span data-ttu-id="16e0e-162">Ha megkapta a jogkivonatot, csatolhatók toohello HTTP `GET` hello kérést `Authorization` fejléc toosecurely hívás `TaskService`:</span><span class="sxs-lookup"><span data-stu-id="16e0e-162">When you have a token, you can attach it toohello HTTP `GET` request in hello `Authorization` header toosecurely call `TaskService`:</span></span>

```CSharp
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try {

        // Retrieve hello token with hello specified scopes
        acquireToken(new string[] { Startup.ReadTasksScope });

        HttpClient client = new HttpClient();
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, apiEndpoint);

        // Add token toohello Authorization header and make hello request
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
        HttpResponseMessage response = await client.SendAsync(request);

        // Handle response ...
}

```

### <a name="create-and-delete-tasks-on-hello-web-api"></a><span data-ttu-id="16e0e-163">Hozzon létre vagy töröljön a feladatokat a hello webes API-k</span><span class="sxs-lookup"><span data-stu-id="16e0e-163">Create and delete tasks on hello web API</span></span>

<span data-ttu-id="16e0e-164">Hajtsa végre hello azonos mintát küldésekor `POST` és `DELETE` a hello gyorsítótárban levő MSAL tooretrieve hello hozzáférési jogkivonat kérelmek toohello webes API-t használ.</span><span class="sxs-lookup"><span data-stu-id="16e0e-164">Follow hello same pattern when you send `POST` and `DELETE` requests toohello web API, using MSAL tooretrieve hello access token from hello cache.</span></span>

## <a name="run-hello-sample-app"></a><span data-ttu-id="16e0e-165">Hello mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="16e0e-165">Run hello sample app</span></span>

<span data-ttu-id="16e0e-166">Végezetül hozza létre, és mindkét hello alkalmazásainak futtatásához.</span><span class="sxs-lookup"><span data-stu-id="16e0e-166">Finally, build and run both hello apps.</span></span> <span data-ttu-id="16e0e-167">Regisztráció és bejelentkezés és hozzon létre feladatokat hello bejelentkezett felhasználó.</span><span class="sxs-lookup"><span data-stu-id="16e0e-167">Sign up and sign in, and create tasks for hello signed-in user.</span></span> <span data-ttu-id="16e0e-168">Kijelentkezés és bejelentkezés másik felhasználóként.</span><span class="sxs-lookup"><span data-stu-id="16e0e-168">Sign out and sign in as a different user.</span></span> <span data-ttu-id="16e0e-169">Hozzon létre feladatokat ennek a felhasználónak.</span><span class="sxs-lookup"><span data-stu-id="16e0e-169">Create tasks for that user.</span></span> <span data-ttu-id="16e0e-170">Figyelje meg, hogyan hello feladatok felhasználónként tárolja a hello API, mert hello API hello felhasználói identitás kiolvassa a hello jogkivonatot kap.</span><span class="sxs-lookup"><span data-stu-id="16e0e-170">Notice how hello tasks are stored per-user on hello API, because hello API extracts hello user's identity from hello token it receives.</span></span> <span data-ttu-id="16e0e-171">Próbálja meg is játszott hello hatókörként.</span><span class="sxs-lookup"><span data-stu-id="16e0e-171">Also try playing with hello scopes.</span></span> <span data-ttu-id="16e0e-172">Távolítsa el hello engedélyt túl "write", és próbálja meg újból felvenni a feladatok.</span><span class="sxs-lookup"><span data-stu-id="16e0e-172">Remove hello permission too"write" and then try adding a task.</span></span> <span data-ttu-id="16e0e-173">Csak győződjön meg arról, hogy toosign kimenő hello hatókör minden módosításakor.</span><span class="sxs-lookup"><span data-stu-id="16e0e-173">Just make sure toosign out each time you change hello scope.</span></span>

