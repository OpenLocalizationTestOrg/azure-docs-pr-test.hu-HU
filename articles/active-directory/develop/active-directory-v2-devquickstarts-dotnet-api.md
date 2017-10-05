---
title: "Bejelentkezés hozzáadása egy .NET MVC webes API-k használata az Azure AD v2.0-végponttól |} Microsoft Docs"
description: "Hogyan hozhat létre egy .NET MVC webes API-t, amely mindkét személyes Microsoft Account jogkivonatokat fogad el és a munkahelyi vagy iskolai fiókját."
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
ms.openlocfilehash: b2d7bbfcd9218698f71e9dfdb1ad5d9ff8740f5e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="secure-an-mvc-web-api"></a><span data-ttu-id="409ce-103">Egy MVC webes API biztonságossá tétele</span><span class="sxs-lookup"><span data-stu-id="409ce-103">Secure an MVC web API</span></span>
<span data-ttu-id="409ce-104">Az Azure Active Directoryban a v2.0-végpontra, megvédheti a Web API használatával [OAuth 2.0](active-directory-v2-protocols.md) hozzáférési jogkivonatok, mindkét személyes Microsoft-fiókkal rendelkező felhasználók és a munkahelyi vagy iskolai fiókok biztonságos hozzáférés a webes API.</span><span class="sxs-lookup"><span data-stu-id="409ce-104">With Azure Active Directory the v2.0 endpoint, you can protect a Web API using [OAuth 2.0](active-directory-v2-protocols.md) access tokens, enabling users with both personal Microsoft account and work or school accounts to securely access your Web API.</span></span>

> [!NOTE]
> <span data-ttu-id="409ce-105">Nem minden Azure Active Directory forgatókönyvek és funkciók támogatják a v2.0-végponttól.</span><span class="sxs-lookup"><span data-stu-id="409ce-105">Not all Azure Active Directory scenarios & features are supported by the v2.0 endpoint.</span></span>  <span data-ttu-id="409ce-106">Annak meghatározásához, ha a v2.0-végponttal kell használnia, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="409ce-106">To determine if you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
>
>

<span data-ttu-id="409ce-107">ASP.NET webes API-k Ez elvégezhető a .NET-keretrendszer 4.5 része a Microsoft OWIN köztes használatával.</span><span class="sxs-lookup"><span data-stu-id="409ce-107">In ASP.NET web APIs, you can accomplish this using Microsoft’s OWIN middleware included in .NET Framework 4.5.</span></span>  <span data-ttu-id="409ce-108">Itt hozhat létre egy "Teendőlista" MVC webes API-t, amely lehetővé teszi az ügyfelek létrehozásához, és feladatokat beolvasni a felhasználói feladatlistában OWIN fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="409ce-108">Here we’ll use OWIN to build a "To Do List" MVC Web API that allows clients to create and read tasks from a user's To-Do list.</span></span>  <span data-ttu-id="409ce-109">A webes API-t, hogy a bejövő kérelmek tartalmaznak egy érvényes hozzáférési jogkivonatot, és bármilyen kérelmeket, amelyek nem teljesíti az ellenőrző egy védett útvonal fogja ellenőrizni.</span><span class="sxs-lookup"><span data-stu-id="409ce-109">The web API will validate that incoming requests contain a valid access token and reject any requests that do not pass validation on a protected route.</span></span>  <span data-ttu-id="409ce-110">Ez a minta a Visual Studio 2015 használatával lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="409ce-110">This sample was built using Visual Studio 2015.</span></span>

## <a name="download"></a><span data-ttu-id="409ce-111">Letöltés</span><span class="sxs-lookup"><span data-stu-id="409ce-111">Download</span></span>
<span data-ttu-id="409ce-112">Az oktatóanyag kódjának [karbantartása a GitHubon történik](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet).</span><span class="sxs-lookup"><span data-stu-id="409ce-112">The code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet).</span></span>  <span data-ttu-id="409ce-113">Követéséhez is [töltse le az alkalmazás vázát egy .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) vagy klónozza a vázat:</span><span class="sxs-lookup"><span data-stu-id="409ce-113">To follow along, you can [download the app's skeleton as a .zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) or clone the skeleton:</span></span>

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

<span data-ttu-id="409ce-114">Az üres alkalmazás egyszerű API-t a bolierplate kódot tartalmaz, de hiányzik az identitás-kapcsolódó darab mindegyikét.</span><span class="sxs-lookup"><span data-stu-id="409ce-114">The skeleton app includes all the boilerplate code for a simple API, but is missing all of the identity-related pieces.</span></span> <span data-ttu-id="409ce-115">Ha nem szeretné követéséhez, hanem klónozhat vagy [töltse le az elkészült mintát](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="409ce-115">If you don't want to follow along, you can instead clone or [download the completed sample](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip).</span></span>

```
git clone https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

## <a name="register-an-app"></a><span data-ttu-id="409ce-116">Alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="409ce-116">Register an app</span></span>
<span data-ttu-id="409ce-117">Hozzon létre egy új alkalmazást [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy kövesse az alábbi [részletes lépéseket](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="409ce-117">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="409ce-118">Győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="409ce-118">Make sure to:</span></span>

* <span data-ttu-id="409ce-119">Másolja le a **alkalmazásazonosító** be az alkalmazáshoz hozzárendelt szüksége lehet rájuk hamarosan.</span><span class="sxs-lookup"><span data-stu-id="409ce-119">Copy down the **Application Id** assigned to your app, you'll need it soon.</span></span>

<span data-ttu-id="409ce-120">A visual studio megoldás is tartalmaz egy "TodoListClient", amely egy egyszerű WPF-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="409ce-120">This visual studio solution also contains a "TodoListClient", which is a simple WPF app.</span></span>  <span data-ttu-id="409ce-121">A TodoListClient bemutatják, hogyan egy felhasználó bejelentkezik, és hogyan ügyfél is kérelmeket kiadni a Web API segítségével.</span><span class="sxs-lookup"><span data-stu-id="409ce-121">The TodoListClient is used to demonstrate how a user signs-in and how a client can issue requests to your Web API.</span></span>  <span data-ttu-id="409ce-122">Ebben az esetben a TodoListClient, mind a TodoListService jelölik ugyanahhoz az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="409ce-122">In this case, both the TodoListClient and the TodoListService are represented by the same app.</span></span>  <span data-ttu-id="409ce-123">A TodoListClient konfigurálásához el a következőket is:</span><span class="sxs-lookup"><span data-stu-id="409ce-123">To configure the TodoListClient, you should also:</span></span>

* <span data-ttu-id="409ce-124">Adja hozzá a **Mobile** platform az alkalmazásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="409ce-124">Add the **Mobile** platform for your app.</span></span>

## <a name="install-owin"></a><span data-ttu-id="409ce-125">Az OWIN telepítése</span><span class="sxs-lookup"><span data-stu-id="409ce-125">Install OWIN</span></span>
<span data-ttu-id="409ce-126">Most, hogy egy alkalmazás regisztrálása kell ellenőrizni fogja a bejövő kérések és a tokeneket a v2.0-végpontra kommunikálni az alkalmazás beállítása.</span><span class="sxs-lookup"><span data-stu-id="409ce-126">Now that you’ve registered an app, you need to set up your app to communicate with the v2.0 endpoint in order to validate incoming requests & tokens.</span></span>

* <span data-ttu-id="409ce-127">Első lépésként nyissa meg a megoldást, és az OWIN köztes NuGet-csomagok hozzáadása a TodoListService projekthez a Csomagkezelő konzol használatával.</span><span class="sxs-lookup"><span data-stu-id="409ce-127">To begin, open the solution and add the OWIN middleware NuGet packages to the TodoListService project using the Package Manager Console.</span></span>

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
PM> Install-Package Microsoft.IdentityModel.Protocol.Extensions -ProjectName TodoListService
```

## <a name="configure-oauth-authentication"></a><span data-ttu-id="409ce-128">OAuth-hitelesítés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="409ce-128">Configure OAuth authentication</span></span>
* <span data-ttu-id="409ce-129">Egy OWIN indítási osztály hozzáadása a TodoListService projekt neve `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="409ce-129">Add an OWIN Startup class to the TodoListService project called `Startup.cs`.</span></span>  <span data-ttu-id="409ce-130">Kattintson a jobb gombbal a projektre--> **Hozzáadás** --> **új elem** --> "OWIN" keresése.</span><span class="sxs-lookup"><span data-stu-id="409ce-130">Right click on the project --> **Add** --> **New Item** --> Search for “OWIN”.</span></span>  <span data-ttu-id="409ce-131">Az OWIN közbenső szoftver meghívja a `Configuration(…)` metódust az alkalmazás indulásakor.</span><span class="sxs-lookup"><span data-stu-id="409ce-131">The OWIN middleware will invoke the `Configuration(…)` method when your app starts.</span></span>
* <span data-ttu-id="409ce-132">Módosítsa az osztálydeklaráció való `public partial class Startup` -azt korábban már megvalósított Ez az osztály tartozik, egy másik fájlban.</span><span class="sxs-lookup"><span data-stu-id="409ce-132">Change the class declaration to `public partial class Startup` - we’ve already implemented part of this class for you in another file.</span></span>  <span data-ttu-id="409ce-133">Az a `Configuration(…)` metódus hívása ConfgureAuth(...) hitelesítés a webalkalmazás beállítása legyen.</span><span class="sxs-lookup"><span data-stu-id="409ce-133">In the `Configuration(…)` method, make a call to ConfgureAuth(…) to set up authentication for your web app.</span></span>

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

* <span data-ttu-id="409ce-134">Nyissa meg a fájlt `App_Start\Startup.Auth.cs` és megvalósítását a `ConfigureAuth(…)` metódus, amely elfogadja a v2.0-végpontra származó jogkivonatokat a Web API beállításához.</span><span class="sxs-lookup"><span data-stu-id="409ce-134">Open the file `App_Start\Startup.Auth.cs` and implement the `ConfigureAuth(…)` method, which will set up the Web API to accept tokens from the v2.0 endpoint.</span></span>

```C#
public void ConfigureAuth(IAppBuilder app)
{
        var tvps = new TokenValidationParameters
        {
                // In this app, the TodoListClient and TodoListService
                // are represented using the same Application Id - we use
                // the Application Id to represent the audience, or the
                // intended recipient of tokens.

                ValidAudience = clientId,

                // In a real applicaiton, you might use issuer validation to
                // verify that the user's organization (if applicable) has
                // signed up for the app.  Here, we'll just turn it off.

                ValidateIssuer = false,
        };

        // Set up the OWIN pipeline to use OAuth 2.0 Bearer authentication.
        // The options provided here tell the middleware about the type of tokens
        // that will be recieved, which are JWTs for the v2.0 endpoint.

        // NOTE: The usual WindowsAzureActiveDirectoryBearerAuthenticaitonMiddleware uses a
        // metadata endpoint which is not supported by the v2.0 endpoint.  Instead, this
        // OpenIdConenctCachingSecurityTokenProvider can be used to fetch & use the OpenIdConnect
        // metadata document.

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {
                AccessTokenFormat = new Microsoft.Owin.Security.Jwt.JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider("https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration")),
        });
}
```

* <span data-ttu-id="409ce-135">Mostantól a `[Authorize]` attribútumok a tartományvezérlők és az OAuth 2.0 tulajdonosi hitelesítéssel műveletek védelme érdekében.</span><span class="sxs-lookup"><span data-stu-id="409ce-135">Now you can use `[Authorize]` attributes to protect your controllers and actions with OAuth 2.0 bearer authentication.</span></span>  <span data-ttu-id="409ce-136">Adja a `Controllers\TodoListController.cs` osztály engedélyezés címke használatával.</span><span class="sxs-lookup"><span data-stu-id="409ce-136">Decorate the `Controllers\TodoListController.cs` class with an authorize tag.</span></span>  <span data-ttu-id="409ce-137">Ezzel kikényszeríti a felhasználót, hogy jelentkezzen be a lap elérése előtt.</span><span class="sxs-lookup"><span data-stu-id="409ce-137">This will force the user to sign in before accessing that page.</span></span>

```C#
[Authorize]
public class TodoListController : ApiController
{
```

* <span data-ttu-id="409ce-138">Amikor egy jogosult hívó sikeresen hív meg, egy a `TodoListController` API-k, a művelet módosítania kell információkhoz juthat a hívóról.</span><span class="sxs-lookup"><span data-stu-id="409ce-138">When an authorized caller successfully invokes one of the `TodoListController` APIs, the action might need access to information about the caller.</span></span>  <span data-ttu-id="409ce-139">A jogcímek belül a tulajdonosi jogkivonattal keresztül hozzáférést biztosít az OWIN a `ClaimsPrincpal` objektum.</span><span class="sxs-lookup"><span data-stu-id="409ce-139">OWIN provides access to the claims inside the bearer token via the `ClaimsPrincpal` object.</span></span>  

```C#
public IEnumerable<TodoItem> Get()
{
    // You can use the ClaimsPrincipal to access information about the
    // user making the call.  In this case, we use the 'sub' or
    // NameIdentifier claim to serve as a key for the tasks in the data store.

    Claim subject = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier);

    return from todo in todoBag
           where todo.Owner == subject.Value
           select todo;
}
```

* <span data-ttu-id="409ce-140">Végül nyissa meg a `web.config` a TodoListService projekt gyökérkönyvtárában található fájlt, és adja meg a konfigurációs értékeit a `<appSettings>` szakasz.</span><span class="sxs-lookup"><span data-stu-id="409ce-140">Finally, open the `web.config` file in the root of the TodoListService project, and enter your configuration values in the `<appSettings>` section.</span></span>
  * <span data-ttu-id="409ce-141">A `ida:Audience` van a **alkalmazásazonosító** az alkalmazás a portálon megadott.</span><span class="sxs-lookup"><span data-stu-id="409ce-141">Your `ida:Audience` is the **Application Id** of the app that you entered in the portal.</span></span>

## <a name="configure-the-client-app"></a><span data-ttu-id="409ce-142">Az ügyfélalkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="409ce-142">Configure the client app</span></span>
<span data-ttu-id="409ce-143">Ahhoz, hogy a művelet a teendőlista lista szolgáltatás, a teendőlista lista ügyfél konfigurálása, hogy azok tokenek beszerzése a v2.0-végpontra és a szolgáltatás meghíváshoz kell.</span><span class="sxs-lookup"><span data-stu-id="409ce-143">Before you can see the Todo List Service in action, you need to configure the Todo List Client so it can get tokens from the v2.0 endpoint and make calls to the service.</span></span>

* <span data-ttu-id="409ce-144">A TodoListClient projektben nyissa meg a `App.config` , és írja be a konfigurációs értékeit a `<appSettings>` szakasz.</span><span class="sxs-lookup"><span data-stu-id="409ce-144">In the TodoListClient project, open `App.config` and enter your configuration values in the `<appSettings>` section.</span></span>
  * <span data-ttu-id="409ce-145">A `ida:ClientId` alkalmazásazonosító másolta a portálról.</span><span class="sxs-lookup"><span data-stu-id="409ce-145">Your `ida:ClientId` Application Id you copied from the portal.</span></span>

<span data-ttu-id="409ce-146">Végezetül tiszta, felépítéséhez, és minden olyan projekthez futtatásához!</span><span class="sxs-lookup"><span data-stu-id="409ce-146">Finally, clean, build and run each project!</span></span>  <span data-ttu-id="409ce-147">Most már rendelkezik egy .NET MVC webes API-t, amely fogadja a személyes Microsoft-fiókot is származó jogkivonatokat és a munkahelyi vagy iskolai fiókját.</span><span class="sxs-lookup"><span data-stu-id="409ce-147">You now have a .NET MVC Web API that accepts tokens from both personal Microsoft accounts and work or school accounts.</span></span>  <span data-ttu-id="409ce-148">Jelentkezzen be a TodoListClient, és hívja meg a webes api tevékenységek hozzáadása a felhasználói feladatlistában.</span><span class="sxs-lookup"><span data-stu-id="409ce-148">Sign into the TodoListClient, and call your web api to add tasks to the user's To-Do list.</span></span>

<span data-ttu-id="409ce-149">Az elkészült mintát (a konfigurációs értékek nélkül) referenciaként [is letöltheti a .zip Itt](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip), vagy a Githubból is klónozhatja:</span><span class="sxs-lookup"><span data-stu-id="409ce-149">For reference, the completed sample (without your configuration values) [is provided as a .zip here](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip), or you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git```

## <a name="next-steps"></a><span data-ttu-id="409ce-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="409ce-150">Next steps</span></span>
<span data-ttu-id="409ce-151">Kiegészítő témakörök most már továbbléphet.</span><span class="sxs-lookup"><span data-stu-id="409ce-151">You can now move onto additional topics.</span></span>  <span data-ttu-id="409ce-152">Előfordulhat, hogy ki szeretné próbálni:</span><span class="sxs-lookup"><span data-stu-id="409ce-152">You may want to try:</span></span>

[<span data-ttu-id="409ce-153">Webes API-k egy webalkalmazásból hívja >></span><span class="sxs-lookup"><span data-stu-id="409ce-153">Calling a Web API from a Web App >></span></span>](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)

<span data-ttu-id="409ce-154">További forrásokért tekintse meg:</span><span class="sxs-lookup"><span data-stu-id="409ce-154">For additional resources, check out:</span></span>

* [<span data-ttu-id="409ce-155">A v2.0 – útmutató fejlesztőknek >></span><span class="sxs-lookup"><span data-stu-id="409ce-155">The v2.0 developer guide >></span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="409ce-156">StackOverflow "azure-active-directory" címke >></span><span class="sxs-lookup"><span data-stu-id="409ce-156">StackOverflow "azure-active-directory" tag >></span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="409ce-157">Biztonsági frissítések termékeinkhez</span><span class="sxs-lookup"><span data-stu-id="409ce-157">Get security updates for our products</span></span>
<span data-ttu-id="409ce-158">Javasoljuk, hogy kérjen értesítést a bekövetkező biztonsági incidensekről. Látogasson el [erre a lapra](https://technet.microsoft.com/security/dd252948), és fizessen elő a biztonsági tanácsadói riasztásokra.</span><span class="sxs-lookup"><span data-stu-id="409ce-158">We encourage you to get notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing to Security Advisory Alerts.</span></span>
