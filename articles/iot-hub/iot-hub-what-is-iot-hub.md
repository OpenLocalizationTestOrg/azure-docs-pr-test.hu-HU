---
title: "az IoT-központ áttekintése aaaAzure |} Microsoft Docs"
description: "Hello Azure IoT-központ szolgáltatás – áttekintés: Mi az az IoT-központot, eszközkapcsolatok, internet dolgot szokások, átjárók és hello szolgáltatás által támogatott kommunikációs minta"
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 24376318-5344-4a81-a1e6-0003ed587d53
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d0b46868a1ec9e13c8f107b90269c5307f4ba27c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-hello-azure-iot-hub-service"></a>Hello Azure IoT-központ szolgáltatás áttekintése

Üdvözli az IoT-központ tooAzure. Ez a cikk áttekintése Azure IoT Hub, és ismerteti, miért kell használni a szolgáltatás tooimplement egy az eszközök internetes hálózatát (IoT) megoldás. Az Azure IoT Hub egy teljesen felügyelt szolgáltatás, amely megbízható és biztonságos kétirányú kommunikációt tesz lehetővé több millió IoT-eszköz között, valamint megoldást biztosít a háttérrendszer kialakításához. Azure IoT Hub:

* Több üzenetkezelési lehetőséget kínál az eszközök és a felhő között mindkét irányban, beleértve az egyirányú üzenetküldést, a fájlátvitelt és a kérés-válasz módszereket.
* Beépített deklaratív üzenet útválasztási tooother Azure-szolgáltatásokat biztosít.
* Egy lekérdezhető tárolót biztosít az eszközök metaadataihoz és szinkronizáltsági állapotával kapcsolatos információkhoz.
* Az eszközönkénti biztonsági hitelesítő adatok vagy X.509 tanúsítványok segítségével lehetővé teszi a biztonságos kommunikációt és hozzáférés-vezérlést.
* Az eszközkapcsolat és az eszközidentitás-kezelési események széles körű megfigyelését nyújtja.
* Eszköz szalagtárak hello legnépszerűbb nyelvekhez és platformokhoz tartalmazza.

hello cikk [összehasonlítása az IoT-központ és az Event Hubs] [ lnk-compare] hello a két szolgáltatás közötti fontosabb különbségeket ismerteti, és kiemeli a hello előnye az IoT-központ az IoT-megoldásokhoz.

Hogyan Azure és az IoT-központ biztonságossá tétele az IoT-megoldásból további információkért lásd: [az eszközök internetes hálózatát biztonsági szabad hello a][lnk-security-ground-up].

![Azure IoT Hub IoT-megoldások felhőátjárójaként][img-architecture]

> [!NOTE]
> Egy IoT-architektúra részletes ismertetéséhez lásd: hello [Microsoft Azure IoT-Referenciaarchitektúra][lnk-refarch].

## <a name="iot-device-connectivity-challenges"></a>Az IoT-eszközkapcsolatok kihívásai

Az IoT-központ és hello eszköz szalagtárak toomeet hello kihívás, hogy hogyan segítenek tooreliably és biztonságosan csatlakozzon az eszközök toohello megoldás háttérrendszerének. IoT-eszközök:

* Általában beágyazott, emberi beavatkozást nem igénylő rendszerek.
* Távoli helyeken is lehetnek, ahol a fizikai hozzáférés drága lenne.
* Csak akkor hello megoldás háttérrendszerének keresztül érhető el.
* Áramellátásuk és feldolgozási erőforrásaik korlátozottak lehetnek.
* A hálózati kapcsolat időszakos, lassú vagy drága lehet.
* Esetleg toouse saját fejlesztésű, egyedi vagy iparág-specifikus alkalmazás-protokollokra.
* Számos népszerű hardver- és szoftverplatform használatával létrehozhatók.

Ezenkívül toohello a fenti követelmények minden IoT-megoldás kell is biztosítanak méretezési, biztonságot és megbízhatóságot. hello eredő hálózati kapcsolati követelményeinek nehéz és időigényes tooimplement használata esetén hagyományos technológiák, például webes tárolók és üzenetkezelő közvetítők.

## <a name="why-use-azure-iot-hub"></a>Miért érdemes az Azure IoT Hubot használni?

Ezenkívül tooa gazdag készlete [eszközről a felhőbe] [ lnk-d2c-guidance] és [felhő eszközre] [ lnk-c2d-guidance] kommunikációs beállításai, többek között az üzenetküldés, fájl átvitel és a kérelem-válasz, Azure IoT Hub címek hello eszközkapcsolatok kihívás hello a következő módon:

* **Ikereszközök**. Az [ikereszközök][lnk-twins] használatával tárolhatja, szinkronizálhatja és lekérdezheti az eszközök metaadatait és állapotinformációit. Az ikereszközök JSON-dokumentumok, amelyek az eszközök állapotinformációit (metaadatokat, konfigurációkat és állapotokat) tárolják. Az IoT-központ továbbra is fennáll, az egyes eszközök, hogy tooIoT központi csatlakozás egy eszköz iker.

* **Eszközönkénti hitelesítés és biztonságos kapcsolat**. Minden saját eszköz létesíthet [biztonsági kulcs] [ lnk-devguide-security] tooenable azt tooconnect tooIoT központ. Hello [IoT-központ identitásjegyzékhez] [ lnk-devguide-identityregistry] eszköz identitások és kulcsok tárolja, a megoldás. A megoldás háttérrendszeréhez hozzáadhat egyéni eszközök tooallow és listák tooenable teljes felügyeletet gyakorolhat az eszközök hozzáférésének megtagadása.

* **Útvonal eszközről-a-felhőbe üzenetek deklaratív szabályok alapján tooAzure szolgáltatások**. Az IoT-központ lehetővé teszi, hogy Ön toodefine útvonalak alapuló üzenetek útválasztási szabályok toocontrol, ahol a központ eszközről a felhőbe üzeneteket küld. Útválasztási szabályokat nem szükséges, akkor toowrite összes kódot, vagy az egyéni utáni adatfeldolgozást üzenet elosztás hello sor kerülhet.

* **Eszközkapcsolatok műveleteinek megfigyelése**. Részletes műveleti naplókat fogadhat az eszközidentitás-kezelési műveletekről és az eszközök kapcsolati eseményeiről. A figyelési funkció lehetővé teszi, hogy az IoT megoldás tooidentify problémák, például az eszközökön, amelyek tooconnect nem megfelelő hitelesítő adatokkal próbálja küldéséhez túl gyakran, vagy az összes felhő eszközre üzenetek elutasítása.

* **Széles körű eszközkönyvtár-készlet**. Az [Azure IoT eszközoldali SDK-k][lnk-device-sdks] többféle nyelven és platformon elérhetők és támogatottak, például C nyelven sok Linux-disztribúcióhoz, Windowshoz és valós idejű operációs rendszerekhez. Az Azure IoT eszközoldali SDK-k a felügyelt nyelveket is támogatják, például a C#, Java és JavaScript nyelveket.

* **IoT-protokollok és bővíthetőség**. A megoldás hello eszköz könyvtárak nem használható, ha az IoT-központ közzététele, amely lehetővé teszi az eszközök toonatively használata hello MQTT v3.1.1, a HTTP 1.1 vagy az AMQP 1.0 protokoll nyilvános protokoll. Is kiterjeszthető az IoT-központ toosupport egyéni protokollok szerint:

  * A mező átjáró létrehozása [Azure IoT peremhálózati] [ lnk-iot-edge] , amely az egyéni protokoll tooone az IoT-központ három protokollok megértettem hello alakítja át.
  * Testreszabás hello [Azure IoT protokoll-átjáró][protocol-gateway], egy nyílt forráskódú összetevő hello felhőben futó.

* **Méretezés**. Az Azure IoT Hub arányosan toomillions egyidejűleg csatlakoztatott eszközt, és több millió esemény / másodperc.

## <a name="gateways"></a>Átjárók

Az IoT-megoldás az átjáró az általában vagy egy [protokoll-átjáró] [ lnk-iotedge] hello felhőben telepített vagy egy [mező átjáró] [ lnk-field-gateway] , amely helyben telepített, az eszközöket. A protokoll-átjáró protokollfordításhoz, például MQTT tooAMQP hajt végre. Mező átjáró is analytics futtatnak hello él, döntéseket időérzékeny tooreduce késés, adja meg az eszközkezelési szolgáltatások, biztonsági és adatvédelmi korlátozások és protokollfordításhoz is végrehajtható. Mindkét átjárótípus közvetítőként működik az eszközök és az IoT Hub között.

A helyszíni átjárók különböznek az egyszerű forgalomirányító eszközöktől (például a hálózati címfordítási eszköztől vagy tűzfaltól), mert általában aktív szerepet játszanak a hozzáférés kezelésében és az információáramlásban a megoldáson belül.

Egy megoldás mindkét protokollt és helyszíni átjárót tartalmazhatja.

## <a name="how-does-iot-hub-work"></a>Hogyan működik az IoT Hub?

Az Azure IoT Hub megvalósítja hello [szolgáltatás által támogatott kommunikációs] [ lnk-service-assisted-pattern] mintát toomediate hello kapcsolati az eszközök és a megoldás közötti háttér. hello szolgáltatás által támogatott kommunikációs célja tooestablish megbízható, kétirányú kommunikációs útvonala egy vezérlő rendszerén, például az IoT-központ, és nem megbízható fizikai lemezterület üzembe helyezett speciális eszközök között. hello mintát hoz létre a következő alapelveket hello:

* A biztonság minden más képességgel szemben elsőbbséget élvez.

* Az eszközök nem fogadnak el kéretlen hálózati információkat. Az eszközök csak kifelé irányuló kapcsolatokat és útvonalakat létesíthetnek. Az eszköz tooreceive hello megoldás háttérből parancs hello eszköz rendszeresen egy kapcsolat toocheck az összes függőben lévő parancsok tooprocess kell kezdeményeznie.

* Eszközök csak csatlakoznia tooor útvonalak toowell ismert szolgáltatások azok vannak társviszonyban, például az IoT Hub létrehozása.

* hello alkalmazásréteg-protokoll a védett hello kommunikációs útvonalat eszköz és a szolgáltatás vagy eszköz és az átjáró között.

* A rendszerszintű engedélyezés és hitelesítés eszközönkénti identitásokon alapul. Ezek a hozzáférési hitelesítő adatokat és engedélyeket szinte azonnal visszavonhatóvá teszik.

* Kétirányú kommunikációt időnként esedékes toopower vagy kapcsolódási problémákat csatlakozó eszközökön megkönnyíthető okozó parancsok és az eszköz csak egy eszköz csatlakozik tooreceive őket. Az IoT-központ eszközspecifikus várólisták küldi hello parancsok tart fenn.

* Alkalmazás hasznos adatok külön-külön biztosítva a védett átvitel átjárók tooa adott szolgáltatáshoz.

hello mobil iparági hello szolgáltatás által támogatott kommunikációs mintát egyidejűleg használt hatalmas méretezési tooimplement leküldéses értesítéseket kezelő szolgáltatása például [Windows leküldéses értesítéseket kezelő szolgáltatása][lnk-wns], [A Google Cloud Messaging][lnk-google-messaging], és [Apple Push Notification szolgáltatás][lnk-apple-push].

Az IoT Hub használata az ExpressRoute nyilvános társhálózati útvonalain is támogatott.

## <a name="next-steps"></a>Következő lépések

toolearn hogyan toosend eszköz érkező üzenetek és a kapott IoT-központot, valamint hogyan tooconfigure üzenet irányítja, lásd: [üzeneteket küldjön és fogadjon IoT hubbal][lnk-send-messages].

toolearn hogyan IoT-központ lehetővé teszi, hogy szabványalapú Eszközkezelés az Ön tooremotely kezelése, konfigurálása és az eszközök frissítését lásd [IoT-központ az eszközkezelés áttekintése][lnk-device-management].

tooimplement ügyfélalkalmazások számos különböző eszközplatformok hardverek és operációs rendszereket, hello Azure IoT-eszközök SDK-k is használhatja. hello eszköz SDK-k, amelyek egyszerűbbé teszik a telemetriai adatok tooan IoT hub és fogadás felhő eszközre üzenetek küldése szalagtárak tartalmazza. Ha hello eszközzel SDK-k, az IoT hubbal választhat különböző hálózati protokollok toocommunicate. toolearn több, tekintse meg a hello [SDK-k eszközére vonatkozó információt][lnk-device-sdks].

Néhány kódot és az egyes minták futtatása elindult tooget lásd: hello [Ismerkedés az IoT-központ] [ lnk-get-started] oktatóanyag.

[img-architecture]: media/iot-hub-what-is-iot-hub/hubarchitecture.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[protocol-gateway]: https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md
[lnk-service-assisted-pattern]: http://blogs.msdn.com/b/clemensv/archive/2014/02/10/service-assisted-communication-for-connected-devices.aspx "Service Assisted Communication (Szolgáltatással támogatott kommunikáció) Clemens Vasters blogbejegyzése"
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-iotedge]: iot-hub-protocol-gateway.md
[lnk-field-gateway]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-devguide-identityregistry]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-wns]: https://msdn.microsoft.com/library/windows/apps/mt187203.aspx
[lnk-google-messaging]: https://developers.google.com/cloud-messaging/
[lnk-apple-push]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-iot-edge]: https://github.com/Azure/iot-edge
[lnk-send-messages]: iot-hub-devguide-messaging.md
[lnk-device-management]: iot-hub-device-management-overview.md

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[lnk-security-ground-up]: iot-hub-security-ground-up.md
