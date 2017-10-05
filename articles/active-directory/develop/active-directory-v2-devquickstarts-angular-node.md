---
title: "Az Azure AD v2.0 NodeJS AngularJS egylapos alkalmazások első lépések |} Microsoft Docs"
description: "Hogyan hozhat létre, hogy mindkét személyes Microsoft-fiókkal rendelkező felhasználók bejelentkezésekor szögben kifejezett JS egylapos alkalmazások és a munkahelyi vagy iskolai fiókját."
services: active-directory
documentationcenter: 
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: d286aa33-8a94-452f-beb7-ddc6c6daa5c8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/23/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 0e90171afd9c4c782fbb18375ab2d147497ef442
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-sign-in-to-an-angularjs-single-page-app---nodejs"></a><span data-ttu-id="abb60-103">Bejelentkezés hozzáadása egy AngularJS egylapos alkalmazás - NodeJS</span><span class="sxs-lookup"><span data-stu-id="abb60-103">Add sign-in to an AngularJS single page app - NodeJS</span></span>
<span data-ttu-id="abb60-104">Ebben a cikkben fel kell venni jelentkezzen be az alkalmazás bekapcsolja Microsoft-fiókok AngularJS alkalmazásokhoz az Azure Active Directory v2.0-végponttól használatával.</span><span class="sxs-lookup"><span data-stu-id="abb60-104">In this article we'll add sign in with Microsoft powered accounts to an AngularJS app using the Azure Active Directory v2.0 endpoint.</span></span> <span data-ttu-id="abb60-105">a v2.0-végpontra lehetővé teszik egy egyetlen integrációt elvégzéséhez az alkalmazáson belüli és a személyes és munkahelyi vagy iskolai fiókkal rendelkező felhasználók hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="abb60-105">the v2.0 endpoint enable you to perform a single integration in your app and authenticate users with both personal and work/school accounts.</span></span>

<span data-ttu-id="abb60-106">Ez a minta egy egyszerű Tennivalólista egylapos alkalmazást, amely tárolja a feladatokat az olyan háttér REST API-t NodeJS nyelven írt, és az Azure AD OAuth tulajdonosi jogkivonatok használatával biztonságossá.</span><span class="sxs-lookup"><span data-stu-id="abb60-106">This sample is a simple To-Do List single page app that stores tasks in a backend REST API, written in NodeJS and secured using OAuth bearer tokens from Azure AD.</span></span>  <span data-ttu-id="abb60-107">Az AngularJS alkalmazás fog használni a nyílt forráskódú JavaScript hitelesítési kódtár [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) a teljes bejelentkezési folyamathoz, és a REST API hívásának jogkivonatainak szerezni.</span><span class="sxs-lookup"><span data-stu-id="abb60-107">The AngularJS app will use our open source JavaScript authentication library [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) to handle the entire sign in process and acquire tokens for calling the REST API.</span></span>  <span data-ttu-id="abb60-108">Ugyanilyen mintájú alkalmazható felé történő hitelesítésre más REST API-k, például a [Microsoft Graph](https://graph.microsoft.com) vagy az Azure Resource Manager API-kat.</span><span class="sxs-lookup"><span data-stu-id="abb60-108">The same pattern can be applied to authenticate to other REST APIs, like the [Microsoft Graph](https://graph.microsoft.com) or the Azure Resource Manager APIs.</span></span>

> [!NOTE]
> <span data-ttu-id="abb60-109">Nem minden Azure Active Directory forgatókönyvek és funkciók támogatják a v2.0-végponttól.</span><span class="sxs-lookup"><span data-stu-id="abb60-109">Not all Azure Active Directory scenarios & features are supported by the v2.0 endpoint.</span></span>  <span data-ttu-id="abb60-110">Annak meghatározásához, ha a v2.0-végponttal kell használnia, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="abb60-110">To determine if you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="download"></a><span data-ttu-id="abb60-111">Letöltés</span><span class="sxs-lookup"><span data-stu-id="abb60-111">Download</span></span>
<span data-ttu-id="abb60-112">A kezdéshez kell letöltése és telepítése [node.js](https://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="abb60-112">To get started, you'll need to download & install [node.js](https://nodejs.org).</span></span>  <span data-ttu-id="abb60-113">Majd átmásolhatja vagy [letöltése](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/skeleton.zip) egy üres alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="abb60-113">Then you can clone or [download](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/skeleton.zip) a skeleton app:</span></span>

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS.git
```

<span data-ttu-id="abb60-114">Az üres alkalmazás egyszerű AngularJS alkalmazások bolierplate kódot tartalmaz, de hiányzik az identitás-kapcsolódó darab mindegyikét.</span><span class="sxs-lookup"><span data-stu-id="abb60-114">The skeleton app includes all the boilerplate code for a simple AngularJS app, but is missing all of the identity-related pieces.</span></span>  <span data-ttu-id="abb60-115">Ha nem szeretné követéséhez, hanem klónozhat vagy [letöltése](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/complete.zip) az elkészült mintát.</span><span class="sxs-lookup"><span data-stu-id="abb60-115">If you don't want to follow along, you can instead clone or [download](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/complete.zip) the completed sample.</span></span>

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-NodeJS.git
```

## <a name="register-an-app"></a><span data-ttu-id="abb60-116">Alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="abb60-116">Register an app</span></span>
<span data-ttu-id="abb60-117">Először hozzon létre egy alkalmazást, az a [App regisztrációs portál](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy kövesse az alábbi [részletes lépéseket](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="abb60-117">First, create an app in the [App Registration Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="abb60-118">Győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="abb60-118">Make sure to:</span></span>

* <span data-ttu-id="abb60-119">Adja hozzá a **webes** platform az alkalmazásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="abb60-119">Add the **Web** platform for your app.</span></span>
* <span data-ttu-id="abb60-120">Adja meg a megfelelő **átirányítási URI-**.</span><span class="sxs-lookup"><span data-stu-id="abb60-120">Enter the correct **Redirect URI**.</span></span> <span data-ttu-id="abb60-121">Ez a minta alapértelmezés szerint `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="abb60-121">The default for this sample is `http://localhost:8080`.</span></span>
* <span data-ttu-id="abb60-122">Hagyja a **Implicit Flow engedélyezése** engedélyezve jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="abb60-122">Leave the **Allow Implicit Flow** checkbox enabled.</span></span> 

<span data-ttu-id="abb60-123">Másolja le a **Alkalmazásazonosító** , amely hozzá van rendelve az alkalmazáshoz, hamarosan kell azt.</span><span class="sxs-lookup"><span data-stu-id="abb60-123">Copy down the **Application ID** that is assigned to your app, you'll need it shortly.</span></span> 

## <a name="install-adaljs"></a><span data-ttu-id="abb60-124">Adal.js telepítése</span><span class="sxs-lookup"><span data-stu-id="abb60-124">Install adal.js</span></span>
<span data-ttu-id="abb60-125">Indítsa el, navigáljon a letöltött projektre, és telepítse a adal.js.</span><span class="sxs-lookup"><span data-stu-id="abb60-125">To start, navigate to project you downloaded and install adal.js.</span></span>  <span data-ttu-id="abb60-126">Ha rendelkezik [bower](http://bower.io/) telepítve, ugyanúgy futtathatja ezt a parancsot.</span><span class="sxs-lookup"><span data-stu-id="abb60-126">If you have [bower](http://bower.io/) installed, you can just run this command.</span></span>  <span data-ttu-id="abb60-127">Semmilyen függőségi verzió eltérést csak adja meg az újabb verziót.</span><span class="sxs-lookup"><span data-stu-id="abb60-127">For any dependency version mismatches, just choose the higher version.</span></span>

```
bower install adal-angular#experimental
```

<span data-ttu-id="abb60-128">Azt is megteheti, manuálisan letöltheti [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) és [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).</span><span class="sxs-lookup"><span data-stu-id="abb60-128">Alternatively, you can manually download [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) and [adal-angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).</span></span>  <span data-ttu-id="abb60-129">Mindkét fájlok hozzáadása a `app/lib/adal-angular-experimental/dist` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="abb60-129">Add both files to the `app/lib/adal-angular-experimental/dist` directory.</span></span>

<span data-ttu-id="abb60-130">Most a kedvenc szövegszerkesztőben nyissa meg a projektet, és a lap törzsében végén adal.js betöltése:</span><span class="sxs-lookup"><span data-stu-id="abb60-130">Now open the project in your favorite text editor, and load adal.js at the end of the page body:</span></span>

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## <a name="set-up-the-rest-api"></a><span data-ttu-id="abb60-131">A REST API beállítása</span><span class="sxs-lookup"><span data-stu-id="abb60-131">Set up the REST API</span></span>
<span data-ttu-id="abb60-132">Során beállítás folyamatban van, lehetővé teszi, hogy a háttér REST API működéséhez.</span><span class="sxs-lookup"><span data-stu-id="abb60-132">While we're setting things up, lets get the backend REST API working.</span></span>  <span data-ttu-id="abb60-133">Egy parancssori ablakot, telepítse a szükséges csomagokat futtatásával (gondoskodjon arról, hogy a projekt legfelső szintű könyvtárában található):</span><span class="sxs-lookup"><span data-stu-id="abb60-133">In a command prompt, install all the necessary packages by running (make sure you're in the top-level directory of the project):</span></span>

```
npm install
```

<span data-ttu-id="abb60-134">Most nyissa meg a `config.js` , és cserélje le a `audience` érték:</span><span class="sxs-lookup"><span data-stu-id="abb60-134">Now open `config.js` and replace the `audience` value:</span></span>

```js
exports.creds = {

     // TODO: Replace this value with the Application ID from the registration portal
     audience: '<Your-application-id>',

     ...
}
```

<span data-ttu-id="abb60-135">A REST API-t használja ezt az értéket érvényesítse a szögben kifejezett alkalmazást, az AJAX-kérelmek megkapja.</span><span class="sxs-lookup"><span data-stu-id="abb60-135">The REST API will use this value to validate tokens it receives from the Angular app on AJAX requests.</span></span>  <span data-ttu-id="abb60-136">Vegye figyelembe, hogy az egyszerű REST API tárolja az adatokat a memóriában - így minden alkalommal, amikor a kiszolgáló leállítására, elveszíti az összes korábban létrehozott feladatok.</span><span class="sxs-lookup"><span data-stu-id="abb60-136">Note that this simple REST API stores data in-memory - so each time to stop the server, you will lose all previously created tasks.</span></span>

<span data-ttu-id="abb60-137">Ez egy, a REST API működése megvitatása fogjuk folyamatosan.</span><span class="sxs-lookup"><span data-stu-id="abb60-137">That's all the time we're going to spend discussing how the REST API works.</span></span>  <span data-ttu-id="abb60-138">Nyugodtan poke, a kódban, de ha szeretné megtudni a további információk védelme webes API-kat az Azure ad-vel, tekintse meg [Ez a cikk](active-directory-v2-devquickstarts-node-api.md).</span><span class="sxs-lookup"><span data-stu-id="abb60-138">Feel free to poke around in the code, but if you want to learn more about securing web APIs with Azure AD, check out [this article](active-directory-v2-devquickstarts-node-api.md).</span></span> 

## <a name="sign-users-in"></a><span data-ttu-id="abb60-139">A felhasználók beléptetése</span><span class="sxs-lookup"><span data-stu-id="abb60-139">Sign users in</span></span>
<span data-ttu-id="abb60-140">Identitás kódírást idő.</span><span class="sxs-lookup"><span data-stu-id="abb60-140">Time to write some identity code.</span></span>  <span data-ttu-id="abb60-141">Észrevette, hogy már adott adal.js egy AngularJS szolgáltatót tartalmaz, amely lehetőségben szépen szögben kifejezett útválasztási mechanizmusokat.</span><span class="sxs-lookup"><span data-stu-id="abb60-141">You might have already noticed that adal.js contains an AngularJS provider, which plays nicely with Angular routing mechanisms.</span></span>  <span data-ttu-id="abb60-142">Indítsa el az alkalmazás az adal modul hozzáadásával:</span><span class="sxs-lookup"><span data-stu-id="abb60-142">Start by adding the adal module to the app:</span></span>

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

<span data-ttu-id="abb60-143">Most töltse a `adalProvider` az alkalmazás azonosítójú:</span><span class="sxs-lookup"><span data-stu-id="abb60-143">You can now initialize the `adalProvider` with your Application ID:</span></span>

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

<span data-ttu-id="abb60-144">Nagyszerű, adal.js már az alkalmazás biztonságos és felhasználói bejelentkezéshez szükséges összes adatot.</span><span class="sxs-lookup"><span data-stu-id="abb60-144">Great, now adal.js has all the information it needs to secure your app and sign users in.</span></span>  <span data-ttu-id="abb60-145">Jelentkezzen be az alkalmazás az adott útvonal kényszerítéséhez tart egy kódsort:</span><span class="sxs-lookup"><span data-stu-id="abb60-145">To force sign in for a particular route in the app, all it takes is one line of code:</span></span>

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

<span data-ttu-id="abb60-146">Most amikor a felhasználó rákattint a `TodoList` hivatkozásra a adal.js automatikusan átirányítja az Azure AD-hez bejelentkezési szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="abb60-146">Now when a user clicks the `TodoList` link, adal.js will automatically redirect to Azure AD for sign-in if necessary.</span></span>  <span data-ttu-id="abb60-147">Be- és kijelentkezési kérések, a vezérlők adal.js hívja közvetlenül is küldhet:</span><span class="sxs-lookup"><span data-stu-id="abb60-147">You can also explicitly send sign-in and sign-out requests by invoking adal.js in your controllers:</span></span>

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

## <a name="display-user-info"></a><span data-ttu-id="abb60-148">Felhasználói adatok megjelenítése</span><span class="sxs-lookup"><span data-stu-id="abb60-148">Display user info</span></span>
<span data-ttu-id="abb60-149">Most, hogy a felhasználó jelentkezett be, valószínűleg szüksége a bejelentkezett felhasználók hitelesítési adatait az alkalmazás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="abb60-149">Now that the user is signed in, you'll probably need to access the signed-in user's authentication data in your application.</span></span>  <span data-ttu-id="abb60-150">Adal.js elérhetővé teszi ezeket az adatokat a a `userInfo` objektum.</span><span class="sxs-lookup"><span data-stu-id="abb60-150">Adal.js exposes this information for you in the `userInfo` object.</span></span>  <span data-ttu-id="abb60-151">Az objektumot a nézet megnyitásához először adja hozzá adal.js a legfelső szintű vezérlő hatókörébe, a megfelelő:</span><span class="sxs-lookup"><span data-stu-id="abb60-151">To access this object in a view, first add adal.js to the root scope of the corresponding controller:</span></span>

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

<span data-ttu-id="abb60-152">Ezt követően közvetlenül meg lehet oldani a `userInfo` objektumot a nézet:</span><span class="sxs-lookup"><span data-stu-id="abb60-152">Then you can directly address the `userInfo` object in your view:</span></span> 

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

<span data-ttu-id="abb60-153">Használhatja a `userInfo` objektumot határozza meg, ha a felhasználó bejelentkezett-e.</span><span class="sxs-lookup"><span data-stu-id="abb60-153">You can also use the `userInfo` object to determine if the user is signed in or not.</span></span>

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

## <a name="call-the-rest-api"></a><span data-ttu-id="abb60-154">A REST API hívása</span><span class="sxs-lookup"><span data-stu-id="abb60-154">Call the REST API</span></span>
<span data-ttu-id="abb60-155">Végül néhány jogkivonatok lekérésére, és az REST API létrehozása, olvasása, frissítése és törölheti a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="abb60-155">Finally, it's time to get some tokens and call the REST API to create, read, update, and delete tasks.</span></span>  <span data-ttu-id="abb60-156">Jól kitalálni a Mi?</span><span class="sxs-lookup"><span data-stu-id="abb60-156">Well guess what?</span></span>  <span data-ttu-id="abb60-157">Ehhez nincs *egy dolog*.</span><span class="sxs-lookup"><span data-stu-id="abb60-157">You don't have to do *a thing*.</span></span>  <span data-ttu-id="abb60-158">Adal.js automatikusan kezeli, gyorsítótárazás, és a jogkivonatok frissítése.</span><span class="sxs-lookup"><span data-stu-id="abb60-158">Adal.js will automatically take care of getting, caching, and refreshing tokens.</span></span>  <span data-ttu-id="abb60-159">Azt is kezeli a jogkivonatok csatolása a REST API-t küldünk kimenő AJAX-kérelmek.</span><span class="sxs-lookup"><span data-stu-id="abb60-159">It will also take care of attaching those tokens to outgoing AJAX requests that you send to the REST API.</span></span>  

<span data-ttu-id="abb60-160">Pontosan hogyan működik ez?</span><span class="sxs-lookup"><span data-stu-id="abb60-160">How exactly does this work?</span></span> <span data-ttu-id="abb60-161">Az összes köszönhetően a Bűvös van [AngularJS elfogókat](https://docs.angularjs.org/api/ng/service/$http), amely lehetővé teszi, hogy a kimenő és bejövő HTTP-üzenetek átalakítására adal.js.</span><span class="sxs-lookup"><span data-stu-id="abb60-161">It's all thanks to the magic of [AngularJS interceptors](https://docs.angularjs.org/api/ng/service/$http), which allows adal.js to transform outgoing and incoming http messages.</span></span>  <span data-ttu-id="abb60-162">Ezenkívül adal.js feltételezi, hogy minden kérést küldeni ugyanabban a tartományban, az ablak jogkivonatok szánt alkalmazás azonosítója megegyezik az AngularJS alkalmazást kell használnia.</span><span class="sxs-lookup"><span data-stu-id="abb60-162">Furthermore, adal.js assumes that any requests send to the same domain as the window should use tokens intended for the same Application ID as the AngularJS app.</span></span>  <span data-ttu-id="abb60-163">Ezért a mindkét szögben kifejezett alkalmazás és a NodeJS REST API azonos Alkalmazásazonosító használtuk.</span><span class="sxs-lookup"><span data-stu-id="abb60-163">This is why we used the same Application ID in both the Angular app and in the NodeJS REST API.</span></span>  <span data-ttu-id="abb60-164">Természetesen bírálja felül ezt a viselkedést, és kérje meg a jogkivonatok lekérésére más REST API-kat, ha szükséges – adal.js, de a egyszerű forgatókönyv az alapértelmezett beállításokat fog tenni.</span><span class="sxs-lookup"><span data-stu-id="abb60-164">Of course, you can override this behavior and tell adal.js to get tokens for other REST APIs if necessary - but for this simple scenario the defaults will do.</span></span>

<span data-ttu-id="abb60-165">Íme egy kódrészletet, amely azt mutatja, milyen egyszerűen azt tulajdonosi jogkivonatok kérések küldése az Azure AD:</span><span class="sxs-lookup"><span data-stu-id="abb60-165">Here's a snippet that shows how easy it is to send requests with bearer tokens from Azure AD:</span></span>

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

<span data-ttu-id="abb60-166">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="abb60-166">Congratulations!</span></span>  <span data-ttu-id="abb60-167">Az Azure AD integrált egylapos alkalmazás most már befejeződött.</span><span class="sxs-lookup"><span data-stu-id="abb60-167">Your Azure AD integrated single page app is now complete.</span></span>  <span data-ttu-id="abb60-168">Lépjen tovább, meghajolni igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="abb60-168">Go ahead, take a bow.</span></span>  <span data-ttu-id="abb60-169">Képes hitelesíti a felhasználókat, biztonságosan hívható meg a háttérrendszer REST API használatával OpenID Connect, és a felhasználó alapszintű adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="abb60-169">It can authenticate users, securely call its backend REST API using OpenID Connect, and get basic information about the user.</span></span>  <span data-ttu-id="abb60-170">Alapesetben minden személyes Microsoft-Account vagy az Azure AD egy munkahelyi vagy iskolai fiókkal rendelkező felhasználó támogatja.</span><span class="sxs-lookup"><span data-stu-id="abb60-170">Out of the box, it supports any user with a personal Microsoft Account or a work/school account from Azure AD.</span></span>  <span data-ttu-id="abb60-171">Az alkalmazás adjon meg egy try futtatásával:</span><span class="sxs-lookup"><span data-stu-id="abb60-171">Give the app a try by running:</span></span>

```
node server.js
```

<span data-ttu-id="abb60-172">A böngészőben navigáljon `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="abb60-172">In a browser navigate to `http://localhost:8080`.</span></span>  <span data-ttu-id="abb60-173">Jelentkezzen be személyes Microsoft-fiókkal vagy a munkahelyi vagy iskolai fiókkal.</span><span class="sxs-lookup"><span data-stu-id="abb60-173">Sign in using either a personal Microsoft account or a work/school account.</span></span>  <span data-ttu-id="abb60-174">Feladatok hozzáadása a felhasználói feladatlistában, és jelentkezzen ki.</span><span class="sxs-lookup"><span data-stu-id="abb60-174">Add tasks to the user's to-do list, and sign out.</span></span>  <span data-ttu-id="abb60-175">Próbáljon meg, a más típusú fiókot bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="abb60-175">Try signing in with the other type of account.</span></span> <span data-ttu-id="abb60-176">Ha az Azure AD-bérlő létrehozása a munkahelyi vagy iskolai felhasználók kell [beszerzéséről egy itt](active-directory-howto-tenant.md) (szabad).</span><span class="sxs-lookup"><span data-stu-id="abb60-176">If you need an Azure AD tenant to create work/school users, [learn how to get one here](active-directory-howto-tenant.md) (it's free).</span></span>

<span data-ttu-id="abb60-177">És folytathatja az a a v2.0-végpontra, központi vissza a [v2.0 – útmutató fejlesztőknek](active-directory-appmodel-v2-overview.md).</span><span class="sxs-lookup"><span data-stu-id="abb60-177">To continue learning about the the v2.0 endpoint, head back to our [v2.0 developer guide](active-directory-appmodel-v2-overview.md).</span></span>  <span data-ttu-id="abb60-178">További forrásokért tekintse meg:</span><span class="sxs-lookup"><span data-stu-id="abb60-178">For additional resources, check out:</span></span>

* [<span data-ttu-id="abb60-179">Azure-minták a Githubon >></span><span class="sxs-lookup"><span data-stu-id="abb60-179">Azure-Samples on GitHub >></span></span>](https://github.com/Azure-Samples)
* [<span data-ttu-id="abb60-180">Az Azure AD a veremtúlcsordulás >></span><span class="sxs-lookup"><span data-stu-id="abb60-180">Azure AD on Stack Overflow >></span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)
* <span data-ttu-id="abb60-181">Az Azure AD-dokumentációja [Azure.com webhelyre >>](https://azure.microsoft.com/documentation/services/active-directory/)</span><span class="sxs-lookup"><span data-stu-id="abb60-181">Azure AD documentation on [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)</span></span>

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="abb60-182">Biztonsági frissítések termékeinkhez</span><span class="sxs-lookup"><span data-stu-id="abb60-182">Get security updates for our products</span></span>
<span data-ttu-id="abb60-183">Javasoljuk, hogy kérjen értesítést a bekövetkező biztonsági incidensekről. Látogasson el [erre a lapra](https://technet.microsoft.com/security/dd252948), és fizessen elő a biztonsági tanácsadói riasztásokra.</span><span class="sxs-lookup"><span data-stu-id="abb60-183">We encourage you to get notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing to Security Advisory Alerts.</span></span>

