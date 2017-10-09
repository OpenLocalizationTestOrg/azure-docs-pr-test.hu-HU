---
title: "az Active Directory v2.0 aaaAzure a Node.js web app bejelentkezés |} Microsoft Docs"
description: "Ismerje meg, hogyan toobuild egy Node.js webes alkalmazást, amely a felhasználó bejelentkezik egy személyes Microsoft-fiók és a munkahelyi vagy iskolai fiók használatával."
services: active-directory
documentationcenter: nodejs
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 1b889e72-f5c3-464a-af57-79abf5e2e147
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 05/13/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: f8ce6e2b841c215cb14e82bcf444fe849634cc88
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-tooa-nodejs-web-app"></a>Bejelentkezési tooa Node.js webalkalmazás hozzáadása

> [!NOTE]
> Nem minden Azure Active Directory forgatókönyvek és funkciók együttműködve hello v2.0-végponttól. toodetermine kell használnia a v2.0-végponttól hello vagy hello 1.0-s verziójú végpontot, hogy olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).
> 

Ebben az oktatóanyagban használjuk Passport toodo hello a következő feladatokat:

* A web app alkalmazásban hello felhasználói bejelentkezés Azure Active Directory (Azure AD) segítségével, és hello v2.0-végponttól.
* Hello felhasználói információkat jelenítenek meg.
* Bejelentkezési hello felhasználói hello alkalmazásból.

A **Passport** a Node.js-hez készült közbenső hitelesítési szoftver. Rugalmas és, moduláris Passport diszkréten eldobása minden Express-alapú vagy restify webalkalmazás. Passport stratégiák széles választékát támogatja a hitelesítést egy felhasználónév és jelszó, Facebook-on, Twitter vagy egyéb beállítások használatával. Kidolgoztunk egy stratégiát, amellyel a szoftver az Azure AD esetében is felhasználható. Ebben a cikkben megmutatjuk, hogyan tooinstall hello modul, és adja hozzá az Azure AD hello `passport-azure-ad` beépülő modult.

## <a name="download"></a>Letöltés
az oktatóanyag kódjának hello kezelt [a Githubon](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs). toofollow hello oktatóanyagban is [hello alkalmazás vázát .zip fájl letöltése](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip) vagy a Klónozás hello vázat:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

Ez az oktatóanyag végén hello befejeződött hello alkalmazás is beszerezheti.

## <a name="1-register-an-app"></a>1: az alkalmazás regisztrálása
Hozzon létre egy új alkalmazást [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy hajtsa végre a [a részletes lépéseket](active-directory-v2-app-registration.md) tooregister egy alkalmazást. Ellenőrizze, hogy:

* Másolás hello **alkalmazásazonosító** tooyour app rendelve. Ebben az oktatóanyagban van szükség.
* Adja hozzá a hello **webes** platform az alkalmazásra vonatkozóan.
* Másolás hello **átirányítási URI-** hello portálról. Hello alapértelmezett URI azonosító értékét kell használnia `urn:ietf:wg:oauth:2.0:oob`.

## <a name="2-add-prerequisities-tooyour-directory"></a>2: prerequisities tooyour könyvtár hozzáadása
Egy parancssorba módosítása könyvtárak toogo tooyour gyökérmappára, ha nem már létezik. Futtassa a következő parancsok hello:

* `npm install express`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install restify`
* `npm install mongoose`
* `npm install bunyan`
* `npm install assert-plus`
* `npm install passport`
* `npm install webfinger`
* `npm install body-parser`
* `npm install express-session`
* `npm install cookie-parser`

Ezenkívül használjuk `passport-azure-ad` a hello vázat a hello gyors üzembe helyezés:

* `npm install passport-azure-ad`

Ezzel telepít hello tárak, amelyek `passport-azure-ad` használja.

## <a name="3-set-up-your-app-toouse-hello-passport-node-js-strategy"></a>3: állítsa be az alkalmazás toouse hello passport-csomópont-js stratégia
Hello Express köztes toouse hello OpenID Connect hitelesítési protokoll beállítása. Használjon Passport tooissue be- és kijelentkezési kérések, hello felhasználói munkamenet kezelésére, és többek között a hello felhasználó adatainak beolvasása.

1.  Hello projekt gyökerében hello nyissa meg hello Config.js fájlt. A hello `exports.creds` területen adja meg az alkalmazás konfigurációs értékeit.
  
  * `clientID`: hello **alkalmazásazonosító** Ez az alkalmazás hozzárendelt tooyour hello Azure-portálon.
  * `returnURL`: hello **átirányítási URI-** hello portálon megadott.
  * `clientSecret`: hello portálon létrehozott hello titkos kulcsot.

2.  Hello projekt gyökerében hello nyissa meg hello App.js fájlban. a tooinvoke hello OIDCStrategy stratey `passport-azure-ad`, adja hozzá a következő hívást hello:

  ```JavaScript
  var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

  // Add some logging
  var log = bunyan.createLogger({
      name: 'Microsoft OIDC Example Web Application'
  });
  ```

3.  toohandle a bejelentkezési kérelmek, használja a korábban említett stratégiát hello:

  ```JavaScript
  // Use hello OIDCStrategy within Passport (section 2)
  //
  //   Strategies in Passport require a `validate` function. hello function accepts
  //   credentials (in this case, an OpenID identifier), and invokes a callback
  //   with a user object.
  passport.use( new OIDCStrategy({
      callbackURL: config.creds.returnURL,
      realm: config.creds.realm,
      clientID: config.creds.clientID,
      clientSecret: config.creds.clientSecret,
      oidcIssuer: config.creds.issuer,
      identityMetadata: config.creds.identityMetadata,
      responseType: config.creds.responseType,
      responseMode: config.creds.responseMode,
      skipUserProfile: config.creds.skipUserProfile
      scope: config.creds.scope
    },
    function(iss, sub, profile, accessToken, refreshToken, done) {
      log.info('Example: Email address we received was: ', profile.email);
      // Asynchronous verification, for effect...
      process.nextTick(function () {
        findByEmail(profile.email, function(err, user) {
          if (err) {
            return done(err);
          }
          if (!user) {
            // "Auto-registration"
            users.push(profile);
            return done(null, profile);
          }
          return done(null, user);
        });
      });
    }
  ));
  ```

A Passport hasonló mintát alkalmaz az összes stratégia (Twitter-, Facebook-on, és így tovább). Stratégiafejlesztő toohello mintát. Hello stratégia átadni egy `function()` tokent használ, és `done` paraméterekként. hello stratégia ad vissza, miután a tevékenységeket végez el. Hello felhasználói és stash hello token tárolására, így nem kell tooask azt újra.

  > [!IMPORTANT]
  > hello előző kód készít minden olyan felhasználó, amely képes hitelesíteni tooyour kiszolgáló. Ez az úgynevezett automatikus regisztráció. Az üzemi kiszolgálón általában csak akkor érdemes toolet bárki, aki nélkül őket nyissa meg az Ön által regisztrációs folyamatot. Ez általában az, hogy a fogyasztói alkalmazásoknál hello mintát. hello alkalmazást is lehetővé teheti a Facebook tooregister, de majd kéri tooenter további információt. Ha ez az oktatóanyag nem parancssori program alkalmaz, objektumból hello token visszaadott lehetett kibontani a hello e-mail. Ezt követően előfordulhat, hogy kérje meg hello felhasználói tooenter további információt. Mivel ez egy tesztkiszolgálón, hozzáadhat hello felhasználó közvetlenül a toohello memóriában lévő adatbázishoz.
  > 
  > 

4.  Adja hozzá, hogy használja-e a felhasználók, akik van bejelentkezve, tookeep követése hello metódusokat Passport által előírt. Ez magában foglalja a szerializálása és deszerializálása hello felhasználói adatokat:

  ```JavaScript

  // Passport session setup (section 2)

  //   toosupport persistent login sessions, Passport needs toobe able to
  //   serialize users into, and deserialize users out of, hello session. Typically,
  //   this is as simple as storing hello user ID when serializing, and finding
  //   hello user by ID when deserializing.
  passport.serializeUser(function(user, done) {
    done(null, user.email);
  });

  passport.deserializeUser(function(id, done) {
    findByEmail(id, function (err, user) {
      done(err, user);
    });
  });

  // Array toohold signed-in users
  var users = [];

  var findByEmail = function(email, fn) {
    for (var i = 0, len = users.length; i < len; i++) {
      var user = users[i];
      log.info('we are using user: ', user);
      if (user.email === email) {
        return fn(null, user);
      }
    }
    return fn(null, null);
  };
  ```

5.  Adja hozzá, amelyek hello Express motor betölti hello kódot. Hello alapértelmezett /views használ, és Express /routes minta biztosítja:

  ```JavaScript

  // Set up Express (section 2)

  var app = express();

  app.configure(function() {
    app.set('views', __dirname + '/views');
    app.set('view engine', 'ejs');
    app.use(express.logger());
    app.use(express.methodOverride());
    app.use(cookieParser());
    app.use(expressSession({ secret: 'keyboard cat', resave: true, saveUninitialized: false }));
    app.use(bodyParser.urlencoded({ extended : true }));
    // Initialize Passport!  Also use passport.session() middleware, toosupport
    // persistent login sessions (recommended).
    app.use(passport.initialize());
    app.use(passport.session());
    app.use(app.router);
    app.use(express.static(__dirname + '/../../public'));
  });

  ```

6.  Adja hozzá a hello POST irányítja a hello tényleges bejelentkezési kérelmek toohello ki, hogy az aktuális `passport-azure-ad` motor:

  ```JavaScript

  // Auth routes (section 3)

  // GET /auth/openid
  //   Use passport.authenticate() as route middleware tooauthenticate the
  //   request. hello first step in OpenID authentication involves redirecting
  //   hello user toohello user's OpenID provider. After authenticating, hello OpenID
  //   provider redirects hello user back toothis application at
  //   /auth/openid/return.

  app.get('/auth/openid',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {
      log.info('Authentication was called in hello sample');
      res.redirect('/');
    });

  // GET /auth/openid/return
  //   Use passport.authenticate() as route middleware tooauthenticate the
  //   request. If authentication fails, hello user is redirected back toothe
  //   sign-in page. Otherwise, hello primary route function is called.
  //   In this example, it redirects hello user toohello home page.
  app.get('/auth/openid/return',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {

      res.redirect('/');
    });

  // POST /auth/openid/return
  //   Use passport.authenticate() as route middleware tooauthenticate the
  //   request. If authentication fails, hello user is redirected back toothe
  //   sign-in page. Otherwise, hello primary route function is called. 
  //   In this example, it redirects hello user toohello home page.

  app.post('/auth/openid/return',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {

      res.redirect('/');
    });
  ```

## <a name="4-use-passport-tooissue-sign-in-and-sign-out-requests-tooazure-ad"></a>4: használatát a Passport tooissue be- és kijelentkezési kérések tooAzure AD
Az alkalmazás most már be van állítva hello v2.0-végponttal toocommunicate hello OpenID Connect hitelesítési protokoll használatával. Hello `passport-azure-ad` stratégia gondoskodik hitelesítési üzenetek létrehozásával, ellenőrzése az Azure ad-jogkivonatok és karbantartását hello felhasználói munkamenet összes hello részleteit. Minden toodo hagyott toogive van a felhasználók a módon toosign, és jelentkezzen ki, és toogather hello van bejelentkezett felhasználó további információt.

1.  Adja hozzá a hello **alapértelmezett**, **bejelentkezési**, **fiók**, és **kijelentkezési** módszerek tooyour App.js fájlban:

  ```JavaScript

  //Routes (section 4)

  app.get('/', function(req, res){
    res.render('index', { user: req.user });
  });

  app.get('/account', ensureAuthenticated, function(req, res){
    res.render('account', { user: req.user });
  });

  app.get('/login',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {
      log.info('Login was called in hello sample');
      res.redirect('/');
  });

  app.get('/logout', function(req, res){
    req.logout();
    res.redirect('/');
  });

  ```

  Az alábbiakban hello részletek:
    
    * Hello `/` útvonal átirányítja a felhasználókat toohello index.ejs nézet. Hello felhasználói haladnak hello kérelem (ha van ilyen).
    * Hello `/account` útvonal először *biztosítja, hogy hitelesítése* (megvalósítása, amely a következő kód hello). Ezt követően átadja hello felhasználói hello kérelemben. Ez a helyzet hello felhasználó további információt kaphat.
    * Hello `/login` útvonal hívja a `azuread-openidconnect` a hitelesítő `passport-azuread`. Ha, amely nem jár sikerrel, az átirányítja hello felhasználói vissza túl`/login`.
    * Hello `/logout` útvonal hívja hello logout.ejs megtekintése (és útvonal). Ez törli a cookie-kat, és majd vissza hello felhasználói hátsó tooindex.ejs.

2.  Adja hozzá a hello **EnsureAuthenticated** , amelyet korábban a használt módszer `/account`:

  ```JavaScript

  // Route middleware tooensure hello user is authenticated (section 4)

  //   Use this route middleware on any resource that needs toobe protected. If
  //   hello request is authenticated (typically via a persistent login session),
  //   hello request proceeds. Otherwise, hello user is redirected toothe
  //   sign-in page.
  function ensureAuthenticated(req, res, next) {
    if (req.isAuthenticated()) { return next(); }
    res.redirect('/login')
  }

  ```

3.  Az App.js hello kiszolgáló létrehozása:

  ```JavaScript

  app.listen(3000);

  ```


## <a name="5-create-hello-views-and-routes-in-express-that-you-show-your-user-on-hello-website"></a>5: hello nézetek és útvonalak létrehozása az Express a felhasználó hello webhely megjelenítése
Adja hozzá a hello útvonalakat és nézeteket, amelyek információt toohello felhasználói megjelenítése. hello útvonalakat és nézeteket is kezelni hello `/logout` és `/login` létrehozott útvonalak.

1. Hello gyökérkönyvtárában, hozzon létre hello `/routes/index.js` útvonal.

  ```JavaScript

  /*
  * GET home page.
  */

  exports.index = function(req, res){
    res.render('index', { title: 'Express' });
  };
  ```

2.  Hello gyökérkönyvtárában, hozzon létre hello `/routes/user.js` útvonal.

  ```JavaScript

  /*
  * GET users listing.
  */

  exports.list = function(req, res){
    res.send("respond with a resource");
  };
  ```

  `/routes/index.js`és `/routes/user.js` vannak egyszerű útvonalak adják át a hello kérelem tooyour nézetek, beleértve a hello felhasználói, ha van ilyen.

3.  Hello gyökérkönyvtárában, hozzon létre hello `/views/index.ejs` nézet. Ezen a lapon meghívja a **bejelentkezési** és **kijelentkezési** módszerek. Hello is a `/views/index.ejs` toocapture fiók adatainak megtekintése. Használhatja a feltételes hello `if (!user)` hello kérelem beadott keresztül hello felhasználóként. Fontos, hogy rendelkezik-e a bejelentkezett felhasználó bizonyító adatok.

  ```JavaScript
  <% if (!user) { %>
      <h2>Welcome! Please sign in.</h2>
      <a href="/login">Sign in</a>
  <% } else { %>
      <h2>Hello, <%= user.displayName %>.</h2>
      <a href="/account">Account info</a></br>
      <a href="/logout">Sign out</a>
  <% } %>
  ```

4.  Hello gyökérkönyvtárában, hozzon létre hello `/views/account.ejs` nézet. Hello `/views/account.ejs` nézet lehetővé teszi tooview további információt, amely `passport-azuread` hello felhasználói kérelem helyezi.

  ```Javascript
  <% if (!user) { %>
      <h2>Welcome! Please sign in.</h2>
      <a href="/login">Sign in</a>
  <% } else { %>
  <p>displayName: <%= user.displayName %></p>
  <p>givenName: <%= user.name.givenName %></p>
  <p>familyName: <%= user.name.familyName %></p>
  <p>UPN: <%= user._json.upn %></p>
  <p>Profile ID: <%= user.id %></p>
  <p>Full Claimes</p>
  <%- JSON.stringify(user) %>
  <p></p>
  <a href="/logout">Sign out</a>
  <% } %>
  ```

5.  Adjon hozzá egy elrendezés. Hello gyökérkönyvtárában, hozzon létre hello `/views/layout.ejs` nézet.

  ```HTML

  <!DOCTYPE html>
  <html>
      <head>
          <title>Passport-OpenID Example</title>
      </head>
      <body>
          <% if (!user) { %>
              <p>
              <a href="/">Home</a> |
              <a href="/login">Sign in</a>
              </p>
          <% } else { %>
              <p>
              <a href="/">Home</a> |
              <a href="/account">Account</a> |
              <a href="/logout">Sign out</a>
              </p>
          <% } %>
          <%- body %>
      </body>
  </html>
  ```

6.  toobuild futtatta az alkalmazását, és futtassa `node app.js`. Ezután térjen túl`http://localhost:3000`.

7.  Jelentkezzen be személyes Microsoft-fiókkal vagy egy munkahelyi vagy iskolai fiókkal. Vegye figyelembe, hogy hello felhasználói identitás tükröz hello /account listában. 

Most már rendelkezik egy webalkalmazást, amelyet az iparági szabványos protokollok használatával. Az alkalmazás a felhasználók a személyes és munkahelyi vagy iskolai fiókok használatával képes hitelesíteni.

## <a name="next-steps"></a>Következő lépések
Befejeződött hello mintát (a konfigurációs értékek nélkül) referenciaként biztosított [egy .zip fájl](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip). Akkor is klónozhatja azt a Githubról:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

Továbbléphet a következő témakörök speciális toomore. Érdemes lehet tootry:

[Node.js webes API biztonságossá hello v2.0-végponttól használatával](active-directory-v2-devquickstarts-node-api.md)

Az alábbiakban néhány további források:

* [Az Azure AD v2.0 fejlesztői útmutató](active-directory-appmodel-v2-overview.md)
* [A verem túlcsordulás "azure-active-directory" címke](http://stackoverflow.com/questions/tagged/azure-active-directory)

### <a name="get-security-updates-for-our-products"></a>Biztonsági frissítések termékeinkhez
Javasoljuk toosign biztonsági incidensek értesítendő toobe fel. A hello [Microsoft műszaki biztonsági értesítéseket](https://technet.microsoft.com/security/dd252948) előfizetés tooSecurity tanácsadók riasztások lapján.

