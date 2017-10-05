---
title: "Az Azure IoT - lecke 2 Connect Raspberry pi (C): eszköz regisztrálása |} Microsoft Docs"
description: "Hozzon létre egy erőforráscsoportot, Azure IoT hub létrehozása és az Azure IoT hub Pi regisztrálása az Azure parancssori felület használatával."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Raspberry pi felhő pi felhő csatlakozás"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 4bcfb071-b3ae-48cc-8ea5-7e7434732287
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: d7bfd8f6ae8d15dfe09f06a40a4ab415ff2e0a7c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-your-iot-hub-and-register-raspberry-pi-3"></a>Az IoT hub létrehozni és regisztrálni az málna Pi 3
## <a name="what-you-will-do"></a>Mit fog
* Hozzon létre egy erőforráscsoportot.
* Az Azure IoT hub létrehozása az erőforráscsoportban.
* Az Azure IoT hub málna Pi 3 hozzáadása az Azure parancssori felület (CLI) használatával.

Ha az Azure parancssori felület használatával Pi hozzáadása az IoT hub, a szolgáltatás kulcsot hoz létre egy az Pi a service szolgáltatással való hitelesítésre. Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:
* Hogyan lehet az Azure CLI segítségével létrehoz egy IoT-központot.
* Hogyan hozhat létre egy eszközidentitás Pi az IoT hub.

## <a name="what-you-need"></a>Mi szükséges
* Az Azure-fiók
* A Mac vagy a Windows rendszerű számítógépeken a telepített Azure parancssori felülettel

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
> Az IoT hub nevét globálisan egyedinek kell lennie. Az Azure-előfizetéshez tartozó Azure IoT Hub kiadása csak egy F1 hozhat létre.

## <a name="register-pi-in-your-iot-hub"></a>Az IoT hub Pi regisztrálása
Minden eszköz, amely üzeneteket küld az IoT hub, valamint üzeneteket fogad az IoT hub regisztrálni kell egy egyedi azonosítót.

A központ Pi regisztrálása fut a következő parancsot:

```bash
az iot device create --device-id myraspberrypi --hub {my hub name} --resource-group iot-sample
```

## <a name="summary"></a>Összefoglalás
Az IoT-központ elkészítette és Pi regisztrálva az IoT hub eszköz megadásával. Készen áll arra, hogyan Pi üzenetek küldése az IoT hub.

## <a name="next-steps"></a>Következő lépések
[Hozzon létre egy Azure függvény alkalmazást és feldolgozni, és az IoT hub üzenetek tárolásához Azure Storage-fiók](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).

