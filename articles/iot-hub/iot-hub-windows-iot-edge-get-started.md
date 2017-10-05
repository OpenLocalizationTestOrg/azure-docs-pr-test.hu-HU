---
title: "Ismerkedés az Azure IoT peremhálózati (Windows) |} Microsoft Docs"
description: "Megtudhatja, hogyan lehet létrehozni az Azure IoT peremhálózati átjáró Windows-gépen, és alapfogalmakat például modulok és a JSON-konfigurációs fájlokat az Azure IoT Edge megismerése."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 9aff3724-5e4e-40ec-b95a-d00df4f590c5
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/07/2017
ms.author: andbuc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5db39bab8e31a8e7026b34e72b4614b0f6f57772
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="explore-azure-iot-edge-architecture-on-windows"></a>A Windows Azure IoT peremhálózati architektúra felfedezés

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="how-to-run-the-sample"></a>A minta futtatása

A **build.cmd** parancsfájlt hoz létre a kimenetét a **build** mappa a helyi példánya a **iot-edge** tárház. A kimenetet a mintában használt két IoT peremhálózati modulok tartalmazza.

A build script helyek **logger.dll** a a **build\\modulok\\naplózó\\Debug** mappa és **hello\_world.dll** a a **build\\modulok\\hello_world\\Debug** mappát. Az elérési utak használata a **modul elérési útján** értékei, ahogy az alábbi JSON fájlban.

A hello\_globális\_minta tart az elérési út egy JSON-konfigurációs fájl parancssori argumentumként. A következő példa JSON-fájl megtalálható a következő SDK tárház **minták\\hello\_globális\\src\\hello\_globális\_win.json**. A konfigurációs fájl, kivéve, ha módosítja a build parancsfájlt helyezze el az IoT peremhálózati modulok vagy a minta végrehajtható fájlok alapértelmezettől eltérő helyeket működik.

> [!NOTE]
> A modul elérési útvonalai képest a könyvtár ahol a hello\_globális\_sample.exe helyezkedik el. A JSON konfigurációs mintafájl a „log.txt” naplófájlokat alapértelmezetten az aktuális munkakönyvtárba írja.

```json
{
  "modules": [
    {
      "name": "logger",
      "loader": {
        "name": "native",
        "entrypoint": {
          "module.path": "..\\..\\..\\modules\\logger\\Debug\\logger.dll"
        }
      },
      "args": { "filename": "log.txt" }
    },
    {
      "name": "hello_world",
      "loader": {
        "name": "native",
        "entrypoint": {
          "module.path": "..\\..\\..\\modules\\hello_world\\Debug\\hello_world.dll"
        }
      },
      "args": null
      }
  ],
  "links": [
    {
      "source": "hello_world",
      "sink": "logger"
    }
  ]
}
```

1. Keresse meg a **build** mappa gyökérkönyvtárában található a helyi másolat készítése a **iot-edge** tárház.

1. Futtassa az alábbi parancsot:

    ```cmd
    samples\hello_world\Debug\hello_world_sample.exe ..\samples\hello_world\src\hello_world_win.json
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]
