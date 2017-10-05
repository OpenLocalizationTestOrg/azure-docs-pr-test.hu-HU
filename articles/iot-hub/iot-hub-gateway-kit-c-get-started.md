---
title: "SensorTag eszköz & Azure IoT átjáró – első lépések |} Microsoft Docs"
description: "Az IoT-átjáró Starter Kit első lépései az Azure IoT hub létrehozása és SensorTag és az IoT hub-átjáró"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az Azure iot hub, az iot-átjáró, az eszközök internetes hálózata, iot eszközkészlet első lépések"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: 56d05f4e-f2c1-4b22-8701-f01e14deead6
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 624bdc7877d5048da08897f868272fd8e8f3f7b6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-iot-gateway-starter-kit-with-a-sensortag"></a>Az IoT átjáró Starter Kit egy SensorTag az első lépései

> [!div class="op_single_selector"]
> * [SensorTag](iot-hub-gateway-kit-c-get-started.md)
> * [Szimulált eszköz](iot-hub-gateway-kit-c-sim-get-started.md)

Ebben az oktatóanyagban kezdődik, majd a használatának alapjait tanulási [IoT átjáró Starter Kit](https://aka.ms/gateway-kit). Az Intel NUC szél folyó Linux operációs rendszert futtató működik, és a [TI SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html#main). Megtudhatja, hogyan seamleesly csatlakozzon a felhőbe az eszközök Azure IoT Hub használatával.

***
**Még nem rendelkezik a csomag?:** kattintson [Itt](https://aka.ms/gateway-kit). **Nem rendelkezik egy SensorTag?:** [indítsa el a szimulált eszköz](iot-hub-gateway-kit-c-sim-get-started.md) vagy [egy SensorTag megvásárlása](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/?INTC=SensorTag&HQS=sensortag)
***

## <a name="lesson-1-configure-your-nuc"></a>1. lecke: A NUC konfigurálása
![Lesson1 végpont diagramja](media/iot-hub-gateway-kit-lessons/e2e-lesson1.png)

Ebben a leckében Intel NUC (következő egység a Informatika) Azure IoT átjáróként Kit beállítása, telepítse az Azure IoT biztonsági csomag NUC, és futtassa a mintaalkalmazást átjáró működésének ellenőrzéséhez.

*Oktatóanyag áttekintésének várható időtartama: 15 perc*

Ugrás a [Intel NUC IoT átjáróként beállítása](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)

## <a name="lesson-2-create-your-iot-hub"></a>2. lecke: Az IoT hub létrehozása
![Lesson2 végpont diagramja](media/iot-hub-gateway-kit-lessons/e2e-lesson2.png)

Ez a lecke az eszközök és szoftverek telepítéséhez a gazdaszámítógépen. Ezután a ingyenes Azure-fiók létrehozásához, kiépíteni az Azure IoT hub és az IoT hub létrehozása az első eszköz.

Teljes lecke 1 Ez a lecke megkezdése előtt.

### <a name="get-the-tools"></a>Eszközök letöltése
Az eszközök és a szoftver telepítése a gazdaszámítógépen.

*Oktatóanyag áttekintésének várható időtartama: 20 perc*

Ugrás a [eszközök](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)

### <a name="create-an-iot-hub-and-register-your-device"></a>Létrehoz egy IoT-központot, és regisztrálja az eszközt
Az erőforráscsoport létrehozásához, telepítéséhez az első Azure IoT hub és az első eszköz hozzáadása az IoT hub, az Azure parancssori felület használatával.

*Oktatóanyag áttekintésének várható időtartama: 10 perc*

Ugrás a [létrehoz egy IoT-központot, és regisztrálja az eszközt](iot-hub-gateway-kit-c-lesson2-register-device.md)

## <a name="lesson-3-receive-messages-from-sensortag-and-read-messages-from-your-iot-hub"></a>3. lecke: SensorTag érkező üzenetek fogadására és üzenetek olvasni az IoT hub
Ez a lecke használandó automatizálására Gedélyezése mintaalkalmazás végrehajtásának és a konfigurációs az átjáró a parancsfájlok. Az ilyen alkalmazások modulok aggregátum- vagy átalakítási adatok gyűjteménye, parancsok, vagy műveletet hajt végre tetszőleges számú kapcsolódó feladat. Modulok egy üzenet közvetítőn keresztül kommunikáljanak egymással. A mintaalkalmazás BLA modul, ezért az IoT hub modul rendelkezik. A táblázat modul adatokat fogad az int SensorTag. Az IoT hub-modul a fogadott adatok csomagokat, és elküldi az IoT hub a megadott Azure IoT peremhálózati átjáró keretrendszeren keresztül.

![Diagram. a végpont 3. rész](media/iot-hub-gateway-kit-lessons/e2e-lesson3.png)

### <a name="configure-and-run-the-ble-sample-app"></a>Konfigurálja és BLA mintaalkalmazás futtatása
Összekapcsolhatók a SensorTag és az átjárót. Majd befejezni a konfigurálást, és futtassa a BLA mintaalkalmazást.

*Oktatóanyag áttekintésének várható időtartama: 15 perc*

Ugrás a [BLA mintaalkalmazás futtatása és konfigurálása](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)

### <a name="read-messages-from-your-iot-hub"></a>Az IoT hub olvasható üzenetek
Futtassa a mintakódot az IoT hub üzenetek olvasásakor a gazdaszámítógépen.

*Oktatóanyag áttekintésének várható időtartama: 15 perc*

Ugrás a [üzenetek olvasni az IoT hub](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)

## <a name="lesson-4-save-messages-to-azure-table-storage"></a>4. lecke: Üzenetek mentése az Azure Table Storage szolgáltatásba
Hozzon létre egy Azure függvény alkalmazást, amely a bejövő üzenetek lekérése az IoT hub, és írja őket az Azure Table storage.

![Rész 4 végpont diagramja](media/iot-hub-gateway-kit-lessons/e2e-lesson4.png)

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Egy Azure függvény app és az Azure Storage-fiók létrehozása
Azure Resource Manager-sablonok segítségével hozzon létre egy Azure függvény alkalmazást és egy Azure Storage-fiók.

*Oktatóanyag áttekintésének várható időtartama: 10 perc*

Ugrás a [egy Azure függvény app és az Azure Storage-fiók létrehozása](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md)

### <a name="read-messages-persisted-in-azure-table-storage"></a>Azure Table storage-ban tárolt üzenetek olvasása
Az átjáró-a-felhőbe küldött üzeneteket, az Azure Table storage írás figyelése

*Oktatóanyag áttekintésének várható időtartama: 5 perc*

Ugrás a [Azure Table storage-ban tárolt üzenetek olvasása](iot-hub-gateway-kit-c-lesson4-read-table-storage.md).

## <a name="troubleshooting"></a>Hibaelhárítás
Ha bármilyen probléma során használatáról, keressen megoldásokat a [hibaelhárítás](iot-hub-gateway-kit-c-troubleshooting.md) cikk.

## <a name="explore-more"></a>További részletek
Látogasson el a [Intel IoT-átjáró csomag fejlesztői zóna](http://software.intel.com/iot/microsoft-azure) további.