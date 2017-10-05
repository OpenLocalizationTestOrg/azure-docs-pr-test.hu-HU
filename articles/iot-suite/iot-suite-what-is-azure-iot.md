---
title: "Azure-megoldások az eszközök internetes hálózatához (IoT Suite) | Microsoft Docs"
description: "Az IoT áttekintése az Azure-ban, beleértve egy minta megoldásarchitektúrát és azt, hogy miként kapcsolódik az Azure IoT Suite-hoz és az előre konfigurált megoldásokhoz."
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
ms.openlocfilehash: 320190488bb4c7b8192421f9dd50a5264f558584
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="azure-iot-suite"></a><span data-ttu-id="ae1ab-103">Azure IoT Suite</span><span class="sxs-lookup"><span data-stu-id="ae1ab-103">Azure IoT Suite</span></span>
<span data-ttu-id="ae1ab-104">A Microsoft Azure IoT Suite egy vállalati szintű megoldás, amellyel gyorsan teheti meg az első lépéseket az előre konfigurált megoldások bővíthető készletével.</span><span class="sxs-lookup"><span data-stu-id="ae1ab-104">The Microsoft Azure IoT Suite is an enterprise-grade solution that enables you to get started quickly through a set of extensible preconfigured solutions.</span></span> <span data-ttu-id="ae1ab-105">Ezek a megoldások olyan általános IoT-forgatókönyveket érintenek, mint a [távoli megfigyelés][lnk-preconfigured-solutions], a [prediktív karbantartás][lnk-predictive-maintenance] és a [csatlakoztatott gyár][lnk-connected-factory].</span><span class="sxs-lookup"><span data-stu-id="ae1ab-105">These solutions address common IoT scenarios, such as [remote monitoring][lnk-preconfigured-solutions], [predictive maintenance][lnk-predictive-maintenance], and [connected factory][lnk-connected-factory].</span></span> <span data-ttu-id="ae1ab-106">Ezek a megoldások az ebben a cikkben leírt IoT-megoldásarchitektúra megvalósításai.</span><span class="sxs-lookup"><span data-stu-id="ae1ab-106">These solutions are implementations of the IoT solution architecture outlined in this article.</span></span>

<span data-ttu-id="ae1ab-107">Az előre konfigurált megoldások teljes körű, működőképes és átfogó megoldások, amelyek a következőket tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="ae1ab-107">The preconfigured solutions are complete, working, end-to-end solutions that include:</span></span>

- <span data-ttu-id="ae1ab-108">Szimulált eszközök az elkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="ae1ab-108">Simulated devices to get you started.</span></span>
- <span data-ttu-id="ae1ab-109">Olyan előre konfigurált Azure-szolgáltatások, mint az [Azure IoT Hub][Azure IoT Hub], [Azure Event Hubs][Azure Event Hubs], [Azure Stream Analytics][Azure Stream Analytics], [Azure Machine Learning][Azure Machine Learning] és [Azure Storage][Azure storage].</span><span class="sxs-lookup"><span data-stu-id="ae1ab-109">Preconfigured Azure services such as [Azure IoT Hub][Azure IoT Hub], [Azure Event Hubs][Azure Event Hubs], [Azure Stream Analytics][Azure Stream Analytics], [Azure Machine Learning][Azure Machine Learning], and [Azure storage][Azure storage].</span></span>
- <span data-ttu-id="ae1ab-110">Megoldásspecifikus felügyeleti konzolok.</span><span class="sxs-lookup"><span data-stu-id="ae1ab-110">Solution-specific management consoles.</span></span>

<span data-ttu-id="ae1ab-111">Az előre konfigurált megoldások bevált, éles környezetekben használható kódot tartalmaznak, amelyek testreszabásával és bővítésével megvalósíthatja saját specifikus IoT-megoldásait.</span><span class="sxs-lookup"><span data-stu-id="ae1ab-111">The preconfigured solutions contain proven, production-ready code that you can customize and extend to implement your own specific IoT scenarios.</span></span>

<span data-ttu-id="ae1ab-112">A számos előre konfigurált megoldás által használt [Azure IoT Hub][Azure IoT Hub] szolgáltatás is érdekelheti.</span><span class="sxs-lookup"><span data-stu-id="ae1ab-112">You may also be interested in the [Azure IoT Hub][Azure IoT Hub] service that many of the preconfigured solutions use.</span></span> <span data-ttu-id="ae1ab-113">Az [Azure IoT Hub][Azure IoT Hub] biztonságos és megbízható, az előre konfigurált megoldásarchitektúrában használt kétirányú kommunikációt nyújt az eszközök és a felhő között.</span><span class="sxs-lookup"><span data-stu-id="ae1ab-113">[Azure IoT Hub][Azure IoT Hub] provides the secure and reliable bi-directional communications between devices and the cloud used in the preconfigured solution architecture.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ae1ab-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ae1ab-114">Next steps</span></span>
<span data-ttu-id="ae1ab-115">Ezekben a forrásanyagokban további információt talál az IoT Suite-ról és az előre konfigurált megoldásokról:</span><span class="sxs-lookup"><span data-stu-id="ae1ab-115">Explore these resources to continue learning about IoT Suite and the preconfigured solutions:</span></span>

* <span data-ttu-id="ae1ab-116">[Mi az az Azure IoT Suite?][lnk-whatissuite]</span><span class="sxs-lookup"><span data-stu-id="ae1ab-116">[What is Azure IoT Suite?][lnk-whatissuite]</span></span>
* <span data-ttu-id="ae1ab-117">[Mik az Azure IoT Suite előre konfigurált megoldásai?][lnk-whatarepreconfigured]</span><span class="sxs-lookup"><span data-stu-id="ae1ab-117">[What are the Azure IoT Suite preconfigured solutions?][lnk-whatarepreconfigured]</span></span>

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