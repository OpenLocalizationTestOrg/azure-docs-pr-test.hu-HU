---
title: "aaaAdd hivatkozás adatkészlet tooyour Azure idő adatsorozat Insights környezetben |} Microsoft Docs"
description: "Ebben az oktatóanyagban hivatkozás adatkészlet tooyour idő adatsorozat Insights környezet hozzáadása"
keywords: 
services: time-series-insights
documentationcenter: 
author: venkatgct
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: venkatja
ms.openlocfilehash: 05e626ed81a22f2a8710b23a931ccd17c0f38ca5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-reference-data-set-for-your-time-series-insights-environment-using-hello-ibiza-portal"></a><span data-ttu-id="a44c8-103">Hello Ibiza portálon idő adatsorozat Insights környezetnek hivatkozás adatok készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="a44c8-103">Create a reference data set for your Time Series Insights environment using hello Ibiza portal</span></span>

<span data-ttu-id="a44c8-104">A referencia-adatkészlet hello események az esemény forrását a rendszer kiegészítve elemek gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="a44c8-104">A Reference Data Set is a collection of items that are augmented with hello events from your event source.</span></span> <span data-ttu-id="a44c8-105">A Time Series Insights bejövő forgalmat kezelő motorja a referencia-adatkészlet egy elemét csatolja egy eseményforrásbeli eseményhez.</span><span class="sxs-lookup"><span data-stu-id="a44c8-105">Time Series Insights ingress engine joins an event from your event source with an item in your reference data set.</span></span> <span data-ttu-id="a44c8-106">Ez a kibővített esemény ezután lekérdezhető.</span><span class="sxs-lookup"><span data-stu-id="a44c8-106">This augmented event is then available for query.</span></span> <span data-ttu-id="a44c8-107">Ezt az összekapcsolást hello kulcsok vannak meghatározva a a referencia-adatkészlet alapján történik.</span><span class="sxs-lookup"><span data-stu-id="a44c8-107">This join is based on hello keys defined in your reference data set.</span></span>

## <a name="steps-tooadd-a-reference-data-set-tooyour-environment"></a><span data-ttu-id="a44c8-108">Lépéseket tooadd egy referencia-adatkészlet tooyour környezet</span><span class="sxs-lookup"><span data-stu-id="a44c8-108">Steps tooadd a reference data set tooyour environment</span></span>

1. <span data-ttu-id="a44c8-109">Jelentkezzen be toohello [Ibiza portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a44c8-109">Sign in toohello [Ibiza portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="a44c8-110">Kattintson az "Összes erőforrás" hello menü hello hello Ibiza portálon a bal oldalán.</span><span class="sxs-lookup"><span data-stu-id="a44c8-110">Click “All resources” in hello menu on hello left side of hello Ibiza portal.</span></span>
3. <span data-ttu-id="a44c8-111">Válassza ki az Azure Time Series Insights-környezetet.</span><span class="sxs-lookup"><span data-stu-id="a44c8-111">Select your Time Series Insights environment.</span></span>

    ![Hozzon létre hello idő adatsorozat Insights referencia-adatkészlet](media/add-reference-data-set/getstarted-create-reference-data-set-1.png)

4. <span data-ttu-id="a44c8-113">Válassza a „Referencia-adatkészletek” lehetőséget, majd kattintson a „+ Hozzáadás” gombra.</span><span class="sxs-lookup"><span data-stu-id="a44c8-113">Select “Reference Data Sets”, click “+ Add.”</span></span>

    ![Hozzon létre hello idő adatsorozat Insights referencia-adatkészlet - részletek](media/add-reference-data-set/getstarted-create-reference-data-set-2.png)

5. <span data-ttu-id="a44c8-115">Adja meg a referencia-adatkészlet hello hello nevét.</span><span class="sxs-lookup"><span data-stu-id="a44c8-115">Specify hello name of hello reference data set.</span></span>
6. <span data-ttu-id="a44c8-116">Adja meg a hello kulcsnév és annak típusára.</span><span class="sxs-lookup"><span data-stu-id="a44c8-116">Specify hello key name and its type.</span></span> <span data-ttu-id="a44c8-117">Ilyen nevű és típusú elem használt toopick hello megfelelő tulajdonság az eseményforrás hello eseménytől.</span><span class="sxs-lookup"><span data-stu-id="a44c8-117">This name and type is used toopick hello correct property from hello event in your event source.</span></span> <span data-ttu-id="a44c8-118">Például ha ad meg, mint a "DeviceId" kulcs nevét és a "String" típusú, majd hello idő adatsorozat Insights érkező motor megkeresi a "DeviceId" típus "Karakterlánc" hello bejövő eseményben hello nevű tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="a44c8-118">For instance, if you provide key name as “DeviceId” and type as “String”, then hello Time Series Insights ingress engine looks for a property with hello name “DeviceId” of type “String” in hello incoming event.</span></span> <span data-ttu-id="a44c8-119">Egynél több kulcsfontosságú toojoin hello esemény biztosíthat.</span><span class="sxs-lookup"><span data-stu-id="a44c8-119">You can provide more than one key toojoin with hello event.</span></span> <span data-ttu-id="a44c8-120">hello tulajdonság nevének egyeznie kell az kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="a44c8-120">hello property name match is case-sensitive.</span></span>

     ![Hozzon létre hello idő adatsorozat Insights referencia-adatkészlet - részletek](media/add-reference-data-set/getstarted-create-reference-data-set-3.png)

7. <span data-ttu-id="a44c8-122">Kattintson a „Létrehozás” elemre.</span><span class="sxs-lookup"><span data-stu-id="a44c8-122">Click “Create.”</span></span>

## <a name="next-steps"></a><span data-ttu-id="a44c8-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a44c8-123">Next steps</span></span>

* <span data-ttu-id="a44c8-124">[Referencia-adatok kezelése](time-series-insights-manage-reference-data-csharp.md) programozott módon.</span><span class="sxs-lookup"><span data-stu-id="a44c8-124">[Manage reference data](time-series-insights-manage-reference-data-csharp.md) programmatically.</span></span>
* <span data-ttu-id="a44c8-125">Hello teljes API referenciáért lásd: [referencia az API](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="a44c8-125">For hello complete API reference, see [Reference Data API](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api) document.</span></span>
