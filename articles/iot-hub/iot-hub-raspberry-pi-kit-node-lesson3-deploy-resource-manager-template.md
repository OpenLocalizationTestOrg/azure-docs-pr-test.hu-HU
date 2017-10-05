---
title: "Csatlakozás Azure IoT - lecke 3 málna Pi (csomópont): sablon-üzembehelyezés |} Microsoft Docs"
description: "Az Azure-függvény alkalmazás Azure IoT hub eseményeket figyeli, dolgozza fel a bejövő üzenetek, és írja őket az Azure Table storage."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "adatok tárolása a felhőben, felhőben tárolt adatok iot felhőalapú szolgáltatás"
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
ms.openlocfilehash: 44901faea37a847a418e6d2b4097302cdb610495
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-function-app-and-azure-storage-account"></a>Az Azure-függvény app és az Azure storage-fiók létrehozása
[Az Azure Functions](../azure-functions/functions-overview.md) egyszerűen futtathatók megoldás *funkciók* (kis kódrészletek) a felhőben. Egy Azure függvényalkalmazás biztosítja a a függvények végrehajtásához szükséges az Azure-ban.

## <a name="what-you-will-do"></a>Mit fog
Az Azure Resource Manager-sablon használatával hozzon létre egy Azure függvény alkalmazást és egy Azure storage-fiók. Az Azure-függvény alkalmazás Azure IoT hub eseményeket figyeli, dolgozza fel a bejövő üzenetek, és írja őket az Azure Table storage. Ha bármilyen problémába ütközik, a keresés megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:

* Használata [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) központi telepítése az Azure-erőforrások.
* Hogyan használható az Azure-függvény alkalmazás IoT hub üzenetek feldolgozásához, és az Azure Table storage egy táblához.

## <a name="what-you-need"></a>Mi szükséges
Sikeresen végrehajtotta:
* [Ismerkedés a málna Pi 3](iot-hub-raspberry-pi-kit-node-get-started.md)
* [Az Azure IoT hub létrehozása](iot-hub-raspberry-pi-kit-node-get-started.md)

## <a name="open-the-sample-app"></a>Nyissa meg a mintaalkalmazás
Nyissa meg a minta-projektet a Visual Studio Code a következő parancsok futtatásával:

```bash
cd Lesson3
code .
```

![Tárház szerkezete](media/iot-hub-raspberry-pi-lessons/lesson3/repo_structure.png)

* A `app.js` fájlt a `app` almappában a kulcs forrásfájl. A forrásfájl üzenet küldése 20 alkalommal az IoT hub, és minden üzenetet küld a LED villogni kódját tartalmazza.
* A `arm-template.json` fájl az Azure Resource Manager sablon, amely tartalmazza az Azure-függvény alkalmazás és egy Azure storage-fiók.
* A `arm-template-param.json` a konfigurációs fájl az Azure Resource Manager-sablon által használt.
* A `ReceiveDeviceMessages` tartalmazza, a Node.js-kódot az Azure-függvény.

## <a name="configure-azure-resource-manager-templates-and-create-resources-in-azure"></a>Konfigurálja az Azure Resource Manager-sablonok és erőforrások létrehozása az Azure-ban
Frissítés a `arm-template-param.json` fájlt a Visual Studio Code.

![Az Azure Resource Manager sablon paraméterek](media/iot-hub-raspberry-pi-lessons/lesson3/arm_para.png)

* Cserélje le **[az IoT Hub name]** rendelkező **{saját központnevet}** , hogy mikor megadott meg [az IoT hub létrehozása és a regisztrált málna Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md).
* Cserélje le **[előtag-karakterlánc az új erőforrások]** bármely kívánt előtaggal. Az előtag biztosítja, hogy az erőforrás nevének globálisan egyedi ütközés elkerülése érdekében. Ne használjon kötőjel vagy kezdeti az előtag a számot.

Miután frissítette a `arm-template-param.json` fájlt, telepítheti az erőforrásokat az Azure, a következő parancs futtatásával:

```bash
az group deployment create --template-file arm-template.json --parameters @arm-template-param.json -g iot-sample
```

Ezek az erőforrások létrehozásához körülbelül öt percet vesz igénybe. Amíg az erőforrás létrehozása folyamatban van, továbbléphet a következő cikk tartalmazza.

## <a name="summary"></a>Összefoglalás
Az Azure-függvény alkalmazás feldolgozni az IoT hub üzenetek és az Azure-tárfiók ezek az üzenetek tárolásához létrehozott. Most már telepítheti és futtathatja a Pi eszköz a felhőbe küldött üzeneteket küldeni.

## <a name="next-steps"></a>Következő lépések
[Futtassa a mintaalkalmazást eszköz a felhőbe küldött üzeneteket küldjön málna Pi 3](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

