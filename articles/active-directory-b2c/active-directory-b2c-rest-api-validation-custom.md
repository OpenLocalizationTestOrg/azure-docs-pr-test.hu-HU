---
title: "Az Azure Active Directory B2C: REST API-jogcímek cseréjét, érvényesítési |} Microsoft Docs"
description: "Témakör: Azure Active Directory B2C egyéni házirendek"
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
ms.openlocfilehash: eb44a0d2234c9ee3801d8b3a1655d877aa2f4fef
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-validation-on-user-input"></a><span data-ttu-id="7533c-103">Forgatókönyv: Integrálása az Azure AD B2C felhasználói út a REST API jogcímek cseréjét, a felhasználói bevitel ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="7533c-103">Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as validation on user input</span></span>

<span data-ttu-id="7533c-104">Az identitás élmény keretrendszer (IEF) Azure Active Directory B2C alapjául szolgáló (az Azure AD B2C) lehetővé teszi, hogy a tevékenységet egy felhasználó út RESTful API-t integrálja a identitás fejlesztő.</span><span class="sxs-lookup"><span data-stu-id="7533c-104">The Identity Experience Framework (IEF) that underlies Azure Active Directory B2C (Azure AD B2C) enables the identity developer to integrate an interaction with a RESTful API in a user journey.</span></span>  

<span data-ttu-id="7533c-105">Ez az útmutató végén lesz létrehozása az Azure AD B2C felhasználói út, amely együttműködik a RESTful-szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="7533c-105">At the end of this walkthrough, you will be able to create an Azure AD B2C user journey that interacts with RESTful services.</span></span>

<span data-ttu-id="7533c-106">A IEF adatok jogcímekben adatokat küld és fogad vissza a jogcímeket.</span><span class="sxs-lookup"><span data-stu-id="7533c-106">The IEF sends data in claims and receives data back in claims.</span></span> <span data-ttu-id="7533c-107">A interakció API-val:</span><span class="sxs-lookup"><span data-stu-id="7533c-107">The interaction with the API:</span></span>

- <span data-ttu-id="7533c-108">A REST API-jogcímek exchange vagy egy érvényességi profilt, amely egy vezénylési lépés belül történik tervezhető.</span><span class="sxs-lookup"><span data-stu-id="7533c-108">Can be designed as a REST API claims exchange or as a validation profile, which happens inside an orchestration step.</span></span>
- <span data-ttu-id="7533c-109">Általában érvényesíti felhasználói beavatkozást.</span><span class="sxs-lookup"><span data-stu-id="7533c-109">Typically validates input from the user.</span></span> <span data-ttu-id="7533c-110">Az érték a felhasználó elutasítása esetén a felhasználó megpróbálja újra adjon meg egy érvényes értéket a számára, hogy egy hibaüzenetet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="7533c-110">If the value from the user is rejected, the user can try again to enter a valid value with the opportunity to return an error message.</span></span>

<span data-ttu-id="7533c-111">Is kialakíthat egy vezénylési lépés a közötti.</span><span class="sxs-lookup"><span data-stu-id="7533c-111">You can also design the interaction as an orchestration step.</span></span> <span data-ttu-id="7533c-112">További információkért lásd: [forgatókönyv: integrálja a REST API-t cseréjét használják az Azure AD B2C felhasználói út egy vezénylési lépés a jogcímek](active-directory-b2c-rest-api-step-custom.md).</span><span class="sxs-lookup"><span data-stu-id="7533c-112">For more information, see [Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as an orchestration step](active-directory-b2c-rest-api-step-custom.md).</span></span>

<span data-ttu-id="7533c-113">Például az érvényességi profilt a profil szerkesztése felhasználói út az alapszintű csomag fájlban ProfileEdit.xml használjuk.</span><span class="sxs-lookup"><span data-stu-id="7533c-113">For the validation profile example, we will use the profile edit user journey in the starter pack file ProfileEdit.xml.</span></span>

<span data-ttu-id="7533c-114">Azt is ellenőrizze, hogy a profil szerkesztése a felhasználó által megadott név nem része egy kizárási listát.</span><span class="sxs-lookup"><span data-stu-id="7533c-114">We can verify that the name provided by the user in the profile edit is not part of an exclusion list.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7533c-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7533c-115">Prerequisites</span></span>

- <span data-ttu-id="7533c-116">Egy helyi fiókot sign-Close-Up/sign-in befejezéséhez, a konfigurált Azure AD B2C-bérlő [bevezetés](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="7533c-116">An Azure AD B2C tenant configured to complete a local account sign-up/sign-in, as described in [Getting started](active-directory-b2c-get-started-custom.md).</span></span>
- <span data-ttu-id="7533c-117">REST API-végpont kommunikál.</span><span class="sxs-lookup"><span data-stu-id="7533c-117">A REST API endpoint to interact with.</span></span> <span data-ttu-id="7533c-118">A forgatókönyv azt létrehoztunk egy bemutató webhelyet nevű [WingTipGames](https://wingtipgamesb2c.azurewebsites.net/) egy REST API-szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="7533c-118">For this walkthrough, we've set up a demo site called [WingTipGames](https://wingtipgamesb2c.azurewebsites.net/) with a REST API service.</span></span>

## <a name="step-1-prepare-the-rest-api-function"></a><span data-ttu-id="7533c-119">1. lépés: Felkészülés a REST API-függvénye</span><span class="sxs-lookup"><span data-stu-id="7533c-119">Step 1: Prepare the REST API function</span></span>

> [!NOTE]
> <span data-ttu-id="7533c-120">Ez a cikk hatókörén kívül található a REST API-függvények telepítése.</span><span class="sxs-lookup"><span data-stu-id="7533c-120">Setup of REST API functions is outside the scope of this article.</span></span> <span data-ttu-id="7533c-121">[Az Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) biztosít egy kiváló eszközkészlet RESTful szolgáltatás létrehozásához a felhőben.</span><span class="sxs-lookup"><span data-stu-id="7533c-121">[Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) provides an excellent toolkit to create RESTful services in the cloud.</span></span>

<span data-ttu-id="7533c-122">Létrehoztunk egy Azure függvény, amely fogad egy jogcímet, akkor várja `playerTag`.</span><span class="sxs-lookup"><span data-stu-id="7533c-122">We have created an Azure function that receives a claim that it expects as `playerTag`.</span></span> <span data-ttu-id="7533c-123">A függvény ellenőrzi, hogy létezik-e ezt a kérelmet.</span><span class="sxs-lookup"><span data-stu-id="7533c-123">The function validates whether this claim exists.</span></span> <span data-ttu-id="7533c-124">Érheti el a teljes Azure funkciókódot [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).</span><span class="sxs-lookup"><span data-stu-id="7533c-124">You can access the complete Azure function code in [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).</span></span>

```csharp
if (requestContentAsJObject.playerTag == null)
{
  return request.CreateResponse(HttpStatusCode.BadRequest);
}

var playerTag = ((string) requestContentAsJObject.playerTag).ToLower();

if (playerTag == "mcvinny" || playerTag == "msgates123" || playerTag == "revcottonmarcus")
{
  return request.CreateResponse<ResponseContent>(
    HttpStatusCode.Conflict,
    new ResponseContent
    {
      version = "1.0.0",
      status = (int) HttpStatusCode.Conflict,
      userMessage = $"The player tag '{requestContentAsJObject.playerTag}' is already used."
    },
    new JsonMediaTypeFormatter(),
    "application/json");
}

return request.CreateResponse(HttpStatusCode.OK);
```

<span data-ttu-id="7533c-125">A IEF vár a `userMessage` jogcímet, amely az Azure függvényt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="7533c-125">The IEF expects the `userMessage` claim that the Azure function returns.</span></span> <span data-ttu-id="7533c-126">A jogcím fog érzékelni karakterláncként a felhasználót, ha az érvényesítés meghiúsul, például amikor egy 409 ütközés állapot eredmény abban az esetben az előző példában.</span><span class="sxs-lookup"><span data-stu-id="7533c-126">This claim will be presented as a string to the user if the validation fails, such as when a 409 conflict status is returned in the preceding example.</span></span>

## <a name="step-2-configure-the-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworkextensionsxml-file"></a><span data-ttu-id="7533c-127">2. lépés: A RESTful API jogcímek az exchange konfigurálása a TrustFrameworkExtensions.xml fájlban műszaki profil</span><span class="sxs-lookup"><span data-stu-id="7533c-127">Step 2: Configure the RESTful API claims exchange as a technical profile in your TrustFrameworkExtensions.xml file</span></span>

<span data-ttu-id="7533c-128">Műszaki profilt a RESTful szolgáltatás szükséges az exchange teljes konfigurációjának.</span><span class="sxs-lookup"><span data-stu-id="7533c-128">A technical profile is the full configuration of the exchange desired with the RESTful service.</span></span> <span data-ttu-id="7533c-129">Nyissa meg a TrustFrameworkExtensions.xml fájlt, és adja hozzá a következő XML-részletet belül a `<ClaimsProviders>` elemet.</span><span class="sxs-lookup"><span data-stu-id="7533c-129">Open the TrustFrameworkExtensions.xml file and add the following XML snippet inside the `<ClaimsProviders>` element.</span></span>

> [!NOTE]
> <span data-ttu-id="7533c-130">A következő XML-kódban, RESTful szolgáltató `Version=1.0.0.0` protokoll leírását.</span><span class="sxs-lookup"><span data-stu-id="7533c-130">In the following XML, RESTful provider `Version=1.0.0.0` is described as the protocol.</span></span> <span data-ttu-id="7533c-131">Vegye figyelembe azt, a függvény, amely a külső szolgáltatással együtt fog működni.</span><span class="sxs-lookup"><span data-stu-id="7533c-131">Consider it as the function that will interact with the external service.</span></span> <!-- TODO: A full definition of the schema can be found...link to RESTful Provider schema definition>-->

```xml
<ClaimsProvider>
    <DisplayName>REST APIs</DisplayName>
    <TechnicalProfiles>
        <TechnicalProfile Id="AzureFunctions-CheckPlayerTagWebHook">
            <DisplayName>Check Player Tag Web Hook Azure Function</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <Metadata>
                <Item Key="ServiceUrl">https://wingtipb2cfuncs.azurewebsites.net/api/CheckPlayerTagWebHook?code=L/05YRSpojU0nECzM4Tp3LjBiA2ZGh3kTwwp1OVV7m0SelnvlRVLCg==</Item>
                <Item Key="AuthenticationType">None</Item>
                <Item Key="SendClaimsIn">Body</Item>
            </Metadata>
            <InputClaims>
                <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="playerTag" />
            </InputClaims>
            <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
        <TechnicalProfile Id="SelfAsserted-ProfileUpdate">
            <ValidationTechnicalProfiles>
                <ValidationTechnicalProfile ReferenceId="AzureFunctions-CheckPlayerTagWebHook" />
            </ValidationTechnicalProfiles>
        </TechnicalProfile>
    </TechnicalProfiles>
</ClaimsProvider>
```

<span data-ttu-id="7533c-132">A `InputClaims` elem definiálja a jogcímeket, amely a REST-szolgáltatást a IEF a kapnak.</span><span class="sxs-lookup"><span data-stu-id="7533c-132">The `InputClaims` element defines the claims that will be sent from the IEF to the REST service.</span></span> <span data-ttu-id="7533c-133">Ebben a példában a jogcím tartalmát `givenName` kapnak a többi szolgáltatás `playerTag`.</span><span class="sxs-lookup"><span data-stu-id="7533c-133">In this example, the contents of the claim `givenName` will be sent to the REST service as `playerTag`.</span></span> <span data-ttu-id="7533c-134">Ebben a példában a IEF nem várt jogcímek vissza.</span><span class="sxs-lookup"><span data-stu-id="7533c-134">In this example, the IEF does not expect claims back.</span></span> <span data-ttu-id="7533c-135">Ehelyett a REST-szolgáltatást és a fogadott állapotkódok alapján tevékenységéért válaszára megvárja.</span><span class="sxs-lookup"><span data-stu-id="7533c-135">Instead, it waits for a response from the REST service and acts based on the status codes that it receives.</span></span>

## <a name="step-3-include-the-restful-service-claims-exchange-in-self-asserted-technical-profile-where-you-want-to-validate-the-user-input"></a><span data-ttu-id="7533c-136">3. lépés: A RESTful szolgáltatás jogcímek exchange foglalandó önálló szükséges műszaki profil, ahol szeretné érvényesíteni a felhasználói bevitel</span><span class="sxs-lookup"><span data-stu-id="7533c-136">Step 3: Include the RESTful service claims exchange in self-asserted technical profile where you want to validate the user input</span></span>

<span data-ttu-id="7533c-137">A leggyakoribb ellenőrzési lépés használata az egy felhasználói beavatkozást.</span><span class="sxs-lookup"><span data-stu-id="7533c-137">The most common use of the validation step is in the interaction with a user.</span></span> <span data-ttu-id="7533c-138">Minden interakció, ahol a felhasználó kellene adnia valamit vannak *önálló magas műszaki profilok*.</span><span class="sxs-lookup"><span data-stu-id="7533c-138">All interactions where the user is expected to provide input are *self-asserted technical profiles*.</span></span> <span data-ttu-id="7533c-139">Ebben a példában az érvényesítési önkiszolgáló Asserted ProfileUpdate műszaki profilt felveszi azt.</span><span class="sxs-lookup"><span data-stu-id="7533c-139">For this example, we will add the validation to the Self-Asserted-ProfileUpdate technical profile.</span></span> <span data-ttu-id="7533c-140">Ez a műszaki profilja, a függő entitásonkénti (RP) házirendfájl `Profile Edit` használja.</span><span class="sxs-lookup"><span data-stu-id="7533c-140">This is the technical profile that the relying party (RP) policy file `Profile Edit` uses.</span></span>

<span data-ttu-id="7533c-141">A jogcímek exchange hozzáadása a saját szükséges műszaki profil:</span><span class="sxs-lookup"><span data-stu-id="7533c-141">To add the claims exchange to the self-asserted technical profile:</span></span>

1. <span data-ttu-id="7533c-142">Nyissa meg a TrustFrameworkBase.xml fájlt, és keresse meg `<TechnicalProfile Id="SelfAsserted-ProfileUpdate">`.</span><span class="sxs-lookup"><span data-stu-id="7533c-142">Open the TrustFrameworkBase.xml file and search for `<TechnicalProfile Id="SelfAsserted-ProfileUpdate">`.</span></span>
2. <span data-ttu-id="7533c-143">Ellenőrizze a műszaki profil konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="7533c-143">Review the configuration of this technical profile.</span></span> <span data-ttu-id="7533c-144">Figyelje meg, hogy az exchange-ben a felhasználó nevezünk kérni fogja annak a felhasználónak (a bemeneti jogcímek), de az önálló szükséges szolgáltatót (kimeneti jogcímek) ból várhatóan jogcímeket.</span><span class="sxs-lookup"><span data-stu-id="7533c-144">Observe how the exchange with the user is defined as claims that will be asked of the user (input claims) and claims that will be expected back from the self-asserted provider (output claims).</span></span>
3. <span data-ttu-id="7533c-145">Keresse meg `TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate`, és figyelje meg, hogy ez a profil hívja meg, az orchestration 6. lépésében `<UserJourney Id="ProfileEdit">`.</span><span class="sxs-lookup"><span data-stu-id="7533c-145">Search for `TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate`, and notice that this profile is invoked as orchestration step 6 of `<UserJourney Id="ProfileEdit">`.</span></span>

## <a name="step-4-upload-and-test-the-profile-edit-rp-policy-file"></a><span data-ttu-id="7533c-146">4. lépés: Töltse fel, és a profil szerkesztése RP házirend fájl tesztelése</span><span class="sxs-lookup"><span data-stu-id="7533c-146">Step 4: Upload and test the profile edit RP policy file</span></span>

1. <span data-ttu-id="7533c-147">Töltse fel a TrustFrameworkExtensions.xml fájl új verzióját.</span><span class="sxs-lookup"><span data-stu-id="7533c-147">Upload the new version of the TrustFrameworkExtensions.xml file.</span></span>
2. <span data-ttu-id="7533c-148">Használjon **futtatása most** a profil szerkesztése RP házirendfájl teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="7533c-148">Use **Run now** to test the profile edit RP policy file.</span></span>
3. <span data-ttu-id="7533c-149">Tesztelje az érvényesítési egyik meglévő nevét (például mcvinny) biztosításával a **Utónév** mező.</span><span class="sxs-lookup"><span data-stu-id="7533c-149">Test the validation by providing one of the existing names (for example, mcvinny) in the **Given Name** field.</span></span> <span data-ttu-id="7533c-150">Ha minden megfelelően van beállítva, kell kapnia egy üzenetet, amely értesíti a felhasználót, hogy a player címke már használatban van.</span><span class="sxs-lookup"><span data-stu-id="7533c-150">If everything is set up correctly, you should receive a message that notifies the user that the player tag is already used.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7533c-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7533c-151">Next steps</span></span>

[<span data-ttu-id="7533c-152">Módosítsa a profil szerkesztése és felhasználói regisztráció gyűjtsön további információt a felhasználók</span><span class="sxs-lookup"><span data-stu-id="7533c-152">Modify the profile edit and user registration to gather additional information from your users</span></span>](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)

[<span data-ttu-id="7533c-153">Forgatókönyv: Integrálása az Azure AD B2C felhasználói út a REST API jogcímek cseréjét egy vezénylési lépés</span><span class="sxs-lookup"><span data-stu-id="7533c-153">Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as an orchestration step</span></span>](active-directory-b2c-rest-api-step-custom.md)
