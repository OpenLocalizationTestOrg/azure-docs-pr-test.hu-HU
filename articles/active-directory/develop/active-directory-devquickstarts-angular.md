---
title: "Első lépések AD AngularJS aaaAzure |} Microsoft Docs"
description: "Hogyan toobuild AngularJS egyoldalas alkalmazás, amely integrálható az Azure ad-val bejelentkezési és az Azure AD-védett API OAuth használatával."
services: active-directory
documentationcenter: 
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: f2991054-8146-4718-a5f7-59b892230ad7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: eca5e1c9662186dfae4f96ca3041f9350583cf79
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="help-secure-angularjs-single-page-apps-by-using-azure-ad"></a><span data-ttu-id="ebb06-103">Számítógépek biztonságossá tétele AngularJS egyoldalas alkalmazások az Azure AD segítségével</span><span class="sxs-lookup"><span data-stu-id="ebb06-103">Help secure AngularJS single-page apps by using Azure AD</span></span>

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="ebb06-104">Azure Active Directory (Azure AD) az egyszerű és magától értetődő, a tooadd bejelentkezés, kijelentkezési, és az OAuth biztonságos API tooyour egyoldalas alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="ebb06-104">Azure Active Directory (Azure AD) makes it simple and straightforward for you tooadd sign-in, sign-out, and secure OAuth API calls tooyour single-page apps.</span></span>  <span data-ttu-id="ebb06-105">Lehetővé teszi, hogy a alkalmazások tooauthenticate felhasználókat a Windows Server Active Directory-fiókba, és minden webes API-t, amely az Azure AD segít megvédeni, például az Office 365 API-k hello vagy hello Azure API felhasználását.</span><span class="sxs-lookup"><span data-stu-id="ebb06-105">It enables your apps tooauthenticate users with their Windows Server Active Directory accounts and consume any web API that Azure AD helps protect, such as hello Office 365 APIs or hello Azure API.</span></span>

<span data-ttu-id="ebb06-106">Böngészőjében futó JavaScript-alkalmazások esetében az Azure AD hello biztosít az Active Directory Authentication Library (ADAL), vagy adal.js.</span><span class="sxs-lookup"><span data-stu-id="ebb06-106">For JavaScript applications running in a browser, Azure AD provides hello Active Directory Authentication Library (ADAL), or adal.js.</span></span> <span data-ttu-id="ebb06-107">hello kizárólagos adal.js célja toomake megkönnyítik az alkalmazás tooget hozzáférési jogkivonatok.</span><span class="sxs-lookup"><span data-stu-id="ebb06-107">hello sole purpose of adal.js is toomake it easy for your app tooget access tokens.</span></span> <span data-ttu-id="ebb06-108">toodemonstrate milyen könnyű van, itt azt fogja build egy AngularJS tooDo lista alkalmazás, amely:</span><span class="sxs-lookup"><span data-stu-id="ebb06-108">toodemonstrate just how easy it is, here we'll build an AngularJS tooDo List application that:</span></span>

* <span data-ttu-id="ebb06-109">Jeleket hello felhasználói identitás-szolgáltatóként hello Azure AD használatával toohello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ebb06-109">Signs hello user in toohello app by using Azure AD as hello identity provider.</span></span>

* <span data-ttu-id="ebb06-110">Néhány hello felhasználói információkat jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="ebb06-110">Displays some information about hello user.</span></span>
* <span data-ttu-id="ebb06-111">Biztonságosan hívások alkalmazás tooDo lista API hello Azure ad-tulajdonosi jogkivonatok segítségével.</span><span class="sxs-lookup"><span data-stu-id="ebb06-111">Securely calls hello app's tooDo List API by using bearer tokens from Azure AD.</span></span>
* <span data-ttu-id="ebb06-112">Jeleket hello felhasználói hello alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="ebb06-112">Signs hello user out of hello app.</span></span>

<span data-ttu-id="ebb06-113">toobuild hello teljes, működő alkalmazást kell:</span><span class="sxs-lookup"><span data-stu-id="ebb06-113">toobuild hello complete, working application, you need to:</span></span>

1. <span data-ttu-id="ebb06-114">Regisztrálja az alkalmazást az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="ebb06-114">Register your app with Azure AD.</span></span>
2. <span data-ttu-id="ebb06-115">Adal-t telepíti és konfigurálja a hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ebb06-115">Install ADAL and configure hello single-page app.</span></span>
3. <span data-ttu-id="ebb06-116">ADAL toohelp biztonságos lapok hello alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="ebb06-116">Use ADAL toohelp secure pages in hello single-page app.</span></span>

<span data-ttu-id="ebb06-117">elindult, tooget [hello app vázat letöltése](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) vagy [letöltése befejeződött hello minta](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="ebb06-117">tooget started, [download hello app skeleton](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) or [download hello completed sample](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="ebb06-118">Akkor is, amelyben felhasználók létrehozása és egy alkalmazás regisztrálása az Azure AD-bérlő kell.</span><span class="sxs-lookup"><span data-stu-id="ebb06-118">You also need an Azure AD tenant in which you can create users and register an application.</span></span> <span data-ttu-id="ebb06-119">Ha még nem rendelkezik a bérlő [megtudhatja, hogyan egy tooget](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="ebb06-119">If you don't already have a tenant, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

## <a name="step-1-register-hello-directorysearcher-application"></a><span data-ttu-id="ebb06-120">1. lépés: Hello DirectorySearcher alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="ebb06-120">Step 1: Register hello DirectorySearcher application</span></span>
<span data-ttu-id="ebb06-121">tooenable a app tooauthenticate felhasználók és a get-jogkivonatokat, először tooregister azt az Azure ad bérlői:</span><span class="sxs-lookup"><span data-stu-id="ebb06-121">tooenable your app tooauthenticate users and get tokens, you first need tooregister it in your Azure AD tenant:</span></span>

1. <span data-ttu-id="ebb06-122">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ebb06-122">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ebb06-123">Ha be van jelentkezve toomultiple könyvtárak, szükség lehet a tooensure megtekintett hello megfelelő könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="ebb06-123">If you are signed in toomultiple directories, you may need tooensure you are viewing hello correct directory.</span></span> <span data-ttu-id="ebb06-124">toodo hello felső sávon, kattintson a fiókját.</span><span class="sxs-lookup"><span data-stu-id="ebb06-124">toodo so, on hello top bar, click your account.</span></span> <span data-ttu-id="ebb06-125">A hello **Directory** menüben válassza ki a kívánt tooregister hello Azure AD-bérlő az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ebb06-125">Under hello **Directory** list, choose hello Azure AD tenant where you want tooregister your application.</span></span>
3. <span data-ttu-id="ebb06-126">Kattintson a **több szolgáltatások** hello bal oldali ablaktáblán, és válassza ki **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ebb06-126">Click **More Services** in hello left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="ebb06-127">Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="ebb06-127">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="ebb06-128">Hello utasításokat követve, és hozzon létre egy új webalkalmazás és/vagy webes API:</span><span class="sxs-lookup"><span data-stu-id="ebb06-128">Follow hello prompts and create a new web application and/or web API:</span></span>
  * <span data-ttu-id="ebb06-129">**Név** az alkalmazás toousers ismerteti.</span><span class="sxs-lookup"><span data-stu-id="ebb06-129">**Name** describes your application toousers.</span></span>
  * <span data-ttu-id="ebb06-130">**Átirányítási Uri** hello hely toowhich az Azure AD visszaadható jogkivonatok.</span><span class="sxs-lookup"><span data-stu-id="ebb06-130">**Redirect Uri** is hello location toowhich Azure AD will return tokens.</span></span> <span data-ttu-id="ebb06-131">Ez a minta hello alapértelmezett helye `https://localhost:44326/`.</span><span class="sxs-lookup"><span data-stu-id="ebb06-131">hello default location for this sample is `https://localhost:44326/`.</span></span>
6. <span data-ttu-id="ebb06-132">Regisztráció befejezése után az Azure AD egy egyedi alkalmazás azonosítója tooyour app rendeli hozzá.</span><span class="sxs-lookup"><span data-stu-id="ebb06-132">After you finish registration, Azure AD assigns a unique application ID tooyour app.</span></span>  <span data-ttu-id="ebb06-133">Ez az érték kell a következő szakaszok hello, ezért másolja hello alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="ebb06-133">You'll need this value in hello next sections, so copy it from hello application tab.</span></span>
7. <span data-ttu-id="ebb06-134">Adal.js hello OAuth implicit engedélyezési folyamat toocommunicate használja az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="ebb06-134">Adal.js uses hello OAuth implicit flow toocommunicate with Azure AD.</span></span> <span data-ttu-id="ebb06-135">Az alkalmazás hello implicit engedélyezési folyamat kell engedélyezni:</span><span class="sxs-lookup"><span data-stu-id="ebb06-135">You must enable hello implicit flow for your application:</span></span>
  1. <span data-ttu-id="ebb06-136">Kattintson a hello alkalmazást, és válassza ki **Manifest** tooopen hello beágyazott jegyzék szerkesztő.</span><span class="sxs-lookup"><span data-stu-id="ebb06-136">Click hello application and select **Manifest** tooopen hello inline manifest editor.</span></span>
  2. <span data-ttu-id="ebb06-137">Keresse meg a hello `oauth2AllowImplicitFlow` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="ebb06-137">Locate hello `oauth2AllowImplicitFlow` property.</span></span> <span data-ttu-id="ebb06-138">Állítsa be az értékét túl`true`.</span><span class="sxs-lookup"><span data-stu-id="ebb06-138">Set its value too`true`.</span></span>
  3. <span data-ttu-id="ebb06-139">Kattintson a **mentése** toosave hello jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="ebb06-139">Click **Save** toosave hello manifest.</span></span>
8. <span data-ttu-id="ebb06-140">Az alkalmazás a bérlő jogosultságot adni.</span><span class="sxs-lookup"><span data-stu-id="ebb06-140">Grant permissions across your tenant for your application.</span></span> <span data-ttu-id="ebb06-141">Nyissa meg túl**beállítások** > **tulajdonságok** > **szükséges engedélyek**, hello kattintson **engedélyt adjon**hello felső sáv gombjára.</span><span class="sxs-lookup"><span data-stu-id="ebb06-141">Go too**Settings** > **Properties** > **Required Permissions**, and click hello **Grant Permissions** button on hello top bar.</span></span> <span data-ttu-id="ebb06-142">Kattintson a **Igen** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="ebb06-142">Click **Yes** tooconfirm.</span></span>

## <a name="step-2-install-adal-and-configure-hello-single-page-app"></a><span data-ttu-id="ebb06-143">2. lépés: Telepítse adal-t és hello alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ebb06-143">Step 2: Install ADAL and configure hello single-page app</span></span>
<span data-ttu-id="ebb06-144">Most, hogy az Azure AD-alkalmazás, adal.js telepítse, és az identitás-kapcsolódó kód írása.</span><span class="sxs-lookup"><span data-stu-id="ebb06-144">Now that you have an application in Azure AD, you can install adal.js and write your identity-related code.</span></span>

### <a name="configure-hello-javascript-client"></a><span data-ttu-id="ebb06-145">Hello JavaScript-ügyfél konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ebb06-145">Configure hello JavaScript client</span></span>
<span data-ttu-id="ebb06-146">Kezdje az adal.js toohello TodoSPA projekt hozzáadása hello Csomagkezelő konzol használatával:</span><span class="sxs-lookup"><span data-stu-id="ebb06-146">Begin by adding adal.js toohello TodoSPA project by using hello Package Manager Console:</span></span>
  1. <span data-ttu-id="ebb06-147">Töltse le [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) , és adja hozzá toohello `App/Scripts/` projekt könyvtárában.</span><span class="sxs-lookup"><span data-stu-id="ebb06-147">Download [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) and add it toohello `App/Scripts/` project directory.</span></span>
  2. <span data-ttu-id="ebb06-148">Töltse le [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) , és adja hozzá toohello `App/Scripts/` projekt könyvtárában.</span><span class="sxs-lookup"><span data-stu-id="ebb06-148">Download [adal-angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) and add it toohello `App/Scripts/` project directory.</span></span>
  3. <span data-ttu-id="ebb06-149">Minden parancsprogramhoz hello hello vége előtt betöltése `</body>` a `index.html`:</span><span class="sxs-lookup"><span data-stu-id="ebb06-149">Load each script before hello end of hello `</body>` in `index.html`:</span></span>

    ```js
    ...
    <script src="App/Scripts/adal.js"></script>
    <script src="App/Scripts/adal-angular.js"></script>
    ...
    ```

### <a name="configure-hello-back-end-server"></a><span data-ttu-id="ebb06-150">Hello háttér-kiszolgáló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ebb06-150">Configure hello back end server</span></span>
<span data-ttu-id="ebb06-151">Hello egyoldalas alkalmazások háttér-tooDo lista API tooaccept jogkivonatokat hello böngészőből hello háttér kell hello app regisztrációs konfigurációs adatait.</span><span class="sxs-lookup"><span data-stu-id="ebb06-151">For hello single-page app's back-end tooDo List API tooaccept tokens from hello browser, hello back end needs configuration information about hello app registration.</span></span> <span data-ttu-id="ebb06-152">Hello TodoSPA projektben nyissa meg `web.config`.</span><span class="sxs-lookup"><span data-stu-id="ebb06-152">In hello TodoSPA project, open `web.config`.</span></span> <span data-ttu-id="ebb06-153">Cserélje le a hello értékek hello hello elemeinek `<appSettings>` szakasz tooreflect hello értékek a használt hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="ebb06-153">Replace hello values of hello elements in hello `<appSettings>` section tooreflect hello values that you used in hello Azure portal.</span></span> <span data-ttu-id="ebb06-154">A kód minden alkalommal ADAL hivatkozik ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="ebb06-154">Your code will reference these values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="ebb06-155">`ida:Tenant`az Azure AD bérlője – például a contoso.onmicrosoft.com hello tartomány.</span><span class="sxs-lookup"><span data-stu-id="ebb06-155">`ida:Tenant` is hello domain of your Azure AD tenant--for example, contoso.onmicrosoft.com.</span></span>
  * <span data-ttu-id="ebb06-156">`ida:Audience`az alkalmazás hello portálról másolt hello ügyfél-azonosító van.</span><span class="sxs-lookup"><span data-stu-id="ebb06-156">`ida:Audience` is hello client ID of your application that you copied from hello portal.</span></span>

## <a name="step-3-use-adal-toohelp-secure-pages-in-hello-single-page-app"></a><span data-ttu-id="ebb06-157">3. lépés: Használja ADAL toohelp biztonságos lapok hello egyoldalas alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="ebb06-157">Step 3: Use ADAL toohelp secure pages in hello single-page app</span></span>
<span data-ttu-id="ebb06-158">Adal.js jól integrálható az AngularJS útvonal és a HTTP-szolgáltatók, az alkalmazás egyoldalas segíthet a biztonságos egyéni nézeteket.</span><span class="sxs-lookup"><span data-stu-id="ebb06-158">Adal.js integrates with AngularJS route and HTTP providers, so you can help secure individual views in your single-page app.</span></span>

1. <span data-ttu-id="ebb06-159">A `App/Scripts/app.js`, kapcsolja a hello adal.js modul:</span><span class="sxs-lookup"><span data-stu-id="ebb06-159">In `App/Scripts/app.js`, bring in hello adal.js module:</span></span>

    ```js
    angular.module('todoApp', ['ngRoute','AdalAngular'])
    .config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
     function ($routeProvider, $httpProvider, adalProvider) {
    ...
    ```
2. <span data-ttu-id="ebb06-160">Inicializálni `adalProvider` használatával hello konfigurációs értékeket az alkalmazás regisztrációja is `App/Scripts/app.js`:</span><span class="sxs-lookup"><span data-stu-id="ebb06-160">Initialize `adalProvider` by using hello configuration values of your application registration, also in `App/Scripts/app.js`:</span></span>

    ```js
    adalProvider.init(
      {
          instance: 'https://login.microsoftonline.com/',
          tenant: 'Enter your tenant name here e.g. contoso.onmicrosoft.com',
          clientId: 'Enter your client ID here e.g. e9a5a8b6-8af7-4719-9821-0deef255f68e',
          extraQueryParameter: 'nux=1',
          //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
      },
      $httpProvider
    );
    ```
3. <span data-ttu-id="ebb06-161">Biztonságos hello segítségével `TodoList` hello alkalmazásban csak egy kódsort a nézet: `requireADLogin`.</span><span class="sxs-lookup"><span data-stu-id="ebb06-161">Help secure hello `TodoList` view in hello app by using only one line of code: `requireADLogin`.</span></span>

    ```js
    ...
    }).when("/TodoList", {
            controller: "todoListCtrl",
            templateUrl: "/App/Views/TodoList.html",
            requireADLogin: true,
    ...
    ```

## <a name="summary"></a><span data-ttu-id="ebb06-162">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="ebb06-162">Summary</span></span>
<span data-ttu-id="ebb06-163">Most már rendelkezik egy biztonságos alkalmazás, amely a felhasználók bejelentkezhetnek és tulajdonosi jogkivonat-védelemmel ellátott kérelmek tooits háttér-API ki.</span><span class="sxs-lookup"><span data-stu-id="ebb06-163">You now have a secure single-page app that can sign in users and issue bearer-token-protected requests tooits back-end API.</span></span> <span data-ttu-id="ebb06-164">Amikor a felhasználó rákattint hello **TodoList** hivatkozást, a adal.js automatikusan átirányítja a tooAzure AD-hez, jelentkezzen be szükséges.</span><span class="sxs-lookup"><span data-stu-id="ebb06-164">When a user clicks hello **TodoList** link, adal.js will automatically redirect tooAzure AD for sign-in if necessary.</span></span> <span data-ttu-id="ebb06-165">Ezenkívül adal.js automatikusan elvégzi az access token tooany Ajax-kérelmek toohello app háttér küldött.</span><span class="sxs-lookup"><span data-stu-id="ebb06-165">In addition, adal.js will automatically attach an access token tooany Ajax requests that are sent toohello app's back end.</span></span>  

<span data-ttu-id="ebb06-166">hello előző lépésekre hello operációs rendszer minimálisan szükséges toobuild egy alkalmazás adal.js használatával.</span><span class="sxs-lookup"><span data-stu-id="ebb06-166">hello preceding steps are hello bare minimum necessary toobuild a single-page app by using adal.js.</span></span> <span data-ttu-id="ebb06-167">Azonban néhány egyéb szolgáltatásokat az alkalmazás hasznos információkat:</span><span class="sxs-lookup"><span data-stu-id="ebb06-167">But a few other features are useful in single-page app:</span></span>

* <span data-ttu-id="ebb06-168">tooexplicitly be- és kijelentkezési kérések kiállítása, a tartományvezérlőket, amelyek aktiválják az adal.js funkciók is meghatározhatja.</span><span class="sxs-lookup"><span data-stu-id="ebb06-168">tooexplicitly issue sign-in and sign-out requests, you can define functions in your controllers that invoke adal.js.</span></span>  <span data-ttu-id="ebb06-169">A `App/Scripts/homeCtrl.js`:</span><span class="sxs-lookup"><span data-stu-id="ebb06-169">In `App/Scripts/homeCtrl.js`:</span></span>

    ```js
    ...
    $scope.login = function () {
        adalService.login();
    };
    $scope.logout = function () {
        adalService.logOut();
    };
    ...
    ```
* <span data-ttu-id="ebb06-170">Érdemes lehet toopresent felhasználói adatok hello alkalmazás felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="ebb06-170">You might want toopresent user information in hello app's UI.</span></span> <span data-ttu-id="ebb06-171">hello ADAL szolgáltatáshoz már hozzáadtak toohello `userDataCtrl` vezérlő, hogy hozzáférjen a hello `userInfo` hello objektum társított nézet, `App/Views/UserData.html`:</span><span class="sxs-lookup"><span data-stu-id="ebb06-171">hello ADAL service has already been added toohello `userDataCtrl` controller, so you can access hello `userInfo` object in hello associated view, `App/Views/UserData.html`:</span></span>

    ```js
    <p>{{userInfo.userName}}</p>
    <p>aud:{{userInfo.profile.aud}}</p>
    <p>iss:{{userInfo.profile.iss}}</p>
    ...
    ```

* <span data-ttu-id="ebb06-172">Nincsenek számos forgatókönyv, ahol érdemes tooknow Ha hello felhasználó van bejelentkezve.</span><span class="sxs-lookup"><span data-stu-id="ebb06-172">There are many scenarios in which you'll want tooknow if hello user is signed in or not.</span></span> <span data-ttu-id="ebb06-173">Is használhatja a hello `userInfo` objektum toogather ezt az információt.</span><span class="sxs-lookup"><span data-stu-id="ebb06-173">You can also use hello `userInfo` object toogather this information.</span></span>  <span data-ttu-id="ebb06-174">Például a `index.html`, megjelenítheti vagy hello **bejelentkezési** vagy **kijelentkezési** gomb a hitelesítési állapot alapján:</span><span class="sxs-lookup"><span data-stu-id="ebb06-174">For instance, in `index.html`, you can show either hello **Login** or **Logout** button based on authentication status:</span></span>

    ```js
    <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
    <li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    ```

<span data-ttu-id="ebb06-175">Az Azure Active Directoryba integrált alkalmazás is hitelesíti a felhasználókat, biztonságosan OAuth 2.0 használatával hívható meg a háttér és alapszintű hello felhasználó adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="ebb06-175">Your Azure AD-integrated single-page app can authenticate users, securely call its back end by using OAuth 2.0, and get basic information about hello user.</span></span> <span data-ttu-id="ebb06-176">Ha még nem tette meg, most az hello idő toopopulate a bérlő az egyes felhasználók.</span><span class="sxs-lookup"><span data-stu-id="ebb06-176">If you haven't already, now is hello time toopopulate your tenant with some users.</span></span> <span data-ttu-id="ebb06-177">Futtassa a tooDo lista egyoldalas alkalmazást, és jelentkezzen be valamelyik azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="ebb06-177">Run your tooDo List single-page app, and sign in with one of those users.</span></span> <span data-ttu-id="ebb06-178">Adja hozzá a feladatok toohello felhasználói feladatlistában, jelentkezzen ki, és jelentkezzen be újra a.</span><span class="sxs-lookup"><span data-stu-id="ebb06-178">Add tasks toohello user's to-do list, sign out, and sign back in.</span></span>

<span data-ttu-id="ebb06-179">Adal.js segítségével könnyen tooincorporate általános identitás-szolgáltatások az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="ebb06-179">Adal.js makes it easy tooincorporate common identity features into your application.</span></span> <span data-ttu-id="ebb06-180">Ez gondoskodik összes hello dirty munkahelyi meg: gyorsítótár kezelése, OAuth protokoll támogatása, bemutató hello felhasználó egy bejelentkezési felhasználói felületén, lejárt jogkivonatokat, és több frissítése.</span><span class="sxs-lookup"><span data-stu-id="ebb06-180">It takes care of all hello dirty work for you: cache management, OAuth protocol support, presenting hello user with a sign-in UI, refreshing expired tokens, and more.</span></span>

<span data-ttu-id="ebb06-181">Referenciaként befejeződött hello mintát (a konfigurációs értékek nélkül) érhető el a [GitHub](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="ebb06-181">For reference, hello completed sample (without your configuration values) is available in [GitHub](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ebb06-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ebb06-182">Next steps</span></span>
<span data-ttu-id="ebb06-183">Most már továbbléphet tooadditional forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="ebb06-183">You can now move on tooadditional scenarios.</span></span> <span data-ttu-id="ebb06-184">Érdemes lehet tootry: [CORS webes API meghívása egy egyoldalas alkalmazásból](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet).</span><span class="sxs-lookup"><span data-stu-id="ebb06-184">You might want tootry: [Call a CORS web API from a single-page app](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
