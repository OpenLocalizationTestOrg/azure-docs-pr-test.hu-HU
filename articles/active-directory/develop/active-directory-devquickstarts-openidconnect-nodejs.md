---
title: "Ismerkedés az Azure ad-val be- és kijelentkezési Node.js használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan hozhat létre, amely az Azure ad-val bejelentkezési Node.js Express MVC webalkalmazás."
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
ms.openlocfilehash: 13317b016f9ff3955f376b858645c42668b0de42
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="nodejs-web-app-sign-in-and-sign-out-with-azure-ad"></a><span data-ttu-id="fa5d4-103">NODE.js-webalkalmazás be- és kijelentkezési az Azure ad szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="fa5d4-103">Node.js web app sign-in and sign-out with Azure AD</span></span>
<span data-ttu-id="fa5d4-104">Jelen példában használjuk a Passport:</span><span class="sxs-lookup"><span data-stu-id="fa5d4-104">Here we use Passport to:</span></span>

* <span data-ttu-id="fa5d4-105">A felhasználói bejelentkezés az Azure Active Directory (Azure AD) alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-105">Sign the user in to the app with Azure Active Directory (Azure AD).</span></span>
* <span data-ttu-id="fa5d4-106">A felhasználói információkat jelenítenek meg.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-106">Display information about the user.</span></span>
* <span data-ttu-id="fa5d4-107">Jelentkezzen ki az alkalmazásból a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-107">Sign the user out of the app.</span></span>

<span data-ttu-id="fa5d4-108">A Passport hitelesítési köztes a Node.js is.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-108">Passport is authentication middleware for Node.js.</span></span> <span data-ttu-id="fa5d4-109">Rugalmas és, moduláris Passport diszkréten eldobja bármely Express-alapú vagy restify webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-109">Flexible and modular, Passport can be unobtrusively dropped in to any Express-based or restify web application.</span></span> <span data-ttu-id="fa5d4-110">Stratégiák széles választékát támogatja a felhasználónév és jelszó, Facebook, Twitter, és több használó hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-110">A comprehensive set of strategies support authentication that uses a username and password, Facebook, Twitter, and more.</span></span> <span data-ttu-id="fa5d4-111">Kidolgoztunk is stratégiát a Microsoft Azure Active Directoryhoz.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-111">We have developed a strategy for Microsoft Azure Active Directory.</span></span> <span data-ttu-id="fa5d4-112">Azt telepítenie kell a modult, és vegye fel a Microsoft Azure Active Directory `passport-azure-ad` beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-112">We install this module and then add the Microsoft Azure Active Directory `passport-azure-ad` plug-in.</span></span>

<span data-ttu-id="fa5d4-113">Ehhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="fa5d4-113">To do this, take the following steps:</span></span>

1. <span data-ttu-id="fa5d4-114">Az alkalmazás regisztrálásához.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-114">Register an app.</span></span>
2. <span data-ttu-id="fa5d4-115">Állítsa be az alkalmazás használatához a `passport-azure-ad` stratégia.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-115">Set up your app to use the `passport-azure-ad` strategy.</span></span>
3. <span data-ttu-id="fa5d4-116">Be- és kijelentkezési kérések kiállítása az Azure AD számára a Passport használatával.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-116">Use Passport to issue sign-in and sign-out requests to Azure AD.</span></span>
4. <span data-ttu-id="fa5d4-117">Nyomtassa ki a felhasználóval kapcsolatos adatokat.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-117">Print data about the user.</span></span>

<span data-ttu-id="fa5d4-118">Az oktatóanyag kódjának [karbantartása a GitHubon történik](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS).</span><span class="sxs-lookup"><span data-stu-id="fa5d4-118">The code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS).</span></span>  <span data-ttu-id="fa5d4-119">Követéséhez is [töltse le az alkalmazás vázát .zip-fájlként](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip) vagy klónozza a vázat:</span><span class="sxs-lookup"><span data-stu-id="fa5d4-119">To follow along, you can [download the app's skeleton as a .zip file](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip) or clone the skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

<span data-ttu-id="fa5d4-120">Az elkészült alkalmazást is megtalálja, valamint az oktatóanyag végén.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-120">The completed application is provided at the end of this tutorial as well.</span></span>

## <a name="step-1-register-an-app"></a><span data-ttu-id="fa5d4-121">1. lépés: Az alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="fa5d4-121">Step 1: Register an app</span></span>
1. <span data-ttu-id="fa5d4-122">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fa5d4-122">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="fa5d4-123">A lap felső menüben válassza ki a fiókját.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-123">In the menu at the top of the page, select your account.</span></span> <span data-ttu-id="fa5d4-124">Az a **Directory** menüben válassza ki, hol szeretné az alkalmazás regisztrálása az Active Directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-124">Under the **Directory** list, choose the Active Directory tenant where you want to register your application.</span></span>

3. <span data-ttu-id="fa5d4-125">Válassza ki **több szolgáltatások** a bal oldalán a képernyő, és válassza a menü **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-125">Select **More Services** in the menu on the left side of the screen, and then select **Azure Active Directory**.</span></span>

4. <span data-ttu-id="fa5d4-126">Válassza ki **App regisztrációk**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-126">Select **App registrations**, and then select **Add**.</span></span>

5. <span data-ttu-id="fa5d4-127">Kövesse a megjelenő utasításokat hozzon létre egy **webalkalmazás** és/vagy **WebAPI**.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-127">Follow the prompts to create a **Web Application** and/or **WebAPI**.</span></span>
  * <span data-ttu-id="fa5d4-128">A **neve** az alkalmazás az alkalmazás a felhasználók számára ismerteti.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-128">The **name** of the application describes your application to users.</span></span>

  * <span data-ttu-id="fa5d4-129">A **bejelentkezési URL-cím** az alkalmazás alap URL-címe.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-129">The **Sign-On URL** is the base URL of your app.</span></span>  <span data-ttu-id="fa5d4-130">A vázat alapértelmezett érték a "return http://localhost: 3000/auth/openid nyelvhez.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-130">The skeleton's default is `http://localhost:3000/auth/openid/return`\`.</span></span>

6. <span data-ttu-id="fa5d4-131">Miután regisztrálta, az Azure AD rendeli hozzá az alkalmazás egy egyedi azonosítót.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-131">After you register, Azure AD assigns your app a unique application ID.</span></span> <span data-ttu-id="fa5d4-132">Ez az érték kell a következő szakaszokban, ezért másolja az alkalmazás oldalról.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-132">You need this value in the following sections, so copy it from the application page.</span></span>
7. <span data-ttu-id="fa5d4-133">Az a **beállítások** -> **tulajdonságok** az alkalmazás lapján frissítse a App ID URI.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-133">From the **Settings** -> **Properties** page for your application, update the App ID URI.</span></span> <span data-ttu-id="fa5d4-134">A **App ID URI** az alkalmazás egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-134">The **App ID URI** is a unique identifier for your application.</span></span> <span data-ttu-id="fa5d4-135">Az egyezmény a formátumának a használatára `https://<tenant-domain>/<app-name>`, például: `https://contoso.onmicrosoft.com/my-first-aad-app`.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-135">The convention is to use the format `https://<tenant-domain>/<app-name>`, for example: `https://contoso.onmicrosoft.com/my-first-aad-app`.</span></span>

## <a name="step-2-add-prerequisites-to-your-directory"></a><span data-ttu-id="fa5d4-136">2. lépés: A címtár Előfeltételek felvétele</span><span class="sxs-lookup"><span data-stu-id="fa5d4-136">Step 2: Add prerequisites to your directory</span></span>
1. <span data-ttu-id="fa5d4-137">A parancssorból módosítsa a könyvtárat a gyökérmappára, ha már nem létezik, és futtassa a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="fa5d4-137">From the command line, change directories to your root folder if you're not already there, and then run the following commands:</span></span>

    * `npm install express`
    * `npm install ejs`
    * `npm install ejs-locals`
    * `npm install restify`
    * `npm install mongoose`
    * `npm install bunyan`
    * `npm install assert-plus`
    * `npm install passport`

2. <span data-ttu-id="fa5d4-138">Továbbá szüksége `passport-azure-ad`:</span><span class="sxs-lookup"><span data-stu-id="fa5d4-138">In addition, you need `passport-azure-ad`:</span></span>
    * `npm install passport-azure-ad`

<span data-ttu-id="fa5d4-139">Ez telepíti a könyvtárak, amelyek `passport-azure-ad` függ.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-139">This installs the libraries that `passport-azure-ad` depends on.</span></span>

## <a name="step-3-set-up-your-app-to-use-the-passport-node-js-strategy"></a><span data-ttu-id="fa5d4-140">3. lépés: Az alkalmazás használatának beállítása a passport-csomópont-js stratégia</span><span class="sxs-lookup"><span data-stu-id="fa5d4-140">Step 3: Set up your app to use the passport-node-js strategy</span></span>
<span data-ttu-id="fa5d4-141">Itt azt az OpenID Connect hitelesítési protokoll használatára Express konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-141">Here, we configure Express to use the OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="fa5d4-142">A Passport használatos különböző műveleteket, beleértve a probléma be- és kijelentkezési kérések, a felhasználói munkamenet kezelésére, és a felhasználó adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-142">Passport is used to do various things, including issue sign-in and sign-out requests, manage the user's session, and get information about the user.</span></span>

1. <span data-ttu-id="fa5d4-143">Első lépésként nyissa meg a `config.js` a projekt gyökerében fájlt, és írja be az alkalmazás konfigurációs értékeit a `exports.creds` szakasz.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-143">To begin, open the `config.js` file at the root of the project, and then enter your app's configuration values in the `exports.creds` section.</span></span>

  * <span data-ttu-id="fa5d4-144">A `clientID` van a **alkalmazásazonosító** az alkalmazáshoz a regisztrációs portálon rendelt.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-144">The `clientID` is the **Application Id** that's assigned to your app in the registration portal.</span></span>

  * <span data-ttu-id="fa5d4-145">A `returnURL` van a **átirányítási Uri-** a portálon megadott.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-145">The `returnURL` is the **Redirect Uri** that you entered in the portal.</span></span>

  * <span data-ttu-id="fa5d4-146">A `clientSecret` a titkos kulcsot, a portálon létrehozott van.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-146">The `clientSecret` is the secret that you generated in the portal.</span></span>

2. <span data-ttu-id="fa5d4-147">Ezután nyissa meg a `app.js` fájlt a projekt gyökérkönyvtárában található.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-147">Next, open the `app.js` file in the root of the project.</span></span> <span data-ttu-id="fa5d4-148">Majd adja hozzá a következő hívást meghívása a `OIDCStrategy` stratégia `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-148">Then add the following call to invoke the `OIDCStrategy` strategy that comes with `passport-azure-ad`.</span></span>

    ```JavaScript
    var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

    // add a logger

    var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
    });
    ```

3. <span data-ttu-id="fa5d4-149">Ezt követően a stratégia azt említett a bejelentkezési kérések kezelésére használja.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-149">After that, use the strategy we just referenced to handle our sign-in requests.</span></span>

    ```JavaScript
    // Use the OIDCStrategy within Passport. (Section 2)
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
<span data-ttu-id="fa5d4-150">A Passport az összes stratégia (Twitter-, Facebook-on, és így tovább), amely stratégiafejlesztő a hasonló mintát alkalmaz.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-150">Passport uses a similar pattern for all its strategies (Twitter, Facebook, and so on) that all strategy writers adhere to.</span></span> <span data-ttu-id="fa5d4-151">Megnézzük a stratégiát, láthatja, hogy azt adja át azt egy függvény, amely rendelkezik egy jogkivonat és kész a paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-151">Looking at the strategy, you see that we pass it a function that has a token and a done as the parameters.</span></span> <span data-ttu-id="fa5d4-152">A stratégia ismét elérhető lesz a számunkra után a tevékenységeket végez el.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-152">The strategy comes back to us after it does its work.</span></span> <span data-ttu-id="fa5d4-153">Majd szeretnénk tárolni a felhasználót és menteni a jogkivonatot, így azt nem kell megismételni.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-153">Then we want to store the user and stash the token so we don't need to ask for it again.</span></span>

> [!IMPORTANT]
<span data-ttu-id="fa5d4-154">Az előző kód készít minden olyan felhasználó, hogy a kiszolgáló hitelesítésére történik.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-154">The previous code takes any user that happens to authenticate to our server.</span></span> <span data-ttu-id="fa5d4-155">Ez az úgynevezett automatikus regisztráció.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-155">This is known as auto-registration.</span></span> <span data-ttu-id="fa5d4-156">Azt javasoljuk, ne engedélyezze, hogy bárki nélkül azokat, amelyeket a folyamat használatával regisztrálja azokat egy üzemi kiszolgálóra hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-156">We    recommend that you don't let anyone authenticate to a production server without first having them register via a process that you decide on.</span></span> <span data-ttu-id="fa5d4-157">Ez általában az a megoldást látjuk a fogyasztói alkalmazásoknál, melyek lehetővé teszik regisztrálni a Facebook, de majd megkérdezi, hogy kiegészítő információkat.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-157">This is usually the pattern you see in consumer apps, which allow you to register with Facebook but then ask you to provide additional information.</span></span> <span data-ttu-id="fa5d4-158">Ha ez nem egy mintaalkalmazást, kinyerhettük volna a felhasználó e-mail címe jogkivonat-objektumból, amely adott vissza, és felkérhettük volna a felhasználót, hogy további adatokat.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-158">If this weren't a sample application, we could have extracted the user's email address from the token object that is returned and then asked the user to fill out additional information.</span></span> <span data-ttu-id="fa5d4-159">Mivel ez egy tesztkiszolgálón, hogy adja hozzá a memóriában lévő adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-159">Because this is a test server, we add them to the in-memory database.</span></span>


4. <span data-ttu-id="fa5d4-160">A következő adjuk hozzá a módszereket, amelyek lehetővé teszik a számunkra, hogy a Passport által előírt bejelentkezett felhasználók nyomon követése.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-160">Next, let's add the methods that enable us to track the signed-in users as required by Passport.</span></span> <span data-ttu-id="fa5d4-161">Ezek a metódusok lehetnek szerializálása és deszerializálása során a felhasználó adatait.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-161">These methods include serializing and deserializing the user's information.</span></span>

    ```JavaScript

            // Passport session setup. (Section 2)

            //   To support persistent sign-in sessions, Passport needs to be able to
            //   serialize users into the session and deserialize them out of the session. Typically,
            //   this is done simply by storing the user ID when serializing and finding
            //   the user by ID when deserializing.
            passport.serializeUser(function(user, done) {
            done(null, user.email);
            });

            passport.deserializeUser(function(id, done) {
            findByEmail(id, function (err, user) {
                done(err, user);
            });
            });

            // array to hold signed-in users
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

5.  <span data-ttu-id="fa5d4-162">A következő adjuk hozzá a kódot az Express motor betöltése.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-162">Next, let's add the code to load the Express engine.</span></span> <span data-ttu-id="fa5d4-163">Jelen példában használjuk a alapértelmezett /views és Express /routes minta nyújt.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-163">Here we use the default /views and /routes pattern that Express provides.</span></span>

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
          // Initialize Passport!  Also use passport.session() middleware, to support
          // persistent login sessions (recommended).
          app.use(passport.initialize());
          app.use(passport.session());
          app.use(app.router);
          app.use(express.static(__dirname + '/../../public'));
        });

    ```

6. <span data-ttu-id="fa5d4-164">Végezetül adjuk hozzá az útvonalakat, amelyek kiosztják a tényleges bejelentkezési kéréseket a `passport-azure-ad` motor:</span><span class="sxs-lookup"><span data-stu-id="fa5d4-164">Finally, let's add the routes that hand off the actual sign-in requests to the `passport-azure-ad` engine:</span></span>


       ```JavaScript

        // Our Auth routes (section 3)

        // GET /auth/openid
        //   Use passport.authenticate() as route middleware to authenticate the
        //   request. The first step in OpenID authentication involves redirecting
        //   the user to their OpenID provider. After authenticating, the OpenID
        //   provider redirects the user back to this application at
        //   /auth/openid/return.
        app.get('/auth/openid',
        passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
        function(req, res) {
            log.info('Authentication was called in the Sample');
            res.redirect('/');
        });

            // GET /auth/openid/return
            //   Use passport.authenticate() as route middleware to authenticate the
            //   request. If authentication fails, the user is redirected back to the
            //   sign-in page. Otherwise, the primary route function is called,
            //   which, in this example, redirects the user to the home page.
            app.get('/auth/openid/return',
              passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
              function(req, res) {
                log.info('We received a return from AzureAD.');
                res.redirect('/');
              });

            // POST /auth/openid/return
            //   Use passport.authenticate() as route middleware to authenticate the
            //   request. If authentication fails, the user is redirected back to the
            //   sign-in page. Otherwise, the primary route function is called,
            //   which, in this example, redirects the user to the home page.
            app.post('/auth/openid/return',
              passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
              function(req, res) {
                log.info('We received a return from AzureAD.');
                res.redirect('/');
              });
       ```


## <a name="step-4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a><span data-ttu-id="fa5d4-165">4. lépés: Használatát a Passport való be- és kijelentkezési kérések kiállítása az Azure AD</span><span class="sxs-lookup"><span data-stu-id="fa5d4-165">Step 4: Use Passport to issue sign-in and sign-out requests to Azure AD</span></span>
<span data-ttu-id="fa5d4-166">Az alkalmazás megfelelően konfigurálva van az endpoint OpenID Connect hitelesítési protokoll használatával kommunikálnak.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-166">Your app is now properly configured to communicate with the endpoint by using the OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="fa5d4-167">`passport-azure-ad`rendelkezik az összes adat hitelesítési üzenetek létrehozásával, ellenőrzése az Azure ad-jogkivonatok és karbantartása a felhasználói munkamenetek végrehajtott fontos.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-167">`passport-azure-ad` has taken care of all the details of crafting authentication messages, validating tokens from Azure AD, and maintaining user sessions.</span></span> <span data-ttu-id="fa5d4-168">Összes fennmaradó jogosultságot ad a felhasználók bejelentkezés és kijelentkezés, és a bejelentkezett felhasználókkal kapcsolatos további információk összegyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-168">All that remains is giving your users a way to sign in and sign out, and gathering additional information about the signed-in users.</span></span>

1. <span data-ttu-id="fa5d4-169">Első lépésként adjuk hozzá az alapértelmezett, bejelentkezési, fiókkal, és kijelentkezési metódusokat az `app.js` fájlt:</span><span class="sxs-lookup"><span data-stu-id="fa5d4-169">First, let's add the default, sign-in, account, and sign-out methods to our `app.js` file:</span></span>

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
            log.info('Login was called in the Sample');
            res.redirect('/');
        });

        app.get('/logout', function(req, res){
          req.logout();
          res.redirect('/');
        });

    ```

2.  <span data-ttu-id="fa5d4-170">Tekintsük át részletesen ezek:</span><span class="sxs-lookup"><span data-stu-id="fa5d4-170">Let's review these in detail:</span></span>

  * <span data-ttu-id="fa5d4-171">A `/`útvonal átirányítja a felhasználókat a index.ejs nézetet, a felhasználó átadja a kérésben (ha van ilyen).</span><span class="sxs-lookup"><span data-stu-id="fa5d4-171">The `/`route redirects to the index.ejs view, passing the user in the request (if it exists).</span></span>
  * <span data-ttu-id="fa5d4-172">A `/account` útvonal először *biztosítja, hogy hitelesítése* (megvalósítása, amely a következő példában), és a felhasználó majd átadja a kérésben, hogy megkaphassa a felhasználó további információt.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-172">The `/account` route first *ensures we are authenticated* (we implement that in the following example), and then passes the user in the request so that we can get additional information about the user.</span></span>
  * <span data-ttu-id="fa5d4-173">A `/login` útvonal hívja fel az azuread-openidconnect hitelesítő `passport-azuread`.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-173">The `/login` route calls our azuread-openidconnect authenticator from `passport-azuread`.</span></span> <span data-ttu-id="fa5d4-174">Ha nem jár sikerrel, amely, vissza a Publication átirányítja a felhasználót.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-174">If that doesn't succeed, it redirects the user back to /login.</span></span>
  * <span data-ttu-id="fa5d4-175">A `/logout` útvonal egyszerűen hívja a logout.ejs (és útvonal), amely törli a cookie-kat, majd visszaadja a felhasználó vissza a index.ejs.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-175">The `/logout` route simply calls the logout.ejs (and route), which clears cookies and then returns the user back to index.ejs.</span></span>

3. <span data-ttu-id="fa5d4-176">Az utolsó részének `app.js`, adjuk hozzá a **EnsureAuthenticated** a használt módszer `/account`, mert a korábban bemutatott.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-176">For the last part of `app.js`, let's add the **EnsureAuthenticated** method that is used in `/account`, as shown earlier.</span></span>

    ```JavaScript

        // Simple route middleware to ensure user is authenticated. (section 4)

        //   Use this route middleware on any resource that needs to be protected. If
        //   the request is authenticated (typically via a persistent sign-in session),
        //   the request proceeds. Otherwise, the user is redirected to the
        //   sign-in page.
        function ensureAuthenticated(req, res, next) {
          if (req.isAuthenticated()) { return next(); }
          res.redirect('/login')
        }
    ```

4. <span data-ttu-id="fa5d4-177">Végül hozzon létre magát a kiszolgálót a `app.js`:</span><span class="sxs-lookup"><span data-stu-id="fa5d4-177">Finally, let's create the server itself in `app.js`:</span></span>

```JavaScript

        app.listen(3000);

```


## <a name="step-5-to-display-our-user-in-the-website-create-the-views-and-routes-in-express"></a><span data-ttu-id="fa5d4-178">5. lépés: Szeretné megjeleníteni a felhasználó a webhelyen, hozza létre a nézetek és útvonalak az Express</span><span class="sxs-lookup"><span data-stu-id="fa5d4-178">Step 5: To display our user in the website, create the views and routes in Express</span></span>
<span data-ttu-id="fa5d4-179">Most `app.js` befejeződött.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-179">Now `app.js` is complete.</span></span> <span data-ttu-id="fa5d4-180">Egyszerűen kell hozzáadni az útvonalakat és nézeteket, amelyek megjelenítik az adatokat, azt a felhasználó eléréséhez, valamint kezelni a `/logout` és `/login` létrehozott útvonalakat.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-180">We simply need to add the routes and views that show the information we get to the user, as well as handle the `/logout` and `/login` routes that we  created.</span></span>

1. <span data-ttu-id="fa5d4-181">Hozza létre a gyökérkönyvtárban a `/routes/index.js` útvonalat.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-181">Create the `/routes/index.js` route under the root directory.</span></span>

    ```JavaScript
                /*
                 * GET home page.
                 */

                exports.index = function(req, res){
                  res.render('index', { title: 'Express' });
                };
    ```

2. <span data-ttu-id="fa5d4-182">Hozza létre a gyökérkönyvtárban a `/routes/user.js` útvonalat.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-182">Create the `/routes/user.js` route under the root directory.</span></span>

                ```JavaScript
                /*
                 * GET users listing.
                 */

                exports.list = function(req, res){
                  res.send("respond with a resource");
                };
                ```

 <span data-ttu-id="fa5d4-183">Ezek adják át a kérést a nézetekhez, beleértve a felhasználó, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-183">These pass along the request to our views, including the user if present.</span></span>

3. <span data-ttu-id="fa5d4-184">Hozza létre a gyökérkönyvtárban a `/views/index.ejs` útvonalat.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-184">Create the `/views/index.ejs` view under the root directory.</span></span> <span data-ttu-id="fa5d4-185">Ez az egy egyszerű lap, amely a bejelentkezési és kijelentkezési módszereket hív meg, és lehetővé teszi, hogy a fiók adatait adása.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-185">This is a simple page that calls our login and logout methods and enables us to grab account information.</span></span> <span data-ttu-id="fa5d4-186">Figyelje meg, hogy a feltételes hozzáférési használhatunk `if (!user)` átadta-e keresztül a kérelem a felhasználó a bejelentkezett felhasználó tudunk bizonyító adatok lesz.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-186">Notice that we can use the conditional `if (!user)` as the user being passed through in the request is evidence we have a signed-in user.</span></span>

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

4. <span data-ttu-id="fa5d4-187">Hozzon létre a `/views/account.ejs` megtekintése a gyökérkönyvtárban, így azt további információk is megtekinthetők, amely `passport-azuread` állította a felhasználói kérelemben.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-187">Create the `/views/account.ejs` view under the root directory so that we can view additional information that `passport-azuread` has put in the user request.</span></span>

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

5. <span data-ttu-id="fa5d4-188">Ellenőrizze a hely jó elrendezés hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-188">Let's make this look good by adding a layout.</span></span> <span data-ttu-id="fa5d4-189">Hozzon létre a ' / views/layout.ejs' nézetet a gyökérkönyvtárban.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-189">Create the '/views/layout.ejs' view under the root directory.</span></span>

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

##<a name="next-steps"></a><span data-ttu-id="fa5d4-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fa5d4-190">Next steps</span></span>
<span data-ttu-id="fa5d4-191">Végezetül hozza létre, és futtassa az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-191">Finally, build and run your app.</span></span> <span data-ttu-id="fa5d4-192">Futtatás `node app.js`, majd lépjen `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-192">Run `node app.js`, and then go to `http://localhost:3000`.</span></span>

<span data-ttu-id="fa5d4-193">Jelentkezzen be személyes Microsoft-fiókkal vagy egy munkahelyi vagy iskolai fiókkal, és figyelje meg, hogyan jelenik meg a felhasználó identitását /account listájában.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-193">Sign in with either a personal Microsoft account or a work or school account, and notice how the user's identity is reflected in the /account list.</span></span> <span data-ttu-id="fa5d4-194">Most már rendelkezik egy webalkalmazást az iparági szabványos protokollok, amely képes hitelesíteni a személyes és munkahelyi vagy iskolai fiókkal rendelkező felhasználók védett.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-194">You now have a web app that's secured with industry standard protocols that can authenticate users with both their personal and work/school accounts.</span></span>

<span data-ttu-id="fa5d4-195">Az elkészült mintát (a konfigurációs értékek nélkül) referenciaként [.zip-fájlban is letöltheti](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="fa5d4-195">For reference, the completed sample (without your configuration values) [is provided as a .zip file](https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS/archive/complete.zip).</span></span> <span data-ttu-id="fa5d4-196">Azt is megteheti hogy is klónozza a Githubról:</span><span class="sxs-lookup"><span data-stu-id="fa5d4-196">Alternatively, you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/WebApp-OpenIDConnect-NodeJS.git```

<span data-ttu-id="fa5d4-197">Ön most már továbbléphet az összetettebb témákra.</span><span class="sxs-lookup"><span data-stu-id="fa5d4-197">You can now move onto more advanced topics.</span></span> <span data-ttu-id="fa5d4-198">Előfordulhat, hogy ki szeretné próbálni:</span><span class="sxs-lookup"><span data-stu-id="fa5d4-198">You might want to try:</span></span>

[<span data-ttu-id="fa5d4-199">Védelem biztosítása webes API-t az Azure ad szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="fa5d4-199">Secure a Web API with Azure AD</span></span>](active-directory-devquickstarts-webapi-nodejs.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
