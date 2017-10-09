---
title: "Csatlakozás málna Pi (csomópont) tooAzure IoT - lecke 3: sablon-üzembehelyezés |} Microsoft Docs"
description: "hello Azure függvény app tooAzure IoT hub eseményeket figyeli, dolgozza fel a bejövő üzenetek és tooAzure a Table storage írja azokat."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "adattárolás hello felhőben, felhőben tárolt adatok iot felhőalapú szolgáltatás"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 6c58de85-c5c4-4989-bb5e-08c45c549966
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: b6c0a9530cb80e3f78c0e96037f6f3942b602aea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a>Az Azure-függvény app és az Azure storage-fiók létrehozása
[Az Azure Functions](../azure-functions/functions-overview.md) egyszerűen futtathatók megoldás *funkciók* (kis kódrészletek) hello felhőben. Egy Azure függvényalkalmazás hello a függvények végrehajtásához szükséges az Azure-ban.

## <a name="what-you-will-do"></a>Mit fog
Használja az Azure Resource Manager sablon toocreate Azure függvény alkalmazást és egy Azure storage-fiókot. hello Azure függvény app tooAzure IoT hub eseményeket figyeli, dolgozza fel a bejövő üzenetek és tooAzure a Table storage írja azokat. Ha bármilyen problémába ütközik, a keresési hello megoldások [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:

* Hogyan toouse [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) toodeploy Azure erőforrásokat.
* Hogyan toouse az Azure app tooprocess IoT hub üzenetek működéséhez, és beírhatók tooa tábla Azure Table storage-ban.

## <a name="what-you-need"></a>Mi szükséges
Sikeresen végrehajtotta:
* [Ismerkedés a málna Pi 3](iot-hub-raspberry-pi-kit-node-get-started.md)
* [Az Azure IoT hub létrehozása](iot-hub-raspberry-pi-kit-node-get-started.md)

## <a name="open-hello-sample-app"></a>Nyissa meg hello mintaalkalmazás
Nyissa meg a Visual Studio Code hello mintaprojektet hello a következő parancsok futtatásával:

```bash
cd Lesson3
code .
```

![Tárház szerkezete](media/iot-hub-raspberry-pi-lessons/lesson3/repo_structure.png)

* Hello `app.js` hello fájlban `app` almappa is hello kulcs forrásfájl. A forrásfájl hello kód toosend küldené üzenet 20 alkalommal tooyour IoT hub és villogási hello LED minden üzenet tartalmazza.
* Hello `arm-template.json` fájl hello Azure Resource Manager-sablon, amely tartalmazza az Azure-függvény alkalmazás és egy Azure storage-fiók.
* Hello `arm-template-param.json` hello konfigurációs fájl hello Azure Resource Manager-sablon által használt.
* Hello `ReceiveDeviceMessages` hello Azure függvény hello Node.js kódját tartalmazza.

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>Konfigurálja az Azure Resource Manager-sablonok és erőforrások létrehozása az Azure-ban
Frissítés hello `arm-template-param.json` fájlt a Visual Studio Code.

![Az Azure Resource Manager sablon paraméterek](media/iot-hub-raspberry-pi-lessons/lesson3/arm_para.png)

* Cserélje le **[az IoT Hub name]** rendelkező **{saját központnevet}** , hogy mikor megadott meg [az IoT hub létrehozása és a regisztrált málna Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).
* Cserélje le **[előtag-karakterlánc az új erőforrások]** bármely kívánt előtaggal. hello előtag biztosítja az erőforrásnév hello globálisan egyedi tooavoid ütközés. Ne használjon kötőjel vagy kezdeti hello előtag a számot.

Miután frissítette hello `arm-template-param.json` fájlt, hello erőforrások tooAzure telepítése hello a következő parancs futtatásával:

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

Tart körülbelül öt perc toocreate ezeket az erőforrásokat. Hello erőforrás létrehozása folyamatban van, míg a következő cikk toohello áthelyezheti.

## <a name="summary"></a>Összefoglalás
Létrehozta az Azure-függvény app tooprocess IoT hub-üzeneteket és az Azure-tárolóra toostore ezeket az üzeneteket. Most már telepítheti és Pi hello minta toosend eszközről a felhőbe üzenetek futtatnak.

## <a name="next-steps"></a>Következő lépések
[Futtassa egy minta alkalmazás toosend eszköz a felhőbe küldött üzeneteket málna Pi 3](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

