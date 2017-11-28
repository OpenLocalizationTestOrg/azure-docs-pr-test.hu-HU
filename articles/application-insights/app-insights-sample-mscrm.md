---
title: "aaaMicrosoft Dynamics CRM-hez és az Azure Application Insights |} Microsoft Docs"
description: "Telemetriai adatok beszerzése a Microsoft Dynamics CRM Online Application Insights segítségével. Forgatókönyv: a telepítés az első adatok, a képi megjelenítés és exportálása."
services: application-insights
documentationcenter: 
author: mazharmicrosoft
manager: carmonm
ms.assetid: 04c66338-687e-49e5-9975-be935f98f156
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/16/2017
ms.author: bwren
ms.openlocfilehash: a39398060d6553fb18a26c101f085b7d87443636
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a><span data-ttu-id="3b978-104">Forgatókönyv: Telemetriai engedélyezése a Microsoft Dynamics CRM Online Application Insights segítségével</span><span class="sxs-lookup"><span data-stu-id="3b978-104">Walkthrough: Enabling Telemetry for Microsoft Dynamics CRM Online using Application Insights</span></span>
<span data-ttu-id="3b978-105">Ez a cikk bemutatja, hogyan tooget telemetriai adatokat [Microsoft Dynamics CRM Online](https://www.dynamics.com/) használatával [Azure Application Insights](https://azure.microsoft.com/services/application-insights/).</span><span class="sxs-lookup"><span data-stu-id="3b978-105">This article shows you how tooget telemetry data from [Microsoft Dynamics CRM Online](https://www.dynamics.com/) using [Azure Application Insights](https://azure.microsoft.com/services/application-insights/).</span></span> <span data-ttu-id="3b978-106">Végigvezetjük a teljes folyamat hello Application Insights parancsfájl tooyour alkalmazás hozzáadása, adatokat és az adatok vizuális rögzítése.</span><span class="sxs-lookup"><span data-stu-id="3b978-106">We’ll walk through hello complete process of adding Application Insights script tooyour application, capturing data, and data visualization.</span></span>

> [!NOTE]
> <span data-ttu-id="3b978-107">[Keresse meg a hello megoldást](https://dynamicsandappinsights.codeplex.com/).</span><span class="sxs-lookup"><span data-stu-id="3b978-107">[Browse hello sample solution](https://dynamicsandappinsights.codeplex.com/).</span></span>
> 
> 

## <a name="add-application-insights-toonew-or-existing-crm-online-instance"></a><span data-ttu-id="3b978-108">Az Application Insights toonew vagy meglévő CRM Online példány hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3b978-108">Add Application Insights toonew or existing CRM Online instance</span></span>
<span data-ttu-id="3b978-109">toomonitor az alkalmazás Application Insights SDK tooyour alkalmazás hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="3b978-109">toomonitor your application, you add an Application Insights SDK tooyour application.</span></span> <span data-ttu-id="3b978-110">hello SDK küld telemetriai toohello [Application Insights portál](https://portal.azure.com), amelyen használja a hatékony elemzés és diagnosztikai eszközöket, vagy hello adatok toostorage exportálása.</span><span class="sxs-lookup"><span data-stu-id="3b978-110">hello SDK sends telemetry toohello [Application Insights portal](https://portal.azure.com), where you can use our powerful analysis and diagnostic tools, or export hello data toostorage.</span></span>

### <a name="create-an-application-insights-resource-in-azure"></a><span data-ttu-id="3b978-111">Az Application Insights-erőforrás létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="3b978-111">Create an Application Insights resource in Azure</span></span>
1. <span data-ttu-id="3b978-112">Első [egy fiókot a Microsoft Azure-ban](http://azure.com/pricing).</span><span class="sxs-lookup"><span data-stu-id="3b978-112">Get [an account in Microsoft Azure](http://azure.com/pricing).</span></span> 
2. <span data-ttu-id="3b978-113">Jelentkezzen be a hello [Azure-portálon](https://portal.azure.com) , és adja hozzá egy új Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="3b978-113">Sign into hello [Azure portal](https://portal.azure.com) and add a new Application Insights resource.</span></span> <span data-ttu-id="3b978-114">Ez az, ha az adatok feldolgozása és jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3b978-114">This is where your data will be processed and displayed.</span></span>
   
    ![Kattintson a +, fejlesztői szolgáltatások, az Application Insights.](./media/app-insights-sample-mscrm/01.png)
   
    <span data-ttu-id="3b978-116">Válassza ki az ASP.NET hello alkalmazás típusként.</span><span class="sxs-lookup"><span data-stu-id="3b978-116">Choose ASP.NET as hello application type.</span></span>
3. <span data-ttu-id="3b978-117">Hello első lépések lap megnyitásához, és nyissa meg a "a figyelő és diagnosztizálhatja az ügyféloldali".</span><span class="sxs-lookup"><span data-stu-id="3b978-117">Open hello Getting Started page and open "Monitor and diagnose client side".</span></span>
   
    ![A weblap beszúrásához kódrészletet](./media/app-insights-sample-mscrm/03.png)

<span data-ttu-id="3b978-119">**Tartsa nyitva, hello kódlap** közben a következő lépés egy másik böngészőablakban hello.</span><span class="sxs-lookup"><span data-stu-id="3b978-119">**Keep hello code page open** while you do hello next step in another browser window.</span></span> <span data-ttu-id="3b978-120">Hello kód hamarosan lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="3b978-120">You'll need hello code soon.</span></span> 

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a><span data-ttu-id="3b978-121">A Microsoft Dynamics CRM JavaScript webes erőforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="3b978-121">Create a JavaScript web resource in Microsoft Dynamics CRM</span></span>
1. <span data-ttu-id="3b978-122">Nyissa meg az CRM Online-példány és a bejelentkezési rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="3b978-122">Open your CRM Online instance and login with administrator privileges.</span></span>
2. <span data-ttu-id="3b978-123">Nyissa meg a Microsoft Dynamics CRM beállításait, testreszabás is szerepelt, testreszabás hello rendszer</span><span class="sxs-lookup"><span data-stu-id="3b978-123">Open Microsoft Dynamics CRM Settings, Customizations, Customize hello System</span></span>
   
    ![A Microsoft Dynamics CRM-beállítások](./media/app-insights-sample-mscrm/04.png)
   
    ![Beállítások > Testreszabás](./media/app-insights-sample-mscrm/05.png)

    ![Hello beállítást testreszabása](./media/app-insights-sample-mscrm/06.png)

1. <span data-ttu-id="3b978-127">Hozzon létre egy JavaScript-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="3b978-127">Create a JavaScript resource.</span></span>
   
    ![Az új webes erőforrás párbeszédpanel](./media/app-insights-sample-mscrm/07.png)
   
    <span data-ttu-id="3b978-129">Adjon neki egy névvel, jelölje be **parancsfájl (JScript)** és a nyitott hello szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="3b978-129">Give it a name, select **Script (JScript)** and open hello text editor.</span></span>
   
    ![Nyissa meg hello szövegszerkesztőben](./media/app-insights-sample-mscrm/08.png)
2. <span data-ttu-id="3b978-131">Az Application Insights hello kód másolása.</span><span class="sxs-lookup"><span data-stu-id="3b978-131">Copy hello code from Application Insights.</span></span> <span data-ttu-id="3b978-132">Másolás közben győződjön meg arról, hogy tooignore parancsprogramcímkéket.</span><span class="sxs-lookup"><span data-stu-id="3b978-132">While copying make sure tooignore script tags.</span></span> <span data-ttu-id="3b978-133">Tekintse meg az alábbi képernyőfelvételen:</span><span class="sxs-lookup"><span data-stu-id="3b978-133">Refer below screenshot:</span></span>
   
    ![A rendszerállapot-kulcs beállítása](./media/app-insights-sample-mscrm/09.png)
   
    <span data-ttu-id="3b978-135">hello kód hello instrumentation kulcsot, amely azonosítja az Application insights-erőforrást tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3b978-135">hello code includes hello instrumentation key that identifies your Application insights resource.</span></span>
3. <span data-ttu-id="3b978-136">Mentse, és tegye közzé.</span><span class="sxs-lookup"><span data-stu-id="3b978-136">Save and publish.</span></span>
   
    ![Mentse és közzététele](./media/app-insights-sample-mscrm/10.png)

### <a name="instrument-forms"></a><span data-ttu-id="3b978-138">Eszköz űrlapok</span><span class="sxs-lookup"><span data-stu-id="3b978-138">Instrument Forms</span></span>
1. <span data-ttu-id="3b978-139">A Microsoft CRM Online hello fiók képernyő megnyitása</span><span class="sxs-lookup"><span data-stu-id="3b978-139">In Microsoft CRM Online, open hello Account form</span></span>
   
    ![Fiók képernyő](./media/app-insights-sample-mscrm/11.png)
2. <span data-ttu-id="3b978-141">Nyissa meg a hello űrlap tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="3b978-141">Open hello form Properties</span></span>
   
    ![Űrlap tulajdonságai](./media/app-insights-sample-mscrm/12.png)
3. <span data-ttu-id="3b978-143">Hello létrehozott JavaScript webes erőforrás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3b978-143">Add hello JavaScript web resource that you created</span></span>
   
    ![Hozzáadásra szolgáló menü](./media/app-insights-sample-mscrm/13.png)
   
    ![Hello webes erőforrás hozzáadása](./media/app-insights-sample-mscrm/14.png)
4. <span data-ttu-id="3b978-146">Mentse, és tegye közzé az űrlapok testreszabásai.</span><span class="sxs-lookup"><span data-stu-id="3b978-146">Save and publish your form customizations.</span></span>

## <a name="metrics-captured"></a><span data-ttu-id="3b978-147">Rögzített metrikák</span><span class="sxs-lookup"><span data-stu-id="3b978-147">Metrics captured</span></span>
<span data-ttu-id="3b978-148">Most már készen hello űrlap a telemetria-rögzítést.</span><span class="sxs-lookup"><span data-stu-id="3b978-148">You have now set up telemetry capture for hello form.</span></span> <span data-ttu-id="3b978-149">Azokat alkalmazni, amikor adatokat küld tooyour Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="3b978-149">Whenever it is used, data will be sent tooyour Application Insights resource.</span></span>

<span data-ttu-id="3b978-150">Az alábbiakban láthatja hello adatok minták.</span><span class="sxs-lookup"><span data-stu-id="3b978-150">Here are samples of hello data that you'll see.</span></span>

#### <a name="application-health"></a><span data-ttu-id="3b978-151">Alkalmazás állapotának</span><span class="sxs-lookup"><span data-stu-id="3b978-151">Application health</span></span>
![Példa lapbetöltési idő](./media/app-insights-sample-mscrm/15.png)

![Példa lap nézetek diagram](./media/app-insights-sample-mscrm/16.png)

<span data-ttu-id="3b978-154">Böngésző kivételek:</span><span class="sxs-lookup"><span data-stu-id="3b978-154">Browser exceptions:</span></span>

![Böngésző kivételek diagram](./media/app-insights-sample-mscrm/17.png)

<span data-ttu-id="3b978-156">Kattintson a hello diagram tooget további részletek:</span><span class="sxs-lookup"><span data-stu-id="3b978-156">Click hello chart tooget more detail:</span></span>

![Kivételek listájáról](./media/app-insights-sample-mscrm/18.png)

#### <a name="usage"></a><span data-ttu-id="3b978-158">Használat</span><span class="sxs-lookup"><span data-stu-id="3b978-158">Usage</span></span>
![Felhasználók, a munkamenetek és az oldalmegtekintéseket](./media/app-insights-sample-mscrm/19.png)

![Sesion diagramok](./media/app-insights-sample-mscrm/20.png)

![Böngésző-verziók](./media/app-insights-sample-mscrm/21.png)

#### <a name="browsers"></a><span data-ttu-id="3b978-162">Böngészők</span><span class="sxs-lookup"><span data-stu-id="3b978-162">Browsers</span></span>
![Lapbetöltési idő bontása](./media/app-insights-sample-mscrm/22.png)

![A böngésző verziója-munkamenetek száma](./media/app-insights-sample-mscrm/23.png)

#### <a name="geolocation"></a><span data-ttu-id="3b978-165">Földrajzi hely</span><span class="sxs-lookup"><span data-stu-id="3b978-165">Geolocation</span></span>
![Ország munkamenetek száma](./media/app-insights-sample-mscrm/24.png)

![A munkamenetek és országonként felhasználók](./media/app-insights-sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a><span data-ttu-id="3b978-168">Belső lapkérelem megtekintése</span><span class="sxs-lookup"><span data-stu-id="3b978-168">Inside page view request</span></span>
![Lap összegzésének megtekintése](./media/app-insights-sample-mscrm/26.png)

![Keresés lap eseményeinek megtekintése](./media/app-insights-sample-mscrm/27.png)

![Hasonló Lapmegtekintések](./media/app-insights-sample-mscrm/28.png)

![Lapmegtekintési tulajdonságok](./media/app-insights-sample-mscrm/29.png)

![Egy munkamenet oldal](./media/app-insights-sample-mscrm/30.png)

## <a name="sample-code"></a><span data-ttu-id="3b978-174">Mintakód</span><span class="sxs-lookup"><span data-stu-id="3b978-174">Sample code</span></span>
<span data-ttu-id="3b978-175">[Keresse meg a hello mintakód](https://dynamicsandappinsights.codeplex.com/).</span><span class="sxs-lookup"><span data-stu-id="3b978-175">[Browse hello sample code](https://dynamicsandappinsights.codeplex.com/).</span></span>

## <a name="power-bi"></a><span data-ttu-id="3b978-176">Power BI</span><span class="sxs-lookup"><span data-stu-id="3b978-176">Power BI</span></span>
<span data-ttu-id="3b978-177">Lehetőség van még mélyebb elemzés, ha Ön [hello adatok tooMicrosoft Power BI exportálása](app-insights-export-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="3b978-177">You can do even deeper analysis if you [export hello data tooMicrosoft Power BI](app-insights-export-power-bi.md).</span></span>

## <a name="sample-microsoft-dynamics-crm-solution"></a><span data-ttu-id="3b978-178">A Microsoft Dynamics CRM megoldást</span><span class="sxs-lookup"><span data-stu-id="3b978-178">Sample Microsoft Dynamics CRM Solution</span></span>
<span data-ttu-id="3b978-179">[Itt van megvalósítva a Microsoft Dynamics CRM hello megoldást](https://dynamicsandappinsights.codeplex.com/).</span><span class="sxs-lookup"><span data-stu-id="3b978-179">[Here is hello sample solution implemented in Microsoft Dynamics CRM](https://dynamicsandappinsights.codeplex.com/).</span></span>

## <a name="learn-more"></a><span data-ttu-id="3b978-180">Részletek</span><span class="sxs-lookup"><span data-stu-id="3b978-180">Learn more</span></span>
* [<span data-ttu-id="3b978-181">Mi az Application Insights?</span><span class="sxs-lookup"><span data-stu-id="3b978-181">What is Application Insights?</span></span>](app-insights-overview.md)
* [<span data-ttu-id="3b978-182">Az Application Insights webes</span><span class="sxs-lookup"><span data-stu-id="3b978-182">Application Insights for web pages</span></span>](app-insights-javascript.md)
* [<span data-ttu-id="3b978-183">További mintákat és forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="3b978-183">More samples and walkthroughs</span></span>](app-insights-code-samples.md)
