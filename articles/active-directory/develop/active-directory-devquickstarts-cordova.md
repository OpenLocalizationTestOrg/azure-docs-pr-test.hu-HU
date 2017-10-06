---
title: "Első lépések AD Cordova aaaAzure |} Microsoft Docs"
description: "Hogyan toobuild Cordova-alkalmazás, amely integrálható az Azure ad-val bejelentkezési és az Azure AD-védett API OAuth használatával."
services: active-directory
documentationcenter: 
author: vibronet
manager: mbaldwin
editor: 
ms.assetid: b1a8d7bd-7ad6-44d5-8ccb-5255bb623345
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: vittorib
ms.custom: aaddev
ms.openlocfilehash: 573ed638c2180c5231648bcb8c49ceb6f53296f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-ad-with-an-apache-cordova-app"></a>Azure AD integrálása az Apache Cordova-alkalmazás
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Apache Cordova toodevelop HTML5/JavaScript alkalmazások futtatható, mint a teljes körű natív alkalmazások mobileszközökön is használhatja. Az Azure Active Directoryval (Azure AD) a vállalati szintű hitelesítési képességek tooyour Cordova alkalmazások is hozzáadhat.

A Cordova beépülő modul becsomagolja az Azure AD natív SDK-k iOS, Android, Windows áruház és Windows Phone. Hogy a beépülő modul, növelheti az alkalmazás toosupport jelentkezzen be a felhasználók a Windows Server Active Directory-fiókok használatával hozzáférést tooOffice 365 és Azure API-k, és még védelme hívások tooyour saját egyéni webes API-t.

Ebben az oktatóanyagban fogjuk használni hello Apache Cordova beépülő modul az Active Directory Authentication Library (ADAL) tooimprove egy egyszerű alkalmazást a következő funkciók hello hozzáadásával:

* A pár sornyi kód hitelesíteni a felhasználót, és jogkivonat beszerzése.
* Használja a token tooinvoke hello Graph API tooquery könyvtárhoz, és hello eredmények megjelenítéséhez.  
* Hello ADAL jogkivonat gyorsítótára toominimize hitelesítés használata a hello felhasználó kéri.

toomake ezen fejlesztések kell:

1. Alkalmazás regisztrálása az Azure AD-ben.
2. Adja meg a kódot tooyour app toorequest elemeket.
3. Adja hozzá a kódot toouse hello token lekérdezése hello Graph API-val, és eredmények megjelenítéséhez.
4. Hello Cordova telepítési projekt létrehozása minden hello platformokkal meg szeretné, hogy tootarget hello ADAL Cordova beépülő modul hozzáadása és hello megoldás tesztelése az emulátorok.

## <a name="prerequisites"></a>Előfeltételek
toocomplete ebben az oktatóanyagban szüksége:

* Az Azure AD-bérlő app development jogosultságokkal rendelkező fiók esetében.
* A fejlesztési környezet, amely toouse Apache Cordova van konfigurálva.  

Ha mindkét már beállítása, hogy továbblépjen közvetlenül toostep 1.

Ha az Azure AD-bérlő nem rendelkezik, akkor hello [útmutatást egy tooget](active-directory-howto-tenant.md).

Ha Apache Cordova állítsa be a számítógépre nincs, telepítse a hello következő:

* [Git](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* [Node.js](https://nodejs.org/download/)
* [Cordova CLI](https://cordova.apache.org/) (Csomagkezelő NPM segítségével könnyedén telepíthető: `npm install -g cordova`)

hello telepítések megelőző hello PC és a Mac hello kell működnie.

Minden érintett rendszerek különböző előfeltételei:

* toobuild, és futtassa az alkalmazást a Windows/Táblagép vagy a Windows Phone:
  * Telepítés [Visual Studio 2013 Update 2 vagy újabb verziójú Windows](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Express vagy egy másik verziója) vagy [Visual Studio 2015-öt](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-community).

* toobuild, és futtassa az alkalmazást IOS rendszerre:

  * Xcode telepítése 6.x vagy újabb. Töltse le a hello [Apple Developer hely](http://developer.apple.com/downloads) vagy hello [Mac App Store](http://itunes.apple.com/us/app/xcode/id497799835?mt=12).
  * Telepítés [ios-sim](https://www.npmjs.org/package/ios-sim). Használhatja toostart iOS-alkalmazások az iOS-szimulátor hello parancssorból. (A Terminálszolgáltatások hello keresztül könnyedén telepíthető: `npm install -g ios-sim`.)
* toobuild, és futtassa az alkalmazást az Android:

  * Telepítés [Java fejlesztői készlet (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) vagy újabb. Győződjön meg arról, hogy `JAVA_HOME` (környezeti változó) megfelelően van beállítva, toohello JDK telepítési útvonalat (például, C:\Program Files\Java\jdk1.7.0_75) szerint.
  * Telepítés [Android SDK](http://developer.android.com/sdk/installing/index.html?pkg=tools) , és adja hozzá a hello `<android-sdk-location>\tools` helyét (például C:\tools\Android\android-sdk\tools) tooyour `PATH` környezeti változó.
  * Nyissa meg az Android SDK Manager (például a Terminálszolgáltatások hello keresztül: `android`) és telepítése:
    * *Android 5.0.1-es (API 21)* platform SDK
    * *Android SDK Build Tools* 19.1.0 verzió vagy újabb verzió
    * *Android-támogatás tárház* (kiegészítő funkciók)

  Android SDK hello bármely alapértelmezett emulátor példány nem biztosít. Futtatásával `android avd` hello terminál és jelölje be a **létrehozása**, ha azt szeretné, hogy toorun hello Android-alkalmazást az emulátor. Azt javasoljuk, hogy egy API-szintet 19 vagy magasabb. Hello Android emulator és létrehozásának beállításokkal kapcsolatos további információkért lásd: [AVD Manager](http://developer.android.com/tools/help/avd-manager.html) hello Android helyen.

## <a name="step-1-register-an-application-with-azure-ad"></a>1. lépés: Egy alkalmazás regisztrálása az Azure ad szolgáltatással
Ez a lépés nem kötelező megadni. Ez az oktatóanyag biztosít használható toosee előtti ponton aktívvá vált értékek hello művelet: a minta bármely saját bérlőt kiépítési nélkül. Azonban azt javasoljuk, hogy hajtsa végre ezt a lépést, és ismerkedjen meg hello folyamat, mert lesz szükség a saját alkalmazások létrehozásakor.

Az Azure AD kibocsát jogkivonatok tooonly alkalmazások ismert. Az Azure AD a alkalmazás használata előtt kell toocreate bejegyzése ki azt az Ön bérelt szolgáltatásának. az Ön bérelt szolgáltatásának új alkalmazás tooregister:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Hello felső sávon kattintson a fiókját. A hello **Directory** menüben válassza ki a kívánt tooregister hello Azure AD-bérlő az alkalmazást.
3. Kattintson a **több szolgáltatások** hello bal oldali ablaktáblán, és válassza ki **Azure Active Directory**.
4. Kattintson a **App regisztrációk**, majd válassza ki **Hozzáadás**.
5. Hello utasításokat követve, és hozzon létre egy **natív ügyfélalkalmazás**. (Bár a Cordova-alkalmazásokkal HTML-alapú, most létrehozzuk natív ügyfélalkalmazás itt. Hello **natív ügyfélalkalmazás** beállítást, vagy hello alkalmazás nem fog működni.)
  * **Név** az alkalmazás toousers ismerteti.
  * **Átirányítási URI** hello tooreturn jogkivonatok tooyour alkalmazás által használt URI. Adja meg **http://MyDirectorySearcherApp**.

Regisztráció befejezése után az Azure AD egy egyedi alkalmazás azonosítója tooyour app rendeli hozzá. Ez az érték a következő szakaszok hello lesz szüksége. Az újonnan létrehozott alkalmazás hello hello alkalmazás lapján találja.

toorun `DirSearchClient Sample`, adja meg az újonnan létrehozott hello app engedély tooquery hello Azure AD Graph API:

1. A hello **beállítások** lapon jelölje be **szükséges engedélyek**, majd válassza ki **Hozzáadás**.  
2. Hello Azure Active Directory-alkalmazást, válassza a **Microsoft Graph** , hello API, és adja hozzá a hello **hozzáférés hello a címtárhoz hello bejelentkezett felhasználó** engedélyt a **meghatalmazott Engedélyek**.  Ez lehetővé teszi az alkalmazás tooquery hello Graph API a felhasználók számára.

## <a name="step-2-clone-hello-sample-app-repository"></a>2. lépés: Hello sample app tárház klónozása
A rendszerhéj vagy a parancssorból írja be a következő parancs hello:

    git clone -b skeleton https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova.git

## <a name="step-3-create-hello-cordova-app"></a>3. lépés: Hello Cordova-alkalmazás létrehozása
Toocreate Cordova-alkalmazás több módon is. Ebben az oktatóanyagban hello Cordova parancssori felület (CLI) fogjuk használni.

1. A rendszerhéj vagy a parancssorból írja be a következő parancs hello:

        cordova create DirSearchClient

   Ez a parancs hello mappaszerkezet és állványok hello Cordova-projekt hoz létre.

2. Toohello új DirSearchClient mappa áthelyezése:

        cd .\DirSearchClient

3. Hello tartalom hello alapszintű projekt hello www almappájában másolja a Fájlkezelőben vagy a következő parancsot a rendszerhéj hello segítségével:

  * Windows:`xcopy ..\NativeClient-MultiTarget-Cordova\DirSearchClient www /E /Y`
  * Mac:`cp -r  ../NativeClient-MultiTarget-Cordova/DirSearchClient/* www`

4. Adja hozzá a beépülő modul hello engedélyezett. Erre akkor szükség, hello Graph API meghívására.

        cordova plugin add cordova-plugin-whitelist

5. Adja hozzá a megjeleníteni kívánt toosupport platformfüggetlen hello. egy minta toohave, a következő parancsok hello legalább egy tooexecute kell. Vegye figyelembe, hogy nem kell tudni tooemulate iOS, Windows vagy Mac a Windows rendszer

        cordova platform add android
        cordova platform add ios
        cordova platform add windows

6. Adja hozzá a Cordova beépülő modul tooyour projekt ADAL hello:

        cordova plugin add cordova-plugin-ms-adal

## <a name="step-4-add-code-tooauthenticate-users-and-obtain-tokens-from-azure-ad"></a>4. lépés: Kód tooauthenticate felhasználók hozzáadása, és az Azure AD tokenek beszerzése
Ebben az oktatóanyagban kidolgozása hello alkalmazás nyújt egy egyszerű directory keresési funkciót. hello felhasználói ezután írja be a hello alias bármely felhasználó hello könyvtárban, és néhány alapvető attribútum megjelenítése. hello alapszintű projekt hello alapszintű felhasználói felület (a www/index.html) hello alkalmazás hello definícióját tartalmazza, és hello állványok, amely alapszintű alkalmazást eseményt kábeleket ciklusok, a felhasználói felület kötések, és ennek eredményeként a megjelenítési a logikai (www/js/index.js). hello csak akkor maradt feladata tooadd hello logika, amely megvalósítja az identitás-feladatokat.

hello toodo a kódban kell elsőként bevezetni hello protokoll értékek alapján azonosítja az alkalmazást használó Azure AD és erőforrásokhoz, hogy a célként meghatározott hello. Ezeket az értékeket meg lesz használt tooconstruct hello jogkivonat-kérelmeket. Szúrja be a következő kódrészletet hello index.js fájl hello tetején hello:

```javascript
var authority = "https://login.microsoftonline.com/common",
    redirectUri = "http://MyDirectorySearcherApp",
    resourceUri = "https://graph.windows.net",
    clientId = "a5d92493-ae5a-4a9f-bcbf-9f1d354067d3",
    graphApiVersion = "2013-11-08";
```

Hello `redirectUri` és `clientId` értékek illeszkednie kell az alkalmazást az Azure AD-leíró hello értékek. Hello szerintiek található **konfigurálása** hello Azure-portálon lapon ez az oktatóanyag a korábban az 1. lépésben leírtak szerint.

> [!NOTE]
> Ha nem regisztrálja egy új alkalmazást a saját bérlőt választotta, egyszerűen beillesztheti, előre konfigurált hello értékeket. Majd látható hello minta fut, bár mindig hozzon létre saját bejegyzést az alkalmazások, amelyek termelési környezetben.

Ezután adja hozzá a hello jogkivonatkérelem kódot. Helyezze be a következő kódrészletben közötti hello hello `search` és `renderData` definíciók:

```javascript
    // Shows hello user authentication dialog box if required
    authenticate: function (authCompletedCallback) {

        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
            // Attempt tooauthorize hello user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials, so this triggers hello authentication dialog box
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed tooauthenticate: " + err);
                });
            });
        });

    },
```
Vizsgáljuk meg, hogy a függvény által bontásához a két fő részből áll.
Ez a minta nem tervezett toowork semmilyen bérlővel, mert az megakadályozását toobeing kötött tooa adott egy. Hello használ "/ közös" végpont, amely lehetővé teszi, hogy hello felhasználói tooenter bármely fiók hitelesítési időpontban, és hello kérelem toohello bérlői irányítja, ahol tartozik.

Ezen hello metódus első része hello ADAL gyorsítótár toosee megvizsgálja, ha egy jogkivonatot már van tárolva. Ha igen, hello metódusnak hello bérlők hello token származási helyét az adal-t újrainicializálását. Ez az további utasításokat, szükséges tooavoid oka hello használja a "/ közös" mindig eredményezi hello felhasználói tooenter egy új fiókot kérni.

```javascript
        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
```
hello metódus második része hello hello megfelelő jogkivonatkérelem hajt végre. Hello `acquireTokenSilentAsync` metódus kér ADAL tooreturn jogkivonat megadott hello erőforrás bármely UX megjelenítése nélkül Amely akkor fordulhat elő, ha hello gyorsítótár már tartalmaz egy tárolt, megfelelő hozzáférési jogkivonatot, vagy ha egy frissítési jogkivonat tooget új tokenre használt minden kérdés megjelenítése nélkül. Amely sikertelen lesz, ha azt térhet vissza `acquireTokenAsync`– amely láthatóan fogja kérni a hello felhasználói tooauthenticate.

```javascript
            // Attempt tooauthorize hello user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials, so this triggers hello authentication dialog box
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed tooauthenticate: " + err);
                });
            });
```
Most, hogy hello jogkivonat, azt végül hello Graph API meghívása és szeretnénk hello-keresési lekérdezést. Helyezze be a következő kódrészletet alatt hello hello `authenticate` definíciója:

```javascript
// Makes an API call tooreceive hello user list
    requestData: function (authResult, searchText) {
        var req = new XMLHttpRequest();
        var url = resourceUri + "/" + authResult.tenantId + "/users?api-version=" + graphApiVersion;
        url = searchText ? url + "&$filter=mailNickname eq '" + searchText + "'" : url + "&$top=10";

        req.open("GET", url, true);
        req.setRequestHeader('Authorization', 'Bearer ' + authResult.accessToken);

        req.onload = function(e) {
            if (e.target.status >= 200 && e.target.status < 300) {
                app.renderData(JSON.parse(e.target.response));
                return;
            }
            app.error('Data request failed: ' + e.target.response);
        };
        req.onerror = function(e) {
            app.error('Data request failed: ' + e.error);
        }

        req.send();
    },

```
hello kiindulópont-fájlokat egy egyszerű UX megadott beviteli mezőben írja be a felhasználói alias. Ezt a módszert használja, hogy érték tooconstruct egy lekérdezést, összevonásához hello hozzáférési jogkivonatot, elküldi a tooMicrosoft Graph és hello eredményeket elemezni. Hello `renderData` metódus már szerepel a hello kiindulópont-fájl, gondoskodik hello eredmények megjelenítése.

## <a name="step-5-run-hello-app"></a>5. lépés: Hello alkalmazás futtatása
Az alkalmazás készen áll a finally toorun. Az operációs rendszer egyszerű: hello alkalmazás indításakor hello alias toolook akarja hello felhasználó adja meg, majd hello gombra. Megkéri, hogy hitelesítésre. Sikeres hitelesítés és a keresés sikeres a keresés hello felhasználó hello attribútumok jelennek meg.

Utólagosan végrehajtják a hello keresési bármilyen kérdés megjelenítése nélkül, köszönhetően hello toohello jelenléte korábban beszerzett jogkivonat a gyorsítótárban.

Platform hello konkrét hello alkalmazást futtató lépései eltérők lehetnek.

### <a name="windows-10"></a>Windows 10
   / Táblagép:`cordova run windows --archs=x64 -- --appx=uap`

   A Mobile (a Windows 10 Mobile-eszköz csatlakoztatásakor tooa PC szükséges):`cordova run windows --archs=arm -- --appx=uap --phone`

   > [!NOTE]
   > Során hello először futtatja kérheti a toosign fejlesztői licencet. További információkért lásd: [fejlesztői licenc](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).

### <a name="windows-81-tabletpc"></a>Windows 8.1 / Táblagép
   `cordova run windows`

   > [!NOTE]
   > Során hello először futtatja kérheti a toosign fejlesztői licencet. További információkért lásd: [fejlesztői licenc](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).

### <a name="windows-phone-81"></a>Windows Phone 8.1
   egy csatlakoztatott eszközön toorun:`cordova run windows --device -- --phone`

   hello alapértelmezett emulátor toorun:`cordova emulate windows -- --phone`

   Használjon `cordova run windows --list -- --phone` toosee összes elérhető cél és `cordova run windows --target=<target_name> -- --phone` toorun hello alkalmazás egy adott eszközt vagy emulátort (például `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).

### <a name="android"></a>Android
   egy csatlakoztatott eszközön toorun:`cordova run android --device`

   hello alapértelmezett emulátor toorun:`cordova emulate android`

   Ellenőrizze, hogy létrehozott egy emulátor példány AVD Manager használatával hello "Előfeltételek" szakaszban korábban leírtak szerint.

   Használjon `cordova run android --list` toosee összes elérhető cél és `cordova run android --target=<target_name>` toorun hello alkalmazás egy adott eszközt vagy emulátort (például `cordova run android --target="Nexus4_emulator"`).

### <a name="ios"></a>iOS
   egy csatlakoztatott eszközön toorun:`cordova run ios --device`

   hello alapértelmezett emulátor toorun:`cordova emulate ios`

   > [!NOTE]
   > Győződjön meg arról, hogy hello `ios-sim` telepített csomag toorun hello emulátorának. További információkért tekintse meg a "Hello"Előfeltételek"szakaszában.

    Use `cordova run ios --list` toosee all available targets and `cordova run ios --target=<target_name>` toorun hello application on specific device or emulator (for example, `cordova run android --target="iPhone-6"`).

    Use `cordova run --help` toosee additional build and run options.

## <a name="next-steps"></a>Következő lépések
Referenciaként befejeződött hello mintát (a konfigurációs értékek nélkül) érhető el a [GitHub](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient).

Most áthelyezés speciális toomore (és további érdekes) forgatókönyvek is. Érdemes lehet tootry: [egy Node.js webes API-t az Azure ad-vel biztonságos](active-directory-devquickstarts-webapi-nodejs.md).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
