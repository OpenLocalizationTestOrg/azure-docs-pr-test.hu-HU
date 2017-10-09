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
# <a name="help-secure-angularjs-single-page-apps-by-using-azure-ad"></a>Számítógépek biztonságossá tétele AngularJS egyoldalas alkalmazások az Azure AD segítségével

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure Active Directory (Azure AD) az egyszerű és magától értetődő, a tooadd bejelentkezés, kijelentkezési, és az OAuth biztonságos API tooyour egyoldalas alkalmazások.  Lehetővé teszi, hogy a alkalmazások tooauthenticate felhasználókat a Windows Server Active Directory-fiókba, és minden webes API-t, amely az Azure AD segít megvédeni, például az Office 365 API-k hello vagy hello Azure API felhasználását.

Böngészőjében futó JavaScript-alkalmazások esetében az Azure AD hello biztosít az Active Directory Authentication Library (ADAL), vagy adal.js. hello kizárólagos adal.js célja toomake megkönnyítik az alkalmazás tooget hozzáférési jogkivonatok. toodemonstrate milyen könnyű van, itt azt fogja build egy AngularJS tooDo lista alkalmazás, amely:

* Jeleket hello felhasználói identitás-szolgáltatóként hello Azure AD használatával toohello alkalmazásban.

* Néhány hello felhasználói információkat jelenít meg.
* Biztonságosan hívások alkalmazás tooDo lista API hello Azure ad-tulajdonosi jogkivonatok segítségével.
* Jeleket hello felhasználói hello alkalmazásból.

toobuild hello teljes, működő alkalmazást kell:

1. Regisztrálja az alkalmazást az Azure ad-val.
2. Adal-t telepíti és konfigurálja a hello alkalmazás.
3. ADAL toohelp biztonságos lapok hello alkalmazás használja.

elindult, tooget [hello app vázat letöltése](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) vagy [letöltése befejeződött hello minta](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip). Akkor is, amelyben felhasználók létrehozása és egy alkalmazás regisztrálása az Azure AD-bérlő kell. Ha még nem rendelkezik a bérlő [megtudhatja, hogyan egy tooget](active-directory-howto-tenant.md).

## <a name="step-1-register-hello-directorysearcher-application"></a>1. lépés: Hello DirectorySearcher alkalmazás regisztrálása
tooenable a app tooauthenticate felhasználók és a get-jogkivonatokat, először tooregister azt az Azure ad bérlői:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Ha be van jelentkezve toomultiple könyvtárak, szükség lehet a tooensure megtekintett hello megfelelő könyvtárban. toodo hello felső sávon, kattintson a fiókját. A hello **Directory** menüben válassza ki a kívánt tooregister hello Azure AD-bérlő az alkalmazást.
3. Kattintson a **több szolgáltatások** hello bal oldali ablaktáblán, és válassza ki **Azure Active Directory**.
4. Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.
5. Hello utasításokat követve, és hozzon létre egy új webalkalmazás és/vagy webes API:
  * **Név** az alkalmazás toousers ismerteti.
  * **Átirányítási Uri** hello hely toowhich az Azure AD visszaadható jogkivonatok. Ez a minta hello alapértelmezett helye `https://localhost:44326/`.
6. Regisztráció befejezése után az Azure AD egy egyedi alkalmazás azonosítója tooyour app rendeli hozzá.  Ez az érték kell a következő szakaszok hello, ezért másolja hello alkalmazás lapján.
7. Adal.js hello OAuth implicit engedélyezési folyamat toocommunicate használja az Azure ad-val. Az alkalmazás hello implicit engedélyezési folyamat kell engedélyezni:
  1. Kattintson a hello alkalmazást, és válassza ki **Manifest** tooopen hello beágyazott jegyzék szerkesztő.
  2. Keresse meg a hello `oauth2AllowImplicitFlow` tulajdonság. Állítsa be az értékét túl`true`.
  3. Kattintson a **mentése** toosave hello jegyzékben.
8. Az alkalmazás a bérlő jogosultságot adni. Nyissa meg túl**beállítások** > **tulajdonságok** > **szükséges engedélyek**, hello kattintson **engedélyt adjon**hello felső sáv gombjára. Kattintson a **Igen** tooconfirm.

## <a name="step-2-install-adal-and-configure-hello-single-page-app"></a>2. lépés: Telepítse adal-t és hello alkalmazás konfigurálása
Most, hogy az Azure AD-alkalmazás, adal.js telepítse, és az identitás-kapcsolódó kód írása.

### <a name="configure-hello-javascript-client"></a>Hello JavaScript-ügyfél konfigurálása
Kezdje az adal.js toohello TodoSPA projekt hozzáadása hello Csomagkezelő konzol használatával:
  1. Töltse le [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) , és adja hozzá toohello `App/Scripts/` projekt könyvtárában.
  2. Töltse le [adal angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) , és adja hozzá toohello `App/Scripts/` projekt könyvtárában.
  3. Minden parancsprogramhoz hello hello vége előtt betöltése `</body>` a `index.html`:

    ```js
    ...
    <script src="App/Scripts/adal.js"></script>
    <script src="App/Scripts/adal-angular.js"></script>
    ...
    ```

### <a name="configure-hello-back-end-server"></a>Hello háttér-kiszolgáló konfigurálása
Hello egyoldalas alkalmazások háttér-tooDo lista API tooaccept jogkivonatokat hello böngészőből hello háttér kell hello app regisztrációs konfigurációs adatait. Hello TodoSPA projektben nyissa meg `web.config`. Cserélje le a hello értékek hello hello elemeinek `<appSettings>` szakasz tooreflect hello értékek a használt hello Azure-portálon. A kód minden alkalommal ADAL hivatkozik ezeket az értékeket.
  * `ida:Tenant`az Azure AD bérlője – például a contoso.onmicrosoft.com hello tartomány.
  * `ida:Audience`az alkalmazás hello portálról másolt hello ügyfél-azonosító van.

## <a name="step-3-use-adal-toohelp-secure-pages-in-hello-single-page-app"></a>3. lépés: Használja ADAL toohelp biztonságos lapok hello egyoldalas alkalmazásban
Adal.js jól integrálható az AngularJS útvonal és a HTTP-szolgáltatók, az alkalmazás egyoldalas segíthet a biztonságos egyéni nézeteket.

1. A `App/Scripts/app.js`, kapcsolja a hello adal.js modul:

    ```js
    angular.module('todoApp', ['ngRoute','AdalAngular'])
    .config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
     function ($routeProvider, $httpProvider, adalProvider) {
    ...
    ```
2. Inicializálni `adalProvider` használatával hello konfigurációs értékeket az alkalmazás regisztrációja is `App/Scripts/app.js`:

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
3. Biztonságos hello segítségével `TodoList` hello alkalmazásban csak egy kódsort a nézet: `requireADLogin`.

    ```js
    ...
    }).when("/TodoList", {
            controller: "todoListCtrl",
            templateUrl: "/App/Views/TodoList.html",
            requireADLogin: true,
    ...
    ```

## <a name="summary"></a>Összefoglalás
Most már rendelkezik egy biztonságos alkalmazás, amely a felhasználók bejelentkezhetnek és tulajdonosi jogkivonat-védelemmel ellátott kérelmek tooits háttér-API ki. Amikor a felhasználó rákattint hello **TodoList** hivatkozást, a adal.js automatikusan átirányítja a tooAzure AD-hez, jelentkezzen be szükséges. Ezenkívül adal.js automatikusan elvégzi az access token tooany Ajax-kérelmek toohello app háttér küldött.  

hello előző lépésekre hello operációs rendszer minimálisan szükséges toobuild egy alkalmazás adal.js használatával. Azonban néhány egyéb szolgáltatásokat az alkalmazás hasznos információkat:

* tooexplicitly be- és kijelentkezési kérések kiállítása, a tartományvezérlőket, amelyek aktiválják az adal.js funkciók is meghatározhatja.  A `App/Scripts/homeCtrl.js`:

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
* Érdemes lehet toopresent felhasználói adatok hello alkalmazás felhasználói felületén. hello ADAL szolgáltatáshoz már hozzáadtak toohello `userDataCtrl` vezérlő, hogy hozzáférjen a hello `userInfo` hello objektum társított nézet, `App/Views/UserData.html`:

    ```js
    <p>{{userInfo.userName}}</p>
    <p>aud:{{userInfo.profile.aud}}</p>
    <p>iss:{{userInfo.profile.iss}}</p>
    ...
    ```

* Nincsenek számos forgatókönyv, ahol érdemes tooknow Ha hello felhasználó van bejelentkezve. Is használhatja a hello `userInfo` objektum toogather ezt az információt.  Például a `index.html`, megjelenítheti vagy hello **bejelentkezési** vagy **kijelentkezési** gomb a hitelesítési állapot alapján:

    ```js
    <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
    <li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    ```

Az Azure Active Directoryba integrált alkalmazás is hitelesíti a felhasználókat, biztonságosan OAuth 2.0 használatával hívható meg a háttér és alapszintű hello felhasználó adatainak beolvasása. Ha még nem tette meg, most az hello idő toopopulate a bérlő az egyes felhasználók. Futtassa a tooDo lista egyoldalas alkalmazást, és jelentkezzen be valamelyik azoknak a felhasználóknak. Adja hozzá a feladatok toohello felhasználói feladatlistában, jelentkezzen ki, és jelentkezzen be újra a.

Adal.js segítségével könnyen tooincorporate általános identitás-szolgáltatások az alkalmazásba. Ez gondoskodik összes hello dirty munkahelyi meg: gyorsítótár kezelése, OAuth protokoll támogatása, bemutató hello felhasználó egy bejelentkezési felhasználói felületén, lejárt jogkivonatokat, és több frissítése.

Referenciaként befejeződött hello mintát (a konfigurációs értékek nélkül) érhető el a [GitHub](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip).

## <a name="next-steps"></a>Következő lépések
Most már továbbléphet tooadditional forgatókönyvek. Érdemes lehet tootry: [CORS webes API meghívása egy egyoldalas alkalmazásból](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
