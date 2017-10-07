---
title: "aaaGet Azure IoT peremhálózati (Windows) használatába |} Microsoft Docs"
description: "Hogyan toobuild egy, a Windows Azure IoT peremhálózati átjáró számítógéphez, és a modulok és a JSON-konfigurációs fájlok például az Azure IoT Edge főbb fogalmait megismerése."
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
ms.openlocfilehash: 5dd13cbfc02eeb55d9f2dbffca5021f2624acf14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="explore-azure-iot-edge-architecture-on-windows"></a>A Windows Azure IoT peremhálózati architektúra felfedezés

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="how-toorun-hello-sample"></a>Hogyan toorun hello minta

Hello **build.cmd** parancsfájlt hoz létre a kimeneti hello **build** hello helyi példánya mappájában **iot-edge** tárházba. A kimeneti hello a mintában használt két IoT peremhálózati modulok tartalmazza.

build script helyek hello **logger.dll** a hello **build\\modulok\\naplózó\\Debug** mappa és **hello\_world.dll**  a hello **build\\modulok\\hello_world\\Debug** mappát. Az elérési utak használata hello **modul elérési útján** látható módon hello beállításokat JSON-fájlt a következő értékeket.

hello hello\_globális\_minta tart hello elérési tooa JSON-konfigurációs fájl parancssori argumentumként. hello alábbi példa JSON-fájl valósul meg a következő SDK-tárházban hello **minták\\hello\_globális\\src\\hello\_globális\_win.json**. A konfigurációs fájl működik, kivéve, ha módosítja a hello build script tooplace hello IoT peremhálózati modulok, vagy végrehajtható fájlok alapértelmezettől eltérő helyeket a mintát.

> [!NOTE]
> hello modul elérési utak relatív toohello directory, ahol hello hello\_globális\_sample.exe helyezkedik el. hello minta JSON konfigurációs fájl alapértelmezett toowriting "log.txt" az aktuális munkakönyvtárban a.

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

1. Keresse meg a toohello **build** hello helyi példánya hello gyökérmappájában mappájában **iot-edge** tárház.

1. Futtassa a következő parancs hello:

    ```cmd
    samples\hello_world\Debug\hello_world_sample.exe ..\samples\hello_world\src\hello_world_win.json
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]
