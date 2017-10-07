---
title: "a webes alkalmazás teljesítmény változásainak Azure Application Insights aaaSmart diagnosztika |} Microsoft Docs"
description: "Automatikus diagnosztizálására igényeiben jelentkező vagy a webes alkalmazás teljesítmény telemetriai lépéseit."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/16/2017
ms.author: cfreeman
ms.openlocfilehash: 8891762c4a4bfdb08b647fe3b702349eb30ec9c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-sudden-changes-in-your-app-telemetry"></a><span data-ttu-id="d6974-103">Az alkalmazás telemetriai hirtelen változásai diagnosztizálása</span><span class="sxs-lookup"><span data-stu-id="d6974-103">Diagnose sudden changes in your app telemetry</span></span>

<span data-ttu-id="d6974-104">*A funkció jelenleg előzetes verzió.*</span><span class="sxs-lookup"><span data-stu-id="d6974-104">*This feature is in preview.*</span></span>

<span data-ttu-id="d6974-105">A webalkalmazás teljesítmény- vagy egyetlen kattintással használat hirtelen változásai diagnosztizálása!</span><span class="sxs-lookup"><span data-stu-id="d6974-105">Diagnose sudden changes in your web app’s performance or usage with a single click!</span></span> <span data-ttu-id="d6974-106">hello intelligens diagnosztika szolgáltatás esetén érhető el, amikor egy idő diagramot a [Analytics](app-insights-analytics.md) a [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d6974-106">hello Smart Diagnostics feature is available whenever you create a time chart in [Analytics](app-insights-analytics.md) in [Application Insights](app-insights-overview.md).</span></span> <span data-ttu-id="d6974-107">Bárhol egy szokatlan módosul az eredményeket, például egy csúcs vagy egy dip hello alakulását az intelligens diagnosztika azonosítja a minta dimenziók és a kapcsolódó értékek, amelyek elmagyarázhatja hello módosítása.</span><span class="sxs-lookup"><span data-stu-id="d6974-107">Wherever there is an unusual change from hello trend of your results, such as a spike or a dip, Smart Diagnostics identifies a pattern of dimensions and related values that might explain hello change.</span></span> <span data-ttu-id="d6974-108">Ezzel a megoldással hello probléma diagnosztizálása gyorsan.</span><span class="sxs-lookup"><span data-stu-id="d6974-108">This helps you diagnose hello problem quickly.</span></span> 

<span data-ttu-id="d6974-109">Ebben a példában intelligens diagnosztikai észlelt egy minta a tulajdonságértékek hello módosítás társított, és kiemeli a eredményeket használatával és anélkül, hogy a minta hello különbsége:</span><span class="sxs-lookup"><span data-stu-id="d6974-109">In this example, Smart Diagnostics has identified a pattern of property values associated with hello change, and highlights hello difference between results with and without that pattern:</span></span>

![Példa analytics diagnosztika eredménye](./media/app-insights-analytics-diagnostics/analytics-result.png)
 

## <a name="diagnose-data-changes"></a><span data-ttu-id="d6974-111">Adatok változásait diagnosztizálása</span><span class="sxs-lookup"><span data-stu-id="d6974-111">Diagnose data changes</span></span>

1.  <span data-ttu-id="d6974-112">Az Analytics-lekérdezés futtatható, és megjelenítheti idő diagramként.</span><span class="sxs-lookup"><span data-stu-id="d6974-112">Run a query in Analytics, and render it as a time chart.</span></span> 
2.  <span data-ttu-id="d6974-113">Ha van ilyen, kattintson a bármely kiemelt csúcs pont.</span><span class="sxs-lookup"><span data-stu-id="d6974-113">Click any highlighted peak point, if there is one.</span></span>
 
    ![csúcsidőszak pont](./media/app-insights-analytics-diagnostics/peak.png)

    <span data-ttu-id="d6974-115">Diagnosztika néhány másodpercet vesz igénybe toodiscover egy mintát.</span><span class="sxs-lookup"><span data-stu-id="d6974-115">Diagnostics takes a few seconds toodiscover a pattern.</span></span>

3. <span data-ttu-id="d6974-116">hello diagnosztikai eredmények lapon látható egy mintát, amely előfordulhat, hogy az adatok folytonosságában ismertetik.</span><span class="sxs-lookup"><span data-stu-id="d6974-116">hello Diagnostics Results tab shows a pattern which may explain your data discontinuity.</span></span>

    ![eredménye](./media/app-insights-analytics-diagnostics/result.png)
 
    <span data-ttu-id="d6974-118">hello szöveg jelenik meg a hello shift toocorrelate hello dimenzióértékek jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="d6974-118">hello text shows hello dimension values that appear toocorrelate with hello shift.</span></span> <span data-ttu-id="d6974-119">Ebben a példában van társítva egy adott kérés és egy adott böngésző verziószáma.</span><span class="sxs-lookup"><span data-stu-id="d6974-119">In this example, it’s associated with a particular request and a particular browser version.</span></span>

    <span data-ttu-id="d6974-120">Figyelje meg hello két összetevői hello diagramot, hello szűrő true és false.</span><span class="sxs-lookup"><span data-stu-id="d6974-120">Notice also hello two components of hello chart, with hello filter true and false.</span></span> <span data-ttu-id="d6974-121">hello hamis összetevő egy változatlan trend jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="d6974-121">hello false component shows an unchanged trend.</span></span> <span data-ttu-id="d6974-122">Más szóval nincs változás hello telemetriai eredmények között, ha diagnosztika azonosított dimenziók problematikus kombinációja hello zárja azt.</span><span class="sxs-lookup"><span data-stu-id="d6974-122">In other words, there is no change in hello telemetry results, if we exclude hello problematic combination of dimensions that Diagnostics has identified.</span></span> <span data-ttu-id="d6974-123">Ezzel szemben hello belül kombinációt eredményben jelentős változás vizsgálat kiemelt hello területen belül.</span><span class="sxs-lookup"><span data-stu-id="d6974-123">By contrast, hello results within that combination do show a dramatic change within hello highlighted area of investigation.</span></span> <span data-ttu-id="d6974-124">Ez azt jelenti, hogy diagnosztika megtalálható-e tulajdonságok kombinációja, amely ismerteti a hello módosítása.</span><span class="sxs-lookup"><span data-stu-id="d6974-124">This shows that Diagnostics has found a combination of properties that explains hello change.</span></span>

4.  <span data-ttu-id="d6974-125">Ha hello minta összetett, át kell toohover **az összes megjelenítése** toosee hello dimenziókat.</span><span class="sxs-lookup"><span data-stu-id="d6974-125">If hello pattern is complex, you need toohover over **Show all** toosee hello dimensions.</span></span>

    ![összes megjelenítése](./media/app-insights-analytics-diagnostics/show-all.png)
 
5.  <span data-ttu-id="d6974-127">Abban az esetben diagnosztika nincs jelentős mintát toonotify kapcsolatos, nem találatokat tartalmazó lap fog érzékelni hello talál.</span><span class="sxs-lookup"><span data-stu-id="d6974-127">In case Diagnostics finds no significant pattern toonotify about, hello ‘no results’ page will be presented.</span></span> <span data-ttu-id="d6974-128">Ezen a ponton módosíthatja a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="d6974-128">At this point, you may change your query.</span></span> <span data-ttu-id="d6974-129">Például szűkítheti hello időtartomány és dobozolás Analytics a lekérdezésben, egy további elemzés és potenciálisan jobb eredményeket.</span><span class="sxs-lookup"><span data-stu-id="d6974-129">For example, you could narrow hello time range and binning in Analytics query, for a further analysis and potentially better results.</span></span>

<span data-ttu-id="d6974-130">Volt az, hogy rendelkezik-e egy adott oldal, a webhely probléma egy adott böngésző hello ismeretében képes felvértezve, most lépjen egyenes toohello probléma lapra, és vizsgálja meg a legutóbbi módosítások.</span><span class="sxs-lookup"><span data-stu-id="d6974-130">Armed with hello knowledge that a particular page of your website has a problem on a particular browser, you can now go straight toohello problem page, and investigate recent changes.</span></span>

## <a name="try-hello-demo"></a><span data-ttu-id="d6974-131">Próbálja meg hello bemutató</span><span class="sxs-lookup"><span data-stu-id="d6974-131">Try hello demo</span></span>

<span data-ttu-id="d6974-132">[Kattintson ide a bemutató toosee](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA3VSTY%2FTQAy991dYPXWlLf0QIO2KIiGWA3duiMPsxEnMzhe2p6WIH48nVUsuGylRNPOe3%2FOzN5vFZgPfRhL4VZHPIGM%2BCdgHdESgpMjOKx0RnsgNKYuSF%2BjRaWUE7xKMGIoBgTpMSv2Z0jBxOWc1QBWEPjM4EMUCP2uc0A3x8E5HKMi%2BEQNC7oHRbIgKdJWdUk5vmr9PvdkArildit%2Fcrk0lBDjnyhBzk%2FKVxdTy0QhNY6RhDPYqdlCy9XMV96NjBZc68IH8y6Tzuf01iZxeIZ%2FI5DqMOYmaQQRXNUdz6qGb5WOdSKEXnOozHtEFK%2Bh0qnq5YQzGF9DcoinoqbcigkO0NOZRNGOZaaBkMuat5xznFOtULKhG%2BdrGlVDhy%2B8SMlsETV8dD6gTd0YrbsBrFq6U1v%2Filv4C%2FsJpRJuwUrQTZ0P7eIDOHLeD1X67e7%2Fe7dbbB9htH%2Ffbu4vQDfvhFez%2B8a1h%2F1f3VSy%2BJ4Ol1oN8X4qN0qMZWv44HJanzKFLeJIltKcRpcbomP7gbHNkdV2Xe1uqO3g%2BwzOl1c3PvbmMlC7KjKlry2GX0w4s%2FgFoo5%2BhBAMAAA%3D%3D&timespan=PT24H) a mintaadatokat.</span><span class="sxs-lookup"><span data-stu-id="d6974-132">[Click here toosee a demonstration](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA3VSTY%2FTQAy991dYPXWlLf0QIO2KIiGWA3duiMPsxEnMzhe2p6WIH48nVUsuGylRNPOe3%2FOzN5vFZgPfRhL4VZHPIGM%2BCdgHdESgpMjOKx0RnsgNKYuSF%2BjRaWUE7xKMGIoBgTpMSv2Z0jBxOWc1QBWEPjM4EMUCP2uc0A3x8E5HKMi%2BEQNC7oHRbIgKdJWdUk5vmr9PvdkArildit%2Fcrk0lBDjnyhBzk%2FKVxdTy0QhNY6RhDPYqdlCy9XMV96NjBZc68IH8y6Tzuf01iZxeIZ%2FI5DqMOYmaQQRXNUdz6qGb5WOdSKEXnOozHtEFK%2Bh0qnq5YQzGF9DcoinoqbcigkO0NOZRNGOZaaBkMuat5xznFOtULKhG%2BdrGlVDhy%2B8SMlsETV8dD6gTd0YrbsBrFq6U1v%2Filv4C%2FsJpRJuwUrQTZ0P7eIDOHLeD1X67e7%2Fe7dbbB9htH%2Ffbu4vQDfvhFez%2B8a1h%2F1f3VSy%2BJ4Ol1oN8X4qN0qMZWv44HJanzKFLeJIltKcRpcbomP7gbHNkdV2Xe1uqO3g%2BwzOl1c3PvbmMlC7KjKlry2GX0w4s%2FgFoo5%2BhBAMAAA%3D%3D&timespan=PT24H) on sample data.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="d6974-133">Működés</span><span class="sxs-lookup"><span data-stu-id="d6974-133">How it works</span></span>

<span data-ttu-id="d6974-134">Intelligens diagnosztika hello alapján speciális felügyeletlen gépi tanulási algoritmust használ [DiffPatterns](app-insights-analytics-reference.md) műveletet.</span><span class="sxs-lookup"><span data-stu-id="d6974-134">Smart Diagnostics uses an advanced unsupervised machine learning algorithm based on hello [DiffPatterns](app-insights-analytics-reference.md) operation.</span></span> <span data-ttu-id="d6974-135">Elmagyarázhatja hello változását jelölt mintázatokat keresi.</span><span class="sxs-lookup"><span data-stu-id="d6974-135">It looks for candidate patterns that might explain hello data change.</span></span> <span data-ttu-id="d6974-136">Minden jelölt a hello metrika hello hatását elemzi, és viselkedésmintát hello, hogy a legjobb korrelációban hello módosítása.</span><span class="sxs-lookup"><span data-stu-id="d6974-136">It analyses hello impact of each candidate on hello metric, and shows hello pattern that best correlates with hello change.</span></span>

## <a name="no-diagnostic-points"></a><span data-ttu-id="d6974-137">Olyan diagnosztikai pontot?</span><span class="sxs-lookup"><span data-stu-id="d6974-137">No diagnostic points?</span></span>

<span data-ttu-id="d6974-138">Intelligens diagnosztika csak akkor működik, amikor hello a következő feltételek teljesülnek:</span><span class="sxs-lookup"><span data-stu-id="d6974-138">Smart Diagnostics only works when hello following criteria are satisfied:</span></span>

 * <span data-ttu-id="d6974-139">Intelligens diagnosztika beállítás kapcsolni.</span><span class="sxs-lookup"><span data-stu-id="d6974-139">Smart Diagnostics setting is switched on.</span></span> <span data-ttu-id="d6974-140">Keresse meg a Analytics hello-beállítások ikonra.</span><span class="sxs-lookup"><span data-stu-id="d6974-140">Look under hello Settings icon in Analytics.</span></span>
 * <span data-ttu-id="d6974-141">hello beállításai intelligens diagnosztika beállítás van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="d6974-141">hello Smart Diagnostics option in Analytics settings is selected.</span></span> 
 * <span data-ttu-id="d6974-142">Időtengelye: hello hello diagram x tengelyéhez típusúnak kell lennie `datetime`.</span><span class="sxs-lookup"><span data-stu-id="d6974-142">Time axis: hello X-axis of hello chart must be of type `datetime`.</span></span>
 * <span data-ttu-id="d6974-143">Sor-vagy terület: diagnosztika csak akkor működik, az ilyen típusú diagram.</span><span class="sxs-lookup"><span data-stu-id="d6974-143">Line or area chart: Diagnostics only works these types of chart.</span></span> <span data-ttu-id="d6974-144">Használjon `| render timechart` vagy `| render areachart` hello végén a lekérdezést; vagy válasszon sor vagy területdiagram hello legördülő választó.</span><span class="sxs-lookup"><span data-stu-id="d6974-144">Use `| render timechart` or `| render areachart` at hello end of your query; or select Line or Area Chart from hello drop-down selector.</span></span>
 * <span data-ttu-id="d6974-145">Folytonosságában: Szerepelnie kell jelentős kihagyást hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="d6974-145">Discontinuity: There must be a significant discontinuity in hello data.</span></span>
 * <span data-ttu-id="d6974-146">Elegendő pontok tooanalyze.</span><span class="sxs-lookup"><span data-stu-id="d6974-146">Sufficient points tooanalyze.</span></span>
 * <span data-ttu-id="d6974-147">Legfeljebb egy foglalják össze a hello lekérdezés záradékában.</span><span class="sxs-lookup"><span data-stu-id="d6974-147">No more than one summarize clause in hello query.</span></span>
 * <span data-ttu-id="d6974-148">Nincs projektet tartalmazó záradékot, amely egy név-definíciót, mielőtt hello záradék foglalják össze.</span><span class="sxs-lookup"><span data-stu-id="d6974-148">No project clause that contains a name definition before hello summarize clause.</span></span>

 
 ## <a name="related-articles"></a><span data-ttu-id="d6974-149">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="d6974-149">Related articles</span></span>

 * [<span data-ttu-id="d6974-150">Elemzés oktatóanyag</span><span class="sxs-lookup"><span data-stu-id="d6974-150">Analytics tutorial</span></span>](app-insights-analytics-tour.md)
 * <span data-ttu-id="d6974-151">[Észlelési intelligens](app-insights-proactive-diagnostics.md) automatikusan riasztást küld a tooperformance problémákat.</span><span class="sxs-lookup"><span data-stu-id="d6974-151">[Smart detection](app-insights-proactive-diagnostics.md) automatically alerts you tooperformance issues.</span></span>
