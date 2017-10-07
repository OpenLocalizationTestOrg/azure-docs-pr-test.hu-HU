---
title: "Az Azure Active Directory B2C: Az Azure AD-szolgáltató felvétele egyéni házirendek használatával |} Microsoft Docs"
description: "További tudnivalók az Azure Active Directory B2C egyéni házirendek"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 31f0dfe5-1ad0-4a25-a53b-8acc71bcea72
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/04/2017
ms.author: parakhj
ms.openlocfilehash: 9b0c32086cebc171d91da2e7bfb48136723ccd4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-sign-in-by-using-azure-ad-accounts"></a>Az Azure Active Directory B2C: Azure AD-fiókok használatával írja alá

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Ez a cikk bemutatja, hogyan tooenable bejelentkezhet egy adott Azure Active Directory (Azure AD) szervezeti hello segítségével a felhasználók [egyéni házirendek](active-directory-b2c-overview-custom.md).

## <a name="prerequisites"></a>Előfeltételek

Teljes hello szükséges lépések hello [Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md) cikk.

Ezek a lépések az alábbiak:

1. Egy Azure Active Directory B2C létrehozása (Azure AD B2C) bérlő.
2. Az Azure AD B2C-alkalmazás létrehozása.
3. Két házirendmotor alkalmazás regisztrálásakor.
4. Kulcsok beállítása.
5. Alapszintű csomag hello beállítása.

## <a name="create-an-azure-ad-app"></a>Az Azure AD-alkalmazás létrehozása

tooenable bejelentkezhet egy meghatározott felhasználók számára az Azure AD szervezet tooregister hello szervezeti Azure AD bérlő belül van szüksége.

>[!NOTE]
> Vesszük "contoso.com" hello szervezeti Azure AD-bérlő és a "fabrikamb2c.onmicrosoft.com" hello Azure AD B2C bérlő hello az utasításoknak a.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
1. Hello felső sávon válassza ki a fiókját. A hello **Directory** menüben válassza ki a kívánt tooregister hello szervezeti Azure AD-bérlő az alkalmazás (contoso.com).
1. Válassza ki **további szolgáltatások** hello bal oldali ablaktáblán, és keresse meg az "App regisztráció."
1. Válassza ki **új alkalmazás regisztrációja**.
1. Adjon meg egy nevet az alkalmazáshoz (például `Azure AD B2C App`).
1. Válassza ki **Web app / API** hello alkalmazás típusra.
1. A **bejelentkezési URL-cím**, adja meg a következő URL-címe, hello ahol `yourtenant` helyébe hello az Azure AD B2C bérlő neve (`fabrikamb2c.onmicrosoft.com`):

    ```
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

1. Mentse a hello azonosítót.
1. Válassza ki az újonnan létrehozott hello alkalmazást.
1. A hello **beállítások** panelen válassza **kulcsok**.
1. Hozzon létre egy új kulcsot, és mentse. A hello a következő szakaszban található lépéseket hello még szüksége lesz rájuk.

## <a name="add-hello-azure-ad-key-tooazure-ad-b2c"></a>Az Azure AD hello kulcs tooAzure AD B2C hozzáadása

Az Azure AD B2C-bérlő toostore hello contoso.com alkalmazás kulcs szükséges. toodo ezt:
1. Nyissa meg tooyour az Azure AD B2C bérlő, és válassza **B2C beállítások** > **identitás élmény keretrendszer** > **házirend kulcsok**.
1. Válassza ki **+ Hozzáadás**.
1. Válassza ki vagy adja meg ezeket a beállításokat:
   * Válassza ki **manuális**.
   * A **neve**, válasszon egy nevet, amely megfelel az Azure AD-bérlő nevét (például `ContosoAppSecret`).  hello előtag `B2C_1A_` a rendszer automatikusan hozzáadja a kulcs toohello neve.
   * Illessze be az alkalmazás kulcs hello **titkos** mezőbe.
   * Válassza ki **aláírás**.
1. Kattintson a **Létrehozás** gombra.
1. Győződjön meg arról, hogy létrehozott hello kulcs `B2C_1A_ContosoAppSecret`.


## <a name="add-a-claims-provider-in-your-base-policy"></a>A jogcím-szolgáltató hozzáadása a kiinduló házirend

Ha azt szeretné, felhasználók toosign Azure AD használatával, egy jogcímszolgáltatótól, az Azure AD toodefine kell. Más szóval toospecify olyan végponttal, amely az Azure AD B2C is kommunikálni fognak van szüksége. hello végpont biztosítja, hogy egy adott felhasználó hitelesítette az Azure AD B2C tooverify által használt jogcímek egy készletét. 

Meghatározhatja az Azure AD egy jogcímszolgáltatótól, az Azure AD toohello hozzáadásával `<ClaimsProvider>` hello bővítményfájl házirend csomópontja:

1. A munkakönyvtár hello bővítményfájl (TrustFrameworkExtensions.xml) megnyitása.
1. Hello található `<ClaimsProviders>` szakasz. Ha még nem létezik, adja hozzá hello legfelső szintű csomópontja alatt.
1. Adjon hozzá egy új `<ClaimsProvider>` csomópont az alábbiak szerint:

    ```XML
    <ClaimsProvider>
        <Domain>Contoso</Domain>
        <DisplayName>Login using Contoso</DisplayName>
        <TechnicalProfiles>
            <TechnicalProfile Id="ContosoProfile">
                <DisplayName>Contoso Employee</DisplayName>
                <Description>Login with your Contoso account</Description>
                <Protocol Name="OpenIdConnect"/>
                <OutputTokenFormat>JWT</OutputTokenFormat>
                <Metadata>
                    <Item Key="METADATA">https://login.windows.net/contoso.com/.well-known/openid-configuration</Item>
                    <Item Key="ProviderName">https://sts.windows.net/00000000-0000-0000-0000-000000000000/</Item>
                    <Item Key="client_id">00000000-0000-0000-0000-000000000000</Item>
                    <Item Key="IdTokenAudience">00000000-0000-0000-0000-000000000000</Item>
                    <Item Key="response_types">id_token</Item>
                    <Item Key="UsePolicyInRedirectUri">false</Item>
                </Metadata>
                <CryptographicKeys>
                    <Key Id="client_secret" StorageReferenceId="B2C_1A_ContosoAppSecret"/>
                </CryptographicKeys>
                <OutputClaims>
                    <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="oid"/>
                    <OutputClaim ClaimTypeReferenceId="tenantId" PartnerClaimType="tid"/>
                    <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
                    <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
                    <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
                    <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="contosoAuthentication" />
                    <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="AzureADContoso" />
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

1. A hello `<ClaimsProvider>` csomópont, a frissítés hello értéke `<Domain>` tooa egyedi érték, amely lehet használt toodistinguish az egyéb identitás-szolgáltatóktól származó.
1. A hello `<ClaimsProvider>` csomópont, a frissítés hello értéke `<DisplayName>` tooa rövid nevet a hello jogcím-szolgáltató. Ez az érték nincs használatban.

### <a name="update-hello-technical-profile"></a>Műszaki hello-profil frissítése

tooget hello Azure AD-végpont származó jogkivonat, toodefine hello protokollok, hogy az Azure AD B2C az Azure ad-val toocommunicate kell használnia kell. Ez történik, belül hello `<TechnicalProfile>` eleme `<ClaimsProvider>`.
1. Hello hello azonosítója frissítése `<TechnicalProfile>` csomópont. Ez az azonosító nem használt toorefer toothis műszaki profil hello házirend egyéb részeitől.
1. Frissítse a hello értéket `<DisplayName>`. Ez az érték a hello bejelentkezési gombra a bejelentkezési képernyőn jelenik meg.
1. Frissítse a hello értéket `<Description>`.
1. Az Azure AD hello OpenID Connect protokollt használja, ezért figyeljen oda arra, hogy hello értéke `<Protocol>` van `"OpenIdConnect"`.

Tooupdate hello kell `<Metadata>` hello XML-fájl a szakaszban említett tooearlier tooreflect hello konfigurációs beállításokat az adott Azure AD-bérlő. Hello XML-fájl, a frissítés hello metaadat az alábbiak szerint:

1. Állítsa be `<Item Key="METADATA">` túl`https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, ahol `yourAzureADtenant` a Azure AD-bérlő neve (contoso.com).
1. Nyissa meg a böngészőt, majd a toohello `METADATA` frissített URL-címet.
1. Hello böngészőben hello "kiállító" objektum keresse meg, és másolja az értékét. Az alábbihoz hasonló hello: `https://sts.windows.net/{tenantId}/`.
1. Illessze be a hello értéke `<Item Key="ProviderName">` hello XML-fájlban.
1. Állítsa be `<Item Key="client_id">` toohello Alkalmazásazonosítót a hello alkalmazás regisztrációja.
1. Állítsa be `<Item Key="IdTokenAudience">` toohello Alkalmazásazonosítót a hello alkalmazás regisztrációja.
1. Győződjön meg arról, hogy `<Item Key="response_types">` értéke túl`id_token`.
1. Győződjön meg arról, hogy `<Item Key="UsePolicyInRedirectUri">` értéke túl`false`.

Emellett szükség van az Azure AD B2C bérlő toohello az Azure AD a regisztrált toolink hello Azure AD titkos `<ClaimsProvider>` információkat:

* A hello `<CryptographicKeys>` XML-fájl megelőző hello területen hello értéke frissítése `StorageReferenceId` toohello hivatkozási azonosítója, amelyet megadott hello titok (például `ContosoAppSecret`).

### <a name="upload-hello-extension-file-for-verification"></a>Az ellenőrzéshez hello kiterjesztésű fájl feltöltése

Mostanra, konfigurálta a házirendet, hogy az Azure AD B2C tudja, hogyan toocommunicate az Azure AD-címtárát. Próbálja meg a házirend csak tooconfirm, hogy nincs probléma merül fel, amennyiben a hello kiterjesztésű fájl feltöltése. toodo így:

1. Nyissa meg hello **házirend** panel az Azure AD B2C-bérlőben.
1. Ellenőrizze a hello **hello házirend felülírja, ha létezik** mezőbe.
1. Töltse fel hello kiterjesztésű fájlt (TrustFrameworkExtensions.xml), és győződjön meg arról, hogy az érvényesítése nem hiúsul meg hello.

## <a name="register-hello-azure-ad-claims-provider-tooa-user-journey"></a>Hello Azure AD jogcímeket szolgáltató tooa felhasználói út regisztrálása

Most kell tooadd hello Azure AD identity provider tooone, a felhasználó utak. Ezen a ponton hello identitásszolgáltató be van állítva, de hello sign-Close-Up/sign-in képernyők egyik nem érhető el. toomake akkor érhető el, rendszer létrehoz egy meglévő sablon felhasználói út duplikált, és módosíthatja azt, hogy az identitásszolgáltató hello Azure AD is rendelkezik:

1. Nyissa meg a hello alap fájlt a házirend (például TrustFrameworkBase.xml).
1. Hello található `<UserJourneys>` teljes elem és a példány hello `<UserJourney>` tartalmazó csomópont `Id="SignUpOrSignIn"`.
1. Nyissa meg a hello bővítményfájl (például TrustFrameworkExtensions.xml), és megkeresi hello `<UserJourneys>` elemet. Ha hello elem nem létezik, vegyen fel egyet.
1. Beillesztés hello teljes `<UserJourney>` hello gyermekeként másolt csomópont `<UserJourneys>` elemet.
1. Hello azonosító hello új felhasználói út átnevezése (például a módon nevezze át `SignUpOrSignUsingContoso`).

### <a name="display-hello-button"></a>Megjelenítési hello "gombra."

Hello `<ClaimsProviderSelection>` elem hasonló tooan identity provider gombjára a sign-Close-Up/bejelentkezési képernyő. Ha ad hozzá egy `<ClaimsProviderSelection>` elem az Azure AD, egy új gombon, amikor egy felhasználó fájljai hello lapon. tooadd ezt az elemet:

1. Hello található `<OrchestrationStep>` tartalmazó csomópont `Order="1"` az imént létrehozott hello felhasználói út.
1. Adja hozzá a hello következő:

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

1. Állítsa be `TargetClaimsExchangeId` tooan megfelelő értékre. Javasoljuk a következő hello ugyanaz, mint mások egyezmény:  *\[ClaimProviderName\]Exchange*.

### <a name="link-hello-button-tooan-action"></a>Hivatkozás hello tooan megnyomásakor

Most, hogy a gomb helyen, toolink kell azt tooan művelet. hello művelet, ebben az esetben, akkor az Azure AD B2C toocommunicate az Azure AD tooreceive jogkivonatot. Hivatkozás hello tooan megnyomásakor létrehozhatja, ha az Azure AD jogcímeket szolgáltató műszaki hello-profil:

1. Hello található `<OrchestrationStep>` is tartalmazó `Order="2"` a hello `<UserJourney>` csomópont.
1. Adja hozzá a hello következő:

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

1. Frissítés `Id` azonos érték, amely toohello `TargetClaimsExchangeId` az előző szakaszban hello.
1. Frissítés `TechnicalProfileReferenceId` műszaki hello toohello azonosítója profilját, korábban létrehozott (ContosoProfile).

### <a name="upload-hello-updated-extension-file"></a>Hello frissített kiterjesztésű fájl feltöltése

Elkészült módosítása hello kiterjesztésű fájlt. Mentse a fájlt. Majd feltölteni hello fájlt, és győződjön meg arról, hogy az összes érvényesítések sikeres.

### <a name="update-hello-rp-file"></a>Hello RP fájl frissítése

Most kell tooupdate hello függő entitásonkénti (RP) fájl, amely fogja elindítani az imént létrehozott hello felhasználói út:

1. Készítsen másolatot a SignUpOrSignIn.xml a munkakönyvtárat, és adjon neki (például nevezni tooSignUpOrSignInWithAAD.xml).
1. Nyissa meg hello új fájl- és frissítési hello `PolicyId` az attribútum `<TrustFrameworkPolicy>` egyedi értékkel (például SignUpOrSignInWithAAD). <br> Ez lesz a házirend hello nevét (például B2C\_1A\_SignUpOrSignInWithAAD).
1. Módosítsa a hello `ReferenceId` attribútumnak `<DefaultUserJourney>` toomatch hello azonosító hello (SignUpOrSignUsingContoso) létrehozott új felhasználói út.
1. A módosítások mentéséhez, és hello-fájl feltöltése.

## <a name="troubleshooting"></a>Hibaelhárítás

A panel megnyitásával, majd feltöltött hello egyéni házirend tesztelése **futtatása most**. olvassa el toodiagnose problémák [hibaelhárítási](active-directory-b2c-troubleshoot-custom.md).

## <a name="next-steps"></a>Következő lépések

Visszajelzés küldése túl[AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).
