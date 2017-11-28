---
title: "aaaAzure AD v2.0 .NET AngularJS egylapos alkalmazások első lépések |} Microsoft Docs"
description: "Hogyan toobuild, hogy a felhasználók a személyes Microsoft-szal szögben kifejezett JS egylapos alkalmazások fiókok és a munkahelyi vagy iskolai fiókjait."
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
ms.openlocfilehash: bd3fc8dce91eb0bedcbfed47a9b3ef52c5568c6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooan-angularjs-single-page-app---net"></a><span data-ttu-id="7bc53-103">Bejelentkezési tooan AngularJS egylapos alkalmazás - .NET hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7bc53-103">Add sign-in tooan AngularJS single page app - .NET</span></span>
<span data-ttu-id="7bc53-104">Ebben a cikkben fel kell venni, jelentkezzen be az alkalmazás bekapcsolja Microsoft fiókok tooan AngularJS használó alkalmazás hello Azure Active Directory v2.0-végponttól.</span><span class="sxs-lookup"><span data-stu-id="7bc53-104">In this article we'll add sign in with Microsoft powered accounts tooan AngularJS app using hello Azure Active Directory v2.0 endpoint.</span></span>  <span data-ttu-id="7bc53-105">hello v2.0-végponttól tooperform egy egyetlen integrációt az alkalmazás lehetővé teszi, és a személyes és munkahelyi vagy iskolai fiókkal rendelkező felhasználók hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="7bc53-105">hello v2.0 endpoint enables you tooperform a single integration in your app and authenticate users with both personal and work/school accounts.</span></span>

<span data-ttu-id="7bc53-106">Ez a minta egy egyszerű Tennivalólista egylapos alkalmazást, amely tárolja a feladatokat az olyan háttér REST API-t hello .NET 4.5 MVC keretrendszer használatával írt, és az Azure AD OAuth tulajdonosi jogkivonatok használatával biztonságossá.</span><span class="sxs-lookup"><span data-stu-id="7bc53-106">This sample is a simple To-Do List single page app that stores tasks in a backend REST API, written using hello .NET 4.5 MVC framework and secured using OAuth bearer tokens from Azure AD.</span></span>  <span data-ttu-id="7bc53-107">hello AngularJS alkalmazás fogja használni a nyílt forráskódú JavaScript hitelesítési kódtár [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) toohandle hello teljes bejelentkezési folyamathoz, és beszerezni a jogkivonatokat a hívó hello REST API-t.</span><span class="sxs-lookup"><span data-stu-id="7bc53-107">hello AngularJS app will use our open source JavaScript authentication library [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) toohandle hello entire sign in process and acquire tokens for calling hello REST API.</span></span>  <span data-ttu-id="7bc53-108">hello ugyanilyen mintájú lehet alkalmazott tooauthenticate tooother REST API-k, például a hello [Microsoft Graph](https://graph.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="7bc53-108">hello same pattern can be applied tooauthenticate tooother REST APIs, like hello [Microsoft Graph](https://graph.microsoft.com).</span></span>

> [!NOTE]
> <span data-ttu-id="7bc53-109">Nem minden Azure Active Directory forgatókönyvek és funkciók támogatják hello v2.0-végponttól.</span><span class="sxs-lookup"><span data-stu-id="7bc53-109">Not all Azure Active Directory scenarios & features are supported by hello v2.0 endpoint.</span></span>  <span data-ttu-id="7bc53-110">toodetermine használatát hello v2.0-végpontra, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="7bc53-110">toodetermine if you should use hello v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="download"></a><span data-ttu-id="7bc53-111">Letöltés</span><span class="sxs-lookup"><span data-stu-id="7bc53-111">Download</span></span>
<span data-ttu-id="7bc53-112">elindult, tooget lesz szüksége toodownload & telepítse a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7bc53-112">tooget started, you'll need toodownload & install Visual Studio.</span></span>  <span data-ttu-id="7bc53-113">Majd átmásolhatja vagy [letöltése](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) egy üres alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="7bc53-113">Then you can clone or [download](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) a skeleton app:</span></span>

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet.git
```

<span data-ttu-id="7bc53-114">hello üres alkalmazás egy egyszerű AngularJS alkalmazás összes hello bolierplate kódot tartalmaz, de összes hello identitás kapcsolatos eleme hiányzik.</span><span class="sxs-lookup"><span data-stu-id="7bc53-114">hello skeleton app includes all hello boilerplate code for a simple AngularJS app, but is missing all of hello identity-related pieces.</span></span>  <span data-ttu-id="7bc53-115">Ha nem szeretné mentén toofollow, hanem klónozhat vagy [letöltése](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/complete.zip) befejeződött hello minta.</span><span class="sxs-lookup"><span data-stu-id="7bc53-115">If you don't want toofollow along, you can instead clone or [download](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/complete.zip) hello completed sample.</span></span>

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-DotNet.git
```

## <a name="register-an-app"></a><span data-ttu-id="7bc53-116">Alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="7bc53-116">Register an app</span></span>
<span data-ttu-id="7bc53-117">Először hozzon létre egy alkalmazást hello [App regisztrációs portál](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy kövesse az alábbi [részletes lépéseket](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="7bc53-117">First, create an app in hello [App Registration Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow these [detailed steps](active-directory-v2-app-registration.md).</span></span>  <span data-ttu-id="7bc53-118">Győződjön meg arról, hogy:</span><span class="sxs-lookup"><span data-stu-id="7bc53-118">Make sure to:</span></span>

* <span data-ttu-id="7bc53-119">Adja hozzá a hello **webes** platform az alkalmazásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="7bc53-119">Add hello **Web** platform for your app.</span></span>
* <span data-ttu-id="7bc53-120">Adja meg a megfelelő hello **átirányítási URI-**.</span><span class="sxs-lookup"><span data-stu-id="7bc53-120">Enter hello correct **Redirect URI**.</span></span> <span data-ttu-id="7bc53-121">Ez a minta hello alapértelmezett értéke `https://localhost:44326/`.</span><span class="sxs-lookup"><span data-stu-id="7bc53-121">hello default for this sample is `https://localhost:44326/`.</span></span>
* <span data-ttu-id="7bc53-122">Hagyja hello **Implicit Flow engedélyezése** engedélyezve jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="7bc53-122">Leave hello **Allow Implicit Flow** checkbox enabled.</span></span> 

<span data-ttu-id="7bc53-123">Másolja le hello **Alkalmazásazonosító** hozzárendelt tooyour app, hamarosan lesz kell.</span><span class="sxs-lookup"><span data-stu-id="7bc53-123">Copy down hello **Application ID** that is assigned tooyour app, you'll need it shortly.</span></span> 

## <a name="install-adaljs"></a><span data-ttu-id="7bc53-124">Adal.js telepítése</span><span class="sxs-lookup"><span data-stu-id="7bc53-124">Install adal.js</span></span>
<span data-ttu-id="7bc53-125">toostart, keresse meg a letöltött tooproject, és telepítse a adal.js.</span><span class="sxs-lookup"><span data-stu-id="7bc53-125">toostart, navigate tooproject you downloaded and install adal.js.</span></span>  <span data-ttu-id="7bc53-126">Ha rendelkezik [bower](http://bower.io/) telepítve, ugyanúgy futtathatja ezt a parancsot.</span><span class="sxs-lookup"><span data-stu-id="7bc53-126">If you have [bower](http://bower.io/) installed, you can just run this command.</span></span>  <span data-ttu-id="7bc53-127">A semmilyen függőségi verzió eltérést válassza a hello újabb verziójú.</span><span class="sxs-lookup"><span data-stu-id="7bc53-127">For any dependency version mismatches, just choose hello higher version.</span></span>

```
bower install adal-angular#experimental
```

<span data-ttu-id="7bc53-128">Azt is megteheti, manuálisan letöltheti [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) és [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).</span><span class="sxs-lookup"><span data-stu-id="7bc53-128">Alternatively, you can manually download [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) and [adal-angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).</span></span>  <span data-ttu-id="7bc53-129">Adja hozzá a mindkét fájlok toohello `app/lib/adal-angular-experimental/dist` mappában található hello `TodoSPA` projekt.</span><span class="sxs-lookup"><span data-stu-id="7bc53-129">Add both files toohello `app/lib/adal-angular-experimental/dist` directory of hello `TodoSPA` project.</span></span>

<span data-ttu-id="7bc53-130">Most nyissa meg a hello projektet a Visual Studio és a betöltési adal.js hello fő lapján törzs hello végén:</span><span class="sxs-lookup"><span data-stu-id="7bc53-130">Now open hello project in Visual Studio, and load adal.js at hello end of hello main page's body:</span></span>

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## <a name="set-up-hello-rest-api"></a><span data-ttu-id="7bc53-131">REST API hello beállítása</span><span class="sxs-lookup"><span data-stu-id="7bc53-131">Set up hello REST API</span></span>
<span data-ttu-id="7bc53-132">Míg beállítás folyamatban van, folytassuk hello háttér REST API-n működő.</span><span class="sxs-lookup"><span data-stu-id="7bc53-132">While we're setting things up, let's get hello backend REST API working.</span></span>  <span data-ttu-id="7bc53-133">Hello projekt gyökerében hello, nyissa meg `web.config` , és cserélje le a hello `audience` érték.</span><span class="sxs-lookup"><span data-stu-id="7bc53-133">In hello root of hello project, open `web.config` and replace hello `audience` value.</span></span>  <span data-ttu-id="7bc53-134">hello REST API-t fogja használni a toovalidate értékelemek hello szögben kifejezett alkalmazás AJAX-kérelmek megkapja.</span><span class="sxs-lookup"><span data-stu-id="7bc53-134">hello REST API will use this value toovalidate tokens it receives from hello Angular app on AJAX requests.</span></span>

```xml
<!--web.config-->

...

    <appSettings>
        <add key="ida:Audience" value="[Your-application-id]" />
    </appSettings>

...
```

<span data-ttu-id="7bc53-135">Ez minden fogjuk megvitatása hello REST API működése toospend hello idő.</span><span class="sxs-lookup"><span data-stu-id="7bc53-135">That's all hello time we're going toospend discussing how hello REST API works.</span></span>  <span data-ttu-id="7bc53-136">Érzi, hogy szabad toopoke hello kódban, de ha azt szeretné, hogy a webes API-k és az Azure AD biztosításával kapcsolatos további toolearn, tekintse meg [Ez a cikk](active-directory-v2-devquickstarts-dotnet-api.md).</span><span class="sxs-lookup"><span data-stu-id="7bc53-136">Feel free toopoke around in hello code, but if you want toolearn more about securing web APIs with Azure AD, check out [this article](active-directory-v2-devquickstarts-dotnet-api.md).</span></span> 

## <a name="sign-users-in"></a><span data-ttu-id="7bc53-137">A felhasználók beléptetése</span><span class="sxs-lookup"><span data-stu-id="7bc53-137">Sign users in</span></span>
<span data-ttu-id="7bc53-138">Idő toowrite néhány identitás kódot.</span><span class="sxs-lookup"><span data-stu-id="7bc53-138">Time toowrite some identity code.</span></span>  <span data-ttu-id="7bc53-139">Észrevette, hogy már adott adal.js egy AngularJS szolgáltatót tartalmaz, amely lehetőségben szépen szögben kifejezett útválasztási mechanizmusokat.</span><span class="sxs-lookup"><span data-stu-id="7bc53-139">You might have already noticed that adal.js contains an AngularJS provider, which plays nicely with Angular routing mechanisms.</span></span>  <span data-ttu-id="7bc53-140">Először vegyen fel hello adal modul toohello alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="7bc53-140">Start by adding hello adal module toohello app:</span></span>

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

<span data-ttu-id="7bc53-141">Most már tudja inicializálni a hello `adalProvider` az alkalmazás azonosítójú:</span><span class="sxs-lookup"><span data-stu-id="7bc53-141">You can now initialize hello `adalProvider` with your Application ID:</span></span>

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

<span data-ttu-id="7bc53-142">Nagyszerű, már adal.js hello kapcsolatos összes információ szükséges toosecure alkalmazás és a bejelentkezési felhasználói számára.</span><span class="sxs-lookup"><span data-stu-id="7bc53-142">Great, now adal.js has all hello information it needs toosecure your app and sign users in.</span></span>  <span data-ttu-id="7bc53-143">egy adott útvonalon hello alkalmazásban, az összes szükséges bejelentkezés tooforce kód egy sor:</span><span class="sxs-lookup"><span data-stu-id="7bc53-143">tooforce sign in for a particular route in hello app, all it takes is one line of code:</span></span>

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

<span data-ttu-id="7bc53-144">Most amikor a felhasználó rákattint hello `TodoList` hivatkozást, a adal.js automatikusan átirányítja a tooAzure AD-hez, jelentkezzen be szükséges.</span><span class="sxs-lookup"><span data-stu-id="7bc53-144">Now when a user clicks hello `TodoList` link, adal.js will automatically redirect tooAzure AD for sign-in if necessary.</span></span>  <span data-ttu-id="7bc53-145">Be- és kijelentkezési kérések, a vezérlők adal.js hívja közvetlenül is küldhet:</span><span class="sxs-lookup"><span data-stu-id="7bc53-145">You can also explicitly send sign-in and sign-out requests by invoking adal.js in your controllers:</span></span>

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

## <a name="display-user-info"></a><span data-ttu-id="7bc53-146">Felhasználói adatok megjelenítése</span><span class="sxs-lookup"><span data-stu-id="7bc53-146">Display user info</span></span>
<span data-ttu-id="7bc53-147">Most, hogy hello van bejelentkezett felhasználó, az alkalmazás valószínűleg tooaccess hello bejelentkezett felhasználó hitelesítési adat lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="7bc53-147">Now that hello user is signed in, you'll probably need tooaccess hello signed-in user's authentication data in your application.</span></span>  <span data-ttu-id="7bc53-148">Adal.js elérhetővé teszi ezeket az adatokat a hello `userInfo` objektum.</span><span class="sxs-lookup"><span data-stu-id="7bc53-148">Adal.js exposes this information for you in hello `userInfo` object.</span></span>  <span data-ttu-id="7bc53-149">tooaccess az objektumot a nézet, először adja hozzá a adal.js toohello legfelső szintű vezérlő hatókörébe, hello megfelelő:</span><span class="sxs-lookup"><span data-stu-id="7bc53-149">tooaccess this object in a view, first add adal.js toohello root scope of hello corresponding controller:</span></span>

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

<span data-ttu-id="7bc53-150">Ezt követően közvetlenül meg lehet oldani hello `userInfo` objektumot a nézet:</span><span class="sxs-lookup"><span data-stu-id="7bc53-150">Then you can directly address hello `userInfo` object in your view:</span></span> 

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

<span data-ttu-id="7bc53-151">Is használhatja a hello `userInfo` toodetermine objektumot, ha hello felhasználó van bejelentkezve.</span><span class="sxs-lookup"><span data-stu-id="7bc53-151">You can also use hello `userInfo` object toodetermine if hello user is signed in or not.</span></span>

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

## <a name="call-hello-rest-api"></a><span data-ttu-id="7bc53-152">Hello REST API hívása</span><span class="sxs-lookup"><span data-stu-id="7bc53-152">Call hello REST API</span></span>
<span data-ttu-id="7bc53-153">Végezetül fontos idő tooget néhány jogkivonatokat és hívás hello REST API toocreate, olvasása, frissítése és törölheti a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="7bc53-153">Finally, it's time tooget some tokens and call hello REST API toocreate, read, update, and delete tasks.</span></span>  <span data-ttu-id="7bc53-154">Jól kitalálni a Mi?</span><span class="sxs-lookup"><span data-stu-id="7bc53-154">Well guess what?</span></span>  <span data-ttu-id="7bc53-155">Nincs toodo *egy dolog*.</span><span class="sxs-lookup"><span data-stu-id="7bc53-155">You don't have toodo *a thing*.</span></span>  <span data-ttu-id="7bc53-156">Adal.js automatikusan kezeli, gyorsítótárazás, és a jogkivonatok frissítése.</span><span class="sxs-lookup"><span data-stu-id="7bc53-156">Adal.js will automatically take care of getting, caching, and refreshing tokens.</span></span>  <span data-ttu-id="7bc53-157">Azt is kezeli a jogkivonatok toooutgoing AJAX-kérelmek, hogy küldjön toohello REST API csatolni.</span><span class="sxs-lookup"><span data-stu-id="7bc53-157">It will also take care of attaching those tokens toooutgoing AJAX requests that you send toohello REST API.</span></span>  

<span data-ttu-id="7bc53-158">Pontosan hogyan működik ez?</span><span class="sxs-lookup"><span data-stu-id="7bc53-158">How exactly does this work?</span></span> <span data-ttu-id="7bc53-159">Köszönjük, hogy az összes toohello magic [AngularJS elfogókat](https://docs.angularjs.org/api/ng/service/$http), amely lehetővé teszi, hogy adal.js tootransform kimenő és bejövő HTTP-üzenetek.</span><span class="sxs-lookup"><span data-stu-id="7bc53-159">It's all thanks toohello magic of [AngularJS interceptors](https://docs.angularjs.org/api/ng/service/$http), which allows adal.js tootransform outgoing and incoming http messages.</span></span>  <span data-ttu-id="7bc53-160">Ezenkívül adal.js feltételezi, hogy minden kérést küldeni toohello ugyanabban a tartományban, hello ablak használandó jogkivonatokat szánt hello ugyanazon alkalmazás azonosítója, az AngularJS app hello.</span><span class="sxs-lookup"><span data-stu-id="7bc53-160">Furthermore, adal.js assumes that any requests send toohello same domain as hello window should use tokens intended for hello same Application ID as hello AngularJS app.</span></span>  <span data-ttu-id="7bc53-161">Ezért hello használtuk azonos Alkalmazásazonosító mindkét hello szögben kifejezett alkalmazásban és a hello NodeJS REST API-t.</span><span class="sxs-lookup"><span data-stu-id="7bc53-161">This is why we used hello same Application ID in both hello Angular app and in hello NodeJS REST API.</span></span>  <span data-ttu-id="7bc53-162">Természetesen bírálja felül ezt a viselkedést, és közölje adal.js tooget jogkivonatok más REST API-kat, ha szükséges – a, de ez a forgatókönyv egyszerű hello alapértelmezett értéke lesz tegye.</span><span class="sxs-lookup"><span data-stu-id="7bc53-162">Of course, you can override this behavior and tell adal.js tooget tokens for other REST APIs if necessary - but for this simple scenario hello defaults will do.</span></span>

<span data-ttu-id="7bc53-163">Íme egy kódrészletet, amely azt mutatja, milyen egyszerűen azt toosend tulajdonosi jogkivonatok az Azure AD rendelkező kérelmek esetében:</span><span class="sxs-lookup"><span data-stu-id="7bc53-163">Here's a snippet that shows how easy it is toosend requests with bearer tokens from Azure AD:</span></span>

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

<span data-ttu-id="7bc53-164">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="7bc53-164">Congratulations!</span></span>  <span data-ttu-id="7bc53-165">Az Azure AD integrált egylapos alkalmazás most már befejeződött.</span><span class="sxs-lookup"><span data-stu-id="7bc53-165">Your Azure AD integrated single page app is now complete.</span></span>  <span data-ttu-id="7bc53-166">Lépjen tovább, meghajolni igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="7bc53-166">Go ahead, take a bow.</span></span>  <span data-ttu-id="7bc53-167">Azt is hitelesíti a felhasználókat, biztonságosan hívható meg a háttérrendszer REST API használatával OpenID Connect és alapszintű hello felhasználó adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="7bc53-167">It can authenticate users, securely call its backend REST API using OpenID Connect, and get basic information about hello user.</span></span>  <span data-ttu-id="7bc53-168">Hello mezőbe kívül bármely felhasználó vagy személyes Microsoft Account, vagy a munkahelyi vagy iskolai fiókkal az Azure AD támogatja.</span><span class="sxs-lookup"><span data-stu-id="7bc53-168">Out of hello box, it supports any user with a personal Microsoft Account or a work/school account from Azure AD.</span></span>  <span data-ttu-id="7bc53-169">Hello alkalmazás futtatását, és a böngészőben lépjen túl`https://localhost:44326/`.</span><span class="sxs-lookup"><span data-stu-id="7bc53-169">Run hello app, and in a browser navigate too`https://localhost:44326/`.</span></span>  <span data-ttu-id="7bc53-170">Jelentkezzen be személyes Microsoft-fiókkal vagy a munkahelyi vagy iskolai fiókkal.</span><span class="sxs-lookup"><span data-stu-id="7bc53-170">Sign in using either a personal Microsoft account or a work/school account.</span></span>  <span data-ttu-id="7bc53-171">Adja hozzá a feladatok toohello felhasználói feladatlistában, és jelentkezzen ki.  Próbálja meg azzal aláíró hello más típusú fiókot.</span><span class="sxs-lookup"><span data-stu-id="7bc53-171">Add tasks toohello user's to-do list, and sign out.  Try signing in with hello other type of account.</span></span> <span data-ttu-id="7bc53-172">Ha az Azure AD bérlő toocreate munkahelyi vagy iskolai felhasználók kell [megtudhatja, hogyan tooget egy itt](active-directory-howto-tenant.md) (szabad).</span><span class="sxs-lookup"><span data-stu-id="7bc53-172">If you need an Azure AD tenant toocreate work/school users, [learn how tooget one here](active-directory-howto-tenant.md) (it's free).</span></span>

<span data-ttu-id="7bc53-173">hello v2.0-végpontra, központi hátsó tooour megtanulni toocontinue [v2.0 – útmutató fejlesztőknek](active-directory-appmodel-v2-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7bc53-173">toocontinue learning about hello v2.0 endpoint, head back tooour [v2.0 developer guide](active-directory-appmodel-v2-overview.md).</span></span>  <span data-ttu-id="7bc53-174">További forrásokért tekintse meg:</span><span class="sxs-lookup"><span data-stu-id="7bc53-174">For additional resources, check out:</span></span>

* [<span data-ttu-id="7bc53-175">Azure-minták a Githubon >></span><span class="sxs-lookup"><span data-stu-id="7bc53-175">Azure-Samples on GitHub >></span></span>](https://github.com/Azure-Samples)
* [<span data-ttu-id="7bc53-176">Az Azure AD a veremtúlcsordulás >></span><span class="sxs-lookup"><span data-stu-id="7bc53-176">Azure AD on Stack Overflow >></span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)
* <span data-ttu-id="7bc53-177">Az Azure AD-dokumentációja [Azure.com webhelyre >>](https://azure.microsoft.com/documentation/services/active-directory/)</span><span class="sxs-lookup"><span data-stu-id="7bc53-177">Azure AD documentation on [Azure.com >>](https://azure.microsoft.com/documentation/services/active-directory/)</span></span>

## <a name="get-security-updates-for-our-products"></a><span data-ttu-id="7bc53-178">Biztonsági frissítések termékeinkhez</span><span class="sxs-lookup"><span data-stu-id="7bc53-178">Get security updates for our products</span></span>
<span data-ttu-id="7bc53-179">Javasoljuk, tooget értesítést a bekövetkező biztonsági incidensekről ellátogatva [ezen a lapon](https://technet.microsoft.com/security/dd252948) és előfizetés tooSecurity tanácsadói riasztásokra.</span><span class="sxs-lookup"><span data-stu-id="7bc53-179">We encourage you tooget notifications of when security incidents occur by visiting [this page](https://technet.microsoft.com/security/dd252948) and subscribing tooSecurity Advisory Alerts.</span></span>

