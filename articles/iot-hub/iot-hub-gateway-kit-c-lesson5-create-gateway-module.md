---
title: "aaaCreate az első Azure IoT átjáró modul |} Microsoft Docs"
description: "Hozzon létre egy modult, és vegye fel tooa sample app toocustomize modul viselkedések."
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
ms.openlocfilehash: 48996fc026c8b708e328b5629801465810e5b6a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="lesson-5-create-your-first-azure-iot-gateway-module"></a><span data-ttu-id="6e48c-103">5. lecke: Az első Azure IoT átjáró modul létrehozása</span><span class="sxs-lookup"><span data-stu-id="6e48c-103">Lesson 5: Create your first Azure IoT Gateway module</span></span>
<span data-ttu-id="6e48c-104">Míg Azure IoT peremhálózati lehetővé teszi a Java, a .NET vagy Node.js nyelven írt toobuild modulok, ez az oktatóanyag végigvezeti hello készítéséhez egy modult a c kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="6e48c-104">While Azure IoT Edge allows you toobuild modules written in Java, .NET, or Node.js, this tutorial walks you through hello steps for building a module in C.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="6e48c-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="6e48c-105">What you will do</span></span>

- <span data-ttu-id="6e48c-106">Fordítsa le, és az Intel NUC hello hello_world mintaalkalmazás futtatása.</span><span class="sxs-lookup"><span data-stu-id="6e48c-106">Compile and run hello hello_world sample app on Intel NUC.</span></span>
- <span data-ttu-id="6e48c-107">Hozzon létre egy modult, és az Intel NUC lefordítani.</span><span class="sxs-lookup"><span data-stu-id="6e48c-107">Create a module and compile it on Intel NUC.</span></span>
- <span data-ttu-id="6e48c-108">Hello új modul toohello hello_world minta alkalmazás hozzáadása, és futtassa a hello minta Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="6e48c-108">Add hello new module toohello hello_world sample app and then run hello sample on Intel NUC.</span></span> <span data-ttu-id="6e48c-109">új modul hello kiírja az időbélyegzőnek "hello_world" üzenetek.</span><span class="sxs-lookup"><span data-stu-id="6e48c-109">hello new module prints out "hello_world" messages with a timestamp.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="6e48c-110">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="6e48c-110">What you will learn</span></span>

- <span data-ttu-id="6e48c-111">Hogyan toocompile, és futtassa a mintaalkalmazást Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="6e48c-111">How toocompile and run a sample app on Intel NUC.</span></span>
- <span data-ttu-id="6e48c-112">Hogyan toocreate modul.</span><span class="sxs-lookup"><span data-stu-id="6e48c-112">How toocreate a module.</span></span>
- <span data-ttu-id="6e48c-113">Hogyan tooadd modul tooa mintaalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6e48c-113">How tooadd module tooa sample app.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="6e48c-114">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="6e48c-114">What you need</span></span>

<span data-ttu-id="6e48c-115">Az Azure IoT-Edge számítógépen telepítve van.</span><span class="sxs-lookup"><span data-stu-id="6e48c-115">Azure IoT Edge that has been installed on your host computer.</span></span>

## <a name="folder-structure"></a><span data-ttu-id="6e48c-116">Mappaszerkezet</span><span class="sxs-lookup"><span data-stu-id="6e48c-116">Folder structure</span></span>

<span data-ttu-id="6e48c-117">Hello mintakód, amely lecke 1 klónozott hello lecke 5 almappájába, van egy `module` mappa és a `sample` mappát.</span><span class="sxs-lookup"><span data-stu-id="6e48c-117">In hello Lesson 5 subfolder of hello sample code which you cloned in lesson 1, there is a `module` folder and a `sample` folder.</span></span>

![my_module](media/iot-hub-gateway-kit-lessons/lesson5/my_module.png)

- <span data-ttu-id="6e48c-119">Hello `module/my_module` mappa hello kód és parancsfájl toobuild hello adatforrásmodul tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6e48c-119">hello `module/my_module` folder contains hello source code and script toobuild hello module.</span></span>
- <span data-ttu-id="6e48c-120">Hello `sample` mappa hello forrás kód és parancsfájl toobuild hello mintaalkalmazás tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6e48c-120">hello `sample` folder contains hello source code and script toobuild hello sample app.</span></span>

## <a name="compile-and-run-hello-helloworld-sample-app-on-intel-nuc"></a><span data-ttu-id="6e48c-121">Fordítási és az Intel NUC hello hello_world mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="6e48c-121">Compile and run hello hello_world sample app on Intel NUC</span></span>

<span data-ttu-id="6e48c-122">Hello `hello_world` minta létrehoz egy átjáró hello alapján `hello_world.json` fájl, amely hello két előre meghatározott modulok hello alkalmazáshoz kapcsolódó adja meg.</span><span class="sxs-lookup"><span data-stu-id="6e48c-122">hello `hello_world` sample creates a gateway based on hello `hello_world.json` file which specifies hello two predefined modules associated with hello app.</span></span> <span data-ttu-id="6e48c-123">hello-átjáró naplói a "hello world" üzenet tooa fájl minden 5 másodperc.</span><span class="sxs-lookup"><span data-stu-id="6e48c-123">hello gateway logs a "hello world" message tooa file every 5 seconds.</span></span> <span data-ttu-id="6e48c-124">Ebben a szakaszban fordítása, és futtassa a hello `hello_world` alkalmazás az alapértelmezett modullal.</span><span class="sxs-lookup"><span data-stu-id="6e48c-124">In this section, you compile and run hello `hello_world` app with its default module.</span></span>

<span data-ttu-id="6e48c-125">toocompile, és futtassa hello `hello_world` alkalmazást, és a gazdagép-számítógépen kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6e48c-125">toocompile and run hello `hello_world` app, follow these steps on your host computer:</span></span>

1. <span data-ttu-id="6e48c-126">Hello konfigurációs fájlok inicializálása hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="6e48c-126">Initialize hello configuration files by running hello following commands:</span></span>

   ```bash
   cd iot-hub-c-intel-nuc-gateway-getting-started
   cd Lesson5
   npm install
   gulp init
   ```

1. <span data-ttu-id="6e48c-127">Hello átjáró konfigurációs fájljának frissítése a hello Intel NUC MAC-címét.</span><span class="sxs-lookup"><span data-stu-id="6e48c-127">Update hello gateway configuration file with hello MAC address of Intel NUC.</span></span> <span data-ttu-id="6e48c-128">Kihagyhatja ezt a lépést, ha megtettünk mindent keresztül hello lecke túl[konfigurálására és futtatására BLA mintaalkalmazás][config_ble].</span><span class="sxs-lookup"><span data-stu-id="6e48c-128">Skip this step if you have gone through hello lesson too[configure and run a BLE sample application][config_ble].</span></span>

   1. <span data-ttu-id="6e48c-129">Nyissa meg a hello átjáró konfigurációs fájlját hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="6e48c-129">Open hello gateway configuration file by running hello following command:</span></span>

      ```bash
      # For Windows command prompt
      code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json

      # For MacOS or Ubuntu
      code ~/.iot-hub-getting-started/config-gateway.json
      ```

   1. <span data-ttu-id="6e48c-130">Frissítés hello átjáró MAC-címe esetén, [Intel NUC IoT átjáróként beállítása][setup_nuc], majd mentse hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="6e48c-130">Update hello gateway's MAC address when you [set up Intel NUC as a IoT gateway][setup_nuc], and then save hello file.</span></span>

1. <span data-ttu-id="6e48c-131">Fordítási forrás mintakód hello hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="6e48c-131">Compile hello sample source code by running hello following command:</span></span>

   ```bash
   gulp compile
   ```

   <span data-ttu-id="6e48c-132">hello parancs hello minta forrás kód tooIntel NUC továbbítja, és futtatja `build.sh` toocompile azt.</span><span class="sxs-lookup"><span data-stu-id="6e48c-132">hello command transfers hello sample source code tooIntel NUC and runs `build.sh` toocompile it.</span></span>

1. <span data-ttu-id="6e48c-133">Futtassa a hello `hello_world` alkalmazást az Intel NUC hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="6e48c-133">Run hello `hello_world` app on Intel NUC by running hello following command:</span></span>

   ```bash
   gulp run
   ```

   <span data-ttu-id="6e48c-134">a parancs futtatása hello `../Tools/run-hello-world.js` meghatározott `config.json` Intel NUC toostart hello mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6e48c-134">hello command runs `../Tools/run-hello-world.js` that is specified in `config.json` toostart hello sample app on Intel NUC.</span></span>

   ![run_sample](media/iot-hub-gateway-kit-lessons/lesson5/run_sample.png)

## <a name="create-a-new-module-and-compile-it-on-intel-nuc"></a><span data-ttu-id="6e48c-136">Hozzon létre egy új modult, és az Intel NUC lefordítani</span><span class="sxs-lookup"><span data-stu-id="6e48c-136">Create a new module and compile it on Intel NUC</span></span>

<span data-ttu-id="6e48c-137">hello lépéseket végigvezetik egy új modul létrehozása, és az Intel NUC lefordítani.</span><span class="sxs-lookup"><span data-stu-id="6e48c-137">hello steps below walk you through creating a new module and compile it on Intel NUC.</span></span> <span data-ttu-id="6e48c-138">hello modul kinyomtatja azokat az üzeneteket az időbélyegzőnek fogadásakor őket.</span><span class="sxs-lookup"><span data-stu-id="6e48c-138">hello module prints out messages with a timestamp upon receiving them.</span></span> <span data-ttu-id="6e48c-139">Ebben a szakaszban az első testreszabott átjáró modul hoz létre.</span><span class="sxs-lookup"><span data-stu-id="6e48c-139">You will create your first customized gateway module in this section.</span></span>

<span data-ttu-id="6e48c-140">Bármely Azure IoT peremhálózati modul a következő illesztők hello kell megvalósítania:</span><span class="sxs-lookup"><span data-stu-id="6e48c-140">Any Azure IoT Edge module must implement hello following interfaces:</span></span>

   ```C
   pfModule_ParseConfigurationFromJson Module_ParseConfigurationFromJson
   pfModule_FreeConfiguration Module_FreeConfiguration
   pfModule_Create Module_Create
   pfModule_Destroy Module_Destroy
   pfModule_Receive Module_Receive
   ```

<span data-ttu-id="6e48c-141">Opcionálisan valósíthatja meg a következő illesztőfelület hello:</span><span class="sxs-lookup"><span data-stu-id="6e48c-141">You can optionally implement hello following interface:</span></span>

   ```C
   pfModule_Start Module_Start
   ```

<span data-ttu-id="6e48c-142">hello alábbi ábrán látható hello fontos állapot elérési utak a modulok.</span><span class="sxs-lookup"><span data-stu-id="6e48c-142">hello following diagram shows hello important state paths of a module.</span></span> <span data-ttu-id="6e48c-143">hello négyzetes téglalapok képviselő módszerek tooperform műveletek végrehajtása, ha hello modul állapotokat közé helyezi.</span><span class="sxs-lookup"><span data-stu-id="6e48c-143">hello square rectangles represent methods you implement tooperform operations when hello module moves between states.</span></span> <span data-ttu-id="6e48c-144">hello ovális fő állapotok hello modul lehet a rendszer.</span><span class="sxs-lookup"><span data-stu-id="6e48c-144">hello ovals are major states hello module can be in.</span></span>

![state_path](media/iot-hub-gateway-kit-lessons/lesson5/state_path.png)

<span data-ttu-id="6e48c-146">Most hozzon létre egy modul hello sablon alapján:</span><span class="sxs-lookup"><span data-stu-id="6e48c-146">Now let’s create a module based on hello template:</span></span>

1. <span data-ttu-id="6e48c-147">Nyissa meg a sablon mappája hello hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="6e48c-147">Open hello template folder by running hello following command:</span></span>

   ```bash
   code module/my_module
   ```

   ![code_module](media/iot-hub-gateway-kit-lessons/lesson5/code_module.png)

   - <span data-ttu-id="6e48c-149">`src/my_module.c`a sablont, amely lehetővé teszi egy modul létrehozása hello funkcionál.</span><span class="sxs-lookup"><span data-stu-id="6e48c-149">`src/my_module.c` serves as a template that facilitates hello creation of a module.</span></span> <span data-ttu-id="6e48c-150">hello sablon hello felületek deklarálja.</span><span class="sxs-lookup"><span data-stu-id="6e48c-150">hello template declares hello interfaces.</span></span> <span data-ttu-id="6e48c-151">Toodo szüksége tooadd logika toohello `MyModule_Receive` függvény.</span><span class="sxs-lookup"><span data-stu-id="6e48c-151">All you need toodo is tooadd logic toohello `MyModule_Receive` function.</span></span>
   - <span data-ttu-id="6e48c-152">`build.sh`az hello build script toocompile hello modul Intel NUC.</span><span class="sxs-lookup"><span data-stu-id="6e48c-152">`build.sh` is hello build script toocompile hello module on Intel NUC.</span></span>
1. <span data-ttu-id="6e48c-153">Nyissa meg hello `src/my_module.c` fájlt, és két fejlécfájlok tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="6e48c-153">Open hello `src/my_module.c` file and include two header files:</span></span>

   ```C
   #include <stdio.h>
   #include "azure_c_shared_utility/xlogging.h"
   ```

1. <span data-ttu-id="6e48c-154">Adja hozzá a következő kód toohello hello `MyModule_Receive` függvény:</span><span class="sxs-lookup"><span data-stu-id="6e48c-154">Add hello following code toohello `MyModule_Receive` function:</span></span>

   ```C
   if (message == NULL)
   {
      LogError("invalid arg message");
   }
   else
   {
      // get hello message content
      const CONSTBUFFER * content = Message_GetContent(message);
      // get hello local time and format it
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
                  LogError("unable toostrftime");
              }
              else
              {
                  printf("[%s] %.*s\r\n", timetemp, (int)content->size, content->buffer);
              }
          }
      }
   }
   ```

1. <span data-ttu-id="6e48c-155">Frissítés hello `config.json` fájl toospecify hello `workspace` a fogadó számítógép hello telepítési elérési út és az Intel NUC mappájába.</span><span class="sxs-lookup"><span data-stu-id="6e48c-155">Update hello `config.json` file toospecify hello `workspace` folder on your host computer and hello deployment path on Intel NUC.</span></span> <span data-ttu-id="6e48c-156">Során fordítása, hello hello fájlok `workspace` mappa átvitt toohello telepítési útvonala lesz.</span><span class="sxs-lookup"><span data-stu-id="6e48c-156">During compiling, hello files in hello `workspace` folder will be transferred toohello deployment path.</span></span>

   1. <span data-ttu-id="6e48c-157">Nyissa meg hello `config.json` fájl hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="6e48c-157">Open hello `config.json` file by running hello following command:</span></span>

      ```bash
      code config.json
      ```

   1. <span data-ttu-id="6e48c-158">Frissítés `config.json` a következő konfigurációs hello:</span><span class="sxs-lookup"><span data-stu-id="6e48c-158">Update `config.json` with hello following configuration:</span></span>

      ```json
      "workspace": "./module/my_module",
      "deploy_path": "module/my_module"
      ```

      ![config_json](media/iot-hub-gateway-kit-lessons/lesson5/config_json.png)

1. <span data-ttu-id="6e48c-160">Fordítási hello modul hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="6e48c-160">Compile hello module by running hello following command:</span></span>

   ```bash
   gulp compile
   ```

   <span data-ttu-id="6e48c-161">hello parancs hello forrás kód tooIntel NUC továbbítja, és futtatja `build.sh` toocompile hello modul.</span><span class="sxs-lookup"><span data-stu-id="6e48c-161">hello command transfers hello source code tooIntel NUC and runs `build.sh` toocompile hello module.</span></span>

## <a name="add-hello-module-toohello-helloworld-sample-app-and-run-hello-app-on-intel-nuc"></a><span data-ttu-id="6e48c-162">Adja hozzá a hello modul toohello hello_world mintaalkalmazás és hello alkalmazás futtatását az Intel NUC</span><span class="sxs-lookup"><span data-stu-id="6e48c-162">Add hello module toohello hello_world sample app and run hello app on Intel NUC</span></span>

<span data-ttu-id="6e48c-163">tooperform ennek a feladatnak, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6e48c-163">tooperform this task, follow these steps:</span></span>

1. <span data-ttu-id="6e48c-164">Minden hello elérhető modul bináris fájlokat (.so fájlok) listájából Intel NUC hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="6e48c-164">List all hello available module binaries (.so files) on Intel NUC by running hello following command:</span></span>

   ```bash
   gulp modules --list
   ```

   <span data-ttu-id="6e48c-165">hello bináris elérési útja `my_module` fordított kell jelennie az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="6e48c-165">hello binary path of `my_module` that you compiled should be listed as below:</span></span>

   ```path
   /root/gateway_sample/module/my_module/build/libmy_module.so
   ```

   <span data-ttu-id="6e48c-166">Ha megváltoztatja hello alapértelmezett bejelentkezési felhasználónevének a `config-gateway.json`, hello bináris fájl elérési útja indul rendelkező `home/<your username>` ahelyett, hogy `root`.</span><span class="sxs-lookup"><span data-stu-id="6e48c-166">If you change hello default login username in `config-gateway.json`, hello binary path will start with `home/<your username>` instead of `root`.</span></span>

1. <span data-ttu-id="6e48c-167">Adja hozzá `my_module` toohello `hello_world` mintaalkalmazást:</span><span class="sxs-lookup"><span data-stu-id="6e48c-167">Add `my_module` toohello `hello_world` sample app:</span></span>

   1. <span data-ttu-id="6e48c-168">Nyissa meg hello `hello_world.json` fájl hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="6e48c-168">Open hello `hello_world.json` file by running hello following command:</span></span>

      ```bash
      code sample/hello_world/src/hello_world.json
      ```

   1. <span data-ttu-id="6e48c-169">Adja hozzá a következő kód toohello hello `modules` szakasz:</span><span class="sxs-lookup"><span data-stu-id="6e48c-169">Add hello following code toohello `modules` section:</span></span>

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

      <span data-ttu-id="6e48c-170">hello értékének `module.path` kell `/root/gateway_sample/module/my_module/build/libmy_module.so`.</span><span class="sxs-lookup"><span data-stu-id="6e48c-170">hello value of `module.path` should be `/root/gateway_sample/module/my_module/build/libmy_module.so`.</span></span> <span data-ttu-id="6e48c-171">hello kód deklarál `my_module` hello átjáró, valamint a megadott hello modul bináris hello helyét társított toobe `module.path`.</span><span class="sxs-lookup"><span data-stu-id="6e48c-171">hello code declares `my_module` toobe associated with hello gateway as well as hello location of hello module binary specified in `module.path`.</span></span>
   1. <span data-ttu-id="6e48c-172">Adja hozzá a következő kód toohello hello `links` szakasz:</span><span class="sxs-lookup"><span data-stu-id="6e48c-172">Add hello following code toohello `links` section:</span></span>

      ```json
      {
        "source": "hello_world",
        "sink": "my_module"
      }
      ```

      <span data-ttu-id="6e48c-173">Ez a kód határozza meg, hogy üzenetek település a hello `hello_world` modul túl`my_module`.</span><span class="sxs-lookup"><span data-stu-id="6e48c-173">This code specifies that messages are transferred from hello `hello_world` module too`my_module`.</span></span>

      ![hello_world_json](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_json.png)

1. <span data-ttu-id="6e48c-175">Futtassa a hello `hello_world` mintaalkalmazás hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="6e48c-175">Run hello `hello_world` sample app by running hello following command:</span></span>

   ```bash
   gulp run --config sample/hello_world/src/hello_world.json
   ```

   <span data-ttu-id="6e48c-176">Hello `--config` paraméter kényszeríti hello `run-hello-world.js` toorun parancsfájl hello segítségével `hello_world.json` fájlt.</span><span class="sxs-lookup"><span data-stu-id="6e48c-176">hello `--config` parameter forces hello `run-hello-world.js` script toorun by using hello `hello_world.json` file.</span></span>

   ![hello_world_new](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_new.png)

<span data-ttu-id="6e48c-178">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="6e48c-178">Congratulations.</span></span> <span data-ttu-id="6e48c-179">Most már megtekintheti az új modul hello viselkedését, egyszerűen jelenít meg az időbélyegzőnek a "hello world" üzeneteket, egy másik eredmény modulból hello eredeti "hello_world".</span><span class="sxs-lookup"><span data-stu-id="6e48c-179">You can now see hello behavior of this new module, it simply prints out "hello world" messages with a timestamp, a different result from hello original "hello_world" module.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6e48c-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6e48c-180">Next Steps</span></span>

<span data-ttu-id="6e48c-181">Már létrehozott egy új modul, akkor az átjáróra hozzá toohello hello_world mintát, és a get hello sample app toorun hello új modul.</span><span class="sxs-lookup"><span data-stu-id="6e48c-181">You’ve created a new module, added it toohello hello_world sample, and get hello sample app toorun with hello new module on your gateway.</span></span> <span data-ttu-id="6e48c-182">Ha azt szeretné, hogy további információk az Azure IoT átjáró modulok toolearn, több modul minta itt találja: [https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules](https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules).</span><span class="sxs-lookup"><span data-stu-id="6e48c-182">If you want toolearn more about Azure IoT gateway modules, you can find more module samples here: [https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules](https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules).</span></span>

<!-- Images and links -->

[config_ble]: iot-hub-gateway-kit-c-lesson3-configure-ble-app.md
[setup_nuc]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
