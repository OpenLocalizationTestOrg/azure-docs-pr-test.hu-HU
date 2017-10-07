---
title: "Az Azure Active Directory B2C: Saját attribútumok toocustom szabályzatok, és használja a profil szerkesztése |} Microsoft Docs"
description: "A forgatókönyv a bővítmény tulajdonságok, egyéni attribútumok használatát, és így azok hello felhasználói felületen"
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
ms.openlocfilehash: 8cc9c6a38d7652797ba54a3e02078ac2bf4a693b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-creating-and-using-custom-attributes-in-a-custom-profile-edit-policy"></a><span data-ttu-id="490a4-103">Az Azure Active Directory B2C: Létrehozása és az egyéni attribútumok használata egy egyéni profilt a házirend szerkesztése</span><span class="sxs-lookup"><span data-stu-id="490a4-103">Azure Active Directory B2C: Creating and using custom attributes in a custom profile edit policy</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="490a4-104">Ebben a cikkben egy egyéni attribútum létrehozása a Azure AD B2C-címtárban, és egy egyéni jogcím hello profil szerkesztése felhasználói út ezt az új attribútumot használja.</span><span class="sxs-lookup"><span data-stu-id="490a4-104">In this article, you create a custom attribute in your Azure AD B2C directory and use this new attribute as a custom claim in hello profile edit user journey.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="490a4-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="490a4-105">Prerequisites</span></span>

<span data-ttu-id="490a4-106">Teljes hello hello cikkben ismertetett visszaállítási lépésekkel [Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="490a4-106">Complete hello steps in hello article [Getting Started with Custom Policies](active-directory-b2c-get-started-custom.md).</span></span>

## <a name="use-custom-attributes-toocollect-information-about-your-customers-in-azure-active-directory-b2c-using-custom-policies"></a><span data-ttu-id="490a4-107">Az ügyfelek az Azure Active Directory B2C egyéni házirendekkel toocollect információt az egyéni attribútumok használata</span><span class="sxs-lookup"><span data-stu-id="490a4-107">Use custom attributes toocollect information about your customers in Azure Active Directory B2C using custom policies</span></span>
<span data-ttu-id="490a4-108">Azure Active Directory (Azure AD) B2C-címtárban tartalmaz egy beépített attribútumok: megadott név, Vezetéknév, város, irányítószám, userPrincipalName, stb.  Gyakran kell toocreate saját attribútumok.</span><span class="sxs-lookup"><span data-stu-id="490a4-108">Your Azure Active Directory (Azure AD) B2C directory comes with a built-in set of attributes: Given Name, Surname, City, Postal Code, userPrincipalName, etc.  You often need toocreate your own attributes.</span></span>  <span data-ttu-id="490a4-109">Példa:</span><span class="sxs-lookup"><span data-stu-id="490a4-109">For example:</span></span>
* <span data-ttu-id="490a4-110">Egy ügyfélkapcsolati alkalmazást kell toopersist egy attribútum, például a "LoyaltyNumber."</span><span class="sxs-lookup"><span data-stu-id="490a4-110">A customer-facing application needs toopersist an attribute such as "LoyaltyNumber."</span></span>
* <span data-ttu-id="490a4-111">Az identitásszolgáltató rendelkezik egy egyedi felhasználói azonosító, amelyet kell menteni, például a "uniqueUserGUID." "</span><span class="sxs-lookup"><span data-stu-id="490a4-111">An identity provider has a unique user identifier that must be saved such as "uniqueUserGUID.""</span></span>
* <span data-ttu-id="490a4-112">Egyéni felhasználói út kell toopersist hello állapotának felhasználó például "migrationStatus."</span><span class="sxs-lookup"><span data-stu-id="490a4-112">A custom user journey needs toopersist hello state of user such as "migrationStatus."</span></span>

<span data-ttu-id="490a4-113">Az Azure AD B2C-ben tárolt felhasználói attribútumok készletét hello bővítheti.</span><span class="sxs-lookup"><span data-stu-id="490a4-113">With Azure AD B2C, you can extend hello set of attributes stored on each user account.</span></span> <span data-ttu-id="490a4-114">Is olvasási és írási ezek az attribútumok hello segítségével [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="490a4-114">You can also read and write these attributes by using hello [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).</span></span>

<span data-ttu-id="490a4-115">Bővítmény tulajdonságai hello felhasználói objektum hello könyvtárban hello-séma kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="490a4-115">Extension properties extend hello schema of hello user objects in hello directory.</span></span>  <span data-ttu-id="490a4-116">hello feltételek bővített tulajdonság, az egyéni attribútum és az egyéni jogcímleírásokat tekintse meg a toohello ugyanaz a cikk és hello név hello környezetében hello környezetben (alkalmazás, objektum, a házirend) függ.</span><span class="sxs-lookup"><span data-stu-id="490a4-116">hello terms extension property, custom attribute and custom claim refer toohello same thing in hello context of this article and hello name varies depending on hello context (application, object, policy).</span></span>

<span data-ttu-id="490a4-117">Bővítmény tulajdonságai csak regisztrálható az alkalmazásobjektum, annak ellenére, hogy egy felhasználó lehet, hogy adatokat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="490a4-117">Extension properties can only be registered on an Application object even though they may contain data for a User.</span></span> <span data-ttu-id="490a4-118">hello tulajdonság csatolt toohello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="490a4-118">hello property is attached toohello application.</span></span> <span data-ttu-id="490a4-119">hello alkalmazásobjektum megadott írási tooregister egy bővített tulajdonság kell lennie.</span><span class="sxs-lookup"><span data-stu-id="490a4-119">hello Application object must be granted write access tooregister an extension property.</span></span> <span data-ttu-id="490a4-120">100 bővítmény tulajdonságai (közötti összes típusa és az összes alkalmazás) csak írható tooany egyetlen objektumhoz.</span><span class="sxs-lookup"><span data-stu-id="490a4-120">100 Extension properties (across ALL types and ALL applications) can be written tooany single object.</span></span> <span data-ttu-id="490a4-121">Bővítmény tulajdonságai toohello directory céltípus kerülnek, és azonnal elérhetővé hello Azure AD B2C directory-bérlőben.</span><span class="sxs-lookup"><span data-stu-id="490a4-121">Extension properties are added toohello target directory type and becomes immediately accessible in hello Azure AD B2C directory tenant.</span></span>
<span data-ttu-id="490a4-122">Hello alkalmazás törlésekor az összes felhasználó számára a bennük található adatokat ilyen bővítmény tulajdonságok is törlődnek.</span><span class="sxs-lookup"><span data-stu-id="490a4-122">If hello application is deleted, those Extension properties along with any data contained in them for all users are also removed.</span></span> <span data-ttu-id="490a4-123">Egy bővített tulajdonság hello alkalmazás törlése, ha eltávolítják azt a hello címtárobjektumok célként, és törölni értékek hello.</span><span class="sxs-lookup"><span data-stu-id="490a4-123">If an extension property is deleted by hello Application, it is removed on hello target directory objects, and hello values deleted.</span></span>

<span data-ttu-id="490a4-124">Bővítmény tulajdonságai csak a hello kontextusában hello bérlő regisztrált alkalmazás szerepel.</span><span class="sxs-lookup"><span data-stu-id="490a4-124">Extension properties exist only in hello context of a registered  Application in hello tenant.</span></span> <span data-ttu-id="490a4-125">hello objektumazonosító alkalmazás, az azt használó TechnicalProfile hello kell szerepelnie.</span><span class="sxs-lookup"><span data-stu-id="490a4-125">hello object id of that Application must be included in hello TechnicalProfile that use it.</span></span>

>[!NOTE]
><span data-ttu-id="490a4-126">hello Azure AD B2C directory többek között a webes alkalmazás neve `b2c-extensions-app`.</span><span class="sxs-lookup"><span data-stu-id="490a4-126">hello Azure AD B2C directory typically includes a Web App named `b2c-extensions-app`.</span></span>  <span data-ttu-id="490a4-127">Ez az alkalmazás elsősorban a hello hello Azure-portálon létrehozott egyéni jogcímek hello b2c beépített házirendjei használják.</span><span class="sxs-lookup"><span data-stu-id="490a4-127">This application is primarily used by hello b2c built-in  policies for hello custom claims created via hello Azure portal.</span></span>  <span data-ttu-id="490a4-128">Az alkalmazás tooregister bővítmények b2c egyéni házirendek használatával csak haladó felhasználóknak javasolt.</span><span class="sxs-lookup"><span data-stu-id="490a4-128">Using this application tooregister extensions for b2c custom policies is recommended only for advanced users.</span></span>  <span data-ttu-id="490a4-129">Ehhez útmutatást hello további lépések című részben szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="490a4-129">Instructions for this are included in hello Next Steps section in this article.</span></span>


## <a name="creating-a-new-application-toostore-hello-extension-properties"></a><span data-ttu-id="490a4-130">Egy új alkalmazás toostore hello bővítmény tulajdonságai létrehozása</span><span class="sxs-lookup"><span data-stu-id="490a4-130">Creating a new application toostore hello extension properties</span></span>

1. <span data-ttu-id="490a4-131">Nyissa meg a böngésző munkamenetet, és keresse meg a toohello [Azure porta](https://portal.azure.com) és jelentkezzen be rendszergazdai hitelesítő adataival hello tooconfigure kívánja B2C-címtárban.</span><span class="sxs-lookup"><span data-stu-id="490a4-131">Open a browsing session and navigate toohello [Azure porta](https://portal.azure.com) and sign in with administrative credentials of hello B2C Directory you wish tooconfigure.</span></span>
1. <span data-ttu-id="490a4-132">Kattintson a **Azure Active Directory** hello bal oldali navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="490a4-132">Click **Azure Active Directory** on hello left navigation menu.</span></span> <span data-ttu-id="490a4-133">Szükség lehet az kiválasztásával további szolgáltatások toofind >.</span><span class="sxs-lookup"><span data-stu-id="490a4-133">You may need toofind it by selecting More services>.</span></span>
1. <span data-ttu-id="490a4-134">Válassza ki **App regisztrációk** kattintson **új alkalmazás regisztrációja**</span><span class="sxs-lookup"><span data-stu-id="490a4-134">Select **App registrations** and click **New application registration**</span></span>
1. <span data-ttu-id="490a4-135">Adja meg a hello következő ajánlott bejegyzéseket:</span><span class="sxs-lookup"><span data-stu-id="490a4-135">Provide hello following recommended entries:</span></span>
  * <span data-ttu-id="490a4-136">Adjon meg egy nevet a webalkalmazás hello: **WebApp-GraphAPI-DirectoryExtensions**</span><span class="sxs-lookup"><span data-stu-id="490a4-136">Specify a name for hello web application: **WebApp-GraphAPI-DirectoryExtensions**</span></span>
  * <span data-ttu-id="490a4-137">Alkalmazás típusa: webes alkalmazás/API-t</span><span class="sxs-lookup"><span data-stu-id="490a4-137">Application type: Web app/API</span></span>
  * <span data-ttu-id="490a4-138">Bejelentkezés URL:https://{tenantName}.onmicrosoft.com/WebApp-GraphAPI-DirectoryExtensions</span><span class="sxs-lookup"><span data-stu-id="490a4-138">Sign-on URL:https://{tenantName}.onmicrosoft.com/WebApp-GraphAPI-DirectoryExtensions</span></span>
1. <span data-ttu-id="490a4-139">Válassza ki ** létrehozása.</span><span class="sxs-lookup"><span data-stu-id="490a4-139">Select **Create.</span></span> <span data-ttu-id="490a4-140">Sikeres létrehozása után megjelennek a hello **értesítések**</span><span class="sxs-lookup"><span data-stu-id="490a4-140">Successful completion appears in hello **notifications**</span></span>
1. <span data-ttu-id="490a4-141">Válassza ki az újonnan létrehozott hello webalkalmazás: **WebApp-GraphAPI-DirectoryExtensions**</span><span class="sxs-lookup"><span data-stu-id="490a4-141">Select hello newly created web application: **WebApp-GraphAPI-DirectoryExtensions**</span></span>
1. <span data-ttu-id="490a4-142">Válassza a beállítások: **szükséges engedélyek**</span><span class="sxs-lookup"><span data-stu-id="490a4-142">Select Settings: **Required permissions**</span></span>
1. <span data-ttu-id="490a4-143">Az API lehetőséget választhatja **Windows Active Directory**</span><span class="sxs-lookup"><span data-stu-id="490a4-143">Select API **Windows Active Directory**</span></span>
1. <span data-ttu-id="490a4-144">Jelölje be az Alkalmazásengedélyek: **címtáradatok olvasása és írása**, és **mentése**</span><span class="sxs-lookup"><span data-stu-id="490a4-144">Place a checkmark in Application Permissions: **Read and write directory data**, and **Save**</span></span>
1. <span data-ttu-id="490a4-145">Válasszon **engedélyeket** , majd erősítse meg **Igen**.</span><span class="sxs-lookup"><span data-stu-id="490a4-145">Choose **Grant permissions** and confirm **Yes**.</span></span>
1. <span data-ttu-id="490a4-146">Tooyour vágólapra másolja ki és mentse a webalkalmazás-GraphAPI-DirectoryExtensions azonosítók következő hello > Beállítások > Tulajdonságok ></span><span class="sxs-lookup"><span data-stu-id="490a4-146">Copy tooyour clipboard and save hello following identifiers from WebApp-GraphAPI-DirectoryExtensions>Settings>Properties></span></span>
*  <span data-ttu-id="490a4-147">**Alkalmazásazonosító** .</span><span class="sxs-lookup"><span data-stu-id="490a4-147">**Application ID** .</span></span> <span data-ttu-id="490a4-148">Példa:`103ee0e6-f92d-4183-b576-8c3739027780`</span><span class="sxs-lookup"><span data-stu-id="490a4-148">Example: `103ee0e6-f92d-4183-b576-8c3739027780`</span></span>
* <span data-ttu-id="490a4-149">**Objektumazonosító:**.</span><span class="sxs-lookup"><span data-stu-id="490a4-149">**Object ID**.</span></span> <span data-ttu-id="490a4-150">Példa:`80d8296a-da0a-49ee-b6ab-fd232aa45201`</span><span class="sxs-lookup"><span data-stu-id="490a4-150">Example: `80d8296a-da0a-49ee-b6ab-fd232aa45201`</span></span>



## <a name="modifying-your-custom-policy-tooadd-hello-applicationobjectid"></a><span data-ttu-id="490a4-151">Az egyéni házirend tooadd hello ApplicationObjectId módosítása</span><span class="sxs-lookup"><span data-stu-id="490a4-151">Modifying your custom policy tooadd hello ApplicationObjectId</span></span>

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
><span data-ttu-id="490a4-152">Hello <TechnicalProfile Id="AAD-Common"> hivatkozott tooas "általános" azért, mert az elemei szerepel, és használja fel újra az összes hello Azure Active Directory TechnicalProfiles hello elem használatával:`<IncludeTechnicalProfile ReferenceId="AAD-Common" />`</span><span class="sxs-lookup"><span data-stu-id="490a4-152">hello <TechnicalProfile Id="AAD-Common"> is referred tooas "common" because its elements are included in and reused in all hello Azure Active Directory TechnicalProfiles by using hello element: `<IncludeTechnicalProfile ReferenceId="AAD-Common" />`</span></span>

>[!NOTE]
><span data-ttu-id="490a4-153">Hello TechnicalProfile írási műveleteknél hello első alkalommal az újonnan létrehozott toohello bővítmény tulajdonság egy egyszeri hibát tapasztalhatnak.</span><span class="sxs-lookup"><span data-stu-id="490a4-153">When hello TechnicalProfile writes for hello first time toohello newly created extension property, you may experience a one-time error.</span></span>  <span data-ttu-id="490a4-154">hello bővített tulajdonság hello jön létre a rendszer első alkalommal.</span><span class="sxs-lookup"><span data-stu-id="490a4-154">hello extension property is created hello first time it is used.</span></span>  

## <a name="using-hello-new-extension-property--custom-attribute-in-a-user-journey"></a><span data-ttu-id="490a4-155">Hello új bővítmény tulajdonság használatával / egyéni attribútum a felhasználó út</span><span class="sxs-lookup"><span data-stu-id="490a4-155">Using hello new extension property / custom attribute in a user journey</span></span>


1. <span data-ttu-id="490a4-156">Nyissa meg hello Party(RP) függő fájl, amely leírja a házirend szerkesztése felhasználói út.</span><span class="sxs-lookup"><span data-stu-id="490a4-156">Open hello Relying Party(RP) file that describes your policy edit user journey.</span></span>  <span data-ttu-id="490a4-157">Ha indítja, ajánlott toodownload hello RP-PolicyEdit már konfigurált verziójának közvetlenül hello hello Azure portál Azure B2C egyéni házirend szakasz fájlt lehet.</span><span class="sxs-lookup"><span data-stu-id="490a4-157">If you are starting out, it may be advisable toodownload your already configured version of hello RP-PolicyEdit file directly from hello Azure B2C Custom Policy section in hello Azure portal.</span></span>  <span data-ttu-id="490a4-158">Azt is megteheti nyissa meg az XML-fájl a tárolási mappából.</span><span class="sxs-lookup"><span data-stu-id="490a4-158">Alternatively, open your XML file from your storage folder.</span></span>
2. <span data-ttu-id="490a4-159">Adja hozzá az egyéni jogcímleírásokat `loyaltyId`.</span><span class="sxs-lookup"><span data-stu-id="490a4-159">Add a custom claim `loyaltyId`.</span></span>  <span data-ttu-id="490a4-160">Hello a jogcím-ot hello egyéni `<RelyingParty>` elem, mint egy paraméterrel toohello UserJourney TechnicalProfiles átadott, és a hello tokenben hello alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="490a4-160">By including hello custom claim in hello `<RelyingParty>` element, it is passed as a parameter toohello UserJourney TechnicalProfiles and included in hello token for hello application.</span></span>
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
3. <span data-ttu-id="490a4-161">Adjon hozzá egy jogcím definition toohello bővítmény házirend fájlt `TrustFrameworkExtensions.xml` belül hello `<ClaimsSchema>` elem látható módon.</span><span class="sxs-lookup"><span data-stu-id="490a4-161">Add a claim definition toohello Extension policy file  `TrustFrameworkExtensions.xml` inside hello `<ClaimsSchema>` element as shown.</span></span>
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
4. <span data-ttu-id="490a4-162">Adja hozzá a hello azonos jogcím-definíció toohello alap házirendfájl `TrustFrameworkBase.xml`.</span><span class="sxs-lookup"><span data-stu-id="490a4-162">Add hello same claim definition toohello Base policy file `TrustFrameworkBase.xml`.</span></span>  
><span data-ttu-id="490a4-163">Hozzáadás a `ClaimType` definition hello talál és hello bővítmények fájl általában nem szükség, azonban hello lépések hello extension_loyaltyId tooTechnicalProfiles hello alap fájlt adja hozzá, mivel hello házirend érvényesítési elutasítják hello feltöltése hello alap fájl nélkül.</span><span class="sxs-lookup"><span data-stu-id="490a4-163">Adding a `ClaimType` definition in both hello base and hello extensions file is normally not necessary, however since hello next steps will add hello extension_loyaltyId tooTechnicalProfiles in hello Base file, hello policy validator will reject hello upload of hello base file without it.</span></span>
><span data-ttu-id="490a4-164">Hasznos tootrace hello végrehajtási hello felhasználói út nevű hello TrustFrameworkBase.xml fájl "ProfileEdit" lehet.</span><span class="sxs-lookup"><span data-stu-id="490a4-164">It may be useful tootrace hello execution of hello user journey named "ProfileEdit" in hello TrustFrameworkBase.xml file.</span></span>  <span data-ttu-id="490a4-165">Ugyanaz a szerkesztőben nevet, és tekintse meg az hív meg, hogy az Orchestration 5. lépés hello TechnicalProfileReferenceID hello hello felhasználói út keresése = "SelfAsserted-ProfileUpdate".</span><span class="sxs-lookup"><span data-stu-id="490a4-165">Search for hello user journey of hello same name in your editor and observe that Orchestration Step 5 invokes hello TechnicalProfileReferenceID="SelfAsserted-ProfileUpdate".</span></span>  <span data-ttu-id="490a4-166">Keresse meg és vizsgálja meg a TechnicalProfile toofamiliarize saját kezűleg a hello folyamata.</span><span class="sxs-lookup"><span data-stu-id="490a4-166">Search and inspect this TechnicalProfile toofamiliarize yourself with hello flow.</span></span>
5. <span data-ttu-id="490a4-167">Adja hozzá a loyaltyId jogcímként bemeneti és kimeneti a hello TechnicalProfile "SelfAsserted-ProfileUpdate"</span><span class="sxs-lookup"><span data-stu-id="490a4-167">Add loyaltyId as input and output claim in hello TechnicalProfile "SelfAsserted-ProfileUpdate"</span></span>
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

            <!-- Optional claims. These claims are collected from hello user and can be modified. Any claim added here should be updated in the
                 ValidationTechnicalProfile referenced below so it can be written toodirectory after being updateed by hello user, i.e. AAD-UserWriteProfileUsingObjectId. -->
            <InputClaim ClaimTypeReferenceId="givenName" />
            <InputClaim ClaimTypeReferenceId="surname" />
            <InputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
          </InputClaims>
          <OutputClaims>
            <!-- Required claims -->
            <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />

            <!-- Optional claims. These claims are collected from hello user and can be modified. Any claim added here should be updated in the
                 ValidationTechnicalProfile referenced below so it can be written toodirectory after being updateed by hello user, i.e. AAD-UserWriteProfileUsingObjectId. -->
            <OutputClaim ClaimTypeReferenceId="givenName" />
            <OutputClaim ClaimTypeReferenceId="surname" />
            <OutputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
          </OutputClaims>
          <ValidationTechnicalProfiles>
            <ValidationTechnicalProfile ReferenceId="AAD-UserWriteProfileUsingObjectId" />
          </ValidationTechnicalProfiles>
        </TechnicalProfile>
```
6. <span data-ttu-id="490a4-168">Jogcím hozzáadása a "AAD-UserWriteProfileUsingObjectId" TechnicalProfile toopersist hello érték hello jogcím hello bővített tulajdonság, az aktuális felhasználó hello hello könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="490a4-168">Add claim in TechnicalProfile "AAD-UserWriteProfileUsingObjectId" toopersist hello value of hello claim in hello extension property, for hello current user in hello directory.</span></span>
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
7. <span data-ttu-id="490a4-169">Adja hozzá a jogcím TechnicalProfile "AAD-UserReadUsingObjectId" tooread hello hello bővítmény attribútum értékének a minden alkalommal, amikor a felhasználó jelentkezik be.</span><span class="sxs-lookup"><span data-stu-id="490a4-169">Add claim in TechnicalProfile "AAD-UserReadUsingObjectId" tooread hello value of hello extension attribute every time a user logs in.</span></span> <span data-ttu-id="490a4-170">Így sokkal hello folyamat csak a helyi fiókok hello TechnicalProfiles megváltoztak.</span><span class="sxs-lookup"><span data-stu-id="490a4-170">Thus far hello TechnicalProfiles have been changed in hello flow of local accounts only.</span></span>  <span data-ttu-id="490a4-171">Hello új attribútum társadalombiztosítási/összevont fiók hello folyamatában van szükség, ha a TechnicalProfiles különböző szabálykészleteket toobe módosítani kell.</span><span class="sxs-lookup"><span data-stu-id="490a4-171">If hello new attribute is desired in hello flow of a social/federated account, a different set of TechnicalProfiles needs toobe changed.</span></span> <span data-ttu-id="490a4-172">Tekintse meg a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="490a4-172">See Next Steps.</span></span>

```xml
<!-- hello following technical profile is used tooread data after user authenticates. -->
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
><span data-ttu-id="490a4-173">hello IncludeTechnicalProfile elem hozzáadása az AAD-közös toothis TechnicalProfile összes hello eleme.</span><span class="sxs-lookup"><span data-stu-id="490a4-173">hello IncludeTechnicalProfile element adds all hello elements of AAD-Common toothis TechnicalProfile.</span></span>

## <a name="test-hello-custom-policy-using-run-now"></a><span data-ttu-id="490a4-174">Teszt hello egyéni házirend használatával "Futtatás most"</span><span class="sxs-lookup"><span data-stu-id="490a4-174">Test hello custom policy using "Run Now"</span></span>
1. <span data-ttu-id="490a4-175">Nyissa meg hello **panel az Azure AD B2C** , és keresse meg a túl**identitás élmény keretrendszer > egyéni házirendek**.</span><span class="sxs-lookup"><span data-stu-id="490a4-175">Open hello **Azure AD B2C Blade** and navigate too**Identity Experience Framework > Custom policies**.</span></span>
1. <span data-ttu-id="490a4-176">Válassza ki a feltöltött hello egyéni házirendet, majd kattintson a hello **futtatása most** gombra.</span><span class="sxs-lookup"><span data-stu-id="490a4-176">Select hello custom policy that you uploaded, and click hello **Run now** button.</span></span>
1. <span data-ttu-id="490a4-177">Meg kell tudni toosign be egy e-mail címet.</span><span class="sxs-lookup"><span data-stu-id="490a4-177">You should be able toosign up using an email address.</span></span>

<span data-ttu-id="490a4-178">hello azonosító tokent küldött vissza tooyour alkalmazás hello új bővített tulajdonság tartalmaz egy egyéni jogcímként extension_loyaltyId utasításnak.</span><span class="sxs-lookup"><span data-stu-id="490a4-178">hello  id token sent back tooyour application includes hello new extension property as a custom claim preceded by extension_loyaltyId.</span></span> <span data-ttu-id="490a4-179">Lásd a példát.</span><span class="sxs-lookup"><span data-stu-id="490a4-179">See example.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="490a4-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="490a4-180">Next steps</span></span>

<span data-ttu-id="490a4-181">Adja hozzá a hello új jogcímet toohello adatfolyamok a közösségi fiók bejelentkezések során TechnicalProfiles felsorolt hello módosításával.</span><span class="sxs-lookup"><span data-stu-id="490a4-181">Add hello new claim toohello flows for social account logins by changing hello TechnicalProfiles listed.</span></span> <span data-ttu-id="490a4-182">E két TechnicalProfiles társadalombiztosítási/összevont fiók bejelentkezések toowrite használják, és hello alternativeSecurityId használatával, mint a lokátor hello felhasználói objektum hello hello felhasználói adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="490a4-182">These two TechnicalProfiles are used by social/federated account logins toowrite and read hello user data using hello alternativeSecurityId as hello locator of hello user object.</span></span>
```
  <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">

  <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
```

<span data-ttu-id="490a4-183">Használatával hello beépített és egyéni házirendek közötti azonos kiterjesztési attribútumot.</span><span class="sxs-lookup"><span data-stu-id="490a4-183">Using hello same extension attributes between built-in and custom policies.</span></span>
<span data-ttu-id="490a4-184">Amikor kiterjesztési attribútumot (más néven egyéni attribútumok) keresztül hello portál élmény, azok hello segítségével regisztrált ** b2c-bővítmények-alkalmazást, amely minden b2c-bérlő szerepel.</span><span class="sxs-lookup"><span data-stu-id="490a4-184">When you add extension attributes (aka custom attributes) via hello portal experience, those attributes are registered using hello **b2c-extensions-app that exists in every b2c tenant.</span></span>  <span data-ttu-id="490a4-185">toouse ezeket a bővítményattribútumokat, az egyéni házirendek:</span><span class="sxs-lookup"><span data-stu-id="490a4-185">toouse these extension attributes in your custom policy:</span></span>
1. <span data-ttu-id="490a4-186">A b2c-bérlő a portal.azure.com, Ugrás túl**Azure Active Directory** válassza **App regisztrációk**</span><span class="sxs-lookup"><span data-stu-id="490a4-186">Within your b2c tenant in portal.azure.com, navigate too**Azure Active Directory** and select **App registrations**</span></span>
2. <span data-ttu-id="490a4-187">Keresés a **b2c-bővítmények-alkalmazás** , és jelölje ki</span><span class="sxs-lookup"><span data-stu-id="490a4-187">Find your **b2c-extensions-app** and select it</span></span>
3. <span data-ttu-id="490a4-188">A "Essentials" rekord hello **Alkalmazásazonosító** és hello **objektum azonosítója**</span><span class="sxs-lookup"><span data-stu-id="490a4-188">Under 'Essentials' record hello **Application ID** and hello **Object ID**</span></span>
4. <span data-ttu-id="490a4-189">Tartalmazza azokat az AAD-gyakori technikai profil metaadatai között, például a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="490a4-189">Include them in your AAD-Common Technical profile metadata like as follows:</span></span>

```xml
    <ClaimsProviders>
        <ClaimsProvider>
              <DisplayName>Azure Active Directory</DisplayName>
            <TechnicalProfile Id="AAD-Common">
              <DisplayName>Azure Active Directory</DisplayName>
              <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
              <!-- Provide objectId and appId before using extension properties. -->
              <Metadata>
                <Item Key="ApplicationObjectId">insert objectId here</Item> <!-- This is hello "Object ID" from hello "b2c-extensions-app"-->
                <Item Key="ClientId">insert appId here</Item> <!--This is hello "Application ID" from hello "b2c-extensions-app"-->
              </Metadata>
```

<span data-ttu-id="490a4-190">tookeep konzisztencia az hello portál révén, ezek az attribútumok hello portál felhasználói felületének használatával hozzon létre *előtt* azokat az egyéni házirendeket használ.</span><span class="sxs-lookup"><span data-stu-id="490a4-190">tookeep consistency with hello portal experience, create these attributes using hello portal UI *before* you use them in your custom policies.</span></span>  <span data-ttu-id="490a4-191">Amikor létrehoz egy attribútum "ActivationStatus" hello portálon, az alábbiak szerint tooit kell hivatkoznia:</span><span class="sxs-lookup"><span data-stu-id="490a4-191">When you create an attribute "ActivationStatus" in hello portal, you must refer tooit as follows:</span></span>

```
extension_ActivationStatus in hello custom policy
extension_<app-guid>_ActivationStatus via hello Graph API.
```


## <a name="reference"></a><span data-ttu-id="490a4-192">Referencia</span><span class="sxs-lookup"><span data-stu-id="490a4-192">Reference</span></span>

* <span data-ttu-id="490a4-193">A **műszaki profil (TP)** -re mint elemtípuson van egy *függvény* , amely meghatározza egy végpont nevét, a metaadatait, a protokollal, és a részletek hello cseréjének, amely identitás hello Felhasználói élmény keretrendszer végre kell hajtania.</span><span class="sxs-lookup"><span data-stu-id="490a4-193">A **Technical Profile (TP)** is an element type that can be thought of as a *function* that defines an endpoint’s name, its metadata, its protocol, and details hello exchange of claims that hello Identity Experience Framework should perform.</span></span>  <span data-ttu-id="490a4-194">Ha ez *függvény* egy vezénylési lépés vagy egy másik TechnicalProfile, InputClaims és OutputClaims vannak megadva, a paraméterek hello hívó hello nevezik.</span><span class="sxs-lookup"><span data-stu-id="490a4-194">When this *function* is called in an orchestration step or from another TechnicalProfile, hello InputClaims and OutputClaims are provided as parameters by hello caller.</span></span>


* <span data-ttu-id="490a4-195">A bővítmény tulajdonságai teljes kezelés cikke hello [DIRECTORY-SÉMA bővítményei |} GRAPH API FOGALMAK](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)</span><span class="sxs-lookup"><span data-stu-id="490a4-195">For full treatment on extension properties, see hello article [DIRECTORY SCHEMA EXTENSIONS | GRAPH API CONCEPTS](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)</span></span>

>[!NOTE]
><span data-ttu-id="490a4-196">A Graph API a bővítményattribútumokat megnevezett hello konvenció `extension_ApplicationObjectID_attributename`.</span><span class="sxs-lookup"><span data-stu-id="490a4-196">Extension attributes in Graph API are named using hello convention `extension_ApplicationObjectID_attributename`.</span></span> <span data-ttu-id="490a4-197">Egyéni házirendek tooextensions attribútumok extension_attributename, így kihagyásával hello ApplicationObjectId a hello XML, tekintse meg a</span><span class="sxs-lookup"><span data-stu-id="490a4-197">Custom policies refer tooextensions attributes as extension_attributename, thus omitting hello ApplicationObjectId in hello XML</span></span>
