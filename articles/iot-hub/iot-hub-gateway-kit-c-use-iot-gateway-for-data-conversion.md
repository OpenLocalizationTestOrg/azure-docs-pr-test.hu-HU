---
title: "az IoT-átjárón Azure IoT oldala aaaData átalakítás |} Microsoft Docs"
description: "Az IoT átjáró tooconvert hello formátum használata az érzékelők adatstreamének keresztül egy testreszabott modul Azure IoT szélétől."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT-átjáró adatok átalakítása, iot átjáró adatok átalakítása"
ms.assetid: 75f2573d-500b-4405-bff7-61021c4c3500
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/25/2017
ms.author: xshi
ms.openlocfilehash: ae94b1f96f36dfcb4f77fadc0ece3cff3d0bba91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-iot-gateway-for-sensor-data-transformation-with-azure-iot-edge"></a><span data-ttu-id="457bc-104">Az IoT-átjáró használata az érzékelő adatokat átalakításhoz Azure IoT oldala</span><span class="sxs-lookup"><span data-stu-id="457bc-104">Use IoT gateway for sensor data transformation with Azure IoT Edge</span></span>

> [!NOTE]
> <span data-ttu-id="457bc-105">Ez az oktatóanyag megkezdése előtt győződjön meg arról megismerte a befejezett hello során tapasztalatokat követő sorrendben:</span><span class="sxs-lookup"><span data-stu-id="457bc-105">Before you start this tutorial, make sure you’ve completed hello following lessons in sequence:</span></span>
> * [<span data-ttu-id="457bc-106">Az Intel NUC beállítása IoT-átjáróként</span><span class="sxs-lookup"><span data-stu-id="457bc-106">Set up Intel NUC as an IoT gateway</span></span>](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
> * [<span data-ttu-id="457bc-107">Az IoT átjáró tooconnect dolgot toohello felhő - SensorTag tooAzure IoT Hub használata</span><span class="sxs-lookup"><span data-stu-id="457bc-107">Use IoT gateway tooconnect things toohello cloud - SensorTag tooAzure IoT Hub</span></span>](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)

<span data-ttu-id="457bc-108">Egy egy Iot-átjáró célja tooprocess gyűjtött adatok toohello felhő elküldése előtt.</span><span class="sxs-lookup"><span data-stu-id="457bc-108">One purpose of an Iot gateway is tooprocess collected data before sending it toohello cloud.</span></span> <span data-ttu-id="457bc-109">Az Azure IoT peremhálózati modulok, amely lehet létrehozott és összeállított tooform hello adatfeldolgozási munkafolyamat vezet be.</span><span class="sxs-lookup"><span data-stu-id="457bc-109">Azure IoT Edge introduces modules which can be created and assembled tooform hello data processing workflow.</span></span> <span data-ttu-id="457bc-110">Egy modul üzenetet kap, néhány műveletet végez, és majd helyezze az egyéb modulok tooprocess.</span><span class="sxs-lookup"><span data-stu-id="457bc-110">A module receives a message, performs some action on it, and then move it on for other modules tooprocess.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="457bc-111">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="457bc-111">What you learn</span></span>

<span data-ttu-id="457bc-112">Megtudhatja, hogyan toocreate egy modul tooconvert a hello SensorTag egy másik formátumú fájlba érkező üzenetek.</span><span class="sxs-lookup"><span data-stu-id="457bc-112">You learn how toocreate a module tooconvert messages from hello SensorTag into a different format.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="457bc-113">Mit</span><span class="sxs-lookup"><span data-stu-id="457bc-113">What you do</span></span>

* <span data-ttu-id="457bc-114">Hozzon létre egy modul tooconvert egy fogadott üzenet hello .JSON kiterjesztésű formátumba.</span><span class="sxs-lookup"><span data-stu-id="457bc-114">Create a module tooconvert a received message into hello .json format.</span></span>
* <span data-ttu-id="457bc-115">Fordítási hello modul.</span><span class="sxs-lookup"><span data-stu-id="457bc-115">Compile hello module.</span></span>
* <span data-ttu-id="457bc-116">Adja hozzá a hello modul toohello BLA mintaalkalmazás Azure IoT szélétől.</span><span class="sxs-lookup"><span data-stu-id="457bc-116">Add hello module toohello BLE sample application from Azure IoT Edge.</span></span>
* <span data-ttu-id="457bc-117">Futtassa a hello mintaalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="457bc-117">Run hello sample application.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="457bc-118">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="457bc-118">What you need</span></span>

* <span data-ttu-id="457bc-119">a következő sorrendben befejeződött oktatóanyagok hello:</span><span class="sxs-lookup"><span data-stu-id="457bc-119">hello following tutorials completed in sequence:</span></span>
  * [<span data-ttu-id="457bc-120">Az Intel NUC beállítása IoT-átjáróként</span><span class="sxs-lookup"><span data-stu-id="457bc-120">Set up Intel NUC as an IoT gateway</span></span>](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md)
  * [<span data-ttu-id="457bc-121">Az IoT átjáró tooconnect dolgot toohello felhő - SensorTag tooAzure IoT Hub használata</span><span class="sxs-lookup"><span data-stu-id="457bc-121">Use IoT gateway tooconnect things toohello cloud - SensorTag tooAzure IoT Hub</span></span>](iot-hub-gateway-kit-c-iot-gateway-connect-device-to-cloud.md)
* <span data-ttu-id="457bc-122">Egy SSH-ügyfél rendszert futtató számítógépen.</span><span class="sxs-lookup"><span data-stu-id="457bc-122">An SSH client that runs on your host computer.</span></span> <span data-ttu-id="457bc-123">A puTTY Windows ajánlott.</span><span class="sxs-lookup"><span data-stu-id="457bc-123">PuTTY is recommended on Windows.</span></span> <span data-ttu-id="457bc-124">Linux- és macOS egy SSH-ügyfél már rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="457bc-124">Linux and macOS already come with an SSH client.</span></span>
* <span data-ttu-id="457bc-125">hello IP-cím és hello felhasználónév és jelszó tooaccess hello átjáró hello SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="457bc-125">hello IP address and hello username and password tooaccess hello gateway from hello SSH client.</span></span>
* <span data-ttu-id="457bc-126">Az internethez.</span><span class="sxs-lookup"><span data-stu-id="457bc-126">An Internet connection.</span></span>

## <a name="create-a-module"></a><span data-ttu-id="457bc-127">Modul létrehozása</span><span class="sxs-lookup"><span data-stu-id="457bc-127">Create a module</span></span>

1. <span data-ttu-id="457bc-128">Hello állomáson hello SSH-ügyfél, és csatlakozzon a toohello IoT átjáró.</span><span class="sxs-lookup"><span data-stu-id="457bc-128">On hello host computer, run hello SSH client and connect toohello IoT gateway.</span></span>
1. <span data-ttu-id="457bc-129">Hello forrásfájljaihoz hello átalakítás modulnak a Githubon toohello kezdőkönyvtárát hello IoT átjáró klónozni hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="457bc-129">Clone hello source files of hello conversion module from GitHub toohello home directory of hello IoT gateway by running hello following commands:</span></span>

   ```bash
   cd ~
   git clone https://github.com/Azure-Samples/iot-hub-c-intel-nuc-gateway-customized-module.git
   ```

   <span data-ttu-id="457bc-130">Ez az Azure biztonsági hello C programozási nyelven írt natív modul.</span><span class="sxs-lookup"><span data-stu-id="457bc-130">This is a native Azure Edge module written in hello C programming language.</span></span> <span data-ttu-id="457bc-131">hello modul fogadott üzenetek hello formátumának alakítja hello egyet a következő:</span><span class="sxs-lookup"><span data-stu-id="457bc-131">hello module converts hello format of received messages into hello following one:</span></span>

   ```json
   {"deviceId": "Intel NUC Gateway", "messageId": 0, "temperature": 0.0}
   ```

## <a name="compile-hello-module"></a><span data-ttu-id="457bc-132">Fordítási hello modul</span><span class="sxs-lookup"><span data-stu-id="457bc-132">Compile hello module</span></span>

<span data-ttu-id="457bc-133">toocompile hello modul, futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="457bc-133">toocompile hello module, run hello following commands:</span></span>

```bash
cd iot-hub-c-intel-nuc-gateway-customized-module/my_module
# change hello build script runnable
chmod 777 build.sh
# remove hello invalid windows character
sed -i -e "s/\r$//" build.sh
# run hello build shell script
./build.sh
```

<span data-ttu-id="457bc-134">Kapott egy `libmy_module.so` fájl hello fordítási befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="457bc-134">You get a `libmy_module.so` file after hello compile is completed.</span></span> <span data-ttu-id="457bc-135">Jegyezze fel a hello fájl abszolút elérési út.</span><span class="sxs-lookup"><span data-stu-id="457bc-135">Make a note of hello absolute path of this file.</span></span>

## <a name="add-hello-module-toohello-ble-sample-application"></a><span data-ttu-id="457bc-136">Adja hozzá a hello modul toohello BLA mintaalkalmazás</span><span class="sxs-lookup"><span data-stu-id="457bc-136">Add hello module toohello BLE sample application</span></span>

1. <span data-ttu-id="457bc-137">Nyissa meg a toohello minták mappa hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="457bc-137">Go toohello samples folder by running hello following command:</span></span>

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples
   ```

1. <span data-ttu-id="457bc-138">Nyissa meg a konfigurációs fájl hello hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="457bc-138">Open hello configuration file by running hello following command:</span></span>

   ```bash
   vi ble_gateway.json
   ```

1. <span data-ttu-id="457bc-139">A modul hozzá lesz adva a következő kód toohello hello beszúrásával `modules` szakasz.</span><span class="sxs-lookup"><span data-stu-id="457bc-139">Add a module by inserting hello following code toohello `modules` section.</span></span>

   ```json
   {
       "name": "MyModule",
       "loader": {
           "name": "native",
           "entrypoint":{
               "module.path": "[Your libmy_module.so path]"
               }
            },
        "args": null
    },
    ```

1. <span data-ttu-id="457bc-140">Cserélje le `[Your libmy_module.so path]` hello kódban hello hello libmy_module.so az abszolút elérési úttal rendelkező "fájl.</span><span class="sxs-lookup"><span data-stu-id="457bc-140">Replace `[Your libmy_module.so path]` in hello code with hello absolute path of hello libmy_module.so\` file.</span></span>
1. <span data-ttu-id="457bc-141">Cserélje le a hello hello kód `links` hello egyet a következő szakaszt:</span><span class="sxs-lookup"><span data-stu-id="457bc-141">Replace hello code in hello `links` section with hello following one:</span></span>

   ```json
   {
       "source": "SensorTag",
       "sink": "MyModule"
   },
   {
       "source": "MyModule",
       "sink": "mapping"
   }
   ```

1. <span data-ttu-id="457bc-142">Nyomja le az `ESC`, majd írja be `:wq` toosave hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="457bc-142">Press `ESC`, and then type `:wq` toosave hello file.</span></span>

## <a name="run-hello-sample-application"></a><span data-ttu-id="457bc-143">Hello mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="457bc-143">Run hello sample application</span></span>

1. <span data-ttu-id="457bc-144">Hello SensorTag bekapcsolása.</span><span class="sxs-lookup"><span data-stu-id="457bc-144">Power on hello SensorTag.</span></span>
1. <span data-ttu-id="457bc-145">Hello SSL_CERT_FILE környezeti változó beállítása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="457bc-145">Set hello SSL_CERT_FILE environment variable by running hello following command:</span></span>

   ```bash
   export SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt
   ```

1. <span data-ttu-id="457bc-146">Futtassa a mintaalkalmazást hello hello hozzáadott modul hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="457bc-146">Run hello sample application with hello added module by running hello following command:</span></span>

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a><span data-ttu-id="457bc-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="457bc-147">Next steps</span></span>

<span data-ttu-id="457bc-148">Sikeresen regisztrálta használata hello IoT átjáró tooconvert hello üzenetét SensorTag hello .JSON kiterjesztésű formátumba.</span><span class="sxs-lookup"><span data-stu-id="457bc-148">You’ve successfully use hello IoT gateway tooconvert hello message from SensorTag into hello .json format.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
