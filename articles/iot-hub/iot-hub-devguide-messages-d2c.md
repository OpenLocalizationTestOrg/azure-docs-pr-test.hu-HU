---
title: "aaaUnderstand Azure IoT Hub eszközről a felhőbe üzenetküldési |} Microsoft Docs"
description: "Fejlesztői útmutató - hogyan toouse eszköz-felhő IoT-központ az üzenetküldési. A telemetriai adatokat, és nem telemtry adatok küldését, és útválasztási toodeliver üzenetek használatával kapcsolatos adatokat tartalmaz."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.openlocfilehash: 07dc8a6be747365c7efbc528ab2762b0d9790758
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="send-device-to-cloud-messages-tooiot-hub"></a>Eszköz a felhőbe küldött üzeneteket tooIoT Hub küldése

toosend idősorozat telemetriai adatokat és az eszközök tooyour megoldás háttérrendszeréhez, a riasztások a eszközről a felhőbe üzeneteket küldhet az eszköz tooyour IoT hub a. Az IoT-központ által támogatott eszközről a felhőbe lehetőségeket tárgyalását lásd: [eszközről a felhőbe kommunikációs útmutatást][lnk-d2c-guidance].

Egy eszköz felé néző végpont keresztül eszközről a felhőbe üzeneteket küld (**/devices/ {deviceId} / üzenetek/események**). Útválasztási szabályokat, majd az üzenetek tooone hello szolgáltatás felé néző végpontok irányításához az IoT hub. Útválasztási szabályok használata hello fejlécek és törzsének átfutó a központ toodetermine eszközről a felhőbe köszönőüzenetei ahol tooroute őket. Üzenetek alapesetben irányított toohello beépített szolgáltatás felé néző végpont (**üzenetek/események**), amely kompatibilis a [Event Hubs][lnk-event-hubs]. Ezért, használhatja a szabványos [Event Hubs-integrációval és SDK-k] [ lnk-compatible-endpoint] háttér tooreceive eszköz-a-felhőbe küldött üzeneteket a megoldásban.

Az IoT-központ megvalósítja az eszközről a felhőbe üzenetküldési adatfolyam üzenetkezelési minta használatával. Az IoT-központ eszközről a felhőbe üzenetek mint [Event Hubs] [ lnk-event-hubs] *események* mint [Service Bus] [ lnk-servicebus] *üzenetek* abban, hogy nagyszámú események, hogy át kellene haladnia hello szolgáltatás, amely több olvasók által is olvasható.

Eszköz-felhő IoT-központ az üzenetküldési hello a következő jellemzőkkel rendelkezik:

* Eszköz a felhőbe küldött üzeneteket a következők tartós és az IoT-központ alapértelmezett megtartott **üzenetek/események** tooseven nap fel végpontot.
* Eszköz a felhőbe küldött üzeneteket legfeljebb 256 KB, és toooptimize küld kötegekben csoportosíthatók. Kötegek legfeljebb 256 KB lehet.
* Hello leírtak [vezérlő hozzáférés tooIoT Hub] [ lnk-devguide-security] szakaszban, IoT-központ lehetővé teszi, hogy a eszközönkénti hitelesítési és hozzáférés-vezérlés.
* Az IoT-központ lehetővé teszi a toocreate too10 egyéni végpontokat fel. Üzenetek kézbesítési útvonalakat az IoT hub konfigurált alapján toohello végpontok. További információkért lásd: [útválasztási szabályok](#routing-rules).
* Az IoT-központ lehetővé teszi, hogy egyidejűleg csatlakoztatott eszközök millióira (lásd: [kvóták és sávszélesség-szabályozási][lnk-quotas]).
* Az IoT-központ nem engedélyezi a tetszőleges particionálást. Eszköz a felhőbe küldött üzeneteket particionáltak, azok származó alapján **deviceId**.

Hello IoT-központ és az Event Hubs szolgáltatások hello különbségei kapcsolatos további információkért lásd: [összehasonlítása az Azure IoT-központ és az Azure Event Hubs][lnk-comparison].

## <a name="send-non-telemetry-traffic"></a>Nem telemetriai forgalom küldése

Gyakran továbbá tootelemetry adatpontok, eszközök üzeneteket küldhet, és külön végrehajtási és kezelését a hello megoldás háttérrendszerének igénylő kérelmek. Például egy adott művelet hello kell kiváltani a kritikus riasztások háttér. A felhasználó egyszerűen készíthet egy [útválasztási szabály] [ lnk-devguide-custom] toosend ilyen típusú üzenetek tooan végpont dedikált tootheir feldolgozási üdvözlőüzenetére vagy a fejlécet vagy hello üzenettörzsbe érték alapján.

Hello legjobb módja tooprocess a kind üzenet kapcsolatos további információkért lásd: hello [oktatóanyag: hogyan tooprocess IoT Hub eszköz-a-felhőbe küldött üzeneteket] [ lnk-d2c-tutorial] oktatóanyag.

## <a name="route-device-to-cloud-messages"></a>Eszköz-felhő üzenetek

Az útválasztási eszköz a felhőbe küldött üzeneteket tooyour háttér-alkalmazások két lehetőség közül választhat:

* Használja a hello beépített [Event Hub-kompatibilis végpont] [ lnk-compatible-endpoint] tooenable háttér-alkalmazások tooread hello eszközről a felhőbe által fogadott üzenetek hello központ. toolearn hello beépített Event Hub-kompatibilis végpont kapcsolatban lásd: [eszköz a felhőbe küldött üzeneteket beolvasni hello beépített végpont][lnk-devguide-builtin].
* Az IoT hub útválasztási szabályok toosend üzenetek toocustom végpontok használja. Egyéni végpontokat engedélyezése a háttér-alkalmazások tooread eszközről a felhőbe az Event Hubs, a Service Bus-üzenetsorok vagy a Service Bus-üzenettémakörök használata. útválasztási és egyéni végpont, kapcsolatos toolearn lásd [egyéni végpontokat és útválasztási szabályokat használja az eszköz a felhőbe küldött üzeneteket][lnk-devguide-custom].

## <a name="anti-spoofing-properties"></a>Hamisításszűrés tulajdonságai

az eszközről a felhőbe üzenetekben, IoT-központ címhamisítást következő tulajdonságai hello üzenetek bélyegzők tooavoid eszköz:

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

hello először két tartalmaznia hello **deviceId** és **generationId** az eszköz, származó megfelelően hello [identitás eszköztulajdonságok][lnk-device-properties].

Hello **ConnectionAuthMethod** tulajdonság egy szerializált JSON-objektum tartalmazza a következő tulajdonságai hello:

```json
{
  "scope": "{ hub | device}",
  "type": "{ symkey | sas}",
  "issuer": "iothub"
}
```

## <a name="next-steps"></a>Következő lépések

Információ a hello SDK-k használata a toosend eszköz-a-felhőbe küldött üzeneteket, hogy [Azure IoT SDK-k][lnk-sdks].

Hello [Ismerkedés] [ lnk-get-started] oktatóprogramok bemutatják, hogyan toosend eszközről-a-felhőbe a szimulált és a fizikai eszközök érkező üzenetek. További részletekért lásd: hello [folyamat IoT Hub eszközről a felhőbe üzenetek útvonalak] [ lnk-d2c-tutorial] oktatóanyag.

[lnk-devguide-builtin]: iot-hub-devguide-messages-read-builtin.md
[lnk-devguide-custom]: iot-hub-devguide-messages-read-custom.md
[lnk-comparison]: iot-hub-compare-event-hubs.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[lnk-get-started]: iot-hub-get-started.md

[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-servicebus]: http://azure.microsoft.com/documentation/services/service-bus/
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-compatible-endpoint]: iot-hub-devguide-messages-read-builtin.md
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
