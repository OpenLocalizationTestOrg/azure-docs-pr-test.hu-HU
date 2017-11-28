---
title: "aaaDeep alaposabban előre jelezni a vehicle állapotát, és ki irányítja az szokásait - Azure |} Microsoft Docs"
description: "Használja a Cortana Intelligence toogain valós idejű és prediktív elemzések hello lehetőségeit a vehicle állapotát, és ki irányítja az szokásokat."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d8866fa6-aba6-40e5-b3b3-33057393c1a8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: ba1448a5081762292561f904d9ec54617c9a5330
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-hello-solution"></a><span data-ttu-id="55229-103">Vehicle telemetriai analytics megoldás forgatókönyv: hello megoldásba történő részletes bemutatója</span><span class="sxs-lookup"><span data-stu-id="55229-103">Vehicle telemetry analytics solution playbook: deep dive into hello solution</span></span>
<span data-ttu-id="55229-104">Ez **menü** hivatkozik, ez a forgatókönyv toohello szakaszait:</span><span class="sxs-lookup"><span data-stu-id="55229-104">This **menu** links toohello sections of this playbook:</span></span> 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

<span data-ttu-id="55229-105">Ez a szakasz részletezi mindegyik hello szakaszból hello megoldásarchitektúra utasításokat és testreszabási mutatók írja le.</span><span class="sxs-lookup"><span data-stu-id="55229-105">This section drills down into each of hello stages depicted in hello Solution Architecture with instructions and pointers for customization.</span></span> 

## <a name="data-sources"></a><span data-ttu-id="55229-106">Adatforrások</span><span class="sxs-lookup"><span data-stu-id="55229-106">Data Sources</span></span>
<span data-ttu-id="55229-107">hello megoldás két különböző adatforrásból használ:</span><span class="sxs-lookup"><span data-stu-id="55229-107">hello solution uses two different data sources:</span></span>

* <span data-ttu-id="55229-108">**Szimulált vehicle jelek és diagnosztikai adatkészlet** és</span><span class="sxs-lookup"><span data-stu-id="55229-108">**simulated vehicle signals and diagnostic dataset** and</span></span> 
* <span data-ttu-id="55229-109">**vehicle katalógus**</span><span class="sxs-lookup"><span data-stu-id="55229-109">**vehicle catalog**</span></span>

<span data-ttu-id="55229-110">A vehicle telematika szimulátor Ez a megoldás részét.</span><span class="sxs-lookup"><span data-stu-id="55229-110">A vehicle telematics simulator is included as part of this solution.</span></span> <span data-ttu-id="55229-111">Diagnosztikai adatok bocsát ki, és hello vehicle és a minta egy időben befolyásoló tényezők toohello megfelelő toohello állapotát jelzi.</span><span class="sxs-lookup"><span data-stu-id="55229-111">It emits diagnostic information and signals corresponding toohello state of hello vehicle and toohello driving pattern at a given point in time.</span></span> <span data-ttu-id="55229-112">Kattintson a [Vehicle telematika szimulátor](http://go.microsoft.com/fwlink/?LinkId=717075) toodownload hello **Vehicle telematika szimulátor Visual Studio megoldás** testreszabni a követelmények alapján.</span><span class="sxs-lookup"><span data-stu-id="55229-112">Click [Vehicle Telematics Simulator](http://go.microsoft.com/fwlink/?LinkId=717075) toodownload hello **Vehicle Telematics Simulator Visual Studio Solution** for customizations based on your requirements.</span></span> <span data-ttu-id="55229-113">hello vehicle katalógus tartalmazza a referencia-adatkészletnek a VIN toomodel leképezéssel.</span><span class="sxs-lookup"><span data-stu-id="55229-113">hello vehicle catalog contains a reference dataset with a VIN toomodel mapping.</span></span>

![Vehicle telematika szimulátor](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig1-vehicle-telematics-simulator.png)

<span data-ttu-id="55229-115">*1. ábra – Vehicle telematika szimulátor*</span><span class="sxs-lookup"><span data-stu-id="55229-115">*Figure 1 – Vehicle Telematics Simulator*</span></span>

<span data-ttu-id="55229-116">Ez a séma a következő hello tartalmazó JSON-formátumú dataset.</span><span class="sxs-lookup"><span data-stu-id="55229-116">This is a JSON-formatted dataset that contains hello following schema.</span></span>

| <span data-ttu-id="55229-117">Oszlop</span><span class="sxs-lookup"><span data-stu-id="55229-117">Column</span></span> | <span data-ttu-id="55229-118">Leírás</span><span class="sxs-lookup"><span data-stu-id="55229-118">Description</span></span> | <span data-ttu-id="55229-119">Értékek</span><span class="sxs-lookup"><span data-stu-id="55229-119">Values</span></span> |
| --- | --- | --- |
| <span data-ttu-id="55229-120">VIN</span><span class="sxs-lookup"><span data-stu-id="55229-120">VIN</span></span> |<span data-ttu-id="55229-121">Véletlenszerűen létrehozott Vehicle azonosító szám</span><span class="sxs-lookup"><span data-stu-id="55229-121">Randomly generated Vehicle Identification Number</span></span> |<span data-ttu-id="55229-122">10 000 véletlenszerűen létrehozott vehicle azonosító számok fő listájának származik.</span><span class="sxs-lookup"><span data-stu-id="55229-122">This is obtained from a master list of 10,000 randomly generated vehicle identification numbers.</span></span> |
| <span data-ttu-id="55229-123">Külső hőmérséklet</span><span class="sxs-lookup"><span data-stu-id="55229-123">Outside temperature</span></span> |<span data-ttu-id="55229-124">hello kívül hőmérséklet, ahol hello vehicle befolyásoló tényezők</span><span class="sxs-lookup"><span data-stu-id="55229-124">hello outside temperature where hello vehicle is driving</span></span> |<span data-ttu-id="55229-125">Véletlenszerűen létrehozott szám 0 – 100</span><span class="sxs-lookup"><span data-stu-id="55229-125">Randomly generated number from 0-100</span></span> |
| <span data-ttu-id="55229-126">Motor</span><span class="sxs-lookup"><span data-stu-id="55229-126">Engine temperature</span></span> |<span data-ttu-id="55229-127">hello motor hőmérséklete hello vehicle</span><span class="sxs-lookup"><span data-stu-id="55229-127">hello engine temperature of hello vehicle</span></span> |<span data-ttu-id="55229-128">Véletlenszerűen létrehozott száma 0-500</span><span class="sxs-lookup"><span data-stu-id="55229-128">Randomly generated number from 0-500</span></span> |
| <span data-ttu-id="55229-129">Gyorsaság</span><span class="sxs-lookup"><span data-stu-id="55229-129">Speed</span></span> |<span data-ttu-id="55229-130">hello motor sebessége, mely hello vehicle befolyásoló tényezők</span><span class="sxs-lookup"><span data-stu-id="55229-130">hello engine speed at which hello vehicle is driving</span></span> |<span data-ttu-id="55229-131">Véletlenszerűen létrehozott szám 0 – 100</span><span class="sxs-lookup"><span data-stu-id="55229-131">Randomly generated number from 0-100</span></span> |
| <span data-ttu-id="55229-132">Üzemanyag</span><span class="sxs-lookup"><span data-stu-id="55229-132">Fuel</span></span> |<span data-ttu-id="55229-133">hello vehicle hello üzemanyag szintje</span><span class="sxs-lookup"><span data-stu-id="55229-133">hello fuel level of hello vehicle</span></span> |<span data-ttu-id="55229-134">Véletlenszerűen létrehozott szám 0 – 100 (üzemanyag százalékos értéke jelzi)</span><span class="sxs-lookup"><span data-stu-id="55229-134">Randomly generated number from 0-100 (indicates fuel level percentage)</span></span> |
| <span data-ttu-id="55229-135">EngineOil</span><span class="sxs-lookup"><span data-stu-id="55229-135">EngineOil</span></span> |<span data-ttu-id="55229-136">hello motor olaj szintű hello vehicle</span><span class="sxs-lookup"><span data-stu-id="55229-136">hello engine oil level of hello vehicle</span></span> |<span data-ttu-id="55229-137">Véletlenszerűen létrehozott szám 0 – 100 (motor olaj százalékos értéke jelzi)</span><span class="sxs-lookup"><span data-stu-id="55229-137">Randomly generated number from 0-100 (indicates engine oil level percentage)</span></span> |
| <span data-ttu-id="55229-138">Kulcsszava nyomás</span><span class="sxs-lookup"><span data-stu-id="55229-138">Tire pressure</span></span> |<span data-ttu-id="55229-139">hello kulcsszava nyomás hello vehicle</span><span class="sxs-lookup"><span data-stu-id="55229-139">hello tire pressure of hello vehicle</span></span> |<span data-ttu-id="55229-140">Véletlenszerűen előállított száma 0-50 (kulcsszava nyomás százalékos értéke jelzi)</span><span class="sxs-lookup"><span data-stu-id="55229-140">Randomly generated number from 0-50 (indicates tire pressure level percentage)</span></span> |
| <span data-ttu-id="55229-141">Kilométer</span><span class="sxs-lookup"><span data-stu-id="55229-141">Odometer</span></span> |<span data-ttu-id="55229-142">hello vehicle hello kilométer olvasása</span><span class="sxs-lookup"><span data-stu-id="55229-142">hello odometer reading of hello vehicle</span></span> |<span data-ttu-id="55229-143">Véletlenszerűen létrehozott szám 0-200000</span><span class="sxs-lookup"><span data-stu-id="55229-143">Randomly generated number from 0-200000</span></span> |
| <span data-ttu-id="55229-144">Accelerator_pedal_position</span><span class="sxs-lookup"><span data-stu-id="55229-144">Accelerator_pedal_position</span></span> |<span data-ttu-id="55229-145">hello gyorsító tartásához lehetőleg keveset pozíciója hello vehicle</span><span class="sxs-lookup"><span data-stu-id="55229-145">hello accelerator pedal position of hello vehicle</span></span> |<span data-ttu-id="55229-146">Véletlenszerűen létrehozott szám 0 – 100 (gyorsító százalékos értéke jelzi)</span><span class="sxs-lookup"><span data-stu-id="55229-146">Randomly generated number from 0-100 (indicates accelerator level percentage)</span></span> |
| <span data-ttu-id="55229-147">Parking_brake_status</span><span class="sxs-lookup"><span data-stu-id="55229-147">Parking_brake_status</span></span> |<span data-ttu-id="55229-148">Azt jelzi, hogy hello vehicle kell itt tartózkodnia vagy sem</span><span class="sxs-lookup"><span data-stu-id="55229-148">Indicates whether hello vehicle is parked or not</span></span> |<span data-ttu-id="55229-149">IGAZ vagy hamis</span><span class="sxs-lookup"><span data-stu-id="55229-149">True or False</span></span> |
| <span data-ttu-id="55229-150">Headlamp_status</span><span class="sxs-lookup"><span data-stu-id="55229-150">Headlamp_status</span></span> |<span data-ttu-id="55229-151">Azt jelzi, ahol hello fényszóró megtalálható-e</span><span class="sxs-lookup"><span data-stu-id="55229-151">Indicates where hello headlamp is on or not</span></span> |<span data-ttu-id="55229-152">IGAZ vagy hamis</span><span class="sxs-lookup"><span data-stu-id="55229-152">True or False</span></span> |
| <span data-ttu-id="55229-153">Brake_pedal_status</span><span class="sxs-lookup"><span data-stu-id="55229-153">Brake_pedal_status</span></span> |<span data-ttu-id="55229-154">Azt jelzi, hogy hello fékpedál nélkül, vagy sem</span><span class="sxs-lookup"><span data-stu-id="55229-154">Indicates whether hello brake pedal is pressed or not</span></span> |<span data-ttu-id="55229-155">IGAZ vagy hamis</span><span class="sxs-lookup"><span data-stu-id="55229-155">True or False</span></span> |
| <span data-ttu-id="55229-156">Transmission_gear_position</span><span class="sxs-lookup"><span data-stu-id="55229-156">Transmission_gear_position</span></span> |<span data-ttu-id="55229-157">hello átviteli fogaskerék hello vehicle helyzete</span><span class="sxs-lookup"><span data-stu-id="55229-157">hello transmission gear position of hello vehicle</span></span> |<span data-ttu-id="55229-158">Állapotok: először, a második, harmadik, negyedik, ötödik, hatodik, hetedik, nyolcadik</span><span class="sxs-lookup"><span data-stu-id="55229-158">States: first, second, third, fourth, fifth, sixth, seventh, eighth</span></span> |
| <span data-ttu-id="55229-159">Ignition_status</span><span class="sxs-lookup"><span data-stu-id="55229-159">Ignition_status</span></span> |<span data-ttu-id="55229-160">Azt jelzi, hogy hello vehicle fut vagy leállt</span><span class="sxs-lookup"><span data-stu-id="55229-160">Indicates whether hello vehicle is running or stopped</span></span> |<span data-ttu-id="55229-161">IGAZ vagy hamis</span><span class="sxs-lookup"><span data-stu-id="55229-161">True or False</span></span> |
| <span data-ttu-id="55229-162">Windshield_wiper_status</span><span class="sxs-lookup"><span data-stu-id="55229-162">Windshield_wiper_status</span></span> |<span data-ttu-id="55229-163">Azt jelzi, hogy az hello szélvédőkeret ablaktörlő be van kapcsolva vagy nem</span><span class="sxs-lookup"><span data-stu-id="55229-163">Indicates whether hello windshield wiper is turned or not</span></span> |<span data-ttu-id="55229-164">IGAZ vagy hamis</span><span class="sxs-lookup"><span data-stu-id="55229-164">True or False</span></span> |
| <span data-ttu-id="55229-165">ABS</span><span class="sxs-lookup"><span data-stu-id="55229-165">ABS</span></span> |<span data-ttu-id="55229-166">Azt jelzi, hogy ABS részt vevő vagy sem</span><span class="sxs-lookup"><span data-stu-id="55229-166">Indicates whether ABS is engaged or not</span></span> |<span data-ttu-id="55229-167">IGAZ vagy hamis</span><span class="sxs-lookup"><span data-stu-id="55229-167">True or False</span></span> |
| <span data-ttu-id="55229-168">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="55229-168">Timestamp</span></span> |<span data-ttu-id="55229-169">hello időbélyeg hello adatpont létrehozásakor</span><span class="sxs-lookup"><span data-stu-id="55229-169">hello timestamp when hello data point is created</span></span> |<span data-ttu-id="55229-170">Dátum</span><span class="sxs-lookup"><span data-stu-id="55229-170">Date</span></span> |
| <span data-ttu-id="55229-171">Város</span><span class="sxs-lookup"><span data-stu-id="55229-171">City</span></span> |<span data-ttu-id="55229-172">hello vehicle hello helye</span><span class="sxs-lookup"><span data-stu-id="55229-172">hello location of hello vehicle</span></span> |<span data-ttu-id="55229-173">Ebben a megoldásban 4 városokat: Bellevue, Redmond, Sammamish, Seattle</span><span class="sxs-lookup"><span data-stu-id="55229-173">4 cities in this solution: Bellevue, Redmond, Sammamish, Seattle</span></span> |

<span data-ttu-id="55229-174">hello vehicle modell referencia-adatkészletnek VIN toohello modell leképezést tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="55229-174">hello vehicle model reference dataset contains VIN toohello model mapping.</span></span> 

| <span data-ttu-id="55229-175">VIN</span><span class="sxs-lookup"><span data-stu-id="55229-175">VIN</span></span> | <span data-ttu-id="55229-176">Modell</span><span class="sxs-lookup"><span data-stu-id="55229-176">Model</span></span> |
| --- | --- |
| <span data-ttu-id="55229-177">FHL3O1SA4IEHB4WU1</span><span class="sxs-lookup"><span data-stu-id="55229-177">FHL3O1SA4IEHB4WU1</span></span> |<span data-ttu-id="55229-178">Sedan</span><span class="sxs-lookup"><span data-stu-id="55229-178">Sedan</span></span> |
| <span data-ttu-id="55229-179">8J0U8XCPRGW4Z3NQE</span><span class="sxs-lookup"><span data-stu-id="55229-179">8J0U8XCPRGW4Z3NQE</span></span> |<span data-ttu-id="55229-180">Hibrid</span><span class="sxs-lookup"><span data-stu-id="55229-180">Hybrid</span></span> |
| <span data-ttu-id="55229-181">WORG68Z2PLTNZDBI7</span><span class="sxs-lookup"><span data-stu-id="55229-181">WORG68Z2PLTNZDBI7</span></span> |<span data-ttu-id="55229-182">Családbiztonsági limuzin</span><span class="sxs-lookup"><span data-stu-id="55229-182">Family Saloon</span></span> |
| <span data-ttu-id="55229-183">JTHMYHQTEPP4WBMRN</span><span class="sxs-lookup"><span data-stu-id="55229-183">JTHMYHQTEPP4WBMRN</span></span> |<span data-ttu-id="55229-184">Sedan</span><span class="sxs-lookup"><span data-stu-id="55229-184">Sedan</span></span> |
| <span data-ttu-id="55229-185">W9FTHG27LZN1YWO0Y</span><span class="sxs-lookup"><span data-stu-id="55229-185">W9FTHG27LZN1YWO0Y</span></span> |<span data-ttu-id="55229-186">Hibrid</span><span class="sxs-lookup"><span data-stu-id="55229-186">Hybrid</span></span> |
| <span data-ttu-id="55229-187">MHTP9N792PHK08WJM</span><span class="sxs-lookup"><span data-stu-id="55229-187">MHTP9N792PHK08WJM</span></span> |<span data-ttu-id="55229-188">Családbiztonsági limuzin</span><span class="sxs-lookup"><span data-stu-id="55229-188">Family Saloon</span></span> |
| <span data-ttu-id="55229-189">EI4QXI2AXVQQING4I</span><span class="sxs-lookup"><span data-stu-id="55229-189">EI4QXI2AXVQQING4I</span></span> |<span data-ttu-id="55229-190">Sedan</span><span class="sxs-lookup"><span data-stu-id="55229-190">Sedan</span></span> |
| <span data-ttu-id="55229-191">5KKR2VB4WHQH97PF8</span><span class="sxs-lookup"><span data-stu-id="55229-191">5KKR2VB4WHQH97PF8</span></span> |<span data-ttu-id="55229-192">Hibrid</span><span class="sxs-lookup"><span data-stu-id="55229-192">Hybrid</span></span> |
| <span data-ttu-id="55229-193">W9NSZ423XZHAONYXB</span><span class="sxs-lookup"><span data-stu-id="55229-193">W9NSZ423XZHAONYXB</span></span> |<span data-ttu-id="55229-194">Családbiztonsági limuzin</span><span class="sxs-lookup"><span data-stu-id="55229-194">Family Saloon</span></span> |
| <span data-ttu-id="55229-195">26WJSGHX4MA5ROHNL</span><span class="sxs-lookup"><span data-stu-id="55229-195">26WJSGHX4MA5ROHNL</span></span> |<span data-ttu-id="55229-196">Váltható</span><span class="sxs-lookup"><span data-stu-id="55229-196">Convertible</span></span> |
| <span data-ttu-id="55229-197">GHLUB6ONKMOSI7E77</span><span class="sxs-lookup"><span data-stu-id="55229-197">GHLUB6ONKMOSI7E77</span></span> |<span data-ttu-id="55229-198">Állomás kocsi</span><span class="sxs-lookup"><span data-stu-id="55229-198">Station Wagon</span></span> |
| <span data-ttu-id="55229-199">9C2RHVRVLMEJDBXLP</span><span class="sxs-lookup"><span data-stu-id="55229-199">9C2RHVRVLMEJDBXLP</span></span> |<span data-ttu-id="55229-200">Kompakt autó</span><span class="sxs-lookup"><span data-stu-id="55229-200">Compact Car</span></span> |
| <span data-ttu-id="55229-201">BRNHVMZOUJ6EOCP32</span><span class="sxs-lookup"><span data-stu-id="55229-201">BRNHVMZOUJ6EOCP32</span></span> |<span data-ttu-id="55229-202">Kis SUV</span><span class="sxs-lookup"><span data-stu-id="55229-202">Small SUV</span></span> |
| <span data-ttu-id="55229-203">VCYVW0WUZNBTM594J</span><span class="sxs-lookup"><span data-stu-id="55229-203">VCYVW0WUZNBTM594J</span></span> |<span data-ttu-id="55229-204">Sport autó</span><span class="sxs-lookup"><span data-stu-id="55229-204">Sports Car</span></span> |
| <span data-ttu-id="55229-205">HNVCE6YFZSA5M82NY</span><span class="sxs-lookup"><span data-stu-id="55229-205">HNVCE6YFZSA5M82NY</span></span> |<span data-ttu-id="55229-206">Közepes SUV</span><span class="sxs-lookup"><span data-stu-id="55229-206">Medium SUV</span></span> |
| <span data-ttu-id="55229-207">4R30FOR7NUOBL05GJ</span><span class="sxs-lookup"><span data-stu-id="55229-207">4R30FOR7NUOBL05GJ</span></span> |<span data-ttu-id="55229-208">Állomás kocsi</span><span class="sxs-lookup"><span data-stu-id="55229-208">Station Wagon</span></span> |
| <span data-ttu-id="55229-209">WYNIIY42VKV6OQS1J</span><span class="sxs-lookup"><span data-stu-id="55229-209">WYNIIY42VKV6OQS1J</span></span> |<span data-ttu-id="55229-210">Nagy SUV</span><span class="sxs-lookup"><span data-stu-id="55229-210">Large SUV</span></span> |
| <span data-ttu-id="55229-211">8Y5QKG27QET1RBK7I</span><span class="sxs-lookup"><span data-stu-id="55229-211">8Y5QKG27QET1RBK7I</span></span> |<span data-ttu-id="55229-212">Nagy SUV</span><span class="sxs-lookup"><span data-stu-id="55229-212">Large SUV</span></span> |
| <span data-ttu-id="55229-213">DF6OX2WSRA6511BVG</span><span class="sxs-lookup"><span data-stu-id="55229-213">DF6OX2WSRA6511BVG</span></span> |<span data-ttu-id="55229-214">Coupe</span><span class="sxs-lookup"><span data-stu-id="55229-214">Coupe</span></span> |
| <span data-ttu-id="55229-215">Z2EOZWZBXAEW3E60T</span><span class="sxs-lookup"><span data-stu-id="55229-215">Z2EOZWZBXAEW3E60T</span></span> |<span data-ttu-id="55229-216">Sedan</span><span class="sxs-lookup"><span data-stu-id="55229-216">Sedan</span></span> |
| <span data-ttu-id="55229-217">M4TV6IEALD5QDS3IR</span><span class="sxs-lookup"><span data-stu-id="55229-217">M4TV6IEALD5QDS3IR</span></span> |<span data-ttu-id="55229-218">Hibrid</span><span class="sxs-lookup"><span data-stu-id="55229-218">Hybrid</span></span> |
| <span data-ttu-id="55229-219">VHRA1Y2TGTA84F00H</span><span class="sxs-lookup"><span data-stu-id="55229-219">VHRA1Y2TGTA84F00H</span></span> |<span data-ttu-id="55229-220">Családbiztonsági limuzin</span><span class="sxs-lookup"><span data-stu-id="55229-220">Family Saloon</span></span> |
| <span data-ttu-id="55229-221">R0JAUHT1L1R3BIKI0</span><span class="sxs-lookup"><span data-stu-id="55229-221">R0JAUHT1L1R3BIKI0</span></span> |<span data-ttu-id="55229-222">Sedan</span><span class="sxs-lookup"><span data-stu-id="55229-222">Sedan</span></span> |
| <span data-ttu-id="55229-223">9230C202Z60XX84AU</span><span class="sxs-lookup"><span data-stu-id="55229-223">9230C202Z60XX84AU</span></span> |<span data-ttu-id="55229-224">Hibrid</span><span class="sxs-lookup"><span data-stu-id="55229-224">Hybrid</span></span> |
| <span data-ttu-id="55229-225">T8DNDN5UDCWL7M72H</span><span class="sxs-lookup"><span data-stu-id="55229-225">T8DNDN5UDCWL7M72H</span></span> |<span data-ttu-id="55229-226">Családbiztonsági limuzin</span><span class="sxs-lookup"><span data-stu-id="55229-226">Family Saloon</span></span> |
| <span data-ttu-id="55229-227">4WPYRUZII5YV7YA42</span><span class="sxs-lookup"><span data-stu-id="55229-227">4WPYRUZII5YV7YA42</span></span> |<span data-ttu-id="55229-228">Sedan</span><span class="sxs-lookup"><span data-stu-id="55229-228">Sedan</span></span> |
| <span data-ttu-id="55229-229">D1ZVY26UV2BFGHZNO</span><span class="sxs-lookup"><span data-stu-id="55229-229">D1ZVY26UV2BFGHZNO</span></span> |<span data-ttu-id="55229-230">Hibrid</span><span class="sxs-lookup"><span data-stu-id="55229-230">Hybrid</span></span> |
| <span data-ttu-id="55229-231">XUF99EW9OIQOMV7Q7</span><span class="sxs-lookup"><span data-stu-id="55229-231">XUF99EW9OIQOMV7Q7</span></span> |<span data-ttu-id="55229-232">Családbiztonsági limuzin</span><span class="sxs-lookup"><span data-stu-id="55229-232">Family Saloon</span></span> |
| <span data-ttu-id="55229-233">8OMCL3LGI7XNCC21U</span><span class="sxs-lookup"><span data-stu-id="55229-233">8OMCL3LGI7XNCC21U</span></span> |<span data-ttu-id="55229-234">Váltható</span><span class="sxs-lookup"><span data-stu-id="55229-234">Convertible</span></span> |
| <span data-ttu-id="55229-235">…….</span><span class="sxs-lookup"><span data-stu-id="55229-235">…….</span></span> | |

### <a name="references"></a><span data-ttu-id="55229-236">Referencia</span><span class="sxs-lookup"><span data-stu-id="55229-236">References</span></span>
[<span data-ttu-id="55229-237">Vehicle telematika szimulátor Visual Studio megoldás</span><span class="sxs-lookup"><span data-stu-id="55229-237">Vehicle Telematics Simulator Visual Studio Solution</span></span>](http://go.microsoft.com/fwlink/?LinkId=717075) 

[<span data-ttu-id="55229-238">Az Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="55229-238">Azure Event Hub</span></span>](https://azure.microsoft.com/services/event-hubs/)

[<span data-ttu-id="55229-239">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="55229-239">Azure Data Factory</span></span>](https://azure.microsoft.com/documentation/learning-paths/data-factory/)

## <a name="ingestion"></a><span data-ttu-id="55229-240">Adatfeldolgozást</span><span class="sxs-lookup"><span data-stu-id="55229-240">Ingestion</span></span>
<span data-ttu-id="55229-241">Azure Event Hubs, a Stream Analytics és a Data Factory kombinációi kihasználhatók tooingest hello vehicle jelek, hello diagnosztikai eseményeket, és valós idejű és kötegelt elemzés.</span><span class="sxs-lookup"><span data-stu-id="55229-241">Combinations of Azure Event Hubs, Stream Analytics, and Data Factory are leveraged tooingest hello vehicle signals, hello diagnostic events, and real-time and batch analytics.</span></span> <span data-ttu-id="55229-242">Ezek az összetevők létrehozni és konfigurálni hello megoldás központi telepítésének részeként.</span><span class="sxs-lookup"><span data-stu-id="55229-242">All these components are created and configured as part of hello solution deployment.</span></span> 

### <a name="real-time-analysis"></a><span data-ttu-id="55229-243">Valós idejű elemzés</span><span class="sxs-lookup"><span data-stu-id="55229-243">Real-time analysis</span></span>
<span data-ttu-id="55229-244">hello hello Vehicle telematika szimulátor által előállított eseményeket közzétett toohello Eseményközpont használatával hello Event Hub SDK.</span><span class="sxs-lookup"><span data-stu-id="55229-244">hello events generated by hello Vehicle Telematics Simulator are published toohello Event Hub using hello Event Hub SDK.</span></span> <span data-ttu-id="55229-245">hello Stream Analytics-feladat ingests ezeket az eseményeket az Event Hubs hello és folyamatok hello valós idejű tooanalyze hello vehicle egészségügyi adatokat.</span><span class="sxs-lookup"><span data-stu-id="55229-245">hello Stream Analytics job ingests these events from hello Event Hub and processes hello data in real time tooanalyze hello vehicle health.</span></span> 

![Event hub irányítópult](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-event-hub-dashboard.png) 

<span data-ttu-id="55229-247">*4. ábra - Eseményközpont irányítópult*</span><span class="sxs-lookup"><span data-stu-id="55229-247">*Figure 4 - Event Hub dashboard*</span></span>

![A Stream Analytics-feladat adatainak feldolgozása](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-stream-analytics-job-processing-data.png) 

<span data-ttu-id="55229-249">*5. ábra - adatfeldolgozás Stream Analytics-feladat*</span><span class="sxs-lookup"><span data-stu-id="55229-249">*Figure 5 - Stream Analytics job processing data*</span></span>

<span data-ttu-id="55229-250">hello Stream Analytics-feladat;</span><span class="sxs-lookup"><span data-stu-id="55229-250">hello Stream Analytics job;</span></span>

* <span data-ttu-id="55229-251">az Event Hubs hello adatait ingests</span><span class="sxs-lookup"><span data-stu-id="55229-251">ingests data from hello Event Hub</span></span> 
* <span data-ttu-id="55229-252">hajt végre egy csatlakoztatás hello adatok toomap hello vehicle VIN toohello megfelelő referenciamodellje</span><span class="sxs-lookup"><span data-stu-id="55229-252">performs a join with hello reference data toomap hello vehicle VIN toohello corresponding model</span></span> 
* <span data-ttu-id="55229-253">az Azure blob-tároló gazdag kötegelt elemzés fenntartása őket.</span><span class="sxs-lookup"><span data-stu-id="55229-253">persists them into Azure blob storage for rich batch analytics.</span></span> 

<span data-ttu-id="55229-254">a következő Stream Analytics lekérdezési hello használt toopersist hello adatok Azure blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="55229-254">hello following Stream Analytics query is used toopersist hello data into Azure blob storage.</span></span> 

![Stream Analytics feladat lekérdezés adatfeldolgozást](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 

<span data-ttu-id="55229-256">*6. ábra – a Stream Analytics-feladat adatfeldolgozást lekérdezése*</span><span class="sxs-lookup"><span data-stu-id="55229-256">*Figure 6 - Stream Analytics job query for data ingestion*</span></span>

### <a name="batch-analysis"></a><span data-ttu-id="55229-257">Kötegelt elemzés</span><span class="sxs-lookup"><span data-stu-id="55229-257">Batch analysis</span></span>
<span data-ttu-id="55229-258">Azt is generál egy szimulált vehicle jelek és diagnosztikai adatkészlet gazdagabb kötegelt elemzés további mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="55229-258">We are also generating an additional volume of simulated vehicle signals and diagnostic dataset for richer batch analytics.</span></span> <span data-ttu-id="55229-259">Ez a helyes reprezentatív kötetet kötegelt feldolgozáshoz szükséges tooensure.</span><span class="sxs-lookup"><span data-stu-id="55229-259">This is required tooensure a good representative data volume for batch processing.</span></span> <span data-ttu-id="55229-260">Erre a célra egy elnevezett "PrepareSampleDataPipeline" hello Azure Data Factory munkafolyamat toogenerate folyamat használunk szimulált vehicle jelek és diagnosztikai adatkészlet egy év alatt érkezett.</span><span class="sxs-lookup"><span data-stu-id="55229-260">For this purpose, we are using a pipeline named "PrepareSampleDataPipeline" in hello Azure Data Factory workflow toogenerate one year's worth of simulated vehicle signals and diagnostic dataset.</span></span> <span data-ttu-id="55229-261">Kattintson a [adat-előállító egyéni tevékenység](http://go.microsoft.com/fwlink/?LinkId=717077) toodownload hello adat-előállító egyéni DotNet tevékenység Visual Studio megoldás testreszabni a követelmények alapján.</span><span class="sxs-lookup"><span data-stu-id="55229-261">Click [Data Factory custom activity](http://go.microsoft.com/fwlink/?LinkId=717077) toodownload hello Data Factory custom DotNet activity Visual Studio solution for customizations based on your requirements.</span></span> 

![Kötegfeldolgozási munkafolyamat mintaadatok előkészítése](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 

<span data-ttu-id="55229-263">*7. ábra - mintaadatok előkészítése kötegelt feldolgozásra munkafolyamat*</span><span class="sxs-lookup"><span data-stu-id="55229-263">*Figure 7 - Prepare Sample data for batch processing workflow*</span></span>

<span data-ttu-id="55229-264">hello csővezeték áll egy egyéni ADF .net tevékenység, itt megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="55229-264">hello pipeline consists of a custom ADF .Net Activity, show here:</span></span>

![PrepareSampleDataPipeline tevékenység](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-pipeline.png) 

<span data-ttu-id="55229-266">*8. ábra - PrepareSampleDataPipeline*</span><span class="sxs-lookup"><span data-stu-id="55229-266">*Figure 8 - PrepareSampleDataPipeline*</span></span>

<span data-ttu-id="55229-267">Miután hello folyamat sikeresen lefut, és "RawCarEventsTable" adatkészlet "Kész" van megjelölve egyéves érdemes szimulált vehicle jelek és diagnosztikai adatainak előállítása.</span><span class="sxs-lookup"><span data-stu-id="55229-267">Once hello pipeline executes successfully and "RawCarEventsTable" dataset is marked "Ready", one-year worth of simulated vehicle signals and diagnostic data are produced.</span></span> <span data-ttu-id="55229-268">Lásd a következő hello mappához és fájlhoz hello "connectedcar" tárolóban a storage-fiókban létrehozni:</span><span class="sxs-lookup"><span data-stu-id="55229-268">You see hello following folder and file created in your storage account under hello "connectedcar" container:</span></span>

![PrepareSampleDataPipeline kimeneti](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

<span data-ttu-id="55229-270">*9. ábra - PrepareSampleDataPipeline kimeneti*</span><span class="sxs-lookup"><span data-stu-id="55229-270">*Figure 9 - PrepareSampleDataPipeline Output*</span></span>

### <a name="references"></a><span data-ttu-id="55229-271">Referencia</span><span class="sxs-lookup"><span data-stu-id="55229-271">References</span></span>
[<span data-ttu-id="55229-272">Az Azure Event Hub SDK a streameket</span><span class="sxs-lookup"><span data-stu-id="55229-272">Azure Event Hub SDK for stream ingestion</span></span>](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

<span data-ttu-id="55229-273">[A mozgás képességek az Azure Data Factory adatok](../data-factory/data-factory-data-movement-activities.md)
[Azure Data Factory DotNet tevékenység](../data-factory/data-factory-use-custom-activities.md)</span><span class="sxs-lookup"><span data-stu-id="55229-273">[Azure Data Factory data movement capabilities](../data-factory/data-factory-data-movement-activities.md)
[Azure Data Factory DotNet Activity](../data-factory/data-factory-use-custom-activities.md)</span></span>

[<span data-ttu-id="55229-274">Az Azure Data Factory DotNet tevékenység visual studio megoldás a mintaadatok előkészítése</span><span class="sxs-lookup"><span data-stu-id="55229-274">Azure Data Factory DotNet activity visual studio solution for preparing sample data</span></span>](http://go.microsoft.com/fwlink/?LinkId=717077) 

## <a name="partition-hello-dataset"></a><span data-ttu-id="55229-275">Partíció hello adatkészlet</span><span class="sxs-lookup"><span data-stu-id="55229-275">Partition hello dataset</span></span>
<span data-ttu-id="55229-276">hello nyers félig strukturált vehicle jelek és diagnosztikai adatkészlet particionáltak hello adatok előkészítő lépésében egy év/hónap formátumú fájlba.</span><span class="sxs-lookup"><span data-stu-id="55229-276">hello raw semi-structured vehicle signals and diagnostic dataset are partitioned in hello data preparation step into a YEAR/MONTH format.</span></span> <span data-ttu-id="55229-277">A particionálás elősegíti hatékonyabb kérdez le, és kitölti-e hiba átvevő egy blob fiók toohello a következő hello első fiókként engedélyezésével méretezhető, hosszú távú tároláshoz.</span><span class="sxs-lookup"><span data-stu-id="55229-277">This partitioning promotes more efficient querying and scalable long-term storage by enabling fault-over from one blob account toohello next as hello first account fills up.</span></span> 

>[!NOTE] 
><span data-ttu-id="55229-278">Ez a lépés hello megoldásban alkalmazható csak toobatch feldolgozásával.</span><span class="sxs-lookup"><span data-stu-id="55229-278">This step in hello solution is applicable only toobatch processing.</span></span>

<span data-ttu-id="55229-279">Bemeneti és kimeneti adatok adatkezelés:</span><span class="sxs-lookup"><span data-stu-id="55229-279">Input and output data data management:</span></span>

* <span data-ttu-id="55229-280">Hello **kimeneti adatai** (feliratú *PartitionedCarEventsTable*) toobe másolatok hosszú időn hello eligazodást / "rawest" képernyő "Data Lake" hello felhasználói adatok.</span><span class="sxs-lookup"><span data-stu-id="55229-280">hello **output data** (labeled *PartitionedCarEventsTable*) is toobe kept for a long period of time as hello foundational/"rawest" form of data in hello customer's "Data Lake".</span></span> 
* <span data-ttu-id="55229-281">Hello **bemeneti adatai** toothis csővezeték volna általában elvesznek, mivel hello kimeneti adatokat adjon meg teljes visszaadása toohello rendelkezik – csak tárolt (particionált) jobban későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="55229-281">hello **input data** toothis pipeline would typically be discarded as hello output data has full fidelity toohello input - it's just stored (partitioned) better for subsequent use.</span></span>

![Partíció car események munkafolyamat](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-partition-car-events-workflow.png)

<span data-ttu-id="55229-283">*10. ábra – partíció Car események munkafolyamat*</span><span class="sxs-lookup"><span data-stu-id="55229-283">*Figure 10 – Partition Car Events workflow*</span></span>

<span data-ttu-id="55229-284">hello nyers adatok particionálása használata a Hive HDInsight tevékenység "PartitionCarEventsPipeline".</span><span class="sxs-lookup"><span data-stu-id="55229-284">hello raw data is partitioned using a Hive HDInsight activity in "PartitionCarEventsPipeline".</span></span> <span data-ttu-id="55229-285">egy évig az 1. lépésben létrehozott hello mintaadatok év/hónap szerint van particionálva.</span><span class="sxs-lookup"><span data-stu-id="55229-285">hello sample data generated in step 1 for a year is partitioned by YEAR/MONTH.</span></span> <span data-ttu-id="55229-286">hello partíció található, az év havonta (összesen 12 partíciók) használt toogenerate vehicle jelek és diagnosztikai adatokat.</span><span class="sxs-lookup"><span data-stu-id="55229-286">hello partitions are used toogenerate vehicle signals and diagnostic data for each month (total 12 partitions) of a year.</span></span> 

![PartitionCarEventsPipeline tevékenység](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-pipeline.png)

<span data-ttu-id="55229-288">*11. ábra - PartitionCarEventsPipeline*</span><span class="sxs-lookup"><span data-stu-id="55229-288">*Figure 11 - PartitionCarEventsPipeline*</span></span>

<span data-ttu-id="55229-289">***PartitionConnectedCarEvents Hive-parancsfájl***</span><span class="sxs-lookup"><span data-stu-id="55229-289">***PartitionConnectedCarEvents Hive Script***</span></span>

<span data-ttu-id="55229-290">hello következő Hive parancsfájl, "partitioncarevents.hql" nevű particionálás szolgál, és a hello letöltött zip hello "\demo\src\connectedcar\scripts" mappában található.</span><span class="sxs-lookup"><span data-stu-id="55229-290">hello following Hive script, named "partitioncarevents.hql", is used for partitioning and is located in hello "\demo\src\connectedcar\scripts" folder of hello downloaded zip.</span></span> 
    
    SET hive.exec.dynamic.partition=true;
    SET hive.exec.dynamic.partition.mode = nonstrict;
    set hive.cli.print.header=true;

    DROP TABLE IF EXISTS RawCarEvents; 
    CREATE EXTERNAL TABLE RawCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RAWINPUT}'; 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string
    ) partitioned by (YearNo int, MonthNo int) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDOUTPUT}';

    DROP TABLE IF EXISTS Stage_RawCarEvents; 
    CREATE TABLE IF NOT EXISTS Stage_RawCarEvents 
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string,
                YearNo                             int,
                MonthNo                         int) 
    ROW FORMAT delimited fields terminated by ',' LINES TERMINATED BY '10';

    INSERT OVERWRITE TABLE Stage_RawCarEvents
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        Year(gendate),
        Month(gendate)

    FROM RawCarEvents WHERE Year(gendate) = ${hiveconf:Year} AND Month(gendate) = ${hiveconf:Month}; 

    INSERT OVERWRITE TABLE PartitionedCarEvents PARTITION(YearNo, MonthNo) 
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        YearNo,
        MonthNo
    FROM Stage_RawCarEvents WHERE YearNo = ${hiveconf:Year} AND MonthNo = ${hiveconf:Month};

<span data-ttu-id="55229-291">Hello folyamat sikeres végrehajtása után a következő partíciók hozott létre a tárfiók hello "connectedcar" tárolóban hello láthatja.</span><span class="sxs-lookup"><span data-stu-id="55229-291">Once hello pipeline is executed successfully, you see hello following partitions generated in your storage account under hello "connectedcar" container.</span></span>

![A particionált kimeneti](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partitioned-output.png)

<span data-ttu-id="55229-293">*12. ábra - particionált kimeneti*</span><span class="sxs-lookup"><span data-stu-id="55229-293">*Figure 12 - Partitioned Output*</span></span>

<span data-ttu-id="55229-294">hello adatok most már megfelelően lett optimalizálva, kezelhető, és készen áll a toogain gazdag kötegelt insights további feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="55229-294">hello data is now optimized, is more manageable and ready for further processing toogain rich batch insights.</span></span> 

## <a name="data-analysis"></a><span data-ttu-id="55229-295">Adatok elemzése</span><span class="sxs-lookup"><span data-stu-id="55229-295">Data Analysis</span></span>
<span data-ttu-id="55229-296">Ebben a szakaszban láthatja, hogyan toocombine Azure Stream Analytics, az Azure Machine Learning, az Azure Data Factory és a gazdag Azure HDInsight speciális elemzés vehicle állapotát, és ki irányítja a szokásokat.</span><span class="sxs-lookup"><span data-stu-id="55229-296">In this section, you see how toocombine Azure Stream Analytics, Azure Machine Learning, Azure Data Factory, and Azure HDInsight for rich advanced analytics on vehicle health and driving habits.</span></span> <span data-ttu-id="55229-297">Itt három alszakaszokat van:</span><span class="sxs-lookup"><span data-stu-id="55229-297">There are three subsections here:</span></span>

1. <span data-ttu-id="55229-298">**Gépi tanulás**: a jelen alszakasz ismerteti a hello anomáliadetektálási észlelési kísérlet, amely a megoldás toopredict járművekről gyűjtött igénylő karbantartási karbantartási és járművekről gyűjtött igénylő visszahívások toosafety problémák miatt a használtuk.</span><span class="sxs-lookup"><span data-stu-id="55229-298">**Machine Learning**: This subsection contains information on hello anomaly detection experiment that we have used in this solution toopredict vehicles requiring servicing maintenance and vehicles requiring recalls due toosafety issues.</span></span>
2. <span data-ttu-id="55229-299">**Valós idejű elemzési**: a jelen alszakasz vonatkozó hello valós idejű elemzési hello Stream Analytics lekérdezési nyelv használatával, és hello gépi tanulás végrehajtott kísérletet használ egy egyéni alkalmazást valós idejű információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="55229-299">**Real-time analysis**: This subsection contains information regarding hello real-time analytics using hello Stream Analytics Query Language and operationalizing hello machine learning experiment in real time using a custom application.</span></span>
3. <span data-ttu-id="55229-300">**Kötegelt elemzés**: a jelen alszakasz hello átalakítása és az Azure HDInsight és az Azure Machine Learning Azure Data Factory által operationalized hello kötegelt adatok feldolgozása vonatkozó információkat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="55229-300">**Batch analysis**: This subsection contains information regarding hello transforming and processing of hello batch data using Azure HDInsight and Azure Machine Learning operationalized by Azure Data Factory.</span></span>

### <a name="machine-learning"></a><span data-ttu-id="55229-301">Machine Learning</span><span class="sxs-lookup"><span data-stu-id="55229-301">Machine Learning</span></span>
<span data-ttu-id="55229-302">Itt célunk toopredict hello járművekről gyűjtött vagy a karbantartási és a visszaírási bizonyos heath statisztika alapján.</span><span class="sxs-lookup"><span data-stu-id="55229-302">Our goal here is toopredict hello vehicles that require maintenance or recall based on certain heath statistics.</span></span> <span data-ttu-id="55229-303">A következő feltételek hello biztosítjuk</span><span class="sxs-lookup"><span data-stu-id="55229-303">We make hello following assumptions</span></span>

* <span data-ttu-id="55229-304">A következő három feltétel hello egyike igaz, ha hello járművekről gyűjtött szükséges **karbantartási karbantartási**:</span><span class="sxs-lookup"><span data-stu-id="55229-304">If one of hello following three conditions are true, hello vehicles require **servicing maintenance**:</span></span>
  
  * <span data-ttu-id="55229-305">Kulcsszava nyomás értéke alacsony</span><span class="sxs-lookup"><span data-stu-id="55229-305">Tire pressure is low</span></span>
  * <span data-ttu-id="55229-306">Motor olaj szintje alacsony</span><span class="sxs-lookup"><span data-stu-id="55229-306">Engine oil level is low</span></span>
  * <span data-ttu-id="55229-307">Motor túl magas</span><span class="sxs-lookup"><span data-stu-id="55229-307">Engine temperature is high</span></span>
* <span data-ttu-id="55229-308">Előfordulhat, ha hello a következő feltételek egyike teljesül, hello járművekről gyűjtött egy **biztonsági kérdés** és a szükséges **visszaírási**:</span><span class="sxs-lookup"><span data-stu-id="55229-308">If one of hello following conditions are true, hello vehicles may have a **safety issue** and require **recall**:</span></span>
  
  * <span data-ttu-id="55229-309">Motor magas, de külső hőmérséklet alacsony</span><span class="sxs-lookup"><span data-stu-id="55229-309">Engine temperature is high but outside temperature is low</span></span>
  * <span data-ttu-id="55229-310">Motor alacsony, de külső hőmérséklet magas</span><span class="sxs-lookup"><span data-stu-id="55229-310">Engine temperature is low but outside temperature is high</span></span>

<span data-ttu-id="55229-311">Hello előző követelmények alapján létrehoztunk két külön modellek toodetect rendellenességek észlelését, egy a vehicle karbantartási észleléséhez és egy a vehicle visszaírási észleléséhez.</span><span class="sxs-lookup"><span data-stu-id="55229-311">Based on hello previous requirements, we have created two separate models toodetect anomalies, one for vehicle maintenance detection, and one for vehicle recall detection.</span></span> <span data-ttu-id="55229-312">Mindkét modellek hello beépített egyszerű összetevő elemzés (PEM) algoritmus használják a anomáliadetektálás.</span><span class="sxs-lookup"><span data-stu-id="55229-312">In both these models, hello built-in Principal Component Analysis (PCA) algorithm is used for anomaly detection.</span></span> 

<span data-ttu-id="55229-313">**Karbantartási modell**</span><span class="sxs-lookup"><span data-stu-id="55229-313">**Maintenance detection model**</span></span>

<span data-ttu-id="55229-314">-Kulcsszava nyomás, motor olaj vagy motor hőmérséklet - három mutatók egyikét megfelel az megfelelő állapotba, ha a modell hello karbantartási jelentések az anomáliadetektálási.</span><span class="sxs-lookup"><span data-stu-id="55229-314">If one of three indicators - tire pressure, engine oil, or engine temperature - satisfies its respective condition, hello maintenance detection model reports an anomaly.</span></span> <span data-ttu-id="55229-315">Ennek eredményeképpen csak kell tooconsider három változókhoz hello modell fejlesztése során.</span><span class="sxs-lookup"><span data-stu-id="55229-315">As a result, we only need tooconsider these three variables in building hello model.</span></span> <span data-ttu-id="55229-316">A kísérlet az Azure Machine Learning, először használjuk a **Select Columns in Dataset** modul tooextract három változókhoz.</span><span class="sxs-lookup"><span data-stu-id="55229-316">In our experiment in Azure Machine Learning, we first use a **Select Columns in Dataset** module tooextract these three variables.</span></span> <span data-ttu-id="55229-317">Ezután hello PCA alapú észlelési modul toobuild hello anomáliadetektálási modell használjuk.</span><span class="sxs-lookup"><span data-stu-id="55229-317">Next we use hello PCA-based anomaly detection module toobuild hello anomaly detection model.</span></span> 

<span data-ttu-id="55229-318">Egyszerű összetevő elemzésre (PEM) egy meglévő technika, a gépi tanulás alkalmazott toofeature kijelölés, a besorolást és a anomáliadetektálási észleléséhez használható.</span><span class="sxs-lookup"><span data-stu-id="55229-318">Principal Component Analysis (PCA) is an established technique in machine learning that can be applied toofeature selection, classification, and anomaly detection.</span></span> <span data-ttu-id="55229-319">PEM alakít egy készletét tartalmazó esetlegesen kapcsolódó változók neve a fő összetevőit értékek esetében.</span><span class="sxs-lookup"><span data-stu-id="55229-319">PCA converts a set of case containing possibly correlated variables, into a set of values called principal components.</span></span> <span data-ttu-id="55229-320">hello kulcs a PEM-alapú modellezési lényege, alsó dimenziós szóközzel tooproject adatokat, hogy a szolgáltatások és rendellenességeket könnyebben azonosítható legyen.</span><span class="sxs-lookup"><span data-stu-id="55229-320">hello key idea of PCA-based modeling is tooproject data onto a lower-dimensional space so that features and anomalies can be more easily identified.</span></span>

<span data-ttu-id="55229-321">Az egyes új bemeneti túl hello modell, hello anomáliadetektálási érzékelő először kiszámítja a leképezés a hello eigenvectors, és majd kiszámítja hello normalizált újjáépítése hiba.</span><span class="sxs-lookup"><span data-stu-id="55229-321">For each new input too hello detection model, hello anomaly detector first computes its projection on hello eigenvectors, and then computes hello normalized reconstruction error.</span></span> <span data-ttu-id="55229-322">Ez a normalizált hiba hello anomáliadetektálási pontszám.</span><span class="sxs-lookup"><span data-stu-id="55229-322">This normalized error is hello anomaly score.</span></span> <span data-ttu-id="55229-323">hello nagyobb hello hiba hello több rendellenes hello példány.</span><span class="sxs-lookup"><span data-stu-id="55229-323">hello higher hello error, hello more anomalous hello instance is.</span></span> 

<span data-ttu-id="55229-324">Hello karbantartási észlelési problémát, a rekordokban tekinthető kulcsszava nyomás motor olaj és motor által definiált egy pont a 3-dimenziós térben koordinátái.</span><span class="sxs-lookup"><span data-stu-id="55229-324">In hello maintenance detection problem, each record can be considered as a point in a 3-dimensional space defined by tire pressure, engine oil, and engine temperature coordinates.</span></span> <span data-ttu-id="55229-325">toocapture ezek rendellenességek észlelését, azt kivetíthetik hello eredeti adatok hello 3 dimenziós területen a 2-dimenziós területére PEM használatával.</span><span class="sxs-lookup"><span data-stu-id="55229-325">toocapture these anomalies, we can project hello original data in hello 3-dimensional space onto a 2-dimensional space using PCA.</span></span> <span data-ttu-id="55229-326">Ebből kifolyólag hello paraméter összetevők toouse száma hivatott PEM toobe 2.</span><span class="sxs-lookup"><span data-stu-id="55229-326">Thus, we set hello parameter Number of components toouse in PCA toobe 2.</span></span> <span data-ttu-id="55229-327">Ez a paraméter a PEM-alapú anomáliadetektálás alkalmazása fontos szerepet játszik.</span><span class="sxs-lookup"><span data-stu-id="55229-327">This parameter plays an important role in applying PCA-based anomaly detection.</span></span> <span data-ttu-id="55229-328">PEM kiálló adatokat, miután azt azonosíthatja ezeket a rendellenességeket könnyebben.</span><span class="sxs-lookup"><span data-stu-id="55229-328">After projecting data using PCA, we can identify these anomalies more easily.</span></span>

<span data-ttu-id="55229-329">**Visszaírási anomáliadetektálási modell** hello Select Columns in Dataset és PCA alapú használjuk hello visszaírási anomáliadetektálási modell, észlelési modulok hasonló módon.</span><span class="sxs-lookup"><span data-stu-id="55229-329">**Recall anomaly detection model** In hello recall anomaly detection model, we use hello Select Columns in Dataset and PCA-based anomaly detection modules in a similar way.</span></span> <span data-ttu-id="55229-330">Pontosabban, azt először nyerje ki három változók - motor, a külső hőmérséklet és a sebességét – hello segítségével **Select Columns in Dataset** modul.</span><span class="sxs-lookup"><span data-stu-id="55229-330">Specifically, we first extract three variables - engine temperature, outside temperature, and speed - using hello **Select Columns in Dataset** module.</span></span> <span data-ttu-id="55229-331">Is magában foglalja az hello sebesség változó, mert hello motor hőmérséklet általában korrelált toohello sebessége.</span><span class="sxs-lookup"><span data-stu-id="55229-331">We also include hello speed variable since hello engine temperature typically is correlated toohello speed.</span></span> <span data-ttu-id="55229-332">PCA alapú észlelési modul tooproject hello adatok mellett a 2-dimenziós területére hello 3 dimenziós területéből használjuk.</span><span class="sxs-lookup"><span data-stu-id="55229-332">Next we use PCA-based anomaly detection module tooproject hello data from hello 3-dimensional space onto a 2-dimensional space.</span></span> <span data-ttu-id="55229-333">hello visszaírási feltételek teljesülnek, és ehhez hello vehicle kell-e visszaírási amikor motor és a külső hőmérséklet magas negatívan közötti kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="55229-333">hello recall criteria are satisfied and so hello vehicle requires recall when engine temperature and outside temperature are highly negatively correlated.</span></span> <span data-ttu-id="55229-334">PCA alapú észlelési algoritmus használ, azt rögzítheti a hello rendellenességeket PEM végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="55229-334">Using PCA-based anomaly detection algorithm, we can capture hello anomalies after performing PCA.</span></span> 

<span data-ttu-id="55229-335">Vagy a modell betanításakor kell toouse megszokott adatforgalmi, amely nem igényli a karbantartás vagy visszaírási hello bemeneti adatok tootrain hello PCA alapú észlelési modellre.</span><span class="sxs-lookup"><span data-stu-id="55229-335">When training either model, we need toouse normal data, which does not require maintenance or recall as hello input data tootrain hello PCA-based anomaly detection model.</span></span> <span data-ttu-id="55229-336">A pontozási kísérlet hello használjuk betanítása hello anomáliadetektálási észlelési modell toodetect hello vehicle karbantartás vagy visszaírási szükséges-e.</span><span class="sxs-lookup"><span data-stu-id="55229-336">In hello scoring experiment, we use hello trained anomaly detection model toodetect whether or not hello vehicle requires maintenance or recall.</span></span> 

### <a name="real-time-analysis"></a><span data-ttu-id="55229-337">Valós idejű elemzés</span><span class="sxs-lookup"><span data-stu-id="55229-337">Real-time analysis</span></span>
<span data-ttu-id="55229-338">Stream Analytics SQL-lekérdezés a következő hello használt összes hello átlaga tooget hello például vehicle sebessége, üzemanyag szint, motor, kilométer olvasási, kulcsszava nyomás, motor olaj szint és más fontos vehicle paraméterek.</span><span class="sxs-lookup"><span data-stu-id="55229-338">hello following Stream Analytics SQL Query is used tooget hello average of all hello important vehicle parameters like vehicle speed, fuel level, engine temperature, odometer reading, tire pressure, engine oil level, and others.</span></span> <span data-ttu-id="55229-339">hello átlagok használt toodetect rendellenességek észlelését, ki riasztásokat, illetve meghatározni hello járművekről gyűjtött adott régióban üzemeltetett feltételeinek általános állapotát, és toodemographics összefüggéseket.</span><span class="sxs-lookup"><span data-stu-id="55229-339">hello averages are used toodetect anomalies, issue alerts, and determine hello overall health conditions of vehicles operated in specific region and then correlate it toodemographics.</span></span> 

![A valós idejű feldolgozással Stream Analytics-lekérdezés](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig13-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

<span data-ttu-id="55229-341">*13. ábra: valós idejű feldolgozással Stream Analytics-lekérdezés*</span><span class="sxs-lookup"><span data-stu-id="55229-341">*Figure 13 – Stream Analytics query for real-time processing*</span></span>

<span data-ttu-id="55229-342">Minden hello átlagok kiszámítása a 3-második TumblingWindow keresztül.</span><span class="sxs-lookup"><span data-stu-id="55229-342">All hello averages are calculated over a 3-second TumblingWindow.</span></span> <span data-ttu-id="55229-343">Használjuk TubmlingWindow ebben az esetben óta mozaikként, átfedés nélkül és összefüggő időközök szükségesek.</span><span class="sxs-lookup"><span data-stu-id="55229-343">We are using TubmlingWindow in this case since we require non-overlapping and contiguous time intervals.</span></span> 

<span data-ttu-id="55229-344">További információ az Azure Stream Analytics összes hello "Ablakozó" képességei toolearn kattintson [Ablakozó (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835019.aspx).</span><span class="sxs-lookup"><span data-stu-id="55229-344">toolearn more about all hello "Windowing" capabilities in Azure Stream Analytics, click [Windowing (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835019.aspx).</span></span>

<span data-ttu-id="55229-345">**Valós idejű előrejelzés**</span><span class="sxs-lookup"><span data-stu-id="55229-345">**Real-time prediction**</span></span>

<span data-ttu-id="55229-346">Az alkalmazás megtalálható részeként hello megoldás toooperationalize hello gépi tanulási modell valós időben.</span><span class="sxs-lookup"><span data-stu-id="55229-346">An application is included as part of hello solution toooperationalize hello machine learning model in real time.</span></span> <span data-ttu-id="55229-347">A "RealTimeDashboardApp" nevű alkalmazás létrehozásáról és hello megoldás központi telepítésének részeként.</span><span class="sxs-lookup"><span data-stu-id="55229-347">This application called “RealTimeDashboardApp” is created and configured as part of hello solution deployment.</span></span> <span data-ttu-id="55229-348">hello alkalmazás hello következőket végzi:</span><span class="sxs-lookup"><span data-stu-id="55229-348">hello application performs hello following:</span></span>

1. <span data-ttu-id="55229-349">Tooan Eseményközpont példány ahol Stream Analytics közzétesz hello események bankkártyaszám folyamatosan figyeli.</span><span class="sxs-lookup"><span data-stu-id="55229-349">Listens tooan Event Hub instance where Stream Analytics is publishing hello events in a pattern continuously.</span></span> <span data-ttu-id="55229-350">![Stream Analytics lekérdezési hello adatok közzétételére vonatkozó](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-stream-analytics-query-for-publishing.png) *14. ábra: a Stream Analytics lekérdezési hello adatok tooan közzététel Event Hub-példány kimeneti*</span><span class="sxs-lookup"><span data-stu-id="55229-350">![Stream Analytics query for publishing hello data](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-stream-analytics-query-for-publishing.png) *Figure 14 – Stream Analytics query for publishing hello data tooan output Event Hub instance*</span></span> 
2. <span data-ttu-id="55229-351">Minden eseményhez, amely megkapja ezt az alkalmazást:</span><span class="sxs-lookup"><span data-stu-id="55229-351">For every event that this application receives:</span></span> 
   
   * <span data-ttu-id="55229-352">Folyamatok hello adatok használata a Machine Learning kérés-válasz pontozási (RR-EKET) végpont.</span><span class="sxs-lookup"><span data-stu-id="55229-352">Processes hello data using Machine Learning Request-Response Scoring (RRS) endpoint.</span></span> <span data-ttu-id="55229-353">hello RR-EKET végpontot automatikusan közzétesz hello központi telepítésének részeként.</span><span class="sxs-lookup"><span data-stu-id="55229-353">hello RRS endpoint is automatically published as part of hello deployment.</span></span>
   * <span data-ttu-id="55229-354">hello RR-EKET eredménye a közzétett tooa Power BI dataset hello leküldéses API-k használatával.</span><span class="sxs-lookup"><span data-stu-id="55229-354">hello RRS output is published tooa Power BI dataset using hello push APIs.</span></span>

<span data-ttu-id="55229-355">Ebben a mintában esetében is alkalmazandó tooscenarios használni kívánt toointegrate egy üzletági (LoB) alkalmazás hello valós idejű elemzési folyamat, például a riasztások, értesítések és üzenetkezelési forgatókönyvek esetén.</span><span class="sxs-lookup"><span data-stu-id="55229-355">This pattern is also applicable tooscenarios in which you want toointegrate a Line of Business (LoB) application with hello real-time analytics flow, for scenarios such as alerts, notifications, and messaging.</span></span>

<span data-ttu-id="55229-356">Kattintson a [RealtimeDashboardApp letöltési](http://go.microsoft.com/fwlink/?LinkId=717078) toodownload hello RealtimeDashboardApp Visual Studio megoldás testreszabásokat.</span><span class="sxs-lookup"><span data-stu-id="55229-356">Click [RealtimeDashboardApp download](http://go.microsoft.com/fwlink/?LinkId=717078) toodownload hello RealtimeDashboardApp Visual Studio solution for customizations.</span></span> 

<span data-ttu-id="55229-357">**valós idejű irányítópulton alkalmazás tooexecute hello**</span><span class="sxs-lookup"><span data-stu-id="55229-357">**tooexecute hello Real-time Dashboard Application**</span></span>
1. <span data-ttu-id="55229-358">Bontsa ki és mentse helyileg ![RealtimeDashboardApp mappa](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-realtimedashboardapp-folder.png) *16. ábra – RealtimeDashboardApp mappa*</span><span class="sxs-lookup"><span data-stu-id="55229-358">Extract and save locally ![RealtimeDashboardApp folder](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-realtimedashboardapp-folder.png) *Figure 16 – RealtimeDashboardApp folder*</span></span>  
2. <span data-ttu-id="55229-359">Hello alkalmazás RealtimeDashboardApp.exe végrehajtása</span><span class="sxs-lookup"><span data-stu-id="55229-359">Execute hello application RealtimeDashboardApp.exe</span></span>
3. <span data-ttu-id="55229-360">Adjon meg érvényes Power BI hitelesítő adatokat, jelentkezzen be, és kattintson az elfogadás</span><span class="sxs-lookup"><span data-stu-id="55229-360">Provide valid Power BI credentials, sign in and click Accept</span></span> ![Valós idejű irányítópulton app bejelentkezési tooPower BI](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) ![Valós idejű irányítópult-alkalmazás tooPower BI-bejelentkezés befejezése](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

<span data-ttu-id="55229-363">*17. ábra – RealtimeDashboardApp: Bejelentkezési tooPower BI*</span><span class="sxs-lookup"><span data-stu-id="55229-363">*Figure 17 – RealtimeDashboardApp: Sign-in tooPower BI*</span></span>

>[!NOTE] 
><span data-ttu-id="55229-364">Ha azt szeretné, hogy tooflush hello Power BI dataset, vezető hello RealtimeDashboardApp hello "flushdata" paraméterrel:</span><span class="sxs-lookup"><span data-stu-id="55229-364">If you want tooflush hello Power BI dataset, execute hello RealtimeDashboardApp with hello "flushdata" parameter:</span></span> 

    RealtimeDashboardApp.exe -flushdata


### <a name="batch-analysis"></a><span data-ttu-id="55229-365">Kötegelt elemzés</span><span class="sxs-lookup"><span data-stu-id="55229-365">Batch analysis</span></span>
<span data-ttu-id="55229-366">hello itt célja hogyan Contoso motorok tooshow hello Azure számítási képességei tooharness big Data típusú adatok toogain részletes információkat a mintát, a használati működésének és a vehicle állapotát befolyásoló tényezők használja.</span><span class="sxs-lookup"><span data-stu-id="55229-366">hello goal here is tooshow how Contoso Motors utilizes hello Azure compute capabilities tooharness big data toogain rich insights on driving pattern, usage behavior, and vehicle health.</span></span> <span data-ttu-id="55229-367">Ez lehetővé teszi:</span><span class="sxs-lookup"><span data-stu-id="55229-367">This makes it possible to:</span></span>

* <span data-ttu-id="55229-368">Hello felhasználói élmény javításához, és teszi olcsóbb szokásait és üzemanyag hatékony vezetői viselkedések vezetői szóló insights megadásával</span><span class="sxs-lookup"><span data-stu-id="55229-368">Improve hello customer experience and make it cheaper by providing insights on driving habits and fuel efficient driving behaviors</span></span>
* <span data-ttu-id="55229-369">Proaktív információ az ügyfelek és a vezetői patters toogovern üzleti döntéseket hozhat és hello legjobb biztosított osztály termékek és szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="55229-369">Learn proactively about customers and their driving patters toogovern business decisions and provide hello best in class products & services</span></span>

<span data-ttu-id="55229-370">Ebben a megoldásban azt céloz meg a következő metrikák hello:</span><span class="sxs-lookup"><span data-stu-id="55229-370">In this solution, we are targeting hello following metrics:</span></span>

1. <span data-ttu-id="55229-371">**Agresszív viselkedését befolyásoló tényezők**: hello trend hello modellek, helyek, vezetői feltételek és hello év toogain áttekinthetik a agresszív vezetői minták idején azonosítja.</span><span class="sxs-lookup"><span data-stu-id="55229-371">**Aggressive driving behavior**: Identifies hello trend of hello models, locations, driving conditions, and time of hello year toogain insights on aggressive driving patterns.</span></span> <span data-ttu-id="55229-372">Contoso motor ezeket insights segítségével marketingkampányok, új személyre szabott szolgáltatásokat és a használat alapú biztosítási vezetői.</span><span class="sxs-lookup"><span data-stu-id="55229-372">Contoso Motors can use these insights for marketing campaigns, driving new personalized features and usage-based insurance.</span></span>
2. <span data-ttu-id="55229-373">**Hatékony vezetői viselkedés üzemanyag-**: hello trend hello modellek, helyek, vezetői feltételek és hello év toogain áttekinthetik a üzemanyag hatékony vezetői minták idején azonosítja.</span><span class="sxs-lookup"><span data-stu-id="55229-373">**Fuel efficient driving behavior**: Identifies hello trend of hello models, locations, driving conditions, and time of hello year toogain insights on fuel efficient driving patterns.</span></span> <span data-ttu-id="55229-374">Contoso motorok használhatja ezeket insights marketingkampányok, ki irányítja az új funkciók, és proaktív jelentéskészítési toohello illesztőprogramjait hatályos és a környezet rövid vezetői szokásait költség.</span><span class="sxs-lookup"><span data-stu-id="55229-374">Contoso Motors can use these insights for marketing campaigns, driving new features and proactive reporting toohello drivers for cost effective and environment friendly driving habits.</span></span> 
3. <span data-ttu-id="55229-375">**Modellek visszahívása**: visszahívások igénylő által végrehajtott hello anomáliadetektálási észlelési gépi tanulási kísérlet modelljeit azonosítja</span><span class="sxs-lookup"><span data-stu-id="55229-375">**Recall models**: Identifies models requiring recalls by operationalizing hello anomaly detection machine learning experiment</span></span>

<span data-ttu-id="55229-376">Nézzük meg az egyes ezeket a mérési hello részleteinek</span><span class="sxs-lookup"><span data-stu-id="55229-376">Let's look into hello details of each of these metrics,</span></span>

<span data-ttu-id="55229-377">**Agresszív vezetői minta**</span><span class="sxs-lookup"><span data-stu-id="55229-377">**Aggressive driving pattern**</span></span>

<span data-ttu-id="55229-378">hello particionálva vehicle jelek és diagnosztikai adatok feldolgozásának Hive toodetermine hello modellek, a hely, a vehicle használatával "AggresiveDrivingPatternPipeline" nevű hello folyamat befolyásoló tényezők feltételeket, és más paramétereket, amelyeket mutat agresszív vezetői mintát.</span><span class="sxs-lookup"><span data-stu-id="55229-378">hello partitioned vehicle signals and diagnostic data are processed in hello pipeline named "AggresiveDrivingPatternPipeline" using Hive toodetermine hello models, location, vehicle, driving conditions, and other parameters that exhibits aggressive driving pattern.</span></span>

<span data-ttu-id="55229-379">![Agresszív vezetői minta munkafolyamat](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-aggressive-driving-pattern.png) 
*. ábra 18 – agresszív vezetői minta munkafolyamat*</span><span class="sxs-lookup"><span data-stu-id="55229-379">![Aggressive driving pattern workflow](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-aggressive-driving-pattern.png) 
*Figure 18 – Aggressive driving pattern workflow*</span></span>


<span data-ttu-id="55229-380">***Agresszív vezetői minta Hive-lekérdezések***</span><span class="sxs-lookup"><span data-stu-id="55229-380">***Aggressive driving pattern Hive query***</span></span>

<span data-ttu-id="55229-381">hello Hive parancsfájl agresszív vezetői feltétel mintát elemzéséhez használt "aggresivedriving.hql" nevű, "\demo\src\connectedcar\scripts" mappában található hello letöltött zip helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="55229-381">hello Hive script named "aggresivedriving.hql" used for analyzing aggressive driving condition pattern is located at "\demo\src\connectedcar\scripts" folder of hello downloaded zip.</span></span> 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS CarEventsAggresive; 
    CREATE EXTERNAL TABLE CarEventsAggresive
    (
                   vin                         string, 
                model                        string,
                timestamp                    string,
                city                        string,
                speed                          string,
                transmission_gear_position    string,
                brake_pedal_status            string,
                Year                        string,
                Month                        string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:AGGRESIVEOUTPUT}';



    INSERT OVERWRITE TABLE CarEventsAggresive
    select
    vin,
    model,
    timestamp,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND brake_pedal_status = '1' AND speed >= '50'


<span data-ttu-id="55229-382">Akkor használja a hello vehicle tartozó átviteli fogaskerék pozíció, berendezés tartásához lehetőleg keveset állapotát, és kombinációja sebesség toodetect reckless/agresszív befolyásoló tényezők viselkedését fékezés nagy sebességű minta alapján.</span><span class="sxs-lookup"><span data-stu-id="55229-382">It uses hello combination of vehicle's transmission gear position, brake pedal status, and speed toodetect reckless/aggressive driving behavior based on braking pattern at high speed.</span></span> 

<span data-ttu-id="55229-383">Hello folyamat sikeres végrehajtása után a következő partíciók hozott létre a tárfiók hello "connectedcar" tárolóban hello láthatja.</span><span class="sxs-lookup"><span data-stu-id="55229-383">Once hello pipeline is executed successfully, you see hello following partitions generated in your storage account under hello "connectedcar" container.</span></span>

![AggressiveDrivingPatternPipeline kimeneti](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-aggressive-driving-pattern-output.png) 

<span data-ttu-id="55229-385">*19. ábra – AggressiveDrivingPatternPipeline kimeneti*</span><span class="sxs-lookup"><span data-stu-id="55229-385">*Figure 19 – AggressiveDrivingPatternPipeline output*</span></span>

<span data-ttu-id="55229-386">**Üzemanyag hatékony vezetői minta**</span><span class="sxs-lookup"><span data-stu-id="55229-386">**Fuel efficient driving pattern**</span></span>

<span data-ttu-id="55229-387">hello particionálva vehicle jelek és diagnosztikai adatok hello adatcsatorna "FuelEfficientDrivingPatternPipeline" nevű dolgozza fel.</span><span class="sxs-lookup"><span data-stu-id="55229-387">hello partitioned vehicle signals and diagnostic data are processed in hello pipeline named "FuelEfficientDrivingPatternPipeline".</span></span> <span data-ttu-id="55229-388">Hive használt toodetermine hello modellek, hely, vehicle, üzemi, de más tulajdonságok, amelyeket üzemanyag hatékony vezetői mintát.</span><span class="sxs-lookup"><span data-stu-id="55229-388">Hive is used toodetermine hello models, location, vehicle, driving conditions, and other properties that exhibit fuel efficient driving pattern.</span></span>

![Tíz vezetői minta](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19-vehicle-telematics-fuel-efficient-driving-pattern.png) 

<span data-ttu-id="55229-390">*20. ábra – tíz vezetői minta munkafolyamat*</span><span class="sxs-lookup"><span data-stu-id="55229-390">*Figure 20 – Fuel-efficient driving pattern workflow*</span></span>

<span data-ttu-id="55229-391">***Üzemanyag hatékony vezetői minta Hive lekérdezés***</span><span class="sxs-lookup"><span data-stu-id="55229-391">***Fuel efficient driving pattern Hive query***</span></span>

<span data-ttu-id="55229-392">hello Hive parancsfájl agresszív vezetői feltétel mintát elemzéséhez használt "fuelefficientdriving.hql" nevű, "\demo\src\connectedcar\scripts" mappában található hello letöltött zip helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="55229-392">hello Hive script named "fuelefficientdriving.hql" used for analyzing aggressive driving condition pattern is located at "\demo\src\connectedcar\scripts" folder of hello downloaded zip.</span></span> 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                                string,
                model                            string,
                timestamp                        string,
                outsidetemperature                string,
                enginetemperature                string,
                speed                            string,
                fuel                            string,
                engineoil                        string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position        string,
                parking_brake_status            string,
                headlamp_status                    string,
                brake_pedal_status                string,
                transmission_gear_position        string,
                ignition_status                    string,
                windshield_wiper_status            string,
                abs                              string,
                gendate                            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS FuelEfficientDriving; 
    CREATE EXTERNAL TABLE FuelEfficientDriving
    (
                   vin                         string, 
                model                        string,
                   city                        string,
                speed                          string,
                transmission_gear_position    string,                
                brake_pedal_status            string,            
                accelerator_pedal_position    string,                             
                Year                        string,
                Month                        string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:FUELEFFICIENTOUTPUT}';



    INSERT OVERWRITE TABLE FuelEfficientDriving
    select
    vin,
    model,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    accelerator_pedal_position,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND parking_brake_status = '0' AND brake_pedal_status = '0' AND speed <= '60' AND accelerator_pedal_position >= '50'


<span data-ttu-id="55229-393">Vehicle tartozó átviteli fogaskerék pozícióját, berendezés tartásához lehetőleg keveset állapota, sebesség és gyorsító tartásához lehetőleg keveset pozíció toodetect üzemanyag hatékony hajtott viselkedés alapján fékezés, a gyorsítás hello kombinációja használ, és minták sebessége.</span><span class="sxs-lookup"><span data-stu-id="55229-393">It uses hello combination of vehicle's transmission gear position, brake pedal status, speed, and accelerator pedal position toodetect fuel efficient driving behavior based on acceleration, braking, and speed patterns.</span></span> 

<span data-ttu-id="55229-394">Hello folyamat sikeres végrehajtása után a következő partíciók hozott létre a tárfiók hello "connectedcar" tárolóban hello láthatja.</span><span class="sxs-lookup"><span data-stu-id="55229-394">Once hello pipeline is executed successfully, you see hello following partitions generated in your storage account under hello "connectedcar" container.</span></span>

![FuelEfficientDrivingPatternPipeline kimeneti](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

<span data-ttu-id="55229-396">*21. ábra – FuelEfficientDrivingPatternPipeline kimeneti*</span><span class="sxs-lookup"><span data-stu-id="55229-396">*Figure 21 – FuelEfficientDrivingPatternPipeline output*</span></span>

<span data-ttu-id="55229-397">**Előrejelzés visszahívása**</span><span class="sxs-lookup"><span data-stu-id="55229-397">**Recall Predictions**</span></span>

<span data-ttu-id="55229-398">hello gépi tanulási kísérlet kiosztásakor és közzétett webszolgáltatásként hello megoldás központi telepítésének részeként.</span><span class="sxs-lookup"><span data-stu-id="55229-398">hello machine learning experiment is provisioned and published as a web service as part of hello solution deployment.</span></span> <span data-ttu-id="55229-399">hello kötegelt pontozási végpont a rendszer a munkafolyamatban, a data factory kapcsolt szolgáltatásként regisztrált és operationalized használatával a data factory kötegelt pontozási tevékenység elkészítéséhez használja.</span><span class="sxs-lookup"><span data-stu-id="55229-399">hello batch scoring end point is leveraged in this workflow, registered as a data factory linked service and operationalized using data factory batch scoring activity.</span></span>

![Machine Learning-végpont](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig21-vehicle-telematics-machine-learning-endpoint.png) 

<span data-ttu-id="55229-401">*A data factory kapcsolt szolgáltatásként regisztrált 22. ábra – gépi tanulási végpont*</span><span class="sxs-lookup"><span data-stu-id="55229-401">*Figure 22 – Machine learning endpoint registered as a linked service in data factory*</span></span>

<span data-ttu-id="55229-402">hello regisztrált kapcsolódó szolgáltatás szerepel hello DetectAnomalyPipeline tooscore hello adatok hello anomáliadetektálási modell használatával.</span><span class="sxs-lookup"><span data-stu-id="55229-402">hello registered linked service is used in hello DetectAnomalyPipeline tooscore hello data using hello anomaly detection model.</span></span> 

![Számítógép-tanulás kötegelt pontozási tevékenység adat](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aml-batch-scoring.png) 

<span data-ttu-id="55229-404">*23. ábra – a data factory Azure Machine Learning kötegelt pontozási tevékenység*</span><span class="sxs-lookup"><span data-stu-id="55229-404">*Figure 23 – Azure Machine Learning Batch Scoring activity in data factory*</span></span> 

<span data-ttu-id="55229-405">Az adatok előkészítése az adatcsatorna végre, hogy a kötegelt pontozás webszolgáltatás hello is operationalized néhány lépésből áll.</span><span class="sxs-lookup"><span data-stu-id="55229-405">There are few steps performed in this pipeline for data preparation so that it can be operationalized with hello batch scoring web service.</span></span> 

![Járművekről gyűjtött visszahívások igénylő előrejelzésére DetectAnomalyPipeline](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-pipeline-predicting-recalls.png) 

<span data-ttu-id="55229-407">*24. ábra – járművekről gyűjtött visszahívások igénylő előrejelzésére DetectAnomalyPipeline*</span><span class="sxs-lookup"><span data-stu-id="55229-407">*Figure 24 – DetectAnomalyPipeline for predicting vehicles requiring recalls*</span></span> 

<span data-ttu-id="55229-408">***Az anomáliadetektálási észlelési Hive-lekérdezések***</span><span class="sxs-lookup"><span data-stu-id="55229-408">***Anomaly detection Hive query***</span></span>

<span data-ttu-id="55229-409">Ha hello pontozási befejeződött, egy HDInsight tevékenysége használt tooprocess és hello modell valószínűségét jelző pontszámot 0,60 vagy nagyobb a rendellenességeket eszközként besorolt összesített hello adatok.</span><span class="sxs-lookup"><span data-stu-id="55229-409">Once hello scoring is completed, an HDInsight activity is used tooprocess and aggregate hello data that are categorized as anomalies by hello model with a probability score of 0.60 or higher.</span></span>

    DROP TABLE IF EXISTS CarEventsAnomaly; 
    CREATE EXTERNAL TABLE CarEventsAnomaly 
    (
                vin                            string,
                model                        string,
                gendate                        string,
                outsidetemperature            string,
                enginetemperature            string,
                speed                        string,
                fuel                        string,
                engineoil                    string,
                tirepressure                string,
                odometer                    string,
                city                        string,
                accelerator_pedal_position    string,
                parking_brake_status        string,
                headlamp_status                string,
                brake_pedal_status            string,
                transmission_gear_position    string,
                ignition_status                string,
                windshield_wiper_status        string,
                abs                          string,
                maintenanceLabel            string,
                maintenanceProbability        string,
                RecallLabel                    string,
                RecallProbability            string

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:ANOMALYOUTPUT}';

    DROP TABLE IF EXISTS RecallModel; 
    CREATE EXTERNAL TABLE RecallModel 
    (

                vin                            string,
                model                        string,
                city                        string,
                outsidetemperature            string,
                enginetemperature            string,
                speed                        string,
                Year                        string,
                Month                        string                

    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RECALLMODELOUTPUT}';

    INSERT OVERWRITE TABLE RecallModel
    select
    vin,
    model,
    city,
    outsidetemperature,
    enginetemperature,
    speed,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from CarEventsAnomaly
    where RecallLabel = '1' AND RecallProbability >= '0.60'


<span data-ttu-id="55229-410">Hello folyamat sikeres végrehajtása után a következő partíciók hozott létre a tárfiók hello "connectedcar" tárolóban hello láthatja.</span><span class="sxs-lookup"><span data-stu-id="55229-410">Once hello pipeline is executed successfully, you see hello following partitions generated in your storage account under hello "connectedcar" container.</span></span>

![DetectAnomalyPipeline kimeneti](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig24-vehicle-telematics-detect-anamoly-pipeline-output.png) 

<span data-ttu-id="55229-412">*25. ábra – DetectAnomalyPipeline kimeneti*</span><span class="sxs-lookup"><span data-stu-id="55229-412">*Figure 25 – DetectAnomalyPipeline output*</span></span>

## <a name="publish"></a><span data-ttu-id="55229-413">Közzététel</span><span class="sxs-lookup"><span data-stu-id="55229-413">Publish</span></span>

### <a name="real-time-analysis"></a><span data-ttu-id="55229-414">Valós idejű elemzés</span><span class="sxs-lookup"><span data-stu-id="55229-414">Real-time analysis</span></span>
<span data-ttu-id="55229-415">A Stream Analytics-feladat hello hello lekérdezések egyik hello események tooan kimeneti tesz közzé az Event Hubs-példány.</span><span class="sxs-lookup"><span data-stu-id="55229-415">One of hello queries in hello Stream Analytics job publishes hello events tooan output Event Hub instance.</span></span> 

![A Stream Analytics-feladat közzéteszi tooan kimeneti Event Hub-példány](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

<span data-ttu-id="55229-417">*26. ábra – Stream Analytics-feladat közzéteszi tooan kimeneti Event Hub-példány*</span><span class="sxs-lookup"><span data-stu-id="55229-417">*Figure 26 – Stream Analytics job publishes tooan output Event Hub instance*</span></span>

![Stream Analytics lekérdezési toopublish toohello kimeneti Event Hub-példány](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

<span data-ttu-id="55229-419">*27. ábra – a Stream Analytics lekérdezési toopublish toohello kimeneti Event Hub-példány*</span><span class="sxs-lookup"><span data-stu-id="55229-419">*Figure 27 – Stream Analytics query toopublish toohello output Event Hub instance*</span></span>

<span data-ttu-id="55229-420">Ez az adatfolyam események RealTimeDashboardApp hello megoldásban szereplő hello fel.</span><span class="sxs-lookup"><span data-stu-id="55229-420">This stream of events is consumed by hello RealTimeDashboardApp included in hello solution.</span></span> <span data-ttu-id="55229-421">Ez az alkalmazás kihasználja a Machine Learning kérés-válasz webszolgáltatás hello valós idejű pontozó, és közzéteszi a hello eredő adatok tooa Power BI dataset felhasználásra.</span><span class="sxs-lookup"><span data-stu-id="55229-421">This application leverages hello Machine Learning Request-Response web service for real-time scoring and publishes hello resultant data tooa Power BI dataset for consumption.</span></span> 

### <a name="batch-analysis"></a><span data-ttu-id="55229-422">Kötegelt elemzés</span><span class="sxs-lookup"><span data-stu-id="55229-422">Batch analysis</span></span>
<span data-ttu-id="55229-423">hello hello kötegelt és valós idejű feldolgozással eredményei közzétett toohello Azure SQL Database táblák felhasználásra.</span><span class="sxs-lookup"><span data-stu-id="55229-423">hello results of hello batch and real-time processing are published toohello Azure SQL Database tables for consumption.</span></span> <span data-ttu-id="55229-424">hello Azure SQL Server adatbázis és hello táblázatok jönnek létre automatikusan hello telepítési parancsfájl részeként.</span><span class="sxs-lookup"><span data-stu-id="55229-424">hello Azure SQL Server, Database, and hello tables are created automatically as part of hello setup script.</span></span> 

![Kötegelt eredmények másolása toodata adatközpont munkafolyamat](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

<span data-ttu-id="55229-426">*28. ábra – kötegfeldolgozási eredmények másolás toodata adatközpont munkafolyamat*</span><span class="sxs-lookup"><span data-stu-id="55229-426">*Figure 28 – Batch processing results copy toodata mart workflow*</span></span>

![A Stream Analytics-feladat közzéteszi toodata adatközpont](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

<span data-ttu-id="55229-428">*29. ábra – Stream Analytics-feladat közzéteszi toodata adatközpont*</span><span class="sxs-lookup"><span data-stu-id="55229-428">*Figure 29 – Stream Analytics job publishes toodata mart*</span></span>

![A Stream Analytics-feladatban Data adatközpont beállítása](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig29-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

<span data-ttu-id="55229-430">*30. ábra – a Stream Analytics-feladat beállítása adatpiac*</span><span class="sxs-lookup"><span data-stu-id="55229-430">*Figure 30 – Data mart setting in Stream Analytics job*</span></span>

## <a name="consume"></a><span data-ttu-id="55229-431">Felhasználás</span><span class="sxs-lookup"><span data-stu-id="55229-431">Consume</span></span>
<span data-ttu-id="55229-432">A Power BI ad a megoldás részletes irányítópult valós idejű adatok és a prediktív elemzés képi megjelenítések.</span><span class="sxs-lookup"><span data-stu-id="55229-432">Power BI gives this solution a rich dashboard for real-time data and predictive analytics visualizations.</span></span> 

<span data-ttu-id="55229-433">Kattintson ide a hello Power BI-jelentéseket és hello irányítópult beállításával kapcsolatos részletes információkra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="55229-433">Click here for detailed instructions on setting up hello Power BI reports and hello dashboard.</span></span> <span data-ttu-id="55229-434">hello végső irányítópult így néz ki:</span><span class="sxs-lookup"><span data-stu-id="55229-434">hello final dashboard looks like this:</span></span>

![A Power BI-irányítópulton](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-powerbi-dashboard.png)

<span data-ttu-id="55229-436">*Ábra 31 - Power BI-irányítópulton*</span><span class="sxs-lookup"><span data-stu-id="55229-436">*Figure 31 - Power BI Dashboard*</span></span>

## <a name="summary"></a><span data-ttu-id="55229-437">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="55229-437">Summary</span></span>
<span data-ttu-id="55229-438">Ez a dokumentum a Vehicle Telemetriai elemzési megoldások hello részletes Lehatolás tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="55229-438">This document contains a detailed drill-down of hello Vehicle Telemetry Analytics Solution.</span></span> <span data-ttu-id="55229-439">Ez egy lambda architektúra mintát bővíthető valós idejű és kötegelt elemzés előrejelzéseket és műveletek.</span><span class="sxs-lookup"><span data-stu-id="55229-439">This showcases a lambda architecture pattern for real-time and batch analytics with predictions and actions.</span></span> <span data-ttu-id="55229-440">Ebben a mintában vonatkozik tooa azokat a gyakran használt adatok elérési útja (valós időben) igénylő használati esetek és az elemzések cold elérési útja (kötegelt).</span><span class="sxs-lookup"><span data-stu-id="55229-440">This pattern applies tooa wide range of use cases that require hot path (real-time) and cold path (batch) analytics.</span></span> 

