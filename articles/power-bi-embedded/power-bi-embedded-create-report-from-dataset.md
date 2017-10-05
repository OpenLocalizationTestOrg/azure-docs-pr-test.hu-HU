---
title: "Új jelentés létrehozása az Azure Power BI Embedded egy adatkészletből |} Microsoft Docs"
description: "A Power BI Embedded jelentések most hozhatók létre a saját alkalmazásban egy adatkészletből."
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
ms.openlocfilehash: 457f53aa76059dbb2faed6b264102f1f59b9918a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-new-report-from-a-dataset-in-power-bi-embedded"></a>A Power BI Embedded egy adatkészletből új jelentés létrehozása

A Power BI Embedded jelentések most hozhatók létre a saját alkalmazásban egy adatkészletből. 

A hitelesítési módszer hasonlít, hogy a jelentés ágyazza be. A DataSet adatkészlet vonatkozó hozzáférési jogkivonatok alapul. A powerbi.com webhelyen használt jogkivonatok ki által Azure Active Directory (AAD), és saját szolgáltatás által kibocsátott Power BI Embedded jogkivonatokat.

Ha egy beágyazott jelentést, a kiállított jogkivonatokat createing van egy adott adatkészlet. Jogkivonatok beágyazási URL-címet egy elemhez mindegyike rendelkezik egy egyedi token társítva kell lennie. Egy beágyazott jelentés létrehozásához *Dataset.Read és Workspace.Report.Create* hatókörök meg kell adni a hozzáférési jogkivonat.

## <a name="create-access-token-needed-to-create-new-report"></a>Új jelentés létrehozásához szükséges hozzáférési jogkivonat létrehozása

A Power BI Embedded használ beágyazási jogkivonatot, amely HMAC aláírt JSON webes jogkivonatainak. A jogkivonatok az access-kulcsot az Azure Power BI Embedded munkaterület-csoportok vannak aláírva. Beágyazása a jogkivonatokat, alapértelmezés szerint, egy jelentés beágyazása egy alkalmazásba, csak olvasási hozzáférésre biztosítja. Beágyazása jogkivonatok egy adott jelentés ki, és lehet társítva egy beágyazási URL-CÍMÉT.

Hozzáférési jogkivonatok a kiszolgálón kell létrehozni a tárelérési kulcsokat a jogkivonatok bejelentkezési/titkosítására használja. Olyan hozzáférési jogkivonatot létrehozásával kapcsolatos további információkért lásd: [Authenticating és engedélyezése a Power BI Embedded](power-bi-embedded-app-token-flow.md). Emellett áttekintheti a [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metódust. Íme egy példa Mi ez alábbihoz hasonlóan fog kinézni a .NET SDK használatával a Power BI.

Ebben a példában az adathalmaz-azonosító, amely szeretnénk creat az új jelentést a van. Azt is meg kell adnia a a hatókörök *Dataset.Read és Workspace.Report.Create*.

A *PowerBIToken osztály* kell telepíteni a [Power BI Core NuGut csomag](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).

**NuGet-csomag telepítése**

```
Install-Package Microsoft.PowerBI.Core
```

**C#-kódban**

```
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Dataset.Read Workspace.Report.Create";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="create-a-new-blank-report"></a>Üres új jelentés létrehozása

Ahhoz, hogy az új jelentés létrehozása a létrehozás konfigurációs kell megadni. Ennek tartalmaznia kell a hozzáférési jogkivonat, a embedURL és a datasetID kell létrehozni a jelentéskészítés vonatkozni fog. Ehhez szükséges, hogy telepítse a nuget [Power BI JavaScript csomag](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/). A embedUrl https://embedded.powerbi.com/appTokenReportEmbed fog majd.

> [!NOTE]
> Használhatja a [JavaScript-jelentés beágyazása minta](https://microsoft.github.io/PowerBI-JavaScript/demo/) funkciók tesztelésére. A különböző műveletek elérhető kódpéldák is biztosít.

**NuGet-csomag telepítése**

```
Install-Package Microsoft.PowerBI.JavaScript
```

**JavaScript-kód**

```
<div id="reportContainer"></div>
  
var embedCreateConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        datasetId: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    };
    
    // Grab the reference to the div HTML element that will host the report
    var reportContainer = $('#reportContainer')[0];

    // Create report
    var report = powerbi.createReport(reportContainer, embedCreateConfiguration);
```

Hívása *powerbi.createReport()* üres vászonból szerkesztési módban szerepelhet biztosítják a *div* elemet.

![](media/power-bi-embedded-create-report-from-dataset/pbi-embedded-create-new-report.png)

## <a name="save-new-reports"></a>Új jelentések menthetők

A jelentés nem ténylegesen jön létre amíg meghívja a **Mentés másként** műveletet. A Fájl menüből, vagy JavaScript végezhető.

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
> A rendszer létrehoz egy új jelentést csak után **Mentés másként** nevezik. Miután menti, a vászonra továbbra is megjeleníti a dataset szerkesztési módban és nem a jelentésben. Szüksége lesz, töltse be újra az új jelentést, mint bármely más jelentés.

![](media/power-bi-embedded-create-report-from-dataset/pbi-embedded-save-new-report.png)

## <a name="load-the-new-report"></a>Az új jelentés betöltése

Kommunikálni az új jelentést kell úgy, ahogy az alkalmazás beágyazza egy rendszeres jelentés beágyazása, ami azt jelenti, hogy egy új jogkivonatot kell kiállítani, kifejezetten az új jelentést a, és ezután hívja meg a beágyazási metódust.

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

## <a name="automate-save-and-load-of-a-new-report-using-the-saved-event"></a>Mentés automatizálhatja, és betölti az új jelentést a "mentett" esemény használatával

A "Mentés másként" automatizálásához és majd betölteni az új jelentést, hogy használja a "mentett" esemény. Ezt az eseményt a mentési művelet befejeződik, és adja vissza, egy Json-objektum, amely tartalmazza az új reportId, a jelentés nevét, a régi reportId (ha volt ilyen), és ha a művelet nem saveAs vagy mentse.

```
{
  "reportObjectId": "5dac7a4a-4452-46b3-99f6-a25915e0fe54",
  "reportName": "newReport",
  "saveAs": true,
  "originalReportObjectId": null
}
```

Automatizálható a folyamat figyelni a "mentett" esemény, az új reportId tenni, létrehozhat az új jogkivonatot, és az új jelentés beágyazása.

```
<div id="reportContainer"></div>
  
var embedCreateConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        datasetId: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    };
    
    // Grab the reference to the div HTML element that will host the report
    var reportContainer = $('#reportContainer')[0];

    // Create report
    var report = powerbi.createReport(reportContainer, embedCreateConfiguration);


   var saveAsParameters = {
        name: "newReport"
    };

    // SaveAs report
    report.saveAs(saveAsParameters);

    // report.on will add an event handler which prints to Log window.
    report.on("saved", function(event) {
        
         // get new Token
         var newReportId =  event.detail.reportObjectId;

        // create new Token. This is a function that the application should provide
        var newToken = createAccessToken(newReportId,scopes /*provide the wanted scopes*/);
        
        
    var embedConfiguration = {
        accessToken: newToken ,
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        reportId: newReportId,
    };

    // Embed report
    var report = powerbi.embed(reportContainer, embedConfiguration);
       
   // report.off removes a given event handler if it exists.
   report.off("saved");
    });
```

## <a name="see-also"></a>Lásd még:

[Bevezetés a minta használatába](power-bi-embedded-get-started-sample.md)  
[Jelentések mentése](power-bi-embedded-save-reports.md)  
[Jelentés beágyazása](power-bi-embedded-embed-report.md)  
[Hitelesítés és engedélyezés a Power BI Embedded használatával](power-bi-embedded-app-token-flow.md)  
[Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[JavaScript beágyazási minta](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[A Power BI Core NuGut csomag](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)  
[A Power BI JavaScript-csomag](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)  
További kérdései vannak? [Tegye próbára a Power BI közösségét](http://community.powerbi.com/)