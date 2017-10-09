---
title: "aaaAzure AD v2.0 NodeJS AngularJS egylapos alkalmazások első lépések |} Microsoft Docs"
description: "Hogyan toobuild, hogy a felhasználók a személyes Microsoft-szal szögben kifejezett JS egylapos alkalmazások fiókok és a munkahelyi vagy iskolai fiókjait."
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
ms.openlocfilehash: 1ab450caf08ab05fba140b94b1b8de652e99cbc1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooan-angularjs-single-page-app---nodejs"></a><span data-ttu-id="a9ced-103">Bejelentkezési tooan AngularJS egylapos alkalmazás - NodeJS hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a9ced-103">Add sign-in tooan AngularJS single page app - NodeJS</span></span>
<span data-ttu-id="a9ced-104">Ebben a cikkben fel kell venni, jelentkezzen be az alkalmazás bekapcsolja Microsoft fiókok tooan AngularJS használó alkalmazás hello Azure Active Directory v2.0-végponttól.</span><span class="sxs-lookup"><span data-stu-id="a9ced-104">In this article we'll add sign in with Microsoft powered accounts tooan AngularJS app using hello Azure Active Directory v2.0 endpoint.</span></span> <span data-ttu-id="a9ced-105">hello v2.0-végponttól tooperform egy egyetlen integrációt az alkalmazás lehetővé teszi, és a személyes és munkahelyi vagy iskolai fiókkal rendelkező felhasználók hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="a9ced-105">hello v2.0 endpoint enable you tooperform a single integration in your app and authenticate users with both personal and work/school accounts.</span></span>

<span data-ttu-id="a9ced-106">Ez a minta egy egyszerű Tennivalólista egylapos alkalmazást, amely tárolja a feladatokat az olyan háttér REST API-t NodeJS nyelven írt, és az Azure AD OAuth tulajdonosi jogkivonatok használatával biztonságossá.</span><span class="sxs-lookup"><span data-stu-id="a9ced-106">This sample is a simple To-Do List single page app that stores tasks in a backend REST API, written in NodeJS and secured using OAuth bearer tokens from Azure AD.</span></span>  <span data-ttu-id="a9ced-107">hello AngularJS alkalmazás fogja használni a nyílt forráskódú JavaScript hitelesítési kódtár [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) toohandle hello teljes bejelentkezési folyamathoz, és beszerezni a jogkivonatokat a hívó hello REST API-t.</span><span class="sxs-lookup"><span data-stu-id="a9ced-107">hello AngularJS app will use our open source JavaScript authentication library [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) toohandle hello entire sign in process and acquire tokens for calling hello REST API.</span></span>  <span data-ttu-id="a9ced-108">hello ugyanilyen mintájú lehet alkalmazott tooauthenticate tooother REST API-k, például a hello [Microsoft Graph](https://graph.microsoft.com) vagy hello Azure Resource Manager API-k.</span><span class="sxs-lookup"><span data-stu-id="a9ced-108">hello same pattern can be applied tooauthenticate tooother REST APIs, like hello [Microsoft Graph](https://graph.microsoft.com) or hello Azure Resource Manager APIs.</span></span>

> [!NOTE]
> <span data-ttu-id="a9ced-109">Nem minden Azure Active Directory forgatókönyvek és funkciók támogatják hello v2.0-végponttól.</span><span class="sxs-lookup"><span data-stu-id="a9ced-109">Not all Azure Active Directory scenarios & features are supported by hello v2.0 endpoint.</span></span>  <span data-ttu-id="a9ced-110">toodetermine használatát hello v2.0-végpontra, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="a9ced-110">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="download"></a><span data-ttu-id="a9ced-111">Letöltés</span><span class="sxs-lookup"><span data-stu-id="a9ced-111">Download</span></span>
<span data-ttu-id="a9ced-112">tooget elindult, szüksége lesz toodownload & telepítés [node.js](https://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="a9ced-112">tooget started, you'll need toodownload & install [node.js](https://nodejs.org).</span></span>  <span data-ttu-id="a9ced-113">Majd átmásolhatja vagy [letöltése](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/skeleton.zip) egy üres alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="a9ced-113">Then you can clone or [download](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/skeleton.zip) a skeleton app:</span></span>

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS.git
```

<span data-ttu-id="a9ced-114">hello üres alkalmazás egy egyszerű AngularJS alkalmazás összes hello bolierplate kódot tartalmaz, de összes hello identitás kapcsolatos eleme hiányzik.</span><span class="sxs-lookup"><span data-stu-id="a9ced-114">hello skeleton app includes all hello boilerplate code for a simple AngularJS app, but is missing all of hello identity-related pieces.</span></span>  <span data-ttu-id="a9ced-115">Ha nem szeretné mentén toofollow, hanem klónozhat vagy [letöltése](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/complete.zip) befejeződött hello minta.</span><span class="sxs-lookup"><span data-stu-id="a9ced-115">If you don't want toofollow along, you can instead clone or [download](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-NodeJS/archive/complete.zip) hello completed sample.</span></span>

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-NodeJS.git
```

## <a name="register-an-app"></a><span data-ttu-id="a9ced-116">Alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="a9ced-116">Register an app</span></span>
<span data-ttu-id="a9ced-117">Először hozzon létre egy alkalmazást hello [App regisztrációs portál](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy kövesse az alábbi [részletes lépéseket](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="a9ced-117">First, create an app in hello [App Registration Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="a9ced-118">Győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="a9ced-118">Make sure to:</span></span>

* <span data-ttu-id="a9ced-119">Adja hozzá a hello **webes** platform az alkalmazásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="a9ced-119">Add hello **Web** platform for your app.</span></span>
* <span data-ttu-id="a9ced-120">Adja meg a megfelelő hello **átirányítási URI-**.</span><span class="sxs-lookup"><span data-stu-id="a9ced-120">Enter hello correct **Redirect URI**.</span></span> <span data-ttu-id="a9ced-121">Ez a minta hello alapértelmezett értéke `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="a9ced-121">hello default for this sample is `http://localhost:8080`.</span></span>
* <span data-ttu-id="a9ced-122">Hagyja hello **Implicit Flow engedélyezése** engedélyezve jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="a9ced-122">Leave hello **Allow Implicit Flow** checkbox enabled.</span></span> 

<span data-ttu-id="a9ced-123">Másolja le hello **Alkalmazásazonosító** hozzárendelt tooyour app, hamarosan lesz kell.</span><span class="sxs-lookup"><span data-stu-id="a9ced-123">Copy down hello **Application ID** that is assigned tooyour app, you'll need it shortly.</span></span> 

## <a name="install-adaljs"></a><span data-ttu-id="a9ced-124">Adal.js telepítése</span><span class="sxs-lookup"><span data-stu-id="a9ced-124">Install adal.js</span></span>
<span data-ttu-id="a9ced-125">toostart, keresse meg a letöltött tooproject, és telepítse a adal.js.</span><span class="sxs-lookup"><span data-stu-id="a9ced-125">toostart, navigate tooproject you downloaded and install adal.js.</span></span>  <span data-ttu-id="a9ced-126">Ha rendelkezik [bower](http://bower.io/) telepítve, ugyanúgy futtathatja ezt a parancsot.</span><span class="sxs-lookup"><span data-stu-id="a9ced-126">If you have [bower](http://bower.io/) installed, you can just run this command.</span></span>  <span data-ttu-id="a9ced-127">A semmilyen függőségi verzió eltérést válassza a hello újabb verziójú.</span><span class="sxs-lookup"><span data-stu-id="a9ced-127">For any dependency version mismatches, just choose hello higher version.</span></span>

```
bower install adal-angular#experimental
```

<span data-ttu-id="a9ced-128">Azt is megteheti, manuálisan letöltheti [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) és [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).</span><span class="sxs-lookup"><span data-stu-id="a9ced-128">Alternatively, you can manually download [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) and [adal-angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).</span></span>  <span data-ttu-id="a9ced-129">Adja hozzá a mindkét fájlok toohello `app/lib/adal-angular-experimental/dist` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="a9ced-129">Add both files toohello `app/lib/adal-angular-experimental/dist` directory.</span></span>

<span data-ttu-id="a9ced-130">Most nyissa meg hello projekt kedvenc szövegszerkesztőjével, és hello lap törzs hello végén adal.js betöltése:</span><span class="sxs-lookup"><span data-stu-id="a9ced-130">Now open hello project in your favorite text editor, and load adal.js at hello end of hello page body:</span></span>

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## <a name="set-up-hello-rest-api"></a><span data-ttu-id="a9ced-131">REST API hello beállítása</span><span class="sxs-lookup"><span data-stu-id="a9ced-131">Set up hello REST API</span></span>
<span data-ttu-id="a9ced-132">Míg beállítás folyamatban van, lehetővé teszi, hogy get hello háttér REST API-n működő.</span><span class="sxs-lookup"><span data-stu-id="a9ced-132">While we're setting things up, lets get hello backend REST API working.</span></span>  <span data-ttu-id="a9ced-133">Egy parancssori ablakot, telepítse az összes hello szükséges csomag futtatásával (gondoskodjon arról, hogy hello projekt hello legfelső szintű könyvtárban):</span><span class="sxs-lookup"><span data-stu-id="a9ced-133">In a command prompt, install all hello necessary packages by running (make sure you're in hello top-level directory of hello project):</span></span>

```
npm install
```

<span data-ttu-id="a9ced-134">Most nyissa meg a `config.js` , és cserélje le a hello `audience` érték:</span><span class="sxs-lookup"><span data-stu-id="a9ced-134">Now open `config.js` and replace hello `audience` value:</span></span>

```js
exports.creds = {

     // TODO: Replace this value with hello Application ID from hello registration portal
     audience: '<Your-application-id>',

     ...
}
```

<span data-ttu-id="a9ced-135">hello REST API-t fogja használni a toovalidate értékelemek hello szögben kifejezett alkalmazás AJAX-kérelmek megkapja.</span><span class="sxs-lookup"><span data-stu-id="a9ced-135">hello REST API will use this value toovalidate tokens it receives from hello Angular app on AJAX requests.</span></span>  <span data-ttu-id="a9ced-136">Vegye figyelembe, hogy az egyszerű REST API tárolja az adatokat a memóriában - így minden toostop hello kiszolgálót, korábban létrehozott összes feladat megszakad.</span><span class="sxs-lookup"><span data-stu-id="a9ced-136">Note that this simple REST API stores data in-memory - so each time toostop hello server, you will lose all previously created tasks.</span></span>

<span data-ttu-id="a9ced-137">Ez minden fogjuk megvitatása hello REST API működése toospend hello idő.</span><span class="sxs-lookup"><span data-stu-id="a9ced-137">That's all hello time we're going toospend discussing how hello REST API works.</span></span>  <span data-ttu-id="a9ced-138">Érzi, hogy szabad toopoke hello kódban, de ha azt szeretné, hogy a webes API-k és az Azure AD biztosításával kapcsolatos további toolearn, tekintse meg [Ez a cikk](active-directory-v2-devquickstarts-node-api.md).</span><span class="sxs-lookup"><span data-stu-id="a9ced-138">Feel free toopoke around in hello code, but if you want toolearn more about securing web APIs with Azure AD, check out [this article](active-directory-v2-devquickstarts-node-api.md).</span></span> 

## <a name="sign-users-in"></a><span data-ttu-id="a9ced-139">A felhasználók beléptetése</span><span class="sxs-lookup"><span data-stu-id="a9ced-139">Sign users in</span></span>
<span data-ttu-id="a9ced-140">Idő toowrite néhány identitás kódot.</span><span class="sxs-lookup"><span data-stu-id="a9ced-140">Time toowrite some identity code.</span></span>  <span data-ttu-id="a9ced-141">Észrevette, hogy már adott adal.js egy AngularJS szolgáltatót tartalmaz, amely lehetőségben szépen szögben kifejezett útválasztási mechanizmusokat.</span><span class="sxs-lookup"><span data-stu-id="a9ced-141">You might have already noticed that adal.js contains an AngularJS provider, which plays nicely with Angular routing mechanisms.</span></span>  <span data-ttu-id="a9ced-142">Először vegyen fel hello adal modul toohello alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="a9ced-142">Start by adding hello adal module toohello app:</span></span>

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

<span data-ttu-id="a9ced-143">Most már tudja inicializálni a hello `adalProvider` az alkalmazás azonosítójú:</span><span class="sxs-lookup"><span data-stu-id="a9ced-143">You can now initialize hello `adalProvider` with your Application ID:</span></span>

```js
// app/scripts/app.js

...

adalProvider.init({

        // Use this value for hello public instance of Azure AD
        instance: 'https://login.microsoftonline.com/', 

        // hello 'common' endpoint is used for multi-tenant applications like this one
        tenant: 'common',

        // Your application id from hello registration portal
        clientId: '<Your-application-id>',

        // If you're using IE, uncommment this line - hello default HTML5 sessionStorage does not work for localhost.
        //cacheLocation: 'localStorage',

    }, $httpProvider);
```

<span data-ttu-id="a9ced-144">Nagyszerű, már adal.js hello kapcsolatos összes információ szükséges toosecure alkalmazás és a bejelentkezési felhasználói számára.</span><span class="sxs-lookup"><span data-stu-id="a9ced-144">Great, now adal.js has all hello information it needs toosecure your app and sign users in.</span></span>  <span data-ttu-id="a9ced-145">egy adott útvonalon hello alkalmazásban, az összes szükséges bejelentkezés tooforce kód egy sor:</span><span class="sxs-lookup"><span data-stu-id="a9ced-145">tooforce sign in for a particular route in hello app, all it takes is one line of code:</span></span>

```js
// app/scripts/app.js

...

}).when("/TodoList", {
    controller: "todoListCtrl",
    templateUrl: "/static/views/TodoList.html",
    requireADLogin: true, // Ensures that hello user must be logged in tooaccess hello route
})

...
```

<span data-ttu-id="a9ced-146">Most amikor a felhasználó rákattint hello `TodoList` hivatkozást, a adal.js automatikusan átirányítja a tooAzure AD-hez, jelentkezzen be szükséges.</span><span class="sxs-lookup"><span data-stu-id="a9ced-146">Now when a user clicks hello `TodoList` link, adal.js will automatically redirect tooAzure AD for sign-in if necessary.</span></span>  <span data-ttu-id="a9ced-147">Be- és kijelentkezési kérések, a vezérlők adal.js hívja közvetlenül is küldhet:</span><span class="sxs-lookup"><span data-stu-id="a9ced-147">You can also explicitly send sign-in and sign-out requests by invoking adal.js in your controllers:</span></span>

```js
// app/scripts/homeCtrl.js

angular.module('todoApp')
// Load adal.js hello same way for use in controllers and views   
.controller('homeCtrl', ['$scope', 'adalAuthenticationService','$location', function ($scope, adalService, $location) {
    $scope.login = function () {

        // Redirect hello user toosign in
        adalService.login();

    };
    $scope.logout = function () {

        // Redirect hello user toolog out    
        adalService.logOut();

    };
...
```

## <a name="display-user-info"></a><span data-ttu-id="a9ced-148">Felhasználói adatok megjelenítése</span><span class="sxs-lookup"><span data-stu-id="a9ced-148">Display user info</span></span>
<span data-ttu-id="a9ced-149">Most, hogy hello van bejelentkezett felhasználó, az alkalmazás valószínűleg tooaccess hello bejelentkezett felhasználó hitelesítési adat lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="a9ced-149">Now that hello user is signed in, you'll probably need tooaccess hello signed-in user's authentication data in your application.</span></span>  <span data-ttu-id="a9ced-150">Adal.js elérhetővé teszi ezeket az adatokat a hello `userInfo` objektum.</span><span class="sxs-lookup"><span data-stu-id="a9ced-150">Adal.js exposes this information for you in hello `userInfo` object.</span></span>  <span data-ttu-id="a9ced-151">tooaccess az objektumot a nézet, először adja hozzá a adal.js toohello legfelső szintű vezérlő hatókörébe, hello megfelelő:</span><span class="sxs-lookup"><span data-stu-id="a9ced-151">tooaccess this object in a view, first add adal.js toohello root scope of hello corresponding controller:</span></span>

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

<span data-ttu-id="a9ced-152">Ezt követően közvetlenül meg lehet oldani hello `userInfo` objektumot a nézet:</span><span class="sxs-lookup"><span data-stu-id="a9ced-152">Then you can directly address hello `userInfo` object in your view:</span></span> 

```html
<!--app/views/UserData.html-->

...

    <!--Get hello user's profile information from hello ADAL userInfo object-->
    <tr ng-repeat="(key, value) in userInfo.profile">
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
...
```

<span data-ttu-id="a9ced-153">Is használhatja a hello `userInfo` toodetermine objektumot, ha hello felhasználó van bejelentkezve.</span><span class="sxs-lookup"><span data-stu-id="a9ced-153">You can also use hello `userInfo` object toodetermine if hello user is signed in or not.</span></span>

```html
<!--index.html-->

...

    <!--Use hello ADAL userInfo object tooshow hello right login/logout button-->
    <ul class="nav navbar-nav navbar-right">
        <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
        <li><a class="btn btn-link" ng-hide="userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    </ul>
...
```

## <a name="call-hello-rest-api"></a><span data-ttu-id="a9ced-154">Hello REST API hívása</span><span class="sxs-lookup"><span data-stu-id="a9ced-154">Call hello REST API</span></span>
<span data-ttu-id="a9ced-155">Végezetül fontos idő tooget néhány jogkivonatokat és hívás hello REST API toocreate, olvasása, frissítése és törölheti a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="a9ced-155">Finally, it's time tooget some tokens and call hello REST API toocreate, read, update, and delete tasks.</span></span>  <span data-ttu-id="a9ced-156">Jól kitalálni a Mi?</span><span class="sxs-lookup"><span data-stu-id="a9ced-156">Well guess what?</span></span>  <span data-ttu-id="a9ced-157">Nincs toodo *egy dolog*.</span><span class="sxs-lookup"><span data-stu-id="a9ced-157">You don't have toodo *a thing*.</span></span>  <span data-ttu-id="a9ced-158">Adal.js automatikusan kezeli, gyorsítótárazás, és a jogkivonatok frissítése.</span><span class="sxs-lookup"><span data-stu-id="a9ced-158">Adal.js will automatically take care of getting, caching, and refreshing tokens.</span></span>  <span data-ttu-id="a9ced-159">Azt is kezeli a jogkivonatok toooutgoing AJAX-kérelmek, hogy küldjön toohello REST API csatolni.</span><span class="sxs-lookup"><span data-stu-id="a9ced-159">It will also take care of attaching those tokens toooutgoing AJAX requests that you send toohello REST API.</span></span>  

<span data-ttu-id="a9ced-160">Pontosan hogyan működik ez?</span><span class="sxs-lookup"><span data-stu-id="a9ced-160">How exactly does this work?</span></span> <span data-ttu-id="a9ced-161">Köszönjük, hogy az összes toohello magic [AngularJS elfogókat](https://docs.angularjs.org/api/ng/service/$http), amely lehetővé teszi, hogy adal.js tootransform kimenő és bejövő HTTP-üzenetek.</span><span class="sxs-lookup"><span data-stu-id="a9ced-161">It's all thanks toohello magic of [AngularJS interceptors](https://docs.angularjs.org/api/ng/service/$http), which allows adal.js tootransform outgoing and incoming http messages.</span></span>  <span data-ttu-id="a9ced-162">Ezenkívül adal.js feltételezi, hogy minden kérést küldeni toohello ugyanabban a tartományban, hello ablak használandó jogkivonatokat szánt hello ugyanazon alkalmazás azonosítója, az AngularJS app hello.</span><span class="sxs-lookup"><span data-stu-id="a9ced-162">Furthermore, adal.js assumes that any requests send toohello same domain as hello window should use tokens intended for hello same Application ID as hello AngularJS app.</span></span>  <span data-ttu-id="a9ced-163">Ezért hello használtuk azonos Alkalmazásazonosító mindkét hello szögben kifejezett alkalmazásban és a hello NodeJS REST API-t.</span><span class="sxs-lookup"><span data-stu-id="a9ced-163">This is why we used hello same Application ID in both hello Angular app and in hello NodeJS REST API.</span></span>  <span data-ttu-id="a9ced-164">Természetesen bírálja felül ezt a viselkedést, és közölje adal.js tooget jogkivonatok más REST API-kat, ha szükséges – a, de ez a forgatókönyv egyszerű hello alapértelmezett értéke lesz tegye.</span><span class="sxs-lookup"><span data-stu-id="a9ced-164">Of course, you can override this behavior and tell adal.js tooget tokens for other REST APIs if necessary - but for this simple scenario hello defaults will do.</span></span>

<span data-ttu-id="a9ced-165">Íme egy kódrészletet, amely azt mutatja, milyen egyszerűen azt toosend tulajdonosi jogkivonatok az Azure AD rendelkező kérelmek esetében:</span><span class="sxs-lookup"><span data-stu-id="a9ced-165">Here's a snippet that shows how easy it is toosend requests with bearer tokens from Azure AD:</span></span>

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

<span data-ttu-id="a9ced-166">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="a9ced-166">Congratulations!</span></span>  <span data-ttu-id="a9ced-167">Az Azure AD integrált egylapos alkalmazás most már befejeződött.</span><span class="sxs-lookup"><span data-stu-id="a9ced-167">Your Azure AD integrated single page app is now complete.</span></span>  <span data-ttu-id="a9ced-168">Lépjen tovább, meghajolni igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="a9ced-168">Go ahead, take a bow.</span></span>  <span data-ttu-id="a9ced-169">Azt is hitelesíti a felhasználókat, biztonságosan hívható meg a háttérrendszer REST API használatával OpenID Connect és alapszintű hello felhasználó adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="a9ced-169">It can authenticate users, securely call its backend REST API using OpenID Connect, and get basic information about hello user.</span></span>  <span data-ttu-id="a9ced-170">Hello mezőbe kívül bármely felhasználó vagy személyes Microsoft Account, vagy a munkahelyi vagy iskolai fiókkal az Azure AD támogatja.</span><span class="sxs-lookup"><span data-stu-id="a9ced-170">Out of hello box, it supports any user with a personal Microsoft Account or a work/school account from Azure AD.</span></span>  <span data-ttu-id="a9ced-171">Hello alkalmazást adjon meg egy try futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a9ced-171">Give hello app a try by running:</span></span>

```
node server.js
```

<span data-ttu-id="a9ced-172">A böngészőben lépjen túl`http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="a9ced-172">In a browser navigate too`http://localhost:8080`.</span></span>  <span data-ttu-id="a9ced-173">Jelentkezzen be személyes Microsoft-fiókkal vagy a munkahelyi vagy iskolai fiókkal.</span><span class="sxs-lookup"><span data-stu-id="a9ced-173">Sign in using either a personal Microsoft account or a work/school account.</span></span>  <span data-ttu-id="a9ced-174">Adja hozzá a feladatok toohello felhasználói feladatlistában, és jelentkezzen ki.  Próbálja meg azzal aláíró hello más típusú fiókot.</span><span class="sxs-lookup"><span data-stu-id="a9ced-174">Add tasks toohello user's to-do list, and sign out.  Try signing in with hello other type of account.</span></span> <span data-ttu-id="a9ced-175">Ha az Azure AD bérlő toocreate munkahelyi vagy iskolai felhasználók kell [megtudhatja, hogyan tooget egy itt](active-directory-howto-tenant.md) (szabad).</span><span class="sxs-lookup"><span data-stu-id="a9ced-175">If you need an Azure AD tenant toocreate work/school users, [learn how tooget one here](active-directory-howto-tenant.md) (it's free).</span></span>

<span data-ttu-id="a9ced-176">hello megtanulni toocontinue hello v2.0-végpontra, látogasson vissza tooour [v2.0 – útmutató fejlesztőknek](active-directory-appmodel-v2-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a9ced-176">toocontinue learning about hello hello v2.0 endpoint, head back tooour [v2.0 developer guide](active-directory-appmodel-v2-overview.md).</span></span>  <span data-ttu-id="a9ced-177">További forrásokért tekintse meg:</span><span class="sxs-lookup"><span data-stu-id="a9ced-177">For additional resources, check out:</span></span>

* [<span data-ttu-id="a9ced-178">Azure-minták a Githubon >></span><span class="sxs-lookup"><span data-stu-id="a9ced-178">Azure-Samples on GitHub >></span></span>](https://github.com/Azure-Samples)
* [<span data-ttu-id="a9ced-179">Az Azure AD a veremtúlcsordulás >></span><span class="sxs-lookup"><span data-stu-id="a9ced-179">Azure AD on Stack Overflow >></span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)
* <span data-ttu-id="a9ced-180">Az Azure AD-dokumentációja [Azure.com webhelyre >>](https://azure.microsoft.com/documentation/services/active-directory/)</span><span class="sxs-lookup"><span data-stu-id="a9ced-180">Azure AD documentation on [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)</span></span>

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="a9ced-181">Biztonsági frissítések termékeinkhez</span><span class="sxs-lookup"><span data-stu-id="a9ced-181">Get security updates for our products</span></span>
<span data-ttu-id="a9ced-182">Javasoljuk, tooget értesítést a bekövetkező biztonsági incidensekről ellátogatva [ezen a lapon](https://technet.microsoft.com/security/dd252948) és előfizetés tooSecurity tanácsadói riasztásokra.</span><span class="sxs-lookup"><span data-stu-id="a9ced-182">We encourage you tooget notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing tooSecurity Advisory Alerts.</span></span>

