---
title: "Csatlakozás málna Pi (csomópont) tooAzure IoT - lecke 1: eszközök konfigurálása |} Microsoft Docs"
description: "Málna Pi 3 konfigurálása az első használatra, és telepítse a hello Raspbian OS, egy ingyenes operációs rendszer, amely hello málna Pi hardver van optimalizálva."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "telepítés raspbian, raspbian letöltési tooinstall raspbian raspbian raspberry a telepítés pi telepítése raspbian raspberry pi telepítése operációs rendszer, raspberry pi sd kártya telepítés raspberry pi csatlakozásának, csatlakozás tooraspberry pi raspberry pi kapcsolat"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started
ms.assetid: 43f7c2cf-f1a5-4dd5-93f0-7e546c6dc91e
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 504a4d2a3f29717f955530812442cce2a78a6448
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-your-device"></a><span data-ttu-id="a5da7-104">Az eszköz konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a5da7-104">Configure your device</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="a5da7-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="a5da7-105">What you will do</span></span>
<span data-ttu-id="a5da7-106">Az első használatnál Pi konfigurálja, és hello Raspbian operációs rendszer telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="a5da7-106">Configure Pi for first-time use and install hello Raspbian operating system.</span></span> <span data-ttu-id="a5da7-107">Raspbian egy ingyenes operációs rendszer, amely hello málna Pi hardver van optimalizálva.</span><span class="sxs-lookup"><span data-stu-id="a5da7-107">Raspbian is a free operating system that is optimized for hello Raspberry Pi hardware.</span></span> <span data-ttu-id="a5da7-108">Ha bármilyen problémába ütközik, ugorhatnak hello megoldások [oldal hibaelhárítási](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="a5da7-108">If you have any problems, you can seek solutions on hello [troubleshooting page](iot-hub-raspberry-pi-kit-node-troubleshooting.md).</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="a5da7-109">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="a5da7-109">What you will learn</span></span>
<span data-ttu-id="a5da7-110">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="a5da7-110">In this article, you will learn:</span></span>

* <span data-ttu-id="a5da7-111">Hogyan tooinstall a Pi Raspbian.</span><span class="sxs-lookup"><span data-stu-id="a5da7-111">How tooinstall Raspbian on Pi.</span></span>
* <span data-ttu-id="a5da7-112">Hogyan toopower Pi mentése USB-kábelen keresztül.</span><span class="sxs-lookup"><span data-stu-id="a5da7-112">How toopower up Pi by using a USB cable.</span></span>
* <span data-ttu-id="a5da7-113">Hogyan tooconnect Pi toohello hálózati kábel vagy vezeték nélküli hálózat használatával.</span><span class="sxs-lookup"><span data-stu-id="a5da7-113">How tooconnect Pi toohello network by using an Ethernet cable or wireless network.</span></span>
* <span data-ttu-id="a5da7-114">Hogyan tooadd egy LED toohello breadboard és tooPi csatlakoztassa.</span><span class="sxs-lookup"><span data-stu-id="a5da7-114">How tooadd an LED toohello breadboard and connect it tooPi.</span></span>

## <a name="what-you-will-need"></a><span data-ttu-id="a5da7-115">Mit kell</span><span class="sxs-lookup"><span data-stu-id="a5da7-115">What you will need</span></span>
<span data-ttu-id="a5da7-116">toocomplete Ez a művelet következő részek a málna Pi 3 Starter Kit hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="a5da7-116">toocomplete this operation, you need hello following parts from your Raspberry Pi 3 Starter Kit:</span></span>

* <span data-ttu-id="a5da7-117">hello málna Pi 3 tábla</span><span class="sxs-lookup"><span data-stu-id="a5da7-117">hello Raspberry Pi 3 board</span></span>
* <span data-ttu-id="a5da7-118">hello 16 GB-os microSD-kártyán</span><span class="sxs-lookup"><span data-stu-id="a5da7-118">hello 16-GB microSD card</span></span>
* <span data-ttu-id="a5da7-119">hello 5-volt 2-amp tápegység hello 6-mértékű kiszolgálóhasználat micro USB-kábellel</span><span class="sxs-lookup"><span data-stu-id="a5da7-119">hello 5-volt 2-amp power supply with hello 6-foot micro USB cable</span></span>
* <span data-ttu-id="a5da7-120">hello breadboard</span><span class="sxs-lookup"><span data-stu-id="a5da7-120">hello breadboard</span></span>
* <span data-ttu-id="a5da7-121">Összekötő fenyegetéseknek</span><span class="sxs-lookup"><span data-stu-id="a5da7-121">Connector wires</span></span>
* <span data-ttu-id="a5da7-122">Egy 560 ohmos ellenállás</span><span class="sxs-lookup"><span data-stu-id="a5da7-122">A 560-ohm resistor</span></span>
* <span data-ttu-id="a5da7-123">Egy szórt 10 mm LED</span><span class="sxs-lookup"><span data-stu-id="a5da7-123">A diffused 10-mm LED</span></span>
* <span data-ttu-id="a5da7-124">hello kábel</span><span class="sxs-lookup"><span data-stu-id="a5da7-124">hello Ethernet cable</span></span>

![Az Starter Kit dolgot](media/iot-hub-raspberry-pi-lessons/lesson1/starter_kit.jpg)

<span data-ttu-id="a5da7-126">Emellett a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="a5da7-126">You also need:</span></span>

* <span data-ttu-id="a5da7-127">A Pi tooconnect a vezetékes vagy vezeték nélküli kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="a5da7-127">A wired or wireless connection for Pi tooconnect to.</span></span>
* <span data-ttu-id="a5da7-128">Egy USB-SD adapter vagy miniSD kártya tooburn hello operációsrendszer-képet hello microSD-kártyán.</span><span class="sxs-lookup"><span data-stu-id="a5da7-128">A USB-SD adapter or miniSD card tooburn hello operating system image onto hello microSD card.</span></span>
* <span data-ttu-id="a5da7-129">Windows, Mac vagy Linux rendszerű számítógép.</span><span class="sxs-lookup"><span data-stu-id="a5da7-129">A computer running Windows, Mac, or Linux.</span></span> <span data-ttu-id="a5da7-130">hello számítógép használt tooinstall Raspbian hello microSD-kártyán.</span><span class="sxs-lookup"><span data-stu-id="a5da7-130">hello computer is used tooinstall Raspbian on hello microSD card.</span></span>
* <span data-ttu-id="a5da7-131">Az Internet kapcsolat toodownload hello szükséges eszközök és szoftverek.</span><span class="sxs-lookup"><span data-stu-id="a5da7-131">An Internet connection toodownload hello necessary tools and software.</span></span>

## <a name="install-raspbian-on-hello-microsd-card"></a><span data-ttu-id="a5da7-132">Hello microSD-kártyán Raspbian telepítése</span><span class="sxs-lookup"><span data-stu-id="a5da7-132">Install Raspbian on hello microSD card</span></span>
<span data-ttu-id="a5da7-133">Készítse elő a hello microSD-kártyán hello Raspbian lemezkép telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="a5da7-133">Prepare hello microSD card for installation of hello Raspbian image.</span></span>

1. <span data-ttu-id="a5da7-134">Töltse le a Raspbian.</span><span class="sxs-lookup"><span data-stu-id="a5da7-134">Download Raspbian.</span></span>
   1. <span data-ttu-id="a5da7-135">[Töltse le](https://www.raspberrypi.org/downloads/raspbian/) hello .zip-fájlt a Pixel Raspbian Jessie.</span><span class="sxs-lookup"><span data-stu-id="a5da7-135">[Download](https://www.raspberrypi.org/downloads/raspbian/) hello .zip file for Raspbian Jessie with Pixel.</span></span>
   2. <span data-ttu-id="a5da7-136">Bontsa ki a hello Raspbian kép tooa mappát a számítógépén.</span><span class="sxs-lookup"><span data-stu-id="a5da7-136">Extract hello Raspbian image tooa folder on your computer.</span></span>
2. <span data-ttu-id="a5da7-137">Telepítse a Raspbian toohello microSD-kártyán.</span><span class="sxs-lookup"><span data-stu-id="a5da7-137">Install Raspbian toohello microSD card.</span></span>
   1. <span data-ttu-id="a5da7-138">[Töltse le](https://www.etcher.io) és hello Etcher SD-kártya író segédprogram telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="a5da7-138">[Download](https://www.etcher.io) and install hello Etcher SD card burner utility.</span></span>
   2. <span data-ttu-id="a5da7-139">Futtassa a Etcher és hello Raspbian kép kibontott válassza az 1. lépésben.</span><span class="sxs-lookup"><span data-stu-id="a5da7-139">Run Etcher and select hello Raspbian image that you extracted in step 1.</span></span>
   3. <span data-ttu-id="a5da7-140">Válassza ki a hello microSD-kártyát meghajtó.</span><span class="sxs-lookup"><span data-stu-id="a5da7-140">Select hello microSD card drive.</span></span>
      <span data-ttu-id="a5da7-141">Vegye figyelembe, hogy Etcher előfordulhat, hogy már választott hello megfelelő meghajtót.</span><span class="sxs-lookup"><span data-stu-id="a5da7-141">Note that Etcher may have already selected hello correct drive.</span></span>
   4. <span data-ttu-id="a5da7-142">Kattintson a **Flash** tooinstall Raspbian toohello microSD-kártyán.</span><span class="sxs-lookup"><span data-stu-id="a5da7-142">Click **Flash** tooinstall Raspbian toohello microSD card.</span></span>
   5. <span data-ttu-id="a5da7-143">Ha a telepítés hello microSD-kártyán eltávolítása a számítógépről.</span><span class="sxs-lookup"><span data-stu-id="a5da7-143">Remove hello microSD card from your computer when installation is complete.</span></span>
      <span data-ttu-id="a5da7-144">Ennek az oka biztonságos tooremove hello microSD-kártyán közvetlenül Etcher automatikusan kiadása vagy leválasztja hello microSD-kártyán befejezését követően.</span><span class="sxs-lookup"><span data-stu-id="a5da7-144">It's safe tooremove hello microSD card directly because Etcher automatically ejects or unmounts hello microSD card upon completion.</span></span>
   6. <span data-ttu-id="a5da7-145">A Pi hello microSD-kártyán beilleszteni.</span><span class="sxs-lookup"><span data-stu-id="a5da7-145">Insert hello microSD card into Pi.</span></span>

![Hello SD-kártya behelyezése](media/iot-hub-raspberry-pi-lessons/lesson1/insert_sdcard.jpg)

## <a name="turn-on-pi"></a><span data-ttu-id="a5da7-147">A Pi bekapcsolása</span><span class="sxs-lookup"><span data-stu-id="a5da7-147">Turn on Pi</span></span>
<span data-ttu-id="a5da7-148">Kapcsolja be a Pi hello micro USB-kábelen és hello tápegység.</span><span class="sxs-lookup"><span data-stu-id="a5da7-148">Turn on Pi by using hello micro USB cable and hello power supply.</span></span>

![Bekapcsolás](media/iot-hub-raspberry-pi-lessons/lesson1/micro_usb_power_on.jpg)

> [!NOTE]
> <span data-ttu-id="a5da7-150">Fontos fontos toouse hello tápegység hello Kit legalább 2/a. toomake, hogy a málna megfelelően van-e elegendő power toowork.</span><span class="sxs-lookup"><span data-stu-id="a5da7-150">It is important toouse hello power supply in hello kit that is at least 2A toomake sure that your Raspberry has enough power toowork correctly.</span></span>

## <a name="enable-ssh"></a><span data-ttu-id="a5da7-151">SSH engedélyezése</span><span class="sxs-lookup"><span data-stu-id="a5da7-151">Enable SSH</span></span>
<span data-ttu-id="a5da7-152">Frissítésétől hello 2016. novemberi kiadásban Raspbian hello SSH-kiszolgálót alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="a5da7-152">As of hello November 2016 release, Raspbian has hello SSH server disabled by default.</span></span> <span data-ttu-id="a5da7-153">Tooenable kell azt manuálisan.</span><span class="sxs-lookup"><span data-stu-id="a5da7-153">You need tooenable it manually.</span></span> <span data-ttu-id="a5da7-154">Olvassa el a toohello [hivatalos utasításokat](https://www.raspberrypi.org/documentation/remote-access/ssh/) vagy csatlakoztassa egy figyelő, és lépjen túl**beállítások -> málna Pi konfigurációs** tooenable SSH.</span><span class="sxs-lookup"><span data-stu-id="a5da7-154">You can refer toohello [official instructions](https://www.raspberrypi.org/documentation/remote-access/ssh/) or connect a monitor and go too**Preferences -> Raspberry Pi Configuration** tooenable SSH.</span></span>

## <a name="connect-raspberry-pi-3-toohello-network"></a><span data-ttu-id="a5da7-155">Málna Pi 3 toohello hálózat</span><span class="sxs-lookup"><span data-stu-id="a5da7-155">Connect Raspberry Pi 3 toohello network</span></span>
<span data-ttu-id="a5da7-156">Vezetékes vagy vezeték nélküli hálózaton tooa Pi tooa is elérheti.</span><span class="sxs-lookup"><span data-stu-id="a5da7-156">You can connect Pi tooa wired network or tooa wireless network.</span></span> <span data-ttu-id="a5da7-157">Győződjön meg arról, hogy Pi ugyanaz, mint a számítógép hálózati csatlakoztatott toohello.</span><span class="sxs-lookup"><span data-stu-id="a5da7-157">Make sure that Pi is connected toohello same network as your computer.</span></span> <span data-ttu-id="a5da7-158">Például csatlakoztathatja a Pi toohello azonos váltani, hogy a számítógép csatlakoztatva van.</span><span class="sxs-lookup"><span data-stu-id="a5da7-158">For example, you can connect Pi toohello same switch that your computer is connected to.</span></span>

### <a name="connect-tooa-wired-network"></a><span data-ttu-id="a5da7-159">Csatlakozás tooa vezetékes hálózathoz</span><span class="sxs-lookup"><span data-stu-id="a5da7-159">Connect tooa wired network</span></span>
<span data-ttu-id="a5da7-160">Hello Ethernet kábel tooconnect vezetékes hálózathoz Pi tooyour használja.</span><span class="sxs-lookup"><span data-stu-id="a5da7-160">Use hello Ethernet cable tooconnect Pi tooyour wired network.</span></span> <span data-ttu-id="a5da7-161">hello a Pi két LED bekapcsolása, ha hello kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="a5da7-161">hello two LEDs on Pi turn on if hello connection is established.</span></span>

![Csatlakozás az Ethernet-kábel segítségével](media/iot-hub-raspberry-pi-lessons/lesson1/connect_ethernet.jpg)

### <a name="connect-tooa-wireless-network"></a><span data-ttu-id="a5da7-163">Tooa vezeték nélküli hálózat</span><span class="sxs-lookup"><span data-stu-id="a5da7-163">Connect tooa wireless network</span></span>
<span data-ttu-id="a5da7-164">Hajtsa végre a hello [utasításokat](https://www.raspberrypi.org/learning/software-guide/wifi/) hello málna Pi Foundation tooconnect Pi tooyour vezeték nélküli hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="a5da7-164">Follow hello [instructions](https://www.raspberrypi.org/learning/software-guide/wifi/) from hello Raspberry Pi Foundation tooconnect Pi tooyour wireless network.</span></span> <span data-ttu-id="a5da7-165">Ezek az utasítások használatba toofirst csatlakozni a figyelő és a billentyűzet tooPi.</span><span class="sxs-lookup"><span data-stu-id="a5da7-165">These instructions require you toofirst connect a monitor and a keyboard tooPi.</span></span>

## <a name="connect-hello-led-toopi"></a><span data-ttu-id="a5da7-166">Csatlakozás hello LED tooPi</span><span class="sxs-lookup"><span data-stu-id="a5da7-166">Connect hello LED tooPi</span></span>
<span data-ttu-id="a5da7-167">toocomplete ezt a feladatot, használjon hello [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), hello összekötő fenyegetéseknek, hello LED és ellenállás hello.</span><span class="sxs-lookup"><span data-stu-id="a5da7-167">toocomplete this task, use hello [breadboard](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), hello connector wires, hello LED, and hello resistor.</span></span> <span data-ttu-id="a5da7-168">Csatlakoztassa őket toohello [általános célú bemeneti/kimeneti](https://www.raspberrypi.org/documentation/usage/gpio/) pi (GPIO) portok.</span><span class="sxs-lookup"><span data-stu-id="a5da7-168">Connect them toohello [general-purpose input/output](https://www.raspberrypi.org/documentation/usage/gpio/) (GPIO) ports of Pi.</span></span>

![Breadboard LED és ellenállás](media/iot-hub-raspberry-pi-lessons/lesson1/breadboard_led_resistor.jpg)

1. <span data-ttu-id="a5da7-170">Hello LED rövidebb szakasza hello túl csatlakozás**GPIO GND (PIN-kód 6)**.</span><span class="sxs-lookup"><span data-stu-id="a5da7-170">Connect hello shorter leg of hello LED too**GPIO GND (Pin 6)**.</span></span>
2. <span data-ttu-id="a5da7-171">Csatlakozás hello hello LED tooone szakasza hello ellenállás hosszabb szakasza.</span><span class="sxs-lookup"><span data-stu-id="a5da7-171">Connect hello longer leg of hello LED tooone leg of hello resistor.</span></span>
3. <span data-ttu-id="a5da7-172">Csatlakozás más hello ellenállás szakasza túl hello**GPIO 4 (PIN-kód 7)**.</span><span class="sxs-lookup"><span data-stu-id="a5da7-172">Connect hello other leg of hello resistor too**GPIO 4 (Pin 7)**.</span></span>

<span data-ttu-id="a5da7-173">Vegye figyelembe, hogy hello LED polaritás fontos.</span><span class="sxs-lookup"><span data-stu-id="a5da7-173">Note that hello LED polarity is important.</span></span> <span data-ttu-id="a5da7-174">Ez a beállítás polaritás aktív alacsony gyakran nevezik.</span><span class="sxs-lookup"><span data-stu-id="a5da7-174">This polarity setting is commonly known as Active Low.</span></span>

![Átalakítókábelre](media/iot-hub-raspberry-pi-lessons/lesson1/pinout_breadboard.png)

<span data-ttu-id="a5da7-176">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="a5da7-176">Congratulations!</span></span> <span data-ttu-id="a5da7-177">Sikeresen konfigurálta az Pi.</span><span class="sxs-lookup"><span data-stu-id="a5da7-177">You've successfully configured Pi.</span></span>

## <a name="summary"></a><span data-ttu-id="a5da7-178">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="a5da7-178">Summary</span></span>
<span data-ttu-id="a5da7-179">A cikkben, hogy megismerte hogyan tooconfigure helyszerepköreinek Raspbian, kapcsolódó Pi tooa hálózati kapcsolódás egy LED tooPi Pi tartományban.</span><span class="sxs-lookup"><span data-stu-id="a5da7-179">In this article, you’ve learned how tooconfigure Pi by installing Raspbian, connecting Pi tooa network, and connecting an LED tooPi.</span></span> <span data-ttu-id="a5da7-180">Vegye figyelembe, hogy hello LED nem még bonyolít le.</span><span class="sxs-lookup"><span data-stu-id="a5da7-180">Note that hello LED doesn't yet light up.</span></span> <span data-ttu-id="a5da7-181">hello következő feladata tooinstall hello szükséges eszközök és szoftverek mintaalkalmazás futó Pi előkészítésekor.</span><span class="sxs-lookup"><span data-stu-id="a5da7-181">hello next task is tooinstall hello necessary tools and software in preparation for running a sample application on Pi.</span></span>

![Készen áll a hardver](media/iot-hub-raspberry-pi-lessons/lesson1/hardware_ready.jpg)

## <a name="next-steps"></a><span data-ttu-id="a5da7-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a5da7-183">Next steps</span></span>
[<span data-ttu-id="a5da7-184">Hello eszközök beszerzése</span><span class="sxs-lookup"><span data-stu-id="a5da7-184">Get hello tools</span></span>](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

