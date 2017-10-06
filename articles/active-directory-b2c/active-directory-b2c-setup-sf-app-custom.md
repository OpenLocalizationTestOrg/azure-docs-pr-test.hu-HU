---
title: "Az Azure Active Directory B2C: Salesforce SAML-szolgáltató hozzáadása a egyéni házirendek használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate és az Azure Active Directory B2C egyéni házirendek kezelése."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: d7f4143f-cd7c-4939-91a8-231a4104dc2c
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 06/11/2017
ms.author: parakhj
ms.openlocfilehash: f14c9d96980ff124110db7cfb58bf7cd81750b7c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-sign-in-by-using-salesforce-accounts-via-saml"></a>Az Azure Active Directory B2C: Salesforce-fiókok keresztül SAML használatával írja alá

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Ez a cikk bemutatja, hogyan toouse [egyéni házirendek](active-directory-b2c-overview-custom.md) tooset mentése bejelentkezhet a felhasználók egy adott Salesforce szervezettől származik.

## <a name="prerequisites"></a>Előfeltételek

### <a name="azure-ad-b2c-setup"></a>Az Azure AD B2C beállítása

Győződjön meg arról, hogy végrehajtotta hello lépéseket, amelyek bemutatják, hogyan túl[Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md) az Azure Active Directory B2C (Azure AD B2C-vel).

Ezek a következők:

* Az Azure AD B2C bérlő létrehozása.
* Az Azure AD B2C alkalmazás létrehozása.
* Két házirend motor alkalmazás regisztrálásához.
* Kulcsok beállítása.
* Hello alapszintű csomag beállításához.

### <a name="salesforce-setup"></a>Salesforce-telepítő

Ebben a cikkben azt feltételezzük, hogy már elvégezte hello következő:

* A Salesforce-fiókhoz való regisztráció. Regisztrálhatnak az egy [ingyenes Developer Edition fiókot](https://developer.salesforce.com/signup).
* [Saját tartomány beállítása](https://help.salesforce.com/articleView?id=domain_name_setup.htm&language=en_US&type=0) Salesforce szervezete számára.

## <a name="set-up-salesforce-so-users-can-federate"></a>Így a felhasználók is összevonni Salesforce beállítása

Salesforce folytatott kommunikációhoz az Azure AD B2C toohelp, tooget hello Salesforce metaadatainak URL-CÍMÉT kell.

### <a name="set-up-salesforce-as-an-identity-provider"></a>Identitás-szolgáltatóként Salesforce beállítása

> [!NOTE]
> Ebben a cikkben azt feltételezzük, hogy azok be [Salesforce Lightning élmény](https://developer.salesforce.com/page/Lightning_Experience_FAQ).

1. [Jelentkezzen be tooSalesforce](https://login.salesforce.com/). 
2. Hello a bal oldali menü alatti **beállítások**, bontsa ki a **identitás**, és kattintson a **identitásszolgáltató**.
3. Kattintson a **identitásszolgáltató engedélyezése**.
4. A **válassza hello tanúsítvány**, jelölje be azt szeretné, hogy Salesforce toouse toocommunicate Azure AD B2C hello tanúsítványt. (Hello alapértelmezett tanúsítvány is használható.) Kattintson a **Save** (Mentés) gombra. 

### <a name="create-a-connected-app-in-salesforce"></a>Hozzon létre egy csatlakoztatott alkalmazáshoz a Salesforce-ban

1. A hello **identitásszolgáltató** lapon, nyissa meg túl**szolgáltatók**.
2. Kattintson a **szolgáltatók létrejön keresztül csatlakozó alkalmazásokat. Kattintson ide.**
3. A **alapvető információkat**, adja meg a csatlakoztatott alkalmazáshoz tartozó hello szükséges értékeket.
4. A **a webalkalmazás-beállítások**, jelölje be hello **SAML engedélyezése** jelölőnégyzetet.
5. A hello **Entitásazonosító** mezőbe írja be a következő URL-cím hello. Győződjön meg arról, hogy cserélje le a hello értéke `tenantName`.
      ```
      https://login.microsoftonline.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase
      ```
6. A hello **ACS URL-cím** mezőbe írja be a következő URL-cím hello. Győződjön meg arról, hogy cserélje le a hello értéke `tenantName`.
      ```
      https://login.microsoftonline.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase/samlp/sso/assertionconsumer
      ```
7. Hagyja meg a többi beállítás hello alapértelmezett értékeit.
8. Görgessen toohello hello lista aljára, és kattintson **mentése**.

### <a name="get-hello-metadata-url"></a>Hello metaadatainak URL-cím beszerzése

1. Kattintson az hello áttekintése lapon a csatlakoztatott alkalmazás **kezelése**.
2. Másolja a hello értéke **metaadatok Discovery Endpoint**, és mentse azt. Ez a cikk későbbi részében fogja használni.

### <a name="set-up-salesforce-users-toofederate"></a>Salesforce felhasználók toofederate beállítása

1. A hello **kezelése** lap a csatlakoztatott alkalmazáshoz, nyissa meg túl**profilok**.
2. Kattintson a **profilok kezeléséhez**.
3. Válassza ki a hello profilok (vagy felhasználói csoportok), amelyet az Azure AD B2C toofederate. Rendszergazdaként, válassza ki a hello **rendszergazda** jelölőnégyzetet, így akkor is összevonva a Salesforce-fiókjával.

## <a name="generate-a-signing-certificate-for-azure-ad-b2c"></a>Aláíró tanúsítvány jön létre az Azure AD B2C-vel

Kérelmek küldése tooSalesforce kell toobe az Azure AD B2C által aláírt. toogenerate egy aláíró tanúsítványt, nyissa meg az Azure PowerShell, és futtassa a következő parancsok hello.

> [!NOTE]
> Ellenőrizze, hogy frissítse hello bérlő neve és a jelszó hello felső két sort.

```PowerShell
$tenantName = "<YOUR TENANT NAME>.onmicrosoft.com"
$pwdText = "<YOUR PASSWORD HERE>"

$Cert = New-SelfSignedCertificate -CertStoreLocation Cert:\CurrentUser\My -DnsName "SamlIdp.$tenantName" -Subject "B2C SAML Signing Cert" -HashAlgorithm SHA256 -KeySpec Signature -KeyLength 2048

$pwd = ConvertTo-SecureString -String $pwdText -Force -AsPlainText

Export-PfxCertificate -Cert $Cert -FilePath .\B2CSigningCert.pfx -Password $pwd
```

## <a name="add-hello-saml-signing-certificate-tooazure-ad-b2c"></a>Hello SAML aláíró tanúsítvány tooAzure AD B2C hozzáadása

Töltse fel az aláíró tanúsítvány tooyour az Azure AD B2C bérlő hello: 

1. Nyissa meg tooyour az Azure AD B2C-bérlő. Kattintson a **beállítások** > **identitás élmény keretrendszer** > **házirend kulcsok**.
2. Kattintson a **+ Hozzáadás**, majd:
    1. Kattintson a **beállítások** > **feltöltése**.
    2. Adjon meg egy **neve** (például SAMLSigningCert). hello előtag *B2C_1A_* a rendszer automatikusan hozzáadja a kulcs toohello neve.
    3. tooselect a tanúsítványt, jelölje be **vezérlő feltöltése**. 
    4. Adja meg a hello PowerShell-parancsfájl a megadott hello tanúsítvány jelszavát.
3. Kattintson a **Create** (Létrehozás) gombra.
4. Győződjön meg arról, hogy létrehozott egy kulcs (például B2C_1A_SAMLSigningCert). Jegyezze fel a teljes név hello (beleértve a *B2C_1A_*). Később hello házirend toothis kulcsot fogja hivatkozunk.

## <a name="create-hello-salesforce-saml-claims-provider-in-your-base-policy"></a>A kiinduló házirend hello Salesforce SAML jogcím-szolgáltató létrehozása

Toodefine Salesforce kell egy jogcímszolgáltatótól, így a felhasználók bejelentkezés Salesforce használatával. Ez azt jelenti az Azure AD B2C kommunikáló toospecify hello végpont van szüksége. hello végpont fog *meg* készlete *jogcímek* , hogy az Azure AD B2C használ, amely egy adott felhasználó hitelesítette tooverify. toodo, vegye fel a `<ClaimsProvider>` a Salesforce fájljában hello bővítmény a házirend:

1. Nyissa meg a munkakönyvtár hello bővítményfájl (TrustFrameworkExtensions.xml).
2. Hello található `<ClaimsProviders>` szakasz. Ha még nem létezik, hozzon létre hello legfelső szintű csomópontja alatt.
3. Adjon hozzá egy új `<ClaimsProvider>`:

    ```XML
    <ClaimsProvider>
      <Domain>salesforce</Domain>
      <DisplayName>Salesforce</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="salesforce">
          <DisplayName>Salesforce</DisplayName>
          <Description>Login with your Salesforce account</Description>
          <Protocol Name="SAML2"/>
          <Metadata>
            <Item Key="RequestsSigned">false</Item>
            <Item Key="WantsEncryptedAssertions">false</Item>
            <Item Key="WantsSignedAssertions">false</Item>
            <Item Key="PartnerEntity">https://contoso-dev-ed.my.salesforce.com/.well-known/samlidp.xml</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_SAMLSigningCert"/>
            <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_SAMLSigningCert"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="userId"/>
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="username"/>
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="externalIdp"/>
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="SAMLIdp" />
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop"/>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

A hello `<ClaimsProvider>` csomópont:

1. Hello értékének módosításához `<Domain>` tooa egyedi érték, amely megkülönbözteti `<ClaimsProvider>` az egyéb identitás-szolgáltatóktól származó.
2. Frissítse a hello értéket `<DisplayName>` tooa megjelenítési név hello jogcím-szolgáltató. Jelenleg ez az érték nem használt.

### <a name="update-hello-technical-profile"></a>Műszaki hello-profil frissítése

a Salesforce, SAML-jogkivonatból tooget hello protokollok, hogy az Azure AD B2C toocommunicate használja az Azure Active Directoryval (Azure AD) határozza meg. Ehhez a hello `<TechnicalProfile>` eleme `<ClaimsProvider>`:

1. Hello hello azonosítója frissítése `<TechnicalProfile>` csomópont. Ez az azonosító nem használt toorefer toothis műszaki profil hello házirend egyéb részeitől.
2. Frissítse a hello értéket `<DisplayName>`. Ez az érték jelenik meg a bejelentkezési lapon hello bejelentkezési gombra.
3. Frissítse a hello értéket `<Description>`.
4. Salesforce hello SAML 2.0-s protokollt használja. Győződjön meg arról, hogy hello értéke `<Protocol>` van **egy SAML2**.

Frissítés hello `<Metadata>` XML tooreflect hello-beállítások a megadott Salesforce-fiókhoz megelőző hello című szakasza. Hello XML, a frissítés hello metaadatok:

1. Hello értékét `<Item Key="PartnerEntity">` hello Salesforce metaadatainak URL-korábban kimásolt. A következő formátumban hello rendelkezik: 

    `https://contoso-dev-ed.my.salesforce.com/.well-known/samlidp/connectedapp.xml`

2. A hello `<CryptographicKeys>` részben, a frissítés hello érték mindkét példányai `StorageReferenceId` hello kulcs az aláíró tanúsítványban (például B2C_1A_SAMLSigningCert) toohello neve.

### <a name="upload-hello-extension-file-for-verification"></a>Az ellenőrzéshez hello kiterjesztésű fájl feltöltése

A házirend most úgy van beállítva, hogy az Azure AD B2C tudja, hogyan toocommunicate a Salesforce. Próbálja meg a házirend, hogy nincs probléma merül fel, amennyiben tooverify hello kiterjesztésű fájl feltöltése. tooupload hello kiterjesztésű fájlt a házirend:

1. Lépjen az Azure AD B2C-bérlő toohello **házirend** panelen.
2. Jelölje be hello **hello házirend felülírja, ha létezik** jelölőnégyzetet.
3. Töltse fel hello kiterjesztésű fájlt (TrustFrameworkExtensions.xml). Győződjön meg arról, hogy azt nem érvényesítése sikertelen.

## <a name="register-hello-salesforce-saml-claims-provider-tooa-user-journey"></a>Hello Salesforce SAML jogcímeket szolgáltató tooa felhasználói út regisztrálása

Ezután adja hozzá a Salesforce SAML identity provider tooone, a felhasználó utak hello. Ezen a ponton hello identitásszolgáltató be van állítva, de bármely hello felhasználói előfizetési vagy a bejelentkezési oldal nem érhető el. tooadd hello identity provider tooa bejelentkezési oldalára, először hozzon létre egy meglévő sablon felhasználói út duplikált. Hello sablont, majd módosítani, hogy az Azure AD identitásszolgáltató hello is rendelkezik.

1. Nyissa meg a hello alap fájlt a házirend (például TrustFrameworkBase.xml).
2. Hello található `<UserJourneys>` elemet, majd a teljes másolat hello `<UserJourney>` érték, beleértve az Id = "SignUpOrSignIn".
3. Nyissa meg a hello kiterjesztésű fájlt (például TrustFrameworkExtensions.xml). Hello található `<UserJourneys>` elemet. Ha hello elem nem létezik, hozzon létre egyet.
4. Beillesztés hello teljes másolt `<UserJourney>` hello gyermekeként `<UserJourneys>` elemet.
5. Nevezze át az új hello hello azonosítója `<UserJourney>` (például SignUpOrSignUsingContoso).

### <a name="display-hello-identity-provider-button"></a>Megjelenítési hello identity provider gomb

Hello `<ClaimsProviderSelection>` elem hasonló tooan identity provider gomb regisztráció vagy bejelentkezés lapon. Adja hozzá egy `<ClaimsProviderSelection>` Salesforce eleme, egy új gomb akkor jelenik meg, amikor egy felhasználó toothis lap. toodisplay hello identity provider gombra:

1. A hello `<UserJourney>` létrehozott, a hello található `<OrchestrationStep>` rendelkező `Order="1"`.
2. Adja hozzá a következő XML hello:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

3. Állítsa be `TargetClaimsExchangeId` tooa logikai érték. Javasoljuk a következő hello ugyanaz, mint mások egyezmény (például  *\[ClaimProviderName\]Exchange*).

### <a name="link-hello-identity-provider-button-tooan-action"></a>Hivatkozás hello identitás szolgáltató tooan megnyomásakor

Most, hogy az identity provider gomb helyen, hivatkozás tooan művelet. Ebben az esetben az Azure AD B2C toocommunicate a Salesforce tooreceive SAML-jogkivonatból kell hello műveletet. Ehhez a Salesforce SAML a műszaki hello-profil összekapcsolásával jogcímszolgáltató:

1. A hello `<UserJourney>` csomópont, a keresés hello `<OrchestrationStep>` rendelkező `Order="2"`.
2. Adja hozzá a következő XML hello:

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

3. Frissítés `Id` toohello ugyanaz az értékre, hogy a korábban használt `TargetClaimsExchangeId`.
4. Frissítés `TechnicalProfileReferenceId` toohello `Id` műszaki hello a profil, korábban létrehozott (például ContosoProfile).

### <a name="upload-hello-updated-extension-file"></a>Hello frissített kiterjesztésű fájl feltöltése

Elkészült módosítása hello kiterjesztésű fájlt. Mentse, majd töltse fel ezt a fájlt. Győződjön meg arról, hogy az összes érvényesítések sikeres.

### <a name="update-hello-relying-party-file"></a>Hello függő entitás fájl frissítése

Következő lépésként frissítse hello függő entitásonkénti (RP) fájlt, amely kezdeményezi hello felhasználói út létrehozott:

1. Készítsen másolatot a SignUpOrSignIn.xml a munkakönyvtárat. Nevezze át (például SignUpOrSignInWithAAD.xml).
2. Nyissa meg hello új fájl- és frissítési hello `PolicyId` az attribútum `<TrustFrameworkPolicy>` egyedi értékkel. Ez az a házirend (például SignUpOrSignInWithAAD) hello nevét.
3. Módosítsa a hello `ReferenceId` attribútumnak `<DefaultUserJourney>` toomatch hello `Id` hello új felhasználói út (például SignUpOrSignUsingContoso) létrehozott.
4. A módosítások mentéséhez, és ezután a hello-fájl feltöltése.

## <a name="test-and-troubleshoot"></a>Tesztelése és hibakeresése

tootest hello egyéni házirend imént feltöltött, az Azure-portálon hello toohello szabályzat paneljén lépjen, és kattintson **futtatása most**. Ha a hiba, lásd: [egyéni szabályzatokkal kapcsolatos problémák elhárítása](active-directory-b2c-troubleshoot-custom.md).

## <a name="next-steps"></a>Következő lépések

Visszajelzés küldése túl[AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).
