---
title: "Intel Edison (csomópont) csatlakozni az Azure IoT - lecke 2: eszköz regisztrálása |} Microsoft Docs"
description: "Hozzon létre egy erőforráscsoportot, Azure IoT hub létrehozása és az Azure IoT hub Edison regisztrálása az Azure parancssori felület használatával."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: 
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-intel-edison-kit-node-get-started
ms.assetid: c1465cc2-d0d9-4326-967c-64d95b61dd54
ms.service: iot-hub
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 81584c1b7314c4331ac059b8e3ed782970588ae2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-your-iot-hub-and-register-intel-edison"></a>Az IoT hub létrehozni és regisztrálni az Intel Edison
## <a name="what-you-will-do"></a>Mit fog
* Hozzon létre egy erőforráscsoportot.
* Az Azure IoT hub létrehozása az erőforráscsoportban.
* Intel Edison az Azure parancssori felület (CLI) használatával adja hozzá az Azure IoT hub.

Ha az Azure parancssori felület használatával Edison hozzáadása az IoT hub, a szolgáltatás kulcsot hoz létre egy az Edison a service szolgáltatással való hitelesítésre. Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási][troubleshooting].

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:
* Hogyan lehet az Azure CLI segítségével létrehoz egy IoT-központot.
* Hogyan hozhat létre egy eszközidentitás a Edison az IoT hub.

## <a name="what-you-need"></a>Mi szükséges
* Egy Azure-fiók. Ha az Azure-fiók nem rendelkezik, hozzon létre egy [ingyenes Azure próba-fiókot](http://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.
* Az Azure parancssori felület telepítése szükséges.

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


## <a name="register-edison-in-your-iot-hub"></a>Az IoT hub Edison regisztrálása
Minden eszköz, amely üzeneteket küld az IoT hub, valamint üzeneteket fogad az IoT hub regisztrálni kell egy egyedi azonosítót.

Az IoT hub Edison regisztrálása fut a következő parancsot:

```bash
az iot device create --device-id myinteledison --hub-name {my hub name}
```

## <a name="summary"></a>Összefoglalás
Az IoT-központ elkészítette és Edison regisztrálva az IoT hub eszköz megadásával. Készen áll arra, hogyan Edison üzenetek küldése az IoT hub.

## <a name="next-steps"></a>Következő lépések
[Hozzon létre egy Azure függvény alkalmazást és feldolgozni, és az IoT hub üzenetek tárolásához Azure Storage-fiók][process-and-store-iot-hub-messages].


<!-- Images and links -->

[troubleshooting]: iot-hub-intel-edison-kit-node-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-intel-edison-kit-node-lesson3-deploy-resource-manager-template.md