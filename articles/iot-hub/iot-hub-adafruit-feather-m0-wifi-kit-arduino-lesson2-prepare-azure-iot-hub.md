---
title: "Csatlakozás Azure IoT - lecke 2 Arduino: eszköz regisztrálása |} Microsoft Docs"
description: "Hozzon létre egy erőforráscsoportot, Azure IoT hub létrehozása és az Azure IoT hub Adafruit lágyított M0 Wi-Fi regisztrálása az Azure parancssori felület használatával."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "eszköz, arduino felhő arduino csatlakozik a felhő, az azure iot hub, dolgot felhőalapú azure iot-központ az eszközök internetes létrehozása"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: 5edc690b-7a1d-4ebc-b011-ff27bfffe6e8
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: c5ad5e900671c7cedd3cdad2c2aa345315de223b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-your-iot-hub-and-register-your-adafruit-feather-m0-wifi-arduino-board"></a>Az IoT hub létrehozása és regisztrálása a Adafruit lágyított M0 Wi-Fi Arduino board

## <a name="what-you-will-do"></a>Mit fog
* Hozzon létre egy erőforráscsoportot.
* Az Azure IoT hub létrehozása az erőforráscsoportban.
* A Arduino board az Azure parancssori felület (CLI) használatával adja hozzá az Azure IoT hub.

Ha az Azure parancssori felület használatával adja hozzá a Arduino board az IoT hub, a szolgáltatás kulcsot hoz létre egy az a Arduino üzenőfalon, a szolgáltatás szolgáltatással való hitelesítésre. Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshoot].

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:
* Hogyan lehet az Azure CLI segítségével létrehoz egy IoT-központot.
* Hogyan hozhat létre egy eszközidentitás a Arduino kártya az IoT hub.

## <a name="what-you-need"></a>Mi szükséges
* Az Azure-fiók
* Az Azure parancssori felület telepítve rendelkező számítógép

## <a name="create-your-iot-hub"></a>Az IoT hub létrehozása
Azure IoT-központ segítségével csatlakozzon, figyeléséhez és több millió az IoT-eszközök kezelése. Az IoT hub létrehozásához kövesse az alábbi lépéseket:

1. Jelentkezzen be az Azure-fiókjával a következő parancs futtatásával:

   ```bash
   az login
   ```

   Az elérhető előfizetések sikeres bejelentkezés után vannak felsorolva.

2. Állítsa be az alapértelmezett előfizetést szeretné használni a következő parancs futtatásával:

   ```bash
   az account set --subscription {subscription id or name}
   ```

   `subscription ID or name`Itt található: a kimenetét a `az login` vagy a `az account list` parancsot.

3. Regisztrálja a szolgáltatót a következő parancs futtatásával. Erőforrás-szolgáltató a szolgáltatások, hogy biztosít erőforrásokat az alkalmazáshoz. Az Azure-erőforrás, a szolgáltató által telepítése előtt regisztrálnia kell a szolgáltatót.

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. Hozzon létre egy erőforráscsoportot az USA nyugati régiója régióban iot-minta a következő parancs futtatásával:

   ```bash
   az group create --name iot-sample --location westus
   ```

   `westus`az a hely, az erőforráscsoport létrehozásához. Ha egy másik hely használni kívánt, futtathatja `az account list-locations -o table` megtekintéséhez az összes hely Azure támogatja.

5. Az iot-minta erőforráscsoportban IoT hub létrehozása a következő parancs futtatásával:

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

Alapértelmezés szerint a létrehoz egy IoT-központ az ingyenes tarifacsomag. További információkért lásd: [Azure IoT Hub árképzési](https://azure.microsoft.com/pricing/details/iot-hub/).

> [!NOTE]
> Az IoT hub nevét globálisan egyedinek kell lennie.
> Az Azure-előfizetéshez tartozó Azure IoT Hub kiadása csak egy F1 hozhat létre.

## <a name="register-your-arduino-board-in-your-iot-hub"></a>Regisztrálja a Arduino board az IoT hub
Minden eszköz, amely üzeneteket küld az IoT hub, valamint üzeneteket fogad az IoT hub regisztrálni kell egy egyedi azonosítót.

Az IoT hub a Arduino board regisztrálása fut a következő parancsot:

```bash
az iot device create --device-id mym0wifi --hub-name {my hub name}
```

## <a name="summary"></a>Összefoglalás
Az IoT-központ elkészítette és a Arduino board regisztrálva az IoT hub eszköz identitással. Készen áll a Arduino board üzenetek küldése az IoT hub módjáról.

## <a name="next-steps"></a>Következő lépések
[Hozzon létre egy Azure függvény alkalmazást és feldolgozni, és az IoT hub üzenetek tárolásához Azure Storage-fiók][process-and-store-iot-hub-messages].


<!-- Images and links -->

[troubleshoot]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md