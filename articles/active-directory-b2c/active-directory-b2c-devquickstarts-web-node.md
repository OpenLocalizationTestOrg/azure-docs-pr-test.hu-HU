---
title: "aaaAdd bejelentkezési tooa Node.js webalkalmazásokba az Azure B2C |} Microsoft Docs"
description: "Hogyan toobuild egy Node.js webes alkalmazást, amely a B2C-bérlő segítségével képes bejelentkeztetni a felhasználókat."
services: active-directory-b2c
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: db97f84a-1f24-447b-b6d2-0265c6896b27
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: hero-article
ms.date: 03/10/2017
ms.author: xerners
ms.openlocfilehash: b4c334b1f7a0669df2d0864140603dc55bbb5408
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-add-sign-in-tooa-nodejs-web-app"></a>Az Azure AD B2C: Bejelentkezési tooa Node.js webalkalmazás hozzáadása

A **Passport** a Node.js-hez készült közbenső hitelesítési szoftver. A rendkívül rugalmasan működő, moduláris Passport gyakorlatilag bármely Express- vagy Restify-alapú webalkalmazásba diszkréten telepíthető. A program számos különböző lehetőséget kínál a felhasználók hitelesítésére: felhasználónév/jelszó, Facebook- vagy Twitter-fiók és így tovább.

Kidolgoztunk egy stratégiát, amellyel a szoftver az Azure Active Directory (Azure AD) esetében is felhasználható. Először telepítenie kell a modult, majd adja hozzá az Azure AD hello `passport-azure-ad` beépülő modult.

toodo, kell:

1. Alkalmazás regisztrálása az Azure AD használatával.
2. Állítsa be az alkalmazás toouse hello `passport-azure-ad` beépülő modult.
3. A Passport tooissue bejelentkezési és kijelentkezési kérések tooAzure AD használatára.
4. Felhasználói adatok kinyomtatása.

Ez az oktatóanyag kód hello [kezelése a Githubon történik](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS). toofollow mellett, akkor [hello alkalmazás vázát .zip fájl letöltése](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip). Hello vázat klónozhatja is:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS.git```

Ez az oktatóanyag végén hello befejeződött hello alkalmazás valósul meg.

## <a name="get-an-azure-ad-b2c-directory"></a>Az Azure AD B2C-címtár beszerzése

Az Azure AD B2C használatához létre kell hoznia egy címtárat vagy bérlőt.  A címtárban tárolhatja az összes felhasználót, alkalmazást, csoportot és sok minden mást. Ha még nem tette meg, [hozzon létre most egy B2C-címtárat](active-directory-b2c-get-started.md), mielőtt továbblépne ebben az útmutatóban.

## <a name="create-an-application"></a>Alkalmazás létrehozása

A következő lépésben toocreate egy alkalmazást a B2C-címtárban. Ez biztosítja, hogy kell-e az alkalmazással biztonságosan toocommunicate az Azure AD-információkat. Mindkét hello ügyfélalkalmazás és webes API-t fog megjelenni, egyetlen **Alkalmazásazonosító**, mert alkalmazássá logikai. hajtsa végre az alkalmazást, és egy toocreate [ezeket az utasításokat](active-directory-b2c-app-registration.md). Ügyeljen arra, hogy:

- Tartalmaznak egy **webalkalmazás**/**webes API-t** hello alkalmazásban.
- A **Reply URL** (Válasz URL-cím) legyen a következő: `http://localhost:3000/auth/openid/return`. A fenti hello alapértelmezett URL-.
- Hozzon létre egy **alkalmazástitkot** az alkalmazáshoz, majd másolja. Erre később még szüksége lesz. Vegye figyelembe, hogy ezt az értéket kell toobe [escape-karaktersorozatot XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) használatba vétel előtt.
- Másolás hello **Alkalmazásazonosító** , amely hozzárendelt tooyour alkalmazást. Később erre is szüksége lesz.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Szabályzatok létrehozása

Az Azure AD B2C-ben a felhasználói élményeket [szabályzatok](active-directory-b2c-reference-policies.md) határozzák meg. Az alkalmazás három különböző, identitással kapcsolatos műveletet tartalmaz: regisztráció, bejelentkezés és bejelentkezés Facebook-fiókkal. Szüksége toocreate ezt a házirendet az egyes típusok leírtak szerint a [házirendek áttekintésével foglalkozó cikkben](active-directory-b2c-reference-policies.md#create-a-sign-up-policy). A három szabályzat létrehozásakor ügyeljen arra, hogy:

- Válassza ki a hello **megjelenített név** és az egyéb regisztrációs attribútumokat a regisztrációs szabályzatban.
- Válassza ki a hello **megjelenített név** és **Objektumazonosító** minden házirend alkalmazási jogcímet. Kiválaszthat egyéb jogcímeket is.
- Másolás hello **neve** egyes házirendek létrehozása után. Hello előtag nem rendelkezhet `b2c_1_`.  A házirendek nevére később még szüksége lesz.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

A három szabályzat létrehozását követően készen áll a toobuild Ön az alkalmazást.

Vegye figyelembe, hogy a cikk nem tér ki hogyan toouse hello házirendek újonnan létrehozott. toolearn arról, hogyan házirendek az Azure AD B2C működnek, hello kezdődnie [.NET webalkalmazások használatába bevezető oktatóanyagot](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="add-prerequisites-tooyour-directory"></a>Előfeltételek tooyour könyvtár hozzáadása

Hello parancssorból módosítsa könyvtárak tooyour gyökérmappára, ha már nem létezik. Futtassa a következő parancsok hello:

- `npm install express`
- `npm install ejs`
- `npm install ejs-locals`
- `npm install restify`
- `npm install mongoose`
- `npm install bunyan`
- `npm install assert-plus`
- `npm install passport`
- `npm install webfinger`
- `npm install body-parser`
- `npm install express-session`
- `npm install cookie-parser`

Ezenkívül használtuk `passport-azure-ad` a a gyors üzembe helyezés hello hello vázban szereplő előzetes verzióban.

- `npm install passport-azure-ad`

Ez a hello szalagtárak telepíti, amely `passport-azure-ad` függ.

## <a name="set-up-your-app-toouse-hello-passport-nodejs-strategy"></a>Állítsa be az alkalmazás toouse hello Passport-Node.js stratégia
Konfigurálja a hello Express köztes toouse hello OpenID Connect hitelesítési protokoll. Passport kell használt tooissue be- és kijelentkezési kéréseket, kezelni a felhasználói munkameneteket, és többek között a felhasználók adatainak beolvasása.

Nyissa meg hello `config.js` hello projekt gyökerében hello fájlt, és adja meg az alkalmazás konfigurációs értékeit hello `exports.creds` szakasz.
- `clientID`: hello **Alkalmazásazonosító** tooyour app hello regisztrációs portálon rendelt.
- `returnURL`: hello **átirányítási URI-** hello portálon megadott.
- `tenantName`: hello bérlő nevét az alkalmazás, például **contoso.onmicrosoft.com**.

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Nyissa meg hello `app.js` hello hello projekt gyökerében található fájl. Adja hozzá a következő hívást tooinvoke hello hello `OIDCStrategy` stratégia `passport-azure-ad`.


```JavaScript
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

// Add some logging
var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
});
```

Használja a korábban említett toohandle bejelentkezési kérelmek hello stratégiát.

```JavaScript
// Use hello OIDCStrategy in Passport (Section 2).
//
//   Strategies in Passport require a "validate" function that accepts
//   credentials (in this case, an OpenID identifier), and invokes a callback
//   by using a user object.
passport.use(new OIDCStrategy({
    callbackURL: config.creds.returnURL,
    realm: config.creds.realm,
    clientID: config.creds.clientID,
    oidcIssuer: config.creds.issuer,
    identityMetadata: config.creds.identityMetadata,
    skipUserProfile: config.creds.skipUserProfile,
    responseType: config.creds.responseType,
    responseMode: config.creds.responseMode,
    tenantName: config.creds.tenantName
  },
  function(iss, sub, profile, accessToken, refreshToken, done) {
    log.info('Example: Email address we received was: ', profile.email);
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
A Passport az összes stratégia (ideértve a Twittert és a Facebookot is) esetében hasonló mintát alkalmaz. Stratégiafejlesztő toothis mintát. Ha hello stratégia tekinti meg, láthatja, át kell adni azt egy `function()` , amely rendelkezik egy jogkivonat és egy `done` hello paraméterekként. hello stratégia ismét elérhető lesz tooyou után fordult elő az összes teendőit. Hello felhasználói tárolja, és menteni a hello jogkivonat, így nem kell tooask azt újra.

> [!IMPORTANT]
hello előző kód szükséges minden olyan felhasználó, akinek hello kiszolgáló hitelesíti. Ezt nevezzük automatikus regisztrációnak. Éles kiszolgálók használata, ha nem szeretné, hogy a felhasználók toolet kivéve, ha túl vannak, amelyek állított be regisztrációs folyamatot. Ez gyakori eljárás a fogyasztói alkalmazásoknál. Ezek lehetővé teszik tooregister Facebook-fiókkal, de majd kéri az toofill meg további információkat. Ha az alkalmazás nem csupán mintaként szolgálna, azt sikerült kinyerése egy e-mail címet hello jogkivonat-objektumból ad vissza, és majd kérje meg a hello felhasználói toofill meg további információkat. Mivel ez egy tesztkiszolgálón, egyszerűen hozzáadjuk a felhasználók toohello memóriában lévő adatbázishoz.

Adja hozzá, amelyek lehetővé teszik a felhasználók, akik bejelentkezett, tookeep követése hello metódusokat Passport által előírt. Ehhez tartozik a felhasználói adatok szerializálása és deszerializálása is:

```JavaScript

// Passport session setup. (Section 2)

//   toosupport persistent sign-in sessions, Passport needs toobe able to
//   serialize users into and deserialize users out of sessions. Typically,
//   this is as simple as storing hello user ID when Passport serializes a user
//   and finding hello user by ID when Passport deserializes that user.
passport.serializeUser(function(user, done) {
  done(null, user.email);
});

passport.deserializeUser(function(id, done) {
  findByEmail(id, function (err, user) {
    done(err, user);
  });
});

// Array toohold users who have signed in
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

Adja hozzá a hello kód tooload hello Express motor. A következő hello, megtekintheti, hogy használjuk-e hello alapértelmezett `/views` és `/routes` mintát, amely az Express által biztosított.

```JavaScript

// configure Express (Section 2)

var app = express();


app.configure(function() {
  app.set('views', __dirname + '/views');
  app.set('view engine', 'ejs');
  app.use(express.logger());
  app.use(express.methodOverride());
  app.use(cookieParser());
  app.use(expressSession({ secret: 'keyboard cat', resave: true, saveUninitialized: false }));
  app.use(bodyParser.urlencoded({ extended : true }));
  // Initialize Passport!  Also use passport.session() middleware toosupport
  // persistent sign-in sessions (recommended).
  app.use(passport.initialize());
  app.use(passport.session());
  app.use(app.router);
  app.use(express.static(__dirname + '/../../public'));
});

```

Adja hozzá a hello `POST` útvonalakat, amelyek kiosztják a tényleges bejelentkezési kérelmek toohello hello `passport-azure-ad` motor:

```JavaScript

// Our Auth routes (Section 3)

// GET /auth/openid
//   Use passport.authenticate() as route middleware tooauthenticate the
//   request. hello first step in OpenID authentication involves redirecting
//   hello user tooan OpenID provider. After hello user is authenticated,
//   hello OpenID provider redirects hello user back toothis application at
//   /auth/openid/return

app.get('/auth/openid',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Authentication was called in hello Sample');
    res.redirect('/');
  });

// GET /auth/openid/return
//   Use passport.authenticate() as route middleware tooauthenticate the
//   request. If authentication fails, hello user will be redirected back toothe
//   sign-in page. Otherwise, hello primary route function will be called.
//   In this example, it redirects hello user toohello home page.
app.get('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });

// POST /auth/openid/return
//   Use passport.authenticate() as route middleware tooauthenticate the
//   request. If authentication fails, hello user will be redirected back toothe
//   sign-in page. Otherwise, hello primary route function will be called.
//   In this example, it will redirect hello user toohello home page.

app.post('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });
```

## <a name="use-passport-tooissue-sign-in-and-sign-out-requests-tooazure-ad"></a>A Passport tooissue bejelentkezési és kijelentkezési kérések tooAzure AD használatára

Az alkalmazás már megfelelően konfigurált toocommunicate hello v2.0-végponttal hello OpenID Connect hitelesítési protokoll használatával. `passport-azure-ad`rendelkezik hitelesítési üzenetek létrehozásával, ellenőrzése az Azure ad-jogkivonatok és karbantartásáról a felhasználói munkamenet hello részleteit végrehajtott kezeli. Hogy továbbra is van toogive a felhasználók a módon toosign, és jelentkezzen ki, és bejelentkezett felhasználók toogather további információkat.

Először adja hozzá a hello alapértelmezett, bejelentkezési, fiókkal, és kijelentkezési metódusokat tooyour `app.js` fájlt:

```JavaScript

//Routes (Section 4)

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

tooreview ezen módszerek részletes információkat:
- Hello `/` útvonal átirányítja a felhasználókat toohello `index.ejs` nézet úgy, hogy hello felhasználói hello kérelem (ha van ilyen).
- Hello `/account` útvonal először ellenőrzi, hogy hitelesítése (hello végrehajtása a következő alatt). Ezután átadja hello felhasználói hello kérelem, hogy hello felhasználó további információt kaphat.
- Hello `/login` útvonal hívások hello `azuread-openidconnect` a hitelesítő `passport-azure-ad`. Ha nem jár sikerrel, hello útvonal túl átirányítja a felhasználókat hello felhasználói vissza`/login`.
- `/logout` egyszerűen meghívja a `logout.ejs`-t (és annak útvonalát). Ez törli a cookie-kat, és ezután a beolvasása hello vissza felhasználói túl`index.ejs`.


Hello utolsó részének `app.js`, vegye fel a hello `EnsureAuthenticated` hello használt módszer `/account` útvonal.

```JavaScript

// Simple route middleware tooensure that hello user is authenticated. (Section 4)

//   Use this route middleware on any resource that needs toobe protected. If
//   hello request is authenticated (typically via a persistent sign-in session),
//   then hello request will proceed. Otherwise, hello user will be redirected toothe
//   sign-in page.
function ensureAuthenticated(req, res, next) {
  if (req.isAuthenticated()) { return next(); }
  res.redirect('/login')
}

```

Végezetül hozza létre magát hello kiszolgálót a `app.js`.

```JavaScript

app.listen(3000);

```


## <a name="create-hello-views-and-routes-in-express-toocall-your-policies"></a>Hello nézeteket hozhat létre, és az Express toocall továbbítja a házirendek

Az `app.js` ezzel elkészült. Egyszerűen tooadd hello útvonalakat és nézeteket, amelyek lehetővé teszik toocall hello bejelentkezési és regisztrációs szabályzatok. Ezek kezelik hello `/logout` és `/login` létrehozott útvonalak.

Hozzon létre hello `/routes/index.js` útvonal hello gyökérkönyvtárban.

```JavaScript

/*
 * GET home page.
 */

exports.index = function(req, res){
  res.render('index', { title: 'Express' });
};
```

Hozzon létre hello `/routes/user.js` útvonal hello gyökérkönyvtárban.

```JavaScript

/*
 * GET users listing.
 */

exports.list = function(req, res){
  res.send("respond with a resource");
};
```

Ezek az egyszerű útvonalak adják át a kérelmek tooyour nézetek. Ha ilyen hello felhasználói tartalmaznak.

Hozzon létre hello `/views/index.ejs` nézet hello gyökérkönyvtárban. Ez egy egyszerű lap, amely a bejelentkezési és kijelentkezési szabályzatok meghívását végzi. Használhatja az toograb fiók adatait. Vegye figyelembe, hogy használhatja-e feltételes hello `if (!user)` , hello felhasználói átadott keresztül hello kérelem tooprovide bizonyíték hello felhasználó bejelentkezett.

```JavaScript
<% if (!user) { %>
    <h2>Welcome! Please sign in.</h2>
    <a href="/login/?p=your facebook policy">Sign in with Facebook</a>
    <a href="/login/?p=your email sign-in policy">Sign in with email</a>
    <a href="/login/?p=your email sign-up policy">Sign up with email</a>
<% } else { %>
    <h2>Hello, <%= user.displayName %>.</h2>
    <a href="/account">Account info</a></br>
    <a href="/logout">Log out</a>
<% } %>
```

Hozzon létre hello `/views/account.ejs` hello gyökérkönyvtárban megtekintheti, hogy további információkat láthat, amelyek `passport-azure-ad` hello felhasználói kérelem be.

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
<p>Full Claims</p>
<%- JSON.stringify(user) %>
<p></p>
<a href="/logout">Log Out</a>
<% } %>
```

Most már lefordíthatja, és futtathatja az alkalmazást.

Futtatás `node app.js` , és keresse meg a túl`http://localhost:3000`


Regisztráció, vagy jelentkezzen be toohello app e-mail vagy Facebook használatával. Jelentkezzen ki, majd jelentkezzen be ismét egy másik felhasználóval.

##<a name="next-steps"></a>Következő lépések

Referenciaként hello befejeződött (a konfigurációs értékek nélkül) minta [egy .zip-fájlban is](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/complete.zip). Ezenfelül a GitHubból is klónozhatja:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-nodejs.git```

Most már továbbléphet a témakörök speciális toomore. Próbálkozzon meg a következőkkel:

[A webes API biztonságossá tétele a node.js hello B2C-modell használatával](active-directory-b2c-devquickstarts-api-node.md)

<!--

For additional resources, check out:
You can now move on toomore advanced B2C topics. You might try:

[Call a Node.js web API from a Node.js web app]()

[Customizing hello your B2C App's UX >>]()

-->
