---
title: "aaaGet Azure IoT peremhálózati (Linux) használatába |} Microsoft Docs"
description: "Hogyan toobuild egy átjáró egy Linux számítógép- és alapfogalmakat például modulok és a JSON-konfigurációs fájlokat az Azure IoT Edge megismerése."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: cf537bdd-2352-4bb1-96cd-a283fcd3d6cf
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/07/2017
ms.author: andbuc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 40aa9c8ddca6a974c361cbb0b453c7d0ddc71b8d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="explore-azure-iot-edge-architecture-on-linux"></a>Az Azure IoT Edge architektúrájának felfedezése Linux rendszeren

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-toorun-hello-sample"></a>Hogyan toorun hello minta

Hello **build.sh** parancsfájlt hoz létre a kimeneti hello **build** hello helyi példánya mappájában **iot-edge** tárházba. A kimeneti hello a mintában használt két IoT peremhálózati modulok tartalmazza.

build script helyek hello **liblogger.so** a hello **build/modulok/naplózó/** mappa és **libhello\_world.so** a hello **build / modulok/hello_world/** mappát. Elérési utak használata hello **modul elérési útján** értékeket a következő példa JSON beállítások fájl hello látható módon.

hello hello\_globális\_minta, amelynek során hello elérési tooa JSON-konfigurációs fájl parancssori argumentum. hello alábbi példa JSON-fájl valósul meg a következő SDK-tárházban hello **minták/hello\_globális/src/hello\_globális\_lin.json**. A konfigurációs fájl működik, kivéve, ha módosítja a hello build script tooplace hello IoT peremhálózati modulok, vagy végrehajtható fájlok alapértelmezettől eltérő helyeket a mintát.

> [!NOTE]
> hello modul elérési útvonalai relatív toohello honnan aktuális munkakönyvtárban hello hello\_globális\_minta végrehajtható indul el, nem hello könyvtárra hello végrehajtható. hello minta JSON konfigurációs fájl alapértelmezett toowriting "log.txt" az aktuális munkakönyvtárban a.

```json
{
    "modules" :
    [
        {
            "name" : "logger",
            "loader": {
            "name": "native",
            "entrypoint": {
                "module.path": "./modules/logger/liblogger.so"
            }
            },
            "args" : {"filename":"log.txt"}
        },
        {
            "name" : "hello_world",
            "loader": {
            "name": "native",
            "entrypoint": {
                "module.path": "./modules/hello_world/libhello_world.so"
            }
            },
            "args" : null
        }
    ],
    "links":
    [
        {
            "source": "hello_world",
            "sink": "logger"
        }
    ]
}
```

1. Keresse meg a toohello **build** hello helyi példánya hello gyökérmappájában mappájában **iot-edge** tárház.

1. Futtassa a következő parancs hello:

    ```sh
    ./samples/hello_world/hello_world_sample ../samples/hello_world/src/hello_world_lin.json`
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]

