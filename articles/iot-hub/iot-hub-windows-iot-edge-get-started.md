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
# <a name="explore-azure-iot-edge-architecture-on-windows"></a><span data-ttu-id="1b1df-103">A Windows Azure IoT peremhálózati architektúra felfedezés</span><span class="sxs-lookup"><span data-stu-id="1b1df-103">Explore Azure IoT Edge architecture on Windows</span></span>

[!INCLUDE [iot-hub-iot-edge-getstarted-selector](../../includes/iot-hub-iot-edge-getstarted-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="how-to-run-the-sample"></a><span data-ttu-id="1b1df-104">A minta futtatása</span><span class="sxs-lookup"><span data-stu-id="1b1df-104">How to run the sample</span></span>

<span data-ttu-id="1b1df-105">A **build.cmd** parancsfájlt hoz létre a kimenetét a **build** mappa a helyi példánya a **iot-edge** tárház.</span><span class="sxs-lookup"><span data-stu-id="1b1df-105">The **build.cmd** script generates its output in the **build** folder in your local copy of the **iot-edge** repository.</span></span> <span data-ttu-id="1b1df-106">A kimenetet a mintában használt két IoT peremhálózati modulok tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="1b1df-106">This output includes the two IoT Edge modules used in this sample.</span></span>

<span data-ttu-id="1b1df-107">A build script helyek **logger.dll** a a **build\\modulok\\naplózó\\Debug** mappa és **hello\_world.dll** a a **build\\modulok\\hello_world\\Debug** mappát.</span><span class="sxs-lookup"><span data-stu-id="1b1df-107">The build script places **logger.dll** in the **build\\modules\\logger\\Debug** folder and **hello\_world.dll** in the **build\\modules\\hello_world\\Debug** folder.</span></span> <span data-ttu-id="1b1df-108">Az elérési utak használata a **modul elérési útján** értékei, ahogy az alábbi JSON fájlban.</span><span class="sxs-lookup"><span data-stu-id="1b1df-108">Use these paths for the **module path** values as shown in the following JSON settings file.</span></span>

<span data-ttu-id="1b1df-109">A hello\_globális\_minta tart az elérési út egy JSON-konfigurációs fájl parancssori argumentumként.</span><span class="sxs-lookup"><span data-stu-id="1b1df-109">The hello\_world\_sample process takes the path to a JSON configuration file as a command-line argument.</span></span> <span data-ttu-id="1b1df-110">A következő példa JSON-fájl megtalálható a következő SDK tárház **minták\\hello\_globális\\src\\hello\_globális\_win.json**.</span><span class="sxs-lookup"><span data-stu-id="1b1df-110">The following example JSON file is provided in the SDK repository at **samples\\hello\_world\\src\\hello\_world\_win.json**.</span></span> <span data-ttu-id="1b1df-111">A konfigurációs fájl, kivéve, ha módosítja a build parancsfájlt helyezze el az IoT peremhálózati modulok vagy a minta végrehajtható fájlok alapértelmezettől eltérő helyeket működik.</span><span class="sxs-lookup"><span data-stu-id="1b1df-111">This configuration file works as is unless you modify the build script to place the IoT Edge modules or sample executables in non-default locations.</span></span>

> [!NOTE]
> <span data-ttu-id="1b1df-112">A modul elérési útvonalai képest a könyvtár ahol a hello\_globális\_sample.exe helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="1b1df-112">The module paths are relative to the directory where the hello\_world\_sample.exe is located.</span></span> <span data-ttu-id="1b1df-113">A JSON konfigurációs mintafájl a „log.txt” naplófájlokat alapértelmezetten az aktuális munkakönyvtárba írja.</span><span class="sxs-lookup"><span data-stu-id="1b1df-113">The sample JSON configuration file defaults to writing 'log.txt' in your current working directory.</span></span>

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

1. <span data-ttu-id="1b1df-114">Keresse meg a **build** mappa gyökérkönyvtárában található a helyi másolat készítése a **iot-edge** tárház.</span><span class="sxs-lookup"><span data-stu-id="1b1df-114">Navigate to the **build** folder in the root of your local copy of the **iot-edge** repository.</span></span>

1. <span data-ttu-id="1b1df-115">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="1b1df-115">Run the following command:</span></span>

    ```cmd
    samples\hello_world\Debug\hello_world_sample.exe ..\samples\hello_world\src\hello_world_win.json
    ```

[!INCLUDE [iot-hub-iot-edge-getstarted-code](../../includes/iot-hub-iot-edge-getstarted-code.md)]
