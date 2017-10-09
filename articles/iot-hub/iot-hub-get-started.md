---
title: "Az Azure IoT Hub - első lépések csatlakoztatása az IoT-eszközök toohello felhő |} Microsoft Docs"
description: "Megtudhatja, hogyan tooconnect az IoT-modulok és a starter Kit tooAzure IoT-központot. Az eszközök küldhet telemetriai tooIoT központ és az IoT-központ figyelheti és az eszközök kezeléséhez."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
keywords: "az Azure iot hub oktatóanyag"
ms.assetid: 24376318-5344-4a81-a1e6-0003ed587d53
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: dobett
ms.openlocfilehash: 6dc956308009091532019ff84aec881f042f0104
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-hub-get-started-tutorials"></a>Az Azure IoT Hub get oktatóanyagok indítása

Azure IoT Hub és hello Azure IoT eszköz SDK-k toobuild az eszközök internetes hálózatát (IoT) megoldások is használhatja:

* Az Azure IoT-központot egy olyan teljes körűen felügyelt szolgáltatás hello felhőbe, amely biztonságosan csatlakozik, figyeli, és kezeli az IoT-eszközök. Hello Azure IoT eszközoldali SDK-k tooimplement az IoT-eszközök használata.
* Az IoT-átjáró összetettebb IoT-forgatókönyvek esetén használható. Például, ha szüksége tooconsider tényezőkkel, például az örökölt eszközök, a sávszélességgel kapcsolatos költségek, a biztonsági és adatvédelmi szabályzatok vagy a biztonsági adatok feldolgozása. Ezek a forgatókönyvek használhatja az Azure IoT peremhálózati tooimplement eszközök tooyour IoT-központ összekapcsoló átjáró.

## <a name="what-hello-tutorials-cover"></a>Hello oktatóanyagok vonatkozik

Ezek az oktatóanyagok tooAzure IoT Hub és hello eszköz SDK-k bevezetése. hello oktatóanyagok általános IoT forgatókönyvek toodemonstrate hello képességeket IoT hub foglalkozik. hello oktatóanyagokat is bemutatják, hogyan-toocombine más Azure IoT hubot szolgáltatásokat és eszközöket több toobuild hatékony IoT-megoldásokhoz. Hello oktatóanyagok válassza toouse szimulált vagy valós az IoT-eszközök. Emellett áttekintheti, hogyan toouse egy átjáró tooenable eszközök tooconnect tooyour IoT-központot.

## <a name="set-up-your-device"></a>Az eszköz beállítása

Csatlakozás egy IoT eszköz vagy az átjáró tooAzure IoT-központot. A szimulált vagy fizikai eszköz tooget lépések közül választhat:

| IoT-eszközök                       | Programozási nyelv |
|----------------------------------|----------------------|
| Raspberry Pi                     | [Python][Pi_Py], [Node.js][Pi_Nd], [C][Pi_C]  |
| Az IoT-DevKit                       | [A VSCode Arduino][DevKit]     |
| Intel Edison                     | [NODE.js][Ed_Nd], [C][Ed_C]    |
| Adafruit lágyított HUZZAH ESP8266  | [Arduino][Hu_Ard]              |
| Sparkfun ESP8266 dolog fejlesztői       | [Arduino][Th_Ard]              |
| Adafruit lágyított M0              | [Arduino][M0_Ard]              |
| Szimulált eszköz számítógépen           | [.NET][Sim_NET], [Java][Sim_Jav], [Node.js][Sim_Nd], [Python][Sim_Pyth] |
| Online eszköz szimulátor         | [Raspberry Pi (Node.js)][Ol_Sim] |

Továbbá az IoT-peremhálózati átjáró tooenable eszközök tooconnect tooyour IoT-központ is használhatja:

| Átjáróeszköz               | Programozási nyelv | Platform         |
|------------------------------|----------------------|------------------|
| Intel NUC (modell DE3815TYKE) | C#                    | [Szél folyó Linux][NUC_Lnx] |
| Szimulált átjáró            | C#                    | [Linux][Sim_Lnx], [Windows][Sim_Win] |

[!INCLUDE [iot-hub-get-started-extended](../../includes/iot-hub-get-started-extended.md)]

[Pi_Nd]: iot-hub-raspberry-pi-kit-node-get-started.md
[Pi_C]: iot-hub-raspberry-pi-kit-c-get-started.md
[Pi_Py]: iot-hub-raspberry-pi-kit-python-get-started.md
[DevKit]: iot-hub-arduino-iot-devkit-az3166-get-started.md
[Ed_Nd]: iot-hub-intel-edison-kit-node-get-started.md
[Ed_C]: iot-hub-intel-edison-kit-c-get-started.md
[Hu_Ard]: iot-hub-arduino-huzzah-esp8266-get-started.md
[Th_Ard]: iot-hub-sparkfun-esp8266-thing-dev-get-started.md
[M0_Ard]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[Sim_NET]: iot-hub-csharp-csharp-getstarted.md
[Sim_Jav]: iot-hub-java-java-getstarted.md
[Sim_Nd]: iot-hub-node-node-getstarted.md
[Sim_Pyth]: iot-hub-python-getstarted.md
[NUC_Lnx]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
[Sim_Lnx]: iot-hub-linux-iot-edge-get-started.md
[Sim_Win]: iot-hub-windows-iot-edge-get-started.md
[Ol_Sim]: iot-hub-raspberry-pi-web-simulator-get-started.md
