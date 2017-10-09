---
title: "aaaSimulated málna Pi toocloud (Node.js) - csatlakozás málna Pi webes szimulátor tooAzure IoT-központ |} Microsoft Docs"
description: "Csatlakozás málna Pi webes szimulátor tooAzure IoT-központ málna Pi toosend adatok toohello Azure felhőben."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Raspberry pi szimulátor, az azure iot raspberry pi raspberry pi iot-központot, raspberry pi küldése adatok toocloud raspberry pi toocloud"
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/28/2017
ms.author: xshi
ms.openlocfilehash: 83736caf6ce723a49001058495a780f7f51946a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-raspberry-pi-online-simulator-tooazure-iot-hub-nodejs"></a>Csatlakozás málna Pi online szimulátor tooAzure IoT Hub (Node.js)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Ebben az oktatóanyagban akkor először hello használatának alapjait málna Pi online szimulátor tanulási. Majd megismerheti, hogyan tooseamlessly hello Pi szimulátor toohello felhő használatával kapcsolódnak [Azure IoT Hub](iot-hub-what-is-iot-hub.md). 

Ha fizikai eszközöket, látogasson [csatlakozás málna Pi tooAzure IoT-központ](iot-hub-raspberry-pi-kit-node-get-started.md) tooget elindult. 

<p>
<div id="diag" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#getstarted" target="_blank">
<img src="media/iot-hub-raspberry-pi-web-simulator/3_banner.png" alt="Connect Raspberry Pi web simulator tooAzure IoT Hub" width="400">
</div>
<p>
<div id="button" style="width:100%; text-align:center">
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#Getstarted" target="_blank">
<img src="media/iot-hub-raspberry-pi-web-simulator/6_button_default.png" alt="Start Raspberry Pi simulator" width="400" onmouseover="this.src='media/iot-hub-raspberry-pi-web-simulator/5_button_click.png';" onmouseout="this.src='media/iot-hub-raspberry-pi-web-simulator/6_button_default.png';">
</div>

## <a name="what-you-do"></a>Mit

* Hello elsajátítását málna Pi online szimulátor.
* Létrehoz egy IoT-központot.
* Eszköz regisztrálása az IoT hub a pi.
* Futtassa a mintaalkalmazást Pi szimulált toosend érzékelő adatokat tooyour IoT-központ.

Csatlakoztassa a szimulált málna Pi tooan IoT-központ az Ön által létrehozott. Majd futtassa a mintaalkalmazást hello szimulátor toogenerate érzékelő adatokkal. Végül el kell küldenie hello érzékelő adatokat tooyour IoT-központot.

## <a name="what-you-learn"></a>Ismertetett témák

* Hogyan toocreate az Azure IoT-központ és az új eszköz kapcsolati karakterláncot. Ha az Azure-fiók nem rendelkezik [hozzon létre egy Azure próbafiókot](https://azure.microsoft.com/free/) csak néhány perc múlva.
* Hogyan toowork rendelkező málna Pi online szimulátor.
* Hogyan toosend érzékelő adatokat tooyour IoT-központot.

## <a name="overview-of-raspberry-pi-web-simulator"></a>Málna Pi webes szimulátor áttekintése

Kattintson a hello gomb toolaunch málna Pi online szimulátor.

> [!div class="button"]
<a href="https://azure-samples.github.io/raspberry-pi-web-simulator/#GetStarted" target="_blank">Indítsa el a Raspberry Pi szimulátor</a>

Nincsenek hello webes szimulátor háromféleképpen használhatók.
1. Összeállítási terület - hello alapértelmezett kapcsolatcsoport, a Pi kapcsolódik BME280 érzékelő és LED. hello terület zárolva van az előzetes verzióra jelenleg így testreszabási nem hajtható végre.
2. Kódolás a terület - toocode málna Pi az online kód szerkesztő. hello alapértelmezett mintaalkalmazás BME280 érzékelő toocollect érzékelőadatait segítségével, és elküldi a tooyour Azure IoT Hub. hello alkalmazásra az teljes mértékben kompatibilis a valódi Pi eszközök. 
3. Integrált Windows - hello kimeneti kódot mutatja. Az ablak hello tetején nincsenek három gomb.
   * **Futtatás** -terület kódolási hello hello alkalmazást futtat.
   * **Alaphelyzetbe** -visszaállítási hello terület toohello alapértelmezett mintaalkalmazás kódolása.
   * **Modellrészek/kibontott** – a megfelelő toofold/bővíthető hello konzolablakban gomb nincs ügyféloldali hello.

> [!NOTE] 
hello málna Pi webes szimulátor jelenleg előzetes verzióban érhető el. Szeretnénk toohear a beszéd a hello [Gitter Chatroom](https://gitter.im/Microsoft/raspberry-pi-web-simulator). hello forráskód a nyilvános [Github](https://github.com/Azure-Samples/raspberry-pi-web-simulator).

![A Pi online szimulátor áttekintése](media/iot-hub-raspberry-pi-web-simulator/0_overview.png)

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]


## <a name="run-a-sample-application-on-pi-web-simulator"></a>A Pi webes szimulátor mintaalkalmazás futtatása

1. Kódolási terület, győződjön meg arról, hello alapértelmezett mintaalkalmazás dolgoznak. A sor 15 hello helyőrzőt cserélje le hello Azure IoT hub eszköz kapcsolati karakterláncot.
   ![Cserélje le a hello eszköz kapcsolati karakterlánc](media/iot-hub-raspberry-pi-web-simulator/1_connectionstring.png)

2. Kattintson a **futtatása** vagy típus `npm start` toorun hello alkalmazás.


Megjelenik a következő hello kimeneti, amely mutatja hello érzékelőadatait és hello tooyour IoT-központ küldött üzenetek ![kimeneti - érzékelő adatokat küldött az málna Pi tooyour IoT-központ](media/iot-hub-raspberry-pi-web-simulator/2_run_application.png)


## <a name="next-steps"></a>Következő lépések

Egy alkalmazás toocollect érzékelő mintaadatok már futtatta, és elküldi a tooyour IoT-központot.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
