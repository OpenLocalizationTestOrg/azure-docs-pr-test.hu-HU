---
title: "aaaVisualize és Stream Analytics-feladatok hibaelhárítása |} Microsoft Docs"
description: "Ismerje meg, hogyan toovisualize a Stream Analytics-feladat a következő feldolgozási sorban az önkiszolgáló hibaelhárítás hello diagnosztika diagram funkció segítségével."
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: d87841cd-c59f-4a46-b46e-8b904fdc12e9
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 8a6715be601fdc47b8d9caf4112da161dad22618
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-and-troubleshoot-stream-analytics-jobs"></a><span data-ttu-id="32b72-103">Megjelenítheti és a Stream Analytics-feladatok hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="32b72-103">Visualize and troubleshoot Stream Analytics jobs</span></span>
<span data-ttu-id="32b72-104">A Stream Analytics más felhőalapú technológiákat a hibaelhárítás néha szükséges toolook történő miért egy feladat nem készít hello várható kimenet (vagy adott függetlenül attól, hogy a kimenetet).</span><span class="sxs-lookup"><span data-stu-id="32b72-104">In Stream Analytics, as with other cloud-based technologies, troubleshooting is sometimes needed toolook into why a job does not produce hello expected output (or any output for that matter).</span></span> <span data-ttu-id="32b72-105">Ennek tudatában Stream Analytics lehetővé teszi hello megjelenítésére a folyamatos átviteli feladatnak.</span><span class="sxs-lookup"><span data-stu-id="32b72-105">With this in mind, Stream Analytics provides hello capability for visualizing a streaming job.</span></span> <span data-ttu-id="32b72-106">Ez is modellezési eszközként lesz szüksége, amely hello ügyféloldali juttatása kívánó a munka dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="32b72-106">This is also handy as a modeling tool and has hello side benefit for those requiring documentation of their work.</span></span>

<span data-ttu-id="32b72-107">Az hello képi megjelenítés panel hello bemenetek láthatók, valamint hello lekérdezés-végrehajtás alatt, és majd mindegyikhez hello konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="32b72-107">In hello visualization panel hello inputs are visible as well as hello query being executed and then all hello outputs configured.</span></span> <span data-ttu-id="32b72-108">Kapcsolat vagy konfigurációs probléma több nyilvánvaló válhat, és hasznos toosee a konfigurációs vizuális ábrázolását is lehet.</span><span class="sxs-lookup"><span data-stu-id="32b72-108">Connectivity or configuration issues can become more apparent and it can also be helpful toosee a visual representation of your configuration.</span></span>

## <a name="using-hello-diagnosis-diagram-tool"></a><span data-ttu-id="32b72-109">Hello diagnosztizálása diagram eszköz használatával</span><span class="sxs-lookup"><span data-stu-id="32b72-109">Using hello diagnosis diagram tool</span></span>
<span data-ttu-id="32b72-110">a vizualizálója, egyszerűen kattintson a "Diagnosztika diagram" gombra a hello tooaccess hello hello hello Stream Analytics-feladat "Beállítások" panel.</span><span class="sxs-lookup"><span data-stu-id="32b72-110">tooaccess this visualizer, simply click on hello “Diagnosis diagram” button in hello “Settings” blade of hello of hello Stream Analytics job.</span></span>

![Stream-Analytics-troubleshoot-Visualization-Diagnosis-diagram](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-diagnosis-diagram1.png)

<span data-ttu-id="32b72-112">Minden bemeneti és kimeneti színkóddal tooindicate hello aktuális adott összetevő állapotát, a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="32b72-112">Every input and output is color coded tooindicate hello current state of that component, as shown below.</span></span>

![Stream-Analytics-troubleshoot-Visualization-input-Map](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-input-map.png)

<span data-ttu-id="32b72-114">Hello felhasználói toolook köztes lekérdezési lépéseket toounderstand hello adatok folyamata mintáknak belül egy feladatot, azt szeretné, ha hello képi megjelenítés eszköz áttekintést nyújt a hello lekérdezés hello részletes információkat az összetevő lépéseket és hello folyamata feladatütemezési.</span><span class="sxs-lookup"><span data-stu-id="32b72-114">When hello user wants toolook at intermediate query steps toounderstand hello data flow patterns inside a job, hello visualization tool provides a view of hello breakdown of hello query into its component steps and hello flow sequence.</span></span> <span data-ttu-id="32b72-115">Minden egyes lekérdezés lépésben kattint, megjelenik egy lekérdezés-szerkesztő ablaktáblán, amint a hello megfelelő szakaszához.</span><span class="sxs-lookup"><span data-stu-id="32b72-115">Clicking on each query step will show hello corresponding section in a query editing pane as illustrated.</span></span> 

![Stream-Analytics-troubleshoot-Visualization-Intermediate-steps](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-intermediate-steps.png)

## <a name="next-steps"></a><span data-ttu-id="32b72-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="32b72-117">Next steps</span></span>
* [<span data-ttu-id="32b72-118">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="32b72-118">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="32b72-119">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="32b72-119">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="32b72-120">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="32b72-120">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="32b72-121">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="32b72-121">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="32b72-122">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="32b72-122">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

