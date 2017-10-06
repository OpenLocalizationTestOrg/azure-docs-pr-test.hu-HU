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
# <a name="azure-active-directory-b2c-manage-sso-and-token-customization-with-custom-policies"></a>Az Azure Active Directory B2C: Egyszeri Bejelentkezéssel és token testreszabási egyéni házirendek kezelése
Egyéni házirendek segítségével biztosítja, azonos vezérelheti a jogkivonatot, munkamenet és egyszeri bejelentkezés (SSO) konfigurációk hello beépített házirendek segítségével.  a beállítások funkciójának, toolearn részletek a dokumentációban hello [Itt](#active-directory-b2c-token-session-sso).

## <a name="token-lifetimes-and-claims-configuration"></a>Jogkivonat élettartamát és a jogcímek konfiguráció
tooadd szüksége toochange hello beállításait a jogkivonat élettartamát, egy `<ClaimsProviders>` elem hello függő entitás fájlban hello házirend tooimpact szeretné.  Hello `<ClaimsProviders>` elem gyermeke hello `<TrustFrameworkPolicy>`.  Belül tooput hello információt, amely befolyásolja a jogkivonat élettartamát lesz szüksége.  hello XML így néz ki:

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

**Hozzáférési jogkivonat élettartamát** hozzáférési jogkivonatok élettartama hello hello értéket módosításával módosítható hello `<Item>` a hello kulcs = "token_lifetime_secs" másodpercben.  hello értéke alapértelmezés szerint beépített 3600 másodperc (60 perc).

**Azonosító a jogkivonatok élettartama** hello azonosítója a jogkivonatok élettartama hello hello értéket módosításával módosítható `<Item>` a hello kulcs = "id_token_lifetime_secs" másodpercben.  hello értéke alapértelmezés szerint beépített 3600 másodperc (60 perc).

**Frissítse a jogkivonatok élettartama** hello frissítési jogkivonat élettartamát hello hello értéket módosításával módosítható `<Item>` a hello kulcs = "refresh_token_lifetime_secs" másodpercben.  hello értéke alapértelmezés szerint beépített 1209600 másodperc (14 nap).

**Mozgó ablak élettartamot frissítése** Ha szeretné, hogy egy mozgó ablak élettartama tooyour frissítési jogkivonat tooset, módosítsa a hello értéket `<Item>` a hello kulcs = "rolling_refresh_token_lifetime_secs" másodpercben.  hello értéke alapértelmezés szerint beépített 7776000 (90 nap).  Ha nem szeretné, hogy tooenfore egy késleltetett ablak élettartamát, cserélje le a sort:
```XML
<Item Key="allow_infinite_rolling_refresh_token">True</Item>
```

**Jogcím kiállítója (iss)** toochange hello kibocsátó (iss) jogcím tetszés szerint módosítható hello értéke belül hello `<Item>` a hello kulcs = "IssuanceClaimPattern".  hello vonatkozó értékek a következők `AuthorityAndTenantGuid` és `AuthorityWithTfp`.

**Beállítás jogcím házirend-azonosító képviselő** hello az érték a következők TFP (megbízhatósági keretrendszer házirend) és ACR (hitelesítési környezeti hivatkozás).  
Azt javasoljuk, hogy a tooTFP toodo állítja, hello biztosítása `<Item>` a hello kulcs = "AuthenticationContextReferenceClaimPattern" létezik, és hello érték `None`.
Az a `<OutputClaims>` cikkhez, ez az elem hozzáadása:
```XML
<OutputClaim ClaimTypeReferenceId="trustFrameworkPolicy" Required="true" DefaultValue="{policy}" />
```
Az ACR, távolítsa el a hello `<Item>` a hello kulcs = "AuthenticationContextReferenceClaimPattern".

**Tulajdonos (rész-) jogcím** Ez a beállítás az alapértelmezett tooObjectID, ha azt szeretné, hogy tooswitch ez túl`Not Supported`, a következő hello:

Cserélje le ezt a sort 
```XML
<OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
```
Ehhez a sorhoz:
```XML
<OutputClaim ClaimTypeReferenceId="sub" />
```

## <a name="session-behavior-and-sso"></a>Munkamenet viselkedést és az egyszeri bejelentkezés
toochange a munkamenet viselkedést és egyszeri Bejelentkezéssel konfigurációk esetén tooadd van szüksége egy `<UserJourneyBehaviors>` hello elemet `<RelyingParty>` elemet.  Hello `<UserJourneyBehaviors>` elem közvetlenül követnie kell a hello `<DefaultUserJourney>`.  hello található a `<UserJourneyBehavors>` elem kell kinéznie:

```XML
<UserJourneyBehaviors>
   <SingleSignOn Scope="Application" />
   <SessionExpiryType>Absolute</SessionExpiryType>
   <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
</UserJourneyBehaviors>
```
**Egyszeri bejelentkezés (SSO) konfigurációs** toochange hello egyszeri bejelentkezés a konfigurációban kell toomodify hello értékének `<SingleSignOn>`.  hello vonatkozó értékek a következők `Tenant`, `Application`, `Policy` és `Disabled`. 

**Webalkalmazás munkamenet élettartama (perc)** toochange hello hello webalkalmazás munkamenetek élettartamát, kell hello toomodify értékének `<SessionExpiryInSeconds>` elemet.  hello alapértelmezett beépített házirendek érték 86400 másodperc (1440 perc).

**Webes alkalmazás munkamenet időkorlátja** toochange hello web app munkamenet időkorlátja kell toomodify hello értékének `<SessionExpiryType>`.  hello vonatkozó értékek a következők `Absolute` és `Rolling`.
