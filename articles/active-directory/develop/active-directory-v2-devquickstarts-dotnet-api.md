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
# <a name="secure-an-mvc-web-api"></a><span data-ttu-id="fdc16-103">Egy MVC webes API biztonságossá tétele</span><span class="sxs-lookup"><span data-stu-id="fdc16-103">Secure an MVC web API</span></span>
<span data-ttu-id="fdc16-104">Az Azure Active Directory hello v2.0-végpontra, megvédheti a Web API használatával [OAuth 2.0](active-directory-v2-protocols.md) hozzáférési jogkivonatok, mindkét személyes Microsoft-fiókkal rendelkező felhasználók és a munkahelyi vagy iskolai fiókok toosecurely hozzáférni a Web API engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="fdc16-104">With Azure Active Directory hello v2.0 endpoint, you can protect a Web API using [OAuth 2.0](active-directory-v2-protocols.md) access tokens, enabling users with both personal Microsoft account and work or school accounts toosecurely access your Web API.</span></span>

> [!NOTE]
> <span data-ttu-id="fdc16-105">Nem minden Azure Active Directory forgatókönyvek és funkciók támogatják hello v2.0-végponttól.</span><span class="sxs-lookup"><span data-stu-id="fdc16-105">Not all Azure Active Directory scenarios & features are supported by hello v2.0 endpoint.</span></span>  <span data-ttu-id="fdc16-106">toodetermine használatát hello v2.0-végpontra, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="fdc16-106">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
>
>

<span data-ttu-id="fdc16-107">ASP.NET webes API-k Ez elvégezhető a .NET-keretrendszer 4.5 része a Microsoft OWIN köztes használatával.</span><span class="sxs-lookup"><span data-stu-id="fdc16-107">In ASP.NET web APIs, you can accomplish this using Microsoft’s OWIN middleware included in .NET Framework 4.5.</span></span>  <span data-ttu-id="fdc16-108">Itt a "lista tooDo" MVC webes API, amely lehetővé teszi az ügyfelek a felhasználók feladatlistáit toocreate és olvasási feladatok OWIN toobuild fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="fdc16-108">Here we’ll use OWIN toobuild a "tooDo List" MVC Web API that allows clients toocreate and read tasks from a user's To-Do list.</span></span>  <span data-ttu-id="fdc16-109">ellenőrzi, hogy a hello webes API-t, hogy a bejövő kérelmek tartalmaznak egy érvényes hozzáférési jogkivonatot, és bármilyen kérelmeket, amelyek nem teljesíti az ellenőrző egy védett útvonal.</span><span class="sxs-lookup"><span data-stu-id="fdc16-109">hello web API will validate that incoming requests contain a valid access token and reject any requests that do not pass validation on a protected route.</span></span>  <span data-ttu-id="fdc16-110">Ez a minta a Visual Studio 2015 használatával lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="fdc16-110">This sample was built using Visual Studio 2015.</span></span>

## <a name="download"></a><span data-ttu-id="fdc16-111">Letöltés</span><span class="sxs-lookup"><span data-stu-id="fdc16-111">Download</span></span>
<span data-ttu-id="fdc16-112">az oktatóanyag kódjának hello kezelt [a Githubon](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet).</span><span class="sxs-lookup"><span data-stu-id="fdc16-112">hello code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet).</span></span>  <span data-ttu-id="fdc16-113">toofollow mellett, akkor [töltse le a .zip hello alkalmazás vázát](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) vagy a Klónozás hello vázat:</span><span class="sxs-lookup"><span data-stu-id="fdc16-113">toofollow along, you can [download hello app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) or clone hello skeleton:</span></span>

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

<span data-ttu-id="fdc16-114">hello üres alkalmazás tartalmazza az összes hello bolierplate kód egyszerű API-t, de összes hello identitás kapcsolatos eleme hiányzik.</span><span class="sxs-lookup"><span data-stu-id="fdc16-114">hello skeleton app includes all hello boilerplate code for a simple API, but is missing all of hello identity-related pieces.</span></span> <span data-ttu-id="fdc16-115">Ha nem szeretné mentén toofollow, hanem klónozhat vagy [letöltése befejeződött hello minta](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="fdc16-115">If you don't want toofollow along, you can instead clone or [download hello completed sample](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip).</span></span>

```
git clone https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

## <a name="register-an-app"></a><span data-ttu-id="fdc16-116">Alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="fdc16-116">Register an app</span></span>
<span data-ttu-id="fdc16-117">Hozzon létre egy új alkalmazást [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy kövesse az alábbi [részletes lépéseket](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="fdc16-117">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="fdc16-118">Győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="fdc16-118">Make sure to:</span></span>

* <span data-ttu-id="fdc16-119">Másolja le hello **alkalmazásazonosító** tooyour app hozzárendelve, szüksége lehet rájuk hamarosan.</span><span class="sxs-lookup"><span data-stu-id="fdc16-119">Copy down hello **Application Id** assigned tooyour app, you'll need it soon.</span></span>

<span data-ttu-id="fdc16-120">A visual studio megoldás is tartalmaz egy "TodoListClient", amely egy egyszerű WPF-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fdc16-120">This visual studio solution also contains a "TodoListClient", which is a simple WPF app.</span></span>  <span data-ttu-id="fdc16-121">hello TodoListClient használt toodemonstrate módját a felhasználó bejelentkezik, és hogyan adhat ki egy ügyfél-kérelmek tooyour Web API.</span><span class="sxs-lookup"><span data-stu-id="fdc16-121">hello TodoListClient is used toodemonstrate how a user signs-in and how a client can issue requests tooyour Web API.</span></span>  <span data-ttu-id="fdc16-122">Ebben az esetben is hello TodoListClient és hello TodoListService képviselik hello ugyanahhoz az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="fdc16-122">In this case, both hello TodoListClient and hello TodoListService are represented by hello same app.</span></span>  <span data-ttu-id="fdc16-123">tooconfigure hello TodoListClient, akkor is:</span><span class="sxs-lookup"><span data-stu-id="fdc16-123">tooconfigure hello TodoListClient, you should also:</span></span>

* <span data-ttu-id="fdc16-124">Adja hozzá a hello **Mobile** platform az alkalmazásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="fdc16-124">Add hello **Mobile** platform for your app.</span></span>

## <a name="install-owin"></a><span data-ttu-id="fdc16-125">Az OWIN telepítése</span><span class="sxs-lookup"><span data-stu-id="fdc16-125">Install OWIN</span></span>
<span data-ttu-id="fdc16-126">Most, hogy egy alkalmazás regisztrálása tooset kell fel az alkalmazás toocommunicate hello v2.0-végponttal rendelés toovalidate bejövő kérelmek & jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="fdc16-126">Now that you’ve registered an app, you need tooset up your app toocommunicate with hello v2.0 endpoint in order toovalidate incoming requests & tokens.</span></span>

* <span data-ttu-id="fdc16-127">toobegin, nyissa meg hello megoldást, és adja hozzá a hello OWIN köztes NuGet csomagjainak toohello TodoListService projekt hello Csomagkezelő konzol használatával.</span><span class="sxs-lookup"><span data-stu-id="fdc16-127">toobegin, open hello solution and add hello OWIN middleware NuGet packages toohello TodoListService project using hello Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
PM> Install-Package Microsoft.IdentityModel.Protocol.Extensions -ProjectName TodoListService
```

## <a name="configure-oauth-authentication"></a><span data-ttu-id="fdc16-128">OAuth-hitelesítés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fdc16-128">Configure OAuth authentication</span></span>
* <span data-ttu-id="fdc16-129">Adja hozzá egy OWIN indítási osztály toohello TodoListService projekt nevű `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="fdc16-129">Add an OWIN Startup class toohello TodoListService project called `Startup.cs`.</span></span>  <span data-ttu-id="fdc16-130">Kattintson a jobb gombbal a projekt hello--> **Hozzáadás** --> **új elem** --> "OWIN" keresése.</span><span class="sxs-lookup"><span data-stu-id="fdc16-130">Right click on hello project --> **Add** --> **New Item** --> Search for “OWIN”.</span></span>  <span data-ttu-id="fdc16-131">hello OWIN köztes által aktivált hello `Configuration(…)` módszer az alkalmazás indításakor.</span><span class="sxs-lookup"><span data-stu-id="fdc16-131">hello OWIN middleware will invoke hello `Configuration(…)` method when your app starts.</span></span>
* <span data-ttu-id="fdc16-132">Hello osztálydeklaráció túl módosítása`public partial class Startup` -azt korábban már megvalósított Ez az osztály tartozik, egy másik fájlban.</span><span class="sxs-lookup"><span data-stu-id="fdc16-132">Change hello class declaration too`public partial class Startup` - we’ve already implemented part of this class for you in another file.</span></span>  <span data-ttu-id="fdc16-133">A hello `Configuration(…)` módszert, győződjön meg a hívás tooConfgureAuth(...) tooset hitelesítés a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fdc16-133">In hello `Configuration(…)` method, make a call tooConfgureAuth(…) tooset up authentication for your web app.</span></span>

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

* <span data-ttu-id="fdc16-134">Nyissa meg hello fájl `App_Start\Startup.Auth.cs` és valósíthatnak meg hello `ConfigureAuth(…)` metódus, amely a v2.0-végponttól hello hello Web API tooaccept jogkivonatok beállításához.</span><span class="sxs-lookup"><span data-stu-id="fdc16-134">Open hello file `App_Start\Startup.Auth.cs` and implement hello `ConfigureAuth(…)` method, which will set up hello Web API tooaccept tokens from hello v2.0 endpoint.</span></span>

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

* <span data-ttu-id="fdc16-135">Mostantól a `[Authorize]` attribútumok tooprotect, a tartományvezérlőket és a műveletek OAuth 2.0 tulajdonosi hitelesítéssel.</span><span class="sxs-lookup"><span data-stu-id="fdc16-135">Now you can use `[Authorize]` attributes tooprotect your controllers and actions with OAuth 2.0 bearer authentication.</span></span>  <span data-ttu-id="fdc16-136">Adja a hello `Controllers\TodoListController.cs` osztály engedélyezés címke használatával.</span><span class="sxs-lookup"><span data-stu-id="fdc16-136">Decorate hello `Controllers\TodoListController.cs` class with an authorize tag.</span></span>  <span data-ttu-id="fdc16-137">Ezzel kikényszeríti a hello felhasználói toosign a lap elérése előtt.</span><span class="sxs-lookup"><span data-stu-id="fdc16-137">This will force hello user toosign in before accessing that page.</span></span>

```C#
[Authorize]
public class TodoListController : ApiController
{
```

* <span data-ttu-id="fdc16-138">Amikor egy jogosult hívó sikeresen hív meg egyik hello `TodoListController` API-k, hello művelet is kell hozzáférhetnek tooinformation hello hívó kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="fdc16-138">When an authorized caller successfully invokes one of hello `TodoListController` APIs, hello action might need access tooinformation about hello caller.</span></span>  <span data-ttu-id="fdc16-139">OWIN hozzáférés toohello jogcímeket belül hello tulajdonosi jogkivonattal hello keresztül biztosít `ClaimsPrincpal` objektum.</span><span class="sxs-lookup"><span data-stu-id="fdc16-139">OWIN provides access toohello claims inside hello bearer token via hello `ClaimsPrincpal` object.</span></span>  

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

* <span data-ttu-id="fdc16-140">Végül nyissa meg a hello `web.config` hello TodoListService projekt gyökerében hello fájlt, és adja meg a konfigurációs értékek hello `<appSettings>` szakasz.</span><span class="sxs-lookup"><span data-stu-id="fdc16-140">Finally, open hello `web.config` file in hello root of hello TodoListService project, and enter your configuration values in hello `<appSettings>` section.</span></span>
  * <span data-ttu-id="fdc16-141">A `ida:Audience` hello van **alkalmazásazonosító** hello portálon megadott hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fdc16-141">Your `ida:Audience` is hello **Application Id** of hello app that you entered in hello portal.</span></span>

## <a name="configure-hello-client-app"></a><span data-ttu-id="fdc16-142">Hello ügyfélalkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fdc16-142">Configure hello client app</span></span>
<span data-ttu-id="fdc16-143">Ahhoz, hogy hello Todo lista szolgáltatás működés közben, tooconfigure hello Todo lista ügyfél szüksége, hogy azok tokenek beszerzése hello v2.0-végpontra, és ellenőrizze a hívások toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="fdc16-143">Before you can see hello Todo List Service in action, you need tooconfigure hello Todo List Client so it can get tokens from hello v2.0 endpoint and make calls toohello service.</span></span>

* <span data-ttu-id="fdc16-144">Hello TodoListClient projektben nyissa meg `App.config` , majd írja be a konfigurációs értékek hello `<appSettings>` szakasz.</span><span class="sxs-lookup"><span data-stu-id="fdc16-144">In hello TodoListClient project, open `App.config` and enter your configuration values in hello `<appSettings>` section.</span></span>
  * <span data-ttu-id="fdc16-145">A `ida:ClientId` hello portálról másolt alkalmazásazonosító.</span><span class="sxs-lookup"><span data-stu-id="fdc16-145">Your `ida:ClientId` Application Id you copied from hello portal.</span></span>

<span data-ttu-id="fdc16-146">Végezetül tiszta, felépítéséhez, és minden olyan projekthez futtatásához!</span><span class="sxs-lookup"><span data-stu-id="fdc16-146">Finally, clean, build and run each project!</span></span>  <span data-ttu-id="fdc16-147">Most már rendelkezik egy .NET MVC webes API-t, amely fogadja a személyes Microsoft-fiókot is származó jogkivonatokat és a munkahelyi vagy iskolai fiókját.</span><span class="sxs-lookup"><span data-stu-id="fdc16-147">You now have a .NET MVC Web API that accepts tokens from both personal Microsoft accounts and work or school accounts.</span></span>  <span data-ttu-id="fdc16-148">Jelentkezzen be a hello TodoListClient, és a webes api tooadd feladatok toohello felhasználói feladatlistát hívja.</span><span class="sxs-lookup"><span data-stu-id="fdc16-148">Sign into hello TodoListClient, and call your web api tooadd tasks toohello user's To-Do list.</span></span>

<span data-ttu-id="fdc16-149">Referenciaként hello befejeződött (a konfigurációs értékek nélkül) minta [is letöltheti a .zip Itt](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip), vagy a Githubból is klónozhatja:</span><span class="sxs-lookup"><span data-stu-id="fdc16-149">For reference, hello completed sample (without your configuration values) [is provided as a .zip here](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip), or you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git```

## <a name="next-steps"></a><span data-ttu-id="fdc16-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fdc16-150">Next steps</span></span>
<span data-ttu-id="fdc16-151">Kiegészítő témakörök most már továbbléphet.</span><span class="sxs-lookup"><span data-stu-id="fdc16-151">You can now move onto additional topics.</span></span>  <span data-ttu-id="fdc16-152">Érdemes lehet tootry:</span><span class="sxs-lookup"><span data-stu-id="fdc16-152">You may want tootry:</span></span>

[<span data-ttu-id="fdc16-153">Webes API-k egy webalkalmazásból hívja >></span><span class="sxs-lookup"><span data-stu-id="fdc16-153">Calling a Web API from a Web App >></span></span>](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)

<span data-ttu-id="fdc16-154">További forrásokért tekintse meg:</span><span class="sxs-lookup"><span data-stu-id="fdc16-154">For additional resources, check out:</span></span>

* [<span data-ttu-id="fdc16-155">hello v2.0 – útmutató fejlesztőknek >></span><span class="sxs-lookup"><span data-stu-id="fdc16-155">hello v2.0 developer guide >></span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="fdc16-156">StackOverflow "azure-active-directory" címke >></span><span class="sxs-lookup"><span data-stu-id="fdc16-156">StackOverflow "azure-active-directory" tag >></span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="fdc16-157">Biztonsági frissítések termékeinkhez</span><span class="sxs-lookup"><span data-stu-id="fdc16-157">Get security updates for our products</span></span>
<span data-ttu-id="fdc16-158">Javasoljuk, tooget értesítést a bekövetkező biztonsági incidensekről ellátogatva [ezen a lapon](https://technet.microsoft.com/security/dd252948) és előfizetés tooSecurity tanácsadói riasztásokra.</span><span class="sxs-lookup"><span data-stu-id="fdc16-158">We encourage you tooget notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing tooSecurity Advisory Alerts.</span></span>
