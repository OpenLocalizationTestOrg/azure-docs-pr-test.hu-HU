---
title: "aaaConnected gyári Azure IoT Suite megoldás forgatókönyv |} Microsoft Docs"
description: "Hello előre konfigurált Azure IoT-megoldás leírása factory és az architektúra csatlakoztatva."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 31fe13af-0482-47be-b4c8-e98e36625855
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: dobett
ms.openlocfilehash: 7fd55c51351659401349cfde91a20fce1045b4f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connected-factory-preconfigured-solution-walkthrough"></a><span data-ttu-id="e6648-103">Előre konfigurált csatlakoztatott gyár megoldás – bemutató</span><span class="sxs-lookup"><span data-stu-id="e6648-103">Connected factory preconfigured solution walkthrough</span></span>

<span data-ttu-id="e6648-104">hello IoT Suite csatlakoztatott gyári [előre konfigurált megoldás] [ lnk-preconfigured-solutions] egy végpontok közötti ipari megoldás megvalósítása, amely:</span><span class="sxs-lookup"><span data-stu-id="e6648-104">hello IoT Suite connected factory [preconfigured solution][lnk-preconfigured-solutions] is an implementation of an end-to-end industrial solution that:</span></span>

* <span data-ttu-id="e6648-105">Szimulált tooboth ipari eszközökön futó OPC EE-kiszolgálók szimulált gyári éles sorok és valós OPC EE-kiszolgálóeszközökkel a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="e6648-105">Connects tooboth simulated industrial devices running OPC UA servers in simulated factory production lines, and real OPC UA server devices.</span></span> <span data-ttu-id="e6648-106">OPC EE kapcsolatos további információkért lásd: hello [gyári gyakran ismételt kérdések csatlakoztatott](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="e6648-106">For more information about OPC UA, see hello [Connected factory FAQ](iot-suite-faq-cf.md).</span></span>
* <span data-ttu-id="e6648-107">Megjeleníti ezen eszközök és gyártósorok működési KPI- és OEE-értékeit.</span><span class="sxs-lookup"><span data-stu-id="e6648-107">Shows operational KPIs and OEE of those devices and production lines.</span></span>
* <span data-ttu-id="e6648-108">Bemutatja, hogyan egy felhőalapú alkalmazás OPC EE server rendszerrel használt toointeract lehet-e.</span><span class="sxs-lookup"><span data-stu-id="e6648-108">Demonstrates how a cloud-based application could be used toointeract with OPC UA server systems.</span></span>
* <span data-ttu-id="e6648-109">Lehetővé teszi, hogy Ön tooconnect saját OPC EE kiszolgálóeszközökkel.</span><span class="sxs-lookup"><span data-stu-id="e6648-109">Enables you tooconnect your own OPC UA server devices.</span></span>
* <span data-ttu-id="e6648-110">Lehetővé teszi a toobrowse és hello OPC EE-kiszolgáló adatainak módosítása.</span><span class="sxs-lookup"><span data-stu-id="e6648-110">Enables you toobrowse and modify hello OPC UA server data.</span></span>
* <span data-ttu-id="e6648-111">Integrálható a hello Azure idő adatsorozat Insights (ÁME) szolgáltatás tooprovide egyéni nézetek hello adatok a OPC EE-kiszolgálókról.</span><span class="sxs-lookup"><span data-stu-id="e6648-111">Integrates with hello Azure Time Series Insights (TSI) service tooprovide customized views of hello data from your OPC UA servers.</span></span>

<span data-ttu-id="e6648-112">Használhat hello megoldás kiindulási pontként a saját megvalósítási és [testreszabása] [ lnk-customize] azt toomeet saját speciális üzleti igényeinek.</span><span class="sxs-lookup"><span data-stu-id="e6648-112">You can use hello solution as a starting point for your own implementation and [customize][lnk-customize] it toomeet your own specific business requirements.</span></span>

<span data-ttu-id="e6648-113">Ez a cikk bemutatja, hogyan néhány hello fontosabb elemei csatlakozó hello gyári megoldás tooenable toounderstand, hogyan működik.</span><span class="sxs-lookup"><span data-stu-id="e6648-113">This article walks you through some of hello key elements of hello connected factory solution tooenable you toounderstand how it works.</span></span> <span data-ttu-id="e6648-114">Ezeknek az ismereteknek a birtokában:</span><span class="sxs-lookup"><span data-stu-id="e6648-114">This knowledge helps you to:</span></span>

* <span data-ttu-id="e6648-115">Problémamegoldás hello megoldásban.</span><span class="sxs-lookup"><span data-stu-id="e6648-115">Troubleshoot issues in hello solution.</span></span>
* <span data-ttu-id="e6648-116">Tervezze meg hogyan toocustomize toohello megoldás toomeet a saját konkrét követelmények.</span><span class="sxs-lookup"><span data-stu-id="e6648-116">Plan how toocustomize toohello solution toomeet your own specific requirements.</span></span>
* <span data-ttu-id="e6648-117">Kialakíthatja saját, Azure-szolgáltatásokat használó IoT-megoldását.</span><span class="sxs-lookup"><span data-stu-id="e6648-117">Design your own IoT solution that uses Azure services.</span></span>

<span data-ttu-id="e6648-118">További információkért lásd: hello [gyári gyakran ismételt kérdések csatlakoztatott](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="e6648-118">For more information, see hello [Connected factory FAQ](iot-suite-faq-cf.md).</span></span>

## <a name="logical-architecture"></a><span data-ttu-id="e6648-119">Logikai architektúra</span><span class="sxs-lookup"><span data-stu-id="e6648-119">Logical architecture</span></span>

<span data-ttu-id="e6648-120">a következő diagram hello hello logikai összetevőinek előre konfigurált hello megoldás ismerteti:</span><span class="sxs-lookup"><span data-stu-id="e6648-120">hello following diagram outlines hello logical components of hello preconfigured solution:</span></span>

![Csatlakoztatott gyár logikai architektúrája][connected-factory-logical]

## <a name="communication-patterns"></a><span data-ttu-id="e6648-122">Kommunikációs minták</span><span class="sxs-lookup"><span data-stu-id="e6648-122">Communication patterns</span></span>

<span data-ttu-id="e6648-123">hello megoldás az hello [OPC EE Pub/Sub specification](https://opcfoundation.org/news/opc-foundation-news/opc-foundation-announces-support-of-publish-subscribe-for-opc-ua/) toosend OPC EE telemetriai adatok tooIoT központ JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="e6648-123">hello solution uses hello [OPC UA Pub/Sub specification](https://opcfoundation.org/news/opc-foundation-news/opc-foundation-announces-support-of-publish-subscribe-for-opc-ua/) toosend OPC UA telemetry data tooIoT Hub in JSON format.</span></span> <span data-ttu-id="e6648-124">hello megoldás az hello [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) IoT peremhálózati modul erre a célra.</span><span class="sxs-lookup"><span data-stu-id="e6648-124">hello solution uses hello [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) IoT Edge module for this purpose.</span></span>

<span data-ttu-id="e6648-125">hello megoldás is rendelkezik egy OPC EE-ügyfél integrálva egy webes alkalmazás, amely képes kapcsolatot létrehozni a helyszíni OPC EE-kiszolgálókkal.</span><span class="sxs-lookup"><span data-stu-id="e6648-125">hello solution also has an OPC UA client integrated into a web application that can establish connections with on-premises OPC UA servers.</span></span> <span data-ttu-id="e6648-126">hello ügyfél használ egy [fordított proxy](https://wikipedia.org/wiki/Reverse_proxy) és súgó fogad az IoT-központ toomake hello kapcsolat hello helyszíni tűzfal portjainak megnyitása nélkül.</span><span class="sxs-lookup"><span data-stu-id="e6648-126">hello client uses a [reverse-proxy](https://wikipedia.org/wiki/Reverse_proxy) and receives help from IoT Hub toomake hello connection without requiring open ports in hello on-premises firewall.</span></span> <span data-ttu-id="e6648-127">Ezt a kommunikációs mintát [szolgáltatás által támogatott kommunikációnak](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/) hívják.</span><span class="sxs-lookup"><span data-stu-id="e6648-127">This communication pattern is called [service-assisted communication](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/).</span></span> <span data-ttu-id="e6648-128">hello megoldás az hello [OPC Proxy](https://github.com/Azure/iot-edge-opc-proxy/) IoT peremhálózati modul erre a célra.</span><span class="sxs-lookup"><span data-stu-id="e6648-128">hello solution uses hello [OPC Proxy](https://github.com/Azure/iot-edge-opc-proxy/) IoT Edge module for this purpose.</span></span>


## <a name="simulation"></a><span data-ttu-id="e6648-129">Szimuláció</span><span class="sxs-lookup"><span data-stu-id="e6648-129">Simulation</span></span>

<span data-ttu-id="e6648-130">hello szimulált állomások és a szimulált hello gyártási végrehajtási rendszerek (MES) jött létre a gyári termelési sor.</span><span class="sxs-lookup"><span data-stu-id="e6648-130">hello simulated stations and hello simulated manufacturing execution systems (MES) make up a factory production line.</span></span> <span data-ttu-id="e6648-131">hello szimulált eszközök és hello OPC Publisher modul alapuló hello [OPC EE .NET szabványos] [ lnk-OPC-UA-NET-Standard] hello OPC Foundation tett közzé.</span><span class="sxs-lookup"><span data-stu-id="e6648-131">hello simulated devices and hello OPC Publisher Module are based on hello [OPC UA .NET Standard][lnk-OPC-UA-NET-Standard] published by hello OPC Foundation.</span></span>

<span data-ttu-id="e6648-132">hello OPC Proxy és OPC szoftvergyártó valósíthatók alapján modulok [Azure IoT peremhálózati][lnk-Azure-IoT-Gateway].</span><span class="sxs-lookup"><span data-stu-id="e6648-132">hello OPC Proxy and OPC Publisher are implemented as modules based on [Azure IoT Edge][lnk-Azure-IoT-Gateway].</span></span> <span data-ttu-id="e6648-133">Mindegyik szimulált gyártósorhoz kijelölt átjáró van csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="e6648-133">Each simulated production line has a designated gateway attached.</span></span>

<span data-ttu-id="e6648-134">Minden szimulációs összetevő Azure Linux virtuális gépen futtatott Docker-tárolókban fut.</span><span class="sxs-lookup"><span data-stu-id="e6648-134">All simulation components run in Docker containers  hosted in an Azure Linux VM.</span></span> <span data-ttu-id="e6648-135">hello szimuláció konfigurált toorun nyolc szimulált éles sorok alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="e6648-135">hello simulation is configured toorun eight simulated production lines by default.</span></span>

## <a name="simulated-production-line"></a><span data-ttu-id="e6648-136">Szimulált gyártósor</span><span class="sxs-lookup"><span data-stu-id="e6648-136">Simulated production line</span></span>

<span data-ttu-id="e6648-137">A gyártósorok alkatrészeket gyártanak.</span><span class="sxs-lookup"><span data-stu-id="e6648-137">A production line manufactures parts.</span></span> <span data-ttu-id="e6648-138">Különböző állomásokból állnak: összeszerelő állomásból, tesztelő állomásból és csomagoló állomásból.</span><span class="sxs-lookup"><span data-stu-id="e6648-138">It is composed of different stations: an assembly station, a test station, and a packaging station.</span></span>

<span data-ttu-id="e6648-139">hello szimuláció fut, és frissíti a hello adatok hello OPC EE csomóponton keresztül elérhetővé.</span><span class="sxs-lookup"><span data-stu-id="e6648-139">hello simulation runs and updates hello data that is exposed through hello OPC UA nodes.</span></span> <span data-ttu-id="e6648-140">Az összes szimulált termelési sor állomás hello MES keresztül OPC EE által összehangolva. vannak.</span><span class="sxs-lookup"><span data-stu-id="e6648-140">All simulated production line stations are orchestrated by hello MES through OPC UA.</span></span>

## <a name="simulated-manufacturing-execution-system"></a><span data-ttu-id="e6648-141">Szimulált gyártási végrehajtó rendszer</span><span class="sxs-lookup"><span data-stu-id="e6648-141">Simulated manufacturing execution system</span></span>

<span data-ttu-id="e6648-142">hello MES figyeli minden állomáshoz hello termelési sorban OPC EE toodetect állomás állapotmódosítások keresztül.</span><span class="sxs-lookup"><span data-stu-id="e6648-142">hello MES monitors each station in hello production line through OPC UA toodetect station status changes.</span></span> <span data-ttu-id="e6648-143">Meghívja a OPC EE módszerek toocontrol hello állomás, és adja át a termék egy állomás toohello a következő mindaddig, amíg nem fejeződik be.</span><span class="sxs-lookup"><span data-stu-id="e6648-143">It calls OPC UA methods toocontrol hello stations and passes a product from one station toohello next until it is complete.</span></span>

## <a name="gateway-opc-publisher-module"></a><span data-ttu-id="e6648-144">Átjáró OPC kiadói modul</span><span class="sxs-lookup"><span data-stu-id="e6648-144">Gateway OPC publisher module</span></span>

<span data-ttu-id="e6648-145">OPC Publisher modul csatlakoztatja toohello állomás OPC EE-kiszolgálókat, és előfizet toohello OPC csomópontok toobe közzé.</span><span class="sxs-lookup"><span data-stu-id="e6648-145">OPC Publisher Module connects toohello station OPC UA servers and subscribes toohello OPC nodes toobe published.</span></span> <span data-ttu-id="e6648-146">hello modul hello csomópont adatait JSON formátumba konvertálja, titkosítja azokat, és elküldi azt OPC EE Pub/Sub üzenetekként tooIoT központ.</span><span class="sxs-lookup"><span data-stu-id="e6648-146">hello module converts hello node data into JSON format, encrypts it, and sends it tooIoT Hub as OPC UA Pub/Sub messages.</span></span>

<span data-ttu-id="e6648-147">hello OPC Publisher modul csak egy kimenő https (443) portra van szüksége, és a meglévő vállalati infrastruktúra is használhatók.</span><span class="sxs-lookup"><span data-stu-id="e6648-147">hello OPC Publisher module only requires an outbound https port (443) and can work with existing enterprise infrastructure.</span></span>

## <a name="gateway-opc-proxy-module"></a><span data-ttu-id="e6648-148">Átjáró OPC-proxymodul</span><span class="sxs-lookup"><span data-stu-id="e6648-148">Gateway OPC proxy module</span></span>

<span data-ttu-id="e6648-149">hello átjáró OPC EE Proxy modul bújtatja bináris OPC EE parancs és a vezérlő üzeneteket, és csak igényel az egy kimenő https (443-as) porton.</span><span class="sxs-lookup"><span data-stu-id="e6648-149">hello Gateway OPC UA Proxy Module tunnels binary OPC UA command and control messages and only requires an outbound https port (443).</span></span> <span data-ttu-id="e6648-150">Képes a meglévő vállalati infrastruktúrát használni, beleértve a webes proxykat is.</span><span class="sxs-lookup"><span data-stu-id="e6648-150">It can work with existing enterprise infrastructure, including Web Proxies.</span></span>

<span data-ttu-id="e6648-151">Az IoT Hub eszközhöz módszerek tootransfer packetized TCP/IP-adatok hello alkalmazás rétegben használ, és így biztosítja a végpont megbízható, az adattitkosítás és az SSL/TLS használatával integritása.</span><span class="sxs-lookup"><span data-stu-id="e6648-151">It uses IoT Hub Device methods tootransfer packetized TCP/IP data at hello application layer and thus ensures endpoint trust, data encryption, and integrity using SSL/TLS.</span></span>

<span data-ttu-id="e6648-152">hello OPC EE bináris protokoll maga hello proxyn keresztül történő továbbítását EE-hitelesítést és titkosítást használ.</span><span class="sxs-lookup"><span data-stu-id="e6648-152">hello OPC UA binary protocol relayed through hello proxy itself uses UA authentication and encryption.</span></span>

## <a name="azure-time-series-insights"></a><span data-ttu-id="e6648-153">Azure Time Series Insights</span><span class="sxs-lookup"><span data-stu-id="e6648-153">Azure Time Series Insights</span></span>

<span data-ttu-id="e6648-154">Átjáró OPC Publisher modul hello előfizet tooOPC EE server csomópontok toodetect változása hello adatértékekkel.</span><span class="sxs-lookup"><span data-stu-id="e6648-154">hello Gateway OPC Publisher Module subscribes tooOPC UA server nodes toodetect change in hello data values.</span></span> <span data-ttu-id="e6648-155">Ha egy adatok valamelyik hello csomópontokat észlelt, ez a modul ezután elküldi üzenetek tooAzure IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="e6648-155">If a data change is detected in one of hello nodes, this module then sends messages tooAzure IoT Hub.</span></span>

<span data-ttu-id="e6648-156">IoT-központot egy esemény forrás tooAzure ÁME biztosít.</span><span class="sxs-lookup"><span data-stu-id="e6648-156">IoT Hub provides an event source tooAzure TSI.</span></span> <span data-ttu-id="e6648-157">ÁME tárolók adatok alapján időbélyegeket 30 napig csatolt toohello üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="e6648-157">TSI stores data for 30 days based on timestamps attached toohello messages.</span></span> <span data-ttu-id="e6648-158">A adatok tartalma:</span><span class="sxs-lookup"><span data-stu-id="e6648-158">This data includes:</span></span>

* <span data-ttu-id="e6648-159">OPC UA ApplicationUri</span><span class="sxs-lookup"><span data-stu-id="e6648-159">OPC UA ApplicationUri</span></span>
* <span data-ttu-id="e6648-160">OPC UA NodeId</span><span class="sxs-lookup"><span data-stu-id="e6648-160">OPC UA NodeId</span></span>
* <span data-ttu-id="e6648-161">Hello csomópont értéke</span><span class="sxs-lookup"><span data-stu-id="e6648-161">Value of hello node</span></span>
* <span data-ttu-id="e6648-162">Forrás időbélyege</span><span class="sxs-lookup"><span data-stu-id="e6648-162">Source timestamp</span></span>
* <span data-ttu-id="e6648-163">OPC UA DisplayName</span><span class="sxs-lookup"><span data-stu-id="e6648-163">OPC UA DisplayName</span></span>

<span data-ttu-id="e6648-164">Jelenleg ÁME nem teszi lehetővé az ügyfelek mennyi ideig toocustomize kívánják tookeep hello adatait.</span><span class="sxs-lookup"><span data-stu-id="e6648-164">Currently, TSI does not allow customers toocustomize how long they wish tookeep hello data for.</span></span>

<span data-ttu-id="e6648-165">A TSI a csomópontadatok lekérdezését egy SearchSpan (Time.From, Time.To), az összesítést pedig az OPC UA ApplicationUri, az OPC UA NodeId vagy az OPC UA DisplayName attribútum alapján végzi.</span><span class="sxs-lookup"><span data-stu-id="e6648-165">TSI queries against node data using a SearchSpan (Time.From, Time.To) and aggregates by OPC UA ApplicationUri or OPC UA NodeId or OPC UA DisplayName.</span></span>

<span data-ttu-id="e6648-166">tooretrieve hello adatok hello OEE és KPI mérőműszer, és az adatsorozat hello idődiagramokat, adatok összesítve van az események, Sum, Avg, minimális és maximális számát.</span><span class="sxs-lookup"><span data-stu-id="e6648-166">tooretrieve hello data for hello OEE and KPI gauges, and hello time series charts, data is aggregated by count of events, Sum, Avg, Min, and Max.</span></span>

<span data-ttu-id="e6648-167">hello a time series egy másik folyamat használatával készített.</span><span class="sxs-lookup"><span data-stu-id="e6648-167">hello time series are built using a different process.</span></span> <span data-ttu-id="e6648-168">Állomás alap adatokból számított és hello alkalmazás feliratkozott hello topológia (éles sorok, előállítók, vállalati) átbuborékoltatni OEE és a KPI-k.</span><span class="sxs-lookup"><span data-stu-id="e6648-168">OEE and KPIs are calculated from station base data and bubbled up for hello topology (production lines, factories, enterprise) in hello application.</span></span>

<span data-ttu-id="e6648-169">Emellett OEE és a KPI-topológia a time series kiszámítása hello alkalmazásban, amikor készen áll a megjelenített timespan érték.</span><span class="sxs-lookup"><span data-stu-id="e6648-169">Additionally, time series for OEE and KPI topology is calculated in hello app, whenever a displayed timespan is ready.</span></span> <span data-ttu-id="e6648-170">Például a hello mai nézete teljes óránként frissül.</span><span class="sxs-lookup"><span data-stu-id="e6648-170">For example, hello day view is updated every full hour.</span></span>

<span data-ttu-id="e6648-171">hello idő adatsorozat nézet csomópont adatok közvetlenül a összesítést használ timespan ÁME származik.</span><span class="sxs-lookup"><span data-stu-id="e6648-171">hello time series view of node data comes directly from TSI using an aggregation for timespan.</span></span>

## <a name="iot-hub"></a><span data-ttu-id="e6648-172">IoT Hub</span><span class="sxs-lookup"><span data-stu-id="e6648-172">IoT Hub</span></span>
<span data-ttu-id="e6648-173">Hello [IoT-központ] [ lnk-IoT Hub] hello OPC Publisher modul hello felhőbe küldött adatokat fogad, és lehetővé teszi az elérhető toohello Azure ÁME szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="e6648-173">hello [IoT hub][lnk-IoT Hub] receives data sent from hello OPC Publisher Module into hello cloud and makes it available toohello Azure TSI service.</span></span> 

<span data-ttu-id="e6648-174">az IoT-központ hello hello megoldásban is:</span><span class="sxs-lookup"><span data-stu-id="e6648-174">hello IoT Hub in hello solution also:</span></span>
- <span data-ttu-id="e6648-175">Kezeli az identitásjegyzékhez hello azonosítók tároló összes OPC Publisher modult és minden OPC Proxy modul.</span><span class="sxs-lookup"><span data-stu-id="e6648-175">Maintains an identity registry that stores hello IDs for all OPC Publisher Module and all OPC Proxy Modules.</span></span>
- <span data-ttu-id="e6648-176">Használható szállítási csatorna hello OPC Proxy modul közötti kommunikáció kétirányú.</span><span class="sxs-lookup"><span data-stu-id="e6648-176">Is used as transport channel for bidirectional communication of hello OPC Proxy Module.</span></span>

## <a name="azure-storage"></a><span data-ttu-id="e6648-177">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="e6648-177">Azure Storage</span></span>
<span data-ttu-id="e6648-178">hello megoldás használ az Azure blob-tároló lemez tárolóként hello VM és toostore központi telepítésével kapcsolatos adatokat.</span><span class="sxs-lookup"><span data-stu-id="e6648-178">hello solution uses Azure blob storage as disk storage for hello VM and toostore deployment data.</span></span>

## <a name="web-app"></a><span data-ttu-id="e6648-179">Webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="e6648-179">Web app</span></span>
<span data-ttu-id="e6648-180">hello webalkalmazás üzembe az előre konfigurált hello megoldás részét képező egy integrált OPC EE-ügyfél, a riasztások feldolgozása és a telemetriai adatokat a képi megjelenítés foglalja magában.</span><span class="sxs-lookup"><span data-stu-id="e6648-180">hello web app deployed as part of hello preconfigured solution comprises of an integrated OPC UA client, alerts processing and telemetry visualization.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e6648-181">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e6648-181">Next steps</span></span>

<span data-ttu-id="e6648-182">Továbbra is a következő cikkek hello olvasásával Ismerkedés az IoT Suite:</span><span class="sxs-lookup"><span data-stu-id="e6648-182">You can continue getting started with IoT Suite by reading hello following articles:</span></span>

* <span data-ttu-id="e6648-183">[Engedélyek hello azureiotsuite.com webhelyen][lnk-permissions]</span><span class="sxs-lookup"><span data-stu-id="e6648-183">[Permissions on hello azureiotsuite.com site][lnk-permissions]</span></span>
* [<span data-ttu-id="e6648-184">Telepítsen egy átjárót a Windows vagy Linux csatlakoztatott hello előre konfigurált gyári megoldás</span><span class="sxs-lookup"><span data-stu-id="e6648-184">Deploy a gateway on Windows or Linux for hello connected factory preconfigured solution</span></span>](iot-suite-connected-factory-gateway-deployment.md)

[connected-factory-logical]:media/iot-suite-connected-factory-walkthrough/cf-logical-architecture.png

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-IoT Hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-OPC-UA-NET-Standard]:https://github.com/OPCFoundation/UA-.NETStandardLibrary
[lnk-Azure-IoT-Gateway]: https://github.com/azure/iot-edge
[lnk-permissions]: iot-suite-permissions.md
