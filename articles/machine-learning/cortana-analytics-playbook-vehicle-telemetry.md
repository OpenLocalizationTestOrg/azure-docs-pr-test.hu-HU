---
title: "aaaPredict vehicle állapotát és ki irányítja az Azure - a szokásokat |} Microsoft Docs"
description: "Használja a Cortana Intelligence toogain valós idejű és prediktív elemzések hello lehetőségeit a vehicle állapotát, és ki irányítja az szokásokat."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 09fad60b-2f48-488b-8a7e-47d1f969ec6f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 54cc890ff39493bc040bb809721388349665720f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a><span data-ttu-id="9feca-103">A járműtelemetria-elemzési megoldás forgatókönyve</span><span class="sxs-lookup"><span data-stu-id="9feca-103">Vehicle telemetry analytics solution playbook</span></span>
<span data-ttu-id="9feca-104">Ez **menü** Ez a forgatókönyv toohello fejezetek hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="9feca-104">This **menu** links toohello chapters in this playbook.</span></span> 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a><span data-ttu-id="9feca-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="9feca-105">Overview</span></span>
<span data-ttu-id="9feca-106">Super számítógépek ki hello laborból került át, és most leparkolni vannak a garázs!</span><span class="sxs-lookup"><span data-stu-id="9feca-106">Super computers have moved out of hello lab and are now parked in our garage!</span></span> <span data-ttu-id="9feca-107">Ezek a legmodernebb autók érzékelők hello képességét tootrack jogosultságot ad a számtalan tartalmaz, és több millió esemény másodpercenként figyelése.</span><span class="sxs-lookup"><span data-stu-id="9feca-107">These cutting-edge automobiles contain a myriad of sensors, giving them hello ability tootrack and monitor millions of events every second.</span></span> <span data-ttu-id="9feca-108">Elvárjuk, hogy által 2020, ezek autók része lesz történt csatlakoztatott toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="9feca-108">We expect that by 2020, most of these cars will have been connected toohello Internet.</span></span> <span data-ttu-id="9feca-109">Képzelje el azokat a számos olyan adatok tooprovide nagyobb biztonsági, megbízhatósági és jobban vezetői élmény koppintva!</span><span class="sxs-lookup"><span data-stu-id="9feca-109">Imagine tapping into this wealth of data tooprovide greater safety, reliability and a better driving experience!</span></span> <span data-ttu-id="9feca-110">A Microsoft tett ezt a helyzetet a Cortana Intelligence megváltozz.</span><span class="sxs-lookup"><span data-stu-id="9feca-110">Microsoft has made this dream a reality with Cortana Intelligence.</span></span>

<span data-ttu-id="9feca-111">A Microsoft a Cortana Intelligence egy teljes körűen felügyelt big Data típusú adatok, és az adatok, amely lehetővé teszi tootransform analytics suite speciális intelligens működésbe.</span><span class="sxs-lookup"><span data-stu-id="9feca-111">Microsoft’s Cortana Intelligence is a fully managed big data and advanced analytics suite that enables you tootransform your data into intelligent action.</span></span> <span data-ttu-id="9feca-112">Azt szeretnénk, ha toointroduce Cortana Intelligence Vehicle Telemetriai Analytics Megoldássablonban toohello.</span><span class="sxs-lookup"><span data-stu-id="9feca-112">We want toointroduce you toohello Cortana Intelligence Vehicle Telemetry Analytics Solution Template.</span></span> <span data-ttu-id="9feca-113">Ez a megoldás bemutatja, hogyan car kereskedők autó gyártók és biztosítási vállalatok szolgáltatásai segítségével végre hello Cortana Intelligence toogain valós idejű és prediktív elemzések a vehicle állapotát, és ki irányítja a szokásokat.</span><span class="sxs-lookup"><span data-stu-id="9feca-113">This solution demonstrates how car dealerships, automobile manufacturers, and insurance companies can use hello capabilities of Cortana Intelligence toogain real-time and predictive insights on vehicle health and driving habits.</span></span> 

<span data-ttu-id="9feca-114">hello megoldás valósul meg a [lambda architektúra mintát](https://en.wikipedia.org/wiki/Lambda_architecture) hello hello Cortana Intelligence Platform for felhasználása megjelenítő valós idejű és kötegelt feldolgozást.</span><span class="sxs-lookup"><span data-stu-id="9feca-114">hello solution is implemented as a [lambda architecture pattern](https://en.wikipedia.org/wiki/Lambda_architecture) showing hello full potential of hello Cortana Intelligence platform for real-time and batch processing.</span></span> <span data-ttu-id="9feca-115">hello megoldást:</span><span class="sxs-lookup"><span data-stu-id="9feca-115">hello solution:</span></span> 

* <span data-ttu-id="9feca-116">a Vehicle telematika szimulátor biztosít</span><span class="sxs-lookup"><span data-stu-id="9feca-116">provides a Vehicle Telematics simulator</span></span>
* <span data-ttu-id="9feca-117">kihasználja a választásával dolgozhat fel szimulált vehicle telemetriai események több millió az Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="9feca-117">leverages Event Hubs for ingesting millions of simulated vehicle telemetry events into Azure</span></span> 
* <span data-ttu-id="9feca-118">használja a Stream Analytics toogain valós idejű elemzése a vehicle állapotát</span><span class="sxs-lookup"><span data-stu-id="9feca-118">uses Stream Analytics toogain real-time insights on vehicle health</span></span>
* <span data-ttu-id="9feca-119">a hosszú távú tároló gazdagabb kötegelt elemzésekben az tartja fenn a hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="9feca-119">persists hello data into long-term storage for richer batch analytics.</span></span> 
* <span data-ttu-id="9feca-120">kihasználja a Machine Learning közüli a valós idejű és feldolgozási toogain prediktív elemzések kötegelt.</span><span class="sxs-lookup"><span data-stu-id="9feca-120">takes advantage of Machine Learning for anomaly detection in real-time and batch processing toogain predictive insights.</span></span>
* <span data-ttu-id="9feca-121">kihasználja a HDInsight tootransform adatokat a méretezési és a Data Factory toohandle vezénylési, ütemezés, erőforrás-kezelés és hello kötegelt feldolgozási folyamatának figyelése</span><span class="sxs-lookup"><span data-stu-id="9feca-121">leverages HDInsight tootransform data at scale and Data Factory toohandle orchestration, scheduling, resource management, and monitoring of hello batch processing pipeline</span></span> 
* <span data-ttu-id="9feca-122">Ez a megoldás részletes irányítópult ad valós idejű adatok és a prediktív elemzés képi megjelenítések Power BI használatával</span><span class="sxs-lookup"><span data-stu-id="9feca-122">gives this solution a rich dashboard for real-time data and predictive analytics visualizations using Power BI</span></span>

## <a name="architecture"></a><span data-ttu-id="9feca-123">Architektúra</span><span class="sxs-lookup"><span data-stu-id="9feca-123">Architecture</span></span>
<span data-ttu-id="9feca-124">![Megoldás architektúrája](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*1. ábra – Vehicle Telemetriai Analytics megoldás architektúrája*</span><span class="sxs-lookup"><span data-stu-id="9feca-124">![Solution architecture diagram](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*Figure 1 – Vehicle Telemetry Analytics Solution Architecture*</span></span>

<span data-ttu-id="9feca-125">Ez a megoldás tartalmaz hello következő **Cortana Intelligence összetevők** és azok befejezési tooend integrációs bővíthető:</span><span class="sxs-lookup"><span data-stu-id="9feca-125">This solution includes hello following **Cortana Intelligence components** and showcases their end tooend integration:</span></span>

* <span data-ttu-id="9feca-126">**Az Event Hubs** választásával dolgozhat fel a vehicle telemetriai események több millió az Azure.</span><span class="sxs-lookup"><span data-stu-id="9feca-126">**Event Hubs** for ingesting millions of vehicle telemetry events into Azure.</span></span>
* <span data-ttu-id="9feca-127">**Stream Analytics** való vehicle egészségügyi valós idejű elemzése és továbbra is fennáll, hogy az adatok a hosszú távú tároló gazdagabb kötegelt elemzéséhez.</span><span class="sxs-lookup"><span data-stu-id="9feca-127">**Stream Analytics** for gaining real-time insights on vehicle health and persists that data into long-term storage for richer batch analytics.</span></span>
* <span data-ttu-id="9feca-128">**Gépi tanulás** közüli valós idejű és kötegfeldolgozási toogain prediktív elemzések.</span><span class="sxs-lookup"><span data-stu-id="9feca-128">**Machine Learning** for anomaly detection in real-time and batch processing toogain predictive insights.</span></span>
* <span data-ttu-id="9feca-129">**HDInsight** léptékű kihasználhatók tootransform adat</span><span class="sxs-lookup"><span data-stu-id="9feca-129">**HDInsight** is leveraged tootransform data at scale</span></span>
* <span data-ttu-id="9feca-130">**Adat-előállító** kezeli az orchestration, erőforrás-kezelés ütemezése és hello kötegelt folyamat figyelését.</span><span class="sxs-lookup"><span data-stu-id="9feca-130">**Data Factory** handles orchestration, scheduling, resource management and monitoring of hello batch processing pipeline.</span></span>
* <span data-ttu-id="9feca-131">**A Power BI** Ez a megoldás részletes irányítópult ad valós idejű adatok és a prediktív elemzés képi megjelenítések.</span><span class="sxs-lookup"><span data-stu-id="9feca-131">**Power BI** gives this solution a rich dashboard for real-time data and predictive analytics visualizations.</span></span>

<span data-ttu-id="9feca-132">Ez a megoldás két különböző hozzáfér **adatforrások**:</span><span class="sxs-lookup"><span data-stu-id="9feca-132">This solution accesses two different **data sources**:</span></span> 

* <span data-ttu-id="9feca-133">**Szimulált vehicle jelek és diagnosztikai**: vehicle telematika szimulátor bocsát ki a diagnosztikai adatokat, és azt jelzi, hogy hello vehicle és befolyásoló tényezők minta egy időben hello toohello állapotának felel meg.</span><span class="sxs-lookup"><span data-stu-id="9feca-133">**Simulated vehicle signals and diagnostics**: A vehicle telematics simulator emits diagnostic information and signals that correspond toohello state of hello vehicle and hello driving pattern at a given point in time.</span></span> 
* <span data-ttu-id="9feca-134">**Vehicle katalógus**: a VIN toomodel tartalmazó referencia-adatkészletnek.</span><span class="sxs-lookup"><span data-stu-id="9feca-134">**Vehicle catalog**: A reference dataset containing a VIN toomodel mapping.</span></span>

