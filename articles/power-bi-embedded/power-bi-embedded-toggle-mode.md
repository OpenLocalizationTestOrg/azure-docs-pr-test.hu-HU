---
title: "Váltás a nézet és szerkesztési módban a jelentések Azure Power BI Embedded |} Microsoft Docs"
description: "Megtudhatja, hogyan nézet közötti váltáshoz és szerkesztési módban a jelentésekhez a Power BI Embedded belül."
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
ms.openlocfilehash: f73bf05b41523a5833cc9366fb84cb7021b4b7a9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="toggle-between-view-and-edit-mode-for-reports-in-power-bi-embedded"></a><span data-ttu-id="28bf1-103">Váltás a nézet és szerkesztési módban a jelentésekhez a Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="28bf1-103">Toggle between view and edit mode for reports in Power BI Embedded</span></span>

<span data-ttu-id="28bf1-104">Megtudhatja, hogyan nézet közötti váltáshoz és szerkesztési módban a jelentésekhez a Power BI Embedded belül.</span><span class="sxs-lookup"><span data-stu-id="28bf1-104">Learn how to toggle between view and edit mode for your reports within Power BI Embedded.</span></span>

## <a name="creating-an-access-token"></a><span data-ttu-id="28bf1-105">Egy hozzáférési jogkivonat létrehozása</span><span class="sxs-lookup"><span data-stu-id="28bf1-105">Creating an access token</span></span>

<span data-ttu-id="28bf1-106">Szüksége lesz egy hozzáférési jogkivonatot, amely lehetővé teszi a megtekintése és-jelentés szerkesztése létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="28bf1-106">You will need to create an access token that gives you the ability to both view and edit a report.</span></span> <span data-ttu-id="28bf1-107">Szerkesztheti, és mentse a jelentést, szüksége lesz a **Report.ReadWrite** token engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="28bf1-107">To edit and save a report, you will need the **Report.ReadWrite** token permission.</span></span> <span data-ttu-id="28bf1-108">További információkért lásd: [Authenticating és a Power BI Embedded engedélyező](power-bi-embedded-app-token-flow.md).</span><span class="sxs-lookup"><span data-stu-id="28bf1-108">For more information, see [Authenticating and authorizing in Power BI Embedded](power-bi-embedded-app-token-flow.md).</span></span>

> [!NOTE]
> <span data-ttu-id="28bf1-109">Ez lehetővé teszi, szerkesztése és a meglévő jelentés módosításainak mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="28bf1-109">This will allow you to edit and save changes to an existing report.</span></span> <span data-ttu-id="28bf1-110">Ha azt is szeretné funkciót támogató **Mentés másként**, akkor adja meg a további engedélyek.</span><span class="sxs-lookup"><span data-stu-id="28bf1-110">If you would also like the function of supporting **Save As**, you will need to supply additional permissions.</span></span> <span data-ttu-id="28bf1-111">További információkért lásd: [hatókörök](power-bi-embedded-app-token-flow.md#scopes).</span><span class="sxs-lookup"><span data-stu-id="28bf1-111">For more information, see [Scopes](power-bi-embedded-app-token-flow.md#scopes).</span></span>

```
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Report.ReadWrite";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="embed-configuration"></a><span data-ttu-id="28bf1-112">Konfigurációs beágyazása</span><span class="sxs-lookup"><span data-stu-id="28bf1-112">Embed configuration</span></span>

<span data-ttu-id="28bf1-113">Szüksége lesz az engedélyek és a viewMode láthatók a Mentés gombra kattint, szerkesztési módban.</span><span class="sxs-lookup"><span data-stu-id="28bf1-113">You will need to supply permissions and a viewMode in order to see the save button when in edit mode.</span></span> <span data-ttu-id="28bf1-114">További információkért lásd: [konfigurációs részletek beágyazása](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).</span><span class="sxs-lookup"><span data-stu-id="28bf1-114">For more information, see [Embed configuration details](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).</span></span>

<span data-ttu-id="28bf1-115">Ha például a JavaScript:</span><span class="sxs-lookup"><span data-stu-id="28bf1-115">For example in JavaScript:</span></span>

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
        permissions: models.Permissions.ReadWrite /*both save & save as buttons will be visible*/,
        viewMode: models.ViewMode.View,
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

<span data-ttu-id="28bf1-116">Ez a jelentés beágyazása a megjelenítési mód alapján fogják **viewMode** állítják be **modellek. ViewMode.View**.</span><span class="sxs-lookup"><span data-stu-id="28bf1-116">This will indicate to embed the report in view mode based on **viewMode** being set to **models.ViewMode.View**.</span></span>

## <a name="view-mode"></a><span data-ttu-id="28bf1-117">Megjelenítési mód</span><span class="sxs-lookup"><span data-stu-id="28bf1-117">View mode</span></span>

<span data-ttu-id="28bf1-118">A következő JavaScript használatával nézet módba kapcsolni, ha Ön a szerkesztési módban.</span><span class="sxs-lookup"><span data-stu-id="28bf1-118">You can use the following JavaScript to switch into view mode, if you are in edit mode.</span></span>

```
// Get a reference to the embedded report HTML element
var reportContainer = $('#reportContainer')[0];

// Get a reference to the embedded report.
report = powerbi.get(reportContainer);

// Switch to view mode.
report.switchMode("view");

```

## <a name="edit-mode"></a><span data-ttu-id="28bf1-119">Szerkesztési módban</span><span class="sxs-lookup"><span data-stu-id="28bf1-119">Edit mode</span></span>

<span data-ttu-id="28bf1-120">A következő JavaScript segítségével átváltani a szerkesztési módot, ha megtekintési módban.</span><span class="sxs-lookup"><span data-stu-id="28bf1-120">You can use the following JavaScript to switch into edit mode, if you are in view mode.</span></span>

```
// Get a reference to the embedded report HTML element
var reportContainer = $('#reportContainer')[0];

// Get a reference to the embedded report.
report = powerbi.get(reportContainer);

// Switch to edit mode.
report.switchMode("edit");

```

## <a name="see-also"></a><span data-ttu-id="28bf1-121">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="28bf1-121">See also</span></span>

[<span data-ttu-id="28bf1-122">Bevezetés a minta használatába</span><span class="sxs-lookup"><span data-stu-id="28bf1-122">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="28bf1-123">Jelentés beágyazása</span><span class="sxs-lookup"><span data-stu-id="28bf1-123">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
[<span data-ttu-id="28bf1-124">Hitelesítés és engedélyezés a Power BI Embedded használatával</span><span class="sxs-lookup"><span data-stu-id="28bf1-124">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="28bf1-125">CreateReportEmbedToken</span><span class="sxs-lookup"><span data-stu-id="28bf1-125">CreateReportEmbedToken</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[<span data-ttu-id="28bf1-126">JavaScript beágyazási minta</span><span class="sxs-lookup"><span data-stu-id="28bf1-126">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[<span data-ttu-id="28bf1-127">A csharp nyelvű Power bi Git-tárház</span><span class="sxs-lookup"><span data-stu-id="28bf1-127">PowerBI-CSharp Git Repo</span></span>](https://github.com/Microsoft/PowerBI-CSharp)  
[<span data-ttu-id="28bf1-128">Power bi-csomópont Git-tárház</span><span class="sxs-lookup"><span data-stu-id="28bf1-128">PowerBI-Node Git Repo</span></span>](https://github.com/Microsoft/PowerBI-Node)  
<span data-ttu-id="28bf1-129">További kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="28bf1-129">More questions?</span></span> [<span data-ttu-id="28bf1-130">Tegye próbára a Power BI közösségét</span><span class="sxs-lookup"><span data-stu-id="28bf1-130">Try the Power BI Community</span></span>](http://community.powerbi.com/)
