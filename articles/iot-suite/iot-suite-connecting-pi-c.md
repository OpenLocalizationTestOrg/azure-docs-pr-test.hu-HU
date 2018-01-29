---
title: "Távoli figyelésével C - Azure használatával málna Pi kiépítése |} Microsoft Docs"
description: "Ismerteti, hogyan lehet egy málna Pi eszköz csatlakoztatása az Azure IoT Suite előre konfigurált távoli figyelési megoldást c nyelven írt alkalmazás segítségével"
services: iot-suite
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: fc50a33f-9fb9-42d7-b1b8-eb5cff19335e
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2018
ms.author: dobett
ms.openlocfilehash: 7cfa6dd93c6db7477e03ff966b2ac8af15de3614
ms.sourcegitcommit: 2e540e6acb953b1294d364f70aee73deaf047441
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/03/2018
---
# <a name="connect-your-raspberry-pi-device-to-the-remote-monitoring-preconfigured-solution-c"></a>A Pi málna eszköz csatlakozni a távoli felügyeleti előkonfigurált megoldás (C)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Ez az oktatóanyag bemutatja, hogyan egy fizikai eszköz csatlakozni a távoli felügyeleti előkonfigurált megoldás. Csakúgy, mint a beágyazott korlátozott eszközökön futó alkalmazások, az ügyfélkód a málna Pi eszköz alkalmazás íródott a c kiszolgálóra. Ebben az oktatóanyagban hoz létre az alkalmazás egy málna Pi Raspbian operációs rendszert futtató.

### <a name="required-hardware"></a>Szükséges hardver

Ahhoz, hogy a parancssor a málna Pi a távoli csatlakozás egy asztali számítógépen.

[Microsoft IoT Starter Kit málna Pi 3](https://azure.microsoft.com/develop/iot/starter-kits/) vagy ezzel egyenértékű összetevőket. Ez az oktatóanyag a csomagot a következő elemeket használja:

- Raspberry Pi 3
- (A NOOBS) MicroSD-kártyán
- A USB Mini-kábellel
- Kábel

### <a name="required-desktop-software"></a>Szükséges asztali szoftverek

SSH-ügyfél van szüksége az asztali gépen ahhoz, hogy a parancssor a málna Pi a érheti el távolról.

- A Windows tartalmaz egy SSH-ügyfél. Azt javasoljuk, [PuTTY](http://www.putty.org/).
- A legtöbb Linux terjesztéseket, a Mac OS közé tartoznak az SSH parancssori segédprogramot. További információkért lásd: [SSH használatával Linux és Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

### <a name="required-raspberry-pi-software"></a>Szükséges málna Pi szoftver

Ez a cikk feltételezi, hogy a legújabb verziójának telepítése a [a málna Pi Raspbian operációs](https://www.raspberrypi.org/learning/software-guide/quickstart/).

A következő lépések bemutatják egy C alkalmazás, amely kapcsolódik az előkonfigurált megoldás létrehozása a málna Pi előkészítése:

1. Kapcsolódás a málna Pi **ssh**. További információkért lásd: [SSH (Secure Shell)](https://www.raspberrypi.org/documentation/remote-access/ssh/README.md) a a [málna Pi webhely](https://www.raspberrypi.org/).

1. A málna Pi frissítéséhez használja a következő parancsot:

    ```sh
    sudo apt-get update
    ```

1. A következő paranccsal adja hozzá a málna Pi szükséges Fejlesztőeszközök és a szalagtárak:

    ```sh
    sudo apt-get purge libssl-dev
    sudo apt-get install g++ make cmake gcc git libssl1.0-dev build-essential curl libcurl4-openssl-dev uuid-dev
    ```

1. A következő parancsok segítségével töltse le, elkészítéséhez és az IoT-központ klienskódtárak segítségével telepítse a málna Pi:

    ```sh
    cd ~
    git clone --recursive https://github.com/azure/azure-iot-sdk-c.git
    cd azure-iot-sdk-c/build_all/linux
    ./build.sh --no-make
    cd ../../cmake/iotsdk_linux
    make
    sudo make install
    ```

## <a name="create-a-project"></a>Projekt létrehozása

Az alábbi lépésekkel használatával a **ssh** a málna Pi kapcsolatot:

1. Hozzon létre egy nevű `remote_monitoring` a málna Pi a saját mappájában. Lépjen abba a mappába, a rendszerhéj:

    ```sh
    cd ~
    mkdir remote_monitoring
    cd remote_monitoring
    ```

1. Hozza létre a négy fájlokat **main.c**, **remote_monitoring.c**, **remote_monitoring.h**, és **CMakeLists.txt** a a `remote_monitoring` a mappa.

1. Egy szövegszerkesztőben nyissa meg a **remote_monitoring.c** fájlt. A málna Pi is használhatja a **nano** vagy **vi** szövegszerkesztőben. Adja hozzá a következő `#include`-utasításokat:

    ```c
    #include "iothubtransportmqtt.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer_devicetwin.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    #include "parson.h"
    ```

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

Mentse a **remote_monitoring.c** fájlt, és zárja be a szerkesztőt.

## <a name="add-code-to-run-the-app"></a>Adja hozzá az alkalmazás futtatásához szükséges kódot

Egy szövegszerkesztőben nyissa meg a **remote_monitoring.h** fájlt. Adja hozzá a következő kódot:

```c
void remote_monitoring_run(void);
```

Mentse a **remote_monitoring.h** fájlt, és zárja be a szerkesztőt.

Egy szövegszerkesztőben nyissa meg a **main.c** fájlt. Adja hozzá a következő kódot:

```c
#include "remote_monitoring.h"

int main(void)
{
  remote_monitoring_run();

  return 0;
}
```

Mentse a **main.c** fájlt, és zárja be a szerkesztőt.

## <a name="build-and-run-the-application"></a>Az alkalmazás fordítása és futtatása

Az alábbi lépések bemutatják, hogyan használható *CMake* hozhat létre az ügyfélalkalmazást.

1. Egy szövegszerkesztőben nyissa meg a **CMakeLists.txt** fájlt a `remote_monitoring` mappát.

1. Az alábbi utasítások segítségével meghatározhatja, hogyan hozható létre az ügyfél-alkalmazás hozzáadása:

    ```cmake
    macro(compileAsC99)
      if (CMAKE_VERSION VERSION_LESS "3.1")
        if (CMAKE_C_COMPILER_ID STREQUAL "GNU")
          set (CMAKE_C_FLAGS "--std=c99 ${CMAKE_C_FLAGS}")
          set (CMAKE_CXX_FLAGS "--std=c++11 ${CMAKE_CXX_FLAGS}")
        endif()
      else()
        set (CMAKE_C_STANDARD 99)
        set (CMAKE_CXX_STANDARD 11)
      endif()
    endmacro(compileAsC99)

    cmake_minimum_required(VERSION 2.8.11)
    compileAsC99()

    set(AZUREIOT_INC_FOLDER "${CMAKE_SOURCE_DIR}" "/usr/local/include/azureiot")

    include_directories(${AZUREIOT_INC_FOLDER})

    set(sample_application_c_files
        ./remote_monitoring.c
        ./main.c
    )

    set(sample_application_h_files
        ./remote_monitoring.h
    )

    add_executable(sample_app ${sample_application_c_files} ${sample_application_h_files})

    target_link_libraries(sample_app
        serializer
        iothub_client
        iothub_client_mqtt_transport
        aziotsharedutil
        umqtt
        pthread
        curl
        ssl
        crypto
        m
    )
    ```

1. Mentse a **CMakeLists.txt** fájlt, és zárja be a szerkesztőt.

1. Az a `remote_monitoring` mappa, hozzon létre egy mappát tárolásához a *ellenőrizze* CMake előállított fájlokat. Ezután futtassa a **cmake** és **ellenőrizze** parancsok az alábbiak szerint:

    ```sh
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

1. Az ügyfélalkalmazás futtatása, és telemetriai adatokat küldhet az IoT hubhoz:

    ```sh
    ./sample_app
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
