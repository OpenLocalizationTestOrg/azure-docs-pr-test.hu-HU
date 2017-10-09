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
ms.openlocfilehash: 577ac612f69015e6790f2fa9f2cfb42c2b524b55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-add-microsoft-account-msa-as-an-identity-provider-using-custom-policies"></a><span data-ttu-id="92a65-103">Az Azure Active Directory B2C: Egyéni házirendekkel identitás-szolgáltatóként hozzáadása a Microsoft-fiók (MSA)</span><span class="sxs-lookup"><span data-stu-id="92a65-103">Azure Active Directory B2C: Add Microsoft Account (MSA) as an identity provider using custom policies</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="92a65-104">A cikkből megtudhatja, hogyan tooenable bejelentkezhet a felhasználók számára a Microsoft-fiókból (MSA) használata hello [egyéni házirendek](active-directory-b2c-overview-custom.md).</span><span class="sxs-lookup"><span data-stu-id="92a65-104">This article shows you how tooenable sign-in for users from Microsoft account (MSA) through hello use of [custom policies](active-directory-b2c-overview-custom.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="92a65-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="92a65-105">Prerequisites</span></span>
<span data-ttu-id="92a65-106">Teljes hello szükséges lépések hello [Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="92a65-106">Complete hello steps in hello [Getting started with custom policies](active-directory-b2c-get-started-custom.md) article.</span></span>

<span data-ttu-id="92a65-107">Ezek a lépések az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="92a65-107">These steps include:</span></span>

1.  <span data-ttu-id="92a65-108">A Microsoft-fiók alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="92a65-108">Creating a Microsoft account application.</span></span>
2.  <span data-ttu-id="92a65-109">Hello Microsoft fiók alkalmazás fő tooAzure AD B2C hozzáadása</span><span class="sxs-lookup"><span data-stu-id="92a65-109">Adding hello Microsoft account application key tooAzure AD B2C</span></span>
3.  <span data-ttu-id="92a65-110">Jogcím-szolgáltató tooa házirend hozzáadása</span><span class="sxs-lookup"><span data-stu-id="92a65-110">Adding claims provider tooa policy</span></span>
4.  <span data-ttu-id="92a65-111">Hello Microsoft Account jogcímeket szolgáltató tooa felhasználói út regisztrálása</span><span class="sxs-lookup"><span data-stu-id="92a65-111">Registering hello Microsoft Account claims provider tooa user journey</span></span>
5.  <span data-ttu-id="92a65-112">Bérlői, és tesztelik azt hello házirend tooan az Azure AD B2C feltöltése</span><span class="sxs-lookup"><span data-stu-id="92a65-112">Uploading hello policy tooan Azure AD B2C tenant and test it</span></span>

## <a name="create-a-microsoft-account-application"></a><span data-ttu-id="92a65-113">Microsoft-fiók alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="92a65-113">Create a Microsoft account application</span></span>
<span data-ttu-id="92a65-114">toouse Microsoft fiók az Azure Active Directory (Azure AD) B2C identitás-szolgáltatóként, kell toocreate egy Microsoft-fiók alkalmazást, és adja meg azt a hello megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="92a65-114">toouse Microsoft account as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a Microsoft account application and supply it with hello right parameters.</span></span> <span data-ttu-id="92a65-115">Microsoft-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="92a65-115">You need a Microsoft account.</span></span> <span data-ttu-id="92a65-116">Ha még nincs fiókja, látogasson el [https://www.live.com/](https://www.live.com/).</span><span class="sxs-lookup"><span data-stu-id="92a65-116">If you don’t have one, visit [https://www.live.com/](https://www.live.com/).</span></span>

1.  <span data-ttu-id="92a65-117">Nyissa meg toohello [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) , és jelentkezzen be Microsoft-fiók hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="92a65-117">Go toohello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) and sign in with your Microsoft account credentials.</span></span>
2.  <span data-ttu-id="92a65-118">Kattintson a **hozzáadhat egy alkalmazást**.</span><span class="sxs-lookup"><span data-stu-id="92a65-118">Click **Add an app**.</span></span>

    ![Microsoft fiók - alkalmazás hozzáadása](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-new-app.png)

3.  <span data-ttu-id="92a65-120">Adja meg egy **neve** az alkalmazás **lépjen kapcsolatba az e-mailek**, törölje a jelet **tudassa velünk megkezdésében súgó** kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="92a65-120">Provide a **Name** for your application, **Contact email**, uncheck **Let us help you get started** and click **Create**.</span></span>

    ![Microsoft-fiók – az alkalmazás regisztrálása](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-name.png)

4.  <span data-ttu-id="92a65-122">Másolja a hello értékének **alkalmazásazonosító**. Esetleg szükség lenne rá tooconfigure Microsoft-fiók identitás-szolgáltatóként az Ön bérelt szolgáltatásának.</span><span class="sxs-lookup"><span data-stu-id="92a65-122">Copy hello value of **Application Id**. You need it tooconfigure Microsoft account as an identity provider in your tenant.</span></span>

    ![Microsoft-fiók - alkalmazásazonosító hello értékének másolása](media/active-directory-b2c-custom-setup-ms-account-idp/msa-app-id.png)

5.  <span data-ttu-id="92a65-124">Kattintson a **Hozzáadás platform**</span><span class="sxs-lookup"><span data-stu-id="92a65-124">Click on **Add platform**</span></span>

    ![Microsoft fiók - platform hozzáadása](media/active-directory-b2c-custom-setup-ms-account-idp/msa-add-platform.png)

6.  <span data-ttu-id="92a65-126">Hello platform listából válassza ki a **webes**.</span><span class="sxs-lookup"><span data-stu-id="92a65-126">From hello platform list, choose **Web**.</span></span>

    ![Microsoft-fiók – hello platform listából válassza a webes](media/active-directory-b2c-custom-setup-ms-account-idp/msa-web.png)

7.  <span data-ttu-id="92a65-128">Adja meg `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` a hello **átirányítási URI-azonosítók** mező.</span><span class="sxs-lookup"><span data-stu-id="92a65-128">Enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in hello **Redirect URIs** field.</span></span> <span data-ttu-id="92a65-129">Cserélje le **{tenant}** a bérlő nevű (például contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="92a65-129">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span>

    ![Microsoft-fiók - készlet átirányítási URL-címek](media/active-directory-b2c-custom-setup-ms-account-idp/msa-redirect-url.png)

8.  <span data-ttu-id="92a65-131">Kattintson a **új jelszó létrehozása** alatt hello **alkalmazás titkos kulcsok** szakasz.</span><span class="sxs-lookup"><span data-stu-id="92a65-131">Click on **Generate New Password** under hello **Application Secrets** section.</span></span> <span data-ttu-id="92a65-132">Másolja a hello új jelszót a képernyőn jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="92a65-132">Copy hello new password displayed on screen.</span></span> <span data-ttu-id="92a65-133">Esetleg szükség lenne rá tooconfigure Microsoft-fiók identitás-szolgáltatóként az Ön bérelt szolgáltatásának.</span><span class="sxs-lookup"><span data-stu-id="92a65-133">You need it tooconfigure Microsoft account as an identity provider in your tenant.</span></span> <span data-ttu-id="92a65-134">Ez a jelszó nem egy fontos biztonsági hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="92a65-134">This password is an important security credential.</span></span>

    ![Microsoft fiók – új jelszó létrehozása](media/active-directory-b2c-custom-setup-ms-account-idp/msa-generate-new-password.png)

    ![Microsoft-fiók – hello új jelszó másolása](media/active-directory-b2c-custom-setup-ms-account-idp/msa-new-password.png)

9.  <span data-ttu-id="92a65-137">Hello jelölőnégyzetet, amely szerint **Live SDK támogatási** alatt hello **speciális beállítások** szakasz.</span><span class="sxs-lookup"><span data-stu-id="92a65-137">Check hello box that says **Live SDK support** under hello **Advanced Options** section.</span></span> <span data-ttu-id="92a65-138">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="92a65-138">Click **Save**.</span></span>

    ![Microsoft-fiók - Live SDK-támogatás](media/active-directory-b2c-custom-setup-ms-account-idp/msa-live-sdk-support.png)

## <a name="add-hello-microsoft-account-application-key-tooazure-ad-b2c"></a><span data-ttu-id="92a65-140">Hello Microsoft fiók alkalmazás fő tooAzure AD B2C hozzáadása</span><span class="sxs-lookup"><span data-stu-id="92a65-140">Add hello Microsoft account application key tooAzure AD B2C</span></span>
<span data-ttu-id="92a65-141">Az összevonáshoz Microsoft-fiókkal rendelkező szükség van egy ügyfélkulcsot a Microsoft-fiók tootrust az Azure AD B2C hello alkalmazás nevében.</span><span class="sxs-lookup"><span data-stu-id="92a65-141">Federation with Microsoft accounts requires a client secret for Microsoft account tootrust Azure AD B2C on behalf of hello application.</span></span> <span data-ttu-id="92a65-142">A Microsoft-fiók alkalmazás az Azure AD B2C bérlő secert kell toostore:</span><span class="sxs-lookup"><span data-stu-id="92a65-142">You need toostore your Microsoft account application secert in Azure AD B2C tenant:</span></span>   

1.  <span data-ttu-id="92a65-143">Nyissa meg tooyour az Azure AD B2C bérlő, és válassza **B2C beállítások** > **identitás élmény keretrendszer**</span><span class="sxs-lookup"><span data-stu-id="92a65-143">Go tooyour Azure AD B2C tenant, and select **B2C Settings** > **Identity Experience Framework**</span></span>
2.  <span data-ttu-id="92a65-144">Válassza ki **házirend kulcsok** tooview hello kulcsok, amely az Ön bérelt szolgáltatásának.</span><span class="sxs-lookup"><span data-stu-id="92a65-144">Select **Policy Keys** tooview hello keys available in your tenant.</span></span>
3.  <span data-ttu-id="92a65-145">Kattintson a **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="92a65-145">Click **+Add**.</span></span>
4.  <span data-ttu-id="92a65-146">A **beállítások**, használjon **manuális**.</span><span class="sxs-lookup"><span data-stu-id="92a65-146">For **Options**, use **Manual**.</span></span>
5.  <span data-ttu-id="92a65-147">A **neve**, használjon `MSASecret`.</span><span class="sxs-lookup"><span data-stu-id="92a65-147">For **Name**, use `MSASecret`.</span></span>  
    <span data-ttu-id="92a65-148">hello előtag `B2C_1A_` előfordulhat, hogy automatikusan hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="92a65-148">hello prefix `B2C_1A_` might be added automatically.</span></span>
6.  <span data-ttu-id="92a65-149">A hello **titkos** mezőbe írja be a Microsoft alkalmazás titkos kulcs a https://apps.dev.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="92a65-149">In hello **Secret** box, enter your Microsoft application secret from https://apps.dev.microsoft.com</span></span>
7.  <span data-ttu-id="92a65-150">A **kulcshasználat**, használjon **aláírás**.</span><span class="sxs-lookup"><span data-stu-id="92a65-150">For **Key usage**, use **Signature**.</span></span>
8.  <span data-ttu-id="92a65-151">Kattintson a **Create** (Létrehozás) gombra</span><span class="sxs-lookup"><span data-stu-id="92a65-151">Click **Create**</span></span>
9.  <span data-ttu-id="92a65-152">Győződjön meg arról, hogy létrehozott hello kulcs `B2C_1A_MSASecret`.</span><span class="sxs-lookup"><span data-stu-id="92a65-152">Confirm that you've created hello key `B2C_1A_MSASecret`.</span></span>

## <a name="add-a-claims-provider-in-your-extension-policy"></a><span data-ttu-id="92a65-153">A jogcím-szolgáltató hozzáadása a bővítmény házirend</span><span class="sxs-lookup"><span data-stu-id="92a65-153">Add a claims provider in your extension policy</span></span>
<span data-ttu-id="92a65-154">Kívánt felhasználók toosign Microsoft Account, ha szüksége toodefine Microsoft Account jogcím-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="92a65-154">If you want users toosign in by using Microsoft Account, you need toodefine Microsoft Account as a claims provider.</span></span> <span data-ttu-id="92a65-155">Más szóval kell toospecify olyan végponttal, amely az Azure AD B2C-vel kommunikál.</span><span class="sxs-lookup"><span data-stu-id="92a65-155">In other words, you need toospecify an endpoint that Azure AD B2C communicates with.</span></span> <span data-ttu-id="92a65-156">hello végpontja biztosítja, hogy egy adott felhasználó hitelesítette az Azure AD B2C tooverify által használt jogcímek egy készletét.</span><span class="sxs-lookup"><span data-stu-id="92a65-156">hello endpoint provides a set of claims that are used by Azure AD B2C tooverify that a specific user has authenticated.</span></span>

<span data-ttu-id="92a65-157">Microsoft Account meghatározni, a Jogcímszolgáltatók hozzáadásával `<ClaimsProvider>` csomópont a bővítmény a házirend-fájlban:</span><span class="sxs-lookup"><span data-stu-id="92a65-157">Define Microsoft Account as a claims provider, by adding `<ClaimsProvider>` node in your extension policy file:</span></span>

1.  <span data-ttu-id="92a65-158">A munkakönyvtár hello bővítmény házirendfájl (TrustFrameworkExtensions.xml) megnyitása.</span><span class="sxs-lookup"><span data-stu-id="92a65-158">Open hello extension policy file (TrustFrameworkExtensions.xml) from your working directory.</span></span> <span data-ttu-id="92a65-159">Ha egy XML-szerkesztőt kell [próbálja meg a Visual Studio Code](https://code.visualstudio.com/download), egy egyszerű platformfüggetlen-szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="92a65-159">If you need an XML editor, [try Visual Studio Code](https://code.visualstudio.com/download), a lightweight cross-platform editor.</span></span>
2.  <span data-ttu-id="92a65-160">Hello található `<ClaimsProviders>` szakasz</span><span class="sxs-lookup"><span data-stu-id="92a65-160">Find hello `<ClaimsProviders>` section</span></span>
3.  <span data-ttu-id="92a65-161">Adja hozzá a következő XML-részletet a hello `ClaimsProviders` elem:</span><span class="sxs-lookup"><span data-stu-id="92a65-161">Add following XML snippet under hello `ClaimsProviders` element:</span></span>

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

4.  <span data-ttu-id="92a65-162">Cserélje le `client_id` értékének a Microsoft Account alkalmazás ügyfél-azonosítóját</span><span class="sxs-lookup"><span data-stu-id="92a65-162">Replace `client_id` value with your Microsoft Account application client Id</span></span>

5.  <span data-ttu-id="92a65-163">Hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="92a65-163">Save hello file.</span></span>

## <a name="register-hello-microsoft-account-claims-provider-toosign-up-or-sign-in-user-journey"></a><span data-ttu-id="92a65-164">Hello Microsoft Account jogcímeket szolgáltató tooSign mentése vagy a bejelentkezési felhasználói út regisztrálása</span><span class="sxs-lookup"><span data-stu-id="92a65-164">Register hello Microsoft Account claims provider tooSign up or Sign-in user journey</span></span>

<span data-ttu-id="92a65-165">Ezen a ponton hello identitásszolgáltató be van állítva, de hello sign-Close-Up/sign-in képernyők egyik nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="92a65-165">At this point, hello identity provider has been set up, but it’s not available in any of hello sign-up/sign-in screens.</span></span> <span data-ttu-id="92a65-166">Most tooadd hello Microsoft Account identity provider tooyour felhasználói `SignUpOrSignIn` felhasználói út.</span><span class="sxs-lookup"><span data-stu-id="92a65-166">Now you need tooadd hello Microsoft Account identity provider tooyour user `SignUpOrSignIn` user journey.</span></span> <span data-ttu-id="92a65-167">toomake akkor érhető el, egy meglévő sablon felhasználói út duplikált létrehozhatunk.</span><span class="sxs-lookup"><span data-stu-id="92a65-167">toomake it available, we create a duplicate of an existing template user journey.</span></span>  <span data-ttu-id="92a65-168">Azt adja hello Microsoft Account identitásszolgáltató:</span><span class="sxs-lookup"><span data-stu-id="92a65-168">Then we add hello Microsoft Account identity provider:</span></span>

> [!NOTE]
>
><span data-ttu-id="92a65-169">Ha korábban kimásolt hello `<UserJourneys>` elem a házirend toohello kiterjesztésű fájl alap fájlból `TrustFrameworkExtensions.xml`, kihagyhatja toothis részt.</span><span class="sxs-lookup"><span data-stu-id="92a65-169">If you previously copied hello `<UserJourneys>` element from base file of your policy toohello extension file `TrustFrameworkExtensions.xml`, you can skip toothis section.</span></span>

1.  <span data-ttu-id="92a65-170">Nyissa meg a hello alap fájlt a házirend (például TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="92a65-170">Open hello base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
2.  <span data-ttu-id="92a65-171">Hello található `<UserJourneys>` elemet, és másolja hello teljes tartalma `<UserJourneys>` csomópont.</span><span class="sxs-lookup"><span data-stu-id="92a65-171">Find hello `<UserJourneys>` element and copy hello entire content of `<UserJourneys>` node.</span></span>
3.  <span data-ttu-id="92a65-172">Nyissa meg a hello bővítményfájl (például TrustFrameworkExtensions.xml), és megkeresi hello `<UserJourneys>` elemet.</span><span class="sxs-lookup"><span data-stu-id="92a65-172">Open hello extension file (for example, TrustFrameworkExtensions.xml) and find hello `<UserJourneys>` element.</span></span> <span data-ttu-id="92a65-173">Ha hello elem nem létezik, vegyen fel egyet.</span><span class="sxs-lookup"><span data-stu-id="92a65-173">If hello element doesn't exist, add one.</span></span>
4.  <span data-ttu-id="92a65-174">Illessze be a teljes tartalma hello `<UserJournesy>` hello gyermekeként másolt csomópont `<UserJourneys>` elemet.</span><span class="sxs-lookup"><span data-stu-id="92a65-174">Paste hello entire content of `<UserJournesy>` node that you copied as a child of hello `<UserJourneys>` element.</span></span>

### <a name="display-hello-button"></a><span data-ttu-id="92a65-175">Megjelenítési hello gomb</span><span class="sxs-lookup"><span data-stu-id="92a65-175">Display hello button</span></span>
<span data-ttu-id="92a65-176">Hello `<ClaimsProviderSelections>` elem definiálja a jogcímeket szolgáltató tanúsítványválasztási beállítások hello listáját és sorrendjét.</span><span class="sxs-lookup"><span data-stu-id="92a65-176">hello `<ClaimsProviderSelections>` element defines hello list of claims provider selection options and their order.</span></span>  <span data-ttu-id="92a65-177">`<ClaimsProviderSelection>`az elem hasonló tooan identitás szolgáltató gombjára egy sign-Close-Up/bejelentkezési oldalára.</span><span class="sxs-lookup"><span data-stu-id="92a65-177">`<ClaimsProviderSelection>` element is analogous tooan identity provider button on a sign-up/sign-in page.</span></span> <span data-ttu-id="92a65-178">Ha ad hozzá egy `<ClaimsProviderSelection>` elem Microsoft-fiókot, egy új gomb megjelenik, amikor egy felhasználó fájljai hello oldalon.</span><span class="sxs-lookup"><span data-stu-id="92a65-178">If you add a `<ClaimsProviderSelection>` element for Microsoft account, a new button shows up when a user lands on hello page.</span></span> <span data-ttu-id="92a65-179">tooadd ezt az elemet:</span><span class="sxs-lookup"><span data-stu-id="92a65-179">tooadd this element:</span></span>

1.  <span data-ttu-id="92a65-180">Hello található `<UserJourney>` tartalmazó csomópont `Id="SignUpOrSignIn"` a másolt hello felhasználói út.</span><span class="sxs-lookup"><span data-stu-id="92a65-180">Find hello `<UserJourney>` node that includes `Id="SignUpOrSignIn"` in hello user journey that you copied.</span></span>
2.  <span data-ttu-id="92a65-181">Keresse meg a hello `<OrchestrationStep>` tartalmazó csomópont`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="92a65-181">Locate hello `<OrchestrationStep>` node that includes `Order="1"`</span></span>
3.  <span data-ttu-id="92a65-182">Adja hozzá a következő XML-részletet a `<ClaimsProviderSelections>` csomópont:</span><span class="sxs-lookup"><span data-stu-id="92a65-182">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-hello-button-tooan-action"></a><span data-ttu-id="92a65-183">Hivatkozás hello tooan megnyomásakor</span><span class="sxs-lookup"><span data-stu-id="92a65-183">Link hello button tooan action</span></span>
<span data-ttu-id="92a65-184">Most, hogy a gomb helyen, toolink kell azt tooan művelet.</span><span class="sxs-lookup"><span data-stu-id="92a65-184">Now that you have a button in place, you need toolink it tooan action.</span></span> <span data-ttu-id="92a65-185">hello művelet, ebben az esetben, akkor az Azure AD B2C toocommunicate Microsoft Account tooreceive a jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="92a65-185">hello action, in this case, is for Azure AD B2C toocommunicate with Microsoft Account tooreceive a token.</span></span> <span data-ttu-id="92a65-186">Hivatkozás hello tooan megnyomásakor létrehozhatja, ha a Microsoft Account jogcímeket szolgáltató műszaki hello-profil:</span><span class="sxs-lookup"><span data-stu-id="92a65-186">Link hello button tooan action by linking hello technical profile for your Microsoft Account claims provider:</span></span>

1.  <span data-ttu-id="92a65-187">Hello található `<OrchestrationStep>` is tartalmazó `Order="2"` a hello `<UserJourney>` csomópont.</span><span class="sxs-lookup"><span data-stu-id="92a65-187">Find hello `<OrchestrationStep>` that includes `Order="2"` in hello `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="92a65-188">Adja hozzá a következő XML-részletet a `<ClaimsExchanges>` csomópont:</span><span class="sxs-lookup"><span data-stu-id="92a65-188">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

> [!NOTE]
>
>   * <span data-ttu-id="92a65-189">Győződjön meg arról hello `Id` hello azonos érték, amely rendelkezik `TargetClaimsExchangeId` az előző szakaszban hello</span><span class="sxs-lookup"><span data-stu-id="92a65-189">Ensure hello `Id` has hello same value as that of `TargetClaimsExchangeId` in hello preceding section</span></span>
>   * <span data-ttu-id="92a65-190">Győződjön meg arról `TechnicalProfileReferenceId` toohello műszaki profil korábbi (MSA-OIDC) létrehozott beállítása.</span><span class="sxs-lookup"><span data-stu-id="92a65-190">Ensure `TechnicalProfileReferenceId` ID is set toohello technical profile you created earlier (MSA-OIDC).</span></span>

## <a name="upload-hello-policy-tooyour-tenant"></a><span data-ttu-id="92a65-191">Hello házirend tooyour bérlői feltöltése</span><span class="sxs-lookup"><span data-stu-id="92a65-191">Upload hello policy tooyour tenant</span></span>
1.  <span data-ttu-id="92a65-192">A hello [Azure-portálon](https://portal.azure.com), hello átváltani [az Azure AD B2C-bérlő kontextusában](active-directory-b2c-navigate-to-b2c-context.md), és nyissa meg hello **az Azure AD B2C** panelen.</span><span class="sxs-lookup"><span data-stu-id="92a65-192">In hello [Azure portal](https://portal.azure.com), switch into hello [context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and open hello **Azure AD B2C** blade.</span></span>
2.  <span data-ttu-id="92a65-193">Válassza ki **identitás élmény keretrendszer**.</span><span class="sxs-lookup"><span data-stu-id="92a65-193">Select **Identity Experience Framework**.</span></span>
3.  <span data-ttu-id="92a65-194">Nyissa meg hello **házirend** panelen.</span><span class="sxs-lookup"><span data-stu-id="92a65-194">Open hello **All Policies** blade.</span></span>
4.  <span data-ttu-id="92a65-195">Válassza ki **házirend feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="92a65-195">Select **Upload Policy**.</span></span>
5.  <span data-ttu-id="92a65-196">Ellenőrizze **hello házirend felülírja, ha létezik** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="92a65-196">Check **Overwrite hello policy if it exists** box.</span></span>
6.  <span data-ttu-id="92a65-197">**Töltse fel** TrustFrameworkExtensions.xml, és győződjön meg arról, hogy az érvényesítése nem hiúsul meg hello</span><span class="sxs-lookup"><span data-stu-id="92a65-197">**Upload** TrustFrameworkExtensions.xml and ensure that it does not fail hello validation</span></span>

## <a name="test-hello-custom-policy-by-using-run-now"></a><span data-ttu-id="92a65-198">Tesztelje hello egyéni házirend segítségével futtatása most</span><span class="sxs-lookup"><span data-stu-id="92a65-198">Test hello custom policy by using Run Now</span></span>

1.  <span data-ttu-id="92a65-199">Nyissa meg **az Azure AD B2C beállítások** , és túl**identitás élmény keretrendszer**.</span><span class="sxs-lookup"><span data-stu-id="92a65-199">Open **Azure AD B2C Settings** and go too**Identity Experience Framework**.</span></span>
> [!NOTE]
>
><span data-ttu-id="92a65-200">**Futtassa most** legalább egy alkalmazás toobe preregistered hello tenant igényel.</span><span class="sxs-lookup"><span data-stu-id="92a65-200">**Run now** requires at least one application toobe preregistered on hello tenant.</span></span> <span data-ttu-id="92a65-201">Hogyan tooregister alkalmazások: toolearn hello Azure AD B2C [Ismerkedés](active-directory-b2c-get-started.md) cikk vagy hello [regisztrációja](active-directory-b2c-app-registration.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="92a65-201">toolearn how tooregister applications, see hello Azure AD B2C [Get started](active-directory-b2c-get-started.md) article or hello [Application registration](active-directory-b2c-app-registration.md) article.</span></span>
2.  <span data-ttu-id="92a65-202">Nyissa meg **B2C_1A_signup_signin**, hello feltöltött függő entitásonkénti (RP) egyéni házirendet.</span><span class="sxs-lookup"><span data-stu-id="92a65-202">Open **B2C_1A_signup_signin**, hello relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="92a65-203">Válassza ki **futtatása most**.</span><span class="sxs-lookup"><span data-stu-id="92a65-203">Select **Run now**.</span></span>
3.  <span data-ttu-id="92a65-204">A Microsoft-fiók használatával képes toosign kell lennie.</span><span class="sxs-lookup"><span data-stu-id="92a65-204">You should be able toosign in using Microsoft account.</span></span>

## <a name="optional-register-hello-microsoft-account-claims-provider-tooprofile-edit-user-journey"></a><span data-ttu-id="92a65-205">[Választható] Hello Microsoft Account jogcímeket szolgáltató tooProfile szerkesztheti a felhasználói út regisztrálása</span><span class="sxs-lookup"><span data-stu-id="92a65-205">[Optional] Register hello Microsoft Account claims provider tooProfile-Edit user journey</span></span>
<span data-ttu-id="92a65-206">Érdemes lehet tooadd hello Microsoft Account identitásszolgáltató is tooyour felhasználói `ProfileEdit` felhasználói út.</span><span class="sxs-lookup"><span data-stu-id="92a65-206">You may want tooadd hello Microsoft Account identity provider also tooyour user `ProfileEdit` user journey.</span></span> <span data-ttu-id="92a65-207">toomake teheti, hogy ismételje meg a hello utolsó két lépést:</span><span class="sxs-lookup"><span data-stu-id="92a65-207">toomake it available, we repeat hello last two steps:</span></span>

### <a name="display-hello-button"></a><span data-ttu-id="92a65-208">Megjelenítési hello gomb</span><span class="sxs-lookup"><span data-stu-id="92a65-208">Display hello button</span></span>
1.  <span data-ttu-id="92a65-209">Nyissa meg a hello kiterjesztésű fájlt (például TrustFrameworkExtensions.xml) házirend.</span><span class="sxs-lookup"><span data-stu-id="92a65-209">Open hello extension file of your policy (for example, TrustFrameworkExtensions.xml).</span></span>
2.  <span data-ttu-id="92a65-210">Hello található `<UserJourney>` tartalmazó csomópont `Id="ProfileEdit"` a másolt hello felhasználói út.</span><span class="sxs-lookup"><span data-stu-id="92a65-210">Find hello `<UserJourney>` node that includes `Id="ProfileEdit"` in hello user journey that you copied.</span></span>
3.  <span data-ttu-id="92a65-211">Keresse meg a hello `<OrchestrationStep>` tartalmazó csomópont`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="92a65-211">Locate hello `<OrchestrationStep>` node that includes `Order="1"`</span></span>
4.  <span data-ttu-id="92a65-212">Adja hozzá a következő XML-részletet a `<ClaimsProviderSelections>` csomópont:</span><span class="sxs-lookup"><span data-stu-id="92a65-212">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="MSAExchange" />
```

### <a name="link-hello-button-tooan-action"></a><span data-ttu-id="92a65-213">Hivatkozás hello tooan megnyomásakor</span><span class="sxs-lookup"><span data-stu-id="92a65-213">Link hello button tooan action</span></span>
1.  <span data-ttu-id="92a65-214">Hello található `<OrchestrationStep>` is tartalmazó `Order="2"` a hello `<UserJourney>` csomópont.</span><span class="sxs-lookup"><span data-stu-id="92a65-214">Find hello `<OrchestrationStep>` that includes `Order="2"` in hello `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="92a65-215">Adja hozzá a következő XML-részletet a `<ClaimsExchanges>` csomópont:</span><span class="sxs-lookup"><span data-stu-id="92a65-215">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="MSAExchange" TechnicalProfileReferenceId="MSA-OIDC" />
```

### <a name="test-hello-custom-profile-edit-policy-by-using-run-now"></a><span data-ttu-id="92a65-216">Egyéni házirend hello a profil-módosítása tesztelése Futtatás most használatával</span><span class="sxs-lookup"><span data-stu-id="92a65-216">Test hello custom Profile-Edit policy by using Run Now</span></span>
1.  <span data-ttu-id="92a65-217">Nyissa meg **az Azure AD B2C beállítások** , és túl**identitás élmény keretrendszer**.</span><span class="sxs-lookup"><span data-stu-id="92a65-217">Open **Azure AD B2C Settings** and go too**Identity Experience Framework**.</span></span>
2.  <span data-ttu-id="92a65-218">Nyissa meg **B2C_1A_ProfileEdit**, hello feltöltött függő entitásonkénti (RP) egyéni házirendet.</span><span class="sxs-lookup"><span data-stu-id="92a65-218">Open **B2C_1A_ProfileEdit**, hello relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="92a65-219">Válassza ki **futtatása most**.</span><span class="sxs-lookup"><span data-stu-id="92a65-219">Select **Run now**.</span></span>
3.  <span data-ttu-id="92a65-220">A Microsoft-fiók használatával képes toosign kell lennie.</span><span class="sxs-lookup"><span data-stu-id="92a65-220">You should be able toosign in using Microsoft account.</span></span>

## <a name="download-hello-complete-policy-files"></a><span data-ttu-id="92a65-221">Hello teljes házirend fájlok letöltése</span><span class="sxs-lookup"><span data-stu-id="92a65-221">Download hello complete policy files</span></span>
<span data-ttu-id="92a65-222">Választható lehetőség: Javasoljuk hoz létre, hogy adott esetben a saját egyéni házirend-fájlok használata a minta-fájlok helyett egyéni házirendek első lépések útmutató hello befejezése után.</span><span class="sxs-lookup"><span data-stu-id="92a65-222">Optional: We recommend you build your scenario using your own Custom policy files after completing hello Getting Started with Custom Policies walk through instead of using these sample files.</span></span>  [<span data-ttu-id="92a65-223">Házirend mintafájlok referencia</span><span class="sxs-lookup"><span data-stu-id="92a65-223">Sample policy files for reference</span></span>](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-msa-app)
