---
title: "aaaUse hello Azure portál tooconfigure fájlfeltöltés |} Microsoft Docs"
description: "Hogyan toouse hello Azure portál tooconfigure a IoT hub tooenable fájlfeltöltések csatlakoztatott eszközökről. Hello cél Azure storage-fiók konfigurálásával kapcsolatos információkat tartalmazza."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 915f1597-272d-4fd4-8c5b-a0ccb1df0d91
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/03/2017
ms.author: dobett
ms.openlocfilehash: b90c3fbed47b4eb144d3cb7480068b7cfc776ba6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-iot-hub-file-uploads-using-hello-azure-portal"></a>Konfigurálja az IoT-központ fájlfeltöltéseket hello Azure-portál használatával

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

## <a name="file-upload"></a>Fájl feltöltése

toouse hello [feltöltés funkció fájlt az IoT-központ][lnk-upload], először társítania kell egy Azure Storage-fiókot a központ. Válassza ki **fájlfeltöltés** toodisplay a fájl feltöltése tulajdonságlistát hello IoT hub, amely módosítás alatt áll.

![Az IoT-központ fájlfeltöltés hello portál beállítások megtekintése][13]

**A tároló**: hello Azure portál tooselect egy blob tároló, az a jelenlegi Azure-előfizetés tooassociate az IoT hubbal egy Azure Storage-fiókot használja. Ha szükséges, létrehozhat egy Azure Storage-fiók hello **tárfiókok** panel megnyitásához, és a blob tároló a hello **tárolók** panelen. Ezek a fájlok feltöltése az IoT-központ automatikusan létrehozza SAS URI-azonosítók az írási engedélyek toothis blob tároló eszközök toouse.

![Tekintse meg a fájl feltöltése a tároló hello portál][14]

**A feltöltött fájlok értesítéseket**: engedélyezheti vagy tilthatja le a fájl feltöltése értesítések hello váltása keresztül.

**SAS-élettartam**: Ez a beállítás akkor idő élettartamát a SAS URI-azonosítók toohello eszköz vissza az IoT-központ hello hello. Alapértelmezés szerint beállított tooone óra, de is testre szabott tooother értékeket használja hello csúszkát.

**Az értesítési beállítások alapértelmezett élettartam**: hello idő TTL fájl feltöltése értesítés előtt lejárt. Alapértelmezés szerint beállított tooone nap, de is testre szabott tooother értékeket használja hello csúszkát.

**Értesítési maximális száma fájl**: hello számú alkalommal fordult elő az IoT-központ kísérletek toodeliver fájl feltöltése értesítést hello. Alapértelmezés szerint beállított too10, de is testre szabott tooother értékeket használja hello csúszkát.

![IoT Hub-fájl feltöltése hello portál konfigurálása][15]

## <a name="next-steps"></a>Következő lépések

Az IoT-központ hello fájl feltöltése képességeivel kapcsolatos további információkért lásd: [egy eszközről tölt fel] [ lnk-upload] az IoT Hub fejlesztői útmutató hello.

Kövesse az alábbi hivatkozások toolearn Azure IoT Hub kezelésével kapcsolatos további:

* [Tömeges az IoT-eszközök kezelése][lnk-bulk]
* [Az IoT-központ metrikák][lnk-metrics]
* [Figyelési műveletek][lnk-monitor]

toofurther megismerkedhet az IoT-központ hello képességeit, lásd:

* [IoT Hub fejlesztői útmutató][lnk-devguide]
* [Egy eszköz szimulálva IoT oldala][lnk-iotedge]
* [Az IoT-megoldásból hello szabad a biztonságos][lnk-securing]

[13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
[14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
[15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md
