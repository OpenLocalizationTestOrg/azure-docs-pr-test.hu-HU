---
title: "Azure IoT Suite csatlakoztatott gyári testreszabása |} Microsoft Docs"
description: "Az előre konfigurált csatlakoztatott gyári megoldás beállításainak testreszabása leírását."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: c#
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 90a6172dbd887ecda5a9f5d9082a4e136092bc10
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="customize-how-the-connected-factory-solution-displays-data-from-your-opc-ua-servers"></a><span data-ttu-id="b5594-103">Testre szabhatja, hogy a csatlakoztatott gyári megoldás OPC EE-kiszolgálóinak adatait jeleníti meg</span><span class="sxs-lookup"><span data-stu-id="b5594-103">Customize how the connected factory solution displays data from your OPC UA servers</span></span>

## <a name="introduction"></a><span data-ttu-id="b5594-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="b5594-104">Introduction</span></span>

<span data-ttu-id="b5594-105">A csatlakoztatott gyári megoldás összesíti, és a megoldás kapcsolódó OPC EE-kiszolgálók adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b5594-105">The connected factory solution aggregates and displays data from the OPC UA servers connected to the solution.</span></span> <span data-ttu-id="b5594-106">Keresse meg, és parancsainak elküldését a OPC EE-kiszolgálók a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="b5594-106">You can browse and send commands to the OPC UA servers in your solution.</span></span> <span data-ttu-id="b5594-107">Az OPC UA architektúráról a [Csatlakoztatott gyár – GYIK](iot-suite-faq-cf.md) fejezetben talál további információt.</span><span class="sxs-lookup"><span data-stu-id="b5594-107">For more information about OPC UA, see the [Connected factory FAQ](iot-suite-faq-cf.md).</span></span>

<span data-ttu-id="b5594-108">Összesített adatai a megoldás például a teljes berendezések hatékonyságát (OEE) és fő teljesítménymutatók (KPI-k), megtekinthető az irányítópulton a gyári, a sor és az állomás szintjén.</span><span class="sxs-lookup"><span data-stu-id="b5594-108">Examples of aggregated data in the solution include the Overall Equipment Efficiency (OEE) and Key Performance Indicators (KPIs) that you can view in the dashboard at the factory, line, and station levels.</span></span> <span data-ttu-id="b5594-109">Az alábbi képernyőfelvételen látható OEE és KPI értékeit a **szerelvény** állomás, a **termelési sor 1**, a a **München** gyári:</span><span class="sxs-lookup"><span data-stu-id="b5594-109">The following screenshot shows the OEE and KPI values for the **Assembly** station, on **Production line 1**, in the **Munich** factory:</span></span>

![A megoldás OEE és a KPI értékének – példa][img-oee-kpi]

<span data-ttu-id="b5594-111">A megoldás lehetővé teszi a OPC EE-kiszolgálókról, úgynevezett adott adatelemek részletes információk megtekintése *állomások*.</span><span class="sxs-lookup"><span data-stu-id="b5594-111">The solution enables you to view detailed information from specific data items from the OPC UA servers, called *stations*.</span></span> <span data-ttu-id="b5594-112">Az alábbi képernyőfelvételen látható függvényében egy adott állomásról előállított elemek száma:</span><span class="sxs-lookup"><span data-stu-id="b5594-112">The following screenshot shows plots of the number of manufactured items from a specific station:</span></span>

![Felvétel az előállított elemek száma.][img-manufactured-items]

<span data-ttu-id="b5594-114">A diagramokon választásakor ismerheti meg az adatokat további időt adatsorozat Insights (ÁME):</span><span class="sxs-lookup"><span data-stu-id="b5594-114">If you click one of the graphs, you can explore the data further using Time Series Insights (TSI):</span></span>

![Adatokba idő adatsorozat Insights segítségével][img-tsi]

<span data-ttu-id="b5594-116">Ez a cikk ismerteti:</span><span class="sxs-lookup"><span data-stu-id="b5594-116">This article describes:</span></span>

- <span data-ttu-id="b5594-117">Hogyan adatokat szeretné elérhetővé tenni a különböző nézetek a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="b5594-117">How the data is made available to the various views in the solution.</span></span>
- <span data-ttu-id="b5594-118">Hogyan szabhatja testre a módját a megoldás adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b5594-118">How you can customize the way the solution displays the data.</span></span>

## <a name="data-sources"></a><span data-ttu-id="b5594-119">Adatforrások</span><span class="sxs-lookup"><span data-stu-id="b5594-119">Data sources</span></span>

<span data-ttu-id="b5594-120">A csatlakoztatott gyári megoldás a megoldás kapcsolódó OPC EE-kiszolgálók adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b5594-120">The connected factory solution displays data from the OPC UA servers connected to the solution.</span></span> <span data-ttu-id="b5594-121">Az alapértelmezett telepítés magában foglalja a gyári szimuláció futtató több OPC EE.</span><span class="sxs-lookup"><span data-stu-id="b5594-121">The default installation includes several OPC UA servers running a factory simulation.</span></span> <span data-ttu-id="b5594-122">A saját OPC EE-kiszolgálót is hozzáadhat, amelyek [átjárón keresztül csatlakozni] [ lnk-connect-cf] a megoldáshoz.</span><span class="sxs-lookup"><span data-stu-id="b5594-122">You can add your own OPC UA servers that [connect through a gateway][lnk-connect-cf] to your solution.</span></span>

<span data-ttu-id="b5594-123">A csatlakoztatott OPC EE-kiszolgáló küldhet a megoldás az irányítópulton adatelemek tallózással:</span><span class="sxs-lookup"><span data-stu-id="b5594-123">You can browse the data items that a connected OPC UA server can send to your solution in the dashboard:</span></span>

1. <span data-ttu-id="b5594-124">Keresse meg a **jelöljön ki egy OPC EE** megtekintése:</span><span class="sxs-lookup"><span data-stu-id="b5594-124">Navigate to the **Select an OPC UA server** view:</span></span>

    ![Navigáljon a válassza egy OPC EE-kiszolgáló megtekintése][img-select-server]

1. <span data-ttu-id="b5594-126">Válasszon kiszolgálót, majd kattintson a **Connect**.</span><span class="sxs-lookup"><span data-stu-id="b5594-126">Select a server and click **Connect**.</span></span> <span data-ttu-id="b5594-127">Kattintson a **folytatása** Ha a biztonsági figyelmeztetés jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b5594-127">Click **Proceed** when the security warning appears.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b5594-128">Ez a figyelmeztetés csak egyszer jelenik meg minden olyan kiszolgálón, és a megoldás irányítópult és a kiszolgáló közötti megbízhatósági kapcsolatot létesít.</span><span class="sxs-lookup"><span data-stu-id="b5594-128">This warning only appears once for each server and establishes a trust relationship between the solution dashboard and the server.</span></span>

1. <span data-ttu-id="b5594-129">A kiszolgáló elküldheti a megoldáshoz adatelemek tallózhatnak.</span><span class="sxs-lookup"><span data-stu-id="b5594-129">You can now browse the data items that the server can send to the solution.</span></span> <span data-ttu-id="b5594-130">A megoldáshoz küldött útjuk zöld pipa jelzi:</span><span class="sxs-lookup"><span data-stu-id="b5594-130">Items that are being sent to the solution have a green check mark:</span></span>

    ![Közzétett elemek][img-published]

1. <span data-ttu-id="b5594-132">Ha Ön egy *rendszergazda* a megoldást választhatja ki elérhetővé a csatlakoztatott gyári megoldásban adatelemet közzététele.</span><span class="sxs-lookup"><span data-stu-id="b5594-132">If you are an *Administrator* in the solution, you can choose to publish a data item to make it available in the connected factory solution.</span></span> <span data-ttu-id="b5594-133">A rendszergazdák is adatelemek értékének módosítása és hívható meg metódus a OPC EE-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="b5594-133">As an Administrator, you can also change the value of data items and call methods in the OPC UA server.</span></span>

## <a name="map-the-data"></a><span data-ttu-id="b5594-134">Az adatok leképezése</span><span class="sxs-lookup"><span data-stu-id="b5594-134">Map the data</span></span>

<span data-ttu-id="b5594-135">A csatlakoztatott gyári megoldás le, és a közzétett adatelemeket összesíti a OPC EE-kiszolgálóról a különböző nézetek a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="b5594-135">The connected factory solution maps and aggregates the published data items from the OPC UA server to the various views in the solution.</span></span> <span data-ttu-id="b5594-136">Ha a megoldás a csatlakoztatott gyári megoldás központilag telepíti az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="b5594-136">The connected factory solution deploys to your Azure account when you provision the solution.</span></span> <span data-ttu-id="b5594-137">A JSON-fájl, a Visual Studio csatlakoztatott gyári megoldásban ez leképezési információkat tárolja.</span><span class="sxs-lookup"><span data-stu-id="b5594-137">A JSON file in the Visual Studio connected factory solution stores this mapping information.</span></span> <span data-ttu-id="b5594-138">Megtekintheti és módosíthatja a Visual Studio megoldás a csatlakoztatott gyárban JSON-konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="b5594-138">You can view and modify this JSON configuration file in the connected factory Visual Studio solution.</span></span> <span data-ttu-id="b5594-139">A módosítást követően központilag telepítheti a megoldás.</span><span class="sxs-lookup"><span data-stu-id="b5594-139">You can redeploy the solution after you make a change.</span></span>

<span data-ttu-id="b5594-140">A konfigurációs fájlt is használhatja:</span><span class="sxs-lookup"><span data-stu-id="b5594-140">You can use the configuration file to:</span></span>

- <span data-ttu-id="b5594-141">A meglévő szimulált előállítók, éles sorok és állomások szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="b5594-141">Edit the existing simulated factories, production lines, and stations.</span></span>
- <span data-ttu-id="b5594-142">Valós OPC EE-kiszolgálókhoz, amelyek a megoldással az adatok leképezése.</span><span class="sxs-lookup"><span data-stu-id="b5594-142">Map data from real OPC UA servers that you connect to the solution.</span></span>

<span data-ttu-id="b5594-143">Klónozza a csatlakoztatott gyári Visual Studio megoldás egy példányát, a következő git paranccsal:</span><span class="sxs-lookup"><span data-stu-id="b5594-143">To clone a copy of the connected factory Visual Studio solution, use the following git command:</span></span>

`git clone https://github.com/Azure/azure-iot-connected-factory.git`

<span data-ttu-id="b5594-144">A fájl **ContosoTopologyDescription.json** a leképezés a OPC EE-kiszolgáló az elemeket a nézetek meghatározása a csatlakoztatott gyári megoldás irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="b5594-144">The file **ContosoTopologyDescription.json** defines the mapping from the OPC UA server data items to the views in the connected factory solution dashboard.</span></span> <span data-ttu-id="b5594-145">A konfigurációs fájlban található a **Contoso\Topology** mappájában a **WebApp** projektet a Visual Studio-megoldásban.</span><span class="sxs-lookup"><span data-stu-id="b5594-145">You can find this configuration file in the **Contoso\Topology** folder in the **WebApp** project in the Visual Studio solution.</span></span>

<span data-ttu-id="b5594-146">A JSON-fájl tartalmának gyári, termelési sor és állomás csomópontok hierarchikus vannak rendezve.</span><span class="sxs-lookup"><span data-stu-id="b5594-146">The content of the JSON file is organized as a hierarchy of factory, production line, and station nodes.</span></span> <span data-ttu-id="b5594-147">Ezt a hierarchiát a navigációs hierarchia a csatlakoztatott gyári irányítópult határozza meg.</span><span class="sxs-lookup"><span data-stu-id="b5594-147">This hierarchy defines the navigation hierarchy in the connected factory dashboard.</span></span> <span data-ttu-id="b5594-148">A hierarchia minden egyes csomóponton értékek meghatározzák, hogy az irányítópulton megjelenő információkat.</span><span class="sxs-lookup"><span data-stu-id="b5594-148">Values at each node of the hierarchy determine the information displayed in the dashboard.</span></span> <span data-ttu-id="b5594-149">Például a JSON-fájl tartalmazza a következő értékek München a beépített:</span><span class="sxs-lookup"><span data-stu-id="b5594-149">For example, the JSON file contains the following values for the Munich factory:</span></span>

```json
"Guid": "73B534AE-7C7E-4877-B826-F1C0EA339F65",
"Name": "Munich",
"Description": "Braking system",
"Location": {
    "City": "Munich",
    "Country": "Germany",
    "Latitude": 48.13641,
    "Longitude": 11.57754
},
"Image": "munich.jpg"
```

<span data-ttu-id="b5594-150">A nevét, leírását és helyen jelennek meg ebben a nézetben az irányítópulton:</span><span class="sxs-lookup"><span data-stu-id="b5594-150">The name, description, and location appear on this view in the dashboard:</span></span>

![Az irányítópult München adatok][img-munich]

<span data-ttu-id="b5594-152">Minden egyes gyári, a termelési sor és a állomás van az image tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b5594-152">Each factory, production line, and station have an image property.</span></span> <span data-ttu-id="b5594-153">Ezeket a JPEG-fájlok találhatók a **Content\img** mappájában a **WebApp** projekt.</span><span class="sxs-lookup"><span data-stu-id="b5594-153">You can find these JPEG files in the **Content\img** folder in the **WebApp** project.</span></span> <span data-ttu-id="b5594-154">A kép fájlok megjelenítése a csatlakoztatott gyári irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="b5594-154">These image files display in the connected factory dashboard.</span></span>

<span data-ttu-id="b5594-155">Minden állomás több részletes meghatározó tulajdonságok biztosítása a leképezés a OPC EE adatelemek tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b5594-155">Each station includes several detailed properties that define the mapping from the OPC UA data items.</span></span> <span data-ttu-id="b5594-156">A következő szakaszok ismertetik ezeket a tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="b5594-156">These properties are described in the following sections:</span></span>

### <a name="opcuri"></a><span data-ttu-id="b5594-157">OpcUri</span><span class="sxs-lookup"><span data-stu-id="b5594-157">OpcUri</span></span>

<span data-ttu-id="b5594-158">A **OpcUri** értéke az EE OPC-alkalmazás, amely egyedileg azonosítja a OPC EE-kiszolgáló URI.</span><span class="sxs-lookup"><span data-stu-id="b5594-158">The **OpcUri** value is the OPC UA Application URI that uniquely identifies the OPC UA server.</span></span> <span data-ttu-id="b5594-159">Például a **OpcUri** a szerelvény állomás éles sor 1 München a következőhöz hasonló, a következő érték: **urn: scada2194:ua:munich:productionline0:assemblystation**.</span><span class="sxs-lookup"><span data-stu-id="b5594-159">For example, the **OpcUri** value for the assembly station on production line 1 in Munich looks like the following: **urn:scada2194:ua:munich:productionline0:assemblystation**.</span></span>

<span data-ttu-id="b5594-160">Az URI-azonosítók a csatlakoztatott OPC EE-kiszolgálók a megoldás irányítópultjának tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="b5594-160">You can view the URIs of the connected OPC UA servers in the solution dashboard:</span></span>

![Nézet OPC EE kiszolgáló URI-azonosítók][img-server-uris]

### <a name="simulation"></a><span data-ttu-id="b5594-162">Szimuláció</span><span class="sxs-lookup"><span data-stu-id="b5594-162">Simulation</span></span>

<span data-ttu-id="b5594-163">Az információk a **szimuláció** csomópont csak a OPC EE szimuláció, amelyen a OPC EE-kiszolgálókon alapértelmezés szerint törlődnek az.</span><span class="sxs-lookup"><span data-stu-id="b5594-163">The information in the **Simulation** node is specific to the OPC UA simulation that runs in the OPC UA servers that are provisioned by default.</span></span> <span data-ttu-id="b5594-164">A valós OPC EE-kiszolgáló nem szolgál.</span><span class="sxs-lookup"><span data-stu-id="b5594-164">It is not used for a real OPC UA server.</span></span>

### <a name="kpi1-and-kpi2"></a><span data-ttu-id="b5594-165">Kpi1 és Kpi2</span><span class="sxs-lookup"><span data-stu-id="b5594-165">Kpi1 and Kpi2</span></span>

<span data-ttu-id="b5594-166">Ezek a csomópontok ismertetik, hogyan hozzájárul az adatok az állomásról a két KPI érték az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="b5594-166">These nodes describe how data from the station contributes to the two KPI values in the dashboard.</span></span> <span data-ttu-id="b5594-167">Alapértelmezett telepítésében ezen KPI értékei óránként egységek és óránként kWh.</span><span class="sxs-lookup"><span data-stu-id="b5594-167">In a default deployment, these KPI values are units per hour and kWh per hour.</span></span> <span data-ttu-id="b5594-168">A megoldás kiszámítja a KPI vales állomás szintjén, és összesíti azokat az éles sor és a gyári szintjén.</span><span class="sxs-lookup"><span data-stu-id="b5594-168">The solution calculates KPI vales at the level of a station and aggregates them at the production line and factory levels.</span></span>

<span data-ttu-id="b5594-169">Egyes KPI-KET egy minimális, maximális és célértékéhez rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b5594-169">Each KPI has a minimum, maximum, and target value.</span></span> <span data-ttu-id="b5594-170">Minden KPI értéket adhat meg riasztási műveletek végrehajtásához a csatlakoztatott gyári megoldás.</span><span class="sxs-lookup"><span data-stu-id="b5594-170">Each KPI value can also define alert actions for the connected factory solution to perform.</span></span> <span data-ttu-id="b5594-171">Az alábbi kódrészletben láthatja a KPI-definíciókat a szerelvény állomás éles sor München 1:</span><span class="sxs-lookup"><span data-stu-id="b5594-171">The following snippet shows the KPI definitions for the assembly station on production line 1 in Munich:</span></span>

```json
"Kpi1": {
  "Minimum": 150,
  "Target": 300,
  "Maximum": 600
},
"Kpi2": {
  "Minimum": 50,
  "Target": 100,
  "Maximum": 200,
  "MinimumAlertActions": [
    {
      "Type": "None"
    }
  ]
}
```

<span data-ttu-id="b5594-172">Az alábbi képernyőfelvételen a KPI adatainak megjelenítése az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="b5594-172">The following screenshot shows the KPI data in the dashboard.</span></span>

![Az irányítópult KPI-információk][lnk-kpi]

### <a name="opcnodes"></a><span data-ttu-id="b5594-174">OpcNodes</span><span class="sxs-lookup"><span data-stu-id="b5594-174">OpcNodes</span></span>

<span data-ttu-id="b5594-175">A **OpcNodes** csomópontok azonosíthatja a közzétett adatelemeket a OPC EE-kiszolgálóról, és adja meg, hogyan kell feldolgozni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="b5594-175">The **OpcNodes** nodes identify the published data items from the OPC UA server and specify how to process that data.</span></span>

<span data-ttu-id="b5594-176">A **NodeId** érték azonosítja az adott OPC EE NodeID a OPC EE-kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="b5594-176">The **NodeId** value identifies the specific OPC UA NodeID from the OPC UA server.</span></span> <span data-ttu-id="b5594-177">A szerelvény állomás gyártási sor München 1 első csomópontjának értéke **ns = 2, i = 385**.</span><span class="sxs-lookup"><span data-stu-id="b5594-177">The first node in the assembly station for production line 1 in Munich has a value **ns=2;i=385**.</span></span> <span data-ttu-id="b5594-178">A **NodeId** érték határozza meg, az elem olvasni a OPC EE-kiszolgálóról, és a **melynek** biztosít egy felhasználóbarát nevet a az irányítópulton az adatok.</span><span class="sxs-lookup"><span data-stu-id="b5594-178">A **NodeId** value specifies the data item to read from the OPC UA server, and the **SymbolicName** provides a user-friendly name to use in the dashboard for that data.</span></span>

<span data-ttu-id="b5594-179">Más csomópontokra társított értékek a következő táblázat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="b5594-179">Other values associated with each node are summarized in the following table:</span></span>

| <span data-ttu-id="b5594-180">Érték</span><span class="sxs-lookup"><span data-stu-id="b5594-180">Value</span></span> | <span data-ttu-id="b5594-181">Leírás</span><span class="sxs-lookup"><span data-stu-id="b5594-181">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="b5594-182">Találati pontosság</span><span class="sxs-lookup"><span data-stu-id="b5594-182">Relevance</span></span>  | <span data-ttu-id="b5594-183">A KPI-t és OEE értékek hozzájárul az adatok.</span><span class="sxs-lookup"><span data-stu-id="b5594-183">The KPI and OEE values this data contributes to.</span></span> |
| <span data-ttu-id="b5594-184">Műveleti kód</span><span class="sxs-lookup"><span data-stu-id="b5594-184">OpCode</span></span>     | <span data-ttu-id="b5594-185">Hogyan összesíti az adatokat.</span><span class="sxs-lookup"><span data-stu-id="b5594-185">How the data is aggregated.</span></span> |
| <span data-ttu-id="b5594-186">egység</span><span class="sxs-lookup"><span data-stu-id="b5594-186">Units</span></span>      | <span data-ttu-id="b5594-187">Az irányítópult használatához egység.</span><span class="sxs-lookup"><span data-stu-id="b5594-187">The units to use in the dashboard.</span></span>  |
| <span data-ttu-id="b5594-188">Látható</span><span class="sxs-lookup"><span data-stu-id="b5594-188">Visible</span></span>    | <span data-ttu-id="b5594-189">Hogy megjelenjenek-e ezt az értéket az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="b5594-189">Whether to display this value in the dashboard.</span></span> <span data-ttu-id="b5594-190">Egyes értékek számítások szerepel, de nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b5594-190">Some values are used in calculations but not displayed.</span></span>  |
| <span data-ttu-id="b5594-191">Maximális</span><span class="sxs-lookup"><span data-stu-id="b5594-191">Maximum</span></span>    | <span data-ttu-id="b5594-192">A maximális érték, amely a riasztást az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="b5594-192">The maximum value that triggers an alert in the dashboard.</span></span> |
| <span data-ttu-id="b5594-193">MaximumAlertActions</span><span class="sxs-lookup"><span data-stu-id="b5594-193">MaximumAlertActions</span></span> | <span data-ttu-id="b5594-194">A riasztás válasz műveletet.</span><span class="sxs-lookup"><span data-stu-id="b5594-194">An action to take in response to an alert.</span></span> <span data-ttu-id="b5594-195">Például egy vonatkozó parancs küldése a állomáshoz.</span><span class="sxs-lookup"><span data-stu-id="b5594-195">For example, send a command to a station.</span></span> |
| <span data-ttu-id="b5594-196">ConstValue</span><span class="sxs-lookup"><span data-stu-id="b5594-196">ConstValue</span></span> | <span data-ttu-id="b5594-197">A számításhoz használt konstans érték.</span><span class="sxs-lookup"><span data-stu-id="b5594-197">A constant value used in a calculation.</span></span> |

## <a name="deploy-the-changes"></a><span data-ttu-id="b5594-198">A módosítások telepítése</span><span class="sxs-lookup"><span data-stu-id="b5594-198">Deploy the changes</span></span>

<span data-ttu-id="b5594-199">Miután végzett a módosítások elvégzése az **ContosoTopologyDescription.json** fájlt, újra kell telepítenie a csatlakoztatott gyári megoldás az Azure-fiókjába.</span><span class="sxs-lookup"><span data-stu-id="b5594-199">When you have finished making changes to the **ContosoTopologyDescription.json** file, you must redeploy the connected factory solution to your Azure account.</span></span>

<span data-ttu-id="b5594-200">A **azure iot-csatlakoztatott-gyári** tárház tartalmaz egy **build.ps1** PowerShell parancsprogram, amellyel újraépítéséhez és a megoldás üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="b5594-200">The **azure-iot-connected-factory** repository includes a **build.ps1** PowerShell script you can use to rebuild and deploy the solution.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b5594-201">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b5594-201">Next Steps</span></span>

<span data-ttu-id="b5594-202">További információ a csatlakoztatott gyári előre konfigurált megoldásról elolvasni a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="b5594-202">Learn more about the connected factory preconfigured solution by reading the following articles:</span></span>

* <span data-ttu-id="b5594-203">[Előre konfigurált csatlakoztatott gyár megoldás – bemutató][lnk-rm-walkthrough]</span><span class="sxs-lookup"><span data-stu-id="b5594-203">[Connected factory preconfigured solution walkthrough][lnk-rm-walkthrough]</span></span>
* <span data-ttu-id="b5594-204">[A beépített csatlakoztatott egy átjáró üzembe helyezéséhez][lnk-connect-cf]</span><span class="sxs-lookup"><span data-stu-id="b5594-204">[Deploy a gateway for connected factory][lnk-connect-cf]</span></span>
* <span data-ttu-id="b5594-205">[Engedélyek az azureiotsuite.com webhelyen][lnk-permissions]</span><span class="sxs-lookup"><span data-stu-id="b5594-205">[Permissions on the azureiotsuite.com site][lnk-permissions]</span></span>
* [<span data-ttu-id="b5594-206">Csatlakoztatott gyár – GYIK</span><span class="sxs-lookup"><span data-stu-id="b5594-206">Connected factory FAQ</span></span>](iot-suite-faq-cf.md)
* <span data-ttu-id="b5594-207">[GYAKORI KÉRDÉSEK][lnk-faq]</span><span class="sxs-lookup"><span data-stu-id="b5594-207">[FAQ][lnk-faq]</span></span>


[img-oee-kpi]: ./media/iot-suite-connected-factory-customize/oeenadkpi.png
[img-manufactured-items]: ./media/iot-suite-connected-factory-customize/manufactured.png
[img-tsi]: ./media/iot-suite-connected-factory-customize/tsi.png
[img-select-server]: ./media/iot-suite-connected-factory-customize/selectserver.png
[img-published]: ./media/iot-suite-connected-factory-customize/published.png
[img-munich]: ./media/iot-suite-connected-factory-customize/munich.png
[img-server-uris]: ./media/iot-suite-connected-factory-customize/serveruris.png
[lnk-kpi]: ./media/iot-suite-connected-factory-customize/kpidisplay.png

[lnk-rm-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[lnk-connect-cf]: iot-suite-connected-factory-gateway-deployment.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-faq]: iot-suite-faq.md