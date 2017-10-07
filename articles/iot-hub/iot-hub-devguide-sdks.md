---
title: aaaUnderstand hello Azure IoT SDK |} Microsoft Docs
description: "Fejlesztői útmutató - információk és hivatkozások toohello különböző Azure IoT eszköz és a szolgáltatás SDK-k használható toobuild eszköz és háttér-alkalmazások."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: c5c9a497-bb03-4301-be2d-00edfb7d308f
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e319451ca97876666e1c011ee0e1a73d0a0f1ed1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-and-use-azure-iot-sdks"></a>Megismeréséhez és használatához Azure IoT SDK-k

Az IoT-központ való munkához SDK három kategóriába sorolhatók:

* **Eszközoldali SDK-k** lehetővé teszik az IoT-eszközök futó toobuild alkalmazások. Ezek az alkalmazások telemetriai tooyour IoT-központ küldeni, és opcionálisan az IoT hub-üzeneteket fogadjon.

* **Szolgáltatás SDK-k** meg toomanage engedélyezése az IoT hub, és opcionálisan tooyour az IoT-eszközök az üzeneteket küldeni.

* **Az Azure IoT peremhálózati** lehetővé teszi a toobuild átjárók tooenable eszközök hello támogatott protokollok, amelyek nem használnak, vagy ha tooprocess üzenetek hello oldal van szükség.

SDK-k vannak megadva toosupport több programozási nyelv.

## <a name="azure-iot-device-sdks"></a>Az Azure SDK-k IoT-eszközök

Microsoft Azure IoT eszközoldali SDK-k hello kódot tartalmaznak, amely elősegíti a épület eszközök és alkalmazások, amelyek tooand Azure IoT-központ szolgáltatás által kezelt.

hello következő Azure IoT eszközoldali SDK-k a Githubról elérhető toodownload:

* [Az Azure IoT-eszközök SDK c] [ lnk-c-device-sdk] írt adatok hordozhatóságára és széles körű platformkompatibilitásra ANSI C (C99). Nincsenek C, két eszköz ügyféloldali kódtáraknál hello alacsony szintű **iothub_client** és hello **szerializáló**.
* [Az Azure IoT-eszközök SDK a .NET-hez][lnk-dotnet-device-sdk]
* [Az Azure IoT-eszközök SDK Java][lnk-java-device-sdk]
* [Az Azure IoT-eszközök SDK for Node.js][lnk-node-device-sdk]
* [Az Azure IoT-eszközök SDK Python-hez][lnk-python-device-sdk]

> [!NOTE]
> Lásd: hello információs fájl hello GitHub-adattárak az információt a nyelvi és a platform-specifikus csomag kezelők tooinstall bináris fájljait és a függőségek a fejlesztési számítógépén.
> 
> 

### <a name="os-platform-and-hardware-compatibility"></a>Az operációs rendszer platform és hardver kompatibilitása

Bizonyos hardvereszközök kompatibilisek SDK kapcsolatos további információkért lásd: hello [Azure IoT eszköz katalógus hitelesített][lnk-certified].

## <a name="azure-iot-service-sdks"></a>Az Azure IoT szolgáltatás SDK-k

hello Azure IoT szolgáltatás SDK-k alkalmazások közvetlenül kommunikáló IoT Hub toomanage eszközöket és a biztonsági kódot toofacilitate tartalmaz.

hello következő Azure IoT szolgáltatás SDK-k a Githubról elérhető toodownload:

* [Az Azure IoT szolgáltatás SDK a .NET-hez][lnk-dotnet-service-sdk]
* [Az Azure IoT szolgáltatás SDK for Node.js][lnk-node-service-sdk]
* [Azure IoT-szolgáltatás SDK Java][lnk-java-service-sdk]
* [Azure IoT-szolgáltatás SDK Python][lnk-python-service-sdk]
* [Az Azure IoT szolgáltatás SDK C-hez][lnk-c-service-sdk]

> [!NOTE]
> Lásd: hello információs fájl hello GitHub-adattárak az információt a nyelvi és a platform-specifikus csomag kezelők tooinstall bináris fájljait és a függőségek a fejlesztési számítógépén.

## <a name="azure-iot-edge"></a>Azure IoT Edge

Az Azure IoT peremhálózati hello infrastruktúra és a modulok toocreate IoT átjáró megoldásait tartalmazza. Az IoT-Edge toocreate átjárók szabott tooany végpont forgatókönyvet kibővítheti a.

Letöltheti a [Azure IoT peremhálózati] [ lnk-iot-edge] a Githubról.

## <a name="online-api-reference-documentation"></a>Online API referenciadokumentációt tartalmaz

hello alábbi lista hivatkozások tooonline API referenciadokumentációt tartalmaz Azure IoT eszköz, a szolgáltatás és az átjáró szalagtárak.

* [Az eszközök internetes hálózata (IoT) .NET][lnk-dotnet-ref]
* [Az IoT Hub REST][lnk-rest-ref]
* [Az Azure IoT-eszközök SDK C-hez][lnk-c-ref]
* [Az Azure IoT-eszközök SDK Java][lnk-java-ref]
* [Azure IoT-szolgáltatás SDK Java][lnk-java-service-ref]
* [Az Azure IoT-eszközök SDK for Node.js][lnk-node-ref]
* [Az Azure IoT szolgáltatás SDK for Node.js][lnk-node-service-ref]
* [Az Azure IoT él][lnk-gateway-ref]

## <a name="next-steps"></a>Következő lépések

Az IoT Hub fejlesztői útmutató egyéb témaköröket tartalmazza:

* [IoT-központok végpontjai][lnk-devguide-endpoints]
* [Az IoT-központ lekérdezési nyelv eszköz twins, a feladatok és az üzenet-útválasztás][lnk-devguide-query]
* [Kvóták és sávszélesség-szabályozás][lnk-devguide-quotas]
* [Az IoT Hub MQTT támogatása][lnk-devguide-mqtt]

<!-- Links and images -->

[lnk-c-device-sdk]: https://github.com/Azure/azure-iot-sdk-c
[lnk-c-service-sdk]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_service_client
[lnk-dotnet-device-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device
[lnk-java-device-sdk]: https://github.com/Azure/azure-iot-sdk-java/tree/master/device
[lnk-dotnet-service-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/service
[lnk-java-service-sdk]: https://github.com/Azure/azure-iot-sdk-java/tree/master/service
[lnk-node-device-sdk]: https://github.com/Azure/azure-iot-sdk-node/tree/master/device
[lnk-node-service-sdk]: https://github.com/Azure/azure-iot-sdk-node/tree/master/service
[lnk-python-device-sdk]: https://github.com/Azure/azure-iot-sdk-python/tree/master/device
[lnk-python-service-sdk]: https://github.com/Azure/azure-iot-sdk-python/tree/master/service
[lnk-certified]: https://catalog.azureiotsuite.com/
[lnk-iot-edge]: https://github.com/Azure/iot-edge

[lnk-dotnet-ref]: https://docs.microsoft.com/dotnet/api/microsoft.azure.devices
[lnk-c-ref]: https://azure.github.io/azure-iot-sdk-c/index.html
[lnk-java-ref]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device
[lnk-node-ref]: https://azure.github.io/azure-iot-sdk-node/
[lnk-rest-ref]: https://docs.microsoft.com/rest/api/iothub/
[lnk-java-service-ref]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service.auth
[lnk-node-service-ref]: https://azure.github.io/azure-iot-sdk-node/
[lnk-gateway-ref]: http://azure.github.io/iot-edge/api_reference/c/html/

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
