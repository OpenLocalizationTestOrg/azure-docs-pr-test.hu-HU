---
title: "Az Azure Active Directory B2C: Hozzáadása Google + identitás-szolgáltatóként OAuth2 egyéni házirendekkel"
description: "Google + identitás-szolgáltatóként OAuth2 protokollt használó mintákkal"
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
ms.openlocfilehash: 4feff21979c9c3b3b12c7a1cae4db0121d1bd79b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-add-google-as-an-oauth2-identity-provider-using-custom-policies"></a>Az Azure Active Directory B2C: Hozzáadása Google + identitás-szolgáltatóként OAuth2 egyéni házirendekkel

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Ez az útmutató bemutatja, hogyan tooenable bejelentkezés Google + fiók hello segítségével a felhasználók [egyéni házirendek](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Előfeltételek

Teljes hello szükséges lépések hello [Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md) cikk.

Ezek a lépések az alábbiak:

1.  A Google + fiók alkalmazás létrehozása.
2.  Hello Google + fiók alkalmazás fő tooAzure AD B2C hozzáadása
3.  Jogcím-szolgáltató tooa házirend hozzáadása
4.  Hello Google + fiók jogcímeket szolgáltató tooa felhasználói út regisztrálása
5.  Bérlői, és tesztelik azt hello házirend tooan az Azure AD B2C feltöltése

## <a name="create-a-google-account-application"></a>Google + fiók-alkalmazás létrehozása
toouse Google + identitás-szolgáltatóként az Azure Active Directory (Azure AD) B2C, toocreate egy Google + alapú alkalmazásba kell, és adja meg azt a hello megfelelő paraméterekkel. A Google + alkalmazás Itt regisztrálhatja: [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)

1.  Nyissa meg toohello [Google fejlesztői konzol](https://console.developers.google.com/) , és jelentkezzen be Google + fiók hitelesítő adataival.
2.  Kattintson a **Create project**, adjon meg egy **projekt neve**, és kattintson a **létrehozása**.

3.  Kattintson a hello **projektek menü**.

    ![Google + fiók – válassza ki a projekt](media/active-directory-b2c-custom-setup-goog-idp/goog-add-new-app1.png)

4.  Kattintson a hello  **+**  gombra.

    ![Google + fiók – új projekt létrehozása](media/active-directory-b2c-custom-setup-goog-idp//goog-add-new-app2.png)

5.  Adjon meg egy **projektnevet**, és kattintson a **létrehozása**.

    ![Google + fiók – új projekt](media/active-directory-b2c-custom-setup-goog-idp//goog-app-name.png)

6.  Várjon, amíg készen áll a hello projektet, majd kattintson a hello **projektek menü**.

    ![Google + fiók - Várjon, amíg az új projekt készen toouse](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app1.png)

7.  Kattintson a a projekt nevét.

    ![Google + fiók – új projekt válassza hello](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app2.png)

8.  Kattintson a **API Manager** majd **hitelesítő adatok** a bal oldali navigációs hello.
9.  Kattintson a hello **OAuth-hozzájárulás képernyő** hello felső fülre.

    ![Google + fiók - beállítása OAuth hozzájárulási képernyő](media/active-directory-b2c-custom-setup-goog-idp/goog-add-cred.png)

10.  Válassza ki vagy adjon meg érvényes **E-mail cím**, adjon meg egy **Terméknév**, és kattintson **mentése**.

    ![Google + - alkalmazás hitelesítő adatait](media/active-directory-b2c-custom-setup-goog-idp/goog-consent-screen.png)

11.  Kattintson a **új hitelesítő adatok** majd **OAuth-Ügyfélazonosító**.

    ![Google + - hozható létre új alkalmazás hitelesítő adatait](media/active-directory-b2c-custom-setup-goog-idp/goog-add-oauth2-client-id.png)

12.  A **alkalmazástípus**, jelölje be **webes alkalmazás**.

    ![Google + - alkalmazástípus kiválasztása](media/active-directory-b2c-custom-setup-goog-idp/goog-web-app.png)

13.  Adjon meg egy **neve** adja meg az alkalmazás `https://login.microsoftonline.com` a hello **JavaScript engedélyezett eredeteket** mező, és `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` a hello **jogosult átirányítási URI-azonosítók**mező. Cserélje le **{tenant}** a bérlő nevű (például contosob2c.onmicrosoft.com). Hello **{tenant}** érték kis-és nagybetűket. Kattintson a **Create** (Létrehozás) gombra.

    ![Google + - JavaScript engedélyezett eredetet adjon meg, és az átirányítási URI](media/active-directory-b2c-custom-setup-goog-idp/goog-create-client-id.png)

14.  Hello értékeit másolja **ügyfél-azonosító** és **ügyfélkulcs**. A bérlő kell mindkét tooconfigure Google + identitás-szolgáltatóként. **Titkos ügyfélkulcsot** van egy fontos biztonsági hitelesítő adatok.

    ![Google + - ügyfél-azonosító és az ügyfél titkos másolat hello értékek](media/active-directory-b2c-custom-setup-goog-idp/goog-client-secret.png)

## <a name="add-hello-google-account-application-key-tooazure-ad-b2c"></a>Hello Google + fiók alkalmazás fő tooAzure AD B2C hozzáadása
Az összevonáshoz Google + fiókok szükség van egy ügyfélkulcsot fiók tootrust Google + az Azure AD B2C hello alkalmazás nevében. A Google + alkalmazás titkos kulcs az Azure AD B2C bérlő kell toostore:  

1.  Nyissa meg tooyour az Azure AD B2C bérlő, és válassza **B2C beállítások** > **identitás élmény keretrendszer**
2.  Válassza ki **házirend kulcsok** tooview hello kulcsok, amely az Ön bérelt szolgáltatásának.
3.  Kattintson a **+ Hozzáadás**.
4.  A **beállítások**, használjon **manuális**.
5.  A **neve**, használjon `GoogleSecret`.  
    hello előtag `B2C_1A_` előfordulhat, hogy automatikusan hozzáadja.
6.  A hello **titkos** mezőbe írja be a Microsoft alkalmazás titkos kulcs a https://apps.dev.microsoft.com
7.  A **kulcshasználat**, használjon **aláírás**.
8.  Kattintson a **Create** (Létrehozás) gombra
9.  Győződjön meg arról, hogy létrehozott hello kulcs `B2C_1A_GoogleSecret`.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>A jogcím-szolgáltató hozzáadása a bővítmény házirend

Kívánt felhasználók toosign Google + fiók használatával, ha toodefine Google +-fiók szükséges, a jogcím-szolgáltató. Más szóval kell toospecify olyan végponttal, amely az Azure AD B2C-vel kommunikál. hello végpontja biztosítja, hogy egy adott felhasználó hitelesítette az Azure AD B2C tooverify által használt jogcímek egy készletét.

Google + fiók megadása egy jogcímszolgáltatótól, mint hozzáadásával `<ClaimsProvider>` csomópont a bővítmény a házirend-fájlban:

1.  A munkakönyvtár hello bővítmény házirendfájl (TrustFrameworkExtensions.xml) megnyitása. Ha egy XML-szerkesztőt kell [próbálja meg a Visual Studio Code](https://code.visualstudio.com/download), egy egyszerű platformfüggetlen-szerkesztőben.
2.  Hello található `<ClaimsProviders>` szakasz
3.  Adja hozzá a következő XML-részletet a hello hello `ClaimsProviders` elem és a név felülírandó `client_id` érték és a Google + fiók alkalmazás ügyfél-azonosító hello fájl mentése előtt.  

```xml
<ClaimsProvider>
    <Domain>google.com</Domain>
    <DisplayName>Google</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="Google-OAUTH">
        <DisplayName>Google</DisplayName>
        <Protocol Name="OAuth2" />
        <Metadata>
        <Item Key="ProviderName">google</Item>
        <Item Key="authorization_endpoint">https://accounts.google.com/o/oauth2/auth</Item>
        <Item Key="AccessTokenEndpoint">https://accounts.google.com/o/oauth2/token</Item>
        <Item Key="ClaimsEndpoint">https://www.googleapis.com/oauth2/v1/userinfo</Item>
        <Item Key="scope">email</Item>
        <Item Key="HttpBinding">POST</Item>
        <Item Key="UsePolicyInRedirectUri">0</Item>
        <Item Key="client_id">Your Google+ application ID</Item>
        </Metadata>
        <CryptographicKeys>
        <Key Id="client_secret" StorageReferenceId="B2C_1A_GoogleSecret" />
        </CryptographicKeys>
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="id" />
        <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
        <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
        <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name" />
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="google.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
        </OutputClaims>
        <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
        <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
        <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
        </OutputClaimsTransformations>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        <ErrorHandlers>
        <ErrorHandler>
            <ErrorResponseFormat>json</ErrorResponseFormat>
            <ResponseMatch>$[?(@@.error == 'invalid_grant')]</ResponseMatch>
            <Action>Reauthenticate</Action>
            <!--In case of authorization code used error, we don't want hello user tooselect his account again.-->
            <!--AdditionalRequestParameters Key="prompt">select_account</AdditionalRequestParameters-->
        </ErrorHandler>
        </ErrorHandlers>
    </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="register-hello-google-account-claims-provider-toosign-up-or-sign-in-user-journey"></a>Hello Google + fiók jogcímeket szolgáltató tooSign regisztrálásához fel, vagy jelentkezzen be a felhasználó út

hello identitásszolgáltató be van állítva.  Azonban nincs sem hello sign-Close-Up/sign-in képernyők áll rendelkezésre. Felhasználó hozzáadása a hello Google + fiók identitását szolgáltató tooyour `SignUpOrSignIn` felhasználói út. toomake akkor érhető el, egy meglévő sablon felhasználói út duplikált létrehozhatunk.  Azt adja hello Google + fiók identitásszolgáltató:

>[!NOTE]
>
>Ha a másolt hello `<UserJourneys>` elem a házirend toohello bővítményfájl (TrustFrameworkExtensions.xml) Alap fájlból, kihagyhatja toothis részt.

1.  Nyissa meg a hello alap fájlt a házirend (például TrustFrameworkBase.xml).
2.  Hello található `<UserJourneys>` elemet, és másolja hello teljes tartalma `<UserJourneys>` csomópont.
3.  Nyissa meg a hello bővítményfájl (például TrustFrameworkExtensions.xml), és megkeresi hello `<UserJourneys>` elemet. Ha hello elem nem létezik, vegyen fel egyet.
4.  Illessze be a teljes tartalma hello `<UserJournesy>` hello gyermekeként másolt csomópont `<UserJourneys>` elemet.

### <a name="display-hello-button"></a>Megjelenítési hello gomb
Hello `<ClaimsProviderSelections>` elem definiálja a jogcímeket szolgáltató tanúsítványválasztási beállítások hello listáját és sorrendjét.  `<ClaimsProviderSelection>`az elem hasonló tooan identitás szolgáltató gombjára egy sign-Close-Up/bejelentkezési oldalára. Ha ad hozzá egy `<ClaimsProviderSelection>` elem Google + fiókhoz, egy új gomb megjelenik, amikor egy felhasználó fájljai hello lapon. tooadd ezt az elemet:

1.  Hello található `<UserJourney>` tartalmazó csomópont `Id="SignUpOrSignIn"` a másolt hello felhasználói út.
2.  Keresse meg a hello `<OrchestrationStep>` tartalmazó csomópont`Order="1"`
3.  Adja hozzá a következő XML-részletet a `<ClaimsProviderSelections>` csomópont:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-hello-button-tooan-action"></a>Hivatkozás hello tooan megnyomásakor
Most, hogy a gomb helyen, toolink kell azt tooan művelet. hello művelet, ebben az esetben, akkor az Azure AD B2C toocommunicate Google + tooreceive a jogkivonatot.

1.  Hello található `<OrchestrationStep>` is tartalmazó `Order="2"` a hello `<UserJourney>` csomópont.
2.  Adja hozzá a következő XML-részletet a `<ClaimsExchanges>` csomópont:

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

>[!NOTE]
>
> * Győződjön meg arról hello `Id` hello azonos érték, amely rendelkezik `TargetClaimsExchangeId` az előző szakaszban hello
> * Győződjön meg arról `TechnicalProfileReferenceId` toohello műszaki profil korábbi (Google-OAUTH) létrehozott beállítása.

## <a name="upload-hello-policy-tooyour-tenant"></a>Hello házirend tooyour bérlői feltöltése
1.  A hello [Azure-portálon](https://portal.azure.com), hello átváltani [az Azure AD B2C-bérlő kontextusában](active-directory-b2c-navigate-to-b2c-context.md), és nyissa meg hello **az Azure AD B2C** panelen.
2.  Válassza ki **identitás élmény keretrendszer**.
3.  Nyissa meg hello **házirend** panelen.
4.  Válassza ki **házirend feltöltése**.
5.  Ellenőrizze **hello házirend felülírja, ha létezik** mezőbe.
6.  **Töltse fel** TrustFrameworkExtensions.xml, és győződjön meg arról, hogy az érvényesítése nem hiúsul meg hello

## <a name="test-hello-custom-policy-by-using-run-now"></a>Tesztelje hello egyéni házirend segítségével futtatása most
1.  Nyissa meg **az Azure AD B2C beállítások** , és túl**identitás élmény keretrendszer**.

    >[!NOTE]
    >
    >    **Futtassa most** legalább egy alkalmazás toobe preregistered hello tenant igényel. 
    >    Hogyan tooregister alkalmazások: toolearn hello Azure AD B2C [Ismerkedés](active-directory-b2c-get-started.md) cikk vagy hello [regisztrációja](active-directory-b2c-app-registration.md) cikk.


2.  Nyissa meg **B2C_1A_signup_signin**, hello feltöltött függő entitásonkénti (RP) egyéni házirendet. Válassza ki **futtatása most**.
3.  A Google + fiók használatával képes toosign kell lennie.

## <a name="optional-register-hello-google-account-claims-provider-tooprofile-edit-user-journey"></a>[Választható] Hello Google + fiók jogcímeket szolgáltató tooProfile szerkesztheti a felhasználói út regisztrálása
Érdemes lehet tooadd hello Google + fiók identitásszolgáltató is tooyour felhasználói `ProfileEdit` felhasználói út. toomake teheti, hogy ismételje meg a hello utolsó két lépést:

### <a name="display-hello-button"></a>Megjelenítési hello gomb
1.  Nyissa meg a hello kiterjesztésű fájlt (például TrustFrameworkExtensions.xml) házirend.
2.  Hello található `<UserJourney>` tartalmazó csomópont `Id="ProfileEdit"` a másolt hello felhasználói út.
3.  Keresse meg a hello `<OrchestrationStep>` tartalmazó csomópont`Order="1"`
4.  Adja hozzá a következő XML-részletet a `<ClaimsProviderSelections>` csomópont:

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-hello-button-tooan-action"></a>Hivatkozás hello tooan megnyomásakor
1.  Hello található `<OrchestrationStep>` is tartalmazó `Order="2"` a hello `<UserJourney>` csomópont.
2.  Adja hozzá a következő XML-részletet a `<ClaimsExchanges>` csomópont:

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

### <a name="test-hello-custom-profile-edit-policy-by-using-run-now"></a>Egyéni házirend hello a profil-módosítása tesztelése Futtatás most használatával

1.  Nyissa meg **az Azure AD B2C beállítások** , és túl**identitás élmény keretrendszer**.
2.  Nyissa meg **B2C_1A_ProfileEdit**, hello feltöltött függő entitásonkénti (RP) egyéni házirendet. Válassza ki **futtatása most**.
3.  A Google + fiók használatával képes toosign kell lennie.

## <a name="download-hello-complete-policy-files"></a>Hello teljes házirend fájlok letöltése
Választható lehetőség: Javasoljuk hoz létre, hogy adott esetben a saját egyéni házirend-fájlok használata a minta-fájlok helyett egyéni házirendek első lépések útmutató hello befejezése után.  [Házirend mintafájlok referencia](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-goog-app)
