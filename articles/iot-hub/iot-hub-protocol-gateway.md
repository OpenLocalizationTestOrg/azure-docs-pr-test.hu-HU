---
title: "aaaAzure IoT protokoll-átjáró |} Microsoft Docs"
description: "Hogyan toouse az Azure IoT protokoll tooextend IoT-központ szolgáltatásait és protokoll támogatása tooenable eszközök tooconnect tooyour hub nem támogatott natív módon az IoT-központ által protokollok használatával."
services: iot-hub
documentationcenter: 
author: kdotchkoff
manager: timlt
editor: 
ms.assetid: 555e59ae-3136-4533-8ba8-f3a3b6acf648
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: kdotchko
ms.openlocfilehash: 9cfed30149672d8f7e021a9899192105bbcdd400
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# Az IoT-központ további protokollok támogatása
Az Azure IoT Hub natív módon támogatja a kommunikációt hello MQTT, az amqp-t és a HTTP protokollon keresztül. Bizonyos esetekben eszközök vagy mező átjárók előfordulhat, hogy nem tud toouse ezek szabványos protokollok és protokoll kiigazítása szükséges. Ilyen esetekben egy egyéni gateway használható. Egy egyéni átjáró adatközponthíd-képzés hello forgalom tooand az IoT Hub protokoll kiigazítása az IoT-központok végpontjai engedélyezheti. Használhatja a hello [Azure IoT protokoll-átjáró](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md) , egy egyéni átjáró tooenable protokoll kiigazítása az IoT-központot.

## Az Azure IoT protokoll-átjáró
hello Azure IoT protokoll-átjáró keretrendszere, amely készült, nagy méretű, kétirányú eszköz kommunikáció IoT Hub protokoll kiigazítása. hello protokoll-átjáró része a csatlakoztatott eszköz kapcsolatok fogadására egy adott protokollon keresztül. Az kulcsösszetevő hello forgalom tooIoT Hub AMQP 1.0-hez képest. hello Azure IoT protokoll-átjáró áll rendelkezésre egy nyílt forráskódú szoftvereket projekt tooprovide rugalmasságot biztosít a különböző protokollok és protokollverziója támogatását.

Telepítheti az Azure-ban hello protokoll-átjáró skálázható módon Azure Service Fabric, a Azure Cloud Services feldolgozói szerepkörök vagy a Windows virtuális gépek használatával. Ezenkívül hello protokoll-átjáró telepíthető helyszíni környezetekben, például a mező átjárók.

hello Azure IoT protokoll-átjáró tartalmaz egy MQTT protokoll adapter, amely lehetővé teszi, hogy toocustomize hello MQTT protokoll viselkedését, ha szükséges. IoT Hub beépített támogatást nyújt az hello MQTT v3.1.1 protokollt, mert csak érdemes hello MQTT protokoll adaptert használ, ha a protokoll testre szabott elem és további funkciók konkrét követelmények szükségesek.

hello MQTT adapter is hello programozási modellt protokoll adapterek más protokollokat mutatja be. Ezenkívül hello Azure IoT protokoll átjáró programozási modell lehetővé teszi az egyéni összetevőkben található tooplug kifejezetten feldolgozásra, például az egyéni hitelesítési, az üzenet átalakítások, a tömörítési/kibontása vagy a titkosítási/visszafejtési forgalom hello eszközök és az IoT-központ.

Rugalmasság hello protokoll-átjáró és MQTT megvalósítási megadott nyílt forráskódú szoftvereket projektben. Ez lehetővé teszi toocustomize hello megvalósítási igény szerint.

## Következő lépések
hello Azure IoT protokoll-átjáró kapcsolatos további toolearn és hogyan toouse és az IoT-megoldásból részeként, a rendszer lásd:

* [Az Azure IoT protokoll átjáró tárház a Githubon](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md)
* [Az Azure IoT protokoll átjáró fejlesztői útmutató](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/docs/DeveloperGuide.md)

További információ az IoT-központ telepítésének tervezése toolearn lásd:

* [Az Event Hubs összehasonlítása][lnk-compare]
* [Méretezés, a magas rendelkezésre ÁLLÁSÚ és vész-Helyreállítási][lnk-scaling]
* [IoT Hub fejlesztői útmutató][lnk-devguide]

[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
