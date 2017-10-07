---
title: aaaAzure AD B2C |} Microsoft Docs
description: "Hogyan toobuild egy .NET webes API használatával az Azure Active Directory B2C használatával biztonságossá OAuth 2.0 hozzáférési jogkivonatok hitelesítéshez."
services: active-directory-b2c
documentationcenter: .net
author: parakhj
manager: krassk
editor: 
ms.assetid: 7146ed7f-2eb5-49e9-8d8b-ea1a895e1966
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: parakhj
ms.openlocfilehash: d45364216deda38ef44b60dd11e86d9a089ad509
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-build-a-net-web-api"></a>Azure Active Directory B2C: .NET webes API készítése

Az Azure Active Directory (Azure AD) B2C segítségével védetté tehet egy webes API-t OAuth 2.0 hozzáférési jogkivonatok használatával. Ezek a jogkivonatok engedélyezik az ügyfél alkalmazások tooauthenticate toohello API. Ez a cikk bemutatja, hogyan toocreate egy .NET MVC "Feladatlista" API-t, amely lehetővé teszi a felhasználók az ügyfél alkalmazás tooCRUD feladatok. hello webes API-t az Azure AD B2C használatával lett biztonságossá téve, és csak lehetővé teszi a hitelesített felhasználók toomanage a tennivalók listájára.

## <a name="create-an-azure-ad-b2c-directory"></a>Azure AD B2C címtár létrehozása

Ahhoz, hogy használni tudja az Azure AD B2C-t, előbb létre kell hoznia egy címtárat vagy bérlőt. A címtárban tárolhatja az összes felhasználót, alkalmazást, csoportot és sok minden mást. Ha még nem tette meg, [hozzon létre most egy B2C-címtárat](active-directory-b2c-get-started.md), mielőtt továbblépne ebben az útmutatóban.

> [!NOTE]
> hello ügyfélalkalmazást és a webes API hello ugyanazt az Azure AD B2C-címtárban kell használnia.
>

## <a name="create-a-web-api"></a>Webes API létrehozása

A következő lépésben toocreate egy webes API-alkalmazást a B2C-címtárban. Ez biztosítja, hogy az igények toosecurely kommunikálni az alkalmazást az Azure AD-információkat. hajtsa végre az alkalmazást, és egy toocreate [ezeket az utasításokat](active-directory-b2c-app-registration.md). Ügyeljen arra, hogy:

* Tartalmaznak egy **webalkalmazás** vagy **webes API-t** hello alkalmazásban.
* Használjon hello **átirányítási URI-** `https://localhost:44332/` hello webalkalmazás. Ez az hello kódmintában hello webalkalmazás ügyféloldalának alapértelmezett helye.
* Másolás hello **Alkalmazásazonosító** , amely hozzárendelt tooyour alkalmazást. Erre később még szüksége lesz.
* Adjon meg egy alkalmazásazonosítót az **Alkalmazásazonosító URI** mezőben.
* Adja hozzá az engedélyek a hello **hatókörök közzétett** menü.

  [!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Szabályzatok létrehozása

Az Azure AD B2C-ben a felhasználói élményeket [szabályzatok](active-directory-b2c-reference-policies.md) határozzák meg. Szüksége lesz egy Azure AD B2C házirend toocommunicate toocreate. Hello segítségével kombinált sign-Close-Up/bejelentkezési házirend, javasoljuk, az a hello [házirendek áttekintésével foglalkozó cikkben](active-directory-b2c-reference-policies.md). A házirend létrehozásakor ügyeljen arra, hogy:

* A házirendben válassza ki a **megjelenítendő nevet** és az egyéb regisztrációs attribútumokat.
* Alkalmazási jogcímnek minden házirend esetén válassza ki a **megjelenítendő nevet** és az **objektumazonosítót**. Kiválaszthat egyéb jogcímeket is.
* Másolás hello **neve** egyes házirendek létrehozása után. Hello házirendnév később lesz szüksége.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Miután sikeresen létrehozta a hello házirend, készen áll a toobuild Ön az alkalmazást.

## <a name="download-hello-code"></a>Hello kód letöltése

az oktatóanyag kódjának hello fenntartott [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi). Hello minta klónozhat futtatásával:

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Hello mintakód letöltése után nyissa meg hello Visual Studio .sln fájlt tooget elindult. hello megoldásfájl két projektet tartalmaz: `TaskWebApp` és `TaskService`. `TaskWebApp`az, hogy a felhasználó hello MVC webalkalmazás kommunikál. `TaskService`minden felhasználói feladatlistát tároló hello app webes API van. Ez a cikk csak ismertetik hello `TaskService` alkalmazás. toolearn hogyan toobuild `TaskWebApp` Azure AD B2C segítségével, lásd: [a .NET web app oktatóanyag](active-directory-b2c-devquickstarts-web-dotnet-susi.md).

### <a name="update-hello-azure-ad-b2c-configuration"></a>Az Azure AD B2C hello konfiguráció frissítése

A minta rendszer konfigurált toouse hello házirendek és az ügyfél-Azonosítóját a bemutató bérlő. Ha azt szeretné, hogy toouse saját bérlőt, szüksége lesz a következő toodo hello:

1. Nyissa meg `web.config` a hello `TaskService` projektre, és cserélje le a hello értékei
    * Az `ida:Tenant` helyett szerepeljen a bérlő neve
    * Az `ida:ClientId` helyett szerepeljen a webes API-alkalmazás azonosítója
    * Az `ida:SignUpSignInPolicyId` helyett szerepeljen a „regisztrálási vagy bejelentkezési” házirend neve

2. Nyissa meg `web.config` a hello `TaskWebApp` projektre, és cserélje le a hello értékei
    * Az `ida:Tenant` helyett szerepeljen a bérlő neve
    * Az `ida:ClientId` helyett szerepeljen a webapp alkalmazásazonosítója
    * Az `ida:ClientSecret` helyett szerepeljen a webapp titkos kulcsa
    * Az `ida:SignUpSignInPolicyId` helyett szerepeljen a „regisztrálási vagy bejelentkezési” házirend neve
    * Az `ida:EditProfilePolicyId` helyett szerepeljen a „profil szerkesztése” házirend neve
    * Az `ida:ResetPasswordPolicyId` helyett szerepeljen a „Jelszó alaphelyzetbe állítása” házirend neve


## <a name="secure-hello-api"></a>Biztonságos hello API

Ha egy ügyfél hívja az API-t, biztonságossá teheti az API-t (pl. `TaskService`) az OAuth 2.0 tulajdonosi jogkivonatok használatával. Ez biztosítja, hogy minden egyes kérelem tooyour API csak akkor érvényes, ha hello kérelem rendelkezik egy tulajdonosi jogkivonatot. Az API a Microsoft Open Web Interface .NET (OWIN) könyvtár használatával tudja elfogadni és érvényesíteni a tulajdonosi jogkivonatokat.

### <a name="install-owin"></a>Az OWIN telepítése

Első lépésként telepítse a Visual Studio Csomagkezelő konzol hello hello OWIN OAuth hitelesítési folyamatot.

```Console
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

Ez a hello OWIN alkalmazott szoftver, amely elfogadja és érvényesíthet jogkivonatokat tulajdonosi telepíti.

### <a name="add-an-owin-startup-class"></a>OWIN indítási osztály hozzáadása

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

### <a name="configure-oauth-20-authentication"></a>OAuth 2.0 típusú hitelesítés konfigurálása

Nyissa meg hello fájl `App_Start\Startup.Auth.cs`, és valósíthatnak meg hello `ConfigureAuth(...)` metódust. Például hogy nézhet hello következő:

```CSharp
// App_Start\Startup.Auth.cs

 public partial class Startup
    {
        // These values are pulled from web.config
        public static string AadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
        public static string Tenant = ConfigurationManager.AppSettings["ida:Tenant"];
        public static string ClientId = ConfigurationManager.AppSettings["ida:ClientId"];
        public static string SignUpSignInPolicy = ConfigurationManager.AppSettings["ida:SignUpSignInPolicyId"];
        public static string DefaultPolicy = SignUpSignInPolicy;

        /*
         * Configure hello authorization OWIN middleware.
         */
        public void ConfigureAuth(IAppBuilder app)
        {
            TokenValidationParameters tvps = new TokenValidationParameters
            {
                // Accept only those tokens where hello audience of hello token is equal toohello client ID of this app
                ValidAudience = ClientId,
                AuthenticationType = Startup.DefaultPolicy
            };

            app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
            {
                // This SecurityTokenProvider fetches hello Azure AD B2C metadata & signing keys from hello OpenIDConnect metadata endpoint
                AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(AadInstance, Tenant, DefaultPolicy)))
            });
        }
    }
```

### <a name="secure-hello-task-controller"></a>Biztonságos hello feladatvezérlőhöz

Miután hello alkalmazás konfigurált toouse OAuth 2.0 típusú hitelesítés, a webes API biztosíthassa hozzáadásával egy `[Authorize]` címke toohello feladatvezérlőhöz. Ez azért, ahol minden tennivalók listája adatkezelési történik, így osztályszinten kell biztonságossá tenni hello teljes vezérlőre hello osztály szinten hello vezérlő. Azt is megteheti hello `[Authorize]` címkét a részletesebb vezérlés tooindividual műveletek.

```CSharp
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-hello-token"></a>Felhasználói adatok beolvasása a hello token

`TasksController`ahol minden feladathoz hozzá van rendelve egy felhasználó "birtokló" hello feladat-adatbázisban tárolja a feladatokat. hello tulajdonos azonosítása hello felhasználói **objektumazonosító**. (Tooadd hello Objektumazonosító alkalmazásként szükséges, ezért a házirendek összes jogcímet.)

```CSharp
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

### <a name="validate-hello-permissions-in-hello-token"></a>Hello jogkivonat hello engedélyek ellenőrzése

Kötelező megadni a webes API-k toovalidate hello "hatókör" hello lexikális elem szerepel. Ez biztosítja, hogy hello a felhasználó hozzájárult toohello engedélyek szükséges tooaccess hello tennivalók listája szolgáltatás.

```CSharp
public IEnumerable<Models.Task> Get()
{
    if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "read")
    {
        throw new HttpResponseException(new HttpResponseMessage {
            StatusCode = HttpStatusCode.Unauthorized,
            ReasonPhrase = "hello Scope claim does not contain 'read' or scope claim not found"
        });
    }
    ...
}
```

## <a name="run-hello-sample-app"></a>Hello mintaalkalmazás futtatása

Végezetül készítse el és futtassa a következőket: `TaskWebApp` és `TaskService`. Hozzon létre feladatokat hello felhasználói feladatlistában, és figyelje meg, hogy azok megmaradnak a hello API hello ügyfél újraindítása után is.

## <a name="edit-your-policies"></a>Házirendek szerkesztése

Miután az Azure AD B2C használatával biztonságossá tett egy API-t, kísérletezhet a bejelentkezési-a/regisztrációs házirend és nézet hello hatások (vagy hiányára) hello API. Hello alkalmazási jogcímet hello házirendek kezelése, és módosíthatja hello felhasználó adatait, amely elérhető a hello webes API-t. Minden hozzáadott jogcím elérhető tooyour .NET MVC webes API hello lesz `ClaimsPrincipal` objektum, a cikkben leírtak szerint.
