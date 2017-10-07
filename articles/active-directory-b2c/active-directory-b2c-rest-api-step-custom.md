---
title: "Az Azure Active Directory B2C: REST API jogcím cseréje egy vezénylési lépés |} Microsoft Docs"
description: "Témakör: Azure Active Directory B2C egyéni házirendek, amelyek az API-k integrálása"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/24/2017
ms.author: joroja
ms.openlocfilehash: 90a495029f48d70232ef3f99de4ea4d351395aa7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-an-orchestration-step"></a>Forgatókönyv: Integrálása az Azure AD B2C felhasználói út a REST API jogcímek cseréjét egy vezénylési lépés

hello identitás élmény keretrendszer (IEF) Azure Active Directory B2C alapjául szolgáló (az Azure AD B2C) lehetővé teszi, hogy a hello identitás fejlesztői toointegrate tevékenységet egy felhasználó út RESTful API-t.  

Ez az útmutató végén hello fogja tudni toocreate az Azure AD B2C felhasználói út, amely együttműködik a RESTful-szolgáltatásokat.

hello IEF adatok jogcímekben adatokat küld és fogad vissza a jogcímeket. REST API jogcímek exchange hello:

- Egy vezénylési lépés tervezhető.
- Egy külső műveletet is elindíthatja. Például a külső adatbázis azt is naplózhat egy eseményt.
- Is lehet használt toofetch értékű, és majd hello felhasználói adatbázisban tárolja.

Hello kapott jogcímek használata újabb toochange hello folyamat végrehajtása.

Hello interakció érvényesítési profil is tervezhet. További információkért lásd: [forgatókönyv: integrálja a REST API-t cseréjét használják az Azure AD B2C felhasználói út a jogcímeket, a felhasználói bevitel ellenőrzése](active-directory-b2c-rest-api-validation-custom.md).

hello például az is, hogy amikor egy felhasználó egy profil szerkesztése végez, szeretnénk:

1. Hello felhasználói kereshet meg egy külső rendszerben.
2. Ha regisztrálva van-e az adott felhasználó hello város beolvasása.
3. Az attribútum toohello alkalmazást jogcímként visszaadása.

## <a name="prerequisites"></a>Előfeltételek

- Az Azure AD B2C bérlő konfigurált toocomplete sign-Close-Up/bejelentkezés, a helyi fiók [bevezetés](active-directory-b2c-get-started-custom.md).
- A REST API végpont toointeract együtt. Ez az útmutató egy egyszerű Azure függvény app webhook használja példaként.
- *Ajánlott*: teljes hello [REST API-jogcímek exchange forgatókönyv érvényesítési lépésként](active-directory-b2c-rest-api-validation-custom.md).

## <a name="step-1-prepare-hello-rest-api-function"></a>1. lépés: Felkészülés hello REST API-függvénye

> [!NOTE]
> A REST API-függvények telepítése Ez a cikk hello hatókörén kívül esik. [Az Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) biztosít egy kiváló eszközkészlet toocreate RESTful-szolgáltatásokat hello felhőben.

Beállítjuk van egy Azure függvény, amely megkapja a jogcím nevű `email`, majd vissza hello jogcím és `city` hozzárendelt hello értékű `Redmond`. hello minta Azure függvény megtalálható [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).

Hello `userMessage` jogcímet, amely hello Azure függvény által visszaadott nem kötelező ebben a környezetben, és hello IEF figyelmen kívül hagyja azt. Potenciálisan használhatja az üzenet átadott toohello alkalmazás, és később bemutatott toohello felhasználó.

```csharp
if (requestContentAsJObject.email == null)
{
    return request.CreateResponse(HttpStatusCode.BadRequest);
}

var email = ((string) requestContentAsJObject.email).ToLower();

return request.CreateResponse<ResponseContent>(
    HttpStatusCode.OK,
    new ResponseContent
    {
        version = "1.0.0",
        status = (int) HttpStatusCode.OK,
        userMessage = "User Found",
        city = "Redmond"
    },
    new JsonMediaTypeFormatter(),
    "application/json");
```

Egy Azure függvény alkalmazás könnyen tooget hello függvény URL-címe, hello azonosítóval hello adott függvény segítségével. Ebben az esetben hello URL-cím van: https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==. Tesztelési használhatja.

## <a name="step-2-configure-hello-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworextensionsxml-file"></a>2. lépés: Hello RESTful API jogcímek az exchange konfigurálása a TrustFrameworExtensions.xml fájlban műszaki profil

Egy technikai profil beállítás hello teljes hello Exchange hello RESTful szolgáltatás a szükséges. Nyissa meg hello TrustFrameworkExtensions.xml fájlt, és adja hozzá a következő XML-részletet belül hello hello `<ClaimsProvider>` elemet.

> [!NOTE]
> A következő XML-RESTful szolgáltató hello `Version=1.0.0.0` hello protokollként leírása. Tekinti azokat hello függvény hello külső szolgáltatással együtt fog működni. <!-- TODO: A full definition of hello schema can be found...link tooRESTful Provider schema definition>-->

```XML
<ClaimsProvider>
    <DisplayName>REST APIs</DisplayName>
    <TechnicalProfiles>
        <TechnicalProfile Id="AzureFunctions-LookUpLoyaltyWebHook">
            <DisplayName>Check LookUpLoyalty Web Hook Azure Function</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
                <Item Key="ServiceUrl">https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==</Item>
                <Item Key="AuthenticationType">None</Item>
                <Item Key="SendClaimsIn">Body</Item>
            </Metadata>
            <InputClaims>
                <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="email" />
            </InputClaims>
            <OutputClaims>
                <OutputClaim ClaimTypeReferenceId="city" PartnerClaimType="city" />
            </OutputClaims>
            <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

Hello `<InputClaims>` elem definiálja hello küldi hello IEF toohello REST-szolgáltatást a jogcímeket. Ebben a példában hello hello jogcím tartalmát `givenName` toohello REST-szolgáltatást küldi hello jogcímként `email`.  

Hello `<OutputClaims>` elem definiálja hello jogcímeket, hogy hello IEF fog várható hello REST-szolgáltatást. Kapott jogcímek hello száma, függetlenül hello IEF használja csak azonosított itt. Ebben a példában a jogcím érkezett `city` csatlakoztatott tooan IEF jogcím fogja meghívni `city`.

## <a name="step-3-add-hello-new-claim-city-toohello-schema-of-your-trustframeworkextensionsxml-file"></a>3. lépés: Új jogcímet hello hozzáadása `city` toohello séma a TrustFrameworkExtensions.xml fájl

hello jogcím `city` nincs még definiálva a sémában. Igen, hozzáadása hello elemen belül `<BuildingBlocks>`. Ez az elem hello TrustFrameworkExtensions.xml fájl hello elején található.

```XML
<BuildingBlocks>
    <!--hello claimtype city must be added toohello TrustFrameworkPolicy-->
    <!-- You can add new claims in hello BASE file Section III, or in hello extensions file-->
    <ClaimsSchema>
        <ClaimType Id="city">
            <DisplayName>City</DisplayName>
            <DataType>string</DataType>
            <UserHelpText>Your city</UserHelpText>
            <UserInputType>TextBox</UserInputType>
        </ClaimType>
    </ClaimsSchema>
</BuildingBlocks>
```

## <a name="step-4-include-hello-rest-service-claims-exchange-as-an-orchestration-step-in-your-profile-edit-user-journey-in-trustframeworkextensionsxml"></a>4. lépés: Hello REST szolgáltatási igények exchange közé az orchestration lépése a profil szerkesztése felhasználói megtett út TrustFrameworkExtensions.xml

Adjon hozzá egy lépést toohello profil szerkesztése felhasználói út után hello felhasználó hitelesített (vezénylési lépésekből 1-4 a következő XML hello), és hello felhasználó már rendelkezik frissített hello-profil adatait (5. lépés).

> [!NOTE]
> Nincsenek hello REST API-hívás helyének egy vezénylési lépés számos használhatók. Egy vezénylési lépés azt követően a felhasználó sikeresen befejezte egy feladat, például az első regisztráció frissítés tooan külső rendszerekből használható vagy profil frissítése tookeep adatok szinkronizálása. Ebben az esetben használt tooaugment hello információk toohello alkalmazás hello-profil szerkesztése után.

Másolás hello-profil szerkesztése felhasználói út XML-kódot fájlból hello TrustFrameworkBase.xml fájl tooyour TrustFrameworkExtensions.xml belül hello `<UserJourneys>` elemet. Végezze el a 6. lépés hello módosítását.

```XML
<OrchestrationStep Order="6" Type="ClaimsExchange">
    <ClaimsExchanges>
        <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-LookUpLoyaltyWebHook" />
    </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> Ha hello sorrendje eltér a verziót, győződjön meg arról, hogy hello lépése előtt hello hello kód beszúrása `ClaimsExchange` típus `SendClaims`.

hello hello felhasználói útra végső XML kell kinéznie:

```XML
<UserJourney Id="ProfileEdit">
    <OrchestrationSteps>
        <OrchestrationStep Order="1" Type="ClaimsProviderSelection" ContentDefinitionReferenceId="api.idpselections">
            <ClaimsProviderSelections>
                <ClaimsProviderSelection TargetClaimsExchangeId="FacebookExchange" />
                <ClaimsProviderSelection TargetClaimsExchangeId="LocalAccountSigninEmailExchange" />
            </ClaimsProviderSelections>
        </OrchestrationStep>
        <OrchestrationStep Order="2" Type="ClaimsExchange">
            <ClaimsExchanges>
                <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
                <ClaimsExchange Id="LocalAccountSigninEmailExchange" TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="3" Type="ClaimsExchange">
            <Preconditions>
                <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
                    <Value>authenticationSource</Value>
                    <Value>localAccountAuthentication</Value>
                    <Action>SkipThisOrchestrationStep</Action>
                </Precondition>
            </Preconditions>
            <ClaimsExchanges>
                <ClaimsExchange Id="AADUserRead" TechnicalProfileReferenceId="AAD-UserReadUsingAlternativeSecurityId" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="4" Type="ClaimsExchange">
            <Preconditions>
                <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
                    <Value>authenticationSource</Value>
                    <Value>socialIdpAuthentication</Value>
                    <Action>SkipThisOrchestrationStep</Action>
                </Precondition>
            </Preconditions>
            <ClaimsExchanges>
                <ClaimsExchange Id="AADUserReadWithObjectId" TechnicalProfileReferenceId="AAD-UserReadUsingObjectId" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="5" Type="ClaimsExchange">
            <ClaimsExchanges>
                <ClaimsExchange Id="B2CUserProfileUpdateExchange" TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <!-- Add a step 6 toohello user journey before hello JWT token is created-->
        <OrchestrationStep Order="6" Type="ClaimsExchange">
            <ClaimsExchanges>
                <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-LookUpLoyaltyWebHook" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="7" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
    </OrchestrationSteps>
    <ClientDefinition ReferenceId="DefaultWeb" />
</UserJourney>
```

## <a name="step-5-add-hello-claim-city-tooyour-relying-party-policy-file-so-hello-claim-is-sent-tooyour-application"></a>5. lépés: Hello jogcímszabály hozzáadása `city` tooyour függő entitás házirend fájlt, így hello jogcím küldött tooyour alkalmazás

Szerkessze a ProfileEdit.xml függő entitásonkénti (RP) fájlt, és módosítsa a hello `<TechnicalProfile Id="PolicyProfile">` elem tooadd hello alábbi: `<OutputClaim ClaimTypeReferenceId="city" />`.

Miután hozzáadta a hello új jogcímet, akkor a műszaki hello-profil néz ki:

```XML
<DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="OpenIdConnect" />
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
      <OutputClaim ClaimTypeReferenceId="city" />
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
</TechnicalProfile>
```

## <a name="step-6-upload-your-changes-and-test"></a>6. lépés: Töltse fel a módosításokat, és tesztelése

Meglévő verzióinak hello hello házirend felülírja.

1.  (Nem kötelező:) Mentse hello meglévő verzióját (Letöltés) a bővítmények fájl folytatás előtt. tookeep hello kezdeti összetettsége alacsony, azt javasoljuk, hogy nem töltsön hello bővítmények fájl több verziója.
2.  (Nem kötelező:) Nevezze át a hello házirend azonosítója hello házirend szerkesztése fájl új verziójának hello módosításával `PolicyId="B2C_1A_TrustFrameworkProfileEdit"`.
3.  Hello bővítmények fájl feltöltése.
4.  Hello házirend szerkesztése RP-fájl feltöltése.
5.  Használjon **Futtatás most** tootest hello házirend. Tekintse át a hello jogkivonatot adott hello IEF toohello alkalmazás adja vissza.

Ha minden megfelelően van beállítva, hello jogkivonat tartalmazza hello új jogcímet `city`, hello értékű `Redmond`.

```JSON
{
  "exp": 1493053292,
  "nbf": 1493049692,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "a58e7c6c-7535-4074-93da-b0023fbaf3ac",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustframeworkprofileedit",
  "nonce": "defaultNonce",
  "iat": 1493049692,
  "auth_time": 1493049692,
  "city": "Redmond"
}
```

## <a name="next-steps"></a>Következő lépések

[A REST API-t használják a ellenőrzési lépés](active-directory-b2c-rest-api-validation-custom.md)

[Hello profil szerkesztése toogather további információt a felhasználók a módosítása](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)
