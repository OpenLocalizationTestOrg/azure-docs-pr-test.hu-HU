---
title: "Ismerkedés az Azure AD AngularJS |} Microsoft Docs"
description: "Megtudhatja, hogyan hozható létre egy AngularJS egyoldalas alkalmazás, amely integrálható az Azure ad-val bejelentkezhet, és meghívja az Azure AD-védelemmel ellátott API-OAuth használatával."
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
ms.openlocfilehash: 4153910bc03f112f84c26cda6f9c78f11028b934
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="help-secure-angularjs-single-page-apps-by-using-azure-ad"></a><span data-ttu-id="96aba-103">Számítógépek biztonságossá tétele AngularJS egyoldalas alkalmazások az Azure AD segítségével</span><span class="sxs-lookup"><span data-stu-id="96aba-103">Help secure AngularJS single-page apps by using Azure AD</span></span>

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="96aba-104">Az Azure Active Directory (Azure AD) teszi egyszerű és magától értetődő, amelyen felvehet bejelentkezési, kijelentkezési és biztonságos OAuth API meghívja az egyoldalas alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="96aba-104">Azure Active Directory (Azure AD) makes it simple and straightforward for you to add sign-in, sign-out, and secure OAuth API calls to your single-page apps.</span></span>  <span data-ttu-id="96aba-105">Ez lehetővé teszi az alkalmazások a Windows Server Active Directory-fiókkal rendelkező felhasználók hitelesítéséhez, és a webes API-t, amely az Azure AD védi, például az Office 365 API-k vagy az Azure API-t használ.</span><span class="sxs-lookup"><span data-stu-id="96aba-105">It enables your apps to authenticate users with their Windows Server Active Directory accounts and consume any web API that Azure AD helps protect, such as the Office 365 APIs or the Azure API.</span></span>

<span data-ttu-id="96aba-106">Böngészőjében futó JavaScript-alkalmazások esetében az Azure AD az Active Directory Authentication Library (ADAL), vagy adal.js biztosít.</span><span class="sxs-lookup"><span data-stu-id="96aba-106">For JavaScript applications running in a browser, Azure AD provides the Active Directory Authentication Library (ADAL), or adal.js.</span></span> <span data-ttu-id="96aba-107">A kizárólagos adal.js célja megkönnyíti a hozzáférési jogkivonatok lekérésére, az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="96aba-107">The sole purpose of adal.js is to make it easy for your app to get access tokens.</span></span> <span data-ttu-id="96aba-108">Annak bemutatásához, milyen könnyű van, itt azt fogja AngularJS teendőlista alkalmazás létrehozásához, amely:</span><span class="sxs-lookup"><span data-stu-id="96aba-108">To demonstrate just how easy it is, here we'll build an AngularJS To Do List application that:</span></span>

* <span data-ttu-id="96aba-109">A felhasználó alkalmazásba jelentkezik be az Azure AD használatával az identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="96aba-109">Signs the user in to the app by using Azure AD as the identity provider.</span></span>

* <span data-ttu-id="96aba-110">A felhasználó bizonyos információkat jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="96aba-110">Displays some information about the user.</span></span>
* <span data-ttu-id="96aba-111">Biztonságosan hívja az alkalmazás számára tegye lista API-t az Azure AD tulajdonosi jogkivonatok segítségével.</span><span class="sxs-lookup"><span data-stu-id="96aba-111">Securely calls the app's To Do List API by using bearer tokens from Azure AD.</span></span>
* <span data-ttu-id="96aba-112">Az alkalmazásból a felhasználó bejelentkezésekor.</span><span class="sxs-lookup"><span data-stu-id="96aba-112">Signs the user out of the app.</span></span>

<span data-ttu-id="96aba-113">A teljes, működő alkalmazást készítéséhez kell:</span><span class="sxs-lookup"><span data-stu-id="96aba-113">To build the complete, working application, you need to:</span></span>

1. <span data-ttu-id="96aba-114">Regisztrálja az alkalmazást az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="96aba-114">Register your app with Azure AD.</span></span>
2. <span data-ttu-id="96aba-115">Adal-t telepíti és konfigurálja az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="96aba-115">Install ADAL and configure the single-page app.</span></span>
3. <span data-ttu-id="96aba-116">Biztonságos oldalán az alkalmazás adal-t használja.</span><span class="sxs-lookup"><span data-stu-id="96aba-116">Use ADAL to help secure pages in the single-page app.</span></span>

<span data-ttu-id="96aba-117">A kezdéshez [töltse le az alkalmazás vázat](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) vagy [töltse le az elkészült mintát](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="96aba-117">To get started, [download the app skeleton](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) or [download the completed sample](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).</span></span> <span data-ttu-id="96aba-118">Akkor is, amelyben felhasználók létrehozása és egy alkalmazás regisztrálása az Azure AD-bérlő kell.</span><span class="sxs-lookup"><span data-stu-id="96aba-118">You also need an Azure AD tenant in which you can create users and register an application.</span></span> <span data-ttu-id="96aba-119">Ha még nem rendelkezik a bérlő [beszerzéséről egy](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="96aba-119">If you don't already have a tenant, [learn how to get one](active-directory-howto-tenant.md).</span></span>

## <a name="step-1-register-the-directorysearcher-application"></a><span data-ttu-id="96aba-120">1. lépés: A DirectorySearcher alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="96aba-120">Step 1: Register the DirectorySearcher application</span></span>
<span data-ttu-id="96aba-121">Ahhoz, hogy az alkalmazás hitelesíti a felhasználókat, és a jogkivonatok lekérésére, először regisztrálnia kell azt az Azure AD-bérlőben:</span><span class="sxs-lookup"><span data-stu-id="96aba-121">To enable your app to authenticate users and get tokens, you first need to register it in your Azure AD tenant:</span></span>

1. <span data-ttu-id="96aba-122">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="96aba-122">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="96aba-123">Ha be van jelentkezve több könyvtárak, szükség lehet annak érdekében, hogy a megfelelő könyvtárban megtekintésekor.</span><span class="sxs-lookup"><span data-stu-id="96aba-123">If you are signed in to multiple directories, you may need to ensure you are viewing the correct directory.</span></span> <span data-ttu-id="96aba-124">Ehhez a felső eszköztáron kattintson a fiókját.</span><span class="sxs-lookup"><span data-stu-id="96aba-124">To do so, on the top bar, click your account.</span></span> <span data-ttu-id="96aba-125">Az a **Directory** menüben válassza ki az Azure AD-bérlőt, ahová az alkalmazás regisztrálásához.</span><span class="sxs-lookup"><span data-stu-id="96aba-125">Under the **Directory** list, choose the Azure AD tenant where you want to register your application.</span></span>
3. <span data-ttu-id="96aba-126">Kattintson a **több szolgáltatások** a bal oldali ablaktáblán, és válassza a **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="96aba-126">Click **More Services** in the left pane, and then select **Azure Active Directory**.</span></span>
4. <span data-ttu-id="96aba-127">Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="96aba-127">Click **App registrations**, and then select **Add**.</span></span>
5. <span data-ttu-id="96aba-128">Kövesse az utasításokat, és hozzon létre egy új webalkalmazás és/vagy webes API:</span><span class="sxs-lookup"><span data-stu-id="96aba-128">Follow the prompts and create a new web application and/or web API:</span></span>
  * <span data-ttu-id="96aba-129">**Név** az alkalmazás a felhasználók számára ismerteti.</span><span class="sxs-lookup"><span data-stu-id="96aba-129">**Name** describes your application to users.</span></span>
  * <span data-ttu-id="96aba-130">**Átirányítási Uri** az a hely, amelyhez az Azure AD jogkivonatok ad vissza.</span><span class="sxs-lookup"><span data-stu-id="96aba-130">**Redirect Uri** is the location to which Azure AD will return tokens.</span></span> <span data-ttu-id="96aba-131">Az alapértelmezett helye Ez a minta `https://localhost:44326/`.</span><span class="sxs-lookup"><span data-stu-id="96aba-131">The default location for this sample is `https://localhost:44326/`.</span></span>
6. <span data-ttu-id="96aba-132">Regisztráció befejezése után az Azure AD egy egyedi alkalmazás Azonosítót rendel az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="96aba-132">After you finish registration, Azure AD assigns a unique application ID to your app.</span></span>  <span data-ttu-id="96aba-133">Ez az érték kell a következő szakaszokban lévő, másolja az alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="96aba-133">You'll need this value in the next sections, so copy it from the application tab.</span></span>
7. <span data-ttu-id="96aba-134">Adal.js az OAuth implicit engedélyezési folyamat használatával kommunikálni az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="96aba-134">Adal.js uses the OAuth implicit flow to communicate with Azure AD.</span></span> <span data-ttu-id="96aba-135">Az alkalmazás engedélyeznie kell a implicit engedélyezési folyamat:</span><span class="sxs-lookup"><span data-stu-id="96aba-135">You must enable the implicit flow for your application:</span></span>
  1. <span data-ttu-id="96aba-136">Kattintson az alkalmazás, és válassza **Manifest** a jegyzék beágyazott-szerkesztő megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="96aba-136">Click the application and select **Manifest** to open the inline manifest editor.</span></span>
  2. <span data-ttu-id="96aba-137">Keresse meg a `oauth2AllowImplicitFlow` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="96aba-137">Locate the `oauth2AllowImplicitFlow` property.</span></span> <span data-ttu-id="96aba-138">Állítsa be az értékét `true`.</span><span class="sxs-lookup"><span data-stu-id="96aba-138">Set its value to `true`.</span></span>
  3. <span data-ttu-id="96aba-139">Kattintson a **mentése** a jegyzékfájl mentése.</span><span class="sxs-lookup"><span data-stu-id="96aba-139">Click **Save** to save the manifest.</span></span>
8. <span data-ttu-id="96aba-140">Az alkalmazás a bérlő jogosultságot adni.</span><span class="sxs-lookup"><span data-stu-id="96aba-140">Grant permissions across your tenant for your application.</span></span> <span data-ttu-id="96aba-141">Lépjen **beállítások** > **tulajdonságok** > **szükséges engedélyek**, és kattintson a **engedélyt adjon** a felső eszköztáron gombra.</span><span class="sxs-lookup"><span data-stu-id="96aba-141">Go to **Settings** > **Properties** > **Required Permissions**, and click the **Grant Permissions** button on the top bar.</span></span> <span data-ttu-id="96aba-142">Kattintson a **Yes** (Igen) gombra a megerősítéshez.</span><span class="sxs-lookup"><span data-stu-id="96aba-142">Click **Yes** to confirm.</span></span>

## <a name="step-2-install-adal-and-configure-the-single-page-app"></a><span data-ttu-id="96aba-143">2. lépés: Telepítse adal-t és az alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="96aba-143">Step 2: Install ADAL and configure the single-page app</span></span>
<span data-ttu-id="96aba-144">Most, hogy az Azure AD-alkalmazás, adal.js telepítse, és az identitás-kapcsolódó kód írása.</span><span class="sxs-lookup"><span data-stu-id="96aba-144">Now that you have an application in Azure AD, you can install adal.js and write your identity-related code.</span></span>

### <a name="configure-the-javascript-client"></a><span data-ttu-id="96aba-145">A JavaScript-ügyfél konfigurálása</span><span class="sxs-lookup"><span data-stu-id="96aba-145">Configure the JavaScript client</span></span>
<span data-ttu-id="96aba-146">Kezdje az adal.js hozzáadása a TodoSPA projekt a Csomagkezelő konzol segítségével:</span><span class="sxs-lookup"><span data-stu-id="96aba-146">Begin by adding adal.js to the TodoSPA project by using the Package Manager Console:</span></span>
  1. <span data-ttu-id="96aba-147">Töltse le [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) és hozzá kell adnia a `App/Scripts/` projekt könyvtárában.</span><span class="sxs-lookup"><span data-stu-id="96aba-147">Download [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) and add it to the `App/Scripts/` project directory.</span></span>
  2. <span data-ttu-id="96aba-148">Töltse le [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) és hozzá kell adnia a `App/Scripts/` projekt könyvtárában.</span><span class="sxs-lookup"><span data-stu-id="96aba-148">Download [adal-angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) and add it to the `App/Scripts/` project directory.</span></span>
  3. <span data-ttu-id="96aba-149">Minden parancsprogram végéig betölteni a `</body>` a `index.html`:</span><span class="sxs-lookup"><span data-stu-id="96aba-149">Load each script before the end of the `</body>` in `index.html`:</span></span>

    ```js
    ...
    <script src="App/Scripts/adal.js"></script>
    <script src="App/Scripts/adal-angular.js"></script>
    ...
    ```

### <a name="configure-the-back-end-server"></a><span data-ttu-id="96aba-150">A háttér-kiszolgáló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="96aba-150">Configure the back end server</span></span>
<span data-ttu-id="96aba-151">A egyoldalas alkalmazások háttér-való tegye lista API-t fogadni a böngészőből a háttér kell konfigurációs adatait az alkalmazás regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="96aba-151">For the single-page app's back-end To Do List API to accept tokens from the browser, the back end needs configuration information about the app registration.</span></span> <span data-ttu-id="96aba-152">A TodoSPA projektben nyissa meg a `web.config`.</span><span class="sxs-lookup"><span data-stu-id="96aba-152">In the TodoSPA project, open `web.config`.</span></span> <span data-ttu-id="96aba-153">Cserélje le az értékeket az elemek a `<appSettings>` szakaszban az Azure portálon használt értékeknek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="96aba-153">Replace the values of the elements in the `<appSettings>` section to reflect the values that you used in the Azure portal.</span></span> <span data-ttu-id="96aba-154">A kód minden alkalommal ADAL hivatkozik ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="96aba-154">Your code will reference these values whenever it uses ADAL.</span></span>
  * <span data-ttu-id="96aba-155">`ida:Tenant`az Azure AD bérlője – például a contoso.onmicrosoft.com tartomány.</span><span class="sxs-lookup"><span data-stu-id="96aba-155">`ida:Tenant` is the domain of your Azure AD tenant--for example, contoso.onmicrosoft.com.</span></span>
  * <span data-ttu-id="96aba-156">`ida:Audience`az ügyfél-Azonosítót, az alkalmazás a portálról másolt van.</span><span class="sxs-lookup"><span data-stu-id="96aba-156">`ida:Audience` is the client ID of your application that you copied from the portal.</span></span>

## <a name="step-3-use-adal-to-help-secure-pages-in-the-single-page-app"></a><span data-ttu-id="96aba-157">3. lépés: Használja az alkalmazás biztonságos lapja segítségével ADAL</span><span class="sxs-lookup"><span data-stu-id="96aba-157">Step 3: Use ADAL to help secure pages in the single-page app</span></span>
<span data-ttu-id="96aba-158">Adal.js jól integrálható az AngularJS útvonal és a HTTP-szolgáltatók, az alkalmazás egyoldalas segíthet a biztonságos egyéni nézeteket.</span><span class="sxs-lookup"><span data-stu-id="96aba-158">Adal.js integrates with AngularJS route and HTTP providers, so you can help secure individual views in your single-page app.</span></span>

1. <span data-ttu-id="96aba-159">A `App/Scripts/app.js`, kapcsolja a adal.js modulban:</span><span class="sxs-lookup"><span data-stu-id="96aba-159">In `App/Scripts/app.js`, bring in the adal.js module:</span></span>

    ```js
    angular.module('todoApp', ['ngRoute','AdalAngular'])
    .config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
     function ($routeProvider, $httpProvider, adalProvider) {
    ...
    ```
2. <span data-ttu-id="96aba-160">Inicializálni `adalProvider` segítségével a konfigurációs értékeket az alkalmazás-regisztrációk is `App/Scripts/app.js`:</span><span class="sxs-lookup"><span data-stu-id="96aba-160">Initialize `adalProvider` by using the configuration values of your application registration, also in `App/Scripts/app.js`:</span></span>

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
3. <span data-ttu-id="96aba-161">Számítógépek biztonságossá tétele a `TodoList` nézet használatával egyetlen kódsort az alkalmazásban: `requireADLogin`.</span><span class="sxs-lookup"><span data-stu-id="96aba-161">Help secure the `TodoList` view in the app by using only one line of code: `requireADLogin`.</span></span>

    ```js
    ...
    }).when("/TodoList", {
            controller: "todoListCtrl",
            templateUrl: "/App/Views/TodoList.html",
            requireADLogin: true,
    ...
    ```

## <a name="summary"></a><span data-ttu-id="96aba-162">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="96aba-162">Summary</span></span>
<span data-ttu-id="96aba-163">Most már rendelkezik egy biztonságos alkalmazás, amely a felhasználók bejelentkezhetnek és tulajdonosi jogkivonat-védelemmel ellátott kérelmeket kiadni a háttér-API.</span><span class="sxs-lookup"><span data-stu-id="96aba-163">You now have a secure single-page app that can sign in users and issue bearer-token-protected requests to its back-end API.</span></span> <span data-ttu-id="96aba-164">Amikor a felhasználó rákattint a **TodoList** hivatkozásra a adal.js automatikusan átirányítja az Azure AD-hez bejelentkezési szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="96aba-164">When a user clicks the **TodoList** link, adal.js will automatically redirect to Azure AD for sign-in if necessary.</span></span> <span data-ttu-id="96aba-165">Ezenkívül adal.js automatikusan kapcsolódni fog olyan hozzáférési jogkivonatot, bármely Ajax-kérelmek az alkalmazás háttér küldött.</span><span class="sxs-lookup"><span data-stu-id="96aba-165">In addition, adal.js will automatically attach an access token to any Ajax requests that are sent to the app's back end.</span></span>  

<span data-ttu-id="96aba-166">Az előző lépéseket kell a operációs rendszer minimálisan szükséges adal.js használatával egy egyoldalas alkalmazás elkészítésére.</span><span class="sxs-lookup"><span data-stu-id="96aba-166">The preceding steps are the bare minimum necessary to build a single-page app by using adal.js.</span></span> <span data-ttu-id="96aba-167">Azonban néhány egyéb szolgáltatásokat az alkalmazás hasznos információkat:</span><span class="sxs-lookup"><span data-stu-id="96aba-167">But a few other features are useful in single-page app:</span></span>

* <span data-ttu-id="96aba-168">Be- és kijelentkezési kérések explicit módon ki, a tartományvezérlőn, amely a adal.js meghívása funkciók is meghatározhatja.</span><span class="sxs-lookup"><span data-stu-id="96aba-168">To explicitly issue sign-in and sign-out requests, you can define functions in your controllers that invoke adal.js.</span></span>  <span data-ttu-id="96aba-169">A `App/Scripts/homeCtrl.js`:</span><span class="sxs-lookup"><span data-stu-id="96aba-169">In `App/Scripts/homeCtrl.js`:</span></span>

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
* <span data-ttu-id="96aba-170">Felhasználói adatokat az alkalmazás Kezelőfelületén megjeleníteni kívánt.</span><span class="sxs-lookup"><span data-stu-id="96aba-170">You might want to present user information in the app's UI.</span></span> <span data-ttu-id="96aba-171">Az ADAL szolgáltatás már szerepel az a `userDataCtrl` vezérlő, hogy hozzáférhessen a `userInfo` a társított nézetben objektum `App/Views/UserData.html`:</span><span class="sxs-lookup"><span data-stu-id="96aba-171">The ADAL service has already been added to the `userDataCtrl` controller, so you can access the `userInfo` object in the associated view, `App/Views/UserData.html`:</span></span>

    ```js
    <p>{{userInfo.userName}}</p>
    <p>aud:{{userInfo.profile.aud}}</p>
    <p>iss:{{userInfo.profile.iss}}</p>
    ...
    ```

* <span data-ttu-id="96aba-172">Nincsenek számos forgatókönyv, ahol érdemes tudni, hogy a felhasználó van bejelentkezve.</span><span class="sxs-lookup"><span data-stu-id="96aba-172">There are many scenarios in which you'll want to know if the user is signed in or not.</span></span> <span data-ttu-id="96aba-173">Használhatja a `userInfo` ehhez objektum.</span><span class="sxs-lookup"><span data-stu-id="96aba-173">You can also use the `userInfo` object to gather this information.</span></span>  <span data-ttu-id="96aba-174">Például a `index.html`, vagy megjelenítheti a **bejelentkezési** vagy **kijelentkezési** gomb a hitelesítési állapot alapján:</span><span class="sxs-lookup"><span data-stu-id="96aba-174">For instance, in `index.html`, you can show either the **Login** or **Logout** button based on authentication status:</span></span>

    ```js
    <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
    <li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    ```

<span data-ttu-id="96aba-175">Az Azure Active Directoryba integrált alkalmazás is hitelesíti a felhasználókat, biztonságosan OAuth 2.0 használatával hívható meg a háttér és a felhasználó alapszintű adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="96aba-175">Your Azure AD-integrated single-page app can authenticate users, securely call its back end by using OAuth 2.0, and get basic information about the user.</span></span> <span data-ttu-id="96aba-176">Ha még nem tette meg, most már az egyes felhasználóival a bérlő feltölti idő.</span><span class="sxs-lookup"><span data-stu-id="96aba-176">If you haven't already, now is the time to populate your tenant with some users.</span></span> <span data-ttu-id="96aba-177">Futtassa a teendőlista alkalmazás, és jelentkezzen be valamelyik azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="96aba-177">Run your To Do List single-page app, and sign in with one of those users.</span></span> <span data-ttu-id="96aba-178">Feladatok hozzáadása a felhasználói feladatlistában, jelentkezzen ki, és jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="96aba-178">Add tasks to the user's to-do list, sign out, and sign back in.</span></span>

<span data-ttu-id="96aba-179">Adal.js megkönnyíti, hogy az általános identitás-szolgáltatások átfogó az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="96aba-179">Adal.js makes it easy to incorporate common identity features into your application.</span></span> <span data-ttu-id="96aba-180">Ez gondoskodik a dirty munkát meg: gyorsítótár kezelése, az OAuth-protokoll támogatása, a felhasználó egy bejelentkezési felhasználói felület, lejárt jogkivonatokat, és több frissítése a bemutató.</span><span class="sxs-lookup"><span data-stu-id="96aba-180">It takes care of all the dirty work for you: cache management, OAuth protocol support, presenting the user with a sign-in UI, refreshing expired tokens, and more.</span></span>

<span data-ttu-id="96aba-181">Referenciaként az elkészült mintát (a konfigurációs értékek nélkül) érhető el a [GitHub](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="96aba-181">For reference, the completed sample (without your configuration values) is available in [GitHub](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).</span></span>

## <a name="next-steps"></a><span data-ttu-id="96aba-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="96aba-182">Next steps</span></span>
<span data-ttu-id="96aba-183">Most már továbbléphet további helyzeteket is.</span><span class="sxs-lookup"><span data-stu-id="96aba-183">You can now move on to additional scenarios.</span></span> <span data-ttu-id="96aba-184">Akkor célszerű: [CORS webes API meghívása egy egyoldalas alkalmazásból](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet).</span><span class="sxs-lookup"><span data-stu-id="96aba-184">You might want to try: [Call a CORS web API from a single-page app](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet).</span></span>

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
