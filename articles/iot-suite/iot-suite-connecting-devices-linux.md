---
title: "egy eszköz használatával C Linux aaaConnect |} Microsoft Docs"
description: "Ismerteti, hogyan tooconnect egy eszköz toohello Azure IoT Suite előre konfigurált távoli figyelési megoldást igényelnek a futó Linux C alkalmazás segítségével."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 0c7c8039-0bbf-4bb5-9e79-ed8cff433629
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: 57393817d40d3555177956a01fa71058bc256988
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-device-toohello-remote-monitoring-preconfigured-solution-linux"></a>Csatlakozás az eszköz toohello távoli felügyeleti előkonfigurált megoldás (Linux)
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-a-sample-c-client-linux"></a>Hozza létre, és egy minta C-ügyfél Linux
hello lépések bemutatják, hogyan toocreate hello távoli megfigyelési kommunikáló ügyfélalkalmazás előre konfigurált megoldás. Ez az alkalmazás a C és beépített és Ubuntu Linux rendszeren futtassa.

Ezek a lépések toocomplete, az Ubuntu 15.04 vagy 15.10 verzióját futtató eszközök szüksége. A folytatás előtt egy Ubuntu eszközt a következő parancs hello hello csomagokat telepíthető:

```
sudo apt-get install cmake gcc g++
```

## <a name="install-hello-client-libraries-on-your-device"></a>Hello klienskódtárak segítségével telepítse az eszközre
hello Azure IoT Hub klienskódtárak érhetők el is telepíthető egy Ubuntu eszközt hello csomag **apt get** parancsot. Hajtsa végre a következő lépéseket tooinstall hello tartalmazó csomagot hello IoT-központ ügyféloldali kódtár és Ubuntu számítógépre fejlécfájlok hello:

1. A rendszerhéj hello AzureIoT tárház tooyour számítógép hozzáadása:
   
    ```
    sudo add-apt-repository ppa:aziotsdklinux/ppa-azureiot
    sudo apt-get update
    ```
2. Hello azure-iot-sdk-c-fejlesztői csomag telepítése
   
    ```
    sudo apt-get install -y azure-iot-sdk-c-dev
    ```

## <a name="install-hello-parson-json-parser"></a>Hello Parson JSON elemző telepítése
szalagtárak használata az IoT-központ ügyfél hello hello Parson JSON elemző tooparse üzenet hasznos adat található. A megfelelő mappát a számítógépén klónozni hello Parson GitHub-tárházban hello a következő parancs használatával:

```
git clone https://github.com/kgabis/parson.git
```

## <a name="prepare-your-project"></a>Készítse elő a projekthez
A Ubuntu gépen, hozzon létre egy nevű **távoli\_figyelési**. A hello **távoli\_figyelési** mappába:

- Hozzon létre hello négy fájlok **main.c**, **távoli\_monitoring.c**, **távoli\_monitoring.h**, és **CMakeLists.txt**.
- Hozzon létre nevű **parson**.

Hello fájlok másolása **parson.c** és **parson.h** hello Parson tárház helyi másolatát a hello **távoli\_figyelési/parson** mappa.

Egy szövegszerkesztőben nyissa meg a hello **távoli\_monitoring.c** fájlt. Adja hozzá a következő hello `#include` utasításokat:
   
```
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

## <a name="call-hello-remotemonitoringrun-function"></a>Távoli eljáráshívás hello\_figyelési\_függvény futtatása
Egy szövegszerkesztőben nyissa meg a hello **remote_monitoring.h** fájlt. Adja hozzá a következő kód hello:

```
void remote_monitoring_run(void);
```

Egy szövegszerkesztőben nyissa meg a hello **main.c** fájlt. Adja hozzá a következő kód hello:

```
#include "remote_monitoring.h"

int main(void)
{
    remote_monitoring_run();

    return 0;
}
```

## <a name="build-and-run-hello-application"></a>Hozza létre és hello alkalmazás futtatása
hello következő lépések bemutatják, hogyan toouse *CMake* toobuild az ügyfélalkalmazást.

1. Egy szövegszerkesztőben nyissa meg a hello **CMakeLists.txt** hello fájlban **remote_monitoring** mappa.

1. Adja hozzá a következő utasításokat toodefine hogyan hello toobuild az ügyfélalkalmazás:
   
    ```
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

    set(AZUREIOT_INC_FOLDER "${CMAKE_SOURCE_DIR}" "${CMAKE_SOURCE_DIR}/parson" "/usr/include/azureiot" "/usr/include/azureiot/inc")

    include_directories(${AZUREIOT_INC_FOLDER})

    set(sample_application_c_files
        ./parson/parson.c
        ./remote_monitoring.c
        ./main.c
    )

    set(sample_application_h_files
        ./parson/parson.h
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
1. A hello **remote_monitoring** mappa, hozzon létre egy mappát toostore hello *ellenőrizze* fájlok adott CMake állít elő, és futtassa a hello **cmake** és **Ellenőrizze** parancsok az alábbiak szerint:
   
    ```
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

1. Hello ügyfélalkalmazás futtatása, és telemetriai tooIoT Hub küldeni:
   
    ```
    ./sample_app
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

