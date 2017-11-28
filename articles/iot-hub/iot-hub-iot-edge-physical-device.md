---
title: "Fizikai eszköz használata az Azure IoT peremhálózati |} Microsoft Docs"
description: "Hogyan lehet egy Texas eszközök SensorTag eszköz segítségével adatokat küldeni az IoT-központ egy málna Pi 3 eszközön futó IoT peremhálózati átjárón keresztül. Az átjáró-Azure IoT peremhálózati t."
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 212dacbf-e5e9-48b2-9c8a-1c14d9e7b913
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/12/2017
ms.author: andbuc
ms.openlocfilehash: 02962a91c739a53dfcf947bcc736e5c293b9384f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-iot-edge-on-a-raspberry-pi-to-forward-device-to-cloud-messages-to-iot-hub"></a><span data-ttu-id="d64a9-104">Egy málna Pi Azure IoT peremhálózati segítségével eszközről a felhőbe üzenetek továbbítására az IoT-központ</span><span class="sxs-lookup"><span data-stu-id="d64a9-104">Use Azure IoT Edge on a Raspberry Pi to forward device-to-cloud messages to IoT Hub</span></span>

<span data-ttu-id="d64a9-105">Ez a forgatókönyv a [Bluetooth alacsony energia minta] [ lnk-ble-samplecode] bemutatja, hogyan használható [Azure IoT peremhálózati] [ lnk-sdk] való:</span><span class="sxs-lookup"><span data-stu-id="d64a9-105">This walkthrough of the [Bluetooth low energy sample][lnk-ble-samplecode] shows you how to use [Azure IoT Edge][lnk-sdk] to:</span></span>

* <span data-ttu-id="d64a9-106">Eszköz-felhő telemetriai továbbítja az IoT-központ fizikai eszközről.</span><span class="sxs-lookup"><span data-stu-id="d64a9-106">Forward device-to-cloud telemetry to IoT Hub from a physical device.</span></span>
* <span data-ttu-id="d64a9-107">Útvonal parancsokat az IoT-központ fizikai eszköz.</span><span class="sxs-lookup"><span data-stu-id="d64a9-107">Route commands from IoT Hub to a physical device.</span></span>

<span data-ttu-id="d64a9-108">A bemutató tartalma:</span><span class="sxs-lookup"><span data-stu-id="d64a9-108">This walkthrough covers:</span></span>

* <span data-ttu-id="d64a9-109">**Architektúra**: a Bluetooth alacsony energia minta fontos architekturális információkat.</span><span class="sxs-lookup"><span data-stu-id="d64a9-109">**Architecture**: important architectural information about the Bluetooth low energy sample.</span></span>
* <span data-ttu-id="d64a9-110">**Létrehozás és futtatás**: A minta elkészítéséhez és futtatásához szükséges lépések.</span><span class="sxs-lookup"><span data-stu-id="d64a9-110">**Build and run**: the steps required to build and run the sample.</span></span>

## <a name="architecture"></a><span data-ttu-id="d64a9-111">Architektúra</span><span class="sxs-lookup"><span data-stu-id="d64a9-111">Architecture</span></span>

<span data-ttu-id="d64a9-112">A bemutató ismerteti, hogyan lehet létrehozni, és futtassa az IoT-átjárónak egy málna Pi 3 Raspbian Linux futtató.</span><span class="sxs-lookup"><span data-stu-id="d64a9-112">The walkthrough shows you how to build and run an IoT Edge gateway on a Raspberry Pi 3 that runs Raspbian Linux.</span></span> <span data-ttu-id="d64a9-113">Az átjáró-IoT peremhálózati t.</span><span class="sxs-lookup"><span data-stu-id="d64a9-113">The gateway is built using IoT Edge.</span></span> <span data-ttu-id="d64a9-114">A minta egy Texas eszközök SensorTag Bluetooth alacsony energia (int) eszközt használ hőmérséklet-adatok gyűjtéséért felelős ügyfélfeladatot.</span><span class="sxs-lookup"><span data-stu-id="d64a9-114">The sample uses a Texas Instruments SensorTag Bluetooth Low Energy (BLE) device to collect temperature data.</span></span>

<span data-ttu-id="d64a9-115">Az IoT-átjárónak futtatásakor azt:</span><span class="sxs-lookup"><span data-stu-id="d64a9-115">When you run the IoT Edge gateway it:</span></span>

* <span data-ttu-id="d64a9-116">A Bluetooth alacsony energia (int) protokollt használó SensorTag eszköz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="d64a9-116">Connects to a SensorTag device using the Bluetooth Low Energy (BLE) protocol.</span></span>
* <span data-ttu-id="d64a9-117">Az IoT-központ a HTTP protokollal csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="d64a9-117">Connects to IoT Hub using the HTTP protocol.</span></span>
* <span data-ttu-id="d64a9-118">Telemetria továbbítja az SensorTag eszközről IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="d64a9-118">Forwards telemetry from the SensorTag device to IoT Hub.</span></span>
* <span data-ttu-id="d64a9-119">Útvonalak parancsok az IoT-központ az SensorTag eszközre.</span><span class="sxs-lookup"><span data-stu-id="d64a9-119">Routes commands from IoT Hub to the SensorTag device.</span></span>

<span data-ttu-id="d64a9-120">Az átjáró a következő IoT peremhálózati modulokat tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="d64a9-120">The gateway contains the following IoT Edge modules:</span></span>

* <span data-ttu-id="d64a9-121">A *BLA modul* , amely kapcsolódási pontok Generálja az eszköz hőmérséklet adatfogadás az eszközről, és az eszköz parancsainak elküldését.</span><span class="sxs-lookup"><span data-stu-id="d64a9-121">A *BLE module* that interfaces with a BLE device to receive temperature data from the device and send commands to the device.</span></span>
* <span data-ttu-id="d64a9-122">A *BLA felhő eszköz modulra* JSON üzenetek küldi az IoT-központ BLA utasításokat a következőkből fordítja le a *BLA modul*.</span><span class="sxs-lookup"><span data-stu-id="d64a9-122">A *BLE cloud to device module* that translates JSON messages sent from IoT Hub into BLE instructions for the *BLE module*.</span></span>
* <span data-ttu-id="d64a9-123">A *naplózó modul* , amely az összes átjáró üzenet helyi fájlban naplózza.</span><span class="sxs-lookup"><span data-stu-id="d64a9-123">A *logger module* that logs all gateway messages to a local file.</span></span>
* <span data-ttu-id="d64a9-124">Egy *identitás hozzárendelési modul* , amely átalakítja az int eszköz MAC-címek és Azure IoT Hub eszköz identitásokat.</span><span class="sxs-lookup"><span data-stu-id="d64a9-124">An *identity mapping module* that translates between BLE device MAC addresses and Azure IoT Hub device identities.</span></span>
* <span data-ttu-id="d64a9-125">Egy *IoT-központ modul* , telemetriai adatokat feltölt egy IoT-központot, és eszközparancsok kapott az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="d64a9-125">An *IoT Hub module* that uploads telemetry data to an IoT hub and receives device commands from an IoT hub.</span></span>
* <span data-ttu-id="d64a9-126">A *BLA nyomtató modul* , amely az int eszközről telemetriai értelmezi, és megrendelése formázott adatok hibaelhárítás és hibakeresés engedélyezése a konzolhoz.</span><span class="sxs-lookup"><span data-stu-id="d64a9-126">A *BLE printer module* that interprets telemetry from the BLE device and prints formatted data to the console to enable troubleshooting and debugging.</span></span>

### <a name="how-data-flows-through-the-gateway"></a><span data-ttu-id="d64a9-127">Hogyan adatáramlás az átjárón keresztül</span><span class="sxs-lookup"><span data-stu-id="d64a9-127">How data flows through the gateway</span></span>

<span data-ttu-id="d64a9-128">A következő blokk ábra szemlélteti a telemetriai adatok feltöltési adatok folyamata folyamat:</span><span class="sxs-lookup"><span data-stu-id="d64a9-128">The following block diagram illustrates the telemetry upload data flow pipeline:</span></span>

![Telemetriai adatainak feltöltése átjáró folyamat](media/iot-hub-iot-edge-physical-device/gateway_ble_upload_data_flow.png)

<span data-ttu-id="d64a9-130">A lépéseket, amelyek telemetriai elem időt vesz igénybe, IoT-központot egy BLA eszközről utazik a következők:</span><span class="sxs-lookup"><span data-stu-id="d64a9-130">The steps that an item of telemetry takes traveling from a BLE device to IoT Hub are:</span></span>

1. <span data-ttu-id="d64a9-131">Az int eszköz hőmérséklet minta hoz létre, és elküldi Bluetooth-on keresztül az átjáró BLA moduljának.</span><span class="sxs-lookup"><span data-stu-id="d64a9-131">The BLE device generates a temperature sample and sends it over Bluetooth to the BLE module in the gateway.</span></span>
1. <span data-ttu-id="d64a9-132">A táblázat modul megkapja a minta, és közzéteszi azokat az átvitelszervező mellett az eszköz MAC-címét.</span><span class="sxs-lookup"><span data-stu-id="d64a9-132">The BLE module receives the sample and publishes it to the broker along with the MAC address of the device.</span></span>
1. <span data-ttu-id="d64a9-133">Az identitás hozzárendelési modul szerzi be ezt az üzenetet, és egy belső tábla segítségével az eszköz MAC-címet jelenti azt, hogy az IoT-központ eszközidentitás.</span><span class="sxs-lookup"><span data-stu-id="d64a9-133">The identity mapping module picks up this message and uses an internal table to translate the MAC address of the device into an IoT Hub device identity.</span></span> <span data-ttu-id="d64a9-134">Az IoT-központ eszközidentitás Eszközazonosító és eszközkulcs áll.</span><span class="sxs-lookup"><span data-stu-id="d64a9-134">An IoT Hub device identity consists of a device ID and device key.</span></span>
1. <span data-ttu-id="d64a9-135">Az identitás hozzárendelési modul tesz közzé egy új üzenet, amely tartalmazza a hőmérséklet mintaadatok, az eszközt, az eszköz azonosítója és a eszközkulcs MAC-címét.</span><span class="sxs-lookup"><span data-stu-id="d64a9-135">The identity mapping module publishes a new message that contains the temperature sample data, the MAC address of the device, the device ID, and the device key.</span></span>
1. <span data-ttu-id="d64a9-136">Az IoT-központ modulja (identity hozzárendelési modul által előállított) az új üzenetet kap, és közzéteszi azokat az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="d64a9-136">The IoT Hub module receives this new message (generated by the identity mapping module) and publishes it to IoT Hub.</span></span>
1. <span data-ttu-id="d64a9-137">A naplózási modul összes üzenetet a broker egy helyi fájlba naplózza.</span><span class="sxs-lookup"><span data-stu-id="d64a9-137">The logger module logs all messages from the broker to a local file.</span></span>

<span data-ttu-id="d64a9-138">A következő blokk ábra szemlélteti a eszköz parancs adatok áramlási folyamat:</span><span class="sxs-lookup"><span data-stu-id="d64a9-138">The following block diagram illustrates the device command data flow pipeline:</span></span>

![Eszköz parancs átjáró folyamat](media/iot-hub-iot-edge-physical-device/gateway_ble_command_data_flow.png)

1. <span data-ttu-id="d64a9-140">Az IoT-központ modul rendszeres időközönként lekérdezi az IoT hub új parancs üzenetek.</span><span class="sxs-lookup"><span data-stu-id="d64a9-140">The IoT Hub module periodically polls the IoT hub for new command messages.</span></span>
1. <span data-ttu-id="d64a9-141">Az IoT-központ modul új parancs üzenetet kap, ha azt közzé teszi azt az átvitelszervező.</span><span class="sxs-lookup"><span data-stu-id="d64a9-141">When the IoT Hub module receives a new command message, it publishes it to the broker.</span></span>
1. <span data-ttu-id="d64a9-142">Az identitás hozzárendelési modul szerzi be a parancs üzenetet, és egy belső tábla lefordítani az IoT-központ Eszközazonosítót az eszköz MAC-címet használ.</span><span class="sxs-lookup"><span data-stu-id="d64a9-142">The identity mapping module picks up the command message and uses an internal table to translate the IoT Hub device ID to a device MAC address.</span></span> <span data-ttu-id="d64a9-143">Majd egy új üzenet, amely tartalmazza a célként megadott eszköz MAC-cím az üzenet tulajdonságai térképen tesz közzé.</span><span class="sxs-lookup"><span data-stu-id="d64a9-143">It then publishes a new message that includes the MAC address of the target device in the properties map of the message.</span></span>
1. <span data-ttu-id="d64a9-144">A lehetséges felhő eszköz modul szerzi be ezt az üzenetet, és fordítja le azt a megfelelő BLA utasítás BLA modul.</span><span class="sxs-lookup"><span data-stu-id="d64a9-144">The BLE Cloud-to-Device module picks up this message and translates it into the proper BLE instruction for the BLE module.</span></span> <span data-ttu-id="d64a9-145">Majd egy új üzenet tesz közzé.</span><span class="sxs-lookup"><span data-stu-id="d64a9-145">It then publishes a new message.</span></span>
1. <span data-ttu-id="d64a9-146">A táblázat modul szerzi be ezt az üzenetet, és végrehajtja az i/o-utasítás által a BLA eszköz kommunikál.</span><span class="sxs-lookup"><span data-stu-id="d64a9-146">The BLE module picks up this message and executes the I/O instruction by communicating with the BLE device.</span></span>
1. <span data-ttu-id="d64a9-147">A naplózási modul naplózza az összes üzenet az átvitelszervező egy lemezen levő fájlra.</span><span class="sxs-lookup"><span data-stu-id="d64a9-147">The logger module logs all messages from the broker to a disk file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d64a9-148">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d64a9-148">Prerequisites</span></span>

<span data-ttu-id="d64a9-149">Az oktatóanyag elvégzéséhez aktív Azure-előfizetésre lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="d64a9-149">To complete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="d64a9-150">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="d64a9-150">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="d64a9-151">További információ: [Ingyenes Azure-fiók létrehozása][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="d64a9-151">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

<span data-ttu-id="d64a9-152">SSH-ügyfél van szüksége az asztali gépen ahhoz, hogy a parancssor a málna Pi a érheti el távolról.</span><span class="sxs-lookup"><span data-stu-id="d64a9-152">You need SSH client on your desktop machine to enable you to remotely access the command line on the Raspberry Pi.</span></span>

- <span data-ttu-id="d64a9-153">A Windows tartalmaz egy SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="d64a9-153">Windows does not include an SSH client.</span></span> <span data-ttu-id="d64a9-154">Azt javasoljuk, [PuTTY](http://www.putty.org/).</span><span class="sxs-lookup"><span data-stu-id="d64a9-154">We recommend using [PuTTY](http://www.putty.org/).</span></span>
- <span data-ttu-id="d64a9-155">A legtöbb Linux terjesztéseket, a Mac OS közé tartoznak az SSH parancssori segédprogramot.</span><span class="sxs-lookup"><span data-stu-id="d64a9-155">Most Linux distributions and Mac OS include the command-line SSH utility.</span></span> <span data-ttu-id="d64a9-156">További információkért lásd: [SSH használatával Linux és Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span><span class="sxs-lookup"><span data-stu-id="d64a9-156">For more information, see [SSH Using Linux or Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span></span>

## <a name="prepare-your-hardware"></a><span data-ttu-id="d64a9-157">Készítse elő a hardvert</span><span class="sxs-lookup"><span data-stu-id="d64a9-157">Prepare your hardware</span></span>

<span data-ttu-id="d64a9-158">Ez az oktatóanyag feltételezi, hogy a egy [Texas eszközök SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) egy málna Pi 3 Raspbian futtató eszköz csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="d64a9-158">This tutorial assumes you are using a [Texas Instruments SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) device connected to a Raspberry Pi 3 running Raspbian.</span></span>

### <a name="install-raspbian"></a><span data-ttu-id="d64a9-159">Raspbian telepítése</span><span class="sxs-lookup"><span data-stu-id="d64a9-159">Install Raspbian</span></span>

<span data-ttu-id="d64a9-160">Segítségével az alábbi lehetőségek valamelyikét Raspbian az málna Pi 3 eszközökre telepíthető.</span><span class="sxs-lookup"><span data-stu-id="d64a9-160">You can use either of the following options to install Raspbian on your Raspberry Pi 3 device.</span></span>

* <span data-ttu-id="d64a9-161">Raspbian legújabb verziójának telepítéséhez használja a [NOOBS] [ lnk-noobs] grafikus felhasználói felületen.</span><span class="sxs-lookup"><span data-stu-id="d64a9-161">To install the latest version of Raspbian, use the [NOOBS][lnk-noobs] graphical user interface.</span></span>
* <span data-ttu-id="d64a9-162">Manuálisan [letöltése] [ lnk-raspbian] és a legújabb a Raspbian operációs rendszer lemezképének írni SD-kártya.</span><span class="sxs-lookup"><span data-stu-id="d64a9-162">Manually [download][lnk-raspbian] and write the latest image of the Raspbian operating system to an SD card.</span></span>

### <a name="sign-in-and-access-the-terminal"></a><span data-ttu-id="d64a9-163">Bejelentkezhet és elérheti a Terminálszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="d64a9-163">Sign in and access the terminal</span></span>

<span data-ttu-id="d64a9-164">A Terminálszolgáltatások tesztkörnyezetben, a málna Pi eléréséhez két lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="d64a9-164">You have two options to access a terminal environment on your Raspberry Pi:</span></span>

* <span data-ttu-id="d64a9-165">Ha a figyelő a málna Pi csatlakoztatott és a billentyűzeten, használhatja a Raspbian grafikus felhasználói felület egy terminálablakot eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="d64a9-165">If you have a keyboard and monitor connected to your Raspberry Pi, you can use the Raspbian GUI to access a terminal window.</span></span>

* <span data-ttu-id="d64a9-166">A parancssorban meg az SSH használata a asztali gépen málna Pi eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="d64a9-166">Access the command line on your Raspberry Pi using SSH from your desktop machine.</span></span>

#### <a name="use-a-terminal-window-in-the-gui"></a><span data-ttu-id="d64a9-167">A grafikus felhasználói felületen terminálablakot használni</span><span class="sxs-lookup"><span data-stu-id="d64a9-167">Use a terminal Window in the GUI</span></span>

<span data-ttu-id="d64a9-168">Az alapértelmezett Raspbian a hitelesítő adatok felhasználónév **pi** és a jelszó **málna**.</span><span class="sxs-lookup"><span data-stu-id="d64a9-168">The default credentials for Raspbian are username **pi** and password **raspberry**.</span></span> <span data-ttu-id="d64a9-169">A tálcán a grafikus felhasználói felületén, elindíthatja a **Terminálszolgáltatások** segédprogram használatával egy figyelőt a ikon.</span><span class="sxs-lookup"><span data-stu-id="d64a9-169">In the task bar in the GUI, you can launch the **Terminal** utility using the icon that looks like a monitor.</span></span>

#### <a name="sign-in-with-ssh"></a><span data-ttu-id="d64a9-170">SSH bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d64a9-170">Sign in with SSH</span></span>

<span data-ttu-id="d64a9-171">SSH használható parancssori hozzáférést a málna Pi.</span><span class="sxs-lookup"><span data-stu-id="d64a9-171">You can use SSH for command-line access to your Raspberry Pi.</span></span> <span data-ttu-id="d64a9-172">A cikk [SSH (Secure Shell)] [ lnk-pi-ssh] útmutatás a málna Pi SSH konfigurálása, valamint csatlakozhat a [Windows] [ lnk-ssh-windows] vagy [ Linux és Mac OS][lnk-ssh-linux].</span><span class="sxs-lookup"><span data-stu-id="d64a9-172">The article [SSH (Secure Shell)][lnk-pi-ssh] describes how to configure SSH on your Raspberry Pi, and how to connect from [Windows][lnk-ssh-windows] or [Linux & Mac OS][lnk-ssh-linux].</span></span>

<span data-ttu-id="d64a9-173">Jelentkezzen be a felhasználónevet **pi** és a jelszó **málna**.</span><span class="sxs-lookup"><span data-stu-id="d64a9-173">Sign in with username **pi** and password **raspberry**.</span></span>

### <a name="install-bluez-537"></a><span data-ttu-id="d64a9-174">BlueZ 5.37 telepítése</span><span class="sxs-lookup"><span data-stu-id="d64a9-174">Install BlueZ 5.37</span></span>

<span data-ttu-id="d64a9-175">A táblázat modulok konzultáljon a Bluetooth hardver a BlueZ vermen keresztül.</span><span class="sxs-lookup"><span data-stu-id="d64a9-175">The BLE modules talk to the Bluetooth hardware via the BlueZ stack.</span></span> <span data-ttu-id="d64a9-176">A megfelelő működéséhez a modulok BlueZ 5.37 verziójára van szükség.</span><span class="sxs-lookup"><span data-stu-id="d64a9-176">You need version 5.37 of BlueZ for the modules to work correctly.</span></span> <span data-ttu-id="d64a9-177">Ezek az utasítások ellenőrizze, hogy BlueZ megfelelő verziója telepítve van.</span><span class="sxs-lookup"><span data-stu-id="d64a9-177">These instructions make sure the correct version of BlueZ is installed.</span></span>

1. <span data-ttu-id="d64a9-178">Állítsa le a jelenlegi bluetooth démon:</span><span class="sxs-lookup"><span data-stu-id="d64a9-178">Stop the current bluetooth daemon:</span></span>

    ```sh
    sudo systemctl stop bluetooth
    ```

1. <span data-ttu-id="d64a9-179">Telepítse a BlueZ függőségek:</span><span class="sxs-lookup"><span data-stu-id="d64a9-179">Install the BlueZ dependencies:</span></span>

    ```sh
    sudo apt-get update
    sudo apt-get install bluetooth bluez-tools build-essential autoconf glib2.0 libglib2.0-dev libdbus-1-dev libudev-dev libical-dev libreadline-dev
    ```

1. <span data-ttu-id="d64a9-180">Töltse le a BlueZ forráskód bluez.org:</span><span class="sxs-lookup"><span data-stu-id="d64a9-180">Download the BlueZ source code from bluez.org:</span></span>

    ```sh
    wget http://www.kernel.org/pub/linux/bluetooth/bluez-5.37.tar.xz
    ```

1. <span data-ttu-id="d64a9-181">Bontsa ki a forráskód:</span><span class="sxs-lookup"><span data-stu-id="d64a9-181">Unzip the source code:</span></span>

    ```sh
    tar -xvf bluez-5.37.tar.xz
    ```

1. <span data-ttu-id="d64a9-182">Módosítsa a könyvtárat az újonnan létrehozott mappa:</span><span class="sxs-lookup"><span data-stu-id="d64a9-182">Change directories to the newly created folder:</span></span>

    ```sh
    cd bluez-5.37
    ```

1. <span data-ttu-id="d64a9-183">Adja meg a kialakítani BlueZ kódot:</span><span class="sxs-lookup"><span data-stu-id="d64a9-183">Configure the BlueZ code to be built:</span></span>

    ```sh
    ./configure --disable-udev --disable-systemd --enable-experimental
    ```

1. <span data-ttu-id="d64a9-184">Build BlueZ:</span><span class="sxs-lookup"><span data-stu-id="d64a9-184">Build BlueZ:</span></span>

    ```sh
    make
    ```

1. <span data-ttu-id="d64a9-185">Telepítse a BlueZ, ha ezzel végzett építési:</span><span class="sxs-lookup"><span data-stu-id="d64a9-185">Install BlueZ once it is done building:</span></span>

    ```sh
    sudo make install
    ```

1. <span data-ttu-id="d64a9-186">Systemd-szolgáltatás konfigurációjának módosítása a Bluetooth-on, így az új bluetooth démon a fájlban mutat `/lib/systemd/system/bluetooth.service`.</span><span class="sxs-lookup"><span data-stu-id="d64a9-186">Change systemd service configuration for bluetooth so it points to the new bluetooth daemon in the file `/lib/systemd/system/bluetooth.service`.</span></span> <span data-ttu-id="d64a9-187">A "ExecStart" sorban cserélje le a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="d64a9-187">Replace the 'ExecStart' line with the following text:</span></span>

    ```conf
    ExecStart=/usr/local/libexec/bluetooth/bluetoothd -E
    ```

### <a name="enable-connectivity-to-the-sensortag-device-from-your-raspberry-pi-3-device"></a><span data-ttu-id="d64a9-188">A SensorTag eszközre kapcsolódásának engedélyezése a málna Pi 3 eszközről</span><span class="sxs-lookup"><span data-stu-id="d64a9-188">Enable connectivity to the SensorTag device from your Raspberry Pi 3 device</span></span>

<span data-ttu-id="d64a9-189">A minta futtatásához vissza kell igazolnia, hogy a málna Pi 3 a SensorTag eszköz csatlakozhat.</span><span class="sxs-lookup"><span data-stu-id="d64a9-189">Before running the sample, you need to verify that your Raspberry Pi 3 can connect to the SensorTag device.</span></span>

1. <span data-ttu-id="d64a9-190">Győződjön meg arról a `rfkill` segédprogram:</span><span class="sxs-lookup"><span data-stu-id="d64a9-190">Ensure the `rfkill` utility is installed:</span></span>

    ```sh
    sudo apt-get install rfkill
    ```

1. <span data-ttu-id="d64a9-191">A málna Pi 3 bluetooth feloldása, és ellenőrizze, hogy a verziószáma **5.37**:</span><span class="sxs-lookup"><span data-stu-id="d64a9-191">Unblock bluetooth on the Raspberry Pi 3 and check that the version number is **5.37**:</span></span>

    ```sh
    sudo rfkill unblock bluetooth
    bluetoothctl --version
    ```

1. <span data-ttu-id="d64a9-192">Adja meg az interaktív bluetooth rendszerhéj, a bluetooth-szolgáltatás elindítása és hajtható végre a **bluetoothctl** parancs:</span><span class="sxs-lookup"><span data-stu-id="d64a9-192">To enter the interactive bluetooth shell, start the bluetooth service and execute the **bluetoothctl** command :</span></span>

    ```sh
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. <span data-ttu-id="d64a9-193">Adja meg a parancsot **bekapcsolási** energiagazdálkodással fel a Bluetooth-vezérlő.</span><span class="sxs-lookup"><span data-stu-id="d64a9-193">Enter the command **power on** to power up the bluetooth controller.</span></span> <span data-ttu-id="d64a9-194">A parancs visszaadja a kimenet az alábbihoz hasonló:</span><span class="sxs-lookup"><span data-stu-id="d64a9-194">The command returns output similar to the following:</span></span>

    ```sh
    [NEW] Controller 98:4F:EE:04:1F:DF C3 raspberrypi [default]
    ```

1. <span data-ttu-id="d64a9-195">Az interaktív bluetooth felületet, adja meg a parancs **vizsgálhat** szolgáltatást a Bluetooth-eszközök.</span><span class="sxs-lookup"><span data-stu-id="d64a9-195">In the interactive bluetooth shell, enter the command **scan on** to scan for bluetooth devices.</span></span> <span data-ttu-id="d64a9-196">A parancs visszaadja a kimenet az alábbihoz hasonló:</span><span class="sxs-lookup"><span data-stu-id="d64a9-196">The command returns output similar to the following:</span></span>

    ```sh
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```

1. <span data-ttu-id="d64a9-197">A SensorTag eszköz felderíthető ügyeljen a kis gombra kattintva (a zöld LED flash kell).</span><span class="sxs-lookup"><span data-stu-id="d64a9-197">Make the SensorTag device discoverable by pressing the small button (the green LED should flash).</span></span> <span data-ttu-id="d64a9-198">A málna Pi 3 kell felderíteni a SensorTag eszköz:</span><span class="sxs-lookup"><span data-stu-id="d64a9-198">The Raspberry Pi 3 should discover the SensorTag device:</span></span>

    ```sh
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```

    <span data-ttu-id="d64a9-199">Ebben a példában látható, hogy a MAC-cím a SensorTag eszköz **A0:E6:F8:B5:F6:00**.</span><span class="sxs-lookup"><span data-stu-id="d64a9-199">In this example, you can see that the MAC address of the SensorTag device is **A0:E6:F8:B5:F6:00**.</span></span>

1. <span data-ttu-id="d64a9-200">Kapcsolja ki a megadásával ellenőrzését a **ki vizsgálata** parancs:</span><span class="sxs-lookup"><span data-stu-id="d64a9-200">Turn off scanning by entering the **scan off** command:</span></span>

    ```sh
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```

1. <span data-ttu-id="d64a9-201">Csatlakozás egy SensorTag-eszközt a MAC-cím megadásával **csatlakozás \<MAC-cím\>**.</span><span class="sxs-lookup"><span data-stu-id="d64a9-201">Connect to your SensorTag device using its MAC address by entering **connect \<MAC address\>**.</span></span> <span data-ttu-id="d64a9-202">A következő minta kimenet az átláthatóság rövidítése:</span><span class="sxs-lookup"><span data-stu-id="d64a9-202">The following sample output is abbreviated for clarity:</span></span>

    ```sh
    Attempting to connect to A0:E6:F8:B5:F6:00
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: yes
    Connection successful
    [CHG] Device A0:E6:F8:B5:F6:00 UUIDs: 00001800-0000-1000-8000-00805f9b34fb
    ...
    [NEW] Primary Service
            /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
            Device Information
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 GattServices: /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 Name: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Alias: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Modalias: bluetooth:v000Dp0000d0110
    ```

    > <span data-ttu-id="d64a9-203">Az eszköz újra GATT jellemzői listázhatja a **lista-attribútumok** parancsot.</span><span class="sxs-lookup"><span data-stu-id="d64a9-203">You can list the GATT characteristics of the device again using the **list-attributes** command.</span></span>

1. <span data-ttu-id="d64a9-204">Most már leválaszthatja a eszköz a a **leválasztása** parancsot, és zárja be a rendszerhéj a bluetooth a **lépjen ki a** parancs:</span><span class="sxs-lookup"><span data-stu-id="d64a9-204">You can now disconnect from the device using the **disconnect** command and then exit from the bluetooth shell using the **quit** command:</span></span>

    ```sh
    Attempting to disconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

<span data-ttu-id="d64a9-205">Most már készen áll a lehetséges IoT peremhálózati minta futtatásához a málna Pi 3.</span><span class="sxs-lookup"><span data-stu-id="d64a9-205">You're now ready to run the BLE IoT Edge sample on your Raspberry Pi 3.</span></span>

## <a name="run-the-iot-edge-ble-sample"></a><span data-ttu-id="d64a9-206">Futtathatja a IoT peremhálózati BLA</span><span class="sxs-lookup"><span data-stu-id="d64a9-206">Run the IoT Edge BLE sample</span></span>

<span data-ttu-id="d64a9-207">Az IoT peremhálózati BLA minta futtatásához kell három feladatok elvégzéséhez:</span><span class="sxs-lookup"><span data-stu-id="d64a9-207">To run the IoT Edge BLE sample, you need to complete three tasks:</span></span>

* <span data-ttu-id="d64a9-208">Adja meg az IoT Hub két minta eszköz.</span><span class="sxs-lookup"><span data-stu-id="d64a9-208">Configure two sample devices in your IoT Hub.</span></span>
* <span data-ttu-id="d64a9-209">Build IoT peremhálózati málna Pi 3 eszközén.</span><span class="sxs-lookup"><span data-stu-id="d64a9-209">Build IoT Edge on your Raspberry Pi 3 device.</span></span>
* <span data-ttu-id="d64a9-210">Konfigurálja, és futtathatja a BLA málna Pi 3 eszközén.</span><span class="sxs-lookup"><span data-stu-id="d64a9-210">Configure and run the BLE sample on your Raspberry Pi 3 device.</span></span>

<span data-ttu-id="d64a9-211">Írásának időpontjában IoT peremhálózati csak támogatja BLA modulok Linux rendszert futtató átjárókat.</span><span class="sxs-lookup"><span data-stu-id="d64a9-211">At the time of writing, IoT Edge only supports BLE modules in gateways running on Linux.</span></span>

### <a name="configure-two-sample-devices-in-your-iot-hub"></a><span data-ttu-id="d64a9-212">Az IoT Hub két minta eszközök konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d64a9-212">Configure two sample devices in your IoT Hub</span></span>

* <span data-ttu-id="d64a9-213">[Létrehoz egy IoT-központot] [ lnk-create-hub] az Azure-előfizetéséhez, a nevét a hub forgatókönyv végrehajtásához szükség van.</span><span class="sxs-lookup"><span data-stu-id="d64a9-213">[Create an IoT hub][lnk-create-hub] in your Azure subscription, you need the name of your hub to complete this walkthrough.</span></span> <span data-ttu-id="d64a9-214">Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="d64a9-214">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="d64a9-215">Hozzáad egy eszközt nevű **SensorTag_01** a IoT-központot, és jegyezze fel az azonosítót és az eszköz kulcsának.</span><span class="sxs-lookup"><span data-stu-id="d64a9-215">Add one device called **SensorTag_01** to your IoT hub and make a note of its id and device key.</span></span> <span data-ttu-id="d64a9-216">Használhatja a [eszköz explorer vagy az IOT hubbal-explorer] [ lnk-explorer-tools] eszközöket, az eszköz hozzáadása az IoT-központ az előző lépésben létrehozott lekérni a kulcsot.</span><span class="sxs-lookup"><span data-stu-id="d64a9-216">You can use the [device explorer or iothub-explorer][lnk-explorer-tools] tools to add this device to the IoT hub you created in the previous step and to retrieve its key.</span></span> <span data-ttu-id="d64a9-217">Ez az eszköz hozzárendelését az SensorTag eszközre az átjáró konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="d64a9-217">You map this device to the SensorTag device when you configure the gateway.</span></span>

### <a name="build-azure-iot-edge-on-your-raspberry-pi-3"></a><span data-ttu-id="d64a9-218">Az Azure IoT peremhálózati létrehozása a Raspberry Pi 3-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="d64a9-218">Build Azure IoT Edge on your Raspberry Pi 3</span></span>

<span data-ttu-id="d64a9-219">Függőségek telepítése Azure IoT szegély:</span><span class="sxs-lookup"><span data-stu-id="d64a9-219">Install dependencies for Azure IoT Edge:</span></span>

```sh
sudo apt-get install cmake uuid-dev curl libcurl4-openssl-dev libssl-dev
```

<span data-ttu-id="d64a9-220">Az alábbi parancsokkal IoT széle és az kezdőkönyvtárához összes submodules klónozása:</span><span class="sxs-lookup"><span data-stu-id="d64a9-220">Use the following commands to clone IoT Edge and all its submodules to your home directory:</span></span>

```sh
cd ~
git clone https://github.com/Azure/iot-edge.git
```

<span data-ttu-id="d64a9-221">Ha rendelkezik a málna Pi 3 IoT peremhálózati összetevőtárházat teljes másolata, hozhat létre a következő parancs használatával, amely tartalmazza az SDK mappából:</span><span class="sxs-lookup"><span data-stu-id="d64a9-221">When you have a complete copy of the IoT Edge repository on your Raspberry Pi 3, you can build it using the following command from the folder that contains the SDK:</span></span>

```sh
cd ~/iot-edge
./tools/build.sh  --disable-native-remote-modules
```

### <a name="configure-and-run-the-ble-sample-on-your-raspberry-pi-3"></a><span data-ttu-id="d64a9-222">Konfigurálja és futtathatja a Generálja a málna Pi 3</span><span class="sxs-lookup"><span data-stu-id="d64a9-222">Configure and run the BLE sample on your Raspberry Pi 3</span></span>

<span data-ttu-id="d64a9-223">Bootstrap, és futtathatja, konfigurálnia kell az egyes IoT peremhálózati modul, amely részt vesz az átjáróban.</span><span class="sxs-lookup"><span data-stu-id="d64a9-223">To bootstrap and run the sample, you must configure each IoT Edge module that participates in the gateway.</span></span> <span data-ttu-id="d64a9-224">Ebben a konfigurációban megadott JSON-fájlt, és konfigurálnia kell az öt részt vevő IoT peremhálózati modulok.</span><span class="sxs-lookup"><span data-stu-id="d64a9-224">This configuration is provided in a JSON file and you must configure all five participating IoT Edge modules.</span></span> <span data-ttu-id="d64a9-225">A tárház nevű minta JSON-fájl van **átjáró\_sample.json** használható kiindulási pontként a saját konfigurációs fájl készítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d64a9-225">There is a sample JSON file in the repository called **gateway\_sample.json** that you can use as the starting point for building your own configuration file.</span></span> <span data-ttu-id="d64a9-226">A fájl a **minták/ble_gateway/src** helyi másolat készítése az IoT-Edge tárház mappájában.</span><span class="sxs-lookup"><span data-stu-id="d64a9-226">This file is in the **samples/ble_gateway/src** folder in local copy of the IoT Edge repository.</span></span>

<span data-ttu-id="d64a9-227">Az alábbi szakaszok ismertetik a lehetséges minta a konfigurációs fájl szerkesztése, és feltételezik, hogy az IoT-Edge tárház a a **/home/pi/iot-edge /** a málna Pi 3 mappájába.</span><span class="sxs-lookup"><span data-stu-id="d64a9-227">The following sections describe how to edit this configuration file for the BLE sample and assume that the IoT Edge repository is in the **/home/pi/iot-edge/** folder on your Raspberry Pi 3.</span></span> <span data-ttu-id="d64a9-228">Ha a tárház máshol, ennek megfelelően állítsa be az elérési utakat.</span><span class="sxs-lookup"><span data-stu-id="d64a9-228">If the repository is elsewhere, adjust the paths accordingly.</span></span>

#### <a name="logger-configuration"></a><span data-ttu-id="d64a9-229">Naplózási konfiguráció</span><span class="sxs-lookup"><span data-stu-id="d64a9-229">Logger configuration</span></span>

<span data-ttu-id="d64a9-230">Feltéve, hogy az átjáró tárházban található a **/home/pi/iot-edge /** mappa, a naplózási modul konfigurálása az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="d64a9-230">Assuming the gateway repository is located in the **/home/pi/iot-edge/** folder, configure the logger module as follows:</span></span>

```json
{
  "name": "Logger",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path" : "build/modules/logger/liblogger.so"
    }
  },
  "args":
  {
    "filename": "<</path/to/log-file.log>>"
  }
}
```

#### <a name="ble-module-configuration"></a><span data-ttu-id="d64a9-231">BLA modul konfigurációja</span><span class="sxs-lookup"><span data-stu-id="d64a9-231">BLE module configuration</span></span>

<span data-ttu-id="d64a9-232">A minta konfigurációs BLA eszköz azt feltételezi, hogy egy Texas eszközök SensorTag eszközt.</span><span class="sxs-lookup"><span data-stu-id="d64a9-232">The sample configuration for the BLE device assumes a Texas Instruments SensorTag device.</span></span> <span data-ttu-id="d64a9-233">Bármely szabványos BLA eszköz, amely működhet, a perifériák GATT kell működnie, de frissítésére lehet szükség a GATT jellemző azonosítók és adatokat.</span><span class="sxs-lookup"><span data-stu-id="d64a9-233">Any standard BLE device that can operate as a GATT peripheral should work but you may need to update the GATT characteristic IDs and data.</span></span> <span data-ttu-id="d64a9-234">Adja hozzá a SensorTag eszköz MAC-címe:</span><span class="sxs-lookup"><span data-stu-id="d64a9-234">Add the MAC address of your SensorTag device:</span></span>

```json
{
  "name": "SensorTag",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/ble/libble.so"
    }
  },
  "args": {
    "controller_index": 0,
    "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
    "instructions": [
      {
        "type": "read_once",
        "characteristic_uuid": "00002A24-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A25-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A26-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A27-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A28-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A29-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "write_at_init",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AQ=="
      },
      {
        "type": "read_periodic",
        "characteristic_uuid": "F000AA01-0451-4000-B000-000000000000",
        "interval_in_ms": 1000
      },
      {
        "type": "write_at_exit",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AA=="
      }
    ]
  }
}
```

<span data-ttu-id="d64a9-235">Ha nem használ egy SensorTag eszközt, tekintse át a meghatározásához, hogy szükséges-e frissíteni a GATT jellemző azonosítókat és az adatértékek lehetséges eszköz dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="d64a9-235">If you are not using a SensorTag device, review the documentation for your BLE device to determine whether you need to update the GATT characteristic IDs and data values.</span></span>

#### <a name="iot-hub-module"></a><span data-ttu-id="d64a9-236">Az IoT-központ modulja</span><span class="sxs-lookup"><span data-stu-id="d64a9-236">IoT Hub module</span></span>

<span data-ttu-id="d64a9-237">Adja hozzá az IoT Hub nevét.</span><span class="sxs-lookup"><span data-stu-id="d64a9-237">Add the name of your IoT Hub.</span></span> <span data-ttu-id="d64a9-238">A utótag érték általában **azure-devices.net**:</span><span class="sxs-lookup"><span data-stu-id="d64a9-238">The suffix value is typically **azure-devices.net**:</span></span>

```json
{
  "name": "IoTHub",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/iothub/libiothub.so"
    }
  },
  "args": {
    "IoTHubName": "<<Azure IoT Hub Name>>",
    "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
    "Transport" : "amqp"
  }
}
```

#### <a name="identity-mapping-module-configuration"></a><span data-ttu-id="d64a9-239">Identitás-hozzárendelési modul konfigurációja</span><span class="sxs-lookup"><span data-stu-id="d64a9-239">Identity mapping module configuration</span></span>

<span data-ttu-id="d64a9-240">Adja hozzá a MAC-címet az SensorTag eszköz eszköz azonosítója és kulcsa a **SensorTag_01** felvette az IoT Hub eszköz:</span><span class="sxs-lookup"><span data-stu-id="d64a9-240">Add the MAC address of your SensorTag device and the device ID and key of the **SensorTag_01** device you added to your IoT Hub:</span></span>

```json
{
  "name": "mapping",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/identitymap/libidentity_map.so"
    }
  },
  "args": [
    {
      "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
      "deviceId": "<<Azure IoT Hub Device ID>>",
      "deviceKey": "<<Azure IoT Hub Device Key>>"
    }
  ]
}
```

#### <a name="ble-printer-module-configuration"></a><span data-ttu-id="d64a9-241">BLA nyomtató modul konfigurációja</span><span class="sxs-lookup"><span data-stu-id="d64a9-241">BLE Printer module configuration</span></span>

```json
{
  "name": "BLE Printer",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/samples/ble_gateway/ble_printer/libble_printer.so"
    }
  },
  "args": null
}
```

#### <a name="blec2d-module-configuration"></a><span data-ttu-id="d64a9-242">BLEC2D modul konfigurációja</span><span class="sxs-lookup"><span data-stu-id="d64a9-242">BLEC2D Module Configuration</span></span>

```json
{
  "name": "BLEC2D",
  "loader": {
    "name" : "native",
    "entrypoint" : {
      "module.path": "build/modules/ble/libble_c2d.so"
    }
  },
  "args": null
}
```

#### <a name="routing-configuration"></a><span data-ttu-id="d64a9-243">Útválasztási konfigurációja</span><span class="sxs-lookup"><span data-stu-id="d64a9-243">Routing Configuration</span></span>

<span data-ttu-id="d64a9-244">A következő konfigurációs biztosítja a következő útválasztási IoT peremhálózati modulok között:</span><span class="sxs-lookup"><span data-stu-id="d64a9-244">The following configuration ensures the following routing between IoT Edge modules:</span></span>

* <span data-ttu-id="d64a9-245">A **naplózó** modul kap, és minden üzenetet naplózza.</span><span class="sxs-lookup"><span data-stu-id="d64a9-245">The **Logger** module receives and logs all messages.</span></span>
* <span data-ttu-id="d64a9-246">A **SensorTag** modul üzeneteket küld mind a **leképezési** és **BLA nyomtató** modulok.</span><span class="sxs-lookup"><span data-stu-id="d64a9-246">The **SensorTag** module sends messages to both the **mapping** and **BLE Printer** modules.</span></span>
* <span data-ttu-id="d64a9-247">A **leképezési** modul üzeneteket küld a **IOT hubbal** küldendő az IoT Hub modul.</span><span class="sxs-lookup"><span data-stu-id="d64a9-247">The **mapping** module sends messages to the **IoTHub** module to be sent up to your IoT Hub.</span></span>
* <span data-ttu-id="d64a9-248">A **IOT hubbal** modul küld vissza üzenetek a **leképezési** modul.</span><span class="sxs-lookup"><span data-stu-id="d64a9-248">The **IoTHub** module sends messages back to the **mapping** module.</span></span>
* <span data-ttu-id="d64a9-249">A **leképezési** modul üzeneteket küld a **BLEC2D** modul.</span><span class="sxs-lookup"><span data-stu-id="d64a9-249">The **mapping** module sends messages to the **BLEC2D** module.</span></span>
* <span data-ttu-id="d64a9-250">A **BLEC2D** modul küld vissza üzenetek a **Sensor Tag** modul.</span><span class="sxs-lookup"><span data-stu-id="d64a9-250">The **BLEC2D** module sends messages back to the **Sensor Tag** module.</span></span>

```json
"links" : [
    {"source" : "*", "sink" : "Logger" },
    {"source" : "SensorTag", "sink" : "mapping" },
    {"source" : "SensorTag", "sink" : "BLE Printer" },
    {"source" : "mapping", "sink" : "IoTHub" },
    {"source" : "IoTHub", "sink" : "mapping" },
    {"source" : "mapping", "sink" : "BLEC2D" },
    {"source" : "BLEC2D", "sink" : "SensorTag"}
 ]
```

<span data-ttu-id="d64a9-251">A minta futtatásához adja át az elérési út a JSON-konfigurációs fájl paramétereként a **gedélyezése\_átjáró** bináris.</span><span class="sxs-lookup"><span data-stu-id="d64a9-251">To run the sample, pass the path to the JSON configuration file as a parameter to the **ble\_gateway** binary.</span></span> <span data-ttu-id="d64a9-252">Az alábbi parancs feltételezi, hogy használja a **gateway_sample.json** konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="d64a9-252">The following command assumes you are using the **gateway_sample.json** configuration file.</span></span> <span data-ttu-id="d64a9-253">A parancs végrehajtásához a **iot-edge** a málna Pi mappájában:</span><span class="sxs-lookup"><span data-stu-id="d64a9-253">Execute this command from the **iot-edge** folder on the Raspberry Pi:</span></span>

```sh
./build/samples/ble_gateway/ble_gateway ./samples/ble_gateway/src/gateway_sample.json
```

<span data-ttu-id="d64a9-254">Szükség lehet a kis gombra a SensorTag eszköz könnyebben felderíthetők a minta futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="d64a9-254">You may need to press the small button on the SensorTag device to make it discoverable before you run the sample.</span></span>

<span data-ttu-id="d64a9-255">A minta futtatásához használhatja a [eszköz explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) vagy a [IOT hubbal-explorer](https://github.com/Azure/iothub-explorer) az IoT-átjárónak a SensorTag eszközről továbbítja az üzeneteket figyelésére.</span><span class="sxs-lookup"><span data-stu-id="d64a9-255">When you run the sample, you can use the [device explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) or the [iothub-explorer](https://github.com/Azure/iothub-explorer) tool to monitor the messages the IoT Edge gateway forwards from the SensorTag device.</span></span> <span data-ttu-id="d64a9-256">Például az IOT hubbal-Explorerben figyelheti eszköz-a-felhőbe küldött üzeneteket a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="d64a9-256">For example, using iothub-explorer you can monitor device-to-cloud messages using the following command:</span></span>

```sh
iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
```

## <a name="send-cloud-to-device-messages"></a><span data-ttu-id="d64a9-257">Üzenetküldés a felhőből az eszközökre</span><span class="sxs-lookup"><span data-stu-id="d64a9-257">Send cloud-to-device messages</span></span>

<span data-ttu-id="d64a9-258">A táblázat modul is támogatja az eszközt az IoT-központ küldő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="d64a9-258">The BLE module also supports sending commands from IoT Hub to the device.</span></span> <span data-ttu-id="d64a9-259">Használhatja a [eszköz explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) vagy a [IOT hubbal-explorer](https://github.com/Azure/iothub-explorer) eszköz, amely a BLA átjáró modul továbbítja az int eszköz be küldési JSON üzenetekre.</span><span class="sxs-lookup"><span data-stu-id="d64a9-259">You can use the [device explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) or the [iothub-explorer](https://github.com/Azure/iothub-explorer) tool to send JSON messages that the BLE gateway module forwards on to the BLE device.</span></span>

<span data-ttu-id="d64a9-260">Ha a Texas eszközök SensorTag eszközt használ, bekapcsolhatja a piros LED-jét, zöld LED-jét, vagy berregő parancsokat küld az IoT-központ.</span><span class="sxs-lookup"><span data-stu-id="d64a9-260">If you are using the Texas Instruments SensorTag device, you can turn on the red LED, green LED, or buzzer by sending commands from IoT Hub.</span></span> <span data-ttu-id="d64a9-261">Az IoT-központ küldeni a parancsok, először a következő két JSON-üzenetek küldése sorrendben.</span><span class="sxs-lookup"><span data-stu-id="d64a9-261">Before you send commands from IoT Hub, first send the following two JSON messages in order.</span></span> <span data-ttu-id="d64a9-262">Ezután a parancsok a fény vagy berregő bekapcsolása küldhet.</span><span class="sxs-lookup"><span data-stu-id="d64a9-262">Then you can send any of the commands to turn on the lights or buzzer.</span></span>

1. <span data-ttu-id="d64a9-263">Minden LED és a berregő alaphelyzetbe állítása (kikapcsolni őket):</span><span class="sxs-lookup"><span data-stu-id="d64a9-263">Reset all LEDs and the buzzer (turn them off):</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```

1. <span data-ttu-id="d64a9-264">"Távoli" i/o konfigurálni:</span><span class="sxs-lookup"><span data-stu-id="d64a9-264">Configure I/O as 'remote':</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

<span data-ttu-id="d64a9-265">Küldheti el a következő parancsok fény vagy berregő az SensorTag eszköz bekapcsolása:</span><span class="sxs-lookup"><span data-stu-id="d64a9-265">Now you can send any of the following commands to turn on the lights or buzzer on the SensorTag device:</span></span>

* <span data-ttu-id="d64a9-266">A piros LED bekapcsolása:</span><span class="sxs-lookup"><span data-stu-id="d64a9-266">Turn on the red LED:</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

* <span data-ttu-id="d64a9-267">A zöld LED bekapcsolása:</span><span class="sxs-lookup"><span data-stu-id="d64a9-267">Turn on the green LED:</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```

* <span data-ttu-id="d64a9-268">Kapcsolja be a berregő:</span><span class="sxs-lookup"><span data-stu-id="d64a9-268">Turn on the buzzer:</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

## <a name="next-steps"></a><span data-ttu-id="d64a9-269">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d64a9-269">Next Steps</span></span>

<span data-ttu-id="d64a9-270">Ha azt szeretné, IoT peremhálózati tájékozottabbak kapnak, és néhány kódpéldák kísérletezhet, látogasson el az alábbi fejlesztői oktatóanyagok és erőforrások:</span><span class="sxs-lookup"><span data-stu-id="d64a9-270">If you want to gain a more advanced understanding of IoT Edge and experiment with some code examples, visit the following developer tutorials and resources:</span></span>

* <span data-ttu-id="d64a9-271">[Az Azure IoT él][lnk-sdk]</span><span class="sxs-lookup"><span data-stu-id="d64a9-271">[Azure IoT Edge][lnk-sdk]</span></span>

<span data-ttu-id="d64a9-272">Az IoT-központ képességeit további megismeréséhez lásd:</span><span class="sxs-lookup"><span data-stu-id="d64a9-272">To further explore the capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="d64a9-273">[IoT Hub fejlesztői útmutató][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="d64a9-273">[IoT Hub developer guide][lnk-devguide]</span></span>

<!-- Links -->
[lnk-ble-samplecode]: https://github.com/Azure/iot-edge/tree/master/samples/ble_gateway
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-sdk]: https://github.com/Azure/iot-edge/
[lnk-noobs]: https://www.raspberrypi.org/documentation/installation/noobs.md
[lnk-raspbian]: https://www.raspberrypi.org/downloads/raspbian/
[lnk-devguide]: iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
