---
title: 'Az Azure IoT - lecke 3 Connect Arduino (C): Table storage |} Microsoft Docs'
description: "Mivel az Azure Table storage írás az eszközről a felhőbe üzenetek figyelése"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "a felhő, felhőalapú adatgyűjtés, iot felhőalapú szolgáltatás, az iot-adatok adatok"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 386083e0-0dbb-48c0-9ac2-4f8fb4590772
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 29fb97f5cf0669acb9e68d8a829294ee64c9cf04
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="read-messages-persisted-in-azure-storage"></a>Az Azure Storage megőrzött üzenetek olvasása
## <a name="what-you-will-do"></a>Mit fog
Az eszközről a felhőbe küldött üzenetek a Adafruit lágyított M0 Wi-Fi Arduino tábláról való az IoT hub, az üzenetek kerülnek az Azure Table storage figyelése.

Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja hogyan használható a gulp üzenet olvasása tevékenység olvassa el az Azure Table storage-ban tárolt üzenetek.

## <a name="what-you-need"></a>Mi szükséges
Ez a folyamat megkezdése előtt kell sikeresen befejeződött [futtassa az Azure villogási mintaalkalmazást a Arduino táblán][run-blink-application].

## <a name="read-new-messages-from-your-storage-account"></a>Új üzenetek olvasásakor a tárfiók
Az előző cikkben a mintaalkalmazást a Arduino táblán futtatta. A minta alkalmazás küldött üzenetek az Azure IoT hub. Az IoT hub küldött üzenetek tárolja azokat az Azure Table storage segítségével az Azure-függvény alkalmazás. Az Azure tárolási kapcsolati karakterlánc üzeneteket beolvasni az Azure Table storage van szüksége.

Olvassa el az Azure Table storage-ban tárolt üzenetek, kövesse az alábbi lépéseket:

1. A kapcsolati karakterlánc beolvasása a következő parancsok futtatásával:

   ```bash
   az storage account list -g iot-sample --query [].name
   az storage account show-connection-string -g iot-sample -n {storage name}
   ```

   Az első parancs segítségével lekérdezhető a `storage name` , amelynek használatával a második parancs a kapcsolati karakterláncot. Használjon `iot-sample` értékeként `{resource group name}` Ha az érték nem módosítható.
2. Nyissa meg a konfigurációs fájl `config-arduino.json` a Visual Studio Code a következő parancs futtatásával:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-arduino.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-arduino.json
   ```
3. Cserélje le `[Azure storage connection string]` az 1. lépésben kapott kapcsolati karakterlánccal.
4. Mentse a `config-arduino.json` fájlt.
5. Küldje el újra az üzeneteket, és olvasni őket az Azure Table storage a következő parancs futtatásával:

   ```bash
   gulp run --read-storage

   # You can monitor the serial port by running listen task:
   gulp listen

   # Or you can combine above two gulp tasks into one:
   gulp run --read-storage --listen
   ```

   Az Azure Table storage-ből történő olvasáshoz logika van a `azure-table.js` fájlt.

   ![gulp futásra – olvasás-tároló][gulp-run]

## <a name="summary"></a>Összefoglalás
Hogy sikeresen csatlakoztatva a Arduino board a felhőben az IoT hub és a villogási mintaalkalmazás eszközről a felhőbe üzenetek küldéséhez használt. Az Azure-függvény alkalmazás bejövő IoT hub üzenetek tárolására az Azure Table Storage is használt. Felhő-eszközre küldött üzenetek – a Arduino kártyához – az IoT hub most elküldeni.

## <a name="next-steps"></a>Következő lépések
[Felhő-eszközre küldött üzenetek küldése][send-cloud-to-device-messages]
<!-- Images and links -->

[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[run-blink-application]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-run-azure-blink.md
[gulp-run]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/gulp_read_message_arduino.png
[send-cloud-to-device-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-send-cloud-to-device-messages.md