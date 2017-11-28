---
title: "aaaChanges toohello az Azure AD v2.0-végponttól |} Microsoft Docs"
description: "Toohello app model v2.0 nyilvános előzetes protokollok végrehajtott módosítások leírását."
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
ms.openlocfilehash: d7b28a481e12d5dbbc4a10110193bdbd754f4929
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="important-updates-toohello-v20-authentication-protocols"></a><span data-ttu-id="5dbbf-103">Fontos frissítések toohello v2.0 hitelesítési protokollok</span><span class="sxs-lookup"><span data-stu-id="5dbbf-103">Important Updates toohello v2.0 Authentication Protocols</span></span>
<span data-ttu-id="5dbbf-104">Figyelmet fejlesztők!</span><span class="sxs-lookup"><span data-stu-id="5dbbf-104">Attention developers!</span></span> <span data-ttu-id="5dbbf-105">Hello keresztül következő két héten frissítjük néhány frissítések tooour v2.0 hitelesítési protokollok, amelyek a módosítások a próbaidőszak alatt írt alkalmazások számára megtörje jelentheti.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-105">Over hello next two weeks, we will be making a few updates tooour v2.0 authentication protocols that may mean breaking changes for any apps you have written during our preview period.</span></span>  

## <a name="who-does-this-affect"></a><span data-ttu-id="5dbbf-106">Aki befolyásolja ez?</span><span class="sxs-lookup"><span data-stu-id="5dbbf-106">Who does this affect?</span></span>
<span data-ttu-id="5dbbf-107">Toouse hello v2.0 írt alkalmazások összevont hitelesítési végpontot</span><span class="sxs-lookup"><span data-stu-id="5dbbf-107">Any apps that have been written toouse hello v2.0 converged authentication endpoint,</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

<span data-ttu-id="5dbbf-108">További információ a v2.0-végponttól hello található [Itt](active-directory-appmodel-v2-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5dbbf-108">More information on hello v2.0 endpoint can be found [here](active-directory-appmodel-v2-overview.md).</span></span>

<span data-ttu-id="5dbbf-109">Ha már létrehozta a buildet hello v2.0-végponttól használatával kódolás közvetlenül toohello v2.0 protokoll, az alkalmazások bármely az OpenID Connect vagy OAuth webes middlewares használatával, vagy más 3. fél szalagtárak tooperform hitelesítést használ, érdemes tootest előkészítve a projektek és később Ha a szükséges módosításokat.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-109">If you have built an app using hello v2.0 endpoint by coding directly toohello v2.0 protocol, using any of our OpenID Connect or OAuth web middlewares, or using other 3rd party libraries tooperform authentication, you should be prepared tootest your projects and make changes if necessary.</span></span>

## <a name="who-doesnt-this-affect"></a><span data-ttu-id="5dbbf-110">Ki nem ez hatással van?</span><span class="sxs-lookup"><span data-stu-id="5dbbf-110">Who doesn\`t this affect?</span></span>
<span data-ttu-id="5dbbf-111">Hello éles Azure AD hitelesítési végpont, elleni írt alkalmazások</span><span class="sxs-lookup"><span data-stu-id="5dbbf-111">Any apps that have been written against hello production Azure AD authentication endpoint,</span></span>

```
https://login.microsoftonline.com/common/oauth2/authorize
```

<span data-ttu-id="5dbbf-112">Ez a protokoll köve van beállítva, és fog nem léptek fel a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-112">This protocol is set in stone and will not be experiencing any changes.</span></span>

<span data-ttu-id="5dbbf-113">Továbbá ha az alkalmazás **csak** az ADAL-könyvtár tooperform hitelesítést használ, toochange semmit nem kell.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-113">Furthermore, if your app **only** uses our ADAL library tooperform authentication, you won\`t have toochange anything.</span></span>  <span data-ttu-id="5dbbf-114">ADAL rendelkezik védett hello módosításokat az alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-114">ADAL has shielded your app from hello changes.</span></span>  

## <a name="what-are-hello-changes"></a><span data-ttu-id="5dbbf-115">Mik azok a hello módosításait?</span><span class="sxs-lookup"><span data-stu-id="5dbbf-115">What are hello changes?</span></span>
### <a name="removing-hello-x5t-value-from-jwt-headers"></a><span data-ttu-id="5dbbf-116">Hello x5t érték eltávolítása a JWT fejlécek</span><span class="sxs-lookup"><span data-stu-id="5dbbf-116">Removing hello x5t value from JWT headers</span></span>
<span data-ttu-id="5dbbf-117">hello v2.0-végponttól használ JWT-jogkivonatokat széles körben, tartalmazó kapcsolódó metaadatokkal kapcsolatos hello token paraméterek fejlécszakasza.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-117">hello v2.0 endpoint uses JWT tokens extensively, which contain a header parameters section with relevant metadata about hello token.</span></span>  <span data-ttu-id="5dbbf-118">Ha dekódolása hello fejléc az aktuális JWTs közül az egyik, találjuk hasonlót:</span><span class="sxs-lookup"><span data-stu-id="5dbbf-118">If you decode hello header of one of our current JWTs, you would find something like:</span></span>

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

<span data-ttu-id="5dbbf-119">Ha mindkét hello "x5t" és "kid" Tulajdonságok hello nyilvános kulcsának azonosítása, amely kell használt toovalidate hello jogkivonat aláírása, lekért hello OpenID Connect metaadat-végpontjához.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-119">Where both hello "x5t" and "kid" properties identify hello public key that should be used toovalidate hello token\`s signature, as retrieved from hello OpenID Connect metadata endpoint.</span></span>

<span data-ttu-id="5dbbf-120">Itt hajtunk hello módosítása tooremove hello "x5t" tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-120">hello change we are making here is tooremove hello "x5t" property.</span></span>  <span data-ttu-id="5dbbf-121">Azonos mechanizmusok toovalidate jogkivonatokat, de a szoftverlicenceknek csak hello "kid" tulajdonság tooretrieve hello megfelelő nyilvános kulccsal, mint a megadott hello OpenID Connect protokoll toouse hello folytathatja.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-121">You may continue toouse hello same mechanisms toovalidate tokens, but should rely only on hello "kid" property tooretrieve hello correct public key, as specified in hello OpenID Connect protocol.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="5dbbf-122">**A feladat: Győződjön meg arról, hogy az alkalmazás nem függ a hello x5t érték hello megléte.**</span><span class="sxs-lookup"><span data-stu-id="5dbbf-122">**Your job: Make sure your app does not depend on hello existence of hello x5t value.**</span></span>
> 
> 

### <a name="removing-profileinfo"></a><span data-ttu-id="5dbbf-123">Profile_info eltávolítása</span><span class="sxs-lookup"><span data-stu-id="5dbbf-123">Removing profile_info</span></span>
<span data-ttu-id="5dbbf-124">Korábban, hello v2.0-végponttal rendelkezik lett visszaadni a base64-kódolású JSON-objektum nevű token válaszok `profile_info`.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-124">Previously, hello v2.0 endpoint has been returning a base64 encoded JSON object in token responses called `profile_info`.</span></span>  <span data-ttu-id="5dbbf-125">Amikor olyan hozzáférési jogkivonatot kérnek hello v2.0-végpontra vonatkozó kérelmet küld:</span><span class="sxs-lookup"><span data-stu-id="5dbbf-125">When requesting an access token from hello v2.0 endpoint by sending a request to:</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

<span data-ttu-id="5dbbf-126">hello válasz JSON-objektum a következő hello jelenne meg:</span><span class="sxs-lookup"><span data-stu-id="5dbbf-126">hello response would look like hello following JSON object:</span></span>

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

<span data-ttu-id="5dbbf-127">Hello `profile_info` hello hello alkalmazás – a megjelenített név, Utónév, Vezetéknév, e-mail cím, azonosítója, és így tovább a bejelentkezett felhasználó értéket tartalmaz információt.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-127">hello `profile_info` value contained information about hello user who signed into hello app - their display name, first name, surname, email address, identifier, and so on.</span></span>  <span data-ttu-id="5dbbf-128">Elsősorban, hello `profile_info` token-gyorsítótárazási volt használva, és megjeleníti a célra.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-128">Primarily, hello `profile_info` was used for token caching and display purposes.</span></span>

<span data-ttu-id="5dbbf-129">Most megszüntetjük hello `profile_info` érték –, de ne aggódjon, Microsoft továbbra is ad az információk toodevelopers némileg eltérő helyen.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-129">We are now removing hello `profile_info` value – but don't worry, we are still providing this information toodevelopers in a slightly different place.</span></span>  <span data-ttu-id="5dbbf-130">Ahelyett, hogy `profile_info`, hello v2.0-végponttól most visszaállítja egy `id_token` minden token válaszként:</span><span class="sxs-lookup"><span data-stu-id="5dbbf-130">Instead of `profile_info`, hello v2.0 endpoint will now return an `id_token` in each token response:</span></span>

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

<span data-ttu-id="5dbbf-131">Előfordulhat, hogy dekódolása és elemezni a hello id_token tooretrieve hello profile_info kapott adatokat.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-131">You may decode and parse hello id_token tooretrieve hello same information that you received from profile_info.</span></span>  <span data-ttu-id="5dbbf-132">hello id_token egy JSON webes jogkivonat (JWT), az OpenID Connect által megadott tartalom.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-132">hello id_token is a JSON Web Token (JWT), with contents as specified by OpenID Connect.</span></span>  <span data-ttu-id="5dbbf-133">hello kódot úgy legyen nagyon hasonló – tooextract hello közel egyszerűen szegmens (hello törzs) hello id_token és base64 dekódolni a tooaccess hello JSON-objektum belül.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-133">hello code for doing so should be very similar – you simply need tooextract hello middle segment (hello body) of hello id_token and base64 decode it tooaccess hello JSON object within.</span></span>

<span data-ttu-id="5dbbf-134">Hello keresztül következő két héten kell kódaláírással az alkalmazás tooretrieve hello felhasználói adatok vagy hello `id_token` vagy `profile_info`; attól jelen.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-134">Over hello next two weeks, you should code your app tooretrieve hello user information from either hello `id_token` or `profile_info`; whichever is present.</span></span>  <span data-ttu-id="5dbbf-135">Ily módon módosításkor hello az alkalmazás zökkenőmentesen kezelni tud a hello áttűnés `profile_info` túl`id_token` megszakítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-135">That way when hello change is made, your app can seamlessly handle hello transition from `profile_info` too`id_token` without interruption.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5dbbf-136">**A feladat: Győződjön meg arról, hogy az alkalmazás nem függ a hello hello megléte `profile_info` érték.**</span><span class="sxs-lookup"><span data-stu-id="5dbbf-136">**Your job: Make sure your app does not depend on hello existence of hello `profile_info` value.**</span></span>
> 
> 

### <a name="removing-idtokenexpiresin"></a><span data-ttu-id="5dbbf-137">Id_token_expires_in eltávolítása</span><span class="sxs-lookup"><span data-stu-id="5dbbf-137">Removing id_token_expires_in</span></span>
<span data-ttu-id="5dbbf-138">Hasonló túl`profile_info`, is megszüntetjük hello `id_token_expires_in` válaszok paraméter.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-138">Similar too`profile_info`, we are also removing hello `id_token_expires_in` parameter from responses.</span></span>  <span data-ttu-id="5dbbf-139">Korábban, a hello v2.0-végpontra vonatkozó értéket visszaadnia `id_token_expires_in` minden egyes id_token válasz, például egy engedélyezés válaszul együtt:</span><span class="sxs-lookup"><span data-stu-id="5dbbf-139">Previously, hello v2.0 endpoint would return a value for `id_token_expires_in` along with each id_token response, for instance in an authorize response:</span></span>

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

<span data-ttu-id="5dbbf-140">Vagy a token válasz:</span><span class="sxs-lookup"><span data-stu-id="5dbbf-140">Or in a token response:</span></span>

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

<span data-ttu-id="5dbbf-141">Hello `id_token_expires_in` érték azt jelentené hello hány másodpercig hello id_token érvényes marad.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-141">hello `id_token_expires_in` value would indicate hello number of seconds hello id_token would remain valid for.</span></span>  <span data-ttu-id="5dbbf-142">Most, megszüntetjük hello `id_token_expires_in` teljesen érték.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-142">Now, we are removing hello `id_token_expires_in` value completely.</span></span>  <span data-ttu-id="5dbbf-143">Ehelyett használhat hello OpenID Connect standard `nbf` és `exp` jogcímek egy id_token tooexamine hello érvényességét.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-143">Instead, you may use hello OpenID Connect standard `nbf` and `exp` claims tooexamine hello validity of an id_token.</span></span>  <span data-ttu-id="5dbbf-144">Lásd: hello [v2.0 jogkivonat referenciái](active-directory-v2-tokens.md) jogcímek olvashat.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-144">See hello [v2.0 token reference](active-directory-v2-tokens.md) for more information on these claims.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5dbbf-145">**A feladat: Győződjön meg arról, hogy az alkalmazás nem függ a hello hello megléte `id_token_expires_in` érték.**</span><span class="sxs-lookup"><span data-stu-id="5dbbf-145">**Your job: Make sure your app does not depend on hello existence of hello `id_token_expires_in` value.**</span></span>
> 
> 

### <a name="changing-hello-claims-returned-by-scopeopenid"></a><span data-ttu-id="5dbbf-146">Hatókör által visszaadott hello jogcímek módosítása = openid</span><span class="sxs-lookup"><span data-stu-id="5dbbf-146">Changing hello claims returned by scope=openid</span></span>
<span data-ttu-id="5dbbf-147">Ez a változás legjelentősebb – hello ténylegesen, az hatással szinte minden hello v2.0-végponttól alkalmazó alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-147">This change will be hello most significant – in fact, it will affect almost every app that uses hello v2.0 endpoint.</span></span>  <span data-ttu-id="5dbbf-148">Sok alkalmazások küldési kérelmek toohello v2.0-végponttól hello segítségével `openid` hatókörét, például:</span><span class="sxs-lookup"><span data-stu-id="5dbbf-148">Many applications send requests toohello v2.0 endpoint using hello `openid` scope, like:</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

<span data-ttu-id="5dbbf-149">Ma, ha engedélyezi a hello felhasználói a hello kapcsolatos jóváhagyásról `openid` hatókör, az alkalmazás fogadása számos olyan információt hello felhasználóról id_token eredő hello.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-149">Today, when hello user grants consent for hello `openid` scope, your app receives a wealth of information about hello user in hello resulting id_token.</span></span>  <span data-ttu-id="5dbbf-150">Ezek a jogcímek tartalmazhatják a neve, elsődleges felhasználónév, e-mail címét, objektumazonosító: és több.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-150">These claims can include their name, preferred username, email address, object ID, and more.</span></span>

<span data-ttu-id="5dbbf-151">Ez a frissítés, azt módosítani hello információt, hogy hello `openid` hatókör számára biztosítja a alkalmazást a hozzáférést, az OpenID Connect specification hello toobetter comform.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-151">In this update, we are changing hello information that hello `openid` scope affords your app access to, toobetter comform with hello OpenID Connect specification.</span></span>  <span data-ttu-id="5dbbf-152">Hello `openid` hatókör fog csak felhasználónak lehetővé teszi az alkalmazás toosign hello és hello felhasználó alkalmazásspecifikus azonosítót kap, hello `sub` hello id_token minősítése.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-152">hello `openid` scope will only allow your app toosign hello user in, and receive an app-specific identifier for hello user in hello `sub` claim of hello id_token.</span></span>  <span data-ttu-id="5dbbf-153">jogcím szerepel egy id_token hello rendelkező csak hello `openid` nyújtott lesz nem tartalmaz személyazonosításra alkalmas adatok.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-153">hello claims in an id_token with only hello `openid` scope granted will be devoid of any personally identifiable information.</span></span>  <span data-ttu-id="5dbbf-154">Példa id_token jogcímeket a következők:</span><span class="sxs-lookup"><span data-stu-id="5dbbf-154">Example id_token claims are:</span></span>

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

<span data-ttu-id="5dbbf-155">Tooobtain személyes azonosításra alkalmas adatokat (PII) hello felhasználóról az alkalmazásban, az alkalmazás fogja kell hello felhasználói toorequest további engedélyek.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-155">If you want tooobtain personally identifiable information (PII) about hello user in your app, your app will need toorequest additional permissions from hello user.</span></span>  <span data-ttu-id="5dbbf-156">Vezetjük be két új hatókört támogatása a hello OpenID Connect spec – hello `email` és `profile` hatókörök – amelyek toodo így.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-156">We are introducing support for two new scopes from hello OpenID Connect spec – hello `email` and `profile` scopes – which allow you toodo so.</span></span>

* <span data-ttu-id="5dbbf-157">Hello `email` hatóköre nagyon egyszerű – lehetővé teszi az alkalmazás hozzáférési toohello felhasználó elsődleges e-mail címét keresztül hello `email` hello id_token a jogcímek.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-157">hello `email` scope is very straightforward – it allows your app access toohello user's primary email address via hello `email` claim in hello id_token.</span></span>  <span data-ttu-id="5dbbf-158">Vegye figyelembe, hogy hello `email` jogcím nem mindig kerül be a id_tokens – csak akkor lesz része Ha hello profil érhető el.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-158">Note that hello `email` claim will not always be present in id_tokens – it will only be included if available in hello user's profile.</span></span>
* <span data-ttu-id="5dbbf-159">Hello `profile` objektumazonosító hatókör számára biztosítja az alkalmazás hozzáférési tooall más hello felhasználó – a neve, az elsődleges felhasználónév alapvető információkat, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-159">hello `profile` scope affords your app access tooall other basic information about hello user – their name, preferred username, object ID, and so on.</span></span>

<span data-ttu-id="5dbbf-160">Ez lehetővé teszi, hogy toocode minimális nyilvánosságra módon – az alkalmazás megkérheti hello felhasználói adatokat, hogy az alkalmazás futtatásához szükséges toodo feladata elvégzéséhez egyszerűen hello készlete esetében.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-160">This allows you toocode your app in a minimal-disclosure fashion – you can ask hello user for just hello set of information that your app requires toodo its job.</span></span>  <span data-ttu-id="5dbbf-161">Ha azt szeretné, hogy az alkalmazás jelenleg fogadó felhasználói adatok teljes készletét hello első toocontinue, a hitelesítési kérések bele kell foglalni a három hatóköröket:</span><span class="sxs-lookup"><span data-stu-id="5dbbf-161">If you want toocontinue getting hello full set of user information that your app is currently receiving, you should include all three scopes in your authorization requests:</span></span>

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

<span data-ttu-id="5dbbf-162">Az alkalmazás megkezdődhet az hello küldése `email` és `profile` azonnal hatókörök és hello v2.0-végponttól elfogadja a két hatóköröket és megkezdéséhez engedélyek kér a felhasználóktól, szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-162">Your app can begin sending hello `email` and `profile` scopes immediately, and hello v2.0 endpoint will accept these two scopes and begin requesting permissions from users as necessary.</span></span>  <span data-ttu-id="5dbbf-163">Azonban a hello hello hello értelmezésének változása `openid` hatókör nem lépnek érvénybe néhány hétig.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-163">However, hello change in hello interpretation of hello `openid` scope will not take effect for a few weeks.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5dbbf-164">**A feladat: hello hozzáadása `profile` és `email` hatóköröket, ha az alkalmazás használatához hello felhasználó adatait.**</span><span class="sxs-lookup"><span data-stu-id="5dbbf-164">**Your job: Add hello `profile` and `email` scopes if your app requires information about hello user.**</span></span>  <span data-ttu-id="5dbbf-165">Vegye figyelembe, hogy adal-t fog tartalmazni mindkét engedéllyel a kérelmek alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-165">Note that ADAL will include both of these permissions in requests by default.</span></span> 
> 
> 

### <a name="removing-hello-issuer-trailing-slash"></a><span data-ttu-id="5dbbf-166">Hello kibocsátó záró perjelet eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-166">Removing hello issuer trailing slash.</span></span>
<span data-ttu-id="5dbbf-167">Korábban a v2.0-végponttól hello jogkivonatok megjelenő hello kibocsátó érték tartott hello képernyő</span><span class="sxs-lookup"><span data-stu-id="5dbbf-167">Previously, hello issuer value that appears in tokens from hello v2.0 endpoint took hello form</span></span>

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

<span data-ttu-id="5dbbf-168">Ha hello guid lett hello tenantId hello jogkivonatot kibocsátó hello Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-168">Where hello guid was hello tenantId of hello Azure AD tenant which issued hello token.</span></span>  <span data-ttu-id="5dbbf-169">A módosítások hello kibocsátó érték válik.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-169">With these changes, hello issuer value becomes</span></span>

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

<span data-ttu-id="5dbbf-170">mindkét jogkivonatokat, hello OpenID Connect felderítési dokumentumban.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-170">in both tokens and in hello OpenID Connect discovery document.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5dbbf-171">**A feladat: Győződjön meg arról, hogy az alkalmazás fogad hello kibocsátó érték vagy záró perjelet anélkül kibocsátó érvényesítése során.**</span><span class="sxs-lookup"><span data-stu-id="5dbbf-171">**Your job: Make sure your app accepts hello issuer value both with and without a trailing slash during issuer validation.**</span></span>
> 
> 

## <a name="why-change"></a><span data-ttu-id="5dbbf-172">Miért is megváltozik?</span><span class="sxs-lookup"><span data-stu-id="5dbbf-172">Why change?</span></span>
<span data-ttu-id="5dbbf-173">hello elsődleges kifejlesztésének vezet be, ezeket a módosításokat az OpenID Connect szabvány hello megfelelő toobe.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-173">hello primary motivation for introducing these changes is toobe compliant with hello OpenID Connect standard specification.</span></span>  <span data-ttu-id="5dbbf-174">Mivel OpenID Connect megfelelő, a Microsoft identity szolgáltatásokkal és az egyéb identitás-szolgáltatások hello iparági integrálásával toominimize különbségei Reméljük.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-174">By being OpenID Connect compliant, we hope toominimize differences between integrating with Microsoft identity services and with other identity services in hello industry.</span></span>  <span data-ttu-id="5dbbf-175">Szeretnénk tooenable fejlesztők toouse a kedvenc nyílt forráskódú hitelesítési tárakat tooalter hello szalagtárak tooaccommodate Microsoft különbségek nélkül.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-175">We want tooenable developers toouse their favorite open source authentication libraries without having tooalter hello libraries tooaccommodate Microsoft differences.</span></span>

## <a name="what-can-you-do"></a><span data-ttu-id="5dbbf-176">Mire szolgál?</span><span class="sxs-lookup"><span data-stu-id="5dbbf-176">What can you do?</span></span>
<span data-ttu-id="5dbbf-177">Mai megkezdheti a fent leírt hello végrehajtott módosítások elvégzése.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-177">As of today, you can begin making all of hello changes described above.</span></span>  <span data-ttu-id="5dbbf-178">Azonnal kell:</span><span class="sxs-lookup"><span data-stu-id="5dbbf-178">You should immediately:</span></span>

1. <span data-ttu-id="5dbbf-179">**Távolítsa el az összes függőségét hello `x5t` fejléc paraméter.**</span><span class="sxs-lookup"><span data-stu-id="5dbbf-179">**Remove any dependencies on hello `x5t` header parameter.**</span></span>
2. <span data-ttu-id="5dbbf-180">**A hello áttűnés kezelésére `profile_info` túl`id_token` token válaszokban.**</span><span class="sxs-lookup"><span data-stu-id="5dbbf-180">**Gracefully handle hello transition from `profile_info` too`id_token` in token responses.**</span></span>
3. <span data-ttu-id="5dbbf-181">**Távolítsa el az összes függőségét hello `id_token_expires_in` válasz paraméter.**</span><span class="sxs-lookup"><span data-stu-id="5dbbf-181">**Remove any dependencies on hello `id_token_expires_in` response parameter.**</span></span>
4. <span data-ttu-id="5dbbf-182">**Adja hozzá a hello `profile` és `email` hatókörök tooyour alkalmazást, ha az alkalmazás alapszintű felhasználói információra van szüksége.**</span><span class="sxs-lookup"><span data-stu-id="5dbbf-182">**Add hello `profile` and `email` scopes tooyour app if your app needs basic user information.**</span></span>
5. <span data-ttu-id="5dbbf-183">**Fogadja el vagy záró perjelet anélkül jogkivonatokba kibocsátó értékeket.**</span><span class="sxs-lookup"><span data-stu-id="5dbbf-183">**Accept issuer values in tokens both with and without a trailing slash.**</span></span>

<span data-ttu-id="5dbbf-184">A [v2.0 protokoll dokumentációját](active-directory-v2-protocols.md) már be van frissített tooreflect ezeket a változásokat, így használhatja referenciaként létrehozásában, frissítse a kódot.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-184">Our [v2.0 protocol documentation](active-directory-v2-protocols.md) has already been updated tooreflect these changes, so you may use it as reference in helping update your code.</span></span>

<span data-ttu-id="5dbbf-185">Ha hello hatókörön hello változások további kérdése van, adjon érzi, hogy szabad tooreach toous a Twitteren, out @AzureAD.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-185">If you have any further questions on hello scope of hello changes, please feel free tooreach out toous on Twitter at @AzureAD.</span></span>

## <a name="how-often-will-protocol-changes-occur"></a><span data-ttu-id="5dbbf-186">Milyen gyakran történik a protokoll módosításait?</span><span class="sxs-lookup"><span data-stu-id="5dbbf-186">How often will protocol changes occur?</span></span>
<span data-ttu-id="5dbbf-187">Bármely további megtörje változik toohello hitelesítési protokollok nem várható.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-187">We do not foresee any further breaking changes toohello authentication protocols.</span></span>  <span data-ttu-id="5dbbf-188">Ezeket a módosításokat, azokat egy kiadási szándékosan azt vannak kötegelése, így nem kell a frissítési folyamat az ilyen típusú toogo újra bármikor elérhető.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-188">We are intentionally bundling these changes into one release so that you won\`t have toogo through this type of update process again any time soon.</span></span>  <span data-ttu-id="5dbbf-189">Természetesen folytatjuk tooadd szolgáltatások toohello v2.0 hitelesítési szolgáltatás, amely kihasználhatja az összevont van, de ezeket a módosításokat adalékanyag és nem a meglévő kódot sortörés.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-189">Of course, we will continue tooadd features toohello converged v2.0 authentication service that you can take advantage of, but those changes should be additive and not break existing code.</span></span>

<span data-ttu-id="5dbbf-190">Végül tapasztalatairól toosay Köszönjük, hogy dolgot kipróbálásánál hello próbaidőszak alatt.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-190">Lastly, we would like toosay thank you for trying things out during hello preview period.</span></span>  <span data-ttu-id="5dbbf-191">hello elemzések és a korai támogatók véleményeket hasznos információt eddigi, és Reméljük, hogy a vélemények és ötleteket tooshare fogja folytatni.</span><span class="sxs-lookup"><span data-stu-id="5dbbf-191">hello insights and experiences of our early adopters have been invaluable thus far, and we hope you\`ll continue tooshare your opinions and ideas.</span></span>

<span data-ttu-id="5dbbf-192">Örömmel kódolási!</span><span class="sxs-lookup"><span data-stu-id="5dbbf-192">Happy coding!</span></span>

<span data-ttu-id="5dbbf-193">a Microsoft Identity osztás hello</span><span class="sxs-lookup"><span data-stu-id="5dbbf-193">hello Microsoft Identity Division</span></span>

