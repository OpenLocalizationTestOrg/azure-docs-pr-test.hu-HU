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
# <a name="lesson-5-create-your-first-azure-iot-gateway-module"></a>5. lecke: Az első Azure IoT átjáró modul létrehozása
Azure IoT peremhálózati build Java, a .NET vagy Node.js nyelven írt modulok teszi lehetővé, amíg ez az oktatóanyag végigvezeti a készítéséhez egy modult a c kiszolgálóra.

## <a name="what-you-will-do"></a>Mit fog

- Fordítsa le, és az Intel NUC hello_world mintaalkalmazás futtatása.
- Hozzon létre egy modult, és az Intel NUC lefordítani.
- Az új modul felvétele a hello_world mintaalkalmazást, és futtassa a minta Intel NUC. Új modul megjeleníti az időbélyegzőnek "hello_world" üzenetek.

## <a name="what-you-will-learn"></a>Amiről tanulni fog

- Fordítsa le, és futtassa a mintaalkalmazást Intel NUC módjáról.
- Hogyan hozható létre modul.
- Hogyan modul hozzáadása egy mintaalkalmazást.

## <a name="what-you-need"></a>Mi szükséges

Az Azure IoT-Edge számítógépen telepítve van.

## <a name="folder-structure"></a>Mappaszerkezet

Az 1 lecke klónozott példakód lecke 5 almappája van egy `module` mappa és a `sample` mappa.

![my_module](media/iot-hub-gateway-kit-lessons/lesson5/my_module.png)

- A `module/my_module` mappa tartalmazza a forráskódot, és a parancsprogramot hozni a modult.
- A `sample` mappa tartalmazza a Forráskód és a parancsprogramot a minta alkalmazás elkészítésére.

## <a name="compile-and-run-the-helloworld-sample-app-on-intel-nuc"></a>Fordítsa le, és az Intel NUC hello_world mintaalkalmazás futtatása

A `hello_world` minta létrehoz egy átjáró alapján a `hello_world.json` fájl, amely azt adja meg az alkalmazással társított alkalmazások két előre meghatározott modulok. Az átjáró a "hello world" üzenet-fájlban naplózza minden 5 másodperc. Ebben a szakaszban fordítása, és futtassa a `hello_world` alkalmazás az alapértelmezett modullal.

Fordításához és futtatásához a `hello_world` alkalmazást, és a gazdagép-számítógépen kövesse az alábbi lépéseket:

1. A konfigurációs fájlok inicializálása a következő parancsok futtatásával:

   ```bash
   cd iot-hub-c-intel-nuc-gateway-getting-started
   cd Lesson5
   npm install
   gulp init
   ```

1. Az átjáró konfigurációs fájljának frissítése Intel NUC a MAC-címmel. Kihagyhatja ezt a lépést, ha a lecke lezajlott [konfigurálására és futtatására BLA mintaalkalmazás][config_ble].

   1. Nyissa meg az átjáró konfigurációs fájlját a következő parancs futtatásával:

      ```bash
      # For Windows command prompt
      code %USERPROFILE%\.iot-hub-getting-started\config-gateway.json

      # For MacOS or Ubuntu
      code ~/.iot-hub-getting-started/config-gateway.json
      ```

   1. Ha az átjáró MAC-címe frissítés meg [Intel NUC IoT átjáróként beállítva][setup_nuc], és mentse a fájlt.

1. A minta forráskód összeállítása a következő parancs futtatásával:

   ```bash
   gulp compile
   ```

   A parancs Intel NUC adja át a minta forráskódot, és futtatja `build.sh` , hogy.

1. Futtassa a `hello_world` Intel NUC alkalmazást a következő parancs futtatásával:

   ```bash
   gulp run
   ```

   A parancs végrehajtása `../Tools/run-hello-world.js` meghatározott `config.json` elindítja a mintaalkalmazás Intel NUC.

   ![run_sample](media/iot-hub-gateway-kit-lessons/lesson5/run_sample.png)

## <a name="create-a-new-module-and-compile-it-on-intel-nuc"></a>Hozzon létre egy új modult, és az Intel NUC lefordítani

Az alábbi lépéseket végigvezetik egy új modul létrehozása, és az Intel NUC lefordítani. A modul kinyomtatja azokat az üzeneteket az időbélyegzőnek fogadásakor őket. Ebben a szakaszban az első testreszabott átjáró modul hoz létre.

Bármely Azure IoT peremhálózati modul implementálnia kell a következő kapcsolódási pontok:

   ```C
   pfModule_ParseConfigurationFromJson Module_ParseConfigurationFromJson
   pfModule_FreeConfiguration Module_FreeConfiguration
   pfModule_Create Module_Create
   pfModule_Destroy Module_Destroy
   pfModule_Receive Module_Receive
   ```

Ha szükséges a következő kapcsolat valósíthatja meg:

   ```C
   pfModule_Start Module_Start
   ```

Az alábbi ábrán látható, a fontos állapot elérési utak a modulok. A négyzet téglalapok képviselő módszerek megvalósítása műveletek végrehajtásához, ha a modul állapotokat közé helyezi. A ovális lehet, hogy a modul a fő állapotok.

![state_path](media/iot-hub-gateway-kit-lessons/lesson5/state_path.png)

Most hozzon létre egy modult a sablon alapján:

1. Nyissa meg a sablon mappát a következő parancs futtatásával:

   ```bash
   code module/my_module
   ```

   ![code_module](media/iot-hub-gateway-kit-lessons/lesson5/code_module.png)

   - `src/my_module.c`a sablont, amely elősegíti a modul létrehozása funkcionál. A sablon a felületek deklarálja. Mindössze annyit kell tennie az logika hozzáadása a `MyModule_Receive` függvény.
   - `build.sh`a build parancsfájl Intel NUC moduljánál fordítása van.
1. Nyissa meg a `src/my_module.c` fájlt, és két fejlécfájlok tartalmazza:

   ```C
   #include <stdio.h>
   #include "azure_c_shared_utility/xlogging.h"
   ```

1. Adja hozzá a következő kódot a `MyModule_Receive` függvény:

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

1. Frissítés a `config.json` fájlban adja meg a `workspace` a gazdaszámítógép és a központi telepítési útvonalán Intel NUC mappájába. Során fordítása, a fájlokat a `workspace` mappát a telepítési útvonala használatával lesz áthelyezve.

   1. Nyissa meg a `config.json` fájl a következő parancs futtatásával:

      ```bash
      code config.json
      ```

   1. Frissítés `config.json` a következő beállításokkal:

      ```json
      "workspace": "./module/my_module",
      "deploy_path": "module/my_module"
      ```

      ![config_json](media/iot-hub-gateway-kit-lessons/lesson5/config_json.png)

1. A modul összeállítása a következő parancs futtatásával:

   ```bash
   gulp compile
   ```

   A parancs Intel NUC átviszi a forráskódot, és futtatja `build.sh` a modul összeállításához.

## <a name="add-the-module-to-the-helloworld-sample-app-and-run-the-app-on-intel-nuc"></a>A modul felvétele a hello_world mintaalkalmazást, és futtassa az alkalmazást azon Intel NUC

Ez a feladat végrehajtásához kövesse az alábbi lépéseket:

1. Minden a rendelkezésre álló modul bináris fájljainak (.so fájlok) a Intel NUC listán a következő parancs futtatásával:

   ```bash
   gulp modules --list
   ```

   A bináris elérési útja `my_module` fordított kell jelennie az alábbiak szerint:

   ```path
   /root/gateway_sample/module/my_module/build/libmy_module.so
   ```

   Ha megváltoztatja az alapértelmezett bejelentkezési felhasználónevének a `config-gateway.json`, a bináris fájl elérési útja indul rendelkező `home/<your username>` helyett `root`.

1. Adja hozzá `my_module` számára a `hello_world` mintaalkalmazást:

   1. Nyissa meg a `hello_world.json` fájl a következő parancs futtatásával:

      ```bash
      code sample/hello_world/src/hello_world.json
      ```

   1. Adja hozzá a következő kódot a `modules` szakasz:

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

      Értékének `module.path` kell `/root/gateway_sample/module/my_module/build/libmy_module.so`. A kód deklarál `my_module` az átjáró, valamint a megadott modul bináris helyét társítandó `module.path`.
   1. Adja hozzá a következő kódot a `links` szakasz:

      ```json
      {
        "source": "hello_world",
        "sink": "my_module"
      }
      ```

      Ez a kód határozza meg, hogy üzenetek település a a `hello_world` modul `my_module`.

      ![hello_world_json](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_json.png)

1. Futtassa a `hello_world` mintaalkalmazást a következő parancs futtatásával:

   ```bash
   gulp run --config sample/hello_world/src/hello_world.json
   ```

   A `--config` paraméter kényszeríti a `run-hello-world.js` parancsfájl használatával a `hello_world.json` fájlt.

   ![hello_world_new](media/iot-hub-gateway-kit-lessons/lesson5/hello_world_new.png)

Gratulálok. Most már megtekintheti az új modul viselkedését, egyszerűen jelenít meg az időbélyegzőnek a "hello world" üzenetet, az eredeti "hello_world" modulból eltérő eredményt.

## <a name="next-steps"></a>Következő lépések

Már létrehozott egy új modul, azt hozzá a hello_world minta, és töltse le a minta-alkalmazást, az átjárót az új moduljának futtatásához. Ha azt szeretné, további információt az Azure IoT átjáró modulok, több modul minta itt találja: [https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules](https://github.com/Azure/azure-iot-gateway-sdk/tree/master/modules).

<!-- Images and links -->

[config_ble]: iot-hub-gateway-kit-c-lesson3-configure-ble-app.md
[setup_nuc]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
