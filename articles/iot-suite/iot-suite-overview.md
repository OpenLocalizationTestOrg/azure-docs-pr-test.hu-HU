---
title: "A Microsoft Azure IoT Suite áttekintése | Microsoft Docs"
description: "A cikk áttekinti, hogyan biztosít az Azure IoT Suite az eszközök internetes hálózatán alapuló, előre konfigurált megoldásokat adatok gyűjtéséhez, elemzéséhez és tárolásához, megjelenítések biztosításához, valamint a más rendszerekkel való integrációhoz."
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
ms.openlocfilehash: bfa8dbbd0b1d943a9eb7a042df0bac25189d9ac9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="overview-of-azure-iot-suite"></a><span data-ttu-id="f1e40-103">Az Azure IoT Suite áttekintése</span><span class="sxs-lookup"><span data-stu-id="f1e40-103">Overview of Azure IoT Suite</span></span>

<span data-ttu-id="f1e40-104">Az Azure eszközök internetes hálózata (IoT) szolgáltatások a képességek széles választékát nyújtják.</span><span class="sxs-lookup"><span data-stu-id="f1e40-104">The Azure internet of things (IoT) services offer a broad range of capabilities.</span></span> <span data-ttu-id="f1e40-105">Ezekkel a vállalati szintű szolgáltatásokkal a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="f1e40-105">These enterprise grade services enable you to:</span></span>

* <span data-ttu-id="f1e40-106">Adatok gyűjtése eszközökről</span><span class="sxs-lookup"><span data-stu-id="f1e40-106">Collect data from devices</span></span>
* <span data-ttu-id="f1e40-107">Streamek elemzése mozgás közben</span><span class="sxs-lookup"><span data-stu-id="f1e40-107">Analyze data streams in-motion</span></span>
* <span data-ttu-id="f1e40-108">Nagy adatkészletek tárolása és lekérdezése</span><span class="sxs-lookup"><span data-stu-id="f1e40-108">Store and query large data sets</span></span>
* <span data-ttu-id="f1e40-109">Valós idejű és előzményadatok megjelenítése</span><span class="sxs-lookup"><span data-stu-id="f1e40-109">Visualize both real-time and historical data</span></span>
* <span data-ttu-id="f1e40-110">Integrálás irodai háttérrendszerekkel</span><span class="sxs-lookup"><span data-stu-id="f1e40-110">Integrate with back-office systems</span></span>
* <span data-ttu-id="f1e40-111">Eszközök kezelése</span><span class="sxs-lookup"><span data-stu-id="f1e40-111">Manage your devices</span></span>

<span data-ttu-id="f1e40-112">Ezen képességek biztosítása érdekében az Azure IoT Suite több Azure-szolgáltatást kínál egyéni bővítményekkel *előre konfigurált megoldásokként*.</span><span class="sxs-lookup"><span data-stu-id="f1e40-112">To deliver these capabilities, Azure IoT Suite packages together multiple Azure services with custom extensions as *preconfigured solutions*.</span></span> <span data-ttu-id="f1e40-113">Ezek az előre konfigurált megoldások az általános IoT -megoldásminták alapszintű megvalósításai, amelyek segítenek csökkenteni az IoT-megoldások szolgáltatásához szükséges időt.</span><span class="sxs-lookup"><span data-stu-id="f1e40-113">These preconfigured solutions are base implementations of common IoT solution patterns that help to reduce the time you take to deliver your IoT solutions.</span></span> <span data-ttu-id="f1e40-114">Az [IoT szoftverfejlesztői készletekkel][lnk-sdks] testreszabhatja és kibővítheti ezeket a megoldásokat, hogy megfeleljenek a saját követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="f1e40-114">Using the [IoT software development kits][lnk-sdks], you can customize and extend these solutions to meet your own requirements.</span></span> <span data-ttu-id="f1e40-115">Ezeket a megoldásokat példaként vagy sablonként is használhatja az új IoT-megoldások fejlesztésekor.</span><span class="sxs-lookup"><span data-stu-id="f1e40-115">You can also use these solutions as examples or templates when you are developing new IoT solutions.</span></span>

<span data-ttu-id="f1e40-116">A következő videó az Azure IoT Suite bemutatását tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="f1e40-116">The following video provides an introduction to Azure IoT Suite:</span></span>

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON309/player]
> 
> 

## <a name="azure-iot-services-in-azure-iot-suite"></a><span data-ttu-id="f1e40-117">Azure IoT-szolgáltatások az Azure IoT Suite-ban</span><span class="sxs-lookup"><span data-stu-id="f1e40-117">Azure IoT services in Azure IoT Suite</span></span>
<span data-ttu-id="f1e40-118">Az előre konfigurált megoldások általában a következő szolgáltatásokat használják:</span><span class="sxs-lookup"><span data-stu-id="f1e40-118">The preconfigured solutions typically use the following services:</span></span>

* <span data-ttu-id="f1e40-119">Az Azure IoT Suite középpontjában az [Azure IoT Hub][lnk-iot-hub] szolgáltatás áll.</span><span class="sxs-lookup"><span data-stu-id="f1e40-119">Core to Azure IoT Suite is the [Azure IoT Hub][lnk-iot-hub] service.</span></span> <span data-ttu-id="f1e40-120">Ez a szolgáltatás az eszközökről a felhőbe irányuló és a felhőből az eszközre irányuló üzenetküldési képességeket nyújt, és a felhő és a többi fontos IoT-szolgáltatás átjárójaként működik.</span><span class="sxs-lookup"><span data-stu-id="f1e40-120">This service provides the device-to-cloud and cloud-to-device messaging capabilities and acts as the gateway to the cloud and the other key IoT Suite services.</span></span> <span data-ttu-id="f1e40-121">A szolgáltatás lehetővé teszi, hogy nagy mennyiségű üzenetet fogadjon az eszközökről, és parancsokat küldjön az eszközökre.</span><span class="sxs-lookup"><span data-stu-id="f1e40-121">The service enables you to receive messages from your devices at scale, and send commands to your devices.</span></span> <span data-ttu-id="f1e40-122">A szolgáltatással [felügyelheti is az eszközöket][lnk-device-management].</span><span class="sxs-lookup"><span data-stu-id="f1e40-122">The service also enables you to [manage your devices][lnk-device-management].</span></span> <span data-ttu-id="f1e40-123">Konfigurálhat vagy újraindíthat például az IoT Hubhoz csatlakozó egy vagy több eszközt, illetve visszaállíthatja rajtuk a gyári beállításokat.</span><span class="sxs-lookup"><span data-stu-id="f1e40-123">For example, you can configure, reboot, or perform a factory reset on one or more devices connected to the hub.</span></span>
* <span data-ttu-id="f1e40-124">Az [Azure Stream Analytics][lnk-asa] a mozgásban lévő adatok elemzését nyújtja.</span><span class="sxs-lookup"><span data-stu-id="f1e40-124">[Azure Stream Analytics][lnk-asa] provides in-motion data analysis.</span></span> <span data-ttu-id="f1e40-125">Az IoT Suite ezzel a szolgáltatással dolgozza fel a bejövő telemetriát, végez összesítést és észlel eseményeket.</span><span class="sxs-lookup"><span data-stu-id="f1e40-125">IoT Suite uses this service to process incoming telemetry, perform aggregation, and detect events.</span></span> <span data-ttu-id="f1e40-126">Az előre konfigurált megoldások is a Stream Analytics eszközzel dolgozzák fel azokat a tájékoztató üzeneteket, amelyek például metaadatokat vagy eszközök parancsválaszait tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="f1e40-126">The preconfigured solutions also use stream analytics to process informational messages that contain data such as metadata or command responses from devices.</span></span> <span data-ttu-id="f1e40-127">A megoldások a Stream Analytics használatával dolgozzák fel az eszközökről kapott üzeneteket, és ezeket az üzeneteket más szolgáltatásokhoz küldik.</span><span class="sxs-lookup"><span data-stu-id="f1e40-127">The solutions use Stream Analytics to process the messages from your devices and deliver those messages to other services.</span></span>
* <span data-ttu-id="f1e40-128">Az [Azure Storage][lnk-azure-storage] és az [Azure Cosmos DB][lnk-document-db] biztosítja az adattárolási képességeket.</span><span class="sxs-lookup"><span data-stu-id="f1e40-128">[Azure Storage][lnk-azure-storage] and [Azure Cosmos DB][lnk-document-db] provide the data storage capabilities.</span></span> <span data-ttu-id="f1e40-129">Az előre konfigurált megoldások a Blob Storage-ban tárolják és teszik elérhetővé elemzésre a telemetriát.</span><span class="sxs-lookup"><span data-stu-id="f1e40-129">The preconfigured solutions use blob storage to store telemetry and to make it available for analysis.</span></span> <span data-ttu-id="f1e40-130">A megoldások a Cosmos DB-ben tárolják az eszközök metaadatait, és engedélyezik a megoldások eszközfelügyeleti képességeit.</span><span class="sxs-lookup"><span data-stu-id="f1e40-130">The solutions use Cosmos DB to store device metadata and enable the device management capabilities of the solutions.</span></span>
* <span data-ttu-id="f1e40-131">Az [Azure Web Apps][lnk-web-apps] és a [Microsoft Power BI][lnk-power-bi] biztosítja az adatmegjelenítési képességeket.</span><span class="sxs-lookup"><span data-stu-id="f1e40-131">[Azure Web Apps][lnk-web-apps] and [Microsoft Power BI][lnk-power-bi] provide the data visualization capabilities.</span></span> <span data-ttu-id="f1e40-132">A Power BI rugalmasságával gyorsan építheti fel a saját, IoT Suite-adatokat használó interaktív irányítópultjait.</span><span class="sxs-lookup"><span data-stu-id="f1e40-132">The flexibility of Power BI enables you to quickly build your own interactive dashboards that use IoT Suite data.</span></span>

<span data-ttu-id="f1e40-133">A tipikus IoT-megoldások architektúrájának áttekintéséért lásd a [Microsoft Azure-t és az eszközök internetes hálózatát (IoT)][iot-suite-what-is-azure-iot] ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="f1e40-133">For an overview of the architecture of a typical IoT solution, see [Microsoft Azure and the Internet of Things (IoT)][iot-suite-what-is-azure-iot].</span></span>

## <a name="preconfigured-solutions"></a><span data-ttu-id="f1e40-134">Előre konfigurált megoldások</span><span class="sxs-lookup"><span data-stu-id="f1e40-134">Preconfigured solutions</span></span>

<span data-ttu-id="f1e40-135">Az IoT Suite előre konfigurált megoldásokat tartalmaz, amelyekkel gyorsan teheti meg az első lépéseket és fedezheti fel az általános IoT-forgatókönyveket, például a következőket:</span><span class="sxs-lookup"><span data-stu-id="f1e40-135">IoT Suite includes preconfigured solutions that enable you to quickly get started with and to explore the common IoT scenarios, such as:</span></span>

* <span data-ttu-id="f1e40-136">Távoli figyelés</span><span class="sxs-lookup"><span data-stu-id="f1e40-136">Remote monitoring</span></span>
* <span data-ttu-id="f1e40-137">Prediktív karbantartás</span><span class="sxs-lookup"><span data-stu-id="f1e40-137">Predictive maintenance</span></span>
* <span data-ttu-id="f1e40-138">Csatlakoztatott gyár</span><span class="sxs-lookup"><span data-stu-id="f1e40-138">Connected factory</span></span>

<span data-ttu-id="f1e40-139">Ezeket a megoldásokat az Azure-előfizetésre telepítheti, majd egy teljes körű IoT-forgatókönyvet futtathat.</span><span class="sxs-lookup"><span data-stu-id="f1e40-139">You can deploy these solutions to your Azure subscription and then run a complete, end-to-end IoT scenario.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f1e40-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f1e40-140">Next steps</span></span>

<span data-ttu-id="f1e40-141">Most, hogy áttekintette az IoT Suite képességeit és fő összetevőit, többet is megtudhat az IoT Suite előre konfigurált megoldásairól.</span><span class="sxs-lookup"><span data-stu-id="f1e40-141">Now that you have an overview of what IoT Suite can do and what are its main components, you can learn more about the preconfigured solutions in IoT Suite.</span></span> <span data-ttu-id="f1e40-142">További információkért lásd [az Azure IoT előre konfigurált megoldásait][lnk-what-are-preconfig] ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="f1e40-142">For more information, see [What are the Azure IoT preconfigured solutions?][lnk-what-are-preconfig]</span></span>

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
