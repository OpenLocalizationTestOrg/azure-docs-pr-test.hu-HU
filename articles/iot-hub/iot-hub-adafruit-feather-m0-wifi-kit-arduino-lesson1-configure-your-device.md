---
title: "Connect Arduino (C) tooAzure IoT - lecke 1: eszközök konfigurálása |} Microsoft Docs"
description: "Adafruit lágyított M0 Wi-Fi konfigurálása az első használatnál."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "beállítása, arduino csatlakozás arduino toopc, a telepítő arduino, arduino board"
ROBOTS: NOINDEX
redirect_url: /azure/iot-hub/iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started
ms.assetid: f5b334f0-a148-41aa-b374-ce7b9f5b305a
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
ms.openlocfilehash: 30b764e8ff6221995456283a226e79f064b2d74e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-your-device"></a><span data-ttu-id="4c427-104">Az eszköz konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4c427-104">Configure your device</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="4c427-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="4c427-105">What you will do</span></span>
<span data-ttu-id="4c427-106">Konfigurálja a Adafruit lágyított M0 Wi-Fi Arduino üzenőfalon, az első használatnál összeállításakor hello üzenőfalon, működtetéséhez azt be.</span><span class="sxs-lookup"><span data-stu-id="4c427-106">Configure your Adafruit Feather M0 WiFi Arduino board for first-time use by assembling hello board, powering it up.</span></span> <span data-ttu-id="4c427-107">Ha bármilyen problémába ütközik, keressen megoldásokat a hello [oldal hibaelhárítási](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="4c427-107">If you have any problems, look for solutions on hello [troubleshooting page](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).</span></span>

## <a name="what-you-need"></a><span data-ttu-id="4c427-108">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="4c427-108">What you need</span></span>
<span data-ttu-id="4c427-109">toocomplete Ez a művelet következő részek a Adafruit lágyított M0 Wi-Fi Starter Kit hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="4c427-109">toocomplete this operation, you need hello following parts for your Adafruit Feather M0 WiFi Starter Kit:</span></span>

* <span data-ttu-id="4c427-110">hello Adafruit lágyított M0 Wi-Fi tábla</span><span class="sxs-lookup"><span data-stu-id="4c427-110">hello Adafruit Feather M0 WiFi board</span></span>
* <span data-ttu-id="4c427-111">Egy Micro B tooType egy USB-kábellel</span><span class="sxs-lookup"><span data-stu-id="4c427-111">A Micro B tooType A USB cable</span></span>

![csomag][kit]

<span data-ttu-id="4c427-113">Emellett a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="4c427-113">You also need:</span></span>

* <span data-ttu-id="4c427-114">Windows, Mac vagy Linux rendszerű számítógép.</span><span class="sxs-lookup"><span data-stu-id="4c427-114">A computer running Windows, Mac, or Linux.</span></span>
* <span data-ttu-id="4c427-115">A Arduino Bizottság tooconnect a vezeték nélküli kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="4c427-115">A wireless connection for your Arduino board tooconnect to.</span></span>
* <span data-ttu-id="4c427-116">Az internetes kapcsolat toodownload konfigurációs eszközt.</span><span class="sxs-lookup"><span data-stu-id="4c427-116">An Internet connection toodownload configuration tool.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="4c427-117">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="4c427-117">What you will learn</span></span>
<span data-ttu-id="4c427-118">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="4c427-118">In this article, you will learn:</span></span>

* <span data-ttu-id="4c427-119">Hogyan tooassemble a Arduino tábla és a tápkábelek azt az alábbi hello szolgáltatáshoz tanítás.</span><span class="sxs-lookup"><span data-stu-id="4c427-119">How tooassemble your Arduino board and power it up for hello following lessons.</span></span>
* <span data-ttu-id="4c427-120">Hogyan Ubuntu tooadd soros port engedélyeit.</span><span class="sxs-lookup"><span data-stu-id="4c427-120">How tooadd serial port permissions on Ubuntu.</span></span>

## <a name="connect-your-arduino-board-tooyour-computer"></a><span data-ttu-id="4c427-121">A Arduino board tooyour számítógép</span><span class="sxs-lookup"><span data-stu-id="4c427-121">Connect your Arduino board tooyour computer</span></span>

1. <span data-ttu-id="4c427-122">Hello micro USB-kábellel csatlakoztassa hello felső micro USB-porttal.</span><span class="sxs-lookup"><span data-stu-id="4c427-122">Plug hello micro USB cable into hello top micro USB port.</span></span>

   ![Felső micro USB-port][top-micro-usb-port]

2. <span data-ttu-id="4c427-124">Plug hello USB-kábel másik végén a számítógépbe.</span><span class="sxs-lookup"><span data-stu-id="4c427-124">Plug hello other end of USB cable into your computer.</span></span>

   ![Számítógép USB][computer-usb]

## <a name="add-serial-port-permissions-on-ubuntu"></a><span data-ttu-id="4c427-126">Ubuntu a soros port engedélyek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4c427-126">Add serial port permissions on Ubuntu</span></span>

<span data-ttu-id="4c427-127">Ez a szakasz kihagyhatja, ha a Windows vagy macOS.</span><span class="sxs-lookup"><span data-stu-id="4c427-127">You can skip this section if you use Windows or macOS.</span></span> <span data-ttu-id="4c427-128">Ubuntu, a következő lépések toomake meg arról, hogy hello normál linux felhasználó a Arduino kártya hello USB-porttal rendelkezik a hello engedélyek toooperate hello kell.</span><span class="sxs-lookup"><span data-stu-id="4c427-128">For Ubuntu, you need hello following steps toomake sure hello normal linux user has hello permissions toooperate on hello USB port of your Arduino board.</span></span>

1. <span data-ttu-id="4c427-129">Most már a normál felhasználói terminálról:</span><span class="sxs-lookup"><span data-stu-id="4c427-129">Now as normal user from terminal:</span></span>

   ```bash
   ls -l /dev/ttyUSB*
   # Or
   ls -l /dev/ttyACM*
   ```

   <span data-ttu-id="4c427-130">Hasonlót fog kapni:</span><span class="sxs-lookup"><span data-stu-id="4c427-130">You will get something like:</span></span>

   ```bash
   crw-rw---- 1 root uucp 188, 0 5 apr 23.01 ttyUSB0
   # Or
   crw-rw---- 1 root dialout 188, 0 5 apr 23.01 ttyACM0
   ```

   <span data-ttu-id="4c427-131">hello "0" lehet egy másik számot, vagy esetleg visszaadott több bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="4c427-131">hello "0" might be a different number, or multiple entries might be returned.</span></span> <span data-ttu-id="4c427-132">Hello azt az első eset hello adatok, igazolnia kell a `uucp`, hello második van `dialout`, amely hello fájl hello csoport tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="4c427-132">In hello first case hello data we need is `uucp`, in hello second is `dialout`, which is hello group owner of hello file.</span></span>

2. <span data-ttu-id="4c427-133">Felhasználói toohello toohello csoport hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="4c427-133">Add user toohello toohello group:</span></span>

   ```bash
   sudo usermod -a -G group-name username
   ```

   <span data-ttu-id="4c427-134">Ha `group-name` hello első lépésben található hello adat és `username` a linux-felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="4c427-134">Where `group-name` is hello data found in hello first step, and `username` is your linux user name.</span></span>

3. <span data-ttu-id="4c427-135">Toolog és újra kell használni a tootake hatás módosítása és a teljes hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="4c427-135">You will need toolog out and in again for this change tootake effect and complete hello setup.</span></span>

## <a name="summary"></a><span data-ttu-id="4c427-136">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="4c427-136">Summary</span></span>
<span data-ttu-id="4c427-137">A cikkben, hogy megismerte hogyan tooconfigure a Arduino tábla.</span><span class="sxs-lookup"><span data-stu-id="4c427-137">In this article, you’ve learned how tooconfigure your Arduino board.</span></span> <span data-ttu-id="4c427-138">hello következő feladata tooinstall hello szükséges eszközök és szoftverek előkészítésekor a Arduino indítópanel mintaalkalmazás futtatása.</span><span class="sxs-lookup"><span data-stu-id="4c427-138">hello next task is tooinstall hello necessary tools and software in preparation for running a sample application on your Arduino board.</span></span>

![Készen áll a hardver][hardware-is-ready]

## <a name="next-steps"></a><span data-ttu-id="4c427-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4c427-140">Next steps</span></span>
<span data-ttu-id="4c427-141">[Hello eszközök beszerzése][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="4c427-141">[Get hello tools][get-the-tools]</span></span>
<!-- Images and links -->

[kit]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/kit.png
[top-micro-usb-port]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/top_usbport.jpg
[computer-usb]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/computer_usb.jpg
[hardware-is-ready]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/hardware_ready.jpg
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md