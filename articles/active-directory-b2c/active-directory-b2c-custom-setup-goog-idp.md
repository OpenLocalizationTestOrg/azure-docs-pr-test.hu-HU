---
title: "Az Azure Active Directory B2C: Hozzáadása Google + identitás-szolgáltatóként OAuth2 egyéni házirendekkel"
description: "Google + identitás-szolgáltatóként OAuth2 protokollt használó mintákkal"
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
ms.openlocfilehash: 4feff21979c9c3b3b12c7a1cae4db0121d1bd79b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-add-google-as-an-oauth2-identity-provider-using-custom-policies"></a><span data-ttu-id="efd71-103">Az Azure Active Directory B2C: Hozzáadása Google + identitás-szolgáltatóként OAuth2 egyéni házirendekkel</span><span class="sxs-lookup"><span data-stu-id="efd71-103">Azure Active Directory B2C: Add Google+ as an OAuth2 identity provider using custom policies</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="efd71-104">Ez az útmutató bemutatja, hogyan tooenable bejelentkezés Google + fiók hello segítségével a felhasználók [egyéni házirendek](active-directory-b2c-overview-custom.md).</span><span class="sxs-lookup"><span data-stu-id="efd71-104">This guide shows you how tooenable sign-in for users from Google+ account through hello use of [custom policies](active-directory-b2c-overview-custom.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="efd71-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="efd71-105">Prerequisites</span></span>

<span data-ttu-id="efd71-106">Teljes hello szükséges lépések hello [Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="efd71-106">Complete hello steps in hello [Getting started with custom policies](active-directory-b2c-get-started-custom.md) article.</span></span>

<span data-ttu-id="efd71-107">Ezek a lépések az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="efd71-107">These steps include:</span></span>

1.  <span data-ttu-id="efd71-108">A Google + fiók alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="efd71-108">Creating a Google+ account application.</span></span>
2.  <span data-ttu-id="efd71-109">Hello Google + fiók alkalmazás fő tooAzure AD B2C hozzáadása</span><span class="sxs-lookup"><span data-stu-id="efd71-109">Adding hello Google+ account application key tooAzure AD B2C</span></span>
3.  <span data-ttu-id="efd71-110">Jogcím-szolgáltató tooa házirend hozzáadása</span><span class="sxs-lookup"><span data-stu-id="efd71-110">Adding claims provider tooa policy</span></span>
4.  <span data-ttu-id="efd71-111">Hello Google + fiók jogcímeket szolgáltató tooa felhasználói út regisztrálása</span><span class="sxs-lookup"><span data-stu-id="efd71-111">Registering hello Google+ account claims provider tooa user journey</span></span>
5.  <span data-ttu-id="efd71-112">Bérlői, és tesztelik azt hello házirend tooan az Azure AD B2C feltöltése</span><span class="sxs-lookup"><span data-stu-id="efd71-112">Uploading hello policy tooan Azure AD B2C tenant and test it</span></span>

## <a name="create-a-google-account-application"></a><span data-ttu-id="efd71-113">Google + fiók-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="efd71-113">Create a Google+ account application</span></span>
<span data-ttu-id="efd71-114">toouse Google + identitás-szolgáltatóként az Azure Active Directory (Azure AD) B2C, toocreate egy Google + alapú alkalmazásba kell, és adja meg azt a hello megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="efd71-114">toouse Google+ as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a Google+ application and supply it with hello right parameters.</span></span> <span data-ttu-id="efd71-115">A Google + alkalmazás Itt regisztrálhatja: [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)</span><span class="sxs-lookup"><span data-stu-id="efd71-115">You can register a Google+ application here: [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)</span></span>

1.  <span data-ttu-id="efd71-116">Nyissa meg toohello [Google fejlesztői konzol](https://console.developers.google.com/) , és jelentkezzen be Google + fiók hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="efd71-116">Go toohello [Google Developers Console](https://console.developers.google.com/) and sign in with your Google+ account credentials.</span></span>
2.  <span data-ttu-id="efd71-117">Kattintson a **Create project**, adjon meg egy **projekt neve**, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="efd71-117">Click **Create project**, enter a **Project name**, and then click **Create**.</span></span>

3.  <span data-ttu-id="efd71-118">Kattintson a hello **projektek menü**.</span><span class="sxs-lookup"><span data-stu-id="efd71-118">Click on hello **Projects menu**.</span></span>

    ![Google + fiók – válassza ki a projekt](media/active-directory-b2c-custom-setup-goog-idp/goog-add-new-app1.png)

4.  <span data-ttu-id="efd71-120">Kattintson a hello  **+**  gombra.</span><span class="sxs-lookup"><span data-stu-id="efd71-120">Click on hello **+** button.</span></span>

    ![Google + fiók – új projekt létrehozása](media/active-directory-b2c-custom-setup-goog-idp//goog-add-new-app2.png)

5.  <span data-ttu-id="efd71-122">Adjon meg egy **projektnevet**, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="efd71-122">Enter a **Project name**, and then click **Create**.</span></span>

    ![Google + fiók – új projekt](media/active-directory-b2c-custom-setup-goog-idp//goog-app-name.png)

6.  <span data-ttu-id="efd71-124">Várjon, amíg készen áll a hello projektet, majd kattintson a hello **projektek menü**.</span><span class="sxs-lookup"><span data-stu-id="efd71-124">Wait until hello project is ready and click on hello **Projects menu**.</span></span>

    ![Google + fiók - Várjon, amíg az új projekt készen toouse](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app1.png)

7.  <span data-ttu-id="efd71-126">Kattintson a a projekt nevét.</span><span class="sxs-lookup"><span data-stu-id="efd71-126">Click on your project name.</span></span>

    ![Google + fiók – új projekt válassza hello](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app2.png)

8.  <span data-ttu-id="efd71-128">Kattintson a **API Manager** majd **hitelesítő adatok** a bal oldali navigációs hello.</span><span class="sxs-lookup"><span data-stu-id="efd71-128">Click **API Manager** and then click **Credentials** in hello left navigation.</span></span>
9.  <span data-ttu-id="efd71-129">Kattintson a hello **OAuth-hozzájárulás képernyő** hello felső fülre.</span><span class="sxs-lookup"><span data-stu-id="efd71-129">Click hello **OAuth consent screen** tab at hello top.</span></span>

    ![Google + fiók - beállítása OAuth hozzájárulási képernyő](media/active-directory-b2c-custom-setup-goog-idp/goog-add-cred.png)

10.  <span data-ttu-id="efd71-131">Válassza ki vagy adjon meg érvényes **E-mail cím**, adjon meg egy **Terméknév**, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="efd71-131">Select or specify a valid **Email address**, provide a **Product name**, and click **Save**.</span></span>

    ![Google + - alkalmazás hitelesítő adatait](media/active-directory-b2c-custom-setup-goog-idp/goog-consent-screen.png)

11.  <span data-ttu-id="efd71-133">Kattintson a **új hitelesítő adatok** majd **OAuth-Ügyfélazonosító**.</span><span class="sxs-lookup"><span data-stu-id="efd71-133">Click **New credentials** and then choose **OAuth client ID**.</span></span>

    ![Google + - hozható létre új alkalmazás hitelesítő adatait](media/active-directory-b2c-custom-setup-goog-idp/goog-add-oauth2-client-id.png)

12.  <span data-ttu-id="efd71-135">A **alkalmazástípus**, jelölje be **webes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="efd71-135">Under **Application type**, select **Web application**.</span></span>

    ![Google + - alkalmazástípus kiválasztása](media/active-directory-b2c-custom-setup-goog-idp/goog-web-app.png)

13.  <span data-ttu-id="efd71-137">Adjon meg egy **neve** adja meg az alkalmazás `https://login.microsoftonline.com` a hello **JavaScript engedélyezett eredeteket** mező, és `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` a hello **jogosult átirányítási URI-azonosítók**mező.</span><span class="sxs-lookup"><span data-stu-id="efd71-137">Provide a **Name** for your application, enter `https://login.microsoftonline.com` in hello **Authorized JavaScript origins** field, and `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in hello **Authorized redirect URIs** field.</span></span> <span data-ttu-id="efd71-138">Cserélje le **{tenant}** a bérlő nevű (például contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="efd71-138">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span> <span data-ttu-id="efd71-139">Hello **{tenant}** érték kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="efd71-139">hello **{tenant}** value is case-sensitive.</span></span> <span data-ttu-id="efd71-140">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="efd71-140">Click **Create**.</span></span>

    ![Google + - JavaScript engedélyezett eredetet adjon meg, és az átirányítási URI](media/active-directory-b2c-custom-setup-goog-idp/goog-create-client-id.png)

14.  <span data-ttu-id="efd71-142">Hello értékeit másolja **ügyfél-azonosító** és **ügyfélkulcs**.</span><span class="sxs-lookup"><span data-stu-id="efd71-142">Copy hello values of **Client Id** and **Client secret**.</span></span> <span data-ttu-id="efd71-143">A bérlő kell mindkét tooconfigure Google + identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="efd71-143">You need both tooconfigure Google+ as an identity provider in your tenant.</span></span> <span data-ttu-id="efd71-144">**Titkos ügyfélkulcsot** van egy fontos biztonsági hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="efd71-144">**Client secret** is an important security credential.</span></span>

    ![Google + - ügyfél-azonosító és az ügyfél titkos másolat hello értékek](media/active-directory-b2c-custom-setup-goog-idp/goog-client-secret.png)

## <a name="add-hello-google-account-application-key-tooazure-ad-b2c"></a><span data-ttu-id="efd71-146">Hello Google + fiók alkalmazás fő tooAzure AD B2C hozzáadása</span><span class="sxs-lookup"><span data-stu-id="efd71-146">Add hello Google+ account application key tooAzure AD B2C</span></span>
<span data-ttu-id="efd71-147">Az összevonáshoz Google + fiókok szükség van egy ügyfélkulcsot fiók tootrust Google + az Azure AD B2C hello alkalmazás nevében.</span><span class="sxs-lookup"><span data-stu-id="efd71-147">Federation with Google+ accounts requires a client secret for Google+ account tootrust Azure AD B2C on behalf of hello application.</span></span> <span data-ttu-id="efd71-148">A Google + alkalmazás titkos kulcs az Azure AD B2C bérlő kell toostore:</span><span class="sxs-lookup"><span data-stu-id="efd71-148">You need toostore your Google+ application secret in Azure AD B2C tenant:</span></span>  

1.  <span data-ttu-id="efd71-149">Nyissa meg tooyour az Azure AD B2C bérlő, és válassza **B2C beállítások** > **identitás élmény keretrendszer**</span><span class="sxs-lookup"><span data-stu-id="efd71-149">Go tooyour Azure AD B2C tenant, and select **B2C Settings** > **Identity Experience Framework**</span></span>
2.  <span data-ttu-id="efd71-150">Válassza ki **házirend kulcsok** tooview hello kulcsok, amely az Ön bérelt szolgáltatásának.</span><span class="sxs-lookup"><span data-stu-id="efd71-150">Select **Policy Keys** tooview hello keys available in your tenant.</span></span>
3.  <span data-ttu-id="efd71-151">Kattintson a **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="efd71-151">Click **+Add**.</span></span>
4.  <span data-ttu-id="efd71-152">A **beállítások**, használjon **manuális**.</span><span class="sxs-lookup"><span data-stu-id="efd71-152">For **Options**, use **Manual**.</span></span>
5.  <span data-ttu-id="efd71-153">A **neve**, használjon `GoogleSecret`.</span><span class="sxs-lookup"><span data-stu-id="efd71-153">For **Name**, use `GoogleSecret`.</span></span>  
    <span data-ttu-id="efd71-154">hello előtag `B2C_1A_` előfordulhat, hogy automatikusan hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="efd71-154">hello prefix `B2C_1A_` might be added automatically.</span></span>
6.  <span data-ttu-id="efd71-155">A hello **titkos** mezőbe írja be a Microsoft alkalmazás titkos kulcs a https://apps.dev.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="efd71-155">In hello **Secret** box, enter your Microsoft application secret from https://apps.dev.microsoft.com</span></span>
7.  <span data-ttu-id="efd71-156">A **kulcshasználat**, használjon **aláírás**.</span><span class="sxs-lookup"><span data-stu-id="efd71-156">For **Key usage**, use **Signature**.</span></span>
8.  <span data-ttu-id="efd71-157">Kattintson a **Create** (Létrehozás) gombra</span><span class="sxs-lookup"><span data-stu-id="efd71-157">Click **Create**</span></span>
9.  <span data-ttu-id="efd71-158">Győződjön meg arról, hogy létrehozott hello kulcs `B2C_1A_GoogleSecret`.</span><span class="sxs-lookup"><span data-stu-id="efd71-158">Confirm that you've created hello key `B2C_1A_GoogleSecret`.</span></span>

## <a name="add-a-claims-provider-in-your-extension-policy"></a><span data-ttu-id="efd71-159">A jogcím-szolgáltató hozzáadása a bővítmény házirend</span><span class="sxs-lookup"><span data-stu-id="efd71-159">Add a claims provider in your extension policy</span></span>

<span data-ttu-id="efd71-160">Kívánt felhasználók toosign Google + fiók használatával, ha toodefine Google +-fiók szükséges, a jogcím-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="efd71-160">If you want users toosign in by using Google+ account, you need toodefine Google+ account as a claims provider.</span></span> <span data-ttu-id="efd71-161">Más szóval kell toospecify olyan végponttal, amely az Azure AD B2C-vel kommunikál.</span><span class="sxs-lookup"><span data-stu-id="efd71-161">In other words, you need toospecify an endpoint that Azure AD B2C communicates with.</span></span> <span data-ttu-id="efd71-162">hello végpontja biztosítja, hogy egy adott felhasználó hitelesítette az Azure AD B2C tooverify által használt jogcímek egy készletét.</span><span class="sxs-lookup"><span data-stu-id="efd71-162">hello endpoint provides a set of claims that are used by Azure AD B2C tooverify that a specific user has authenticated.</span></span>

<span data-ttu-id="efd71-163">Google + fiók megadása egy jogcímszolgáltatótól, mint hozzáadásával `<ClaimsProvider>` csomópont a bővítmény a házirend-fájlban:</span><span class="sxs-lookup"><span data-stu-id="efd71-163">Define Google+ Account as a claims provider, by adding `<ClaimsProvider>` node in your extension policy file:</span></span>

1.  <span data-ttu-id="efd71-164">A munkakönyvtár hello bővítmény házirendfájl (TrustFrameworkExtensions.xml) megnyitása.</span><span class="sxs-lookup"><span data-stu-id="efd71-164">Open hello extension policy file (TrustFrameworkExtensions.xml) from your working directory.</span></span> <span data-ttu-id="efd71-165">Ha egy XML-szerkesztőt kell [próbálja meg a Visual Studio Code](https://code.visualstudio.com/download), egy egyszerű platformfüggetlen-szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="efd71-165">If you need an XML editor, [try Visual Studio Code](https://code.visualstudio.com/download), a lightweight cross-platform editor.</span></span>
2.  <span data-ttu-id="efd71-166">Hello található `<ClaimsProviders>` szakasz</span><span class="sxs-lookup"><span data-stu-id="efd71-166">Find hello `<ClaimsProviders>` section</span></span>
3.  <span data-ttu-id="efd71-167">Adja hozzá a következő XML-részletet a hello hello `ClaimsProviders` elem és a név felülírandó `client_id` érték és a Google + fiók alkalmazás ügyfél-azonosító hello fájl mentése előtt.</span><span class="sxs-lookup"><span data-stu-id="efd71-167">Add hello following XML snippet under hello `ClaimsProviders` element and replace `client_id` value with your Google+ account application client ID before saving hello file.</span></span>  

```xml
<ClaimsProvider>
    <Domain>google.com</Domain>
    <DisplayName>Google</DisplayName>
    <TechnicalProfiles>
    <TechnicalProfile Id="Google-OAUTH">
        <DisplayName>Google</DisplayName>
        <Protocol Name="OAuth2" />
        <Metadata>
        <Item Key="ProviderName">google</Item>
        <Item Key="authorization_endpoint">https://accounts.google.com/o/oauth2/auth</Item>
        <Item Key="AccessTokenEndpoint">https://accounts.google.com/o/oauth2/token</Item>
        <Item Key="ClaimsEndpoint">https://www.googleapis.com/oauth2/v1/userinfo</Item>
        <Item Key="scope">email</Item>
        <Item Key="HttpBinding">POST</Item>
        <Item Key="UsePolicyInRedirectUri">0</Item>
        <Item Key="client_id">Your Google+ application ID</Item>
        </Metadata>
        <CryptographicKeys>
        <Key Id="client_secret" StorageReferenceId="B2C_1A_GoogleSecret" />
        </CryptographicKeys>
        <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="id" />
        <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email" />
        <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
        <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name" />
        <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
        <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="google.com" />
        <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
        </OutputClaims>
        <OutputClaimsTransformations>
        <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName" />
        <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName" />
        <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId" />
        <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId" />
        </OutputClaimsTransformations>
        <UseTechnicalProfileForSessionManagement ReferenceId="SM-SocialLogin" />
        <ErrorHandlers>
        <ErrorHandler>
            <ErrorResponseFormat>json</ErrorResponseFormat>
            <ResponseMatch>$[?(@@.error == 'invalid_grant')]</ResponseMatch>
            <Action>Reauthenticate</Action>
            <!--In case of authorization code used error, we don't want hello user tooselect his account again.-->
            <!--AdditionalRequestParameters Key="prompt">select_account</AdditionalRequestParameters-->
        </ErrorHandler>
        </ErrorHandlers>
    </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="register-hello-google-account-claims-provider-toosign-up-or-sign-in-user-journey"></a><span data-ttu-id="efd71-168">Hello Google + fiók jogcímeket szolgáltató tooSign regisztrálásához fel, vagy jelentkezzen be a felhasználó út</span><span class="sxs-lookup"><span data-stu-id="efd71-168">Register hello Google+ account claims provider tooSign up or Sign in user journey</span></span>

<span data-ttu-id="efd71-169">hello identitásszolgáltató be van állítva.</span><span class="sxs-lookup"><span data-stu-id="efd71-169">hello identity provider has been set up.</span></span>  <span data-ttu-id="efd71-170">Azonban nincs sem hello sign-Close-Up/sign-in képernyők áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="efd71-170">However, it is not available in any of hello sign-up/sign-in screens.</span></span> <span data-ttu-id="efd71-171">Felhasználó hozzáadása a hello Google + fiók identitását szolgáltató tooyour `SignUpOrSignIn` felhasználói út.</span><span class="sxs-lookup"><span data-stu-id="efd71-171">Add hello Google+ account identity provider tooyour user `SignUpOrSignIn` user journey.</span></span> <span data-ttu-id="efd71-172">toomake akkor érhető el, egy meglévő sablon felhasználói út duplikált létrehozhatunk.</span><span class="sxs-lookup"><span data-stu-id="efd71-172">toomake it available, we create a duplicate of an existing template user journey.</span></span>  <span data-ttu-id="efd71-173">Azt adja hello Google + fiók identitásszolgáltató:</span><span class="sxs-lookup"><span data-stu-id="efd71-173">Then we add hello Google+ account identity provider:</span></span>

>[!NOTE]
>
><span data-ttu-id="efd71-174">Ha a másolt hello `<UserJourneys>` elem a házirend toohello bővítményfájl (TrustFrameworkExtensions.xml) Alap fájlból, kihagyhatja toothis részt.</span><span class="sxs-lookup"><span data-stu-id="efd71-174">If you copied hello `<UserJourneys>` element from base file of your policy toohello extension file (TrustFrameworkExtensions.xml), you can skip toothis section.</span></span>

1.  <span data-ttu-id="efd71-175">Nyissa meg a hello alap fájlt a házirend (például TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="efd71-175">Open hello base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
2.  <span data-ttu-id="efd71-176">Hello található `<UserJourneys>` elemet, és másolja hello teljes tartalma `<UserJourneys>` csomópont.</span><span class="sxs-lookup"><span data-stu-id="efd71-176">Find hello `<UserJourneys>` element and copy hello entire content of `<UserJourneys>` node.</span></span>
3.  <span data-ttu-id="efd71-177">Nyissa meg a hello bővítményfájl (például TrustFrameworkExtensions.xml), és megkeresi hello `<UserJourneys>` elemet.</span><span class="sxs-lookup"><span data-stu-id="efd71-177">Open hello extension file (for example, TrustFrameworkExtensions.xml) and find hello `<UserJourneys>` element.</span></span> <span data-ttu-id="efd71-178">Ha hello elem nem létezik, vegyen fel egyet.</span><span class="sxs-lookup"><span data-stu-id="efd71-178">If hello element doesn't exist, add one.</span></span>
4.  <span data-ttu-id="efd71-179">Illessze be a teljes tartalma hello `<UserJournesy>` hello gyermekeként másolt csomópont `<UserJourneys>` elemet.</span><span class="sxs-lookup"><span data-stu-id="efd71-179">Paste hello entire content of `<UserJournesy>` node that you copied as a child of hello `<UserJourneys>` element.</span></span>

### <a name="display-hello-button"></a><span data-ttu-id="efd71-180">Megjelenítési hello gomb</span><span class="sxs-lookup"><span data-stu-id="efd71-180">Display hello button</span></span>
<span data-ttu-id="efd71-181">Hello `<ClaimsProviderSelections>` elem definiálja a jogcímeket szolgáltató tanúsítványválasztási beállítások hello listáját és sorrendjét.</span><span class="sxs-lookup"><span data-stu-id="efd71-181">hello `<ClaimsProviderSelections>` element defines hello list of claims provider selection options and their order.</span></span>  <span data-ttu-id="efd71-182">`<ClaimsProviderSelection>`az elem hasonló tooan identitás szolgáltató gombjára egy sign-Close-Up/bejelentkezési oldalára.</span><span class="sxs-lookup"><span data-stu-id="efd71-182">`<ClaimsProviderSelection>` element is analogous tooan identity provider button on a sign-up/sign-in page.</span></span> <span data-ttu-id="efd71-183">Ha ad hozzá egy `<ClaimsProviderSelection>` elem Google + fiókhoz, egy új gomb megjelenik, amikor egy felhasználó fájljai hello lapon.</span><span class="sxs-lookup"><span data-stu-id="efd71-183">If you add a `<ClaimsProviderSelection>` element for Google+ account, a new button shows up when a user lands on hello page.</span></span> <span data-ttu-id="efd71-184">tooadd ezt az elemet:</span><span class="sxs-lookup"><span data-stu-id="efd71-184">tooadd this element:</span></span>

1.  <span data-ttu-id="efd71-185">Hello található `<UserJourney>` tartalmazó csomópont `Id="SignUpOrSignIn"` a másolt hello felhasználói út.</span><span class="sxs-lookup"><span data-stu-id="efd71-185">Find hello `<UserJourney>` node that includes `Id="SignUpOrSignIn"` in hello user journey that you copied.</span></span>
2.  <span data-ttu-id="efd71-186">Keresse meg a hello `<OrchestrationStep>` tartalmazó csomópont`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="efd71-186">Locate hello `<OrchestrationStep>` node that includes `Order="1"`</span></span>
3.  <span data-ttu-id="efd71-187">Adja hozzá a következő XML-részletet a `<ClaimsProviderSelections>` csomópont:</span><span class="sxs-lookup"><span data-stu-id="efd71-187">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-hello-button-tooan-action"></a><span data-ttu-id="efd71-188">Hivatkozás hello tooan megnyomásakor</span><span class="sxs-lookup"><span data-stu-id="efd71-188">Link hello button tooan action</span></span>
<span data-ttu-id="efd71-189">Most, hogy a gomb helyen, toolink kell azt tooan művelet.</span><span class="sxs-lookup"><span data-stu-id="efd71-189">Now that you have a button in place, you need toolink it tooan action.</span></span> <span data-ttu-id="efd71-190">hello művelet, ebben az esetben, akkor az Azure AD B2C toocommunicate Google + tooreceive a jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="efd71-190">hello action, in this case, is for Azure AD B2C toocommunicate with Google+ account tooreceive a token.</span></span>

1.  <span data-ttu-id="efd71-191">Hello található `<OrchestrationStep>` is tartalmazó `Order="2"` a hello `<UserJourney>` csomópont.</span><span class="sxs-lookup"><span data-stu-id="efd71-191">Find hello `<OrchestrationStep>` that includes `Order="2"` in hello `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="efd71-192">Adja hozzá a következő XML-részletet a `<ClaimsExchanges>` csomópont:</span><span class="sxs-lookup"><span data-stu-id="efd71-192">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

>[!NOTE]
>
> * <span data-ttu-id="efd71-193">Győződjön meg arról hello `Id` hello azonos érték, amely rendelkezik `TargetClaimsExchangeId` az előző szakaszban hello</span><span class="sxs-lookup"><span data-stu-id="efd71-193">Ensure hello `Id` has hello same value as that of `TargetClaimsExchangeId` in hello preceding section</span></span>
> * <span data-ttu-id="efd71-194">Győződjön meg arról `TechnicalProfileReferenceId` toohello műszaki profil korábbi (Google-OAUTH) létrehozott beállítása.</span><span class="sxs-lookup"><span data-stu-id="efd71-194">Ensure `TechnicalProfileReferenceId` ID is set toohello technical profile you created earlier (Google-OAUTH).</span></span>

## <a name="upload-hello-policy-tooyour-tenant"></a><span data-ttu-id="efd71-195">Hello házirend tooyour bérlői feltöltése</span><span class="sxs-lookup"><span data-stu-id="efd71-195">Upload hello policy tooyour tenant</span></span>
1.  <span data-ttu-id="efd71-196">A hello [Azure-portálon](https://portal.azure.com), hello átváltani [az Azure AD B2C-bérlő kontextusában](active-directory-b2c-navigate-to-b2c-context.md), és nyissa meg hello **az Azure AD B2C** panelen.</span><span class="sxs-lookup"><span data-stu-id="efd71-196">In hello [Azure portal](https://portal.azure.com), switch into hello [context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and open hello **Azure AD B2C** blade.</span></span>
2.  <span data-ttu-id="efd71-197">Válassza ki **identitás élmény keretrendszer**.</span><span class="sxs-lookup"><span data-stu-id="efd71-197">Select **Identity Experience Framework**.</span></span>
3.  <span data-ttu-id="efd71-198">Nyissa meg hello **házirend** panelen.</span><span class="sxs-lookup"><span data-stu-id="efd71-198">Open hello **All Policies** blade.</span></span>
4.  <span data-ttu-id="efd71-199">Válassza ki **házirend feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="efd71-199">Select **Upload Policy**.</span></span>
5.  <span data-ttu-id="efd71-200">Ellenőrizze **hello házirend felülírja, ha létezik** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="efd71-200">Check **Overwrite hello policy if it exists** box.</span></span>
6.  <span data-ttu-id="efd71-201">**Töltse fel** TrustFrameworkExtensions.xml, és győződjön meg arról, hogy az érvényesítése nem hiúsul meg hello</span><span class="sxs-lookup"><span data-stu-id="efd71-201">**Upload** TrustFrameworkExtensions.xml and ensure that it does not fail hello validation</span></span>

## <a name="test-hello-custom-policy-by-using-run-now"></a><span data-ttu-id="efd71-202">Tesztelje hello egyéni házirend segítségével futtatása most</span><span class="sxs-lookup"><span data-stu-id="efd71-202">Test hello custom policy by using Run Now</span></span>
1.  <span data-ttu-id="efd71-203">Nyissa meg **az Azure AD B2C beállítások** , és túl**identitás élmény keretrendszer**.</span><span class="sxs-lookup"><span data-stu-id="efd71-203">Open **Azure AD B2C Settings** and go too**Identity Experience Framework**.</span></span>

    >[!NOTE]
    >
    >    <span data-ttu-id="efd71-204">**Futtassa most** legalább egy alkalmazás toobe preregistered hello tenant igényel.</span><span class="sxs-lookup"><span data-stu-id="efd71-204">**Run now** requires at least one application toobe preregistered on hello tenant.</span></span> 
    >    <span data-ttu-id="efd71-205">Hogyan tooregister alkalmazások: toolearn hello Azure AD B2C [Ismerkedés](active-directory-b2c-get-started.md) cikk vagy hello [regisztrációja](active-directory-b2c-app-registration.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="efd71-205">toolearn how tooregister applications, see hello Azure AD B2C [Get started](active-directory-b2c-get-started.md) article or hello [Application registration](active-directory-b2c-app-registration.md) article.</span></span>


2.  <span data-ttu-id="efd71-206">Nyissa meg **B2C_1A_signup_signin**, hello feltöltött függő entitásonkénti (RP) egyéni házirendet.</span><span class="sxs-lookup"><span data-stu-id="efd71-206">Open **B2C_1A_signup_signin**, hello relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="efd71-207">Válassza ki **futtatása most**.</span><span class="sxs-lookup"><span data-stu-id="efd71-207">Select **Run now**.</span></span>
3.  <span data-ttu-id="efd71-208">A Google + fiók használatával képes toosign kell lennie.</span><span class="sxs-lookup"><span data-stu-id="efd71-208">You should be able toosign in using Google+ account.</span></span>

## <a name="optional-register-hello-google-account-claims-provider-tooprofile-edit-user-journey"></a><span data-ttu-id="efd71-209">[Választható] Hello Google + fiók jogcímeket szolgáltató tooProfile szerkesztheti a felhasználói út regisztrálása</span><span class="sxs-lookup"><span data-stu-id="efd71-209">[Optional] Register hello Google+ account claims provider tooProfile-Edit user journey</span></span>
<span data-ttu-id="efd71-210">Érdemes lehet tooadd hello Google + fiók identitásszolgáltató is tooyour felhasználói `ProfileEdit` felhasználói út.</span><span class="sxs-lookup"><span data-stu-id="efd71-210">You may want tooadd hello Google+ account identity provider also tooyour user `ProfileEdit` user journey.</span></span> <span data-ttu-id="efd71-211">toomake teheti, hogy ismételje meg a hello utolsó két lépést:</span><span class="sxs-lookup"><span data-stu-id="efd71-211">toomake it available, we repeat hello last two steps:</span></span>

### <a name="display-hello-button"></a><span data-ttu-id="efd71-212">Megjelenítési hello gomb</span><span class="sxs-lookup"><span data-stu-id="efd71-212">Display hello button</span></span>
1.  <span data-ttu-id="efd71-213">Nyissa meg a hello kiterjesztésű fájlt (például TrustFrameworkExtensions.xml) házirend.</span><span class="sxs-lookup"><span data-stu-id="efd71-213">Open hello extension file of your policy (for example, TrustFrameworkExtensions.xml).</span></span>
2.  <span data-ttu-id="efd71-214">Hello található `<UserJourney>` tartalmazó csomópont `Id="ProfileEdit"` a másolt hello felhasználói út.</span><span class="sxs-lookup"><span data-stu-id="efd71-214">Find hello `<UserJourney>` node that includes `Id="ProfileEdit"` in hello user journey that you copied.</span></span>
3.  <span data-ttu-id="efd71-215">Keresse meg a hello `<OrchestrationStep>` tartalmazó csomópont`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="efd71-215">Locate hello `<OrchestrationStep>` node that includes `Order="1"`</span></span>
4.  <span data-ttu-id="efd71-216">Adja hozzá a következő XML-részletet a `<ClaimsProviderSelections>` csomópont:</span><span class="sxs-lookup"><span data-stu-id="efd71-216">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-hello-button-tooan-action"></a><span data-ttu-id="efd71-217">Hivatkozás hello tooan megnyomásakor</span><span class="sxs-lookup"><span data-stu-id="efd71-217">Link hello button tooan action</span></span>
1.  <span data-ttu-id="efd71-218">Hello található `<OrchestrationStep>` is tartalmazó `Order="2"` a hello `<UserJourney>` csomópont.</span><span class="sxs-lookup"><span data-stu-id="efd71-218">Find hello `<OrchestrationStep>` that includes `Order="2"` in hello `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="efd71-219">Adja hozzá a következő XML-részletet a `<ClaimsExchanges>` csomópont:</span><span class="sxs-lookup"><span data-stu-id="efd71-219">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

### <a name="test-hello-custom-profile-edit-policy-by-using-run-now"></a><span data-ttu-id="efd71-220">Egyéni házirend hello a profil-módosítása tesztelése Futtatás most használatával</span><span class="sxs-lookup"><span data-stu-id="efd71-220">Test hello custom Profile-Edit policy by using Run Now</span></span>

1.  <span data-ttu-id="efd71-221">Nyissa meg **az Azure AD B2C beállítások** , és túl**identitás élmény keretrendszer**.</span><span class="sxs-lookup"><span data-stu-id="efd71-221">Open **Azure AD B2C Settings** and go too**Identity Experience Framework**.</span></span>
2.  <span data-ttu-id="efd71-222">Nyissa meg **B2C_1A_ProfileEdit**, hello feltöltött függő entitásonkénti (RP) egyéni házirendet.</span><span class="sxs-lookup"><span data-stu-id="efd71-222">Open **B2C_1A_ProfileEdit**, hello relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="efd71-223">Válassza ki **futtatása most**.</span><span class="sxs-lookup"><span data-stu-id="efd71-223">Select **Run now**.</span></span>
3.  <span data-ttu-id="efd71-224">A Google + fiók használatával képes toosign kell lennie.</span><span class="sxs-lookup"><span data-stu-id="efd71-224">You should be able toosign in using Google+ account.</span></span>

## <a name="download-hello-complete-policy-files"></a><span data-ttu-id="efd71-225">Hello teljes házirend fájlok letöltése</span><span class="sxs-lookup"><span data-stu-id="efd71-225">Download hello complete policy files</span></span>
<span data-ttu-id="efd71-226">Választható lehetőség: Javasoljuk hoz létre, hogy adott esetben a saját egyéni házirend-fájlok használata a minta-fájlok helyett egyéni házirendek első lépések útmutató hello befejezése után.</span><span class="sxs-lookup"><span data-stu-id="efd71-226">Optional: We recommend you build your scenario using your own Custom policy files after completing hello Getting Started with Custom Policies walk through instead of using these sample files.</span></span>  [<span data-ttu-id="efd71-227">Házirend mintafájlok referencia</span><span class="sxs-lookup"><span data-stu-id="efd71-227">Sample policy files for reference</span></span>](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-goog-app)
