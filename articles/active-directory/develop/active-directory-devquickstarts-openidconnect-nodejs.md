---
title: "elindult az Azure AD-bejelentkezés és kijelentkezés Node.js segítségével aaaGetting |} Microsoft Docs"
description: "Ismerje meg, hogyan toobuild egy Node.js Express MVC webalkalmazás, amely az Azure AD-bejelentkezés."
services: active-directory
documentationcenter: nodejs
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 81deecec-dbe2-4e75-8bc0-cf3788645f99
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 26481899c74741743b947bd891b65ff24ffc43c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="nodejs-web-app-sign-in-and-sign-out-with-azure-ad"></a>NODE.js-webalkalmazás be- és kijelentkezési az Azure ad szolgáltatással
Jelen példában használjuk a Passport:

* Bejelentkezési hello toohello alkalmazás Azure Active Directory (Azure AD) felhasználó.
* Hello felhasználói információkat jelenítenek meg.
* Bejelentkezési hello felhasználói hello alkalmazásból.

A Passport hitelesítési köztes a Node.js is. Rugalmas és, moduláris Passport diszkréten eldobása tooany az Express-alapú vagy restify webalkalmazás. Stratégiák széles választékát támogatja a felhasználónév és jelszó, Facebook, Twitter, és több használó hitelesítés. Kidolgoztunk is stratégiát a Microsoft Azure Active Directoryhoz. Azt telepítenie kell a modult, és vegye fel a Microsoft Azure Active Directory hello `passport-azure-ad` beépülő modult.

toodo, a következő lépéseket hajtsa végre a megfelelő hello:

1. Az alkalmazás regisztrálásához.
2. Állítsa be az alkalmazás toouse hello `passport-azure-ad` stratégia.
3. A Passport tooissue bejelentkezési és kijelentkezési kérések tooAzure AD használatára.
4. Nyomtatás hello felhasználó adatait.

az oktatóanyag kódjának hello kezelt [a Githubon](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS).  toofollow mellett, akkor [hello alkalmazás vázát .zip fájl letöltése](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip) vagy a Klónozás hello vázat:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

befejeződött hello alkalmazást is megtalálja, valamint az oktatóanyag hello végén.

## <a name="step-1-register-an-app"></a>1. lépés: Az alkalmazás regisztrálása
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).

2. Hello oldal hello tetején hello menüben válassza ki a fiókját. A hello **Directory** menüben válassza ki a kívánt tooregister hello Active Directory-bérlőt az alkalmazást.

3. Válassza ki **több szolgáltatások** a hello hello menüjének bal oldal az üdvözlő képernyőt, és jelölje ki **Azure Active Directory**.

4. Válassza ki **App regisztrációk**, majd válassza ki **Hozzáadás**.

5. Kövesse az utasításokat toocreate hello egy **webalkalmazás** és/vagy **WebAPI**.
  * Hello **neve** hello az alkalmazás az alkalmazás toousers ismerteti.

  * Hello **bejelentkezési URL-cím** hello alap URL-cím az alkalmazás.  hello vázat tartozó alapértelmezett érték a "return http://localhost: 3000/auth/openid nyelvhez.

6. Miután regisztrálta, az Azure AD rendeli hozzá az alkalmazás egy egyedi azonosítót. Ez az érték a következő hello kell szakaszok, ezért másolja hello alkalmazás oldalról.
7. A hello **beállítások** -> **tulajdonságok** az alkalmazás lapján hello App ID URI frissítése. Hello **App ID URI** az alkalmazás egyedi azonosítója. hello egyezmény toouse hello formátuma `https://<tenant-domain>/<app-name>`, például: `https://contoso.onmicrosoft.com/my-first-aad-app`.

## <a name="step-2-add-prerequisites-tooyour-directory"></a>2. lépés: Előfeltételek tooyour könyvtár hozzáadása
1. Könyvtárak tooyour gyökérmappa hello parancssorból módosítsa, ha már nem létezik, és majd a futtatási hello a következő parancsokat:

    * `npm install express`
    * `npm install ejs`
    * `npm install ejs-locals`
    * `npm install restify`
    * `npm install mongoose`
    * `npm install bunyan`
    * `npm install assert-plus`
    * `npm install passport`

2. Továbbá szüksége `passport-azure-ad`:
    * `npm install passport-azure-ad`

Ezzel telepít hello tárak, amelyek `passport-azure-ad` függ.

## <a name="step-3-set-up-your-app-toouse-hello-passport-node-js-strategy"></a>3. lépés: Az alkalmazás toouse hello passport-csomópont-js stratégia beállítása
Itt konfiguráljuk Express toouse hello OpenID Connect hitelesítési protokoll.  A Passport is használt toodo probléma be- és kijelentkezési kérések, például különböző dolgokat hello felhasználói munkamenet kezelésére, és hello felhasználó adatainak beolvasása.

1. toobegin, nyissa meg hello `config.js` hello gyökerében hello projekt fájlt, és adja meg az alkalmazás konfigurációs értékeit hello `exports.creds` szakasz.

  * Hello `clientID` hello van **alkalmazásazonosító** hello regisztrációs portálon rendelt tooyour alkalmazás esetén.

  * Hello `returnURL` hello van **átirányítási Uri-** hello portálon megadott.

  * Hello `clientSecret` hello titkos hello portálon létrehozott van.

2. Ezután nyissa meg a hello `app.js` hello hello projekt gyökerében található fájl. Majd adja hozzá a következő hívást tooinvoke hello hello `OIDCStrategy` stratégia `passport-azure-ad`.

    ```JavaScript
    var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

    // add a logger

    var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
    });
    ```

3. Ezt követően használja azt említett toohandle a bejelentkezési kérelmek hello stratégia.

    ```JavaScript
    // Use hello OIDCStrategy within Passport. (Section 2)
    //
    //   Strategies in passport require a `validate` function that accepts
    //   credentials (in this case, an OpenID identifier), and invokes a callback
    //   with a user object.
    passport.use(new OIDCStrategy({
        callbackURL: config.creds.returnURL,
        realm: config.creds.realm,
        clientID: config.creds.clientID,
        clientSecret: config.creds.clientSecret,
        oidcIssuer: config.creds.issuer,
        identityMetadata: config.creds.identityMetadata,
        skipUserProfile: config.creds.skipUserProfile,
        responseType: config.creds.responseType,
        responseMode: config.creds.responseMode
    },
    function(iss, sub, profile, accessToken, refreshToken, done) {
        if (!profile.email) {
        return done(new Error("No email found"), null);
        }
        // asynchronous verification, for effect...
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
A Passport az összes stratégia (Twitter-, Facebook-on, és így tovább), amely stratégiafejlesztő a hasonló mintát alkalmaz. Hello stratégia megnézi, láthatja, hogy azt adja át azt egy függvényt, amely rendelkezik egy jogkivonat és egy kész hello paraméterekként. hello stratégia előre vissza toous, miután a tevékenységeket végez el. Majd szeretnénk toostore hello felhasználói és stash hello jogkivonatot, nem szükséges tooask azt újra.

> [!IMPORTANT]
hello előző kód minden olyan felhasználó, akkor fordul elő, tooauthenticate tooour kiszolgáló vesz igénybe. Ez az úgynevezett automatikus regisztráció. Azt javasoljuk, ne engedélyezze, hogy bárki nélkül azokat, amelyeket a folyamat keresztül regisztrálniuk tooa az üzemi kiszolgáló hitelesítéséhez. Ez általában akkor hello megoldást látjuk a fogyasztói alkalmazásoknál, amelyek lehetővé teszik a Facebook tooregister, de majd megkérdezi, hogy tooprovide további információt. Ha ez nem egy mintaalkalmazást, kinyerhettük volna hello a felhasználó e-mail címét a hello jogkivonat-objektumból, amely adott vissza, és felkérhettük volna hello felhasználói toofill meg további információkat. Mivel ez egy tesztkiszolgálón, hogy adja hozzá toohello memóriában lévő adatbázishoz.


4. A következő hello módszereket, amelyek lehetővé teszik számunkra tootrack hello bejelentkezett felhasználók a Passport által előírt adjuk hozzá. Ezek a metódusok szerializálása és deszerializálása hello felhasználói adatok lehetnek.

    ```JavaScript

            // Passport session setup. (Section 2)

            //   toosupport persistent sign-in sessions, Passport needs toobe able to
            //   serialize users into hello session and deserialize them out of hello session. Typically,
            //   this is done simply by storing hello user ID when serializing and finding
            //   hello user by ID when deserializing.
            passport.serializeUser(function(user, done) {
            done(null, user.email);
            });

            passport.deserializeUser(function(id, done) {
            findByEmail(id, function (err, user) {
                done(err, user);
            });
            });

            // array toohold signed-in users
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

5.  A következő hello kód tooload hello Express motor adjuk hozzá. Jelen példában használjuk hello alapértelmezett /views és Express /routes minta nyújt.

    ```JavaScript

        // configure Express (section 2)

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

6. Végezetül adjuk hozzá hello irányítja a tényleges bejelentkezési kérelmek toohello hello átadása `passport-azure-ad` motor:


       ```JavaScript

        // Our Auth routes (section 3)

        // GET /auth/openid
        //   Use passport.authenticate() as route middleware tooauthenticate the
        //   request. hello first step in OpenID authentication involves redirecting
        //   hello user tootheir OpenID provider. After authenticating, hello OpenID
        //   provider redirects hello user back toothis application at
        //   /auth/openid/return.
        app.get('/auth/openid',
        passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
        function(req, res) {
            log.info('Authentication was called in hello Sample');
            res.redirect('/');
        });

            // GET /auth/openid/return
            //   Use passport.authenticate() as route middleware tooauthenticate the
            //   request. If authentication fails, hello user is redirected back toothe
            //   sign-in page. Otherwise, hello primary route function is called,
            //   which, in this example, redirects hello user toohello home page.
            app.get('/auth/openid/return',
              passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
              function(req, res) {
                log.info('We received a return from AzureAD.');
                res.redirect('/');
              });

            // POST /auth/openid/return
            //   Use passport.authenticate() as route middleware tooauthenticate the
            //   request. If authentication fails, hello user is redirected back toothe
            //   sign-in page. Otherwise, hello primary route function is called,
            //   which, in this example, redirects hello user toohello home page.
            app.post('/auth/openid/return',
              passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
              function(req, res) {
                log.info('We received a return from AzureAD.');
                res.redirect('/');
              });
       ```


## <a name="step-4-use-passport-tooissue-sign-in-and-sign-out-requests-tooazure-ad"></a>4. lépés: Használatát a Passport tooissue bejelentkezési és kijelentkezési kérések tooAzure AD
Az alkalmazás már megfelelően konfigurált toocommunicate hello végponttal hello OpenID Connect hitelesítési protokoll használatával.  `passport-azure-ad`rendelkezik az összes hello műveletek hitelesítési üzenetek létrehozásával, ellenőrzése az Azure ad-jogkivonatok és karbantartása a felhasználói munkamenetek végrehajtott végrehajtására. Marad, hogy a felhasználók megkapják, egy a módon toosign és kijelentkezési, és hello bejelentkezett felhasználók további adatokat gyűjt.

1. Első lépésként adjuk hozzá hello alapértelmezett, bejelentkezési, fiókkal, és kijelentkezési metódusokat tooour `app.js` fájlt:

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
            log.info('Login was called in hello Sample');
            res.redirect('/');
        });

        app.get('/logout', function(req, res){
          req.logout();
          res.redirect('/');
        });

    ```

2.  Tekintsük át részletesen ezek:

  * Hello `/`útvonal átirányítja a felhasználókat toohello index.ejs nézet benyújtása hello felhasználói hello kérelem (ha van ilyen).
  * Hello `/account` útvonal először *biztosítja, hogy hitelesítése* (megvalósítása, amely az alábbi példa hello), és ezután fázisok hello-e felhasználói hello kérelem, hogy megkaphassa hello felhasználó további információt.
  * Hello `/login` útvonal hívja fel az azuread-openidconnect hitelesítő `passport-azuread`. Ha nem jár sikerrel, amely, hello hátsó túl/felhasználónevet irányítja át.
  * Hello `/logout` útvonal egyszerűen meghívja a hello logout.ejs (és útvonal), mely törlése cookie-kat, és a beolvasása hello felhasználói hátsó tooindex.ejs.

3. Hello utolsó részének `app.js`, adjuk hozzá hello **EnsureAuthenticated** a használt módszer `/account`, mert a korábban bemutatott.

    ```JavaScript

        // Simple route middleware tooensure user is authenticated. (section 4)

        //   Use this route middleware on any resource that needs toobe protected. If
        //   hello request is authenticated (typically via a persistent sign-in session),
        //   hello request proceeds. Otherwise, hello user is redirected toothe
        //   sign-in page.
        function ensureAuthenticated(req, res, next) {
          if (req.isAuthenticated()) { return next(); }
          res.redirect('/login')
        }
    ```

4. Végül magát hello kiszolgálót hozzuk létre az `app.js`:

```JavaScript

        app.listen(3000);

```


## <a name="step-5-toodisplay-our-user-in-hello-website-create-hello-views-and-routes-in-express"></a>5. lépés: toodisplay hello webhelyen, a felhasználó létrehozása hello nézetek és útvonalak az Express
Most `app.js` befejeződött. A Microsoft egyszerűen tooadd hello útvonalak kell, és megtekinti azt lekérése toohello felhasználó, valamint hello kezelni megjelenítése hello adatokat `/logout` és `/login` létrehozott útvonalakat.

1. Hozzon létre hello `/routes/index.js` útvonal hello gyökérkönyvtárban.

    ```JavaScript
                /*
                 * GET home page.
                 */

                exports.index = function(req, res){
                  res.render('index', { title: 'Express' });
                };
    ```

2. Hozzon létre hello `/routes/user.js` útvonal hello gyökérkönyvtárban.

                ```JavaScript
                /*
                 * GET users listing.
                 */

                exports.list = function(req, res){
                  res.send("respond with a resource");
                };
                ```

 Ezek adják át a hello kérelem tooour nézetek, valamint a hello felhasználói, ha van ilyen.

3. Hozzon létre hello `/views/index.ejs` nézet hello gyökérkönyvtárban. Ez egy egyszerű lap, amely behívja a bejelentkezési és kijelentkezési metódusokat az és lehetővé teszi a us toograb fiók adatait. Figyelje meg, hogy használhatunk hello feltételes `if (!user)` , a beadott keresztül hello kérelem hello felhasználói bizonyító adatok van bejelentkezett felhasználó.

    ```JavaScript
    <% if (!user) { %>
        <h2>Welcome! Please log in.</h2>
        <a href="/login">Log In</a>
    <% } else { %>
        <h2>Hello, <%= user.displayName %>.</h2>
        <a href="/account">Account Info</a></br>
        <a href="/logout">Log Out</a>
    <% } %>
    ```

4. Hozzon létre hello `/views/account.ejs` megtekintése hello gyökérkönyvtárban, így azt további információk is megtekinthetők, amely `passport-azuread` hello felhasználói kérelem állította.

    ```Javascript
    <% if (!user) { %>
        <h2>Welcome! Please log in.</h2>
        <a href="/login">Log In</a>
    <% } else { %>
    <p>displayName: <%= user.displayName %></p>
    <p>givenName: <%= user.name.givenName %></p>
    <p>familyName: <%= user.name.familyName %></p>
    <p>UPN: <%= user._json.upn %></p>
    <p>Profile ID: <%= user.id %></p>
  ##Next steps  <p>Full Claimes</p>
    <%- JSON.stringify(user) %>
    <p></p>
    <a href="/logout">Log Out</a>
    <% } %>
    ```

5. Ellenőrizze a hely jó elrendezés hozzáadásával. Hozzon létre hello ' / views/layout.ejs' view hello a gyökérkönyvtár.

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
                <a href="/login">Log In</a>
                </p>
            <% } else { %>
                <p>
                <a href="/">Home</a> |
                <a href="/account">Account</a> |
                <a href="/logout">Log Out</a>
                </p>
            <% } %>
            <%- body %>
        </body>
    </html>
    ```

##<a name="next-steps"></a>Következő lépések
Végezetül hozza létre, és futtassa az alkalmazást. Futtatás `node app.js`, és folytassa a túl`http://localhost:3000`.

Jelentkezzen be személyes Microsoft-fiókkal vagy egy munkahelyi vagy iskolai fiókkal, és figyelje meg, hogyan hello felhasználói identitás hello /account lista megjelenik. Most már rendelkezik egy webalkalmazást az iparági szabványos protokollok, amely képes hitelesíteni a személyes és munkahelyi vagy iskolai fiókkal rendelkező felhasználók védett.

Referenciaként hello befejeződött (a konfigurációs értékek nélkül) minta [egy .zip-fájlban is](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip). Azt is megteheti hogy is klónozza a Githubról:

```git clone --branch complete https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

Ön most már továbbléphet az összetettebb témákra. Érdemes lehet tootry:

[Védelem biztosítása webes API-t az Azure ad szolgáltatással](active-directory-devquickstarts-webapi-nodejs.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
