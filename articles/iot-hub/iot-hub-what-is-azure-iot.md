---
title: "az eszközök internetes hálózatát (IoT Suite) aaaAzure megoldások |} Microsoft Docs"
description: "Egy minta IoT-megoldásarchitektúra és toodevices, kapcsolódására áttekintése hello Azure IoT-központ szolgáltatás, Azure IoT eszközoldali SDK-k, Azure IoT szolgáltatás SDK-k és más Azure-szolgáltatásokkal."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: a859e379-dca7-42fa-bdf6-1125c86ad140
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.openlocfilehash: 2d934e3f988c530de6a242869c021712d2aa1576
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a>Következő lépések

Az Azure IoT Hub olyan Azure-szolgáltatás, amely biztonságos és megbízható kétirányú kommunikációt tesz lehetővé a megoldás háttérrendszere és több millió eszköz között. Ez lehetővé teszi, hogy a hello megoldás háttérrendszeréhez:

* Az eszközök telemetriai adatainak lekérése távolról.
* Az eszközök tooa adatfolyam eseményfeldolgozóhoz útvonal adatait.
* Fájlfeltöltés az eszközökről.
* Felhő-eszközre küldött üzenetek küldése toospecific eszközök.

Az IoT-központ tooimplement saját megoldás háttérrendszere is használhatja. Az IoT-központ tartalmazza továbbá egy identitás használt beállításjegyzék tooprovision eszközök, a hitelesítő adatokat és a jogok tooconnect toohello IoT-központot. toolearn IoT Hub kapcsolatos további információkért lásd: [Mi az az IoT-központ?] [lnk-iot-hub].

toolearn hogyan Azure IoT Hub szabványalapú kezelés lehetővé teszi, az Ön tooremotely kezelése, konfigurálása és az eszközök frissítését lásd [IoT-központ az eszközkezelés áttekintése][lnk-device-management].

tooimplement ügyfélalkalmazások számos különböző eszközplatformok hardverek és operációs rendszereket, hello Azure IoT-eszközök SDK-k is használhatja. hello eszköz SDK-k, amelyek egyszerűbbé teszik a telemetriai adatok tooan IoT hub és fogadás felhő eszközre üzenetek küldése szalagtárak tartalmazza. Ha hello eszközzel SDK-k, az IoT hubbal választhat több hálózati protokollok toocommunicate. toolearn több, tekintse meg a hello [SDK-k eszközére vonatkozó információt][lnk-device-sdks].

Néhány kódot és az egyes minták futtatása elindult tooget lásd: hello [Ismerkedés az IoT-központ] [ lnk-getstarted] oktatóanyag.

Az [Azure IoT Suite][lnk-iot-suite] is hasznos lehet, amely egy előre konfigurált megoldásokat tartalmazó gyűjtemény. Az IoT Suite lehetővé teszi, hogy a tooget, gyorsan, és a méret IoT projektek tooaddress általános IoT-forgatókönyvek esetén – például távoli figyelés, az eszközök kezelése és a prediktív karbantartási.

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md
