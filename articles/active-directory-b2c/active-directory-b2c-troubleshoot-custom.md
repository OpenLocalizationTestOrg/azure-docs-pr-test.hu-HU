---
title: "Az Application Insights egyéni házirendek – hibaelhárítás az Azure AD B2C |} Microsoft Docs"
description: "a telepítő az Application Insights nyomon követésére, a végrehajtás, egyéni házirendek hogyan"
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
ms.openlocfilehash: 8c79df33cd5f04f490e2cc6372f7e8ac1c4d9bbe
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-b2c-collecting-logs"></a><span data-ttu-id="11b25-103">Az Azure Active Directory B2C: Naplógyűjtés időtartamát</span><span class="sxs-lookup"><span data-stu-id="11b25-103">Azure Active Directory B2C: Collecting Logs</span></span>

<span data-ttu-id="11b25-104">Ez a cikk lépéseit a naplógyűjtés időtartamát az Azure AD B2C-vel, hogy diagnosztizálhatja a problémákat az egyéni házirendeknek.</span><span class="sxs-lookup"><span data-stu-id="11b25-104">This article provides steps for collecting logs from Azure AD B2C so that you can diagnose problems with your custom policies.</span></span>

>[!NOTE]
><span data-ttu-id="11b25-105">Az itt leírt részletes tevékenységi naplóit célja jelenleg **csak** a egyéni házirendek fejlesztésének segítése érdekében.</span><span class="sxs-lookup"><span data-stu-id="11b25-105">Currently, the detailed activity logs described here are designed **ONLY** to aid in development of custom policies.</span></span> <span data-ttu-id="11b25-106">Éles környezetben fejlesztői mód nem használható.</span><span class="sxs-lookup"><span data-stu-id="11b25-106">Do not use development mode  in production.</span></span>  <span data-ttu-id="11b25-107">Naplók gyűjtése és az identitás-szolgáltatóktól származó a fejlesztés során küldött összes jogcímet.</span><span class="sxs-lookup"><span data-stu-id="11b25-107">Logs collect all claims sent to and from the identity providers during development.</span></span>  <span data-ttu-id="11b25-108">Ha éles környezetben használt, a fejlesztői felelősséget PII (közvetlenül a Microsoftnak azonosításra alkalmas adatokat) gyűjtése a saját App Insights naplóban.</span><span class="sxs-lookup"><span data-stu-id="11b25-108">If used in production, the developer assumes responsibility for PII (Privately Identifiable Information) collected in the App Insights log that they own.</span></span>  <span data-ttu-id="11b25-109">Ezek a részletes naplók csak gyűjtött, ha a házirend a **fejlesztői mód**.</span><span class="sxs-lookup"><span data-stu-id="11b25-109">These detailed logs are only collected when the policy is placed on **DEVELOPMENT MODE**.</span></span>


## <a name="use-application-insights"></a><span data-ttu-id="11b25-110">Az Application Insights használata</span><span class="sxs-lookup"><span data-stu-id="11b25-110">Use Application Insights</span></span>

<span data-ttu-id="11b25-111">Az Azure AD B2C támogatja egy szolgáltatást, hogy az Application insights részére.</span><span class="sxs-lookup"><span data-stu-id="11b25-111">Azure AD B2C supports a feature for sending data to Application Insights.</span></span>  <span data-ttu-id="11b25-112">Az Application Insights kivételek diagnosztizálhatja és alkalmazás teljesítményproblémák megjelenítése lehetőséget biztosít.</span><span class="sxs-lookup"><span data-stu-id="11b25-112">Application Insights provides a way to diagnose exceptions and visualize application performance issues.</span></span>

### <a name="setup-application-insights"></a><span data-ttu-id="11b25-113">Az Application Insights beállítása</span><span class="sxs-lookup"><span data-stu-id="11b25-113">Setup Application Insights</span></span>

1. <span data-ttu-id="11b25-114">Nyissa meg az [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="11b25-114">Go to the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="11b25-115">Hogy a bérlő az Azure-előfizetéséhez (nem az Azure AD B2C-bérlő).</span><span class="sxs-lookup"><span data-stu-id="11b25-115">Ensure you are in the tenant with your Azure subscription (not your Azure AD B2C tenant).</span></span>
1. <span data-ttu-id="11b25-116">Kattintson a **+ új** a bal oldali navigációs menü.</span><span class="sxs-lookup"><span data-stu-id="11b25-116">Click **+ New** in the left-hand navigation menu.</span></span>
1. <span data-ttu-id="11b25-117">Keresse meg és jelölje ki **Application Insights**, majd kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="11b25-117">Search for and select **Application Insights**, then click **Create**.</span></span>
1. <span data-ttu-id="11b25-118">Töltse ki az űrlapot, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="11b25-118">Complete the form and click **Create**.</span></span> <span data-ttu-id="11b25-119">Válassza ki **általános** a a **alkalmazástípus**.</span><span class="sxs-lookup"><span data-stu-id="11b25-119">Select **General** for the **Application Type**.</span></span>
1. <span data-ttu-id="11b25-120">Ha az erőforrás létrejött, nyissa meg az Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="11b25-120">Once the resource has been created, open the Application Insights resource.</span></span>
1. <span data-ttu-id="11b25-121">Található **tulajdonságok** a bal oldali menüben, kattintson rá.</span><span class="sxs-lookup"><span data-stu-id="11b25-121">Find **Properties** in the left-menu, and click on it.</span></span>
1. <span data-ttu-id="11b25-122">Másolás a **Instrumentation kulcs** , és mentse a következő szakasz.</span><span class="sxs-lookup"><span data-stu-id="11b25-122">Copy the **Instrumentation Key** and save it for the next section.</span></span>

### <a name="set-up-the-custom-policy"></a><span data-ttu-id="11b25-123">Az egyéni házirend beállítása</span><span class="sxs-lookup"><span data-stu-id="11b25-123">Set up the custom policy</span></span>

1. <span data-ttu-id="11b25-124">Nyissa meg a függő Entitás fájlt (például SignUpOrSignin.xml).</span><span class="sxs-lookup"><span data-stu-id="11b25-124">Open the RP file (for example, SignUpOrSignin.xml).</span></span>
1. <span data-ttu-id="11b25-125">A következő attribútumok hozzáadása a `<TrustFrameworkPolicy>` elem:</span><span class="sxs-lookup"><span data-stu-id="11b25-125">Add the following attributes to the `<TrustFrameworkPolicy>` element:</span></span>

  ```XML
  DeploymentMode="Development"
  UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
  ```

1. <span data-ttu-id="11b25-126">Ha már nem létezik, adja hozzá a gyermekcsomópontja `<UserJourneyBehaviors>` számára a `<RelyingParty>` csomópont.</span><span class="sxs-lookup"><span data-stu-id="11b25-126">If it doesn't exist already, add a child node `<UserJourneyBehaviors>` to the `<RelyingParty>` node.</span></span> <span data-ttu-id="11b25-127">Kell elhelyezni után azonnal a`<DefaultUserJourney ReferenceId="YourPolicyName" />`</span><span class="sxs-lookup"><span data-stu-id="11b25-127">It must be located immediately after the `<DefaultUserJourney ReferenceId="YourPolicyName" />`</span></span>
2. <span data-ttu-id="11b25-128">Adja hozzá a következő csomópont gyermekeként a `<UserJourneyBehaviors>` elemet.</span><span class="sxs-lookup"><span data-stu-id="11b25-128">Add the following node as a child of the `<UserJourneyBehaviors>` element.</span></span> <span data-ttu-id="11b25-129">Győződjön meg arról, hogy `{Your Application Insights Key}` rendelkező a **Instrumentation kulcs** az Application Insights az előző szakaszban beszerzett.</span><span class="sxs-lookup"><span data-stu-id="11b25-129">Make sure to replace `{Your Application Insights Key}` with the **Instrumentation Key** that you obtained from Application Insights in the previous section.</span></span>

  ```XML
  <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
  ```

  * <span data-ttu-id="11b25-130">`DeveloperMode="true"`be van állítva ApplicationInsights keresztül a feldolgozási sorban, a telemetria jó elősegítésére fejlesztési, de a nagy mennyiségük korlátozott.</span><span class="sxs-lookup"><span data-stu-id="11b25-130">`DeveloperMode="true"` tells ApplicationInsights to expedite the telemetry through the processing pipeline, good for development, but constrained at high volumes.</span></span>
  * <span data-ttu-id="11b25-131">`ClientEnabled="true"`elküldi a ApplicationInsights ügyféloldali parancsprogram nyomon követése lap megtekintése és ügyféloldali hibák (nem kötelező).</span><span class="sxs-lookup"><span data-stu-id="11b25-131">`ClientEnabled="true"` sends the ApplicationInsights client-side script for tracking page view and client-side errors (not needed).</span></span>
  * <span data-ttu-id="11b25-132">`ServerEnabled="true"`a meglévő UserJourneyRecorder JSON egyéni eseményként küld az Application Insights.</span><span class="sxs-lookup"><span data-stu-id="11b25-132">`ServerEnabled="true"` sends the existing UserJourneyRecorder JSON as a custom event to Application Insights.</span></span>
<span data-ttu-id="11b25-133">Minta:</span><span class="sxs-lookup"><span data-stu-id="11b25-133">Sample:</span></span>

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

3. <span data-ttu-id="11b25-134">Töltse fel a házirendet.</span><span class="sxs-lookup"><span data-stu-id="11b25-134">Upload the policy.</span></span>

### <a name="see-the-logs-in-application-insights"></a><span data-ttu-id="11b25-135">Tekintse meg a naplókat az Application Insightsban</span><span class="sxs-lookup"><span data-stu-id="11b25-135">See the logs in Application Insights</span></span>

>[!NOTE]
> <span data-ttu-id="11b25-136">Nincs a rövid késleltetés (kevesebb mint öt perc), új naplófájlok az Application Insights megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="11b25-136">There is a short delay (less than five minutes) before you can see new logs in Application Insights.</span></span>

1. <span data-ttu-id="11b25-137">Nyissa meg az Application Insights-erőforrás a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="11b25-137">Open the Application Insights resource that you created in the [Azure portal](https://portal.azure.com).</span></span>
1. <span data-ttu-id="11b25-138">Az a **áttekintése** menüben kattintson a **Analytics**.</span><span class="sxs-lookup"><span data-stu-id="11b25-138">In the **Overview** menu, click on **Analytics**.</span></span>
1. <span data-ttu-id="11b25-139">Az Application Insightsban új lap megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="11b25-139">Open a new tab in Application Insights.</span></span>
1. <span data-ttu-id="11b25-140">Ez egy lista lekérdezések segítségével tekintse meg a naplókat</span><span class="sxs-lookup"><span data-stu-id="11b25-140">Here is a list of queries you can use to see the logs</span></span>

| <span data-ttu-id="11b25-141">Lekérdezés</span><span class="sxs-lookup"><span data-stu-id="11b25-141">Query</span></span> | <span data-ttu-id="11b25-142">Leírás</span><span class="sxs-lookup"><span data-stu-id="11b25-142">Description</span></span> |
|---------------------|--------------------|
<span data-ttu-id="11b25-143">nyomkövetések</span><span class="sxs-lookup"><span data-stu-id="11b25-143">traces</span></span> | <span data-ttu-id="11b25-144">Az összes Azure AD B2C által létrehozott naplók</span><span class="sxs-lookup"><span data-stu-id="11b25-144">See all of the logs generated by Azure AD B2C</span></span> |
<span data-ttu-id="11b25-145">nyomok \\</span><span class="sxs-lookup"><span data-stu-id="11b25-145">traces \\</span></span>| <span data-ttu-id="11b25-146">Ha időbélyeg > ago(1d)</span><span class="sxs-lookup"><span data-stu-id="11b25-146">where timestamp > ago(1d)</span></span> | <span data-ttu-id="11b25-147">Az elmúlt nap során az Azure AD B2C által létrehozott naplók számú</span><span class="sxs-lookup"><span data-stu-id="11b25-147">See all of the logs generated by Azure AD B2C for the last day</span></span>

<span data-ttu-id="11b25-148">A bejegyzések hosszú lehet.</span><span class="sxs-lookup"><span data-stu-id="11b25-148">The entries may be long.</span></span>  <span data-ttu-id="11b25-149">Exportálás CSV-FÁJLBA a részletes bemutatása.</span><span class="sxs-lookup"><span data-stu-id="11b25-149">Export to CSV for a closer look.</span></span>

<span data-ttu-id="11b25-150">Az elemzés eszközzel kapcsolatos részletesebb [Itt](https://docs.microsoft.com/azure/application-insights/app-insights-analytics).</span><span class="sxs-lookup"><span data-stu-id="11b25-150">You can learn more about the Analytics tool [here](https://docs.microsoft.com/azure/application-insights/app-insights-analytics).</span></span>

>[!NOTE]
><span data-ttu-id="11b25-151">A Közösség dolgozott egy felhasználó út megjelenítő segítségével a fejlesztők identitás.</span><span class="sxs-lookup"><span data-stu-id="11b25-151">The community has developed a user journey viewer to help identity developers.</span></span>  <span data-ttu-id="11b25-152">Nem Microsoft által támogatott és elérhetővé tegyen szigorúan-van.</span><span class="sxs-lookup"><span data-stu-id="11b25-152">It is not supported by Microsoft and made available strictly as-is.</span></span>  <span data-ttu-id="11b25-153">Olvassa be az Application Insights-példány, és a felhasználó well-struktúra áttekintést nyújt a út események.</span><span class="sxs-lookup"><span data-stu-id="11b25-153">It reads from your Application Insights instance and provides a well-structure view of the user journey events.</span></span>  <span data-ttu-id="11b25-154">Szerezze be a forráskódot, és telepítheti saját megoldásban.</span><span class="sxs-lookup"><span data-stu-id="11b25-154">You obtain the source code and deploy it in your own solution.</span></span>

>[!NOTE]
><span data-ttu-id="11b25-155">Az itt leírt részletes tevékenységi naplóit célja jelenleg **csak** a egyéni házirendek fejlesztésének segítése érdekében.</span><span class="sxs-lookup"><span data-stu-id="11b25-155">Currently, the detailed activity logs described here are designed **ONLY** to aid in development of custom policies.</span></span> <span data-ttu-id="11b25-156">Éles környezetben fejlesztői mód nem használható.</span><span class="sxs-lookup"><span data-stu-id="11b25-156">Do not use development mode in production.</span></span>  <span data-ttu-id="11b25-157">Naplók gyűjtése és az identitás-szolgáltatóktól származó a fejlesztés során küldött összes jogcímet.</span><span class="sxs-lookup"><span data-stu-id="11b25-157">Logs collect all claims sent to and from the identity providers during development.</span></span>  <span data-ttu-id="11b25-158">Ha éles környezetben használt, a fejlesztői felelősséget PII (közvetlenül a Microsoftnak azonosításra alkalmas adatokat) gyűjtése a saját App Insights naplóban.</span><span class="sxs-lookup"><span data-stu-id="11b25-158">If used in production, the developer assumes responsibility for PII (Privately Identifiable Information) collected in the App Insights log that they own.</span></span>  <span data-ttu-id="11b25-159">Ezek a részletes naplók csak gyűjtött, ha a házirend a **fejlesztői mód**.</span><span class="sxs-lookup"><span data-stu-id="11b25-159">These detailed logs are only collected when the policy is placed on **DEVELOPMENT MODE**.</span></span>

[<span data-ttu-id="11b25-160">Github-tárházban nem támogatott egyéni házirend mintákat és a kapcsolódó eszközök</span><span class="sxs-lookup"><span data-stu-id="11b25-160">Github Repository for Unsupported Custom Policy Samples and Related tools</span></span>](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies)



## <a name="next-steps"></a><span data-ttu-id="11b25-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="11b25-161">Next Steps</span></span>

<span data-ttu-id="11b25-162">Az Application Insights segítségével megtudhatja, hogyan képes biztosítani a saját identitás működik alapul szolgáló B2C identitás élmény keretében észlel az adatokba.</span><span class="sxs-lookup"><span data-stu-id="11b25-162">Explore the data in Application Insights to help you understand how the Identity Experience Framework underlying B2C works to deliver your own identity experiences.</span></span>
