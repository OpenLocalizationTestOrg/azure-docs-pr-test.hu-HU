---
title: "interaktív Azure Application Insights-munkafüzetek aaaInvestigate és megosztási használati adatok |} Microsoft docs"
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
ms.openlocfilehash: bdcebe0f97fdad0a0b301df5950dc09698f5a4dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="investigate-and-share-usage-data-with-interactive-workbooks-in-application-insights"></a><span data-ttu-id="d9964-103">Vizsgálja meg és használati adatok megosztása az Application Insightsban interaktív munkafüzetek</span><span class="sxs-lookup"><span data-stu-id="d9964-103">Investigate and share usage data with interactive workbooks in Application Insights</span></span>

<span data-ttu-id="d9964-104">Munkafüzetek egyesítése [Azure Application Insights](app-insights-overview.md) adatmegjelenítésekkel, [elemzési lekérdezések](app-insights-analytics.md), és interaktív dokumentumok szövegdobozba.</span><span class="sxs-lookup"><span data-stu-id="d9964-104">Workbooks combine [Azure Application Insights](app-insights-overview.md) data visualizations, [Analytics queries](app-insights-analytics.md), and text into interactive documents.</span></span> <span data-ttu-id="d9964-105">Munkafüzetekhez a többi csoport tagjai az access toohello azonos Azure-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="d9964-105">Workbooks are editable by other team members with access toohello same Azure resource.</span></span> <span data-ttu-id="d9964-106">Ez azt jelenti, hello lekérdezések és -vezérlők használt toocreate munkafüzet elérhető tooother személy éri el az hello munkafüzet, így azok könnyen tooexplore, kiterjesztése, és ellenőrizze a hibákat.</span><span class="sxs-lookup"><span data-stu-id="d9964-106">This means hello queries and controls used toocreate a workbook are available tooother people reading hello workbook, making them easy tooexplore, extend, and check for mistakes.</span></span>

<span data-ttu-id="d9964-107">Munkafüzetek hasznosak a helyzetekben, például:</span><span class="sxs-lookup"><span data-stu-id="d9964-107">Workbooks are helpful for scenarios like:</span></span>

* <span data-ttu-id="d9964-108">Hello használata az alkalmazás fel, ha fontos hello metrikák előzetesen nem tudja: számok felhasználók, adatmegőrzési arányt, átváltási, stb. Egyéb használati elemzőeszközök az Application Insightsban, eltérően munkafüzetek lehetővé teszik, hogy több típusú képi megjelenítések és elemzéseket, szabad formátumú feltárása az ilyen nagyszerű minősítené kombinálni.</span><span class="sxs-lookup"><span data-stu-id="d9964-108">Exploring hello usage of your app when you don't know hello metrics of interest in advance: numbers of users, retention rates, conversion rates, etc. Unlike other usage analytics tools in Application Insights, workbooks let you combine multiple kinds of visualizations and analyses, making them great for this kind of free-form exploration.</span></span>
* <span data-ttu-id="d9964-109">Hogyan működik-e egy újonnan kiadott szolgáltatás tooyour team foglalja össze, megjelenítő felhasználó álló kulcs kapcsolati és más metrikákkal.</span><span class="sxs-lookup"><span data-stu-id="d9964-109">Explaining tooyour team how a newly released feature is performing, by showing user counts for key interactions and other metrics.</span></span>
* <span data-ttu-id="d9964-110">Megosztás hello eredményei egy A / B kísérletezhet az alkalmazás más a csoport tagjai.</span><span class="sxs-lookup"><span data-stu-id="d9964-110">Sharing hello results of an A/B experiment in your app with other members of your team.</span></span> <span data-ttu-id="d9964-111">Hello hello célokat szöveg kísérletezhet, majd minden egyes használati metrika megjelenítése és Analytics query használják tooevaluate hello kísérletet, és törölje a jelet hívás elemek számára, hogy volt-e mindegyik metrikát fölött vagy alatt-célzott ismertetik.</span><span class="sxs-lookup"><span data-stu-id="d9964-111">You can explain hello goals for hello experiment with text, then show each usage metric and Analytics query used tooevaluate hello experiment, along with clear call-outs for whether each metric was above- or below-target.</span></span>
* <span data-ttu-id="d9964-112">Nem tervezett kimaradás hello hatását Reporting hello használata az alkalmazás adatokat, a szöveg magyarázat és az információt a következő lépéseket tooprevent kimaradások számát a jövőbeli hello.</span><span class="sxs-lookup"><span data-stu-id="d9964-112">Reporting hello impact of an outage on hello usage of your app, combining data, text explanation, and a discussion of next steps tooprevent outages in hello future.</span></span>

> [!NOTE]
> <span data-ttu-id="d9964-113">Az Application Insights-erőforrás Lapmegtekintések vagy egyéni események toouse munkafüzetek kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="d9964-113">Your Application Insights resource must contain page views or custom events toouse workbooks.</span></span> <span data-ttu-id="d9964-114">[Ismerje meg, hogyan az alkalmazások toocollect oldalára tooset automatikusan az Application Insights JavaScript SDK hello megtekinti](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="d9964-114">[Learn how tooset up your app toocollect page views automatically with hello Application Insights JavaScript SDK](app-insights-javascript.md).</span></span>
> 
> 

## <a name="editing-rearranging-cloning-and-deleting-workbook-sections"></a><span data-ttu-id="d9964-115">Szerkesztés, átrendezése, a Klónozás és a munkafüzet szakaszok törlése</span><span class="sxs-lookup"><span data-stu-id="d9964-115">Editing, rearranging, cloning, and deleting workbook sections</span></span>

<span data-ttu-id="d9964-116">A munkafüzet egy készült szakaszok: függetlenül szerkeszthető használati képi megjelenítéseket, diagramok, táblák, text vagy Analytics lekérdezési eredmények.</span><span class="sxs-lookup"><span data-stu-id="d9964-116">A workbook is a made of sections: independently editable usage visualizations, charts, tables, text, or Analytics query results.</span></span>

<span data-ttu-id="d9964-117">a munkafüzet szakasz tartalmának megjelenítése tooedit hello kattintson hello **szerkesztése** gombra és toohello sarkában hello munkafüzet szakasz.</span><span class="sxs-lookup"><span data-stu-id="d9964-117">tooedit hello contents of a workbook section, click hello **Edit** button below and toohello right of hello workbook section.</span></span>

![Application Insights munkafüzetek szakasz szerkesztési vezérlők](./media/app-insights-usage-workbooks/editing-controls.png)

1. <span data-ttu-id="d9964-119">Ha elkészült szakasz módosításához kattintson a **végzett szerkesztése** hello bal alsó sarkában hello szakaszban található.</span><span class="sxs-lookup"><span data-stu-id="d9964-119">When you're done editing a section, click **Done Editing** in hello bottom left corner of hello section.</span></span>

2. <span data-ttu-id="d9964-120">a szakasz duplikált toocreate kattintson hello **klónozni ebben a szakaszban** ikon.</span><span class="sxs-lookup"><span data-stu-id="d9964-120">toocreate a duplicate of a section, click hello **Clone this section** icon.</span></span> <span data-ttu-id="d9964-121">Ismétlődő szakaszok létrehozása egy nagyszerű tooway tooiterate lekérdezés előző ismétlési elvesztése nélkül.</span><span class="sxs-lookup"><span data-stu-id="d9964-121">Creating duplicate sections is a great tooway tooiterate on a query without losing previous iterations.</span></span>

3. <span data-ttu-id="d9964-122">egy munkafüzet egy másolat toomove kattintson hello **feljebb** vagy **lejjebb** ikon.</span><span class="sxs-lookup"><span data-stu-id="d9964-122">toomove up a section in a workbook, click hello **Move up** or **Move down** icon.</span></span>

4. <span data-ttu-id="d9964-123">a szakasz tooremove véglegesen, kattintson a hello **eltávolítása** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d9964-123">tooremove a section permanently, click hello **Remove** icon.</span></span>

## <a name="adding-usage-data-visualization-sections"></a><span data-ttu-id="d9964-124">Használati adatok képi megjelenítés szakaszok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d9964-124">Adding usage data visualization sections</span></span>

<span data-ttu-id="d9964-125">Munkafüzetek beépített használati analytics képi megjelenítések négy típusú kínálnak.</span><span class="sxs-lookup"><span data-stu-id="d9964-125">Workbooks offer four types of built-in usage analytics visualizations.</span></span> <span data-ttu-id="d9964-126">Minden ad választ az alkalmazás hello használati közös kérdése.</span><span class="sxs-lookup"><span data-stu-id="d9964-126">Each answers a common question about hello usage of your app.</span></span> <span data-ttu-id="d9964-127">tooadd táblázatokat és diagramokat eltérő a következő négy részeket is vegye fel az elemzés lekérdezés szakaszok (lásd alább).</span><span class="sxs-lookup"><span data-stu-id="d9964-127">tooadd tables and charts other than these four sections, add Analytics query sections (see below).</span></span>

<span data-ttu-id="d9964-128">a felhasználók tooadd, munkamenetek, események vagy megőrzési szakasz tooyour munkafüzet, használjon hello **felhasználó hozzáadása** vagy más megfelelő gomb hello munkafüzet hello alján, vagy bármely szakasz hello alján.</span><span class="sxs-lookup"><span data-stu-id="d9964-128">tooadd a Users, Sessions, Events, or Retention section tooyour workbook, use hello **Add Users** or other corresponding button at hello bottom of hello workbook, or at hello bottom of any section.</span></span>

![Felhasználók szakaszban munkafüzetekben](./media/app-insights-usage-workbooks/users-section.png)

<span data-ttu-id="d9964-130">**Felhasználók** szakaszok választ "hány felhasználó néhány tekint, vagy használja a webhely néhány funkciója?"</span><span class="sxs-lookup"><span data-stu-id="d9964-130">**Users** sections answer "How many users viewed some page or used some feature of my site?"</span></span>

<span data-ttu-id="d9964-131">**Munkamenetek** szakaszok választ "hány munkamenetek volt igénybe néhány lap megtekintése vagy a webhely néhány funkció használatával?"</span><span class="sxs-lookup"><span data-stu-id="d9964-131">**Sessions** sections answer "How many sessions did users spend viewing some page or using some feature of my site?"</span></span>

<span data-ttu-id="d9964-132">**Események** szakaszok választ "hány alkalommal nem felhasználók néhány a lapnak a megtekintésére vagy használja a webhely néhány szolgáltatást?"</span><span class="sxs-lookup"><span data-stu-id="d9964-132">**Events** sections answer "How many times did users view some page or use some feature of my site?"</span></span>

<span data-ttu-id="d9964-133">Minden, a következő három szakasz típusú vezérlők és a képi megjelenítések azonos készleteinek kínál hello:</span><span class="sxs-lookup"><span data-stu-id="d9964-133">Each of these three section types offers hello same sets of controls and visualizations:</span></span>

* [<span data-ttu-id="d9964-134">További tudnivalók a felhasználói munkamenetek és események szakaszok szerkesztése</span><span class="sxs-lookup"><span data-stu-id="d9964-134">Learn more about editing Users, Sessions, and Events sections</span></span>](app-insights-usage-segmentation.md)
* <span data-ttu-id="d9964-135">Váltás hello fő diagram, hisztogram rácsok, automatikus insights és minta felhasználók képi megjelenítések hello segítségével **diagram megjelenítése**, **rácsvonalak megjelenítése**, **megjelenítése Insights**, és **Ezek próbafelhasználók** jelölőnégyzeteket minden szakasz hello tetején.</span><span class="sxs-lookup"><span data-stu-id="d9964-135">Toggle hello main chart, histogram grids, automatic insights, and sample users visualizations using hello **Show Chart**, **Show Grid**, **Show Insights**, and **Sample of These Users** checkboxes at hello top of each section.</span></span>

![A munkafüzet megőrzési szakasz](./media/app-insights-usage-workbooks/retention-section.png)

<span data-ttu-id="d9964-137">**Megőrzési** szakaszok választ "Személyek néhány tekint, vagy a használt egyes szolgáltatások egy nap vagy hét, hány visszatért az ezt követő nap vagy hét?"</span><span class="sxs-lookup"><span data-stu-id="d9964-137">**Retention** sections answer "Of people who viewed some page or used some feature on one day or week, how many came back in a subsequent day or week?"</span></span>

* [<span data-ttu-id="d9964-138">Tudjon meg többet az adatmegőrzési szakaszok szerkesztése</span><span class="sxs-lookup"><span data-stu-id="d9964-138">Learn more about editing Retention sections</span></span>](app-insights-usage-retention.md)
* <span data-ttu-id="d9964-139">Váltás hello választható általános megőrzési diagram hello segítségével **megjelenítése a teljes megőrzési diagram** hello szakasz hello tetején jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="d9964-139">Toggle hello optional Overall Retention chart using hello **Show overall retention chart** checkbox at hello top of hello section.</span></span>

## <a name="adding-application-insights-analytics-sections"></a><span data-ttu-id="d9964-140">Application Insights Analytics szakaszok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d9964-140">Adding Application Insights Analytics sections</span></span>

![A munkafüzeteket Analytics szakasz](./media/app-insights-usage-workbooks/analytics-section.png)

<span data-ttu-id="d9964-142">az Application Insights Analytics lekérdezési szakasz tooyour munkafüzet tooadd hello használata **hozzáadása Analytics lekérdezési** gomb hello munkafüzet hello alján, vagy bármely szakasz hello alján.</span><span class="sxs-lookup"><span data-stu-id="d9964-142">tooadd an Application Insights Analytics query section tooyour workbook, use hello **Add Analytics query** button at hello bottom of hello workbook, or at hello bottom of any section.</span></span>

<span data-ttu-id="d9964-143">Vegyen fel tetszőleges lekérdezések keresztül az Application Insights adatainak munkafüzetek Analytics lekérdezés szakaszok segítségével.</span><span class="sxs-lookup"><span data-stu-id="d9964-143">Analytics query sections let you add arbitrary queries over your Application Insights data into workbooks.</span></span> <span data-ttu-id="d9964-144">Ez azt jelenti, hogy Analytics lekérdezési szakaszok kell lennie a Ugrás-toofor bármely hello felhasználók, a munkamenetek, az események és a megőrzési, például a fent felsorolt négy eltérő a hellyel kapcsolatos kérdések megválaszolásával:</span><span class="sxs-lookup"><span data-stu-id="d9964-144">This flexibility means Analytics query sections should be your go-toofor answering any questions about your site other than hello four listed above for Users, Sessions, Events, and Retention, like:</span></span>

* <span data-ttu-id="d9964-145">Hány kivételek fejeződött a hely throw során hello azonos időszak, a használati csökkenése?</span><span class="sxs-lookup"><span data-stu-id="d9964-145">How many exceptions did your site throw during hello same time period as a decline in usage?</span></span>
* <span data-ttu-id="d9964-146">Mi volt a hello terjesztése lapbetöltési idők néhány lap megtekintésének felhasználók számára?</span><span class="sxs-lookup"><span data-stu-id="d9964-146">What was hello distribution of page load times for users viewing some page?</span></span>
* <span data-ttu-id="d9964-147">Hány felhasználó láthatók a webhely néhány azon lapok készlete, de ez nem valamilyen egyéb lapok?</span><span class="sxs-lookup"><span data-stu-id="d9964-147">How many users viewed some set of pages on your site, but not some other set of pages?</span></span> <span data-ttu-id="d9964-148">Ez akkor lehet hasznos toounderstand, ha a felhasználók, akik használni a webhely funkciók különböző részhalmazai fürttel rendelkezik (hello használata `join` hello operátor `kind=leftanti` módosító a hello Log Analytics lekérdezési nyelv).</span><span class="sxs-lookup"><span data-stu-id="d9964-148">This can be useful toounderstand if you have clusters of users who use different subsets of your site's functionality (use hello `join` operator with hello `kind=leftanti` modifier in hello Log Analytics query language).</span></span>

<span data-ttu-id="d9964-149">Használjon hello [Naplóelemzési lekérdezése nyelvi referencia](https://docs.loganalytics.io/) kapcsolatos lekérdezések írásáról további toolearn.</span><span class="sxs-lookup"><span data-stu-id="d9964-149">Use hello [Log Analytics query language reference](https://docs.loganalytics.io/) toolearn more about writing queries.</span></span>

## <a name="adding-text-and-markdown-sections"></a><span data-ttu-id="d9964-150">Szöveg- és Markdown-szakaszok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d9964-150">Adding text and Markdown sections</span></span>

<span data-ttu-id="d9964-151">A fejlécére kattintva rendezhető, magyarázatot és magyarázatokkal tooyour munkafüzetek hozzáadása segíti a táblázatokat és diagramokat olyan készlete kapcsolja be a leírásokat.</span><span class="sxs-lookup"><span data-stu-id="d9964-151">Adding headings, explanations, and commentary tooyour workbooks helps turn a set of tables and charts into a narrative.</span></span> <span data-ttu-id="d9964-152">Szöveg részeiben munkafüzetek támogatja hello [Markdown-szintaxis](https://daringfireball.net/projects/markdown/) formázás, például a fejlécére kattintva rendezhető, félkövér, dőlt és felsorolásokat szöveg.</span><span class="sxs-lookup"><span data-stu-id="d9964-152">Text sections in workbooks support hello [Markdown syntax](https://daringfireball.net/projects/markdown/) for text formatting, like headings, bold, italics, and bulleted lists.</span></span>

<span data-ttu-id="d9964-153">tooadd szöveg szakasz tooyour munkafüzet, használja a hello **szöveget** gomb hello munkafüzet hello alján, vagy bármely szakasz hello alján.</span><span class="sxs-lookup"><span data-stu-id="d9964-153">tooadd a text section tooyour workbook, use hello **Add text** button at hello bottom of hello workbook, or at hello bottom of any section.</span></span>

## <a name="saving-and-sharing-workbooks-with-your-team"></a><span data-ttu-id="d9964-154">És munkafüzetek megosztása a munkatársaival</span><span class="sxs-lookup"><span data-stu-id="d9964-154">Saving and sharing workbooks with your team</span></span>

<span data-ttu-id="d9964-155">Az Application Insights-erőforrást, vagy hello mentett munkafüzetek **jelentések** titkos tooyou szakaszban vagy hello **megosztott jelentések** hozzáféréssel rendelkező elérhető tooeveryone szakaszban toohello Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="d9964-155">Workbooks are saved within an Application Insights resource, either in hello **My Reports** section that's private tooyou or in hello **Shared Reports** section that's accessible tooeveryone with access toohello Application Insights resource.</span></span> <span data-ttu-id="d9964-156">tooview hello erőforrás összes hello munkafüzeteket kattintson hello **nyitott** hello művelet található gombra.</span><span class="sxs-lookup"><span data-stu-id="d9964-156">tooview all hello workbooks in hello resource, click hello **Open** button in hello action bar.</span></span>

<span data-ttu-id="d9964-157">a munkafüzetet, hogy e tooshare **jelentések**:</span><span class="sxs-lookup"><span data-stu-id="d9964-157">tooshare a workbook that's currently in **My Reports**:</span></span>

1. <span data-ttu-id="d9964-158">Kattintson a **nyitott** hello művelet sávon</span><span class="sxs-lookup"><span data-stu-id="d9964-158">Click **Open** in hello action bar</span></span>
2. <span data-ttu-id="d9964-159">Kattintson a hello "..." gomb melletti hello munkafüzet tooshare kívánt</span><span class="sxs-lookup"><span data-stu-id="d9964-159">Click hello "..." button beside hello workbook you want tooshare</span></span>
3. <span data-ttu-id="d9964-160">Kattintson a **tooShared jelentések áthelyezése**.</span><span class="sxs-lookup"><span data-stu-id="d9964-160">Click **Move tooShared Reports**.</span></span>

<span data-ttu-id="d9964-161">Kattintson egy munkafüzet egy hivatkozás, vagy e-mailben, tooshare **megosztás** hello művelet sávon.</span><span class="sxs-lookup"><span data-stu-id="d9964-161">tooshare a workbook with a link or via email, click **Share** in hello action bar.</span></span> <span data-ttu-id="d9964-162">Ne feledje, hogy hello hivatkozás címzettjei toothis erőforrás hello Azure portál tooview hello munkafüzet kell hozzáférni.</span><span class="sxs-lookup"><span data-stu-id="d9964-162">Keep in mind that recipients of hello link need access toothis resource in hello Azure portal tooview hello workbook.</span></span> <span data-ttu-id="d9964-163">toomake módosításokat címzettek kell legalább hello erőforrás-közreműködői jogokat.</span><span class="sxs-lookup"><span data-stu-id="d9964-163">toomake edits, recipients need at least Contributor permissions for hello resource.</span></span>

<span data-ttu-id="d9964-164">a hivatkozás tooa munkafüzet tooan Azure irányítópult toopin:</span><span class="sxs-lookup"><span data-stu-id="d9964-164">toopin a link tooa workbook tooan Azure Dashboard:</span></span>

1. <span data-ttu-id="d9964-165">Kattintson a **nyitott** hello művelet sávon</span><span class="sxs-lookup"><span data-stu-id="d9964-165">Click **Open** in hello action bar</span></span>
2. <span data-ttu-id="d9964-166">Kattintson a hello "..." gomb melletti hello munkafüzet toopin kívánt</span><span class="sxs-lookup"><span data-stu-id="d9964-166">Click hello "..." button beside hello workbook you want toopin</span></span>
3. <span data-ttu-id="d9964-167">Kattintson a **PIN-kód toodashboard**.</span><span class="sxs-lookup"><span data-stu-id="d9964-167">Click **Pin toodashboard**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d9964-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d9964-168">Next steps</span></span>

## <a name="next-steps"></a><span data-ttu-id="d9964-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d9964-169">Next steps</span></span>
- <span data-ttu-id="d9964-170">tooenable használati észlel, küldésének megkezdése [egyéni események](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) vagy [lapmegtekintés](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span><span class="sxs-lookup"><span data-stu-id="d9964-170">tooenable usage experiences, start sending [custom events](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) or [page views](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span></span>
- <span data-ttu-id="d9964-171">Ha már küld egyéni események vagy Lapmegtekintések, így megismerkedhet hello használati eszközök toolearn hogyan felhasználók használhatja a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d9964-171">If you already send custom events or page views, explore hello Usage tools toolearn how users use your service.</span></span>
    - [<span data-ttu-id="d9964-172">Felhasználók, munkamenetek, események</span><span class="sxs-lookup"><span data-stu-id="d9964-172">Users, Sessions, Events</span></span>](app-insights-usage-segmentation.md)
    - [<span data-ttu-id="d9964-173">Tölcsérek</span><span class="sxs-lookup"><span data-stu-id="d9964-173">Funnels</span></span>](usage-funnels.md)
    - [<span data-ttu-id="d9964-174">Megőrzés</span><span class="sxs-lookup"><span data-stu-id="d9964-174">Retention</span></span>](app-insights-usage-retention.md)
    - [<span data-ttu-id="d9964-175">Felhasználói folyamatok</span><span class="sxs-lookup"><span data-stu-id="d9964-175">User Flows</span></span>](app-insights-usage-flows.md)
    - [<span data-ttu-id="d9964-176">Felhasználói környezet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d9964-176">Add user context</span></span>](app-insights-usage-send-user-context.md)
    
