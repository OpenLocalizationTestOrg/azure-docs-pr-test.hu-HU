---
title: "Csatlakozás málna Pi (csomópont) tooAzure IoT - lecke 3: a minta futtatásához |} Microsoft Docs"
description: "Regisztrálhat és futtathat egy minta alkalmazás tooRaspberry Pi 3, amely tooyour IoT-központ üzeneteket küld, és villogjon-hello LED-jét."
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
ms.openlocfilehash: 991c64e0b1b6f965b029560cdc6403e557841e30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-a-sample-application-toosend-device-to-cloud-messages"></a>Futtassa egy minta alkalmazás toosend eszközről a felhőbe üzenetek
## <a name="what-you-will-do"></a>Mit fog
Ez a cikk bemutatja, hogyan toodeploy, és futtassa a mintaalkalmazást málna Pi 3 által küldött üzenetek tooyour IoT-központot. Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Megtudhatja, hogyan toouse hello eszköz toodeploy gulp és Pi hello minta Node.js-alkalmazást futtatnak.

## <a name="what-you-need"></a>Mi szükséges
* Ez a feladat indítása előtt sikeresen végrehajtotta [hozzon létre egy Azure függvény alkalmazás és a tárolási fiók tooprocess és a tároló IoT-központ üzenetek](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Az IoT hub és az eszköz kapcsolati karakterláncok beolvasása
a Pi tooconnect tooyour IoT-központ hello eszköz kapcsolati karakterlánc használja. az IoT hub kapcsolati karakterlánc hello használt tooconnect toohello identitásjegyzékhez az IoT hub toomanage hello eszközök, amelyek számára engedélyezett tooconnect tooyour IoT-központ. 

* Az erőforráscsoport az IoT hub listában hello Azure CLI parancs a következő futtatásával:

```bash
az iot hub list -g iot-sample --query [].name
```

Használjon `iot-sample` hello értékeként `{resource group name}` Ha hello érték nem módosítható.

* Hello IoT hub kapcsolati karakterlánc lekéréséhez futtassa a következő parancs az Azure parancssori felület hello:

```bash
az iot hub show-connection-string --name {my hub name} -g iot-sample
```

`{my hub name}`Ez az IoT hub létrehozása, és ha a Pi regisztrált megadott hello név.

* Hello eszköz kapcsolati karakterlánc beolvasása hello a következő parancs futtatásával:

```bash
az iot device show-connection-string --hub-name {my hub name} --device-id myraspberrypi -g iot-sample
```

Használjon `myraspberrypi` hello értékeként `{device id}` Ha hello érték nem módosítható.

## <a name="configure-hello-device-connection"></a>Hello eszköz kapcsolat konfigurálása
1. Hello konfigurációs fájl inicializálása hello a következő parancsok futtatásával:
   
   ```bash
   npm install
   gulp init
   ```
2. Nyissa meg hello eszköz konfigurációs fájl `config-raspberrypi.json` a Visual Studio Code hello a következő parancs futtatásával:
   
   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
  
   # For macOS or Ubuntu
   code ~/.iot-hub-getting-started/config-raspberrypi.json
   ```
  
   ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)
3. Ellenőrizze a következő cserékhez a hello hello `config-raspberrypi.json` fájlt:
   
   * Cserélje le **[eszköz állomásnév vagy IP-címe]** hello IP cím vagy állomásnév eszköznévvel során kapott azonosítóértékeket `device-discovery-cli` vagy az eszköz konfigurálása során örökölt hello értékkel.
   * Cserélje le **[IoT eszköz kapcsolati karakterlánc]** a hello `device connection string` beszerzett.
   * Cserélje le **[IoT hub kapcsolati karakterlánc]** a hello `iot hub connection string` beszerzett.

Frissítés hello `config-raspberrypi.json` fájlt úgy, hogy a számítógépről hello mintaalkalmazás telepítése.

## <a name="deploy-and-run-hello-sample-application"></a>Regisztrálhat és futtathat hello mintaalkalmazás
Központi telepítése, és futtassa a mintaalkalmazást hello Pi hello a következő parancs futtatásával:

```bash
gulp deploy && gulp run
```

## <a name="verify-that-hello-sample-application-works"></a>Győződjön meg arról, hogy működik-e hello mintaalkalmazás
Hello LED-jét, amely két másodpercenként villogó csatlakoztatott tooPi kell megjelennie. Minden alkalommal, amikor villogjon-hello LED-jét, hello mintaalkalmazás üzenet tooyour IoT-központ küld, és ellenőrzi, hogy hello üzenet elküldése megtörtént tooyour IoT-központ. Emellett minden egyes hello IoT-központ által fogadott üzenet nyomtatása hello console ablakban. hello mintaalkalmazás automatikusan 20 üzenetek elküldése után leáll.

![A mintaalkalmazás küldött és fogadott üzenetek](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run.png)

## <a name="summary"></a>Összefoglalás
Már telepítve és Pi toosend eszköz a felhőbe küldött üzeneteket tooyour IoT hub hello új villogási mintaalkalmazás futtatása. Az üzenetek, az oktatóprogram toohello tárfiók figyelheti.

## <a name="next-steps"></a>Következő lépések
[Az Azure Storage megőrzött üzenetek olvasása](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)

