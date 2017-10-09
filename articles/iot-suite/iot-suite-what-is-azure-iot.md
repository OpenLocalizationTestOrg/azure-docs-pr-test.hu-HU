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

## <a name="azure-iot-suite"></a>Azure IoT Suite
Microsoft Azure IoT Suite hello egy vállalati szintű megoldás, amely lehetővé teszi, hogy a tooget, bővíthető előkonfigurált megoldásokat keresztül gyorsan lépéseket. Ezek a megoldások olyan általános IoT-forgatókönyveket érintenek, mint a [távoli megfigyelés][lnk-preconfigured-solutions], a [prediktív karbantartás][lnk-predictive-maintenance] és a [csatlakoztatott gyár][lnk-connected-factory]. Ezek a megoldások olyan IoT-megoldásarchitektúra a cikkben ismertetett hello implementációja.

hello előkonfigurált megoldásokat teljesülnek, dolgozik, végpontok közötti megoldások, amelyek tartalmazzák:

- A szimulált eszköz tooget-t elindította.
- Olyan előre konfigurált Azure-szolgáltatások, mint az [Azure IoT Hub][Azure IoT Hub], [Azure Event Hubs][Azure Event Hubs], [Azure Stream Analytics][Azure Stream Analytics], [Azure Machine Learning][Azure Machine Learning] és [Azure Storage][Azure storage].
- Megoldásspecifikus felügyeleti konzolok.

előre konfigurált hello megoldások bevált, éles használatra kész kód testreszabása és kiterjesztése tooimplement saját adott IoT-forgatókönyvek esetén is tartalmaz.

Bizonyos is érdeklődik hello [Azure IoT Hub] [ Azure IoT Hub] számos előre konfigurált hello megoldásokat használó szolgáltatás. [Az Azure IoT Hub] [ Azure IoT Hub] lehetővé teszi az hello biztonságos és megbízható kétirányú kommunikációt eszközök és az előre konfigurált hello megoldásarchitektúra használt hello felhő között.

## <a name="next-steps"></a>Következő lépések
Ezen erőforrások toocontinue IoT Suite megtanulni vizsgálatát, és előkonfigurált megoldásokat hello:

* [Mi az az Azure IoT Suite?][lnk-whatissuite]
* [Mik azok a hello Azure IoT Suite előre konfigurált megoldások?][lnk-whatarepreconfigured]

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