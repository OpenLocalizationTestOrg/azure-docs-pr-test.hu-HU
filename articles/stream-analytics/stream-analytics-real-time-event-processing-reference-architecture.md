---
title: "a Stream Analytics esemény feldolgozása aaaReal idő Eseményfeldolgozási |} Microsoft Docs"
description: "Ismerje meg, hogyan képes együttműködni az Azure szolgáltatások valós idejű esemény feldolgozása és az elemzések engedélyezéséhez."
keywords: "valós idejű feldolgozással, az események feldolgozásával, a referencia-architektúrában"
services: stream-analytics,event-hubs,storage,sql-database
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: 
ms.assetid: 11af48bc-313c-4527-8c80-91088dc9f3c6
ms.service: stream-analytics
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.author: jeffstok
ms.openlocfilehash: a43c503d709609ba61e9932822d30bc2208906ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a><span data-ttu-id="11e78-104">Architektúra hivatkozni: a Microsoft Azure Stream Analytics valós idejű Eseményfeldolgozási</span><span class="sxs-lookup"><span data-stu-id="11e78-104">Reference architecture: Real-time event processing with Microsoft Azure Stream Analytics</span></span>
<span data-ttu-id="11e78-105">hello referencia-architektúrában az Azure Stream Analytics valós idejű Eseményfeldolgozási tervezett tooprovide valós idejű platform telepítése a Microsoft Azure-szolgáltatás (PaaS) adatfolyam-feldolgozási megoldásként egy általános szerkezeti terve.</span><span class="sxs-lookup"><span data-stu-id="11e78-105">hello reference architecture for real-time event processing with Azure Stream Analytics is intended tooprovide a generic blueprint for deploying a real-time platform as a service (PaaS) stream-processing solution with Microsoft Azure.</span></span>

## <a name="summary"></a><span data-ttu-id="11e78-106">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="11e78-106">Summary</span></span>
<span data-ttu-id="11e78-107">Hagyományosan elemzési megoldásokat alapulnak képességeit ETL (kinyerés, átalakítás, betöltés) és az adatraktározás terén, például a korábbi tooanalysis tárolt adatok esetén.</span><span class="sxs-lookup"><span data-stu-id="11e78-107">Traditionally, analytics solutions have been based on capabilities such as ETL (extract, transform, load) and data warehousing, where data is stored prior tooanalysis.</span></span> <span data-ttu-id="11e78-108">Változó követelményeknek megfelelően további gyorsan érkező adatokat, beleértve a létező modell toohello korlát leküldendő.</span><span class="sxs-lookup"><span data-stu-id="11e78-108">Changing requirements, including more rapidly arriving data, are pushing this existing model toohello limit.</span></span> <span data-ttu-id="11e78-109">hello képességét tooanalyze adatok áthelyezése adatfolyamok előzetes toostorage belül egy megoldást, és bár ez nem egy új képesség, hello megközelítés nem széles körben elfogadott összes iparági referenciaegyenesen között.</span><span class="sxs-lookup"><span data-stu-id="11e78-109">hello ability tooanalyze data within moving streams prior toostorage is one solution, and while it is not a new capability, hello approach has not been widely adopted across all industry verticals.</span></span> 

<span data-ttu-id="11e78-110">Microsoft Azure biztosít egy kiterjedt katalógust, amely támogathatja a másik megoldás forgatókönyvek és követelmények tömbje analytics technológiák.</span><span class="sxs-lookup"><span data-stu-id="11e78-110">Microsoft Azure provides an extensive catalog of analytics technologies that are capable of supporting an array of different solution scenarios and requirements.</span></span> <span data-ttu-id="11e78-111">Ha a mely Azure-szolgáltatások toodeploy végpont megoldás ajánlatok hello mélysége adott feladat lehet.</span><span class="sxs-lookup"><span data-stu-id="11e78-111">Selecting which Azure services toodeploy for an end-to-end solution can be a challenge given hello breadth of offerings.</span></span> <span data-ttu-id="11e78-112">A dokumentum tervezett toodescribe hello képességeket, és együttműködését az hello különböző Azure-egy esemény-adatfolyam-megoldást támogató szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="11e78-112">This paper is designed toodescribe hello capabilities and interoperation of hello various Azure services that support an event-streaming solution.</span></span> <span data-ttu-id="11e78-113">Ezen kívül néhány hello forgatókönyv, amelyben az ügyfelek is kihasználhatja az ilyen típusú megközelítés ismerteti.</span><span class="sxs-lookup"><span data-stu-id="11e78-113">It also explains some of hello scenarios in which customers can benefit from this type of approach.</span></span>

## <a name="contents"></a><span data-ttu-id="11e78-114">Tartalom</span><span class="sxs-lookup"><span data-stu-id="11e78-114">Contents</span></span>
* <span data-ttu-id="11e78-115">Vezetői összefoglaló</span><span class="sxs-lookup"><span data-stu-id="11e78-115">Executive Summary</span></span>
* <span data-ttu-id="11e78-116">Bevezetés tooReal idejű elemzés</span><span class="sxs-lookup"><span data-stu-id="11e78-116">Introduction tooReal-Time Analytics</span></span>
* <span data-ttu-id="11e78-117">A valós idejű adatok Azure-ban Értékajánlatához</span><span class="sxs-lookup"><span data-stu-id="11e78-117">Value Proposition of Real-Time Data in Azure</span></span>
* <span data-ttu-id="11e78-118">Valós idejű elemzések gyakori helyzetek</span><span class="sxs-lookup"><span data-stu-id="11e78-118">Common Scenarios for Real-Time Analytics</span></span>
* <span data-ttu-id="11e78-119">Architektúrája és összetevői</span><span class="sxs-lookup"><span data-stu-id="11e78-119">Architecture and Components</span></span>
  * <span data-ttu-id="11e78-120">Adatforrások</span><span class="sxs-lookup"><span data-stu-id="11e78-120">Data Sources</span></span>
  * <span data-ttu-id="11e78-121">Adatintegráció réteg</span><span class="sxs-lookup"><span data-stu-id="11e78-121">Data-Integration Layer</span></span>
  * <span data-ttu-id="11e78-122">Valós idejű elemzési réteg</span><span class="sxs-lookup"><span data-stu-id="11e78-122">Real-time Analytics Layer</span></span>
  * <span data-ttu-id="11e78-123">Adatok tárolási rétegből</span><span class="sxs-lookup"><span data-stu-id="11e78-123">Data Storage Layer</span></span>
  * <span data-ttu-id="11e78-124">Bemutató / fogyasztás réteg</span><span class="sxs-lookup"><span data-stu-id="11e78-124">Presentation / Consumption Layer</span></span>
* <span data-ttu-id="11e78-125">Összegzés</span><span class="sxs-lookup"><span data-stu-id="11e78-125">Conclusion</span></span>

<span data-ttu-id="11e78-126">**Szerző:** Insights adatközpont Charles Feddersen, megoldásokért felelős mérnök kiváló, Microsoft Corporation</span><span class="sxs-lookup"><span data-stu-id="11e78-126">**Author:** Charles Feddersen, Solution Architect, Data Insights Center of Excellence, Microsoft Corporation</span></span>

<span data-ttu-id="11e78-127">**Közzétett:** 2015. januári</span><span class="sxs-lookup"><span data-stu-id="11e78-127">**Published:** January 2015</span></span>

<span data-ttu-id="11e78-128">**Változat:** 1.0</span><span class="sxs-lookup"><span data-stu-id="11e78-128">**Revision:** 1.0</span></span>

<span data-ttu-id="11e78-129">**Letöltés:** [valós idejű, a Microsoft Azure Stream Analytics Eseményfeldolgozási](http://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)</span><span class="sxs-lookup"><span data-stu-id="11e78-129">**Download:** [Real-Time Event Processing with Microsoft Azure Stream Analytics](http://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)</span></span>

## <a name="get-help"></a><span data-ttu-id="11e78-130">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="11e78-130">Get help</span></span>
<span data-ttu-id="11e78-131">További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="11e78-131">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="11e78-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="11e78-132">Next steps</span></span>
* [<span data-ttu-id="11e78-133">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="11e78-133">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="11e78-134">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="11e78-134">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="11e78-135">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="11e78-135">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="11e78-136">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="11e78-136">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="11e78-137">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="11e78-137">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

