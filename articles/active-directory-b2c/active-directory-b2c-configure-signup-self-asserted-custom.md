---
title: "Az Azure Active Directory B2C: Módosítsa a bejelentkezési be az egyéni házirendek, és saját magas szolgáltató konfigurálása"
description: "Egy általános bemutató hozzáadása regisztrálhat és konfigurálása a felhasználó által megadott jogcímek"
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
ms.openlocfilehash: 64b9d904d7d070052e125b479f4719d208c9ff85
ms.sourcegitcommit: b0af2a2cf44101a1b1ff41bd2ad795eaef29612a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 09/28/2017
---
# <a name="azure-active-directory-b2c-modify-sign-up-to-add-new-claims-and-configure-user-input"></a><span data-ttu-id="0c820-103">Az Azure Active Directory B2C: A módosítás jelentkezzen be új jogcímeket adhatnak hozzá, és konfigurálja a felhasználói bevitel.</span><span class="sxs-lookup"><span data-stu-id="0c820-103">Azure Active Directory B2C: Modify sign up to add new claims and configure user input.</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="0c820-104">Ebben a cikkben egy új felhasználó által megadott bejegyzés (a jogcímek) fogja hozzáadni a bejelentkezési felhasználói használatában.</span><span class="sxs-lookup"><span data-stu-id="0c820-104">In this article, you will add a new user provided entry (a claim) to your signup user journey.</span></span>  <span data-ttu-id="0c820-105">A bejegyzés konfigurálása, a legördülő menüből, és szükség esetén adja meg.</span><span class="sxs-lookup"><span data-stu-id="0c820-105">You will configure the entry as a dropdown, and define if it is required.</span></span>

<span data-ttu-id="0c820-106">Módosította a Sipi elindítani a teszt handoff számára.</span><span class="sxs-lookup"><span data-stu-id="0c820-106">Edited by Sipi to trigger test handoff.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0c820-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0c820-107">Prerequisites</span></span>

* <span data-ttu-id="0c820-108">Hajtsa végre a cikk a [Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="0c820-108">Complete the steps in the article [Getting Started with Custom Policies](active-directory-b2c-get-started-custom.md).</span></span>  <span data-ttu-id="0c820-109">A regisztráció vagy bejelentkezés felhasználói utazás előfizetési egy új helyi fiók a folytatás előtt tesztelje.</span><span class="sxs-lookup"><span data-stu-id="0c820-109">Test the signup/signin user journey to signup a new local account before proceeding.</span></span>


<span data-ttu-id="0c820-110">A felhasználók kezdeti adatgyűjtés az előfizetési/signin keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="0c820-110">Gathering initial data from your users is achieved via signup/signin.</span></span>  <span data-ttu-id="0c820-111">További jogcímek később gyűjthetők profil szerkesztése felhasználói utak keresztül.</span><span class="sxs-lookup"><span data-stu-id="0c820-111">Additional claims can be gathered later via profile edit user journeys.</span></span> <span data-ttu-id="0c820-112">Az Azure AD B2C gyűjtse össze az adatokat közvetlenül a felhasználó interaktív módon, bármikor identitás élmény keretében használja a `selfasserted provider`.</span><span class="sxs-lookup"><span data-stu-id="0c820-112">Anytime Azure AD B2C gathers information directly from the user interactively, the Identity Experience Framework uses its `selfasserted provider`.</span></span> <span data-ttu-id="0c820-113">Az alábbi lépéseket alkalmazni, bármikor ezt a szolgáltatót használja.</span><span class="sxs-lookup"><span data-stu-id="0c820-113">The steps below apply anytime this provider is used.</span></span>


## <a name="define-the-claim-its-display-name-and-the-user-input-type"></a><span data-ttu-id="0c820-114">A jogcímek, a megjelenített név és a felhasználó bemeneti típus megadása</span><span class="sxs-lookup"><span data-stu-id="0c820-114">Define the claim, its display name and the user input type</span></span>
<span data-ttu-id="0c820-115">Lehetővé teszi, hogy azok városhoz kérnie a felhasználót.</span><span class="sxs-lookup"><span data-stu-id="0c820-115">Lets ask the user for their city.</span></span>  <span data-ttu-id="0c820-116">A következő elem hozzáadása a `<ClaimsSchema>` a TrustFrameWorkExtensions házirend fájlban:</span><span class="sxs-lookup"><span data-stu-id="0c820-116">Add the following element to the `<ClaimsSchema>` element in the TrustFrameWorkExtensions policy file:</span></span>

```xml
<ClaimType Id="city">
  <DisplayName>city</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your city</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```
<span data-ttu-id="0c820-117">Nincsenek további lehetőségek tehet itt testre szabhatja a jogcímek.</span><span class="sxs-lookup"><span data-stu-id="0c820-117">There are additional choices you can make here to customize the claim.</span></span>  <span data-ttu-id="0c820-118">A teljes séma, tekintse meg a **identitás élmény keretrendszer a műszaki referencia-útmutató**.</span><span class="sxs-lookup"><span data-stu-id="0c820-118">For a full schema, refer to the **Identity Experience Framework Technical Reference Guide**.</span></span>  <span data-ttu-id="0c820-119">Ez az útmutató az útmutató szakaszban a közeljövőben lesznek közzétéve.</span><span class="sxs-lookup"><span data-stu-id="0c820-119">This guide will be published soon in the reference section.</span></span>

* <span data-ttu-id="0c820-120">`<DisplayName>`egy karakterlánc, amely meghatározza a felhasználók számára is elérhető *címke*</span><span class="sxs-lookup"><span data-stu-id="0c820-120">`<DisplayName>` is a string that defines the user-facing *label*</span></span>

* <span data-ttu-id="0c820-121">`<UserHelpText>`a rendszer kötelező felhasználó segítségével</span><span class="sxs-lookup"><span data-stu-id="0c820-121">`<UserHelpText>` helps the user understand what is required</span></span>

* <span data-ttu-id="0c820-122">`<UserInputType>`a következő négy beállítás alatt van kiemelve:</span><span class="sxs-lookup"><span data-stu-id="0c820-122">`<UserInputType>` has the following four options highlighted below:</span></span>
    * `TextBox`
```xml
<ClaimType Id="city">
  <DisplayName>city where you work</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your city</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```

    * <span data-ttu-id="0c820-123">`RadioSingleSelectduration`-Egyszeres kijelölésnél érvényesíti.</span><span class="sxs-lookup"><span data-stu-id="0c820-123">`RadioSingleSelectduration` - Enforces a single selection.</span></span>
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

    * <span data-ttu-id="0c820-124">`DropdownSingleSelect`-Lehetővé teszi, hogy a kijelölés csak érvényes érték.</span><span class="sxs-lookup"><span data-stu-id="0c820-124">`DropdownSingleSelect` - Allows the selection of only valid value.</span></span>

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


* <span data-ttu-id="0c820-126">`CheckboxMultiSelect`Lehetővé teszi a kijelölt egy vagy több értéket.</span><span class="sxs-lookup"><span data-stu-id="0c820-126">`CheckboxMultiSelect` Allows for the selection of one or more values.</span></span>

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

## <a name="add-the-claim-to-the-sign-upsign-in-user-journey"></a><span data-ttu-id="0c820-128">Vegye fel a kérelmet a bejelentkezési felhasználói út felfelé vagy bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0c820-128">Add the claim to the sign up/sign in user journey</span></span>

1. <span data-ttu-id="0c820-129">Adja hozzá a jogcímek, az `<OutputClaim ClaimTypeReferenceId="city"/>` a TechnicalProfile való `LocalAccountSignUpWithLogonEmail` (a TrustFrameworkBase házirend fájlban található).</span><span class="sxs-lookup"><span data-stu-id="0c820-129">Add the claim as an `<OutputClaim ClaimTypeReferenceId="city"/>` to the TechnicalProfile `LocalAccountSignUpWithLogonEmail` (found in the TrustFrameworkBase policy file).</span></span>  <span data-ttu-id="0c820-130">Vegye figyelembe a TechnicalProfile használja a SelfAssertedAttributeProvider.</span><span class="sxs-lookup"><span data-stu-id="0c820-130">Note this TechnicalProfile uses the SelfAssertedAttributeProvider.</span></span>

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
      <!-- Optional claims, to be collected from the user -->
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

2. <span data-ttu-id="0c820-131">Az AAD-UserWriteUsingLogonEmail, a jogcím hozzáadása egy `<PersistedClaim ClaimTypeReferenceId="city" />` az AAD-címtárában, a felhasználó összegyűjtése után a jogcím írni.</span><span class="sxs-lookup"><span data-stu-id="0c820-131">Add the claim to the AAD-UserWriteUsingLogonEmail as a `<PersistedClaim ClaimTypeReferenceId="city" />` to write the claim to the AAD directory after collecting it from the user.</span></span> <span data-ttu-id="0c820-132">Ezt a lépést kihagyhatja, ha nem szeretné megőrizni a későbbi használatra a könyvtárban a jogcímet.</span><span class="sxs-lookup"><span data-stu-id="0c820-132">You may skip this step if you prefer not to persist the claim in the directory for future use.</span></span>

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

3. <span data-ttu-id="0c820-133">Az olvasó a könyvtárból, amikor a felhasználó jelentkezik be a TechnicalProfile jogcím hozzáadása egy`<OutputClaim ClaimTypeReferenceId="city" />`</span><span class="sxs-lookup"><span data-stu-id="0c820-133">Add the claim to the TechnicalProfile that reads from the directory when a user logs in as an `<OutputClaim ClaimTypeReferenceId="city" />`</span></span>

  ```xml
  <TechnicalProfile Id="AAD-UserReadUsingEmailAddress">
    <Metadata>
      <Item Key="Operation">Read</Item>
      <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
      <Item Key="UserMessageIfClaimsPrincipalDoesNotExist">An account could not be found for the provided user ID.</Item>
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

4. <span data-ttu-id="0c820-134">Adja hozzá a `<OutputClaim ClaimTypeReferenceId="city" />` SignUporSignIn.xml RP szabályzat fájl, ezért ezt a kérelmet küld az alkalmazás a jogkivonat a felhasználó sikeres út után.</span><span class="sxs-lookup"><span data-stu-id="0c820-134">Add the `<OutputClaim ClaimTypeReferenceId="city" />` to the RP policy file SignUporSignIn.xml so this claim is sent to the application in the token after a successful user journey.</span></span>

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

## <a name="test-the-custom-policy-using-run-now"></a><span data-ttu-id="0c820-135">Az egyéni házirend használatával "Futtatás most" tesztelése</span><span class="sxs-lookup"><span data-stu-id="0c820-135">Test the custom policy using "Run Now"</span></span>

1. <span data-ttu-id="0c820-136">Nyissa meg a **panel az Azure AD B2C** , és keresse meg **identitás élmény keretrendszer > egyéni házirendek**.</span><span class="sxs-lookup"><span data-stu-id="0c820-136">Open the **Azure AD B2C Blade** and navigate to **Identity Experience Framework > Custom policies**.</span></span>
2. <span data-ttu-id="0c820-137">Válassza ki az egyéni házirendet, feltöltött, majd kattintson a **futtatása most** gombra.</span><span class="sxs-lookup"><span data-stu-id="0c820-137">Select the custom policy that you uploaded, and click the **Run now** button.</span></span>
3. <span data-ttu-id="0c820-138">Iratkozhat fel e-mail cím használatával kell lennie.</span><span class="sxs-lookup"><span data-stu-id="0c820-138">You should be able to sign up using an email address.</span></span>

<span data-ttu-id="0c820-139">A regisztráció képernyő tesztmódban ehhez hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="0c820-139">The signup screen in test mode should look similar to this:</span></span>

![Képernyőkép a módosított előfizetési lehetőség](./media/active-directory-b2c-configure-signup-self-asserted-custom/signup-with-city-claim-dropdown-example.png)

  <span data-ttu-id="0c820-141">A jogkivonat az alkalmazás most már tartalmazza a `city` jogcím a lent látható módon</span><span class="sxs-lookup"><span data-stu-id="0c820-141">The token back to your application will now include the `city` claim as shown below</span></span>
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

## <a name="optional-remove-email-verification-from-signup-journey"></a><span data-ttu-id="0c820-142">Választható lehetőség: Távolítsa el e-mail ellenőrzése az előfizetési út</span><span class="sxs-lookup"><span data-stu-id="0c820-142">Optional: Remove email verification from signup journey</span></span>

<span data-ttu-id="0c820-143">E-mail-ellenőrzés kihagyása a házirend Szerző beállíthatja úgy a eltávolítása `PartnerClaimType="Verified.Email"`.</span><span class="sxs-lookup"><span data-stu-id="0c820-143">To skip email verification, the policy author can choose to remove `PartnerClaimType="Verified.Email"`.</span></span> <span data-ttu-id="0c820-144">Az e-mail cím lesz szükség, de nincs ellenőrizve, kivéve, ha a "Kötelező" = true törlődik.</span><span class="sxs-lookup"><span data-stu-id="0c820-144">The email address will be required but not verified, unless “Required” = true is removed.</span></span>  <span data-ttu-id="0c820-145">Alaposan fontolja meg, hogy ez a beállítás a használati esetek számára megfelelő!</span><span class="sxs-lookup"><span data-stu-id="0c820-145">Carefully consider if this option is right for your use cases!</span></span>

<span data-ttu-id="0c820-146">Alapértelmezés szerint engedélyezve van a e-mailek ellenőrzése a `<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">` az alapszintű csomag TrustFrameworkBase házirend fájlban:</span><span class="sxs-lookup"><span data-stu-id="0c820-146">Verified email is enabled by default in the `<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">` in the TrustFrameworkBase policy file in the starter pack:</span></span>
```xml
<OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="Verified.Email" Required="true" />
```

## <a name="next-steps"></a><span data-ttu-id="0c820-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0c820-147">Next steps</span></span>

<span data-ttu-id="0c820-148">Adja hozzá a közösségi fiók bejelentkezések során a viszonylatában új jogcímet a lenti TechnicalProfiles módosításával.</span><span class="sxs-lookup"><span data-stu-id="0c820-148">Add the new claim to the flows for social account logins by changing the TechnicalProfiles listed below.</span></span> <span data-ttu-id="0c820-149">Ezek használhatók társadalombiztosítási/összevont fiók bejelentkezések által írható és olvasható a felhasználói adatokat a alternativeSecurityId használja, mint a lokátor.</span><span class="sxs-lookup"><span data-stu-id="0c820-149">These are used by social/federated account logins to write and read the user data using the alternativeSecurityId as the locator.</span></span>
```xml
<TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
<TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```
