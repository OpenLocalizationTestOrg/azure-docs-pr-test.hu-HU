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
# <a name="lesson-5-create-your-first-azure-iot-gateway-module"></a>5. lecke: Az első Azure IoT átjáró modul létrehozása
Míg Azure IoT peremhálózati lehetővé teszi a Java, a .NET vagy Node.js nyelven írt toobuild modulok, ez az oktatóanyag végigvezeti hello készítéséhez egy modult a c kiszolgálóra.

## <a name="what-you-will-do"></a>Mit fog

- Fordítsa le, és az Intel NUC hello hello_world mintaalkalmazás futtatása.
- Hozzon létre egy modult, és az Intel NUC lefordítani.
- Hello új modul toohello hello_world minta alkalmazás hozzáadása, és futtassa a hello minta Intel NUC. új modul hello kiírja az időbélyegzőnek "hello_world" üzenetek.

## <a name="what-you-will-learn"></a>Amiről tanulni fog

- Hogyan toocompile, és futtassa a mintaalkalmazást Intel NUC.
- Hogyan toocreate modul.
- Hogyan tooadd modul tooa mintaalkalmazás.

## <a name="what-you-need"></a>Mi szükséges

Az Azure IoT-Edge számítógépen telepítve van.

## <a name="folder-structure"></a>Mappaszerkezet

Hello mintakód, amely lecke 1 klónozott hello lecke 5 almappájába, van egy `module` mappa és a `sample` mappát.

![my_module](media/iot-hub-gateway-kit-lessons/lesson5/my_module.png)

- Hello `module/my_module` mappa hello kód és parancsfájl toobuild hello adatforrásmodul tartalmazza.
- Hello `sample` mappa hello forrás kód és parancsfájl toobuild hello mintaalkalmazás tartalmazza.

## <a name="compile-and-run-hello-helloworld-sample-app-on-intel-nuc"></a>Fordítási és az Intel NUC hello hello_world mintaalkalmazás futtatása

Hello `hello_world` minta létrehoz egy átjáró hello alapján `hello_world.json` fájl, amely hello két előre meghatározott modulok hello alkalmazáshoz kapcsolódó adja meg. hello-átjáró naplói a "hello world" üzenet tooa fájl minden 5 másodperc. Ebben a szakaszban fordítása, és futtassa a hello `hello_world` alkalmazás az alapértelmezett modullal.

toocompile, és futtassa hello `hello_world` alkalmazást, és a gazdagép-számítógépen kövesse az alábbi lépéseket:

1. Hello konfigurációs fájlok inicializálása hello a következő parancsok futtatásával:

   ```bash
   cd iot-hub-c-intel-nuc-gateway-getting-started
   cd Lesson5
   npm install
   gulp init
   ```

1. Hello átjáró konfigurációs fájljának frissítése a hello Intel NUC MAC-címét. Kihagyhatja ezt a lépést, ha megtettünk mindent keresztül hello lecke túl[konfigurálására és futtatására BLA mintaalkalmazás][config_ble].

   1. Nyissa meg a hello átjáró konfigurációs fájlját hello a következő parancs futtatásával:

      ```bash
      # For Windows command prompt
      code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json

      # For MacOS or Ubuntu
      code ~/.iot-hub-getting-started/config-gateway.json
      ```

   1. Frissítés hello átjáró MAC-címe esetén, [Intel NUC IoT átjáróként beállítása][setup_nuc], majd mentse hello fájlt.

1. Fordítási forrás mintakód hello hello a következő parancs futtatásával:

   ```bash
   gulp compile
   ```

   hello parancs hello minta forrás kód tooIntel NUC továbbítja, és futtatja `build.sh` toocompile azt.

1. Futtassa a hello `hello_world` alkalmazást az Intel NUC hello a következő parancs futtatásával:

   ```bash
   gulp run
   ```

   a parancs futtatása hello `../Tools/run-hello-world.js` meghatározott `config.json` Intel NUC toostart hello mintaalkalmazást.

   ![run_sample](media/iot-hub-gateway-kit-lessons/lesson5/run_sample.png)

## <a name="create-a-new-module-and-compile-it-on-intel-nuc"></a>Hozzon létre egy új modult, és az Intel NUC lefordítani

hello lépéseket végigvezetik egy új modul létrehozása, és az Intel NUC lefordítani. hello modul kinyomtatja azokat az üzeneteket az időbélyegzőnek fogadásakor őket. Ebben a szakaszban az első testreszabott átjáró modul hoz létre.

Bármely Azure IoT peremhálózati modul a következő illesztők hello kell megvalósítania:

   ```C
   pfModule_ParseConfigurationFromJson Module_ParseConfigurationFromJson
   pfModule_FreeConfiguration Module_FreeConfiguration
   pfModule_Create Module_Create
   pfModule_Destroy Module_Destroy
   pfModule_Receive Module_Receive
   ```

Opcionálisan valósíthatja meg a következő illesztőfelület hello:

   ```C
   pfModule_Start Module_Start
   ```

hello alábbi ábrán látható hello fontos állapot elérési utak a modulok. hello négyzetes téglalapok képviselő módszerek tooperform műveletek végrehajtása, ha hello modul állapotokat közé helyezi. hello ovális fő állapotok hello modul lehet a rendszer.

![state_path](media/iot-hub-gateway-kit-lessons/lesson5/state_path.png)

Most hozzon létre egy modul hello sablon alapján:

1. Nyissa meg a sablon mappája hello hello a következő parancs futtatásával:

   ```bash
   code module/my_module
   ```

   ![code_module](media/iot-hub-gateway-kit-lessons/lesson5/code_module.png)

   - `src/my_module.c`a sablont, amely lehetővé teszi egy modul létrehozása hello funkcionál. hello sablon hello felületek deklarálja. Toodo szüksége tooadd logika toohello `MyModule_Receive` függvény.
   - `build.sh`az hello build script toocompile hello modul Intel NUC.
1. Nyissa meg hello `src/my_module.c` fájlt, és két fejlécfájlok tartalmazza:

   ```C
   #include <stdio.h>
   #include "azure_c_shared_utility/xlogging.h"
   ```

1. Adja hozzá a következő kód toohello hello `MyModule_Receive` függvény:

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

1. Frissítés hello `config.json` fájl toospecify hello `workspace` a fogadó számítógép hello telepítési elérési út és az Intel NUC mappájába. Során fordítása, hello hello fájlok `workspace` mappa átvitt toohello telepítési útvonala lesz.

   1. Nyissa meg hello `config.json` fájl hello a következő parancs futtatásával:

      ```bash
      code config.json
      ```

   1. Frissítés `config.json` a következő konfigurációs hello:

      ```json
      "workspace": "./module/my_module",
      "deploy_path": "module/my_module"
      ```

      ![config_json](media/iot-hub-gateway-kit-lessons/lesson5/config_json.png)

1. Fordítási hello modul hello a következő parancs futtatásával:

   ```bash
   gulp compile
   ```

   hello parancs hello forrás kód tooIntel NUC továbbítja, és futtatja `build.sh` toocompile hello modul.

## <a name="add-hello-module-toohello-helloworld-sample-app-and-run-hello-app-on-intel-nuc"></a>Adja hozzá a hello modul toohello hello_world mintaalkalmazás és hello alkalmazás futtatását az Intel NUC

tooperform ennek a feladatnak, kövesse az alábbi lépéseket:

1. Minden hello elérhető modul bináris fájlokat (.so fájlok) listájából Intel NUC hello a következő parancs futtatásával:

   ```bash
   gulp modules --list
   ```

   hello bináris elérési útja `my_module` fordított kell jelennie az alábbiak szerint:

   ```path
   /root/gateway_sample/module/my_module/build/libmy_module.so
   ```

   Ha megváltoztatja hello alapértelmezett bejelentkezési felhasználónevének a `config-gateway.json`, hello bináris fájl elérési útja indul rendelkező `home/<your username>` ahelyett, hogy `root`.

1. Adja hozzá `my_module` toohello `hello_world` mintaalkalmazást:

   1. Nyissa meg hello `hello_world.json` fájl hello a következő parancs futtatásával:

      ```bash
      code sample/hello_world/src/hello_world.json
      ```

   1. Adja hozzá a következő kód toohello hello `modules` szakasz:

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

      hello értékének `module.path` kell `/root/gateway_sample/module/my_module/build/libmy_module.so`. hello kód deklarál `my_module` hello átjáró, valamint a megadott hello modul bináris hello helyét társított toobe `module.path`.
   1. Adja hozzá a következő kód toohello hello `links` szakasz:

      ```json
      {
        "source": "hello_world",
        "sink": "my_module"
      }
      ```

      Ez a kód határozza meg, hogy üzenetek település a hello `hello_world` modul túl`my_module`.

      ![hello_world_json](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_json.png)

1. Futtassa a hello `hello_world` mintaalkalmazás hello a következő parancs futtatásával:

   ```bash
   gulp run --config sample/hello_world/src/hello_world.json
   ```

   Hello `--config` paraméter kényszeríti hello `run-hello-world.js` toorun parancsfájl hello segítségével `hello_world.json` fájlt.

   ![hello_world_new](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_new.png)

Gratulálunk! Most már megtekintheti az új modul hello viselkedését, egyszerűen jelenít meg az időbélyegzőnek a "hello world" üzeneteket, egy másik eredmény modulból hello eredeti "hello_world".

## <a name="next-steps"></a>Következő lépések

Már létrehozott egy új modul, akkor az átjáróra hozzá toohello hello_world mintát, és a get hello sample app toorun hello új modul. Ha azt szeretné, hogy további információk az Azure IoT átjáró modulok toolearn, több modul minta itt találja: [https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules](https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules).

<!-- Images and links -->

[config_ble]: iot-hub-gateway-kit-c-lesson3-configure-ble-app.md
[setup_nuc]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
