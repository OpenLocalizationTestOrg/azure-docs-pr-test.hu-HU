---
title: "Az Azure AD v2.0-végponttól vált |} Microsoft Docs"
description: "Az app model v2.0 nyilvános előzetes verzió protokollok végzett módosításokat leírását."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: e6c5b891-0b5d-47dc-8184-5d0ef6a1a006
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2016
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: ae73833a68db14804dc40eaf07ff7d3effaa9052
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="important-updates-to-the-v20-authentication-protocols"></a><span data-ttu-id="114e2-103">Fontos frissítések a v2.0-hitelesítési protokollok</span><span class="sxs-lookup"><span data-stu-id="114e2-103">Important Updates to the v2.0 Authentication Protocols</span></span>
<span data-ttu-id="114e2-104">Figyelmet fejlesztők!</span><span class="sxs-lookup"><span data-stu-id="114e2-104">Attention developers!</span></span> <span data-ttu-id="114e2-105">A következő két hétben frissítjük néhány frissítések az lehet, hogy olyan módosításokat a próbaidőszak alatt írt alkalmazások számára megtörje, amely a v2.0-hitelesítési protokollok számára.</span><span class="sxs-lookup"><span data-stu-id="114e2-105">Over the next two weeks, we will be making a few updates to our v2.0 authentication protocols that may mean breaking changes for any apps you have written during our preview period.</span></span>  

## <a name="who-does-this-affect"></a><span data-ttu-id="114e2-106">Aki befolyásolja ez?</span><span class="sxs-lookup"><span data-stu-id="114e2-106">Who does this affect?</span></span>
<span data-ttu-id="114e2-107">A v2.0 használandó írt alkalmazások összevont hitelesítési végpontot</span><span class="sxs-lookup"><span data-stu-id="114e2-107">Any apps that have been written to use the v2.0 converged authentication endpoint,</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

<span data-ttu-id="114e2-108">További információ a v2.0-végpontra található [Itt](active-directory-appmodel-v2-overview.md).</span><span class="sxs-lookup"><span data-stu-id="114e2-108">More information on the v2.0 endpoint can be found [here](active-directory-appmodel-v2-overview.md).</span></span>

<span data-ttu-id="114e2-109">A v2.0-végpontra használatával kódolás közvetlenül a v2.0 protokoll az alkalmazás létrehozása, ha bármely az OpenID Connect vagy OAuth webes middlewares használatával, vagy más 3. fél könyvtárak segítségével végezhet hitelesítést, akkor kell készíteni a projektek teszteléséhez, és végezze el a szükséges módosításokat.</span><span class="sxs-lookup"><span data-stu-id="114e2-109">If you have built an app using the v2.0 endpoint by coding directly to the v2.0 protocol, using any of our OpenID Connect or OAuth web middlewares, or using other 3rd party libraries to perform authentication, you should be prepared to test your projects and make changes if necessary.</span></span>

## <a name="who-doesnt-this-affect"></a><span data-ttu-id="114e2-110">Ki nem ez hatással van?</span><span class="sxs-lookup"><span data-stu-id="114e2-110">Who doesn\`t this affect?</span></span>
<span data-ttu-id="114e2-111">Az éles Azure AD hitelesítési végpont, elleni írt alkalmazások</span><span class="sxs-lookup"><span data-stu-id="114e2-111">Any apps that have been written against the production Azure AD authentication endpoint,</span></span>

```
https://login.microsoftonline.com/common/oauth2/authorize
```

<span data-ttu-id="114e2-112">Ez a protokoll köve van beállítva, és fog nem léptek fel a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="114e2-112">This protocol is set in stone and will not be experiencing any changes.</span></span>

<span data-ttu-id="114e2-113">Továbbá ha az alkalmazás **csak** használja az ADAL-könyvtár végezzen hitelesítést, nem kell bármit módosíthat.</span><span class="sxs-lookup"><span data-stu-id="114e2-113">Furthermore, if your app **only** uses our ADAL library to perform authentication, you won\`t have to change anything.</span></span>  <span data-ttu-id="114e2-114">ADAL rendelkezik az alkalmazás a módosításokat a védett.</span><span class="sxs-lookup"><span data-stu-id="114e2-114">ADAL has shielded your app from the changes.</span></span>  

## <a name="what-are-the-changes"></a><span data-ttu-id="114e2-115">Mik a módosításokat?</span><span class="sxs-lookup"><span data-stu-id="114e2-115">What are the changes?</span></span>
### <a name="removing-the-x5t-value-from-jwt-headers"></a><span data-ttu-id="114e2-116">A x5t érték eltávolítása a JWT fejlécek</span><span class="sxs-lookup"><span data-stu-id="114e2-116">Removing the x5t value from JWT headers</span></span>
<span data-ttu-id="114e2-117">A v2.0-végpontra használ JWT-jogkivonatokat széles körben, tartalmazó vonatkozó metaadatokat a jogkivonatot a paraméterek fejlécszakasza.</span><span class="sxs-lookup"><span data-stu-id="114e2-117">The v2.0 endpoint uses JWT tokens extensively, which contain a header parameters section with relevant metadata about the token.</span></span>  <span data-ttu-id="114e2-118">Ha dekódolni a fejléc az aktuális JWTs közül az egyik, találjuk hasonlót:</span><span class="sxs-lookup"><span data-stu-id="114e2-118">If you decode the header of one of our current JWTs, you would find something like:</span></span>

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

<span data-ttu-id="114e2-119">Ahol a "x5t" és a "kid" tulajdonság azonosítása a nyilvános kulcs ellenőrzése a jogkivonat aláírása, mert beolvasta az OpenID Connect metaadat-végpontjához használandó.</span><span class="sxs-lookup"><span data-stu-id="114e2-119">Where both the "x5t" and "kid" properties identify the public key that should be used to validate the token\`s signature, as retrieved from the OpenID Connect metadata endpoint.</span></span>

<span data-ttu-id="114e2-120">Itt hajtunk módosítása, hogy távolítsa el a "x5t" tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="114e2-120">The change we are making here is to remove the "x5t" property.</span></span>  <span data-ttu-id="114e2-121">Előfordulhat, hogy továbbra is használhatja ugyanazt a mechanizmust érvényesíthet jogkivonatokat, de csak a "kid" tulajdonság beolvasása a megfelelő nyilvános kulccsal, az OpenID Connect protokoll megadott szoftverlicenceknek.</span><span class="sxs-lookup"><span data-stu-id="114e2-121">You may continue to use the same mechanisms to validate tokens, but should rely only on the "kid" property to retrieve the correct public key, as specified in the OpenID Connect protocol.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="114e2-122">**A feladat: Győződjön meg arról, hogy az alkalmazás nem létezik-e a x5t érték függ.**</span><span class="sxs-lookup"><span data-stu-id="114e2-122">**Your job: Make sure your app does not depend on the existence of the x5t value.**</span></span>
> 
> 

### <a name="removing-profileinfo"></a><span data-ttu-id="114e2-123">Profile_info eltávolítása</span><span class="sxs-lookup"><span data-stu-id="114e2-123">Removing profile_info</span></span>
<span data-ttu-id="114e2-124">Korábban, a v2.0-végponttal rendelkezik lett visszaadni a base64-kódolású JSON-objektum nevű token válaszok `profile_info`.</span><span class="sxs-lookup"><span data-stu-id="114e2-124">Previously, the v2.0 endpoint has been returning a base64 encoded JSON object in token responses called `profile_info`.</span></span>  <span data-ttu-id="114e2-125">Amikor olyan hozzáférési jogkivonatot kérnek a v2.0-végpontra vonatkozó kérelmet küld:</span><span class="sxs-lookup"><span data-stu-id="114e2-125">When requesting an access token from the v2.0 endpoint by sending a request to:</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

<span data-ttu-id="114e2-126">A válasz a következő JSON-objektum jelenne meg:</span><span class="sxs-lookup"><span data-stu-id="114e2-126">The response would look like the following JSON object:</span></span>

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

<span data-ttu-id="114e2-127">A `profile_info` érték található információ a felhasználó, aki regisztrált a alkalmazásba – a megjelenített név, Utónév, Vezetéknév, e-mail cím, azonosítója, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="114e2-127">The `profile_info` value contained information about the user who signed into the app - their display name, first name, surname, email address, identifier, and so on.</span></span>  <span data-ttu-id="114e2-128">Elsősorban a `profile_info` token-gyorsítótárazási volt használva, és megjeleníti a célra.</span><span class="sxs-lookup"><span data-stu-id="114e2-128">Primarily, the `profile_info` was used for token caching and display purposes.</span></span>

<span data-ttu-id="114e2-129">Most megszüntetjük a `profile_info` érték –, de ne aggódjon, azt az adatokat továbbra is ad át a fejlesztőkig némileg eltérő helyen.</span><span class="sxs-lookup"><span data-stu-id="114e2-129">We are now removing the `profile_info` value – but don't worry, we are still providing this information to developers in a slightly different place.</span></span>  <span data-ttu-id="114e2-130">Ahelyett, hogy `profile_info`, a v2.0-végpontra most visszaállítja egy `id_token` minden token válaszként:</span><span class="sxs-lookup"><span data-stu-id="114e2-130">Instead of `profile_info`, the v2.0 endpoint will now return an `id_token` in each token response:</span></span>

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

<span data-ttu-id="114e2-131">Előfordulhat, hogy dekódolni, és elemezni a id_token profile_info kapott adatokat beolvasni.</span><span class="sxs-lookup"><span data-stu-id="114e2-131">You may decode and parse the id_token to retrieve the same information that you received from profile_info.</span></span>  <span data-ttu-id="114e2-132">A id_token egy JSON webes jogkivonat (JWT), az OpenID Connect által megadott tartalom.</span><span class="sxs-lookup"><span data-stu-id="114e2-132">The id_token is a JSON Web Token (JWT), with contents as specified by OpenID Connect.</span></span>  <span data-ttu-id="114e2-133">A kód minderre, legyen nagyon hasonló –, egyszerűen vissza kell fejteni a id_token a középső szegmens (szervezet), és base64 dekódolni a belül a JSON-objektum eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="114e2-133">The code for doing so should be very similar – you simply need to extract the middle segment (the body) of the id_token and base64 decode it to access the JSON object within.</span></span>

<span data-ttu-id="114e2-134">A következő két hétben kódaláírással kell az alkalmazásnak, hogy a felhasználói adatok lekérését vagy a `id_token` vagy `profile_info`; attól jelen.</span><span class="sxs-lookup"><span data-stu-id="114e2-134">Over the next two weeks, you should code your app to retrieve the user information from either the `id_token` or `profile_info`; whichever is present.</span></span>  <span data-ttu-id="114e2-135">Ily módon, ha módosítás történik, az alkalmazás zökkenőmentesen kezelni tud a átállás `profile_info` való `id_token` megszakítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="114e2-135">That way when the change is made, your app can seamlessly handle the transition from `profile_info` to `id_token` without interruption.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="114e2-136">**A feladat: Győződjön meg arról, hogy az alkalmazás nem függ a megléte a `profile_info` érték.**</span><span class="sxs-lookup"><span data-stu-id="114e2-136">**Your job: Make sure your app does not depend on the existence of the `profile_info` value.**</span></span>
> 
> 

### <a name="removing-idtokenexpiresin"></a><span data-ttu-id="114e2-137">Id_token_expires_in eltávolítása</span><span class="sxs-lookup"><span data-stu-id="114e2-137">Removing id_token_expires_in</span></span>
<span data-ttu-id="114e2-138">Hasonló `profile_info`, is megszüntetjük a `id_token_expires_in` válaszok paraméter.</span><span class="sxs-lookup"><span data-stu-id="114e2-138">Similar to `profile_info`, we are also removing the `id_token_expires_in` parameter from responses.</span></span>  <span data-ttu-id="114e2-139">Korábban, a a v2.0-végpontra vonatkozó értéket visszaadnia `id_token_expires_in` minden egyes id_token válasz, például egy engedélyezés válaszul együtt:</span><span class="sxs-lookup"><span data-stu-id="114e2-139">Previously, the v2.0 endpoint would return a value for `id_token_expires_in` along with each id_token response, for instance in an authorize response:</span></span>

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

<span data-ttu-id="114e2-140">Vagy a token válasz:</span><span class="sxs-lookup"><span data-stu-id="114e2-140">Or in a token response:</span></span>

```
{ 
    "token_type": "Bearer",
    "id_token_expires_in": 3599,
    "scope": "openid",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

<span data-ttu-id="114e2-141">A `id_token_expires_in` érték azt jelzi másodpercben a id_token érvényes marad.</span><span class="sxs-lookup"><span data-stu-id="114e2-141">The `id_token_expires_in` value would indicate the number of seconds the id_token would remain valid for.</span></span>  <span data-ttu-id="114e2-142">Most, megszüntetjük a `id_token_expires_in` teljesen érték.</span><span class="sxs-lookup"><span data-stu-id="114e2-142">Now, we are removing the `id_token_expires_in` value completely.</span></span>  <span data-ttu-id="114e2-143">Ehelyett használhat az OpenID Connect szabvány `nbf` és `exp` jogcímeket kell vizsgálni egy id_token érvényességét.</span><span class="sxs-lookup"><span data-stu-id="114e2-143">Instead, you may use the OpenID Connect standard `nbf` and `exp` claims to examine the validity of an id_token.</span></span>  <span data-ttu-id="114e2-144">Tekintse meg a [v2.0 jogkivonat referenciái](active-directory-v2-tokens.md) jogcímek olvashat.</span><span class="sxs-lookup"><span data-stu-id="114e2-144">See the [v2.0 token reference](active-directory-v2-tokens.md) for more information on these claims.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="114e2-145">**A feladat: Győződjön meg arról, hogy az alkalmazás nem függ a megléte a `id_token_expires_in` érték.**</span><span class="sxs-lookup"><span data-stu-id="114e2-145">**Your job: Make sure your app does not depend on the existence of the `id_token_expires_in` value.**</span></span>
> 
> 

### <a name="changing-the-claims-returned-by-scopeopenid"></a><span data-ttu-id="114e2-146">A jogcímek hatókör által visszaadott módosítása = openid</span><span class="sxs-lookup"><span data-stu-id="114e2-146">Changing the claims returned by scope=openid</span></span>
<span data-ttu-id="114e2-147">Ez a változás valójában a legjelentősebb – lesz, az hatással szinte minden a v2.0-végpontra alkalmazó alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="114e2-147">This change will be the most significant – in fact, it will affect almost every app that uses the v2.0 endpoint.</span></span>  <span data-ttu-id="114e2-148">Számos alkalmazás kérelmeket küldeni a v2.0-végpontot a a `openid` hatókörét, például:</span><span class="sxs-lookup"><span data-stu-id="114e2-148">Many applications send requests to the v2.0 endpoint using the `openid` scope, like:</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

<span data-ttu-id="114e2-149">Ma, amikor a felhasználó engedélyezi a hozzájárulási a `openid` hatókör, az alkalmazás a felhasználó adatai rengeteg hasznos fogadása az eredményül kapott id_token.</span><span class="sxs-lookup"><span data-stu-id="114e2-149">Today, when the user grants consent for the `openid` scope, your app receives a wealth of information about the user in the resulting id_token.</span></span>  <span data-ttu-id="114e2-150">Ezek a jogcímek tartalmazhatják a neve, elsődleges felhasználónév, e-mail címét, objektumazonosító: és több.</span><span class="sxs-lookup"><span data-stu-id="114e2-150">These claims can include their name, preferred username, email address, object ID, and more.</span></span>

<span data-ttu-id="114e2-151">Ez a frissítés azt módosítani az adatokat, amelyek a `openid` hatókör számára biztosítja az alkalmazás eléréséhez, jobb comform az OpenID Connect meghatározásával.</span><span class="sxs-lookup"><span data-stu-id="114e2-151">In this update, we are changing the information that the `openid` scope affords your app access to, to better comform with the OpenID Connect specification.</span></span>  <span data-ttu-id="114e2-152">A `openid` hatókör fog csak lehetővé teszi az alkalmazásnak a felhasználó bejelentkezni, és egy alkalmazás-specifikus azonosítót a felhasználó kap a `sub` a id_token a jogcímek.</span><span class="sxs-lookup"><span data-stu-id="114e2-152">The `openid` scope will only allow your app to sign the user in, and receive an app-specific identifier for the user in the `sub` claim of the id_token.</span></span>  <span data-ttu-id="114e2-153">A jogcímek, az csak egy id_token a `openid` nyújtott lesz nem tartalmaz személyazonosításra alkalmas adatok.</span><span class="sxs-lookup"><span data-stu-id="114e2-153">The claims in an id_token with only the `openid` scope granted will be devoid of any personally identifiable information.</span></span>  <span data-ttu-id="114e2-154">Példa id_token jogcímeket a következők:</span><span class="sxs-lookup"><span data-stu-id="114e2-154">Example id_token claims are:</span></span>

```
{ 
    "aud": "580e250c-8f26-49d0-bee8-1c078add1609",
    "iss": "https://login.microsoftonline.com/b9410318-09af-49c2-b0c3-653adc1f376e/v2.0 ",
    "iat": 1449520283,
    "nbf": 1449520283,
    "exp": 1449524183,
    "nonce": "12345",
    "sub": "MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q",
    "tid": "b9410318-09af-49c2-b0c3-653adc1f376e",
    "ver": "2.0",
}
```

<span data-ttu-id="114e2-155">Ha azt szeretné, személyazonosításra alkalmas adatokat (PII) beszerzése a felhasználóról az alkalmazásban, az alkalmazás további engedélyek kéréséhez a felhasználónak a kell.</span><span class="sxs-lookup"><span data-stu-id="114e2-155">If you want to obtain personally identifiable information (PII) about the user in your app, your app will need to request additional permissions from the user.</span></span>  <span data-ttu-id="114e2-156">Az OpenID Connect spec – vezetjük be két új hatókört támogatása a `email` és `profile` hatókörök – ami lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="114e2-156">We are introducing support for two new scopes from the OpenID Connect spec – the `email` and `profile` scopes – which allow you to do so.</span></span>

* <span data-ttu-id="114e2-157">A `email` hatóköre nagyon egyszerű – az alkalmazás eléréséhez a felhasználó elsődleges e-mail címéhez keresztül lehetővé teszi a `email` a id_token a jogcímek.</span><span class="sxs-lookup"><span data-stu-id="114e2-157">The `email` scope is very straightforward – it allows your app access to the user's primary email address via the `email` claim in the id_token.</span></span>  <span data-ttu-id="114e2-158">Vegye figyelembe, hogy a `email` jogcím nem mindig kerül be a id_tokens – csak akkor lesz része ha rendelkezésre áll a profil.</span><span class="sxs-lookup"><span data-stu-id="114e2-158">Note that the `email` claim will not always be present in id_tokens – it will only be included if available in the user's profile.</span></span>
* <span data-ttu-id="114e2-159">A `profile` objektumazonosító hatókör számára biztosítja az alkalmazás eléréséhez a felhasználó – a neve, az elsődleges felhasználónév, minden más alapvető adatait, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="114e2-159">The `profile` scope affords your app access to all other basic information about the user – their name, preferred username, object ID, and so on.</span></span>

<span data-ttu-id="114e2-160">Ez lehetővé teszi, hogy az alkalmazás minimális nyilvánosságra módon code – kérje meg a felhasználó csak a készlethez, hogy az alkalmazás a feladat elvégzéséhez szükséges információk.</span><span class="sxs-lookup"><span data-stu-id="114e2-160">This allows you to code your app in a minimal-disclosure fashion – you can ask the user for just the set of information that your app requires to do its job.</span></span>  <span data-ttu-id="114e2-161">Ha folytatja a az alkalmazás jelenleg fogadó felhasználói adatok teljes készletének első, a hitelesítési kérések bele kell foglalni a három hatóköröket:</span><span class="sxs-lookup"><span data-stu-id="114e2-161">If you want to continue getting the full set of user information that your app is currently receiving, you should include all three scopes in your authorization requests:</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

<span data-ttu-id="114e2-162">Az alkalmazás megkezdődhet az küldése a `email` és `profile` hatókörök azonnal, és a v2.0-végpontra a két hatóköröket elfogadja és engedélyek kér a felhasználóktól, szükség esetén megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="114e2-162">Your app can begin sending the `email` and `profile` scopes immediately, and the v2.0 endpoint will accept these two scopes and begin requesting permissions from users as necessary.</span></span>  <span data-ttu-id="114e2-163">Azonban az értelmezési módosítása a `openid` hatókör nem lépnek érvénybe néhány hétig.</span><span class="sxs-lookup"><span data-stu-id="114e2-163">However, the change in the interpretation of the `openid` scope will not take effect for a few weeks.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="114e2-164">**A feladat: vegye fel a `profile` és `email` hatóköröket, ha az alkalmazás csak a felhasználó adatait.**</span><span class="sxs-lookup"><span data-stu-id="114e2-164">**Your job: Add the `profile` and `email` scopes if your app requires information about the user.**</span></span>  <span data-ttu-id="114e2-165">Vegye figyelembe, hogy adal-t fog tartalmazni mindkét engedéllyel a kérelmek alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="114e2-165">Note that ADAL will include both of these permissions in requests by default.</span></span> 
> 
> 

### <a name="removing-the-issuer-trailing-slash"></a><span data-ttu-id="114e2-166">A záró perjelet kibocsátó eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="114e2-166">Removing the issuer trailing slash.</span></span>
<span data-ttu-id="114e2-167">Korábban a kibocsátó érték, amely megjelenik a v2.0-végpontra a jogkivonatok volt</span><span class="sxs-lookup"><span data-stu-id="114e2-167">Previously, the issuer value that appears in tokens from the v2.0 endpoint took the form</span></span>

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

<span data-ttu-id="114e2-168">Ha a guid volt a tenantId az Azure AD-bérlő a jogkivonatot kibocsátó.</span><span class="sxs-lookup"><span data-stu-id="114e2-168">Where the guid was the tenantId of the Azure AD tenant which issued the token.</span></span>  <span data-ttu-id="114e2-169">A módosítások a kibocsátó érték válik.</span><span class="sxs-lookup"><span data-stu-id="114e2-169">With these changes, the issuer value becomes</span></span>

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

<span data-ttu-id="114e2-170">mindkét jogkivonatokat, az OpenID Connect felderítési dokumentumban.</span><span class="sxs-lookup"><span data-stu-id="114e2-170">in both tokens and in the OpenID Connect discovery document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="114e2-171">**A feladat: Győződjön meg arról, hogy az alkalmazás és a záró perjelet anélkül kibocsátó értékét fogadja a kibocsátó érvényesítése során.**</span><span class="sxs-lookup"><span data-stu-id="114e2-171">**Your job: Make sure your app accepts the issuer value both with and without a trailing slash during issuer validation.**</span></span>
> 
> 

## <a name="why-change"></a><span data-ttu-id="114e2-172">Miért is megváltozik?</span><span class="sxs-lookup"><span data-stu-id="114e2-172">Why change?</span></span>
<span data-ttu-id="114e2-173">Ezek a változások bevezetéséről az elsődleges kifejlesztésének meg kell felelnie az OpenID Connect szabvány.</span><span class="sxs-lookup"><span data-stu-id="114e2-173">The primary motivation for introducing these changes is to be compliant with the OpenID Connect standard specification.</span></span>  <span data-ttu-id="114e2-174">Mivel OpenID Connect megfelelő, Reméljük, a Microsoft identity szolgáltatásokkal és az egyéb identitás-szolgáltatások az iparág integrálásával közötti különbségek minimalizálása érdekében.</span><span class="sxs-lookup"><span data-stu-id="114e2-174">By being OpenID Connect compliant, we hope to minimize differences between integrating with Microsoft identity services and with other identity services in the industry.</span></span>  <span data-ttu-id="114e2-175">A fejlesztők a kedvenc nyílt forráskódú hitelesítési könyvtárat használja anélkül, hogy módosítják a könyvtárak Microsoft különbségek befogadásához szeretnénk.</span><span class="sxs-lookup"><span data-stu-id="114e2-175">We want to enable developers to use their favorite open source authentication libraries without having to alter the libraries to accommodate Microsoft differences.</span></span>

## <a name="what-can-you-do"></a><span data-ttu-id="114e2-176">Mire szolgál?</span><span class="sxs-lookup"><span data-stu-id="114e2-176">What can you do?</span></span>
<span data-ttu-id="114e2-177">Mai megkezdheti a fent leírt módosításokat végzett.</span><span class="sxs-lookup"><span data-stu-id="114e2-177">As of today, you can begin making all of the changes described above.</span></span>  <span data-ttu-id="114e2-178">Azonnal kell:</span><span class="sxs-lookup"><span data-stu-id="114e2-178">You should immediately:</span></span>

1. <span data-ttu-id="114e2-179">**Távolítsa el a függőségeket a a `x5t` fejléc paraméter.**</span><span class="sxs-lookup"><span data-stu-id="114e2-179">**Remove any dependencies on the `x5t` header parameter.**</span></span>
2. <span data-ttu-id="114e2-180">**Áttérés kezelésére `profile_info` való `id_token` token válaszokban.**</span><span class="sxs-lookup"><span data-stu-id="114e2-180">**Gracefully handle the transition from `profile_info` to `id_token` in token responses.**</span></span>
3. <span data-ttu-id="114e2-181">**Távolítsa el a függőségeket a a `id_token_expires_in` válasz paraméter.**</span><span class="sxs-lookup"><span data-stu-id="114e2-181">**Remove any dependencies on the `id_token_expires_in` response parameter.**</span></span>
4. <span data-ttu-id="114e2-182">**Adja hozzá a `profile` és `email` hatóköröket az alkalmazáshoz, ha az alkalmazás alapszintű felhasználói információra van szüksége.**</span><span class="sxs-lookup"><span data-stu-id="114e2-182">**Add the `profile` and `email` scopes to your app if your app needs basic user information.**</span></span>
5. <span data-ttu-id="114e2-183">**Fogadja el vagy záró perjelet anélkül jogkivonatokba kibocsátó értékeket.**</span><span class="sxs-lookup"><span data-stu-id="114e2-183">**Accept issuer values in tokens both with and without a trailing slash.**</span></span>

<span data-ttu-id="114e2-184">A [v2.0 protokoll dokumentációját](active-directory-v2-protocols.md) már frissítették a változásoknak, így használhatja referenciaként létrehozásában, frissítse a kódot.</span><span class="sxs-lookup"><span data-stu-id="114e2-184">Our [v2.0 protocol documentation](active-directory-v2-protocols.md) has already been updated to reflect these changes, so you may use it as reference in helping update your code.</span></span>

<span data-ttu-id="114e2-185">Ha módosításait hatókörön további kérdése van, küldje el nyugodtan érheti el nekünk a Twitteren: @AzureAD.</span><span class="sxs-lookup"><span data-stu-id="114e2-185">If you have any further questions on the scope of the changes, please feel free to reach out to us on Twitter at @AzureAD.</span></span>

## <a name="how-often-will-protocol-changes-occur"></a><span data-ttu-id="114e2-186">Milyen gyakran történik a protokoll módosításait?</span><span class="sxs-lookup"><span data-stu-id="114e2-186">How often will protocol changes occur?</span></span>
<span data-ttu-id="114e2-187">A hitelesítési protokollok működéséhez minden további megtörje vált nem várható.</span><span class="sxs-lookup"><span data-stu-id="114e2-187">We do not foresee any further breaking changes to the authentication protocols.</span></span>  <span data-ttu-id="114e2-188">Ezeket a módosításokat, azokat egy kiadási szándékosan azt vannak kötegelése, így nem kell végighaladnia az ilyen típusú frissítési folyamat újra bármikor elérhető.</span><span class="sxs-lookup"><span data-stu-id="114e2-188">We are intentionally bundling these changes into one release so that you won\`t have to go through this type of update process again any time soon.</span></span>  <span data-ttu-id="114e2-189">Természetesen segítségével szolgáltatásokat adhat a v2.0 összevont hitelesítési szolgáltatás, amely kihasználhatja a rendszer a Folytatás, de ezeket a módosításokat kell adalékanyag és nem a meglévő kódot sortörés.</span><span class="sxs-lookup"><span data-stu-id="114e2-189">Of course, we will continue to add features to the converged v2.0 authentication service that you can take advantage of, but those changes should be additive and not break existing code.</span></span>

<span data-ttu-id="114e2-190">Végül szeretnénk dolgot próbálhatja ki a próbaidőszak alatt Köszönjük, hogy fel.</span><span class="sxs-lookup"><span data-stu-id="114e2-190">Lastly, we would like to say thank you for trying things out during the preview period.</span></span>  <span data-ttu-id="114e2-191">Az elemzések és a korai támogatók véleményeket hasznos információt eddigi, és Reméljük, megoszthatja vélemények és ötleteket fogja folytatni.</span><span class="sxs-lookup"><span data-stu-id="114e2-191">The insights and experiences of our early adopters have been invaluable thus far, and we hope you\`ll continue to share your opinions and ideas.</span></span>

<span data-ttu-id="114e2-192">Örömmel kódolási!</span><span class="sxs-lookup"><span data-stu-id="114e2-192">Happy coding!</span></span>

<span data-ttu-id="114e2-193">A Microsoft Identity osztály</span><span class="sxs-lookup"><span data-stu-id="114e2-193">The Microsoft Identity Division</span></span>

