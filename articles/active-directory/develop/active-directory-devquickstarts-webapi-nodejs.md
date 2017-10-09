---
title: "Első lépések AD Node.js aaaAzure |} Microsoft Docs"
description: "Hogyan toobuild egy Node.js REST webes API, amely integrálható az Azure AD használatára a hitelesítéshez."
services: active-directory
documentationcenter: nodejs
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 7654ab4c-4489-4ea5-aba9-d7cdc256e42a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 512ae6de9acfde8b58c0447ab4a6b573fb6407c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-web-apis-for-nodejs"></a>A Node.js webes API-knak az első lépései
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

A *Passport* a Node.js-hez készült közbenső hitelesítési szoftver. Rugalmas és, moduláris Passport diszkréten eldobása tooany az Express-alapú vagy Restify webalkalmazás. Stratégiák széles választékát támogatja a hitelesítés és az felhasználónév és jelszó, Facebook, Twitter, és több. Kidolgoztunk is stratégiát a Microsoft Azure Active Directory (Azure AD). Azt telepítenie kell a modult, és vegye fel a Microsoft Azure Active Directory hello `passport-azure-ad` beépülő modult.

toodo, kell:

1. Alkalmazás regisztrálása az Azure AD-ben.
2. Állítsa be az alkalmazás toouse Passport által `passport-azure-ad` beépülő modult.
3. Ügyfél alkalmazás toocall hello tooDo lista webes API-k konfigurálása.

az oktatóanyag kódjának hello kezelt [a Githubon](https://github.com/Azure-Samples/active-directory-node-webapi).

> [!NOTE]
> Ez a cikk nem fedi le, hogyan tooimplement bejelentkezés, -előfizetés, vagy az Azure AD B2C felügyeleti profilt. A cikk foglalkozik a hívó webes API-k hello felhasználó már hitelesítése után.  Azt javasoljuk, hogy a kiindulási pont [hogyan Azure Active Directory dokumentummal toointegrate](active-directory-how-to-integrate.md) toolearn hello alapokról az Azure Active Directory.
>
>

A futó példa github MIT licenccel, az összes hello forráskódja már megjelent úgy érzi, hogy szabad tooclone (vagy még jobban elágazás), és visszajelzést, és lekéréses kérelmeket.

## <a name="about-nodejs-modules"></a>A Node.js modulok kapcsolatos
Ebben a bemutatóban a Node.js modulok használjuk. A modulokra betölthető JavaScript-csomagok, amelyek bizonyos funkciók biztosítanak az alkalmazáshoz. Általában modulok használatával telepítse az NPM parancssori eszköz Node.js hello hello NPM telepítési könyvtárában. Azonban néhány modulok, például HTTP-modulja hello hello core Node.js csomagban található.

Telepített modulok menti a hello **node_modules** könyvtárhoz, a Node.js telepítési könyvtár hello gyökerében. Minden hello modulja **node_modules** directory kezeli a saját **node_modules** , amelyektől függ modulokat tartalmazó könyvtárat. Emellett minden egyes szükséges a modulnak a **node_modules** könyvtár. A rekurzív könyvtárstruktúrát hello függőségi lánc jelöli.

Ez a függőségi lánc struktúra egy nagyobb alkalmazás erőforrásigényét eredményez. De azt is garantálja, hogy minden függőség teljesülnek, és adott hello verziójának hello modulok fejlesztési használt termelési is használja. Ez sokkal kiszámíthatóbbá teszi hello éles alkalmazása működését, és megakadályozza, hogy a versioning problémák, amelyek hatással lehetnek a felhasználók.

## <a name="step-1-register-an-azure-ad-tenant"></a>1. lépés: Az Azure AD-bérlő regisztrálása
toouse ez mintát, az Azure Active Directory-bérlő van szüksége. Ha nem tudja, milyen a bérlő vagy hogyan tooget, lásd [hogyan tooget az Azure AD bérlői](active-directory-howto-tenant.md).

## <a name="step-2-create-an-application"></a>2. lépés: Az alkalmazás létrehozása
Ezután alkalmazást hoz létre a címtárban, hogy az Azure AD által biztosított információkat, hogy kell-e toosecurely kommunikálni az alkalmazást.  Hello ügyfélalkalmazást és a webes API jelölik egyetlen **Alkalmazásazonosító** ebben az esetben, mert alkalmazássá logikai.  hajtsa végre az alkalmazást, és egy toocreate [ezeket az utasításokat](active-directory-how-applications-are-added.md). A sor üzleti alkalmazás készítésekor [hasznosak lehetnek a további utasítások](../active-directory-applications-guiding-developers-for-lob-applications.md).

egy alkalmazás toocreate:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).

2. A hello felső menüben válassza ki a fiókját. Ekkor a hello **Directory** menüben válassza ki a kívánt tooregister hello Active Directory-bérlőt az alkalmazást.

3. Hello hello bal oldali menüben válasszon ki **több szolgáltatások**, majd válassza ki **Azure Active Directory**.

4. Válassza ki **App regisztrációk**, majd válassza ki **Hozzáadás**.

5. Kövesse az utasításokat toocreate hello egy **webalkalmazás és/vagy WebAPI**.

      * Hello **neve** hello az alkalmazás az alkalmazás tooend felhasználók ismerteti.

      * Hello **bejelentkezési URL-cím** hello alap URL-cím az alkalmazás.  az alapértelmezett URL-címe hello mintakód hello `https://localhost:8080`.

6. Miután regisztrálta, az Azure AD rendeli hozzá az alkalmazás egy egyedi azonosítót. Ez az érték kell a következő szakaszok hello, ezért másolja hello alkalmazás oldalról.

7. A hello **beállítások** -> **tulajdonságok** az alkalmazás lapján hello App ID URI frissítése. Hello **App ID URI** az alkalmazás egyedi azonosítója. hello konvenció: toouse `https://<tenant-domain>/<app-name>`, például: `https://contoso.onmicrosoft.com/my-first-aad-app`.

8. Hozzon létre egy **kulcs** hello az alkalmazás **beállítások** lapon, majd másolja ki valahová. Szüksége lehet rájuk hamarosan.

## <a name="step-3-download-nodejs-for-your-platform"></a>3. lépés: A Node.js letöltése a platformra
toosuccessfully Ez a minta használatához rendelkeznie kell a Node.js telepített és működő.

Telepítse a Node.js-t [http://nodejs.org](http://nodejs.org).

## <a name="step-4-install-mongodb-on-your-platform"></a>4. lépés: Telepítse MongoDB a platformon
toosuccessfully Ez a minta használatához rendelkeznie kell a MongoDB telepített és működő. MongoDB toomake hello állandó REST API különböző kiszolgálópéldányok közötti használhatja.

Telepítse a mongodb-t a [http://mongodb.org](http://www.mongodb.org).

> [!NOTE]
> Ez a forgatókönyv azt feltételezi, hogy a használt hello alapértelmezett telepítését és kiszolgálóvégpontjait mongodb, amelyek írásának időpontjában hello mongodb://localhost.
>
>

## <a name="step-5-install-hello-restify-modules-in-your-web-api"></a>5. lépés: Hello Restify-modulok telepítése a webes API
Használunk Restify toobuild REST API-n. A restify egy minimális igényű és rugalmas Node.js alkalmazási keretrendszer származó Express van. Számos hatékony funkciót kínál a Connectre épített REST API-k létrehozásához.

### <a name="install-restify"></a>A Restify telepítése
1. Hello parancssorból módosítsa a könyvtárakat toohello **azuread** könyvtár. Ha hello **azuread** könyvtár nem létezik, hozza létre.

        `cd azuread - or- mkdir azuread; cd azuread`

2. Írja be a következő parancs hello:

    `npm install restify`

    Ez a parancs elvégzi a Restify telepítését.

#### <a name="did-you-get-an-error"></a>Hibaüzenetet kapott?
Egyes operációs rendszerek NPM használata esetén előfordulhat, hogy hibaüzenet, amely szerint **hiba: EPERM, chmod "/ usr/helyi/bin /..."** és, hogy rendszergazdaként futó hello fiók megpróbál javaslatot. Ha ez történik, a hello sudo parancs toorun NPM magasabb jogosultsági szinten.

#### <a name="did-you-get-an-error-regarding-dtrace"></a>Vonatkozó DTRACE hibaüzenetet kapott?
Ilyen hiba láthatja a Restify telepítése során:

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:267:23)
gyp ERR! stack     at ChildProcess.EventEmitter.emit (events.js:98:17)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (child_process.js:789:12)
gyp ERR! System Darwin 13.1.0
gyp ERR! command "node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Volumes/Development HD/azuread/node_modules/restify/node_modules/dtrace-provider
gyp ERR! node -v v0.10.11
gyp ERR! node-gyp -v v0.10.0
gyp ERR! not ok
npm WARN optional dep failed, continuing dtrace-provider@0.2.8
```
A Restify a DTrace segítségével biztosít a REST-hívások követésére szolgáló funkciókat. Számos operációs rendszer azonban nem rendelkezik DTrace. Ezeket a hibákat nyugodtan figyelmen kívül hagyhatja.

a parancs kimenetének hello alábbihoz hasonló toohello kimenete a következő:

    restify@2.6.1 node_modules/restify
    ├── assert-plus@0.1.4
    ├── once@1.3.0
    ├── deep-equal@0.0.0
    ├── escape-regexp-component@1.0.2
    ├── qs@0.6.5
    ├── tunnel-agent@0.3.0
    ├── keep-alive-agent@0.0.1
    ├── lru-cache@2.3.1
    ├── node-uuid@1.4.0
    ├── negotiator@0.3.0
    ├── mime@1.2.11
    ├── semver@2.2.1
    ├── spdy@1.14.12
    ├── backoff@2.3.0
    ├── formidable@1.0.14
    ├── verror@1.3.6 (extsprintf@1.0.2)
    ├── csv@0.3.6
    ├── http-signature@0.10.0 (assert-plus@0.1.2, asn1@0.1.11, ctype@0.5.2)
    └── bunyan@0.22.0 (mv@0.0.5)


## <a name="step-6-install-passportjs-in-your-web-api"></a>6. lépés: Telepítse Passport.js a webes API
A [Passport](http://passportjs.org/) a Node.js-hez készült közbenső hitelesítési szoftver. Rugalmas és, moduláris Passport diszkréten eldobása tooany az Express-alapú vagy Restify webalkalmazás. Stratégiák széles választékát támogatja a hitelesítés és az felhasználónév és jelszó, Facebook, Twitter, és több.

Az Azure Active Directory kidolgoztunk is stratégiát. Azt telepítenie kell a modult, és adja hozzá a hello Azure Active Directory stratégiai bővítményt.

1. Hello parancssorból módosítsa a könyvtárakat toohello **azuread** könyvtár.

2. tooinstall passport.js, adja meg a következő parancs hello:

    `npm install passport`

    hello parancs kimenetében hello alábbihoz hasonló toohello következő:

``
        passport@0.1.17 node_modules\passport
        ├── pause@0.0.1
        └── pkginfo@0.2.3
``

## <a name="step-7-add-passport-azure-ad-tooyour-web-api"></a>7. lépés:, Adja hozzá a Passport-Azure-AD tooyour webes API
Ezután azt hozzá hello OAuth stratégiát `passport-azure-ad`, a stratégiacsomag szolgál, amelyek kapcsolódnak az Azure Active Directory tooPassport. Ez a stratégia használható a tulajdonosi jogkivonatokhoz a REST API minta használjuk.

> [!NOTE]
> Bár az OAuth2 keretrendszerében, amelyben bármely ismert jogkivonat típus kibocsáthatja, csak bizonyos tokentípusokat gyakran használják. Tulajdonosi jogkivonatok olyan végpontok védelmére szolgáló leggyakrabban használt hello jogkivonatok. Az OAuth2 token legszélesebb körben kiadott hello típusú. Számos implementáció eleve feltételezik, hogy hello csak ilyen típusú kiállított jogkivonatokat tulajdonosi jogkivonatoknak nevezzük.
>
>

Hello parancssorból módosítsa a könyvtárakat toohello **azuread** könyvtár.

Típus hello következő parancsot a tooinstall hello Passport.js `passport-azure-ad module`:

`npm install passport-azure-ad`

hello hello parancs kimenete a következő kimeneti hasonló toohello kell kinéznie:


    passport-azure-ad@1.0.0 node_modules/passport-azure-ad
    ├── xtend@4.0.0
    ├── xmldom@0.1.19
    ├── passport-http-bearer@1.0.1 (passport-strategy@1.0.0)
    ├── underscore@1.8.3
    ├── async@1.3.0
    ├── jsonwebtoken@5.0.2
    ├── xml-crypto@0.5.27 (xpath.js@1.0.6)
    ├── ursa@0.8.5 (bindings@1.2.1, nan@1.8.4)
    ├── jws@3.0.0 (jwa@1.0.1, base64url@1.0.4)
    ├── request@2.58.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, tunnel-agent@0.4.1, oauth-sign@0.8.0, isstream@0.1.2, extend@2.0.1, json-stringify-safe@5.0.1, node-uuid@1.4.3, qs@3.1.0, combined-stream@1.0.5, mime-types@2.0.14, form-data@1.0.0-rc1, http-signature@0.11.0, bl@0.9.4, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
    └── xml2js@0.4.9 (sax@0.6.1, xmlbuilder@2.6.4)



## <a name="step-8-add-mongodb-modules-tooyour-web-api"></a>8. lépés: A MongoDB modulok tooyour webes API hozzáadása
Az adattároló azt a mongodb. Éppen ezért ellenőriznünk kell tooinstall hello széles körben használt beépülő modul hívott Mongoose toomanage modellek és sémák. Mongodb-protokolltámogatással (amelyet a MongoDB is neveznek) kell tooinstall hello adatbázis-illesztőprogramot is.

 `npm install mongoose`

## <a name="step-9-install-additional-modules"></a>9. lépés: További modulok telepítése
Ezután a fennmaradó szükséges modulokat hello telepítjük.

1. Hello parancssorból módosítsa a könyvtárakat toohello **azuread** mappát, ha már nem létezik.

    `cd azuread`

2. Adja meg a következő parancsok tooinstall hello ezeket a modulokat a **node_modules** könyvtár:

    * `npm install assert-plus`
    * `npm install bunyan`
    * `npm update`

## <a name="step-10-create-a-serverjs-with-your-dependencies"></a>10. lépés: Hozzon létre egy server.js a függőségek
hello server.js fájl hello funkcióinak többségét biztosít a webes API-kiszolgálóhoz. Azt adja hozzá a kódfájl toothis része. Élesben való használat esetén javasoljuk, hogy Ön azonosítóterületen kisebb fájlok, például különálló útvonalak és vezérlők hello funkciókat. Ebben a bemutatóban a server.js használjuk funkcióhoz.

1. Hello parancssorból módosítsa a könyvtárakat toohello **azuread** mappát, ha már nem létezik.

    `cd azuread`

2. Hozzon létre egy `server.js` a kedvenc szerkesztő fájlt, és adja hozzá a következő információ hello:

    ```Javascript
        'use strict';

        /**
         * Module dependencies.
         */

        var fs = require('fs');
        var path = require('path');
        var util = require('util');
        var assert = require('assert-plus');
        var bunyan = require('bunyan');
        var getopt = require('posix-getopt');
        var mongoose = require('mongoose/');
        var restify = require('restify');
        var passport = require('passport');
      var BearerStrategy = require('passport-azure-ad').BearerStrategy;
    ```

3. Hello fájl mentéséhez. Tooit hamarosan még visszatérünk.

## <a name="step-11-create-a-config-file-toostore-your-azure-ad-settings"></a>11. lépés: Hozzon létre egy konfigurációs fájl toostore az Azure AD-beállítások
Ez a kódfájl hello konfigurációs paraméterek az Azure Active Directory portálon tooPassport.js a adja át. Ezeket a konfigurációs értékeket létrehozott hello webes API toohello portál hello forgatókönyv első része hello való hozzáadásakor. Milyen tooput hello értékeket a következő paraméterek közül azt ismertetik, hello kód másolását követően.

1. Hello parancssorból módosítsa a könyvtárakat toohello **azuread** mappát, ha már nem létezik.

    `cd azuread`

2. Hozzon létre egy `config.js` a kedvenc szerkesztő fájlt, és adja hozzá a következő információ hello:

    ```Javascript
         exports.creds = {
             mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
             clientID: 'your client ID',
             audience: 'your application URL',
            // you cannot have users from multiple tenants sign in tooyour server unless you use hello common endpoint
          // example: https://login.microsoftonline.com/common/.well-known/openid-configuration
             identityMetadata: 'https://login.microsoftonline.com/<your tenant id>/.well-known/openid-configuration',
             validateIssuer: true, // if you have validation on, you cannot have users from multiple tenants sign in tooyour server
             passReqToCallback: false,
             loggingLevel: 'info' // valid are 'info', 'warn', 'error'. Error always goes toostderr in Unix.

         };
    ```
3. Hello fájl mentéséhez.

## <a name="step-12-add-configuration-values-tooyour-serverjs-file"></a>12. lépés: A konfigurációs értékek tooyour server.js fájl felvétele
Igazolnia kell tooread ezeket az értékeket a hello .config fájlból létrehozott alkalmazás között. toodo, jelenleg felvenni hello .config kiterjesztésű fájlt kötelező erőforrásként az alkalmazásban. Majd hello globális változók toomatch hello változók hivatott hello config.js dokumentum.

1. Hello parancssorból módosítsa a könyvtárakat toohello **azuread** mappát, ha már nem létezik.

    `cd azuread`

2. Nyissa meg a `server.js` a kedvenc szerkesztő fájlt, és adja hozzá a következő információ hello:

    ```Javascript
    var config = require('./config');
    ```
3. Majd adja hozzá az új szakasz túl`server.js` a hello a következő kódot:

    ```Javascript
    var options = {
        // hello URL of hello metadata document for your app. We will put hello keys for token validation from hello URL found in hello jwks_uri tag of hello in hello metadata.
        identityMetadata: config.creds.identityMetadata,
        clientID: config.creds.clientID,
        validateIssuer: config.creds.validateIssuer,
        audience: config.creds.audience,
        passReqToCallback: config.creds.passReqToCallback,
        loggingLevel: config.creds.loggingLevel

    };

    // Array toohold logged in users and hello current logged in user (owner).
    var users = [];
    var owner = null;

    // Our logger.
    var log = bunyan.createLogger({
        name: 'Azure Active Directory Bearer Sample',
             streams: [
            {
                stream: process.stderr,
                level: "error",
                name: "error"
            },
            {
                stream: process.stdout,
                level: "warn",
                name: "console"
            }, ]
    });

      // If hello logging level is specified, switch tooit.
      if (config.creds.loggingLevel) { log.levels("console", config.creds.loggingLevel); }

    // MongoDB setup.
    // Set up some configuration.
    var serverPort = process.env.PORT || 8080;
    var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
    ```

4. Hello fájl mentéséhez.

## <a name="step-13-add-hello-mongodb-model-and-schema-information-by-using-mongoose"></a>13. lépés: Hello MongoDB modell és séma információk hozzáadása a Mongoose segítségével
Ez a felkészítés most toostart fizető, mivel ezek a fájlok egy REST API-szolgáltatásba kombinációja lesz.

Ennél a bemutatónál használjuk MongoDB toostore a feladatok, lásd a 4. lépés.

A hello `config.js` fájlt, hogy a 11. lépésben létrehozott, az adatbázis hívtuk `tasklist`, mert, hogy mi azt put hello végén a **mogoose_auth_local** kapcsolat URL-címet. Ez az adatbázis előre a MongoDB-ben nem kell toocreate. Ehelyett MongoDB hoz létre ez az USA hello először futtassa a kiszolgálói alkalmazás (feltéve, hogy hello az adatbázis nem létezik).

Most, hogy megadta, hogy rendelkezik hello server melyik MongoDB-adatbázist toouse tapasztalatairól, igazolnia kell toowrite néhány további kódrészletet toocreate hello modell és séma a kiszolgálói feladatokhoz.

### <a name="discussion-of-hello-model"></a>Az ismertető hello modell
A sémamodell felettébb egyszerű. Szükség szerint bontsa ki.

: Hello név hello személy, aki hozzá van rendelve toohello feladat. A **karakterlánc**.

FELADAT: hello feladat magát. A **karakterlánc**.

: Hello dátum hello tevékenység egy lejárt. A **DATETIME**.

BEFEJEZŐDÖTT: Ha hello feladat befejeződött-e. A **LOGIKAI**.

### <a name="creating-hello-schema-in-hello-code"></a>Hello kódban hello séma létrehozása
1. Hello parancssorból módosítsa a könyvtárakat toohello **azuread** mappát, ha már nem létezik.

    `cd azuread`

2. Nyissa meg a `server.js` a kedvenc szerkesztő fájlt, és adja hozzá a következő információ hello konfigurációs bejegyzés alatt hello:

    ```Javascript
    // Connect tooMongoDB.
    global.db = mongoose.connect(serverURI);
    var Schema = mongoose.Schema;
    log.info('MongoDB Schema loaded');

    // Here we create a schema toostore our tasks and users. It's a fairly simple schema for now.
    var TaskSchema = new Schema({
        owner: String,
        task: String,
        completed: Boolean,
        date: Date
    });

    // Use hello schema tooregister a model.
    mongoose.model('Task', TaskSchema);
    var Task = mongoose.model('Task');
    ```
Hello kódból állapítható meg, mivel azt először hozza létre a séma. Majd a modellobjektumot, amelyet az adatok teljes hello code, amikor meghatároztuk toostore használjuk létrehozhatunk a **útvonalak**.

## <a name="step-14-add-our-routes-for-our-task-rest-api-server"></a>14. lépés: A feladat REST API-kiszolgálóhoz az mutató útvonalak hozzáadása
Most, hogy egy adatbázis-modell toowork rendelkező, adjuk hozzá hello útvonalak folyamatos használata dolgozunk a REST API-kiszolgálóhoz.

### <a name="about-routes-in-restify"></a>Az útvonalak működése a Restify programban
Útvonalak használhatók Restify hello azonos módon azokat a hello Express verem. Útvonalak megadása hello hello ügyfél alkalmazások toocall várható URI segítségével. Az útvonalakat általában külön fájlban definiálni. A célokra azt helyezze el az útvonalakat hello server.js fájl. Azt javasoljuk, hogy a saját üzemi használatra fájlba figyelembe ezeket az útvonalakat.

A Restify-útvonal egy tipikus mintája a következőképpen történik:

```Javascript
function createObject(req, res, next) {

// Do work on object.

 _object.name = req.params.object; // passed value is in req.params under object

 ///...

return next(); // Keep hello server going.
}

....

server.post('/service/:add/:object', createObject); // Calls createObject on routes that match this.

```


Ez az a legalapvetőbb szinten hello mintát. A restify (és az Express) sokkal összetettebb funkciók, például velük az alkalmazástípusokat és a különböző végpontok között összetett útválasztási adja meg. A célból hogy megakadályozzák ezeket az útvonalakat egyszerű.

### <a name="add-default-routes-tooour-server"></a>Alapértelmezett útvonalak tooour kiszolgáló hozzáadása
A Microsoft most hozzáadása hello alapvető CRUD-útvonalakat: létrehozása, beolvasása, frissítése és törlése.

1. Hello parancssorból módosítsa a könyvtárakat toohello **azuread** mappát, ha már nem létezik:

    `cd azuread`

2. Nyissa meg hello `server.js` a kedvenc szerkesztő fájlt, és adja hozzá a következő információ alább hello előző adatbázis bejegyzések hello:

```Javascript

/**
 *
 * APIs for our REST Task server.
 */

// Create a task.

function createTask(req, res, next) {

    // Restify currently has a bug which doesn't allow you tooset default headers.
    // These headers comply with CORS and allow us toomongodbServer our response tooany origin.

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it, and save it tooMongodb.
    var _task = new Task();

    if (!req.params.task) {
        req.log.warn('createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.task = req.params.task;
    _task.date = new Date();

    _task.save(function(err) {
        if (err) {
            req.log.warn(err, 'createTask: unable toosave');
            next(err);
        } else {
            res.send(201, _task);

        }
    });

    return next();

}


// Delete a task by name.

function removeTask(req, res, next) {

    Task.remove({
        task: req.params.task,
        owner: owner
    }, function(err) {
        if (err) {
            req.log.warn(err,
                'removeTask: unable toodelete %s',
                req.params.task);
            next(err);
        } else {
            log.info('Deleted task:', req.params.task);
            res.send(204);
            next();
        }
    });
}

// Delete all tasks.

function removeAll(req, res, next) {
    Task.remove();
    res.send(204);
    return next();
}


// Get a specific task based on name.

function getTask(req, res, next) {

    log.info('getTask was called for: ', owner);
    Task.find({
        owner: owner
    }, function(err, data) {
        if (err) {
            req.log.warn(err, 'get: unable tooread %s', owner);
            next(err);
            return;
        }

        res.json(data);
    });

    return next();
}

/// Simple returns hello list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Restify currently has a bug which doesn't allow you tooset default headers.
    // These headers comply with CORS and allow us toomongodbServer our response tooany origin.

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    log.info("listTasks was called for: ", owner);

    Task.find({
        owner: owner
    }).limit(20).sort('date').exec(function(err, data) {

        if (err) {
            return next(err);
        }

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in hello database. Did you initialize hello database as stated in hello README?");
        }

        if (!owner) {
            log.warn(err, "You did not pass an owner when listing tasks.");
        } else {

            res.json(data);

        }
    });

    return next();
}

```

### <a name="add-error-handling-in-our-apis"></a>Az API-Jainkkal a hibakezelés
```

///--- Errors for communicating something interesting back toohello client.

function MissingTaskError() {
    restify.RestError.call(this, {
        statusCode: 409,
        restCode: 'MissingTask',
        message: '"task" is a required parameter',
        constructorOpt: MissingTaskError
    });

    this.name = 'MissingTaskError';
}
util.inherits(MissingTaskError, restify.RestError);


function TaskExistsError(owner) {
    assert.string(owner, 'owner');

    restify.RestError.call(this, {
        statusCode: 409,
        restCode: 'TaskExists',
        message: owner + ' already exists',
        constructorOpt: TaskExistsError
    });

    this.name = 'TaskExistsError';
}
util.inherits(TaskExistsError, restify.RestError);


function TaskNotFoundError(owner) {
    assert.string(owner, 'owner');

    restify.RestError.call(this, {
        statusCode: 404,
        restCode: 'TaskNotFound',
        message: owner + ' was not found',
        constructorOpt: TaskNotFoundError
    });

    this.name = 'TaskNotFoundError';
}

util.inherits(TaskNotFoundError, restify.RestError);
```


## <a name="step-15-create-your-server"></a>15. lépés: A kiszolgáló létrehozása
Az adatbázis definiáltuk, és az útvonalak legyenek érvényben. hello utolsó lépésként toodo van adja hozzá a hívásokat kezelő hello server-példányt.

A Restify (és az Express) teheti a nagy mennyiségű testreszabási egy REST API-kiszolgálóhoz, de újra fogjuk toouse hello most az alapszintű beállításokat a célokra.

```Javascript
/**
 * Our server.
 */


var server = restify.createServer({
    name: "Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads.
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//.
server.pre(restify.pre.sanitizePath());

// Handle annoying user agents (curl).
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in).
server.use(restify.requestLogger());

// Allow five requests per second by IP, and burst too10.
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use hello common stuff you probably want.
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allow for JSON mapping tooREST.
```

## <a name="step-16-add-hello-routes-toohello-server-without-authentication-for-now"></a>16. lépés: Hello útvonalak toohello kiszolgáló (hitelesítés nélkül most) hozzáadása
```Javascript
/// Now hello real handlers. Here we just CRUD.
/**
/*
/* Each of these handlers is protected by our OIDCBearerStrategy by invoking 'oidc-bearer'.
/* In hello pasport.authenticate() method. We set 'session: false' because REST is stateless and
/* we don't need toomaintain session state. You can experiment with removing API protection
/* by removing hello passport.authenticate() method as follows:
/*
/* server.get('/tasks', listTasks);
/*
**/
server.get('/tasks', listTasks);
server.get('/tasks', listTasks);
server.get('/tasks/:owner', getTask);
server.head('/tasks/:owner', getTask);
server.post('/tasks/:owner/:task', createTask);
server.post('/tasks', createTask);
server.del('/tasks/:owner/:task', removeTask);
server.del('/tasks/:owner', removeTask);
server.del('/tasks', removeTask);
server.del('/tasks', removeAll, function respond(req, res, next) {
res.send(204);
next();
});
// Register a default '/' handler.
server.get('/', function root(req, res, next) {
var routes = [
'GET /',
'POST /tasks/:owner/:task',
'POST /tasks (for JSON body)',
'GET /tasks',
'PUT /tasks/:owner',
'GET /tasks/:owner',
'DELETE /tasks/:owner/:task'
];
res.send(200, routes);
next();
});
server.listen(serverPort, function() {
var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
consoleMessage += '\n %s server is listening at %s';
consoleMessage += '\n Open your browser too%s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json tooget some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```

## <a name="step-17-run-hello-server-before-adding-oauth-support"></a>17. lépés: Futtatása hello server (OAuth-támogatás hozzáadása)
Ahhoz, hogy fel hitelesítési tesztelje a kiszolgálót.

hello legegyszerűbb módja tootest a kiszolgáló van, a parancssorban curl használatával. Nézzük meg, igazolnia kell egy segédprogram, amely lehetővé teszi tooparse kimeneti adatok JSON-ként.

1. Telepítse a következő (a következő példák az összes hello ezzel az eszközzel) JSON-segédprogramot hello:

    `$npm install -g jsontool`

    Ezzel globálisan telepíti a hello JSON-segédprogramot. Most, hogy azt már valósítható meg, amely, most lejátszása hello kiszolgálóval:

2. Először is győződjön meg arról, hogy fut-e a mongoDB-példány:

    `$sudo mongod`

3. Ezután módosítsa toohello könyvtárat, és sütővasak start:

    `$ cd azuread` `$ node server.js`

    `$ curl -isS http://127.0.0.1:8080 | json`

    ```Shell
    HTTP/1.1 200 OK
    Connection: close
    Content-Type: application/json
    Content-Length: 171
    Date: Tue, 14 Jul 2015 05:43:38 GMT
    [
    "GET /",
    "POST /tasks/:owner/:task",
    "POST /tasks (for JSON body)",
    "GET /tasks",
    "PUT /tasks/:owner",
    "GET /tasks/:owner",
    "DELETE /tasks/:owner/:task"
    ]
    ```

4. Ezt követően hozzá lehessen adni a feladat ily módon:

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    hello választ kell kapnia:

        ```Shell
        HTTP/1.1 201 Created
        Connection: close
        Access-Control-Allow-Origin: *
        Access-Control-Allow-Headers: X-Requested-With
        Content-Type: application/x-www-form-urlencoded
        Content-Length: 5
        Date: Tue, 04 Feb 2014 01:02:26 GMT
        Hello
        ```
    És megjelenő feladatok Brandon az ilyen módon:

        `$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Ha minden minden megfelelően működik, a rendszer készen áll a tooadd OAuth toohello REST API-kiszolgálóhoz.

MongoDB-vel futó REST API-kiszolgáló rendelkezik!

## <a name="step-18-add-authentication-tooour-rest-api-server"></a>18. lépés: A hitelesítés tooour REST API-kiszolgáló hozzáadása
Most, hogy a futó REST API-t, először az Azure ad-val hasznos információkkal.

Hello parancssorból módosítsa a könyvtárakat toohello **azuread** mappát, ha már nem létezik.

`cd azuread`

### <a name="use-hello-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>Hello részét passport-azure-ad részét képező OIDCBearerStrategy használata
Mi építettünk, amennyiben egy tipikus REST TODO kiszolgálót hitelesítés nélkül. Ez azért, ha először üzembe, amelyek.

1. Először igazolnia kell, hogy szeretnénk toouse Passport tooindicate. Ez a jogosultság helyezze a másik kiszolgáló konfigurációja után:

    ```Javascript
            // Let's start using Passport.js.

            server.use(passport.initialize()); // Starts passport.
            server.use(passport.session()); // Provides session support.
    ```
    > [!TIP]
    > Amikor ír API-k, javasoljuk, hogy mindig kapcsolja hello adatok toosomething egyedi felhasználói hello hello jogkivonatból nem tudnak jogosulatlanul hozzáférni. Ha a kiszolgáló tárolja a TODO elemeket tárolja őket hello Objektumazonosító hello felhasználó hello jogkivonatot (a token.OID), amelyeket a Microsoft hello "tulajdonos" mező alapján. Ez biztosítja, hogy csak adott felhasználó hozzáférhet-e a TODOs. Nincs nem jelenik meg sehol hello API "tulajdonos", a külső felhasználó kérhet hello TODOs mások, még akkor is, ha hitelesített.                    

2. Következő most használja az hello tulajdonosi stratégia `passport-azure-ad`. Tekintse meg most hello kódot, és hamarosan elmagyarázzuk hello rest. Be az alábbiakat a fentiekben beillesztett:

```Javascript
    /**
    /*
    /* Calling hello OIDCBearerStrategy and managing users.
    /*
    /* Passport pattern provides hello need toomanage users and info tokens
    /* with a FindorCreate() method that must be provided by hello implementor.
    /* Here we just auto-register any user and implement a FindById().
    /* You'll want toodo something smarter.
    **/

    var findById = function(id, fn) {
        for (var i = 0, len = users.length; i < len; i++) {
            var user = users[i];
            if (user.sub === id) {
                log.info('Found user: ', user);
                return fn(null, user);
            }
        }
        return fn(null, null);
    };


    var bearerStrategy = new BearerStrategy(options,
        function(token, done) {
            log.info('verifying hello user');
            log.info(token, 'was hello token retreived');
            findById(token.sub, function(err, user) {
                if (err) {
                    return done(err);
                }
                if (!user) {
                    // "Auto-registration"
                    log.info('User was added automatically as they were new. Their sub is: ', token.sub);
                    users.push(token);
                    owner = token.sub;
                    return done(null, token);
                }
                owner = token.sub;
                return done(null, user, token);
            });
        }
    );

    passport.use(bearerStrategy);
```

A Passport az összes stratégia (Twitter-, Facebook-on, és így tovább), amely stratégiafejlesztő a hasonló mintát alkalmaz. Hello stratégia megnézi, láthatja azt adja át azt egy függvényt, amely rendelkezik egy jogkivonat és egy kész hello paraméterekként. hello stratégia előre vissza toous, miután a tevékenységeket végez el. Igen, miután tároljuk hello felhasználói és stash hello jogkivonat, nem szükséges tooask azt újra.

> [!IMPORTANT]
> hello előző kód minden olyan felhasználó, akkor fordul elő, tooauthenticate tooour kiszolgáló vesz igénybe. Ez az úgynevezett automatikus regisztráció. Az üzemi kiszolgálók, azt javasoljuk, ne engedélyezze, hogy bárki, aki nélkül őket keresse meg, amelyeket a regisztrációs folyamatot. Ez általában akkor hello megoldást látjuk a fogyasztói alkalmazásokba, lehetővé teszi a Facebook tooregister, de majd megkérdezi, hogy toofill meg további információkat. Ha a cserét nem parancssori program, kinyerhettük volna hello e-mailt a hello jogkivonat-objektumból, amely adott vissza, és felkérhettük volna hello felhasználói toofill meg további információkat. Mivel ez egy tesztkiszolgálón, egyszerűen hozzáadjuk a őket toohello memóriában lévő adatbázishoz.
>
>

### <a name="protect-some-endpoints"></a>Egyes végpontok védelme
Védelmet biztosít a végpontoknak hello megadásával `passport.authenticate()` hívás hello protokoll, amelyet az toouse.

a kiszolgálóoldali kódban tegye valami további érdekes toomake most Szerkesztés hello útvonal.

```Javascript
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="step-19-run-your-server-application-again-and-ensure-it-rejects-you"></a>19. lépés: Futtassa újra a kiszolgálóalkalmazás, és győződjön meg arról, hogy elutasítja
Most használja `curl` újra toosee szemben a végpontok most megfelelő OAuth2-védelem. Ez a vizsgálat előtt futtató az ügyfél-SDK-kat a végponton végezzük. hello visszakapott fejlécek alapján legyen elég tootell, ha az oktatóanyagban módosítjuk hello jó úton jár le.

1. Először is győződjön meg arról, hogy fut-e a mongoDB-példány:

    `$sudo mongod`

2. Ezután módosítsa toohello könyvtárat, és sütővasak start.

      `$ cd azuread` `$ node server.js`

3. Próbálja meg egy egyszerű POST MŰVELETTEL.

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    ```Shell
    HTTP/1.1 401 Unauthorized
    Connection: close
    WWW-Authenticate: Bearer realm="Users"
    Date: Tue, 14 Jul 2015 05:45:03 GMT
    Transfer-Encoding: chunked
    ```

A 401-es hello választ keres itt. Ez a válasz azt jelzi, hogy a hello Passport réteg próbál tooredirect toohello engedélyezett végpontot, amely pontosan mit szeretne.

## <a name="next-steps"></a>Következő lépések
Ön már megtettünk mindent, ami az OAuth2-kompatibilis ügyfél használata nélkül is ezen a kiszolgálón. Szüksége lesz egy további forgatókönyv keresztül toogo.

Most már tudja, hogyan tooimplement segítségével egy REST API Restify és OAuth2. Azt is, hogy a szolgáltatás fejlesztés és megismerését több mint elég kód tookeep hogyan ebben a példában a toobuild.

Ha érdekli hello lépések a a ADAL használatában, az alábbiakban néhány azt javasoljuk, hogy tovább dolgozik a támogatott ADAL ügyfelek.

Lefelé tooyour fejlesztői gép klónozását, és hello forgatókönyv megadottak szerint.

[Az IOS-es ADAL](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios)

[ADAL androidhoz](https://github.com/MSOpenTech/azure-activedirectory-library-for-android)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
