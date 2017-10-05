---
title: "Ismerkedés az Azure IoT peremhálózati (Linux) |} Microsoft Docs"
description: "Útmutató átjáró létrehozásához linuxos számítógépen, és az Azure IoT Edge alapvető fogalmainak, például a moduloknak és a JSON konfigurációs fájloknak a bemutatása."
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
ms.openlocfilehash: b02d79fcd9cd2a2ef0041aac4e85528263c8d58a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="explore-azure-iot-edge-architecture-on-linux"></a>Az Azure IoT Edge architektúrájának felfedezése Linux rendszeren

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-to-run-the-sample"></a>A minta futtatása

A **build.sh** szkript a kimenetét a **build** mappában hozza létre, az **iot-edge** adattár helyi példányában. A kimenetet a mintában használt két IoT peremhálózati modulok tartalmazza.

A buildszkript a **liblogger.so** fájlt a **build/modules/logger/** mappába, a **libhello\_world.so** fájlt pedig a **build/modules/hello_world/** mappába menti. Az elérési utak használata a **modul elérési útján** értékei, ahogy az az alábbi példa JSON beállításfájl.

A hello\_world\_sample folyamat egy JSON-konfigurációs fájl elérési útját veszi fel argumentumként a parancssorban. Az alábbi JSON-példafájl az SDK-tárházban a **samples/hello\_world/src/hello\_world\_lin.json** helyen található. A konfigurációs fájl, kivéve, ha módosítja a build parancsfájlt helyezze el az IoT peremhálózati modulok vagy a minta végrehajtható fájlok alapértelmezettől eltérő helyeket működik.

> [!NOTE]
> A modul az aktuális munkakönyvtárhoz képest relatív útvonalakat használ. A munkamappa az, ahonnan a hello\_world\_sample végrehajtható fájlt elindította, és nem a végrehajtható fájl könyvtára. A JSON konfigurációs mintafájl a „log.txt” naplófájlokat alapértelmezetten az aktuális munkakönyvtárba írja.

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

1. Keresse meg a **build** mappa gyökérkönyvtárában található a helyi másolat készítése a **iot-edge** tárház.

1. Futtassa az alábbi parancsot:

    ```sh
    ./samples/hello_world/hello_world_sample ../samples/hello_world/src/hello_world_lin.json`
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]

