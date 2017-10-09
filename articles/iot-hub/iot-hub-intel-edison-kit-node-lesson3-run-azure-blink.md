---
title: "Csatlakozás Intel Edison (csomópont) tooAzure IoT - lecke 3: üzenetek küldése |} Microsoft Docs"
description: "Regisztrálhat és futtathat egy minta alkalmazás tooIntel Edison, amely tooyour IoT-központ üzeneteket küld, és villogjon-hello LED-jét."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az IOT felhőalapú szolgáltatás, arduino küldése adatok toocloud"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: 1b3b1074-f4d4-42ac-b32c-55f18b304b44
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: ebd4c7558544d64086fb4cd615cee546aeed2fc1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-toosend-device-to-cloud-messages"></a>Futtassa egy minta alkalmazás toosend eszközről a felhőbe üzenetek
## <a name="what-you-will-do"></a>Mit fog
Ez a cikk bemutatja, hogyan toodeploy, és futtassa a mintaalkalmazást az Intel Edison által küldött üzenetek tooyour IoT-központot. Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshooting].

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Megtudhatja, hogyan toouse hello gulp eszköz toodeploy lesz, és C mintaalkalmazás hello futtatnak Edison.

## <a name="what-you-need"></a>Mi szükséges
* Ez a feladat indítása előtt sikeresen végrehajtotta [hozzon létre egy Azure függvény alkalmazás és a tárolási fiók tooprocess és a tároló IoT-központ üzenetek][process-and-store-iot-hub-messages].

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Az IoT hub és az eszköz kapcsolati karakterláncok beolvasása
hello eszköz kapcsolati karakterlánca használt tooconnect Edison tooyour IoT-központot. az IoT hub kapcsolati karakterlánc hello használt tooconnect az IoT hub toohello eszközidentitás hello IoT-központ Edison jelölő van.

* Az erőforráscsoport az IoT hub listában hello Azure CLI parancs a következő futtatásával:

```bash
az iot hub list -g iot-sample --query [].name
```

Használjon `iot-sample` hello értékeként `{resource group name}` Ha hello érték nem módosítható.

* Hello IoT hub kapcsolati karakterlánc lekéréséhez futtassa a következő parancs az Azure parancssori felület hello:

```bash
az iot hub show-connection-string --name {my hub name}
```

`{my hub name}`Ez az IoT hub létrehozása, és ha a Edison regisztrált megadott hello név.

* Hello eszköz kapcsolati karakterlánc beolvasása hello a következő parancs futtatásával:

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myinteledison
```

Használjon `myinteledison` hello értékeként `{device id}` Ha hello érték nem módosítható.

## <a name="configure-hello-device-connection"></a>Hello eszköz kapcsolat konfigurálása
1. Hello konfigurációs fájl inicializálása hello a következő parancsok futtatásával:

   ```bash
   npm install
   gulp init
   ```

2. Nyissa meg hello eszköz konfigurációs fájl `config-edison.json` a Visual Studio Code hello a következő parancs futtatásával:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson3/config.png)
3. Ellenőrizze a következő cserékhez a hello hello `config-edison.json` fájlt:

   * Cserélje le **[eszköz állomásnév vagy IP-címe]** hello eszköz IP-cím az eszköz konfigurálása során az lefelé jelölésű.
   * Cserélje le **[IoT eszköz kapcsolati karakterlánc]** a hello `device connection string` beszerzett.
   * Cserélje le **[IoT hub kapcsolati karakterlánc]** a hello `iot hub connection string` beszerzett.

   > [!NOTE]
   > Nem kell `azure_storage_connection_string` ebben a cikkben. Tartsa a jelenlegi állapotában.

## <a name="deploy-and-run-hello-sample-application"></a>Regisztrálhat és futtathat hello mintaalkalmazás
Központi telepítése, és futtassa a mintaalkalmazást hello Edison hello a következő parancs futtatásával:

```bash
gulp deploy && gulp run
```

## <a name="verify-that-hello-sample-application-works"></a>Győződjön meg arról, hogy működik-e hello mintaalkalmazás
Hello LED-jét, amely két másodpercenként villogó csatlakoztatott tooEdison kell megjelennie. Minden alkalommal, amikor villogjon-hello LED-jét, hello mintaalkalmazás üzenet tooyour IoT-központ küld, és ellenőrzi, hogy hello üzenet elküldése megtörtént tooyour IoT-központ. Emellett minden egyes hello IoT-központ által fogadott üzenet nyomtatása hello console ablakban. hello mintaalkalmazás automatikusan 20 üzenetek elküldése után leáll.

![A mintaalkalmazás küldött és fogadott üzenetek][sample-application-with-sent-and-received-messages]

## <a name="summary"></a>Összefoglalás
Már telepített, és új villogási mintaalkalmazás hello futtatnak Edison toosend eszköz a felhőbe küldött üzeneteket tooyour IoT-központot. Az üzenetek most szerint toohello tárfiók írás figyelje.

## <a name="next-steps"></a>Következő lépések
[Az Azure Storage megőrzött üzenetek olvasása][read-messages-persisted-in-azure-storage]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-intel-edison-kit-node-lesson3-deploy-resource-manager-template.md
[sample-application-with-sent-and-received-messages]: media/iot-hub-intel-edison-lessons/lesson3/gulp_run.png
[read-messages-persisted-in-azure-storage]: iot-hub-intel-edison-kit-node-lesson3-read-table-storage.md