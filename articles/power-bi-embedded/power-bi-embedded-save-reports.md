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
# <a name="save-reports-in-power-bi-embedded"></a>A Power BI Embedded jelentések menthetők

Tudnivalók a Power BI embedded belül jelentések menthetők. Ehhez szükséges, hogy megfelelő engedélyekkel ahhoz, hogy sikeresen működjön.

Power BI Embedded belül szerkesztheti a meglévő jelentéseket, és mentse őket. Is új jelentés létrehozása és mentése új jelentésként kattintva létrehozhat egyet.

Ahhoz, hogy a jelentés mentése először kell a megfelelő hatókörként az adott jelentés jogkivonat létrehozásához:

* Engedélyezi a Mentés Report.ReadWrite hatókörben szükség
* Ha engedélyezi a Mentés másként, Report.Read és Workspace.Report.Copy hatókörben szükség
* Engedélyezi a Mentés másként, Report.ReadWrite Workspace.Report.Copy és szolgáltatás

Illetve ahhoz, hogy a jobb oldali gombok fájl menü meg kell adnia a beágyazási konfigurációjában a megfelelő engedélye a jelentés beágyazása a save/mentés:

* modellek. Permissions.ReadWrite
* modellek. Permissions.Copy
* modellek. Permissions.All

> [!NOTE]
> A hozzáférési token is kell a megfelelő hatókörök. További információkért lásd: [hatókörök](power-bi-embedded-app-token-flow.md#scopes).

## <a name="embed-report-in-edit-mode"></a>Jelentés beágyazása a szerkesztési módban

Tegyük fel, jelentés beágyazása szerkesztési módban az alkalmazásban, csak adja át a megfelelő tulajdonságok beágyazási konfigurációban, és hívja meg powerbi.embed() belül kívánja. Engedélyek és a viewMode láthatók a mentési szüksége lesz, és menteni, ha a gomb szerkesztési módban. További információkért lásd: [konfigurációs részletek beágyazása](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details).

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

Most egy jelentés rendszer beágyazza az alkalmazás szerkesztési módban.

## <a name="save-report"></a>Jelentés mentése

Embbeding után a a jelentést a szerkesztési módban a jobb oldali token és mentheti a jelentést, a Fájl menüből, vagy JavaScript engedélyeket:

```
 // Get a reference to the embedded report.
    report = powerbi.get(reportContainer);

 // Save report
    report.save();
```

## <a name="save-as"></a>Mentés másként

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
> Csak azután *Mentés másként* egy új jelentés létrehozása. A mentés után a vászon továbbra is azt a régi jelentés szerkesztési módban, és nem az új jelentést. Szüksége lesz a létrehozott új jelentés beágyazása. Ehhez egy új hozzáférési jogkivonat-jelentés létrehozásához szükségesek.

Majd szüksége lesz a betöltése után az új jelentést a *Mentés másként*. Ez hasonlít bármely jelentés beágyazása.

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

## <a name="see-also"></a>Lásd még:

[Bevezetés a minta használatába](power-bi-embedded-get-started-sample.md)  
[Jelentés beágyazása](power-bi-embedded-embed-report.md)  
[Új jelentés létrehozása adatkészletből](power-bi-embedded-create-report-from-dataset.md)  
[Hitelesítés és engedélyezés a Power BI Embedded használatával](power-bi-embedded-app-token-flow.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[JavaScript beágyazási minta](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
További kérdései vannak? [Tegye próbára a Power BI közösségét](http://community.powerbi.com/)

