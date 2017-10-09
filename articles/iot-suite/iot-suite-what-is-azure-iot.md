---
title: "az eszközök internetes hálózatát (IoT Suite) aaaAzure megoldások |} Microsoft Docs"
description: "Például egy minta-megoldásarchitektúra és az IoT Suite tooAzure, és előre konfigurált hello megoldások kapcsolódására Azure IoT áttekintése."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 437d2655-896f-4a9e-a4a8-b864790d3ef8
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: e527ca3f7541c84fbd6abc99ee38792468f88644
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="azure-iot-suite"></a><span data-ttu-id="177bb-103">Azure IoT Suite</span><span class="sxs-lookup"><span data-stu-id="177bb-103">Azure IoT Suite</span></span>
<span data-ttu-id="177bb-104">Microsoft Azure IoT Suite hello egy vállalati szintű megoldás, amely lehetővé teszi, hogy a tooget, bővíthető előkonfigurált megoldásokat keresztül gyorsan lépéseket.</span><span class="sxs-lookup"><span data-stu-id="177bb-104">hello Microsoft Azure IoT Suite is an enterprise-grade solution that enables you tooget started quickly through a set of extensible preconfigured solutions.</span></span> <span data-ttu-id="177bb-105">Ezek a megoldások olyan általános IoT-forgatókönyveket érintenek, mint a [távoli megfigyelés][lnk-preconfigured-solutions], a [prediktív karbantartás][lnk-predictive-maintenance] és a [csatlakoztatott gyár][lnk-connected-factory].</span><span class="sxs-lookup"><span data-stu-id="177bb-105">These solutions address common IoT scenarios, such as [remote monitoring][lnk-preconfigured-solutions], [predictive maintenance][lnk-predictive-maintenance], and [connected factory][lnk-connected-factory].</span></span> <span data-ttu-id="177bb-106">Ezek a megoldások olyan IoT-megoldásarchitektúra a cikkben ismertetett hello implementációja.</span><span class="sxs-lookup"><span data-stu-id="177bb-106">These solutions are implementations of hello IoT solution architecture outlined in this article.</span></span>

<span data-ttu-id="177bb-107">hello előkonfigurált megoldásokat teljesülnek, dolgozik, végpontok közötti megoldások, amelyek tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="177bb-107">hello preconfigured solutions are complete, working, end-to-end solutions that include:</span></span>

- <span data-ttu-id="177bb-108">A szimulált eszköz tooget-t elindította.</span><span class="sxs-lookup"><span data-stu-id="177bb-108">Simulated devices tooget you started.</span></span>
- <span data-ttu-id="177bb-109">Olyan előre konfigurált Azure-szolgáltatások, mint az [Azure IoT Hub][Azure IoT Hub], [Azure Event Hubs][Azure Event Hubs], [Azure Stream Analytics][Azure Stream Analytics], [Azure Machine Learning][Azure Machine Learning] és [Azure Storage][Azure storage].</span><span class="sxs-lookup"><span data-stu-id="177bb-109">Preconfigured Azure services such as [Azure IoT Hub][Azure IoT Hub], [Azure Event Hubs][Azure Event Hubs], [Azure Stream Analytics][Azure Stream Analytics], [Azure Machine Learning][Azure Machine Learning], and [Azure storage][Azure storage].</span></span>
- <span data-ttu-id="177bb-110">Megoldásspecifikus felügyeleti konzolok.</span><span class="sxs-lookup"><span data-stu-id="177bb-110">Solution-specific management consoles.</span></span>

<span data-ttu-id="177bb-111">előre konfigurált hello megoldások bevált, éles használatra kész kód testreszabása és kiterjesztése tooimplement saját adott IoT-forgatókönyvek esetén is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="177bb-111">hello preconfigured solutions contain proven, production-ready code that you can customize and extend tooimplement your own specific IoT scenarios.</span></span>

<span data-ttu-id="177bb-112">Bizonyos is érdeklődik hello [Azure IoT Hub] [ Azure IoT Hub] számos előre konfigurált hello megoldásokat használó szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="177bb-112">You may also be interested in hello [Azure IoT Hub][Azure IoT Hub] service that many of hello preconfigured solutions use.</span></span> <span data-ttu-id="177bb-113">[Az Azure IoT Hub] [ Azure IoT Hub] lehetővé teszi az hello biztonságos és megbízható kétirányú kommunikációt eszközök és az előre konfigurált hello megoldásarchitektúra használt hello felhő között.</span><span class="sxs-lookup"><span data-stu-id="177bb-113">[Azure IoT Hub][Azure IoT Hub] provides hello secure and reliable bi-directional communications between devices and hello cloud used in hello preconfigured solution architecture.</span></span>

## <a name="next-steps"></a><span data-ttu-id="177bb-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="177bb-114">Next steps</span></span>
<span data-ttu-id="177bb-115">Ezen erőforrások toocontinue IoT Suite megtanulni vizsgálatát, és előkonfigurált megoldásokat hello:</span><span class="sxs-lookup"><span data-stu-id="177bb-115">Explore these resources toocontinue learning about IoT Suite and hello preconfigured solutions:</span></span>

* <span data-ttu-id="177bb-116">[Mi az az Azure IoT Suite?][lnk-whatissuite]</span><span class="sxs-lookup"><span data-stu-id="177bb-116">[What is Azure IoT Suite?][lnk-whatissuite]</span></span>
* <span data-ttu-id="177bb-117">[Mik azok a hello Azure IoT Suite előre konfigurált megoldások?][lnk-whatarepreconfigured]</span><span class="sxs-lookup"><span data-stu-id="177bb-117">[What are hello Azure IoT Suite preconfigured solutions?][lnk-whatarepreconfigured]</span></span>

[lnk-whatissuite]: iot-suite-overview.md
[lnk-whatarepreconfigured]: iot-suite-what-are-preconfigured-solutions.md

[lnk-preconfigured-solutions]: iot-suite-getstarted-preconfigured-solutions.md
[Azure IoT Hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[Azure Event Hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Azure Stream Analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[Azure Machine Learning]: https://azure.microsoft.com/documentation/services/machine-learning/
[Azure storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-connected-factory]: iot-suite-connected-factory-overview.md