---
title: "Az Azure Power BI Embedded jelentések menthetők |} Microsoft Docs"
description: "Tudnivalók a Power BI embedded belül jelentések menthetők. Ehhez szükséges, hogy megfelelő engedélyekkel ahhoz, hogy sikeresen működjön."
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
ms.openlocfilehash: ad895004cc2972f2ded81566186325a16d401151
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="save-reports-in-power-bi-embedded"></a><span data-ttu-id="568d2-104">A Power BI Embedded jelentések menthetők</span><span class="sxs-lookup"><span data-stu-id="568d2-104">Save reports in Power BI Embedded</span></span>

<span data-ttu-id="568d2-105">Tudnivalók a Power BI embedded belül jelentések menthetők.</span><span class="sxs-lookup"><span data-stu-id="568d2-105">Learn how to save reports within Power BI embedded.</span></span> <span data-ttu-id="568d2-106">Ehhez szükséges, hogy megfelelő engedélyekkel ahhoz, hogy sikeresen működjön.</span><span class="sxs-lookup"><span data-stu-id="568d2-106">This requires proper permissions in order to work successfully.</span></span>

<span data-ttu-id="568d2-107">Power BI Embedded belül szerkesztheti a meglévő jelentéseket, és mentse őket.</span><span class="sxs-lookup"><span data-stu-id="568d2-107">Within Power BI Embedded, you can edit existing reports and save them.</span></span> <span data-ttu-id="568d2-108">Is új jelentés létrehozása és mentése új jelentésként kattintva létrehozhat egyet.</span><span class="sxs-lookup"><span data-stu-id="568d2-108">You can also create a new report and save as a new report to create one.</span></span>

<span data-ttu-id="568d2-109">Ahhoz, hogy a jelentés mentése először kell a megfelelő hatókörként az adott jelentés jogkivonat létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="568d2-109">In order to save a report you first need to create a token for the specific report with the right scopes:</span></span>

* <span data-ttu-id="568d2-110">Engedélyezi a Mentés Report.ReadWrite hatókörben szükség</span><span class="sxs-lookup"><span data-stu-id="568d2-110">To enable save Report.ReadWrite scope is required</span></span>
* <span data-ttu-id="568d2-111">Ha engedélyezi a Mentés másként, Report.Read és Workspace.Report.Copy hatókörben szükség</span><span class="sxs-lookup"><span data-stu-id="568d2-111">To enable save as, Report.Read and Workspace.Report.Copy scopes are required</span></span>
* <span data-ttu-id="568d2-112">Engedélyezi a Mentés másként, Report.ReadWrite Workspace.Report.Copy és szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="568d2-112">To enable save and save as, Report.ReadWrite and Workspace.Report.Copy are requierd</span></span>

<span data-ttu-id="568d2-113">Illetve ahhoz, hogy a jobb oldali gombok fájl menü meg kell adnia a beágyazási konfigurációjában a megfelelő engedélye a jelentés beágyazása a save/mentés:</span><span class="sxs-lookup"><span data-stu-id="568d2-113">Respectively in order to enable the right save/save as buttons in file menu you need to provide the right permission in the Embed configuration when you Embed the report:</span></span>

* <span data-ttu-id="568d2-114">modellek. Permissions.ReadWrite</span><span class="sxs-lookup"><span data-stu-id="568d2-114">models.Permissions.ReadWrite</span></span>
* <span data-ttu-id="568d2-115">modellek. Permissions.Copy</span><span class="sxs-lookup"><span data-stu-id="568d2-115">models.Permissions.Copy</span></span>
* <span data-ttu-id="568d2-116">modellek. Permissions.All</span><span class="sxs-lookup"><span data-stu-id="568d2-116">models.Permissions.All</span></span>

> [!NOTE]
> <span data-ttu-id="568d2-117">A hozzáférési token is kell a megfelelő hatókörök.</span><span class="sxs-lookup"><span data-stu-id="568d2-117">Your access token also needs the appropriate scopes.</span></span> <span data-ttu-id="568d2-118">További információkért lásd: [hatókörök](power-bi-embedded-app-token-flow.md#scopes).</span><span class="sxs-lookup"><span data-stu-id="568d2-118">For more information, see [Scopes](power-bi-embedded-app-token-flow.md#scopes).</span></span>

## <a name="embed-report-in-edit-mode"></a><span data-ttu-id="568d2-119">Jelentés beágyazása a szerkesztési módban</span><span class="sxs-lookup"><span data-stu-id="568d2-119">Embed report in edit mode</span></span>

<span data-ttu-id="568d2-120">Tegyük fel, jelentés beágyazása szerkesztési módban az alkalmazásban, csak adja át a megfelelő tulajdonságok beágyazási konfigurációban, és hívja meg powerbi.embed() belül kívánja.</span><span class="sxs-lookup"><span data-stu-id="568d2-120">Let's say you want to Embed a report in edit mode inside your app, to do so just pass the right properties in Embed configuration and call powerbi.embed().</span></span> <span data-ttu-id="568d2-121">Engedélyek és a viewMode láthatók a mentési szüksége lesz, és menteni, ha a gomb szerkesztési módban.</span><span class="sxs-lookup"><span data-stu-id="568d2-121">You will need to supply permissions and a viewMode in order to see the save and save as buttons when in edit mode.</span></span> <span data-ttu-id="568d2-122">További információkért lásd: [konfigurációs részletek beágyazása](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).</span><span class="sxs-lookup"><span data-stu-id="568d2-122">For more information, see [Embed configuration details](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).</span></span>

<span data-ttu-id="568d2-123">Ha például a JavaScript:</span><span class="sxs-lookup"><span data-stu-id="568d2-123">For example in JavaScript:</span></span>

```
   <div id="reportContainer"></div>

    // Get models. Models, it contains enums that can be used.
    var models = window['powerbi-client'].models;

    // Embed configuration used to describe the what and how to embed.
    // This object is used when calling powerbi.embed.
    // This also includes settings and options such as filters.
    // You can find more information at https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details.
    var config= {
        type: 'report',
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        id:  '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
        permissions: models.Permissions.All /*both save & save as buttons will be visible*/,
        viewMode: models.ViewMode.Edit,
        settings: {
            filterPaneEnabled: true,
            navContentPaneEnabled: true
        }
    };

    // Get a reference to the embedded report HTML element
    var reportContainer = $('#reportContainer')[0];

    // Embed the report and display it within the div container.
    var report = powerbi.embed(reportContainer, config);
```

<span data-ttu-id="568d2-124">Most egy jelentés rendszer beágyazza az alkalmazás szerkesztési módban.</span><span class="sxs-lookup"><span data-stu-id="568d2-124">Now a report will be embedded in your app in edit mode.</span></span>

## <a name="save-report"></a><span data-ttu-id="568d2-125">Jelentés mentése</span><span class="sxs-lookup"><span data-stu-id="568d2-125">Save report</span></span>

<span data-ttu-id="568d2-126">Embbeding után a a jelentést a szerkesztési módban a jobb oldali token és mentheti a jelentést, a Fájl menüből, vagy JavaScript engedélyeket:</span><span class="sxs-lookup"><span data-stu-id="568d2-126">After Embbeding the report in edit mode with the right token and permissions you can save the report from the file menu or from javascript:</span></span>

```
 // Get a reference to the embedded report.
    report = powerbi.get(reportContainer);

 // Save report
    report.save();
```

## <a name="save-as"></a><span data-ttu-id="568d2-127">Mentés másként</span><span class="sxs-lookup"><span data-stu-id="568d2-127">Save as</span></span>

```
// Get a reference to the embedded report.
    report = powerbi.get(reportContainer);
    
    var saveAsParameters = {
        name: "newReport"
    };

    // SaveAs report
    report.saveAs(saveAsParameters);
```

> [!IMPORTANT]
> <span data-ttu-id="568d2-128">Csak azután *Mentés másként* egy új jelentés létrehozása.</span><span class="sxs-lookup"><span data-stu-id="568d2-128">Only after *save as* is a new report created.</span></span> <span data-ttu-id="568d2-129">A mentés után a vászon továbbra is azt a régi jelentés szerkesztési módban, és nem az új jelentést.</span><span class="sxs-lookup"><span data-stu-id="568d2-129">After the save, the canvas is still showing the old report in edit mode and not the new report.</span></span> <span data-ttu-id="568d2-130">Szüksége lesz a létrehozott új jelentés beágyazása.</span><span class="sxs-lookup"><span data-stu-id="568d2-130">You will need to embed the new report that was created.</span></span> <span data-ttu-id="568d2-131">Ehhez egy új hozzáférési jogkivonat-jelentés létrehozásához szükségesek.</span><span class="sxs-lookup"><span data-stu-id="568d2-131">This requires a new access token as they are created per report.</span></span>

<span data-ttu-id="568d2-132">Majd szüksége lesz a betöltése után az új jelentést a *Mentés másként*.</span><span class="sxs-lookup"><span data-stu-id="568d2-132">You will then need to load the new report after a *save as*.</span></span> <span data-ttu-id="568d2-133">Ez hasonlít bármely jelentés beágyazása.</span><span class="sxs-lookup"><span data-stu-id="568d2-133">This is similar to embedding any report.</span></span>

```
<div id="reportContainer"></div>
  
var embedConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MJ',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        reportId: '5dac7a4a-4452-46b3-99f6-a25915e0fe54',
    };
    
    // Grab the reference to the div HTML element that will host the report
    var reportContainer = $('#reportContainer')[0];

    // Embed report
    var report = powerbi.embed(reportContainer, embedConfiguration);
```

## <a name="see-also"></a><span data-ttu-id="568d2-134">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="568d2-134">See also</span></span>

[<span data-ttu-id="568d2-135">Bevezetés a minta használatába</span><span class="sxs-lookup"><span data-stu-id="568d2-135">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="568d2-136">Jelentés beágyazása</span><span class="sxs-lookup"><span data-stu-id="568d2-136">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
[<span data-ttu-id="568d2-137">Új jelentés létrehozása adatkészletből</span><span class="sxs-lookup"><span data-stu-id="568d2-137">Create a new report from a dataset</span></span>](power-bi-embedded-create-report-from-dataset.md)  
[<span data-ttu-id="568d2-138">Hitelesítés és engedélyezés a Power BI Embedded használatával</span><span class="sxs-lookup"><span data-stu-id="568d2-138">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="568d2-139">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="568d2-139">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="568d2-140">JavaScript beágyazási minta</span><span class="sxs-lookup"><span data-stu-id="568d2-140">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
<span data-ttu-id="568d2-141">További kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="568d2-141">More questions?</span></span> [<span data-ttu-id="568d2-142">Tegye próbára a Power BI közösségét</span><span class="sxs-lookup"><span data-stu-id="568d2-142">Try the Power BI Community</span></span>](http://community.powerbi.com/)

