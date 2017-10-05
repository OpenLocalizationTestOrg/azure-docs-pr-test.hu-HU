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
ms.openlocfilehash: e0aaf710d230f7667fff32b50ddb64104509d740
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-b2c-add-google-as-an-oauth2-identity-provider-using-custom-policies"></a><span data-ttu-id="6f34e-103">Az Azure Active Directory B2C: Hozzáadása Google + identitás-szolgáltatóként OAuth2 egyéni házirendekkel</span><span class="sxs-lookup"><span data-stu-id="6f34e-103">Azure Active Directory B2C: Add Google+ as an OAuth2 identity provider using custom policies</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="6f34e-104">Ez az útmutató bemutatja, hogyan bejelentkezés Google + fiókból keresztül a felhasználók engedélyezése [egyéni házirendek](active-directory-b2c-overview-custom.md).</span><span class="sxs-lookup"><span data-stu-id="6f34e-104">This guide shows you how to enable sign-in for users from Google+ account through the use of [custom policies](active-directory-b2c-overview-custom.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6f34e-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6f34e-105">Prerequisites</span></span>

<span data-ttu-id="6f34e-106">Hajtsa végre a a [Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="6f34e-106">Complete the steps in the [Getting started with custom policies](active-directory-b2c-get-started-custom.md) article.</span></span>

<span data-ttu-id="6f34e-107">Ezek a lépések az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="6f34e-107">These steps include:</span></span>

1.  <span data-ttu-id="6f34e-108">A Google + fiók alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6f34e-108">Creating a Google+ account application.</span></span>
2.  <span data-ttu-id="6f34e-109">A Google + alkalmazás fiókkulcs felvétele az Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="6f34e-109">Adding the Google+ account application key to Azure AD B2C</span></span>
3.  <span data-ttu-id="6f34e-110">Egy házirend hozzáadása jogcím-szolgáltató</span><span class="sxs-lookup"><span data-stu-id="6f34e-110">Adding claims provider to a policy</span></span>
4.  <span data-ttu-id="6f34e-111">A Google + fiók jogcímszolgáltató felhasználói út regisztrálása</span><span class="sxs-lookup"><span data-stu-id="6f34e-111">Registering the Google+ account claims provider to a user journey</span></span>
5.  <span data-ttu-id="6f34e-112">A házirend feltöltése az Azure AD B2C bérlői és tesztelik azt</span><span class="sxs-lookup"><span data-stu-id="6f34e-112">Uploading the policy to an Azure AD B2C tenant and test it</span></span>

## <a name="create-a-google-account-application"></a><span data-ttu-id="6f34e-113">Google + fiók-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6f34e-113">Create a Google+ account application</span></span>
<span data-ttu-id="6f34e-114">Használatához Google + az Azure Active Directory (Azure AD) B2C identitás-szolgáltatóként kell Google +-alkalmazás létrehozása, és adja meg azt a megfelelő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="6f34e-114">To use Google+ as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a Google+ application and supply it with the right parameters.</span></span> <span data-ttu-id="6f34e-115">A Google + alkalmazás Itt regisztrálhatja: [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)</span><span class="sxs-lookup"><span data-stu-id="6f34e-115">You can register a Google+ application here: [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)</span></span>

1.  <span data-ttu-id="6f34e-116">Lépjen a [Google fejlesztői konzol](https://console.developers.google.com/) , és jelentkezzen be Google + fiók hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="6f34e-116">Go to the [Google Developers Console](https://console.developers.google.com/) and sign in with your Google+ account credentials.</span></span>
2.  <span data-ttu-id="6f34e-117">Kattintson a **Create project**, adjon meg egy **projekt neve**, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="6f34e-117">Click **Create project**, enter a **Project name**, and then click **Create**.</span></span>

3.  <span data-ttu-id="6f34e-118">Kattintson a **projektek menü**.</span><span class="sxs-lookup"><span data-stu-id="6f34e-118">Click on the **Projects menu**.</span></span>

    ![Google + fiók – válassza ki a projekt](media/active-directory-b2c-custom-setup-goog-idp/goog-add-new-app1.png)

4.  <span data-ttu-id="6f34e-120">Kattintson a  **+**  gombra.</span><span class="sxs-lookup"><span data-stu-id="6f34e-120">Click on the **+** button.</span></span>

    ![Google + fiók – új projekt létrehozása](media/active-directory-b2c-custom-setup-goog-idp//goog-add-new-app2.png)

5.  <span data-ttu-id="6f34e-122">Adjon meg egy **projektnevet**, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="6f34e-122">Enter a **Project name**, and then click **Create**.</span></span>

    ![Google + fiók – új projekt](media/active-directory-b2c-custom-setup-goog-idp//goog-app-name.png)

6.  <span data-ttu-id="6f34e-124">Várjon, amíg készen áll a projektet, majd kattintson a a **projektek menü**.</span><span class="sxs-lookup"><span data-stu-id="6f34e-124">Wait until the project is ready and click on the **Projects menu**.</span></span>

    ![Google + fiók - Várjon, amíg az új projekt használatra készen áll](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app1.png)

7.  <span data-ttu-id="6f34e-126">Kattintson a a projekt nevét.</span><span class="sxs-lookup"><span data-stu-id="6f34e-126">Click on your project name.</span></span>

    ![Google + fiók – válassza ki az új projekt](media/active-directory-b2c-custom-setup-goog-idp//goog-select-app2.png)

8.  <span data-ttu-id="6f34e-128">Kattintson a **API Manager** majd **hitelesítő adatok** a bal oldali navigációs.</span><span class="sxs-lookup"><span data-stu-id="6f34e-128">Click **API Manager** and then click **Credentials** in the left navigation.</span></span>
9.  <span data-ttu-id="6f34e-129">Kattintson a **OAuth-hozzájárulás képernyő** fülre az oldal tetején.</span><span class="sxs-lookup"><span data-stu-id="6f34e-129">Click the **OAuth consent screen** tab at the top.</span></span>

    ![Google + fiók - beállítása OAuth hozzájárulási képernyő](media/active-directory-b2c-custom-setup-goog-idp/goog-add-cred.png)

10.  <span data-ttu-id="6f34e-131">Válassza ki vagy adjon meg érvényes **E-mail cím**, adjon meg egy **Terméknév**, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="6f34e-131">Select or specify a valid **Email address**, provide a **Product name**, and click **Save**.</span></span>

    ![Google + - alkalmazás hitelesítő adatait](media/active-directory-b2c-custom-setup-goog-idp/goog-consent-screen.png)

11.  <span data-ttu-id="6f34e-133">Kattintson a **új hitelesítő adatok** majd **OAuth-Ügyfélazonosító**.</span><span class="sxs-lookup"><span data-stu-id="6f34e-133">Click **New credentials** and then choose **OAuth client ID**.</span></span>

    ![Google + - hozható létre új alkalmazás hitelesítő adatait](media/active-directory-b2c-custom-setup-goog-idp/goog-add-oauth2-client-id.png)

12.  <span data-ttu-id="6f34e-135">A **alkalmazástípus**, jelölje be **webes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6f34e-135">Under **Application type**, select **Web application**.</span></span>

    ![Google + - alkalmazástípus kiválasztása](media/active-directory-b2c-custom-setup-goog-idp/goog-web-app.png)

13.  <span data-ttu-id="6f34e-137">Adjon meg egy **neve** adja meg az alkalmazás `https://login.microsoftonline.com` a a **JavaScript engedélyezett eredeteket** mező, és `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` a a **jogosult átirányítási URI-azonosítók** a mező.</span><span class="sxs-lookup"><span data-stu-id="6f34e-137">Provide a **Name** for your application, enter `https://login.microsoftonline.com` in the **Authorized JavaScript origins** field, and `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in the **Authorized redirect URIs** field.</span></span> <span data-ttu-id="6f34e-138">Cserélje le **{tenant}** a bérlő nevű (például contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="6f34e-138">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span> <span data-ttu-id="6f34e-139">A **{tenant}** érték kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="6f34e-139">The **{tenant}** value is case-sensitive.</span></span> <span data-ttu-id="6f34e-140">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6f34e-140">Click **Create**.</span></span>

    ![Google + - JavaScript engedélyezett eredetet adjon meg, és az átirányítási URI](media/active-directory-b2c-custom-setup-goog-idp/goog-create-client-id.png)

14.  <span data-ttu-id="6f34e-142">Értékeinek másolására **ügyfél-azonosító** és **ügyfélkulcs**.</span><span class="sxs-lookup"><span data-stu-id="6f34e-142">Copy the values of **Client Id** and **Client secret**.</span></span> <span data-ttu-id="6f34e-143">Mindkettő konfigurálásához Google +-bérlőben identitás-szolgáltatóként van szüksége.</span><span class="sxs-lookup"><span data-stu-id="6f34e-143">You need both to configure Google+ as an identity provider in your tenant.</span></span> <span data-ttu-id="6f34e-144">**Titkos ügyfélkulcsot** van egy fontos biztonsági hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="6f34e-144">**Client secret** is an important security credential.</span></span>

    ![Google + - azonosító és az ügyfél ügyfélkulcs értékeinek másolására](media/active-directory-b2c-custom-setup-goog-idp/goog-client-secret.png)

## <a name="add-the-google-account-application-key-to-azure-ad-b2c"></a><span data-ttu-id="6f34e-146">A Google + alkalmazás fiókkulcs hozzáadása az Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="6f34e-146">Add the Google+ account application key to Azure AD B2C</span></span>
<span data-ttu-id="6f34e-147">A Google +-fiókokkal összevonási szükség van egy ügyfélkulcsot Google + fiók nevében az alkalmazás az Azure AD B2C megbízhatóságának.</span><span class="sxs-lookup"><span data-stu-id="6f34e-147">Federation with Google+ accounts requires a client secret for Google+ account to trust Azure AD B2C on behalf of the application.</span></span> <span data-ttu-id="6f34e-148">A Google + alkalmazás titkos kulcs tárolása az Azure AD B2C bérlő kell:</span><span class="sxs-lookup"><span data-stu-id="6f34e-148">You need to store your Google+ application secret in Azure AD B2C tenant:</span></span>  

1.  <span data-ttu-id="6f34e-149">Nyissa meg az Azure AD B2C-bérlő, és válassza ki **B2C beállítások** > **identitás élmény keretrendszer**</span><span class="sxs-lookup"><span data-stu-id="6f34e-149">Go to your Azure AD B2C tenant, and select **B2C Settings** > **Identity Experience Framework**</span></span>
2.  <span data-ttu-id="6f34e-150">Válassza ki **házirend kulcsok** érhető el a bérlői kulcsok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="6f34e-150">Select **Policy Keys** to view the keys available in your tenant.</span></span>
3.  <span data-ttu-id="6f34e-151">Kattintson a **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="6f34e-151">Click **+Add**.</span></span>
4.  <span data-ttu-id="6f34e-152">A **beállítások**, használjon **manuális**.</span><span class="sxs-lookup"><span data-stu-id="6f34e-152">For **Options**, use **Manual**.</span></span>
5.  <span data-ttu-id="6f34e-153">A **neve**, használjon `GoogleSecret`.</span><span class="sxs-lookup"><span data-stu-id="6f34e-153">For **Name**, use `GoogleSecret`.</span></span>  
    <span data-ttu-id="6f34e-154">Az előtag `B2C_1A_` előfordulhat, hogy automatikusan hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="6f34e-154">The prefix `B2C_1A_` might be added automatically.</span></span>
6.  <span data-ttu-id="6f34e-155">Az a **titkos** mezőbe írja be a Microsoft alkalmazás titkos kulcs a https://apps.dev.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="6f34e-155">In the **Secret** box, enter your Microsoft application secret from https://apps.dev.microsoft.com</span></span>
7.  <span data-ttu-id="6f34e-156">A **kulcshasználat**, használjon **aláírás**.</span><span class="sxs-lookup"><span data-stu-id="6f34e-156">For **Key usage**, use **Signature**.</span></span>
8.  <span data-ttu-id="6f34e-157">Kattintson a **Create** (Létrehozás) gombra</span><span class="sxs-lookup"><span data-stu-id="6f34e-157">Click **Create**</span></span>
9.  <span data-ttu-id="6f34e-158">Győződjön meg arról, hogy létrehozta a kulcs `B2C_1A_GoogleSecret`.</span><span class="sxs-lookup"><span data-stu-id="6f34e-158">Confirm that you've created the key `B2C_1A_GoogleSecret`.</span></span>

## <a name="add-a-claims-provider-in-your-extension-policy"></a><span data-ttu-id="6f34e-159">A jogcím-szolgáltató hozzáadása a bővítmény házirend</span><span class="sxs-lookup"><span data-stu-id="6f34e-159">Add a claims provider in your extension policy</span></span>

<span data-ttu-id="6f34e-160">Ha azt szeretné, hogy a felhasználók a Google + fiók használatával jelentkezzen be, meg kell Google + fiók megadása jogcímek szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="6f34e-160">If you want users to sign in by using Google+ account, you need to define Google+ account as a claims provider.</span></span> <span data-ttu-id="6f34e-161">Ez azt jelenti meg kell adnia egy végpontot, amely az Azure AD B2C-vel kommunikál.</span><span class="sxs-lookup"><span data-stu-id="6f34e-161">In other words, you need to specify an endpoint that Azure AD B2C communicates with.</span></span> <span data-ttu-id="6f34e-162">A végpont a jogcímek, az Azure AD B2C által használt győződjön meg arról, hogy egy adott felhasználó engedélyezést biztosít.</span><span class="sxs-lookup"><span data-stu-id="6f34e-162">The endpoint provides a set of claims that are used by Azure AD B2C to verify that a specific user has authenticated.</span></span>

<span data-ttu-id="6f34e-163">Google + fiók megadása egy jogcímszolgáltatótól, mint hozzáadásával `<ClaimsProvider>` csomópont a bővítmény a házirend-fájlban:</span><span class="sxs-lookup"><span data-stu-id="6f34e-163">Define Google+ Account as a claims provider, by adding `<ClaimsProvider>` node in your extension policy file:</span></span>

1.  <span data-ttu-id="6f34e-164">Nyissa meg a házirend bővítményfájl (TrustFrameworkExtensions.xml) a munkakönyvtárat.</span><span class="sxs-lookup"><span data-stu-id="6f34e-164">Open the extension policy file (TrustFrameworkExtensions.xml) from your working directory.</span></span> <span data-ttu-id="6f34e-165">Ha egy XML-szerkesztőt kell [próbálja meg a Visual Studio Code](https://code.visualstudio.com/download), egy egyszerű platformfüggetlen-szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="6f34e-165">If you need an XML editor, [try Visual Studio Code](https://code.visualstudio.com/download), a lightweight cross-platform editor.</span></span>
2.  <span data-ttu-id="6f34e-166">Keresés a `<ClaimsProviders>` szakasz</span><span class="sxs-lookup"><span data-stu-id="6f34e-166">Find the `<ClaimsProviders>` section</span></span>
3.  <span data-ttu-id="6f34e-167">Adja hozzá a következő XML-részletet alatt a `ClaimsProviders` elem és a név felülírandó `client_id` érték és a Google + fiók alkalmazás ügyfél-azonosító a fájl mentése előtt.</span><span class="sxs-lookup"><span data-stu-id="6f34e-167">Add the following XML snippet under the `ClaimsProviders` element and replace `client_id` value with your Google+ account application client ID before saving the file.</span></span>  

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
            <!--In case of authorization code used error, we don't want the user to select his account again.-->
            <!--AdditionalRequestParameters Key="prompt">select_account</AdditionalRequestParameters-->
        </ErrorHandler>
        </ErrorHandlers>
    </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

## <a name="register-the-google-account-claims-provider-to-sign-up-or-sign-in-user-journey"></a><span data-ttu-id="6f34e-168">A Google + fiók jogcímszolgáltató való regisztráció vagy bejelentkezés felhasználói út regisztrálása</span><span class="sxs-lookup"><span data-stu-id="6f34e-168">Register the Google+ account claims provider to Sign up or Sign in user journey</span></span>

<span data-ttu-id="6f34e-169">Az identitásszolgáltató beállítása.</span><span class="sxs-lookup"><span data-stu-id="6f34e-169">The identity provider has been set up.</span></span>  <span data-ttu-id="6f34e-170">Azonban nincs sem a sign-Close-Up/sign-in képernyők áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="6f34e-170">However, it is not available in any of the sign-up/sign-in screens.</span></span> <span data-ttu-id="6f34e-171">A Google + fiók identitásszolgáltató hozzáadása a felhasználói `SignUpOrSignIn` felhasználói út.</span><span class="sxs-lookup"><span data-stu-id="6f34e-171">Add the Google+ account identity provider to your user `SignUpOrSignIn` user journey.</span></span> <span data-ttu-id="6f34e-172">Tegye elérhetővé, azt hozzon létre egy meglévő sablon felhasználói út duplikált.</span><span class="sxs-lookup"><span data-stu-id="6f34e-172">To make it available, we create a duplicate of an existing template user journey.</span></span>  <span data-ttu-id="6f34e-173">Azt adja a Google + fiók identitásszolgáltató:</span><span class="sxs-lookup"><span data-stu-id="6f34e-173">Then we add the Google+ account identity provider:</span></span>

>[!NOTE]
>
><span data-ttu-id="6f34e-174">Ha a másolt a `<UserJourneys>` elem a bővítményfájl (TrustFrameworkExtensions.xml) a szabályzat alapszintű fájlból, kihagyhatja ezt a szakaszt.</span><span class="sxs-lookup"><span data-stu-id="6f34e-174">If you copied the `<UserJourneys>` element from base file of your policy to the extension file (TrustFrameworkExtensions.xml), you can skip to this section.</span></span>

1.  <span data-ttu-id="6f34e-175">Nyissa meg a házirend (például TrustFrameworkBase.xml) Alap fájlt.</span><span class="sxs-lookup"><span data-stu-id="6f34e-175">Open the base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
2.  <span data-ttu-id="6f34e-176">Keresés a `<UserJourneys>` elem és a teljes tartalmának másolása a `<UserJourneys>` csomópont.</span><span class="sxs-lookup"><span data-stu-id="6f34e-176">Find the `<UserJourneys>` element and copy the entire content of `<UserJourneys>` node.</span></span>
3.  <span data-ttu-id="6f34e-177">Nyissa meg a bővítmény (például TrustFrameworkExtensions.xml) fájlt, és keresse a `<UserJourneys>` elemet.</span><span class="sxs-lookup"><span data-stu-id="6f34e-177">Open the extension file (for example, TrustFrameworkExtensions.xml) and find the `<UserJourneys>` element.</span></span> <span data-ttu-id="6f34e-178">Ha az elem nem létezik, vegyen fel egyet.</span><span class="sxs-lookup"><span data-stu-id="6f34e-178">If the element doesn't exist, add one.</span></span>
4.  <span data-ttu-id="6f34e-179">Illessze be a teljes tartalmát `<UserJournesy>` csomópont gyermekeként másolt a `<UserJourneys>` elemet.</span><span class="sxs-lookup"><span data-stu-id="6f34e-179">Paste the entire content of `<UserJournesy>` node that you copied as a child of the `<UserJourneys>` element.</span></span>

### <a name="display-the-button"></a><span data-ttu-id="6f34e-180">A gomb megjelenítése</span><span class="sxs-lookup"><span data-stu-id="6f34e-180">Display the button</span></span>
<span data-ttu-id="6f34e-181">A `<ClaimsProviderSelections>` elem definiálja a jogcímeket szolgáltató tanúsítványválasztási beállítások és a sorrendjük listáját.</span><span class="sxs-lookup"><span data-stu-id="6f34e-181">The `<ClaimsProviderSelections>` element defines the list of claims provider selection options and their order.</span></span>  <span data-ttu-id="6f34e-182">`<ClaimsProviderSelection>`a elem egy identity provider gombra a sign-Close-Up/sign-in oldalán hasonló.</span><span class="sxs-lookup"><span data-stu-id="6f34e-182">`<ClaimsProviderSelection>` element is analogous to an identity provider button on a sign-up/sign-in page.</span></span> <span data-ttu-id="6f34e-183">Ha ad hozzá egy `<ClaimsProviderSelection>` elem Google + fiókhoz, egy új gomb megjelenik, amikor egy felhasználó fájljai az oldalon.</span><span class="sxs-lookup"><span data-stu-id="6f34e-183">If you add a `<ClaimsProviderSelection>` element for Google+ account, a new button shows up when a user lands on the page.</span></span> <span data-ttu-id="6f34e-184">Ez az elem hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="6f34e-184">To add this element:</span></span>

1.  <span data-ttu-id="6f34e-185">Keresés a `<UserJourney>` tartalmazó csomópont `Id="SignUpOrSignIn"` a a felhasználók utazás másolt.</span><span class="sxs-lookup"><span data-stu-id="6f34e-185">Find the `<UserJourney>` node that includes `Id="SignUpOrSignIn"` in the user journey that you copied.</span></span>
2.  <span data-ttu-id="6f34e-186">Keresse meg a `<OrchestrationStep>` tartalmazó csomópont`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="6f34e-186">Locate the `<OrchestrationStep>` node that includes `Order="1"`</span></span>
3.  <span data-ttu-id="6f34e-187">Adja hozzá a következő XML-részletet a `<ClaimsProviderSelections>` csomópont:</span><span class="sxs-lookup"><span data-stu-id="6f34e-187">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-the-button-to-an-action"></a><span data-ttu-id="6f34e-188">A gomb összekapcsolása egy műveletet</span><span class="sxs-lookup"><span data-stu-id="6f34e-188">Link the button to an action</span></span>
<span data-ttu-id="6f34e-189">Most, hogy a gomb helyen, hogy egy művelet kapcsolódnia kell.</span><span class="sxs-lookup"><span data-stu-id="6f34e-189">Now that you have a button in place, you need to link it to an action.</span></span> <span data-ttu-id="6f34e-190">A műveleti fiók Google + fogadhatnak jogkivonatot kommunikálni az Azure AD B2C ebben az esetben szolgál.</span><span class="sxs-lookup"><span data-stu-id="6f34e-190">The action, in this case, is for Azure AD B2C to communicate with Google+ account to receive a token.</span></span>

1.  <span data-ttu-id="6f34e-191">Keresés a `<OrchestrationStep>` is tartalmazó `Order="2"` a a `<UserJourney>` csomópont.</span><span class="sxs-lookup"><span data-stu-id="6f34e-191">Find the `<OrchestrationStep>` that includes `Order="2"` in the `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="6f34e-192">Adja hozzá a következő XML-részletet a `<ClaimsExchanges>` csomópont:</span><span class="sxs-lookup"><span data-stu-id="6f34e-192">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

>[!NOTE]
>
> * <span data-ttu-id="6f34e-193">Győződjön meg arról a `Id` ugyanazzal az értékkel, amely rendelkezik `TargetClaimsExchangeId` az előző szakaszban leírt</span><span class="sxs-lookup"><span data-stu-id="6f34e-193">Ensure the `Id` has the same value as that of `TargetClaimsExchangeId` in the preceding section</span></span>
> * <span data-ttu-id="6f34e-194">Győződjön meg arról `TechnicalProfileReferenceId` beállítása a műszaki profil létrehozott korábbi (Google-OAUTH).</span><span class="sxs-lookup"><span data-stu-id="6f34e-194">Ensure `TechnicalProfileReferenceId` ID is set to the technical profile you created earlier (Google-OAUTH).</span></span>

## <a name="upload-the-policy-to-your-tenant"></a><span data-ttu-id="6f34e-195">A házirend a bérlő feltöltése</span><span class="sxs-lookup"><span data-stu-id="6f34e-195">Upload the policy to your tenant</span></span>
1.  <span data-ttu-id="6f34e-196">A a [Azure-portálon](https://portal.azure.com), átváltani a [az Azure AD B2C-bérlő kontextusában](active-directory-b2c-navigate-to-b2c-context.md), és nyissa meg a **az Azure AD B2C** panelen.</span><span class="sxs-lookup"><span data-stu-id="6f34e-196">In the [Azure portal](https://portal.azure.com), switch into the [context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and open the **Azure AD B2C** blade.</span></span>
2.  <span data-ttu-id="6f34e-197">Válassza ki **identitás élmény keretrendszer**.</span><span class="sxs-lookup"><span data-stu-id="6f34e-197">Select **Identity Experience Framework**.</span></span>
3.  <span data-ttu-id="6f34e-198">Nyissa meg a **házirend** panelen.</span><span class="sxs-lookup"><span data-stu-id="6f34e-198">Open the **All Policies** blade.</span></span>
4.  <span data-ttu-id="6f34e-199">Válassza ki **házirend feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="6f34e-199">Select **Upload Policy**.</span></span>
5.  <span data-ttu-id="6f34e-200">Ellenőrizze **felülírja a házirendet, ha létezik** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="6f34e-200">Check **Overwrite the policy if it exists** box.</span></span>
6.  <span data-ttu-id="6f34e-201">**Töltse fel** TrustFrameworkExtensions.xml, és győződjön meg arról, hogy az érvényesítése nem hiúsul meg az</span><span class="sxs-lookup"><span data-stu-id="6f34e-201">**Upload** TrustFrameworkExtensions.xml and ensure that it does not fail the validation</span></span>

## <a name="test-the-custom-policy-by-using-run-now"></a><span data-ttu-id="6f34e-202">Tesztelje az egyéni házirend segítségével futtatása most</span><span class="sxs-lookup"><span data-stu-id="6f34e-202">Test the custom policy by using Run Now</span></span>
1.  <span data-ttu-id="6f34e-203">Nyissa meg **az Azure AD B2C beállítások** , és navigáljon **identitás élmény keretrendszer**.</span><span class="sxs-lookup"><span data-stu-id="6f34e-203">Open **Azure AD B2C Settings** and go to **Identity Experience Framework**.</span></span>

    >[!NOTE]
    >
    >    <span data-ttu-id="6f34e-204">**Futtassa most** legalább egy alkalmazás a tenant preregistered lehet szükséges.</span><span class="sxs-lookup"><span data-stu-id="6f34e-204">**Run now** requires at least one application to be preregistered on the tenant.</span></span> 
    >    <span data-ttu-id="6f34e-205">Megtudhatja, hogyan kell regisztrálni az alkalmazások, tekintse meg az Azure AD B2C [Ismerkedés](active-directory-b2c-get-started.md) cikk vagy a [regisztrációja](active-directory-b2c-app-registration.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="6f34e-205">To learn how to register applications, see the Azure AD B2C [Get started](active-directory-b2c-get-started.md) article or the [Application registration](active-directory-b2c-app-registration.md) article.</span></span>


2.  <span data-ttu-id="6f34e-206">Nyissa meg **B2C_1A_signup_signin**, a függő entitásonkénti (RP) egyéni házirend feltöltött.</span><span class="sxs-lookup"><span data-stu-id="6f34e-206">Open **B2C_1A_signup_signin**, the relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="6f34e-207">Válassza ki **futtatása most**.</span><span class="sxs-lookup"><span data-stu-id="6f34e-207">Select **Run now**.</span></span>
3.  <span data-ttu-id="6f34e-208">Meg kell tudni jelentkezzen be Google + fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="6f34e-208">You should be able to sign in using Google+ account.</span></span>

## <a name="optional-register-the-google-account-claims-provider-to-profile-edit-user-journey"></a><span data-ttu-id="6f34e-209">[Választható] A Google + fiók jogcímszolgáltató profil szerkesztheti a felhasználói út regisztrálása</span><span class="sxs-lookup"><span data-stu-id="6f34e-209">[Optional] Register the Google+ account claims provider to Profile-Edit user journey</span></span>
<span data-ttu-id="6f34e-210">Érdemes lehet a Google + fiók identitásszolgáltató is hozzáadása a felhasználói `ProfileEdit` felhasználói út.</span><span class="sxs-lookup"><span data-stu-id="6f34e-210">You may want to add the Google+ account identity provider also to your user `ProfileEdit` user journey.</span></span> <span data-ttu-id="6f34e-211">Tegye elérhetővé, hogy ismételje meg az utolsó két lépést:</span><span class="sxs-lookup"><span data-stu-id="6f34e-211">To make it available, we repeat the last two steps:</span></span>

### <a name="display-the-button"></a><span data-ttu-id="6f34e-212">A gomb megjelenítése</span><span class="sxs-lookup"><span data-stu-id="6f34e-212">Display the button</span></span>
1.  <span data-ttu-id="6f34e-213">Nyissa meg a bővítményfájl házirend (például TrustFrameworkExtensions.xml).</span><span class="sxs-lookup"><span data-stu-id="6f34e-213">Open the extension file of your policy (for example, TrustFrameworkExtensions.xml).</span></span>
2.  <span data-ttu-id="6f34e-214">Keresés a `<UserJourney>` tartalmazó csomópont `Id="ProfileEdit"` a a felhasználók utazás másolt.</span><span class="sxs-lookup"><span data-stu-id="6f34e-214">Find the `<UserJourney>` node that includes `Id="ProfileEdit"` in the user journey that you copied.</span></span>
3.  <span data-ttu-id="6f34e-215">Keresse meg a `<OrchestrationStep>` tartalmazó csomópont`Order="1"`</span><span class="sxs-lookup"><span data-stu-id="6f34e-215">Locate the `<OrchestrationStep>` node that includes `Order="1"`</span></span>
4.  <span data-ttu-id="6f34e-216">Adja hozzá a következő XML-részletet a `<ClaimsProviderSelections>` csomópont:</span><span class="sxs-lookup"><span data-stu-id="6f34e-216">Add following XML snippet under `<ClaimsProviderSelections>` node:</span></span>

```xml
<ClaimsProviderSelection TargetClaimsExchangeId="GoogleExchange" />
```

### <a name="link-the-button-to-an-action"></a><span data-ttu-id="6f34e-217">A gomb összekapcsolása egy műveletet</span><span class="sxs-lookup"><span data-stu-id="6f34e-217">Link the button to an action</span></span>
1.  <span data-ttu-id="6f34e-218">Keresés a `<OrchestrationStep>` is tartalmazó `Order="2"` a a `<UserJourney>` csomópont.</span><span class="sxs-lookup"><span data-stu-id="6f34e-218">Find the `<OrchestrationStep>` that includes `Order="2"` in the `<UserJourney>` node.</span></span>
2.  <span data-ttu-id="6f34e-219">Adja hozzá a következő XML-részletet a `<ClaimsExchanges>` csomópont:</span><span class="sxs-lookup"><span data-stu-id="6f34e-219">Add following XML snippet under `<ClaimsExchanges>` node:</span></span>

```xml
<ClaimsExchange Id="GoogleExchange" TechnicalProfileReferenceId="Google-OAUTH" />
```

### <a name="test-the-custom-profile-edit-policy-by-using-run-now"></a><span data-ttu-id="6f34e-220">Tesztelje az egyéni házirend profil-módosítása segítségével futtatása most</span><span class="sxs-lookup"><span data-stu-id="6f34e-220">Test the custom Profile-Edit policy by using Run Now</span></span>

1.  <span data-ttu-id="6f34e-221">Nyissa meg **az Azure AD B2C beállítások** , és navigáljon **identitás élmény keretrendszer**.</span><span class="sxs-lookup"><span data-stu-id="6f34e-221">Open **Azure AD B2C Settings** and go to **Identity Experience Framework**.</span></span>
2.  <span data-ttu-id="6f34e-222">Nyissa meg **B2C_1A_ProfileEdit**, a függő entitásonkénti (RP) egyéni házirend feltöltött.</span><span class="sxs-lookup"><span data-stu-id="6f34e-222">Open **B2C_1A_ProfileEdit**, the relying party (RP) custom policy that you uploaded.</span></span> <span data-ttu-id="6f34e-223">Válassza ki **futtatása most**.</span><span class="sxs-lookup"><span data-stu-id="6f34e-223">Select **Run now**.</span></span>
3.  <span data-ttu-id="6f34e-224">Meg kell tudni jelentkezzen be Google + fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="6f34e-224">You should be able to sign in using Google+ account.</span></span>

## <a name="download-the-complete-policy-files"></a><span data-ttu-id="6f34e-225">A teljes házirend-fájlok letöltésére</span><span class="sxs-lookup"><span data-stu-id="6f34e-225">Download the complete policy files</span></span>
<span data-ttu-id="6f34e-226">Választható lehetőség: Javasoljuk hoz létre a minta-fájlok használata helyett a saját egyéni házirend fájlok az első lépések egyéni házirendekkel befejezése után végezze el a forgatókönyvet.</span><span class="sxs-lookup"><span data-stu-id="6f34e-226">Optional: We recommend you build your scenario using your own Custom policy files after completing the Getting Started with Custom Policies walk through instead of using these sample files.</span></span>  [<span data-ttu-id="6f34e-227">Házirend mintafájlok referencia</span><span class="sxs-lookup"><span data-stu-id="6f34e-227">Sample policy files for reference</span></span>](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-goog-app)
