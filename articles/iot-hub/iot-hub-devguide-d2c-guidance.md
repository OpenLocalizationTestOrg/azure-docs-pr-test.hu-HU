---
title: "IoT Hub eszköz-felhő aaaAzure beállítások |} Microsoft Docs"
description: "Fejlesztői útmutató - amikor toouse eszközről a felhőbe üzenetek, a jelentésben szereplő tulajdonságok vagy a fájl feltöltése a felhőből eszközre kommunikációhoz útmutatást."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 979136db-c92d-4288-870c-f305e8777bdd
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: elioda
ms.openlocfilehash: 2caefc4f59e16ad28b0d8cf4b3bb627b4cba803b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="device-to-cloud-communications-guidance"></a>Eszköz-felhő kommunikációs útmutató
Információk hello eszköz alkalmazás toohello megoldás háttérből küldésekor IoT-központ három beállítást tesz elérhetővé:

* [Eszköz a felhőbe küldött üzeneteket] [ lnk-d2c] idő adatsorozat telemetriai adatok és riasztások.
* [Tulajdonságok jelentett] [ lnk-twins] eszköz állapotadatokat például elérhető képességek, a feltételek vagy a hosszan futó munkafolyamatok hello szerinti jelentéskészítéshez. Például a konfigurációs és szoftverfrissítéseket.
* [A fájl feltöltések] [ lnk-fileupload] adathordozó nagy telemetriai kötegek időnként csatlakoztatott eszközök által feltöltött, illetve fájlok és tömörített toosave sávszélesség.

Ez hello részletes összehasonlítása különböző eszköz-felhő kommunikációs beállításait.

|  | Az eszközről a felhőbe irányuló üzenetek | Jelentett tulajdonságai | Fájlfeltöltések |
| ---- | ------- | ---------- | ---- |
| Forgatókönyv | Telemetria idősorok és riasztásokat. Például a 256 KB-os érzékelő adatkötegek 5 percenként lett küldve. | Elérhető képességek és a kikötéseket. Például hello aktuális eszköz csatlakozási mód például mobilhálózati vagy WiFi. Hosszan futó munkafolyamatok, például a konfigurációs és a szoftverfrissítések szinkronizálása. | Médiafájlok. Nagy (általában tömörített) telemetriai kötegek. |
| Tárolása és lekérése | Mindaddig ideiglenesen tárolja az IoT hubból too7 nap fel. Csak a Szekvenciális olvasási. | Az IoT-központ által tárolt hello eszköz iker. Lekérhető hello segítségével [IoT-központ lekérdezési nyelv][lnk-query]. | Felhasználó által megadott Azure Storage-fiókban tárolt. |
| Méret | Too256 KB-os üzenetek. | Jelentett tulajdonságok maximális mérete 8 KB-os. | Azure Blob Storage által támogatott maximális fájlméretet. |
| gyakoriság | Magas. További információkért lásd: [korlátozza az IoT-központ][lnk-quotas]. | Közepes. További információkért lásd: [korlátozza az IoT-központ][lnk-quotas]. | Alacsony. További információkért lásd: [korlátozza az IoT-központ][lnk-quotas]. |
| Protokoll | Minden protokoll érhető el. | Jelenleg csak akkor érhető el MQTT használatával. | Érhető el, ha bármely protokollon keresztül, de szükséges HTTP hello eszközön. |

Lehetséges, hogy egy alkalmazás megkövetel egy tooboth küldési információ a telemetriai adatokat a time series vagy riasztás és is toomake azt hello eszköz iker érhető el. Ebben a forgatókönyvben válassza az alábbi beállítások hello teheti meg:

* hello eszközalkalmazás eszközről a felhőbe üzenetet küld, és jelentések tulajdonság módosítását.
* hello megoldás háttérrendszerének hello adatokat tárolhat hello eszköz iker címkék hello üzenet fogadásakor.

Mivel az eszköz a felhőbe küldött üzeneteket egy sokkal nagyobb átviteli sebességgel mint eszköz iker frissítések engedélyezéséhez néha kívánatos tooavoid frissítési hello eszköz iker minden eszköz-felhő üzenet.


[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-fileupload]: iot-hub-devguide-file-upload.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
