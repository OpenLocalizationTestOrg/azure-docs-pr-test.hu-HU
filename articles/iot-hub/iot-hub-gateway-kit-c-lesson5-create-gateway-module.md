---
title: "Az első Azure IoT átjáró modul létrehozása |} Microsoft Docs"
description: "Hozzon létre egy modult, és adja hozzá egy mintaalkalmazást modul viselkedés testreszabásához."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: 
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-gateway-kit-c-lesson1-set-up-nuc
ms.assetid: cd7660f4-7b8b-4091-8d71-bb8723165b0b
ms.service: iot-hub
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 5e28422158684c3aaf0ac3fdf5b19c80fbccfb02
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="lesson-5-create-your-first-azure-iot-gateway-module"></a><span data-ttu-id="64b09-103">5. lecke: Az első Azure IoT átjáró modul létrehozása</span><span class="sxs-lookup"><span data-stu-id="64b09-103">Lesson 5: Create your first Azure IoT Gateway module</span></span>
<span data-ttu-id="64b09-104">Azure IoT peremhálózati build Java, a .NET vagy Node.js nyelven írt modulok teszi lehetővé, amíg ez az oktatóanyag végigvezeti a készítéséhez egy modult a c kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="64b09-104">While Azure IoT Edge allows you to build modules written in Java, .NET, or Node.js, this tutorial walks you through the steps for building a module in C.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="64b09-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="64b09-105">What you will do</span></span>

- <span data-ttu-id="64b09-106">Fordítsa le, és az Intel NUC hello_world mintaalkalmazás futtatása.</span><span class="sxs-lookup"><span data-stu-id="64b09-106">Compile and run the hello_world sample app on Intel NUC.</span></span>
- <span data-ttu-id="64b09-107">Hozzon létre egy modult, és az Intel NUC lefordítani.</span><span class="sxs-lookup"><span data-stu-id="64b09-107">Create a module and compile it on Intel NUC.</span></span>
- <span data-ttu-id="64b09-108">Az új modul felvétele a hello_world mintaalkalmazást, és futtassa a minta Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="64b09-108">Add the new module to the hello_world sample app and then run the sample on Intel NUC.</span></span> <span data-ttu-id="64b09-109">Új modul megjeleníti az időbélyegzőnek "hello_world" üzenetek.</span><span class="sxs-lookup"><span data-stu-id="64b09-109">The new module prints out "hello_world" messages with a timestamp.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="64b09-110">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="64b09-110">What you will learn</span></span>

- <span data-ttu-id="64b09-111">Fordítsa le, és futtassa a mintaalkalmazást Intel NUC módjáról.</span><span class="sxs-lookup"><span data-stu-id="64b09-111">How to compile and run a sample app on Intel NUC.</span></span>
- <span data-ttu-id="64b09-112">Hogyan hozható létre modul.</span><span class="sxs-lookup"><span data-stu-id="64b09-112">How to create a module.</span></span>
- <span data-ttu-id="64b09-113">Hogyan modul hozzáadása egy mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="64b09-113">How to add module to a sample app.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="64b09-114">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="64b09-114">What you need</span></span>

<span data-ttu-id="64b09-115">Az Azure IoT-Edge számítógépen telepítve van.</span><span class="sxs-lookup"><span data-stu-id="64b09-115">Azure IoT Edge that has been installed on your host computer.</span></span>

## <a name="folder-structure"></a><span data-ttu-id="64b09-116">Mappaszerkezet</span><span class="sxs-lookup"><span data-stu-id="64b09-116">Folder structure</span></span>

<span data-ttu-id="64b09-117">Az 1 lecke klónozott példakód lecke 5 almappája van egy `module` mappa és a `sample` mappa.</span><span class="sxs-lookup"><span data-stu-id="64b09-117">In the Lesson 5 subfolder of the sample code which you cloned in lesson 1, there is a `module` folder and a `sample` folder.</span></span>

![my_module](media/iot-hub-gateway-kit-lessons/lesson5/my_module.png)

- <span data-ttu-id="64b09-119">A `module/my_module` mappa tartalmazza a forráskódot, és a parancsprogramot hozni a modult.</span><span class="sxs-lookup"><span data-stu-id="64b09-119">The `module/my_module` folder contains the source code and script to build the module.</span></span>
- <span data-ttu-id="64b09-120">A `sample` mappa tartalmazza a Forráskód és a parancsprogramot a minta alkalmazás elkészítésére.</span><span class="sxs-lookup"><span data-stu-id="64b09-120">The `sample` folder contains the source code and script to build the sample app.</span></span>

## <a name="compile-and-run-the-helloworld-sample-app-on-intel-nuc"></a><span data-ttu-id="64b09-121">Fordítsa le, és az Intel NUC hello_world mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="64b09-121">Compile and run the hello_world sample app on Intel NUC</span></span>

<span data-ttu-id="64b09-122">A `hello_world` minta létrehoz egy átjáró alapján a `hello_world.json` fájl, amely azt adja meg az alkalmazással társított alkalmazások két előre meghatározott modulok.</span><span class="sxs-lookup"><span data-stu-id="64b09-122">The `hello_world` sample creates a gateway based on the `hello_world.json` file which specifies the two predefined modules associated with the app.</span></span> <span data-ttu-id="64b09-123">Az átjáró a "hello world" üzenet-fájlban naplózza minden 5 másodperc.</span><span class="sxs-lookup"><span data-stu-id="64b09-123">The gateway logs a "hello world" message to a file every 5 seconds.</span></span> <span data-ttu-id="64b09-124">Ebben a szakaszban fordítása, és futtassa a `hello_world` alkalmazás az alapértelmezett modullal.</span><span class="sxs-lookup"><span data-stu-id="64b09-124">In this section, you compile and run the `hello_world` app with its default module.</span></span>

<span data-ttu-id="64b09-125">Fordításához és futtatásához a `hello_world` alkalmazást, és a gazdagép-számítógépen kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="64b09-125">To compile and run the `hello_world` app, follow these steps on your host computer:</span></span>

1. <span data-ttu-id="64b09-126">A konfigurációs fájlok inicializálása a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="64b09-126">Initialize the configuration files by running the following commands:</span></span>

   ```bash
   cd iot-hub-c-intel-nuc-gateway-getting-started
   cd Lesson5
   npm install
   gulp init
   ```

1. <span data-ttu-id="64b09-127">Az átjáró konfigurációs fájljának frissítése Intel NUC a MAC-címmel.</span><span class="sxs-lookup"><span data-stu-id="64b09-127">Update the gateway configuration file with the MAC address of Intel NUC.</span></span> <span data-ttu-id="64b09-128">Kihagyhatja ezt a lépést, ha a lecke lezajlott [konfigurálására és futtatására BLA mintaalkalmazás][config_ble].</span><span class="sxs-lookup"><span data-stu-id="64b09-128">Skip this step if you have gone through the lesson to [configure and run a BLE sample application][config_ble].</span></span>

   1. <span data-ttu-id="64b09-129">Nyissa meg az átjáró konfigurációs fájlját a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="64b09-129">Open the gateway configuration file by running the following command:</span></span>

      ```bash
      # For Windows command prompt
      code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json

      # For MacOS or Ubuntu
      code ~/.iot-hub-getting-started/config-gateway.json
      ```

   1. <span data-ttu-id="64b09-130">Ha az átjáró MAC-címe frissítés meg [Intel NUC IoT átjáróként beállítva][setup_nuc], és mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="64b09-130">Update the gateway's MAC address when you [set up Intel NUC as a IoT gateway][setup_nuc], and then save the file.</span></span>

1. <span data-ttu-id="64b09-131">A minta forráskód összeállítása a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="64b09-131">Compile the sample source code by running the following command:</span></span>

   ```bash
   gulp compile
   ```

   <span data-ttu-id="64b09-132">A parancs Intel NUC adja át a minta forráskódot, és futtatja `build.sh` , hogy.</span><span class="sxs-lookup"><span data-stu-id="64b09-132">The command transfers the sample source code to Intel NUC and runs `build.sh` to compile it.</span></span>

1. <span data-ttu-id="64b09-133">Futtassa a `hello_world` Intel NUC alkalmazást a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="64b09-133">Run the `hello_world` app on Intel NUC by running the following command:</span></span>

   ```bash
   gulp run
   ```

   <span data-ttu-id="64b09-134">A parancs végrehajtása `../Tools/run-hello-world.js` meghatározott `config.json` elindítja a mintaalkalmazás Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="64b09-134">The command runs `../Tools/run-hello-world.js` that is specified in `config.json` to start the sample app on Intel NUC.</span></span>

   ![run_sample](media/iot-hub-gateway-kit-lessons/lesson5/run_sample.png)

## <a name="create-a-new-module-and-compile-it-on-intel-nuc"></a><span data-ttu-id="64b09-136">Hozzon létre egy új modult, és az Intel NUC lefordítani</span><span class="sxs-lookup"><span data-stu-id="64b09-136">Create a new module and compile it on Intel NUC</span></span>

<span data-ttu-id="64b09-137">Az alábbi lépéseket végigvezetik egy új modul létrehozása, és az Intel NUC lefordítani.</span><span class="sxs-lookup"><span data-stu-id="64b09-137">The steps below walk you through creating a new module and compile it on Intel NUC.</span></span> <span data-ttu-id="64b09-138">A modul kinyomtatja azokat az üzeneteket az időbélyegzőnek fogadásakor őket.</span><span class="sxs-lookup"><span data-stu-id="64b09-138">The module prints out messages with a timestamp upon receiving them.</span></span> <span data-ttu-id="64b09-139">Ebben a szakaszban az első testreszabott átjáró modul hoz létre.</span><span class="sxs-lookup"><span data-stu-id="64b09-139">You will create your first customized gateway module in this section.</span></span>

<span data-ttu-id="64b09-140">Bármely Azure IoT peremhálózati modul implementálnia kell a következő kapcsolódási pontok:</span><span class="sxs-lookup"><span data-stu-id="64b09-140">Any Azure IoT Edge module must implement the following interfaces:</span></span>

   ```C
   pfModule_ParseConfigurationFromJson Module_ParseConfigurationFromJson
   pfModule_FreeConfiguration Module_FreeConfiguration
   pfModule_Create Module_Create
   pfModule_Destroy Module_Destroy
   pfModule_Receive Module_Receive
   ```

<span data-ttu-id="64b09-141">Ha szükséges a következő kapcsolat valósíthatja meg:</span><span class="sxs-lookup"><span data-stu-id="64b09-141">You can optionally implement the following interface:</span></span>

   ```C
   pfModule_Start Module_Start
   ```

<span data-ttu-id="64b09-142">Az alábbi ábrán látható, a fontos állapot elérési utak a modulok.</span><span class="sxs-lookup"><span data-stu-id="64b09-142">The following diagram shows the important state paths of a module.</span></span> <span data-ttu-id="64b09-143">A négyzet téglalapok képviselő módszerek megvalósítása műveletek végrehajtásához, ha a modul állapotokat közé helyezi.</span><span class="sxs-lookup"><span data-stu-id="64b09-143">The square rectangles represent methods you implement to perform operations when the module moves between states.</span></span> <span data-ttu-id="64b09-144">A ovális lehet, hogy a modul a fő állapotok.</span><span class="sxs-lookup"><span data-stu-id="64b09-144">The ovals are major states the module can be in.</span></span>

![state_path](media/iot-hub-gateway-kit-lessons/lesson5/state_path.png)

<span data-ttu-id="64b09-146">Most hozzon létre egy modult a sablon alapján:</span><span class="sxs-lookup"><span data-stu-id="64b09-146">Now let’s create a module based on the template:</span></span>

1. <span data-ttu-id="64b09-147">Nyissa meg a sablon mappát a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="64b09-147">Open the template folder by running the following command:</span></span>

   ```bash
   code module/my_module
   ```

   ![code_module](media/iot-hub-gateway-kit-lessons/lesson5/code_module.png)

   - <span data-ttu-id="64b09-149">`src/my_module.c`a sablont, amely elősegíti a modul létrehozása funkcionál.</span><span class="sxs-lookup"><span data-stu-id="64b09-149">`src/my_module.c` serves as a template that facilitates the creation of a module.</span></span> <span data-ttu-id="64b09-150">A sablon a felületek deklarálja.</span><span class="sxs-lookup"><span data-stu-id="64b09-150">The template declares the interfaces.</span></span> <span data-ttu-id="64b09-151">Mindössze annyit kell tennie az logika hozzáadása a `MyModule_Receive` függvény.</span><span class="sxs-lookup"><span data-stu-id="64b09-151">All you need to do is to add logic to the `MyModule_Receive` function.</span></span>
   - <span data-ttu-id="64b09-152">`build.sh`a build parancsfájl Intel NUC moduljánál fordítása van.</span><span class="sxs-lookup"><span data-stu-id="64b09-152">`build.sh` is the build script to compile the module on Intel NUC.</span></span>
1. <span data-ttu-id="64b09-153">Nyissa meg a `src/my_module.c` fájlt, és két fejlécfájlok tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="64b09-153">Open the `src/my_module.c` file and include two header files:</span></span>

   ```C
   #include <stdio.h>
   #include "azure_c_shared_utility/xlogging.h"
   ```

1. <span data-ttu-id="64b09-154">Adja hozzá a következő kódot a `MyModule_Receive` függvény:</span><span class="sxs-lookup"><span data-stu-id="64b09-154">Add the following code to the `MyModule_Receive` function:</span></span>

   ```C
   if (message == NULL)
   {
      LogError("invalid arg message");
   }
   else
   {
      // get the message content
      const CONSTBUFFER * content = Message_GetContent(message);
      // get the local time and format it
      time_t temp = time(NULL);
      if (temp == (time_t)-1)
      {
          LogError("time function failed");
      }
      else
      {
          struct tm* t = localtime(&temp);
          if (t == NULL)
          {
              LogError("localtime failed");
          }
          else
          {
              char timetemp[80] = { 0 };
              if (strftime(timetemp, sizeof(timetemp) / sizeof(timetemp[0]), "%c", t) == 0)
              {
                  LogError("unable to strftime");
              }
              else
              {
                  printf("[%s] %.*s\r\n", timetemp, (int)content->size, content->buffer);
              }
          }
      }
   }
   ```

1. <span data-ttu-id="64b09-155">Frissítés a `config.json` fájlban adja meg a `workspace` a gazdaszámítógép és a központi telepítési útvonalán Intel NUC mappájába.</span><span class="sxs-lookup"><span data-stu-id="64b09-155">Update the `config.json` file to specify the `workspace` folder on your host computer and the deployment path on Intel NUC.</span></span> <span data-ttu-id="64b09-156">Során fordítása, a fájlokat a `workspace` mappát a telepítési útvonala használatával lesz áthelyezve.</span><span class="sxs-lookup"><span data-stu-id="64b09-156">During compiling, the files in the `workspace` folder will be transferred to the deployment path.</span></span>

   1. <span data-ttu-id="64b09-157">Nyissa meg a `config.json` fájl a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="64b09-157">Open the `config.json` file by running the following command:</span></span>

      ```bash
      code config.json
      ```

   1. <span data-ttu-id="64b09-158">Frissítés `config.json` a következő beállításokkal:</span><span class="sxs-lookup"><span data-stu-id="64b09-158">Update `config.json` with the following configuration:</span></span>

      ```json
      "workspace": "./module/my_module",
      "deploy_path": "module/my_module"
      ```

      ![config_json](media/iot-hub-gateway-kit-lessons/lesson5/config_json.png)

1. <span data-ttu-id="64b09-160">A modul összeállítása a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="64b09-160">Compile the module by running the following command:</span></span>

   ```bash
   gulp compile
   ```

   <span data-ttu-id="64b09-161">A parancs Intel NUC átviszi a forráskódot, és futtatja `build.sh` a modul összeállításához.</span><span class="sxs-lookup"><span data-stu-id="64b09-161">The command transfers the source code to Intel NUC and runs `build.sh` to compile the module.</span></span>

## <a name="add-the-module-to-the-helloworld-sample-app-and-run-the-app-on-intel-nuc"></a><span data-ttu-id="64b09-162">A modul felvétele a hello_world mintaalkalmazást, és futtassa az alkalmazást azon Intel NUC</span><span class="sxs-lookup"><span data-stu-id="64b09-162">Add the module to the hello_world sample app and run the app on Intel NUC</span></span>

<span data-ttu-id="64b09-163">Ez a feladat végrehajtásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="64b09-163">To perform this task, follow these steps:</span></span>

1. <span data-ttu-id="64b09-164">Minden a rendelkezésre álló modul bináris fájljainak (.so fájlok) a Intel NUC listán a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="64b09-164">List all the available module binaries (.so files) on Intel NUC by running the following command:</span></span>

   ```bash
   gulp modules --list
   ```

   <span data-ttu-id="64b09-165">A bináris elérési útja `my_module` fordított kell jelennie az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="64b09-165">The binary path of `my_module` that you compiled should be listed as below:</span></span>

   ```path
   /root/gateway_sample/module/my_module/build/libmy_module.so
   ```

   <span data-ttu-id="64b09-166">Ha megváltoztatja az alapértelmezett bejelentkezési felhasználónevének a `config-gateway.json`, a bináris fájl elérési útja indul rendelkező `home/<your username>` helyett `root`.</span><span class="sxs-lookup"><span data-stu-id="64b09-166">If you change the default login username in `config-gateway.json`, the binary path will start with `home/<your username>` instead of `root`.</span></span>

1. <span data-ttu-id="64b09-167">Adja hozzá `my_module` számára a `hello_world` mintaalkalmazást:</span><span class="sxs-lookup"><span data-stu-id="64b09-167">Add `my_module` to the `hello_world` sample app:</span></span>

   1. <span data-ttu-id="64b09-168">Nyissa meg a `hello_world.json` fájl a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="64b09-168">Open the `hello_world.json` file by running the following command:</span></span>

      ```bash
      code sample/hello_world/src/hello_world.json
      ```

   1. <span data-ttu-id="64b09-169">Adja hozzá a következő kódot a `modules` szakasz:</span><span class="sxs-lookup"><span data-stu-id="64b09-169">Add the following code to the `modules` section:</span></span>

      ```json
      {
       "name": "my_module",
       "loader": {
       "name": "native",
       "entrypoint": {
       "module.path": "/root/gateway_sample/module/my_module/build/libmy_module.so"
         }
        },
       "args": null
      }
      ```

      <span data-ttu-id="64b09-170">Értékének `module.path` kell `/root/gateway_sample/module/my_module/build/libmy_module.so`.</span><span class="sxs-lookup"><span data-stu-id="64b09-170">The value of `module.path` should be `/root/gateway_sample/module/my_module/build/libmy_module.so`.</span></span> <span data-ttu-id="64b09-171">A kód deklarál `my_module` az átjáró, valamint a megadott modul bináris helyét társítandó `module.path`.</span><span class="sxs-lookup"><span data-stu-id="64b09-171">The code declares `my_module` to be associated with the gateway as well as the location of the module binary specified in `module.path`.</span></span>
   1. <span data-ttu-id="64b09-172">Adja hozzá a következő kódot a `links` szakasz:</span><span class="sxs-lookup"><span data-stu-id="64b09-172">Add the following code to the `links` section:</span></span>

      ```json
      {
        "source": "hello_world",
        "sink": "my_module"
      }
      ```

      <span data-ttu-id="64b09-173">Ez a kód határozza meg, hogy üzenetek település a a `hello_world` modul `my_module`.</span><span class="sxs-lookup"><span data-stu-id="64b09-173">This code specifies that messages are transferred from the `hello_world` module to `my_module`.</span></span>

      ![hello_world_json](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_json.png)

1. <span data-ttu-id="64b09-175">Futtassa a `hello_world` mintaalkalmazást a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="64b09-175">Run the `hello_world` sample app by running the following command:</span></span>

   ```bash
   gulp run --config sample/hello_world/src/hello_world.json
   ```

   <span data-ttu-id="64b09-176">A `--config` paraméter kényszeríti a `run-hello-world.js` parancsfájl használatával a `hello_world.json` fájlt.</span><span class="sxs-lookup"><span data-stu-id="64b09-176">The `--config` parameter forces the `run-hello-world.js` script to run by using the `hello_world.json` file.</span></span>

   ![hello_world_new](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_new.png)

<span data-ttu-id="64b09-178">Gratulálok.</span><span class="sxs-lookup"><span data-stu-id="64b09-178">Congratulations.</span></span> <span data-ttu-id="64b09-179">Most már megtekintheti az új modul viselkedését, egyszerűen jelenít meg az időbélyegzőnek a "hello world" üzenetet, az eredeti "hello_world" modulból eltérő eredményt.</span><span class="sxs-lookup"><span data-stu-id="64b09-179">You can now see the behavior of this new module, it simply prints out "hello world" messages with a timestamp, a different result from the original "hello_world" module.</span></span>

## <a name="next-steps"></a><span data-ttu-id="64b09-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="64b09-180">Next Steps</span></span>

<span data-ttu-id="64b09-181">Már létrehozott egy új modul, azt hozzá a hello_world minta, és töltse le a minta-alkalmazást, az átjárót az új moduljának futtatásához.</span><span class="sxs-lookup"><span data-stu-id="64b09-181">You’ve created a new module, added it to the hello_world sample, and get the sample app to run with the new module on your gateway.</span></span> <span data-ttu-id="64b09-182">Ha azt szeretné, további információt az Azure IoT átjáró modulok, több modul minta itt találja: [https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules](https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules).</span><span class="sxs-lookup"><span data-stu-id="64b09-182">If you want to learn more about Azure IoT gateway modules, you can find more module samples here: [https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules](https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules).</span></span>

<!-- Images and links -->

[config_ble]: iot-hub-gateway-kit-c-lesson3-configure-ble-app.md
[setup_nuc]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
