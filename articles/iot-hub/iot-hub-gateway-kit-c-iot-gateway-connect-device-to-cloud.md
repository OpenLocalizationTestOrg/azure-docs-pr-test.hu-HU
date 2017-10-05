---
title: "Az IoT-átjáró segítségével egy eszköz csatlakoztatása az Azure IoT Hub |} Microsoft Docs"
description: "Megtudhatja, hogyan IoT átjáróként Intel NUC segítségével csatlakozzon a TI SensorTag és érzékelő adatokat küldeni a felhőben Azure IoT-központot."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "az IOT-átjáró felhőbe eszköz csatlakoztatása"
ms.assetid: cb851648-018c-4a7e-860f-b62ed3b493a5
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/25/2017
ms.author: xshi
ms.openlocfilehash: 4fb77ed0241d15338c2574fd22828507c3e40cb3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="use-iot-gateway-to-connect-things-to-the-cloud---sensortag-to-azure-iot-hub"></a><span data-ttu-id="808f9-104">Az IoT-átjáró használata dolgot csatlakozni a felhő - SensorTag Azure IoT hubhoz</span><span class="sxs-lookup"><span data-stu-id="808f9-104">Use IoT gateway to connect things to the cloud - SensorTag to Azure IoT Hub</span></span>

> [!NOTE]
> <span data-ttu-id="808f9-105">Ez az oktatóanyag megkezdése előtt győződjön meg arról, hogy befejezte [Intel NUC IoT átjáróként beállítva](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md).</span><span class="sxs-lookup"><span data-stu-id="808f9-105">Before you start this tutorial, make sure you’ve completed [Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md).</span></span> <span data-ttu-id="808f9-106">A [Intel NUC IoT átjáróként beállítva](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md), úgy konfigurálja az Intel NUC eszközt az IoT átjáróként.</span><span class="sxs-lookup"><span data-stu-id="808f9-106">In [Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md), you set up the Intel NUC device as an IoT gateway.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="808f9-107">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="808f9-107">What you will learn</span></span>

<span data-ttu-id="808f9-108">Megismerheti, hogyan egy IoT-átjáró használatához Azure IoT-központot egy Texas eszközök SensorTag (CC2650STK) kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="808f9-108">You learn how to use an IoT gateway to connect a Texas Instruments SensorTag (CC2650STK) to Azure IoT Hub.</span></span> <span data-ttu-id="808f9-109">Az IoT-átjáró adatokat küld a hőmérséklet és a páratartalom összegyűjtött a SensorTag az Azure IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="808f9-109">The IoT gateway sends temperature and humidity data collected from the SensorTag to Azure IoT Hub.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="808f9-110">Mit fog</span><span class="sxs-lookup"><span data-stu-id="808f9-110">What you will do</span></span>

- <span data-ttu-id="808f9-111">Létrehoz egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="808f9-111">Create an IoT hub.</span></span>
- <span data-ttu-id="808f9-112">A SensorTag az IoT hub eszköz regisztrálása</span><span class="sxs-lookup"><span data-stu-id="808f9-112">Register a device in the IoT hub for the SensorTag.</span></span>
- <span data-ttu-id="808f9-113">Engedélyezze a kapcsolatot az IoT-átjáró és a SensorTag.</span><span class="sxs-lookup"><span data-stu-id="808f9-113">Enable the connection between the IoT gateway and the SensorTag.</span></span>
- <span data-ttu-id="808f9-114">SensorTag adatokat küldeni az IoT hub BLA mintaalkalmazás futtatása.</span><span class="sxs-lookup"><span data-stu-id="808f9-114">Run a BLE sample application to send SensorTag data to your IoT hub.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="808f9-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="808f9-115">What you need</span></span>

- <span data-ttu-id="808f9-116">Az oktatóanyag [Intel NUC IoT átjáróként beállítva](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md) befejeződött a amely Intel NUC IoT átjáróként beállítása.</span><span class="sxs-lookup"><span data-stu-id="808f9-116">Tutorial [Set up Intel NUC as an IoT gateway](iot-hub-gateway-kit-c-lesson1-set-up-nuc.md) completed in which you set up Intel NUC as an IoT gateway.</span></span>
- * <span data-ttu-id="808f9-117">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="808f9-117">An active Azure subscription.</span></span> <span data-ttu-id="808f9-118">Ha az Azure-fiók nem rendelkezik [hozzon létre egy Azure próbafiókot](https://azure.microsoft.com/free/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="808f9-118">If you don't have an Azure account, [create a free Azure trial account](https://azure.microsoft.com/free/) in just a few minutes.</span></span>
- <span data-ttu-id="808f9-119">Egy SSH-ügyfél rendszert futtató számítógépen.</span><span class="sxs-lookup"><span data-stu-id="808f9-119">An SSH client that runs on your host computer.</span></span> <span data-ttu-id="808f9-120">A puTTY Windows ajánlott.</span><span class="sxs-lookup"><span data-stu-id="808f9-120">PuTTY is recommended on Windows.</span></span> <span data-ttu-id="808f9-121">Linux- és macOS egy SSH-ügyfél már rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="808f9-121">Linux and macOS already come with an SSH client.</span></span>
- <span data-ttu-id="808f9-122">Az IP-cím és a felhasználónév és jelszó megadásával érheti el az átjárót az SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="808f9-122">The IP address and the username and password to access the gateway from the SSH client.</span></span>
- <span data-ttu-id="808f9-123">Az internethez.</span><span class="sxs-lookup"><span data-stu-id="808f9-123">An Internet connection.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

> [!NOTE]
> <span data-ttu-id="808f9-124">Itt az új eszköz regisztrálásához a SensorTag</span><span class="sxs-lookup"><span data-stu-id="808f9-124">Here you register this new device for your SensorTag</span></span>

## <a name="enable-the-connection-between-the-iot-gateway-and-the-sensortag"></a><span data-ttu-id="808f9-125">Az IoT-átjáró és a SensorTag közötti kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="808f9-125">Enable the connection between the IoT gateway and the SensorTag</span></span>

<span data-ttu-id="808f9-126">Ebben a szakaszban hajtsa végre a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="808f9-126">In this section, you perform the following tasks:</span></span>

- <span data-ttu-id="808f9-127">Bluetooth-kapcsolat a MAC-címét a SensorTag beolvasása.</span><span class="sxs-lookup"><span data-stu-id="808f9-127">Get the MAC address of the SensorTag for Bluetooth connection.</span></span>
- <span data-ttu-id="808f9-128">Az IoT-átjáró a SensorTag a Bluetooth-kapcsolat kezdeményezéséhez.</span><span class="sxs-lookup"><span data-stu-id="808f9-128">Initiate a Bluetooth connection from the IoT gateway to the SensorTag.</span></span>

### <a name="get-the-mac-address-of-the-sensortag-for-bluetooth-connection"></a><span data-ttu-id="808f9-129">A MAC-címét a SensorTag lekérése Bluetooth-kapcsolat</span><span class="sxs-lookup"><span data-stu-id="808f9-129">Get the MAC address of the SensorTag for Bluetooth connection</span></span>

1. <span data-ttu-id="808f9-130">Az állomáson az SSH-ügyfél, és az IoT-átjárón.</span><span class="sxs-lookup"><span data-stu-id="808f9-130">On the host computer, run the SSH client and connect to the IoT gateway.</span></span>
1. <span data-ttu-id="808f9-131">Bluetooth feloldása a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="808f9-131">Unblock Bluetooth by running the following command:</span></span>

   ```bash
   sudo rfkill unblock bluetooth
   ```

1. <span data-ttu-id="808f9-132">A Bluetooth-szolgáltatás elindítása az IoT-átjárón, és írja be a Bluetooth rendszerhéjat Bluetooth konfigurálása a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="808f9-132">Start the Bluetooth service on the IoT gateway and enter a Bluetooth shell to configure Bluetooth by running the following commands:</span></span>

   ```bash
   sudo systemctl start bluetooth
   bluetoothctl
   ```

1. <span data-ttu-id="808f9-133">A Bluetooth-vezérlő a következő parancs futtatásával a Bluetooth rendszerhéj bekapcsolása:</span><span class="sxs-lookup"><span data-stu-id="808f9-133">Power on the Bluetooth controller by running the following command at the Bluetooth shell:</span></span>

   ```bash
   power on
   ```

   ![a Bluetooth-vezérlő az IoT-átjárón a bluetoothctl bekapcsolása](./media/iot-hub-iot-gateway-connect-device-to-cloud/8_power-on-bluetooth-controller-at-bluetooth-shell-bluetoothctl.png)

1. <span data-ttu-id="808f9-135">Indítsa el a következő parancs futtatásával közeli Bluetooth-eszközök keresése:</span><span class="sxs-lookup"><span data-stu-id="808f9-135">Start scanning for nearby Bluetooth devices by running the following command:</span></span>

   ```bash
   scan on
   ```

   ![Bluetooth-eszközök a bluetoothctl közelben vizsgálata](./media/iot-hub-iot-gateway-connect-device-to-cloud/9_start-scan-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

1. <span data-ttu-id="808f9-137">Nyomja meg a SensorTag párosítási gombját.</span><span class="sxs-lookup"><span data-stu-id="808f9-137">Press the pairing button on the SensorTag.</span></span> <span data-ttu-id="808f9-138">A zöld a SensorTag villanás a vezetett.</span><span class="sxs-lookup"><span data-stu-id="808f9-138">The green LED on the SensorTag flashes.</span></span>
1. <span data-ttu-id="808f9-139">A Bluetooth rendszerhéj, meg kell jelennie a SensorTag megtalálható-e.</span><span class="sxs-lookup"><span data-stu-id="808f9-139">At the Bluetooth shell, you should see the SensorTag is found.</span></span> <span data-ttu-id="808f9-140">Jegyezze fel a SensorTag MAC-címét.</span><span class="sxs-lookup"><span data-stu-id="808f9-140">Make a note of the MAC address of the SensorTag.</span></span> <span data-ttu-id="808f9-141">Ebben a példában a SensorTag a MAC-címe: `24:71:89:C0:7F:82`.</span><span class="sxs-lookup"><span data-stu-id="808f9-141">In this example, the MAC address of the SensorTag is `24:71:89:C0:7F:82`.</span></span>
1. <span data-ttu-id="808f9-142">Kapcsolja ki a vizsgálat a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="808f9-142">Turn off the scan by running the following command:</span></span>

   ```bash
   scan off
   ```

   ![Állítsa le a közeli bluetoothctl a Bluetooth-eszközök keresése](./media/iot-hub-iot-gateway-connect-device-to-cloud/10_stop-scanning-nearby-bluetooth-devices-at-bluetooth-shell-bluetoothctl.png)

### <a name="initiate-a-bluetooth-connection-from-the-iot-gateway-to-the-sensortag"></a><span data-ttu-id="808f9-144">Az IoT-átjáró a SensorTag a Bluetooth-kapcsolat kezdeményezéséhez</span><span class="sxs-lookup"><span data-stu-id="808f9-144">Initiate a Bluetooth connection from the IoT gateway to the SensorTag</span></span>

1. <span data-ttu-id="808f9-145">A SensorTag csatlakoztatása a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="808f9-145">Connect to the SensorTag by running the following command:</span></span>

   ```bash
   connect <MAC address>
   ```

   ![Csatlakozás a bluetoothctl SensorTag](./media/iot-hub-iot-gateway-connect-device-to-cloud/11_connect-to-sensortag-at-bluetooth-shell-bluetoothctl.png)

1. <span data-ttu-id="808f9-147">Válassza le a SensorTag, és zárja be a Bluetooth rendszerhéj a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="808f9-147">Disconnect from the SensorTag and exit the Bluetooth shell by running the following commands:</span></span>

   ```bash
   disconnect
   exit
   ```

   ![Válassza le a bluetoothctl SensorTag](./media/iot-hub-iot-gateway-connect-device-to-cloud/12_disconnect-from-sensortag-at-bluetooth-shell-bluetoothctl.png)

<span data-ttu-id="808f9-149">Sikeresen engedélyezte a SensorTag és az IoT-átjáró közötti kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="808f9-149">You've successfully enabled the connection between the SensorTag and the IoT gateway.</span></span>

## <a name="run-a-ble-sample-application-to-send-sensortag-data-to-your-iot-hub"></a><span data-ttu-id="808f9-150">SensorTag adatokat küldeni az IoT hub BLA mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="808f9-150">Run a BLE sample application to send SensorTag data to your IoT hub</span></span>

<span data-ttu-id="808f9-151">A Bluetooth alacsony energia (int) mintaalkalmazás Azure IoT peremhálózati biztosítja.</span><span class="sxs-lookup"><span data-stu-id="808f9-151">The Bluetooth Low Energy (BLE) sample application is provided by Azure IoT Edge.</span></span> <span data-ttu-id="808f9-152">A mintaalkalmazás BLA kapcsolat gyűjti az adatokat, és elküldheti az adatokat az IoT hub.</span><span class="sxs-lookup"><span data-stu-id="808f9-152">The sample application collects data from BLE connection and send the data to you IoT hub.</span></span> <span data-ttu-id="808f9-153">Futtassa a mintaalkalmazást, kell:</span><span class="sxs-lookup"><span data-stu-id="808f9-153">To run the sample application, you need to:</span></span>

1. <span data-ttu-id="808f9-154">A mintaalkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="808f9-154">Configure the sample application.</span></span>
1. <span data-ttu-id="808f9-155">Futtassa a mintaalkalmazást az IoT-átjárón.</span><span class="sxs-lookup"><span data-stu-id="808f9-155">Run the sample application on the IoT gateway.</span></span>

### <a name="configure-the-sample-application"></a><span data-ttu-id="808f9-156">A mintaalkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="808f9-156">Configure the sample application</span></span>

1. <span data-ttu-id="808f9-157">Ugrás a mintaalkalmazásnak a mappa a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="808f9-157">Go to the folder of the sample application by running the following command:</span></span>

   ```bash
   cd /usr/share/azureiotgatewaysdk/samples/ble_gateway
   ```

1. <span data-ttu-id="808f9-158">Nyissa meg a konfigurációs fájlt a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="808f9-158">Open the configuration file by running the following command:</span></span>

   ```bash
   vi ble_gateway.json
   ```

1. <span data-ttu-id="808f9-159">A konfigurációs fájlban töltse ki a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="808f9-159">In the configuration file, fill in the following values:</span></span>

   <span data-ttu-id="808f9-160">**IoTHubName**: az IoT hub nevét.</span><span class="sxs-lookup"><span data-stu-id="808f9-160">**IoTHubName**: The name of your IoT hub.</span></span>

   <span data-ttu-id="808f9-161">**IoTHubSuffix**: IoTHubSuffix beolvasása az eszköz kapcsolati karakterlánc az elsődleges kulcs le feljegyzett.</span><span class="sxs-lookup"><span data-stu-id="808f9-161">**IoTHubSuffix**: Get IoTHubSuffix from the primary key of the device connection string that you noted down.</span></span> <span data-ttu-id="808f9-162">Biztosítják, hogy az eszköz kapcsolati karakterláncot, az IoT hub kapcsolati karakterlánc nem az elsődleges kulcs az elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="808f9-162">Ensure that you get the primary key of the device connection string, not the primary key of your IoT hub connection string.</span></span> <span data-ttu-id="808f9-163">Az eszköz kapcsolati karakterlánc az elsődleges kulcs van formátumban `HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY`.</span><span class="sxs-lookup"><span data-stu-id="808f9-163">The primary key of the device connection string is in the format of `HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY`.</span></span>

   <span data-ttu-id="808f9-164">**Átviteli**: az alapértelmezett érték `amqp`.</span><span class="sxs-lookup"><span data-stu-id="808f9-164">**Transport**: The default value is `amqp`.</span></span> <span data-ttu-id="808f9-165">Ez az érték a protokoll során transpotation tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="808f9-165">This value shows the protocol during transpotation.</span></span> <span data-ttu-id="808f9-166">Annak oka az lehet `http`, `amqp`, vagy `mqtt`.</span><span class="sxs-lookup"><span data-stu-id="808f9-166">It could be `http`, `amqp`, or `mqtt`.</span></span>

   <span data-ttu-id="808f9-167">**macAddress**: a SensorTag feljegyzett le a MAC-címét.</span><span class="sxs-lookup"><span data-stu-id="808f9-167">**macAddress**: The MAC address of the SensorTag that you noted down.</span></span>

   <span data-ttu-id="808f9-168">**deviceID**: az eszköz az IoT hub létrehozott azonosítója.</span><span class="sxs-lookup"><span data-stu-id="808f9-168">**deviceID**: ID of the device that you created in your IoT hub.</span></span>

   <span data-ttu-id="808f9-169">**deviceKey**: az eszköz kapcsolati karakterlánc az elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="808f9-169">**deviceKey**: The primary key of the device connection string.</span></span>

   ![Fejezze be a konfigurációs fájl a Gedélyezése mintaalkalmazásnak](./media/iot-hub-iot-gateway-connect-device-to-cloud/13_edit-config-file-of-ble-sample.png)

1. <span data-ttu-id="808f9-171">Nyomja le az `ESC` és típus `:wq` fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="808f9-171">Press `ESC` and type `:wq` to save the file.</span></span>

### <a name="run-the-sample-application"></a><span data-ttu-id="808f9-172">Futtassa a mintaalkalmazást</span><span class="sxs-lookup"><span data-stu-id="808f9-172">Run the sample application</span></span>

1. <span data-ttu-id="808f9-173">Győződjön meg arról, hogy a SensorTag be van kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="808f9-173">Make sure the SensorTag is powered on.</span></span>
1. <span data-ttu-id="808f9-174">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="808f9-174">Run the following command:</span></span>

   ```bash
   ./ble_gateway ble_gateway.json
   ```

## <a name="next-steps"></a><span data-ttu-id="808f9-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="808f9-175">Next steps</span></span>

[<span data-ttu-id="808f9-176">Az IoT-átjáró használata az érzékelő adatokat átalakításhoz Azure IoT oldala</span><span class="sxs-lookup"><span data-stu-id="808f9-176">Use IoT gateway for sensor data transformation with Azure IoT Edge</span></span>](iot-hub-gateway-kit-c-use-iot-gateway-for-data-conversion.md)
