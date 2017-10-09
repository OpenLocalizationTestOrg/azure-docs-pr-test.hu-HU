---
title: "Application Insights tootroubleshoot egyéni házirendek – az Azure AD B2C |} Microsoft Docs"
description: "Hogyan toosetup Application Insights tootrace hello végrehajtás, egyéni házirendek"
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: 658c597e-3787-465e-b377-26aebc94e46d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: saeda
ms.openlocfilehash: c02d7178512c7f9e022385371c3effd4f8cb7726
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-collecting-logs"></a>Az Azure Active Directory B2C: Naplógyűjtés időtartamát

Ez a cikk lépéseit a naplógyűjtés időtartamát az Azure AD B2C-vel, hogy diagnosztizálhatja a problémákat az egyéni házirendeknek.

>[!NOTE]
>Jelenleg hello itt leírt részletes tevékenységi naplóit tervezett **csak** tooaid egyéni házirendek fejlesztésében. Éles környezetben fejlesztői mód nem használható.  Naplók gyűjtése tooand hello identitás-szolgáltatóktól a fejlesztés során küldött összes jogcímet.  Ha éles környezetben használt, hello fejlesztői felelősséget PII (közvetlenül a Microsoftnak azonosításra alkalmas adatokat) gyűjtött saját hello App Insights naplóban.  A részletes naplókat a rendszer csak gyűjti hello házirend elhelyezésekor a **fejlesztői mód**.


## <a name="use-application-insights"></a>Az Application Insights használata

Az Azure AD B2C az adatok tooApplication Insights szolgáltatás támogatja.  Az Application Insights tartalmaz egy módja toodiagnose kivételeket, és megjelenítheti az alkalmazás teljesítményproblémákat.

### <a name="setup-application-insights"></a>Az Application Insights beállítása

1. Nyissa meg toohello [Azure-portálon](https://portal.azure.com). Győződjön meg arról, Azure-előfizetéséhez (nem az Azure AD B2C-bérlő) hello bérlői szerepelnek.
1. Kattintson a **+ új** hello bal oldali navigációs menü.
1. Keresse meg és jelölje ki **Application Insights**, majd kattintson a **létrehozása**.
1. Hello űrlap kitöltése és kattintson a **létrehozása**. Válassza ki **általános** a hello **alkalmazástípus**.
1. Hello erőforrás létrehozása után, nyissa meg a hello Application Insights-erőforrást.
1. Található **tulajdonságok** a hello a bal oldali menüben, és kattintson rá.
1. Másolás hello **Instrumentation kulcs** , és mentse a következő szakaszban hello.

### <a name="set-up-hello-custom-policy"></a>Hello egyéni házirend beállítása

1. Nyissa meg a hello RP fájlt (például SignUpOrSignin.xml).
1. Adja hozzá a következő attribútumok toohello hello `<TrustFrameworkPolicy>` elem:

  ```XML
  DeploymentMode="Development"
  UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
  ```

1. Ha már nem létezik, adja hozzá a gyermekcsomópontja `<UserJourneyBehaviors>` toohello `<RelyingParty>` csomópont. Hello után azonnal helyen kell lennie`<DefaultUserJourney ReferenceId="YourPolicyName" />`
2. Adja hozzá a következő csomópont hello gyermekeként hello `<UserJourneyBehaviors>` elemet. Győződjön meg arról, hogy tooreplace `{Your Application Insights Key}` a hello **Instrumentation kulcs** az Application Insights hello előző szakaszban beszerzett.

  ```XML
  <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
  ```

  * `DeveloperMode="true"`közli a ApplicationInsights tooexpedite hello telemetriai keresztül hello feldolgozási sorban, hasznos megoldás fejlesztési, de a nagy mennyiségük korlátozott.
  * `ClientEnabled="true"`küldi hello ApplicationInsights ügyféloldali parancsprogram nyomon követése lap megtekintése és ügyféloldali hibák (nem kötelező).
  * `ServerEnabled="true"`küld egy egyéni esemény tooApplication Insights, meglévő UserJourneyRecorder JSON hello.
Minta:

  ```XML
  <TrustFrameworkPolicy
    ...
    TenantId="fabrikamb2c.onmicrosoft.com"
    PolicyId="SignUpOrSignInWithAAD"
    DeploymentMode="Development"
    UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
  >
    ...
    <RelyingParty>
      <DefaultUserJourney ReferenceId="YourPolicyName" />
      <UserJourneyBehaviors>
        <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
      </UserJourneyBehaviors>
      ...
  </TrustFrameworkPolicy>
  ```

3. Töltse fel a hello házirend.

### <a name="see-hello-logs-in-application-insights"></a>Lásd: hello naplózza az Application Insightsban

>[!NOTE]
> Nincs a rövid késleltetés (kevesebb mint öt perc), új naplófájlok az Application Insights megjelenítéséhez.

1. Nyissa meg a hello hello Application Insights-erőforrás [Azure-portálon](https://portal.azure.com).
1. A hello **áttekintése** menüben kattintson a **Analytics**.
1. Az Application Insightsban új lap megnyitásához.
1. Ez egy lista, használhatja a toosee hello naplók lekérdezések

| Lekérdezés | Leírás |
|---------------------|--------------------|
nyomkövetések | Tekintse meg az Azure AD B2C által generált hello naplók |
nyomok \| Ha időbélyeg > ago(1d) | Összes hello által létrehozott naplók az Azure AD B2C hello az utolsó napja

lehet, hogy hosszú hello bejegyzéseket.  Exportálás tooCSV a részletes bemutatása.

Hello Analytics eszközzel kapcsolatos részletesebb [Itt](https://docs.microsoft.com/azure/application-insights/app-insights-analytics).

>[!NOTE]
>egy felhasználó út viewer toohelp identitás fejlesztők hello közösségi fejlesztett ki.  Nem Microsoft által támogatott és elérhetővé tegyen szigorúan-van.  Olvassa be az Application Insights-példány, és hello felhasználói út események well-struktúra nézetét jeleníti meg.  Szerezze be a hello forráskódját, és telepítheti saját megoldásban.

>[!NOTE]
>Jelenleg hello itt leírt részletes tevékenységi naplóit tervezett **csak** tooaid egyéni házirendek fejlesztésében. Éles környezetben fejlesztői mód nem használható.  Naplók gyűjtése tooand hello identitás-szolgáltatóktól a fejlesztés során küldött összes jogcímet.  Ha éles környezetben használt, hello fejlesztői felelősséget PII (közvetlenül a Microsoftnak azonosításra alkalmas adatokat) gyűjtött saját hello App Insights naplóban.  A részletes naplókat a rendszer csak gyűjti hello házirend elhelyezésekor a **fejlesztői mód**.

[Github-tárházban nem támogatott egyéni házirend mintákat és a kapcsolódó eszközök](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies)



## <a name="next-steps"></a>Következő lépések

Az Application Insights toohelp tisztában hello adatokba hogyan hello identitás élmény keretrendszer alapul szolgáló B2C észlel a saját identitás toodeliver működik.
