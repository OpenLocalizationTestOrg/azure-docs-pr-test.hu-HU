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
# <a name="explore-azure-iot-edge-architecture-on-windows"></a><span data-ttu-id="845ce-103">A Windows Azure IoT peremhálózati architektúra felfedezés</span><span class="sxs-lookup"><span data-stu-id="845ce-103">Explore Azure IoT Edge architecture on Windows</span></span>

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="how-toorun-hello-sample"></a><span data-ttu-id="845ce-104">Hogyan toorun hello minta</span><span class="sxs-lookup"><span data-stu-id="845ce-104">How toorun hello sample</span></span>

<span data-ttu-id="845ce-105">Hello **build.cmd** parancsfájlt hoz létre a kimeneti hello **build** hello helyi példánya mappájában **iot-edge** tárházba.</span><span class="sxs-lookup"><span data-stu-id="845ce-105">hello **build.cmd** script generates its output in hello **build** folder in your local copy of hello **iot-edge** repository.</span></span> <span data-ttu-id="845ce-106">A kimeneti hello a mintában használt két IoT peremhálózati modulok tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="845ce-106">This output includes hello two IoT Edge modules used in this sample.</span></span>

<span data-ttu-id="845ce-107">build script helyek hello **logger.dll** a hello **build\\modulok\\naplózó\\Debug** mappa és **hello\_world.dll**  a hello **build\\modulok\\hello_world\\Debug** mappát.</span><span class="sxs-lookup"><span data-stu-id="845ce-107">hello build script places **logger.dll** in hello **build\\modules\\logger\\Debug** folder and **hello\_world.dll** in hello **build\\modules\\hello_world\\Debug** folder.</span></span> <span data-ttu-id="845ce-108">Az elérési utak használata hello **modul elérési útján** látható módon hello beállításokat JSON-fájlt a következő értékeket.</span><span class="sxs-lookup"><span data-stu-id="845ce-108">Use these paths for hello **module path** values as shown in hello following JSON settings file.</span></span>

<span data-ttu-id="845ce-109">hello hello\_globális\_minta tart hello elérési tooa JSON-konfigurációs fájl parancssori argumentumként.</span><span class="sxs-lookup"><span data-stu-id="845ce-109">hello hello\_world\_sample process takes hello path tooa JSON configuration file as a command-line argument.</span></span> <span data-ttu-id="845ce-110">hello alábbi példa JSON-fájl valósul meg a következő SDK-tárházban hello **minták\\hello\_globális\\src\\hello\_globális\_win.json**.</span><span class="sxs-lookup"><span data-stu-id="845ce-110">hello following example JSON file is provided in hello SDK repository at **samples\\hello\_world\\src\\hello\_world\_win.json**.</span></span> <span data-ttu-id="845ce-111">A konfigurációs fájl működik, kivéve, ha módosítja a hello build script tooplace hello IoT peremhálózati modulok, vagy végrehajtható fájlok alapértelmezettől eltérő helyeket a mintát.</span><span class="sxs-lookup"><span data-stu-id="845ce-111">This configuration file works as is unless you modify hello build script tooplace hello IoT Edge modules or sample executables in non-default locations.</span></span>

> [!NOTE]
> <span data-ttu-id="845ce-112">hello modul elérési utak relatív toohello directory, ahol hello hello\_globális\_sample.exe helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="845ce-112">hello module paths are relative toohello directory where hello hello\_world\_sample.exe is located.</span></span> <span data-ttu-id="845ce-113">hello minta JSON konfigurációs fájl alapértelmezett toowriting "log.txt" az aktuális munkakönyvtárban a.</span><span class="sxs-lookup"><span data-stu-id="845ce-113">hello sample JSON configuration file defaults toowriting 'log.txt' in your current working directory.</span></span>

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

1. <span data-ttu-id="845ce-114">Keresse meg a toohello **build** hello helyi példánya hello gyökérmappájában mappájában **iot-edge** tárház.</span><span class="sxs-lookup"><span data-stu-id="845ce-114">Navigate toohello **build** folder in hello root of your local copy of hello **iot-edge** repository.</span></span>

1. <span data-ttu-id="845ce-115">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="845ce-115">Run hello following command:</span></span>

    ```cmd
    samples\hello_world\Debug\hello_world_sample.exe ..\samples\hello_world\src\hello_world_win.json
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]
