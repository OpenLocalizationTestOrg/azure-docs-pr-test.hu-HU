---
title: "az Azure Power BI Embedded aaaSave jelentések |} Microsoft Docs"
description: "Ismerje meg, hogyan toosave jelentések belül a Power BI embedded. Sikeresen ehhez rendelés toowork a megfelelő engedélyekkel."
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
ms.openlocfilehash: 984537ce1ce1afc787d6c6c9f61ae8d6226d1171
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="save-reports-in-power-bi-embedded"></a><span data-ttu-id="d90fc-104">A Power BI Embedded jelentések menthetők</span><span class="sxs-lookup"><span data-stu-id="d90fc-104">Save reports in Power BI Embedded</span></span>

<span data-ttu-id="d90fc-105">Ismerje meg, hogyan toosave jelentések belül a Power BI embedded.</span><span class="sxs-lookup"><span data-stu-id="d90fc-105">Learn how toosave reports within Power BI embedded.</span></span> <span data-ttu-id="d90fc-106">Sikeresen ehhez rendelés toowork a megfelelő engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="d90fc-106">This requires proper permissions in order toowork successfully.</span></span>

<span data-ttu-id="d90fc-107">Power BI Embedded belül szerkesztheti a meglévő jelentéseket, és mentse őket.</span><span class="sxs-lookup"><span data-stu-id="d90fc-107">Within Power BI Embedded, you can edit existing reports and save them.</span></span> <span data-ttu-id="d90fc-108">Új jelentés létrehozása is, és elmentse egy új jelentés toocreate egyet.</span><span class="sxs-lookup"><span data-stu-id="d90fc-108">You can also create a new report and save as a new report toocreate one.</span></span>

<span data-ttu-id="d90fc-109">A sorrend toosave jelentés először kell toocreate jogkivonat hello adott jelentés hello jobb hatókörök:</span><span class="sxs-lookup"><span data-stu-id="d90fc-109">In order toosave a report you first need toocreate a token for hello specific report with hello right scopes:</span></span>

* <span data-ttu-id="d90fc-110">Mentés Report.ReadWrite hatókör tooenable szükség</span><span class="sxs-lookup"><span data-stu-id="d90fc-110">tooenable save Report.ReadWrite scope is required</span></span>
* <span data-ttu-id="d90fc-111">Mentés másként tooenable, Report.Read és Workspace.Report.Copy hatókörben szükség</span><span class="sxs-lookup"><span data-stu-id="d90fc-111">tooenable save as, Report.Read and Workspace.Report.Copy scopes are required</span></span>
* <span data-ttu-id="d90fc-112">tooenable mentése, valamint Mentés másként, Report.ReadWrite és Workspace.Report.Copy szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="d90fc-112">tooenable save and save as, Report.ReadWrite and Workspace.Report.Copy are requierd</span></span>

<span data-ttu-id="d90fc-113">Illetve rendelés tooenable hello jobb save/mentése a gombok fájl menü, mivel kell tooprovide hello jobb gombbal hello beágyazási konfigurációs engedélyt amikor Ön beágyazási hello jelentés:</span><span class="sxs-lookup"><span data-stu-id="d90fc-113">Respectively in order tooenable hello right save/save as buttons in file menu you need tooprovide hello right permission in hello Embed configuration when you Embed hello report:</span></span>

* <span data-ttu-id="d90fc-114">modellek. Permissions.ReadWrite</span><span class="sxs-lookup"><span data-stu-id="d90fc-114">models.Permissions.ReadWrite</span></span>
* <span data-ttu-id="d90fc-115">modellek. Permissions.Copy</span><span class="sxs-lookup"><span data-stu-id="d90fc-115">models.Permissions.Copy</span></span>
* <span data-ttu-id="d90fc-116">modellek. Permissions.All</span><span class="sxs-lookup"><span data-stu-id="d90fc-116">models.Permissions.All</span></span>

> [!NOTE]
> <span data-ttu-id="d90fc-117">A hozzáférési token kell hello megfelelő hatókörök.</span><span class="sxs-lookup"><span data-stu-id="d90fc-117">Your access token also needs hello appropriate scopes.</span></span> <span data-ttu-id="d90fc-118">További információkért lásd: [hatókörök](power-bi-embedded-app-token-flow.md#scopes).</span><span class="sxs-lookup"><span data-stu-id="d90fc-118">For more information, see [Scopes](power-bi-embedded-app-token-flow.md#scopes).</span></span>

## <a name="embed-report-in-edit-mode"></a><span data-ttu-id="d90fc-119">Jelentés beágyazása a szerkesztési módban</span><span class="sxs-lookup"><span data-stu-id="d90fc-119">Embed report in edit mode</span></span>

<span data-ttu-id="d90fc-120">Most mondja ki meg tooEmbed szerkesztési módban a jelentés az alkalmazás belül, toodo csak hello engedély tulajdonságai beágyazási konfigurációs adjon át, és hívja meg powerbi.embed().</span><span class="sxs-lookup"><span data-stu-id="d90fc-120">Let's say you want tooEmbed a report in edit mode inside your app, toodo so just pass hello right properties in Embed configuration and call powerbi.embed().</span></span> <span data-ttu-id="d90fc-121">Szüksége lesz toosupply engedélyek és a sorrend toosee hello mentés a viewMode gombként szerkesztési módban.</span><span class="sxs-lookup"><span data-stu-id="d90fc-121">You will need toosupply permissions and a viewMode in order toosee hello save and save as buttons when in edit mode.</span></span> <span data-ttu-id="d90fc-122">További információkért lásd: [konfigurációs részletek beágyazása](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).</span><span class="sxs-lookup"><span data-stu-id="d90fc-122">For more information, see [Embed configuration details](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).</span></span>

<span data-ttu-id="d90fc-123">Ha például a JavaScript:</span><span class="sxs-lookup"><span data-stu-id="d90fc-123">For example in JavaScript:</span></span>

```
   <div id="reportContainer"></div>

    // Get models. Models, it contains enums that can be used.
    var models = window['powerbi-client'].models;

    // Embed configuration used toodescribe hello what and how tooembed.
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

    // Get a reference toohello embedded report HTML element
    var reportContainer = $('#reportContainer')[0];

    // Embed hello report and display it within hello div container.
    var report = powerbi.embed(reportContainer, config);
```

<span data-ttu-id="d90fc-124">Most egy jelentés rendszer beágyazza az alkalmazás szerkesztési módban.</span><span class="sxs-lookup"><span data-stu-id="d90fc-124">Now a report will be embedded in your app in edit mode.</span></span>

## <a name="save-report"></a><span data-ttu-id="d90fc-125">Jelentés mentése</span><span class="sxs-lookup"><span data-stu-id="d90fc-125">Save report</span></span>

<span data-ttu-id="d90fc-126">Miután Embbeding hello jelentést a szerkesztési módban hello jobb token és engedélyeket hello jelentés mentheti, hello fájl menüből, vagy JavaScript:</span><span class="sxs-lookup"><span data-stu-id="d90fc-126">After Embbeding hello report in edit mode with hello right token and permissions you can save hello report from hello file menu or from javascript:</span></span>

```
 // Get a reference toohello embedded report.
    report = powerbi.get(reportContainer);

 // Save report
    report.save();
```

## <a name="save-as"></a><span data-ttu-id="d90fc-127">Mentés másként</span><span class="sxs-lookup"><span data-stu-id="d90fc-127">Save as</span></span>

```
// Get a reference toohello embedded report.
    report = powerbi.get(reportContainer);
    
    var saveAsParameters = {
        name: "newReport"
    };

    // SaveAs report
    report.saveAs(saveAsParameters);
```

> [!IMPORTANT]
> <span data-ttu-id="d90fc-128">Csak azután *Mentés másként* egy új jelentés létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d90fc-128">Only after *save as* is a new report created.</span></span> <span data-ttu-id="d90fc-129">Hello menteni, miután vásznon a hello továbbra is látható hello régi jelentés a szerkesztési módot, és nem hello új jelentés.</span><span class="sxs-lookup"><span data-stu-id="d90fc-129">After hello save, hello canvas is still showing hello old report in edit mode and not hello new report.</span></span> <span data-ttu-id="d90fc-130">Szüksége lesz tooembed hello új jelentés létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d90fc-130">You will need tooembed hello new report that was created.</span></span> <span data-ttu-id="d90fc-131">Ehhez egy új hozzáférési jogkivonat-jelentés létrehozásához szükségesek.</span><span class="sxs-lookup"><span data-stu-id="d90fc-131">This requires a new access token as they are created per report.</span></span>

<span data-ttu-id="d90fc-132">Tooload hello új jelentés után kell egy *Mentés másként*.</span><span class="sxs-lookup"><span data-stu-id="d90fc-132">You will then need tooload hello new report after a *save as*.</span></span> <span data-ttu-id="d90fc-133">Ez hasonló tooembedding jelentései van.</span><span class="sxs-lookup"><span data-stu-id="d90fc-133">This is similar tooembedding any report.</span></span>

```
<div id="reportContainer"></div>
  
var embedConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MJ',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        reportId: '5dac7a4a-4452-46b3-99f6-a25915e0fe54',
    };
    
    // Grab hello reference toohello div HTML element that will host hello report
    var reportContainer = $('#reportContainer')[0];

    // Embed report
    var report = powerbi.embed(reportContainer, embedConfiguration);
```

## <a name="see-also"></a><span data-ttu-id="d90fc-134">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="d90fc-134">See also</span></span>

[<span data-ttu-id="d90fc-135">Bevezetés a minta használatába</span><span class="sxs-lookup"><span data-stu-id="d90fc-135">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="d90fc-136">Jelentés beágyazása</span><span class="sxs-lookup"><span data-stu-id="d90fc-136">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
[<span data-ttu-id="d90fc-137">Új jelentés létrehozása adatkészletből</span><span class="sxs-lookup"><span data-stu-id="d90fc-137">Create a new report from a dataset</span></span>](power-bi-embedded-create-report-from-dataset.md)  
[<span data-ttu-id="d90fc-138">Hitelesítés és engedélyezés a Power BI Embedded használatával</span><span class="sxs-lookup"><span data-stu-id="d90fc-138">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="d90fc-139">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="d90fc-139">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="d90fc-140">JavaScript beágyazási minta</span><span class="sxs-lookup"><span data-stu-id="d90fc-140">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
<span data-ttu-id="d90fc-141">További kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="d90fc-141">More questions?</span></span> [<span data-ttu-id="d90fc-142">Próbálja meg a Power BI-Közösség hello</span><span class="sxs-lookup"><span data-stu-id="d90fc-142">Try hello Power BI Community</span></span>](http://community.powerbi.com/)

