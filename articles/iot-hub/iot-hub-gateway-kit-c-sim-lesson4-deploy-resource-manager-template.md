---
title: "A szimulált eszköz & Azure IoT átjáró - lecke 4: üzenetek menthetők |} Microsoft Docs"
description: "Intel NUC üzenetek mentéséhez az IoT hub, Azure Table Storage írja őket, és majd olvashatja őket a felhőből."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "adatok tárolása a felhőben, felhőben tárolt adatok iot felhőalapú szolgáltatás"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: ffed0c2e-b092-40e1-9113-8196ec057d67
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c7fc47b07acede28ffe790debca7e38521726011
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-function-app-and-storage-account"></a>Azure-függvényalkalmazás és -tárfiók létrehozása

Az Azure Functions megoldással egyszerűen futtathatók _funkciók_ (kis kódrészletek) a felhőben. Egy Azure függvényalkalmazás biztosítja a a függvények végrehajtásához szükséges az Azure-ban. 

## <a name="what-you-will-do"></a>Mit fog

- Az Azure Resource Manager-sablon használatával hozzon létre egy Azure függvény alkalmazást és egy Azure storage-fiók. Az Azure-függvény alkalmazás Azure IoT hub eseményeket figyeli, dolgozza fel a bejövő üzenetek, és írja őket az Azure Table storage.

Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-gateway-kit-c-sim-troubleshooting.md).


## <a name="what-you-will-learn"></a>Amiről tanulni fog

Ez a lecke témák köre:

- Hogyan használható az Azure Resource Manager központi telepítése az Azure-erőforrások.
- Hogyan használható az Azure-függvény alkalmazás IoT-központ üzenetek feldolgozásához, és az Azure Table storage egy táblához.

## <a name="what-you-need"></a>Mi szükséges

Sikeresen végrehajtotta a korábbi rendelkezésre:

- [1. lecke: Állítsa be az Intel NUC IoT átjáróként](iot-hub-gateway-kit-c-sim-lesson1-set-up-nuc.md)
- [2. lecke: Felkészülés a gazdaszámítógép és Azure IoT-központ](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)
- [3. lecke: Üzeneteket fogadjon a szimulált eszköz, és üzeneteket beolvasni az IoT hub](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md)

## <a name="open-a-sample-app"></a>Nyissa meg a mintaalkalmazás

Lépjen a `iot-hub-c-intel-nuc-gateway-getting-started` tárház mappa inicializálni a konfigurációs fájlokat, és ezután nyissa meg a minta projektet a Visual Studio Code a következő parancs futtatásával:

```bash
cd Lesson4
npm install
gulp init
code .
```

![Tárház szerkezete](media/iot-hub-gateway-kit-lessons/lesson4/arm_template.png)

- A `arm-template.json` fájl az Azure Resource Manager sablon, amely tartalmazza az Azure-függvény alkalmazás és egy Azure storage-fiók.
- A `arm-template-param.json` a konfigurációs fájl az Azure Resource Manager-sablon által használt.
- A `ReceiveDeviceMessages` tartalmazza, a Node.js-kódot az Azure-függvény.

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>Konfigurálja az Azure Resource Manager-sablonok és erőforrások létrehozása az Azure-ban

Frissítés a `arm-template-param.json` fájlt a Visual Studio Code.

![ARM-sablon json](media/iot-hub-gateway-kit-lessons/lesson4/arm_template_param.png)

- Cserélje le `[your IoT Hub name]` rendelkező `{my hub name}` , amelyet a 2.

Miután frissítette a `arm-template-param.json` fájlt, telepítheti az erőforrásokat az Azure, a következő parancs futtatásával:

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-gateway
```

Használjon `iot-gateway` értékeként `{resource group name}` Ha nem módosítja az érték a 2.

## <a name="summary"></a>Összefoglalás

Az Azure-függvény alkalmazás feldolgozni az IoT hub üzenetek és az Azure-tárfiók ezek az üzenetek tárolásához létrehozott. Elolvashatja az átjárót az IoT hub által küldött állapotüzenetek.

## <a name="next-steps"></a>Következő lépések
[Az Azure Storage megőrzött olvasható üzenetek](iot-hub-gateway-kit-c-sim-lesson4-read-table-storage.md).
