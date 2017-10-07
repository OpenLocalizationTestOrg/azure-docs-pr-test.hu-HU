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
# <a name="create-a-new-report-from-a-dataset-in-power-bi-embedded"></a><span data-ttu-id="3d50e-103">A Power BI Embedded egy adatkészletből új jelentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d50e-103">Create a new report from a dataset in Power BI Embedded</span></span>

<span data-ttu-id="3d50e-104">A Power BI Embedded jelentések most hozhatók létre a saját alkalmazásban egy adatkészletből.</span><span class="sxs-lookup"><span data-stu-id="3d50e-104">Power BI Embedded reports can now be created from a dataset in your own application.</span></span> 

<span data-ttu-id="3d50e-105">hello hitelesítési módszer hasonlít toothat jelentés beágyazása.</span><span class="sxs-lookup"><span data-stu-id="3d50e-105">hello authentication method is similar toothat of report embed.</span></span> <span data-ttu-id="3d50e-106">Hozzáférési jogkivonatok, amelyek adott tooa dataset alapul.</span><span class="sxs-lookup"><span data-stu-id="3d50e-106">It is based on access tokens that are specific tooa dataset.</span></span> <span data-ttu-id="3d50e-107">A powerbi.com webhelyen használt jogkivonatok ki által Azure Active Directory (AAD), és saját szolgáltatás által kibocsátott Power BI Embedded jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="3d50e-107">Tokens used for PowerBI.com are issued by Azure Active Directory (AAD) and Power BI Embedded tokens are issued by your own service.</span></span>

<span data-ttu-id="3d50e-108">Ha egy beágyazott jelentés createing hello kiadott jogkivonatok egy adott adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="3d50e-108">When createing an Embedded report, hello tokens issued are for a specific dataset.</span></span> <span data-ttu-id="3d50e-109">Jogkivonatok kell társítani a hello beágyazása az URL-cím hello azonos elem tooensure minden rendelkezik egy egyedi jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="3d50e-109">Tokens should be associated with hello embed URL on hello same element tooensure each has a unique token.</span></span> <span data-ttu-id="3d50e-110">A rendezés toocreate egy beágyazott jelentés *Dataset.Read és Workspace.Report.Create* hatókörök hello hozzáférési jogkivonatot kell megadni.</span><span class="sxs-lookup"><span data-stu-id="3d50e-110">In order toocreate an Embedded report, *Dataset.Read and Workspace.Report.Create* scopes must be provided in hello access token.</span></span>

## <a name="create-access-token-needed-toocreate-new-report"></a><span data-ttu-id="3d50e-111">Access token szükséges toocreate új jelentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d50e-111">Create access token needed toocreate new report</span></span>

<span data-ttu-id="3d50e-112">A Power BI Embedded használ beágyazási jogkivonatot, amely HMAC aláírt JSON webes jogkivonatainak.</span><span class="sxs-lookup"><span data-stu-id="3d50e-112">Power BI Embedded uses embed token, which are HMAC signed JSON Web Tokens.</span></span> <span data-ttu-id="3d50e-113">hello jogkivonatok hello hozzáférési kulcsot a Azure Power BI Embedded munkaterület-csoportok vannak aláírva.</span><span class="sxs-lookup"><span data-stu-id="3d50e-113">hello tokens are signed with hello access key from your Azure Power BI Embedded workspace collection.</span></span> <span data-ttu-id="3d50e-114">Beágyazása a jogkivonatokat, alapértelmezés szerint, van olvasási használt tooprovide csak akkor férhessenek hozzá tooa jelentés tooembed egy alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="3d50e-114">Embed tokens, by default, are used tooprovide read only access tooa report tooembed into an application.</span></span> <span data-ttu-id="3d50e-115">Beágyazása jogkivonatok egy adott jelentés ki, és lehet társítva egy beágyazási URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="3d50e-115">Embed tokens are issued for a specific report and should be associated with an embed URL.</span></span>

<span data-ttu-id="3d50e-116">Hozzáférési jogkivonatok hello elérési kulcsok vannak használt toosign/titkosítása hello jogkivonatok hello kiszolgálón kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="3d50e-116">Access tokens should be created on hello server as hello access keys are used toosign/encrypt hello tokens.</span></span> <span data-ttu-id="3d50e-117">Információ toocreate hozzáférési tokent, lásd: [Authenticating és engedélyezése a Power BI Embedded](power-bi-embedded-app-token-flow.md).</span><span class="sxs-lookup"><span data-stu-id="3d50e-117">For information on how toocreate an access token, see [Authenticating and authorizing with Power BI Embedded](power-bi-embedded-app-token-flow.md).</span></span> <span data-ttu-id="3d50e-118">Emellett áttekintheti hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metódust.</span><span class="sxs-lookup"><span data-stu-id="3d50e-118">You can also review hello [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) method.</span></span> <span data-ttu-id="3d50e-119">Íme egy példa Mi ez néz hello .NET SDK használatával a Power BI.</span><span class="sxs-lookup"><span data-stu-id="3d50e-119">Here is an example of what this would look like using hello .NET SDK for Power BI.</span></span>

<span data-ttu-id="3d50e-120">Ebben a példában a toocreat hello új jelentés szeretnénk az adathalmaz-azonosító van.</span><span class="sxs-lookup"><span data-stu-id="3d50e-120">In this example, we have our dataset id that we want toocreat hello new report on.</span></span> <span data-ttu-id="3d50e-121">Tooadd hello hatókört is kell *Dataset.Read és Workspace.Report.Create*.</span><span class="sxs-lookup"><span data-stu-id="3d50e-121">We also need tooadd hello scopes for *Dataset.Read and Workspace.Report.Create*.</span></span>

<span data-ttu-id="3d50e-122">Hello *PowerBIToken osztály* kell telepíteni hello [Power BI Core NuGut csomag](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).</span><span class="sxs-lookup"><span data-stu-id="3d50e-122">hello *PowerBIToken class* requires that you install hello [Power BI Core NuGut Package](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).</span></span>

<span data-ttu-id="3d50e-123">**NuGet-csomag telepítése**</span><span class="sxs-lookup"><span data-stu-id="3d50e-123">**NuGet package install**</span></span>

```
Install-Package Microsoft.PowerBI.Core
```

<span data-ttu-id="3d50e-124">**C#-kódban**</span><span class="sxs-lookup"><span data-stu-id="3d50e-124">**C# code**</span></span>

```
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Dataset.Read Workspace.Report.Create";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="create-a-new-blank-report"></a><span data-ttu-id="3d50e-125">Üres új jelentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d50e-125">Create a new blank report</span></span>

<span data-ttu-id="3d50e-126">A sorrend toocreate új jelentés, hello hozzon létre configuration kell megadni.</span><span class="sxs-lookup"><span data-stu-id="3d50e-126">In order toocreate a new report, hello create configuration should be provided.</span></span> <span data-ttu-id="3d50e-127">Ennek tartalmaznia kell hello hozzáférési jogkivonatot, hello embedURL és hello datasetID, amely azt szeretnénk, hogy toocreate hello jelentéskészítés vonatkozni fog.</span><span class="sxs-lookup"><span data-stu-id="3d50e-127">This should include hello access token, hello embedURL and hello datasetID that we want toocreate hello report against.</span></span> <span data-ttu-id="3d50e-128">Ehhez szükséges, hogy telepítse hello nuget [Power BI JavaScript csomag](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/).</span><span class="sxs-lookup"><span data-stu-id="3d50e-128">This requires that you install hello nuget [Power BI JavaScript package](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/).</span></span> <span data-ttu-id="3d50e-129">hello embedUrl https://embedded.powerbi.com/appTokenReportEmbed fog majd.</span><span class="sxs-lookup"><span data-stu-id="3d50e-129">hello embedUrl will just be https://embedded.powerbi.com/appTokenReportEmbed.</span></span>

> [!NOTE]
> <span data-ttu-id="3d50e-130">Használhatja a hello [JavaScript-jelentés beágyazása minta](https://microsoft.github.io/PowerBI-JavaScript/demo/) tootest funkciót.</span><span class="sxs-lookup"><span data-stu-id="3d50e-130">You can use hello [JavaScript Report Embed Sample](https://microsoft.github.io/PowerBI-JavaScript/demo/) tootest functionality.</span></span> <span data-ttu-id="3d50e-131">Emellett a segítségével kódpéldák hello különböző műveletek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="3d50e-131">It also gives code examples for hello different operations that are available.</span></span>

<span data-ttu-id="3d50e-132">**NuGet-csomag telepítése**</span><span class="sxs-lookup"><span data-stu-id="3d50e-132">**NuGet package install**</span></span>

```
Install-Package Microsoft.PowerBI.JavaScript
```

<span data-ttu-id="3d50e-133">**JavaScript-kód**</span><span class="sxs-lookup"><span data-stu-id="3d50e-133">**JavaScript code**</span></span>

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

<span data-ttu-id="3d50e-134">Hívása *powerbi.createReport()* egy üres vásznon a hello szerepelhet szerkesztési módban megkönnyítő *div* elemet.</span><span class="sxs-lookup"><span data-stu-id="3d50e-134">Calling *powerbi.createReport()* will make a blank canvas in edit mode appear within hello *div* element.</span></span>

![](media/power-bi-embedded-create-report-from-dataset/pbi-embedded-create-new-report.png)

## <a name="save-new-reports"></a><span data-ttu-id="3d50e-135">Új jelentések menthetők</span><span class="sxs-lookup"><span data-stu-id="3d50e-135">Save new reports</span></span>

<span data-ttu-id="3d50e-136">hello jelentés nem ténylegesen jön létre amíg meghívja a hello **Mentés másként** műveletet.</span><span class="sxs-lookup"><span data-stu-id="3d50e-136">hello report will not actually be created until you call hello **save as** operation.</span></span> <span data-ttu-id="3d50e-137">A Fájl menüből, vagy JavaScript végezhető.</span><span class="sxs-lookup"><span data-stu-id="3d50e-137">This can be done from file menu or from JavaScript.</span></span>

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
> <span data-ttu-id="3d50e-138">A rendszer létrehoz egy új jelentést csak után **Mentés másként** nevezik.</span><span class="sxs-lookup"><span data-stu-id="3d50e-138">A new report is created only after **save as** is called.</span></span> <span data-ttu-id="3d50e-139">Miután menti, a vásznon a hello továbbra is hello adatkészlet megjelenítése a szerkesztési módot, és nem hello jelentésben.</span><span class="sxs-lookup"><span data-stu-id="3d50e-139">After you save, hello canvas will still show hello dataset in edit mode and not hello report.</span></span> <span data-ttu-id="3d50e-140">Szüksége lesz tooreload hello új jelentés, mint bármely más jelentés.</span><span class="sxs-lookup"><span data-stu-id="3d50e-140">You will need tooreload hello new report like you would any other report.</span></span>

![](media/power-bi-embedded-create-report-from-dataset/pbi-embedded-save-new-report.png)

## <a name="load-hello-new-report"></a><span data-ttu-id="3d50e-141">Hello új jelentés betöltése</span><span class="sxs-lookup"><span data-stu-id="3d50e-141">Load hello new report</span></span>

<span data-ttu-id="3d50e-142">Rendelés toointeract hello új jelentést kell azt a hello ugyanúgy hello alkalmazás beágyazza rendszeres jelentést, azaz a, egy új jogkivonatot kell kiállítani, kifejezetten a hello új jelentést, majd a hívás hello beágyazása metódus tooembed.</span><span class="sxs-lookup"><span data-stu-id="3d50e-142">In order toointeract with hello new report you need tooembed it in hello same way hello application embeds a regular report, meaning, a new token must be issued specifically for hello new report and then call hello embed method.</span></span>

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

## <a name="automate-save-and-load-of-a-new-report-using-hello-saved-event"></a><span data-ttu-id="3d50e-143">Mentés automatizálhatja és egy új jelentés használatával "mentett" esemény hello betöltése</span><span class="sxs-lookup"><span data-stu-id="3d50e-143">Automate save and load of a new report using hello "saved" event</span></span>

<span data-ttu-id="3d50e-144">A sorrend tooautomate hello folyamat a "Mentés másként" és "mentett" esemény hello majd betöltése hello új jelentést, hogy használatát.</span><span class="sxs-lookup"><span data-stu-id="3d50e-144">In order tooautomate hello process of "save as" and then loading hello new report you can make use of hello "saved" Event.</span></span> <span data-ttu-id="3d50e-145">Ez az esemény lép működésbe, amikor hello mentési művelet befejeződik, és adja vissza, egy Json-objektum tartalmazó hello új reportId, a jelentés neve, a régi reportId hello (ha volt ilyen), és ha hello művelet volt saveAs vagy mentse.</span><span class="sxs-lookup"><span data-stu-id="3d50e-145">This event is fired when hello save operation is complete and it returns a Json object containing hello new reportId, report name, hello old reportId(if there was one) and if hello operation was saveAs or save.</span></span>

```
{
  "reportObjectId": "5dac7a4a-4452-46b3-99f6-a25915e0fe54",
  "reportName": "newReport",
  "saveAs": true,
  "originalReportObjectId": null
}
```

<span data-ttu-id="3d50e-146">tooAutomate hello folyamat "mentett" hello esemény figyelni, hello új reportId tenni, létrehozhat hello új jogkivonatot, és hello új jelentés beágyazása.</span><span class="sxs-lookup"><span data-stu-id="3d50e-146">tooAutomate hello process you can listen on hello "saved" event, take hello new reportId, create hello new token and embed hello new report with it.</span></span>

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

## <a name="see-also"></a><span data-ttu-id="3d50e-147">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="3d50e-147">See also</span></span>

[<span data-ttu-id="3d50e-148">Bevezetés a minta használatába</span><span class="sxs-lookup"><span data-stu-id="3d50e-148">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="3d50e-149">Jelentések mentése</span><span class="sxs-lookup"><span data-stu-id="3d50e-149">Save reports</span></span>](power-bi-embedded-save-reports.md)  
[<span data-ttu-id="3d50e-150">Jelentés beágyazása</span><span class="sxs-lookup"><span data-stu-id="3d50e-150">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
[<span data-ttu-id="3d50e-151">Hitelesítés és engedélyezés a Power BI Embedded használatával</span><span class="sxs-lookup"><span data-stu-id="3d50e-151">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="3d50e-152">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="3d50e-152">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="3d50e-153">JavaScript beágyazási minta</span><span class="sxs-lookup"><span data-stu-id="3d50e-153">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[<span data-ttu-id="3d50e-154">A Power BI Core NuGut csomag</span><span class="sxs-lookup"><span data-stu-id="3d50e-154">Power BI Core NuGut Package</span></span>](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)  
[<span data-ttu-id="3d50e-155">A Power BI JavaScript-csomag</span><span class="sxs-lookup"><span data-stu-id="3d50e-155">Power BI JavaScript package</span></span>](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)  
<span data-ttu-id="3d50e-156">További kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="3d50e-156">More questions?</span></span> [<span data-ttu-id="3d50e-157">Próbálja meg a Power BI-Közösség hello</span><span class="sxs-lookup"><span data-stu-id="3d50e-157">Try hello Power BI Community</span></span>](http://community.powerbi.com/)
