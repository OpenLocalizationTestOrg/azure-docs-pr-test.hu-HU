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
# <a name="save-reports-in-power-bi-embedded"></a>A Power BI Embedded jelentések menthetők

Ismerje meg, hogyan toosave jelentések belül a Power BI embedded. Sikeresen ehhez rendelés toowork a megfelelő engedélyekkel.

Power BI Embedded belül szerkesztheti a meglévő jelentéseket, és mentse őket. Új jelentés létrehozása is, és elmentse egy új jelentés toocreate egyet.

A sorrend toosave jelentés először kell toocreate jogkivonat hello adott jelentés hello jobb hatókörök:

* Mentés Report.ReadWrite hatókör tooenable szükség
* Mentés másként tooenable, Report.Read és Workspace.Report.Copy hatókörben szükség
* tooenable mentése, valamint Mentés másként, Report.ReadWrite és Workspace.Report.Copy szolgáltatás

Illetve rendelés tooenable hello jobb save/mentése a gombok fájl menü, mivel kell tooprovide hello jobb gombbal hello beágyazási konfigurációs engedélyt amikor Ön beágyazási hello jelentés:

* modellek. Permissions.ReadWrite
* modellek. Permissions.Copy
* modellek. Permissions.All

> [!NOTE]
> A hozzáférési token kell hello megfelelő hatókörök. További információkért lásd: [hatókörök](power-bi-embedded-app-token-flow.md#scopes).

## <a name="embed-report-in-edit-mode"></a>Jelentés beágyazása a szerkesztési módban

Most mondja ki meg tooEmbed szerkesztési módban a jelentés az alkalmazás belül, toodo csak hello engedély tulajdonságai beágyazási konfigurációs adjon át, és hívja meg powerbi.embed(). Szüksége lesz toosupply engedélyek és a sorrend toosee hello mentés a viewMode gombként szerkesztési módban. További információkért lásd: [konfigurációs részletek beágyazása](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).

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

Most egy jelentés rendszer beágyazza az alkalmazás szerkesztési módban.

## <a name="save-report"></a>Jelentés mentése

Miután Embbeding hello jelentést a szerkesztési módban hello jobb token és engedélyeket hello jelentés mentheti, hello fájl menüből, vagy JavaScript:

```
 // Get a reference toohello embedded report.
    report = powerbi.get(reportContainer);

 // Save report
    report.save();
```

## <a name="save-as"></a>Mentés másként

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
> Csak azután *Mentés másként* egy új jelentés létrehozása. Hello menteni, miután vásznon a hello továbbra is látható hello régi jelentés a szerkesztési módot, és nem hello új jelentés. Szüksége lesz tooembed hello új jelentés létrehozása. Ehhez egy új hozzáférési jogkivonat-jelentés létrehozásához szükségesek.

Tooload hello új jelentés után kell egy *Mentés másként*. Ez hasonló tooembedding jelentései van.

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

## <a name="see-also"></a>Lásd még:

[Bevezetés a minta használatába](power-bi-embedded-get-started-sample.md)  
[Jelentés beágyazása](power-bi-embedded-embed-report.md)  
[Új jelentés létrehozása adatkészletből](power-bi-embedded-create-report-from-dataset.md)  
[Hitelesítés és engedélyezés a Power BI Embedded használatával](power-bi-embedded-app-token-flow.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[JavaScript beágyazási minta](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
További kérdései vannak? [Próbálja meg a Power BI-Közösség hello](http://community.powerbi.com/)

