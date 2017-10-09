---
title: "Csatlakozás Intel Edison (csomópont) tooAzure IoT - lecke 3: üzeneteket figyelő |} Microsoft Docs"
description: "Szerint tooyour Azure Table storage megírásának módjától, figyelje a köszönőüzenetei eszközről a felhőbe."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "adatok hello felhő, felhőalapú adatgyűjtés, iot felhőalapú szolgáltatás, az iot-adatok"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: fa2c7efe-7e34-4e39-bb70-015c15ac69ed
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 8f6371482123bc9aa12db55b38d3e8863645f981
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="read-messages-persisted-in-azure-storage"></a>Az Azure Storage megőrzött üzenetek olvasása
## <a name="what-you-will-do"></a>Mit fog
A figyelő hello eszközről a felhőbe küldött állapotüzenetek az Intel Edison tooyour IoT-központ hello üzenetekként tooyour Azure Table storage készültek. Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja, hogyan toouse gulp üzenet olvasása tevékenység tooread köszönőüzenetei megőrzött az Azure Table storage-ban.

## <a name="what-you-need"></a>Mi szükséges
Ez a folyamat megkezdése előtt kell sikeresen befejeződött [hello Azure villogási mintaalkalmazás futtatása az Intel Edison][run-the-azure-blink-sample-application-on-intel-edison].

## <a name="read-new-messages-from-your-storage-account"></a>Új üzenetek olvasásakor a tárfiók
Hello előző cikkben a mintaalkalmazást a Edison futtatta. hello mintaalkalmazás küldött üzenetek tooyour Azure IoT-központot. köszönőüzenetei tooyour IoT-központ küldött be a Azure Table storage hello Azure függvény app keresztül tárolja. Az az Azure Table storage Azure storage kapcsolati karakterlánc tooread köszönőüzenetei van szüksége.

az Azure Table storage-ban tárolt tooread üzenetek kövesse az alábbi lépéseket:

1. Hello kapcsolati karakterlánc lekéréséhez futtassa a következő parancsok hello:

   ```bash
   az storage account list -g iot-sample --query [].name
   az storage account show-connection-string -g iot-sample -n {storage name}
   ```

   hello első parancs segítségével lekérdezhető hello `storage name` használt hello második parancs tooget hello kapcsolati karakterláncban. Használjon `iot-sample` hello értékeként `{resource group name}` Ha hello érték nem módosítható.
2. Nyissa meg hello konfigurációs fájl `config-edison.json` a Visual Studio Code hello a következő parancs futtatásával:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```
3. Cserélje le `[Azure storage connection string]` az 1. lépésben kapott hello kapcsolati karakterlánccal.
4. Mentse a hello `config-edison.json` fájlt.
5. Küldje el újra az üzeneteket, és olvasni őket az Azure Table storage hello a következő parancs futtatásával:

   ```bash
   gulp run --read-storage
   ```

   hello programot az Azure Table storage olvasásra van hello `azure-table.js` fájlt.

   ![gulp futásra – olvasás-tároló][gulp run]

## <a name="summary"></a>Összefoglalás
Hogy sikeresen csatlakoztatva Edison tooyour IoT-központ hello felhőben és villogási minta alkalmazás toosend eszközről a felhőbe köszönőüzenetei használt. Hello Azure függvény app toostore bejövő IoT hub üzenetek tooyour Azure Table storage is használt. Most már az IoT hub tooEdison küldhet felhő-eszközre küldött üzenetek.

## <a name="next-steps"></a>Következő lépések
[Futtassa egy minta alkalmazás tooreceive felhő-eszközre küldött üzenetek][receive-cloud-to-device-messages]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[run-the-azure-blink-sample-application-on-intel-edison]: iot-hub-intel-edison-kit-node-lesson3-run-azure-blink.md
[gulp run]: media/iot-hub-intel-edison-lessons/lesson3/gulp_read_message.png
[receive-cloud-to-device-messages]: iot-hub-intel-edison-kit-node-lesson4-send-cloud-to-device-messages.md