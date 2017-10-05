---
featureFlags: usabilla
title: "Csatlakozás Azure IoT - lecke 3 málna Pi (csomópont): Table storage |} Microsoft Docs"
description: "Mivel az Azure Table storage írás az eszközről a felhőbe üzenetek figyelése"
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "adatok beolvasása a felhőből, iot-felhőszolgáltatás"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 9965bd54-61da-479b-aaad-5604850a2be5
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 60084906c05ff9e5396f8e2378d73f7ac939d8df
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="read-messages-persisted-in-azure-storage"></a>Az Azure Storage megőrzött üzenetek olvasása
## <a name="what-you-will-do"></a>Mit fog
Az eszközről a felhőbe küldött üzenetek málna Pi 3 az IoT hubhoz, az üzenetek kerülnek az Azure Table storage figyelése. Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja hogyan használható a gulp üzenet olvasása tevékenység olvassa el az Azure Table storage-ban tárolt üzenetek.

## <a name="what-you-need"></a>Mi szükséges
Ez a folyamat megkezdése előtt kell sikeresen befejeződött [futtassa az Azure villogási mintaalkalmazást málna Pi 3](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md).

## <a name="read-new-messages-from-your-storage-account"></a>Új üzenetek olvasásakor a tárfiók
Az előző cikkben a mintaalkalmazást a Pi futtatta. A minta alkalmazás küldött üzenetek az Azure IoT hub. Az IoT hub küldött üzenetek tárolja azokat az Azure Table storage segítségével az Azure-függvény alkalmazás. Az Azure tárolási kapcsolati karakterlánc üzeneteket beolvasni az Azure Table storage van szüksége.

Olvassa el az Azure Table storage-ban tárolt üzenetek, kövesse az alábbi lépéseket:

1. A kapcsolati karakterlánc beolvasása a következő parancsok futtatásával:

   ```bash
   az storage account list -g iot-sample --query [].name
   az storage account show-connection-string -g iot-sample -n {storage name}
   ```

   Az első parancs segítségével lekérdezhető a `storage name` , amelynek használatával a második parancs a kapcsolati karakterláncot. Használjon `iot-sample` értékeként `{resource group name}` Ha az érték nem módosítható.
2. Nyissa meg a konfigurációs fájl `config-raspberrypi.json` a Visual Studio Code a következő parancs futtatásával:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
   
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
3. Cserélje le `[Azure storage connection string]` az 1. lépésben kapott kapcsolati karakterlánccal.
4. Mentse a `config-raspberrypi.json` fájlt.
5. Küldje el újra az üzeneteket, és olvasni őket az Azure Table storage a következő parancs futtatásával:
   
   ```bash
   gulp run --read-storage
   ```
   
   Az Azure Table storage-ből történő olvasáshoz logika van a `azure-table.js` fájlt.
   
    ![gulp futásra – olvasás-tároló](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_read_message.png)

## <a name="summary"></a>Összefoglalás
Hogy sikeresen Pi csatlakozik az IoT hub a felhőben és a villogási mintaalkalmazás eszközről a felhőbe üzenetek küldéséhez használt. Az Azure-függvény alkalmazás bejövő IoT hub üzenetek tárolására az Azure Table Storage is használt. Most küldhet felhő-eszközre küldött üzenetek az IoT hub a Pi.

## <a name="next-steps"></a>Következő lépések
[Futtassa a mintaalkalmazást a felhő-eszközre küldött üzenetek fogadására](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)

