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
# <a name="azure-active-directory-b2c-collecting-logs"></a><span data-ttu-id="161b3-103">Az Azure Active Directory B2C: Naplógyűjtés időtartamát</span><span class="sxs-lookup"><span data-stu-id="161b3-103">Azure Active Directory B2C: Collecting Logs</span></span>

<span data-ttu-id="161b3-104">Ez a cikk lépéseit a naplógyűjtés időtartamát az Azure AD B2C-vel, hogy diagnosztizálhatja a problémákat az egyéni házirendeknek.</span><span class="sxs-lookup"><span data-stu-id="161b3-104">This article provides steps for collecting logs from Azure AD B2C so that you can diagnose problems with your custom policies.</span></span>

>[!NOTE]
><span data-ttu-id="161b3-105">Jelenleg hello itt leírt részletes tevékenységi naplóit tervezett **csak** tooaid egyéni házirendek fejlesztésében.</span><span class="sxs-lookup"><span data-stu-id="161b3-105">Currently, hello detailed activity logs described here are designed **ONLY** tooaid in development of custom policies.</span></span> <span data-ttu-id="161b3-106">Éles környezetben fejlesztői mód nem használható.</span><span class="sxs-lookup"><span data-stu-id="161b3-106">Do not use development mode  in production.</span></span>  <span data-ttu-id="161b3-107">Naplók gyűjtése tooand hello identitás-szolgáltatóktól a fejlesztés során küldött összes jogcímet.</span><span class="sxs-lookup"><span data-stu-id="161b3-107">Logs collect all claims sent tooand from hello identity providers during development.</span></span>  <span data-ttu-id="161b3-108">Ha éles környezetben használt, hello fejlesztői felelősséget PII (közvetlenül a Microsoftnak azonosításra alkalmas adatokat) gyűjtött saját hello App Insights naplóban.</span><span class="sxs-lookup"><span data-stu-id="161b3-108">If used in production, hello developer assumes responsibility for PII (Privately Identifiable Information) collected in hello App Insights log that they own.</span></span>  <span data-ttu-id="161b3-109">A részletes naplókat a rendszer csak gyűjti hello házirend elhelyezésekor a **fejlesztői mód**.</span><span class="sxs-lookup"><span data-stu-id="161b3-109">These detailed logs are only collected when hello policy is placed on **DEVELOPMENT MODE**.</span></span>


## <a name="use-application-insights"></a><span data-ttu-id="161b3-110">Az Application Insights használata</span><span class="sxs-lookup"><span data-stu-id="161b3-110">Use Application Insights</span></span>

<span data-ttu-id="161b3-111">Az Azure AD B2C az adatok tooApplication Insights szolgáltatás támogatja.</span><span class="sxs-lookup"><span data-stu-id="161b3-111">Azure AD B2C supports a feature for sending data tooApplication Insights.</span></span>  <span data-ttu-id="161b3-112">Az Application Insights tartalmaz egy módja toodiagnose kivételeket, és megjelenítheti az alkalmazás teljesítményproblémákat.</span><span class="sxs-lookup"><span data-stu-id="161b3-112">Application Insights provides a way toodiagnose exceptions and visualize application performance issues.</span></span>

### <a name="setup-application-insights"></a><span data-ttu-id="161b3-113">Az Application Insights beállítása</span><span class="sxs-lookup"><span data-stu-id="161b3-113">Setup Application Insights</span></span>

1. <span data-ttu-id="161b3-114">Nyissa meg toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="161b3-114">Go toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="161b3-115">Győződjön meg arról, Azure-előfizetéséhez (nem az Azure AD B2C-bérlő) hello bérlői szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="161b3-115">Ensure you are in hello tenant with your Azure subscription (not your Azure AD B2C tenant).</span></span>
1. <span data-ttu-id="161b3-116">Kattintson a **+ új** hello bal oldali navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="161b3-116">Click **+ New** in hello left-hand navigation menu.</span></span>
1. <span data-ttu-id="161b3-117">Keresse meg és jelölje ki **Application Insights**, majd kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="161b3-117">Search for and select **Application Insights**, then click **Create**.</span></span>
1. <span data-ttu-id="161b3-118">Hello űrlap kitöltése és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="161b3-118">Complete hello form and click **Create**.</span></span> <span data-ttu-id="161b3-119">Válassza ki **általános** a hello **alkalmazástípus**.</span><span class="sxs-lookup"><span data-stu-id="161b3-119">Select **General** for hello **Application Type**.</span></span>
1. <span data-ttu-id="161b3-120">Hello erőforrás létrehozása után, nyissa meg a hello Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="161b3-120">Once hello resource has been created, open hello Application Insights resource.</span></span>
1. <span data-ttu-id="161b3-121">Található **tulajdonságok** a hello a bal oldali menüben, és kattintson rá.</span><span class="sxs-lookup"><span data-stu-id="161b3-121">Find **Properties** in hello left-menu, and click on it.</span></span>
1. <span data-ttu-id="161b3-122">Másolás hello **Instrumentation kulcs** , és mentse a következő szakaszban hello.</span><span class="sxs-lookup"><span data-stu-id="161b3-122">Copy hello **Instrumentation Key** and save it for hello next section.</span></span>

### <a name="set-up-hello-custom-policy"></a><span data-ttu-id="161b3-123">Hello egyéni házirend beállítása</span><span class="sxs-lookup"><span data-stu-id="161b3-123">Set up hello custom policy</span></span>

1. <span data-ttu-id="161b3-124">Nyissa meg a hello RP fájlt (például SignUpOrSignin.xml).</span><span class="sxs-lookup"><span data-stu-id="161b3-124">Open hello RP file (for example, SignUpOrSignin.xml).</span></span>
1. <span data-ttu-id="161b3-125">Adja hozzá a következő attribútumok toohello hello `<TrustFrameworkPolicy>` elem:</span><span class="sxs-lookup"><span data-stu-id="161b3-125">Add hello following attributes toohello `<TrustFrameworkPolicy>` element:</span></span>

  ```XML
  DeploymentMode="Development"
  UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
  ```

1. <span data-ttu-id="161b3-126">Ha már nem létezik, adja hozzá a gyermekcsomópontja `<UserJourneyBehaviors>` toohello `<RelyingParty>` csomópont.</span><span class="sxs-lookup"><span data-stu-id="161b3-126">If it doesn't exist already, add a child node `<UserJourneyBehaviors>` toohello `<RelyingParty>` node.</span></span> <span data-ttu-id="161b3-127">Hello után azonnal helyen kell lennie`<DefaultUserJourney ReferenceId="YourPolicyName" />`</span><span class="sxs-lookup"><span data-stu-id="161b3-127">It must be located immediately after hello `<DefaultUserJourney ReferenceId="YourPolicyName" />`</span></span>
2. <span data-ttu-id="161b3-128">Adja hozzá a következő csomópont hello gyermekeként hello `<UserJourneyBehaviors>` elemet.</span><span class="sxs-lookup"><span data-stu-id="161b3-128">Add hello following node as a child of hello `<UserJourneyBehaviors>` element.</span></span> <span data-ttu-id="161b3-129">Győződjön meg arról, hogy tooreplace `{Your Application Insights Key}` a hello **Instrumentation kulcs** az Application Insights hello előző szakaszban beszerzett.</span><span class="sxs-lookup"><span data-stu-id="161b3-129">Make sure tooreplace `{Your Application Insights Key}` with hello **Instrumentation Key** that you obtained from Application Insights in hello previous section.</span></span>

  ```XML
  <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
  ```

  * <span data-ttu-id="161b3-130">`DeveloperMode="true"`közli a ApplicationInsights tooexpedite hello telemetriai keresztül hello feldolgozási sorban, hasznos megoldás fejlesztési, de a nagy mennyiségük korlátozott.</span><span class="sxs-lookup"><span data-stu-id="161b3-130">`DeveloperMode="true"` tells ApplicationInsights tooexpedite hello telemetry through hello processing pipeline, good for development, but constrained at high volumes.</span></span>
  * <span data-ttu-id="161b3-131">`ClientEnabled="true"`küldi hello ApplicationInsights ügyféloldali parancsprogram nyomon követése lap megtekintése és ügyféloldali hibák (nem kötelező).</span><span class="sxs-lookup"><span data-stu-id="161b3-131">`ClientEnabled="true"` sends hello ApplicationInsights client-side script for tracking page view and client-side errors (not needed).</span></span>
  * <span data-ttu-id="161b3-132">`ServerEnabled="true"`küld egy egyéni esemény tooApplication Insights, meglévő UserJourneyRecorder JSON hello.</span><span class="sxs-lookup"><span data-stu-id="161b3-132">`ServerEnabled="true"` sends hello existing UserJourneyRecorder JSON as a custom event tooApplication Insights.</span></span>
<span data-ttu-id="161b3-133">Minta:</span><span class="sxs-lookup"><span data-stu-id="161b3-133">Sample:</span></span>

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

3. <span data-ttu-id="161b3-134">Töltse fel a hello házirend.</span><span class="sxs-lookup"><span data-stu-id="161b3-134">Upload hello policy.</span></span>

### <a name="see-hello-logs-in-application-insights"></a><span data-ttu-id="161b3-135">Lásd: hello naplózza az Application Insightsban</span><span class="sxs-lookup"><span data-stu-id="161b3-135">See hello logs in Application Insights</span></span>

>[!NOTE]
> <span data-ttu-id="161b3-136">Nincs a rövid késleltetés (kevesebb mint öt perc), új naplófájlok az Application Insights megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="161b3-136">There is a short delay (less than five minutes) before you can see new logs in Application Insights.</span></span>

1. <span data-ttu-id="161b3-137">Nyissa meg a hello hello Application Insights-erőforrás [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="161b3-137">Open hello Application Insights resource that you created in hello [Azure portal](https://portal.azure.com).</span></span>
1. <span data-ttu-id="161b3-138">A hello **áttekintése** menüben kattintson a **Analytics**.</span><span class="sxs-lookup"><span data-stu-id="161b3-138">In hello **Overview** menu, click on **Analytics**.</span></span>
1. <span data-ttu-id="161b3-139">Az Application Insightsban új lap megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="161b3-139">Open a new tab in Application Insights.</span></span>
1. <span data-ttu-id="161b3-140">Ez egy lista, használhatja a toosee hello naplók lekérdezések</span><span class="sxs-lookup"><span data-stu-id="161b3-140">Here is a list of queries you can use toosee hello logs</span></span>

| <span data-ttu-id="161b3-141">Lekérdezés</span><span class="sxs-lookup"><span data-stu-id="161b3-141">Query</span></span> | <span data-ttu-id="161b3-142">Leírás</span><span class="sxs-lookup"><span data-stu-id="161b3-142">Description</span></span> |
|---------------------|--------------------|
<span data-ttu-id="161b3-143">nyomkövetések</span><span class="sxs-lookup"><span data-stu-id="161b3-143">traces</span></span> | <span data-ttu-id="161b3-144">Tekintse meg az Azure AD B2C által generált hello naplók</span><span class="sxs-lookup"><span data-stu-id="161b3-144">See all of hello logs generated by Azure AD B2C</span></span> |
<span data-ttu-id="161b3-145">nyomok \\</span><span class="sxs-lookup"><span data-stu-id="161b3-145">traces \\</span></span>| <span data-ttu-id="161b3-146">Ha időbélyeg > ago(1d)</span><span class="sxs-lookup"><span data-stu-id="161b3-146">where timestamp > ago(1d)</span></span> | <span data-ttu-id="161b3-147">Összes hello által létrehozott naplók az Azure AD B2C hello az utolsó napja</span><span class="sxs-lookup"><span data-stu-id="161b3-147">See all of hello logs generated by Azure AD B2C for hello last day</span></span>

<span data-ttu-id="161b3-148">lehet, hogy hosszú hello bejegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="161b3-148">hello entries may be long.</span></span>  <span data-ttu-id="161b3-149">Exportálás tooCSV a részletes bemutatása.</span><span class="sxs-lookup"><span data-stu-id="161b3-149">Export tooCSV for a closer look.</span></span>

<span data-ttu-id="161b3-150">Hello Analytics eszközzel kapcsolatos részletesebb [Itt](https://docs.microsoft.com/azure/application-insights/app-insights-analytics).</span><span class="sxs-lookup"><span data-stu-id="161b3-150">You can learn more about hello Analytics tool [here](https://docs.microsoft.com/azure/application-insights/app-insights-analytics).</span></span>

>[!NOTE]
><span data-ttu-id="161b3-151">egy felhasználó út viewer toohelp identitás fejlesztők hello közösségi fejlesztett ki.</span><span class="sxs-lookup"><span data-stu-id="161b3-151">hello community has developed a user journey viewer toohelp identity developers.</span></span>  <span data-ttu-id="161b3-152">Nem Microsoft által támogatott és elérhetővé tegyen szigorúan-van.</span><span class="sxs-lookup"><span data-stu-id="161b3-152">It is not supported by Microsoft and made available strictly as-is.</span></span>  <span data-ttu-id="161b3-153">Olvassa be az Application Insights-példány, és hello felhasználói út események well-struktúra nézetét jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="161b3-153">It reads from your Application Insights instance and provides a well-structure view of hello user journey events.</span></span>  <span data-ttu-id="161b3-154">Szerezze be a hello forráskódját, és telepítheti saját megoldásban.</span><span class="sxs-lookup"><span data-stu-id="161b3-154">You obtain hello source code and deploy it in your own solution.</span></span>

>[!NOTE]
><span data-ttu-id="161b3-155">Jelenleg hello itt leírt részletes tevékenységi naplóit tervezett **csak** tooaid egyéni házirendek fejlesztésében.</span><span class="sxs-lookup"><span data-stu-id="161b3-155">Currently, hello detailed activity logs described here are designed **ONLY** tooaid in development of custom policies.</span></span> <span data-ttu-id="161b3-156">Éles környezetben fejlesztői mód nem használható.</span><span class="sxs-lookup"><span data-stu-id="161b3-156">Do not use development mode in production.</span></span>  <span data-ttu-id="161b3-157">Naplók gyűjtése tooand hello identitás-szolgáltatóktól a fejlesztés során küldött összes jogcímet.</span><span class="sxs-lookup"><span data-stu-id="161b3-157">Logs collect all claims sent tooand from hello identity providers during development.</span></span>  <span data-ttu-id="161b3-158">Ha éles környezetben használt, hello fejlesztői felelősséget PII (közvetlenül a Microsoftnak azonosításra alkalmas adatokat) gyűjtött saját hello App Insights naplóban.</span><span class="sxs-lookup"><span data-stu-id="161b3-158">If used in production, hello developer assumes responsibility for PII (Privately Identifiable Information) collected in hello App Insights log that they own.</span></span>  <span data-ttu-id="161b3-159">A részletes naplókat a rendszer csak gyűjti hello házirend elhelyezésekor a **fejlesztői mód**.</span><span class="sxs-lookup"><span data-stu-id="161b3-159">These detailed logs are only collected when hello policy is placed on **DEVELOPMENT MODE**.</span></span>

[<span data-ttu-id="161b3-160">Github-tárházban nem támogatott egyéni házirend mintákat és a kapcsolódó eszközök</span><span class="sxs-lookup"><span data-stu-id="161b3-160">Github Repository for Unsupported Custom Policy Samples and Related tools</span></span>](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies)



## <a name="next-steps"></a><span data-ttu-id="161b3-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="161b3-161">Next Steps</span></span>

<span data-ttu-id="161b3-162">Az Application Insights toohelp tisztában hello adatokba hogyan hello identitás élmény keretrendszer alapul szolgáló B2C észlel a saját identitás toodeliver működik.</span><span class="sxs-lookup"><span data-stu-id="161b3-162">Explore hello data in Application Insights toohelp you understand how hello Identity Experience Framework underlying B2C works toodeliver your own identity experiences.</span></span>
