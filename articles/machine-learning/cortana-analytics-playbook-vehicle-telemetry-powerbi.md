---
title: "aaaPower BI-irányítópulton vehicle állapot-és befolyásoló tényezők szokásait - Azure |} Microsoft Docs"
description: "Használja a Cortana Intelligence toogain valós idejű és prediktív elemzések hello lehetőségeit a vehicle állapotát, és ki irányítja az szokásokat."
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
ms.openlocfilehash: 0bd054d943387ecad7301236eebae22458173aba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="vehicle-telemetry-analytics-solution-template-power-bi-dashboard-setup-instructions"></a><span data-ttu-id="6f87f-103">Vehicle telemetriai analytics megoldás sablon Power BI-irányítópulton telepítési utasításokat</span><span class="sxs-lookup"><span data-stu-id="6f87f-103">Vehicle telemetry analytics solution template Power BI Dashboard setup instructions</span></span>
<span data-ttu-id="6f87f-104">Ez **menü** Ez a forgatókönyv toohello fejezetek hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6f87f-104">This **menu** links toohello chapters in this playbook.</span></span> 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

<span data-ttu-id="6f87f-105">hello Vehicle Telemetriai elemzési megoldások bővíthető hogyan car kereskedők, autó gyártók és biztosítási vállalatok kihasználhatják a Cortana Intelligence toogain valós idejű és prediktív áttekinthetik a vehicle állapotát, és ki irányítja az hello képességei szokások toodrive továbbfejlesztése hello területen felhasználói felület, a R & D és marketingkampányok.</span><span class="sxs-lookup"><span data-stu-id="6f87f-105">hello Vehicle Telemetry Analytics solution showcases how car dealerships, automobile manufacturers and insurance companies can leverage hello capabilities of Cortana Intelligence toogain real-time and predictive insights on vehicle health and driving habits toodrive improvements in hello area of customer experience, R&D and marketing campaigns.</span></span> <span data-ttu-id="6f87f-106">Ez a dokumentum konfigurálásának hello Power BI-jelentéseket és az irányítópult után hello megoldást már telepítették az előfizetésében részletes útmutatást tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6f87f-106">This document contains step by step instructions on how you can configure hello Power BI reports and dashboard once hello solution is deployed in your subscription.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="6f87f-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6f87f-107">Prerequisites</span></span>
1. <span data-ttu-id="6f87f-108">Hello telepítése [Telemetriai Analytics](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90) megoldás</span><span class="sxs-lookup"><span data-stu-id="6f87f-108">Deploy hello [Telemetry Analytics](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90) solution</span></span>  
2. [<span data-ttu-id="6f87f-109">Telepítse a Microsoft Power BI Desktopba</span><span class="sxs-lookup"><span data-stu-id="6f87f-109">Install Microsoft Power BI Desktop</span></span>](http://www.microsoft.com/download/details.aspx?id=45331)
3. <span data-ttu-id="6f87f-110">Egy [Azure-előfizetés](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6f87f-110">An [Azure subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="6f87f-111">Ha nem rendelkezik Azure-előfizetéssel, ingyenes Azure-előfizetés az első lépései</span><span class="sxs-lookup"><span data-stu-id="6f87f-111">If you don't have an Azure subscription, get started with Azure free subscription</span></span>
4. <span data-ttu-id="6f87f-112">Microsoft Power BI-fiók</span><span class="sxs-lookup"><span data-stu-id="6f87f-112">Microsoft Power BI account</span></span>

## <a name="cortana-intelligence-suite-components"></a><span data-ttu-id="6f87f-113">A Cortana Intelligence Suite összetevői</span><span class="sxs-lookup"><span data-stu-id="6f87f-113">Cortana Intelligence Suite Components</span></span>
<span data-ttu-id="6f87f-114">Hello Vehicle Telemetriai Analytics megoldássablonban részeként hello Cortana Intelligence szolgáltatások a következő előfizetéshez lettek telepítve.</span><span class="sxs-lookup"><span data-stu-id="6f87f-114">As part of hello Vehicle Telemetry Analytics solution template, hello following Cortana Intelligence services are deployed in your subscription.</span></span>

* <span data-ttu-id="6f87f-115">**Az Event Hubs** választásával dolgozhat fel a vehicle telemetriai események több millió az Azure.</span><span class="sxs-lookup"><span data-stu-id="6f87f-115">**Event Hub** for ingesting millions of vehicle telemetry events into Azure.</span></span>
* <span data-ttu-id="6f87f-116">**Stream Analytics** való vehicle egészségügyi valós idejű elemzése és továbbra is fennáll, hogy az adatok a hosszú távú tároló gazdagabb kötegelt elemzéséhez.</span><span class="sxs-lookup"><span data-stu-id="6f87f-116">**Stream Analytics** for gaining real-time insights on vehicle health and persists that data into long-term storage for richer batch analytics.</span></span>
* <span data-ttu-id="6f87f-117">**Gépi tanulás** közüli valós idejű és kötegfeldolgozási toogain prediktív elemzések.</span><span class="sxs-lookup"><span data-stu-id="6f87f-117">**Machine Learning** for anomaly detection in real-time and batch processing toogain predictive insights.</span></span>
* <span data-ttu-id="6f87f-118">**HDInsight** léptékű kihasználhatók tootransform adat</span><span class="sxs-lookup"><span data-stu-id="6f87f-118">**HDInsight** is leveraged tootransform data at scale</span></span>
* <span data-ttu-id="6f87f-119">**Adat-előállító** kezeli az orchestration, erőforrás-kezelés ütemezése és hello kötegelt folyamat figyelését.</span><span class="sxs-lookup"><span data-stu-id="6f87f-119">**Data Factory** handles orchestration, scheduling, resource management and monitoring of hello batch processing pipeline.</span></span>

<span data-ttu-id="6f87f-120">**A Power BI** Ez a megoldás részletes irányítópult ad valós idejű adatok és a prediktív elemzés képi megjelenítések.</span><span class="sxs-lookup"><span data-stu-id="6f87f-120">**Power BI** gives this solution a rich dashboard for real-time data and predictive analytics visualizations.</span></span> 

<span data-ttu-id="6f87f-121">hello megoldás két különböző adatforrásból használ: **vehicle jelek és diagnosztikai adatkészlet szimulált** és **vehicle katalógus**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-121">hello solution uses two different data sources: **Simulated vehicle signals and diagnostic dataset** and **vehicle catalog**.</span></span>

<span data-ttu-id="6f87f-122">A vehicle telematika szimulátor Ez a megoldás részét.</span><span class="sxs-lookup"><span data-stu-id="6f87f-122">A vehicle telematics simulator is included as part of this solution.</span></span> <span data-ttu-id="6f87f-123">Diagnosztikai adatok bocsát ki, és időben hello vehicle, valamint adott vezetői mintának megfelelő toohello állapotát jelzi.</span><span class="sxs-lookup"><span data-stu-id="6f87f-123">It emits diagnostic information and signals corresponding toohello state of hello vehicle and driving pattern at a given point in time.</span></span> 

<span data-ttu-id="6f87f-124">hello Vehicle katalógus hivatkozás adatkészletet tartalmazó VIN toomodel társítás</span><span class="sxs-lookup"><span data-stu-id="6f87f-124">hello Vehicle Catalog is a reference dataset containing VIN toomodel mapping</span></span>

## <a name="power-bi-dashboard-preparation"></a><span data-ttu-id="6f87f-125">A Power BI-irányítópulton előkészítése</span><span class="sxs-lookup"><span data-stu-id="6f87f-125">Power BI Dashboard Preparation</span></span>
### <a name="setup-power-bi-real-time-dashboard"></a><span data-ttu-id="6f87f-126">A telepítő a Power BI-valós idejű irányítópulton</span><span class="sxs-lookup"><span data-stu-id="6f87f-126">Setup Power BI Real-Time Dashboard</span></span>

<span data-ttu-id="6f87f-127">**Indítsa el a hello valós idejű irányítópulton alkalmazás** hello központi telepítés befejezése után hello manuális művelet utasításokat kövesse</span><span class="sxs-lookup"><span data-stu-id="6f87f-127">**Start hello real-time dashboard application** Once hello deployment is completed, you should follow hello Manual Operation Instructions</span></span>

* <span data-ttu-id="6f87f-128">Töltse le a valós idejű irányítópulton alkalmazás RealtimeDashboardApp.zip, és bontsa ki azt.</span><span class="sxs-lookup"><span data-stu-id="6f87f-128">Download real-time dashboard application RealtimeDashboardApp.zip, and unzip it.</span></span>
*  <span data-ttu-id="6f87f-129">Hello tömörítetlen mappába nyissa meg az alkalmazás konfigurációs fájl "RealtimeDashboardApp.exe.config", a név felülírandó appSettings hello értékek hello manuális művelet utasításokat, és mentse a módosításokat az Eventhub, a Blob Storage és a gépi tanulás összetevő szolgáltatás-kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="6f87f-129">In hello unzipped folder, open app config file 'RealtimeDashboardApp.exe.config', replace appSettings for Eventhub, Blob Storage, and ML service connections with hello values in hello Manual Operation Instructions, and save your changes.</span></span>
* <span data-ttu-id="6f87f-130">Futtassa az alkalmazást RealtimeDashboardApp.exe.</span><span class="sxs-lookup"><span data-stu-id="6f87f-130">Run application RealtimeDashboardApp.exe.</span></span> <span data-ttu-id="6f87f-131">A bejelentkezési ablak előugró ablak, érvényes Power bi hitelesítő adatok megadása és hello kattintson **elfogadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="6f87f-131">A login window will pop up, provide your valid PowerBI credentials and click hello **Accept** button.</span></span> <span data-ttu-id="6f87f-132">Majd hello app toorun elindul.</span><span class="sxs-lookup"><span data-stu-id="6f87f-132">Then hello app will start toorun.</span></span>

   ![Bejelentkezési tooPower BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
   
   ![A Power BI-irányítópulton engedélyek](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)

* <span data-ttu-id="6f87f-135">Bejelentkezési tooPowerBI webhelyét, és hozza létre a valós idejű irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="6f87f-135">Login tooPowerBI website, and create real-time dashboard.</span></span>

<span data-ttu-id="6f87f-136">Most készen áll a tooconfigure hello Power BI irányítópultot, amelynek valós idejű gazdag képi megjelenítések toogain és prediktív elemzések a vehicle állapotát, és ki irányítja a szokásokat.</span><span class="sxs-lookup"><span data-stu-id="6f87f-136">Now, you are ready tooconfigure hello Power BI dashboard with rich visualizations toogain real-time and predictive insights on vehicle health and driving habits.</span></span> <span data-ttu-id="6f87f-137">Körülbelül 45 percig tooan óra toocreate összes hello jelentéseket, és konfigurálja a hello irányítópult vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="6f87f-137">It takes about 45 minutes tooan hour toocreate all hello reports and configure hello dashboard.</span></span> 

### <a name="configure-power-bi-reports"></a><span data-ttu-id="6f87f-138">A Power BI-jelentések konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6f87f-138">Configure Power BI reports</span></span>
<span data-ttu-id="6f87f-139">hello valós idejű jelentéseket és hello irányítópult eltarthat 30-45 percig toocomplete kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="6f87f-139">hello real-time reports and hello dashboard take about 30-45 minutes toocomplete.</span></span> <span data-ttu-id="6f87f-140">Keresse meg a túl[http://powerbi.com](http://powerbi.com) és a bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="6f87f-140">Browse too[http://powerbi.com](http://powerbi.com) and login.</span></span>

![Bejelentkezési tooPower BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

<span data-ttu-id="6f87f-142">Új adatkészlet jön létre a Power bi-ban.</span><span class="sxs-lookup"><span data-stu-id="6f87f-142">A new dataset is generated in Power BI.</span></span> <span data-ttu-id="6f87f-143">Kattintson a hello **ConnectedCarsRealtime** adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="6f87f-143">Click hello **ConnectedCarsRealtime** dataset.</span></span>

![Be van jelölve csatlakoztatott autók valós idejű adatkészlet](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

<span data-ttu-id="6f87f-145">Mentés hello üres jelentés használatával **Ctrl + s**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-145">Save hello blank report using **Ctrl + s**.</span></span>

![Üres jelentés mentése](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

<span data-ttu-id="6f87f-147">Adja meg a jelentés neve *Vehicle Telemetriai elemzés valós idejű - jelentések*.</span><span class="sxs-lookup"><span data-stu-id="6f87f-147">Provide report name *Vehicle Telemetry Analytics Real-time - Reports*.</span></span>

![Adja meg a jelentés neve.](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a><span data-ttu-id="6f87f-149">Valós idejű jelentéseket</span><span class="sxs-lookup"><span data-stu-id="6f87f-149">Real-time reports</span></span>
<span data-ttu-id="6f87f-150">Ez a megoldás három valós idejű jelentés áll:</span><span class="sxs-lookup"><span data-stu-id="6f87f-150">There are three real-time reports in this solution:</span></span>

1. <span data-ttu-id="6f87f-151">A művelet járművekről gyűjtött</span><span class="sxs-lookup"><span data-stu-id="6f87f-151">Vehicles in operation</span></span>
2. <span data-ttu-id="6f87f-152">Karbantartási igénylő járművekről gyűjtött</span><span class="sxs-lookup"><span data-stu-id="6f87f-152">Vehicles Requiring Maintenance</span></span>
3. <span data-ttu-id="6f87f-153">Járművekről gyűjtött egészségügyi statisztikák</span><span class="sxs-lookup"><span data-stu-id="6f87f-153">Vehicles Health Statistics</span></span>

<span data-ttu-id="6f87f-154">Választhat tooconfigure összes hello három valós idejű jelentéseket vagy leállítása után fel, és folytassa a toohello következő szakaszában hello kötegelt jelentések konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6f87f-154">You can choose tooconfigure all hello three real-time reports or stop after any stage and proceed toohello next section of configuring hello batch reports.</span></span> <span data-ttu-id="6f87f-155">Javasoljuk, hogy minden toocreate hello három jelentések toovisualize hello teljes insights hello hello megoldás a valós idejű elérési út.</span><span class="sxs-lookup"><span data-stu-id="6f87f-155">We recommend you toocreate all hello three reports toovisualize hello full insights of hello real-time path of hello solution.</span></span>  

### <a name="1-vehicles-in-operation"></a><span data-ttu-id="6f87f-156">1. A művelet járművekről gyűjtött</span><span class="sxs-lookup"><span data-stu-id="6f87f-156">1. Vehicles in operation</span></span>
<span data-ttu-id="6f87f-157">Kattintson duplán a **1. oldal** és adjon neki túl "Műveletben járművekről gyűjtött"</span><span class="sxs-lookup"><span data-stu-id="6f87f-157">Double-click **Page 1** and rename it too“Vehicles in operation”</span></span>  
    <span data-ttu-id="6f87f-158">![Autók - csatlakoztatott járművekről gyűjtött műveletben](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)</span><span class="sxs-lookup"><span data-stu-id="6f87f-158">![Connected Cars - Vehicles in operation](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)</span></span>  

<span data-ttu-id="6f87f-159">Válassza ki **vin** mezőjét a **mezők** , és válassza a típusú képi megjelenítés **"Kártyás"**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-159">Select **vin** field from **Fields** and choose visualization type as **“Card”**.</span></span>  

<span data-ttu-id="6f87f-160">Kártya képi megjelenítés létrehozása ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="6f87f-160">Card visualization is created as shown in figure.</span></span>  
    ![Csatlakoztatott autók - válassza vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

<span data-ttu-id="6f87f-162">Kattintson a hello üres területre tooadd új képi megjelenítést.</span><span class="sxs-lookup"><span data-stu-id="6f87f-162">Click hello blank area tooadd new visualization.</span></span>  

<span data-ttu-id="6f87f-163">Válassza ki **Város** és **vin** mezők.</span><span class="sxs-lookup"><span data-stu-id="6f87f-163">Select **City** and **vin** from fields.</span></span> <span data-ttu-id="6f87f-164">Módosíthatja a képi megjelenítés túl**"Térkép"**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-164">Change visualization too**“Map”**.</span></span> <span data-ttu-id="6f87f-165">A csomóponthúzási **vin** az értékek területére.</span><span class="sxs-lookup"><span data-stu-id="6f87f-165">Drag **vin** in values area.</span></span> <span data-ttu-id="6f87f-166">A csomóponthúzási **Város** mezők túl**jelmagyarázat** területen.</span><span class="sxs-lookup"><span data-stu-id="6f87f-166">Drag **city** from fields too**Legend** area.</span></span>   
    <span data-ttu-id="6f87f-167">![Csatlakoztatott autók - kártya képi megjelenítés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)</span><span class="sxs-lookup"><span data-stu-id="6f87f-167">![Connected Cars - Card Visualization](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)</span></span>

<span data-ttu-id="6f87f-168">Válassza ki **formátum** szakasz **képi megjelenítések**, kattintson a **cím** , és módosítsa a hello **szöveg** túl**"járművekről gyűjtött a művelet városonként"**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-168">Select **format** section from **Visualizations**, click **Title** and change hello **Text** too**“Vehicles in operation by city”**.</span></span>  
    <span data-ttu-id="6f87f-169">![Autók - csatlakoztatott járművekről gyűjtött városonként műveletben](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)</span><span class="sxs-lookup"><span data-stu-id="6f87f-169">![Connected Cars - Vehicles in operation by city](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)</span></span>   

<span data-ttu-id="6f87f-170">Végső képi megjelenítés keres az ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="6f87f-170">Final visualization looks as shown in figure.</span></span>    
    ![Csatlakoztatott autók - végső képi megjelenítés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

<span data-ttu-id="6f87f-172">Kattintson a hello üres területre tooadd új képi megjelenítést.</span><span class="sxs-lookup"><span data-stu-id="6f87f-172">Click hello blank area tooadd new visualization.</span></span>  

<span data-ttu-id="6f87f-173">Válassza ki **Város** és **vin**, módosítsa a képi megjelenítés típusát túl**fürtözött oszlopdiagram**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-173">Select **City** and **vin**, change visualization type too**Clustered Column Chart**.</span></span> <span data-ttu-id="6f87f-174">Győződjön meg arról **Város** mezőjét **tengely terület** és **vin** a **érték-terület**</span><span class="sxs-lookup"><span data-stu-id="6f87f-174">Ensure **City** field in **Axis area** and **vin** in **Value area**</span></span>  

<span data-ttu-id="6f87f-175">Rendezés a diagram által **"Vin száma"**</span><span class="sxs-lookup"><span data-stu-id="6f87f-175">Sort chart by **“Count of vin”**</span></span>  
    <span data-ttu-id="6f87f-176">![Csatlakoztatott autók - vin száma](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)</span><span class="sxs-lookup"><span data-stu-id="6f87f-176">![Connected Cars - Count of vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)</span></span>  

<span data-ttu-id="6f87f-177">Módosítsa a diagram **cím** túl**"Műveletben városonként járművekről gyűjtött"**</span><span class="sxs-lookup"><span data-stu-id="6f87f-177">Change chart **Title** too**“Vehicles in operation by city”**</span></span>  

<span data-ttu-id="6f87f-178">Hello kattintson **formátum** területen, majd válassza ki **adatok színek**, kattintson a hello **"A"** túl**összes megjelenítése**</span><span class="sxs-lookup"><span data-stu-id="6f87f-178">Click hello **Format** section, then select **Data Colors**,  Click hello **“On”** too**Show All**</span></span>  
    <span data-ttu-id="6f87f-179">![Csatlakoztatott autók - színek adatok megjelenítése](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)</span><span class="sxs-lookup"><span data-stu-id="6f87f-179">![Connected Cars - Show all Data Colors](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)</span></span>  

<span data-ttu-id="6f87f-180">Egyes város hello színének módosítása a szín ikonra kattintva.</span><span class="sxs-lookup"><span data-stu-id="6f87f-180">Change hello color of individual city by clicking on color icon.</span></span>  
    ![Csatlakoztatott autók - módosítás színek](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

<span data-ttu-id="6f87f-182">Kattintson a hello üres területre tooadd új képi megjelenítést.</span><span class="sxs-lookup"><span data-stu-id="6f87f-182">Click hello blank area tooadd new visualization.</span></span>  

<span data-ttu-id="6f87f-183">Válassza ki **fürtözött oszlopdiagram** képi megjelenítéseket, a képi megjelenítés húzza **Város** mezőjét **tengely** területen **modell** a **Jelmagyarázat** terület és **vin** a **érték** területen.</span><span class="sxs-lookup"><span data-stu-id="6f87f-183">Select **Clustered Column Chart** visualization from visualizations, drag **city** field in **Axis** area, **Model** in **Legend** area and **vin** in **Value** area.</span></span>  
    <span data-ttu-id="6f87f-184">![Csatlakoztatott autók - csoportosított oszlopdiagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)</span><span class="sxs-lookup"><span data-stu-id="6f87f-184">![Connected Cars - Clustered Column Chart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)</span></span>  
    <span data-ttu-id="6f87f-185">![Csatlakoztatott autók - megjelenítése](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)</span><span class="sxs-lookup"><span data-stu-id="6f87f-185">![Connected Cars - Rendering](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)</span></span>

<span data-ttu-id="6f87f-186">Ezen a lapon minden képi megjelenítés átrendezését, az ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="6f87f-186">Rearrange all visualization on this page as shown in figure.</span></span>  
    ![Csatlakoztatott autók - képi megjelenítések](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

<span data-ttu-id="6f87f-188">Sikeresen konfigurálta a hello "Műveletben járművekről gyűjtött" valós idejű jelentés.</span><span class="sxs-lookup"><span data-stu-id="6f87f-188">You have successfully configured hello “Vehicles in operation” real-time report.</span></span> <span data-ttu-id="6f87f-189">Toocreate hello következő valós idejű jelentés folytassa vagy állítsa le a ide, és konfigurálja a hello irányítópult.</span><span class="sxs-lookup"><span data-stu-id="6f87f-189">You can proceed toocreate hello next real-time report or stop here and configure hello dashboard.</span></span> 

### <a name="2-vehicles-requiring-maintenance"></a><span data-ttu-id="6f87f-190">2. Karbantartási igénylő járművekről gyűjtött</span><span class="sxs-lookup"><span data-stu-id="6f87f-190">2. Vehicles Requiring Maintenance</span></span>
<span data-ttu-id="6f87f-191">Kattintson a ![Hozzáadás](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) tooadd új jelentést, nevezze át túl**"Járművekről gyűjtött igénylő karbantartási"**</span><span class="sxs-lookup"><span data-stu-id="6f87f-191">Click ![Add](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) tooadd a new report, rename it too**“Vehicles Requiring Maintenance”**</span></span>

![Csatlakoztatott autók - karbantartási igénylő járművekről gyűjtött](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

<span data-ttu-id="6f87f-193">Válassza ki **vin** mezőben, majd a képi megjelenítés típusának módosítása túl**kártya**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-193">Select **vin** field and change visualization type too**Card**.</span></span>  
    <span data-ttu-id="6f87f-194">![Csatlakoztatott autók - Vin kártya képi megjelenítés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)</span><span class="sxs-lookup"><span data-stu-id="6f87f-194">![Connected Cars - Vin Card Visualization](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)</span></span>  

<span data-ttu-id="6f87f-195">Tudunk hello adatkészletben levő "MaintenanceLabel" nevű mező.</span><span class="sxs-lookup"><span data-stu-id="6f87f-195">We have a field named “MaintenanceLabel” In hello dataset.</span></span> <span data-ttu-id="6f87f-196">Ez a mező is rendelkezik értéke "0" vagy "1"."</span><span class="sxs-lookup"><span data-stu-id="6f87f-196">This field can have a value of “0” or “1”.”</span></span> <span data-ttu-id="6f87f-197">Szerint hello Azure Machine Learning modell megoldás részeként kiosztása és hello valós idejű elérési integrálva van állítva.</span><span class="sxs-lookup"><span data-stu-id="6f87f-197">It is set by hello Azure Machine Learning model provisioned as part of solution and integrated with hello real-time path.</span></span> <span data-ttu-id="6f87f-198">hello "1" érték a vehicle igényel karbantartást.</span><span class="sxs-lookup"><span data-stu-id="6f87f-198">hello value “1” indicates a vehicle requires maintenance.</span></span> 

<span data-ttu-id="6f87f-199">tooadd egy **oldal szintjének** karbantartási megköveteli, az járművekről gyűjtött adatok jelennek meg a szűrő:</span><span class="sxs-lookup"><span data-stu-id="6f87f-199">tooadd a **Page Level** filter for showing vehicles data, which are requiring maintenance:</span></span> 

1. <span data-ttu-id="6f87f-200">Húzza hello **"MaintenanceLabel"** mezőjét a **szint Lapszűrők**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-200">Drag hello **“MaintenanceLabel”** field into **Page Level Filters**.</span></span>  
   <span data-ttu-id="6f87f-201">![Csatlakoztatott autók - oldal szintjének szűrők](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)</span><span class="sxs-lookup"><span data-stu-id="6f87f-201">![Connected Cars - Page Level Filters](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)</span></span>  
2. <span data-ttu-id="6f87f-202">Kattintson a **alapszintű szűrési** menü MaintenanceLabel lap szint szűrő alsó részén található.</span><span class="sxs-lookup"><span data-stu-id="6f87f-202">Click **Basic Filtering** menu present at bottom of MaintenanceLabel Page Level Filter.</span></span>  
   <span data-ttu-id="6f87f-203">![Csatlakoztatott autók - alapvető szűrése](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)</span><span class="sxs-lookup"><span data-stu-id="6f87f-203">![Connected Cars - Basic Filtering](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)</span></span>  
3. <span data-ttu-id="6f87f-204">Állítsa be az szűrő értékét túl**"1"**  </span><span class="sxs-lookup"><span data-stu-id="6f87f-204">Set its filter value too**“1”**  </span></span>  
   <span data-ttu-id="6f87f-205">![Csatlakoztatott autók - szűrő értéke](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)</span><span class="sxs-lookup"><span data-stu-id="6f87f-205">![Connected Cars - Filter Value](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)</span></span>  

<span data-ttu-id="6f87f-206">Kattintson a hello üres területre tooadd új képi megjelenítést.</span><span class="sxs-lookup"><span data-stu-id="6f87f-206">Click hello blank area tooadd new visualization.</span></span>  

<span data-ttu-id="6f87f-207">Válassza ki **fürtözött oszlopdiagram** a képi megjelenítések</span><span class="sxs-lookup"><span data-stu-id="6f87f-207">Select **Clustered Column Chart** from visualizations</span></span>  
<span data-ttu-id="6f87f-208">![Csatlakoztatott autók - Vind kártya képi megjelenítés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)</span><span class="sxs-lookup"><span data-stu-id="6f87f-208">![Connected Cars - Vind Card Visualization](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)</span></span>  
<span data-ttu-id="6f87f-209">![Csatlakoztatott autók - csoportosított oszlopdiagram](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)</span><span class="sxs-lookup"><span data-stu-id="6f87f-209">![Connected Cars - Clustered Column Chart](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)</span></span>

<span data-ttu-id="6f87f-210">Húzza a mező **modell** történő **tengely** területen **Vin** túl**érték** területen.</span><span class="sxs-lookup"><span data-stu-id="6f87f-210">Drag field **Model** into **Axis** area, **Vin** too**Value** area.</span></span> <span data-ttu-id="6f87f-211">Ezután szűrje a képi megjelenítés által **vin száma**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-211">Then sort visualization by **Count of vin**.</span></span>  <span data-ttu-id="6f87f-212">Módosítsa a diagram **cím** túl**"Járművekről gyűjtött karbantartási igénylő modell"**</span><span class="sxs-lookup"><span data-stu-id="6f87f-212">Change chart **Title** too**“Vehicles requiring maintenance by model”**</span></span>  

<span data-ttu-id="6f87f-213">Húzza **vin** a mezők **szín telítettségét** jelen **mezők** ![mezők](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) szakasza **aképimegjelenítés**lap</span><span class="sxs-lookup"><span data-stu-id="6f87f-213">Drag **vin** fields into **Color Saturation** present at **Fields** ![Fields](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) section of **Visualization** tab</span></span>  
<span data-ttu-id="6f87f-214">![Csatlakoztatott autók - szín telítettségét](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)</span><span class="sxs-lookup"><span data-stu-id="6f87f-214">![Connected Cars - Color Saturation](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)</span></span>  

<span data-ttu-id="6f87f-215">Változás **adatok színek** a képi megjelenítéseket **formátum** szakasz</span><span class="sxs-lookup"><span data-stu-id="6f87f-215">Change **Data Colors** in visualizations from **Format** section</span></span>  
<span data-ttu-id="6f87f-216">Módosítsa a minimális színhez: **F2C812**</span><span class="sxs-lookup"><span data-stu-id="6f87f-216">Change Minimum color to: **F2C812**</span></span>  
<span data-ttu-id="6f87f-217">A maximális színhez módosítása: **FF6300**</span><span class="sxs-lookup"><span data-stu-id="6f87f-217">Change Maximum color to: **FF6300**</span></span>  
<span data-ttu-id="6f87f-218">![Autók - szín módosítások csatlakoztatva](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)</span><span class="sxs-lookup"><span data-stu-id="6f87f-218">![Connected Cars - Color Changes](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)</span></span>  
<span data-ttu-id="6f87f-219">![Csatlakoztatott autók - új színeit](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)</span><span class="sxs-lookup"><span data-stu-id="6f87f-219">![Connected Cars - New Visualization Colors](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)</span></span>  

<span data-ttu-id="6f87f-220">Kattintson a hello üres területre tooadd új képi megjelenítést.</span><span class="sxs-lookup"><span data-stu-id="6f87f-220">Click hello blank area tooadd new visualization.</span></span>  

<span data-ttu-id="6f87f-221">Válassza ki **fürtözött oszlopdiagram** a képi megjelenítések, húzza **vin** mezőjét a **érték** terület, húzza **Város** mezőjét a **Tengely** területen.</span><span class="sxs-lookup"><span data-stu-id="6f87f-221">Select **Clustered column chart** from visualizations, drag **vin** field into **Value** area, drag **City** field into **Axis** area.</span></span> <span data-ttu-id="6f87f-222">Rendezés a diagram által **"Vin száma"**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-222">Sort chart by **“Count of vin”**.</span></span> <span data-ttu-id="6f87f-223">Módosítsa a diagram **cím** túl**"Járművekről gyűjtött városonként karbantartási megkövetelése"** </span><span class="sxs-lookup"><span data-stu-id="6f87f-223">Change chart **Title** too**“Vehicles requiring maintenance by city”** </span></span>  
<span data-ttu-id="6f87f-224">![Csatlakoztatott autók - karbantartási városonként igénylő járművekről gyűjtött](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)</span><span class="sxs-lookup"><span data-stu-id="6f87f-224">![Connected Cars - Vehicles requiring maintenance by city](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)</span></span>  

<span data-ttu-id="6f87f-225">Kattintson a hello üres területre tooadd új képi megjelenítést.</span><span class="sxs-lookup"><span data-stu-id="6f87f-225">Click hello blank area tooadd new visualization.</span></span>  

<span data-ttu-id="6f87f-226">Válassza ki **többsoros kártya** képi megjelenítéseket, a képi megjelenítés húzza **modell** és **vin** történő hello **mezők** területen.</span><span class="sxs-lookup"><span data-stu-id="6f87f-226">Select **Multi-Row Card** visualization from visualizations, drag **Model** and **vin** into hello **Fields** area.</span></span>  
<span data-ttu-id="6f87f-227">![Csatlakoztatott autók - többsoros kártya](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)</span><span class="sxs-lookup"><span data-stu-id="6f87f-227">![Connected Cars - Multi-Row Card](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)</span></span>    

<span data-ttu-id="6f87f-228">Az összes hello képi megjelenítés átrendezése, hello végső jelentés a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="6f87f-228">Rearranging all of hello visualization, hello final report looks as follows:</span></span>  
![Csatlakoztatott autók - többsoros kártya](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

<span data-ttu-id="6f87f-230">Sikeresen konfigurálta a hello "Járművekről gyűjtött igénylő karbantartás" valós idejű jelentést.</span><span class="sxs-lookup"><span data-stu-id="6f87f-230">You have successfully configured hello “Vehicles Requiring Maintenance” real-time report.</span></span> <span data-ttu-id="6f87f-231">Toocreate hello következő valós idejű jelentés folytassa vagy állítsa le a ide, és konfigurálja a hello irányítópult.</span><span class="sxs-lookup"><span data-stu-id="6f87f-231">You can proceed toocreate hello next real-time report or stop here and configure hello dashboard.</span></span> 

### <a name="3-vehicles-health-statistics"></a><span data-ttu-id="6f87f-232">3. Járművekről gyűjtött egészségügyi statisztikák</span><span class="sxs-lookup"><span data-stu-id="6f87f-232">3. Vehicles Health Statistics</span></span>
<span data-ttu-id="6f87f-233">Kattintson a ![Hozzáadás](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) tooadd új jelentést, nevezze át túl**"Járművekről gyűjtött állapotstatisztika"**</span><span class="sxs-lookup"><span data-stu-id="6f87f-233">Click ![Add](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) tooadd new report, rename it too**“Vehicles Health Statistics”**</span></span>  

<span data-ttu-id="6f87f-234">Válassza ki **mérőműszer** képi megjelenítés a képi megjelenítések, majd húzza hello **sebesség** mezőjét a **, minimális érték, maximális érték** területeket.</span><span class="sxs-lookup"><span data-stu-id="6f87f-234">Select **Gauge** visualization from visualizations, then drag hello **Speed** field into **Value, Minimum Value, Maximum Value** areas.</span></span>  
<span data-ttu-id="6f87f-235">![Csatlakoztatott autók - többsoros kártya](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)</span><span class="sxs-lookup"><span data-stu-id="6f87f-235">![Connected Cars - Multi-Row Card](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)</span></span>  

<span data-ttu-id="6f87f-236">Módosítsa a hello alapértelmezett összesítése **sebesség** a **terület érték** túl**átlagos**</span><span class="sxs-lookup"><span data-stu-id="6f87f-236">Change hello default aggregation of **speed** in **Value area** too**Average**</span></span> 

<span data-ttu-id="6f87f-237">Módosítsa a hello alapértelmezett összesítése **sebesség** a **minimális terület** túl**minimális**</span><span class="sxs-lookup"><span data-stu-id="6f87f-237">Change hello default aggregation of **speed** in **Minimum area** too**Minimum**</span></span>

<span data-ttu-id="6f87f-238">Módosítsa a hello alapértelmezett összesítése **sebesség** a **maximális terület** túl**maximális**</span><span class="sxs-lookup"><span data-stu-id="6f87f-238">Change hello default aggregation of **speed** in **Maximum area** too**Maximum**</span></span>

![Csatlakoztatott autók - többsoros kártya](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

<span data-ttu-id="6f87f-240">Nevezze át a hello **mérőműszer cím** túl**"Átlagos sebesség"**</span><span class="sxs-lookup"><span data-stu-id="6f87f-240">Rename hello **Gauge Title** too**“Average speed”**</span></span> 

![Csatlakoztatott autók - mérőműszer](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

<span data-ttu-id="6f87f-242">Kattintson a hello üres területre tooadd új képi megjelenítést.</span><span class="sxs-lookup"><span data-stu-id="6f87f-242">Click hello blank area tooadd new visualization.</span></span>  

<span data-ttu-id="6f87f-243">Hasonlóképpen adja hozzá a **mérőműszer** a **motor olaj átlagos**, **üzemanyag átlagos**, és **átlagos motor mérsékelt**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-243">Similarly add a **Gauge** for **average engine oil**, **average fuel**, and **average engine temperate**.</span></span>  

<span data-ttu-id="6f87f-244">Hello alapértelmezett összesítés minden mérőműszer megfelelően a fent ismertetett mezők módosítása **"Átlagos sebesség"** fel tudja mérni.</span><span class="sxs-lookup"><span data-stu-id="6f87f-244">Change hello default aggregation of fields in each gauge as per above steps in **“Average speed”** gauge.</span></span>

![Csatlakoztatott autók - mérőműszer](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

<span data-ttu-id="6f87f-246">Kattintson a hello üres területre tooadd új képi megjelenítést.</span><span class="sxs-lookup"><span data-stu-id="6f87f-246">Click hello blank area tooadd new visualization.</span></span>

<span data-ttu-id="6f87f-247">Válassza ki **sor- és fürtözött oszlopdiagram** a képi megjelenítések, majd húzza **Város** mezőjét a **megosztott tengely**, húzza **sebesség**, **tirepressure és engineoil mezők** történő **oszlopok értékeinek** területen túl módosítsa az összesítési típusát**átlagos**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-247">Select **Line and Clustered Column Chart** from visualizations, then drag **City** field into **Shared Axis**, drag **speed**, **tirepressure and engineoil fields** into **Column Values** area, change their aggregation type too**Average**.</span></span> 

<span data-ttu-id="6f87f-248">Húzza hello **engineTemperature** mezőjét a **sor értékek** területen hello összesítéstípusa túl módosítása**átlagos**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-248">Drag hello **engineTemperature** field into **Line Values** area, change hello  aggregation type too**Average**.</span></span> 

![Csatlakoztatott autók - képi megjelenítések mezők](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

<span data-ttu-id="6f87f-250">Változás hello diagram **cím** túl**"Átlagos sebességét, kulcsszava nyomás, motor olaj- és motor"**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-250">Change hello chart **Title** too**“Average speed, tire pressure, engine oil and engine temperature”**.</span></span>  

![Csatlakoztatott autók - képi megjelenítések mezők](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

<span data-ttu-id="6f87f-252">Kattintson a hello üres területre tooadd új képi megjelenítést.</span><span class="sxs-lookup"><span data-stu-id="6f87f-252">Click hello blank area tooadd new visualization.</span></span>

<span data-ttu-id="6f87f-253">Válassza ki **Treemap** képi megjelenítéseket, a képi megjelenítés húzza hello **modell** mezőjét a hello **csoport** területen, és húzza hello mező  **MaintenanceProbability** történő hello **értékek** területen.</span><span class="sxs-lookup"><span data-stu-id="6f87f-253">Select **Treemap** visualization from visualizations, drag hello **Model** field into hello **Group** area, and drag hello field **MaintenanceProbability** into hello **Values** area.</span></span>

<span data-ttu-id="6f87f-254">Változás hello diagram **cím** túl**"Vehicle modellek karbantartási igénylő"**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-254">Change hello chart **Title** too**“Vehicle models requiring maintenance”**.</span></span>

![Csatlakoztatott autók - diagram cím módosítása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

<span data-ttu-id="6f87f-256">Kattintson a hello üres területre tooadd új képi megjelenítést.</span><span class="sxs-lookup"><span data-stu-id="6f87f-256">Click hello blank area tooadd new visualization.</span></span>

<span data-ttu-id="6f87f-257">Válassza ki **100 százalékos halmozott sáv-** a képi megjelenítés, húzza a hello **Város** mezőjét a hello **tengely** területen, és húzza hello **MaintenanceProbability**, **RecallProbability** hello mezők **érték** területen.</span><span class="sxs-lookup"><span data-stu-id="6f87f-257">Select **100% Stacked Bar Chart** from visualization, drag hello **city** field into hello **Axis** area, and drag hello **MaintenanceProbability**, **RecallProbability** fields into hello **Value** area.</span></span>

![Csatlakoztatott autók - hozzáadása új képi megjelenítés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

<span data-ttu-id="6f87f-259">Kattintson a **formátum**, jelölje be **adatok színek**, és a set hello **MaintenanceProbability** toohello érték szín **"F2C80F"**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-259">Click **Format**, select **Data Colors**, and set hello **MaintenanceProbability** color toohello value **“F2C80F”**.</span></span>

<span data-ttu-id="6f87f-260">Változás hello **cím** hello a diagram túl**"Probability a Vehicle karbantartási & visszahívása által város"**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-260">Change hello **Title** of hello chart too**“Probability of Vehicle Maintenance & Recall by City”**.</span></span>

![Csatlakoztatott autók - hozzáadása új képi megjelenítés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

<span data-ttu-id="6f87f-262">Kattintson a hello üres területre tooadd új képi megjelenítést.</span><span class="sxs-lookup"><span data-stu-id="6f87f-262">Click hello blank area tooadd new visualization.</span></span>

<span data-ttu-id="6f87f-263">Válassza ki **területdiagram** a képi megjelenítés a képi megjelenítések, húzza a hello **modell** mezőjét a hello **tengely** területen, és húzza hello **engineOil, tirepressure, sebesség és a MaintenanceProbability** hello mezők **értékek** területen.</span><span class="sxs-lookup"><span data-stu-id="6f87f-263">Select **Area Chart** from visualization from visualizations, drag hello **Model** field into hello **Axis** area, and drag hello **engineOil, tirepressure, speed and MaintenanceProbability** fields into hello **Values** area.</span></span> <span data-ttu-id="6f87f-264">Az összesítési típusát túl módosítása**"Átlagos"**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-264">Change their aggregation type too**“Average”**.</span></span> 

![Csatlakoztatott autók - összesítési típusának módosítása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

<span data-ttu-id="6f87f-266">Módosítás hello hello diagram túl**"Átlagos motor olaj, nyomás, a sebesség és a karbantartás valószínűség megunja modell"**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-266">Change hello title of hello chart too**“Average engine oil, tire pressure, speed and maintenance probability by model”**.</span></span>

![Csatlakoztatott autók - diagram cím módosítása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

<span data-ttu-id="6f87f-268">Kattintson a hello üres területre tooadd új képi megjelenítést:</span><span class="sxs-lookup"><span data-stu-id="6f87f-268">Click hello blank area tooadd new visualization:</span></span>

1. <span data-ttu-id="6f87f-269">Válassza ki **pontdiagram diagram** képi megjelenítések a képi megjelenítés.</span><span class="sxs-lookup"><span data-stu-id="6f87f-269">Select **Scatter Chart** visualization from visualizations.</span></span>
2. <span data-ttu-id="6f87f-270">Húzza hello **modell** mezőjét a hello **részletek** és **jelmagyarázat** területen.</span><span class="sxs-lookup"><span data-stu-id="6f87f-270">Drag hello **Model** field into hello **Details** and **Legend** area.</span></span>
3. <span data-ttu-id="6f87f-271">Húzza hello **üzemanyag** mezőjét a hello **x tengely** területen hello összesítés túl módosítása**átlagos**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-271">Drag hello **fuel** field into hello **X-Axis** area, change hello aggregation too**Average**.</span></span>
4. <span data-ttu-id="6f87f-272">A csomóponthúzási **engineTemparature** be **y tengely terület**, hello összesítés túl módosítása**átlagos**</span><span class="sxs-lookup"><span data-stu-id="6f87f-272">Drag **engineTemparature** into **Y-Axis area**, change hello aggregation too**Average**</span></span>
5. <span data-ttu-id="6f87f-273">Húzza hello **vin** mezőjét a hello **mérete** területen.</span><span class="sxs-lookup"><span data-stu-id="6f87f-273">Drag hello **vin** field into hello **Size** area.</span></span>

![Csatlakoztatott autók - hozzáadása új képi megjelenítés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

<span data-ttu-id="6f87f-275">Változás hello diagram **cím** túl**"Üzemanyag átlagok, motor hőmérséklet modell"**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-275">Change hello chart **Title** too**“Averages of Fuel, Engine Temperature by Model”**.</span></span>

![Csatlakoztatott autók - diagram cím módosítása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

<span data-ttu-id="6f87f-277">hello végső jelentést fog megjelenni, alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="6f87f-277">hello final report will look like as shown below.</span></span>

![Csatlakoztatott autók végleges jelentés](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-hello-reports-toohello-real-time-dashboard"></a><span data-ttu-id="6f87f-279">PIN-kód képi megjelenítések hello jelentések toohello valós idejű irányítópulton</span><span class="sxs-lookup"><span data-stu-id="6f87f-279">Pin visualizations from hello reports toohello real-time dashboard</span></span>
<span data-ttu-id="6f87f-280">Hozzon létre egy üres irányítópult a következő tooDashboards hello plusz ikonra kattintva.</span><span class="sxs-lookup"><span data-stu-id="6f87f-280">Create a blank dashboard by clicking on hello plus icon next tooDashboards.</span></span> <span data-ttu-id="6f87f-281">"Vehicle Telemetriai elemzések irányítópultján" is neve</span><span class="sxs-lookup"><span data-stu-id="6f87f-281">You can name it “Vehicle Telemetry Analytics Dashboard”</span></span>

![Csatlakoztatott autók-irányítópult](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

<span data-ttu-id="6f87f-283">A jelentések toohello irányítópulton fent hello PIN hello képi megjelenítés.</span><span class="sxs-lookup"><span data-stu-id="6f87f-283">Pin hello visualization from hello above reports toohello dashboard.</span></span> 

![Csatlakoztatott autók-irányítópult](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

<span data-ttu-id="6f87f-285">hello irányítópult kell hasonlítania Ha hello három jelentések jönnek létre, és a megfelelő képi megjelenítések hello rögzített toohello irányítópult.</span><span class="sxs-lookup"><span data-stu-id="6f87f-285">hello dashboard should look as follows when all hello three reports are created and hello corresponding visualizations are pinned toohello dashboard.</span></span> <span data-ttu-id="6f87f-286">Ha nem hozott létre minden hello jelentést, az irányítópult sikerült jelenik meg</span><span class="sxs-lookup"><span data-stu-id="6f87f-286">If you have not created all hello reports, your dashboard could look different.</span></span> 

![Csatlakoztatott autók-irányítópult](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

<span data-ttu-id="6f87f-288">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="6f87f-288">Congratulations!</span></span> <span data-ttu-id="6f87f-289">Sikeresen létrehozta a hello valós idejű irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="6f87f-289">You have successfully created hello real-time dashboard.</span></span> <span data-ttu-id="6f87f-290">Tooexecute CarEventGenerator.exe és RealtimeDashboardApp.exe továbbra is, mivel látni működés közbeni frissítések hello irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="6f87f-290">As you continue tooexecute CarEventGenerator.exe and RealtimeDashboardApp.exe, you should see live updates on hello dashboard.</span></span> <span data-ttu-id="6f87f-291">Too15 körülbelül 10 percig toocomplete hello a következő lépéseket kell vennie.</span><span class="sxs-lookup"><span data-stu-id="6f87f-291">It should take about 10 too15 minutes toocomplete hello following steps.</span></span>

## <a name="setup-power-bi-batch-processing-dashboard"></a><span data-ttu-id="6f87f-292">A telepítő a Power BI kötegelt feldolgozásra irányítópult</span><span class="sxs-lookup"><span data-stu-id="6f87f-292">Setup Power BI batch processing dashboard</span></span>
> [!NOTE]
> <span data-ttu-id="6f87f-293">A hello end tooend kötegfeldolgozási feldolgozási sor toofinish végrehajtása (a hello hello központi telepítés sikeres befejezése) körülbelül két órát vesz igénybe, és egy év alatt érkezett létrehozott adatokat feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="6f87f-293">It takes about two hours (from hello successful completion of hello deployment) for hello end tooend batch processing pipeline toofinish execution and process a year worth of generated data.</span></span> <span data-ttu-id="6f87f-294">Várja meg, hello feldolgozása toofinish hello következő lépések végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="6f87f-294">So wait for hello processing toofinish before proceeding with hello next steps.</span></span> 
> 
> 

<span data-ttu-id="6f87f-295">**Hello Power BI designer-fájl letöltése**</span><span class="sxs-lookup"><span data-stu-id="6f87f-295">**Download hello Power BI designer file**</span></span>

* <span data-ttu-id="6f87f-296">Egy előre konfigurált Power BI designer fájl tartalmazzák a hello telepítési manuális művelet utasításokat</span><span class="sxs-lookup"><span data-stu-id="6f87f-296">A pre-configured Power BI designer file is included as part of hello deployment Manual Operation Instructions</span></span>
* <span data-ttu-id="6f87f-297">Keresse meg a 2.</span><span class="sxs-lookup"><span data-stu-id="6f87f-297">Look for 2.</span></span> <span data-ttu-id="6f87f-298">Telepítő Power bi kötegelt feldolgozásra irányítópult letöltheti a Power bi sablon hello itt nevű kötegfeldolgozási irányítópult **ConnectedCarsPbiReport.pbix**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-298">Setup PowerBI batch processing dashboard You can download hello PowerBI template for batch processing dashboard here called **ConnectedCarsPbiReport.pbix**.</span></span>
* <span data-ttu-id="6f87f-299">Mentse helyileg</span><span class="sxs-lookup"><span data-stu-id="6f87f-299">Save locally</span></span>

<span data-ttu-id="6f87f-300">**A Power BI-jelentések konfigurálása**</span><span class="sxs-lookup"><span data-stu-id="6f87f-300">**Configure Power BI reports**</span></span>

* <span data-ttu-id="6f87f-301">Nyissa meg hello Tervező fájl "**ConnectedCarsPbiReport.pbix**" Power BI Desktop használatával.</span><span class="sxs-lookup"><span data-stu-id="6f87f-301">Open hello designer file ‘**ConnectedCarsPbiReport.pbix**’ using Power BI Desktop.</span></span> <span data-ttu-id="6f87f-302">Ha nem már rendelkezik, telepíti a Power BI Desktop hello [Power BI Desktop telepítés](http://www.microsoft.com/download/details.aspx?id=45331).</span><span class="sxs-lookup"><span data-stu-id="6f87f-302">If you do not already have, install hello Power BI Desktop from [Power BI Desktop install](http://www.microsoft.com/download/details.aspx?id=45331).</span></span> 
* <span data-ttu-id="6f87f-303">Kattintson a hello **szerkesztése lekérdezések**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-303">Click hello **Edit Queries**.</span></span>

![A Power BI lekérdezés szerkesztése](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

* <span data-ttu-id="6f87f-305">Kattintson duplán a hello **forrás**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-305">Double-click hello **Source**.</span></span>

![A Power BI-forrás beállítása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

* <span data-ttu-id="6f87f-307">Frissítse a kiszolgáló kapcsolódási karakterlánc hello Azure SQL-kiszolgálót, amely hello központi telepítésének részeként lett kiépítve.</span><span class="sxs-lookup"><span data-stu-id="6f87f-307">Update Server connection string with hello Azure SQL server that got provisioned as part of hello deployment.</span></span>  <span data-ttu-id="6f87f-308">Hello manuális művelet utasításokat a hely</span><span class="sxs-lookup"><span data-stu-id="6f87f-308">Look in hello Manual Operation Instructions under</span></span> 

    4. <span data-ttu-id="6f87f-309">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="6f87f-309">Azure SQL Database</span></span>
    
    * <span data-ttu-id="6f87f-310">Server: somethingsrv.database.windows.net</span><span class="sxs-lookup"><span data-stu-id="6f87f-310">Server: somethingsrv.database.windows.net</span></span>
    * <span data-ttu-id="6f87f-311">Adatbázis: connectedcar</span><span class="sxs-lookup"><span data-stu-id="6f87f-311">Database: connectedcar</span></span>
    * <span data-ttu-id="6f87f-312">Felhasználónév: felhasználónév</span><span class="sxs-lookup"><span data-stu-id="6f87f-312">Username: username</span></span>
    * <span data-ttu-id="6f87f-313">Jelszó: Kezelheti az SQL server jelszó Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="6f87f-313">Password: You can manage your SQL server password from Azure portal</span></span>

* <span data-ttu-id="6f87f-314">Hagyja **adatbázis** , *connectedcar*.</span><span class="sxs-lookup"><span data-stu-id="6f87f-314">Leave **Database** as *connectedcar*.</span></span>

![A Power BI-adatbázis beállítása](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

* <span data-ttu-id="6f87f-316">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="6f87f-316">Click **OK**.</span></span>
* <span data-ttu-id="6f87f-317">Látni fogja **Windows hitelesítő adatok** lapon alapértelmezés szerint kiválasztva, majd azt túl**adatbázis-hitelesítő adatok** kattintva **adatbázis** jobb fülre.</span><span class="sxs-lookup"><span data-stu-id="6f87f-317">You will see **Windows credential** tab selected by default, change it too**Database credentials** by clicking on **Database** tab at right.</span></span>
* <span data-ttu-id="6f87f-318">Adja meg a hello **felhasználónév** és **jelszó** , az Azure SQL Database, a telepítés során megadott.</span><span class="sxs-lookup"><span data-stu-id="6f87f-318">Provide hello **Username** and **Password** of your Azure SQL Database that was specified during its deployment setup.</span></span>

![Adja meg az adatbázis hitelesítő adatai](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

* <span data-ttu-id="6f87f-320">Kattintson a **csatlakozás**</span><span class="sxs-lookup"><span data-stu-id="6f87f-320">Click **Connect**</span></span>
* <span data-ttu-id="6f87f-321">Ismételje meg minden egyes hello három fennmaradó lekérdezések jobb oldali ablaktáblában található hello a fenti lépéseket, és frissítse a hello az adatforrás kapcsolódási adatait.</span><span class="sxs-lookup"><span data-stu-id="6f87f-321">Repeat hello above steps for each of hello three remaining queries present at right pane, and then update hello data source connection details.</span></span>
* <span data-ttu-id="6f87f-322">Kattintson a **zárja be, és betölti**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-322">Click **Close and Load**.</span></span> <span data-ttu-id="6f87f-323">A Power BI Desktop-fájl adatkészletek csatlakoztatott tooSQL Azure adatbázistáblák.</span><span class="sxs-lookup"><span data-stu-id="6f87f-323">Power BI Desktop file datasets are connected tooSQL Azure Database tables.</span></span>
* <span data-ttu-id="6f87f-324">**Bezárás** Power BI Desktop-fájlba.</span><span class="sxs-lookup"><span data-stu-id="6f87f-324">**Close** Power BI Desktop file.</span></span>

![Zárja be a Power BI desktop](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

* <span data-ttu-id="6f87f-326">Kattintson a **mentése** toosave hello módosítások gombra.</span><span class="sxs-lookup"><span data-stu-id="6f87f-326">Click **Save** button toosave hello changes.</span></span> 

<span data-ttu-id="6f87f-327">Ezzel beállította a megfelelő toohello kötegelt feldolgozásra elérési hello megoldásban minden hello jelentést.</span><span class="sxs-lookup"><span data-stu-id="6f87f-327">You have now configured all hello reports corresponding toohello batch processing path in hello solution.</span></span> 

## <a name="upload-toopowerbicom"></a><span data-ttu-id="6f87f-328">Töltse fel a túl*powerbi.com webhelyre*</span><span class="sxs-lookup"><span data-stu-id="6f87f-328">Upload too*powerbi.com*</span></span>
1. <span data-ttu-id="6f87f-329">Keresse meg a Power BI webportál toohello http://powerbi.com és a bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="6f87f-329">Navigate toohello Power BI web portal at http://powerbi.com and login.</span></span>
2. <span data-ttu-id="6f87f-330">Kattintson a **adatok beolvasása**</span><span class="sxs-lookup"><span data-stu-id="6f87f-330">Click **Get Data**</span></span>  
3. <span data-ttu-id="6f87f-331">Hello Power BI Desktop-fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="6f87f-331">Upload hello Power BI Desktop File.</span></span>  
4. <span data-ttu-id="6f87f-332">tooupload, kattintson a **adatok beolvasása -> fájlok Get -> helyi fájl**</span><span class="sxs-lookup"><span data-stu-id="6f87f-332">tooupload, click **Get Data -> Files Get -> Local file**</span></span>  
5. <span data-ttu-id="6f87f-333">Keresse meg a toohello **"**ConnectedCarsPbiReport.pbix**"**</span><span class="sxs-lookup"><span data-stu-id="6f87f-333">Navigate toohello **“**ConnectedCarsPbiReport.pbix**”**</span></span>  
6. <span data-ttu-id="6f87f-334">Hello fájl a feltöltést követően nyílik hátsó tooyour Power BI munkaterület fogja.</span><span class="sxs-lookup"><span data-stu-id="6f87f-334">Once hello file is uploaded, you will be navigated back tooyour Power BI work space.</span></span>  

<span data-ttu-id="6f87f-335">A DataSet adatkészlet, jelentést és egy üres irányítópult meg hozható létre.</span><span class="sxs-lookup"><span data-stu-id="6f87f-335">A dataset, report and a blank dashboard will be created for you.</span></span>  

<span data-ttu-id="6f87f-336">PIN-kód diagramokat tooa új irányítópult nevű **Vehicle Telemetriai elemzések irányítópultján** a **Power BI**.</span><span class="sxs-lookup"><span data-stu-id="6f87f-336">Pin charts tooa new dashboard called **Vehicle Telemetry Analytics Dashboard** in **Power BI**.</span></span> <span data-ttu-id="6f87f-337">Kattintson a fenti létrehozott hello üres irányítópult, és navigáljon a toohello **jelentések** szakaszban kattintson hello újonnan feltöltött jelentés.</span><span class="sxs-lookup"><span data-stu-id="6f87f-337">Click hello blank dashboard created above and then navigate toohello **Reports** section click hello newly uploaded report.</span></span>  

![Vehicle Telemetriai Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 

<span data-ttu-id="6f87f-339">**Megjegyzés: hello jelentés hat lapok rendelkezik:**</span><span class="sxs-lookup"><span data-stu-id="6f87f-339">**Note hello report has six pages:**</span></span>  
<span data-ttu-id="6f87f-340">1. oldal: Vehicle sűrűség</span><span class="sxs-lookup"><span data-stu-id="6f87f-340">Page 1: Vehicle density</span></span>  
<span data-ttu-id="6f87f-341">2. lap: Valós idejű vehicle állapota</span><span class="sxs-lookup"><span data-stu-id="6f87f-341">Page 2: Real-time vehicle health</span></span>  
<span data-ttu-id="6f87f-342">3. oldal: Agresszív vezérelt járművekről gyűjtött</span><span class="sxs-lookup"><span data-stu-id="6f87f-342">Page 3: Aggressively Driven Vehicles</span></span>   
<span data-ttu-id="6f87f-343">4. lap: Visszaírt járművekről gyűjtött</span><span class="sxs-lookup"><span data-stu-id="6f87f-343">Page 4: Recalled vehicles</span></span>  
<span data-ttu-id="6f87f-344">Lap 5: Hatékony vezérelt járművekről gyűjtött üzemanyag</span><span class="sxs-lookup"><span data-stu-id="6f87f-344">Page 5: Fuel Efficiently Driven Vehicles</span></span>  
<span data-ttu-id="6f87f-345">6. lap: Contoso-embléma</span><span class="sxs-lookup"><span data-stu-id="6f87f-345">Page 6: Contoso Logo</span></span>  

![Csatlakoztatott autók Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)

<span data-ttu-id="6f87f-347">**A lap 3**, PIN-kód hello következő:</span><span class="sxs-lookup"><span data-stu-id="6f87f-347">**From Page 3**, pin hello following:</span></span>  

1. <span data-ttu-id="6f87f-348">VIN száma</span><span class="sxs-lookup"><span data-stu-id="6f87f-348">Count of VIN</span></span>  
   ![Csatlakoztatott autók Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png) 
2. <span data-ttu-id="6f87f-350">Járművekről gyűjtött agresszív vezérlik modell – diagram vízesés egyes szintjei</span><span class="sxs-lookup"><span data-stu-id="6f87f-350">Aggressively driven vehicles by model – Waterfall chart</span></span>  
   ![Vehicle Telemetria - PIN-kód diagramok 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

<span data-ttu-id="6f87f-352">**Az oldal 5**, PIN-kód hello következő:</span><span class="sxs-lookup"><span data-stu-id="6f87f-352">**From Page 5**, pin hello following:</span></span> 

1. <span data-ttu-id="6f87f-353">Vin száma</span><span class="sxs-lookup"><span data-stu-id="6f87f-353">Count of vin</span></span>    
   ![Vehicle Telemetria - 5 PIN-kód diagramok](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)  
2. <span data-ttu-id="6f87f-355">Üzemanyag-a hatékony járművekről gyűjtött modell: csoportosított oszlopdiagram</span><span class="sxs-lookup"><span data-stu-id="6f87f-355">Fuel efficient vehicles by model: Clustered column chart</span></span>  
   ![Vehicle Telemetria - PIN-kód diagramok 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

<span data-ttu-id="6f87f-357">**A lap 4**, PIN-kód hello következő:</span><span class="sxs-lookup"><span data-stu-id="6f87f-357">**From Page 4**, pin hello following:</span></span>  

1. <span data-ttu-id="6f87f-358">Vin száma</span><span class="sxs-lookup"><span data-stu-id="6f87f-358">Count of vin</span></span>  
   ![Vehicle Telemetria - PIN-kód diagramok 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 
2. <span data-ttu-id="6f87f-360">Visszaírt járművekről gyűjtött városonként, a modell: Treemap</span><span class="sxs-lookup"><span data-stu-id="6f87f-360">Recalled vehicles by city, model: Treemap</span></span>  
   ![Vehicle Telemetria - PIN-kód diagramok 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

<span data-ttu-id="6f87f-362">**A lap 6**, PIN-kód hello következő:</span><span class="sxs-lookup"><span data-stu-id="6f87f-362">**From Page 6**, pin hello following:</span></span>  

1. <span data-ttu-id="6f87f-363">Contoso motorok embléma</span><span class="sxs-lookup"><span data-stu-id="6f87f-363">Contoso Motors logo</span></span>  
   ![Vehicle Telemetria - PIN-kód diagramok 9](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

<span data-ttu-id="6f87f-365">**Hello irányítópult rendezése**</span><span class="sxs-lookup"><span data-stu-id="6f87f-365">**Organize hello dashboard**</span></span>  

1. <span data-ttu-id="6f87f-366">Keresse meg a toohello irányítópult</span><span class="sxs-lookup"><span data-stu-id="6f87f-366">Navigate toohello dashboard</span></span>
2. <span data-ttu-id="6f87f-367">Minden egyes diagram mutat, és nevezze át hello elnevezési alapján hello teljes irányítópult az alábbi képen találhatók.</span><span class="sxs-lookup"><span data-stu-id="6f87f-367">Hover over each chart and rename it based on hello naming provided in hello complete dashboard image below.</span></span> <span data-ttu-id="6f87f-368">Például az alábbi hello irányítópult toolook körül hello diagramok is helyezheti.</span><span class="sxs-lookup"><span data-stu-id="6f87f-368">Also move hello charts around toolook like hello dashboard below.</span></span>  
   ![Vehicle Telemetria - irányítópult 2 rendszerezése](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png)  
   ![Vehicle Telemetriai Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)
3. <span data-ttu-id="6f87f-371">Ha a jelen dokumentum összes hello-jelentéseket készített, hello utolsó befejezett irányítópult kell hasonlítania. ábra a következő hello.</span><span class="sxs-lookup"><span data-stu-id="6f87f-371">If you have created all hello reports as mentioned in this document, hello final completed dashboard should look like hello following figure.</span></span> 

![Vehicle Telemetria - irányítópult 2 rendszerezése](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

<span data-ttu-id="6f87f-373">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="6f87f-373">Congratulations!</span></span> <span data-ttu-id="6f87f-374">Sikeresen létrehozott hello jelentések és a valós idejű, a prediktív irányítópult toogain hello és kötegelt áttekinthetik a vehicle állapotát, és ki irányítja a szokásokat.</span><span class="sxs-lookup"><span data-stu-id="6f87f-374">You have successfully created hello reports and hello dashboard toogain real-time, predictive and batch insights on vehicle health and driving habits.</span></span>  
