---
title: "Vizsgálja meg és használati adatok megosztása a interaktív Azure Application Insights-munkafüzetek |} Microsoft docs"
description: "Webes alkalmazása felhasználóit demográfiai elemzése."
services: application-insights
documentationcenter: 
author: numberbycolors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 06/12/2017
ms.author: bwren
ms.openlocfilehash: 75028b4fbda43d90f56690a33c7eb624fce049c8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="investigate-and-share-usage-data-with-interactive-workbooks-in-application-insights"></a><span data-ttu-id="966a5-103">Vizsgálja meg és használati adatok megosztása az Application Insightsban interaktív munkafüzetek</span><span class="sxs-lookup"><span data-stu-id="966a5-103">Investigate and share usage data with interactive workbooks in Application Insights</span></span>

<span data-ttu-id="966a5-104">Munkafüzetek egyesítése [Azure Application Insights](app-insights-overview.md) adatmegjelenítésekkel, [elemzési lekérdezések](app-insights-analytics.md), és interaktív dokumentumok szövegdobozba.</span><span class="sxs-lookup"><span data-stu-id="966a5-104">Workbooks combine [Azure Application Insights](app-insights-overview.md) data visualizations, [Analytics queries](app-insights-analytics.md), and text into interactive documents.</span></span> <span data-ttu-id="966a5-105">Munkafüzetekhez a ugyanahhoz az Azure-erőforráshoz hozzáféréssel rendelkező más csoport tagjai.</span><span class="sxs-lookup"><span data-stu-id="966a5-105">Workbooks are editable by other team members with access to the same Azure resource.</span></span> <span data-ttu-id="966a5-106">Ez azt jelenti, hogy a lekérdezések és a munkafüzet létrehozásához használt funkciók érhetők el a más személy éri el a munkafüzetet, így könnyen vizsgálatát, kiterjesztése, és ellenőrizze a hibákat.</span><span class="sxs-lookup"><span data-stu-id="966a5-106">This means the queries and controls used to create a workbook are available to other people reading the workbook, making them easy to explore, extend, and check for mistakes.</span></span>

<span data-ttu-id="966a5-107">Munkafüzetek hasznosak a helyzetekben, például:</span><span class="sxs-lookup"><span data-stu-id="966a5-107">Workbooks are helpful for scenarios like:</span></span>

* <span data-ttu-id="966a5-108">Az alkalmazás használatát tervezi, amikor a fontos metrikák előzetesen nem tudja: számok felhasználók, adatmegőrzési arányt, átváltási, stb. Egyéb használati elemzőeszközök az Application Insightsban, eltérően munkafüzetek lehetővé teszik, hogy több típusú képi megjelenítések és elemzéseket, szabad formátumú feltárása az ilyen nagyszerű minősítené kombinálni.</span><span class="sxs-lookup"><span data-stu-id="966a5-108">Exploring the usage of your app when you don't know the metrics of interest in advance: numbers of users, retention rates, conversion rates, etc. Unlike other usage analytics tools in Application Insights, workbooks let you combine multiple kinds of visualizations and analyses, making them great for this kind of free-form exploration.</span></span>
* <span data-ttu-id="966a5-109">A csapattal elmagyarázza, hogyan működik-e egy újonnan kiadott funkció, megjelenítő felhasználó álló kulcs kapcsolati és más metrikákkal.</span><span class="sxs-lookup"><span data-stu-id="966a5-109">Explaining to your team how a newly released feature is performing, by showing user counts for key interactions and other metrics.</span></span>
* <span data-ttu-id="966a5-110">Megosztják az eredményeket, egy A / B kísérletezhet az alkalmazás más a csoport tagjai.</span><span class="sxs-lookup"><span data-stu-id="966a5-110">Sharing the results of an A/B experiment in your app with other members of your team.</span></span> <span data-ttu-id="966a5-111">A kitűzött célokat a kísérleti fázisú funkciókat szövegre ismertetik, majd minden használati metrika és annak ellenőrzésére használ a kísérlet, és törölje a jelet hívás elemek számára, hogy volt-e mindegyik metrikát fölött vagy alatt-célzott Analytics lekérdezés megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="966a5-111">You can explain the goals for the experiment with text, then show each usage metric and Analytics query used to evaluate the experiment, along with clear call-outs for whether each metric was above- or below-target.</span></span>
* <span data-ttu-id="966a5-112">A jelentéskészítési kimaradás hatását a az alkalmazás adatokat, a szöveg magyarázat és az információt a következő lépései kimaradások elkerülése használati.</span><span class="sxs-lookup"><span data-stu-id="966a5-112">Reporting the impact of an outage on the usage of your app, combining data, text explanation, and a discussion of next steps to prevent outages in the future.</span></span>

> [!NOTE]
> <span data-ttu-id="966a5-113">Az Application Insights-erőforrás Lapmegtekintések vagy munkafüzetek használandó egyéni események kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="966a5-113">Your Application Insights resource must contain page views or custom events to use workbooks.</span></span> <span data-ttu-id="966a5-114">[Ismerje meg, hogyan állíthat be az alkalmazás automatikusan az Application Insights JavaScript SDK a lapmegtekintések gyűjtéséhez](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="966a5-114">[Learn how to set up your app to collect page views automatically with the Application Insights JavaScript SDK](app-insights-javascript.md).</span></span>
> 
> 

## <a name="editing-rearranging-cloning-and-deleting-workbook-sections"></a><span data-ttu-id="966a5-115">Szerkesztés, átrendezése, a Klónozás és a munkafüzet szakaszok törlése</span><span class="sxs-lookup"><span data-stu-id="966a5-115">Editing, rearranging, cloning, and deleting workbook sections</span></span>

<span data-ttu-id="966a5-116">A munkafüzet egy készült szakaszok: függetlenül szerkeszthető használati képi megjelenítéseket, diagramok, táblák, text vagy Analytics lekérdezési eredmények.</span><span class="sxs-lookup"><span data-stu-id="966a5-116">A workbook is a made of sections: independently editable usage visualizations, charts, tables, text, or Analytics query results.</span></span>

<span data-ttu-id="966a5-117">A munkafüzet szakasz tartalmának szerkesztéséhez kattintson a **szerkesztése** alatt és a munkafüzet szakasz jobb gomb.</span><span class="sxs-lookup"><span data-stu-id="966a5-117">To edit the contents of a workbook section, click the **Edit** button below and to the right of the workbook section.</span></span>

![Application Insights munkafüzetek szakasz szerkesztési vezérlők](./media/app-insights-usage-workbooks/editing-controls.png)

1. <span data-ttu-id="966a5-119">Ha elkészült szakasz módosításához kattintson a **végzett szerkesztése** szakasz bal alsó sarkában található.</span><span class="sxs-lookup"><span data-stu-id="966a5-119">When you're done editing a section, click **Done Editing** in the bottom left corner of the section.</span></span>

2. <span data-ttu-id="966a5-120">Szakasz duplikált létrehozásához kattintson a **ebben a szakaszban klónozni** ikonra.</span><span class="sxs-lookup"><span data-stu-id="966a5-120">To create a duplicate of a section, click the **Clone this section** icon.</span></span> <span data-ttu-id="966a5-121">Ismétlődő szakaszok létrehozása egy nagy előző ismétlési elvesztése nélkül lekérdezés felépítésének módját.</span><span class="sxs-lookup"><span data-stu-id="966a5-121">Creating duplicate sections is a great to way to iterate on a query without losing previous iterations.</span></span>

3. <span data-ttu-id="966a5-122">Kattintson egy munkafüzet szakaszt áthelyezéséhez a **feljebb** vagy **lejjebb** ikonra.</span><span class="sxs-lookup"><span data-stu-id="966a5-122">To move up a section in a workbook, click the **Move up** or **Move down** icon.</span></span>

4. <span data-ttu-id="966a5-123">A szakasz végleges eltávolításához kattintson a **eltávolítása** ikonra.</span><span class="sxs-lookup"><span data-stu-id="966a5-123">To remove a section permanently, click the **Remove** icon.</span></span>

## <a name="adding-usage-data-visualization-sections"></a><span data-ttu-id="966a5-124">Használati adatok képi megjelenítés szakaszok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="966a5-124">Adding usage data visualization sections</span></span>

<span data-ttu-id="966a5-125">Munkafüzetek beépített használati analytics képi megjelenítések négy típusú kínálnak.</span><span class="sxs-lookup"><span data-stu-id="966a5-125">Workbooks offer four types of built-in usage analytics visualizations.</span></span> <span data-ttu-id="966a5-126">Minden ad választ az alkalmazás a használati közös kérdése.</span><span class="sxs-lookup"><span data-stu-id="966a5-126">Each answers a common question about the usage of your app.</span></span> <span data-ttu-id="966a5-127">Vegye fel a táblázatokat és diagramokat, ezek a szakaszok eltérő, vegye fel az Analytics lekérdezési szakaszok (lásd alább).</span><span class="sxs-lookup"><span data-stu-id="966a5-127">To add tables and charts other than these four sections, add Analytics query sections (see below).</span></span>

<span data-ttu-id="966a5-128">A felhasználók hozzáadásához munkamenetek, az események vagy az adatmegőrzési szakasz a munkafüzetbe, használja a **felhasználó hozzáadása** vagy más megfelelő gombra a munkafüzet alján, vagy bármely szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="966a5-128">To add a Users, Sessions, Events, or Retention section to your workbook, use the **Add Users** or other corresponding button at the bottom of the workbook, or at the bottom of any section.</span></span>

![Felhasználók szakaszban munkafüzetekben](./media/app-insights-usage-workbooks/users-section.png)

<span data-ttu-id="966a5-130">**Felhasználók** szakaszok választ "hány felhasználó néhány tekint, vagy használja a webhely néhány funkciója?"</span><span class="sxs-lookup"><span data-stu-id="966a5-130">**Users** sections answer "How many users viewed some page or used some feature of my site?"</span></span>

<span data-ttu-id="966a5-131">**Munkamenetek** szakaszok választ "hány munkamenetek volt igénybe néhány lap megtekintése vagy a webhely néhány funkció használatával?"</span><span class="sxs-lookup"><span data-stu-id="966a5-131">**Sessions** sections answer "How many sessions did users spend viewing some page or using some feature of my site?"</span></span>

<span data-ttu-id="966a5-132">**Események** szakaszok választ "hány alkalommal nem felhasználók néhány a lapnak a megtekintésére vagy használja a webhely néhány szolgáltatást?"</span><span class="sxs-lookup"><span data-stu-id="966a5-132">**Events** sections answer "How many times did users view some page or use some feature of my site?"</span></span>

<span data-ttu-id="966a5-133">A vezérlők és a képi megjelenítések egy készletet minden, a következő három szakasz típusú kínálja:</span><span class="sxs-lookup"><span data-stu-id="966a5-133">Each of these three section types offers the same sets of controls and visualizations:</span></span>

* [<span data-ttu-id="966a5-134">További tudnivalók a felhasználói munkamenetek és események szakaszok szerkesztése</span><span class="sxs-lookup"><span data-stu-id="966a5-134">Learn more about editing Users, Sessions, and Events sections</span></span>](app-insights-usage-segmentation.md)
* <span data-ttu-id="966a5-135">Váltás a fő diagram, hisztogram rácsok, automatikus insights és minta felhasználók képi megjelenítések használatával a **diagram megjelenítése**, **rácsvonalak megjelenítése**, **megjelenítése Insights**, és **Ezek próbafelhasználók** jelölőnégyzeteket minden szakasz elején.</span><span class="sxs-lookup"><span data-stu-id="966a5-135">Toggle the main chart, histogram grids, automatic insights, and sample users visualizations using the **Show Chart**, **Show Grid**, **Show Insights**, and **Sample of These Users** checkboxes at the top of each section.</span></span>

![A munkafüzet megőrzési szakasz](./media/app-insights-usage-workbooks/retention-section.png)

<span data-ttu-id="966a5-137">**Megőrzési** szakaszok választ "Személyek néhány tekint, vagy a használt egyes szolgáltatások egy nap vagy hét, hány visszatért az ezt követő nap vagy hét?"</span><span class="sxs-lookup"><span data-stu-id="966a5-137">**Retention** sections answer "Of people who viewed some page or used some feature on one day or week, how many came back in a subsequent day or week?"</span></span>

* [<span data-ttu-id="966a5-138">Tudjon meg többet az adatmegőrzési szakaszok szerkesztése</span><span class="sxs-lookup"><span data-stu-id="966a5-138">Learn more about editing Retention sections</span></span>](app-insights-usage-retention.md)
* <span data-ttu-id="966a5-139">A választható általános megőrzési diagram használatával váltása a **megjelenítése a teljes megőrzési diagram** jelölőnégyzetet a szakasz tetején.</span><span class="sxs-lookup"><span data-stu-id="966a5-139">Toggle the optional Overall Retention chart using the **Show overall retention chart** checkbox at the top of the section.</span></span>

## <a name="adding-application-insights-analytics-sections"></a><span data-ttu-id="966a5-140">Application Insights Analytics szakaszok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="966a5-140">Adding Application Insights Analytics sections</span></span>

![A munkafüzeteket Analytics szakasz](./media/app-insights-usage-workbooks/analytics-section.png)

<span data-ttu-id="966a5-142">Az Application Insights Analytics lekérdezésszakaszt adhat hozzá a munkafüzethez a **hozzáadása Analytics lekérdezési** gombra a munkafüzet alján, vagy bármely szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="966a5-142">To add an Application Insights Analytics query section to your workbook, use the **Add Analytics query** button at the bottom of the workbook, or at the bottom of any section.</span></span>

<span data-ttu-id="966a5-143">Vegyen fel tetszőleges lekérdezések keresztül az Application Insights adatainak munkafüzetek Analytics lekérdezés szakaszok segítségével.</span><span class="sxs-lookup"><span data-stu-id="966a5-143">Analytics query sections let you add arbitrary queries over your Application Insights data into workbooks.</span></span> <span data-ttu-id="966a5-144">Ez azt jelenti, hogy Analytics lekérdezési szakaszok kell lennie a nyissa meg a a felhasználók, a munkamenetek, az események és a megőrzési, például a fent felsorolt négy eltérő a hellyel kapcsolatos kérdése megválaszolásához:</span><span class="sxs-lookup"><span data-stu-id="966a5-144">This flexibility means Analytics query sections should be your go-to for answering any questions about your site other than the four listed above for Users, Sessions, Events, and Retention, like:</span></span>

* <span data-ttu-id="966a5-145">Hány kivételek fejeződött a hely throw idő alatt, csökken a használat során?</span><span class="sxs-lookup"><span data-stu-id="966a5-145">How many exceptions did your site throw during the same time period as a decline in usage?</span></span>
* <span data-ttu-id="966a5-146">Mi volt a lapbetöltési idők néhány lap megtekintésének felhasználók terjesztése?</span><span class="sxs-lookup"><span data-stu-id="966a5-146">What was the distribution of page load times for users viewing some page?</span></span>
* <span data-ttu-id="966a5-147">Hány felhasználó láthatók a webhely néhány azon lapok készlete, de ez nem valamilyen egyéb lapok?</span><span class="sxs-lookup"><span data-stu-id="966a5-147">How many users viewed some set of pages on your site, but not some other set of pages?</span></span> <span data-ttu-id="966a5-148">Ez akkor lehet hasznos megértéséhez, ha a felhasználók, akik használni a webhely funkciók különböző részhalmazai fürttel rendelkezik (használja a `join` operátor ezzel a `kind=leftanti` Naplóelemzési lekérdezés nyelvű módosítóval).</span><span class="sxs-lookup"><span data-stu-id="966a5-148">This can be useful to understand if you have clusters of users who use different subsets of your site's functionality (use the `join` operator with the `kind=leftanti` modifier in the Log Analytics query language).</span></span>

<span data-ttu-id="966a5-149">Használja a [Naplóelemzési lekérdezése nyelvi referencia](https://docs.loganalytics.io/) kapcsolatos lekérdezések írásáról további.</span><span class="sxs-lookup"><span data-stu-id="966a5-149">Use the [Log Analytics query language reference](https://docs.loganalytics.io/) to learn more about writing queries.</span></span>

## <a name="adding-text-and-markdown-sections"></a><span data-ttu-id="966a5-150">Szöveg- és Markdown-szakaszok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="966a5-150">Adding text and Markdown sections</span></span>

<span data-ttu-id="966a5-151">Segítséget nyújt a fejlécére kattintva rendezhető, magyarázatot és magyarázatokkal hozzáadása a munkafüzeteket azokat a leírásokat kapcsolja táblázatokat és diagramokat olyan készlete.</span><span class="sxs-lookup"><span data-stu-id="966a5-151">Adding headings, explanations, and commentary to your workbooks helps turn a set of tables and charts into a narrative.</span></span> <span data-ttu-id="966a5-152">Szöveg részeiben munkafüzetek támogatása a [Markdown-szintaxis](https://daringfireball.net/projects/markdown/) formázás, például a fejlécére kattintva rendezhető, félkövér, dőlt és felsorolásokat szöveg.</span><span class="sxs-lookup"><span data-stu-id="966a5-152">Text sections in workbooks support the [Markdown syntax](https://daringfireball.net/projects/markdown/) for text formatting, like headings, bold, italics, and bulleted lists.</span></span>

<span data-ttu-id="966a5-153">A munkafüzet szöveges szakaszt hozzáadásához használja a **szöveget** gombra a munkafüzet alján, vagy bármely szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="966a5-153">To add a text section to your workbook, use the **Add text** button at the bottom of the workbook, or at the bottom of any section.</span></span>

## <a name="saving-and-sharing-workbooks-with-your-team"></a><span data-ttu-id="966a5-154">És munkafüzetek megosztása a munkatársaival</span><span class="sxs-lookup"><span data-stu-id="966a5-154">Saving and sharing workbooks with your team</span></span>

<span data-ttu-id="966a5-155">Az Application Insights-erőforrást, vagy mentett munkafüzetek a **jelentések** privát, akkor vagy a következő szakasz a **megosztott jelentések** hozzáféréssel rendelkező összes felhasználó számára elérhető szakaszban a Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="966a5-155">Workbooks are saved within an Application Insights resource, either in the **My Reports** section that's private to you or in the **Shared Reports** section that's accessible to everyone with access to the Application Insights resource.</span></span> <span data-ttu-id="966a5-156">Az erőforrás a munkafüzetek megtekintéséhez kattintson a **nyitott** műveletsávon gombra.</span><span class="sxs-lookup"><span data-stu-id="966a5-156">To view all the workbooks in the resource, click the **Open** button in the action bar.</span></span>

<span data-ttu-id="966a5-157">A munkafüzetet, hogy e **jelentések**:</span><span class="sxs-lookup"><span data-stu-id="966a5-157">To share a workbook that's currently in **My Reports**:</span></span>

1. <span data-ttu-id="966a5-158">Kattintson a **nyitott** műveletsávon</span><span class="sxs-lookup"><span data-stu-id="966a5-158">Click **Open** in the action bar</span></span>
2. <span data-ttu-id="966a5-159">Kattintson a "..." gomb melletti meg szeretné osztani a munkafüzet</span><span class="sxs-lookup"><span data-stu-id="966a5-159">Click the "..." button beside the workbook you want to share</span></span>
3. <span data-ttu-id="966a5-160">Kattintson a **megosztott jelentések áthelyezése**.</span><span class="sxs-lookup"><span data-stu-id="966a5-160">Click **Move to Shared Reports**.</span></span>

<span data-ttu-id="966a5-161">A munkafüzet egy hivatkozás, vagy e-mailben megosztásához kattintson **megosztása** műveletsávon.</span><span class="sxs-lookup"><span data-stu-id="966a5-161">To share a workbook with a link or via email, click **Share** in the action bar.</span></span> <span data-ttu-id="966a5-162">Ne feledje, hogy a hivatkozás címzettjei kell-e az Azure portálon a munkafüzet az erőforráshoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="966a5-162">Keep in mind that recipients of the link need access to this resource in the Azure portal to view the workbook.</span></span> <span data-ttu-id="966a5-163">Ahhoz, hogy a módosításokat, a címzettnek van szüksége, legalább az erőforrás-közreműködői jogokat.</span><span class="sxs-lookup"><span data-stu-id="966a5-163">To make edits, recipients need at least Contributor permissions for the resource.</span></span>

<span data-ttu-id="966a5-164">PIN-kód egy Azure-irányítópultot munkafüzet mutató hivatkozást:</span><span class="sxs-lookup"><span data-stu-id="966a5-164">To pin a link to a workbook to an Azure Dashboard:</span></span>

1. <span data-ttu-id="966a5-165">Kattintson a **nyitott** műveletsávon</span><span class="sxs-lookup"><span data-stu-id="966a5-165">Click **Open** in the action bar</span></span>
2. <span data-ttu-id="966a5-166">Kattintson a "..." gombra a munkafüzetet, amelyet szeretne rögzíteni mellett</span><span class="sxs-lookup"><span data-stu-id="966a5-166">Click the "..." button beside the workbook you want to pin</span></span>
3. <span data-ttu-id="966a5-167">Kattintson a **rögzítés az irányítópulton**.</span><span class="sxs-lookup"><span data-stu-id="966a5-167">Click **Pin to dashboard**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="966a5-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="966a5-168">Next steps</span></span>

## <a name="next-steps"></a><span data-ttu-id="966a5-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="966a5-169">Next steps</span></span>
- <span data-ttu-id="966a5-170">Ahhoz, hogy a használati tapasztalatok, küldésének megkezdése [egyéni események](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) vagy [lapmegtekintés](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span><span class="sxs-lookup"><span data-stu-id="966a5-170">To enable usage experiences, start sending [custom events](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) or [page views](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span></span>
- <span data-ttu-id="966a5-171">Ha egyéni események vagy Lapmegtekintések már küld, megismerkedhet a használati eszközök további, a szolgáltatás használatát a felhasználók.</span><span class="sxs-lookup"><span data-stu-id="966a5-171">If you already send custom events or page views, explore the Usage tools to learn how users use your service.</span></span>
    - [<span data-ttu-id="966a5-172">Felhasználók, munkamenetek, események</span><span class="sxs-lookup"><span data-stu-id="966a5-172">Users, Sessions, Events</span></span>](app-insights-usage-segmentation.md)
    - [<span data-ttu-id="966a5-173">Tölcsérek</span><span class="sxs-lookup"><span data-stu-id="966a5-173">Funnels</span></span>](usage-funnels.md)
    - [<span data-ttu-id="966a5-174">Megőrzés</span><span class="sxs-lookup"><span data-stu-id="966a5-174">Retention</span></span>](app-insights-usage-retention.md)
    - [<span data-ttu-id="966a5-175">Felhasználói folyamatok</span><span class="sxs-lookup"><span data-stu-id="966a5-175">User Flows</span></span>](app-insights-usage-flows.md)
    - [<span data-ttu-id="966a5-176">Felhasználói környezet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="966a5-176">Add user context</span></span>](app-insights-usage-send-user-context.md)
    
