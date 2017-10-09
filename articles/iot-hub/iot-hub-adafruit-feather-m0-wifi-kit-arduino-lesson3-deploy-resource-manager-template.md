---
title: "Connect Arduino (C) tooAzure IoT - lecke 3: sablon-üzembehelyezés |} Microsoft Docs"
description: "hello Azure függvény app tooAzure IoT hub eseményeket figyeli, dolgozza fel a bejövő üzenetek és tooAzure a Table storage írja azokat."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "adattárolás hello felhőben, felhőben tárolt adatok iot felhőalapú szolgáltatás"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 9c8f4cd1-9511-4601-ad7e-51761a986753
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 6a84a6d3c5263a85c8997cf69fe446d73ab7a5fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a>Az Azure-függvény app és az Azure storage-fiók létrehozása
[Az Azure Functions](../../articles/azure-functions/functions-overview.md) egyszerűen futtathatók megoldás *funkciók* (kis kódrészletek) hello felhőben. Egy Azure függvényalkalmazás hello a függvények végrehajtásához szükséges az Azure-ban.

## <a name="what-will-you-do"></a>Rendszer mit
Használja az Azure Resource Manager sablon toocreate Azure függvény alkalmazást és egy Azure storage-fiókot. hello Azure függvény app tooAzure IoT hub eseményeket figyeli, dolgozza fel a bejövő üzenetek és tooAzure a Table storage írja azokat.

Ha bármilyen problémába ütközik, keressen megoldásokat a hello [lap a Adafruit lágyított M0 Wi-Fi Arduino kártya hibaelhárítási](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).

## <a name="what-will-you-learn"></a>Mi ismerkedhet meg
Ebből a cikkből megtudhatja:
* Hogyan toouse [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) toodeploy Azure erőforrásokat.
* Hogyan toouse az Azure app tooprocess IoT hub üzenetek működéséhez, és beírhatók tooa tábla Azure Table storage-ban.

## <a name="what-do-you-need"></a>Mire van szüksége
Sikeresen végrehajtotta:
- [Ismerkedjen meg a Arduino tábla][get-started]
- [Az Azure IoT hub létrehozása][create-iot-hub]

## <a name="open-hello-sample-app"></a>Nyissa meg hello mintaalkalmazás
Nyissa meg a Visual Studio Code hello mintaprojektet hello a következő parancsok futtatásával:

```bash
cd Lesson3
code .
```

![Tárház szerkezete][repo-structure]

* Hello `app.ino` hello fájlban `app` almappa is hello kulcs forrásfájl. A forrásfájl hello kód toosend küldené üzenet 20 alkalommal tooyour IoT hub és villogási hello LED minden üzenet tartalmazza.
* Hello `config.json` szükséges konfigurációs beállításait tartalmazza.
* Hello `arm-template.json` fájl hello Azure Resource Manager-sablon, amely tartalmazza az Azure-függvény alkalmazás és egy Azure storage-fiók.
* Hello `arm-template-param.json` hello konfigurációs fájl hello Azure Resource Manager-sablon által használt.
* Hello `ReceiveDeviceMessages` hello Azure függvény hello Node.js kódját tartalmazza.

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>Konfigurálja az Azure Resource Manager-sablonok és erőforrások létrehozása az Azure-ban
Frissítés hello `arm-template-param.json` fájlt a Visual Studio Code.

![Az Azure Resource Manager sablon paraméterek][arm-template-params]

* Cserélje le **[az IoT Hub name]** rendelkező **{saját központnevet}** , hogy mikor megadott meg [az IoT hub létrehozása és a Arduino board regisztrált] [ created-iot-hub-and-registered-arduino-board].
* Cserélje le **[előtag-karakterlánc az új erőforrások]** bármely kívánt előtaggal. hello előtag biztosítja az erőforrásnév hello globálisan egyedi tooavoid ütközés. Ne használjon kötőjel vagy kezdeti hello előtag a számot.

Miután frissítette hello `arm-template-param.json` fájlt, hello erőforrások tooAzure telepítése hello a következő parancs futtatásával:

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

Tart körülbelül öt perc toocreate ezeket az erőforrásokat. Hello erőforrás létrehozása folyamatban van, míg a következő cikk toohello áthelyezheti.

## <a name="summary"></a>Összefoglalás
Létrehozta az Azure-függvény app tooprocess IoT hub-üzeneteket és az Azure-tárolóra toostore ezeket az üzeneteket. Most már telepítheti és üdvözlő minta toosend eszközről a felhőbe üzenetek futtassa a Arduino táblán.

## <a name="next-steps"></a>Következő lépések
[A Arduino táblán az eszköz a felhőbe küldött üzeneteket egy minta alkalmazás toosend futtatása][send-device-to-cloud-messages]

<!-- Images and links -->

[get-started]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[create-iot-hub]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/repo_structure_c.png
[arm-template-params]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/arm_para_arduino.png
[created-iot-hub-and-registered-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[send-device-to-cloud-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-run-azure-blink.md