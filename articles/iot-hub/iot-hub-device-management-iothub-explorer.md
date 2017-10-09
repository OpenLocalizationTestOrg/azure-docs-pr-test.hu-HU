---
title: "az IoT-eszközök kezelése a IOT hubbal-explorer aaaAzure |} Microsoft Docs"
description: "Eszközzel hello IOT hubbal-explorer CLI Azure IoT Hub kezeléséhez, ha kiemeli hello közvetlen módszerek és hello iker kívánt tulajdonságokkal kezelési lehetőségek."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az Azure iot kezelés, az azure iot hub – eszközkezelés, eszköz felügyeleti iot, iot hub – Eszközkezelés"
ms.assetid: b34f799a-fc14-41b9-bf45-54751163fffe
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/12/2017
ms.author: xshi
ms.openlocfilehash: e0a5e6120db5c4fb12f7f8b605a56e0e4aad9217
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-iothub-explorer-for-azure-iot-hub-device-management"></a>Használja az IOT hubbal-explorer Azure IoT Hub – Eszközkezelés

![Végpontok közötti diagramja](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[IOT hubbal-explorer](https://github.com/azure/iothub-explorer) parancssori eszköz, amely futtatja a fogadó számítógép toomanage eszköz azonosítói az IoT hub beállításjegyzék. Az ismét tooperform használt beállítások a különböző feladatok.

| Lehetőséget          | Tevékenység                                                                                                                            |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Közvetlen metódusok             | Egy eszköz, például indítása és leállítása üzenetküldésre vagy hello eszköz újraindítása működésre tétele.                                        |
| A két szükségeskonfiguráció-tulajdonságok    | Egy eszköz bocsátott egyes állapotok, például egy LED toogreen beállítást, vagy beállítás hello telemetriai adatok küldése időköz too30 perc.         |
| A két jelentett tulajdonságai   | Első hello jelentett eszközök állapotát. Például a hello eszköz jelentés hello LED most villogó-e.                                    |
| A két címkék                  | Hello felhőben tárolt az eszközre vonatkozó metaadatok. Például hello egy Eladóautomata telepítési helyét.                         |
| Felhő-eszközre küldött üzenetek   | Értesítések tooa eszköz küldése. Például "akkor nagyon valószínű toorain ma. Ne feledje toobring egy összevonó."              |
| Eszköz iker lekérdezések        | A lekérdezés az összes eszköz twins tooretrieve rendelkező tetszőleges feltételek, például használható hello eszközök azonosításához. |

Részletesebb hello különbségek a Magyarázat és útmutatást ezen beállítások használatával, lásd: [eszközről a felhőbe kommunikációs útmutatást](iot-hub-devguide-d2c-guidance.md) és [felhő eszközre kommunikációs útmutatást](iot-hub-devguide-c2d-guidance.md).

> [!NOTE]
> Az ikereszközök JSON-dokumentumok, amelyek az eszközök állapotinformációit (metaadatokat, konfigurációkat és állapotokat) tárolják. Az IoT-központ továbbra is fennáll, egy eszköz iker tooit csatlakozó eszközökről. Eszköz twins kapcsolatos további információkért lásd: [Ismerkedés az eszköz twins](iot-hub-node-node-twin-getstarted.md).

## <a name="what-you-learn"></a>Ismertetett témák

Megismerheti a fejlesztői gépen különböző kezelési lehetőségek az IOT hubbal-Intéző segítségével.

## <a name="what-you-do"></a>Mit

IOT hubbal-kezelő futtatása a különböző felügyeleti beállításokkal.

## <a name="what-you-need"></a>Mi szükséges

- Az oktatóanyag [beállítani az eszközét](iot-hub-raspberry-pi-kit-node-get-started.md) fejeződött be, amely hozzá van rendelve hello követelményeknek:
  - Aktív Azure-előfizetés.
  - Az előfizetéshez tartozó Azure IoT hub.
  - Egy ügyfélalkalmazás által küldött üzenetek tooyour Azure IoT-központot.
- Győződjön meg arról, hogy az eszköz hello ügyfélalkalmazás Ez az oktatóanyag során fut-e.
- IOT hubbal-Explorerben [telepítse az IOT hubbal-explorer](https://github.com/azure/iothub-explorer) a fejlesztési számítógépén.

## <a name="connect-tooyour-iot-hub"></a>Csatlakozás tooyour IoT-központ

Csatlakozás az IoT-központ tooyour hello a következő parancs futtatásával:

```bash
iothub-explorer login <your IoT hub connection string>
```

## <a name="use-iothub-explorer-with-direct-methods"></a>Használja az IOT hubbal-explorer közvetlen módszer

Hello meghívása `start` metódus a hello eszköz alkalmazás toosend üzenetek tooyour IoT-központ hello a következő parancs futtatásával:

```bash
iothub-explorer device-method <your device Id> start
```

Hello meghívása `stop` hello eszköz alkalmazás toostop küldésekor metódus tooyour IoT-központ üzenetek hello a következő parancs futtatásával:

```bash
iothub-explorer device-method <your device Id> stop
```

## <a name="use-iothub-explorer-with-twins-desired-properties"></a>Használja az IOT hubbal-explorer iker a kívánt tulajdonságokkal

Állítsa be a kívánt tulajdonság intervallumát 3000 = hello a következő parancs futtatásával:

```bash
iothub-explorer update-twin <your device id> {\"properties\":{\"desired\":{\"interval\":3000}}}
```

Ez a tulajdonság az eszköz által is olvasható.

## <a name="use-iothub-explorer-with-twins-reported-properties"></a>Használja az IOT hubbal-explorer iker által jelentett tulajdonságokkal

Első hello hello eszköz futtatásával hello jelentett tulajdonságait a következő parancsot:

```bash
iothub-explorer get-twin <your device id>
```

Hello tulajdonságok egyike $metadata. melyik látható hello utolsó idő az eszköz $lastUpdated küld vagy üzenetet kap.

## <a name="use-iothub-explorer-with-twins-tags"></a>-Kezelővel IOT hubbal-iker tartozó címkékkel

Megjeleníti a hello címkék és hello eszköz tulajdonságait hello a következő parancs futtatásával:

```bash
iothub-explorer get-twin <your device id>
```

A mező szerepkör hozzáadása = a hőmérséklet és a páratartalom toohello eszköz hello a következő parancs futtatásával:

```bash
iothub-explorer update-twin <your device id> "{\"tags\":{\"role\":\"temperature&humidity\"}}"

```

## <a name="use-iothub-explorer-with-cloud-to-device-messages"></a>IOT hubbal-explorer használata a felhő-eszközre küldött üzenetek

A "Hello World" üzenet toohello eszköz küldése hello a következő parancs futtatásával:

```bash
iothub-explorer send <device-id> "Hello World"
```

Lásd: [IOT hubbal-explorer toosend használja, és az eszköz és az IoT-központ között üzeneteket fogadni](iot-hub-explorer-cloud-device-messaging.md) valós forgatókönyv esetén ez a parancs használatával.

## <a name="use-iothub-explorer-with-device-twins-queries"></a>Használja az IOT hubbal-explorer eszköz twins lekérdezések

Eszközök a szerepkör címkével ellátott lekérdezése = "a hőmérséklet és a páratartalom" hello, a következő parancs futtatásával:

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role = 'temperature&humidity'"
```

Szerepkör címkével ellátott kivételével minden eszköz lekérdezése = "a hőmérséklet és a páratartalom" hello, a következő parancs futtatásával:

```bash
iothub-explorer query-twin "SELECT * FROM devices WHERE tags.role != 'temperature&humidity'"
```

## <a name="next-steps"></a>Következő lépések

Hogy megismerte hogyan toouse IOT hubbal-explorer, a különböző felügyeleti beállításokkal.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
