---
title: 'Connect Arduino (C) tooAzure IoT - lecke 3: Table storage |} Microsoft Docs'
description: "Szerint tooyour Azure Table storage megírásának módjától, figyelje a köszönőüzenetei eszközről a felhőbe."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "adatok hello felhő, felhőalapú adatgyűjtés, iot felhőalapú szolgáltatás, az iot-adatok"
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
ms.openlocfilehash: 9fef18bc9e780e78d95f0c643a5f193125130a5b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="read-messages-persisted-in-azure-storage"></a>Az Azure Storage megőrzött üzenetek olvasása
## <a name="what-you-will-do"></a>Mit fog
A figyelő hello eszközről a felhőbe küldött állapotüzenetek az Adafruit lágyított M0 Wi-Fi Arduino board tooyour IoT hub a hello üzenetekként tooyour Azure Table storage készültek.

Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja, hogyan toouse gulp üzenet olvasása tevékenység tooread köszönőüzenetei megőrzött az Azure Table storage-ban.

## <a name="what-you-need"></a>Mi szükséges
Ez a folyamat megkezdése előtt kell sikeresen befejeződött [hello Azure villogási mintaalkalmazás futtatása a Arduino táblán][run-blink-application].

## <a name="read-new-messages-from-your-storage-account"></a>Új üzenetek olvasásakor a tárfiók
Hello előző cikkben a mintaalkalmazást a Arduino táblán futtatta. hello mintaalkalmazás küldött üzenetek tooyour Azure IoT-központot. köszönőüzenetei tooyour IoT-központ küldött be a Azure Table storage hello Azure függvény app keresztül tárolja. Az az Azure Table storage Azure storage kapcsolati karakterlánc tooread köszönőüzenetei van szüksége.

az Azure Table storage-ban tárolt tooread üzenetek kövesse az alábbi lépéseket:

1. Hello kapcsolati karakterlánc lekéréséhez futtassa a következő parancsok hello:

   ```bash
   az storage account list -g iot-sample --query [].name
   az storage account show-connection-string -g iot-sample -n {storage name}
   ```

   hello első parancs segítségével lekérdezhető hello `storage name` használt hello második parancs tooget hello kapcsolati karakterláncban. Használjon `iot-sample` hello értékeként `{resource group name}` Ha hello érték nem módosítható.
2. Nyissa meg hello konfigurációs fájl `config-arduino.json` a Visual Studio Code hello a következő parancs futtatásával:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-arduino.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-arduino.json
   ```
3. Cserélje le `[Azure storage connection string]` az 1. lépésben kapott hello kapcsolati karakterlánccal.
4. Mentse a hello `config-arduino.json` fájlt.
5. Küldje el újra az üzeneteket, és olvasni őket az Azure Table storage hello a következő parancs futtatásával:

   ```bash
   gulp run --read-storage

   # You can monitor hello serial port by running listen task:
   gulp listen

   # Or you can combine above two gulp tasks into one:
   gulp run --read-storage --listen
   ```

   hello programot az Azure Table storage olvasásra van hello `azure-table.js` fájlt.

   ![gulp futásra – olvasás-tároló][gulp-run]

## <a name="summary"></a>Összefoglalás
Hogy sikeresen csatlakozott az Arduino Bizottság tooyour IoT hub hello felhőben és villogási minta alkalmazás toosend eszközről a felhőbe köszönőüzenetei használt. Hello Azure függvény app toostore bejövő IoT hub üzenetek tooyour Azure Table storage is használt. Most már az IoT hub tooyour Arduino board küldhet felhő-eszközre küldött üzenetek.

## <a name="next-steps"></a>Következő lépések
[Felhő-eszközre küldött üzenetek küldése][send-cloud-to-device-messages]
<!-- Images and links -->

[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[run-blink-application]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-run-azure-blink.md
[gulp-run]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/gulp_read_message_arduino.png
[send-cloud-to-device-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-send-cloud-to-device-messages.md