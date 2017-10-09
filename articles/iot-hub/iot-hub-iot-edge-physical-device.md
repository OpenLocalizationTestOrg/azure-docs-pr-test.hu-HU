---
title: "Azure IoT peremhálózati rendelkező fizikai eszköz aaaUse |} Microsoft Docs"
description: "Hogyan toouse egy Texas eszközök SensorTag eszköz toosend adatok tooan IoT-központot egy málna Pi 3 eszközön futó IoT peremhálózati átjárón keresztül. hello átjáró Azure IoT peremhálózati épül."
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
ms.openlocfilehash: a2385accdbd99012ad094232653ee47d4e5c7839
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-iot-edge-on-a-raspberry-pi-tooforward-device-to-cloud-messages-tooiot-hub"></a><span data-ttu-id="2cbeb-104">Azure IoT peremhálózati használatát egy málna Pi tooforward eszköz a felhőbe küldött üzeneteket tooIoT Hub</span><span class="sxs-lookup"><span data-stu-id="2cbeb-104">Use Azure IoT Edge on a Raspberry Pi tooforward device-to-cloud messages tooIoT Hub</span></span>

<span data-ttu-id="2cbeb-105">Ez a forgatókönyv a hello [Bluetooth alacsony energia minta] [ lnk-ble-samplecode] bemutatja, hogyan toouse [Azure IoT peremhálózati] [ lnk-sdk] számára:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-105">This walkthrough of hello [Bluetooth low energy sample][lnk-ble-samplecode] shows you how toouse [Azure IoT Edge][lnk-sdk] to:</span></span>

* <span data-ttu-id="2cbeb-106">Továbbítsa az eszközről a felhőbe telemetriai tooIoT Hub fizikai eszközről.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-106">Forward device-to-cloud telemetry tooIoT Hub from a physical device.</span></span>
* <span data-ttu-id="2cbeb-107">Útvonal-parancsok az IoT-központ tooa fizikai eszközről.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-107">Route commands from IoT Hub tooa physical device.</span></span>

<span data-ttu-id="2cbeb-108">A bemutató tartalma:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-108">This walkthrough covers:</span></span>

* <span data-ttu-id="2cbeb-109">**Architektúra**: hello Bluetooth alacsony energia minta fontos architekturális információkat.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-109">**Architecture**: important architectural information about hello Bluetooth low energy sample.</span></span>
* <span data-ttu-id="2cbeb-110">**Létrehozása és futtatása**: hello lépéseket szükséges toobuild és futtatási hello minta.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-110">**Build and run**: hello steps required toobuild and run hello sample.</span></span>

## <a name="architecture"></a><span data-ttu-id="2cbeb-111">Architektúra</span><span class="sxs-lookup"><span data-stu-id="2cbeb-111">Architecture</span></span>

<span data-ttu-id="2cbeb-112">hello a bemutató ismerteti, hogyan toobuild, és futtassa az IoT-átjárónak egy málna Pi 3 futtató Raspbian Linux.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-112">hello walkthrough shows you how toobuild and run an IoT Edge gateway on a Raspberry Pi 3 that runs Raspbian Linux.</span></span> <span data-ttu-id="2cbeb-113">hello átjáró IoT peremhálózati épül.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-113">hello gateway is built using IoT Edge.</span></span> <span data-ttu-id="2cbeb-114">hello minta egy Texas eszközök SensorTag Bluetooth alacsony energia (int) eszköz toocollect hőmérséklet-adatokat használja.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-114">hello sample uses a Texas Instruments SensorTag Bluetooth Low Energy (BLE) device toocollect temperature data.</span></span>

<span data-ttu-id="2cbeb-115">Az IoT-átjárónak hello futtatásakor azt:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-115">When you run hello IoT Edge gateway it:</span></span>

* <span data-ttu-id="2cbeb-116">Kapcsolatot hoz létre tooa SensorTag eszköz hello Bluetooth alacsony energia (int) protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-116">Connects tooa SensorTag device using hello Bluetooth Low Energy (BLE) protocol.</span></span>
* <span data-ttu-id="2cbeb-117">TooIoT Hub csatlakozik hello HTTP protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-117">Connects tooIoT Hub using hello HTTP protocol.</span></span>
* <span data-ttu-id="2cbeb-118">Hello SensorTag eszköz tooIoT Hub telemetriai továbbítja.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-118">Forwards telemetry from hello SensorTag device tooIoT Hub.</span></span>
* <span data-ttu-id="2cbeb-119">Az IoT-központ toohello SensorTag eszközről parancsok irányítja.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-119">Routes commands from IoT Hub toohello SensorTag device.</span></span>

<span data-ttu-id="2cbeb-120">hello IoT peremhálózati modulok a következő hello tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-120">hello gateway contains hello following IoT Edge modules:</span></span>

* <span data-ttu-id="2cbeb-121">A *BLA modul* , amely egy táblázat eszköz tooreceive hőmérséklet származó adatokkal hello eszköz és küldési parancsok toohello eszköz felületek.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-121">A *BLE module* that interfaces with a BLE device tooreceive temperature data from hello device and send commands toohello device.</span></span>
* <span data-ttu-id="2cbeb-122">A *BLA felhő toodevice modul* , amely az eszköz küldi hello BLA utasításokat az IoT-központ JSON-üzenetek *BLA modul*.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-122">A *BLE cloud toodevice module* that translates JSON messages sent from IoT Hub into BLE instructions for hello *BLE module*.</span></span>
* <span data-ttu-id="2cbeb-123">A *naplózó modul* , amely az összes átjáró üzenetek tooa helyi fájl naplózza.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-123">A *logger module* that logs all gateway messages tooa local file.</span></span>
* <span data-ttu-id="2cbeb-124">Egy *identitás hozzárendelési modul* , amely átalakítja az int eszköz MAC-címek és Azure IoT Hub eszköz identitásokat.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-124">An *identity mapping module* that translates between BLE device MAC addresses and Azure IoT Hub device identities.</span></span>
* <span data-ttu-id="2cbeb-125">Egy *IoT-központ modul* , amely feltölt telemetriai adatok tooan IoT-központot, és eszközparancsok fogad egy IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-125">An *IoT Hub module* that uploads telemetry data tooan IoT hub and receives device commands from an IoT hub.</span></span>
* <span data-ttu-id="2cbeb-126">A *BLA nyomtató modul* , telemetriai hello BLA eszközről értelmezi, és kiírja a formázott adatok toohello konzol tooenable hibaelhárítási és hibakeresési.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-126">A *BLE printer module* that interprets telemetry from hello BLE device and prints formatted data toohello console tooenable troubleshooting and debugging.</span></span>

### <a name="how-data-flows-through-hello-gateway"></a><span data-ttu-id="2cbeb-127">Hogyan adatáramlás hello átjárón keresztül</span><span class="sxs-lookup"><span data-stu-id="2cbeb-127">How data flows through hello gateway</span></span>

<span data-ttu-id="2cbeb-128">a következő blokk diagram hello hello telemetriai adatainak feltöltése adatok folyamata csővezeték mutatja be:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-128">hello following block diagram illustrates hello telemetry upload data flow pipeline:</span></span>

![Telemetriai adatainak feltöltése átjáró folyamat](media/iot-hub-iot-edge-physical-device/gateway_ble_upload_data_flow.png)

<span data-ttu-id="2cbeb-130">hello telemetriai elem történik, az a táblázat eszköz tooIoT utazás Hub lépésekre:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-130">hello steps that an item of telemetry takes traveling from a BLE device tooIoT Hub are:</span></span>

1. <span data-ttu-id="2cbeb-131">hello BLA eszköz hőmérséklet minta hoz létre, és elküldi azt Bluetooth toohello BLA modul az hello átjáró keresztül.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-131">hello BLE device generates a temperature sample and sends it over Bluetooth toohello BLE module in hello gateway.</span></span>
1. <span data-ttu-id="2cbeb-132">hello BLA modul hello minta kap, és közzéteszi azokat toohello broker együtt hello hello eszköz MAC-címét.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-132">hello BLE module receives hello sample and publishes it toohello broker along with hello MAC address of hello device.</span></span>
1. <span data-ttu-id="2cbeb-133">hello identitás hozzárendelési modul szerzi be ezt az üzenetet, és az IoT Hub eszköz identitásának egy belső tábla tootranslate hello hello eszköz MAC-címét használja.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-133">hello identity mapping module picks up this message and uses an internal table tootranslate hello MAC address of hello device into an IoT Hub device identity.</span></span> <span data-ttu-id="2cbeb-134">Az IoT-központ eszközidentitás Eszközazonosító és eszközkulcs áll.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-134">An IoT Hub device identity consists of a device ID and device key.</span></span>
1. <span data-ttu-id="2cbeb-135">hello identitás hozzárendelési modul tesz közzé egy új hello hőmérséklet mintaadatok, hello eszköz hello Eszközazonosító és hello eszközkulcs hello MAC-címét tartalmazó üzenetet.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-135">hello identity mapping module publishes a new message that contains hello temperature sample data, hello MAC address of hello device, hello device ID, and hello device key.</span></span>
1. <span data-ttu-id="2cbeb-136">az IoT-központ modul hello (hello identitás hozzárendelési modul által előállított) az új üzenetet kap, és közzéteszi azokat tooIoT Hub.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-136">hello IoT Hub module receives this new message (generated by hello identity mapping module) and publishes it tooIoT Hub.</span></span>
1. <span data-ttu-id="2cbeb-137">hello naplózó modul hello broker tooa helyi fájl összes üzenetet naplózza.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-137">hello logger module logs all messages from hello broker tooa local file.</span></span>

<span data-ttu-id="2cbeb-138">a következő blokk diagram hello hello eszköz parancs adatok áramlási folyamat mutatja be:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-138">hello following block diagram illustrates hello device command data flow pipeline:</span></span>

![Eszköz parancs átjáró folyamat](media/iot-hub-iot-edge-physical-device/gateway_ble_command_data_flow.png)

1. <span data-ttu-id="2cbeb-140">hello modul rendszeres időközönként lekérdezi az IoT-központ hello új parancs üzenetek IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-140">hello IoT Hub module periodically polls hello IoT hub for new command messages.</span></span>
1. <span data-ttu-id="2cbeb-141">Az IoT-központ modul hello új parancs üzenetet kap, ha azt közzéteszi azokat toohello broker.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-141">When hello IoT Hub module receives a new command message, it publishes it toohello broker.</span></span>
1. <span data-ttu-id="2cbeb-142">hello identitás hozzárendelési modul szerzi be üdvözlőüzenetére parancsot, és egy belső tábla tootranslate hello IoT Hub eszköz azonosítója tooa eszköz MAC-címet használ.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-142">hello identity mapping module picks up hello command message and uses an internal table tootranslate hello IoT Hub device ID tooa device MAC address.</span></span> <span data-ttu-id="2cbeb-143">Majd egy új üzenet hello tulajdonságok memóriatérkép üdvözlőüzenetére hello céleszköz hello MAC-címét tartalmazó tesz közzé.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-143">It then publishes a new message that includes hello MAC address of hello target device in hello properties map of hello message.</span></span>
1. <span data-ttu-id="2cbeb-144">hello BLA felhő eszköz modul szerzi be ezt az üzenetet, és fordítja le azt hello megfelelő BLA utasítás hello BLA modul.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-144">hello BLE Cloud-to-Device module picks up this message and translates it into hello proper BLE instruction for hello BLE module.</span></span> <span data-ttu-id="2cbeb-145">Majd egy új üzenet tesz közzé.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-145">It then publishes a new message.</span></span>
1. <span data-ttu-id="2cbeb-146">hello BLA modul szerzi be ezt az üzenetet, és végrehajtja a hello i/o-utasítás által hello BLA eszköz kommunikál.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-146">hello BLE module picks up this message and executes hello I/O instruction by communicating with hello BLE device.</span></span>
1. <span data-ttu-id="2cbeb-147">hello naplózási modul hello broker tooa fájlhoz üzenetekhez naplózza.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-147">hello logger module logs all messages from hello broker tooa disk file.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2cbeb-148">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2cbeb-148">Prerequisites</span></span>

<span data-ttu-id="2cbeb-149">toocomplete ebben az oktatóanyagban aktív Azure-előfizetés szükséges.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-149">toocomplete this tutorial, you need an active Azure subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="2cbeb-150">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-150">If you don’t have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="2cbeb-151">További információ: [Ingyenes Azure-fiók létrehozása][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="2cbeb-151">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

<span data-ttu-id="2cbeb-152">SSH-ügyfél az Ön tooremotely hozzáférés hello parancs sor a hello málna Pi asztali gépen tooenable kell.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-152">You need SSH client on your desktop machine tooenable you tooremotely access hello command line on hello Raspberry Pi.</span></span>

- <span data-ttu-id="2cbeb-153">A Windows tartalmaz egy SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-153">Windows does not include an SSH client.</span></span> <span data-ttu-id="2cbeb-154">Azt javasoljuk, [PuTTY](http://www.putty.org/).</span><span class="sxs-lookup"><span data-stu-id="2cbeb-154">We recommend using [PuTTY](http://www.putty.org/).</span></span>
- <span data-ttu-id="2cbeb-155">A legtöbb Linux terjesztéseket, a Mac OS parancssori segédprogram hello a SSH közé</span><span class="sxs-lookup"><span data-stu-id="2cbeb-155">Most Linux distributions and Mac OS include hello command-line SSH utility.</span></span> <span data-ttu-id="2cbeb-156">További információkért lásd: [SSH használatával Linux és Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span><span class="sxs-lookup"><span data-stu-id="2cbeb-156">For more information, see [SSH Using Linux or Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).</span></span>

## <a name="prepare-your-hardware"></a><span data-ttu-id="2cbeb-157">Készítse elő a hardvert</span><span class="sxs-lookup"><span data-stu-id="2cbeb-157">Prepare your hardware</span></span>

<span data-ttu-id="2cbeb-158">Ez az oktatóanyag feltételezi, hogy a egy [Texas eszközök SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) eszköz csatlakoztatva tooa málna Pi 3 Raspbian futtatása.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-158">This tutorial assumes you are using a [Texas Instruments SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) device connected tooa Raspberry Pi 3 running Raspbian.</span></span>

### <a name="install-raspbian"></a><span data-ttu-id="2cbeb-159">Raspbian telepítése</span><span class="sxs-lookup"><span data-stu-id="2cbeb-159">Install Raspbian</span></span>

<span data-ttu-id="2cbeb-160">Beállítások tooinstall Raspbian következő málna Pi 3 eszközén hello bármelyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-160">You can use either of hello following options tooinstall Raspbian on your Raspberry Pi 3 device.</span></span>

* <span data-ttu-id="2cbeb-161">tooinstall hello legújabb verziójára Raspbian, használjon hello [NOOBS] [ lnk-noobs] grafikus felhasználói felületen.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-161">tooinstall hello latest version of Raspbian, use hello [NOOBS][lnk-noobs] graphical user interface.</span></span>
* <span data-ttu-id="2cbeb-162">Manuálisan [letöltése] [ lnk-raspbian] írási és olvasási hello legújabb kép hello Raspbian operációs rendszer tooan SD-kártya.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-162">Manually [download][lnk-raspbian] and write hello latest image of hello Raspbian operating system tooan SD card.</span></span>

### <a name="sign-in-and-access-hello-terminal"></a><span data-ttu-id="2cbeb-163">Bejelentkezhetnek és elérhetik a hello Terminálszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="2cbeb-163">Sign in and access hello terminal</span></span>

<span data-ttu-id="2cbeb-164">Két beállítások tooaccess terminál környezettel rendelkezik a málna Pi meg:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-164">You have two options tooaccess a terminal environment on your Raspberry Pi:</span></span>

* <span data-ttu-id="2cbeb-165">Ha a billentyűzet és csatlakoztatott tooyour málna Pi figyelése, használhatja a hello Raspbian grafikus felhasználói Felülettel tooaccess egy terminálablakot.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-165">If you have a keyboard and monitor connected tooyour Raspberry Pi, you can use hello Raspbian GUI tooaccess a terminal window.</span></span>

* <span data-ttu-id="2cbeb-166">Hozzáférés hello parancs vonalát. az SSH használata a asztali gépen málna Pi.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-166">Access hello command line on your Raspberry Pi using SSH from your desktop machine.</span></span>

#### <a name="use-a-terminal-window-in-hello-gui"></a><span data-ttu-id="2cbeb-167">A grafikus felhasználói Felülettel hello terminálablakot használni</span><span class="sxs-lookup"><span data-stu-id="2cbeb-167">Use a terminal Window in hello GUI</span></span>

<span data-ttu-id="2cbeb-168">hello alapértelmezett Raspbian a hitelesítő adatok felhasználónév **pi** és a jelszó **málna**.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-168">hello default credentials for Raspbian are username **pi** and password **raspberry**.</span></span> <span data-ttu-id="2cbeb-169">Hello tálcán hello grafikus felhasználói Felülettel, elindíthatja a hello **Terminálszolgáltatások** segédprogram használatával egy figyelő hello ikon.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-169">In hello task bar in hello GUI, you can launch hello **Terminal** utility using hello icon that looks like a monitor.</span></span>

#### <a name="sign-in-with-ssh"></a><span data-ttu-id="2cbeb-170">SSH bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2cbeb-170">Sign in with SSH</span></span>

<span data-ttu-id="2cbeb-171">Parancssori hozzáférést tooyour málna Pi SSH használható.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-171">You can use SSH for command-line access tooyour Raspberry Pi.</span></span> <span data-ttu-id="2cbeb-172">hello cikk [SSH (Secure Shell)] [ lnk-pi-ssh] ismerteti, hogyan tooconfigure a málna Pi az SSH és hogyan tooconnect a [Windows] [ lnk-ssh-windows] vagy [Linux és Mac OS][lnk-ssh-linux].</span><span class="sxs-lookup"><span data-stu-id="2cbeb-172">hello article [SSH (Secure Shell)][lnk-pi-ssh] describes how tooconfigure SSH on your Raspberry Pi, and how tooconnect from [Windows][lnk-ssh-windows] or [Linux & Mac OS][lnk-ssh-linux].</span></span>

<span data-ttu-id="2cbeb-173">Jelentkezzen be a felhasználónevet **pi** és a jelszó **málna**.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-173">Sign in with username **pi** and password **raspberry**.</span></span>

### <a name="install-bluez-537"></a><span data-ttu-id="2cbeb-174">BlueZ 5.37 telepítése</span><span class="sxs-lookup"><span data-stu-id="2cbeb-174">Install BlueZ 5.37</span></span>

<span data-ttu-id="2cbeb-175">hello BLA modulok konzultáljon toohello Bluetooth hardver hello BlueZ verem keresztül.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-175">hello BLE modules talk toohello Bluetooth hardware via hello BlueZ stack.</span></span> <span data-ttu-id="2cbeb-176">Szükség van BlueZ 5.37 verziója hello modulok toowork megfelelően.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-176">You need version 5.37 of BlueZ for hello modules toowork correctly.</span></span> <span data-ttu-id="2cbeb-177">Ezek az utasítások ellenőrizze, hogy hello BlueZ megfelelő verziója telepítve van.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-177">These instructions make sure hello correct version of BlueZ is installed.</span></span>

1. <span data-ttu-id="2cbeb-178">Állítsa le a hello aktuális bluetooth démon:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-178">Stop hello current bluetooth daemon:</span></span>

    ```sh
    sudo systemctl stop bluetooth
    ```

1. <span data-ttu-id="2cbeb-179">Hello BlueZ függőségek telepítése:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-179">Install hello BlueZ dependencies:</span></span>

    ```sh
    sudo apt-get update
    sudo apt-get install bluetooth bluez-tools build-essential autoconf glib2.0 libglib2.0-dev libdbus-1-dev libudev-dev libical-dev libreadline-dev
    ```

1. <span data-ttu-id="2cbeb-180">Hello BlueZ forráskódja letölthető bluez.org:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-180">Download hello BlueZ source code from bluez.org:</span></span>

    ```sh
    wget http://www.kernel.org/pub/linux/bluetooth/bluez-5.37.tar.xz
    ```

1. <span data-ttu-id="2cbeb-181">Bontsa ki a forráskód hello:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-181">Unzip hello source code:</span></span>

    ```sh
    tar -xvf bluez-5.37.tar.xz
    ```

1. <span data-ttu-id="2cbeb-182">Könyvtárak toohello az újonnan létrehozott mappa módosítása:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-182">Change directories toohello newly created folder:</span></span>

    ```sh
    cd bluez-5.37
    ```

1. <span data-ttu-id="2cbeb-183">Adja meg a beépített hello BlueZ kód toobe:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-183">Configure hello BlueZ code toobe built:</span></span>

    ```sh
    ./configure --disable-udev --disable-systemd --enable-experimental
    ```

1. <span data-ttu-id="2cbeb-184">Build BlueZ:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-184">Build BlueZ:</span></span>

    ```sh
    make
    ```

1. <span data-ttu-id="2cbeb-185">Telepítse a BlueZ, ha ezzel végzett építési:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-185">Install BlueZ once it is done building:</span></span>

    ```sh
    sudo make install
    ```

1. <span data-ttu-id="2cbeb-186">Systemd bluetooth toohello új bluetooth démon hello fájlban mutat, a szolgáltatás konfigurációjának módosítása `/lib/systemd/system/bluetooth.service`.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-186">Change systemd service configuration for bluetooth so it points toohello new bluetooth daemon in hello file `/lib/systemd/system/bluetooth.service`.</span></span> <span data-ttu-id="2cbeb-187">Hello "ExecStart" sorban cserélje le a következő szöveg hello:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-187">Replace hello 'ExecStart' line with hello following text:</span></span>

    ```conf
    ExecStart=/usr/local/libexec/bluetooth/bluetoothd -E
    ```

### <a name="enable-connectivity-toohello-sensortag-device-from-your-raspberry-pi-3-device"></a><span data-ttu-id="2cbeb-188">Kapcsolat toohello SensorTag eszköz a málna Pi 3 eszközről engedélyezése</span><span class="sxs-lookup"><span data-stu-id="2cbeb-188">Enable connectivity toohello SensorTag device from your Raspberry Pi 3 device</span></span>

<span data-ttu-id="2cbeb-189">Futó hello minta előtt meg kell, hogy a málna Pi 3 csatlakozni tud-e toohello SensorTag eszköz tooverify.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-189">Before running hello sample, you need tooverify that your Raspberry Pi 3 can connect toohello SensorTag device.</span></span>

1. <span data-ttu-id="2cbeb-190">Győződjön meg arról hello `rfkill` segédprogram:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-190">Ensure hello `rfkill` utility is installed:</span></span>

    ```sh
    sudo apt-get install rfkill
    ```

1. <span data-ttu-id="2cbeb-191">A hello málna Pi 3 bluetooth feloldása, és ellenőrizze, hogy hello verziószáma **5.37**:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-191">Unblock bluetooth on hello Raspberry Pi 3 and check that hello version number is **5.37**:</span></span>

    ```sh
    sudo rfkill unblock bluetooth
    bluetoothctl --version
    ```

1. <span data-ttu-id="2cbeb-192">tooenter hello interaktív bluetooth rendszerhéj hello bluetooth szolgáltatás kiindulva hajtják végre hello **bluetoothctl** parancs:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-192">tooenter hello interactive bluetooth shell, start hello bluetooth service and execute hello **bluetoothctl** command :</span></span>

    ```sh
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. <span data-ttu-id="2cbeb-193">Adja meg a hello parancs **bekapcsolási** toopower hello Bluetooth-vezérlő be.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-193">Enter hello command **power on** toopower up hello bluetooth controller.</span></span> <span data-ttu-id="2cbeb-194">hello parancs kimenete hasonló toohello következő adja vissza:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-194">hello command returns output similar toohello following:</span></span>

    ```sh
    [NEW] Controller 98:4F:EE:04:1F:DF C3 raspberrypi [default]
    ```

1. <span data-ttu-id="2cbeb-195">Hello interaktív bluetooth rendszerhéj, adja meg a hello parancsot **vizsgálhat** tooscan bluetooth-eszközök.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-195">In hello interactive bluetooth shell, enter hello command **scan on** tooscan for bluetooth devices.</span></span> <span data-ttu-id="2cbeb-196">hello parancs kimenete hasonló toohello következő adja vissza:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-196">hello command returns output similar toohello following:</span></span>

    ```sh
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```

1. <span data-ttu-id="2cbeb-197">Hello SensorTag eszköz felderíthető tétele hello kis gomb (zöld LED kell flash hello).</span><span class="sxs-lookup"><span data-stu-id="2cbeb-197">Make hello SensorTag device discoverable by pressing hello small button (hello green LED should flash).</span></span> <span data-ttu-id="2cbeb-198">hello málna Pi 3 hello SensorTag eszköz kell észlelése:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-198">hello Raspberry Pi 3 should discover hello SensorTag device:</span></span>

    ```sh
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```

    <span data-ttu-id="2cbeb-199">Ebben a példában láthatja, hogy hello hello SensorTag az eszköz MAC-címét **A0:E6:F8:B5:F6:00**.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-199">In this example, you can see that hello MAC address of hello SensorTag device is **A0:E6:F8:B5:F6:00**.</span></span>

1. <span data-ttu-id="2cbeb-200">Kapcsolja ki hello megadásával ellenőrzését **ki vizsgálata** parancs:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-200">Turn off scanning by entering hello **scan off** command:</span></span>

    ```sh
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```

1. <span data-ttu-id="2cbeb-201">Csatlakozás tooyour SensorTag-eszközt a MAC-cím megadásával **csatlakozás \<MAC-cím\>**.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-201">Connect tooyour SensorTag device using its MAC address by entering **connect \<MAC address\>**.</span></span> <span data-ttu-id="2cbeb-202">a következő minta kimenet hello jobb érthetőség kedvéért bizonyos rövidítése:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-202">hello following sample output is abbreviated for clarity:</span></span>

    ```sh
    Attempting tooconnect tooA0:E6:F8:B5:F6:00
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

    > <span data-ttu-id="2cbeb-203">Hello GATT jellemzői hello eszközt újra hello listázhatja **lista-attribútumok** parancsot.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-203">You can list hello GATT characteristics of hello device again using hello **list-attributes** command.</span></span>

1. <span data-ttu-id="2cbeb-204">Most már leválaszthatja hello segítségével hello eszközről **leválasztása** parancsot, és zárja be a hello bluetooth rendszerhéjból hello segítségével **lépjen ki a** parancs:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-204">You can now disconnect from hello device using hello **disconnect** command and then exit from hello bluetooth shell using hello **quit** command:</span></span>

    ```sh
    Attempting toodisconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

<span data-ttu-id="2cbeb-205">Épp most már készen áll a toorun hello BLA IoT peremhálózati minta a málna Pi 3.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-205">You're now ready toorun hello BLE IoT Edge sample on your Raspberry Pi 3.</span></span>

## <a name="run-hello-iot-edge-ble-sample"></a><span data-ttu-id="2cbeb-206">Hello IoT peremhálózati BLA minta futtatásához</span><span class="sxs-lookup"><span data-stu-id="2cbeb-206">Run hello IoT Edge BLE sample</span></span>

<span data-ttu-id="2cbeb-207">toorun hello IoT peremhálózati BLA minta kell toocomplete három feladatok:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-207">toorun hello IoT Edge BLE sample, you need toocomplete three tasks:</span></span>

* <span data-ttu-id="2cbeb-208">Adja meg az IoT Hub két minta eszköz.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-208">Configure two sample devices in your IoT Hub.</span></span>
* <span data-ttu-id="2cbeb-209">Build IoT peremhálózati málna Pi 3 eszközén.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-209">Build IoT Edge on your Raspberry Pi 3 device.</span></span>
* <span data-ttu-id="2cbeb-210">Konfigurálja, és futtasson hello BLA mintát málna Pi 3 eszközén.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-210">Configure and run hello BLE sample on your Raspberry Pi 3 device.</span></span>

<span data-ttu-id="2cbeb-211">Hello írásának időpontjában IoT peremhálózati csak támogatja BLA modulok Linux rendszert futtató átjárókat.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-211">At hello time of writing, IoT Edge only supports BLE modules in gateways running on Linux.</span></span>

### <a name="configure-two-sample-devices-in-your-iot-hub"></a><span data-ttu-id="2cbeb-212">Az IoT Hub két minta eszközök konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2cbeb-212">Configure two sample devices in your IoT Hub</span></span>

* <span data-ttu-id="2cbeb-213">[Létrehoz egy IoT-központot] [ lnk-create-hub] az Azure-előfizetéshez kell hello nevét a hub toocomplete Ez a forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-213">[Create an IoT hub][lnk-create-hub] in your Azure subscription, you need hello name of your hub toocomplete this walkthrough.</span></span> <span data-ttu-id="2cbeb-214">Ha nincs fiókja, néhány perc alatt létrehozhat egy [ingyenes fiókot][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="2cbeb-214">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="2cbeb-215">Hozzáad egy eszközt nevű **SensorTag_01** tooyour IoT-központot, és jegyezze fel az azonosítót és az eszköz kulcsának.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-215">Add one device called **SensorTag_01** tooyour IoT hub and make a note of its id and device key.</span></span> <span data-ttu-id="2cbeb-216">Használhatja a hello [eszköz explorer vagy az IOT hubbal-explorer] [ lnk-explorer-tools] eszközök tooadd az eszköz toohello IoT-központ létrehozta az előző lépésben hello és tooretrieve annak kulcsát.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-216">You can use hello [device explorer or iothub-explorer][lnk-explorer-tools] tools tooadd this device toohello IoT hub you created in hello previous step and tooretrieve its key.</span></span> <span data-ttu-id="2cbeb-217">Az eszköz toohello SensorTag eszköz hello átjáró konfigurálásakor képeznek le.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-217">You map this device toohello SensorTag device when you configure hello gateway.</span></span>

### <a name="build-azure-iot-edge-on-your-raspberry-pi-3"></a><span data-ttu-id="2cbeb-218">Az Azure IoT peremhálózati létrehozása a Raspberry Pi 3-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="2cbeb-218">Build Azure IoT Edge on your Raspberry Pi 3</span></span>

<span data-ttu-id="2cbeb-219">Függőségek telepítése Azure IoT szegély:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-219">Install dependencies for Azure IoT Edge:</span></span>

```sh
sudo apt-get install cmake uuid-dev curl libcurl4-openssl-dev libssl-dev
```

<span data-ttu-id="2cbeb-220">Használjon hello következő tooclone IoT peremhálózati és minden submodules tooyour kezdőkönyvtárral parancsokat:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-220">Use hello following commands tooclone IoT Edge and all its submodules tooyour home directory:</span></span>

```sh
cd ~
git clone https://github.com/Azure/iot-edge.git
```

<span data-ttu-id="2cbeb-221">Ha rendelkezik a málna Pi 3 hello IoT peremhálózati tárház teljes másolata, hozhat létre hello parancs hello SDK tartalmazó hello mappából a következő használatával:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-221">When you have a complete copy of hello IoT Edge repository on your Raspberry Pi 3, you can build it using hello following command from hello folder that contains hello SDK:</span></span>

```sh
cd ~/iot-edge
./tools/build.sh  --disable-native-remote-modules
```

### <a name="configure-and-run-hello-ble-sample-on-your-raspberry-pi-3"></a><span data-ttu-id="2cbeb-222">Konfigurálja és hello BLA minta futtatásához a málna Pi 3</span><span class="sxs-lookup"><span data-stu-id="2cbeb-222">Configure and run hello BLE sample on your Raspberry Pi 3</span></span>

<span data-ttu-id="2cbeb-223">toobootstrap és futtatási hello minta, konfigurálnia kell minden egyes IoT peremhálózati modul, amely részt vesz az hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-223">toobootstrap and run hello sample, you must configure each IoT Edge module that participates in hello gateway.</span></span> <span data-ttu-id="2cbeb-224">Ebben a konfigurációban megadott JSON-fájlt, és konfigurálnia kell az öt részt vevő IoT peremhálózati modulok.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-224">This configuration is provided in a JSON file and you must configure all five participating IoT Edge modules.</span></span> <span data-ttu-id="2cbeb-225">Nincs JSON mintafájl nevű hello tárházban **átjáró\_sample.json** használható mint hello kiindulópont saját konfigurációs fájl létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-225">There is a sample JSON file in hello repository called **gateway\_sample.json** that you can use as hello starting point for building your own configuration file.</span></span> <span data-ttu-id="2cbeb-226">Ez a fájl megtalálható-e hello **minták/ble_gateway/src** hello IoT peremhálózati tárház helyi példányát mappájában.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-226">This file is in hello **samples/ble_gateway/src** folder in local copy of hello IoT Edge repository.</span></span>

<span data-ttu-id="2cbeb-227">hello alábbi szakaszok azt ismertetik, hogyan tooedit Ez a konfiguráció hello BLA minta fájlt, és azt feltételezik, hogy hello IoT peremhálózati tárház megtalálható-e hello **/home/pi/iot-edge /** a málna Pi 3 mappájába.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-227">hello following sections describe how tooedit this configuration file for hello BLE sample and assume that hello IoT Edge repository is in hello **/home/pi/iot-edge/** folder on your Raspberry Pi 3.</span></span> <span data-ttu-id="2cbeb-228">Ha hello tárház máshol, módosítsa a hello elérési utak ennek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-228">If hello repository is elsewhere, adjust hello paths accordingly.</span></span>

#### <a name="logger-configuration"></a><span data-ttu-id="2cbeb-229">Naplózási konfiguráció</span><span class="sxs-lookup"><span data-stu-id="2cbeb-229">Logger configuration</span></span>

<span data-ttu-id="2cbeb-230">Feltéve, hogy hello átjáró tárházban található hello **/home/pi/iot-edge /** mappa, hello naplózó modul konfigurálása az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-230">Assuming hello gateway repository is located in hello **/home/pi/iot-edge/** folder, configure hello logger module as follows:</span></span>

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

#### <a name="ble-module-configuration"></a><span data-ttu-id="2cbeb-231">BLA modul konfigurációja</span><span class="sxs-lookup"><span data-stu-id="2cbeb-231">BLE module configuration</span></span>

<span data-ttu-id="2cbeb-232">hello mintakonfiguráció hello BLA eszköz azt feltételezi, hogy egy Texas eszközök SensorTag eszközt.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-232">hello sample configuration for hello BLE device assumes a Texas Instruments SensorTag device.</span></span> <span data-ttu-id="2cbeb-233">Bármely szabványos BLA eszköz, amely működhet, a perifériák GATT kell működnie, de szükség lehet a tooupdate hello GATT jellemző azonosítók és adatokat.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-233">Any standard BLE device that can operate as a GATT peripheral should work but you may need tooupdate hello GATT characteristic IDs and data.</span></span> <span data-ttu-id="2cbeb-234">Adja hozzá az SensorTag eszköz hello MAC-címe:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-234">Add hello MAC address of your SensorTag device:</span></span>

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

<span data-ttu-id="2cbeb-235">Ha nem használ egy SensorTag eszközt, tekintse át a Gedélyezése eszköz toodetermine hello dokumentációja tooupdate hello GATT jellemző azonosítókat és az adatértékek szüksége van.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-235">If you are not using a SensorTag device, review hello documentation for your BLE device toodetermine whether you need tooupdate hello GATT characteristic IDs and data values.</span></span>

#### <a name="iot-hub-module"></a><span data-ttu-id="2cbeb-236">Az IoT-központ modulja</span><span class="sxs-lookup"><span data-stu-id="2cbeb-236">IoT Hub module</span></span>

<span data-ttu-id="2cbeb-237">Adja hozzá az IoT Hub hello nevét.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-237">Add hello name of your IoT Hub.</span></span> <span data-ttu-id="2cbeb-238">hello utótag érték általában **azure-devices.net**:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-238">hello suffix value is typically **azure-devices.net**:</span></span>

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

#### <a name="identity-mapping-module-configuration"></a><span data-ttu-id="2cbeb-239">Identitás-hozzárendelési modul konfigurációja</span><span class="sxs-lookup"><span data-stu-id="2cbeb-239">Identity mapping module configuration</span></span>

<span data-ttu-id="2cbeb-240">Adja hozzá a MAC-címét hello a SensorTag eszköz és hello eszköz azonosítója és kulcsa hello **SensorTag_01** hozzáadott tooyour IoT Hub eszköz:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-240">Add hello MAC address of your SensorTag device and hello device ID and key of hello **SensorTag_01** device you added tooyour IoT Hub:</span></span>

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

#### <a name="ble-printer-module-configuration"></a><span data-ttu-id="2cbeb-241">BLA nyomtató modul konfigurációja</span><span class="sxs-lookup"><span data-stu-id="2cbeb-241">BLE Printer module configuration</span></span>

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

#### <a name="blec2d-module-configuration"></a><span data-ttu-id="2cbeb-242">BLEC2D modul konfigurációja</span><span class="sxs-lookup"><span data-stu-id="2cbeb-242">BLEC2D Module Configuration</span></span>

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

#### <a name="routing-configuration"></a><span data-ttu-id="2cbeb-243">Útválasztási konfigurációja</span><span class="sxs-lookup"><span data-stu-id="2cbeb-243">Routing Configuration</span></span>

<span data-ttu-id="2cbeb-244">hello következő a konfiguráció biztosítja hello következő IoT peremhálózati modulok között:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-244">hello following configuration ensures hello following routing between IoT Edge modules:</span></span>

* <span data-ttu-id="2cbeb-245">Hello **naplózó** modul kap, és minden üzenetet naplózza.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-245">hello **Logger** module receives and logs all messages.</span></span>
* <span data-ttu-id="2cbeb-246">Hello **SensorTag** modul küld üzeneteket tooboth hello **leképezési** és **BLA nyomtató** modulok.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-246">hello **SensorTag** module sends messages tooboth hello **mapping** and **BLE Printer** modules.</span></span>
* <span data-ttu-id="2cbeb-247">Hello **leképezési** modul küld üzeneteket toohello **IOT hubbal** modul toobe tooyour IoT-központ felküldve.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-247">hello **mapping** module sends messages toohello **IoTHub** module toobe sent up tooyour IoT Hub.</span></span>
* <span data-ttu-id="2cbeb-248">Hello **IOT hubbal** modul küld üzenetek biztonsági toohello **leképezési** modul.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-248">hello **IoTHub** module sends messages back toohello **mapping** module.</span></span>
* <span data-ttu-id="2cbeb-249">Hello **leképezési** modul küld üzeneteket toohello **BLEC2D** modul.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-249">hello **mapping** module sends messages toohello **BLEC2D** module.</span></span>
* <span data-ttu-id="2cbeb-250">Hello **BLEC2D** modul küld üzenetek biztonsági toohello **Sensor Tag** modul.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-250">hello **BLEC2D** module sends messages back toohello **Sensor Tag** module.</span></span>

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

<span data-ttu-id="2cbeb-251">toorun hello minta, mint egy paraméterrel toohello fázis hello elérési toohello JSON konfigurációs fájl **gedélyezése\_átjáró** bináris.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-251">toorun hello sample, pass hello path toohello JSON configuration file as a parameter toohello **ble\_gateway** binary.</span></span> <span data-ttu-id="2cbeb-252">hello alábbi parancs feltételezi a hello **gateway_sample.json** konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-252">hello following command assumes you are using hello **gateway_sample.json** configuration file.</span></span> <span data-ttu-id="2cbeb-253">Ez a parancs végrehajtása a hello **iot-edge** hello málna Pi mappájában:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-253">Execute this command from hello **iot-edge** folder on hello Raspberry Pi:</span></span>

```sh
./build/samples/ble_gateway/ble_gateway ./samples/ble_gateway/src/gateway_sample.json
```

<span data-ttu-id="2cbeb-254">Szükség lehet a toopress kis hello gombra hello SensorTag eszköz toomake azt felderíthető hello minta futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-254">You may need toopress hello small button on hello SensorTag device toomake it discoverable before you run hello sample.</span></span>

<span data-ttu-id="2cbeb-255">Hello minta futtatásához használhatja hello [eszköz explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) vagy hello [IOT hubbal-explorer](https://github.com/Azure/iothub-explorer) eszköz toomonitor hello üzenetek hello IoT peremhálózati átjáró hello SensorTag eszközről továbbítja.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-255">When you run hello sample, you can use hello [device explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) or hello [iothub-explorer](https://github.com/Azure/iothub-explorer) tool toomonitor hello messages hello IoT Edge gateway forwards from hello SensorTag device.</span></span> <span data-ttu-id="2cbeb-256">Például az IOT hubbal-Explorerben figyelheti eszközről a felhőbe üzenetek hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-256">For example, using iothub-explorer you can monitor device-to-cloud messages using hello following command:</span></span>

```sh
iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
```

## <a name="send-cloud-to-device-messages"></a><span data-ttu-id="2cbeb-257">Üzenetküldés a felhőből az eszközökre</span><span class="sxs-lookup"><span data-stu-id="2cbeb-257">Send cloud-to-device messages</span></span>

<span data-ttu-id="2cbeb-258">hello BLA modul támogatja az IoT-központ toohello eszközről küldő parancsok is.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-258">hello BLE module also supports sending commands from IoT Hub toohello device.</span></span> <span data-ttu-id="2cbeb-259">Használhatja a hello [eszköz explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) vagy hello [IOT hubbal-explorer](https://github.com/Azure/iothub-explorer) eszköz toosend JSON üzenetek hello BLA átjáró modult toohello BLA eszközön továbbítja.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-259">You can use hello [device explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer) or hello [iothub-explorer](https://github.com/Azure/iothub-explorer) tool toosend JSON messages that hello BLE gateway module forwards on toohello BLE device.</span></span>

<span data-ttu-id="2cbeb-260">Ha hello Texas eszközök SensorTag eszközt használ, bekapcsolhatja a piros hello LED-jét, zöld LED vagy berregő parancsokat küld az IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-260">If you are using hello Texas Instruments SensorTag device, you can turn on hello red LED, green LED, or buzzer by sending commands from IoT Hub.</span></span> <span data-ttu-id="2cbeb-261">Az IoT-központ küldeni a parancsok, először küldése a következő két JSON üzenetek sorrendben hello.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-261">Before you send commands from IoT Hub, first send hello following two JSON messages in order.</span></span> <span data-ttu-id="2cbeb-262">Ezután bármelyik hello parancsok tooturn hello fény vagy berregő küldhet.</span><span class="sxs-lookup"><span data-stu-id="2cbeb-262">Then you can send any of hello commands tooturn on hello lights or buzzer.</span></span>

1. <span data-ttu-id="2cbeb-263">Alaphelyzetbe állítja az összes LED és hello berregő (kikapcsolni őket):</span><span class="sxs-lookup"><span data-stu-id="2cbeb-263">Reset all LEDs and hello buzzer (turn them off):</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```

1. <span data-ttu-id="2cbeb-264">"Távoli" i/o konfigurálni:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-264">Configure I/O as 'remote':</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

<span data-ttu-id="2cbeb-265">Küldheti el a következő parancsok tooturn hello fény vagy hello SensorTag eszközön berregő hello:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-265">Now you can send any of hello following commands tooturn on hello lights or buzzer on hello SensorTag device:</span></span>

* <span data-ttu-id="2cbeb-266">Piros hello LED bekapcsolása:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-266">Turn on hello red LED:</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

* <span data-ttu-id="2cbeb-267">Zöld hello LED bekapcsolása:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-267">Turn on hello green LED:</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```

* <span data-ttu-id="2cbeb-268">Hello berregő bekapcsolása:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-268">Turn on hello buzzer:</span></span>

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

## <a name="next-steps"></a><span data-ttu-id="2cbeb-269">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2cbeb-269">Next Steps</span></span>

<span data-ttu-id="2cbeb-270">Ha toogain tájékozottabbak IoT széle és az egyes kódpéldák kísérletezhet, keresse fel hello fejlesztői oktatóanyagok és erőforrások:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-270">If you want toogain a more advanced understanding of IoT Edge and experiment with some code examples, visit hello following developer tutorials and resources:</span></span>

* <span data-ttu-id="2cbeb-271">[Az Azure IoT él][lnk-sdk]</span><span class="sxs-lookup"><span data-stu-id="2cbeb-271">[Azure IoT Edge][lnk-sdk]</span></span>

<span data-ttu-id="2cbeb-272">toofurther megismerkedhet az IoT-központ hello képességeit, lásd:</span><span class="sxs-lookup"><span data-stu-id="2cbeb-272">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="2cbeb-273">[IoT Hub fejlesztői útmutató][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="2cbeb-273">[IoT Hub developer guide][lnk-devguide]</span></span>

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
