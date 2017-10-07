---
title: "aaaUnderstand Azure IoT-központok végpontjai |} Microsoft Docs"
description: "Fejlesztői útmutató - információk az IoT-központ eszköz számára is elérhető, és a szolgáltatás felé néző végpontok."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 57ba52ae-19c6-43e4-bc6c-d8a5c2476e95
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 8647f15d2f2a050ad5799ea82f4d2d46db0dbec1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reference---iot-hub-endpoints"></a>Referencia - IoT-központok végpontjai

## <a name="iot-hub-names"></a>Az IoT-központ nevét

Hello neve, amelyen a végpontokat a hello hello portálon hello IoT-központ található **áttekintése** panelen. Alapértelmezés szerint az IoT-központ hello DNS-neve a következőképpen néz: `{your iot hub name}.azure-devices.net`.

Használhatja az Azure DNS toocreate az IoT hub egy egyéni DNS-nevet. További információkért lásd: [használata Azure DNS-tooprovide egyéni tartomány beállításait az Azure-szolgáltatások](../dns/dns-custom-domain.md#azure-iot).

## <a name="list-of-built-in-iot-hub-endpoints"></a>Beépített IoT-központok végpontjai listája

Az Azure IoT-központot egy több-bérlős szolgáltatást, amely elérhetővé teszi a funkció toovarious szereplője. hello alábbi ábrán látható hello különböző végpontok, amely az IoT-központ elérhetővé teszi.

![IoT Hub-végpontok][img-endpoints]

a következő lista hello hello végpontok ismerteti:

* **Erőforrás-szolgáltató**. hello IoT-központ erőforrás-szolgáltató közzétesz egy [Azure Resource Manager] [ lnk-arm] felületet. Ez lehetővé teszi az Azure-előfizetések tulajdonosai toocreate és IoT-központok és tooupdate IoT hub tulajdonságainak törlése. Az IoT Hub tulajdonságainak szabályozására [hub szintű biztonsági házirendek][lnk-accesscontrol], és nem toodevice szintű hozzáférés-vezérlés, és működési beállításainak felhő eszköz és eszköz-felhő üzenetküldési. hello IoT-központ erőforrás-szolgáltató azt is lehetővé teszi túl[eszköz identitások exportálása][lnk-importexport].
* **Eszköz Identitáskezelés**. Minden egyes IoT-központ tesz elérhetővé a HTTP REST végpontok toomanage eszköz azonosítók (létrehozása, beolvasása, frissítése és törlése). [Eszköz identitások] [ lnk-device-identities] eszköz hitelesítési és hozzáférés-vezérlés használhatók.
* **A két kezelés**. Minden egyes IoT-központ tesz elérhetővé, szolgáltatás felé néző végpont tooquery HTTP REST és frissítés [eszköz twins] [ lnk-twins] (címkék és a tulajdonságok frissítése).
* **Felügyeleti feladatok**. Minden egyes IoT-központ szolgáltatás irányuló HTTP REST-végpont tooquery tesz elérhetővé, és kezelheti [feladatok][lnk-jobs].
* **Eszköz végpontok**. Az egyes eszközök a hello identitásjegyzékhez IoT-központ tesz elérhetővé a végpontok:

  * *Eszköz-felhő üzenetek küldéséhez*. Egy eszköz használja ezt a végpontot túl[eszközről a felhőbe üzenetek küldéséhez][lnk-d2c].
  * *Felhő-eszközre küldött üzenetek fogadására*. Egy eszköz használja a végpont tooreceive céloz [felhő-eszközre küldött üzenetek][lnk-c2d].
  * *Fájlfeltöltési kezdeményezése*. Egy eszköz használja a végpont tooreceive egy Azure Storage SAS URI-azonosító az IoT-központ túl[-fájl feltöltése][lnk-upload].
  * *A lekérésére és frissítésére iker eszköztulajdonságok*. Egy eszköz a végpont tooaccess használja a [eszköz iker][lnk-twins]tartozó tulajdonságok.
  * *Közvetlen módszer kérések fogadásához*. Egy eszköz használja ezt a végpontot toolisten [közvetlen módszer][lnk-methods]a kérelmeket.

    Ezeket a végpontokat feltárt használatával [MQTT v3.1.1][lnk-mqtt], HTTP 1.1-es és [AMQP 1.0] [ lnk-amqp] protokollokat. AMQP is rendelkezésre áll, keresztül [websocket elemek] [ lnk-websockets] a 443-as porton.

    hello eszköz twins és módszerek végpontok érhetők el, csak ha hello használata [MQTT v3.1.1] [ lnk-mqtt] protokoll.

* **Szolgáltatás végpontjait**. Minden egyes IoT-központ tesz elérhetővé a megoldás háttér toocommunicate végpontokat az eszközöket. Egy kivétellel ezeket a végpontokat csak akkor jelennek meg hello segítségével [AMQP] [ lnk-amqp] protokoll. hello metódus meghívása végpont hello HTTP protokollon keresztül kommunikál.
  
  * *Eszköz-felhő üzeneteket fogadni*. Ez a végpont összeegyeztethető [Azure Event Hubs][lnk-event-hubs]. A háttér-szolgáltatás használható tooread hello [eszköz a felhőbe küldött üzeneteket] [ lnk-d2c] az eszközök által küldött. Létrehozhat egyéni végpontokat az IoT hub hozzáadása toothis beépített végpont.
  * *Felhő-eszközre küldött üzenetek küldésére és fogadására a kézbesítési visszaigazolások*. Ezeket a végpontokat engedélyezése a megoldás háttér toosend megbízható [felhő-eszközre küldött üzenetek][lnk-c2d], és tooreceive hello megfelelő kézbesítési vagy lejárati nyugták.
  * *Fájl értesítéseket*. Üzenetküldési végponti lehetővé teszi tooreceive értesítések, amikor az eszközök sikeresen-fájl feltöltése. 
  * *A metódushívás közvetlen*. Ez a végpont lehetővé teszi, hogy a háttér-szolgáltatás tooinvoke egy [közvetlen módszer] [ lnk-methods] az eszközön.
  * *Fogadási műveletek események figyelése*. Ez a végpont események figyelése, ha az IoT hub lett konfigurálva tooemit tooreceive műveletek lehetővé teszi őket. További információkért lásd: [IoT-központ műveletek figyelési][lnk-operations-mon].

Hello [Azure IoT SDK-k] [ lnk-sdks] cikkből hello különböző módokon tooaccess ezeket a végpontokat.

Minden IoT-központok végpontjai hello használata [TLS] [ lnk-tls] protokoll, és nincs végpont legalább egyszer fel van fedve titkosítatlanul/titkosítatlan csatornán.

## <a name="custom-endpoints"></a>Egyéni végpontokat

Az előfizetés tooyour IoT hub tooact a meglévő Azure-szolgáltatások is kapcsolhat üzenet útválasztási végpontjaiként. Ezeket a végpontokat Szolgáltatásvégpontok összekötőként és üzenet útvonalak mosdók használják. Eszközök nem írható közvetlenül toohello további végpontokat. További információ az üzenet útvonalak toolearn látható hello fejlesztői útmutató bejegyzés [IoT hubbal üzenetek küldése és fogadása][lnk-devguide-messaging].

Az IoT-központ jelenleg a következő Azure-szolgáltatások további végpontként hello támogatja:

* Event Hubs
* Service Bus által kezelt üzenetsorok
* Service Bus-üzenettémák

Az IoT-központ írási toothese Szolgáltatásvégpontok üzenet útválasztási toowork szüksége van. Konfigurálja a végpontokat hello Azure-portálon keresztül, ha a hello szükséges engedélyek az Ön kerülnek. Ügyeljen arra, hogy a szolgáltatások toosupport várt hello átviteli sebesség. Az IoT-megoldásból először konfigurálásakor toomonitor a további végpontokat kell, és minden szükséges változtatások hello tényleges betöltést.

Ha egy üzenetet, hogy az összes mutatnak toohello több útvonal megegyezik ugyanabban a végponthoz, IoT-központ biztosítja üzenet toothat végpont csak egyszer. Emiatt nem kell tooconfigure deduplikációs a Service Bus-üzenetsorba vagy témakör teheti meg. A particionált várólisták partíció affinitás biztosítja, hogy az üzenet rendezés.

> [!NOTE]
> Service Bus-üzenetsorok és témakörök használatos az IoT-központok végpontjai nem lehet **munkamenetek** vagy **ismétlődő észlelési** engedélyezve van. Ha ezek a lehetőségek valamelyikét engedélyezve vannak, hello végpont megjelenik **Unreachable** a hello Azure-portálon.

Hello vonatkozó korlátozások is hozzáadhat végpontok hello száma, lásd: [kvóták és sávszélesség-szabályozási][lnk-devguide-quotas].

## <a name="field-gateways"></a>A mező átjárók

Az IoT-megoldás egy *mező átjáró* között az eszközök és az IoT-központok végpontjai helyezkedik el. Általában Bezárás tooyour eszközöket is. A kommunikálnak az eszközök közvetlenül hello mező átjáró hello eszközök támogatják a protokoll használatával. hello mező átjáró csatlakozik tooan IoT-központ végpontjának IoT-központ által támogatott protokoll használatát. Egy mező átjáró egy dedikált hardvereszköz, vagy az alacsony futtató egyéni átjáró szoftverének lehet.

Használhat [Azure IoT peremhálózati] [ lnk-iot-edge] tooimplement mező átjáró. IoT biztonsági funkciók, például multiplexáló alakzatot hello több eszközöktől érkező kommunikációt biztosít ugyanazt az IoT-központ a kapcsolatot.

## <a name="next-steps"></a>Következő lépések

Az IoT Hub fejlesztői útmutató egyéb témaköröket tartalmazza:

* [Az IoT-központ lekérdezési nyelv eszköz twins, a feladatok és az üzenet-útválasztás][lnk-devguide-query]
* [Kvóták és sávszélesség-szabályozás][lnk-devguide-quotas]
* [Az IoT Hub MQTT támogatása][lnk-devguide-mqtt]

[lnk-iot-edge]: https://github.com/Azure/iot-edge

[img-endpoints]: ./media/iot-hub-devguide-endpoints/endpoints.png
[lnk-amqp]: https://www.amqp.org/
[lnk-mqtt]: http://mqtt.org/
[lnk-websockets]: https://tools.ietf.org/html/rfc6455
[lnk-arm]: ../azure-resource-manager/resource-group-overview.md
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/

[lnk-tls]: https://tools.ietf.org/html/rfc5246


[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-accesscontrol]: iot-hub-devguide-security.md#access-control-and-permissions
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-device-identities]: iot-hub-devguide-identity-registry.md
[lnk-upload]: iot-hub-devguide-file-upload.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md

[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[lnk-operations-mon]: iot-hub-operations-monitoring.md
