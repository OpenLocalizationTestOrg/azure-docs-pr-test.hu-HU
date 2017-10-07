---
title: "Szimulált eszköz & Azure IoT átjáró – első lépések |} Microsoft Docs"
description: "Az IoT-átjáró Starter Kit első lépései az Azure IoT hub létrehozása és connect Gateway toohello IoT-központ"
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "az Azure iot hub, iot-átjáró első lépések hello az eszközök internetes hálózata, iot eszközkészlet"
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
ms.openlocfilehash: 1a54a5e5f1c1d9b2e657c9e4448274256e2533f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-iot-gateway-starter-kit-with-a-simulated-device"></a>A szimulált eszköz Ismerkedés az IoT-átjáró Starter Kit

> [!div class="op_single_selector"]
> * [SensorTag](iot-hub-gateway-kit-c-get-started.md)
> * [Szimulált eszköz](iot-hub-gateway-kit-c-sim-get-started.md)

Ebben az oktatóanyagban kezdődik, majd hello használatának alapjait tanulási [IoT átjáró Starter Kit](https://aka.ms/gateway-kit). Dolgozni fog az Intel NUC szél folyó Linux operációs rendszert futtató. Megtudhatja, hogyan tooseamleesly kapcsolódnak az eszközök toohello felhőalapú Azure IoT Hub használatával.

***
**Még nem rendelkezik a csomag?:** kattintson [Itt](https://aka.ms/gateway-kit).
***

## <a name="lesson-1-configure-your-nuc"></a>1. lecke: A NUC konfigurálása
![Lesson1 végpont diagramja](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson1.png)

Ez a lecke hello Azure IoT átjáróként Kit Intel NUC (következő egység a Informatika) beállítása, NUC hello Azure IoT biztonsági csomag telepítését, és futtatni egy minta app tooverify hello átjáró funkciót.

*Becsült idő toocomplete: 15 perc*

Nyissa meg túl[Intel NUC IoT átjáróként beállítása](iot-hub-gateway-kit-c-sim-lesson1-set-up-nuc.md)

## <a name="lesson-2-create-your-iot-hub"></a>2. lecke: Az IoT hub létrehozása
![Lesson2 végpont diagramja](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson2.png)

Ez a lecke hello eszközök és szoftverek telepítéséhez a gazdaszámítógépen. Ezután a ingyenes Azure-fiók létrehozásához, kiépíteni az Azure IoT hub és az első eszköz hello IoT hub létrehozása.

Teljes lecke 1 Ez a lecke megkezdése előtt.

### <a name="get-hello-tools"></a>Hello eszközök beszerzése
A gazdaszámítógép hello eszközök és szoftverek telepítéséhez.

*Becsült idő toocomplete: 20 perc*

Nyissa meg túl[hello eszközök beszerzése](iot-hub-gateway-kit-c-sim-lesson2-get-the-tools-win32.md)

### <a name="create-an-iot-hub-and-register-your-device"></a>Létrehoz egy IoT-központot, és regisztrálja az eszközt
Az erőforráscsoport létrehozása, az első Azure IoT hub kiépíteni, és adja hozzá az első eszköz toohello IoT hub hello Azure parancssori felület használatával.

*Becsült idő toocomplete: 10 perc*

Nyissa meg túl[létrehoz egy IoT-központot, és regisztrálja az eszközt](iot-hub-gateway-kit-c-sim-lesson2-register-device.md)

## <a name="lesson-3-receive-messages-from-hello-simulated-device-and-read-messages-from-your-iot-hub"></a>3. lecke: Hello szimulált eszköz érkező üzenetek fogadására, és üzeneteket beolvasni az IoT hub
Ez a lecke szüksége lesz egy szimulált eszköz alkalmazásának végrehajtásának és parancsfájlok tooautomate hello konfigurációs az átjárót. hello szimulált eszköz alkalmazásának minta hőmérséklet adatokat állít elő, és elküldi azt tooan IoT hub modul. hello IoT hub modul csomagok hello adatokat fogad, és elküldi azt az Azure IoT Edge tooyour IoT-központ hello átjáró keretrendszere keresztül.

![Diagram. a végpont 3. rész](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson3.png)

### <a name="configure-and-run-a-simulated-device"></a>Konfigurálja és futtassa a szimulált eszköz
Készítse elő a hello minta kódokat. Ezután konfigurálja, és hello szimulált eszköz mintaalkalmazás futtatása.

*Becsült idő toocomplete: 15 perc*

Nyissa meg túl[beállítása és futtatása a szimulált eszköz](iot-hub-gateway-kit-c-sim-lesson3-configure-simulated-device-app.md)

### <a name="read-messages-from-your-iot-hub"></a>Az IoT hub olvasható üzenetek
Futtassa a mintakód a fogadó számítógép tooread köszönőüzenetei az IoT hub.

*Becsült idő toocomplete: 15 perc*

Nyissa meg túl[üzenetek olvasni az IoT hub](iot-hub-gateway-kit-c-sim-lesson3-read-messages-from-hub.md)

## <a name="lesson-4-save-messages-tooazure-table-storage"></a>4. lecke: Üzenetek tooAzure a Table storage mentése
Hozzon létre, hogy a bejövő üzenetek lekérése az IoT hub, majd beírja őket tooAzure a Table storage Azure függvény alkalmazások.

![Rész 4 végpont diagramja](media/iot-hub-gateway-kit-lessons/e2e-sim-Lesson4.png)

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Egy Azure függvény app és az Azure Storage-fiók létrehozása
Használja az Azure Resource Manager sablon toocreate Azure függvény alkalmazást és egy Azure Storage-fiókot.

*Becsült idő toocomplete: 10 perc*

Nyissa meg túl[Azure függvény app és Azure Storage-fiók létrehozása](iot-hub-gateway-kit-c-sim-lesson4-deploy-resource-manager-template.md)

### <a name="read-messages-persisted-in-azure-table-storage"></a>Azure Table storage-ban tárolt üzenetek olvasása
Átjáró felhő köszönőüzenetei, az oktatóprogram tooAzure a Table storage figyelése

*Becsült idő toocomplete: 5 perc*

Nyissa meg túl[Azure Table storage-ban tárolt üzenetek olvasása](iot-hub-gateway-kit-c-sim-lesson4-read-table-storage.md).

## <a name="troubleshooting"></a>Hibaelhárítás
Ha bármilyen probléma során hello során tapasztalatokat, keressen megoldásokat hello [hibaelhárítás](iot-hub-gateway-kit-c-sim-troubleshooting.md) cikk.

## <a name="explore-more"></a>További részletek
A Microsoft hello [Intel IoT-átjáró csomag fejlesztői zóna](https://software.intel.com/en-us/iot/hardware/gateways/dev-kit) további toolearn.
