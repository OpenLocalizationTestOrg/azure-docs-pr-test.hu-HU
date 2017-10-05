---
title: "Az Azure IoT - lecke 1 Connect Raspberry pi (C): eszközök konfigurálása |} Microsoft Docs"
description: "Málna Pi 3 konfigurálása az első használatra, és telepítse a Raspbian az operációs rendszer, a szabad operációs rendszer, amely a málna Pi hardver van optimalizálva."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "telepítés raspbian, raspbian letöltése, telepítése raspbian, raspbian beállítás raspberry pi telepítési raspbian, raspberry pi telepítése operációs rendszer, raspberry pi sd-kártya telepítése, málna pi csatlakozásának, csatlakozni raspberry pi raspberry pi kapcsolat"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-c-get-started
ms.assetid: 8ee9b23c-93f7-43ff-8ea1-e7761eb87a6f
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 2a380f78d67db47a0dcab5b90843404921510528
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-your-device"></a><span data-ttu-id="b319d-104">Az eszköz konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b319d-104">Configure your device</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="b319d-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="b319d-105">What you will do</span></span>
<span data-ttu-id="b319d-106">Az első használatnál Pi konfigurálja, és a Raspbian operációs rendszer telepítése.</span><span class="sxs-lookup"><span data-stu-id="b319d-106">Configure Pi for first-time use and install the Raspbian operating system.</span></span> <span data-ttu-id="b319d-107">Raspbian egy ingyenes operációs rendszer, amely a málna Pi hardver van optimalizálva.</span><span class="sxs-lookup"><span data-stu-id="b319d-107">Raspbian is a free operating system that is optimized for the Raspberry Pi hardware.</span></span> <span data-ttu-id="b319d-108">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="b319d-108">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-raspberry-pi-kit-c-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="b319d-109">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="b319d-109">What you will learn</span></span>
<span data-ttu-id="b319d-110">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="b319d-110">In this article, you will learn:</span></span>

* <span data-ttu-id="b319d-111">Hogyan Raspbian Pi telepítése.</span><span class="sxs-lookup"><span data-stu-id="b319d-111">How to install Raspbian on Pi.</span></span>
* <span data-ttu-id="b319d-112">Hogyan lehet a Pi bekapcsolására, USB-kábelen keresztül.</span><span class="sxs-lookup"><span data-stu-id="b319d-112">How to power up Pi by using a USB cable.</span></span>
* <span data-ttu-id="b319d-113">Hogyan Pi csatlakozhat a hálózati kábel vagy vezeték nélküli hálózat használatával.</span><span class="sxs-lookup"><span data-stu-id="b319d-113">How to connect Pi to the network by using an Ethernet cable or wireless network.</span></span>
* <span data-ttu-id="b319d-114">Hogyan LED hozzáadása a breadboard Pi csatlakoztassa.</span><span class="sxs-lookup"><span data-stu-id="b319d-114">How to add an LED to the breadboard and connect it to Pi.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="b319d-115">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="b319d-115">What you need</span></span>
<span data-ttu-id="b319d-116">A művelet végrehajtásához a következő részek a málna Pi 3 Starter Kit kell:</span><span class="sxs-lookup"><span data-stu-id="b319d-116">To complete this operation, you need the following parts from your Raspberry Pi 3 Starter Kit:</span></span>

* <span data-ttu-id="b319d-117">A Pi 3 málna tábla</span><span class="sxs-lookup"><span data-stu-id="b319d-117">The Raspberry Pi 3 board</span></span>
* <span data-ttu-id="b319d-118">A 16 GB-os microSD-kártyán</span><span class="sxs-lookup"><span data-stu-id="b319d-118">The 16-GB microSD card</span></span>
* <span data-ttu-id="b319d-119">Az 5-volt 2-amp tápegység a 6-mértékű kiszolgálóhasználat micro USB-kábellel</span><span class="sxs-lookup"><span data-stu-id="b319d-119">The 5-volt 2-amp power supply with the 6-foot micro USB cable</span></span>
* <span data-ttu-id="b319d-120">A breadboard</span><span class="sxs-lookup"><span data-stu-id="b319d-120">The breadboard</span></span>
* <span data-ttu-id="b319d-121">Összekötő fenyegetéseknek</span><span class="sxs-lookup"><span data-stu-id="b319d-121">Connector wires</span></span>
* <span data-ttu-id="b319d-122">Egy 560 ohmos ellenállás</span><span class="sxs-lookup"><span data-stu-id="b319d-122">A 560-ohm resistor</span></span>
* <span data-ttu-id="b319d-123">Egy szórt 10 mm LED</span><span class="sxs-lookup"><span data-stu-id="b319d-123">A diffused 10-mm LED</span></span>
* <span data-ttu-id="b319d-124">Az Ethernet-kábel</span><span class="sxs-lookup"><span data-stu-id="b319d-124">The Ethernet cable</span></span>

![Az Starter Kit dolgot](media/iot-hub-raspberry-pi-lessons/lesson1/starter_kit.jpg)

<span data-ttu-id="b319d-126">Emellett a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="b319d-126">You also need:</span></span>

* <span data-ttu-id="b319d-127">Vezetékes vagy vezeték nélküli kapcsolat pi való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="b319d-127">A wired or wireless connection for Pi to connect to.</span></span>
* <span data-ttu-id="b319d-128">Egy USB-SD adapter vagy mini-SD-kártyán írása az operációs rendszer képet a microSD-kártyán.</span><span class="sxs-lookup"><span data-stu-id="b319d-128">A USB-SD adapter or mini-SD card to burn the OS image onto the microSD card.</span></span>
* <span data-ttu-id="b319d-129">Windows, Mac vagy Linux rendszerű számítógép.</span><span class="sxs-lookup"><span data-stu-id="b319d-129">A computer running Windows, Mac, or Linux.</span></span> <span data-ttu-id="b319d-130">A számítógép a microSD-kártyán Raspbian telepítéséhez használt.</span><span class="sxs-lookup"><span data-stu-id="b319d-130">The computer is used to install Raspbian on the microSD card.</span></span>
* <span data-ttu-id="b319d-131">Az internethez, a szükséges eszközöket és a szoftverek letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="b319d-131">An Internet connection to download the necessary tools and software.</span></span>

## <a name="install-raspbian-on-the-microsd-card"></a><span data-ttu-id="b319d-132">Telepítse a MicroSD-kártyán Raspbian</span><span class="sxs-lookup"><span data-stu-id="b319d-132">Install Raspbian on the MicroSD card</span></span>
<span data-ttu-id="b319d-133">Készítse elő a microSD-kártyán Raspbian kép telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="b319d-133">Prepare the microSD card for installation of the Raspbian image.</span></span>

1. <span data-ttu-id="b319d-134">Töltse le a Raspbian.</span><span class="sxs-lookup"><span data-stu-id="b319d-134">Download Raspbian.</span></span>
   1. <span data-ttu-id="b319d-135">[Töltse le](https://www.raspberrypi.org/downloads/raspbian/) Raspbian Jessie a Pixel a .zip fájlt.</span><span class="sxs-lookup"><span data-stu-id="b319d-135">[Download](https://www.raspberrypi.org/downloads/raspbian/) the .zip file for Raspbian Jessie with Pixel.</span></span>
   2. <span data-ttu-id="b319d-136">Bontsa ki a Raspbian lemezképet a számítógép egyik mappájába.</span><span class="sxs-lookup"><span data-stu-id="b319d-136">Extract the Raspbian image to a folder on your computer.</span></span>
2. <span data-ttu-id="b319d-137">A microSD-kártyán Raspbian telepítése.</span><span class="sxs-lookup"><span data-stu-id="b319d-137">Install Raspbian to the microSD card.</span></span>
   1. <span data-ttu-id="b319d-138">[Töltse le](https://www.etcher.io) és telepítse a Etcher SD-kártya író segédprogramot.</span><span class="sxs-lookup"><span data-stu-id="b319d-138">[Download](https://www.etcher.io) and install the Etcher SD card burner utility.</span></span>
   2. <span data-ttu-id="b319d-139">Futtassa a Etcher, és az 1. lépésben válassza ki a kibontott Raspbian kép.</span><span class="sxs-lookup"><span data-stu-id="b319d-139">Run Etcher and select the Raspbian image that you extracted in step 1.</span></span>
   3. <span data-ttu-id="b319d-140">Jelölje ki a microSD-kártyát meghajtót.</span><span class="sxs-lookup"><span data-stu-id="b319d-140">Select the microSD card drive.</span></span>
      <span data-ttu-id="b319d-141">Vegye figyelembe, hogy Etcher előfordulhat, hogy már választott ki a megfelelő meghajtó.</span><span class="sxs-lookup"><span data-stu-id="b319d-141">Note that Etcher may have already selected the correct drive.</span></span>
   4. <span data-ttu-id="b319d-142">Kattintson a **Flash** a microSD-kártyán Raspbian telepítése.</span><span class="sxs-lookup"><span data-stu-id="b319d-142">Click **Flash** to install Raspbian to the microSD card.</span></span>
   5. <span data-ttu-id="b319d-143">Eltávolítja a számítógépről a microSD-kártyán, ha a telepítés befejeződött.</span><span class="sxs-lookup"><span data-stu-id="b319d-143">Remove the microSD card from your computer when installation is complete.</span></span>
      <span data-ttu-id="b319d-144">Biztonságos a microSD-kártyán közvetlenül eltávolítani, mert Etcher automatikusan kiadása vagy leválasztja a microSD-kártyán befejezése után is.</span><span class="sxs-lookup"><span data-stu-id="b319d-144">It is safe to remove the microSD card directly because Etcher automatically ejects or unmounts the microSD card upon completion.</span></span>
   6. <span data-ttu-id="b319d-145">A Pi a microSD-kártyán beilleszteni.</span><span class="sxs-lookup"><span data-stu-id="b319d-145">Insert the microSD card into your Pi.</span></span>

![Az SD-kártya beszúrása](media/iot-hub-raspberry-pi-lessons/lesson1/insert_sdcard.jpg)

## <a name="turn-on-pi"></a><span data-ttu-id="b319d-147">A Pi bekapcsolása</span><span class="sxs-lookup"><span data-stu-id="b319d-147">Turn on Pi</span></span>
<span data-ttu-id="b319d-148">Kapcsolja be a Pi a micro USB-kábelen és a tápegység.</span><span class="sxs-lookup"><span data-stu-id="b319d-148">Turn on Pi by using the micro USB cable and the power supply.</span></span>

![Bekapcsolás](media/iot-hub-raspberry-pi-lessons/lesson1/micro_usb_power_on.jpg)

> [!NOTE]
> <span data-ttu-id="b319d-150">Fontos, hogy a tápegység használja a csomag, amely nem alacsonyabb 2A. Győződjön meg arról, hogy a málna rendelkezik-e elegendő power megfelelő működéséhez.</span><span class="sxs-lookup"><span data-stu-id="b319d-150">It is important to use the power supply in the kit that is at least 2A to make sure that your Raspberry has enough power to work correctly.</span></span>

## <a name="enable-ssh"></a><span data-ttu-id="b319d-151">SSH engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b319d-151">Enable SSH</span></span>
<span data-ttu-id="b319d-152">A November 2016 kiadás Raspbian rendelkezik az SSH-kiszolgálót, alapértelmezés szerint le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="b319d-152">As of the November 2016 release, Raspbian has the SSH server disabled by default.</span></span> <span data-ttu-id="b319d-153">Engedélyezze manuálisan kell.</span><span class="sxs-lookup"><span data-stu-id="b319d-153">You need to enable it manually.</span></span> <span data-ttu-id="b319d-154">Olvassa el a [hivatalos utasításokat](https://www.raspberrypi.org/documentation/remote-access/ssh/) vagy csatlakoztassa egy figyelő, és navigáljon **beállítások -> málna Pi konfigurációs** SSH engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="b319d-154">You can refer to the [official instructions](https://www.raspberrypi.org/documentation/remote-access/ssh/) or connect a monitor and go to **Preferences -> Raspberry Pi Configuration** to enable SSH.</span></span>

## <a name="connect-raspberry-pi-3-to-the-network"></a><span data-ttu-id="b319d-155">Málna Pi 3 csatlakoznak a hálózathoz</span><span class="sxs-lookup"><span data-stu-id="b319d-155">Connect Raspberry Pi 3 to the network</span></span>
<span data-ttu-id="b319d-156">A Pi kapcsolódhatnak egy vezetékes hálózathoz vagy vezeték nélküli hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="b319d-156">You can connect Pi to a wired network or to a wireless network.</span></span> <span data-ttu-id="b319d-157">Győződjön meg arról, hogy a Pi és a számítógép ugyanahhoz a hálózathoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="b319d-157">Make sure that Pi is connected to the same network as your computer.</span></span> <span data-ttu-id="b319d-158">Például Pi is csatlakozik ugyanahhoz a kapcsolóhoz, amely a számítógép csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="b319d-158">For example, you can connect Pi to the same switch that your computer is connected to.</span></span>

### <a name="connect-to-a-wired-network"></a><span data-ttu-id="b319d-159">Kapcsolódjon vezetékes hálózathoz</span><span class="sxs-lookup"><span data-stu-id="b319d-159">Connect to a wired network</span></span>
<span data-ttu-id="b319d-160">Az Ethernet kábellel Pi a vezetékes hálózathoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="b319d-160">Use the Ethernet cable to connect Pi to your wired network.</span></span> <span data-ttu-id="b319d-161">A Pi két LED bekapcsolása, ha létrejött a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="b319d-161">The two LEDs on Pi turn on if the connection is established.</span></span>

![Csatlakozás az Ethernet-kábel segítségével](media/iot-hub-raspberry-pi-lessons/lesson1/connect_ethernet.jpg)

### <a name="connect-to-a-wireless-network"></a><span data-ttu-id="b319d-163">A vezeték nélküli hálózat</span><span class="sxs-lookup"><span data-stu-id="b319d-163">Connect to a wireless network</span></span>
<span data-ttu-id="b319d-164">Kövesse a [utasításokat](https://www.raspberrypi.org/learning/software-guide/wifi/) a Pi a vezeték nélküli hálózathoz való kapcsolódáshoz málna Pi alapját.</span><span class="sxs-lookup"><span data-stu-id="b319d-164">Follow the [instructions](https://www.raspberrypi.org/learning/software-guide/wifi/) from the Raspberry Pi Foundation to connect Pi to your wireless network.</span></span> <span data-ttu-id="b319d-165">Ezek az utasítások igényli, hogy először csatlakoztassa a figyelő és a billentyűzet Pi.</span><span class="sxs-lookup"><span data-stu-id="b319d-165">These instructions require you to first connect a monitor and a keyboard to Pi.</span></span>

## <a name="connect-the-led-to-pi"></a><span data-ttu-id="b319d-166">Csatlakozás a LED Pi</span><span class="sxs-lookup"><span data-stu-id="b319d-166">Connect the LED to Pi</span></span>
<span data-ttu-id="b319d-167">A feladat végrehajtásához használja a [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), az összekötő fenyegetéseknek, a LED-jét, és a ellenállás.</span><span class="sxs-lookup"><span data-stu-id="b319d-167">To complete this task, use the [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), the connector wires, the LED, and the resistor.</span></span> <span data-ttu-id="b319d-168">Csatlakoztassa őket, hogy a [általános célú bemeneti/kimeneti](https://www.raspberrypi.org/documentation/usage/gpio/) pi (GPIO) portok.</span><span class="sxs-lookup"><span data-stu-id="b319d-168">Connect them to the [general-purpose input/output](https://www.raspberrypi.org/documentation/usage/gpio/) (GPIO) ports of Pi.</span></span>

![Breadboard LED és ellenállás](media/iot-hub-raspberry-pi-lessons/lesson1/breadboard_led_resistor.jpg)

1. <span data-ttu-id="b319d-170">Csatlakozás a LED rövidebb szakasza **GPIO GND (PIN-kód 6)**.</span><span class="sxs-lookup"><span data-stu-id="b319d-170">Connect the shorter leg of the LED to **GPIO GND (Pin 6)**.</span></span>
2. <span data-ttu-id="b319d-171">Csatlakoztassa a LED hosszabb szakasza a ellenállás egy szakasza.</span><span class="sxs-lookup"><span data-stu-id="b319d-171">Connect the longer leg of the LED to one leg of the resistor.</span></span>
3. <span data-ttu-id="b319d-172">Csatlakoztassa a többi szakasza a ellenállás **GPIO 4 (PIN-kód 7)**.</span><span class="sxs-lookup"><span data-stu-id="b319d-172">Connect the other leg of the resistor to **GPIO 4 (Pin 7)**.</span></span>

<span data-ttu-id="b319d-173">Ne feledje, hogy a LED polaritás fontos.</span><span class="sxs-lookup"><span data-stu-id="b319d-173">Note that the LED polarity is important.</span></span> <span data-ttu-id="b319d-174">Ez a beállítás polaritás aktív alacsony gyakran nevezik.</span><span class="sxs-lookup"><span data-stu-id="b319d-174">This polarity setting is commonly known as Active Low.</span></span>

![Átalakítókábelre](media/iot-hub-raspberry-pi-lessons/lesson1/pinout_breadboard.png)

<span data-ttu-id="b319d-176">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="b319d-176">Congratulations!</span></span> <span data-ttu-id="b319d-177">Sikeresen konfigurálta az Pi.</span><span class="sxs-lookup"><span data-stu-id="b319d-177">You've successfully configured Pi.</span></span>

## <a name="summary"></a><span data-ttu-id="b319d-178">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="b319d-178">Summary</span></span>
<span data-ttu-id="b319d-179">Ebben a cikkben megtanulta már Raspbian telepítésével, Pi csatlakoznak a hálózathoz, és LED kapcsolódás Pi Pi konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="b319d-179">In this article, you’ve learned how to configure Pi by installing Raspbian, connecting Pi to a network, and connecting an LED to Pi.</span></span> <span data-ttu-id="b319d-180">Vegye figyelembe, hogy a LED nem még bonyolít le.</span><span class="sxs-lookup"><span data-stu-id="b319d-180">Note that the LED doesn't yet light up.</span></span> <span data-ttu-id="b319d-181">A következő feladathoz a rendszer telepíti a szükséges eszközök és szoftverek mintaalkalmazás futó Pi előkészítésekor.</span><span class="sxs-lookup"><span data-stu-id="b319d-181">The next task is to install the necessary tools and software in preparation for running a sample application on Pi.</span></span>

![Készen áll a hardver](media/iot-hub-raspberry-pi-lessons/lesson1/hardware_ready.jpg)

## <a name="next-steps"></a><span data-ttu-id="b319d-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b319d-183">Next steps</span></span>
[<span data-ttu-id="b319d-184">Eszközök</span><span class="sxs-lookup"><span data-stu-id="b319d-184">Get the tools</span></span>](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)

