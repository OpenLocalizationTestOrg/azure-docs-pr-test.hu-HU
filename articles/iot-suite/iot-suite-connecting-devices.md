---
title: "Csatlakozás egy eszközt a Windows C |} Microsoft Docs"
description: "Eszköz csatlakoztatása az Azure IoT Suite előre konfigurált távoli figyelési megoldást a Windows rendszeren futó C alkalmazással ismerteti."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 34e39a58-2434-482c-b3fa-29438a0c05e8
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: d222bcbd64f288d4091acb0ecd2922b9ceee57e5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-windows"></a><span data-ttu-id="c49e2-103">Csatlakoztassa az eszközt a távoli felügyeleti előkonfigurált megoldás (Windows)</span><span class="sxs-lookup"><span data-stu-id="c49e2-103">Connect your device to the remote monitoring preconfigured solution (Windows)</span></span>
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-c-sample-solution-on-windows"></a><span data-ttu-id="c49e2-104">C minta megoldás létrehozása a Windows rendszeren</span><span class="sxs-lookup"><span data-stu-id="c49e2-104">Create a C sample solution on Windows</span></span>
<span data-ttu-id="c49e2-105">A következő lépések bemutatják a hozzon létre egy ügyfélalkalmazást, amely kommunikál a távoli felügyeleti előkonfigurált megoldás.</span><span class="sxs-lookup"><span data-stu-id="c49e2-105">The following steps show you how to create a client application that communicates with the remote monitoring preconfigured solution.</span></span> <span data-ttu-id="c49e2-106">Ez az alkalmazás a C és beépített, és futtassa a Windows.</span><span class="sxs-lookup"><span data-stu-id="c49e2-106">This application is written in C and built and run on Windows.</span></span>

<span data-ttu-id="c49e2-107">Hozzon létre egy alapszintű projektet a Visual Studio 2015-öt vagy a Visual Studio 2017, és az IoT Hub eszköz ügyfél NuGet-csomagok hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="c49e2-107">Create a starter project in Visual Studio 2015 or Visual Studio 2017 and add the IoT Hub device client NuGet packages:</span></span>

1. <span data-ttu-id="c49e2-108">A Visual Studio, hozzon létre egy C-konzolalkalmazást a Visual C++ használatával **Win32 Konzolalkalmazás** sablont.</span><span class="sxs-lookup"><span data-stu-id="c49e2-108">In Visual Studio, create a C console application using the Visual C++ **Win32 Console Application** template.</span></span> <span data-ttu-id="c49e2-109">Nevet a projektnek **RMDevice**.</span><span class="sxs-lookup"><span data-stu-id="c49e2-109">Name the project **RMDevice**.</span></span>
2. <span data-ttu-id="c49e2-110">Az a **Alkalmazásbeállítások** lapját a **Win32 alkalmazás varázsló**, ügyeljen arra, hogy **Konzolalkalmazás** van kiválasztva, és törölje a jelet **Precompiled fejléc** és **biztonságos fejlesztési Életciklussal (SDL) ellenőrzi**.</span><span class="sxs-lookup"><span data-stu-id="c49e2-110">On the **Application Settings** page in the **Win32 Application Wizard**, ensure that **Console application** is selected, and uncheck **Precompiled header** and **Security Development Lifecycle (SDL) checks**.</span></span>
3. <span data-ttu-id="c49e2-111">A **Megoldáskezelőben**, a fájlok stdafx.h, targetver.h és stdafx.cpp törléséhez.</span><span class="sxs-lookup"><span data-stu-id="c49e2-111">In **Solution Explorer**, delete the files stdafx.h, targetver.h, and stdafx.cpp.</span></span>
4. <span data-ttu-id="c49e2-112">A **Megoldáskezelőben**, nevezze át a fájlt RMDevice.cpp RMDevice.c.</span><span class="sxs-lookup"><span data-stu-id="c49e2-112">In **Solution Explorer**, rename the file RMDevice.cpp to RMDevice.c.</span></span>
5. <span data-ttu-id="c49e2-113">A **Megoldáskezelőben**, kattintson a jobb gombbal a **RMDevice** projektre, majd kattintson **kezelése NuGet-csomagok**.</span><span class="sxs-lookup"><span data-stu-id="c49e2-113">In **Solution Explorer**, right-click on the **RMDevice** project and then click **Manage NuGet packages**.</span></span> <span data-ttu-id="c49e2-114">Kattintson a **Tallózás**, majd keresse meg, és telepítse a következő NuGet-csomagok:</span><span class="sxs-lookup"><span data-stu-id="c49e2-114">Click **Browse**, then search for and install the following NuGet packages:</span></span>
   
   * <span data-ttu-id="c49e2-115">Microsoft.Azure.IoTHub.Serializer</span><span class="sxs-lookup"><span data-stu-id="c49e2-115">Microsoft.Azure.IoTHub.Serializer</span></span>
   * <span data-ttu-id="c49e2-116">Microsoft.Azure.IoTHub.IoTHubClient</span><span class="sxs-lookup"><span data-stu-id="c49e2-116">Microsoft.Azure.IoTHub.IoTHubClient</span></span>
   * <span data-ttu-id="c49e2-117">Microsoft.Azure.IoTHub.MqttTransport</span><span class="sxs-lookup"><span data-stu-id="c49e2-117">Microsoft.Azure.IoTHub.MqttTransport</span></span>
6. <span data-ttu-id="c49e2-118">A **Solution Explorer**, kattintson a jobb gombbal a **RMDevice** projektre, majd kattintson **tulajdonságok** nyissa meg a projektet a **tulajdonságlapjain** a párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c49e2-118">In **Solution Explorer**, right-click on the **RMDevice** project and then click **Properties** to open the project's **Property Pages** dialog box.</span></span> <span data-ttu-id="c49e2-119">További információkért lásd: [beállításának Visual C++ projekt tulajdonságai][lnk-c-project-properties].</span><span class="sxs-lookup"><span data-stu-id="c49e2-119">For details, see [Setting Visual C++ Project Properties][lnk-c-project-properties].</span></span> 
7. <span data-ttu-id="c49e2-120">Kattintson a **Linker** mappát, majd kattintson a **bemeneti** tulajdonságlapján.</span><span class="sxs-lookup"><span data-stu-id="c49e2-120">Click the **Linker** folder, then click the **Input** property page.</span></span>
8. <span data-ttu-id="c49e2-121">Adja hozzá **crypt32.lib** számára a **további függőségek** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="c49e2-121">Add **crypt32.lib** to the **Additional Dependencies** property.</span></span> <span data-ttu-id="c49e2-122">Kattintson a **OK** , majd **OK** újra mentésére a projekt tulajdonságok értékeit.</span><span class="sxs-lookup"><span data-stu-id="c49e2-122">Click **OK** and then **OK** again to save the project property values.</span></span>

<span data-ttu-id="c49e2-123">A Parson JSON könyvtár hozzáadása a **RMDevice** projektre, és adja hozzá a szükséges `#include` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="c49e2-123">Add the Parson JSON library to the **RMDevice** project and add the required `#include` statements:</span></span>

1. <span data-ttu-id="c49e2-124">A megfelelő mappát a számítógépén klónozza a Parson GitHub-tárházban, a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="c49e2-124">In a suitable folder on your computer, clone the Parson GitHub repository using the following command:</span></span>

    ```
    git clone https://github.com/kgabis/parson.git
    ```

1. <span data-ttu-id="c49e2-125">A parson.h és parson.c fájlok másolását a Parson tárház helyi példányát a **RMDevice** projekt mappájából.</span><span class="sxs-lookup"><span data-stu-id="c49e2-125">Copy the parson.h and parson.c files from the local copy of the Parson repository to your **RMDevice** project folder.</span></span>

1. <span data-ttu-id="c49e2-126">A Visual Studióban, kattintson a jobb gombbal a **RMDevice** projektre, kattintson **Hozzáadás**, és kattintson a **meglévő cikk**.</span><span class="sxs-lookup"><span data-stu-id="c49e2-126">In Visual Studio, right-click the **RMDevice** project, click **Add**, and then click **Existing Item**.</span></span>

1. <span data-ttu-id="c49e2-127">Az a **meglévő elem hozzáadása** párbeszédpanelen válassza a parson.h és parson.c-fájlok a **RMDevice** projekt mappájából.</span><span class="sxs-lookup"><span data-stu-id="c49e2-127">In the **Add Existing Item** dialog, select the parson.h and parson.c files in the **RMDevice** project folder.</span></span> <span data-ttu-id="c49e2-128">Kattintson a **Hozzáadás** két fájlt hozzáadása a projekthez.</span><span class="sxs-lookup"><span data-stu-id="c49e2-128">Then click **Add** to add these two files to your project.</span></span>

1. <span data-ttu-id="c49e2-129">A Visual Studióban nyissa meg a RMDevice.c fájlt.</span><span class="sxs-lookup"><span data-stu-id="c49e2-129">In Visual Studio, open the RMDevice.c file.</span></span> <span data-ttu-id="c49e2-130">Cserélje le a meglévő `#include` utasítások a következő kóddal:</span><span class="sxs-lookup"><span data-stu-id="c49e2-130">Replace the existing `#include` statements with the following code:</span></span>
   
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

    > [!NOTE]
    > <span data-ttu-id="c49e2-131">Most már ellenőrizheti, hogy a projekt rendelkezik-e a helyes függőség az épület beállítása.</span><span class="sxs-lookup"><span data-stu-id="c49e2-131">Now you can verify that your project has the correct dependencies set up by building it.</span></span>

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-the-sample"></a><span data-ttu-id="c49e2-132">Hozza létre, és futtathatja a</span><span class="sxs-lookup"><span data-stu-id="c49e2-132">Build and run the sample</span></span>

<span data-ttu-id="c49e2-133">Adja hozzá a meghívni kívánt kódot a **távoli\_figyelési\_futtatása** működéséhez majd összeállítása, és futtassa az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c49e2-133">Add code to invoke the **remote\_monitoring\_run** function and then build and run the device application.</span></span>

1. <span data-ttu-id="c49e2-134">Cserélje le a **fő** függvény hívása a következő kóddal a **távoli\_figyelési\_futtatása** függvény:</span><span class="sxs-lookup"><span data-stu-id="c49e2-134">Replace the **main** function with following code to invoke the **remote\_monitoring\_run** function:</span></span>
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. <span data-ttu-id="c49e2-135">Kattintson a **Build** , majd **megoldás fordítása** hozható létre az eszköz alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c49e2-135">Click **Build** and then **Build Solution** to build the device application.</span></span>

1. <span data-ttu-id="c49e2-136">A **Megoldáskezelőben**, kattintson a jobb gombbal a **RMDevice** projektre, kattintson **Debug**, és kattintson a **Start új példány** a minta futtatásához.</span><span class="sxs-lookup"><span data-stu-id="c49e2-136">In **Solution Explorer**, right-click the **RMDevice** project, click **Debug**, and then click **Start new instance** to run the sample.</span></span> <span data-ttu-id="c49e2-137">A konzol üzeneteket jelenít meg, az alkalmazás minta telemetriai adatokat küld az előkonfigurált megoldás, a megoldás irányítópultjának beállítani kívánt tulajdonságértékek kap, és válaszol-e a megoldás irányítópultja metódusokra.</span><span class="sxs-lookup"><span data-stu-id="c49e2-137">The console displays messages as the application sends sample telemetry to the preconfigured solution, receives desired property values set in the solution dashboard, and responds to methods invoked from the solution dashboard.</span></span>

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx
