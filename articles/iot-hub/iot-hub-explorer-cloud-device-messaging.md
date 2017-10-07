---
title: "az IOT hubbal-explorer üzenetküldési aaaManage Azure IoT Hub felhő eszköz |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello IOT hubbal-explorer CLI eszköz toomonitor toocloud (D2C) üzeneteket, és Azure IoT Hub felhő toodevice (C2D) üzenetek küldése."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT hubbal explorer, a felhő eszköz üzenetküldés, iot hub felhő toodevice, a felhő toodevice üzenetkezelés"
ms.assetid: 04521658-35d3-4503-ae48-51d6ad3c62cc
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: xshi
ms.openlocfilehash: 741383b5631714cc2fef3309541fd2117aa7824c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-iothub-explorer-toosend-and-receive-messages-between-your-device-and-iot-hub"></a>Használja az IOT hubbal-explorer toosend és az eszköz és az IoT-központ között üzeneteket fogadni

![Végpontok közötti diagramja](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[IOT hubbal-explorer](https://github.com/azure/iothub-explorer) rendelkezik, amely egyszerűbbé teszi az IoT-központ felügyeleti néhány olyan parancsok. Ez az oktatóanyag témája módja toouse IOT hubbal-explorer toosend és az eszköz és az IoT hub között üzeneteket fogadni.

## <a name="what-you-will-learn"></a>Amiről tanulni fog

Megismerheti, hogyan toouse IOT hubbal-explorer toomonitor eszközről a felhőbe üzenetek és toosend felhő eszközre üzeneteket. Eszköz a felhőbe küldött üzeneteket lehet, hogy az eszköz összegyűjti, és ezután elküldi az IoT-központ tooyour érzékelőadatait. Felhő-eszközre küldött üzenetek oka az lehet, hogy az IoT hub küld tooyour eszköz tooblink, amelyek csatlakoztatott tooyour eszköz LED parancsok.

## <a name="what-you-will-do"></a>Mit fog

- Használja az IOT hubbal-explorer toomonitor eszköz-a-felhőbe küldött üzeneteket.
- Használja az IOT hubbal-explorer toosend felhő eszközre üzeneteket.

## <a name="what-you-need"></a>Mi szükséges

- Az oktatóanyag [beállítani az eszközét](iot-hub-raspberry-pi-kit-node-get-started.md) fejeződött be, amely hozzá van rendelve hello követelményeknek:
  - Aktív Azure-előfizetés.
  - Az előfizetéshez tartozó Azure IoT hub.
  - Egy ügyfélalkalmazás által küldött üzenetek tooyour Azure IoT-központot.
- IOT hubbal-explorer. ([Telepítse az IOT hubbal-explorer](https://github.com/azure/iothub-explorer))

## <a name="monitor-device-to-cloud-messages"></a>Eszköz a felhőbe küldött üzeneteket figyelése

az eszköz tooyour IoT hub érkező üzenetek toomonitor kövesse az alábbi lépéseket:

1. Nyissa meg a konzol ablakot.
1. Futtassa a következő parancs hello:

   ```bash
   iothub-explorer monitor-events <device-id> --login "<IoTHubConnectionString>"
   ```

   > [!Note]
   > Első `<device-id>` és `<IoTHubConnectionString>` az IoT hub a. Győződjön meg arról, hogy hello előző oktatóanyag befejezése után. Vagy megpróbálhatja toouse `iothub-explorer monitor-events <device-id> --login "HostName=<my-hub>.azure-devices.net;SharedAccessKeyName=<my-policy>;SharedAccessKey=<my-policy-key>"` Ha `HostName`, `SharedAccessKeyName` és `SharedAccessKey`.

## <a name="send-cloud-to-device-messages"></a>Üzenetküldés a felhőből az eszközökre

egy üzenetet az IoT hub tooyour eszközről toosend kövesse az alábbi lépéseket:

1. Nyissa meg a konzol ablakot.
1. Az IoT hub a munkamenet indításához hello a következő parancs futtatásával:

   ```bash
   iothub-explorer login `<IoTHubConnectionString>`
   ```

1. Egy üzenet tooyour eszköz küldése hello a következő parancs futtatásával:

   ```bash
   iothub-explorer send <device-id> <message>
   ```

hello parancs villogjon-hello LED-jét, csatlakoztatott tooyour eszközt, és elküldi azt hello üzenet tooyour eszköz.

> [!Note]
> Nincs szükség a hello eszköz toosend egy külön ack parancs hátsó tooyour IoT-központ hello üzenet fogadásakor.

## <a name="next-steps"></a>Következő lépések

Hogy mér megismerte hogyan toomonitor eszköz-felhő üzeneteket, és az IoT-eszközök és az Azure IoT-központ között felhő eszközre üzeneteket küldeni.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
