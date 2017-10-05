---
title: "A szimulált eszköz & Azure IoT átjáró - lecke 3: üzeneteit |} Microsoft Docs"
description: "Futtassa a mintakódot az üzenetek olvasásakor az IoT hub a gazdaszámítógépen."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "a felhő, felhőalapú adatgyűjtés, iot felhőalapú szolgáltatás, az iot-adatok adatok"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 5a6ec9c1-d83c-41c1-beaf-7c0d3395d77f
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 9fbf7958e2437d274f2692dbc235ac8147bdfa63
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="read-messages-from-your-iot-hub"></a>Az IoT hub olvasható üzenetek

## <a name="what-you-will-do"></a>Mit fog

- Futtassa a mintakódot az IoT hub üzenetek olvasásakor a gazdaszámítógépen.

Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog

Hogyan használhatja a gulp eszközt az IoT hub üzeneteket beolvasni.

## <a name="what-you-need"></a>Mi szükséges

- A szimulált eszköz minta [beállítása és futtatása a szimulált eszköz felhő feltöltése mintaalkalmazás](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Az IoT hub és az eszköz kapcsolati karakterláncok beolvasása

Az eszköz kapcsolati karakterláncát a szimulált eszköz használják az IoT hub való kapcsolódáshoz. Az IoT hub kapcsolati karakterlánc biztosít az identitásjegyzékhez az IoT hub kezelheti az eszközöket, amelyek használata engedélyezett az IoT hub csatlakozni a csatlakozáshoz használt.

- Az erőforráscsoport az IoT hub listán a következő parancs futtatásával:

   ```bash
   az iot hub list -g iot-gateway --query [].name
   ```

   Használjon `iot-gateway` értékeként `{resource group name}` Ha nem módosítja azt.
- Az IoT hub kapcsolati karakterlánc lekéréséhez futtassa a következő parancsot:

   ```bash
   az iot hub show-connection-string --name {my hub name} -g iot-gateway
   ```

   `{my hub name}`a rendszer az Ön által megadott nevét a 2.

## <a name="configure-the-device-connection-for-the-sample-code"></a>A mintakód az eszköz kapcsolat konfigurálása

Az IoT hub és az eszköz kapcsolat konfigurációja frissítése `config-azure.json` az alábbi lépések elvégzésével:

1. Nyissa meg `config-azure.json` a Visual Studio Code a konzolablakban a következő parancs futtatásával:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

2. Ellenőrizze a következő cserékhez a `config-azure.json` fájlt:

   ![az azure config képernyőképe](media/iot-hub-gateway-kit-lessons/lesson3/config_azure.png)

   Cserélje le `[IoT hub connection string]` az IoT hub kapcsolati karakterlánccal.

## <a name="read-messages-from-your-iot-hub"></a>Az IoT hub olvasható üzenetek

Futtassa a szimulált eszköz mintaalkalmazást, és olvashatja az IoT-központ által a következő parancsot:

```bash
gulp run --iot-hub
```

A parancs futtatja az alkalmazást, amely üzeneteket küld az IoT hub 2 másodpercenként. Azt is indít egyik gyermekfolyamata az üzenetet.

Által küldött és fogadott összes üzenetek azonnal jelenik meg az azonos konzolablak a gazdaszámítógépen. Az alkalmazás 40 másodpercben kilép.

![Küldött és fogadott üzenetek szimulált mintaalkalmazás](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_read_hub_simudev.png)

## <a name="summary"></a>Összefoglalás

Már sikeresen futott az IoT hub szimulált eszköz adatokat küldeni a mintaalkalmazáshoz. Az IoT hub elküldött üzenetek is olvasott.

## <a name="next-steps"></a>Következő lépések
[Azure-függvényalkalmazás és Azure Storage-fiók létrehozása](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md)


