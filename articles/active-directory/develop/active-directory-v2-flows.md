---
title: "Az Azure Active Directory v2.0-végpontra vonatkozó alkalmazástípusok |} Microsoft Docs"
description: "Milyen alkalmazásokat és az Azure Active Directory v2.0-végponttól által támogatott forgatókönyveket."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 494a06b8-0f9b-44e1-a7a2-d728cf2077ae
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 9d59e7f0e8f326c40be86e199d7712f6c565cc13
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="app-types-for-the-azure-active-directory-v20-endpoint"></a><span data-ttu-id="2bfd2-103">Alkalmazástípusok az Azure Active Directory v2.0 végpont</span><span class="sxs-lookup"><span data-stu-id="2bfd2-103">App types for the Azure Active Directory v2.0 endpoint</span></span>
<span data-ttu-id="2bfd2-104">Az Azure Active Directory (Azure AD) v2.0-végponttól hitelesítést is támogatja a modern alkalmazás-architektúrák esetén az összes szabványos protokollokat számos [OAuth 2.0-s vagy az OpenID Connect](active-directory-v2-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="2bfd2-104">The Azure Active Directory (Azure AD) v2.0 endpoint supports authentication for a variety of modern app architectures, all of them based on industry-standard protocols [OAuth 2.0 or OpenID Connect](active-directory-v2-protocols.md).</span></span> <span data-ttu-id="2bfd2-105">A cikkből megtudhatja, milyen típusú alkalmazásokat úgy, hogy az Azure AD v2.0, függetlenül a választott nyelv vagy platform használatával.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-105">This article describes the types of apps that you can build by using Azure AD v2.0, regardless of your preferred language or platform.</span></span> <span data-ttu-id="2bfd2-106">A cikkben szereplő információkat, amelyekkel jobban megértheti az összetettebb feladatok előtt készült [a kód munka megkezdéséhez](active-directory-appmodel-v2-overview.md#getting-started).</span><span class="sxs-lookup"><span data-stu-id="2bfd2-106">The information in this article is designed to help you understand high-level scenarios before you [start working with the code](active-directory-appmodel-v2-overview.md#getting-started).</span></span>

> [!NOTE]
> <span data-ttu-id="2bfd2-107">A v2.0-végpontra nem támogatja, minden Azure Active Directory forgatókönyvek és funkciók.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-107">The v2.0 endpoint doesn't support all Azure Active Directory scenarios and features.</span></span> <span data-ttu-id="2bfd2-108">Annak megállapításához, hogy a v2.0-végponttal kell használnia, olvassa el [v2.0 korlátozások](active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="2bfd2-108">To determine whether you should use the v2.0 endpoint, read about [v2.0 limitations](active-directory-v2-limitations.md).</span></span>
> 
> 

## <a name="the-basics"></a><span data-ttu-id="2bfd2-109">Az alapok</span><span class="sxs-lookup"><span data-stu-id="2bfd2-109">The basics</span></span>
<span data-ttu-id="2bfd2-110">Regisztrálnia kell az egyes alkalmazások által használt a v2.0-végpontra a [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="2bfd2-110">You must register each app that uses the v2.0 endpoint in the [Microsoft Application Registration Portal](https://apps.dev.microsoft.com).</span></span> <span data-ttu-id="2bfd2-111">A regisztrációs folyamata gyűjti, és hozzárendeli ezeket az értékeket az alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="2bfd2-111">The app registration process collects and assigns these values for your app:</span></span>

* <span data-ttu-id="2bfd2-112">Egy **Alkalmazásazonosító** , amely egyedileg azonosítja az alkalmazást</span><span class="sxs-lookup"><span data-stu-id="2bfd2-112">An **Application ID** that uniquely identifies your app</span></span>
* <span data-ttu-id="2bfd2-113">A **átirányítási URI-** , amely közvetlen válaszokhoz az alkalmazás segítségével</span><span class="sxs-lookup"><span data-stu-id="2bfd2-113">A **Redirect URI** that you can use to direct responses back to your app</span></span>
* <span data-ttu-id="2bfd2-114">Néhány más forgatókönyvekre jellemző értékek</span><span class="sxs-lookup"><span data-stu-id="2bfd2-114">A few other scenario-specific values</span></span>

<span data-ttu-id="2bfd2-115">További részletek megtudhatja, hogyan [alkalmazások regisztrálásának folyamatával](active-directory-v2-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="2bfd2-115">For details, learn how to [register an app](active-directory-v2-app-registration.md).</span></span>

<span data-ttu-id="2bfd2-116">Az alkalmazás regisztrálása után az alkalmazás kommunikál az Azure AD kérelmeket küld az Azure AD v2.0-végponttól.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-116">After the app is registered, the app communicates with Azure AD by sending requests to the Azure AD v2.0 endpoint.</span></span> <span data-ttu-id="2bfd2-117">Azt adja meg, nyílt forráskódú keretrendszerek és tárak, amelyek kezelik ezeket a kérelmeket részleteit.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-117">We provide open-source frameworks and libraries that handle the details of these requests.</span></span> <span data-ttu-id="2bfd2-118">Lehetősége is van a megvalósítása a hitelesítési logikát saját kérések végpontokkal való létrehozásával:</span><span class="sxs-lookup"><span data-stu-id="2bfd2-118">You also have the option to implement the authentication logic yourself by creating requests to these endpoints:</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```
<!-- TODO: Need a page for libraries to link to -->

## <a name="web-apps"></a><span data-ttu-id="2bfd2-119">Webalkalmazások</span><span class="sxs-lookup"><span data-stu-id="2bfd2-119">Web apps</span></span>
<span data-ttu-id="2bfd2-120">A web Apps (.NET, PHP, Java, Ruby, Python, csomópont), amelyhez a felhasználó böngészőn keresztül hozzáfér, használhatja a [OpenID Connect](active-directory-v2-protocols.md) a felhasználói bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-120">For web apps (.NET, PHP, Java, Ruby, Python, Node) that the user accesses through a browser, you can use [OpenID Connect](active-directory-v2-protocols.md) for user sign-in.</span></span> <span data-ttu-id="2bfd2-121">Az OpenID Connect a web app egy azonosító jogkivonatot kap.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-121">In OpenID Connect, the web app receives an ID token.</span></span> <span data-ttu-id="2bfd2-122">Egy azonosító lexikális elem egy biztonsági jogkivonatot, ellenőrzi a felhasználó személyazonosságát, és a felhasználói jogcímek formájában információkat nyújt:</span><span class="sxs-lookup"><span data-stu-id="2bfd2-122">An ID token is a security token that verifies the user's identity and provides information about the user in the form of claims:</span></span>

```
// Partial raw ID token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded ID token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

<span data-ttu-id="2bfd2-123">Minden a jogkivonatok és jogcímek különböző típusairól az alkalmazásban elérhető megismerheti a [v2.0 jogkivonatok hivatkozás](active-directory-v2-tokens.md).</span><span class="sxs-lookup"><span data-stu-id="2bfd2-123">You can learn about all the types of tokens and claims that are available to an app in the [v2.0 tokens reference](active-directory-v2-tokens.md).</span></span>

<span data-ttu-id="2bfd2-124">A kiszolgáló webalkalmazásokban bejelentkezés hitelesítési folyamata hajtja végre ezeket a magas szintű lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2bfd2-124">In web server apps, the sign-in authentication flow takes these high-level steps:</span></span>

![Webes alkalmazás hitelesítési folyamat](../../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

<span data-ttu-id="2bfd2-126">A felhasználói identitás biztosítható a Azonosítót jogkivonatban érvényesítés a v2.0-végpontra-tól kapott nyilvános aláírókulcs a.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-126">You can ensure the user's identity by validating the ID token with a public signing key that is received from the v2.0 endpoint.</span></span> <span data-ttu-id="2bfd2-127">A munkamenet cookie-k be van állítva, a felhasználó a következő lapkérések azonosítására használható.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-127">A session cookie is set, which can be used to identify the user on subsequent page requests.</span></span>

<span data-ttu-id="2bfd2-128">Ebben a forgatókönyvben a művelet megtekintéséhez tekintse meg a webes alkalmazás bejelentkezési Kódminták a 2.0-s verziója [bevezetés](active-directory-appmodel-v2-overview.md#getting-started) szakasz.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-128">To see this scenario in action, try one of the web app sign-in code samples in our v2.0 [Getting Started](active-directory-appmodel-v2-overview.md#getting-started) section.</span></span>

<span data-ttu-id="2bfd2-129">Egyszerű bejelentkezhet, valamint egy webalkalmazás-kiszolgáló valószínűleg hozzá kell egy másik webes szolgáltatás, például egy REST API-t.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-129">In addition to simple sign-in, a web server app might need to access another web service, such as a REST API.</span></span> <span data-ttu-id="2bfd2-130">Ebben az esetben a kiszolgáló webalkalmazás kapcsolatba lép kombinált OpenID Connectet és az OAuth 2.0 folyamatában, segítségével a [OAuth 2.0 hitelesítésikód-folyamata](active-directory-v2-protocols.md).</span><span class="sxs-lookup"><span data-stu-id="2bfd2-130">In this case, the web server app engages in a combined OpenID Connect and OAuth 2.0 flow, by using the [OAuth 2.0 authorization code flow](active-directory-v2-protocols.md).</span></span> <span data-ttu-id="2bfd2-131">Ebben a forgatókönyvben kapcsolatos további információkért olvassa el [Ismerkedés a webalkalmazások és webes API-k](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="2bfd2-131">For more information about this scenario, read about [getting started with web apps and Web APIs](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).</span></span>

## <a name="web-apis"></a><span data-ttu-id="2bfd2-132">Webes API-k</span><span class="sxs-lookup"><span data-stu-id="2bfd2-132">Web APIs</span></span>
<span data-ttu-id="2bfd2-133">A v2.0-végpontra segítségével webszolgáltatásai, például az alkalmazás RESTful webes API-t.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-133">You can use the v2.0 endpoint to secure web services, such as your app's RESTful Web API.</span></span> <span data-ttu-id="2bfd2-134">Azonosító-jogkivonatokat és a munkamenet cookie-k helyett egy webes API-t OAuth 2.0 hozzáférési tokent használ, az adatok védelmét és a bejövő kérelmek hitelesítéséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-134">Instead of ID tokens and session cookies, a Web API uses an OAuth 2.0 access token to secure its data and to authenticate incoming requests.</span></span> <span data-ttu-id="2bfd2-135">A hívó egy webes API hozzáfűz egy jogkivonatot a HTTP-kérések, például a hitelesítési fejlécéhez:</span><span class="sxs-lookup"><span data-stu-id="2bfd2-135">The caller of a Web API appends an access token in the authorization header of an HTTP request, like this:</span></span>

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

<span data-ttu-id="2bfd2-136">A webes API a hozzáférési tokent használ, ellenőrzi az API hívójának identitását, és a hozzáférési jogkivonat kódolt jogcímek információkhoz juthat a hívóról kibontani.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-136">The Web API uses the access token to verify the API caller's identity and to extract information about the caller from claims that are encoded in the access token.</span></span> <span data-ttu-id="2bfd2-137">Jogkivonatok és jogcímek, az alkalmazások számára elérhető összes típusú kapcsolatos további tudnivalókért lásd: a [v2.0 jogkivonatok hivatkozás](active-directory-v2-tokens.md).</span><span class="sxs-lookup"><span data-stu-id="2bfd2-137">To learn about all the types of tokens and claims that are available to an app, see the [v2.0 tokens reference](active-directory-v2-tokens.md).</span></span>

<span data-ttu-id="2bfd2-138">A webes API adhat a felhasználóknak részt vevő vagy tilthatják le az adott funkció vagy adatok engedélyek, más néven jelentkezik, mintha [hatókörök](active-directory-v2-scopes.md).</span><span class="sxs-lookup"><span data-stu-id="2bfd2-138">A Web API can give users the power to opt in or opt out of specific functionality or data by exposing permissions, also known as [scopes](active-directory-v2-scopes.md).</span></span> <span data-ttu-id="2bfd2-139">Egy hívó alkalmazás szerezni a hatókör engedéllyel, a felhasználónak bele kell egyeznie a hatókör a folyamat során.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-139">For a calling app to acquire permission to a scope, the user must consent to the scope during a flow.</span></span> <span data-ttu-id="2bfd2-140">A v2.0-végpontra megkérdezi a felhasználót, az engedélyeket, és majd rögzíti a engedélyeket kap a webes API-t minden hozzáférési jogkivonatok.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-140">The v2.0 endpoint asks the user for permission, and then records permissions in all access tokens that the Web API receives.</span></span> <span data-ttu-id="2bfd2-141">A Web API ellenőrzi a hozzáférési jogkivonatok egyes hívás fogadása és engedélyezési ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-141">The Web API validates the access tokens it receives on each call and performs authorization checks.</span></span>

<span data-ttu-id="2bfd2-142">Egy webes API-alkalmazás, illetve a kiszolgáló webalkalmazások, asztali és mobile apps szolgáltatásban, egyoldalas alkalmazások, kiszolgálóoldali démonok, és akár egyéb webes API-k minden típusú hozzáférési jogkivonatok fogadhat.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-142">A Web API can receive access tokens from all types of apps, including web server apps, desktop and mobile apps, single-page apps, server-side daemons, and even other Web APIs.</span></span> <span data-ttu-id="2bfd2-143">A magas szintű folyamata egy webes API így néz ki:</span><span class="sxs-lookup"><span data-stu-id="2bfd2-143">The high-level flow for a Web API looks like this:</span></span>

![Webes API-hitelesítési folyamat](../../media/active-directory-v2-flows/convergence_scenarios_webapi.png)

<span data-ttu-id="2bfd2-145">A webes API biztonságossá OAuth2 hozzáférési jogkivonatok segítségével történő beállításáról tekintse meg a webes API mintakódok a [bevezetés](active-directory-appmodel-v2-overview.md#getting-started) szakasz.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-145">To learn how to secure a Web API by using OAuth2 access tokens, check out the Web API code samples in our [Getting Started](active-directory-appmodel-v2-overview.md#getting-started) section.</span></span>

<span data-ttu-id="2bfd2-146">Sok esetben webes API-kat is kell kimenő kérelmek más alsóbb rétegbeli webes API-kat Azure Active Directory által biztosított.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-146">In many cases, web APIs also need to make outbound requests to other downstream web APIs secured by Azure Active Directory.</span></span>  <span data-ttu-id="2bfd2-147">Ehhez az szükséges, webes API-k kihasználhatja az Azure AD **a nevében a** folyamatot, amely lehetővé teszi, hogy a webes API-t a helyszíni Exchange számára egy új hozzáférési jogkivonat kimenő kérelmek használhatók egy bejövő jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-147">To do so, web APIs can take advantage of Azure AD's **On Behalf Of** flow, which allows the web API to exchange an incoming access token for another access token to be used in outbound requests.</span></span>  <span data-ttu-id="2bfd2-148">A v2.0-végpontra a folyamat nevében ismertetett [részletesen Itt](active-directory-v2-protocols-oauth-on-behalf-of.md).</span><span class="sxs-lookup"><span data-stu-id="2bfd2-148">The v2.0 endpoint's On Behalf Of flow is described in [detail here](active-directory-v2-protocols-oauth-on-behalf-of.md).</span></span>

## <a name="mobile-and-native-apps"></a><span data-ttu-id="2bfd2-149">Mobil- és natív alkalmazások</span><span class="sxs-lookup"><span data-stu-id="2bfd2-149">Mobile and native apps</span></span>
<span data-ttu-id="2bfd2-150">Eszköz telepített alkalmazások, többek között a mobil- és asztali alkalmazások esetében gyakran igényelnek hozzáférést a háttér-szolgáltatásaihoz vagy webes API-k, amelyek adatokat tárolhatnak, és a feladatokat egy felhasználó nevében.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-150">Device-installed apps, such as mobile and desktop apps, often need to access back-end services or Web APIs that store data and perform functions on behalf of a user.</span></span> <span data-ttu-id="2bfd2-151">Ezek az alkalmazások használatával adhat hozzá bejelentkezési és hitelesítési háttér szolgáltatásaihoz a [OAuth 2.0 hitelesítésikód-folyamata](active-directory-v2-protocols-oauth-code.md).</span><span class="sxs-lookup"><span data-stu-id="2bfd2-151">These apps can add sign-in and authorization to back-end services by using the [OAuth 2.0 authorization code flow](active-directory-v2-protocols-oauth-code.md).</span></span>

<span data-ttu-id="2bfd2-152">A folyamatot az alkalmazás fogad egy engedélyezési kód a v2.0-végpontra a felhasználó bejelentkezésekor.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-152">In this flow, the app receives an authorization code from the v2.0 endpoint when the user signs in.</span></span> <span data-ttu-id="2bfd2-153">Az engedélyezési kód hívására háttér-szolgáltatásaihoz a bejelentkezett felhasználó nevében az alkalmazás engedélyt jelöli.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-153">The authorization code represents the app's permission to call back-end services on behalf of the user who is signed in.</span></span> <span data-ttu-id="2bfd2-154">Az alkalmazás továbbíthatja az OAuth 2.0 hozzáférési tokent, és egy frissítési jogkivonat a háttérben engedélyezési kódot.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-154">The app can exchange the authorization code in the background for an OAuth 2.0 access token and a refresh token.</span></span> <span data-ttu-id="2bfd2-155">Az alkalmazás a hozzáférési jogkivonat hitelesítéshez használni kívánt webes API-k számára a HTTP-kérelmekre, és a frissítési jogkivonat használatával új hozzáférési jogkivonatok lekérésére, ha régebbi hozzáférési jogkivonatok végén.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-155">The app can use the access token to authenticate to Web APIs in HTTP requests, and use the refresh token to get new access tokens when older access tokens expire.</span></span>

![Natív alkalmazás hitelesítési folyamat](../../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="single-page-apps-javascript"></a><span data-ttu-id="2bfd2-157">Egyoldalas alkalmazások (JavaScript)</span><span class="sxs-lookup"><span data-stu-id="2bfd2-157">Single-page apps (JavaScript)</span></span>
<span data-ttu-id="2bfd2-158">Számos modern alkalmazások alkalmazás előtér írt elsősorban a JavaScript rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-158">Many modern apps have a single-page app front end that primarily is written in JavaScript.</span></span> <span data-ttu-id="2bfd2-159">Gyakran írás például AngularJS, az Ember.js vagy Durandal.js keretrendszer használatával.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-159">Often, it's written by using a framework like AngularJS, Ember.js, or Durandal.js.</span></span> <span data-ttu-id="2bfd2-160">Az Azure AD v2.0-végponttól használatával támogatja ezeket az alkalmazásokat a [OAuth 2.0 típusú implicit engedélyezési folyamat](active-directory-v2-protocols-implicit.md).</span><span class="sxs-lookup"><span data-stu-id="2bfd2-160">The Azure AD v2.0 endpoint supports these apps by using the [OAuth 2.0 implicit flow](active-directory-v2-protocols-implicit.md).</span></span>

<span data-ttu-id="2bfd2-161">A folyamatot, az alkalmazás-jogkivonatokat kap a v2.0-ről a végponthoz, minden kiszolgáló-kiszolgáló cseréje nélkül engedélyezik.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-161">In this flow, the app receives tokens directly from the v2.0 authorize endpoint, without any server-to-server exchanges.</span></span> <span data-ttu-id="2bfd2-162">Minden hitelesítési logikát és kezelési vesz munkamenet helyezhető el teljes egészében a JavaScript ügyfélprogramokban, de további oldal átirányítja az.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-162">All authentication logic and session handling takes place entirely in the JavaScript client, without extra page redirects.</span></span>

![Implicit hitelesítési folyamat](../../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

<span data-ttu-id="2bfd2-164">Tekintse meg a folyamatot, próbálja meg az alkalmazás mintakódok egyikét a [bevezetés](active-directory-appmodel-v2-overview.md#getting-started) szakasz.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-164">To see this scenario in action, try one of the single-page app code samples in our [Getting Started](active-directory-appmodel-v2-overview.md#getting-started) section.</span></span>

## <a name="daemons-and-server-side-apps"></a><span data-ttu-id="2bfd2-165">Démonok és kiszolgálóoldali alkalmazások</span><span class="sxs-lookup"><span data-stu-id="2bfd2-165">Daemons and server-side apps</span></span>
<span data-ttu-id="2bfd2-166">Az alkalmazások, amelyek hosszú futású folyamatokat, vagy egy felhasználói beavatkozás nélkül is szükség van a védett erőforrások, például webes API-k eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-166">Apps that have long-running processes or that operate without interaction with a user also need a way to access secured resources, such as Web APIs.</span></span> <span data-ttu-id="2bfd2-167">Ezek az alkalmazások hitelesítheti és a jogkivonatok lekérésére, az alkalmazás identitásával, nem pedig a felhasználó delegált identitása, az OAuth 2.0 ügyfél hitelesítő adatok folyamata a.</span><span class="sxs-lookup"><span data-stu-id="2bfd2-167">These apps can authenticate and get tokens by using the app's identity, rather than a user's delegated identity, with the OAuth 2.0 client credentials flow.</span></span>

<span data-ttu-id="2bfd2-168">A folyamatot, az alkalmazás kommunikál közvetlenül a `/token` végpont végpontok beszerzése:</span><span class="sxs-lookup"><span data-stu-id="2bfd2-168">In this flow, the app interacts directly with the `/token` endpoint to obtain endpoints:</span></span>

![Démon alkalmazás hitelesítési folyamat](../../media/active-directory-v2-flows/convergence_scenarios_daemon.png)

<span data-ttu-id="2bfd2-170">Egy démon alkalmazás elkészítésére, tekintse meg az ügyfél-hitelesítő adatok dokumentációjában találhatók a [bevezetés](active-directory-appmodel-v2-overview.md#getting-started) szakaszban, vagy adjon meg egy [.NET mintaalkalmazás](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).</span><span class="sxs-lookup"><span data-stu-id="2bfd2-170">To build a daemon app, see the client credentials documentation in our [Getting Started](active-directory-appmodel-v2-overview.md#getting-started) section, or try a [.NET sample app](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).</span></span>
