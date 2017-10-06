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
# <a name="walkthrough-integrate-rest-api-claims-exchanges-in-your-azure-ad-b2c-user-journey-as-validation-on-user-input"></a>Forgatókönyv: Integrálása az Azure AD B2C felhasználói út a REST API jogcímek cseréjét, a felhasználói bevitel ellenőrzése

hello identitás élmény keretrendszer (IEF) Azure Active Directory B2C alapjául szolgáló (az Azure AD B2C) lehetővé teszi, hogy a hello identitás fejlesztői toointegrate tevékenységet egy felhasználó út RESTful API-t.  

Ez az útmutató végén hello fogja tudni toocreate az Azure AD B2C felhasználói út, amely együttműködik a RESTful-szolgáltatásokat.

hello IEF adatok jogcímekben adatokat küld és fogad vissza a jogcímeket. hello API való együttműködéshez hello:

- A REST API-jogcímek exchange vagy egy érvényességi profilt, amely egy vezénylési lépés belül történik tervezhető.
- Általában érvényesíti a bejövő hello felhasználói adatokat. Hello felhasználói hello értéket elutasítása esetén hello felhasználó próbálja újra tooenter hello lehetőség tooreturn hibaüzenet egy érvényes értéket.

Is kialakíthat egy vezénylési lépés hello interakció. További információkért lásd: [forgatókönyv: integrálja a REST API-t cseréjét használják az Azure AD B2C felhasználói út egy vezénylési lépés a jogcímek](active-directory-b2c-rest-api-step-custom.md).

Például hello érvényesítési profil használjuk hello profil szerkesztése felhasználói út hello alapszintű felügyeleticsomag-fájl ProfileEdit.xml.

Által megadott hello felhasználói hello-profil szerkesztése nem része egy kizárási listát ellenőrizni tudja, hogy hello nevét.

## <a name="prerequisites"></a>Előfeltételek

- Az Azure AD B2C bérlő konfigurált toocomplete sign-Close-Up/bejelentkezés, a helyi fiók [bevezetés](active-directory-b2c-get-started-custom.md).
- A REST API végpont toointeract együtt. A forgatókönyv azt létrehoztunk egy bemutató webhelyet nevű [WingTipGames](https://wingtipgamesb2c.azurewebsites.net/) egy REST API-szolgáltatással.

## <a name="step-1-prepare-hello-rest-api-function"></a>1. lépés: Felkészülés hello REST API-függvénye

> [!NOTE]
> A REST API-függvények telepítése Ez a cikk hello hatókörén kívül esik. [Az Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-reference) biztosít egy kiváló eszközkészlet toocreate RESTful-szolgáltatásokat hello felhőben.

Létrehoztunk egy Azure függvény, amely fogad egy jogcímet, akkor várja `playerTag`. hello függvény ellenőrzi, hogy létezik-e ezt a kérelmet. Van-e hozzáférési hello teljes Azure-függvény kód [GitHub](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/AzureFunctionsSamples).

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

hello IEF vár hello `userMessage` jogcím adott hello Azure függvényt ad vissza. A jogcím fog érzékelni karakterlánc toohello felhasználóként, ha hello érvényesítése sikertelen, például amikor egy 409 ütközés állapot eredmény abban az esetben példa megelőző hello.

## <a name="step-2-configure-hello-restful-api-claims-exchange-as-a-technical-profile-in-your-trustframeworkextensionsxml-file"></a>2. lépés: Hello RESTful API jogcímek az exchange konfigurálása a TrustFrameworkExtensions.xml fájlban műszaki profil

Egy technikai profil beállítás hello teljes hello Exchange hello RESTful szolgáltatás a szükséges. Nyissa meg hello TrustFrameworkExtensions.xml fájlt, és adja hozzá a következő XML-részletet belül hello hello `<ClaimsProviders>` elemet.

> [!NOTE]
> A következő XML-RESTful szolgáltató hello `Version=1.0.0.0` hello protokollként leírása. Tekinti azokat hello függvény hello külső szolgáltatással együtt fog működni. <!-- TODO: A full definition of hello schema can be found...link tooRESTful Provider schema definition>-->

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

Hello `InputClaims` elem definiálja hello küldi hello IEF toohello REST-szolgáltatást a jogcímeket. Ebben a példában hello hello jogcím tartalmát `givenName` küld a többi szolgáltatás toohello `playerTag`. Ebben a példában a nem várt IEF hello vissza jogcímek. Ehelyett hello REST-szolgáltatást és a fogadott hello állapotkódok alapján tevékenységéért válaszára megvárja.

## <a name="step-3-include-hello-restful-service-claims-exchange-in-self-asserted-technical-profile-where-you-want-toovalidate-hello-user-input"></a>3. lépés: Hello RESTful szolgáltatás jogcímek exchange szerepeltetni kívánt toovalidate hello felhasználói bevitel önálló szükséges műszaki profil

hello leggyakoribb hello ellenőrzési lépés használata a felhasználó hello interakció. Bemeneti várt tooprovide hello felhasználó esetén az összes kapcsolati vannak *önálló magas műszaki profilok*. Ehhez a példához hello érvényesítési toohello önkiszolgáló Asserted ProfileUpdate műszaki profil adjuk hozzá. Ez az hello műszaki profil függő entitásonkénti (RP) házirendfájl hello `Profile Edit` használja.

tooadd hello jogcímek exchange toohello önálló magas műszaki profil:

1. Nyissa meg a hello TrustFrameworkBase.xml fájlt, és keressen a `<TechnicalProfile Id="SelfAsserted-ProfileUpdate">`.
2. Ellenőrizze a műszaki profil hello konfigurációját. Figyelje meg, hogyan hello exchange hello felhasználóval nevezünk hello felhasználó (a bemeneti jogcímek) a rendszer kéri, de hello önálló szükséges szolgáltató (kimeneti jogcímek) ból várhatóan jogcímeket.
3. Keresse meg `TechnicalProfileReferenceId="SelfAsserted-ProfileUpdate`, és figyelje meg, hogy ez a profil hívja meg, az orchestration 6. lépésében `<UserJourney Id="ProfileEdit">`.

## <a name="step-4-upload-and-test-hello-profile-edit-rp-policy-file"></a>4. lépés: Töltse fel, és hello profil szerkesztése RP házirend fájl tesztelése

1. Töltse fel a hello hello TrustFrameworkExtensions.xml fájl új verziója.
2. Használjon **futtatása most** tootest hello-profil szerkesztése a függő Entitás házirend fájlja.
3. Tesztelje hello érvényesítési egyik hello meglévő nevét (például mcvinny) megadásával hello **Utónév** mező. Ha minden megfelelően van beállítva, kell kapnia egy üzenet, amely értesíti hello felhasználói hello player címke már használatban van.

## <a name="next-steps"></a>Következő lépések

[Hello profil szerkesztése és felhasználói regisztrációs toogather további információt a felhasználók a módosítása](active-directory-b2c-create-custom-attributes-profile-edit-custom.md)

[Forgatókönyv: Integrálása az Azure AD B2C felhasználói út a REST API jogcímek cseréjét egy vezénylési lépés](active-directory-b2c-rest-api-step-custom.md)
