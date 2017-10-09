---
title: "SensorTag eszköz & Azure IoT átjáró - rész 2: eszköz regisztrálása |} Microsoft Docs"
description: 
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az Azure iot hub dolgot felhőalapú azure iot-központ az internet létrehozása eszköz, a ti sensortag, a ti bla"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 2c18f5ae-e39a-48ae-a9fe-04bb595740a0
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 5d2322268aa18f52f60c2833778323773ac4eec3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-azure-iot-hub-and-register-your-device"></a>Az Azure IoT hub létrehozása, és regisztrálja az eszközt

## <a name="what-you-will-do"></a>Mit fog

- Hozzon létre egy erőforráscsoportot
- Az első IoT hub létrehozása
- Regisztrálja az eszközt az IoT hub hello Azure parancssori felület használatával. 

Amikor regisztrál az eszközt az IoT hub, hello Azure IoT-központ szolgáltatás kulcsot hoz létre egy az az eszköz toouse tooauthenticate hello szolgáltatásban. 

Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Amiről tanulni fog

Ez a lecke témák köre:

- Hogyan toouse hello Azure CLI toocreate egy IoT-központot.
- Hogyan tooregister egy eszközt az IoT-központ.

## <a name="what-you-need"></a>Mi szükséges

- Aktív Azure-előfizetés. Ha az Azure-fiók nem rendelkezik, akkor létrehozhat egy [ingyenes Azure próba-fiókot](http://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.
- Az Azure parancssori felület telepítve hello kell rendelkeznie.

## <a name="create-an-iot-hub"></a>IoT Hub létrehozása

az IoT-központ toocreate kövesse az alábbi lépéseket:

1. Jelentkezzen be tooyour Azure-fiók hello a következő parancs futtatásával:

   ```bash
   az login
   ```

   Sikeres bejelentkezés után megjelenik az elérhető előfizetések.

2. Hello alapértelmezett beállítása Azure-előfizetést, amelyet az toouse hello a következő parancs futtatásával:

   ```bash
   az account set --subscription {subscription id or name}
   ```

   `subscription ID or name`Itt található: hello hello kimenete `az login` vagy hello `az account list` parancsot.

3. Hello szolgáltató regisztrálása hello a következő parancs futtatásával. Erőforrás-szolgáltató a szolgáltatások, hogy biztosít erőforrásokat az alkalmazáshoz. Hello szolgáltató ajánlatok hello Azure-erőforrás telepítése előtt regisztrálnia kell az hello szolgáltató.

   ```bash
   az provider register -n "Microsoft.Devices"
   ```

4. Hozzon létre egy erőforráscsoportot nevű `iot-gateway` hello USA nyugati régiója régióban hello a következő parancs futtatásával:

   ```bash
   az group create --name iot-gateway --location westus
   ```
   
   `westus`az hello hely, az erőforráscsoport létrehozásához. Ha máshová szeretné toouse, futtathatja `az account list-locations -o table` toosee összes hello Azure támogatja helyét.

5. Hozzon létre egy IoT-központot hello `iot-gateway` erőforráscsoport hello a következő parancs futtatásával:

   ```bash
   az iot hub create --name {my hub name} --resource-group iot-gateway
   ```

Alapértelmezés szerint hello létrehoz egy IoT-központot hello ingyenes tarifacsomag. További információkért lásd: [Azure IoT Hub árképzési](https://azure.microsoft.com/pricing/details/iot-hub/).

> [!NOTE]
> az IoT hub hello nevének globálisan egyedi kell lennie. Az Azure-előfizetéshez tartozó Azure Iot Hub kiadása csak egy F1 hozhat létre.

## <a name="register-your-device-in-your-iot-hub"></a>Regisztrálja az eszközt az IoT hub

Minden eszköz, amely üzeneteket küld üzenetek tooyour IoT-központot, illetve üzeneteket fogad az IoT hub regisztrálni kell egy egyedi azonosítót.
Az eszköz regisztrálása az IoT hub fut a következő parancsot:

```bash
az iot device create --device-id mydevice --hub-name {my hub name} --resource-group iot-gateway
```

## <a name="summary"></a>Összefoglalás

Az IoT-központ elkészítette és a logikai eszköz regisztrálva az IoT hub eszköz megadásával. Most készen áll a toolearn hogyan tooconfigure és a fizikai eszköz tooyour IoT-központ a hello egy átjáró alkalmazás toosend mintaadatok futtatható felhőalapú.

## <a name="next-steps"></a>Következő lépések
[Konfigurálja és egy táblázat mintaalkalmazás futtatása](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)