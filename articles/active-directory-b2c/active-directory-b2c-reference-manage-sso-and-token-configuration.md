---
title: "Az Azure Active Directory B2C: Egyszeri Bejelentkezéssel és egyéni házirendekkel token testreszabási kezelése |} Microsoft Docs"
description: 
services: active-directory-b2c
documentationcenter: 
author: sama
ms.assetid: eec4d418-453f-4755-8b30-5ed997841b56
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 05/02/2017
ms.author: sama
ms.openlocfilehash: 8f5703d15766f221517cd89352d41685652d32d6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-b2c-manage-sso-and-token-customization-with-custom-policies"></a><span data-ttu-id="de327-102">Az Azure Active Directory B2C: Egyszeri Bejelentkezéssel és token testreszabási egyéni házirendek kezelése</span><span class="sxs-lookup"><span data-stu-id="de327-102">Azure Active Directory B2C: Manage SSO and token customization with custom policies</span></span>
<span data-ttu-id="de327-103">Egyéni házirendek használatával teszi lehetővé a jogkivonatot, munkamenet és egyszeri bejelentkezés (SSO) konfigurációk ugyanarra a vezérlőre beépített házirendek segítségével.</span><span class="sxs-lookup"><span data-stu-id="de327-103">Using custom policies provides you the same control over your token, session and single sign-on (SSO) configurations as through built-in policies.</span></span>  <span data-ttu-id="de327-104">Ha szeretné megtudni, hogy egyes beállítások funkciója, a dokumentáció [Itt](#active-directory-b2c-token-session-sso).</span><span class="sxs-lookup"><span data-stu-id="de327-104">To learn what each setting does, please see the documentation [here](#active-directory-b2c-token-session-sso).</span></span>

## <a name="token-lifetimes-and-claims-configuration"></a><span data-ttu-id="de327-105">Jogkivonat élettartamát és a jogcímek konfiguráció</span><span class="sxs-lookup"><span data-stu-id="de327-105">Token lifetimes and claims configuration</span></span>
<span data-ttu-id="de327-106">A jogkivonat élettartamát a beállítások módosításához meg kell adni egy `<ClaimsProviders>` befolyásolja a kívánt házirendet, a függő entitás fájlban.</span><span class="sxs-lookup"><span data-stu-id="de327-106">To change the settings on your token lifetimes, you need to add a `<ClaimsProviders>` element in the relying party file of the policy you want to impact.</span></span>  <span data-ttu-id="de327-107">A `<ClaimsProviders>` elem gyermeke a `<TrustFrameworkPolicy>`.</span><span class="sxs-lookup"><span data-stu-id="de327-107">The `<ClaimsProviders>` element is a child of the `<TrustFrameworkPolicy>`.</span></span>  <span data-ttu-id="de327-108">Belül, amely befolyásolja a jogkivonat élettartamát helyezni lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="de327-108">Inside you'll need to put the information that affects your token lifetimes.</span></span>  <span data-ttu-id="de327-109">Az XML-fájl így néz ki:</span><span class="sxs-lookup"><span data-stu-id="de327-109">The XML looks like this:</span></span>

```XML
<ClaimsProviders>
   <ClaimsProvider>
      <DisplayName>Token Issuer</DisplayName>
      <TechnicalProfiles>
         <TechnicalProfile Id="JwtIssuer">
            <Metadata>
               <Item Key="token_lifetime_secs">3600</Item>
               <Item Key="id_token_lifetime_secs">3600</Item>
               <Item Key="refresh_token_lifetime_secs">1209600</Item>
               <Item Key="rolling_refresh_token_lifetime_secs">7776000</Item>
               <Item Key="IssuanceClaimPattern">AuthorityAndTenantGuid</Item>
               <Item Key="AuthenticationContextReferenceClaimPattern">None</Item>
            </Metadata>
         </TechnicalProfile>
      </TechnicalProfiles>
   </ClaimsProvider>
</ClaimsProviders>
```

<span data-ttu-id="de327-110">**Hozzáférési jogkivonat élettartamát** értéket módosításával módosíthatja a hozzáférési jogkivonat élettartamát a `<Item>` kulccsal = "token_lifetime_secs" másodpercben.</span><span class="sxs-lookup"><span data-stu-id="de327-110">**Access token lifetimes** The access token lifetime can be changed by modifying the value inside the `<Item>` with the Key="token_lifetime_secs" in seconds.</span></span>  <span data-ttu-id="de327-111">A beépített alapértelmezett érték 3600 másodperc (60 perc).</span><span class="sxs-lookup"><span data-stu-id="de327-111">The default value in built-in is 3600 seconds (60 minutes).</span></span>

<span data-ttu-id="de327-112">**A jogkivonatok élettartama azonosító** jogkivonat élettartamát az azonosító értéket módosításával módosítható a `<Item>` kulccsal = "id_token_lifetime_secs" másodpercben.</span><span class="sxs-lookup"><span data-stu-id="de327-112">**ID token lifetime** The ID token lifetime can be changed by modifying the value inside the `<Item>` with the Key="id_token_lifetime_secs" in seconds.</span></span>  <span data-ttu-id="de327-113">A beépített alapértelmezett érték 3600 másodperc (60 perc).</span><span class="sxs-lookup"><span data-stu-id="de327-113">The default value in built-in is 3600 seconds (60 minutes).</span></span>

<span data-ttu-id="de327-114">**Frissítse a jogkivonatok élettartama** értéket módosításával módosíthatja a frissítési jogkivonat élettartamát a `<Item>` kulccsal = "refresh_token_lifetime_secs" másodpercben.</span><span class="sxs-lookup"><span data-stu-id="de327-114">**Refresh token lifetime** The refresh token lifetime can be changed by modifying the value inside the `<Item>` with the Key="refresh_token_lifetime_secs" in seconds.</span></span>  <span data-ttu-id="de327-115">A beépített alapértelmezett érték 1209600 másodperc (14 nap).</span><span class="sxs-lookup"><span data-stu-id="de327-115">The default value in built-in is 1209600 seconds (14 days).</span></span>

<span data-ttu-id="de327-116">**Frissítési jogkivonat mozgó ablak élettartamát** Ha azt szeretné beállítani a mozgó ablak élettartama a frissítési jogkivonat, módosítsa az értéket `<Item>` kulccsal = "rolling_refresh_token_lifetime_secs" másodpercben.</span><span class="sxs-lookup"><span data-stu-id="de327-116">**Refresh token sliding window lifetime** If you would like to set a sliding window lifetime to your refresh token, modify the value inside `<Item>` with the Key="rolling_refresh_token_lifetime_secs" in seconds.</span></span>  <span data-ttu-id="de327-117">A beépített alapértelmezett érték 7776000 (90 nap).</span><span class="sxs-lookup"><span data-stu-id="de327-117">The default value in built-in is 7776000 (90 days).</span></span>  <span data-ttu-id="de327-118">Ha nem szeretné enfore egy késleltetett ablak élettartamát, cserélje le a sort:</span><span class="sxs-lookup"><span data-stu-id="de327-118">If you don't want to enfore a sliding window lifetime, replace this line with:</span></span>
```XML
<Item Key="allow_infinite_rolling_refresh_token">True</Item>
```

<span data-ttu-id="de327-119">**A jogcím kiállítójának (iss)** Ha a kibocsátó (iss) jogcím módosítani kívánja, módosítsa az értéket a `<Item>` kulccsal = "IssuanceClaimPattern".</span><span class="sxs-lookup"><span data-stu-id="de327-119">**Issuer (iss) claim** If you want to change the Issuer (iss) claim, modify the value inside the `<Item>` with the Key="IssuanceClaimPattern".</span></span>  <span data-ttu-id="de327-120">A megfelelő értékek `AuthorityAndTenantGuid` és `AuthorityWithTfp`.</span><span class="sxs-lookup"><span data-stu-id="de327-120">The applicable values are `AuthorityAndTenantGuid` and `AuthorityWithTfp`.</span></span>

<span data-ttu-id="de327-121">**Beállítás jogcím házirend-azonosító képviselő** érték a beállítások a következők TFP (megbízhatósági keretrendszer házirend) és ACR (hitelesítési környezeti hivatkozás).</span><span class="sxs-lookup"><span data-stu-id="de327-121">**Setting claim representing policy ID** The options for setting this value are TFP (trust framework policy) and ACR (authentication context reference).</span></span>  
<span data-ttu-id="de327-122">Javasoljuk, hogy ezt a TFP beállítást, ehhez ellenőrizze a `<Item>` és a kulcs = "AuthenticationContextReferenceClaimPattern" létezik, és az érték `None`.</span><span class="sxs-lookup"><span data-stu-id="de327-122">We recommend setting this to TFP, to do this, ensure the `<Item>` with the Key="AuthenticationContextReferenceClaimPattern" exists and the value is `None`.</span></span>
<span data-ttu-id="de327-123">Az a `<OutputClaims>` cikkhez, ez az elem hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="de327-123">In your `<OutputClaims>` item, add this element:</span></span>
```XML
<OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />
```
<span data-ttu-id="de327-124">ACR, távolítsa el a `<Item>` kulccsal = "AuthenticationContextReferenceClaimPattern".</span><span class="sxs-lookup"><span data-stu-id="de327-124">For ACR, remove the `<Item>` with the Key="AuthenticationContextReferenceClaimPattern".</span></span>

<span data-ttu-id="de327-125">**Tulajdonos (rész-) jogcím** ObjectID, hogy alapértelmezett ezt a beállítást, ha azt szeretné, a kapcsolóhoz `Not Supported`, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="de327-125">**Subject (sub) claim** This option is defaulted to ObjectID, if you would like to switch this to `Not Supported`, do the following:</span></span>

<span data-ttu-id="de327-126">Cserélje le ezt a sort</span><span class="sxs-lookup"><span data-stu-id="de327-126">Replace this line</span></span> 
```XML
<OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
```
<span data-ttu-id="de327-127">Ehhez a sorhoz:</span><span class="sxs-lookup"><span data-stu-id="de327-127">with this line:</span></span>
```XML
<OutputClaim ClaimTypeReferenceId="sub" />
```

## <a name="session-behavior-and-sso"></a><span data-ttu-id="de327-128">Munkamenet viselkedést és az egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="de327-128">Session behavior and SSO</span></span>
<span data-ttu-id="de327-129">A munkamenet viselkedést és egyszeri Bejelentkezéssel konfigurációk módosítása, meg kell adni egy `<UserJourneyBehaviors>` elemben található a `<RelyingParty>` elemet.</span><span class="sxs-lookup"><span data-stu-id="de327-129">To change your session behavior and SSO configurations, you need to add a `<UserJourneyBehaviors>` element inside of the `<RelyingParty>` element.</span></span>  <span data-ttu-id="de327-130">A `<UserJourneyBehaviors>` elem közvetlenül követnie kell a `<DefaultUserJourney>`.</span><span class="sxs-lookup"><span data-stu-id="de327-130">The `<UserJourneyBehaviors>` element must immediately follow the `<DefaultUserJourney>`.</span></span>  <span data-ttu-id="de327-131">Belső a `<UserJourneyBehavors>` elem kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="de327-131">The inside of your `<UserJourneyBehavors>` element should look like this:</span></span>

```XML
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```
<span data-ttu-id="de327-132">**Egyszeri bejelentkezés (SSO) konfigurációs** való egyszeri bejelentkezés beállításainak módosítása esetén módosítania kell a értékének `<SingleSignOn>`.</span><span class="sxs-lookup"><span data-stu-id="de327-132">**Single sign-on (SSO) configuration** To change the single sign-on configuration, you need to modify the value of `<SingleSignOn>`.</span></span>  <span data-ttu-id="de327-133">A megfelelő értékek `Tenant`, `Application`, `Policy` és `Disabled`.</span><span class="sxs-lookup"><span data-stu-id="de327-133">The applicable values are `Tenant`, `Application`, `Policy` and `Disabled`.</span></span> 

<span data-ttu-id="de327-134">**Webalkalmazás munkamenet élettartama (perc)** módosítása a webalkalmazás munkamenetek élettartamát, módosítania kell értékének a `<SessionExpiryInSeconds>` elemet.</span><span class="sxs-lookup"><span data-stu-id="de327-134">**Web app session lifetime (minutes)** To change the the web app session lifetime, you need to modify value of the `<SessionExpiryInSeconds>` element.</span></span>  <span data-ttu-id="de327-135">Az alapértelmezett beépített házirendek értéke 86400 másodperc (1440 perc).</span><span class="sxs-lookup"><span data-stu-id="de327-135">The default value in built-in policies is 86400 seconds (1440 minutes).</span></span>

<span data-ttu-id="de327-136">**Webes alkalmazás munkamenet időkorlátja** Ha módosítani szeretné a webes alkalmazás munkamenet időkorlátja, módosítania kell a értékének `<SessionExpiryType>`.</span><span class="sxs-lookup"><span data-stu-id="de327-136">**Web app session timeout** To change the web app session timeout, you need to modify the value of `<SessionExpiryType>`.</span></span>  <span data-ttu-id="de327-137">A megfelelő értékek `Absolute` és `Rolling`.</span><span class="sxs-lookup"><span data-stu-id="de327-137">The applicable values are `Absolute` and `Rolling`.</span></span>
