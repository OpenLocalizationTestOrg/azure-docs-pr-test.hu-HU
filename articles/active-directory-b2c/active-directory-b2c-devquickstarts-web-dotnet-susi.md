---
title: Active Directory B2C aaaAzure |} Microsoft Docs
description: "Hogyan toobuild rendelkező sign-Close-Up/sign-in webes alkalmazás profilját, szerkesztése és a jelszó alaphelyzetbe állítása az Azure Active Directory B2C használatával."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: krassk
editor: barbaraselden
ms.assetid: 30261336-d7a5-4a6d-8c1a-7943ad76ed25
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/17/2017
ms.author: parakhj
ms.openlocfilehash: 187f99a8dd50d212de4f0517f552cdbbe5a8edf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-aspnet-web-app-with-azure-active-directory-b2c-sign-up-sign-in-profile-edit-and-password-reset"></a>Egy ASP.NET-webalkalmazás létrehozása az Azure Active Directory B2C regisztráció, bejelentkezés, profil szerkesztése és a jelszó alaphelyzetbe állítása

Ez az oktatóanyag a következőket mutatja be:

> [!div class="checklist"]
> * Az Azure AD B2C identitáskezelési funkciókat tooyour webalkalmazás hozzáadása
> * A webes alkalmazás regisztrálása az Azure AD B2C-címtárban
> * Egy felhasználó sign-Close-Up/sign-in, a profil szerkesztése és a jelszó-visszaállítási házirend a webalkalmazás létrehozása

## <a name="prerequisites"></a>Előfeltételek

- A B2C-bérlő tooan Azure-fiókot kell csatlakoztatni. Létrehozhat egy ingyenes Azure-fiók [Itt](https://azure.microsoft.com/en-us/).
- Szüksége [Microsoft Visual Studio](https://www.visualstudio.com/) vagy egy hasonló tooview programot, és módosítsa a hello mintakód.

## <a name="create-an-azure-ad-b2c-directory"></a>Azure AD B2C címtár létrehozása

Ahhoz, hogy használni tudja az Azure AD B2C-t, előbb létre kell hoznia egy címtárat vagy bérlőt. A könyvtár egy olyan tároló, a felhasználók, alkalmazások, csoportok és több. Ha egy már nem rendelkezik, ez az útmutató a folytatás előtt hozzon létre egy B2C-címtárat.

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

> [!NOTE]
> 
> Tooconnect hello B2C-bérlő tooyour Azure-előfizetésre van szüksége. Kiválasztása után **létrehozása**, jelölje be hello **kapcsolat egy meglévő Azure AD B2C bérlő toomy Azure-előfizetés** lehetőséget, majd a hello **Azure AD B2C bérlő** legördülő listán, válassza ki azt szeretné, hogy tooassociate hello bérlő.

## <a name="create-and-register-an-application"></a>Hozzon létre, és egy alkalmazás regisztrálása

A következő toocreate kell, és regisztrálja a hello alkalmazást a B2C-címtárban. Ez biztosítja, hogy az Azure AD B2C szükséges toosecurely információkat az alkalmazás kommunikálni. 

[!INCLUDE [active-directory-b2c-register-web-api](../../includes/active-directory-b2c-register-web-api.md)]

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

Amikor elkészült, hogy az API-k és a natív alkalmazás az alkalmazás beállításaiban.

## <a name="create-policies-on-your-b2c-tenant"></a>A B2C-bérlő a házirendek létrehozása

Az Azure AD B2C-ben a felhasználói élményeket [szabályzatok](active-directory-b2c-reference-policies.md) határozzák meg. Ebben a kódmintában három identitásélményt tartalmaz: **előfizetési & bejelentkezési**, **profil szerkesztése**, és **jelszó-változtatási**.  Szüksége van a toocreate egy házirendet az egyes típusok hello leírtak [házirendek áttekintésével foglalkozó cikkben](active-directory-b2c-reference-policies.md). Minden házirend esetében meg arról, hogy tooselect hello megjelenítési név attribútum vagy a jogcím és a toocopy hello nevet a házirend a későbbi felhasználásra vonatkozó lennie.

### <a name="add-your-identity-providers"></a>Az identitás-szolgáltatóktól hozzáadása

Válassza ki a beállítások **identitás-szolgáltatóktól** , és válassza a felhasználónév előfizetési vagy e-mailt létrehozni.

### <a name="create-a-sign-up-and-sign-in-policy"></a>Regisztráció és bejelentkezés házirend létrehozása

[!INCLUDE [active-directory-b2c-create-sign-in-sign-up-policy](../../includes/active-directory-b2c-create-sign-in-sign-up-policy.md)]

### <a name="create-a-profile-editing-policy"></a>A Profilszerkesztési házirend létrehozása

[!INCLUDE [active-directory-b2c-create-profile-editing-policy](../../includes/active-directory-b2c-create-profile-editing-policy.md)]

### <a name="create-a-password-reset-policy"></a>A jelszó-visszaállítási házirend létrehozása

[!INCLUDE [active-directory-b2c-create-password-reset-policy](../../includes/active-directory-b2c-create-password-reset-policy.md)]

A házirendek létrehozása után készen áll a toobuild Ön az alkalmazást.

## <a name="download-hello-sample-code"></a>Hello mintakód letöltése

az oktatóanyag kódjának hello fenntartott [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi). Hello minta klónozhat futtatásával:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Hello mintakód letöltése után nyissa meg hello Visual Studio .sln fájlt tooget elindult. hello megoldásfájl két projektet tartalmaz: `TaskWebApp` és `TaskService`. `TaskWebApp`hello felhasználói hello MVC webalkalmazás kommunikál. `TaskService`minden felhasználói feladatlistát tároló hello app webes API van. Ez a cikk csak ismertetik hello `TaskWebApp` alkalmazás. toolearn hogyan toobuild `TaskService` Azure AD B2C segítségével, lásd: [a .NET webes api-oktatóanyag](active-directory-b2c-devquickstarts-api-dotnet.md).

## <a name="update-code-toouse-your-tenant-and-policies"></a>A bérlői és a házirendek kód toouse frissítése

A minta rendszer konfigurált toouse hello házirendek és az ügyfél-Azonosítóját a bemutató bérlő. tooconnect azt tooyour saját bérlőt, tooopen kell `web.config` a hello `TaskWebApp` projektre, és cserélje le a következő értékek hello:

* Az `ida:Tenant` helyett szerepeljen a bérlő neve
* Az `ida:ClientId` helyett szerepeljen a webapp alkalmazásazonosítója
* Az `ida:ClientSecret` helyett szerepeljen a webapp titkos kulcsa
* Az `ida:SignUpSignInPolicyId` helyett szerepeljen a „regisztrálási vagy bejelentkezési” házirend neve
* Az `ida:EditProfilePolicyId` helyett szerepeljen a „profil szerkesztése” házirend neve
* Az `ida:ResetPasswordPolicyId` helyett szerepeljen a „Jelszó alaphelyzetbe állítása” házirend neve

## <a name="launch-hello-app"></a>Hello alkalmazás indítása
A Visual studióban, indítsa el a hello alkalmazást. Keresse meg a toohello feladatlista fülre, majd a Megjegyzés hello URL-címe: https://login.microsoftonline.com/*YourTenantName*/oauth2/v2.0/authorize?p=*YourSignUpPolicyName*& client_id = *YourclientID*...

Iratkozzon fel az e-mail cím vagy a felhasználó neve hello alkalmazást. Jelentkezzen ki, majd jelentkezzen be újból hello-profil szerkesztése vagy hello jelszó alaphelyzetbe állítása. Kijelentkezés és bejelentkezés másik felhasználóként. 

## <a name="add-social-idps"></a>Közösségi IDPs hozzáadása

Jelenleg hello alkalmazás támogatja a csak a regisztráció és bejelentkezés felhasználói **helyi fiókok**; fiókok a B2C-címtárban tárolja, amely a felhasználónév és jelszó használata. Az Azure AD B2C segítségével támogathatja más **identitás-szolgáltatóktól** (IDPs) bármely, a kód módosítása nélkül.

tooadd közösségi IDPs tooyour alkalmazást, és kezdje az alábbi hello részletes utasításokat a cikkeiben. Az egyes IDP meg toosupport, egy alkalmazás tooregister kell, hogy a rendszer, és szerezzen be egy ügyfél-azonosítót.

* [Az IDP Facebook beállítása](active-directory-b2c-setup-fb-app.md)
* [Az IDP Google beállítása](active-directory-b2c-setup-goog-app.md)
* [Az IDP Amazon beállítása](active-directory-b2c-setup-amzn-app.md)
* [Az IDP LinkedIn beállítása](active-directory-b2c-setup-li-app.md)

Miután hozzáadta a hello identity providers tooyour B2C-címtárban, Szerkesztés minden, a három házirend tooinclude hello új IDPs hello leírtak [házirendek áttekintésével foglalkozó cikkben](active-directory-b2c-reference-policies.md). A házirend mentése után futtassa újra a hello alkalmazást.  Új IDPs fel, mert a bejelentkezési és regisztrációs beállítások a identitással kapcsolatos műveletet mindegyikében hello kell megjelennie.

A házirendek kísérletezhet, és figyelje meg a mintaalkalmazás hello hatással. Vegye fel vagy távolítsa el a IDPs, kezelheti az alkalmazás jogcímét, vagy módosítsa a regisztrációs attribútumokat. Kísérletet, amíg meg nem látja hogyan házirendeket, a hitelesítési kérelmek és OWIN alkalmazássá.

## <a name="sample-code-walkthrough"></a>A minta kód forgatókönyv
hello a következő szakaszok bemutatják, hogyan hello mintakód alkalmazás van konfigurálva. Akkor lehet, hogy ezzel útmutatóként a jövőbeli fejlesztés.

### <a name="add-authentication-support"></a>Vegye fel a hitelesítéshez

Most már konfigurálhatja az alkalmazás toouse az Azure AD B2C. Az alkalmazást úgy, hogy az OpenID Connect hitelesítési kérelmeket küld az Azure AD B2C kommunikál. hello kérelmek előírják hello felhasználói élményt az alkalmazás tooexecute szeretne hello házirend. Használja a Microsoft OWIN könyvtár toosend ezeket a kérelmeket, házirendek hajtható végre, kezelheti a felhasználói munkamenetek és még sok más.

#### <a name="install-owin"></a>Az OWIN telepítése

toobegin, hello OWIN köztes NuGet csomagjainak toohello projekt hozzáadása hello Visual Studio Csomagkezelő konzol használatával.

```Console
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

#### <a name="add-an-owin-startup-class"></a>OWIN indítási osztály hozzáadása

Adja hozzá egy OWIN indítási osztály toohello API hívása `Startup.cs`.  Kattintson a jobb gombbal hello projektre, válassza a **Hozzáadás** és **új elem**, majd keresse meg az owin ELEMET. hello OWIN köztes által aktivált hello `Configuration(…)` módszer az alkalmazás indításakor.

A mintában szereplő változtattuk hello osztálydeklaráció túl`public partial class Startup` és megvalósított hello osztály a többi részét hello `App_Start\Startup.Auth.cs`. Belső hello `Configuration` módszer, hívása túl hozzáadott`ConfigureAuth`, amelyhez definiálva van `Startup.Auth.cs`. Hello módosítások után `Startup.cs` tűnik hello következő:

```CSharp
// Startup.cs

public partial class Startup
{
    // hello OWIN middleware will invoke this method when hello app starts
    public void Configuration(IAppBuilder app)
    {
        // ConfigureAuth defined in other part of hello class
        ConfigureAuth(app);
    }
}
```

#### <a name="configure-hello-authentication-middleware"></a>Hello hitelesítési köztes konfigurálása

Nyissa meg hello fájl `App_Start\Startup.Auth.cs` és valósíthatnak meg hello `ConfigureAuth(...)` metódust. Megadja a paraméterek hello `OpenIdConnectAuthenticationOptions` szolgáljanak, az Azure AD B2C app toocommunicate koordinátáit. Ha nem adja meg a paraméterek, hello alapértelmezett értéket fogja használni. Például, hogy ne adjon meg hello `ResponseType` hello mintában, így hello az alapértelmezett érték `code id_token` minden kimenő kérelem tooAzure AD B2C használni fogják.

Meg kell készíteni, hitelesítési cookie-k tooset is. hello OpenID Connect köztes használ cookie-k toomaintain felhasználói munkamenetek, többek között.

```CSharp
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // Initialize variables ...

    // Configure hello OWIN middleware
    public void ConfigureAuth(IAppBuilder app)
    {
        app.UseCookieAuthentication(new CookieAuthenticationOptions());
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseOpenIdConnectAuthentication(
            new OpenIdConnectAuthenticationOptions
            {
                // Generate hello metadata address using hello tenant and policy information
                MetadataAddress = String.Format(AadInstance, Tenant, DefaultPolicy),

                // These are standard OpenID Connect parameters, with values pulled from web.config
                ClientId = ClientId,
                RedirectUri = RedirectUri,
                PostLogoutRedirectUri = RedirectUri,

                // Specify hello callbacks for each type of notifications
                Notifications = new OpenIdConnectAuthenticationNotifications
                {
                    RedirectToIdentityProvider = OnRedirectToIdentityProvider,
                    AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    AuthenticationFailed = OnAuthenticationFailed,
                },

                // Specify hello claims toovalidate
                TokenValidationParameters = new TokenValidationParameters
                {
                    NameClaimType = "name"
                },

                // Specify hello scope by appending all of hello scopes requested into one string (seperated by a blank space)
                Scope = $"{OpenIdConnectScopes.OpenId} {ReadTasksScope} {WriteTasksScope}"
            }
        );
    }

    // Implement hello "Notification" methods...
}
```

A `OpenIdConnectAuthenticationOptions` újabb meghatároztuk visszahívási funkciók hello OpenID Connect köztes által kapott az adott értesítésekhez. Ezek közül a viselkedésmódok meghatározása egy `OpenIdConnectAuthenticationNotifications` objektumra, és hello eltárolni `Notifications` változó. A mintában szereplő meghatároztuk három különböző visszahívások attól függően, hogy hello esemény.

### <a name="using-different-policies"></a>Eltérő házirendek használatával

Hello `RedirectToIdentityProvider` értesítési aktiválódik, amikor a kérelem tooAzure AD B2C. A visszahívási függvény hello `OnRedirectToIdentityProvider`, ellenőrizzük a kimenő hívás, ha azt szeretné, hogy toouse hello másik szabályt. Rendelés toodo jelszó alaphelyzetbe állítása, vagy szerkesztheti a profilját kell toouse hello megfelelő házirendet például hello jelszó-visszaállítási házirend hello alapértelmezett "Regisztráció vagy bejelentkezés" házirend helyett.

A mintában, amikor a felhasználó szeretne tooreset hello jelszó vagy hello-profil szerkesztése azt hello házirend hozzáadása azt toouse inkább hello OWIN összefüggésben. Amely végezhető hello következő tevékenységek végrehajtásával:

```CSharp
    // Let hello middleware know you are trying toouse hello edit profile policy
    HttpContext.GetOwinContext().Set("Policy", EditProfilePolicyId);
```

Megvalósíthat hello visszahívási függvény és `OnRedirectToIdentityProvider` hello következő tevékenységek végrehajtásával:

```CSharp
/*
*  On each call tooAzure AD B2C, check if a policy (e.g. hello profile edit or password reset policy) has been specified in hello OWIN context.
*  If so, use that policy when making hello call. Also, don't request a code (since it won't be needed).
*/
private Task OnRedirectToIdentityProvider(RedirectToIdentityProviderNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    var policy = notification.OwinContext.Get<string>("Policy");

    if (!string.IsNullOrEmpty(policy) && !policy.Equals(DefaultPolicy))
    {
        notification.ProtocolMessage.Scope = OpenIdConnectScopes.OpenId;
        notification.ProtocolMessage.ResponseType = OpenIdConnectResponseTypes.IdToken;
        notification.ProtocolMessage.IssuerAddress = notification.ProtocolMessage.IssuerAddress.Replace(DefaultPolicy, policy);
    }

    return Task.FromResult(0);
}
```

### <a name="handling-authorization-codes"></a>Hitelesítési kódok kezelése

Hello `AuthorizationCodeReceived` értesítési lesz kiváltva, ha az engedélyezési kód érkezik. hello OpenID Connect köztes nem támogatja a hozzáférési jogkivonatok cserélő kódokat. Manuálisan továbbíthatja az hello token a visszahívási függvény hello kódját. További információkért tekintse meg hello [dokumentáció](active-directory-b2c-devquickstarts-web-api-dotnet.md) , amely ismerteti, hogyan.

### <a name="handling-errors"></a>Hibák kezelése

Hello `AuthenticationFailed` értesítési lesz kiváltva, ha a hitelesítés sikertelen. A visszahívási metódus hello hibák képes kezelni, igény szerint. Azonban hozzá kell adnia egy ellenőrzést hello hibakód `AADB2C90118`. Hello végrehajtásakor hello "Regisztráció vagy bejelentkezés" házirend, hello felhasználó rendelkezik-e hello lehetőség tooselect egy **elfelejtette a jelszavát?** hivatkozásra. Ebben az esetben az Azure AD B2C elküldi az alkalmazás adott hiba mely azt jelzi, hogy az alkalmazás egy kérelmet, használja helyette: hello jelszó-visszaállítási házirend győződjön.

```CSharp
/*
* Catch any failures received by hello authentication middleware and handle appropriately
*/
private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    notification.HandleResponse();

    // Handle hello error code that Azure AD B2C throws when trying tooreset a password from hello login page
    // because password reset is not supported by a "sign-up or sign-in policy"
    if (notification.ProtocolMessage.ErrorDescription != null && notification.ProtocolMessage.ErrorDescription.Contains("AADB2C90118"))
    {
        // If hello user clicked hello reset password link, redirect toohello reset password route
        notification.Response.Redirect("/Account/ResetPassword");
    }
    else if (notification.Exception.Message == "access_denied")
    {
        notification.Response.Redirect("/");
    }
    else
    {
        notification.Response.Redirect("/Home/Error?message=" + notification.Exception.Message);
    }

    return Task.FromResult(0);
}
```

### <a name="send-authentication-requests-tooazure-ad"></a>Hitelesítési kérelmek tooAzure AD küldése

Az alkalmazás már Azure AD B2C megfelelően konfigurált toocommunicate hello OpenID Connect hitelesítési protokoll használatával. OWIN kezeli a hitelesítési üzenetek létrehozásával, az ellenőrzése az Azure AD B2C származó jogkivonatokat és a felhasználói munkamenet megtartásával hello részleteit. Az marad tooinitiate van minden egyes felhasználói folyamat.

Amikor a felhasználó megadja **Sign up/bejelentkezési**, **profilszerkesztés**, vagy **jelszó-átállítási** hello web app alkalmazásban társított hello művelet indították el a `Controllers\AccountController.cs`:

```CSharp
// Controllers\AccountController.cs

/*
*  Called when requesting toosign up or sign in
*/
public void SignUpSignIn()
{
    // Use hello default policy tooprocess hello sign up / sign in flow
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge();
        return;
    }

    Response.Redirect("/");
}

/*
*  Called when requesting tooedit a profile
*/
public void EditProfile()
{
    if (Request.IsAuthenticated)
    {
        // Let hello middleware know you are trying toouse hello edit profile policy (see OnRedirectToIdentityProvider in Startup.Auth.cs)
        HttpContext.GetOwinContext().Set("Policy", Startup.EditProfilePolicyId);

        // Set hello page tooredirect tooafter editing hello profile
        var authenticationProperties = new AuthenticationProperties { RedirectUri = "/" };
        HttpContext.GetOwinContext().Authentication.Challenge(authenticationProperties);

        return;
    }

    Response.Redirect("/");

}

/*
*  Called when requesting tooreset a password
*/
public void ResetPassword()
{
    // Let hello middleware know you are trying toouse hello reset password policy (see OnRedirectToIdentityProvider in Startup.Auth.cs)
    HttpContext.GetOwinContext().Set("Policy", Startup.ResetPasswordPolicyId);

    // Set hello page tooredirect tooafter changing passwords
    var authenticationProperties = new AuthenticationProperties { RedirectUri = "/" };
    HttpContext.GetOwinContext().Authentication.Challenge(authenticationProperties);

    return;
}
```

Kimenő hello felhasználói hello alkalmazás OWIN toosign is használható. A `Controllers\AccountController.cs` vezetünk be:

```CSharp
// Controllers\AccountController.cs

/*
*  Called when requesting toosign out
*/
public void SignOut()
{
    // toosign out hello user, you should issue an OpenIDConnect sign out request.
    if (Request.IsAuthenticated)
    {
        IEnumerable<AuthenticationDescription> authTypes = HttpContext.GetOwinContext().Authentication.GetAuthenticationTypes();
        HttpContext.GetOwinContext().Authentication.SignOut(authTypes.Select(t => t.AuthenticationType).ToArray());
        Request.GetOwinContext().Authentication.GetAuthenticationTypes();
    }
}
```

Továbbá tooexplicitly meghívása egy házirendet, használhat egy `[Authorize]` a tartományvezérlőket-címke, amely végrehajtja a házirendet, ha hello felhasználó nem jelentkezett be. Nyissa meg `Controllers\HomeController.cs` , és adja hozzá a hello `[Authorize]` címke toohello jogcím-vezérlő.  OWIN kiválasztja hello utolsó házirend konfigurálását, ha hello `[Authorize]` címke talált.

```CSharp
// Controllers\HomeController.cs

// You can use hello Authorize decorator tooexecute a certain policy if hello user is not already signed into hello app.
[Authorize]
public ActionResult Claims()
{
  ...
```

### <a name="display-user-information"></a>Felhasználói adatok megjelenítése

Amikor a felhasználók hitelesítésére OpenID Connect használatával, az Azure AD B2C adja vissza egy azonosító token toohello alkalmazást tartalmazó **jogcímek**. Ezek a helyességi feltételek hello felhasználóról. Jogcímek toopersonalize használhatja az alkalmazást.

Nyissa meg hello `Controllers\HomeController.cs` fájlt. Felhasználói jogcímek érheti el, a vezérlők keresztül hello `ClaimsPrincipal.Current` rendszerbiztonsági objektumot.

```CSharp
// Controllers\HomeController.cs

[Authorize]
public ActionResult Claims()
{
    Claim displayName = ClaimsPrincipal.Current.FindFirst(ClaimsPrincipal.Current.Identities.First().NameClaimType);
    ViewBag.DisplayName = displayName != null ? displayName.Value : string.Empty;
    return View();
}
```

Érheti el bármilyen jogcímet, hogy az alkalmazás fogadása hello azonos módon.  Hello app kap minden hello jogcím listája megtalálható az Ön hello **jogcímek** lap.
