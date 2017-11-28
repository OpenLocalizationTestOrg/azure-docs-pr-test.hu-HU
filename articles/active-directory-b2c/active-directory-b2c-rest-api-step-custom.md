---
title: "Az Azure Active Directory B2C: REST API jogcím cseréje egy vezénylési lépés |} Microsoft Docs"
description: "Témakör: Azure Active Directory B2C egyéni házirendek, amelyek az API-k integrálása"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/24/2017
ms.author: joroja
ms.openlocfilehash: 90a495029f48d70232ef3f99de4ea4d351395aa7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-an-orchestration-step"></a><span data-ttu-id="41848-103">Forgatókönyv: Integrálása az Azure AD B2C felhasználói út a REST API jogcímek cseréjét egy vezénylési lépés</span><span class="sxs-lookup"><span data-stu-id="41848-103">Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as an orchestration step</span></span>

<span data-ttu-id="41848-104">hello identitás élmény keretrendszer (IEF) Azure Active Directory B2C alapjául szolgáló (az Azure AD B2C) lehetővé teszi, hogy a hello identitás fejlesztői toointegrate tevékenységet egy felhasználó út RESTful API-t.</span><span class="sxs-lookup"><span data-stu-id="41848-104">hello Identity Experience Framework (IEF) that underlies Azure Active Directory B2C (Azure AD B2C) enables hello identity developer toointegrate an interaction with a RESTful API in a user journey.</span></span>  

<span data-ttu-id="41848-105">Ez az útmutató végén hello fogja tudni toocreate az Azure AD B2C felhasználói út, amely együttműködik a RESTful-szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="41848-105">At hello end of this walkthrough, you will be able toocreate an Azure AD B2C user journey that interacts with RESTful services.</span></span>

<span data-ttu-id="41848-106">hello IEF adatok jogcímekben adatokat küld és fogad vissza a jogcímeket.</span><span class="sxs-lookup"><span data-stu-id="41848-106">hello IEF sends data in claims and receives data back in claims.</span></span> <span data-ttu-id="41848-107">REST API jogcímek exchange hello:</span><span class="sxs-lookup"><span data-stu-id="41848-107">hello REST API claims exchange:</span></span>

- <span data-ttu-id="41848-108">Egy vezénylési lépés tervezhető.</span><span class="sxs-lookup"><span data-stu-id="41848-108">Can be designed as an orchestration step.</span></span>
- <span data-ttu-id="41848-109">Egy külső műveletet is elindíthatja.</span><span class="sxs-lookup"><span data-stu-id="41848-109">Can trigger an external action.</span></span> <span data-ttu-id="41848-110">Például a külső adatbázis azt is naplózhat egy eseményt.</span><span class="sxs-lookup"><span data-stu-id="41848-110">For instance, it can log an event in an external database.</span></span>
- <span data-ttu-id="41848-111">Is lehet használt toofetch értékű, és majd hello felhasználói adatbázisban tárolja.</span><span class="sxs-lookup"><span data-stu-id="41848-111">Can be used toofetch a value and then store it in hello user database.</span></span>

<span data-ttu-id="41848-112">Hello kapott jogcímek használata újabb toochange hello folyamat végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="41848-112">You can use hello received claims later toochange hello flow of execution.</span></span>

<span data-ttu-id="41848-113">Hello interakció érvényesítési profil is tervezhet.</span><span class="sxs-lookup"><span data-stu-id="41848-113">You can also design hello interaction as a validation profile.</span></span> <span data-ttu-id="41848-114">További információkért lásd: [forgatókönyv: integrálja a REST API-t cseréjét használják az Azure AD B2C felhasználói út a jogcímeket, a felhasználói bevitel ellenőrzése](active-directory-b2c-rest-api-validation-custom.md).</span><span class="sxs-lookup"><span data-stu-id="41848-114">For more information, see [Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as validation on user input](active-directory-b2c-rest-api-validation-custom.md).</span></span>

<span data-ttu-id="41848-115">hello például az is, hogy amikor egy felhasználó egy profil szerkesztése végez, szeretnénk:</span><span class="sxs-lookup"><span data-stu-id="41848-115">hello scenario is that when a user performs a profile edit, we want to:</span></span>

1. <span data-ttu-id="41848-116">Hello felhasználói kereshet meg egy külső rendszerben.</span><span class="sxs-lookup"><span data-stu-id="41848-116">Look up hello user in an external system.</span></span>
2. <span data-ttu-id="41848-117">Ha regisztrálva van-e az adott felhasználó hello város beolvasása.</span><span class="sxs-lookup"><span data-stu-id="41848-117">Get hello city where that user is registered.</span></span>
3. <span data-ttu-id="41848-118">Az attribútum toohello alkalmazást jogcímként visszaadása.</span><span class="sxs-lookup"><span data-stu-id="41848-118">Return that attribute toohello application as a claim.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="41848-119">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="41848-119">Prerequisites</span></span>

- <span data-ttu-id="41848-120">Az Azure AD B2C bérlő konfigurált toocomplete sign-Close-Up/bejelentkezés, a helyi fiók [bevezetés](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="41848-120">An Azure AD B2C tenant configured toocomplete a local account sign-up/sign-in, as described in [Getting started](active-directory-b2c-get-started-custom.md).</span></span>
- <span data-ttu-id="41848-121">A REST API végpont toointeract együtt.</span><span class="sxs-lookup"><span data-stu-id="41848-121">A REST API endpoint toointeract with.</span></span> <span data-ttu-id="41848-122">Ez az útmutató egy egyszerű Azure függvény app webhook használja példaként.</span><span class="sxs-lookup"><span data-stu-id="41848-122">This walkthrough uses a simple Azure function app webhook as an example.</span></span>
- <span data-ttu-id="41848-123">*Ajánlott*: teljes hello [REST API-jogcímek exchange forgatókönyv érvényesítési lépésként](active-directory-b2c-rest-api-validation-custom.md).</span><span class="sxs-lookup"><span data-stu-id="41848-123">*Recommended*: Complete hello [REST API claims exchange walkthrough as a validation step](active-directory-b2c-rest-api-validation-custom.md).</span></span>

## <a name="step-1-prepare-hello-rest-api-function"></a><span data-ttu-id="41848-124">1. lépés: Felkészülés hello REST API-függvénye</span><span class="sxs-lookup"><span data-stu-id="41848-124">Step 1: Prepare hello REST API function</span></span>

> [!NOTE]
> <span data-ttu-id="41848-125">A REST API-függvények telepítése Ez a cikk hello hatókörén kívül esik.</span><span class="sxs-lookup"><span data-stu-id="41848-125">Setup of REST API functions is outside hello scope of this article.</span></span> <span data-ttu-id="41848-126">[Az Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) biztosít egy kiváló eszközkészlet toocreate RESTful-szolgáltatásokat hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="41848-126">[Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) provides an excellent toolkit toocreate RESTful services in hello cloud.</span></span>

<span data-ttu-id="41848-127">Beállítjuk van egy Azure függvény, amely megkapja a jogcím nevű `email`, majd vissza hello jogcím és `city` hozzárendelt hello értékű `Redmond`.</span><span class="sxs-lookup"><span data-stu-id="41848-127">We have set up an Azure function that receives a claim called `email`, and then returns hello claim `city` with hello assigned value of `Redmond`.</span></span> <span data-ttu-id="41848-128">hello minta Azure függvény megtalálható [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).</span><span class="sxs-lookup"><span data-stu-id="41848-128">hello sample Azure function is on [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).</span></span>

<span data-ttu-id="41848-129">Hello `userMessage` jogcímet, amely hello Azure függvény által visszaadott nem kötelező ebben a környezetben, és hello IEF figyelmen kívül hagyja azt.</span><span class="sxs-lookup"><span data-stu-id="41848-129">hello `userMessage` claim that hello Azure function returns is optional in this context, and hello IEF will ignore it.</span></span> <span data-ttu-id="41848-130">Potenciálisan használhatja az üzenet átadott toohello alkalmazás, és később bemutatott toohello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="41848-130">You can potentially use it as a message passed toohello application and presented toohello user later.</span></span>

```csharp
if (requestContentAsJObject.email == null)
{
    return request.CreateResponse(HttpStatusCode.BadRequest);
}

var email = ((string) requestContentAsJObject.email).ToLower();

return request.CreateResponse<ResponseContent>(
    HttpStatusCode.OK,
    new ResponseContent
    {
        version = "1.0.0",
        status = (int) HttpStatusCode.OK,
        userMessage = "User Found",
        city = "Redmond"
    },
    new JsonMediaTypeFormatter(),
    "application/json");
```

<span data-ttu-id="41848-131">Egy Azure függvény alkalmazás könnyen tooget hello függvény URL-címe, hello azonosítóval hello adott függvény segítségével.</span><span class="sxs-lookup"><span data-stu-id="41848-131">An Azure function app makes it easy tooget hello function URL, which includes hello identifier of hello specific function.</span></span> <span data-ttu-id="41848-132">Ebben az esetben hello URL-cím van: https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==.</span><span class="sxs-lookup"><span data-stu-id="41848-132">In this case, hello URL is: https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==.</span></span> <span data-ttu-id="41848-133">Tesztelési használhatja.</span><span class="sxs-lookup"><span data-stu-id="41848-133">You can use it for testing.</span></span>

## <a name="step-2-configure-hello-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworextensionsxml-file"></a><span data-ttu-id="41848-134">2. lépés: Hello RESTful API jogcímek az exchange konfigurálása a TrustFrameworExtensions.xml fájlban műszaki profil</span><span class="sxs-lookup"><span data-stu-id="41848-134">Step 2: Configure hello RESTful API claims exchange as a technical profile in your TrustFrameworExtensions.xml file</span></span>

<span data-ttu-id="41848-135">Egy technikai profil beállítás hello teljes hello Exchange hello RESTful szolgáltatás a szükséges.</span><span class="sxs-lookup"><span data-stu-id="41848-135">A technical profile is hello full configuration of hello exchange desired with hello RESTful service.</span></span> <span data-ttu-id="41848-136">Nyissa meg hello TrustFrameworkExtensions.xml fájlt, és adja hozzá a következő XML-részletet belül hello hello `<ClaimsProvider>` elemet.</span><span class="sxs-lookup"><span data-stu-id="41848-136">Open hello TrustFrameworkExtensions.xml file and add hello following XML snippet inside hello `<ClaimsProvider>` element.</span></span>

> [!NOTE]
> <span data-ttu-id="41848-137">A következő XML-RESTful szolgáltató hello `Version=1.0.0.0` hello protokollként leírása.</span><span class="sxs-lookup"><span data-stu-id="41848-137">In hello following XML, RESTful provider `Version=1.0.0.0` is described as hello protocol.</span></span> <span data-ttu-id="41848-138">Tekinti azokat hello függvény hello külső szolgáltatással együtt fog működni.</span><span class="sxs-lookup"><span data-stu-id="41848-138">Consider it as hello function that will interact with hello external service.</span></span> <!-- TODO: A full definition of hello schema can be found...link tooRESTful Provider schema definition>-->

```XML
<ClaimsProvider>
    <DisplayName>REST APIs</DisplayName>
    <TechnicalProfiles>
        <TechnicalProfile Id="AzureFunctions-LookUpLoyaltyWebHook">
            <DisplayName>Check LookUpLoyalty Web Hook Azure Function</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
                <Item Key="ServiceUrl">https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==</Item>
                <Item Key="AuthenticationType">None</Item>
                <Item Key="SendClaimsIn">Body</Item>
            </Metadata>
            <InputClaims>
                <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="email" />
            </InputClaims>
            <OutputClaims>
                <OutputClaim ClaimTypeReferenceId="city" PartnerClaimType="city" />
            </OutputClaims>
            <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

<span data-ttu-id="41848-139">Hello `<InputClaims>` elem definiálja hello küldi hello IEF toohello REST-szolgáltatást a jogcímeket.</span><span class="sxs-lookup"><span data-stu-id="41848-139">hello `<InputClaims>` element defines hello claims that will be sent from hello IEF toohello REST service.</span></span> <span data-ttu-id="41848-140">Ebben a példában hello hello jogcím tartalmát `givenName` toohello REST-szolgáltatást küldi hello jogcímként `email`.</span><span class="sxs-lookup"><span data-stu-id="41848-140">In this example, hello contents of hello claim `givenName` will be sent toohello REST service as hello claim `email`.</span></span>  

<span data-ttu-id="41848-141">Hello `<OutputClaims>` elem definiálja hello jogcímeket, hogy hello IEF fog várható hello REST-szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="41848-141">hello `<OutputClaims>` element defines hello claims that hello IEF will expect from hello REST service.</span></span> <span data-ttu-id="41848-142">Kapott jogcímek hello száma, függetlenül hello IEF használja csak azonosított itt.</span><span class="sxs-lookup"><span data-stu-id="41848-142">Regardless of hello number of claims that are received, hello IEF will use only those identified here.</span></span> <span data-ttu-id="41848-143">Ebben a példában a jogcím érkezett `city` csatlakoztatott tooan IEF jogcím fogja meghívni `city`.</span><span class="sxs-lookup"><span data-stu-id="41848-143">In this example, a claim received as `city` will be mapped tooan IEF claim called `city`.</span></span>

## <a name="step-3-add-hello-new-claim-city-toohello-schema-of-your-trustframeworkextensionsxml-file"></a><span data-ttu-id="41848-144">3. lépés: Új jogcímet hello hozzáadása `city` toohello séma a TrustFrameworkExtensions.xml fájl</span><span class="sxs-lookup"><span data-stu-id="41848-144">Step 3: Add hello new claim `city` toohello schema of your TrustFrameworkExtensions.xml file</span></span>

<span data-ttu-id="41848-145">hello jogcím `city` nincs még definiálva a sémában.</span><span class="sxs-lookup"><span data-stu-id="41848-145">hello claim `city` is not yet defined anywhere in our schema.</span></span> <span data-ttu-id="41848-146">Igen, hozzáadása hello elemen belül `<BuildingBlocks>`.</span><span class="sxs-lookup"><span data-stu-id="41848-146">So, add a definition inside hello element `<BuildingBlocks>`.</span></span> <span data-ttu-id="41848-147">Ez az elem hello TrustFrameworkExtensions.xml fájl hello elején található.</span><span class="sxs-lookup"><span data-stu-id="41848-147">You can find this element at hello beginning of hello TrustFrameworkExtensions.xml file.</span></span>

```XML
<BuildingBlocks>
    <!--hello claimtype city must be added toohello TrustFrameworkPolicy-->
    <!-- You can add new claims in hello BASE file Section III, or in hello extensions file-->
    <ClaimsSchema>
        <ClaimType Id="city">
            <DisplayName>City</DisplayName>
            <DataType>string</DataType>
            <UserHelpText>Your city</UserHelpText>
            <UserInputType>TextBox</UserInputType>
        </ClaimType>
    </ClaimsSchema>
</BuildingBlocks>
```

## <a name="step-4-include-hello-rest-service-claims-exchange-as-an-orchestration-step-in-your-profile-edit-user-journey-in-trustframeworkextensionsxml"></a><span data-ttu-id="41848-148">4. lépés: Hello REST szolgáltatási igények exchange közé az orchestration lépése a profil szerkesztése felhasználói megtett út TrustFrameworkExtensions.xml</span><span class="sxs-lookup"><span data-stu-id="41848-148">Step 4: Include hello REST service claims exchange as an orchestration step in your profile edit user journey in TrustFrameworkExtensions.xml</span></span>

<span data-ttu-id="41848-149">Adjon hozzá egy lépést toohello profil szerkesztése felhasználói út után hello felhasználó hitelesített (vezénylési lépésekből 1-4 a következő XML hello), és hello felhasználó már rendelkezik frissített hello-profil adatait (5. lépés).</span><span class="sxs-lookup"><span data-stu-id="41848-149">Add a step toohello profile edit user journey, after hello user has been authenticated (orchestration steps 1-4 in hello following XML) and hello user has provided hello updated profile information (step 5).</span></span>

> [!NOTE]
> <span data-ttu-id="41848-150">Nincsenek hello REST API-hívás helyének egy vezénylési lépés számos használhatók.</span><span class="sxs-lookup"><span data-stu-id="41848-150">There are many use cases where hello REST API call can be used as an orchestration step.</span></span> <span data-ttu-id="41848-151">Egy vezénylési lépés azt követően a felhasználó sikeresen befejezte egy feladat, például az első regisztráció frissítés tooan külső rendszerekből használható vagy profil frissítése tookeep adatok szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="41848-151">As an orchestration step, it can be used as an update tooan external system after a user has successfully completed a task like first-time registration, or as a profile update tookeep information synchronized.</span></span> <span data-ttu-id="41848-152">Ebben az esetben használt tooaugment hello információk toohello alkalmazás hello-profil szerkesztése után.</span><span class="sxs-lookup"><span data-stu-id="41848-152">In this case, it's used tooaugment hello information provided toohello application after hello profile edit.</span></span>

<span data-ttu-id="41848-153">Másolás hello-profil szerkesztése felhasználói út XML-kódot fájlból hello TrustFrameworkBase.xml fájl tooyour TrustFrameworkExtensions.xml belül hello `<UserJourneys>` elemet.</span><span class="sxs-lookup"><span data-stu-id="41848-153">Copy hello profile edit user journey XML code from hello TrustFrameworkBase.xml file tooyour TrustFrameworkExtensions.xml file inside hello `<UserJourneys>` element.</span></span> <span data-ttu-id="41848-154">Végezze el a 6. lépés hello módosítását.</span><span class="sxs-lookup"><span data-stu-id="41848-154">Then make hello modification under step 6.</span></span>

```XML
<OrchestrationStep Order="6" Type="ClaimsExchange">
    <ClaimsExchanges>
        <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-LookUpLoyaltyWebHook" />
    </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> <span data-ttu-id="41848-155">Ha hello sorrendje eltér a verziót, győződjön meg arról, hogy hello lépése előtt hello hello kód beszúrása `ClaimsExchange` típus `SendClaims`.</span><span class="sxs-lookup"><span data-stu-id="41848-155">If hello order does not match your version, make sure that you insert hello code as hello step before hello `ClaimsExchange` type `SendClaims`.</span></span>

<span data-ttu-id="41848-156">hello hello felhasználói útra végső XML kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="41848-156">hello final XML for hello user journey should look like this:</span></span>

```XML
<UserJourney Id="ProfileEdit">
    <OrchestrationSteps>
        <OrchestrationStep Order="1" Type="ClaimsProviderSelection" ContentDefinitionReferenceId="api.idpselections">
            <ClaimsProviderSelections>
                <ClaimsProviderSelection TargetClaimsExchangeId="FacebookExchange" />
                <ClaimsProviderSelection TargetClaimsExchangeId="LocalAccountSigninEmailExchange" />
            </ClaimsProviderSelections>
        </OrchestrationStep>
        <OrchestrationStep Order="2" Type="ClaimsExchange">
            <ClaimsExchanges>
                <ClaimsExchange Id="FacebookExchange" TechnicalProfileReferenceId="Facebook-OAUTH" />
                <ClaimsExchange Id="LocalAccountSigninEmailExchange" TechnicalProfileReferenceId="SelfAsserted-LocalAccountSignin-Email" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="3" Type="ClaimsExchange">
            <Preconditions>
                <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
                    <Value>authenticationSource</Value>
                    <Value>localAccountAuthentication</Value>
                    <Action>SkipThisOrchestrationStep</Action>
                </Precondition>
            </Preconditions>
            <ClaimsExchanges>
                <ClaimsExchange Id="AADUserRead" TechnicalProfileReferenceId="AAD-UserReadUsingAlternativeSecurityId" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="4" Type="ClaimsExchange">
            <Preconditions>
                <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
                    <Value>authenticationSource</Value>
                    <Value>socialIdpAuthentication</Value>
                    <Action>SkipThisOrchestrationStep</Action>
                </Precondition>
            </Preconditions>
            <ClaimsExchanges>
                <ClaimsExchange Id="AADUserReadWithObjectId" TechnicalProfileReferenceId="AAD-UserReadUsingObjectId" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="5" Type="ClaimsExchange">
            <ClaimsExchanges>
                <ClaimsExchange Id="B2CUserProfileUpdateExchange" TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <!-- Add a step 6 toohello user journey before hello JWT token is created-->
        <OrchestrationStep Order="6" Type="ClaimsExchange">
            <ClaimsExchanges>
                <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-LookUpLoyaltyWebHook" />
            </ClaimsExchanges>
        </OrchestrationStep>
        <OrchestrationStep Order="7" Type="SendClaims" CpimIssuerTechnicalProfileReferenceId="JwtIssuer" />
    </OrchestrationSteps>
    <ClientDefinition ReferenceId="DefaultWeb" />
</UserJourney>
```

## <a name="step-5-add-hello-claim-city-tooyour-relying-party-policy-file-so-hello-claim-is-sent-tooyour-application"></a><span data-ttu-id="41848-157">5. lépés: Hello jogcímszabály hozzáadása `city` tooyour függő entitás házirend fájlt, így hello jogcím küldött tooyour alkalmazás</span><span class="sxs-lookup"><span data-stu-id="41848-157">Step 5: Add hello claim `city` tooyour relying party policy file so hello claim is sent tooyour application</span></span>

<span data-ttu-id="41848-158">Szerkessze a ProfileEdit.xml függő entitásonkénti (RP) fájlt, és módosítsa a hello `<TechnicalProfile Id="PolicyProfile">` elem tooadd hello alábbi: `<OutputClaim ClaimTypeReferenceId="city" />`.</span><span class="sxs-lookup"><span data-stu-id="41848-158">Edit your ProfileEdit.xml relying party (RP) file and modify hello `<TechnicalProfile Id="PolicyProfile">` element tooadd hello following: `<OutputClaim ClaimTypeReferenceId="city" />`.</span></span>

<span data-ttu-id="41848-159">Miután hozzáadta a hello új jogcímet, akkor a műszaki hello-profil néz ki:</span><span class="sxs-lookup"><span data-stu-id="41848-159">After you add hello new claim, hello technical profile looks like this:</span></span>

```XML
<DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="OpenIdConnect" />
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
      <OutputClaim ClaimTypeReferenceId="city" />
    </OutputClaims>
    <SubjectNamingInfo ClaimType="sub" />
</TechnicalProfile>
```

## <a name="step-6-upload-your-changes-and-test"></a><span data-ttu-id="41848-160">6. lépés: Töltse fel a módosításokat, és tesztelése</span><span class="sxs-lookup"><span data-stu-id="41848-160">Step 6: Upload your changes and test</span></span>

<span data-ttu-id="41848-161">Meglévő verzióinak hello hello házirend felülírja.</span><span class="sxs-lookup"><span data-stu-id="41848-161">Overwrite hello existing versions of hello policy.</span></span>

1.  <span data-ttu-id="41848-162">(Nem kötelező:) Mentse hello meglévő verzióját (Letöltés) a bővítmények fájl folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="41848-162">(Optional:) Save hello existing version (by downloading) of your extensions file before you proceed.</span></span> <span data-ttu-id="41848-163">tookeep hello kezdeti összetettsége alacsony, azt javasoljuk, hogy nem töltsön hello bővítmények fájl több verziója.</span><span class="sxs-lookup"><span data-stu-id="41848-163">tookeep hello initial complexity low, we recommend that you do not upload multiple versions of hello extensions file.</span></span>
2.  <span data-ttu-id="41848-164">(Nem kötelező:) Nevezze át a hello házirend azonosítója hello házirend szerkesztése fájl új verziójának hello módosításával `PolicyId="B2C_1A_TrustFrameworkProfileEdit"`.</span><span class="sxs-lookup"><span data-stu-id="41848-164">(Optional:) Rename hello new version of hello policy ID for hello policy edit file by changing   `PolicyId="B2C_1A_TrustFrameworkProfileEdit"`.</span></span>
3.  <span data-ttu-id="41848-165">Hello bővítmények fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="41848-165">Upload hello extensions file.</span></span>
4.  <span data-ttu-id="41848-166">Hello házirend szerkesztése RP-fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="41848-166">Upload hello policy edit RP file.</span></span>
5.  <span data-ttu-id="41848-167">Használjon **Futtatás most** tootest hello házirend.</span><span class="sxs-lookup"><span data-stu-id="41848-167">Use **Run Now** tootest hello policy.</span></span> <span data-ttu-id="41848-168">Tekintse át a hello jogkivonatot adott hello IEF toohello alkalmazás adja vissza.</span><span class="sxs-lookup"><span data-stu-id="41848-168">Review hello token that hello IEF returns toohello application.</span></span>

<span data-ttu-id="41848-169">Ha minden megfelelően van beállítva, hello jogkivonat tartalmazza hello új jogcímet `city`, hello értékű `Redmond`.</span><span class="sxs-lookup"><span data-stu-id="41848-169">If everything is set up correctly, hello token will include hello new claim `city`, with hello value `Redmond`.</span></span>

```JSON
{
  "exp": 1493053292,
  "nbf": 1493049692,
  "ver": "1.0",
  "iss": "https://login.microsoftonline.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "a58e7c6c-7535-4074-93da-b0023fbaf3ac",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustframeworkprofileedit",
  "nonce": "defaultNonce",
  "iat": 1493049692,
  "auth_time": 1493049692,
  "city": "Redmond"
}
```

## <a name="next-steps"></a><span data-ttu-id="41848-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="41848-170">Next steps</span></span>

[<span data-ttu-id="41848-171">A REST API-t használják a ellenőrzési lépés</span><span class="sxs-lookup"><span data-stu-id="41848-171">Use a REST API as a validation step</span></span>](active-directory-b2c-rest-api-validation-custom.md)

[<span data-ttu-id="41848-172">Hello profil szerkesztése toogather további információt a felhasználók a módosítása</span><span class="sxs-lookup"><span data-stu-id="41848-172">Modify hello profile edit toogather additional information from your users</span></span>](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)
