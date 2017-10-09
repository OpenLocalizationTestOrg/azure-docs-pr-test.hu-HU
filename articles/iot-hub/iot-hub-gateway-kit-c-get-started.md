---
title: "SensorTag eszköz & Azure IoT átjáró – első lépések |} Microsoft Docs"
description: "Az IoT-átjáró Starter Kit első lépései az Azure IoT hub létrehozása és csatlakozás SensorTag és átjáró toohello IoT-központ"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az Azure iot hub, iot-átjáró első lépések hello az eszközök internetes hálózata, iot eszközkészlet"
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
ms.openlocfilehash: d8e4057e7774e43c069dd3f2f2e03f098c1ac844
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-iot-gateway-starter-kit-with-a-sensortag"></a>Az IoT átjáró Starter Kit egy SensorTag az első lépései

> [!div class="op_single_selector"]
> * [SensorTag](iot-hub-gateway-kit-c-get-started.md)
> * [Szimulált eszköz](iot-hub-gateway-kit-c-sim-get-started.md)

Ebben az oktatóanyagban kezdődik, majd hello használatának alapjait tanulási [IoT átjáró Starter Kit](https://aka.ms/gateway-kit). Az Intel NUC szél folyó Linux és hello futtató működik [TI SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html#main). Megtudhatja, hogyan tooseamleesly kapcsolódnak az eszközök toohello felhőalapú Azure IoT Hub használatával.

***
**Még nem rendelkezik a csomag?:** kattintson [Itt](https://aka.ms/gateway-kit). **Nem rendelkezik egy SensorTag?:** [indítsa el a szimulált eszköz](iot-hub-gateway-kit-c-sim-get-started.md) vagy [egy SensorTag megvásárlása](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/?INTC=SensorTag&HQS=sensortag)
***

## <a name="lesson-1-configure-your-nuc"></a>1. lecke: A NUC konfigurálása
![Lesson1 végpont diagramja](media/iot-hub-gateway-kit-lessons/e2e-lesson1.png)

Ez a lecke hello Azure IoT átjáróként Kit Intel NUC (következő egység a Informatika) beállítása, NUC hello Azure IoT biztonsági csomag telepítését, és futtatni egy minta app tooverify hello átjáró funkciót.

*Becsült idő toocomplete: 15 perc*

Nyissa meg túl[Intel NUC IoT átjáróként beállítása](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)

## <a name="lesson-2-create-your-iot-hub"></a>2. lecke: Az IoT hub létrehozása
![Lesson2 végpont diagramja](media/iot-hub-gateway-kit-lessons/e2e-lesson2.png)

Ez a lecke hello eszközök és szoftverek telepítéséhez a gazdaszámítógépen. Ezután a ingyenes Azure-fiók létrehozásához, kiépíteni az Azure IoT hub és az első eszköz hello IoT hub létrehozása.

Teljes lecke 1 Ez a lecke megkezdése előtt.

### <a name="get-hello-tools"></a>Hello eszközök beszerzése
A gazdaszámítógép hello eszközök és szoftverek telepítéséhez.

*Becsült idő toocomplete: 20 perc*

Nyissa meg túl[hello eszközök beszerzése](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)

### <a name="create-an-iot-hub-and-register-your-device"></a>Létrehoz egy IoT-központot, és regisztrálja az eszközt
Az erőforráscsoport létrehozása, az első Azure IoT hub kiépíteni, és adja hozzá az első eszköz toohello IoT hub hello Azure parancssori felület használatával.

*Becsült idő toocomplete: 10 perc*

Nyissa meg túl[létrehoz egy IoT-központot, és regisztrálja az eszközt](iot-hub-gateway-kit-c-lesson2-register-device.md)

## <a name="lesson-3-receive-messages-from-sensortag-and-read-messages-from-your-iot-hub"></a>3. lecke: SensorTag érkező üzenetek fogadására és üzenetek olvasni az IoT hub
Ez a lecke szüksége lesz BLA mintaalkalmazás végrehajtásának és parancsfájlok tooautomate hello konfigurációs az átjárót. Az ilyen alkalmazások modulok tooaggregate és átalakítási adatok gyűjteménye, parancsok, vagy műveletet hajt végre tetszőleges számú kapcsolódó feladat. Modulok egy üzenet közvetítőn keresztül kommunikáljanak egymással. hello mintaalkalmazás BLA modul, ezért az IoT hub modul rendelkezik. hello BLA modul adatokat fogad az int SensorTag. hello IoT hub modul csomagok hello adatokat fogad, és elküldi azt az Azure IoT Edge tooyour IoT-központ hello átjáró keretrendszere keresztül.

![Diagram. a végpont 3. rész](media/iot-hub-gateway-kit-lessons/e2e-lesson3.png)

### <a name="configure-and-run-hello-ble-sample-app"></a>Konfigurálja és hello BLA mintaalkalmazás futtatása
Összekapcsolhatók hello SensorTag és az átjárót. Majd hello konfigurálás befejezése, és futtassa hello BLA mintaalkalmazást.

*Becsült idő toocomplete: 15 perc*

Nyissa meg túl[mintaalkalmazás futtatása hello BLA és konfigurálása](iot-hub-gateway-kit-c-lesson3-configure-ble-app.md)

### <a name="read-messages-from-your-iot-hub"></a>Az IoT hub olvasható üzenetek
Futtassa mintakód a gazdagépen futó számítógép tooread üzenetet az IoT hub.

*Becsült idő toocomplete: 15 perc*

Nyissa meg túl[üzenetek olvasni az IoT hub](iot-hub-gateway-kit-c-lesson3-read-messages-from-hub.md)

## <a name="lesson-4-save-messages-tooazure-table-storage"></a>4. lecke: Üzenetek tooAzure a Table storage mentése
Hozzon létre, hogy a bejövő üzenetek lekérése az IoT hub, majd beírja őket tooAzure a Table storage Azure függvény alkalmazások.

![Rész 4 végpont diagramja](media/iot-hub-gateway-kit-lessons/e2e-lesson4.png)

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Egy Azure függvény app és az Azure Storage-fiók létrehozása
Használja az Azure Resource Manager sablon toocreate Azure függvény alkalmazást és egy Azure Storage-fiókot.

*Becsült idő toocomplete: 10 perc*

Nyissa meg túl[Azure függvény app és Azure Storage-fiók létrehozása](iot-hub-gateway-kit-c-lesson4-deploy-resource-manager-template.md)

### <a name="read-messages-persisted-in-azure-table-storage"></a>Azure Table storage-ban tárolt üzenetek olvasása
Átjáró felhő köszönőüzenetei, az oktatóprogram tooAzure a Table storage figyelése

*Becsült idő toocomplete: 5 perc*

Nyissa meg túl[Azure Table storage-ban tárolt üzenetek olvasása](iot-hub-gateway-kit-c-lesson4-read-table-storage.md).

## <a name="troubleshooting"></a>Hibaelhárítás
Ha bármilyen probléma során hello során tapasztalatokat, keressen megoldásokat hello [hibaelhárítás](iot-hub-gateway-kit-c-troubleshooting.md) cikk.

## <a name="explore-more"></a>További részletek
A Microsoft hello [Intel IoT-átjáró csomag fejlesztői zóna](http://software.intel.com/iot/microsoft-azure) további toolearn.
