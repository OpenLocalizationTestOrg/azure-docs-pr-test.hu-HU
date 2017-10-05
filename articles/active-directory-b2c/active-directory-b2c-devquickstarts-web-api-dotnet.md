---
title: Az Azure Active Directory B2C |} Microsoft Docs
description: "Hogyan kell a .NET-webalkalmazás létrehozása, és hívja meg a webes api Azure Active Directory B2C és az OAuth 2.0 hozzáférési jogkivonatok használatával."
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
ms.openlocfilehash: 48452eb68f826d1c7aa61d5e5531f941ac1422b0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-ad-b2c-call-a-net-web-api-from-a-net-web-app"></a><span data-ttu-id="ff110-103">Az Azure AD B2C: .NET webes API-hívás .NET-webalkalmazásból</span><span class="sxs-lookup"><span data-stu-id="ff110-103">Azure AD B2C: Call a .NET web API from a .NET web app</span></span>

<span data-ttu-id="ff110-104">Az Azure AD B2C segítségével hatékony identitáskezelési funkciókat adhat hozzá a webalkalmazások és webes API-k.</span><span class="sxs-lookup"><span data-stu-id="ff110-104">By using Azure AD B2C, you can add powerful identity management features to your web apps and web APIs.</span></span> <span data-ttu-id="ff110-105">A cikkből megtudhatja, hogyan kérjen hozzáférési jogkivonatok és ellenőrizze a .NET "Feladatlista" webes alkalmazás a .NET webes API-t.</span><span class="sxs-lookup"><span data-stu-id="ff110-105">This article discusses how to request access tokens and make calls from a .NET "to-do list" web app to a .NET web api.</span></span>

<span data-ttu-id="ff110-106">Ez a cikk nem foglalkozik megvalósítható bejelentkezési, regisztrációs és profilkezelési Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="ff110-106">This article does not cover how to implement sign-in, sign-up and profile management with Azure AD B2C.</span></span> <span data-ttu-id="ff110-107">A cikk foglalkozik hívó Web API-kat a felhasználó már hitelesítése után.</span><span class="sxs-lookup"><span data-stu-id="ff110-107">It focuses on calling web APIs after the user is already authenticated.</span></span> <span data-ttu-id="ff110-108">Ha még nem tette meg, a következőket:</span><span class="sxs-lookup"><span data-stu-id="ff110-108">If you haven't already, you should:</span></span>

* <span data-ttu-id="ff110-109">Első lépések egy [.NET-webalkalmazás](active-directory-b2c-devquickstarts-web-dotnet-susi.md)</span><span class="sxs-lookup"><span data-stu-id="ff110-109">Get started with a [.NET web app](active-directory-b2c-devquickstarts-web-dotnet-susi.md)</span></span>
* <span data-ttu-id="ff110-110">Első lépések egy [.NET webes API-t](active-directory-b2c-devquickstarts-api-dotnet.md)</span><span class="sxs-lookup"><span data-stu-id="ff110-110">Get started with a [.NET web api](active-directory-b2c-devquickstarts-api-dotnet.md)</span></span>

## <a name="prerequisite"></a><span data-ttu-id="ff110-111">Előfeltétel</span><span class="sxs-lookup"><span data-stu-id="ff110-111">Prerequisite</span></span>

<span data-ttu-id="ff110-112">Hozható létre olyan webes alkalmazás, amely behívja a webes API-t kell:</span><span class="sxs-lookup"><span data-stu-id="ff110-112">To build a web application that calls a web api, you need to:</span></span>

1. <span data-ttu-id="ff110-113">[Az Azure AD B2C bérlő létrehozása](active-directory-b2c-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ff110-113">[Create an Azure AD B2C tenant](active-directory-b2c-get-started.md).</span></span>
2. <span data-ttu-id="ff110-114">[Regisztrálja a webes api](active-directory-b2c-app-registration.md#register-a-web-api).</span><span class="sxs-lookup"><span data-stu-id="ff110-114">[Register a web api](active-directory-b2c-app-registration.md#register-a-web-api).</span></span>
3. <span data-ttu-id="ff110-115">[A webes alkalmazás regisztrálása](active-directory-b2c-app-registration.md#register-a-web-app).</span><span class="sxs-lookup"><span data-stu-id="ff110-115">[Register a web app](active-directory-b2c-app-registration.md#register-a-web-app).</span></span>
4. <span data-ttu-id="ff110-116">[Házirendek beállítása](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="ff110-116">[Set up policies](active-directory-b2c-reference-policies.md).</span></span>
5. <span data-ttu-id="ff110-117">[Engedélyeket kap a webes alkalmazás használja a web api](active-directory-b2c-access-tokens.md#publishing-permissions).</span><span class="sxs-lookup"><span data-stu-id="ff110-117">[Grant the web app permissions to use the web api](active-directory-b2c-access-tokens.md#publishing-permissions).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ff110-118">Az ügyfélalkalmazásnak és a webes API-nak ugyanazt az Azure AD B2C könyvtárat kell használnia.</span><span class="sxs-lookup"><span data-stu-id="ff110-118">The client application and web API must use the same Azure AD B2C directory.</span></span>
>

## <a name="download-the-code"></a><span data-ttu-id="ff110-119">A kód letöltése</span><span class="sxs-lookup"><span data-stu-id="ff110-119">Download the code</span></span>

<span data-ttu-id="ff110-120">Az oktatóanyag kódjának kezelése a [GitHubon](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi) történik.</span><span class="sxs-lookup"><span data-stu-id="ff110-120">The code for this tutorial is maintained on [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span></span> <span data-ttu-id="ff110-121">A minta klónozásához futtassa a következőt:</span><span class="sxs-lookup"><span data-stu-id="ff110-121">You can clone the sample by running:</span></span>

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

<span data-ttu-id="ff110-122">Miután letöltötte a mintakódot, nyissa meg a Visual Studio .sln fájlt a kezdéshez.</span><span class="sxs-lookup"><span data-stu-id="ff110-122">After you download the sample code, open the Visual Studio .sln file to get started.</span></span> <span data-ttu-id="ff110-123">A megoldásfájl két projektet tartalmaz: `TaskWebApp` és `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="ff110-123">The solution file contains two projects: `TaskWebApp` and `TaskService`.</span></span> <span data-ttu-id="ff110-124">`TaskWebApp`a felhasználó kommunikál MVC webalkalmazás van.</span><span class="sxs-lookup"><span data-stu-id="ff110-124">`TaskWebApp` is a MVC web application that the user interacts with.</span></span> <span data-ttu-id="ff110-125">A `TaskService` az alkalmazás webes API háttérszolgáltatása, amely tárolja a felhasználók feladatlistáit.</span><span class="sxs-lookup"><span data-stu-id="ff110-125">`TaskService` is the app's back-end web API that stores each user's to-do list.</span></span> <span data-ttu-id="ff110-126">Ez a cikk nem tér ki az épület a `TaskWebApp` webes alkalmazás vagy a `TaskService` webes API-t.</span><span class="sxs-lookup"><span data-stu-id="ff110-126">This article does not cover building the `TaskWebApp` web app or the `TaskService` web api.</span></span> <span data-ttu-id="ff110-127">Megtudhatja, hogyan hozhat létre az Azure AD B2C segítségével .NET web app, tekintse meg a [.NET web app oktatóanyag](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span><span class="sxs-lookup"><span data-stu-id="ff110-127">To learn how to build the .NET web app using Azure AD B2C, see our [.NET web app tutorial](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span></span> <span data-ttu-id="ff110-128">Megtudhatja, hogyan hozhat létre a .NET webes API-t az Azure AD B2C használatával biztonságossá téve, tekintse meg a [.NET webes API-k oktatóanyag](active-directory-b2c-devquickstarts-api-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="ff110-128">To learn how to build the .NET web API secured using Azure AD B2C, see our [.NET web API tutorial](active-directory-b2c-devquickstarts-api-dotnet.md).</span></span>

### <a name="update-the-azure-ad-b2c-configuration"></a><span data-ttu-id="ff110-129">Az Azure AD B2C konfiguráció frissítése</span><span class="sxs-lookup"><span data-stu-id="ff110-129">Update the Azure AD B2C configuration</span></span>

<span data-ttu-id="ff110-130">A minta úgy van konfigurálva, hogy a bemutató bérlőnk házirendjeit és ügyfél-azonosítóját használja.</span><span class="sxs-lookup"><span data-stu-id="ff110-130">Our sample is configured to use the policies and client ID of our demo tenant.</span></span> <span data-ttu-id="ff110-131">Ha azt szeretné, használhatja a saját bérlőt:</span><span class="sxs-lookup"><span data-stu-id="ff110-131">If you would like to use your own tenant:</span></span>

1. <span data-ttu-id="ff110-132">Nyissa meg a `web.config` fájlt a `TaskService` projektben, és a következőképpen cserélje le az értékeket:</span><span class="sxs-lookup"><span data-stu-id="ff110-132">Open `web.config` in the `TaskService` project and replace the values for</span></span>

    * <span data-ttu-id="ff110-133">Az `ida:Tenant` helyett szerepeljen a bérlő neve</span><span class="sxs-lookup"><span data-stu-id="ff110-133">`ida:Tenant` with your tenant name</span></span>
    * <span data-ttu-id="ff110-134">`ida:ClientId`a webes api-alkalmazás azonosítójú</span><span class="sxs-lookup"><span data-stu-id="ff110-134">`ida:ClientId` with your web api application ID</span></span>
    * <span data-ttu-id="ff110-135">Az `ida:SignUpSignInPolicyId` helyett szerepeljen a „regisztrálási vagy bejelentkezési” házirend neve</span><span class="sxs-lookup"><span data-stu-id="ff110-135">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>

2. <span data-ttu-id="ff110-136">Nyissa meg a `web.config` fájlt a `TaskWebApp` projektben, és a következőképpen cserélje le az értékeket:</span><span class="sxs-lookup"><span data-stu-id="ff110-136">Open `web.config` in the `TaskWebApp` project and replace the values for</span></span>

    * <span data-ttu-id="ff110-137">Az `ida:Tenant` helyett szerepeljen a bérlő neve</span><span class="sxs-lookup"><span data-stu-id="ff110-137">`ida:Tenant` with your tenant name</span></span>
    * <span data-ttu-id="ff110-138">Az `ida:ClientId` helyett szerepeljen a webapp alkalmazásazonosítója</span><span class="sxs-lookup"><span data-stu-id="ff110-138">`ida:ClientId` with your web app application ID</span></span>
    * <span data-ttu-id="ff110-139">Az `ida:ClientSecret` helyett szerepeljen a webapp titkos kulcsa</span><span class="sxs-lookup"><span data-stu-id="ff110-139">`ida:ClientSecret` with your web app secret key</span></span>
    * <span data-ttu-id="ff110-140">Az `ida:SignUpSignInPolicyId` helyett szerepeljen a „regisztrálási vagy bejelentkezési” házirend neve</span><span class="sxs-lookup"><span data-stu-id="ff110-140">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>
    * <span data-ttu-id="ff110-141">Az `ida:EditProfilePolicyId` helyett szerepeljen a „profil szerkesztése” házirend neve</span><span class="sxs-lookup"><span data-stu-id="ff110-141">`ida:EditProfilePolicyId` with your "Edit Profile" policy name</span></span>
    * <span data-ttu-id="ff110-142">Az `ida:ResetPasswordPolicyId` helyett szerepeljen a „Jelszó alaphelyzetbe állítása” házirend neve</span><span class="sxs-lookup"><span data-stu-id="ff110-142">`ida:ResetPasswordPolicyId` with your "Reset Password" policy name</span></span>



## <a name="requesting-and-saving-an-access-token"></a><span data-ttu-id="ff110-143">A kért és mentése egy hozzáférési jogkivonatot:</span><span class="sxs-lookup"><span data-stu-id="ff110-143">Requesting and saving an access token</span></span>

### <a name="specify-the-permissions"></a><span data-ttu-id="ff110-144">Az engedélyek megadásához</span><span class="sxs-lookup"><span data-stu-id="ff110-144">Specify the permissions</span></span>

<span data-ttu-id="ff110-145">Annak érdekében, hogy a webes API-hívás, szeretné hitelesíteni a felhasználót (a sign-Close-Up/bejelentkezési házirend használatával) és [olyan hozzáférési jogkivonatot kap](active-directory-b2c-access-tokens.md) az Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="ff110-145">In order to make the call to the web API, you need to authenticate the user (using your sign-up/sign-in policy) and [receive an access token](active-directory-b2c-access-tokens.md) from Azure AD B2C.</span></span> <span data-ttu-id="ff110-146">Ahhoz, hogy olyan hozzáférési jogkivonatot kap, először meg kell azt szeretné, hogy a hozzáférési jogkivonat megadását az engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="ff110-146">In order to receive an access token, you first must specify the permissions you would like the access token to grant.</span></span> <span data-ttu-id="ff110-147">Az engedélyek vannak megadva a `scope` paraméter, amikor a kérést a `/authorize` végpont.</span><span class="sxs-lookup"><span data-stu-id="ff110-147">The permissions are specified in the `scope` parameter when you make the request to the `/authorize` endpoint.</span></span> <span data-ttu-id="ff110-148">Ahhoz például, hogy egy hozzáférési jogkivonat App ID URI-azonosítója az erőforrás alkalmazás "olvasási" engedéllyel rendelkező `https://contoso.onmicrosoft.com/tasks`, a hatókör lenne `https://contoso.onmicrosoft.com/tasks/read`.</span><span class="sxs-lookup"><span data-stu-id="ff110-148">For example, to acquire an access token with the “read” permission to the resource application that has the App ID URI of `https://contoso.onmicrosoft.com/tasks`, the scope would be `https://contoso.onmicrosoft.com/tasks/read`.</span></span>

<span data-ttu-id="ff110-149">A hatókör a mintában szereplő, nyissa meg a fájlt `App_Start\Startup.Auth.cs` , és adja meg a `Scope` OpenIdConnectAuthenticationOptions változóját.</span><span class="sxs-lookup"><span data-stu-id="ff110-149">To specify the scope in our sample, open the file `App_Start\Startup.Auth.cs` and define the `Scope` variable in OpenIdConnectAuthenticationOptions.</span></span>

```CSharp
// App_Start\Startup.Auth.cs

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            ...

            // Specify the scope by appending all of the scopes requested into one string (seperated by a blank space)
            Scope = $"{OpenIdConnectScopes.OpenId} {ReadTasksScope} {WriteTasksScope}"
        }
    );
}
```

### <a name="exchange-the-authorization-code-for-an-access-token"></a><span data-ttu-id="ff110-150">Exchange-hozzáférési tokent az engedélyezési kódot</span><span class="sxs-lookup"><span data-stu-id="ff110-150">Exchange the authorization code for an access token</span></span>

<span data-ttu-id="ff110-151">Egy felhasználót a regisztráció és a bejelentkezési élmény befejezése után az alkalmazás kap egy engedélyezési kód az Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="ff110-151">After an user completes the sign-up or sign-in experience, your app will receive an authorization code from Azure AD B2C.</span></span> <span data-ttu-id="ff110-152">Az OWIN OpenID Connect köztes fogja tárolni a kódot, de nem cserélnek azt a hozzáférési tokent.</span><span class="sxs-lookup"><span data-stu-id="ff110-152">The OWIN OpenID Connect middleware will store the code, but will not exchange it for an access token.</span></span> <span data-ttu-id="ff110-153">Használhatja a [MSAL könyvtár](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) az exchange végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="ff110-153">You can use the [MSAL library](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) to make the exchange.</span></span> <span data-ttu-id="ff110-154">A mintában szereplő konfigurált a lekérdezésértesítési visszahívást az OpenID Connect köztes be, amikor egy engedélyezési kód érkezik.</span><span class="sxs-lookup"><span data-stu-id="ff110-154">In our sample, we configured a notification callback into the OpenID Connect middleware whenever an authorization code is received.</span></span> <span data-ttu-id="ff110-155">A visszahívási MSAL használjuk a jogkivonat kód csere, és menteni a jogkivonatot a gyorsítótárba.</span><span class="sxs-lookup"><span data-stu-id="ff110-155">In the callback, we use MSAL to exchange the code for a token and save the token into the cache.</span></span>

```CSharp
/*
* Callback function when an authorization code is received
*/
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
    // Extract the code from the response notification
    var code = notification.Code;

    var userObjectId = notification.AuthenticationTicket.Identity.FindFirst(ObjectIdElement).Value;
    var authority = String.Format(AadInstance, Tenant, DefaultPolicy);
    var httpContext = notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase;

    // Exchange the code for a token. Make sure to specify the necessary scopes
    ClientCredential cred = new ClientCredential(ClientSecret);
    ConfidentialClientApplication app = new ConfidentialClientApplication(authority, Startup.ClientId,
                                            RedirectUri, cred, new NaiveSessionCache(userObjectId, httpContext));
    var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { ReadTasksScope, WriteTasksScope }, code, DefaultPolicy);
}
```

## <a name="calling-the-web-api"></a><span data-ttu-id="ff110-156">A webes API meghívása</span><span class="sxs-lookup"><span data-stu-id="ff110-156">Calling the web API</span></span>

<span data-ttu-id="ff110-157">Ez a szakasz ismerteti, amelyek használata során kapott jogkivonat sign-Close-Up/bejelentkezés az Azure AD B2C a webes API-k eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="ff110-157">This section discusses how to use the token received during sign-up/sign-in with Azure AD B2C in order to access the web API.</span></span>

### <a name="retrieve-the-saved-token-in-the-controllers"></a><span data-ttu-id="ff110-158">A mentett lexikális elem szerepel a vezérlők beolvasása</span><span class="sxs-lookup"><span data-stu-id="ff110-158">Retrieve the saved token in the controllers</span></span>

<span data-ttu-id="ff110-159">A `TasksController` felelős kommunikál a webes API-t és HTTP-kérelmek küldéséhez az API használatával olvasását, létrehozását és törölheti a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="ff110-159">The `TasksController` is responsible for communicating with the web API and for sending HTTP requests to the API to read, create, and delete tasks.</span></span> <span data-ttu-id="ff110-160">Az API-t az Azure AD B2C által védett, mert kell először kérjen le a jogkivonatot az előző lépésben mentett.</span><span class="sxs-lookup"><span data-stu-id="ff110-160">Because the API is secured by Azure AD B2C, you need to first retrieve the token you saved in the above step.</span></span>

```CSharp
// Controllers\TasksController.cs

/*
* Uses MSAL to retrieve the token from the cache
*/
private async void acquireToken(String[] scope)
{
    string userObjectID = ClaimsPrincipal.Current.FindFirst(Startup.ObjectIdElement).Value;
    string authority = String.Format(Startup.AadInstance, Startup.Tenant, Startup.DefaultPolicy);

    ClientCredential credential = new ClientCredential(Startup.ClientSecret);

    // Retrieve the token using the provided scopes
    ConfidentialClientApplication app = new ConfidentialClientApplication(authority, Startup.ClientId,
                                        Startup.RedirectUri, credential,
                                        new NaiveSessionCache(userObjectID, this.HttpContext));
    AuthenticationResult result = await app.AcquireTokenSilentAsync(scope);

    accessToken = result.Token;
}
```

### <a name="read-tasks-from-the-web-api"></a><span data-ttu-id="ff110-161">Feladatok olvasni a webes API-k</span><span class="sxs-lookup"><span data-stu-id="ff110-161">Read tasks from the web API</span></span>

<span data-ttu-id="ff110-162">Ha megkapta a jogkivonatot, csatolhat azt a HTTP `GET` kérheti a `Authorization` biztonságosan hívására fejléc `TaskService`:</span><span class="sxs-lookup"><span data-stu-id="ff110-162">When you have a token, you can attach it to the HTTP `GET` request in the `Authorization` header to securely call `TaskService`:</span></span>

```CSharp
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try {

        // Retrieve the token with the specified scopes
        acquireToken(new string[] { Startup.ReadTasksScope });

        HttpClient client = new HttpClient();
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, apiEndpoint);

        // Add token to the Authorization header and make the request
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
        HttpResponseMessage response = await client.SendAsync(request);

        // Handle response ...
}

```

### <a name="create-and-delete-tasks-on-the-web-api"></a><span data-ttu-id="ff110-163">Hozzon létre vagy töröljön a webes API-k feladatai</span><span class="sxs-lookup"><span data-stu-id="ff110-163">Create and delete tasks on the web API</span></span>

<span data-ttu-id="ff110-164">Ugyanezt a mintát követik küldésekor `POST` és `DELETE` kéréseket a webes API-hoz, MSAL segítségével kéri le a hozzáférési jogkivonatot a gyorsítótárból.</span><span class="sxs-lookup"><span data-stu-id="ff110-164">Follow the same pattern when you send `POST` and `DELETE` requests to the web API, using MSAL to retrieve the access token from the cache.</span></span>

## <a name="run-the-sample-app"></a><span data-ttu-id="ff110-165">Mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="ff110-165">Run the sample app</span></span>

<span data-ttu-id="ff110-166">Végezetül hozza létre, és mindkét az alkalmazások futtatása.</span><span class="sxs-lookup"><span data-stu-id="ff110-166">Finally, build and run both the apps.</span></span> <span data-ttu-id="ff110-167">Regisztráció és bejelentkezés, és hozzon létre feladatokat a bejelentkezett felhasználó.</span><span class="sxs-lookup"><span data-stu-id="ff110-167">Sign up and sign in, and create tasks for the signed-in user.</span></span> <span data-ttu-id="ff110-168">Kijelentkezés és bejelentkezés másik felhasználóként.</span><span class="sxs-lookup"><span data-stu-id="ff110-168">Sign out and sign in as a different user.</span></span> <span data-ttu-id="ff110-169">Hozzon létre feladatokat ennek a felhasználónak.</span><span class="sxs-lookup"><span data-stu-id="ff110-169">Create tasks for that user.</span></span> <span data-ttu-id="ff110-170">Figyelje meg, hogyan feladatok felhasználónként tárolja a API, mivel az API kinyeri a felhasználó identitását a jogkivonatból kap.</span><span class="sxs-lookup"><span data-stu-id="ff110-170">Notice how the tasks are stored per-user on the API, because the API extracts the user's identity from the token it receives.</span></span> <span data-ttu-id="ff110-171">Próbálja meg is játszik és a hatókörök.</span><span class="sxs-lookup"><span data-stu-id="ff110-171">Also try playing with the scopes.</span></span> <span data-ttu-id="ff110-172">Távolítsa el a "write", és próbálja meg újból felvenni a feladatok engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="ff110-172">Remove the permission to "write" and then try adding a task.</span></span> <span data-ttu-id="ff110-173">Ne feledje Kijelentkezés a hatókör minden módosításakor.</span><span class="sxs-lookup"><span data-stu-id="ff110-173">Just make sure to sign out each time you change the scope.</span></span>

