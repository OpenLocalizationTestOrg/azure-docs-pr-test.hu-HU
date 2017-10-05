---
title: Azure AD B2C | Microsoft Docs
description: "Hogyan lehet OAuth 2.0 hitelesítési hozzáférési jogkivonat általi védelemmel és az Azure Active Directory B2C alkalmazásával .NET-alapú webes API-t készíteni?"
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
ms.openlocfilehash: 48749bfa2ab54a0e766a4aad4f39073cc4e90818
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-b2c-build-a-net-web-api"></a><span data-ttu-id="a94b4-103">Azure Active Directory B2C: .NET webes API készítése</span><span class="sxs-lookup"><span data-stu-id="a94b4-103">Azure Active Directory B2C: Build a .NET web API</span></span>

<span data-ttu-id="a94b4-104">Az Azure Active Directory (Azure AD) B2C segítségével védetté tehet egy webes API-t OAuth 2.0 hozzáférési jogkivonatok használatával.</span><span class="sxs-lookup"><span data-stu-id="a94b4-104">With Azure Active Directory (Azure AD) B2C, you can secure a web API by using OAuth 2.0 access tokens.</span></span> <span data-ttu-id="a94b4-105">Ezek a jogkivonatok teszik lehetővé az ügyfélalkalmazások számára a hitelesítést az API felé.</span><span class="sxs-lookup"><span data-stu-id="a94b4-105">These tokens allow your client apps to authenticate to the API.</span></span> <span data-ttu-id="a94b4-106">Ebből a cikkből megtudhatja, hogyan hozhat létre egy .NET MVC „feladatlista” API-t, amellyel az ügyfélalkalmazása felhasználói CRUD-feladatokat végezhetnek el.</span><span class="sxs-lookup"><span data-stu-id="a94b4-106">This article shows you how to create a .NET MVC "to-do list" API that allows users of your client application to CRUD tasks.</span></span> <span data-ttu-id="a94b4-107">A webes API-t az Azure AD B2C látja el védelemmel, így csak a hitelesített felhasználók kezelhetik a feladatlistájukat.</span><span class="sxs-lookup"><span data-stu-id="a94b4-107">The web API is secured using Azure AD B2C and only allows authenticated users to manage their to-do list.</span></span>

## <a name="create-an-azure-ad-b2c-directory"></a><span data-ttu-id="a94b4-108">Azure AD B2C címtár létrehozása</span><span class="sxs-lookup"><span data-stu-id="a94b4-108">Create an Azure AD B2C directory</span></span>

<span data-ttu-id="a94b4-109">Ahhoz, hogy használni tudja az Azure AD B2C-t, előbb létre kell hoznia egy címtárat vagy bérlőt.</span><span class="sxs-lookup"><span data-stu-id="a94b4-109">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span> <span data-ttu-id="a94b4-110">A címtárban tárolhatja az összes felhasználót, alkalmazást, csoportot és sok minden mást.</span><span class="sxs-lookup"><span data-stu-id="a94b4-110">A directory is a container for all of your users, apps, groups, and more.</span></span> <span data-ttu-id="a94b4-111">Ha még nem tette meg, [hozzon létre most egy B2C-címtárat](active-directory-b2c-get-started.md), mielőtt továbblépne ebben az útmutatóban.</span><span class="sxs-lookup"><span data-stu-id="a94b4-111">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue in this guide.</span></span>

> [!NOTE]
> <span data-ttu-id="a94b4-112">Az ügyfélalkalmazásnak és a webes API-nak ugyanazt az Azure AD B2C könyvtárat kell használnia.</span><span class="sxs-lookup"><span data-stu-id="a94b4-112">The client application and web API must use the same Azure AD B2C directory.</span></span>
>

## <a name="create-a-web-api"></a><span data-ttu-id="a94b4-113">Webes API létrehozása</span><span class="sxs-lookup"><span data-stu-id="a94b4-113">Create a web API</span></span>

<span data-ttu-id="a94b4-114">A következő lépésben hozzon létre egy webes API-alkalmazást a B2C-címtárban.</span><span class="sxs-lookup"><span data-stu-id="a94b4-114">Next, you need to create a web API app in your B2C directory.</span></span> <span data-ttu-id="a94b4-115">Ez biztosítja az alkalmazással történő biztonságos kommunikációhoz szükséges információkat az Azure AD számára.</span><span class="sxs-lookup"><span data-stu-id="a94b4-115">This gives Azure AD information that it needs to securely communicate with your app.</span></span> <span data-ttu-id="a94b4-116">Az alkalmazást a következő [utasítások](active-directory-b2c-app-registration.md) alapján hozza létre.</span><span class="sxs-lookup"><span data-stu-id="a94b4-116">To create an app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="a94b4-117">Ügyeljen arra, hogy:</span><span class="sxs-lookup"><span data-stu-id="a94b4-117">Be sure to:</span></span>

* <span data-ttu-id="a94b4-118">Az alkalmazás tartalmazzon egy **webalkalmazást** vagy **webes API-t**.</span><span class="sxs-lookup"><span data-stu-id="a94b4-118">Include a **web app** or **web API** in the application.</span></span>
* <span data-ttu-id="a94b4-119">A webapphoz használja a `https://localhost:44332/` **URI-átirányítást**.</span><span class="sxs-lookup"><span data-stu-id="a94b4-119">Use the **Redirect URI** `https://localhost:44332/` for the web app.</span></span> <span data-ttu-id="a94b4-120">Ez a webalkalmazás ügyféloldalának alapértelmezett helye ehhez a kódmintához.</span><span class="sxs-lookup"><span data-stu-id="a94b4-120">This is the default location of the web app client for this code sample.</span></span>
* <span data-ttu-id="a94b4-121">Másolja az alkalmazáshoz rendelt **alkalmazásazonosítót**.</span><span class="sxs-lookup"><span data-stu-id="a94b4-121">Copy the **Application ID** that is assigned to your app.</span></span> <span data-ttu-id="a94b4-122">Erre később még szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="a94b4-122">You'll need it later.</span></span>
* <span data-ttu-id="a94b4-123">Adjon meg egy alkalmazásazonosítót az **Alkalmazásazonosító URI** mezőben.</span><span class="sxs-lookup"><span data-stu-id="a94b4-123">Enter an app identifier into **App ID URI**.</span></span>
* <span data-ttu-id="a94b4-124">Adjon hozzá engedélyeket a **Közzétett hatókörök** menüből.</span><span class="sxs-lookup"><span data-stu-id="a94b4-124">Add permissions through the **Published scopes** menu.</span></span>

  [!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="a94b4-125">Szabályzatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="a94b4-125">Create your policies</span></span>

<span data-ttu-id="a94b4-126">Az Azure AD B2C-ben a felhasználói élményeket [szabályzatok](active-directory-b2c-reference-policies.md) határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="a94b4-126">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="a94b4-127">Létre kell hoznia egy házirendet az Azure AD B2C-vel való kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="a94b4-127">You will need to create a policy to communicate with Azure AD B2C.</span></span> <span data-ttu-id="a94b4-128">Javasoljuk a kombinált regisztrálási/bejelentkezési házirend használatát a [házirendek áttekintésével foglalkozó cikkben](active-directory-b2c-reference-policies.md) leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="a94b4-128">We recommend using the combined sign-up/sign-in policy, as described in the [policy reference article](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="a94b4-129">A házirend létrehozásakor ügyeljen arra, hogy:</span><span class="sxs-lookup"><span data-stu-id="a94b4-129">When you create your policy, be sure to:</span></span>

* <span data-ttu-id="a94b4-130">A házirendben válassza ki a **megjelenítendő nevet** és az egyéb regisztrációs attribútumokat.</span><span class="sxs-lookup"><span data-stu-id="a94b4-130">Choose **Display name** and other sign-up attributes in your policy.</span></span>
* <span data-ttu-id="a94b4-131">Alkalmazási jogcímnek minden házirend esetén válassza ki a **megjelenítendő nevet** és az **objektumazonosítót**.</span><span class="sxs-lookup"><span data-stu-id="a94b4-131">Choose **Display name** and **Object ID** claims as application claims for every policy.</span></span> <span data-ttu-id="a94b4-132">Kiválaszthat egyéb jogcímeket is.</span><span class="sxs-lookup"><span data-stu-id="a94b4-132">You can choose other claims as well.</span></span>
* <span data-ttu-id="a94b4-133">Az egyes házirendek létrehozása után másolja a házirend **nevét**.</span><span class="sxs-lookup"><span data-stu-id="a94b4-133">Copy the **Name** of each policy after you create it.</span></span> <span data-ttu-id="a94b4-134">A szabályzat nevére később még szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="a94b4-134">You'll need the policy name later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="a94b4-135">A házirend sikeres létrehozása után készen áll az app elkészítésére.</span><span class="sxs-lookup"><span data-stu-id="a94b4-135">After you have successfully created the policy, you're ready to build your app.</span></span>

## <a name="download-the-code"></a><span data-ttu-id="a94b4-136">A kód letöltése</span><span class="sxs-lookup"><span data-stu-id="a94b4-136">Download the code</span></span>

<span data-ttu-id="a94b4-137">Az oktatóanyag kódjának kezelése a [GitHubon](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi) történik.</span><span class="sxs-lookup"><span data-stu-id="a94b4-137">The code for this tutorial is maintained on [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span></span> <span data-ttu-id="a94b4-138">A minta klónozásához futtassa a következőt:</span><span class="sxs-lookup"><span data-stu-id="a94b4-138">You can clone the sample by running:</span></span>

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

<span data-ttu-id="a94b4-139">Miután letöltötte a mintakódot, nyissa meg a Visual Studio .sln fájlt a kezdéshez.</span><span class="sxs-lookup"><span data-stu-id="a94b4-139">After you download the sample code, open the Visual Studio .sln file to get started.</span></span> <span data-ttu-id="a94b4-140">A megoldásfájl két projektet tartalmaz: `TaskWebApp` és `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="a94b4-140">The solution file contains two projects: `TaskWebApp` and `TaskService`.</span></span> <span data-ttu-id="a94b4-141">A `TaskWebApp` egy MVC-webalkalmazás, amellyel a felhasználó kommunikál.</span><span class="sxs-lookup"><span data-stu-id="a94b4-141">`TaskWebApp` is an MVC web application that the user interacts with.</span></span> <span data-ttu-id="a94b4-142">A `TaskService` az alkalmazás webes API háttérszolgáltatása, amely tárolja a felhasználók feladatlistáit.</span><span class="sxs-lookup"><span data-stu-id="a94b4-142">`TaskService` is the app's back-end web API that stores each user's to-do list.</span></span> <span data-ttu-id="a94b4-143">Ez a cikk csak a `TaskService` alkalmazást ismerteti.</span><span class="sxs-lookup"><span data-stu-id="a94b4-143">This article will only discuss the `TaskService` application.</span></span> <span data-ttu-id="a94b4-144">Ha tudni szeretné, hogyan hozhatja létre a `TaskWebApp` alkalmazást az Azure AD B2C-vel, tekintse meg a [.NET-alapú webappokról szóló oktatóanyagunkat](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span><span class="sxs-lookup"><span data-stu-id="a94b4-144">To learn how to build `TaskWebApp` using Azure AD B2C, see [our .NET web app tutorial](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span></span>

### <a name="update-the-azure-ad-b2c-configuration"></a><span data-ttu-id="a94b4-145">Az Azure AD B2C konfiguráció frissítése</span><span class="sxs-lookup"><span data-stu-id="a94b4-145">Update the Azure AD B2C configuration</span></span>

<span data-ttu-id="a94b4-146">A minta úgy van konfigurálva, hogy a bemutató bérlőnk házirendjeit és ügyfél-azonosítóját használja.</span><span class="sxs-lookup"><span data-stu-id="a94b4-146">Our sample is configured to use the policies and client ID of our demo tenant.</span></span> <span data-ttu-id="a94b4-147">Ha a saját bérlőjét szeretné használni, tegye a következőt:</span><span class="sxs-lookup"><span data-stu-id="a94b4-147">If you would like to use your own tenant, you will need to do the following:</span></span>

1. <span data-ttu-id="a94b4-148">Nyissa meg a `web.config` fájlt a `TaskService` projektben, és a következőképpen cserélje le az értékeket:</span><span class="sxs-lookup"><span data-stu-id="a94b4-148">Open `web.config` in the `TaskService` project and replace the values for</span></span>
    * <span data-ttu-id="a94b4-149">Az `ida:Tenant` helyett szerepeljen a bérlő neve</span><span class="sxs-lookup"><span data-stu-id="a94b4-149">`ida:Tenant` with your tenant name</span></span>
    * <span data-ttu-id="a94b4-150">Az `ida:ClientId` helyett szerepeljen a webes API-alkalmazás azonosítója</span><span class="sxs-lookup"><span data-stu-id="a94b4-150">`ida:ClientId` with your web API application ID</span></span>
    * <span data-ttu-id="a94b4-151">Az `ida:SignUpSignInPolicyId` helyett szerepeljen a „regisztrálási vagy bejelentkezési” házirend neve</span><span class="sxs-lookup"><span data-stu-id="a94b4-151">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>

2. <span data-ttu-id="a94b4-152">Nyissa meg a `web.config` fájlt a `TaskWebApp` projektben, és a következőképpen cserélje le az értékeket:</span><span class="sxs-lookup"><span data-stu-id="a94b4-152">Open `web.config` in the `TaskWebApp` project and replace the values for</span></span>
    * <span data-ttu-id="a94b4-153">Az `ida:Tenant` helyett szerepeljen a bérlő neve</span><span class="sxs-lookup"><span data-stu-id="a94b4-153">`ida:Tenant` with your tenant name</span></span>
    * <span data-ttu-id="a94b4-154">Az `ida:ClientId` helyett szerepeljen a webapp alkalmazásazonosítója</span><span class="sxs-lookup"><span data-stu-id="a94b4-154">`ida:ClientId` with your web app application ID</span></span>
    * <span data-ttu-id="a94b4-155">Az `ida:ClientSecret` helyett szerepeljen a webapp titkos kulcsa</span><span class="sxs-lookup"><span data-stu-id="a94b4-155">`ida:ClientSecret` with your web app secret key</span></span>
    * <span data-ttu-id="a94b4-156">Az `ida:SignUpSignInPolicyId` helyett szerepeljen a „regisztrálási vagy bejelentkezési” házirend neve</span><span class="sxs-lookup"><span data-stu-id="a94b4-156">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>
    * <span data-ttu-id="a94b4-157">Az `ida:EditProfilePolicyId` helyett szerepeljen a „profil szerkesztése” házirend neve</span><span class="sxs-lookup"><span data-stu-id="a94b4-157">`ida:EditProfilePolicyId` with your "Edit Profile" policy name</span></span>
    * <span data-ttu-id="a94b4-158">Az `ida:ResetPasswordPolicyId` helyett szerepeljen a „Jelszó alaphelyzetbe állítása” házirend neve</span><span class="sxs-lookup"><span data-stu-id="a94b4-158">`ida:ResetPasswordPolicyId` with your "Reset Password" policy name</span></span>


## <a name="secure-the-api"></a><span data-ttu-id="a94b4-159">Az API védelme</span><span class="sxs-lookup"><span data-stu-id="a94b4-159">Secure the API</span></span>

<span data-ttu-id="a94b4-160">Ha egy ügyfél hívja az API-t, biztonságossá teheti az API-t (pl. `TaskService`) az OAuth 2.0 tulajdonosi jogkivonatok használatával.</span><span class="sxs-lookup"><span data-stu-id="a94b4-160">When you have a client that calls your API, you can secure your API (e.g `TaskService`) by using OAuth 2.0 bearer tokens.</span></span> <span data-ttu-id="a94b4-161">Ez biztosítja, hogy az API felé irányuló kérések csak akkor legyenek érvényesek, ha a kérés rendelkezik tulajdonosi jogkivonattal.</span><span class="sxs-lookup"><span data-stu-id="a94b4-161">This ensures that each request to your API will only be valid if the request has a bearer token.</span></span> <span data-ttu-id="a94b4-162">Az API a Microsoft Open Web Interface .NET (OWIN) könyvtár használatával tudja elfogadni és érvényesíteni a tulajdonosi jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="a94b4-162">Your API can accept and validate bearer tokens by using Microsoft's Open Web Interface for .NET (OWIN) library.</span></span>

### <a name="install-owin"></a><span data-ttu-id="a94b4-163">Az OWIN telepítése</span><span class="sxs-lookup"><span data-stu-id="a94b4-163">Install OWIN</span></span>

<span data-ttu-id="a94b4-164">Első lépésként telepítse az OWIN OAuth hitelesítési folyamatot a Visual Studio csomagkezelő konzoljával.</span><span class="sxs-lookup"><span data-stu-id="a94b4-164">Begin by installing the OWIN OAuth authentication pipeline by using the Visual Studio Package Manager Console.</span></span>

```Console
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

<span data-ttu-id="a94b4-165">Ez telepíti az OWIN közbenső szoftvert, amely fogadja és érvényesíti a tulajdonosi jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="a94b4-165">This will install the OWIN middleware that will accept and validate bearer tokens.</span></span>

### <a name="add-an-owin-startup-class"></a><span data-ttu-id="a94b4-166">OWIN indítási osztály hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a94b4-166">Add an OWIN startup class</span></span>

<span data-ttu-id="a94b4-167">Adjon hozzá egy OWIN indítási osztályt a `Startup.cs` nevű API-hoz.</span><span class="sxs-lookup"><span data-stu-id="a94b4-167">Add an OWIN startup class to the API called `Startup.cs`.</span></span>  <span data-ttu-id="a94b4-168">Kattintson a jobb gombbal a projektre, válassza az **Add** (Hozzáadás), majd a **New Item** (Új elem) lehetőséget, és keresse meg az OWIN elemet.</span><span class="sxs-lookup"><span data-stu-id="a94b4-168">Right-click on the project, select **Add** and **New Item**, and then search for OWIN.</span></span> <span data-ttu-id="a94b4-169">Az OWIN közbenső szoftver meghívja a `Configuration(…)` metódust az alkalmazás indulásakor.</span><span class="sxs-lookup"><span data-stu-id="a94b4-169">The OWIN middleware will invoke the `Configuration(…)` method when your app starts.</span></span>

<span data-ttu-id="a94b4-170">A mintánkban módosítottuk az osztálydeklarációt `public partial class Startup` értékre, és implementáltuk az `App_Start\Startup.Auth.cs` fájlban lévő osztály másik részét.</span><span class="sxs-lookup"><span data-stu-id="a94b4-170">In our sample, we changed the class declaration to `public partial class Startup` and implemented the other part of the class in `App_Start\Startup.Auth.cs`.</span></span> <span data-ttu-id="a94b4-171">A `Configuration` metóduson belül hozzáadtunk egy hívást a `ConfigureAuth` metódusra, amely a `Startup.Auth.cs` fájlban van definiálva.</span><span class="sxs-lookup"><span data-stu-id="a94b4-171">Inside the `Configuration` method, we added a call to `ConfigureAuth`, which is defined in `Startup.Auth.cs`.</span></span> <span data-ttu-id="a94b4-172">A módosítás után a `Startup.cs` a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="a94b4-172">After the modifications, `Startup.cs` looks like the following:</span></span>

```CSharp
// Startup.cs

public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        // ConfigureAuth defined in other part of the class
        ConfigureAuth(app);
    }
}
```

### <a name="configure-oauth-20-authentication"></a><span data-ttu-id="a94b4-173">OAuth 2.0 típusú hitelesítés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a94b4-173">Configure OAuth 2.0 authentication</span></span>

<span data-ttu-id="a94b4-174">Nyissa meg a(z) `App_Start\Startup.Auth.cs` fájlt, és hajtsa végre a(z) `ConfigureAuth(...)` módszert.</span><span class="sxs-lookup"><span data-stu-id="a94b4-174">Open the file `App_Start\Startup.Auth.cs`, and implement the `ConfigureAuth(...)` method.</span></span> <span data-ttu-id="a94b4-175">A következő módon nézhet ki például:</span><span class="sxs-lookup"><span data-stu-id="a94b4-175">For example, it could look like the following:</span></span>

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
         * Configure the authorization OWIN middleware.
         */
        public void ConfigureAuth(IAppBuilder app)
        {
            TokenValidationParameters tvps = new TokenValidationParameters
            {
                // Accept only those tokens where the audience of the token is equal to the client ID of this app
                ValidAudience = ClientId,
                AuthenticationType = Startup.DefaultPolicy
            };

            app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
            {
                // This SecurityTokenProvider fetches the Azure AD B2C metadata & signing keys from the OpenIDConnect metadata endpoint
                AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(AadInstance, Tenant, DefaultPolicy)))
            });
        }
    }
```

### <a name="secure-the-task-controller"></a><span data-ttu-id="a94b4-176">Feladatvezérlő védelme</span><span class="sxs-lookup"><span data-stu-id="a94b4-176">Secure the task controller</span></span>

<span data-ttu-id="a94b4-177">Az alkalmazás OAuth 2.0 típusú hitelesítés használatára történő konfigurálása után, a webes API-t biztonságossá teheti úgy, hogy egy `[Authorize]` címkét ad hozzá a feladatvezérlőhöz.</span><span class="sxs-lookup"><span data-stu-id="a94b4-177">After the app is configured to use OAuth 2.0 authentication, you can secure your web API by adding an `[Authorize]` tag to the task controller.</span></span> <span data-ttu-id="a94b4-178">A feladatvezérlőben történik a feladatlisták kezelése, így osztályszinten kell biztonságossá tenni az egész vezérlőt.</span><span class="sxs-lookup"><span data-stu-id="a94b4-178">This is the controller where all to-do list manipulation takes place, so you should secure the entire controller at the class level.</span></span> <span data-ttu-id="a94b4-179">Az egyes műveletekre vonatkozóan is hozzáadhatja a(z) `[Authorize]` címkét a részletesebb vezérlés érdekében.</span><span class="sxs-lookup"><span data-stu-id="a94b4-179">You can also add the `[Authorize]` tag to individual actions for more fine-grained control.</span></span>

```CSharp
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-the-token"></a><span data-ttu-id="a94b4-180">Felhasználói adatok beolvasása a jogkivonatból</span><span class="sxs-lookup"><span data-stu-id="a94b4-180">Get user information from the token</span></span>

<span data-ttu-id="a94b4-181">A `TasksController` egy olyan adatbázisban tárolja a feladatokat, ahol minden feladathoz hozzá van rendelve egy felhasználó, aki a feladat „tulajdonosa”.</span><span class="sxs-lookup"><span data-stu-id="a94b4-181">`TasksController` stores tasks in a database where each task has an associated user who "owns" the task.</span></span> <span data-ttu-id="a94b4-182">A tulajdonos azonosítása a felhasználói **objektumazonosítóval** történik.</span><span class="sxs-lookup"><span data-stu-id="a94b4-182">The owner is identified by the user's **object ID**.</span></span> <span data-ttu-id="a94b4-183">(Ezért kellett az alkalmazás jogcímeként objektumazonosítót hozzáadnia minden házirendben.)</span><span class="sxs-lookup"><span data-stu-id="a94b4-183">(This is why you needed to add the object ID as an application claim in all of your policies.)</span></span>

```CSharp
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

### <a name="validate-the-permissions-in-the-token"></a><span data-ttu-id="a94b4-184">A jogkivonat engedélyeinek érvényesítése</span><span class="sxs-lookup"><span data-stu-id="a94b4-184">Validate the permissions in the token</span></span>

<span data-ttu-id="a94b4-185">Általános követelmény a webes API-khoz a jogkivonatban jelen lévő „hatókörök” érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="a94b4-185">A common requirement for web APIs is to validate the "scopes" present in the token.</span></span> <span data-ttu-id="a94b4-186">Ez biztosítja, hogy a felhasználó hozzájárult a feladatlista szolgáltatáshoz való hozzáféréshez szükséges engedélyekhez.</span><span class="sxs-lookup"><span data-stu-id="a94b4-186">This ensures that the user has consented to the permissions required to access the to-do list service.</span></span>

```CSharp
public IEnumerable<Models.Task> Get()
{
    if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "read")
    {
        throw new HttpResponseException(new HttpResponseMessage {
            StatusCode = HttpStatusCode.Unauthorized,
            ReasonPhrase = "The Scope claim does not contain 'read' or scope claim not found"
        });
    }
    ...
}
```

## <a name="run-the-sample-app"></a><span data-ttu-id="a94b4-187">Mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="a94b4-187">Run the sample app</span></span>

<span data-ttu-id="a94b4-188">Végezetül készítse el és futtassa a következőket: `TaskWebApp` és `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="a94b4-188">Finally, build and run both `TaskWebApp` and `TaskService`.</span></span> <span data-ttu-id="a94b4-189">Hozzon létre feladatokat a felhasználói feladatlistában, és figyelje meg, hogy azok megmaradnak a API-ban azután is, ha leállította, majd újraindította az adott ügyfelet.</span><span class="sxs-lookup"><span data-stu-id="a94b4-189">Create some tasks on the user's to-do list and notice how they are persisted in the API even after you stop and restart the client.</span></span>

## <a name="edit-your-policies"></a><span data-ttu-id="a94b4-190">Házirendek szerkesztése</span><span class="sxs-lookup"><span data-stu-id="a94b4-190">Edit your policies</span></span>

<span data-ttu-id="a94b4-191">Miután az Azure AD B2C használatával biztonságossá tett egy API-t, kísérletezhet a bejelentkezési/regisztrálási szabályzattal, és megtekintheti ennek az API-ra gyakorolt hatásait (vagy a hatás hiányát).</span><span class="sxs-lookup"><span data-stu-id="a94b4-191">After you have secured an API by using Azure AD B2C, you can experiment with your Sign-in/Sign-up policy and view the effects (or lack thereof) on the API.</span></span> <span data-ttu-id="a94b4-192">Módosíthatja a házirendekben az alkalmazás jogcímét és a webes API-n hozzáférhető felhasználói adatokat.</span><span class="sxs-lookup"><span data-stu-id="a94b4-192">You can manipulate the application claims in the policies and change the user information that is available in the web API.</span></span> <span data-ttu-id="a94b4-193">Minden hozzáadott jogcím elérhető lesz a .NET MVC webes API számára a(z) `ClaimsPrincipal` objektumban az ebben a cikkben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="a94b4-193">Any claims that you add will be available to your .NET MVC web API in the `ClaimsPrincipal` object, as described earlier in this article.</span></span>
