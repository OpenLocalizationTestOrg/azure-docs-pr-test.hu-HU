---
title: "aaaUnderstand Azure IoT Hub árképzési |} Microsoft Docs"
description: "Fejlesztői útmutató - hogyan mérési és árképzési működik, beleértve az IoT-központ dolgozott példák kapcsolatos információkat."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 1ac90923-1edf-4134-bbd4-77fee9b68d24
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2016
ms.author: elioda
ms.openlocfilehash: e294c0b7f483e042ca3f63e93c14e0c2d773ae7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-iot-hub-pricing-information"></a>Díjszabási információkért Azure IoT Hub

[Az Azure IoT Hub árképzési] [ lnk-pricing] hello általános információkat nyújt a különböző Termékváltozatai és az IoT-központ díjszabása. A cikkben megtudhatja, hogyan hello különböző IoT-központ funkciók mérése, IoT-központ által küldött üzenetek további részletekkel.

## <a name="charges-per-operation"></a>Művelet díjak

| Művelet | Számlázási adatokat | 
| --------- | ------------------- |
| Identitásjegyzék műveletei <br/> (létrehozása, beolvasása, listázása, frissítése és törlése) | Nem számítjuk fel. |
| Az eszközről a felhőbe irányuló üzenetek | Sikeresen elküldött üzenetek van szó, a 4 KB-os adattömbök érkező az IoT hubhoz például 6 KB-os üzenet díjfizetéssel 2 üzenet. |
| Felhő-eszközre küldött üzenetek | Sikeresen elküldött üzenetek van szó, a 4 KB-os adattömböket, például egy 6 KB-os üzenet díjfizetéssel 2 üzenet. |
| Fájlfeltöltések | A fájl adatátviteli tooAzure tárolási nem forgalmi díjas IoT hubból. Átviteli kezdeményezés és befejezési üzeneteit van szó, mint 4 KB-os lépésekben mért messaged. Például egy 10 MB-os fájlban fel van töltve az Azure Storage-költségeket, továbbá toohello két üzenet. |
| Közvetlen metódusok | Sikeres metóduskérelmek van szó, a 4 KB-os adattömböket, nem üres azokkal válaszait van szó, a 4 KB-os további üzeneteihez. Kérelmek toodisconnected eszközök van szó, a 4 KB-os adattömbök üzeneteihez. Például egy metódust, amely a válasz nem szervezethez hello eszközről 6 KB-os szervezethez díjfizetéssel két üzenetekként; egy metódust, amely hello eszközről egy 1 KB-os válasz eredményez 6 KB-os szervezethez hello kérelem két üzenetről és egy másik üzenetet hello válasz fel van töltve. |
| Ikereszköz-olvasások | Eszköz iker beolvassa hello eszközről és a megoldás háttérrendszere hello end van szó, mint 512 bájtos adattömbök üzenetek. Például egy 6 KB-os eszköz iker olvasása díjfizetéssel 12 üzeneteihez. |
| A két eszközfrissítésekhez (címkék és tulajdonságait) | Kettős eszközfrissítésekhez hello eszköz, hello eszközről van szó, mint 512 bájtos adattömbök üzenetek. Például egy 6 KB-os eszköz iker olvasása díjfizetéssel 12 üzeneteihez. |
| Eszköz iker lekérdezések | Lekérdezések van szó, attól függően, hogy az 512 bájtos adattömbök hello eredmény méretének üzeneteihez. |
| Feladatműveletek <br/> (létrehozás, frissítés, listázás, törlés) | Nem számítjuk fel. |
| Feladatok eszközönkénti műveletek | Feladatok műveletek (például a kettős eszközfrissítésekhez, és módszerek) normál van szó. Például egy 1 KB-os kérelmek és üres-törzsének válaszokban 1000 metódushívások eredményezve feladat díjfizetéssel 1000 üzeneteket. |

> [!NOTE]
> Különböző méretű arra az esetre vonatkoznak annak eldöntéséhez, hogy a hello tartalom mérete bájtban (protokoll keretezési figyelmen kívül hagyja). Üzenetek (melyek tulajdonságait és a szervezet) esetén hello mérete számított protokoll-független módon hello leírtak [IoT Hub fejlesztői útmutató üzenetküldési][lnk-message-size].

## <a name="example-1"></a>#1. példa

Egy eszköz / perc tooIoT Hub, amely majd beolvassa az Azure Stream Analytics egy 1 KB-os eszközről a felhőbe üzenetet küld. hello megoldás háttérrendszerének hív meg (a 512 bájtos tartalom) metódus hello eszközön minden tíz perc tootrigger olyan adott műveletet. hello eszköz válaszol 200 bájt eredménye toohello metódust.

hello eszköz 1 üzenetet feldolgozó * 60 perc * 24 óra = 1440 üzenet köszönőüzenetei eszközről a felhőbe, és 2 kérés és válasz naponta * 6 alkalommal óránként * 24 óra = 288 üzenetek hello módszerek 1728 üzenet naponta összesen.

## <a name="example-2"></a>#2. példa

Egy eszköz óránként egy 100 KB-os eszközről a felhőbe üzenetet küld. Azt is frissíti az eszköz kettős 1 KB-os hasznos adat található 4 óránként. hello háttérrendszerre, naponta egyszer, olvasási hello 14 KB-os eszköz iker befejezését, majd frissíti a hasznos adatok az 512 bájtos toochange konfigurációival.

hello eszköz feldolgozó 25 (100KB/4 KB-os) üzenetek * eszközről a felhőbe üzenetek, valamint 1 üzenetet 24 órát * 6 napi időpontot az eszközfrissítésekhez két, összesen 156 üzenet naponta.
hello megoldás háttérrendszerének használ fel (14KB/0,5 KB) 28 üzenetek tooread hello eszköz iker, valamint 1 üzenetet tooupdate 29 üzenetek összesen azt.

Összesen a hello eszköz és hello megoldás háttérrendszerének 185 üzenet naponta felhasználását.


[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-message-size]: iot-hub-devguide-messages-construct.md
