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
ms.openlocfilehash: 8c981046ff41d3927ff60d6dc4f40366ae25ba74
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-b2c-add-microsoft-account-msa-as-an-identity-provider-using-custom-policies"></a><span data-ttu-id="a03af-103">Az Azure Active Directory B2C: Egyéni házirendekkel identitás-szolgáltatóként hozzáadása a Microsoft-fiók (MSA)</span><span class="sxs-lookup"><span data-stu-id="a03af-103">Azure Active Directory B2C: Add Microsoft Account (MSA) as an identity provider using custom policies</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="a03af-104">Ez a cikk bemutatja, hogyan használatával Bejelentkezés Microsoft-fiók (msa-t) a felhasználók engedélyezése [egyéni házirendek](active-directory-b2c-overview-custom.md).</span><span class="sxs-lookup"><span data-stu-id="a03af-104">This article shows you how to enable sign-in for users from Microsoft account (MSA) through the use of [custom policies](active-directory-b2c-overview-custom.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a03af-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a03af-105">Prerequisites</span></span>
<span data-ttu-id="a03af-106">Hajtsa végre a a [Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="a03af-106">Complete the steps in the [Getting started with custom policies](active-directory-b2c-get-started-custom.md) article.</span></span>

<span data-ttu-id="a03af-107">Ezek a lépések az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="a03af-107">These steps include:</span></span>

1.  <span data-ttu-id="a03af-108">A Microsoft-fiók alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a03af-108">Creating a Microsoft account application.</span></span>
2.  <span data-ttu-id="a03af-109">A Microsoft-fiók alkalmazás kulcs hozzáadása az Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="a03af-109">Adding the Microsoft account application key to Azure AD B2C</span></span>
3.  <span data-ttu-id="a03af-110">Egy házirend hozzáadása jogcím-szolgáltató</span><span class="sxs-lookup"><span data-stu-id="a03af-110">Adding claims provider to a policy</span></span>
4.  <span data-ttu-id="a03af-111">A Microsoft Account jogcímszolgáltató felhasználói út regisztrálása</span><span class="sxs-lookup"><span data-stu-id="a03af-111">Registering the Microsoft Account claims provider to a user journey</span></span>
5.  <span data-ttu-id="a03af-112">A házirend feltöltése az Azure AD B2C bérlői és tesztelik azt</span><span class="sxs-lookup"><span data-stu-id="a03af-112">Uploading the policy to an Azure AD B2C tenant and test it</span></span>

## <a name="create-a-microsoft-account-application"></a><span data-ttu-id="a03af-113">Microsoft-fiók alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a03af-113">Create a Microsoft account application</span></span>
<span data-ttu-id="a03af-114">Az Azure Active Directory (Azure AD) B2C identitás-szolgáltatóként a Microsoft-fiók használatához szüksége hozzon létre egy Microsoft-fiók alkalmazást, és adja meg azt a megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="a03af-114">To use Microsoft account as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a Microsoft account application and supply it with the right parameters.</span></span> <span data-ttu-id="a03af-115">Microsoft-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="a03af-115">You need a Microsoft account.</span></span> <span data-ttu-id="a03af-116">Ha még nincs fiókja, látogasson el [https://www.live.com/](https://www.live.com/).</span><span class="sxs-lookup"><span data-stu-id="a03af-116">If you don’t have one, visit [https://www.live.com/](https://www.live.com/).</span></span>

1.  <span data-ttu-id="a03af-117">Lépjen a [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) , és jelentkezzen be Microsoft-fiók hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="a03af-117">Go to the [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) and sign in with your Microsoft account credentials.</span></span>
2.  <span data-ttu-id="a03af-118">Kattintson a **hozzáadhat egy alkalmazást**.</span><span class="sxs-lookup"><span data-stu-id="a03af-118">Click **Add an app**.</span></span>

    ![Microsoft fiók - alkalmazás hozzáadása](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-new-app.png)

3.  <span data-ttu-id="a03af-120">Adja meg egy **neve** az alkalmazás **lépjen kapcsolatba az e-mailek**, törölje a jelet **tudassa velünk megkezdésében súgó** kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="a03af-120">Provide a **Name** for your application, **Contact email**, uncheck **Let us help you get started** and click **Create**.</span></span>

    ![Microsoft-fiók – az alkalmazás regisztrálása](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-name.png)

4.  <span data-ttu-id="a03af-122">Másolja a értékének **alkalmazásazonosító**. Esetleg szükség lenne rá, Microsoft-fiókok konfigurálása a bérlő az identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="a03af-122">Copy the value of **Application Id**. You need it to configure Microsoft account as an identity provider in your tenant.</span></span>

    ![Microsoft-fiók - példány az alkalmazás azonosítójának értéke](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-id.png)

5.  <span data-ttu-id="a03af-124">Kattintson a **Hozzáadás platform**</span><span class="sxs-lookup"><span data-stu-id="a03af-124">Click on **Add platform**</span></span>

    ![Microsoft fiók - platform hozzáadása](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-platform.png)

6.  <span data-ttu-id="a03af-126">A platform listából válassza ki a **webes**.</span><span class="sxs-lookup"><span data-stu-id="a03af-126">From the platform list, choose **Web**.</span></span>

    ![Microsoft-fiók – a platform listából válassza a webes](media/active-directory-b2c-custom-setup-ms-account-idp/msa-web.png)

7.  <span data-ttu-id="a03af-128">Adja meg `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` a a **átirányítási URI-azonosítók** mező.</span><span class="sxs-lookup"><span data-stu-id="a03af-128">Enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in the **Redirect URIs** field.</span></span> <span data-ttu-id="a03af-129">Cserélje le **{tenant}** a bérlő nevű (például contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="a03af-129">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span>

    ![Microsoft-fiók - készlet átirányítási URL-címek](media/active-directory-b2c-custom-setup-ms-account-idp/msa-redirect-url.png)

8.  <span data-ttu-id="a03af-131">Kattintson a **új jelszó létrehozása** alatt a **alkalmazás titkos kulcsok** szakasz.</span><span class="sxs-lookup"><span data-stu-id="a03af-131">Click on **Generate New Password** under the **Application Secrets** section.</span></span> <span data-ttu-id="a03af-132">Másolja az új jelszót a képernyőn jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a03af-132">Copy the new password displayed on screen.</span></span> <span data-ttu-id="a03af-133">Esetleg szükség lenne rá, Microsoft-fiókok konfigurálása a bérlő az identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="a03af-133">You need it to configure Microsoft account as an identity provider in your tenant.</span></span> <span data-ttu-id="a03af-134">Ez a jelszó nem egy fontos biztonsági hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="a03af-134">This password is an important security credential.</span></span>

    ![Microsoft fiók – új jelszó létrehozása](media/active-directory-b2c-custom-setup-ms-account-idp/msa-generate-new-password.png)

    ![Microsoft-fiók - példány az új jelszó](media/active-directory-b2c-custom-setup-ms-account-idp/msa-new-password.png)

9.  <span data-ttu-id="a03af-137">Jelölje be a jelölőnégyzetet, amely szerint **Live SDK támogatási** alatt a **speciális beállítások** szakasz.</span><span class="sxs-lookup"><span data-stu-id="a03af-137">Check the box that says **Live SDK support** under the **Advanced Options** section.</span></span> <span data-ttu-id="a03af-138">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="a03af-138">Click **Save**.</span></span>

    ![Microsoft-fiók - Live SDK-támogatás](media/active-directory-b2c-custom-setup-ms-account-idp/msa-live-sdk-support.png)

## <a name="add-the-microsoft-account-application-key-to-azure-ad-b2c"></a><span data-ttu-id="a03af-140">A Microsoft-fiók alkalmazás kulcs hozzáadása az Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="a03af-140">Add the Microsoft account application key to Azure AD B2C</span></span>
<span data-ttu-id="a03af-141">A Microsoft-fiókokkal az összevonáshoz szükség van egy ügyfélkulcsot Microsoft-fiók nevében az alkalmazás az Azure AD B2C megbízhatósági.</span><span class="sxs-lookup"><span data-stu-id="a03af-141">Federation with Microsoft accounts requires a client secret for Microsoft account to trust Azure AD B2C on behalf of the application.</span></span> <span data-ttu-id="a03af-142">A Microsoft-fiók alkalmazás secert tárolása az Azure AD B2C bérlő kell:</span><span class="sxs-lookup"><span data-stu-id="a03af-142">You need to store your Microsoft account application secert in Azure AD B2C tenant:</span></span>   

1.  <span data-ttu-id="a03af-143">Nyissa meg az Azure AD B2C-bérlő, és válassza ki **B2C beállítások** > **identitás élmény keretrendszer**</span><span class="sxs-lookup"><span data-stu-id="a03af-143">Go to your Azure AD B2C tenant, and select **B2C Settings** > **Identity Experience Framework**</span></span>
2.  <span data-ttu-id="a03af-144">Válassza ki **házirend kulcsok** érhető el a bérlői kulcsok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="a03af-144">Select **Policy Keys** to view the keys available in your tenant.</span></span>
3.  <span data-ttu-id="a03af-145">Kattintson a **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="a03af-145">Click **+Add**.</span></span>
4.  <span data-ttu-id="a03af-146">A **beállítások**, használjon **manuális**.</span><span class="sxs-lookup"><span data-stu-id="a03af-146">For **Options**, use **Manual**.</span></span>
5.  <span data-ttu-id="a03af-147">A **neve**, használjon `MSASecret`.</span><span class="sxs-lookup"><span data-stu-id="a03af-147">For **Name**, use `MSASecret`.</span></span>  
    <span data-ttu-id="a03af-148">Az előtag `B2C_1A_` előfordulhat, hogy automatikusan hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="a03af-148">The prefix `B2C_1A_` might be added automatically.</span></span>
6.  <span data-ttu-id="a03af-149">Az a **titkos** mezőbe írja be a Microsoft alkalmazás titkos kulcs a https://apps.dev.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="a03af-149">In the **Secret** box, enter your Microsoft application secret from https://apps.dev.microsoft.com</span></span>
7.  <span data-ttu-id="a03af-150">A **kulcshasználat**, használjon **aláírás**.</span><span class="sxs-lookup"><span data-stu-id="a03af-150">For **Key usage**, use **Signature**.</span></span>
8.  <span data-ttu-id="a03af-151">Kattintson a **Create** (Létrehozás) gombra</span><span class="sxs-lookup"><span data-stu-id="a03af-151">Click **Create**</span></span>
9.  <span data-ttu-id="a03af-152">Győződjön meg arról, hogy létrehozta a kulcs `B2C_1A_MSASecret`.</span><span class="sxs-lookup"><span data-stu-id="a03af-152">Confirm that you've created the key `B2C_1A_MSASecret`.</span></span>

## <a name="add-a-claims-provider-in-your-extension-policy"></a><span data-ttu-id="a03af-153">A jogcím-szolgáltató hozzáadása a bővítmény házirend</span><span class="sxs-lookup"><span data-stu-id="a03af-153">Add a claims provider in your extension policy</span></span>
<span data-ttu-id="a03af-154">Ha azt szeretné, hogy a felhasználók Microsoft Account bejelentkezni, szüksége Microsoft Account megadása jogcímek-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="a03af-154">If you want users to sign in by using Microsoft Account, you need to define Microsoft Account as a claims provider.</span></span> <span data-ttu-id="a03af-155">Ez azt jelenti meg kell adnia egy végpontot, amely az Azure AD B2C-vel kommunikál.</span><span class="sxs-lookup"><span data-stu-id="a03af-155">In other words, you need to specify an endpoint that Azure AD B2C communicates with.</span></span> <span data-ttu-id="a03af-156">A végpont a jogcímek, az Azure AD B2C által használt győződjön meg arról, hogy egy adott felhasználó engedélyezést biztosít.</span><span class="sxs-lookup"><span data-stu-id="a03af-156">The endpoint provides a set of claims that are used by Azure AD B2C to verify that a specific user has authenticated.</span></span>

<span data-ttu-id="a03af-157">Microsoft Account meghatározni, a Jogcímszolgáltatók hozzáadásával `<ClaimsProvider>` csomópont a bővítmény a házirend-fájlban:</span><span class="sxs-lookup"><span data-stu-id="a03af-157">Define Microsoft Account as a claims provider, by adding `<ClaimsProvider>` node in your extension policy file:</span></span>

1.  <span data-ttu-id="a03af-158">Nyissa meg a házirend bővítményfájl (TrustFrameworkExtensions.xml) a munkakönyvtárat.</span><span class="sxs-lookup"><span data-stu-id="a03af-158">Open the extension policy file (TrustFrameworkExtensions.xml) from your working directory.</span></span> <span data-ttu-id="a03af-159">Ha egy XML-szerkesztőt kell [próbálja meg a Visual Studio Code](https://code.visualstudio.com/download), egy egyszerű platformfüggetlen-szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="a03af-159">If you need an XML editor, [try Visual Studio Code](https://code.visualstudio.com/download), a lightweight cross-platform editor.</span></span>
2.  <span data-ttu-id="a03af-160">Keresés a `<ClaimsProviders>` szakasz</span><span class="sxs-lookup"><span data-stu-id="a03af-160">Find the `<ClaimsProviders>` section</span></span>
3.  <span data-ttu-id="a03af-161">Adja hozzá a következő XML-részletet alatt a `ClaimsProviders` elem:</span><span class="sxs-lookup"><span data-stu-id="a03af-161">Add following XML snippet under the `ClaimsProviders` element:</span></span>

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

4.  <span data-ttu-id="a03af-162">Cserélje le `client_id` értékének a Microsoft Account alkalmazás ügyfél-azonosítóját</span><span class="sxs-lookup"><span data-stu-id="a03af-162">Replace `client_id` value with your Microsoft Account application client Id</span></span>

5.  <span data-ttu-id="a03af-163">Mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="a03af-163">Save the file.</span></span>

## <a name="register-the-microsoft-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a><span data-ttu-id="a03af-164">A Microsoft Account jogcímszolgáltató bejelentkezési mentése regisztrálásának vagy a felhasználói bejelentkezés használatában</span><span class="sxs-lookup"><span data-stu-id="a03af-164">Register the Microsoft Account claims provider to Sign up or Sign-in user journey</span></span>

<span data-ttu-id="a03af-165">Ezen a ponton az identitásszolgáltató be van állítva, de a sign-Close-Up/sign-in képernyők egyik nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="a03af-165">At this point, the identity provider has been set up, but it’s not available in any of the sign-up/sign-in screens.</span></span> <span data-ttu-id="a03af-166">Most a Microsoft Account identitásszolgáltató hozzáadása a felhasználói `SignUpOrSignIn` felhasználói út.</span><span class="sxs-lookup"><span data-stu-id="a03af-166">Now you need to add the Microsoft Account identity provider to your user `SignUpOrSignIn` user journey.</span></span> <span data-ttu-id="a03af-167">Tegye elérhetővé, azt hozzon létre egy meglévő sablon felhasználói út duplikált.</span><span class="sxs-lookup"><span data-stu-id="a03af-167">To make it available, we create a duplicate of an existing template user journey.</span></span>  <span data-ttu-id="a03af-168">Majd azt a Microsoft Account identitásszolgáltató hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="a03af-168">Then we add the Microsoft Account identity provider:</span></span>

> [!NOTE]
>
><span data-ttu-id="a03af-169">Ha korábban kimásolt a `<UserJourneys>` a bővítményfájl házirend alap fájlból elem `TrustFrameworkExtensions.xml`, ezt a szakaszt kihagyhatja.</span><span class="sxs-lookup"><span data-stu-id="a03af-169">If you previously copied the `<UserJourneys>` element from base file of your policy to the extension file `TrustFrameworkExtensions.xml`, you can skip to this section.</span></span>

1.  <span data-ttu-id="a03af-170">Nyissa meg a házirend (például TrustFrameworkBase.xml) Alap fájlt.</span><span class="sxs-lookup"><span data-stu-id="a03af-170">Open the base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
2.  <span data-ttu-id="a03af-171">Keresés a `<UserJourneys>` elem és a teljes tartalmának másolása a `<UserJourneys>` csomópont.</span><span class="sxs-lookup"><span data-stu-id="a03af-171">Find the `<UserJourneys>` element and copy the entire content of `<UserJourneys>` node.</span></span>
3.  <span data-ttu-id="a03af-172">Nyissa meg a bővítmény (például TrustFrameworkExtensions.xml) fájlt, és keresse a `<UserJourneys>` elemet.</span><span class="sxs-lookup"><span data-stu-id="a03af-172">Open the extension file (for example, TrustFrameworkExtensions.xml) and find the `<UserJourneys>` element.</span></span> <span data-ttu-id="a03af-173">Ha az elem nem létezik, vegyen fel egyet.</span><span class="sxs-lookup"><span data-stu-id="a03af-173">If the element doesn't exist, add one.</span></span>
4.  <span data-ttu-id="a03af-174">Illessze be a teljes tartalmát `<UserJournesy>` csomópont gyermekeként másolt a `<UserJourneys>` elemet.</span><span class="sxs-lookup"><span data-stu-id="a03af-174">Paste the entire content of `<UserJournesy>` node that you copied as a child of the `<UserJourneys>` element.</span></span>

### <a name="display-the-button"></a><span data-ttu-id="a03af-175">A gomb megjelenítése</span><span class="sxs-lookup"><span data-stu-id="a03af-175">Display the button</span></span>
<span data-ttu-id="a03af-176">A `<ClaimsProviderSelections>` elem definiálja a jogcímeket szolgáltató tanúsítványválasztási beállítások és a sorrendjük listáját.</span><span class="sxs-lookup"><span data-stu-id="a03af-176">The `<ClaimsProviderSelections>` element defines the list of claims provider selection options and their order.</span></span>  <span data-ttu-id="a03af-177">`<ClaimsProviderSelection>`a elem egy identity provider gombra a sign-Close-Up/sign-in oldalán hasonló.</span><span class="sxs-lookup"><span data-stu-id="a03af-177">`<ClaimsProviderSelection>` element is analogous to an identity provider button on a sign-up/sign-in page.</span></span> <span data-ttu-id="a03af-178">Ha ad hozzá egy `<ClaimsProviderSelection>` elem Microsoft-fiókot, egy új gomb megjelenik, amikor a felhasználó az oldalon fájljai.</span><span class="sxs-lookup"><span data-stu-id="a03af-178">If you add a `<ClaimsProviderSelection>` element for Microsoft account, a new button shows up when a user lands on the page.</span></span> <span data-ttu-id="a03af-179">Ez az elem hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="a03af-179">To add this element:</span></span>

1.  <span data-ttu-id="a03af-180">Keresés a `<UserJourney>` tartalmazó csomópont `Id="SignUpOrSignIn"` a a felhasználók utazás másolt.</span><span class="sxs-lookup"><span data-stu-id="a03af-180">Find the `<UserJourney>` node that includes `Id="SignUpOrSignIn"` in the user journey that you copied.</span></span>
2.  <span data-ttu-id="a03af-181">Keresse meg a `<OrchestrationStep>` tartalmazó csomópont`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="a03af-181">Locate the `<OrchestrationStep>` node that includes `Order="1"`</span></span>
3.  <span data-ttu-id="a03af-182">Adja hozzá a következő XML-részletet a `<ClaimsProviderSelections>` csomópont:</span><span class="sxs-lookup"><span data-stu-id="a03af-182">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-the-button-to-an-action"></a><span data-ttu-id="a03af-183">A gomb összekapcsolása egy műveletet</span><span class="sxs-lookup"><span data-stu-id="a03af-183">Link the button to an action</span></span>
<span data-ttu-id="a03af-184">Most, hogy a gomb helyen, hogy egy művelet kapcsolódnia kell.</span><span class="sxs-lookup"><span data-stu-id="a03af-184">Now that you have a button in place, you need to link it to an action.</span></span> <span data-ttu-id="a03af-185">A művelet, ebben az esetben, akkor az Azure AD B2C kommunikálni a Microsoft Account fogadhatnak jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="a03af-185">The action, in this case, is for Azure AD B2C to communicate with Microsoft Account to receive a token.</span></span> <span data-ttu-id="a03af-186">A gomb művelet mutató hivatkozás létrehozása a műszaki profil létrehozhatja, ha a Microsoft Account jogcímeket szolgáltató:</span><span class="sxs-lookup"><span data-stu-id="a03af-186">Link the button to an action by linking the technical profile for your Microsoft Account claims provider:</span></span>

1.  <span data-ttu-id="a03af-187">Keresés a `<OrchestrationStep>` is tartalmazó `Order="2"` a a `<UserJourney>` csomópont.</span><span class="sxs-lookup"><span data-stu-id="a03af-187">Find the `<OrchestrationStep>` that includes `Order="2"` in the `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="a03af-188">Adja hozzá a következő XML-részletet a `<ClaimsExchanges>` csomópont:</span><span class="sxs-lookup"><span data-stu-id="a03af-188">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

> [!NOTE]
>
>   * <span data-ttu-id="a03af-189">Győződjön meg arról a `Id` ugyanazzal az értékkel, amely rendelkezik `TargetClaimsExchangeId` az előző szakaszban leírt</span><span class="sxs-lookup"><span data-stu-id="a03af-189">Ensure the `Id` has the same value as that of `TargetClaimsExchangeId` in the preceding section</span></span>
>   * <span data-ttu-id="a03af-190">Győződjön meg arról `TechnicalProfileReferenceId` beállítása a műszaki profil létrehozott korábbi (MSA-OIDC).</span><span class="sxs-lookup"><span data-stu-id="a03af-190">Ensure `TechnicalProfileReferenceId` ID is set to the technical profile you created earlier (MSA-OIDC).</span></span>

## <a name="upload-the-policy-to-your-tenant"></a><span data-ttu-id="a03af-191">A házirend a bérlő feltöltése</span><span class="sxs-lookup"><span data-stu-id="a03af-191">Upload the policy to your tenant</span></span>
1.  <span data-ttu-id="a03af-192">A a [Azure-portálon](https://portal.azure.com), átváltani a [az Azure AD B2C-bérlő kontextusában](active-directory-b2c-navigate-to-b2c-context.md), és nyissa meg a **az Azure AD B2C** panelen.</span><span class="sxs-lookup"><span data-stu-id="a03af-192">In the [Azure portal](https://portal.azure.com), switch into the [context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and open the **Azure AD B2C** blade.</span></span>
2.  <span data-ttu-id="a03af-193">Válassza ki **identitás élmény keretrendszer**.</span><span class="sxs-lookup"><span data-stu-id="a03af-193">Select **Identity Experience Framework**.</span></span>
3.  <span data-ttu-id="a03af-194">Nyissa meg a **házirend** panelen.</span><span class="sxs-lookup"><span data-stu-id="a03af-194">Open the **All Policies** blade.</span></span>
4.  <span data-ttu-id="a03af-195">Válassza ki **házirend feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="a03af-195">Select **Upload Policy**.</span></span>
5.  <span data-ttu-id="a03af-196">Ellenőrizze **felülírja a házirendet, ha létezik** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="a03af-196">Check **Overwrite the policy if it exists** box.</span></span>
6.  <span data-ttu-id="a03af-197">**Töltse fel** TrustFrameworkExtensions.xml, és győződjön meg arról, hogy az érvényesítése nem hiúsul meg az</span><span class="sxs-lookup"><span data-stu-id="a03af-197">**Upload** TrustFrameworkExtensions.xml and ensure that it does not fail the validation</span></span>

## <a name="test-the-custom-policy-by-using-run-now"></a><span data-ttu-id="a03af-198">Tesztelje az egyéni házirend segítségével futtatása most</span><span class="sxs-lookup"><span data-stu-id="a03af-198">Test the custom policy by using Run Now</span></span>

1.  <span data-ttu-id="a03af-199">Nyissa meg **az Azure AD B2C beállítások** , és navigáljon **identitás élmény keretrendszer**.</span><span class="sxs-lookup"><span data-stu-id="a03af-199">Open **Azure AD B2C Settings** and go to **Identity Experience Framework**.</span></span>
> [!NOTE]
>
><span data-ttu-id="a03af-200">**Futtassa most** legalább egy alkalmazás a tenant preregistered lehet szükséges.</span><span class="sxs-lookup"><span data-stu-id="a03af-200">**Run now** requires at least one application to be preregistered on the tenant.</span></span> <span data-ttu-id="a03af-201">Megtudhatja, hogyan kell regisztrálni az alkalmazások, tekintse meg az Azure AD B2C [Ismerkedés](active-directory-b2c-get-started.md) cikk vagy a [regisztrációja](active-directory-b2c-app-registration.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="a03af-201">To learn how to register applications, see the Azure AD B2C [Get started](active-directory-b2c-get-started.md) article or the [Application registration](active-directory-b2c-app-registration.md) article.</span></span>
2.  <span data-ttu-id="a03af-202">Nyissa meg **B2C_1A_signup_signin**, a függő entitásonkénti (RP) egyéni házirend feltöltött.</span><span class="sxs-lookup"><span data-stu-id="a03af-202">Open **B2C_1A_signup_signin**, the relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="a03af-203">Válassza ki **futtatása most**.</span><span class="sxs-lookup"><span data-stu-id="a03af-203">Select **Run now**.</span></span>
3.  <span data-ttu-id="a03af-204">Jelentkezhet be Microsoft-fiók használatával kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a03af-204">You should be able to sign in using Microsoft account.</span></span>

## <a name="optional-register-the-microsoft-account-claims-provider-to-profile-edit-user-journey"></a><span data-ttu-id="a03af-205">[Választható] A Microsoft Account jogcímszolgáltató profil szerkesztheti a felhasználói út regisztrálása</span><span class="sxs-lookup"><span data-stu-id="a03af-205">[Optional] Register the Microsoft Account claims provider to Profile-Edit user journey</span></span>
<span data-ttu-id="a03af-206">Érdemes lehet a Microsoft Account identitásszolgáltató is hozzáadása a felhasználói `ProfileEdit` felhasználói út.</span><span class="sxs-lookup"><span data-stu-id="a03af-206">You may want to add the Microsoft Account identity provider also to your user `ProfileEdit` user journey.</span></span> <span data-ttu-id="a03af-207">Tegye elérhetővé, hogy ismételje meg az utolsó két lépést:</span><span class="sxs-lookup"><span data-stu-id="a03af-207">To make it available, we repeat the last two steps:</span></span>

### <a name="display-the-button"></a><span data-ttu-id="a03af-208">A gomb megjelenítése</span><span class="sxs-lookup"><span data-stu-id="a03af-208">Display the button</span></span>
1.  <span data-ttu-id="a03af-209">Nyissa meg a bővítményfájl házirend (például TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="a03af-209">Open the extension file of your policy (for example, TrustFrameworkExtensions.xml).</span></span>
2.  <span data-ttu-id="a03af-210">Keresés a `<UserJourney>` tartalmazó csomópont `Id="ProfileEdit"` a a felhasználók utazás másolt.</span><span class="sxs-lookup"><span data-stu-id="a03af-210">Find the `<UserJourney>` node that includes `Id="ProfileEdit"` in the user journey that you copied.</span></span>
3.  <span data-ttu-id="a03af-211">Keresse meg a `<OrchestrationStep>` tartalmazó csomópont`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="a03af-211">Locate the `<OrchestrationStep>` node that includes `Order="1"`</span></span>
4.  <span data-ttu-id="a03af-212">Adja hozzá a következő XML-részletet a `<ClaimsProviderSelections>` csomópont:</span><span class="sxs-lookup"><span data-stu-id="a03af-212">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-the-button-to-an-action"></a><span data-ttu-id="a03af-213">A gomb összekapcsolása egy műveletet</span><span class="sxs-lookup"><span data-stu-id="a03af-213">Link the button to an action</span></span>
1.  <span data-ttu-id="a03af-214">Keresés a `<OrchestrationStep>` is tartalmazó `Order="2"` a a `<UserJourney>` csomópont.</span><span class="sxs-lookup"><span data-stu-id="a03af-214">Find the `<OrchestrationStep>` that includes `Order="2"` in the `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="a03af-215">Adja hozzá a következő XML-részletet a `<ClaimsExchanges>` csomópont:</span><span class="sxs-lookup"><span data-stu-id="a03af-215">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a><span data-ttu-id="a03af-216">Tesztelje az egyéni házirend profil-módosítása segítségével futtatása most</span><span class="sxs-lookup"><span data-stu-id="a03af-216">Test the custom Profile-Edit policy by using Run Now</span></span>
1.  <span data-ttu-id="a03af-217">Nyissa meg **az Azure AD B2C beállítások** , és navigáljon **identitás élmény keretrendszer**.</span><span class="sxs-lookup"><span data-stu-id="a03af-217">Open **Azure AD B2C Settings** and go to **Identity Experience Framework**.</span></span>
2.  <span data-ttu-id="a03af-218">Nyissa meg **B2C_1A_ProfileEdit**, a függő entitásonkénti (RP) egyéni házirend feltöltött.</span><span class="sxs-lookup"><span data-stu-id="a03af-218">Open **B2C_1A_ProfileEdit**, the relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="a03af-219">Válassza ki **futtatása most**.</span><span class="sxs-lookup"><span data-stu-id="a03af-219">Select **Run now**.</span></span>
3.  <span data-ttu-id="a03af-220">Jelentkezhet be Microsoft-fiók használatával kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a03af-220">You should be able to sign in using Microsoft account.</span></span>

## <a name="download-the-complete-policy-files"></a><span data-ttu-id="a03af-221">A teljes házirend-fájlok letöltésére</span><span class="sxs-lookup"><span data-stu-id="a03af-221">Download the complete policy files</span></span>
<span data-ttu-id="a03af-222">Választható lehetőség: Javasoljuk hoz létre a minta-fájlok használata helyett a saját egyéni házirend fájlok az első lépések egyéni házirendekkel befejezése után végezze el a forgatókönyvet.</span><span class="sxs-lookup"><span data-stu-id="a03af-222">Optional: We recommend you build your scenario using your own Custom policy files after completing the Getting Started with Custom Policies walk through instead of using these sample files.</span></span>  [<span data-ttu-id="a03af-223">Házirend mintafájlok referencia</span><span class="sxs-lookup"><span data-stu-id="a03af-223">Sample policy files for reference</span></span>](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-msa-app)
