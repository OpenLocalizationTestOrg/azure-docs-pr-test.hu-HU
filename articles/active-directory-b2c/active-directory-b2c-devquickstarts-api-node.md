---
title: "Azure AD B2C: Védelem biztosítása webes API-k számára a Node.js segítségével | Microsoft Docs"
description: "Hogyan toobuild egy Node.js webes API-t, amely a B2C-bérlő származó jogkivonatokat fogad el"
services: active-directory-b2c
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: fc2b9af8-fbda-44e0-962a-8b963449106a
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: hero-article
ms.date: 01/07/2017
ms.author: xerners
ms.openlocfilehash: 47f5bae025a9ba2f486e36acef36aa37cfb43543
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-secure-a-web-api-by-using-nodejs"></a>Azure AD B2C: Védelem biztosítása webes API-k számára a Node.js segítségével
<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Az Azure Active Directory (Azure AD) B2C segítségével védetté tehet egy webes API-t OAuth 2.0 hozzáférési jogkivonatok használatával. Ezek a jogkivonatok engedélyezik az Azure AD B2C tooauthenticate toohello API-t használó ügyfél alkalmazásait. Ez a cikk bemutatja, hogyan toocreate egy "Feladatlista" API-t, amely lehetővé teszi a felhasználók tooadd és lista feladatokat. hello webes API-t az Azure AD B2C használatával lett biztonságossá téve, és csak lehetővé teszi a hitelesített felhasználók toomanage a tennivalók listájára.

> [!NOTE]
> Ez a minta csatlakoztatott toobe tooby használatával íródott a [iOS B2C mintaalkalmazással](active-directory-b2c-devquickstarts-ios.md). Aktuális útmutató hello először, és hajtsa végre mintaalkalmazással.
>
>

A **Passport** a Node.js-hez készült közbenső hitelesítési szoftver. A rugalmasan működő, moduláris Passport gyakorlatilag bármely Express- vagy Restify-alapú webalkalmazásba diszkréten telepíthető. A program számos különböző lehetőséget kínál a felhasználók hitelesítésére: felhasználónév/jelszó, Facebook- vagy Twitter-fiók és így tovább. Kidolgoztunk egy stratégiát, amellyel a szoftver az Azure Active Directory (Azure AD) esetében is felhasználható. Telepítenie kell a modult, és adja meg az Azure AD hello `passport-azure-ad` beépülő modult.

toodo ezt a mintát kell:

1. Alkalmazás regisztrálása az Azure AD-ben.
2. Állítsa be az alkalmazás toouse Passport által `azure-ad-passport` beépülő modult.
3. Ügyfél alkalmazás toocall hello "Feladatlista" webes API-k konfigurálása.

## <a name="get-an-azure-ad-b2c-directory"></a>Az Azure AD B2C-címtár beszerzése
Az Azure AD B2C használatához létre kell hoznia egy címtárat vagy bérlőt.  A címtárban tárolhatja az összes felhasználót, alkalmazást, csoportot és más elemeket.  Ha még nem tette meg, [hozzon létre most egy B2C-címtárat](active-directory-b2c-get-started.md), mielőtt továbblépne.

## <a name="create-an-application"></a>Alkalmazás létrehozása
A következő lépésben egy alkalmazást a B2C-címtárban, amely ellátja az Azure AD néhány információt, hogy kell-e toosecurely kommunikációhoz toocreate az alkalmazással. Ebben az esetben hello ügyfélalkalmazást és a webes API jelöli egyetlen **Alkalmazásazonosító**, mert alkalmazássá logikai. hajtsa végre az alkalmazást, és egy toocreate [ezeket az utasításokat](active-directory-b2c-app-registration.md). Ügyeljen arra, hogy:

* Tartalmaznak egy **webalkalmazást vagy webes api** hello alkalmazásban
* A **Reply URL** (Válasz URL-cím) legyen a következő: `http://localhost/TodoListService`. A fenti hello alapértelmezett URL-.
* Hozzon létre egy **alkalmazástitkot** az alkalmazáshoz, majd másolja. Erre az adatra később még szükség lesz. Vegye figyelembe, hogy ezt az értéket kell toobe [escape-karaktersorozatot XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) használatba vétel előtt.
* Másolás hello **Alkalmazásazonosító** , amely hozzárendelt tooyour alkalmazást. Erre az adatra később még szükség lesz.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Házirendek létrehozása
Az Azure AD B2C-ben minden felhasználói élményt [házirendek](active-directory-b2c-reference-policies.md) határoznak meg. Ez az alkalmazás két, identitással kapcsolatos műveletet tartalmaz: regisztráció és bejelentkezés. Szüksége van a toocreate egy házirendet az egyes típusok leírtak szerint a [házirendek áttekintésével foglalkozó cikkben](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).  A három szabályzat létrehozásakor ügyeljen arra, hogy:

* Válassza ki a hello **megjelenített név** és az egyéb regisztrációs attribútumokat a regisztrációs szabályzatban.
* Válassza ki a hello **megjelenített név** és **Objektumazonosító** minden házirend alkalmazási jogcímet.  Kiválaszthat egyéb jogcímeket is.
* Másolja le hello **neve** egyes házirendek létrehozása után. Hello előtag nem rendelkezhet `b2c_1_`.  A szabályzatok nevére később még szüksége lesz.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

A három szabályzat létrehozását követően készen áll a toobuild Ön az alkalmazást.

toolearn arról, hogyan házirendek az Azure AD B2C működnek, hello kezdődnie [.NET webalkalmazások használatába bevezető oktatóanyagot](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-hello-code"></a>Hello kód letöltése
Ez az oktatóanyag kód hello [kezelése a Githubon történik](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS). Ugrás toobuild hello mintát, ha Ön is [töltse le a projektvázát tartalmazó .zip-fájlt](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip). Hello vázat klónozhatja is:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS.git
```

befejeződött hello alkalmazás is van [elérhető .zip-fájlként](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) vagy hello `complete` hello ága adott adattár.

## <a name="download-nodejs-for-your-platform"></a>A Node.js letöltése a platformra
toosuccessfully ezt a mintát használja, a Node.js telepített és működő van szüksége.

Telepítse a Node.js-t a [nodejs.org](http://nodejs.org) oldalról.

## <a name="install-mongodb-for-your-platform"></a>A MongoDB telepítése a platformra
toosuccessfully Ez a minta használatához a MongoDB telepített és működő van szüksége. Használjuk MongoDB toomake állandó a REST API különböző kiszolgálópéldányok közötti.

Telepítse a MongoDB-t a [mongodb.org](http://www.mongodb.org) oldalról.

> [!NOTE]
> Ez az útmutató feltételezi, hogy MongoDB, amely írásának hello időpontban hello alapértelmezett telepítését és kiszolgálóvégpontjait használja `mongodb://localhost`.
>
>

## <a name="install-hello-restify-modules-in-your-web-api"></a>Hello Restify-modulok telepítése a webes API
Használjuk Restify toobuild a REST API-t. A Restify egy minimális igényű és rugalmas Node.js alkalmazási keretrendszer, amely az Express alapján készült. Számos hatékony funkciót kínál a Connectre épített REST API-k létrehozásához.

### <a name="install-restify"></a>A Restify telepítése
Hello parancssorból módosítsa a könyvtárat túl`azuread`. Ha hello `azuread` könyvtár nem létezik, hozza létre.

`cd azuread` vagy `mkdir azuread;`

Adja meg a következő parancs hello:

`npm install restify`

Ez a parancs elvégzi a Restify telepítését.

#### <a name="did-you-get-an-error"></a>Hibaüzenetet kapott?
Az egyes operációs rendszerek használatakor `npm`, hello hibaüzenet `Error: EPERM, chmod '/usr/local/bin/..'` és rendszergazdaként futtatni a hello fiók kérelmet. Ha a probléma akkor fordul elő, használja a hello `sudo` parancs toorun `npm` magasabb jogosultsági szinten.

#### <a name="did-you-get-a-dtrace-error"></a>DTrace hibaüzenetet kapott?
Előfordulhat, hogy a Restify telepítése során az alábbihoz hasonló üzenet jelenik meg:

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

A Restify a DTrace segítségével biztosít a REST-hívások követésére szolgáló funkciókat. Számos operációs rendszerben azonban nem található meg a DTrace. Ezeket a hibákat nyugodtan figyelmen kívül hagyhatja.

hello parancs kimenetében hello hasonló toothis szöveget kell megjelennie:

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

## <a name="install-passport-in-your-web-api"></a>A Passport telepítése a webes API-ba
Hello parancssorból módosítsa a könyvtárat túl`azuread`, ha még nem szerepel ott.

A következő parancs hello használata a Passport telepítéséhez:

`npm install passport`

hello parancs kimenetében hello hasonló toothis szöveget kell lennie:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="add-passport-azuread-tooyour-web-api"></a>Adja hozzá a passport-azuread tooyour webes API
Ezután adja hozzá az OAuth stratégiát hello segítségével `passport-azuread`, a stratégiacsomag szolgál, amely az Azure AD connect a Passport. Használja ezt a stratégiát használható a tulajdonosi jogkivonatokhoz hello REST API minta.

> [!NOTE]
> Az OAuth2 keretrendszerében ugyan bármely ismert jogkivonat kibocsátható, csupán néhány típust használnak szélesebb körben. hello végpontok védelmére szolgáló jogkivonatok tulajdonosi jogkivonatoknak nevezzük. Az ilyen típusú jogkivonatok hello legszélesebb körben kiadott OAuth2-ben. Számos implementáció eleve feltételezik, hogy hello csak ilyen típusú kiállított jogkivonat tulajdonosi jogkivonatoknak nevezzük.
>
>

Hello parancssorból módosítsa a könyvtárat túl`azuread`, ha még nem szerepel ott.

Telepítse a hello Passport `passport-azure-ad` moduljának hello a következő parancs használatával:

`npm install passport-azure-ad`

hello parancs kimenetében hello hasonló toothis szöveget kell lennie:

``
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
``

## <a name="add-mongodb-modules-tooyour-web-api"></a>Adja hozzá a MongoDB modulok tooyour webes API
Ebben a példában a MongoDB lesz az adattároló. Ezért telepítenie kell a népszerű Mongoose bővítményt, amely modellek és sémák kezelésére szolgál.

* `npm install mongoose`

## <a name="install-additional-modules"></a>A kiegészítő modulok telepítése
Következő lépésként telepítse a szükséges modulokat fennmaradó hello.

Hello parancssorból módosítsa a könyvtárat túl`azuread`, ha még nem szerepel ott:

`cd azuread`

Hello modulok telepítése a `node_modules` könyvtár:

* `npm install assert-plus`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install express`
* `npm install bunyan`

## <a name="create-a-serverjs-file-with-your-dependencies"></a>A függőségeknek megfelelő server.js fájl létrehozása
Hello `server.js` fájl hello többsége hello funkciókat biztosít a webes API-kiszolgálóhoz.

Hello parancssorból módosítsa a könyvtárat túl`azuread`, ha még nem szerepel ott:

`cd azuread`

Hozza létre a `server.js` fájlt valamelyik szerkesztőben. Adja hozzá a következő információ hello:

```Javascript
'use strict';
/**
* Module dependencies.
*/
var fs = require('fs');
var path = require('path');
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').BearerStrategy;
```

Hello fájl mentéséhez. Később visszatérhet tooit.

## <a name="create-a-configjs-file-toostore-your-azure-ad-settings"></a>A config.js fájl toostore létrehozása az Azure AD-beállítások
Ez a kódfájl adja át a hello konfigurációs paraméterek, az az Azure AD portálon toohello `Passport.js` fájlt. Ezeket a konfigurációs értékeket létrehozott hello webes API toohello portál hello segédlet első része hello való hozzáadásakor. Milyen tooput hello értékeket a következő paraméterek közül azt ismertetik, hello kód másolását követően.

Hello parancssorból módosítsa a könyvtárat túl`azuread`, ha még nem szerepel ott:

`cd azuread`

Hozza létre a `config.js` fájlt valamelyik szerkesztőben. Adja hozzá a következő információ hello:

```Javascript
// Don't commit this file tooyour public repos. This config is for first-run
exports.creds = {
clientID: <your client ID for this Web API you created in hello portal>
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
audience: '<your audience URI>', // hello Client ID of hello application that is calling your API, usually a web API or native client
identityMetadata: 'https://login.microsoftonline.com/<tenant name>/.well-known/openid-configuration', // Make sure you add hello B2C tenant name in hello <tenant name> area
tenantName:'<tenant name>',
policyName:'b2c_1_<sign in policy name>' // This is hello policy you'll want toovalidate against in B2C. Usually this is your Sign-in policy (as users sign in toothis API)
passReqToCallback: false // This is a node.js construct that lets you pass hello req all hello way back tooany upstream caller. We turn this off as there is no upstream caller.
};

```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="required-values"></a>Kötelező értékek
`clientID`: a webes API-alkalmazás Ügyfélazonosítója hello.

`IdentityMetadata`: Ez akkor, ha `passport-azure-ad` keresi a hello identitásszolgáltató konfigurációs adatait. Hello kulcsok toovalidate hello JSON webes jogkivonatainak is keres.

`audience`: hello egységes erőforrás-azonosítója (URI), amely azonosítja a hívó alkalmazás hello portálról.

`tenantName`: A bérlő neve (például **contoso.onmicrosoft.com**).

`policyName`: hello tooyour server érkező toovalidate hello jogkivonatok beállítani kívánt szabályzatot. Ezzel a házirend-érdemes lehet ugyanabban a házirendben a hello ügyfélalkalmazás a használt bejelentkezési hello.

> [!NOTE]
> Most használja hello azonos házirendek közötti ügyfél és a kiszolgáló beállítása. Ha már oktatóanyagok keretében létrehozta ezeket a szabályzatokat, ezért újra toodo nincs szükség. Hello segédlet fejeződött be, mert nem szükséges új szabályzatokat tooset az ügyfelekre hello helyen.
>
>

## <a name="add-configuration-tooyour-serverjs-file"></a>Konfigurációs tooyour server.js fájl felvétele
hello tooread hello értékeinek `config.js` létrehozott fájlt, adja hozzá a hello `.config` fájlt kötelező erőforrásként az alkalmazásban, és utána állítsa be a globális változók toothose hello hello `config.js` dokumentum.

Hello parancssorból módosítsa a könyvtárat túl`azuread`, ha még nem szerepel ott:

`cd azuread`

Nyissa meg hello `server.js` fájlt valamelyik szerkesztőben. Adja hozzá a következő információ hello:

```Javascript
var config = require('./config');
```
Adja hozzá egy új szakaszt túl`server.js` , amely tartalmazza a következő kód hello:

```Javascript
// We pass these options in toohello ODICBearerStrategy.

var options = {
    // hello URL of hello metadata document for your app. We put hello keys for token validation from hello URL found in hello jwks_uri tag of hello in hello metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    tenantName: config.creds.tenantName,
    policyName: config.creds.policyName,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback

};
```

A következő adjunk néhány helyőrzőket a hívó alkalmazásból érkező hello felhasználók számára.

```Javascript
// array toohold logged in users and hello current logged in user (owner)
var users = [];
var owner = null;
```

Most pedig hozzon létre naplózót.

```Javascript
// Our logger
var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="add-hello-mongodb-model-and-schema-information-by-using-mongoose"></a>Hello MongoDB modell és séma-információk hozzáadása a Mongoose segítségével
hello korábbi előkészítő fizet nevén, ezek a fájlok együtt a REST API-szolgáltatás.

Ez az útmutató használja MongoDB toostore a feladatokat, a fentiekben taglaltak.

A hello `config.js` az adatbázis nevű fájl, **tasklist**. Ugyanez a neve egyben hello hello végén put `mongoose_auth_local` kapcsolat URL-címet. Ez az adatbázis előre a MongoDB-ben nem kell toocreate. Az adatbázist hoz létre hello meg hello első futtatáskor a kiszolgálói alkalmazás.

Miután Ön hello server melyik MongoDB adatbázis toouse, szükség van toowrite néhány további kódrészletet toocreate hello modell és séma a kiszolgálói feladatokhoz.

### <a name="expand-hello-model"></a>Bontsa ki a hello modell
Ez a sémamodell egyszerű. Ez azt is jelenti, hogy szükség esetén könnyedén kibővíthető.

`owner`: Toohello feladat ki hozzá van rendelve. Ez egy **karakterlánc**.  

`Text`: maga hello feladat. Ez egy **karakterlánc**.

`date`: hello dátum hello tevékenység esedékessé válik. Ez egy **datetime** (dátum és idő) típusú objektum.

`completed`: Ha hello feladat már befejeződött. Ez egy **logikai** típusú objektum.

### <a name="create-hello-schema-in-hello-code"></a>Hozzon létre hello séma hello kódban
Hello parancssorból módosítsa a könyvtárat túl`azuread`, ha még nem szerepel ott:

`cd azuread`

Nyissa meg hello `server.js` fájlt valamelyik szerkesztőben. Adja hozzá a következő információ hello konfigurációs bejegyzés alatt hello:

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 3000; // Note we are hosting our API on port 3000
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;

// Connect tooMongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema toostore our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    Text: String,
    completed: Boolean,
    date: Date
});

// Use hello schema tooregister a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Először létre kell hoznia hello séma, és majd a modellobjektumot, amelyet használ az adatok teljes hello code definiálásakor toostore hozza létre a **útvonalak**.

## <a name="add-routes-for-your-rest-api-task-server"></a>A REST API-feladatkiszolgálóra mutató útvonalak hozzáadása
Most, hogy egy adatbázis-modell toowork rendelkező, adja hozzá a hello útvonalakat használ a REST API-kiszolgálóhoz.

### <a name="about-routes-in-restify"></a>Az útvonalak működése a Restify programban
Útvonalak használhatók a Restifyban hello azonos úgy, hogy működnek-hello Express verem használata. Útvonalak megadása hello hello ügyfél alkalmazások toocall várható URI segítségével.

Íme egy jellegzetes Restify-útvonal:

```Javascript
function createObject(req, res, next) {
// do work on Object
_object.name = req.params.object; // passed value is in req.params under object
///...
return next(); // keep hello server going
}
....
server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.
```

A Restify és az Express ennél jóval összetettebb funkciók biztosítására is képes: például definiálhatja velük az alkalmazástípusokat, valamint bonyolult útvonalvezetést valósíthat meg a különböző végpontok között. Ez az oktatóanyag hello alkalmazásában azt megőrizni ezeket az útvonalakat egyszerű.

#### <a name="add-default-routes-tooyour-server"></a>Alapértelmezett útvonalak tooyour kiszolgáló hozzáadása
Mostantól felvehet hello alapvető CRUD-útvonalakat: **létrehozása** és **lista** a REST API-n. Más útvonalak hello található `complete` hello minta ága.

Hello parancssorból módosítsa a könyvtárat túl`azuread`, ha még nem szerepel ott:

`cd azuread`

Nyissa meg hello `server.js` fájlt valamelyik szerkesztőben. Hello adatbáziselemek alatt végrehajtott újabb adja hozzá a következő információ hello:

```Javascript
/**
 *
 * APIs for our REST Task server
 */

// Create a task

function createTask(req, res, next) {

    // Resitify currently has a bug which doesn't allow you tooset default headers
    // This headers comply with CORS and allow us toomongodbServer our response tooany origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it up and save it tooMongodb
    var _task = new Task();

    if (!req.params.Text) {
        req.log.warn({
            params: req.params
        }, 'createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.Text = req.params.Text;
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
```

```Javascript
/// Simple returns hello list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Resitify currently has a bug which doesn't allow you tooset default headers
    // This headers comply with CORS and allow us toomongodbServer our response tooany origin

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
            log.warn(err, "There is no tasks in hello database. Add one!");
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


#### <a name="add-error-handling-for-hello-routes"></a>Hibakezelés hello útvonalak beállítása
Adjon hozzá néhány hibakezelés, így képes kommunikálni a hátsó toohello ügyfél úgy, hogy az képes megérteni felmerülő problémákat.

Adja hozzá a következő kód hello:

```Javascript
///--- Errors for communicating something interesting back toohello client
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


## <a name="create-your-server"></a>A kiszolgáló létrehozása
Készen áll az adatbázis, és az útvonalak is a helyükön vannak. utolsó lépésként hello meg toodo tooadd hello server-példány a hívásokat kezelő.

A restify és Express adja meg a részletes testreszabási egy REST API-kiszolgálóhoz, de itt használjuk hello most az alapszintű beállításokat.

```Javascript

/**
 * Our Server
 */


var server = restify.createServer({
    name: "Microsoft Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//
server.pre(restify.pre.sanitizePath());

// Handles annoying user agents (curl)
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in)
server.use(restify.requestLogger());

// Allow 5 requests/second by IP, and burst too10
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use hello common stuff you probably want
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allows for JSON mapping tooREST
server.use(restify.authorizationParser()); // Looks for authorization headers

// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support


```
## <a name="add-hello-routes-toohello-server-without-authentication"></a>Adja hozzá a hello útvonalak toohello kiszolgálóhoz (hitelesítés nélkül)
```Javascript
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.head('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.post('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.post('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.del('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeAll, function respond(req, res, next) {
    res.send(204);
    next();
});


// Register a default '/' handler

server.get('/', function root(req, res, next) {
    var routes = [
        'GET     /',
        'POST    /api/tasks/:owner/:task',
        'POST    /api/tasks (for JSON body)',
        'GET     /api/tasks',
        'PUT     /api/tasks/:owner',
        'GET     /api/tasks/:owner',
        'DELETE  /api/tasks/:owner/:task'
    ];
    res.send(200, routes);
    next();
});
```

```Javascript

server.listen(serverPort, function() {

    var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
    consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
    consoleMessage += '\n %s server is listening at %s';
    consoleMessage += '\n Open your browser too%s/api/tasks\n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
    consoleMessage += '\n !!! why not try a $curl -isS %s | json tooget some ideas? \n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';

    //log.info(consoleMessage, server.name, server.url, server.url, server.url);

});

```

## <a name="add-authentication-tooyour-rest-api-server"></a>Hitelesítési tooyour REST API-kiszolgáló hozzáadása
Most, hogy rendelkezésére áll a futó REST API-kiszolgáló, felhasználhatja azt az Azure AD-ben is.

Hello parancssorból módosítsa a könyvtárat túl`azuread`, ha még nem szerepel ott:

`cd azuread`

### <a name="use-hello-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>Hello részét passport-azure-ad részét képező OIDCBearerStrategy használata
> [!TIP]
> Amikor ír API-k, mindig csatolja hello adatok toosomething egyedi felhasználói hello hello jogkivonatból nem tudnak jogosulatlanul hozzáférni. Hello kiszolgáló tárolja a ToDo elemeket hajtja hello alapján **oid** hello felhasználó hello jogkivonatot (a token.OID), amely a "hello"owner"mezőjében. Ez az érték biztosítja, hogy csak az adott felhasználó férhet hozzá a saját ToDo elemeihez. Nincs nem jelenik meg sehol hello API "tulajdonos", a külső felhasználó kérhet mások ToDo elemeit, még akkor is, ha hitelesített.
>
>

Következő lépésként az hello tulajdonosi stratégia `passport-azure-ad`.

```Javascript
var findById = function(id, fn) {
    for (var i = 0, len = users.length; i < len; i++) {
        var user = users[i];
        if (user.oid === id) {
            log.info('Found user: ', user);
            return fn(null, user);
        }
    }
    return fn(null, null);
};


var oidcStrategy = new OIDCBearerStrategy(options,
    function(token, done) {
        log.info('verifying hello user');
        log.info(token, 'was hello token retreived');
        findById(token.sub, function(err, user) {
            if (err) {
                return done(err);
            }
            if (!user) {
                // "Auto-registration"
                log.info('User was added automatically as they were new. Their sub is: ', token.oid);
                users.push(token);
                owner = token.oid;
                return done(null, token);
            }
            owner = token.sub;
            return done(null, user, token);
        });
    }
);

passport.use(oidcStrategy);
```

A Passport a stratégiákhoz hello azonos mintát használ. Át kell neki adni egy `function()` elemet, amelynek paramétereihez tartozik a `token` és a `done`. hello stratégia ismét elérhető lesz tooyou után fordult elő az összes teendőit. Kell majd hello felhasználói tároljuk, és mentse a hello jogkivonat, így nem kell tooask azt újra.

> [!IMPORTANT]
> hello a fenti kódrészlettel áteső felhasználók bejuthatnak tooauthenticate tooyour kiszolgáló. Ezt a folyamatot automatikus regisztrációnak nevezzük. Az üzemi kiszolgálók nem lehetővé teszik a felhasználók hozzáférést hello API-k nélkül nyissa meg a regisztrációs folyamat őket. Ez a folyamat az általában hello megoldást látjuk a fogyasztói alkalmazásoknál, amelyek lehetővé teszik tooregister Facebook-fiókkal, majd megkérdezi, hogy toofill meg további információkat. Ha a program nem parancssori program, kinyerhettük volna hello e-mailt a hello jogkivonat-objektumból, amely adott vissza, és felkérhettük volna a felhasználók toofill meg további információkat. Mivel ez egy minta, azt hozzá tooan memóriában lévő adatbázishoz.
>
>

## <a name="run-your-server-application-tooverify-that-it-rejects-you"></a>A server application tooverify futtassa, hogy elutasítja-e Önt
Használhat `curl` toosee, ha már rendelkezik az OAuth2-védelem e a végpontokon. hello visszaadott fejlécek legyen elég tootell, hogy a hello helyes elérési útjával.

Ellenőrizze, hogy fut-e a MongoDB-példány:

    $sudo mongodb

Toohello könyvtár és futtatási hello kiszolgáló módosítása:

    $ cd azuread
    $ node server.js

Egy új terminálablakban futtassa a `curl` parancsot

Próbálkozzon meg egy egyszerű POST művelettel:

`$ curl -isS -X POST http://127.0.0.1:3000/api/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

A 401-es hiba: hello jelenjen meg. Azt jelzi, hogy hello Passport réteg próbál tooredirect toohello végpont hitelesítéséhez.

## <a name="you-now-have-a-rest-api-service-that-uses-oauth2"></a>Az OAuth2-t használó REST API-szolgáltatás ezzel elkészült
A Restify és az OAuth használatával elkészítette a REST API-t! Már megfelelő kódrészletek, hogy toodevelop a szolgáltatás folytatáshoz és létrehozása az ebben a példában-kiszolgálón. Megtettünk mindent, ami a kiszolgálón OAuth2-kompatibilis ügyfél nélkül lehetséges volt. A következő lépéshez használja a továbbiakhoz újabb oktatóanyagokat, például a [iOS B2C használatával tooa webes API kapcsolódnak](active-directory-b2c-devquickstarts-ios.md) forgatókönyv.

## <a name="next-steps"></a>Következő lépések
Speciális toomore témakörök, most már továbbléphet, mint:

[Csatlakozás tooa webes API iOS B2C használatával](active-directory-b2c-devquickstarts-ios.md)
