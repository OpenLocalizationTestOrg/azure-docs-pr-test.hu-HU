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
# <a name="explore-azure-iot-edge-architecture-on-linux"></a><span data-ttu-id="5df96-103">Az Azure IoT Edge architektúrájának felfedezése Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="5df96-103">Explore Azure IoT Edge architecture on Linux</span></span>

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-toorun-hello-sample"></a><span data-ttu-id="5df96-104">Hogyan toorun hello minta</span><span class="sxs-lookup"><span data-stu-id="5df96-104">How toorun hello sample</span></span>

<span data-ttu-id="5df96-105">Hello **build.sh** parancsfájlt hoz létre a kimeneti hello **build** hello helyi példánya mappájában **iot-edge** tárházba.</span><span class="sxs-lookup"><span data-stu-id="5df96-105">hello **build.sh** script generates its output in hello **build** folder in your local copy of hello **iot-edge** repository.</span></span> <span data-ttu-id="5df96-106">A kimeneti hello a mintában használt két IoT peremhálózati modulok tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5df96-106">This output includes hello two IoT Edge modules used in this sample.</span></span>

<span data-ttu-id="5df96-107">build script helyek hello **liblogger.so** a hello **build/modulok/naplózó/** mappa és **libhello\_world.so** a hello **build / modulok/hello_world/** mappát.</span><span class="sxs-lookup"><span data-stu-id="5df96-107">hello build script places **liblogger.so** in hello **build/modules/logger/** folder and **libhello\_world.so** in hello **build/modules/hello_world/** folder.</span></span> <span data-ttu-id="5df96-108">Elérési utak használata hello **modul elérési útján** értékeket a következő példa JSON beállítások fájl hello látható módon.</span><span class="sxs-lookup"><span data-stu-id="5df96-108">Use these paths for hello **module path** values as shown in hello following example JSON settings file.</span></span>

<span data-ttu-id="5df96-109">hello hello\_globális\_minta, amelynek során hello elérési tooa JSON-konfigurációs fájl parancssori argumentum.</span><span class="sxs-lookup"><span data-stu-id="5df96-109">hello hello\_world\_sample process takes hello path tooa JSON configuration file a command-line argument.</span></span> <span data-ttu-id="5df96-110">hello alábbi példa JSON-fájl valósul meg a következő SDK-tárházban hello **minták/hello\_globális/src/hello\_globális\_lin.json**.</span><span class="sxs-lookup"><span data-stu-id="5df96-110">hello following example JSON file is provided in hello SDK repository at **samples/hello\_world/src/hello\_world\_lin.json**.</span></span> <span data-ttu-id="5df96-111">A konfigurációs fájl működik, kivéve, ha módosítja a hello build script tooplace hello IoT peremhálózati modulok, vagy végrehajtható fájlok alapértelmezettől eltérő helyeket a mintát.</span><span class="sxs-lookup"><span data-stu-id="5df96-111">This configuration file works as is unless you modify hello build script tooplace hello IoT Edge modules or sample executables in non-default locations.</span></span>

> [!NOTE]
> <span data-ttu-id="5df96-112">hello modul elérési útvonalai relatív toohello honnan aktuális munkakönyvtárban hello hello\_globális\_minta végrehajtható indul el, nem hello könyvtárra hello végrehajtható.</span><span class="sxs-lookup"><span data-stu-id="5df96-112">hello module paths are relative toohello current working directory from where hello hello\_world\_sample executable is launched, not hello directory where hello executable is located.</span></span> <span data-ttu-id="5df96-113">hello minta JSON konfigurációs fájl alapértelmezett toowriting "log.txt" az aktuális munkakönyvtárban a.</span><span class="sxs-lookup"><span data-stu-id="5df96-113">hello sample JSON configuration file defaults toowriting 'log.txt' in your current working directory.</span></span>

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

1. <span data-ttu-id="5df96-114">Keresse meg a toohello **build** hello helyi példánya hello gyökérmappájában mappájában **iot-edge** tárház.</span><span class="sxs-lookup"><span data-stu-id="5df96-114">Navigate toohello **build** folder in hello root of your local copy of hello **iot-edge** repository.</span></span>

1. <span data-ttu-id="5df96-115">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="5df96-115">Run hello following command:</span></span>

    ```sh
    ./samples/hello_world/hello_world_sample ../samples/hello_world/src/hello_world_lin.json`
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]

