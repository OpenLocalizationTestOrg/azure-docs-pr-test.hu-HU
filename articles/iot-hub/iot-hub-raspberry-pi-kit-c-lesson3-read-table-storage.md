---
title: 'Connect Raspberry pi (C) tooAzure IoT - lecke 3: Table storage |} Microsoft Docs'
description: "Szerint tooyour Azure Table storage megírásának módjától, figyelje a köszönőüzenetei eszközről a felhőbe."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "adatok beolvasása a felhőből, iot-felhőszolgáltatás"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 8c5558bb-3c31-4445-90e6-b1a978738545
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 307ce2bc595339790db7379cc011fe262c2b8734
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="read-messages-persisted-in-azure-storage"></a>Az Azure Storage megőrzött üzenetek olvasása
## <a name="what-you-will-do"></a>Mit fog
A figyelő hello eszközről a felhőbe küldött állapotüzenetek málna Pi 3 tooyour IoT hubról hello üzenetekként tooyour Azure Table storage készültek. Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja, hogyan toouse gulp üzenet olvasása tevékenység tooread köszönőüzenetei megőrzött az Azure Table storage-ban.

## <a name="what-you-need"></a>Mi szükséges
Ez a folyamat megkezdése előtt kell sikeresen befejeződött [hello Azure villogási mintaalkalmazás futtatnak málna Pi 3](iot-hub-raspberry-pi-kit-c-lesson3-run-azure-blink.md).

## <a name="read-new-messages-from-your-storage-account"></a>Új üzenetek olvasásakor a tárfiók
Hello előző cikkben a mintaalkalmazást a Pi futtatta. hello mintaalkalmazás küldött üzenetek tooyour Azure IoT-központot. köszönőüzenetei tooyour IoT-központ küldött be a Azure Table storage hello Azure függvény app keresztül tárolja. Az az Azure Table storage Azure storage kapcsolati karakterlánc tooread köszönőüzenetei van szüksége.

az Azure Table storage-ban tárolt tooread üzenetek kövesse az alábbi lépéseket:

1. Hello kapcsolati karakterlánc lekéréséhez futtassa a következő parancsok hello:

   ```bash
   az storage account list -g iot-sample --query [].name
   az storage account show-connection-string -g iot-sample -n {storage name}
   ```

   hello első parancs segítségével lekérdezhető hello `storage name` használt hello második parancs tooget hello kapcsolati karakterláncban. Használjon `iot-sample` hello értékeként `{resource group name}` Ha hello érték nem módosítható.
2. Nyissa meg hello konfigurációs fájl `config-raspberrypi.json` a Visual Studio Code hello a következő parancs futtatásával:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
3. Cserélje le `[Azure storage connection string]` az 1. lépésben kapott hello kapcsolati karakterlánccal.
4. Mentse a hello `config-raspberrypi.json` fájlt.
5. Küldje el újra az üzeneteket, és olvasni őket az Azure Table storage hello a következő parancs futtatásával:
   
   ```bash
   gulp run --read-storage
   ```
   
   hello programot az Azure Table storage olvasásra van hello `azure-table.js` fájlt.
   
   ![gulp futásra – olvasás-tároló](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_read_message_c.png)

## <a name="summary"></a>Összefoglalás
Hogy sikeresen csatlakoztatva Pi tooyour IoT-központ hello felhőben és villogási minta alkalmazás toosend eszközről a felhőbe köszönőüzenetei használt. Hello Azure függvény app toostore bejövő IoT hub üzenetek tooyour Azure Table storage is használt. Most már az IoT hub tooPi küldhet felhő-eszközre küldött üzenetek.

## <a name="next-steps"></a>Következő lépések
[Futtassa egy minta alkalmazás tooreceive felhő-eszközre küldött üzenetek](iot-hub-raspberry-pi-kit-c-lesson4-send-cloud-to-device-messages.md)

