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
# <a name="toggle-between-view-and-edit-mode-for-reports-in-power-bi-embedded"></a>Váltás a nézet és szerkesztési módban a jelentésekhez a Power BI Embedded

Megtudhatja, hogyan tootoggle a jelentésekhez a Power BI Embedded belül megtekintés és szerkesztés mód között.

## <a name="creating-an-access-token"></a>Egy hozzáférési jogkivonat létrehozása

Szüksége lesz egy hozzáférési jogkivonatot, amely lehetővé teszi az hello képességét tooboth megtekintés és Szerkesztés jelentés toocreate. tooedit és mentse a jelentést, szüksége lesz a hello **Report.ReadWrite** token engedéllyel. További információkért lásd: [Authenticating és a Power BI Embedded engedélyező](power-bi-embedded-app-token-flow.md).

> [!NOTE]
> Ez lehetővé teszi tooedit, és tooan meglévő jelentés módosítások mentése. Ha azt is szeretné hello funkciót támogató **Mentés másként**, szüksége lesz a toosupply további engedélyek. További információkért lásd: [hatókörök](power-bi-embedded-app-token-flow.md#scopes).

```
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Report.ReadWrite";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="embed-configuration"></a>Konfigurációs beágyazása

Toosupply engedélyek és a sorrend toosee hello mentési gomb szerkesztési módban a viewMode lesz szüksége. További információkért lásd: [konfigurációs részletek beágyazása](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).

Ha például a JavaScript:

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

Ez tájékoztatja tooembed hello jelentés alapján nézetét **viewMode** túl állítják**modellek. ViewMode.View**.

## <a name="view-mode"></a>Megjelenítési mód

JavaScript tooswitch nézet módba a következő hello is használhatja, ha szerkesztési módban.

```
// Get a reference toohello embedded report HTML element
var reportContainer = $('#reportContainer')[0];

// Get a reference toohello embedded report.
report = powerbi.get(reportContainer);

// Switch tooview mode.
report.switchMode("view");

```

## <a name="edit-mode"></a>Szerkesztési módban

JavaScript tooswitch szerkesztési módba a következő hello is használhatja, ha megtekintési módban.

```
// Get a reference toohello embedded report HTML element
var reportContainer = $('#reportContainer')[0];

// Get a reference toohello embedded report.
report = powerbi.get(reportContainer);

// Switch tooedit mode.
report.switchMode("edit");

```

## <a name="see-also"></a>Lásd még:

[Bevezetés a minta használatába](power-bi-embedded-get-started-sample.md)  
[Jelentés beágyazása](power-bi-embedded-embed-report.md)  
[Hitelesítés és engedélyezés a Power BI Embedded használatával](power-bi-embedded-app-token-flow.md)  
[CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_)  
[JavaScript beágyazási minta](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[A csharp nyelvű Power bi Git-tárház](https://github.com/Microsoft/PowerBI-CSharp)  
[Power bi-csomópont Git-tárház](https://github.com/Microsoft/PowerBI-Node)  
További kérdései vannak? [Próbálja meg a Power BI-Közösség hello](http://community.powerbi.com/)
