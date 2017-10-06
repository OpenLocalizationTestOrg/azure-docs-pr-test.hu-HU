---
title: "az Azure AD v2.0-végponttól aaaAdd bejelentkezési tooa .NET MVC webes API használatával hello |} Microsoft Docs"
description: "Hogyan toobuild egy .NET MVC webes API-t, amely fogadja a mindkét személyes Microsoft-Account jogkivonatokat és a munkahelyi vagy iskolai fiókját."
services: active-directory
documentationcenter: .net
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: e77bc4e0-d0c9-4075-a3f6-769e2c810206
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 4e517145422bb6e9368e82a7eef4a5c57cce530a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="secure-an-mvc-web-api"></a>Egy MVC webes API biztonságossá tétele
Az Azure Active Directory hello v2.0-végpontra, megvédheti a Web API használatával [OAuth 2.0](active-directory-v2-protocols.md) hozzáférési jogkivonatok, mindkét személyes Microsoft-fiókkal rendelkező felhasználók és a munkahelyi vagy iskolai fiókok toosecurely hozzáférni a Web API engedélyezése.

> [!NOTE]
> Nem minden Azure Active Directory forgatókönyvek és funkciók támogatják hello v2.0-végponttól.  toodetermine használatát hello v2.0-végpontra, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).
>
>

ASP.NET webes API-k Ez elvégezhető a .NET-keretrendszer 4.5 része a Microsoft OWIN köztes használatával.  Itt a "lista tooDo" MVC webes API, amely lehetővé teszi az ügyfelek a felhasználók feladatlistáit toocreate és olvasási feladatok OWIN toobuild fogjuk használni.  ellenőrzi, hogy a hello webes API-t, hogy a bejövő kérelmek tartalmaznak egy érvényes hozzáférési jogkivonatot, és bármilyen kérelmeket, amelyek nem teljesíti az ellenőrző egy védett útvonal.  Ez a minta a Visual Studio 2015 használatával lett létrehozva.

## <a name="download"></a>Letöltés
az oktatóanyag kódjának hello kezelt [a Githubon](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet).  toofollow mellett, akkor [töltse le a .zip hello alkalmazás vázát](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) vagy a Klónozás hello vázat:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

hello üres alkalmazás tartalmazza az összes hello bolierplate kód egyszerű API-t, de összes hello identitás kapcsolatos eleme hiányzik. Ha nem szeretné mentén toofollow, hanem klónozhat vagy [letöltése befejeződött hello minta](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip).

```
git clone https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

## <a name="register-an-app"></a>Alkalmazás regisztrálása
Hozzon létre egy új alkalmazást [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy kövesse az alábbi [részletes lépéseket](active-directory-v2-app-registration.md).  Győződjön meg arról, hogy:

* Másolja le hello **alkalmazásazonosító** tooyour app hozzárendelve, szüksége lehet rájuk hamarosan.

A visual studio megoldás is tartalmaz egy "TodoListClient", amely egy egyszerű WPF-alkalmazás.  hello TodoListClient használt toodemonstrate módját a felhasználó bejelentkezik, és hogyan adhat ki egy ügyfél-kérelmek tooyour Web API.  Ebben az esetben is hello TodoListClient és hello TodoListService képviselik hello ugyanahhoz az alkalmazáshoz.  tooconfigure hello TodoListClient, akkor is:

* Adja hozzá a hello **Mobile** platform az alkalmazásra vonatkozóan.

## <a name="install-owin"></a>Az OWIN telepítése
Most, hogy egy alkalmazás regisztrálása tooset kell fel az alkalmazás toocommunicate hello v2.0-végponttal rendelés toovalidate bejövő kérelmek & jogkivonatokat.

* toobegin, nyissa meg hello megoldást, és adja hozzá a hello OWIN köztes NuGet csomagjainak toohello TodoListService projekt hello Csomagkezelő konzol használatával.

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
PM> Install-Package Microsoft.IdentityModel.Protocol.Extensions -ProjectName TodoListService
```

## <a name="configure-oauth-authentication"></a>OAuth-hitelesítés konfigurálása
* Adja hozzá egy OWIN indítási osztály toohello TodoListService projekt nevű `Startup.cs`.  Kattintson a jobb gombbal a projekt hello--> **Hozzáadás** --> **új elem** --> "OWIN" keresése.  hello OWIN köztes által aktivált hello `Configuration(…)` módszer az alkalmazás indításakor.
* Hello osztálydeklaráció túl módosítása`public partial class Startup` -azt korábban már megvalósított Ez az osztály tartozik, egy másik fájlban.  A hello `Configuration(…)` módszert, győződjön meg a hívás tooConfgureAuth(...) tooset hitelesítés a webalkalmazás.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

* Nyissa meg hello fájl `App_Start\Startup.Auth.cs` és valósíthatnak meg hello `ConfigureAuth(…)` metódus, amely a v2.0-végponttól hello hello Web API tooaccept jogkivonatok beállításához.

```C#
public void ConfigureAuth(IAppBuilder app)
{
        var tvps = new TokenValidationParameters
        {
                // In this app, hello TodoListClient and TodoListService
                // are represented using hello same Application Id - we use
                // hello Application Id toorepresent hello audience, or the
                // intended recipient of tokens.

                ValidAudience = clientId,

                // In a real applicaiton, you might use issuer validation to
                // verify that hello user's organization (if applicable) has
                // signed up for hello app.  Here, we'll just turn it off.

                ValidateIssuer = false,
        };

        // Set up hello OWIN pipeline toouse OAuth 2.0 Bearer authentication.
        // hello options provided here tell hello middleware about hello type of tokens
        // that will be recieved, which are JWTs for hello v2.0 endpoint.

        // NOTE: hello usual WindowsAzureActiveDirectoryBearerAuthenticaitonMiddleware uses a
        // metadata endpoint which is not supported by hello v2.0 endpoint.  Instead, this
        // OpenIdConenctCachingSecurityTokenProvider can be used toofetch & use hello OpenIdConnect
        // metadata document.

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {
                AccessTokenFormat = new Microsoft.Owin.Security.Jwt.JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider("https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration")),
        });
}
```

* Mostantól a `[Authorize]` attribútumok tooprotect, a tartományvezérlőket és a műveletek OAuth 2.0 tulajdonosi hitelesítéssel.  Adja a hello `Controllers\TodoListController.cs` osztály engedélyezés címke használatával.  Ezzel kikényszeríti a hello felhasználói toosign a lap elérése előtt.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

* Amikor egy jogosult hívó sikeresen hív meg egyik hello `TodoListController` API-k, hello művelet is kell hozzáférhetnek tooinformation hello hívó kapcsolatban.  OWIN hozzáférés toohello jogcímeket belül hello tulajdonosi jogkivonattal hello keresztül biztosít `ClaimsPrincpal` objektum.  

```C#
public IEnumerable<TodoItem> Get()
{
    // You can use hello ClaimsPrincipal tooaccess information about the
    // user making hello call.  In this case, we use hello 'sub' or
    // NameIdentifier claim tooserve as a key for hello tasks in hello data store.

    Claim subject = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier);

    return from todo in todoBag
           where todo.Owner == subject.Value
           select todo;
}
```

* Végül nyissa meg a hello `web.config` hello TodoListService projekt gyökerében hello fájlt, és adja meg a konfigurációs értékek hello `<appSettings>` szakasz.
  * A `ida:Audience` hello van **alkalmazásazonosító** hello portálon megadott hello alkalmazás.

## <a name="configure-hello-client-app"></a>Hello ügyfélalkalmazás konfigurálása
Ahhoz, hogy hello Todo lista szolgáltatás működés közben, tooconfigure hello Todo lista ügyfél szüksége, hogy azok tokenek beszerzése hello v2.0-végpontra, és ellenőrizze a hívások toohello szolgáltatás.

* Hello TodoListClient projektben nyissa meg `App.config` , majd írja be a konfigurációs értékek hello `<appSettings>` szakasz.
  * A `ida:ClientId` hello portálról másolt alkalmazásazonosító.

Végezetül tiszta, felépítéséhez, és minden olyan projekthez futtatásához!  Most már rendelkezik egy .NET MVC webes API-t, amely fogadja a személyes Microsoft-fiókot is származó jogkivonatokat és a munkahelyi vagy iskolai fiókját.  Jelentkezzen be a hello TodoListClient, és a webes api tooadd feladatok toohello felhasználói feladatlistát hívja.

Referenciaként hello befejeződött (a konfigurációs értékek nélkül) minta [is letöltheti a .zip Itt](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip), vagy a Githubból is klónozhatja:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git```

## <a name="next-steps"></a>Következő lépések
Kiegészítő témakörök most már továbbléphet.  Érdemes lehet tootry:

[Webes API-k egy webalkalmazásból hívja >>](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)

További forrásokért tekintse meg:

* [hello v2.0 – útmutató fejlesztőknek >>](active-directory-appmodel-v2-overview.md)
* [StackOverflow "azure-active-directory" címke >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Biztonsági frissítések termékeinkhez
Javasoljuk, tooget értesítést a bekövetkező biztonsági incidensekről ellátogatva [ezen a lapon](https://technet.microsoft.com/security/dd252948) és előfizetés tooSecurity tanácsadói riasztásokra.
