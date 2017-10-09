---
title: az Application Insights aaaRelease jegyzetek |} Microsoft Docs
description: "Központi telepítés hozzáadása, vagy összeállíthatja a jelölők tooyour metrikák explorer diagramokat az Application Insightsban."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 23173e33-d4f2-4528-a730-913a8fd5f02e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: bwren
ms.openlocfilehash: e802d22701cb69e96fd1a6b469ea67454195f77a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="annotations-on-metric-charts-in-application-insights"></a><span data-ttu-id="b9e2f-103">Az Application Insightsban metrika diagramok jegyzetek</span><span class="sxs-lookup"><span data-stu-id="b9e2f-103">Annotations on metric charts in Application Insights</span></span>
<span data-ttu-id="b9e2f-104">A jegyzetek [Metrikaböngésző](app-insights-metrics-explorer.md) diagramok megjelenítése, amelyen rendszerbe van állítva egy új buildverziót, vagy más jelentős esemény történt.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-104">Annotations on [Metrics Explorer](app-insights-metrics-explorer.md) charts show where you deployed a new build, or other significant event.</span></span> <span data-ttu-id="b9e2f-105">Ezek teszik, hogy könnyen toosee, hogy a módosítások az alkalmazás teljesítményére hatással volt.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-105">They make it easy toosee whether your changes had any effect on your application's performance.</span></span> <span data-ttu-id="b9e2f-106">Ezek automatikusan létrehozhatók hello [Visual Studio Team Services rendszer build](https://www.visualstudio.com/en-us/get-started/build/build-your-app-vs).</span><span class="sxs-lookup"><span data-stu-id="b9e2f-106">They can be automatically created by hello [Visual Studio Team Services build system](https://www.visualstudio.com/en-us/get-started/build/build-your-app-vs).</span></span> <span data-ttu-id="b9e2f-107">Is létrehozhat jegyzetek tooflag tetszés szerint mindenképpen [hozza létre őket a Powershellből](#create-annotations-from-powershell).</span><span class="sxs-lookup"><span data-stu-id="b9e2f-107">You can also create annotations tooflag any event you like by [creating them from PowerShell](#create-annotations-from-powershell).</span></span>

![Kiszolgáló válaszideje látható korrelációban állnak a jegyzetek – példa](./media/app-insights-annotations/00.png)



## <a name="release-annotations-with-vsts-build"></a><span data-ttu-id="b9e2f-109">Kiadási jegyzetek a VSTS-build</span><span class="sxs-lookup"><span data-stu-id="b9e2f-109">Release annotations with VSTS build</span></span>

<span data-ttu-id="b9e2f-110">Kiadási jegyzetek a hello felhőalapú build szolgáltatása, és felszabadíthatja a Visual Studio Team Services szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-110">Release annotations are a feature of hello cloud-based build and release service of Visual Studio Team Services.</span></span> 

### <a name="install-hello-annotations-extension-one-time"></a><span data-ttu-id="b9e2f-111">Hello jegyzetek kiterjesztés (egyszer) telepítése</span><span class="sxs-lookup"><span data-stu-id="b9e2f-111">Install hello Annotations extension (one time)</span></span>
<span data-ttu-id="b9e2f-112">toobe képes toocreate kiadási jegyzetek, szüksége lesz egy tooinstall a hello sok Team bővítmények érhető el a Visual Studio piactér hello.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-112">toobe able toocreate release annotations, you'll need tooinstall one of hello many Team Service extensions available in hello Visual Studio Marketplace.</span></span>

1. <span data-ttu-id="b9e2f-113">Jelentkezzen be tooyour [Visual Studio Team Services](https://www.visualstudio.com/en-us/get-started/setup/sign-up-for-visual-studio-online) projekt.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-113">Sign in tooyour [Visual Studio Team Services](https://www.visualstudio.com/en-us/get-started/setup/sign-up-for-visual-studio-online) project.</span></span>
2. <span data-ttu-id="b9e2f-114">A Visual Studio piactéren [hello kiadási jegyzetek bővítmény](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations), és adja hozzá tooyour Team Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-114">In Visual Studio Marketplace, [get hello Release Annotations extension](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations), and add it tooyour Team Services account.</span></span>

![At Team Services weblap, nyissa meg piactér jobb felső.](./media/app-insights-annotations/10.png)

<span data-ttu-id="b9e2f-117">Csak akkor kell toodo egyszer ebben a Visual Studio Team Services-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-117">You only need toodo this once for your Visual Studio Team Services account.</span></span> <span data-ttu-id="b9e2f-118">Kiadási jegyzetek a projektre a fiókját most konfigurálhatók.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-118">Release annotations can now be configured for any project in your account.</span></span> 

### <a name="configure-release-annotations"></a><span data-ttu-id="b9e2f-119">Kiadási jegyzetek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b9e2f-119">Configure release annotations</span></span>

<span data-ttu-id="b9e2f-120">Minden VSTS-kiadás sablon tooget egy külön API-kulcs szükséges.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-120">You need tooget a separate API key for each VSTS release template.</span></span>

1. <span data-ttu-id="b9e2f-121">Jelentkezzen be toohello [Microsoft Azure portál](https://portal.azure.com) , és nyissa meg a hello Application Insights-erőforrást, amely az alkalmazás figyeli.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-121">Sign in toohello [Microsoft Azure Portal](https://portal.azure.com) and open hello Application Insights resource that monitors your application.</span></span> <span data-ttu-id="b9e2f-122">(Vagy [létrehozhat egy tárhelyet](app-insights-overview.md), ha még nem tette meg még.)</span><span class="sxs-lookup"><span data-stu-id="b9e2f-122">(Or [create one now](app-insights-overview.md), if you haven't done so yet.)</span></span>
2. <span data-ttu-id="b9e2f-123">Nyissa meg **API-hozzáférés**, **Application Insights azonosító**.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-123">Open **API Access**,  **Application Insights Id**.</span></span>
   
    ![A portal.azure.com nyissa meg az Application Insights-erőforrást, majd válassza a beállítások.](./media/app-insights-annotations/20.png)

4. <span data-ttu-id="b9e2f-127">Egy külön böngészőablakban nyissa meg a (vagy hozzon létre), amely a központi telepítések kezeli a Visual Studio Team Services hello kiadási sablon.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-127">In a separate browser window, open (or create) hello release template that manages your deployments from Visual Studio Team Services.</span></span> 
   
    <span data-ttu-id="b9e2f-128">Adjon hozzá egy feladatot, és válassza a hello Application Insights kiadási Megjegyzés feladat hello menüpontot.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-128">Add a task, and select hello Application Insights Release Annotation task from hello menu.</span></span>
   
    <span data-ttu-id="b9e2f-129">Beillesztés hello **alkalmazásazonosító** hello API-hozzáférés panel fájlból másolt.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-129">Paste hello **Application Id** that you copied from hello API Access blade.</span></span>
   
    ![A Visual Studio Team Services nyissa meg a kiadási válassza ki a kiadási definícióját, kattintson a Szerkesztés.](./media/app-insights-annotations/30.png)
4. <span data-ttu-id="b9e2f-133">Set hello **APIKey** mező tooa változó `$(ApiKey)`.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-133">Set hello **APIKey** field tooa variable `$(ApiKey)`.</span></span>

5. <span data-ttu-id="b9e2f-134">Hello Azure ablak hozzon létre egy új API-kulcsot, és igénybe vehet egy példányát.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-134">Back in hello Azure window, create a new API Key and take a copy of it.</span></span>
   
    ![Hello API-hozzáférés panel az hello Azure ablak kattintson az API-kulcs létrehozása.](./media/app-insights-annotations/40.png)

6. <span data-ttu-id="b9e2f-138">Nyissa meg a hello kiadási sablon hello konfiguráció lapon.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-138">Open hello Configuration tab of hello release template.</span></span>
   
    <span data-ttu-id="b9e2f-139">Hozzon létre egy változó definíciója `ApiKey`.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-139">Create a variable definition for `ApiKey`.</span></span>
   
    <span data-ttu-id="b9e2f-140">Illessze be az API fő toohello ApiKey változó definícióját.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-140">Paste your API key toohello ApiKey variable definition.</span></span>
   
    ![Hello Team Services ablakban jelölje ki a hello konfigurációs lapján, és kattintson a változó hozzáadása.](./media/app-insights-annotations/50.png)
7. <span data-ttu-id="b9e2f-143">Végezetül **mentése** hello kiadás definíciója.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-143">Finally, **Save** hello release definition.</span></span>


## <a name="view-annotations"></a><span data-ttu-id="b9e2f-144">Jegyzetek megtekintése</span><span class="sxs-lookup"><span data-stu-id="b9e2f-144">View annotations</span></span>
<span data-ttu-id="b9e2f-145">Most hello kiadási sablon toodeploy új kiadását használja, amikor a jegyzet küld tooApplication Insights.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-145">Now, whenever you use hello release template toodeploy a new release, an annotation will be sent tooApplication Insights.</span></span> <span data-ttu-id="b9e2f-146">hello jegyzetek a Metrikaböngészőben diagramok jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-146">hello annotations will appear on charts in Metrics Explorer.</span></span>

<span data-ttu-id="b9e2f-147">Kattintson a bármely hello kiadás, például kérelmező, forrás vezérlő fiókirodai, kiadási definition, környezetre és egyéb jegyzet jelölő tooopen részleteit.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-147">Click on any annotation marker tooopen details about hello release, including requestor, source control branch, release definition, environment, and more.</span></span>

![Kattintson a kiadási Megjegyzés jelölő.](./media/app-insights-annotations/60.png)

## <a name="create-custom-annotations-from-powershell"></a><span data-ttu-id="b9e2f-149">Hozzon létre egyéni jegyzetek a Powershellből</span><span class="sxs-lookup"><span data-stu-id="b9e2f-149">Create custom annotations from PowerShell</span></span>
<span data-ttu-id="b9e2f-150">Jegyzetek e folyamat (nélkül használja a Visual STUDIO Team System) is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-150">You can also create annotations from any process you like (without using VS Team System).</span></span> 


1. <span data-ttu-id="b9e2f-151">Helyi másolat készítése a hello [Powershell-parancsfájl a Githubról](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).</span><span class="sxs-lookup"><span data-stu-id="b9e2f-151">Make a local copy of hello [Powershell script from GitHub](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).</span></span>

2. <span data-ttu-id="b9e2f-152">Hello Alkalmazásazonosító beszerzése és hello API-hozzáférés panelen az API-kulcs létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-152">Get hello Application ID and create an API key from hello API Access blade.</span></span>

3. <span data-ttu-id="b9e2f-153">Ehhez hasonló hello parancsfájl hívása:</span><span class="sxs-lookup"><span data-stu-id="b9e2f-153">Call hello script like this:</span></span>

```PS

     .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }
```

<span data-ttu-id="b9e2f-154">Egyszerű toomodify hello parancsfájl, például toocreate jegyzetek az elmúlt hello.</span><span class="sxs-lookup"><span data-stu-id="b9e2f-154">It's easy toomodify hello script, for example toocreate annotations for hello past.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b9e2f-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b9e2f-155">Next steps</span></span>

* [<span data-ttu-id="b9e2f-156">Munkaelemek létrehozása</span><span class="sxs-lookup"><span data-stu-id="b9e2f-156">Create work items</span></span>](app-insights-diagnostic-search.md#create-work-item)
* [<span data-ttu-id="b9e2f-157">Automatizálása a PowerShell</span><span class="sxs-lookup"><span data-stu-id="b9e2f-157">Automation with PowerShell</span></span>](app-insights-powershell.md)
