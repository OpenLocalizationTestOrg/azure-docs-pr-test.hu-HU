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
# <a name="calling-a-web-api-from-a-net-web-app"></a>A webes API hívása a .NET-webalkalmazás
Hello v2.0-végponttal gyorsan hozzáadhat hitelesítési tooyour webalkalmazások és webes API-khoz személyes Microsoft-fiókot is támogatása és a munkahelyi vagy iskolai fiókok.  Itt azt fogja létrehozni egy MVC webalkalmazás, mely aláírja a felhasználók az OpenID Connect, használja a Microsoft OWIN köztes segítséget.  hello webes alkalmazás OAuth 2.0 hozzáférési jogkivonatok lekérése egy webes api lehetővé teszi, hogy OAuth 2.0-s által védett létrehozása, olvasása, és törli a egy adott felhasználó "Feladatlista".

Ez az oktatóanyag fog elsősorban összpontosítanak MSAL tooacquire használatával, és a hozzáférési jogkivonatok használata egy webalkalmazás teljes ismertetett [Itt](active-directory-v2-flows.md#web-apps).  Előfeltételként, érdemes lehet toofirst megtudhatja, hogyan túl[hozzáadása alapszintű bejelentkezési tooa webalkalmazás](active-directory-v2-devquickstarts-dotnet-web.md) vagy hogyan túl[megfelelően a webes API biztonságossá tétele](active-directory-v2-devquickstarts-dotnet-api.md).

> [!NOTE]
> Nem minden Azure Active Directory forgatókönyvek és funkciók támogatják hello v2.0-végponttól.  toodetermine használatát hello v2.0-végpontra, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).
> 
> 

## <a name="download-sample-code"></a>Mintakód letöltése
az oktatóanyag kódjának hello kezelt [a Githubon](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet).  toofollow mellett, akkor [töltse le a .zip hello alkalmazás vázát](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) vagy a Klónozás hello vázat:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

Alternatív megoldásként, [letöltése befejeződött hello alkalmazást egy .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) vagy a Klónozás befejeződött hello alkalmazásba:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

## <a name="register-an-app"></a>Alkalmazás regisztrálása
Hozzon létre egy új alkalmazást [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy kövesse az alábbi [részletes lépéseket](active-directory-v2-app-registration.md).  Győződjön meg arról, hogy:

* Másolja le hello **alkalmazásazonosító** tooyour app hozzárendelve, szüksége lehet rájuk hamarosan.
* Hozzon létre egy **alkalmazás titkos kulcs** a hello **jelszó** típusát, és másolja le az érték későbbi használatra
* Adja hozzá a hello **webes** platform az alkalmazásra vonatkozóan.
* Adja meg a megfelelő hello **átirányítási URI-**. hello átirányítási uri azt jelzi, ha hitelesítési válaszok legyenek irányítva – hello ebben az oktatóanyagban alapértelmezés szerint AD tooAzure `https://localhost:44326/`.

## <a name="install-owin"></a>Az OWIN telepítése
Adja hozzá a hello OWIN köztes NuGet csomagjainak toohello `TodoList-WebApp` projektet a hello Csomagkezelő konzol.  hello OWIN köztes kell használt tooissue be- és kijelentkezési kérések, hello felhasználói munkamenet kezeli, és többek között a hello felhasználó adatainak beolvasása.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

## <a name="sign-hello-user-in"></a>A bejelentkezési hello felhasználó
Mostantól konfigurálhatja az hello OWIN köztes toouse hello [OpenID Connect hitelesítési protokoll](active-directory-v2-protocols.md).  

* Nyissa meg hello `web.config` hello hello legfelső-szintű fájl `TodoList-WebApp` projektre, és írja be az alkalmazás konfigurációs értékeit hello `<appSettings>` szakasz.
  * Hello `ida:ClientId` hello van **alkalmazásazonosító** tooyour app hello regisztrációs portálon rendelt.
  * Hello `ida:ClientSecret` hello van **alkalmazás titkos kulcs** hello regisztrációs portálon létrehozott.
  * Hello `ida:RedirectUri` hello van **átirányítási Uri-** hello portálon megadott.
* Nyissa meg hello `web.config` hello hello legfelső-szintű fájl `TodoList-Service` projektre, és cserélje le a hello `ida:Audience` a hello azonos **alkalmazásazonosító** a fentiek szerint.
* Nyissa meg hello fájl `App_Start\Startup.Auth.cs` , és adja hozzá `using` a hello tárak a fenti utasítások.
* A hello ugyanazt a fájlt, hello megvalósítása `ConfigureAuth(...)` metódust.  Megadja a paraméterek hello `OpenIDConnectAuthenticationOptions` erre a célra az Azure ad alkalmazás toocommunicate koordinátáit.

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

## <a name="use-msal-tooget-access-tokens"></a>MSAL tooget hozzáférési jogkivonatok használata
A hello `AuthorizationCodeReceived` értesítési, azt szeretnénk, ha toouse [OAuth 2.0 párhuzamosan az OpenID Connect](active-directory-v2-protocols.md) tooredeem hello authorization_code egy hozzáférési jogkivonat toohello tennivalók listája szolgáltatás számára.  MSAL teheti a folyamat egyszerűen meg:

* Először telepítse a MSAL hello előzetes verziója:

```PM> Install-Package Microsoft.Identity.Client -ProjectName TodoList-WebApp -IncludePrerelease```

* Adja hozzá egy másik `using` utasítás toohello `App_Start\Startup.Auth.cs` MSAL fájlt.
* Ezután adja hozzá egy új módszer, hello `OnAuthorizationCodeReceived` eseménykezelő.  A kezelő MSAL tooacquire egy hozzáférési jogkivonat toohello tennivalók listája API-t fogja használni, és hello jogkivonat-ban tárolja MSAL tartozó jogkivonat gyorsítótára későbbi használatra:

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

* A webalkalmazásokban MSAL rendelkezik egy bővíthető jogkivonatok gyorsítótárát, amely használt toostore jogkivonatokat.  Ezt a mintát megvalósító hello `NaiveSessionCache` http munkamenet tárolási toocache jogkivonatokat használ, amely.

<!-- TODO: Token Cache article -->


## <a name="call-hello-web-api"></a>Hello Web API hívása
Most már tooactually használja a 3. lépésben beszerzett hello access_token idő.  Nyissa meg hello webes alkalmazás `Controllers\TodoListController.cs` fájlt, ami lehetővé teszi, hogy minden hello CRUD-kérelmek toohello tennivalók listája API.

* Használhatja a MSAL újra ide toofetch access_tokens a hello MSAL gyorsítótár.  Először adja hozzá a `using` MSAL toothis fájl utasítást.
  
    `using Microsoft.Identity.Client;`
* A hello `Index` művelet, használjon MSAL `AcquireTokenSilentAsync` metódus tooget egy access_token használható hello feladatlista szolgáltatáshoz használt tooread adatait:

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

* hello minta majd felveszi hello eredményül kapott jogkivonat toohello HTTP GET kérést hello `Authorization` fejléc, mely hello feladatlista szolgáltatás használja tooauthenticate hello kérelem.
* Ha hello feladatlista szolgáltatás adja vissza egy `401 Unauthorized` választ, a MSAL hello access_tokens váltak érvénytelen valamilyen okból.  Ebben az esetben engedje el bármilyen access_tokens a hello MSAL gyorsítótár és hello felhasználót, hogy a újra, amely újraindítja hello token beszerzési folyamatot toosign esetleg üzenet megjelenítése.

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

* Hasonlóképpen ha MSAL nem tooreturn egy access_token bármilyen okból, meg kell kérni a hello felhasználói toosign a újra.  Ez egyszerűen bármelyik elfogja az `MSALException`:

```C#
// ...
catch (MsalException ee)
{
        // If MSAL could not get a token silently, show hello user an error indicating they might need toosign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading tooDo List: " + ee.Message + " You might need toolog out and log back in.");
}
// ...
```

* hello pontos azonos `AcquireTokenSilentAsync` tekintendő, amely a hello implementd `Create` és `Delete` műveletek.  Webalkalmazások a MSAL metódus tooget access_tokens is használhatja, amikor az alkalmazás kell.  MSAL beszerzése, gyorsítótárazás és jogkivonatok frissítése az Ön kezeli.

Végül felépítéséhez és az alkalmazás futtatása!  Jelentkezzen be Microsoft-Account vagy egy Azure AD-fiókot, és figyelje meg, hogyan hello felhasználói identitás hello felső navigációs sáv megjelenik.  Adja hozzá, és töröljön néhány elemet hello felhasználói feladatlistát OAuth 2.0 API védett toosee hello meghívja a működés közben.  Most már rendelkezik egy webalkalmazás & webes API egyaránt az iparági szabványos protokollok, amely képes hitelesíteni a személyes és munkahelyi vagy iskolai fiókkal rendelkező felhasználók használatával biztonságossá.

Referenciaként hello befejeződött (a konfigurációs értékek nélkül) minta [itt megadott](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).  

## <a name="next-steps"></a>Következő lépések
További forrásokért tekintse meg:

* [hello v2.0 – útmutató fejlesztőknek >>](active-directory-appmodel-v2-overview.md)
* [StackOverflow "azure-active-directory" címke >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Biztonsági frissítések termékeinkhez
Javasoljuk, tooget értesítést a bekövetkező biztonsági incidensekről ellátogatva [ezen a lapon](https://technet.microsoft.com/security/dd252948) és előfizetés tooSecurity tanácsadói riasztásokra.

