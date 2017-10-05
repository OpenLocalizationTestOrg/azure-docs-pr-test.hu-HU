---
title: "Az Azure IoT - lecke 3 Connect Intel Edison (C): üzenetek küldése |} Microsoft Docs"
description: "Regisztrálhat és futtathat a mintaalkalmazás, amely üzeneteket küld az IoT hub és a LED villogjon Intel Edison számára."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "IOT felhőalapú szolgáltatás, arduino adatok felhőbe küldése"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-c-get-started
ms.assetid: 12672b64-795a-4dfc-86fd-df53ed3eeef7
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: d104618ebb37a19c83f161beceb5c71bc89bbb56
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a>Futtassa a mintaalkalmazást eszközről a felhőbe üzenetek küldése
## <a name="what-you-will-do"></a>Mit fog
Ez a cikk bemutatja, hogyan helyezhet üzembe és futtassa a mintaalkalmazást, amely üzeneteket küld az IoT hub Intel Edison. Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Megtudhatja, hogyan telepítheti, és futtassa a mintaalkalmazást C Edison gulp eszközzel.

## <a name="what-you-need"></a>Mi szükséges
* Ez a feladat indítása előtt sikeresen végrehajtotta [hozzon létre egy Azure függvény alkalmazást és egy tárfiókot, feldolgozást és tárolást IoT hub üzenetek][process-and-store-iot-hub-messages].

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Az IoT hub és az eszköz kapcsolati karakterláncok beolvasása
Az eszköz kapcsolati karakterláncát Edison csatlakozni az IoT hub szolgál. Az IoT hub kapcsolati karakterlánc az IoT hub csatlakozni az eszközidentitást az IoT hub Edison jelölő szolgál.

* Az erőforráscsoport az IoT hub listán a következő Azure CLI parancs futtatásával:

```bash
az iot hub list -g iot-sample --query [].name
```

Használjon `iot-sample` értékeként `{resource group name}` Ha az érték nem módosítható.

* Az IoT hub kapcsolati karakterlánc lekéréséhez futtassa a következő Azure CLI-parancsot:

```bash
az iot hub show-connection-string --name {my hub name}
```

`{my hub name}`az Ön által megadott nevét az IoT hub létrehozása, és ha a Edison regisztrálva van.

* Az eszköz kapcsolati karakterlánc lekéréséhez futtassa a következő parancsot:

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myinteledison
```

Használjon `myinteledison` értékeként `{device id}` Ha az érték nem módosítható.

## <a name="configure-the-device-connection"></a>Az eszköz kapcsolat konfigurálása
1. A konfigurációs fájl inicializálása a következő parancsok futtatásával:

   ```bash
   npm install
   gulp init
   ```
   > [!NOTE]
   > Futtatás **gulp az install-eszközök** és, ha még nem meg lecke 1.

2. Nyissa meg az eszköz konfigurációs fájlját `config-edison.json` a Visual Studio Code a következő parancs futtatásával:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-edison.json

   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-edison.json
   ```

   ![Config.JSON](media/iot-hub-intel-edison-lessons/lesson3/config.png)

3. Ellenőrizze a következő cserékhez a `config-edison.json` fájlt:

   * Cserélje le **[eszköz állomásnév vagy IP-címe]** az eszköz konfigurálása során az lefelé megjelölt eszköz IP-címmel.
   * Cserélje le **[IoT eszköz kapcsolati karakterlánc]** rendelkező a `device connection string` beszerzett.
   * Cserélje le **[IoT hub kapcsolati karakterlánc]** rendelkező a `iot hub connection string` beszerzett.

   > [!NOTE]
   > Nem kell `azure_storage_connection_string` ebben a cikkben. Tartsa a jelenlegi állapotában.

## <a name="deploy-and-run-the-sample-application"></a>Regisztrálhat és futtathat a mintaalkalmazás
Telepíthet, és futtassa a mintaalkalmazást a Edison a következő parancs futtatásával:

```bash
gulp deploy && gulp run
```

## <a name="verify-that-the-sample-application-works"></a>Győződjön meg arról, hogy működik-e a mintaalkalmazáshoz
Két másodpercenként villogó Edison kapcsolódó LED kell megjelennie. Minden alkalommal, amikor a LED villogjon, a mintaalkalmazást az IoT hub üzenetet küld, és ellenőrzi, hogy az üzenet már sikeresen elküldte az IoT hub. Emellett minden üzenetet az IoT-központ fogadja a konzolablakban nyomtatása. A mintaalkalmazás automatikusan 20 üzenetek elküldése után leáll.

![A mintaalkalmazás küldött és fogadott üzenetek][sample-application-with-sent-and-received-messages]

## <a name="summary"></a>Összefoglalás
Már telepített, és futtassa a villogási új mintaalkalmazást az eszközről a felhőbe üzeneteket küldhet az IoT hub Edison. Az üzenetek most szerint a tárfiók írás figyelje.

## <a name="next-steps"></a>Következő lépések
[Az Azure Storage megőrzött üzenetek olvasása][read-messages-persisted-in-azure-storage]
<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-intel-edison-kit-c-lesson3-deploy-resource-manager-template.md
[sample-application-with-sent-and-received-messages]: media/iot-hub-intel-edison-lessons/lesson3/gulp_run_c.png
[read-messages-persisted-in-azure-storage]: iot-hub-intel-edison-kit-c-lesson3-read-table-storage.md