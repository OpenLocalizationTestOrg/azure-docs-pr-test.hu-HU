---
title: "aaaAzure AD v2 JS SPA interaktív telepítés - használata |} Microsoft Docs"
description: "Hogyan JavaScript SPA-alkalmazások Azure Active Directory-v2 végpontja hozzáférési jogkivonatok igénylő API meghívása"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/01/2017
ms.author: andret
ms.openlocfilehash: 4f7f824ed787d998dc4aea3dc21c95d7dfe70ae0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="use-hello-microsoft-authentication-library-msal-toosign-in-hello-user"></a>Hello Microsoft hitelesítési könyvtár (MSAL) toosign a hello felhasználói használata

1.  Hozzon létre egy fájlt `app.js`. Ha használja a Visual Studio, válassza hello project (projekt gyökérmappájában), kattintson a jobb gombbal, és válassza ki: `Add`  >  `New Item`  >  `JavaScript File`:
2.  Adja hozzá a következő kód tooyour hello `app.js` fájlt:

```javascript
// Graph API endpoint tooshow user profile
var graphApiEndpoint = "https://graph.microsoft.com/v1.0/me";

// Graph API scope used tooobtain hello access token tooread user profile
var graphAPIScopes = ["https://graph.microsoft.com/user.read"];

// Initialize application
var userAgentApplication = new Msal.UserAgentApplication(msalconfig.clientID, null, loginCallback, {
    redirectUri: msalconfig.redirectUri
});

//Previous version of msal uses redirect url via a property
if (userAgentApplication.redirectUri) {
    userAgentApplication.redirectUri = msalconfig.redirectUri;
}

window.onload = function () {
    // If page is refreshed, continue toodisplay user info
    if (!userAgentApplication.isCallback(window.location.hash) && window.parent === window && !window.opener) {
        var user = userAgentApplication.getUser();
        if (user) {
            callGraphApi();
        }
    }
}

/**
 * Call hello Microsoft Graph API and display hello results on hello page. Sign hello user in if necessary
 */
function callGraphApi() {
    var user = userAgentApplication.getUser();
    if (!user) {
        // If user is not signed in, then prompt user toosign in via loginRedirect.
        // This will redirect user toohello Azure Active Directory v2 Endpoint
        userAgentApplication.loginRedirect(graphAPIScopes);
        // hello call toologinRedirect above frontloads hello consent tooquery Graph API during hello sign-in.
        // If you want toouse dynamic consent, just remove hello graphAPIScopes from loginRedirect call.
        // As such, user will be prompted toogive consent when requested access tooa resource that 
        // he/she hasn't consented before. In hello case of this application - 
        // hello first time hello Graph API call tooobtain user's profile is executed.
    } else {
        // If user is already signed in, display hello user info
        var userInfoElement = document.getElementById("userInfo");
        userInfoElement.parentElement.classList.remove("hidden");
        userInfoElement.innerHTML = JSON.stringify(user, null, 4);

        // Show Sign-Out button
        document.getElementById("signOutButton").classList.remove("hidden");

        // Now Call Graph API tooshow hello user profile information:
        var graphCallResponseElement = document.getElementById("graphResponse");
        graphCallResponseElement.parentElement.classList.remove("hidden");
        graphCallResponseElement.innerText = "Calling Graph ...";

        // In order toocall hello Graph API, an access token needs toobe acquired.
        // Try tooacquire hello token used tooquery Graph API silently first:
        userAgentApplication.acquireTokenSilent(graphAPIScopes)
            .then(function (token) {
                //After hello access token is acquired, call hello Web API, sending hello acquired token
                callWebApiWithToken(graphApiEndpoint, token, graphCallResponseElement, document.getElementById("accessToken"));

            }, function (error) {
                // If hello acquireTokenSilent() method fails, then acquire hello token interactively via acquireTokenRedirect().
                // In this case, hello browser will redirect user back toohello Azure Active Directory v2 Endpoint so hello user 
                // can reenter hello current username/ password and/ or give consent toonew permissions your application is requesting.
                // After authentication/ authorization completes, this page will be reloaded again and callGraphApi() will be executed on page load.
                // Then, acquireTokenSilent will then get hello token silently, hello Graph API call results will be made and results will be displayed in hello page.
                if (error) {
                    userAgentApplication.acquireTokenRedirect(graphAPIScopes);
                }
            });

    }
}

/**
 * Callback method from sign-in: if no errors, call callGraphApi() tooshow results.
 * @param {string} errorDesc - If error occur, hello error message
 * @param {object} token - hello token received from login
 * @param {object} error - hello error string
 * @param {string} tokenType - hello token type: usually id_token
 */
function loginCallback(errorDesc, token, error, tokenType) {
    if (errorDesc) {
        showError(msal.authority, error, errorDesc);
    } else {
        callGraphApi();
    }
}

/**
 * Show an error message in hello page
 * @param {string} endpoint - hello endpoint used for hello error message
 * @param {string} error - Error string
 * @param {string} errorDesc - Error description
 */
function showError(endpoint, error, errorDesc) {
    var formattedError = JSON.stringify(error, null, 4);
    if (formattedError.length < 3) {
        formattedError = error;
    }
    document.getElementById("errorMessage").innerHTML = "An error has occurred:<br/>Endpoint: " + endpoint + "<br/>Error: " + formattedError + "<br/>" + errorDesc;
    console.error(error);
}

```

<!--start-collapse-->
### <a name="more-information"></a>További információ

Után a felhasználó kattint hello *"Hívható meg Microsoft Graph API"* hello első elindításakor a gomb `callGraphApi` metódushívások `loginRedirect` toosign hello felhasználó. Ez a módszer eredményezi hello felhasználói toohello átirányítása *Microsoft Azure Active Directory v2 végpont* tooprompt és hello felhasználó hitelesítő adatainak ellenőrzése. Eredményeként egy sikeres bejelentkezés, hello felhasználói az eredeti átirányított hátsó toohello *index.html* lap, és egy jogkivonat érkezett, által feldolgozott `msal.js` és hello token lévő hello információt a rendszer gyorsítótárazza. Ez a token nevezik hello *azonosító token* és hello felhasználó, például hello felhasználó megjelenített neve alapvető információkat tartalmazza. Ha azt tervezi, hogy toouse adatokat megadott toomake meg arról, hogy a háttér-kiszolgáló tooguarantee, amely a hello token érvényesíti a jogkivonatot kell a célokat jogkivonatot, amelyet ki tooa érvényes felhasználói alkalmazás.

Ez az útmutató által generált SPA hello nem tesz közvetlenül hello azonosító token használatát – ehelyett meghívja `acquireTokenSilent` és/vagy `acquireTokenRedirect` tooacquire egy *hozzáférési jogkivonat* tooquery hello Microsoft Graph API-t használja. Ha egy mintát, amely érvényesíti hello azonosító jogkivonatot kell, vessen egy pillantást [ez](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "Github active-directory-javascript-singlepageapp-dotnet-webapi-v2 minta") mintaalkalmazást a Githubon – hello mintánkban az ASP.NET Web API a jogkivonat-ellenőrzéshez.

#### <a name="getting-a-user-token-interactively"></a>A felhasználói token első interaktív módon

Bejelentkezési kezdeti hello után, a nem kívánt hello kérje meg felhasználók tooreauthenticate minden alkalommal, amikor toorequest van szükségük a token tooaccess erőforrás – így *acquireTokenSilent* kell lennie a legtöbb hello idő tooacquire jogkivonatok használt. Vannak azonban olyan helyzetekben, hogy kell-e a v2 végpont Azure Active Directory – felhasználók használják tooforce néhány példa:
-   Felhasználók valószínűleg tooreenter hitelesítő adataikat, mert hello jelszó lejárt.
-   Az alkalmazás, amely a felhasználói igények tooconsent hello hozzáférés tooa erőforrás
-   Kéttényezős hitelesítés szükség

Hívása hello *acquireTokenRedirect(scope)* átirányítás a felhasználók Azure Active Directory v2 toohello végpont eredményez (vagy *acquireTokenPopup(scope)* eredménye egy előugró ablak) Ha a felhasználók kell a toointeract vagy erősítse meg a hitelesítő adatok jogosultságot ad a hello hozzájárulási toohello erőforrás vagy épp hello kéttényezős hitelesítéshez szükséges.

#### <a name="getting-a-user-token-silently"></a>A felhasználói token első beavatkozás nélkül
Hello ` acquireTokenSilent` metódus kezeli a token kérése és -megújítás felhasználói beavatkozás nélkül. Után `loginRedirect` (vagy `loginPopup`) a hello végrehajtása első alkalommal `acquireTokenSilent` hello általánosan használt módszer tooobtain használt a jogkivonatokat tooaccess későbbi hívások - erőforrások védelmét szolgálja hívások toorequest vagy megújítása jogkivonatok beavatkozás nélkül történik.
`acquireTokenSilent`Előfordulhat, hogy sikertelen lesz, bizonyos esetekben – például a hello jelszó lejárt. Az alkalmazás kezeli ezt a kivételt, két módon:

1.  Hívás túl`acquireTokenRedirect` azonnal, mely eredmények arra kéri a hello a felhasználó toosign. Ezt a mintát gyakran használják az online alkalmazások ahol hello alkalmazás elérhető toohello felhasználó nincs hitelesítve tartalom van. Ez az interaktív telepítő által létrehozott hello minta a mintát használ.

2. Alkalmazások is készíthet, amelyek egy interaktív bejelentkezés szükség, hogy hello felhasználó kiválaszthatja a hello megfelelő időben toosign, vagy megpróbálhatja hello alkalmazás jelzést toohello felhasználó `acquireTokenSilent` egy későbbi időpontban. Ez általában akkor használatos, amikor hello felhasználói alatt megszakad nélkül is használható egyéb funkciók hello alkalmazás – például nincs hitelesítve tartalom hello alkalmazásban érhető el. Ebben az esetben hello felhasználó dönthet arról amikor tooaccess hello védett erőforrás toosign kívánják, vagy toorefresh hello elavult információkat.

<!--end-collapse-->

## <a name="call-hello-microsoft-graph-api-using-hello-token-you-just-obtained"></a>Csak kapott hello tokent hello Microsoft Graph API hívása

Adja hozzá a következő kód tooyour hello `app.js` fájlt:

```javascript
/**
 * Call a Web API using an access token.
 * @param {any} endpoint - Web API endpoint
 * @param {any} token - Access token
 * @param {object} responseElement - HTML element used toodisplay hello results
 * @param {object} showTokenElement = HTML element used toodisplay hello RAW access token
 */
function callWebApiWithToken(endpoint, token, responseElement, showTokenElement) {
    var headers = new Headers();
    var bearer = "Bearer " + token;
    headers.append("Authorization", bearer);
    var options = {
        method: "GET",
        headers: headers
    };

    fetch(endpoint, options)
        .then(function (response) {
            var contentType = response.headers.get("content-type");
            if (response.status === 200 && contentType && contentType.indexOf("application/json") !== -1) {
                response.json()
                    .then(function (data) {
                        // Display response in hello page
                        console.log(data);
                        responseElement.innerHTML = JSON.stringify(data, null, 4);
                        if (showTokenElement) {
                            showTokenElement.parentElement.classList.remove("hidden");
                            showTokenElement.innerHTML = token;
                        }
                    })
                    .catch(function (error) {
                        showError(endpoint, error);
                    });
            } else {
                response.json()
                    .then(function (data) {
                        // Display response as error in hello page
                        showError(endpoint, data);
                    })
                    .catch(function (error) {
                        showError(endpoint, error);
                    });
            }
        })
        .catch(function (error) {
            showError(endpoint, error);
        });
}
```
<!--start-collapse-->

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>További információ a ellen védett API REST hívás

Ez az útmutató által létrehozott hello mintaalkalmazást a hello `callWebApiWithToken()` metódus használt toomake HTTP `GET` egy jogkivonat és majd visszatérési hello tartalom toohello hívó igénylő védett erőforrás kérelmet. Ez a módszer szerzett hello token helyez hello *HTTP Authorization fejlécet*. Ez az útmutató által létrehozott hello mintaalkalmazás hello erőforrás hello Microsoft Graph API *me* végpont – hello felhasználói profil adatait jeleníti meg.

<!--end-collapse-->

## <a name="add-a-method-toosign-out-hello-user"></a>Egy metódus toosign kimenő hello felhasználó hozzáadása

Adja hozzá a következő kód tooyour hello `app.js` fájlt:

```javascript
/**
 * Sign-out hello user
 */
function signOut() {
    userAgentApplication.logout();
}
```
