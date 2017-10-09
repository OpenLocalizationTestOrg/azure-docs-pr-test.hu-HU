---
title: "az Azure Active Directory v2.0 webes API-k aaaSecure Node.js használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan toobuild egy Node.js webes API, a személyes Microsoft-fiók pedig a munkahelyi vagy iskolai fiókok jogkivonatokat fogad el."
services: active-directory
documentationcenter: nodejs
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 0b572fc1-2aaf-4cb6-82de-63010fb1941d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 05/13/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 219e324cca11e107186b7e5f995589b9260af8a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="secure-a-web-api-by-using-nodejs"></a>Biztonságos webes API-k számára a Node.js segítségével
> [!NOTE]
> Nem minden Azure Active Directory forgatókönyvek és funkciók együttműködve hello v2.0-végponttól. toodetermine kell használnia a v2.0-végponttól hello vagy hello 1.0-s verziójú végpontot, hogy olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).
> 
> 

Azure Active Directory (Azure AD) v2.0-végponttól hello használata esetén használhatja [OAuth 2.0](active-directory-v2-protocols.md) hozzáférési jogkivonatok tooprotect a webes API-t. OAuth 2.0 hozzáférési jogkivonatok, felhasználók, akik egy személyes Microsoft-fiók és a munkahelyi vagy iskolai fiókok biztonságosan elérhet a webes API-t.

A *Passport* a Node.js-hez készült közbenső hitelesítési szoftver. Rugalmas és, moduláris Passport diszkréten eldobása minden Express-alapú vagy restify webalkalmazás. Passport stratégiák széles választékát támogatja a hitelesítést egy felhasználónév és jelszó, Facebook-on, Twitter vagy egyéb beállítások használatával. Kidolgoztunk egy stratégiát, amellyel a szoftver az Azure AD esetében is felhasználható. Ebben a cikkben megmutatjuk, hogyan tooinstall hello modul, és adja hozzá az Azure AD hello `passport-azure-ad` beépülő modult.

## <a name="download"></a>Letöltés
az oktatóanyag kódjának hello kezelt [a Githubon](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs). toofollow hello oktatóanyagban is [hello alkalmazás vázát .zip fájl letöltése](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/skeleton.zip), vagy a Klónozás hello vázat:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

Ez az oktatóanyag végén hello befejeződött hello alkalmazás is beszerezheti.

## <a name="1-register-an-app"></a>1: az alkalmazás regisztrálása
Hozzon létre egy új alkalmazást [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy hajtsa végre a [a részletes lépéseket](active-directory-v2-app-registration.md) tooregister egy alkalmazást. Ellenőrizze, hogy:

* Másolás hello **alkalmazásazonosító** tooyour app rendelve. Szüksége lesz az ehhez az oktatóanyaghoz.
* Adja hozzá a hello **Mobile** platform az alkalmazásra vonatkozóan.
* Másolás hello **átirányítási URI-** hello portálról. Hello alapértelmezett URI azonosító értékét kell használnia `urn:ietf:wg:oauth:2.0:oob`.

## <a name="2-install-nodejs"></a>2: telepítse a Node.js-
Ebben az oktatóanyagban toouse hello példában kell [telepítse a Node.js-](http://nodejs.org).

## <a name="3-install-mongodb"></a>3: a MongoDB telepítése
toosuccessfully Ez a minta használatához be kell [a MongoDB telepítése](http://www.mongodb.org). Ez a példa MongoDB toomake a REST API-t használja az állandó különböző kiszolgálópéldányok közötti.

> [!NOTE]
> Ez a cikk azt feltételezzük, hogy mongodb alapértelmezett telepítését és kiszolgálóvégpontjait hello használata: mongodb://localhost.
> 
> 

## <a name="4-install-hello-restify-modules-in-your-web-api"></a>4: telepítés hello restify-modulok a webes API
Használjuk Resitfy toobuild REST API-n. A restify egy minimális igényű és rugalmas Node.js alkalmazási keretrendszer származó Express van. A restify tartozik egy hatékony funkciókat, amelyeket felhasználhat toobuild REST API-k létrehozásához.

### <a name="install-restify"></a>A restify telepítése
1.  A parancssorban módosítsa hello könyvtárat túl**azuread**:

    `cd azuread`

    Ha hello **azuread** könyvtár nem létezik, hozza létre:

    `mkdir azuread`

2.  A restify telepítése:

    `npm install restify`

    a parancs kimenetének hello kell kinéznie:

    ```
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
    └── bunyan@0.22.0(mv@0.0.5)
    ```

#### <a name="did-you-get-an-error"></a>Hibaüzenetet kapott?
Egyes operációs rendszerek, hello használatakor `npm` parancsban, láthatja, hogy ez az üzenet: `Error: EPERM, chmod '/usr/local/bin/..'`. hello hiba követi, hogy rendszergazdaként futó hello fiók megpróbál kérelmet. Ha ez történik, a hello parancs `sudo` toorun `npm` magasabb jogosultsági szinten.

#### <a name="did-you-get-an-error-related-toodtrace"></a>Vonatkozó hibaüzenetet kapott tooDTrace?
Amikor telepíti a restify, láthatja, hogy ez az üzenet:

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: two
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

A restify egy rendkívül erős eszköz tootrace REST meghívja a DTrace segítségével rendelkezik. DTrace azonban számos operációs rendszeren nem érhető el. Ez a hibaüzenet nyugodtan figyelmen kívül hagyhatja.


## <a name="5-install-passportjs-in-your-web-api"></a>5: telepítés Passport.js a webes API
1.  Hello parancssorban módosítsa hello könyvtárat túl**azuread**.

2.  Passport.js telepítése:

    `npm install passport`

    hello parancs kimenetében hello kell kinéznie:

    ```
     passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3
    ```

## <a name="6-add-passport-azure-ad-tooyour-web-api"></a>6: a passport-azure-ad tooyour webes API hozzáadása
Ezután adja hozzá a hello OAuth stratégiát, a passport-azuread használatával. `passport-azuread`a stratégiacsomag szolgál, amely az Azure AD connect a Passport van. Ez a stratégia használható a tulajdonosi jogkivonatokhoz a REST API minta használjuk.

> [!NOTE]
> Bár az OAuth 2.0-s, amelyben bármely ismert jogkivonat típus kibocsáthatja keretet biztosít, néhány gyakran használják. Tulajdonosi jogkivonatok olyan gyakran használt tooprotect végpontok. Tulajdonosi jogkivonatok legszélesebb körben kiadott hello lexikális elem szerepel az OAuth 2.0 típusú. Sok OAuth 2.0 típusú hitelesítés megvalósításához feltételezik, hogy hello csak ilyen típusú kiállított jogkivonat tulajdonosi jogkivonatoknak nevezzük.
> 
> 

1.  A parancssorban módosítsa hello könyvtárat túl**azuread**.

    `cd azuread`

2.  Telepítse a hello Passport.js `passport-azure-ad` modul:

    `npm install passport-azure-ad`

    hello parancs kimenetében hello kell kinéznie:

    ```
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
    ```

## <a name="7-add-mongodb-modules-tooyour-web-api"></a>7: vegye fel a MongoDB modulok tooyour webes API
Ez a példa azt a mongodb az adattárban. 

1.  Telepítse a mongoose bővítményt, széles körben használt beépülő modult, toomanage modellek és sémák: 

    `npm install mongoose`

2.  Hello adatbázis illesztőprogram telepítése a mongodb, más néven MongoDB:

    `npm install mongodb`

## <a name="8-install-additional-modules"></a>8: további modulok telepítése
Telepítse a szükséges modulokat fennmaradó hello.

1.  A parancssorban módosítsa hello könyvtárat túl**azuread**:

    `cd azuread`

2.  Adja meg a következő parancsok hello. hello parancsok a node_modules könyvtárban modulok a következő hello telepítése:

    *   `npm install crypto`
    *   `npm install assert-plus`
    *   `npm install posix-getopt`
    *   `npm install util`
    *   `npm install path`
    *   `npm install connect`
    *   `npm install xml-crypto`
    *   `npm install xml2js`
    *   `npm install xmldom`
    *   `npm install async`
    *   `npm install request`
    *   `npm install underscore`
    *   `npm install grunt-contrib-jshint@0.1.1`
    *   `npm install grunt-contrib-nodeunit@0.1.2`
    *   `npm install grunt-contrib-watch@0.2.0`
    *   `npm install grunt@0.4.1`
    *   `npm install xtend@2.0.3`
    *   `npm install bunyan`
    *   `npm update`

## <a name="9-create-a-serverjs-file-for-your-dependencies"></a>9: a függőségek Server.js fájl létrehozása
A Server.js fájl tárolja a hello többsége hello funkcióit a webes API-kiszolgálóhoz. Adja hozzá a kódfájl toothis része. Élesben való használat akkor is refactor hello funkciókat a kisebb fájlok, például különálló útvonalak és vezérlők. Ebben a cikkben a Server.js erre a célra használjuk.

1.  A parancssorban módosítsa hello könyvtárat túl**azuread**:

    `cd azuread`

2.  A szerkesztővel az Ön által választott, hozzon létre egy Server.js fájlt. Adja hozzá a következő információk toohello fájl hello:

    ```Javascript
    'use strict';
    /**
    * Module dependencies.
    */
    var util = require('util');
    var assert = require('assert-plus');
    var mongoose = require('mongoose/');
    var bunyan = require('bunyan');
    var restify = require('restify');
    var config = require('./config');
    var passport = require('passport');
    var OIDCBearerStrategy = require('passport-azure-ad').OIDCStrategy;
    ```

3.  Hello fájl mentéséhez. Ekkor visszakerül tooit hamarosan.

## <a name="10-create-a-config-file-toostore-your-azure-ad-settings"></a>10: hozzon létre egy konfigurációs fájl toostore az Azure AD-beállítások
Ez a kódfájl hello konfigurációs paraméterek az Azure AD portálon tooPassport.js a adja át. Ezeket a konfigurációs értékeket létrehozott hello webes API toohello portál hello elején hello cikk felvételekor. Hello kód másolását követően azt ismertetjük, milyen tooput hello értékeket a következő paraméterek közül.

1.  A parancssorban módosítsa hello könyvtárat túl**azuread**:

    `cd azuread`

2.  Hozzon létre egy Config.js fájlt valamelyik szerkesztőben. Adja hozzá a következő információ hello:

    ```Javascript
    // Don't commit this file tooyour public repos. This config is for first-run.
    exports.creds = {
    mongoose_auth_local: 'mongodb://localhost/tasklist', // Your Mongo auth URI goes here.
    issuer: 'https://sts.windows.net/**<your application id>**/',
    audience: '<your redirect URI>',
    identityMetadata: 'https://login.microsoftonline.com/common/.well-known/openid-configuration' // For Microsoft, you should never need toochange this.
    };

    ```



### <a name="required-values"></a>Kötelező értékek

*   **IdentityMetadata**: Ez akkor, ha `passport-azure-ad` hello identitásszolgáltató (IDP) és hello kulcsok toovalidate hello JSON Web Tokens (JWTs) a konfigurációs adatok keresi. Ha az Azure AD használ, valószínűleg nem toochange ezt szeretné.

*   **a célközönség**: az átirányítási URI-t hello portálról.

> [!NOTE]
> Rotálja a kulcsokat rendszeres időközönként. Győződjön meg arról, hogy mindig lekéréses hello "openid_keys" URL-címről, és adott hello alkalmazáshoz hozzáférnek hello Internet.
> 
> 

## <a name="11-add-hello-configuration-tooyour-serverjs-file"></a>11: hello konfigurációs tooyour Server.js fájl felvétele
Az alkalmazás kell tooread hello értékek újonnan létrehozott hello konfigurációs fájlból. Adja hozzá a hello .config kiterjesztésű fájlt kötelező erőforrásként az alkalmazásban. Állítsa be a Config.js hello globális változók toothose.

1.  Hello parancssorban módosítsa hello könyvtárat túl**azuread**:

    `cd azuread`

2.  Nyissa meg a Server.js valamelyik szerkesztőben. Adja hozzá a következő információ hello:

    ```Javascript
    var config = require('./config');
    ```

3.  Adja hozzá egy új szakasz tooServer.js:

    ```Javascript
    // Pass these options in toohello ODICBearerStrategy.
    var options = {
    // hello URL of hello metadata document for your app. Put hello keys for token validation from hello URL found in hello jwks_uri tag in hello metadata.
    identityMetadata: config.creds.identityMetadata,
    issuer: config.creds.issuer,
    audience: config.creds.audience
    };
    // Array toohold signed-in users and hello current signed-in user (owner).
    var users = [];
    var owner = null;
    // Your logger
    var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
    });
    ```

## <a name="12-add-hello-mongodb-model-and-schema-information-by-using-mongoose"></a>12: hello MongoDB modell és séma-információk hozzáadása a Mongoose segítségével
Ezek a fájlok a REST API-szolgáltatás a következő csatlakozzon.

Ebben a cikkben használjuk MongoDB toostore a feladatokat. Ezt a tárgyaljuk *4. lépés*.

Hello Config.js fájl 11 lépésben létrehozott, az adatbázis neve *tasklist*. Amely volt a mongoose_auth_local kapcsolati URL-cím végén hello helyezni. Ez az adatbázis előre a MongoDB-ben nem kell toocreate. hello adatbázis létrehozása a hello először futtassa a kiszolgálóalkalmazás (feltéve, hogy hello adatbázis már nem létezik).

Megadta, hogy rendelkezik hello server melyik MongoDB adatbázis toouse. A következő lépésben toowrite néhány további kódrészletet toocreate hello modell és séma a kiszolgálói feladatokhoz.

### <a name="hello-model"></a>hello modell
hello sémamodell nagyon egyszerű. Ha szeretné bővítheti. 

hello séma modellje ezeket az értékeket:

*   **NÉV**. hello személy hozzárendelt toohello feladat. Ez egy **karakterlánc** érték.
*   **A FELADAT**. hello hello feladat nevét. Ez egy **karakterlánc** érték.
*   **DÁTUM**. hello dátum hello tevékenység esedékessé válik. Ez egy **datetime** érték.
*   **BEFEJEZŐDÖTT**. E hello feladat befejeződött. Ez egy **logikai** érték.

### <a name="create-hello-schema-in-hello-code"></a>Hozzon létre hello séma hello kódban
1.  A parancssorban módosítsa hello könyvtárat túl**azuread**:

    `cd azuread`

2.  A szerkesztő nyissa meg a Server.js. Hello konfigurációs bejegyzés alatt adja hozzá a következő információ hello:

    ```Javascript
    // MongoDB setup.
    // Set up some configuration.
    var serverPort = process.env.PORT || 8080;
    var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
    // Connect tooMongoDB.
    global.db = mongoose.connect(serverURI);
    var Schema = mongoose.Schema;
    log.info('MongoDB Schema loaded');
    ```

Ez a kód toohello MongoDB-kiszolgálóhoz csatlakozik. Emellett egy séma-objektumot ad vissza.

#### <a name="using-hello-schema-create-your-model-in-hello-code"></a>A modell hello séma használatával hozhat létre hello kódban
Kód megelőző hello, alá adja hozzá a következő kód hello:

```Javascript
// Create a basic schema toostore your tasks and users.
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

Hello kódból állapítható meg, mert először hoz létre a sémát. A következő modell-objektumot hoz létre. Hello modell objektum toostore az adatok teljes hello code definiálásakor használja a **útvonalak**.

## <a name="13-add-your-routes-for-your-task-rest-api-server"></a>13: a feladat REST API-kiszolgálóhoz a mutató útvonalak hozzáadása
Most, hogy egy adatbázis-modell toowork rendelkező, adja hozzá a hello útvonalakat a REST API-kiszolgálóhoz használni.

### <a name="about-routes-in-restify"></a>Az útvonalakra vonatkozó restify
Az útvonalak restify pontosan hello használatakor tehetik ugyanúgy hello Express verem munka. Útvonalak megadása hello hello ügyfél alkalmazások toocall várható URI segítségével. Az útvonalakat általában külön fájlban definiálni. Ebben az oktatóanyagban a Server.js az útvonalak elhelyezni azt. Éles környezetben való használathoz javasoljuk a saját fájlba útvonalak figyelembe.

A restify-útvonal egy tipikus mintája a következő:

```Javascript
function createObject(req, res, next) {
// Do work on object.
_object.name = req.params.object; // Passed value is in req.params under object.
///...
return next(); // Keep hello server going.
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```


Ez a hello mintát hello legalapvetőbb szinten. A restify (és az Express) sokkal mélyebb funkciók, például a hello képességét toodefine alkalmazástípusok és összetett útválasztás különböző végpontok között adja meg.

#### <a name="add-default-routes-tooyour-server"></a>Alapértelmezett útvonalak tooyour kiszolgáló hozzáadása
Adja hozzá a hello alapszintű CRUD-útvonalakat: **létrehozása**, **beolvasása**, **frissítése**, és **törlése**.

1.  A parancssorban módosítsa hello könyvtárat túl**azuread**:

    `cd azuread`

2.  Nyissa meg a Server.js valamelyik szerkesztőben. Az alábbiakban hello korábban létrehozott adatbázis-bejegyzések, adja hozzá a következő információ hello:

    ```Javascript
    /**
    *
    * APIs for your REST task server
    */
    // Create a task.
    function createTask(req, res, next) {
    // Resitify currently has a bug that doesn't allow you tooset default headers.
    // These headers comply with CORS, and allow you toouse MongoDB Server as your response tooany origin.
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    // Create a new task model, fill it, and save it tooMongoDB.
    var _task = new Task();
    if (!req.params.task) {
    req.log.warn({
    params: p
    }, 'createTodo: missing task');
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
    /// Returns hello list of TODOs that were loaded.
    function listTasks(req, res, next) {
    // Resitify currently has a bug that doesn't allow you tooset default headers.
    // These headers comply with CORS, and allow us toouse MongoDB Server as our response tooany origin.
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    log.info("listTasks was called for: ", owner);
    Task.find({
    owner: owner
    }).limit(20).sort('date').exec(function(err, data) {
    if (err)
    return next(err);
    if (data.length > 0) {
    log.info(data);
    }
    if (!data.length) {
    log.warn(err, "There are no tasks in hello database. Add one!");
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

### <a name="add-error-handling-for-hello-routes"></a>Hibakezelés hello útvonalak beállítása
Hibakezelést, képes kommunikálni a hátsó toohello ügyfél hello tapasztalt problémáról.

Adja hozzá a következő alábbi hello kód, amely korábban már írt kódot hello:

```Javascript
///--- Errors for communicating something more information back toohello client.
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


## <a name="14-create-your-server"></a>14: a kiszolgáló létrehozása
utolsó lépésként toodo hello tooadd a server-példány van. hello server-példány kezeli a hívásokat.

A restify (és az Express) részletes testreszabási, melyekkel egy REST API-kiszolgálóval rendelkezik. Ebben az oktatóanyagban hello legalapvetőbb telepítő használjuk.

```Javascript
/**
* Your server
*/
var server = restify.createServer({
name: "Microsoft Azure Active Directory TODO Server",
version: "2.0.1"
});
// Ensure that you don't drop data on uploads.
server.pre(restify.pre.pause());
// Clean up imprecise paths like //todo//////1//.
server.pre(restify.pre.sanitizePath());
// Handle annoying user agents (curl).
server.pre(restify.pre.userAgentConnection());
// Set a per-request Bunyan logger (with requestid filled in).
server.use(restify.requestLogger());
// Allow 5 requests/second by IP address, and burst too10.
server.use(restify.throttle({
burst: 10,
rate: 5,
ip: true,
}));
// Use common commands, such as:
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
mapParams: true
}));
```
## <a name="15-add-hello-routes-without-authentication-for-now"></a>15: Adjon hozzá hello útvonalakat (hitelesítés nélkül, a lépést)
```Javascript
/// Use CRUD tooadd hello real handlers.
/**
/*
/* Each of these handlers is protected by your Open ID Connect Bearer strategy. Invoke 'oidc-bearer'
/* in hello pasport.authenticate() method. Because REST is stateless, set 'session: false'. You 
/* don't need toomaintain session state. You can experiment with removing API protection.
/* toodo this, remove hello passport.authenticate() method:
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
// Register a default '/' handler
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
## <a name="16-run-hello-server"></a>16: hello server futtatása
Egy jó ötlet tootest a kiszolgáló hitelesítési hozzáadása előtt.

hello legegyszerűbb módja tootest a kiszolgáló van, a parancssorba curl használatával. toodo, ez egy egyszerű segédprogramra használható tooparse kimeneti adatok JSON-ként van szüksége. 

1.  Telepítse a következő példák hello hello JSON-segédprogramot, amely használjuk:

    `$npm install -g jsontool`

    Ezzel globálisan telepíti a hello JSON-segédprogramot.

2.  Ellenőrizze, hogy fut-e a MongoDB-példány:

    `$sudo mongod`

3.  Hello könyvtárváltás túl**azuread**, majd futtassa a curl:

    `$ cd azuread`
    `$ node server.js`

    `$ curl -isS http://127.0.0.1:8080 | json`

    ```Shell
    HTTP/1.1 2.0OK
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

4.  egy feladat tooadd:

    `$ curl -isS -X POST http://127.0.0.1:8888/tasks/brandon/Hello`

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

5.  Lista feladatok Brandon:

    `$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Ha ezek a parancsok futnak, nem jelenik meg hibaüzenet, készen áll a tooadd OAuth toohello REST API-kiszolgálóhoz is.

*Most már rendelkezik a MongoDB-vel futó REST API-kiszolgáló!*

## <a name="17-add-authentication-tooyour-rest-api-server"></a>17: hitelesítési tooyour REST API-kiszolgáló hozzáadása
Most, hogy a futó REST API-t, állítsa be toouse az Azure AD-val.

A parancssorban módosítsa hello könyvtárat túl**azuread**:

`cd azuread`

### <a name="use-hello-oidcbearerstrategy-thats-included-with-passport-azure-ad"></a>Hello részét képező képező oidcbearerstrategy használata a passport-azure-ad
Az eddigi létrehozott egy tipikus REST TODO kiszolgálót hitelesítés nélkül. Ezután adja hozzá a hitelesítést.

Először adja meg, hogy toouse Passport. Ez a jogosultság helyezze a korábbi kiszolgáló konfigurálása után:

```Javascript
// Start using Passport.js.

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support
```

> [!TIP]
> API-k írásakor egy jó ötlet tooalways hivatkozás hello adatok toosomething egyedi felhasználói hello hello jogkivonatból nem tudnak jogosulatlanul hozzáférni. Ha ez a kiszolgáló tárolja a TODO elemeket tárolja őket (más néven token.sub keresztül) hello token hello felhasználói előfizetés-azonosító alapján. Hello token.sub be hello "owner" mezőjében. Ez biztosítja, hogy csak adott felhasználó hozzáférhet-e hello felhasználói TODOs. A megadott hello TODOs senki más nem érheti el. Nem jelenik meg sehol van hello API a "tulajdonos". A külső felhasználó kérhet más felhasználók TODOs, még akkor is, ha hitelesített.
> 
> 

Következő lépésként az hello nyitott ID Connect tulajdonosi stratégia `passport-azure-ad`. Be az alábbiakat mi beillesztett korábban:

```Javascript
/**
/*
/* Calling hello OIDCBearerStrategy and managing users.
/*
/* Because of hello Passport pattern, you need toomanage users and info tokens
/* with a FindorCreate() method. hello method must be provided by hello implementor.
/* In hello following code, you autoregister any user and implement a FindById().
/* It's a good idea toodo something more advanced.
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
var oidcStrategy = new OIDCBearerStrategy(options,
function(token, done) {
log.info('verifying hello user');
log.info(token, 'was hello token retrieved');
findById(token.sub, function(err, user) {
if (err) {
return done(err);
}
if (!user) {
// "Auto-registration"
log.info('User was added automatically, because they were new. Their sub is: ', token.sub);
users.push(token);
owner = token.sub;
return done(null, token);
}
owner = token.sub;
return done(null, user, token);
});
}
);
passport.use(oidcStrategy);
```

A Passport hasonló mintát alkalmaz az összes stratégia (Twitter-, Facebook-on, és így tovább). Stratégiafejlesztő toohello mintát. Hello stratégia átadni egy `function()` tokent használ, és `done` paraméterekként. hello stratégia ad vissza, miután a tevékenységeket végez el. Hello felhasználói és stash hello token tárolására, így nem kell tooask azt újra.

> [!IMPORTANT]
> hello előző kód készít minden olyan felhasználó, amely képes hitelesíteni tooyour kiszolgáló. Ez az úgynevezett automatikus regisztráció. Az üzemi kiszolgálón általában csak akkor érdemes toolet bárki, aki nélkül őket nyissa meg az Ön által regisztrációs folyamatot. Ez általában akkor hello megoldást látjuk a fogyasztói alkalmazásokba. hello alkalmazást is lehetővé teheti a Facebook tooregister, de majd kéri tooenter további információt. Ha ez az oktatóanyag nem parancssori program alkalmaz, objektumból hello token visszaadott lehetett kibontani a hello e-mail. Ezt követően előfordulhat, hogy kérje meg hello felhasználói tooenter további információt. Mivel ez egy tesztkiszolgálón, hozzáadhat hello felhasználó közvetlenül a toohello memóriában lévő adatbázishoz.
> 
> 

### <a name="protect-endpoints"></a>A végpontok védelme
A végpontok védelme hello megadásával **passport.authenticate()** hívás hello protokoll, amelyet az toouse.

Az útvonal egy speciális beállítás a kiszolgáló kódjában található módosíthatja:

```Javascript
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oidc-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="18-run-your-server-application-again"></a>18: futtassa újra a kiszolgálóalkalmazás
Használata curl újra toosee Ha OAuth 2.0 védelmi e a végpontokon. Ehhez minden az ügyfél-SDK-kat a végponton futtatása előtt. visszaadott hello fejlécek kell tájékoztatnia, ha a hitelesítés megfelelően működik-e.

1.  Győződjön meg arról, hogy fut-e a MongoDB isntance:

    `$sudo mongod`

2.  Módosítsa a toohello **azuread** könyvtárra, és használata curl:

    `$ cd azuread`

    `$ node server.js`

3.  Próbálkozzon meg egy egyszerű POST művelettel:

    `$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

    ```Shell
    HTTP/1.1 401 Unauthorized
    Connection: close
    WWW-Authenticate: Bearer realm="Users"
    Date: Tue, 14 Jul 2015 05:45:03 GMT
    Transfer-Encoding: chunked
    ```

A 401-es válasz azt jelzi, hogy a hello Passport réteg próbál tooredirect toohello engedélyezik a végponthoz, vagyis pontosan mit szeretne.

*Most már rendelkezik egy REST API-szolgáltatás által használt OAuth 2.0!*

Ön már megtettünk mindent, ami az OAuth 2.0-kompatibilis ügyfél használata nélkül is ezen a kiszolgálón. Az adott szüksége lesz egy további oktatóanyag tooreview.

## <a name="next-steps"></a>Következő lépések
Befejeződött hello mintát (a konfigurációs értékek nélkül) referenciaként biztosított [egy .zip fájl](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs/archive/complete.zip). Akkor is klónozhatja azt a Githubról:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-nodejs.git```

Most áthelyezheti a témakörök speciális toomore. Érdemes lehet tootry [hello v2.0-végponttól használata Node.js-webalkalmazás biztonságos](active-directory-v2-devquickstarts-node-web.md).

Az alábbiakban néhány további források:

* [Az Azure AD v2.0 fejlesztői útmutató](active-directory-appmodel-v2-overview.md)
* [A verem túlcsordulás "azure-active-directory" címke](http://stackoverflow.com/questions/tagged/azure-active-directory)

### <a name="get-security-updates-for-our-products"></a>Biztonsági frissítések termékeinkhez
Javasoljuk toosign biztonsági incidensek értesítendő toobe fel. A hello [Microsoft műszaki biztonsági értesítéseket](https://technet.microsoft.com/security/dd252948) előfizetés tooSecurity tanácsadók riasztások lapján.

