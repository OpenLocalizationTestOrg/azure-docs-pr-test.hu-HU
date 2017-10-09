---
title: "Az Azure Active Directory B2C: Egyéni házirendekkel identitás-szolgáltatóként hozzáadása a Microsoft-fiók (MSA)"
description: "A Microsoft identitás-szolgáltatóként OpenID Connect (OIDC) protokollt használó minta"
services: active-directory-b2c
documentationcenter: 
author: yoelhor
manager: joroja
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: yoelh
ms.openlocfilehash: 577ac612f69015e6790f2fa9f2cfb42c2b524b55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-add-microsoft-account-msa-as-an-identity-provider-using-custom-policies"></a>Az Azure Active Directory B2C: Egyéni házirendekkel identitás-szolgáltatóként hozzáadása a Microsoft-fiók (MSA)

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

A cikkből megtudhatja, hogyan tooenable bejelentkezhet a felhasználók számára a Microsoft-fiókból (MSA) használata hello [egyéni házirendek](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Előfeltételek
Teljes hello szükséges lépések hello [Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md) cikk.

Ezek a lépések az alábbiak:

1.  A Microsoft-fiók alkalmazás létrehozása.
2.  Hello Microsoft fiók alkalmazás fő tooAzure AD B2C hozzáadása
3.  Jogcím-szolgáltató tooa házirend hozzáadása
4.  Hello Microsoft Account jogcímeket szolgáltató tooa felhasználói út regisztrálása
5.  Bérlői, és tesztelik azt hello házirend tooan az Azure AD B2C feltöltése

## <a name="create-a-microsoft-account-application"></a>Microsoft-fiók alkalmazás létrehozása
toouse Microsoft fiók az Azure Active Directory (Azure AD) B2C identitás-szolgáltatóként, kell toocreate egy Microsoft-fiók alkalmazást, és adja meg azt a hello megfelelő paraméterekkel. Microsoft-fiók szükséges. Ha még nincs fiókja, látogasson el [https://www.live.com/](https://www.live.com/).

1.  Nyissa meg toohello [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) , és jelentkezzen be Microsoft-fiók hitelesítő adataival.
2.  Kattintson a **hozzáadhat egy alkalmazást**.

    ![Microsoft fiók - alkalmazás hozzáadása](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-new-app.png)

3.  Adja meg egy **neve** az alkalmazás **lépjen kapcsolatba az e-mailek**, törölje a jelet **tudassa velünk megkezdésében súgó** kattintson **létrehozása**.

    ![Microsoft-fiók – az alkalmazás regisztrálása](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-name.png)

4.  Másolja a hello értékének **alkalmazásazonosító**. Esetleg szükség lenne rá tooconfigure Microsoft-fiók identitás-szolgáltatóként az Ön bérelt szolgáltatásának.

    ![Microsoft-fiók - alkalmazásazonosító hello értékének másolása](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-id.png)

5.  Kattintson a **Hozzáadás platform**

    ![Microsoft fiók - platform hozzáadása](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-platform.png)

6.  Hello platform listából válassza ki a **webes**.

    ![Microsoft-fiók – hello platform listából válassza a webes](media/active-directory-b2c-custom-setup-ms-account-idp/msa-web.png)

7.  Adja meg `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` a hello **átirányítási URI-azonosítók** mező. Cserélje le **{tenant}** a bérlő nevű (például contosob2c.onmicrosoft.com).

    ![Microsoft-fiók - készlet átirányítási URL-címek](media/active-directory-b2c-custom-setup-ms-account-idp/msa-redirect-url.png)

8.  Kattintson a **új jelszó létrehozása** alatt hello **alkalmazás titkos kulcsok** szakasz. Másolja a hello új jelszót a képernyőn jelenik meg. Esetleg szükség lenne rá tooconfigure Microsoft-fiók identitás-szolgáltatóként az Ön bérelt szolgáltatásának. Ez a jelszó nem egy fontos biztonsági hitelesítő adatok.

    ![Microsoft fiók – új jelszó létrehozása](media/active-directory-b2c-custom-setup-ms-account-idp/msa-generate-new-password.png)

    ![Microsoft-fiók – hello új jelszó másolása](media/active-directory-b2c-custom-setup-ms-account-idp/msa-new-password.png)

9.  Hello jelölőnégyzetet, amely szerint **Live SDK támogatási** alatt hello **speciális beállítások** szakasz. Kattintson a **Save** (Mentés) gombra.

    ![Microsoft-fiók - Live SDK-támogatás](media/active-directory-b2c-custom-setup-ms-account-idp/msa-live-sdk-support.png)

## <a name="add-hello-microsoft-account-application-key-tooazure-ad-b2c"></a>Hello Microsoft fiók alkalmazás fő tooAzure AD B2C hozzáadása
Az összevonáshoz Microsoft-fiókkal rendelkező szükség van egy ügyfélkulcsot a Microsoft-fiók tootrust az Azure AD B2C hello alkalmazás nevében. A Microsoft-fiók alkalmazás az Azure AD B2C bérlő secert kell toostore:   

1.  Nyissa meg tooyour az Azure AD B2C bérlő, és válassza **B2C beállítások** > **identitás élmény keretrendszer**
2.  Válassza ki **házirend kulcsok** tooview hello kulcsok, amely az Ön bérelt szolgáltatásának.
3.  Kattintson a **+ Hozzáadás**.
4.  A **beállítások**, használjon **manuális**.
5.  A **neve**, használjon `MSASecret`.  
    hello előtag `B2C_1A_` előfordulhat, hogy automatikusan hozzáadja.
6.  A hello **titkos** mezőbe írja be a Microsoft alkalmazás titkos kulcs a https://apps.dev.microsoft.com
7.  A **kulcshasználat**, használjon **aláírás**.
8.  Kattintson a **Create** (Létrehozás) gombra
9.  Győződjön meg arról, hogy létrehozott hello kulcs `B2C_1A_MSASecret`.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>A jogcím-szolgáltató hozzáadása a bővítmény házirend
Kívánt felhasználók toosign Microsoft Account, ha szüksége toodefine Microsoft Account jogcím-szolgáltatóként. Más szóval kell toospecify olyan végponttal, amely az Azure AD B2C-vel kommunikál. hello végpontja biztosítja, hogy egy adott felhasználó hitelesítette az Azure AD B2C tooverify által használt jogcímek egy készletét.

Microsoft Account meghatározni, a Jogcímszolgáltatók hozzáadásával `<ClaimsProvider>` csomópont a bővítmény a házirend-fájlban:

1.  A munkakönyvtár hello bővítmény házirendfájl (TrustFrameworkExtensions.xml) megnyitása. Ha egy XML-szerkesztőt kell [próbálja meg a Visual Studio Code](https://code.visualstudio.com/download), egy egyszerű platformfüggetlen-szerkesztőben.
2.  Hello található `<ClaimsProviders>` szakasz
3.  Adja hozzá a következő XML-részletet a hello `ClaimsProviders` elem:

    ```xml
<ClaimsProvider>
    <Domain>live.com</Domain>
    <DisplayName>Microsoft Account</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="MSA-OIDC">
        <DisplayName>Microsoft Account</DisplayName>
        <Protocol Name="OpenIdConnect" />
        <Metadata>
        <Item Key="ProviderName">https://login.live.com</Item>
        <Item Key="METADATA">https://login.live.com/.well-known/openid-configuration</Item>
        <Item Key="response_types">code</Item>
        <Item Key="response_mode">form_post</Item>
        <Item Key="scope">openid profile email</Item>
        <Item Key="HttpBinding">POST</Item>
        <Item Key="UsePolicyInRedirectUri">0</Item>
        <Item Key="client_id">Your Microsoft application client id</Item>
        </Metadata>
    <CryptographicKeys>
        <Key Id="client_secret" StorageReferenceId="B2C_1A_MSASecret" />
    </CryptographicKeys>
    <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="live.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="sub" />
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
        <OutputClaim ClaimTypeReferenceId="email" />
        </OutputClaims>
        <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
        <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
        <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
        </OutputClaimsTransformations>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
    </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

4.  Cserélje le `client_id` értékének a Microsoft Account alkalmazás ügyfél-azonosítóját

5.  Hello fájl mentéséhez.

## <a name="register-hello-microsoft-account-claims-provider-toosign-up-or-sign-in-user-journey"></a>Hello Microsoft Account jogcímeket szolgáltató tooSign mentése vagy a bejelentkezési felhasználói út regisztrálása

Ezen a ponton hello identitásszolgáltató be van állítva, de hello sign-Close-Up/sign-in képernyők egyik nem érhető el. Most tooadd hello Microsoft Account identity provider tooyour felhasználói `SignUpOrSignIn` felhasználói út. toomake akkor érhető el, egy meglévő sablon felhasználói út duplikált létrehozhatunk.  Azt adja hello Microsoft Account identitásszolgáltató:

> [!NOTE]
>
>Ha korábban kimásolt hello `<UserJourneys>` elem a házirend toohello kiterjesztésű fájl alap fájlból `TrustFrameworkExtensions.xml`, kihagyhatja toothis részt.

1.  Nyissa meg a hello alap fájlt a házirend (például TrustFrameworkBase.xml).
2.  Hello található `<UserJourneys>` elemet, és másolja hello teljes tartalma `<UserJourneys>` csomópont.
3.  Nyissa meg a hello bővítményfájl (például TrustFrameworkExtensions.xml), és megkeresi hello `<UserJourneys>` elemet. Ha hello elem nem létezik, vegyen fel egyet.
4.  Illessze be a teljes tartalma hello `<UserJournesy>` hello gyermekeként másolt csomópont `<UserJourneys>` elemet.

### <a name="display-hello-button"></a>Megjelenítési hello gomb
Hello `<ClaimsProviderSelections>` elem definiálja a jogcímeket szolgáltató tanúsítványválasztási beállítások hello listáját és sorrendjét.  `<ClaimsProviderSelection>`az elem hasonló tooan identitás szolgáltató gombjára egy sign-Close-Up/bejelentkezési oldalára. Ha ad hozzá egy `<ClaimsProviderSelection>` elem Microsoft-fiókot, egy új gomb megjelenik, amikor egy felhasználó fájljai hello oldalon. tooadd ezt az elemet:

1.  Hello található `<UserJourney>` tartalmazó csomópont `Id="SignUpOrSignIn"` a másolt hello felhasználói út.
2.  Keresse meg a hello `<OrchestrationStep>` tartalmazó csomópont`Order="1"`
3.  Adja hozzá a következő XML-részletet a `<ClaimsProviderSelections>` csomópont:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-hello-button-tooan-action"></a>Hivatkozás hello tooan megnyomásakor
Most, hogy a gomb helyen, toolink kell azt tooan művelet. hello művelet, ebben az esetben, akkor az Azure AD B2C toocommunicate Microsoft Account tooreceive a jogkivonatot. Hivatkozás hello tooan megnyomásakor létrehozhatja, ha a Microsoft Account jogcímeket szolgáltató műszaki hello-profil:

1.  Hello található `<OrchestrationStep>` is tartalmazó `Order="2"` a hello `<UserJourney>` csomópont.
2.  Adja hozzá a következő XML-részletet a `<ClaimsExchanges>` csomópont:

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

> [!NOTE]
>
>   * Győződjön meg arról hello `Id` hello azonos érték, amely rendelkezik `TargetClaimsExchangeId` az előző szakaszban hello
>   * Győződjön meg arról `TechnicalProfileReferenceId` toohello műszaki profil korábbi (MSA-OIDC) létrehozott beállítása.

## <a name="upload-hello-policy-tooyour-tenant"></a>Hello házirend tooyour bérlői feltöltése
1.  A hello [Azure-portálon](https://portal.azure.com), hello átváltani [az Azure AD B2C-bérlő kontextusában](active-directory-b2c-navigate-to-b2c-context.md), és nyissa meg hello **az Azure AD B2C** panelen.
2.  Válassza ki **identitás élmény keretrendszer**.
3.  Nyissa meg hello **házirend** panelen.
4.  Válassza ki **házirend feltöltése**.
5.  Ellenőrizze **hello házirend felülírja, ha létezik** mezőbe.
6.  **Töltse fel** TrustFrameworkExtensions.xml, és győződjön meg arról, hogy az érvényesítése nem hiúsul meg hello

## <a name="test-hello-custom-policy-by-using-run-now"></a>Tesztelje hello egyéni házirend segítségével futtatása most

1.  Nyissa meg **az Azure AD B2C beállítások** , és túl**identitás élmény keretrendszer**.
> [!NOTE]
>
>**Futtassa most** legalább egy alkalmazás toobe preregistered hello tenant igényel. Hogyan tooregister alkalmazások: toolearn hello Azure AD B2C [Ismerkedés](active-directory-b2c-get-started.md) cikk vagy hello [regisztrációja](active-directory-b2c-app-registration.md) cikk.
2.  Nyissa meg **B2C_1A_signup_signin**, hello feltöltött függő entitásonkénti (RP) egyéni házirendet. Válassza ki **futtatása most**.
3.  A Microsoft-fiók használatával képes toosign kell lennie.

## <a name="optional-register-hello-microsoft-account-claims-provider-tooprofile-edit-user-journey"></a>[Választható] Hello Microsoft Account jogcímeket szolgáltató tooProfile szerkesztheti a felhasználói út regisztrálása
Érdemes lehet tooadd hello Microsoft Account identitásszolgáltató is tooyour felhasználói `ProfileEdit` felhasználói út. toomake teheti, hogy ismételje meg a hello utolsó két lépést:

### <a name="display-hello-button"></a>Megjelenítési hello gomb
1.  Nyissa meg a hello kiterjesztésű fájlt (például TrustFrameworkExtensions.xml) házirend.
2.  Hello található `<UserJourney>` tartalmazó csomópont `Id="ProfileEdit"` a másolt hello felhasználói út.
3.  Keresse meg a hello `<OrchestrationStep>` tartalmazó csomópont`Order="1"`
4.  Adja hozzá a következő XML-részletet a `<ClaimsProviderSelections>` csomópont:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-hello-button-tooan-action"></a>Hivatkozás hello tooan megnyomásakor
1.  Hello található `<OrchestrationStep>` is tartalmazó `Order="2"` a hello `<UserJourney>` csomópont.
2.  Adja hozzá a következő XML-részletet a `<ClaimsExchanges>` csomópont:

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

### <a name="test-hello-custom-profile-edit-policy-by-using-run-now"></a>Egyéni házirend hello a profil-módosítása tesztelése Futtatás most használatával
1.  Nyissa meg **az Azure AD B2C beállítások** , és túl**identitás élmény keretrendszer**.
2.  Nyissa meg **B2C_1A_ProfileEdit**, hello feltöltött függő entitásonkénti (RP) egyéni házirendet. Válassza ki **futtatása most**.
3.  A Microsoft-fiók használatával képes toosign kell lennie.

## <a name="download-hello-complete-policy-files"></a>Hello teljes házirend fájlok letöltése
Választható lehetőség: Javasoljuk hoz létre, hogy adott esetben a saját egyéni házirend-fájlok használata a minta-fájlok helyett egyéni házirendek első lépések útmutató hello befejezése után.  [Házirend mintafájlok referencia](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-msa-app)
