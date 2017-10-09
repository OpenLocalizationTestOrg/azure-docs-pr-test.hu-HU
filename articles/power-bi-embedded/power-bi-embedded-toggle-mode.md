---
title: "a jelentések Azure Power BI Embedded megtekintés és szerkesztés mód közötti aaaToggle |} Microsoft Docs"
description: "Megtudhatja, hogyan tootoggle a jelentésekhez a Power BI Embedded belül megtekintés és szerkesztés mód között."
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
ms.openlocfilehash: c9e3da5f9ae74d221af650adebde7c9d83b38a99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="toggle-between-view-and-edit-mode-for-reports-in-power-bi-embedded"></a><span data-ttu-id="cd397-103">Váltás a nézet és szerkesztési módban a jelentésekhez a Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="cd397-103">Toggle between view and edit mode for reports in Power BI Embedded</span></span>

<span data-ttu-id="cd397-104">Megtudhatja, hogyan tootoggle a jelentésekhez a Power BI Embedded belül megtekintés és szerkesztés mód között.</span><span class="sxs-lookup"><span data-stu-id="cd397-104">Learn how tootoggle between view and edit mode for your reports within Power BI Embedded.</span></span>

## <a name="creating-an-access-token"></a><span data-ttu-id="cd397-105">Egy hozzáférési jogkivonat létrehozása</span><span class="sxs-lookup"><span data-stu-id="cd397-105">Creating an access token</span></span>

<span data-ttu-id="cd397-106">Szüksége lesz egy hozzáférési jogkivonatot, amely lehetővé teszi az hello képességét tooboth megtekintés és Szerkesztés jelentés toocreate.</span><span class="sxs-lookup"><span data-stu-id="cd397-106">You will need toocreate an access token that gives you hello ability tooboth view and edit a report.</span></span> <span data-ttu-id="cd397-107">tooedit és mentse a jelentést, szüksége lesz a hello **Report.ReadWrite** token engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="cd397-107">tooedit and save a report, you will need hello **Report.ReadWrite** token permission.</span></span> <span data-ttu-id="cd397-108">További információkért lásd: [Authenticating és a Power BI Embedded engedélyező](power-bi-embedded-app-token-flow.md).</span><span class="sxs-lookup"><span data-stu-id="cd397-108">For more information, see [Authenticating and authorizing in Power BI Embedded](power-bi-embedded-app-token-flow.md).</span></span>

> [!NOTE]
> <span data-ttu-id="cd397-109">Ez lehetővé teszi tooedit, és tooan meglévő jelentés módosítások mentése.</span><span class="sxs-lookup"><span data-stu-id="cd397-109">This will allow you tooedit and save changes tooan existing report.</span></span> <span data-ttu-id="cd397-110">Ha azt is szeretné hello funkciót támogató **Mentés másként**, szüksége lesz a toosupply további engedélyek.</span><span class="sxs-lookup"><span data-stu-id="cd397-110">If you would also like hello function of supporting **Save As**, you will need toosupply additional permissions.</span></span> <span data-ttu-id="cd397-111">További információkért lásd: [hatókörök](power-bi-embedded-app-token-flow.md#scopes).</span><span class="sxs-lookup"><span data-stu-id="cd397-111">For more information, see [Scopes](power-bi-embedded-app-token-flow.md#scopes).</span></span>

```
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Report.ReadWrite";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="embed-configuration"></a><span data-ttu-id="cd397-112">Konfigurációs beágyazása</span><span class="sxs-lookup"><span data-stu-id="cd397-112">Embed configuration</span></span>

<span data-ttu-id="cd397-113">Toosupply engedélyek és a sorrend toosee hello mentési gomb szerkesztési módban a viewMode lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="cd397-113">You will need toosupply permissions and a viewMode in order toosee hello save button when in edit mode.</span></span> <span data-ttu-id="cd397-114">További információkért lásd: [konfigurációs részletek beágyazása](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).</span><span class="sxs-lookup"><span data-stu-id="cd397-114">For more information, see [Embed configuration details](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).</span></span>

<span data-ttu-id="cd397-115">Ha például a JavaScript:</span><span class="sxs-lookup"><span data-stu-id="cd397-115">For example in JavaScript:</span></span>

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
        permissions: models.Permissions.ReadWrite /*both save & save as buttons will be visible*/,
        viewMode: models.ViewMode.View,
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

<span data-ttu-id="cd397-116">Ez tájékoztatja tooembed hello jelentés alapján nézetét **viewMode** túl állítják**modellek. ViewMode.View**.</span><span class="sxs-lookup"><span data-stu-id="cd397-116">This will indicate tooembed hello report in view mode based on **viewMode** being set too**models.ViewMode.View**.</span></span>

## <a name="view-mode"></a><span data-ttu-id="cd397-117">Megjelenítési mód</span><span class="sxs-lookup"><span data-stu-id="cd397-117">View mode</span></span>

<span data-ttu-id="cd397-118">JavaScript tooswitch nézet módba a következő hello is használhatja, ha szerkesztési módban.</span><span class="sxs-lookup"><span data-stu-id="cd397-118">You can use hello following JavaScript tooswitch into view mode, if you are in edit mode.</span></span>

```
// Get a reference toohello embedded report HTML element
var reportContainer = $('#reportContainer')[0];

// Get a reference toohello embedded report.
report = powerbi.get(reportContainer);

// Switch tooview mode.
report.switchMode("view");

```

## <a name="edit-mode"></a><span data-ttu-id="cd397-119">Szerkesztési módban</span><span class="sxs-lookup"><span data-stu-id="cd397-119">Edit mode</span></span>

<span data-ttu-id="cd397-120">JavaScript tooswitch szerkesztési módba a következő hello is használhatja, ha megtekintési módban.</span><span class="sxs-lookup"><span data-stu-id="cd397-120">You can use hello following JavaScript tooswitch into edit mode, if you are in view mode.</span></span>

```
// Get a reference toohello embedded report HTML element
var reportContainer = $('#reportContainer')[0];

// Get a reference toohello embedded report.
report = powerbi.get(reportContainer);

// Switch tooedit mode.
report.switchMode("edit");

```

## <a name="see-also"></a><span data-ttu-id="cd397-121">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="cd397-121">See also</span></span>

[<span data-ttu-id="cd397-122">Bevezetés a minta használatába</span><span class="sxs-lookup"><span data-stu-id="cd397-122">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="cd397-123">Jelentés beágyazása</span><span class="sxs-lookup"><span data-stu-id="cd397-123">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
[<span data-ttu-id="cd397-124">Hitelesítés és engedélyezés a Power BI Embedded használatával</span><span class="sxs-lookup"><span data-stu-id="cd397-124">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="cd397-125">CreateReportEmbedToken</span><span class="sxs-lookup"><span data-stu-id="cd397-125">CreateReportEmbedToken</span></span>](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[<span data-ttu-id="cd397-126">JavaScript beágyazási minta</span><span class="sxs-lookup"><span data-stu-id="cd397-126">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[<span data-ttu-id="cd397-127">A csharp nyelvű Power bi Git-tárház</span><span class="sxs-lookup"><span data-stu-id="cd397-127">PowerBI-CSharp Git Repo</span></span>](https://github.com/Microsoft/PowerBI-CSharp)  
[<span data-ttu-id="cd397-128">Power bi-csomópont Git-tárház</span><span class="sxs-lookup"><span data-stu-id="cd397-128">PowerBI-Node Git Repo</span></span>](https://github.com/Microsoft/PowerBI-Node)  
<span data-ttu-id="cd397-129">További kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="cd397-129">More questions?</span></span> [<span data-ttu-id="cd397-130">Próbálja meg a Power BI-Közösség hello</span><span class="sxs-lookup"><span data-stu-id="cd397-130">Try hello Power BI Community</span></span>](http://community.powerbi.com/)
