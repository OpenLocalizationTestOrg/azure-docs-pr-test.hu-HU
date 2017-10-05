---
title: "Az Azure IoT - lecke 1 Connect Arduino (C): eszközök konfigurálása |} Microsoft Docs"
description: "Adafruit lágyított M0 Wi-Fi konfigurálása az első használatnál."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Állítsa be, arduino csatlakozni arduino pc, a telepítő arduino, arduino board"
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
ms.openlocfilehash: 9e319292e5d30dea7e45857e435825861aad1c84
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-your-device"></a><span data-ttu-id="a9bdc-104">Az eszköz konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a9bdc-104">Configure your device</span></span>
## <a name="what-you-will-do"></a><span data-ttu-id="a9bdc-105">Mit fog</span><span class="sxs-lookup"><span data-stu-id="a9bdc-105">What you will do</span></span>
<span data-ttu-id="a9bdc-106">Konfigurálja a Adafruit lágyított M0 Wi-Fi Arduino üzenőfalon, az első használatnál össze a üzenőfalon, működtetéséhez azt be.</span><span class="sxs-lookup"><span data-stu-id="a9bdc-106">Configure your Adafruit Feather M0 WiFi Arduino board for first-time use by assembling the board, powering it up.</span></span> <span data-ttu-id="a9bdc-107">Ha bármilyen problémába ütközik, tekintse meg a megoldások a [oldal hibaelhárítási](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="a9bdc-107">If you have any problems, look for solutions on the [troubleshooting page](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).</span></span>

## <a name="what-you-need"></a><span data-ttu-id="a9bdc-108">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="a9bdc-108">What you need</span></span>
<span data-ttu-id="a9bdc-109">A művelet végrehajtásához a következő részek a Adafruit lágyított M0 Wi-Fi Starter Kit szüksége:</span><span class="sxs-lookup"><span data-stu-id="a9bdc-109">To complete this operation, you need the following parts for your Adafruit Feather M0 WiFi Starter Kit:</span></span>

* <span data-ttu-id="a9bdc-110">A Wi-Fi M0 Adafruit lágyított tábla</span><span class="sxs-lookup"><span data-stu-id="a9bdc-110">The Adafruit Feather M0 WiFi board</span></span>
* <span data-ttu-id="a9bdc-111">A típus egy USB-kábel Micro B</span><span class="sxs-lookup"><span data-stu-id="a9bdc-111">A Micro B to Type A USB cable</span></span>

![csomag][kit]

<span data-ttu-id="a9bdc-113">Emellett a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="a9bdc-113">You also need:</span></span>

* <span data-ttu-id="a9bdc-114">Windows, Mac vagy Linux rendszerű számítógép.</span><span class="sxs-lookup"><span data-stu-id="a9bdc-114">A computer running Windows, Mac, or Linux.</span></span>
* <span data-ttu-id="a9bdc-115">Vezeték nélküli kapcsolat a Arduino kártya való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="a9bdc-115">A wireless connection for your Arduino board to connect to.</span></span>
* <span data-ttu-id="a9bdc-116">Az internethez konfigurációs eszköz letöltése.</span><span class="sxs-lookup"><span data-stu-id="a9bdc-116">An Internet connection to download configuration tool.</span></span>

## <a name="what-you-will-learn"></a><span data-ttu-id="a9bdc-117">Amiről tanulni fog</span><span class="sxs-lookup"><span data-stu-id="a9bdc-117">What you will learn</span></span>
<span data-ttu-id="a9bdc-118">Ebből a cikkből megtudhatja:</span><span class="sxs-lookup"><span data-stu-id="a9bdc-118">In this article, you will learn:</span></span>

* <span data-ttu-id="a9bdc-119">Megtudhatja, hogyan állíthatja össze a Arduino üzenőfalon, és kapcsolja be az szolgáltatáshoz a következő rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="a9bdc-119">How to assemble your Arduino board and power it up for the following lessons.</span></span>
* <span data-ttu-id="a9bdc-120">Hogyan Ubuntu soros port engedélyek hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="a9bdc-120">How to add serial port permissions on Ubuntu.</span></span>

## <a name="connect-your-arduino-board-to-your-computer"></a><span data-ttu-id="a9bdc-121">A Arduino board kapcsolódni a számítógéphez</span><span class="sxs-lookup"><span data-stu-id="a9bdc-121">Connect your Arduino board to your computer</span></span>

1. <span data-ttu-id="a9bdc-122">A micro USB-kábellel csatlakoztassa a felső micro USB-porttal.</span><span class="sxs-lookup"><span data-stu-id="a9bdc-122">Plug the micro USB cable into the top micro USB port.</span></span>

   ![Felső micro USB-port][top-micro-usb-port]

2. <span data-ttu-id="a9bdc-124">Másik végén USB-kábellel csatlakoztassa a számítógépet.</span><span class="sxs-lookup"><span data-stu-id="a9bdc-124">Plug the other end of USB cable into your computer.</span></span>

   ![Számítógép USB][computer-usb]

## <a name="add-serial-port-permissions-on-ubuntu"></a><span data-ttu-id="a9bdc-126">Ubuntu a soros port engedélyek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a9bdc-126">Add serial port permissions on Ubuntu</span></span>

<span data-ttu-id="a9bdc-127">Ez a szakasz kihagyhatja, ha a Windows vagy macOS.</span><span class="sxs-lookup"><span data-stu-id="a9bdc-127">You can skip this section if you use Windows or macOS.</span></span> <span data-ttu-id="a9bdc-128">Ubuntu van szüksége az alábbi lépések végrehajtásával győződjön meg arról a normál linux felhasználó jogosult az USB-port a Arduino kártya működéséhez.</span><span class="sxs-lookup"><span data-stu-id="a9bdc-128">For Ubuntu, you need the following steps to make sure the normal linux user has the permissions to operate on the USB port of your Arduino board.</span></span>

1. <span data-ttu-id="a9bdc-129">Most már a normál felhasználói terminálról:</span><span class="sxs-lookup"><span data-stu-id="a9bdc-129">Now as normal user from terminal:</span></span>

   ```bash
   ls -l /dev/ttyUSB*
   # Or
   ls -l /dev/ttyACM*
   ```

   <span data-ttu-id="a9bdc-130">Hasonlót fog kapni:</span><span class="sxs-lookup"><span data-stu-id="a9bdc-130">You will get something like:</span></span>

   ```bash
   crw-rw---- 1 root uucp 188, 0 5 apr 23.01 ttyUSB0
   # Or
   crw-rw---- 1 root dialout 188, 0 5 apr 23.01 ttyACM0
   ```

   <span data-ttu-id="a9bdc-131">A "0" lehet, hogy egy másik számot, vagy esetleg visszaadott több bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="a9bdc-131">The "0" might be a different number, or multiple entries might be returned.</span></span> <span data-ttu-id="a9bdc-132">Az első esetben az adatokat, igazolnia kell van `uucp`, a második van `dialout`, vagyis a fájl a csoport tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="a9bdc-132">In the first case the data we need is `uucp`, in the second is `dialout`, which is the group owner of the file.</span></span>

2. <span data-ttu-id="a9bdc-133">Adja hozzá felhasználót a csoporthoz:</span><span class="sxs-lookup"><span data-stu-id="a9bdc-133">Add user to the to the group:</span></span>

   ```bash
   sudo usermod -a -G group-name username
   ```

   <span data-ttu-id="a9bdc-134">Ha `group-name` az első lépésben található adatok és `username` a linux-felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="a9bdc-134">Where `group-name` is the data found in the first step, and `username` is your linux user name.</span></span>

3. <span data-ttu-id="a9bdc-135">Szüksége lesz, és a módosítás érvénybe lépéséhez, majd fejezze be a telepítő újbóli bejelentkezésre.</span><span class="sxs-lookup"><span data-stu-id="a9bdc-135">You will need to log out and in again for this change to take effect and complete the setup.</span></span>

## <a name="summary"></a><span data-ttu-id="a9bdc-136">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="a9bdc-136">Summary</span></span>
<span data-ttu-id="a9bdc-137">Ebben a cikkben a Arduino board konfigurálása hogy megismerte.</span><span class="sxs-lookup"><span data-stu-id="a9bdc-137">In this article, you’ve learned how to configure your Arduino board.</span></span> <span data-ttu-id="a9bdc-138">A következő feladathoz a rendszer telepíti a szükséges eszközök és szoftverek előkészítésekor a Arduino indítópanel mintaalkalmazás futtatása.</span><span class="sxs-lookup"><span data-stu-id="a9bdc-138">The next task is to install the necessary tools and software in preparation for running a sample application on your Arduino board.</span></span>

![Készen áll a hardver][hardware-is-ready]

## <a name="next-steps"></a><span data-ttu-id="a9bdc-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a9bdc-140">Next steps</span></span>
<span data-ttu-id="a9bdc-141">[Eszközök][get-the-tools]</span><span class="sxs-lookup"><span data-stu-id="a9bdc-141">[Get the tools][get-the-tools]</span></span>
<!-- Images and links -->

[kit]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/kit.png
[top-micro-usb-port]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/top_usbport.jpg
[computer-usb]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/computer_usb.jpg
[hardware-is-ready]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/hardware_ready.jpg
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md