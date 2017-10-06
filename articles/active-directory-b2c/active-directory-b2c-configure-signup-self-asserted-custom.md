---
title: "Az Azure Active Directory B2C: Módosítsa a bejelentkezési be az egyéni házirendek, és saját magas szolgáltató konfigurálása"
description: "Egy általános bemutató hozzáadása toosign állítja be, és konfigurálja a hello felhasználói bevitel"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: tbd
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/29/2017
ms.author: joroja
ms.openlocfilehash: c31d737263fef3e771bdf451b809b0ca522c8fe0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-modify-sign-up-tooadd-new-claims-and-configure-user-input"></a>Az Azure Active Directory B2C: Tooadd új jogcímeket regisztrációs módosítása, és konfigurálja a felhasználói beavatkozást.

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Ebben a cikkben adhat egy új felhasználó által megadott bejegyzés (a jogcímek) tooyour előfizetési felhasználói út.  Hello bejegyzés konfigurálása, a legördülő menüből, és szükség esetén adja meg.

Módosította a Sipi tootrigger teszt handoff számára.

## <a name="prerequisites"></a>Előfeltételek

* Teljes hello hello cikkben ismertetett visszaállítási lépésekkel [Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md).  Tesztelje a hello regisztráció vagy bejelentkezés felhasználói út toosignup a folytatás előtt egy új helyi fiókot.


A felhasználók kezdeti adatgyűjtés az előfizetési/signin keresztül érhető el.  További jogcímek később gyűjthetők profil szerkesztése felhasználói utak keresztül. Az Azure AD B2C gyűjtse össze az adatokat közvetlenül hello felhasználói interaktív módon, bármikor hello identitás élmény keretrendszer használja annak `selfasserted provider`. hello az alábbi lépéseket bármikor ezt a szolgáltatót használja.


## <a name="define-hello-claim-its-display-name-and-hello-user-input-type"></a>Adja meg a hello jogcím, a megjelenítési nevet és egy hello felhasználói bevitel típusa
Lehetővé teszi, hogy hello felhasználói kérjen a város.  Adja hozzá a következő elem toohello hello `<ClaimsSchema>` hello TrustFrameWorkExtensions házirend fájlban:

```xml
<ClaimType Id="city">
  <DisplayName>city</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your city</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```
Nincsenek további lehetőségek itt jogcím toocustomize hello biztosíthatja.  A teljes séma, tekintse meg a toohello **identitás élmény keretrendszer a műszaki referencia-útmutató**.  Ez az útmutató hello útmutató szakaszban a közeljövőben lesznek közzétéve.

* `<DisplayName>`egy karakterlánc, amely meghatározza a hello felhasználók számára is elérhető *címke*

* `<UserHelpText>`segít megérteni a szükséges hello felhasználói

* `<UserInputType>`hello következő négy beállítás kiemelt alatt:
    * `TextBox`
```xml
<ClaimType Id="city">
  <DisplayName>city where you work</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your city</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```

    * `RadioSingleSelectduration`-Egyszeres kijelölésnél érvényesíti.
```xml
<ClaimType Id="city">
  <DisplayName>city where you work</DisplayName>
  <DataType>string</DataType>
  <UserInputType>RadioSingleSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="Kirkland" Value="kirkland" SelectByDefault="false" />
  </Restriction>
</ClaimType>
```

    * `DropdownSingleSelect`-Érvényes értéke csak hello kiválasztását teszi lehetővé.

![Képernyőfelvétel a legördülő lista lehetőséget](./media/active-directory-b2c-configure-signup-self-asserted-custom/dropdown-menu-example.png)


```xml
<ClaimType Id="city">
  <DisplayName>city where you work</DisplayName>
  <DataType>string</DataType>
  <UserInputType>DropdownSingleSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="Kirkland" Value="kirkland" SelectByDefault="false" />
  </Restriction>
</ClaimType>
```


* `CheckboxMultiSelect`Lehetővé teszi egy vagy több érték hello kiválasztását.

![Képernyőkép a multiselect beállítás](./media/active-directory-b2c-configure-signup-self-asserted-custom/multiselect-menu-example.png)


```xml
<ClaimType Id="city">
  <DisplayName>Receive updates from which cities?</DisplayName>
  <DataType>string</DataType>
  <UserInputType>CheckboxMultiSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="Kirkland" Value="kirkland" SelectByDefault="false" />
  </Restriction>
</ClaimType>
```

## <a name="add-hello-claim-toohello-sign-upsign-in-user-journey"></a>Adja hozzá a hello jogcím toohello bejelentkezési felhasználói út fel vagy bejelentkezés

1. Mint hello jogcím hozzáadása egy `<OutputClaim ClaimTypeReferenceId="city"/>` toohello TechnicalProfile `LocalAccountSignUpWithLogonEmail` (hello TrustFrameworkBase házirend fájl található).  Vegye figyelembe a TechnicalProfile hello SelfAssertedAttributeProvider használja.

  ```xml
  <TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
    <DisplayName>Email signup</DisplayName>
    <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
    <Metadata>
      <Item Key="IpAddressClaimReferenceId">IpAddress</Item>
      <Item Key="ContentDefinitionReferenceId">api.localaccountsignup</Item>
      <Item Key="language.button_continue">Create</Item>
    </Metadata>
    <CryptographicKeys>
      <Key Id="issuer_secret" StorageReferenceId="TokenSigningKeyContainer" />
    </CryptographicKeys>
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="email" />
    </InputClaims>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="objectId" />
      <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="Verified.Email" Required="true" />
      <OutputClaim ClaimTypeReferenceId="newPassword" Required="true" />
      <OutputClaim ClaimTypeReferenceId="reenterPassword" Required="true" />
      <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />
      <OutputClaim ClaimTypeReferenceId="authenticationSource" />
      <OutputClaim ClaimTypeReferenceId="newUser" />
      <!-- Optional claims, toobe collected from hello user -->
      <OutputClaim ClaimTypeReferenceId="givenName" />
      <OutputClaim ClaimTypeReferenceId="surName" />
      <OutputClaim ClaimTypeReferenceId="city"/>
    </OutputClaims>
    <ValidationTechnicalProfiles>
      <ValidationTechnicalProfile ReferenceId="AAD-UserWriteUsingLogonEmail" />
    </ValidationTechnicalProfiles>
    <UseTechnicalProfileForSessionManagement ReferenceId="SM-AAD" />
  </TechnicalProfile>
  ```

2. Hello jogcím toohello AAD-UserWriteUsingLogonEmail, vegye fel a `<PersistedClaim ClaimTypeReferenceId="city" />` toowrite hello jogcím toohello AAD-címtárában hello felhasználói azt összegyűjtése után. Előfordulhat, hogy kihagyja ezt a lépést, ha szeretné, hogy nem toopersist hello jogcím hello directory későbbi használatra.

  ```xml
  <!-- Technical profiles for local accounts -->
  <TechnicalProfile Id="AAD-UserWriteUsingLogonEmail">
    <Metadata>
      <Item Key="Operation">Write</Item>
      <Item Key="RaiseErrorIfClaimsPrincipalAlreadyExists">true</Item>
    </Metadata>
    <IncludeInSso>false</IncludeInSso>
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="email" PartnerClaimType="signInNames.emailAddress" Required="true" />
    </InputClaims>
    <PersistedClaims>
      <!-- Required claims -->
      <PersistedClaim ClaimTypeReferenceId="email" PartnerClaimType="signInNames.emailAddress" />
      <PersistedClaim ClaimTypeReferenceId="newPassword" PartnerClaimType="password" />
      <PersistedClaim ClaimTypeReferenceId="displayName" DefaultValue="unknown" />
      <PersistedClaim ClaimTypeReferenceId="passwordPolicies" DefaultValue="DisablePasswordExpiration" />
      <!-- Optional claims. -->
      <PersistedClaim ClaimTypeReferenceId="givenName" />
      <PersistedClaim ClaimTypeReferenceId="surname" />
      <PersistedClaim ClaimTypeReferenceId="city" />
    </PersistedClaims>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="objectId" />
      <OutputClaim ClaimTypeReferenceId="newUser" PartnerClaimType="newClaimsPrincipalCreated" />
      <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="localAccountAuthentication" />
      <OutputClaim ClaimTypeReferenceId="userPrincipalName" />
      <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
    </OutputClaims>
    <IncludeTechnicalProfile ReferenceId="AAD-Common" />
    <UseTechnicalProfileForSessionManagement ReferenceId="SM-AAD" />
  </TechnicalProfile>
  ```

3. Adja hozzá a hello jogcím toohello TechnicalProfile hello könyvtárból olvassa be, amikor a felhasználó jelentkezik be amely egy`<OutputClaim ClaimTypeReferenceId="city" />`

  ```xml
  <TechnicalProfile Id="AAD-UserReadUsingEmailAddress">
    <Metadata>
      <Item Key="Operation">Read</Item>
      <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
      <Item Key="UserMessageIfClaimsPrincipalDoesNotExist">An account could not be found for hello provided user ID.</Item>
    </Metadata>
    <IncludeInSso>false</IncludeInSso>
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="email" PartnerClaimType="signInNames" Required="true" />
    </InputClaims>
    <OutputClaims>
      <!-- Required claims -->
      <OutputClaim ClaimTypeReferenceId="objectId" />
      <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="localAccountAuthentication" />
      <!-- Optional claims -->
      <OutputClaim ClaimTypeReferenceId="userPrincipalName" />
      <OutputClaim ClaimTypeReferenceId="displayName" />
      <OutputClaim ClaimTypeReferenceId="otherMails" />
      <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
      <OutputClaim ClaimTypeReferenceId="city" />
    </OutputClaims>
    <IncludeTechnicalProfile ReferenceId="AAD-Common" />
  </TechnicalProfile>
  ```

4. Adja hozzá a hello `<OutputClaim ClaimTypeReferenceId="city" />` toohello RP házirendfájl SignUporSignIn.xml, ezt a kérelmet küldött toohello alkalmazás hello jogkivonat a felhasználó sikeres út után.

  ```xml
  <RelyingParty>
    <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
    <TechnicalProfile Id="PolicyProfile">
      <DisplayName>PolicyProfile</DisplayName>
      <Protocol Name="OpenIdConnect" />
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />
        <OutputClaim ClaimTypeReferenceId="email" />
        <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
        <OutputClaim ClaimTypeReferenceId="identityProvider" />
        <OutputClaim ClaimTypeReferenceId="city" />
      </OutputClaims>
      <SubjectNamingInfo ClaimType="sub" />
    </TechnicalProfile>
  </RelyingParty>
  ```

## <a name="test-hello-custom-policy-using-run-now"></a>Teszt hello egyéni házirend használatával "Futtatás most"

1. Nyissa meg hello **panel az Azure AD B2C** , és keresse meg a túl**identitás élmény keretrendszer > egyéni házirendek**.
2. Válassza ki a feltöltött hello egyéni házirendet, majd kattintson a hello **futtatása most** gombra.
3. Meg kell tudni toosign be egy e-mail címet.

hello előfizetési képernyő tesztmódban toothis hasonlóan kell kinéznie:

![Képernyőkép a módosított előfizetési lehetőség](./media/active-directory-b2c-configure-signup-self-asserted-custom/signup-with-city-claim-dropdown-example.png)

  hello token hátsó tooyour alkalmazás most már tartalmazza hello `city` jogcím a lent látható módon
```json
{
  "exp": 1493596822,
  "nbf": 1493593222,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "9c2a3a9e-ac65-4e46-a12d-9557b63033a9",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustf_signup_signin",
  "nonce": "defaultNonce",
  "iat": 1493593222,
  "auth_time": 1493593222,
  "email": "joe@outlook.com",
  "given_name": "Joe",
  "family_name": "Ras",
  "city": "Bellevue",
  "name": "unknown"
}
```

## <a name="optional-remove-email-verification-from-signup-journey"></a>Választható lehetőség: Távolítsa el e-mail ellenőrzése az előfizetési út

tooskip e-mail ellenőrzése, hello házirend Szerző választhat tooremove `PartnerClaimType="Verified.Email"`. hello e-mail cím lesz szükség, de nincs ellenőrizve, kivéve, ha a "Kötelező" = true törlődik.  Alaposan fontolja meg, hogy ez a beállítás a használati esetek számára megfelelő!

A hello alapértelmezés szerint engedélyezve van a e-mailek ellenőrzése `<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">` hello TrustFrameworkBase házirend hello alapszintű csomag fájlban:
```xml
<OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="Verified.Email" Required="true" />
```

## <a name="next-steps"></a>Következő lépések

Adja hozzá a hello új jogcímet toohello adatfolyamok a közösségi fiók bejelentkezések során TechnicalProfiles lenti hello módosításával. Ezek társadalombiztosítási/összevont fiók bejelentkezések toowrite használják, és hello alternativeSecurityId használatával, mint a lokátor hello hello felhasználói adatokat olvasni.
```xml
<TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
<TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```
