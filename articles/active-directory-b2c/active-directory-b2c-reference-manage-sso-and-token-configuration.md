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
ms.openlocfilehash: b65271a22c77ea41eeec2126e4a3ad24364edd17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-manage-sso-and-token-customization-with-custom-policies"></a><span data-ttu-id="65110-102">Az Azure Active Directory B2C: Egyszeri Bejelentkezéssel és token testreszabási egyéni házirendek kezelése</span><span class="sxs-lookup"><span data-stu-id="65110-102">Azure Active Directory B2C: Manage SSO and token customization with custom policies</span></span>
<span data-ttu-id="65110-103">Egyéni házirendek segítségével biztosítja, azonos vezérelheti a jogkivonatot, munkamenet és egyszeri bejelentkezés (SSO) konfigurációk hello beépített házirendek segítségével.</span><span class="sxs-lookup"><span data-stu-id="65110-103">Using custom policies provides you hello same control over your token, session and single sign-on (SSO) configurations as through built-in policies.</span></span>  <span data-ttu-id="65110-104">a beállítások funkciójának, toolearn részletek a dokumentációban hello [Itt](#active-directory-b2c-token-session-sso).</span><span class="sxs-lookup"><span data-stu-id="65110-104">toolearn what each setting does, please see hello documentation [here](#active-directory-b2c-token-session-sso).</span></span>

## <a name="token-lifetimes-and-claims-configuration"></a><span data-ttu-id="65110-105">Jogkivonat élettartamát és a jogcímek konfiguráció</span><span class="sxs-lookup"><span data-stu-id="65110-105">Token lifetimes and claims configuration</span></span>
<span data-ttu-id="65110-106">tooadd szüksége toochange hello beállításait a jogkivonat élettartamát, egy `<ClaimsProviders>` elem hello függő entitás fájlban hello házirend tooimpact szeretné.</span><span class="sxs-lookup"><span data-stu-id="65110-106">toochange hello settings on your token lifetimes, you need tooadd a `<ClaimsProviders>` element in hello relying party file of hello policy you want tooimpact.</span></span>  <span data-ttu-id="65110-107">Hello `<ClaimsProviders>` elem gyermeke hello `<TrustFrameworkPolicy>`.</span><span class="sxs-lookup"><span data-stu-id="65110-107">hello `<ClaimsProviders>` element is a child of hello `<TrustFrameworkPolicy>`.</span></span>  <span data-ttu-id="65110-108">Belül tooput hello információt, amely befolyásolja a jogkivonat élettartamát lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="65110-108">Inside you'll need tooput hello information that affects your token lifetimes.</span></span>  <span data-ttu-id="65110-109">hello XML így néz ki:</span><span class="sxs-lookup"><span data-stu-id="65110-109">hello XML looks like this:</span></span>

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

<span data-ttu-id="65110-110">**Hozzáférési jogkivonat élettartamát** hozzáférési jogkivonatok élettartama hello hello értéket módosításával módosítható hello `<Item>` a hello kulcs = "token_lifetime_secs" másodpercben.</span><span class="sxs-lookup"><span data-stu-id="65110-110">**Access token lifetimes** hello access token lifetime can be changed by modifying hello value inside hello `<Item>` with hello Key="token_lifetime_secs" in seconds.</span></span>  <span data-ttu-id="65110-111">hello értéke alapértelmezés szerint beépített 3600 másodperc (60 perc).</span><span class="sxs-lookup"><span data-stu-id="65110-111">hello default value in built-in is 3600 seconds (60 minutes).</span></span>

<span data-ttu-id="65110-112">**Azonosító a jogkivonatok élettartama** hello azonosítója a jogkivonatok élettartama hello hello értéket módosításával módosítható `<Item>` a hello kulcs = "id_token_lifetime_secs" másodpercben.</span><span class="sxs-lookup"><span data-stu-id="65110-112">**ID token lifetime** hello ID token lifetime can be changed by modifying hello value inside hello `<Item>` with hello Key="id_token_lifetime_secs" in seconds.</span></span>  <span data-ttu-id="65110-113">hello értéke alapértelmezés szerint beépített 3600 másodperc (60 perc).</span><span class="sxs-lookup"><span data-stu-id="65110-113">hello default value in built-in is 3600 seconds (60 minutes).</span></span>

<span data-ttu-id="65110-114">**Frissítse a jogkivonatok élettartama** hello frissítési jogkivonat élettartamát hello hello értéket módosításával módosítható `<Item>` a hello kulcs = "refresh_token_lifetime_secs" másodpercben.</span><span class="sxs-lookup"><span data-stu-id="65110-114">**Refresh token lifetime** hello refresh token lifetime can be changed by modifying hello value inside hello `<Item>` with hello Key="refresh_token_lifetime_secs" in seconds.</span></span>  <span data-ttu-id="65110-115">hello értéke alapértelmezés szerint beépített 1209600 másodperc (14 nap).</span><span class="sxs-lookup"><span data-stu-id="65110-115">hello default value in built-in is 1209600 seconds (14 days).</span></span>

<span data-ttu-id="65110-116">**Mozgó ablak élettartamot frissítése** Ha szeretné, hogy egy mozgó ablak élettartama tooyour frissítési jogkivonat tooset, módosítsa a hello értéket `<Item>` a hello kulcs = "rolling_refresh_token_lifetime_secs" másodpercben.</span><span class="sxs-lookup"><span data-stu-id="65110-116">**Refresh token sliding window lifetime** If you would like tooset a sliding window lifetime tooyour refresh token, modify hello value inside `<Item>` with hello Key="rolling_refresh_token_lifetime_secs" in seconds.</span></span>  <span data-ttu-id="65110-117">hello értéke alapértelmezés szerint beépített 7776000 (90 nap).</span><span class="sxs-lookup"><span data-stu-id="65110-117">hello default value in built-in is 7776000 (90 days).</span></span>  <span data-ttu-id="65110-118">Ha nem szeretné, hogy tooenfore egy késleltetett ablak élettartamát, cserélje le a sort:</span><span class="sxs-lookup"><span data-stu-id="65110-118">If you don't want tooenfore a sliding window lifetime, replace this line with:</span></span>
```XML
<Item Key="allow_infinite_rolling_refresh_token">True</Item>
```

<span data-ttu-id="65110-119">**Jogcím kiállítója (iss)** toochange hello kibocsátó (iss) jogcím tetszés szerint módosítható hello értéke belül hello `<Item>` a hello kulcs = "IssuanceClaimPattern".</span><span class="sxs-lookup"><span data-stu-id="65110-119">**Issuer (iss) claim** If you want toochange hello Issuer (iss) claim, modify hello value inside hello `<Item>` with hello Key="IssuanceClaimPattern".</span></span>  <span data-ttu-id="65110-120">hello vonatkozó értékek a következők `AuthorityAndTenantGuid` és `AuthorityWithTfp`.</span><span class="sxs-lookup"><span data-stu-id="65110-120">hello applicable values are `AuthorityAndTenantGuid` and `AuthorityWithTfp`.</span></span>

<span data-ttu-id="65110-121">**Beállítás jogcím házirend-azonosító képviselő** hello az érték a következők TFP (megbízhatósági keretrendszer házirend) és ACR (hitelesítési környezeti hivatkozás).</span><span class="sxs-lookup"><span data-stu-id="65110-121">**Setting claim representing policy ID** hello options for setting this value are TFP (trust framework policy) and ACR (authentication context reference).</span></span>  
<span data-ttu-id="65110-122">Azt javasoljuk, hogy a tooTFP toodo állítja, hello biztosítása `<Item>` a hello kulcs = "AuthenticationContextReferenceClaimPattern" létezik, és hello érték `None`.</span><span class="sxs-lookup"><span data-stu-id="65110-122">We recommend setting this tooTFP, toodo this, ensure hello `<Item>` with hello Key="AuthenticationContextReferenceClaimPattern" exists and hello value is `None`.</span></span>
<span data-ttu-id="65110-123">Az a `<OutputClaims>` cikkhez, ez az elem hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="65110-123">In your `<OutputClaims>` item, add this element:</span></span>
```XML
<OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />
```
<span data-ttu-id="65110-124">Az ACR, távolítsa el a hello `<Item>` a hello kulcs = "AuthenticationContextReferenceClaimPattern".</span><span class="sxs-lookup"><span data-stu-id="65110-124">For ACR, remove hello `<Item>` with hello Key="AuthenticationContextReferenceClaimPattern".</span></span>

<span data-ttu-id="65110-125">**Tulajdonos (rész-) jogcím** Ez a beállítás az alapértelmezett tooObjectID, ha azt szeretné, hogy tooswitch ez túl`Not Supported`, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="65110-125">**Subject (sub) claim** This option is defaulted tooObjectID, if you would like tooswitch this too`Not Supported`, do hello following:</span></span>

<span data-ttu-id="65110-126">Cserélje le ezt a sort</span><span class="sxs-lookup"><span data-stu-id="65110-126">Replace this line</span></span> 
```XML
<OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
```
<span data-ttu-id="65110-127">Ehhez a sorhoz:</span><span class="sxs-lookup"><span data-stu-id="65110-127">with this line:</span></span>
```XML
<OutputClaim ClaimTypeReferenceId="sub" />
```

## <a name="session-behavior-and-sso"></a><span data-ttu-id="65110-128">Munkamenet viselkedést és az egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="65110-128">Session behavior and SSO</span></span>
<span data-ttu-id="65110-129">toochange a munkamenet viselkedést és egyszeri Bejelentkezéssel konfigurációk esetén tooadd van szüksége egy `<UserJourneyBehaviors>` hello elemet `<RelyingParty>` elemet.</span><span class="sxs-lookup"><span data-stu-id="65110-129">toochange your session behavior and SSO configurations, you need tooadd a `<UserJourneyBehaviors>` element inside of hello `<RelyingParty>` element.</span></span>  <span data-ttu-id="65110-130">Hello `<UserJourneyBehaviors>` elem közvetlenül követnie kell a hello `<DefaultUserJourney>`.</span><span class="sxs-lookup"><span data-stu-id="65110-130">hello `<UserJourneyBehaviors>` element must immediately follow hello `<DefaultUserJourney>`.</span></span>  <span data-ttu-id="65110-131">hello található a `<UserJourneyBehavors>` elem kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="65110-131">hello inside of your `<UserJourneyBehavors>` element should look like this:</span></span>

```XML
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```
<span data-ttu-id="65110-132">**Egyszeri bejelentkezés (SSO) konfigurációs** toochange hello egyszeri bejelentkezés a konfigurációban kell toomodify hello értékének `<SingleSignOn>`.</span><span class="sxs-lookup"><span data-stu-id="65110-132">**Single sign-on (SSO) configuration** toochange hello single sign-on configuration, you need toomodify hello value of `<SingleSignOn>`.</span></span>  <span data-ttu-id="65110-133">hello vonatkozó értékek a következők `Tenant`, `Application`, `Policy` és `Disabled`.</span><span class="sxs-lookup"><span data-stu-id="65110-133">hello applicable values are `Tenant`, `Application`, `Policy` and `Disabled`.</span></span> 

<span data-ttu-id="65110-134">**Webalkalmazás munkamenet élettartama (perc)** toochange hello hello webalkalmazás munkamenetek élettartamát, kell hello toomodify értékének `<SessionExpiryInSeconds>` elemet.</span><span class="sxs-lookup"><span data-stu-id="65110-134">**Web app session lifetime (minutes)** toochange hello hello web app session lifetime, you need toomodify value of hello `<SessionExpiryInSeconds>` element.</span></span>  <span data-ttu-id="65110-135">hello alapértelmezett beépített házirendek érték 86400 másodperc (1440 perc).</span><span class="sxs-lookup"><span data-stu-id="65110-135">hello default value in built-in policies is 86400 seconds (1440 minutes).</span></span>

<span data-ttu-id="65110-136">**Webes alkalmazás munkamenet időkorlátja** toochange hello web app munkamenet időkorlátja kell toomodify hello értékének `<SessionExpiryType>`.</span><span class="sxs-lookup"><span data-stu-id="65110-136">**Web app session timeout** toochange hello web app session timeout, you need toomodify hello value of `<SessionExpiryType>`.</span></span>  <span data-ttu-id="65110-137">hello vonatkozó értékek a következők `Absolute` és `Rolling`.</span><span class="sxs-lookup"><span data-stu-id="65110-137">hello applicable values are `Absolute` and `Rolling`.</span></span>
