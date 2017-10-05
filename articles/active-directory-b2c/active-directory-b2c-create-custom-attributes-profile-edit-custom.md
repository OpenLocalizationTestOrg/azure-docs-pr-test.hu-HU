---
title: "Az Azure Active Directory B2C: Saját attribútumokat adhat hozzá egyéni házirendeket, és használja a profil szerkesztése |} Microsoft Docs"
description: "A forgatókönyv bővítmény tulajdonságok, egyéni attribútumok használatát, és többek között azokat a felhasználói felületen"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: 
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: joroja
ms.openlocfilehash: 67c9f6eca18e2dd77e00b8bc8c7bcc546ea3936e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-b2c-creating-and-using-custom-attributes-in-a-custom-profile-edit-policy"></a><span data-ttu-id="acd51-103">Az Azure Active Directory B2C: Létrehozása és az egyéni attribútumok használata egy egyéni profilt a házirend szerkesztése</span><span class="sxs-lookup"><span data-stu-id="acd51-103">Azure Active Directory B2C: Creating and using custom attributes in a custom profile edit policy</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="acd51-104">Ebben a cikkben egy egyéni attribútum létrehozása a Azure AD B2C-címtárban, és egy egyéni jogcímet a felhasználó utazás profil szerkesztése az új attribútum használja.</span><span class="sxs-lookup"><span data-stu-id="acd51-104">In this article, you create a custom attribute in your Azure AD B2C directory and use this new attribute as a custom claim in the profile edit user journey.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="acd51-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="acd51-105">Prerequisites</span></span>

<span data-ttu-id="acd51-106">Hajtsa végre a cikk a [Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="acd51-106">Complete the steps in the article [Getting Started with Custom Policies](active-directory-b2c-get-started-custom.md).</span></span>

## <a name="use-custom-attributes-to-collect-information-about-your-customers-in-azure-active-directory-b2c-using-custom-policies"></a><span data-ttu-id="acd51-107">Az ügyfelek az Azure Active Directory B2C egyéni házirendekkel kapcsolatos információk összegyűjtéséhez használja az egyéni attribútumok</span><span class="sxs-lookup"><span data-stu-id="acd51-107">Use custom attributes to collect information about your customers in Azure Active Directory B2C using custom policies</span></span>
<span data-ttu-id="acd51-108">Azure Active Directory (Azure AD) B2C-címtárban tartalmaz egy beépített attribútumok: megadott név, Vezetéknév, város, irányítószám, userPrincipalName, stb.  Gyakran a saját attribútumok létrehozásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="acd51-108">Your Azure Active Directory (Azure AD) B2C directory comes with a built-in set of attributes: Given Name, Surname, City, Postal Code, userPrincipalName, etc.  You often need to create your own attributes.</span></span>  <span data-ttu-id="acd51-109">Példa:</span><span class="sxs-lookup"><span data-stu-id="acd51-109">For example:</span></span>
* <span data-ttu-id="acd51-110">Egy ügyfélkapcsolati alkalmazást kell megőrizni a egy attribútum, például a "LoyaltyNumber."</span><span class="sxs-lookup"><span data-stu-id="acd51-110">A customer-facing application needs to persist an attribute such as "LoyaltyNumber."</span></span>
* <span data-ttu-id="acd51-111">Az identitásszolgáltató rendelkezik egy egyedi felhasználói azonosító, amelyet kell menteni, például a "uniqueUserGUID." "</span><span class="sxs-lookup"><span data-stu-id="acd51-111">An identity provider has a unique user identifier that must be saved such as "uniqueUserGUID.""</span></span>
* <span data-ttu-id="acd51-112">Egyéni felhasználói út kell megőrizni a felhasználó például "migrationStatus." állapota</span><span class="sxs-lookup"><span data-stu-id="acd51-112">A custom user journey needs to persist the state of user such as "migrationStatus."</span></span>

<span data-ttu-id="acd51-113">Az Azure AD B2C-ben az attribútumokat, minden egyes felhasználói fiókjában tárolt bővítheti.</span><span class="sxs-lookup"><span data-stu-id="acd51-113">With Azure AD B2C, you can extend the set of attributes stored on each user account.</span></span> <span data-ttu-id="acd51-114">Is olvasási és írási ezek az attribútumok használatával a [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="acd51-114">You can also read and write these attributes by using the [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).</span></span>

<span data-ttu-id="acd51-115">Bővítmény tulajdonságai a felhasználó a címtárban található objektumokhoz sémája bővíthető.</span><span class="sxs-lookup"><span data-stu-id="acd51-115">Extension properties extend the schema of the user objects in the directory.</span></span>  <span data-ttu-id="acd51-116">A feltételek bővített tulajdonság, az egyéni attribútum és az egyéni jogcím tekintse meg az ugyanaz a cikk a környezetében, és nevét a környezetben (alkalmazás, objektum, a házirend) függ.</span><span class="sxs-lookup"><span data-stu-id="acd51-116">The terms extension property, custom attribute and custom claim refer to the same thing in the context of this article and the name varies depending on the context (application, object, policy).</span></span>

<span data-ttu-id="acd51-117">Bővítmény tulajdonságai csak regisztrálható az alkalmazásobjektum, annak ellenére, hogy egy felhasználó lehet, hogy adatokat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="acd51-117">Extension properties can only be registered on an Application object even though they may contain data for a User.</span></span> <span data-ttu-id="acd51-118">Az alkalmazás a tulajdonság van csatolva.</span><span class="sxs-lookup"><span data-stu-id="acd51-118">The property is attached to the application.</span></span> <span data-ttu-id="acd51-119">Az Application objektum regisztrálni egy bővített tulajdonság írási hozzáférést kell rendelni.</span><span class="sxs-lookup"><span data-stu-id="acd51-119">The Application object must be granted write access to register an extension property.</span></span> <span data-ttu-id="acd51-120">100 bővítmény tulajdonságai (közötti összes típusa és az összes alkalmazás) csak írható egyetlen objektumhoz sem.</span><span class="sxs-lookup"><span data-stu-id="acd51-120">100 Extension properties (across ALL types and ALL applications) can be written to any single object.</span></span> <span data-ttu-id="acd51-121">Bővítmény tulajdonságai a céltípus directory adnak, és az Azure AD B2C directory bérlő azonnal elérhető lesz.</span><span class="sxs-lookup"><span data-stu-id="acd51-121">Extension properties are added to the target directory type and becomes immediately accessible in the Azure AD B2C directory tenant.</span></span>
<span data-ttu-id="acd51-122">Az alkalmazás törlése, ha az összes felhasználó számára a bennük található adatokat ilyen bővítmény tulajdonságok is törlődnek.</span><span class="sxs-lookup"><span data-stu-id="acd51-122">If the application is deleted, those Extension properties along with any data contained in them for all users are also removed.</span></span> <span data-ttu-id="acd51-123">Egy bővített tulajdonság nem törli azokat az alkalmazást, ha a rendszer eltávolítja a cél címtárobjektumok, és törli az értékeket.</span><span class="sxs-lookup"><span data-stu-id="acd51-123">If an extension property is deleted by the Application, it is removed on the target directory objects, and the values deleted.</span></span>

<span data-ttu-id="acd51-124">Bővítmény tulajdonságai csak a bérlő regisztrált alkalmazás környezetében található.</span><span class="sxs-lookup"><span data-stu-id="acd51-124">Extension properties exist only in the context of a registered  Application in the tenant.</span></span> <span data-ttu-id="acd51-125">Az objektumazonosító alkalmazás az azt használó TechnicalProfile kell szerepelnie.</span><span class="sxs-lookup"><span data-stu-id="acd51-125">The object id of that Application must be included in the TechnicalProfile that use it.</span></span>

>[!NOTE]
><span data-ttu-id="acd51-126">Az Azure AD B2C-címtár közé tartozik a webes alkalmazás neve `b2c-extensions-app`.</span><span class="sxs-lookup"><span data-stu-id="acd51-126">The Azure AD B2C directory typically includes a Web App named `b2c-extensions-app`.</span></span>  <span data-ttu-id="acd51-127">Ez az alkalmazás elsősorban a b2c beépített házirendek az Azure-portálon létrehozott egyéni jogcímek esetében.</span><span class="sxs-lookup"><span data-stu-id="acd51-127">This application is primarily used by the b2c built-in  policies for the custom claims created via the Azure portal.</span></span>  <span data-ttu-id="acd51-128">Egyéni házirendek b2c-bővítmények regisztrálni az alkalmazás használata csak haladó felhasználóknak javasolt.</span><span class="sxs-lookup"><span data-stu-id="acd51-128">Using this application to register extensions for b2c custom policies is recommended only for advanced users.</span></span>  <span data-ttu-id="acd51-129">Ehhez útmutatást a következő lépések című részben szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="acd51-129">Instructions for this are included in the Next Steps section in this article.</span></span>


## <a name="creating-a-new-application-to-store-the-extension-properties"></a><span data-ttu-id="acd51-130">A bővítmény tulajdonságok tárolásához egy új alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="acd51-130">Creating a new application to store the extension properties</span></span>

1. <span data-ttu-id="acd51-131">Nyissa meg a böngésző munkamenetet, és keresse meg a [Azure porta](https://portal.azure.com) és jelentkezzen be rendszergazdai hitelesítő adataival a B2C-címtárban való konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="acd51-131">Open a browsing session and navigate to the [Azure porta](https://portal.azure.com) and sign in with administrative credentials of the B2C Directory you wish to configure.</span></span>
1. <span data-ttu-id="acd51-132">Kattintson a **Azure Active Directory** a bal oldali navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="acd51-132">Click **Azure Active Directory** on the left navigation menu.</span></span> <span data-ttu-id="acd51-133">Szükség lehet további szolgáltatások kiválasztásával kereséséhez >.</span><span class="sxs-lookup"><span data-stu-id="acd51-133">You may need to find it by selecting More services>.</span></span>
1. <span data-ttu-id="acd51-134">Válassza ki **App regisztrációk** kattintson **új alkalmazás regisztrációja**</span><span class="sxs-lookup"><span data-stu-id="acd51-134">Select **App registrations** and click **New application registration**</span></span>
1. <span data-ttu-id="acd51-135">Adja meg az alábbi ajánlott bejegyzéseket:</span><span class="sxs-lookup"><span data-stu-id="acd51-135">Provide the following recommended entries:</span></span>
  * <span data-ttu-id="acd51-136">Adjon meg egy nevet a webalkalmazás: **WebApp-GraphAPI-DirectoryExtensions**</span><span class="sxs-lookup"><span data-stu-id="acd51-136">Specify a name for the web application: **WebApp-GraphAPI-DirectoryExtensions**</span></span>
  * <span data-ttu-id="acd51-137">Alkalmazás típusa: webes alkalmazás/API-t</span><span class="sxs-lookup"><span data-stu-id="acd51-137">Application type: Web app/API</span></span>
  * <span data-ttu-id="acd51-138">Bejelentkezés URL:https://{tenantName}.onmicrosoft.com/WebApp-GraphAPI-DirectoryExtensions</span><span class="sxs-lookup"><span data-stu-id="acd51-138">Sign-on URL:https://{tenantName}.onmicrosoft.com/WebApp-GraphAPI-DirectoryExtensions</span></span>
1. <span data-ttu-id="acd51-139">Válassza ki ** létrehozása.</span><span class="sxs-lookup"><span data-stu-id="acd51-139">Select **Create.</span></span> <span data-ttu-id="acd51-140">Sikeres létrehozása után megjelenik a **értesítések**</span><span class="sxs-lookup"><span data-stu-id="acd51-140">Successful completion appears in the **notifications**</span></span>
1. <span data-ttu-id="acd51-141">Válassza ki az újonnan létrehozott webalkalmazás: **WebApp-GraphAPI-DirectoryExtensions**</span><span class="sxs-lookup"><span data-stu-id="acd51-141">Select the newly created web application: **WebApp-GraphAPI-DirectoryExtensions**</span></span>
1. <span data-ttu-id="acd51-142">Válassza a beállítások: **szükséges engedélyek**</span><span class="sxs-lookup"><span data-stu-id="acd51-142">Select Settings: **Required permissions**</span></span>
1. <span data-ttu-id="acd51-143">Az API lehetőséget választhatja **Windows Active Directory**</span><span class="sxs-lookup"><span data-stu-id="acd51-143">Select API **Windows Active Directory**</span></span>
1. <span data-ttu-id="acd51-144">Jelölje be az Alkalmazásengedélyek: **címtáradatok olvasása és írása**, és **mentése**</span><span class="sxs-lookup"><span data-stu-id="acd51-144">Place a checkmark in Application Permissions: **Read and write directory data**, and **Save**</span></span>
1. <span data-ttu-id="acd51-145">Válasszon **engedélyeket** , majd erősítse meg **Igen**.</span><span class="sxs-lookup"><span data-stu-id="acd51-145">Choose **Grant permissions** and confirm **Yes**.</span></span>
1. <span data-ttu-id="acd51-146">A vágólapra másolja ki és mentse a következő azonosítók a webalkalmazás-GraphAPI-DirectoryExtensions > Beállítások > Tulajdonságok ></span><span class="sxs-lookup"><span data-stu-id="acd51-146">Copy to your clipboard and save the following identifiers from WebApp-GraphAPI-DirectoryExtensions>Settings>Properties></span></span>
*  <span data-ttu-id="acd51-147">**Alkalmazásazonosító** .</span><span class="sxs-lookup"><span data-stu-id="acd51-147">**Application ID** .</span></span> <span data-ttu-id="acd51-148">Példa:`103ee0e6-f92d-4183-b576-8c3739027780`</span><span class="sxs-lookup"><span data-stu-id="acd51-148">Example: `103ee0e6-f92d-4183-b576-8c3739027780`</span></span>
* <span data-ttu-id="acd51-149">**Objektumazonosító:**.</span><span class="sxs-lookup"><span data-stu-id="acd51-149">**Object ID**.</span></span> <span data-ttu-id="acd51-150">Példa:`80d8296a-da0a-49ee-b6ab-fd232aa45201`</span><span class="sxs-lookup"><span data-stu-id="acd51-150">Example: `80d8296a-da0a-49ee-b6ab-fd232aa45201`</span></span>



## <a name="modifying-your-custom-policy-to-add-the-applicationobjectid"></a><span data-ttu-id="acd51-151">Az egyéni házirend hozzáadása a ApplicationObjectId módosítása</span><span class="sxs-lookup"><span data-stu-id="acd51-151">Modifying your custom policy to add the ApplicationObjectId</span></span>

```xml
    <ClaimsProviders>
        <ClaimsProvider>
              <DisplayName>Azure Active Directory</DisplayName>
            <TechnicalProfile Id="AAD-Common">
              <DisplayName>Azure Active Directory</DisplayName>
              <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
              <!-- Provide objectId and appId before using extension properties. -->
              <Metadata>
                <Item Key="ApplicationObjectId">insert objectId here</Item>
                <Item Key="ClientId">insert appId here</Item>
              </Metadata>
            <!-- End of changes -->
              <CryptographicKeys>
                <Key Id="issuer_secret" StorageReferenceId="TokenSigningKeyContainer" />
              </CryptographicKeys>
              <IncludeInSso>false</IncludeInSso>
              <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
            </TechnicalProfile>
        </ClaimsProvider>
    </ClaimsProviders>
```

>[!NOTE]
><span data-ttu-id="acd51-152">A <TechnicalProfile Id="AAD-Common"> nevezzük "általános", mert az elemei szerepel, és használja fel újra az összes az Azure Active Directory TechnicalProfiles az elem használatával:`<IncludeTechnicalProfile ReferenceId="AAD-Common" />`</span><span class="sxs-lookup"><span data-stu-id="acd51-152">The <TechnicalProfile Id="AAD-Common"> is referred to as "common" because its elements are included in and reused in all the Azure Active Directory TechnicalProfiles by using the element: `<IncludeTechnicalProfile ReferenceId="AAD-Common" />`</span></span>

>[!NOTE]
><span data-ttu-id="acd51-153">A TechnicalProfile először írja az újonnan létrehozott bővített tulajdonság, egy egyszeri hibát tapasztalhatnak.</span><span class="sxs-lookup"><span data-stu-id="acd51-153">When the TechnicalProfile writes for the first time to the newly created extension property, you may experience a one-time error.</span></span>  <span data-ttu-id="acd51-154">A bővített tulajdonság jön létre a rendszer először.</span><span class="sxs-lookup"><span data-stu-id="acd51-154">The extension property is created the first time it is used.</span></span>  

## <a name="using-the-new-extension-property--custom-attribute-in-a-user-journey"></a><span data-ttu-id="acd51-155">A bővítmény új tulajdonsággal egyéni attribútum a felhasználó út /</span><span class="sxs-lookup"><span data-stu-id="acd51-155">Using the new extension property / custom attribute in a user journey</span></span>


1. <span data-ttu-id="acd51-156">Nyissa meg a függő Party(RP) fájlt, amely bemutatja a szerkesztő felhasználói út.</span><span class="sxs-lookup"><span data-stu-id="acd51-156">Open the Relying Party(RP) file that describes your policy edit user journey.</span></span>  <span data-ttu-id="acd51-157">Ha indítja, töltse le a már konfigurált RP-PolicyEdit fájl az Azure portál Azure B2C egyéni házirend szakaszából tanácsos lehet.</span><span class="sxs-lookup"><span data-stu-id="acd51-157">If you are starting out, it may be advisable to download your already configured version of the RP-PolicyEdit file directly from the Azure B2C Custom Policy section in the Azure portal.</span></span>  <span data-ttu-id="acd51-158">Azt is megteheti nyissa meg az XML-fájl a tárolási mappából.</span><span class="sxs-lookup"><span data-stu-id="acd51-158">Alternatively, open your XML file from your storage folder.</span></span>
2. <span data-ttu-id="acd51-159">Adja hozzá az egyéni jogcímleírásokat `loyaltyId`.</span><span class="sxs-lookup"><span data-stu-id="acd51-159">Add a custom claim `loyaltyId`.</span></span>  <span data-ttu-id="acd51-160">A jogcímek az egyéni-ot a `<RelyingParty>` elem, a UserJourney TechnicalProfiles átadott paraméterként, és a az alkalmazás a tokenben.</span><span class="sxs-lookup"><span data-stu-id="acd51-160">By including the custom claim in the `<RelyingParty>` element, it is passed as a parameter to the UserJourney TechnicalProfiles and included in the token for the application.</span></span>
```xml
<RelyingParty>
   <DefaultUserJourney ReferenceId="ProfileEdit" />
   <TechnicalProfile Id="PolicyProfile">
     <DisplayName>PolicyProfile</DisplayName>
     <Protocol Name="OpenIdConnect" />
     <OutputClaims>
       <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
       <OutputClaim ClaimTypeReferenceId="city" />

       <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />

     </OutputClaims>
     <SubjectNamingInfo ClaimType="sub" />
   </TechnicalProfile>
 </RelyingParty>
 ```
3. <span data-ttu-id="acd51-161">A bővítményfájl házirend hozzáadása egy jogcím-definíció `TrustFrameworkExtensions.xml` belül a `<ClaimsSchema>` elem látható módon.</span><span class="sxs-lookup"><span data-stu-id="acd51-161">Add a claim definition to the Extension policy file  `TrustFrameworkExtensions.xml` inside the `<ClaimsSchema>` element as shown.</span></span>
```xml
<ClaimsSchema>
        <ClaimType Id="extension_loyaltyId">
            <DisplayName>Loyalty Identification Tag</DisplayName>
            <DataType>string</DataType>
            <UserHelpText>Your loyalty number from your membership card</UserHelpText>
            <UserInputType>TextBox</UserInputType>
        </ClaimType>
</ClaimsSchema>
```
4. <span data-ttu-id="acd51-162">Adja hozzá ugyanazt az alap házirendfájl-definíciót a jogcím `TrustFrameworkBase.xml`.</span><span class="sxs-lookup"><span data-stu-id="acd51-162">Add the same claim definition to the Base policy file `TrustFrameworkBase.xml`.</span></span>  
><span data-ttu-id="acd51-163">Hozzáadás a `ClaimType` az alap- és a bővítmények fájl definíciójában általában nem szükség, azonban a következő lépéseket a extension_loyaltyId felveszi az alap fájlban TechnicalProfiles, mert a házirend-érvényesítő elutasítják az alap fájl feltöltése nélkül.</span><span class="sxs-lookup"><span data-stu-id="acd51-163">Adding a `ClaimType` definition in both the base and the extensions file is normally not necessary, however since the next steps will add the extension_loyaltyId to TechnicalProfiles in the Base file, the policy validator will reject the upload of the base file without it.</span></span>
><span data-ttu-id="acd51-164">Az a TrustFrameworkBase.xml fájlban a "ProfileEdit" nevű felhasználó út végrehajtása nyomkövetéséhez hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="acd51-164">It may be useful to trace the execution of the user journey named "ProfileEdit" in the TrustFrameworkBase.xml file.</span></span>  <span data-ttu-id="acd51-165">Keresse meg a felhasználó út a szerkesztőben azonos nevű, és figyelje meg, hogy az Orchestration 5. lépés meghívja a TechnicalProfileReferenceID = "SelfAsserted-ProfileUpdate".</span><span class="sxs-lookup"><span data-stu-id="acd51-165">Search for the user journey of the same name in your editor and observe that Orchestration Step 5 invokes the TechnicalProfileReferenceID="SelfAsserted-ProfileUpdate".</span></span>  <span data-ttu-id="acd51-166">Keresse meg és vizsgálja meg a TechnicalProfile, és ismerje meg az a folyamat.</span><span class="sxs-lookup"><span data-stu-id="acd51-166">Search and inspect this TechnicalProfile to familiarize yourself with the flow.</span></span>
5. <span data-ttu-id="acd51-167">Adja hozzá a loyaltyId jogcímként bemeneti és kimeneti a a TechnicalProfile "SelfAsserted-ProfileUpdate"</span><span class="sxs-lookup"><span data-stu-id="acd51-167">Add loyaltyId as input and output claim in the TechnicalProfile "SelfAsserted-ProfileUpdate"</span></span>
```xml
<TechnicalProfile Id="SelfAsserted-ProfileUpdate">
          <DisplayName>User ID signup</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
          <Metadata>
            <Item Key="ContentDefinitionReferenceId">api.selfasserted.profileupdate</Item>
          </Metadata>
          <IncludeInSso>false</IncludeInSso>
          <InputClaims>

            <InputClaim ClaimTypeReferenceId="alternativeSecurityId" />
            <InputClaim ClaimTypeReferenceId="userPrincipalName" />

            <!-- Optional claims. These claims are collected from the user and can be modified. Any claim added here should be updated in the
                 ValidationTechnicalProfile referenced below so it can be written to directory after being updateed by the user, i.e. AAD-UserWriteProfileUsingObjectId. -->
            <InputClaim ClaimTypeReferenceId="givenName" />
            <InputClaim ClaimTypeReferenceId="surname" />
            <InputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
          </InputClaims>
          <OutputClaims>
            <!-- Required claims -->
            <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />

            <!-- Optional claims. These claims are collected from the user and can be modified. Any claim added here should be updated in the
                 ValidationTechnicalProfile referenced below so it can be written to directory after being updateed by the user, i.e. AAD-UserWriteProfileUsingObjectId. -->
            <OutputClaim ClaimTypeReferenceId="givenName" />
            <OutputClaim ClaimTypeReferenceId="surname" />
            <OutputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
          </OutputClaims>
          <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="AAD-UserWriteProfileUsingObjectId" />
          </ValidationTechnicalProfiles>
        </TechnicalProfile>
```
6. <span data-ttu-id="acd51-168">Jogcím hozzáadása a TechnicalProfile "AAD-UserWriteProfileUsingObjectId" megőrizni az aktuális felhasználó a címtárban a bővített tulajdonság a jogcím értéke.</span><span class="sxs-lookup"><span data-stu-id="acd51-168">Add claim in TechnicalProfile "AAD-UserWriteProfileUsingObjectId" to persist the value of the claim in the extension property, for the current user in the directory.</span></span>
```xml
<TechnicalProfile Id="AAD-UserWriteProfileUsingObjectId">
          <Metadata>
            <Item Key="Operation">Write</Item>
            <Item Key="RaiseErrorIfClaimsPrincipalAlreadyExists">false</Item>
            <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
          </Metadata>
          <IncludeInSso>false</IncludeInSso>
          <InputClaims>
            <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
          </InputClaims>
          <PersistedClaims>
            <!-- Required claims -->
            <PersistedClaim ClaimTypeReferenceId="objectId" />

            <!-- Optional claims -->
            <PersistedClaim ClaimTypeReferenceId="givenName" />
            <PersistedClaim ClaimTypeReferenceId="surname" />
            <PersistedClaim ClaimTypeReferenceId="extension_loyaltyId" />

          </PersistedClaims>
          <IncludeTechnicalProfile ReferenceId="AAD-Common" />
        </TechnicalProfile>
```
7. <span data-ttu-id="acd51-169">Jogcím hozzáadása a TechnicalProfile "AAD-UserReadUsingObjectId" a mellék attribútum értékének olvasásához minden alkalommal, amikor a felhasználó jelentkezik be.</span><span class="sxs-lookup"><span data-stu-id="acd51-169">Add claim in TechnicalProfile "AAD-UserReadUsingObjectId" to read the value of the extension attribute every time a user logs in.</span></span> <span data-ttu-id="acd51-170">A TechnicalProfiles eddigi csak a helyi fiókok folyamata megváltoztak.</span><span class="sxs-lookup"><span data-stu-id="acd51-170">Thus far the TechnicalProfiles have been changed in the flow of local accounts only.</span></span>  <span data-ttu-id="acd51-171">Ha az új attribútumhoz társadalombiztosítási/összevont fiók folyamata van szükség, TechnicalProfiles különböző szabálykészleteket kell módosítani.</span><span class="sxs-lookup"><span data-stu-id="acd51-171">If the new attribute is desired in the flow of a social/federated account, a different set of TechnicalProfiles needs to be changed.</span></span> <span data-ttu-id="acd51-172">Tekintse meg a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="acd51-172">See Next Steps.</span></span>

```xml
<!-- The following technical profile is used to read data after user authenticates. -->
     <TechnicalProfile Id="AAD-UserReadUsingObjectId">
       <Metadata>
         <Item Key="Operation">Read</Item>
         <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
       </Metadata>
       <IncludeInSso>false</IncludeInSso>
       <InputClaims>
         <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
       </InputClaims>
       <OutputClaims>
         <!-- Optional claims -->
         <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
         <OutputClaim ClaimTypeReferenceId="displayName" />
         <OutputClaim ClaimTypeReferenceId="otherMails" />
         <OutputClaim ClaimTypeReferenceId="givenName" />
         <OutputClaim ClaimTypeReferenceId="surname" />
         <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />
       </OutputClaims>
       <IncludeTechnicalProfile ReferenceId="AAD-Common" />
     </TechnicalProfile>
```


>[!IMPORTANT]
><span data-ttu-id="acd51-173">A IncludeTechnicalProfile elem hozzáadása a TechnicalProfile az AAD-közös minden elemét.</span><span class="sxs-lookup"><span data-stu-id="acd51-173">The IncludeTechnicalProfile element adds all the elements of AAD-Common to this TechnicalProfile.</span></span>

## <a name="test-the-custom-policy-using-run-now"></a><span data-ttu-id="acd51-174">Az egyéni házirend használatával "Futtatás most" tesztelése</span><span class="sxs-lookup"><span data-stu-id="acd51-174">Test the custom policy using "Run Now"</span></span>
1. <span data-ttu-id="acd51-175">Nyissa meg a **panel az Azure AD B2C** , és keresse meg **identitás élmény keretrendszer > egyéni házirendek**.</span><span class="sxs-lookup"><span data-stu-id="acd51-175">Open the **Azure AD B2C Blade** and navigate to **Identity Experience Framework > Custom policies**.</span></span>
1. <span data-ttu-id="acd51-176">Válassza ki az egyéni házirendet, feltöltött, majd kattintson a **futtatása most** gombra.</span><span class="sxs-lookup"><span data-stu-id="acd51-176">Select the custom policy that you uploaded, and click the **Run now** button.</span></span>
1. <span data-ttu-id="acd51-177">Iratkozhat fel e-mail cím használatával kell lennie.</span><span class="sxs-lookup"><span data-stu-id="acd51-177">You should be able to sign up using an email address.</span></span>

<span data-ttu-id="acd51-178">Az azonosító tokent küldött vissza az alkalmazásba az új bővített tulajdonság extension_loyaltyId utasításnak egyéni jogcímként magában foglalja.</span><span class="sxs-lookup"><span data-stu-id="acd51-178">The  id token sent back to your application includes the new extension property as a custom claim preceded by extension_loyaltyId.</span></span> <span data-ttu-id="acd51-179">Lásd a példát.</span><span class="sxs-lookup"><span data-stu-id="acd51-179">See example.</span></span>

```
{
  "exp": 1493585187,
  "nbf": 1493581587,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "a58e7c6c-7535-4074-93da-b0023fbaf3ac",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustframeworkprofileedit",
  "nonce": "defaultNonce",
  "iat": 1493581587,
  "auth_time": 1493581587,
  "extension_loyaltyId": "abc",
  "city": "Redmond"
}
```

## <a name="next-steps"></a><span data-ttu-id="acd51-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="acd51-180">Next steps</span></span>

<span data-ttu-id="acd51-181">Adja hozzá a közösségi fiók bejelentkezések során a folyamatok az új jogcím felsorolt TechnicalProfiles módosításával.</span><span class="sxs-lookup"><span data-stu-id="acd51-181">Add the new claim to the flows for social account logins by changing the TechnicalProfiles listed.</span></span> <span data-ttu-id="acd51-182">E két TechnicalProfiles írható és olvasható a felhasználói adatokat a alternativeSecurityId használja, mint a lokátor felhasználói objektum társadalombiztosítási/összevont fiók bejelentkezések használják.</span><span class="sxs-lookup"><span data-stu-id="acd51-182">These two TechnicalProfiles are used by social/federated account logins to write and read the user data using the alternativeSecurityId as the locator of the user object.</span></span>
```
  <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">

  <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```

<span data-ttu-id="acd51-183">Beépített és egyéni házirendek közötti azonos kiterjesztési attribútumot használja.</span><span class="sxs-lookup"><span data-stu-id="acd51-183">Using the same extension attributes between built-in and custom policies.</span></span>
<span data-ttu-id="acd51-184">Amikor kiterjesztési attribútumot (más néven egyéni attribútumok) keresztül a portál élményt, azok használatával regisztrált a ** b2c-bővítmények-alkalmazást, amely minden b2c-bérlő szerepel.</span><span class="sxs-lookup"><span data-stu-id="acd51-184">When you add extension attributes (aka custom attributes) via the portal experience, those attributes are registered using the **b2c-extensions-app that exists in every b2c tenant.</span></span>  <span data-ttu-id="acd51-185">Ezeket a bővítményattribútumokat használatához az egyéni házirendek:</span><span class="sxs-lookup"><span data-stu-id="acd51-185">To use these extension attributes in your custom policy:</span></span>
1. <span data-ttu-id="acd51-186">Lépjen a b2c bérlő portal.azure.com belül **Azure Active Directory** válassza **App regisztrációk**</span><span class="sxs-lookup"><span data-stu-id="acd51-186">Within your b2c tenant in portal.azure.com, navigate to **Azure Active Directory** and select **App registrations**</span></span>
2. <span data-ttu-id="acd51-187">Keresés a **b2c-bővítmények-alkalmazás** , és jelölje ki</span><span class="sxs-lookup"><span data-stu-id="acd51-187">Find your **b2c-extensions-app** and select it</span></span>
3. <span data-ttu-id="acd51-188">A "Essentials" rekord a **Alkalmazásazonosító** és a **objektum azonosítója**</span><span class="sxs-lookup"><span data-stu-id="acd51-188">Under 'Essentials' record the **Application ID** and the **Object ID**</span></span>
4. <span data-ttu-id="acd51-189">Tartalmazza azokat az AAD-gyakori technikai profil metaadatai között, például a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="acd51-189">Include them in your AAD-Common Technical profile metadata like as follows:</span></span>

```xml
    <ClaimsProviders>
        <ClaimsProvider>
              <DisplayName>Azure Active Directory</DisplayName>
            <TechnicalProfile Id="AAD-Common">
              <DisplayName>Azure Active Directory</DisplayName>
              <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
              <!-- Provide objectId and appId before using extension properties. -->
              <Metadata>
                <Item Key="ApplicationObjectId">insert objectId here</Item> <!-- This is the "Object ID" from the "b2c-extensions-app"-->
                <Item Key="ClientId">insert appId here</Item> <!--This is the "Application ID" from the "b2c-extensions-app"-->
              </Metadata>
```

<span data-ttu-id="acd51-190">A portál nyújthassunk konzisztencia fenntartása, hozzon létre a portál felhasználói felületének használatával ezek az attribútumok *előtt* azokat az egyéni házirendeket használ.</span><span class="sxs-lookup"><span data-stu-id="acd51-190">To keep consistency with the portal experience, create these attributes using the portal UI *before* you use them in your custom policies.</span></span>  <span data-ttu-id="acd51-191">Amikor létrehoz egy attribútum "ActivationStatus" a portálon, meg kell hivatkozik rá az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="acd51-191">When you create an attribute "ActivationStatus" in the portal, you must refer to it as follows:</span></span>

```
extension_ActivationStatus in the custom policy
extension_<app-guid>_ActivationStatus via the Graph API.
```


## <a name="reference"></a><span data-ttu-id="acd51-192">Referencia</span><span class="sxs-lookup"><span data-stu-id="acd51-192">Reference</span></span>

* <span data-ttu-id="acd51-193">A **műszaki profil (TP)** egy elem típus, amely-re, egy *függvény* , amely definiál egy végpont nevét, a metaadatait, a protokollal, és a cseréjének részletezi, amelyek az identitás Felhasználói élmény keretrendszer végre kell hajtania.</span><span class="sxs-lookup"><span data-stu-id="acd51-193">A **Technical Profile (TP)** is an element type that can be thought of as a *function* that defines an endpoint’s name, its metadata, its protocol, and details the exchange of claims that the Identity Experience Framework should perform.</span></span>  <span data-ttu-id="acd51-194">Ha ez *függvény* az orchestration lépésben neve, vagy egy másik TechnicalProfile, a InputClaims és OutputClaims vannak megadva, a paraméterek a hívó által.</span><span class="sxs-lookup"><span data-stu-id="acd51-194">When this *function* is called in an orchestration step or from another TechnicalProfile, the InputClaims and OutputClaims are provided as parameters by the caller.</span></span>


* <span data-ttu-id="acd51-195">A bővítmény tulajdonságai teljes kezelését, tekintse meg a cikket [DIRECTORY-SÉMA bővítményei |} GRAPH API FOGALMAK](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)</span><span class="sxs-lookup"><span data-stu-id="acd51-195">For full treatment on extension properties, see the article [DIRECTORY SCHEMA EXTENSIONS | GRAPH API CONCEPTS](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)</span></span>

>[!NOTE]
><span data-ttu-id="acd51-196">A Graph API a bővítményattribútumokat megnevezett az konvenció `extension_ApplicationObjectID_attributename`.</span><span class="sxs-lookup"><span data-stu-id="acd51-196">Extension attributes in Graph API are named using the convention `extension_ApplicationObjectID_attributename`.</span></span> <span data-ttu-id="acd51-197">Bővítmények attribútumok extension_attributename, így kihagyása az XML ApplicationObjectId lesz az egyéni házirendek</span><span class="sxs-lookup"><span data-stu-id="acd51-197">Custom policies refer to extensions attributes as extension_attributename, thus omitting the ApplicationObjectId in the XML</span></span>
