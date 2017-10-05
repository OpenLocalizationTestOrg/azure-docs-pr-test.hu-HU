---
title: "Szimulált eszköz & Azure IoT átjáró – első lépések |} Microsoft Docs"
description: "Az IoT-átjáró Starter Kit első lépései az Azure IoT hub létrehozása és csatlakozás az IoT hub-átjáró"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az Azure iot hub, az iot-átjáró, az eszközök internetes hálózata, iot eszközkészlet első lépések"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 0c110b8b-bee4-4aec-a18a-dfc292aa17a3
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 916fa40d9ac857dfa72197b40c232834593d3891
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-iot-gateway-starter-kit-with-a-simulated-device"></a>A szimulált eszköz Ismerkedés az IoT-átjáró Starter Kit

> [!div class="op_single_selector"]
> * [SensorTag](iot-hub-gateway-kit-c-get-started.md)
> * [Szimulált eszköz](iot-hub-gateway-kit-c-sim-get-started.md)

Ebben az oktatóanyagban kezdődik, majd a használatának alapjait tanulási [IoT átjáró Starter Kit](https://aka.ms/gateway-kit). Dolgozni fog az Intel NUC szél folyó Linux operációs rendszert futtató. Megtudhatja, hogyan seamleesly csatlakozzon a felhőbe az eszközök Azure IoT Hub használatával.

***
**Még nem rendelkezik a csomag?:** kattintson [Itt](https://aka.ms/gateway-kit).
***

## <a name="lesson-1-configure-your-nuc"></a>1. lecke: A NUC konfigurálása
![Lesson1 végpont diagramja](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson1.png)

Ebben a leckében Intel NUC (következő egység a Informatika) Azure IoT átjáróként Kit beállítása, telepítse az Azure IoT biztonsági csomag NUC, és futtassa a mintaalkalmazást átjáró működésének ellenőrzéséhez.

*Oktatóanyag áttekintésének várható időtartama: 15 perc*

Ugrás a [Intel NUC IoT átjáróként beállítása](iot-hub-gateway-kit-c-sim-lesson1-set-up-nuc.md)

## <a name="lesson-2-create-your-iot-hub"></a>2. lecke: Az IoT hub létrehozása
![Lesson2 végpont diagramja](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson2.png)

Ez a lecke az eszközök és szoftverek telepítéséhez a gazdaszámítógépen. Ezután a ingyenes Azure-fiók létrehozásához, kiépíteni az Azure IoT hub és az IoT hub létrehozása az első eszköz.

Teljes lecke 1 Ez a lecke megkezdése előtt.

### <a name="get-the-tools"></a>Eszközök letöltése
Az eszközök és a szoftver telepítése a gazdaszámítógépen.

*Oktatóanyag áttekintésének várható időtartama: 20 perc*

Ugrás a [eszközök](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)

### <a name="create-an-iot-hub-and-register-your-device"></a>Létrehoz egy IoT-központot, és regisztrálja az eszközt
Az erőforráscsoport létrehozásához, telepítéséhez az első Azure IoT hub és az első eszköz hozzáadása az IoT hub, az Azure parancssori felület használatával.

*Oktatóanyag áttekintésének várható időtartama: 10 perc*

Ugrás a [létrehoz egy IoT-központot, és regisztrálja az eszközt](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)

## <a name="lesson-3-receive-messages-from-the-simulated-device-and-read-messages-from-your-iot-hub"></a>3. lecke: Üzeneteket fogadjon a szimulált eszköz, és üzeneteket beolvasni az IoT hub
Ez a lecke parancsfájlok használandó automatizálhatja a konfigurációs és a szimulált eszköz alkalmazások futtatásához az átjáró található. A szimulált eszköz alkalmazásának minta hőmérséklet adatokat állít elő, és elküldi az IoT hub modul. Az IoT hub-modul a fogadott adatok csomagokat, és elküldi az IoT hub a megadott Azure IoT peremhálózati átjáró keretrendszeren keresztül.

![Diagram. a végpont 3. rész](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson3.png)

### <a name="configure-and-run-a-simulated-device"></a>Konfigurálja és futtassa a szimulált eszköz
Készítse elő a minta-kódokat. Ezután konfigurálja, és futtassa a szimulált eszköz mintaalkalmazást.

*Oktatóanyag áttekintésének várható időtartama: 15 perc*

Ugrás a [beállítása és futtatása a szimulált eszköz](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md)

### <a name="read-messages-from-your-iot-hub"></a>Az IoT hub olvasható üzenetek
Futtassa a mintakódot az üzenetek olvasásakor az IoT hub a gazdaszámítógépen.

*Oktatóanyag áttekintésének várható időtartama: 15 perc*

Ugrás a [üzenetek olvasni az IoT hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)

## <a name="lesson-4-save-messages-to-azure-table-storage"></a>4. lecke: Üzenetek mentése az Azure Table Storage szolgáltatásba
Hozzon létre egy Azure függvény alkalmazást, amely a bejövő üzenetek lekérése az IoT hub, és írja őket az Azure Table storage.

![Rész 4 végpont diagramja](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson4.png)

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Egy Azure függvény app és az Azure Storage-fiók létrehozása
Azure Resource Manager-sablonok segítségével hozzon létre egy Azure függvény alkalmazást és egy Azure Storage-fiók.

*Oktatóanyag áttekintésének várható időtartama: 10 perc*

Ugrás a [egy Azure függvény app és az Azure Storage-fiók létrehozása](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md)

### <a name="read-messages-persisted-in-azure-table-storage"></a>Azure Table storage-ban tárolt üzenetek olvasása
Az átjáró-a-felhőbe küldött üzeneteket, az Azure Table storage írás figyelése

*Oktatóanyag áttekintésének várható időtartama: 5 perc*

Ugrás a [Azure Table storage-ban tárolt üzenetek olvasása](iot-hub-gateway-kit-c-sim-lesson4-read-table-storage.md).

## <a name="troubleshooting"></a>Hibaelhárítás
Ha bármilyen probléma során használatáról, keressen megoldásokat a [hibaelhárítás](iot-hub-gateway-kit-c-sim-troubleshooting.md) cikk.

## <a name="explore-more"></a>További részletek
Látogasson el a [Intel IoT-átjáró csomag fejlesztői zóna](https://software.intel.com/en-us/iot/hardware/gateways/dev-kit) további.