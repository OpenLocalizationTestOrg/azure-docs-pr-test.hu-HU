---
title: "Az Azure Active Directory B2C: Az Azure AD-szolgáltató felvétele egyéni házirendek használatával |} Microsoft Docs"
description: "További tudnivalók az Azure Active Directory B2C egyéni házirendek"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 31f0dfe5-1ad0-4a25-a53b-8acc71bcea72
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/04/2017
ms.author: parakhj
ms.openlocfilehash: 6c073d70debfdc3560405955d65fa9ccaa7d8b1f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-b2c-sign-in-by-using-azure-ad-accounts"></a><span data-ttu-id="3155d-103">Az Azure Active Directory B2C: Azure AD-fiókok használatával írja alá</span><span class="sxs-lookup"><span data-stu-id="3155d-103">Azure Active Directory B2C: Sign in by using Azure AD accounts</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="3155d-104">Ez a cikk bemutatja, hogyan bejelentkezhet egy adott Azure Active Directory (Azure AD) szervezet keresztül a felhasználók engedélyezése [egyéni házirendek](active-directory-b2c-overview-custom.md).</span><span class="sxs-lookup"><span data-stu-id="3155d-104">This article shows you how to enable sign-in for users from a specific Azure Active Directory (Azure AD) organization through the use of [custom policies](active-directory-b2c-overview-custom.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3155d-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3155d-105">Prerequisites</span></span>

<span data-ttu-id="3155d-106">Hajtsa végre a a [Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="3155d-106">Complete the steps in the [Getting started with custom policies](active-directory-b2c-get-started-custom.md) article.</span></span>

<span data-ttu-id="3155d-107">Ezek a lépések az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="3155d-107">These steps include:</span></span>

1. <span data-ttu-id="3155d-108">Egy Azure Active Directory B2C létrehozása (Azure AD B2C) bérlő.</span><span class="sxs-lookup"><span data-stu-id="3155d-108">Creating an Azure Active Directory B2C (Azure AD B2C) tenant.</span></span>
2. <span data-ttu-id="3155d-109">Az Azure AD B2C-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3155d-109">Creating an Azure AD B2C application.</span></span>
3. <span data-ttu-id="3155d-110">Két házirendmotor alkalmazás regisztrálásakor.</span><span class="sxs-lookup"><span data-stu-id="3155d-110">Registering two policy-engine applications.</span></span>
4. <span data-ttu-id="3155d-111">Kulcsok beállítása.</span><span class="sxs-lookup"><span data-stu-id="3155d-111">Setting up keys.</span></span>
5. <span data-ttu-id="3155d-112">Az alapszintű csomag beállítása.</span><span class="sxs-lookup"><span data-stu-id="3155d-112">Setting up the starter pack.</span></span>

## <a name="create-an-azure-ad-app"></a><span data-ttu-id="3155d-113">Az Azure AD-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="3155d-113">Create an Azure AD app</span></span>

<span data-ttu-id="3155d-114">Bejelentkezés a felhasználók egy meghatározott engedélyezése az Azure AD a szervezeten belül, regisztrálnia kell az alkalmazás belül a szervezeti Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="3155d-114">To enable sign-in for users from a specific Azure AD organization, you need to register an application within the organizational Azure AD tenant.</span></span>

>[!NOTE]
> <span data-ttu-id="3155d-115">A szervezeti a használjuk a "contoso.com" Azure AD-bérlő és a "fabrikamb2c.onmicrosoft.com" az Azure AD B2C bérlő az alábbi utasításokat.</span><span class="sxs-lookup"><span data-stu-id="3155d-115">We use "contoso.com" for the organizational Azure AD tenant and "fabrikamb2c.onmicrosoft.com" as the Azure AD B2C tenant in the following instructions.</span></span>

1. <span data-ttu-id="3155d-116">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3155d-116">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
1. <span data-ttu-id="3155d-117">A felső sávon válassza ki a fiókját.</span><span class="sxs-lookup"><span data-stu-id="3155d-117">On the top bar, select your account.</span></span> <span data-ttu-id="3155d-118">Az a **Directory** menüben válassza ki a szervezeti Azure AD-bérlő hol szeretne regisztrálni az alkalmazás (contoso.com).</span><span class="sxs-lookup"><span data-stu-id="3155d-118">From the **Directory** list, choose the organizational Azure AD tenant where you want to register your application (contoso.com).</span></span>
1. <span data-ttu-id="3155d-119">Válassza ki **további szolgáltatások** a bal oldali ablaktáblán, és keresse meg az "App regisztráció."</span><span class="sxs-lookup"><span data-stu-id="3155d-119">Select **More services** in the left pane, and search for "App registrations."</span></span>
1. <span data-ttu-id="3155d-120">Válassza ki **új alkalmazás regisztrációja**.</span><span class="sxs-lookup"><span data-stu-id="3155d-120">Select **New application registration**.</span></span>
1. <span data-ttu-id="3155d-121">Adjon meg egy nevet az alkalmazáshoz (például `Azure AD B2C App`).</span><span class="sxs-lookup"><span data-stu-id="3155d-121">Enter a name for your application (for example, `Azure AD B2C App`).</span></span>
1. <span data-ttu-id="3155d-122">Válassza ki **Web app / API** az alkalmazás típusra.</span><span class="sxs-lookup"><span data-stu-id="3155d-122">Select **Web app / API** for the application type.</span></span>
1. <span data-ttu-id="3155d-123">A **bejelentkezési URL-cím**, adja meg a következő URL-cím, ahol `yourtenant` cseréli le az Azure AD B2C bérlő neve (`fabrikamb2c.onmicrosoft.com`):</span><span class="sxs-lookup"><span data-stu-id="3155d-123">For **Sign-on URL**, enter the following URL, where `yourtenant` is replaced by the name of your Azure AD B2C tenant (`fabrikamb2c.onmicrosoft.com`):</span></span>

    ```
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

1. <span data-ttu-id="3155d-124">Mentse az azonosítót.</span><span class="sxs-lookup"><span data-stu-id="3155d-124">Save the application ID.</span></span>
1. <span data-ttu-id="3155d-125">Válassza ki az újonnan létrehozott alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3155d-125">Select the newly created application.</span></span>
1. <span data-ttu-id="3155d-126">Az a **beállítások** panelen válassza **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="3155d-126">Under the **Settings** blade, select **Keys**.</span></span>
1. <span data-ttu-id="3155d-127">Hozzon létre egy új kulcsot, és mentse.</span><span class="sxs-lookup"><span data-stu-id="3155d-127">Create a new key, and save it.</span></span> <span data-ttu-id="3155d-128">A következő szakaszban a lépések még szüksége lesz rájuk.</span><span class="sxs-lookup"><span data-stu-id="3155d-128">You will use it in the steps in the next section.</span></span>

## <a name="add-the-azure-ad-key-to-azure-ad-b2c"></a><span data-ttu-id="3155d-129">Az Azure AD-kulcs hozzáadása az Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="3155d-129">Add the Azure AD key to Azure AD B2C</span></span>

<span data-ttu-id="3155d-130">A contoso.com alkalmazás kulcs tárolása az Azure AD B2C-bérlő kell.</span><span class="sxs-lookup"><span data-stu-id="3155d-130">You need to store the contoso.com application key in your Azure AD B2C tenant.</span></span> <span data-ttu-id="3155d-131">Ehhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="3155d-131">To do this:</span></span>
1. <span data-ttu-id="3155d-132">Nyissa meg az Azure AD B2C-bérlő, és válassza ki **B2C beállítások** > **identitás élmény keretrendszer** > **házirend kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="3155d-132">Go to your Azure AD B2C tenant, and select **B2C Settings** > **Identity Experience Framework** > **Policy Keys**.</span></span>
1. <span data-ttu-id="3155d-133">Válassza ki **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="3155d-133">Select **+Add**.</span></span>
1. <span data-ttu-id="3155d-134">Válassza ki vagy adja meg ezeket a beállításokat:</span><span class="sxs-lookup"><span data-stu-id="3155d-134">Select or enter these options:</span></span>
   * <span data-ttu-id="3155d-135">Válassza ki **manuális**.</span><span class="sxs-lookup"><span data-stu-id="3155d-135">Select **Manual**.</span></span>
   * <span data-ttu-id="3155d-136">A **neve**, válasszon egy nevet, amely megfelel az Azure AD-bérlő nevét (például `ContosoAppSecret`).</span><span class="sxs-lookup"><span data-stu-id="3155d-136">For **Name**, choose a name that matches your Azure AD tenant name (for example, `ContosoAppSecret`).</span></span>  <span data-ttu-id="3155d-137">Az előtag `B2C_1A_` automatikusan hozzáadódik a kulcs neve.</span><span class="sxs-lookup"><span data-stu-id="3155d-137">The prefix `B2C_1A_` is added automatically to the name of your key.</span></span>
   * <span data-ttu-id="3155d-138">Illessze be az alkalmazás kulcs a **titkos** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="3155d-138">Paste your application key in the **Secret** box.</span></span>
   * <span data-ttu-id="3155d-139">Válassza ki **aláírás**.</span><span class="sxs-lookup"><span data-stu-id="3155d-139">Select **Signature**.</span></span>
1. <span data-ttu-id="3155d-140">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3155d-140">Select **Create**.</span></span>
1. <span data-ttu-id="3155d-141">Győződjön meg arról, hogy létrehozta a kulcs `B2C_1A_ContosoAppSecret`.</span><span class="sxs-lookup"><span data-stu-id="3155d-141">Confirm that you've created the key `B2C_1A_ContosoAppSecret`.</span></span>


## <a name="add-a-claims-provider-in-your-base-policy"></a><span data-ttu-id="3155d-142">A jogcím-szolgáltató hozzáadása a kiinduló házirend</span><span class="sxs-lookup"><span data-stu-id="3155d-142">Add a claims provider in your base policy</span></span>

<span data-ttu-id="3155d-143">Ha azt szeretné, hogy a felhasználók jelentkezhetnek be az Azure AD használatával, kell meghatározni az Azure AD, a jogcím-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="3155d-143">If you want users to sign in by using Azure AD, you need to define Azure AD as a claims provider.</span></span> <span data-ttu-id="3155d-144">Ez azt jelenti meg kell adnia egy végpontot, amely az Azure AD B2C fognak kommunikálni.</span><span class="sxs-lookup"><span data-stu-id="3155d-144">In other words, you need to specify an endpoint that Azure AD B2C will communicate with.</span></span> <span data-ttu-id="3155d-145">A végpont jogcímeket, az Azure AD B2C által használt győződjön meg arról, hogy egy adott felhasználó engedélyezést biztosít.</span><span class="sxs-lookup"><span data-stu-id="3155d-145">The endpoint will provide a set of claims that are used by Azure AD B2C to verify that a specific user has authenticated.</span></span> 

<span data-ttu-id="3155d-146">Meghatározhatja az Azure AD egy jogcímszolgáltatótól, az Azure AD-be való hozzáadásával a `<ClaimsProvider>` csomópont házirend bővítmény fájlban:</span><span class="sxs-lookup"><span data-stu-id="3155d-146">You can define Azure AD as a claims provider by adding Azure AD to the `<ClaimsProvider>` node in the extension file of your policy:</span></span>

1. <span data-ttu-id="3155d-147">A bővítményfájl (TrustFrameworkExtensions.xml) megnyitása a munkakönyvtárat.</span><span class="sxs-lookup"><span data-stu-id="3155d-147">Open the extension file (TrustFrameworkExtensions.xml) from your working directory.</span></span>
1. <span data-ttu-id="3155d-148">Keresés a `<ClaimsProviders>` szakasz.</span><span class="sxs-lookup"><span data-stu-id="3155d-148">Find the `<ClaimsProviders>` section.</span></span> <span data-ttu-id="3155d-149">Ha még nem létezik, adja hozzá a legfelső szintű csomópontja alatt.</span><span class="sxs-lookup"><span data-stu-id="3155d-149">If it does not exist, add it under the root node.</span></span>
1. <span data-ttu-id="3155d-150">Adjon hozzá egy új `<ClaimsProvider>` csomópont az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="3155d-150">Add a new `<ClaimsProvider>` node as follows:</span></span>

    ```XML
    <ClaimsProvider>
        <Domain>Contoso</Domain>
        <DisplayName>Login using Contoso</DisplayName>
        <TechnicalProfiles>
            <TechnicalProfile Id="ContosoProfile">
                <DisplayName>Contoso Employee</DisplayName>
                <Description>Login with your Contoso account</Description>
                <Protocol Name="OpenIdConnect"/>
                <OutputTokenFormat>JWT</OutputTokenFormat>
                <Metadata>
                    <Item Key="METADATA">https://login.windows.net/contoso.com/.well-known/openid-configuration</Item>
                    <Item Key="ProviderName">https://sts.windows.net/00000000-0000-0000-0000-000000000000/</Item>
                    <Item Key="client_id">00000000-0000-0000-0000-000000000000</Item>
                    <Item Key="IdTokenAudience">00000000-0000-0000-0000-000000000000</Item>
                    <Item Key="response_types">id_token</Item>
                    <Item Key="UsePolicyInRedirectUri">false</Item>
                </Metadata>
                <CryptographicKeys>
                    <Key Id="client_secret" StorageReferenceId="B2C_1A_ContosoAppSecret"/>
                </CryptographicKeys>
                <OutputClaims>
                    <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="oid"/>
                    <OutputClaim ClaimTypeReferenceId="tenantId" PartnerClaimType="tid"/>
                    <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name" />
                    <OutputClaim ClaimTypeReferenceId="surName" PartnerClaimType="family_name" />
                    <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name" />
                    <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="contosoAuthentication" />
                    <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="AzureADContoso" />
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

1. <span data-ttu-id="3155d-151">Az a `<ClaimsProvider>` csomópont, frissítse az értéket a `<Domain>` segítségével különböztetheti meg egymástól az egyéb identitás-szolgáltatóktól származó egyedi értéket.</span><span class="sxs-lookup"><span data-stu-id="3155d-151">Under the `<ClaimsProvider>` node, update the value for `<Domain>` to a unique value that can be used to distinguish it from other identity providers.</span></span>
1. <span data-ttu-id="3155d-152">A a `<ClaimsProvider>` csomópont, frissítse az értéket a `<DisplayName>` egy rövid nevet a jogcím-szolgáltató számára.</span><span class="sxs-lookup"><span data-stu-id="3155d-152">Under the `<ClaimsProvider>` node, update the value for `<DisplayName>` to a friendly name for the claims provider.</span></span> <span data-ttu-id="3155d-153">Ez az érték nincs használatban.</span><span class="sxs-lookup"><span data-stu-id="3155d-153">This value is not currently used.</span></span>

### <a name="update-the-technical-profile"></a><span data-ttu-id="3155d-154">A műszaki profil frissítése</span><span class="sxs-lookup"><span data-stu-id="3155d-154">Update the technical profile</span></span>

<span data-ttu-id="3155d-155">Az Azure AD-végpont egy token beszerzéséhez ját definiálja a protokollokat, amelyeket az Azure AD B2C segítségével kommunikálni az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3155d-155">To get a token from the Azure AD endpoint, you need to define the protocols that Azure AD B2C should use to communicate with Azure AD.</span></span> <span data-ttu-id="3155d-156">Ez a `<TechnicalProfile>` eleme `<ClaimsProvider>`.</span><span class="sxs-lookup"><span data-stu-id="3155d-156">This is done inside the `<TechnicalProfile>` element of  `<ClaimsProvider>`.</span></span>
1. <span data-ttu-id="3155d-157">Frissítés Azonosítóját a `<TechnicalProfile>` csomópont.</span><span class="sxs-lookup"><span data-stu-id="3155d-157">Update the ID of the `<TechnicalProfile>` node.</span></span> <span data-ttu-id="3155d-158">Az azonosító olyan utal a műszaki profilhoz, a házirend egyéb részeitől.</span><span class="sxs-lookup"><span data-stu-id="3155d-158">This ID is used to refer to this technical profile from other parts of the policy.</span></span>
1. <span data-ttu-id="3155d-159">Frissítse az értéket a `<DisplayName>`.</span><span class="sxs-lookup"><span data-stu-id="3155d-159">Update the value for `<DisplayName>`.</span></span> <span data-ttu-id="3155d-160">Ez az érték jelenik meg a Bejelentkezés gombra, a bejelentkezési képernyőn.</span><span class="sxs-lookup"><span data-stu-id="3155d-160">This value will be displayed on the sign-in button on your sign-in screen.</span></span>
1. <span data-ttu-id="3155d-161">Frissítse az értéket a `<Description>`.</span><span class="sxs-lookup"><span data-stu-id="3155d-161">Update the value for `<Description>`.</span></span>
1. <span data-ttu-id="3155d-162">Az Azure AD az OpenID Connect protokollt használja, ezért ügyeljen arra, hogy az érték `<Protocol>` van `"OpenIdConnect"`.</span><span class="sxs-lookup"><span data-stu-id="3155d-162">Azure AD uses the OpenID Connect protocol, so ensure that the value for `<Protocol>` is `"OpenIdConnect"`.</span></span>

<span data-ttu-id="3155d-163">Frissíteni kell a `<Metadata>` az XML-fájl a szakaszban említett korábban megfelelően a beállításokat az adott Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="3155d-163">You need to update the `<Metadata>` section in the XML file referred to earlier to reflect the configuration settings for your specific Azure AD tenant.</span></span> <span data-ttu-id="3155d-164">Az XML-fájlban, a metaadatok értékeinek frissítéséhez az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="3155d-164">In the XML file, update the metadata values as follows:</span></span>

1. <span data-ttu-id="3155d-165">Állítsa be `<Item Key="METADATA">` való `https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, ahol `yourAzureADtenant` a Azure AD-bérlő neve (contoso.com).</span><span class="sxs-lookup"><span data-stu-id="3155d-165">Set `<Item Key="METADATA">` to `https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, where `yourAzureADtenant` is your Azure AD tenant name (contoso.com).</span></span>
1. <span data-ttu-id="3155d-166">Nyissa meg a böngészőt, és navigáljon a `METADATA` frissített URL-címet.</span><span class="sxs-lookup"><span data-stu-id="3155d-166">Open your browser and go to the `METADATA` URL that you just updated.</span></span>
1. <span data-ttu-id="3155d-167">A böngészőben keresse meg a "kiállító" objektum, és másolja az értékét.</span><span class="sxs-lookup"><span data-stu-id="3155d-167">In the browser, look for the 'issuer' object and copy its value.</span></span> <span data-ttu-id="3155d-168">A következő formában: `https://sts.windows.net/{tenantId}/`.</span><span class="sxs-lookup"><span data-stu-id="3155d-168">It should look like the following: `https://sts.windows.net/{tenantId}/`.</span></span>
1. <span data-ttu-id="3155d-169">Illessze be a következő `<Item Key="ProviderName">` az XML-fájlban.</span><span class="sxs-lookup"><span data-stu-id="3155d-169">Paste the value for `<Item Key="ProviderName">` in the XML file.</span></span>
1. <span data-ttu-id="3155d-170">Állítsa be `<Item Key="client_id">` számára az alkalmazás regisztrálása a alkalmazás azonosítója.</span><span class="sxs-lookup"><span data-stu-id="3155d-170">Set `<Item Key="client_id">` to the application ID from the app registration.</span></span>
1. <span data-ttu-id="3155d-171">Állítsa be `<Item Key="IdTokenAudience">` számára az alkalmazás regisztrálása a alkalmazás azonosítója.</span><span class="sxs-lookup"><span data-stu-id="3155d-171">Set `<Item Key="IdTokenAudience">` to the application ID from the app registration.</span></span>
1. <span data-ttu-id="3155d-172">Győződjön meg arról, hogy `<Item Key="response_types">` értéke `id_token`.</span><span class="sxs-lookup"><span data-stu-id="3155d-172">Ensure that `<Item Key="response_types">` is set to `id_token`.</span></span>
1. <span data-ttu-id="3155d-173">Győződjön meg arról, hogy `<Item Key="UsePolicyInRedirectUri">` értéke `false`.</span><span class="sxs-lookup"><span data-stu-id="3155d-173">Ensure that `<Item Key="UsePolicyInRedirectUri">` is set to `false`.</span></span>

<span data-ttu-id="3155d-174">Is szükség van az Azure AD titkos kulcs regisztrált az Azure AD B2C-bérlő az Azure ad a hivatkozás `<ClaimsProvider>` információkat:</span><span class="sxs-lookup"><span data-stu-id="3155d-174">You also need to link the Azure AD secret that you registered in your Azure AD B2C tenant to the Azure AD `<ClaimsProvider>` information:</span></span>

* <span data-ttu-id="3155d-175">Az a `<CryptographicKeys>` szakasz az előző XML-fájl, frissítse az értéket a `StorageReferenceId` a titkos kulcsot, amelyet megadott, a referencia-azonosítóra (például `ContosoAppSecret`).</span><span class="sxs-lookup"><span data-stu-id="3155d-175">In the `<CryptographicKeys>` section in the preceding XML file, update the value for `StorageReferenceId` to the reference ID of the secret that you defined (for example, `ContosoAppSecret`).</span></span>

### <a name="upload-the-extension-file-for-verification"></a><span data-ttu-id="3155d-176">Az ellenőrzéshez a bővítmény-fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="3155d-176">Upload the extension file for verification</span></span>

<span data-ttu-id="3155d-177">Mostanra így az Azure AD B2C tudja kommunikálni az Azure AD-címtár konfigurálta a házirendet.</span><span class="sxs-lookup"><span data-stu-id="3155d-177">By now, you have configured your policy so that Azure AD B2C knows how to communicate with your Azure AD directory.</span></span> <span data-ttu-id="3155d-178">Próbálja feltölti a bővítmény a házirend csak győződjön meg arról, hogy nincs probléma merül fel eddig.</span><span class="sxs-lookup"><span data-stu-id="3155d-178">Try uploading the extension file of your policy just to confirm that it doesn't have any issues so far.</span></span> <span data-ttu-id="3155d-179">Ehhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="3155d-179">To do so:</span></span>

1. <span data-ttu-id="3155d-180">Nyissa meg a **házirend** panel az Azure AD B2C-bérlőben.</span><span class="sxs-lookup"><span data-stu-id="3155d-180">Open the **All Policies** blade in your Azure AD B2C tenant.</span></span>
1. <span data-ttu-id="3155d-181">Ellenőrizze a **felülírja a házirendet, ha létezik** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="3155d-181">Check the **Overwrite the policy if it exists** box.</span></span>
1. <span data-ttu-id="3155d-182">A bővítményfájl (TrustFrameworkExtensions.xml) feltöltése, és győződjön meg arról, hogy azt nem az érvényesítés.</span><span class="sxs-lookup"><span data-stu-id="3155d-182">Upload the extension file (TrustFrameworkExtensions.xml), and ensure that it does not fail the validation.</span></span>

## <a name="register-the-azure-ad-claims-provider-to-a-user-journey"></a><span data-ttu-id="3155d-183">Regisztráció az Azure AD jogcímszolgáltató felhasználói út</span><span class="sxs-lookup"><span data-stu-id="3155d-183">Register the Azure AD claims provider to a user journey</span></span>

<span data-ttu-id="3155d-184">Most kell az Azure AD identity provider hozzáadása a felhasználói utak egyike.</span><span class="sxs-lookup"><span data-stu-id="3155d-184">You now need to add the Azure AD identity provider to one of your user journeys.</span></span> <span data-ttu-id="3155d-185">Ezen a ponton az identitásszolgáltató be van állítva, de a sign-Close-Up/sign-in képernyők egyik nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="3155d-185">At this point, the identity provider has been set up, but it’s not available in any of the sign-up/sign-in screens.</span></span> <span data-ttu-id="3155d-186">Tegye elérhetővé, a rendszer létrehoz egy meglévő sablon felhasználói út duplikált, és módosíthatja azt, hogy az Azure AD identity provider is rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="3155d-186">To make it available, we will create a duplicate of an existing template user journey, and then modify it so that it also has the Azure AD identity provider:</span></span>

1. <span data-ttu-id="3155d-187">Nyissa meg a házirend (például TrustFrameworkBase.xml) Alap fájlt.</span><span class="sxs-lookup"><span data-stu-id="3155d-187">Open the base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
1. <span data-ttu-id="3155d-188">Keresés a `<UserJourneys>` elemet, és másolja a teljes `<UserJourney>` tartalmazó csomópont `Id="SignUpOrSignIn"`.</span><span class="sxs-lookup"><span data-stu-id="3155d-188">Find the `<UserJourneys>` element and copy the entire `<UserJourney>` node that includes `Id="SignUpOrSignIn"`.</span></span>
1. <span data-ttu-id="3155d-189">Nyissa meg a bővítmény (például TrustFrameworkExtensions.xml) fájlt, és keresse a `<UserJourneys>` elemet.</span><span class="sxs-lookup"><span data-stu-id="3155d-189">Open the extension file (for example, TrustFrameworkExtensions.xml) and find the `<UserJourneys>` element.</span></span> <span data-ttu-id="3155d-190">Ha az elem nem létezik, vegyen fel egyet.</span><span class="sxs-lookup"><span data-stu-id="3155d-190">If the element doesn't exist, add one.</span></span>
1. <span data-ttu-id="3155d-191">Illessze be a teljes `<UserJourney>` csomópont gyermekeként másolt a `<UserJourneys>` elemet.</span><span class="sxs-lookup"><span data-stu-id="3155d-191">Paste the entire `<UserJourney>` node that you copied as a child of the `<UserJourneys>` element.</span></span>
1. <span data-ttu-id="3155d-192">Nevezze át az új felhasználói út azonosítója (például a módon nevezze át `SignUpOrSignUsingContoso`).</span><span class="sxs-lookup"><span data-stu-id="3155d-192">Rename the ID of the new user journey (for example, rename as `SignUpOrSignUsingContoso`).</span></span>

### <a name="display-the-button"></a><span data-ttu-id="3155d-193">A "gomb"</span><span class="sxs-lookup"><span data-stu-id="3155d-193">Display the "button"</span></span>

<span data-ttu-id="3155d-194">A `<ClaimsProviderSelection>` a elem egy identitás szolgáltató gombjára egy sign-Close-Up/bejelentkezési képernyő hasonló.</span><span class="sxs-lookup"><span data-stu-id="3155d-194">The `<ClaimsProviderSelection>` element is analogous to an identity provider button on a sign-up/sign-in screen.</span></span> <span data-ttu-id="3155d-195">Ha ad hozzá egy `<ClaimsProviderSelection>` elem az Azure AD, egy új gombon, amikor egy felhasználó fájljai az oldalon.</span><span class="sxs-lookup"><span data-stu-id="3155d-195">If you add a `<ClaimsProviderSelection>` element for Azure AD, a new button shows up when a user lands on the page.</span></span> <span data-ttu-id="3155d-196">Ez az elem hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="3155d-196">To add this element:</span></span>

1. <span data-ttu-id="3155d-197">Keresse a `<OrchestrationStep>` tartalmazó csomópont `Order="1"` az újonnan létrehozott felhasználó út található.</span><span class="sxs-lookup"><span data-stu-id="3155d-197">Find the `<OrchestrationStep>` node that includes `Order="1"` in the user journey that you just created.</span></span>
1. <span data-ttu-id="3155d-198">Adja hozzá a következőket:</span><span class="sxs-lookup"><span data-stu-id="3155d-198">Add the following:</span></span>

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

1. <span data-ttu-id="3155d-199">Állítsa be `TargetClaimsExchangeId` egy megfelelő értékre.</span><span class="sxs-lookup"><span data-stu-id="3155d-199">Set `TargetClaimsExchangeId` to an appropriate value.</span></span> <span data-ttu-id="3155d-200">Azt javasoljuk, mint mások azonos egyezmény követően:  *\[ClaimProviderName\]Exchange*.</span><span class="sxs-lookup"><span data-stu-id="3155d-200">We recommend following the same convention as others: *\[ClaimProviderName\]Exchange*.</span></span>

### <a name="link-the-button-to-an-action"></a><span data-ttu-id="3155d-201">A gomb összekapcsolása egy műveletet</span><span class="sxs-lookup"><span data-stu-id="3155d-201">Link the button to an action</span></span>

<span data-ttu-id="3155d-202">Most, hogy a gomb helyen, hogy egy művelet kapcsolódnia kell.</span><span class="sxs-lookup"><span data-stu-id="3155d-202">Now that you have a button in place, you need to link it to an action.</span></span> <span data-ttu-id="3155d-203">A művelet, ebben az esetben, akkor az Azure AD B2C-vel való kommunikációhoz fogadhatnak jogkivonatot az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3155d-203">The action, in this case, is for Azure AD B2C to communicate with Azure AD to receive a token.</span></span> <span data-ttu-id="3155d-204">A gomb művelet mutató hivatkozás létrehozása az Azure AD jogcímeket szolgáltató létrehozhatja, ha a műszaki profil:</span><span class="sxs-lookup"><span data-stu-id="3155d-204">Link the button to an action by linking the technical profile for your Azure AD claims provider:</span></span>

1. <span data-ttu-id="3155d-205">Keresés a `<OrchestrationStep>` is tartalmazó `Order="2"` a a `<UserJourney>` csomópont.</span><span class="sxs-lookup"><span data-stu-id="3155d-205">Find the `<OrchestrationStep>` that includes `Order="2"` in the `<UserJourney>` node.</span></span>
1. <span data-ttu-id="3155d-206">Adja hozzá a következőket:</span><span class="sxs-lookup"><span data-stu-id="3155d-206">Add the following:</span></span>

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

1. <span data-ttu-id="3155d-207">Frissítés `Id` ugyanarra az értékre, amely `TargetClaimsExchangeId` az előző szakaszban leírt.</span><span class="sxs-lookup"><span data-stu-id="3155d-207">Update `Id` to the same value as that of `TargetClaimsExchangeId` in the preceding section.</span></span>
1. <span data-ttu-id="3155d-208">Frissítés `TechnicalProfileReferenceId` a műszaki profil azonosítója létrehozott korábbi (ContosoProfile).</span><span class="sxs-lookup"><span data-stu-id="3155d-208">Update `TechnicalProfileReferenceId` to the ID of the technical profile you created earlier (ContosoProfile).</span></span>

### <a name="upload-the-updated-extension-file"></a><span data-ttu-id="3155d-209">A frissített kiterjesztésű fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="3155d-209">Upload the updated extension file</span></span>

<span data-ttu-id="3155d-210">Befejezte a bővítmény fájl módosítása.</span><span class="sxs-lookup"><span data-stu-id="3155d-210">You are done modifying the extension file.</span></span> <span data-ttu-id="3155d-211">Mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="3155d-211">Save it.</span></span> <span data-ttu-id="3155d-212">Majd feltölteni a fájlt, és győződjön meg arról, hogy az összes érvényesítések sikeres.</span><span class="sxs-lookup"><span data-stu-id="3155d-212">Then upload the file, and ensure that all validations succeed.</span></span>

### <a name="update-the-rp-file"></a><span data-ttu-id="3155d-213">A függő Entitás fájl frissítése</span><span class="sxs-lookup"><span data-stu-id="3155d-213">Update the RP file</span></span>

<span data-ttu-id="3155d-214">Most frissíteni kell a függő entitásonkénti (RP) fájl, amely indít el az újonnan létrehozott felhasználó út:</span><span class="sxs-lookup"><span data-stu-id="3155d-214">You now need to update the relying party (RP) file that will initiate the user journey that you just created:</span></span>

1. <span data-ttu-id="3155d-215">Készítsen másolatot a SignUpOrSignIn.xml a munkakönyvtárat, és adjon neki (például nevezze át SignUpOrSignInWithAAD.xml).</span><span class="sxs-lookup"><span data-stu-id="3155d-215">Make a copy of SignUpOrSignIn.xml in your working directory, and rename it (for example, rename it to SignUpOrSignInWithAAD.xml).</span></span>
1. <span data-ttu-id="3155d-216">Nyissa meg az új fájlt, és frissítse a `PolicyId` az attribútum `<TrustFrameworkPolicy>` egyedi értékkel (például SignUpOrSignInWithAAD).</span><span class="sxs-lookup"><span data-stu-id="3155d-216">Open the new file and update the `PolicyId` attribute for `<TrustFrameworkPolicy>` with a unique value (for example, SignUpOrSignInWithAAD).</span></span> <br> <span data-ttu-id="3155d-217">Ez lesz a házirend nevét (például B2C\_1A\_SignUpOrSignInWithAAD).</span><span class="sxs-lookup"><span data-stu-id="3155d-217">This will be the name of your policy (for example, B2C\_1A\_SignUpOrSignInWithAAD).</span></span>
1. <span data-ttu-id="3155d-218">Módosítsa a `ReferenceId` attribútumnak `<DefaultUserJourney>` megfelelően az új felhasználói út (SignUpOrSignUsingContoso) létrehozott azonosítója.</span><span class="sxs-lookup"><span data-stu-id="3155d-218">Modify the `ReferenceId` attribute in `<DefaultUserJourney>` to match the ID of the new user journey that you created (SignUpOrSignUsingContoso).</span></span>
1. <span data-ttu-id="3155d-219">A módosítások mentéséhez, és töltse fel a fájlt.</span><span class="sxs-lookup"><span data-stu-id="3155d-219">Save your changes, and upload the file.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="3155d-220">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="3155d-220">Troubleshooting</span></span>

<span data-ttu-id="3155d-221">Tesztelje a egyéni házirendet, majd kattintson a panel megnyitása feltöltött **futtatása most**.</span><span class="sxs-lookup"><span data-stu-id="3155d-221">Test the custom policy that you just uploaded by opening its blade and clicking **Run now**.</span></span> <span data-ttu-id="3155d-222">Problémák diagnosztizálásához, olvassa el [hibaelhárítási](active-directory-b2c-troubleshoot-custom.md).</span><span class="sxs-lookup"><span data-stu-id="3155d-222">To diagnose problems, read about [troubleshooting](active-directory-b2c-troubleshoot-custom.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3155d-223">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3155d-223">Next steps</span></span>

<span data-ttu-id="3155d-224">Visszajelzés küldése a [ AADB2CPreview@microsoft.com ](mailto:AADB2CPreview@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="3155d-224">Provide feedback to [AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).</span></span>
