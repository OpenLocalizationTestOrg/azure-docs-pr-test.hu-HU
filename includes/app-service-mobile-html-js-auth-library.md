### <span data-ttu-id="cc06b-101"><a name="server-auth"></a>Útmutató: Hitelesítés szolgáltatóval (Server Flow)</span><span class="sxs-lookup"><span data-stu-id="cc06b-101"><a name="server-auth"></a>How to: Authenticate with a provider (Server Flow)</span></span>
<span data-ttu-id="cc06b-102">toohave Mobile Apps kezelése hello hitelesítési folyamat az alkalmazásban, regisztrálnia kell az alkalmazást az identitásszolgáltató.</span><span class="sxs-lookup"><span data-stu-id="cc06b-102">toohave Mobile Apps manage hello authentication process in your app, you must register your app with your identity provider.</span></span> <span data-ttu-id="cc06b-103">Az Azure App Service, végre kell tooconfigure hello Alkalmazásazonosítót és a szolgáltató által biztosított titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="cc06b-103">Then in your Azure App Service, you need tooconfigure hello application ID and secret provided by your provider.</span></span>
<span data-ttu-id="cc06b-104">További információkért lásd: hello oktatóanyag [Add authentication tooyour alkalmazás](../articles/app-service-mobile/app-service-mobile-cordova-get-started-users.md).</span><span class="sxs-lookup"><span data-stu-id="cc06b-104">For more information, see hello tutorial [Add authentication tooyour app](../articles/app-service-mobile/app-service-mobile-cordova-get-started-users.md).</span></span>

<span data-ttu-id="cc06b-105">Miután regisztrálta az identitásszolgáltató, hívja hello `.login()` a szolgáltató hello nevű metódus.</span><span class="sxs-lookup"><span data-stu-id="cc06b-105">Once you have registered your identity provider, call hello `.login()` method with hello name of your provider.</span></span> <span data-ttu-id="cc06b-106">A Facebook toologin például a következő kód hello használja:</span><span class="sxs-lookup"><span data-stu-id="cc06b-106">For example, toologin with Facebook use hello following code:</span></span>

```
client.login("facebook").done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```

<span data-ttu-id="cc06b-107">hello érvényes hello szolgáltató értékei a "aad", "facebook", "google", "microsoftaccount" és "twitter".</span><span class="sxs-lookup"><span data-stu-id="cc06b-107">hello valid values for hello provider are 'aad', 'facebook', 'google', 'microsoftaccount', and 'twitter'.</span></span>

> [!NOTE]
> <span data-ttu-id="cc06b-108">A hitelesítés Google-fiókkal jelenleg nem használható a Server Flow-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="cc06b-108">Google Authentication does not currently work via Server Flow.</span></span>  <span data-ttu-id="cc06b-109">a Google tooauthenticate, kell használnia egy [ügyféltanúsítvány-folyamat metódus](#client-auth).</span><span class="sxs-lookup"><span data-stu-id="cc06b-109">tooauthenticate with Google, you must use a [client-flow method](#client-auth).</span></span>

<span data-ttu-id="cc06b-110">Ebben az esetben az Azure App Service hello OAuth 2.0 hitelesítési folyamat kezeli.</span><span class="sxs-lookup"><span data-stu-id="cc06b-110">In this case, Azure App Service manages hello OAuth 2.0 authentication flow.</span></span>  <span data-ttu-id="cc06b-111">A kijelölt szolgáltató hello hello bejelentkezési oldalát jeleníti meg, és létrehoz egy App Service hitelesítés jogkivonatot hello identitásszolgáltatóval sikeres bejelentkezés után.</span><span class="sxs-lookup"><span data-stu-id="cc06b-111">It displays hello login page of hello selected provider and generates an App Service authentication token after successful login with hello identity provider.</span></span> <span data-ttu-id="cc06b-112">hello bejelentkezési függvény, amikor végzett, adja vissza egy JSON-objektum, amely azt mutatja, mind a hello felhasználói Azonosítót, és az App Service hitelesítésére szolgáló jogkivonat hello userId és authenticationToken mezőket, illetve.</span><span class="sxs-lookup"><span data-stu-id="cc06b-112">hello login function, when complete, returns a JSON object that exposes both hello user ID and App Service authentication token in hello userId and authenticationToken fields, respectively.</span></span> <span data-ttu-id="cc06b-113">Ez a token gyorsítótárazható, és újra felhasználható, amíg le nem jár.</span><span class="sxs-lookup"><span data-stu-id="cc06b-113">This token can be cached and reused until it expires.</span></span>

###<span data-ttu-id="cc06b-114"><a name="client-auth"></a>Útmutató: Hitelesítés szolgáltatóval (Client Flow)</span><span class="sxs-lookup"><span data-stu-id="cc06b-114"><a name="client-auth"></a>How to: Authenticate with a provider (Client Flow)</span></span>

<span data-ttu-id="cc06b-115">Az alkalmazás hello identitásszolgáltató függetlenül is kapcsolatba, és adja meg a hello visszaadott token tooyour App Service-hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="cc06b-115">Your app can also independently contact hello identity provider and then provide hello returned token tooyour App Service for authentication.</span></span> <span data-ttu-id="cc06b-116">Az ügyfél folyamata lehetővé teszi tooprovide egy egyszeri bejelentkezési élmény a felhasználók vagy a hello identitásszolgáltató tooretrieve további felhasználó adatait.</span><span class="sxs-lookup"><span data-stu-id="cc06b-116">This client flow enables you tooprovide a single sign-in experience for users or tooretrieve additional user data from hello identity provider.</span></span>

#### <a name="social-authentication-basic-example"></a><span data-ttu-id="cc06b-117">Közösségi portálos hitelesítés – alapszintű példa</span><span class="sxs-lookup"><span data-stu-id="cc06b-117">Social Authentication basic example</span></span>

<span data-ttu-id="cc06b-118">Ez a példa a Facebook ügyfél-SDK-t használja a hitelesítéshez:</span><span class="sxs-lookup"><span data-stu-id="cc06b-118">This example uses Facebook client SDK for authentication:</span></span>

```
client.login(
     "facebook",
     {"access_token": token})
.done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});

```
<span data-ttu-id="cc06b-119">Ez a példa feltételezi, hogy hello jogkivonat megadott hello megfelelő szolgáltató SDK hello token változó tárolja.</span><span class="sxs-lookup"><span data-stu-id="cc06b-119">This example assumes that hello token provided by hello respective provider SDK is stored in hello token variable.</span></span>

#### <a name="microsoft-account-example"></a><span data-ttu-id="cc06b-120">Microsoft-fiók – példa</span><span class="sxs-lookup"><span data-stu-id="cc06b-120">Microsoft Account example</span></span>

<span data-ttu-id="cc06b-121">a következő példában hello Live SDK, amely támogatja a single-sign-on Windows Áruházbeli alkalmazások a Microsoft Account hello:</span><span class="sxs-lookup"><span data-stu-id="cc06b-121">hello following example uses hello Live SDK, which supports single-sign-on for Windows Store apps by using Microsoft Account:</span></span>

```
WL.login({ scope: "wl.basic"}).then(function (result) {
      client.login(
            "microsoftaccount",
            {"authenticationToken": result.session.authentication_token})
      .done(function(results){
            alert("You are now logged in as: " + results.userId);
      },
      function(error){
            alert("Error: " + err);
      });
});

```

<span data-ttu-id="cc06b-122">Ebben a példában egy tokent kap az élő csatlakozás esetén ez az App Service megadott tooyour hello bejelentkezési függvény meghívásával.</span><span class="sxs-lookup"><span data-stu-id="cc06b-122">This example gets a token from Live Connect, which is supplied tooyour App Service by calling hello login function.</span></span>

###<span data-ttu-id="cc06b-123"><a name="auth-getinfo"></a>Hogyan: hello hitelesített felhasználó kapcsolatos információkhoz</span><span class="sxs-lookup"><span data-stu-id="cc06b-123"><a name="auth-getinfo"></a>How to: Obtain information about hello authenticated user</span></span>

<span data-ttu-id="cc06b-124">hello hitelesítési adatok lekérhetők hello `/.auth/me` végpont egy HTTP-n keresztül hívja meg az AJAX-függvénytárat.</span><span class="sxs-lookup"><span data-stu-id="cc06b-124">hello authentication information can be retrieved from hello `/.auth/me` endpoint using an HTTP call with any AJAX library.</span></span>  <span data-ttu-id="cc06b-125">Győződjön meg arról, beállíthatja a hello `X-ZUMO-AUTH` fejléc tooyour hitelesítésére szolgáló jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="cc06b-125">Ensure you set hello `X-ZUMO-AUTH` header tooyour authentication token.</span></span>  <span data-ttu-id="cc06b-126">hello hitelesítésére szolgáló jogkivonat tárolódik `client.currentUser.mobileServiceAuthenticationToken`.</span><span class="sxs-lookup"><span data-stu-id="cc06b-126">hello authentication token is stored in `client.currentUser.mobileServiceAuthenticationToken`.</span></span>  <span data-ttu-id="cc06b-127">Például toouse hello fetch API:</span><span class="sxs-lookup"><span data-stu-id="cc06b-127">For example, toouse hello fetch API:</span></span>

```
var url = client.applicationUrl + '/.auth/me';
var headers = new Headers();
headers.append('X-ZUMO-AUTH', client.currentUser.mobileServiceAuthenticationToken);
fetch(url, { headers: headers })
    .then(function (data) {
        return data.json()
    }).then(function (user) {
        // hello user object contains hello claims for hello authenticated user
    });
```

<span data-ttu-id="cc06b-128">A fetch elérhető [npm-csomagként](https://www.npmjs.com/package/whatwg-fetch), vagy letölthető a [CDNJS-ről](https://cdnjs.com/libraries/fetch).</span><span class="sxs-lookup"><span data-stu-id="cc06b-128">Fetch is available as [an npm package](https://www.npmjs.com/package/whatwg-fetch) or for browser download from [CDNJS](https://cdnjs.com/libraries/fetch).</span></span> <span data-ttu-id="cc06b-129">JQuery vagy egy másik AJAX API toofetch hello információkat is használhatja.</span><span class="sxs-lookup"><span data-stu-id="cc06b-129">You could also use jQuery or another AJAX API toofetch hello information.</span></span>  <span data-ttu-id="cc06b-130">Az adatokat a rendszer JSON-objektumként fogadja.</span><span class="sxs-lookup"><span data-stu-id="cc06b-130">Data is received as a JSON object.</span></span>
