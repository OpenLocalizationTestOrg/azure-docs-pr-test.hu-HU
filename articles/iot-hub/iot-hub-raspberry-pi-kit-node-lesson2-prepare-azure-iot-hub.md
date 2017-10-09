---
featureFlags: usabilla
title: "Csatlakozás málna Pi (csomópont) tooAzure IoT - lecke 2: eszköz regisztrálása |} Microsoft Docs"
description: "Hozzon létre egy erőforráscsoportot, hozzon létre egy Azure IoT-központot, és Pi regisztrálni kell az IoT-központ identitásjegyzékhez hello Azure parancssori felület használatával."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "Raspberry pi felhő pi felhő csatlakozás"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 736215b6-e7e4-46f9-af30-0ded9ffa5204
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 97533298d52d1187c49a4c35ddda922d6e45c87d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-iot-hub-and-register-raspberry-pi-3"></a>Az IoT hub létrehozni és regisztrálni az málna Pi 3
## <a name="what-you-will-do"></a>Mit fog
* Hozzon létre egy erőforráscsoportot.
* Az Azure IoT hub létrehozása hello erőforráscsoportban.
* Adja hozzá a málna Pi 3 toohello Azure IoT hub hello Azure parancssori felület (CLI) használatával.

Az Azure parancssori felület tooadd Pi tooyour IoT-központ használatakor hello szolgáltatás kulcsot hoz létre egy az Pi tooauthenticate hello szolgáltatásban. Ha bármilyen problémába ütközik, a keresési hello megoldások [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:
* Hogyan toouse Azure CLI toocreate az IoT-központ
* Hogyan toocreate az IoT hub eszköz identitása pi

## <a name="what-you-need"></a>Mi szükséges
* Az Azure-fiók
* Mac vagy Windows-számítógép hello Azure parancssori felület telepítése

## <a name="create-your-iot-hub"></a>Az IoT hub létrehozása
Azure IoT-központ segítségével csatlakozzon, figyeléséhez és több millió az IoT-eszközök kezelése. toocreate az IoT hub, kövesse az alábbi lépéseket:

1. Jelentkezzen be tooyour Azure-fiók hello a következő parancs futtatásával:

   ```bash
   az login
   ```

   Az elérhető előfizetések sikeres bejelentkezés után vannak felsorolva.

2. Állítsa be a hello alapértelmezett előfizetést, amelyet az toouse hello a következő parancs futtatásával:

   ```bash
   az account set --subscription {subscription id or name}
   ```

   `subscription ID or name`Itt található: hello hello kimenete `az login` vagy hello `az account list` parancsot.

3. Hello szolgáltató regisztrálása hello a következő parancs futtatásával. Erőforrás-szolgáltató a szolgáltatások, hogy biztosít erőforrásokat az alkalmazáshoz. Hello szolgáltató ajánlatok hello Azure-erőforrás telepítése előtt regisztrálnia kell az hello szolgáltató.

   ```bash
   az provider register -n "Microsoft.Devices"
   ```
4. Hozzon létre egy erőforráscsoportot nevű iot-minta hello USA nyugati régiója régióban hello a következő parancs futtatásával:

   ```bash
   az group create --name iot-sample --location westus
   ```

   `westus`az hello hely, az erőforráscsoport létrehozásához. Ha máshová szeretné toouse, futtathatja `az account list-locations -o table` toosee összes hello Azure támogatja helyét.
 
5. IoT hub létrehozása hello iot-minta erőforráscsoportban hello a következő parancs futtatásával:

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-sample
   ```

   Alapértelmezés szerint hello létrehoz egy IoT-központot hello ingyenes tarifacsomag. További információkért lásd: [Azure IoT Hub árképzési](https://azure.microsoft.com/pricing/details/iot-hub/).

> [!NOTE] 
> az IoT hub hello nevének globálisan egyedi kell lennie. Az Azure-előfizetéshez tartozó Azure IoT Hub kiadása csak egy F1 hozhat létre.

## <a name="register-pi-in-your-iot-hub"></a>Az IoT hub Pi regisztrálása
Minden eszköz, amely üzeneteket küld üzenetek tooyour IoT-központot, illetve üzeneteket fogad az IoT hub regisztrálni kell egy egyedi azonosítót. Először a Azure CLI tooregister a telepítővel, eszközhitelesítési X.509 önaláírt tanúsítvány létrehozása.

Futtassa a következő parancs hello:

```bash
# For Windows command prompt
az iot device create --device-id myraspberrypi --hub-name {my hub name} --x509 --output-dir %USERPROFILE%\.iot-hub-getting-started
 
# For macOS or Ubuntu
az iot device create --device-id myraspberrypi --hub-name {my hub name} --x509 --output-dir ~/.iot-hub-getting-started
```

## <a name="summary"></a>Összefoglalás
Az IoT-központ elkészítette és Pi regisztrálva az IoT hub eszköz megadásával. Most készen áll a toolearn hogyan toosend a Pi tooyour IoT-központ érkező üzenetek.

## <a name="next-steps"></a>Következő lépések
[Hozzon létre egy Azure függvény alkalmazást és egy Azure storage-fiók tooprocess és az IoT hub üzenetek tárolásához](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md)

