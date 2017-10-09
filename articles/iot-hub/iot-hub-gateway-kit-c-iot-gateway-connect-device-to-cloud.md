---
title: "az IoT-átjáró tooconnect egy eszköz tooAzure IoT-központ aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Intel NUC egy IoT-átjáró tooconnect egy TI SensorTag, és a küldési érzékelő adatokat tooAzure IoT-központ hello felhő."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az IOT-átjáró eszköz toocloud kapcsolódni"
ms.assetid: cb851648-018c-4a7e-860f-b62ed3b493a5
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/25/2017
ms.author: xshi
ms.openlocfilehash: 418af34bf29992d46b76ae59ef548744808664c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-iot-gateway-tooconnect-things-toohello-cloud---sensortag-tooazure-iot-hub"></a><span data-ttu-id="26c78-104">Az IoT átjáró tooconnect dolgot toohello felhő - SensorTag tooAzure IoT Hub használata</span><span class="sxs-lookup"><span data-stu-id="26c78-104">Use IoT gateway tooconnect things toohello cloud - SensorTag tooAzure IoT Hub</span></span>

> [!NOTE]
> <span data-ttu-id="26c78-105">Ez az oktatóanyag megkezdése előtt győződjön meg arról, hogy befejezte [Intel NUC IoT átjáróként beállítva](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md).</span><span class="sxs-lookup"><span data-stu-id="26c78-105">Before you start this tutorial, make sure you’ve completed [Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md).</span></span> <span data-ttu-id="26c78-106">A [Intel NUC IoT átjáróként beállítva](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md), úgy konfigurálja az IoT átjáróként hello Intel NUC eszközt.</span><span class="sxs-lookup"><span data-stu-id="26c78-106">In [Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md), you set up hello Intel NUC device as an IoT gateway.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="26c78-107">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="26c78-107">What you will learn</span></span>

<span data-ttu-id="26c78-108">Megismerheti, hogyan toouse egy IoT-átjáró tooconnect egy Texas eszközök SensorTag (CC2650STK) tooAzure IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="26c78-108">You learn how toouse an IoT gateway tooconnect a Texas Instruments SensorTag (CC2650STK) tooAzure IoT Hub.</span></span> <span data-ttu-id="26c78-109">hello IoT átjáró elküldi a hőmérséklet és a páratartalom során összegyűjtött adatokat hello SensorTag tooAzure IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="26c78-109">hello IoT gateway sends temperature and humidity data collected from hello SensorTag tooAzure IoT Hub.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="26c78-110">Mit fog</span><span class="sxs-lookup"><span data-stu-id="26c78-110">What you will do</span></span>

- <span data-ttu-id="26c78-111">Létrehoz egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="26c78-111">Create an IoT hub.</span></span>
- <span data-ttu-id="26c78-112">Eszköz regisztrálása a hello IoT-központ hello SensorTag.</span><span class="sxs-lookup"><span data-stu-id="26c78-112">Register a device in hello IoT hub for hello SensorTag.</span></span>
- <span data-ttu-id="26c78-113">Hello kapcsolatot az IoT-átjáró hello és hello SensorTag engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="26c78-113">Enable hello connection between hello IoT gateway and hello SensorTag.</span></span>
- <span data-ttu-id="26c78-114">Egy táblázat minta alkalmazás toosend SensorTag adatok tooyour IoT-központ futtatása.</span><span class="sxs-lookup"><span data-stu-id="26c78-114">Run a BLE sample application toosend SensorTag data tooyour IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="26c78-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="26c78-115">What you need</span></span>

- <span data-ttu-id="26c78-116">Az oktatóanyag [Intel NUC IoT átjáróként beállítva](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md) befejeződött a amely Intel NUC IoT átjáróként beállítása.</span><span class="sxs-lookup"><span data-stu-id="26c78-116">Tutorial [Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md) completed in which you set up Intel NUC as an IoT gateway.</span></span>
- * <span data-ttu-id="26c78-117">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="26c78-117">An active Azure subscription.</span></span> <span data-ttu-id="26c78-118">Ha az Azure-fiók nem rendelkezik [hozzon létre egy Azure próbafiókot](https://azure.microsoft.com/free/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="26c78-118">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
- <span data-ttu-id="26c78-119">Egy SSH-ügyfél rendszert futtató számítógépen.</span><span class="sxs-lookup"><span data-stu-id="26c78-119">An SSH client that runs on your host computer.</span></span> <span data-ttu-id="26c78-120">A puTTY Windows ajánlott.</span><span class="sxs-lookup"><span data-stu-id="26c78-120">PuTTY is recommended on Windows.</span></span> <span data-ttu-id="26c78-121">Linux- és macOS egy SSH-ügyfél már rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="26c78-121">Linux and macOS already come with an SSH client.</span></span>
- <span data-ttu-id="26c78-122">hello IP-cím és hello felhasználónév és jelszó tooaccess hello átjáró hello SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="26c78-122">hello IP address and hello username and password tooaccess hello gateway from hello SSH client.</span></span>
- <span data-ttu-id="26c78-123">Az internethez.</span><span class="sxs-lookup"><span data-stu-id="26c78-123">An Internet connection.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

> [!NOTE]
> <span data-ttu-id="26c78-124">Itt az új eszköz regisztrálásához a SensorTag</span><span class="sxs-lookup"><span data-stu-id="26c78-124">Here you register this new device for your SensorTag</span></span>

## <a name="enable-hello-connection-between-hello-iot-gateway-and-hello-sensortag"></a><span data-ttu-id="26c78-125">Hello kapcsolatot az IoT-átjáró hello és hello SensorTag engedélyezése</span><span class="sxs-lookup"><span data-stu-id="26c78-125">Enable hello connection between hello IoT gateway and hello SensorTag</span></span>

<span data-ttu-id="26c78-126">Ebben a szakaszban hajtsa végre a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="26c78-126">In this section, you perform hello following tasks:</span></span>

- <span data-ttu-id="26c78-127">Bluetooth-kapcsolat beolvasása hello SensorTag hello MAC-címét.</span><span class="sxs-lookup"><span data-stu-id="26c78-127">Get hello MAC address of hello SensorTag for Bluetooth connection.</span></span>
- <span data-ttu-id="26c78-128">A hello IoT átjáró toohello SensorTag Bluetooth kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="26c78-128">Initiate a Bluetooth connection from hello IoT gateway toohello SensorTag.</span></span>

### <a name="get-hello-mac-address-of-hello-sensortag-for-bluetooth-connection"></a><span data-ttu-id="26c78-129">Bluetooth-csatlakozási hello MAC-címét hello SensorTag lekérése</span><span class="sxs-lookup"><span data-stu-id="26c78-129">Get hello MAC address of hello SensorTag for Bluetooth connection</span></span>

1. <span data-ttu-id="26c78-130">Hello állomáson hello SSH-ügyfél, és csatlakozzon a toohello IoT átjáró.</span><span class="sxs-lookup"><span data-stu-id="26c78-130">On hello host computer, run hello SSH client and connect toohello IoT gateway.</span></span>
1. <span data-ttu-id="26c78-131">Bluetooth feloldása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="26c78-131">Unblock Bluetooth by running hello following command:</span></span>

   ```bash
   sudo rfkill unblock bluetooth
   ```

1. <span data-ttu-id="26c78-132">Az IoT-átjárón hello hello Bluetooth szolgáltatás elindítása, és írja be a Bluetooth rendszerhéj tooconfigure futtatásával Bluetooth hello a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="26c78-132">Start hello Bluetooth service on hello IoT gateway and enter a Bluetooth shell tooconfigure Bluetooth by running hello following commands:</span></span>

   ```bash
   sudo systemctl start bluetooth
   bluetoothctl
   ```

1. <span data-ttu-id="26c78-133">Hello Bluetooth-vezérlő futtatásával bekapcsolás hello a következő parancsot a következő hello Bluetooth rendszerhéj:</span><span class="sxs-lookup"><span data-stu-id="26c78-133">Power on hello Bluetooth controller by running hello following command at hello Bluetooth shell:</span></span>

   ```bash
   power on
   ```

   ![az IoT-átjárót hello bluetoothctl hello Bluetooth-vezérlő bekapcsolása](./media/iot-hub-iot-gateway-connect-device-to-cloud/8_power-on-bluetooth-controller-at-bluetooth-shell-bluetoothctl.png)

1. <span data-ttu-id="26c78-135">Indítsa el a Bluetooth-eszközök közelben hello a következő parancs futtatásával keresése:</span><span class="sxs-lookup"><span data-stu-id="26c78-135">Start scanning for nearby Bluetooth devices by running hello following command:</span></span>

   ```bash
   scan on
   ```

   ![Bluetooth-eszközök a bluetoothctl közelben vizsgálata](./media/iot-hub-iot-gateway-connect-device-to-cloud/9_start-scan-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

1. <span data-ttu-id="26c78-137">Nyomja le az hello párosítás hello SensorTag gombjára.</span><span class="sxs-lookup"><span data-stu-id="26c78-137">Press hello pairing button on hello SensorTag.</span></span> <span data-ttu-id="26c78-138">zöld hello a hello SensorTag villanás vezetett.</span><span class="sxs-lookup"><span data-stu-id="26c78-138">hello green LED on hello SensorTag flashes.</span></span>
1. <span data-ttu-id="26c78-139">A Bluetooth rendszerhéj hello meg kell jelennie hello SensorTag megtalálható-e.</span><span class="sxs-lookup"><span data-stu-id="26c78-139">At hello Bluetooth shell, you should see hello SensorTag is found.</span></span> <span data-ttu-id="26c78-140">Jegyezze fel a hello hello SensorTag MAC-címét.</span><span class="sxs-lookup"><span data-stu-id="26c78-140">Make a note of hello MAC address of hello SensorTag.</span></span> <span data-ttu-id="26c78-141">Ebben a példában a hello hello SensorTag MAC-címét a `24:71:89:C0:7F:82`.</span><span class="sxs-lookup"><span data-stu-id="26c78-141">In this example, hello MAC address of hello SensorTag is `24:71:89:C0:7F:82`.</span></span>
1. <span data-ttu-id="26c78-142">Kapcsolja ki a hello vizsgálat hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="26c78-142">Turn off hello scan by running hello following command:</span></span>

   ```bash
   scan off
   ```

   ![Állítsa le a közeli bluetoothctl a Bluetooth-eszközök keresése](./media/iot-hub-iot-gateway-connect-device-to-cloud/10_stop-scanning-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

### <a name="initiate-a-bluetooth-connection-from-hello-iot-gateway-toohello-sensortag"></a><span data-ttu-id="26c78-144">Hello IoT átjáró toohello SensorTag a Bluetooth-kapcsolat kezdeményezéséhez</span><span class="sxs-lookup"><span data-stu-id="26c78-144">Initiate a Bluetooth connection from hello IoT gateway toohello SensorTag</span></span>

1. <span data-ttu-id="26c78-145">Csatlakozás toohello SensorTag hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="26c78-145">Connect toohello SensorTag by running hello following command:</span></span>

   ```bash
   connect <MAC address>
   ```

   ![Csatlakozás toohello SensorTag bluetoothctl](./media/iot-hub-iot-gateway-connect-device-to-cloud/11_connect-to-sensortag-at-bluetooth-shell-bluetoothctl.png)

1. <span data-ttu-id="26c78-147">Hello SensorTag megszakítja a kapcsolatot, és zárja be a hello Bluetooth rendszerhéj hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="26c78-147">Disconnect from hello SensorTag and exit hello Bluetooth shell by running hello following commands:</span></span>

   ```bash
   disconnect
   exit
   ```

   ![Válassza le a bluetoothctl SensorTag hello](./media/iot-hub-iot-gateway-connect-device-to-cloud/12_disconnect-from-sensortag-at-bluetooth-shell-bluetoothctl.png)

<span data-ttu-id="26c78-149">Sikeresen engedélyezve van a hello kapcsolat hello SensorTag és hello IoT-átjáró között.</span><span class="sxs-lookup"><span data-stu-id="26c78-149">You've successfully enabled hello connection between hello SensorTag and hello IoT gateway.</span></span>

## <a name="run-a-ble-sample-application-toosend-sensortag-data-tooyour-iot-hub"></a><span data-ttu-id="26c78-150">Egy táblázat minta alkalmazás toosend SensorTag adatok tooyour IoT-központ futtatása</span><span class="sxs-lookup"><span data-stu-id="26c78-150">Run a BLE sample application toosend SensorTag data tooyour IoT hub</span></span>

<span data-ttu-id="26c78-151">Bluetooth alacsony energia (int) mintaalkalmazás hello Azure IoT peremhálózati biztosítja.</span><span class="sxs-lookup"><span data-stu-id="26c78-151">hello Bluetooth Low Energy (BLE) sample application is provided by Azure IoT Edge.</span></span> <span data-ttu-id="26c78-152">hello mintaalkalmazás BLA kapcsolat gyűjti az adatokat, és küldje hello adatok tooyou IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="26c78-152">hello sample application collects data from BLE connection and send hello data tooyou IoT hub.</span></span> <span data-ttu-id="26c78-153">toorun hello mintaalkalmazás kell:</span><span class="sxs-lookup"><span data-stu-id="26c78-153">toorun hello sample application, you need to:</span></span>

1. <span data-ttu-id="26c78-154">Hello mintaalkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="26c78-154">Configure hello sample application.</span></span>
1. <span data-ttu-id="26c78-155">Hello mintaalkalmazás futtatása hello IoT-átjárón.</span><span class="sxs-lookup"><span data-stu-id="26c78-155">Run hello sample application on hello IoT gateway.</span></span>

### <a name="configure-hello-sample-application"></a><span data-ttu-id="26c78-156">Hello mintaalkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="26c78-156">Configure hello sample application</span></span>

1. <span data-ttu-id="26c78-157">Nyissa meg a toohello mappa hello mintaalkalmazásnak hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="26c78-157">Go toohello folder of hello sample application by running hello following command:</span></span>

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples/ble_gateway
   ```

1. <span data-ttu-id="26c78-158">Nyissa meg a konfigurációs fájl hello hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="26c78-158">Open hello configuration file by running hello following command:</span></span>

   ```bash
   vi ble_gateway.json
   ```

1. <span data-ttu-id="26c78-159">Hello konfigurációs fájlt töltse ki a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="26c78-159">In hello configuration file, fill in hello following values:</span></span>

   <span data-ttu-id="26c78-160">**IoTHubName**: hello az IoT hub nevét.</span><span class="sxs-lookup"><span data-stu-id="26c78-160">**IoTHubName**: hello name of your IoT hub.</span></span>

   <span data-ttu-id="26c78-161">**IoTHubSuffix**: hello elsődleges kulcs hello eszköz kapcsolati karakterlánc le feljegyzett az beszerzése IoTHubSuffix.</span><span class="sxs-lookup"><span data-stu-id="26c78-161">**IoTHubSuffix**: Get IoTHubSuffix from hello primary key of hello device connection string that you noted down.</span></span> <span data-ttu-id="26c78-162">Győződjön meg arról, hogy hello elsődleges kulcs hello eszköz kapcsolati karakterlánc beolvasása, az IoT hub kapcsolati karakterlánc az elsődleges kulcsa nem hello.</span><span class="sxs-lookup"><span data-stu-id="26c78-162">Ensure that you get hello primary key of hello device connection string, not hello primary key of your IoT hub connection string.</span></span> <span data-ttu-id="26c78-163">hello formátumban van elsődleges kulcs hello hello eszköz kapcsolati karakterlánc `HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY`.</span><span class="sxs-lookup"><span data-stu-id="26c78-163">hello primary key of hello device connection string is in hello format of `HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY`.</span></span>

   <span data-ttu-id="26c78-164">**Átviteli**: hello alapértelmezett értéke `amqp`.</span><span class="sxs-lookup"><span data-stu-id="26c78-164">**Transport**: hello default value is `amqp`.</span></span> <span data-ttu-id="26c78-165">Ez az érték hello protokoll transpotation során jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="26c78-165">This value shows hello protocol during transpotation.</span></span> <span data-ttu-id="26c78-166">Annak oka az lehet `http`, `amqp`, vagy `mqtt`.</span><span class="sxs-lookup"><span data-stu-id="26c78-166">It could be `http`, `amqp`, or `mqtt`.</span></span>

   <span data-ttu-id="26c78-167">**macAddress**: hello hello le feljegyzett SensorTag MAC-címét.</span><span class="sxs-lookup"><span data-stu-id="26c78-167">**macAddress**: hello MAC address of hello SensorTag that you noted down.</span></span>

   <span data-ttu-id="26c78-168">**deviceID**: hello eszközt az IoT hub létrehozott azonosítója.</span><span class="sxs-lookup"><span data-stu-id="26c78-168">**deviceID**: ID of hello device that you created in your IoT hub.</span></span>

   <span data-ttu-id="26c78-169">**deviceKey**: hello hello eszköz kapcsolati karakterlánc az elsődleges kulcsa.</span><span class="sxs-lookup"><span data-stu-id="26c78-169">**deviceKey**: hello primary key of hello device connection string.</span></span>

   ![Teljes hello konfigurációs fájljának hello BLA mintaalkalmazás](./media/iot-hub-iot-gateway-connect-device-to-cloud/13_edit-config-file-of-ble-sample.png)

1. <span data-ttu-id="26c78-171">Nyomja le az `ESC` és típus `:wq` toosave hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="26c78-171">Press `ESC` and type `:wq` toosave hello file.</span></span>

### <a name="run-hello-sample-application"></a><span data-ttu-id="26c78-172">Hello mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="26c78-172">Run hello sample application</span></span>

1. <span data-ttu-id="26c78-173">Győződjön meg arról, hogy hello SensorTag be van kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="26c78-173">Make sure hello SensorTag is powered on.</span></span>
1. <span data-ttu-id="26c78-174">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="26c78-174">Run hello following command:</span></span>

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a><span data-ttu-id="26c78-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="26c78-175">Next steps</span></span>

[<span data-ttu-id="26c78-176">Az IoT-átjáró használata az érzékelő adatokat átalakításhoz Azure IoT oldala</span><span class="sxs-lookup"><span data-stu-id="26c78-176">Use IoT gateway for sensor data transformation with Azure IoT Edge</span></span>](iot-hub-gateway-kit-c-use-iot-gateway-for-data-conversion.md)
