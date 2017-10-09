---
title: "Azure IoT Hub felhő eszköz aaaUnderstand üzenetküldési |} Microsoft Docs"
description: "Fejlesztői útmutató - hogyan toouse felhő eszközre az IoT-központ az üzenetküldési. Hello üzenet életciklusát, és a konfigurációs beállítások kapcsolatos adatokat tartalmaz."
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
ms.openlocfilehash: 5c747b50163873d823556a8baa769c4b8f7f8c44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="send-cloud-to-device-messages-from-iot-hub"></a>Felhő-eszközre küldött üzenetek küldése az IoT-központ

toosend egyirányú értesítések toohello eszköz alkalmazást, a megoldás háttérrendszeréhez, az IoT hub tooyour eszközről felhő-eszközök üzeneteket küldeni. Más felhőalapú-eszközök beállításai, IoT-központ által támogatott tárgyalását lásd: [felhő eszközre kommunikációs útmutatást][lnk-c2d-guidance].

A szolgáltatás felé néző végpont keresztül felhő eszközre üzeneteket küld (**/üzenetek/devicebound**). Egy eszköz majd megkapja egy eszközspecifikus végpontot keresztül köszönőüzenetei (**/devices/ {deviceId} / üzenetek/devicebound**).

Minden felhő eszközre üzenetet egyetlen eszközt a célja által hello beállítása **való** tulajdonság túl**/devices/ {deviceId} / üzenetek/devicebound**.

Minden eszköz várólista legfeljebb 50 felhő eszközre üzeneteket tárolja. Próbált toosend további üzenetek toohello ugyanarra az eszközre hibát eredményez.

## <a name="hello-cloud-to-device-message-lifecycle"></a>hello felhő-eszközre küldött üzenetek életciklusa

tooguarantee: legalább egyszeri üzenet kézbesítési, IoT-központ továbbra is fennáll, eszköz felhő eszközre üzeneteket. Eszközök kell kifejezetten elismeri *befejezési* az IoT-központ tooremove azokat hello várólista. Ez a megközelítés biztosítja, hogy kapcsolatot és meghibásodása esetén szembeni hibatűrést.

hello következő diagramon láthatók hello életciklus állapot graph felhő eszközre üzenetet az IoT-központot.

![Felhő-eszközre küldött üzenetek életciklusa][img-lifecycle]

Ha hello IoT-központ szolgáltatás elküldi egy üzenet tooa eszköz, hello szolgáltatás túl beállítja hello üzenet állapot**a várólistában levő**. Ha egy eszköz túl szeretne*kap* üzenetet az IoT-központ *zárolások* üdvözlőüzenetére (hello állapot túl beállításával**láthatatlan**), amely lehetővé teszi, hogy más szálak hello eszköz toostart a más üzenetek fogadására. Egy eszköz szál befejezése után egy üzenet feldolgozása hello értesíti a felhasználót az IoT-központ által *befejezése* üdvözlőüzenetére. Az IoT-központ majd az túl hello állapotának beállítása**befejezve**.

Egy eszköz is megteheti, hogy:

* *Elutasítása* üdvözlőüzenetére, aminek következtében az IoT-központ tooset azt toohello **Deadlettered** állapotát. Hello MQTT protokoll keresztül csatlakozó eszközökön nem utasíthat el a felhő-eszközre küldött üzenetek.
* *Abandon* üdvözlőüzenetére, aminek következtében az IoT-központ tooput üdvözlőüzenetére újra hello várólista, a hello állapot értéke túl a**a várólistában levő**.

A szál sikertelen lehet, tooprocess egy üzenetet az IoT-központ értesítése nélkül. Ebben az esetben üzenetek automatikusan a hello áttűnés **láthatatlan** állapot hátsó toohello **a várólistában levő** állapot után egy *látható (vagy a zárolás) időtúllépési*. hello alapértelmezett Ez az időkorlát értéke egy perc.

Egy üzenet is átmenet hello közötti **a várólistában levő** és **láthatatlan** állapotai, legfeljebb hányszor hello megadott hello **kézbesítési száma legfeljebb** IoT hub tulajdonság. Adott áttérések számával, miután az IoT-központ hello állapotának beállítása üdvözlőüzenetére túl**Deadlettered**. Ehhez hasonlóan az IoT-központ hello állapotának beállítása üzenet túl**Deadlettered** a lejárati idő után (lásd: [toolive idő][lnk-ttl]).

Hello [hogyan toosend felhőből eszközre küldött üzenetek IoT-központ] [ lnk-c2d-tutorial] bemutatja, hogyan toosend felhő eszközre üzeneteit hello a felhő és a fogadásukra az eszközön.

Általában egy eszköz befejezése felhő eszközre üzenet, amikor üdvözlőüzenetére hello megszűnését hello úgy az alkalmazáslogikát nincs hatással. Például ha hello eszköz rendelkezik megőrzött hello üzenet tartalmat a helyi vagy sikeresen megtörtént a következő művelet végrehajtása. üdvözlőüzenetére is sikerült továbbítani az átmeneti információkat, amelyek adatvesztés nem befolyásolná a hello alkalmazás hello funkcióit. Egyes esetekben a hosszan futó feladatokat, befejezheti felhő eszközre üdvözlőüzenetére után megőrzése hello feladat leírása található helyi tárterület. Majd értesítheti hello megoldás háttérrendszerének integrációját egy vagy több eszköz-a-felhőbe küldött üzeneteket különböző szakaszaiban hello a feladat előrehaladását.

## <a name="message-expiration-time-toolive"></a>Üzenet lejárati (toolive idő)

Minden felhő eszközre üzenetnek lejárati időt. Megadott idő van beállítva, vagy hello szolgáltatást (hello a **ExpiryTimeUtc** tulajdonság), vagy az IoT hubból hello alapértelmezett *toolive idő* egy IoT-központ tulajdonságban megadott. Lásd: [felhő eszközre konfigurációs beállítások][lnk-c2d-configuration].

Egy általános módszer tootake előnyeit lejárati üzenet és üzenetküldésre toodisconnected eszközök elkerüléséhez tooset toolive értékeket rövid időn belül van. Ez a megközelítés azonos eredménye karbantartása hello eszköz kapcsolati állapotát, miközben sokkal hatékonyabb, hello éri el. Amikor üzenet nyugták kér le, az IoT-központ értesítést küld, mely eszközök képes tooreceive üzeneteket, és mely eszközök nincs online állapotban, vagy nem sikerült.

## <a name="message-feedback"></a>Üzenet visszajelzés

Felhő eszközre üzenet küldésekor hello szolgáltatás kérhetnek hello végső állapot üzenet vonatkozó állapotüzenet visszajelzés hello kézbesítését.

| Nyugtázási tulajdonság | Viselkedés |
| ------------ | -------- |
| **pozitív** | Az IoT-központ visszajelzést-üzenetet hoz létre, ha, és csak akkor hello felhő eszközre üzenet elérte hello **befejezve** állapotát. |
| **negatív.** | Az IoT-központ visszajelzés üzenetet hoz létre, csak ha, felhőalapú eszközre üdvözlőüzenetére eléri hello **Deadlettered** állapotát. |
| **teljes**     | Az IoT-központ visszajelzés üzenet mindkét esetben állít elő. |

Ha **nyugtázási** van **teljes**, és nem kap egy üzenetet, visszajelzést, az azt jelenti, hogy üdvözlőüzenetére visszajelzés lejárt. hello szolgáltatás nem tudja, milyen történt toohello eredeti üzenet. A gyakorlatban egy szolgáltatás győződjön meg arról, hogy feldolgozható hello visszajelzés lejárata előtt. hello maximális lejárati ideje két nap bőséges időt tooget hello szolgáltatás újra futtatni a hiba akkor fordul elő, ha lehetővé teszi.

A [végpontok][lnk-endpoints], IoT-központ biztosítja a szolgáltatás felé néző végpont visszajelzései (**/messages/servicebound/feedback**) üzeneteihez. hello szemantikáját visszajelzés fogadására vannak hello ugyanaz, mint a felhő-eszközre küldött üzenetek és hello azonos [üzenet életciklus][lnk-lifecycle]. Amikor csak lehetséges, üzenet visszajelzés egyetlen üzenetben van kötegelni a hello a következő formátumban:

| Tulajdonság     | Leírás |
| ------------ | ----------- |
| EnqueuedTime | Üdvözlőüzenetére létrehozásának jelző időbélyegző. |
| Felhasználói azonosítóját       | `{iot hub name}` |
| A ContentType  | `application/vnd.microsoft.iothub.feedback.json` |

hello törzse JSON-szerializált tömbje rögzíti, az alábbi tulajdonságokkal hello:

| Tulajdonság           | Leírás |
| ------------------ | ----------- |
| EnqueuedTimeUtc    | Mikor történt a üdvözlőüzenetére hello eredményét jelző időbélyegző. Például az eszköz befejezte hello, vagy üdvözlőüzenetére lejárt. |
| originalMessageId  | **MessageId** hello felhő-eszközre küldött üzenetek toowhich a visszajelzési információk vonatkozik. |
| statusCode         | Szükséges karakterlánc. Az IoT-központ által generált visszajelzés üzenetekben használatos. <br/> "Sikeres" <br/> "A lejárt" <br/> "DeliveryCountExceeded" <br/> "Visszautasított" <br/> "Kiürítve" |
| Leírás        | Karakterlánc-értékek **StatusCode**. |
| Eszközazonosító           | **DeviceId** hello céleszköz a hello felhő-eszközre küldött üzenetek toowhich a visszajelzésekben vonatkozik. |
| DeviceGenerationId | **DeviceGenerationId** hello céleszköz a hello felhő-eszközre küldött üzenetek toowhich a visszajelzésekben vonatkozik. |

hello szolgáltatást meg kell adnia egy **MessageId** a hello felhő eszközre üzenet toobe képes toocorrelate a visszajelzés hello eredeti üzenettel.

hello következő példa bemutatja hello visszajelzés üzenet törzsét.

```json
[
  {
    "OriginalMessageId": "0987654321",
    "EnqueuedTimeUtc": "2015-07-28T16:24:48.789Z",
    "StatusCode": 0,
    "Description": "Success",
    "DeviceId": "123",
    "DeviceGenerationId": "abcdefghijklmnopqrstuvwxyz"
  },
  {
    ...
  },
  ...
]
```

## <a name="cloud-to-device-configuration-options"></a>Felhő-eszköz konfigurációs beállítások

Minden egyes IoT-központ mutatja meg a következő konfigurációs beállításokat a felhőből eszközre üzenetek hello:

| Tulajdonság                  | Leírás | Tartomány- és alapértelmezett |
| ------------------------- | ----------- | ----------------- |
| defaultTtlAsIso8601       | Felhő-eszközre küldött üzenetek alapértelmezett élettartam. | Másolatot too2D ISO_8601 időköz (legalább 1 perc). Alapértelmezett: 1 óra. |
| maxDeliveryCount          | Felhő eszközre eszközönkénti sorok maximális száma. | 1 too100. Alapértelmezett: 10. |
| feedback.ttlAsIso8601     | Szolgáltatás adathoz kötött visszajelzés üzenetek megőrzési. | Másolatot too2D ISO_8601 időköz (legalább 1 perc). Alapértelmezett: 1 óra. |
| feedback.maxDeliveryCount |A visszajelzési üzenetsor maximális száma. | 1 too100. Alapértelmezett: 100. |

További információ a hogyan tooset a konfigurációs beállításokról: [létrehozása IoT-központok][lnk-portal].

## <a name="next-steps"></a>Következő lépések

Információ a hello SDK-k használata a tooreceive felhő eszközre üzenetek, hogy [Azure IoT SDK-k][lnk-sdks].

tootry ki felhő-eszközre küldött üzenetek fogadását, lásd: hello [küldése eszközre felhő] [ lnk-c2d-tutorial] oktatóanyag.

[img-lifecycle]: ./media/iot-hub-devguide-messages-c2d/lifecycle.png

[lnk-portal]: iot-hub-create-through-portal.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-ttl]: #message-expiration-time-to-live
[lnk-c2d-configuration]: #cloud-to-device-configuration-options
[lnk-lifecycle]: #message-lifecycle
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
