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
# <a name="add-sign-in-tooa-nodejs-web-app"></a><span data-ttu-id="24da8-103">Bejelentkezési tooa Node.js webalkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="24da8-103">Add sign-in tooa Node.js web app</span></span>

> [!NOTE]
> <span data-ttu-id="24da8-104">Nem minden Azure Active Directory forgatókönyvek és funkciók együttműködve hello v2.0-végponttól.</span><span class="sxs-lookup"><span data-stu-id="24da8-104">Not all Azure Active Directory scenarios and features work with hello v2.0 endpoint.</span></span> <span data-ttu-id="24da8-105">toodetermine kell használnia a v2.0-végponttól hello vagy hello 1.0-s verziójú végpontot, hogy olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="24da8-105">toodetermine whether you should use hello v2.0 endpoint or hello v1.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 

<span data-ttu-id="24da8-106">Ebben az oktatóanyagban használjuk Passport toodo hello a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="24da8-106">In this tutorial, we use Passport toodo hello following tasks:</span></span>

* <span data-ttu-id="24da8-107">A web app alkalmazásban hello felhasználói bejelentkezés Azure Active Directory (Azure AD) segítségével, és hello v2.0-végponttól.</span><span class="sxs-lookup"><span data-stu-id="24da8-107">In a web app, sign in hello user by using Azure Active Directory (Azure AD) and hello v2.0 endpoint.</span></span>
* <span data-ttu-id="24da8-108">Hello felhasználói információkat jelenítenek meg.</span><span class="sxs-lookup"><span data-stu-id="24da8-108">Display information about hello user.</span></span>
* <span data-ttu-id="24da8-109">Bejelentkezési hello felhasználói hello alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="24da8-109">Sign hello user out of hello app.</span></span>

<span data-ttu-id="24da8-110">A **Passport** a Node.js-hez készült közbenső hitelesítési szoftver.</span><span class="sxs-lookup"><span data-stu-id="24da8-110">**Passport** is authentication middleware for Node.js.</span></span> <span data-ttu-id="24da8-111">Rugalmas és, moduláris Passport diszkréten eldobása minden Express-alapú vagy restify webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="24da8-111">Flexible and modular, Passport can be unobtrusively dropped into any Express-based or restify web application.</span></span> <span data-ttu-id="24da8-112">Passport stratégiák széles választékát támogatja a hitelesítést egy felhasználónév és jelszó, Facebook-on, Twitter vagy egyéb beállítások használatával.</span><span class="sxs-lookup"><span data-stu-id="24da8-112">In Passport, a comprehensive set of strategies support authentication by using a username and password, Facebook, Twitter, or other options.</span></span> <span data-ttu-id="24da8-113">Kidolgoztunk egy stratégiát, amellyel a szoftver az Azure AD esetében is felhasználható.</span><span class="sxs-lookup"><span data-stu-id="24da8-113">We have developed a strategy for Azure AD.</span></span> <span data-ttu-id="24da8-114">Ebben a cikkben megmutatjuk, hogyan tooinstall hello modul, és adja hozzá az Azure AD hello `passport-azure-ad` beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="24da8-114">In this article, we show you how tooinstall hello module, and then add hello Azure AD `passport-azure-ad` plug-in.</span></span>

## <a name="download"></a><span data-ttu-id="24da8-115">Letöltés</span><span class="sxs-lookup"><span data-stu-id="24da8-115">Download</span></span>
<span data-ttu-id="24da8-116">az oktatóanyag kódjának hello kezelt [a Githubon](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs).</span><span class="sxs-lookup"><span data-stu-id="24da8-116">hello code for this tutorial is maintained [on GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs).</span></span> <span data-ttu-id="24da8-117">toofollow hello oktatóanyagban is [hello alkalmazás vázát .zip fájl letöltése](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip) vagy a Klónozás hello vázat:</span><span class="sxs-lookup"><span data-stu-id="24da8-117">toofollow hello tutorial, you can [download hello app's skeleton as a .zip file](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip) or clone hello skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

<span data-ttu-id="24da8-118">Ez az oktatóanyag végén hello befejeződött hello alkalmazás is beszerezheti.</span><span class="sxs-lookup"><span data-stu-id="24da8-118">You also can get hello completed application at hello end of this tutorial.</span></span>

## <a name="1-register-an-app"></a><span data-ttu-id="24da8-119">1: az alkalmazás regisztrálása</span><span class="sxs-lookup"><span data-stu-id="24da8-119">1: Register an app</span></span>
<span data-ttu-id="24da8-120">Hozzon létre egy új alkalmazást [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), vagy hajtsa végre a [a részletes lépéseket](active-directory-v2-app-registration.md) tooregister egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="24da8-120">Create a new app at [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), or follow [these detailed steps](active-directory-v2-app-registration.md) tooregister an app.</span></span> <span data-ttu-id="24da8-121">Ellenőrizze, hogy:</span><span class="sxs-lookup"><span data-stu-id="24da8-121">Make sure you:</span></span>

* <span data-ttu-id="24da8-122">Másolás hello **alkalmazásazonosító** tooyour app rendelve.</span><span class="sxs-lookup"><span data-stu-id="24da8-122">Copy hello **Application Id** assigned tooyour app.</span></span> <span data-ttu-id="24da8-123">Ebben az oktatóanyagban van szükség.</span><span class="sxs-lookup"><span data-stu-id="24da8-123">You need it for this tutorial.</span></span>
* <span data-ttu-id="24da8-124">Adja hozzá a hello **webes** platform az alkalmazásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="24da8-124">Add hello **Web** platform for your app.</span></span>
* <span data-ttu-id="24da8-125">Másolás hello **átirányítási URI-** hello portálról.</span><span class="sxs-lookup"><span data-stu-id="24da8-125">Copy hello **Redirect URI** from hello portal.</span></span> <span data-ttu-id="24da8-126">Hello alapértelmezett URI azonosító értékét kell használnia `urn:ietf:wg:oauth:2.0:oob`.</span><span class="sxs-lookup"><span data-stu-id="24da8-126">You must use hello default URI value of `urn:ietf:wg:oauth:2.0:oob`.</span></span>

## <a name="2-add-prerequisities-tooyour-directory"></a><span data-ttu-id="24da8-127">2: prerequisities tooyour könyvtár hozzáadása</span><span class="sxs-lookup"><span data-stu-id="24da8-127">2: Add prerequisities tooyour directory</span></span>
<span data-ttu-id="24da8-128">Egy parancssorba módosítása könyvtárak toogo tooyour gyökérmappára, ha nem már létezik.</span><span class="sxs-lookup"><span data-stu-id="24da8-128">At a command prompt, change directories toogo tooyour root folder, if you are not already there.</span></span> <span data-ttu-id="24da8-129">Futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="24da8-129">Run hello following commands:</span></span>

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

<span data-ttu-id="24da8-130">Ezenkívül használjuk `passport-azure-ad` a hello vázat a hello gyors üzembe helyezés:</span><span class="sxs-lookup"><span data-stu-id="24da8-130">In addition, we use `passport-azure-ad` in hello skeleton of hello quickstart:</span></span>

* `npm install passport-azure-ad`

<span data-ttu-id="24da8-131">Ezzel telepít hello tárak, amelyek `passport-azure-ad` használja.</span><span class="sxs-lookup"><span data-stu-id="24da8-131">This installs hello libraries that `passport-azure-ad` uses.</span></span>

## <a name="3-set-up-your-app-toouse-hello-passport-node-js-strategy"></a><span data-ttu-id="24da8-132">3: állítsa be az alkalmazás toouse hello passport-csomópont-js stratégia</span><span class="sxs-lookup"><span data-stu-id="24da8-132">3: Set up your app toouse hello passport-node-js strategy</span></span>
<span data-ttu-id="24da8-133">Hello Express köztes toouse hello OpenID Connect hitelesítési protokoll beállítása.</span><span class="sxs-lookup"><span data-stu-id="24da8-133">Set up hello Express middleware toouse hello OpenID Connect authentication protocol.</span></span> <span data-ttu-id="24da8-134">Használjon Passport tooissue be- és kijelentkezési kérések, hello felhasználói munkamenet kezelésére, és többek között a hello felhasználó adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="24da8-134">You use Passport tooissue sign-in and sign-out requests, manage hello user's session, and get information about hello user, among other things.</span></span>

1.  <span data-ttu-id="24da8-135">Hello projekt gyökerében hello nyissa meg hello Config.js fájlt.</span><span class="sxs-lookup"><span data-stu-id="24da8-135">In hello root of hello project, open hello Config.js file.</span></span> <span data-ttu-id="24da8-136">A hello `exports.creds` területen adja meg az alkalmazás konfigurációs értékeit.</span><span class="sxs-lookup"><span data-stu-id="24da8-136">In hello `exports.creds` section, enter your app's configuration values.</span></span>
  
  * <span data-ttu-id="24da8-137">`clientID`: hello **alkalmazásazonosító** Ez az alkalmazás hozzárendelt tooyour hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="24da8-137">`clientID`: hello **Application Id** that's assigned tooyour app in hello Azure portal.</span></span>
  * <span data-ttu-id="24da8-138">`returnURL`: hello **átirányítási URI-** hello portálon megadott.</span><span class="sxs-lookup"><span data-stu-id="24da8-138">`returnURL`: hello **Redirect URI** that you entered in hello portal.</span></span>
  * <span data-ttu-id="24da8-139">`clientSecret`: hello portálon létrehozott hello titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="24da8-139">`clientSecret`: hello secret that you generated in hello portal.</span></span>

2.  <span data-ttu-id="24da8-140">Hello projekt gyökerében hello nyissa meg hello App.js fájlban.</span><span class="sxs-lookup"><span data-stu-id="24da8-140">In hello root of hello project, open hello App.js file.</span></span> <span data-ttu-id="24da8-141">a tooinvoke hello OIDCStrategy stratey `passport-azure-ad`, adja hozzá a következő hívást hello:</span><span class="sxs-lookup"><span data-stu-id="24da8-141">tooinvoke hello OIDCStrategy stratey, which comes with `passport-azure-ad`, add hello following call:</span></span>

  ```JavaScript
  var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

  // Add some logging
  var log = bunyan.createLogger({
      name: 'Microsoft OIDC Example Web Application'
  });
  ```

3.  <span data-ttu-id="24da8-142">toohandle a bejelentkezési kérelmek, használja a korábban említett stratégiát hello:</span><span class="sxs-lookup"><span data-stu-id="24da8-142">toohandle your sign-in requests, use hello strategy you just referenced:</span></span>

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

<span data-ttu-id="24da8-143">A Passport hasonló mintát alkalmaz az összes stratégia (Twitter-, Facebook-on, és így tovább).</span><span class="sxs-lookup"><span data-stu-id="24da8-143">Passport uses a similar pattern for all its strategies (Twitter, Facebook, and so on).</span></span> <span data-ttu-id="24da8-144">Stratégiafejlesztő toohello mintát.</span><span class="sxs-lookup"><span data-stu-id="24da8-144">All strategy writers adhere toohello pattern.</span></span> <span data-ttu-id="24da8-145">Hello stratégia átadni egy `function()` tokent használ, és `done` paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="24da8-145">Pass hello strategy a `function()` that uses a token and `done` as parameters.</span></span> <span data-ttu-id="24da8-146">hello stratégia ad vissza, miután a tevékenységeket végez el.</span><span class="sxs-lookup"><span data-stu-id="24da8-146">hello strategy is returned after it does all its work.</span></span> <span data-ttu-id="24da8-147">Hello felhasználói és stash hello token tárolására, így nem kell tooask azt újra.</span><span class="sxs-lookup"><span data-stu-id="24da8-147">Store hello user and stash hello token so you don’t need tooask for it again.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="24da8-148">hello előző kód készít minden olyan felhasználó, amely képes hitelesíteni tooyour kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="24da8-148">hello preceding code takes any user that can authenticate tooyour server.</span></span> <span data-ttu-id="24da8-149">Ez az úgynevezett automatikus regisztráció.</span><span class="sxs-lookup"><span data-stu-id="24da8-149">This is known as auto-registration.</span></span> <span data-ttu-id="24da8-150">Az üzemi kiszolgálón általában csak akkor érdemes toolet bárki, aki nélkül őket nyissa meg az Ön által regisztrációs folyamatot.</span><span class="sxs-lookup"><span data-stu-id="24da8-150">On a production server, you wouldn’t want toolet anyone in without first having them go through a registration process that you choose.</span></span> <span data-ttu-id="24da8-151">Ez általában az, hogy a fogyasztói alkalmazásoknál hello mintát.</span><span class="sxs-lookup"><span data-stu-id="24da8-151">This is usually hello pattern that you see in consumer apps.</span></span> <span data-ttu-id="24da8-152">hello alkalmazást is lehetővé teheti a Facebook tooregister, de majd kéri tooenter további információt.</span><span class="sxs-lookup"><span data-stu-id="24da8-152">hello app might allow you tooregister with Facebook, but then it asks you tooenter additional information.</span></span> <span data-ttu-id="24da8-153">Ha ez az oktatóanyag nem parancssori program alkalmaz, objektumból hello token visszaadott lehetett kibontani a hello e-mail.</span><span class="sxs-lookup"><span data-stu-id="24da8-153">If you weren't using a command-line program for this tutorial, you could extract hello email from hello token object that is returned.</span></span> <span data-ttu-id="24da8-154">Ezt követően előfordulhat, hogy kérje meg hello felhasználói tooenter további információt.</span><span class="sxs-lookup"><span data-stu-id="24da8-154">Then, you might ask hello user tooenter additional information.</span></span> <span data-ttu-id="24da8-155">Mivel ez egy tesztkiszolgálón, hozzáadhat hello felhasználó közvetlenül a toohello memóriában lévő adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="24da8-155">Because this is a test server, you add hello user directly toohello in-memory database.</span></span>
  > 
  > 

4.  <span data-ttu-id="24da8-156">Adja hozzá, hogy használja-e a felhasználók, akik van bejelentkezve, tookeep követése hello metódusokat Passport által előírt.</span><span class="sxs-lookup"><span data-stu-id="24da8-156">Add hello methods that you use tookeep track of users who are signed in, as required by Passport.</span></span> <span data-ttu-id="24da8-157">Ez magában foglalja a szerializálása és deszerializálása hello felhasználói adatokat:</span><span class="sxs-lookup"><span data-stu-id="24da8-157">This includes serializing and deserializing hello user's information:</span></span>

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

5.  <span data-ttu-id="24da8-158">Adja hozzá, amelyek hello Express motor betölti hello kódot.</span><span class="sxs-lookup"><span data-stu-id="24da8-158">Add hello code that loads hello Express engine.</span></span> <span data-ttu-id="24da8-159">Hello alapértelmezett /views használ, és Express /routes minta biztosítja:</span><span class="sxs-lookup"><span data-stu-id="24da8-159">You use hello default /views and /routes pattern that Express provides:</span></span>

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

6.  <span data-ttu-id="24da8-160">Adja hozzá a hello POST irányítja a hello tényleges bejelentkezési kérelmek toohello ki, hogy az aktuális `passport-azure-ad` motor:</span><span class="sxs-lookup"><span data-stu-id="24da8-160">Add hello POST routes that hand off hello actual sign-in requests toohello `passport-azure-ad` engine:</span></span>

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

## <a name="4-use-passport-tooissue-sign-in-and-sign-out-requests-tooazure-ad"></a><span data-ttu-id="24da8-161">4: használatát a Passport tooissue be- és kijelentkezési kérések tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="24da8-161">4: Use Passport tooissue sign-in and sign-out requests tooAzure AD</span></span>
<span data-ttu-id="24da8-162">Az alkalmazás most már be van állítva hello v2.0-végponttal toocommunicate hello OpenID Connect hitelesítési protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="24da8-162">Your app is now set up toocommunicate with hello v2.0 endpoint by using hello OpenID Connect authentication protocol.</span></span> <span data-ttu-id="24da8-163">Hello `passport-azure-ad` stratégia gondoskodik hitelesítési üzenetek létrehozásával, ellenőrzése az Azure ad-jogkivonatok és karbantartását hello felhasználói munkamenet összes hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="24da8-163">hello `passport-azure-ad` strategy takes care of all hello details of crafting authentication messages, validating tokens from Azure AD, and maintaining hello user session.</span></span> <span data-ttu-id="24da8-164">Minden toodo hagyott toogive van a felhasználók a módon toosign, és jelentkezzen ki, és toogather hello van bejelentkezett felhasználó további információt.</span><span class="sxs-lookup"><span data-stu-id="24da8-164">All that is left toodo is toogive your users a way toosign in and sign out, and toogather more information about hello user who is signed in.</span></span>

1.  <span data-ttu-id="24da8-165">Adja hozzá a hello **alapértelmezett**, **bejelentkezési**, **fiók**, és **kijelentkezési** módszerek tooyour App.js fájlban:</span><span class="sxs-lookup"><span data-stu-id="24da8-165">Add hello **default**, **login**, **account**, and **logout** methods tooyour App.js file:</span></span>

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

  <span data-ttu-id="24da8-166">Az alábbiakban hello részletek:</span><span class="sxs-lookup"><span data-stu-id="24da8-166">Here are hello details:</span></span>
    
    * <span data-ttu-id="24da8-167">Hello `/` útvonal átirányítja a felhasználókat toohello index.ejs nézet.</span><span class="sxs-lookup"><span data-stu-id="24da8-167">hello `/` route redirects toohello index.ejs view.</span></span> <span data-ttu-id="24da8-168">Hello felhasználói haladnak hello kérelem (ha van ilyen).</span><span class="sxs-lookup"><span data-stu-id="24da8-168">It passes hello user in hello request (if it exists).</span></span>
    * <span data-ttu-id="24da8-169">Hello `/account` útvonal először *biztosítja, hogy hitelesítése* (megvalósítása, amely a következő kód hello).</span><span class="sxs-lookup"><span data-stu-id="24da8-169">hello `/account` route first *ensures that you are authenticated* (you implement that in hello following code).</span></span> <span data-ttu-id="24da8-170">Ezt követően átadja hello felhasználói hello kérelemben.</span><span class="sxs-lookup"><span data-stu-id="24da8-170">Then, it passes hello user in hello request.</span></span> <span data-ttu-id="24da8-171">Ez a helyzet hello felhasználó további információt kaphat.</span><span class="sxs-lookup"><span data-stu-id="24da8-171">This is so you can get more information about hello user.</span></span>
    * <span data-ttu-id="24da8-172">Hello `/login` útvonal hívja a `azuread-openidconnect` a hitelesítő `passport-azuread`.</span><span class="sxs-lookup"><span data-stu-id="24da8-172">hello `/login` route calls your `azuread-openidconnect` authenticator from `passport-azuread`.</span></span> <span data-ttu-id="24da8-173">Ha, amely nem jár sikerrel, az átirányítja hello felhasználói vissza túl`/login`.</span><span class="sxs-lookup"><span data-stu-id="24da8-173">If that doesn't succeed, it redirects hello user back too`/login`.</span></span>
    * <span data-ttu-id="24da8-174">Hello `/logout` útvonal hívja hello logout.ejs megtekintése (és útvonal).</span><span class="sxs-lookup"><span data-stu-id="24da8-174">hello `/logout` route calls hello logout.ejs view (and route).</span></span> <span data-ttu-id="24da8-175">Ez törli a cookie-kat, és majd vissza hello felhasználói hátsó tooindex.ejs.</span><span class="sxs-lookup"><span data-stu-id="24da8-175">This clears cookies, and then returns hello user back tooindex.ejs.</span></span>

2.  <span data-ttu-id="24da8-176">Adja hozzá a hello **EnsureAuthenticated** , amelyet korábban a használt módszer `/account`:</span><span class="sxs-lookup"><span data-stu-id="24da8-176">Add hello **EnsureAuthenticated** method that you used earlier in `/account`:</span></span>

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

3.  <span data-ttu-id="24da8-177">Az App.js hello kiszolgáló létrehozása:</span><span class="sxs-lookup"><span data-stu-id="24da8-177">In App.js, create hello server:</span></span>

  ```JavaScript

  app.listen(3000);

  ```


## <a name="5-create-hello-views-and-routes-in-express-that-you-show-your-user-on-hello-website"></a><span data-ttu-id="24da8-178">5: hello nézetek és útvonalak létrehozása az Express a felhasználó hello webhely megjelenítése</span><span class="sxs-lookup"><span data-stu-id="24da8-178">5: Create hello views and routes in Express that you show your user on hello website</span></span>
<span data-ttu-id="24da8-179">Adja hozzá a hello útvonalakat és nézeteket, amelyek információt toohello felhasználói megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="24da8-179">Add hello routes and views that show information toohello user.</span></span> <span data-ttu-id="24da8-180">hello útvonalakat és nézeteket is kezelni hello `/logout` és `/login` létrehozott útvonalak.</span><span class="sxs-lookup"><span data-stu-id="24da8-180">hello routes and views also handle hello `/logout` and `/login` routes that you created.</span></span>

1. <span data-ttu-id="24da8-181">Hello gyökérkönyvtárában, hozzon létre hello `/routes/index.js` útvonal.</span><span class="sxs-lookup"><span data-stu-id="24da8-181">In hello root directory, create hello `/routes/index.js` route.</span></span>

  ```JavaScript

  /*
  * GET home page.
  */

  exports.index = function(req, res){
    res.render('index', { title: 'Express' });
  };
  ```

2.  <span data-ttu-id="24da8-182">Hello gyökérkönyvtárában, hozzon létre hello `/routes/user.js` útvonal.</span><span class="sxs-lookup"><span data-stu-id="24da8-182">In hello root directory, create hello `/routes/user.js` route.</span></span>

  ```JavaScript

  /*
  * GET users listing.
  */

  exports.list = function(req, res){
    res.send("respond with a resource");
  };
  ```

  <span data-ttu-id="24da8-183">`/routes/index.js`és `/routes/user.js` vannak egyszerű útvonalak adják át a hello kérelem tooyour nézetek, beleértve a hello felhasználói, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="24da8-183">`/routes/index.js` and `/routes/user.js` are simple routes that pass along hello request tooyour views, including hello user, if present.</span></span>

3.  <span data-ttu-id="24da8-184">Hello gyökérkönyvtárában, hozzon létre hello `/views/index.ejs` nézet.</span><span class="sxs-lookup"><span data-stu-id="24da8-184">In hello root directory, create hello `/views/index.ejs` view.</span></span> <span data-ttu-id="24da8-185">Ezen a lapon meghívja a **bejelentkezési** és **kijelentkezési** módszerek.</span><span class="sxs-lookup"><span data-stu-id="24da8-185">This page calls your **login** and **logout** methods.</span></span> <span data-ttu-id="24da8-186">Hello is a `/views/index.ejs` toocapture fiók adatainak megtekintése.</span><span class="sxs-lookup"><span data-stu-id="24da8-186">You also use hello `/views/index.ejs` view toocapture account information.</span></span> <span data-ttu-id="24da8-187">Használhatja a feltételes hello `if (!user)` hello kérelem beadott keresztül hello felhasználóként.</span><span class="sxs-lookup"><span data-stu-id="24da8-187">You can use hello conditional `if (!user)` as hello user being passed through in hello request.</span></span> <span data-ttu-id="24da8-188">Fontos, hogy rendelkezik-e a bejelentkezett felhasználó bizonyító adatok.</span><span class="sxs-lookup"><span data-stu-id="24da8-188">It is evidence that you have a user signed in.</span></span>

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

4.  <span data-ttu-id="24da8-189">Hello gyökérkönyvtárában, hozzon létre hello `/views/account.ejs` nézet.</span><span class="sxs-lookup"><span data-stu-id="24da8-189">In hello root directory, create hello `/views/account.ejs` view.</span></span> <span data-ttu-id="24da8-190">Hello `/views/account.ejs` nézet lehetővé teszi tooview további információt, amely `passport-azuread` hello felhasználói kérelem helyezi.</span><span class="sxs-lookup"><span data-stu-id="24da8-190">hello `/views/account.ejs` view allows you tooview additional information that `passport-azuread` puts in hello user request.</span></span>

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

5.  <span data-ttu-id="24da8-191">Adjon hozzá egy elrendezés.</span><span class="sxs-lookup"><span data-stu-id="24da8-191">Add a layout.</span></span> <span data-ttu-id="24da8-192">Hello gyökérkönyvtárában, hozzon létre hello `/views/layout.ejs` nézet.</span><span class="sxs-lookup"><span data-stu-id="24da8-192">In hello root directory, create hello `/views/layout.ejs` view.</span></span>

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

6.  <span data-ttu-id="24da8-193">toobuild futtatta az alkalmazását, és futtassa `node app.js`.</span><span class="sxs-lookup"><span data-stu-id="24da8-193">toobuild and run your app, run `node app.js`.</span></span> <span data-ttu-id="24da8-194">Ezután térjen túl`http://localhost:3000`.</span><span class="sxs-lookup"><span data-stu-id="24da8-194">Then, go too`http://localhost:3000`.</span></span>

7.  <span data-ttu-id="24da8-195">Jelentkezzen be személyes Microsoft-fiókkal vagy egy munkahelyi vagy iskolai fiókkal.</span><span class="sxs-lookup"><span data-stu-id="24da8-195">Sign in with either a personal Microsoft account or a work or school account.</span></span> <span data-ttu-id="24da8-196">Vegye figyelembe, hogy hello felhasználói identitás tükröz hello /account listában.</span><span class="sxs-lookup"><span data-stu-id="24da8-196">Note that hello user's identity is reflected in hello /account list.</span></span> 

<span data-ttu-id="24da8-197">Most már rendelkezik egy webalkalmazást, amelyet az iparági szabványos protokollok használatával.</span><span class="sxs-lookup"><span data-stu-id="24da8-197">You now have a web app that is secured by using industry standard protocols.</span></span> <span data-ttu-id="24da8-198">Az alkalmazás a felhasználók a személyes és munkahelyi vagy iskolai fiókok használatával képes hitelesíteni.</span><span class="sxs-lookup"><span data-stu-id="24da8-198">You can authenticate users in your app by using their personal and work or school accounts.</span></span>

## <a name="next-steps"></a><span data-ttu-id="24da8-199">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="24da8-199">Next steps</span></span>
<span data-ttu-id="24da8-200">Befejeződött hello mintát (a konfigurációs értékek nélkül) referenciaként biztosított [egy .zip fájl](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="24da8-200">For reference, hello completed sample (without your configuration values) is provided as [a .zip file](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip).</span></span> <span data-ttu-id="24da8-201">Akkor is klónozhatja azt a Githubról:</span><span class="sxs-lookup"><span data-stu-id="24da8-201">You also can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

<span data-ttu-id="24da8-202">Továbbléphet a következő témakörök speciális toomore.</span><span class="sxs-lookup"><span data-stu-id="24da8-202">Next, you can move on toomore advanced topics.</span></span> <span data-ttu-id="24da8-203">Érdemes lehet tootry:</span><span class="sxs-lookup"><span data-stu-id="24da8-203">You might want tootry:</span></span>

[<span data-ttu-id="24da8-204">Node.js webes API biztonságossá hello v2.0-végponttól használatával</span><span class="sxs-lookup"><span data-stu-id="24da8-204">Secure a Node.js web API by using hello v2.0 endpoint</span></span>](active-directory-v2-devquickstarts-node-api.md)

<span data-ttu-id="24da8-205">Az alábbiakban néhány további források:</span><span class="sxs-lookup"><span data-stu-id="24da8-205">Here are some additional resources:</span></span>

* [<span data-ttu-id="24da8-206">Az Azure AD v2.0 fejlesztői útmutató</span><span class="sxs-lookup"><span data-stu-id="24da8-206">Azure AD v2.0 developer guide</span></span>](active-directory-appmodel-v2-overview.md)
* [<span data-ttu-id="24da8-207">A verem túlcsordulás "azure-active-directory" címke</span><span class="sxs-lookup"><span data-stu-id="24da8-207">Stack Overflow "azure-active-directory" tag</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

### <a name="get-security-updates-for-our-products"></a><span data-ttu-id="24da8-208">Biztonsági frissítések termékeinkhez</span><span class="sxs-lookup"><span data-stu-id="24da8-208">Get security updates for our products</span></span>
<span data-ttu-id="24da8-209">Javasoljuk toosign biztonsági incidensek értesítendő toobe fel.</span><span class="sxs-lookup"><span data-stu-id="24da8-209">We encourage you toosign up toobe notified when security incidents occur.</span></span> <span data-ttu-id="24da8-210">A hello [Microsoft műszaki biztonsági értesítéseket](https://technet.microsoft.com/security/dd252948) előfizetés tooSecurity tanácsadók riasztások lapján.</span><span class="sxs-lookup"><span data-stu-id="24da8-210">On hello [Microsoft Technical Security Notifications](https://technet.microsoft.com/security/dd252948) page, subscribe tooSecurity Advisories Alerts.</span></span>

