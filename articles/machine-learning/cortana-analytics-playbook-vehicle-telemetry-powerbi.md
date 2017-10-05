---
title: "Power BI-irányítópulton a vehicle állapotát, és ki irányítja az szokásait - Azure |} Microsoft Docs"
description: "A Cortana Intelligence szolgáltatásai segítségével a vehicle állapotát, és ki irányítja a valós idejű és prediktív dcu szokásokat."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: aaeb29a5-4a13-4eab-bbf1-885690d86c56
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: bradsev
ms.openlocfilehash: f880aceb1657ffdfe909b73f175b9673d9ab02cd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="vehicle-telemetry-analytics-solution-template-power-bi-dashboard-setup-instructions"></a><span data-ttu-id="1feb5-103">Vehicle telemetriai analytics megoldás sablon Power BI-irányítópulton telepítési utasításokat</span><span class="sxs-lookup"><span data-stu-id="1feb5-103">Vehicle telemetry analytics solution template Power BI Dashboard setup instructions</span></span>
<span data-ttu-id="1feb5-104">Ez **menü** Ez a forgatókönyv a fejezetek mutató hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="1feb5-104">This **menu** links to the chapters in this playbook.</span></span> 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

<span data-ttu-id="1feb5-105">A Vehicle Telemetriai elemzési megoldások hogyan car kereskedők, autó gyártók és biztosítási vállalatok kihasználhatják a Cortana Intelligence ahhoz, hogy a valós idejű képességeit és a vehicle állapotát, és ki irányítja a prediktív elemzések szokásait bővíthető meghajtó továbbfejlesztése területén felhasználói felület, a R & D és marketingkampányok.</span><span class="sxs-lookup"><span data-stu-id="1feb5-105">The Vehicle Telemetry Analytics solution showcases how car dealerships, automobile manufacturers and insurance companies can leverage the capabilities of Cortana Intelligence to gain real-time and predictive insights on vehicle health and driving habits to drive improvements in the area of customer experience, R&D and marketing campaigns.</span></span> <span data-ttu-id="1feb5-106">Ez a dokumentum konfigurálásának a Power BI-jelentéseket és az irányítópult után a megoldást már telepítették az előfizetésében részletes útmutatást tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="1feb5-106">This document contains step by step instructions on how you can configure the Power BI reports and dashboard once the solution is deployed in your subscription.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="1feb5-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1feb5-107">Prerequisites</span></span>
1. <span data-ttu-id="1feb5-108">Telepítheti a [Telemetriai Analytics](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90) megoldás</span><span class="sxs-lookup"><span data-stu-id="1feb5-108">Deploy the [Telemetry Analytics](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90) solution</span></span>  
2. [<span data-ttu-id="1feb5-109">Telepítse a Microsoft Power BI Desktopba</span><span class="sxs-lookup"><span data-stu-id="1feb5-109">Install Microsoft Power BI Desktop</span></span>](http://www.microsoft.com/download/details.aspx?id=45331)
3. <span data-ttu-id="1feb5-110">Egy [Azure-előfizetés](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1feb5-110">An [Azure subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="1feb5-111">Ha nem rendelkezik Azure-előfizetéssel, ingyenes Azure-előfizetés az első lépései</span><span class="sxs-lookup"><span data-stu-id="1feb5-111">If you don't have an Azure subscription, get started with Azure free subscription</span></span>
4. <span data-ttu-id="1feb5-112">Microsoft Power BI-fiók</span><span class="sxs-lookup"><span data-stu-id="1feb5-112">Microsoft Power BI account</span></span>

## <a name="cortana-intelligence-suite-components"></a><span data-ttu-id="1feb5-113">A Cortana Intelligence Suite összetevői</span><span class="sxs-lookup"><span data-stu-id="1feb5-113">Cortana Intelligence Suite Components</span></span>
<span data-ttu-id="1feb5-114">A Vehicle Telemetriai Analytics megoldássablonban részeként a következő Cortana Intelligence szolgáltatások vannak telepítve az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="1feb5-114">As part of the Vehicle Telemetry Analytics solution template, the following Cortana Intelligence services are deployed in your subscription.</span></span>

* <span data-ttu-id="1feb5-115">**Az Event Hubs** választásával dolgozhat fel a vehicle telemetriai események több millió az Azure.</span><span class="sxs-lookup"><span data-stu-id="1feb5-115">**Event Hub** for ingesting millions of vehicle telemetry events into Azure.</span></span>
* <span data-ttu-id="1feb5-116">**Stream Analytics** való vehicle egészségügyi valós idejű elemzése és továbbra is fennáll, hogy az adatok a hosszú távú tároló gazdagabb kötegelt elemzéséhez.</span><span class="sxs-lookup"><span data-stu-id="1feb5-116">**Stream Analytics** for gaining real-time insights on vehicle health and persists that data into long-term storage for richer batch analytics.</span></span>
* <span data-ttu-id="1feb5-117">**Gépi tanulás** közüli valós idejű és prediktív dcu kötegfeldolgozási.</span><span class="sxs-lookup"><span data-stu-id="1feb5-117">**Machine Learning** for anomaly detection in real-time and batch processing to gain predictive insights.</span></span>
* <span data-ttu-id="1feb5-118">**HDInsight** rendszer elkészítéséhez használja léptékű adatok átalakítása</span><span class="sxs-lookup"><span data-stu-id="1feb5-118">**HDInsight** is leveraged to transform data at scale</span></span>
* <span data-ttu-id="1feb5-119">**Adat-előállító** vezénylési, ütemezés, erőforrás-kezelés és figyelés, a köteges feldolgozás folyamatának kezeli.</span><span class="sxs-lookup"><span data-stu-id="1feb5-119">**Data Factory** handles orchestration, scheduling, resource management and monitoring of the batch processing pipeline.</span></span>

<span data-ttu-id="1feb5-120">**A Power BI** Ez a megoldás részletes irányítópult ad valós idejű adatok és a prediktív elemzés képi megjelenítések.</span><span class="sxs-lookup"><span data-stu-id="1feb5-120">**Power BI** gives this solution a rich dashboard for real-time data and predictive analytics visualizations.</span></span> 

<span data-ttu-id="1feb5-121">A megoldás az két különböző forrásokból: **vehicle jelek és diagnosztikai adatkészlet szimulált** és **vehicle katalógus**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-121">The solution uses two different data sources: **Simulated vehicle signals and diagnostic dataset** and **vehicle catalog**.</span></span>

<span data-ttu-id="1feb5-122">A vehicle telematika szimulátor Ez a megoldás részét.</span><span class="sxs-lookup"><span data-stu-id="1feb5-122">A vehicle telematics simulator is included as part of this solution.</span></span> <span data-ttu-id="1feb5-123">Diagnosztikai adatok bocsát ki, és jelzi a vehicle állapota megfelelő, és ki irányítja a minta egy időben.</span><span class="sxs-lookup"><span data-stu-id="1feb5-123">It emits diagnostic information and signals corresponding to the state of the vehicle and driving pattern at a given point in time.</span></span> 

<span data-ttu-id="1feb5-124">A Vehicle katalógus egy hivatkozási adatkészletet tartalmazó VIN az modell-lel</span><span class="sxs-lookup"><span data-stu-id="1feb5-124">The Vehicle Catalog is a reference dataset containing VIN to model mapping</span></span>

## <a name="power-bi-dashboard-preparation"></a><span data-ttu-id="1feb5-125">A Power BI-irányítópulton előkészítése</span><span class="sxs-lookup"><span data-stu-id="1feb5-125">Power BI Dashboard Preparation</span></span>
### <a name="setup-power-bi-real-time-dashboard"></a><span data-ttu-id="1feb5-126">A telepítő a Power BI-valós idejű irányítópulton</span><span class="sxs-lookup"><span data-stu-id="1feb5-126">Setup Power BI Real-Time Dashboard</span></span>

<span data-ttu-id="1feb5-127">**A valós idejű irányítópulton alkalmazás** Ha a telepítés befejeződött, kövesse a manuális műveletet utasításokat</span><span class="sxs-lookup"><span data-stu-id="1feb5-127">**Start the real-time dashboard application** Once the deployment is completed, you should follow the Manual Operation Instructions</span></span>

* <span data-ttu-id="1feb5-128">Töltse le a valós idejű irányítópulton alkalmazás RealtimeDashboardApp.zip, és bontsa ki azt.</span><span class="sxs-lookup"><span data-stu-id="1feb5-128">Download real-time dashboard application RealtimeDashboardApp.zip, and unzip it.</span></span>
*  <span data-ttu-id="1feb5-129">A tömörítetlen mappába nyissa meg az alkalmazás konfigurációs fájljában "RealtimeDashboardApp.exe.config", a név felülírandó appSettings az értékek manuális művelet utasításokat, és mentse a módosításokat az Eventhub, a Blob Storage és a gépi tanulás szolgáltatás kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="1feb5-129">In the unzipped folder, open app config file 'RealtimeDashboardApp.exe.config', replace appSettings for Eventhub, Blob Storage, and ML service connections with the values in the Manual Operation Instructions, and save your changes.</span></span>
* <span data-ttu-id="1feb5-130">Futtassa az alkalmazást RealtimeDashboardApp.exe.</span><span class="sxs-lookup"><span data-stu-id="1feb5-130">Run application RealtimeDashboardApp.exe.</span></span> <span data-ttu-id="1feb5-131">A bejelentkezési ablak előugró ablak, érvényes Power bi hitelesítő adatok megadása és kattintson a **elfogadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="1feb5-131">A login window will pop up, provide your valid PowerBI credentials and click the **Accept** button.</span></span> <span data-ttu-id="1feb5-132">Ezt követően az alkalmazás futása elindul.</span><span class="sxs-lookup"><span data-stu-id="1feb5-132">Then the app will start to run.</span></span>

   ![Bejelentkezés a Power bi-hoz](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
   
   ![A Power BI-irányítópulton engedélyek](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)

* <span data-ttu-id="1feb5-135">Jelentkezzen be a Power bi webhelyen, és hozza létre a valós idejű irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="1feb5-135">Login to PowerBI website, and create real-time dashboard.</span></span>

<span data-ttu-id="1feb5-136">Most készen áll a Power BI-irányítópultot konfigurálása ahhoz, hogy a valós idejű gazdag megjelenítésekkel és prediktív elemzések a vehicle állapotát, és ki irányítja a szokásokat.</span><span class="sxs-lookup"><span data-stu-id="1feb5-136">Now, you are ready to configure the Power BI dashboard with rich visualizations to gain real-time and predictive insights on vehicle health and driving habits.</span></span> <span data-ttu-id="1feb5-137">A jelentések létrehozása és konfigurálása az Irányítópulton egy órával körülbelül 45 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="1feb5-137">It takes about 45 minutes to an hour to create all the reports and configure the dashboard.</span></span> 

### <a name="configure-power-bi-reports"></a><span data-ttu-id="1feb5-138">A Power BI-jelentések konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1feb5-138">Configure Power BI reports</span></span>
<span data-ttu-id="1feb5-139">A valós idejű jelentéseket és az irányítópult körülbelül 30-45 percig tarthat a befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="1feb5-139">The real-time reports and the dashboard take about 30-45 minutes to complete.</span></span> <span data-ttu-id="1feb5-140">Keresse meg a [http://powerbi.com](http://powerbi.com) és a bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="1feb5-140">Browse to [http://powerbi.com](http://powerbi.com) and login.</span></span>

![Bejelentkezés a Power bi-hoz](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

<span data-ttu-id="1feb5-142">Új adatkészlet jön létre a Power bi-ban.</span><span class="sxs-lookup"><span data-stu-id="1feb5-142">A new dataset is generated in Power BI.</span></span> <span data-ttu-id="1feb5-143">Kattintson a **ConnectedCarsRealtime** adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="1feb5-143">Click the **ConnectedCarsRealtime** dataset.</span></span>

![Be van jelölve csatlakoztatott autók valós idejű adatkészlet](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

<span data-ttu-id="1feb5-145">Mentés a üres jelentés használatával **Ctrl + s**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-145">Save the blank report using **Ctrl + s**.</span></span>

![Üres jelentés mentése](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

<span data-ttu-id="1feb5-147">Adja meg a jelentés neve *Vehicle Telemetriai elemzés valós idejű - jelentések*.</span><span class="sxs-lookup"><span data-stu-id="1feb5-147">Provide report name *Vehicle Telemetry Analytics Real-time - Reports*.</span></span>

![Adja meg a jelentés neve.](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a><span data-ttu-id="1feb5-149">Valós idejű jelentéseket</span><span class="sxs-lookup"><span data-stu-id="1feb5-149">Real-time reports</span></span>
<span data-ttu-id="1feb5-150">Ez a megoldás három valós idejű jelentés áll:</span><span class="sxs-lookup"><span data-stu-id="1feb5-150">There are three real-time reports in this solution:</span></span>

1. <span data-ttu-id="1feb5-151">A művelet járművekről gyűjtött</span><span class="sxs-lookup"><span data-stu-id="1feb5-151">Vehicles in operation</span></span>
2. <span data-ttu-id="1feb5-152">Karbantartási igénylő járművekről gyűjtött</span><span class="sxs-lookup"><span data-stu-id="1feb5-152">Vehicles Requiring Maintenance</span></span>
3. <span data-ttu-id="1feb5-153">Járművekről gyűjtött egészségügyi statisztikák</span><span class="sxs-lookup"><span data-stu-id="1feb5-153">Vehicles Health Statistics</span></span>

<span data-ttu-id="1feb5-154">Ha szeretné, konfigurálja az összes három valós idejű jelentéseket vagy leállítása után fel, és folytassa a következő szakaszban a kötegelt jelentések konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="1feb5-154">You can choose to configure all the three real-time reports or stop after any stage and proceed to the next section of configuring the batch reports.</span></span> <span data-ttu-id="1feb5-155">Azt javasoljuk, hogy a három jelentések megjelenítéséhez a teljes elemzéseket a megoldás a valós idejű elérési út létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1feb5-155">We recommend you to create all the three reports to visualize the full insights of the real-time path of the solution.</span></span>  

### <a name="1-vehicles-in-operation"></a><span data-ttu-id="1feb5-156">1. A művelet járművekről gyűjtött</span><span class="sxs-lookup"><span data-stu-id="1feb5-156">1. Vehicles in operation</span></span>
<span data-ttu-id="1feb5-157">Kattintson duplán a **1. oldal** és adjon neki "Műveletben járművekről gyűjtött"</span><span class="sxs-lookup"><span data-stu-id="1feb5-157">Double-click **Page 1** and rename it to “Vehicles in operation”</span></span>  
    <span data-ttu-id="1feb5-158">![Autók - csatlakoztatott járművekről gyűjtött műveletben](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)</span><span class="sxs-lookup"><span data-stu-id="1feb5-158">![Connected Cars - Vehicles in operation](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)</span></span>  

<span data-ttu-id="1feb5-159">Válassza ki **vin** mezőjét a **mezők** , és válassza a típusú képi megjelenítés **"Kártyás"**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-159">Select **vin** field from **Fields** and choose visualization type as **“Card”**.</span></span>  

<span data-ttu-id="1feb5-160">Kártya képi megjelenítés létrehozása ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="1feb5-160">Card visualization is created as shown in figure.</span></span>  
    ![Csatlakoztatott autók - válassza vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

<span data-ttu-id="1feb5-162">Kattintson az üres területen adja hozzá az új képi megjelenítést.</span><span class="sxs-lookup"><span data-stu-id="1feb5-162">Click the blank area to add new visualization.</span></span>  

<span data-ttu-id="1feb5-163">Válassza ki **Város** és **vin** mezők.</span><span class="sxs-lookup"><span data-stu-id="1feb5-163">Select **City** and **vin** from fields.</span></span> <span data-ttu-id="1feb5-164">Módosíthatja a képi megjelenítés **"Térkép"**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-164">Change visualization to **“Map”**.</span></span> <span data-ttu-id="1feb5-165">A csomóponthúzási **vin** az értékek területére.</span><span class="sxs-lookup"><span data-stu-id="1feb5-165">Drag **vin** in values area.</span></span> <span data-ttu-id="1feb5-166">A csomóponthúzási **Város** a mezők **jelmagyarázat** területen.</span><span class="sxs-lookup"><span data-stu-id="1feb5-166">Drag **city** from fields to **Legend** area.</span></span>   
    <span data-ttu-id="1feb5-167">![Csatlakoztatott autók - kártya képi megjelenítés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)</span><span class="sxs-lookup"><span data-stu-id="1feb5-167">![Connected Cars - Card Visualization](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)</span></span>

<span data-ttu-id="1feb5-168">Válassza ki **formátum** szakasz **képi megjelenítések**, kattintson a **cím** , és módosítsa a **szöveg** való **"járművekről gyűjtött műveletben "városonként**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-168">Select **format** section from **Visualizations**, click **Title** and change the **Text** to **“Vehicles in operation by city”**.</span></span>  
    <span data-ttu-id="1feb5-169">![Autók - csatlakoztatott járművekről gyűjtött városonként műveletben](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)</span><span class="sxs-lookup"><span data-stu-id="1feb5-169">![Connected Cars - Vehicles in operation by city](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)</span></span>   

<span data-ttu-id="1feb5-170">Végső képi megjelenítés keres az ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="1feb5-170">Final visualization looks as shown in figure.</span></span>    
    ![Csatlakoztatott autók - végső képi megjelenítés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

<span data-ttu-id="1feb5-172">Kattintson az üres területen adja hozzá az új képi megjelenítést.</span><span class="sxs-lookup"><span data-stu-id="1feb5-172">Click the blank area to add new visualization.</span></span>  

<span data-ttu-id="1feb5-173">Válassza ki **Város** és **vin**, a képi megjelenítés típus a **fürtözött oszlopdiagram**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-173">Select **City** and **vin**, change visualization type to **Clustered Column Chart**.</span></span> <span data-ttu-id="1feb5-174">Győződjön meg arról **Város** mezőjét **tengely terület** és **vin** a **érték-terület**</span><span class="sxs-lookup"><span data-stu-id="1feb5-174">Ensure **City** field in **Axis area** and **vin** in **Value area**</span></span>  

<span data-ttu-id="1feb5-175">Rendezés a diagram által **"Vin száma"**</span><span class="sxs-lookup"><span data-stu-id="1feb5-175">Sort chart by **“Count of vin”**</span></span>  
    <span data-ttu-id="1feb5-176">![Csatlakoztatott autók - vin száma](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)</span><span class="sxs-lookup"><span data-stu-id="1feb5-176">![Connected Cars - Count of vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)</span></span>  

<span data-ttu-id="1feb5-177">Módosítsa a diagram **cím** való **"Műveletben városonként járművekről gyűjtött"**</span><span class="sxs-lookup"><span data-stu-id="1feb5-177">Change chart **Title** to **“Vehicles in operation by city”**</span></span>  

<span data-ttu-id="1feb5-178">Kattintson a **formátum** területen, majd válassza ki **adatok színek**, kattintson a **"A"** való **összes megjelenítése**</span><span class="sxs-lookup"><span data-stu-id="1feb5-178">Click the **Format** section, then select **Data Colors**,  Click the **“On”** to **Show All**</span></span>  
    <span data-ttu-id="1feb5-179">![Csatlakoztatott autók - színek adatok megjelenítése](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)</span><span class="sxs-lookup"><span data-stu-id="1feb5-179">![Connected Cars - Show all Data Colors](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)</span></span>  

<span data-ttu-id="1feb5-180">Egyes város színének módosítása a szín ikonra kattintva.</span><span class="sxs-lookup"><span data-stu-id="1feb5-180">Change the color of individual city by clicking on color icon.</span></span>  
    ![Csatlakoztatott autók - módosítás színek](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

<span data-ttu-id="1feb5-182">Kattintson az üres területen adja hozzá az új képi megjelenítést.</span><span class="sxs-lookup"><span data-stu-id="1feb5-182">Click the blank area to add new visualization.</span></span>  

<span data-ttu-id="1feb5-183">Válassza ki **fürtözött oszlopdiagram** képi megjelenítéseket, a képi megjelenítés húzza **Város** mezőjét **tengely** területen **modell** a **Jelmagyarázat** terület és **vin** a **érték** területen.</span><span class="sxs-lookup"><span data-stu-id="1feb5-183">Select **Clustered Column Chart** visualization from visualizations, drag **city** field in **Axis** area, **Model** in **Legend** area and **vin** in **Value** area.</span></span>  
    <span data-ttu-id="1feb5-184">![Csatlakoztatott autók - csoportosított oszlopdiagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)</span><span class="sxs-lookup"><span data-stu-id="1feb5-184">![Connected Cars - Clustered Column Chart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)</span></span>  
    <span data-ttu-id="1feb5-185">![Csatlakoztatott autók - megjelenítése](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)</span><span class="sxs-lookup"><span data-stu-id="1feb5-185">![Connected Cars - Rendering](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)</span></span>

<span data-ttu-id="1feb5-186">Ezen a lapon minden képi megjelenítés átrendezését, az ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="1feb5-186">Rearrange all visualization on this page as shown in figure.</span></span>  
    ![Csatlakoztatott autók - képi megjelenítések](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

<span data-ttu-id="1feb5-188">Sikeresen konfigurálta a "Műveletben járművekről gyűjtött" valós idejű jelentést.</span><span class="sxs-lookup"><span data-stu-id="1feb5-188">You have successfully configured the “Vehicles in operation” real-time report.</span></span> <span data-ttu-id="1feb5-189">A következő valós idejű jelentést készít vagy leállás, és konfigurálja az irányítópult lépne.</span><span class="sxs-lookup"><span data-stu-id="1feb5-189">You can proceed to create the next real-time report or stop here and configure the dashboard.</span></span> 

### <a name="2-vehicles-requiring-maintenance"></a><span data-ttu-id="1feb5-190">2. Karbantartási igénylő járművekről gyűjtött</span><span class="sxs-lookup"><span data-stu-id="1feb5-190">2. Vehicles Requiring Maintenance</span></span>
<span data-ttu-id="1feb5-191">Kattintson a ![Hozzáadás](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) hozzáadása egy új jelentést, nevezze át **"Járművekről gyűjtött igénylő karbantartási"**</span><span class="sxs-lookup"><span data-stu-id="1feb5-191">Click ![Add](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) to add a new report, rename it to **“Vehicles Requiring Maintenance”**</span></span>

![Csatlakoztatott autók - karbantartási igénylő járművekről gyűjtött](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

<span data-ttu-id="1feb5-193">Válassza ki **vin** mezőben, majd a képi megjelenítés típus a **kártya**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-193">Select **vin** field and change visualization type to **Card**.</span></span>  
    <span data-ttu-id="1feb5-194">![Csatlakoztatott autók - Vin kártya képi megjelenítés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)</span><span class="sxs-lookup"><span data-stu-id="1feb5-194">![Connected Cars - Vin Card Visualization](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)</span></span>  

<span data-ttu-id="1feb5-195">Tudunk adatkészlet "MaintenanceLabel" nevű mező.</span><span class="sxs-lookup"><span data-stu-id="1feb5-195">We have a field named “MaintenanceLabel” In the dataset.</span></span> <span data-ttu-id="1feb5-196">Ez a mező is rendelkezik értéke "0" vagy "1"."</span><span class="sxs-lookup"><span data-stu-id="1feb5-196">This field can have a value of “0” or “1”.”</span></span> <span data-ttu-id="1feb5-197">Azt állítja be az Azure Machine Learning modell megoldás részeként üzembe, és integrálva van a valós idejű elérési útja.</span><span class="sxs-lookup"><span data-stu-id="1feb5-197">It is set by the Azure Machine Learning model provisioned as part of solution and integrated with the real-time path.</span></span> <span data-ttu-id="1feb5-198">Az "1" érték azt jelzi, hogy a vehicle igényel karbantartást.</span><span class="sxs-lookup"><span data-stu-id="1feb5-198">The value “1” indicates a vehicle requires maintenance.</span></span> 

<span data-ttu-id="1feb5-199">Hozzáadása egy **oldal szintjének** karbantartási megköveteli, az járművekről gyűjtött adatok jelennek meg a szűrő:</span><span class="sxs-lookup"><span data-stu-id="1feb5-199">To add a **Page Level** filter for showing vehicles data, which are requiring maintenance:</span></span> 

1. <span data-ttu-id="1feb5-200">Húzza a **"MaintenanceLabel"** mezőjét a **szint Lapszűrők**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-200">Drag the **“MaintenanceLabel”** field into **Page Level Filters**.</span></span>  
   <span data-ttu-id="1feb5-201">![Csatlakoztatott autók - oldal szintjének szűrők](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)</span><span class="sxs-lookup"><span data-stu-id="1feb5-201">![Connected Cars - Page Level Filters](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)</span></span>  
2. <span data-ttu-id="1feb5-202">Kattintson a **alapszintű szűrési** menü MaintenanceLabel lap szint szűrő alsó részén található.</span><span class="sxs-lookup"><span data-stu-id="1feb5-202">Click **Basic Filtering** menu present at bottom of MaintenanceLabel Page Level Filter.</span></span>  
   <span data-ttu-id="1feb5-203">![Csatlakoztatott autók - alapvető szűrése](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)</span><span class="sxs-lookup"><span data-stu-id="1feb5-203">![Connected Cars - Basic Filtering](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)</span></span>  
3. <span data-ttu-id="1feb5-204">Állítsa be az szűrő értékét **"1"**  </span><span class="sxs-lookup"><span data-stu-id="1feb5-204">Set its filter value to **“1”**  </span></span>  
   <span data-ttu-id="1feb5-205">![Csatlakoztatott autók - szűrő értéke](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)</span><span class="sxs-lookup"><span data-stu-id="1feb5-205">![Connected Cars - Filter Value](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)</span></span>  

<span data-ttu-id="1feb5-206">Kattintson az üres területen adja hozzá az új képi megjelenítést.</span><span class="sxs-lookup"><span data-stu-id="1feb5-206">Click the blank area to add new visualization.</span></span>  

<span data-ttu-id="1feb5-207">Válassza ki **fürtözött oszlopdiagram** a képi megjelenítések</span><span class="sxs-lookup"><span data-stu-id="1feb5-207">Select **Clustered Column Chart** from visualizations</span></span>  
<span data-ttu-id="1feb5-208">![Csatlakoztatott autók - Vind kártya képi megjelenítés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)</span><span class="sxs-lookup"><span data-stu-id="1feb5-208">![Connected Cars - Vind Card Visualization](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)</span></span>  
<span data-ttu-id="1feb5-209">![Csatlakoztatott autók - csoportosított oszlopdiagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)</span><span class="sxs-lookup"><span data-stu-id="1feb5-209">![Connected Cars - Clustered Column Chart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)</span></span>

<span data-ttu-id="1feb5-210">Húzza a mező **modell** történő **tengely** területen **Vin** való **érték** területen.</span><span class="sxs-lookup"><span data-stu-id="1feb5-210">Drag field **Model** into **Axis** area, **Vin** to **Value** area.</span></span> <span data-ttu-id="1feb5-211">Ezután szűrje a képi megjelenítés által **vin száma**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-211">Then sort visualization by **Count of vin**.</span></span>  <span data-ttu-id="1feb5-212">Módosítsa a diagram **cím** való **"Járművekről gyűjtött karbantartási igénylő modell"**</span><span class="sxs-lookup"><span data-stu-id="1feb5-212">Change chart **Title** to **“Vehicles requiring maintenance by model”**</span></span>  

<span data-ttu-id="1feb5-213">Húzza **vin** a mezők **szín telítettségét** jelen **mezők** ![mezők](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) szakasza **aképimegjelenítés**lap</span><span class="sxs-lookup"><span data-stu-id="1feb5-213">Drag **vin** fields into **Color Saturation** present at **Fields** ![Fields](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) section of **Visualization** tab</span></span>  
<span data-ttu-id="1feb5-214">![Csatlakoztatott autók - szín telítettségét](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)</span><span class="sxs-lookup"><span data-stu-id="1feb5-214">![Connected Cars - Color Saturation](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)</span></span>  

<span data-ttu-id="1feb5-215">Változás **adatok színek** a képi megjelenítéseket **formátum** szakasz</span><span class="sxs-lookup"><span data-stu-id="1feb5-215">Change **Data Colors** in visualizations from **Format** section</span></span>  
<span data-ttu-id="1feb5-216">Módosítsa a minimális színhez: **F2C812**</span><span class="sxs-lookup"><span data-stu-id="1feb5-216">Change Minimum color to: **F2C812**</span></span>  
<span data-ttu-id="1feb5-217">A maximális színhez módosítása: **FF6300**</span><span class="sxs-lookup"><span data-stu-id="1feb5-217">Change Maximum color to: **FF6300**</span></span>  
<span data-ttu-id="1feb5-218">![Autók - szín módosítások csatlakoztatva](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)</span><span class="sxs-lookup"><span data-stu-id="1feb5-218">![Connected Cars - Color Changes](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)</span></span>  
<span data-ttu-id="1feb5-219">![Csatlakoztatott autók - új színeit](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)</span><span class="sxs-lookup"><span data-stu-id="1feb5-219">![Connected Cars - New Visualization Colors](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)</span></span>  

<span data-ttu-id="1feb5-220">Kattintson az üres területen adja hozzá az új képi megjelenítést.</span><span class="sxs-lookup"><span data-stu-id="1feb5-220">Click the blank area to add new visualization.</span></span>  

<span data-ttu-id="1feb5-221">Válassza ki **fürtözött oszlopdiagram** a képi megjelenítések, húzza **vin** mezőjét a **érték** terület, húzza **Város** mezőjét a **Tengely** területen.</span><span class="sxs-lookup"><span data-stu-id="1feb5-221">Select **Clustered column chart** from visualizations, drag **vin** field into **Value** area, drag **City** field into **Axis** area.</span></span> <span data-ttu-id="1feb5-222">Rendezés a diagram által **"Vin száma"**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-222">Sort chart by **“Count of vin”**.</span></span> <span data-ttu-id="1feb5-223">Módosítsa a diagram **cím** való **"Járművekről gyűjtött városonként karbantartási megkövetelése"** </span><span class="sxs-lookup"><span data-stu-id="1feb5-223">Change chart **Title** to **“Vehicles requiring maintenance by city”** </span></span>  
<span data-ttu-id="1feb5-224">![Csatlakoztatott autók - karbantartási városonként igénylő járművekről gyűjtött](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)</span><span class="sxs-lookup"><span data-stu-id="1feb5-224">![Connected Cars - Vehicles requiring maintenance by city](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)</span></span>  

<span data-ttu-id="1feb5-225">Kattintson az üres területen adja hozzá az új képi megjelenítést.</span><span class="sxs-lookup"><span data-stu-id="1feb5-225">Click the blank area to add new visualization.</span></span>  

<span data-ttu-id="1feb5-226">Válassza ki **többsoros kártya** képi megjelenítéseket, a képi megjelenítés húzza **modell** és **vin** azokat a **mezők** területen.</span><span class="sxs-lookup"><span data-stu-id="1feb5-226">Select **Multi-Row Card** visualization from visualizations, drag **Model** and **vin** into the **Fields** area.</span></span>  
<span data-ttu-id="1feb5-227">![Csatlakoztatott autók - többsoros kártya](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)</span><span class="sxs-lookup"><span data-stu-id="1feb5-227">![Connected Cars - Multi-Row Card](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)</span></span>    

<span data-ttu-id="1feb5-228">A képi megjelenítés mindegyikét átrendezése, a végső jelentés a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="1feb5-228">Rearranging all of the visualization, the final report looks as follows:</span></span>  
![Csatlakoztatott autók - többsoros kártya](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

<span data-ttu-id="1feb5-230">Sikeresen konfigurálta a "Járművekről gyűjtött igénylő karbantartás" valós idejű jelentést.</span><span class="sxs-lookup"><span data-stu-id="1feb5-230">You have successfully configured the “Vehicles Requiring Maintenance” real-time report.</span></span> <span data-ttu-id="1feb5-231">A következő valós idejű jelentést készít vagy leállás, és konfigurálja az irányítópult lépne.</span><span class="sxs-lookup"><span data-stu-id="1feb5-231">You can proceed to create the next real-time report or stop here and configure the dashboard.</span></span> 

### <a name="3-vehicles-health-statistics"></a><span data-ttu-id="1feb5-232">3. Járművekről gyűjtött egészségügyi statisztikák</span><span class="sxs-lookup"><span data-stu-id="1feb5-232">3. Vehicles Health Statistics</span></span>
<span data-ttu-id="1feb5-233">Kattintson a ![Hozzáadás](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) adja hozzá az új jelentést, nevezze át, hogy **"Járművekről gyűjtött állapotstatisztika"**</span><span class="sxs-lookup"><span data-stu-id="1feb5-233">Click ![Add](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) to add new report, rename it to **“Vehicles Health Statistics”**</span></span>  

<span data-ttu-id="1feb5-234">Válassza ki **mérőműszer** képi megjelenítés a képi megjelenítések, majd húzza a **sebesség** mezőjét a **, minimális érték, maximális érték** területeket.</span><span class="sxs-lookup"><span data-stu-id="1feb5-234">Select **Gauge** visualization from visualizations, then drag the **Speed** field into **Value, Minimum Value, Maximum Value** areas.</span></span>  
<span data-ttu-id="1feb5-235">![Csatlakoztatott autók - többsoros kártya](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)</span><span class="sxs-lookup"><span data-stu-id="1feb5-235">![Connected Cars - Multi-Row Card](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)</span></span>  

<span data-ttu-id="1feb5-236">Módosítsa az alapértelmezett összesítése **sebesség** a **terület érték** való **átlagos**</span><span class="sxs-lookup"><span data-stu-id="1feb5-236">Change the default aggregation of **speed** in **Value area** to **Average**</span></span> 

<span data-ttu-id="1feb5-237">Módosítsa az alapértelmezett összesítése **sebesség** a **minimális terület** való **minimális**</span><span class="sxs-lookup"><span data-stu-id="1feb5-237">Change the default aggregation of **speed** in **Minimum area** to **Minimum**</span></span>

<span data-ttu-id="1feb5-238">Módosítsa az alapértelmezett összesítése **sebesség** a **maximális terület** való **maximális**</span><span class="sxs-lookup"><span data-stu-id="1feb5-238">Change the default aggregation of **speed** in **Maximum area** to **Maximum**</span></span>

![Csatlakoztatott autók - többsoros kártya](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

<span data-ttu-id="1feb5-240">Nevezze át a **mérőműszer cím** való **"Átlagos sebesség"**</span><span class="sxs-lookup"><span data-stu-id="1feb5-240">Rename the **Gauge Title** to **“Average speed”**</span></span> 

![Csatlakoztatott autók - mérőműszer](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

<span data-ttu-id="1feb5-242">Kattintson az üres területen adja hozzá az új képi megjelenítést.</span><span class="sxs-lookup"><span data-stu-id="1feb5-242">Click the blank area to add new visualization.</span></span>  

<span data-ttu-id="1feb5-243">Hasonlóképpen adja hozzá a **mérőműszer** a **motor olaj átlagos**, **üzemanyag átlagos**, és **átlagos motor mérsékelt**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-243">Similarly add a **Gauge** for **average engine oil**, **average fuel**, and **average engine temperate**.</span></span>  

<span data-ttu-id="1feb5-244">Módosítsa az alapértelmezett összesítés minden mérőműszer megfelelően a fent ismertetett mezők **"Átlagos sebesség"** fel tudja mérni.</span><span class="sxs-lookup"><span data-stu-id="1feb5-244">Change the default aggregation of fields in each gauge as per above steps in **“Average speed”** gauge.</span></span>

![Csatlakoztatott autók - mérőműszer](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

<span data-ttu-id="1feb5-246">Kattintson az üres területen adja hozzá az új képi megjelenítést.</span><span class="sxs-lookup"><span data-stu-id="1feb5-246">Click the blank area to add new visualization.</span></span>

<span data-ttu-id="1feb5-247">Válassza ki **sor- és fürtözött oszlopdiagram** a képi megjelenítések, majd húzza **Város** mezőjét a **megosztott tengely**, húzza **sebesség**, **tirepressure és engineoil mezők** történő **oszlopok értékeinek** terület, az összesítési típus a **átlagos**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-247">Select **Line and Clustered Column Chart** from visualizations, then drag **City** field into **Shared Axis**, drag **speed**, **tirepressure and engineoil fields** into **Column Values** area, change their aggregation type to **Average**.</span></span> 

<span data-ttu-id="1feb5-248">Húzza a **engineTemperature** mezőjét a **sor értékek** terület, összesítési típus a **átlagos**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-248">Drag the **engineTemperature** field into **Line Values** area, change the  aggregation type to **Average**.</span></span> 

![Csatlakoztatott autók - képi megjelenítések mezők](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

<span data-ttu-id="1feb5-250">A diagram **cím** való **"Átlagos sebességét, kulcsszava nyomás, motor olaj- és motor"**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-250">Change the chart **Title** to **“Average speed, tire pressure, engine oil and engine temperature”**.</span></span>  

![Csatlakoztatott autók - képi megjelenítések mezők](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

<span data-ttu-id="1feb5-252">Kattintson az üres területen adja hozzá az új képi megjelenítést.</span><span class="sxs-lookup"><span data-stu-id="1feb5-252">Click the blank area to add new visualization.</span></span>

<span data-ttu-id="1feb5-253">Válassza ki **Treemap** képi megjelenítés a képi megjelenítések, húzza a **modell** történő mezőjét a **csoport** területen, majd húzza a mező **MaintenanceProbability** azokat a **értékek** területen.</span><span class="sxs-lookup"><span data-stu-id="1feb5-253">Select **Treemap** visualization from visualizations, drag the **Model** field into the **Group** area, and drag the field **MaintenanceProbability** into the **Values** area.</span></span>

<span data-ttu-id="1feb5-254">A diagram **cím** való **"Vehicle modellek karbantartási igénylő"**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-254">Change the chart **Title** to **“Vehicle models requiring maintenance”**.</span></span>

![Csatlakoztatott autók - diagram cím módosítása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

<span data-ttu-id="1feb5-256">Kattintson az üres területen adja hozzá az új képi megjelenítést.</span><span class="sxs-lookup"><span data-stu-id="1feb5-256">Click the blank area to add new visualization.</span></span>

<span data-ttu-id="1feb5-257">Válassza ki **100 százalékos halmozott sáv-** a képi megjelenítés, húzza a **Város** történő mezőjét a **tengely** területen, majd húzza a **MaintenanceProbability**, **RecallProbability** a mezők a **érték** területen.</span><span class="sxs-lookup"><span data-stu-id="1feb5-257">Select **100% Stacked Bar Chart** from visualization, drag the **city** field into the **Axis** area, and drag the **MaintenanceProbability**, **RecallProbability** fields into the **Value** area.</span></span>

![Csatlakoztatott autók - hozzáadása új képi megjelenítés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

<span data-ttu-id="1feb5-259">Kattintson a **formátum**, jelölje be **adatok színek**, és állítsa be a **MaintenanceProbability** értékre szín **"F2C80F"**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-259">Click **Format**, select **Data Colors**, and set the **MaintenanceProbability** color to the value **“F2C80F”**.</span></span>

<span data-ttu-id="1feb5-260">Módosítsa a **cím** a diagram **"Probability a Vehicle karbantartási & visszahívása által város"**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-260">Change the **Title** of the chart to **“Probability of Vehicle Maintenance & Recall by City”**.</span></span>

![Csatlakoztatott autók - hozzáadása új képi megjelenítés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

<span data-ttu-id="1feb5-262">Kattintson az üres területen adja hozzá az új képi megjelenítést.</span><span class="sxs-lookup"><span data-stu-id="1feb5-262">Click the blank area to add new visualization.</span></span>

<span data-ttu-id="1feb5-263">Válassza ki **területdiagram** a képi megjelenítés a képi megjelenítések, húzza a **modell** történő mezőjét a **tengely** területen, majd húzza a **engineOil, tirepressure, sebesség és a MaintenanceProbability** a mezők a **értékek** területen.</span><span class="sxs-lookup"><span data-stu-id="1feb5-263">Select **Area Chart** from visualization from visualizations, drag the **Model** field into the **Axis** area, and drag the **engineOil, tirepressure, speed and MaintenanceProbability** fields into the **Values** area.</span></span> <span data-ttu-id="1feb5-264">Az összesítési típus a **"Átlagos"**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-264">Change their aggregation type to **“Average”**.</span></span> 

![Csatlakoztatott autók - összesítési típusának módosítása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

<span data-ttu-id="1feb5-266">Módosítsa a diagram címét **"Átlagos motor olaj, nyomás, a sebesség és a karbantartás valószínűség megunja modell"**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-266">Change the title of the chart to **“Average engine oil, tire pressure, speed and maintenance probability by model”**.</span></span>

![Csatlakoztatott autók - diagram cím módosítása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

<span data-ttu-id="1feb5-268">Kattintson az üres területen adja hozzá az új képi megjelenítést:</span><span class="sxs-lookup"><span data-stu-id="1feb5-268">Click the blank area to add new visualization:</span></span>

1. <span data-ttu-id="1feb5-269">Válassza ki **pontdiagram diagram** képi megjelenítések a képi megjelenítés.</span><span class="sxs-lookup"><span data-stu-id="1feb5-269">Select **Scatter Chart** visualization from visualizations.</span></span>
2. <span data-ttu-id="1feb5-270">Húzza a **modell** történő mezőjét a **részletek** és **jelmagyarázat** területen.</span><span class="sxs-lookup"><span data-stu-id="1feb5-270">Drag the **Model** field into the **Details** and **Legend** area.</span></span>
3. <span data-ttu-id="1feb5-271">Húzza a **üzemanyag** mezőben az a **x tengely** területen módosítsa a összevonása a **átlagos**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-271">Drag the **fuel** field into the **X-Axis** area, change the aggregation to **Average**.</span></span>
4. <span data-ttu-id="1feb5-272">Húzza **engineTemparature** történő **y tengely terület**, módosítsa a összevonása a **átlagos**</span><span class="sxs-lookup"><span data-stu-id="1feb5-272">Drag **engineTemparature** into **Y-Axis area**, change the aggregation to **Average**</span></span>
5. <span data-ttu-id="1feb5-273">Húzza a **vin** történő mezőjét a **mérete** területen.</span><span class="sxs-lookup"><span data-stu-id="1feb5-273">Drag the **vin** field into the **Size** area.</span></span>

![Csatlakoztatott autók - hozzáadása új képi megjelenítés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

<span data-ttu-id="1feb5-275">A diagram **cím** való **"Üzemanyag átlagok, motor hőmérséklet modell"**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-275">Change the chart **Title** to **“Averages of Fuel, Engine Temperature by Model”**.</span></span>

![Csatlakoztatott autók - diagram cím módosítása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

<span data-ttu-id="1feb5-277">A végső jelentést fog megjelenni, alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="1feb5-277">The final report will look like as shown below.</span></span>

![Csatlakoztatott autók végleges jelentés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-the-reports-to-the-real-time-dashboard"></a><span data-ttu-id="1feb5-279">PIN-kód képi megjelenítéseket készíthet a jelentésekben, a valós idejű irányítópulton</span><span class="sxs-lookup"><span data-stu-id="1feb5-279">Pin visualizations from the reports to the real-time dashboard</span></span>
<span data-ttu-id="1feb5-280">Hozzon létre egy üres irányítópult irányítópultok mellett a plusz ikonra kattintva.</span><span class="sxs-lookup"><span data-stu-id="1feb5-280">Create a blank dashboard by clicking on the plus icon next to Dashboards.</span></span> <span data-ttu-id="1feb5-281">"Vehicle Telemetriai elemzések irányítópultján" is neve</span><span class="sxs-lookup"><span data-stu-id="1feb5-281">You can name it “Vehicle Telemetry Analytics Dashboard”</span></span>

![Csatlakoztatott autók-irányítópult](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

<span data-ttu-id="1feb5-283">A képi megjelenítés a fenti jelentésekben az irányítópulton rögzítheti.</span><span class="sxs-lookup"><span data-stu-id="1feb5-283">Pin the visualization from the above reports to the dashboard.</span></span> 

![Csatlakoztatott autók-irányítópult](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

<span data-ttu-id="1feb5-285">Az irányítópult kell hasonlítania az három jelentések létrehozása és a megfelelő vizuális vannak rögzítve az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="1feb5-285">The dashboard should look as follows when all the three reports are created and the corresponding visualizations are pinned to the dashboard.</span></span> <span data-ttu-id="1feb5-286">Ha nem hozott létre a jelentéseket, az irányítópult másképp sikerült.</span><span class="sxs-lookup"><span data-stu-id="1feb5-286">If you have not created all the reports, your dashboard could look different.</span></span> 

![Csatlakoztatott autók-irányítópult](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

<span data-ttu-id="1feb5-288">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="1feb5-288">Congratulations!</span></span> <span data-ttu-id="1feb5-289">Sikeresen létrehozta a valós idejű irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="1feb5-289">You have successfully created the real-time dashboard.</span></span> <span data-ttu-id="1feb5-290">Továbbra is CarEventGenerator.exe és RealtimeDashboardApp.exe hajtható végre, mert az élő frissítések meg kell jelennie az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="1feb5-290">As you continue to execute CarEventGenerator.exe and RealtimeDashboardApp.exe, you should see live updates on the dashboard.</span></span> <span data-ttu-id="1feb5-291">Nagyjából 10 – 15 percet kell vennie az alábbi lépések elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="1feb5-291">It should take about 10 to 15 minutes to complete the following steps.</span></span>

## <a name="setup-power-bi-batch-processing-dashboard"></a><span data-ttu-id="1feb5-292">A telepítő a Power BI kötegelt feldolgozásra irányítópult</span><span class="sxs-lookup"><span data-stu-id="1feb5-292">Setup Power BI batch processing dashboard</span></span>
> [!NOTE]
> <span data-ttu-id="1feb5-293">A végpontok közötti kötegfeldolgozási adatcsatorna végrehajtásának befejeződését, és érdemes év létrehozott adatok feldolgozása körülbelül két órás (az a telepítés sikeres befejezése) vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="1feb5-293">It takes about two hours (from the successful completion of the deployment) for the end to end batch processing pipeline to finish execution and process a year worth of generated data.</span></span> <span data-ttu-id="1feb5-294">Ezért Várjon, amíg a feldolgozás a következő lépések végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="1feb5-294">So wait for the processing to finish before proceeding with the next steps.</span></span> 
> 
> 

<span data-ttu-id="1feb5-295">**A Power BI designer-fájl letöltése**</span><span class="sxs-lookup"><span data-stu-id="1feb5-295">**Download the Power BI designer file**</span></span>

* <span data-ttu-id="1feb5-296">Egy előre konfigurált Power BI designer fájl tartalmazzák a telepítési utasításokat a manuális műveletet a</span><span class="sxs-lookup"><span data-stu-id="1feb5-296">A pre-configured Power BI designer file is included as part of the deployment Manual Operation Instructions</span></span>
* <span data-ttu-id="1feb5-297">Keresse meg a 2.</span><span class="sxs-lookup"><span data-stu-id="1feb5-297">Look for 2.</span></span> <span data-ttu-id="1feb5-298">Telepítő Power bi kötegelt feldolgozásra irányítópult letöltheti a Power bi sablon itt nevű kötegfeldolgozási irányítópult **ConnectedCarsPbiReport.pbix**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-298">Setup PowerBI batch processing dashboard You can download the PowerBI template for batch processing dashboard here called **ConnectedCarsPbiReport.pbix**.</span></span>
* <span data-ttu-id="1feb5-299">Mentse helyileg</span><span class="sxs-lookup"><span data-stu-id="1feb5-299">Save locally</span></span>

<span data-ttu-id="1feb5-300">**A Power BI-jelentések konfigurálása**</span><span class="sxs-lookup"><span data-stu-id="1feb5-300">**Configure Power BI reports**</span></span>

* <span data-ttu-id="1feb5-301">Nyissa meg a Tervező fájlt "**ConnectedCarsPbiReport.pbix**" Power BI Desktop használatával.</span><span class="sxs-lookup"><span data-stu-id="1feb5-301">Open the designer file ‘**ConnectedCarsPbiReport.pbix**’ using Power BI Desktop.</span></span> <span data-ttu-id="1feb5-302">Ha még nem rendelkezik, telepítse a Power BI Desktop a [Power BI Desktop telepítés](http://www.microsoft.com/download/details.aspx?id=45331).</span><span class="sxs-lookup"><span data-stu-id="1feb5-302">If you do not already have, install the Power BI Desktop from [Power BI Desktop install](http://www.microsoft.com/download/details.aspx?id=45331).</span></span> 
* <span data-ttu-id="1feb5-303">Kattintson a **lekérdezések szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-303">Click the **Edit Queries**.</span></span>

![A Power BI lekérdezés szerkesztése](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

* <span data-ttu-id="1feb5-305">Kattintson duplán a **forrás**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-305">Double-click the **Source**.</span></span>

![A Power BI-forrás beállítása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

* <span data-ttu-id="1feb5-307">Frissítse a kiszolgáló kapcsolati karakterláncot az Azure SQL-kiszolgálót, amely a központi telepítésének részeként lett kiépítve.</span><span class="sxs-lookup"><span data-stu-id="1feb5-307">Update Server connection string with the Azure SQL server that got provisioned as part of the deployment.</span></span>  <span data-ttu-id="1feb5-308">A manuális műveletet utasításokat a hely</span><span class="sxs-lookup"><span data-stu-id="1feb5-308">Look in the Manual Operation Instructions under</span></span> 

    4. <span data-ttu-id="1feb5-309">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="1feb5-309">Azure SQL Database</span></span>
    
    * <span data-ttu-id="1feb5-310">Server: somethingsrv.database.windows.net</span><span class="sxs-lookup"><span data-stu-id="1feb5-310">Server: somethingsrv.database.windows.net</span></span>
    * <span data-ttu-id="1feb5-311">Adatbázis: connectedcar</span><span class="sxs-lookup"><span data-stu-id="1feb5-311">Database: connectedcar</span></span>
    * <span data-ttu-id="1feb5-312">Felhasználónév: felhasználónév</span><span class="sxs-lookup"><span data-stu-id="1feb5-312">Username: username</span></span>
    * <span data-ttu-id="1feb5-313">Jelszó: Kezelheti az SQL server jelszó Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1feb5-313">Password: You can manage your SQL server password from Azure portal</span></span>

* <span data-ttu-id="1feb5-314">Hagyja **adatbázis** , *connectedcar*.</span><span class="sxs-lookup"><span data-stu-id="1feb5-314">Leave **Database** as *connectedcar*.</span></span>

![A Power BI-adatbázis beállítása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

* <span data-ttu-id="1feb5-316">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="1feb5-316">Click **OK**.</span></span>
* <span data-ttu-id="1feb5-317">Látni fogja **Windows hitelesítő adatok** lapon alapértelmezés szerint kiválasztva, módosítsa úgy, hogy **adatbázis-hitelesítő adatok** kattintva **adatbázis** jobb fülre.</span><span class="sxs-lookup"><span data-stu-id="1feb5-317">You will see **Windows credential** tab selected by default, change it to **Database credentials** by clicking on **Database** tab at right.</span></span>
* <span data-ttu-id="1feb5-318">Adja meg a **felhasználónév** és **jelszó** , az Azure SQL Database, a telepítés során megadott.</span><span class="sxs-lookup"><span data-stu-id="1feb5-318">Provide the **Username** and **Password** of your Azure SQL Database that was specified during its deployment setup.</span></span>

![Adja meg az adatbázis hitelesítő adatai](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

* <span data-ttu-id="1feb5-320">Kattintson a **csatlakozás**</span><span class="sxs-lookup"><span data-stu-id="1feb5-320">Click **Connect**</span></span>
* <span data-ttu-id="1feb5-321">Ismételje meg a fenti lépéseket minden olyan jobb oldali ablaktáblában található három fennmaradó lekérdezések, és frissítse az az adatforrás kapcsolati részleteit.</span><span class="sxs-lookup"><span data-stu-id="1feb5-321">Repeat the above steps for each of the three remaining queries present at right pane, and then update the data source connection details.</span></span>
* <span data-ttu-id="1feb5-322">Kattintson a **zárja be, és betölti**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-322">Click **Close and Load**.</span></span> <span data-ttu-id="1feb5-323">A Power BI Desktop-fájl adatkészletek SQL Azure Database-táblázatok kapcsolódnak.</span><span class="sxs-lookup"><span data-stu-id="1feb5-323">Power BI Desktop file datasets are connected to SQL Azure Database tables.</span></span>
* <span data-ttu-id="1feb5-324">**Bezárás** Power BI Desktop-fájlba.</span><span class="sxs-lookup"><span data-stu-id="1feb5-324">**Close** Power BI Desktop file.</span></span>

![Zárja be a Power BI desktop](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

* <span data-ttu-id="1feb5-326">Kattintson a **mentése** gombra a módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="1feb5-326">Click **Save** button to save the changes.</span></span> 

<span data-ttu-id="1feb5-327">Ezzel beállította a kötegelt feldolgozásra elérési útjára a megoldás megfelelő jelentéseihez.</span><span class="sxs-lookup"><span data-stu-id="1feb5-327">You have now configured all the reports corresponding to the batch processing path in the solution.</span></span> 

## <a name="upload-to-powerbicom"></a><span data-ttu-id="1feb5-328">Töltse fel a *powerbi.com webhelyre*</span><span class="sxs-lookup"><span data-stu-id="1feb5-328">Upload to *powerbi.com*</span></span>
1. <span data-ttu-id="1feb5-329">Keresse meg a Power bi-ban webes portál http://powerbi.com és a bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="1feb5-329">Navigate to the Power BI web portal at http://powerbi.com and login.</span></span>
2. <span data-ttu-id="1feb5-330">Kattintson a **adatok beolvasása**</span><span class="sxs-lookup"><span data-stu-id="1feb5-330">Click **Get Data**</span></span>  
3. <span data-ttu-id="1feb5-331">A Power BI Desktop-fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="1feb5-331">Upload the Power BI Desktop File.</span></span>  
4. <span data-ttu-id="1feb5-332">Töltse fel, kattintson a **adatok beolvasása -> fájlok Get -> helyi fájl**</span><span class="sxs-lookup"><span data-stu-id="1feb5-332">To upload, click **Get Data -> Files Get -> Local file**</span></span>  
5. <span data-ttu-id="1feb5-333">Keresse meg a **"**ConnectedCarsPbiReport.pbix**"**</span><span class="sxs-lookup"><span data-stu-id="1feb5-333">Navigate to the **“**ConnectedCarsPbiReport.pbix**”**</span></span>  
6. <span data-ttu-id="1feb5-334">A fájl a feltöltést követően nyílik vissza a Power BI munkaterület a.</span><span class="sxs-lookup"><span data-stu-id="1feb5-334">Once the file is uploaded, you will be navigated back to your Power BI work space.</span></span>  

<span data-ttu-id="1feb5-335">A DataSet adatkészlet, jelentést és egy üres irányítópult meg hozható létre.</span><span class="sxs-lookup"><span data-stu-id="1feb5-335">A dataset, report and a blank dashboard will be created for you.</span></span>  

<span data-ttu-id="1feb5-336">Új irányítópult PIN-kód diagramok nevű **Vehicle Telemetriai elemzések irányítópultján** a **Power BI**.</span><span class="sxs-lookup"><span data-stu-id="1feb5-336">Pin charts to a new dashboard called **Vehicle Telemetry Analytics Dashboard** in **Power BI**.</span></span> <span data-ttu-id="1feb5-337">A fenti létrehozott üres irányítópult, és keresse meg a **jelentések** szakaszban kattintson az újonnan feltöltött jelentés.</span><span class="sxs-lookup"><span data-stu-id="1feb5-337">Click the blank dashboard created above and then navigate to the **Reports** section click the newly uploaded report.</span></span>  

![Vehicle Telemetriai Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 

<span data-ttu-id="1feb5-339">**Vegye figyelembe a jelentés hat lapok:**</span><span class="sxs-lookup"><span data-stu-id="1feb5-339">**Note the report has six pages:**</span></span>  
<span data-ttu-id="1feb5-340">1. oldal: Vehicle sűrűség</span><span class="sxs-lookup"><span data-stu-id="1feb5-340">Page 1: Vehicle density</span></span>  
<span data-ttu-id="1feb5-341">2. lap: Valós idejű vehicle állapota</span><span class="sxs-lookup"><span data-stu-id="1feb5-341">Page 2: Real-time vehicle health</span></span>  
<span data-ttu-id="1feb5-342">3. oldal: Agresszív vezérelt járművekről gyűjtött</span><span class="sxs-lookup"><span data-stu-id="1feb5-342">Page 3: Aggressively Driven Vehicles</span></span>   
<span data-ttu-id="1feb5-343">4. lap: Visszaírt járművekről gyűjtött</span><span class="sxs-lookup"><span data-stu-id="1feb5-343">Page 4: Recalled vehicles</span></span>  
<span data-ttu-id="1feb5-344">Lap 5: Hatékony vezérelt járművekről gyűjtött üzemanyag</span><span class="sxs-lookup"><span data-stu-id="1feb5-344">Page 5: Fuel Efficiently Driven Vehicles</span></span>  
<span data-ttu-id="1feb5-345">6. lap: Contoso-embléma</span><span class="sxs-lookup"><span data-stu-id="1feb5-345">Page 6: Contoso Logo</span></span>  

![Csatlakoztatott autók Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)

<span data-ttu-id="1feb5-347">**A lap 3**, PIN-kód a következő:</span><span class="sxs-lookup"><span data-stu-id="1feb5-347">**From Page 3**, pin the following:</span></span>  

1. <span data-ttu-id="1feb5-348">VIN száma</span><span class="sxs-lookup"><span data-stu-id="1feb5-348">Count of VIN</span></span>  
   ![Csatlakoztatott autók Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png) 
2. <span data-ttu-id="1feb5-350">Járművekről gyűjtött agresszív vezérlik modell – diagram vízesés egyes szintjei</span><span class="sxs-lookup"><span data-stu-id="1feb5-350">Aggressively driven vehicles by model – Waterfall chart</span></span>  
   ![Vehicle Telemetria - PIN-kód diagramok 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

<span data-ttu-id="1feb5-352">**Az oldal 5**, PIN-kód a következő:</span><span class="sxs-lookup"><span data-stu-id="1feb5-352">**From Page 5**, pin the following:</span></span> 

1. <span data-ttu-id="1feb5-353">Vin száma</span><span class="sxs-lookup"><span data-stu-id="1feb5-353">Count of vin</span></span>    
   ![Vehicle Telemetria - 5 PIN-kód diagramok](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)  
2. <span data-ttu-id="1feb5-355">Üzemanyag-a hatékony járművekről gyűjtött modell: csoportosított oszlopdiagram</span><span class="sxs-lookup"><span data-stu-id="1feb5-355">Fuel efficient vehicles by model: Clustered column chart</span></span>  
   ![Vehicle Telemetria - PIN-kód diagramok 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

<span data-ttu-id="1feb5-357">**A lap 4**, PIN-kód a következő:</span><span class="sxs-lookup"><span data-stu-id="1feb5-357">**From Page 4**, pin the following:</span></span>  

1. <span data-ttu-id="1feb5-358">Vin száma</span><span class="sxs-lookup"><span data-stu-id="1feb5-358">Count of vin</span></span>  
   ![Vehicle Telemetria - PIN-kód diagramok 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 
2. <span data-ttu-id="1feb5-360">Visszaírt járművekről gyűjtött városonként, a modell: Treemap</span><span class="sxs-lookup"><span data-stu-id="1feb5-360">Recalled vehicles by city, model: Treemap</span></span>  
   ![Vehicle Telemetria - PIN-kód diagramok 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

<span data-ttu-id="1feb5-362">**A lap 6**, PIN-kód a következő:</span><span class="sxs-lookup"><span data-stu-id="1feb5-362">**From Page 6**, pin the following:</span></span>  

1. <span data-ttu-id="1feb5-363">Contoso motorok embléma</span><span class="sxs-lookup"><span data-stu-id="1feb5-363">Contoso Motors logo</span></span>  
   ![Vehicle Telemetria - PIN-kód diagramok 9](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

<span data-ttu-id="1feb5-365">**Az irányítópult rendezése**</span><span class="sxs-lookup"><span data-stu-id="1feb5-365">**Organize the dashboard**</span></span>  

1. <span data-ttu-id="1feb5-366">Keresse meg az irányítópulton</span><span class="sxs-lookup"><span data-stu-id="1feb5-366">Navigate to the dashboard</span></span>
2. <span data-ttu-id="1feb5-367">Minden egyes diagram és az alapján, az alábbi képen teljes irányítópult megadott elnevezési átnevezése mutasson.</span><span class="sxs-lookup"><span data-stu-id="1feb5-367">Hover over each chart and rename it based on the naming provided in the complete dashboard image below.</span></span> <span data-ttu-id="1feb5-368">Is helyezheti a diagramok az alábbi irányítópult tűnik.</span><span class="sxs-lookup"><span data-stu-id="1feb5-368">Also move the charts around to look like the dashboard below.</span></span>  
   ![Vehicle Telemetria - irányítópult 2 rendszerezése](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png)  
   ![Vehicle Telemetriai Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)
3. <span data-ttu-id="1feb5-371">Ha a jelentések hozott létre, ahogy azt korábban említettük, a dokumentum, a végső befejezett irányítópult a következő ábra kell hasonlítania.</span><span class="sxs-lookup"><span data-stu-id="1feb5-371">If you have created all the reports as mentioned in this document, the final completed dashboard should look like the following figure.</span></span> 

![Vehicle Telemetria - irányítópult 2 rendszerezése](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

<span data-ttu-id="1feb5-373">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="1feb5-373">Congratulations!</span></span> <span data-ttu-id="1feb5-374">Sikeresen létrehozta a jelentések és az irányítópult valós idejű, a prediktív kapnak, és áttekinthetik a vehicle állapotát, és ki irányítja a batch-szokásokat.</span><span class="sxs-lookup"><span data-stu-id="1feb5-374">You have successfully created the reports and the dashboard to gain real-time, predictive and batch insights on vehicle health and driving habits.</span></span>  
