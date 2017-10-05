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
# <a name="create-a-new-report-from-a-dataset-in-power-bi-embedded"></a><span data-ttu-id="c9613-103">A Power BI Embedded egy adatkészletből új jelentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="c9613-103">Create a new report from a dataset in Power BI Embedded</span></span>

<span data-ttu-id="c9613-104">A Power BI Embedded jelentések most hozhatók létre a saját alkalmazásban egy adatkészletből.</span><span class="sxs-lookup"><span data-stu-id="c9613-104">Power BI Embedded reports can now be created from a dataset in your own application.</span></span> 

<span data-ttu-id="c9613-105">A hitelesítési módszer hasonlít, hogy a jelentés ágyazza be.</span><span class="sxs-lookup"><span data-stu-id="c9613-105">The authentication method is similar to that of report embed.</span></span> <span data-ttu-id="c9613-106">A DataSet adatkészlet vonatkozó hozzáférési jogkivonatok alapul.</span><span class="sxs-lookup"><span data-stu-id="c9613-106">It is based on access tokens that are specific to a dataset.</span></span> <span data-ttu-id="c9613-107">A powerbi.com webhelyen használt jogkivonatok ki által Azure Active Directory (AAD), és saját szolgáltatás által kibocsátott Power BI Embedded jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="c9613-107">Tokens used for PowerBI.com are issued by Azure Active Directory (AAD) and Power BI Embedded tokens are issued by your own service.</span></span>

<span data-ttu-id="c9613-108">Ha egy beágyazott jelentést, a kiállított jogkivonatokat createing van egy adott adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="c9613-108">When createing an Embedded report, the tokens issued are for a specific dataset.</span></span> <span data-ttu-id="c9613-109">Jogkivonatok beágyazási URL-címet egy elemhez mindegyike rendelkezik egy egyedi token társítva kell lennie.</span><span class="sxs-lookup"><span data-stu-id="c9613-109">Tokens should be associated with the embed URL on the same element to ensure each has a unique token.</span></span> <span data-ttu-id="c9613-110">Egy beágyazott jelentés létrehozásához *Dataset.Read és Workspace.Report.Create* hatókörök meg kell adni a hozzáférési jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="c9613-110">In order to create an Embedded report, *Dataset.Read and Workspace.Report.Create* scopes must be provided in the access token.</span></span>

## <a name="create-access-token-needed-to-create-new-report"></a><span data-ttu-id="c9613-111">Új jelentés létrehozásához szükséges hozzáférési jogkivonat létrehozása</span><span class="sxs-lookup"><span data-stu-id="c9613-111">Create access token needed to create new report</span></span>

<span data-ttu-id="c9613-112">A Power BI Embedded használ beágyazási jogkivonatot, amely HMAC aláírt JSON webes jogkivonatainak.</span><span class="sxs-lookup"><span data-stu-id="c9613-112">Power BI Embedded uses embed token, which are HMAC signed JSON Web Tokens.</span></span> <span data-ttu-id="c9613-113">A jogkivonatok az access-kulcsot az Azure Power BI Embedded munkaterület-csoportok vannak aláírva.</span><span class="sxs-lookup"><span data-stu-id="c9613-113">The tokens are signed with the access key from your Azure Power BI Embedded workspace collection.</span></span> <span data-ttu-id="c9613-114">Beágyazása a jogkivonatokat, alapértelmezés szerint, egy jelentés beágyazása egy alkalmazásba, csak olvasási hozzáférésre biztosítja.</span><span class="sxs-lookup"><span data-stu-id="c9613-114">Embed tokens, by default, are used to provide read only access to a report to embed into an application.</span></span> <span data-ttu-id="c9613-115">Beágyazása jogkivonatok egy adott jelentés ki, és lehet társítva egy beágyazási URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="c9613-115">Embed tokens are issued for a specific report and should be associated with an embed URL.</span></span>

<span data-ttu-id="c9613-116">Hozzáférési jogkivonatok a kiszolgálón kell létrehozni a tárelérési kulcsokat a jogkivonatok bejelentkezési/titkosítására használja.</span><span class="sxs-lookup"><span data-stu-id="c9613-116">Access tokens should be created on the server as the access keys are used to sign/encrypt the tokens.</span></span> <span data-ttu-id="c9613-117">Olyan hozzáférési jogkivonatot létrehozásával kapcsolatos további információkért lásd: [Authenticating és engedélyezése a Power BI Embedded](power-bi-embedded-app-token-flow.md).</span><span class="sxs-lookup"><span data-stu-id="c9613-117">For information on how to create an access token, see [Authenticating and authorizing with Power BI Embedded](power-bi-embedded-app-token-flow.md).</span></span> <span data-ttu-id="c9613-118">Emellett áttekintheti a [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) metódust.</span><span class="sxs-lookup"><span data-stu-id="c9613-118">You can also review the [CreateReportEmbedToken](https://docs.microsoft.com/dotnet/api/microsoft.powerbi.security.powerbitoken?redirectedfrom=MSDN#methods_) method.</span></span> <span data-ttu-id="c9613-119">Íme egy példa Mi ez alábbihoz hasonlóan fog kinézni a .NET SDK használatával a Power BI.</span><span class="sxs-lookup"><span data-stu-id="c9613-119">Here is an example of what this would look like using the .NET SDK for Power BI.</span></span>

<span data-ttu-id="c9613-120">Ebben a példában az adathalmaz-azonosító, amely szeretnénk creat az új jelentést a van.</span><span class="sxs-lookup"><span data-stu-id="c9613-120">In this example, we have our dataset id that we want to creat the new report on.</span></span> <span data-ttu-id="c9613-121">Azt is meg kell adnia a a hatókörök *Dataset.Read és Workspace.Report.Create*.</span><span class="sxs-lookup"><span data-stu-id="c9613-121">We also need to add the scopes for *Dataset.Read and Workspace.Report.Create*.</span></span>

<span data-ttu-id="c9613-122">A *PowerBIToken osztály* kell telepíteni a [Power BI Core NuGut csomag](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).</span><span class="sxs-lookup"><span data-stu-id="c9613-122">The *PowerBIToken class* requires that you install the [Power BI Core NuGut Package](https://www.nuget.org/packages/Microsoft.PowerBI.Core/).</span></span>

<span data-ttu-id="c9613-123">**NuGet-csomag telepítése**</span><span class="sxs-lookup"><span data-stu-id="c9613-123">**NuGet package install**</span></span>

```
Install-Package Microsoft.PowerBI.Core
```

<span data-ttu-id="c9613-124">**C#-kódban**</span><span class="sxs-lookup"><span data-stu-id="c9613-124">**C# code**</span></span>

```
using Microsoft.PowerBI.Security;

// rlsUsername and roles are optional
string scopes = "Dataset.Read Workspace.Report.Create";
PowerBIToken embedToken = PowerBIToken.CreateReportEmbedTokenForCreation(workspaceCollectionName, workspaceId, datasetId, null, null, scopes);

var token = embedToken.Generate("{access key}");
```

## <a name="create-a-new-blank-report"></a><span data-ttu-id="c9613-125">Üres új jelentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="c9613-125">Create a new blank report</span></span>

<span data-ttu-id="c9613-126">Ahhoz, hogy az új jelentés létrehozása a létrehozás konfigurációs kell megadni.</span><span class="sxs-lookup"><span data-stu-id="c9613-126">In order to create a new report, the create configuration should be provided.</span></span> <span data-ttu-id="c9613-127">Ennek tartalmaznia kell a hozzáférési jogkivonat, a embedURL és a datasetID kell létrehozni a jelentéskészítés vonatkozni fog.</span><span class="sxs-lookup"><span data-stu-id="c9613-127">This should include the access token, the embedURL and the datasetID that we want to create the report against.</span></span> <span data-ttu-id="c9613-128">Ehhez szükséges, hogy telepítse a nuget [Power BI JavaScript csomag](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/).</span><span class="sxs-lookup"><span data-stu-id="c9613-128">This requires that you install the nuget [Power BI JavaScript package](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/).</span></span> <span data-ttu-id="c9613-129">A embedUrl https://embedded.powerbi.com/appTokenReportEmbed fog majd.</span><span class="sxs-lookup"><span data-stu-id="c9613-129">The embedUrl will just be https://embedded.powerbi.com/appTokenReportEmbed.</span></span>

> [!NOTE]
> <span data-ttu-id="c9613-130">Használhatja a [JavaScript-jelentés beágyazása minta](https://microsoft.github.io/PowerBI-JavaScript/demo/) funkciók tesztelésére.</span><span class="sxs-lookup"><span data-stu-id="c9613-130">You can use the [JavaScript Report Embed Sample](https://microsoft.github.io/PowerBI-JavaScript/demo/) to test functionality.</span></span> <span data-ttu-id="c9613-131">A különböző műveletek elérhető kódpéldák is biztosít.</span><span class="sxs-lookup"><span data-stu-id="c9613-131">It also gives code examples for the different operations that are available.</span></span>

<span data-ttu-id="c9613-132">**NuGet-csomag telepítése**</span><span class="sxs-lookup"><span data-stu-id="c9613-132">**NuGet package install**</span></span>

```
Install-Package Microsoft.PowerBI.JavaScript
```

<span data-ttu-id="c9613-133">**JavaScript-kód**</span><span class="sxs-lookup"><span data-stu-id="c9613-133">**JavaScript code**</span></span>

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

<span data-ttu-id="c9613-134">Hívása *powerbi.createReport()* üres vászonból szerkesztési módban szerepelhet biztosítják a *div* elemet.</span><span class="sxs-lookup"><span data-stu-id="c9613-134">Calling *powerbi.createReport()* will make a blank canvas in edit mode appear within the *div* element.</span></span>

![](media/power-bi-embedded-create-report-from-dataset/pbi-embedded-create-new-report.png)

## <a name="save-new-reports"></a><span data-ttu-id="c9613-135">Új jelentések menthetők</span><span class="sxs-lookup"><span data-stu-id="c9613-135">Save new reports</span></span>

<span data-ttu-id="c9613-136">A jelentés nem ténylegesen jön létre amíg meghívja a **Mentés másként** műveletet.</span><span class="sxs-lookup"><span data-stu-id="c9613-136">The report will not actually be created until you call the **save as** operation.</span></span> <span data-ttu-id="c9613-137">A Fájl menüből, vagy JavaScript végezhető.</span><span class="sxs-lookup"><span data-stu-id="c9613-137">This can be done from file menu or from JavaScript.</span></span>

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
> <span data-ttu-id="c9613-138">A rendszer létrehoz egy új jelentést csak után **Mentés másként** nevezik.</span><span class="sxs-lookup"><span data-stu-id="c9613-138">A new report is created only after **save as** is called.</span></span> <span data-ttu-id="c9613-139">Miután menti, a vászonra továbbra is megjeleníti a dataset szerkesztési módban és nem a jelentésben.</span><span class="sxs-lookup"><span data-stu-id="c9613-139">After you save, the canvas will still show the dataset in edit mode and not the report.</span></span> <span data-ttu-id="c9613-140">Szüksége lesz, töltse be újra az új jelentést, mint bármely más jelentés.</span><span class="sxs-lookup"><span data-stu-id="c9613-140">You will need to reload the new report like you would any other report.</span></span>

![](media/power-bi-embedded-create-report-from-dataset/pbi-embedded-save-new-report.png)

## <a name="load-the-new-report"></a><span data-ttu-id="c9613-141">Az új jelentés betöltése</span><span class="sxs-lookup"><span data-stu-id="c9613-141">Load the new report</span></span>

<span data-ttu-id="c9613-142">Kommunikálni az új jelentést kell úgy, ahogy az alkalmazás beágyazza egy rendszeres jelentés beágyazása, ami azt jelenti, hogy egy új jogkivonatot kell kiállítani, kifejezetten az új jelentést a, és ezután hívja meg a beágyazási metódust.</span><span class="sxs-lookup"><span data-stu-id="c9613-142">In order to interact with the new report you need to embed it in the same way the application embeds a regular report, meaning, a new token must be issued specifically for the new report and then call the embed method.</span></span>

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

## <a name="automate-save-and-load-of-a-new-report-using-the-saved-event"></a><span data-ttu-id="c9613-143">Mentés automatizálhatja, és betölti az új jelentést a "mentett" esemény használatával</span><span class="sxs-lookup"><span data-stu-id="c9613-143">Automate save and load of a new report using the "saved" event</span></span>

<span data-ttu-id="c9613-144">A "Mentés másként" automatizálásához és majd betölteni az új jelentést, hogy használja a "mentett" esemény.</span><span class="sxs-lookup"><span data-stu-id="c9613-144">In order to automate the process of "save as" and then loading the new report you can make use of the "saved" Event.</span></span> <span data-ttu-id="c9613-145">Ezt az eseményt a mentési művelet befejeződik, és adja vissza, egy Json-objektum, amely tartalmazza az új reportId, a jelentés nevét, a régi reportId (ha volt ilyen), és ha a művelet nem saveAs vagy mentse.</span><span class="sxs-lookup"><span data-stu-id="c9613-145">This event is fired when the save operation is complete and it returns a Json object containing the new reportId, report name, the old reportId(if there was one) and if the operation was saveAs or save.</span></span>

```
{
  "reportObjectId": "5dac7a4a-4452-46b3-99f6-a25915e0fe54",
  "reportName": "newReport",
  "saveAs": true,
  "originalReportObjectId": null
}
```

<span data-ttu-id="c9613-146">Automatizálható a folyamat figyelni a "mentett" esemény, az új reportId tenni, létrehozhat az új jogkivonatot, és az új jelentés beágyazása.</span><span class="sxs-lookup"><span data-stu-id="c9613-146">To Automate the process you can listen on the "saved" event, take the new reportId, create the new token and embed the new report with it.</span></span>

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

## <a name="see-also"></a><span data-ttu-id="c9613-147">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="c9613-147">See also</span></span>

[<span data-ttu-id="c9613-148">Bevezetés a minta használatába</span><span class="sxs-lookup"><span data-stu-id="c9613-148">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)  
[<span data-ttu-id="c9613-149">Jelentések mentése</span><span class="sxs-lookup"><span data-stu-id="c9613-149">Save reports</span></span>](power-bi-embedded-save-reports.md)  
[<span data-ttu-id="c9613-150">Jelentés beágyazása</span><span class="sxs-lookup"><span data-stu-id="c9613-150">Embed a report</span></span>](power-bi-embedded-embed-report.md)  
[<span data-ttu-id="c9613-151">Hitelesítés és engedélyezés a Power BI Embedded használatával</span><span class="sxs-lookup"><span data-stu-id="c9613-151">Authenticating and authorizing in Power BI Embedded</span></span>](power-bi-embedded-app-token-flow.md)  
[<span data-ttu-id="c9613-152">Power BI Desktop</span><span class="sxs-lookup"><span data-stu-id="c9613-152">Power BI Desktop</span></span>](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)  
[<span data-ttu-id="c9613-153">JavaScript beágyazási minta</span><span class="sxs-lookup"><span data-stu-id="c9613-153">JavaScript Embed Sample</span></span>](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[<span data-ttu-id="c9613-154">A Power BI Core NuGut csomag</span><span class="sxs-lookup"><span data-stu-id="c9613-154">Power BI Core NuGut Package</span></span>](https://www.nuget.org/packages/Microsoft.PowerBI.Core/)  
[<span data-ttu-id="c9613-155">A Power BI JavaScript-csomag</span><span class="sxs-lookup"><span data-stu-id="c9613-155">Power BI JavaScript package</span></span>](https://www.nuget.org/packages/Microsoft.PowerBI.JavaScript/)  
<span data-ttu-id="c9613-156">További kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="c9613-156">More questions?</span></span> [<span data-ttu-id="c9613-157">Tegye próbára a Power BI közösségét</span><span class="sxs-lookup"><span data-stu-id="c9613-157">Try the Power BI Community</span></span>](http://community.powerbi.com/)