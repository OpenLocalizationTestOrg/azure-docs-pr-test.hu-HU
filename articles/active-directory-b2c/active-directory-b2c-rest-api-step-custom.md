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
ms.openlocfilehash: dc319c97e64e55861b84cc3943667418077a05d8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-an-orchestration-step"></a><span data-ttu-id="52357-103">Forgatókönyv: Integrálása az Azure AD B2C felhasználói út a REST API jogcímek cseréjét egy vezénylési lépés</span><span class="sxs-lookup"><span data-stu-id="52357-103">Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as an orchestration step</span></span>

<span data-ttu-id="52357-104">Az identitás élmény keretrendszer (IEF) Azure Active Directory B2C alapjául szolgáló (az Azure AD B2C) lehetővé teszi, hogy a tevékenységet egy felhasználó út RESTful API-t integrálja a identitás fejlesztő.</span><span class="sxs-lookup"><span data-stu-id="52357-104">The Identity Experience Framework (IEF) that underlies Azure Active Directory B2C (Azure AD B2C) enables the identity developer to integrate an interaction with a RESTful API in a user journey.</span></span>  

<span data-ttu-id="52357-105">Ez az útmutató végén lesz létrehozása az Azure AD B2C felhasználói út, amely együttműködik a RESTful-szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="52357-105">At the end of this walkthrough, you will be able to create an Azure AD B2C user journey that interacts with RESTful services.</span></span>

<span data-ttu-id="52357-106">A IEF adatok jogcímekben adatokat küld és fogad vissza a jogcímeket.</span><span class="sxs-lookup"><span data-stu-id="52357-106">The IEF sends data in claims and receives data back in claims.</span></span> <span data-ttu-id="52357-107">A REST API-exchange jogcímek:</span><span class="sxs-lookup"><span data-stu-id="52357-107">The REST API claims exchange:</span></span>

- <span data-ttu-id="52357-108">Egy vezénylési lépés tervezhető.</span><span class="sxs-lookup"><span data-stu-id="52357-108">Can be designed as an orchestration step.</span></span>
- <span data-ttu-id="52357-109">Egy külső műveletet is elindíthatja.</span><span class="sxs-lookup"><span data-stu-id="52357-109">Can trigger an external action.</span></span> <span data-ttu-id="52357-110">Például a külső adatbázis azt is naplózhat egy eseményt.</span><span class="sxs-lookup"><span data-stu-id="52357-110">For instance, it can log an event in an external database.</span></span>
- <span data-ttu-id="52357-111">Olyan érték beolvasása, és majd tárolja a felhasználói adatbázis használható.</span><span class="sxs-lookup"><span data-stu-id="52357-111">Can be used to fetch a value and then store it in the user database.</span></span>

<span data-ttu-id="52357-112">A kapott jogcímek később segítségével módosíthatja a végrehajtási folyamat.</span><span class="sxs-lookup"><span data-stu-id="52357-112">You can use the received claims later to change the flow of execution.</span></span>

<span data-ttu-id="52357-113">A kapcsolati érvényesítési profil tervezhet.</span><span class="sxs-lookup"><span data-stu-id="52357-113">You can also design the interaction as a validation profile.</span></span> <span data-ttu-id="52357-114">További információkért lásd: [forgatókönyv: integrálja a REST API-t cseréjét használják az Azure AD B2C felhasználói út a jogcímeket, a felhasználói bevitel ellenőrzése](active-directory-b2c-rest-api-validation-custom.md).</span><span class="sxs-lookup"><span data-stu-id="52357-114">For more information, see [Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as validation on user input](active-directory-b2c-rest-api-validation-custom.md).</span></span>

<span data-ttu-id="52357-115">A például az is, hogy amikor egy felhasználó egy profil szerkesztése végez, szeretnénk:</span><span class="sxs-lookup"><span data-stu-id="52357-115">The scenario is that when a user performs a profile edit, we want to:</span></span>

1. <span data-ttu-id="52357-116">Keresse meg a felhasználó egy külső rendszerben.</span><span class="sxs-lookup"><span data-stu-id="52357-116">Look up the user in an external system.</span></span>
2. <span data-ttu-id="52357-117">A város, amelyen regisztrálva van-e az, hogy a felhasználó beolvasása.</span><span class="sxs-lookup"><span data-stu-id="52357-117">Get the city where that user is registered.</span></span>
3. <span data-ttu-id="52357-118">Térjen vissza az alkalmazás jogcímként ezt az attribútumot.</span><span class="sxs-lookup"><span data-stu-id="52357-118">Return that attribute to the application as a claim.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="52357-119">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="52357-119">Prerequisites</span></span>

- <span data-ttu-id="52357-120">Egy helyi fiókot sign-Close-Up/sign-in befejezéséhez, a konfigurált Azure AD B2C-bérlő [bevezetés](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="52357-120">An Azure AD B2C tenant configured to complete a local account sign-up/sign-in, as described in [Getting started](active-directory-b2c-get-started-custom.md).</span></span>
- <span data-ttu-id="52357-121">REST API-végpont kommunikál.</span><span class="sxs-lookup"><span data-stu-id="52357-121">A REST API endpoint to interact with.</span></span> <span data-ttu-id="52357-122">Ez az útmutató egy egyszerű Azure függvény app webhook használja példaként.</span><span class="sxs-lookup"><span data-stu-id="52357-122">This walkthrough uses a simple Azure function app webhook as an example.</span></span>
- <span data-ttu-id="52357-123">*Ajánlott*: végezze el a [REST API-jogcímek exchange forgatókönyv érvényesítési lépésként](active-directory-b2c-rest-api-validation-custom.md).</span><span class="sxs-lookup"><span data-stu-id="52357-123">*Recommended*: Complete the [REST API claims exchange walkthrough as a validation step](active-directory-b2c-rest-api-validation-custom.md).</span></span>

## <a name="step-1-prepare-the-rest-api-function"></a><span data-ttu-id="52357-124">1. lépés: Felkészülés a REST API-függvénye</span><span class="sxs-lookup"><span data-stu-id="52357-124">Step 1: Prepare the REST API function</span></span>

> [!NOTE]
> <span data-ttu-id="52357-125">Ez a cikk hatókörén kívül található a REST API-függvények telepítése.</span><span class="sxs-lookup"><span data-stu-id="52357-125">Setup of REST API functions is outside the scope of this article.</span></span> <span data-ttu-id="52357-126">[Az Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) biztosít egy kiváló eszközkészlet RESTful szolgáltatás létrehozásához a felhőben.</span><span class="sxs-lookup"><span data-stu-id="52357-126">[Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) provides an excellent toolkit to create RESTful services in the cloud.</span></span>

<span data-ttu-id="52357-127">Beállítjuk van egy Azure függvény, amely megkapja a jogcím nevű `email`, majd visszatér az a jogcím `city` hozzárendelt értékét `Redmond`.</span><span class="sxs-lookup"><span data-stu-id="52357-127">We have set up an Azure function that receives a claim called `email`, and then returns the claim `city` with the assigned value of `Redmond`.</span></span> <span data-ttu-id="52357-128">A minta Azure függvény megtalálható [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).</span><span class="sxs-lookup"><span data-stu-id="52357-128">The sample Azure function is on [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).</span></span>

<span data-ttu-id="52357-129">A `userMessage` jogcímet, amely az Azure függvény nem kötelező ebben a környezetben, és a IEF figyelmen kívül hagyja azt.</span><span class="sxs-lookup"><span data-stu-id="52357-129">The `userMessage` claim that the Azure function returns is optional in this context, and the IEF will ignore it.</span></span> <span data-ttu-id="52357-130">Potenciálisan akár is használhatja az alkalmazásnak átadott, és később a felhasználó számára megjelenő üzenet.</span><span class="sxs-lookup"><span data-stu-id="52357-130">You can potentially use it as a message passed to the application and presented to the user later.</span></span>

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

<span data-ttu-id="52357-131">Egy Azure függvény alkalmazás megkönnyíti az első a függvény URL-cím, az adott függvény azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="52357-131">An Azure function app makes it easy to get the function URL, which includes the identifier of the specific function.</span></span> <span data-ttu-id="52357-132">Ebben az esetben a URL-je: https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==.</span><span class="sxs-lookup"><span data-stu-id="52357-132">In this case, the URL is: https://wingtipb2cfuncs.azurewebsites.net/api/LookUpLoyaltyWebHook?code=MQuG7BIE3eXBaCZ/YCfY1SHabm55HEphpNLmh1OP3hdfHkvI2QwPrw==.</span></span> <span data-ttu-id="52357-133">Tesztelési használhatja.</span><span class="sxs-lookup"><span data-stu-id="52357-133">You can use it for testing.</span></span>

## <a name="step-2-configure-the-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworextensionsxml-file"></a><span data-ttu-id="52357-134">2. lépés: A RESTful API jogcímek az exchange konfigurálása a TrustFrameworExtensions.xml fájlban műszaki profil</span><span class="sxs-lookup"><span data-stu-id="52357-134">Step 2: Configure the RESTful API claims exchange as a technical profile in your TrustFrameworExtensions.xml file</span></span>

<span data-ttu-id="52357-135">Műszaki profilt a RESTful szolgáltatás szükséges az exchange teljes konfigurációjának.</span><span class="sxs-lookup"><span data-stu-id="52357-135">A technical profile is the full configuration of the exchange desired with the RESTful service.</span></span> <span data-ttu-id="52357-136">Nyissa meg a TrustFrameworkExtensions.xml fájlt, és adja hozzá a következő XML-részletet belül a `<ClaimsProvider>` elemet.</span><span class="sxs-lookup"><span data-stu-id="52357-136">Open the TrustFrameworkExtensions.xml file and add the following XML snippet inside the `<ClaimsProvider>` element.</span></span>

> [!NOTE]
> <span data-ttu-id="52357-137">A következő XML-kódban, RESTful szolgáltató `Version=1.0.0.0` protokoll leírását.</span><span class="sxs-lookup"><span data-stu-id="52357-137">In the following XML, RESTful provider `Version=1.0.0.0` is described as the protocol.</span></span> <span data-ttu-id="52357-138">Vegye figyelembe azt, a függvény, amely a külső szolgáltatással együtt fog működni.</span><span class="sxs-lookup"><span data-stu-id="52357-138">Consider it as the function that will interact with the external service.</span></span> <!-- TODO: A full definition of the schema can be found...link to RESTful Provider schema definition>-->

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

<span data-ttu-id="52357-139">A `<InputClaims>` elem definiálja a jogcímeket, amely a REST-szolgáltatást a IEF a kapnak.</span><span class="sxs-lookup"><span data-stu-id="52357-139">The `<InputClaims>` element defines the claims that will be sent from the IEF to the REST service.</span></span> <span data-ttu-id="52357-140">Ebben a példában a jogcím tartalmát `givenName` a REST-szolgáltatást a jogcímként való küldésének `email`.</span><span class="sxs-lookup"><span data-stu-id="52357-140">In this example, the contents of the claim `givenName` will be sent to the REST service as the claim `email`.</span></span>  

<span data-ttu-id="52357-141">A `<OutputClaims>` elem definiálja a IEF várható fog a többi szolgáltatás jogcímeket.</span><span class="sxs-lookup"><span data-stu-id="52357-141">The `<OutputClaims>` element defines the claims that the IEF will expect from the REST service.</span></span> <span data-ttu-id="52357-142">Kapott jogcímek száma, függetlenül a IEF használja csak azonosított itt.</span><span class="sxs-lookup"><span data-stu-id="52357-142">Regardless of the number of claims that are received, the IEF will use only those identified here.</span></span> <span data-ttu-id="52357-143">Ebben a példában a jogcím érkezett `city` kell hozzárendelni egy IEF nevű jogcím `city`.</span><span class="sxs-lookup"><span data-stu-id="52357-143">In this example, a claim received as `city` will be mapped to an IEF claim called `city`.</span></span>

## <a name="step-3-add-the-new-claim-city-to-the-schema-of-your-trustframeworkextensionsxml-file"></a><span data-ttu-id="52357-144">3. lépés:, Adja hozzá az új jogcímet `city` a sémának a TrustFrameworkExtensions.xml fájl</span><span class="sxs-lookup"><span data-stu-id="52357-144">Step 3: Add the new claim `city` to the schema of your TrustFrameworkExtensions.xml file</span></span>

<span data-ttu-id="52357-145">A jogcímek `city` nincs még definiálva a sémában.</span><span class="sxs-lookup"><span data-stu-id="52357-145">The claim `city` is not yet defined anywhere in our schema.</span></span> <span data-ttu-id="52357-146">Igen, hozzáadása a elemen belül `<BuildingBlocks>`.</span><span class="sxs-lookup"><span data-stu-id="52357-146">So, add a definition inside the element `<BuildingBlocks>`.</span></span> <span data-ttu-id="52357-147">Ez az elem a TrustFrameworkExtensions.xml fájl elején található.</span><span class="sxs-lookup"><span data-stu-id="52357-147">You can find this element at the beginning of the TrustFrameworkExtensions.xml file.</span></span>

```XML
<BuildingBlocks>
    <!--The claimtype city must be added to the TrustFrameworkPolicy-->
    <!-- You can add new claims in the BASE file Section III, or in the extensions file-->
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

## <a name="step-4-include-the-rest-service-claims-exchange-as-an-orchestration-step-in-your-profile-edit-user-journey-in-trustframeworkextensionsxml"></a><span data-ttu-id="52357-148">4. lépés: A profil szerkesztése felhasználói megtett út TrustFrameworkExtensions.xml lépést az orchestration közé a többi szolgáltatás jogcímek exchange</span><span class="sxs-lookup"><span data-stu-id="52357-148">Step 4: Include the REST service claims exchange as an orchestration step in your profile edit user journey in TrustFrameworkExtensions.xml</span></span>

<span data-ttu-id="52357-149">Adjon hozzá egy lépést, amely a profil szerkesztése felhasználói út után a felhasználó hitelesítése (vezénylési lépésekből 1-4 a következő XML-kódban), és a felhasználó már rendelkezik a frissített profil adatait (5. lépés).</span><span class="sxs-lookup"><span data-stu-id="52357-149">Add a step to the profile edit user journey, after the user has been authenticated (orchestration steps 1-4 in the following XML) and the user has provided the updated profile information (step 5).</span></span>

> [!NOTE]
> <span data-ttu-id="52357-150">Nincsenek a REST API-hívás helyének egy vezénylési lépés számos használhatók.</span><span class="sxs-lookup"><span data-stu-id="52357-150">There are many use cases where the REST API call can be used as an orchestration step.</span></span> <span data-ttu-id="52357-151">Az orchestration lépésként használat egy frissítést adunk ki a külső rendszerek, a felhasználó sikeresen befejezte egy feladat, például az első regisztráció után, akár egy profil frissítéséhez megőrizheti az adatok szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="52357-151">As an orchestration step, it can be used as an update to an external system after a user has successfully completed a task like first-time registration, or as a profile update to keep information synchronized.</span></span> <span data-ttu-id="52357-152">Ebben az esetben szolgál a profil szerkesztése után az alkalmazás foglalt információk révén.</span><span class="sxs-lookup"><span data-stu-id="52357-152">In this case, it's used to augment the information provided to the application after the profile edit.</span></span>

<span data-ttu-id="52357-153">Másolás a profil szerkesztése felhasználói út XML-kódot a TrustFrameworkBase.xml fájlból a TrustFrameworkExtensions.xml fájl belül a `<UserJourneys>` elemet.</span><span class="sxs-lookup"><span data-stu-id="52357-153">Copy the profile edit user journey XML code from the TrustFrameworkBase.xml file to your TrustFrameworkExtensions.xml file inside the `<UserJourneys>` element.</span></span> <span data-ttu-id="52357-154">Végezze el a módosítást, a 6. lépés.</span><span class="sxs-lookup"><span data-stu-id="52357-154">Then make the modification under step 6.</span></span>

```XML
<OrchestrationStep Order="6" Type="ClaimsExchange">
    <ClaimsExchanges>
        <ClaimsExchange Id="GetLoyaltyData" TechnicalProfileReferenceId="AzureFunctions-LookUpLoyaltyWebHook" />
    </ClaimsExchanges>
</OrchestrationStep>
```

> [!IMPORTANT]
> <span data-ttu-id="52357-155">Ha a sorrend nem egyezik meg a verziójával, győződjön meg arról, hogy lépése előtt helyezze be a kódját a `ClaimsExchange` típus `SendClaims`.</span><span class="sxs-lookup"><span data-stu-id="52357-155">If the order does not match your version, make sure that you insert the code as the step before the `ClaimsExchange` type `SendClaims`.</span></span>

<span data-ttu-id="52357-156">A felhasználó útra végső XML kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="52357-156">The final XML for the user journey should look like this:</span></span>

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
        <!-- Add a step 6 to the user journey before the JWT token is created-->
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

## <a name="step-5-add-the-claim-city-to-your-relying-party-policy-file-so-the-claim-is-sent-to-your-application"></a><span data-ttu-id="52357-157">5. lépés:, Vegye fel a jogcím `city` a függő entitás házirend fájl, a jogcím kap az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="52357-157">Step 5: Add the claim `city` to your relying party policy file so the claim is sent to your application</span></span>

<span data-ttu-id="52357-158">Szerkessze a ProfileEdit.xml függő entitásonkénti (RP) fájlt, és módosítsa a `<TechnicalProfile Id="PolicyProfile">` adja hozzá a következő elem: `<OutputClaim ClaimTypeReferenceId="city" />`.</span><span class="sxs-lookup"><span data-stu-id="52357-158">Edit your ProfileEdit.xml relying party (RP) file and modify the `<TechnicalProfile Id="PolicyProfile">` element to add the following: `<OutputClaim ClaimTypeReferenceId="city" />`.</span></span>

<span data-ttu-id="52357-159">Miután az új jogcím, a műszaki profil néz ki:</span><span class="sxs-lookup"><span data-stu-id="52357-159">After you add the new claim, the technical profile looks like this:</span></span>

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

## <a name="step-6-upload-your-changes-and-test"></a><span data-ttu-id="52357-160">6. lépés: Töltse fel a módosításokat, és tesztelése</span><span class="sxs-lookup"><span data-stu-id="52357-160">Step 6: Upload your changes and test</span></span>

<span data-ttu-id="52357-161">A házirend meglévő verzióinak felülírását.</span><span class="sxs-lookup"><span data-stu-id="52357-161">Overwrite the existing versions of the policy.</span></span>

1.  <span data-ttu-id="52357-162">(Nem kötelező:) Mentse a meglévő verziót (Letöltés) a bővítmények fájl folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="52357-162">(Optional:) Save the existing version (by downloading) of your extensions file before you proceed.</span></span> <span data-ttu-id="52357-163">Kezdeti összetettségét alacsony megtartásához, javasoljuk, hogy nem töltsön a bővítmények fájl több verziója.</span><span class="sxs-lookup"><span data-stu-id="52357-163">To keep the initial complexity low, we recommend that you do not upload multiple versions of the extensions file.</span></span>
2.  <span data-ttu-id="52357-164">(Nem kötelező:) Nevezze át az új verziót a házirend-azonosító a házirend szerkesztése fájl módosításával `PolicyId="B2C_1A_TrustFrameworkProfileEdit"`.</span><span class="sxs-lookup"><span data-stu-id="52357-164">(Optional:) Rename the new version of the policy ID for the policy edit file by changing   `PolicyId="B2C_1A_TrustFrameworkProfileEdit"`.</span></span>
3.  <span data-ttu-id="52357-165">A bővítmények-fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="52357-165">Upload the extensions file.</span></span>
4.  <span data-ttu-id="52357-166">A házirend szerkesztése RP-fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="52357-166">Upload the policy edit RP file.</span></span>
5.  <span data-ttu-id="52357-167">Használjon **Futtatás most** tesztelése a házirendet.</span><span class="sxs-lookup"><span data-stu-id="52357-167">Use **Run Now** to test the policy.</span></span> <span data-ttu-id="52357-168">Tekintse át a jogkivonatot, amely a IEF visszaadja az alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="52357-168">Review the token that the IEF returns to the application.</span></span>

<span data-ttu-id="52357-169">Ha minden megfelelően van beállítva, a jogkivonat tartalmazza az új jogcímet `city`, a értékű `Redmond`.</span><span class="sxs-lookup"><span data-stu-id="52357-169">If everything is set up correctly, the token will include the new claim `city`, with the value `Redmond`.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="52357-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="52357-170">Next steps</span></span>

[<span data-ttu-id="52357-171">A REST API-t használják a ellenőrzési lépés</span><span class="sxs-lookup"><span data-stu-id="52357-171">Use a REST API as a validation step</span></span>](active-directory-b2c-rest-api-validation-custom.md)

[<span data-ttu-id="52357-172">Módosítsa a profil szerkesztése további információkat kell gyűjteni a felhasználóktól</span><span class="sxs-lookup"><span data-stu-id="52357-172">Modify the profile edit to gather additional information from your users</span></span>](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)
