---
title: "Az Azure AD v2 JS SPA interaktív telepítés - használata |} Microsoft Docs"
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
ms.openlocfilehash: f52157df298ddfc1c1b29a18dc9a54aae59b52a3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
## <a name="use-the-microsoft-authentication-library-msal-to-sign-in-the-user"></a><span data-ttu-id="752bc-103">A Microsoft hitelesítési könyvtár (MSAL) segítségével bejelentkezés a felhasználói</span><span class="sxs-lookup"><span data-stu-id="752bc-103">Use the Microsoft Authentication Library (MSAL) to sign-in the user</span></span>

1.  <span data-ttu-id="752bc-104">Hozzon létre egy fájlt `app.js`.</span><span class="sxs-lookup"><span data-stu-id="752bc-104">Create a file named `app.js`.</span></span> <span data-ttu-id="752bc-105">Ha a Visual Studio használ, válassza ki a project (projekt gyökérmappájában), kattintson a jobb gombbal, és válassza: `Add`  >  `New Item`  >  `JavaScript File`:</span><span class="sxs-lookup"><span data-stu-id="752bc-105">If you are using Visual Studio, select the project (project root folder), right click and select: `Add` > `New Item` > `JavaScript File`:</span></span>
2.  <span data-ttu-id="752bc-106">Adja hozzá a következő kódot a `app.js` fájlt:</span><span class="sxs-lookup"><span data-stu-id="752bc-106">Add the following code to your `app.js` file:</span></span>

```javascript
// Graph API endpoint to show user profile
var graphApiEndpoint = "https://graph.microsoft.com/v1.0/me";

// Graph API scope used to obtain the access token to read user profile
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
    // If page is refreshed, continue to display user info
    if (!userAgentApplication.isCallback(window.location.hash) && window.parent === window && !window.opener) {
        var user = userAgentApplication.getUser();
        if (user) {
            callGraphApi();
        }
    }
}

/**
 * Call the Microsoft Graph API and display the results on the page. Sign the user in if necessary
 */
function callGraphApi() {
    var user = userAgentApplication.getUser();
    if (!user) {
        // If user is not signed in, then prompt user to sign in via loginRedirect.
        // This will redirect user to the Azure Active Directory v2 Endpoint
        userAgentApplication.loginRedirect(graphAPIScopes);
        // The call to loginRedirect above frontloads the consent to query Graph API during the sign-in.
        // If you want to use dynamic consent, just remove the graphAPIScopes from loginRedirect call.
        // As such, user will be prompted to give consent when requested access to a resource that 
        // he/she hasn't consented before. In the case of this application - 
        // the first time the Graph API call to obtain user's profile is executed.
    } else {
        // If user is already signed in, display the user info
        var userInfoElement = document.getElementById("userInfo");
        userInfoElement.parentElement.classList.remove("hidden");
        userInfoElement.innerHTML = JSON.stringify(user, null, 4);

        // Show Sign-Out button
        document.getElementById("signOutButton").classList.remove("hidden");

        // Now Call Graph API to show the user profile information:
        var graphCallResponseElement = document.getElementById("graphResponse");
        graphCallResponseElement.parentElement.classList.remove("hidden");
        graphCallResponseElement.innerText = "Calling Graph ...";

        // In order to call the Graph API, an access token needs to be acquired.
        // Try to acquire the token used to query Graph API silently first:
        userAgentApplication.acquireTokenSilent(graphAPIScopes)
            .then(function (token) {
                //After the access token is acquired, call the Web API, sending the acquired token
                callWebApiWithToken(graphApiEndpoint, token, graphCallResponseElement, document.getElementById("accessToken"));

            }, function (error) {
                // If the acquireTokenSilent() method fails, then acquire the token interactively via acquireTokenRedirect().
                // In this case, the browser will redirect user back to the Azure Active Directory v2 Endpoint so the user 
                // can reenter the current username/ password and/ or give consent to new permissions your application is requesting.
                // After authentication/ authorization completes, this page will be reloaded again and callGraphApi() will be executed on page load.
                // Then, acquireTokenSilent will then get the token silently, the Graph API call results will be made and results will be displayed in the page.
                if (error) {
                    userAgentApplication.acquireTokenRedirect(graphAPIScopes);
                }
            });

    }
}

/**
 * Callback method from sign-in: if no errors, call callGraphApi() to show results.
 * @param {string} errorDesc - If error occur, the error message
 * @param {object} token - The token received from login
 * @param {object} error - The error string
 * @param {string} tokenType - the token type: usually id_token
 */
function loginCallback(errorDesc, token, error, tokenType) {
    if (errorDesc) {
        showError(msal.authority, error, errorDesc);
    } else {
        callGraphApi();
    }
}

/**
 * Show an error message in the page
 * @param {string} endpoint - the endpoint used for the error message
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
### <a name="more-information"></a><span data-ttu-id="752bc-107">További információ</span><span class="sxs-lookup"><span data-stu-id="752bc-107">More Information</span></span>

<span data-ttu-id="752bc-108">Miután a felhasználó kattint a *"A Microsoft Graph API hívása"* gomb, először `callGraphApi` metódushívások `loginRedirect` jelentkezhet be a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="752bc-108">After a user clicks the *‘Call Microsoft Graph API’* button for the first time, `callGraphApi` method calls `loginRedirect` to sign in the user.</span></span> <span data-ttu-id="752bc-109">Ez a módszer eredményezi irányít át a felhasználót, hogy a *Microsoft Azure Active Directory v2 végpont* kérni, és a felhasználó hitelesítő adatainak ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="752bc-109">This method results in redirecting the user to the *Microsoft Azure Active Directory v2 endpoint* to prompt and validate the user's credentials.</span></span> <span data-ttu-id="752bc-110">Hatására a sikeres bejelentkezés, a felhasználót a rendszer átirányítja az eredetire *index.html* lap, és egy jogkivonat érkezett, által feldolgozott `msal.js` és a rendszer gyorsítótárazza a jogkivonat található információkat.</span><span class="sxs-lookup"><span data-stu-id="752bc-110">As a result of a successful sign-in, the user is redirected back to the original *index.html* page, and a token is received, processed by `msal.js` and the information contained in the token is cached.</span></span> <span data-ttu-id="752bc-111">Ez a token is ismert, a *azonosító token* és a felhasználó, például a felhasználó megjelenített neve alapvető információkat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="752bc-111">This token is known as the *ID token* and contains basic information about the user, such as the user display name.</span></span> <span data-ttu-id="752bc-112">Ha használja a célokat a token által megadott adatokat, győződjön meg arról, hogy ez a token érvényesíti a háttérkiszolgálón garantálja, hogy a jogkivonat részére adták ki egy érvényes felhasználói alkalmazás szeretné.</span><span class="sxs-lookup"><span data-stu-id="752bc-112">If you plan to use any data provided by this token for any purposes, you need to make sure this token is validated by your backend server to guarantee that the token was issued to a valid user for your application.</span></span>

<span data-ttu-id="752bc-113">Ez az útmutató által generált SPA hajtsa végre közvetlenül a Azonosítót jogkivonatban használatát – ehelyett meghívja `acquireTokenSilent` és/vagy `acquireTokenRedirect` megszerzése egy *hozzáférési jogkivonat* kérdezhetők le a Microsoft Graph API-val.</span><span class="sxs-lookup"><span data-stu-id="752bc-113">The SPA generated by this guide does not make use directly of the ID token – instead, it calls `acquireTokenSilent` and/or `acquireTokenRedirect` to acquire an *access token* used to query the Microsoft Graph API.</span></span> <span data-ttu-id="752bc-114">Ha egy mintát, amely érvényesíti a azonosító jogkivonatot kell, vessen egy pillantást [ez](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "Github active-directory-javascript-singlepageapp-dotnet-webapi-v2 minta") mintaalkalmazást a Githubon – a minta használja az ASP .NET webes API-t a jogkivonatok érvényesség-ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="752bc-114">If you need a sample that validates the ID token, take a look at [this](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2 "Github active-directory-javascript-singlepageapp-dotnet-webapi-v2 sample") sample application in GitHub – the sample uses an ASP.NET Web API for token validation.</span></span>

#### <a name="getting-a-user-token-interactively"></a><span data-ttu-id="752bc-115">A felhasználói token első interaktív módon</span><span class="sxs-lookup"><span data-stu-id="752bc-115">Getting a user token interactively</span></span>

<span data-ttu-id="752bc-116">Miután a kezdeti bejelentkezés, nem szeretné a kérje meg felhasználók minden alkalommal, amikor kérjen így elért egy erőforrást – tokent kell újból hitelesítésre *acquireTokenSilent* kell használni a legtöbbször ennek tokenek beszerzése.</span><span class="sxs-lookup"><span data-stu-id="752bc-116">After the initial sign-in, you do not want the ask users to reauthenticate every time they need to request a token to access a resource – so *acquireTokenSilent* should be used most of the time to acquire tokens.</span></span> <span data-ttu-id="752bc-117">Vannak azonban olyan helyzetekben, amelyekre szüksége van azzal kényszerítheti a felhasználók kommunikálni az Azure Active Directory v2 végpont – például:</span><span class="sxs-lookup"><span data-stu-id="752bc-117">There are situations however that you need to force users interact with Azure Active Directory v2 endpoint – some examples include:</span></span>
-   <span data-ttu-id="752bc-118">Felhasználók esetleg adja meg újra a hitelesítő adataikat, mert lejárt a jelszava</span><span class="sxs-lookup"><span data-stu-id="752bc-118">Users may need to reenter their credentials because the password has expired</span></span>
-   <span data-ttu-id="752bc-119">Az alkalmazás, amelyet a felhasználó járul hozzá az erőforráshoz való hozzáférés</span><span class="sxs-lookup"><span data-stu-id="752bc-119">Your application is requesting access to a resource that the user needs to consent to</span></span>
-   <span data-ttu-id="752bc-120">Kéttényezős hitelesítés szükség</span><span class="sxs-lookup"><span data-stu-id="752bc-120">Two factor authentication is required</span></span>

<span data-ttu-id="752bc-121">Hívja a *acquireTokenRedirect(scope)* irányít át felhasználók az Azure Active Directory v2 végpont eredményez (vagy *acquireTokenPopup(scope)* eredménye egy előugró ablak) Ha a felhasználók számára kell a erősítse meg a hitelesítő adataikat, adjon engedélye szükséges erőforrás, vagy befejezése a két kéttényezős hitelesítéssel.</span><span class="sxs-lookup"><span data-stu-id="752bc-121">Calling the *acquireTokenRedirect(scope)* result in redirecting users to the Azure Active Directory v2 endpoint (or *acquireTokenPopup(scope)* result on a popup window) where users need to interact with by either confirming their credentials, giving the consent to the required resource, or completing the two factor authentication.</span></span>

#### <a name="getting-a-user-token-silently"></a><span data-ttu-id="752bc-122">A felhasználói token első beavatkozás nélkül</span><span class="sxs-lookup"><span data-stu-id="752bc-122">Getting a user token silently</span></span>
<span data-ttu-id="752bc-123">A ` acquireTokenSilent` metódus kezeli a token kérése és -megújítás felhasználói beavatkozás nélkül.</span><span class="sxs-lookup"><span data-stu-id="752bc-123">The ` acquireTokenSilent` method handles token acquisitions and renewal without any user interaction.</span></span> <span data-ttu-id="752bc-124">Miután `loginRedirect` (vagy `loginPopup`) végrehajtása az első alkalommal `acquireTokenSilent` az beszerzése a jogkivonatokat, mivel kérelmezése vagy megújítása jogkivonatok hívásainak beavatkozás nélkül történik a további hívások - védett erőforrások eléréséhez használt általánosan használt módszer.</span><span class="sxs-lookup"><span data-stu-id="752bc-124">After `loginRedirect` (or `loginPopup`) is executed for the first time, `acquireTokenSilent` is the method commonly used to obtain tokens used to access protected resources for subsequent calls - as calls to request or renew tokens are made silently.</span></span>
<span data-ttu-id="752bc-125">`acquireTokenSilent`sikertelen lehet, hogy bizonyos esetekben – például a jelszó lejárt.</span><span class="sxs-lookup"><span data-stu-id="752bc-125">`acquireTokenSilent` may fail in some cases – for example, the user's password has expired.</span></span> <span data-ttu-id="752bc-126">Az alkalmazás kezeli ezt a kivételt, két módon:</span><span class="sxs-lookup"><span data-stu-id="752bc-126">Your application can handle this exception in two ways:</span></span>

1.  <span data-ttu-id="752bc-127">Hívás `acquireTokenRedirect` azonnal, ennek eredményeképpen a felhasználótól a bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="752bc-127">Make a call to `acquireTokenRedirect` immediately, which results in prompting the user to sign in.</span></span> <span data-ttu-id="752bc-128">Ezt a mintát gyakran használják az online alkalmazások esetén nincs hitelesítve tartalma az alkalmazásban a felhasználó számára elérhető.</span><span class="sxs-lookup"><span data-stu-id="752bc-128">This pattern is commonly used in online applications where there is no unauthenticated content in the application available to the user.</span></span> <span data-ttu-id="752bc-129">Ez az interaktív telepítő által létrehozott mintánkban Ez a minta.</span><span class="sxs-lookup"><span data-stu-id="752bc-129">The sample generated by this guided setup uses this pattern.</span></span>

2. <span data-ttu-id="752bc-130">Alkalmazások is végezhet egy visual arra utal, hogy a felhasználót, hogy egy interaktív bejelentkezés szükség, hogy a felhasználó kiválaszthatja a megfelelő időben való bejelentkezéshez, vagy az alkalmazás újra `acquireTokenSilent` egy későbbi időpontban.</span><span class="sxs-lookup"><span data-stu-id="752bc-130">Applications can also make a visual indication to the user that an interactive sign-in is required, so the user can select the right time to sign in, or the application can retry `acquireTokenSilent` at a later time.</span></span> <span data-ttu-id="752bc-131">Ez általában akkor használatos, amikor a felhasználó éppen megszakad nélkül tudja használni az alkalmazás egyéb funkciók – például nincs hitelesítve tartalom elérhető az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="752bc-131">This is commonly used when the user can use other functionality of the application without being disrupted - for example, there is unauthenticated content available in the application.</span></span> <span data-ttu-id="752bc-132">Ebben az esetben a felhasználó megadhatja, hogy jelentkezzen be a védett erőforrások eléréséhez, vagy frissítse az elavult adatokat szeretne.</span><span class="sxs-lookup"><span data-stu-id="752bc-132">In this case, the user can decide when they want to sign in to access the protected resource, or to refresh the outdated information.</span></span>

<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a><span data-ttu-id="752bc-133">A Microsoft Graph API használatával csak megszerzett jogkivonattal hívható</span><span class="sxs-lookup"><span data-stu-id="752bc-133">Call the Microsoft Graph API using the token you just obtained</span></span>

<span data-ttu-id="752bc-134">Adja hozzá a következő kódot a `app.js` fájlt:</span><span class="sxs-lookup"><span data-stu-id="752bc-134">Add the following code to your `app.js` file:</span></span>

```javascript
/**
 * Call a Web API using an access token.
 * @param {any} endpoint - Web API endpoint
 * @param {any} token - Access token
 * @param {object} responseElement - HTML element used to display the results
 * @param {object} showTokenElement = HTML element used to display the RAW access token
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
                        // Display response in the page
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
                        // Display response as error in the page
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

### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a><span data-ttu-id="752bc-135">További információ a ellen védett API REST hívás</span><span class="sxs-lookup"><span data-stu-id="752bc-135">More information on making a REST call against a protected API</span></span>

<span data-ttu-id="752bc-136">Ez az útmutató által létrehozott minta alkalmazásban a `callWebApiWithToken()` módszer használható HTTP `GET` kérelem ellen védett erőforrás jogkivonat szükséges, és térjen vissza a tartalom a hívó.</span><span class="sxs-lookup"><span data-stu-id="752bc-136">In the sample application created by this guide, the `callWebApiWithToken()` method is used to make an HTTP `GET` request against a protected resource that requires a token and then return the content to the caller.</span></span> <span data-ttu-id="752bc-137">Ez a módszer hozzáadja a megszerzett lexikális elem szerepel a *HTTP Authorization fejlécet*.</span><span class="sxs-lookup"><span data-stu-id="752bc-137">This method adds the acquired token in the *HTTP Authorization header*.</span></span> <span data-ttu-id="752bc-138">A mintaalkalmazás, ez az útmutató által létrehozott, az erőforrás a Microsoft Graph API *me* végpont – amely a felhasználói profil adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="752bc-138">For the sample application created by this guide, the resource is the Microsoft Graph API *me* endpoint – which displays the user's profile information.</span></span>

<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a><span data-ttu-id="752bc-139">A felhasználó kijelentkezik egy olyan metódus hozzáadása</span><span class="sxs-lookup"><span data-stu-id="752bc-139">Add a method to sign out the user</span></span>

<span data-ttu-id="752bc-140">Adja hozzá a következő kódot a `app.js` fájlt:</span><span class="sxs-lookup"><span data-stu-id="752bc-140">Add the following code to your `app.js` file:</span></span>

```javascript
/**
 * Sign-out the user
 */
function signOut() {
    userAgentApplication.logout();
}
```
