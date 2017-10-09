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
# <a name="connect-your-device-toohello-remote-monitoring-preconfigured-solution-linux"></a><span data-ttu-id="60ee7-103">Csatlakozás az eszköz toohello távoli felügyeleti előkonfigurált megoldás (Linux)</span><span class="sxs-lookup"><span data-stu-id="60ee7-103">Connect your device toohello remote monitoring preconfigured solution (Linux)</span></span>
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-a-sample-c-client-linux"></a><span data-ttu-id="60ee7-104">Hozza létre, és egy minta C-ügyfél Linux</span><span class="sxs-lookup"><span data-stu-id="60ee7-104">Build and run a sample C client Linux</span></span>
<span data-ttu-id="60ee7-105">hello lépések bemutatják, hogyan toocreate hello távoli megfigyelési kommunikáló ügyfélalkalmazás előre konfigurált megoldás.</span><span class="sxs-lookup"><span data-stu-id="60ee7-105">hello following steps show you how toocreate a client application that communicates with hello remote monitoring preconfigured solution.</span></span> <span data-ttu-id="60ee7-106">Ez az alkalmazás a C és beépített és Ubuntu Linux rendszeren futtassa.</span><span class="sxs-lookup"><span data-stu-id="60ee7-106">This application is written in C and built and run on Ubuntu Linux.</span></span>

<span data-ttu-id="60ee7-107">Ezek a lépések toocomplete, az Ubuntu 15.04 vagy 15.10 verzióját futtató eszközök szüksége.</span><span class="sxs-lookup"><span data-stu-id="60ee7-107">toocomplete these steps, you need a device running Ubuntu version 15.04 or 15.10.</span></span> <span data-ttu-id="60ee7-108">A folytatás előtt egy Ubuntu eszközt a következő parancs hello hello csomagokat telepíthető:</span><span class="sxs-lookup"><span data-stu-id="60ee7-108">Before proceeding, install hello prerequisite packages on your Ubuntu device using hello following command:</span></span>

```
sudo apt-get install cmake gcc g++
```

## <a name="install-hello-client-libraries-on-your-device"></a><span data-ttu-id="60ee7-109">Hello klienskódtárak segítségével telepítse az eszközre</span><span class="sxs-lookup"><span data-stu-id="60ee7-109">Install hello client libraries on your device</span></span>
<span data-ttu-id="60ee7-110">hello Azure IoT Hub klienskódtárak érhetők el is telepíthető egy Ubuntu eszközt hello csomag **apt get** parancsot.</span><span class="sxs-lookup"><span data-stu-id="60ee7-110">hello Azure IoT Hub client libraries are available as a package you can install on your Ubuntu device using hello **apt-get** command.</span></span> <span data-ttu-id="60ee7-111">Hajtsa végre a következő lépéseket tooinstall hello tartalmazó csomagot hello IoT-központ ügyféloldali kódtár és Ubuntu számítógépre fejlécfájlok hello:</span><span class="sxs-lookup"><span data-stu-id="60ee7-111">Complete hello following steps tooinstall hello package that contains hello IoT Hub client library and header files on your Ubuntu computer:</span></span>

1. <span data-ttu-id="60ee7-112">A rendszerhéj hello AzureIoT tárház tooyour számítógép hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="60ee7-112">In a shell, add hello AzureIoT repository tooyour computer:</span></span>
   
    ```
    sudo add-apt-repository ppa:aziotsdklinux/ppa-azureiot
    sudo apt-get update
    ```
2. <span data-ttu-id="60ee7-113">Hello azure-iot-sdk-c-fejlesztői csomag telepítése</span><span class="sxs-lookup"><span data-stu-id="60ee7-113">Install hello azure-iot-sdk-c-dev package</span></span>
   
    ```
    sudo apt-get install -y azure-iot-sdk-c-dev
    ```

## <a name="install-hello-parson-json-parser"></a><span data-ttu-id="60ee7-114">Hello Parson JSON elemző telepítése</span><span class="sxs-lookup"><span data-stu-id="60ee7-114">Install hello Parson JSON parser</span></span>
<span data-ttu-id="60ee7-115">szalagtárak használata az IoT-központ ügyfél hello hello Parson JSON elemző tooparse üzenet hasznos adat található.</span><span class="sxs-lookup"><span data-stu-id="60ee7-115">hello IoT Hub client libraries use hello Parson JSON parser tooparse message payloads.</span></span> <span data-ttu-id="60ee7-116">A megfelelő mappát a számítógépén klónozni hello Parson GitHub-tárházban hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="60ee7-116">In a suitable folder on your computer, clone hello Parson GitHub repository using hello following command:</span></span>

```
git clone https://github.com/kgabis/parson.git
```

## <a name="prepare-your-project"></a><span data-ttu-id="60ee7-117">Készítse elő a projekthez</span><span class="sxs-lookup"><span data-stu-id="60ee7-117">Prepare your project</span></span>
<span data-ttu-id="60ee7-118">A Ubuntu gépen, hozzon létre egy nevű **távoli\_figyelési**.</span><span class="sxs-lookup"><span data-stu-id="60ee7-118">On your Ubuntu machine, create a folder called **remote\_monitoring**.</span></span> <span data-ttu-id="60ee7-119">A hello **távoli\_figyelési** mappába:</span><span class="sxs-lookup"><span data-stu-id="60ee7-119">In hello **remote\_monitoring** folder:</span></span>

- <span data-ttu-id="60ee7-120">Hozzon létre hello négy fájlok **main.c**, **távoli\_monitoring.c**, **távoli\_monitoring.h**, és **CMakeLists.txt**.</span><span class="sxs-lookup"><span data-stu-id="60ee7-120">Create hello four files **main.c**, **remote\_monitoring.c**, **remote\_monitoring.h**, and **CMakeLists.txt**.</span></span>
- <span data-ttu-id="60ee7-121">Hozzon létre nevű **parson**.</span><span class="sxs-lookup"><span data-stu-id="60ee7-121">Create folder called **parson**.</span></span>

<span data-ttu-id="60ee7-122">Hello fájlok másolása **parson.c** és **parson.h** hello Parson tárház helyi másolatát a hello **távoli\_figyelési/parson** mappa.</span><span class="sxs-lookup"><span data-stu-id="60ee7-122">Copy hello files **parson.c** and **parson.h** from your local copy of hello Parson repository into hello **remote\_monitoring/parson** folder.</span></span>

<span data-ttu-id="60ee7-123">Egy szövegszerkesztőben nyissa meg a hello **távoli\_monitoring.c** fájlt.</span><span class="sxs-lookup"><span data-stu-id="60ee7-123">In a text editor, open hello **remote\_monitoring.c** file.</span></span> <span data-ttu-id="60ee7-124">Adja hozzá a következő hello `#include` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="60ee7-124">Add hello following `#include` statements:</span></span>
   
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

## <a name="call-hello-remotemonitoringrun-function"></a><span data-ttu-id="60ee7-125">Távoli eljáráshívás hello\_figyelési\_függvény futtatása</span><span class="sxs-lookup"><span data-stu-id="60ee7-125">Call hello remote\_monitoring\_run function</span></span>
<span data-ttu-id="60ee7-126">Egy szövegszerkesztőben nyissa meg a hello **remote_monitoring.h** fájlt.</span><span class="sxs-lookup"><span data-stu-id="60ee7-126">In a text editor, open hello **remote_monitoring.h** file.</span></span> <span data-ttu-id="60ee7-127">Adja hozzá a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="60ee7-127">Add hello following code:</span></span>

```
void remote_monitoring_run(void);
```

<span data-ttu-id="60ee7-128">Egy szövegszerkesztőben nyissa meg a hello **main.c** fájlt.</span><span class="sxs-lookup"><span data-stu-id="60ee7-128">In a text editor, open hello **main.c** file.</span></span> <span data-ttu-id="60ee7-129">Adja hozzá a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="60ee7-129">Add hello following code:</span></span>

```
#include "remote_monitoring.h"

int main(void)
{
    remote_monitoring_run();

    return 0;
}
```

## <a name="build-and-run-hello-application"></a><span data-ttu-id="60ee7-130">Hozza létre és hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="60ee7-130">Build and run hello application</span></span>
<span data-ttu-id="60ee7-131">hello következő lépések bemutatják, hogyan toouse *CMake* toobuild az ügyfélalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="60ee7-131">hello following steps describe how toouse *CMake* toobuild your client application.</span></span>

1. <span data-ttu-id="60ee7-132">Egy szövegszerkesztőben nyissa meg a hello **CMakeLists.txt** hello fájlban **remote_monitoring** mappa.</span><span class="sxs-lookup"><span data-stu-id="60ee7-132">In a text editor, open hello **CMakeLists.txt** file in hello **remote_monitoring** folder.</span></span>

1. <span data-ttu-id="60ee7-133">Adja hozzá a következő utasításokat toodefine hogyan hello toobuild az ügyfélalkalmazás:</span><span class="sxs-lookup"><span data-stu-id="60ee7-133">Add hello following instructions toodefine how toobuild your client application:</span></span>
   
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
1. <span data-ttu-id="60ee7-134">A hello **remote_monitoring** mappa, hozzon létre egy mappát toostore hello *ellenőrizze* fájlok adott CMake állít elő, és futtassa a hello **cmake** és **Ellenőrizze** parancsok az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="60ee7-134">In hello **remote_monitoring** folder, create a folder toostore hello *make* files that CMake generates and then run hello **cmake** and **make** commands as follows:</span></span>
   
    ```
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

1. <span data-ttu-id="60ee7-135">Hello ügyfélalkalmazás futtatása, és telemetriai tooIoT Hub küldeni:</span><span class="sxs-lookup"><span data-stu-id="60ee7-135">Run hello client application and send telemetry tooIoT Hub:</span></span>
   
    ```
    ./sample_app
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

