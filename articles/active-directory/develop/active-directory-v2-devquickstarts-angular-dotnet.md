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
# <a name="add-sign-in-tooan-angularjs-single-page-app---net"></a>Bejelentkezési tooan AngularJS egylapos alkalmazás - .NET hozzáadása
Ebben a cikkben fel kell venni, jelentkezzen be az alkalmazás bekapcsolja Microsoft fiókok tooan AngularJS használó alkalmazás hello Azure Active Directory v2.0-végponttól.  hello v2.0-végponttól tooperform egy egyetlen integrációt az alkalmazás lehetővé teszi, és a személyes és munkahelyi vagy iskolai fiókkal rendelkező felhasználók hitelesítéséhez.

Ez a minta egy egyszerű Tennivalólista egylapos alkalmazást, amely tárolja a feladatokat az olyan háttér REST API-t hello .NET 4.5 MVC keretrendszer használatával írt, és az Azure AD OAuth tulajdonosi jogkivonatok használatával biztonságossá.  hello AngularJS alkalmazás fogja használni a nyílt forráskódú JavaScript hitelesítési kódtár [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) toohandle hello teljes bejelentkezési folyamathoz, és beszerezni a jogkivonatokat a hívó hello REST API-t.  hello ugyanilyen mintájú lehet alkalmazott tooauthenticate tooother REST API-k, például a hello [Microsoft Graph](https://graph.microsoft.com).

> [!NOTE]
> Nem minden Azure Active Directory forgatókönyvek és funkciók támogatják hello v2.0-végponttól.  toodetermine használatát hello v2.0-végpontra, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).
> 
> 

## <a name="download"></a>Letöltés
elindult, tooget lesz szüksége toodownload & telepítse a Visual Studio.  Majd átmásolhatja vagy [letöltése](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) egy üres alkalmazást:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet.git
```

hello üres alkalmazás egy egyszerű AngularJS alkalmazás összes hello bolierplate kódot tartalmaz, de összes hello identitás kapcsolatos eleme hiányzik.  Ha nem szeretné mentén toofollow, hanem klónozhat vagy [letöltése](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/complete.zip) befejeződött hello minta.

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-DotNet.git
```

## <a name="register-an-app"></a>Alkalmazás regisztrálása
Először hozzon létre egy alkalmazást hello [App regisztrációs portál](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy kövesse az alábbi [részletes lépéseket](active-directory-v2-app-registration.md).  Győződjön meg arról, hogy:

* Adja hozzá a hello **webes** platform az alkalmazásra vonatkozóan.
* Adja meg a megfelelő hello **átirányítási URI-**. Ez a minta hello alapértelmezett értéke `https://localhost:44326/`.
* Hagyja hello **Implicit Flow engedélyezése** engedélyezve jelölőnégyzetet. 

Másolja le hello **Alkalmazásazonosító** hozzárendelt tooyour app, hamarosan lesz kell. 

## <a name="install-adaljs"></a>Adal.js telepítése
toostart, keresse meg a letöltött tooproject, és telepítse a adal.js.  Ha rendelkezik [bower](http://bower.io/) telepítve, ugyanúgy futtathatja ezt a parancsot.  A semmilyen függőségi verzió eltérést válassza a hello újabb verziójú.

```
bower install adal-angular#experimental
```

Azt is megteheti, manuálisan letöltheti [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) és [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js).  Adja hozzá a mindkét fájlok toohello `app/lib/adal-angular-experimental/dist` mappában található hello `TodoSPA` projekt.

Most nyissa meg a hello projektet a Visual Studio és a betöltési adal.js hello fő lapján törzs hello végén:

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## <a name="set-up-hello-rest-api"></a>REST API hello beállítása
Míg beállítás folyamatban van, folytassuk hello háttér REST API-n működő.  Hello projekt gyökerében hello, nyissa meg `web.config` , és cserélje le a hello `audience` érték.  hello REST API-t fogja használni a toovalidate értékelemek hello szögben kifejezett alkalmazás AJAX-kérelmek megkapja.

```xml
<!--web.config-->

...

    <appSettings>
        <add key="ida:Audience" value="[Your-application-id]" />
    </appSettings>

...
```

Ez minden fogjuk megvitatása hello REST API működése toospend hello idő.  Érzi, hogy szabad toopoke hello kódban, de ha azt szeretné, hogy a webes API-k és az Azure AD biztosításával kapcsolatos további toolearn, tekintse meg [Ez a cikk](active-directory-v2-devquickstarts-dotnet-api.md). 

## <a name="sign-users-in"></a>A felhasználók beléptetése
Idő toowrite néhány identitás kódot.  Észrevette, hogy már adott adal.js egy AngularJS szolgáltatót tartalmaz, amely lehetőségben szépen szögben kifejezett útválasztási mechanizmusokat.  Először vegyen fel hello adal modul toohello alkalmazást:

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

Most már tudja inicializálni a hello `adalProvider` az alkalmazás azonosítójú:

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

Nagyszerű, már adal.js hello kapcsolatos összes információ szükséges toosecure alkalmazás és a bejelentkezési felhasználói számára.  egy adott útvonalon hello alkalmazásban, az összes szükséges bejelentkezés tooforce kód egy sor:

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

Most amikor a felhasználó rákattint hello `TodoList` hivatkozást, a adal.js automatikusan átirányítja a tooAzure AD-hez, jelentkezzen be szükséges.  Be- és kijelentkezési kérések, a vezérlők adal.js hívja közvetlenül is küldhet:

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

## <a name="display-user-info"></a>Felhasználói adatok megjelenítése
Most, hogy hello van bejelentkezett felhasználó, az alkalmazás valószínűleg tooaccess hello bejelentkezett felhasználó hitelesítési adat lesz szüksége.  Adal.js elérhetővé teszi ezeket az adatokat a hello `userInfo` objektum.  tooaccess az objektumot a nézet, először adja hozzá a adal.js toohello legfelső szintű vezérlő hatókörébe, hello megfelelő:

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

Ezt követően közvetlenül meg lehet oldani hello `userInfo` objektumot a nézet: 

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

Is használhatja a hello `userInfo` toodetermine objektumot, ha hello felhasználó van bejelentkezve.

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

## <a name="call-hello-rest-api"></a>Hello REST API hívása
Végezetül fontos idő tooget néhány jogkivonatokat és hívás hello REST API toocreate, olvasása, frissítése és törölheti a feladatokat.  Jól kitalálni a Mi?  Nincs toodo *egy dolog*.  Adal.js automatikusan kezeli, gyorsítótárazás, és a jogkivonatok frissítése.  Azt is kezeli a jogkivonatok toooutgoing AJAX-kérelmek, hogy küldjön toohello REST API csatolni.  

Pontosan hogyan működik ez? Köszönjük, hogy az összes toohello magic [AngularJS elfogókat](https://docs.angularjs.org/api/ng/service/$http), amely lehetővé teszi, hogy adal.js tootransform kimenő és bejövő HTTP-üzenetek.  Ezenkívül adal.js feltételezi, hogy minden kérést küldeni toohello ugyanabban a tartományban, hello ablak használandó jogkivonatokat szánt hello ugyanazon alkalmazás azonosítója, az AngularJS app hello.  Ezért hello használtuk azonos Alkalmazásazonosító mindkét hello szögben kifejezett alkalmazásban és a hello NodeJS REST API-t.  Természetesen bírálja felül ezt a viselkedést, és közölje adal.js tooget jogkivonatok más REST API-kat, ha szükséges – a, de ez a forgatókönyv egyszerű hello alapértelmezett értéke lesz tegye.

Íme egy kódrészletet, amely azt mutatja, milyen egyszerűen azt toosend tulajdonosi jogkivonatok az Azure AD rendelkező kérelmek esetében:

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

Gratulálunk!  Az Azure AD integrált egylapos alkalmazás most már befejeződött.  Lépjen tovább, meghajolni igénybe vehet.  Azt is hitelesíti a felhasználókat, biztonságosan hívható meg a háttérrendszer REST API használatával OpenID Connect és alapszintű hello felhasználó adatainak beolvasása.  Hello mezőbe kívül bármely felhasználó vagy személyes Microsoft Account, vagy a munkahelyi vagy iskolai fiókkal az Azure AD támogatja.  Hello alkalmazás futtatását, és a böngészőben lépjen túl`https://localhost:44326/`.  Jelentkezzen be személyes Microsoft-fiókkal vagy a munkahelyi vagy iskolai fiókkal.  Adja hozzá a feladatok toohello felhasználói feladatlistában, és jelentkezzen ki.  Próbálja meg azzal aláíró hello más típusú fiókot. Ha az Azure AD bérlő toocreate munkahelyi vagy iskolai felhasználók kell [megtudhatja, hogyan tooget egy itt](active-directory-howto-tenant.md) (szabad).

hello v2.0-végpontra, központi hátsó tooour megtanulni toocontinue [v2.0 – útmutató fejlesztőknek](active-directory-appmodel-v2-overview.md).  További forrásokért tekintse meg:

* [Azure-minták a Githubon >>](https://github.com/Azure-Samples)
* [Az Azure AD a veremtúlcsordulás >>](http://stackoverflow.com/questions/tagged/azure-active-directory)
* Az Azure AD-dokumentációja [Azure.com webhelyre >>](https://azure.microsoft.com/documentation/services/active-directory/)

## <a name="get-security-updates-for-our-products"></a>Biztonsági frissítések termékeinkhez
Javasoljuk, tooget értesítést a bekövetkező biztonsági incidensekről ellátogatva [ezen a lapon](https://technet.microsoft.com/security/dd252948) és előfizetés tooSecurity tanácsadói riasztásokra.

