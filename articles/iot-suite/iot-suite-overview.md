---
title: "aaaMicrosoft Azure IoT Suite – áttekintés |} Microsoft Docs"
description: "Hogyan nyújt az Azure IoT Suite a dolgot előkonfigurált megoldásokat toocollect, az eszközök internetes áttekintése elemzése, és adatok tárolására, adja meg a képi megjelenítések és integrálása más rendszerekkel."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 2d38d08a-4133-4e5c-8b28-f93cadb5df05
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 385025c5ec0d37c74689a928bc09e85b33439634
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-azure-iot-suite"></a><span data-ttu-id="6f84d-103">Az Azure IoT Suite áttekintése</span><span class="sxs-lookup"><span data-stu-id="6f84d-103">Overview of Azure IoT Suite</span></span>

<span data-ttu-id="6f84d-104">hello Azure internetes hálózata (IoT) szolgáltatások széles skálájával képességeket kínálnak.</span><span class="sxs-lookup"><span data-stu-id="6f84d-104">hello Azure internet of things (IoT) services offer a broad range of capabilities.</span></span> <span data-ttu-id="6f84d-105">Ezekkel a vállalati szintű szolgáltatásokkal a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="6f84d-105">These enterprise grade services enable you to:</span></span>

* <span data-ttu-id="6f84d-106">Adatok gyűjtése eszközökről</span><span class="sxs-lookup"><span data-stu-id="6f84d-106">Collect data from devices</span></span>
* <span data-ttu-id="6f84d-107">Streamek elemzése mozgás közben</span><span class="sxs-lookup"><span data-stu-id="6f84d-107">Analyze data streams in-motion</span></span>
* <span data-ttu-id="6f84d-108">Nagy adatkészletek tárolása és lekérdezése</span><span class="sxs-lookup"><span data-stu-id="6f84d-108">Store and query large data sets</span></span>
* <span data-ttu-id="6f84d-109">Valós idejű és előzményadatok megjelenítése</span><span class="sxs-lookup"><span data-stu-id="6f84d-109">Visualize both real-time and historical data</span></span>
* <span data-ttu-id="6f84d-110">Integrálás irodai háttérrendszerekkel</span><span class="sxs-lookup"><span data-stu-id="6f84d-110">Integrate with back-office systems</span></span>
* <span data-ttu-id="6f84d-111">Eszközök kezelése</span><span class="sxs-lookup"><span data-stu-id="6f84d-111">Manage your devices</span></span>

<span data-ttu-id="6f84d-112">toodeliver ezeket a képességeket, Azure IoT Suite csomagok egyszerre több Azure-szolgáltatások, az egyéni kiterjesztéseket tartalmazó *előre konfigurált megoldások*.</span><span class="sxs-lookup"><span data-stu-id="6f84d-112">toodeliver these capabilities, Azure IoT Suite packages together multiple Azure services with custom extensions as *preconfigured solutions*.</span></span> <span data-ttu-id="6f84d-113">Ezek előkonfigurált megoldásokat olyan alap megvalósítások annak a közös IoT megoldást, amelyek segítenek tooreduce hello időt, toodeliver az IoT-megoldásokhoz.</span><span class="sxs-lookup"><span data-stu-id="6f84d-113">These preconfigured solutions are base implementations of common IoT solution patterns that help tooreduce hello time you take toodeliver your IoT solutions.</span></span> <span data-ttu-id="6f84d-114">Hello segítségével [IoT szoftverfejlesztői készletek][lnk-sdks], testre szabhatja, és ezek a megoldások toomeet kiterjesztése a saját követelményeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="6f84d-114">Using hello [IoT software development kits][lnk-sdks], you can customize and extend these solutions toomeet your own requirements.</span></span> <span data-ttu-id="6f84d-115">Ezeket a megoldásokat példaként vagy sablonként is használhatja az új IoT-megoldások fejlesztésekor.</span><span class="sxs-lookup"><span data-stu-id="6f84d-115">You can also use these solutions as examples or templates when you are developing new IoT solutions.</span></span>

<span data-ttu-id="6f84d-116">hello következő videó biztosít egy bevezető tooAzure IoT Suite:</span><span class="sxs-lookup"><span data-stu-id="6f84d-116">hello following video provides an introduction tooAzure IoT Suite:</span></span>

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON309/player]
> 
> 

## <a name="azure-iot-services-in-azure-iot-suite"></a><span data-ttu-id="6f84d-117">Azure IoT-szolgáltatások az Azure IoT Suite-ban</span><span class="sxs-lookup"><span data-stu-id="6f84d-117">Azure IoT services in Azure IoT Suite</span></span>
<span data-ttu-id="6f84d-118">előre konfigurált hello megoldások általában a következő szolgáltatások hello használata:</span><span class="sxs-lookup"><span data-stu-id="6f84d-118">hello preconfigured solutions typically use hello following services:</span></span>

* <span data-ttu-id="6f84d-119">Alapvető tooAzure IoT Suite hello [Azure IoT Hub] [ lnk-iot-hub] szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6f84d-119">Core tooAzure IoT Suite is hello [Azure IoT Hub][lnk-iot-hub] service.</span></span> <span data-ttu-id="6f84d-120">Ez a szolgáltatás hello eszközről-a-felhőbe és felhő eszközre üzenetkezelési képességeket és tevékenységéért átjáró toohello hello, a felhő és egyéb fontos IoT Suite szolgáltatások hello.</span><span class="sxs-lookup"><span data-stu-id="6f84d-120">This service provides hello device-to-cloud and cloud-to-device messaging capabilities and acts as hello gateway toohello cloud and hello other key IoT Suite services.</span></span> <span data-ttu-id="6f84d-121">hello szolgáltatás lehetővé teszi az eszközök léptékű tooreceive üzeneteit, és parancsok tooyour eszközök küldése.</span><span class="sxs-lookup"><span data-stu-id="6f84d-121">hello service enables you tooreceive messages from your devices at scale, and send commands tooyour devices.</span></span> <span data-ttu-id="6f84d-122">hello szolgáltatás lehetővé teszi túl[az eszközök kezeléséhez][lnk-device-management].</span><span class="sxs-lookup"><span data-stu-id="6f84d-122">hello service also enables you too[manage your devices][lnk-device-management].</span></span> <span data-ttu-id="6f84d-123">Például konfigurálása, indítsa újra, vagy hajtsa végre a gyári beállításokat egy vagy több eszköz csatlakoztatott toohello központ.</span><span class="sxs-lookup"><span data-stu-id="6f84d-123">For example, you can configure, reboot, or perform a factory reset on one or more devices connected toohello hub.</span></span>
* <span data-ttu-id="6f84d-124">Az [Azure Stream Analytics][lnk-asa] a mozgásban lévő adatok elemzését nyújtja.</span><span class="sxs-lookup"><span data-stu-id="6f84d-124">[Azure Stream Analytics][lnk-asa] provides in-motion data analysis.</span></span> <span data-ttu-id="6f84d-125">IoT Suite használja a szolgáltatás tooprocess bejövő telemetriai, hajtsa végre az összesítő és események észleléséhez.</span><span class="sxs-lookup"><span data-stu-id="6f84d-125">IoT Suite uses this service tooprocess incoming telemetry, perform aggregation, and detect events.</span></span> <span data-ttu-id="6f84d-126">előre konfigurált hello megoldások is használja a stream analytics tooprocess tájékoztató üzeneteket adatok, például az eszközök metaadataira, illetve a parancs válaszát tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="6f84d-126">hello preconfigured solutions also use stream analytics tooprocess informational messages that contain data such as metadata or command responses from devices.</span></span> <span data-ttu-id="6f84d-127">hello megoldások használja a Stream Analytics tooprocess köszönőüzenetei az eszközökről, és az üzenetek tooother szolgáltatások biztosításához.</span><span class="sxs-lookup"><span data-stu-id="6f84d-127">hello solutions use Stream Analytics tooprocess hello messages from your devices and deliver those messages tooother services.</span></span>
* <span data-ttu-id="6f84d-128">[Az Azure Storage] [ lnk-azure-storage] és [Azure Cosmos DB] [ lnk-document-db] hello adatok tárolási lehetőségeket biztosít.</span><span class="sxs-lookup"><span data-stu-id="6f84d-128">[Azure Storage][lnk-azure-storage] and [Azure Cosmos DB][lnk-document-db] provide hello data storage capabilities.</span></span> <span data-ttu-id="6f84d-129">hello előkonfigurált megoldásokat használja a blob storage toostore telemetriai és toomake elérhetővé elemzés céljából.</span><span class="sxs-lookup"><span data-stu-id="6f84d-129">hello preconfigured solutions use blob storage toostore telemetry and toomake it available for analysis.</span></span> <span data-ttu-id="6f84d-130">hello megoldások Cosmos DB toostore eszköz metaadatait és hello Eszközkezelési lehetőségeivel foglalkozó hello megoldások engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="6f84d-130">hello solutions use Cosmos DB toostore device metadata and enable hello device management capabilities of hello solutions.</span></span>
* <span data-ttu-id="6f84d-131">[Az Azure Web Apps] [ lnk-web-apps] és [Microsoft Power BI] [ lnk-power-bi] adja meg a hello adatok képi megjelenítés képességeinek.</span><span class="sxs-lookup"><span data-stu-id="6f84d-131">[Azure Web Apps][lnk-web-apps] and [Microsoft Power BI][lnk-power-bi] provide hello data visualization capabilities.</span></span> <span data-ttu-id="6f84d-132">Power BI hello rugalmasságával lehetővé teszi a tooquickly létre saját IoT Suite adatokat használó interaktív irányítópultokat.</span><span class="sxs-lookup"><span data-stu-id="6f84d-132">hello flexibility of Power BI enables you tooquickly build your own interactive dashboards that use IoT Suite data.</span></span>

<span data-ttu-id="6f84d-133">Egy tipikus IoT-megoldás hello architektúrájának áttekintését lásd: [Microsoft Azure és az eszközök internetes hálózatát (IoT) hello][iot-suite-what-is-azure-iot].</span><span class="sxs-lookup"><span data-stu-id="6f84d-133">For an overview of hello architecture of a typical IoT solution, see [Microsoft Azure and hello Internet of Things (IoT)][iot-suite-what-is-azure-iot].</span></span>

## <a name="preconfigured-solutions"></a><span data-ttu-id="6f84d-134">Előre konfigurált megoldások</span><span class="sxs-lookup"><span data-stu-id="6f84d-134">Preconfigured solutions</span></span>

<span data-ttu-id="6f84d-135">Az IoT Suite előkonfigurált megoldásokat tartalmazza, hogy Ön tooquickly Ismerkedés a engedélyezése és az általános IoT-forgatókönyvek esetén, mint például hello tooexplore:</span><span class="sxs-lookup"><span data-stu-id="6f84d-135">IoT Suite includes preconfigured solutions that enable you tooquickly get started with and tooexplore hello common IoT scenarios, such as:</span></span>

* <span data-ttu-id="6f84d-136">Távoli figyelés</span><span class="sxs-lookup"><span data-stu-id="6f84d-136">Remote monitoring</span></span>
* <span data-ttu-id="6f84d-137">Prediktív karbantartás</span><span class="sxs-lookup"><span data-stu-id="6f84d-137">Predictive maintenance</span></span>
* <span data-ttu-id="6f84d-138">Csatlakoztatott gyár</span><span class="sxs-lookup"><span data-stu-id="6f84d-138">Connected factory</span></span>

<span data-ttu-id="6f84d-139">Ezen megoldások tooyour Azure-előfizetés telepítheti, és futtassa újra a teljes, végpontok közötti IoT-forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="6f84d-139">You can deploy these solutions tooyour Azure subscription and then run a complete, end-to-end IoT scenario.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6f84d-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6f84d-140">Next steps</span></span>

<span data-ttu-id="6f84d-141">Most, hogy az IoT Suite teendők áttekintése rendelkezik, és Mik a fő összetevőit, többet is megtudhat az IoT Suite előre konfigurált hello megoldásokról.</span><span class="sxs-lookup"><span data-stu-id="6f84d-141">Now that you have an overview of what IoT Suite can do and what are its main components, you can learn more about hello preconfigured solutions in IoT Suite.</span></span> <span data-ttu-id="6f84d-142">További információkért lásd: [hello Azure IoT Mik előre konfigurált megoldások?][lnk-what-are-preconfig]</span><span class="sxs-lookup"><span data-stu-id="6f84d-142">For more information, see [What are hello Azure IoT preconfigured solutions?][lnk-what-are-preconfig]</span></span>

[lnk-sdks]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-azure-storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-document-db]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-power-bi]: https://powerbi.microsoft.com/
[lnk-web-apps]: https://azure.microsoft.com/documentation/services/app-service/web/
[iot-suite-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-what-are-preconfig]: iot-suite-what-are-preconfigured-solutions.md
[lnk-device-management]: ../iot-hub/iot-hub-device-management-overview.md
