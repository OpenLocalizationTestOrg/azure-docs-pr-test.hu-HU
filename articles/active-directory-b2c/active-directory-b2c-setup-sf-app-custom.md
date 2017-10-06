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
# <a name="azure-active-directory-b2c-sign-in-by-using-salesforce-accounts-via-saml"></a><span data-ttu-id="c84b0-103">Az Azure Active Directory B2C: Salesforce-fiókok keresztül SAML használatával írja alá</span><span class="sxs-lookup"><span data-stu-id="c84b0-103">Azure Active Directory B2C: Sign in by using Salesforce accounts via SAML</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="c84b0-104">Ez a cikk bemutatja, hogyan toouse [egyéni házirendek](active-directory-b2c-overview-custom.md) tooset mentése bejelentkezhet a felhasználók egy adott Salesforce szervezettől származik.</span><span class="sxs-lookup"><span data-stu-id="c84b0-104">This article shows you how toouse [custom policies](active-directory-b2c-overview-custom.md) tooset up sign-in for users from a specific Salesforce organization.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c84b0-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c84b0-105">Prerequisites</span></span>

### <a name="azure-ad-b2c-setup"></a><span data-ttu-id="c84b0-106">Az Azure AD B2C beállítása</span><span class="sxs-lookup"><span data-stu-id="c84b0-106">Azure AD B2C setup</span></span>

<span data-ttu-id="c84b0-107">Győződjön meg arról, hogy végrehajtotta hello lépéseket, amelyek bemutatják, hogyan túl[Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md) az Azure Active Directory B2C (Azure AD B2C-vel).</span><span class="sxs-lookup"><span data-stu-id="c84b0-107">Ensure that you have completed all of hello steps that show you how too[get started with custom policies](active-directory-b2c-get-started-custom.md) in Azure Active Directory B2C (Azure AD B2C).</span></span>

<span data-ttu-id="c84b0-108">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="c84b0-108">These include:</span></span>

* <span data-ttu-id="c84b0-109">Az Azure AD B2C bérlő létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c84b0-109">Create an Azure AD B2C tenant.</span></span>
* <span data-ttu-id="c84b0-110">Az Azure AD B2C alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c84b0-110">Create an Azure AD B2C application.</span></span>
* <span data-ttu-id="c84b0-111">Két házirend motor alkalmazás regisztrálásához.</span><span class="sxs-lookup"><span data-stu-id="c84b0-111">Register two policy engine applications.</span></span>
* <span data-ttu-id="c84b0-112">Kulcsok beállítása.</span><span class="sxs-lookup"><span data-stu-id="c84b0-112">Set up keys.</span></span>
* <span data-ttu-id="c84b0-113">Hello alapszintű csomag beállításához.</span><span class="sxs-lookup"><span data-stu-id="c84b0-113">Set up hello starter pack.</span></span>

### <a name="salesforce-setup"></a><span data-ttu-id="c84b0-114">Salesforce-telepítő</span><span class="sxs-lookup"><span data-stu-id="c84b0-114">Salesforce setup</span></span>

<span data-ttu-id="c84b0-115">Ebben a cikkben azt feltételezzük, hogy már elvégezte hello következő:</span><span class="sxs-lookup"><span data-stu-id="c84b0-115">In this article, we assume that you have already done hello following:</span></span>

* <span data-ttu-id="c84b0-116">A Salesforce-fiókhoz való regisztráció.</span><span class="sxs-lookup"><span data-stu-id="c84b0-116">Signed up for a Salesforce account.</span></span> <span data-ttu-id="c84b0-117">Regisztrálhatnak az egy [ingyenes Developer Edition fiókot](https://developer.salesforce.com/signup).</span><span class="sxs-lookup"><span data-stu-id="c84b0-117">You can sign up for a [free Developer Edition account](https://developer.salesforce.com/signup).</span></span>
* <span data-ttu-id="c84b0-118">[Saját tartomány beállítása](https://help.salesforce.com/articleView?id=domain_name_setup.htm&language=en_US&type=0) Salesforce szervezete számára.</span><span class="sxs-lookup"><span data-stu-id="c84b0-118">[Set up a My Domain](https://help.salesforce.com/articleView?id=domain_name_setup.htm&language=en_US&type=0) for your Salesforce organization.</span></span>

## <a name="set-up-salesforce-so-users-can-federate"></a><span data-ttu-id="c84b0-119">Így a felhasználók is összevonni Salesforce beállítása</span><span class="sxs-lookup"><span data-stu-id="c84b0-119">Set up Salesforce so users can federate</span></span>

<span data-ttu-id="c84b0-120">Salesforce folytatott kommunikációhoz az Azure AD B2C toohelp, tooget hello Salesforce metaadatainak URL-CÍMÉT kell.</span><span class="sxs-lookup"><span data-stu-id="c84b0-120">toohelp Azure AD B2C communicate with Salesforce, you need tooget hello Salesforce metadata URL.</span></span>

### <a name="set-up-salesforce-as-an-identity-provider"></a><span data-ttu-id="c84b0-121">Identitás-szolgáltatóként Salesforce beállítása</span><span class="sxs-lookup"><span data-stu-id="c84b0-121">Set up Salesforce as an identity provider</span></span>

> [!NOTE]
> <span data-ttu-id="c84b0-122">Ebben a cikkben azt feltételezzük, hogy azok be [Salesforce Lightning élmény](https://developer.salesforce.com/page/Lightning_Experience_FAQ).</span><span class="sxs-lookup"><span data-stu-id="c84b0-122">In this article, we assume you are using [Salesforce Lightning Experience](https://developer.salesforce.com/page/Lightning_Experience_FAQ).</span></span>

1. <span data-ttu-id="c84b0-123">[Jelentkezzen be tooSalesforce](https://login.salesforce.com/).</span><span class="sxs-lookup"><span data-stu-id="c84b0-123">[Sign in tooSalesforce](https://login.salesforce.com/).</span></span> 
2. <span data-ttu-id="c84b0-124">Hello a bal oldali menü alatti **beállítások**, bontsa ki a **identitás**, és kattintson a **identitásszolgáltató**.</span><span class="sxs-lookup"><span data-stu-id="c84b0-124">On hello left menu, under **Settings**, expand **Identity**, and then click **Identity Provider**.</span></span>
3. <span data-ttu-id="c84b0-125">Kattintson a **identitásszolgáltató engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="c84b0-125">Click **Enable Identity Provider**.</span></span>
4. <span data-ttu-id="c84b0-126">A **válassza hello tanúsítvány**, jelölje be azt szeretné, hogy Salesforce toouse toocommunicate Azure AD B2C hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="c84b0-126">Under **Select hello certificate**, select hello certificate you want Salesforce toouse toocommunicate with Azure AD B2C.</span></span> <span data-ttu-id="c84b0-127">(Hello alapértelmezett tanúsítvány is használható.) Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="c84b0-127">(You can use hello default certificate.) Click **Save**.</span></span> 

### <a name="create-a-connected-app-in-salesforce"></a><span data-ttu-id="c84b0-128">Hozzon létre egy csatlakoztatott alkalmazáshoz a Salesforce-ban</span><span class="sxs-lookup"><span data-stu-id="c84b0-128">Create a connected app in Salesforce</span></span>

1. <span data-ttu-id="c84b0-129">A hello **identitásszolgáltató** lapon, nyissa meg túl**szolgáltatók**.</span><span class="sxs-lookup"><span data-stu-id="c84b0-129">On hello **Identity Provider** page, go too**Service Providers**.</span></span>
2. <span data-ttu-id="c84b0-130">Kattintson a **szolgáltatók létrejön keresztül csatlakozó alkalmazásokat. Kattintson ide.**</span><span class="sxs-lookup"><span data-stu-id="c84b0-130">Click **Service Providers are now created via Connected Apps. Click here.**</span></span>
3. <span data-ttu-id="c84b0-131">A **alapvető információkat**, adja meg a csatlakoztatott alkalmazáshoz tartozó hello szükséges értékeket.</span><span class="sxs-lookup"><span data-stu-id="c84b0-131">Under **Basic Information**,  enter hello required values for your connected app.</span></span>
4. <span data-ttu-id="c84b0-132">A **a webalkalmazás-beállítások**, jelölje be hello **SAML engedélyezése** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="c84b0-132">Under **Web App Settings**, select hello **Enable SAML** check box.</span></span>
5. <span data-ttu-id="c84b0-133">A hello **Entitásazonosító** mezőbe írja be a következő URL-cím hello.</span><span class="sxs-lookup"><span data-stu-id="c84b0-133">In hello **Entity ID** field, enter hello following URL.</span></span> <span data-ttu-id="c84b0-134">Győződjön meg arról, hogy cserélje le a hello értéke `tenantName`.</span><span class="sxs-lookup"><span data-stu-id="c84b0-134">Ensure that you replace hello value for `tenantName`.</span></span>
      ```
      https://login.microsoftonline.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase
      ```
6. <span data-ttu-id="c84b0-135">A hello **ACS URL-cím** mezőbe írja be a következő URL-cím hello.</span><span class="sxs-lookup"><span data-stu-id="c84b0-135">In hello **ACS URL** field, enter hello following URL.</span></span> <span data-ttu-id="c84b0-136">Győződjön meg arról, hogy cserélje le a hello értéke `tenantName`.</span><span class="sxs-lookup"><span data-stu-id="c84b0-136">Ensure that you replace hello value for `tenantName`.</span></span>
      ```
      https://login.microsoftonline.com/te/tenantName.onmicrosoft.com/B2C_1A_TrustFrameworkBase/samlp/sso/assertionconsumer
      ```
7. <span data-ttu-id="c84b0-137">Hagyja meg a többi beállítás hello alapértelmezett értékeit.</span><span class="sxs-lookup"><span data-stu-id="c84b0-137">Leave hello default values for all other settings.</span></span>
8. <span data-ttu-id="c84b0-138">Görgessen toohello hello lista aljára, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="c84b0-138">Scroll toohello bottom of hello list, and then click **Save**.</span></span>

### <a name="get-hello-metadata-url"></a><span data-ttu-id="c84b0-139">Hello metaadatainak URL-cím beszerzése</span><span class="sxs-lookup"><span data-stu-id="c84b0-139">Get hello metadata URL</span></span>

1. <span data-ttu-id="c84b0-140">Kattintson az hello áttekintése lapon a csatlakoztatott alkalmazás **kezelése**.</span><span class="sxs-lookup"><span data-stu-id="c84b0-140">On hello overview page of your connected app, click **Manage**.</span></span>
2. <span data-ttu-id="c84b0-141">Másolja a hello értéke **metaadatok Discovery Endpoint**, és mentse azt.</span><span class="sxs-lookup"><span data-stu-id="c84b0-141">Copy hello value for **Metadata Discovery Endpoint**, and then save it.</span></span> <span data-ttu-id="c84b0-142">Ez a cikk későbbi részében fogja használni.</span><span class="sxs-lookup"><span data-stu-id="c84b0-142">You'll use it later in this article.</span></span>

### <a name="set-up-salesforce-users-toofederate"></a><span data-ttu-id="c84b0-143">Salesforce felhasználók toofederate beállítása</span><span class="sxs-lookup"><span data-stu-id="c84b0-143">Set up Salesforce users toofederate</span></span>

1. <span data-ttu-id="c84b0-144">A hello **kezelése** lap a csatlakoztatott alkalmazáshoz, nyissa meg túl**profilok**.</span><span class="sxs-lookup"><span data-stu-id="c84b0-144">On hello **Manage** page of your connected app, go too**Profiles**.</span></span>
2. <span data-ttu-id="c84b0-145">Kattintson a **profilok kezeléséhez**.</span><span class="sxs-lookup"><span data-stu-id="c84b0-145">Click **Manage Profiles**.</span></span>
3. <span data-ttu-id="c84b0-146">Válassza ki a hello profilok (vagy felhasználói csoportok), amelyet az Azure AD B2C toofederate.</span><span class="sxs-lookup"><span data-stu-id="c84b0-146">Select hello profiles (or groups of users) that you want toofederate with Azure AD B2C.</span></span> <span data-ttu-id="c84b0-147">Rendszergazdaként, válassza ki a hello **rendszergazda** jelölőnégyzetet, így akkor is összevonva a Salesforce-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="c84b0-147">As a system administrator, select hello **System Administrator** check box, so that you can federate by using your Salesforce account.</span></span>

## <a name="generate-a-signing-certificate-for-azure-ad-b2c"></a><span data-ttu-id="c84b0-148">Aláíró tanúsítvány jön létre az Azure AD B2C-vel</span><span class="sxs-lookup"><span data-stu-id="c84b0-148">Generate a signing certificate for Azure AD B2C</span></span>

<span data-ttu-id="c84b0-149">Kérelmek küldése tooSalesforce kell toobe az Azure AD B2C által aláírt.</span><span class="sxs-lookup"><span data-stu-id="c84b0-149">Requests sent tooSalesforce need toobe signed by Azure AD B2C.</span></span> <span data-ttu-id="c84b0-150">toogenerate egy aláíró tanúsítványt, nyissa meg az Azure PowerShell, és futtassa a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="c84b0-150">toogenerate a signing certificate, open Azure PowerShell, and then run hello following commands.</span></span>

> [!NOTE]
> <span data-ttu-id="c84b0-151">Ellenőrizze, hogy frissítse hello bérlő neve és a jelszó hello felső két sort.</span><span class="sxs-lookup"><span data-stu-id="c84b0-151">Make sure that you update hello tenant name and password in hello top two lines.</span></span>

```PowerShell
$tenantName = "<YOUR TENANT NAME>.onmicrosoft.com"
$pwdText = "<YOUR PASSWORD HERE>"

$Cert = New-SelfSignedCertificate -CertStoreLocation Cert:\CurrentUser\My -DnsName "SamlIdp.$tenantName" -Subject "B2C SAML Signing Cert" -HashAlgorithm SHA256 -KeySpec Signature -KeyLength 2048

$pwd = ConvertTo-SecureString -String $pwdText -Force -AsPlainText

Export-PfxCertificate -Cert $Cert -FilePath .\B2CSigningCert.pfx -Password $pwd
```

## <a name="add-hello-saml-signing-certificate-tooazure-ad-b2c"></a><span data-ttu-id="c84b0-152">Hello SAML aláíró tanúsítvány tooAzure AD B2C hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c84b0-152">Add hello SAML signing certificate tooAzure AD B2C</span></span>

<span data-ttu-id="c84b0-153">Töltse fel az aláíró tanúsítvány tooyour az Azure AD B2C bérlő hello:</span><span class="sxs-lookup"><span data-stu-id="c84b0-153">Upload hello signing certificate tooyour Azure AD B2C tenant:</span></span> 

1. <span data-ttu-id="c84b0-154">Nyissa meg tooyour az Azure AD B2C-bérlő.</span><span class="sxs-lookup"><span data-stu-id="c84b0-154">Go tooyour Azure AD B2C tenant.</span></span> <span data-ttu-id="c84b0-155">Kattintson a **beállítások** > **identitás élmény keretrendszer** > **házirend kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="c84b0-155">Click **Settings** > **Identity Experience Framework** > **Policy Keys**.</span></span>
2. <span data-ttu-id="c84b0-156">Kattintson a **+ Hozzáadás**, majd:</span><span class="sxs-lookup"><span data-stu-id="c84b0-156">Click **+Add**, and then:</span></span>
    1. <span data-ttu-id="c84b0-157">Kattintson a **beállítások** > **feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="c84b0-157">Click **Options** > **Upload**.</span></span>
    2. <span data-ttu-id="c84b0-158">Adjon meg egy **neve** (például SAMLSigningCert).</span><span class="sxs-lookup"><span data-stu-id="c84b0-158">Enter a **Name** (for example, SAMLSigningCert).</span></span> <span data-ttu-id="c84b0-159">hello előtag *B2C_1A_* a rendszer automatikusan hozzáadja a kulcs toohello neve.</span><span class="sxs-lookup"><span data-stu-id="c84b0-159">hello prefix *B2C_1A_* is automatically added toohello name of your key.</span></span>
    3. <span data-ttu-id="c84b0-160">tooselect a tanúsítványt, jelölje be **vezérlő feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="c84b0-160">tooselect your certificate, select **upload file control**.</span></span> 
    4. <span data-ttu-id="c84b0-161">Adja meg a hello PowerShell-parancsfájl a megadott hello tanúsítvány jelszavát.</span><span class="sxs-lookup"><span data-stu-id="c84b0-161">Enter hello certificate's password that you set in hello PowerShell script.</span></span>
3. <span data-ttu-id="c84b0-162">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c84b0-162">Click **Create**.</span></span>
4. <span data-ttu-id="c84b0-163">Győződjön meg arról, hogy létrehozott egy kulcs (például B2C_1A_SAMLSigningCert).</span><span class="sxs-lookup"><span data-stu-id="c84b0-163">Verify that you've created a key (for example, B2C_1A_SAMLSigningCert).</span></span> <span data-ttu-id="c84b0-164">Jegyezze fel a teljes név hello (beleértve a *B2C_1A_*).</span><span class="sxs-lookup"><span data-stu-id="c84b0-164">Take note of hello full name (including *B2C_1A_*).</span></span> <span data-ttu-id="c84b0-165">Később hello házirend toothis kulcsot fogja hivatkozunk.</span><span class="sxs-lookup"><span data-stu-id="c84b0-165">You will refer toothis key later in hello policy.</span></span>

## <a name="create-hello-salesforce-saml-claims-provider-in-your-base-policy"></a><span data-ttu-id="c84b0-166">A kiinduló házirend hello Salesforce SAML jogcím-szolgáltató létrehozása</span><span class="sxs-lookup"><span data-stu-id="c84b0-166">Create hello Salesforce SAML claims provider in your base policy</span></span>

<span data-ttu-id="c84b0-167">Toodefine Salesforce kell egy jogcímszolgáltatótól, így a felhasználók bejelentkezés Salesforce használatával.</span><span class="sxs-lookup"><span data-stu-id="c84b0-167">You need toodefine Salesforce as a claims provider so users can sign in by using Salesforce.</span></span> <span data-ttu-id="c84b0-168">Ez azt jelenti az Azure AD B2C kommunikáló toospecify hello végpont van szüksége.</span><span class="sxs-lookup"><span data-stu-id="c84b0-168">In other words, you need toospecify hello endpoint that Azure AD B2C will communicate with.</span></span> <span data-ttu-id="c84b0-169">hello végpont fog *meg* készlete *jogcímek* , hogy az Azure AD B2C használ, amely egy adott felhasználó hitelesítette tooverify.</span><span class="sxs-lookup"><span data-stu-id="c84b0-169">hello endpoint will *provide* a set of *claims* that Azure AD B2C uses tooverify that a specific user has authenticated.</span></span> <span data-ttu-id="c84b0-170">toodo, vegye fel a `<ClaimsProvider>` a Salesforce fájljában hello bővítmény a házirend:</span><span class="sxs-lookup"><span data-stu-id="c84b0-170">toodo this, add a `<ClaimsProvider>` for Salesforce in hello extension file of your policy:</span></span>

1. <span data-ttu-id="c84b0-171">Nyissa meg a munkakönyvtár hello bővítményfájl (TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="c84b0-171">In your working directory, open hello extension file (TrustFrameworkExtensions.xml).</span></span>
2. <span data-ttu-id="c84b0-172">Hello található `<ClaimsProviders>` szakasz.</span><span class="sxs-lookup"><span data-stu-id="c84b0-172">Find hello `<ClaimsProviders>` section.</span></span> <span data-ttu-id="c84b0-173">Ha még nem létezik, hozzon létre hello legfelső szintű csomópontja alatt.</span><span class="sxs-lookup"><span data-stu-id="c84b0-173">If it does not exist, create it under hello root node.</span></span>
3. <span data-ttu-id="c84b0-174">Adjon hozzá egy új `<ClaimsProvider>`:</span><span class="sxs-lookup"><span data-stu-id="c84b0-174">Add a new `<ClaimsProvider>`:</span></span>

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

<span data-ttu-id="c84b0-175">A hello `<ClaimsProvider>` csomópont:</span><span class="sxs-lookup"><span data-stu-id="c84b0-175">Under hello `<ClaimsProvider>` node:</span></span>

1. <span data-ttu-id="c84b0-176">Hello értékének módosításához `<Domain>` tooa egyedi érték, amely megkülönbözteti `<ClaimsProvider>` az egyéb identitás-szolgáltatóktól származó.</span><span class="sxs-lookup"><span data-stu-id="c84b0-176">Change hello value for `<Domain>` tooa unique value that distinguishes `<ClaimsProvider>` from other identity providers.</span></span>
2. <span data-ttu-id="c84b0-177">Frissítse a hello értéket `<DisplayName>` tooa megjelenítési név hello jogcím-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="c84b0-177">Update hello value for `<DisplayName>` tooa display name for hello claims provider.</span></span> <span data-ttu-id="c84b0-178">Jelenleg ez az érték nem használt.</span><span class="sxs-lookup"><span data-stu-id="c84b0-178">Currently, this value is not used.</span></span>

### <a name="update-hello-technical-profile"></a><span data-ttu-id="c84b0-179">Műszaki hello-profil frissítése</span><span class="sxs-lookup"><span data-stu-id="c84b0-179">Update hello technical profile</span></span>

<span data-ttu-id="c84b0-180">a Salesforce, SAML-jogkivonatból tooget hello protokollok, hogy az Azure AD B2C toocommunicate használja az Azure Active Directoryval (Azure AD) határozza meg.</span><span class="sxs-lookup"><span data-stu-id="c84b0-180">tooget a SAML token from Salesforce, define hello protocols that Azure AD B2C will use toocommunicate with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="c84b0-181">Ehhez a hello `<TechnicalProfile>` eleme `<ClaimsProvider>`:</span><span class="sxs-lookup"><span data-stu-id="c84b0-181">Do this in hello `<TechnicalProfile>` element of `<ClaimsProvider>`:</span></span>

1. <span data-ttu-id="c84b0-182">Hello hello azonosítója frissítése `<TechnicalProfile>` csomópont.</span><span class="sxs-lookup"><span data-stu-id="c84b0-182">Update hello ID of hello `<TechnicalProfile>` node.</span></span> <span data-ttu-id="c84b0-183">Ez az azonosító nem használt toorefer toothis műszaki profil hello házirend egyéb részeitől.</span><span class="sxs-lookup"><span data-stu-id="c84b0-183">This ID is used toorefer toothis technical profile from other parts of hello policy.</span></span>
2. <span data-ttu-id="c84b0-184">Frissítse a hello értéket `<DisplayName>`.</span><span class="sxs-lookup"><span data-stu-id="c84b0-184">Update hello value for `<DisplayName>`.</span></span> <span data-ttu-id="c84b0-185">Ez az érték jelenik meg a bejelentkezési lapon hello bejelentkezési gombra.</span><span class="sxs-lookup"><span data-stu-id="c84b0-185">This value is displayed on hello sign-in button on your sign-in page.</span></span>
3. <span data-ttu-id="c84b0-186">Frissítse a hello értéket `<Description>`.</span><span class="sxs-lookup"><span data-stu-id="c84b0-186">Update hello value for `<Description>`.</span></span>
4. <span data-ttu-id="c84b0-187">Salesforce hello SAML 2.0-s protokollt használja.</span><span class="sxs-lookup"><span data-stu-id="c84b0-187">Salesforce uses hello SAML 2.0 protocol.</span></span> <span data-ttu-id="c84b0-188">Győződjön meg arról, hogy hello értéke `<Protocol>` van **egy SAML2**.</span><span class="sxs-lookup"><span data-stu-id="c84b0-188">Ensure that hello value for `<Protocol>` is **SAML2**.</span></span>

<span data-ttu-id="c84b0-189">Frissítés hello `<Metadata>` XML tooreflect hello-beállítások a megadott Salesforce-fiókhoz megelőző hello című szakasza.</span><span class="sxs-lookup"><span data-stu-id="c84b0-189">Update hello `<Metadata>` section in hello preceding XML tooreflect hello settings for your specific Salesforce account.</span></span> <span data-ttu-id="c84b0-190">Hello XML, a frissítés hello metaadatok:</span><span class="sxs-lookup"><span data-stu-id="c84b0-190">In hello XML, update hello metadata values:</span></span>

1. <span data-ttu-id="c84b0-191">Hello értékét `<Item Key="PartnerEntity">` hello Salesforce metaadatainak URL-korábban kimásolt.</span><span class="sxs-lookup"><span data-stu-id="c84b0-191">Update hello value of `<Item Key="PartnerEntity">` with hello Salesforce metadata URL you copied earlier.</span></span> <span data-ttu-id="c84b0-192">A következő formátumban hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="c84b0-192">It has hello following format:</span></span> 

    `https://contoso-dev-ed.my.salesforce.com/.well-known/samlidp/connectedapp.xml`

2. <span data-ttu-id="c84b0-193">A hello `<CryptographicKeys>` részben, a frissítés hello érték mindkét példányai `StorageReferenceId` hello kulcs az aláíró tanúsítványban (például B2C_1A_SAMLSigningCert) toohello neve.</span><span class="sxs-lookup"><span data-stu-id="c84b0-193">In hello `<CryptographicKeys>` section, update hello value for both instances of `StorageReferenceId` toohello name of hello key of your signing certificate (for example, B2C_1A_SAMLSigningCert).</span></span>

### <a name="upload-hello-extension-file-for-verification"></a><span data-ttu-id="c84b0-194">Az ellenőrzéshez hello kiterjesztésű fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="c84b0-194">Upload hello extension file for verification</span></span>

<span data-ttu-id="c84b0-195">A házirend most úgy van beállítva, hogy az Azure AD B2C tudja, hogyan toocommunicate a Salesforce.</span><span class="sxs-lookup"><span data-stu-id="c84b0-195">Your policy is now configured so that Azure AD B2C knows how toocommunicate with Salesforce.</span></span> <span data-ttu-id="c84b0-196">Próbálja meg a házirend, hogy nincs probléma merül fel, amennyiben tooverify hello kiterjesztésű fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="c84b0-196">Try uploading hello extension file of your policy, tooverify that there aren't any issues so far.</span></span> <span data-ttu-id="c84b0-197">tooupload hello kiterjesztésű fájlt a házirend:</span><span class="sxs-lookup"><span data-stu-id="c84b0-197">tooupload hello extension file of your policy:</span></span>

1. <span data-ttu-id="c84b0-198">Lépjen az Azure AD B2C-bérlő toohello **házirend** panelen.</span><span class="sxs-lookup"><span data-stu-id="c84b0-198">In your Azure AD B2C tenant, go toohello **All Policies** blade.</span></span>
2. <span data-ttu-id="c84b0-199">Jelölje be hello **hello házirend felülírja, ha létezik** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="c84b0-199">Select hello **Overwrite hello policy if it exists** check box.</span></span>
3. <span data-ttu-id="c84b0-200">Töltse fel hello kiterjesztésű fájlt (TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="c84b0-200">Upload hello extension file (TrustFrameworkExtensions.xml).</span></span> <span data-ttu-id="c84b0-201">Győződjön meg arról, hogy azt nem érvényesítése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="c84b0-201">Ensure that it doesn't fail validation.</span></span>

## <a name="register-hello-salesforce-saml-claims-provider-tooa-user-journey"></a><span data-ttu-id="c84b0-202">Hello Salesforce SAML jogcímeket szolgáltató tooa felhasználói út regisztrálása</span><span class="sxs-lookup"><span data-stu-id="c84b0-202">Register hello Salesforce SAML claims provider tooa user journey</span></span>

<span data-ttu-id="c84b0-203">Ezután adja hozzá a Salesforce SAML identity provider tooone, a felhasználó utak hello.</span><span class="sxs-lookup"><span data-stu-id="c84b0-203">Next, add hello Salesforce SAML identity provider tooone of your user journeys.</span></span> <span data-ttu-id="c84b0-204">Ezen a ponton hello identitásszolgáltató be van állítva, de bármely hello felhasználói előfizetési vagy a bejelentkezési oldal nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="c84b0-204">At this point, hello identity provider has been set up, but it’s not available on any of hello user sign-up or sign-in pages.</span></span> <span data-ttu-id="c84b0-205">tooadd hello identity provider tooa bejelentkezési oldalára, először hozzon létre egy meglévő sablon felhasználói út duplikált.</span><span class="sxs-lookup"><span data-stu-id="c84b0-205">tooadd hello identity provider tooa sign-in page, first, create a duplicate of an existing template user journey.</span></span> <span data-ttu-id="c84b0-206">Hello sablont, majd módosítani, hogy az Azure AD identitásszolgáltató hello is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c84b0-206">Then, modify hello template so that it also has hello Azure AD identity provider.</span></span>

1. <span data-ttu-id="c84b0-207">Nyissa meg a hello alap fájlt a házirend (például TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="c84b0-207">Open hello base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
2. <span data-ttu-id="c84b0-208">Hello található `<UserJourneys>` elemet, majd a teljes másolat hello `<UserJourney>` érték, beleértve az Id = "SignUpOrSignIn".</span><span class="sxs-lookup"><span data-stu-id="c84b0-208">Find hello `<UserJourneys>` element, and then copy hello entire `<UserJourney>` value, including Id=”SignUpOrSignIn”.</span></span>
3. <span data-ttu-id="c84b0-209">Nyissa meg a hello kiterjesztésű fájlt (például TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="c84b0-209">Open hello extension file (for example, TrustFrameworkExtensions.xml).</span></span> <span data-ttu-id="c84b0-210">Hello található `<UserJourneys>` elemet.</span><span class="sxs-lookup"><span data-stu-id="c84b0-210">Find hello `<UserJourneys>` element.</span></span> <span data-ttu-id="c84b0-211">Ha hello elem nem létezik, hozzon létre egyet.</span><span class="sxs-lookup"><span data-stu-id="c84b0-211">If hello element doesn't exist, create one.</span></span>
4. <span data-ttu-id="c84b0-212">Beillesztés hello teljes másolt `<UserJourney>` hello gyermekeként `<UserJourneys>` elemet.</span><span class="sxs-lookup"><span data-stu-id="c84b0-212">Paste hello entire copied `<UserJourney>` as a child of hello `<UserJourneys>` element.</span></span>
5. <span data-ttu-id="c84b0-213">Nevezze át az új hello hello azonosítója `<UserJourney>` (például SignUpOrSignUsingContoso).</span><span class="sxs-lookup"><span data-stu-id="c84b0-213">Rename hello ID of hello new `<UserJourney>` (for example, SignUpOrSignUsingContoso).</span></span>

### <a name="display-hello-identity-provider-button"></a><span data-ttu-id="c84b0-214">Megjelenítési hello identity provider gomb</span><span class="sxs-lookup"><span data-stu-id="c84b0-214">Display hello identity provider button</span></span>

<span data-ttu-id="c84b0-215">Hello `<ClaimsProviderSelection>` elem hasonló tooan identity provider gomb regisztráció vagy bejelentkezés lapon.</span><span class="sxs-lookup"><span data-stu-id="c84b0-215">hello `<ClaimsProviderSelection>` element is analogous tooan identity provider button on a sign-up or sign-in page.</span></span> <span data-ttu-id="c84b0-216">Adja hozzá egy `<ClaimsProviderSelection>` Salesforce eleme, egy új gomb akkor jelenik meg, amikor egy felhasználó toothis lap.</span><span class="sxs-lookup"><span data-stu-id="c84b0-216">By adding an `<ClaimsProviderSelection>` element for Salesforce, a new button appears when a user goes toothis page.</span></span> <span data-ttu-id="c84b0-217">toodisplay hello identity provider gombra:</span><span class="sxs-lookup"><span data-stu-id="c84b0-217">toodisplay hello identity provider button:</span></span>

1. <span data-ttu-id="c84b0-218">A hello `<UserJourney>` létrehozott, a hello található `<OrchestrationStep>` rendelkező `Order="1"`.</span><span class="sxs-lookup"><span data-stu-id="c84b0-218">In hello `<UserJourney>` that you created, find hello `<OrchestrationStep>` with `Order="1"`.</span></span>
2. <span data-ttu-id="c84b0-219">Adja hozzá a következő XML hello:</span><span class="sxs-lookup"><span data-stu-id="c84b0-219">Add hello following XML:</span></span>

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

3. <span data-ttu-id="c84b0-220">Állítsa be `TargetClaimsExchangeId` tooa logikai érték.</span><span class="sxs-lookup"><span data-stu-id="c84b0-220">Set `TargetClaimsExchangeId` tooa logical value.</span></span> <span data-ttu-id="c84b0-221">Javasoljuk a következő hello ugyanaz, mint mások egyezmény (például  *\[ClaimProviderName\]Exchange*).</span><span class="sxs-lookup"><span data-stu-id="c84b0-221">We recommend following hello same convention as others (for example, *\[ClaimProviderName\]Exchange*).</span></span>

### <a name="link-hello-identity-provider-button-tooan-action"></a><span data-ttu-id="c84b0-222">Hivatkozás hello identitás szolgáltató tooan megnyomásakor</span><span class="sxs-lookup"><span data-stu-id="c84b0-222">Link hello identity provider button tooan action</span></span>

<span data-ttu-id="c84b0-223">Most, hogy az identity provider gomb helyen, hivatkozás tooan művelet.</span><span class="sxs-lookup"><span data-stu-id="c84b0-223">Now that you have an identity provider button in place, link it tooan action.</span></span> <span data-ttu-id="c84b0-224">Ebben az esetben az Azure AD B2C toocommunicate a Salesforce tooreceive SAML-jogkivonatból kell hello műveletet.</span><span class="sxs-lookup"><span data-stu-id="c84b0-224">In this case, hello action is for Azure AD B2C toocommunicate with Salesforce tooreceive a SAML token.</span></span> <span data-ttu-id="c84b0-225">Ehhez a Salesforce SAML a műszaki hello-profil összekapcsolásával jogcímszolgáltató:</span><span class="sxs-lookup"><span data-stu-id="c84b0-225">You can do this by linking hello technical profile for your Salesforce SAML claims provider:</span></span>

1. <span data-ttu-id="c84b0-226">A hello `<UserJourney>` csomópont, a keresés hello `<OrchestrationStep>` rendelkező `Order="2"`.</span><span class="sxs-lookup"><span data-stu-id="c84b0-226">In hello `<UserJourney>` node, find hello `<OrchestrationStep>` with `Order="2"`.</span></span>
2. <span data-ttu-id="c84b0-227">Adja hozzá a következő XML hello:</span><span class="sxs-lookup"><span data-stu-id="c84b0-227">Add hello following XML:</span></span>

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

3. <span data-ttu-id="c84b0-228">Frissítés `Id` toohello ugyanaz az értékre, hogy a korábban használt `TargetClaimsExchangeId`.</span><span class="sxs-lookup"><span data-stu-id="c84b0-228">Update `Id` toohello same value that you used earlier for `TargetClaimsExchangeId`.</span></span>
4. <span data-ttu-id="c84b0-229">Frissítés `TechnicalProfileReferenceId` toohello `Id` műszaki hello a profil, korábban létrehozott (például ContosoProfile).</span><span class="sxs-lookup"><span data-stu-id="c84b0-229">Update `TechnicalProfileReferenceId` toohello `Id` of hello technical profile you created earlier (for example, ContosoProfile).</span></span>

### <a name="upload-hello-updated-extension-file"></a><span data-ttu-id="c84b0-230">Hello frissített kiterjesztésű fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="c84b0-230">Upload hello updated extension file</span></span>

<span data-ttu-id="c84b0-231">Elkészült módosítása hello kiterjesztésű fájlt.</span><span class="sxs-lookup"><span data-stu-id="c84b0-231">You are done modifying hello extension file.</span></span> <span data-ttu-id="c84b0-232">Mentse, majd töltse fel ezt a fájlt.</span><span class="sxs-lookup"><span data-stu-id="c84b0-232">Save and upload this file.</span></span> <span data-ttu-id="c84b0-233">Győződjön meg arról, hogy az összes érvényesítések sikeres.</span><span class="sxs-lookup"><span data-stu-id="c84b0-233">Ensure that all validations succeed.</span></span>

### <a name="update-hello-relying-party-file"></a><span data-ttu-id="c84b0-234">Hello függő entitás fájl frissítése</span><span class="sxs-lookup"><span data-stu-id="c84b0-234">Update hello relying party file</span></span>

<span data-ttu-id="c84b0-235">Következő lépésként frissítse hello függő entitásonkénti (RP) fájlt, amely kezdeményezi hello felhasználói út létrehozott:</span><span class="sxs-lookup"><span data-stu-id="c84b0-235">Next, update hello relying party (RP) file that initiates hello user journey that you created:</span></span>

1. <span data-ttu-id="c84b0-236">Készítsen másolatot a SignUpOrSignIn.xml a munkakönyvtárat.</span><span class="sxs-lookup"><span data-stu-id="c84b0-236">Make a copy of SignUpOrSignIn.xml in your working directory.</span></span> <span data-ttu-id="c84b0-237">Nevezze át (például SignUpOrSignInWithAAD.xml).</span><span class="sxs-lookup"><span data-stu-id="c84b0-237">Then, rename it (for example, SignUpOrSignInWithAAD.xml).</span></span>
2. <span data-ttu-id="c84b0-238">Nyissa meg hello új fájl- és frissítési hello `PolicyId` az attribútum `<TrustFrameworkPolicy>` egyedi értékkel.</span><span class="sxs-lookup"><span data-stu-id="c84b0-238">Open hello new file and update hello `PolicyId` attribute for `<TrustFrameworkPolicy>` with a unique value.</span></span> <span data-ttu-id="c84b0-239">Ez az a házirend (például SignUpOrSignInWithAAD) hello nevét.</span><span class="sxs-lookup"><span data-stu-id="c84b0-239">This is hello name of your policy (for example, SignUpOrSignInWithAAD).</span></span>
3. <span data-ttu-id="c84b0-240">Módosítsa a hello `ReferenceId` attribútumnak `<DefaultUserJourney>` toomatch hello `Id` hello új felhasználói út (például SignUpOrSignUsingContoso) létrehozott.</span><span class="sxs-lookup"><span data-stu-id="c84b0-240">Modify hello `ReferenceId` attribute in `<DefaultUserJourney>` toomatch hello `Id` of hello new user journey that you created (for example, SignUpOrSignUsingContoso).</span></span>
4. <span data-ttu-id="c84b0-241">A módosítások mentéséhez, és ezután a hello-fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="c84b0-241">Save your changes, and then upload hello file.</span></span>

## <a name="test-and-troubleshoot"></a><span data-ttu-id="c84b0-242">Tesztelése és hibakeresése</span><span class="sxs-lookup"><span data-stu-id="c84b0-242">Test and troubleshoot</span></span>

<span data-ttu-id="c84b0-243">tootest hello egyéni házirend imént feltöltött, az Azure-portálon hello toohello szabályzat paneljén lépjen, és kattintson **futtatása most**.</span><span class="sxs-lookup"><span data-stu-id="c84b0-243">tootest hello custom policy that you just uploaded, in hello Azure portal, go toohello policy blade, and then click **Run now**.</span></span> <span data-ttu-id="c84b0-244">Ha a hiba, lásd: [egyéni szabályzatokkal kapcsolatos problémák elhárítása](active-directory-b2c-troubleshoot-custom.md).</span><span class="sxs-lookup"><span data-stu-id="c84b0-244">If it fails, see [Troubleshoot custom policies](active-directory-b2c-troubleshoot-custom.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c84b0-245">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c84b0-245">Next steps</span></span>

<span data-ttu-id="c84b0-246">Visszajelzés küldése túl[AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="c84b0-246">Provide feedback too[AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).</span></span>
