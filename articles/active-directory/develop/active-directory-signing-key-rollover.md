---
title: "az Azure AD-kulcs átfordulási aaaSigning |} Microsoft Docs"
description: "Ez a cikk ismerteti, amelyek hello aláírási kulcs átfordulási gyakorlati tanácsok az Azure Active Directory"
services: active-directory
documentationcenter: .net
author: dstrockis
manager: krassk
editor: 
ms.assetid: ed964056-0723-42fe-bb69-e57323b9407f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2016
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: ac6ade7f3ba2fbd22ea6d447aa5d07a2d6bdd451
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="signing-key-rollover-in-azure-active-directory"></a><span data-ttu-id="b563f-103">Az Azure Active Directoryban kulcsváltás aláírása</span><span class="sxs-lookup"><span data-stu-id="b563f-103">Signing key rollover in Azure Active Directory</span></span>
<span data-ttu-id="b563f-104">Ez a témakör ismerteti, mire van szüksége az Azure Active Directory (Azure AD) toosign biztonsági jogkivonatokat használt nyilvános kulcsok hello tooknow.</span><span class="sxs-lookup"><span data-stu-id="b563f-104">This topic discusses what you need tooknow about hello public keys that are used in Azure Active Directory (Azure AD) toosign security tokens.</span></span> <span data-ttu-id="b563f-105">Fontos, hogy rendszeres időközönként, és vészhelyzet esetén a kulcsok helyettesítő volt állítva keresztül azonnal toonote.</span><span class="sxs-lookup"><span data-stu-id="b563f-105">It is important toonote that these keys rollover on a periodic basis and, in an emergency, could be rolled over immediately.</span></span> <span data-ttu-id="b563f-106">Minden alkalmazás, amely használhatja az Azure Active Directory legyen képes tooprogrammatically leíró hello kulcsváltás folyamat vagy rendszeres manuális helyettesítő-folyamatot.</span><span class="sxs-lookup"><span data-stu-id="b563f-106">All applications that use Azure AD should be able tooprogrammatically handle hello key rollover process or establish a periodic manual rollover process.</span></span> <span data-ttu-id="b563f-107">Továbbra is toounderstand olvasása, hogyan hello kulcsok működik, hogyan tooassess hello hello helyettesítő tooyour alkalmazás hatását, és hogyan tooupdate az alkalmazás vagy egy rendszeres manuális váltása folyamat toohandle kulcsváltás létesíteni, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="b563f-107">Continue reading toounderstand how hello keys work, how tooassess hello impact of hello rollover tooyour application and how tooupdate your application or establish a periodic manual rollover process toohandle key rollover if necessary.</span></span>

## <a name="overview-of-signing-keys-in-azure-ad"></a><span data-ttu-id="b563f-108">Az Azure AD-aláírókulcsok áttekintése</span><span class="sxs-lookup"><span data-stu-id="b563f-108">Overview of signing keys in Azure AD</span></span>
<span data-ttu-id="b563f-109">Az Azure AD ipari szabványok tooestablish megbízhatósági és hello között épülő nyilvános kulcsú titkosítást használ, az azt használó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="b563f-109">Azure AD uses public-key cryptography built on industry standards tooestablish trust between itself and hello applications that use it.</span></span> <span data-ttu-id="b563f-110">Gyakorlatilag, ez hello a következő módon működik: az Azure AD nyilvános és titkos kulcsból álló kulcspárt tartalmazó aláíró kulcsot használ.</span><span class="sxs-lookup"><span data-stu-id="b563f-110">In practical terms, this works in hello following way: Azure AD uses a signing key that consists of a public and private key pair.</span></span> <span data-ttu-id="b563f-111">Tooan alkalmazás az Azure AD használ a hitelesítéshez a felhasználó bejelentkezik, az Azure AD létrehoz egy biztonsági jogkivonatot, amely hello felhasználói adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b563f-111">When a user signs in tooan application that uses Azure AD for authentication, Azure AD creates a security token that contains information about hello user.</span></span> <span data-ttu-id="b563f-112">Ez a token használatával annak titkos kulcsát, hátsó toohello alkalmazás elküldés előtt az Azure AD által aláírt.</span><span class="sxs-lookup"><span data-stu-id="b563f-112">This token is signed by Azure AD using its private key before it is sent back toohello application.</span></span> <span data-ttu-id="b563f-113">amely a hello token tooverify érvényes és az Azure AD ténylegesen kezdeményezésű, hello alkalmazásnak ellenőriznie kell a nyilvános kulcsával. hello hello bérlői lévő az Azure AD által elérhetővé tett hello jogkivonat aláírása [OpenID Connect felderítési dokumentum](http://openid.net/specs/openid-connect-discovery-1_0.html) vagy SAML/WS-Fed [összevonási metaadat-dokumentum](active-directory-federation-metadata.md).</span><span class="sxs-lookup"><span data-stu-id="b563f-113">tooverify that hello token is valid and actually originated from Azure AD, hello application must validate hello token’s signature using hello public key exposed by Azure AD that is contained in hello tenant’s [OpenID Connect discovery document](http://openid.net/specs/openid-connect-discovery-1_0.html) or SAML/WS-Fed [federation metadata document](active-directory-federation-metadata.md).</span></span>

<span data-ttu-id="b563f-114">Biztonsági okokból az Azure AD aláírási kulcs összesíti rendszeres időközönként és hello esetben egy ideiglenes volt állítva azonnal.</span><span class="sxs-lookup"><span data-stu-id="b563f-114">For security purposes, Azure AD’s signing key rolls on a periodic basis and, in hello case of an emergency, could be rolled over immediately.</span></span> <span data-ttu-id="b563f-115">Bármely alkalmazás, amely az Azure AD egy kulcsváltás esemény nem számít, hogy milyen gyakran előfordulhat toohandle kell készíteni.</span><span class="sxs-lookup"><span data-stu-id="b563f-115">Any application that integrates with Azure AD should be prepared toohandle a key rollover event no matter how frequently it may occur.</span></span> <span data-ttu-id="b563f-116">Ha nem, és a kísérletet tesz egy lejárt kulcs tooverify hello aláírás a jogkivonat toouse, hello bejelentkezési kérelme sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="b563f-116">If it doesn’t, and your application attempts toouse an expired key tooverify hello signature on a token, hello sign-in request will fail.</span></span>

<span data-ttu-id="b563f-117">Nincs mindig egynél több érvényes kulcs hello OpenID Connect felderítési dokumentum és hello összevonási metaadat-dokumentum.</span><span class="sxs-lookup"><span data-stu-id="b563f-117">There is always more than one valid key available in hello OpenID Connect discovery document and hello federation metadata document.</span></span> <span data-ttu-id="b563f-118">Az alkalmazás kell készíteni a toouse hello kulcsok megadott hello dokumentum, mivel előfordulhat, hogy később is visszalátogatnia, állítva egy kulcs egy másik előfordulhat, hogy azok helyettesítéseivel, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="b563f-118">Your application should be prepared toouse any of hello keys specified in hello document, since one key may be rolled soon, another may be its replacement, and so forth.</span></span>

## <a name="how-tooassess-if-your-application-will-be-affected-and-what-toodo-about-it"></a><span data-ttu-id="b563f-119">Hogyan tooassess, ha az alkalmazás érinti, és milyen toodo információk</span><span class="sxs-lookup"><span data-stu-id="b563f-119">How tooassess if your application will be affected and what toodo about it</span></span>
<span data-ttu-id="b563f-120">Például az alkalmazás vagy milyen identitás protokoll és a könyvtár használt hello típusú változók függ, hogy miként kezeli az alkalmazás a kulcsváltás.</span><span class="sxs-lookup"><span data-stu-id="b563f-120">How your application handles key rollover depends on variables such as hello type of application or what identity protocol and library was used.</span></span> <span data-ttu-id="b563f-121">az alábbi hello szakaszok ellenőrzéséhez, hogy hello leggyakoribb alkalmazások hello kulcsváltás által érintett, és hogyan tooupdate alkalmazás toosupport automatikus helyettesítő hello, vagy manuálisan frissítse a hello kulcs útmutatást nyújtanak.</span><span class="sxs-lookup"><span data-stu-id="b563f-121">hello sections below assess whether hello most common types of applications are impacted by hello key rollover and provide guidance on how tooupdate hello application toosupport automatic rollover or manually update hello key.</span></span>

* [<span data-ttu-id="b563f-122">Natív ügyfélalkalmazások erőforrások elérése</span><span class="sxs-lookup"><span data-stu-id="b563f-122">Native client applications accessing resources</span></span>](#nativeclient)
* [<span data-ttu-id="b563f-123">Webalkalmazások / API-k erőforrások elérése</span><span class="sxs-lookup"><span data-stu-id="b563f-123">Web applications / APIs accessing resources</span></span>](#webclient)
* [<span data-ttu-id="b563f-124">Webalkalmazások / API-k erőforrások védelméről, és segítségével az Azure App Service szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b563f-124">Web applications / APIs protecting resources and built using Azure App Services</span></span>](#appservices)
* [<span data-ttu-id="b563f-125">Webalkalmazások / API-k használata a .NET OWIN OpenID Connect, WS-Fed vagy WindowsAzureActiveDirectoryBearerAuthentication köztes erőforrások védelme</span><span class="sxs-lookup"><span data-stu-id="b563f-125">Web applications / APIs protecting resources using .NET OWIN OpenID Connect, WS-Fed or WindowsAzureActiveDirectoryBearerAuthentication middleware</span></span>](#owin)
* [<span data-ttu-id="b563f-126">Webalkalmazások / API-k használata a .NET Core OpenID Connect vagy JwtBearerAuthentication köztes erőforrások védelme</span><span class="sxs-lookup"><span data-stu-id="b563f-126">Web applications / APIs protecting resources using .NET Core OpenID Connect or  JwtBearerAuthentication middleware</span></span>](#owincore)
* [<span data-ttu-id="b563f-127">Webalkalmazások / API-k Node.js passport-azure-ad-modullal erőforrások védelme</span><span class="sxs-lookup"><span data-stu-id="b563f-127">Web applications / APIs protecting resources using Node.js passport-azure-ad module</span></span>](#passport)
* [<span data-ttu-id="b563f-128">Webalkalmazások / API-k erőforrások védelméről, és a Visual Studio 2015-öt vagy a Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="b563f-128">Web applications / APIs protecting resources and created with Visual Studio 2015 or Visual Studio 2017</span></span>](#vs2015)
* [<span data-ttu-id="b563f-129">Webalkalmazások erőforrások védelméről, és a Visual Studio 2013 létre</span><span class="sxs-lookup"><span data-stu-id="b563f-129">Web applications protecting resources and created with Visual Studio 2013</span></span>](#vs2013)
* [<span data-ttu-id="b563f-130">Webes API-k erőforrások védelméről, és a Visual Studio 2013 létre</span><span class="sxs-lookup"><span data-stu-id="b563f-130">Web APIs protecting resources and created with Visual Studio 2013</span></span>](#vs2013_webapi)
* [<span data-ttu-id="b563f-131">Erőforrások védelme és a Visual Studio 2012 létrehozott webes alkalmazások</span><span class="sxs-lookup"><span data-stu-id="b563f-131">Web applications protecting resources and created with Visual Studio 2012</span></span>](#vs2012)
* [<span data-ttu-id="b563f-132">Webalkalmazások erőforrások védelméről, és a Visual Studio 2010, a Windows Identity Foundation használatával 2008 o létre</span><span class="sxs-lookup"><span data-stu-id="b563f-132">Web applications protecting resources and created with Visual Studio 2010, 2008 o using Windows Identity Foundation</span></span>](#vs2010)
* [<span data-ttu-id="b563f-133">Webalkalmazások / API-k erőforrások védelme bármely más szalagtárak, vagy manuálisan végrehajtási bármelyik hello támogatott protokollok</span><span class="sxs-lookup"><span data-stu-id="b563f-133">Web applications / APIs protecting resources using any other libraries or manually implementing any of hello supported protocols</span></span>](#other)

<span data-ttu-id="b563f-134">Ez az útmutató **nem** alkalmazható:</span><span class="sxs-lookup"><span data-stu-id="b563f-134">This guidance is **not** applicable for:</span></span>

* <span data-ttu-id="b563f-135">Az Azure AD Application Gallery (beleértve az egyéni) hozzáadott alkalmazások rendelkeznek tanúsítványinformációit toosigning kulcsok külön segítséget.</span><span class="sxs-lookup"><span data-stu-id="b563f-135">Applications added from Azure AD Application Gallery (including Custom) have separate guidance with regards toosigning keys.</span></span> [<span data-ttu-id="b563f-136">További információt.</span><span class="sxs-lookup"><span data-stu-id="b563f-136">More information.</span></span>](../active-directory-sso-certs.md)
* <span data-ttu-id="b563f-137">A helyszíni alkalmazásproxy keresztül közzétett alkalmazás nem rendelkezik tooworry információ az aláíró kulcsok.</span><span class="sxs-lookup"><span data-stu-id="b563f-137">On-premises applications published via application proxy don't have tooworry about signing keys.</span></span>

### <span data-ttu-id="b563f-138"><a name="nativeclient"></a>Natív ügyfélalkalmazások erőforrások elérése</span><span class="sxs-lookup"><span data-stu-id="b563f-138"><a name="nativeclient"></a>Native client applications accessing resources</span></span>
<span data-ttu-id="b563f-139">Erőforrások (azaz csak elérő alkalmazások</span><span class="sxs-lookup"><span data-stu-id="b563f-139">Applications that are only accessing resources (i.e</span></span> <span data-ttu-id="b563f-140">A Microsoft Graph, KeyVault, Outlook API és más Microsoft APIs) általában csak jogkivonat beszerzése, és adja át mentén toohello erőforrás tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="b563f-140">Microsoft Graph, KeyVault, Outlook API and other Microsoft APIs) generally only obtain a token and pass it along toohello resource owner.</span></span> <span data-ttu-id="b563f-141">Figyelembe véve, hogy azok nem védett erőforrások, nem vizsgálja meg a hello jogkivonatot, és emiatt nem kell a megfelelő aláírás tooensure.</span><span class="sxs-lookup"><span data-stu-id="b563f-141">Given that they are not protecting any resources, they do not inspect hello token and therefore do not need tooensure it is properly signed.</span></span>

<span data-ttu-id="b563f-142">Natív ügyfélalkalmazások, asztali vagy hordozható, ebbe a kategóriába tartozik, és így nem érinti hello kulcsváltás.</span><span class="sxs-lookup"><span data-stu-id="b563f-142">Native client applications, whether desktop or mobile, fall into this category and are thus not impacted by hello rollover.</span></span>

### <span data-ttu-id="b563f-143"><a name="webclient"></a>Webalkalmazások / API-k erőforrások elérése</span><span class="sxs-lookup"><span data-stu-id="b563f-143"><a name="webclient"></a>Web applications / APIs accessing resources</span></span>
<span data-ttu-id="b563f-144">Erőforrások (azaz csak elérő alkalmazások</span><span class="sxs-lookup"><span data-stu-id="b563f-144">Applications that are only accessing resources (i.e</span></span> <span data-ttu-id="b563f-145">A Microsoft Graph, KeyVault, Outlook API és más Microsoft APIs) általában csak jogkivonat beszerzése, és adja át mentén toohello erőforrás tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="b563f-145">Microsoft Graph, KeyVault, Outlook API and other Microsoft APIs) generally only obtain a token and pass it along toohello resource owner.</span></span> <span data-ttu-id="b563f-146">Figyelembe véve, hogy azok nem védett erőforrások, nem vizsgálja meg a hello jogkivonatot, és emiatt nem kell a megfelelő aláírás tooensure.</span><span class="sxs-lookup"><span data-stu-id="b563f-146">Given that they are not protecting any resources, they do not inspect hello token and therefore do not need tooensure it is properly signed.</span></span>

<span data-ttu-id="b563f-147">Webalkalmazások és webes API-khoz hello csak alkalmazás folyamat által használt (ügyfél-hitelesítő adatok / ügyféltanúsítvány), ebbe a kategóriába tartozik, és így nem érinti hello helyettesítő.</span><span class="sxs-lookup"><span data-stu-id="b563f-147">Web applications and web APIs that are using hello app-only flow (client credentials / client certificate), fall into this category and are thus not impacted by hello rollover.</span></span>

### <span data-ttu-id="b563f-148"><a name="appservices"></a>Webalkalmazások / API-k erőforrások védelméről, és segítségével az Azure App Service szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="b563f-148"><a name="appservices"></a>Web applications / APIs protecting resources and built using Azure App Services</span></span>
<span data-ttu-id="b563f-149">Az Azure App Services hitelesítési / engedélyezési (EasyAuth) funkció már hello szükséges logika toohandle kulcsváltás automatikusan.</span><span class="sxs-lookup"><span data-stu-id="b563f-149">Azure App Services' Authentication / Authorization (EasyAuth) functionality already has hello necessary logic toohandle key rollover automatically.</span></span>

### <span data-ttu-id="b563f-150"><a name="owin"></a>Webalkalmazások / API-k használata a .NET OWIN OpenID Connect, WS-Fed vagy WindowsAzureActiveDirectoryBearerAuthentication köztes erőforrások védelme</span><span class="sxs-lookup"><span data-stu-id="b563f-150"><a name="owin"></a>Web applications / APIs protecting resources using .NET OWIN OpenID Connect, WS-Fed or WindowsAzureActiveDirectoryBearerAuthentication middleware</span></span>
<span data-ttu-id="b563f-151">Ha az alkalmazás által használt .NET OWIN OpenID Connect, WS-Fed vagy WindowsAzureActiveDirectoryBearerAuthentication köztes hello már rendelkezik hello szükséges logika toohandle kulcsváltás automatikusan.</span><span class="sxs-lookup"><span data-stu-id="b563f-151">If your application is using hello .NET OWIN OpenID Connect, WS-Fed or WindowsAzureActiveDirectoryBearerAuthentication middleware, it already has hello necessary logic toohandle key rollover automatically.</span></span>

<span data-ttu-id="b563f-152">Úgy ellenőrizheti, hogy az alkalmazás ezek az alábbi kódtöredékek az alkalmazás Startup.cs vagy Startup.Auth.cs hello bármelyike használ</span><span class="sxs-lookup"><span data-stu-id="b563f-152">You can confirm that your application is using any of these by looking for any of hello following snippets in your application's Startup.cs or Startup.Auth.cs</span></span>

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseWsFederationAuthentication(
    new WsFederationAuthenticationOptions
    {
     // ...
     });
```
```
 app.UseWindowsAzureActiveDirectoryBearerAuthentication(
     new WindowsAzureActiveDirectoryBearerAuthenticationOptions
     {
     // ...
     });
```

### <span data-ttu-id="b563f-153"><a name="owincore"></a>Webalkalmazások / API-k használata a .NET Core OpenID Connect vagy JwtBearerAuthentication köztes erőforrások védelme</span><span class="sxs-lookup"><span data-stu-id="b563f-153"><a name="owincore"></a>Web applications / APIs protecting resources using .NET Core OpenID Connect or  JwtBearerAuthentication middleware</span></span>
<span data-ttu-id="b563f-154">Ha az alkalmazás nem használja a .NET Core OWIN OpenID Connect hello vagy JwtBearerAuthentication köztes, már rendelkezik hello szükséges logika toohandle kulcsváltás automatikusan.</span><span class="sxs-lookup"><span data-stu-id="b563f-154">If your application is using hello .NET Core OWIN OpenID Connect  or JwtBearerAuthentication middleware, it already has hello necessary logic toohandle key rollover automatically.</span></span>

<span data-ttu-id="b563f-155">Úgy ellenőrizheti, hogy az alkalmazás ezek az alábbi kódtöredékek az alkalmazás Startup.cs vagy Startup.Auth.cs hello bármelyike használ</span><span class="sxs-lookup"><span data-stu-id="b563f-155">You can confirm that your application is using any of these by looking for any of hello following snippets in your application's Startup.cs or Startup.Auth.cs</span></span>

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseJwtBearerAuthentication(
    new JwtBearerAuthenticationOptions
    {
     // ...
     });
```

### <span data-ttu-id="b563f-156"><a name="passport"></a>Webalkalmazások / API-k Node.js passport-azure-ad-modullal erőforrások védelme</span><span class="sxs-lookup"><span data-stu-id="b563f-156"><a name="passport"></a>Web applications / APIs protecting resources using Node.js passport-azure-ad module</span></span>
<span data-ttu-id="b563f-157">Az alkalmazás hello Node.js passport-ad-modullal használ, ha már rendelkezik hello szükséges logika toohandle kulcsváltás automatikusan.</span><span class="sxs-lookup"><span data-stu-id="b563f-157">If your application is using hello Node.js passport-ad module, it already has hello necessary logic toohandle key rollover automatically.</span></span>

<span data-ttu-id="b563f-158">Úgy ellenőrizheti, hogy az alkalmazás passport-ad a következő kódrészletet a az alkalmazás app.js hello keresése</span><span class="sxs-lookup"><span data-stu-id="b563f-158">You can confirm that your application passport-ad by searching for hello following snippet in your application's app.js</span></span>

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <span data-ttu-id="b563f-159"><a name="vs2015"></a>Webalkalmazások / API-k erőforrások védelméről, és a Visual Studio 2015-öt vagy a Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="b563f-159"><a name="vs2015"></a>Web applications / APIs protecting resources and created with Visual Studio 2015 or Visual Studio 2017</span></span>
<span data-ttu-id="b563f-160">Ha az alkalmazást a Visual Studio 2015-öt vagy a Visual Studio 2017 webes alkalmazás sablon használatával lett létrehozva, és a kiválasztott **munkahelyi és iskolai fiókok** a hello **hitelesítés módosítása** menü azt már hello szükséges logika toohandle kulcsváltás automatikusan rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b563f-160">If your application was built using a web application template in Visual Studio 2015 or Visual Studio 2017 and you selected **Work And School Accounts** from hello **Change Authentication** menu, it already has hello necessary logic toohandle key rollover automatically.</span></span> <span data-ttu-id="b563f-161">A programot, hello OWIN OpenID Connect köztes ágyazott kéri le, és gyorsítótárba helyezi azt a hello OpenID Connect felderítési dokumentum hello kulcsokat, és rendszeresen frissülnek.</span><span class="sxs-lookup"><span data-stu-id="b563f-161">This logic, embedded in hello OWIN OpenID Connect middleware, retrieves and caches hello keys from hello OpenID Connect discovery document and periodically refreshes them.</span></span>

<span data-ttu-id="b563f-162">Ha manuálisan adta hozzá a hitelesítési tooyour megoldás, az alkalmazás esetleg nincs hello szükséges kulcsváltás logikát.</span><span class="sxs-lookup"><span data-stu-id="b563f-162">If you added authentication tooyour solution manually, your application might not have hello necessary key rollover logic.</span></span> <span data-ttu-id="b563f-163">Szüksége lesz a toowrite azt saját magára vagy kövesse hello szükséges lépések [webalkalmazások / API-k bármely más könyvtárak segítségével, vagy manuálisan végrehajtási bármelyik hello támogatott protokollok.](#other).</span><span class="sxs-lookup"><span data-stu-id="b563f-163">You will need toowrite it yourself, or follow hello steps in [Web applications / APIs using any other libraries or manually implementing any of hello supported protocols.](#other).</span></span>

### <span data-ttu-id="b563f-164"><a name="vs2013"></a>Webalkalmazások erőforrások védelméről, és a Visual Studio 2013 létre</span><span class="sxs-lookup"><span data-stu-id="b563f-164"><a name="vs2013"></a>Web applications protecting resources and created with Visual Studio 2013</span></span>
<span data-ttu-id="b563f-165">Ha az alkalmazás a Visual Studio 2013 webes alkalmazás sablon használatával lett létrehozva, és a kiválasztott **munkahelyi és iskolai fiókok** a hello **hitelesítés módosítása** menü már hello szükséges logikai toohandle automatikusan kulcs átfordulási.</span><span class="sxs-lookup"><span data-stu-id="b563f-165">If your application was built using a web application template in Visual Studio 2013 and you selected **Organizational Accounts** from hello **Change Authentication** menu, it already has hello necessary logic toohandle key rollover automatically.</span></span> <span data-ttu-id="b563f-166">Ezt a szervezet egyedi azonosítóját és a két adatbázis tábla hello projekttel kapcsolatos legfontosabb tudnivalókat aláíró hello tárolja.</span><span class="sxs-lookup"><span data-stu-id="b563f-166">This logic stores your organization’s unique identifier and hello signing key information in two database tables associated with hello project.</span></span> <span data-ttu-id="b563f-167">Hello kapcsolati karakterlánc hello adatbázis hello projekt Web.config fájlban található.</span><span class="sxs-lookup"><span data-stu-id="b563f-167">You can find hello connection string for hello database in hello project’s Web.config file.</span></span>

<span data-ttu-id="b563f-168">Ha manuálisan adta hozzá a hitelesítési tooyour megoldás, az alkalmazás esetleg nincs hello szükséges kulcsváltás logikát.</span><span class="sxs-lookup"><span data-stu-id="b563f-168">If you added authentication tooyour solution manually, your application might not have hello necessary key rollover logic.</span></span> <span data-ttu-id="b563f-169">Szüksége lesz a toowrite azt saját magára vagy kövesse hello szükséges lépések [webalkalmazások / API-k bármely más könyvtárak segítségével, vagy manuálisan végrehajtási bármelyik hello támogatott protokollok.](#other).</span><span class="sxs-lookup"><span data-stu-id="b563f-169">You will need toowrite it yourself, or follow hello steps in [Web applications / APIs using any other libraries or manually implementing any of hello supported protocols.](#other).</span></span>

<span data-ttu-id="b563f-170">a lépéseket követve hello segítségével ellenőrizze, hogy hello programot az alkalmazás megfelelően működik.</span><span class="sxs-lookup"><span data-stu-id="b563f-170">hello following steps will help you verify that hello logic is working properly in your application.</span></span>

1. <span data-ttu-id="b563f-171">A Visual Studio 2013, nyissa meg hello megoldást, és kattintson a hello **Server Explorer** hello jobb oldali lapján.</span><span class="sxs-lookup"><span data-stu-id="b563f-171">In Visual Studio 2013, open hello solution, and then click on hello **Server Explorer** tab on hello right window.</span></span>
2. <span data-ttu-id="b563f-172">Bontsa ki a **adatkapcsolatok**, **DefaultConnection**, majd **táblák**.</span><span class="sxs-lookup"><span data-stu-id="b563f-172">Expand **Data Connections**, **DefaultConnection**, and then **Tables**.</span></span> <span data-ttu-id="b563f-173">Keresse meg a hello **IssuingAuthorityKeys** tábla, kattintson a jobb gombbal, és kattintson a **tábla adatok megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="b563f-173">Locate hello **IssuingAuthorityKeys** table, right-click it, and then click **Show Table Data**.</span></span>
3. <span data-ttu-id="b563f-174">A hello **IssuingAuthorityKeys** table, legalább egy sort, hello kulcs toohello ujjlenyomat értékét megfelelő lesz.</span><span class="sxs-lookup"><span data-stu-id="b563f-174">In hello **IssuingAuthorityKeys** table, there will be at least one row, which corresponds toohello thumbprint value for hello key.</span></span> <span data-ttu-id="b563f-175">Hello tábla sorokat törölni.</span><span class="sxs-lookup"><span data-stu-id="b563f-175">Delete any rows in hello table.</span></span>
4. <span data-ttu-id="b563f-176">Kattintson a jobb gombbal hello **bérlők** tábla, és kattintson a **tábla adatok megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="b563f-176">Right-click hello **Tenants** table, and then click **Show Table Data**.</span></span>
5. <span data-ttu-id="b563f-177">A hello **bérlők** table, legalább egy sort, amely megfelel a tooa egyedi directory bérlő azonosítója lesz.</span><span class="sxs-lookup"><span data-stu-id="b563f-177">In hello **Tenants** table, there will be at least one row, which corresponds tooa unique directory tenant identifier.</span></span> <span data-ttu-id="b563f-178">Hello tábla sorokat törölni.</span><span class="sxs-lookup"><span data-stu-id="b563f-178">Delete any rows in hello table.</span></span> <span data-ttu-id="b563f-179">Ha mindkét hello hello sorok nem töröljük **bérlők** tábla és **IssuingAuthorityKeys** tábla, akkor hibaüzenet fog futásidőben.</span><span class="sxs-lookup"><span data-stu-id="b563f-179">If you don't delete hello rows in both hello **Tenants** table and **IssuingAuthorityKeys** table, you will get an error at runtime.</span></span>
6. <span data-ttu-id="b563f-180">Hozza létre és hello alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="b563f-180">Build and run hello application.</span></span> <span data-ttu-id="b563f-181">Miután tooyour fiók már bejelentkezett, le is hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b563f-181">After you have logged in tooyour account, you can stop hello application.</span></span>
7. <span data-ttu-id="b563f-182">Térjen vissza a toohello **Server Explorer** , és tekintse meg a hello hello értékek **IssuingAuthorityKeys** és **bérlők** tábla.</span><span class="sxs-lookup"><span data-stu-id="b563f-182">Return toohello **Server Explorer** and look at hello values in hello **IssuingAuthorityKeys** and **Tenants** table.</span></span> <span data-ttu-id="b563f-183">Láthatja, hogy azok rendelkeznek lett automatikusan tölteni hello összevonási metaadatok dokumentumból hello megfelelő információkkal.</span><span class="sxs-lookup"><span data-stu-id="b563f-183">You’ll notice that they have been automatically repopulated with hello appropriate information from hello federation metadata document.</span></span>

### <span data-ttu-id="b563f-184"><a name="vs2013"></a>Webes API-k erőforrások védelméről, és a Visual Studio 2013 létre</span><span class="sxs-lookup"><span data-stu-id="b563f-184"><a name="vs2013"></a>Web APIs protecting resources and created with Visual Studio 2013</span></span>
<span data-ttu-id="b563f-185">Ha a webes API-alkalmazás létrehozása a Visual Studio 2013 hello webes API-sablonnal, és akkor kiválasztva **munkahelyi és iskolai fiókok** a hello **hitelesítés módosítása** menü, akkor már rendelkezik hello az alkalmazás lévő szükséges logika.</span><span class="sxs-lookup"><span data-stu-id="b563f-185">If you created a web API application in Visual Studio 2013 using hello Web API template, and then selected **Organizational Accounts** from hello **Change Authentication** menu, you already have hello necessary logic in your application.</span></span>

<span data-ttu-id="b563f-186">Ha manuálisan konfigurált hitelesítési, lépésekkel hello alatt toolearn hogyan tooconfigure a Web API tooautomatically frissíteni az kulcs adatait.</span><span class="sxs-lookup"><span data-stu-id="b563f-186">If you manually configured authentication, follow hello instructions below toolearn how tooconfigure your Web API tooautomatically update its key information.</span></span>

<span data-ttu-id="b563f-187">hello következő kódrészletet bemutatja, hogyan tooget hello hello összevonási metaadat-dokumentum legújabb kulcsokat, és a hello [JWT jogkivonat-kezelő](https://msdn.microsoft.com/library/dn205065.aspx) toovalidate hello jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="b563f-187">hello following code snippet demonstrates how tooget hello latest keys from hello federation metadata document, and then use hello [JWT Token Handler](https://msdn.microsoft.com/library/dn205065.aspx) toovalidate hello token.</span></span> <span data-ttu-id="b563f-188">hello kódrészletet feltételezi, hogy az Azure AD-ről saját tárolásakor hello kulcs toovalidate jövőbeli mechanizmus gyorsítótárazás jogkivonatok használt legyenek az adatbázis, a konfigurációs fájlban vagy máshol.</span><span class="sxs-lookup"><span data-stu-id="b563f-188">hello code snippet assumes that you will use your own caching mechanism for persisting hello key toovalidate future tokens from Azure AD, whether it be in a database, configuration file, or elsewhere.</span></span>

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IdentityModel.Tokens;
using System.Configuration;
using System.Security.Cryptography.X509Certificates;
using System.Xml;
using System.IdentityModel.Metadata;
using System.ServiceModel.Security;
using System.Threading;

namespace JWTValidation
{
    public class JWTValidator
    {
        private string MetadataAddress = "[Your Federation Metadata document address goes here]";

        // Validates hello JWT Token that's part of hello Authorization header in an HTTP request.
        public void ValidateJwtToken(string token)
        {
            JwtSecurityTokenHandler tokenHandler = new JwtSecurityTokenHandler()
            {
                // Do not disable for production code
                CertificateValidator = X509CertificateValidator.None
            };

            TokenValidationParameters validationParams = new TokenValidationParameters()
            {
                AllowedAudience = "[Your App ID URI goes here, as registered in hello Azure Classic Portal]",
                ValidIssuer = "[hello issuer for hello token goes here, such as https://sts.windows.net/68b98905-130e-4d7c-b6e1-a158a9ed8449/]",
                SigningTokens = GetSigningCertificates(MetadataAddress)

                // Cache hello signing tokens by your desired mechanism
            };

            Thread.CurrentPrincipal = tokenHandler.ValidateToken(token, validationParams);
        }

        // Returns a list of certificates from hello specified metadata document.
        public List<X509SecurityToken> GetSigningCertificates(string metadataAddress)
        {
            List<X509SecurityToken> tokens = new List<X509SecurityToken>();

            if (metadataAddress == null)
            {
                throw new ArgumentNullException(metadataAddress);
            }

            using (XmlReader metadataReader = XmlReader.Create(metadataAddress))
            {
                MetadataSerializer serializer = new MetadataSerializer()
                {
                    // Do not disable for production code
                    CertificateValidationMode = X509CertificateValidationMode.None
                };

                EntityDescriptor metadata = serializer.ReadMetadata(metadataReader) as EntityDescriptor;

                if (metadata != null)
                {
                    SecurityTokenServiceDescriptor stsd = metadata.RoleDescriptors.OfType<SecurityTokenServiceDescriptor>().First();

                    if (stsd != null)
                    {
                        IEnumerable<X509RawDataKeyIdentifierClause> x509DataClauses = stsd.Keys.Where(key => key.KeyInfo != null && (key.Use == KeyType.Signing || key.Use == KeyType.Unspecified)).
                                                             Select(key => key.KeyInfo.OfType<X509RawDataKeyIdentifierClause>().First());

                        tokens.AddRange(x509DataClauses.Select(token => new X509SecurityToken(new X509Certificate2(token.GetX509RawData()))));
                    }
                    else
                    {
                        throw new InvalidOperationException("There is no RoleDescriptor of type SecurityTokenServiceType in hello metadata");
                    }
                }
                else
                {
                    throw new Exception("Invalid Federation Metadata document");
                }
            }
            return tokens;
        }
    }
}
```

### <span data-ttu-id="b563f-189"><a name="vs2012"></a>Erőforrások védelme és a Visual Studio 2012 létrehozott webes alkalmazások</span><span class="sxs-lookup"><span data-stu-id="b563f-189"><a name="vs2012"></a>Web applications protecting resources and created with Visual Studio 2012</span></span>
<span data-ttu-id="b563f-190">Ha az alkalmazást a Visual Studio 2012 lett létrehozva, akkor valószínűleg használt hello identitás és hozzáférés eszköz tooconfigure az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b563f-190">If your application was built in Visual Studio 2012, you probably used hello Identity and Access Tool tooconfigure your application.</span></span> <span data-ttu-id="b563f-191">Is valószínű, hogy azok be hello [ellenőrzése kibocsátó neve beállításjegyzék (VINR)](https://msdn.microsoft.com/library/dn205067.aspx).</span><span class="sxs-lookup"><span data-stu-id="b563f-191">It’s also likely that you are using hello [Validating Issuer Name Registry (VINR)](https://msdn.microsoft.com/library/dn205067.aspx).</span></span> <span data-ttu-id="b563f-192">hello VINR karbantartásáért felelős megbízható identitás-szolgáltatóktól (az Azure AD) kapcsolatos információkat, és hello kulcsait használja az általuk kiállított toovalidate jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="b563f-192">hello VINR is responsible for maintaining information about trusted identity providers (Azure AD) and hello keys used toovalidate tokens issued by them.</span></span> <span data-ttu-id="b563f-193">hello VINR is teszi, hogy könnyen tooautomatically frissítés hello kulcsadatokat hello legújabb összevonási metaadatok társított dokumentum a címtárban, ha hello konfiguráció legújabb elavult hello ellenőrzése a letöltés a Web.config fájlban tárolt a dokumentum, és frissítési hello alkalmazás toouse hello új kulcs szükséges.</span><span class="sxs-lookup"><span data-stu-id="b563f-193">hello VINR also makes it easy tooautomatically update hello key information stored in a Web.config file by downloading hello latest federation metadata document associated with your directory, checking if hello configuration is out of date with hello latest document, and updating hello application toouse hello new key as necessary.</span></span>

<span data-ttu-id="b563f-194">Ha bármely hello mintakódok vagy a Microsoft által biztosított forgatókönyv dokumentáció segítségével az alkalmazás hozott létre, hello kulcsváltás logika már szerepel a projektben.</span><span class="sxs-lookup"><span data-stu-id="b563f-194">If you created your application using any of hello code samples or walkthrough documentation provided by Microsoft, hello key rollover logic is already included in your project.</span></span> <span data-ttu-id="b563f-195">Megfigyelheti, hogy hello kódot a projekt már létezik.</span><span class="sxs-lookup"><span data-stu-id="b563f-195">You will notice that hello code below already exists in your project.</span></span> <span data-ttu-id="b563f-196">Ha az alkalmazás már nincs a logikai, lépésekkel hello tooadd alatt, és megfelelően működik tooverify.</span><span class="sxs-lookup"><span data-stu-id="b563f-196">If your application does not already have this logic, follow hello steps below tooadd it and tooverify that it’s working correctly.</span></span>

1. <span data-ttu-id="b563f-197">A **Megoldáskezelőben**, adja hozzá egy hivatkozást toohello **System.IdentityModel** hello megfelelő projekt szerelvény.</span><span class="sxs-lookup"><span data-stu-id="b563f-197">In **Solution Explorer**, add a reference toohello **System.IdentityModel** assembly for hello appropriate project.</span></span>
2. <span data-ttu-id="b563f-198">Nyissa meg hello **Global.asax.cs** fájlt, és adja hozzá a következő hello irányelvek segítségével:</span><span class="sxs-lookup"><span data-stu-id="b563f-198">Open hello **Global.asax.cs** file and add hello following using directives:</span></span>
   ```
   using System.Configuration;
   using System.IdentityModel.Tokens;
   ```
3. <span data-ttu-id="b563f-199">Adja hozzá a következő metódus toohello hello **Global.asax.cs** fájlt:</span><span class="sxs-lookup"><span data-stu-id="b563f-199">Add hello following method toohello **Global.asax.cs** file:</span></span>
   ```
   protected void RefreshValidationSettings()
   {
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
   }
   ```
4. <span data-ttu-id="b563f-200">Hello meghívása **RefreshValidationSettings()** metódus a hello **Application_Start()** metódus a **Global.asax.cs** látható módon:</span><span class="sxs-lookup"><span data-stu-id="b563f-200">Invoke hello **RefreshValidationSettings()** method in hello **Application_Start()** method in **Global.asax.cs** as shown:</span></span>
   ```
   protected void Application_Start()
   {
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
   }
   ```

<span data-ttu-id="b563f-201">Ha követte ezeket a lépéseket, az alkalmazás Web.config dokumentumból hello összevonási metaadatok, többek között a legújabb kulcsok hello hello legújabb adatokkal frissül.</span><span class="sxs-lookup"><span data-stu-id="b563f-201">Once you have followed these steps, your application’s Web.config will be updated with hello latest information from hello federation metadata document, including hello latest keys.</span></span> <span data-ttu-id="b563f-202">A frissítés történik, minden alkalommal, amikor az alkalmazáskészlet újraindul, az IIS-ben; az IIS alapértelmezés szerint értéke toorecycle alkalmazások 29 óránként.</span><span class="sxs-lookup"><span data-stu-id="b563f-202">This update will occur every time your application pool recycles in IIS; by default IIS is set toorecycle applications every 29 hours.</span></span>

<span data-ttu-id="b563f-203">Kövesse az alábbi tooverify, hogy működik-e hello kulcsváltás logika hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="b563f-203">Follow hello steps below tooverify that hello key rollover logic is working.</span></span>

1. <span data-ttu-id="b563f-204">Miután ellenőrizte, hogy az alkalmazás hello nyissa meg a fenti hello olyan helykódot alkalmaz **Web.config** fájlt, és keresse meg a toohello  **<issuerNameRegistry>**  blokk, néhány sor a következő hello kifejezetten keresése:</span><span class="sxs-lookup"><span data-stu-id="b563f-204">After you have verified that your application is using hello code above, open hello **Web.config** file and navigate toohello **<issuerNameRegistry>** block, specifically looking for hello following few lines:</span></span>
   ```
   <issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
   ```
2. <span data-ttu-id="b563f-205">A hello  **<add thumbprint=””>**  bármely karakter helyett egy másikat a beállításban módosítsa az hello ujjlenyomat értékét.</span><span class="sxs-lookup"><span data-stu-id="b563f-205">In hello **<add thumbprint=””>** setting, change hello thumbprint value by replacing any character with a different one.</span></span> <span data-ttu-id="b563f-206">Mentse a hello **Web.config** fájlt.</span><span class="sxs-lookup"><span data-stu-id="b563f-206">Save hello **Web.config** file.</span></span>
3. <span data-ttu-id="b563f-207">Hello alkalmazás létrehozása, és futtassa azt.</span><span class="sxs-lookup"><span data-stu-id="b563f-207">Build hello application, and then run it.</span></span> <span data-ttu-id="b563f-208">Ha hello bejelentkezési folyamat elvégzése az alkalmazás sikeresen frissíti a hello kulcs által hello szükséges adatok letöltése a címtár összevonási metaadatok dokumentumból.</span><span class="sxs-lookup"><span data-stu-id="b563f-208">If you can complete hello sign-in process, your application is successfully updating hello key by downloading hello required information from your directory’s federation metadata document.</span></span> <span data-ttu-id="b563f-209">Ha a probléma jelentkezik be, győződjön meg arról, hello módosítások az alkalmazás helyesek hello olvasásával [hozzáadása bejelentkezés tooYour webes alkalmazás használatával Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) a témakörben vagy letöltése, és tanulmányozza a következő példakód hello: [ Az Azure Active Directory több-Bérlős felhőalapú alkalmazásnál](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).</span><span class="sxs-lookup"><span data-stu-id="b563f-209">If you are having issues signing in, ensure hello changes in your application are correct by reading hello [Adding Sign-On tooYour Web Application Using Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) topic, or downloading and inspecting hello following code sample: [Multi-Tenant Cloud Application for Azure Active Directory](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).</span></span>

### <span data-ttu-id="b563f-210"><a name="vs2010"></a>Erőforrások védelme és a Visual Studio 2008 vagy 2010 webes alkalmazások és a Windows Identity Foundation (WIF) 1.0-s a .NET 3.5</span><span class="sxs-lookup"><span data-stu-id="b563f-210"><a name="vs2010"></a>Web applications protecting resources and created with Visual Studio 2008 or 2010 and Windows Identity Foundation (WIF) v1.0 for .NET 3.5</span></span>
<span data-ttu-id="b563f-211">Ha egy alkalmazás WIF 1.0-s verziója található, nincs nincs megadott mechanizmus tooautomatically frissítése az alkalmazás konfigurációs toouse egy új kulcsot.</span><span class="sxs-lookup"><span data-stu-id="b563f-211">If you built an application on WIF v1.0, there is no provided mechanism tooautomatically refresh your application’s configuration toouse a new key.</span></span>

* <span data-ttu-id="b563f-212">*Legegyszerűbben* hello FedUtil tooling szereplő hello WIF SDK-t, így hello legújabb metaadat-dokumentum beolvasása és a konfiguráció frissítését használja.</span><span class="sxs-lookup"><span data-stu-id="b563f-212">*Easiest way* Use hello FedUtil tooling included in hello WIF SDK, which can retrieve hello latest metadata document and update your configuration.</span></span>
* <span data-ttu-id="b563f-213">Frissítse az alkalmazás too.NET 4.5, amely hello WIF hello rendszer névtérben található legújabb verzióját is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b563f-213">Update your application too.NET 4.5, which includes hello newest version of WIF located in hello System namespace.</span></span> <span data-ttu-id="b563f-214">Hello segítségével [ellenőrzése kibocsátó neve beállításjegyzék (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) tooperform a hello alkalmazást automatikus frissítéseit.</span><span class="sxs-lookup"><span data-stu-id="b563f-214">You can then use hello [Validating Issuer Name Registry (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) tooperform automatic updates of hello application’s configuration.</span></span>
* <span data-ttu-id="b563f-215">Hajtsa végre a manuális váltása hello utasításokat az útmutató végén hello szerint.</span><span class="sxs-lookup"><span data-stu-id="b563f-215">Perform a manual rollover as per hello instructions at hello end of this guidance document.</span></span>

<span data-ttu-id="b563f-216">Toouse hello FedUtil tooupdate a konfigurációs utasításokat:</span><span class="sxs-lookup"><span data-stu-id="b563f-216">Instructions toouse hello FedUtil tooupdate your configuration:</span></span>

1. <span data-ttu-id="b563f-217">Győződjön meg arról, hogy rendelkezik-e hello WIF 1.0-s verziója a fejlesztői gépen telepítve van a Visual Studio 2008 vagy 2010 SDK.</span><span class="sxs-lookup"><span data-stu-id="b563f-217">Verify that you have hello WIF v1.0 SDK installed on your development machine for Visual Studio 2008 or 2010.</span></span> <span data-ttu-id="b563f-218">Is [töltse le innen](https://www.microsoft.com/en-us/download/details.aspx?id=4451) Ha nem telepítette még azt.</span><span class="sxs-lookup"><span data-stu-id="b563f-218">You can [download it from here](https://www.microsoft.com/en-us/download/details.aspx?id=4451) if you have not yet installed it.</span></span>
2. <span data-ttu-id="b563f-219">A Visual Studióban nyissa meg a hello megoldást, kattintson a jobb gombbal a megfelelő projekt hello és válassza a **összevonási metaadatok frissítése**.</span><span class="sxs-lookup"><span data-stu-id="b563f-219">In Visual Studio, open hello solution, and then right-click hello applicable project and select **Update federation metadata**.</span></span> <span data-ttu-id="b563f-220">Ez a beállítás nem érhető el, ha FedUtil és/vagy hello WIF 1.0-s verziójú SDK nem lett telepítve.</span><span class="sxs-lookup"><span data-stu-id="b563f-220">If this option is not available, FedUtil and/or hello WIF v1.0 SDK has not been installed.</span></span>
3. <span data-ttu-id="b563f-221">Hello parancssorból kiválasztása **frissítés** toobegin az összevonási metaadatok frissítése.</span><span class="sxs-lookup"><span data-stu-id="b563f-221">From hello prompt, select **Update** toobegin updating your federation metadata.</span></span> <span data-ttu-id="b563f-222">Ha hozzáférés-toohello server dolgozik, ahol hello alkalmazás tárolása, is használhat FedUtil tartozó [automatikus metaadat-frissítés Feladatütemező](https://msdn.microsoft.com/library/ee517272.aspx).</span><span class="sxs-lookup"><span data-stu-id="b563f-222">If you have access toohello server environment where hello application is hosted, you can optionally use FedUtil’s [automatic metadata update scheduler](https://msdn.microsoft.com/library/ee517272.aspx).</span></span>
4. <span data-ttu-id="b563f-223">Kattintson a **Befejezés** toocomplete hello frissítési folyamatot.</span><span class="sxs-lookup"><span data-stu-id="b563f-223">Click **Finish** toocomplete hello update process.</span></span>

### <span data-ttu-id="b563f-224"><a name="other"></a>Webalkalmazások / API-k erőforrások védelme bármely más szalagtárak, vagy manuálisan végrehajtási bármelyik hello támogatott protokollok</span><span class="sxs-lookup"><span data-stu-id="b563f-224"><a name="other"></a>Web applications / APIs protecting resources using any other libraries or manually implementing any of hello supported protocols</span></span>
<span data-ttu-id="b563f-225">Ha néhány más szalagtár használata, vagy manuálisan megvalósított hello támogatott protokollok, tooreview hello könyvtárban lesz szüksége, vagy a megvalósítás tooensure, amely hello kulcs lekérése folyamatban van a hello OpenID Connect felderítési dokumentum vagy hello összevonási metaadatokat tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="b563f-225">If you are using some other library or manually implemented any of hello supported protocols, you'll need tooreview hello library or your implementation tooensure that hello key is being retrieved from either hello OpenID Connect discovery document or hello federation metadata document.</span></span> <span data-ttu-id="b563f-226">Ez a módosítás nem vonható toocheck toodo egy keresésre a vagy hello könyvtár kódban bármely hívások tooeither hello OpenID felderítési dokumentum vagy hello összevonási metaadat-dokumentum.</span><span class="sxs-lookup"><span data-stu-id="b563f-226">One way toocheck for this is toodo a search in your code or hello library's code for any calls out tooeither hello OpenID discovery document or hello federation metadata document.</span></span>

<span data-ttu-id="b563f-227">Ha ezek a kulcsok tárolása valahol vagy szoftveresen kötött az alkalmazásban, manuálisan beolvasása hello kulcs és a frissítés, ennek megfelelően szerint hajtsa végre egy manuális váltása hello utasításokat az útmutató végén hello szerint.</span><span class="sxs-lookup"><span data-stu-id="b563f-227">If they key is being stored somewhere or hardcoded in your application, you can manually retrieve hello key and update it accordingly by perform a manual rollover as per hello instructions at hello end of this guidance document.</span></span> <span data-ttu-id="b563f-228">**Erősen ajánlott, hogy javítható-e az alkalmazás toosupport automatikus helyettesítő** e hello megközelíti vázlat ezen cikk tooavoid jövőbeli megszakításait és a terhelést, hogy az Azure AD növeli az átfordulási ütemben történik, illetve hogy az vészhelyzeti sávon kívüli kulcsváltás.</span><span class="sxs-lookup"><span data-stu-id="b563f-228">**It is strongly encouraged that you enhance your application toosupport automatic rollover** using any of hello approaches outline in this article tooavoid future disruptions and overhead if Azure AD increases it's rollover cadence or has an emergency out-of-band rollover.</span></span>

## <a name="how-tootest-your-application-toodetermine-if-it-will-be-affected"></a><span data-ttu-id="b563f-229">Hogyan tootest az alkalmazás toodetermine, amennyiben ez érinti</span><span class="sxs-lookup"><span data-stu-id="b563f-229">How tootest your application toodetermine if it will be affected</span></span>
<span data-ttu-id="b563f-230">Ellenőrizheti, hogy az alkalmazás támogatja az automatikus kulcsváltást hello parancsfájlok letöltése és hello utasításait követve [a GitHub-tárházban.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)</span><span class="sxs-lookup"><span data-stu-id="b563f-230">You can validate whether your application supports automatic key rollover by downloading hello scripts and following hello instructions in [this GitHub repository.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)</span></span>

## <a name="how-tooperform-a-manual-rollover-if-you-application-does-not-support-automatic-rollover"></a><span data-ttu-id="b563f-231">Hogyan tooperform Ha alkalmazás nem támogatja az automatikus helyettesítő egy manuális váltása</span><span class="sxs-lookup"><span data-stu-id="b563f-231">How tooperform a manual rollover if you application does not support automatic rollover</span></span>
<span data-ttu-id="b563f-232">Ha az alkalmazás **nem** támogatja az automatikus helyettesítő, szüksége lesz egy folyamat, amely rendszeresen figyeli az Azure AD által aláíró kulcsok és hajt végre egy manuális váltása tooestablish ennek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="b563f-232">If your application does **not** support automatic rollover, you will need tooestablish a process that periodically monitors Azure AD's signing keys and performs a manual rollover accordingly.</span></span> <span data-ttu-id="b563f-233">[A GitHub-tárházban](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) parancsfájlok és útmutatást tartalmaz toodo ez.</span><span class="sxs-lookup"><span data-stu-id="b563f-233">[This GitHub repository](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) contains scripts and instructions on how toodo this.</span></span>

