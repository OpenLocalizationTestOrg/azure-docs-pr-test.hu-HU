### <a name="server-auth"></a>Útmutató: Hitelesítés szolgáltatóval (Server Flow)
toohave Mobile Apps kezelése hello hitelesítési folyamat az alkalmazásban, regisztrálnia kell az alkalmazást az identitásszolgáltató. Az Azure App Service, végre kell tooconfigure hello Alkalmazásazonosítót és a szolgáltató által biztosított titkos kulcsot.
További információkért lásd: hello oktatóanyag [Add authentication tooyour alkalmazás](../articles/app-service-mobile/app-service-mobile-cordova-get-started-users.md).

Miután regisztrálta az identitásszolgáltató, hívja hello `.login()` a szolgáltató hello nevű metódus. A Facebook toologin például a következő kód hello használja:

```
client.login("facebook").done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```

hello érvényes hello szolgáltató értékei a "aad", "facebook", "google", "microsoftaccount" és "twitter".

> [!NOTE]
> A hitelesítés Google-fiókkal jelenleg nem használható a Server Flow-n keresztül.  a Google tooauthenticate, kell használnia egy [ügyféltanúsítvány-folyamat metódus](#client-auth).

Ebben az esetben az Azure App Service hello OAuth 2.0 hitelesítési folyamat kezeli.  A kijelölt szolgáltató hello hello bejelentkezési oldalát jeleníti meg, és létrehoz egy App Service hitelesítés jogkivonatot hello identitásszolgáltatóval sikeres bejelentkezés után. hello bejelentkezési függvény, amikor végzett, adja vissza egy JSON-objektum, amely azt mutatja, mind a hello felhasználói Azonosítót, és az App Service hitelesítésére szolgáló jogkivonat hello userId és authenticationToken mezőket, illetve. Ez a token gyorsítótárazható, és újra felhasználható, amíg le nem jár.

###<a name="client-auth"></a>Útmutató: Hitelesítés szolgáltatóval (Client Flow)

Az alkalmazás hello identitásszolgáltató függetlenül is kapcsolatba, és adja meg a hello visszaadott token tooyour App Service-hitelesítéshez. Az ügyfél folyamata lehetővé teszi tooprovide egy egyszeri bejelentkezési élmény a felhasználók vagy a hello identitásszolgáltató tooretrieve további felhasználó adatait.

#### <a name="social-authentication-basic-example"></a>Közösségi portálos hitelesítés – alapszintű példa

Ez a példa a Facebook ügyfél-SDK-t használja a hitelesítéshez:

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
Ez a példa feltételezi, hogy hello jogkivonat megadott hello megfelelő szolgáltató SDK hello token változó tárolja.

#### <a name="microsoft-account-example"></a>Microsoft-fiók – példa

a következő példában hello Live SDK, amely támogatja a single-sign-on Windows Áruházbeli alkalmazások a Microsoft Account hello:

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

Ebben a példában egy tokent kap az élő csatlakozás esetén ez az App Service megadott tooyour hello bejelentkezési függvény meghívásával.

###<a name="auth-getinfo"></a>Hogyan: hello hitelesített felhasználó kapcsolatos információkhoz

hello hitelesítési adatok lekérhetők hello `/.auth/me` végpont egy HTTP-n keresztül hívja meg az AJAX-függvénytárat.  Győződjön meg arról, beállíthatja a hello `X-ZUMO-AUTH` fejléc tooyour hitelesítésére szolgáló jogkivonat.  hello hitelesítésére szolgáló jogkivonat tárolódik `client.currentUser.mobileServiceAuthenticationToken`.  Például toouse hello fetch API:

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

A fetch elérhető [npm-csomagként](https://www.npmjs.com/package/whatwg-fetch), vagy letölthető a [CDNJS-ről](https://cdnjs.com/libraries/fetch). JQuery vagy egy másik AJAX API toofetch hello információkat is használhatja.  Az adatokat a rendszer JSON-objektumként fogadja.
