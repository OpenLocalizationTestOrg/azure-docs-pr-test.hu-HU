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
# <a name="azure-ad-b2c-add-sign-in-tooa-nodejs-web-app"></a><span data-ttu-id="6bb44-103">Az Azure AD B2C: Bejelentkezési tooa Node.js webalkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6bb44-103">Azure AD B2C: Add sign-in tooa Node.js web app</span></span>

<span data-ttu-id="6bb44-104">A **Passport** a Node.js-hez készült közbenső hitelesítési szoftver.</span><span class="sxs-lookup"><span data-stu-id="6bb44-104">**Passport** is authentication middleware for Node.js.</span></span> <span data-ttu-id="6bb44-105">A rendkívül rugalmasan működő, moduláris Passport gyakorlatilag bármely Express- vagy Restify-alapú webalkalmazásba diszkréten telepíthető.</span><span class="sxs-lookup"><span data-stu-id="6bb44-105">Extremely flexible and modular, Passport can be unobtrusively installed in any Express-based or Restify web application.</span></span> <span data-ttu-id="6bb44-106">A program számos különböző lehetőséget kínál a felhasználók hitelesítésére: felhasználónév/jelszó, Facebook- vagy Twitter-fiók és így tovább.</span><span class="sxs-lookup"><span data-stu-id="6bb44-106">A comprehensive set of strategies supports authentication by using a user name and password, Facebook, Twitter, and more.</span></span>

<span data-ttu-id="6bb44-107">Kidolgoztunk egy stratégiát, amellyel a szoftver az Azure Active Directory (Azure AD) esetében is felhasználható.</span><span class="sxs-lookup"><span data-stu-id="6bb44-107">We have developed a strategy for Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="6bb44-108">Először telepítenie kell a modult, majd adja hozzá az Azure AD hello `passport-azure-ad` beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="6bb44-108">You will install this module and then add hello Azure AD `passport-azure-ad` plug-in.</span></span>

<span data-ttu-id="6bb44-109">toodo, kell:</span><span class="sxs-lookup"><span data-stu-id="6bb44-109">toodo this, you need to:</span></span>

1. <span data-ttu-id="6bb44-110">Alkalmazás regisztrálása az Azure AD használatával.</span><span class="sxs-lookup"><span data-stu-id="6bb44-110">Register an application by using Azure AD.</span></span>
2. <span data-ttu-id="6bb44-111">Állítsa be az alkalmazás toouse hello `passport-azure-ad` beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="6bb44-111">Set up your app toouse hello `passport-azure-ad` plug-in.</span></span>
3. <span data-ttu-id="6bb44-112">A Passport tooissue bejelentkezési és kijelentkezési kérések tooAzure AD használatára.</span><span class="sxs-lookup"><span data-stu-id="6bb44-112">Use Passport tooissue sign-in and sign-out requests tooAzure AD.</span></span>
4. <span data-ttu-id="6bb44-113">Felhasználói adatok kinyomtatása.</span><span class="sxs-lookup"><span data-stu-id="6bb44-113">Print user data.</span></span>

<span data-ttu-id="6bb44-114">Ez az oktatóanyag kód hello [kezelése a Githubon történik](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS).</span><span class="sxs-lookup"><span data-stu-id="6bb44-114">hello code for this tutorial [is maintained on GitHub](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS).</span></span> <span data-ttu-id="6bb44-115">toofollow mellett, akkor [hello alkalmazás vázát .zip fájl letöltése](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip).</span><span class="sxs-lookup"><span data-stu-id="6bb44-115">toofollow along, you can [download hello app's skeleton as a .zip file](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/skeleton.zip).</span></span> <span data-ttu-id="6bb44-116">Hello vázat klónozhatja is:</span><span class="sxs-lookup"><span data-stu-id="6bb44-116">You can also clone hello skeleton:</span></span>

```git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS.git```

<span data-ttu-id="6bb44-117">Ez az oktatóanyag végén hello befejeződött hello alkalmazás valósul meg.</span><span class="sxs-lookup"><span data-stu-id="6bb44-117">hello completed application is provided at hello end of this tutorial.</span></span>

## <a name="get-an-azure-ad-b2c-directory"></a><span data-ttu-id="6bb44-118">Az Azure AD B2C-címtár beszerzése</span><span class="sxs-lookup"><span data-stu-id="6bb44-118">Get an Azure AD B2C directory</span></span>

<span data-ttu-id="6bb44-119">Az Azure AD B2C használatához létre kell hoznia egy címtárat vagy bérlőt.</span><span class="sxs-lookup"><span data-stu-id="6bb44-119">Before you can use Azure AD B2C, you must create a directory, or tenant.</span></span>  <span data-ttu-id="6bb44-120">A címtárban tárolhatja az összes felhasználót, alkalmazást, csoportot és sok minden mást.</span><span class="sxs-lookup"><span data-stu-id="6bb44-120">A directory is a container for all of your users, apps, groups, and more.</span></span> <span data-ttu-id="6bb44-121">Ha még nem tette meg, [hozzon létre most egy B2C-címtárat](active-directory-b2c-get-started.md), mielőtt továbblépne ebben az útmutatóban.</span><span class="sxs-lookup"><span data-stu-id="6bb44-121">If you don't have one already, [create a B2C directory](active-directory-b2c-get-started.md) before you continue in this guide.</span></span>

## <a name="create-an-application"></a><span data-ttu-id="6bb44-122">Alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6bb44-122">Create an application</span></span>

<span data-ttu-id="6bb44-123">A következő lépésben toocreate egy alkalmazást a B2C-címtárban.</span><span class="sxs-lookup"><span data-stu-id="6bb44-123">Next, you need toocreate an app in your B2C directory.</span></span> <span data-ttu-id="6bb44-124">Ez biztosítja, hogy kell-e az alkalmazással biztonságosan toocommunicate az Azure AD-információkat.</span><span class="sxs-lookup"><span data-stu-id="6bb44-124">This gives Azure AD information that it needs toocommunicate securely with your app.</span></span> <span data-ttu-id="6bb44-125">Mindkét hello ügyfélalkalmazás és webes API-t fog megjelenni, egyetlen **Alkalmazásazonosító**, mert alkalmazássá logikai.</span><span class="sxs-lookup"><span data-stu-id="6bb44-125">Both hello client app and web API will be represented by a single **Application ID**, because they comprise one logical app.</span></span> <span data-ttu-id="6bb44-126">hajtsa végre az alkalmazást, és egy toocreate [ezeket az utasításokat](active-directory-b2c-app-registration.md).</span><span class="sxs-lookup"><span data-stu-id="6bb44-126">toocreate an app, follow [these instructions](active-directory-b2c-app-registration.md).</span></span> <span data-ttu-id="6bb44-127">Ügyeljen arra, hogy:</span><span class="sxs-lookup"><span data-stu-id="6bb44-127">Be sure to:</span></span>

- <span data-ttu-id="6bb44-128">Tartalmaznak egy **webalkalmazás**/**webes API-t** hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="6bb44-128">Include a **web app**/**web API** in hello application.</span></span>
- <span data-ttu-id="6bb44-129">A **Reply URL** (Válasz URL-cím) legyen a következő: `http://localhost:3000/auth/openid/return`.</span><span class="sxs-lookup"><span data-stu-id="6bb44-129">Enter `http://localhost:3000/auth/openid/return` as a **Reply URL**.</span></span> <span data-ttu-id="6bb44-130">A fenti hello alapértelmezett URL-.</span><span class="sxs-lookup"><span data-stu-id="6bb44-130">It is hello default URL for this code sample.</span></span>
- <span data-ttu-id="6bb44-131">Hozzon létre egy **alkalmazástitkot** az alkalmazáshoz, majd másolja.</span><span class="sxs-lookup"><span data-stu-id="6bb44-131">Create an **Application secret** for your application and copy it.</span></span> <span data-ttu-id="6bb44-132">Erre később még szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="6bb44-132">You will need it later.</span></span> <span data-ttu-id="6bb44-133">Vegye figyelembe, hogy ezt az értéket kell toobe [escape-karaktersorozatot XML](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) használatba vétel előtt.</span><span class="sxs-lookup"><span data-stu-id="6bb44-133">Note that this value needs toobe [XML escaped](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) before you use it.</span></span>
- <span data-ttu-id="6bb44-134">Másolás hello **Alkalmazásazonosító** , amely hozzárendelt tooyour alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6bb44-134">Copy hello **Application ID** that is assigned tooyour app.</span></span> <span data-ttu-id="6bb44-135">Később erre is szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="6bb44-135">You'll also need this later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a><span data-ttu-id="6bb44-136">Szabályzatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="6bb44-136">Create your policies</span></span>

<span data-ttu-id="6bb44-137">Az Azure AD B2C-ben a felhasználói élményeket [szabályzatok](active-directory-b2c-reference-policies.md) határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="6bb44-137">In Azure AD B2C, every user experience is defined by a [policy](active-directory-b2c-reference-policies.md).</span></span> <span data-ttu-id="6bb44-138">Az alkalmazás három különböző, identitással kapcsolatos műveletet tartalmaz: regisztráció, bejelentkezés és bejelentkezés Facebook-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="6bb44-138">This app contains three identity experiences: sign up, sign in, and sign in by using Facebook.</span></span> <span data-ttu-id="6bb44-139">Szüksége toocreate ezt a házirendet az egyes típusok leírtak szerint a [házirendek áttekintésével foglalkozó cikkben](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span><span class="sxs-lookup"><span data-stu-id="6bb44-139">You need toocreate this policy of each type, as described in the [policy reference article](active-directory-b2c-reference-policies.md#create-a-sign-up-policy).</span></span> <span data-ttu-id="6bb44-140">A három szabályzat létrehozásakor ügyeljen arra, hogy:</span><span class="sxs-lookup"><span data-stu-id="6bb44-140">When you create your three policies, be sure to:</span></span>

- <span data-ttu-id="6bb44-141">Válassza ki a hello **megjelenített név** és az egyéb regisztrációs attribútumokat a regisztrációs szabályzatban.</span><span class="sxs-lookup"><span data-stu-id="6bb44-141">Choose hello **Display name** and other sign-up attributes in your sign-up policy.</span></span>
- <span data-ttu-id="6bb44-142">Válassza ki a hello **megjelenített név** és **Objektumazonosító** minden házirend alkalmazási jogcímet.</span><span class="sxs-lookup"><span data-stu-id="6bb44-142">Choose hello **Display name** and **Object ID** application claims in every policy.</span></span> <span data-ttu-id="6bb44-143">Kiválaszthat egyéb jogcímeket is.</span><span class="sxs-lookup"><span data-stu-id="6bb44-143">You can choose other claims as well.</span></span>
- <span data-ttu-id="6bb44-144">Másolás hello **neve** egyes házirendek létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="6bb44-144">Copy hello **Name** of each policy after you create it.</span></span> <span data-ttu-id="6bb44-145">Hello előtag nem rendelkezhet `b2c_1_`.</span><span class="sxs-lookup"><span data-stu-id="6bb44-145">It should have hello prefix `b2c_1_`.</span></span>  <span data-ttu-id="6bb44-146">A házirendek nevére később még szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="6bb44-146">You'll need these policy names later.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

<span data-ttu-id="6bb44-147">A három szabályzat létrehozását követően készen áll a toobuild Ön az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6bb44-147">After you create your three policies, you're ready toobuild your app.</span></span>

<span data-ttu-id="6bb44-148">Vegye figyelembe, hogy a cikk nem tér ki hogyan toouse hello házirendek újonnan létrehozott.</span><span class="sxs-lookup"><span data-stu-id="6bb44-148">Note that this article does not cover how toouse hello policies you just created.</span></span> <span data-ttu-id="6bb44-149">toolearn arról, hogyan házirendek az Azure AD B2C működnek, hello kezdődnie [.NET webalkalmazások használatába bevezető oktatóanyagot](active-directory-b2c-devquickstarts-web-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="6bb44-149">toolearn about how policies work in Azure AD B2C, start with hello [.NET web app getting started tutorial](active-directory-b2c-devquickstarts-web-dotnet.md).</span></span>

## <a name="add-prerequisites-tooyour-directory"></a><span data-ttu-id="6bb44-150">Előfeltételek tooyour könyvtár hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6bb44-150">Add prerequisites tooyour directory</span></span>

<span data-ttu-id="6bb44-151">Hello parancssorból módosítsa könyvtárak tooyour gyökérmappára, ha már nem létezik.</span><span class="sxs-lookup"><span data-stu-id="6bb44-151">From hello command line, change directories tooyour root folder, if you're not already there.</span></span> <span data-ttu-id="6bb44-152">Futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="6bb44-152">Run hello following commands:</span></span>

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

<span data-ttu-id="6bb44-153">Ezenkívül használtuk `passport-azure-ad` a a gyors üzembe helyezés hello hello vázban szereplő előzetes verzióban.</span><span class="sxs-lookup"><span data-stu-id="6bb44-153">In addition, we used `passport-azure-ad` for our preview in hello skeleton of hello Quickstart.</span></span>

- `npm install passport-azure-ad`

<span data-ttu-id="6bb44-154">Ez a hello szalagtárak telepíti, amely `passport-azure-ad` függ.</span><span class="sxs-lookup"><span data-stu-id="6bb44-154">This will install hello libraries that `passport-azure-ad` depends on.</span></span>

## <a name="set-up-your-app-toouse-hello-passport-nodejs-strategy"></a><span data-ttu-id="6bb44-155">Állítsa be az alkalmazás toouse hello Passport-Node.js stratégia</span><span class="sxs-lookup"><span data-stu-id="6bb44-155">Set up your app toouse hello Passport-Node.js strategy</span></span>
<span data-ttu-id="6bb44-156">Konfigurálja a hello Express köztes toouse hello OpenID Connect hitelesítési protokoll.</span><span class="sxs-lookup"><span data-stu-id="6bb44-156">Configure hello Express middleware toouse hello OpenID Connect authentication protocol.</span></span> <span data-ttu-id="6bb44-157">Passport kell használt tooissue be- és kijelentkezési kéréseket, kezelni a felhasználói munkameneteket, és többek között a felhasználók adatainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="6bb44-157">Passport will be used tooissue sign-in and sign-out requests, manage user sessions, and get information about users, among other actions.</span></span>

<span data-ttu-id="6bb44-158">Nyissa meg hello `config.js` hello projekt gyökerében hello fájlt, és adja meg az alkalmazás konfigurációs értékeit hello `exports.creds` szakasz.</span><span class="sxs-lookup"><span data-stu-id="6bb44-158">Open hello `config.js` file in hello root of hello project and enter your app's configuration values in hello `exports.creds` section.</span></span>
- <span data-ttu-id="6bb44-159">`clientID`: hello **Alkalmazásazonosító** tooyour app hello regisztrációs portálon rendelt.</span><span class="sxs-lookup"><span data-stu-id="6bb44-159">`clientID`: hello **Application ID** assigned tooyour app in hello registration portal.</span></span>
- <span data-ttu-id="6bb44-160">`returnURL`: hello **átirányítási URI-** hello portálon megadott.</span><span class="sxs-lookup"><span data-stu-id="6bb44-160">`returnURL`: hello **Redirect URI** you entered in hello portal.</span></span>
- <span data-ttu-id="6bb44-161">`tenantName`: hello bérlő nevét az alkalmazás, például **contoso.onmicrosoft.com**.</span><span class="sxs-lookup"><span data-stu-id="6bb44-161">`tenantName`: hello tenant name of your app, for example, **contoso.onmicrosoft.com**.</span></span>

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

<span data-ttu-id="6bb44-162">Nyissa meg hello `app.js` hello hello projekt gyökerében található fájl.</span><span class="sxs-lookup"><span data-stu-id="6bb44-162">Open hello `app.js` file in hello root of hello project.</span></span> <span data-ttu-id="6bb44-163">Adja hozzá a következő hívást tooinvoke hello hello `OIDCStrategy` stratégia `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="6bb44-163">Add hello following call tooinvoke hello `OIDCStrategy` strategy that comes with `passport-azure-ad`.</span></span>


```JavaScript
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

// Add some logging
var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
});
```

<span data-ttu-id="6bb44-164">Használja a korábban említett toohandle bejelentkezési kérelmek hello stratégiát.</span><span class="sxs-lookup"><span data-stu-id="6bb44-164">Use hello strategy you just referenced toohandle sign-in requests.</span></span>

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
<span data-ttu-id="6bb44-165">A Passport az összes stratégia (ideértve a Twittert és a Facebookot is) esetében hasonló mintát alkalmaz.</span><span class="sxs-lookup"><span data-stu-id="6bb44-165">Passport uses a similar pattern for all of its strategies (including Twitter and Facebook).</span></span> <span data-ttu-id="6bb44-166">Stratégiafejlesztő toothis mintát.</span><span class="sxs-lookup"><span data-stu-id="6bb44-166">All strategy writers adhere toothis pattern.</span></span> <span data-ttu-id="6bb44-167">Ha hello stratégia tekinti meg, láthatja, át kell adni azt egy `function()` , amely rendelkezik egy jogkivonat és egy `done` hello paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="6bb44-167">When you look at hello strategy, you can see that you pass it a `function()` that has a token and a `done` as hello parameters.</span></span> <span data-ttu-id="6bb44-168">hello stratégia ismét elérhető lesz tooyou után fordult elő az összes teendőit.</span><span class="sxs-lookup"><span data-stu-id="6bb44-168">hello strategy comes back tooyou after it has done all of its work.</span></span> <span data-ttu-id="6bb44-169">Hello felhasználói tárolja, és menteni a hello jogkivonat, így nem kell tooask azt újra.</span><span class="sxs-lookup"><span data-stu-id="6bb44-169">Store hello user and stash hello token so that you don’t need tooask for it again.</span></span>

> [!IMPORTANT]
<span data-ttu-id="6bb44-170">hello előző kód szükséges minden olyan felhasználó, akinek hello kiszolgáló hitelesíti.</span><span class="sxs-lookup"><span data-stu-id="6bb44-170">hello preceding code takes all users whom hello server authenticates.</span></span> <span data-ttu-id="6bb44-171">Ezt nevezzük automatikus regisztrációnak.</span><span class="sxs-lookup"><span data-stu-id="6bb44-171">This is autoregistration.</span></span> <span data-ttu-id="6bb44-172">Éles kiszolgálók használata, ha nem szeretné, hogy a felhasználók toolet kivéve, ha túl vannak, amelyek állított be regisztrációs folyamatot.</span><span class="sxs-lookup"><span data-stu-id="6bb44-172">When you use production servers, you don’t want toolet in users unless they have gone through a registration process that you have set up.</span></span> <span data-ttu-id="6bb44-173">Ez gyakori eljárás a fogyasztói alkalmazásoknál.</span><span class="sxs-lookup"><span data-stu-id="6bb44-173">You can often see this pattern in consumer apps.</span></span> <span data-ttu-id="6bb44-174">Ezek lehetővé teszik tooregister Facebook-fiókkal, de majd kéri az toofill meg további információkat.</span><span class="sxs-lookup"><span data-stu-id="6bb44-174">These allow you tooregister by using Facebook, but then they ask you toofill out additional information.</span></span> <span data-ttu-id="6bb44-175">Ha az alkalmazás nem csupán mintaként szolgálna, azt sikerült kinyerése egy e-mail címet hello jogkivonat-objektumból ad vissza, és majd kérje meg a hello felhasználói toofill meg további információkat.</span><span class="sxs-lookup"><span data-stu-id="6bb44-175">If our application wasn’t a sample, we could extract an email address from hello token object that is returned, and then ask hello user toofill out additional information.</span></span> <span data-ttu-id="6bb44-176">Mivel ez egy tesztkiszolgálón, egyszerűen hozzáadjuk a felhasználók toohello memóriában lévő adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="6bb44-176">Because this is a test server, we simply add users toohello in-memory database.</span></span>

<span data-ttu-id="6bb44-177">Adja hozzá, amelyek lehetővé teszik a felhasználók, akik bejelentkezett, tookeep követése hello metódusokat Passport által előírt.</span><span class="sxs-lookup"><span data-stu-id="6bb44-177">Add hello methods that allow you tookeep track of users who have signed in, as required by Passport.</span></span> <span data-ttu-id="6bb44-178">Ehhez tartozik a felhasználói adatok szerializálása és deszerializálása is:</span><span class="sxs-lookup"><span data-stu-id="6bb44-178">This includes serializing and deserializing user information:</span></span>

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

<span data-ttu-id="6bb44-179">Adja hozzá a hello kód tooload hello Express motor.</span><span class="sxs-lookup"><span data-stu-id="6bb44-179">Add hello code tooload hello Express engine.</span></span> <span data-ttu-id="6bb44-180">A következő hello, megtekintheti, hogy használjuk-e hello alapértelmezett `/views` és `/routes` mintát, amely az Express által biztosított.</span><span class="sxs-lookup"><span data-stu-id="6bb44-180">In hello following, you can see that we use hello default `/views` and `/routes` pattern that Express provides.</span></span>

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

<span data-ttu-id="6bb44-181">Adja hozzá a hello `POST` útvonalakat, amelyek kiosztják a tényleges bejelentkezési kérelmek toohello hello `passport-azure-ad` motor:</span><span class="sxs-lookup"><span data-stu-id="6bb44-181">Add hello `POST` routes that hand off hello actual sign-in requests toohello `passport-azure-ad` engine:</span></span>

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

## <a name="use-passport-tooissue-sign-in-and-sign-out-requests-tooazure-ad"></a><span data-ttu-id="6bb44-182">A Passport tooissue bejelentkezési és kijelentkezési kérések tooAzure AD használatára</span><span class="sxs-lookup"><span data-stu-id="6bb44-182">Use Passport tooissue sign-in and sign-out requests tooAzure AD</span></span>

<span data-ttu-id="6bb44-183">Az alkalmazás már megfelelően konfigurált toocommunicate hello v2.0-végponttal hello OpenID Connect hitelesítési protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="6bb44-183">Your app is now properly configured toocommunicate with hello v2.0 endpoint by using hello OpenID Connect authentication protocol.</span></span> <span data-ttu-id="6bb44-184">`passport-azure-ad`rendelkezik hitelesítési üzenetek létrehozásával, ellenőrzése az Azure ad-jogkivonatok és karbantartásáról a felhasználói munkamenet hello részleteit végrehajtott kezeli.</span><span class="sxs-lookup"><span data-stu-id="6bb44-184">`passport-azure-ad` has taken care of hello details of crafting authentication messages, validating tokens from Azure AD, and maintaining user session.</span></span> <span data-ttu-id="6bb44-185">Hogy továbbra is van toogive a felhasználók a módon toosign, és jelentkezzen ki, és bejelentkezett felhasználók toogather további információkat.</span><span class="sxs-lookup"><span data-stu-id="6bb44-185">All that remains is toogive your users a way toosign in and sign out, and toogather additional information on users who have signed in.</span></span>

<span data-ttu-id="6bb44-186">Először adja hozzá a hello alapértelmezett, bejelentkezési, fiókkal, és kijelentkezési metódusokat tooyour `app.js` fájlt:</span><span class="sxs-lookup"><span data-stu-id="6bb44-186">First, add hello default, sign-in, account, and sign-out methods tooyour `app.js` file:</span></span>

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

<span data-ttu-id="6bb44-187">tooreview ezen módszerek részletes információkat:</span><span class="sxs-lookup"><span data-stu-id="6bb44-187">tooreview these methods in detail:</span></span>
- <span data-ttu-id="6bb44-188">Hello `/` útvonal átirányítja a felhasználókat toohello `index.ejs` nézet úgy, hogy hello felhasználói hello kérelem (ha van ilyen).</span><span class="sxs-lookup"><span data-stu-id="6bb44-188">hello `/` route redirects toohello `index.ejs` view by passing hello user in hello request (if it exists).</span></span>
- <span data-ttu-id="6bb44-189">Hello `/account` útvonal először ellenőrzi, hogy hitelesítése (hello végrehajtása a következő alatt).</span><span class="sxs-lookup"><span data-stu-id="6bb44-189">hello `/account` route first verifies that you are authenticated (hello implementation for this is below).</span></span> <span data-ttu-id="6bb44-190">Ezután átadja hello felhasználói hello kérelem, hogy hello felhasználó további információt kaphat.</span><span class="sxs-lookup"><span data-stu-id="6bb44-190">It then passes hello user in hello request so that you can get additional information about hello user.</span></span>
- <span data-ttu-id="6bb44-191">Hello `/login` útvonal hívások hello `azuread-openidconnect` a hitelesítő `passport-azure-ad`.</span><span class="sxs-lookup"><span data-stu-id="6bb44-191">hello `/login` route calls hello `azuread-openidconnect` authenticator from `passport-azure-ad`.</span></span> <span data-ttu-id="6bb44-192">Ha nem jár sikerrel, hello útvonal túl átirányítja a felhasználókat hello felhasználói vissza`/login`.</span><span class="sxs-lookup"><span data-stu-id="6bb44-192">If it doesn't succeed, hello route redirects hello user back too`/login`.</span></span>
- <span data-ttu-id="6bb44-193">`/logout` egyszerűen meghívja a `logout.ejs`-t (és annak útvonalát).</span><span class="sxs-lookup"><span data-stu-id="6bb44-193">`/logout` simply calls `logout.ejs` (and its route).</span></span> <span data-ttu-id="6bb44-194">Ez törli a cookie-kat, és ezután a beolvasása hello vissza felhasználói túl`index.ejs`.</span><span class="sxs-lookup"><span data-stu-id="6bb44-194">This clears cookies and then returns hello user back too`index.ejs`.</span></span>


<span data-ttu-id="6bb44-195">Hello utolsó részének `app.js`, vegye fel a hello `EnsureAuthenticated` hello használt módszer `/account` útvonal.</span><span class="sxs-lookup"><span data-stu-id="6bb44-195">For hello last part of `app.js`, add hello `EnsureAuthenticated` method that is used in hello `/account` route.</span></span>

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

<span data-ttu-id="6bb44-196">Végezetül hozza létre magát hello kiszolgálót a `app.js`.</span><span class="sxs-lookup"><span data-stu-id="6bb44-196">Finally, create hello server itself in `app.js`.</span></span>

```JavaScript

app.listen(3000);

```


## <a name="create-hello-views-and-routes-in-express-toocall-your-policies"></a><span data-ttu-id="6bb44-197">Hello nézeteket hozhat létre, és az Express toocall továbbítja a házirendek</span><span class="sxs-lookup"><span data-stu-id="6bb44-197">Create hello views and routes in Express toocall your policies</span></span>

<span data-ttu-id="6bb44-198">Az `app.js` ezzel elkészült.</span><span class="sxs-lookup"><span data-stu-id="6bb44-198">Your `app.js` is now complete.</span></span> <span data-ttu-id="6bb44-199">Egyszerűen tooadd hello útvonalakat és nézeteket, amelyek lehetővé teszik toocall hello bejelentkezési és regisztrációs szabályzatok.</span><span class="sxs-lookup"><span data-stu-id="6bb44-199">You just need tooadd hello routes and views that allow you toocall hello sign-in and sign-up policies.</span></span> <span data-ttu-id="6bb44-200">Ezek kezelik hello `/logout` és `/login` létrehozott útvonalak.</span><span class="sxs-lookup"><span data-stu-id="6bb44-200">These also handle hello `/logout` and `/login` routes you created.</span></span>

<span data-ttu-id="6bb44-201">Hozzon létre hello `/routes/index.js` útvonal hello gyökérkönyvtárban.</span><span class="sxs-lookup"><span data-stu-id="6bb44-201">Create hello `/routes/index.js` route under hello root directory.</span></span>

```JavaScript

/*
 * GET home page.
 */

exports.index = function(req, res){
  res.render('index', { title: 'Express' });
};
```

<span data-ttu-id="6bb44-202">Hozzon létre hello `/routes/user.js` útvonal hello gyökérkönyvtárban.</span><span class="sxs-lookup"><span data-stu-id="6bb44-202">Create hello `/routes/user.js` route under hello root directory.</span></span>

```JavaScript

/*
 * GET users listing.
 */

exports.list = function(req, res){
  res.send("respond with a resource");
};
```

<span data-ttu-id="6bb44-203">Ezek az egyszerű útvonalak adják át a kérelmek tooyour nézetek.</span><span class="sxs-lookup"><span data-stu-id="6bb44-203">These simple routes pass along requests tooyour views.</span></span> <span data-ttu-id="6bb44-204">Ha ilyen hello felhasználói tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="6bb44-204">They include hello user, if one is present.</span></span>

<span data-ttu-id="6bb44-205">Hozzon létre hello `/views/index.ejs` nézet hello gyökérkönyvtárban.</span><span class="sxs-lookup"><span data-stu-id="6bb44-205">Create hello `/views/index.ejs` view under hello root directory.</span></span> <span data-ttu-id="6bb44-206">Ez egy egyszerű lap, amely a bejelentkezési és kijelentkezési szabályzatok meghívását végzi. Használhatja az toograb fiók adatait.</span><span class="sxs-lookup"><span data-stu-id="6bb44-206">This is a simple page that calls policies for sign-in and sign-out. You can also use it toograb account information.</span></span> <span data-ttu-id="6bb44-207">Vegye figyelembe, hogy használhatja-e feltételes hello `if (!user)` , hello felhasználói átadott keresztül hello kérelem tooprovide bizonyíték hello felhasználó bejelentkezett.</span><span class="sxs-lookup"><span data-stu-id="6bb44-207">Note that you can use hello conditional `if (!user)` as hello user is passed through in hello request tooprovide evidence that hello user is signed in.</span></span>

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

<span data-ttu-id="6bb44-208">Hozzon létre hello `/views/account.ejs` hello gyökérkönyvtárban megtekintheti, hogy további információkat láthat, amelyek `passport-azure-ad` hello felhasználói kérelem be.</span><span class="sxs-lookup"><span data-stu-id="6bb44-208">Create hello `/views/account.ejs` view under hello root directory so that you can view additional information that `passport-azure-ad` put in hello user request.</span></span>

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

<span data-ttu-id="6bb44-209">Most már lefordíthatja, és futtathatja az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6bb44-209">You can now build and run your app.</span></span>

<span data-ttu-id="6bb44-210">Futtatás `node app.js` , és keresse meg a túl`http://localhost:3000`</span><span class="sxs-lookup"><span data-stu-id="6bb44-210">Run `node app.js` and navigate too`http://localhost:3000`</span></span>


<span data-ttu-id="6bb44-211">Regisztráció, vagy jelentkezzen be toohello app e-mail vagy Facebook használatával.</span><span class="sxs-lookup"><span data-stu-id="6bb44-211">Sign up or sign in toohello app by using email or Facebook.</span></span> <span data-ttu-id="6bb44-212">Jelentkezzen ki, majd jelentkezzen be ismét egy másik felhasználóval.</span><span class="sxs-lookup"><span data-stu-id="6bb44-212">Sign out and sign back in as a different user.</span></span>

##<a name="next-steps"></a><span data-ttu-id="6bb44-213">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6bb44-213">Next steps</span></span>

<span data-ttu-id="6bb44-214">Referenciaként hello befejeződött (a konfigurációs értékek nélkül) minta [egy .zip-fájlban is](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="6bb44-214">For reference, hello completed sample (without your configuration values) [is provided as a .zip file](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-NodeJS/archive/complete.zip).</span></span> <span data-ttu-id="6bb44-215">Ezenfelül a GitHubból is klónozhatja:</span><span class="sxs-lookup"><span data-stu-id="6bb44-215">You can also clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-nodejs.git```

<span data-ttu-id="6bb44-216">Most már továbbléphet a témakörök speciális toomore.</span><span class="sxs-lookup"><span data-stu-id="6bb44-216">You can now move on toomore advanced topics.</span></span> <span data-ttu-id="6bb44-217">Próbálkozzon meg a következőkkel:</span><span class="sxs-lookup"><span data-stu-id="6bb44-217">You might try:</span></span>

[<span data-ttu-id="6bb44-218">A webes API biztonságossá tétele a node.js hello B2C-modell használatával</span><span class="sxs-lookup"><span data-stu-id="6bb44-218">Secure a web API by using hello B2C model in Node.js</span></span>](active-directory-b2c-devquickstarts-api-node.md)

<!--

For additional resources, check out:
You can now move on toomore advanced B2C topics. You might try:

[Call a Node.js web API from a Node.js web app]()

[Customizing hello your B2C App's UX >>]()

-->
