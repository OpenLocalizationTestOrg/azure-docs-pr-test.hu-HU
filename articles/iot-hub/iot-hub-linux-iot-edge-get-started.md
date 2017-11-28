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
# <a name="explore-azure-iot-edge-architecture-on-linux"></a><span data-ttu-id="561c3-103">Az Azure IoT Edge architektúrájának felfedezése Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="561c3-103">Explore Azure IoT Edge architecture on Linux</span></span>

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-linux](../../includes/iot-hub-iot-edge-install-build-linux.md)]

## <a name="how-to-run-the-sample"></a><span data-ttu-id="561c3-104">A minta futtatása</span><span class="sxs-lookup"><span data-stu-id="561c3-104">How to run the sample</span></span>

<span data-ttu-id="561c3-105">A **build.sh** szkript a kimenetét a **build** mappában hozza létre, az **iot-edge** adattár helyi példányában.</span><span class="sxs-lookup"><span data-stu-id="561c3-105">The **build.sh** script generates its output in the **build** folder in your local copy of the **iot-edge** repository.</span></span> <span data-ttu-id="561c3-106">A kimenetet a mintában használt két IoT peremhálózati modulok tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="561c3-106">This output includes the two IoT Edge modules used in this sample.</span></span>

<span data-ttu-id="561c3-107">A buildszkript a **liblogger.so** fájlt a **build/modules/logger/** mappába, a **libhello\_world.so** fájlt pedig a **build/modules/hello_world/** mappába menti.</span><span class="sxs-lookup"><span data-stu-id="561c3-107">The build script places **liblogger.so** in the **build/modules/logger/** folder and **libhello\_world.so** in the **build/modules/hello_world/** folder.</span></span> <span data-ttu-id="561c3-108">Az elérési utak használata a **modul elérési útján** értékei, ahogy az az alábbi példa JSON beállításfájl.</span><span class="sxs-lookup"><span data-stu-id="561c3-108">Use these paths for the **module path** values as shown in the following example JSON settings file.</span></span>

<span data-ttu-id="561c3-109">A hello\_world\_sample folyamat egy JSON-konfigurációs fájl elérési útját veszi fel argumentumként a parancssorban.</span><span class="sxs-lookup"><span data-stu-id="561c3-109">The hello\_world\_sample process takes the path to a JSON configuration file a command-line argument.</span></span> <span data-ttu-id="561c3-110">Az alábbi JSON-példafájl az SDK-tárházban a **samples/hello\_world/src/hello\_world\_lin.json** helyen található.</span><span class="sxs-lookup"><span data-stu-id="561c3-110">The following example JSON file is provided in the SDK repository at **samples/hello\_world/src/hello\_world\_lin.json**.</span></span> <span data-ttu-id="561c3-111">A konfigurációs fájl, kivéve, ha módosítja a build parancsfájlt helyezze el az IoT peremhálózati modulok vagy a minta végrehajtható fájlok alapértelmezettől eltérő helyeket működik.</span><span class="sxs-lookup"><span data-stu-id="561c3-111">This configuration file works as is unless you modify the build script to place the IoT Edge modules or sample executables in non-default locations.</span></span>

> [!NOTE]
> <span data-ttu-id="561c3-112">A modul az aktuális munkakönyvtárhoz képest relatív útvonalakat használ. A munkamappa az, ahonnan a hello\_world\_sample végrehajtható fájlt elindította, és nem a végrehajtható fájl könyvtára.</span><span class="sxs-lookup"><span data-stu-id="561c3-112">The module paths are relative to the current working directory from where the hello\_world\_sample executable is launched, not the directory where the executable is located.</span></span> <span data-ttu-id="561c3-113">A JSON konfigurációs mintafájl a „log.txt” naplófájlokat alapértelmezetten az aktuális munkakönyvtárba írja.</span><span class="sxs-lookup"><span data-stu-id="561c3-113">The sample JSON configuration file defaults to writing 'log.txt' in your current working directory.</span></span>

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

1. <span data-ttu-id="561c3-114">Keresse meg a **build** mappa gyökérkönyvtárában található a helyi másolat készítése a **iot-edge** tárház.</span><span class="sxs-lookup"><span data-stu-id="561c3-114">Navigate to the **build** folder in the root of your local copy of the **iot-edge** repository.</span></span>

1. <span data-ttu-id="561c3-115">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="561c3-115">Run the following command:</span></span>

    ```sh
    ./samples/hello_world/hello_world_sample ../samples/hello_world/src/hello_world_lin.json`
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]

