---
title: "Kulcsváltás bejelentkezés az Azure AD |} Microsoft Docs"
description: "Ez a cikk ismerteti, amelyek az aláírási kulcs átfordulási ajánlott eljárások az Azure Active Directory"
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
ms.openlocfilehash: 228bb9058537af1e4eb38207c376c2eb86aee68c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="signing-key-rollover-in-azure-active-directory"></a><span data-ttu-id="11c2c-103">Az Azure Active Directoryban kulcsváltás aláírása</span><span class="sxs-lookup"><span data-stu-id="11c2c-103">Signing key rollover in Azure Active Directory</span></span>
<span data-ttu-id="11c2c-104">Ez a témakör ismerteti, mit kell tudnia a nyilvános kulcsok biztonsági jogkivonatok aláírásához használt Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="11c2c-104">This topic discusses what you need to know about the public keys that are used in Azure Active Directory (Azure AD) to sign security tokens.</span></span> <span data-ttu-id="11c2c-105">Fontos megjegyezni, hogy rendszeres időközönként, és vészhelyzet esetén a kulcsok helyettesítő volt állítva azonnal.</span><span class="sxs-lookup"><span data-stu-id="11c2c-105">It is important to note that these keys rollover on a periodic basis and, in an emergency, could be rolled over immediately.</span></span> <span data-ttu-id="11c2c-106">Minden alkalmazás, amely használhatja az Azure Active Directory kell tudni programozott módon a kulcsváltás folyamat, vagy a rendszeres manuális helyettesítő-folyamatot.</span><span class="sxs-lookup"><span data-stu-id="11c2c-106">All applications that use Azure AD should be able to programmatically handle the key rollover process or establish a periodic manual rollover process.</span></span> <span data-ttu-id="11c2c-107">Olvasási megértése, hogyan működnek a kulcsokat, továbbra is az alkalmazás a Váltás hatásának értékelése és az alkalmazás frissítésére, vagy kezelje a kulcsváltás, szükség esetén rendszeres manuális váltása folyamatot.</span><span class="sxs-lookup"><span data-stu-id="11c2c-107">Continue reading to understand how the keys work, how to assess the impact of the rollover to your application and how to update your application or establish a periodic manual rollover process to handle key rollover if necessary.</span></span>

## <a name="overview-of-signing-keys-in-azure-ad"></a><span data-ttu-id="11c2c-108">Az Azure AD-aláírókulcsok áttekintése</span><span class="sxs-lookup"><span data-stu-id="11c2c-108">Overview of signing keys in Azure AD</span></span>
<span data-ttu-id="11c2c-109">Az Azure AD és az azt használó alkalmazások között megbízhatósági kapcsolat létrehozása az ipari szabványok épülő nyilvános kulcsú titkosítást használ.</span><span class="sxs-lookup"><span data-stu-id="11c2c-109">Azure AD uses public-key cryptography built on industry standards to establish trust between itself and the applications that use it.</span></span> <span data-ttu-id="11c2c-110">Gyakorlatilag, ez a következő módon működik: az Azure AD nyilvános és titkos kulcsból álló kulcspárt tartalmazó aláíró kulcsot használ.</span><span class="sxs-lookup"><span data-stu-id="11c2c-110">In practical terms, this works in the following way: Azure AD uses a signing key that consists of a public and private key pair.</span></span> <span data-ttu-id="11c2c-111">Ha egy felhasználó jelentkezik be valamely alkalmazás az Azure AD használ a hitelesítéshez, az Azure AD létrehoz egy biztonsági jogkivonatot, amely a felhasználó adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="11c2c-111">When a user signs in to an application that uses Azure AD for authentication, Azure AD creates a security token that contains information about the user.</span></span> <span data-ttu-id="11c2c-112">Ez a token aláírta az Azure AD használatával a titkos kulcsot, az alkalmazásnak elküldés előtt.</span><span class="sxs-lookup"><span data-stu-id="11c2c-112">This token is signed by Azure AD using its private key before it is sent back to the application.</span></span> <span data-ttu-id="11c2c-113">Győződjön meg arról, hogy a jogkivonat érvényes, és ténylegesen kezdeményezésű az Azure AD, hogy az alkalmazás ellenőrizni kell a jogkivonat aláírása a nyilvános kulccsal, hogy a bérlő tartalmazza az Azure AD által elérhetővé tett [OpenID Connect felderítési dokumentum](http://openid.net/specs/openid-connect-discovery-1_0.html) vagy SAML/WS-Fed [összevonási metaadat-dokumentum](active-directory-federation-metadata.md).</span><span class="sxs-lookup"><span data-stu-id="11c2c-113">To verify that the token is valid and actually originated from Azure AD, the application must validate the token’s signature using the public key exposed by Azure AD that is contained in the tenant’s [OpenID Connect discovery document](http://openid.net/specs/openid-connect-discovery-1_0.html) or SAML/WS-Fed [federation metadata document](active-directory-federation-metadata.md).</span></span>

<span data-ttu-id="11c2c-114">Biztonsági okokból az Azure AD-aláíró kulcs összesíti rendszeres időközönként, és vészhelyzet esetén volt állítva azonnal.</span><span class="sxs-lookup"><span data-stu-id="11c2c-114">For security purposes, Azure AD’s signing key rolls on a periodic basis and, in the case of an emergency, could be rolled over immediately.</span></span> <span data-ttu-id="11c2c-115">Bármely alkalmazás, amely az Azure AD kezelni egy kulcsváltás eseményt, függetlenül attól, milyen gyakran ez akkor fordulhat elő kell készíteni.</span><span class="sxs-lookup"><span data-stu-id="11c2c-115">Any application that integrates with Azure AD should be prepared to handle a key rollover event no matter how frequently it may occur.</span></span> <span data-ttu-id="11c2c-116">Ha nem, és az alkalmazás megpróbálja ellenőrzése a jogkivonat aláírása lejárt-kulcsot használ, a bejelentkezési kérelem sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="11c2c-116">If it doesn’t, and your application attempts to use an expired key to verify the signature on a token, the sign-in request will fail.</span></span>

<span data-ttu-id="11c2c-117">Mindig egynél több érvényes kulcs rendelkezésre áll az OpenID Connect felderítés és az összevonási metaadatok dokumentum.</span><span class="sxs-lookup"><span data-stu-id="11c2c-117">There is always more than one valid key available in the OpenID Connect discovery document and the federation metadata document.</span></span> <span data-ttu-id="11c2c-118">Az alkalmazás minden a dokumentumban szereplő kulcsok használatához felkészültnek kell lennie, egy kulcs amint lehet, hogy lesz vonva, mivel egy másik előfordulhat, hogy azok helyettesítéseivel, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="11c2c-118">Your application should be prepared to use any of the keys specified in the document, since one key may be rolled soon, another may be its replacement, and so forth.</span></span>

## <a name="how-to-assess-if-your-application-will-be-affected-and-what-to-do-about-it"></a><span data-ttu-id="11c2c-119">Annak ellenőrzéséhez, ha az alkalmazás az érintett hogyan és mit kell tenni információk</span><span class="sxs-lookup"><span data-stu-id="11c2c-119">How to assess if your application will be affected and what to do about it</span></span>
<span data-ttu-id="11c2c-120">Az alkalmazás kezelésének kulcsváltás attól függ, hogy változók, például az alkalmazás-vagy milyen identitás protokoll és a könyvtár lett megadva.</span><span class="sxs-lookup"><span data-stu-id="11c2c-120">How your application handles key rollover depends on variables such as the type of application or what identity protocol and library was used.</span></span> <span data-ttu-id="11c2c-121">Az alábbi szakaszok ellenőrzéséhez, hogy a leggyakoribb alkalmazások is hatással van a kulcsváltás, és útmutatást nyújtanak az alkalmazás automatikus helyettesítő támogatásához, vagy manuálisan frissítse a kulcs frissítése.</span><span class="sxs-lookup"><span data-stu-id="11c2c-121">The sections below assess whether the most common types of applications are impacted by the key rollover and provide guidance on how to update the application to support automatic rollover or manually update the key.</span></span>

* [<span data-ttu-id="11c2c-122">Natív ügyfélalkalmazások erőforrások elérése</span><span class="sxs-lookup"><span data-stu-id="11c2c-122">Native client applications accessing resources</span></span>](#nativeclient)
* [<span data-ttu-id="11c2c-123">Webalkalmazások / API-k erőforrások elérése</span><span class="sxs-lookup"><span data-stu-id="11c2c-123">Web applications / APIs accessing resources</span></span>](#webclient)
* [<span data-ttu-id="11c2c-124">Webalkalmazások / API-k erőforrások védelméről, és segítségével az Azure App Service szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="11c2c-124">Web applications / APIs protecting resources and built using Azure App Services</span></span>](#appservices)
* [<span data-ttu-id="11c2c-125">Webalkalmazások / API-k használata a .NET OWIN OpenID Connect, WS-Fed vagy WindowsAzureActiveDirectoryBearerAuthentication köztes erőforrások védelme</span><span class="sxs-lookup"><span data-stu-id="11c2c-125">Web applications / APIs protecting resources using .NET OWIN OpenID Connect, WS-Fed or WindowsAzureActiveDirectoryBearerAuthentication middleware</span></span>](#owin)
* [<span data-ttu-id="11c2c-126">Webalkalmazások / API-k használata a .NET Core OpenID Connect vagy JwtBearerAuthentication köztes erőforrások védelme</span><span class="sxs-lookup"><span data-stu-id="11c2c-126">Web applications / APIs protecting resources using .NET Core OpenID Connect or  JwtBearerAuthentication middleware</span></span>](#owincore)
* [<span data-ttu-id="11c2c-127">Webalkalmazások / API-k Node.js passport-azure-ad-modullal erőforrások védelme</span><span class="sxs-lookup"><span data-stu-id="11c2c-127">Web applications / APIs protecting resources using Node.js passport-azure-ad module</span></span>](#passport)
* [<span data-ttu-id="11c2c-128">Webalkalmazások / API-k erőforrások védelméről, és a Visual Studio 2015-öt vagy a Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="11c2c-128">Web applications / APIs protecting resources and created with Visual Studio 2015 or Visual Studio 2017</span></span>](#vs2015)
* [<span data-ttu-id="11c2c-129">Webalkalmazások erőforrások védelméről, és a Visual Studio 2013 létre</span><span class="sxs-lookup"><span data-stu-id="11c2c-129">Web applications protecting resources and created with Visual Studio 2013</span></span>](#vs2013)
* [<span data-ttu-id="11c2c-130">Webes API-k erőforrások védelméről, és a Visual Studio 2013 létre</span><span class="sxs-lookup"><span data-stu-id="11c2c-130">Web APIs protecting resources and created with Visual Studio 2013</span></span>](#vs2013_webapi)
* [<span data-ttu-id="11c2c-131">Erőforrások védelme és a Visual Studio 2012 létrehozott webes alkalmazások</span><span class="sxs-lookup"><span data-stu-id="11c2c-131">Web applications protecting resources and created with Visual Studio 2012</span></span>](#vs2012)
* [<span data-ttu-id="11c2c-132">Webalkalmazások erőforrások védelméről, és a Visual Studio 2010, a Windows Identity Foundation használatával 2008 o létre</span><span class="sxs-lookup"><span data-stu-id="11c2c-132">Web applications protecting resources and created with Visual Studio 2010, 2008 o using Windows Identity Foundation</span></span>](#vs2010)
* [<span data-ttu-id="11c2c-133">Webalkalmazások / API-k bármely más könyvtárak segítségével, vagy manuálisan végrehajtási a támogatott protokollok erőforrások védelme</span><span class="sxs-lookup"><span data-stu-id="11c2c-133">Web applications / APIs protecting resources using any other libraries or manually implementing any of the supported protocols</span></span>](#other)

<span data-ttu-id="11c2c-134">Ez az útmutató **nem** alkalmazható:</span><span class="sxs-lookup"><span data-stu-id="11c2c-134">This guidance is **not** applicable for:</span></span>

* <span data-ttu-id="11c2c-135">Az Azure AD Application Gallery (beleértve az egyéni) hozzáadott alkalmazások rendelkeznek jelenítik aláírókulcsok külön útmutatást.</span><span class="sxs-lookup"><span data-stu-id="11c2c-135">Applications added from Azure AD Application Gallery (including Custom) have separate guidance with regards to signing keys.</span></span> [<span data-ttu-id="11c2c-136">További információt.</span><span class="sxs-lookup"><span data-stu-id="11c2c-136">More information.</span></span>](../active-directory-sso-certs.md)
* <span data-ttu-id="11c2c-137">A helyszíni alkalmazásproxy keresztül közzétett alkalmazás nem kell foglalkoznia az aláírási kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="11c2c-137">On-premises applications published via application proxy don't have to worry about signing keys.</span></span>

### <span data-ttu-id="11c2c-138"><a name="nativeclient"></a>Natív ügyfélalkalmazások erőforrások elérése</span><span class="sxs-lookup"><span data-stu-id="11c2c-138"><a name="nativeclient"></a>Native client applications accessing resources</span></span>
<span data-ttu-id="11c2c-139">Erőforrások (azaz csak elérő alkalmazások</span><span class="sxs-lookup"><span data-stu-id="11c2c-139">Applications that are only accessing resources (i.e</span></span> <span data-ttu-id="11c2c-140">A Microsoft Graph, KeyVault, Outlook API és más Microsoft APIs) általában csak jogkivonat beszerzése, és adja át mentén az erőforrás tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="11c2c-140">Microsoft Graph, KeyVault, Outlook API and other Microsoft APIs) generally only obtain a token and pass it along to the resource owner.</span></span> <span data-ttu-id="11c2c-141">Figyelembe véve, hogy azok nem védett erőforrások, nem vizsgálja meg a jogkivonatot, és emiatt nem kell annak érdekében, hogy helyesen alá van írva.</span><span class="sxs-lookup"><span data-stu-id="11c2c-141">Given that they are not protecting any resources, they do not inspect the token and therefore do not need to ensure it is properly signed.</span></span>

<span data-ttu-id="11c2c-142">Natív ügyfélalkalmazások, asztali vagy hordozható, ebbe a kategóriába tartozik, és így nem érinti a váltáshoz.</span><span class="sxs-lookup"><span data-stu-id="11c2c-142">Native client applications, whether desktop or mobile, fall into this category and are thus not impacted by the rollover.</span></span>

### <span data-ttu-id="11c2c-143"><a name="webclient"></a>Webalkalmazások / API-k erőforrások elérése</span><span class="sxs-lookup"><span data-stu-id="11c2c-143"><a name="webclient"></a>Web applications / APIs accessing resources</span></span>
<span data-ttu-id="11c2c-144">Erőforrások (azaz csak elérő alkalmazások</span><span class="sxs-lookup"><span data-stu-id="11c2c-144">Applications that are only accessing resources (i.e</span></span> <span data-ttu-id="11c2c-145">A Microsoft Graph, KeyVault, Outlook API és más Microsoft APIs) általában csak jogkivonat beszerzése, és adja át mentén az erőforrás tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="11c2c-145">Microsoft Graph, KeyVault, Outlook API and other Microsoft APIs) generally only obtain a token and pass it along to the resource owner.</span></span> <span data-ttu-id="11c2c-146">Figyelembe véve, hogy azok nem védett erőforrások, nem vizsgálja meg a jogkivonatot, és emiatt nem kell annak érdekében, hogy helyesen alá van írva.</span><span class="sxs-lookup"><span data-stu-id="11c2c-146">Given that they are not protecting any resources, they do not inspect the token and therefore do not need to ensure it is properly signed.</span></span>

<span data-ttu-id="11c2c-147">Webalkalmazások és webes API-khoz, az alkalmazás csak a folyamat által használt (ügyfél-hitelesítő adatok / ügyféltanúsítvány), ebbe a kategóriába tartozik, és így nem érinti a váltáshoz.</span><span class="sxs-lookup"><span data-stu-id="11c2c-147">Web applications and web APIs that are using the app-only flow (client credentials / client certificate), fall into this category and are thus not impacted by the rollover.</span></span>

### <span data-ttu-id="11c2c-148"><a name="appservices"></a>Webalkalmazások / API-k erőforrások védelméről, és segítségével az Azure App Service szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="11c2c-148"><a name="appservices"></a>Web applications / APIs protecting resources and built using Azure App Services</span></span>
<span data-ttu-id="11c2c-149">Az Azure App Services hitelesítési / engedélyezési (EasyAuth) funkció már kulcsváltás automatikusan kezeléséhez szükséges logika.</span><span class="sxs-lookup"><span data-stu-id="11c2c-149">Azure App Services' Authentication / Authorization (EasyAuth) functionality already has the necessary logic to handle key rollover automatically.</span></span>

### <span data-ttu-id="11c2c-150"><a name="owin"></a>Webalkalmazások / API-k használata a .NET OWIN OpenID Connect, WS-Fed vagy WindowsAzureActiveDirectoryBearerAuthentication köztes erőforrások védelme</span><span class="sxs-lookup"><span data-stu-id="11c2c-150"><a name="owin"></a>Web applications / APIs protecting resources using .NET OWIN OpenID Connect, WS-Fed or WindowsAzureActiveDirectoryBearerAuthentication middleware</span></span>
<span data-ttu-id="11c2c-151">Ha az alkalmazás nem használja a .NET OWIN OpenID Connect, WS-Fed vagy WindowsAzureActiveDirectoryBearerAuthentication köztes kulcsváltás automatikusan kezeléséhez szükséges logika már rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="11c2c-151">If your application is using the .NET OWIN OpenID Connect, WS-Fed or WindowsAzureActiveDirectoryBearerAuthentication middleware, it already has the necessary logic to handle key rollover automatically.</span></span>

<span data-ttu-id="11c2c-152">Úgy ellenőrizheti, hogy az alkalmazás ezek alapján az alkalmazás Startup.cs vagy Startup.Auth.cs alábbi kódtöredékek bármelyikéhez használ</span><span class="sxs-lookup"><span data-stu-id="11c2c-152">You can confirm that your application is using any of these by looking for any of the following snippets in your application's Startup.cs or Startup.Auth.cs</span></span>

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

### <span data-ttu-id="11c2c-153"><a name="owincore"></a>Webalkalmazások / API-k használata a .NET Core OpenID Connect vagy JwtBearerAuthentication köztes erőforrások védelme</span><span class="sxs-lookup"><span data-stu-id="11c2c-153"><a name="owincore"></a>Web applications / APIs protecting resources using .NET Core OpenID Connect or  JwtBearerAuthentication middleware</span></span>
<span data-ttu-id="11c2c-154">Ha az alkalmazás nem használja a .NET Core OWIN OpenID Connect vagy JwtBearerAuthentication köztes, kulcsváltás automatikusan kezeléséhez szükséges logika már rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="11c2c-154">If your application is using the .NET Core OWIN OpenID Connect  or JwtBearerAuthentication middleware, it already has the necessary logic to handle key rollover automatically.</span></span>

<span data-ttu-id="11c2c-155">Úgy ellenőrizheti, hogy az alkalmazás ezek alapján az alkalmazás Startup.cs vagy Startup.Auth.cs alábbi kódtöredékek bármelyikéhez használ</span><span class="sxs-lookup"><span data-stu-id="11c2c-155">You can confirm that your application is using any of these by looking for any of the following snippets in your application's Startup.cs or Startup.Auth.cs</span></span>

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

### <span data-ttu-id="11c2c-156"><a name="passport"></a>Webalkalmazások / API-k Node.js passport-azure-ad-modullal erőforrások védelme</span><span class="sxs-lookup"><span data-stu-id="11c2c-156"><a name="passport"></a>Web applications / APIs protecting resources using Node.js passport-azure-ad module</span></span>
<span data-ttu-id="11c2c-157">Ha az alkalmazás nem használja a Node.js passport-ad-modullal, kulcsváltás automatikusan kezeléséhez szükséges logika már rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="11c2c-157">If your application is using the Node.js passport-ad module, it already has the necessary logic to handle key rollover automatically.</span></span>

<span data-ttu-id="11c2c-158">Úgy ellenőrizheti, hogy az alkalmazás passport-ad a következő kódrészletet a az alkalmazás app.js keresése</span><span class="sxs-lookup"><span data-stu-id="11c2c-158">You can confirm that your application passport-ad by searching for the following snippet in your application's app.js</span></span>

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <span data-ttu-id="11c2c-159"><a name="vs2015"></a>Webalkalmazások / API-k erőforrások védelméről, és a Visual Studio 2015-öt vagy a Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="11c2c-159"><a name="vs2015"></a>Web applications / APIs protecting resources and created with Visual Studio 2015 or Visual Studio 2017</span></span>
<span data-ttu-id="11c2c-160">Ha az alkalmazást a Visual Studio 2015-öt vagy a Visual Studio 2017 webes alkalmazás sablon használatával lett létrehozva, és a kiválasztott **munkahelyi és iskolai fiókok** a a **hitelesítés módosítása** menü már rendelkezik a szükséges logika kulcsváltás automatikusan kezelni.</span><span class="sxs-lookup"><span data-stu-id="11c2c-160">If your application was built using a web application template in Visual Studio 2015 or Visual Studio 2017 and you selected **Work And School Accounts** from the **Change Authentication** menu, it already has the necessary logic to handle key rollover automatically.</span></span> <span data-ttu-id="11c2c-161">A programot, az OWIN OpenID Connect köztes ágyazott kéri le, és gyorsítótárba helyezi azt a kulcsokat az OpenID Connect felderítési dokumentum, és rendszeresen frissülnek.</span><span class="sxs-lookup"><span data-stu-id="11c2c-161">This logic, embedded in the OWIN OpenID Connect middleware, retrieves and caches the keys from the OpenID Connect discovery document and periodically refreshes them.</span></span>

<span data-ttu-id="11c2c-162">Ha manuálisan adta hozzá hitelesítési a megoldáshoz, az alkalmazás esetleg nincs szükség kulcsváltás logika.</span><span class="sxs-lookup"><span data-stu-id="11c2c-162">If you added authentication to your solution manually, your application might not have the necessary key rollover logic.</span></span> <span data-ttu-id="11c2c-163">Jegyezze meg, vagy kövesse a kell [webalkalmazások / API-k bármely más könyvtárak segítségével, vagy manuálisan végrehajtási a támogatott protokollok.](#other).</span><span class="sxs-lookup"><span data-stu-id="11c2c-163">You will need to write it yourself, or follow the steps in [Web applications / APIs using any other libraries or manually implementing any of the supported protocols.](#other).</span></span>

### <span data-ttu-id="11c2c-164"><a name="vs2013"></a>Webalkalmazások erőforrások védelméről, és a Visual Studio 2013 létre</span><span class="sxs-lookup"><span data-stu-id="11c2c-164"><a name="vs2013"></a>Web applications protecting resources and created with Visual Studio 2013</span></span>
<span data-ttu-id="11c2c-165">Ha az alkalmazás a Visual Studio 2013 webes alkalmazás sablon használatával lett létrehozva, és a kiválasztott **munkahelyi és iskolai fiókok** a a **hitelesítés módosítása** menü már rendelkezik a szükséges logika kulcsváltás automatikusan kezelni.</span><span class="sxs-lookup"><span data-stu-id="11c2c-165">If your application was built using a web application template in Visual Studio 2013 and you selected **Organizational Accounts** from the **Change Authentication** menu, it already has the necessary logic to handle key rollover automatically.</span></span> <span data-ttu-id="11c2c-166">A működési elvet tárolja a projekthez tartozó két adatbázistáblák a szervezet egyedi azonosító és az aláírási kulcs adataira.</span><span class="sxs-lookup"><span data-stu-id="11c2c-166">This logic stores your organization’s unique identifier and the signing key information in two database tables associated with the project.</span></span> <span data-ttu-id="11c2c-167">A kapcsolati karakterláncot az adatbázishoz a projekt Web.config fájlban található.</span><span class="sxs-lookup"><span data-stu-id="11c2c-167">You can find the connection string for the database in the project’s Web.config file.</span></span>

<span data-ttu-id="11c2c-168">Ha manuálisan adta hozzá hitelesítési a megoldáshoz, az alkalmazás esetleg nincs szükség kulcsváltás logika.</span><span class="sxs-lookup"><span data-stu-id="11c2c-168">If you added authentication to your solution manually, your application might not have the necessary key rollover logic.</span></span> <span data-ttu-id="11c2c-169">Jegyezze meg, vagy kövesse a kell [webalkalmazások / API-k bármely más könyvtárak segítségével, vagy manuálisan végrehajtási a támogatott protokollok.](#other).</span><span class="sxs-lookup"><span data-stu-id="11c2c-169">You will need to write it yourself, or follow the steps in [Web applications / APIs using any other libraries or manually implementing any of the supported protocols.](#other).</span></span>

<span data-ttu-id="11c2c-170">A következő lépésekkel ellenőrizze, hogy a programot az alkalmazás megfelelően működik.</span><span class="sxs-lookup"><span data-stu-id="11c2c-170">The following steps will help you verify that the logic is working properly in your application.</span></span>

1. <span data-ttu-id="11c2c-171">A Visual Studio 2013, nyissa meg a megoldást, és kattintson a a **Server Explorer** a jobb oldali lapján.</span><span class="sxs-lookup"><span data-stu-id="11c2c-171">In Visual Studio 2013, open the solution, and then click on the **Server Explorer** tab on the right window.</span></span>
2. <span data-ttu-id="11c2c-172">Bontsa ki a **adatkapcsolatok**, **DefaultConnection**, majd **táblák**.</span><span class="sxs-lookup"><span data-stu-id="11c2c-172">Expand **Data Connections**, **DefaultConnection**, and then **Tables**.</span></span> <span data-ttu-id="11c2c-173">Keresse meg a **IssuingAuthorityKeys** tábla, kattintson a jobb gombbal, és kattintson a **tábla adatok megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="11c2c-173">Locate the **IssuingAuthorityKeys** table, right-click it, and then click **Show Table Data**.</span></span>
3. <span data-ttu-id="11c2c-174">Az a **IssuingAuthorityKeys** table, legalább egy sort, amely megfelel az ujjlenyomat értékének lesz.</span><span class="sxs-lookup"><span data-stu-id="11c2c-174">In the **IssuingAuthorityKeys** table, there will be at least one row, which corresponds to the thumbprint value for the key.</span></span> <span data-ttu-id="11c2c-175">Törli a sorokat a táblában.</span><span class="sxs-lookup"><span data-stu-id="11c2c-175">Delete any rows in the table.</span></span>
4. <span data-ttu-id="11c2c-176">Kattintson a jobb gombbal a **bérlők** tábla, és kattintson a **tábla adatok megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="11c2c-176">Right-click the **Tenants** table, and then click **Show Table Data**.</span></span>
5. <span data-ttu-id="11c2c-177">Az a **bérlők** table, legalább egy sort, amely felel meg egy egyedi könyvtárat bérlő azonosítója lesz.</span><span class="sxs-lookup"><span data-stu-id="11c2c-177">In the **Tenants** table, there will be at least one row, which corresponds to a unique directory tenant identifier.</span></span> <span data-ttu-id="11c2c-178">Törli a sorokat a táblában.</span><span class="sxs-lookup"><span data-stu-id="11c2c-178">Delete any rows in the table.</span></span> <span data-ttu-id="11c2c-179">Ha nem törli a sorokat is a **bérlők** tábla és **IssuingAuthorityKeys** tábla, akkor hibaüzenet fog futásidőben.</span><span class="sxs-lookup"><span data-stu-id="11c2c-179">If you don't delete the rows in both the **Tenants** table and **IssuingAuthorityKeys** table, you will get an error at runtime.</span></span>
6. <span data-ttu-id="11c2c-180">Hozza létre, és futtassa az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="11c2c-180">Build and run the application.</span></span> <span data-ttu-id="11c2c-181">Rendelkezik bejelentkezést követően a fiókjához, leállíthatja, az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="11c2c-181">After you have logged in to your account, you can stop the application.</span></span>
7. <span data-ttu-id="11c2c-182">Lépjen vissza a **Server Explorer** , és tekintse meg az értékeket a **IssuingAuthorityKeys** és **bérlők** tábla.</span><span class="sxs-lookup"><span data-stu-id="11c2c-182">Return to the **Server Explorer** and look at the values in the **IssuingAuthorityKeys** and **Tenants** table.</span></span> <span data-ttu-id="11c2c-183">Láthatja, hogy azok rendelkeznek lett automatikusan tölteni az összevonási metaadatok dokumentumból a megfelelő információkkal.</span><span class="sxs-lookup"><span data-stu-id="11c2c-183">You’ll notice that they have been automatically repopulated with the appropriate information from the federation metadata document.</span></span>

### <span data-ttu-id="11c2c-184"><a name="vs2013"></a>Webes API-k erőforrások védelméről, és a Visual Studio 2013 létre</span><span class="sxs-lookup"><span data-stu-id="11c2c-184"><a name="vs2013"></a>Web APIs protecting resources and created with Visual Studio 2013</span></span>
<span data-ttu-id="11c2c-185">Ha a webes API-alkalmazás létrehozása a Visual Studio 2013 a webes API-sablonnal, és akkor kiválasztva **munkahelyi és iskolai fiókok** a a **hitelesítés módosítása** menü, akkor már rendelkezik a szükséges az alkalmazás logikáját.</span><span class="sxs-lookup"><span data-stu-id="11c2c-185">If you created a web API application in Visual Studio 2013 using the Web API template, and then selected **Organizational Accounts** from the **Change Authentication** menu, you already have the necessary logic in your application.</span></span>

<span data-ttu-id="11c2c-186">Ha manuálisan konfigurált hitelesítést, kövesse az alábbi áttekintésével megismerheti, hogyan konfigurálhatja a webes API-t automatikusan frissíteni az kulcs adatait.</span><span class="sxs-lookup"><span data-stu-id="11c2c-186">If you manually configured authentication, follow the instructions below to learn how to configure your Web API to automatically update its key information.</span></span>

<span data-ttu-id="11c2c-187">A következő kódrészletet bemutatja, hogyan kell a legújabb kulcsok beszerzése az összevonási metaadatok dokumentum, és használja a [JWT jogkivonat-kezelő](https://msdn.microsoft.com/library/dn205065.aspx) ellenőrzése a jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="11c2c-187">The following code snippet demonstrates how to get the latest keys from the federation metadata document, and then use the [JWT Token Handler](https://msdn.microsoft.com/library/dn205065.aspx) to validate the token.</span></span> <span data-ttu-id="11c2c-188">A kódrészletet feltételezi, hogy használni a saját gyorsítótárazást megőrzése a kulcsot a jövőbeli érvényesítse az Azure AD legyenek az adatbázis, a konfigurációs fájlban vagy máshol.</span><span class="sxs-lookup"><span data-stu-id="11c2c-188">The code snippet assumes that you will use your own caching mechanism for persisting the key to validate future tokens from Azure AD, whether it be in a database, configuration file, or elsewhere.</span></span>

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

        // Validates the JWT Token that's part of the Authorization header in an HTTP request.
        public void ValidateJwtToken(string token)
        {
            JwtSecurityTokenHandler tokenHandler = new JwtSecurityTokenHandler()
            {
                // Do not disable for production code
                CertificateValidator = X509CertificateValidator.None
            };

            TokenValidationParameters validationParams = new TokenValidationParameters()
            {
                AllowedAudience = "[Your App ID URI goes here, as registered in the Azure Classic Portal]",
                ValidIssuer = "[The issuer for the token goes here, such as https://sts.windows.net/68b98905-130e-4d7c-b6e1-a158a9ed8449/]",
                SigningTokens = GetSigningCertificates(MetadataAddress)

                // Cache the signing tokens by your desired mechanism
            };

            Thread.CurrentPrincipal = tokenHandler.ValidateToken(token, validationParams);
        }

        // Returns a list of certificates from the specified metadata document.
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
                        throw new InvalidOperationException("There is no RoleDescriptor of type SecurityTokenServiceType in the metadata");
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

### <span data-ttu-id="11c2c-189"><a name="vs2012"></a>Erőforrások védelme és a Visual Studio 2012 létrehozott webes alkalmazások</span><span class="sxs-lookup"><span data-stu-id="11c2c-189"><a name="vs2012"></a>Web applications protecting resources and created with Visual Studio 2012</span></span>
<span data-ttu-id="11c2c-190">Ha az alkalmazást a Visual Studio 2012 lett létrehozva, valószínűleg az identitás- és hozzáférés eszköz konfigurálására szolgáló alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="11c2c-190">If your application was built in Visual Studio 2012, you probably used the Identity and Access Tool to configure your application.</span></span> <span data-ttu-id="11c2c-191">Is valószínű, hogy használja a [ellenőrzése kibocsátó neve beállításjegyzék (VINR)](https://msdn.microsoft.com/library/dn205067.aspx).</span><span class="sxs-lookup"><span data-stu-id="11c2c-191">It’s also likely that you are using the [Validating Issuer Name Registry (VINR)](https://msdn.microsoft.com/library/dn205067.aspx).</span></span> <span data-ttu-id="11c2c-192">A VINR megbízható identitás-szolgáltatóktól (az Azure AD) kapcsolatos információkat, valamint az általuk kiállított jogkivonatokat érvényesítéséhez használt kulcsok karbantartásáért felelős.</span><span class="sxs-lookup"><span data-stu-id="11c2c-192">The VINR is responsible for maintaining information about trusted identity providers (Azure AD) and the keys used to validate tokens issued by them.</span></span> <span data-ttu-id="11c2c-193">A VINR is megkönnyíti, hogy automatikusan frissíteni a Web.config fájlban úgy, hogy letölti a legújabb összevonási metaadat-dokumentum társított a címtárban tárolt kulcs adatainak ellenőrzése, ha a konfiguráció a legújabb dokumentumon elavult és frissíteni az alkalmazást az új kulcs használatához szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="11c2c-193">The VINR also makes it easy to automatically update the key information stored in a Web.config file by downloading the latest federation metadata document associated with your directory, checking if the configuration is out of date with the latest document, and updating the application to use the new key as necessary.</span></span>

<span data-ttu-id="11c2c-194">A Kódminták vagy a Microsoft által biztosított forgatókönyv dokumentáció segítségével az alkalmazás hozott létre, ha a kulcsváltás logika már szerepel a projekthez.</span><span class="sxs-lookup"><span data-stu-id="11c2c-194">If you created your application using any of the code samples or walkthrough documentation provided by Microsoft, the key rollover logic is already included in your project.</span></span> <span data-ttu-id="11c2c-195">Megfigyelheti, hogy az alábbi kódot a projekt már létezik.</span><span class="sxs-lookup"><span data-stu-id="11c2c-195">You will notice that the code below already exists in your project.</span></span> <span data-ttu-id="11c2c-196">Ha az alkalmazás nem rendelkezik a programot, az alábbi lépésekkel veheti fel, és ellenőrizze, hogy megfelelően működik-e.</span><span class="sxs-lookup"><span data-stu-id="11c2c-196">If your application does not already have this logic, follow the steps below to add it and to verify that it’s working correctly.</span></span>

1. <span data-ttu-id="11c2c-197">A **Megoldáskezelőben**, adjon hozzá egy hivatkozást, a **System.IdentityModel** szerelvény a megfelelő projektet.</span><span class="sxs-lookup"><span data-stu-id="11c2c-197">In **Solution Explorer**, add a reference to the **System.IdentityModel** assembly for the appropriate project.</span></span>
2. <span data-ttu-id="11c2c-198">Nyissa meg a **Global.asax.cs** fájlt, és adja hozzá a következő irányelvek segítségével:</span><span class="sxs-lookup"><span data-stu-id="11c2c-198">Open the **Global.asax.cs** file and add the following using directives:</span></span>
   ```
   using System.Configuration;
   using System.IdentityModel.Tokens;
   ```
3. <span data-ttu-id="11c2c-199">Adja hozzá a következő metódust a **Global.asax.cs** fájlt:</span><span class="sxs-lookup"><span data-stu-id="11c2c-199">Add the following method to the **Global.asax.cs** file:</span></span>
   ```
   protected void RefreshValidationSettings()
   {
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
   }
   ```
4. <span data-ttu-id="11c2c-200">Hívása a **RefreshValidationSettings()** metódust a **Application_Start()** metódus a **Global.asax.cs** látható módon:</span><span class="sxs-lookup"><span data-stu-id="11c2c-200">Invoke the **RefreshValidationSettings()** method in the **Application_Start()** method in **Global.asax.cs** as shown:</span></span>
   ```
   protected void Application_Start()
   {
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
   }
   ```

<span data-ttu-id="11c2c-201">Ha követte ezeket a lépéseket, az alkalmazás Web.config frissíti a dokumentumból összevonási metaadatok, többek között a legújabb kulcsok a legújabb adatokkal.</span><span class="sxs-lookup"><span data-stu-id="11c2c-201">Once you have followed these steps, your application’s Web.config will be updated with the latest information from the federation metadata document, including the latest keys.</span></span> <span data-ttu-id="11c2c-202">A frissítés történik, minden alkalommal, amikor az alkalmazáskészlet újraindul, az IIS-ben; Alapértelmezés szerint az IIS újrahasznosítása alkalmazások 29 óránként van beállítva.</span><span class="sxs-lookup"><span data-stu-id="11c2c-202">This update will occur every time your application pool recycles in IIS; by default IIS is set to recycle applications every 29 hours.</span></span>

<span data-ttu-id="11c2c-203">Győződjön meg arról, hogy működik-e a kulcsváltás programot az alábbi lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="11c2c-203">Follow the steps below to verify that the key rollover logic is working.</span></span>

1. <span data-ttu-id="11c2c-204">Miután ellenőrizte, hogy az alkalmazás a fenti olyan helykódot alkalmaz, nyissa meg a **Web.config** fájlt, és keresse meg a  **<issuerNameRegistry>**  blokk, kifejezetten keres a következő néhány sor:</span><span class="sxs-lookup"><span data-stu-id="11c2c-204">After you have verified that your application is using the code above, open the **Web.config** file and navigate to the **<issuerNameRegistry>** block, specifically looking for the following few lines:</span></span>
   ```
   <issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
   ```
2. <span data-ttu-id="11c2c-205">Az a  **<add thumbprint=””>**  beállításban módosítsa az ujjlenyomat értékét azáltal, hogy egy másik bármely karakter.</span><span class="sxs-lookup"><span data-stu-id="11c2c-205">In the **<add thumbprint=””>** setting, change the thumbprint value by replacing any character with a different one.</span></span> <span data-ttu-id="11c2c-206">Mentse a **Web.config** fájlt.</span><span class="sxs-lookup"><span data-stu-id="11c2c-206">Save the **Web.config** file.</span></span>
3. <span data-ttu-id="11c2c-207">Építenie az alkalmazást, és futtassa azt.</span><span class="sxs-lookup"><span data-stu-id="11c2c-207">Build the application, and then run it.</span></span> <span data-ttu-id="11c2c-208">Ha a bejelentkezési folyamat elvégzése az alkalmazás sikeresen frissíti a kulcs úgy, hogy letölti a szükséges adatokat a címtár összevonási metaadatok dokumentumból.</span><span class="sxs-lookup"><span data-stu-id="11c2c-208">If you can complete the sign-in process, your application is successfully updating the key by downloading the required information from your directory’s federation metadata document.</span></span> <span data-ttu-id="11c2c-209">Ha bejelentkezik problémát tapasztal, győződjön meg arról, a módosítások az alkalmazás olvasásával helyesek a [hozzáadása bejelentkezés a webes alkalmazás használata az Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) a témakörben vagy letöltése, és tanulmányozza a következő példakód: [ Az Azure Active Directory több-Bérlős felhőalapú alkalmazásnál](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).</span><span class="sxs-lookup"><span data-stu-id="11c2c-209">If you are having issues signing in, ensure the changes in your application are correct by reading the [Adding Sign-On to Your Web Application Using Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) topic, or downloading and inspecting the following code sample: [Multi-Tenant Cloud Application for Azure Active Directory](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).</span></span>

### <span data-ttu-id="11c2c-210"><a name="vs2010"></a>Erőforrások védelme és a Visual Studio 2008 vagy 2010 webes alkalmazások és a Windows Identity Foundation (WIF) 1.0-s a .NET 3.5</span><span class="sxs-lookup"><span data-stu-id="11c2c-210"><a name="vs2010"></a>Web applications protecting resources and created with Visual Studio 2008 or 2010 and Windows Identity Foundation (WIF) v1.0 for .NET 3.5</span></span>
<span data-ttu-id="11c2c-211">Ha egy alkalmazás WIF 1.0-s verziója található, nincs megadott mechanizmus, hogy automatikusan frissítse az alkalmazás konfigurációját, és egy új kulcsot használják.</span><span class="sxs-lookup"><span data-stu-id="11c2c-211">If you built an application on WIF v1.0, there is no provided mechanism to automatically refresh your application’s configuration to use a new key.</span></span>

* <span data-ttu-id="11c2c-212">*Legegyszerűbben* használja a FedUtil eszközt használunk erre a WIF SDK, így a legújabb metaadat-dokumentum beolvasása és a konfiguráció frissítését tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="11c2c-212">*Easiest way* Use the FedUtil tooling included in the WIF SDK, which can retrieve the latest metadata document and update your configuration.</span></span>
* <span data-ttu-id="11c2c-213">A .NET 4.5, amely tartalmazza a legújabb verziója található, a rendszer egyik névtere WIF alkalmazás frissítése.</span><span class="sxs-lookup"><span data-stu-id="11c2c-213">Update your application to .NET 4.5, which includes the newest version of WIF located in the System namespace.</span></span> <span data-ttu-id="11c2c-214">Ezután a [ellenőrzése kibocsátó neve beállításjegyzék (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) az alkalmazást az automatikus frissítések végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="11c2c-214">You can then use the [Validating Issuer Name Registry (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) to perform automatic updates of the application’s configuration.</span></span>
* <span data-ttu-id="11c2c-215">Hajtsa végre a manuális váltása szereplő utasításokat az útmutató végén.</span><span class="sxs-lookup"><span data-stu-id="11c2c-215">Perform a manual rollover as per the instructions at the end of this guidance document.</span></span>

<span data-ttu-id="11c2c-216">Útmutatás a FedUtil használatáról a konfiguráció frissítése:</span><span class="sxs-lookup"><span data-stu-id="11c2c-216">Instructions to use the FedUtil to update your configuration:</span></span>

1. <span data-ttu-id="11c2c-217">Győződjön meg arról, hogy rendelkezik-e a WIF 1.0-s verziója a fejlesztői gépen telepítve van a Visual Studio 2008 vagy 2010 SDK.</span><span class="sxs-lookup"><span data-stu-id="11c2c-217">Verify that you have the WIF v1.0 SDK installed on your development machine for Visual Studio 2008 or 2010.</span></span> <span data-ttu-id="11c2c-218">Is [töltse le innen](https://www.microsoft.com/en-us/download/details.aspx?id=4451) Ha nem telepítette még azt.</span><span class="sxs-lookup"><span data-stu-id="11c2c-218">You can [download it from here](https://www.microsoft.com/en-us/download/details.aspx?id=4451) if you have not yet installed it.</span></span>
2. <span data-ttu-id="11c2c-219">A Visual Studióban nyissa meg a megoldást, majd kattintson a jobb gombbal a megfelelő projektet, és válassza ki **összevonási metaadatok frissítése**.</span><span class="sxs-lookup"><span data-stu-id="11c2c-219">In Visual Studio, open the solution, and then right-click the applicable project and select **Update federation metadata**.</span></span> <span data-ttu-id="11c2c-220">Ha ez a beállítás nem érhető el, FedUtil és/vagy a WIF 1.0-s SDK nem lett telepítve.</span><span class="sxs-lookup"><span data-stu-id="11c2c-220">If this option is not available, FedUtil and/or the WIF v1.0 SDK has not been installed.</span></span>
3. <span data-ttu-id="11c2c-221">Válassza ki a parancssorból **frissítés** megkezdéséhez az összevonási metaadatok frissítése.</span><span class="sxs-lookup"><span data-stu-id="11c2c-221">From the prompt, select **Update** to begin updating your federation metadata.</span></span> <span data-ttu-id="11c2c-222">Ha hozzáfér a server környezetet, ahol az alkalmazás üzemel, is használhat FedUtil tartozó [automatikus metaadat-frissítés Feladatütemező](https://msdn.microsoft.com/library/ee517272.aspx).</span><span class="sxs-lookup"><span data-stu-id="11c2c-222">If you have access to the server environment where the application is hosted, you can optionally use FedUtil’s [automatic metadata update scheduler](https://msdn.microsoft.com/library/ee517272.aspx).</span></span>
4. <span data-ttu-id="11c2c-223">Kattintson a **Befejezés** a frissítési folyamat befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="11c2c-223">Click **Finish** to complete the update process.</span></span>

### <span data-ttu-id="11c2c-224"><a name="other"></a>Webalkalmazások / API-k bármely más könyvtárak segítségével, vagy manuálisan végrehajtási a támogatott protokollok erőforrások védelme</span><span class="sxs-lookup"><span data-stu-id="11c2c-224"><a name="other"></a>Web applications / APIs protecting resources using any other libraries or manually implementing any of the supported protocols</span></span>
<span data-ttu-id="11c2c-225">Ha néhány más szalagtár használata, vagy manuálisan végrehajtani a támogatott protokollok, szüksége lesz a könyvtárban vagy annak érdekében, hogy a kulcs az OpenID Connect felderítési dokumentum vagy az összevonási metaadatok lekérésére a megvalósítás áttekintése a dokumentum.</span><span class="sxs-lookup"><span data-stu-id="11c2c-225">If you are using some other library or manually implemented any of the supported protocols, you'll need to review the library or your implementation to ensure that the key is being retrieved from either the OpenID Connect discovery document or the federation metadata document.</span></span> <span data-ttu-id="11c2c-226">Egy ez kereséséhez módja az OpenID felderítési dokumentum vagy az összevonási metaadatok dokumentum kimenő még a vagy a könyvtár-kódban a kereséshez.</span><span class="sxs-lookup"><span data-stu-id="11c2c-226">One way to check for this is to do a search in your code or the library's code for any calls out to either the OpenID discovery document or the federation metadata document.</span></span>

<span data-ttu-id="11c2c-227">Ha ezek a kulcsok tárolása valahol vagy szoftveresen kötött az alkalmazásban, manuálisan beolvasása a kulcs és a frissítés, ennek megfelelően szerint hajtsa végre egy manuális váltása szereplő utasításokat az útmutató végén.</span><span class="sxs-lookup"><span data-stu-id="11c2c-227">If they key is being stored somewhere or hardcoded in your application, you can manually retrieve the key and update it accordingly by perform a manual rollover as per the instructions at the end of this guidance document.</span></span> <span data-ttu-id="11c2c-228">**Erősen ajánlott, hogy javítható-e az alkalmazás támogatja az automatikus helyettesítő** ebben a cikkben a megközelítések vázlat bármelyikét elkerülése érdekében jövőbeli megszakításait és a terhelés, hogy az Azure AD növeli az átfordulási ütemben történik, illetve hogy vészhelyzet esetén használja sávon kívüli kulcsváltás.</span><span class="sxs-lookup"><span data-stu-id="11c2c-228">**It is strongly encouraged that you enhance your application to support automatic rollover** using any of the approaches outline in this article to avoid future disruptions and overhead if Azure AD increases it's rollover cadence or has an emergency out-of-band rollover.</span></span>

## <a name="how-to-test-your-application-to-determine-if-it-will-be-affected"></a><span data-ttu-id="11c2c-229">Állapítsa meg, amennyiben ez érinti az alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="11c2c-229">How to test your application to determine if it will be affected</span></span>
<span data-ttu-id="11c2c-230">Ellenőrizheti, hogy az alkalmazás támogatja az automatikus kulcsváltást letöltése a parancsfájlok és utasításait követve [a GitHub-tárházban.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)</span><span class="sxs-lookup"><span data-stu-id="11c2c-230">You can validate whether your application supports automatic key rollover by downloading the scripts and following the instructions in [this GitHub repository.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)</span></span>

## <a name="how-to-perform-a-manual-rollover-if-you-application-does-not-support-automatic-rollover"></a><span data-ttu-id="11c2c-231">Hogyan hajthat végre egy manuális váltása, ha alkalmazás nem támogatja az automatikus váltása</span><span class="sxs-lookup"><span data-stu-id="11c2c-231">How to perform a manual rollover if you application does not support automatic rollover</span></span>
<span data-ttu-id="11c2c-232">Ha az alkalmazás **nem** támogatja az automatikus helyettesítő, szüksége lesz egy folyamat, amely rendszeresen figyeli az Azure AD által aláíró kulcsok, és ennek megfelelően hajt végre egy manuális váltása létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="11c2c-232">If your application does **not** support automatic rollover, you will need to establish a process that periodically monitors Azure AD's signing keys and performs a manual rollover accordingly.</span></span> <span data-ttu-id="11c2c-233">[A GitHub-tárházban](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) parancsfájlok és ehhez útmutatást tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="11c2c-233">[This GitHub repository](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) contains scripts and instructions on how to do this.</span></span>

