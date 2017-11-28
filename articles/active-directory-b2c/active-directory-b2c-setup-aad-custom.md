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
ms.openlocfilehash: 9b0c32086cebc171d91da2e7bfb48136723ccd4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-sign-in-by-using-azure-ad-accounts"></a><span data-ttu-id="68d69-103">Az Azure Active Directory B2C: Azure AD-fiókok használatával írja alá</span><span class="sxs-lookup"><span data-stu-id="68d69-103">Azure Active Directory B2C: Sign in by using Azure AD accounts</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="68d69-104">Ez a cikk bemutatja, hogyan tooenable bejelentkezhet egy adott Azure Active Directory (Azure AD) szervezeti hello segítségével a felhasználók [egyéni házirendek](active-directory-b2c-overview-custom.md).</span><span class="sxs-lookup"><span data-stu-id="68d69-104">This article shows you how tooenable sign-in for users from a specific Azure Active Directory (Azure AD) organization through hello use of [custom policies](active-directory-b2c-overview-custom.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="68d69-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="68d69-105">Prerequisites</span></span>

<span data-ttu-id="68d69-106">Teljes hello szükséges lépések hello [Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="68d69-106">Complete hello steps in hello [Getting started with custom policies](active-directory-b2c-get-started-custom.md) article.</span></span>

<span data-ttu-id="68d69-107">Ezek a lépések az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="68d69-107">These steps include:</span></span>

1. <span data-ttu-id="68d69-108">Egy Azure Active Directory B2C létrehozása (Azure AD B2C) bérlő.</span><span class="sxs-lookup"><span data-stu-id="68d69-108">Creating an Azure Active Directory B2C (Azure AD B2C) tenant.</span></span>
2. <span data-ttu-id="68d69-109">Az Azure AD B2C-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="68d69-109">Creating an Azure AD B2C application.</span></span>
3. <span data-ttu-id="68d69-110">Két házirendmotor alkalmazás regisztrálásakor.</span><span class="sxs-lookup"><span data-stu-id="68d69-110">Registering two policy-engine applications.</span></span>
4. <span data-ttu-id="68d69-111">Kulcsok beállítása.</span><span class="sxs-lookup"><span data-stu-id="68d69-111">Setting up keys.</span></span>
5. <span data-ttu-id="68d69-112">Alapszintű csomag hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="68d69-112">Setting up hello starter pack.</span></span>

## <a name="create-an-azure-ad-app"></a><span data-ttu-id="68d69-113">Az Azure AD-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="68d69-113">Create an Azure AD app</span></span>

<span data-ttu-id="68d69-114">tooenable bejelentkezhet egy meghatározott felhasználók számára az Azure AD szervezet tooregister hello szervezeti Azure AD bérlő belül van szüksége.</span><span class="sxs-lookup"><span data-stu-id="68d69-114">tooenable sign-in for users from a specific Azure AD organization, you need tooregister an application within hello organizational Azure AD tenant.</span></span>

>[!NOTE]
> <span data-ttu-id="68d69-115">Vesszük "contoso.com" hello szervezeti Azure AD-bérlő és a "fabrikamb2c.onmicrosoft.com" hello Azure AD B2C bérlő hello az utasításoknak a.</span><span class="sxs-lookup"><span data-stu-id="68d69-115">We use "contoso.com" for hello organizational Azure AD tenant and "fabrikamb2c.onmicrosoft.com" as hello Azure AD B2C tenant in hello following instructions.</span></span>

1. <span data-ttu-id="68d69-116">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="68d69-116">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
1. <span data-ttu-id="68d69-117">Hello felső sávon válassza ki a fiókját.</span><span class="sxs-lookup"><span data-stu-id="68d69-117">On hello top bar, select your account.</span></span> <span data-ttu-id="68d69-118">A hello **Directory** menüben válassza ki a kívánt tooregister hello szervezeti Azure AD-bérlő az alkalmazás (contoso.com).</span><span class="sxs-lookup"><span data-stu-id="68d69-118">From hello **Directory** list, choose hello organizational Azure AD tenant where you want tooregister your application (contoso.com).</span></span>
1. <span data-ttu-id="68d69-119">Válassza ki **további szolgáltatások** hello bal oldali ablaktáblán, és keresse meg az "App regisztráció."</span><span class="sxs-lookup"><span data-stu-id="68d69-119">Select **More services** in hello left pane, and search for "App registrations."</span></span>
1. <span data-ttu-id="68d69-120">Válassza ki **új alkalmazás regisztrációja**.</span><span class="sxs-lookup"><span data-stu-id="68d69-120">Select **New application registration**.</span></span>
1. <span data-ttu-id="68d69-121">Adjon meg egy nevet az alkalmazáshoz (például `Azure AD B2C App`).</span><span class="sxs-lookup"><span data-stu-id="68d69-121">Enter a name for your application (for example, `Azure AD B2C App`).</span></span>
1. <span data-ttu-id="68d69-122">Válassza ki **Web app / API** hello alkalmazás típusra.</span><span class="sxs-lookup"><span data-stu-id="68d69-122">Select **Web app / API** for hello application type.</span></span>
1. <span data-ttu-id="68d69-123">A **bejelentkezési URL-cím**, adja meg a következő URL-címe, hello ahol `yourtenant` helyébe hello az Azure AD B2C bérlő neve (`fabrikamb2c.onmicrosoft.com`):</span><span class="sxs-lookup"><span data-stu-id="68d69-123">For **Sign-on URL**, enter hello following URL, where `yourtenant` is replaced by hello name of your Azure AD B2C tenant (`fabrikamb2c.onmicrosoft.com`):</span></span>

    ```
    https://login.microsoftonline.com/te/yourtenant.onmicrosoft.com/oauth2/authresp
    ```

1. <span data-ttu-id="68d69-124">Mentse a hello azonosítót.</span><span class="sxs-lookup"><span data-stu-id="68d69-124">Save hello application ID.</span></span>
1. <span data-ttu-id="68d69-125">Válassza ki az újonnan létrehozott hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="68d69-125">Select hello newly created application.</span></span>
1. <span data-ttu-id="68d69-126">A hello **beállítások** panelen válassza **kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="68d69-126">Under hello **Settings** blade, select **Keys**.</span></span>
1. <span data-ttu-id="68d69-127">Hozzon létre egy új kulcsot, és mentse.</span><span class="sxs-lookup"><span data-stu-id="68d69-127">Create a new key, and save it.</span></span> <span data-ttu-id="68d69-128">A hello a következő szakaszban található lépéseket hello még szüksége lesz rájuk.</span><span class="sxs-lookup"><span data-stu-id="68d69-128">You will use it in hello steps in hello next section.</span></span>

## <a name="add-hello-azure-ad-key-tooazure-ad-b2c"></a><span data-ttu-id="68d69-129">Az Azure AD hello kulcs tooAzure AD B2C hozzáadása</span><span class="sxs-lookup"><span data-stu-id="68d69-129">Add hello Azure AD key tooAzure AD B2C</span></span>

<span data-ttu-id="68d69-130">Az Azure AD B2C-bérlő toostore hello contoso.com alkalmazás kulcs szükséges.</span><span class="sxs-lookup"><span data-stu-id="68d69-130">You need toostore hello contoso.com application key in your Azure AD B2C tenant.</span></span> <span data-ttu-id="68d69-131">toodo ezt:</span><span class="sxs-lookup"><span data-stu-id="68d69-131">toodo this:</span></span>
1. <span data-ttu-id="68d69-132">Nyissa meg tooyour az Azure AD B2C bérlő, és válassza **B2C beállítások** > **identitás élmény keretrendszer** > **házirend kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="68d69-132">Go tooyour Azure AD B2C tenant, and select **B2C Settings** > **Identity Experience Framework** > **Policy Keys**.</span></span>
1. <span data-ttu-id="68d69-133">Válassza ki **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="68d69-133">Select **+Add**.</span></span>
1. <span data-ttu-id="68d69-134">Válassza ki vagy adja meg ezeket a beállításokat:</span><span class="sxs-lookup"><span data-stu-id="68d69-134">Select or enter these options:</span></span>
   * <span data-ttu-id="68d69-135">Válassza ki **manuális**.</span><span class="sxs-lookup"><span data-stu-id="68d69-135">Select **Manual**.</span></span>
   * <span data-ttu-id="68d69-136">A **neve**, válasszon egy nevet, amely megfelel az Azure AD-bérlő nevét (például `ContosoAppSecret`).</span><span class="sxs-lookup"><span data-stu-id="68d69-136">For **Name**, choose a name that matches your Azure AD tenant name (for example, `ContosoAppSecret`).</span></span>  <span data-ttu-id="68d69-137">hello előtag `B2C_1A_` a rendszer automatikusan hozzáadja a kulcs toohello neve.</span><span class="sxs-lookup"><span data-stu-id="68d69-137">hello prefix `B2C_1A_` is added automatically toohello name of your key.</span></span>
   * <span data-ttu-id="68d69-138">Illessze be az alkalmazás kulcs hello **titkos** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="68d69-138">Paste your application key in hello **Secret** box.</span></span>
   * <span data-ttu-id="68d69-139">Válassza ki **aláírás**.</span><span class="sxs-lookup"><span data-stu-id="68d69-139">Select **Signature**.</span></span>
1. <span data-ttu-id="68d69-140">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="68d69-140">Select **Create**.</span></span>
1. <span data-ttu-id="68d69-141">Győződjön meg arról, hogy létrehozott hello kulcs `B2C_1A_ContosoAppSecret`.</span><span class="sxs-lookup"><span data-stu-id="68d69-141">Confirm that you've created hello key `B2C_1A_ContosoAppSecret`.</span></span>


## <a name="add-a-claims-provider-in-your-base-policy"></a><span data-ttu-id="68d69-142">A jogcím-szolgáltató hozzáadása a kiinduló házirend</span><span class="sxs-lookup"><span data-stu-id="68d69-142">Add a claims provider in your base policy</span></span>

<span data-ttu-id="68d69-143">Ha azt szeretné, felhasználók toosign Azure AD használatával, egy jogcímszolgáltatótól, az Azure AD toodefine kell.</span><span class="sxs-lookup"><span data-stu-id="68d69-143">If you want users toosign in by using Azure AD, you need toodefine Azure AD as a claims provider.</span></span> <span data-ttu-id="68d69-144">Más szóval toospecify olyan végponttal, amely az Azure AD B2C is kommunikálni fognak van szüksége.</span><span class="sxs-lookup"><span data-stu-id="68d69-144">In other words, you need toospecify an endpoint that Azure AD B2C will communicate with.</span></span> <span data-ttu-id="68d69-145">hello végpont biztosítja, hogy egy adott felhasználó hitelesítette az Azure AD B2C tooverify által használt jogcímek egy készletét.</span><span class="sxs-lookup"><span data-stu-id="68d69-145">hello endpoint will provide a set of claims that are used by Azure AD B2C tooverify that a specific user has authenticated.</span></span> 

<span data-ttu-id="68d69-146">Meghatározhatja az Azure AD egy jogcímszolgáltatótól, az Azure AD toohello hozzáadásával `<ClaimsProvider>` hello bővítményfájl házirend csomópontja:</span><span class="sxs-lookup"><span data-stu-id="68d69-146">You can define Azure AD as a claims provider by adding Azure AD toohello `<ClaimsProvider>` node in hello extension file of your policy:</span></span>

1. <span data-ttu-id="68d69-147">A munkakönyvtár hello bővítményfájl (TrustFrameworkExtensions.xml) megnyitása.</span><span class="sxs-lookup"><span data-stu-id="68d69-147">Open hello extension file (TrustFrameworkExtensions.xml) from your working directory.</span></span>
1. <span data-ttu-id="68d69-148">Hello található `<ClaimsProviders>` szakasz.</span><span class="sxs-lookup"><span data-stu-id="68d69-148">Find hello `<ClaimsProviders>` section.</span></span> <span data-ttu-id="68d69-149">Ha még nem létezik, adja hozzá hello legfelső szintű csomópontja alatt.</span><span class="sxs-lookup"><span data-stu-id="68d69-149">If it does not exist, add it under hello root node.</span></span>
1. <span data-ttu-id="68d69-150">Adjon hozzá egy új `<ClaimsProvider>` csomópont az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="68d69-150">Add a new `<ClaimsProvider>` node as follows:</span></span>

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

1. <span data-ttu-id="68d69-151">A hello `<ClaimsProvider>` csomópont, a frissítés hello értéke `<Domain>` tooa egyedi érték, amely lehet használt toodistinguish az egyéb identitás-szolgáltatóktól származó.</span><span class="sxs-lookup"><span data-stu-id="68d69-151">Under hello `<ClaimsProvider>` node, update hello value for `<Domain>` tooa unique value that can be used toodistinguish it from other identity providers.</span></span>
1. <span data-ttu-id="68d69-152">A hello `<ClaimsProvider>` csomópont, a frissítés hello értéke `<DisplayName>` tooa rövid nevet a hello jogcím-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="68d69-152">Under hello `<ClaimsProvider>` node, update hello value for `<DisplayName>` tooa friendly name for hello claims provider.</span></span> <span data-ttu-id="68d69-153">Ez az érték nincs használatban.</span><span class="sxs-lookup"><span data-stu-id="68d69-153">This value is not currently used.</span></span>

### <a name="update-hello-technical-profile"></a><span data-ttu-id="68d69-154">Műszaki hello-profil frissítése</span><span class="sxs-lookup"><span data-stu-id="68d69-154">Update hello technical profile</span></span>

<span data-ttu-id="68d69-155">tooget hello Azure AD-végpont származó jogkivonat, toodefine hello protokollok, hogy az Azure AD B2C az Azure ad-val toocommunicate kell használnia kell.</span><span class="sxs-lookup"><span data-stu-id="68d69-155">tooget a token from hello Azure AD endpoint, you need toodefine hello protocols that Azure AD B2C should use toocommunicate with Azure AD.</span></span> <span data-ttu-id="68d69-156">Ez történik, belül hello `<TechnicalProfile>` eleme `<ClaimsProvider>`.</span><span class="sxs-lookup"><span data-stu-id="68d69-156">This is done inside hello `<TechnicalProfile>` element of  `<ClaimsProvider>`.</span></span>
1. <span data-ttu-id="68d69-157">Hello hello azonosítója frissítése `<TechnicalProfile>` csomópont.</span><span class="sxs-lookup"><span data-stu-id="68d69-157">Update hello ID of hello `<TechnicalProfile>` node.</span></span> <span data-ttu-id="68d69-158">Ez az azonosító nem használt toorefer toothis műszaki profil hello házirend egyéb részeitől.</span><span class="sxs-lookup"><span data-stu-id="68d69-158">This ID is used toorefer toothis technical profile from other parts of hello policy.</span></span>
1. <span data-ttu-id="68d69-159">Frissítse a hello értéket `<DisplayName>`.</span><span class="sxs-lookup"><span data-stu-id="68d69-159">Update hello value for `<DisplayName>`.</span></span> <span data-ttu-id="68d69-160">Ez az érték a hello bejelentkezési gombra a bejelentkezési képernyőn jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="68d69-160">This value will be displayed on hello sign-in button on your sign-in screen.</span></span>
1. <span data-ttu-id="68d69-161">Frissítse a hello értéket `<Description>`.</span><span class="sxs-lookup"><span data-stu-id="68d69-161">Update hello value for `<Description>`.</span></span>
1. <span data-ttu-id="68d69-162">Az Azure AD hello OpenID Connect protokollt használja, ezért figyeljen oda arra, hogy hello értéke `<Protocol>` van `"OpenIdConnect"`.</span><span class="sxs-lookup"><span data-stu-id="68d69-162">Azure AD uses hello OpenID Connect protocol, so ensure that hello value for `<Protocol>` is `"OpenIdConnect"`.</span></span>

<span data-ttu-id="68d69-163">Tooupdate hello kell `<Metadata>` hello XML-fájl a szakaszban említett tooearlier tooreflect hello konfigurációs beállításokat az adott Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="68d69-163">You need tooupdate hello `<Metadata>` section in hello XML file referred tooearlier tooreflect hello configuration settings for your specific Azure AD tenant.</span></span> <span data-ttu-id="68d69-164">Hello XML-fájl, a frissítés hello metaadat az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="68d69-164">In hello XML file, update hello metadata values as follows:</span></span>

1. <span data-ttu-id="68d69-165">Állítsa be `<Item Key="METADATA">` túl`https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, ahol `yourAzureADtenant` a Azure AD-bérlő neve (contoso.com).</span><span class="sxs-lookup"><span data-stu-id="68d69-165">Set `<Item Key="METADATA">` too`https://login.windows.net/yourAzureADtenant/.well-known/openid-configuration`, where `yourAzureADtenant` is your Azure AD tenant name (contoso.com).</span></span>
1. <span data-ttu-id="68d69-166">Nyissa meg a böngészőt, majd a toohello `METADATA` frissített URL-címet.</span><span class="sxs-lookup"><span data-stu-id="68d69-166">Open your browser and go toohello `METADATA` URL that you just updated.</span></span>
1. <span data-ttu-id="68d69-167">Hello böngészőben hello "kiállító" objektum keresse meg, és másolja az értékét.</span><span class="sxs-lookup"><span data-stu-id="68d69-167">In hello browser, look for hello 'issuer' object and copy its value.</span></span> <span data-ttu-id="68d69-168">Az alábbihoz hasonló hello: `https://sts.windows.net/{tenantId}/`.</span><span class="sxs-lookup"><span data-stu-id="68d69-168">It should look like hello following: `https://sts.windows.net/{tenantId}/`.</span></span>
1. <span data-ttu-id="68d69-169">Illessze be a hello értéke `<Item Key="ProviderName">` hello XML-fájlban.</span><span class="sxs-lookup"><span data-stu-id="68d69-169">Paste hello value for `<Item Key="ProviderName">` in hello XML file.</span></span>
1. <span data-ttu-id="68d69-170">Állítsa be `<Item Key="client_id">` toohello Alkalmazásazonosítót a hello alkalmazás regisztrációja.</span><span class="sxs-lookup"><span data-stu-id="68d69-170">Set `<Item Key="client_id">` toohello application ID from hello app registration.</span></span>
1. <span data-ttu-id="68d69-171">Állítsa be `<Item Key="IdTokenAudience">` toohello Alkalmazásazonosítót a hello alkalmazás regisztrációja.</span><span class="sxs-lookup"><span data-stu-id="68d69-171">Set `<Item Key="IdTokenAudience">` toohello application ID from hello app registration.</span></span>
1. <span data-ttu-id="68d69-172">Győződjön meg arról, hogy `<Item Key="response_types">` értéke túl`id_token`.</span><span class="sxs-lookup"><span data-stu-id="68d69-172">Ensure that `<Item Key="response_types">` is set too`id_token`.</span></span>
1. <span data-ttu-id="68d69-173">Győződjön meg arról, hogy `<Item Key="UsePolicyInRedirectUri">` értéke túl`false`.</span><span class="sxs-lookup"><span data-stu-id="68d69-173">Ensure that `<Item Key="UsePolicyInRedirectUri">` is set too`false`.</span></span>

<span data-ttu-id="68d69-174">Emellett szükség van az Azure AD B2C bérlő toohello az Azure AD a regisztrált toolink hello Azure AD titkos `<ClaimsProvider>` információkat:</span><span class="sxs-lookup"><span data-stu-id="68d69-174">You also need toolink hello Azure AD secret that you registered in your Azure AD B2C tenant toohello Azure AD `<ClaimsProvider>` information:</span></span>

* <span data-ttu-id="68d69-175">A hello `<CryptographicKeys>` XML-fájl megelőző hello területen hello értéke frissítése `StorageReferenceId` toohello hivatkozási azonosítója, amelyet megadott hello titok (például `ContosoAppSecret`).</span><span class="sxs-lookup"><span data-stu-id="68d69-175">In hello `<CryptographicKeys>` section in hello preceding XML file, update hello value for `StorageReferenceId` toohello reference ID of hello secret that you defined (for example, `ContosoAppSecret`).</span></span>

### <a name="upload-hello-extension-file-for-verification"></a><span data-ttu-id="68d69-176">Az ellenőrzéshez hello kiterjesztésű fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="68d69-176">Upload hello extension file for verification</span></span>

<span data-ttu-id="68d69-177">Mostanra, konfigurálta a házirendet, hogy az Azure AD B2C tudja, hogyan toocommunicate az Azure AD-címtárát.</span><span class="sxs-lookup"><span data-stu-id="68d69-177">By now, you have configured your policy so that Azure AD B2C knows how toocommunicate with your Azure AD directory.</span></span> <span data-ttu-id="68d69-178">Próbálja meg a házirend csak tooconfirm, hogy nincs probléma merül fel, amennyiben a hello kiterjesztésű fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="68d69-178">Try uploading hello extension file of your policy just tooconfirm that it doesn't have any issues so far.</span></span> <span data-ttu-id="68d69-179">toodo így:</span><span class="sxs-lookup"><span data-stu-id="68d69-179">toodo so:</span></span>

1. <span data-ttu-id="68d69-180">Nyissa meg hello **házirend** panel az Azure AD B2C-bérlőben.</span><span class="sxs-lookup"><span data-stu-id="68d69-180">Open hello **All Policies** blade in your Azure AD B2C tenant.</span></span>
1. <span data-ttu-id="68d69-181">Ellenőrizze a hello **hello házirend felülírja, ha létezik** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="68d69-181">Check hello **Overwrite hello policy if it exists** box.</span></span>
1. <span data-ttu-id="68d69-182">Töltse fel hello kiterjesztésű fájlt (TrustFrameworkExtensions.xml), és győződjön meg arról, hogy az érvényesítése nem hiúsul meg hello.</span><span class="sxs-lookup"><span data-stu-id="68d69-182">Upload hello extension file (TrustFrameworkExtensions.xml), and ensure that it does not fail hello validation.</span></span>

## <a name="register-hello-azure-ad-claims-provider-tooa-user-journey"></a><span data-ttu-id="68d69-183">Hello Azure AD jogcímeket szolgáltató tooa felhasználói út regisztrálása</span><span class="sxs-lookup"><span data-stu-id="68d69-183">Register hello Azure AD claims provider tooa user journey</span></span>

<span data-ttu-id="68d69-184">Most kell tooadd hello Azure AD identity provider tooone, a felhasználó utak.</span><span class="sxs-lookup"><span data-stu-id="68d69-184">You now need tooadd hello Azure AD identity provider tooone of your user journeys.</span></span> <span data-ttu-id="68d69-185">Ezen a ponton hello identitásszolgáltató be van állítva, de hello sign-Close-Up/sign-in képernyők egyik nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="68d69-185">At this point, hello identity provider has been set up, but it’s not available in any of hello sign-up/sign-in screens.</span></span> <span data-ttu-id="68d69-186">toomake akkor érhető el, rendszer létrehoz egy meglévő sablon felhasználói út duplikált, és módosíthatja azt, hogy az identitásszolgáltató hello Azure AD is rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="68d69-186">toomake it available, we will create a duplicate of an existing template user journey, and then modify it so that it also has hello Azure AD identity provider:</span></span>

1. <span data-ttu-id="68d69-187">Nyissa meg a hello alap fájlt a házirend (például TrustFrameworkBase.xml).</span><span class="sxs-lookup"><span data-stu-id="68d69-187">Open hello base file of your policy (for example, TrustFrameworkBase.xml).</span></span>
1. <span data-ttu-id="68d69-188">Hello található `<UserJourneys>` teljes elem és a példány hello `<UserJourney>` tartalmazó csomópont `Id="SignUpOrSignIn"`.</span><span class="sxs-lookup"><span data-stu-id="68d69-188">Find hello `<UserJourneys>` element and copy hello entire `<UserJourney>` node that includes `Id="SignUpOrSignIn"`.</span></span>
1. <span data-ttu-id="68d69-189">Nyissa meg a hello bővítményfájl (például TrustFrameworkExtensions.xml), és megkeresi hello `<UserJourneys>` elemet.</span><span class="sxs-lookup"><span data-stu-id="68d69-189">Open hello extension file (for example, TrustFrameworkExtensions.xml) and find hello `<UserJourneys>` element.</span></span> <span data-ttu-id="68d69-190">Ha hello elem nem létezik, vegyen fel egyet.</span><span class="sxs-lookup"><span data-stu-id="68d69-190">If hello element doesn't exist, add one.</span></span>
1. <span data-ttu-id="68d69-191">Beillesztés hello teljes `<UserJourney>` hello gyermekeként másolt csomópont `<UserJourneys>` elemet.</span><span class="sxs-lookup"><span data-stu-id="68d69-191">Paste hello entire `<UserJourney>` node that you copied as a child of hello `<UserJourneys>` element.</span></span>
1. <span data-ttu-id="68d69-192">Hello azonosító hello új felhasználói út átnevezése (például a módon nevezze át `SignUpOrSignUsingContoso`).</span><span class="sxs-lookup"><span data-stu-id="68d69-192">Rename hello ID of hello new user journey (for example, rename as `SignUpOrSignUsingContoso`).</span></span>

### <a name="display-hello-button"></a><span data-ttu-id="68d69-193">Megjelenítési hello "gombra."</span><span class="sxs-lookup"><span data-stu-id="68d69-193">Display hello "button"</span></span>

<span data-ttu-id="68d69-194">Hello `<ClaimsProviderSelection>` elem hasonló tooan identity provider gombjára a sign-Close-Up/bejelentkezési képernyő.</span><span class="sxs-lookup"><span data-stu-id="68d69-194">hello `<ClaimsProviderSelection>` element is analogous tooan identity provider button on a sign-up/sign-in screen.</span></span> <span data-ttu-id="68d69-195">Ha ad hozzá egy `<ClaimsProviderSelection>` elem az Azure AD, egy új gombon, amikor egy felhasználó fájljai hello lapon.</span><span class="sxs-lookup"><span data-stu-id="68d69-195">If you add a `<ClaimsProviderSelection>` element for Azure AD, a new button shows up when a user lands on hello page.</span></span> <span data-ttu-id="68d69-196">tooadd ezt az elemet:</span><span class="sxs-lookup"><span data-stu-id="68d69-196">tooadd this element:</span></span>

1. <span data-ttu-id="68d69-197">Hello található `<OrchestrationStep>` tartalmazó csomópont `Order="1"` az imént létrehozott hello felhasználói út.</span><span class="sxs-lookup"><span data-stu-id="68d69-197">Find hello `<OrchestrationStep>` node that includes `Order="1"` in hello user journey that you just created.</span></span>
1. <span data-ttu-id="68d69-198">Adja hozzá a hello következő:</span><span class="sxs-lookup"><span data-stu-id="68d69-198">Add hello following:</span></span>

    ```XML
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

1. <span data-ttu-id="68d69-199">Állítsa be `TargetClaimsExchangeId` tooan megfelelő értékre.</span><span class="sxs-lookup"><span data-stu-id="68d69-199">Set `TargetClaimsExchangeId` tooan appropriate value.</span></span> <span data-ttu-id="68d69-200">Javasoljuk a következő hello ugyanaz, mint mások egyezmény:  *\[ClaimProviderName\]Exchange*.</span><span class="sxs-lookup"><span data-stu-id="68d69-200">We recommend following hello same convention as others: *\[ClaimProviderName\]Exchange*.</span></span>

### <a name="link-hello-button-tooan-action"></a><span data-ttu-id="68d69-201">Hivatkozás hello tooan megnyomásakor</span><span class="sxs-lookup"><span data-stu-id="68d69-201">Link hello button tooan action</span></span>

<span data-ttu-id="68d69-202">Most, hogy a gomb helyen, toolink kell azt tooan művelet.</span><span class="sxs-lookup"><span data-stu-id="68d69-202">Now that you have a button in place, you need toolink it tooan action.</span></span> <span data-ttu-id="68d69-203">hello művelet, ebben az esetben, akkor az Azure AD B2C toocommunicate az Azure AD tooreceive jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="68d69-203">hello action, in this case, is for Azure AD B2C toocommunicate with Azure AD tooreceive a token.</span></span> <span data-ttu-id="68d69-204">Hivatkozás hello tooan megnyomásakor létrehozhatja, ha az Azure AD jogcímeket szolgáltató műszaki hello-profil:</span><span class="sxs-lookup"><span data-stu-id="68d69-204">Link hello button tooan action by linking hello technical profile for your Azure AD claims provider:</span></span>

1. <span data-ttu-id="68d69-205">Hello található `<OrchestrationStep>` is tartalmazó `Order="2"` a hello `<UserJourney>` csomópont.</span><span class="sxs-lookup"><span data-stu-id="68d69-205">Find hello `<OrchestrationStep>` that includes `Order="2"` in hello `<UserJourney>` node.</span></span>
1. <span data-ttu-id="68d69-206">Adja hozzá a hello következő:</span><span class="sxs-lookup"><span data-stu-id="68d69-206">Add hello following:</span></span>

    ```XML
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="ContosoProfile" />
    ```

1. <span data-ttu-id="68d69-207">Frissítés `Id` azonos érték, amely toohello `TargetClaimsExchangeId` az előző szakaszban hello.</span><span class="sxs-lookup"><span data-stu-id="68d69-207">Update `Id` toohello same value as that of `TargetClaimsExchangeId` in hello preceding section.</span></span>
1. <span data-ttu-id="68d69-208">Frissítés `TechnicalProfileReferenceId` műszaki hello toohello azonosítója profilját, korábban létrehozott (ContosoProfile).</span><span class="sxs-lookup"><span data-stu-id="68d69-208">Update `TechnicalProfileReferenceId` toohello ID of hello technical profile you created earlier (ContosoProfile).</span></span>

### <a name="upload-hello-updated-extension-file"></a><span data-ttu-id="68d69-209">Hello frissített kiterjesztésű fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="68d69-209">Upload hello updated extension file</span></span>

<span data-ttu-id="68d69-210">Elkészült módosítása hello kiterjesztésű fájlt.</span><span class="sxs-lookup"><span data-stu-id="68d69-210">You are done modifying hello extension file.</span></span> <span data-ttu-id="68d69-211">Mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="68d69-211">Save it.</span></span> <span data-ttu-id="68d69-212">Majd feltölteni hello fájlt, és győződjön meg arról, hogy az összes érvényesítések sikeres.</span><span class="sxs-lookup"><span data-stu-id="68d69-212">Then upload hello file, and ensure that all validations succeed.</span></span>

### <a name="update-hello-rp-file"></a><span data-ttu-id="68d69-213">Hello RP fájl frissítése</span><span class="sxs-lookup"><span data-stu-id="68d69-213">Update hello RP file</span></span>

<span data-ttu-id="68d69-214">Most kell tooupdate hello függő entitásonkénti (RP) fájl, amely fogja elindítani az imént létrehozott hello felhasználói út:</span><span class="sxs-lookup"><span data-stu-id="68d69-214">You now need tooupdate hello relying party (RP) file that will initiate hello user journey that you just created:</span></span>

1. <span data-ttu-id="68d69-215">Készítsen másolatot a SignUpOrSignIn.xml a munkakönyvtárat, és adjon neki (például nevezni tooSignUpOrSignInWithAAD.xml).</span><span class="sxs-lookup"><span data-stu-id="68d69-215">Make a copy of SignUpOrSignIn.xml in your working directory, and rename it (for example, rename it tooSignUpOrSignInWithAAD.xml).</span></span>
1. <span data-ttu-id="68d69-216">Nyissa meg hello új fájl- és frissítési hello `PolicyId` az attribútum `<TrustFrameworkPolicy>` egyedi értékkel (például SignUpOrSignInWithAAD).</span><span class="sxs-lookup"><span data-stu-id="68d69-216">Open hello new file and update hello `PolicyId` attribute for `<TrustFrameworkPolicy>` with a unique value (for example, SignUpOrSignInWithAAD).</span></span> <br> <span data-ttu-id="68d69-217">Ez lesz a házirend hello nevét (például B2C\_1A\_SignUpOrSignInWithAAD).</span><span class="sxs-lookup"><span data-stu-id="68d69-217">This will be hello name of your policy (for example, B2C\_1A\_SignUpOrSignInWithAAD).</span></span>
1. <span data-ttu-id="68d69-218">Módosítsa a hello `ReferenceId` attribútumnak `<DefaultUserJourney>` toomatch hello azonosító hello (SignUpOrSignUsingContoso) létrehozott új felhasználói út.</span><span class="sxs-lookup"><span data-stu-id="68d69-218">Modify hello `ReferenceId` attribute in `<DefaultUserJourney>` toomatch hello ID of hello new user journey that you created (SignUpOrSignUsingContoso).</span></span>
1. <span data-ttu-id="68d69-219">A módosítások mentéséhez, és hello-fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="68d69-219">Save your changes, and upload hello file.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="68d69-220">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="68d69-220">Troubleshooting</span></span>

<span data-ttu-id="68d69-221">A panel megnyitásával, majd feltöltött hello egyéni házirend tesztelése **futtatása most**.</span><span class="sxs-lookup"><span data-stu-id="68d69-221">Test hello custom policy that you just uploaded by opening its blade and clicking **Run now**.</span></span> <span data-ttu-id="68d69-222">olvassa el toodiagnose problémák [hibaelhárítási](active-directory-b2c-troubleshoot-custom.md).</span><span class="sxs-lookup"><span data-stu-id="68d69-222">toodiagnose problems, read about [troubleshooting](active-directory-b2c-troubleshoot-custom.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="68d69-223">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="68d69-223">Next steps</span></span>

<span data-ttu-id="68d69-224">Visszajelzés küldése túl[AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="68d69-224">Provide feedback too[AADB2CPreview@microsoft.com](mailto:AADB2CPreview@microsoft.com).</span></span>
