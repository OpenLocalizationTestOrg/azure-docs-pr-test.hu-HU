---
title: "Csatlakozás Azure IoT - lecke 3 málna Pi (csomópont): a minta futtatásához |} Microsoft Docs"
description: "Központi telepítése, és futtassa a mintaalkalmazást, amely üzeneteket küld az IoT hub és a LED villogjon málna Pi 3."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "villogási kereslet az olyan felhő pi, villogási vezetett a felhőből"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 427d8e5e-8af8-4249-8607-44edc90a4972
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 1c03283ee276a954f822d6eca5f0a3d5f93ec64b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a>Futtassa a mintaalkalmazást eszközről a felhőbe üzenetek küldése
## <a name="what-you-will-do"></a>Mit fog
Ez a cikk bemutatja, hogyan helyezhet üzembe és futtassa a mintaalkalmazást, amely üzeneteket küld az IoT hub málna Pi 3. Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Megtudhatja, hogyan telepítheti, és futtassa a minta Node.js alkalmazást a Pi gulp eszközzel.

## <a name="what-you-need"></a>Mi szükséges
* Ez a feladat indítása előtt sikeresen végrehajtotta [hozzon létre egy Azure függvény alkalmazást és egy tárfiókot, feldolgozást és tárolást IoT hub üzenetek](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Az IoT hub és az eszköz kapcsolati karakterláncok beolvasása
Az eszköz kapcsolati karakterláncát a Pi használják az IoT hub való kapcsolódáshoz. Az IoT hub kapcsolati karakterlánc biztosít az identitásjegyzékhez az IoT hub kezelheti az eszközöket, amelyek használata engedélyezett az IoT hub csatlakozni a csatlakozáshoz használt. 

* Az erőforráscsoport az IoT hub listán a következő Azure CLI parancs futtatásával:

```bash
az iot hub list -g iot-sample --query [].name
```

Használjon `iot-sample` értékeként `{resource group name}` Ha az érték nem módosítható.

* Az IoT hub kapcsolati karakterlánc lekéréséhez futtassa a következő Azure CLI-parancsot:

```bash
az iot hub show-connection-string --name {my hub name} -g iot-sample
```

`{my hub name}`az Ön által megadott nevét az IoT hub létrehozása, és ha a Pi regisztrálva van.

* Az eszköz kapcsolati karakterlánc lekéréséhez futtassa a következő parancsot:

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myraspberrypi -g iot-sample
```

Használjon `myraspberrypi` értékeként `{device id}` Ha az érték nem módosítható.

## <a name="configure-the-device-connection"></a>Az eszköz kapcsolat konfigurálása
1. A konfigurációs fájl inicializálása a következő parancsok futtatásával:
   
   ```bash
   npm install
   gulp init
   ```
2. Nyissa meg az eszköz konfigurációs fájlját `config-raspberrypi.json` a Visual Studio Code a következő parancs futtatásával:
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
  
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
  
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)
3. Ellenőrizze a következő cserékhez a `config-raspberrypi.json` fájlt:
   
   * Cserélje le **[eszköz állomásnév vagy IP-címe]** nevű eszköz IP cím vagy állomásnév során kapott azonosítóértékeket `device-discovery-cli` vagy az eszköz konfigurálása során örökölt értékkel.
   * Cserélje le **[IoT eszköz kapcsolati karakterlánc]** rendelkező a `device connection string` beszerzett.
   * Cserélje le **[IoT hub kapcsolati karakterlánc]** rendelkező a `iot hub connection string` beszerzett.

Frissítés a `config-raspberrypi.json` fájlt úgy, hogy a számítógépről a mintaalkalmazás telepítése.

## <a name="deploy-and-run-the-sample-application"></a>Regisztrálhat és futtathat a mintaalkalmazás
Telepíthet, és futtassa a mintaalkalmazást a Pi a következő parancs futtatásával:

```bash
gulp deploy && gulp run
```

## <a name="verify-that-the-sample-application-works"></a>Győződjön meg arról, hogy működik-e a mintaalkalmazáshoz
A Pi két másodpercenként villogó kapcsolódó LED kell megjelennie. Minden alkalommal, amikor a LED villogjon, a mintaalkalmazást az IoT hub üzenetet küld, és ellenőrzi, hogy az üzenet már sikeresen elküldte az IoT hub. Emellett minden üzenetet az IoT-központ fogadja a konzolablakban nyomtatása. A mintaalkalmazás automatikusan 20 üzenetek elküldése után leáll.

![A mintaalkalmazás küldött és fogadott üzenetek](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run.png)

## <a name="summary"></a>Összefoglalás
Már telepített, és futtassa a villogási új mintaalkalmazást az eszközről a felhőbe üzeneteket küldhet az IoT hub Pi. Az üzenetek, a tárfiók írás figyelheti.

## <a name="next-steps"></a>Következő lépések
[Az Azure Storage megőrzött üzenetek olvasása](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)

