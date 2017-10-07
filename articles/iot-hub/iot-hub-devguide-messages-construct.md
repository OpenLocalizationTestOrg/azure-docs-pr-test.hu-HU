---
title: "aaaUnderstand Azure IoT Hub üzenetformátum |} Microsoft Docs"
description: "Fejlesztői útmutató - descibes hello formátum és az IoT-központ üzenetek várt tartalom."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3fc5f1a3-3711-4611-9897-d4db079b4250
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.openlocfilehash: 3b1567e47bc32f70c0c252996648c4035ae81243
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-read-iot-hub-messages"></a>Hozzon létre, és az IoT-központ üzenet olvasása

toosupport zökkenőmentes együttműködési biztosíthat a protokollokon, IoT-központ az összes eszköz számára is elérhető protokollhoz közös üzenetformátum határozza meg. Mindkét használt üzenetformátuma [eszközről a felhőbe] [ lnk-d2c] és [felhő eszközre] [ lnk-c2d] üzeneteket. Egy [IoT-központ üzenet] [ lnk-messaging] áll:

* Egy *Rendszertulajdonságok*. Az IoT-központ értelmezi, vagy beállítja a tulajdonságokat. A csoportok pedig előre meghatározott.
* Egy *alkalmazástulajdonságok*. A szótár hello alkalmazás meghatározó karakterlánc-tulajdonságok és a hozzáférés, anélkül, hogy toodeserialize hello üzenet törzsében. Az IoT-központ soha nem módosítja ezeket a tulajdonságokat.
* Nem átlátszó bináris törzsében.

Nevét és értékeit csak ASCII alfanumerikus karaktereket tartalmazhat, valamint ``{'!', '#', '$', '%, '&', "'", '*', '*', '+', '-', '.', '^', '_', '`', '|', '~'}`` amikor Ön:

* Hello HTTP protokollt használó eszközről a felhőbe üzeneteket küldeni.
* Felhő-eszközre küldött üzenetek küldése.

További információ tooencode és különböző protokollok használatával küldött üzenetek dekódolni, lásd: [Azure IoT SDK-k][lnk-sdks].

hello a következő táblázat sorolja fel az IoT-központ üzeneteket a rendszer tulajdonságai hello készletét.

| Tulajdonság | Leírás |
| --- | --- |
| messageId |A kérelem-válasz minták használt üdvözlőüzenetére felhasználó állítható be azonosítója. Formátum: A kis-és nagybetűket (felfelé too128 karakter) ASCII 7 bites alfanumerikus karakterekből álló karakterlánc + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| Sorozat száma |Az IoT-központ tooeach felhő eszközre üzenet által hozzárendelt szám (eszköz-várólista minden egyedi). |
| túl|A megadott célhelyre [felhő eszközre] [ lnk-c2d] üzeneteket. |
| ExpiryTimeUtc |Dátum és az üzenet lejárati idejét. |
| EnqueuedTime |Dátum és idő hello [felhő eszközre] [ lnk-c2d] üzenet érkezett az IoT-központot. |
| CorrelationId |Egy válaszüzenetben hello hello kérelem a kérelem-válasz minták MessageId általában tartalmazó karakterlánc típusú tulajdonság. |
| Felhasználói azonosítóját |Egy azonosító használt üzenetek toospecify hello forrása. Az IoT-központ által előállított üzeneteket, ha túl van-e állítva`{iot hub name}`. |
| Nyugtázási |A visszajelzési üzenet generátor. Ez a tulajdonság hello eszköz használ felhő-eszközre küldött üzenetek toorequest IoT-központ toogenerate visszajelzés üzenetek üdvözlőüzenetére hello használata miatt. A lehetséges értékek: **nincs** (alapértelmezett): Nincs visszajelzés üzenet jön létre, **pozitív**: visszajelzés üzenetet kap, ha üdvözlőüzenetére befejeződött, **negatív**: kap egy Ha nélkül hello eszköz befejezését üdvözlőüzenetére lejárt (vagy elérte a maximális száma) visszajelzés üzenet vagy **teljes**: pozitív és negatív. További információkért lásd: [visszajelzés üzenet][lnk-feedback]. |
| ConnectionDeviceId |Az eszköz a felhőbe küldött üzeneteket az IoT-központ által beállított azonosító. Hello tartalmaz **deviceId** hello üzenetet küldő hello eszköz. |
| ConnectionDeviceGenerationId |Az eszköz a felhőbe küldött üzeneteket az IoT-központ által beállított azonosító. Hello tartalmaz **generationId** (megfelelően [identitás eszköztulajdonságok][lnk-device-properties]) hello üzenetet küldő hello eszköz. |
| ConnectionAuthMethod |Az eszköz a felhőbe küldött üzeneteket az IoT-központ által beállított hitelesítési módszert. Ez a tulajdonság hello hitelesítési módszerét tooauthenticate hello eszköz hello üzenetet küld információkat tartalmaz. További információkért lásd: [eszköz toocloud hamisításszűrés][lnk-antispoofing]. |

## <a name="message-size"></a>Üzenet mérete

Az IoT-központ méri üzenet mérete protokoll-független módon csak hello tényleges hasznos figyelembe véve. hello mérete bájtban hello összeg hello következő számítható ki:

* hello törzs mérete bájtban.
* hello mérete bájtban hello üzenet tulajdonságainak hello értékek.
* hello mérete bájtban minden felhasználó nevét és értékeit.

Nevét és értékeit karakterek korlátozott tooASCII, így hello karakterláncok hello hosszát eredménye hello mérete bájtban.

## <a name="next-steps"></a>Következő lépések

További információ a méretkorlátozásokról üzenetet az IoT hubon: [IoT-központ kvóták és sávszélesség-szabályozási][lnk-quotas].

toolearn hogyan toocreate és a különböző programnyelveken, olvassa el az IoT-központ üzenetek: hello [Ismerkedés] [ lnk-get-started] oktatóanyagok.

[lnk-messaging]: iot-hub-devguide-messaging.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-get-started]: iot-hub-get-started.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-feedback]: iot-hub-devguide-messages-c2d.md#message-feedback
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-antispoofing]: iot-hub-devguide-messages-d2c.md#anti-spoofing-properties
