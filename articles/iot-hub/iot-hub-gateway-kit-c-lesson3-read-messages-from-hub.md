---
title: "SensorTag eszköz & Azure IoT átjáró - rész 3: üzeneteit |} Microsoft Docs"
description: "Futtassa a mintakódot az üzenetek olvasásakor az IoT hub a gazdaszámítógépen."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "a felhő, felhőalapú adatgyűjtés, iot felhőalapú szolgáltatás, az iot-adatok adatok"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: cc88be24-b5c0-4ef2-ba21-4e8f77f3e167
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 45f3595c4848d5c283cdf95604adf8d2c8d6a809
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="read-messages-from-your-iot-hub"></a>Az IoT hub olvasható üzenetek

## <a name="what-you-will-do"></a>Mit fog

- Futtassa a mintakódot az IoT hub üzenetek olvasásakor a gazdaszámítógépen.

Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog

Hogyan használhatja a gulp eszközt az IoT hub üzeneteket beolvasni.

## <a name="what-you-need"></a>Mi szükséges

- A táblázat mintaalkalmazás, amely a 3 sikeresen lefutott.

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Az IoT hub és az eszköz kapcsolati karakterláncok beolvasása

Az eszköz kapcsolati karakterlánc az eszköz (TI SensorTag vagy szimulált eszköz) az IoT hub való csatlakozáshoz használják. Az IoT hub kapcsolati karakterlánc biztosít az identitásjegyzékhez az IoT hub kezelheti az eszközöket, amelyek használata engedélyezett az IoT hub csatlakozni a csatlakozáshoz használt.

- Az erőforráscsoport az IoT hub listán a következő parancs futtatásával:

   ```bash
   az iot hub list -g iot-gateway --query [].name
   ```

   Használjon `iot-gateway` értékeként `{resource group name}` Ha az érték nem módosítható.
- Az IoT hub kapcsolati karakterlánc lekéréséhez futtassa a következő parancsot:

   ```bash
   az iot hub show-connection-string --name {my hub name} -g iot-gateway
   ```

   `{my hub name}`a rendszer az Ön által megadott nevét a 2.

## <a name="configure-the-device-connection-for-the-sample-code"></a>A mintakód az eszköz kapcsolat konfigurálása

Az eszköz konfigurációs fájljának frissítése `config-azure.json` , hogy elolvashatja az IoT hub a számítógépen. Ehhez kövesse az alábbi lépéseket:

1. Nyissa meg `config-azure.json` a Visual Studio Code a konzolablakban a következő parancs futtatásával:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

2. Ellenőrizze a következő cserékhez a `config-azure.json` fájlt:

   ![az azure config képernyőképe](media/iot-hub-gateway-kit-lessons/lesson3/config_azure.png)

   Cserélje le `[IoT hub connection string]` az IoT hub kapcsolati karakterlánccal beszerzett.

## <a name="read-messages-from-your-iot-hub"></a>Az IoT hub olvasható üzenetek

Ha egy TI SensorTag, ellenőrizze, hogy már kapcsolva a SensorTag. Futtassa az átjáró mintaalkalmazást, és olvashatja az IoT-központ által a következő parancsot:

```bash
gulp run --iot-hub
```

A parancs a BLA mintaalkalmazás, amely beolvassa és csomagok a SensorTag vagy a szimulált eszköz hőmérséklete adatait, és az üzenet elküldése az IoT hub 2 másodpercenként futtatja. Azt is indít egyik gyermekfolyamata az üzenetet.

Által küldött és fogadott összes üzenetek azonnal jelenik meg az azonos konzolablak a gazdaszámítógépen. A minta alkalmazáspéldány 40 másodperc múlva automatikusan leáll.

![BLA mintaalkalmazást a küldött és fogadott üzenetek](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_read_hub.png)

## <a name="summary"></a>Összefoglalás

A mintakód üzenetek olvasásakor az IoT hub futtatását. Készen áll az üzenetek az Azure table storage-ban tárolt olvasásához.

## <a name="next-steps"></a>Következő lépések
[Azure-függvényalkalmazás és Azure Storage-fiók létrehozása](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md)


