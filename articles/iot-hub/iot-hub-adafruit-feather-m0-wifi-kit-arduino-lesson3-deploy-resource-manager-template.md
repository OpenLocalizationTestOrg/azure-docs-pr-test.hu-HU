---
title: "Az Azure IoT - lecke 3 Connect Arduino (C): sablon-üzembehelyezés |} Microsoft Docs"
description: "Az Azure-függvény alkalmazás Azure IoT hub eseményeket figyeli, dolgozza fel a bejövő üzenetek, és írja őket az Azure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "adatok tárolása a felhőben, felhőben tárolt adatok iot felhőalapú szolgáltatás"
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
ms.openlocfilehash: be6105927645ae2ec56f6885c61dbcb6faf5b11f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a>Az Azure-függvény app és az Azure storage-fiók létrehozása
[Az Azure Functions](../../articles/azure-functions/functions-overview.md) egyszerűen futtathatók megoldás *funkciók* (kis kódrészletek) a felhőben. Egy Azure függvényalkalmazás biztosítja a a függvények végrehajtásához szükséges az Azure-ban.

## <a name="what-will-you-do"></a>Rendszer mit
Az Azure Resource Manager-sablon használatával hozzon létre egy Azure függvény alkalmazást és egy Azure storage-fiók. Az Azure-függvény alkalmazás Azure IoT hub eseményeket figyeli, dolgozza fel a bejövő üzenetek, és írja őket az Azure Table storage.

Ha bármilyen problémába ütközik, tekintse meg a megoldások a [lap a Adafruit lágyított M0 Wi-Fi Arduino kártya hibaelhárítási](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).

## <a name="what-will-you-learn"></a>Mi ismerkedhet meg
Ebből a cikkből megtudhatja:
* Használata [Azure Resource Manager](../../articles/azure-resource-manager/resource-group-overview.md) központi telepítése az Azure-erőforrások.
* Hogyan használható az Azure-függvény alkalmazás IoT hub üzenetek feldolgozásához, és az Azure Table storage egy táblához.

## <a name="what-do-you-need"></a>Mire van szüksége
Sikeresen végrehajtotta:
- [Ismerkedjen meg a Arduino tábla][get-started]
- [Az Azure IoT hub létrehozása][create-iot-hub]

## <a name="open-the-sample-app"></a>Nyissa meg a mintaalkalmazás
Nyissa meg a minta-projektet a Visual Studio Code a következő parancsok futtatásával:

```bash
cd Lesson3
code .
```

![Tárház szerkezete][repo-structure]

* A `app.ino` fájlt a `app` almappában a kulcs forrásfájl. A forrásfájl üzenet küldése 20 alkalommal az IoT hub, és minden üzenetet küld a LED villogni kódját tartalmazza.
* A `config.json` szükséges konfigurációs beállításait tartalmazza.
* A `arm-template.json` fájl az Azure Resource Manager sablon, amely tartalmazza az Azure-függvény alkalmazás és egy Azure storage-fiók.
* A `arm-template-param.json` a konfigurációs fájl az Azure Resource Manager-sablon által használt.
* A `ReceiveDeviceMessages` tartalmazza, a Node.js-kódot az Azure-függvény.

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>Konfigurálja az Azure Resource Manager-sablonok és erőforrások létrehozása az Azure-ban
Frissítés a `arm-template-param.json` fájlt a Visual Studio Code.

![Az Azure Resource Manager sablon paraméterek][arm-template-params]

* Cserélje le **[az IoT Hub name]** rendelkező **{saját központnevet}** , hogy mikor megadott meg [az IoT hub létrehozása és a Arduino board regisztrált] [ created-iot-hub-and-registered-arduino-board].
* Cserélje le **[előtag-karakterlánc az új erőforrások]** bármely kívánt előtaggal. Az előtag biztosítja, hogy az erőforrás nevének globálisan egyedi ütközés elkerülése érdekében. Ne használjon kötőjel vagy kezdeti az előtag a számot.

Miután frissítette a `arm-template-param.json` fájlt, telepítheti az erőforrásokat az Azure, a következő parancs futtatásával:

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

Ezek az erőforrások létrehozásához körülbelül öt percet vesz igénybe. Amíg az erőforrás létrehozása folyamatban van, továbbléphet a következő cikk tartalmazza.

## <a name="summary"></a>Összefoglalás
Az Azure-függvény alkalmazás feldolgozni az IoT hub üzenetek és az Azure-tárfiók ezek az üzenetek tárolásához létrehozott. Most már telepítheti és futtathatja a Arduino táblán eszköz a felhőbe küldött üzeneteket küldeni.

## <a name="next-steps"></a>Következő lépések
[Küldjön az eszköz a felhőbe küldött üzeneteket a Arduino indítópanel mintaalkalmazás futtatása][send-device-to-cloud-messages]

<!-- Images and links -->

[get-started]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[create-iot-hub]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[repo-structure]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/repo_structure_c.png
[arm-template-params]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson3/arm_para_arduino.png
[created-iot-hub-and-registered-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[send-device-to-cloud-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-run-azure-blink.md