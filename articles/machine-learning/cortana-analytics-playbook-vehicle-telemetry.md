---
title: "Előre jelezni a vehicle állapotát, és ki irányítja az szokásait - Azure |} Microsoft Docs"
description: "A Cortana Intelligence szolgáltatásai segítségével a vehicle állapotát, és ki irányítja a valós idejű és prediktív dcu szokásokat."
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
ms.openlocfilehash: d202d314c61416cf306f760f93e0a4a88a1ab42b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a><span data-ttu-id="645ff-103">A járműtelemetria-elemzési megoldás forgatókönyve</span><span class="sxs-lookup"><span data-stu-id="645ff-103">Vehicle telemetry analytics solution playbook</span></span>
<span data-ttu-id="645ff-104">Ez **menü** Ez a forgatókönyv a fejezetek mutató hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="645ff-104">This **menu** links to the chapters in this playbook.</span></span> 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a><span data-ttu-id="645ff-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="645ff-105">Overview</span></span>
<span data-ttu-id="645ff-106">Super számítógépek kívül a labor került át, és most leparkolni vannak a garázs!</span><span class="sxs-lookup"><span data-stu-id="645ff-106">Super computers have moved out of the lab and are now parked in our garage!</span></span> <span data-ttu-id="645ff-107">Ezek a legmodernebb autók érzékelők, nyomon követheti és felügyelheti a több millió esemény másodpercenként teszi lehetővé a számtalan tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="645ff-107">These cutting-edge automobiles contain a myriad of sensors, giving them the ability to track and monitor millions of events every second.</span></span> <span data-ttu-id="645ff-108">Elvárjuk, hogy által 2020, ezek autók többsége fogja csatlakoztatni az internethez.</span><span class="sxs-lookup"><span data-stu-id="645ff-108">We expect that by 2020, most of these cars will have been connected to the Internet.</span></span> <span data-ttu-id="645ff-109">Képzelje el azokat a számos olyan adatokat annak érdekében, nagyobb biztonsági, megbízhatósági és jobban vezetői élmény koppintva!</span><span class="sxs-lookup"><span data-stu-id="645ff-109">Imagine tapping into this wealth of data to provide greater safety, reliability and a better driving experience!</span></span> <span data-ttu-id="645ff-110">A Microsoft tett ezt a helyzetet a Cortana Intelligence megváltozz.</span><span class="sxs-lookup"><span data-stu-id="645ff-110">Microsoft has made this dream a reality with Cortana Intelligence.</span></span>

<span data-ttu-id="645ff-111">A Microsoft a Cortana Intelligence egy teljes körűen felügyelt big Data típusú adatok és a speciális analytics suite, amely lehetővé teszi az adatok átalakítása intelligens művelet.</span><span class="sxs-lookup"><span data-stu-id="645ff-111">Microsoft’s Cortana Intelligence is a fully managed big data and advanced analytics suite that enables you to transform your data into intelligent action.</span></span> <span data-ttu-id="645ff-112">Azt szeretnénk, bemutatja a Cortana Intelligence Vehicle Telemetriai Analytics megoldás sablont.</span><span class="sxs-lookup"><span data-stu-id="645ff-112">We want to introduce you to the Cortana Intelligence Vehicle Telemetry Analytics Solution Template.</span></span> <span data-ttu-id="645ff-113">Ez a megoldás bemutatja, hogyan car kereskedők autó gyártók és biztosítási vállalatok segítségével a Cortana Intelligence képességeit valós idejű és prediktív elemzések a vehicle állapotát, és ki irányítja a szokásokat.</span><span class="sxs-lookup"><span data-stu-id="645ff-113">This solution demonstrates how car dealerships, automobile manufacturers, and insurance companies can use the capabilities of Cortana Intelligence to gain real-time and predictive insights on vehicle health and driving habits.</span></span> 

<span data-ttu-id="645ff-114">A megoldás bevezetése egy [lambda architektúra mintát](https://en.wikipedia.org/wiki/Lambda_architecture) megjelenítése a teljes lehetséges a Cortana Intelligence Platform for valós idejű és kötegelt feldolgozást.</span><span class="sxs-lookup"><span data-stu-id="645ff-114">The solution is implemented as a [lambda architecture pattern](https://en.wikipedia.org/wiki/Lambda_architecture) showing the full potential of the Cortana Intelligence platform for real-time and batch processing.</span></span> <span data-ttu-id="645ff-115">A megoldás:</span><span class="sxs-lookup"><span data-stu-id="645ff-115">The solution:</span></span> 

* <span data-ttu-id="645ff-116">a Vehicle telematika szimulátor biztosít</span><span class="sxs-lookup"><span data-stu-id="645ff-116">provides a Vehicle Telematics simulator</span></span>
* <span data-ttu-id="645ff-117">kihasználja a választásával dolgozhat fel szimulált vehicle telemetriai események több millió az Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="645ff-117">leverages Event Hubs for ingesting millions of simulated vehicle telemetry events into Azure</span></span> 
* <span data-ttu-id="645ff-118">használja a Stream Analytics vehicle egészségügyi valós idejű dcu</span><span class="sxs-lookup"><span data-stu-id="645ff-118">uses Stream Analytics to gain real-time insights on vehicle health</span></span>
* <span data-ttu-id="645ff-119">továbbra is fennáll az adatokat a hosszú távú tároló gazdagabb kötegelt elemzéséhez.</span><span class="sxs-lookup"><span data-stu-id="645ff-119">persists the data into long-term storage for richer batch analytics.</span></span> 
* <span data-ttu-id="645ff-120">kihasználja a Machine Learning közüli a valós idejű és a batch-prediktív dcu feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="645ff-120">takes advantage of Machine Learning for anomaly detection in real-time and batch processing to gain predictive insights.</span></span>
* <span data-ttu-id="645ff-121">kihasználja a HDInsight átalakítására léptékű adatok és a Data Factory vezénylési, ütemezés, erőforrás-kezelés és figyelés, a köteges feldolgozás folyamatának kezelése</span><span class="sxs-lookup"><span data-stu-id="645ff-121">leverages HDInsight to transform data at scale and Data Factory to handle orchestration, scheduling, resource management, and monitoring of the batch processing pipeline</span></span> 
* <span data-ttu-id="645ff-122">Ez a megoldás részletes irányítópult ad valós idejű adatok és a prediktív elemzés képi megjelenítések Power BI használatával</span><span class="sxs-lookup"><span data-stu-id="645ff-122">gives this solution a rich dashboard for real-time data and predictive analytics visualizations using Power BI</span></span>

## <a name="architecture"></a><span data-ttu-id="645ff-123">Architektúra</span><span class="sxs-lookup"><span data-stu-id="645ff-123">Architecture</span></span>
<span data-ttu-id="645ff-124">![Megoldás architektúrája](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*1. ábra – Vehicle Telemetriai Analytics megoldás architektúrája*</span><span class="sxs-lookup"><span data-stu-id="645ff-124">![Solution architecture diagram](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*Figure 1 – Vehicle Telemetry Analytics Solution Architecture*</span></span>

<span data-ttu-id="645ff-125">Ez a megoldás tartalmaz a következő **Cortana Intelligence összetevők** és a végpontok közötti integráció bővíthető:</span><span class="sxs-lookup"><span data-stu-id="645ff-125">This solution includes the following **Cortana Intelligence components** and showcases their end to end integration:</span></span>

* <span data-ttu-id="645ff-126">**Az Event Hubs** választásával dolgozhat fel a vehicle telemetriai események több millió az Azure.</span><span class="sxs-lookup"><span data-stu-id="645ff-126">**Event Hubs** for ingesting millions of vehicle telemetry events into Azure.</span></span>
* <span data-ttu-id="645ff-127">**Stream Analytics** való vehicle egészségügyi valós idejű elemzése és továbbra is fennáll, hogy az adatok a hosszú távú tároló gazdagabb kötegelt elemzéséhez.</span><span class="sxs-lookup"><span data-stu-id="645ff-127">**Stream Analytics** for gaining real-time insights on vehicle health and persists that data into long-term storage for richer batch analytics.</span></span>
* <span data-ttu-id="645ff-128">**Gépi tanulás** közüli valós idejű és prediktív dcu kötegfeldolgozási.</span><span class="sxs-lookup"><span data-stu-id="645ff-128">**Machine Learning** for anomaly detection in real-time and batch processing to gain predictive insights.</span></span>
* <span data-ttu-id="645ff-129">**HDInsight** rendszer elkészítéséhez használja léptékű adatok átalakítása</span><span class="sxs-lookup"><span data-stu-id="645ff-129">**HDInsight** is leveraged to transform data at scale</span></span>
* <span data-ttu-id="645ff-130">**Adat-előállító** vezénylési, ütemezés, erőforrás-kezelés és figyelés, a köteges feldolgozás folyamatának kezeli.</span><span class="sxs-lookup"><span data-stu-id="645ff-130">**Data Factory** handles orchestration, scheduling, resource management and monitoring of the batch processing pipeline.</span></span>
* <span data-ttu-id="645ff-131">**A Power BI** Ez a megoldás részletes irányítópult ad valós idejű adatok és a prediktív elemzés képi megjelenítések.</span><span class="sxs-lookup"><span data-stu-id="645ff-131">**Power BI** gives this solution a rich dashboard for real-time data and predictive analytics visualizations.</span></span>

<span data-ttu-id="645ff-132">Ez a megoldás két különböző hozzáfér **adatforrások**:</span><span class="sxs-lookup"><span data-stu-id="645ff-132">This solution accesses two different **data sources**:</span></span> 

* <span data-ttu-id="645ff-133">**Szimulált vehicle jelek és diagnosztikai**: bocsát ki diagnosztikai adatokat, és azt jelzi, hogy megfelelnek a vehicle és adott vezetői mintát állapotának vehicle telematika szimulátor időben.</span><span class="sxs-lookup"><span data-stu-id="645ff-133">**Simulated vehicle signals and diagnostics**: A vehicle telematics simulator emits diagnostic information and signals that correspond to the state of the vehicle and the driving pattern at a given point in time.</span></span> 
* <span data-ttu-id="645ff-134">**Vehicle katalógus**: az modell-lel egy VIN tartalmazó referencia-adatkészletnek.</span><span class="sxs-lookup"><span data-stu-id="645ff-134">**Vehicle catalog**: A reference dataset containing a VIN to model mapping.</span></span>

