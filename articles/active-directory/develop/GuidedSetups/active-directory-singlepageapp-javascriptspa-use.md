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
## <a name="use-hello-microsoft-authentication-library-msal-toosign-in-hello-user"></a><span data-ttu-id="1d0af-103">Hello Microsoft hitelesítési könyvtár (MSAL) toosign a hello felhasználói használata</span><span class="sxs-lookup"><span data-stu-id="1d0af-103">Use hello Microsoft Authentication Library (MSAL) toosign-in hello user</span></span>

1.  <span data-ttu-id="1d0af-104">Hozzon létre egy fájlt `app.js`.</span><span class="sxs-lookup"><span data-stu-id="1d0af-104">Create a file named `app.js`.</span></span> <span data-ttu-id="1d0af-105">Ha használja a Visual Studio, válassza hello project (projekt gyökérmappájában), kattintson a jobb gombbal, és válassza ki: `Add`  >  `New Item`  >  `JavaScript File`:</span><span class="sxs-lookup"><span data-stu-id="1d0af-105">If you are using Visual Studio, select hello project (project root folder), right click and select: `Add` > `New Item` > `JavaScript File`:</span></span>
2.  <span data-ttu-id="1d0af-106">Adja hozzá a következő kód tooyour hello `app.js` fájlt:</span><span class="sxs-lookup"><span data-stu-id="1d0af-106">Add hello following code tooyour `app.js` file:</span></span>

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
### <a name="more-information"></a><span data-ttu-id="1d0af-107">További információ</span><span class="sxs-lookup"><span data-stu-id="1d0af-107">More Information</span></span>

<span data-ttu-id="1d0af-108">Után a felhasználó kattint hello *"Hívható meg Microsoft Graph API"* hello első elindításakor a gomb `callGraphApi` metódushívások `loginRedirect` toosign hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="1d0af-108">After a user clicks hello *‘Call Microsoft Graph API’* button for hello first time, `callGraphApi` method calls `loginRedirect` toosign in hello user.</span></span> <span data-ttu-id="1d0af-109">Ez a módszer eredményezi hello felhasználói toohello átirányítása *Microsoft Azure Active Directory v2 végpont* tooprompt és hello felhasználó hitelesítő adatainak ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="1d0af-109">This method results in redirecting hello user toohello *Microsoft Azure Active Directory v2 endpoint* tooprompt and validate hello user's credentials.</span></span> <span data-ttu-id="1d0af-110">Eredményeként egy sikeres bejelentkezés, hello felhasználói az eredeti átirányított hátsó toohello *index.html* lap, és egy jogkivonat érkezett, által feldolgozott `msal.js` és hello token lévő hello információt a rendszer gyorsítótárazza.</span><span class="sxs-lookup"><span data-stu-id="1d0af-110">As a result of a successful sign-in, hello user is redirected back toohello original *index.html* page, and a token is received, processed by `msal.js` and hello information contained in hello token is cached.</span></span> <span data-ttu-id="1d0af-111">Ez a token nevezik hello *azonosító token* és hello felhasználó, például hello felhasználó megjelenített neve alapvető információkat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="1d0af-111">This token is known as hello *ID token* and contains basic information about hello user, such as hello user display name.</span></span> <span data-ttu-id="1d0af-112">Ha azt tervezi, hogy toouse adatokat megadott toomake meg arról, hogy a háttér-kiszolgáló tooguarantee, amely a hello token érvényesíti a jogkivonatot kell a célokat jogkivonatot, amelyet ki tooa érvényes felhasználói alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="1d0af-112">If you plan toouse any data provided by this token for any purposes, you need toomake sure this token is validated by your backend server tooguarantee that hello token was issued tooa valid user for your application.</span></span>

<span data-ttu-id="1d0af-113">Ez az útmutató által generált SPA hello nem tesz közvetlenül hello azonosító token használatát – ehelyett meghívja `acquireTokenSilent` és/vagy `acquireTokenRedirect` tooacquire egy *hozzáférési jogkivonat* tooquery hello Microsoft Graph API-t használja.</span><span class="sxs-lookup"><span data-stu-id="1d0af-113">hello SPA generated by this guide does not make use directly of hello ID token – instead, it calls `acquireTokenSilent` and/or `acquireTokenRedirect` tooacquire an *access token* used tooquery hello Microsoft Graph API.</span></span> <span data-ttu-id="1d0af-114">Ha egy mintát, amely érvényesíti hello azonosító jogkivonatot kell, vessen egy pillantást [ez](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "Github active-directory-javascript-singlepageapp-dotnet-webapi-v2 minta") mintaalkalmazást a Githubon – hello mintánkban az ASP.NET Web API a jogkivonat-ellenőrzéshez.</span><span class="sxs-lookup"><span data-stu-id="1d0af-114">If you need a sample that validates hello ID token, take a look at [this](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "Github active-directory-javascript-singlepageapp-dotnet-webapi-v2 sample") sample application in GitHub – hello sample uses an ASP.NET Web API for token validation.</span></span>

#### <a name="getting-a-user-token-interactively"></a><span data-ttu-id="1d0af-115">A felhasználói token első interaktív módon</span><span class="sxs-lookup"><span data-stu-id="1d0af-115">Getting a user token interactively</span></span>

<span data-ttu-id="1d0af-116">Bejelentkezési kezdeti hello után, a nem kívánt hello kérje meg felhasználók tooreauthenticate minden alkalommal, amikor toorequest van szükségük a token tooaccess erőforrás – így *acquireTokenSilent* kell lennie a legtöbb hello idő tooacquire jogkivonatok használt.</span><span class="sxs-lookup"><span data-stu-id="1d0af-116">After hello initial sign-in, you do not want hello ask users tooreauthenticate every time they need toorequest a token tooaccess a resource – so *acquireTokenSilent* should be used most of hello time tooacquire tokens.</span></span> <span data-ttu-id="1d0af-117">Vannak azonban olyan helyzetekben, hogy kell-e a v2 végpont Azure Active Directory – felhasználók használják tooforce néhány példa:</span><span class="sxs-lookup"><span data-stu-id="1d0af-117">There are situations however that you need tooforce users interact with Azure Active Directory v2 endpoint – some examples include:</span></span>
-   <span data-ttu-id="1d0af-118">Felhasználók valószínűleg tooreenter hitelesítő adataikat, mert hello jelszó lejárt.</span><span class="sxs-lookup"><span data-stu-id="1d0af-118">Users may need tooreenter their credentials because hello password has expired</span></span>
-   <span data-ttu-id="1d0af-119">Az alkalmazás, amely a felhasználói igények tooconsent hello hozzáférés tooa erőforrás</span><span class="sxs-lookup"><span data-stu-id="1d0af-119">Your application is requesting access tooa resource that hello user needs tooconsent to</span></span>
-   <span data-ttu-id="1d0af-120">Kéttényezős hitelesítés szükség</span><span class="sxs-lookup"><span data-stu-id="1d0af-120">Two factor authentication is required</span></span>

<span data-ttu-id="1d0af-121">Hívása hello *acquireTokenRedirect(scope)* átirányítás a felhasználók Azure Active Directory v2 toohello végpont eredményez (vagy *acquireTokenPopup(scope)* eredménye egy előugró ablak) Ha a felhasználók kell a toointeract vagy erősítse meg a hitelesítő adatok jogosultságot ad a hello hozzájárulási toohello erőforrás vagy épp hello kéttényezős hitelesítéshez szükséges.</span><span class="sxs-lookup"><span data-stu-id="1d0af-121">Calling hello *acquireTokenRedirect(scope)* result in redirecting users toohello Azure Active Directory v2 endpoint (or *acquireTokenPopup(scope)* result on a popup window) where users need toointeract with by either confirming their credentials, giving hello consent toohello required resource, or completing hello two factor authentication.</span></span>

#### <a name="getting-a-user-token-silently"></a><span data-ttu-id="1d0af-122">A felhasználói token első beavatkozás nélkül</span><span class="sxs-lookup"><span data-stu-id="1d0af-122">Getting a user token silently</span></span>
<span data-ttu-id="1d0af-123">Hello ` acquireTokenSilent` metódus kezeli a token kérése és -megújítás felhasználói beavatkozás nélkül.</span><span class="sxs-lookup"><span data-stu-id="1d0af-123">hello ` acquireTokenSilent` method handles token acquisitions and renewal without any user interaction.</span></span> <span data-ttu-id="1d0af-124">Után `loginRedirect` (vagy `loginPopup`) a hello végrehajtása első alkalommal `acquireTokenSilent` hello általánosan használt módszer tooobtain használt a jogkivonatokat tooaccess későbbi hívások - erőforrások védelmét szolgálja hívások toorequest vagy megújítása jogkivonatok beavatkozás nélkül történik.</span><span class="sxs-lookup"><span data-stu-id="1d0af-124">After `loginRedirect` (or `loginPopup`) is executed for hello first time, `acquireTokenSilent` is hello method commonly used tooobtain tokens used tooaccess protected resources for subsequent calls - as calls toorequest or renew tokens are made silently.</span></span>
<span data-ttu-id="1d0af-125">`acquireTokenSilent`Előfordulhat, hogy sikertelen lesz, bizonyos esetekben – például a hello jelszó lejárt.</span><span class="sxs-lookup"><span data-stu-id="1d0af-125">`acquireTokenSilent` may fail in some cases – for example, hello user's password has expired.</span></span> <span data-ttu-id="1d0af-126">Az alkalmazás kezeli ezt a kivételt, két módon:</span><span class="sxs-lookup"><span data-stu-id="1d0af-126">Your application can handle this exception in two ways:</span></span>

1.  <span data-ttu-id="1d0af-127">Hívás túl`acquireTokenRedirect` azonnal, mely eredmények arra kéri a hello a felhasználó toosign.</span><span class="sxs-lookup"><span data-stu-id="1d0af-127">Make a call too`acquireTokenRedirect` immediately, which results in prompting hello user toosign in.</span></span> <span data-ttu-id="1d0af-128">Ezt a mintát gyakran használják az online alkalmazások ahol hello alkalmazás elérhető toohello felhasználó nincs hitelesítve tartalom van.</span><span class="sxs-lookup"><span data-stu-id="1d0af-128">This pattern is commonly used in online applications where there is no unauthenticated content in hello application available toohello user.</span></span> <span data-ttu-id="1d0af-129">Ez az interaktív telepítő által létrehozott hello minta a mintát használ.</span><span class="sxs-lookup"><span data-stu-id="1d0af-129">hello sample generated by this guided setup uses this pattern.</span></span>

2. <span data-ttu-id="1d0af-130">Alkalmazások is készíthet, amelyek egy interaktív bejelentkezés szükség, hogy hello felhasználó kiválaszthatja a hello megfelelő időben toosign, vagy megpróbálhatja hello alkalmazás jelzést toohello felhasználó `acquireTokenSilent` egy későbbi időpontban.</span><span class="sxs-lookup"><span data-stu-id="1d0af-130">Applications can also make a visual indication toohello user that an interactive sign-in is required, so hello user can select hello right time toosign in, or hello application can retry `acquireTokenSilent` at a later time.</span></span> <span data-ttu-id="1d0af-131">Ez általában akkor használatos, amikor hello felhasználói alatt megszakad nélkül is használható egyéb funkciók hello alkalmazás – például nincs hitelesítve tartalom hello alkalmazásban érhető el.</span><span class="sxs-lookup"><span data-stu-id="1d0af-131">This is commonly used when hello user can use other functionality of hello application without being disrupted - for example, there is unauthenticated content available in hello application.</span></span> <span data-ttu-id="1d0af-132">Ebben az esetben hello felhasználó dönthet arról amikor tooaccess hello védett erőforrás toosign kívánják, vagy toorefresh hello elavult információkat.</span><span class="sxs-lookup"><span data-stu-id="1d0af-132">In this case, hello user can decide when they want toosign in tooaccess hello protected resource, or toorefresh hello outdated information.</span></span>

<!--end-collapse-->

## <a name="call-hello-microsoft-graph-api-using-hello-token-you-just-obtained"></a><span data-ttu-id="1d0af-133">Csak kapott hello tokent hello Microsoft Graph API hívása</span><span class="sxs-lookup"><span data-stu-id="1d0af-133">Call hello Microsoft Graph API using hello token you just obtained</span></span>

<span data-ttu-id="1d0af-134">Adja hozzá a következő kód tooyour hello `app.js` fájlt:</span><span class="sxs-lookup"><span data-stu-id="1d0af-134">Add hello following code tooyour `app.js` file:</span></span>

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

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a><span data-ttu-id="1d0af-135">További információ a ellen védett API REST hívás</span><span class="sxs-lookup"><span data-stu-id="1d0af-135">More information on making a REST call against a protected API</span></span>

<span data-ttu-id="1d0af-136">Ez az útmutató által létrehozott hello mintaalkalmazást a hello `callWebApiWithToken()` metódus használt toomake HTTP `GET` egy jogkivonat és majd visszatérési hello tartalom toohello hívó igénylő védett erőforrás kérelmet.</span><span class="sxs-lookup"><span data-stu-id="1d0af-136">In hello sample application created by this guide, hello `callWebApiWithToken()` method is used toomake an HTTP `GET` request against a protected resource that requires a token and then return hello content toohello caller.</span></span> <span data-ttu-id="1d0af-137">Ez a módszer szerzett hello token helyez hello *HTTP Authorization fejlécet*.</span><span class="sxs-lookup"><span data-stu-id="1d0af-137">This method adds hello acquired token in hello *HTTP Authorization header*.</span></span> <span data-ttu-id="1d0af-138">Ez az útmutató által létrehozott hello mintaalkalmazás hello erőforrás hello Microsoft Graph API *me* végpont – hello felhasználói profil adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="1d0af-138">For hello sample application created by this guide, hello resource is hello Microsoft Graph API *me* endpoint – which displays hello user's profile information.</span></span>

<!--end-collapse-->

## <a name="add-a-method-toosign-out-hello-user"></a><span data-ttu-id="1d0af-139">Egy metódus toosign kimenő hello felhasználó hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1d0af-139">Add a method toosign out hello user</span></span>

<span data-ttu-id="1d0af-140">Adja hozzá a következő kód tooyour hello `app.js` fájlt:</span><span class="sxs-lookup"><span data-stu-id="1d0af-140">Add hello following code tooyour `app.js` file:</span></span>

```javascript
/**
 * Sign-out hello user
 */
function signOut() {
    userAgentApplication.logout();
}
```
