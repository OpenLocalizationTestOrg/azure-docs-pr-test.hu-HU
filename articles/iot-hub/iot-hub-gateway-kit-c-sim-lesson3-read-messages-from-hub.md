---
title: "A szimulált eszköz & Azure IoT átjáró - lecke 3: üzeneteit |} Microsoft Docs"
description: "Futtassa a mintakód a fogadó számítógép tooread köszönőüzenetei az IoT hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "adatok hello felhő, felhőalapú adatgyűjtés, iot felhőalapú szolgáltatás, az iot-adatok"
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
ms.openlocfilehash: 540575724bb5cdac4db581a226d8a02a59004d8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="read-messages-from-your-iot-hub"></a>Az IoT hub olvasható üzenetek

## <a name="what-you-will-do"></a>Mit fog

- Futtassa mintakód a gazdagépen futó számítógép tooread üzenetet az IoT hub.

Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-sim-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog

Hogyan toouse hello gulp az IoT hub eszköz tooread üzeneteit.

## <a name="what-you-need"></a>Mi szükséges

- hello szimulált eszköz minta [beállítása és futtatása a szimulált eszköz felhő feltöltése mintaalkalmazás](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md).

## <a name="get-your-iot-hub-and-device-connection-strings"></a>Az IoT hub és az eszköz kapcsolati karakterláncok beolvasása

a szimulált eszköz tooconnect tooyour IoT-központ hello eszköz kapcsolati karakterlánc használja. az IoT hub kapcsolati karakterlánc hello használt tooconnect toohello identitásjegyzékhez az IoT hub toomanage hello eszközök, amelyek számára engedélyezett tooconnect tooyour IoT-központ.

- Az erőforráscsoport az IoT hub listában hello a következő parancs futtatásával:

   ```bash
   az iot hub list -g iot-gateway --query [].name
   ```

   Használjon `iot-gateway` hello értékeként `{resource group name}` Ha nem módosítja azt.
- Hello IoT hub kapcsolati karakterlánc beolvasása hello a következő parancs futtatásával:

   ```bash
   az iot hub show-connection-string --name {my hub name} -g iot-gateway
   ```

   `{my hub name}`van hello Ön által megadott nevét a 2.

## <a name="configure-hello-device-connection-for-hello-sample-code"></a>Hello mintakód hello eszköz kapcsolat konfigurálása

Az IoT hub és az eszköz kapcsolat konfigurációja frissítése `config-azure.json` hello lépések végrehajtásával:

1. Nyissa meg `config-azure.json` a Visual Studio Code hello a konzolablakban parancs a következő futtatásával:

   ```bash
   # For Windows command prompt
   code %USERPROFILE%\.iot-hub-getting-started\config-azure.json
   # For MacOS or Ubuntu
   code ~/.iot-hub-getting-started/config-azure.json
   ```

2. Ellenőrizze a következő cserékhez a hello hello `config-azure.json` fájlt:

   ![az azure config képernyőképe](media/iot-hub-gateway-kit-lessons/lesson3/config_azure.png)

   Cserélje le `[IoT hub connection string]` a hello IoT hub kapcsolati karakterláncot.

## <a name="read-messages-from-your-iot-hub"></a>Az IoT hub olvasható üzenetek

Hello szimulált eszköz mintaalkalmazás futtatása pedig beolvassák az IoT-központ üzenetek hello a következő parancsot:

```bash
gulp run --iot-hub
```

hello parancs által küldött üzenetek tooyour IoT-központ 2 másodpercenként hello-alkalmazást futtat. Azt is egy gyermek folyamat tooreceive üdvözlőüzenetére indít.

küldött és fogadott azonos konzolablakában az összes megjelenített azonnal hello köszönőüzenetei hello gazdaszámítógépen. hello alkalmazás 40 másodpercben kilép.

![Küldött és fogadott üzenetek szimulált mintaalkalmazás](media/iot-hub-gateway-kit-lessons/lesson3/gulp_run_read_hub_simudev.png)

## <a name="summary"></a>Összefoglalás

A szimulált eszköz sikeresen hello minta alkalmazás toosend adatok tooyour IoT-központ már futtatta. Az IoT-központ tooyour elküldött köszönőüzenetei is, hogy elolvasta.

## <a name="next-steps"></a>Következő lépések
[Azure-függvényalkalmazás és Azure Storage-fiók létrehozása](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md)


