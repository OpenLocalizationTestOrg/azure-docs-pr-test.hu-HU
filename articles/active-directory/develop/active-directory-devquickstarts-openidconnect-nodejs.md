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
# <a name="nodejs-web-app-sign-in-and-sign-out-with-azure-ad"></a><span data-ttu-id="17b31-103">NODE.js-webalkalmazás be- és kijelentkezési az Azure ad szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="17b31-103">Node.js web app sign-in and sign-out with Azure AD</span></span>
<span data-ttu-id="17b31-104">Jelen példában használjuk a Passport:</span><span class="sxs-lookup"><span data-stu-id="17b31-104">Here we use Passport to:</span></span>

* <span data-ttu-id="17b31-105">Bejelentkezési hello toohello alkalmazás Azure Active Directory (Azure AD) felhasználó.</span><span class="sxs-lookup"><span data-stu-id="17b31-105">Sign hello user in toohello app with Azure Active Directory (Azure AD).</span></span>
* <span data-ttu-id="17b31-106">Hello felhasználói információkat jelenítenek meg.</span><span class="sxs-lookup"><span data-stu-id="17b31-106">Display information about hello user.</span></span>
* <span data-ttu-id="17b31-107">Bejelentkezési hello felhasználói hello alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="17b31-107">Sign hello user out of hello app.</span></span>

<span data-ttu-id="17b31-108">A Passport hitelesítési köztes a Node.js is.</span><span class="sxs-lookup"><span data-stu-id="17b31-108">Passport is authentication middleware for Node.js.</span></span> <span data-ttu-id="17b31-109">Rugalmas és, moduláris Passport diszkréten eldobása tooany az Express-alapú vagy restify webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="17b31-109">Flexible and modular, Passport can be unobtrusively dropped in tooany Express-based or restify web application.</span></span> <span data-ttu-id="17b31-110">Stratégiák széles választékát támogatja a felhasználónév és jelszó, Facebook, Twitter, és több használó hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="17b31-110">A comprehensive set of strategies support authentication that uses a username and password, Facebook, Twitter, and more.</span></span> <span data-ttu-id="17b31-111">Kidolgoztunk is stratégiát a Microsoft Azure Active Directoryhoz.</span><span class="sxs-lookup"><span data-stu-id="17b31-111">We have developed a strategy for Microsoft Azure Active Directory.</span></span> <span data-ttu-id="17b31-112">Azt telepítenie kell a modult, és vegye fel a Microsoft Azure Active Directory hello `passport-azure-ad` beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="17b31-112">We install this module and then add hello Microsoft Azure Active Directory `passport-azure-ad` plug-in.</span></span>

<span data-ttu-id="17b31-113">toodo, a következő lépéseket hajtsa végre a megfelelő hello:</span><span class="sxs-lookup"><span data-stu-id="17b31-113">toodo this, take hello following steps:</span></span>

1. <span data-ttu-id="17b31-114">Az alkalmazás regisztrálásához.</span><span class="sxs-lookup"><span data-stu-id="17b31-114">Register an app.</span></span>
2. <span data-ttu-id="17b31-115">Állítsa be az alkalmazás toouse hello `passport-azure-ad` stratégia.</span><span class="sxs-lookup"><span data-stu-id="17b31-115">Set up your app toouse hello `passport-azure-ad` strategy.</span></span>
3. <span data-ttu-id="17b31-116">A Passport tooissue bejelentkezési és kijelentkezési kérések tooAzure AD használatára.</span><span class="sxs-lookup"><span data-stu-id="17b31-116">Use Passport tooissue sign-in and sign-out requests tooAzure AD.</span></span>
4. <span data-ttu-id="17b31-117">Nyomtatás hello felhasználó adatait.</span><span class="sxs-lookup"><span data-stu-id="17b31-117">Print data about hello user.</span></span>

<span data-ttu-id="17b31-118">az oktatóanyag kódjának hello kezelt [a Githubon](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS).</span><span class="sxs-lookup"><span data-stu-id="17b31-118">hello code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS).</span></span>  <span data-ttu-id="17b31-119">toofollow mellett, akkor [hello alkalmazás vázát .zip fájl letöltése](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip) vagy a Klónozás hello vázat:</span><span class="sxs-lookup"><span data-stu-id="17b31-119">toofollow along, you can [download hello app's skeleton as a .zip file](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip) or clone hello skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

<span data-ttu-id="17b31-120">befejeződött hello alkalmazást is megtalálja, valamint az oktatóanyag hello végén.</span><span class="sxs-lookup"><span data-stu-id="17b31-120">hello completed application is provided at hello end of this tutorial as well.</span></span>

## <a name="step-1-register-an-app"></a><span data-ttu-id="17b31-121">1. lépés: Az alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="17b31-121">Step 1: Register an app</span></span>
1. <span data-ttu-id="17b31-122">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="17b31-122">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="17b31-123">Hello oldal hello tetején hello menüben válassza ki a fiókját.</span><span class="sxs-lookup"><span data-stu-id="17b31-123">In hello menu at hello top of hello page, select your account.</span></span> <span data-ttu-id="17b31-124">A hello **Directory** menüben válassza ki a kívánt tooregister hello Active Directory-bérlőt az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="17b31-124">Under hello **Directory** list, choose hello Active Directory tenant where you want tooregister your application.</span></span>

3. <span data-ttu-id="17b31-125">Válassza ki **több szolgáltatások** a hello hello menüjének bal oldal az üdvözlő képernyőt, és jelölje ki **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="17b31-125">Select **More Services** in hello menu on hello left side of hello screen, and then select **Azure Active Directory**.</span></span>

4. <span data-ttu-id="17b31-126">Válassza ki **App regisztrációk**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="17b31-126">Select **App registrations**, and then select **Add**.</span></span>

5. <span data-ttu-id="17b31-127">Kövesse az utasításokat toocreate hello egy **webalkalmazás** és/vagy **WebAPI**.</span><span class="sxs-lookup"><span data-stu-id="17b31-127">Follow hello prompts toocreate a **Web Application** and/or **WebAPI**.</span></span>
  * <span data-ttu-id="17b31-128">Hello **neve** hello az alkalmazás az alkalmazás toousers ismerteti.</span><span class="sxs-lookup"><span data-stu-id="17b31-128">hello **name** of hello application describes your application toousers.</span></span>

  * <span data-ttu-id="17b31-129">Hello **bejelentkezési URL-cím** hello alap URL-cím az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="17b31-129">hello **Sign-On URL** is hello base URL of your app.</span></span>  <span data-ttu-id="17b31-130">hello vázat tartozó alapértelmezett érték a "return http://localhost: 3000/auth/openid nyelvhez.</span><span class="sxs-lookup"><span data-stu-id="17b31-130">hello skeleton's default is `http://localhost:3000/auth/openid/return`\`.</span></span>

6. <span data-ttu-id="17b31-131">Miután regisztrálta, az Azure AD rendeli hozzá az alkalmazás egy egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="17b31-131">After you register, Azure AD assigns your app a unique application ID.</span></span> <span data-ttu-id="17b31-132">Ez az érték a következő hello kell szakaszok, ezért másolja hello alkalmazás oldalról.</span><span class="sxs-lookup"><span data-stu-id="17b31-132">You need this value in hello following sections, so copy it from hello application page.</span></span>
7. <span data-ttu-id="17b31-133">A hello **beállítások** -> **tulajdonságok** az alkalmazás lapján hello App ID URI frissítése.</span><span class="sxs-lookup"><span data-stu-id="17b31-133">From hello **Settings** -> **Properties** page for your application, update hello App ID URI.</span></span> <span data-ttu-id="17b31-134">Hello **App ID URI** az alkalmazás egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="17b31-134">hello **App ID URI** is a unique identifier for your application.</span></span> <span data-ttu-id="17b31-135">hello egyezmény toouse hello formátuma `https://<tenant-domain>/<app-name>`, például: `https://contoso.onmicrosoft.com/my-first-aad-app`.</span><span class="sxs-lookup"><span data-stu-id="17b31-135">hello convention is toouse hello format `https://<tenant-domain>/<app-name>`, for example: `https://contoso.onmicrosoft.com/my-first-aad-app`.</span></span>

## <a name="step-2-add-prerequisites-tooyour-directory"></a><span data-ttu-id="17b31-136">2. lépés: Előfeltételek tooyour könyvtár hozzáadása</span><span class="sxs-lookup"><span data-stu-id="17b31-136">Step 2: Add prerequisites tooyour directory</span></span>
1. <span data-ttu-id="17b31-137">Könyvtárak tooyour gyökérmappa hello parancssorból módosítsa, ha már nem létezik, és majd a futtatási hello a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="17b31-137">From hello command line, change directories tooyour root folder if you're not already there, and then run hello following commands:</span></span>

    * `npm install express`
    * `npm install ejs`
    * `npm install ejs-locals`
    * `npm install restify`
    * `npm install mongoose`
    * `npm install bunyan`
    * `npm install assert-plus`
    * `npm install passport`

2. <span data-ttu-id="17b31-138">Továbbá szüksége `passport-azure-ad`:</span><span class="sxs-lookup"><span data-stu-id="17b31-138">In addition, you need `passport-azure-ad`:</span></span>
    * `npm install passport-azure-ad`

<span data-ttu-id="17b31-139">Ezzel telepít hello tárak, amelyek `passport-azure-ad` függ.</span><span class="sxs-lookup"><span data-stu-id="17b31-139">This installs hello libraries that `passport-azure-ad` depends on.</span></span>

## <a name="step-3-set-up-your-app-toouse-hello-passport-node-js-strategy"></a><span data-ttu-id="17b31-140">3. lépés: Az alkalmazás toouse hello passport-csomópont-js stratégia beállítása</span><span class="sxs-lookup"><span data-stu-id="17b31-140">Step 3: Set up your app toouse hello passport-node-js strategy</span></span>
<span data-ttu-id="17b31-141">Itt konfiguráljuk Express toouse hello OpenID Connect hitelesítési protokoll.</span><span class="sxs-lookup"><span data-stu-id="17b31-141">Here, we configure Express toouse hello OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="17b31-142">A Passport is használt toodo probléma be- és kijelentkezési kérések, például különböző dolgokat hello felhasználói munkamenet kezelésére, és hello felhasználó adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="17b31-142">Passport is used toodo various things, including issue sign-in and sign-out requests, manage hello user's session, and get information about hello user.</span></span>

1. <span data-ttu-id="17b31-143">toobegin, nyissa meg hello `config.js` hello gyökerében hello projekt fájlt, és adja meg az alkalmazás konfigurációs értékeit hello `exports.creds` szakasz.</span><span class="sxs-lookup"><span data-stu-id="17b31-143">toobegin, open hello `config.js` file at hello root of hello project, and then enter your app's configuration values in hello `exports.creds` section.</span></span>

  * <span data-ttu-id="17b31-144">Hello `clientID` hello van **alkalmazásazonosító** hello regisztrációs portálon rendelt tooyour alkalmazás esetén.</span><span class="sxs-lookup"><span data-stu-id="17b31-144">hello `clientID` is hello **Application Id** that's assigned tooyour app in hello registration portal.</span></span>

  * <span data-ttu-id="17b31-145">Hello `returnURL` hello van **átirányítási Uri-** hello portálon megadott.</span><span class="sxs-lookup"><span data-stu-id="17b31-145">hello `returnURL` is hello **Redirect Uri** that you entered in hello portal.</span></span>

  * <span data-ttu-id="17b31-146">Hello `clientSecret` hello titkos hello portálon létrehozott van.</span><span class="sxs-lookup"><span data-stu-id="17b31-146">hello `clientSecret` is hello secret that you generated in hello portal.</span></span>

2. <span data-ttu-id="17b31-147">Ezután nyissa meg a hello `app.js` hello hello projekt gyökerében található fájl.</span><span class="sxs-lookup"><span data-stu-id="17b31-147">Next, open hello `app.js` file in hello root of hello project.</span></span> <span data-ttu-id="17b31-148">Majd adja hozzá a következő hívást tooinvoke hello hello `OIDCStrategy` stratégia `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="17b31-148">Then add hello following call tooinvoke hello `OIDCStrategy` strategy that comes with `passport-azure-ad`.</span></span>

    ```JavaScript
    var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

    // add a logger

    var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
    });
    ```

3. <span data-ttu-id="17b31-149">Ezt követően használja azt említett toohandle a bejelentkezési kérelmek hello stratégia.</span><span class="sxs-lookup"><span data-stu-id="17b31-149">After that, use hello strategy we just referenced toohandle our sign-in requests.</span></span>

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
<span data-ttu-id="17b31-150">A Passport az összes stratégia (Twitter-, Facebook-on, és így tovább), amely stratégiafejlesztő a hasonló mintát alkalmaz.</span><span class="sxs-lookup"><span data-stu-id="17b31-150">Passport uses a similar pattern for all its strategies (Twitter, Facebook, and so on) that all strategy writers adhere to.</span></span> <span data-ttu-id="17b31-151">Hello stratégia megnézi, láthatja, hogy azt adja át azt egy függvényt, amely rendelkezik egy jogkivonat és egy kész hello paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="17b31-151">Looking at hello strategy, you see that we pass it a function that has a token and a done as hello parameters.</span></span> <span data-ttu-id="17b31-152">hello stratégia előre vissza toous, miután a tevékenységeket végez el.</span><span class="sxs-lookup"><span data-stu-id="17b31-152">hello strategy comes back toous after it does its work.</span></span> <span data-ttu-id="17b31-153">Majd szeretnénk toostore hello felhasználói és stash hello jogkivonatot, nem szükséges tooask azt újra.</span><span class="sxs-lookup"><span data-stu-id="17b31-153">Then we want toostore hello user and stash hello token so we don't need tooask for it again.</span></span>

> [!IMPORTANT]
<span data-ttu-id="17b31-154">hello előző kód minden olyan felhasználó, akkor fordul elő, tooauthenticate tooour kiszolgáló vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="17b31-154">hello previous code takes any user that happens tooauthenticate tooour server.</span></span> <span data-ttu-id="17b31-155">Ez az úgynevezett automatikus regisztráció.</span><span class="sxs-lookup"><span data-stu-id="17b31-155">This is known as auto-registration.</span></span> <span data-ttu-id="17b31-156">Azt javasoljuk, ne engedélyezze, hogy bárki nélkül azokat, amelyeket a folyamat keresztül regisztrálniuk tooa az üzemi kiszolgáló hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="17b31-156">We    recommend that you don't let anyone authenticate tooa production server without first having them register via a process that you decide on.</span></span> <span data-ttu-id="17b31-157">Ez általában akkor hello megoldást látjuk a fogyasztói alkalmazásoknál, amelyek lehetővé teszik a Facebook tooregister, de majd megkérdezi, hogy tooprovide további információt.</span><span class="sxs-lookup"><span data-stu-id="17b31-157">This is usually hello pattern you see in consumer apps, which allow you tooregister with Facebook but then ask you tooprovide additional information.</span></span> <span data-ttu-id="17b31-158">Ha ez nem egy mintaalkalmazást, kinyerhettük volna hello a felhasználó e-mail címét a hello jogkivonat-objektumból, amely adott vissza, és felkérhettük volna hello felhasználói toofill meg további információkat.</span><span class="sxs-lookup"><span data-stu-id="17b31-158">If this weren't a sample application, we could have extracted hello user's email address from hello token object that is returned and then asked hello user toofill out additional information.</span></span> <span data-ttu-id="17b31-159">Mivel ez egy tesztkiszolgálón, hogy adja hozzá toohello memóriában lévő adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="17b31-159">Because this is a test server, we add them toohello in-memory database.</span></span>


4. <span data-ttu-id="17b31-160">A következő hello módszereket, amelyek lehetővé teszik számunkra tootrack hello bejelentkezett felhasználók a Passport által előírt adjuk hozzá.</span><span class="sxs-lookup"><span data-stu-id="17b31-160">Next, let's add hello methods that enable us tootrack hello signed-in users as required by Passport.</span></span> <span data-ttu-id="17b31-161">Ezek a metódusok szerializálása és deszerializálása hello felhasználói adatok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="17b31-161">These methods include serializing and deserializing hello user's information.</span></span>

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

5.  <span data-ttu-id="17b31-162">A következő hello kód tooload hello Express motor adjuk hozzá.</span><span class="sxs-lookup"><span data-stu-id="17b31-162">Next, let's add hello code tooload hello Express engine.</span></span> <span data-ttu-id="17b31-163">Jelen példában használjuk hello alapértelmezett /views és Express /routes minta nyújt.</span><span class="sxs-lookup"><span data-stu-id="17b31-163">Here we use hello default /views and /routes pattern that Express provides.</span></span>

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

6. <span data-ttu-id="17b31-164">Végezetül adjuk hozzá hello irányítja a tényleges bejelentkezési kérelmek toohello hello átadása `passport-azure-ad` motor:</span><span class="sxs-lookup"><span data-stu-id="17b31-164">Finally, let's add hello routes that hand off hello actual sign-in requests toohello `passport-azure-ad` engine:</span></span>


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


## <a name="step-4-use-passport-tooissue-sign-in-and-sign-out-requests-tooazure-ad"></a><span data-ttu-id="17b31-165">4. lépés: Használatát a Passport tooissue bejelentkezési és kijelentkezési kérések tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="17b31-165">Step 4: Use Passport tooissue sign-in and sign-out requests tooAzure AD</span></span>
<span data-ttu-id="17b31-166">Az alkalmazás már megfelelően konfigurált toocommunicate hello végponttal hello OpenID Connect hitelesítési protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="17b31-166">Your app is now properly configured toocommunicate with hello endpoint by using hello OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="17b31-167">`passport-azure-ad`rendelkezik az összes hello műveletek hitelesítési üzenetek létrehozásával, ellenőrzése az Azure ad-jogkivonatok és karbantartása a felhasználói munkamenetek végrehajtott végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="17b31-167">`passport-azure-ad` has taken care of all hello details of crafting authentication messages, validating tokens from Azure AD, and maintaining user sessions.</span></span> <span data-ttu-id="17b31-168">Marad, hogy a felhasználók megkapják, egy a módon toosign és kijelentkezési, és hello bejelentkezett felhasználók további adatokat gyűjt.</span><span class="sxs-lookup"><span data-stu-id="17b31-168">All that remains is giving your users a way toosign in and sign out, and gathering additional information about hello signed-in users.</span></span>

1. <span data-ttu-id="17b31-169">Első lépésként adjuk hozzá hello alapértelmezett, bejelentkezési, fiókkal, és kijelentkezési metódusokat tooour `app.js` fájlt:</span><span class="sxs-lookup"><span data-stu-id="17b31-169">First, let's add hello default, sign-in, account, and sign-out methods tooour `app.js` file:</span></span>

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

2.  <span data-ttu-id="17b31-170">Tekintsük át részletesen ezek:</span><span class="sxs-lookup"><span data-stu-id="17b31-170">Let's review these in detail:</span></span>

  * <span data-ttu-id="17b31-171">Hello `/`útvonal átirányítja a felhasználókat toohello index.ejs nézet benyújtása hello felhasználói hello kérelem (ha van ilyen).</span><span class="sxs-lookup"><span data-stu-id="17b31-171">hello `/`route redirects toohello index.ejs view, passing hello user in hello request (if it exists).</span></span>
  * <span data-ttu-id="17b31-172">Hello `/account` útvonal először *biztosítja, hogy hitelesítése* (megvalósítása, amely az alábbi példa hello), és ezután fázisok hello-e felhasználói hello kérelem, hogy megkaphassa hello felhasználó további információt.</span><span class="sxs-lookup"><span data-stu-id="17b31-172">hello `/account` route first *ensures we are authenticated* (we implement that in hello following example), and then passes hello user in hello request so that we can get additional information about hello user.</span></span>
  * <span data-ttu-id="17b31-173">Hello `/login` útvonal hívja fel az azuread-openidconnect hitelesítő `passport-azuread`.</span><span class="sxs-lookup"><span data-stu-id="17b31-173">hello `/login` route calls our azuread-openidconnect authenticator from `passport-azuread`.</span></span> <span data-ttu-id="17b31-174">Ha nem jár sikerrel, amely, hello hátsó túl/felhasználónevet irányítja át.</span><span class="sxs-lookup"><span data-stu-id="17b31-174">If that doesn't succeed, it redirects hello user back too/login.</span></span>
  * <span data-ttu-id="17b31-175">Hello `/logout` útvonal egyszerűen meghívja a hello logout.ejs (és útvonal), mely törlése cookie-kat, és a beolvasása hello felhasználói hátsó tooindex.ejs.</span><span class="sxs-lookup"><span data-stu-id="17b31-175">hello `/logout` route simply calls hello logout.ejs (and route), which clears cookies and then returns hello user back tooindex.ejs.</span></span>

3. <span data-ttu-id="17b31-176">Hello utolsó részének `app.js`, adjuk hozzá hello **EnsureAuthenticated** a használt módszer `/account`, mert a korábban bemutatott.</span><span class="sxs-lookup"><span data-stu-id="17b31-176">For hello last part of `app.js`, let's add hello **EnsureAuthenticated** method that is used in `/account`, as shown earlier.</span></span>

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

4. <span data-ttu-id="17b31-177">Végül magát hello kiszolgálót hozzuk létre az `app.js`:</span><span class="sxs-lookup"><span data-stu-id="17b31-177">Finally, let's create hello server itself in `app.js`:</span></span>

```JavaScript

        app.listen(3000);

```


## <a name="step-5-toodisplay-our-user-in-hello-website-create-hello-views-and-routes-in-express"></a><span data-ttu-id="17b31-178">5. lépés: toodisplay hello webhelyen, a felhasználó létrehozása hello nézetek és útvonalak az Express</span><span class="sxs-lookup"><span data-stu-id="17b31-178">Step 5: toodisplay our user in hello website, create hello views and routes in Express</span></span>
<span data-ttu-id="17b31-179">Most `app.js` befejeződött.</span><span class="sxs-lookup"><span data-stu-id="17b31-179">Now `app.js` is complete.</span></span> <span data-ttu-id="17b31-180">A Microsoft egyszerűen tooadd hello útvonalak kell, és megtekinti azt lekérése toohello felhasználó, valamint hello kezelni megjelenítése hello adatokat `/logout` és `/login` létrehozott útvonalakat.</span><span class="sxs-lookup"><span data-stu-id="17b31-180">We simply need tooadd hello routes and views that show hello information we get toohello user, as well as handle hello `/logout` and `/login` routes that we  created.</span></span>

1. <span data-ttu-id="17b31-181">Hozzon létre hello `/routes/index.js` útvonal hello gyökérkönyvtárban.</span><span class="sxs-lookup"><span data-stu-id="17b31-181">Create hello `/routes/index.js` route under hello root directory.</span></span>

    ```JavaScript
                /*
                 * GET home page.
                 */

                exports.index = function(req, res){
                  res.render('index', { title: 'Express' });
                };
    ```

2. <span data-ttu-id="17b31-182">Hozzon létre hello `/routes/user.js` útvonal hello gyökérkönyvtárban.</span><span class="sxs-lookup"><span data-stu-id="17b31-182">Create hello `/routes/user.js` route under hello root directory.</span></span>

                ```JavaScript
                /*
                 * GET users listing.
                 */

                exports.list = function(req, res){
                  res.send("respond with a resource");
                };
                ```

 <span data-ttu-id="17b31-183">Ezek adják át a hello kérelem tooour nézetek, valamint a hello felhasználói, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="17b31-183">These pass along hello request tooour views, including hello user if present.</span></span>

3. <span data-ttu-id="17b31-184">Hozzon létre hello `/views/index.ejs` nézet hello gyökérkönyvtárban.</span><span class="sxs-lookup"><span data-stu-id="17b31-184">Create hello `/views/index.ejs` view under hello root directory.</span></span> <span data-ttu-id="17b31-185">Ez egy egyszerű lap, amely behívja a bejelentkezési és kijelentkezési metódusokat az és lehetővé teszi a us toograb fiók adatait.</span><span class="sxs-lookup"><span data-stu-id="17b31-185">This is a simple page that calls our login and logout methods and enables us toograb account information.</span></span> <span data-ttu-id="17b31-186">Figyelje meg, hogy használhatunk hello feltételes `if (!user)` , a beadott keresztül hello kérelem hello felhasználói bizonyító adatok van bejelentkezett felhasználó.</span><span class="sxs-lookup"><span data-stu-id="17b31-186">Notice that we can use hello conditional `if (!user)` as hello user being passed through in hello request is evidence we have a signed-in user.</span></span>

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

4. <span data-ttu-id="17b31-187">Hozzon létre hello `/views/account.ejs` megtekintése hello gyökérkönyvtárban, így azt további információk is megtekinthetők, amely `passport-azuread` hello felhasználói kérelem állította.</span><span class="sxs-lookup"><span data-stu-id="17b31-187">Create hello `/views/account.ejs` view under hello root directory so that we can view additional information that `passport-azuread` has put in hello user request.</span></span>

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

5. <span data-ttu-id="17b31-188">Ellenőrizze a hely jó elrendezés hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="17b31-188">Let's make this look good by adding a layout.</span></span> <span data-ttu-id="17b31-189">Hozzon létre hello ' / views/layout.ejs' view hello a gyökérkönyvtár.</span><span class="sxs-lookup"><span data-stu-id="17b31-189">Create hello '/views/layout.ejs' view under hello root directory.</span></span>

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

##<a name="next-steps"></a><span data-ttu-id="17b31-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="17b31-190">Next steps</span></span>
<span data-ttu-id="17b31-191">Végezetül hozza létre, és futtassa az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="17b31-191">Finally, build and run your app.</span></span> <span data-ttu-id="17b31-192">Futtatás `node app.js`, és folytassa a túl`http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="17b31-192">Run `node app.js`, and then go too`http://localhost:3000`.</span></span>

<span data-ttu-id="17b31-193">Jelentkezzen be személyes Microsoft-fiókkal vagy egy munkahelyi vagy iskolai fiókkal, és figyelje meg, hogyan hello felhasználói identitás hello /account lista megjelenik.</span><span class="sxs-lookup"><span data-stu-id="17b31-193">Sign in with either a personal Microsoft account or a work or school account, and notice how hello user's identity is reflected in hello /account list.</span></span> <span data-ttu-id="17b31-194">Most már rendelkezik egy webalkalmazást az iparági szabványos protokollok, amely képes hitelesíteni a személyes és munkahelyi vagy iskolai fiókkal rendelkező felhasználók védett.</span><span class="sxs-lookup"><span data-stu-id="17b31-194">You now have a web app that's secured with industry standard protocols that can authenticate users with both their personal and work/school accounts.</span></span>

<span data-ttu-id="17b31-195">Referenciaként hello befejeződött (a konfigurációs értékek nélkül) minta [egy .zip-fájlban is](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="17b31-195">For reference, hello completed sample (without your configuration values) [is provided as a .zip file](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip).</span></span> <span data-ttu-id="17b31-196">Azt is megteheti hogy is klónozza a Githubról:</span><span class="sxs-lookup"><span data-stu-id="17b31-196">Alternatively, you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

<span data-ttu-id="17b31-197">Ön most már továbbléphet az összetettebb témákra.</span><span class="sxs-lookup"><span data-stu-id="17b31-197">You can now move onto more advanced topics.</span></span> <span data-ttu-id="17b31-198">Érdemes lehet tootry:</span><span class="sxs-lookup"><span data-stu-id="17b31-198">You might want tootry:</span></span>

[<span data-ttu-id="17b31-199">Védelem biztosítása webes API-t az Azure ad szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="17b31-199">Secure a Web API with Azure AD</span></span>](active-directory-devquickstarts-webapi-nodejs.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
