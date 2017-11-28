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
ms.openlocfilehash: cec6c6e110514a8bbe0e0780f36738ff21ae2f00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-validation-on-user-input"></a><span data-ttu-id="d529e-103">Forgatókönyv: Integrálása az Azure AD B2C felhasználói út a REST API jogcímek cseréjét, a felhasználói bevitel ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="d529e-103">Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as validation on user input</span></span>

<span data-ttu-id="d529e-104">hello identitás élmény keretrendszer (IEF) Azure Active Directory B2C alapjául szolgáló (az Azure AD B2C) lehetővé teszi, hogy a hello identitás fejlesztői toointegrate tevékenységet egy felhasználó út RESTful API-t.</span><span class="sxs-lookup"><span data-stu-id="d529e-104">hello Identity Experience Framework (IEF) that underlies Azure Active Directory B2C (Azure AD B2C) enables hello identity developer toointegrate an interaction with a RESTful API in a user journey.</span></span>  

<span data-ttu-id="d529e-105">Ez az útmutató végén hello fogja tudni toocreate az Azure AD B2C felhasználói út, amely együttműködik a RESTful-szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="d529e-105">At hello end of this walkthrough, you will be able toocreate an Azure AD B2C user journey that interacts with RESTful services.</span></span>

<span data-ttu-id="d529e-106">hello IEF adatok jogcímekben adatokat küld és fogad vissza a jogcímeket.</span><span class="sxs-lookup"><span data-stu-id="d529e-106">hello IEF sends data in claims and receives data back in claims.</span></span> <span data-ttu-id="d529e-107">hello API való együttműködéshez hello:</span><span class="sxs-lookup"><span data-stu-id="d529e-107">hello interaction with hello API:</span></span>

- <span data-ttu-id="d529e-108">A REST API-jogcímek exchange vagy egy érvényességi profilt, amely egy vezénylési lépés belül történik tervezhető.</span><span class="sxs-lookup"><span data-stu-id="d529e-108">Can be designed as a REST API claims exchange or as a validation profile, which happens inside an orchestration step.</span></span>
- <span data-ttu-id="d529e-109">Általában érvényesíti a bejövő hello felhasználói adatokat.</span><span class="sxs-lookup"><span data-stu-id="d529e-109">Typically validates input from hello user.</span></span> <span data-ttu-id="d529e-110">Hello felhasználói hello értéket elutasítása esetén hello felhasználó próbálja újra tooenter hello lehetőség tooreturn hibaüzenet egy érvényes értéket.</span><span class="sxs-lookup"><span data-stu-id="d529e-110">If hello value from hello user is rejected, hello user can try again tooenter a valid value with hello opportunity tooreturn an error message.</span></span>

<span data-ttu-id="d529e-111">Is kialakíthat egy vezénylési lépés hello interakció.</span><span class="sxs-lookup"><span data-stu-id="d529e-111">You can also design hello interaction as an orchestration step.</span></span> <span data-ttu-id="d529e-112">További információkért lásd: [forgatókönyv: integrálja a REST API-t cseréjét használják az Azure AD B2C felhasználói út egy vezénylési lépés a jogcímek](active-directory-b2c-rest-api-step-custom.md).</span><span class="sxs-lookup"><span data-stu-id="d529e-112">For more information, see [Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as an orchestration step](active-directory-b2c-rest-api-step-custom.md).</span></span>

<span data-ttu-id="d529e-113">Például hello érvényesítési profil használjuk hello profil szerkesztése felhasználói út hello alapszintű felügyeleticsomag-fájl ProfileEdit.xml.</span><span class="sxs-lookup"><span data-stu-id="d529e-113">For hello validation profile example, we will use hello profile edit user journey in hello starter pack file ProfileEdit.xml.</span></span>

<span data-ttu-id="d529e-114">Által megadott hello felhasználói hello-profil szerkesztése nem része egy kizárási listát ellenőrizni tudja, hogy hello nevét.</span><span class="sxs-lookup"><span data-stu-id="d529e-114">We can verify that hello name provided by hello user in hello profile edit is not part of an exclusion list.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d529e-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d529e-115">Prerequisites</span></span>

- <span data-ttu-id="d529e-116">Az Azure AD B2C bérlő konfigurált toocomplete sign-Close-Up/bejelentkezés, a helyi fiók [bevezetés](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="d529e-116">An Azure AD B2C tenant configured toocomplete a local account sign-up/sign-in, as described in [Getting started](active-directory-b2c-get-started-custom.md).</span></span>
- <span data-ttu-id="d529e-117">A REST API végpont toointeract együtt.</span><span class="sxs-lookup"><span data-stu-id="d529e-117">A REST API endpoint toointeract with.</span></span> <span data-ttu-id="d529e-118">A forgatókönyv azt létrehoztunk egy bemutató webhelyet nevű [WingTipGames](https://wingtipgamesb2c.azurewebsites.net/) egy REST API-szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="d529e-118">For this walkthrough, we've set up a demo site called [WingTipGames](https://wingtipgamesb2c.azurewebsites.net/) with a REST API service.</span></span>

## <a name="step-1-prepare-hello-rest-api-function"></a><span data-ttu-id="d529e-119">1. lépés: Felkészülés hello REST API-függvénye</span><span class="sxs-lookup"><span data-stu-id="d529e-119">Step 1: Prepare hello REST API function</span></span>

> [!NOTE]
> <span data-ttu-id="d529e-120">A REST API-függvények telepítése Ez a cikk hello hatókörén kívül esik.</span><span class="sxs-lookup"><span data-stu-id="d529e-120">Setup of REST API functions is outside hello scope of this article.</span></span> <span data-ttu-id="d529e-121">[Az Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) biztosít egy kiváló eszközkészlet toocreate RESTful-szolgáltatásokat hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="d529e-121">[Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) provides an excellent toolkit toocreate RESTful services in hello cloud.</span></span>

<span data-ttu-id="d529e-122">Létrehoztunk egy Azure függvény, amely fogad egy jogcímet, akkor várja `playerTag`.</span><span class="sxs-lookup"><span data-stu-id="d529e-122">We have created an Azure function that receives a claim that it expects as `playerTag`.</span></span> <span data-ttu-id="d529e-123">hello függvény ellenőrzi, hogy létezik-e ezt a kérelmet.</span><span class="sxs-lookup"><span data-stu-id="d529e-123">hello function validates whether this claim exists.</span></span> <span data-ttu-id="d529e-124">Van-e hozzáférési hello teljes Azure-függvény kód [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).</span><span class="sxs-lookup"><span data-stu-id="d529e-124">You can access hello complete Azure function code in [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).</span></span>

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
      userMessage = $"hello player tag '{requestContentAsJObject.playerTag}' is already used."
    },
    new JsonMediaTypeFormatter(),
    "application/json");
}

return request.CreateResponse(HttpStatusCode.OK);
```

<span data-ttu-id="d529e-125">hello IEF vár hello `userMessage` jogcím adott hello Azure függvényt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="d529e-125">hello IEF expects hello `userMessage` claim that hello Azure function returns.</span></span> <span data-ttu-id="d529e-126">A jogcím fog érzékelni karakterlánc toohello felhasználóként, ha hello érvényesítése sikertelen, például amikor egy 409 ütközés állapot eredmény abban az esetben példa megelőző hello.</span><span class="sxs-lookup"><span data-stu-id="d529e-126">This claim will be presented as a string toohello user if hello validation fails, such as when a 409 conflict status is returned in hello preceding example.</span></span>

## <a name="step-2-configure-hello-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworkextensionsxml-file"></a><span data-ttu-id="d529e-127">2. lépés: Hello RESTful API jogcímek az exchange konfigurálása a TrustFrameworkExtensions.xml fájlban műszaki profil</span><span class="sxs-lookup"><span data-stu-id="d529e-127">Step 2: Configure hello RESTful API claims exchange as a technical profile in your TrustFrameworkExtensions.xml file</span></span>

<span data-ttu-id="d529e-128">Egy technikai profil beállítás hello teljes hello Exchange hello RESTful szolgáltatás a szükséges.</span><span class="sxs-lookup"><span data-stu-id="d529e-128">A technical profile is hello full configuration of hello exchange desired with hello RESTful service.</span></span> <span data-ttu-id="d529e-129">Nyissa meg hello TrustFrameworkExtensions.xml fájlt, és adja hozzá a következő XML-részletet belül hello hello `<ClaimsProviders>` elemet.</span><span class="sxs-lookup"><span data-stu-id="d529e-129">Open hello TrustFrameworkExtensions.xml file and add hello following XML snippet inside hello `<ClaimsProviders>` element.</span></span>

> [!NOTE]
> <span data-ttu-id="d529e-130">A következő XML-RESTful szolgáltató hello `Version=1.0.0.0` hello protokollként leírása.</span><span class="sxs-lookup"><span data-stu-id="d529e-130">In hello following XML, RESTful provider `Version=1.0.0.0` is described as hello protocol.</span></span> <span data-ttu-id="d529e-131">Tekinti azokat hello függvény hello külső szolgáltatással együtt fog működni.</span><span class="sxs-lookup"><span data-stu-id="d529e-131">Consider it as hello function that will interact with hello external service.</span></span> <!-- TODO: A full definition of hello schema can be found...link tooRESTful Provider schema definition>-->

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

<span data-ttu-id="d529e-132">Hello `InputClaims` elem definiálja hello küldi hello IEF toohello REST-szolgáltatást a jogcímeket.</span><span class="sxs-lookup"><span data-stu-id="d529e-132">hello `InputClaims` element defines hello claims that will be sent from hello IEF toohello REST service.</span></span> <span data-ttu-id="d529e-133">Ebben a példában hello hello jogcím tartalmát `givenName` küld a többi szolgáltatás toohello `playerTag`.</span><span class="sxs-lookup"><span data-stu-id="d529e-133">In this example, hello contents of hello claim `givenName` will be sent toohello REST service as `playerTag`.</span></span> <span data-ttu-id="d529e-134">Ebben a példában a nem várt IEF hello vissza jogcímek.</span><span class="sxs-lookup"><span data-stu-id="d529e-134">In this example, hello IEF does not expect claims back.</span></span> <span data-ttu-id="d529e-135">Ehelyett hello REST-szolgáltatást és a fogadott hello állapotkódok alapján tevékenységéért válaszára megvárja.</span><span class="sxs-lookup"><span data-stu-id="d529e-135">Instead, it waits for a response from hello REST service and acts based on hello status codes that it receives.</span></span>

## <a name="step-3-include-hello-restful-service-claims-exchange-in-self-asserted-technical-profile-where-you-want-toovalidate-hello-user-input"></a><span data-ttu-id="d529e-136">3. lépés: Hello RESTful szolgáltatás jogcímek exchange szerepeltetni kívánt toovalidate hello felhasználói bevitel önálló szükséges műszaki profil</span><span class="sxs-lookup"><span data-stu-id="d529e-136">Step 3: Include hello RESTful service claims exchange in self-asserted technical profile where you want toovalidate hello user input</span></span>

<span data-ttu-id="d529e-137">hello leggyakoribb hello ellenőrzési lépés használata a felhasználó hello interakció.</span><span class="sxs-lookup"><span data-stu-id="d529e-137">hello most common use of hello validation step is in hello interaction with a user.</span></span> <span data-ttu-id="d529e-138">Bemeneti várt tooprovide hello felhasználó esetén az összes kapcsolati vannak *önálló magas műszaki profilok*.</span><span class="sxs-lookup"><span data-stu-id="d529e-138">All interactions where hello user is expected tooprovide input are *self-asserted technical profiles*.</span></span> <span data-ttu-id="d529e-139">Ehhez a példához hello érvényesítési toohello önkiszolgáló Asserted ProfileUpdate műszaki profil adjuk hozzá.</span><span class="sxs-lookup"><span data-stu-id="d529e-139">For this example, we will add hello validation toohello Self-Asserted-ProfileUpdate technical profile.</span></span> <span data-ttu-id="d529e-140">Ez az hello műszaki profil függő entitásonkénti (RP) házirendfájl hello `Profile Edit` használja.</span><span class="sxs-lookup"><span data-stu-id="d529e-140">This is hello technical profile that hello relying party (RP) policy file `Profile Edit` uses.</span></span>

<span data-ttu-id="d529e-141">tooadd hello jogcímek exchange toohello önálló magas műszaki profil:</span><span class="sxs-lookup"><span data-stu-id="d529e-141">tooadd hello claims exchange toohello self-asserted technical profile:</span></span>

1. <span data-ttu-id="d529e-142">Nyissa meg a hello TrustFrameworkBase.xml fájlt, és keressen a `<TechnicalProfile Id="SelfAsserted-ProfileUpdate">`.</span><span class="sxs-lookup"><span data-stu-id="d529e-142">Open hello TrustFrameworkBase.xml file and search for `<TechnicalProfile Id="SelfAsserted-ProfileUpdate">`.</span></span>
2. <span data-ttu-id="d529e-143">Ellenőrizze a műszaki profil hello konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="d529e-143">Review hello configuration of this technical profile.</span></span> <span data-ttu-id="d529e-144">Figyelje meg, hogyan hello exchange hello felhasználóval nevezünk hello felhasználó (a bemeneti jogcímek) a rendszer kéri, de hello önálló szükséges szolgáltató (kimeneti jogcímek) ból várhatóan jogcímeket.</span><span class="sxs-lookup"><span data-stu-id="d529e-144">Observe how hello exchange with hello user is defined as claims that will be asked of hello user (input claims) and claims that will be expected back from hello self-asserted provider (output claims).</span></span>
3. <span data-ttu-id="d529e-145">Keresse meg `TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate`, és figyelje meg, hogy ez a profil hívja meg, az orchestration 6. lépésében `<UserJourney Id="ProfileEdit">`.</span><span class="sxs-lookup"><span data-stu-id="d529e-145">Search for `TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate`, and notice that this profile is invoked as orchestration step 6 of `<UserJourney Id="ProfileEdit">`.</span></span>

## <a name="step-4-upload-and-test-hello-profile-edit-rp-policy-file"></a><span data-ttu-id="d529e-146">4. lépés: Töltse fel, és hello profil szerkesztése RP házirend fájl tesztelése</span><span class="sxs-lookup"><span data-stu-id="d529e-146">Step 4: Upload and test hello profile edit RP policy file</span></span>

1. <span data-ttu-id="d529e-147">Töltse fel a hello hello TrustFrameworkExtensions.xml fájl új verziója.</span><span class="sxs-lookup"><span data-stu-id="d529e-147">Upload hello new version of hello TrustFrameworkExtensions.xml file.</span></span>
2. <span data-ttu-id="d529e-148">Használjon **futtatása most** tootest hello-profil szerkesztése a függő Entitás házirend fájlja.</span><span class="sxs-lookup"><span data-stu-id="d529e-148">Use **Run now** tootest hello profile edit RP policy file.</span></span>
3. <span data-ttu-id="d529e-149">Tesztelje hello érvényesítési egyik hello meglévő nevét (például mcvinny) megadásával hello **Utónév** mező.</span><span class="sxs-lookup"><span data-stu-id="d529e-149">Test hello validation by providing one of hello existing names (for example, mcvinny) in hello **Given Name** field.</span></span> <span data-ttu-id="d529e-150">Ha minden megfelelően van beállítva, kell kapnia egy üzenet, amely értesíti hello felhasználói hello player címke már használatban van.</span><span class="sxs-lookup"><span data-stu-id="d529e-150">If everything is set up correctly, you should receive a message that notifies hello user that hello player tag is already used.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d529e-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d529e-151">Next steps</span></span>

[<span data-ttu-id="d529e-152">Hello profil szerkesztése és felhasználói regisztrációs toogather további információt a felhasználók a módosítása</span><span class="sxs-lookup"><span data-stu-id="d529e-152">Modify hello profile edit and user registration toogather additional information from your users</span></span>](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)

[<span data-ttu-id="d529e-153">Forgatókönyv: Integrálása az Azure AD B2C felhasználói út a REST API jogcímek cseréjét egy vezénylési lépés</span><span class="sxs-lookup"><span data-stu-id="d529e-153">Walkthrough: Integrate REST API claims exchanges in your Azure AD B2C user journey as an orchestration step</span></span>](active-directory-b2c-rest-api-step-custom.md)
