---
title: "Azure IoT Suite aaaCustomize csatlakoztatott gyári |} Microsoft Docs"
description: "Hogyan toocustomize hello viselkedését hello csatlakoztatva gyári leírása előre konfigurált megoldás."
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
ms.openlocfilehash: 53f2fef7a76b5d8e6ad023945a7812dc7fabd12c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="customize-how-hello-connected-factory-solution-displays-data-from-your-opc-ua-servers"></a><span data-ttu-id="01927-103">Hogyan hello csatlakoztatva a OPC EE-kiszolgálóiról gyári megoldás jelenít meg adatokat testreszabása</span><span class="sxs-lookup"><span data-stu-id="01927-103">Customize how hello connected factory solution displays data from your OPC UA servers</span></span>

## <a name="introduction"></a><span data-ttu-id="01927-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="01927-104">Introduction</span></span>

<span data-ttu-id="01927-105">hello csatlakoztatott gyári megoldás összesíti és hello OPC EE kiszolgálók csatlakoztatott toohello megoldás adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="01927-105">hello connected factory solution aggregates and displays data from hello OPC UA servers connected toohello solution.</span></span> <span data-ttu-id="01927-106">Keresse meg és kiszolgálók parancsok toohello OPC EE küldhet a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="01927-106">You can browse and send commands toohello OPC UA servers in your solution.</span></span> <span data-ttu-id="01927-107">OPC EE kapcsolatos további információkért lásd: hello [gyári gyakran ismételt kérdések csatlakoztatott](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="01927-107">For more information about OPC UA, see hello [Connected factory FAQ](iot-suite-faq-cf.md).</span></span>

<span data-ttu-id="01927-108">Összesített adatot hello megoldás például hello általános berendezések hatékonyságát (OEE) és fő teljesítménymutatók (KPI-k), megtekintheti a hello irányítópult hello gyári, a sor és az állomás szintjén.</span><span class="sxs-lookup"><span data-stu-id="01927-108">Examples of aggregated data in hello solution include hello Overall Equipment Efficiency (OEE) and Key Performance Indicators (KPIs) that you can view in hello dashboard at hello factory, line, and station levels.</span></span> <span data-ttu-id="01927-109">hello alábbi képernyőfelvételen látható hello OEE és KPI értékeit hello **szerelvény** állomás, a **termelési sor 1**, a hello **München** gyári:</span><span class="sxs-lookup"><span data-stu-id="01927-109">hello following screenshot shows hello OEE and KPI values for hello **Assembly** station, on **Production line 1**, in hello **Munich** factory:</span></span>

![OEE és KPI értékek hello megoldás – példa][img-oee-kpi]

<span data-ttu-id="01927-111">hello megoldás lehetővé teszi, hogy Ön tooview részletes információkat az adott adatok tételekhez hello nevezett OPC EE-kiszolgálók *állomások*.</span><span class="sxs-lookup"><span data-stu-id="01927-111">hello solution enables you tooview detailed information from specific data items from hello OPC UA servers, called *stations*.</span></span> <span data-ttu-id="01927-112">hello alábbi képernyőfelvételen látható függvényében hello elemszáma gyártott egy adott állomásról:</span><span class="sxs-lookup"><span data-stu-id="01927-112">hello following screenshot shows plots of hello number of manufactured items from a specific station:</span></span>

![Felvétel az előállított elemek száma.][img-manufactured-items]

<span data-ttu-id="01927-114">Ha egy grafikonon hello gombra kattint, ismerje meg a hello adatokat, és további időt adatsorozat Insights (ÁME):</span><span class="sxs-lookup"><span data-stu-id="01927-114">If you click one of hello graphs, you can explore hello data further using Time Series Insights (TSI):</span></span>

![Adatokba idő adatsorozat Insights segítségével][img-tsi]

<span data-ttu-id="01927-116">Ez a cikk ismerteti:</span><span class="sxs-lookup"><span data-stu-id="01927-116">This article describes:</span></span>

- <span data-ttu-id="01927-117">Hogyan hello adatok legyen elérhető toohello különböző nézetek hello megoldásban.</span><span class="sxs-lookup"><span data-stu-id="01927-117">How hello data is made available toohello various views in hello solution.</span></span>
- <span data-ttu-id="01927-118">Hogyan szabhatja testre hello módon hello megoldás hello adatokat jeleníthet meg.</span><span class="sxs-lookup"><span data-stu-id="01927-118">How you can customize hello way hello solution displays hello data.</span></span>

## <a name="data-sources"></a><span data-ttu-id="01927-119">Adatforrások</span><span class="sxs-lookup"><span data-stu-id="01927-119">Data sources</span></span>

<span data-ttu-id="01927-120">hello csatlakoztatott gyári megoldás adatait jeleníti meg hello OPC EE kiszolgálók csatlakoztatott toohello megoldás.</span><span class="sxs-lookup"><span data-stu-id="01927-120">hello connected factory solution displays data from hello OPC UA servers connected toohello solution.</span></span> <span data-ttu-id="01927-121">hello alapértelmezett telepítése magában foglalja a gyári szimuláció futtató több OPC EE.</span><span class="sxs-lookup"><span data-stu-id="01927-121">hello default installation includes several OPC UA servers running a factory simulation.</span></span> <span data-ttu-id="01927-122">A saját OPC EE-kiszolgálót is hozzáadhat, amelyek [átjárón keresztül csatlakozni] [ lnk-connect-cf] tooyour megoldás.</span><span class="sxs-lookup"><span data-stu-id="01927-122">You can add your own OPC UA servers that [connect through a gateway][lnk-connect-cf] tooyour solution.</span></span>

<span data-ttu-id="01927-123">Tallózással is kikeresheti, hogy a csatlakoztatott OPC EE-kiszolgáló küldhet-e a tooyour megoldás hello irányítópulton hello adatelemek:</span><span class="sxs-lookup"><span data-stu-id="01927-123">You can browse hello data items that a connected OPC UA server can send tooyour solution in hello dashboard:</span></span>

1. <span data-ttu-id="01927-124">Keresse meg a toohello **jelöljön ki egy OPC EE** megtekintése:</span><span class="sxs-lookup"><span data-stu-id="01927-124">Navigate toohello **Select an OPC UA server** view:</span></span>

    ![Keresse meg a toohello válasszon egy OPC EE-kiszolgáló megtekintése][img-select-server]

1. <span data-ttu-id="01927-126">Válasszon kiszolgálót, majd kattintson a **Connect**.</span><span class="sxs-lookup"><span data-stu-id="01927-126">Select a server and click **Connect**.</span></span> <span data-ttu-id="01927-127">Kattintson a **folytatása** hello biztonsági figyelmeztetés megjelenésekor.</span><span class="sxs-lookup"><span data-stu-id="01927-127">Click **Proceed** when hello security warning appears.</span></span>

    > [!NOTE]
    > <span data-ttu-id="01927-128">Ez a figyelmeztetés csak egyszer jelenik meg minden olyan kiszolgálón, és hello megoldás irányítópultja és hello kiszolgáló közötti megbízhatósági kapcsolatot létesít.</span><span class="sxs-lookup"><span data-stu-id="01927-128">This warning only appears once for each server and establishes a trust relationship between hello solution dashboard and hello server.</span></span>

1. <span data-ttu-id="01927-129">Most Tallózás hello adatelemek hello server küldhet toohello megoldás is.</span><span class="sxs-lookup"><span data-stu-id="01927-129">You can now browse hello data items that hello server can send toohello solution.</span></span> <span data-ttu-id="01927-130">Toohello megoldás küldött elemeket zöld pipa rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="01927-130">Items that are being sent toohello solution have a green check mark:</span></span>

    ![Közzétett elemek][img-published]

1. <span data-ttu-id="01927-132">Ha Ön egy *rendszergazda* hello megoldást választhatja ki toopublish egy adatok elem toomake hello elérhető csatlakoztatva gyári megoldás.</span><span class="sxs-lookup"><span data-stu-id="01927-132">If you are an *Administrator* in hello solution, you can choose toopublish a data item toomake it available in hello connected factory solution.</span></span> <span data-ttu-id="01927-133">A rendszergazdák is adatelemek hello értékének módosítása és hívható meg metódus hello OPC EE-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="01927-133">As an Administrator, you can also change hello value of data items and call methods in hello OPC UA server.</span></span>

## <a name="map-hello-data"></a><span data-ttu-id="01927-134">Hello adatok leképezése</span><span class="sxs-lookup"><span data-stu-id="01927-134">Map hello data</span></span>

<span data-ttu-id="01927-135">hello gyári megoldás maps csatlakoztatva, és összesítések hello közzétett adatelemek hello OPC EE server toohello a különböző nézetek hello megoldásban.</span><span class="sxs-lookup"><span data-stu-id="01927-135">hello connected factory solution maps and aggregates hello published data items from hello OPC UA server toohello various views in hello solution.</span></span> <span data-ttu-id="01927-136">hello csatlakoztatott gyári megoldás telepíti tooyour Azure-fiók amikor hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="01927-136">hello connected factory solution deploys tooyour Azure account when you provision hello solution.</span></span> <span data-ttu-id="01927-137">A JSON-fájl, a Visual Studio csatlakoztatott gyári megoldás hello a leképezési információkat tárolja.</span><span class="sxs-lookup"><span data-stu-id="01927-137">A JSON file in hello Visual Studio connected factory solution stores this mapping information.</span></span> <span data-ttu-id="01927-138">Megtekintheti és módosíthatja a JSON-konfigurációs fájl hello csatlakoztatott gyárban Visual Studio megoldás.</span><span class="sxs-lookup"><span data-stu-id="01927-138">You can view and modify this JSON configuration file in hello connected factory Visual Studio solution.</span></span> <span data-ttu-id="01927-139">A módosítást követően központilag telepítheti hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="01927-139">You can redeploy hello solution after you make a change.</span></span>

<span data-ttu-id="01927-140">A konfigurációs fájlt hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="01927-140">You can use hello configuration file to:</span></span>

- <span data-ttu-id="01927-141">Hello meglévő szimulált előállítók, éles sorok és állomások szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="01927-141">Edit hello existing simulated factories, production lines, and stations.</span></span>
- <span data-ttu-id="01927-142">Képezze le a csatlakozni, toohello megoldás valós OPC EE-kiszolgálóról származó adatok.</span><span class="sxs-lookup"><span data-stu-id="01927-142">Map data from real OPC UA servers that you connect toohello solution.</span></span>

<span data-ttu-id="01927-143">hello másolatát tooclone gyári Visual Studio megoldás, a következő parancsot a git használata hello csatlakoztatva:</span><span class="sxs-lookup"><span data-stu-id="01927-143">tooclone a copy of hello connected factory Visual Studio solution, use hello following git command:</span></span>

`git clone https://github.com/Azure/azure-iot-connected-factory.git`

<span data-ttu-id="01927-144">hello fájl **ContosoTopologyDescription.json** társítást hello OPC EE-kiszolgálóadatok elemek toohello nézetek hello csatlakoztatott gyári megoldás irányítópultjának hello határozza meg.</span><span class="sxs-lookup"><span data-stu-id="01927-144">hello file **ContosoTopologyDescription.json** defines hello mapping from hello OPC UA server data items toohello views in hello connected factory solution dashboard.</span></span> <span data-ttu-id="01927-145">A konfigurációs fájl található hello **Contoso\Topology** hello mappájában **WebApp** projektre a Visual Studio megoldás hello.</span><span class="sxs-lookup"><span data-stu-id="01927-145">You can find this configuration file in hello **Contoso\Topology** folder in hello **WebApp** project in hello Visual Studio solution.</span></span>

<span data-ttu-id="01927-146">hello JSON-fájl tartalmának hello gyári, termelési sor és állomás csomópontok hierarchikus vannak rendezve.</span><span class="sxs-lookup"><span data-stu-id="01927-146">hello content of hello JSON file is organized as a hierarchy of factory, production line, and station nodes.</span></span> <span data-ttu-id="01927-147">Ezt a hierarchiát hello navigációs hierarchia hello csatlakoztatott gyári irányítópult határozza meg.</span><span class="sxs-lookup"><span data-stu-id="01927-147">This hierarchy defines hello navigation hierarchy in hello connected factory dashboard.</span></span> <span data-ttu-id="01927-148">Minden egyes csomóponton hello hierarchia értékek meghatározzák, hogy hello irányítópulton megjelenő hello információ.</span><span class="sxs-lookup"><span data-stu-id="01927-148">Values at each node of hello hierarchy determine hello information displayed in hello dashboard.</span></span> <span data-ttu-id="01927-149">Például hello JSON-fájl tartalmazza a következő értékek hello München gyári hello:</span><span class="sxs-lookup"><span data-stu-id="01927-149">For example, hello JSON file contains hello following values for hello Munich factory:</span></span>

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

<span data-ttu-id="01927-150">hello nevét, leírását és helyen jelennek meg ebben a nézetben hello irányítópulton:</span><span class="sxs-lookup"><span data-stu-id="01927-150">hello name, description, and location appear on this view in hello dashboard:</span></span>

![München adatok hello irányítópult][img-munich]

<span data-ttu-id="01927-152">Minden egyes gyári, a termelési sor és a állomás van az image tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="01927-152">Each factory, production line, and station have an image property.</span></span> <span data-ttu-id="01927-153">A JPEG-fájlok találhatók hello **Content\img** hello mappájában **WebApp** projekt.</span><span class="sxs-lookup"><span data-stu-id="01927-153">You can find these JPEG files in hello **Content\img** folder in hello **WebApp** project.</span></span> <span data-ttu-id="01927-154">A kép fájlok hello csatlakoztatott gyári irányítópult jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="01927-154">These image files display in hello connected factory dashboard.</span></span>

<span data-ttu-id="01927-155">Minden állomás több társítást hello OPC EE adatelemek hello meghatározó részletes tulajdonságok tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="01927-155">Each station includes several detailed properties that define hello mapping from hello OPC UA data items.</span></span> <span data-ttu-id="01927-156">Ezeket a tulajdonságokat ismerteti a következő részekben hello:</span><span class="sxs-lookup"><span data-stu-id="01927-156">These properties are described in hello following sections:</span></span>

### <a name="opcuri"></a><span data-ttu-id="01927-157">OpcUri</span><span class="sxs-lookup"><span data-stu-id="01927-157">OpcUri</span></span>

<span data-ttu-id="01927-158">Hello **OpcUri** értéke hello OPC EE alkalmazás URI, amely egyedileg azonosítja a hello OPC EE-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="01927-158">hello **OpcUri** value is hello OPC UA Application URI that uniquely identifies hello OPC UA server.</span></span> <span data-ttu-id="01927-159">Például hello **OpcUri** hello szerelvény állomás éles sor 1 München a következőhöz hasonló hello értéke: **urn: scada2194:ua:munich:productionline0:assemblystation**.</span><span class="sxs-lookup"><span data-stu-id="01927-159">For example, hello **OpcUri** value for hello assembly station on production line 1 in Munich looks like hello following: **urn:scada2194:ua:munich:productionline0:assemblystation**.</span></span>

<span data-ttu-id="01927-160">Hello URI-azonosítók csatlakoztatott hello OPC EE-kiszolgálók hello megoldás irányítópultja tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="01927-160">You can view hello URIs of hello connected OPC UA servers in hello solution dashboard:</span></span>

![Nézet OPC EE kiszolgáló URI-azonosítók][img-server-uris]

### <a name="simulation"></a><span data-ttu-id="01927-162">Szimuláció</span><span class="sxs-lookup"><span data-stu-id="01927-162">Simulation</span></span>

<span data-ttu-id="01927-163">hello információk hello **szimuláció** csomópont adott toohello OPC EE szimuláció futó hello OPC EE-kiszolgálók alapértelmezés szerint törlődnek.</span><span class="sxs-lookup"><span data-stu-id="01927-163">hello information in hello **Simulation** node is specific toohello OPC UA simulation that runs in hello OPC UA servers that are provisioned by default.</span></span> <span data-ttu-id="01927-164">A valós OPC EE-kiszolgáló nem szolgál.</span><span class="sxs-lookup"><span data-stu-id="01927-164">It is not used for a real OPC UA server.</span></span>

### <a name="kpi1-and-kpi2"></a><span data-ttu-id="01927-165">Kpi1 és Kpi2</span><span class="sxs-lookup"><span data-stu-id="01927-165">Kpi1 and Kpi2</span></span>

<span data-ttu-id="01927-166">Ezek a csomópontok ismertetik, hogyan hello állomás adatait hozzájárul toohello két KPI értékek hello irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="01927-166">These nodes describe how data from hello station contributes toohello two KPI values in hello dashboard.</span></span> <span data-ttu-id="01927-167">Alapértelmezett telepítésében ezen KPI értékei óránként egységek és óránként kWh.</span><span class="sxs-lookup"><span data-stu-id="01927-167">In a default deployment, these KPI values are units per hour and kWh per hour.</span></span> <span data-ttu-id="01927-168">hello megoldás kiszámítja a KPI vales állomás hello szintjén, és összesíti a hello termelési sor és a gyári szintjén.</span><span class="sxs-lookup"><span data-stu-id="01927-168">hello solution calculates KPI vales at hello level of a station and aggregates them at hello production line and factory levels.</span></span>

<span data-ttu-id="01927-169">Egyes KPI-KET egy minimális, maximális és célértékéhez rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="01927-169">Each KPI has a minimum, maximum, and target value.</span></span> <span data-ttu-id="01927-170">Minden KPI értéket adhat meg csatlakoztatott hello gyári megoldás tooperform riasztási műveletek.</span><span class="sxs-lookup"><span data-stu-id="01927-170">Each KPI value can also define alert actions for hello connected factory solution tooperform.</span></span> <span data-ttu-id="01927-171">hello alábbi kódrészletben láthatja hello KPI definícióit hello szerelvény állomás éles sor München 1:</span><span class="sxs-lookup"><span data-stu-id="01927-171">hello following snippet shows hello KPI definitions for hello assembly station on production line 1 in Munich:</span></span>

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

<span data-ttu-id="01927-172">hello alábbi képernyőfelvételen látható hello KPI adatok hello irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="01927-172">hello following screenshot shows hello KPI data in hello dashboard.</span></span>

![Hello irányítópulton KPI-információk][lnk-kpi]

### <a name="opcnodes"></a><span data-ttu-id="01927-174">OpcNodes</span><span class="sxs-lookup"><span data-stu-id="01927-174">OpcNodes</span></span>

<span data-ttu-id="01927-175">Hello **OpcNodes** csomópontok azonosítása hello hello OPC EE-kiszolgáló a közzétett adatelemeket, és adja meg, hogyan tooprocess adatokat.</span><span class="sxs-lookup"><span data-stu-id="01927-175">hello **OpcNodes** nodes identify hello published data items from hello OPC UA server and specify how tooprocess that data.</span></span>

<span data-ttu-id="01927-176">Hello **NodeId** érték azonosítja hello hello OPC EE-kiszolgáló az adott OPC EE NodeID.</span><span class="sxs-lookup"><span data-stu-id="01927-176">hello **NodeId** value identifies hello specific OPC UA NodeID from hello OPC UA server.</span></span> <span data-ttu-id="01927-177">első csomópontjának hello hello szerelvény állomás gyártási sor München 1 értékű **ns = 2, i = 385**.</span><span class="sxs-lookup"><span data-stu-id="01927-177">hello first node in hello assembly station for production line 1 in Munich has a value **ns=2;i=385**.</span></span> <span data-ttu-id="01927-178">A **NodeId** érték határozza meg, hello adatok elem tooread hello OPC EE-kiszolgáló és a hello **melynek** egy felhasználóbarát név toouse hello irányítópulton biztosítja az adatok.</span><span class="sxs-lookup"><span data-stu-id="01927-178">A **NodeId** value specifies hello data item tooread from hello OPC UA server, and hello **SymbolicName** provides a user-friendly name toouse in hello dashboard for that data.</span></span>

<span data-ttu-id="01927-179">Más csomópontokra társított értékek hello a következő táblázat foglalja össze:</span><span class="sxs-lookup"><span data-stu-id="01927-179">Other values associated with each node are summarized in hello following table:</span></span>

| <span data-ttu-id="01927-180">Érték</span><span class="sxs-lookup"><span data-stu-id="01927-180">Value</span></span> | <span data-ttu-id="01927-181">Leírás</span><span class="sxs-lookup"><span data-stu-id="01927-181">Description</span></span> |
| ----- | ----------- |
| <span data-ttu-id="01927-182">Találati pontosság</span><span class="sxs-lookup"><span data-stu-id="01927-182">Relevance</span></span>  | <span data-ttu-id="01927-183">hello KPI-t és OEE értékek ezek az adatok hozzájárul.</span><span class="sxs-lookup"><span data-stu-id="01927-183">hello KPI and OEE values this data contributes to.</span></span> |
| <span data-ttu-id="01927-184">Műveleti kód</span><span class="sxs-lookup"><span data-stu-id="01927-184">OpCode</span></span>     | <span data-ttu-id="01927-185">Hogyan hello adatok összesíti.</span><span class="sxs-lookup"><span data-stu-id="01927-185">How hello data is aggregated.</span></span> |
| <span data-ttu-id="01927-186">egység</span><span class="sxs-lookup"><span data-stu-id="01927-186">Units</span></span>      | <span data-ttu-id="01927-187">hello egységek toouse hello irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="01927-187">hello units toouse in hello dashboard.</span></span>  |
| <span data-ttu-id="01927-188">Látható</span><span class="sxs-lookup"><span data-stu-id="01927-188">Visible</span></span>    | <span data-ttu-id="01927-189">E toodisplay ez értéket hello irányítópult.</span><span class="sxs-lookup"><span data-stu-id="01927-189">Whether toodisplay this value in hello dashboard.</span></span> <span data-ttu-id="01927-190">Egyes értékek számítások szerepel, de nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="01927-190">Some values are used in calculations but not displayed.</span></span>  |
| <span data-ttu-id="01927-191">Maximális</span><span class="sxs-lookup"><span data-stu-id="01927-191">Maximum</span></span>    | <span data-ttu-id="01927-192">hello maximális érték, amely riasztást hello irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="01927-192">hello maximum value that triggers an alert in hello dashboard.</span></span> |
| <span data-ttu-id="01927-193">MaximumAlertActions</span><span class="sxs-lookup"><span data-stu-id="01927-193">MaximumAlertActions</span></span> | <span data-ttu-id="01927-194">Egy művelet tootake válasz tooan riasztásban.</span><span class="sxs-lookup"><span data-stu-id="01927-194">An action tootake in response tooan alert.</span></span> <span data-ttu-id="01927-195">A parancs tooa állomás küldenek például.</span><span class="sxs-lookup"><span data-stu-id="01927-195">For example, send a command tooa station.</span></span> |
| <span data-ttu-id="01927-196">ConstValue</span><span class="sxs-lookup"><span data-stu-id="01927-196">ConstValue</span></span> | <span data-ttu-id="01927-197">A számításhoz használt konstans érték.</span><span class="sxs-lookup"><span data-stu-id="01927-197">A constant value used in a calculation.</span></span> |

## <a name="deploy-hello-changes"></a><span data-ttu-id="01927-198">Hello módosítása</span><span class="sxs-lookup"><span data-stu-id="01927-198">Deploy hello changes</span></span>

<span data-ttu-id="01927-199">Miután végzett a végzett módosítások toohello **ContosoTopologyDescription.json** fájlt, újra kell telepítenie hello gyári megoldás tooyour Azure-fiók csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="01927-199">When you have finished making changes toohello **ContosoTopologyDescription.json** file, you must redeploy hello connected factory solution tooyour Azure account.</span></span>

<span data-ttu-id="01927-200">Hello **azure iot-csatlakoztatott-gyári** tárház tartalmaz egy **build.ps1** PowerShell-parancsfájlt használhatja toorebuild és hello megoldás üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="01927-200">hello **azure-iot-connected-factory** repository includes a **build.ps1** PowerShell script you can use toorebuild and deploy hello solution.</span></span>

## <a name="next-steps"></a><span data-ttu-id="01927-201">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="01927-201">Next Steps</span></span>

<span data-ttu-id="01927-202">További információ csatlakoztatott hello előre konfigurált gyári megoldásról által olvasása hello a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="01927-202">Learn more about hello connected factory preconfigured solution by reading hello following articles:</span></span>

* <span data-ttu-id="01927-203">[Előre konfigurált csatlakoztatott gyár megoldás – bemutató][lnk-rm-walkthrough]</span><span class="sxs-lookup"><span data-stu-id="01927-203">[Connected factory preconfigured solution walkthrough][lnk-rm-walkthrough]</span></span>
* <span data-ttu-id="01927-204">[A beépített csatlakoztatott egy átjáró üzembe helyezéséhez][lnk-connect-cf]</span><span class="sxs-lookup"><span data-stu-id="01927-204">[Deploy a gateway for connected factory][lnk-connect-cf]</span></span>
* <span data-ttu-id="01927-205">[Engedélyek hello azureiotsuite.com webhelyen][lnk-permissions]</span><span class="sxs-lookup"><span data-stu-id="01927-205">[Permissions on hello azureiotsuite.com site][lnk-permissions]</span></span>
* [<span data-ttu-id="01927-206">Csatlakoztatott gyár – GYIK</span><span class="sxs-lookup"><span data-stu-id="01927-206">Connected factory FAQ</span></span>](iot-suite-faq-cf.md)
* <span data-ttu-id="01927-207">[GYAKORI KÉRDÉSEK][lnk-faq]</span><span class="sxs-lookup"><span data-stu-id="01927-207">[FAQ][lnk-faq]</span></span>


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