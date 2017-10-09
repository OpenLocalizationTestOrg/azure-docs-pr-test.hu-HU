---
title: "Csatlakozás Arduino tooAzure IoT - lecke 2: eszköz regisztrálása |} Microsoft Docs"
description: "Hozzon létre egy erőforráscsoportot, Azure IoT hub létrehozása és regisztrálása Adafruit lágyított M0 Wi-Fi hello Azure IoT hub hello Azure parancssori felület használatával."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Csatlakozás arduino toocloud, az azure iot hub, internet dolgot felhőalapú azure iot hub eszköz, arduino felhő létrehozása"
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
ms.openlocfilehash: ca362f9c143dd3a98bf47a66b63a9725a0ffc2d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-iot-hub-and-register-your-adafruit-feather-m0-wifi-arduino-board"></a>Az IoT hub létrehozása és regisztrálása a Adafruit lágyított M0 Wi-Fi Arduino board

## <a name="what-you-will-do"></a>Mit fog
* Hozzon létre egy erőforráscsoportot.
* Az Azure IoT hub létrehozása hello erőforráscsoportban.
* Adja hozzá az Arduino board toohello Azure IoT hub hello Azure parancssori felület (CLI) használatával.

Hello Azure CLI tooadd használatakor az Arduino Bizottság tooyour IoT hub hello szolgáltatás kulcsot hoz létre egy az a Arduino board tooauthenticate hello szolgáltatásban. Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási][troubleshoot].

## <a name="what-you-will-learn"></a>Amiről tanulni fog
Ebből a cikkből megtudhatja:
* Hogyan toouse hello Azure CLI toocreate egy IoT-központot.
* Hogyan toocreate egy eszköz identitást a Arduino board az IoT hub.

## <a name="what-you-need"></a>Mi szükséges
* Az Azure-fiók
* Egy számítógép, amelyen hello Azure parancssori felület telepítve

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
> az IoT hub hello nevének globálisan egyedi kell lennie.
> Az Azure-előfizetéshez tartozó Azure IoT Hub kiadása csak egy F1 hozhat létre.

## <a name="register-your-arduino-board-in-your-iot-hub"></a>Regisztrálja a Arduino board az IoT hub
Minden eszköz, amely üzeneteket küld üzenetek tooyour IoT-központot, illetve üzeneteket fogad az IoT hub regisztrálni kell egy egyedi azonosítót.

Az IoT hub a Arduino board regisztrálása fut a következő parancsot:

```bash
az iot device create --device-id mym0wifi --hub-name {my hub name}
```

## <a name="summary"></a>Összefoglalás
Az IoT-központ elkészítette és a Arduino board regisztrálva az IoT hub eszköz identitással. Most készen áll a toolearn hogyan toosend üzenetek a Arduino board tooyour IoT hubról.

## <a name="next-steps"></a>Következő lépések
[Hozzon létre egy Azure függvény alkalmazás és az Azure Storage fiók tooprocess és a tároló IoT-központ üzenetek][process-and-store-iot-hub-messages].


<!-- Images and links -->

[troubleshoot]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md
[process-and-store-iot-hub-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md