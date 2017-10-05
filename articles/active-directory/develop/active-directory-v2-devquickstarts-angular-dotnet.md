---
title: "Azure AD v2.0 .NET AngularJS egylapos alkalmazás első lépések |} Microsoft Docs"
description: "Hogyan hozhat létre, hogy mindkét személyes Microsoft-fiókkal rendelkező felhasználók bejelentkezésekor szögben kifejezett JS egylapos alkalmazások és a munkahelyi vagy iskolai fiókját."
services: active-directory
documentationcenter: 
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 6a341781-278f-461b-92ca-7572a06e6852
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/23/2017
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: c68180c0ecabf5c0732f0db77ef1f3cc93be965b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-sign-in-to-an-angularjs-single-page-app---net"></a><span data-ttu-id="33941-103">Bejelentkezési hozzáadása egy AngularJS egylapos alkalmazás - .NET</span><span class="sxs-lookup"><span data-stu-id="33941-103">Add sign-in to an AngularJS single page app - .NET</span></span>
<span data-ttu-id="33941-104">Ebben a cikkben fel kell venni jelentkezzen be az alkalmazás bekapcsolja Microsoft-fiókok AngularJS alkalmazásokhoz az Azure Active Directory v2.0-végponttól használatával.</span><span class="sxs-lookup"><span data-stu-id="33941-104">In this article we'll add sign in with Microsoft powered accounts to an AngularJS app using the Azure Active Directory v2.0 endpoint.</span></span>  <span data-ttu-id="33941-105">A v2.0-végpontra lehetővé teszi egyetlen integrációs elvégzéséhez az alkalmazáson belüli és a személyes és munkahelyi vagy iskolai fiókkal rendelkező felhasználók hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="33941-105">The v2.0 endpoint enables you to perform a single integration in your app and authenticate users with both personal and work/school accounts.</span></span>

<span data-ttu-id="33941-106">Ez a minta egy egyszerű Tennivalólista egylapos alkalmazást, amely tárolja a feladatokat az olyan háttér REST API-t a .NET 4.5 MVC-keretrendszer használatával írt, és az Azure AD OAuth tulajdonosi jogkivonatok használatával biztonságossá.</span><span class="sxs-lookup"><span data-stu-id="33941-106">This sample is a simple To-Do List single page app that stores tasks in a backend REST API, written using the .NET 4.5 MVC framework and secured using OAuth bearer tokens from Azure AD.</span></span>  <span data-ttu-id="33941-107">Az AngularJS alkalmazás fog használni a nyílt forráskódú JavaScript hitelesítési kódtár [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) a teljes bejelentkezési folyamathoz, és a REST API hívásának jogkivonatainak szerezni.</span><span class="sxs-lookup"><span data-stu-id="33941-107">The AngularJS app will use our open source JavaScript authentication library [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) to handle the entire sign in process and acquire tokens for calling the REST API.</span></span>  <span data-ttu-id="33941-108">Ugyanilyen mintájú alkalmazható felé történő hitelesítésre más REST API-k, például a [Microsoft Graph](https://graph.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="33941-108">The same pattern can be applied to authenticate to other REST APIs, like the [Microsoft Graph](https://graph.microsoft.com).</span></span>

> [!NOTE]
> <span data-ttu-id="33941-109">Nem minden Azure Active Directory forgatókönyvek és funkciók támogatják a v2.0-végponttól.</span><span class="sxs-lookup"><span data-stu-id="33941-109">Not all Azure Active Directory scenarios & features are supported by the v2.0 endpoint.</span></span>  <span data-ttu-id="33941-110">Annak meghatározásához, ha a v2.0-végponttal kell használnia, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="33941-110">To determine if you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="download"></a><span data-ttu-id="33941-111">Letöltés</span><span class="sxs-lookup"><span data-stu-id="33941-111">Download</span></span>
<span data-ttu-id="33941-112">A kezdéshez töltse le és telepítse a Visual Studio szüksége.</span><span class="sxs-lookup"><span data-stu-id="33941-112">To get started, you'll need to download & install Visual Studio.</span></span>  <span data-ttu-id="33941-113">Majd átmásolhatja vagy [letöltése](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) egy üres alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="33941-113">Then you can clone or [download](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) a skeleton app:</span></span>

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet.git
```

<span data-ttu-id="33941-114">Az üres alkalmazás egyszerű AngularJS alkalmazások bolierplate kódot tartalmaz, de hiányzik az identitás-kapcsolódó darab mindegyikét.</span><span class="sxs-lookup"><span data-stu-id="33941-114">The skeleton app includes all the boilerplate code for a simple AngularJS app, but is missing all of the identity-related pieces.</span></span>  <span data-ttu-id="33941-115">Ha nem szeretné követéséhez, hanem klónozhat vagy [letöltése](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/complete.zip) az elkészült mintát.</span><span class="sxs-lookup"><span data-stu-id="33941-115">If you don't want to follow along, you can instead clone or [download](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/complete.zip) the completed sample.</span></span>

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-DotNet.git
```

## <a name="register-an-app"></a><span data-ttu-id="33941-116">Alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="33941-116">Register an app</span></span>
<span data-ttu-id="33941-117">Először hozzon létre egy alkalmazást, az a [App regisztrációs portál](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy kövesse az alábbi [részletes lépéseket](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="33941-117">First, create an app in the [App Registration Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="33941-118">Győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="33941-118">Make sure to:</span></span>

* <span data-ttu-id="33941-119">Adja hozzá a **webes** platform az alkalmazásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="33941-119">Add the **Web** platform for your app.</span></span>
* <span data-ttu-id="33941-120">Adja meg a megfelelő **átirányítási URI-**.</span><span class="sxs-lookup"><span data-stu-id="33941-120">Enter the correct **Redirect URI**.</span></span> <span data-ttu-id="33941-121">Ez a minta alapértelmezés szerint `https://localhost:44326/`.</span><span class="sxs-lookup"><span data-stu-id="33941-121">The default for this sample is `https://localhost:44326/`.</span></span>
* <span data-ttu-id="33941-122">Hagyja a **Implicit Flow engedélyezése** engedélyezve jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="33941-122">Leave the **Allow Implicit Flow** checkbox enabled.</span></span> 

<span data-ttu-id="33941-123">Másolja le a **Alkalmazásazonosító** , amely hozzá van rendelve az alkalmazáshoz, hamarosan kell azt.</span><span class="sxs-lookup"><span data-stu-id="33941-123">Copy down the **Application ID** that is assigned to your app, you'll need it shortly.</span></span> 

## <a name="install-adaljs"></a><span data-ttu-id="33941-124">Adal.js telepítése</span><span class="sxs-lookup"><span data-stu-id="33941-124">Install adal.js</span></span>
<span data-ttu-id="33941-125">Indítsa el, navigáljon a letöltött projektre, és telepítse a adal.js.</span><span class="sxs-lookup"><span data-stu-id="33941-125">To start, navigate to project you downloaded and install adal.js.</span></span>  <span data-ttu-id="33941-126">Ha rendelkezik [bower](http://bower.io/) telepítve, ugyanúgy futtathatja ezt a parancsot.</span><span class="sxs-lookup"><span data-stu-id="33941-126">If you have [bower](http://bower.io/) installed, you can just run this command.</span></span>  <span data-ttu-id="33941-127">Semmilyen függőségi verzió eltérést csak adja meg az újabb verziót.</span><span class="sxs-lookup"><span data-stu-id="33941-127">For any dependency version mismatches, just choose the higher version.</span></span>

```
bower install adal-angular#experimental
```

<span data-ttu-id="33941-128">Azt is megteheti, manuálisan letöltheti [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) és [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).</span><span class="sxs-lookup"><span data-stu-id="33941-128">Alternatively, you can manually download [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) and [adal-angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).</span></span>  <span data-ttu-id="33941-129">Mindkét fájlok hozzáadása a `app/lib/adal-angular-experimental/dist` mappában található a `TodoSPA` projekt.</span><span class="sxs-lookup"><span data-stu-id="33941-129">Add both files to the `app/lib/adal-angular-experimental/dist` directory of the `TodoSPA` project.</span></span>

<span data-ttu-id="33941-130">Most nyissa meg a projektet a Visual Studio, és a fő lapján törzs végén adal.js betöltése:</span><span class="sxs-lookup"><span data-stu-id="33941-130">Now open the project in Visual Studio, and load adal.js at the end of the main page's body:</span></span>

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## <a name="set-up-the-rest-api"></a><span data-ttu-id="33941-131">A REST API beállítása</span><span class="sxs-lookup"><span data-stu-id="33941-131">Set up the REST API</span></span>
<span data-ttu-id="33941-132">Míg beállítás folyamatban van, folytassuk a háttérrendszer REST API-n működő.</span><span class="sxs-lookup"><span data-stu-id="33941-132">While we're setting things up, let's get the backend REST API working.</span></span>  <span data-ttu-id="33941-133">A projekt gyökérkönyvtárában nyissa meg a `web.config` , és cserélje le a `audience` érték.</span><span class="sxs-lookup"><span data-stu-id="33941-133">In the root of the project, open `web.config` and replace the `audience` value.</span></span>  <span data-ttu-id="33941-134">A REST API-t használja ezt az értéket érvényesítse a szögben kifejezett alkalmazást, az AJAX-kérelmek megkapja.</span><span class="sxs-lookup"><span data-stu-id="33941-134">The REST API will use this value to validate tokens it receives from the Angular app on AJAX requests.</span></span>

```xml
<!--web.config-->

...

    <appSettings>
        <add key="ida:Audience" value="[Your-application-id]" />
    </appSettings>

...
```

<span data-ttu-id="33941-135">Ez egy, a REST API működése megvitatása fogjuk folyamatosan.</span><span class="sxs-lookup"><span data-stu-id="33941-135">That's all the time we're going to spend discussing how the REST API works.</span></span>  <span data-ttu-id="33941-136">Nyugodtan poke, a kódban, de ha szeretné megtudni a további információk védelme webes API-kat az Azure ad-vel, tekintse meg [Ez a cikk](active-directory-v2-devquickstarts-dotnet-api.md).</span><span class="sxs-lookup"><span data-stu-id="33941-136">Feel free to poke around in the code, but if you want to learn more about securing web APIs with Azure AD, check out [this article](active-directory-v2-devquickstarts-dotnet-api.md).</span></span> 

## <a name="sign-users-in"></a><span data-ttu-id="33941-137">A felhasználók beléptetése</span><span class="sxs-lookup"><span data-stu-id="33941-137">Sign users in</span></span>
<span data-ttu-id="33941-138">Identitás kódírást idő.</span><span class="sxs-lookup"><span data-stu-id="33941-138">Time to write some identity code.</span></span>  <span data-ttu-id="33941-139">Észrevette, hogy már adott adal.js egy AngularJS szolgáltatót tartalmaz, amely lehetőségben szépen szögben kifejezett útválasztási mechanizmusokat.</span><span class="sxs-lookup"><span data-stu-id="33941-139">You might have already noticed that adal.js contains an AngularJS provider, which plays nicely with Angular routing mechanisms.</span></span>  <span data-ttu-id="33941-140">Indítsa el az alkalmazás az adal modul hozzáadásával:</span><span class="sxs-lookup"><span data-stu-id="33941-140">Start by adding the adal module to the app:</span></span>

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

<span data-ttu-id="33941-141">Most töltse a `adalProvider` az alkalmazás azonosítójú:</span><span class="sxs-lookup"><span data-stu-id="33941-141">You can now initialize the `adalProvider` with your Application ID:</span></span>

```js
// app/scripts/app.js

...

adalProvider.init({

        // Use this value for the public instance of Azure AD
        instance: 'https://login.microsoftonline.com/', 

        // The 'common' endpoint is used for multi-tenant applications like this one
        tenant: 'common',

        // Your application id from the registration portal
        clientId: '<Your-application-id>',

        // If you're using IE, uncommment this line - the default HTML5 sessionStorage does not work for localhost.
        //cacheLocation: 'localStorage',

    }, $httpProvider);
```

<span data-ttu-id="33941-142">Nagyszerű, adal.js már az alkalmazás biztonságos és felhasználói bejelentkezéshez szükséges összes adatot.</span><span class="sxs-lookup"><span data-stu-id="33941-142">Great, now adal.js has all the information it needs to secure your app and sign users in.</span></span>  <span data-ttu-id="33941-143">Jelentkezzen be az alkalmazás az adott útvonal kényszerítéséhez tart egy kódsort:</span><span class="sxs-lookup"><span data-stu-id="33941-143">To force sign in for a particular route in the app, all it takes is one line of code:</span></span>

```js
// app/scripts/app.js

...

}).when("/TodoList", {
    controller: "todoListCtrl",
    templateUrl: "/static/views/TodoList.html",
    requireADLogin: true, // Ensures that the user must be logged in to access the route
})

...
```

<span data-ttu-id="33941-144">Most amikor a felhasználó rákattint a `TodoList` hivatkozásra a adal.js automatikusan átirányítja az Azure AD-hez bejelentkezési szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="33941-144">Now when a user clicks the `TodoList` link, adal.js will automatically redirect to Azure AD for sign-in if necessary.</span></span>  <span data-ttu-id="33941-145">Be- és kijelentkezési kérések, a vezérlők adal.js hívja közvetlenül is küldhet:</span><span class="sxs-lookup"><span data-stu-id="33941-145">You can also explicitly send sign-in and sign-out requests by invoking adal.js in your controllers:</span></span>

```js
// app/scripts/homeCtrl.js

angular.module('todoApp')
// Load adal.js the same way for use in controllers and views   
.controller('homeCtrl', ['$scope', 'adalAuthenticationService','$location', function ($scope, adalService, $location) {
    $scope.login = function () {

        // Redirect the user to sign in
        adalService.login();

    };
    $scope.logout = function () {

        // Redirect the user to log out    
        adalService.logOut();

    };
...
```

## <a name="display-user-info"></a><span data-ttu-id="33941-146">Felhasználói adatok megjelenítése</span><span class="sxs-lookup"><span data-stu-id="33941-146">Display user info</span></span>
<span data-ttu-id="33941-147">Most, hogy a felhasználó jelentkezett be, valószínűleg szüksége a bejelentkezett felhasználók hitelesítési adatait az alkalmazás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="33941-147">Now that the user is signed in, you'll probably need to access the signed-in user's authentication data in your application.</span></span>  <span data-ttu-id="33941-148">Adal.js elérhetővé teszi ezeket az adatokat a a `userInfo` objektum.</span><span class="sxs-lookup"><span data-stu-id="33941-148">Adal.js exposes this information for you in the `userInfo` object.</span></span>  <span data-ttu-id="33941-149">Az objektumot a nézet megnyitásához először adja hozzá adal.js a legfelső szintű vezérlő hatókörébe, a megfelelő:</span><span class="sxs-lookup"><span data-stu-id="33941-149">To access this object in a view, first add adal.js to the root scope of the corresponding controller:</span></span>

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

<span data-ttu-id="33941-150">Ezt követően közvetlenül meg lehet oldani a `userInfo` objektumot a nézet:</span><span class="sxs-lookup"><span data-stu-id="33941-150">Then you can directly address the `userInfo` object in your view:</span></span> 

```html
<!--app/views/UserData.html-->

...

    <!--Get the user's profile information from the ADAL userInfo object-->
    <tr ng-repeat="(key, value) in userInfo.profile">
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
...
```

<span data-ttu-id="33941-151">Használhatja a `userInfo` objektumot határozza meg, ha a felhasználó bejelentkezett-e.</span><span class="sxs-lookup"><span data-stu-id="33941-151">You can also use the `userInfo` object to determine if the user is signed in or not.</span></span>

```html
<!--index.html-->

...

    <!--Use the ADAL userInfo object to show the right login/logout button-->
    <ul class="nav navbar-nav navbar-right">
        <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
        <li><a class="btn btn-link" ng-hide="userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    </ul>
...
```

## <a name="call-the-rest-api"></a><span data-ttu-id="33941-152">A REST API hívása</span><span class="sxs-lookup"><span data-stu-id="33941-152">Call the REST API</span></span>
<span data-ttu-id="33941-153">Végül néhány jogkivonatok lekérésére, és az REST API létrehozása, olvasása, frissítése és törölheti a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="33941-153">Finally, it's time to get some tokens and call the REST API to create, read, update, and delete tasks.</span></span>  <span data-ttu-id="33941-154">Jól kitalálni a Mi?</span><span class="sxs-lookup"><span data-stu-id="33941-154">Well guess what?</span></span>  <span data-ttu-id="33941-155">Ehhez nincs *egy dolog*.</span><span class="sxs-lookup"><span data-stu-id="33941-155">You don't have to do *a thing*.</span></span>  <span data-ttu-id="33941-156">Adal.js automatikusan kezeli, gyorsítótárazás, és a jogkivonatok frissítése.</span><span class="sxs-lookup"><span data-stu-id="33941-156">Adal.js will automatically take care of getting, caching, and refreshing tokens.</span></span>  <span data-ttu-id="33941-157">Azt is kezeli a jogkivonatok csatolása a REST API-t küldünk kimenő AJAX-kérelmek.</span><span class="sxs-lookup"><span data-stu-id="33941-157">It will also take care of attaching those tokens to outgoing AJAX requests that you send to the REST API.</span></span>  

<span data-ttu-id="33941-158">Pontosan hogyan működik ez?</span><span class="sxs-lookup"><span data-stu-id="33941-158">How exactly does this work?</span></span> <span data-ttu-id="33941-159">Az összes köszönhetően a Bűvös van [AngularJS elfogókat](https://docs.angularjs.org/api/ng/service/$http), amely lehetővé teszi, hogy a kimenő és bejövő HTTP-üzenetek átalakítására adal.js.</span><span class="sxs-lookup"><span data-stu-id="33941-159">It's all thanks to the magic of [AngularJS interceptors](https://docs.angularjs.org/api/ng/service/$http), which allows adal.js to transform outgoing and incoming http messages.</span></span>  <span data-ttu-id="33941-160">Ezenkívül adal.js feltételezi, hogy minden kérést küldeni ugyanabban a tartományban, az ablak jogkivonatok szánt alkalmazás azonosítója megegyezik az AngularJS alkalmazást kell használnia.</span><span class="sxs-lookup"><span data-stu-id="33941-160">Furthermore, adal.js assumes that any requests send to the same domain as the window should use tokens intended for the same Application ID as the AngularJS app.</span></span>  <span data-ttu-id="33941-161">Ezért a mindkét szögben kifejezett alkalmazás és a NodeJS REST API azonos Alkalmazásazonosító használtuk.</span><span class="sxs-lookup"><span data-stu-id="33941-161">This is why we used the same Application ID in both the Angular app and in the NodeJS REST API.</span></span>  <span data-ttu-id="33941-162">Természetesen bírálja felül ezt a viselkedést, és kérje meg a jogkivonatok lekérésére más REST API-kat, ha szükséges – adal.js, de a egyszerű forgatókönyv az alapértelmezett beállításokat fog tenni.</span><span class="sxs-lookup"><span data-stu-id="33941-162">Of course, you can override this behavior and tell adal.js to get tokens for other REST APIs if necessary - but for this simple scenario the defaults will do.</span></span>

<span data-ttu-id="33941-163">Íme egy kódrészletet, amely azt mutatja, milyen egyszerűen azt tulajdonosi jogkivonatok kérések küldése az Azure AD:</span><span class="sxs-lookup"><span data-stu-id="33941-163">Here's a snippet that shows how easy it is to send requests with bearer tokens from Azure AD:</span></span>

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

<span data-ttu-id="33941-164">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="33941-164">Congratulations!</span></span>  <span data-ttu-id="33941-165">Az Azure AD integrált egylapos alkalmazás most már befejeződött.</span><span class="sxs-lookup"><span data-stu-id="33941-165">Your Azure AD integrated single page app is now complete.</span></span>  <span data-ttu-id="33941-166">Lépjen tovább, meghajolni igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="33941-166">Go ahead, take a bow.</span></span>  <span data-ttu-id="33941-167">Képes hitelesíti a felhasználókat, biztonságosan hívható meg a háttérrendszer REST API használatával OpenID Connect, és a felhasználó alapszintű adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="33941-167">It can authenticate users, securely call its backend REST API using OpenID Connect, and get basic information about the user.</span></span>  <span data-ttu-id="33941-168">Alapesetben minden személyes Microsoft-Account vagy az Azure AD egy munkahelyi vagy iskolai fiókkal rendelkező felhasználó támogatja.</span><span class="sxs-lookup"><span data-stu-id="33941-168">Out of the box, it supports any user with a personal Microsoft Account or a work/school account from Azure AD.</span></span>  <span data-ttu-id="33941-169">Futtassa az alkalmazást, és a böngészőben navigáljon `https://localhost:44326/`.</span><span class="sxs-lookup"><span data-stu-id="33941-169">Run the app, and in a browser navigate to `https://localhost:44326/`.</span></span>  <span data-ttu-id="33941-170">Jelentkezzen be személyes Microsoft-fiókkal vagy a munkahelyi vagy iskolai fiókkal.</span><span class="sxs-lookup"><span data-stu-id="33941-170">Sign in using either a personal Microsoft account or a work/school account.</span></span>  <span data-ttu-id="33941-171">Feladatok hozzáadása a felhasználói feladatlistában, és jelentkezzen ki.</span><span class="sxs-lookup"><span data-stu-id="33941-171">Add tasks to the user's to-do list, and sign out.</span></span>  <span data-ttu-id="33941-172">Próbáljon meg, a más típusú fiókot bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="33941-172">Try signing in with the other type of account.</span></span> <span data-ttu-id="33941-173">Ha az Azure AD-bérlő létrehozása a munkahelyi vagy iskolai felhasználók kell [beszerzéséről egy itt](active-directory-howto-tenant.md) (szabad).</span><span class="sxs-lookup"><span data-stu-id="33941-173">If you need an Azure AD tenant to create work/school users, [learn how to get one here](active-directory-howto-tenant.md) (it's free).</span></span>

<span data-ttu-id="33941-174">A folytatáshoz a v2.0-végpontra megismerését head biztonsági a [v2.0 – útmutató fejlesztőknek](active-directory-appmodel-v2-overview.md).</span><span class="sxs-lookup"><span data-stu-id="33941-174">To continue learning about the v2.0 endpoint, head back to our [v2.0 developer guide](active-directory-appmodel-v2-overview.md).</span></span>  <span data-ttu-id="33941-175">További forrásokért tekintse meg:</span><span class="sxs-lookup"><span data-stu-id="33941-175">For additional resources, check out:</span></span>

* [<span data-ttu-id="33941-176">Azure-minták a Githubon >></span><span class="sxs-lookup"><span data-stu-id="33941-176">Azure-Samples on GitHub >></span></span>](https://github.com/Azure-Samples)
* [<span data-ttu-id="33941-177">Az Azure AD a veremtúlcsordulás >></span><span class="sxs-lookup"><span data-stu-id="33941-177">Azure AD on Stack Overflow >></span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)
* <span data-ttu-id="33941-178">Az Azure AD-dokumentációja [Azure.com webhelyre >>](https://azure.microsoft.com/documentation/services/active-directory/)</span><span class="sxs-lookup"><span data-stu-id="33941-178">Azure AD documentation on [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)</span></span>

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="33941-179">Biztonsági frissítések termékeinkhez</span><span class="sxs-lookup"><span data-stu-id="33941-179">Get security updates for our products</span></span>
<span data-ttu-id="33941-180">Javasoljuk, hogy kérjen értesítést a bekövetkező biztonsági incidensekről. Látogasson el [erre a lapra](https://technet.microsoft.com/security/dd252948), és fizessen elő a biztonsági tanácsadói riasztásokra.</span><span class="sxs-lookup"><span data-stu-id="33941-180">We encourage you to get notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing to Security Advisory Alerts.</span></span>

