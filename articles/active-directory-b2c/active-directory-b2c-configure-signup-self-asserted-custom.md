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
# <a name="azure-active-directory-b2c-modify-sign-up-tooadd-new-claims-and-configure-user-input"></a><span data-ttu-id="53082-103">Az Azure Active Directory B2C: Tooadd új jogcímeket regisztrációs módosítása, és konfigurálja a felhasználói beavatkozást.</span><span class="sxs-lookup"><span data-stu-id="53082-103">Azure Active Directory B2C: Modify sign up tooadd new claims and configure user input.</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="53082-104">Ebben a cikkben adhat egy új felhasználó által megadott bejegyzés (a jogcímek) tooyour előfizetési felhasználói út.</span><span class="sxs-lookup"><span data-stu-id="53082-104">In this article, you will add a new user provided entry (a claim) tooyour signup user journey.</span></span>  <span data-ttu-id="53082-105">Hello bejegyzés konfigurálása, a legördülő menüből, és szükség esetén adja meg.</span><span class="sxs-lookup"><span data-stu-id="53082-105">You will configure hello entry as a dropdown, and define if it is required.</span></span>

<span data-ttu-id="53082-106">Módosította a Sipi tootrigger teszt handoff számára.</span><span class="sxs-lookup"><span data-stu-id="53082-106">Edited by Sipi tootrigger test handoff.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="53082-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="53082-107">Prerequisites</span></span>

* <span data-ttu-id="53082-108">Teljes hello hello cikkben ismertetett visszaállítási lépésekkel [Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="53082-108">Complete hello steps in hello article [Getting Started with Custom Policies](active-directory-b2c-get-started-custom.md).</span></span>  <span data-ttu-id="53082-109">Tesztelje a hello regisztráció vagy bejelentkezés felhasználói út toosignup a folytatás előtt egy új helyi fiókot.</span><span class="sxs-lookup"><span data-stu-id="53082-109">Test hello signup/signin user journey toosignup a new local account before proceeding.</span></span>


<span data-ttu-id="53082-110">A felhasználók kezdeti adatgyűjtés az előfizetési/signin keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="53082-110">Gathering initial data from your users is achieved via signup/signin.</span></span>  <span data-ttu-id="53082-111">További jogcímek később gyűjthetők profil szerkesztése felhasználói utak keresztül.</span><span class="sxs-lookup"><span data-stu-id="53082-111">Additional claims can be gathered later via profile edit user journeys.</span></span> <span data-ttu-id="53082-112">Az Azure AD B2C gyűjtse össze az adatokat közvetlenül hello felhasználói interaktív módon, bármikor hello identitás élmény keretrendszer használja annak `selfasserted provider`.</span><span class="sxs-lookup"><span data-stu-id="53082-112">Anytime Azure AD B2C gathers information directly from hello user interactively, hello Identity Experience Framework uses its `selfasserted provider`.</span></span> <span data-ttu-id="53082-113">hello az alábbi lépéseket bármikor ezt a szolgáltatót használja.</span><span class="sxs-lookup"><span data-stu-id="53082-113">hello steps below apply anytime this provider is used.</span></span>


## <a name="define-hello-claim-its-display-name-and-hello-user-input-type"></a><span data-ttu-id="53082-114">Adja meg a hello jogcím, a megjelenítési nevet és egy hello felhasználói bevitel típusa</span><span class="sxs-lookup"><span data-stu-id="53082-114">Define hello claim, its display name and hello user input type</span></span>
<span data-ttu-id="53082-115">Lehetővé teszi, hogy hello felhasználói kérjen a város.</span><span class="sxs-lookup"><span data-stu-id="53082-115">Lets ask hello user for their city.</span></span>  <span data-ttu-id="53082-116">Adja hozzá a következő elem toohello hello `<ClaimsSchema>` hello TrustFrameWorkExtensions házirend fájlban:</span><span class="sxs-lookup"><span data-stu-id="53082-116">Add hello following element toohello `<ClaimsSchema>` element in hello TrustFrameWorkExtensions policy file:</span></span>

```xml
<ClaimType Id="city">
  <DisplayName>city</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your city</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```
<span data-ttu-id="53082-117">Nincsenek további lehetőségek itt jogcím toocustomize hello biztosíthatja.</span><span class="sxs-lookup"><span data-stu-id="53082-117">There are additional choices you can make here toocustomize hello claim.</span></span>  <span data-ttu-id="53082-118">A teljes séma, tekintse meg a toohello **identitás élmény keretrendszer a műszaki referencia-útmutató**.</span><span class="sxs-lookup"><span data-stu-id="53082-118">For a full schema, refer toohello **Identity Experience Framework Technical Reference Guide**.</span></span>  <span data-ttu-id="53082-119">Ez az útmutató hello útmutató szakaszban a közeljövőben lesznek közzétéve.</span><span class="sxs-lookup"><span data-stu-id="53082-119">This guide will be published soon in hello reference section.</span></span>

* <span data-ttu-id="53082-120">`<DisplayName>`egy karakterlánc, amely meghatározza a hello felhasználók számára is elérhető *címke*</span><span class="sxs-lookup"><span data-stu-id="53082-120">`<DisplayName>` is a string that defines hello user-facing *label*</span></span>

* <span data-ttu-id="53082-121">`<UserHelpText>`segít megérteni a szükséges hello felhasználói</span><span class="sxs-lookup"><span data-stu-id="53082-121">`<UserHelpText>` helps hello user understand what is required</span></span>

* <span data-ttu-id="53082-122">`<UserInputType>`hello következő négy beállítás kiemelt alatt:</span><span class="sxs-lookup"><span data-stu-id="53082-122">`<UserInputType>` has hello following four options highlighted below:</span></span>
    * `TextBox`
```xml
<ClaimType Id="city">
  <DisplayName>city where you work</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your city</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```

    * <span data-ttu-id="53082-123">`RadioSingleSelectduration`-Egyszeres kijelölésnél érvényesíti.</span><span class="sxs-lookup"><span data-stu-id="53082-123">`RadioSingleSelectduration` - Enforces a single selection.</span></span>
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

    * <span data-ttu-id="53082-124">`DropdownSingleSelect`-Érvényes értéke csak hello kiválasztását teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="53082-124">`DropdownSingleSelect` - Allows hello selection of only valid value.</span></span>

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


* <span data-ttu-id="53082-126">`CheckboxMultiSelect`Lehetővé teszi egy vagy több érték hello kiválasztását.</span><span class="sxs-lookup"><span data-stu-id="53082-126">`CheckboxMultiSelect` Allows for hello selection of one or more values.</span></span>

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

## <a name="add-hello-claim-toohello-sign-upsign-in-user-journey"></a><span data-ttu-id="53082-128">Adja hozzá a hello jogcím toohello bejelentkezési felhasználói út fel vagy bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="53082-128">Add hello claim toohello sign up/sign in user journey</span></span>

1. <span data-ttu-id="53082-129">Mint hello jogcím hozzáadása egy `<OutputClaim ClaimTypeReferenceId="city"/>` toohello TechnicalProfile `LocalAccountSignUpWithLogonEmail` (hello TrustFrameworkBase házirend fájl található).</span><span class="sxs-lookup"><span data-stu-id="53082-129">Add hello claim as an `<OutputClaim ClaimTypeReferenceId="city"/>` toohello TechnicalProfile `LocalAccountSignUpWithLogonEmail` (found in hello TrustFrameworkBase policy file).</span></span>  <span data-ttu-id="53082-130">Vegye figyelembe a TechnicalProfile hello SelfAssertedAttributeProvider használja.</span><span class="sxs-lookup"><span data-stu-id="53082-130">Note this TechnicalProfile uses hello SelfAssertedAttributeProvider.</span></span>

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

2. <span data-ttu-id="53082-131">Hello jogcím toohello AAD-UserWriteUsingLogonEmail, vegye fel a `<PersistedClaim ClaimTypeReferenceId="city" />` toowrite hello jogcím toohello AAD-címtárában hello felhasználói azt összegyűjtése után.</span><span class="sxs-lookup"><span data-stu-id="53082-131">Add hello claim toohello AAD-UserWriteUsingLogonEmail as a `<PersistedClaim ClaimTypeReferenceId="city" />` toowrite hello claim toohello AAD directory after collecting it from hello user.</span></span> <span data-ttu-id="53082-132">Előfordulhat, hogy kihagyja ezt a lépést, ha szeretné, hogy nem toopersist hello jogcím hello directory későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="53082-132">You may skip this step if you prefer not toopersist hello claim in hello directory for future use.</span></span>

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

3. <span data-ttu-id="53082-133">Adja hozzá a hello jogcím toohello TechnicalProfile hello könyvtárból olvassa be, amikor a felhasználó jelentkezik be amely egy`<OutputClaim ClaimTypeReferenceId="city" />`</span><span class="sxs-lookup"><span data-stu-id="53082-133">Add hello claim toohello TechnicalProfile that reads from hello directory when a user logs in as an `<OutputClaim ClaimTypeReferenceId="city" />`</span></span>

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

4. <span data-ttu-id="53082-134">Adja hozzá a hello `<OutputClaim ClaimTypeReferenceId="city" />` toohello RP házirendfájl SignUporSignIn.xml, ezt a kérelmet küldött toohello alkalmazás hello jogkivonat a felhasználó sikeres út után.</span><span class="sxs-lookup"><span data-stu-id="53082-134">Add hello `<OutputClaim ClaimTypeReferenceId="city" />` toohello RP policy file SignUporSignIn.xml so this claim is sent toohello application in hello token after a successful user journey.</span></span>

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

## <a name="test-hello-custom-policy-using-run-now"></a><span data-ttu-id="53082-135">Teszt hello egyéni házirend használatával "Futtatás most"</span><span class="sxs-lookup"><span data-stu-id="53082-135">Test hello custom policy using "Run Now"</span></span>

1. <span data-ttu-id="53082-136">Nyissa meg hello **panel az Azure AD B2C** , és keresse meg a túl**identitás élmény keretrendszer > egyéni házirendek**.</span><span class="sxs-lookup"><span data-stu-id="53082-136">Open hello **Azure AD B2C Blade** and navigate too**Identity Experience Framework > Custom policies**.</span></span>
2. <span data-ttu-id="53082-137">Válassza ki a feltöltött hello egyéni házirendet, majd kattintson a hello **futtatása most** gombra.</span><span class="sxs-lookup"><span data-stu-id="53082-137">Select hello custom policy that you uploaded, and click hello **Run now** button.</span></span>
3. <span data-ttu-id="53082-138">Meg kell tudni toosign be egy e-mail címet.</span><span class="sxs-lookup"><span data-stu-id="53082-138">You should be able toosign up using an email address.</span></span>

<span data-ttu-id="53082-139">hello előfizetési képernyő tesztmódban toothis hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="53082-139">hello signup screen in test mode should look similar toothis:</span></span>

![Képernyőkép a módosított előfizetési lehetőség](./media/active-directory-b2c-configure-signup-self-asserted-custom/signup-with-city-claim-dropdown-example.png)

  <span data-ttu-id="53082-141">hello token hátsó tooyour alkalmazás most már tartalmazza hello `city` jogcím a lent látható módon</span><span class="sxs-lookup"><span data-stu-id="53082-141">hello token back tooyour application will now include hello `city` claim as shown below</span></span>
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

## <a name="optional-remove-email-verification-from-signup-journey"></a><span data-ttu-id="53082-142">Választható lehetőség: Távolítsa el e-mail ellenőrzése az előfizetési út</span><span class="sxs-lookup"><span data-stu-id="53082-142">Optional: Remove email verification from signup journey</span></span>

<span data-ttu-id="53082-143">tooskip e-mail ellenőrzése, hello házirend Szerző választhat tooremove `PartnerClaimType="Verified.Email"`.</span><span class="sxs-lookup"><span data-stu-id="53082-143">tooskip email verification, hello policy author can choose tooremove `PartnerClaimType="Verified.Email"`.</span></span> <span data-ttu-id="53082-144">hello e-mail cím lesz szükség, de nincs ellenőrizve, kivéve, ha a "Kötelező" = true törlődik.</span><span class="sxs-lookup"><span data-stu-id="53082-144">hello email address will be required but not verified, unless “Required” = true is removed.</span></span>  <span data-ttu-id="53082-145">Alaposan fontolja meg, hogy ez a beállítás a használati esetek számára megfelelő!</span><span class="sxs-lookup"><span data-stu-id="53082-145">Carefully consider if this option is right for your use cases!</span></span>

<span data-ttu-id="53082-146">A hello alapértelmezés szerint engedélyezve van a e-mailek ellenőrzése `<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">` hello TrustFrameworkBase házirend hello alapszintű csomag fájlban:</span><span class="sxs-lookup"><span data-stu-id="53082-146">Verified email is enabled by default in hello `<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">` in hello TrustFrameworkBase policy file in hello starter pack:</span></span>
```xml
<OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="Verified.Email" Required="true" />
```

## <a name="next-steps"></a><span data-ttu-id="53082-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="53082-147">Next steps</span></span>

<span data-ttu-id="53082-148">Adja hozzá a hello új jogcímet toohello adatfolyamok a közösségi fiók bejelentkezések során TechnicalProfiles lenti hello módosításával.</span><span class="sxs-lookup"><span data-stu-id="53082-148">Add hello new claim toohello flows for social account logins by changing hello TechnicalProfiles listed below.</span></span> <span data-ttu-id="53082-149">Ezek társadalombiztosítási/összevont fiók bejelentkezések toowrite használják, és hello alternativeSecurityId használatával, mint a lokátor hello hello felhasználói adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="53082-149">These are used by social/federated account logins toowrite and read hello user data using hello alternativeSecurityId as hello locator.</span></span>
```xml
<TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
<TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```
