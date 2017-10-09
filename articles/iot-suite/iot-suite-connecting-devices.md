---
title: "egy eszköz a Windows C használatával aaaConnect |} Microsoft Docs"
description: "Ismerteti, hogyan tooconnect egy eszköz toohello Azure IoT Suite előre konfigurált távoli figyelési megoldást igényelnek a C Windows rendszeren futó alkalmazást használ."
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
ms.openlocfilehash: 51041e0cec113a5cfa006ab2276096baf928eef5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-device-toohello-remote-monitoring-preconfigured-solution-windows"></a><span data-ttu-id="53e8e-103">Csatlakozás az eszköz toohello távoli felügyeleti előkonfigurált megoldás (Windows)</span><span class="sxs-lookup"><span data-stu-id="53e8e-103">Connect your device toohello remote monitoring preconfigured solution (Windows)</span></span>
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-c-sample-solution-on-windows"></a><span data-ttu-id="53e8e-104">C minta megoldás létrehozása a Windows rendszeren</span><span class="sxs-lookup"><span data-stu-id="53e8e-104">Create a C sample solution on Windows</span></span>
<span data-ttu-id="53e8e-105">hello lépések bemutatják, hogyan toocreate hello távoli megfigyelési kommunikáló ügyfélalkalmazás előre konfigurált megoldás.</span><span class="sxs-lookup"><span data-stu-id="53e8e-105">hello following steps show you how toocreate a client application that communicates with hello remote monitoring preconfigured solution.</span></span> <span data-ttu-id="53e8e-106">Ez az alkalmazás a C és beépített, és futtassa a Windows.</span><span class="sxs-lookup"><span data-stu-id="53e8e-106">This application is written in C and built and run on Windows.</span></span>

<span data-ttu-id="53e8e-107">Hozzon létre egy alapszintű projektet a Visual Studio 2015-öt vagy a Visual Studio 2017 és hello IoT Hub eszköz ügyfél NuGet-csomagok hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="53e8e-107">Create a starter project in Visual Studio 2015 or Visual Studio 2017 and add hello IoT Hub device client NuGet packages:</span></span>

1. <span data-ttu-id="53e8e-108">A Visual Studio, a Visual C++ hello segítségével C Konzolalkalmazás létrehozása **Win32 Konzolalkalmazás** sablont.</span><span class="sxs-lookup"><span data-stu-id="53e8e-108">In Visual Studio, create a C console application using hello Visual C++ **Win32 Console Application** template.</span></span> <span data-ttu-id="53e8e-109">Név hello projekt **RMDevice**.</span><span class="sxs-lookup"><span data-stu-id="53e8e-109">Name hello project **RMDevice**.</span></span>
2. <span data-ttu-id="53e8e-110">A hello **Alkalmazásbeállítások** hello lap **Win32 alkalmazás varázsló**, ügyeljen arra, hogy **Konzolalkalmazás** van kiválasztva, és törölje a jelet **Precompiled fejléc** és **biztonságos fejlesztési Életciklussal (SDL) ellenőrzi**.</span><span class="sxs-lookup"><span data-stu-id="53e8e-110">On hello **Application Settings** page in hello **Win32 Application Wizard**, ensure that **Console application** is selected, and uncheck **Precompiled header** and **Security Development Lifecycle (SDL) checks**.</span></span>
3. <span data-ttu-id="53e8e-111">A **Megoldáskezelőben**, hello fájlok stdafx.h, targetver.h és stdafx.cpp törléséhez.</span><span class="sxs-lookup"><span data-stu-id="53e8e-111">In **Solution Explorer**, delete hello files stdafx.h, targetver.h, and stdafx.cpp.</span></span>
4. <span data-ttu-id="53e8e-112">A **Megoldáskezelőben**, nevezze át a hello fájl RMDevice.cpp tooRMDevice.c.</span><span class="sxs-lookup"><span data-stu-id="53e8e-112">In **Solution Explorer**, rename hello file RMDevice.cpp tooRMDevice.c.</span></span>
5. <span data-ttu-id="53e8e-113">A **Megoldáskezelőben**, kattintson a jobb gombbal a hello **RMDevice** projektre, majd kattintson **kezelése NuGet-csomagok**.</span><span class="sxs-lookup"><span data-stu-id="53e8e-113">In **Solution Explorer**, right-click on hello **RMDevice** project and then click **Manage NuGet packages**.</span></span> <span data-ttu-id="53e8e-114">Kattintson a **Tallózás**, majd keresse meg, és telepítse a következő NuGet-csomagok hello:</span><span class="sxs-lookup"><span data-stu-id="53e8e-114">Click **Browse**, then search for and install hello following NuGet packages:</span></span>
   
   * <span data-ttu-id="53e8e-115">Microsoft.Azure.IoTHub.Serializer</span><span class="sxs-lookup"><span data-stu-id="53e8e-115">Microsoft.Azure.IoTHub.Serializer</span></span>
   * <span data-ttu-id="53e8e-116">Microsoft.Azure.IoTHub.IoTHubClient</span><span class="sxs-lookup"><span data-stu-id="53e8e-116">Microsoft.Azure.IoTHub.IoTHubClient</span></span>
   * <span data-ttu-id="53e8e-117">Microsoft.Azure.IoTHub.MqttTransport</span><span class="sxs-lookup"><span data-stu-id="53e8e-117">Microsoft.Azure.IoTHub.MqttTransport</span></span>
6. <span data-ttu-id="53e8e-118">A **Megoldáskezelőben**, kattintson a jobb gombbal a hello **RMDevice** projektre, majd kattintson **tulajdonságok** tooopen hello projekt **tulajdonságlapjain**párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="53e8e-118">In **Solution Explorer**, right-click on hello **RMDevice** project and then click **Properties** tooopen hello project's **Property Pages** dialog box.</span></span> <span data-ttu-id="53e8e-119">További információkért lásd: [beállításának Visual C++ projekt tulajdonságai][lnk-c-project-properties].</span><span class="sxs-lookup"><span data-stu-id="53e8e-119">For details, see [Setting Visual C++ Project Properties][lnk-c-project-properties].</span></span> 
7. <span data-ttu-id="53e8e-120">Hello kattintson **Linker** mappát, majd kattintson a hello **bemeneti** tulajdonságlapján.</span><span class="sxs-lookup"><span data-stu-id="53e8e-120">Click hello **Linker** folder, then click hello **Input** property page.</span></span>
8. <span data-ttu-id="53e8e-121">Adja hozzá **crypt32.lib** toohello **további függőségek** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="53e8e-121">Add **crypt32.lib** toohello **Additional Dependencies** property.</span></span> <span data-ttu-id="53e8e-122">Kattintson a **OK** , majd **OK** újra toosave hello tulajdonság értékek.</span><span class="sxs-lookup"><span data-stu-id="53e8e-122">Click **OK** and then **OK** again toosave hello project property values.</span></span>

<span data-ttu-id="53e8e-123">Adja hozzá a hello Parson JSON könyvtár toohello **RMDevice** projektre, majd adja hozzá a szükséges hello `#include` utasításokat:</span><span class="sxs-lookup"><span data-stu-id="53e8e-123">Add hello Parson JSON library toohello **RMDevice** project and add hello required `#include` statements:</span></span>

1. <span data-ttu-id="53e8e-124">A megfelelő mappát a számítógépén klónozni hello Parson GitHub-tárházban hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="53e8e-124">In a suitable folder on your computer, clone hello Parson GitHub repository using hello following command:</span></span>

    ```
    git clone https://github.com/kgabis/parson.git
    ```

1. <span data-ttu-id="53e8e-125">Hello parson.h és parson.c fájlok másolása helyi másolatot hello hello Parson tárház tooyour **RMDevice** projekt mappájából.</span><span class="sxs-lookup"><span data-stu-id="53e8e-125">Copy hello parson.h and parson.c files from hello local copy of hello Parson repository tooyour **RMDevice** project folder.</span></span>

1. <span data-ttu-id="53e8e-126">A Visual Studióban, kattintson a jobb gombbal hello **RMDevice** projektre, kattintson **Hozzáadás**, és kattintson a **meglévő cikk**.</span><span class="sxs-lookup"><span data-stu-id="53e8e-126">In Visual Studio, right-click hello **RMDevice** project, click **Add**, and then click **Existing Item**.</span></span>

1. <span data-ttu-id="53e8e-127">A hello **meglévő elem hozzáadása** párbeszédpanelen jelölje be hello parson.h és parson.c fájlok hello **RMDevice** projekt mappájából.</span><span class="sxs-lookup"><span data-stu-id="53e8e-127">In hello **Add Existing Item** dialog, select hello parson.h and parson.c files in hello **RMDevice** project folder.</span></span> <span data-ttu-id="53e8e-128">Kattintson a **Hozzáadás** tooadd ezek két fájlt tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="53e8e-128">Then click **Add** tooadd these two files tooyour project.</span></span>

1. <span data-ttu-id="53e8e-129">A Visual Studióban nyissa meg a hello RMDevice.c fájlt.</span><span class="sxs-lookup"><span data-stu-id="53e8e-129">In Visual Studio, open hello RMDevice.c file.</span></span> <span data-ttu-id="53e8e-130">Lecseréli a meglévő hello `#include` utasítások hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="53e8e-130">Replace hello existing `#include` statements with hello following code:</span></span>
   
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
    > <span data-ttu-id="53e8e-131">Most már ellenőrizheti, hogy rendelkezik-e a projekt hello helyes függőség az épület beállítása.</span><span class="sxs-lookup"><span data-stu-id="53e8e-131">Now you can verify that your project has hello correct dependencies set up by building it.</span></span>

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-hello-sample"></a><span data-ttu-id="53e8e-132">Hozza létre és futtasson hello mintát</span><span class="sxs-lookup"><span data-stu-id="53e8e-132">Build and run hello sample</span></span>

<span data-ttu-id="53e8e-133">Adja hozzá a kódot tooinvoke hello **távoli\_figyelési\_futtatása** funkciót, majd létre és hello eszköz alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="53e8e-133">Add code tooinvoke hello **remote\_monitoring\_run** function and then build and run hello device application.</span></span>

1. <span data-ttu-id="53e8e-134">Cserélje le a hello **fő** függvényt a következő kód tooinvoke hello **távoli\_figyelési\_futtatása** függvény:</span><span class="sxs-lookup"><span data-stu-id="53e8e-134">Replace hello **main** function with following code tooinvoke hello **remote\_monitoring\_run** function:</span></span>
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. <span data-ttu-id="53e8e-135">Kattintson a **Build** , majd **megoldás fordítása** toobuild hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="53e8e-135">Click **Build** and then **Build Solution** toobuild hello device application.</span></span>

1. <span data-ttu-id="53e8e-136">A **Megoldáskezelőben**, kattintson a jobb gombbal hello **RMDevice** projektre, kattintson **Debug**, és kattintson a **Start új példány** toorun hello minta.</span><span class="sxs-lookup"><span data-stu-id="53e8e-136">In **Solution Explorer**, right-click hello **RMDevice** project, click **Debug**, and then click **Start new instance** toorun hello sample.</span></span> <span data-ttu-id="53e8e-137">hello konzol üzeneteket jelenít meg, mint hello alkalmazás elküldi minta telemetriai toohello előre konfigurált megoldás, hello megoldás irányítópultja beállítani kívánt tulajdonságértékek kap, és meghívni a hello megoldás irányítópultja toomethods válaszol.</span><span class="sxs-lookup"><span data-stu-id="53e8e-137">hello console displays messages as hello application sends sample telemetry toohello preconfigured solution, receives desired property values set in hello solution dashboard, and responds toomethods invoked from hello solution dashboard.</span></span>

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx
