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
# <a name="azure-ad-b2c-call-a-net-web-api-from-a-net-web-app"></a>Az Azure AD B2C: .NET webes API-hívás .NET-webalkalmazásból

Az Azure AD B2C segítségével hatékony identity management szolgáltatások tooyour webalkalmazások és webes API-kat is hozzáadhat. Ez a cikk ismerteti, hogyan toorequest hozzáférési jogkivonatok, valamint a hívásokat a .NET-"Feladatlista" webes alkalmazás tooa .NET webes API-t.

Ez a cikk nem foglalkozik hogyan tooimplement bejelentkezés, regisztrációs profil-kezelés az Azure AD B2C és. A cikk foglalkozik a hívó webes API-k hello felhasználó már hitelesítése után. Ha még nem tette meg, a következőket:

* Első lépések egy [.NET-webalkalmazás](active-directory-b2c-devquickstarts-web-dotnet-susi.md)
* Első lépések egy [.NET webes API-t](active-directory-b2c-devquickstarts-api-dotnet.md)

## <a name="prerequisite"></a>Előfeltétel

toobuild egy webes alkalmazás, amely meghívja a webes API-t kell:

1. [Az Azure AD B2C bérlő létrehozása](active-directory-b2c-get-started.md).
2. [Regisztrálja a webes api](active-directory-b2c-app-registration.md#register-a-web-api).
3. [A webes alkalmazás regisztrálása](active-directory-b2c-app-registration.md#register-a-web-app).
4. [Házirendek beállítása](active-directory-b2c-reference-policies.md).
5. [Támogatás hello web app engedélyek toouse hello webes API-t](active-directory-b2c-access-tokens.md#publishing-permissions).

> [!IMPORTANT]
> hello ügyfélalkalmazást és a webes API hello ugyanazt az Azure AD B2C-címtárban kell használnia.
>

## <a name="download-hello-code"></a>Hello kód letöltése

az oktatóanyag kódjának hello fenntartott [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi). Hello minta klónozhat futtatásával:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Hello mintakód letöltése után nyissa meg hello Visual Studio .sln fájlt tooget elindult. hello megoldásfájl két projektet tartalmaz: `TaskWebApp` és `TaskService`. `TaskWebApp`az MVC webalkalmazás hello felhasználó kommunikál. `TaskService`minden felhasználói feladatlistát tároló hello app webes API van. Ez a cikk nem tér ki az épület hello `TaskWebApp` webalkalmazás vagy hello `TaskService` webes API-t. toolearn hogyan toobuild hello .NET webalkalmazás-alkalmazásokhoz az Azure AD B2C használatával tekintse meg a [.NET web app oktatóanyag](active-directory-b2c-devquickstarts-web-dotnet-susi.md). Hogyan toobuild hello .NET webes API-t az Azure AD B2C használatával biztonságossá toolearn tekintse meg a [.NET webes API-k oktatóanyag](active-directory-b2c-devquickstarts-api-dotnet.md).

### <a name="update-hello-azure-ad-b2c-configuration"></a>Az Azure AD B2C hello konfiguráció frissítése

A minta rendszer konfigurált toouse hello házirendek és az ügyfél-Azonosítóját a bemutató bérlő. Ha azt szeretné, hogy toouse saját bérlőt:

1. Nyissa meg `web.config` a hello `TaskService` projektre, és cserélje le a hello értékei

    * Az `ida:Tenant` helyett szerepeljen a bérlő neve
    * `ida:ClientId`a webes api-alkalmazás azonosítójú
    * Az `ida:SignUpSignInPolicyId` helyett szerepeljen a „regisztrálási vagy bejelentkezési” házirend neve

2. Nyissa meg `web.config` a hello `TaskWebApp` projektre, és cserélje le a hello értékei

    * Az `ida:Tenant` helyett szerepeljen a bérlő neve
    * Az `ida:ClientId` helyett szerepeljen a webapp alkalmazásazonosítója
    * Az `ida:ClientSecret` helyett szerepeljen a webapp titkos kulcsa
    * Az `ida:SignUpSignInPolicyId` helyett szerepeljen a „regisztrálási vagy bejelentkezési” házirend neve
    * Az `ida:EditProfilePolicyId` helyett szerepeljen a „profil szerkesztése” házirend neve
    * Az `ida:ResetPasswordPolicyId` helyett szerepeljen a „Jelszó alaphelyzetbe állítása” házirend neve



## <a name="requesting-and-saving-an-access-token"></a>A kért és mentése egy hozzáférési jogkivonatot:

### <a name="specify-hello-permissions"></a>Hello engedélyek megadásához

Rendelés toomake hello hívás toohello webes API-t kell tooauthenticate hello felhasználói (a sign-Close-Up/bejelentkezési házirend használatával) és [olyan hozzáférési jogkivonatot kap](active-directory-b2c-access-tokens.md) az Azure AD B2C. A sorrend tooreceive olyan hozzáférési jogkivonatot először meg kell hello access token toogrant milyen hello engedélyek. hello engedélyek meg van határozva a hello `scope` paraméter, amikor hello kérelem toohello `/authorize` végpont. Például egy hozzáférési jogkivonat hello "olvasási" engedély toohello erőforrás alkalmazás tooacquire hello App ID URI-azonosítója `https://contoso.onmicrosoft.com/tasks`, hello hatókör lenne `https://contoso.onmicrosoft.com/tasks/read`.

a minta, nyissa meg hello fájlban toospecify hello hatókör `App_Start\Startup.Auth.cs` , és adja meg a hello `Scope` OpenIdConnectAuthenticationOptions változóját.

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

### <a name="exchange-hello-authorization-code-for-an-access-token"></a>Exchange hello engedélyezési kód egy hozzáférési jogkivonatot:

Egy felhasználó hello-előfizetés és a bejelentkezési élmény befejezése után az alkalmazás kap egy engedélyezési kód az Azure AD B2C. hello OWIN OpenID Connect köztes hello kódot fogja tárolni, de nem cserélnek azt a hozzáférési tokent. Használhatja a hello [MSAL könyvtár](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) toomake hello exchange. A mintában szereplő konfigurált a lekérdezésértesítési visszahívást hello OpenID Connect köztes be, amikor egy engedélyezési kód érkezik. A hello visszahívási azt MSAL tooexchange hello kód használata a jogkivonat és mentse hello token hello gyorsítótárába.

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

## <a name="calling-hello-web-api"></a>Hello webes API meghívása

Ez a szakasz ismerteti, hogyan toouse hello jogkivonatot kapott sign-Close-Up/bejelentkezés az Azure AD B2C sorrendben tooaccess hello webes API-t.

### <a name="retrieve-hello-saved-token-in-hello-controllers"></a>A hello, tartományvezérlői mentett hello-jogkivonatot lekérdezni

Hello `TasksController` felelős hello webes API-k kommunikál a HTTP-kérelmek toohello API tooread küld, hozzon létre és törölheti a feladatokat. Mivel hello API védi-e az Azure AD B2C, meg kell toofirst hello/token lekérése a fenti lépés hello mentette.

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

### <a name="read-tasks-from-hello-web-api"></a>Feladatok olvasni hello webes API-k

Ha megkapta a jogkivonatot, csatolhatók toohello HTTP `GET` hello kérést `Authorization` fejléc toosecurely hívás `TaskService`:

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

### <a name="create-and-delete-tasks-on-hello-web-api"></a>Hozzon létre vagy töröljön a feladatokat a hello webes API-k

Hajtsa végre hello azonos mintát küldésekor `POST` és `DELETE` a hello gyorsítótárban levő MSAL tooretrieve hello hozzáférési jogkivonat kérelmek toohello webes API-t használ.

## <a name="run-hello-sample-app"></a>Hello mintaalkalmazás futtatása

Végezetül hozza létre, és mindkét hello alkalmazásainak futtatásához. Regisztráció és bejelentkezés és hozzon létre feladatokat hello bejelentkezett felhasználó. Kijelentkezés és bejelentkezés másik felhasználóként. Hozzon létre feladatokat ennek a felhasználónak. Figyelje meg, hogyan hello feladatok felhasználónként tárolja a hello API, mert hello API hello felhasználói identitás kiolvassa a hello jogkivonatot kap. Próbálja meg is játszott hello hatókörként. Távolítsa el hello engedélyt túl "write", és próbálja meg újból felvenni a feladatok. Csak győződjön meg arról, hogy toosign kimenő hello hatókör minden módosításakor.

