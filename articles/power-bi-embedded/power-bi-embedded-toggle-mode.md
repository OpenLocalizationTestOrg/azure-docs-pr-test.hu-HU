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
# <a name="toggle-between-view-and-edit-mode-for-reports-in-power-bi-embedded"></a>Váltás a nézet és szerkesztési módban a jelentésekhez a Power BI Embedded

Megtudhatja, hogyan nézet közötti váltáshoz és szerkesztési módban a jelentésekhez a Power BI Embedded belül.

## <a name="creating-an-access-token"></a>Egy hozzáférési jogkivonat létrehozása

Szüksége lesz egy hozzáférési jogkivonatot, amely lehetővé teszi a megtekintése és-jelentés szerkesztése létrehozásához. Szerkesztheti, és mentse a jelentést, szüksége lesz a **Report.ReadWrite** token engedéllyel. További információkért lásd: [Authenticating és a Power BI Embedded engedélyező](power-bi-embedded-app-token-flow.md).

> [!NOTE]
> Ez lehetővé teszi, szerkesztése és a meglévő jelentés módosításainak mentéséhez. Ha azt is szeretné funkciót támogató **Mentés másként**, akkor adja meg a további engedélyek. További információkért lásd: [hatókörök](power-bi-embedded-app-token-flow.md#scopes).

```
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Report.ReadWrite";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="embed-configuration"></a>Konfigurációs beágyazása

Szüksége lesz az engedélyek és a viewMode láthatók a Mentés gombra kattint, szerkesztési módban. További információkért lásd: [konfigurációs részletek beágyazása](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).

Ha például a JavaScript:

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

Ez a jelentés beágyazása a megjelenítési mód alapján fogják **viewMode** állítják be **modellek. ViewMode.View**.

## <a name="view-mode"></a>Megjelenítési mód

A következő JavaScript használatával nézet módba kapcsolni, ha Ön a szerkesztési módban.

```
// Get a reference to the embedded report HTML element
var reportContainer = $('#reportContainer')[0];

// Get a reference to the embedded report.
report = powerbi.get(reportContainer);

// Switch to view mode.
report.switchMode("view");

```

## <a name="edit-mode"></a>Szerkesztési módban

A következő JavaScript segítségével átváltani a szerkesztési módot, ha megtekintési módban.

```
// Get a reference to the embedded report HTML element
var reportContainer = $('#reportContainer')[0];

// Get a reference to the embedded report.
report = powerbi.get(reportContainer);

// Switch to edit mode.
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
További kérdései vannak? [Tegye próbára a Power BI közösségét](http://community.powerbi.com/)
