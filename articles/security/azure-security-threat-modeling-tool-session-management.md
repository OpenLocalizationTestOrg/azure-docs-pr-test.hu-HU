---
title: "Munkamenet - Microsoft fenyegetés modellezési eszköz - kezelés Azure |} Microsoft Docs"
description: "a fenyegetések modellezése eszköz felfedett fenyegetések megoldást"
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
ms.openlocfilehash: 56471d8ef68eacacb3ecebad5056d7e7a9f3ca40
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="security-frame-session-management--articles"></a><span data-ttu-id="1c796-103">Biztonsági keret: Munkamenet-kezelés |} Cikkek</span><span class="sxs-lookup"><span data-stu-id="1c796-103">Security Frame: Session Management | Articles</span></span> 
| <span data-ttu-id="1c796-104">A termék vagy szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="1c796-104">Product/Service</span></span> | <span data-ttu-id="1c796-105">Cikk</span><span class="sxs-lookup"><span data-stu-id="1c796-105">Article</span></span> |
| --------------- | ------- |
| <span data-ttu-id="1c796-106">**Azure AD**</span><span class="sxs-lookup"><span data-stu-id="1c796-106">**Azure AD**</span></span>    | <ul><li>[<span data-ttu-id="1c796-107">Alkalmazzon megfelelő kijelentkezési ADAL módszerek használatát, ha az Azure AD segítségével</span><span class="sxs-lookup"><span data-stu-id="1c796-107">Implement proper logout using ADAL methods when using Azure AD</span></span>](#logout-adal)</li></ul> |
| <span data-ttu-id="1c796-108">IoT-eszközök</span><span class="sxs-lookup"><span data-stu-id="1c796-108">IoT Device</span></span> | <ul><li>[<span data-ttu-id="1c796-109">Generált SaS-tokenje véges élettartamai használata</span><span class="sxs-lookup"><span data-stu-id="1c796-109">Use finite lifetimes for generated SaS tokens</span></span>](#finite-tokens)</li></ul> |
| <span data-ttu-id="1c796-110">**Az Azure Document DB rendszerbe**</span><span class="sxs-lookup"><span data-stu-id="1c796-110">**Azure Document DB**</span></span> | <ul><li>[<span data-ttu-id="1c796-111">A létrehozott erőforrás-jogkivonatokat minimális jogkivonat élettartamát használata</span><span class="sxs-lookup"><span data-stu-id="1c796-111">Use minimum token lifetimes for generated Resource tokens</span></span>](#resource-tokens)</li></ul> |
| <span data-ttu-id="1c796-112">**ADFS**</span><span class="sxs-lookup"><span data-stu-id="1c796-112">**ADFS**</span></span> | <ul><li>[<span data-ttu-id="1c796-113">Alkalmazzon megfelelő kijelentkezési WsFederation módszerek használatát, ha az AD FS használatával</span><span class="sxs-lookup"><span data-stu-id="1c796-113">Implement proper logout using WsFederation methods when using ADFS</span></span>](#wsfederation-logout)</li></ul> |
| <span data-ttu-id="1c796-114">**Identity Serverben**</span><span class="sxs-lookup"><span data-stu-id="1c796-114">**Identity Server**</span></span> | <ul><li>[<span data-ttu-id="1c796-115">Alkalmazzon megfelelő kijelentkezési Identitáskiszolgálók használatakor</span><span class="sxs-lookup"><span data-stu-id="1c796-115">Implement proper logout when using Identity Server</span></span>](#proper-logout)</li></ul> |
| <span data-ttu-id="1c796-116">**Webalkalmazás**</span><span class="sxs-lookup"><span data-stu-id="1c796-116">**Web Application**</span></span> | <ul><li>[<span data-ttu-id="1c796-117">HTTPS-KAPCSOLATON keresztül elérhető alkalmazások biztonságos cookie-kat kell használni.</span><span class="sxs-lookup"><span data-stu-id="1c796-117">Applications available over HTTPS must use secure cookies</span></span>](#https-secure-cookies)</li><li>[<span data-ttu-id="1c796-118">Az összes HTTP-alapú alkalmazások csak olyan cookie-k meghatározása http kell megadni</span><span class="sxs-lookup"><span data-stu-id="1c796-118">All http based application should specify http only for cookie definition</span></span>](#cookie-definition)</li><li>[<span data-ttu-id="1c796-119">ASP.NET-weblapok többhelyes kérelem hamisítására (CSRF) támadások elleni</span><span class="sxs-lookup"><span data-stu-id="1c796-119">Mitigate against Cross-Site Request Forgery (CSRF) attacks on ASP.NET web pages</span></span>](#csrf-asp)</li><li>[<span data-ttu-id="1c796-120">Állítsa be a munkamenet inaktivitás élettartama</span><span class="sxs-lookup"><span data-stu-id="1c796-120">Set up session for inactivity lifetime</span></span>](#inactivity-lifetime)</li><li>[<span data-ttu-id="1c796-121">Az alkalmazás megfelelő kijelentkezési megvalósítása</span><span class="sxs-lookup"><span data-stu-id="1c796-121">Implement proper logout from the application</span></span>](#proper-app-logout)</li></ul> |
| <span data-ttu-id="1c796-122">**Webes API**</span><span class="sxs-lookup"><span data-stu-id="1c796-122">**Web API**</span></span> | <ul><li>[<span data-ttu-id="1c796-123">ASP.NET webes API-k többhelyes kérelem hamisítására (CSRF) támadások elleni</span><span class="sxs-lookup"><span data-stu-id="1c796-123">Mitigate against Cross-Site Request Forgery (CSRF) attacks on ASP.NET Web APIs</span></span>](#csrf-api)</li></ul> |

## <span data-ttu-id="1c796-124"><a id="logout-adal"></a>Alkalmazzon megfelelő kijelentkezési ADAL módszerek használatát, ha az Azure AD segítségével</span><span class="sxs-lookup"><span data-stu-id="1c796-124"><a id="logout-adal"></a>Implement proper logout using ADAL methods when using Azure AD</span></span>

| <span data-ttu-id="1c796-125">Cím</span><span class="sxs-lookup"><span data-stu-id="1c796-125">Title</span></span>                   | <span data-ttu-id="1c796-126">Részletek</span><span class="sxs-lookup"><span data-stu-id="1c796-126">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="1c796-127">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="1c796-127">**Component**</span></span>               | <span data-ttu-id="1c796-128">Azure AD</span><span class="sxs-lookup"><span data-stu-id="1c796-128">Azure AD</span></span> | 
| <span data-ttu-id="1c796-129">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="1c796-129">**SDL Phase**</span></span>               | <span data-ttu-id="1c796-130">Felépítés</span><span class="sxs-lookup"><span data-stu-id="1c796-130">Build</span></span> |  
| <span data-ttu-id="1c796-131">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="1c796-131">**Applicable Technologies**</span></span> | <span data-ttu-id="1c796-132">Általános</span><span class="sxs-lookup"><span data-stu-id="1c796-132">Generic</span></span> |
| <span data-ttu-id="1c796-133">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="1c796-133">**Attributes**</span></span>              | <span data-ttu-id="1c796-134">N/A</span><span class="sxs-lookup"><span data-stu-id="1c796-134">N/A</span></span>  |
| <span data-ttu-id="1c796-135">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="1c796-135">**References**</span></span>              | <span data-ttu-id="1c796-136">N/A</span><span class="sxs-lookup"><span data-stu-id="1c796-136">N/A</span></span>  |
| <span data-ttu-id="1c796-137">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="1c796-137">**Steps**</span></span> | <span data-ttu-id="1c796-138">Ha az alkalmazás az Azure AD által kiállított jogkivonat támaszkodik, meg kell hívnia a kijelentkezési eseménykezelő</span><span class="sxs-lookup"><span data-stu-id="1c796-138">If the application relies on access token issued by Azure AD, the logout event handler should call</span></span> |

### <a name="example"></a><span data-ttu-id="1c796-139">Példa</span><span class="sxs-lookup"><span data-stu-id="1c796-139">Example</span></span>
```C#
HttpContext.GetOwinContext().Authentication.SignOut(OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType)
```

### <a name="example"></a><span data-ttu-id="1c796-140">Példa</span><span class="sxs-lookup"><span data-stu-id="1c796-140">Example</span></span>
<span data-ttu-id="1c796-141">Akkor kell felhasználói munkamenet is megsemmisítése Session.Abandon() metódus meghívásával.</span><span class="sxs-lookup"><span data-stu-id="1c796-141">It should also destroy user's session by calling Session.Abandon() method.</span></span> <span data-ttu-id="1c796-142">A következő metódus jeleníti meg a felhasználó kijelentkezik biztonságos végrehajtásának:</span><span class="sxs-lookup"><span data-stu-id="1c796-142">Following method shows secure implementation of user logout:</span></span>
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

## <span data-ttu-id="1c796-143"><a id="finite-tokens"></a>Generált SaS-tokenje véges élettartamai használata</span><span class="sxs-lookup"><span data-stu-id="1c796-143"><a id="finite-tokens"></a>Use finite lifetimes for generated SaS tokens</span></span>

| <span data-ttu-id="1c796-144">Cím</span><span class="sxs-lookup"><span data-stu-id="1c796-144">Title</span></span>                   | <span data-ttu-id="1c796-145">Részletek</span><span class="sxs-lookup"><span data-stu-id="1c796-145">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="1c796-146">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="1c796-146">**Component**</span></span>               | <span data-ttu-id="1c796-147">IoT-eszközök</span><span class="sxs-lookup"><span data-stu-id="1c796-147">IoT Device</span></span> | 
| <span data-ttu-id="1c796-148">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="1c796-148">**SDL Phase**</span></span>               | <span data-ttu-id="1c796-149">Felépítés</span><span class="sxs-lookup"><span data-stu-id="1c796-149">Build</span></span> |  
| <span data-ttu-id="1c796-150">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="1c796-150">**Applicable Technologies**</span></span> | <span data-ttu-id="1c796-151">Általános</span><span class="sxs-lookup"><span data-stu-id="1c796-151">Generic</span></span> |
| <span data-ttu-id="1c796-152">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="1c796-152">**Attributes**</span></span>              | <span data-ttu-id="1c796-153">N/A</span><span class="sxs-lookup"><span data-stu-id="1c796-153">N/A</span></span>  |
| <span data-ttu-id="1c796-154">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="1c796-154">**References**</span></span>              | <span data-ttu-id="1c796-155">N/A</span><span class="sxs-lookup"><span data-stu-id="1c796-155">N/A</span></span>  |
| <span data-ttu-id="1c796-156">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="1c796-156">**Steps**</span></span> | <span data-ttu-id="1c796-157">SaS-tokenje jön létre az Azure IoT Hub kell véges lejárati idővel rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="1c796-157">SaS tokens generated for authenticating to Azure IoT Hub should have a finite expiry period.</span></span> <span data-ttu-id="1c796-158">A SaS-jogkivonat élettartamát nyomon legalább mennyi ideig visszajátszani abban az esetben, ha a jogkivonatok kerülnek veszélybe korlátozni.</span><span class="sxs-lookup"><span data-stu-id="1c796-158">Keep the SaS token lifetimes to a minimum to limit the amount of time they can be replayed in case the tokens are compromised.</span></span>|

## <span data-ttu-id="1c796-159"><a id="resource-tokens"></a>A létrehozott erőforrás-jogkivonatokat minimális jogkivonat élettartamát használata</span><span class="sxs-lookup"><span data-stu-id="1c796-159"><a id="resource-tokens"></a>Use minimum token lifetimes for generated Resource tokens</span></span>

| <span data-ttu-id="1c796-160">Cím</span><span class="sxs-lookup"><span data-stu-id="1c796-160">Title</span></span>                   | <span data-ttu-id="1c796-161">Részletek</span><span class="sxs-lookup"><span data-stu-id="1c796-161">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="1c796-162">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="1c796-162">**Component**</span></span>               | <span data-ttu-id="1c796-163">Az Azure Document DB rendszerbe</span><span class="sxs-lookup"><span data-stu-id="1c796-163">Azure Document DB</span></span> | 
| <span data-ttu-id="1c796-164">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="1c796-164">**SDL Phase**</span></span>               | <span data-ttu-id="1c796-165">Felépítés</span><span class="sxs-lookup"><span data-stu-id="1c796-165">Build</span></span> |  
| <span data-ttu-id="1c796-166">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="1c796-166">**Applicable Technologies**</span></span> | <span data-ttu-id="1c796-167">Általános</span><span class="sxs-lookup"><span data-stu-id="1c796-167">Generic</span></span> |
| <span data-ttu-id="1c796-168">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="1c796-168">**Attributes**</span></span>              | <span data-ttu-id="1c796-169">N/A</span><span class="sxs-lookup"><span data-stu-id="1c796-169">N/A</span></span>  |
| <span data-ttu-id="1c796-170">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="1c796-170">**References**</span></span>              | <span data-ttu-id="1c796-171">N/A</span><span class="sxs-lookup"><span data-stu-id="1c796-171">N/A</span></span>  |
| <span data-ttu-id="1c796-172">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="1c796-172">**Steps**</span></span> | <span data-ttu-id="1c796-173">Csökkentse az erőforrás-jogkivonat timespan szükséges minimális értékre.</span><span class="sxs-lookup"><span data-stu-id="1c796-173">Reduce the timespan of resource token to a minimum value required.</span></span> <span data-ttu-id="1c796-174">Erőforrás alapértelmezett 1 órás érvényes timespan lehet.</span><span class="sxs-lookup"><span data-stu-id="1c796-174">Resource tokens have a default valid timespan of 1 hour.</span></span>|

## <span data-ttu-id="1c796-175"><a id="wsfederation-logout"></a>Alkalmazzon megfelelő kijelentkezési WsFederation módszerek használatát, ha az AD FS használatával</span><span class="sxs-lookup"><span data-stu-id="1c796-175"><a id="wsfederation-logout"></a>Implement proper logout using WsFederation methods when using ADFS</span></span>

| <span data-ttu-id="1c796-176">Cím</span><span class="sxs-lookup"><span data-stu-id="1c796-176">Title</span></span>                   | <span data-ttu-id="1c796-177">Részletek</span><span class="sxs-lookup"><span data-stu-id="1c796-177">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="1c796-178">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="1c796-178">**Component**</span></span>               | <span data-ttu-id="1c796-179">ADFS</span><span class="sxs-lookup"><span data-stu-id="1c796-179">ADFS</span></span> | 
| <span data-ttu-id="1c796-180">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="1c796-180">**SDL Phase**</span></span>               | <span data-ttu-id="1c796-181">Felépítés</span><span class="sxs-lookup"><span data-stu-id="1c796-181">Build</span></span> |  
| <span data-ttu-id="1c796-182">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="1c796-182">**Applicable Technologies**</span></span> | <span data-ttu-id="1c796-183">Általános</span><span class="sxs-lookup"><span data-stu-id="1c796-183">Generic</span></span> |
| <span data-ttu-id="1c796-184">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="1c796-184">**Attributes**</span></span>              | <span data-ttu-id="1c796-185">N/A</span><span class="sxs-lookup"><span data-stu-id="1c796-185">N/A</span></span>  |
| <span data-ttu-id="1c796-186">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="1c796-186">**References**</span></span>              | <span data-ttu-id="1c796-187">N/A</span><span class="sxs-lookup"><span data-stu-id="1c796-187">N/A</span></span>  |
| <span data-ttu-id="1c796-188">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="1c796-188">**Steps**</span></span> | <span data-ttu-id="1c796-189">Ha az alkalmazás STS-jogkivonatot AD FS által kibocsátott, a kijelentkezési eseménykezelő kell metódushívás WSFederationAuthenticationModule.FederatedSignOut() jelentkezzen ki a felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="1c796-189">If the application relies on STS token issued by ADFS, the logout event handler should call WSFederationAuthenticationModule.FederatedSignOut() method to log out the user.</span></span> <span data-ttu-id="1c796-190">Is az aktuális munkamenet meg kell semmisíteni, és a munkamenet biztonságijogkivonat legyen alaphelyzetbe állítása és hatálytalanítja.</span><span class="sxs-lookup"><span data-stu-id="1c796-190">Also the current session should be destroyed, and the session token value should be reset and nullified.</span></span>|

### <a name="example"></a><span data-ttu-id="1c796-191">Példa</span><span class="sxs-lookup"><span data-stu-id="1c796-191">Example</span></span>
```C#
        [HttpPost, ValidateAntiForgeryToken]
        [Authorization]
        public ActionResult SignOut(string redirectUrl)
        {
            if (!this.User.Identity.IsAuthenticated)
            {
                return this.View("LogOff", null);
            }

            // Removes the user profile.
            this.Session.Clear();
            this.Session.Abandon();
            HttpContext.Current.Response.Cookies.Add(new System.Web.HttpCookie("ASP.NET_SessionId", string.Empty)
                {
                    Expires = DateTime.Now.AddDays(-1D),
                    Secure = true,
                    HttpOnly = true
                });

            // Signs out at the specified security token service (STS) by using the WS-Federation protocol.
            Uri signOutUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Issuer);
            Uri replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm);
            if (!string.IsNullOrEmpty(redirectUrl))
            {
                replyUrl = new Uri(FederatedAuthentication.WSFederationAuthenticationModule.Realm + redirectUrl);
            }
           //     Signs out of the current session and raises the appropriate events.
            var authModule = FederatedAuthentication.WSFederationAuthenticationModule;
            authModule.SignOut(false);
        //     Signs out at the specified security token service (STS) by using the WS-Federation
        //     protocol.            
            WSFederationAuthenticationModule.FederatedSignOut(signOutUrl, replyUrl);
            return new RedirectResult(redirectUrl);
        }
```

## <span data-ttu-id="1c796-192"><a id="proper-logout"></a>Alkalmazzon megfelelő kijelentkezési Identitáskiszolgálók használatakor</span><span class="sxs-lookup"><span data-stu-id="1c796-192"><a id="proper-logout"></a>Implement proper logout when using Identity Server</span></span>

| <span data-ttu-id="1c796-193">Cím</span><span class="sxs-lookup"><span data-stu-id="1c796-193">Title</span></span>                   | <span data-ttu-id="1c796-194">Részletek</span><span class="sxs-lookup"><span data-stu-id="1c796-194">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="1c796-195">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="1c796-195">**Component**</span></span>               | <span data-ttu-id="1c796-196">Identity Serverben</span><span class="sxs-lookup"><span data-stu-id="1c796-196">Identity Server</span></span> | 
| <span data-ttu-id="1c796-197">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="1c796-197">**SDL Phase**</span></span>               | <span data-ttu-id="1c796-198">Felépítés</span><span class="sxs-lookup"><span data-stu-id="1c796-198">Build</span></span> |  
| <span data-ttu-id="1c796-199">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="1c796-199">**Applicable Technologies**</span></span> | <span data-ttu-id="1c796-200">Általános</span><span class="sxs-lookup"><span data-stu-id="1c796-200">Generic</span></span> |
| <span data-ttu-id="1c796-201">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="1c796-201">**Attributes**</span></span>              | <span data-ttu-id="1c796-202">N/A</span><span class="sxs-lookup"><span data-stu-id="1c796-202">N/A</span></span>  |
| <span data-ttu-id="1c796-203">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="1c796-203">**References**</span></span>              | [<span data-ttu-id="1c796-204">IdentityServer3 összevont kijelentkezés</span><span class="sxs-lookup"><span data-stu-id="1c796-204">IdentityServer3-Federated sign out</span></span>](https://identityserver.github.io/Documentation/docsv2/advanced/federated-signout.html) |
| <span data-ttu-id="1c796-205">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="1c796-205">**Steps**</span></span> | <span data-ttu-id="1c796-206">IdentityServer támogatja a külső Identitásszolgáltatók összevonni kívánt.</span><span class="sxs-lookup"><span data-stu-id="1c796-206">IdentityServer supports the ability to federate with external identity providers.</span></span> <span data-ttu-id="1c796-207">Amikor egy felhasználó kijelentkezik a felsőbb rétegbeli identitásszolgáltató, a használt protokolltól függően esetleg is megkapja az értesítéseket, amikor a felhasználó kijelentkezik. Az, akkor a felhasználó is jelentkezzen ki az ügyfelek IdentityServer lehetővé teszi. A megvalósítás részletei hivatkozások részben dokumentációjában.</span><span class="sxs-lookup"><span data-stu-id="1c796-207">When a user signs out of an upstream identity provider, depending upon the protocol used, it might be possible to receive a notification when the user signs out. It allows IdentityServer to notify its clients so they can also sign the user out. Check the documentation in the references section for the implementation details.</span></span>|

## <span data-ttu-id="1c796-208"><a id="https-secure-cookies"></a>HTTPS-KAPCSOLATON keresztül elérhető alkalmazások biztonságos cookie-kat kell használni.</span><span class="sxs-lookup"><span data-stu-id="1c796-208"><a id="https-secure-cookies"></a>Applications available over HTTPS must use secure cookies</span></span>

| <span data-ttu-id="1c796-209">Cím</span><span class="sxs-lookup"><span data-stu-id="1c796-209">Title</span></span>                   | <span data-ttu-id="1c796-210">Részletek</span><span class="sxs-lookup"><span data-stu-id="1c796-210">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="1c796-211">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="1c796-211">**Component**</span></span>               | <span data-ttu-id="1c796-212">Web Application</span><span class="sxs-lookup"><span data-stu-id="1c796-212">Web Application</span></span> | 
| <span data-ttu-id="1c796-213">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="1c796-213">**SDL Phase**</span></span>               | <span data-ttu-id="1c796-214">Felépítés</span><span class="sxs-lookup"><span data-stu-id="1c796-214">Build</span></span> |  
| <span data-ttu-id="1c796-215">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="1c796-215">**Applicable Technologies**</span></span> | <span data-ttu-id="1c796-216">Általános</span><span class="sxs-lookup"><span data-stu-id="1c796-216">Generic</span></span> |
| <span data-ttu-id="1c796-217">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="1c796-217">**Attributes**</span></span>              | <span data-ttu-id="1c796-218">EnvironmentType - a helyi üzemeltetésű</span><span class="sxs-lookup"><span data-stu-id="1c796-218">EnvironmentType - OnPrem</span></span> |
| <span data-ttu-id="1c796-219">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="1c796-219">**References**</span></span>              | <span data-ttu-id="1c796-220">[Elem (ASP.NET beállítási séma) httpCookies](http://msdn.microsoft.com/library/ms228262(v=vs.100).aspx), [HttpCookie.Secure tulajdonság](http://msdn.microsoft.com/library/system.web.httpcookie.secure.aspx)</span><span class="sxs-lookup"><span data-stu-id="1c796-220">[httpCookies Element (ASP.NET Settings Schema)](http://msdn.microsoft.com/library/ms228262(v=vs.100).aspx), [HttpCookie.Secure Property](http://msdn.microsoft.com/library/system.web.httpcookie.secure.aspx)</span></span> |
| <span data-ttu-id="1c796-221">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="1c796-221">**Steps**</span></span> | <span data-ttu-id="1c796-222">A cookie-k általában csak a tartományhoz, amelynek hatóköre volt elérhető.</span><span class="sxs-lookup"><span data-stu-id="1c796-222">Cookies are normally only accessible to the domain for which they were scoped.</span></span> <span data-ttu-id="1c796-223">Definíciója: "tartományi" sajnos nem tartalmazza a protokollt úgy, hogy a cookie-k, amelyek létrejönnek a HTTPS-KAPCSOLATON keresztül elérhető HTTP Protokollon keresztül.</span><span class="sxs-lookup"><span data-stu-id="1c796-223">Unfortunately, the definition of "domain" does not include the protocol so cookies that are created over HTTPS are accessible over HTTP.</span></span> <span data-ttu-id="1c796-224">A "biztonságos" attribútum azt jelöli, a böngészőnek, hogy a cookie-k csak elérhetővé kell tenni HTTPS-KAPCSOLATON keresztül.</span><span class="sxs-lookup"><span data-stu-id="1c796-224">The "secure" attribute indicates to the browser that the cookie should only be made available over HTTPS.</span></span> <span data-ttu-id="1c796-225">Győződjön meg arról, hogy az összes cookie beállítása HTTPS használata a **biztonságos** attribútum.</span><span class="sxs-lookup"><span data-stu-id="1c796-225">Ensure that all cookies set over HTTPS use the **secure** attribute.</span></span> <span data-ttu-id="1c796-226">A követelmény a requireSSL attribútum true értékre állításával kényszerítheti a web.config fájlban.</span><span class="sxs-lookup"><span data-stu-id="1c796-226">The requirement can be enforced in the web.config file by setting the requireSSL attribute to true.</span></span> <span data-ttu-id="1c796-227">Ennek az oka az előnyben részesített módszerrel kényszeríti ki azt a **biztonságos** attribútum jelenlegi és jövőbeli cookie-k nem kell további kód módosításokat.</span><span class="sxs-lookup"><span data-stu-id="1c796-227">It is the preferred approach because it will enforce the **secure** attribute for all current and future cookies without the need to make any additional code changes.</span></span>|

### <a name="example"></a><span data-ttu-id="1c796-228">Példa</span><span class="sxs-lookup"><span data-stu-id="1c796-228">Example</span></span>
```C#
<configuration>
  <system.web>
    <httpCookies requireSSL="true"/>
  </system.web>
</configuration>
```
<span data-ttu-id="1c796-229">A beállítás akkor is, ha az alkalmazás eléréséhez használt HTTP lép életbe.</span><span class="sxs-lookup"><span data-stu-id="1c796-229">The setting is enforced even if HTTP is used to access the application.</span></span> <span data-ttu-id="1c796-230">Az alkalmazáshoz való hozzáférés HTTP használata esetén a beállítás az alkalmazás megszakítja, mert a cookie-kat a biztonságos attribútummal van beállítva, és a böngésző nem visszaküldi azokat az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="1c796-230">If HTTP is used to access the application, the setting breaks the application because the cookies are set with the secure attribute and the browser will not send them back to the application.</span></span>

| <span data-ttu-id="1c796-231">Cím</span><span class="sxs-lookup"><span data-stu-id="1c796-231">Title</span></span>                   | <span data-ttu-id="1c796-232">Részletek</span><span class="sxs-lookup"><span data-stu-id="1c796-232">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="1c796-233">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="1c796-233">**Component**</span></span>               | <span data-ttu-id="1c796-234">Web Application</span><span class="sxs-lookup"><span data-stu-id="1c796-234">Web Application</span></span> | 
| <span data-ttu-id="1c796-235">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="1c796-235">**SDL Phase**</span></span>               | <span data-ttu-id="1c796-236">Felépítés</span><span class="sxs-lookup"><span data-stu-id="1c796-236">Build</span></span> |  
| <span data-ttu-id="1c796-237">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="1c796-237">**Applicable Technologies**</span></span> | <span data-ttu-id="1c796-238">Web Forms keretrendszerre, MVC5</span><span class="sxs-lookup"><span data-stu-id="1c796-238">Web Forms, MVC5</span></span> |
| <span data-ttu-id="1c796-239">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="1c796-239">**Attributes**</span></span>              | <span data-ttu-id="1c796-240">EnvironmentType - a helyi üzemeltetésű</span><span class="sxs-lookup"><span data-stu-id="1c796-240">EnvironmentType - OnPrem</span></span> |
| <span data-ttu-id="1c796-241">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="1c796-241">**References**</span></span>              | <span data-ttu-id="1c796-242">N/A</span><span class="sxs-lookup"><span data-stu-id="1c796-242">N/A</span></span>  |
| <span data-ttu-id="1c796-243">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="1c796-243">**Steps**</span></span> | <span data-ttu-id="1c796-244">A webalkalmazás a függő entitáshoz, és az IdP ADFS-kiszolgáló, a FedAuth jogkivonat biztonságos attribútum konfigurálható úgy, hogy requireSSL TRUE a `system.identityModel.services` szakasz a Web.config fájl:</span><span class="sxs-lookup"><span data-stu-id="1c796-244">When the web application is the Relying Party, and the IdP is ADFS server, the FedAuth token's secure attribute can be configured by setting requireSSL to True in `system.identityModel.services` section of web.config:</span></span>|

### <a name="example"></a><span data-ttu-id="1c796-245">Példa</span><span class="sxs-lookup"><span data-stu-id="1c796-245">Example</span></span>
```C#
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:20:0" />
    ....  
    </federationConfiguration>
  </system.identityModel.services>
```

## <span data-ttu-id="1c796-246"><a id="cookie-definition"></a>Az összes HTTP-alapú alkalmazások csak olyan cookie-k meghatározása http kell megadni</span><span class="sxs-lookup"><span data-stu-id="1c796-246"><a id="cookie-definition"></a>All http based application should specify http only for cookie definition</span></span>

| <span data-ttu-id="1c796-247">Cím</span><span class="sxs-lookup"><span data-stu-id="1c796-247">Title</span></span>                   | <span data-ttu-id="1c796-248">Részletek</span><span class="sxs-lookup"><span data-stu-id="1c796-248">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="1c796-249">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="1c796-249">**Component**</span></span>               | <span data-ttu-id="1c796-250">Web Application</span><span class="sxs-lookup"><span data-stu-id="1c796-250">Web Application</span></span> | 
| <span data-ttu-id="1c796-251">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="1c796-251">**SDL Phase**</span></span>               | <span data-ttu-id="1c796-252">Felépítés</span><span class="sxs-lookup"><span data-stu-id="1c796-252">Build</span></span> |  
| <span data-ttu-id="1c796-253">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="1c796-253">**Applicable Technologies**</span></span> | <span data-ttu-id="1c796-254">Általános</span><span class="sxs-lookup"><span data-stu-id="1c796-254">Generic</span></span> |
| <span data-ttu-id="1c796-255">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="1c796-255">**Attributes**</span></span>              | <span data-ttu-id="1c796-256">N/A</span><span class="sxs-lookup"><span data-stu-id="1c796-256">N/A</span></span>  |
| <span data-ttu-id="1c796-257">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="1c796-257">**References**</span></span>              | [<span data-ttu-id="1c796-258">Biztonságos Cookie-attribútum</span><span class="sxs-lookup"><span data-stu-id="1c796-258">Secure Cookie Attribute</span></span>](https://en.wikipedia.org/wiki/HTTP_cookie#Secure_cookie) |
| <span data-ttu-id="1c796-259">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="1c796-259">**Steps**</span></span> | <span data-ttu-id="1c796-260">Az információfelfedés kockázatának csökkentéséhez a többhelyes scripting (lehetővé) támadás a mérséklése érdekében új attribútum - httpOnly - cookie-k jelent, és az összes ismertebb böngésző támogatja.</span><span class="sxs-lookup"><span data-stu-id="1c796-260">To mitigate the risk of information disclosure with a cross-site scripting (XSS) attack, a new attribute - httpOnly - was introduced to cookies and is supported by all major browsers.</span></span> <span data-ttu-id="1c796-261">Az attribútum Megadja, hogy a cookie-k nem érhető el a parancsfájlon keresztül.</span><span class="sxs-lookup"><span data-stu-id="1c796-261">The attribute specifies that a cookie is not accessible through script.</span></span> <span data-ttu-id="1c796-262">Webalkalmazás HttpOnly cookie-k használata, csökkenti a lehetősége, hogy a cookie-ban tárolt bizalmas információ ellopják keresztül parancsfájl-e, és hogy egy támadó webhelynek küldött.</span><span class="sxs-lookup"><span data-stu-id="1c796-262">By using HttpOnly cookies, a web application reduces the possibility that sensitive information contained in the cookie can be stolen via script and sent to an attacker's website.</span></span> |

### <a name="example"></a><span data-ttu-id="1c796-263">Példa</span><span class="sxs-lookup"><span data-stu-id="1c796-263">Example</span></span>
<span data-ttu-id="1c796-264">Cookie-k használó összes HTTP-alapú alkalmazások kell HttpOnly a cookie-definíció adható meg, alkalmazásával segítse a web.config konfigurációs követően:</span><span class="sxs-lookup"><span data-stu-id="1c796-264">All HTTP-based applications that use cookies should specify HttpOnly in the cookie definition, by implementing following configuration in web.config:</span></span>
```XML
<system.web>
.
.
   <httpCookies requireSSL="false" httpOnlyCookies="true"/>
.
.
</system.web>
```

| <span data-ttu-id="1c796-265">Cím</span><span class="sxs-lookup"><span data-stu-id="1c796-265">Title</span></span>                   | <span data-ttu-id="1c796-266">Részletek</span><span class="sxs-lookup"><span data-stu-id="1c796-266">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="1c796-267">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="1c796-267">**Component**</span></span>               | <span data-ttu-id="1c796-268">Web Application</span><span class="sxs-lookup"><span data-stu-id="1c796-268">Web Application</span></span> | 
| <span data-ttu-id="1c796-269">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="1c796-269">**SDL Phase**</span></span>               | <span data-ttu-id="1c796-270">Felépítés</span><span class="sxs-lookup"><span data-stu-id="1c796-270">Build</span></span> |  
| <span data-ttu-id="1c796-271">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="1c796-271">**Applicable Technologies**</span></span> | <span data-ttu-id="1c796-272">Web Forms keretrendszerre</span><span class="sxs-lookup"><span data-stu-id="1c796-272">Web Forms</span></span> |
| <span data-ttu-id="1c796-273">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="1c796-273">**Attributes**</span></span>              | <span data-ttu-id="1c796-274">N/A</span><span class="sxs-lookup"><span data-stu-id="1c796-274">N/A</span></span>  |
| <span data-ttu-id="1c796-275">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="1c796-275">**References**</span></span>              | [<span data-ttu-id="1c796-276">FormsAuthentication.RequireSSL tulajdonság</span><span class="sxs-lookup"><span data-stu-id="1c796-276">FormsAuthentication.RequireSSL Property</span></span>](https://msdn.microsoft.com/library/system.web.security.formsauthentication.requiressl.aspx) |
| <span data-ttu-id="1c796-277">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="1c796-277">**Steps**</span></span> | <span data-ttu-id="1c796-278">A RequireSSL tulajdonság értéke a konfigurációs fájlban, az ASP.NET-alkalmazás a konfigurációs elem requireSSL attribútumával.</span><span class="sxs-lookup"><span data-stu-id="1c796-278">The RequireSSL property value is set in the configuration file for an ASP.NET application by using the requireSSL attribute of the configuration element.</span></span> <span data-ttu-id="1c796-279">Megadhatja a Web.config fájlban az ASP.NET alkalmazás hogy SSL (Secure Sockets Layer) szükséges az űrlap-hitelesítési cookie-k visszatérhet a kiszolgáló úgy, hogy a requireSSL attribútum.</span><span class="sxs-lookup"><span data-stu-id="1c796-279">You can specify in the Web.config file for your ASP.NET application whether SSL (Secure Sockets Layer) is required to return the forms-authentication cookie to the server by setting the requireSSL attribute.</span></span>|

### <a name="example"></a><span data-ttu-id="1c796-280">Példa</span><span class="sxs-lookup"><span data-stu-id="1c796-280">Example</span></span> 
<span data-ttu-id="1c796-281">Az alábbi példakód beállítja a requireSSL attribútumot a Web.config fájlban.</span><span class="sxs-lookup"><span data-stu-id="1c796-281">The following code example sets the requireSSL attribute in the Web.config file.</span></span>
```XML
<authentication mode="Forms">
  <forms loginUrl="member_login.aspx" cookieless="UseCookies" requireSSL="true"/>
</authentication>
```

| <span data-ttu-id="1c796-282">Cím</span><span class="sxs-lookup"><span data-stu-id="1c796-282">Title</span></span>                   | <span data-ttu-id="1c796-283">Részletek</span><span class="sxs-lookup"><span data-stu-id="1c796-283">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="1c796-284">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="1c796-284">**Component**</span></span>               | <span data-ttu-id="1c796-285">Web Application</span><span class="sxs-lookup"><span data-stu-id="1c796-285">Web Application</span></span> | 
| <span data-ttu-id="1c796-286">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="1c796-286">**SDL Phase**</span></span>               | <span data-ttu-id="1c796-287">Felépítés</span><span class="sxs-lookup"><span data-stu-id="1c796-287">Build</span></span> |  
| <span data-ttu-id="1c796-288">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="1c796-288">**Applicable Technologies**</span></span> | <span data-ttu-id="1c796-289">MVC5</span><span class="sxs-lookup"><span data-stu-id="1c796-289">MVC5</span></span> |
| <span data-ttu-id="1c796-290">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="1c796-290">**Attributes**</span></span>              | <span data-ttu-id="1c796-291">EnvironmentType - a helyi üzemeltetésű</span><span class="sxs-lookup"><span data-stu-id="1c796-291">EnvironmentType - OnPrem</span></span> |
| <span data-ttu-id="1c796-292">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="1c796-292">**References**</span></span>              | [<span data-ttu-id="1c796-293">A Windows Identity Foundation (WIF) konfigurációja – II. rész</span><span class="sxs-lookup"><span data-stu-id="1c796-293">Windows Identity Foundation (WIF) Configuration – Part II</span></span>](https://blogs.msdn.microsoft.com/alikl/2011/02/01/windows-identity-foundation-wif-configuration-part-ii-cookiehandler-chunkedcookiehandler-customcookiehandler/) |
| <span data-ttu-id="1c796-294">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="1c796-294">**Steps**</span></span> | <span data-ttu-id="1c796-295">A FedAuth cookie-k httpOnly attribútum beállítása, hideFromCsript attribútum értéke TRUE értéket kell megadni.</span><span class="sxs-lookup"><span data-stu-id="1c796-295">To set httpOnly attribute for FedAuth cookies, hideFromCsript attribute value should be set to True.</span></span> |

### <a name="example"></a><span data-ttu-id="1c796-296">Példa</span><span class="sxs-lookup"><span data-stu-id="1c796-296">Example</span></span>
<span data-ttu-id="1c796-297">Következő konfigurációt a helyes konfiguráció látható:</span><span class="sxs-lookup"><span data-stu-id="1c796-297">Following configuration shows the correct configuration:</span></span>
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

## <span data-ttu-id="1c796-298"><a id="csrf-asp"></a>ASP.NET-weblapok többhelyes kérelem hamisítására (CSRF) támadások elleni</span><span class="sxs-lookup"><span data-stu-id="1c796-298"><a id="csrf-asp"></a>Mitigate against Cross-Site Request Forgery (CSRF) attacks on ASP.NET web pages</span></span>

| <span data-ttu-id="1c796-299">Cím</span><span class="sxs-lookup"><span data-stu-id="1c796-299">Title</span></span>                   | <span data-ttu-id="1c796-300">Részletek</span><span class="sxs-lookup"><span data-stu-id="1c796-300">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="1c796-301">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="1c796-301">**Component**</span></span>               | <span data-ttu-id="1c796-302">Web Application</span><span class="sxs-lookup"><span data-stu-id="1c796-302">Web Application</span></span> | 
| <span data-ttu-id="1c796-303">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="1c796-303">**SDL Phase**</span></span>               | <span data-ttu-id="1c796-304">Felépítés</span><span class="sxs-lookup"><span data-stu-id="1c796-304">Build</span></span> |  
| <span data-ttu-id="1c796-305">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="1c796-305">**Applicable Technologies**</span></span> | <span data-ttu-id="1c796-306">Általános</span><span class="sxs-lookup"><span data-stu-id="1c796-306">Generic</span></span> |
| <span data-ttu-id="1c796-307">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="1c796-307">**Attributes**</span></span>              | <span data-ttu-id="1c796-308">N/A</span><span class="sxs-lookup"><span data-stu-id="1c796-308">N/A</span></span>  |
| <span data-ttu-id="1c796-309">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="1c796-309">**References**</span></span>              | <span data-ttu-id="1c796-310">N/A</span><span class="sxs-lookup"><span data-stu-id="1c796-310">N/A</span></span>  |
| <span data-ttu-id="1c796-311">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="1c796-311">**Steps**</span></span> | <span data-ttu-id="1c796-312">Webhelyközi kérések hamisítására (CSRF vagy XSRF), amelyben a támadó műveleteket végezhet el a biztonsági környezetében webhelyen egy másik felhasználói munkamenetet a támadás típusú.</span><span class="sxs-lookup"><span data-stu-id="1c796-312">Cross-site request forgery (CSRF or XSRF) is a type of attack in which an attacker can carry out actions in the security context of a different user's established session on a web site.</span></span> <span data-ttu-id="1c796-313">A célja módosítsa vagy törölje a tartalmat, ha a célként kijelölt webhely kizárólag a munkamenet-cookie-k hitelesítésére kérelem érkezett.</span><span class="sxs-lookup"><span data-stu-id="1c796-313">The goal is to modify or delete content, if the targeted web site relies exclusively on session cookies to authenticate received request.</span></span> <span data-ttu-id="1c796-314">A támadó a biztonsági rés olvasson be egy másik felhasználó böngésző betölteni egy parancs egy URL-címet egy sebezhető helyről, amikor a felhasználó már jelentkezett be.</span><span class="sxs-lookup"><span data-stu-id="1c796-314">An attacker could exploit this vulnerability by getting a different user's browser to load a URL with a command from a vulnerable site on which the user is already logged in.</span></span> <span data-ttu-id="1c796-315">Számos módon egy támadó úgy teheti meg, amely, például egy erőforrás betölti a sebezhető kiszolgálóról egy másik webhely üzemeltetéséhez, vagy a felhasználó első hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="1c796-315">There are many ways for an attacker to do that, such as by hosting a different web site that loads a resource from the vulnerable server, or getting the user to click a link.</span></span> <span data-ttu-id="1c796-316">Ha a kiszolgáló elküldi az ügyfélnek a további tokent, az ügyfél számára, hogy a jogkivonat szerepeljen minden jövőbeni kérelemhez szükséges, és ellenőrzi, hogy minden későbbi kérelmek tartalmaz egy jogkivonatot, amely vonatkozik az aktuális munkamenetről, például az ASP.NET használatával megelőzhető a támadás AntiForgeryToken vagy a megjelenítési állapot.</span><span class="sxs-lookup"><span data-stu-id="1c796-316">The attack can be prevented if the server sends an additional token to the client, requires the client to include that token in all future requests, and verifies that all future requests include a token that pertains to the current session, such as by using the ASP.NET AntiForgeryToken or ViewState.</span></span> |

| <span data-ttu-id="1c796-317">Cím</span><span class="sxs-lookup"><span data-stu-id="1c796-317">Title</span></span>                   | <span data-ttu-id="1c796-318">Részletek</span><span class="sxs-lookup"><span data-stu-id="1c796-318">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="1c796-319">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="1c796-319">**Component**</span></span>               | <span data-ttu-id="1c796-320">Web Application</span><span class="sxs-lookup"><span data-stu-id="1c796-320">Web Application</span></span> | 
| <span data-ttu-id="1c796-321">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="1c796-321">**SDL Phase**</span></span>               | <span data-ttu-id="1c796-322">Felépítés</span><span class="sxs-lookup"><span data-stu-id="1c796-322">Build</span></span> |  
| <span data-ttu-id="1c796-323">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="1c796-323">**Applicable Technologies**</span></span> | <span data-ttu-id="1c796-324">MVC5, MVC6</span><span class="sxs-lookup"><span data-stu-id="1c796-324">MVC5, MVC6</span></span> |
| <span data-ttu-id="1c796-325">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="1c796-325">**Attributes**</span></span>              | <span data-ttu-id="1c796-326">N/A</span><span class="sxs-lookup"><span data-stu-id="1c796-326">N/A</span></span>  |
| <span data-ttu-id="1c796-327">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="1c796-327">**References**</span></span>              | [<span data-ttu-id="1c796-328">ASP.NET MVC és weblapok XSRF/CSRF megelőzése</span><span class="sxs-lookup"><span data-stu-id="1c796-328">XSRF/CSRF Prevention in ASP.NET MVC and Web Pages</span></span>](http://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) |
| <span data-ttu-id="1c796-329">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="1c796-329">**Steps**</span></span> | <span data-ttu-id="1c796-330">Kártevőirtó-CSRF és az ASP.NET MVC űrlapok - a `AntiForgeryToken` segédmetódust a nézetek; put egy `Html.AntiForgeryToken()` az űrlapon, például</span><span class="sxs-lookup"><span data-stu-id="1c796-330">Anti-CSRF and ASP.NET MVC forms - Use the `AntiForgeryToken` helper method on Views; put an `Html.AntiForgeryToken()` into the form, for example,</span></span>|

### <a name="example"></a><span data-ttu-id="1c796-331">Példa</span><span class="sxs-lookup"><span data-stu-id="1c796-331">Example</span></span>
```C#
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
```

### <a name="example"></a><span data-ttu-id="1c796-332">Példa</span><span class="sxs-lookup"><span data-stu-id="1c796-332">Example</span></span>
```C#
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a><span data-ttu-id="1c796-333">Példa</span><span class="sxs-lookup"><span data-stu-id="1c796-333">Example</span></span>
<span data-ttu-id="1c796-334">Egy időben Html.AntiForgeryToken() __RequestVerificationToken, a fenti véletlenszerű rejtett érték azonos értékű nevű cookie lehetőséget ad a látogató adott területre.</span><span class="sxs-lookup"><span data-stu-id="1c796-334">At the same time, Html.AntiForgeryToken() gives the visitor a cookie called __RequestVerificationToken, with the same value as the random hidden value shown above.</span></span> <span data-ttu-id="1c796-335">Ezután egy bejövő közzétett űrlapból ellenőrzéséhez adja hozzá a [ValidateAntiForgeryToken] szűrő a célmetódushoz való kötéskor művelet.</span><span class="sxs-lookup"><span data-stu-id="1c796-335">Next, to validate an incoming form post, add the [ValidateAntiForgeryToken] filter to the target action method.</span></span> <span data-ttu-id="1c796-336">Példa:</span><span class="sxs-lookup"><span data-stu-id="1c796-336">For example:</span></span>
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
<span data-ttu-id="1c796-337">Ellenőrzi, hogy a szűrő engedélyezési:</span><span class="sxs-lookup"><span data-stu-id="1c796-337">Authorization filter that checks that:</span></span>
* <span data-ttu-id="1c796-338">A bejövő kérelem rendelkezik egy cookie-k hívása __RequestVerificationToken</span><span class="sxs-lookup"><span data-stu-id="1c796-338">The incoming request has a cookie called __RequestVerificationToken</span></span>
* <span data-ttu-id="1c796-339">A bejövő kérelem egy `Request.Form` __RequestVerificationToken nevezett bejegyzés</span><span class="sxs-lookup"><span data-stu-id="1c796-339">The incoming request has a `Request.Form` entry called __RequestVerificationToken</span></span>
* <span data-ttu-id="1c796-340">A cookie-k és `Request.Form` jól értékek egyeznek, feltéve, hogy minden, a kérelem végighalad normál.</span><span class="sxs-lookup"><span data-stu-id="1c796-340">These cookie and `Request.Form` values match Assuming all is well, the request goes through as normal.</span></span> <span data-ttu-id="1c796-341">De ha nem, majd üzenettel engedélyezési hiba "szükséges hamisításgátló jogkivonat nincs megadva vagy érvénytelen".</span><span class="sxs-lookup"><span data-stu-id="1c796-341">But if not, then an authorization failure with message “A required anti-forgery token was not supplied or was invalid”.</span></span> 

### <a name="example"></a><span data-ttu-id="1c796-342">Példa</span><span class="sxs-lookup"><span data-stu-id="1c796-342">Example</span></span>
<span data-ttu-id="1c796-343">Kártevőirtó-CSRF és AJAX: az űrlap token az AJAX-kérelmek problémát okozhat, mert AJAX-kérelmet el tudja küldeni a JSON-adatokat, nem a HTML-űrlapot adatok.</span><span class="sxs-lookup"><span data-stu-id="1c796-343">Anti-CSRF and AJAX: The form token can be a problem for AJAX requests, because an AJAX request might send JSON data, not HTML form data.</span></span> <span data-ttu-id="1c796-344">Egy megoldás, a jogkivonatok küldeni egy egyéni HTTP-fejlécben.</span><span class="sxs-lookup"><span data-stu-id="1c796-344">One solution is to send the tokens in a custom HTTP header.</span></span> <span data-ttu-id="1c796-345">Az alábbi kód Razor szintaxist használja a jogkivonatok létrehozásához, és hozzáadja a tokenek egy AJAX-kérelemre.</span><span class="sxs-lookup"><span data-stu-id="1c796-345">The following code uses Razor syntax to generate the tokens, and then adds the tokens to an AJAX request.</span></span> 
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

### <a name="example"></a><span data-ttu-id="1c796-346">Példa</span><span class="sxs-lookup"><span data-stu-id="1c796-346">Example</span></span>
<span data-ttu-id="1c796-347">A kérelem feldolgozása során a jogkivonatok kinyerése a kérelem fejlécében.</span><span class="sxs-lookup"><span data-stu-id="1c796-347">When you process the request, extract the tokens from the request header.</span></span> <span data-ttu-id="1c796-348">Majd hívja a következő érvényesítse az AntiForgery.Validate metódust.</span><span class="sxs-lookup"><span data-stu-id="1c796-348">Then call the AntiForgery.Validate method to validate the tokens.</span></span> <span data-ttu-id="1c796-349">A Validate metódus kivételt jelez, ha a jogkivonatok nem érvényes.</span><span class="sxs-lookup"><span data-stu-id="1c796-349">The Validate method throws an exception if the tokens are not valid.</span></span>
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

| <span data-ttu-id="1c796-350">Cím</span><span class="sxs-lookup"><span data-stu-id="1c796-350">Title</span></span>                   | <span data-ttu-id="1c796-351">Részletek</span><span class="sxs-lookup"><span data-stu-id="1c796-351">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="1c796-352">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="1c796-352">**Component**</span></span>               | <span data-ttu-id="1c796-353">Web Application</span><span class="sxs-lookup"><span data-stu-id="1c796-353">Web Application</span></span> | 
| <span data-ttu-id="1c796-354">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="1c796-354">**SDL Phase**</span></span>               | <span data-ttu-id="1c796-355">Felépítés</span><span class="sxs-lookup"><span data-stu-id="1c796-355">Build</span></span> |  
| <span data-ttu-id="1c796-356">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="1c796-356">**Applicable Technologies**</span></span> | <span data-ttu-id="1c796-357">Web Forms keretrendszerre</span><span class="sxs-lookup"><span data-stu-id="1c796-357">Web Forms</span></span> |
| <span data-ttu-id="1c796-358">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="1c796-358">**Attributes**</span></span>              | <span data-ttu-id="1c796-359">N/A</span><span class="sxs-lookup"><span data-stu-id="1c796-359">N/A</span></span>  |
| <span data-ttu-id="1c796-360">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="1c796-360">**References**</span></span>              | [<span data-ttu-id="1c796-361">Az ASP.NET beépített a támadások elleni védekezésben kivédése előnyeit</span><span class="sxs-lookup"><span data-stu-id="1c796-361">Take Advantage of ASP.NET Built-in Features to Fend Off Web Attacks</span></span>](https://msdn.microsoft.com/library/ms972969.aspx#securitybarriers_topic2) |
| <span data-ttu-id="1c796-362">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="1c796-362">**Steps**</span></span> | <span data-ttu-id="1c796-363">A webes alapú alkalmazásokban CSRF támadások mérsékelhető a ViewStateUserKey értékre állításával véletlenszerű karakterlánc, amely változtatja az egyes felhasználók - felhasználói Azonosítóját, vagy jobb még, munkamenet-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="1c796-363">CSRF attacks in WebForm based applications can be mitigated by setting ViewStateUserKey to a random string that varies for each user - user ID or, better yet, session ID.</span></span> <span data-ttu-id="1c796-364">Műszaki és közösségi okokból számos, a munkamenet-azonosító egy javulás méretezése azért, mert egy munkamenet-azonosító előre nem látható, túllépi az időkorlátot, és a felhasználónkénti alapon változik.</span><span class="sxs-lookup"><span data-stu-id="1c796-364">For a number of technical and social reasons, session ID is a much better fit because a session ID is unpredictable, times out, and varies on a per-user basis.</span></span>|

### <a name="example"></a><span data-ttu-id="1c796-365">Példa</span><span class="sxs-lookup"><span data-stu-id="1c796-365">Example</span></span>
<span data-ttu-id="1c796-366">Az összes weblapot van szükség a kód itt látható:</span><span class="sxs-lookup"><span data-stu-id="1c796-366">Here's the code you need to have in all of your pages:</span></span>
```C#
void Page_Init (object sender, EventArgs e) {
   ViewStateUserKey = Session.SessionID;
   :
}
```

## <span data-ttu-id="1c796-367"><a id="inactivity-lifetime"></a>Állítsa be a munkamenet inaktivitás élettartama</span><span class="sxs-lookup"><span data-stu-id="1c796-367"><a id="inactivity-lifetime"></a>Set up session for inactivity lifetime</span></span>

| <span data-ttu-id="1c796-368">Cím</span><span class="sxs-lookup"><span data-stu-id="1c796-368">Title</span></span>                   | <span data-ttu-id="1c796-369">Részletek</span><span class="sxs-lookup"><span data-stu-id="1c796-369">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="1c796-370">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="1c796-370">**Component**</span></span>               | <span data-ttu-id="1c796-371">Web Application</span><span class="sxs-lookup"><span data-stu-id="1c796-371">Web Application</span></span> | 
| <span data-ttu-id="1c796-372">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="1c796-372">**SDL Phase**</span></span>               | <span data-ttu-id="1c796-373">Felépítés</span><span class="sxs-lookup"><span data-stu-id="1c796-373">Build</span></span> |  
| <span data-ttu-id="1c796-374">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="1c796-374">**Applicable Technologies**</span></span> | <span data-ttu-id="1c796-375">Általános</span><span class="sxs-lookup"><span data-stu-id="1c796-375">Generic</span></span> |
| <span data-ttu-id="1c796-376">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="1c796-376">**Attributes**</span></span>              | <span data-ttu-id="1c796-377">N/A</span><span class="sxs-lookup"><span data-stu-id="1c796-377">N/A</span></span>  |
| <span data-ttu-id="1c796-378">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="1c796-378">**References**</span></span>              | <span data-ttu-id="1c796-379">[HttpSessionState.Timeout tulajdonság](https://msdn.microsoft.com/library/system.web.sessionstate.httpsessionstate.timeout(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="1c796-379">[HttpSessionState.Timeout Property](https://msdn.microsoft.com/library/system.web.sessionstate.httpsessionstate.timeout(v=vs.110).aspx)</span></span> |
| <span data-ttu-id="1c796-380">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="1c796-380">**Steps**</span></span> | <span data-ttu-id="1c796-381">Időtúllépés az esemény lépett fel, amikor a felhasználó nem bármely művelet elvégzésére webhelyen a időközben (a webkiszolgáló által megadott) jelöli.</span><span class="sxs-lookup"><span data-stu-id="1c796-381">Session timeout represents the event occurring when a user does not perform any action on a web site during a interval (defined by web server).</span></span> <span data-ttu-id="1c796-382">Az esemény, a kiszolgáló oldalán, módosítsa a felhasználói munkamenet állapota "érvénytelen" (például "nem használható többé"), és kérje meg a webkiszolgálón. szüntesse meg az (bele tárolt összes adat törlése).</span><span class="sxs-lookup"><span data-stu-id="1c796-382">The event, on server side, change the status of the user session to 'invalid' (for example  "not used anymore") and instruct the web server to destroy it (deleting all data contained into it).</span></span> <span data-ttu-id="1c796-383">Az alábbi példakód állítja be a munkamenet időtúllépés attribútumot 15 perc a Web.config fájlban.</span><span class="sxs-lookup"><span data-stu-id="1c796-383">The following code example sets the timeout session attribute to 15 minutes in the Web.config file.</span></span>|

### <a name="example"></a><span data-ttu-id="1c796-384">Példa</span><span class="sxs-lookup"><span data-stu-id="1c796-384">Example</span></span>
<span data-ttu-id="1c796-385">A(z) "XML-kódot <configuration> < system.web > <sessionState mode="InProc" cookieless="true" timeout="15" /> < /system.web ></configuration></span><span class="sxs-lookup"><span data-stu-id="1c796-385">``\`XML code <configuration> <system.web> <sessionState mode="InProc" cookieless="true" timeout="15" /> </system.web> </configuration></span></span>
```

## <a id="threat-detection"></a>Enable Threat detection on Azure SQL
```

| <span data-ttu-id="1c796-386">Cím</span><span class="sxs-lookup"><span data-stu-id="1c796-386">Title</span></span>                   | <span data-ttu-id="1c796-387">Részletek</span><span class="sxs-lookup"><span data-stu-id="1c796-387">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="1c796-388">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="1c796-388">**Component**</span></span>               | <span data-ttu-id="1c796-389">Web Application</span><span class="sxs-lookup"><span data-stu-id="1c796-389">Web Application</span></span> | 
| <span data-ttu-id="1c796-390">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="1c796-390">**SDL Phase**</span></span>               | <span data-ttu-id="1c796-391">Felépítés</span><span class="sxs-lookup"><span data-stu-id="1c796-391">Build</span></span> |  
| <span data-ttu-id="1c796-392">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="1c796-392">**Applicable Technologies**</span></span> | <span data-ttu-id="1c796-393">Web Forms keretrendszerre</span><span class="sxs-lookup"><span data-stu-id="1c796-393">Web Forms</span></span> |
| <span data-ttu-id="1c796-394">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="1c796-394">**Attributes**</span></span>              | <span data-ttu-id="1c796-395">N/A</span><span class="sxs-lookup"><span data-stu-id="1c796-395">N/A</span></span>  |
| <span data-ttu-id="1c796-396">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="1c796-396">**References**</span></span>              | <span data-ttu-id="1c796-397">[Elem űrlap-hitelesítés (ASP.NET beállítási séma)](https://msdn.microsoft.com/library/1d3t3c61(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="1c796-397">[forms Element for authentication (ASP.NET Settings Schema)](https://msdn.microsoft.com/library/1d3t3c61(v=vs.100).aspx)</span></span> |
| <span data-ttu-id="1c796-398">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="1c796-398">**Steps**</span></span> | <span data-ttu-id="1c796-399">Az űrlapos hitelesítési jegyet cookie-k időkorlátja 15 percre beállítva</span><span class="sxs-lookup"><span data-stu-id="1c796-399">Set the Forms Authentication Ticket cookie timeout to 15 minutes</span></span>|

### <a name="example"></a><span data-ttu-id="1c796-400">Példa</span><span class="sxs-lookup"><span data-stu-id="1c796-400">Example</span></span>
<span data-ttu-id="1c796-401">A(z) "XML-kódot<forms  name=".ASPXAUTH" loginUrl="login.aspx"  defaultUrl="default.aspx" protection="All" timeout="15" path="/" requireSSL="true" slidingExpiration="true"/>
</forms></span><span class="sxs-lookup"><span data-stu-id="1c796-401">``\`XML code <forms  name=".ASPXAUTH" loginUrl="login.aspx"  defaultUrl="default.aspx" protection="All" timeout="15" path="/" requireSSL="true" slidingExpiration="true"/>
</forms></span></span>
```

| Title                   | Details      |
| ----------------------- | ------------ |
| **Component**               | Web Application | 
| **SDL Phase**               | Build |  
| **Applicable Technologies** | Web Forms, MVC5 |
| **Attributes**              | EnvironmentType - OnPrem |
| **References**              | [asdeqa](https://skf.azurewebsites.net/Mitigations/Details/wefr) |
| **Steps** | When the web application is Relying Party and ADFS is the STS, the lifetime of the authentication cookies - FedAuth tokens - can be set by the following configuration in web.config:|

### Example
```XML
  <system.identityModel.services>
    <federationConfiguration>
      <!-- Set requireSsl=true; domain=application domain name used by FedAuth cookies (Ex: .gdinfra.com); -->
      <cookieHandler requireSsl="true" persistentSessionLifetime="0.0:15:0" />
      <!-- Set requireHttps=true; -->
      <wsFederation passiveRedirectEnabled="true" issuer="http://localhost:39529/" realm="https://localhost:44302/" reply="https://localhost:44302/" requireHttps="true"/>
      <!--
      Use the code below to enable encryption-decryption of claims received from ADFS. Thumbprint value varies based on the certificate being used.
      <serviceCertificate>
        <certificateReference findValue="4FBBBA33A1D11A9022A5BF3492FF83320007686A" storeLocation="LocalMachine" storeName="My" x509FindType="FindByThumbprint" />
      </serviceCertificate>
      -->
    </federationConfiguration>
  </system.identityModel.services>
```

### <a name="example"></a><span data-ttu-id="1c796-402">Példa</span><span class="sxs-lookup"><span data-stu-id="1c796-402">Example</span></span>
<span data-ttu-id="1c796-403">Az AD FS SAML kiadott is jogcím-jogkivonat élettartamát meg 15 perc, a következő az ADFS-kiszolgálón a következő powershell-parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1c796-403">Also the ADFS issued SAML claim token's lifetime should be set to 15 minutes, by executing the following powershell command on the ADFS server:</span></span>
```C#
Set-ADFSRelyingPartyTrust -TargetName “<RelyingPartyWebApp>” -ClaimsProviderName @(“Active Directory”) -TokenLifetime 15 -AlwaysRequireAuthentication $true
```

## <span data-ttu-id="1c796-404"><a id="proper-app-logout"></a>Az alkalmazás megfelelő kijelentkezési megvalósítása</span><span class="sxs-lookup"><span data-stu-id="1c796-404"><a id="proper-app-logout"></a>Implement proper logout from the application</span></span>

| <span data-ttu-id="1c796-405">Cím</span><span class="sxs-lookup"><span data-stu-id="1c796-405">Title</span></span>                   | <span data-ttu-id="1c796-406">Részletek</span><span class="sxs-lookup"><span data-stu-id="1c796-406">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="1c796-407">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="1c796-407">**Component**</span></span>               | <span data-ttu-id="1c796-408">Web Application</span><span class="sxs-lookup"><span data-stu-id="1c796-408">Web Application</span></span> | 
| <span data-ttu-id="1c796-409">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="1c796-409">**SDL Phase**</span></span>               | <span data-ttu-id="1c796-410">Felépítés</span><span class="sxs-lookup"><span data-stu-id="1c796-410">Build</span></span> |  
| <span data-ttu-id="1c796-411">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="1c796-411">**Applicable Technologies**</span></span> | <span data-ttu-id="1c796-412">Általános</span><span class="sxs-lookup"><span data-stu-id="1c796-412">Generic</span></span> |
| <span data-ttu-id="1c796-413">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="1c796-413">**Attributes**</span></span>              | <span data-ttu-id="1c796-414">N/A</span><span class="sxs-lookup"><span data-stu-id="1c796-414">N/A</span></span>  |
| <span data-ttu-id="1c796-415">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="1c796-415">**References**</span></span>              | <span data-ttu-id="1c796-416">N/A</span><span class="sxs-lookup"><span data-stu-id="1c796-416">N/A</span></span>  |
| <span data-ttu-id="1c796-417">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="1c796-417">**Steps**</span></span> | <span data-ttu-id="1c796-418">Hajtsa végre megfelelő Kijelentkezés az alkalmazásból, amikor a felhasználó présgépet Kijelentkezés gombra.</span><span class="sxs-lookup"><span data-stu-id="1c796-418">Perform proper Sign Out from the application, when user presses log out button.</span></span> <span data-ttu-id="1c796-419">Követően jelentkezzen ki alkalmazás kell semmisítse meg a felhasználó munkamenetét, és is alaphelyzetbe állítása és érvényteleníti a munkamenet cookie-értéket alaphelyzetbe állítása és a hitelesítési cookie-értéket érvénytelenítését együtt.</span><span class="sxs-lookup"><span data-stu-id="1c796-419">Upon logout, application should destroy user's session, and also reset and nullify session cookie value, along with resetting and nullifying authentication cookie value.</span></span> <span data-ttu-id="1c796-420">Is ha több munkamenetet egy felhasználói azonosítót vannak társítva, azok kell együttesen lezárni időkorlát, vagy jelentkezzen ki, a kiszolgáló oldalán.</span><span class="sxs-lookup"><span data-stu-id="1c796-420">Also, when multiple sessions are tied to a single user identity, they must be collectively terminated on the server side at timeout or logout.</span></span> <span data-ttu-id="1c796-421">Végül győződjön meg arról, hogy kijelentkezési funkció minden oldalon érhető el.</span><span class="sxs-lookup"><span data-stu-id="1c796-421">Lastly, ensure that Logout functionality is available on every page.</span></span> |

## <span data-ttu-id="1c796-422"><a id="csrf-api"></a>ASP.NET webes API-k többhelyes kérelem hamisítására (CSRF) támadások elleni</span><span class="sxs-lookup"><span data-stu-id="1c796-422"><a id="csrf-api"></a>Mitigate against Cross-Site Request Forgery (CSRF) attacks on ASP.NET Web APIs</span></span>

| <span data-ttu-id="1c796-423">Cím</span><span class="sxs-lookup"><span data-stu-id="1c796-423">Title</span></span>                   | <span data-ttu-id="1c796-424">Részletek</span><span class="sxs-lookup"><span data-stu-id="1c796-424">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="1c796-425">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="1c796-425">**Component**</span></span>               | <span data-ttu-id="1c796-426">Webes API</span><span class="sxs-lookup"><span data-stu-id="1c796-426">Web API</span></span> | 
| <span data-ttu-id="1c796-427">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="1c796-427">**SDL Phase**</span></span>               | <span data-ttu-id="1c796-428">Felépítés</span><span class="sxs-lookup"><span data-stu-id="1c796-428">Build</span></span> |  
| <span data-ttu-id="1c796-429">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="1c796-429">**Applicable Technologies**</span></span> | <span data-ttu-id="1c796-430">Általános</span><span class="sxs-lookup"><span data-stu-id="1c796-430">Generic</span></span> |
| <span data-ttu-id="1c796-431">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="1c796-431">**Attributes**</span></span>              | <span data-ttu-id="1c796-432">N/A</span><span class="sxs-lookup"><span data-stu-id="1c796-432">N/A</span></span>  |
| <span data-ttu-id="1c796-433">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="1c796-433">**References**</span></span>              | <span data-ttu-id="1c796-434">N/A</span><span class="sxs-lookup"><span data-stu-id="1c796-434">N/A</span></span>  |
| <span data-ttu-id="1c796-435">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="1c796-435">**Steps**</span></span> | <span data-ttu-id="1c796-436">Webhelyközi kérések hamisítására (CSRF vagy XSRF), amelyben a támadó műveleteket végezhet el a biztonsági környezetében webhelyen egy másik felhasználói munkamenetet a támadás típusú.</span><span class="sxs-lookup"><span data-stu-id="1c796-436">Cross-site request forgery (CSRF or XSRF) is a type of attack in which an attacker can carry out actions in the security context of a different user's established session on a web site.</span></span> <span data-ttu-id="1c796-437">A célja módosítsa vagy törölje a tartalmat, ha a célként kijelölt webhely kizárólag a munkamenet-cookie-k hitelesítésére kérelem érkezett.</span><span class="sxs-lookup"><span data-stu-id="1c796-437">The goal is to modify or delete content, if the targeted web site relies exclusively on session cookies to authenticate received request.</span></span> <span data-ttu-id="1c796-438">A támadó a biztonsági rés olvasson be egy másik felhasználó böngésző betölteni egy parancs egy URL-címet egy sebezhető helyről, amikor a felhasználó már jelentkezett be.</span><span class="sxs-lookup"><span data-stu-id="1c796-438">An attacker could exploit this vulnerability by getting a different user's browser to load a URL with a command from a vulnerable site on which the user is already logged in.</span></span> <span data-ttu-id="1c796-439">Számos módon egy támadó úgy teheti meg, amely, például egy erőforrás betölti a sebezhető kiszolgálóról egy másik webhely üzemeltetéséhez, vagy a felhasználó első hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="1c796-439">There are many ways for an attacker to do that, such as by hosting a different web site that loads a resource from the vulnerable server, or getting the user to click a link.</span></span> <span data-ttu-id="1c796-440">Ha a kiszolgáló elküldi az ügyfélnek a további tokent, az ügyfél számára, hogy a jogkivonat szerepeljen minden jövőbeni kérelemhez szükséges, és ellenőrzi, hogy minden későbbi kérelmek tartalmaz egy jogkivonatot, amely vonatkozik az aktuális munkamenetről, például az ASP.NET használatával megelőzhető a támadás AntiForgeryToken vagy a megjelenítési állapot.</span><span class="sxs-lookup"><span data-stu-id="1c796-440">The attack can be prevented if the server sends an additional token to the client, requires the client to include that token in all future requests, and verifies that all future requests include a token that pertains to the current session, such as by using the ASP.NET AntiForgeryToken or ViewState.</span></span> |

| <span data-ttu-id="1c796-441">Cím</span><span class="sxs-lookup"><span data-stu-id="1c796-441">Title</span></span>                   | <span data-ttu-id="1c796-442">Részletek</span><span class="sxs-lookup"><span data-stu-id="1c796-442">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="1c796-443">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="1c796-443">**Component**</span></span>               | <span data-ttu-id="1c796-444">Webes API</span><span class="sxs-lookup"><span data-stu-id="1c796-444">Web API</span></span> | 
| <span data-ttu-id="1c796-445">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="1c796-445">**SDL Phase**</span></span>               | <span data-ttu-id="1c796-446">Felépítés</span><span class="sxs-lookup"><span data-stu-id="1c796-446">Build</span></span> |  
| <span data-ttu-id="1c796-447">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="1c796-447">**Applicable Technologies**</span></span> | <span data-ttu-id="1c796-448">MVC5, MVC6</span><span class="sxs-lookup"><span data-stu-id="1c796-448">MVC5, MVC6</span></span> |
| <span data-ttu-id="1c796-449">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="1c796-449">**Attributes**</span></span>              | <span data-ttu-id="1c796-450">N/A</span><span class="sxs-lookup"><span data-stu-id="1c796-450">N/A</span></span>  |
| <span data-ttu-id="1c796-451">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="1c796-451">**References**</span></span>              | [<span data-ttu-id="1c796-452">ASP.NET webes API-t a Webhelyközi kérések hamisítására (CSRF) támadások megelőzése</span><span class="sxs-lookup"><span data-stu-id="1c796-452">Preventing Cross-Site Request Forgery (CSRF) Attacks in ASP.NET Web API</span></span>](http://www.asp.net/web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks) |
| <span data-ttu-id="1c796-453">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="1c796-453">**Steps**</span></span> | <span data-ttu-id="1c796-454">Kártevőirtó-CSRF és AJAX: az űrlap token az AJAX-kérelmek problémát okozhat, mert AJAX-kérelmet el tudja küldeni a JSON-adatokat, nem a HTML-űrlapot adatok.</span><span class="sxs-lookup"><span data-stu-id="1c796-454">Anti-CSRF and AJAX: The form token can be a problem for AJAX requests, because an AJAX request might send JSON data, not HTML form data.</span></span> <span data-ttu-id="1c796-455">Egy megoldás, a jogkivonatok küldeni egy egyéni HTTP-fejlécben.</span><span class="sxs-lookup"><span data-stu-id="1c796-455">One solution is to send the tokens in a custom HTTP header.</span></span> <span data-ttu-id="1c796-456">Az alábbi kód Razor szintaxist használja a jogkivonatok létrehozásához, és hozzáadja a tokenek egy AJAX-kérelemre.</span><span class="sxs-lookup"><span data-stu-id="1c796-456">The following code uses Razor syntax to generate the tokens, and then adds the tokens to an AJAX request.</span></span> |

### <a name="example"></a><span data-ttu-id="1c796-457">Példa</span><span class="sxs-lookup"><span data-stu-id="1c796-457">Example</span></span>
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

### <a name="example"></a><span data-ttu-id="1c796-458">Példa</span><span class="sxs-lookup"><span data-stu-id="1c796-458">Example</span></span>
<span data-ttu-id="1c796-459">A kérelem feldolgozása során a jogkivonatok kinyerése a kérelem fejlécében.</span><span class="sxs-lookup"><span data-stu-id="1c796-459">When you process the request, extract the tokens from the request header.</span></span> <span data-ttu-id="1c796-460">Majd hívja a következő érvényesítse az AntiForgery.Validate metódust.</span><span class="sxs-lookup"><span data-stu-id="1c796-460">Then call the AntiForgery.Validate method to validate the tokens.</span></span> <span data-ttu-id="1c796-461">A Validate metódus kivételt jelez, ha a jogkivonatok nem érvényes.</span><span class="sxs-lookup"><span data-stu-id="1c796-461">The Validate method throws an exception if the tokens are not valid.</span></span>
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

### <a name="example"></a><span data-ttu-id="1c796-462">Példa</span><span class="sxs-lookup"><span data-stu-id="1c796-462">Example</span></span>
<span data-ttu-id="1c796-463">Kártevőirtó-CSRF és űrlapok ASP.NET MVC - AntiForgeryToken segédmetódus használatát nézetek; például egy Html.AntiForgeryToken() üzembe a formátumban:</span><span class="sxs-lookup"><span data-stu-id="1c796-463">Anti-CSRF and ASP.NET MVC forms - Use the AntiForgeryToken helper method on Views; put an Html.AntiForgeryToken() into the form, for example,</span></span>
```C#
@using (Html.BeginForm("UserProfile", "SubmitUpdate")) { 
    @Html.ValidationSummary(true) 
    @Html.AntiForgeryToken()
    <fieldset> 
}
```

### <a name="example"></a><span data-ttu-id="1c796-464">Példa</span><span class="sxs-lookup"><span data-stu-id="1c796-464">Example</span></span>
<span data-ttu-id="1c796-465">A fenti példában a következő hasonlót fog kimeneti:</span><span class="sxs-lookup"><span data-stu-id="1c796-465">The example above will output something like the following:</span></span>
```C#
<form action="/UserProfile/SubmitUpdate" method="post">
    <input name="__RequestVerificationToken" type="hidden" value="saTFWpkKN0BYazFtN6c4YbZAmsEwG0srqlUqqloi/fVgeV2ciIFVmelvzwRZpArs" />
    <!-- rest of form goes here -->
</form>
```

### <a name="example"></a><span data-ttu-id="1c796-466">Példa</span><span class="sxs-lookup"><span data-stu-id="1c796-466">Example</span></span>
<span data-ttu-id="1c796-467">Egy időben Html.AntiForgeryToken() __RequestVerificationToken, a fenti véletlenszerű rejtett érték azonos értékű nevű cookie lehetőséget ad a látogató adott területre.</span><span class="sxs-lookup"><span data-stu-id="1c796-467">At the same time, Html.AntiForgeryToken() gives the visitor a cookie called __RequestVerificationToken, with the same value as the random hidden value shown above.</span></span> <span data-ttu-id="1c796-468">Ezután egy bejövő közzétett űrlapból ellenőrzéséhez adja hozzá a [ValidateAntiForgeryToken] szűrő a célmetódushoz való kötéskor művelet.</span><span class="sxs-lookup"><span data-stu-id="1c796-468">Next, to validate an incoming form post, add the [ValidateAntiForgeryToken] filter to the target action method.</span></span> <span data-ttu-id="1c796-469">Példa:</span><span class="sxs-lookup"><span data-stu-id="1c796-469">For example:</span></span>
```
[ValidateAntiForgeryToken]
public ViewResult SubmitUpdate()
{
// ... etc.
}
```
<span data-ttu-id="1c796-470">Ellenőrzi, hogy a szűrő engedélyezési:</span><span class="sxs-lookup"><span data-stu-id="1c796-470">Authorization filter that checks that:</span></span>
* <span data-ttu-id="1c796-471">A bejövő kérelem rendelkezik egy cookie-k hívása __RequestVerificationToken</span><span class="sxs-lookup"><span data-stu-id="1c796-471">The incoming request has a cookie called __RequestVerificationToken</span></span>
* <span data-ttu-id="1c796-472">A bejövő kérelem egy `Request.Form` __RequestVerificationToken nevezett bejegyzés</span><span class="sxs-lookup"><span data-stu-id="1c796-472">The incoming request has a `Request.Form` entry called __RequestVerificationToken</span></span>
* <span data-ttu-id="1c796-473">A cookie-k és `Request.Form` jól értékek egyeznek, feltéve, hogy minden, a kérelem végighalad normál.</span><span class="sxs-lookup"><span data-stu-id="1c796-473">These cookie and `Request.Form` values match Assuming all is well, the request goes through as normal.</span></span> <span data-ttu-id="1c796-474">De ha nem, majd üzenettel engedélyezési hiba "szükséges hamisításgátló jogkivonat nincs megadva vagy érvénytelen".</span><span class="sxs-lookup"><span data-stu-id="1c796-474">But if not, then an authorization failure with message “A required anti-forgery token was not supplied or was invalid”.</span></span>

| <span data-ttu-id="1c796-475">Cím</span><span class="sxs-lookup"><span data-stu-id="1c796-475">Title</span></span>                   | <span data-ttu-id="1c796-476">Részletek</span><span class="sxs-lookup"><span data-stu-id="1c796-476">Details</span></span>      |
| ----------------------- | ------------ |
| <span data-ttu-id="1c796-477">**Összetevő**</span><span class="sxs-lookup"><span data-stu-id="1c796-477">**Component**</span></span>               | <span data-ttu-id="1c796-478">Webes API</span><span class="sxs-lookup"><span data-stu-id="1c796-478">Web API</span></span> | 
| <span data-ttu-id="1c796-479">**SDL fázis**</span><span class="sxs-lookup"><span data-stu-id="1c796-479">**SDL Phase**</span></span>               | <span data-ttu-id="1c796-480">Felépítés</span><span class="sxs-lookup"><span data-stu-id="1c796-480">Build</span></span> |  
| <span data-ttu-id="1c796-481">**Alkalmazandó technológiák**</span><span class="sxs-lookup"><span data-stu-id="1c796-481">**Applicable Technologies**</span></span> | <span data-ttu-id="1c796-482">MVC5, MVC6</span><span class="sxs-lookup"><span data-stu-id="1c796-482">MVC5, MVC6</span></span> |
| <span data-ttu-id="1c796-483">**Attribútumok**</span><span class="sxs-lookup"><span data-stu-id="1c796-483">**Attributes**</span></span>              | <span data-ttu-id="1c796-484">Identity Provider - ADFS, identitásszolgáltató - az Azure AD</span><span class="sxs-lookup"><span data-stu-id="1c796-484">Identity Provider - ADFS, Identity Provider - Azure AD</span></span> |
| <span data-ttu-id="1c796-485">**Hivatkozások**</span><span class="sxs-lookup"><span data-stu-id="1c796-485">**References**</span></span>              | [<span data-ttu-id="1c796-486">Egyes partnerek és az ASP.NET Web API 2.2 helyi bejelentkezési webes API biztonságossá tétele</span><span class="sxs-lookup"><span data-stu-id="1c796-486">Secure a Web API with Individual Accounts and Local Login in ASP.NET Web API 2.2</span></span>](http://www.asp.net/web-api/overview/security/individual-accounts-in-web-api) |
| <span data-ttu-id="1c796-487">**Lépések**</span><span class="sxs-lookup"><span data-stu-id="1c796-487">**Steps**</span></span> | <span data-ttu-id="1c796-488">Ha a webes API-t OAuth 2.0 használatával biztonságossá majd azt vár egy tulajdonosi jogkivonatot a hitelesítési kérelem fejlécében, és hozzáférést biztosít a kérelem csak akkor, ha a jogkivonat érvényes.</span><span class="sxs-lookup"><span data-stu-id="1c796-488">If the Web API is secured using OAuth 2.0, then it expects a bearer token in Authorization request header and grants access to the request only if the token is valid.</span></span> <span data-ttu-id="1c796-489">Cookie-alapú hitelesítés, eltérően böngészők csatolja a tulajdonosi jogkivonatok kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="1c796-489">Unlike cookie based authentication, browsers do not attach the bearer tokens to requests.</span></span> <span data-ttu-id="1c796-490">A kérelmező ügyfélnek kell explicit módon csatolja a tulajdonosi jogkivonattal, a kérelem fejlécében.</span><span class="sxs-lookup"><span data-stu-id="1c796-490">The requesting client needs to explicitly attach the bearer token in the request header.</span></span> <span data-ttu-id="1c796-491">Ezért az ASP.NET Web API-k OAuth 2.0 használatával védett, tulajdonosi jogkivonatok számít egy CSRF támadások elleni védelmet.</span><span class="sxs-lookup"><span data-stu-id="1c796-491">Therefore, for ASP.NET Web APIs protected using OAuth 2.0, bearer tokens are considered as a defense against CSRF attacks.</span></span> <span data-ttu-id="1c796-492">Vegye figyelembe, hogy ha az alkalmazás MVC része az űrlapos hitelesítés (azaz használ cookie-k) használ, hamisítás lehet az MVC webes alkalmazás által használható.</span><span class="sxs-lookup"><span data-stu-id="1c796-492">Please note that if the MVC portion of the application uses forms authentication (i.e., uses cookies), anti-forgery tokens have to be used by the MVC web app.</span></span> |

### <a name="example"></a><span data-ttu-id="1c796-493">Példa</span><span class="sxs-lookup"><span data-stu-id="1c796-493">Example</span></span>
<span data-ttu-id="1c796-494">A webes API-nak kell tájékoztatni csak a tulajdonosi jogkivonatok és nem a cookie-k használják.</span><span class="sxs-lookup"><span data-stu-id="1c796-494">The Web API has to be informed to rely ONLY on bearer tokens and not on cookies.</span></span> <span data-ttu-id="1c796-495">A következő konfigurációs végezhető `WebApiConfig.Register` metódus: "" C-éles kód config. SuppressDefaultHostAuthentication(); Config. Filters.Add (új HostAuthenticationFilter(OAuthDefaults.AuthenticationType));</span><span class="sxs-lookup"><span data-stu-id="1c796-495">It can be done by the following configuration in `WebApiConfig.Register` method: ``\`C-Sharp code config.SuppressDefaultHostAuthentication(); config.Filters.Add(new HostAuthenticationFilter(OAuthDefaults.AuthenticationType));</span></span>
```
The SuppressDefaultHostAuthentication method tells Web API to ignore any authentication that happens before the request reaches the Web API pipeline, either by IIS or by OWIN middleware. That way, we can restrict Web API to authenticate only using bearer tokens.