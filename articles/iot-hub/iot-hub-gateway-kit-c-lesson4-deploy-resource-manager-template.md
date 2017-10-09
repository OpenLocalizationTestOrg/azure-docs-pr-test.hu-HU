---
title: "SensorTag eszköz & Azure IoT átjáró - rész 4: függvény-alkalmazás létrehozása |} Microsoft Docs"
description: "Intel NUC tooyour IoT hubról üzenetek menthetők, írja őket tooAzure a Table storage, és majd hello felhőből olvashatja őket."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "adattárolás hello felhőben, felhőben tárolt adatok iot felhőalapú szolgáltatás"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: f84f9a85-e2c4-4a92-8969-f65eb34c194e
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: efee3bdc15ced104651f4a500311a5fe614267c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-function-app-and-storage-account"></a>Azure-függvényalkalmazás és -tárfiók létrehozása

Az Azure Functions megoldással egyszerűen futtathatók _funkciók_ (kis kódrészletek) hello felhőben. Egy Azure függvényalkalmazás hello a függvények végrehajtásához szükséges az Azure-ban. 

## <a name="what-you-will-do"></a>Mit fog

- Használja az Azure Resource Manager sablon toocreate Azure függvény alkalmazást és egy Azure storage-fiókot. hello Azure függvény app tooAzure IoT hub eseményeket figyeli, dolgozza fel a bejövő üzenetek és tooAzure a Table storage írja azokat.

Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).


## <a name="what-you-will-learn"></a>Amiről tanulni fog

Ez a lecke témák köre:

- Hogyan toouse Azure Resource Manager toodeploy Azure-erőforrások.
- Hogyan toouse az Azure app tooprocess IoT-központ üzenetek működéséhez, és beírhatók tooa tábla Azure Table storage-ban.

## <a name="what-you-need"></a>Mi szükséges

Sikeresen végrehajtotta hello előző megszerzett:

- [1. lecke: Állítsa be az Intel NUC IoT átjáróként](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
- [2. lecke: Felkészülés a gazdaszámítógép és Azure IoT-központ](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)
- [3. lecke: SensorTag érkező üzenetek fogadására és üzenetek olvasni az IoT-központ](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)

## <a name="open-a-sample-app"></a>Nyissa meg a mintaalkalmazás

Nyissa meg tooyour `iot-hub-c-intel-nuc-gateway-getting-started` tárház mappa, inicializálási hello konfigurációs fájlokat és majd nyitott hello minta projektre a Visual Studio Code hello a következő parancs futtatásával:

```bash
cd Lesson4
npm install
gulp init
code .
```

![Tárház szerkezete](media/iot-hub-gateway-kit-lessons/lesson4/arm_template.png)

- Hello `arm-template.json` fájl hello Azure Resource Manager-sablon, amely tartalmazza az Azure-függvény alkalmazás és egy Azure storage-fiók.
- Hello `arm-template-param.json` hello konfigurációs fájl hello Azure Resource Manager-sablon által használt.
- Hello `ReceiveDeviceMessages` hello Azure függvény hello Node.js kódját tartalmazza.

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>Konfigurálja az Azure Resource Manager-sablonok és erőforrások létrehozása az Azure-ban

Frissítés hello `arm-template-param.json` fájlt a Visual Studio Code.

![ARM-sablon json](media/iot-hub-gateway-kit-lessons/lesson4/arm_template_param.png)

- Cserélje le `[your IoT Hub name]` rendelkező `{my hub name}` , amelyet a 2.

Miután frissítette hello `arm-template-param.json` fájlt, hello erőforrások tooAzure telepítése hello a következő parancs futtatásával:

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-gateway
```

Használjon `iot-gateway` hello értékeként `{resource group name}` nem módosítása hello érték a 2.

## <a name="summary"></a>Összefoglalás

Létrehozta az Azure-függvény app tooprocess IoT hub-üzeneteket és az Azure-tárolóra toostore ezeket az üzeneteket. Elolvashatja az átjáró tooyour IoT hub által küldött állapotüzenetek.

## <a name="next-steps"></a>Következő lépések
[Az Azure Storage megőrzött olvasható üzenetek](iot-hub-gateway-kit-c-lesson4-read-table-storage.md).
