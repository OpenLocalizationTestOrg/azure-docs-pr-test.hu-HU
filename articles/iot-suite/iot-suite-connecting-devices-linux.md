---
title: "Csatlakoztassa a C használatával Linux |} Microsoft Docs"
description: "Eszköz csatlakoztatása az Azure IoT Suite előre konfigurált távoli figyelési megoldást igényelnek a futó Linux C alkalmazással ismerteti."
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
ms.openlocfilehash: 9adbc9cc13f0b4cafa3a3a7703c46f8085b15232
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-linux"></a><span data-ttu-id="0d2e7-103">Csatlakoztassa az eszközt a távoli felügyeleti előkonfigurált megoldás (Linux)</span><span class="sxs-lookup"><span data-stu-id="0d2e7-103">Connect your device to the remote monitoring preconfigured solution (Linux)</span></span>
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-a-sample-c-client-linux"></a><span data-ttu-id="0d2e7-104">Hozza létre, és egy minta C-ügyfél Linux</span><span class="sxs-lookup"><span data-stu-id="0d2e7-104">Build and run a sample C client Linux</span></span>
<span data-ttu-id="0d2e7-105">A következő lépések bemutatják a hozzon létre egy ügyfélalkalmazást, amely kommunikál a távoli felügyeleti előkonfigurált megoldás.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-105">The following steps show you how to create a client application that communicates with the remote monitoring preconfigured solution.</span></span> <span data-ttu-id="0d2e7-106">Ez az alkalmazás a C és beépített és Ubuntu Linux rendszeren futtassa.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-106">This application is written in C and built and run on Ubuntu Linux.</span></span>

<span data-ttu-id="0d2e7-107">A lépések elvégzésével Ubuntu 15.04 vagy 15.10 verzióját futtató eszközök kell.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-107">To complete these steps, you need a device running Ubuntu version 15.04 or 15.10.</span></span> <span data-ttu-id="0d2e7-108">A folytatás előtt telepítse a csomagokat az Ubuntu eszközre, a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="0d2e7-108">Before proceeding, install the prerequisite packages on your Ubuntu device using the following command:</span></span>

```
sudo apt-get install cmake gcc g++
```

## <a name="install-the-client-libraries-on-your-device"></a><span data-ttu-id="0d2e7-109">A klienskódtárak segítségével telepítse az eszközre</span><span class="sxs-lookup"><span data-stu-id="0d2e7-109">Install the client libraries on your device</span></span>
<span data-ttu-id="0d2e7-110">Az Azure IoT Hub klienskódtárak segítségével telepítheti az Ubuntu eszköz használt csomag érhetők el a **apt get** parancsot.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-110">The Azure IoT Hub client libraries are available as a package you can install on your Ubuntu device using the **apt-get** command.</span></span> <span data-ttu-id="0d2e7-111">Az alábbi lépésekkel telepítse a csomagot, amely tartalmazza az IoT-központ ügyfél és a fejléc fájlokat a számítógépre Ubuntu:</span><span class="sxs-lookup"><span data-stu-id="0d2e7-111">Complete the following steps to install the package that contains the IoT Hub client library and header files on your Ubuntu computer:</span></span>

1. <span data-ttu-id="0d2e7-112">A rendszerhéj a AzureIoT tárház hozzáadása a számítógéphez:</span><span class="sxs-lookup"><span data-stu-id="0d2e7-112">In a shell, add the AzureIoT repository to your computer:</span></span>
   
    ```
    sudo add-apt-repository ppa:aziotsdklinux/ppa-azureiot
    sudo apt-get update
    ```
2. <span data-ttu-id="0d2e7-113">Az azure-iot-sdk-c-fejlesztői csomag telepítése</span><span class="sxs-lookup"><span data-stu-id="0d2e7-113">Install the azure-iot-sdk-c-dev package</span></span>
   
    ```
    sudo apt-get install -y azure-iot-sdk-c-dev
    ```

## <a name="install-the-parson-json-parser"></a><span data-ttu-id="0d2e7-114">Telepítse a Parson JSON-elemző</span><span class="sxs-lookup"><span data-stu-id="0d2e7-114">Install the Parson JSON parser</span></span>
<span data-ttu-id="0d2e7-115">Az IoT-központ klienskódtárak segítségével a Parson JSON-elemző használatával elemezni üzenet Payload van jelen.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-115">The IoT Hub client libraries use the Parson JSON parser to parse message payloads.</span></span> <span data-ttu-id="0d2e7-116">A megfelelő mappát a számítógépén klónozza a Parson GitHub-tárházban, a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="0d2e7-116">In a suitable folder on your computer, clone the Parson GitHub repository using the following command:</span></span>

```
git clone https://github.com/kgabis/parson.git
```

## <a name="prepare-your-project"></a><span data-ttu-id="0d2e7-117">Készítse elő a projekthez</span><span class="sxs-lookup"><span data-stu-id="0d2e7-117">Prepare your project</span></span>
<span data-ttu-id="0d2e7-118">A Ubuntu gépen, hozzon létre egy nevű **távoli\_figyelési**.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-118">On your Ubuntu machine, create a folder called **remote\_monitoring**.</span></span> <span data-ttu-id="0d2e7-119">Az a **távoli\_figyelési** mappába:</span><span class="sxs-lookup"><span data-stu-id="0d2e7-119">In the **remote\_monitoring** folder:</span></span>

- <span data-ttu-id="0d2e7-120">Hozza létre a négy fájlokat **main.c**, **távoli\_monitoring.c**, **távoli\_monitoring.h**, és **CMakeLists.txt**.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-120">Create the four files **main.c**, **remote\_monitoring.c**, **remote\_monitoring.h**, and **CMakeLists.txt**.</span></span>
- <span data-ttu-id="0d2e7-121">Hozzon létre nevű **parson**.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-121">Create folder called **parson**.</span></span>

<span data-ttu-id="0d2e7-122">Másolja a fájlokat **parson.c** és **parson.h** Parson összetevőtárházat be helyi másolatát a **távoli\_figyelési/parson** mappa.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-122">Copy the files **parson.c** and **parson.h** from your local copy of the Parson repository into the **remote\_monitoring/parson** folder.</span></span>

<span data-ttu-id="0d2e7-123">Egy szövegszerkesztőben nyissa meg a **távoli\_monitoring.c** fájlt.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-123">In a text editor, open the **remote\_monitoring.c** file.</span></span> <span data-ttu-id="0d2e7-124">Adja hozzá a következő `#include`-utasításokat:</span><span class="sxs-lookup"><span data-stu-id="0d2e7-124">Add the following `#include` statements:</span></span>
   
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

## <a name="call-the-remotemonitoringrun-function"></a><span data-ttu-id="0d2e7-125">A távoli eljáráshívás\_figyelési\_függvény futtatása</span><span class="sxs-lookup"><span data-stu-id="0d2e7-125">Call the remote\_monitoring\_run function</span></span>
<span data-ttu-id="0d2e7-126">Egy szövegszerkesztőben nyissa meg a **remote_monitoring.h** fájlt.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-126">In a text editor, open the **remote_monitoring.h** file.</span></span> <span data-ttu-id="0d2e7-127">Adja hozzá a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="0d2e7-127">Add the following code:</span></span>

```
void remote_monitoring_run(void);
```

<span data-ttu-id="0d2e7-128">Egy szövegszerkesztőben nyissa meg a **main.c** fájlt.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-128">In a text editor, open the **main.c** file.</span></span> <span data-ttu-id="0d2e7-129">Adja hozzá a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="0d2e7-129">Add the following code:</span></span>

```
#include "remote_monitoring.h"

int main(void)
{
    remote_monitoring_run();

    return 0;
}
```

## <a name="build-and-run-the-application"></a><span data-ttu-id="0d2e7-130">Az alkalmazás fordítása és futtatása</span><span class="sxs-lookup"><span data-stu-id="0d2e7-130">Build and run the application</span></span>
<span data-ttu-id="0d2e7-131">Az alábbi lépések bemutatják, hogyan használható *CMake* hozhat létre az ügyfélalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-131">The following steps describe how to use *CMake* to build your client application.</span></span>

1. <span data-ttu-id="0d2e7-132">Egy szövegszerkesztőben nyissa meg a **CMakeLists.txt** fájlt a **remote_monitoring** mappa.</span><span class="sxs-lookup"><span data-stu-id="0d2e7-132">In a text editor, open the **CMakeLists.txt** file in the **remote_monitoring** folder.</span></span>

1. <span data-ttu-id="0d2e7-133">Az alábbi utasítások segítségével meghatározhatja, hogyan hozható létre az ügyfél-alkalmazás hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="0d2e7-133">Add the following instructions to define how to build your client application:</span></span>
   
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
1. <span data-ttu-id="0d2e7-134">Az a **remote_monitoring** mappa, hozzon létre egy mappát tárolásához a *ellenőrizze* CMake hoz létre fájlokat, majd futtassa a **cmake** és **győződjön** parancsok az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="0d2e7-134">In the **remote_monitoring** folder, create a folder to store the *make* files that CMake generates and then run the **cmake** and **make** commands as follows:</span></span>
   
    ```
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

1. <span data-ttu-id="0d2e7-135">Az ügyfélalkalmazás futtatása, és telemetriai adatokat küldhet az IoT hubhoz:</span><span class="sxs-lookup"><span data-stu-id="0d2e7-135">Run the client application and send telemetry to IoT Hub:</span></span>
   
    ```
    ./sample_app
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

