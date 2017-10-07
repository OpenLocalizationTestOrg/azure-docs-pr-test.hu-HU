---
title: "-Microsoft fenyegetések modellezése eszköz - kezelés Azure aaaSession |} Microsoft Docs"
description: "a fenyegetések modellezése eszköz hello felfedett fenyegetések megoldást"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 915ffae3f775ca6902fcfb93e7e1952ce85612f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="security-frame-session-management--articles"></a><span data-ttu-id="8e656-103">Biztonsági keret: Munkamenet-kezelés |} Cikkek</span><span class="sxs-lookup"><span data-stu-id="8e656-103">Security Frame: Session Management | Articles</span></span> 
| <span data-ttu-id="8e656-104">A termék vagy szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="8e656-104">Product/Service</span></span> | <span data-ttu-id="8e656-105">Cikk</span><span class="sxs-lookup"><span data-stu-id="8e656-105">Article</span></span> |
| --------------- | ------- |
| <span data-ttu-id="8e656-106">**Azure AD**</span><span class="sxs-lookup"><span data-stu-id="8e656-106">**Azure AD**</span></span>    | <ul><li>[<span data-ttu-id="8e656-107">Alkalmazzon megfelelő kijelentkezési ADAL módszerek használatát, ha az Azure AD segítségével</span><span class="sxs-lookup"><span data-stu-id="8e656-107">Implement proper logout using ADAL methods when using Azure AD</span></span>](#logout-adal)</li></ul> |
| <span data-ttu-id="8e656-108">IoT-eszközök</span><span class="sxs-lookup"><span data-stu-id="8e656-108">IoT Device</span></span> | <ul><li>[<span data-ttu-id="8e656-109">Generált SaS-tokenje véges élettartamai használata</span><span class="sxs-lookup"><span data-stu-id="8e656-109">Use finite lifetimes for generated SaS tokens</span></span>](#finite-tokens)</li></ul> |
| <span data-ttu-id="8e656-110">**Az Azure Document DB rendszerbe**</span><span class="sxs-lookup"><span data-stu-id="8e656-110">**Azure Document DB**</span></span> | <ul><li>[<span data-ttu-id="8e656-111">A létrehozott erőforrás-jogkivonatokat minimális jogkivonat élettartamát használata</span><span class="sxs-lookup"><span data-stu-id="8e656-111">Use minimum token lifetimes for generated Resource tokens</span></span>](#resource-tokens)</li></ul> |
| <span data-ttu-id="8e656-112">**ADFS**</span><span class="sxs-lookup"><span data-stu-id="8e656-112">**ADFS**</span></span> | <ul><li>[<span data-ttu-id="8e656-113">Alkalmazzon megfelelő kijelentkezési WsFederation módszerek használatát, ha az AD FS használatával</span><span class="sxs-lookup"><span data-stu-id="8e656-113">Implement proper logout using WsFederation methods when using ADFS</span></span>](#wsfederation-logout)</li></ul> |
| <span data-ttu-id="8e656-114">**Identity Serverben**</span><span class="sxs-lookup"><span data-stu-id="8e656-114">**Identity Server**</span></span> | <ul><li>[<span data-ttu-id="8e656-115">Alkalmazzon megfelelő kijelentkezési Identitáskiszolgálók használatakor</span><span class="sxs-lookup"><span data-stu-id="8e656-115">Implement proper logout when using Identity Server</span></span>](#proper-logout)</li></ul> |
| <span data-ttu-id="8e656-116">**Webalkalmazás**</span><span class="sxs-lookup"><span data-stu-id="8e656-116">**Web Application**</span></span> | <ul><li>[<span data-ttu-id="8e656-117">HTTPS-KAPCSOLATON keresztül elérhető alkalmazások biztonságos cookie-kat kell használni.</span><span class="sxs-lookup"><span data-stu-id="8e656-117">Applications available over HTTPS must use secure cookies</span></span>](#https-secure-cookies)</li><li>[<span data-ttu-id="8e656-118">Az összes HTTP-alapú alkalmazások csak olyan cookie-k meghatározása http kell megadni</span><span class="sxs-lookup"><span data-stu-id="8e656-118">All http based application should specify http only for cookie definition</span></span>](#cookie-definition)</li><li>[<span data-ttu-id="8e656-119">ASP.NET-weblapok többhelyes kérelem hamisítására (CSRF) támadások elleni</span><span class="sxs-lookup"><span data-stu-id="8e656-119">Mitigate against Cross-Site Request Forgery (CSRF) attacks on ASP.NET web pages</span></span>](#csrf-asp)</li><li>[<span data-ttu-id="8e656-120">Állítsa be a munkamenet inaktivitás élettartama</span><span class="sxs-lookup"><span data-stu-id="8e656-120">Set up session for inactivity lifetime</span></span>](#inactivity-lifetime)</li><li>[<span data-ttu-id="8e656-121">Alkalmazzon megfelelő kijelentkezési hello alkalmazásból</span><span class="sxs-lookup"><span data-stu-id="8e656-121">Implement proper logout from hello application</span></span>](#proper-app-logout)</li></ul> |
| <span data-ttu-id="8e656-122">**Webes API**</span><span class="sxs-lookup"><span data-stu-id="8e656-122">**Web API**</span></span> | <ul><li>[<span data-ttu-id="8e656-123">ASP.NET webes API-k többhelyes kérelem hamisítására (CSRF) támadások elleni</span><span class="sxs-lookup"><span data-stu-id="8e656-123">Mitigate against Cross-Site Request Forgery (CSRF) attacks on ASP.NET Web APIs</span></span>](#csrf-api)</li></ul> |

## <span data-ttu-id="8e656-124"><a id="logout-adal"></a>Alkalmazzon megfelelő kijelentkezési ADAL módszerek használatát, ha az Azure AD segítségével</span><span class="sxs-lookup"><span data-stu-id="8e656-124"><a id="logout-adal"></a>Implement proper logout using ADAL methods when using Azure AD</span></span>

| <span data-ttu-id="8e656-125">Cím</span><span class="sxs-lookup"><span data-stu-id="8e656-125">Title</span></span>                   | <span data-ttu-id="8e656-126">Részletek</span><span class="sxs-lookup"><span data-stu-id="8e656-126">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="8e656-127">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="8e656-127">**Component**</span></span>               | <span data-ttu-id="8e656-128">Azure AD</span><span class="sxs-lookup"><span data-stu-id="8e656-128">Azure AD</span></span> | 
| <span data-ttu-id="8e656-129">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="8e656-129">**SDL Phase**</span></span>               | <span data-ttu-id="8e656-130">Felépítés</span><span class="sxs-lookup"><span data-stu-id="8e656-130">Build</span></span> |  
| <span data-ttu-id="8e656-131">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="8e656-131">**Applicable Technologies**</span></span> | <span data-ttu-id="8e656-132">Általános</span><span class="sxs-lookup"><span data-stu-id="8e656-132">Generic</span></span> |
| <span data-ttu-id="8e656-133">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="8e656-133">**Attributes**</span></span>              | <span data-ttu-id="8e656-134">N/A</span><span class="sxs-lookup"><span data-stu-id="8e656-134">N/A</span></span>  |
| <span data-ttu-id="8e656-135">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="8e656-135">**References**</span></span>              | <span data-ttu-id="8e656-136">N/A</span><span class="sxs-lookup"><span data-stu-id="8e656-136">N/A</span></span>  |
| <span data-ttu-id="8e656-137">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="8e656-137">**Steps**</span></span> | <span data-ttu-id="8e656-138">Ha hello alkalmazás az Azure AD által kiállított jogkivonat támaszkodik, meg kell hívnia az hello kijelentkezési eseménykezelő</span><span class="sxs-lookup"><span data-stu-id="8e656-138">If hello application relies on access token issued by Azure AD, hello logout event handler should call</span></span> |

### <a name="example"></a><span data-ttu-id="8e656-139">Példa</span><span class="sxs-lookup"><span data-stu-id="8e656-139">Example</span></span>
```C#
HttpContext.GetOwinContext().Authentication.SignOut(OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType)
```

### <a name="example"></a><span data-ttu-id="8e656-140">Példa</span><span class="sxs-lookup"><span data-stu-id="8e656-140">Example</span></span>
<span data-ttu-id="8e656-141">Akkor kell felhasználói munkamenet is megsemmisítése Session.Abandon() metódus meghívásával.</span><span class="sxs-lookup"><span data-stu-id="8e656-141">It should also destroy user's session by calling Session.Abandon() method.</span></span> <span data-ttu-id="8e656-142">A következő metódus jeleníti meg a felhasználó kijelentkezik biztonságos végrehajtásának:</span><span class="sxs-lookup"><span data-stu-id="8e656-142">Following method shows secure implementation of user logout:</span></span>
```C#
    [HttpPost]
        [ValidateAntiForgeryToken]
        public void LogOff()
        {
            string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
            AuthenticationContext authContext = new AuthenticationContext(Authority + TenantId, new NaiveSessionCache(userObjectID));
            authContext.TokenCache.Clear();
            Session.Clear();
            Session.Abandon();
            Response.SetCookie(new HttpCookie("ASP.NET_SessionId", string.Empty));
            HttpContext.GetOwinContext().Authentication.SignOut(
                OpenIdConnectAuthenticationDefaults.AuthenticationType,
                CookieAuthenticationDefaults.AuthenticationType);
        } 
```

## <span data-ttu-id="8e656-143"><a id="finite-tokens"></a>Generált SaS-tokenje véges élettartamai használata</span><span class="sxs-lookup"><span data-stu-id="8e656-143"><a id="finite-tokens"></a>Use finite lifetimes for generated SaS tokens</span></span>

| <span data-ttu-id="8e656-144">Cím</span><span class="sxs-lookup"><span data-stu-id="8e656-144">Title</span></span>                   | <span data-ttu-id="8e656-145">Részletek</span><span class="sxs-lookup"><span data-stu-id="8e656-145">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="8e656-146">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="8e656-146">**Component**</span></span>               | <span data-ttu-id="8e656-147">IoT-eszközök</span><span class="sxs-lookup"><span data-stu-id="8e656-147">IoT Device</span></span> | 
| <span data-ttu-id="8e656-148">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="8e656-148">**SDL Phase**</span></span>               | <span data-ttu-id="8e656-149">Felépítés</span><span class="sxs-lookup"><span data-stu-id="8e656-149">Build</span></span> |  
| <span data-ttu-id="8e656-150">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="8e656-150">**Applicable Technologies**</span></span> | <span data-ttu-id="8e656-151">Általános</span><span class="sxs-lookup"><span data-stu-id="8e656-151">Generic</span></span> |
| <span data-ttu-id="8e656-152">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="8e656-152">**Attributes**</span></span>              | <span data-ttu-id="8e656-153">N/A</span><span class="sxs-lookup"><span data-stu-id="8e656-153">N/A</span></span>  |
| <span data-ttu-id="8e656-154">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="8e656-154">**References**</span></span>              | <span data-ttu-id="8e656-155">N/A</span><span class="sxs-lookup"><span data-stu-id="8e656-155">N/A</span></span>  |
| <span data-ttu-id="8e656-156">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="8e656-156">**Steps**</span></span> | <span data-ttu-id="8e656-157">Az IoT-központ tooAzure generált SaS-tokenje kell véges lejárati idővel rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="8e656-157">SaS tokens generated for authenticating tooAzure IoT Hub should have a finite expiry period.</span></span> <span data-ttu-id="8e656-158">Tartsa hello SaS tokent élettartama tooa minimális toolimit hello időn visszajátszani abban az esetben hello jogkivonatok kerülnek veszélybe.</span><span class="sxs-lookup"><span data-stu-id="8e656-158">Keep hello SaS token lifetimes tooa minimum toolimit hello amount of time they can be replayed in case hello tokens are compromised.</span></span>|

## <span data-ttu-id="8e656-159"><a id="resource-tokens"></a>A létrehozott erőforrás-jogkivonatokat minimális jogkivonat élettartamát használata</span><span class="sxs-lookup"><span data-stu-id="8e656-159"><a id="resource-tokens"></a>Use minimum token lifetimes for generated Resource tokens</span></span>

| <span data-ttu-id="8e656-160">Cím</span><span class="sxs-lookup"><span data-stu-id="8e656-160">Title</span></span>                   | <span data-ttu-id="8e656-161">Részletek</span><span class="sxs-lookup"><span data-stu-id="8e656-161">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="8e656-162">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="8e656-162">**Component**</span></span>               | <span data-ttu-id="8e656-163">Az Azure Document DB rendszerbe</span><span class="sxs-lookup"><span data-stu-id="8e656-163">Azure Document DB</span></span> | 
| <span data-ttu-id="8e656-164">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="8e656-164">**SDL Phase**</span></span>               | <span data-ttu-id="8e656-165">Felépítés</span><span class="sxs-lookup"><span data-stu-id="8e656-165">Build</span></span> |  
| <span data-ttu-id="8e656-166">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="8e656-166">**Applicable Technologies**</span></span> | <span data-ttu-id="8e656-167">Általános</span><span class="sxs-lookup"><span data-stu-id="8e656-167">Generic</span></span> |
| <span data-ttu-id="8e656-168">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="8e656-168">**Attributes**</span></span>              | <span data-ttu-id="8e656-169">N/A</span><span class="sxs-lookup"><span data-stu-id="8e656-169">N/A</span></span>  |
| <span data-ttu-id="8e656-170">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="8e656-170">**References**</span></span>              | <span data-ttu-id="8e656-171">N/A</span><span class="sxs-lookup"><span data-stu-id="8e656-171">N/A</span></span>  |
| <span data-ttu-id="8e656-172">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="8e656-172">**Steps**</span></span> | <span data-ttu-id="8e656-173">Csökkentse a hello timespan erőforrás token tooa minimális érték szükséges.</span><span class="sxs-lookup"><span data-stu-id="8e656-173">Reduce hello timespan of resource token tooa minimum value required.</span></span> <span data-ttu-id="8e656-174">Erőforrás alapértelmezett 1 órás érvényes timespan lehet.</span><span class="sxs-lookup"><span data-stu-id="8e656-174">Resource tokens have a default valid timespan of 1 hour.</span></span>|

## <span data-ttu-id="8e656-175"><a id="wsfederation-logout"></a>Alkalmazzon megfelelő kijelentkezési WsFederation módszerek használatát, ha az AD FS használatával</span><span class="sxs-lookup"><span data-stu-id="8e656-175"><a id="wsfederation-logout"></a>Implement proper logout using WsFederation methods when using ADFS</span></span>

| <span data-ttu-id="8e656-176">Cím</span><span class="sxs-lookup"><span data-stu-id="8e656-176">Title</span></span>                   | <span data-ttu-id="8e656-177">Részletek</span><span class="sxs-lookup"><span data-stu-id="8e656-177">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="8e656-178">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="8e656-178">**Component**</span></span>               | <span data-ttu-id="8e656-179">ADFS</span><span class="sxs-lookup"><span data-stu-id="8e656-179">ADFS</span></span> | 
| <span data-ttu-id="8e656-180">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="8e656-180">**SDL Phase**</span></span>               | <span data-ttu-id="8e656-181">Felépítés</span><span class="sxs-lookup"><span data-stu-id="8e656-181">Build</span></span> |  
| <span data-ttu-id="8e656-182">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="8e656-182">**Applicable Technologies**</span></span> | <span data-ttu-id="8e656-183">Általános</span><span class="sxs-lookup"><span data-stu-id="8e656-183">Generic</span></span> |
| <span data-ttu-id="8e656-184">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="8e656-184">**Attributes**</span></span>              | <span data-ttu-id="8e656-185">N/A</span><span class="sxs-lookup"><span data-stu-id="8e656-185">N/A</span></span>  |
| <span data-ttu-id="8e656-186">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="8e656-186">**References**</span></span>              | <span data-ttu-id="8e656-187">N/A</span><span class="sxs-lookup"><span data-stu-id="8e656-187">N/A</span></span>  |
| <span data-ttu-id="8e656-188">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="8e656-188">**Steps**</span></span> | <span data-ttu-id="8e656-189">Hello alkalmazás STS-jogkivonatot AD FS által kibocsátott támaszkodik, hello kijelentkezési eseménykezelő WSFederationAuthenticationModule.FederatedSignOut() metódus toolog hello felhasználói ki kell hívjuk.</span><span class="sxs-lookup"><span data-stu-id="8e656-189">If hello application relies on STS token issued by ADFS, hello logout event handler should call WSFederationAuthenticationModule.FederatedSignOut() method toolog out hello user.</span></span> <span data-ttu-id="8e656-190">Is hello aktuális munkamenet meg kell semmisíteni, és hello munkamenet token legyen alaphelyzetbe állítása és hatálytalanítja.</span><span class="sxs-lookup"><span data-stu-id="8e656-190">Also hello current session should be destroyed, and hello session token value should be reset and nullified.</span></span>|

### <a name="example"></a><span data-ttu-id="8e656-191">Példa</span><span class="sxs-lookup"><span data-stu-id="8e656-191">Example</span></span>
```C#
        [HttpPost, ValidateAntiForgeryToken]
        [Authorization]
        public ActionResult SignOut(string redirectUrl)
        {
            if (!this.User.Identity.IsAuthenticated)
            {
                return this.View("LogOff", null);
            }

            // Removes hello user profile.
            this.Session.Clear();
            this.Session.Abandon();
            HttpContext.Current.Response.Cookies.Add(new System.Web.HttpCookie("ASP.NET_SessionId", string.Empty)
                {
                    Expires = DateTime.Now.AddDays(-1D),
                    Secure = true,
                    HttpOnly = true
                });

            // Signs out at hello specified security token service (STS) by using hello WS-Federation protocol.
            Uri signOutUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Issuer);
            Uri replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm);
            if (!string.IsNullOrEmpty(redirectUrl))
            {
                replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm + redirectUrl);
            }
           //     Signs out of hello current session and raises hello appropriate events.
            var authModule = FederatedAuthentication.WSFederationAuthenticationModule;
            authModule.SignOut(false);
        //     Signs out at hello specified security token service (STS) by using hello WS-Federation
        //     protocol.            
            WSFederationAuthenticationModule.FederatedSignOut(signOutUrl, replyUrl);
            return new RedirectResult(redirectUrl);
        }
```

## <span data-ttu-id="8e656-192"><a id="proper-logout"></a>Alkalmazzon megfelelő kijelentkezési Identitáskiszolgálók használatakor</span><span class="sxs-lookup"><span data-stu-id="8e656-192"><a id="proper-logout"></a>Implement proper logout when using Identity Server</span></span>

| <span data-ttu-id="8e656-193">Cím</span><span class="sxs-lookup"><span data-stu-id="8e656-193">Title</span></span>                   | <span data-ttu-id="8e656-194">Részletek</span><span class="sxs-lookup"><span data-stu-id="8e656-194">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="8e656-195">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="8e656-195">**Component**</span></span>               | <span data-ttu-id="8e656-196">Identity Serverben</span><span class="sxs-lookup"><span data-stu-id="8e656-196">Identity Server</span></span> | 
| <span data-ttu-id="8e656-197">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="8e656-197">**SDL Phase**</span></span>               | <span data-ttu-id="8e656-198">Felépítés</span><span class="sxs-lookup"><span data-stu-id="8e656-198">Build</span></span> |  
| <span data-ttu-id="8e656-199">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="8e656-199">**Applicable Technologies**</span></span> | <span data-ttu-id="8e656-200">Általános</span><span class="sxs-lookup"><span data-stu-id="8e656-200">Generic</span></span> |
| <span data-ttu-id="8e656-201">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="8e656-201">**Attributes**</span></span>              | <span data-ttu-id="8e656-202">N/A</span><span class="sxs-lookup"><span data-stu-id="8e656-202">N/A</span></span>  |
| <span data-ttu-id="8e656-203">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="8e656-203">**References**</span></span>              | [<span data-ttu-id="8e656-204">IdentityServer3 összevont kijelentkezés</span><span class="sxs-lookup"><span data-stu-id="8e656-204">IdentityServer3-Federated sign out</span></span>](https://identityserver.github.io/Documentation/docsv2/advanced/federated-signout.html) |
| <span data-ttu-id="8e656-205">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="8e656-205">**Steps**</span></span> | <span data-ttu-id="8e656-206">IdentityServer hello képességét toofederate a külső Identitásszolgáltatók támogatja.</span><span class="sxs-lookup"><span data-stu-id="8e656-206">IdentityServer supports hello ability toofederate with external identity providers.</span></span> <span data-ttu-id="8e656-207">Amikor egy felhasználó kijelentkezik a felsőbb rétegbeli identitásszolgáltató, attól függően, hello protokoll által használt, előfordulhat, lehetséges tooreceive értesítést amikor hello felhasználó kijelentkezik. Lehetővé teszi az ügyfelek így is bejelentkezhetnek hello felhasználó IdentityServer toonotify. Hello dokumentációjában hello references szakaszában a hello megvalósítás részletei.</span><span class="sxs-lookup"><span data-stu-id="8e656-207">When a user signs out of an upstream identity provider, depending upon hello protocol used, it might be possible tooreceive a notification when hello user signs out. It allows IdentityServer toonotify its clients so they can also sign hello user out. Check hello documentation in hello references section for hello implementation details.</span></span>|

## <span data-ttu-id="8e656-208"><a id="https-secure-cookies"></a>HTTPS-KAPCSOLATON keresztül elérhető alkalmazások biztonságos cookie-kat kell használni.</span><span class="sxs-lookup"><span data-stu-id="8e656-208"><a id="https-secure-cookies"></a>Applications available over HTTPS must use secure cookies</span></span>

| <span data-ttu-id="8e656-209">Cím</span><span class="sxs-lookup"><span data-stu-id="8e656-209">Title</span></span>                   | <span data-ttu-id="8e656-210">Részletek</span><span class="sxs-lookup"><span data-stu-id="8e656-210">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="8e656-211">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="8e656-211">**Component**</span></span>               | <span data-ttu-id="8e656-212">Web Application</span><span class="sxs-lookup"><span data-stu-id="8e656-212">Web Application</span></span> | 
| <span data-ttu-id="8e656-213">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="8e656-213">**SDL Phase**</span></span>               | <span data-ttu-id="8e656-214">Felépítés</span><span class="sxs-lookup"><span data-stu-id="8e656-214">Build</span></span> |  
| <span data-ttu-id="8e656-215">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="8e656-215">**Applicable Technologies**</span></span> | <span data-ttu-id="8e656-216">Általános</span><span class="sxs-lookup"><span data-stu-id="8e656-216">Generic</span></span> |
| <span data-ttu-id="8e656-217">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="8e656-217">**Attributes**</span></span>              | <span data-ttu-id="8e656-218">EnvironmentType - a helyi üzemeltetésű</span><span class="sxs-lookup"><span data-stu-id="8e656-218">EnvironmentType - OnPrem</span></span> |
| <span data-ttu-id="8e656-219">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="8e656-219">**References**</span></span>              | <span data-ttu-id="8e656-220">[Elem (ASP.NET beállítási séma) httpCookies](http://msdn.microsoft.com/library/ms228262(v=vs.100).aspx), [HttpCookie.Secure tulajdonság](http://msdn.microsoft.com/library/system.web.httpcookie.secure.aspx)</span><span class="sxs-lookup"><span data-stu-id="8e656-220">[httpCookies Element (ASP.NET Settings Schema)](http://msdn.microsoft.com/library/ms228262(v=vs.100).aspx), [HttpCookie.Secure Property](http://msdn.microsoft.com/library/system.web.httpcookie.secure.aspx)</span></span> |
| <span data-ttu-id="8e656-221">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="8e656-221">**Steps**</span></span> | <span data-ttu-id="8e656-222">A cookie-k általában csak elérhető toohello tartományt, amelynek hatóköre volt.</span><span class="sxs-lookup"><span data-stu-id="8e656-222">Cookies are normally only accessible toohello domain for which they were scoped.</span></span> <span data-ttu-id="8e656-223">Sajnos "tartományi" hello definíciója nem tartalmazza a hello protokoll úgy, hogy a cookie-k, amelyek létrejönnek a HTTPS-KAPCSOLATON keresztül elérhető HTTP Protokollon keresztül.</span><span class="sxs-lookup"><span data-stu-id="8e656-223">Unfortunately, hello definition of "domain" does not include hello protocol so cookies that are created over HTTPS are accessible over HTTP.</span></span> <span data-ttu-id="8e656-224">hello "biztonságos" attribútum azt jelöli, toohello böngésző cookie-k hello csak elérhetővé kell tenni HTTPS-KAPCSOLATON keresztül.</span><span class="sxs-lookup"><span data-stu-id="8e656-224">hello "secure" attribute indicates toohello browser that hello cookie should only be made available over HTTPS.</span></span> <span data-ttu-id="8e656-225">Győződjön meg arról, hogy az összes cookie-kat, állítsa be HTTPS-KAPCSOLATON keresztül hello **biztonságos** attribútum.</span><span class="sxs-lookup"><span data-stu-id="8e656-225">Ensure that all cookies set over HTTPS use hello **secure** attribute.</span></span> <span data-ttu-id="8e656-226">hello követelmény hello requireSSL attribútum tootrue beállításával kényszerítheti hello web.config fájlban.</span><span class="sxs-lookup"><span data-stu-id="8e656-226">hello requirement can be enforced in hello web.config file by setting hello requireSSL attribute tootrue.</span></span> <span data-ttu-id="8e656-227">Hello megközelítés előnyben részesített, mert azt kényszeríti ki a hello **biztonságos** jelenlegi és jövőbeli cookie-k nélkül hello kell toomake attribútum kód módosításokat.</span><span class="sxs-lookup"><span data-stu-id="8e656-227">It is hello preferred approach because it will enforce hello **secure** attribute for all current and future cookies without hello need toomake any additional code changes.</span></span>|

### <a name="example"></a><span data-ttu-id="8e656-228">Példa</span><span class="sxs-lookup"><span data-stu-id="8e656-228">Example</span></span>
```C#
<configuration>
  <system.web>
    <httpCookies requireSSL="true"/>
  </system.web>
</configuration>
```
<span data-ttu-id="8e656-229">hello beállítás akkor is, ha HTTP használt tooaccess hello alkalmazás lép életbe.</span><span class="sxs-lookup"><span data-stu-id="8e656-229">hello setting is enforced even if HTTP is used tooaccess hello application.</span></span> <span data-ttu-id="8e656-230">Ha a HTTP protokoll tooaccess hello alkalmazás, hello hello alkalmazás mivel hello cookie-k hello biztonságos attribútum és hello böngészővel nem küld őket beállítás oldaltörések biztonsági toohello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8e656-230">If HTTP is used tooaccess hello application, hello setting breaks hello application because hello cookies are set with hello secure attribute and hello browser will not send them back toohello application.</span></span>

| <span data-ttu-id="8e656-231">Cím</span><span class="sxs-lookup"><span data-stu-id="8e656-231">Title</span></span>                   | <span data-ttu-id="8e656-232">Részletek</span><span class="sxs-lookup"><span data-stu-id="8e656-232">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="8e656-233">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="8e656-233">**Component**</span></span>               | <span data-ttu-id="8e656-234">Web Application</span><span class="sxs-lookup"><span data-stu-id="8e656-234">Web Application</span></span> | 
| <span data-ttu-id="8e656-235">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="8e656-235">**SDL Phase**</span></span>               | <span data-ttu-id="8e656-236">Felépítés</span><span class="sxs-lookup"><span data-stu-id="8e656-236">Build</span></span> |  
| <span data-ttu-id="8e656-237">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="8e656-237">**Applicable Technologies**</span></span> | <span data-ttu-id="8e656-238">Web Forms keretrendszerre, MVC5</span><span class="sxs-lookup"><span data-stu-id="8e656-238">Web Forms, MVC5</span></span> |
| <span data-ttu-id="8e656-239">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="8e656-239">**Attributes**</span></span>              | <span data-ttu-id="8e656-240">EnvironmentType - a helyi üzemeltetésű</span><span class="sxs-lookup"><span data-stu-id="8e656-240">EnvironmentType - OnPrem</span></span> |
| <span data-ttu-id="8e656-241">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="8e656-241">**References**</span></span>              | <span data-ttu-id="8e656-242">N/A</span><span class="sxs-lookup"><span data-stu-id="8e656-242">N/A</span></span>  |
| <span data-ttu-id="8e656-243">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="8e656-243">**Steps**</span></span> | <span data-ttu-id="8e656-244">Amikor hello webalkalmazás hello függő fél részére, és hello IdP ADFS-kiszolgáló, hello FedAuth jogkivonat biztonságos attribútum konfigurálható requireSSL tooTrue beállítása a `system.identityModel.services` szakasz a Web.config fájl:</span><span class="sxs-lookup"><span data-stu-id="8e656-244">When hello web application is hello Relying Party, and hello IdP is ADFS server, hello FedAuth token's secure attribute can be configured by setting requireSSL tooTrue in `system.identityModel.services` section of web.config:</span></span>|

### <a name="example"></a><span data-ttu-id="8e656-245">Példa</span><span class="sxs-lookup"><span data-stu-id="8e656-245">Example</span></span>
```C#
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:20:0" />
    ....  
    </federationConfiguration>
  </system.identityModel.services>
```

## <span data-ttu-id="8e656-246"><a id="cookie-definition"></a>Az összes HTTP-alapú alkalmazások csak olyan cookie-k meghatározása http kell megadni</span><span class="sxs-lookup"><span data-stu-id="8e656-246"><a id="cookie-definition"></a>All http based application should specify http only for cookie definition</span></span>

| <span data-ttu-id="8e656-247">Cím</span><span class="sxs-lookup"><span data-stu-id="8e656-247">Title</span></span>                   | <span data-ttu-id="8e656-248">Részletek</span><span class="sxs-lookup"><span data-stu-id="8e656-248">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="8e656-249">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="8e656-249">**Component**</span></span>               | <span data-ttu-id="8e656-250">Web Application</span><span class="sxs-lookup"><span data-stu-id="8e656-250">Web Application</span></span> | 
| <span data-ttu-id="8e656-251">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="8e656-251">**SDL Phase**</span></span>               | <span data-ttu-id="8e656-252">Felépítés</span><span class="sxs-lookup"><span data-stu-id="8e656-252">Build</span></span> |  
| <span data-ttu-id="8e656-253">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="8e656-253">**Applicable Technologies**</span></span> | <span data-ttu-id="8e656-254">Általános</span><span class="sxs-lookup"><span data-stu-id="8e656-254">Generic</span></span> |
| <span data-ttu-id="8e656-255">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="8e656-255">**Attributes**</span></span>              | <span data-ttu-id="8e656-256">N/A</span><span class="sxs-lookup"><span data-stu-id="8e656-256">N/A</span></span>  |
| <span data-ttu-id="8e656-257">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="8e656-257">**References**</span></span>              | [<span data-ttu-id="8e656-258">Biztonságos Cookie-attribútum</span><span class="sxs-lookup"><span data-stu-id="8e656-258">Secure Cookie Attribute</span></span>](https://en.wikipedia.org/wiki/HTTP_cookie#Secure_cookie) |
| <span data-ttu-id="8e656-259">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="8e656-259">**Steps**</span></span> | <span data-ttu-id="8e656-260">toomitigate hello információfelfedés kockázatának csökkentéséhez a többhelyes scripting (lehetővé) támadás rendelkező, új attribútum - httpOnly - bevezetett toocookies volt, és az összes ismertebb böngésző támogatja.</span><span class="sxs-lookup"><span data-stu-id="8e656-260">toomitigate hello risk of information disclosure with a cross-site scripting (XSS) attack, a new attribute - httpOnly - was introduced toocookies and is supported by all major browsers.</span></span> <span data-ttu-id="8e656-261">hello attribútum Megadja, hogy a cookie-k nem érhető el a parancsfájlon keresztül.</span><span class="sxs-lookup"><span data-stu-id="8e656-261">hello attribute specifies that a cookie is not accessible through script.</span></span> <span data-ttu-id="8e656-262">HttpOnly cookie-k használatával egy webalkalmazás hello lehetőségét, hogy az hello cookie-ban tárolt bizalmas információ keresztül parancsfájl ellopják és tooan támadó webhely küldött csökkenti.</span><span class="sxs-lookup"><span data-stu-id="8e656-262">By using HttpOnly cookies, a web application reduces hello possibility that sensitive information contained in hello cookie can be stolen via script and sent tooan attacker's website.</span></span> |

### <a name="example"></a><span data-ttu-id="8e656-263">Példa</span><span class="sxs-lookup"><span data-stu-id="8e656-263">Example</span></span>
<span data-ttu-id="8e656-264">Cookie-k használó összes HTTP-alapú alkalmazások kell HttpOnly hello cookie-definíció adható meg, alkalmazásával segítse a web.config konfigurációs követően:</span><span class="sxs-lookup"><span data-stu-id="8e656-264">All HTTP-based applications that use cookies should specify HttpOnly in hello cookie definition, by implementing following configuration in web.config:</span></span>
```XML
<system.web>
.
.
   <httpCookies requireSSL="false" httpOnlyCookies="true"/>
.
.
</system.web>
```

| <span data-ttu-id="8e656-265">Cím</span><span class="sxs-lookup"><span data-stu-id="8e656-265">Title</span></span>                   | <span data-ttu-id="8e656-266">Részletek</span><span class="sxs-lookup"><span data-stu-id="8e656-266">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="8e656-267">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="8e656-267">**Component**</span></span>               | <span data-ttu-id="8e656-268">Web Application</span><span class="sxs-lookup"><span data-stu-id="8e656-268">Web Application</span></span> | 
| <span data-ttu-id="8e656-269">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="8e656-269">**SDL Phase**</span></span>               | <span data-ttu-id="8e656-270">Felépítés</span><span class="sxs-lookup"><span data-stu-id="8e656-270">Build</span></span> |  
| <span data-ttu-id="8e656-271">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="8e656-271">**Applicable Technologies**</span></span> | <span data-ttu-id="8e656-272">Web Forms keretrendszerre</span><span class="sxs-lookup"><span data-stu-id="8e656-272">Web Forms</span></span> |
| <span data-ttu-id="8e656-273">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="8e656-273">**Attributes**</span></span>              | <span data-ttu-id="8e656-274">N/A</span><span class="sxs-lookup"><span data-stu-id="8e656-274">N/A</span></span>  |
| <span data-ttu-id="8e656-275">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="8e656-275">**References**</span></span>              | [<span data-ttu-id="8e656-276">FormsAuthentication.RequireSSL tulajdonság</span><span class="sxs-lookup"><span data-stu-id="8e656-276">FormsAuthentication.RequireSSL Property</span></span>](https://msdn.microsoft.com/library/system.web.security.formsauthentication.requiressl.aspx) |
| <span data-ttu-id="8e656-277">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="8e656-277">**Steps**</span></span> | <span data-ttu-id="8e656-278">hello RequireSSL tulajdonság értéke attribútumával hello requireSSL hello konfigurációs elem értéke egy ASP.NET-alkalmazás hello konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="8e656-278">hello RequireSSL property value is set in hello configuration file for an ASP.NET application by using hello requireSSL attribute of hello configuration element.</span></span> <span data-ttu-id="8e656-279">Megadhat hello Web.config fájl az ASP.NET alkalmazás hogy SSL (Secure Sockets Layer)-e a szükséges tooreturn hello űrlap-hitelesítési cookie-toohello kiszolgáló hello requireSSL attribútumának beállításakor.</span><span class="sxs-lookup"><span data-stu-id="8e656-279">You can specify in hello Web.config file for your ASP.NET application whether SSL (Secure Sockets Layer) is required tooreturn hello forms-authentication cookie toohello server by setting hello requireSSL attribute.</span></span>|

### <a name="example"></a><span data-ttu-id="8e656-280">Példa</span><span class="sxs-lookup"><span data-stu-id="8e656-280">Example</span></span> 
<span data-ttu-id="8e656-281">hello alábbi példakód beállítja hello requireSSL attribútum hello Web.config fájlban.</span><span class="sxs-lookup"><span data-stu-id="8e656-281">hello following code example sets hello requireSSL attribute in hello Web.config file.</span></span>
```XML
<authentication mode="Forms">
  <forms loginUrl="member_login.aspx" cookieless="UseCookies" requireSSL="true"/>
</authentication>
```

| <span data-ttu-id="8e656-282">Cím</span><span class="sxs-lookup"><span data-stu-id="8e656-282">Title</span></span>                   | <span data-ttu-id="8e656-283">Részletek</span><span class="sxs-lookup"><span data-stu-id="8e656-283">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="8e656-284">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="8e656-284">**Component**</span></span>               | <span data-ttu-id="8e656-285">Web Application</span><span class="sxs-lookup"><span data-stu-id="8e656-285">Web Application</span></span> | 
| <span data-ttu-id="8e656-286">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="8e656-286">**SDL Phase**</span></span>               | <span data-ttu-id="8e656-287">Felépítés</span><span class="sxs-lookup"><span data-stu-id="8e656-287">Build</span></span> |  
| <span data-ttu-id="8e656-288">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="8e656-288">**Applicable Technologies**</span></span> | <span data-ttu-id="8e656-289">MVC5</span><span class="sxs-lookup"><span data-stu-id="8e656-289">MVC5</span></span> |
| <span data-ttu-id="8e656-290">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="8e656-290">**Attributes**</span></span>              | <span data-ttu-id="8e656-291">EnvironmentType - a helyi üzemeltetésű</span><span class="sxs-lookup"><span data-stu-id="8e656-291">EnvironmentType - OnPrem</span></span> |
| <span data-ttu-id="8e656-292">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="8e656-292">**References**</span></span>              | [<span data-ttu-id="8e656-293">A Windows Identity Foundation (WIF) konfigurációja – II. rész</span><span class="sxs-lookup"><span data-stu-id="8e656-293">Windows Identity Foundation (WIF) Configuration – Part II</span></span>](https://blogs.msdn.microsoft.com/alikl/2011/02/01/windows-identity-foundation-wif-configuration-part-ii-cookiehandler-chunkedcookiehandler-customcookiehandler/) |
| <span data-ttu-id="8e656-294">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="8e656-294">**Steps**</span></span> | <span data-ttu-id="8e656-295">tooset httpOnly attribútuma FedAuth cookie-kat, hideFromCsript attribútum értékét meg kell tooTrue.</span><span class="sxs-lookup"><span data-stu-id="8e656-295">tooset httpOnly attribute for FedAuth cookies, hideFromCsript attribute value should be set tooTrue.</span></span> |

### <a name="example"></a><span data-ttu-id="8e656-296">Példa</span><span class="sxs-lookup"><span data-stu-id="8e656-296">Example</span></span>
<span data-ttu-id="8e656-297">Következő konfigurációs hello helyes konfiguráció látható:</span><span class="sxs-lookup"><span data-stu-id="8e656-297">Following configuration shows hello correct configuration:</span></span>
```XML
<federatedAuthentication>
<cookieHandler mode="Custom"
                       hideFromScript="true"
                       name="FedAuth"
                       path="/"
                       requireSsl="true"
                       persistentSessionLifetime="25">
</cookieHandler>
</federatedAuthentication>
```

## <span data-ttu-id="8e656-298"><a id="csrf-asp"></a>ASP.NET-weblapok többhelyes kérelem hamisítására (CSRF) támadások elleni</span><span class="sxs-lookup"><span data-stu-id="8e656-298"><a id="csrf-asp"></a>Mitigate against Cross-Site Request Forgery (CSRF) attacks on ASP.NET web pages</span></span>

| <span data-ttu-id="8e656-299">Cím</span><span class="sxs-lookup"><span data-stu-id="8e656-299">Title</span></span>                   | <span data-ttu-id="8e656-300">Részletek</span><span class="sxs-lookup"><span data-stu-id="8e656-300">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="8e656-301">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="8e656-301">**Component**</span></span>               | <span data-ttu-id="8e656-302">Web Application</span><span class="sxs-lookup"><span data-stu-id="8e656-302">Web Application</span></span> | 
| <span data-ttu-id="8e656-303">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="8e656-303">**SDL Phase**</span></span>               | <span data-ttu-id="8e656-304">Felépítés</span><span class="sxs-lookup"><span data-stu-id="8e656-304">Build</span></span> |  
| <span data-ttu-id="8e656-305">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="8e656-305">**Applicable Technologies**</span></span> | <span data-ttu-id="8e656-306">Általános</span><span class="sxs-lookup"><span data-stu-id="8e656-306">Generic</span></span> |
| <span data-ttu-id="8e656-307">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="8e656-307">**Attributes**</span></span>              | <span data-ttu-id="8e656-308">N/A</span><span class="sxs-lookup"><span data-stu-id="8e656-308">N/A</span></span>  |
| <span data-ttu-id="8e656-309">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="8e656-309">**References**</span></span>              | <span data-ttu-id="8e656-310">N/A</span><span class="sxs-lookup"><span data-stu-id="8e656-310">N/A</span></span>  |
| <span data-ttu-id="8e656-311">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="8e656-311">**Steps**</span></span> | <span data-ttu-id="8e656-312">Webhelyközi kérések hamisítására (CSRF vagy XSRF), amelyben a támadó műveleteket végezhet el hello biztonsági környezetében webhelyen egy másik felhasználói munkamenetet a támadás típusú.</span><span class="sxs-lookup"><span data-stu-id="8e656-312">Cross-site request forgery (CSRF or XSRF) is a type of attack in which an attacker can carry out actions in hello security context of a different user's established session on a web site.</span></span> <span data-ttu-id="8e656-313">hello cél toomodify, vagy törölje a tartalmat, ha hello célzott webhely kizárólag támaszkodik munkamenet cookie-k tooauthenticate kérelem érkezett.</span><span class="sxs-lookup"><span data-stu-id="8e656-313">hello goal is toomodify or delete content, if hello targeted web site relies exclusively on session cookies tooauthenticate received request.</span></span> <span data-ttu-id="8e656-314">A támadó a biztonsági rés beolvas egy másik felhasználó böngésző tooload egy URL-címet egy parancs egy sebezhető helyhez, amelyen hello felhasználó van bejelentkezve.</span><span class="sxs-lookup"><span data-stu-id="8e656-314">An attacker could exploit this vulnerability by getting a different user's browser tooload a URL with a command from a vulnerable site on which hello user is already logged in.</span></span> <span data-ttu-id="8e656-315">Egy támadó toodo, többek között helyez el egy másik webhely, amely betölti erőforrás hello sebezhető kiszolgálóról, vagy beolvasásakor hello felhasználói tooclick egy hivatkozást a számos módja van.</span><span class="sxs-lookup"><span data-stu-id="8e656-315">There are many ways for an attacker toodo that, such as by hosting a different web site that loads a resource from hello vulnerable server, or getting hello user tooclick a link.</span></span> <span data-ttu-id="8e656-316">hello támadás megelőzhető, ha hello kiszolgáló egy további token toohello ügyfél küldi hello ügyfél tooinclude adott lexikális elem szerepel az összes későbbi kérelmek, és ellenőrzi, hogy minden későbbi kérelmek tartalmaz egy jogkivonatot, amely számítógépfiókokhoz toohello aktuális munkamenetről, például által igényel ASP.NET AntiForgeryToken hello vagy ViewState használatával.</span><span class="sxs-lookup"><span data-stu-id="8e656-316">hello attack can be prevented if hello server sends an additional token toohello client, requires hello client tooinclude that token in all future requests, and verifies that all future requests include a token that pertains toohello current session, such as by using hello ASP.NET AntiForgeryToken or ViewState.</span></span> |

| <span data-ttu-id="8e656-317">Cím</span><span class="sxs-lookup"><span data-stu-id="8e656-317">Title</span></span>                   | <span data-ttu-id="8e656-318">Részletek</span><span class="sxs-lookup"><span data-stu-id="8e656-318">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="8e656-319">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="8e656-319">**Component**</span></span>               | <span data-ttu-id="8e656-320">Web Application</span><span class="sxs-lookup"><span data-stu-id="8e656-320">Web Application</span></span> | 
| <span data-ttu-id="8e656-321">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="8e656-321">**SDL Phase**</span></span>               | <span data-ttu-id="8e656-322">Felépítés</span><span class="sxs-lookup"><span data-stu-id="8e656-322">Build</span></span> |  
| <span data-ttu-id="8e656-323">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="8e656-323">**Applicable Technologies**</span></span> | <span data-ttu-id="8e656-324">MVC5, MVC6</span><span class="sxs-lookup"><span data-stu-id="8e656-324">MVC5, MVC6</span></span> |
| <span data-ttu-id="8e656-325">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="8e656-325">**Attributes**</span></span>              | <span data-ttu-id="8e656-326">N/A</span><span class="sxs-lookup"><span data-stu-id="8e656-326">N/A</span></span>  |
| <span data-ttu-id="8e656-327">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="8e656-327">**References**</span></span>              | [<span data-ttu-id="8e656-328">ASP.NET MVC és weblapok XSRF/CSRF megelőzése</span><span class="sxs-lookup"><span data-stu-id="8e656-328">XSRF/CSRF Prevention in ASP.NET MVC and Web Pages</span></span>](http://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) |
| <span data-ttu-id="8e656-329">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="8e656-329">**Steps**</span></span> | <span data-ttu-id="8e656-330">Kártevőirtó-CSRF és az ASP.NET MVC űrlapok - használata hello `AntiForgeryToken` segédmetódust a nézetek; put egy `Html.AntiForgeryToken()` hello űrlapra, például</span><span class="sxs-lookup"><span data-stu-id="8e656-330">Anti-CSRF and ASP.NET MVC forms - Use hello `AntiForgeryToken` helper method on Views; put an `Html.AntiForgeryToken()` into hello form, for example,</span></span>|

### <a name="example"></a><span data-ttu-id="8e656-331">Példa</span><span class="sxs-lookup"><span data-stu-id="8e656-331">Example</span></span>
```C#
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
```

### <a name="example"></a><span data-ttu-id="8e656-332">Példa</span><span class="sxs-lookup"><span data-stu-id="8e656-332">Example</span></span>
```C#
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a><span data-ttu-id="8e656-333">Példa</span><span class="sxs-lookup"><span data-stu-id="8e656-333">Example</span></span>
<span data-ttu-id="8e656-334">Hello: ugyanaz, Html.AntiForgeryToken() által biztosított hello látogató adott területre a cookie-k hívása __RequestVerificationToken, az azonos érték értékként hello véletlenszerű rejtett fent látható hello eltöltött idő</span><span class="sxs-lookup"><span data-stu-id="8e656-334">At hello same time, Html.AntiForgeryToken() gives hello visitor a cookie called __RequestVerificationToken, with hello same value as hello random hidden value shown above.</span></span> <span data-ttu-id="8e656-335">Egy bejövő közzétett űrlapból toovalidate ezután hello [ValidateAntiForgeryToken] szűrő toohello cél műveletmetódus adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="8e656-335">Next, toovalidate an incoming form post, add hello [ValidateAntiForgeryToken] filter toohello target action method.</span></span> <span data-ttu-id="8e656-336">Példa:</span><span class="sxs-lookup"><span data-stu-id="8e656-336">For example:</span></span>
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
<span data-ttu-id="8e656-337">Ellenőrzi, hogy a szűrő engedélyezési:</span><span class="sxs-lookup"><span data-stu-id="8e656-337">Authorization filter that checks that:</span></span>
* <span data-ttu-id="8e656-338">hello bejövő kérelem rendelkezik egy cookie-k hívása __RequestVerificationToken</span><span class="sxs-lookup"><span data-stu-id="8e656-338">hello incoming request has a cookie called __RequestVerificationToken</span></span>
* <span data-ttu-id="8e656-339">hello bejövő kérelem rendelkezik egy `Request.Form` __RequestVerificationToken nevezett bejegyzés</span><span class="sxs-lookup"><span data-stu-id="8e656-339">hello incoming request has a `Request.Form` entry called __RequestVerificationToken</span></span>
* <span data-ttu-id="8e656-340">A cookie-k és `Request.Form` jól értékek egyeznek, feltéve, hogy az összes, hello kérelem végighalad normál.</span><span class="sxs-lookup"><span data-stu-id="8e656-340">These cookie and `Request.Form` values match Assuming all is well, hello request goes through as normal.</span></span> <span data-ttu-id="8e656-341">De ha nem, majd üzenettel engedélyezési hiba "szükséges hamisításgátló jogkivonat nincs megadva vagy érvénytelen".</span><span class="sxs-lookup"><span data-stu-id="8e656-341">But if not, then an authorization failure with message “A required anti-forgery token was not supplied or was invalid”.</span></span> 

### <a name="example"></a><span data-ttu-id="8e656-342">Példa</span><span class="sxs-lookup"><span data-stu-id="8e656-342">Example</span></span>
<span data-ttu-id="8e656-343">Kártevőirtó-CSRF és AJAX: hello űrlap token az AJAX-kérelmek problémát okozhat, mert AJAX-kérelmet el tudja küldeni a JSON-adatokat, nem a HTML-űrlapot adatok.</span><span class="sxs-lookup"><span data-stu-id="8e656-343">Anti-CSRF and AJAX: hello form token can be a problem for AJAX requests, because an AJAX request might send JSON data, not HTML form data.</span></span> <span data-ttu-id="8e656-344">Egy megoldás, toosend hello jogkivonatok egyéni HTTP-fejlécben.</span><span class="sxs-lookup"><span data-stu-id="8e656-344">One solution is toosend hello tokens in a custom HTTP header.</span></span> <span data-ttu-id="8e656-345">hello alábbira Razor szintaxis toogenerate hello jogkivonatokat használ, és hozzáadja hello jogkivonatok tooan AJAX kérelem.</span><span class="sxs-lookup"><span data-stu-id="8e656-345">hello following code uses Razor syntax toogenerate hello tokens, and then adds hello tokens tooan AJAX request.</span></span> 
```C#
<script>
    @functions{
        public string TokenHeaderValue()
        {
            string cookieToken, formToken;
            AntiForgery.GetTokens(null, out cookieToken, out formToken);
            return cookieToken + ":" + formToken;                
        }
    }

    $.ajax("api/values", {
        type: "post",
        contentType: "application/json",
        data: {  }, // JSON data goes here
        dataType: "json",
        headers: {
            'RequestVerificationToken': '@TokenHeaderValue()'
        }
    });
</script>
```

### <a name="example"></a><span data-ttu-id="8e656-346">Példa</span><span class="sxs-lookup"><span data-stu-id="8e656-346">Example</span></span>
<span data-ttu-id="8e656-347">Hello kérelem dolgozza fel, amikor hello jogkivonatok kinyerése hello kérelemfejlécet.</span><span class="sxs-lookup"><span data-stu-id="8e656-347">When you process hello request, extract hello tokens from hello request header.</span></span> <span data-ttu-id="8e656-348">Majd metódushívás hello AntiForgery.Validate toovalidate hello jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="8e656-348">Then call hello AntiForgery.Validate method toovalidate hello tokens.</span></span> <span data-ttu-id="8e656-349">hello Validate metódusának hívása kivételt jelez, ha hello jogkivonatok nem érvényes.</span><span class="sxs-lookup"><span data-stu-id="8e656-349">hello Validate method throws an exception if hello tokens are not valid.</span></span>
```C#
void ValidateRequestHeader(HttpRequestMessage request)
{
    string cookieToken = "";
    string formToken = "";

    IEnumerable<string> tokenHeaders;
    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
    {
        string[] tokens = tokenHeaders.First().Split(':');
        if (tokens.Length == 2)
        {
            cookieToken = tokens[0].Trim();
            formToken = tokens[1].Trim();
        }
    }
    AntiForgery.Validate(cookieToken, formToken);
}
```

| <span data-ttu-id="8e656-350">Cím</span><span class="sxs-lookup"><span data-stu-id="8e656-350">Title</span></span>                   | <span data-ttu-id="8e656-351">Részletek</span><span class="sxs-lookup"><span data-stu-id="8e656-351">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="8e656-352">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="8e656-352">**Component**</span></span>               | <span data-ttu-id="8e656-353">Web Application</span><span class="sxs-lookup"><span data-stu-id="8e656-353">Web Application</span></span> | 
| <span data-ttu-id="8e656-354">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="8e656-354">**SDL Phase**</span></span>               | <span data-ttu-id="8e656-355">Felépítés</span><span class="sxs-lookup"><span data-stu-id="8e656-355">Build</span></span> |  
| <span data-ttu-id="8e656-356">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="8e656-356">**Applicable Technologies**</span></span> | <span data-ttu-id="8e656-357">Web Forms keretrendszerre</span><span class="sxs-lookup"><span data-stu-id="8e656-357">Web Forms</span></span> |
| <span data-ttu-id="8e656-358">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="8e656-358">**Attributes**</span></span>              | <span data-ttu-id="8e656-359">N/A</span><span class="sxs-lookup"><span data-stu-id="8e656-359">N/A</span></span>  |
| <span data-ttu-id="8e656-360">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="8e656-360">**References**</span></span>              | [<span data-ttu-id="8e656-361">Hajtsa végre a megfelelő előny az ASP.NET beépített szolgáltatásai tooFend ki webes támadások</span><span class="sxs-lookup"><span data-stu-id="8e656-361">Take Advantage of ASP.NET Built-in Features tooFend Off Web Attacks</span></span>](https://msdn.microsoft.com/library/ms972969.aspx#securitybarriers_topic2) |
| <span data-ttu-id="8e656-362">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="8e656-362">**Steps**</span></span> | <span data-ttu-id="8e656-363">A webes alapú alkalmazásokban CSRF támadások enyhíthetők ViewStateUserKey tooa véletlenszerű karakterlánc változó beállítása minden felhasználó – felhasználói Azonosítót, vagy jobban még, munkamenet-azonosító.</span><span class="sxs-lookup"><span data-stu-id="8e656-363">CSRF attacks in WebForm based applications can be mitigated by setting ViewStateUserKey tooa random string that varies for each user - user ID or, better yet, session ID.</span></span> <span data-ttu-id="8e656-364">Műszaki és közösségi okokból számos, a munkamenet-azonosító egy javulás méretezése azért, mert egy munkamenet-azonosító előre nem látható, túllépi az időkorlátot, és a felhasználónkénti alapon változik.</span><span class="sxs-lookup"><span data-stu-id="8e656-364">For a number of technical and social reasons, session ID is a much better fit because a session ID is unpredictable, times out, and varies on a per-user basis.</span></span>|

### <a name="example"></a><span data-ttu-id="8e656-365">Példa</span><span class="sxs-lookup"><span data-stu-id="8e656-365">Example</span></span>
<span data-ttu-id="8e656-366">Az összes weblapot toohave szükséges hello kód itt található:</span><span class="sxs-lookup"><span data-stu-id="8e656-366">Here's hello code you need toohave in all of your pages:</span></span>
```C#
void Page_Init (object sender, EventArgs e) {
   ViewStateUserKey = Session.SessionID;
   :
}
```

## <span data-ttu-id="8e656-367"><a id="inactivity-lifetime"></a>Állítsa be a munkamenet inaktivitás élettartama</span><span class="sxs-lookup"><span data-stu-id="8e656-367"><a id="inactivity-lifetime"></a>Set up session for inactivity lifetime</span></span>

| <span data-ttu-id="8e656-368">Cím</span><span class="sxs-lookup"><span data-stu-id="8e656-368">Title</span></span>                   | <span data-ttu-id="8e656-369">Részletek</span><span class="sxs-lookup"><span data-stu-id="8e656-369">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="8e656-370">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="8e656-370">**Component**</span></span>               | <span data-ttu-id="8e656-371">Web Application</span><span class="sxs-lookup"><span data-stu-id="8e656-371">Web Application</span></span> | 
| <span data-ttu-id="8e656-372">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="8e656-372">**SDL Phase**</span></span>               | <span data-ttu-id="8e656-373">Felépítés</span><span class="sxs-lookup"><span data-stu-id="8e656-373">Build</span></span> |  
| <span data-ttu-id="8e656-374">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="8e656-374">**Applicable Technologies**</span></span> | <span data-ttu-id="8e656-375">Általános</span><span class="sxs-lookup"><span data-stu-id="8e656-375">Generic</span></span> |
| <span data-ttu-id="8e656-376">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="8e656-376">**Attributes**</span></span>              | <span data-ttu-id="8e656-377">N/A</span><span class="sxs-lookup"><span data-stu-id="8e656-377">N/A</span></span>  |
| <span data-ttu-id="8e656-378">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="8e656-378">**References**</span></span>              | <span data-ttu-id="8e656-379">[HttpSessionState.Timeout tulajdonság](https://msdn.microsoft.com/library/system.web.sessionstate.httpsessionstate.timeout(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="8e656-379">[HttpSessionState.Timeout Property](https://msdn.microsoft.com/library/system.web.sessionstate.httpsessionstate.timeout(v=vs.110).aspx)</span></span> |
| <span data-ttu-id="8e656-380">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="8e656-380">**Steps**</span></span> | <span data-ttu-id="8e656-381">Munkamenet időkorlátja jelöli, amikor a felhasználó nem bármely művelet elvégzésére webhelyen (a webkiszolgáló által megadott) egy időszakban bekövetkező hello esemény.</span><span class="sxs-lookup"><span data-stu-id="8e656-381">Session timeout represents hello event occurring when a user does not perform any action on a web site during a interval (defined by web server).</span></span> <span data-ttu-id="8e656-382">hello esemény, a kiszolgáló oldalán, hello felhasználói munkamenet too'invalid hello állapotának módosítása "(például" nem használható többé") és utasítsa hello web server toodestroy azt (bele tárolt összes adat törlése).</span><span class="sxs-lookup"><span data-stu-id="8e656-382">hello event, on server side, change hello status of hello user session too'invalid' (for example  "not used anymore") and instruct hello web server toodestroy it (deleting all data contained into it).</span></span> <span data-ttu-id="8e656-383">hello alábbi példakód beállítja hello időtúllépés munkamenet attribútum too15 perc hello Web.config fájlban.</span><span class="sxs-lookup"><span data-stu-id="8e656-383">hello following code example sets hello timeout session attribute too15 minutes in hello Web.config file.</span></span>|

### <a name="example"></a><span data-ttu-id="8e656-384">Példa</span><span class="sxs-lookup"><span data-stu-id="8e656-384">Example</span></span>
<span data-ttu-id="8e656-385">A(z) "XML-kódot <configuration> < system.web > <sessionState mode="InProc" cookieless="true" timeout="15" /> < /system.web ></configuration></span><span class="sxs-lookup"><span data-stu-id="8e656-385">``\`XML code <configuration> <system.web> <sessionState mode="InProc" cookieless="true" timeout="15" /> </system.web> </configuration></span></span>
```

## <a id="threat-detection"></a>Enable Threat detection on Azure SQL
```

| <span data-ttu-id="8e656-386">Cím</span><span class="sxs-lookup"><span data-stu-id="8e656-386">Title</span></span>                   | <span data-ttu-id="8e656-387">Részletek</span><span class="sxs-lookup"><span data-stu-id="8e656-387">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="8e656-388">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="8e656-388">**Component**</span></span>               | <span data-ttu-id="8e656-389">Web Application</span><span class="sxs-lookup"><span data-stu-id="8e656-389">Web Application</span></span> | 
| <span data-ttu-id="8e656-390">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="8e656-390">**SDL Phase**</span></span>               | <span data-ttu-id="8e656-391">Felépítés</span><span class="sxs-lookup"><span data-stu-id="8e656-391">Build</span></span> |  
| <span data-ttu-id="8e656-392">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="8e656-392">**Applicable Technologies**</span></span> | <span data-ttu-id="8e656-393">Web Forms keretrendszerre</span><span class="sxs-lookup"><span data-stu-id="8e656-393">Web Forms</span></span> |
| <span data-ttu-id="8e656-394">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="8e656-394">**Attributes**</span></span>              | <span data-ttu-id="8e656-395">N/A</span><span class="sxs-lookup"><span data-stu-id="8e656-395">N/A</span></span>  |
| <span data-ttu-id="8e656-396">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="8e656-396">**References**</span></span>              | <span data-ttu-id="8e656-397">[Elem űrlap-hitelesítés (ASP.NET beállítási séma)](https://msdn.microsoft.com/library/1d3t3c61(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="8e656-397">[forms Element for authentication (ASP.NET Settings Schema)](https://msdn.microsoft.com/library/1d3t3c61(v=vs.100).aspx)</span></span> |
| <span data-ttu-id="8e656-398">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="8e656-398">**Steps**</span></span> | <span data-ttu-id="8e656-399">Állítsa be az űrlapos hitelesítési jegyet cookie-k időtúllépési too15 hello perc</span><span class="sxs-lookup"><span data-stu-id="8e656-399">Set hello Forms Authentication Ticket cookie timeout too15 minutes</span></span>|

### <a name="example"></a><span data-ttu-id="8e656-400">Példa</span><span class="sxs-lookup"><span data-stu-id="8e656-400">Example</span></span>
<span data-ttu-id="8e656-401">A(z) "XML-kódot<forms  name=".ASPXAUTH" loginUrl="login.aspx"  defaultUrl="default.aspx" protection="All" timeout="15" path="/" requireSSL="true" slidingExpiration="true"/>
</forms></span><span class="sxs-lookup"><span data-stu-id="8e656-401">``\`XML code <forms  name=".ASPXAUTH" loginUrl="login.aspx"  defaultUrl="default.aspx" protection="All" timeout="15" path="/" requireSSL="true" slidingExpiration="true"/>
</forms></span></span>
```

| Title                   | Details      |
| ----------------------- | ------------ |
| **Component**               | Web Application | 
| **SDL Phase**               | Build |  
| **Applicable Technologies** | Web Forms, MVC5 |
| **Attributes**              | EnvironmentType - OnPrem |
| **References**              | [asdeqa](https://skf.azurewebsites.net/Mitigations/Details/wefr) |
| **Steps** | When hello web application is Relying Party and ADFS is hello STS, hello lifetime of hello authentication cookies - FedAuth tokens - can be set by hello following configuration in web.config:|

### Example
```XML
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:15:0" />
      <!-- Set requireHttps=true; -->
      <wsFederation passiveRedirectEnabled="true" issuer="http://localhost:39529/" realm="https://localhost:44302/" reply="https://localhost:44302/" requireHttps="true"/>
      <!--
      Use hello code below tooenable encryption-decryption of claims received from ADFS. Thumbprint value varies based on hello certificate being used.
      <serviceCertificate>
        <certificateReference findValue="4FBBBA33A1D11A9022A5BF3492FF83320007686A" storeLocation="LocalMachine" storeName="My" x509FindType="FindByThumbprint" />
      </serviceCertificate>
      -->
    </federationConfiguration>
  </system.identityModel.services>
```

### <a name="example"></a><span data-ttu-id="8e656-402">Példa</span><span class="sxs-lookup"><span data-stu-id="8e656-402">Example</span></span>
<span data-ttu-id="8e656-403">Is SAML jogcímek jogkivonat élettartama kiadott AD FS beállításaként too15 hello perc, a következő powershell-parancs hello ADFS-kiszolgálón a következő hello futtatásával:</span><span class="sxs-lookup"><span data-stu-id="8e656-403">Also hello ADFS issued SAML claim token's lifetime should be set too15 minutes, by executing hello following powershell command on hello ADFS server:</span></span>
```C#
Set-ADFSRelyingPartyTrust -TargetName “<RelyingPartyWebApp>” -ClaimsProviderName @(“Active Directory”) -TokenLifetime 15 -AlwaysRequireAuthentication $true
```

## <span data-ttu-id="8e656-404"><a id="proper-app-logout"></a>Alkalmazzon megfelelő kijelentkezési hello alkalmazásból</span><span class="sxs-lookup"><span data-stu-id="8e656-404"><a id="proper-app-logout"></a>Implement proper logout from hello application</span></span>

| <span data-ttu-id="8e656-405">Cím</span><span class="sxs-lookup"><span data-stu-id="8e656-405">Title</span></span>                   | <span data-ttu-id="8e656-406">Részletek</span><span class="sxs-lookup"><span data-stu-id="8e656-406">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="8e656-407">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="8e656-407">**Component**</span></span>               | <span data-ttu-id="8e656-408">Web Application</span><span class="sxs-lookup"><span data-stu-id="8e656-408">Web Application</span></span> | 
| <span data-ttu-id="8e656-409">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="8e656-409">**SDL Phase**</span></span>               | <span data-ttu-id="8e656-410">Felépítés</span><span class="sxs-lookup"><span data-stu-id="8e656-410">Build</span></span> |  
| <span data-ttu-id="8e656-411">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="8e656-411">**Applicable Technologies**</span></span> | <span data-ttu-id="8e656-412">Általános</span><span class="sxs-lookup"><span data-stu-id="8e656-412">Generic</span></span> |
| <span data-ttu-id="8e656-413">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="8e656-413">**Attributes**</span></span>              | <span data-ttu-id="8e656-414">N/A</span><span class="sxs-lookup"><span data-stu-id="8e656-414">N/A</span></span>  |
| <span data-ttu-id="8e656-415">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="8e656-415">**References**</span></span>              | <span data-ttu-id="8e656-416">N/A</span><span class="sxs-lookup"><span data-stu-id="8e656-416">N/A</span></span>  |
| <span data-ttu-id="8e656-417">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="8e656-417">**Steps**</span></span> | <span data-ttu-id="8e656-418">Hajtsa végre megfelelő kijelentkezés hello alkalmazásból, amikor a felhasználó présgépet Kijelentkezés gombra.</span><span class="sxs-lookup"><span data-stu-id="8e656-418">Perform proper Sign Out from hello application, when user presses log out button.</span></span> <span data-ttu-id="8e656-419">Követően jelentkezzen ki alkalmazás kell semmisítse meg a felhasználó munkamenetét, és is alaphelyzetbe állítása és érvényteleníti a munkamenet cookie-értéket alaphelyzetbe állítása és a hitelesítési cookie-értéket érvénytelenítését együtt.</span><span class="sxs-lookup"><span data-stu-id="8e656-419">Upon logout, application should destroy user's session, and also reset and nullify session cookie value, along with resetting and nullifying authentication cookie value.</span></span> <span data-ttu-id="8e656-420">Is ha több munkamenetet a feltételekhez tooa egyetlen felhasználói identitást, azok kell együttesen lezárni hello kiszolgáló oldalán, időtúllépés vagy jelentkezzen ki.</span><span class="sxs-lookup"><span data-stu-id="8e656-420">Also, when multiple sessions are tied tooa single user identity, they must be collectively terminated on hello server side at timeout or logout.</span></span> <span data-ttu-id="8e656-421">Végül győződjön meg arról, hogy kijelentkezési funkció minden oldalon érhető el.</span><span class="sxs-lookup"><span data-stu-id="8e656-421">Lastly, ensure that Logout functionality is available on every page.</span></span> |

## <span data-ttu-id="8e656-422"><a id="csrf-api"></a>ASP.NET webes API-k többhelyes kérelem hamisítására (CSRF) támadások elleni</span><span class="sxs-lookup"><span data-stu-id="8e656-422"><a id="csrf-api"></a>Mitigate against Cross-Site Request Forgery (CSRF) attacks on ASP.NET Web APIs</span></span>

| <span data-ttu-id="8e656-423">Cím</span><span class="sxs-lookup"><span data-stu-id="8e656-423">Title</span></span>                   | <span data-ttu-id="8e656-424">Részletek</span><span class="sxs-lookup"><span data-stu-id="8e656-424">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="8e656-425">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="8e656-425">**Component**</span></span>               | <span data-ttu-id="8e656-426">Webes API</span><span class="sxs-lookup"><span data-stu-id="8e656-426">Web API</span></span> | 
| <span data-ttu-id="8e656-427">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="8e656-427">**SDL Phase**</span></span>               | <span data-ttu-id="8e656-428">Felépítés</span><span class="sxs-lookup"><span data-stu-id="8e656-428">Build</span></span> |  
| <span data-ttu-id="8e656-429">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="8e656-429">**Applicable Technologies**</span></span> | <span data-ttu-id="8e656-430">Általános</span><span class="sxs-lookup"><span data-stu-id="8e656-430">Generic</span></span> |
| <span data-ttu-id="8e656-431">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="8e656-431">**Attributes**</span></span>              | <span data-ttu-id="8e656-432">N/A</span><span class="sxs-lookup"><span data-stu-id="8e656-432">N/A</span></span>  |
| <span data-ttu-id="8e656-433">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="8e656-433">**References**</span></span>              | <span data-ttu-id="8e656-434">N/A</span><span class="sxs-lookup"><span data-stu-id="8e656-434">N/A</span></span>  |
| <span data-ttu-id="8e656-435">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="8e656-435">**Steps**</span></span> | <span data-ttu-id="8e656-436">Webhelyközi kérések hamisítására (CSRF vagy XSRF), amelyben a támadó műveleteket végezhet el hello biztonsági környezetében webhelyen egy másik felhasználói munkamenetet a támadás típusú.</span><span class="sxs-lookup"><span data-stu-id="8e656-436">Cross-site request forgery (CSRF or XSRF) is a type of attack in which an attacker can carry out actions in hello security context of a different user's established session on a web site.</span></span> <span data-ttu-id="8e656-437">hello cél toomodify, vagy törölje a tartalmat, ha hello célzott webhely kizárólag támaszkodik munkamenet cookie-k tooauthenticate kérelem érkezett.</span><span class="sxs-lookup"><span data-stu-id="8e656-437">hello goal is toomodify or delete content, if hello targeted web site relies exclusively on session cookies tooauthenticate received request.</span></span> <span data-ttu-id="8e656-438">A támadó a biztonsági rés beolvas egy másik felhasználó böngésző tooload egy URL-címet egy parancs egy sebezhető helyhez, amelyen hello felhasználó van bejelentkezve.</span><span class="sxs-lookup"><span data-stu-id="8e656-438">An attacker could exploit this vulnerability by getting a different user's browser tooload a URL with a command from a vulnerable site on which hello user is already logged in.</span></span> <span data-ttu-id="8e656-439">Egy támadó toodo, többek között helyez el egy másik webhely, amely betölti erőforrás hello sebezhető kiszolgálóról, vagy beolvasásakor hello felhasználói tooclick egy hivatkozást a számos módja van.</span><span class="sxs-lookup"><span data-stu-id="8e656-439">There are many ways for an attacker toodo that, such as by hosting a different web site that loads a resource from hello vulnerable server, or getting hello user tooclick a link.</span></span> <span data-ttu-id="8e656-440">hello támadás megelőzhető, ha hello kiszolgáló egy további token toohello ügyfél küldi hello ügyfél tooinclude adott lexikális elem szerepel az összes későbbi kérelmek, és ellenőrzi, hogy minden későbbi kérelmek tartalmaz egy jogkivonatot, amely számítógépfiókokhoz toohello aktuális munkamenetről, például által igényel ASP.NET AntiForgeryToken hello vagy ViewState használatával.</span><span class="sxs-lookup"><span data-stu-id="8e656-440">hello attack can be prevented if hello server sends an additional token toohello client, requires hello client tooinclude that token in all future requests, and verifies that all future requests include a token that pertains toohello current session, such as by using hello ASP.NET AntiForgeryToken or ViewState.</span></span> |

| <span data-ttu-id="8e656-441">Cím</span><span class="sxs-lookup"><span data-stu-id="8e656-441">Title</span></span>                   | <span data-ttu-id="8e656-442">Részletek</span><span class="sxs-lookup"><span data-stu-id="8e656-442">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="8e656-443">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="8e656-443">**Component**</span></span>               | <span data-ttu-id="8e656-444">Webes API</span><span class="sxs-lookup"><span data-stu-id="8e656-444">Web API</span></span> | 
| <span data-ttu-id="8e656-445">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="8e656-445">**SDL Phase**</span></span>               | <span data-ttu-id="8e656-446">Felépítés</span><span class="sxs-lookup"><span data-stu-id="8e656-446">Build</span></span> |  
| <span data-ttu-id="8e656-447">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="8e656-447">**Applicable Technologies**</span></span> | <span data-ttu-id="8e656-448">MVC5, MVC6</span><span class="sxs-lookup"><span data-stu-id="8e656-448">MVC5, MVC6</span></span> |
| <span data-ttu-id="8e656-449">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="8e656-449">**Attributes**</span></span>              | <span data-ttu-id="8e656-450">N/A</span><span class="sxs-lookup"><span data-stu-id="8e656-450">N/A</span></span>  |
| <span data-ttu-id="8e656-451">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="8e656-451">**References**</span></span>              | [<span data-ttu-id="8e656-452">ASP.NET webes API-t a Webhelyközi kérések hamisítására (CSRF) támadások megelőzése</span><span class="sxs-lookup"><span data-stu-id="8e656-452">Preventing Cross-Site Request Forgery (CSRF) Attacks in ASP.NET Web API</span></span>](http://www.asp.net/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks) |
| <span data-ttu-id="8e656-453">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="8e656-453">**Steps**</span></span> | <span data-ttu-id="8e656-454">Kártevőirtó-CSRF és AJAX: hello űrlap token az AJAX-kérelmek problémát okozhat, mert AJAX-kérelmet el tudja küldeni a JSON-adatokat, nem a HTML-űrlapot adatok.</span><span class="sxs-lookup"><span data-stu-id="8e656-454">Anti-CSRF and AJAX: hello form token can be a problem for AJAX requests, because an AJAX request might send JSON data, not HTML form data.</span></span> <span data-ttu-id="8e656-455">Egy megoldás, toosend hello jogkivonatok egyéni HTTP-fejlécben.</span><span class="sxs-lookup"><span data-stu-id="8e656-455">One solution is toosend hello tokens in a custom HTTP header.</span></span> <span data-ttu-id="8e656-456">hello alábbira Razor szintaxis toogenerate hello jogkivonatokat használ, és hozzáadja hello jogkivonatok tooan AJAX kérelem.</span><span class="sxs-lookup"><span data-stu-id="8e656-456">hello following code uses Razor syntax toogenerate hello tokens, and then adds hello tokens tooan AJAX request.</span></span> |

### <a name="example"></a><span data-ttu-id="8e656-457">Példa</span><span class="sxs-lookup"><span data-stu-id="8e656-457">Example</span></span>
```Javascript
<script>
    @functions{
        public string TokenHeaderValue()
        {
            string cookieToken, formToken;
            AntiForgery.GetTokens(null, out cookieToken, out formToken);
            return cookieToken + ":" + formToken;                
        }
    }
    $.ajax("api/values", {
        type: "post",
        contentType: "application/json",
        data: {  }, // JSON data goes here
        dataType: "json",
        headers: {
            'RequestVerificationToken': '@TokenHeaderValue()'
        }
    });
</script>
```

### <a name="example"></a><span data-ttu-id="8e656-458">Példa</span><span class="sxs-lookup"><span data-stu-id="8e656-458">Example</span></span>
<span data-ttu-id="8e656-459">Hello kérelem dolgozza fel, amikor hello jogkivonatok kinyerése hello kérelemfejlécet.</span><span class="sxs-lookup"><span data-stu-id="8e656-459">When you process hello request, extract hello tokens from hello request header.</span></span> <span data-ttu-id="8e656-460">Majd metódushívás hello AntiForgery.Validate toovalidate hello jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="8e656-460">Then call hello AntiForgery.Validate method toovalidate hello tokens.</span></span> <span data-ttu-id="8e656-461">hello Validate metódusának hívása kivételt jelez, ha hello jogkivonatok nem érvényes.</span><span class="sxs-lookup"><span data-stu-id="8e656-461">hello Validate method throws an exception if hello tokens are not valid.</span></span>
```C#
void ValidateRequestHeader(HttpRequestMessage request)
{
    string cookieToken = "";
    string formToken = "";

    IEnumerable<string> tokenHeaders;
    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
    {
        string[] tokens = tokenHeaders.First().Split(':');
        if (tokens.Length == 2)
        {
            cookieToken = tokens[0].Trim();
            formToken = tokens[1].Trim();
        }
    }
    AntiForgery.Validate(cookieToken, formToken);
}
```

### <a name="example"></a><span data-ttu-id="8e656-462">Példa</span><span class="sxs-lookup"><span data-stu-id="8e656-462">Example</span></span>
<span data-ttu-id="8e656-463">Kártevőirtó-CSRF és ASP.NET MVC űrlapok - használata hello AntiForgeryToken segédmetódust a nézetet. például egy Html.AntiForgeryToken() üzembe hello képernyő</span><span class="sxs-lookup"><span data-stu-id="8e656-463">Anti-CSRF and ASP.NET MVC forms - Use hello AntiForgeryToken helper method on Views; put an Html.AntiForgeryToken() into hello form, for example,</span></span>
```C#
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
}
```

### <a name="example"></a><span data-ttu-id="8e656-464">Példa</span><span class="sxs-lookup"><span data-stu-id="8e656-464">Example</span></span>
<span data-ttu-id="8e656-465">a fenti hello példa fog kimeneti valami hasonló hello:</span><span class="sxs-lookup"><span data-stu-id="8e656-465">hello example above will output something like hello following:</span></span>
```C#
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a><span data-ttu-id="8e656-466">Példa</span><span class="sxs-lookup"><span data-stu-id="8e656-466">Example</span></span>
<span data-ttu-id="8e656-467">Hello: ugyanaz, Html.AntiForgeryToken() által biztosított hello látogató adott területre a cookie-k hívása __RequestVerificationToken, az azonos érték értékként hello véletlenszerű rejtett fent látható hello eltöltött idő</span><span class="sxs-lookup"><span data-stu-id="8e656-467">At hello same time, Html.AntiForgeryToken() gives hello visitor a cookie called __RequestVerificationToken, with hello same value as hello random hidden value shown above.</span></span> <span data-ttu-id="8e656-468">Egy bejövő közzétett űrlapból toovalidate ezután hello [ValidateAntiForgeryToken] szűrő toohello cél műveletmetódus adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="8e656-468">Next, toovalidate an incoming form post, add hello [ValidateAntiForgeryToken] filter toohello target action method.</span></span> <span data-ttu-id="8e656-469">Példa:</span><span class="sxs-lookup"><span data-stu-id="8e656-469">For example:</span></span>
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
<span data-ttu-id="8e656-470">Ellenőrzi, hogy a szűrő engedélyezési:</span><span class="sxs-lookup"><span data-stu-id="8e656-470">Authorization filter that checks that:</span></span>
* <span data-ttu-id="8e656-471">hello bejövő kérelem rendelkezik egy cookie-k hívása __RequestVerificationToken</span><span class="sxs-lookup"><span data-stu-id="8e656-471">hello incoming request has a cookie called __RequestVerificationToken</span></span>
* <span data-ttu-id="8e656-472">hello bejövő kérelem rendelkezik egy `Request.Form` __RequestVerificationToken nevezett bejegyzés</span><span class="sxs-lookup"><span data-stu-id="8e656-472">hello incoming request has a `Request.Form` entry called __RequestVerificationToken</span></span>
* <span data-ttu-id="8e656-473">A cookie-k és `Request.Form` jól értékek egyeznek, feltéve, hogy az összes, hello kérelem végighalad normál.</span><span class="sxs-lookup"><span data-stu-id="8e656-473">These cookie and `Request.Form` values match Assuming all is well, hello request goes through as normal.</span></span> <span data-ttu-id="8e656-474">De ha nem, majd üzenettel engedélyezési hiba "szükséges hamisításgátló jogkivonat nincs megadva vagy érvénytelen".</span><span class="sxs-lookup"><span data-stu-id="8e656-474">But if not, then an authorization failure with message “A required anti-forgery token was not supplied or was invalid”.</span></span>

| <span data-ttu-id="8e656-475">Cím</span><span class="sxs-lookup"><span data-stu-id="8e656-475">Title</span></span>                   | <span data-ttu-id="8e656-476">Részletek</span><span class="sxs-lookup"><span data-stu-id="8e656-476">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="8e656-477">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="8e656-477">**Component**</span></span>               | <span data-ttu-id="8e656-478">Webes API</span><span class="sxs-lookup"><span data-stu-id="8e656-478">Web API</span></span> | 
| <span data-ttu-id="8e656-479">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="8e656-479">**SDL Phase**</span></span>               | <span data-ttu-id="8e656-480">Felépítés</span><span class="sxs-lookup"><span data-stu-id="8e656-480">Build</span></span> |  
| <span data-ttu-id="8e656-481">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="8e656-481">**Applicable Technologies**</span></span> | <span data-ttu-id="8e656-482">MVC5, MVC6</span><span class="sxs-lookup"><span data-stu-id="8e656-482">MVC5, MVC6</span></span> |
| <span data-ttu-id="8e656-483">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="8e656-483">**Attributes**</span></span>              | <span data-ttu-id="8e656-484">Identity Provider - ADFS, identitásszolgáltató - az Azure AD</span><span class="sxs-lookup"><span data-stu-id="8e656-484">Identity Provider - ADFS, Identity Provider - Azure AD</span></span> |
| <span data-ttu-id="8e656-485">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="8e656-485">**References**</span></span>              | [<span data-ttu-id="8e656-486">Egyes partnerek és az ASP.NET Web API 2.2 helyi bejelentkezési webes API biztonságossá tétele</span><span class="sxs-lookup"><span data-stu-id="8e656-486">Secure a Web API with Individual Accounts and Local Login in ASP.NET Web API 2.2</span></span>](http://www.asp.net/web-api/overview/security/individual-accounts-in-web-api) |
| <span data-ttu-id="8e656-487">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="8e656-487">**Steps**</span></span> | <span data-ttu-id="8e656-488">Ha a webes API használatával lett biztonságossá téve OAuth 2.0-s, akkor azt hello egy tulajdonosi jogkivonatot csak akkor, ha hello jogkivonat érvényes engedélyezési kérelem fejléc és biztosít toohello kérést vár.</span><span class="sxs-lookup"><span data-stu-id="8e656-488">If hello Web API is secured using OAuth 2.0, then it expects a bearer token in Authorization request header and grants access toohello request only if hello token is valid.</span></span> <span data-ttu-id="8e656-489">Cookie-alapú hitelesítés, ellentétben a böngészők nem csatolható hello tulajdonosi jogkivonatok toorequests.</span><span class="sxs-lookup"><span data-stu-id="8e656-489">Unlike cookie based authentication, browsers do not attach hello bearer tokens toorequests.</span></span> <span data-ttu-id="8e656-490">hello tulajdonosi jogkivonattal, a kérelem fejlécében hello hello kérő ügyfél tooexplicitly kell csatolni.</span><span class="sxs-lookup"><span data-stu-id="8e656-490">hello requesting client needs tooexplicitly attach hello bearer token in hello request header.</span></span> <span data-ttu-id="8e656-491">Ezért az ASP.NET Web API-k OAuth 2.0 használatával védett, tulajdonosi jogkivonatok számít egy CSRF támadások elleni védelmet.</span><span class="sxs-lookup"><span data-stu-id="8e656-491">Therefore, for ASP.NET Web APIs protected using OAuth 2.0, bearer tokens are considered as a defense against CSRF attacks.</span></span> <span data-ttu-id="8e656-492">Adjon ne feledje, hogy ha hello alkalmazás hello MVC része az űrlapos hitelesítés (azaz használ cookie-k) használ, hamisítás jogkivonatok toobe hello MVC webalkalmazás használják.</span><span class="sxs-lookup"><span data-stu-id="8e656-492">Please note that if hello MVC portion of hello application uses forms authentication (i.e., uses cookies), anti-forgery tokens have toobe used by hello MVC web app.</span></span> |

### <a name="example"></a><span data-ttu-id="8e656-493">Példa</span><span class="sxs-lookup"><span data-stu-id="8e656-493">Example</span></span>
<span data-ttu-id="8e656-494">Webes API hello rendelkezik toobe toorely küldjenek csak tulajdonosi jogkivonatokat és nem a cookie-kat.</span><span class="sxs-lookup"><span data-stu-id="8e656-494">hello Web API has toobe informed toorely ONLY on bearer tokens and not on cookies.</span></span> <span data-ttu-id="8e656-495">A konfiguráció a következő hello úgy teheti `WebApiConfig.Register` metódus: "" C-éles kód config. SuppressDefaultHostAuthentication(); Config. Filters.Add (új HostAuthenticationFilter(OAuthDefaults.AuthenticationType));</span><span class="sxs-lookup"><span data-stu-id="8e656-495">It can be done by hello following configuration in `WebApiConfig.Register` method: ``\`C-Sharp code config.SuppressDefaultHostAuthentication(); config.Filters.Add(new HostAuthenticationFilter(OAuthDefaults.AuthenticationType));</span></span>
```
hello SuppressDefaultHostAuthentication method tells Web API tooignore any authentication that happens before hello request reaches hello Web API pipeline, either by IIS or by OWIN middleware. That way, we can restrict Web API tooauthenticate only using bearer tokens.
