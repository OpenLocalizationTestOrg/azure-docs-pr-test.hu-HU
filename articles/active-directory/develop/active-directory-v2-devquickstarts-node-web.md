---
title: "Node.js web app bejelentkezés az Azure Active Directory v2.0 |} Microsoft Docs"
description: "Ismerje meg, hogyan hozhat létre Node.js webalkalmazásokba jelentkezik be egy felhasználó személyes Microsoft-fiókkal és a munkahelyi vagy iskolai fiók használatával."
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
ms.openlocfilehash: 6d49c742f72440e22830915c90de009d9188db2a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-sign-in-to-a-nodejs-web-app"></a><span data-ttu-id="843c5-103">Bejelentkezés felvétele Node.js webalkalmazásokba</span><span class="sxs-lookup"><span data-stu-id="843c5-103">Add sign-in to a Node.js web app</span></span>

> [!NOTE]
> <span data-ttu-id="843c5-104">Nem minden Azure Active Directory forgatókönyvek és funkciók használata a v2.0-végponttól.</span><span class="sxs-lookup"><span data-stu-id="843c5-104">Not all Azure Active Directory scenarios and features work with the v2.0 endpoint.</span></span> <span data-ttu-id="843c5-105">Annak megállapításához, hogy használja a v2.0-végpontra vagy az 1.0-s verziójú végpont, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="843c5-105">To determine whether you should use the v2.0 endpoint or the v1.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 

<span data-ttu-id="843c5-106">Ebben az oktatóanyagban Passport használjuk a következő feladatok elvégzéséhez:</span><span class="sxs-lookup"><span data-stu-id="843c5-106">In this tutorial, we use Passport to do the following tasks:</span></span>

* <span data-ttu-id="843c5-107">A webalkalmazásban jelentkezzen be a felhasználó Azure Active Directory (Azure AD) és a v2.0-végponttól.</span><span class="sxs-lookup"><span data-stu-id="843c5-107">In a web app, sign in the user by using Azure Active Directory (Azure AD) and the v2.0 endpoint.</span></span>
* <span data-ttu-id="843c5-108">A felhasználói információkat jelenítenek meg.</span><span class="sxs-lookup"><span data-stu-id="843c5-108">Display information about the user.</span></span>
* <span data-ttu-id="843c5-109">Jelentkezzen ki az alkalmazásból a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="843c5-109">Sign the user out of the app.</span></span>

<span data-ttu-id="843c5-110">A **Passport** a Node.js-hez készült közbenső hitelesítési szoftver.</span><span class="sxs-lookup"><span data-stu-id="843c5-110">**Passport** is authentication middleware for Node.js.</span></span> <span data-ttu-id="843c5-111">Rugalmas és, moduláris Passport diszkréten eldobása minden Express-alapú vagy restify webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="843c5-111">Flexible and modular, Passport can be unobtrusively dropped into any Express-based or restify web application.</span></span> <span data-ttu-id="843c5-112">Passport stratégiák széles választékát támogatja a hitelesítést egy felhasználónév és jelszó, Facebook-on, Twitter vagy egyéb beállítások használatával.</span><span class="sxs-lookup"><span data-stu-id="843c5-112">In Passport, a comprehensive set of strategies support authentication by using a username and password, Facebook, Twitter, or other options.</span></span> <span data-ttu-id="843c5-113">Kidolgoztunk egy stratégiát, amellyel a szoftver az Azure AD esetében is felhasználható.</span><span class="sxs-lookup"><span data-stu-id="843c5-113">We have developed a strategy for Azure AD.</span></span> <span data-ttu-id="843c5-114">Ebben a cikkben azt mutatja be a modul telepítése, és adja hozzá a az Azure AD `passport-azure-ad` beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="843c5-114">In this article, we show you how to install the module, and then add the Azure AD `passport-azure-ad` plug-in.</span></span>

## <a name="download"></a><span data-ttu-id="843c5-115">Letöltés</span><span class="sxs-lookup"><span data-stu-id="843c5-115">Download</span></span>
<span data-ttu-id="843c5-116">Az oktatóanyag kódjának [karbantartása a GitHubon történik](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs).</span><span class="sxs-lookup"><span data-stu-id="843c5-116">The code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs).</span></span> <span data-ttu-id="843c5-117">Az oktatóanyag ismerteti, hogy is [töltse le az alkalmazás vázát .zip-fájlként](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip) vagy klónozza a vázat:</span><span class="sxs-lookup"><span data-stu-id="843c5-117">To follow the tutorial, you can [download the app's skeleton as a .zip file](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip) or clone the skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

<span data-ttu-id="843c5-118">Ez az oktatóanyag végén az elkészült alkalmazás is beszerezheti.</span><span class="sxs-lookup"><span data-stu-id="843c5-118">You also can get the completed application at the end of this tutorial.</span></span>

## <a name="1-register-an-app"></a><span data-ttu-id="843c5-119">1: az alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="843c5-119">1: Register an app</span></span>
<span data-ttu-id="843c5-120">Hozzon létre egy új alkalmazást [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy hajtsa végre a [a részletes lépéseket](active-directory-v2-app-registration.md) regisztrálnia az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="843c5-120">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow [these detailed steps](active-directory-v2-app-registration.md) to register an app.</span></span> <span data-ttu-id="843c5-121">Ellenőrizze, hogy:</span><span class="sxs-lookup"><span data-stu-id="843c5-121">Make sure you:</span></span>

* <span data-ttu-id="843c5-122">Másolás a **alkalmazásazonosító** az alkalmazáshoz hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="843c5-122">Copy the **Application Id** assigned to your app.</span></span> <span data-ttu-id="843c5-123">Ebben az oktatóanyagban van szükség.</span><span class="sxs-lookup"><span data-stu-id="843c5-123">You need it for this tutorial.</span></span>
* <span data-ttu-id="843c5-124">Adja hozzá a **webes** platform az alkalmazásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="843c5-124">Add the **Web** platform for your app.</span></span>
* <span data-ttu-id="843c5-125">Másolás a **átirányítási URI-** a portálról.</span><span class="sxs-lookup"><span data-stu-id="843c5-125">Copy the **Redirect URI** from the portal.</span></span> <span data-ttu-id="843c5-126">Az alapértelmezett URI értéket kell használnia `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="843c5-126">You must use the default URI value of `urn:ietf:wg:oauth:2.0:oob`.</span></span>

## <a name="2-add-prerequisities-to-your-directory"></a><span data-ttu-id="843c5-127">2: prerequisities adni a könyvtárhoz</span><span class="sxs-lookup"><span data-stu-id="843c5-127">2: Add prerequisities to your directory</span></span>
<span data-ttu-id="843c5-128">Egy parancssorban lépjen nyissa meg a gyökérmappára, ha nem már létezik.</span><span class="sxs-lookup"><span data-stu-id="843c5-128">At a command prompt, change directories to go to your root folder, if you are not already there.</span></span> <span data-ttu-id="843c5-129">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="843c5-129">Run the following commands:</span></span>

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

<span data-ttu-id="843c5-130">Ezenkívül használjuk `passport-azure-ad` a a vázat is használtuk:</span><span class="sxs-lookup"><span data-stu-id="843c5-130">In addition, we use `passport-azure-ad` in the skeleton of the quickstart:</span></span>

* `npm install passport-azure-ad`

<span data-ttu-id="843c5-131">Ez telepíti a könyvtárak, amelyek `passport-azure-ad` használja.</span><span class="sxs-lookup"><span data-stu-id="843c5-131">This installs the libraries that `passport-azure-ad` uses.</span></span>

## <a name="3-set-up-your-app-to-use-the-passport-node-js-strategy"></a><span data-ttu-id="843c5-132">3: az alkalmazás beállítása a passport-csomópont-js stratégia használatára</span><span class="sxs-lookup"><span data-stu-id="843c5-132">3: Set up your app to use the passport-node-js strategy</span></span>
<span data-ttu-id="843c5-133">Állítsa be az Express közbenső szoftvert az OpenID Connect hitelesítési protokoll használandó.</span><span class="sxs-lookup"><span data-stu-id="843c5-133">Set up the Express middleware to use the OpenID Connect authentication protocol.</span></span> <span data-ttu-id="843c5-134">A Passport használatával be- és kijelentkezési kérések kiállítása, a felhasználói munkamenet kezelése és a felhasználó, többek között adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="843c5-134">You use Passport to issue sign-in and sign-out requests, manage the user's session, and get information about the user, among other things.</span></span>

1.  <span data-ttu-id="843c5-135">A projekt gyökérkönyvtárában nyissa meg a Config.js fájlt.</span><span class="sxs-lookup"><span data-stu-id="843c5-135">In the root of the project, open the Config.js file.</span></span> <span data-ttu-id="843c5-136">Az a `exports.creds` területen adja meg az alkalmazás konfigurációs értékeit.</span><span class="sxs-lookup"><span data-stu-id="843c5-136">In the `exports.creds` section, enter your app's configuration values.</span></span>
  
  * <span data-ttu-id="843c5-137">`clientID`: A **alkalmazásazonosító** , amely hozzá van rendelve az alkalmazást az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="843c5-137">`clientID`: The **Application Id** that's assigned to your app in the Azure portal.</span></span>
  * <span data-ttu-id="843c5-138">`returnURL`: A **átirányítási URI-** a portálon megadott.</span><span class="sxs-lookup"><span data-stu-id="843c5-138">`returnURL`: The **Redirect URI** that you entered in the portal.</span></span>
  * <span data-ttu-id="843c5-139">`clientSecret`: A titkos kulcsot, a portálon létrehozott.</span><span class="sxs-lookup"><span data-stu-id="843c5-139">`clientSecret`: The secret that you generated in the portal.</span></span>

2.  <span data-ttu-id="843c5-140">A projekt gyökérkönyvtárában nyissa meg az App.js fájlban.</span><span class="sxs-lookup"><span data-stu-id="843c5-140">In the root of the project, open the App.js file.</span></span> <span data-ttu-id="843c5-141">A OIDCStrategy stratey, amely a meghívni kívánt `passport-azure-ad`, adja hozzá a következő hívást:</span><span class="sxs-lookup"><span data-stu-id="843c5-141">To invoke the OIDCStrategy stratey, which comes with `passport-azure-ad`, add the following call:</span></span>

  ```JavaScript
  var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

  // Add some logging
  var log = bunyan.createLogger({
      name: 'Microsoft OIDC Example Web Application'
  });
  ```

3.  <span data-ttu-id="843c5-142">A bejelentkezési kérések kezelésére, használja a korábban említett stratégiát:</span><span class="sxs-lookup"><span data-stu-id="843c5-142">To handle your sign-in requests, use the strategy you just referenced:</span></span>

  ```JavaScript
  // Use the OIDCStrategy within Passport (section 2)
  //
  //   Strategies in Passport require a `validate` function. The function accepts
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

<span data-ttu-id="843c5-143">A Passport hasonló mintát alkalmaz az összes stratégia (Twitter-, Facebook-on, és így tovább).</span><span class="sxs-lookup"><span data-stu-id="843c5-143">Passport uses a similar pattern for all its strategies (Twitter, Facebook, and so on).</span></span> <span data-ttu-id="843c5-144">Stratégiafejlesztő mintát.</span><span class="sxs-lookup"><span data-stu-id="843c5-144">All strategy writers adhere to the pattern.</span></span> <span data-ttu-id="843c5-145">A stratégia átadni egy `function()` tokent használ, és `done` paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="843c5-145">Pass the strategy a `function()` that uses a token and `done` as parameters.</span></span> <span data-ttu-id="843c5-146">A stratégia ad vissza, miután a tevékenységeket végez el.</span><span class="sxs-lookup"><span data-stu-id="843c5-146">The strategy is returned after it does all its work.</span></span> <span data-ttu-id="843c5-147">Tárolni a felhasználót és menteni a jogkivonatot, így Önnek nem kell megismételni.</span><span class="sxs-lookup"><span data-stu-id="843c5-147">Store the user and stash the token so you don’t need to ask for it again.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="843c5-148">Az előzőekben látható kód minden olyan felhasználó, amely képes hitelesíteni a kiszolgáló vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="843c5-148">The preceding code takes any user that can authenticate to your server.</span></span> <span data-ttu-id="843c5-149">Ez az úgynevezett automatikus regisztráció.</span><span class="sxs-lookup"><span data-stu-id="843c5-149">This is known as auto-registration.</span></span> <span data-ttu-id="843c5-150">Az üzemi kiszolgálón így engedélyezni kívánja bárki, aki nélkül őket nyissa meg a regisztrációs folyamat az Ön által.</span><span class="sxs-lookup"><span data-stu-id="843c5-150">On a production server, you wouldn’t want to let anyone in without first having them go through a registration process that you choose.</span></span> <span data-ttu-id="843c5-151">Ez általában egy mintát, amely a fogyasztói alkalmazásoknál megjelenik az.</span><span class="sxs-lookup"><span data-stu-id="843c5-151">This is usually the pattern that you see in consumer apps.</span></span> <span data-ttu-id="843c5-152">Az alkalmazás is lehetővé teheti, regisztrálni a Facebook, de azt kéri, hogy további adatokat.</span><span class="sxs-lookup"><span data-stu-id="843c5-152">The app might allow you to register with Facebook, but then it asks you to enter additional information.</span></span> <span data-ttu-id="843c5-153">Ha ez az oktatóanyag nem parancssori program alkalmaz, jogkivonat-objektumból visszaadott lehetett kibontani a az e-mailt.</span><span class="sxs-lookup"><span data-stu-id="843c5-153">If you weren't using a command-line program for this tutorial, you could extract the email from the token object that is returned.</span></span> <span data-ttu-id="843c5-154">Ezután meg lehet, hogy kérnie a felhasználót adhat meg további információt.</span><span class="sxs-lookup"><span data-stu-id="843c5-154">Then, you might ask the user to enter additional information.</span></span> <span data-ttu-id="843c5-155">Mivel ez egy tesztkiszolgálón, hozzáadja a felhasználó közvetlenül a memóriában lévő adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="843c5-155">Because this is a test server, you add the user directly to the in-memory database.</span></span>
  > 
  > 

4.  <span data-ttu-id="843c5-156">Adja hozzá a metódusokat, amelyek segítségével nyomon követni a felhasználók, akik van bejelentkezve, a Passport által előírt.</span><span class="sxs-lookup"><span data-stu-id="843c5-156">Add the methods that you use to keep track of users who are signed in, as required by Passport.</span></span> <span data-ttu-id="843c5-157">Ez magában foglalja a szerializálása és deszerializálása során a felhasználó adatait:</span><span class="sxs-lookup"><span data-stu-id="843c5-157">This includes serializing and deserializing the user's information:</span></span>

  ```JavaScript

  // Passport session setup (section 2)

  //   To support persistent login sessions, Passport needs to be able to
  //   serialize users into, and deserialize users out of, the session. Typically,
  //   this is as simple as storing the user ID when serializing, and finding
  //   the user by ID when deserializing.
  passport.serializeUser(function(user, done) {
    done(null, user.email);
  });

  passport.deserializeUser(function(id, done) {
    findByEmail(id, function (err, user) {
      done(err, user);
    });
  });

  // Array to hold signed-in users
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

5.  <span data-ttu-id="843c5-158">Adja hozzá a kódot, amely az Express motor betöltése.</span><span class="sxs-lookup"><span data-stu-id="843c5-158">Add the code that loads the Express engine.</span></span> <span data-ttu-id="843c5-159">Az alapértelmezett /views használ, és Express /routes minta biztosítja:</span><span class="sxs-lookup"><span data-stu-id="843c5-159">You use the default /views and /routes pattern that Express provides:</span></span>

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
    // Initialize Passport!  Also use passport.session() middleware, to support
    // persistent login sessions (recommended).
    app.use(passport.initialize());
    app.use(passport.session());
    app.use(app.router);
    app.use(express.static(__dirname + '/../../public'));
  });

  ```

6.  <span data-ttu-id="843c5-160">Adja hozzá a FELADÁS egy vagy több irányítja, hogy a tényleges bejelentkezési kéréseket átadása a `passport-azure-ad` motor:</span><span class="sxs-lookup"><span data-stu-id="843c5-160">Add the POST routes that hand off the actual sign-in requests to the `passport-azure-ad` engine:</span></span>

  ```JavaScript

  // Auth routes (section 3)

  // GET /auth/openid
  //   Use passport.authenticate() as route middleware to authenticate the
  //   request. The first step in OpenID authentication involves redirecting
  //   the user to the user's OpenID provider. After authenticating, the OpenID
  //   provider redirects the user back to this application at
  //   /auth/openid/return.

  app.get('/auth/openid',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {
      log.info('Authentication was called in the sample');
      res.redirect('/');
    });

  // GET /auth/openid/return
  //   Use passport.authenticate() as route middleware to authenticate the
  //   request. If authentication fails, the user is redirected back to the
  //   sign-in page. Otherwise, the primary route function is called.
  //   In this example, it redirects the user to the home page.
  app.get('/auth/openid/return',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {

      res.redirect('/');
    });

  // POST /auth/openid/return
  //   Use passport.authenticate() as route middleware to authenticate the
  //   request. If authentication fails, the user is redirected back to the
  //   sign-in page. Otherwise, the primary route function is called. 
  //   In this example, it redirects the user to the home page.

  app.post('/auth/openid/return',
    passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
    function(req, res) {

      res.redirect('/');
    });
  ```

## <a name="4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a><span data-ttu-id="843c5-161">4: a be- és kijelentkezési kérések kiállítása az Azure AD használata a Passport</span><span class="sxs-lookup"><span data-stu-id="843c5-161">4: Use Passport to issue sign-in and sign-out requests to Azure AD</span></span>
<span data-ttu-id="843c5-162">Az alkalmazás most már be van állítva az OpenID Connect hitelesítési protokoll használatával kommunikálnak a v2.0-végponttól.</span><span class="sxs-lookup"><span data-stu-id="843c5-162">Your app is now set up to communicate with the v2.0 endpoint by using the OpenID Connect authentication protocol.</span></span> <span data-ttu-id="843c5-163">A `passport-azure-ad` stratégia gondoskodik hitelesítési üzenetek létrehozásával, ellenőrzése az Azure ad-jogkivonatok és karbantartása a felhasználói munkamenet összes részleteit.</span><span class="sxs-lookup"><span data-stu-id="843c5-163">The `passport-azure-ad` strategy takes care of all the details of crafting authentication messages, validating tokens from Azure AD, and maintaining the user session.</span></span> <span data-ttu-id="843c5-164">Ehhez marad csak a kell biztosítani a felhasználóknak olyan módon, bejelentkezés és kijelentkezés, valamint a bejelentkezett felhasználó további információt gyűjteni.</span><span class="sxs-lookup"><span data-stu-id="843c5-164">All that is left to do is to give your users a way to sign in and sign out, and to gather more information about the user who is signed in.</span></span>

1.  <span data-ttu-id="843c5-165">Adja hozzá a **alapértelmezett**, **bejelentkezési**, **fiók**, és **kijelentkezési** az App.js fájl módszerek:</span><span class="sxs-lookup"><span data-stu-id="843c5-165">Add the **default**, **login**, **account**, and **logout** methods to your App.js file:</span></span>

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
      log.info('Login was called in the sample');
      res.redirect('/');
  });

  app.get('/logout', function(req, res){
    req.logout();
    res.redirect('/');
  });

  ```

  <span data-ttu-id="843c5-166">Az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="843c5-166">Here are the details:</span></span>
    
    * <span data-ttu-id="843c5-167">A `/` útvonal átirányítja a felhasználókat a index.ejs nézetet.</span><span class="sxs-lookup"><span data-stu-id="843c5-167">The `/` route redirects to the index.ejs view.</span></span> <span data-ttu-id="843c5-168">Átadja a felhasználó a kérés (ha van ilyen).</span><span class="sxs-lookup"><span data-stu-id="843c5-168">It passes the user in the request (if it exists).</span></span>
    * <span data-ttu-id="843c5-169">A `/account` útvonal először *biztosítja, hogy hitelesítése* (megvalósítása, amely a következő kódrészlet).</span><span class="sxs-lookup"><span data-stu-id="843c5-169">The `/account` route first *ensures that you are authenticated* (you implement that in the following code).</span></span> <span data-ttu-id="843c5-170">Ezt követően átadja a felhasználó a kérelemben.</span><span class="sxs-lookup"><span data-stu-id="843c5-170">Then, it passes the user in the request.</span></span> <span data-ttu-id="843c5-171">Ez az, hogy a felhasználó további információt.</span><span class="sxs-lookup"><span data-stu-id="843c5-171">This is so you can get more information about the user.</span></span>
    * <span data-ttu-id="843c5-172">A `/login` útvonal hívja a `azuread-openidconnect` a hitelesítő `passport-azuread`.</span><span class="sxs-lookup"><span data-stu-id="843c5-172">The `/login` route calls your `azuread-openidconnect` authenticator from `passport-azuread`.</span></span> <span data-ttu-id="843c5-173">Ha, amely nem jár sikerrel, az átirányítja a felhasználó vissza `/login`.</span><span class="sxs-lookup"><span data-stu-id="843c5-173">If that doesn't succeed, it redirects the user back to `/login`.</span></span>
    * <span data-ttu-id="843c5-174">A `/logout` útvonal meghívja a logout.ejs nézet (és).</span><span class="sxs-lookup"><span data-stu-id="843c5-174">The `/logout` route calls the logout.ejs view (and route).</span></span> <span data-ttu-id="843c5-175">Ez törli a cookie-kat, és adja vissza a felhasználót vissza index.ejs.</span><span class="sxs-lookup"><span data-stu-id="843c5-175">This clears cookies, and then returns the user back to index.ejs.</span></span>

2.  <span data-ttu-id="843c5-176">Adja hozzá a **EnsureAuthenticated** , amelyet korábban a használt módszer `/account`:</span><span class="sxs-lookup"><span data-stu-id="843c5-176">Add the **EnsureAuthenticated** method that you used earlier in `/account`:</span></span>

  ```JavaScript

  // Route middleware to ensure the user is authenticated (section 4)

  //   Use this route middleware on any resource that needs to be protected. If
  //   the request is authenticated (typically via a persistent login session),
  //   the request proceeds. Otherwise, the user is redirected to the
  //   sign-in page.
  function ensureAuthenticated(req, res, next) {
    if (req.isAuthenticated()) { return next(); }
    res.redirect('/login')
  }

  ```

3.  <span data-ttu-id="843c5-177">Az App.js fájl a kiszolgáló létrehozása:</span><span class="sxs-lookup"><span data-stu-id="843c5-177">In App.js, create the server:</span></span>

  ```JavaScript

  app.listen(3000);

  ```


## <a name="5-create-the-views-and-routes-in-express-that-you-show-your-user-on-the-website"></a><span data-ttu-id="843c5-178">5: a nézetek és útvonalak létrehozása az Express a felhasználó a webhely megjelenítése</span><span class="sxs-lookup"><span data-stu-id="843c5-178">5: Create the views and routes in Express that you show your user on the website</span></span>
<span data-ttu-id="843c5-179">Adja hozzá az útvonalakat és nézeteket, amelyek az adatok megjelenítése a felhasználónak.</span><span class="sxs-lookup"><span data-stu-id="843c5-179">Add the routes and views that show information to the user.</span></span> <span data-ttu-id="843c5-180">Az útvonalakat és nézeteket is kezelni a `/logout` és `/login` létrehozott útvonalak.</span><span class="sxs-lookup"><span data-stu-id="843c5-180">The routes and views also handle the `/logout` and `/login` routes that you created.</span></span>

1. <span data-ttu-id="843c5-181">Hozzon létre a gyökérkönyvtárban a `/routes/index.js` útvonal.</span><span class="sxs-lookup"><span data-stu-id="843c5-181">In the root directory, create the `/routes/index.js` route.</span></span>

  ```JavaScript

  /*
  * GET home page.
  */

  exports.index = function(req, res){
    res.render('index', { title: 'Express' });
  };
  ```

2.  <span data-ttu-id="843c5-182">Hozzon létre a gyökérkönyvtárban a `/routes/user.js` útvonal.</span><span class="sxs-lookup"><span data-stu-id="843c5-182">In the root directory, create the `/routes/user.js` route.</span></span>

  ```JavaScript

  /*
  * GET users listing.
  */

  exports.list = function(req, res){
    res.send("respond with a resource");
  };
  ```

  <span data-ttu-id="843c5-183">`/routes/index.js`és `/routes/user.js` egyszerű útvonalak adják át a nézetekhez, ha van ilyen, többek között a felhasználó a kérés vannak.</span><span class="sxs-lookup"><span data-stu-id="843c5-183">`/routes/index.js` and `/routes/user.js` are simple routes that pass along the request to your views, including the user, if present.</span></span>

3.  <span data-ttu-id="843c5-184">Hozzon létre a gyökérkönyvtárban a `/views/index.ejs` nézet.</span><span class="sxs-lookup"><span data-stu-id="843c5-184">In the root directory, create the `/views/index.ejs` view.</span></span> <span data-ttu-id="843c5-185">Ezen a lapon meghívja a **bejelentkezési** és **kijelentkezési** módszerek.</span><span class="sxs-lookup"><span data-stu-id="843c5-185">This page calls your **login** and **logout** methods.</span></span> <span data-ttu-id="843c5-186">Is használhatja a `/views/index.ejs` nézetre, és a fiók adatait rögzíti.</span><span class="sxs-lookup"><span data-stu-id="843c5-186">You also use the `/views/index.ejs` view to capture account information.</span></span> <span data-ttu-id="843c5-187">Használhatja a feltételes hozzáférési `if (!user)` átadta-e keresztül a kérelemben szereplő felhasználóként.</span><span class="sxs-lookup"><span data-stu-id="843c5-187">You can use the conditional `if (!user)` as the user being passed through in the request.</span></span> <span data-ttu-id="843c5-188">Fontos, hogy rendelkezik-e a bejelentkezett felhasználó bizonyító adatok.</span><span class="sxs-lookup"><span data-stu-id="843c5-188">It is evidence that you have a user signed in.</span></span>

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

4.  <span data-ttu-id="843c5-189">Hozzon létre a gyökérkönyvtárban a `/views/account.ejs` nézet.</span><span class="sxs-lookup"><span data-stu-id="843c5-189">In the root directory, create the `/views/account.ejs` view.</span></span> <span data-ttu-id="843c5-190">A `/views/account.ejs` nézet lehetővé teszi a további információk megjelenítéséhez, amelyek `passport-azuread` a felhasználói kérelem helyezi.</span><span class="sxs-lookup"><span data-stu-id="843c5-190">The `/views/account.ejs` view allows you to view additional information that `passport-azuread` puts in the user request.</span></span>

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

5.  <span data-ttu-id="843c5-191">Adjon hozzá egy elrendezés.</span><span class="sxs-lookup"><span data-stu-id="843c5-191">Add a layout.</span></span> <span data-ttu-id="843c5-192">Hozzon létre a gyökérkönyvtárban a `/views/layout.ejs` nézet.</span><span class="sxs-lookup"><span data-stu-id="843c5-192">In the root directory, create the `/views/layout.ejs` view.</span></span>

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

6.  <span data-ttu-id="843c5-193">Build és az alkalmazás futtatása futtassa `node app.js`.</span><span class="sxs-lookup"><span data-stu-id="843c5-193">To build and run your app, run `node app.js`.</span></span> <span data-ttu-id="843c5-194">Folytassa a `http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="843c5-194">Then, go to `http://localhost:3000`.</span></span>

7.  <span data-ttu-id="843c5-195">Jelentkezzen be személyes Microsoft-fiókkal vagy egy munkahelyi vagy iskolai fiókkal.</span><span class="sxs-lookup"><span data-stu-id="843c5-195">Sign in with either a personal Microsoft account or a work or school account.</span></span> <span data-ttu-id="843c5-196">Vegye figyelembe, hogy a felhasználó identitását tükröz /account listájában.</span><span class="sxs-lookup"><span data-stu-id="843c5-196">Note that the user's identity is reflected in the /account list.</span></span> 

<span data-ttu-id="843c5-197">Most már rendelkezik egy webalkalmazást, amelyet az iparági szabványos protokollok használatával.</span><span class="sxs-lookup"><span data-stu-id="843c5-197">You now have a web app that is secured by using industry standard protocols.</span></span> <span data-ttu-id="843c5-198">Az alkalmazás a felhasználók a személyes és munkahelyi vagy iskolai fiókok használatával képes hitelesíteni.</span><span class="sxs-lookup"><span data-stu-id="843c5-198">You can authenticate users in your app by using their personal and work or school accounts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="843c5-199">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="843c5-199">Next steps</span></span>
<span data-ttu-id="843c5-200">Az elkészült mintát (a konfigurációs értékek nélkül) referenciaként biztosított [egy .zip fájl](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="843c5-200">For reference, the completed sample (without your configuration values) is provided as [a .zip file](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip).</span></span> <span data-ttu-id="843c5-201">Akkor is klónozhatja azt a Githubról:</span><span class="sxs-lookup"><span data-stu-id="843c5-201">You also can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

<span data-ttu-id="843c5-202">A következő továbbléphet az összetettebb témákra.</span><span class="sxs-lookup"><span data-stu-id="843c5-202">Next, you can move on to more advanced topics.</span></span> <span data-ttu-id="843c5-203">Előfordulhat, hogy ki szeretné próbálni:</span><span class="sxs-lookup"><span data-stu-id="843c5-203">You might want to try:</span></span>

[<span data-ttu-id="843c5-204">Node.js webes API biztonságossá a v2.0-végpontra használatával</span><span class="sxs-lookup"><span data-stu-id="843c5-204">Secure a Node.js web API by using the v2.0 endpoint</span></span>](active-directory-v2-devquickstarts-node-api.md)

<span data-ttu-id="843c5-205">Az alábbiakban néhány további források:</span><span class="sxs-lookup"><span data-stu-id="843c5-205">Here are some additional resources:</span></span>

* [<span data-ttu-id="843c5-206">Az Azure AD v2.0 fejlesztői útmutató</span><span class="sxs-lookup"><span data-stu-id="843c5-206">Azure AD v2.0 developer guide</span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="843c5-207">A verem túlcsordulás "azure-active-directory" címke</span><span class="sxs-lookup"><span data-stu-id="843c5-207">Stack Overflow "azure-active-directory" tag</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

### <a name="get-security-updates-for-our-products"></a><span data-ttu-id="843c5-208">Biztonsági frissítések termékeinkhez</span><span class="sxs-lookup"><span data-stu-id="843c5-208">Get security updates for our products</span></span>
<span data-ttu-id="843c5-209">Javasoljuk, hogy regisztráljon biztonsági incidensek fordulhat elő, ha értesítést szeretne kapni.</span><span class="sxs-lookup"><span data-stu-id="843c5-209">We encourage you to sign up to be notified when security incidents occur.</span></span> <span data-ttu-id="843c5-210">Az a [Microsoft műszaki biztonsági értesítéseket](https://technet.microsoft.com/security/dd252948) lapon, a biztonsági tanácsadók riasztást fizessen elő.</span><span class="sxs-lookup"><span data-stu-id="843c5-210">On the [Microsoft Technical Security Notifications](https://technet.microsoft.com/security/dd252948) page, subscribe to Security Advisories Alerts.</span></span>

