---
title: "az Azure Power BI Embedded egy adatkészletből új jelentés aaaCreate |} Microsoft Docs"
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
ms.openlocfilehash: 41a0a52e4c833313f495bb5ff14749203fef9b41
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-new-report-from-a-dataset-in-power-bi-embedded"></a>A Power BI Embedded egy adatkészletből új jelentés létrehozása

A Power BI Embedded jelentések most hozhatók létre a saját alkalmazásban egy adatkészletből. 

hello hitelesítési módszer hasonlít toothat jelentés beágyazása. Hozzáférési jogkivonatok, amelyek adott tooa dataset alapul. A powerbi.com webhelyen használt jogkivonatok ki által Azure Active Directory (AAD), és saját szolgáltatás által kibocsátott Power BI Embedded jogkivonatokat.

Ha egy beágyazott jelentés createing hello kiadott jogkivonatok egy adott adatkészlet. Jogkivonatok kell társítani a hello beágyazása az URL-cím hello azonos elem tooensure minden rendelkezik egy egyedi jogkivonatot. A rendezés toocreate egy beágyazott jelentés *Dataset.Read és Workspace.Report.Create* hatókörök hello hozzáférési jogkivonatot kell megadni.

## <a name="create-access-token-needed-toocreate-new-report"></a>Access token szükséges toocreate új jelentés létrehozása

A Power BI Embedded használ beágyazási jogkivonatot, amely HMAC aláírt JSON webes jogkivonatainak. hello jogkivonatok hello hozzáférési kulcsot a Azure Power BI Embedded munkaterület-csoportok vannak aláírva. Beágyazása a jogkivonatokat, alapértelmezés szerint, van olvasási használt tooprovide csak akkor férhessenek hozzá tooa jelentés tooembed egy alkalmazásba. Beágyazása jogkivonatok egy adott jelentés ki, és lehet társítva egy beágyazási URL-CÍMÉT.

Hozzáférési jogkivonatok hello elérési kulcsok vannak használt toosign/titkosítása hello jogkivonatok hello kiszolgálón kell létrehozni. Információ toocreate hozzáférési tokent, lásd: [Authenticating és engedélyezése a Power BI Embedded](power-bi-embedded-app-token-flow.md). Emellett áttekintheti hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metódust. Íme egy példa Mi ez néz hello .NET SDK használatával a Power BI.

Ebben a példában a toocreat hello új jelentés szeretnénk az adathalmaz-azonosító van. Tooadd hello hatókört is kell *Dataset.Read és Workspace.Report.Create*.

Hello *PowerBIToken osztály* kell telepíteni hello [Power BI Core NuGut csomag](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).

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

A sorrend toocreate új jelentés, hello hozzon létre configuration kell megadni. Ennek tartalmaznia kell hello hozzáférési jogkivonatot, hello embedURL és hello datasetID, amely azt szeretnénk, hogy toocreate hello jelentéskészítés vonatkozni fog. Ehhez szükséges, hogy telepítse hello nuget [Power BI JavaScript csomag](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/). hello embedUrl https://embedded.powerbi.com/appTokenReportEmbed fog majd.

> [!NOTE]
> Használhatja a hello [JavaScript-jelentés beágyazása minta](https://microsoft.github.io/PowerBI-JavaScript/demo/) tootest funkciót. Emellett a segítségével kódpéldák hello különböző műveletek érhetők el.

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
    
    // Grab hello reference toohello div HTML element that will host hello report
    var reportContainer = $('#reportContainer')[0];

    // Create report
    var report = powerbi.createReport(reportContainer, embedCreateConfiguration);
```

Hívása *powerbi.createReport()* egy üres vásznon a hello szerepelhet szerkesztési módban megkönnyítő *div* elemet.

![](media/power-bi-embedded-create-report-from-dataset/pbi-embedded-create-new-report.png)

## <a name="save-new-reports"></a>Új jelentések menthetők

hello jelentés nem ténylegesen jön létre amíg meghívja a hello **Mentés másként** műveletet. A Fájl menüből, vagy JavaScript végezhető.

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
> A rendszer létrehoz egy új jelentést csak után **Mentés másként** nevezik. Miután menti, a vásznon a hello továbbra is hello adatkészlet megjelenítése a szerkesztési módot, és nem hello jelentésben. Szüksége lesz tooreload hello új jelentés, mint bármely más jelentés.

![](media/power-bi-embedded-create-report-from-dataset/pbi-embedded-save-new-report.png)

## <a name="load-hello-new-report"></a>Hello új jelentés betöltése

Rendelés toointeract hello új jelentést kell azt a hello ugyanúgy hello alkalmazás beágyazza rendszeres jelentést, azaz a, egy új jogkivonatot kell kiállítani, kifejezetten a hello új jelentést, majd a hívás hello beágyazása metódus tooembed.

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

## <a name="automate-save-and-load-of-a-new-report-using-hello-saved-event"></a>Mentés automatizálhatja és egy új jelentés használatával "mentett" esemény hello betöltése

A sorrend tooautomate hello folyamat a "Mentés másként" és "mentett" esemény hello majd betöltése hello új jelentést, hogy használatát. Ez az esemény lép működésbe, amikor hello mentési művelet befejeződik, és adja vissza, egy Json-objektum tartalmazó hello új reportId, a jelentés neve, a régi reportId hello (ha volt ilyen), és ha hello művelet volt saveAs vagy mentse.

```
{
  "reportObjectId": "5dac7a4a-4452-46b3-99f6-a25915e0fe54",
  "reportName": "newReport",
  "saveAs": true,
  "originalReportObjectId": null
}
```

tooAutomate hello folyamat "mentett" hello esemény figyelni, hello új reportId tenni, létrehozhat hello új jogkivonatot, és hello új jelentés beágyazása.

```
<div id="reportContainer"></div>
  
var embedCreateConfiguration = {
        accessToken: 'eyJ0eXAiO...Qron7qYpY9MI',
        embedUrl: 'https://embedded.powerbi.com/appTokenReportEmbed',
        datasetId: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    };
    
    // Grab hello reference toohello div HTML element that will host hello report
    var reportContainer = $('#reportContainer')[0];

    // Create report
    var report = powerbi.createReport(reportContainer, embedCreateConfiguration);


   var saveAsParameters = {
        name: "newReport"
    };

    // SaveAs report
    report.saveAs(saveAsParameters);

    // report.on will add an event handler which prints tooLog window.
    report.on("saved", function(event) {
        
         // get new Token
         var newReportId =  event.detail.reportObjectId;

        // create new Token. This is a function that hello application should provide
        var newToken = createAccessToken(newReportId,scopes /*provide hello wanted scopes*/);
        
        
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
További kérdései vannak? [Próbálja meg a Power BI-Közösség hello](http://community.powerbi.com/)
