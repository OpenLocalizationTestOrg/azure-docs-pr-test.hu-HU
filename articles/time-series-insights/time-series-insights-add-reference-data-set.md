---
title: "Referencia-adatkészlet hozzáadása Azure Time Series Insight-környezethez | Microsoft Docs"
description: "Ebben az oktatóanyagban referencia-adatkészletet fog hozzáadni az Azure Time Series Insights-környezetéhez"
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
ms.openlocfilehash: 23444297b5231e6a026bcd9ce3ee9f943bf05867
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-reference-data-set-for-your-time-series-insights-environment-using-the-ibiza-portal"></a><span data-ttu-id="ce5d4-103">Referencia-adatkészlet létrehozása Azure Time Series Insights-környezethez az Ibiza Portal használatával</span><span class="sxs-lookup"><span data-stu-id="ce5d4-103">Create a reference data set for your Time Series Insights environment using the Ibiza portal</span></span>

<span data-ttu-id="ce5d4-104">Egy referencia-adatkészlet az eseményforrásbeli eseményekkel kibővített elemek gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="ce5d4-104">A Reference Data Set is a collection of items that are augmented with the events from your event source.</span></span> <span data-ttu-id="ce5d4-105">A Time Series Insights bejövő forgalmat kezelő motorja a referencia-adatkészlet egy elemét csatolja egy eseményforrásbeli eseményhez.</span><span class="sxs-lookup"><span data-stu-id="ce5d4-105">Time Series Insights ingress engine joins an event from your event source with an item in your reference data set.</span></span> <span data-ttu-id="ce5d4-106">Ez a kibővített esemény ezután lekérdezhető.</span><span class="sxs-lookup"><span data-stu-id="ce5d4-106">This augmented event is then available for query.</span></span> <span data-ttu-id="ce5d4-107">A csatolás a referencia-adatkészletben definiált kulcsokon alapul.</span><span class="sxs-lookup"><span data-stu-id="ce5d4-107">This join is based on the keys defined in your reference data set.</span></span>

## <a name="steps-to-add-a-reference-data-set-to-your-environment"></a><span data-ttu-id="ce5d4-108">Referencia-adatkészlet környezethez adása lépésenként</span><span class="sxs-lookup"><span data-stu-id="ce5d4-108">Steps to add a reference data set to your environment</span></span>

1. <span data-ttu-id="ce5d4-109">Jelentkezzen be az [Ibiza Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ce5d4-109">Sign in to the [Ibiza portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ce5d4-110">Az Ibiza Portal bal oldali menüjében kattintson a „Minden erőforrás” lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="ce5d4-110">Click “All resources” in the menu on the left side of the Ibiza portal.</span></span>
3. <span data-ttu-id="ce5d4-111">Válassza ki az Azure Time Series Insights-környezetet.</span><span class="sxs-lookup"><span data-stu-id="ce5d4-111">Select your Time Series Insights environment.</span></span>

    ![A Time Series Insights referencia-adatkészlet létrehozása](media/add-reference-data-set/getstarted-create-reference-data-set-1.png)

4. <span data-ttu-id="ce5d4-113">Válassza a „Referencia-adatkészletek” lehetőséget, majd kattintson a „+ Hozzáadás” gombra.</span><span class="sxs-lookup"><span data-stu-id="ce5d4-113">Select “Reference Data Sets”, click “+ Add.”</span></span>

    ![A Time Series Insights referencia-adatkészlet létrehozása – részletesen](media/add-reference-data-set/getstarted-create-reference-data-set-2.png)

5. <span data-ttu-id="ce5d4-115">Adja meg a referencia-adatkészlet nevét.</span><span class="sxs-lookup"><span data-stu-id="ce5d4-115">Specify the name of the reference data set.</span></span>
6. <span data-ttu-id="ce5d4-116">Adja meg a kulcs nevét és típusát.</span><span class="sxs-lookup"><span data-stu-id="ce5d4-116">Specify the key name and its type.</span></span> <span data-ttu-id="ce5d4-117">E név és típus alapján lesz kiválasztva a megfelelő tulajdonság az eseményforrásbeli eseményből.</span><span class="sxs-lookup"><span data-stu-id="ce5d4-117">This name and type is used to pick the correct property from the event in your event source.</span></span> <span data-ttu-id="ce5d4-118">Ha kulcsnévként például „EszkozAzon”-t ad meg, típusként pedig karakterláncot („String”), akkor a Time Series Insights bejövő forgalmat kezelő motorja egy „EszkozAzon” nevű, karakterlánc típusú tulajdonságot fog keresni a beérkező eseményben.</span><span class="sxs-lookup"><span data-stu-id="ce5d4-118">For instance, if you provide key name as “DeviceId” and type as “String”, then the Time Series Insights ingress engine looks for a property with the name “DeviceId” of type “String” in the incoming event.</span></span> <span data-ttu-id="ce5d4-119">Az eseményhez csatoláshoz egynél több kulcs is megadható.</span><span class="sxs-lookup"><span data-stu-id="ce5d4-119">You can provide more than one key to join with the event.</span></span> <span data-ttu-id="ce5d4-120">A tulajdonságnév egyeztetésénél számítanak a kis- és nagybetűk.</span><span class="sxs-lookup"><span data-stu-id="ce5d4-120">The property name match is case-sensitive.</span></span>

     ![A Time Series Insights referencia-adatkészlet létrehozása – részletesen](media/add-reference-data-set/getstarted-create-reference-data-set-3.png)

7. <span data-ttu-id="ce5d4-122">Kattintson a „Létrehozás” elemre.</span><span class="sxs-lookup"><span data-stu-id="ce5d4-122">Click “Create.”</span></span>

## <a name="next-steps"></a><span data-ttu-id="ce5d4-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ce5d4-123">Next steps</span></span>

* <span data-ttu-id="ce5d4-124">[Referencia-adatok kezelése](time-series-insights-manage-reference-data-csharp.md) programozott módon.</span><span class="sxs-lookup"><span data-stu-id="ce5d4-124">[Manage reference data](time-series-insights-manage-reference-data-csharp.md) programmatically.</span></span>
* <span data-ttu-id="ce5d4-125">Az alkalmazásprogramozási felület (API) teljes leírását a [Referencia-adatok API](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api) dokumentum tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ce5d4-125">For the complete API reference, see [Reference Data API](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api) document.</span></span>