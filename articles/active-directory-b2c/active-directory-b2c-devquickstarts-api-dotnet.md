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
# <a name="azure-active-directory-b2c-build-a-net-web-api"></a><span data-ttu-id="9a2e7-103">Azure Active Directory B2C: .NET webes API készítése</span><span class="sxs-lookup"><span data-stu-id="9a2e7-103">Azure Active Directory B2C: Build a .NET web API</span></span>

<span data-ttu-id="9a2e7-104">Az Azure Active Directory (Azure AD) B2C segítségével védetté tehet egy webes API-t OAuth 2.0 hozzáférési jogkivonatok használatával.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-104">With Azure Active Directory (Azure AD) B2C, you can secure a web API by using OAuth 2.0 access tokens.</span></span> <span data-ttu-id="9a2e7-105">Ezek a jogkivonatok engedélyezik az ügyfél alkalmazások tooauthenticate toohello API.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-105">These tokens allow your client apps tooauthenticate toohello API.</span></span> <span data-ttu-id="9a2e7-106">Ez a cikk bemutatja, hogyan toocreate egy .NET MVC "Feladatlista" API-t, amely lehetővé teszi a felhasználók az ügyfél alkalmazás tooCRUD feladatok.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-106">This article shows you how toocreate a .NET MVC "to-do list" API that allows users of your client application tooCRUD tasks.</span></span> <span data-ttu-id="9a2e7-107">hello webes API-t az Azure AD B2C használatával lett biztonságossá téve, és csak lehetővé teszi a hitelesített felhasználók toomanage a tennivalók listájára.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-107">hello web API is secured using Azure AD B2C and only allows authenticated users toomanage their to-do list.</span></span>

## <a name="create-an-azure-ad-b2c-directory"></a><span data-ttu-id="9a2e7-108">Azure AD B2C címtár létrehozása</span><span class="sxs-lookup"><span data-stu-id="9a2e7-108">Create an Azure AD B2C directory</span></span>

<span data-ttu-id="9a2e7-109">Ahhoz, hogy használni tudja az Azure AD B2C-t, előbb létre kell hoznia egy címtárat vagy bérlőt.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-109">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span> <span data-ttu-id="9a2e7-110">A címtárban tárolhatja az összes felhasználót, alkalmazást, csoportot és sok minden mást.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-110">A directory is a container for all of your users, apps, groups, and more.</span></span> <span data-ttu-id="9a2e7-111">Ha még nem tette meg, [hozzon létre most egy B2C-címtárat](active-directory-b2c-get-started.md), mielőtt továbblépne ebben az útmutatóban.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-111">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue in this guide.</span></span>

> [!NOTE]
> <span data-ttu-id="9a2e7-112">hello ügyfélalkalmazást és a webes API hello ugyanazt az Azure AD B2C-címtárban kell használnia.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-112">hello client application and web API must use hello same Azure AD B2C directory.</span></span>
>

## <a name="create-a-web-api"></a><span data-ttu-id="9a2e7-113">Webes API létrehozása</span><span class="sxs-lookup"><span data-stu-id="9a2e7-113">Create a web API</span></span>

<span data-ttu-id="9a2e7-114">A következő lépésben toocreate egy webes API-alkalmazást a B2C-címtárban.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-114">Next, you need toocreate a web API app in your B2C directory.</span></span> <span data-ttu-id="9a2e7-115">Ez biztosítja, hogy az igények toosecurely kommunikálni az alkalmazást az Azure AD-információkat.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-115">This gives Azure AD information that it needs toosecurely communicate with your app.</span></span> <span data-ttu-id="9a2e7-116">hajtsa végre az alkalmazást, és egy toocreate [ezeket az utasításokat](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="9a2e7-116">toocreate an app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="9a2e7-117">Ügyeljen arra, hogy:</span><span class="sxs-lookup"><span data-stu-id="9a2e7-117">Be sure to:</span></span>

* <span data-ttu-id="9a2e7-118">Tartalmaznak egy **webalkalmazás** vagy **webes API-t** hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-118">Include a **web app** or **web API** in hello application.</span></span>
* <span data-ttu-id="9a2e7-119">Használjon hello **átirányítási URI-** `https://localhost:44332/` hello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-119">Use hello **Redirect URI** `https://localhost:44332/` for hello web app.</span></span> <span data-ttu-id="9a2e7-120">Ez az hello kódmintában hello webalkalmazás ügyféloldalának alapértelmezett helye.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-120">This is hello default location of hello web app client for this code sample.</span></span>
* <span data-ttu-id="9a2e7-121">Másolás hello **Alkalmazásazonosító** , amely hozzárendelt tooyour alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-121">Copy hello **Application ID** that is assigned tooyour app.</span></span> <span data-ttu-id="9a2e7-122">Erre később még szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-122">You'll need it later.</span></span>
* <span data-ttu-id="9a2e7-123">Adjon meg egy alkalmazásazonosítót az **Alkalmazásazonosító URI** mezőben.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-123">Enter an app identifier into **App ID URI**.</span></span>
* <span data-ttu-id="9a2e7-124">Adja hozzá az engedélyek a hello **hatókörök közzétett** menü.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-124">Add permissions through hello **Published scopes** menu.</span></span>

  [!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="9a2e7-125">Szabályzatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="9a2e7-125">Create your policies</span></span>

<span data-ttu-id="9a2e7-126">Az Azure AD B2C-ben a felhasználói élményeket [szabályzatok](active-directory-b2c-reference-policies.md) határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-126">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="9a2e7-127">Szüksége lesz egy Azure AD B2C házirend toocommunicate toocreate.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-127">You will need toocreate a policy toocommunicate with Azure AD B2C.</span></span> <span data-ttu-id="9a2e7-128">Hello segítségével kombinált sign-Close-Up/bejelentkezési házirend, javasoljuk, az a hello [házirendek áttekintésével foglalkozó cikkben](active-directory-b2c-reference-policies.md).</span><span class="sxs-lookup"><span data-stu-id="9a2e7-128">We recommend using hello combined sign-up/sign-in policy, as described in hello [policy reference article](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="9a2e7-129">A házirend létrehozásakor ügyeljen arra, hogy:</span><span class="sxs-lookup"><span data-stu-id="9a2e7-129">When you create your policy, be sure to:</span></span>

* <span data-ttu-id="9a2e7-130">A házirendben válassza ki a **megjelenítendő nevet** és az egyéb regisztrációs attribútumokat.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-130">Choose **Display name** and other sign-up attributes in your policy.</span></span>
* <span data-ttu-id="9a2e7-131">Alkalmazási jogcímnek minden házirend esetén válassza ki a **megjelenítendő nevet** és az **objektumazonosítót**.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-131">Choose **Display name** and **Object ID** claims as application claims for every policy.</span></span> <span data-ttu-id="9a2e7-132">Kiválaszthat egyéb jogcímeket is.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-132">You can choose other claims as well.</span></span>
* <span data-ttu-id="9a2e7-133">Másolás hello **neve** egyes házirendek létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-133">Copy hello **Name** of each policy after you create it.</span></span> <span data-ttu-id="9a2e7-134">Hello házirendnév később lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-134">You'll need hello policy name later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="9a2e7-135">Miután sikeresen létrehozta a hello házirend, készen áll a toobuild Ön az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-135">After you have successfully created hello policy, you're ready toobuild your app.</span></span>

## <a name="download-hello-code"></a><span data-ttu-id="9a2e7-136">Hello kód letöltése</span><span class="sxs-lookup"><span data-stu-id="9a2e7-136">Download hello code</span></span>

<span data-ttu-id="9a2e7-137">az oktatóanyag kódjának hello fenntartott [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span><span class="sxs-lookup"><span data-stu-id="9a2e7-137">hello code for this tutorial is maintained on [GitHub](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi).</span></span> <span data-ttu-id="9a2e7-138">Hello minta klónozhat futtatásával:</span><span class="sxs-lookup"><span data-stu-id="9a2e7-138">You can clone hello sample by running:</span></span>

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

<span data-ttu-id="9a2e7-139">Hello mintakód letöltése után nyissa meg hello Visual Studio .sln fájlt tooget elindult.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-139">After you download hello sample code, open hello Visual Studio .sln file tooget started.</span></span> <span data-ttu-id="9a2e7-140">hello megoldásfájl két projektet tartalmaz: `TaskWebApp` és `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-140">hello solution file contains two projects: `TaskWebApp` and `TaskService`.</span></span> <span data-ttu-id="9a2e7-141">`TaskWebApp`az, hogy a felhasználó hello MVC webalkalmazás kommunikál.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-141">`TaskWebApp` is an MVC web application that hello user interacts with.</span></span> <span data-ttu-id="9a2e7-142">`TaskService`minden felhasználói feladatlistát tároló hello app webes API van.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-142">`TaskService` is hello app's back-end web API that stores each user's to-do list.</span></span> <span data-ttu-id="9a2e7-143">Ez a cikk csak ismertetik hello `TaskService` alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-143">This article will only discuss hello `TaskService` application.</span></span> <span data-ttu-id="9a2e7-144">toolearn hogyan toobuild `TaskWebApp` Azure AD B2C segítségével, lásd: [a .NET web app oktatóanyag](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span><span class="sxs-lookup"><span data-stu-id="9a2e7-144">toolearn how toobuild `TaskWebApp` using Azure AD B2C, see [our .NET web app tutorial](active-directory-b2c-devquickstarts-web-dotnet-susi.md).</span></span>

### <a name="update-hello-azure-ad-b2c-configuration"></a><span data-ttu-id="9a2e7-145">Az Azure AD B2C hello konfiguráció frissítése</span><span class="sxs-lookup"><span data-stu-id="9a2e7-145">Update hello Azure AD B2C configuration</span></span>

<span data-ttu-id="9a2e7-146">A minta rendszer konfigurált toouse hello házirendek és az ügyfél-Azonosítóját a bemutató bérlő.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-146">Our sample is configured toouse hello policies and client ID of our demo tenant.</span></span> <span data-ttu-id="9a2e7-147">Ha azt szeretné, hogy toouse saját bérlőt, szüksége lesz a következő toodo hello:</span><span class="sxs-lookup"><span data-stu-id="9a2e7-147">If you would like toouse your own tenant, you will need toodo hello following:</span></span>

1. <span data-ttu-id="9a2e7-148">Nyissa meg `web.config` a hello `TaskService` projektre, és cserélje le a hello értékei</span><span class="sxs-lookup"><span data-stu-id="9a2e7-148">Open `web.config` in hello `TaskService` project and replace hello values for</span></span>
    * <span data-ttu-id="9a2e7-149">Az `ida:Tenant` helyett szerepeljen a bérlő neve</span><span class="sxs-lookup"><span data-stu-id="9a2e7-149">`ida:Tenant` with your tenant name</span></span>
    * <span data-ttu-id="9a2e7-150">Az `ida:ClientId` helyett szerepeljen a webes API-alkalmazás azonosítója</span><span class="sxs-lookup"><span data-stu-id="9a2e7-150">`ida:ClientId` with your web API application ID</span></span>
    * <span data-ttu-id="9a2e7-151">Az `ida:SignUpSignInPolicyId` helyett szerepeljen a „regisztrálási vagy bejelentkezési” házirend neve</span><span class="sxs-lookup"><span data-stu-id="9a2e7-151">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>

2. <span data-ttu-id="9a2e7-152">Nyissa meg `web.config` a hello `TaskWebApp` projektre, és cserélje le a hello értékei</span><span class="sxs-lookup"><span data-stu-id="9a2e7-152">Open `web.config` in hello `TaskWebApp` project and replace hello values for</span></span>
    * <span data-ttu-id="9a2e7-153">Az `ida:Tenant` helyett szerepeljen a bérlő neve</span><span class="sxs-lookup"><span data-stu-id="9a2e7-153">`ida:Tenant` with your tenant name</span></span>
    * <span data-ttu-id="9a2e7-154">Az `ida:ClientId` helyett szerepeljen a webapp alkalmazásazonosítója</span><span class="sxs-lookup"><span data-stu-id="9a2e7-154">`ida:ClientId` with your web app application ID</span></span>
    * <span data-ttu-id="9a2e7-155">Az `ida:ClientSecret` helyett szerepeljen a webapp titkos kulcsa</span><span class="sxs-lookup"><span data-stu-id="9a2e7-155">`ida:ClientSecret` with your web app secret key</span></span>
    * <span data-ttu-id="9a2e7-156">Az `ida:SignUpSignInPolicyId` helyett szerepeljen a „regisztrálási vagy bejelentkezési” házirend neve</span><span class="sxs-lookup"><span data-stu-id="9a2e7-156">`ida:SignUpSignInPolicyId` with your "Sign-up or Sign-in" policy name</span></span>
    * <span data-ttu-id="9a2e7-157">Az `ida:EditProfilePolicyId` helyett szerepeljen a „profil szerkesztése” házirend neve</span><span class="sxs-lookup"><span data-stu-id="9a2e7-157">`ida:EditProfilePolicyId` with your "Edit Profile" policy name</span></span>
    * <span data-ttu-id="9a2e7-158">Az `ida:ResetPasswordPolicyId` helyett szerepeljen a „Jelszó alaphelyzetbe állítása” házirend neve</span><span class="sxs-lookup"><span data-stu-id="9a2e7-158">`ida:ResetPasswordPolicyId` with your "Reset Password" policy name</span></span>


## <a name="secure-hello-api"></a><span data-ttu-id="9a2e7-159">Biztonságos hello API</span><span class="sxs-lookup"><span data-stu-id="9a2e7-159">Secure hello API</span></span>

<span data-ttu-id="9a2e7-160">Ha egy ügyfél hívja az API-t, biztonságossá teheti az API-t (pl. `TaskService`) az OAuth 2.0 tulajdonosi jogkivonatok használatával.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-160">When you have a client that calls your API, you can secure your API (e.g `TaskService`) by using OAuth 2.0 bearer tokens.</span></span> <span data-ttu-id="9a2e7-161">Ez biztosítja, hogy minden egyes kérelem tooyour API csak akkor érvényes, ha hello kérelem rendelkezik egy tulajdonosi jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-161">This ensures that each request tooyour API will only be valid if hello request has a bearer token.</span></span> <span data-ttu-id="9a2e7-162">Az API a Microsoft Open Web Interface .NET (OWIN) könyvtár használatával tudja elfogadni és érvényesíteni a tulajdonosi jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-162">Your API can accept and validate bearer tokens by using Microsoft's Open Web Interface for .NET (OWIN) library.</span></span>

### <a name="install-owin"></a><span data-ttu-id="9a2e7-163">Az OWIN telepítése</span><span class="sxs-lookup"><span data-stu-id="9a2e7-163">Install OWIN</span></span>

<span data-ttu-id="9a2e7-164">Első lépésként telepítse a Visual Studio Csomagkezelő konzol hello hello OWIN OAuth hitelesítési folyamatot.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-164">Begin by installing hello OWIN OAuth authentication pipeline by using hello Visual Studio Package Manager Console.</span></span>

```Console
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

<span data-ttu-id="9a2e7-165">Ez a hello OWIN alkalmazott szoftver, amely elfogadja és érvényesíthet jogkivonatokat tulajdonosi telepíti.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-165">This will install hello OWIN middleware that will accept and validate bearer tokens.</span></span>

### <a name="add-an-owin-startup-class"></a><span data-ttu-id="9a2e7-166">OWIN indítási osztály hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9a2e7-166">Add an OWIN startup class</span></span>

<span data-ttu-id="9a2e7-167">Adja hozzá egy OWIN indítási osztály toohello API hívása `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-167">Add an OWIN startup class toohello API called `Startup.cs`.</span></span>  <span data-ttu-id="9a2e7-168">Kattintson a jobb gombbal hello projektre, válassza a **Hozzáadás** és **új elem**, majd keresse meg az owin ELEMET.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-168">Right-click on hello project, select **Add** and **New Item**, and then search for OWIN.</span></span> <span data-ttu-id="9a2e7-169">hello OWIN köztes által aktivált hello `Configuration(…)` módszer az alkalmazás indításakor.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-169">hello OWIN middleware will invoke hello `Configuration(…)` method when your app starts.</span></span>

<span data-ttu-id="9a2e7-170">A mintában szereplő változtattuk hello osztálydeklaráció túl`public partial class Startup` és megvalósított hello osztály a többi részét hello `App_Start\Startup.Auth.cs`.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-170">In our sample, we changed hello class declaration too`public partial class Startup` and implemented hello other part of hello class in `App_Start\Startup.Auth.cs`.</span></span> <span data-ttu-id="9a2e7-171">Belső hello `Configuration` módszer, hívása túl hozzáadott`ConfigureAuth`, amelyhez definiálva van `Startup.Auth.cs`.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-171">Inside hello `Configuration` method, we added a call too`ConfigureAuth`, which is defined in `Startup.Auth.cs`.</span></span> <span data-ttu-id="9a2e7-172">Hello módosítások után `Startup.cs` tűnik hello következő:</span><span class="sxs-lookup"><span data-stu-id="9a2e7-172">After hello modifications, `Startup.cs` looks like hello following:</span></span>

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

### <a name="configure-oauth-20-authentication"></a><span data-ttu-id="9a2e7-173">OAuth 2.0 típusú hitelesítés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9a2e7-173">Configure OAuth 2.0 authentication</span></span>

<span data-ttu-id="9a2e7-174">Nyissa meg hello fájl `App_Start\Startup.Auth.cs`, és valósíthatnak meg hello `ConfigureAuth(...)` metódust.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-174">Open hello file `App_Start\Startup.Auth.cs`, and implement hello `ConfigureAuth(...)` method.</span></span> <span data-ttu-id="9a2e7-175">Például hogy nézhet hello következő:</span><span class="sxs-lookup"><span data-stu-id="9a2e7-175">For example, it could look like hello following:</span></span>

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

### <a name="secure-hello-task-controller"></a><span data-ttu-id="9a2e7-176">Biztonságos hello feladatvezérlőhöz</span><span class="sxs-lookup"><span data-stu-id="9a2e7-176">Secure hello task controller</span></span>

<span data-ttu-id="9a2e7-177">Miután hello alkalmazás konfigurált toouse OAuth 2.0 típusú hitelesítés, a webes API biztosíthassa hozzáadásával egy `[Authorize]` címke toohello feladatvezérlőhöz.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-177">After hello app is configured toouse OAuth 2.0 authentication, you can secure your web API by adding an `[Authorize]` tag toohello task controller.</span></span> <span data-ttu-id="9a2e7-178">Ez azért, ahol minden tennivalók listája adatkezelési történik, így osztályszinten kell biztonságossá tenni hello teljes vezérlőre hello osztály szinten hello vezérlő.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-178">This is hello controller where all to-do list manipulation takes place, so you should secure hello entire controller at hello class level.</span></span> <span data-ttu-id="9a2e7-179">Azt is megteheti hello `[Authorize]` címkét a részletesebb vezérlés tooindividual műveletek.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-179">You can also add hello `[Authorize]` tag tooindividual actions for more fine-grained control.</span></span>

```CSharp
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-hello-token"></a><span data-ttu-id="9a2e7-180">Felhasználói adatok beolvasása a hello token</span><span class="sxs-lookup"><span data-stu-id="9a2e7-180">Get user information from hello token</span></span>

<span data-ttu-id="9a2e7-181">`TasksController`ahol minden feladathoz hozzá van rendelve egy felhasználó "birtokló" hello feladat-adatbázisban tárolja a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-181">`TasksController` stores tasks in a database where each task has an associated user who "owns" hello task.</span></span> <span data-ttu-id="9a2e7-182">hello tulajdonos azonosítása hello felhasználói **objektumazonosító**.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-182">hello owner is identified by hello user's **object ID**.</span></span> <span data-ttu-id="9a2e7-183">(Tooadd hello Objektumazonosító alkalmazásként szükséges, ezért a házirendek összes jogcímet.)</span><span class="sxs-lookup"><span data-stu-id="9a2e7-183">(This is why you needed tooadd hello object ID as an application claim in all of your policies.)</span></span>

```CSharp
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

### <a name="validate-hello-permissions-in-hello-token"></a><span data-ttu-id="9a2e7-184">Hello jogkivonat hello engedélyek ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="9a2e7-184">Validate hello permissions in hello token</span></span>

<span data-ttu-id="9a2e7-185">Kötelező megadni a webes API-k toovalidate hello "hatókör" hello lexikális elem szerepel.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-185">A common requirement for web APIs is toovalidate hello "scopes" present in hello token.</span></span> <span data-ttu-id="9a2e7-186">Ez biztosítja, hogy hello a felhasználó hozzájárult toohello engedélyek szükséges tooaccess hello tennivalók listája szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-186">This ensures that hello user has consented toohello permissions required tooaccess hello to-do list service.</span></span>

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

## <a name="run-hello-sample-app"></a><span data-ttu-id="9a2e7-187">Hello mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="9a2e7-187">Run hello sample app</span></span>

<span data-ttu-id="9a2e7-188">Végezetül készítse el és futtassa a következőket: `TaskWebApp` és `TaskService`.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-188">Finally, build and run both `TaskWebApp` and `TaskService`.</span></span> <span data-ttu-id="9a2e7-189">Hozzon létre feladatokat hello felhasználói feladatlistában, és figyelje meg, hogy azok megmaradnak a hello API hello ügyfél újraindítása után is.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-189">Create some tasks on hello user's to-do list and notice how they are persisted in hello API even after you stop and restart hello client.</span></span>

## <a name="edit-your-policies"></a><span data-ttu-id="9a2e7-190">Házirendek szerkesztése</span><span class="sxs-lookup"><span data-stu-id="9a2e7-190">Edit your policies</span></span>

<span data-ttu-id="9a2e7-191">Miután az Azure AD B2C használatával biztonságossá tett egy API-t, kísérletezhet a bejelentkezési-a/regisztrációs házirend és nézet hello hatások (vagy hiányára) hello API.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-191">After you have secured an API by using Azure AD B2C, you can experiment with your Sign-in/Sign-up policy and view hello effects (or lack thereof) on hello API.</span></span> <span data-ttu-id="9a2e7-192">Hello alkalmazási jogcímet hello házirendek kezelése, és módosíthatja hello felhasználó adatait, amely elérhető a hello webes API-t.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-192">You can manipulate hello application claims in hello policies and change hello user information that is available in hello web API.</span></span> <span data-ttu-id="9a2e7-193">Minden hozzáadott jogcím elérhető tooyour .NET MVC webes API hello lesz `ClaimsPrincipal` objektum, a cikkben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="9a2e7-193">Any claims that you add will be available tooyour .NET MVC web API in hello `ClaimsPrincipal` object, as described earlier in this article.</span></span>
